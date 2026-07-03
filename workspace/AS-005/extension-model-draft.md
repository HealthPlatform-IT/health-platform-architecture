# Extension Model Draft

> **Promovido para documentação oficial:** `docs/05-architecture/extension-model.md` (ADR-0009).  
> Este arquivo permanece como registro histórico da investigação NR-005.

**Investigação:** NR-005 — modelo de extensão conceitual  
**Pré-requisitos:** `core-boundary.md` v0.2.1 · `module-strategy-draft.md` v0.2.0 (validado)  
**Status:** Rascunho em revisão  
**Última atualização:** 2026-07-03

---

## 1. Purpose

Definir como **extensões** operam na Health Platform sem:

- contaminar o Core Platform;
- confundir extensão com customização por cliente (ADR-0006);
- misturar Extension Domain, Extension Module e Extension Mechanism (H-EXT-001).

Este documento **não** define implementação técnica de hooks, APIs ou deploy (Q-009).

---

## 2. Three Distinct Concepts (H-EXT-001)

| Termo | O que é | Onde vive | Exemplos |
|---|---|---|---|
| **Extension Business Domain** | Domínio de negócio para modelo operacional específico | Camada de domínio (ADR-0008) | Diagnostic Operations, Home Care Operations |
| **Extension Module** | Módulo opcional que implementa um Extension Domain | Camada de módulo | M-14 Diagnostic, M-15 Home Care |
| **Extension Mechanism** | Infraestrutura do Core para adicionar capacidade sem alterar fundação | Core Platform (D-001) | Module Registry, extension hooks, composição |

**Regra terminológica:** usar qualificador quando ambíguo — nunca "Extension" sozinho.

```text
Extension Mechanism (Core)  →  habilita registro e composição
Extension Domain (negócio)  →  define regras do modelo operacional
Extension Module (produto)  →  implementa o domínio para o usuário
```

---

## 3. Variability Hierarchy (ADR-0006)

```text
1. Configuração              ← preferir sempre
2. Composição de módulos     ← habilitar/desabilitar módulos Core product
3. Extensão                  ← Extension Modules + Extension Domains
4. Customização por código   ← exceção governada (Q-016)
5. Alteração do Core         ← proibida sem ADR
```

**Extensão na AS-005** = níveis **2 e 3** — reutilizável para todos os tenants que habilitam o modelo operacional, **não** código exclusivo por cliente.

| Nível | Exemplo | É extensão? |
|---|---|---|
| 1 — Configuração | Horário de atendimento, templates | Não |
| 2 — Composição | Desabilitar Payer Module | Parcial — composição de Core modules |
| 3 — Extensão | Habilitar Diagnostic Module | Sim |
| 4 — Customização | Workflow exclusivo para Hospital X | Não — exceção governada |
| 5 — Core change | Alterar Care Journey types para um cliente | Proibido |

---

## 4. Extension Mechanism (Core Platform)

Componentes do Core que suportam extensão (D-001):

| Componente Core | Papel na extensão |
|---|---|
| **Module Registry** | Registro, descoberta, metadados, dependências de Extension Modules |
| **Extension Mechanism** | Contratos de hooks e slots de composição |
| **Runtime Context** | Propaga tenant, organization, modelo operacional habilitado |
| **Configuration contract** | Interface para PS ler habilitação de módulos |
| **Event Foundation** | Extension Modules publicam/consomem eventos sem acoplamento |

**O Core não contém regras de Diagnostic ou Home Care** — apenas mecanismos que permitem módulos Extension se registrar e compor.

**UI composition (OQ-C03 parcial):** slot/hook mínimo no Core para Clinical Workspace (M-02) carregar Extension Modules quando habilitados.

---

## 5. Extension Business Domains (confirmed)

Domínios Extension confirmados na AS-004 (ADR-0008):

| Domínio | Modelo operacional | Capability | Extension Module |
|---|---|---|---|
| **Diagnostic Operations** | Diagnostic Care | Executar | M-14 Diagnostic Module |
| **Home Care Operations** | Home Care | Executar | M-15 Home Care Module |

### Deferred (not active Extension)

| Domínio | Motivo |
|---|---|
| Hospital Operations | Deferred — fora do escopo Sprint 1 |
| Billing & Financial | Q-005 Open |

**Regra:** novo Extension Domain exige sessão arquitetural + ADR — inventário fechado na AS-004.

---

## 6. Extension Modules Catalog

| ID | Módulo | Extension Domain | Habilitação | Composição |
|---|---|---|---|---|
| M-14 | Diagnostic Module | Diagnostic Operations | Configuration + Feature Flag + Registry | Clinical Workspace (M-02) ou fluxo standalone |
| M-15 | Home Care Module | Home Care Operations | Configuration + Feature Flag + Registry | Clinical Workspace (M-02) ou fluxo standalone |

**Características comuns:**

- Opcionais por tenant/instituição
- Desabilitáveis sem quebrar Core Platform nem Core product modules
- Implementam vocabulário do Extension Domain — não criam domínio novo
- Consomem PS por contrato (mesmas regras A-01 a A-06 de `module-strategy-draft.md`)

---

## 7. Extension Module vs Core Product Module

| Critério | Core product module | Extension module |
|---|---|---|
| Necessário para operação ambulatorial básica? | Sim | Não |
| Pode ser desabilitado sem quebrar fundação? | Não *(para operação mínima)* | Sim |
| Ligado a modelo operacional específico? | Não | Sim |
| Declarado em ADR-0008 como Extension Domain? | Não | Sim |
| Exemplos | M-01 Scheduling, M-03 Attendance | M-14 Diagnostic, M-15 Home Care |

**Supporting modules** (M-09 a M-12, M-16) são Core product da plataforma — habilitáveis por composição (nível 2), mas **não** Extension Modules.

---

## 8. Operational Modes vs Extension Modules

Nem todo modelo operacional gera Extension Module.

| Conceito | Definição | Exemplo |
|---|---|---|
| **Operational Mode** | Variante de execução dentro de um Core product module | Telemedicine como modo de M-03 Attendance |
| **Extension Module** | Módulo separado para Extension Domain | M-14 Diagnostic |

### Telemedicine — resolved (D-004 confirmed)

| Alternativa | Resultado |
|---|---|
| Extension Module independente (M-13) | **Rejeitada** — duplica Care Delivery |
| Operational Mode em Attendance | **Adotada** — habilitado via Configuration + Feature Flag |
| Extension Module futuro | **Deferred** — apenas se surgir Extension Domain Telemedicine |

**Distinção:** Telemedicine altera **como** o atendimento é executado (Care Delivery), não adiciona domínio operacional novo.

---

## 9. How Extensions Are Enabled (conceptual)

```text
Instituição define modelo operacional
        ↓
Configuration Service persiste habilitação
        ↓
Feature Flag Service (rollout gradual, opcional)
        ↓
Module Registry (Core) expõe módulos habilitados
        ↓
Clinical Workspace / shell compõe Extension Modules
```

| Mecanismo | Camada | Papel |
|---|---|---|
| **Configuration Service** | PS | Instituição habilita módulos e modelos operacionais |
| **Feature Flag Service** | PS | Rollout gradual, A/B, kill switch |
| **Module Registry** | Core | Descoberta, metadados, dependências |
| **Extension Mechanism** | Core | Hooks de composição e integração |
| **Authorization Service** | PS | Permissões por módulo habilitado |

Implementação técnica: **Q-009 — fora de escopo AS-005**.

---

## 10. Composition with Clinical Workspace

Extension Modules integram-se ao ecossistema via composição — não substituem Core modules.

```text
Clinical Workspace (M-02) — shell
├── Core product (sempre que habilitados)
│   ├── Attendance (M-03) — inclui modo Telemedicine
│   ├── Clinical Documentation (M-04)
│   ├── Orders (M-05)
│   ├── Documents (M-06)
│   └── Care Monitoring (M-07)
└── Extension (se habilitados)
    ├── Diagnostic (M-14)
    └── Home Care (M-15)
```

**Regras:**

1. Extension Modules **não** alteram Core product modules — apenas adicionam capacidade.
2. Shell descobre módulos via Module Registry — sem hardcode.
3. Extension desabilitada = slot vazio — Core path intacto.
4. Eventos cross-module via Event Foundation — não chamadas diretas.

---

## 11. Extension vs Customization (Q-016)

| Aspecto | Extensão | Customização |
|---|---|---|
| Reutilizável | Sim — todos os tenants que habilitam | Não — específico de um cliente |
| Governança | Module Registry + Configuration | Exceção aprovada, código isolado |
| Impacto no Core | Nenhum | Nenhum *(se bem feita)* |
| ADR necessário | Extension Domain já em ADR-0008 | Caso a caso |
| Hierarquia ADR-0006 | Nível 3 | Nível 4 |

**Regra:** se a necessidade pode ser atendida por Configuration ou composição de módulos existentes, **não** criar Extension nem customização.

---

## 12. Extension vs Configuration Boundary

| Pergunta | Extensão | Configuração |
|---|---|---|
| Adiciona novo vocabulário de domínio? | Sim (Extension Domain) | Não |
| Adiciona módulo registrável? | Sim | Não |
| Altera comportamento de módulo existente? | Adiciona módulo paralelo | Ajusta parâmetros |
| Exemplo | Habilitar Diagnostic Module | Definir horário de agendamento |

**Zona cinzenta:** Feature Flag habilitando modo Telemedicine em Attendance = **configuração/composição** (nível 1–2), não Extension Module.

---

## 13. What Extension Is NOT

| Não é extensão | É |
|---|---|
| Código específico por tenant | Customização excepcional (ADR-0006 nível 4) |
| Alterar Core para um cliente | Violação ADR-0003 |
| Novo domínio sem sessão/ADR | Inventário fechado AS-004 |
| Novo Platform Service sem critério | Requer update ADR-0005 |
| Read Model (Timeline, Analytics) | Projeção — NR-006 |
| Platform Service | Transversal sem UI |
| Telemedicine como módulo | Operational Mode em Attendance |

---

## 14. Risks

| # | Risco | Mitigação |
|---|---|---|
| R-EXT01 | Extensão = customização | Hierarquia ADR-0006; Extension reutilizável |
| R-EXT02 | Regras operacionais no Core | Extension Domains + módulos opcionais |
| R-EXT03 | Colisão terminológica | Glossário H-EXT-001; qualificadores obrigatórios |
| R-EXT04 | Extension Module acoplado a Core | Event Foundation + Module Registry |
| R-EXT05 | Proliferação de Extension Domains | Inventário fechado; deferred explícito |
| R-EXT06 | Telemedicine como Extension órfã | Operational Mode — D-004 |
| R-EXT07 | Extension sem governança de habilitação | Configuration + Authorization + Audit |

---

## 15. Open Questions

| ID | Pergunta | Status |
|---|---|---|
| OQ-E01 | Hospital Operations como Extension futuro? | Deferred |
| OQ-E02 | Billing Module como Extension ou domínio deferred? | Q-005 Open |
| OQ-E03 | Mecanismo técnico de hooks | Q-009 — fase técnica |
| OQ-E04 | Extension Module standalone vs só via shell? | **Resolvido provisório:** ambos via Registry |
| OQ-C03 | UI composition contract no Core | Parcial — slot mínimo no Core |
| Q-009 | Implementação Extension Mechanism | Deferred — conceito confirmado em D-001 |

---

## 16. Initial Recommendations

1. **D-005** → promover para **Ready for Confirmation** em `decisions.md`.
2. **T-002** (Extension Domain vs Mechanism) → resolvida.
3. Telemedicine permanece **Operational Mode** — não reabrir como Extension.
4. **NR-006** — `read-models-boundary.md`: Extension Modules não consomem Timeline como domínio.
5. Formalização em ADR aguarda **NR-008** — possível update ADR-0003 seção extensão.

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1.0 | 2026-07-03 | Esqueleto NR-005 |
| 0.2.0 | 2026-07-03 | Investigação completa — H-EXT-001, catálogo, Telemedicine, composição |

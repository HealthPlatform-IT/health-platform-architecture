# Decision Package — AS-005

> **Status do pacote:** **Confirmado** em 2026-07-03.  
> **Confirmação:** D-001 a D-008 — *"Confirmo o pacote AS-005"*.  
> **Formalização:** Executada — ver `confirmation-package.md`.

---

## 1. Purpose

Consolidar o resultado da investigação da **AS-005 — Core Platform Boundary**, separando:

- decisões maduras prontas para confirmação;
- hipóteses que exigem revisão;
- itens adiados e riscos;
- impacto em open questions — **Q-007 Answered** (AS-005 confirmada).

Este pacote **não** define implementação (código, APIs, banco de dados).

### Distinções preservadas (reafirmação AS-004)

| Conceito | Representa |
|---|---|
| **Core Platform** | Fundação estrutural mínima — contratos e invariantes |
| **Platform Service** | Implementação transversal reutilizável (ADR-0005) |
| **Business Domain** | Responsabilidade de negócio (16 confirmados — ADR-0008) |
| **Module** | Solução implementável derivada de domínio(s) |
| **Extension** | Módulo/domínio opcional por modelo operacional |
| **Read Model / View** | Projeção cross-domain sem regras autônomas |

---

## 2. Decision Status Legend

| Status | Significado |
|---|---|
| **Ready for Confirmation** | Maduro; pode ser confirmado |
| **Confirmed** | Validado pelo usuário nesta sessão |
| **Needs Review** | Direção forte; exige validação |
| **Deferred** | Adiado conscientemente |
| **Rejected** | Alternativa descartada |
| **In Investigation** | Em análise nesta sessão |

---

## 3. Confirmed

> Validado pelo usuário em 2026-07-03 (*"Pacote revisado"* + *"Confirmo o pacote AS-005"*).

### D-001 — Definição de Core Platform

**Status:** Confirmed

**Decisão proposta:**

Core Platform é a camada de **fundação estrutural mínima** — contratos, invariantes e mecanismos de extensão — sobre a qual PS, domínios e módulos são construídos. **Não** implementa regras de negócio nem serviços transversais operacionais.

**Catálogo validado (8 itens):**

1. Hierarchical Care Model Contracts (ADR-0001)
2. Care Journey Type System (ADR-0007)
3. Tenant Context (contrato)
4. Module Registry
5. Extension Mechanism (contratos/hooks)
6. Event Foundation (contrato limitado — sem Event Model)
7. Runtime Context (agregador: tenant, organization, user, escopo)
8. Shared Kernel Types (referências mínimas estáveis)

**Rejeitado do Core:** implementações de config, permissão, auditoria, feature toggle, deploy/infra.

**Fonte:** `core-boundary.md` v0.2.1 — validado pelo usuário 2026-07-03

**Open Questions residuais:** OQ-C01 a OQ-C08 — não bloqueiam confirmação do catálogo principal.

---

### D-002 — Reconciliação Core ↔ Platform Services (T-001)

**Status:** Confirmed

**Decisão proposta:**

ADR-0003 "Core" (sentido amplo) e ADR-0005 Platform Services **não se contradizem**:

- **Core Platform** = contratos e invariantes estruturais.
- **Platform Services** = implementações transversais (Identity, Audit, Configuration, Communication, etc.).

**Regra operacional:**

```text
Core define invariantes → PS operam dentro dos invariantes → Domínios definem política → PS executam mecanismo
```

**Invariantes Core que PS devem honrar:** I-01 a I-10 em `platform-services-boundary.md` seção 3.

**Fonte:** `platform-services-boundary.md` v0.3.0

---

### D-003 — Catálogo Platform Services com tiers

**Status:** Confirmed

**Decisão proposta:**

| Tier | Quantidade | Serviços |
|---|---|---|
| Confirmed Candidate | 12 | Identity, Authorization, Audit, Configuration, Feature Flag, Communication, Notification, Integration, Webhook, Storage, File, Observability |
| Strong Candidate | 5 | Search, Document Engine, Medical Form Engine, Template Service, Event Bus |
| Needs Review | 1 | Compliance Service (Q-019) |

**Fonte:** `platform-services-boundary.md` v0.3.0

---

### D-004 — Relação domínio → módulo

**Status:** Confirmed

**Decisão proposta:**

- Cardinalidade **flexível** — não 1:1 obrigatório.
- Padrões: 1:1, 1:N, N:1, 1:0 (domínio sem módulo dedicado), Extension.
- **15 módulos candidatos** derivados de 16 domínios.
- Communication Domain **sem módulo dedicado** — orquestrado por módulos consumidores + PS.

**Fonte:** `module-strategy-draft.md` v0.2.0

---

### D-007 — Clinical Workspace (T-003)

**Status:** Confirmed

**Decisão proposta:**

Clinical Workspace (M-02) é **módulo shell transversal** que compõe M-03 a M-07 sem regras de negócio próprias. Alternativas B (Core UI) e C (sem shell) rejeitadas.

**Fonte:** `module-strategy-draft.md` seções 10–11; H-WS-001

---

### D-005 — Modelo de extensão

**Status:** Confirmed

**Decisão:**

Três conceitos distintos e não intercambiáveis (H-EXT-001):

| Termo | Papel |
|---|---|
| Extension Business Domain | Regras do modelo operacional (Diagnostic, Home Care) |
| Extension Module | M-14, M-15 — implementação opcional |
| Extension Mechanism | Module Registry + hooks no Core (D-001) |

**Telemedicine:** Operational Mode em Attendance — **não** Extension Module.

**Hierarquia ADR-0006:** extensão = níveis 2–3; customização = nível 4.

**Fonte:** `extension-model-draft.md` v0.2.0

---

### D-006 — Read Models boundary

**Status:** Confirmed

**Decisão:**

- **Clinical Timeline** e **Analytics & Reporting** permanecem Read Models — reafirmação AS-004 D-003/D-004.
- Ownership nos domínios fonte; projeção via Event Foundation (Q-003 deferred).
- Timeline consumida por M-02 e M-08 (parcial); Analytics por M-12.
- Timeline **não** é módulo nem domínio.

**Fonte:** `read-models-boundary.md` v0.2.0

---

### D-008 — Classification matrix unificada

**Status:** Confirmed

**Decisão:**

Árvore de decisão em 6 categorias (Core, PS, Domain, Read Model, Module, Extension) com invariantes I-01 a I-10, testes de validação e anti-patterns.

**Fonte:** `classification-matrix.md` v0.2.0

---

## 4. Formalização

| Artefato | Status |
|---|---|
| `confirmation-package.md` | Confirmado |
| **ADR-0009** | Accepted |
| `docs/05-architecture/core-platform.md` | Criado |
| `docs/05-architecture/module-strategy.md` | Criado |
| `docs/05-architecture/extension-model.md` | Criado |
| `docs/05-architecture/read-models.md` | Criado |
| `docs/05-architecture/architecture-classification.md` | Criado |
| `docs/05-architecture/platform-services.md` | Atualizado v0.2.0 |
| `docs/00-introduction/glossary.md` | Criado |
| `ai-context/development-guidelines.md` | Criado |
| `ai-context/architecture-foundation.md` | Atualizado |
| `ai-context/open-questions.md` | Q-007 Answered (ADR-0009) |
| `CLAUDE.md` + `.cursor/rules/health-platform.mdc` | Criados |
| Sessão AS-005 | Encerrada |

---

## 5. Needs Review

| Item | Motivo |
|---|---|
| Organization Context | OQ-C01 — subcontrato de Runtime Context vs domínio Organization Management |
| UI composition contract no Core | OQ-C03 parcial — slot/hook mínimo no Core; shell em M-02 |
| Compliance Service (Q-019) | Hipótese PS — enforcement de Governance Domain |
| User Context explícito | OQ-C02 — absorvido em Runtime Context? |

---

## 6. Deferred

| Item | Motivo |
|---|---|
| Event Model detalhado | Q-003 — AS-010 |
| DDD Aggregates | Q-004 — sessão futura |
| MVP por módulo | Q-006 — decisão de produto |
| Mecanismos de extensão na implementação | Q-009 — fase técnica |
| Environment/Deployment no Core | Fase técnica Sprint 3 |

---

## 7. Rejected

| Alternativa | Motivo |
|---|---|
| Core monolítico (regras clínicas no Core) | ADR-0003 rejeitado |
| Core inclui implementação Identity/Audit/Config | Contradiz ADR-0005 — candidatos rejeitados em core-boundary |
| 1 módulo por domínio (16 módulos rígidos) | Risco R-03 — provável rejeição |
| Platform Service como domínio de negócio | Viola D-002 AS-004 |
| Clinical Timeline como módulo ou domínio | D-006 — projeção consumida por módulos |
| Telemedicine como Extension Module | D-005 — Operational Mode em Attendance |

---

## 8. Open Questions impactadas

| ID | Impacto |
|---|---|
| **Q-007** | **Answered** — fronteiras Core/PS/Module/Extension/View definidas |
| Q-009 | Parcial — Extension Mechanism no Core confirmado em D-001 |
| Q-019 | Needs Review — Compliance PS |

---

## 9. Encerramento

Sessão **AS-005 encerrada** em 2026-07-03.

1. ~~NR-001 a NR-007~~ ✅
2. ~~NR-008 — confirmação final~~ ✅
3. Formalização documental executada

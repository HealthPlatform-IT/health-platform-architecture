---
id: AS-005
title: Core Platform Boundary
status: Completed
version: 0.2.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
reviewers: []
phase: Product & Architecture Foundation
category: Product Architecture
priority: High
estimated-impact: High
tags:
  - core-platform
  - platform-services
  - modules
  - extension-model
  - Q-007
related:
  - docs/04-domain/business-domains.md
  - docs/04-domain/domain-map.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - architecture-sessions/AS-004-business-domain-map.md
  - ai-context/open-questions.md
  - workspace/AS-005/README.md
---

# AS-005 — Core Platform Boundary

## Informações Gerais

| Campo | Valor |
|---|---|
| Sessão | AS-005 |
| Tema | Core Platform Boundary |
| Status | Completed |
| Fase | Product & Architecture Foundation |
| Categoria | Product Architecture |
| Prioridade | High |
| Impacto Estimado | High |
| Responsável | Architecture Team |
| Data de início | 2026-07-03 |
| Data de encerramento | 2026-07-03 |
| Open Question | Q-007 (Answered) |

---

## Objetivo

Definir claramente as fronteiras entre:

- **Core Platform**
- **Platform Services**
- **Business Domains**
- **Modules**
- **Extensions**
- **Read Models / Views**

Esta sessão deve responder **Q-007** (completar o que ficou parcial na AS-004):

> Qual é a fronteira exata entre Core Platform, Platform Services, Business Domains e Modules?

---

## Contexto

A Sprint 1 — Business Discovery foi concluída:

- **AS-002** — Business Capability Map (39 sub-capabilities)
- **AS-003** — Care Journey Lifecycle (Q-001 Answered, ADR-0007)
- **AS-004** — Business Domain Map (Q-002 Answered, ADR-0008)

**16 Business Domains** confirmados. Critérios **Domain vs Platform Service vs Read Model** (D-002) definidos na AS-004.

A cadeia conceitual atual:

```text
Healthcare Operating Model
        ↓
Institution Care Journey
        ↓
Core Business Capabilities
        ↓
Business Capability Map
        ↓
Business Domains              ← AS-004 ✅
        ↓
Platform Services
        ↓
Modules                       ← AS-005 (esta sessão)
        ↓
Features
```

ADRs foundation relevantes: ADR-0003 (Core Protection), ADR-0005 (Platform Services), ADR-0006 (Configuration over Customization), ADR-0008 (Business Domain Map).

---

## Problema

Sem fronteiras operacionais claras entre Core, PS, domínios e módulos:

- O **Core** pode inflar ou ser contaminado por regras de cliente ou modelo operacional.
- **Platform Services** podem ser confundidos com domínios de negócio.
- **Business Domains** podem virar módulos 1:1 ou microsserviços prematuros.
- **Módulos** podem acoplar-se diretamente entre si.
- **Extensões** podem virar customização por tenant.
- **Read Models** podem assumir regras de negócio que pertencem a domínios.

Tensão identificada: ADR-0003 lista identidade, autorização e auditoria como infraestrutura do Core; ADR-0005 cataloga Identity, Authorization e Audit como Platform Services. A AS-005 deve reconciliar sem reabrir ADRs Accepted.

---

## Hipótese Inicial

> Hipótese de trabalho — não é decisão final.

- **Core Platform** contém apenas fundamentos estruturais necessários para a plataforma existir como SaaS multi-tenant extensível.
- **Platform Services** implementam responsabilidades transversais reutilizáveis.
- **Business Domains** representam responsabilidades de negócio (16 confirmados).
- **Modules** são implementações funcionais derivadas de domínio(s).
- **Extensions** adicionam variações por modelo operacional sem alterar o Core.
- **Read Models / Views** consolidam leitura cross-domain sem possuir regras de negócio autônomas.

Exemplo conceitual (a validar criticamente):

| Camada | Exemplos candidatos |
|---|---|
| Core Platform | Tenant context · Module registry · Extension contracts · Care Journey type system · Hierarchical Care Model contracts |
| Platform Services | Identity · Authorization · Audit · Communication · Configuration · Document Engine · … |
| Business Domains | Care Delivery · Clinical Record · Communication · Governance & Compliance · … |
| Modules | Scheduling · Clinical Workspace · Patient Portal · Telemedicine · Diagnostic · Home Care |
| Read Models | Clinical Timeline · Analytics & Reporting |

---

## Escopo da Sessão

### Dentro do escopo

1. Definição operacional de **Core Platform** vs Core do ADR-0003.
2. Reconciliação **Core ↔ Platform Services** (tensão ADR-0003/ADR-0005).
3. Critérios para classificar responsabilidades nas 6 categorias.
4. Relação **domínio → módulo** (cardinalidade, opcionalidade).
5. Modelo de **extensão** (Extension Domain vs mecanismo Extension vs composição).
6. **Read Models** — produção, consumo, ownership conceitual.
7. Catálogo **preliminar de módulos candidatos** (derivados dos 16 domínios).
8. Regras de **acoplamento** entre módulos e consumo de PS.
9. Impacto de **Configuration over Customization** em módulos e extensões.
10. Completar **Q-007** conceitualmente.

### Fora do escopo

- Código, banco de dados, APIs, schemas, microsserviços.
- Event Model detalhado (Q-003 — AS-010).
- DDD Aggregates e entidades (Q-004).
- UI, componentes, stack, infraestrutura, deploy.
- MVP técnico e priorização de release (Q-006).
- Bounded Contexts detalhados.
- ADR e documentação oficial em `docs/05-architecture/` (fase pós-confirmação).

---

## Premissas

- ADRs 0001–0008 permanecem Accepted.
- Os 16 Business Domains da AS-004 não serão reagrupados.
- Módulos derivam de domínios e capabilities — não de telas.
- Platform Services não viram módulos; Business Domains não viram microsserviços.

---

## Restrições

- Não inventar decisões — registrar hipóteses e Open Questions quando imaturo.
- Não criar ADR nem docs oficiais antes da confirmação do pacote.
- Não detalhar implementação técnica.
- Não transformar Platform Services em módulos nem domínios em deploy units.

---

## Perguntas Arquiteturais

Ver `workspace/AS-005/questions.md`.

---

## Critérios para Classificar Responsabilidades

Ver `workspace/AS-005/classification-matrix.md`.

---

## Investigação por Camada

| NR | Tema | Workspace | Status |
|---|---|---|---|
| NR-001 | Reconciliar Core vs PS | `core-boundary.md`, `platform-services-boundary.md` | 🟡 Em andamento |
| NR-002 | Definir Core Platform | `core-boundary.md` | ⚪ Pendente |
| NR-003 | Validar fronteiras PS | `platform-services-boundary.md` | ⚪ Pendente |
| NR-004 | Derivar módulos candidatos | `module-strategy-draft.md` | ⚪ Pendente |
| NR-005 | Modelo de extensão | `extension-model-draft.md` | ⚪ Pendente |
| NR-006 | Read Models | `read-models-boundary.md` | ⚪ Pendente |
| NR-007 | Consolidar classification matrix | `classification-matrix.md` | ⚪ Pendente |
| NR-008 | Pacote de decisões + confirmação | `decisions.md`, `confirmation-package.md` | ⚪ Pendente |

---

## Alternativas a Avaliar

| Alternativa | Descrição | Status |
|---|---|---|
| A | Core monolítico (tudo na fundação) | Rejeitada (ADR-0003) — revalidar |
| B | Core = apenas contratos; PS = implementações | A avaliar |
| C | 1 módulo por domínio | A avaliar — provável rejeição |
| D | Módulos transversais (Clinical Workspace, Portal) | A avaliar |
| E | Extension = domínio Extension habilitado como módulo opcional | A avaliar |
| F | Read Model como serviço dedicado vs projeção nos módulos | A avaliar |

---

## Tensões Identificadas

| ID | Tensão | Fonte |
|---|---|---|
| T-001 | Identidade/autorização/auditoria no Core ou em PS? | ADR-0003 vs ADR-0005 |
| T-002 | Extension Business Domain vs mecanismo Extension | ADR-0003 vs ADR-0008 |
| T-003 | Clinical Workspace: módulo, shell ou camada acima? | product-overview.md |
| T-004 | Communication Domain vs Communication Service | ADR-0008 + ADR-0005 |
| T-005 | Compliance Service (Q-019) — PS ou extensão do Audit? | workspace AS-004 |

---

## Riscos

Ver seção de riscos em `workspace/AS-005/notes.md` e preparação AS-005 (R-01 a R-12).

---

## Decisões Esperadas

- Definição de **Core Platform** e lista do que não pertence.
- Critérios unificados **Core / PS / Domain / Module / Extension / View**.
- Relação **domínio → módulo** documentada.
- Regras de **consumo de PS** e **acoplamento entre módulos**.
- Catálogo preliminar de **módulos candidatos** com domínio primário.
- Posicionamento conceitual de **Clinical Workspace**.
- **Q-007** → Answered (conceitualmente).

---

## Documentos Impactados (pós-confirmação) — ✅ Executado

| Documento | Status |
|---|---|
| **ADR-0009** — Core Platform Boundary | ✅ Accepted |
| `docs/05-architecture/core-platform.md` | ✅ Criado |
| `docs/05-architecture/module-strategy.md` | ✅ Criado |
| `docs/05-architecture/extension-model.md` | ✅ Criado |
| `docs/05-architecture/read-models.md` | ✅ Criado |
| `docs/05-architecture/architecture-classification.md` | ✅ Criado |
| `docs/05-architecture/platform-services.md` | ✅ v0.2.0 |
| `docs/00-introduction/glossary.md` | ✅ Criado |
| `docs/01-product/product-overview.md` | ✅ Atualizado |
| `ai-context/architecture-foundation.md` | ✅ Atualizado |
| `ai-context/open-questions.md` | ✅ Q-007 Answered |
| `ai-context/development-guidelines.md` | ✅ Criado |
| `ai-context/adr-summary.md` | ✅ Atualizado |
| `CLAUDE.md` + `.cursor/rules/health-platform.mdc` | ✅ Criados |
| `ARCHITECTURE_INDEX.md` | ✅ Atualizado |

---

## Open Questions Relacionadas

| ID | Relação com AS-005 |
|---|---|
| Q-007 | Pergunta central — **Answered** (ADR-0009) |
| Q-009 | Mecanismos de extensão na implementação — parcial conceitual |
| Q-015 | Herança de configuração — influencia composição de módulos |
| Q-016 | Processo de customização excepcional |
| Q-019 | Compliance Service no catálogo PS |

---

## Critério de Encerramento

A sessão estará pronta para confirmação quando:

1. Pacote de decisões em `workspace/AS-005/decisions.md` com status Ready for Confirmation.
2. `classification-matrix.md` validada com exemplos reais da plataforma.
3. Catálogo preliminar de módulos candidatos com rastreio domínio → capability.
4. Tensão T-001 (Core vs PS) resolvida conceitualmente.
5. `confirmation-package.md` revisado e apresentado ao usuário.
6. Nenhuma decisão de código, API, schema ou stack.

---

## Workspace

Investigação detalhada: [`workspace/AS-005/`](../workspace/AS-005/README.md)

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.2.0 | 2026-07-03 | Sessão encerrada — pacote confirmado, Q-007 Answered |

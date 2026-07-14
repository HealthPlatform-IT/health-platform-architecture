# Health Platform Architecture Index

> Este documento representa o painel de controle da arquitetura da Health Platform.

Seu objetivo é fornecer uma visão consolidada da evolução arquitetural da plataforma, indicando o estado atual do projeto, as decisões já tomadas, as sessões em andamento e o roadmap de arquitetura.

---

# Estado Atual

| Item | Status |
|---|---|
| Repositório | 🟢 Criado |
| Estrutura de Documentação | 🟢 Concluída |
| Templates | 🟢 Concluídos |
| Metodologia de Arquitetura | 🟢 Definida |
| ADRs Foundation | 🟢 18 Accepted (0001–0018) |
| Sprint Atual | Sprint 3 — Technical Architecture 🟢 |
| Fase | Technical Architecture |
| Última Architecture Session | AS-016 — Clinical Aggregates ✅ |
| Sessão em andamento | — |
| Próximo Marco | AS-009 Security · Q-006 MVP |

---

# Roadmap Arquitetural

## Sprint 0 — Foundation

Objetivo: Construir a infraestrutura de documentação e o processo arquitetural da plataforma.

| Entrega | Status |
|---|---|
| Vision | 🟢 |
| Estrutura do Repositório | 🟢 |
| Templates | 🟢 |
| Architecture Session | 🟢 |
| ADR | 🟢 |
| Documentation / AI Context / Diagram Templates | 🟢 |
| Domain Templates | 🟢 |

**Status Geral:** ✅ Concluída

---

## Sprint 1 — Business Discovery

Objetivo: Descobrir como a Health Platform deve ser organizada sob a perspectiva do negócio.

### Architecture Sessions

| Sessão | Tema | Status |
|---|---|---|
| AS-002 | Business Capability Map | ✅ Concluída |
| AS-003 | Care Journey Lifecycle | ✅ Concluída |
| AS-004 | Business Domain Map | ✅ Concluída |

**Status Geral:** ✅ Concluída

---

## Sprint 2 — Product Architecture

Objetivo: Transformar capabilities em domínios, módulos e componentes.

| Entrega | Status |
|---|---|
| Product Overview (revisão) | 🟡 Draft |
| Domain Map | 🟡 Draft |
| Business Domains | 🟡 Draft |
| Platform Services (doc oficial) | 🟢 v0.5.0 |
| Module Strategy | 🟢 ADR-0009 |
| Core Platform | 🟢 ADR-0009 / ADR-0012 |
| Extension Model | 🟢 ADR-0009 |
| Read Models | 🟢 ADR-0009 / ADR-0012 |
| Architecture Classification | 🟢 ADR-0009 |
| Medical Form Engine | 🟢 ADR-0010 |
| Document Engine | 🟢 ADR-0011 |
| Principles | 🟢 Sprint 2 closure |
| Event Strategy | 🟢 ADR-0012 |

**Status Geral:** ✅ Concluída

---

## Sprint 3 — Technical Architecture

Objetivo: Definir a arquitetura técnica da plataforma.

| Entrega | Status |
|---|---|
| API Strategy | 🟢 ADR-0016 |
| Database Architecture | 🟢 ADR-0015 |
| Backend Architecture | 🟢 ADR-0014 |
| Multi-Tenant Strategy (Q-008) | 🟢 ADR-0013 |
| Frontend Architecture | ⚪ |
| Security | ⚪ |
| DevOps / Observability | ⚪ |
| Event Bus Technical (Q-003 tecnologia) | 🟢 ADR-0017 |

**Status Geral:** 🟢 Sprint 3 em andamento (AS-016 ✅)

---

# Architecture Sessions

## Concluídas

| ID | Sessão | Status |
|---|---|---|
| AS-001 | Product Vision | ✅ |
| AS-002 | Business Capability Map | ✅ |
| AS-003 | Care Journey Lifecycle | ✅ |
| AS-004 | Business Domain Map | ✅ |
| AS-005 | Core Platform Boundary | ✅ |
| AS-006 | Medical Form Engine | ✅ |
| AS-007 | Document Engine | ✅ |
| AS-010 | Event Strategy | ✅ |
| AS-011 | Multi-Tenant Strategy | ✅ |
| AS-012 | Backend Architecture | ✅ |
| AS-013 | Database Architecture | ✅ |
| AS-014 | API Strategy | ✅ |
| AS-015 | Event Bus Technical | ✅ |
| AS-016 | Clinical Aggregates (Q-004) | ✅ |

## Em andamento

| ID | Sessão | Prioridade | Status |
|---|---|---|---|
| — | — | — | — |

## Planejadas

| ID | Sessão | Prioridade | Status |
|---|---|---|---|
| AS-008 | Telemedicine | Baixa | ⚪ |
| AS-009 | Platform Security | Média | ⚪ |

---

# Architecture Decision Records (ADR)

> Série oficial: `docs/05-architecture/adr/foundation/`
>
> A pasta `adr/` na raiz contém placeholders legados vazios — **não utilizar**.

## Concluídos (Foundation)

| ADR | Título | Status |
|---|---|---|
| ADR-0001 | Hierarchical Care Model | Accepted |
| ADR-0002 | Capability-Driven Architecture | Accepted |
| ADR-0003 | Core Protection and Extension Model | Accepted |
| ADR-0004 | Communication and Integration Separation | Accepted |
| ADR-0005 | Platform Services | Accepted |
| ADR-0006 | Configuration over Customization | Accepted |
| ADR-0007 | Care Journey Lifecycle | Accepted |
| ADR-0008 | Business Domain Map | Accepted |
| ADR-0009 | Core Platform Boundary | Accepted |
| ADR-0010 | Medical Form Engine | Accepted |
| ADR-0011 | Document Engine | Accepted |
| ADR-0012 | Event Strategy | Accepted |
| ADR-0013 | Multi-Tenant Strategy | Accepted |
| ADR-0014 | Backend Architecture | Accepted |
| ADR-0015 | Database Architecture | Accepted |
| ADR-0016 | API Strategy | Accepted |
| ADR-0017 | Event Bus Technical Strategy | Accepted |
| ADR-0018 | Clinical Domain Aggregates | Accepted |

## Planejados (fase técnica — após confirmação de sessão)

| ADR | Tema | Sessão |
|---|---|---|
| — | Telemedicine | AS-008 |
| — | Platform Security | AS-009 |

---

# Documentação Oficial

## Introdução

| Documento | Caminho | Status |
|---|---|---|
| Vision | `docs/00-introduction/vision.md` | 🟢 |
| Principles | `docs/00-introduction/principles.md` | 🟢 |
| Glossary | `docs/00-introduction/glossary.md` | 🟢 AS-010 |

## Produto

| Documento | Caminho | Status |
|---|---|---|
| Product Overview | `docs/01-product/product-overview.md` | 🟡 Draft |

## Modelo Operacional

| Documento | Caminho | Status |
|---|---|---|
| Healthcare Operating Model | `docs/02-business-processes/healthcare-operating-model.md` | 🟡 Draft |
| Care Journey Lifecycle | `docs/02-business-processes/care-journey-lifecycle.md` | 🟡 Draft |

## Business Capabilities

| Documento | Caminho | Status |
|---|---|---|
| Core Business Capabilities | `docs/03-capabilities/core-business-capabilities.md` | 🟡 Draft |
| Business Capability Map | `docs/03-capabilities/business-capability-map.md` | 🟡 Draft |

## Domínios

| Documento | Caminho | Status |
|---|---|---|
| Business Domains | `docs/04-domain/business-domains.md` | 🟡 Draft |
| Domain Map | `docs/04-domain/domain-map.md` | 🟡 Draft |

## Arquitetura

| Documento | Caminho | Status |
|---|---|---|
| Document Engine | `docs/05-architecture/document-engine.md` | 🟢 ADR-0011 |
| Event Strategy | `docs/05-architecture/event-strategy.md` | 🟢 ADR-0012 / ADR-0017 |
| Clinical Aggregates | `docs/05-architecture/clinical-aggregates.md` | 🟢 ADR-0018 |
| API Strategy | `docs/05-architecture/api-strategy.md` | 🟢 ADR-0016 |
| Database Architecture | `docs/05-architecture/database-architecture.md` | 🟢 ADR-0015 |
| Backend Architecture | `docs/05-architecture/backend-architecture.md` | 🟢 ADR-0014 |
| Multi-Tenant Strategy | `docs/05-architecture/multi-tenant-strategy.md` | 🟢 ADR-0013 |
| Platform Services | `docs/05-architecture/platform-services.md` | 🟢 v0.5.0 |
| Medical Form Engine | `docs/05-architecture/medical-form-engine.md` | 🟢 ADR-0010 |
| Core Platform | `docs/05-architecture/core-platform.md` | 🟢 ADR-0009 / ADR-0012 / ADR-0013 |
| Module Strategy | `docs/05-architecture/module-strategy.md` | 🟢 ADR-0009 |
| Extension Model | `docs/05-architecture/extension-model.md` | 🟢 ADR-0009 |
| Read Models | `docs/05-architecture/read-models.md` | 🟢 ADR-0009 / ADR-0012 |
| Architecture Classification | `docs/05-architecture/architecture-classification.md` | 🟢 ADR-0009 |

---

# AI Context

| Documento | Caminho | Status |
|---|---|---|
| Architecture Foundation | `ai-context/architecture-foundation.md` | 🟡 Draft |
| Open Questions | `ai-context/open-questions.md` | 🟡 Draft |
| ADR Summary | `ai-context/adr-summary.md` | 🟡 Draft |
| Platform Overview | `ai-context/platform-overview.md` | 🟡 Draft |
| Development Guidelines | `ai-context/development-guidelines.md` | 🟢 v0.3.0 |

---

# Diagramas

| Documento | Status |
|---|---|
| Business Capability Map | ⚪ |
| Domain Map | ⚪ |
| Core Architecture | ⚪ |

---

# Próximos Passos

## Onda 1 — Consolidar

1. ~~`ai-context/open-questions.md`~~ ✅
2. ~~Encerrar AS-002~~ ✅
3. ~~Atualizar `ARCHITECTURE_INDEX.md`~~ ✅
4. ~~`docs/00-introduction/principles.md`~~ ✅ Sprint 2 closure 2026-07-03

## Onda 2 — Jornada de Cuidado

1. ~~Iniciar **AS-003 — Care Journey Lifecycle**~~ ✅
2. ~~Resolver **Q-001**~~ ✅
3. ~~Criar `docs/02-business-processes/care-journey-lifecycle.md`~~ ✅
4. ~~Completar `healthcare-operating-model.md`~~ ✅

## Onda 3 — Business Domains

1. ~~**AS-004 — Business Domain Map** (Q-002)~~ ✅
2. ~~Criar `business-domains.md` e `domain-map.md`~~ ✅
3. ~~ADR-0008~~ ✅

## Onda 4 — Consolidação pós-Sprint 1

1. ~~`platform-services.md`~~ ✅
2. ~~Revisar `product-overview.md`~~ ✅
3. ~~AI Contexts derivados (`adr-summary.md`, `platform-overview.md`)~~ ✅
4. ~~Atualizar `architecture-foundation.md` e docs derivados~~ ✅
5. ~~`development-guidelines.md`, regras Cursor/Claude~~ ✅
6. ~~**AS-005** — Core Platform Boundary~~ ✅ Confirmada 2026-07-03
7. ~~Consolidação documental pós-AS-005~~ ✅ (ADR-0009, docs oficiais, glossary, IA)
8. ~~**AS-006** — Medical Form Engine~~ ✅ Confirmada 2026-07-03
9. ~~**AS-007** — Document Engine~~ ✅ Confirmada 2026-07-03
10. ~~Sprint 2 closure (P0/P1 documental)~~ ✅ 2026-07-03
11. ~~**AS-010** — Event Strategy (Q-003)~~ ✅ Confirmada 2026-07-03 (ADR-0012)
12. **Sprint 3** — Technical Architecture 🟢 iniciada 2026-07-04
13. ~~**AS-011** — Multi-Tenant Strategy (Q-008)~~ ✅ Confirmada 2026-07-14 (ADR-0013)
14. ~~**AS-012** — Backend Architecture~~ ✅ Confirmada 2026-07-14 (ADR-0014)
15. ~~**AS-013** — Database Architecture~~ ✅ Confirmada 2026-07-14 (ADR-0015)
16. ~~**AS-014** — API Strategy~~ ✅ Confirmada 2026-07-14 (ADR-0016)
17. ~~**AS-015** — Event Bus Technical~~ ✅ Confirmada 2026-07-14 (ADR-0017)
18. ~~**AS-016** — Clinical Aggregates (Q-004)~~ ✅ Confirmada 2026-07-14 (ADR-0018)
19. AS-009 Security · Q-006 MVP |

---

# Princípio da Arquitetura

Toda decisão arquitetural da Health Platform seguirá obrigatoriamente o fluxo abaixo:

```text
Architecture Session
        ↓
Workspace Draft
        ↓
Decision
        ↓
Architecture Decision Record (ADR)
        ↓
Documentação Oficial
        ↓
AI Context
        ↓
Diagramas
        ↓
Implementação
```

Nenhuma implementação deverá ocorrer antes da conclusão das etapas arquiteturais correspondentes.

---

# Visão de Longo Prazo

A Health Platform está sendo concebida como uma plataforma de saúde modular, escalável e orientada a capacidades de negócio.

Toda evolução arquitetural deverá preservar os seguintes princípios:

* Orientação por Jornada de Cuidado e modelos operacionais de saúde.
* Organização por Core Business Capabilities.
* Separação clara de responsabilidades.
* Baixo acoplamento entre domínios.
* Configuração acima de customização.
* Core pequeno e protegido — crescimento por extensão.
* Platform Services para responsabilidades transversais.
* Documentação antes da implementação.

Este documento deve ser mantido atualizado ao final de cada Architecture Session, refletindo sempre o estado atual da arquitetura da plataforma.

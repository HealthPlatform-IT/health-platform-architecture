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
| ADRs Foundation | 🟢 9 Accepted (0001–0009) |
| Sprint Atual | Sprint 2 — Product Architecture |
| Fase | Product & Architecture Foundation (~99%) |
| Última Architecture Session | AS-005 — Core Platform Boundary ✅ |
| Próximo Marco | AS-006 — Medical Form Engine |

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
| Platform Services (doc oficial) | 🟢 v0.2.0 |
| Module Strategy | 🟢 ADR-0009 |
| Core Platform | 🟢 ADR-0009 |
| Extension Model | 🟢 ADR-0009 |
| Read Models | 🟢 ADR-0009 |
| Architecture Classification | 🟢 ADR-0009 |
| Event Strategy | ⚪ |

**Status Geral:** 🟡 AS-005 concluída — consolidação documental executada

---

## Sprint 3 — Technical Architecture

Objetivo: Definir a arquitetura técnica da plataforma.

| Entrega | Status |
|---|---|
| Backend / Frontend / Database Architecture | ⚪ |
| Security | ⚪ |
| DevOps / Observability | ⚪ |
| Multi-Tenant Strategy (Q-008) | ⚪ |

**Status Geral:** ⚪ Não iniciada

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

## Planejadas

| ID | Sessão | Prioridade |
|---|---|---|
| AS-006 | Medical Form Engine | Média |
| AS-007 | Document Engine | Média |
| AS-008 | Telemedicine | Baixa |
| AS-009 | Platform Security | Média |
| AS-010 | Event Strategy | Futura |

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

## Planejados (fase futura)

| ADR | Tema | Sessão |
|---|---|---|
| — | Medical Form Engine | AS-006 |
| — | Document Engine | AS-007 |
| — | Clinical Workspace | AS-005 ✅ (M-02 shell) |
| — | Event Strategy | AS-010 |
| — | Telemedicine | AS-008 |

---

# Documentação Oficial

## Introdução

| Documento | Caminho | Status |
|---|---|---|
| Vision | `docs/00-introduction/vision.md` | 🟢 |
| Principles | `docs/00-introduction/principles.md` | ⚪ |
| Glossary | `docs/00-introduction/glossary.md` | 🟢 AS-005 |

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
| Platform Services | `docs/05-architecture/platform-services.md` | 🟢 v0.2.0 |
| Core Platform | `docs/05-architecture/core-platform.md` | 🟢 ADR-0009 |
| Module Strategy | `docs/05-architecture/module-strategy.md` | 🟢 ADR-0009 |
| Extension Model | `docs/05-architecture/extension-model.md` | 🟢 ADR-0009 |
| Read Models | `docs/05-architecture/read-models.md` | 🟢 ADR-0009 |
| Architecture Classification | `docs/05-architecture/architecture-classification.md` | 🟢 ADR-0009 |

---

# AI Context

| Documento | Caminho | Status |
|---|---|---|
| Architecture Foundation | `ai-context/architecture-foundation.md` | 🟡 Draft |
| Open Questions | `ai-context/open-questions.md` | 🟡 Draft |
| ADR Summary | `ai-context/adr-summary.md` | 🟡 Draft |
| Platform Overview | `ai-context/platform-overview.md` | 🟡 Draft |
| Development Guidelines | `ai-context/development-guidelines.md` | 🟢 v0.1.0 |

---

# Diagramas

| Documento | Status |
|---|---|
| Business Capability Map | ⚪ |
| Domain Map | ⚪ |
| Core Architecture | ⚪ |

---

# Próximos Passos

## Onda 1 — Consolidar (em andamento)

1. ~~`ai-context/open-questions.md`~~ ✅
2. ~~Encerrar AS-002~~ ✅
3. ~~Atualizar `ARCHITECTURE_INDEX.md`~~ ✅
4. Criar ou atualizar `docs/00-introduction/principles.md`

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

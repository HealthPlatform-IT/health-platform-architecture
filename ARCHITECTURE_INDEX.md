# Health Platform Architecture Index

> Este documento representa o painel de controle da arquitetura da Health Platform.

Seu objetivo é fornecer uma visão consolidada da evolução arquitetural da plataforma, indicando o estado atual do projeto, as decisões já tomadas, as sessões em andamento e o roadmap de arquitetura.

---

# Estado Atual

| Item                        | Status                              |
| --------------------------- | ----------------------------------- |
| Repositório                 | 🟢 Criado                           |
| Estrutura de Documentação   | 🟢 Concluída                        |
| Templates                   | 🟢 Concluídos                       |
| Metodologia de Arquitetura  | 🟢 Definida                         |
| Sprint Atual                | Sprint 1 — Business Discovery       |
| Última Architecture Session | AS-002 — Business Capability Map    |
| Próximo Marco               | Definição das Business Capabilities |

---

# Roadmap Arquitetural

## Sprint 0 — Foundation

Objetivo: Construir toda a infraestrutura de documentação e o processo arquitetural da plataforma.

### Status

* 🟢 Vision
* 🟢 Estrutura do Repositório
* 🟢 Templates
* 🟢 Architecture Session
* 🟢 ADR
* 🟢 Documentation Template
* 🟢 AI Context Template
* 🟢 Diagram Template
* 🟢 Domain Templates

**Status Geral:** ✅ Concluída

---

## Sprint 1 — Business Discovery

Objetivo: Descobrir como a Health Platform deve ser organizada sob a perspectiva do negócio.

### Architecture Sessions

| Sessão | Tema                    | Status          |
| ------ | ----------------------- | --------------- |
| AS-002 | Business Capability Map | 🟡 Em andamento |
| AS-003 | Business Domains        | ⚪ Planejada     |
| AS-004 | Core Platform           | ⚪ Planejada     |
| AS-005 | Clinical Workspace      | ⚪ Planejada     |

---

## Sprint 2 — Product Architecture

Objetivo: Transformar as capacidades em domínios, módulos e componentes.

### Entregas previstas

* Product Overview
* Domain Map
* Module Strategy
* Core Architecture
* Event Strategy

Status: ⚪ Não iniciada

---

## Sprint 3 — Technical Architecture

Objetivo: Definir a arquitetura técnica da plataforma.

### Entregas previstas

* Backend Architecture
* Frontend Architecture
* Database Architecture
* Security
* DevOps
* Observability

Status: ⚪ Não iniciada

---

# Architecture Sessions

## Concluídas

| ID     | Sessão         | Status |
| ------ | -------------- | ------ |
| AS-001 | Product Vision | ✅      |

---

## Em andamento

| ID     | Sessão                  | Status |
| ------ | ----------------------- | ------ |
| AS-002 | Business Capability Map | 🟡     |

---

## Planejadas

* AS-003 — Business Domains
* AS-004 — Core Platform
* AS-005 — Clinical Workspace
* AS-006 — Medical Form Engine
* AS-007 — Document Engine
* AS-008 — Telemedicine
* AS-009 — Platform Security
* AS-010 — Event Strategy

---

# Architecture Decision Records (ADR)

## Concluídos

Nenhum.

---

## Planejados

* ADR-0001 — Business Capability Map
* ADR-0002 — Business Domains
* ADR-0003 — Core Platform
* ADR-0004 — Clinical Workspace
* ADR-0005 — Medical Form Engine
* ADR-0006 — Document Engine
* ADR-0007 — Telemedicine
* ADR-0008 — Event Strategy

---

# Documentação Oficial

## Introdução

| Documento                | Status |
| ------------------------ | ------ |
| Vision                   | 🟢     |
| Principles               | ⚪      |
| Glossary                 | ⚪      |
| Architecture Methodology | ⚪      |

---

## Produto

| Documento        | Status   |
| ---------------- | -------- |
| Product Overview | 🟡 Draft |

---

## Business Capabilities

| Documento               | Status |
| ----------------------- | ------ |
| Business Capability Map | ⚪      |

---

# AI Context

| Documento         | Status |
| ----------------- | ------ |
| Platform Overview | ⚪      |

---

# Diagramas

| Documento               | Status |
| ----------------------- | ------ |
| Business Capability Map | ⚪      |
| Domain Map              | ⚪      |
| Core Architecture       | ⚪      |

---

# Próximos Passos

A prioridade atual da arquitetura é concluir a AS-002 — Business Capability Map.

Após a conclusão da sessão serão realizadas as seguintes atividades:

1. Formalizar a decisão através do ADR-0001.
2. Publicar o documento oficial `docs/02-capabilities/business-capability-map.md`.
3. Atualizar o Product Overview.
4. Gerar o AI Context correspondente.
5. Criar os diagramas oficiais.
6. Iniciar a AS-003 — Business Domains.

---

# Princípio da Arquitetura

Toda decisão arquitetural da Health Platform seguirá obrigatoriamente o fluxo abaixo:

```text
Architecture Session
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

* Separação clara de responsabilidades.
* Baixo acoplamento entre domínios.
* Alta coesão dos módulos.
* Configuração acima de customização.
* Evolução incremental da arquitetura.
* Proteção do Core da plataforma.
* Documentação como fonte oficial de conhecimento.
* Arquitetura guiando o desenvolvimento, e não o contrário.

Este documento deve ser mantido atualizado ao final de cada Architecture Session, refletindo sempre o estado atual da arquitetura da plataforma.

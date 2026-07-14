---
title: ADR-0002 - Capability-Driven Architecture
status: Accepted
date: 2026-07-02
deciders:
  - Architecture Team
tags:
  - architecture
  - capabilities
  - foundation
  - business
related:
  - architecture-sessions/AS-002-business-capability-map.md
  - docs/03-capabilities/core-business-capabilities.md
  - docs/03-capabilities/business-capability-map.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
---

# ADR-0002 — Capability-Driven Architecture

## Status

Accepted

---

# Contexto

A Health Platform foi concebida como uma plataforma SaaS modular capaz de atender diferentes modelos operacionais de saúde — consultórios, clínicas, laboratórios, home care e, futuramente, hospitais.

Durante a **AS-002 — Business Capability Map**, foi identificado que organizar a plataforma diretamente por módulos, telas ou tecnologias criaria riscos de crescimento desordenado, acoplamento entre funcionalidades e dificuldade para encaixar novos modelos operacionais no futuro.

Era necessário definir **como a plataforma deve ser organizada** antes de derivar domínios, módulos, APIs e implementação.

> O processo completo de discussão está registrado em `architecture-sessions/AS-002-business-capability-map.md`.

---

# Problema

Como organizar a Health Platform de forma estável e orientada por negócio, garantindo que:

- Novas funcionalidades não sejam criadas a partir de telas ou requisitos pontuais.
- O Core da plataforma permaneça protegido contra responsabilidades indevidas.
- Diferentes modelos operacionais de saúde possam ser suportados sobre uma base comum.
- Domínios, módulos e implementação sejam consequência do modelo de negócio — e não o ponto de partida.

---

# Decisão

A Health Platform adotará **Capability-Driven Architecture** como modelo organizacional da arquitetura.

## 1. Oito Core Business Capabilities como fundação

A plataforma será organizada em torno de oito capacidades fundamentais de negócio:

```text
Relacionar → Organizar → Executar → Registrar → Comunicar → Integrar → Monitorar → Governar
```

Essas capacidades representam o menor conjunto de competências necessárias para que qualquer instituição de saúde opere dentro do escopo da plataforma.

Elas são permanentes, estáveis e independentes de módulos, telas ou tecnologias.

## 2. Sequência obrigatória de derivação

Toda evolução da plataforma deve seguir esta sequência:

```text
Healthcare Operating Model
        ↓
Core Business Capabilities
        ↓
Business Capability Map
        ↓
Business Domains
        ↓
Modules
        ↓
Features
        ↓
Components
        ↓
Screens
```

Nenhum módulo, API, entidade de banco de dados ou tela deve ser criado sem identificar a qual capability pertence.

## 3. Dois níveis de capabilities

| Nível | Documento | Descrição |
|---|---|---|
| Core Business Capability | `core-business-capabilities.md` | 8 capacidades fundamentais — permanentes |
| Business Capability | `business-capability-map.md` | Sub-capabilities de segundo nível — decomposição das cores |

## 4. Distinções obrigatórias

| Conceito | Representa |
|---|---|
| Core Business Capability | Competência permanente de negócio |
| Business Capability (sub-capability) | Decomposição operacional de uma core |
| Business Domain | Fronteira de responsabilidade derivada do mapa |
| Platform Service | Implementação reutilizável de responsabilidade transversal |
| Module | Solução implementável derivada de domínio e capability |

Capabilities, domínios, módulos e Platform Services são conceitos distintos e não devem ser confundidos.

---

# Alternativas consideradas

## Alternativa A — Organização por módulos

Organizar a plataforma diretamente por módulos funcionais: agenda, prontuário, telemedicina, documentos, laudos, portal do paciente.

**Vantagens:** mais simples de entender; próximo da visão do usuário; facilita planejamento de MVP.

**Desvantagens:** crescimento desordenado; mistura negócio com implementação; dificulta novos módulos; gera acoplamento.

**Resultado:** rejeitada.

---

## Alternativa B — Organização por domínios técnicos

Organizar a plataforma por domínios técnicos: backend, frontend, banco de dados, autenticação, storage, APIs.

**Vantagens:** facilita visão técnica; útil para organização de repositórios.

**Desvantagens:** começa pela tecnologia; não explica valor de negócio; não descobre capacidades clínicas e operacionais.

**Resultado:** rejeitada.

---

## Alternativa C — Organização por capacidades de negócio

Organizar a plataforma por capacidades de negócio — o que a plataforma precisa ser capaz de realizar, independentemente de implementação.

**Vantagens:** foco no negócio; evolução de longo prazo; base para domínios; protege o Core; planeja expansões futuras.

**Desvantagens:** maior esforço conceitual inicial; pode parecer abstrato; exige tradução posterior em domínios e módulos.

**Resultado:** adotada.

---

# Justificativa

Organizar a plataforma por capacidades de negócio permite compreender o produto de forma estratégica e duradoura.

As Core Business Capabilities permanecem estáveis ao longo do tempo, enquanto módulos, features e tecnologias evoluem sobre elas.

Essa abordagem:

- Evita que a arquitetura seja guiada por telas ou requisitos pontuais.
- Protege o Core de responsabilidades indevidas.
- Facilita a descoberta de Business Domains (AS-004 — concluída, ADR-0008).
- Permite suportar múltiplos modelos operacionais sobre uma base comum.
- Complementa o Modelo Hierárquico do Cuidado (ADR-0001) com uma organização operacional coerente.

---

# Regras derivadas

Toda nova funcionalidade, módulo ou proposta de implementação deve obedecer:

1. **Capability primária** — identificar a qual Core Business Capability pertence.
2. **Capabilities antes de módulos** — módulos derivam de capabilities e domínios, não o contrário.
3. **Múltiplas capabilities** — se uma funcionalidade parece pertencer a várias, envolve orquestração entre domínios ou deve ser decomposta.
4. **Sub-capability** — identificar também a Business Capability do mapa quando aplicável.
5. **Sem atalhos** — não criar módulos baseados apenas em telas ou solicitações isoladas.
6. **Capabilities ≠ serviços** — responsabilidades transversais reutilizáveis são Platform Services, não capabilities.

---

# Consequências

## Positivas

- Base organizacional estável e orientada por negócio.
- Proteção do Core contra crescimento desordenado.
- Facilita onboarding de novos membros e agentes de IA.
- Base sólida para definição de Business Domains, APIs e modelagem de dados.
- Suporta evolução para novos modelos operacionais sem reestruturação.
- Complementaridade com o Modelo Hierárquico do Cuidado (ADR-0001).

## Negativas

- Exige maior esforço conceitual antes da implementação.
- Pode parecer abstrato para quem espera começar por telas ou módulos.
- Requer disciplina arquitetural contínua para evitar atalhos.
- A tradução de capabilities em módulos ainda está pendente (AS-005).

---

# Impacto na Arquitetura

Este ADR influencia diretamente:

- Business Capability Map
- Business Domains (ADR-0008)
- Module Strategy
- API Strategy
- Database Modeling
- Platform Services
- AI Context
- Product Overview
- Development Guidelines

---

# Documentação oficial derivada

| Documento | Relação |
|---|---|
| `docs/03-capabilities/core-business-capabilities.md` | Define as 8 Core Business Capabilities |
| `docs/03-capabilities/business-capability-map.md` | Decompõe as cores em 39 sub-capabilities |
| `docs/04-domain/business-domains.md` | Catálogo dos 16 Business Domains |
| `docs/04-domain/domain-map.md` | Mapeamento sub-capability → domínio |

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| [ADR-0001 — Modelo Hierárquico do Cuidado](ADR-0001-hierarchical-care-model.md) | Complementar — ADR-0001 define **como** representar o cuidado; ADR-0002 define **como** organizar a plataforma |
| ADR-0003 — Core Protection & Extension | Depende desta decisão |
| ADR-0004 — Communication vs. Integration | Depende desta decisão |
| ADR-0005 — Platform Services | Depende desta decisão |

Nenhum ADR futuro deverá contrariar a organização por capabilities sem que este documento seja revisado.

---

# Open Questions

| ID | Pergunta | Situação |
|---|---|---|
| Q-002 | Como agrupar sub-capabilities em Business Domains? | ✅ Answered — ADR-0008 |
| Q-005 | Onde posicionar capabilities administrativas e financeiras? | Pendente |
| Q-006 | Quais sub-capabilities pertencem ao escopo inicial (MVP)? | ✅ Answered — ADR-0019 |

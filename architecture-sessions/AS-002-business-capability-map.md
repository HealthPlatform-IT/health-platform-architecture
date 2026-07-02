---
id: AS-002
title: Business Capability Map
status: Completed
version: 1.0.0
created: 2026-06-30
updated: 2026-07-02
author: Architecture Team
reviewers: []
phase: Product & Architecture Foundation
category: Business Discovery
priority: High
estimated-impact: High
tags:
  - business-capabilities
  - product-architecture
  - domain-discovery
related:
  - docs/00-introduction/vision.md
  - docs/03-capabilities/core-business-capabilities.md
  - docs/03-capabilities/business-capability-map.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md
  - ai-context/open-questions.md
  - workspace/AS-002/decisions.md
---

# AS-002 — Business Capability Map

## Informações Gerais

| Campo | Valor |
|---|---|
| Sessão | AS-002 |
| Tema | Business Capability Map |
| Status | Completed |
| Fase | Product & Architecture Foundation |
| Categoria | Business Discovery |
| Prioridade | High |
| Impacto Estimado | High |
| Responsável | Architecture Team |
| Data | 2026-06-30 |

---

## Objetivo

Definir o mapa inicial de capacidades de negócio da Health Platform.

Esta sessão tem como objetivo identificar quais capacidades a plataforma precisa possuir para atender instituições de saúde de diferentes portes, especialidades e modelos operacionais ao longo dos próximos anos.

---

## Hipótese Inicial

A Health Platform deve ser organizada inicialmente a partir de capacidades de negócio, e não a partir de módulos, telas ou tecnologias.

As capacidades de negócio devem representar aquilo que a plataforma precisa ser capaz de realizar, independentemente de como essas capacidades serão implementadas tecnicamente.

---

## Contexto

A Health Platform está sendo concebida como uma plataforma SaaS modular para saúde, capaz de atender consultórios, clínicas, redes de atendimento e futuras instituições mais complexas.

Até o momento, a visão do produto já definiu que a plataforma deve ser modular, configurável, multi-tenant, preparada para diferentes especialidades e capaz de evoluir ao longo de vários anos.

Antes de definir módulos, entidades, banco de dados, APIs ou arquitetura técnica, é necessário compreender quais são as grandes capacidades de negócio que sustentam a plataforma.

---

## Problema

Se a plataforma for organizada diretamente por funcionalidades ou telas, existe o risco de crescimento desordenado, acoplamento entre módulos e dificuldade para encaixar novas funcionalidades no futuro.

Precisamos definir uma estrutura conceitual mais estável, capaz de orientar o crescimento da plataforma antes das decisões técnicas.

---

## Premissas

- A plataforma será multi-tenant.
- A plataforma deverá atender múltiplas clínicas e unidades.
- A plataforma deverá suportar diferentes especialidades médicas.
- A plataforma deverá permitir novos módulos no futuro.
- O Core deverá permanecer pequeno e estável.
- A documentação deve guiar o desenvolvimento antes da implementação.
- A plataforma deve ser preparada para integrações e inteligência artificial.

---

## Restrições

- A solução não deve partir de telas ou funcionalidades isoladas.
- A solução não deve acoplar módulos diretamente ao Core.
- A solução deve permitir evolução futura sem reescrita arquitetural.
- A solução deve ser compreensível para novos membros da equipe.
- A solução deve separar negócio, domínio, módulos e implementação.

---

## Perguntas Arquiteturais

- Quais capacidades fundamentais a Health Platform precisa possuir?
- Quais capacidades pertencem à operação clínica?
- Quais capacidades pertencem à operação administrativa?
- Quais capacidades pertencem à plataforma como infraestrutura?
- Quais capacidades serão necessárias apenas em fases futuras?
- Como evitar confundir capacidades com módulos?
- Como esse mapa ajudará a definir domínios, módulos e APIs no futuro?
- Como esse mapa ajuda a proteger o Core da plataforma?

---

## Alternativas Avaliadas

### Alternativa A — Organizar por módulos

#### Descrição

A plataforma seria organizada diretamente por módulos como agenda, prontuário, telemedicina, documentos, laudos e portal do paciente.

#### Vantagens

- Mais simples de entender inicialmente.
- Próximo da visão funcional do usuário.
- Facilita planejamento de MVP.

#### Desvantagens

- Pode levar a crescimento desorganizado.
- Mistura capacidades de negócio com implementação.
- Dificulta encaixar novos módulos futuros.
- Pode gerar acoplamento entre funcionalidades.

---

### Alternativa B — Organizar por domínios técnicos

#### Descrição

A plataforma seria organizada a partir de domínios técnicos como backend, frontend, banco de dados, autenticação, storage e APIs.

#### Vantagens

- Facilita a visão técnica.
- Ajuda posteriormente na implementação.
- Pode ser útil para organização de repositórios.

#### Desvantagens

- Começa pela tecnologia antes do negócio.
- Não explica como a plataforma gera valor.
- Não ajuda a descobrir capacidades clínicas e operacionais.
- Pode direcionar decisões prematuras de implementação.

---

### Alternativa C — Organizar por capacidades de negócio

#### Descrição

A plataforma seria organizada inicialmente a partir de capacidades de negócio, representando aquilo que o produto precisa ser capaz de realizar, independentemente de módulos, telas ou tecnologias.

#### Vantagens

- Mantém foco no negócio.
- Facilita evolução de longo prazo.
- Ajuda a definir domínios posteriormente.
- Evita confundir módulo com capacidade.
- Protege o Core de responsabilidades indevidas.
- Ajuda a planejar futuras expansões.

#### Desvantagens

- Exige maior esforço conceitual inicial.
- Pode parecer abstrato no começo.
- Precisa ser traduzido depois em domínios, módulos e funcionalidades.

---

## Trade-offs

A organização por módulos é mais simples para iniciar o desenvolvimento, mas pode limitar a visão estratégica da plataforma.

A organização por domínios técnicos é útil para implementação, mas prematura nesta fase.

A organização por capacidades de negócio exige maior esforço inicial, porém oferece uma base mais estável para evolução futura.

---

## Riscos

### Risco 1 — Abstração excessiva

O mapa de capacidades pode ficar abstrato demais e distante da implementação.

**Mitigação:** cada capability deverá futuramente ser relacionada a domínios, módulos e fluxos reais.

---

### Risco 2 — Confundir capacidade com módulo

Uma capability pode ser confundida com um módulo funcional.

**Mitigação:** documentar claramente que capability representa uma capacidade de negócio, enquanto módulo representa uma solução implementável.

---

### Risco 3 — Mapa incompleto

Algumas capacidades futuras podem não ser identificadas agora.

**Mitigação:** o mapa será versionado e poderá evoluir com novas sessões de arquitetura.

---

## Decisão

A Health Platform será organizada inicialmente a partir de um Business Capability Map.

Esse mapa servirá como base para a descoberta dos domínios, definição dos módulos, planejamento das APIs, modelagem do banco e evolução futura da plataforma.

---

## Justificativa

Organizar a plataforma por capacidades de negócio permite compreender o produto de forma mais estratégica e duradoura.

Essa abordagem evita que a arquitetura seja guiada prematuramente por telas, módulos ou tecnologias, mantendo o foco naquilo que a plataforma precisa ser capaz de realizar para entregar valor ao setor de saúde.

---

## Impactos Arquiteturais

- Product Overview
- Domain Map
- Core Architecture
- Module Strategy
- API Strategy
- Database Modeling
- AI Context
- Roadmap

---

## Critérios de Qualidade da Decisão

| Critério | Avaliação |
|---|---|
| Escalabilidade | ⭐⭐⭐⭐⭐ |
| Modularidade | ⭐⭐⭐⭐⭐ |
| Baixo Acoplamento | ⭐⭐⭐⭐⭐ |
| Simplicidade | ⭐⭐⭐⭐☆ |
| Evolução Futura | ⭐⭐⭐⭐⭐ |
| Impacto no Core | Baixo |

---

## Próximas Ações

### Concluídas nesta sessão

- ADR-0002 — Capability-Driven Architecture (Accepted).
- `docs/03-capabilities/core-business-capabilities.md` — 8 Core Business Capabilities.
- `docs/03-capabilities/business-capability-map.md` — decomposição em 35 sub-capabilities.
- Decisões emergentes do workspace refletidas nos ADRs foundation (ADR-0003 a ADR-0006).
- `ai-context/open-questions.md` — perguntas em aberto centralizadas.

### Adiadas para entregas futuras

- Atualizar `docs/01-product/product-overview.md` (Onda 4).
- Criar `ai-context/platform-overview.md` (Onda 4).
- Atualizar `ARCHITECTURE_INDEX.md` (Onda 1, entrega 1.3).
- Diagramas oficiais do Business Capability Map.

### Próxima Architecture Session

**AS-003 — Care Journey Lifecycle**

Resolver Q-001 (início da Institution Care Journey) antes de derivar Business Domains na AS-004.

---

## Questões em Aberto

### Respondidas nesta sessão

| Questão | Resposta / Destino |
|---|---|
| Quais serão as capacidades iniciais da plataforma? | 8 Core Business Capabilities — `core-business-capabilities.md` |
| Como agrupar capacidades clínicas, administrativas e técnicas? | Modelo de 8 cores adotado; rascunho de 6 grupos abandonado — ADR-0002 |

### Encaminhadas para `ai-context/open-questions.md`

| Questão original | ID | Sessão recomendada |
|---|---|---|
| Como relacionar capabilities com domínios? | Q-002 | AS-004 — Business Domain Map |
| Quais capabilities fazem parte do MVP? | Q-006 | Future Product/Operational Session |
| Quais capabilities são futuras? | Q-006 | Future Product/Operational Session |

### Adiada — após definição de domínios

| Questão | Observação |
|---|---|
| Como relacionar capabilities com módulos? | Depende de Q-002 (Business Domains) e AS-004 |

Pergunta adicional da sessão registrada em `workspace/AS-002/questions.md`:

| ID | Pergunta | Sessão recomendada |
|---|---|---|
| Q-001 | Quando a instituição assume responsabilidade pelo cuidado? | AS-003 — Care Journey Lifecycle |

---

## Resultado da Sessão

A sessão foi **concluída com sucesso**.

A Health Platform será organizada por **capacidades de negócio**, com oito Core Business Capabilities e um Business Capability Map de 35 sub-capabilities. A decisão foi formalizada no ADR-0002. Decisões emergentes durante a sessão foram refletidas nos ADRs foundation ADR-0003 a ADR-0006.

O escopo desta sessão **não incluía** derivar Business Domains, definir MVP ou documentar o ciclo de vida completo da Jornada de Cuidado — esses temas permanecem abertos e estão registrados em `ai-context/open-questions.md`.

---

## Checklist de Encerramento

- [x] A hipótese inicial foi confirmada — organização por capacidades de negócio (Alternativa C).
- [x] As premissas foram validadas.
- [x] Os riscos foram identificados.
- [x] A decisão foi registrada — seção Decisão desta sessão + workspace/AS-002/decisions.md.
- [x] O ADR foi criado — ADR-0002 (Accepted).
- [x] A documentação oficial foi atualizada — `core-business-capabilities.md` e `business-capability-map.md` (Draft).
- [x] Perguntas em aberto centralizadas — `ai-context/open-questions.md`.
- [x] Próxima sessão indicada — AS-003 — Care Journey Lifecycle.
- [ ] Todas as perguntas arquiteturais foram respondidas — domínios, MVP e jornada encaminhados como Open Questions.
- [ ] O AI Context derivado foi atualizado — `platform-overview.md` pendente (Onda 4).
- [ ] Os diagramas foram revisados — pendente.
- [ ] O ARCHITECTURE_INDEX foi atualizado — pendente (Onda 1, entrega 1.3).
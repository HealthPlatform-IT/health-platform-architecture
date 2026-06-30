---
id: AS-002
title: Business Capability Map
status: Draft
version: 0.1.0
created: 2026-06-30
updated: 2026-06-30
author: Architecture Team
reviewers: []
phase: Product Architecture
category: Business Discovery
priority: High
estimated-impact: High
tags:
  - business-capabilities
  - product-architecture
  - domain-discovery
related:
  - docs/00-introduction/vision.md
---

# AS-002 — Business Capability Map

## Informações Gerais

| Campo | Valor |
|---|---|
| Sessão | AS-002 |
| Tema | Business Capability Map |
| Status | Draft |
| Fase | Product Architecture |
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

- Criar ADR correspondente.
- Criar `docs/02-capabilities/business-capability-map.md`.
- Atualizar `docs/01-product/product-overview.md`.
- Criar `ai-context/platform-overview.md`.
- Atualizar `ARCHITECTURE_INDEX.md`.

---

## Questões em Aberto

- Quais serão as capacidades iniciais da plataforma?
- Como agrupar capacidades clínicas, administrativas e técnicas?
- Como relacionar capabilities com domínios?
- Como relacionar capabilities com módulos?
- Quais capabilities fazem parte do MVP?
- Quais capabilities são futuras?

---

## Resultado da Sessão

A sessão ainda está em andamento.

---

## Checklist de Encerramento

- [ ] Todas as perguntas arquiteturais foram respondidas.
- [ ] A hipótese inicial foi confirmada ou descartada.
- [ ] As premissas foram validadas.
- [ ] Os riscos foram identificados.
- [ ] A decisão foi registrada.
- [ ] O ADR foi criado.
- [ ] A documentação oficial foi atualizada.
- [ ] O AI Context foi atualizado.
- [ ] Os diagramas foram revisados.
- [ ] O ARCHITECTURE_INDEX foi atualizado.
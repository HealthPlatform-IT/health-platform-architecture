---
title: Open Questions
status: Draft
version: 0.4.1
created: 2026-07-02
updated: 2026-07-03
author: Architecture Team
category: AI Context
phase: Product & Architecture Foundation
related:
  - ai-context/architecture-foundation.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md
  - workspace/AS-002/questions.md
---

# Open Questions

> Este documento centraliza as perguntas arquiteturais ainda em aberto na Health Platform. É a fonte única para saber o que ainda não foi decidido.

---

## 1. Purpose

Este arquivo consolida perguntas arquiteturais dispersas em ADRs, Architecture Sessions, workspace e documentação oficial.

Ele **não responde** às perguntas. Quando uma pergunta for resolvida, deve ser atualizada para `Answered` com referência ao documento ou ADR que a responde — e a resposta deve existir na documentação oficial, não apenas neste arquivo.

---

## 2. Question Status

| Status | Significado |
|---|---|
| **Open** | Pergunta registrada, sem análise ou decisão iniciada |
| **In Analysis** | Em discussão; pode haver direção preliminar, mas sem decisão formal |
| **Blocked** | Depende da resolução de outra pergunta |
| **Deferred** | Conscientemente adiada para fase futura |
| **Answered** | Respondida por documento oficial ou ADR Accepted |

**Critério para `Answered`:** deve existir ADR Accepted ou documento oficial que responda claramente. Direção preliminar em outro ADR **não** é suficiente.

---

## 3. Priority Levels

| Prioridade | Significado |
|---|---|
| **Critical** | Bloqueia decisões fundacionais ou próxima Architecture Session obrigatória |
| **High** | Impacto alto na arquitetura; resolver antes ou durante derivação de domínios |
| **Medium** | Importante, mas pode aguardar sessão específica ou dependência parcial |
| **Low** | Relevante, sem urgência na fase atual |
| **Future** | Pertence à fase de arquitetura técnica ou evolução futura do produto |

---

## 4. Critical Questions

| ID | Pergunta | Por que é crítica |
|---|---|---|
| Q-001 | Quando a instituição assume responsabilidade pelo cuidado? | ~~Bloqueia~~ Respondida — ADR-0007, AS-003 ✅ |
| Q-002 | Como agrupar sub-capabilities em Business Domains? | ~~Bloqueia~~ Respondida — ADR-0008, AS-004 ✅ |

---

## 5. Open Questions Catalog

### Q-001 — Quando a instituição assume responsabilidade pelo cuidado?

**Status:** Answered

**Priority:** Critical

**Question:**

Qual é o evento ou condição que marca o início de uma **Institution Care Journey** dentro da Health Platform?

**Answer:**

A Institution Care Journey inicia com um **Care Journey Start Event** — aceite explícito de responsabilidade por uma demanda de cuidado. Cadastro em Relacionar é pré-requisito, mas não inicia a jornada. O Core define tipos válidos; a instituição configura o gatilho por modelo operacional.

**Answered by:**

- `docs/05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md`
- `docs/02-business-processes/care-journey-lifecycle.md`
- `architecture-sessions/AS-003-care-journey-lifecycle.md`

**Source:**

- `workspace/AS-003/decisions.md`
- `ai-context/architecture-foundation.md` (seção 7)

---

### Q-002 — Como agrupar sub-capabilities em Business Domains?

**Status:** Answered

**Priority:** Critical

**Question:**

Como as 39 sub-capabilities do Business Capability Map devem ser agrupadas em Business Domains com fronteiras de responsabilidade claras?

**Answer:**

16 Business Domains ativos (5 Core + 8 Supporting + 2 Extension + 1 Cross-cutting), derivados do agrupamento de sub-capabilities relacionadas com PS transversais. Dois read models (Clinical Timeline, Analytics & Reporting). Modelo de decomposição C+E confirmado na AS-004.

**Answered by:**

- `docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md`
- `docs/04-domain/business-domains.md`
- `docs/04-domain/domain-map.md`
- `architecture-sessions/AS-004-business-domain-map.md`

**Source:**

- `workspace/AS-004/confirmation-package.md`
- `ai-context/architecture-foundation.md` (seção 17)

**Notes:**

Confirmado em 2026-07-02. Open Questions residuais (Q-005, Q-010, P-REG-*, etc.) não invalidam o catálogo.

---

### Q-003 — Qual será o Event Model e a tecnologia do Event Bus?

**Status:** Answered *(modelo conceitual — ADR-0012)*; **tecnologia/critérios — ADR-0017**; produto broker **Deferred** (PoC)

**Priority:** Medium *(produto broker restante)*

**Question:**

Qual será o Event Model da plataforma e qual tecnologia ou padrão o Event Bus utilizará?

**Answer (conceitual):**

Event Foundation (Core) = contrato; Event Bus = PS Confirmed; taxonomia em três camadas; domínio publica após persistir; bus só interna. Ver ADR-0012.

**Answer (tecnologia / critérios — AS-015):**

At-least-once + consumers idempotentes; ordering por stream lógico; retry+DLQ; Outbox→relay→broker; critérios de broker. Ver ADR-0017 e `event-strategy.md` §9.

**Deferred:** produto broker concreto (Kafka/Rabbit/cloud), tópicos físicos, schema registry obrigatório.

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md`
- `docs/05-architecture/adr/foundation/ADR-0017-event-bus-technical.md`
- `docs/05-architecture/event-strategy.md`
- `architecture-sessions/AS-015-event-bus-technical.md`

**Session:** AS-010 ✅ · AS-015 ✅

---

### Q-004 — Quais são os principais agregados do domínio clínico?

**Status:** Answered — ADR-0018

**Priority:** High

**Question:**

Como os conceitos Patient, Care Journey, Care Episode, Attendance, Clinical Event e Clinical Artifact devem ser refinados em agregados de domínio?

**Answer:**

Hierarquia ADR-0001 ≠ aggregates 1:1. Roots: Patient, CareJourney, CareEpisode, Attendance, ClinicalEntry, ClinicalOrder, ClinicalArtifact. Clinical Event (nível A) não é root e ≠ Domain Event. Refs por ID+tenant. Ver ADR-0018 e `clinical-aggregates.md`.

**Deferred:** DDL, subtypes ClinicalEntry, comandos detalhados.

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0018-clinical-aggregates.md`
- `docs/05-architecture/clinical-aggregates.md`
- `architecture-sessions/AS-016-clinical-aggregates.md`

**Session:** AS-016 ✅

---

### Q-005 — Onde posicionar capabilities administrativas e financeiras?

**Status:** Open

**Priority:** High

**Question:**

Onde posicionar capabilities administrativas e financeiras — como faturamento, estoque e gestão financeira — dentro das oito Core Business Capabilities?

**Context:**

As oito Core Business Capabilities não incluem explicitamente um domínio administrativo ou financeiro. O rascunho antigo do Business Capability Map (6 grupos) listava Administrative Capabilities, mas esse modelo foi substituído. Capacidades como convênios estão parcialmente em Relacionar; faturamento e estoque não têm posição consolidada.

**Architectural Impact:**

- Completude do Business Capability Map
- Business Domains administrativos/financeiros
- Escopo do produto e MVP

**Source:**

- `docs/03-capabilities/business-capability-map.md`
- `docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md`

**Recommended Session:** Future Product/Operational Session (parcialmente endereçado na AS-004 — Hospital Operations e Billing deferred)

**Notes:**

AS-004 confirmou Hospital Operations e Billing & Financial como deferred (Q-005 parcial). Faturamento e estoque permanecem fora do catálogo de 16 domínios ativos.

---

### Q-006 — Quais sub-capabilities pertencem ao escopo inicial (MVP)?

**Status:** Open

**Priority:** Medium

**Question:**

Quais sub-capabilities do Business Capability Map pertencem ao escopo inicial da plataforma (MVP)?

**Context:**

O mapa documenta 39 sub-capabilities (35 ativas + 4 deferred/future) sem distinção de MVP. A definição de MVP impacta priorização de domínios, módulos e Platform Services a implementar primeiro.

**Architectural Impact:**

- Roadmap de produto
- Module Strategy
- Priorização de Platform Services
- Escopo da primeira release

**Source:**

- `docs/03-capabilities/business-capability-map.md`
- `docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md`
- `architecture-sessions/AS-002-business-capability-map.md` (questão: quais capabilities fazem parte do MVP)

**Recommended Session:** Future Product/Operational Session (após AS-005)

**Notes:**

Q-002 respondida. Depende parcialmente de Q-005 (escopo administrativo/financeiro) e AS-005 (Module Strategy).

---

### Q-007 — Qual a fronteira Core / Platform Services / Modules?

**Status:** Answered

**Priority:** High

**Question:**

Qual é a fronteira exata entre Core, Platform Services e Modules?

**Answer (AS-005 — 2026-07-03):**

| Camada | Papel |
|---|---|
| **Core Platform** | 8 contratos/invariantes estruturais — sem implementação operacional |
| **Platform Services** | 15 Confirmed + 2 Strong + 1 Needs Review — transversais por contrato |
| **Business Domains** | 16 domínios (ADR-0008) — regras e vocabulário |
| **Modules** | 15 módulos candidatos — cardinalidade flexível |
| **Extension** | M-14, M-15 + Extension Mechanism no Core |
| **Read Models** | Clinical Timeline, Analytics — projeções consumidas por módulos |

**Regra:** `Core define invariantes → PS operam → Domínios definem política → PS executam mecanismo`

**Context:**

ADR-0003 define proteção do Core. ADR-0005 cataloga PS. AS-004 definiu Domain/PS/View. AS-005 completou fronteira com Modules, Extension e Core Platform operacional.

**Architectural Impact:**

- Organização de código e repositórios
- Proteção do Core
- Consumo de PS por módulos
- Classification matrix (6 categorias)

**Source:**

- [ADR-0009](../docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md) — **Answered by**
- `docs/05-architecture/core-platform.md`
- `docs/05-architecture/module-strategy.md`
- `docs/05-architecture/extension-model.md`
- `docs/05-architecture/read-models.md`
- `docs/05-architecture/architecture-classification.md`
- ADR-0003, ADR-0005, ADR-0008

**Recommended Session:** ~~AS-005~~ Concluída

**Notes:**

Clinical Workspace = M-02 shell. Telemedicine = Operational Mode em Attendance. Communication sem módulo dedicado.

---

### Q-008 — Qual a estratégia multi-tenant?

**Status:** Answered — ADR-0013 (estratégia); persistência materializada em **ADR-0015**; vendor produto ainda Deferred

**Priority:** — *(estratégia fechada)*

**Question:**

Qual será a estratégia de multi-tenant da plataforma (isolamento, schema, banco de dados)?

**Answer (estratégia):**

Tenant = isolamento SaaS; Organization/Institution dentro do tenant (domínio); Runtime Context referencia organization (OQ-C01); isolamento shared + discriminator como padrão (silo = exceção); Identity estabelece sessão; tenant obrigatório em operações e Event Bus; Config/Flags/Modules por tenant. Ver ADR-0013 e `multi-tenant-strategy.md`.

**Deferred:** produto de banco concreto (critérios em ADR-0015); AuthZ detalhada (AS-009).

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0013-multi-tenant-strategy.md`
- `docs/05-architecture/multi-tenant-strategy.md`
- `architecture-sessions/AS-011-multi-tenant-strategy.md`

**Session:** AS-011 — Multi-Tenant Strategy ✅

---

### Q-009 — Quais mecanismos de extensão na implementação?

**Status:** Open

**Priority:** Future

**Question:**

Quais mecanismos de extensão serão suportados na implementação — plugins, feature flags, eventos, configuração dinâmica ou outros?

**Context:**

ADR-0003 lista cinco mecanismos de extensão conceituais: Modules, Platform Services, Configuration, Events e Integrations. Quais serão suportados na implementação e com qual maturidade ainda não está definido.

**Architectural Impact:**

- Extensibilidade da plataforma
- Feature Flag Service e Configuration Service
- Event Bus
- Modelo de plugins ou extensões por tenant

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md`

**Recommended Session:** Future Technical Architecture Sessions (após definição de domínios)

**Notes:**

Depende de Q-002 e Q-003 (eventos).

---

### Q-010 — Fronteira Communication Service vs Notification Service

**Status:** In Analysis

**Priority:** Medium

**Question:**

Qual é a fronteira definitiva entre Communication Service e Notification Service?

**Context:**

Ambos estão listados em `architecture-foundation.md`. ADR-0004 registrou a pergunta como pendente. ADR-0005 propõe fronteira provisória: Communication Service para canais externos (e-mail, WhatsApp, SMS, push); Notification Service para notificações in-app e alertas a usuários autenticados na plataforma. Essa distinção ainda não foi formalizada em ADR dedicado ou revisão do ADR-0004.

**Architectural Impact:**

- Platform Services de Comunicar
- Experiência do usuário (in-app vs. externo)
- Contratos de API para módulos consumidores

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md`
- `docs/05-architecture/adr/foundation/ADR-0005-platform-services.md`
- `ai-context/architecture-foundation.md` (seção 11)

**Recommended Session:** Future Technical Architecture ou revisão pontual de Platform Services

**Notes:**

Não marcar como Answered até formalização. Direção preliminar em ADR-0005.

---

### Q-011 — FHIR Messaging: Comunicar ou Integrar?

**Status:** Open

**Priority:** Medium

**Question:**

Como tratar comunicação clínica estruturada entre sistemas (ex.: FHIR Messaging) — pertence a Comunicar ou a Integrar?

**Context:**

ADR-0004 estabelece Comunicar para pessoas e Integrar para sistemas. FHIR Messaging pode envolver troca entre sistemas de saúde com conteúdo clínico, situando-se em zona limítrofe.

**Architectural Impact:**

- Fronteira Comunicar vs. Integrar
- Integration Service e Clinical Interoperability
- Contratos de interoperabilidade

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md`

**Recommended Session:** Future Technical Architecture — Integração clínica

**Notes:**

Direção provável: Integrar — sujeito a análise por caso de uso.

---

### Q-013 — Document Engine e Medical Form Engine — escopo e contratos

**Status:** Answered — ADR-0010 (Medical Form Engine) + ADR-0011 (Document Engine)

**Priority:** Medium

**Question:**

Qual será o escopo detalhado e os contratos dos Platform Services Document Engine e Medical Form Engine?

**Context:**

Ambos foram formalizados como **Platform Services Confirmed** — ADR-0010 (Medical Form Engine) e ADR-0011 (Document Engine).

**Architectural Impact:**

- Capability Registrar
- Geração de documentos clínicos e formulários dinâmicos
- Template Service e fronteiras com engines

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0005-platform-services.md`
- `ARCHITECTURE_INDEX.md`

**Answer (Medical Form Engine — AS-006 / ADR-0010):**

Medical Form Engine = **Confirmed** Platform Service. Modelo Form Definition / Instance / Clinical Content. Engine valida estrutura; domínio valida conteúdo. Consumidores: M-03, M-04, M-05, M-14, M-15.

**Answer (Document Engine — AS-007 / ADR-0011):**

Document Engine = **Confirmed** Platform Service. Modelo Document Template / Generation Instance / Clinical Artifact. Engine renderiza; Clinical Documents possui regras e artefato. Template Service independente. Consumidores: M-05, M-06.

**Source:**

- [ADR-0010](../docs/05-architecture/adr/foundation/ADR-0010-medical-form-engine.md)
- [ADR-0011](../docs/05-architecture/adr/foundation/ADR-0011-document-engine.md)
- `docs/05-architecture/medical-form-engine.md`
- `docs/05-architecture/document-engine.md`

**Recommended Session:** ~~AS-006~~ ~~AS-007~~ Concluídas

---

### Q-014 — Template Service: independente ou integrado?

**Status:** Answered — ADR-0011 D-005; Template Service independente; Engine e Communication consomem

**Priority:** Medium

**Question:**

O Template Service deve permanecer independente ou ser absorvido pelo Communication Service ou Document Engine?

**Context:**

Template Service está consolidado no ADR-0005 com responsabilidade sobre templates de mensagens e documentos. A relação com Communication Service (templates de mensagem) e Document Engine (templates de documentos clínicos) ainda não foi definida.

**Architectural Impact:**

- Arquitetura de Platform Services
- Reutilização de templates entre comunicação e documentos
- Contratos de consumo

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0005-platform-services.md`

**Recommended Session:** ~~AS-006~~ ~~AS-007~~ Concluídas — ADR-0011 D-005

**Notes:**

Depende de Q-013.

---

### Q-015 — Herança de configuração tenant → instituição → unidade

**Status:** Open

**Priority:** Medium

**Question:**

Como funciona a herança de configuração entre os níveis tenant, instituição e unidade?

**Context:**

ADR-0006 define níveis conceituais de configuração (tenant, instituição, unidade), mas a implementação de herança, sobrescrita e precedência entre níveis não foi definida.

**Architectural Impact:**

- Configuration Service
- Multi-tenant e hierarquia organizacional
- Comportamento configurável por escopo

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md`

**Recommended Session:** Future Product/Operational Session (após domínios)

**Notes:**

Relacionada a Q-008 (multi-tenant), mas pode ser tratada conceitualmente antes da implementação técnica.

---

### Q-016 — Processo de aprovação para customização excepcional

**Status:** Open

**Priority:** Medium

**Question:**

Qual será o processo formal de aprovação para customização excepcional por tenant?

**Context:**

ADR-0006 define customização por código como exceção, com requisitos de justificativa documentada, isolamento e revisão periódica. O processo formal de aprovação, governança e critérios de revisão não foi definido.

**Architectural Impact:**

- Governança de produto e arquitetura
- Modelo SaaS e manutenção de código por cliente
- Development Guidelines

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md`

**Recommended Session:** Future Product/Operational Session

**Notes:**

Pode ser documentado em Development Guidelines quando criado.

---

### Q-017 — Schema ou modelo de dados de configuração

**Status:** Deferred

**Priority:** Future

**Question:**

Qual será o schema ou modelo de dados de configuração da plataforma?

**Context:**

Configuration Service está consolidado conceitualmente (ADR-0005), e o princípio de configuração está formalizado (ADR-0006). O modelo de dados, tipos de configuração e schema ainda não existem.

**Architectural Impact:**

- Configuration Service
- Banco de dados
- APIs de configuração
- Hierarquia tenant → instituição → unidade (Q-015)

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md`

**Recommended Session:** Future Technical Architecture Sessions (após definição de domínios)

**Notes:**

Depende de Q-002 e parcialmente de Q-015.

---

### Q-018 — Múltiplas Institution Care Journeys ativas simultâneas?

**Status:** Open

**Priority:** Medium

**Question:**

Um paciente na mesma instituição pode ter mais de uma Institution Care Journey **Active** simultaneamente (ex.: programa ocupacional + acompanhamento ambulatorial)?

**Context:**

ADR-0007 define uma jornada Active por paciente e instituição por vez. Cenários com programas paralelos podem exigir exceção ou modelo de episódios paralelos dentro de uma única jornada.

**Architectural Impact:**

- Modelo de domínio clínico
- Cardinalidade de Care Journey
- Business Domains (AS-004)

**Source:**

- `workspace/AS-003/decisions.md` — Decisão 007
- `docs/05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md`

**Recommended Session:** AS-005 — Business Domain Map (parcial) ou sessão dedicada

**Notes:**

Emergente do encerramento da AS-003. ADR-0007 mantém uma jornada Active por paciente/instituição; cenários paralelos permanecem em análise.

---

### Q-019 — Compliance Service no catálogo ADR-0005?

**Status:** Open

**Priority:** Medium

**Question:**

O **Compliance Service** deve ser adicionado ao catálogo oficial de Platform Services (ADR-0005) como mecanismo de enforcement técnico de políticas definidas pelo domínio Governance & Compliance?

**Context:**

Durante a AS-004 (NR-009), surgiu a hipótese de um Compliance Service para enforcement técnico de políticas de LGPD, retenção e conformidade — com o domínio Governance & Compliance definindo as políticas. O serviço não está no catálogo ADR-0005.

**Architectural Impact:**

- Platform Services de Governar
- Governance & Compliance Domain
- Audit Service, Configuration Service (consumidores relacionados)

**Source:**

- `workspace/AS-004/decisions.md` — NR-009
- `workspace/AS-004/confirmation-package.md`
- `docs/04-domain/domain-map.md`

**Recommended Session:** Future Technical Architecture ou revisão pontual de Platform Services

**Notes:**

Hipótese da AS-004 — não bloqueia o catálogo de 16 domínios. Promovida de workspace para catálogo central em 2026-07-03.

---

### Q-020 — Research Consent — domínio futuro?

**Status:** Deferred

**Priority:** Low

**Question:**

Como tratar **Research Consent** (consentimento para pesquisa clínica) — domínio próprio, extensão de Clinical Documents ou capability futura?

**Context:**

A AS-004 classificou quatro tipos de consentimento: Communication, Data Processing/Privacy, Clinical e Research. Research Consent foi explicitamente deferred para fase futura.

**Architectural Impact:**

- Governance & Compliance Domain
- Clinical Documents Domain (possível extensão)
- Escopo de produto em pesquisa clínica

**Source:**

- `workspace/AS-004/decisions.md` — tipos de consentimento
- `workspace/AS-004/confirmation-package.md`

**Recommended Session:** Future Product/Operational Session

**Notes:**

Deferred conscientemente na AS-004. Não bloqueia MVP nem catálogo de domínios.

---

## 6. Questions by Recommended Session

### AS-003 — Care Journey Lifecycle

| ID | Pergunta | Status |
|---|---|---|
| Q-001 | Quando a instituição assume responsabilidade pelo cuidado? | ✅ Answered |

### AS-004 — Business Domain Map

| ID | Pergunta | Status |
|---|---|---|
| Q-002 | Como agrupar sub-capabilities em Business Domains? | ✅ Answered |
| Q-004 | Principais agregados do domínio clínico (parcial) | Open |
| Q-005 | Capabilities administrativas e financeiras (parcial — deferred) | Open |
| Q-007 | Fronteira Core / Platform Services / Modules | Answered |
| Q-018 | Múltiplas jornadas ativas simultâneas (parcial) | Open |
| Q-019 | Compliance Service no catálogo ADR-0005 | Open |
| Q-020 | Research Consent — domínio futuro | Deferred |

### AS-006 — Medical Form Engine

**Status:** ✅ Concluída (2026-07-03) — [`workspace/AS-006/`](../workspace/AS-006/README.md)

| ID | Pergunta | Status |
|---|---|---|
| Q-013 (MFE) | Medical Form Engine | **Answered** — ADR-0010 |
| OQ-PS05 | PS ou parte de Clinical Record? | **Answered** — PS dedicado |
| Q-014 | Template Service | **Answered** — ADR-0011 (formalizado em AS-007) |

### AS-007 — Document Engine

**Status:** ✅ Concluída (2026-07-03) — [`workspace/AS-007/`](../workspace/AS-007/README.md)

| ID | Pergunta | Status |
|---|---|---|
| Q-013 (DE) | Document Engine | **Answered** — ADR-0011 |
| Q-014 | Template Service | **Answered** — ADR-0011 |
| OQ-PS06 | PS ou parte de Clinical Documents? | **Answered** |

### AS-010 — Event Strategy

**Status:** ✅ Concluída (2026-07-03) — [`workspace/AS-010/`](../workspace/AS-010/README.md)

| ID | Pergunta | Status |
|---|---|---|
| Q-003 | Event Model e Event Bus | **Answered** — ADR-0012 + ADR-0017; produto broker Deferred |
| OQ-EV01 | Catálogo de eventos | **Answered** — domínio publicador |
| OQ-EV02 | Tier Event Bus | **Answered** — Confirmed (15º PS) |

### AS-011 — Multi-Tenant Strategy

**Status:** ✅ Concluída (2026-07-14) — [`workspace/AS-011/`](../workspace/AS-011/README.md)

| ID | Pergunta | Status |
|---|---|---|
| Q-008 | Estratégia multi-tenant | **Answered** — ADR-0013 + ADR-0015 (persistência); vendor Deferred |
| OQ-C01 | Organization Context | **Answered** — referência no Runtime Context |
| OQ-MT01 | Modelo de isolamento | **Answered** — shared + discriminator |

### Future Technical Architecture Sessions

| ID | Pergunta |
|---|---|
| Q-003 *(produto broker)* | PoC / escolha concreta Kafka|Rabbit|cloud |
| Q-009 | Mecanismos de extensão na implementação |
| Q-011 | FHIR Messaging |
| Q-017 | Schema de configuração |

### Future Product / Operational Sessions

| ID | Pergunta |
|---|---|
| Q-005 | Capabilities administrativas e financeiras (parcial) |
| Q-006 | Escopo MVP por sub-capability |
| Q-015 | Herança de configuração |
| Q-016 | Processo de aprovação de customização |

---

## 7. Questions Not to Resolve Yet

Perguntas importantes que **não devem ser resolvidas na fase atual** de Product & Architecture Foundation:

| ID | Motivo para adiar |
|---|---|
| Q-003 *(produto broker)* | Critérios Answered ADR-0017; produto via PoC |
| Q-009 | Mecanismos de extensão — após domínios |
| Q-017 | Schema de configuração — após domínios e fase técnica |
| Q-006 | MVP — decisão de produto após AS-005 |
| Q-020 | Research Consent — fase futura |

Perguntas que **não devem ser resolvidas antes de Q-002**:

| ID | Motivo |
|---|---|
| ~~Q-004~~ | Agregados — **desbloqueado**; pode avançar pós AS-004 |

---

## 8. Maintenance Rules

1. **Toda nova Open Question** surgida em ADR, Architecture Session, workspace ou revisão de documentação deve ser adicionada neste arquivo com ID sequencial (próximo: Q-021).
2. **Não duplicar** — verificar se a pergunta já existe antes de criar novo ID.
3. **Atualizar status** quando uma Architecture Session iniciar (`In Analysis`) ou quando documento oficial/ADR responder (`Answered` + referência).
4. **Não responder aqui** — respostas vivem em documentação oficial ou ADRs; este arquivo apenas rastreia estado.
5. **Revisar prioridades** ao final de cada Architecture Session.
6. **Manter rastreabilidade** — campo `Source` sempre preenchido com caminho do arquivo fonte.

---

## Summary

| Métrica | Valor |
|---|---|
| Total de perguntas centralizadas | **19** (Q-001 a Q-011, Q-013 a Q-020 — Q-012 inexistente) |
| Critical | **0** |
| High | **1** (Q-005) |
| Medium | **7** (Q-006, Q-010, Q-011, Q-015, Q-016, Q-018, Q-019) |
| Future (prioridade) | **2** (Q-009, Q-017) |
| Status Deferred | **2** (Q-017, Q-020) |
| Status In Analysis | **1** (Q-010) |
| Status Partial | **0** |
| Status Open | **7** |
| Answered | **8** (Q-001, Q-002, Q-003, Q-004, Q-007, Q-008, Q-013, Q-014) |

**Próximo marco:** Sprint 3 — **AS-009 Security** · Q-006 (AS-016 ✅ ADR-0018).

**Podem ficar para fase técnica:** produto broker (PoC), Q-009, Q-011, Q-017.

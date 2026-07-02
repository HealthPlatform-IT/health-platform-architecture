---
title: Open Questions
status: Draft
version: 0.1.0
created: 2026-07-02
updated: 2026-07-02
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
| Q-001 | Quando a instituição assume responsabilidade pelo cuidado? | Bloqueia documentação oficial da Jornada de Cuidado e AS-003 |
| Q-002 | Como agrupar sub-capabilities em Business Domains? | Bloqueia AS-004 e pergunta 9 do critério de fundação |

---

## 5. Open Questions Catalog

### Q-001 — Quando a instituição assume responsabilidade pelo cuidado?

**Status:** Open

**Priority:** Critical

**Question:**

Qual é o evento ou condição que marca o início de uma **Institution Care Journey** dentro da Health Platform?

**Context:**

A plataforma é orientada pela Jornada de Cuidado, mas a jornada não nasce necessariamente dentro da plataforma. Ela passa a ser representada quando uma instituição assume responsabilidade por um trecho do cuidado. Diferentes modelos operacionais podem assumir essa responsabilidade em momentos distintos.

Gatilhos candidatos identificados nas fontes:

- Agendamento de consulta
- Encaminhamento de outro profissional
- Admissão em uma unidade
- Solicitação de exame
- Programa de acompanhamento
- Atendimento de urgência

**Architectural Impact:**

- Modelo de domínio clínico
- Institution Care Journey e Care Episode
- Clinical Workspace
- Timeline clínica
- Modelagem de dados, APIs e eventos

**Source:**

- `ai-context/architecture-foundation.md` (seção 17)
- `workspace/AS-002/questions.md`

**Recommended Session:** AS-003 — Care Journey Lifecycle

**Notes:**

Questão em análise no workspace. Deve ser resolvida antes de derivar Business Domains.

---

### Q-002 — Como agrupar sub-capabilities em Business Domains?

**Status:** Open

**Priority:** Critical

**Question:**

Como as 35 sub-capabilities do Business Capability Map devem ser agrupadas em Business Domains com fronteiras de responsabilidade claras?

**Context:**

As oito Core Business Capabilities e o Business Capability Map estão consolidados. A próxima etapa é derivar domínios de negócio que agrupem sub-capabilities relacionadas, sem confundir domínio com módulo ou Platform Service.

**Architectural Impact:**

- Business Domains e Bounded Contexts
- Module Strategy
- APIs e fronteiras de responsabilidade
- Organização de repositórios e equipes

**Source:**

- `ai-context/architecture-foundation.md` (seção 17)
- `docs/03-capabilities/core-business-capabilities.md`
- `docs/03-capabilities/business-capability-map.md`
- `docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md`
- `architecture-sessions/AS-002-business-capability-map.md` (questão: como relacionar capabilities com domínios)

**Recommended Session:** AS-004 — Business Domain Map

**Notes:**

Blocked parcialmente por Q-001. Não iniciar AS-004 antes de Q-001 estar suficientemente respondida.

---

### Q-003 — Qual será o Event Model e a tecnologia do Event Bus?

**Status:** Deferred

**Priority:** Future

**Question:**

Qual será o Event Model da plataforma e qual tecnologia ou padrão o Event Bus utilizará?

**Context:**

A plataforma provavelmente será orientada por eventos. É necessário definir quais eventos existem, quem os publica, quem os consome e como impactam auditoria, comunicação, integração, monitoramento e analytics. O Event Bus está identificado no catálogo de Platform Services (ADR-0005), mas sem decisão de implementação.

**Architectural Impact:**

- Event Bus (Platform Service)
- Comunicação desacoplada entre módulos
- Auditoria, integração, monitoramento e analytics
- Arquitetura técnica backend

**Source:**

- `ai-context/architecture-foundation.md` (seção 17)
- `docs/05-architecture/adr/foundation/ADR-0005-platform-services.md`

**Recommended Session:** Future Technical Architecture — Event Strategy (AS-010 planejada em `ARCHITECTURE_INDEX.md`)

**Notes:**

Não resolver na fase Product & Architecture Foundation atual.

---

### Q-004 — Quais são os principais agregados do domínio clínico?

**Status:** Open

**Priority:** High

**Question:**

Como os conceitos Patient, Care Journey, Care Episode, Attendance, Clinical Event e Clinical Artifact devem ser refinados em agregados de domínio?

**Context:**

O Modelo Hierárquico do Cuidado está consolidado no ADR-0001 como estrutura conceitual de seis níveis. A tradução em agregados de domínio (DDD) ainda não foi feita.

**Architectural Impact:**

- Modelagem de domínio clínico
- Bounded Contexts
- Modelagem de banco de dados
- APIs e invariantes de domínio

**Source:**

- `ai-context/architecture-foundation.md` (seção 17)

**Recommended Session:** AS-004 — Business Domain Map (ou sessão dedicada após Q-001 e Q-002)

**Notes:**

Depende de Q-001 (início da jornada) e Q-002 (fronteiras de domínio).

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

**Recommended Session:** AS-004 — Business Domain Map (parcial) ou Future Product/Operational Session

**Notes:**

Pode exigir extensão do mapa de capabilities ou confirmação de que ficam fora do escopo inicial.

---

### Q-006 — Quais sub-capabilities pertencem ao escopo inicial (MVP)?

**Status:** Open

**Priority:** Medium

**Question:**

Quais sub-capabilities do Business Capability Map pertencem ao escopo inicial da plataforma (MVP)?

**Context:**

O mapa documenta 35 sub-capabilities sem distinção de fase. A definição de MVP impacta priorização de domínios, módulos e Platform Services a implementar primeiro.

**Architectural Impact:**

- Roadmap de produto
- Module Strategy
- Priorização de Platform Services
- Escopo da primeira release

**Source:**

- `docs/03-capabilities/business-capability-map.md`
- `docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md`
- `architecture-sessions/AS-002-business-capability-map.md` (questão: quais capabilities fazem parte do MVP)

**Recommended Session:** Future Product/Operational Session (após AS-004)

**Notes:**

Depende de Q-002 (domínios) e parcialmente de Q-005 (escopo administrativo/financeiro).

---

### Q-007 — Qual a fronteira Core / Platform Services / Modules?

**Status:** Open

**Priority:** High

**Question:**

Qual é a fronteira exata entre Core, Platform Services e Modules?

**Context:**

ADR-0003 define conceitualmente o que pertence e não pertence ao Core. ADR-0005 cataloga 17 Platform Services. A fronteira operacional entre os três ainda será refinada com a definição de Business Domains e módulos.

**Architectural Impact:**

- Organização de código e repositórios
- Responsabilidades de implementação
- Proteção do Core
- Consumo de Platform Services por módulos

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md`
- `docs/05-architecture/adr/foundation/ADR-0005-platform-services.md`

**Recommended Session:** AS-004 — Business Domain Map (parcial); sessão futura de Core Platform

**Notes:**

`ARCHITECTURE_INDEX.md` planeja AS-004 — Core Platform para tema relacionado.

---

### Q-008 — Qual a estratégia multi-tenant?

**Status:** Deferred

**Priority:** Future

**Question:**

Qual será a estratégia de multi-tenant da plataforma (isolamento, schema, banco de dados)?

**Context:**

A Health Platform é concebida como SaaS multi-tenant. Identity Service gerencia contexto multi-tenant (ADR-0005), mas a estratégia técnica de isolamento de dados não foi definida.

**Architectural Impact:**

- Arquitetura de banco de dados
- Identity Service
- Configuration Service
- Segurança e isolamento entre tenants

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md`
- `docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md`

**Recommended Session:** Future Technical Architecture Sessions

**Notes:**

Pertence à fase de arquitetura técnica (Sprint 3 no `ARCHITECTURE_INDEX.md`).

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

**Status:** Open

**Priority:** Medium

**Question:**

Qual será o escopo detalhado e os contratos dos Platform Services Document Engine e Medical Form Engine?

**Context:**

Ambos estão no catálogo como **Identificados** (ADR-0005), com decisão detalhada pendente. `ARCHITECTURE_INDEX.md` planeja AS-006 (Medical Form Engine) e AS-007 (Document Engine).

**Architectural Impact:**

- Capability Registrar
- Geração de documentos clínicos e formulários dinâmicos
- Template Service e fronteiras com engines

**Source:**

- `docs/05-architecture/adr/foundation/ADR-0005-platform-services.md`
- `ARCHITECTURE_INDEX.md`

**Recommended Session:** AS-006 — Medical Form Engine; AS-007 — Document Engine

**Notes:**

Não implementar antes das sessões dedicadas.

---

### Q-014 — Template Service: independente ou integrado?

**Status:** Open

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

**Recommended Session:** AS-006 / AS-007 (revisar após engines)

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

## 6. Questions by Recommended Session

### AS-003 — Care Journey Lifecycle

| ID | Pergunta |
|---|---|
| Q-001 | Quando a instituição assume responsabilidade pelo cuidado? |

### AS-004 — Business Domain Map

| ID | Pergunta |
|---|---|
| Q-002 | Como agrupar sub-capabilities em Business Domains? |
| Q-004 | Principais agregados do domínio clínico (parcial) |
| Q-005 | Capabilities administrativas e financeiras (parcial) |
| Q-007 | Fronteira Core / Platform Services / Modules (parcial) |

### AS-006 — Medical Form Engine

| ID | Pergunta |
|---|---|
| Q-013 | Document Engine e Medical Form Engine (Medical Form Engine) |
| Q-014 | Template Service (parcial) |

### AS-007 — Document Engine

| ID | Pergunta |
|---|---|
| Q-013 | Document Engine |
| Q-014 | Template Service (parcial) |

### Future Technical Architecture Sessions

| ID | Pergunta |
|---|---|
| Q-003 | Event Model e Event Bus |
| Q-008 | Estratégia multi-tenant |
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
| Q-003 | Event Model — fase técnica (AS-010) |
| Q-008 | Multi-tenant — fase técnica |
| Q-009 | Mecanismos de extensão — após domínios |
| Q-017 | Schema de configuração — após domínios e fase técnica |
| Q-013 | Engines — aguardar AS-006 e AS-007 |
| Q-006 | MVP — decisão de produto após domínios |

Perguntas que **não devem ser resolvidas antes de Q-001**:

| ID | Motivo |
|---|---|
| Q-002 | Domínios dependem do modelo de jornada institucional |
| Q-004 | Agregados dependem de fronteiras de domínio e início da jornada |

---

## 8. Maintenance Rules

1. **Toda nova Open Question** surgida em ADR, Architecture Session, workspace ou revisão de documentação deve ser adicionada neste arquivo com ID sequencial (próximo: Q-018).
2. **Não duplicar** — verificar se a pergunta já existe antes de criar novo ID.
3. **Atualizar status** quando uma Architecture Session iniciar (`In Analysis`) ou quando documento oficial/ADR responder (`Answered` + referência).
4. **Não responder aqui** — respostas vivem em documentação oficial ou ADRs; este arquivo apenas rastreia estado.
5. **Revisar prioridades** ao final de cada Architecture Session.
6. **Manter rastreabilidade** — campo `Source` sempre preenchido com caminho do arquivo fonte.

---

## Summary

| Métrica | Valor |
|---|---|
| Total de perguntas centralizadas | **16** (Q-001 a Q-011, Q-013 a Q-017 — Q-012 inexistente nas fontes) |
| Critical | **2** (Q-001, Q-002) |
| High | **3** (Q-004, Q-005, Q-007) |
| Medium | **7** (Q-006, Q-010, Q-011, Q-013, Q-014, Q-015, Q-016) |
| Future (prioridade) | **4** (Q-003, Q-008, Q-009, Q-017) |
| Status Deferred | **3** (Q-003, Q-008, Q-017) |
| Status In Analysis | **1** (Q-010) |
| Status Open | **12** |
| Answered | **0** |

**Resolver antes de Business Domains:** Q-001 (obrigatório), Q-002 (objeto da AS-004, blocked por Q-001).

**Podem ficar para fase técnica:** Q-003, Q-008, Q-009, Q-011, Q-017.

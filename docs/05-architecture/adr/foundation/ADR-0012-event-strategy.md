---
title: ADR-0012 - Event Strategy
status: Accepted
date: 2026-07-03
deciders:
  - Architecture Team
tags:
  - architecture
  - event-strategy
  - event-bus
  - event-foundation
  - platform-services
  - foundation
related:
  - architecture-sessions/AS-010-event-strategy.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - workspace/AS-010/confirmation-package.md
---

# ADR-0012 — Event Strategy

## Status

Accepted

---

# Contexto

A **AS-005** (ADR-0009) definiu **Event Foundation** como 6º componente do Core Platform — contrato limitado, **sem catálogo de eventos no Core** (I-07).

O **ADR-0005** catalogou **Event Bus** como Platform Service **Identificado** (Strong Candidate). **Q-003** permanecia aberta: Event Model conceitual e papel do Bus.

A **AS-010** investigou NR-001 a NR-006 e confirmou o pacote D-001 a D-008 em 2026-07-03.

---

# Problema

Sem estratégia de eventos:

- Risco de colocar catálogo de tipos no Core (viola I-07).
- Risco de tratar Event Bus como Core operacional.
- Homonímia entre Clinical Event (modelo hierárquico), Domain Event e mensagem na bus.
- Confusão entre mensageria interna e Integration/Webhook (ADR-0004).
- Read Models e Audit sem padrão de consumo.

---

# Decisão

## 1. Classificação Event Bus (D-001)

**Event Bus** é **Platform Service Confirmed** (15º PS). Implementa o contrato **Event Foundation** do Core.

**OQ-EV02 Answered.**

## 2. Event Foundation vs Event Bus (D-002)

| Camada | Papel |
|---|---|
| **Event Foundation (Core)** | Contrato: envelope, contexto tenant, regras de publicação/consumo — **sem catálogo de tipos** (I-07) |
| **Event Bus (PS)** | Transporte: publicação, assinatura, roteamento, entrega, retry técnico |

## 3. Taxonomia em três camadas (D-003)

| Camada | Termo | Exemplo | Owner |
|---|---|---|---|
| **A — Hierárquico / ciclo de vida** | Clinical Event, Care Journey Start Event | Consulta; início de jornada | Modelo clínico (ADR-0001, ADR-0007) |
| **B — Domain Event** | Evento de domínio | `Attendance.Started`, `ClinicalArtifact.Formalized` | Business Domain publicador |
| **C — Platform Event Message** | Mensagem na bus | Envelope transportado | Event Bus (mecanismo) |

Camada A **não é** mensagem na bus por si só — o domínio pode emitir Domain Event (B) quando A ocorre.

## 4. Ownership de tipos (D-004)

Tipos e payload semântico definidos pelo **domínio publicador**. Catálogo **não** no Core. Registry centralizado de tipos — fora de escopo deste ADR.

**OQ-EV01 Answered.**

## 5. Padrão publicador/consumidor (D-005)

```text
Módulo → domínio valida + persiste → domínio publica → Event Bus → assinantes
```

Módulos **não** publicam eventos de negócio diretamente. Read Models **não** publicam.

## 6. Fronteira Integration / Webhook / Communication (D-006)

Event Bus = **apenas interno**. Integration e Webhook = Integrar (ADR-0004), acionados por bridge/handler. Communication e Notification = Comunicar — **não** na bus.

## 7. Audit e Read Models (D-007)

| Destino | Papel |
|---|---|
| **Clinical Timeline, Analytics** | Assinantes — atualizam projeção |
| **Audit Service** | Assinante de eventos + API síncrona para acesso/auth |
| **Search Service** | Assinante candidato — indexação |

## 8. Tecnologia deferred (D-008)

Broker (Kafka, RabbitMQ, etc.), serialização, tópicos físicos e ordering implementation — **Sprint 3**.

**Q-003:** Answered (modelo conceitual) + tecnologia **Deferred** Sprint 3.

## 9. O que pertence / não pertence ao Event Bus

**Pertence:** publicação/assinatura/entrega assíncrona interna; roteamento técnico; implementação do contrato Event Foundation.

**Não pertence:** significado clínico do payload; catálogo de tipos no Core; webhooks externos; integração FHIR/HL7; envio a pessoas (Communication); orquestração de regra de negócio.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Event Bus no Core | Core só contratos (ADR-0009) |
| Catálogo de eventos no Core | Viola I-07 |
| Integration Service como broker interno | ADR-0004 — Integrar ≠ mensageria interna |
| Módulo publica eventos de negócio | Domínio é owner semântico |

---

# Consequências

## Positivas

- **Q-003 Answered** (parte conceitual).
- Event Bus **Confirmed** — 15º PS.
- Taxonomia evita homonímia clínico vs plataforma.
- Fronteiras ADR-0004 preservadas.
- Read Models e Audit com padrão de consumo.

## Negativas / deferred

- Tecnologia de broker — Sprint 3.
- Registry centralizado de tipos — sessão futura se necessário.
- Q-010 (Communication vs Notification) permanece In Analysis.
- Q-011 (FHIR Messaging) permanece Open.

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| ADR-0003 | Events como mecanismo de extensão |
| ADR-0004 | Integrar ≠ Comunicar; bus ≠ webhook |
| ADR-0005 | Catálogo PS — Event Bus promovido |
| ADR-0009 | Event Foundation (Core); I-07 |

---

# Documentação oficial

- `docs/05-architecture/event-strategy.md`
- `docs/05-architecture/platform-services.md` (v0.5.0)
- `docs/05-architecture/core-platform.md`
- `docs/05-architecture/read-models.md`

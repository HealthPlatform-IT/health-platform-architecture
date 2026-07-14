---
title: Event Strategy
status: Draft
version: 0.1.0
created: 2026-07-03
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0017-event-bus-technical.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md
  - architecture-sessions/AS-010-event-strategy.md
  - architecture-sessions/AS-015-event-bus-technical.md
---

# Event Strategy

> Estratégia de eventos da Health Platform — **Event Foundation** (Core) + **Event Bus** (PS) + taxonomia + semântica técnica.

Decisão formal: [ADR-0012](adr/foundation/ADR-0012-event-strategy.md) (conceitual) · [ADR-0017](adr/foundation/ADR-0017-event-bus-technical.md) (técnica) · AS-010 / AS-015.

---

## 1. Purpose

Definir como módulos e domínios se comunicam de forma **assíncrona e desacoplada** — sem acoplar semântica de negócio ao transporte.

**Pergunta respondida (Q-003 — conceitual):** ADR-0012.

**Pergunta respondida (Q-003 — tecnologia / critérios):** ADR-0017 (produto broker ainda Deferred / PoC).

---

## 2. Camadas

| Camada | Natureza | Papel |
|---|---|---|
| **Event Foundation (Core)** | Contrato / invariante | Envelope, contexto tenant, regras de publicação — **sem catálogo** (I-07) |
| **Event Bus (PS Confirmed)** | Implementação / mecanismo | Pub/sub, roteamento, entrega, retry técnico |
| **Business Domain** | Owner semântico | Define *o que* aconteceu e *quando* publicar |
| **Módulo** | Orquestração UX | Aciona domínio; **não** publica eventos de negócio |

```text
Domínio decide "o que aconteceu"
        ↓
Monta payload + tipo (vocabulário do domínio)
        ↓
Event Foundation valida envelope/contexto
        ↓
Event Bus entrega a assinantes
        ↓
Consumidores (módulos, Read Models, Audit) reagem
```

---

## 3. Taxonomia — três camadas de "evento"

| Camada | Termo | Exemplo | Owner |
|---|---|---|---|
| **A — Hierárquico / ciclo de vida** | Clinical Event, Care Journey Start Event | Consulta; início de jornada | ADR-0001, ADR-0007 |
| **B — Domain Event** | Evento de domínio | `Attendance.Started`, `Order.Prescribed` | Business Domain publicador |
| **C — Platform Event Message** | Mensagem na bus | Envelope transportado | Event Bus |

**Regra:** Camada A não é mensagem na bus por si só — o domínio emite Domain Event (B) quando A ocorre.

Convenção de nome (conceitual): `{AggregateOrConcept}.{PastTense}` — vocabulário do domínio publicador.

---

## 4. Padrão publicador / consumidor

```text
Módulo → domínio valida + persiste → domínio publica → Event Bus → assinantes
```

| Ator | Publica Domain Event? | Consome? |
|---|---|---|
| Business Domain | **Sim** (após persistir) | Opcional |
| Módulo | **Não** | Sim (UI / orquestração) |
| Read Model | **Não** | **Sim** (projeção) |
| Audit Service | **Não** | **Sim** (+ API síncrona) |
| Event Bus | Não (transporta) | Não (roteia) |

---

## 5. Event Bus — responsabilidades

### Pertence

- Publicação, assinatura e entrega assíncrona **interna**
- Roteamento técnico entre assinantes
- Retry / dead-letter *(conceitual)*
- Implementação do contrato Event Foundation

### Não pertence

- Significado clínico do payload
- Catálogo de tipos no Core
- Webhooks para sistemas externos
- Integração FHIR/HL7 (Integration Service)
- Envio a pessoas (Communication / Notification)
- Orquestração de regra de negócio

---

## 6. Fronteiras (ADR-0004)

| Mecanismo | Escopo | Exemplo |
|---|---|---|
| **Event Bus** | Processos **internos** | `Order.Prescribed` → Timeline |
| **Integration Service** | Sistemas externos | FHIR outbound |
| **Webhook Service** | Callbacks HTTP externos | Webhook de pagamento |
| **Communication / Notification** | Pessoas | SMS, e-mail, alerta in-app |

Bridge interno→externo: handler ou domínio consome Domain Event e **invoca** Integration/Webhook — a bus **não** entrega ao mundo externo.

---

## 7. Audit e Read Models

| Destino | Papel |
|---|---|
| **Clinical Timeline** | Assinante — projeção cronológica |
| **Analytics** | Assinante — projeção analítica |
| **Audit Service** | Assinante de eventos de negócio + API síncrona (acesso/auth) |
| **Search Service** | Assinante candidato — indexação |

Fluxo de projeção:

```text
Domínio persiste → Domain Event → Event Bus → Projeção atualiza → Módulo renderiza
```

---

## 8. Exemplos conceituais

| Situação | Domain Event (exemplo) | Assinantes típicos |
|---|---|---|
| Jornada iniciada | `CareJourney.Started` | Timeline, Audit, Care Monitoring |
| Evolução registrada | `ClinicalRecord.EvolutionRecorded` | Timeline, Search |
| Ordem prescrita | `Order.Prescribed` | Timeline, Documents, Audit |
| Documento formalizado | `ClinicalArtifact.Formalized` | Timeline, M-06 UI, Care Monitoring |

---

## 9. Semântica técnica (AS-015 / ADR-0017)

| Aspecto | Decisão |
|---|---|
| Entrega | **At-least-once**; consumers **idempotentes** |
| Ordering | Stream lógico `(tenant_id, aggregate_type, aggregate_id)` |
| Falhas | Retry + **DLQ** + alerta |
| Path | **Outbox → relay → broker** |
| Tenant | Envelope + roteamento; sem cross-tenant |
| Serialização | JSON/envelope; schema registry opcional |
| Broker | **Critérios** definidos; **produto Deferred** (PoC) |

Ver [ADR-0017](adr/foundation/ADR-0017-event-bus-technical.md).

---

## 10. Fora de escopo restante

- Escolha final de produto broker (Kafka, RabbitMQ, cloud queue, etc.)
- Tópicos físicos e particionamento detalhado
- Schema registry obrigatório
- Event Sourcing completo

---

## 11. Referências

| Documento | Uso |
|---|---|
| [ADR-0012](adr/foundation/ADR-0012-event-strategy.md) | Modelo conceitual |
| [ADR-0017](adr/foundation/ADR-0017-event-bus-technical.md) | Semântica técnica / critérios |
| [ADR-0015](adr/foundation/ADR-0015-database-architecture.md) | Outbox |
| [core-platform.md](core-platform.md) | Event Foundation |
| [platform-services.md](platform-services.md) | Event Bus Confirmed |
| [read-models.md](read-models.md) | Projeções assinantes |

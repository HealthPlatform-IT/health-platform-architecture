---
title: ADR-0017 - Event Bus Technical Strategy
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - event-bus
  - Q-003
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-015-event-bus-technical.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/database-architecture.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0015-database-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0014-backend-architecture.md
  - workspace/AS-015/confirmation-package.md
---

# ADR-0017 — Event Bus Technical Strategy

## Status

Accepted

---

# Contexto

O **ADR-0012** fechou o modelo conceitual do Event Bus (PS Confirmed, taxonomia, ownership). A tecnologia (broker, entrega, ordering) permanece Deferred. A **AS-015** confirmou D-001 a D-010 em 2026-07-14 (*"confirmo o pacote"*) **sem** reabrir ADR-0012.

---

# Problema

Sem semântica técnica:

- Incerteza sobre garantias de entrega e idempotência
- Ordering mal definido
- Escolha prematura de broker
- Desalinhamento Outbox ↔ bus

---

# Decisão

## 1. Definição (D-001)

**Event Bus Technical Strategy** = semânticas de runtime e critérios de broker — não catálogo de eventos nem Event Sourcing.

## 2. Entrega (D-002)

- Padrão: **at-least-once**
- Consumidores **idempotentes** (event id / business key)
- Exactly-once ponta a ponta **não** exigido do broker

## 3. Ordering (D-003)

Ordering por **stream lógico** `(tenant_id, aggregate_type, aggregate_id)`. Sem ordenação global.

## 4. Retry e DLQ (D-004)

Retry com backoff; após N falhas → **DLQ** + alerta; reprocessamento governado.

## 5. Broker (D-005)

**Critérios** obrigatórios (ack, consumer groups, particionamento/chave, retenção/DLQ, ops SaaS, alinhamento ao monólito). **Produto concreto Deferred** (PoC).

## 6. Path de publicação (D-006)

**Outbox → relay → broker** (alinha ADR-0015).

## 7. Tenant (D-007)

Tenant no envelope e no roteamento; **sem** entrega cross-tenant padrão.

## 8. Serialização (D-008)

JSON (ou equivalente) sobre envelope Event Foundation; schema registry opcional na implementação.

## 9. Limites (D-009)

Não reabre ADR-0012. Sem Event Sourcing completo. Bus só interna.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| At-most-once para Domain Events | Perda inaceitável |
| Ordenação global | Custo/complexidade |
| Escolher Kafka/Rabbit nesta ADR | Critérios primeiro (como ADR-0015) |
| DB da aplicação como bus principal | Fora do Outbox transitório |

---

# Consequências

## Positivas

- Q-003 **tecnologia** fechada no nível de critérios/semântica
- Base para implementação e PoC de broker
- Coerência com Outbox e Tenant

## Negativas / deferred

- Produto broker ainda aberto
- Tópicos físicos e schema registry na implementação

---

# Relação

| ADR | Relação |
|---|---|
| ADR-0012 | Conceitual — não reaberto |
| ADR-0015 | Outbox |
| ADR-0014 | Workers / relay no monólito |

---

# Referências

- `docs/05-architecture/event-strategy.md` (seção técnica)
- `architecture-sessions/AS-015-event-bus-technical.md`

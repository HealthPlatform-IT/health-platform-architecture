# Event Bus Technical — Boundary

> Rascunho AS-015 — NR-001. **Não é decisão** até confirmação.

---

## 1. Definição

**Event Bus Technical Strategy** = critérios e semânticas de runtime do Event Bus (entrega, retry, ordering, isolamento, serialização, seleção de broker) — **sem** reabrir taxonomia/ownership de ADR-0012 e **sem** código.

---

## 2. Inclui / não inclui

| Inclui | Não inclui |
|---|---|
| Critérios de broker | Escolha final Kafka/Rabbit/… sem PoC |
| At-least-once + idempotência | Event Sourcing completo |
| Ordering por stream lógico | Catálogo de tipos no Core |
| Retry / DLQ conceitual | Webhooks externos |
| Tenant no roteamento | Serialização binária obrigatória específica |
| Alinhamento Outbox | Implementação de filas |

---

## 3. Herança (não reabrir)

- Bus **só interna** (ADR-0012)
- Tipos no domínio publicador
- Tenant no envelope; sem cross-tenant padrão
- Publish após persist (Outbox ADR-0015)

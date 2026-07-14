# Delivery, Retry & Ordering

> Rascunho AS-015 — NR-002.

---

## 1. Entrega

| Garantia | Decisão candidata |
|---|---|
| **At-least-once** | **Padrão** da plataforma |
| Exactly-once ponta a ponta | Não exigido do broker; compensa com Outbox + idempotência do consumer |
| At-most-once | Rejeitado para Domain Events de negócio |

Consumidores **devem** ser idempotentes (dedupe por event id / business key).

---

## 2. Ordering

| Nível | Decisão candidata |
|---|---|
| Global total order | **Não** |
| Por tenant (todos eventos) | Não obrigatório |
| **Por stream lógico** `(tenant_id, aggregate_type, aggregate_id)` | **Sim** — melhor esforço / garantia do broker quando suportado |

UI/Read Models não assumem ordem global cross-aggregate.

---

## 3. Retry & DLQ

```text
Publish (Outbox relay) → Broker → Consumer
                              ↓ fail
                         retry (backoff)
                              ↓ N esgotado
                         DLQ + alerta Observability/Audit
```

- Retry automático limitado
- DLQ inspecionável por ops (tenant no registro)
- Reprocessamento governado — não silencioso

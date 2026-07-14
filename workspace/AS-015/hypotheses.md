# Hipóteses — AS-015

| ID | Hipótese | Status |
|---|---|---|
| H-01 | Entrega **at-least-once**; consumidores **idempotentes** | Candidata |
| H-02 | Ordering **por stream lógico** (aggregate id dentro do tenant) — não ordenação global | Candidata |
| H-03 | Retry com backoff + **DLQ** após N falhas | Candidata |
| H-04 | Broker produto = Deferred; critérios fecham Q-003 tech parcialmente | Candidata |
| H-05 | Outbox → relay → broker (ADR-0015) é o path padrão de publish | Candidata |
| H-06 | Tenant no envelope **e** em metadados/roteamento (sem cross-tenant) | Confirmada (ADR-0012/13) |
| H-07 | Payload JSON (ou equivalente) + contrato de envelope Event Foundation | Candidata |
| H-08 | Schema registry formal = Strong/Deferred na implementação | Candidata |
| H-09 | Competing consumers com consumer group por assinante lógico | Candidata |
| H-10 | Event Sourcing completo **fora** de escopo | Confirmada (cartão) |

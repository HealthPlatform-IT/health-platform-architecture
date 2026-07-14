# Tenant, Serialization & Outbox

> Rascunho AS-015 — NR-004.

---

## 1. Tenant

- Envelope Event Foundation com **tenant obrigatório**
- Relay Outbox e consumers rejeitam/missam sem tenant
- Sem entrega cross-tenant por padrão
- Metadados/headers de roteamento podem repetir tenant_id (defesa)

---

## 2. Serialização

| Aspecto | Orientação candidata |
|---|---|
| Formato inicial | JSON (ou equivalente textual) sobre envelope |
| Event id | UUID/ULID estável para idempotência |
| Evolução | Additive preferida; breaking = novo tipo/versão de evento de domínio |
| Schema registry | Opcional na implementação — Strong candidate |

---

## 3. Outbox ↔ Broker

```text
UoW domínio + outbox → commit → relay publica no broker → consumer
```

Relay é infraestrutura do Event Bus / platform-api workers (ADR-0014) — não regra de negócio.

---

## 4. Tópicos

Orientação: tópicos/streams **lógicos** por bounded context ou família de eventos — detalhe físico Deferred junto com produto.

Evitar um único tópico global sem chave de ordenação.

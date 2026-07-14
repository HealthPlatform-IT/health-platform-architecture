# Read Models, Outbox & Vendor Criteria

> Rascunho AS-013 — NR-004. Candidato a confirmação.

---

## 1. Read Models

| Orientação | Decisão candidata |
|---|---|
| Escopo | Sempre **dentro do tenant** (ADR-0013) |
| Store | **Separação lógica** (schema/tableset de projeção) — mesmo DB permitido no início |
| Deploy | Mesmo modular monolith / workers (ADR-0014) |
| Escrita | Só via projeção a partir de Domain Events — Read Model **não** publica Domain Events |

Timeline e Analytics: sem store cross-tenant compartilhado de leitura clínica.

---

## 2. Outbox / atomicidade

Alinhado ADR-0012 (publicar após persistir):

```text
UoW: Domain write + Outbox record (tenant) → commit → relay → Event Bus
```

- Outbox (ou padrão equivalente) **recomendado** no shared DB
- Relay/worker usa o mesmo Runtime Context / tenant do registro
- Detalhe de tabela outbox = implementação (não DDL de produto nesta sessão)

---

## 3. Critérios de vendor (sem escolha final)

Vendor/SGBD deve satisfazer **preferencialmente**:

1. Suporte a workloads transacionais (ACID adequado a monólito)
2. Isolamento forte o suficiente para outbox + domínio
3. Facilidade de discriminador / RLS (quando disponível)
4. Backup/restore e ops compatíveis com SaaS multi-tenant
5. Ecossistema alinhado à stack futura (Deferred AS-012 D-009)

**Não decidido nesta sessão:** produto concreto (Postgres, etc.).

---

## 4. Deferred explícito

| Tema | Destino |
|---|---|
| Migrations tool / CI | Implementação / DevOps |
| Pooling, replicas, sharding | DevOps / capacity |
| Schema Configuration Service | Q-017 |
| Policies RLS SQL finais | Implementação pós-vendor |
| Agregados clínicos DDL | Q-004 / domínio |

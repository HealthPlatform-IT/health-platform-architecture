# Database Architecture — Boundary

> Rascunho AS-013 — NR-001. **Não é decisão** até confirmação.

---

## 1. Definição candidata

**Database Architecture** = estratégia de **persistência e isolamento de dados** da Health Platform: modelo shared vs silo, ownership relacional vs blob, projeções/Read Models, e critérios de vendor — honrando ADR-0013 e ADR-0014 — **sem** DDL de produto nem migrations.

---

## 2. Inclui / não inclui

| Inclui | Não inclui |
|---|---|
| Shared + discriminator materializado | Migrations de domínio clínico |
| Regras silo (exceção) | Escolha final de vendor sem critérios |
| Fronteira Domain ↔ Storage/File | Schemas OpenAPI (API Strategy) |
| Orientação Read Model store | AuthZ policies (AS-009) |
| Outbox / atomicidade write+publish | Q-017 schema de configuração |
| Critérios de produto de banco | Pooling/HA detalhado de ops |

---

## 3. Herança (não reabrir)

- Tenant isolation A padrão / C exceção (ADR-0013)
- Todo registro de negócio ∈ um tenant
- Modular monolith; DB split ≠ automático (ADR-0014)
- Domain Event após persistir (ADR-0012)

---

## 4. Riscos

- Vazamento cross-tenant via query sem filtro
- Schema-por-tenant como “atalho” operacional caro
- Domínio gravando blobs fora do Storage PS
- Escolher Postgres/MySQL/Cosmos antes dos critérios

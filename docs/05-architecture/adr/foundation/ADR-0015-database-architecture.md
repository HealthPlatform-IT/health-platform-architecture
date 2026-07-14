---
title: ADR-0015 - Database Architecture
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - database
  - persistence
  - multi-tenant
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-013-database-architecture.md
  - docs/05-architecture/database-architecture.md
  - docs/05-architecture/multi-tenant-strategy.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0013-multi-tenant-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0014-backend-architecture.md
  - workspace/AS-013/confirmation-package.md
---

# ADR-0015 — Database Architecture

## Status

Accepted

---

# Contexto

O **ADR-0013** definiu isolamento **shared + discriminator** como padrão e silo como exceção, deixando DDL/vendor **Deferred**. O **ADR-0014** fixou modular monolith. A **AS-013** materializou a estratégia de persistência e confirmou o pacote D-001 a D-010 em 2026-07-14 (*"pacote confirmado"*).

---

# Problema

Sem Database Architecture:

- Risco de vazamento cross-tenant na persistência
- Schema-por-tenant ou silo prematuros
- Domínio misturado com blobs fora de Storage/File
- Publicação de eventos sem atomicidade com a escrita
- Escolha de vendor sem critérios

---

# Decisão

## 1. Definição (D-001)

**Database Architecture** = estratégia de persistência e isolamento de dados — sem migrations de produto nem schemas clínicos finais nesta decisão.

## 2. Modelo de isolamento (D-002 / D-003)

| Modelo | Papel |
|---|---|
| **Shared database + discriminador `tenant_id`** | **Padrão** |
| **Database / instância por tenant (silo)** | **Exceção governada** |
| **Schema por tenant** | **Não é padrão** |

## 3. Enforcement (D-004)

- Filtro/contexto de tenant na **aplicação** = obrigatório
- **RLS** (quando o SGBD permitir) = defesa em profundidade **recomendada** — não substitui Runtime Context

## 4. Ownership (D-005)

| Dado | Ownership |
|---|---|
| Estado estruturado de negócio | Business Domain |
| Blobs | Storage Service |
| Metadados de arquivo | File Service |
| Config tipada | Configuration Service (schema detalhado = Q-017 Deferred) |
| Trilha de auditoria | Audit Service |

## 5. Read Models (D-006)

Projeções **tenant-scoped**; store/schema lógico de projeção separado permitido no mesmo DB; Read Model não publica Domain Events.

## 6. Outbox (D-007)

Padrão **Outbox** (ou equivalente) no mesmo UoW da escrita de domínio → commit → relay → Event Bus (ADR-0012).

## 7. Topologia (D-008)

**Um database lógico principal** no modular monolith; splits adicionais só com critério (projeção pesada, silo C).

## 8. Vendor (D-009)

Critérios obrigatórios (ACID adequado, discriminator/RLS, ops SaaS, ecossistema alinhável à stack). **Produto concreto Deferred**.

## 9. Deferred (explícito)

Migrations tool, pooling/HA, policies RLS SQL finais, Q-017, DDL de agregados clínicos (Q-004).

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Schema-por-tenant como default | Custo ops; não necessário com discriminator |
| Silo como padrão | Contradiz ADR-0013 |
| Domínio gravar blobs diretamente | Viola Storage/File PS |
| Escolher vendor nesta ADR | Critérios primeiro |

---

# Consequências

## Positivas

- Q-008 DDL parcialmente fechado (estratégia de persistência)
- Base para implementação e testes multi-tenant
- Coerência write → event

## Negativas / deferred

- Vendor e migrations ainda abertos
- Detalhe SQL/RLS na implementação

---

# Relação com ADRs

| ADR | Relação |
|---|---|
| ADR-0012 | Outbox / publish após persistir |
| ADR-0013 | Shared + discriminator; silo exceção |
| ADR-0014 | Um DB lógico no monólito |

---

# Referências

- `docs/05-architecture/database-architecture.md`
- `architecture-sessions/AS-013-database-architecture.md`
- `workspace/AS-013/`

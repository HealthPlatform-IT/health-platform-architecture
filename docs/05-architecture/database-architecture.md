---
title: Database Architecture
status: Draft
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/multi-tenant-strategy.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0015-database-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0013-multi-tenant-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0014-backend-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - architecture-sessions/AS-013-database-architecture.md
---

# Database Architecture

> Estratégia de persistência e isolamento de dados da Health Platform.

Decisão formal: [ADR-0015](adr/foundation/ADR-0015-database-architecture.md) · AS-013 D-001 a D-010.

---

## 1. Purpose

Materializar o isolamento multi-tenant na persistência e definir ownership de dados — **sem** migrations de produto nem escolha final de vendor.

---

## 2. Isolamento

| Modelo | Uso |
|---|---|
| **Shared DB + `tenant_id`** | **Padrão** |
| **Silo (DB/instância por tenant)** | **Exceção governada** |
| **Schema por tenant** | **Não padrão** |

### Invariantes

- Toda tabela de negócio no shared inclui `tenant_id` (ou equivalente)
- Query/comando sem Runtime Context = inválido
- Índices relevantes orientados a incluir `tenant_id`
- Cross-tenant só superfície plataforma + Audit

### Enforcement

1. **Application filter** obrigatório
2. **RLS** recomendada como defesa em profundidade (quando disponível)

---

## 3. Ownership

| Tipo | Dono | Store |
|---|---|---|
| Estado estruturado | Business Domain | DB principal |
| Blob | Storage Service | Object storage (tenant) |
| Metadados de arquivo | File Service | Metadados + ref Storage |
| Configuração | Configuration Service | Q-017 Deferred para schema |
| Auditoria | Audit Service | Store do PS |

---

## 4. Read Models

- Projeção **dentro do tenant**
- Schema/tableset de projeção logicamente separado (mesmo DB permitido)
- Não publicam Domain Events

---

## 5. Outbox

```text
UoW: Domain write + Outbox (tenant) → commit → relay → Event Bus
```

Garante alinhamento ADR-0012 (publicar após persistir).

---

## 6. Topologia

- **Um database lógico principal** no modular monolith (ADR-0014)
- Databases adicionais só com critério (silo C, projeção pesada)

---

## 7. Critérios de vendor

Produto futuro deve preferir: ACID adequado; suporte a discriminator/RLS; ops SaaS (backup/restore); ecossistema alinhável à stack (Deferred).

**Produto concreto:** Deferred.

---

## 8. Deferred

| Tema | Destino |
|---|---|
| Migrations tool / CI | Implementação / DevOps |
| Pooling, replicas, sharding | DevOps |
| Policies RLS SQL finais | Implementação pós-vendor |
| Schema Configuration | Q-017 |
| DDL agregados clínicos | Q-004 / domínio |

---

## 9. Referências

| Documento | Uso |
|---|---|
| [ADR-0015](adr/foundation/ADR-0015-database-architecture.md) | Decisão formal |
| [multi-tenant-strategy.md](multi-tenant-strategy.md) | Isolamento SaaS |
| [backend-architecture.md](backend-architecture.md) | Modular monolith |
| [event-strategy.md](event-strategy.md) | Publish após persistir |
| [read-models.md](read-models.md) | Timeline / Analytics |

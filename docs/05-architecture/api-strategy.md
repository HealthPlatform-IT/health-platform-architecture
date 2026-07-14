---
title: API Strategy
status: Draft
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/multi-tenant-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0016-api-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0014-backend-architecture.md
  - architecture-sessions/AS-014-api-strategy.md
---

# API Strategy

> Estratégia de contratos síncronos da Health Platform.

Decisão formal: [ADR-0016](adr/foundation/ADR-0016-api-strategy.md) · AS-014 D-001 a D-010.

---

## 1. Purpose

Definir APIs síncronas compatíveis com Event Bus e Tenant Context — **sem** OpenAPI final.

---

## 2. Sync vs Domain Event

| Sync | Event |
|---|---|
| Resposta imediata | Fato persistido + reações |
| Module → Domain no request | Outbox → Event Bus |

Não usar a bus para fechar request HTTP do usuário nem para webhook externo.

---

## 3. Superfícies

| Superfície | Público |
|---|---|
| **Product API** | UI / módulos de produto |
| **Platform API** | Platform Services |
| **Integration API** | Sistemas externos (Integrar) |

---

## 4. Estilo e versionamento

- Product: HTTP **resource-oriented**
- Breaking change: path `/v{n}/...`
- Evolução additive preferida

---

## 5. Tenant, correlation e erros

- Runtime Context obrigatório (exceto paths públicos)
- Request-Id / Correlation-Id em todo pipeline
- Erro: código estável + mensagem segura + correlation id + status HTTP

---

## 6. Webhooks

Outbound → Integration / Webhook PS.

---

## 7. Deferred

OpenAPI · BFF · GraphQL · AuthZ fina (AS-009) · rate limits

---

## 8. Referências

[ADR-0016](adr/foundation/ADR-0016-api-strategy.md) · [event-strategy.md](event-strategy.md) · [backend-architecture.md](backend-architecture.md) · [multi-tenant-strategy.md](multi-tenant-strategy.md)

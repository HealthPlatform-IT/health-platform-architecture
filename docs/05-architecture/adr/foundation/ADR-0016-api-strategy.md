---
title: ADR-0016 - API Strategy
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - api
  - sync
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-014-api-strategy.md
  - docs/05-architecture/api-strategy.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/multi-tenant-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0013-multi-tenant-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0014-backend-architecture.md
  - workspace/AS-014/confirmation-package.md
---

# ADR-0016 — API Strategy

## Status

Accepted

---

# Contexto

A Health Platform precisa de contratos **síncronos** coerentes com Event Bus (ADR-0012), Tenant Context (ADR-0013) e Backend (ADR-0014). A **AS-014** confirmou o pacote D-001 a D-010 em 2026-07-14 (*"confirmo o pacote"*).

---

# Problema

Sem API Strategy:

- Confusão entre sync e Domain Events
- Superfícies Product / Integration misturadas
- Versionamento e tenant inconsistentes
- Webhooks acoplados à Product API

---

# Decisão

## 1. Definição (D-001)

**API Strategy** = contratos síncronos entre clientes, módulos, PS e integrações — estilo, sync vs eventos, versionamento conceitual e Tenant — **sem** OpenAPI final nesta decisão.

## 2. Sync vs Domain Event (D-002)

| Sync API | Domain Event |
|---|---|
| Resposta imediata (UI/comando/consulta) | Fato já persistido; reações desacopladas |
| Orquestração no request | Fan-out via Outbox → Event Bus |

Anti-padrões: evento no lugar de endpoint legítimo; await na bus para fechar HTTP; Module publicando Domain Event; bus para webhook externo.

## 3. Três superfícies (D-003)

| Superfície | Papel |
|---|---|
| **Product API** | Clientes de produto / módulos |
| **Platform API** | Contratos de Platform Services (HTTP ou in-proc) |
| **Integration API** | Sistemas externos (ADR-0004 Integrar) |

## 4. Estilo Product (D-004)

HTTP **resource-oriented** (REST-like) como padrão externo. RPC-only rejeitado como padrão.

## 5. Versionamento (D-005)

Versão major incompatível via path `/v{n}/...`. Evolução compatível preferida sem bump.

## 6. Tenant e Correlation (D-006)

Runtime Context obrigatório em Product/Platform (exceto paths públicos AS-009). Request-Id / Correlation-Id no pipeline e nas respostas.

## 7. Erros (D-007)

Contrato mínimo: código estável, mensagem segura, correlation id, HTTP status semântico. Formato JSON exato Deferred (OpenAPI).

## 8. Webhooks (D-008)

Outbound webhooks = **Integration / Webhook PS** — não misturar com Product API.

## 9. Deferred (D-009)

OpenAPI completo · BFF · GraphQL · AuthZ fina · rate limit policies.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Event-first para UI sync | Viola ADR-0012/0014 |
| Uma API única sem superfícies | Mistura Integrar e produto |
| Versionamento só por header | Path major mais explícito para breaking |

---

# Consequências

## Positivas

- Fronteira clara sync / async
- Base para implementação e Security
- Alinhamento ADR-0004 / 0012 / 0013 / 0014

## Negativas / deferred

- OpenAPI e BFF ainda abertos
- Detalhe AuthZ na AS-009

---

# Referências

- `docs/05-architecture/api-strategy.md`
- `architecture-sessions/AS-014-api-strategy.md`
- `workspace/AS-014/`

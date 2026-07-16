---
title: Technical Bootstrap
status: Stable
version: 1.1.1
created: 2026-07-14
updated: 2026-07-16
author: Architecture Team
category: Architecture
phase: Implementation
related:
  - docs/05-architecture/adr/foundation/ADR-0024-technical-bootstrap.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/database-architecture.md
  - docs/05-architecture/frontend-architecture.md
  - docs/05-architecture/devops-observability.md
  - docs/05-architecture/platform-security.md
  - docs/05-architecture/mvp-scope.md
  - architecture-sessions/AS-021-technical-bootstrap.md
---

# Technical Bootstrap

> Escolhas mínimas de stack, repositórios e roles Must que liberam o 1º código de aplicação.

Decisão formal: [ADR-0024](adr/foundation/ADR-0024-technical-bootstrap.md) · AS-021  
**D-006 emendado 2026-07-14:** layout `api` / `front` / `mobile` (não monorepo).

---

## 1. Purpose

Registrar o que os ADRs técnicos deixaram Deferred **somente onde o scaffold exige escolha**.

---

## 2. Stack

| Camada | Escolha |
|---|---|
| Backend | TypeScript · Node.js LTS · NestJS |
| Frontend Staff | React · TypeScript · Vite (SPA) |
| Database | PostgreSQL · shared + `tenant_id` · migrations em `health-platform-api` |
| Eventos (bootstrap) | Outbox + worker · broker produto Deferred |
| Auth (bootstrap) | Adapter Identity PS · IdP Deferred |

---

## 3. Repositórios

Três repos de aplicação + arquitetura. **Regra:** novas APIs/apps do mesmo tipo entram como **subdiretórios**, não repos novos.

```text
health-platform-architecture/     # docs + ADRs
health-platform-api/
  platform/                       # Nest modular monolith
  packages/platform-contracts/
  # workers/, bff/, … → futuros subdirs
health-platform-front/
  staff-web/                      # React Staff (M-02)
  packages/
  # portal-web/, admin-web/ → futuros
health-platform-mobile/
  # patient-app/, staff-app/ → futuros (Deferred)
```

---

## 4. CI mínimo

GitHub Actions **por repo** de aplicação: install · lint · test · build · scan de secrets.  
Postgres local: `docker compose` em `health-platform-api`.

---

## 5. Roles Must

`platform_admin` · `org_admin` · `clinician` · `receptionist` · `patient`

Deny-by-default. Permissions `resource:action` evoluem com módulos Must.

---

## 6. Deferred (explícito)

IdP/SSO · vault/KMS · broker · cloud hosting DB · OpenAPI completo · design system · matriz AuthZ completa · mobile apps · AS-008 / OQs residuais.

**Bootstrap Auth (dev mint):** hoje `POST /auth/dev/token` devolve só `accessToken`. O par com **`refreshToken`** está acordado como gap de contrato/sessão — ver [workspace/implementation/known-gaps.md](../../workspace/implementation/known-gaps.md) (GAP-001); implementação sob priorização, não bloqueia o scaffold.

---

## 7. Referências

[ADR-0024](adr/foundation/ADR-0024-technical-bootstrap.md) · [backend-architecture.md](backend-architecture.md) · [mvp-scope.md](mvp-scope.md) · [platform-security.md](platform-security.md) · [known-gaps (implementation)](../../workspace/implementation/known-gaps.md)

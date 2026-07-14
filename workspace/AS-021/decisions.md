# Decisões — AS-021 (preliminares)

> Válidas somente após *"confirmo o pacote"*. Em seguida → **ADR-0024** + `technical-bootstrap.md`.

| ID | Decisão | Proposta |
|---|---|---|
| D-001 | Backend | TypeScript / Node.js LTS · NestJS (alt. Fastify+módulos) |
| D-002 | Frontend Staff | React + TypeScript · Vite · SPA |
| D-003 | Database | PostgreSQL · shared + `tenant_id` · migrations no repo app |
| D-004 | Eventos bootstrap | Outbox + worker; broker produto Deferred |
| D-005 | Auth bootstrap | Adapter Identity PS; IdP produto Deferred |
| D-006 | Repos | Monorepo `health-platform` (apps/api + apps/staff-web); arquitetura permanece em repo separado |
| D-007 | CI | GitHub Actions · lint/test/build/scan · local compose |
| D-008 | Roles Must | `platform_admin`, `org_admin`, `clinician`, `receptionist`, `patient` · deny-by-default |

## Explicitamente fora

IdP/vault/broker finais · OpenAPI completo · Q-005/011/015 fino · AS-008 · design system · matriz AuthZ completa.

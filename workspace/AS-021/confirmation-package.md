# Pacote de Confirmação — AS-021

> 🟡 **Ready for Confirmation**

Responder com **"confirmo o pacote"** (ou ajustes pontuais nas propostas D-001…D-008) para autorizar:

1. **ADR-0024 — Technical Bootstrap**
2. Doc oficial `docs/05-architecture/technical-bootstrap.md`
3. Propagação INDEX / AI Context / CLAUDE
4. Liberação do scaffold do monorepo de aplicação (Fases 5–6)

---

## Resumo do que será formalizado

| # | Escolha |
|---|---|
| Backend | TypeScript / Node LTS · NestJS |
| Frontend | React + TypeScript · Vite (Staff SPA) |
| DB | PostgreSQL (shared + tenant_id) |
| Eventos | Outbox + worker; broker Deferred |
| Auth | Adapter; IdP Deferred |
| Repos | Monorepo app + repo arquitetura separado |
| CI | GitHub Actions mínimo |
| Roles | platform_admin · org_admin · clinician · receptionist · patient |

## Fonte

- [stack-proposal.md](./stack-proposal.md)
- [repos-layout.md](./repos-layout.md)
- [roles-must.md](./roles-must.md)
- [decisions.md](./decisions.md)
- [bootstrap-boundary.md](./bootstrap-boundary.md)

## Critério de aceite pós-confirmação

ADR Accepted + doc Stable + INDEX aponta bootstrap fechado → checklist go (Fase 6) → 1º código no monorepo app.

---
id: AS-021
title: Technical Bootstrap
status: Completed
version: 1.0.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
phase: Implementation Readiness
priority: Critical
tags:
  - bootstrap
  - stack
  - repositories
related:
  - workspace/AS-021/README.md
  - workspace/AS-021/confirmation-package.md
  - docs/05-architecture/technical-bootstrap.md
  - docs/05-architecture/adr/foundation/ADR-0024-technical-bootstrap.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/database-architecture.md
  - docs/05-architecture/frontend-architecture.md
  - docs/05-architecture/devops-observability.md
  - docs/05-architecture/platform-security.md
  - docs/05-architecture/mvp-scope.md
---

# AS-021 — Technical Bootstrap

## Informações Gerais

| Campo | Valor |
|---|---|
| Sessão | AS-021 |
| Tema | Technical Bootstrap (stack / repos / roles Must mínimos) |
| Status | Completed |
| Confirmação | *"pacote aprovado"* — 2026-07-14 |
| ADR | ADR-0024 |
| Fase | Implementation Readiness |
| Prioridade | Critical — gate do 1º código |
| Data | 2026-07-14 |

---

## Objetivo

Registrar as escolhas **mínimas de produto técnico** que os ADRs deixaram Deferred e que o primeiro código exige: linguagem/runtime backend, frontend, banco de dados, organização de repositórios e papéis Must mínimos.

Esta sessão **não** redefine estilo arquitetural (já ADR-0014+), nem fecha IdP/vault/broker finais.

---

## Hipótese Inicial

Um **monorepo de aplicação** com backend TypeScript (modular monolith), frontend React (Staff shell M-02), PostgreSQL (shared + `tenant_id`) e CI mínimo é suficiente para bootstrap do MVP Must, alinhado aos critérios já fixados nos ADRs.

---

## Fora do escopo

- IdP / SSO produto final · vault/KMS produto · broker produto
- OpenAPI completo · design system · microfrontends · GraphQL
- Q-005, Q-011, AS-008, Q-015 fino, Q-019
- Código de aplicação (só após ADR Accepted desta sessão)

---

## Workspace

Ver [`workspace/AS-021/`](../workspace/AS-021/README.md).

ADR-0024 Accepted. Pacote: [`workspace/AS-021/confirmation-package.md`](../workspace/AS-021/confirmation-package.md).

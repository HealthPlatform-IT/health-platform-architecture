# Stack Proposal — AS-021

> **Proposta para confirmação** — ajustar antes do *"confirmo"* se necessário.

## D-001 — Backend

| Opção | Avaliação |
|---|---|
| **TypeScript / Node.js (proposta)** | Alinha FE/BE no mesmo ecossistema; bom para modular monolith + Outbox; hiring amplo |
| .NET / C# | Forte em enterprise/health BR; mais distância do FE TS |
| JVM (Kotlin/Java) | Maduro; overhead de bootstrap MVP |
| Go | Bom para serviços; menos ecossistema “monolith modular + ORM” típico |

**Proposta:** TypeScript no Node.js LTS.  
**Framework HTTP:** NestJS *ou* Fastify + estrutura de camadas ADR-0014 — **preferência NestJS** (módulos = boundaries).  
Alternativa aceitável se o time preferir: Fastify + pasta `modules/`.

## D-002 — Frontend

| Opção | Avaliação |
|---|---|
| **React + TypeScript (proposta)** | Alinha ADR-0021 (SPA web); shell M-02 |
| Vue / Angular | Válidos; menor sinergia com hipotético shared types TS |

**Proposta:** React + TypeScript (SPA Staff). Framework build: Vite.  
Portal/Admin: mesma stack; apps separados no monorepo (ADR-0021 superfícies).

## D-003 — Banco de dados

| Opção | Avaliação vs ADR-0015 |
|---|---|
| **PostgreSQL (proposta)** | ACID; RLS/`tenant_id`; ops SaaS maduro |
| MySQL / MariaDB | Possível; RLS mais fraco |
| SQL Server | Válido enterprise; custo/ops |

**Proposta:** PostgreSQL. Migrations versionadas no repo app. Vendor cloud (RDS/Cloud SQL/Supabase/Neon…) **Deferred** — local Docker Postgres no bootstrap.

## D-004 — Mensageria (bootstrap)

| Fase | Escolha |
|---|---|
| Bootstrap / MVP inicial | Outbox + worker in-process (ou fila leve) — **sem** fixar broker produto |
| Pós-PoC | Produto broker conforme critérios ADR-0017 |

## D-005 — Auth bootstrap

| Fase | Escolha |
|---|---|
| Bootstrap | AuthN mínima (dev IdP / OIDC local ou Cognito/Auth0 depois) — **adapter** atrás do Identity PS |
| Produto IdP | Deferred (ADR-0020) |

Não fixar vault/KMS nesta sessão.

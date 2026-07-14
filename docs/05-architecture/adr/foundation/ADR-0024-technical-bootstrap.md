---
title: ADR-0024 - Technical Bootstrap
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - bootstrap
  - stack
  - repositories
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-021-technical-bootstrap.md
  - docs/05-architecture/technical-bootstrap.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/database-architecture.md
  - docs/05-architecture/frontend-architecture.md
  - docs/05-architecture/devops-observability.md
  - docs/05-architecture/platform-security.md
  - docs/05-architecture/mvp-scope.md
  - docs/05-architecture/adr/foundation/ADR-0014-backend-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0015-database-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0020-platform-security.md
  - docs/05-architecture/adr/foundation/ADR-0021-frontend-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0022-devops-observability.md
  - workspace/AS-021/confirmation-package.md
---

# ADR-0024 — Technical Bootstrap

## Status

Accepted

---

# Contexto

Os ADRs **0013–0023** definiram estilo, persistência, API, eventos, segurança, frontend, DevOps e MVP, deixando **Deferred** as escolhas de produto técnico (linguagem, framework, DB vendor, organização de repos, IdP, broker).

O Sprint 3 Readiness Audit classificou o bootstrap como **bloqueante do 1º código**. A **AS-021** confirmou o pacote em 2026-07-14 (*"pacote aprovado"*).

Este ADR **não** reabre modular monolith, shared+`tenant_id`, Outbox, deny-by-default nem Shell Composition.

---

# Problema

Sem escolha registrada de stack/repos:

- Scaffold implícito e conflitante entre desenvolvedores/IA
- Retrabalho de boundaries e CI
- Ambiguidad de papéis mínimos no dia 1

---

# Decisão

## 1. Backend (D-001)

- **Linguagem/runtime:** TypeScript em Node.js LTS
- **Framework:** NestJS (estrutura de módulos alinhada a ADR-0014)
- Alternativa operacional aceitável: Fastify + pasta `modules/` — só com justificativa explícita em PR de bootstrap

## 2. Frontend (D-002)

- **Staff SPA:** React + TypeScript + Vite
- Superfícies Portal/Admin: mesma stack; apps em subdiretórios de `health-platform-front/`

## 3. Database (D-003)

- **Produto:** PostgreSQL
- Isolamento: shared + `tenant_id` (ADR-0013 / ADR-0015)
- Migrations versionadas em `health-platform-api`
- Hosting cloud concreto (RDS/Cloud SQL/Neon…): **Deferred** — Docker Compose local no bootstrap

## 4. Eventos no bootstrap (D-004)

- Outbox no mesmo UoW + worker/consumer
- Produto de broker: **Deferred** (critérios ADR-0017)

## 5. Auth bootstrap (D-005)

- Identity PS atrás de **adapter**
- IdP/SSO produto: **Deferred** (ADR-0020)
- Vault/KMS produto: **Deferred**

## 6. Repositórios (D-006) — *emenda 2026-07-14*

Organização **por superfície de entrega** (não monorepo único). Cada raiz abaixo é um **repositório Git** independente.

```text
health-platform-architecture/     # docs + ADRs (sem código de app)
health-platform-api/              # todo backend
  platform/                       # modular monolith NestJS (hoje)
  packages/                       # libs/contratos backend
  # futuras APIs/workers → subdirs irmãos (ex.: workers/, bff/)
health-platform-front/            # todo frontend web
  staff-web/                      # SPA Staff (shell M-02)
  packages/
  # portal-web/, admin-web/ → subdirs futuros
health-platform-mobile/           # todo mobile (scaffold Deferred)
  # patient-app/, staff-app/ → subdirs futuros
```

**Regra:** múltiplas APIs/apps do mesmo tipo **não** viram repos novos — ficam em **subdiretórios** do repo da superfície (`api` / `front` / `mobile`).

Código de aplicação **não** entra no repo de arquitetura.

> **Supersede:** a proposta original de monorepo `health-platform/` (AS-021) foi substituída por esta organização convencionada pela equipe antes do push remoto.

## 7. CI (D-007)

- **GitHub Actions** em **cada** repo de aplicação: install · lint · test · build · scan básico de secrets
- Preferência operacional: **pnpm** workspaces dentro de cada repo
- Ambiente local via Compose em `health-platform-api`; promoção alinhada a ADR-0022

## 8. Roles Must mínimos (D-008)

| Role | Uso |
|---|---|
| `platform_admin` | Operação plataforma |
| `org_admin` | Admin organização |
| `clinician` | Atendimento / documentação |
| `receptionist` | Agenda / operacional |
| `patient` | Portal (contrato antecipado) |

Ausência de grant = **deny** (ADR-0020). Catálogo fino de permissions cresce com módulos Must.

## 9. Formalização (D-009)

Este ADR + `docs/05-architecture/technical-bootstrap.md`.

---

# Alternativas rejeitadas / adiadas

| Alternativa | Motivo |
|---|---|
| Escolher IdP/broker/vault agora | Prematuro; adapters bastam |
| Monorepo único `health-platform/` | Substituído pela convenção de equipe (api/front/mobile) antes do remoto |
| Um repo Git por microserviço/API | Contradiz a regra de subdiretórios por superfície |
| Schema-per-tenant | Contradiz ADR-0015 |
| Microfrontends obrigatórios | Contradiz ADR-0021 |
| Fixar cloud/K8s/APM | ADR-0022 Deferred |

---

# Consequências

## Positivas

- Gate do 1º código desbloqueado com stack explícita
- Boundaries Nest/React/Postgres alinhados aos ADRs técnicos
- Roles mínimos para deny-by-default no scaffold

## Negativas / débitos

- Troca futura de IdP/broker/cloud permanece necessária
- Matriz AuthZ completa ainda Deferred
- pnpm vs npm/yarn: preferência operacional **pnpm** no scaffold (não bloqueante)

---

# Conformidade

Respeita ADR-0014, 0015, 0017, 0020, 0021, 0022 e MVP ADR-0019. Não inventa domínio, PS ou módulo novos.

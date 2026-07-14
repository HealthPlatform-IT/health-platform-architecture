---
title: ADR-0020 - Platform Security
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - security
  - identity
  - authorization
  - audit
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-009-platform-security.md
  - docs/05-architecture/platform-security.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/multi-tenant-strategy.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/api-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/adr/foundation/ADR-0013-multi-tenant-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0014-backend-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0016-api-strategy.md
  - workspace/AS-009/confirmation-package.md
---

# ADR-0020 — Platform Security

## Status

Accepted

---

# Contexto

Identity, Authorization e Audit são **Platform Services Confirmed** (ADR-0005), fora do Core (ADR-0009). Multi-tenant (ADR-0013), backend (ADR-0014) e API (ADR-0016) adiaram AuthZ detalhada, secrets e paths públicos para **AS-009**.

A **AS-009** confirmou D-001 a D-010 em 2026-07-14 (*"confirmo"*). **Q-019** (Compliance Service) permanece Open.

---

# Problema

Sem modelo de segurança técnica:

- Fronteira AuthN/AuthZ ambígua na implementação
- I-10 sem regra operacional
- Pipeline inconsistente entre HTTP, jobs e consumers
- Risco de secrets e AuthZ no Core ou só em módulos

---

# Decisão

## 1. Camadas (D-001)

| Camada | Papel |
|---|---|
| **Core** | Contratos / invariantes I-01, I-02, I-10 |
| **Identity / Authorization / Audit** | Platform Services — enforcement |
| **Módulos / Domains** | Consomem AuthZ — não inventam papéis |

## 2. Fronteiras PS (D-002)

| PS | Faz | Não faz |
|---|---|---|
| **Identity** | AuthN, sessão, User + Tenant Context | Allow/deny de recurso; Audit de negócio |
| **Authorization** | Role + Permission + Scope → allow/deny | AuthN; registro de Audit |
| **Audit** | Rastreabilidade de ação e acesso sensível | Observabilidade técnica; AuthZ |

## 3. Modelo AuthZ (D-003)

- **Role** por tenant (catálogo fino Deferred)
- **Permission** sobre tipos de recurso/ação
- **Scope:** tenant obrigatório + org/unit/resource quando aplicável
- **Default:** deny-by-default
- Motor ABAC completo / produto de políticas = Deferred

## 4. Pipeline (D-004)

```text
Inbound → Identity (AuthN) → Runtime Context → Authorization (AuthZ)
        → Application / Domain → Persist / Response
        → Audit (relevante) → Domain Event (tenant no envelope)
```

AuthZ **não** fecha via Event Bus no request sync.

## 5. I-10 — Dados sensíveis (D-005)

Clínico/PHI, identidade do paciente e cross-tenant exigem AuthZ + Audit. Em dúvida → tratar como sensível.

## 6. Cross-tenant (D-006)

Exceção governada: papel de plataforma + Authorization + Audit (ADR-0013).

## 7. Paths públicos (D-007)

Allowlist mínima (health / readiness / well-known). Demais Product/Platform exigem AuthN + tenant + AuthZ.

## 8. Secrets (D-008)

| ID | Princípio |
|---|---|
| S-01 | Secrets fora de repositório / ADRs / config de domínio |
| S-02 | Credenciais fora do monólito de domínio |
| S-03 | Vault / KMS / rotação = Deferred (DevOps) |
| S-04 | Env vars não substituem política de cofre em produção |

## 9. Q-019 (D-009)

Compliance Service **não** promovido. Governance & Compliance Domain permanece dono de políticas; enforcement técnico atual via Identity / AuthZ / Audit / Config.

## 10. Deferred (D-010 / formalização)

IdP/SSO produto · vault/KMS · matriz completa de papéis · motor de políticas · Frontend Auth UX.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Identity/AuthZ no Core | ADR-0009 |
| AuthZ só embutida em módulos | Duplicação; sem contrato transversal |
| ABAC completo obrigatório no MVP | Prematuro |
| Fechar Q-019 nesta sessão | Needs Review explícito |

---

# Consequências

## Positivas

- Base clara para Identity/AuthZ/Audit na implementação
- I-10 operacionalizado
- Alinhamento com tenant, backend e API

## Negativas / deferred

- Catálogo de roles/permissions ainda a detalhar no MVP
- Produto IdP/vault em sessão futura
- Q-019 permanece Open

---

# Referências

- `docs/05-architecture/platform-security.md`
- `architecture-sessions/AS-009-platform-security.md`
- `workspace/AS-009/`

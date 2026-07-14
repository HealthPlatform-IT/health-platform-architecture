---
title: ADR-0022 - DevOps and Observability
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - devops
  - observability
  - ci-cd
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-019-devops-observability.md
  - docs/05-architecture/devops-observability.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/platform-security.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0014-backend-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0020-platform-security.md
  - workspace/AS-019/confirmation-package.md
---

# ADR-0022 — DevOps and Observability

## Status

Accepted

---

# Contexto

Observability é **PS Confirmed** (ADR-0005), distinto de Audit. Deployables e Module ≠ deploy (ADR-0014). Secrets S-01…S-04 com vault Deferred (ADR-0020). Sprint 3 restava fechar princípios de ambientes, CI/CD e telemetria.

A **AS-019** confirmou D-001 a D-010 em 2026-07-14 (*"confirmo"*).

---

# Problema

Sem modelo DevOps/Observability:

- Ambientes e promoção indefinidos
- Confusão Observability vs Audit vs Care Monitoring
- Risco de PHI em logs
- Lock-in precoce de tools

---

# Decisão

## 1. Fronteira (D-001)

DevOps/Observability = operação técnica — fora do Core. **≠** Audit (negócio) **≠** Care Monitoring (clínico).

## 2. Ambientes (D-002 / D-009)

Mínimos: **local/dev · staging · production**. Dados clínicos reais **não** em local/dev por padrão. Secrets/config por ambiente.

## 3. Deployables (D-003)

| Unidade | Papel |
|---|---|
| **platform-api** | Monólito modular + porta Event Bus |
| **workers** (opc.) | Consumers/jobs — mesmo pipeline tenant |
| **web** | Staff / Portal (ADR-0021) |

Split extra só com critérios ADR-0014.

## 4. CI/CD (D-004)

CI: build · testes · análise · artefato sem secrets. CD: promoção controlada · rollback · artefato versionado. Migrações alinhadas ADR-0015. **Produto** de CI/CD Deferred.

## 5. Observability (D-005 / D-006)

Pilares: **logs · metrics · traces · alerts** técnicos. Propagar **Correlation-Id** e **tenant** (sem PHI). Proibido dump clínico em logs.

## 6. Secrets (D-007)

Confirma S-01…S-04. Vault/KMS: critérios (isolamento env, rotação, auditoria de acesso, sem print em pipeline); **produto Deferred**.

## 7. Critérios de tool (D-008)

Cloud / IaC / APM: isolamento multi-tenant · observabilidade exportável · sem forçar microsserviços. Produto Deferred.

## 8. Formalização (D-010)

Este ADR + `devops-observability.md`.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Observability = Audit | ADR-0005 / 0020 |
| Escolher cloud/K8s/APM agora | Prematuro |
| Um deployable por módulo | Viola ADR-0014 |

---

# Consequências

## Positivas

- Sprint 3 técnico (INDEX) fechável neste bloco
- Fronteiras claras de telemetria e promoção

## Negativas / deferred

- Vendors ainda abertos
- Runbooks detalhados = fase de implementação
- Q-019 permanece Open

---

# Referências

- `docs/05-architecture/devops-observability.md`
- `architecture-sessions/AS-019-devops-observability.md`
- `workspace/AS-019/`

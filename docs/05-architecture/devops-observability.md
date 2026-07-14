---
title: DevOps and Observability
status: Stable
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/adr/foundation/ADR-0022-devops-observability.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/platform-security.md
  - docs/05-architecture/platform-services.md
  - architecture-sessions/AS-019-devops-observability.md
---

# DevOps and Observability

> Ambientes, CI/CD princípios, Observability Service operacional e secrets runtime.

Decisão formal: [ADR-0022](adr/foundation/ADR-0022-devops-observability.md) · AS-019.

---

## 1. Purpose

Definir operação técnica da plataforma — **sem** escolher cloud, CI ou APM produto.

---

## 2. Fronteira

| Sistema | Papel |
|---|---|
| DevOps / Observability | Saúde técnica, promoção, telemetria |
| Audit Service | Rastreabilidade de negócio / acesso |
| Care Monitoring | Indicadores de cuidado (domínio/módulo) |

---

## 3. Ambientes

| Ambiente | Uso |
|---|---|
| local/dev | Desenvolvimento; sem PHI real por padrão |
| staging | Validação pré-prod |
| production | Tenants reais |

---

## 4. Deployables

`platform-api` · `workers` (opc.) · `web` — alinhados ADR-0014 / ADR-0021.

---

## 5. CI/CD

Build · test · scan · artefato limpo → promoção controlada → rollback. Tool Deferred.

---

## 6. Observability

Logs · metrics · traces · alerts. Correlation-Id + tenant. Sem PHI em logs.

---

## 7. Secrets

S-01…S-04 (ADR-0020). Vault/KMS por critérios; produto Deferred.

---

## 8. Deferred

CI vendor · APM · cloud · K8s · IaC produto · runbooks · Q-019.

---

## 9. Referências

[ADR-0022](adr/foundation/ADR-0022-devops-observability.md) · [backend-architecture.md](backend-architecture.md) · [platform-security.md](platform-security.md) · [platform-services.md](platform-services.md)

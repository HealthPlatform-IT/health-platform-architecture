---
title: Platform Security
status: Stable
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/adr/foundation/ADR-0020-platform-security.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/multi-tenant-strategy.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/api-strategy.md
  - architecture-sessions/AS-009-platform-security.md
---

# Platform Security

> Modelo técnico de AuthN, AuthZ, Audit, I-10, paths públicos e secrets.

Decisão formal: [ADR-0020](adr/foundation/ADR-0020-platform-security.md) · AS-009.

---

## 1. Purpose

Definir como a plataforma autentica, autoriza e audita — sem escolher IdP/vault produto e sem matriz completa de papéis.

---

## 2. Camadas

| Camada | Responsabilidade |
|---|---|
| Core | I-01 Tenant Context · I-02 Runtime Context · I-10 autorização para dados sensíveis |
| Identity PS | AuthN, sessão, User + Tenant |
| Authorization PS | Role + Permission + Scope · deny-by-default |
| Audit PS | Rastreabilidade de negócio / acesso |
| Observability PS | Saúde técnica — **≠** Audit |
| Módulos | Consomem Authorization |

---

## 3. AuthN vs AuthZ

| | Identity | Authorization |
|---|---|---|
| Pergunta | Quem é? Em qual tenant? | Pode fazer X em Y? |
| Saída | Sessão + Runtime Context | allow \| deny |

Sem tenant válido → rejeição antes da AuthZ (ADR-0013).

---

## 4. Modelo AuthZ

- **Role** (por tenant)
- **Permission** (ação sobre tipo de recurso)
- **Scope** (tenant + org/unit/resource)
- **Default:** deny-by-default
- Catálogo fino de roles e motor ABAC = Deferred

---

## 5. Pipeline

```text
Inbound → AuthN → Runtime Context → AuthZ → Application/Domain
        → Persist/Response → Audit → Event (tenant no envelope)
```

Aplicável a Product/Platform API, jobs e consumers (contexto da mensagem). AuthZ sync **não** usa Event Bus.

---

## 6. I-10 — Dados sensíveis

| Classe | Regra |
|---|---|
| Clínico / PHI | AuthZ + Audit de acesso |
| Identidade do paciente | AuthZ no escopo tenant/org |
| Cross-tenant | Papel plataforma + AuthZ + Audit |
| Dúvida | Tratar como sensível |

---

## 7. Paths públicos

Allowlist: health / readiness / well-known. Demais rotas Product/Platform: AuthN + tenant + AuthZ.

---

## 8. Secrets

S-01…S-04 em ADR-0020: fora de repo/domínio; vault/KMS Deferred.

---

## 9. Deferred

IdP/SSO · vault · matriz de papéis · motor de políticas · Frontend Auth · **Q-019** Compliance Service.

---

## 10. Referências

[ADR-0020](adr/foundation/ADR-0020-platform-security.md) · [platform-services.md](platform-services.md) · [multi-tenant-strategy.md](multi-tenant-strategy.md) · [backend-architecture.md](backend-architecture.md) · [api-strategy.md](api-strategy.md)

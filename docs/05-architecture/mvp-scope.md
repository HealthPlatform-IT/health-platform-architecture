---
title: MVP Scope
status: Draft
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/adr/foundation/ADR-0019-mvp-scope.md
  - docs/03-capabilities/business-capability-map.md
  - docs/05-architecture/module-strategy.md
  - architecture-sessions/AS-017-mvp-scope.md
---

# MVP Scope

> Recorte de sub-capabilities e módulos para o escopo inicial (produto v1).

Decisão formal: [ADR-0019](adr/foundation/ADR-0019-mvp-scope.md) · AS-017 · **Q-006 Answered**.

---

## 1. Purpose

Definir o que entra no **MVP arquitetural/produto v1** — preservando Care Journey — sem roadmap comercial completo e **sem Billing** (Q-005).

---

## 2. Tiers

| Tier | Uso |
|---|---|
| **Must** | Primeira fatia implementável |
| **Should** | Em seguida; não bloqueia validação clínica staff |
| **Later** | Catálogo permanece; fora do MVP inicial |
| **Out** | Fora até decisão futura (Q-005 / Hospital Future) |

### Critérios Must

Necessário a Journey ambulatorial (+ tele); segurança multi-tenant mínima; Registrar o cuidado; sem Extension/Hospital/Billing profundo.

---

## 3. Loop Must

```text
Patient / Professional / Org
        → Scheduling
        → Attendance (+ Telemedicine mode)
        → Clinical Documentation / Orders / Artifacts
        → Clinical Timeline (Read Model)
```

---

## 4. Sub-capabilities (resumo)

| Área | Must | Should | Later / Out |
|---|---|---|---|
| Relacionar | Patient, Professional, Organization | — | Payer profundo Later |
| Organizar | Care Scheduling | Referral, Operational Workflow mínimo | Resource/Queue Later |
| Executar | Clinical Attendance + tele | Therapeutic básico | Diagnostic/Home Later; Hospital Out |
| Registrar | Documentation, Orders, Artifacts, Timeline | — | Diagnostic Recording Later |
| Comunicar | — | Patient/Professional/Notifications/Preferences | — |
| Integrar | — | Webhooks mínimos | FHIR/Diagnostics Later; ERP Out |
| Monitorar | — | Pendencies; Dashboard mínimo | Alerts/Analytics profundo Later; IA Out |
| Governar | Access/Audit/Config/Flags (via PS) | Governance Admin | Compliance Q-019 Later |

Detalhe: `workspace/AS-017/mvp-subcapability-slice.md` (evidência da sessão).

---

## 5. Módulos

| Must | Should | Later |
|---|---|---|
| M-01, M-02, M-03, M-04, M-05, M-06, M-09 | M-07, M-08, M-11, M-12, M-16 | M-10, M-14, M-15 |

---

## 6. Platform Services Must

Identity · Authorization · Audit · Configuration · Feature Flag · Event Bus · Storage · File · Observability · Medical Form Engine · Document Engine

---

## 7. Board

Later/Out não sobem a **Pronto para desenvolvimento** sem novo recorte.

---

## 8. Referências

[ADR-0019](adr/foundation/ADR-0019-mvp-scope.md) · [business-capability-map.md](../03-capabilities/business-capability-map.md) · [module-strategy.md](module-strategy.md)

---
title: Domain Map
status: Draft
version: 0.1.0
created: 2026-07-02
updated: 2026-07-02
author: Architecture Team
category: Domain
phase: Product & Architecture Foundation
related:
  - docs/04-domain/business-domains.md
  - docs/03-capabilities/business-capability-map.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - architecture-sessions/AS-004-business-domain-map.md
---

# Domain Map

> Mapeamento oficial de **sub-capabilities → Business Domains**, Platform Services e read models.

Complementa [business-domains.md](business-domains.md). Decisão formal: [ADR-0008](../05-architecture/adr/foundation/ADR-0008-business-domain-map.md).

---

## 1. Legenda

| Tipo | Significado |
|---|---|
| **DOM** | Business Domain |
| **PS** | Platform Service (ADR-0005) |
| **VIS** | Read model / visão |
| **DEF** | Deferred |

---

## 2. Visão por Core Business Capability

```
Relacionar ──► 4 DOM (Patient, Professional, Organization, Payer)
Organizar  ──► 1 DOM (Care Coordination)
Executar   ──► 1 DOM Core + 2 Extension (+ 1 DEF)
Registrar  ──► 3 DOM + 1 VIS
Comunicar  ──► 1 DOM + 1 PS
Integrar   ──► 1 DOM + 1 DEF (+ PS)
Monitorar  ──► 2 DOM + 1 VIS + 1 DEF
Governar   ──► 1 DOM + 5 PS
```

---

## 3. Mapeamento completo

### Relacionar

| Sub-capability | Destino | Tipo |
|---|---|---|
| Patient Relationship | Patient Relationship | DOM |
| Professional & Provider Relationship | Professional Management | DOM |
| Organization & Network Relationship | Organization Management | DOM |
| Insurance & Payer Relationship | Payer & Insurance | DOM |

### Organizar

| Sub-capability | Destino | Tipo |
|---|---|---|
| Care Scheduling | Care Coordination | DOM |
| Resource Planning | Care Coordination | DOM |
| Queue & Flow Management | Care Coordination | DOM |
| Referral Coordination | Care Coordination | DOM |
| Operational Workflow | Care Coordination | DOM |

### Executar

| Sub-capability | Destino | Tipo |
|---|---|---|
| Clinical Attendance | Care Delivery | DOM |
| Therapeutic Care Delivery | Care Delivery | DOM |
| Diagnostic Service Delivery | Diagnostic Operations | DOM |
| Home Care Delivery | Home Care Operations | DOM |
| Hospital Care Delivery | Hospital Operations | DEF |

### Registrar

| Sub-capability | Destino | Tipo |
|---|---|---|
| Clinical Documentation | Clinical Record | DOM |
| Diagnostic Recording | Clinical Record | DOM |
| Prescription & Clinical Orders | Clinical Orders | DOM |
| Clinical Artifacts | Clinical Documents | DOM |
| Clinical Timeline | Clinical Timeline | VIS |

### Comunicar

| Sub-capability | Destino | Tipo |
|---|---|---|
| Patient Communication | Communication | DOM |
| Professional Communication | Communication | DOM |
| Communication Preferences & Consent | Communication | DOM |
| Internal Notifications | Notification Service | PS |

### Integrar

| Sub-capability | Destino | Tipo |
|---|---|---|
| Clinical Interoperability | Integration | DOM |
| Diagnostic Systems Integration | Integration | DOM |
| Webhooks & External APIs | Integration | DOM |
| Medical Devices & IoT | Integration | DOM |
| Financial & ERP Integration | — | DEF |

### Monitorar

| Sub-capability | Destino | Tipo |
|---|---|---|
| Care Pendencies & Follow-up | Care Monitoring | DOM |
| Clinical Alerts & Protocols | Care Monitoring | DOM |
| Operational SLAs | Operations Monitoring | DOM |
| Indicators & Dashboards | Analytics & Reporting | VIS |
| Intelligence & Automation | — | DEF |

### Governar

| Sub-capability | Destino | Tipo |
|---|---|---|
| Identity & Access | Identity Service | PS |
| Authorization & Permissions | Authorization Service | PS |
| Audit & Traceability | Audit Service | PS |
| Configuration & Feature Management | Configuration Service, Feature Flag Service | PS |
| Compliance & Data Protection | Governance & Compliance | DOM |
| Observability | Observability Service | PS |

---

## 4. Catálogo dos 16 domínios

| # | Domínio | Tipo | Capability primária |
|---|---|---|---|
| 1 | Care Delivery | Core | Executar |
| 2 | Clinical Record | Core | Registrar |
| 3 | Clinical Orders | Core | Registrar |
| 4 | Clinical Documents | Core | Registrar |
| 5 | Care Monitoring | Core | Monitorar |
| 6 | Patient Relationship | Supporting | Relacionar |
| 7 | Professional Management | Supporting | Relacionar |
| 8 | Organization Management | Supporting | Relacionar |
| 9 | Payer & Insurance | Supporting | Relacionar |
| 10 | Care Coordination | Supporting | Organizar |
| 11 | Communication | Supporting | Comunicar |
| 12 | Integration | Supporting | Integrar |
| 13 | Operations Monitoring | Supporting | Monitorar |
| 14 | Diagnostic Operations | Extension | Executar |
| 15 | Home Care Operations | Extension | Executar |
| 16 | Governance & Compliance | Cross-cutting | Governar |

---

## 5. Platform Services por origem

| Origem | Platform Service |
|---|---|
| Governar | Identity, Authorization, Audit, Configuration, Feature Flag, Observability |
| Comunicar | Notification Service, Communication Service |
| Integrar | Integration Service, Webhook Service |
| Registrar | Document Engine, Template Service |
| Read models | Search Service *(hipótese)* |
| Governança | Compliance Service *(hipótese — Q-019)* |

---

## 6. Contagem

| Métrica | Valor |
|---|---|
| Sub-capabilities mapeadas | 39 (35 ativas + 4 deferred) |
| Business Domains únicos | 16 |
| Mapeamentos DOM | 25 |
| Platform Services (sub-capabilities) | 6 |
| Read models | 2 |
| Deferred | 4 |

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1.0 | 2026-07-02 | Versão inicial — AS-004 confirmada |

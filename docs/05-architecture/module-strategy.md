---
title: Module Strategy
status: Draft
version: 0.1.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Architecture
phase: Product & Architecture Foundation
related:
  - docs/04-domain/business-domains.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/extension-model.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
---

# Module Strategy

> Catálogo oficial de **módulos candidatos** da Health Platform — unidades funcionais derivadas de Business Domains.

Decisão formal: [ADR-0009](adr/foundation/ADR-0009-core-platform-boundary.md) D-004, D-005, D-007.

---

## 1. Definition

**Module** = unidade funcional implementável derivada de domínio(s) + capability, que:

- entrega funcionalidade ao usuário ou orquestra outras unidades (shell);
- consome Platform Services por contrato;
- pode ser habilitado/desabilitado por tenant (Configuration + Feature Flag + Module Registry).

**Module** ≠ Business Domain ≠ Platform Service ≠ Read Model.

---

## 2. Cardinalidade domínio → módulo

| Padrão | Exemplo |
|---|---|
| 1:1 | Clinical Orders → Orders Module |
| 1:N | Care Coordination → Scheduling |
| N:1 | Patient Portal → Patient Relationship + Communication |
| 1:0 | Communication — sem módulo dedicado |
| Extension | Diagnostic Operations → Diagnostic Module |

**15 módulos** derivados de **16 domínios**.

---

## 3. Catálogo de módulos

### Core product (8)

| ID | Módulo | Domínio(s) primário(s) | Capability |
|---|---|---|---|
| M-01 | Scheduling | Care Coordination | Organizar |
| M-02 | Clinical Workspace | Care Delivery + clínicos *(shell)* | Executar, Registrar, Monitorar |
| M-03 | Attendance | Care Delivery | Executar |
| M-04 | Clinical Documentation | Clinical Record | Registrar |
| M-05 | Orders | Clinical Orders | Registrar |
| M-06 | Documents | Clinical Documents | Registrar |
| M-07 | Care Monitoring | Care Monitoring | Monitorar |
| M-08 | Patient Portal | Patient Relationship, Communication | Relacionar, Comunicar |

### Supporting (5)

| ID | Módulo | Domínio(s) primário(s) | Capability |
|---|---|---|---|
| M-09 | Professional & Org Admin | Professional Management, Organization Management | Relacionar |
| M-10 | Payer & Insurance | Payer & Insurance | Relacionar |
| M-11 | Integration Admin | Integration | Integrar |
| M-12 | Operations Dashboard | Operations Monitoring | Monitorar |
| M-16 | Governance Admin | Governance & Compliance | Governar |

### Extension (2)

| ID | Módulo | Extension Domain | Modelo operacional |
|---|---|---|---|
| M-14 | Diagnostic | Diagnostic Operations | Diagnostic Care |
| M-15 | Home Care | Home Care Operations | Home Care |

---

## 4. Clinical Workspace (M-02)

Módulo **shell transversal** que compõe M-03 a M-07 sem regras de negócio próprias. Extension Modules (M-14, M-15) carregados quando habilitados.

**Clinical Timeline** (read model) é consumida pelo shell — não é módulo.

---

## 5. Operational modes

| Conceito | Tratamento |
|---|---|
| **Telemedicine** | Operational Mode em M-03 Attendance — não Extension Module |
| **Communication** | Domínio orquestrado por módulos consumidores + PS — sem módulo dedicado |

---

## 6. Regras de acoplamento

1. Módulos **não** acessam estado interno de outro módulo.
2. Orquestração via **Event Foundation** e Platform Services.
3. Shell compõe via **Module Registry** — sem hardcode.
4. Extension Modules não alteram Core product modules.

---

## 7. Read models consumidos

| Read Model | Consumidores |
|---|---|
| Clinical Timeline | M-02, M-08 (parcial) |
| Analytics & Reporting | M-12 |

---

## 8. Domain → Module map (16 domínios)

| # | Business Domain | Módulo(s) | Padrão |
|---|---|---|---|
| 1 | Care Delivery | M-02, M-03 *(Telemedicine = mode)* | 1:N + shell |
| 2 | Clinical Record | M-04 | 1:1 |
| 3 | Clinical Orders | M-05 | 1:1 |
| 4 | Clinical Documents | M-06 | 1:1 |
| 5 | Care Monitoring | M-07 | 1:1 |
| 6 | Patient Relationship | M-08 | N:1 |
| 7–8 | Professional / Organization Mgmt | M-09 | N:1 |
| 9 | Payer & Insurance | M-10 | 1:1 |
| 10 | Care Coordination | M-01 | 1:1 |
| 11 | Communication | — | orquestrado |
| 12 | Integration | M-11 | 1:1 |
| 13 | Operations Monitoring | M-12 | 1:1 |
| 14 | Diagnostic Operations | M-14 | Extension |
| 15 | Home Care Operations | M-15 | Extension |
| 16 | Governance & Compliance | M-16 | 1:1 |

**Read models:** Clinical Timeline (M-02, M-08); Analytics (M-12).

---

## 9. Platform Services — consumo resumido

| PS | M-01 | M-02 | M-03 | M-04 | M-05 | M-06 | M-07 | M-08 | M-09+ |
|---|---|---|---|---|---|---|---|---|---|
| Identity / Authorization / Audit / Config | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Communication | ✓ | ○ | ✓ | ○ | ✓ | ○ | ✓ | ✓ | ○ |
| Medical Form Engine | ○ | ○ | ✓ | ✓ | ○ | ○ | ○ | ○ | ○ |
| Document Engine | ○ | ○ | ○ | ○ | ✓ | ✓ | ○ | ○ | ○ |
| Search | ○ | ✓ | ○ | ✓ | ○ | ○ | ○ | ○ | ✓ |

✓ = esperado · ○ = opcional. Detalhe: `workspace/AS-005/module-strategy-draft.md`.

---

## 10. Documentos relacionados

- [extension-model.md](extension-model.md) — D-005
- [read-models.md](read-models.md) — D-006
- [architecture-classification.md](architecture-classification.md) — D-008

---

## 11. Fora de escopo

- MVP por módulo (Q-006)
- APIs, telas, bounded contexts
- Implementação técnica de extensão (Q-009)

---
title: Platform Services
status: Draft
version: 0.4.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Architecture
phase: Product & Architecture Foundation
related:
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - docs/05-architecture/adr/foundation/ADR-0010-medical-form-engine.md
  - docs/05-architecture/adr/foundation/ADR-0011-document-engine.md
  - docs/05-architecture/document-engine.md
---

# Platform Services

> Catálogo oficial de **Platform Services** da Health Platform — responsabilidades transversais reutilizáveis consumidas por Business Domains e módulos.

Decisão formal: [ADR-0005](adr/foundation/ADR-0005-platform-services.md) · [ADR-0010](adr/foundation/ADR-0010-medical-form-engine.md) · [ADR-0011](adr/foundation/ADR-0011-document-engine.md).

---

## 1. Purpose

Este documento consolida o catálogo de Platform Services para consulta rápida. O ADR-0005 permanece como fonte de decisão de catálogo; ADR-0009 define **tiers de maturidade** pós-AS-005.

**Platform Service** ≠ Business Domain. Domínios possuem regras de negócio; serviços implementam transversais por contratos estáveis.

---

## 2. Princípio de decisão

```text
Regra específica de domínio    → Module / Business Domain
Reutilizável e transversal     → Platform Service
Fundação estrutural (contrato) → Core Platform
```

**Core Platform** (8 componentes) vs **Platform Services** (implementações): [core-platform.md](core-platform.md) — AS-005 D-001/D-002.

---

## 3. Tiers (atualizado AS-007)

| Tier | Qtd | Significado |
|---|---|---|
| **Confirmed** | 14 | Catálogo maduro para operação |
| **Strong Candidate** | 3 | Search, Template Service, Event Bus |
| **Needs Review** | 1 | Compliance Service (Q-019) |

### Confirmed (14)

Identity · Authorization · Audit · Configuration · Feature Flag · Communication · Notification · Integration · Webhook · Storage · File · Observability · **Medical Form Engine** · **Document Engine**

### Strong Candidate (3)

Search · Template Service · Event Bus

### Needs Review (1)

Compliance Service (Q-019)

---

## 4. Catálogo resumido

| Serviço | Capability | Tier AS-005 | ADR-0005 | Detalhe |
|---|---|---|---|---|
| Identity Service | Governar | Confirmed | Consolidado | Autenticação, sessões, multi-tenant |
| Authorization Service | Governar | Confirmed | Consolidado | Papéis, permissões, políticas |
| Audit Service | Governar | Confirmed | Consolidado | Rastreabilidade |
| Configuration Service | Governar | Confirmed | Consolidado | Config por tenant/instituição |
| Feature Flag Service | Governar | Confirmed | Consolidado | Rollout gradual |
| Observability Service | Governar | Confirmed | Consolidado | Métricas, logs, tracing |
| Communication Service | Comunicar | Confirmed | Consolidado | Canais externos |
| Notification Service | Comunicar | Confirmed | Consolidado | In-app, alertas internos |
| Integration Service | Integrar | Confirmed | Consolidado | FHIR, TISS, PACS… |
| Webhook Service | Integrar | Confirmed | Consolidado | Callbacks HTTP |
| Storage Service | Governar | Confirmed | Consolidado | Blobs |
| File Service | Governar / Registrar | Confirmed | Consolidado | Metadados de arquivos |
| Search Service | Governar / Monitorar | Strong | Consolidado | Busca indexada |
| Template Service | Comunicar / Registrar | Strong | Consolidado | Templates |
| Event Bus | Monitorar / Integrar | Strong | Identificado | Mensageria — Q-003 |
| Document Engine | Registrar | Confirmed | Identificado → ADR-0011 | [document-engine.md](document-engine.md) |
| Medical Form Engine | Registrar / Executar | Confirmed | Identificado → ADR-0010 | [medical-form-engine.md](medical-form-engine.md) |
| Compliance Service | Governar | Needs Review | — | Q-019 |

---

## 5. Visão por capability

```text
Governar
├── Identity, Authorization, Audit
├── Configuration, Feature Flag, Observability
└── Compliance Service *(Needs Review — Q-019)*

Comunicar
├── Communication Service
├── Notification Service
└── Template Service *(Strong)*

Integrar
├── Integration Service
└── Webhook Service

Infraestrutura transversal
├── Storage, File
├── Search *(Strong)*
└── Event Bus *(Strong)*

Registrar
├── Medical Form Engine *(Confirmed — ADR-0010)*
└── Document Engine *(Confirmed — ADR-0011)*
```

---

## 6. Fronteiras importantes

| Situação | Destino |
|---|---|
| E-mail de confirmação ao paciente | Communication Service |
| Alerta in-app para profissional | Notification Service |
| Envio de dados via FHIR | Integration Service |
| Webhook de pagamento | Webhook Service |
| Metadados de arquivo clínico | File Service |
| Blob storage | Storage Service |
| Política de retenção LGPD | Governance & Compliance Domain |
| Enforcement técnico de política | Compliance Service *(hipótese)* |

Detalhes: ADR-0004, ADR-0005.

---

## 7. Invariantes Core (I-01 a I-10)

Todo PS deve honrar invariantes do Core Platform — ver [core-platform.md](core-platform.md) §5.

---

## 8. Relação com Business Domains

Domínio **Governance & Compliance** define políticas; PS executam enforcement e rastreabilidade.

Mapeamento completo: [domain-map.md](../04-domain/domain-map.md).

---

## 9. Open Questions

| ID | Pergunta | Situação |
|---|---|---|
| Q-003 | Event Model e Event Bus | Deferred — AS-010 |
| Q-010 | Communication vs Notification | In Analysis |
| Q-013 | Document Engine e Medical Form Engine | **Answered** — ADR-0010, ADR-0011 |
| Q-014 | Template Service | **Answered** — ADR-0011 D-005 |
| Q-019 | Compliance Service | Needs Review |

**Q-007:** Answered — ADR-0009.

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1.0 | 2026-07-03 | Versão inicial — ADR-0005 |
| 0.2.0 | 2026-07-03 | Tiers AS-005 (D-003) — pós-confirmação AS-005 |
| 0.3.0 | 2026-07-03 | Medical Form Engine Confirmed — ADR-0010 |
| 0.4.0 | 2026-07-03 | Document Engine Confirmed — ADR-0011 |

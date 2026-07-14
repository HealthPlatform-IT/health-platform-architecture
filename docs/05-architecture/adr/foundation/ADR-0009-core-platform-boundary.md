---
title: ADR-0009 - Core Platform Boundary
status: Accepted
date: 2026-07-03
deciders:
  - Architecture Team
tags:
  - architecture
  - core-platform
  - platform-services
  - modules
  - extension
  - foundation
related:
  - architecture-sessions/AS-005-core-platform-boundary.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/extension-model.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/architecture-classification.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - workspace/AS-005/confirmation-package.md
---

# ADR-0009 — Core Platform Boundary

## Status

Accepted

---

# Contexto

A **AS-004** (ADR-0008) definiu 16 Business Domains e critérios **Domain vs Platform Service vs Read Model**. **Q-007** permaneceu parcial — faltavam fronteiras operacionais de **Core Platform**, **Modules**, **Extension** e classificação unificada.

A **AS-005** investigou e confirmou o pacote D-001 a D-008 em 2026-07-03.

---

# Problema

Sem fronteiras claras entre as seis categorias arquiteturais:

- Identity, Audit e Configuration podem ser confundidos com Core.
- Módulos podem duplicar Platform Services ou Read Models.
- Extension pode ser confundida com customização por cliente.
- IA e equipes não têm gate repetível para novas responsabilidades.

---

# Decisão

## 1. Core Platform (D-001)

Core Platform = **fundação estrutural mínima** — 8 contratos/invariantes:

1. Hierarchical Care Model Contracts
2. Care Journey Type System
3. Tenant Context (contrato)
4. Module Registry
5. Extension Mechanism
6. Event Foundation (contrato limitado)
7. Runtime Context
8. Shared Kernel Types

**Rejeitado do Core:** implementações Identity, Authorization, Audit, Configuration, Feature Flag, deploy/infra.

## 2. Reconciliação Core ↔ PS (D-002)

```text
Core define invariantes → PS operam dentro dos invariantes → Domínios definem política → PS executam mecanismo
```

ADR-0003 (sentido amplo) e ADR-0005 **não se contradizem**.

## 3. Platform Services — tiers (D-003)

| Tier | Qtd | Serviços |
|---|---|---|
| **Confirmed** | 12 | Identity, Authorization, Audit, Configuration, Feature Flag, Communication, Notification, Integration, Webhook, Storage, File, Observability |
| **Strong Candidate** | 5 | Search, Document Engine, Medical Form Engine, Template Service, Event Bus |
| **Needs Review** | 1 | Compliance Service (Q-019) |

## 4. Module Strategy (D-004, D-007)

- **15 módulos candidatos** — cardinalidade flexível (1:1, 1:N, N:1, 1:0, Extension).
- Communication Domain **sem módulo dedicado**.
- **Clinical Workspace (M-02)** = shell transversal — compõe M-03 a M-07 sem regras próprias.

## 5. Extension Model (D-005)

Três conceitos distintos: Extension Business Domain · Extension Module · Extension Mechanism.

- Extension Modules ativos: **M-14 Diagnostic**, **M-15 Home Care**.
- **Telemedicine** = Operational Mode em Attendance — **não** Extension Module.

## 6. Read Models (D-006)

- **Clinical Timeline** e **Analytics & Reporting** permanecem Read Models.
- Timeline consumida por M-02 e M-08 (parcial); Analytics por M-12.
- Timeline **não** é módulo nem domínio.

## 7. Classification matrix (D-008)

Árvore de **6 categorias** com invariantes I-01 a I-10, testes e anti-patterns.

Documentação: `docs/05-architecture/architecture-classification.md`.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Core monolítico com regras clínicas | ADR-0003 |
| Identity/Audit/Config no Core | ADR-0005 |
| 16 módulos = 16 domínios | Cardinalidade flexível |
| Timeline como domínio ou módulo | Read Model |
| Telemedicine como Extension Module | Operational Mode |
| Communication Module dedicado | Orquestrado por consumidores |
| Clinical Workspace no Core | Risco Core grande |

---

# Consequências

## Positivas

- Q-007 **Answered**.
- Gate repetível para AS-006 e sessões futuras.
- IA e equipes têm vocabulário estável (6 categorias).

## Negativas

- Tiers PS coexistem com Consolidado/Identificado do ADR-0005 — ambos válidos em níveis distintos.

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| ADR-0003 | Core Protection — refinado com Core Platform operacional |
| ADR-0005 | PS — tiers AS-005 complementam Consolidado/Identificado |
| ADR-0006 | Hierarquia configuração → extensão → customização |
| ADR-0008 | 16 domínios — base para módulos |

---

# Open Questions residuais

| ID | Assunto |
|---|---|
| Q-009 | Implementação Extension Mechanism |
| Q-019 | Compliance Service |
| OQ-C01 | Organization Context |
| OQ-C03 | UI composition contract no Core | **Answered** — ADR-0021 |
| Q-003 | Event Model |

---

# Documentação oficial

- `docs/05-architecture/core-platform.md`
- `docs/05-architecture/module-strategy.md`
- `docs/05-architecture/extension-model.md`
- `docs/05-architecture/read-models.md`
- `docs/05-architecture/architecture-classification.md`
- `docs/05-architecture/platform-services.md`

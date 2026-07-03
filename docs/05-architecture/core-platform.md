---
title: Core Platform
status: Draft
version: 0.1.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Architecture
phase: Product & Architecture Foundation
related:
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/module-strategy.md
  - architecture-sessions/AS-005-core-platform-boundary.md
  - workspace/AS-005/confirmation-package.md
---

# Core Platform

> Catálogo oficial do **Core Platform** — fundação estrutural mínima da Health Platform.

Responde **Q-007**. Decisão formal: [ADR-0009](adr/foundation/ADR-0009-core-platform-boundary.md) · [ADR-0003](adr/foundation/ADR-0003-core-protection-and-extension-model.md) D-001.

---

## 1. Purpose

Define o **Core Platform** como camada de contratos, invariantes e mecanismos de extensão — **sem** implementação operacional de negócio ou transversais.

**Core Platform** ≠ "Core" no sentido amplo do ADR-0003. Implementações de Identity, Audit, Configuration etc. são **Platform Services** (ADR-0005).

---

## 2. Regra operacional

```text
Core define invariantes → PS operam dentro dos invariantes → Domínios definem política → PS executam mecanismo
```

---

## 3. Catálogo — 8 componentes

| # | Componente | Papel |
|---|---|---|
| 1 | **Hierarchical Care Model Contracts** | Níveis Patient → Artifact (ADR-0001) |
| 2 | **Care Journey Type System** | Tipos de start event (ADR-0007) |
| 3 | **Tenant Context** | Contrato multi-tenant obrigatório |
| 4 | **Module Registry** | Registro e descoberta de módulos |
| 5 | **Extension Mechanism** | Hooks e composição (contratos) |
| 6 | **Event Foundation** | Contrato limitado — sem Event Model (Q-003) |
| 7 | **Runtime Context** | Agregador: tenant, organization, user, escopo |
| 8 | **Shared Kernel Types** | Referências mínimas estáveis entre camadas |

---

## 4. O que NÃO pertence ao Core

| Rejeitado do Core | Destino |
|---|---|
| Identity, Authorization, Audit | Platform Services |
| Configuration, Feature Flag | Platform Services |
| Regras clínicas e operacionais | Business Domains |
| UI de produto | Modules |
| Implementação de storage, e-mail | Platform Services |

---

## 5. Invariantes (I-01 a I-10)

Todo Platform Service e projeção deve honrar:

| # | Invariante |
|---|---|
| I-01 | Tenant Context obrigatório |
| I-02 | Runtime Context propagado |
| I-03 | Hierarchical Care Model respeitado |
| I-04 | Care Journey types — PS não inventam tipos |
| I-05 | Module Registry — módulos por contrato |
| I-06 | Extension Mechanism — novo PS não altera Core sem ADR |
| I-07 | Event Foundation — sem catálogo de eventos no Core |
| I-08 | Shared Kernel Types — referências, não modelos internos |
| I-09 | Auditabilidade via Audit Service |
| I-10 | Autorização obrigatória para dados sensíveis |

Detalhe: [platform-services.md](platform-services.md) §7.

---

## 6. Classification

Use [architecture-classification.md](architecture-classification.md) — árvore de 6 categorias (D-008).

---

## 7. Open Questions residuais

| ID | Assunto |
|---|---|
| OQ-C01 | Organization Context — Runtime Context vs Organization Management |
| OQ-C03 | UI composition contract — slot mínimo no Core |
| Q-009 | Implementação técnica do Extension Mechanism |

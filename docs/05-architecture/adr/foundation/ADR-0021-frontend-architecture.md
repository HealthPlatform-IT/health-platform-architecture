---
title: ADR-0021 - Frontend Architecture
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - frontend
  - UI
  - BFF
  - clinical-workspace
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-018-frontend-architecture.md
  - docs/05-architecture/frontend-architecture.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/api-strategy.md
  - docs/05-architecture/platform-security.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/adr/foundation/ADR-0016-api-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0020-platform-security.md
  - workspace/AS-018/confirmation-package.md
---

# ADR-0021 — Frontend Architecture

## Status

Accepted

---

# Contexto

UI de produto pertence a **Modules**; Platform Services **não** têm UI (ADR-0005). **M-02 Clinical Workspace** é shell (ADR-0009). Product API serve UI (ADR-0016); BFF e Auth UX estavam Deferred. **OQ-C03** pedia o contrato mínimo de composição UI no Core.

A **AS-018** confirmou D-001 a D-010 em 2026-07-14 (*"confirmado"*).

---

# Problema

Sem arquitetura frontend:

- Ambiguidade Staff vs Portal vs Admin
- Composição de módulos no shell indefinida
- Risco de AuthZ no cliente
- BFF confundido com contrato canônico

---

# Decisão

## 1. Fronteira (D-001)

UI em **Modules**. PS sem UI. Shell **sem** regra clínica.

## 2. Superfícies (D-002)

| Superfície | Papel |
|---|---|
| **Staff App** | Shell **M-02** + módulos clínicos/operacionais |
| **Patient Portal** | **M-08** (Should no MVP — ADR-0019) |
| **Admin surfaces** | M-09, M-16, Integration Admin etc. (área no Staff ou app lógico) |

## 3. Composição / OQ-C03 (D-003)

**OQ-C03 Answered:** Core expõe contrato mínimo via **Module Registry** — contributions (rotas/slots/áreas). **Host** = M-02. Core **não** embute framework UI nem design system. Enablement por tenant (Flags + Registry).

## 4. API e BFF (D-004)

**Product API** = contrato canônico. **Thin BFF** opcional (agregação Staff). GraphQL Deferred. BFF não concentra domínio nem AuthZ.

## 5. Auth UX (D-005)

Cliente: login/logout/refresh e guardas de UX. **Authorization** real no servidor (ADR-0020). UI não é perímetro de segurança.

## 6. Cliente técnico (D-006)

SPA web = padrão MVP. Mobile nativo e microfrontends obrigatórios = **não**.

## 7. Telemedicine (D-007)

UI = Operational Mode em Attendance dentro do Staff App — não app separado.

## 8. Estado no cliente (D-008)

Shell = contexto de trabalho (referências). Módulo = estado de tela. Cliente **não** é Source of Truth de domínio.

## 9. Deferred (D-009)

Framework UI · design system produto · GraphQL · mobile · storage detalhado de token · IdP.

## 10. Formalização (D-010)

Este ADR + `frontend-architecture.md`.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| UI em Platform Services | ADR-0005 |
| Framework UI no Core | Infla Core; OQ-C03 = contrato, não React |
| BFF como única API | Viola ADR-0016 |
| AuthZ só no cliente | Viola ADR-0020 |
| App Telemedicine separado | Viola Operational Mode (ADR-0009) |

---

# Consequências

## Positivas

- OQ-C03 Answered
- Base clara Staff/Portal/Admin e shell
- Alinhamento com API e Security

## Negativas / deferred

- Framework e design system ainda abertos
- Detalhe de contributions (schema) na implementação

---

# Referências

- `docs/05-architecture/frontend-architecture.md`
- `architecture-sessions/AS-018-frontend-architecture.md`
- `workspace/AS-018/`

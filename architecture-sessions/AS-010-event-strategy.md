---
id: AS-010
title: Event Strategy
status: Completed
version: 1.0.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
reviewers: []
phase: Product & Architecture Foundation
category: Product Architecture
priority: High
estimated-impact: High
tags:
  - platform-services
  - event-bus
  - event-foundation
  - Q-003
related:
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - architecture-sessions/AS-005-core-platform-boundary.md
  - ai-context/open-questions.md
  - workspace/AS-010/README.md
---

# AS-010 — Event Strategy

## Informações Gerais

| Campo | Valor |
|---|---|
| Sessão | AS-010 |
| Tema | Event Strategy |
| Status | Completed |
| Fase | Product & Architecture Foundation (encerramento Sprint 2) |
| Categoria | Product Architecture |
| Prioridade | High |
| Impacto Estimado | High |
| Responsável | Architecture Team |
| Data de início | 2026-07-03 |
| Data de encerramento | 2026-07-03 |
| Confirmação | *"confirmo o pacote"* |
| Open Question principal | **Q-003** — Answered (conceitual); broker Deferred Sprint 3 |

---

## Objetivo

Definir a **estratégia conceitual de eventos** da Health Platform — distinguindo:

- **Event Foundation** (Core — contrato limitado, ADR-0009)
- **Event Bus** (Platform Service — mecanismo de mensageria)
- **Clinical Event** e **Care Journey Start Event** (modelo hierárquico / ciclo de vida)
- **Webhook Service** e **Integration Service** (Integrar — ADR-0004)
- **Audit Service**, **Read Models** e **Communication** (consumidores)

---

## Decisões Confirmadas

D-001 Event Bus Confirmed ✅ · D-002 Foundation vs Bus ✅ · D-003 Taxonomia 3 camadas ✅ · D-004 Ownership domínio ✅ · D-005 Domínio publica ✅ · D-006 Bus só interna ✅ · D-007 Audit + Read Models ✅ · D-008 Tecnologia Deferred Sprint 3 ✅

---

## Escopo da Investigação (NR)

| NR | Tema | Artefato |
|---|---|---|
| NR-001 | Event Foundation vs Event Bus vs domínio | `event-strategy-boundary.md` ✅ |
| NR-002 | Taxonomia — Clinical Event vs platform event vs integration | Mesmo artefato ✅ |
| NR-003 | Matriz publicadores/consumidores | `domain-interactions.md` ✅ |
| NR-004 | Fronteira Webhook / Integration / Communication | `integration-boundary-draft.md` ✅ |
| NR-005 | Audit, Timeline, Analytics | `audit-readmodels-draft.md` ✅ |
| NR-006 | Pacote de confirmação | `confirmation-package.md` ✅ |

---

## Workspace

[`workspace/AS-010/`](../workspace/AS-010/README.md)

---

## Resultado (encerramento 2026-07-03)

Pacote confirmado pelo usuário (*"confirmo o pacote"*).

| Entrega | Status |
|---|---|
| ADR-0012 — Event Strategy | ✅ Accepted |
| `docs/05-architecture/event-strategy.md` | ✅ Publicado |
| `platform-services.md` v0.5.0 — Event Bus Confirmed (15º PS) | ✅ |
| `core-platform.md` — Event Foundation alinhado | ✅ |
| `read-models.md` — V-05 atualizado | ✅ |
| Q-003 (conceitual), OQ-EV01, OQ-EV02 | ✅ Answered |
| Q-003 (tecnologia broker) | Deferred Sprint 3 |
| Workspace AS-010 | ✅ Encerrado |
| Sprint 2 Event Strategy | 🟢 |

**Próximo marco:** Sprint 3 — Technical Architecture (Q-008, broker, stack).

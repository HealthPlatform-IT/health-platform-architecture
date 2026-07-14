---
title: ADR-0023 - Communication vs Notification Boundary
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - communication
  - notification
  - Q-010
  - platform-services
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-020-communication-notification.md
  - docs/05-architecture/communication-notification.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0011-document-engine.md
  - workspace/AS-020/confirmation-package.md
---

# ADR-0023 — Communication vs Notification Boundary

## Status

Accepted

---

# Contexto

ADR-0004 separa Comunicar (pessoas) de Integrar (sistemas). ADR-0005 listou **Communication Service** e **Notification Service** com fronteira provisória (externo vs in-app). **Q-010** permanecia In Analysis sem ADR dedicado.

A **AS-020** confirmou D-001 a D-008 em 2026-07-14 (*"confirmo"*). **Q-011** (FHIR Messaging) permanece Open.

---

# Problema

Sem critério definitivo:

- Ambiguïdade da palavra “notificação”
- Risco de misturar in-app com e-mail/SMS/push
- Contratos de módulos/PS instáveis

---

# Decisão

## 1. Critério (D-001)

| Serviço | Critério |
|---|---|
| **Notification Service** | Entrega **dentro da plataforma** a **usuário autenticado** (inbox, alertas in-app, realtime na sessão) |
| **Communication Service** | Entrega a **pessoas** por **canais externos** (e-mail, SMS, WhatsApp, push de device, etc.) |

Critério = **superfície de entrega** — não o tipo de conteúdo nem o módulo de origem.

## 2. Escopos (D-002)

Push mobile / e-mail / SMS / WhatsApp → **Communication** (canal externo). Inbox / toast / bell na UI logada → **Notification**.

## 3. Fan-out (D-003)

O mesmo Domain Event pode acionar **ambos** os PS quando o produto exige in-app e canal externo.

## 4. Domínio vs PS (D-004)

**Communication Domain:** políticas, preferências, Communication Consent, destinatários, histórico de negócio. **PS:** execução de entrega.

## 5. Template Service (D-005)

Permanece **independente** (ADR-0011 / Q-014). Communication e Document Engine consomem — Template **não** envia.

## 6. Integrar (D-006)

Sistema↔sistema continua **Integration / Webhook** (ADR-0004) — fora destes dois PS.

## 7. Fora de escopo (D-007)

**Q-011** FHIR Messaging — não resolvido nesta sessão.

## 8. Formalização (D-008)

Este ADR + `communication-notification.md` · **Q-010 Answered**.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Um único “Messaging Service” | Mistura superfícies e provedores |
| Push = Notification | Confunde jargão de produto com canal externo |
| Preferências só no PS | Perde ownership de domínio |

---

# Consequências

## Positivas

- **Q-010 Answered**
- Contratos claros para módulos e Event Bus fan-out

## Negativas / deferred

- Provedores e SDKs de canal = implementação
- Q-011 permanece Open

---

# Referências

- `docs/05-architecture/communication-notification.md`
- `architecture-sessions/AS-020-communication-notification.md`
- `workspace/AS-020/`

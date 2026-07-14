---
title: Communication and Notification
status: Draft
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/adr/foundation/ADR-0023-communication-notification.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md
  - architecture-sessions/AS-020-communication-notification.md
---

# Communication and Notification

> Fronteira oficial entre Communication Service e Notification Service.

Decisão formal: [ADR-0023](adr/foundation/ADR-0023-communication-notification.md) · AS-020 · **Q-010 Answered**.

---

## 1. Critério

| Serviço | Superfície |
|---|---|
| **Notification** | In-app · usuário autenticado |
| **Communication** | Canais externos à pessoa |

Comunicar ≠ Integrar (ADR-0004). Sistema↔sistema = Integration/Webhook.

---

## 2. Exemplos

| Cenário | PS |
|---|---|
| Inbox / bell no Staff App | Notification |
| E-mail / SMS / WhatsApp | Communication |
| Push no device do paciente | Communication |
| Mesmo evento → inbox + e-mail | Ambos (fan-out) |

---

## 3. Domínio e Template

- **Communication Domain** — preferências, consentimento, políticas
- **Template Service** — independente; não envia
- **Event Bus** — pode fan-out para ambos após Domain Event

---

## 4. Deferred / Open

Provedores de canal · Q-011 FHIR Messaging.

---

## 5. Referências

[ADR-0023](adr/foundation/ADR-0023-communication-notification.md) · [platform-services.md](platform-services.md) · [ADR-0004](adr/foundation/ADR-0004-communication-and-integration-separation.md)

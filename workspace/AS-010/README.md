# Workspace AS-010 — Event Strategy

**Status:** ✅ Encerrado — pacote confirmado; ADR-0012 Accepted

## Objetivo

Responder **Q-003** (Event Model conceitual e papel do Event Bus) — encerramento da Sprint 2.

## Open Questions

| ID | Pergunta | Status |
|---|---|---|
| **Q-003** (conceitual) | Event Model e Event Bus | **Answered** — ADR-0012 |
| **Q-003** (tecnologia) | Broker / protocolo | Deferred Sprint 3 (D-008) |
| **OQ-EV01** | Catálogo de eventos | **Answered** — domínio publicador |
| **OQ-EV02** | Event Bus Confirmed? | **Answered** — sim (D-001) |

## Arquivos do workspace

| Arquivo | Conteúdo | Status |
|---|---|---|
| **`event-strategy-boundary.md`** | NR-001/002 — boundary + taxonomia | ✅ |
| **`domain-interactions.md`** | NR-003 — publicadores/consumidores | ✅ |
| **`integration-boundary-draft.md`** | NR-004 — Webhook/Integration | ✅ |
| **`audit-readmodels-draft.md`** | NR-005 — Audit + Read Models | ✅ |
| `decisions.md` | D-001 a D-008 | ✅ Confirmed |
| `confirmation-package.md` | Pacote confirmado | ✅ |
| `questions.md` · `hypotheses.md` · `notes.md` | Suporte | ✅ |

## Formalização

| Entrega | Caminho |
|---|---|
| ADR-0012 | `docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md` |
| Doc oficial | `docs/05-architecture/event-strategy.md` |
| PS catálogo | `platform-services.md` v0.5.0 |

## Sessão oficial

[`architecture-sessions/AS-010-event-strategy.md`](../../architecture-sessions/AS-010-event-strategy.md)

## Próximo marco

Sprint 3 — Technical Architecture (broker, Q-008, stack).

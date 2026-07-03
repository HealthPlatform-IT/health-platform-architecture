# Pacote de Confirmação — AS-010

> **Sessão:** AS-010 — Event Strategy  
> **Status:** ✅ Confirmado pelo usuário (*"confirmo o pacote"*) — 2026-07-03  
> **Data:** 2026-07-03

---

## Resumo executivo

Estratégia de eventos da Health Platform: **Event Foundation** (Core — contrato) + **Event Bus** (PS — transporte) + **taxonomia em três camadas** para separar vocabulário clínico, eventos de domínio e mensagens na bus. Integração externa e comunicação com pessoas **permanecem fora** da bus (ADR-0004). Tecnologia de broker — **Sprint 3**.

---

## Decisões confirmadas

| ID | Decisão | Status |
|---|---|---|
| D-001 | Event Bus = Platform Service (**Confirmed** — 15º PS) | ✅ Confirmed |
| D-002 | Event Foundation (Core) vs Event Bus (PS) | ✅ Confirmed |
| D-003 | Taxonomia: hierárquico clínico · domain event · platform message | ✅ Confirmed |
| D-004 | Ownership de tipos no **domínio publicador** | ✅ Confirmed |
| D-005 | Módulo orquestra; **domínio publica** após persistir | ✅ Confirmed |
| D-006 | Event Bus só interno; Integration/Webhook = bridge externo | ✅ Confirmed |
| D-007 | Read Models assinantes; Audit assinante + API síncrona | ✅ Confirmed |
| D-008 | Tecnologia broker — **Deferred** Sprint 3 | ✅ Confirmed |

---

## Checklist de investigação

- [x] NR-001 — boundary Event Foundation vs Bus
- [x] NR-002 — taxonomia três camadas
- [x] NR-003 — domain-interactions (publicadores/consumidores)
- [x] NR-004 — Integration / Webhook / Communication
- [x] NR-005 — Audit + Read Models
- [x] NR-006 — pacote de confirmação
- [x] Confirmação do usuário

---

## Formalização (concluída)

1. ✅ **ADR-0012** — Event Strategy (Accepted)
2. ✅ `docs/05-architecture/event-strategy.md`
3. ✅ `platform-services.md` v0.5.0 — Event Bus Confirmed
4. ✅ `core-platform.md` — Event Foundation alinhado
5. ✅ `read-models.md` — V-05 atualizado
6. ✅ `open-questions.md`, `ARCHITECTURE_INDEX.md` — Sprint 2 Event Strategy 🟢
7. ✅ Encerramento AS-010

---

## Open Questions — impacto

| ID | Status |
|---|---|
| Q-003 (conceitual) | **Answered** — ADR-0012 |
| Q-003 (tecnologia) | **Deferred** — Sprint 3 |
| OQ-EV01 | **Answered** — tipos no domínio publicador |
| OQ-EV02 | **Answered** — Event Bus Confirmed |

**Próximo marco:** Sprint 3 — Technical Architecture.

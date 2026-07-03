# Decisões — AS-010

> Pacote **confirmado** pelo usuário (*"confirmo o pacote"*) — 2026-07-03. Formalizado em ADR-0012.

**Sessão:** AS-010 — Event Strategy  
**Última atualização:** 2026-07-03

---

## Status do pacote

| Campo | Valor |
|---|---|
| Decisões | 8 Confirmed |
| Investigação | NR-001 a NR-006 ✅ |
| ADR | ADR-0012 Accepted |
| Sessão | Completed |

---

## D-001 — Classificação Event Bus

**Status:** ✅ Confirmed

Event Bus = **Platform Service Confirmed** (15º PS). Implementa contrato **Event Foundation** do Core.

**OQ-EV02 Answered.**

---

## D-002 — Event Foundation vs Event Bus

**Status:** ✅ Confirmed

| Camada | Papel |
|---|---|
| **Event Foundation (Core)** | Contrato: envelope, contexto tenant, regras de publicação — **sem catálogo** (I-07) |
| **Event Bus (PS)** | Transporte: pub/sub, roteamento, entrega, retry técnico |

---

## D-003 — Taxonomia de eventos

**Status:** ✅ Confirmed

| Camada | Exemplo |
|---|---|
| **A — Hierárquico / ciclo de vida** | Clinical Event, Care Journey Start Event |
| **B — Domain Event** | `Attendance.Started`, `ClinicalArtifact.Formalized` |
| **C — Platform Message** | Envelope na bus |

---

## D-004 — Ownership de tipos

**Status:** ✅ Confirmed

Tipos e payload semântico definidos pelo **domínio publicador**. Catálogo **não** no Core.

**OQ-EV01 Answered.**

---

## D-005 — Padrão publicador/consumidor

**Status:** ✅ Confirmed

```text
Módulo → domínio valida + persiste → domínio publica → Event Bus → assinantes
```

Módulos **não** publicam eventos de negócio diretamente. Read Models **não** publicam.

---

## D-006 — Webhook / Integration / Communication

**Status:** ✅ Confirmed

Event Bus = **apenas interno**. Integration e Webhook = Integrar (ADR-0004), via bridge/handler. Communication/Notification = Comunicar — **não** na bus.

---

## D-007 — Audit e Read Models

**Status:** ✅ Confirmed

| Destino | Papel |
|---|---|
| **Clinical Timeline, Analytics** | Assinantes — atualizam projeção |
| **Audit Service** | Assinante de eventos + API síncrona para acesso/auth |
| **Search Service** | Assinante candidato — indexação |

---

## D-008 — Tecnologia deferred

**Status:** ✅ Confirmed

Broker (Kafka, RabbitMQ, etc.), serialização, tópicos físicos, ordering implementation — **Sprint 3**.

**Q-003:** Answered (parte conceitual) + nota Deferred (tecnologia).

---

## Impacto Open Questions

| ID | Status |
|---|---|
| Q-003 (conceitual) | **Answered** — ADR-0012 |
| Q-003 (tecnologia) | **Deferred** — Sprint 3 |
| OQ-EV01 | **Answered** |
| OQ-EV02 | **Answered** |

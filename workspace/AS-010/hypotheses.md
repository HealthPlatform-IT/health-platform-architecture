# Hipóteses — AS-010

> Hipóteses de investigação — **não são decisões** até confirmação do pacote.

## H-EV-001 — Event Bus como Platform Service

**Status:** ✅ Validada na investigação NR-001 — candidata a Confirmed (D-001).

Event Bus = mecanismo de mensageria assíncrona interna — **não** é domínio nem parte do Core operacional.

**Fonte:** ADR-0005, AS-005 `platform-services-boundary.md`.

## H-EV-002 — Event Foundation no Core

**Status:** ✅ Validada — I-07, D-002.

Core define **contrato** (envelope, contexto tenant, regras de publicação) — **não** catálogo de eventos nem broker.

**Fonte:** ADR-0009 I-07, `core-platform.md`.

## H-EV-003 — Três camadas de "evento"

**Status:** ✅ Validada — D-003, NR-002.

| Camada | Exemplo | Owner |
|---|---|---|
| **Conceito clínico** | Clinical Event, Care Journey Start Event | Modelo hierárquico / domínio |
| **Evento de domínio** | `ClinicalRecord.EvolutionRecorded` | Business Domain publicador |
| **Mensagem na bus** | Envelope transportado pelo Event Bus | Event Bus (mecanismo) |

**Risco:** homonímia "evento" — documentar glossário na confirmação.

## H-EV-004 — Significado vs transporte

**Status:** ✅ Validada — domínio publica; Bus transporta.

Event Bus **não interpreta** payload clínico — domínio publicador define semântica (AS-005).

## H-EV-005 — Tecnologia deferred

**Status:** ✅ Validada — D-008, Sprint 3.

AS-010 responde **modelo conceitual**; broker e protocolo → **Sprint 3** (Q-003 parcial ou nota explícita).

## H-EV-006 — Read Models assíncronos

**Status:** ✅ Validada — NR-005, alinhado `read-models.md` V-05.

Clinical Timeline e Analytics consomem eventos de domínio para projeção — não publicam regra clínica.

**Fonte:** `read-models.md`, ADR-0009 D-006.

## H-EV-007 — Audit paralelo

**Status:** ✅ Validada — consumo de eventos + API síncrona (NR-005).

Audit Service registra fatos para compliance — assinante de eventos e API síncrona para acesso/auth.

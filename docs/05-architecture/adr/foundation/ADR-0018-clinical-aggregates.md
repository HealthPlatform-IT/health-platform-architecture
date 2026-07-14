---
title: ADR-0018 - Clinical Domain Aggregates
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - aggregates
  - clinical
  - Q-004
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-016-clinical-aggregates.md
  - docs/05-architecture/clinical-aggregates.md
  - docs/04-domain/business-domains.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - docs/05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - workspace/AS-016/confirmation-package.md
---

# ADR-0018 — Clinical Domain Aggregates

## Status

Accepted

---

# Contexto

O **ADR-0001** definiu seis níveis hierárquicos conceituais. **Q-004** pedia a tradução em agregados DDD. A **AS-016** confirmou D-001 a D-010 em 2026-07-14 (*"confirmo o pacote"*).

---

# Problema

Sem aggregates:

- Risco de “god aggregate” (Journey/Prontuário único)
- Confusão Clinical Event (nível A) × Domain Event (bus)
- Fronteiras de consistência indefinidas antes do código clínico

---

# Decisão

## 1. Hierarquia ≠ aggregates 1:1 (D-001)

Os seis níveis ADR-0001 **não** mapeiam um-para-um para Aggregate Roots.

## 2. Catálogo de Aggregate Roots (D-002)

| Aggregate Root | Domínio (ADR-0008) |
|---|---|
| **Patient** | Patient Relationship |
| **CareJourney** | Care Coordination |
| **CareEpisode** | Care Coordination |
| **Attendance** | Care Delivery |
| **ClinicalEntry** | Clinical Record |
| **ClinicalOrder** | Clinical Orders |
| **ClinicalArtifact** | Clinical Documents |

## 3. Clinical Event nível A (D-003 / D-004)

- **Não** é Aggregate Root
- É entity/coleção no **Attendance** e/ou fato clínico associado ao Record
- **≠** Domain Event (ADR-0012): após persistir, o domínio pode publicar Domain Event correspondente

## 4. Ownership e tamanho (D-005 / D-006)

Ownership pelos 16 domínios. Aggregates **pequenos**; referências por **ID + tenant**; um comando altera um root por UoW (padrão).

## 5. Invariantes (D-007)

I-AGG-01 tenant em todo aggregate clínico · I-AGG-02 Journey→Patient · I-AGG-03 Episode→Journey · I-AGG-04 Attendance→Episode · I-AGG-05 Artifact/Order/Entry→Patient (+ Attendance quando aplicável) · I-AGG-06 sem carregar outro aggregate inteiro na mesma escrita · I-AGG-07 stream ordering usa id do root publicador (ADR-0017).

## 6. Não reabrir (D-008)

Regras de início de Institution Care Journey = **ADR-0007**.

## 7. Deferred (D-009)

DDL, subtypes fechados de ClinicalEntry, comandos/métodos detalhados, mapa BC completo.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| 1 nível = 1 aggregate root | Força roots artificiais (Clinical Event) |
| Prontuário/Journey únicos “god” | Consistência e performance |
| Clinical Event = Domain Event | Viola ADR-0012 |

---

# Consequências

## Positivas

- **Q-004 Answered**
- Base clara para modelagem e Event Bus streams
- Separação A/B preservada

## Negativas / deferred

- Detalhe de ClinicalEntry e DDL ainda aberto

---

# Referências

- `docs/05-architecture/clinical-aggregates.md`
- `architecture-sessions/AS-016-clinical-aggregates.md`

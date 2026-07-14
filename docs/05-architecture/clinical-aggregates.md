---
title: Clinical Aggregates
status: Stable
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/adr/foundation/ADR-0018-clinical-aggregates.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - docs/04-domain/business-domains.md
  - docs/05-architecture/event-strategy.md
  - architecture-sessions/AS-016-clinical-aggregates.md
---

# Clinical Aggregates

> Consistency boundaries DDD do cuidado clínico — tradução do Modelo Hierárquico (ADR-0001).

Decisão formal: [ADR-0018](adr/foundation/ADR-0018-clinical-aggregates.md) · AS-016 · **Q-004 Answered**.

---

## 1. Princípio

Níveis hierárquicos **≠** Aggregate Roots 1:1. Preferir aggregates pequenos; referências por ID + tenant.

---

## 2. Aggregate Roots

| Root | Domínio | Nível / papel |
|---|---|---|
| **Patient** | Patient Relationship | Paciente |
| **CareJourney** | Care Coordination | Jornada |
| **CareEpisode** | Care Coordination | Episódio |
| **Attendance** | Care Delivery | Atendimento |
| **ClinicalEntry** | Clinical Record | Conhecimento clínico (evolução, etc.) |
| **ClinicalOrder** | Clinical Orders | Ordem / prescrição |
| **ClinicalArtifact** | Clinical Documents | Artefato formal |

---

## 3. Clinical Event (nível A)

- Entity / fato no Attendance (e relação com Record)
- **Não** é Aggregate Root
- **≠** Domain Event (bus) — ver [event-strategy.md](event-strategy.md)

---

## 4. Relação (referências)

```text
Patient ← CareJourney ← CareEpisode ← Attendance
                                    ├ Clinical Events (entities)
                                    ← ClinicalEntry / Order / Artifact
```

---

## 5. Invariantes (resumo)

Todo aggregate clínico com tenant; cadeia Patient←Journey←Episode←Attendance; escritas de um root por UoW; ordering de eventos pelo id do root publicador.

---

## 6. Deferred

DDL · subtypes ClinicalEntry · comandos detalhados · mapa BC completo

---

## 7. Referências

[ADR-0018](adr/foundation/ADR-0018-clinical-aggregates.md) · [ADR-0001](adr/foundation/ADR-0001-hierarchical-care-model.md) · [business-domains.md](../04-domain/business-domains.md)

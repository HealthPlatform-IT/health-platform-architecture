---
title: Care Journey Lifecycle
status: Draft
version: 0.1.0
created: 2026-07-02
updated: 2026-07-02
author: Architecture Team
category: Business
phase: Product & Architecture Foundation
related:
  - docs/02-business-processes/healthcare-operating-model.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - docs/05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md
  - architecture-sessions/AS-003-care-journey-lifecycle.md
---

# Care Journey Lifecycle

> Ciclo de vida da **Institution Care Journey** na Health Platform — quando começa, como se mantém, pausa, encerra e reabre.

---

## 1. Purpose

Este documento define o ciclo de vida da **Institution Care Journey** — o trecho do cuidado em que uma instituição assume responsabilidade assistencial dentro da plataforma.

Responde **Q-001**: quando a instituição assume responsabilidade pelo cuidado de um paciente.

Decisão formal: [ADR-0007](../05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md).

---

## 2. Conceitos

| Conceito | Significado |
|---|---|
| **Patient Life Journey** | História ampla de saúde da pessoa — fora do escopo integral da plataforma |
| **Institution Care Journey** | História de cuidado gerenciada por uma instituição na Health Platform (= **Care Journey** no ADR-0001) |
| **Care Journey Start Event** | Evento que marca o início da responsabilidade institucional pelo cuidado |

Cadastro e vínculo em **Relacionar** são pré-requisito, mas **não iniciam** a jornada.

---

## 3. Início da Institution Care Journey

A jornada **inicia** quando a instituição registra um **Care Journey Start Event** — aceite explícito de responsabilidade por uma demanda de cuidado.

O Core define tipos de evento válidos. A instituição configura qual tipo aplica por modelo operacional (ADR-0006).

### Tipos de Care Journey Start Event

| Tipo | Descrição |
|---|---|
| `ReferralAccepted` | Encaminhamento aceito |
| `CareProgramEnrolled` | Admissão em programa de cuidado |
| `ServiceRequestAccepted` | Pedido de serviço aceito |
| `ScheduledCareCommitted` | Agendamento com compromisso institucional |
| `EmergencyCareAssumed` | Urgência — responsabilidade assumida |
| `HomeCareAdmitted` | Admissão em home care |
| `OccupationalCareEnrolled` | Ingresso em saúde ocupacional |

### Por modelo operacional (padrão configurável)

| Modelo | Start Event padrão |
|---|---|
| Ambulatory Care | `ReferralAccepted` ou `ScheduledCareCommitted` |
| Telemedicine | Igual ao ambulatorial |
| Diagnostic Care (laboratório) | `ServiceRequestAccepted` |
| Diagnostic Care (imagem) | `ServiceRequestAccepted` ou `ScheduledCareCommitted` |
| Home Care | `HomeCareAdmitted` |
| Occupational Health | `OccupationalCareEnrolled` |
| Hospital Care *(futuro)* | `EmergencyCareAssumed` — reserva conceitual |

O primeiro **Attendance** não é obrigatório para iniciar a jornada.

---

## 4. Care Journey, Care Episode e Attendance

```text
Institution Care Journey
    └── Care Episode(s)
            └── Attendance(s)
                    └── Clinical Event(s) → Clinical Artifact(s)
```

| Nível | Regra |
|---|---|
| **Care Journey** | Possui estados de ciclo de vida (Active, Paused, Closed) |
| **Care Episode** | Nasce com a jornada ou após o Start Event; condição ou objetivo assistencial |
| **Attendance** | Interação concreta; sempre dentro de um Episode |

Jornadas diagnósticas curtas com **um único episódio** são válidas.

---

## 5. Estados e transições

### Estados

| Estado | Significado |
|---|---|
| **Active** | Instituição mantém responsabilidade assistencial |
| **Paused** | Responsabilidade suspensa temporariamente |
| **Closed** | Responsabilidade encerrada |

### O que mantém Active

Episódios ou atendimentos em andamento, programas ativos, pendências monitoradas pela capability **Monitorar**.

### Pausa

Programa suspenso ou inatividade assistencial prolongada. Critério de inatividade é **configurável** por instituição.

### Encerramento

Alta assistencial, transferência, conclusão de programa, entrega de resultado diagnóstico, desistência.

### Reabertura

Após **Closed**, um novo Care Journey Start Event cria uma **nova** Institution Care Journey — o registro anterior permanece histórico.

```text
[Start Event] → Active ⇄ Paused → Closed
                                    ↓
                          [novo Start Event] → nova Journey (Active)
```

---

## 6. Cardinalidade

Por paciente e instituição: **uma Institution Care Journey Active** por vez.

Múltiplas jornadas ativas simultâneas (ex.: ocupacional + ambulatorial): ver **Q-018** em `open-questions.md`.

---

## 7. Impactos conceituais (sem implementação)

| Área | Impacto |
|---|---|
| **Modelo Hierárquico** | Estados no nível Care Journey; invariantes ADR-0001 preservados |
| **Core Business Capabilities** | Relacionar (pré-requisito), Organizar/Executar (gatilhos), Monitorar (estados) |
| **Clinical Workspace** | Reflete jornada Active e episódios em andamento |
| **Timeline clínica** | Ancorada na Institution Care Journey |
| **Eventos de negócio** *(candidatos — Q-003 Deferred)* | `InstitutionCareJourneyStarted`, `Paused`, `Closed` |

---

## 8. Related Documents

| Documento | Relação |
|---|---|
| [healthcare-operating-model.md](./healthcare-operating-model.md) | Modelo operacional e posição da jornada |
| [ADR-0001](../05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md) | Hierarquia Patient → Clinical Artifact |
| [ADR-0007](../05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md) | Decisão formal do ciclo de vida |
| [ADR-0006](../05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md) | Configuração de gatilhos por modelo |

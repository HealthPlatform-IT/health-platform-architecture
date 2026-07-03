---
title: ADR-0007 - Care Journey Lifecycle
status: Accepted
date: 2026-07-02
deciders:
  - Architecture Team
tags:
  - architecture
  - care-journey
  - lifecycle
  - foundation
  - healthcare
related:
  - architecture-sessions/AS-003-care-journey-lifecycle.md
  - docs/02-business-processes/care-journey-lifecycle.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md
  - workspace/AS-003/decisions.md
---

# ADR-0007 — Care Journey Lifecycle

## Status

Accepted

---

# Contexto

A Health Platform é orientada pela **Jornada de Cuidado**. O [ADR-0001](ADR-0001-hierarchical-care-model.md) define a hierarquia Patient → Care Journey → Care Episode → Attendance → Clinical Event → Clinical Artifact, mas **não define** quando uma Institution Care Journey começa, como se mantém ativa, pausa, encerra ou reabre.

A **AS-003 — Care Journey Lifecycle** investigou **Q-001**: quando uma instituição assume responsabilidade pelo cuidado de um paciente.

A documentação em `architecture-foundation.md` distingue **Patient Life Journey** (história ampla, multi-institucional) de **Institution Care Journey** (trecho gerenciado pela instituição na plataforma).

---

# Problema

Sem critério formal para início e ciclo de vida da Institution Care Journey:

- Cadastro pode ser confundido com responsabilidade assistencial.
- Modelos operacionais distintos (ambulatorial, diagnóstico, home care) seriam modelados de forma inconsistente.
- Business Domains, timeline clínica e Clinical Workspace carecem de âncora conceitual.
- A derivação de domínios (AS-004) ficaria bloqueada.

---

# Decisão

A Health Platform adotará as seguintes regras para o ciclo de vida da **Institution Care Journey**.

## 1. Distinção de jornadas

| Conceito | Escopo na plataforma |
|---|---|
| **Patient Life Journey** | Fora do escopo integral — a plataforma não presume conhecer toda a vida clínica |
| **Institution Care Journey** | História de cuidado gerenciada por uma instituição — equivale ao **Care Journey** do ADR-0001 no contexto institucional |

## 2. Vínculo ≠ início

Cadastro e vínculo em **Relacionar** são pré-requisito. **Não iniciam** a Institution Care Journey.

## 3. Início — Care Journey Start Event

A jornada inicia quando a instituição registra um **Care Journey Start Event** — aceite explícito de responsabilidade por uma demanda de cuidado.

O Core define tipos válidos:

- `ReferralAccepted`
- `CareProgramEnrolled`
- `ServiceRequestAccepted`
- `ScheduledCareCommitted`
- `EmergencyCareAssumed`
- `HomeCareAdmitted`
- `OccupationalCareEnrolled`

Qual tipo dispara o início por modelo operacional é **configurável** (ADR-0006). O primeiro Attendance não é obrigatório para iniciar a jornada.

## 4. Hierarquia dentro da jornada

- **Care Journey** — container com ciclo de vida.
- **Care Episode** — nasce com a jornada ou após o Start Event.
- **Attendance** — sempre dentro de um Episode.

Jornadas diagnósticas curtas com episódio único são válidas.

## 5. Estados

| Estado | Significado |
|---|---|
| **Active** | Responsabilidade assistencial ativa |
| **Paused** | Responsabilidade suspensa temporariamente |
| **Closed** | Responsabilidade encerrada |

Reabertura após Closed: novo Start Event cria **nova** Institution Care Journey.

## 6. Cardinalidade

Uma Institution Care Journey **Active** por paciente e instituição por vez. Cenários de múltiplas jornadas paralelas: **Q-018** (open question).

---

# Justificativa

- Separa vínculo cadastral de responsabilidade assistencial (evita jornadas vazias).
- Combina semântica clara (aceite explícito) com flexibilidade por modelo operacional (configuração).
- Compatível com ADR-0001 sem reabri-lo — complementa com ciclo de vida e gatilhos.
- Suporta jornadas longas (ambulatorial) e curtas (diagnóstico).

---

# Consequências

## Positivas

- Q-001 respondida; AS-004 pode prosseguir.
- Âncora para timeline clínica, Clinical Workspace e domínios clínicos.
- Vocabulário comum de Start Events no Core.

## Negativas

- Configuração por modelo operacional exige Configuration Service maduro.
- Q-018 permanece aberta para programas paralelos.

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| ADR-0001 | Hierarquia preservada; este ADR define ciclo de vida e início |
| ADR-0002 | Jornada como centro conceitual — agora com critério de início |
| ADR-0006 | Gatilhos configuráveis por instituição e modelo operacional |

---

# Documentação oficial

Resposta detalhada: [care-journey-lifecycle.md](../../../02-business-processes/care-journey-lifecycle.md)

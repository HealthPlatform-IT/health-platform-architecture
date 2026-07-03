# Notas — AS-010

> Log de investigação. Notas não são decisões.

---

## 2026-07-03 — Início da preparação

Sessão iniciada após fechamento documental Sprint 2 (commit `3934320`).

**Inputs consolidados:**
- ADR-0009 — Event Foundation (6º componente Core); I-07 sem catálogo no Core
- ADR-0005 — Event Bus Identificado; Q-003 Deferred
- AS-005 — Event Bus Strong Candidate; implementa Foundation
- Q-003 em `open-questions.md` — Recommended Session AS-010
- AS-006 — submit MFE pode gerar evento pós-domínio (futuro)
- AS-007 — formalização DE pode gerar `ClinicalArtifact.Formalized` (exemplo conceitual)

**Próximo:** expandir `event-strategy-boundary.md` (NR-001/002) e matriz NR-003.

---

## Tensões iniciais

| ID | Tensão | Status |
|---|---|---|
| T-EV-01 | Clinical Event (hierarquia) vs platform event | ✅ D-003 |
| T-EV-02 | Catálogo de tipos — onde registrar | ✅ D-004 — domínio publicador |
| T-EV-03 | Event Bus Confirmed vs Strong | ✅ D-001 — Confirmed candidato |
| T-EV-04 | Q-003 Answered vs Partial (tech Sprint 3) | ✅ D-008 — conceitual agora |
| T-EV-05 | Audit síncrono vs via evento | ✅ D-007 — ambos (NR-005) |

---

## 2026-07-03 — NR-003 a NR-006 concluídos

**Artefatos produzidos:**

| NR | Arquivo | Conteúdo |
|---|---|---|
| NR-003 | `domain-interactions.md` | Matrizes publicador/consumidor; fluxos F-01 a F-04 |
| NR-004 | `integration-boundary-draft.md` | Bridge interno→externo; D-006 |
| NR-005 | `audit-readmodels-draft.md` | Timeline, Analytics, Audit padrões A/B, Search |
| NR-006 | `confirmation-package.md` | Pacote Ready for Confirmation |

**Decisões D-001 a D-008:** promovidas para Ready for Confirmation.

**Próximo:** confirmação do usuário → ADR-0012 + formalização docs.

---

## 2026-07-03 — Pacote confirmado e formalizado

Usuário: *"confirmo o pacote"*.

| Entrega | Status |
|---|---|
| ADR-0012 Accepted | ✅ |
| `event-strategy.md` | ✅ |
| `platform-services.md` v0.5.0 — Event Bus Confirmed (15º) | ✅ |
| `core-platform.md`, `read-models.md` | ✅ |
| Q-003 (conceitual), OQ-EV01, OQ-EV02 | ✅ Answered |
| AS-010 Completed | ✅ |
| Sprint 2 Event Strategy | 🟢 |

**Próximo marco:** Sprint 3 — Technical Architecture.

---

## Riscos (AS-005 herdados)

| ID | Risco | Mitigação proposta |
|---|---|---|
| R-EV-01 | Catálogo de eventos no Core | I-07 — manter fora |
| R-EV-02 | Event Bus no Core | PS dedicado |
| R-EV-03 | Bus interpreta regra clínica | Domínio publica; Bus transporta |
| R-EV-04 | Confundir com Webhook externo | ADR-0004 + NR-004 |

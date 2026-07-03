# Notas — AS-006

> Log de investigação. Notas não são decisões.

---

## 2026-07-03 — Preparação da sessão

Sessão preparada após encerramento e commit de fechamento da **AS-005** (ADR-0009, Q-007 Answered).

**Inputs consolidados:**

- Medical Form Engine = Strong Candidate PS (ADR-0009 D-003)
- ADR-0005 — Identificado, capability Registrar
- OQ-PS05 — Needs Review até AS-006
- Consumidores: M-03, M-04 (module-strategy.md)
- Clinical Record Domain — ownership de conhecimento clínico
- R-MFE-01 herdado de AS-005 (Engine virar prontuário)

**Primeira ação planejada:** NR-001 — `medical-form-engine-boundary.md`

---

## Tensões registradas

| ID | Tensão | Status |
|---|---|---|
| T-MFE-01 | Form definition — Engine vs domínio | ⚪ Pendente |
| T-MFE-02 | M-03 captura vs M-04 registro — mesmo Engine? | ⚪ Pendente |
| T-MFE-03 | Template Service overlap (Q-014) | ⚪ Pendente |
| T-MFE-04 | Document Engine overlap (AS-007) | ⚪ Pendente |

---

## Riscos da sessão

| ID | Risco | Mitigação |
|---|---|---|
| R-MFE-01 | Engine vira prontuário | H-MFE-006; ownership no domínio |
| R-MFE-02 | Regra clínica no schema do Engine | Separar validação estrutural vs clínica |
| R-MFE-03 | Sessão absorve AS-007 | NR-005 apenas fronteira |
| R-MFE-04 | Sessão resolve Q-014 inteira | NR-004 parcial apenas |
| R-MFE-05 | Reabrir fronteiras AS-005 | Escopo fechado em README |

---

## Referências cruzadas

- AS-005 `platform-services-boundary.md` § Medical Form Engine
- AS-005 `module-strategy-draft.md` — M-03, M-04
- ADR-0005 § Medical Form Engine
- `docs/04-domain/business-domains.md` — Clinical Record
- `architecture-classification.md` — critérios PS

---

## Log de investigação

_(Adicionar entradas por NR conforme avanço)_

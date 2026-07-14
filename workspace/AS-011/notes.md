# Notas — AS-011

> Log de investigação. Notas não são decisões.

---

## 2026-07-04 — Kickoff Sprint 3 / AS-011

Usuário confirmou preparação da Sprint 3 (*"confirmado"*).

**Decisões de kickoff (processo, não técnicas):**

- Sprint 3 iniciada
- AS-010 **não** reaberta (ADR-0012 permanece)
- Primeira sessão: **AS-011 — Multi-Tenant Strategy** (Q-008)
- Sem ADR até confirmação do pacote da sessão
- Sem código, sem stack, sem schemas físicos

**Inputs consolidados:**

- Tenant Context + I-01 / Runtime Context + I-02 (ADR-0009)
- Identity = sessões e contexto multi-tenant (ADR-0005)
- Config por tenant (ADR-0006)
- Envelope de evento com tenant (ADR-0012)
- OQ-C01 aberta em core-platform.md

**Próximo:** expandir `multi-tenant-boundary.md` e NR-003 propagação.

---

## Tensões iniciais

| ID | Tensão | Status |
|---|---|---|
| T-MT-01 | Tenant vs Organization vs Institution | ✅ D-002 |
| T-MT-02 | Shared vs silo de dados | ✅ D-003 |
| T-MT-03 | OQ-C01 Organization no Core vs domínio | ✅ referência ID |
| T-MT-04 | Q-008 Answered vs detalhe DDL Deferred | ✅ D-007 |
| T-MT-05 | Cross-tenant (plataforma) vs isolamento estrito | ✅ NR-003 |

---

## 2026-07-14 — NR-003 a NR-006 + pacote Ready for Confirmation

Cartão Trello trabalhado: [FUNDAÇÃO] Fechar AS-011 (YqM1DTgx).

## 2026-07-14 — Confirmação e formalização

- Usuário: *"Confirmado o pacote"*
- ADR-0013 Accepted; `multi-tenant-strategy.md`; Q-008 Answered
- Cartão Trello → **08 — Concluído**
- Próximo: Backend / Database / API · AS-009 Security

**Artefatos:**

| NR | Arquivo |
|---|---|
| NR-003 | `context-propagation-draft.md` |
| NR-004 | `configuration-tenancy-draft.md` |
| NR-005 | `data-isolation-draft.md` |
| NR-006 | `confirmation-package.md` + `decisions.md` Ready for Confirmation |

**Sem ADR criado** — aguarda *"confirmo o pacote"*.

**Próximo:** validação do usuário.

---

## Riscos

| ID | Risco | Mitigação proposta |
|---|---|---|
| R-MT-01 | Escolher produto de banco cedo demais | Critérios apenas; produto em sessão Database |
| R-MT-02 | Colocar isolamento no Core | Core = contrato; isolamento = estratégia técnica |
| R-MT-03 | Esquecer tenant em eventos | ADR-0012 — envelope com tenant |
| R-MT-04 | Customização por tenant em código | ADR-0006 — proibido como padrão |

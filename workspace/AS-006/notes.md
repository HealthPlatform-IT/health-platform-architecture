# Notas — AS-006

> Log de investigação. Notas não são decisões.

---

## 2026-07-03 — Início da sessão (confirmado pelo usuário)

Preparação AS-006 confirmada. Sessão iniciada.

**Inputs consolidados:**
- ADR-0009 — Medical Form Engine Strong Candidate, OQ-PS05 Needs Review
- ADR-0005 — Identificado, capability Registrar/Executar
- module-strategy — M-03, M-04 consumidores; M-05 não listado
- business-domains — Clinical Record ownership clínico
- AS-005 platform-services-boundary — recomendação provisória

---

## Tensões

| ID | Tensão | Status |
|---|---|---|
| T-MFE-01 | Form definition — Engine vs domínio | ✅ Resolvida — Engine |
| T-MFE-02 | M-03 vs M-04 — mesmo Engine? | ✅ Resolvida — sim, contextos distintos |
| T-MFE-03 | Template Service overlap | 🟡 D-007 em investigação |
| T-MFE-04 | Document Engine overlap | 🟡 D-008 esboço |
| T-MFE-05 | Submit Orders vs Record | ✅ Resolvida — destino por tipo de formulário (E-11) |
| T-MFE-06 | M-06 consome diretamente? | ⚪ Provisório ○ |
| T-MFE-07 | Extension definitions | Hipótese: sim via registry |

---

## Log NR

### NR-001 / NR-002 — boundary + três artefatos (2026-07-03)

`medical-form-engine-boundary.md` expandido:
- Definição e critério PS
- 13 exemplos classificados (E-01 a E-13)
- OQ-PS05 resolvida investigativamente — PS dedicado
- D-001, D-002 candidatas Ready for Confirmation

### NR-003 — domain-interactions (2026-07-03)

Matriz domínios e módulos. **Descoberta:** M-05 Orders como consumidor e Clinical Orders como destino de submit (E-11). Proposta atualizar module-strategy pós-confirmação.

### NR-004 — validation-strategy (2026-07-03)

Taxonomia V-01 a V-11. D-003 Ready for Confirmation.

### NR-005 — versioning-strategy (2026-07-03)

Definition imutável pós-publicação. D-004 Ready for Confirmation.

### NR-006 — configuration-model (2026-07-03)

ADR-0006 aplicado. D-005 Ready for Confirmation.

### NR-007 / NR-008 — confirmados com pacote

D-007 e D-008 confirmados (parcial/esboço).

---

## Confirmação final (2026-07-03)

**Confirmado pelo usuário** (*"Confirmo o pacote AS-006"*):

- D-001 a D-008 Confirmados
- ADR-0010 Accepted
- Formalização executada
- Sessão encerrada

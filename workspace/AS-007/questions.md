# Perguntas — AS-007

> Sessão encerrada 2026-07-03 — perguntas respondidas por ADR-0011.

## Q-013 — Document Engine

| # | Pergunta | Resposta (ADR-0011) |
|---|---|---|
| 1 | O que é Document Engine? | PS Confirmed de geração/renderização formal |
| 2 | Por que é Platform Service? | Transversal — M-05 e M-06 consomem |
| 3 | Engine vs Clinical Documents? | Engine renderiza; domínio possui regras e artefato |
| 4 | Relação com Medical Form Engine? | Captura ≠ documento formal (D-006) |
| 5 | Relação com Template Service? | Template independente; Engine consome (D-005) |
| 6 | Dono do Clinical Artifact? | Clinical Documents Domain |
| 7 | Regra vs renderização? | Engine valida render; domínio valida documento (D-003) |
| 8 | M-05 e M-06 — mesmo contrato? | Sim — contrato único de geração |
| 9 | Assinatura digital? | Clinical Documents — implementação futura (D-008) |
| 10 | Fase técnica? | APIs, PDF, certificado — fora de escopo |

**Status:** ✅ **Answered** — ADR-0011

## OQ-PS06

Document Engine — PS ou parte de Clinical Documents?

**Resposta:** PS dedicado (D-001). **Status:** ✅ Answered

## Q-014

Template independente?

**Resposta:** Sim — Template Service permanece independente (D-005). **Status:** ✅ Answered

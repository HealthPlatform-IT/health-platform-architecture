# Decisões — AS-007

> Pacote confirmado pelo usuário em 2026-07-03 (*"confirmo o pacote"*).

**Sessão:** AS-007 — Document Engine  
**Status:** ✅ Encerrada  
**Última atualização:** 2026-07-03

---

## Status do pacote

| Campo | Valor |
|---|---|
| Decisões | 8 Confirmadas |
| ADR | ADR-0011 Accepted |
| Sessão | Encerrada |

---

## D-001 — Platform Service Confirmed

**Status:** ✅ Confirmado

Document Engine = **Platform Service Confirmed** (14º PS). Não é Clinical Documents Domain.

**OQ-PS06 Answered.**

---

## D-002 — Três artefatos

**Status:** ✅ Confirmado

Document Template (Template PS) → Generation Instance (Engine) → Clinical Artifact (Clinical Documents).

---

## D-003 — Render vs regra de documento

**Status:** ✅ Confirmado

Engine: renderização. Domínio: regra e ciclo de vida do artefato.

---

## D-004 — Consumidores

**Status:** ✅ Confirmado

| Módulo | Uso |
|---|---|
| M-05 Orders | Receita, solicitação formal |
| M-06 Documents | Laudo, atestado, termo, relatório |

---

## D-005 — Template Service (Q-014)

**Status:** ✅ Confirmado

Template Service **independente**; Engine consome templates. **Q-014 → Answered**.

---

## D-006 — Fronteira Medical Form Engine

**Status:** ✅ Confirmado

Captura (MFE) ≠ geração formal (DE). Alinhado ADR-0010 D-008.

---

## D-007 — Versionamento

**Status:** ✅ Confirmado

Template version (Template PS); artifact imutável pós-formalização (Clinical Documents).

---

## D-008 — Assinatura digital

**Status:** ✅ Confirmado (conceitual)

Assinatura e validade legal = **Clinical Documents** + capacidade futura — **fora de implementação** nesta sessão.

---

## Impacto Open Questions

| ID | Status |
|---|---|
| Q-013 (Document Engine) | **Answered** — ADR-0011 |
| Q-013 (global) | **Answered** — ADR-0010 + ADR-0011 |
| Q-014 | **Answered** — ADR-0011 D-005 |
| OQ-PS06 | **Answered** — ADR-0011 D-001 |

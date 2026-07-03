# Decisões — AS-006

> Pacote confirmado pelo usuário em 2026-07-03 (*"Confirmo o pacote AS-006"*).

**Sessão:** AS-006 — Medical Form Engine  
**Status:** ✅ Encerrada  
**Última atualização:** 2026-07-03

---

## Status do pacote

| Campo | Valor |
|---|---|
| Decisões | 8 Confirmadas |
| ADR | ADR-0010 Accepted |
| Sessão | Encerrada |

---

## D-001 — Classificação: Platform Service Confirmed

**Status:** ✅ Confirmado

Medical Form Engine = **Platform Service Confirmed** (13º PS). Não é Business Domain, Module, Read Model nem Core.

**OQ-PS05 Answered.**

---

## D-002 — Modelo de três artefatos

**Status:** ✅ Confirmado

Form Definition (Engine) → Form Instance (runtime) → Clinical Content (domínio destino).

---

## D-003 — Separação forma vs conteúdo clínico

**Status:** ✅ Confirmado

Engine: validação estrutural. Domínio: validação clínica.

---

## D-004 — Versionamento

**Status:** ✅ Confirmado

Definition imutável pós-publicação; instance referencia version; content independente.

---

## D-005 — Configuração

**Status:** ✅ Confirmado

Configuration Service + metadados; sem customização por código.

---

## D-006 — Consumidores

**Status:** ✅ Confirmado

M-03, M-04, M-05, M-14, M-15. Destinos: Clinical Record (primário), Clinical Orders (E-11).

---

## D-007 — Template Service

**Status:** ✅ Confirmado (parcial)

Engine = estrutura; Template = layout textual. Q-014 permanece Partial.

---

## D-008 — Document Engine

**Status:** ✅ Confirmado (esboço)

Captura vs documento formal. Detalhe em ADR-0011 (AS-007).

---

## §4 Formalização

| Artefato | Status |
|---|---|
| **ADR-0010** | ✅ Accepted |
| `medical-form-engine.md` | ✅ Criado |
| `platform-services.md` v0.3.0 | ✅ Atualizado |
| `module-strategy.md` v0.2.0 | ✅ Atualizado |
| `open-questions.md` | ✅ Atualizado |
| `architecture-foundation.md` | ✅ Atualizado |
| `adr-summary.md` | ✅ Atualizado |
| `ARCHITECTURE_INDEX.md` | ✅ Atualizado |
| Sessão AS-006 | ✅ Encerrada |

---

## Impacto Open Questions

| ID | Status |
|---|---|
| Q-013 (MFE) | **Answered** |
| Q-013 (Document) | **Answered** — ADR-0011 (AS-007) |
| OQ-PS05 | **Answered** |
| Q-014 | Partial |

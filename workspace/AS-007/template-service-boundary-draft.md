# Template Service Boundary — Document Engine

> **Promovido para documentação oficial:** `docs/05-architecture/document-engine.md` (ADR-0011).  
> Este arquivo permanece como registro histórico da investigação NR-005 (Q-014).

**Última atualização:** 2026-07-03

---

## Problema Q-014

Template Service deve permanecer **independente** ou ser absorvido pelo Document Engine ou Communication Service?

---

## Decisão investigativa (D-005)

| Serviço | Responsabilidade |
|---|---|
| **Template Service** | Catálogo, versionamento e variáveis de **templates** (mensagens e documentos) |
| **Document Engine** | **Renderização** — consome template + dados → artefato formal |
| **Communication Service** | Envio — consome template para mensagens |

**Template Service permanece independente (Confirmed ADR-0005).**

Document Engine **não substitui** Template Service — **consome** templates por contrato.

---

## Overlap resolvido

| Item | Owner |
|---|---|
| Storage de template | Template Service |
| Variáveis `{{patientName}}` | Template Service |
| Merge + render PDF/HTML | Document Engine |
| Template de e-mail | Template Service + Communication |

---

## D-005 — Confirmada

✅ Confirmada em AS-007 — ADR-0011 D-005. Q-014 Answered.

**Proposta confirmada:** Q-014 → **Answered** — Template independente; Document Engine e Communication consomem.

*Detalhe de implementação (template único vs duplicado) — fase técnica.*

---

## Anti-pattern

Absorver Template no Document Engine — **rejeitado** (Communication também precisa de templates).

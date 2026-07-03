# Versioning Strategy Draft — Document Engine

> **Promovido para documentação oficial:** `docs/05-architecture/document-engine.md` (ADR-0011).  
> Este arquivo permanece como registro histórico da investigação NR-006.

---

## O que versionar

| Entidade | Versionada? | Owner |
|---|---|---|
| **Document Template** | Sim | Template Service |
| **Render output** | Referência fixa | Engine (generationId) |
| **Clinical Artifact** | Ciclo domínio | Clinical Documents |

Alterar template **não reescreve** artefatos já formalizados — novo generate usa nova template version.

---

## D-007 — Confirmada

✅ Confirmada em AS-007 — ADR-0011 D-007.

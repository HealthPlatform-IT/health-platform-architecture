# Validation Strategy Draft — Document Engine

> **Promovido para documentação oficial:** `docs/05-architecture/document-engine.md` (ADR-0011).  
> Este arquivo permanece como registro histórico da investigação NR-006.

---

## Taxonomia

| Tipo | Owner | Exemplo |
|---|---|---|
| V-01 Render — dados completos | Engine | Campos obrigatórios para merge |
| V-02 Render — formato saída | Engine | PDF válido sintaticamente |
| V-03 Documento — regra clínica | **Clinical Documents** | Laudo exige CID |
| V-04 Documento — ciclo de vida | **Clinical Documents** | Rascunho → assinado |
| V-05 Ordem — prescrição válida | **Clinical Orders** | Antes de gerar receita |
| V-06 Assinatura | **Clinical Documents** *(futuro)* | Certificado, carimbo |

**Regra:** Engine valida **renderização**; domínios validam **documento clínico/ legal**.

---

## D-003 — Confirmada

✅ Confirmada em AS-007 — ADR-0011 D-003.

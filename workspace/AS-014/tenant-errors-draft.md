# Tenant, Correlation & Errors

> Rascunho AS-014 — NR-004.

---

## 1. Tenant Context

- Toda chamada Product/Platform exige Runtime Context resolvido (ADR-0013)
- Paths públicos (login, health) = lista explícita (AS-009)
- PS **não** inventa tenant; propaga o do caller
- Integration inbound mapeia identidade externa → tenant via Integration/Identity

---

## 2. Correlation

| Artefato | Uso |
|---|---|
| **Request-Id / Correlation-Id** | Obrigatório no pipeline; ecoado na resposta e no Audit |
| **Trace** | Observability PS (detalhe Deferred) |

---

## 3. Erros (contrato mínimo)

Resposta de erro deve permitir:

- código estável (machine-readable)
- mensagem segura (sem vazar dados de outro tenant)
- request/correlation id
- HTTP status semântico adequado

Formato JSON exato = Deferred (OpenAPI).

---

## 4. Deferred

OpenAPI completo · BFF · AuthZ fine-grained · rate limit policies · GraphQL

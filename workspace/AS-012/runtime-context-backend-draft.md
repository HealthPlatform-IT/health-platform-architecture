# Runtime Context no Backend

> Rascunho AS-012 — NR-004. Candidato a confirmação — alinhado ADR-0013 — **não é ADR**.

---

## 1. Regra herdada (não reabrir)

- **I-01 Tenant Context** obrigatório
- **I-02 Runtime Context** agrega tenant + organization (ref) + user + escopo
- Identity (PS) estabelece sessão; backend **propaga**, não redefine Tenant
- Sem tenant → rejeitar operação sensível (exceto path público documentado — Security)

---

## 2. Pontos de entrada (candidato)

| Entrada | Obrigação |
|---|---|
| HTTP/API request | Extrair/validar contexto antes do use case |
| Event consumer | Envelope com tenant; rejeitar/miss se ausente |
| Scheduled job / worker | Contexto explícito por tenant ou job scoped |
| Admin cross-tenant | **Proibido** na bus; só superfície Security/ops (AS-009) |

---

## 3. Pipeline mínimo

```text
Inbound
  → Identity / token validation (PS)
  → Resolve Runtime Context
  → Authorization check (PS)  [detalhe AS-009]
  → Application use case (tenant no scope)
  → Domain + persistence (discriminator)
  → Publish Domain Event (tenant no envelope)
```

Backend Architecture fixa o **pipeline**; policies AuthZ detalhadas = AS-009.

---

## 4. Relação Persistence

- Application/Domain **sempre** operam sob tenant resolvido.
- Adapter de persistência aplica isolamento shared+discriminator (DDL = Database Architecture).
- Testes futuros: suite multi-tenant obrigatória (critério, não implementação agora).

---

## 5. Deferred

| Tema | Sessão |
|---|---|
| Claims JWT / headers | AS-009 + API Strategy |
| RLS policies vendor | Database Architecture |
| Impersonation / suporte | AS-009 |

---

## Próximo

**NR-005** — `decisions.md` + `confirmation-package.md`.

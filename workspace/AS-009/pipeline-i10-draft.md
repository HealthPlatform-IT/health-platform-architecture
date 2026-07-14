# NR-003 — Pipeline e I-10

> Rascunho AS-009. Candidato a confirmação.

## 1. Pipeline obrigatório

```text
Inbound
  → Identity (AuthN) — sessão / credencial
  → Runtime Context — tenant obrigatório (+ user, org ref, correlation)
  → Authorization (AuthZ) — deny-by-default
  → Application / Domain
  → Persistência / resposta
  → Audit (ações relevantes + acessos sensíveis)
  → Domain Event (tenant no envelope) quando aplicável
```

Alinha ADR-0014 / ADR-0016. AuthZ **não** fecha via Event Bus no request sync.

## 2. Paths públicos (allowlist)

Permitidos **sem** sessão de usuário de produto, sob governança:

| Tipo | Exemplos (conceito) |
|---|---|
| Health / readiness | `/health`, `/ready` |
| Well-known / discovery mínimos | metadata pública necessária |

Qualquer outro path Product/Platform exige AuthN + tenant + AuthZ.

## 3. I-10 — Dados sensíveis

**Invariante Core:** autorização obrigatória para dados sensíveis.

| Classe | Exemplos | Regra |
|---|---|---|
| **Clínico / PHI** | Record, Orders, Documents, Timeline, Attendance | AuthZ + Audit de acesso |
| **Identidade do paciente** | Demographics Relacionar | AuthZ no escopo do tenant/org |
| **Cross-tenant** | Operação plataforma | AuthZ plataforma + Audit obrigatório |
| **Não sensível operacional** | health check | Allowlist |

**Princípio:** se há dúvida se é sensível → tratar como sensível (AuthZ + Audit).

## 4. Audit vs Observability

| | Audit | Observability |
|---|---|---|
| Propósito | Rastreabilidade de negócio / acesso | Saúde técnica |
| Quem | Audit Service | Observability Service |
| Exemplo | “usuário X leu registro Y” | latência p99, erro 5xx |

Leitura de dados sensíveis e decisões cross-tenant → **Audit**.

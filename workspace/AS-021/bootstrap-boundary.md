# Bootstrap Boundary — AS-021

> Rascunho de workspace — **não** é ADR.

## O que esta sessão decide

| Decide | Não decide |
|---|---|
| Linguagem + runtime backend do MVP | Microsserviços vs monolith (já ADR-0014) |
| Stack frontend Staff (shell M-02) | Design system / microfrontends |
| DB produto inicial | Schema-per-tenant (já rejeitado como padrão) |
| Monorepo vs multi-repo app | Onde vive o repo de arquitetura (já existe) |
| Roles Must mínimos (3–6) | Matriz AuthZ completa / ABAC |
| CI mínimo (princípios ADR-0022) | Produto APM / IaC cloud final |

## Critérios herdados (não reabrir)

- Modular monolith (ADR-0014)
- Shared DB + `tenant_id` (ADR-0013 / ADR-0015)
- Sync ≠ Domain Event; Outbox (ADR-0016 / ADR-0017)
- Deny-by-default; AuthN/AuthZ/Audit em PS (ADR-0020)
- Shell Composition M-02 (ADR-0021)
- Observability ≠ Audit; sem PHI em logs (ADR-0022)

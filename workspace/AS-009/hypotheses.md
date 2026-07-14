# Hipóteses — AS-009

| ID | Hipótese | Estado |
|---|---|---|
| H-01 | Identity = AuthN + sessão + User/Tenant Context; AuthZ = decisão de permissão | Candidata |
| H-02 | Modelo AuthZ = Role + Permission + escopo (tenant/org/recurso) — deny-by-default | Candidata |
| H-03 | Pipeline único: Authenticate → Runtime Context → Authorize → (use case) → Audit | Candidata |
| H-04 | Dados clínicos / PHI e cross-tenant exigem AuthZ explícita (I-10) | Candidata |
| H-05 | Paths públicos = allowlist mínima (health/readiness/well-known) | Candidata |
| H-06 | Secrets: sem commit/config de domínio; vault/produto Deferred | Candidata |
| H-07 | Q-019 Compliance permanece Open / Later — fora do ADR desta sessão | Candidata |
| H-08 | SSO/IdP produto Deferred; contrato Identity permanece | Candidata |

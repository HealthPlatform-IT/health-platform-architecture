# NR-004 — Secrets e Compliance

> Rascunho AS-009. Candidato a confirmação.

## 1. Secrets — princípios (agora)

| Princípio | Regra |
|---|---|
| S-01 | Secrets **não** vivem em repositório, ADRs nem config de domínio |
| S-02 | Credenciais de integração / DB / IdP ficam fora do monólito de domínio |
| S-03 | Rotação e cofre = **Deferred** (produto / DevOps session) |
| S-04 | Environment se taps não substituem política de vault em produção |

## 2. O que **não** decidimos nesta sessão

- Produto IdP / SSO (Keycloak, Auth0, Cognito, …)
- Produto vault / KMS
- Matriz completa de roles MVP em YAML
- Motor de políticas (OPA, Casbin, …)
- **Q-019 Compliance Service** — permanece Needs Review / Open
- Frontend Auth UX — Frontend Architecture

## 3. Q-019

Compliance Service **não** é promovido a Confirmed aqui. Governance & Compliance Domain permanece dono de políticas; PS de enforcement atuais = Identity / AuthZ / Audit / Config.

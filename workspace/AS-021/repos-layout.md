# Repos Layout — AS-021

## D-006 — Organização

| Opção | Avaliação |
|---|---|
| **Monorepo de aplicação (proposta)** | Um repo `health-platform` (ou nome equivalente) com `apps/` + `packages/`; boundaries por pasta = modules/PS |
| Multi-repo (api / web / libs) | Mais CI/custo de versão no MVP |

**Proposta:**

```text
health-platform-architecture/     # já existe — só docs (este repo)
health-platform/                  # NOVO — aplicação (após ADR)
  apps/
    api/                          # modular monolith backend
    staff-web/                    # SPA Staff (shell M-02)
    # portal-web, admin-web — depois
  packages/
    domain-contracts/             # types/contratos compartilhados (sem lógica clínica)
    …                             # libs internas mínimas
```

Arquitetura continua **somente** em `health-platform-architecture`.  
Código de app **não** entra no repo de arquitetura.

## D-007 — CI mínimo (ADR-0022)

No monorepo app (após ADR):

1. Install · lint · test · build
2. Scan básico de secrets
3. Artefato versionado
4. Ambiente `local` (compose) + `dev` (placeholder)

Produto CI (GitHub Actions vs outro) = **GitHub Actions** como proposta padrão se o host for GitHub.

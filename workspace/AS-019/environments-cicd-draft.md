# NR-002 — Ambientes e CI/CD

> Rascunho AS-019. Candidato a confirmação.

## 1. Ambientes mínimos

| Ambiente | Papel |
|---|---|
| **local / dev** | Desenvolvimento; dados não-prod |
| **staging** | Validação pré-prod; configuração próxima de prod |
| **production** | Tráfego real / tenants reais |

Ambientes adicionais (QA, sandbox customer) = opcionais — não bloqueiam o modelo.

**Regras:**
- Dados clínicos reais **não** em local/dev por padrão
- Secrets e configs **por ambiente** (Configuration/Feature Flag ≠ secrets)
- Tenant Context respeitado em todos os ambientes de runtime

## 2. Deployables (alinhado ADR-0014)

| Unidade | Conteúdo |
|---|---|
| **platform-api** | Modules + Domains + PS in-proc + porta Event Bus |
| **workers** (opcional) | Consumers/jobs — mesmo pipeline de tenant |
| **web** (Staff/Portal) | Frontends SPA (ADR-0021) |

Split adicional só com critérios ADR-0014 (≥2).

## 3. CI/CD princípios

| Fase | Exigência mínima (conceito) |
|---|---|
| **CI** | Build · testes automatizados · lint/análise estática · sem secrets no artefato |
| **Gate** | Staging verde antes de prod (quando aplicável) |
| **CD** | Promoção controlada; rollback possível; versionamento de artefato |
| **Migrações** | Alinhadas a database (ADR-0015) — orderadas, reversíveis quando possível |

**Não decide:** GitHub Actions vs GitLab vs outro; blue/green vs canary produto.

## 4. Infra como código (critério)

Mudanças de infra preferencialmente versionadas. Produto IaC Deferred.

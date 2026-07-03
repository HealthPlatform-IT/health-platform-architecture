# health-platform-architecture

Arquitetura, visão de produto, documentação técnica e decisões da **Health Platform**.

> Repositório de **documentação** — sem código de aplicação nesta fase (Sprint 3 não iniciada).

## Estado atual

| Item | Valor |
|---|---|
| Sprint | Fechamento Sprint 2 — **AS-010 Event Strategy** pendente |
| ADRs Foundation | 11 Accepted (0001–0011) |
| Architecture Sessions | AS-001 a AS-007 concluídas |
| Próximo marco | AS-010 (Q-003) → Sprint 3 Technical Architecture |

## Comece aqui

1. **[ARCHITECTURE_INDEX.md](ARCHITECTURE_INDEX.md)** — painel de controle do projeto
2. **[docs/00-introduction/principles.md](docs/00-introduction/principles.md)** — princípios arquiteturais
3. **[ai-context/architecture-foundation.md](ai-context/architecture-foundation.md)** — contexto completo para IA
4. **[ai-context/open-questions.md](ai-context/open-questions.md)** — o que ainda não foi decidido

## Fluxo arquitetural

```text
Architecture Session → Workspace Draft → Decision → ADR → Documentação Oficial → AI Context → Implementação
```

## Documentação principal

| Área | Caminho |
|---|---|
| ADRs Foundation | `docs/05-architecture/adr/foundation/` |
| Core Platform | `docs/05-architecture/core-platform.md` |
| Platform Services | `docs/05-architecture/platform-services.md` |
| Engines Registrar | `medical-form-engine.md`, `document-engine.md` |
| Módulos | `docs/05-architecture/module-strategy.md` |
| Domínios | `docs/04-domain/business-domains.md` |

## AI Context

| Documento | Uso |
|---|---|
| [architecture-foundation.md](ai-context/architecture-foundation.md) | Fundação para assistentes |
| [development-guidelines.md](ai-context/development-guidelines.md) | Regras antes de propor código |
| [adr-summary.md](ai-context/adr-summary.md) | Resumo dos ADRs |

## Repositório raiz

Regras para IA no monorepo: [`../CLAUDE.md`](../CLAUDE.md) e [`.cursor/rules/health-platform.mdc`](../.cursor/rules/health-platform.mdc).

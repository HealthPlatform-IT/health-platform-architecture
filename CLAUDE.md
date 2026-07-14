# Health Platform — Contexto para IA

Repositório de **arquitetura e documentação** — não contém código de aplicação. O 1º código de app depende de **AS-021 Technical Bootstrap**.

## Leitura obrigatória (nesta ordem)

1. [ARCHITECTURE_INDEX.md](ARCHITECTURE_INDEX.md) — estado do projeto
2. [ai-context/architecture-foundation.md](ai-context/architecture-foundation.md) — fundação completa
3. [ai-context/open-questions.md](ai-context/open-questions.md) — o que **não** está decidido (+ gate vs 1º código)
4. [ai-context/development-guidelines.md](ai-context/development-guidelines.md) — regras para propor mudanças

## ADRs e decisões

- Série oficial: `docs/05-architecture/adr/foundation/` — **0001–0023 Accepted**
- Pasta `adr/` = **legado não autoritativo**
- Resumo: [ai-context/adr-summary.md](ai-context/adr-summary.md)

### Marcos relevantes

| ADR | Tema |
|---|---|
| 0009 | Core Platform Boundary |
| 0010–0011 | Medical Form / Document Engines |
| 0012 / 0017 | Event Strategy (conceito + critérios tech) |
| 0013–0016 | Multi-Tenant · Backend · Database · API |
| 0018–0019 | Clinical Aggregates · MVP Scope |
| 0020–0023 | Security · Frontend · DevOps · Comm vs Notification |

## Classificação

Antes de propor domínio, módulo, PS ou Core:

- [architecture-classification.md](docs/05-architecture/architecture-classification.md)

## Docs Must (Sprint 3)

| Documento | Conteúdo |
|---|---|
| [mvp-scope.md](docs/05-architecture/mvp-scope.md) | Must / Should / Later / Out |
| [platform-security.md](docs/05-architecture/platform-security.md) | Security — **não** usar `security-strategy.md` |
| [frontend-architecture.md](docs/05-architecture/frontend-architecture.md) | Shell Composition |
| [devops-observability.md](docs/05-architecture/devops-observability.md) | Ambientes / CI / Obs |
| [communication-notification.md](docs/05-architecture/communication-notification.md) | In-app vs canais externos |

## Platform Services — tiers

| Tier | Qtd |
|---|---|
| **Confirmed** | 15 |
| **Strong Candidate** | 2 |
| **Needs Review** | 1 (Compliance, Q-019) |

## Regras

- **Não inventar** decisões — consultar open-questions.
- **Não contradizer** ADRs Accepted sem novo ADR.
- **Não implementar app** neste repo de arquitetura; stack só após AS-021.
- **Não criar** domínio fora dos 16 (ADR-0008).
- Engines e Event Bus = **PS** — não Business Domains.
- Notification = **in-app**; Communication = **canais externos**; Comunicar ≠ Integrar.
- Observability ≠ Audit; telemetria sem PHI.
- Português BR na documentação.
- Fluxo: Session → Workspace → ADR → Docs → AI Context → Implementação.

## Gate do 1º código

| Item | Status |
|---|---|
| ADRs Must Sprint 3 | ✅ 0001–0023 |
| Higiene placeholders | ✅ Fase 1 |
| Alinhamento INDEX / AI | ✅ Fase 2 |
| **AS-021 Bootstrap** | ⚪ Pendente |
| OQs residuais (Q-005, 009, 011, 015…) | Fora do gate |

## Próximo marco

**AS-021 — Technical Bootstrap** (linguagem, FE, DB produto, monorepo/repos, roles Must mínimos).  
AS-008 Telemedicine e OQs de negócio/técnicas residuais **não** bloqueiam o bootstrap.

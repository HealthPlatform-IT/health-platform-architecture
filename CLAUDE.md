# Health Platform — Contexto para IA

Repositório de **arquitetura e documentação** — sem código de aplicação neste repo.

Stack e layout de repos em **ADR-0024** (`technical-bootstrap.md`).

## Leitura obrigatória (nesta ordem)

1. [ARCHITECTURE_INDEX.md](ARCHITECTURE_INDEX.md) — estado do projeto
2. [ai-context/architecture-foundation.md](ai-context/architecture-foundation.md) — fundação completa
3. [ai-context/open-questions.md](ai-context/open-questions.md) — o que **não** está decidido (+ gate vs 1º código)
4. [ai-context/development-guidelines.md](ai-context/development-guidelines.md) — regras para propor mudanças

## ADRs e decisões

- Série oficial: `docs/05-architecture/adr/foundation/` — **0001–0024 Accepted**
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
| **0024** | Technical Bootstrap (stack / repos / roles) |

## Classificação

Antes de propor domínio, módulo, PS ou Core:

- [architecture-classification.md](docs/05-architecture/architecture-classification.md)

## Docs Must

| Documento | Conteúdo |
|---|---|
| [technical-bootstrap.md](docs/05-architecture/technical-bootstrap.md) | Stack · repos · roles (ADR-0024) |
| [mvp-scope.md](docs/05-architecture/mvp-scope.md) | Must / Should / Later / Out |
| [platform-security.md](docs/05-architecture/platform-security.md) | Security — **não** usar `security-strategy.md` |
| [frontend-architecture.md](docs/05-architecture/frontend-architecture.md) | Shell Composition |
| [devops-observability.md](docs/05-architecture/devops-observability.md) | Ambientes / CI / Obs |
| [communication-notification.md](docs/05-architecture/communication-notification.md) | In-app vs canais externos |

## Stack e repos (ADR-0024)

| Camada | Escolha |
|---|---|
| Backend | TypeScript · Node LTS · NestJS → `health-platform-api/platform/` |
| Frontend | React · TypeScript · Vite → `health-platform-front/staff-web/` |
| Mobile | Deferred → `health-platform-mobile/` (subdirs futuros) |
| DB | PostgreSQL · shared + `tenant_id` |

**Regra:** múltiplas APIs/fronts/apps mobile = **subdiretórios** no repo da superfície — não repos novos por serviço.

## Regras

- **Não inventar** decisões — consultar open-questions.
- **Não contradizer** ADRs Accepted sem novo ADR.
- **Não colocar código de app** neste repo de arquitetura.
- **Não criar** domínio fora dos 16 (ADR-0008).
- Engines e Event Bus = **PS** — não Business Domains.
- Notification = **in-app**; Communication = **canais externos**; Comunicar ≠ Integrar.
- Observability ≠ Audit; telemetria sem PHI.
- Português BR na documentação.
- Fluxo: Session → Workspace → ADR → Docs → AI Context → Implementação.

## Gate do 1º código

| Item | Status |
|---|---|
| ADRs Must + Bootstrap | ✅ 0001–0024 |
| Layout api/front/mobile | ✅ |
| OQs residuais | Fora do gate |

## Próximo marco

Identity/Tenant em `health-platform-api` · CI remotes quando autorizados.  
AS-008 e OQs residuais **não** bloqueiam.

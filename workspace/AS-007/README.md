# Workspace AS-007 — Document Engine

**Status:** ✅ Concluída — confirmada em 2026-07-03

## Objetivo

~~Responder Q-013 (parte Document Engine) e Q-014~~ ✅ — [ADR-0011](../../docs/05-architecture/adr/foundation/ADR-0011-document-engine.md)

## Resultado

| Item | Decisão |
|---|---|
| Classificação | Platform Service **Confirmed** (14º PS) |
| Modelo | Document Template / Generation Instance / Clinical Artifact |
| Validação | Render (Engine) vs regra de documento (domínio) |
| Consumidores | M-05, M-06 |
| Template Service | **Independente** — Q-014 Answered |
| Q-013 (DE) | **Answered** |
| Q-014 | **Answered** |
| OQ-PS06 | **Answered** |

## Documentação oficial

| Documento | Caminho |
|---|---|
| ADR-0011 | [`docs/05-architecture/adr/foundation/ADR-0011-document-engine.md`](../../docs/05-architecture/adr/foundation/ADR-0011-document-engine.md) |
| Document Engine | [`docs/05-architecture/document-engine.md`](../../docs/05-architecture/document-engine.md) |
| Pacote de confirmação | [`confirmation-package.md`](confirmation-package.md) |

## Arquivos do workspace (histórico)

| Arquivo | Papel |
|---|---|
| `document-engine-boundary.md` | Deep dive NR-001/002 |
| `domain-interactions.md` | Matriz domínios e módulos |
| `template-service-boundary-draft.md` | Q-014 — Template vs Engine |
| `validation-strategy-draft.md` | Taxonomia validação |
| `versioning-strategy-draft.md` | Versionamento template vs artefato |
| `decisions.md` | D-001 a D-008 Confirmados |

## Sequência NR

```text
NR-001  boundary + definição              [✅]
NR-002  três artefatos                    [✅]
NR-003  domain-interactions               [✅]
NR-004  Medical Form Engine fronteira     [✅]
NR-005  Template Service (Q-014)          [✅]
NR-006  validation + versioning           [✅]
NR-007  confirmation-package              [✅]
```

## Sessão anterior

[AS-006 — Medical Form Engine](../AS-006/README.md) — ADR-0010

## Sessão oficial

[`architecture-sessions/AS-007-document-engine.md`](../../architecture-sessions/AS-007-document-engine.md)

## Próxima sessão

**AS-010 — Event Strategy** (Q-003) ou **Sprint 3 — Technical Architecture**

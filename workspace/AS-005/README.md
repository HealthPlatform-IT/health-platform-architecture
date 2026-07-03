# Workspace AS-005 — Core Platform Boundary

**Status:** ✅ Concluída — confirmada em 2026-07-03

## Objetivo

Responder **Q-007** — definir fronteiras entre Core Platform, Platform Services, Business Domains, Modules, Extensions e Read Models/Views.

## Resultado

| Item | Decisão |
|---|---|
| Core Platform | 8 contratos/invariantes |
| Platform Services | 12 Confirmed + 5 Strong + 1 Needs Review |
| Modules | 15 candidatos |
| Extension | M-14, M-15 + H-EXT-001 |
| Clinical Workspace | M-02 shell |
| Read Models | Timeline + Analytics reafirmados |
| Q-007 | **Answered** — ADR-0009 |

## Open Question principal

| ID | Pergunta | Status |
|---|---|---|
| Q-007 | Fronteira Core / Platform Services / Modules | **Answered** |

## Documentação oficial

| Documento | Caminho |
|---|---|
| ADR-0009 | [`docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md`](../../docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md) |
| Core Platform | [`docs/05-architecture/core-platform.md`](../../docs/05-architecture/core-platform.md) |
| Module Strategy | [`docs/05-architecture/module-strategy.md`](../../docs/05-architecture/module-strategy.md) |
| Extension Model | [`docs/05-architecture/extension-model.md`](../../docs/05-architecture/extension-model.md) |
| Read Models | [`docs/05-architecture/read-models.md`](../../docs/05-architecture/read-models.md) |
| Architecture Classification | [`docs/05-architecture/architecture-classification.md`](../../docs/05-architecture/architecture-classification.md) |
| Platform Services | [`docs/05-architecture/platform-services.md`](../../docs/05-architecture/platform-services.md) |
| Glossary | [`docs/00-introduction/glossary.md`](../../docs/00-introduction/glossary.md) |
| Pacote de confirmação | [`confirmation-package.md`](confirmation-package.md) |

## Arquivos do workspace

| Arquivo | Conteúdo | Status |
|---|---|---|
| **`core-boundary.md`** | Deep dive: Core Platform | ✅ |
| **`platform-services-boundary.md`** | Deep dive: Core ↔ PS | ✅ |
| **`module-strategy-draft.md`** | Módulos candidatos | ✅ Validado |
| **`extension-model-draft.md`** | Modelo de extensão | ✅ |
| **`read-models-boundary.md`** | Clinical Timeline, Analytics | ✅ |
| **`classification-matrix.md`** | Árvore de decisão unificada | ✅ |
| **`decisions.md`** | D-001 a D-008 Confirmados | ✅ |
| **`confirmation-package.md`** | Pacote final | ✅ Confirmado |

## Sequência de investigação (NR)

```text
NR-001 a NR-008  [✅ concluído]
```

## Sessão anterior

[AS-004 — Business Domain Map](../AS-004/README.md) — Q-002 Answered

## Próxima sessão sugerida

**AS-006 — Medical Form Engine** (Q-013).

# Workspace AS-006 — Medical Form Engine

**Status:** 🟡 Preparada — aguardando início da investigação

## Objetivo

Responder **Q-013** (parte Medical Form Engine) — definir escopo, responsabilidades e contratos conceituais do Platform Service **Medical Form Engine**.

## Open Question principal

| ID | Pergunta | Status |
|---|---|---|
| **Q-013** | Escopo e contratos Medical Form Engine | Open |
| Q-014 | Template Service independente ou integrado? | Open *(influência parcial)* |
| OQ-PS05 | Medical Form Engine — PS ou parte de Clinical Record? | Open *(herdado AS-005)* |

## Contexto herdado (AS-005)

| Item | Valor provisório |
|---|---|
| Classificação | Platform Service — **Strong Candidate** |
| Fronteira | **Needs Review** (OQ-PS05) |
| Consumidores | M-03 Attendance, M-04 Clinical Documentation |
| Domínio relacionado | Clinical Record (conteúdo); Care Delivery (contexto) |
| Risco principal | R-MFE-01 — Engine virar prontuário |

## Documentação de referência

| Documento | Caminho |
|---|---|
| ADR-0005 (catálogo PS) | [`docs/05-architecture/adr/foundation/ADR-0005-platform-services.md`](../../docs/05-architecture/adr/foundation/ADR-0005-platform-services.md) |
| ADR-0009 (fronteiras) | [`docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md`](../../docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md) |
| Module Strategy | [`docs/05-architecture/module-strategy.md`](../../docs/05-architecture/module-strategy.md) |
| Clinical Record Domain | [`docs/04-domain/business-domains.md`](../../docs/04-domain/business-domains.md) |
| Deep dive AS-005 | [`../AS-005/platform-services-boundary.md`](../AS-005/platform-services-boundary.md) § Medical Form Engine |

## Arquivos do workspace

| Arquivo | Conteúdo | Status |
|---|---|---|
| **`medical-form-engine-boundary.md`** | Deep dive — fronteira PS vs domínio vs módulos | 🟡 Rascunho inicial |
| `questions.md` | Perguntas da sessão | ✅ Preparado |
| `hypotheses.md` | Hipóteses de trabalho | ✅ Preparado |
| `notes.md` | Log de investigação | ✅ Iniciado |
| `decisions.md` | Decisões (vazio até investigação) | ⚪ Aguardando |
| `confirmation-package.md` | Pacote final | ⚪ Aguardando |

## Sequência de investigação (NR)

```text
NR-001  Fronteira PS vs Clinical Record Domain     [próximo]
NR-002  Form definition / instance / content
NR-003  Consumo M-03 vs M-04
NR-004  Template Service (Q-014 parcial)
NR-005  Preview Document Engine (sem AS-007)
NR-006  Contratos conceituais
NR-007  confirmation-package.md
```

## Sessão anterior

[AS-005 — Core Platform Boundary](../AS-005/README.md) — Q-007 Answered (ADR-0009)

## Próxima sessão (após AS-006)

**AS-007 — Document Engine** (Q-013 parte Document Engine)

## Sessão oficial

[`architecture-sessions/AS-006-medical-form-engine.md`](../../architecture-sessions/AS-006-medical-form-engine.md)

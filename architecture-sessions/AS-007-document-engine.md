---
id: AS-007
title: Document Engine
status: Completed
version: 1.0.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
reviewers: []
phase: Product & Architecture Foundation
category: Product Architecture
priority: Medium
estimated-impact: High
tags:
  - platform-services
  - document-engine
  - clinical-documents
  - Q-013
  - Q-014
related:
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/medical-form-engine.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0011-document-engine.md
  - docs/05-architecture/document-engine.md
  - ai-context/open-questions.md
  - workspace/AS-007/README.md
---

# AS-007 — Document Engine

## Informações Gerais

| Campo | Valor |
|---|---|
| Sessão | AS-007 |
| Tema | Document Engine |
| Status | Completed |
| Fase | Product & Architecture Foundation |
| Categoria | Product Architecture |
| Prioridade | Medium |
| Impacto Estimado | High |
| Responsável | Architecture Team |
| Data de início | 2026-07-03 |
| Data de encerramento | 2026-07-03 |
| Confirmação | *"confirmo o pacote"* |
| Open Questions | Q-013 · Q-014 — **Answered** (ADR-0011) |

---

## Objetivo

Definir o **escopo, responsabilidades e contratos conceituais** do Platform Service **Document Engine** — distinguindo-o de:

- **Clinical Documents Domain** (artefato formal, regras, ciclo de vida)
- **Medical Form Engine** (captura dinâmica — ADR-0010)
- **Template Service** (templates reutilizáveis — Q-014 **Answered**)
- **Módulos M-05 Orders** e **M-06 Documents**

**Pergunta central:**

> O que pertence ao Document Engine, o que pertence ao domínio Clinical Documents e o que deve ficar fora do engine?

---

## Contexto

- **AS-006** (ADR-0010): Medical Form Engine = captura estruturada; fronteira captura ≠ formal (D-008).
- **ADR-0005**: Document Engine **Confirmado** (ADR-0011) — laudos, atestados, receitas, termos.
- **ADR-0008**: Clinical Documents = artefatos formais assináveis; geração técnica PDF no PS.
- **module-strategy**: M-05 e M-06 consomem Document Engine.

---

## Hipótese Inicial (H-DE-001)

Document Engine é **Platform Service** que fornece **mecanismo de geração e renderização** de documentos clínicos formais a partir de templates e dados estruturados — **sem** possuir regras de negócio de documento nem ownership do artefato clínico.

---

## Escopo da Investigação (NR)

| NR | Tema | Artefato |
|---|---|---|
| NR-001 | Definição e fronteira PS vs domínio | `document-engine-boundary.md` |
| NR-002 | Três artefatos / fluxo geração | Mesmo artefato |
| NR-003 | domain-interactions | `domain-interactions.md` |
| NR-004 | Fronteira Medical Form Engine | Seção em boundary |
| NR-005 | Fronteira Template Service (Q-014) | `template-service-boundary-draft.md` |
| NR-006 | Validação e versionamento | drafts |
| NR-007 | Pacote de confirmação | `confirmation-package.md` |

---

## Decisões Confirmadas

D-001 Classificação PS ✅ · D-002 Artefatos ✅ · D-003 Render vs regra ✅ · D-004 Consumidores ✅ · D-005 Template ✅ · D-006 MFE fronteira ✅ · D-007 Versionamento ✅ · D-008 Assinatura (conceitual) ✅

---

## Workspace

[`workspace/AS-007/`](../workspace/AS-007/README.md)

---

## Resultado (encerramento 2026-07-03)

Pacote confirmado pelo usuário (*"confirmo o pacote"*).

| Entrega | Status |
|---|---|
| ADR-0011 — Document Engine | ✅ Accepted |
| `docs/05-architecture/document-engine.md` | ✅ Publicado |
| `platform-services.md` v0.4.0 — 14 Confirmed | ✅ |
| Q-013, Q-014, OQ-PS06 | ✅ Answered |
| Workspace AS-007 | ✅ Encerrado |

**Próximo marco:** AS-010 Event Strategy ou Sprint 3 — Technical Architecture.

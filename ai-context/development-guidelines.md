---
title: Development Guidelines
status: Draft
version: 0.3.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: AI Context
phase: Product & Architecture Foundation
related:
  - ai-context/architecture-foundation.md
  - ai-context/open-questions.md
  - docs/05-architecture/architecture-classification.md
  - docs/05-architecture/medical-form-engine.md
  - docs/05-architecture/document-engine.md
  - docs/05-architecture/event-strategy.md
  - ARCHITECTURE_INDEX.md
---

# Development Guidelines

> Diretrizes para **desenvolvimento assistido por IA** e equipe — conceitual, sem stack técnica.

**Versão 0.2.0** — pós-AS-007 (Sprint 2 documental). Stack, APIs e repositórios: Sprint 3.

---

## 1. Fluxo obrigatório

```text
Architecture Session → Workspace Draft → Decision → ADR → Documentação Oficial → AI Context → Implementação
```

**Não implementar** funcionalidade sem decisão arquitetural correspondente.

---

## 2. Antes de propor código

1. Ler `ai-context/architecture-foundation.md`.
2. Consultar `ai-context/open-questions.md` — não resolver perguntas Open por código.
3. Classificar responsabilidade com `docs/05-architecture/architecture-classification.md`.
4. Verificar ADRs Accepted em `docs/05-architecture/adr/foundation/` (0001–0020).
5. Respeitar recorte MVP (`mvp-scope.md` / ADR-0019) e Security (`platform-security.md` / ADR-0020).
5. Não contradizer ADR sem propor novo ADR.

---

## 3. Seis categorias — regras práticas

| Categoria | Regra para implementação |
|---|---|
| **Core Platform** | Apenas contratos/invariantes — alteração exige ADR |
| **Platform Service** | Consumir por contrato — não duplicar em módulos |
| **Business Domain** | Regras e vocabulário no domínio — PS para mecanismo |
| **Module** | UI/fluxo — consome PS, não reimplementa transversais |
| **Extension** | Opcional por tenant — Feature Flag + Registry |
| **Read Model** | Projeção — ownership nos domínios fonte |

---

## 4. Módulos

- Declarar domínio primário + capability.
- **Não** acessar estado interno de outro módulo — PS ou Event Foundation.
- Habilitação via Configuration + Feature Flag + Module Registry.
- Catálogo: `docs/05-architecture/module-strategy.md`.

---

## 5. Platform Services

- Política no domínio; execução no PS.
- Ex.: Communication Domain decide *quando*; Communication Service envia.
- Tiers atuais: **15 Confirmed** + **2 Strong** + **1 Needs Review** — `platform-services.md` v0.5.0.

### Registrar — engines (ADR-0010, ADR-0011)

| PS | Responsabilidade | Consumidores principais |
|---|---|---|
| **Medical Form Engine** | Captura estruturada — Form Definition / Instance / Clinical Content | M-03, M-04, M-05, M-14, M-15 |
| **Document Engine** | Geração/renderização formal — Template → Generation → Clinical Artifact | M-05, M-06 |

**Regras:** Engine valida forma/render; domínio valida conteúdo clínico ou regra de documento. Captura (MFE) ≠ documento formal (DE).

### Eventos (ADR-0012)

```text
Módulo → domínio valida + persiste → domínio publica → Event Bus → assinantes
```

- Event Foundation (Core) = contrato; Event Bus (PS) = transporte interno.
- Tipos de evento no **domínio publicador** — não no Core.
- Read Models e Audit **assinam**; módulos **não** publicam eventos de negócio.
- Integration/Webhook/Communication **fora** da bus (ADR-0004).
- Broker técnico — Sprint 3.

---

## 6. Extensão e customização

Hierarquia ADR-0006: Configuração → Composição → Extensão → Customização → Core.

Customização por tenant é **exceção governada** (Q-016) — não padrão.

---

## 7. O que não fazer

- Inventar domínio fora dos 16 (ADR-0008).
- Criar Platform Service sem critério ADR-0005.
- Colocar regras clínicas no Core ou nos engines Registrar.
- Promover Read Model a domínio ou módulo.
- Implementar antes de documentar (Sprint 3 ainda não iniciada para stack).
- Absorver Template Service no Document Engine (Q-014 Answered).

---

## 8. Próximas sessões

| Sessão | Foco |
|---|---|
| ~~AS-011~~ | Multi-Tenant Strategy — ✅ ADR-0013 |
| ~~AS-012~~ | Backend Architecture — ✅ ADR-0014 |
| ~~AS-013~~ | Database Architecture — ✅ ADR-0015 |
| ~~AS-014~~ | API Strategy — ✅ ADR-0016 |
| ~~AS-015~~ | Event Bus Technical — ✅ ADR-0017 |
| ~~AS-016~~ | Clinical Aggregates — ✅ ADR-0018 / Q-004 |
| ~~AS-017~~ | MVP Scope — ✅ ADR-0019 / Q-006 |
| ~~AS-009~~ | Platform Security — ✅ ADR-0020 |
| **Frontend / DevOps** | Próximos marcos Sprint 3 |

---

## 9. Referências obrigatórias

| Documento | Uso |
|---|---|
| [architecture-foundation.md](architecture-foundation.md) | Contexto completo |
| [open-questions.md](open-questions.md) | O que não está decidido |
| [adr-summary.md](adr-summary.md) | Resumo ADRs 0001–0013 |
| [ARCHITECTURE_INDEX.md](../ARCHITECTURE_INDEX.md) | Estado do projeto |
| [medical-form-engine.md](../docs/05-architecture/medical-form-engine.md) | ADR-0010 |
| [document-engine.md](../docs/05-architecture/document-engine.md) | ADR-0011 |
| [event-strategy.md](../docs/05-architecture/event-strategy.md) | ADR-0012 |
| [multi-tenant-strategy.md](../docs/05-architecture/multi-tenant-strategy.md) | ADR-0013 |
| [backend-architecture.md](../docs/05-architecture/backend-architecture.md) | ADR-0014 |
| [database-architecture.md](../docs/05-architecture/database-architecture.md) | ADR-0015 |
| [api-strategy.md](../docs/05-architecture/api-strategy.md) | ADR-0016 |

---
title: Development Guidelines
status: Draft
version: 0.4.0
created: 2026-07-03
updated: 2026-07-14
author: Architecture Team
category: AI Context
phase: Implementation Readiness
related:
  - ai-context/architecture-foundation.md
  - ai-context/open-questions.md
  - docs/05-architecture/architecture-classification.md
  - docs/05-architecture/mvp-scope.md
  - docs/05-architecture/platform-security.md
  - docs/05-architecture/frontend-architecture.md
  - docs/05-architecture/devops-observability.md
  - docs/05-architecture/communication-notification.md
  - ARCHITECTURE_INDEX.md
---

# Development Guidelines

> Diretrizes para **desenvolvimento assistido por IA** e equipe.

**Versão 0.4.0** — pós Sprint 3 Must (ADRs 0001–0023). Stack/repos: **AS-021 Technical Bootstrap** (pendente). MVP Must pode ser implementado **após** bootstrap registrado.

---

## 1. Fluxo obrigatório

```text
Architecture Session → Workspace Draft → Decision → ADR → Documentação Oficial → AI Context → Implementação
```

**Não implementar** funcionalidade de produto sem decisão arquitetural correspondente (ADR Accepted ou doc oficial).

---

## 2. Antes de propor código

1. Ler `ai-context/architecture-foundation.md`.
2. Consultar `ai-context/open-questions.md` — classificar se a OQ **bloqueia** o trabalho ou é Deferred/Important.
3. Classificar responsabilidade com `docs/05-architecture/architecture-classification.md`.
4. Verificar ADRs Accepted em `docs/05-architecture/adr/foundation/` (**0001–0023**).
5. Respeitar Must: MVP, Security, Frontend, DevOps, Communication/Notification, Backend, Database, API, Multi-Tenant, Events.
6. Não contradizer ADR sem propor novo ADR.
7. **Stack/linguagem/repos**: só após **AS-021** (ou ADR de bootstrap). Não escolher stack “no silêncio”.

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
- Must MVP: M-01…M-06 + M-09 — `mvp-scope.md` / ADR-0019.

---

## 5. Platform Services

- Política no domínio; execução no PS.
- Ex.: Communication Domain decide *quando*; Communication Service envia.
- Tiers: **15 Confirmed** + **2 Strong** + **1 Needs Review** — `platform-services.md`.

### Registrar — engines (ADR-0010, ADR-0011)

| PS | Responsabilidade | Consumidores principais |
|---|---|---|
| **Medical Form Engine** | Captura estruturada | M-03, M-04, M-05, M-14, M-15 |
| **Document Engine** | Geração/renderização formal | M-05, M-06 |

**Regras:** Engine valida forma/render; domínio valida conteúdo clínico ou regra de documento. Captura (MFE) ≠ documento formal (DE).

### Eventos (ADR-0012 / ADR-0017)

```text
Módulo → domínio valida + persiste → domínio publica → Event Bus → assinantes
```

- Event Foundation (Core) = contrato; Event Bus (PS) = transporte interno.
- Tipos de evento no **domínio publicador** — não no Core.
- Read Models e Audit **assinam**; módulos **não** publicam eventos de negócio.
- Integration/Webhook/Communication **fora** da bus (ADR-0004).
- Produto broker concreto — Deferred (PoC / pós-bootstrap).

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
- Absorver Template Service no Document Engine (Q-014 Answered).
- Usar pasta `adr/` na raiz do repo (legado) ou stubs de processes/journeys como fonte da verdade.
- Tratar `security-strategy.md` como documento — use `platform-security.md`.
- Bloquear o MVP Must por OQs residuais (Q-005 Billing Out, Q-011 FHIR, etc.).
- Escolher stack/IdP/broker sem AS-021 / ADR.

---

## 8. Gate de implementação

| Pode começar após AS-021 | Fica de fora do gate |
|---|---|
| Scaffold monorepo/repos | Q-005 Billing |
| Identity / Tenant / AuthZ mínimos | Q-011 FHIR Messaging |
| Módulos Must MVP | AS-008 Telemedicine (doc) |
| Outbox + contratos de evento | Produto broker final |
| Shell M-02 | Matriz AuthZ completa |

---

## 9. Próximas sessões

| Sessão | Foco |
|---|---|
| ~~AS-011…AS-020~~ | ✅ Sprint 3 Must |
| **AS-021** | Technical Bootstrap — **próximo** |
| AS-008 | Telemedicine (baixa) |

---

## 10. Referências obrigatórias

| Documento | Uso |
|---|---|
| [architecture-foundation.md](architecture-foundation.md) | Contexto completo |
| [open-questions.md](open-questions.md) | OQs + gate vs 1º código |
| [adr-summary.md](adr-summary.md) | Resumo ADRs 0001–0023 |
| [ARCHITECTURE_INDEX.md](../ARCHITECTURE_INDEX.md) | Estado do projeto |
| [mvp-scope.md](../docs/05-architecture/mvp-scope.md) | ADR-0019 |
| [platform-security.md](../docs/05-architecture/platform-security.md) | ADR-0020 |
| [frontend-architecture.md](../docs/05-architecture/frontend-architecture.md) | ADR-0021 |
| [devops-observability.md](../docs/05-architecture/devops-observability.md) | ADR-0022 |
| [communication-notification.md](../docs/05-architecture/communication-notification.md) | ADR-0023 |
| [backend-architecture.md](../docs/05-architecture/backend-architecture.md) | ADR-0014 |
| [database-architecture.md](../docs/05-architecture/database-architecture.md) | ADR-0015 |
| [api-strategy.md](../docs/05-architecture/api-strategy.md) | ADR-0016 |
| [event-strategy.md](../docs/05-architecture/event-strategy.md) | ADR-0012 / 0017 |
| [multi-tenant-strategy.md](../docs/05-architecture/multi-tenant-strategy.md) | ADR-0013 |
| [medical-form-engine.md](../docs/05-architecture/medical-form-engine.md) | ADR-0010 |
| [document-engine.md](../docs/05-architecture/document-engine.md) | ADR-0011 |

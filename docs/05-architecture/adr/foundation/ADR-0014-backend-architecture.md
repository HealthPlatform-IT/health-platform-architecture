---
title: ADR-0014 - Backend Architecture
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - backend
  - modular-monolith
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-012-backend-architecture.md
  - docs/05-architecture/backend-architecture.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/multi-tenant-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0013-multi-tenant-strategy.md
  - workspace/AS-012/confirmation-package.md
---

# ADR-0014 — Backend Architecture

## Status

Accepted

---

# Contexto

A Sprint 3 (Technical Architecture) exige definir **como o software servidor executa** a Health Platform após o fechamento de Multi-Tenant (ADR-0013) e Event Strategy (ADR-0012).

O **ADR-0009** estabeleceu que Module é unidade lógica de produto — **não** unidade de deploy por definição. Sem Backend Architecture, há risco de microsserviços prematuros e de confundir Categories oficiais com topologia de deploy.

A **AS-012** investigou NR-001 a NR-005 e confirmou o pacote D-001 a D-010 em 2026-07-14 (*"Confirmado o pacote"*).

---

# Problema

Sem decisão de backend:

- Tendência a 1 módulo = 1 microsserviço
- Fronteiras Module / Domain / PS ambíguas em runtime
- “Core Service” absorvendo regra e infraestrutura
- Tenant Context inconsistente em requests, jobs e consumers
- Escolha de stack antes do estilo arquitetural

---

# Decisão

## 1. Definição (D-001)

**Backend Architecture** = estilo de execução servidor, camadas lógicas e fronteiras Module / Domain / Platform Service em runtime — **sem** framework, DDL ou OpenAPI nesta decisão.

## 2. Estilo padrão = modular monolith (D-002)

O backend padrão é um **modular monolith**: um (ou poucos) deployables com boundaries lógicos claros entre Module, Domain e PS.

## 3. Microsserviço = exceção (D-003)

Extrair deployable separado **somente** com critérios explícitos (ex.: ≥ 2 entre escala, isolamento de falha crítico, release independente regulatório, ownership de time com contrato estável).

**Rejeitado:** um Module ID = um serviço.

## 4. Unidades lógicas ≠ deploy (D-004)

Business Domain, Module e Platform Service **não** são unidades de deploy por default. PS pode ser **in-process** ou **remote** conforme necessidade — contratos Core permanecem válidos.

## 5. Camadas (D-005)

```text
API / Presentation
        ↓
Application (Module orchestration / use cases)
        ↓
Domain (Business Domain rules + Domain Event types)
        ↓
Infrastructure (PS adapters, persistence, Event Bus, files)
```

Dependência: camadas externas dependem das internas; Domain não conhece HTTP/SQL.

## 6. Core = contratos (D-006)

Core Platform permanece **contratos / shared kernel packages** — **não** um “Core Service” runtime que concentra negócio e infra.

## 7. Pipeline Runtime Context (D-007)

Todo request HTTP, consumer de evento e job scoped carrega **Runtime Context** (tenant obrigatório — ADR-0013). Envelope de Domain Event inclui tenant (ADR-0012). AuthZ detalhada = AS-009.

## 8. Fronteiras vizinhas (D-008)

| Tema | Destino |
|---|---|
| DDL, RLS, vendor DB | Database Architecture |
| Contratos sync / versionamento | API Strategy |
| AuthZ / secrets | AS-009 Security |
| Broker Event Bus | Q-003 tecnologia |

## 9. Stack Deferred (D-009)

Linguagem e framework **Deferred** após esta decisão de estilo + preferência da equipe.

## 10. Deployables iniciais (orientação)

Candidato mínimo: **platform-api** (modules + domains + PS in-proc + porta Event Bus); workers opcionais no mesmo código com mesmo pipeline de tenant. Silo DB (ADR-0013) **não** implica split automático de serviço.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Microsserviços por módulo (15) | Contradiz ADR-0009; custo operacional prematuro |
| Core Service monolítico de “tudo” | Viola Core = contratos |
| Escolher framework nesta ADR | Estilo e fronteiras primeiro |
| Absorver Database + API nesta ADR | Escopo e cartões vizinhos |

---

# Consequências

## Positivas

- Base clara para Database Architecture e API Strategy
- Alinhamento Module ≠ deploy (ADR-0009)
- Tenant e eventos coerentes no runtime
- Menor risco de over-engineering precoce

## Negativas / deferred

- Stack ainda aberta
- Detalhe de contratos sync e persistência em sessões seguintes
- Critérios de split exigem disciplina de governança

---

# Relação com ADRs

| ADR | Relação |
|---|---|
| ADR-0009 | Module ≠ deploy; Core = contratos |
| ADR-0012 | Sync ≠ evento; publish pós-persistência |
| ADR-0013 | Runtime Context em todo pipeline |

---

# Referências

- `docs/05-architecture/backend-architecture.md`
- `architecture-sessions/AS-012-backend-architecture.md`
- `workspace/AS-012/`

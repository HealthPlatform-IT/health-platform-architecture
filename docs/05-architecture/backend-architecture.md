---
title: Backend Architecture
status: Stable
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/multi-tenant-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0014-backend-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0013-multi-tenant-strategy.md
  - architecture-sessions/AS-012-backend-architecture.md
---

# Backend Architecture

> Estratégia de execução do servidor da Health Platform — estilo, camadas e fronteiras Module / Domain / PS.

Decisão formal: [ADR-0014](adr/foundation/ADR-0014-backend-architecture.md) · AS-012 D-001 a D-010.

---

## 1. Purpose

Definir como o backend organiza runtime **sem** escolher framework, DDL ou OpenAPI.

**Decisão central:** modular monolith como padrão; microsserviço como exceção; camadas claras; Tenant Context em todo pipeline.

---

## 2. Estilo de aplicação

| Modelo | Papel |
|---|---|
| **Modular monolith** | **Padrão** |
| **Microsserviço / deploy separado** | **Exceção** com critérios (escala, falha, release, ownership) |

**Module ≠ microsserviço.** Domain ≠ deploy unit. PS pode ser in-proc ou remote.

### Critérios mínimos para split

Extrair deployable separado somente se ≥ 2 forem verdadeiros: necessidade de escala distinta; isolamento de falha crítico; release independente por risco regulatório; time/ownership com contrato estável.

---

## 3. Camadas

```text
API / Presentation
        ↓
Application (Module orchestration / use cases)
        ↓
Domain (regras + tipos de Domain Event)
        ↓
Infrastructure (adapters PS, persistência, Event Bus, files)
```

| Camada | Responsabilidade |
|---|---|
| **API** | Entrada, Tenant no pipeline, validação de contrato |
| **Application** | Orquestra fluxos de módulo; coordena domínios |
| **Domain** | Invariantes e Domain Events |
| **Infrastructure** | Implementa ports (DB, PS, bus) |

---

## 4. Core, Module, Domain, PS

| Categoria | No runtime |
|---|---|
| **Core Platform** | Pacotes/contratos — **não** “Core Service” |
| **Module** | Superfície de produto / orquestração |
| **Business Domain** | Ownership da regra e dos tipos de evento |
| **Platform Service** | Mecanismos transversais (local ou remoto) |
| **Read Model** | Projection; pode iniciar no mesmo deployable |

---

## 5. Pipeline Runtime Context

Alinhado [multi-tenant-strategy.md](multi-tenant-strategy.md) e [event-strategy.md](event-strategy.md):

```text
Inbound → Identity → Runtime Context → AuthZ ([platform-security.md](platform-security.md) / ADR-0020)
        → Application / Domain → Persist → Publish Event (tenant no envelope)
```

Operação sem tenant (exceto paths públicos governados) é inválida.

---

## 6. Sync vs Event Bus

- Use case síncrono na Application/Domain
- Publicação assíncrona **após** persistência via Event Bus (ADR-0012)
- Não usar a bus para esconder acoplamento sync injustificado no monólito

---

## 7. Deployables iniciais (orientação)

| Deployable lógico | Conteúdo |
|---|---|
| **platform-api** | Modules + Domains + PS in-proc + porta Event Bus |
| **workers** (opcional) | Mesmo código; consumers/jobs com mesmo pipeline de tenant |

Silo de banco (ADR-0013) não implica split automático de serviço.

---

## 8. Deferred

| Tema | Destino |
|---|---|
| Linguagem / framework | Preferência de equipe pós-estilo |
| DDL / RLS / vendor | Database Architecture |
| OpenAPI / versionamento | API Strategy |
| AuthZ detalhada | ✅ ADR-0020 |
| Broker | Q-003 tech |
| BFF / Frontend | ✅ ADR-0021 |

---

## 9. Referências

| Documento | Uso |
|---|---|
| [ADR-0014](adr/foundation/ADR-0014-backend-architecture.md) | Decisão formal |
| [ADR-0009](adr/foundation/ADR-0009-core-platform-boundary.md) | Module ≠ deploy |
| [module-strategy.md](module-strategy.md) | Catálogo de módulos |
| [multi-tenant-strategy.md](multi-tenant-strategy.md) | Tenant pipeline |
| [event-strategy.md](event-strategy.md) | Event Bus |

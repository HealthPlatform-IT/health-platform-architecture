---
title: ADR-0013 - Multi-Tenant Strategy
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - multi-tenant
  - tenant-context
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-011-multi-tenant-strategy.md
  - docs/05-architecture/multi-tenant-strategy.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - workspace/AS-011/confirmation-package.md
---

# ADR-0013 — Multi-Tenant Strategy

## Status

Accepted

---

# Contexto

A Health Platform é SaaS multi-tenant. O **ADR-0009** definiu **Tenant Context** como contrato do Core (I-01) e **Runtime Context** (I-02). O **Identity Service** (ADR-0005) gerencia autenticação e sessão. O **ADR-0012** exige tenant no envelope de eventos.

**Q-008** permanecia aberta quanto à estratégia de isolamento e fronteiras Tenant vs Organization.

A **AS-011** investigou NR-001 a NR-006 e confirmou o pacote D-001 a D-008 em 2026-07-14.

---

# Problema

Sem estratégia multi-tenant:

- Confusão entre Tenant (SaaS) e Institution (negócio)
- Risco de vazamento cross-tenant
- Isolamento de dados indefinido
- Propagação de contexto inconsistente em APIs, jobs e Event Bus
- Configuração/módulos sem regra por tenant

---

# Decisão

## 1. Definição de Tenant (D-001)

**Tenant** = unidade de isolamento e contratação SaaS da Health Platform (cliente da plataforma).

## 2. Tenant vs Organization vs Institution (D-002)

| Conceito | Papel | Camada |
|---|---|---|
| **Tenant** | Fronteira de isolamento SaaS | Core (contrato) + Identity |
| **Organization / Institution** | Entidade de negócio que presta cuidado | Organization Management Domain |
| **Unidade / Site** | Nível operacional | Config / domínio |
| **User** | Ator autenticado | Identity |

Um Tenant pode conter **uma ou mais** organizations/institutions.

**OQ-C01 Answered:** Organization no Runtime Context = **referência (ID)** — ownership do cadastro **não** está no Core.

## 3. Modelo de isolamento (D-003)

| Modelo | Papel |
|---|---|
| **A — Shared + discriminador** | **Padrão** da plataforma |
| **C — Database / instância por tenant** | **Exceção governada** (compliance/contrato) |

**OQ-MT01 Answered.** DDL, RLS, vendor de banco — **Deferred** (Database Architecture).

## 4. Resolução e propagação (D-004)

Identity autentica e estabelece sessão com Tenant Context + User Context. Runtime Context propaga tenant, organization (ref), user e escopo. Operação sem tenant é **inválida**. Platform Services não inventam tenant.

## 5. Tenant em eventos (D-005)

Alinhado ADR-0012: envelope inclui tenant; Event Bus **não** entrega cross-tenant por padrão; assinantes processam no tenant do envelope.

## 6. Configuração e módulos (D-006)

Configuration Service, Feature Flag Service e Module Registry operam **por tenant** (mínimo), com hierarquia reconhecida tenant → institution → unidade. Customização por código proibida como padrão (ADR-0006). Herança detalhada permanece **Q-015**.

## 7. Cross-tenant

Acesso cross-tenant (operador da plataforma) é **exceção governada** com Authorization + Audit — não padrão de produto clínico.

## 8. Deferred (D-007)

| Tema | Destino |
|---|---|
| DDL, RLS, particionamento, vendor DB | Database Architecture |
| AuthZ detalhada, secrets | AS-009 / Security |
| Broker Event Bus | Sessão técnica (Q-003 tech) |
| Herança config / schema config / custom | Q-015, Q-017, Q-016 |

## 9. O que pertence / não pertence

**Pertence:** definição de Tenant; fronteiras Organization; modelo de isolamento conceitual; regras de propagação; implicações em eventos e config.

**Não pertence:** regra clínica; escolha de produto de banco; schemas DDL; AuthZ detalhada; customização por código por tenant.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Tenant = Institution (mesmo conceito) | Mistura isolamento SaaS com estrutura de negócio |
| Silo (DB por tenant) como padrão | Custo operacional alto para SaaS típico |
| Catálogo/isolamento no Core operacional | Core só contratos (ADR-0009) |
| Event Bus cross-tenant por padrão | Viola isolamento e ADR-0012 |

---

# Consequências

## Positivas

- **Q-008 Answered** (estratégia)
- OQ-C01 / OQ-MT01 Answered
- Base clara para Database Architecture, Identity e Security
- Alinhamento com Event Strategy

## Negativas / deferred

- Detalhe físico de persistência ainda aberto
- AuthZ operacional detalhada na AS-009
- Q-015 herança de configuração permanece Open

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| ADR-0005 | Identity / Config / Feature Flag |
| ADR-0006 | Configuração sobre customização |
| ADR-0009 | Tenant Context, Runtime Context, I-01/I-02 |
| ADR-0012 | Tenant no envelope de eventos |

---

# Documentação oficial

- `docs/05-architecture/multi-tenant-strategy.md`
- `docs/05-architecture/core-platform.md`

---
title: Multi-Tenant Strategy
status: Draft
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/event-strategy.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/adr/foundation/ADR-0013-multi-tenant-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/adr/foundation/ADR-0012-event-strategy.md
  - architecture-sessions/AS-011-multi-tenant-strategy.md
---

# Multi-Tenant Strategy

> Estratégia multi-tenant da Health Platform — isolamento SaaS, propagação de contexto e fronteiras Tenant vs Organization.

Decisão formal: [ADR-0013](adr/foundation/ADR-0013-multi-tenant-strategy.md) · AS-011 D-001 a D-008.

---

## 1. Purpose

Definir como a plataforma isola e opera múltiplos clientes SaaS (tenants), honrando Tenant Context (I-01) e Runtime Context (I-02) — **sem** implementar código, DDL ou escolher produto de banco nesta documentação.

**Pergunta respondida (Q-008 — estratégia):**

> Tenant isola o SaaS; Organization vive no domínio; dados shared + discriminator como padrão; contexto propaga em toda operação e evento.

**Deferred:** produto de banco concreto (critérios em [database-architecture.md](database-architecture.md) / ADR-0015); AuthZ operacional detalhada em [platform-security.md](platform-security.md) (ADR-0020); broker Event Bus produto.

---

## 2. Taxonomia

| Conceito | Papel | Camada |
|---|---|---|
| **Tenant** | Isolamento e contratação SaaS | Core + Identity |
| **Organization / Institution** | Entidade de negócio | Organization Management |
| **Unidade / Site** | Nível operacional | Config / domínio |
| **User** | Ator autenticado | Identity |

Um Tenant pode conter uma ou mais organizations/institutions. Runtime Context carrega **referência** de organization — não o ownership do cadastro.

---

## 3. Camadas

| Camada | Papel |
|---|---|
| **Tenant Context (Core)** | Contrato — tenant obrigatório (I-01) |
| **Runtime Context (Core)** | Agrega tenant, organization (ref), user, escopo (I-02) |
| **Identity Service (PS)** | Autentica e estabelece sessão com contexto |
| **Persistência** | Shared + discriminator (padrão); silo = exceção |

```text
Usuário autentica
        ↓
Identity → Tenant Context + User Context
        ↓
Runtime Context propaga
        ↓
Módulos / Domínios / PS / Event Bus no escopo do tenant
```

---

## 4. Isolamento de dados

| Modelo | Uso |
|---|---|
| **Shared + discriminador** | Padrão |
| **Database/instância por tenant** | Exceção governada (compliance/contrato) |

Invariantes:

- Todo registro de negócio pertence a exatamente um tenant
- Operação/query sem contexto de tenant = inválida
- Storage/File no escopo do tenant

---

## 5. Propagação

| Canal | Regra |
|---|---|
| **API / módulo** | Tenant do Runtime Context; PS não inventa tenant |
| **Event Bus** | Envelope com tenant; sem entrega cross-tenant padrão |
| **Jobs** | Tenant explícito ou derivado do evento disparador |
| **Cross-tenant** | Exceção + Authorization + Audit |

---

## 6. Configuração e módulos

Configuration, Feature Flag e Module Registry operam **por tenant** (mínimo).

Hierarquia reconhecida:

```text
Tenant → Organization / Institution → Unidade
```

Herança detalhada: **Q-015** (Open). Customização por código: proibida como padrão (ADR-0006).

---

## 7. Read Models

Clinical Timeline e Analytics projetam **dentro do tenant**. Relatórios multi-tenant só com papel de plataforma + Audit.

---

## 8. Fora de escopo deste documento

- Escolha de produto de banco
- Migrations / RLS / particionamento
- Claims JWT / headers HTTP
- Políticas AuthZ finas de roles MVP (Deferred em ADR-0020)

---

## 9. Referências

| Documento | Uso |
|---|---|
| [ADR-0013](adr/foundation/ADR-0013-multi-tenant-strategy.md) | Decisão formal |
| [core-platform.md](core-platform.md) | Tenant Context / I-01 |
| [event-strategy.md](event-strategy.md) | Tenant no envelope |
| [platform-services.md](platform-services.md) | Identity, Config, Feature Flag |

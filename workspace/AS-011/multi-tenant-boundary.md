# Multi-Tenant Strategy — Boundary

> Rascunho de trabalho AS-011 — NR-001/NR-002. **Não é decisão** até confirmação do pacote.

**Pré-requisitos:** ADR-0009 · ADR-0005 · ADR-0006 · ADR-0012 · `core-platform.md`  
**Última atualização:** 2026-07-04

---

## 1. Propósito

Definir **o que é multi-tenancy** na Health Platform, **o que pertence a cada camada** e **como se diferencia** de:

- **Tenant Context** (Core — contrato)
- **Runtime Context** (Core — agregador)
- **Identity Service** (PS — sessão e autenticação)
- **Organization Management** (Business Domain — cadastro organizacional)
- **Configuration / Feature Flag** (PS — variabilidade)
- **Isolamento de dados** (estratégia técnica — Q-008)

---

## 2. Definição proposta (investigativa)

**Multi-Tenant Strategy** = conjunto de decisões sobre **como a plataforma isola e opera múltiplos clientes SaaS (tenants)** — honrando Tenant Context (I-01) e Runtime Context (I-02) — **sem** acoplar regra de negócio ao mecanismo de isolamento.

| Pergunta | Direção investigativa |
|---|---|
| O que é Tenant? | Unidade de isolamento e contratação SaaS |
| Por que Core? | Contrato obrigatório em toda operação (I-01) |
| Por que não só Identity? | Identity implementa sessão; Core define invariante |

---

## 3. Tenant Context (Core) vs Identity (PS)

| Aspecto | Tenant Context (Core) | Identity Service (PS) |
|---|---|---|
| **Natureza** | Contrato / invariante | Implementação |
| **Conteúdo** | Tenant obrigatório no escopo da operação | Login, tokens, sessão, resolução de identidade |
| **Owner semântico de cadastro de tenant** | — | A validar (Identity vs domínio admin) |
| **O que não faz** | Persistência de usuários | Definir isolamento físico de dados |

```text
Usuário autentica
        ↓
Identity estabelece sessão + Tenant Context + User Context
        ↓
Runtime Context propaga (tenant, organization, user, escopo)
        ↓
Módulos / Domínios / PS / Event Bus operam no escopo do tenant
```

---

## 4. Taxonomia — Tenant vs Organization vs Institution (NR-001)

Evitar homonímia entre isolamento SaaS e estrutura organizacional.

| Conceito | Papel investigativo | Camada |
|---|---|---|
| **Tenant** | Fronteira de isolamento SaaS (cliente da plataforma) | Core (contrato) + Identity |
| **Organization / Institution** | Entidade de negócio que presta cuidado | Business Domain (Organization Management) |
| **Unidade / Site** | Nível operacional (filial, unidade) | Config / domínio |
| **User** | Ator autenticado | Identity |

**Regra investigativa:** um Tenant pode conter **uma ou mais** organizations/institutions — cardinalidade **a validar** (não inventar modelo de produto).

**OQ-C01:** Organization no Runtime Context é **referência** (ID), não ownership de cadastro no Core.

---

## 5. Modelos de isolamento (NR-002)

Alternativas **conceituais** — sem produto de banco, sem DDL.

| Modelo | Isolamento | Custo operacional | Flexibilidade |
|---|---|---|
| **A — Shared + discriminador** | Lógico (tenant em todo registro/mensagem) | Menor | Alta |
| **B — Schema por tenant** | Médio | Médio | Média |
| **C — Database / instância por tenant** | Forte | Alto | Baixa (ops) |

### Critérios de avaliação (investigativos)

| Critério | Preferência investigativa |
|---|---|
| SaaS multi-tenant padrão | Favorece A |
| Compliance extremo / dados segregados | Pode exigir C como **exceção** |
| Complexidade operacional | Penaliza C como padrão |
| Alinhamento a Event Bus (tenant no envelope) | Todos exigem tenant no contexto |

**H-MT-004:** A como padrão; C como exceção governada — **não confirmado**.

**Fora desta sessão:** implementação RLS, particionamento, connection strings por tenant.

---

## 6. O que pertence / não pertence à Multi-Tenant Strategy

| Pertence | Não pertence |
|---|---|
| Definição de Tenant e fronteiras | Regra clínica de domínio |
| Modelo de isolamento (conceitual) | Escolha de produto de banco |
| Propagação de Tenant Context | Customização por código por tenant |
| Implicações em eventos e config | AuthZ detalhada (Security) |
| Critérios para exceção silo | Schemas DDL finais |

---

## 7. Fronteiras com outros componentes

| Componente | Fronteira |
|---|---|
| **Event Foundation / Event Bus** | Envelope com tenant (ADR-0012); sem entrega cross-tenant padrão |
| **Configuration Service** | Config por tenant / instituição / unidade |
| **Feature Flag Service** | Habilitação por tenant |
| **Module Registry** | Módulos habilitados por tenant |
| **Audit Service** | Trilha inclui tenant; acesso cross-tenant auditado |
| **Authorization Service** | Políticas no escopo do tenant (+ exceção plataforma) |
| **Read Models** | Projeção por tenant (NR-005) |
| **Storage / File** | Objetos no escopo do tenant |

---

## 8. Decisões candidatas (status)

| ID | Tema | Status |
|---|---|---|
| D-001 | Definição de Tenant | Ready for Confirmation |
| D-002 | Tenant vs Organization | Ready for Confirmation |
| D-003 | Modelo de isolamento | Ready for Confirmation |
| D-004–D-008 | Ver `decisions.md` | Ready for Confirmation |

---

## 9. Open Questions ligadas

| ID | Tema |
|---|---|
| **Q-008** | Estratégia multi-tenant — **objetivo da sessão** |
| **OQ-C01** | Organization Context |
| **OQ-MT01** | Shared vs silo |
| Q-015 | Herança de configuração tenant → instituição *(relacionada, não bloqueante)* |
| Q-017 | Schema de configuração — Deferred |

---

## 10. Próximo NR

**NR-003** — `context-propagation-draft.md`: APIs síncronas, Domain Events, jobs, Read Models.

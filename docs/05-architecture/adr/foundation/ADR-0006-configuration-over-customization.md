---
title: ADR-0006 - Configuration over Customization
status: Accepted
date: 2026-07-02
deciders:
  - Architecture Team
tags:
  - architecture
  - configuration
  - customization
  - foundation
  - multi-tenant
related:
  - docs/00-introduction/vision.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
---

# ADR-0006 — Configuration over Customization

## Status

Accepted

---

# Contexto

A Health Platform é uma plataforma **SaaS multi-tenant** projetada para atender diferentes instituições de saúde — consultórios, clínicas, laboratórios, home care e outros modelos operacionais — sobre uma base arquitetural comum.

Cada instituição possui particularidades operacionais: fluxos, especialidades, integrações, branding e regras de negócio. **Variabilidade é esperada e necessária**.

Porém, tratar cada diferença com **código específico por cliente** fragmenta o produto, aumenta o custo de manutenção e compromete o modelo SaaS.

O princípio *"configuração acima de customização"* já está presente em `vision.md`, `architecture-foundation.md` e no [ADR-0003](ADR-0003-core-protection-and-extension-model.md). Este ADR formaliza a decisão e define como aplicá-la.

---

# Problema

Como garantir que:

- Diferenças entre clientes sejam tratadas por **configuração** sempre que possível.
- O produto permaneça **um único código-base** sustentável para todos os tenants.
- Regras específicas de cliente **não entrem no Core** da plataforma.
- A equipe saiba **quando configurar, compor, estender ou — excepcionalmente — customizar**.
- A plataforma se adapte às instituições **sem obrigar o profissional a adaptar-se ao software** por limitação técnica.

---

# Decisão

A Health Platform adotará **Configuration over Customization** como princípio arquitetural para tratar variabilidade entre clientes e instituições.

## 1. Hierarquia de preferência para variabilidade

Toda necessidade de diferença entre clientes deve ser resolvida seguindo esta ordem de preferência:

```text
1. Configuração           ← sempre preferir
2. Composição de módulos  ← habilitar, desabilitar, combinar
3. Extensão               ← novos módulos, serviços, integrações reutilizáveis
4. Customização por código ← exceção, com justificativa documentada
5. Alteração do Core      ← proibida sem ADR
```

Nunca pular níveis sem justificativa. Nunca ir direto a customização quando configuração ou composição resolvem.

## 2. O que é configuração

**Configuração** é a parametrização do comportamento da plataforma **sem alteração de código**, aplicada por tenant, instituição ou unidade.

**Exemplos:**

- Tempo padrão de consulta, horário de funcionamento, políticas operacionais.
- Branding por tenant (logo, cores, identidade visual).
- Módulos habilitados ou desabilitados por instituição.
- Workflows operacionais configuráveis.
- Templates de comunicação e documentos (via Template Service).
- Feature flags para rollout gradual (via Feature Flag Service).
- Formulários clínicos configuráveis (futuro — Medical Form Engine).

**Níveis conceituais de configuração:**

| Nível | Escopo |
|---|---|
| Tenant | Configurações da organização contratante da plataforma |
| Instituição | Configurações de uma instituição de saúde dentro do tenant |
| Unidade | Configurações de uma unidade, clínica ou filial |

> A implementação técnica de herança entre níveis (tenant → instituição → unidade) é pendente (Q-015).

**Platform Service responsável:** Configuration Service (configuração permanente de negócio e operação).

## 3. O que é composição de módulos

**Composição** é a habilitação e combinação de **módulos existentes** para atender um modelo operacional, sem criar código novo.

**Exemplos:**

- Clínica ambulatorial: agenda + atendimento + prontuário.
- Laboratório: módulo diagnóstico + integração LIS — sem módulo de agenda.
- Clínica com telemedicina: habilitar módulo de telemedicina além dos módulos base.

Composição não é configuração de parâmetro — é **seleção de capacidades** já disponíveis na plataforma.

## 4. O que é extensão

**Extensão** é a adição de novos módulos, Platform Services ou integrações **reutilizáveis** por múltiplos clientes — conforme [ADR-0003](ADR-0003-core-protection-and-extension-model.md).

Extensão é preferível a customização quando a necessidade será compartilhada por outros tenants ou modelos operacionais no futuro.

## 5. O que é customização

**Customização** é a implementação de **código específico para um tenant ou cliente**, fora dos mecanismos padrão de configuração, composição e extensão.

**Quando é aceitável (exceção):**

- Integração única com sistema legado de um cliente, sem reuso previsto.
- Regra de negócio genuinamente exclusiva, sem alternativa configurável, com prazo de revisão definido.
- Necessidade urgente enquanto extensão reutilizável é desenvolvida.

**Requisitos para customização excepcional:**

1. **Justificativa documentada** — por que configuração, composição e extensão não resolvem.
2. **Isolamento** — código customizado não deve alterar o Core nem comportamento de outros tenants.
3. **Revisão periódica** — customizações devem ser reavaliadas para possível absorção como configuração ou extensão.

**O que nunca é customização aceitável:**

- Regras no Core da plataforma.
- Alteração de Core Business Capabilities.
- Comportamento que afeta outros tenants sem isolamento.
- Atalho permanente para evitar investimento em configuração.

## 6. Platform Services envolvidos

| Serviço | Papel na variabilidade |
|---|---|
| **Configuration Service** | Configuração **permanente** de negócio e operação |
| **Feature Flag Service** | Habilitação **temporária ou gradual** de funcionalidades |
| **Template Service** | Templates configuráveis de mensagens e documentos |

| Serviço | Diferença |
|---|---|
| Configuration Service | Parâmetro de negócio estável (ex.: duração padrão de consulta) |
| Feature Flag Service | Rollout, experimento ou desligamento temporário (ex.: nova feature em beta) |

## 7. Regras de decisão

Antes de escrever código específico para um cliente:

| Passo | Pergunta | Se sim |
|---|---|---|
| 1 | Pode ser resolvido por **configuração**? | Usar Configuration Service |
| 2 | Pode ser resolvido **habilitando/combinando módulos**? | Composição |
| 3 | Será útil para **outros clientes** no futuro? | Criar **extensão** reutilizável |
| 4 | É genuinamente exclusivo e urgente? | **Customização** excepcional — documentar |
| 5 | Exige mudança no Core? | **ADR obrigatório** — quase sempre rejeitar para regra de cliente |

**Tabela de exemplos:**

| Necessidade | Abordagem |
|---|---|
| Clínica quer consultas de 30 min em vez de 20 | Configuração |
| Laboratório não usa agenda | Composição (desabilitar módulo) |
| Suporte a novo modelo operacional de vacinação | Extensão (novo módulo) |
| Integração com ERP legado de um único hospital | Customização excepcional |
| Cliente quer fluxo que viola Modelo Hierárquico do Cuidado | Rejeitar — não customizar o Core |

---

# Alternativas consideradas

## Alternativa A — Customização por cliente como padrão

Cada cliente recebe alterações de código específicas conforme demanda.

**Vantagens:** atende necessidades pontuais rapidamente.

**Desvantagens:** produto fragmentado; custo de manutenção insustentável; inviabiliza SaaS; regressões entre clientes.

**Resultado:** rejeitada.

---

## Alternativa B — Fluxo único e rígido para todos

Uma única configuração e fluxo para todas as instituições.

**Vantagens:** simplicidade máxima de código.

**Desvantagens:** não atende realidade da saúde; obriga profissionais a adaptar-se ao software; inviável comercialmente.

**Resultado:** rejeitada.

---

## Alternativa C — Configuração + composição + extensão

Variabilidade tratada por configuração e composição de módulos; extensão para novos modelos; customização apenas como exceção documentada.

**Vantagens:** SaaS sustentável; adaptação institucional; Core protegido; evolução planejada.

**Desvantagens:** investimento inicial em mecanismos de configuração; disciplina arquitetural exigida.

**Resultado:** adotada.

---

# Justificativa

Configuration over Customization é essencial para que a Health Platform:

- Mantenha **um produto, múltiplas configurações** — viabilidade do modelo SaaS.
- **Proteja o Core** contra regras específicas de cliente (ADR-0003).
- Permita que instituições **configurem seus processos** sem código dedicado.
- Reduza custo de manutenção e risco de regressão entre tenants.
- Alinhe implementação ao princípio da visão: *o software adapta-se ao profissional*.

---

# Regras derivadas

1. **Configurar antes de codificar** — toda demanda de cliente passa pelo fluxo de decisão deste ADR.
2. **Customização exige justificativa** — código por tenant sem documentação é proibido.
3. **Core intocável por cliente** — regras de tenant nunca entram no Core.
4. **Extensão antes de customização** — se outros clientes podem precisar, criar extensão reutilizável.
5. **Configuration ≠ Feature Flag** — parâmetro permanente vs. rollout temporário.
6. **Revisar customizações** — exceções devem ter plano de absorção ou remoção.

---

# Consequências

## Positivas

- Modelo SaaS sustentável com um código-base.
- Variabilidade institucional sem fragmentação do produto.
- Core protegido contra regras específicas de cliente.
- Base clara para Configuration Service e Development Guidelines.
- Alinhamento com visão do produto e ADRs foundation.

## Negativas

- Investimento necessário em mecanismos de configuração desde o início.
- Nem toda necessidade de cliente será resolvida imediatamente sem extensão.
- Disciplina exigida para resistir a atalhos de customização.
- Herança de configuração entre níveis ainda precisa ser definida.

---

# Impacto na Arquitetura

Este ADR influencia diretamente:

- Configuration Service e Feature Flag Service
- Module Strategy (composição por tenant)
- Multi-Tenant Strategy
- Development Guidelines
- Product Overview
- AI Context

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| [ADR-0002](ADR-0002-capability-driven-architecture.md) | Capabilities estáveis; variabilidade fora das capabilities |
| [ADR-0003](ADR-0003-core-protection-and-extension-model.md) | Configuration como mecanismo de extensão; introduziu o princípio |
| [ADR-0005](ADR-0005-platform-services.md) | Configuration Service e Feature Flag Service implementam variabilidade |

Este ADR **fecha a série de ADRs fundacionais** da Health Platform.

---

# Open Questions

| ID | Pergunta | Situação |
|---|---|---|
| Q-008 | Qual será a estratégia de multi-tenant (isolamento, schema, banco)? | Answered (estratégia) — ADR-0013; DDL Deferred |
| Q-015 | Como funciona a herança de configuração entre tenant, instituição e unidade? | Pendente |
| Q-016 | Qual o processo formal de aprovação para customização excepcional? | Pendente |
| Q-017 | Qual o schema ou modelo de dados de configuração? | Pendente — após definição de domínios |

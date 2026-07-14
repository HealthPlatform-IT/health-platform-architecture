---
title: ADR-0003 - Core Protection and Extension Model
status: Accepted
date: 2026-07-02
deciders:
  - Architecture Team
tags:
  - architecture
  - core
  - extension
  - foundation
related:
  - architecture-sessions/AS-002-business-capability-map.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - workspace/AS-002/decisions.md
---

# ADR-0003 — Core Protection and Extension Model

## Status

Accepted

---

# Contexto

A Health Platform foi concebida para atender múltiplos modelos operacionais de saúde — consultórios, clínicas, laboratórios, home care e, futuramente, hospitais — sobre uma base arquitetural comum.

Durante a **AS-002 — Business Capability Map**, foi identificado que permitir que regras específicas de clientes, especialidades ou modelos operacionais entrem no núcleo da plataforma comprometeria sua estabilidade, escalabilidade e capacidade de evolução.

A **Decisão 004** da sessão estabeleceu que a plataforma deve evoluir prioritariamente por extensão, e não por alteração do Core.

Este ADR complementa o [ADR-0002 — Capability-Driven Architecture](ADR-0002-capability-driven-architecture.md), definindo **como a plataforma protege sua fundação e cresce de forma sustentável**.

---

# Problema

Como garantir que:

- O núcleo da plataforma permaneça **pequeno, estável e protegido** contra regras variáveis.
- Novos modelos operacionais e funcionalidades sejam adicionados **sem reestruturar a fundação**.
- Diferenças entre clientes sejam tratadas por **configuração** sempre que possível, evitando customização por código.
- A equipe e ferramentas de IA saibam **quando algo pertence ao Core** e quando deve ser implementado por extensão.

---

# Decisão

A Health Platform adotará o modelo **Core Protection and Extension** como princípio de evolução arquitetural.

## 1. Core pequeno e protegido

O **Core** é o núcleo arquitetural protegido da plataforma — a fundação estável sobre a qual módulos, serviços e extensões são construídos.

O Core deve permanecer:

- **Pequeno** — com o menor conjunto possível de responsabilidades.
- **Estável** — mudanças estruturais são raras e exigem justificativa forte.
- **Protegido** — contra regras específicas de clientes, especialidades ou modelos operacionais.

## 2. Crescimento por extensão

A plataforma evolui **por extensão**, não por alteração estrutural constante do Core.

Novos modelos operacionais, funcionalidades e comportamentos específicos devem ser adicionados por mecanismos de extensão, preservando a fundação existente.

## 3. Mecanismos de extensão

| Mecanismo | Quando usar |
|---|---|
| **Modules** | Funcionalidades de negócio derivadas de Business Domains e capabilities |
| **Platform Services** | Responsabilidades transversais reutilizáveis por múltiplos módulos |
| **Configuration** | Diferenças de comportamento entre tenants, instituições ou unidades |
| **Events** | Comunicação desacoplada entre módulos e serviços |
| **Integrations** | Conectores com sistemas externos |

## 4. Configuração acima de customização

Sempre que possível, diferenças entre clientes devem ser tratadas por **configuração**.

Customização por cliente — código específico para um tenant — deve ser tratada como **exceção**, não como padrão.

> O detalhamento deste princípio será registrado no ADR-0006 — Configuration over Customization.

## 5. Regras de decisão

Antes de adicionar qualquer funcionalidade ao Core, responder:

| Pergunta | Se sim → Core | Se não → Extensão |
|---|---|---|
| É necessário para **toda** operação da plataforma? | Provável Core | Módulo ou serviço |
| É estável e independente de modelo operacional? | Provável Core | Extensão |
| É específico de um cliente, especialidade ou fluxo? | **Nunca Core** | Configuração ou módulo |
| É reutilizável por múltiplos módulos? | Platform Service | Módulo de domínio |
| Pode ser desabilitado por tenant sem afetar a fundação? | Extensão | — |

---

# O que pertence ao Core

Conceitualmente, o Core abriga:

- A fundação das **Core Business Capabilities**, especialmente **Governar** (como capability — implementação em PS).
- O **Modelo Hierárquico do Cuidado** (ADR-0001) como estrutura clínica central.
- **Contratos estáveis** compartilhados entre módulos e serviços.
- Mecanismos de **extensão** que permitem crescimento sem alterar a fundação.

> **Refinamento AS-005 (Q-007 Answered):** O termo operacional **Core Platform** designa os **8 contratos/invariantes estruturais** (Tenant Context, Module Registry, Extension Mechanism, Event Foundation, Runtime Context, etc.). **Implementações** de identidade, autorização, auditoria, configuração e demais transversais são **Platform Services** (ADR-0005) — não o Core Platform. Ver `docs/05-architecture/core-platform.md`.

O Core fornece a fundação — não implementa regras de negócio específicas.

---

# O que NÃO pertence ao Core

O Core **não deve conter**:

- Regras específicas de cliente, especialidade ou modelo operacional.
- Fluxos clínicos de modelos operacionais específicos (ambulatorial, laboratorial, home care).
- Lógica de módulos de negócio (agenda, prescrição, laudos, faturamento).
- Integrações com sistemas externos específicos.
- Customizações por tenant no código.
- Regras que variam entre instituições e poderiam ser configuradas.

---

# Alternativas consideradas

## Alternativa A — Core monolítico

Centralizar a maior parte das funcionalidades no Core, incluindo regras de negócio e fluxos clínicos.

**Vantagens:** implementação inicial mais simples; tudo em um lugar.

**Desvantagens:** Core instável e difícil de manter; impossível suportar múltiplos modelos operacionais; alto custo de evolução; acoplamento generalizado.

**Resultado:** rejeitada.

---

## Alternativa B — Customização por cliente no código

Permitir alterações específicas por cliente diretamente no código da plataforma.

**Vantagens:** atende necessidades pontuais rapidamente.

**Desvantagens:** código fragmentado; impossível manter múltiplos clientes; alto custo de manutenção; quebra o modelo SaaS.

**Resultado:** rejeitada.

---

## Alternativa C — Core protegido + extensão

Manter o Core pequeno e estável; adicionar funcionalidades por módulos, Platform Services, configuração, eventos e integrações.

**Vantagens:** estabilidade da fundação; suporte a múltiplos modelos operacionais; evolução sustentável; redução de risco.

**Desvantagens:** exige disciplina arquitetural; mecanismos de extensão precisam ser bem projetados; abstração inicial maior.

**Resultado:** adotada.

---

# Justificativa

Proteger o Core e evoluir por extensão permite que a Health Platform:

- Suporte múltiplos modelos operacionais de saúde sobre uma base comum.
- Evolua continuamente sem comprometer a fundação.
- Reduza o custo de manutenção e o risco de regressões.
- Mantenha o modelo SaaS viável — um Core, múltiplas configurações e extensões.
- Preserve os investimentos em arquitetura orientada por capabilities (ADR-0002).

---

# Regras derivadas

1. **Nunca acoplar regras de cliente ao Core** — regras específicas vão para configuração, módulos ou extensões.
2. **Novos modelos operacionais via extensão** — laboratório, home care e hospital não alteram o Core.
3. **Platform Services para transversal** — responsabilidades compartilhadas não entram em módulos de domínio nem no Core de negócio.
4. **Configuração antes de código** — parametrizar antes de implementar customização.
5. **Alteração do Core exige ADR** — mudanças estruturais no Core devem ser registradas e justificadas.
6. **Módulos não alteram o Core** — módulos consomem contratos do Core; não o modificam.

---

# Consequências

## Positivas

- Fundação estável e previsível para toda a plataforma.
- Suporte a múltiplos modelos operacionais sem reescrita.
- Redução de risco em evoluções e novos clientes.
- Base clara para definição de módulos, Platform Services e configuração.
- Viabiliza o modelo SaaS multi-tenant de forma sustentável.

## Negativas

- Exige disciplina arquitetural contínua da equipe e de ferramentas de IA.
- Mecanismos de extensão (configuração, eventos, plugins) precisam ser projetados com antecedência.
- Pode parecer mais lento no início do que colocar tudo no Core.
- A fronteira exata entre Core, Platform Services e módulos foi refinada na **AS-005** (Q-007 **Answered** — `docs/05-architecture/core-platform.md`, `module-strategy.md`).

---

# Impacto na Arquitetura

Este ADR influencia diretamente:

- Module Strategy
- Platform Services (ADR-0005)
- Configuration Strategy (ADR-0006)
- Event Model
- Multi-Tenant Strategy
- Product Overview
- Development Guidelines

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| [ADR-0001 — Modelo Hierárquico do Cuidado](ADR-0001-hierarchical-care-model.md) | O modelo clínico faz parte da fundação estável do Core |
| [ADR-0002 — Capability-Driven Architecture](ADR-0002-capability-driven-architecture.md) | As capabilities definem o que é fundação; este ADR define como protegê-la |
| ADR-0004 — Communication vs. Integration | Mecanismos de extensão para capabilities distintas |
| ADR-0005 — Platform Services | Principal mecanismo de extensão transversal |
| ADR-0006 — Configuration over Customization | Detalhamento do princípio de configuração |
| [ADR-0008 — Business Domain Map](ADR-0008-business-domain-map.md) | Critérios Domain vs PS vs Read Model |

Nenhum ADR futuro deverá introduzir regras específicas de cliente ou modelo operacional no Core sem que este documento seja revisado.

---

# Open Questions

| ID | Pergunta | Situação |
|---|---|---|
| Q-007 | Qual é a fronteira exata entre Core, Platform Services e Modules? | **Answered** — ADR-0009, docs AS-005 |
| Q-008 | Qual será a estratégia de multi-tenant (isolamento, schema, banco)? | Answered (estratégia) — ADR-0013; DDL Deferred |
| Q-009 | Quais mecanismos de extensão serão suportados na implementação (plugins, feature flags, eventos)? | Parcial conceitual — Extension Mechanism no Core confirmado; implementação pendente |

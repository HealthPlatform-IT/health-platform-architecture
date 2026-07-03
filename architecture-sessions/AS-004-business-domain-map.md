---
id: AS-004
title: Business Domain Map
status: Completed
version: 1.0.0
created: 2026-07-02
updated: 2026-07-02
author: Architecture Team
reviewers: []
phase: Product & Architecture Foundation
category: Business Discovery
priority: Critical
estimated-impact: High
tags:
  - business-domains
  - domain-map
  - capability-driven
  - Q-002
related:
  - docs/03-capabilities/core-business-capabilities.md
  - docs/03-capabilities/business-capability-map.md
  - docs/02-business-processes/healthcare-operating-model.md
  - docs/02-business-processes/care-journey-lifecycle.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md
  - architecture-sessions/AS-003-care-journey-lifecycle.md
  - ai-context/open-questions.md
  - docs/04-domain/business-domains.md
  - docs/04-domain/domain-map.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - workspace/AS-004/confirmation-package.md
---

# AS-004 — Business Domain Map

## Informações Gerais

| Campo | Valor |
|---|---|
| Sessão | AS-004 |
| Tema | Business Domain Map |
| Status | Completed |
| Fase | Product & Architecture Foundation |
| Categoria | Business Discovery |
| Prioridade | Critical |
| Impacto Estimado | High |
| Responsável | Architecture Team |
| Data | 2026-07-02 |
| Open Question principal | Q-002 |

---

## Objetivo

Derivar os **Business Domains** da Health Platform a partir das oito Core Business Capabilities e das 35 sub-capabilities do Business Capability Map.

Esta sessão deve responder **Q-002**:

> Como as Core Business Capabilities e suas sub-capabilities devem ser agrupadas em Business Domains com responsabilidades claras, fronteiras coerentes e baixo acoplamento?

---

## Hipótese Inicial

Business Domains **emergem do agrupamento de sub-capabilities relacionadas**, e não de uma cópia direta das oito Core Business Capabilities nem de módulos/telas existentes.

Exemplo conceitual (não definitivo):

- **Relacionar** → Patient Relationship, Professional Management, Organization Management, Health Network
- **Registrar** → Clinical Documentation, Medical Documents, Clinical Timeline, Prescription, Exam Request, Diagnostic Report

A sessão deve validar, refinar ou rejeitar essa hipótese.

---

## Contexto

A AS-002 consolidou o Business Capability Map (35 sub-capabilities). A AS-003 respondeu Q-001 e formalizou o ciclo de vida da Institution Care Journey (ADR-0007).

A sequência de derivação obrigatória (ADR-0002):

```text
Healthcare Operating Model
        ↓
Institution Care Journey
        ↓
Core Business Capabilities
        ↓
Business Capability Map
        ↓
Business Domains          ← esta sessão
        ↓
Modules → Features
```

O ADR-0002 distingue explicitamente:

| Conceito | Representa |
|---|---|
| Core Business Capability | Competência permanente de negócio |
| Business Capability | Sub-capability do mapa |
| **Business Domain** | Fronteira de responsabilidade derivada do mapa |
| Platform Service | Implementação transversal reutilizável |
| Module | Solução implementável |

O ADR-0005 cataloga 17 Platform Services — muitas sub-capabilities de **Governar** e parte de **Comunicar/Integrar** podem ser suportadas por Platform Services, não por domínios de negócio.

---

## Problema

Sem Business Domains definidos:

- Não há fronteiras claras para módulos, APIs e equipes.
- Risco de organizar por telas (agenda, prontuário) ou microsserviços prematuros.
- Risco de domínios excessivamente grandes (um "Clinical" monolítico) ou fragmentados (um domínio por sub-capability).
- Q-004 (agregados), Q-007 (fronteira Core/Services/Modules) e Q-018 permanecem bloqueadas.

---

## Escopo da Sessão

### Dentro do escopo

1. Critérios para identificar e rejeitar um Business Domain.
2. Agrupamento das 35 sub-capabilities em domínios candidatos.
3. Classificação: domínios centrais, de suporte e de extensão operacional.
4. Fronteira Business Domain vs Platform Service (parcial — Q-007).
5. Mapeamento sub-capability → domínio candidato.
6. Impactos futuros em módulos, APIs e Bounded Contexts — **conceituais**.
7. Documentação oficial: `business-domains.md` e `domain-map.md`.

### Fora do escopo

- Bounded Contexts detalhados, Aggregates DDD (Q-004).
- Event Model (Q-003 — Deferred).
- Banco de dados, APIs, microsserviços, UI, implementação.
- MVP por sub-capability (Q-006).
- Lista final de módulos.

---

## Premissas

- ADRs 0001–0007 permanecem Accepted.
- Business Domains derivam do mapa de capabilities — não de módulos ou telas.
- Governar é transversal — nem toda sub-capability de Governar vira domínio.
- Institution Care Journey (ADR-0007) é âncora para domínios centrais ao cuidado.

---

## Restrições

- Não criar domínios baseados em telas ou módulos legados.
- Não confundir domínio com Core Business Capability, Platform Service ou módulo.
- Não inventar decisões — registrar hipóteses e open questions quando imaturo.
- Não detalhar implementação técnica.

---

## Perguntas Arquiteturais

### Estrutura

- Os domínios emergem de sub-capabilities agrupadas ou de outra unidade?
- Qual faixa razoável de quantidade de domínios nesta fase?
- Uma sub-capability pode pertencer a mais de um domínio?

### Tipologia

- Quais domínios são centrais (âncora na Institution Care Journey)?
- Quais são de suporte (relacionamento, organização)?
- Quais são extensão de modelos operacionais?
- Quais responsabilidades de Governar viram domínio vs Platform Service?

### Fronteiras

- Como evitar domínios por tela ou módulo?
- Como evitar domínios grandes demais ou pequenos demais?
- Clinical Timeline: domínio, visão cross-domain ou parte de Clinical Documentation?

### Relações

- Como domínios consomem Platform Services?
- Relação futura domínio : módulo : Bounded Context?
- Q-018: múltiplas jornadas ativas impacta fronteira de domínio?

### Lacunas

- Q-005: capabilities administrativas/financeiras — domínio ou fora do escopo inicial?

---

## Critérios para Identificar um Business Domain

| # | Critério |
|---|---|
| 1 | Agrupa sub-capabilities com **alta coesão de negócio** e vocabulário ubíquo distinto |
| 2 | Possui **responsabilidade clara** e ownership conceitual |
| 3 | Pode evoluir com **baixo acoplamento** a outros domínios |
| 4 | Representa **regras de negócio** — não implementação transversal |
| 5 | **Não** é tela, módulo, microsserviço ou Core Business Capability |
| 6 | **Não** duplica responsabilidade já coberta por Platform Service transversal |

---

## Critérios para Rejeitar um Agrupamento

| # | Critério de rejeição |
|---|---|
| 1 | Baseado em tela ou módulo ("domínio agenda", "domínio prontuário") |
| 2 | Espelha 1:1 uma Core Business Capability sem necessidade |
| 3 | Domínio com uma única sub-capability trivial |
| 4 | "Mega domínio" misturando clínico + financeiro + governança |
| 5 | Sub-capability melhor como Platform Service (Identity, Audit, Communication) |
| 6 | Fronteira instável por open question não resolvida — marcar pendente |

---

## Alternativas a Avaliar

### Alternativa A — Um domínio por Core Business Capability (8 domínios)

**Vantagens:** simples; alinhado às 8 cores.

**Desvantagens:** ignora granularidade do mapa; domínios possivelmente grandes demais (Registrar, Governar).

---

### Alternativa B — Um domínio por sub-capability (35 domínios)

**Vantagens:** granularidade máxima.

**Desvantagens:** fragmentação; orquestração excessiva; anti-padrão para esta fase.

---

### Alternativa C — Agrupamento por sub-capabilities relacionadas (hipótese inicial)

**Vantagens:** coesão de negócio; reflete o mapa; flexível.

**Desvantagens:** exige critérios explícitos; subjetividade nos agrupamentos.

---

### Alternativa D — Domínios por modelo operacional

Ex.: domínio Ambulatory, domínio Diagnostic, domínio Home Care.

**Vantagens:** próximo da operação.

**Desvantagens:** duplicação entre modelos; viola base comum + extensão (ADR-0003). **Provável rejeição.**

---

### Alternativa E — Híbrido

Domínios de negócio (por agrupamento de sub-capabilities) + transversais via Platform Services.

**Vantagens:** alinhado a ADR-0005; separa regra de negócio de infraestrutura.

**Desvantagens:** exige fronteira Domain/Service bem documentada (Q-007 parcial).

---

## Trade-offs

- **Simplicidade (8 domínios) vs precisão (agrupamento fino):** mais domínios = fronteiras claras, mais orquestração.
- **Domínio vs Platform Service:** centralizar em serviço reduz duplicação, mas não substitui domínio quando há regras de negócio próprias.
- **Cuidado central vs suporte:** domínios âncora na jornada vs domínios de relacionamento/organização.

---

## Riscos

### Risco 1 — Domínios baseados em telas

**Mitigação:** critérios de rejeição; rastrear origem no mapa de capabilities.

### Risco 2 — Fragmentação em Registrar

Prescription, Clinical Documentation, Clinical Timeline como domínios separados demais.

**Mitigação:** avaliar coesão clínica vs separação por responsabilidade.

### Risco 3 — Governar como domínios

Identity, Audit, Configuration como domínios duplicando Platform Services.

**Mitigação:** matriz capability → domínio **ou** Platform Service.

### Risco 4 — Lacuna administrativa/financeira (Q-005)

Financial & ERP Integration sem domínio financeiro definido.

**Mitigação:** registrar como open question; não forçar domínio prematuro.

### Risco 5 — Q-018

Múltiplas jornadas ativas pode alterar fronteira do domínio de cuidado.

**Mitigação:** decisão conservadora; documentar ressalva.

---

## Método de Descoberta

```text
1. Inventariar     → 35 sub-capabilities (fonte única: business-capability-map.md)
2. Clusterizar     → agrupar por coesão de negócio e vocabulário
3. Separar transversal → Governar/Comunicar/Integrar → Platform Service?
4. Validar         → critérios de identificação/rejeição
5. Classificar     → central · suporte · extensão operacional
6. Mapear          → capability-to-domain-mapping.md
7. Documentar      → business-domains.md + domain-map.md
```

**Anti-padrões:** começar por módulos, telas, entidades de banco ou microsserviços.

---

## Decisões Esperadas

- Critérios de decomposição aceitos.
- Lista de Business Domains candidatos validados.
- Mapeamento sub-capability → domínio (draft oficial).
- Fronteira preliminar Domain vs Platform Service.
- Encaminhamentos: Q-004, Q-005, Q-007, Q-018.
- Possível ADR Business Domain Decomposition (se fundacional).

---

## Documentos Impactados

| Documento | Ação |
|---|---|
| `docs/04-domain/business-domains.md` | Criar — Draft |
| `docs/04-domain/domain-map.md` | Criar — Draft |
| `docs/03-capabilities/business-capability-map.md` | Referência cruzada (se necessário) |
| `ai-context/open-questions.md` | Q-002 → Answered; parcial Q-004, Q-005, Q-007 |
| `ARCHITECTURE_INDEX.md` | AS-004 em andamento / encerrada |

---

## Open Questions Esperadas

| ID | Tratamento na sessão |
|---|---|
| Q-002 | Objeto central — responder ao encerrar |
| Q-004 | Parcial — encaminhar |
| Q-005 | Parcial — domínios admin/financeiros |
| Q-007 | Parcial — fronteira Core / Services / Modules |
| Q-018 | Parcial — cardinalidade de jornadas |
| Q-019+ | Novas emergentes → open-questions.md |

---

## Entregáveis ao Encerrar

- [x] Q-002 respondida na documentação oficial
- [x] `workspace/AS-004/` completo
- [x] `docs/04-domain/business-domains.md` — Draft v0.1.0
- [x] `docs/04-domain/domain-map.md` — Draft v0.1.0
- [x] ADR-0008 — Business Domain Map (Accepted)
- [x] `open-questions.md` atualizado
- [x] `ARCHITECTURE_INDEX.md` atualizado

**Encerrada em:** 2026-07-02  
**Confirmação:** Pacote AS-004 confirmado explicitamente pelo usuário.

---

## Próxima Architecture Session

**AS-005 — Core Platform** (ou continuação conforme roadmap) — após domínios consolidados.

---

## Questões em Aberto

| ID | Pergunta | Status |
|---|---|---|
| Q-002 | Como agrupar sub-capabilities em Business Domains? | ✅ Answered — ADR-0008 |
| Q-001 | Início da jornada | ✅ Answered — AS-003 |

---

## Checklist de Encerramento

- [x] Hipótese inicial confirmada ou refinada.
- [x] Critérios de domínio documentados.
- [x] Mapeamento 39 sub-capabilities → domínios.
- [x] Fronteira Domain / Platform Service documentada.
- [x] Q-002 respondida.
- [x] Documentação oficial criada.
- [x] Próxima sessão indicada.

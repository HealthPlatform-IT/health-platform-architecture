---
id: AS-006
title: Medical Form Engine
status: Prepared
version: 0.1.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
reviewers: []
phase: Product & Architecture Foundation
category: Product Architecture
priority: Medium
estimated-impact: High
tags:
  - platform-services
  - medical-form-engine
  - clinical-record
  - Q-013
related:
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/module-strategy.md
  - docs/04-domain/business-domains.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - architecture-sessions/AS-005-core-platform-boundary.md
  - ai-context/open-questions.md
  - workspace/AS-006/README.md
---

# AS-006 — Medical Form Engine

## Informações Gerais

| Campo | Valor |
|---|---|
| Sessão | AS-006 |
| Tema | Medical Form Engine |
| Status | Prepared |
| Fase | Product & Architecture Foundation |
| Categoria | Product Architecture |
| Prioridade | Medium |
| Impacto Estimado | High |
| Responsável | Architecture Team |
| Data de preparação | 2026-07-03 |
| Open Question principal | Q-013 *(escopo Medical Form Engine)* |

---

## Objetivo

Definir o **escopo, responsabilidades e contratos conceituais** do Platform Service **Medical Form Engine** — distinguindo-o de:

- **Clinical Record Domain** (conteúdo e semântica clínica)
- **Document Engine** (documentos formais finalizados — AS-007)
- **Template Service** (templates genéricos — influência em Q-014)
- **Módulos M-03 Attendance** e **M-04 Clinical Documentation**

Esta sessão deve responder **Q-013** (parte Medical Form Engine):

> Qual será o escopo detalhado e os contratos do Platform Service Medical Form Engine?

---

## Hipótese Inicial (H-MFE-001)

O Medical Form Engine é um **Platform Service Strong Candidate** que fornece **mecanismo de captura estruturada** — definição de formulários, renderização em atendimento e validação estrutural de campos — **sem** possuir regras de negócio clínicas nem ownership de conteúdo clínico.

| Camada | Responsabilidade |
|---|---|
| **Medical Form Engine (PS)** | Estrutura de formulário, renderização, validação de tipo/campo |
| **Clinical Record Domain** | Semântica clínica, evolução, diagnóstico, persistência de conhecimento |
| **M-03 / M-04** | Orquestração de UX e fluxo — consomem Engine por contrato |

**A validar:** se "definição de formulário por especialidade" pertence ao Engine ou ao domínio com Engine apenas executando schema.

---

## Contexto

A **AS-005** (ADR-0009) classificou Medical Form Engine como **Strong Candidate** com fronteira **Needs Review** (OQ-PS05):

- Consumido por **M-03 Attendance** e **M-04 Clinical Documentation**
- ADR-0005 cataloga como **Identificado** — escopo preliminar em capability **Registrar**
- Tensão: *definição de formulário* pode parecer domínio; Engine não deve virar prontuário (R-MFE-01)

**Inputs consolidados:**

- 16 Business Domains — Clinical Record define conhecimento clínico
- Module Strategy — M-03 (Executar) vs M-04 (Registrar) separados
- Classification matrix — PS = mecanismo transversal, não domínio
- `platform-services-boundary.md` — recomendação provisória AS-005

---

## Problema

Sem decisão sobre Medical Form Engine:

- Risco de **duplicar** lógica de formulários em M-03 e M-04
- Risco do Engine **absorver** regras clínicas (prontuário disfarçado de PS)
- Risco de **confundir** com Document Engine (formulário vs documento formal)
- **Q-013** e parte de **Q-014** permanecem Open
- Implementação futura de captura clínica fica sem contrato

---

## Premissas

- Medical Form Engine **não** é Business Domain nem Module
- Clinical Record Domain **mantém** ownership do conteúdo clínico capturado
- Document Engine trata **documentos formais** — escopo detalhado em AS-007
- Nenhuma decisão de stack, API REST, schema JSON ou biblioteca de formulários nesta sessão
- Configuration over Customization (ADR-0006) — formulários configuráveis por tenant/instituição

---

## Restrições

- Não reabrir AS-004 (domínios) nem AS-005 (fronteiras Core/PS/Module)
- Não decidir Document Engine em profundidade (AS-007)
- Não implementar código
- Preservar separação **Executar (M-03)** vs **Registrar (M-04)**

---

## Perguntas Arquiteturais

### Fronteira PS ↔ Domínio

1. O que é **form definition** vs **form instance** vs **clinical content**?
2. Quem define **semântica clínica** de um campo — domínio ou Engine?
3. O Engine pode armazenar **templates de formulário** ou isso é Template Service?
4. Validações clínicas (ex.: diagnóstico obrigatório) — domínio ou Engine?

### Consumo por módulos

5. M-03 e M-04 consomem o **mesmo contrato** ou contratos distintos?
6. Captura em atendimento (M-03) vs evolução estruturada (M-04) — mesmo Engine?
7. O shell M-02 orquestra formulários ou apenas hospeda módulos?

### Relação com outros PS

8. Medical Form Engine vs **Template Service** — overlap? *(Q-014)*
9. Medical Form Engine vs **Document Engine** — onde termina captura e começa documento?
10. File Service — anexos em formulários: responsabilidade de quem?

### Configuração e extensão

11. Formulários por especialidade — Configuration Service, Engine ou domínio?
12. Extension Modules (M-14, M-15) podem registrar **schemas de formulário** via Engine?
13. Feature Flag — habilitar tipos de formulário por tenant?

### Riscos e invariantes

14. Como evitar Engine virar prontuário (R-MFE-01)?
15. Quais invariantes I-01 a I-10 da AS-005 se aplicam?

---

## Escopo da Investigação (NR)

| NR | Tema | Artefato esperado |
|---|---|---|
| NR-001 | Fronteira PS vs Clinical Record Domain | `medical-form-engine-boundary.md` |
| NR-002 | Form definition / instance / clinical content | Mesmo artefato |
| NR-003 | Consumo M-03 vs M-04 | Seção em boundary + `decisions.md` |
| NR-004 | Template Service e Q-014 (parcial) | Seção em boundary |
| NR-005 | Preview Document Engine (sem decidir AS-007) | Seção de fronteira |
| NR-006 | Contratos conceituais do PS | `decisions.md` |
| NR-007 | Pacote de confirmação | `confirmation-package.md` |

---

## Alternativas a Avaliar

### Alternativa A — Medical Form Engine como PS dedicado (H-MFE-001)

Engine transversal: schema, render, validação estrutural. Domínio possui conteúdo.

**Vantagens:** reutilização M-03/M-04; alinhado ADR-0005 e AS-005.  
**Desvantagens:** fronteira sutil com domínio; risco de scope creep.

### Alternativa B — Formulários dentro do Clinical Record Domain

M-04 implementa tudo; M-03 delega ao domínio.

**Vantagens:** semântica unificada.  
**Desvantagens:** duplicação com Attendance; viola critério PS (2+ módulos).

### Alternativa C — Formulários embutidos em cada módulo

M-03 e M-04 cada um com motor próprio.

**Vantagens:** autonomia de módulo.  
**Desvantagens:** duplicação; inconsistência UX; rejeitada na AS-005.

---

## Riscos

| ID | Risco | Mitigação |
|---|---|---|
| R-MFE-01 | Engine vira prontuário | Ownership clínico no domínio; Engine só mecanismo |
| R-MFE-02 | Definição de formulário no PS com regra clínica | Separar schema estrutural vs semântica |
| R-MFE-03 | Overlap com Template Service | NR-004; não absorver Q-014 inteira |
| R-MFE-04 | Overlap com Document Engine | NR-005; captura ≠ documento formal |
| R-MFE-05 | M-03 e M-04 acoplados via Engine | Contratos estáveis; domínio como destino dos dados |

---

## Resultado Esperado

Ao encerrar AS-006:

1. Medical Form Engine **Confirmed** ou **Strong Candidate refinado** com fronteira explícita
2. Contratos conceituais: Form Definition, Form Instance, Clinical Content Capture
3. Matriz de responsabilidades Engine vs Clinical Record vs M-03/M-04
4. Q-013 **Answered** (parte Medical Form Engine) ou **Partial** com critério claro para AS-007
5. Q-014 avançada se Template overlap resolvido
6. Possível ADR-0010 ou atualização ADR-0005 + `platform-services.md`
7. Nenhum código

---

## Documentos Impactados (pós-confirmação)

| Documento | Tipo de impacto |
|---|---|
| ADR-0010 ou ADR-0005 update | Decisão formal |
| `docs/05-architecture/platform-services.md` | Seção Medical Form Engine |
| `ai-context/open-questions.md` | Q-013, possivelmente Q-014 |
| `ai-context/architecture-foundation.md` | Catálogo PS |
| `ARCHITECTURE_INDEX.md` | Status AS-006 |

---

## Open Questions Relacionadas

| ID | Relação |
|---|---|
| **Q-013** | Pergunta central — Medical Form Engine |
| Q-014 | Template Service — revisar parcialmente |
| Q-009 | Implementação de extensão — fora de escopo |
| OQ-PS05 | PS ou parte de Clinical Record? — resolver |

---

## Critério de Encerramento

1. Pacote em `workspace/AS-006/decisions.md` com status Ready for Confirmation
2. `medical-form-engine-boundary.md` com exemplos reais (anamnese, evolução, formulário de atendimento)
3. Fronteira Document Engine esboçada sem decidir AS-007
4. `confirmation-package.md` revisado
5. Nenhuma decisão de tecnologia

---

## Workspace

[`workspace/AS-006/`](../workspace/AS-006/README.md)

---

## Sequência recomendada

```text
AS-005 ✅ → AS-006 (Medical Form Engine) → AS-007 (Document Engine)
```

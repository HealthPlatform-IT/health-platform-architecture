---
id: AS-003
title: Care Journey Lifecycle
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
  - care-journey
  - lifecycle
  - institution-care-journey
  - operating-model
  - Q-001
related:
  - docs/02-business-processes/healthcare-operating-model.md
  - docs/02-business-processes/care-journey-lifecycle.md
  - docs/03-capabilities/core-business-capabilities.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - architecture-sessions/AS-002-business-capability-map.md
  - ai-context/architecture-foundation.md
  - ai-context/open-questions.md
  - workspace/AS-003/
---

# AS-003 — Care Journey Lifecycle

## Informações Gerais

| Campo | Valor |
|---|---|
| Sessão | AS-003 |
| Tema | Care Journey Lifecycle |
| Status | Completed |
| Fase | Product & Architecture Foundation |
| Categoria | Business Discovery |
| Prioridade | Critical |
| Impacto Estimado | High |
| Responsável | Architecture Team |
| Data | 2026-07-02 |
| Open Question principal | Q-001 |

---

## Objetivo

Definir o **ciclo de vida da Institution Care Journey** na Health Platform.

Esta sessão deve responder **Q-001**:

> Quando uma instituição assume responsabilidade pelo cuidado de um paciente dentro da Health Platform?

Além do início da jornada, a sessão deve investigar o que mantém uma jornada ativa, quando ela pode ser pausada, encerrada ou reaberta, e como esses comportamentos variam por modelo operacional — sem decidir implementação técnica.

---

## Hipótese Inicial

A Health Platform representa apenas o **trecho da jornada em que uma instituição assume responsabilidade assistencial** — a **Institution Care Journey** — e não a vida clínica integral do paciente.

O início dessa jornada é marcado por um **gatilho operacional** (não apenas cadastro), que pode variar conforme o modelo operacional da instituição, dentro de um vocabulário comum de estados e transições de ciclo de vida.

---

## Contexto

A AS-002 consolidou as oito Core Business Capabilities e o Business Capability Map. A plataforma está orientada pela **Jornada de Cuidado** como centro conceitual (decisão 005 do workspace AS-002, ADR-0002).

O **Modelo Hierárquico do Cuidado** (ADR-0001) define seis níveis conceituais:

```text
Paciente → Jornada de Cuidado → Episódio de Cuidado → Atendimento → Evento Clínico → Artefato Clínico
```

Porém o ADR-0001 **não define** quando uma Jornada de Cuidado institucional começa, termina ou muda de estado.

A documentação em `ai-context/architecture-foundation.md` (seção 7) já distingue:

| Conceito | Significado |
|---|---|
| **Patient Life Journey** | História ampla de saúde da pessoa, conceitual e potencialmente distribuída por todo o ecossistema de saúde. |
| **Institution Care Journey** | Trecho da jornada representado e gerenciado por uma instituição dentro da Health Platform. |

A plataforma passa a representar a jornada **quando uma instituição assume responsabilidade por algum trecho do cuidado** — mas o critério exato desse momento ainda não foi formalizado.

O documento `healthcare-operating-model.md` está em Draft e incompleto. O documento oficial `care-journey-lifecycle.md` ainda não existe.

**Q-001** está registrada como **Critical** em `ai-context/open-questions.md` e bloqueia a AS-004 (Business Domain Map) e refinamentos de Q-004 (agregados do domínio clínico).

---

## Problema

Sem critério claro para início, manutenção, pausa, encerramento e reabertura da Institution Care Journey:

- Não é possível modelar corretamente domínios de negócio nem fronteiras de responsabilidade.
- A timeline clínica e o Clinical Workspace carecem de âncora conceitual.
- Eventos de negócio relevantes permanecem indefinidos.
- Diferentes modelos operacionais (ambulatorial, diagnóstico, home care, telemedicina) podem ser modelados de forma inconsistente.
- Corremos o risco de confundir **cadastro/vínculo** com **responsabilidade assistencial**.

---

## Escopo da Sessão

### Dentro do escopo

1. O que inicia uma Institution Care Journey.
2. O que mantém uma jornada ativa.
3. Quando uma jornada pode ser pausada.
4. Quando uma jornada pode ser encerrada.
5. Quando uma jornada pode ser reaberta.
6. Diferença entre Patient Life Journey e Institution Care Journey.
7. Diferença entre Care Journey, Care Episode e Attendance.
8. Gatilhos por modelo operacional:
   - Clínica ambulatorial
   - Telemedicina
   - Laboratório
   - Centro de imagem
   - Home care
   - Hospital (futuro)
   - Medicina ocupacional
9. Impactos no Modelo Hierárquico do Cuidado (ADR-0001).
10. Impactos futuros em Business Domains, APIs, eventos e Clinical Workspace — **conceituais**, sem desenho técnico.

### Fora do escopo

- Derivação de Business Domains (AS-004 / Q-002).
- Agregados DDD detalhados (Q-004).
- Event Model e tecnologia do Event Bus (Q-003 — Deferred).
- Implementação (código, APIs, banco de dados, schemas).
- Escopo MVP por sub-capability (Q-006).
- Operação hospitalar em profundidade (apenas hipóteses e reserva conceitual).

---

## Premissas

- O **ADR-0001** (Modelo Hierárquico do Cuidado) permanece Accepted — esta sessão complementa, não substitui.
- A Jornada de Cuidado institucional **não é** sinônimo da vida clínica integral do paciente.
- O prontuário é uma **visão** da jornada, não o centro conceitual da plataforma.
- Gatilhos podem variar por modelo operacional, mas o **conceito de jornada e seus estados** deve ser único no Core.
- Configuração acima de customização (ADR-0006) — variações entre instituições devem ser configuráveis quando possível.
- Documentação antes de implementação.

---

## Restrições

- Não centrar a arquitetura em consulta, agenda ou prontuário isolados.
- Não presumir que a plataforma conhece toda a Patient Life Journey.
- Não decidir stack, schema de banco ou contratos de API.
- Não inventar decisões — o que não estiver maduro deve ser registrado como hipótese ou open question.
- Não reabrir ADRs Accepted sem justificativa formal e novo ADR.

---

## Perguntas Arquiteturais

### Ciclo de vida

- Qual evento ou condição marca o **início** de uma Institution Care Journey?
- O cadastro ou vínculo institucional (capability Relacionar) é suficiente, necessário, ou apenas pré-requisito?
- O que **mantém** uma jornada ativa ao longo do tempo?
- Quando e por que uma jornada pode ser **pausada**?
- Quando e por que uma jornada pode ser **encerrada**?
- Quando e por que uma jornada pode ser **reaberta** — e isso reativa a mesma jornada ou cria uma nova?

### Conceitos

- Qual a diferença operacional entre **Patient Life Journey** e **Institution Care Journey**?
- Um paciente pode ter **múltiplas** Institution Care Journeys ativas (mesma instituição? instituições diferentes?)?
- Qual a diferença entre **Care Journey**, **Care Episode** e **Attendance**?
- Quando nasce o primeiro Care Episode em relação ao início da jornada?
- Um Attendance pode existir sem Care Episode prévio?

### Modelos operacionais

- Quais gatilhos de início, manutenção, pausa, encerramento e reabertura se aplicam a cada modelo operacional?
- Modelos diagnósticos (laboratório, imagem) têm jornadas mais curtas — como isso se reflete no ciclo de vida?
- Medicina ocupacional inicia jornada por vínculo empregatício, exame admissional ou programa contínuo?
- Hospital (futuro): admissão vs urgência — que reserva conceitual fazer agora?

### Arquitetura

- Estados de ciclo de vida pertencem a qual nível do Modelo Hierárquico (apenas Care Journey ou também Episode)?
- O ADR-0001 precisa de complemento formal (novo ADR) ou apenas documentação oficial?
- Quais Core Business Capabilities são mais impactadas?
- Quais eventos de negócio candidatos existem (sem definir Event Model — Q-003)?
- O que o Clinical Workspace precisa representar por estado da jornada?

### Tensão documental a resolver

O ADR-0001 define Care Journey como *"toda a história de relacionamento clínico do paciente com a instituição"*, enquanto `architecture-foundation.md` descreve Institution Care Journey como um *trecho* da jornada ampla.

A sessão deve **alinhar** esses enunciados — provavelmente: Institution Care Journey = história institucional completa na plataforma; Patient Life Journey = história ampla multi-institucional fora do escopo integral da plataforma.

---

## Hipóteses Iniciais

> **Atenção:** hipóteses de trabalho — não são decisões. Devem ser validadas ou refutadas durante a sessão.

| ID | Hipótese | Fonte |
|---|---|---|
| H-001 | A Patient Life Journey existe fora da plataforma; a Health Platform representa apenas a Institution Care Journey. | `architecture-foundation.md` §7 |
| H-002 | Cadastro em Relacionar estabelece vínculo, mas **não** marca início de responsabilidade assistencial — é necessário um gatilho operacional posterior. | Inferência: §7 + capability Relacionar |
| H-003 | Gatilhos candidatos: agendamento, encaminhamento aceito, admissão, solicitação de exame aceita, programa de acompanhamento, atendimento de urgência. O gatilho efetivo pode variar por modelo operacional. | Q-001, `open-questions.md` |
| H-004 | Uma Institution Care Journey agrupa um ou mais Care Episodes e pode durar anos. | ADR-0001 |
| H-005 | Care Episode = problema, condição ou objetivo assistencial. Attendance = interação concreta paciente–instituição. Um episódio contém um ou mais atendimentos. | ADR-0001, `architecture-foundation.md` §8 |
| H-006 | Em laboratório e centro de imagem, a jornada pode ser mais curta, mas ainda possui início e fim explícitos. | ADR-0001 (modelos diagnósticos) |
| H-007 | **Pausa** = responsabilidade institucional suspensa temporariamente, distinta de **encerramento** (alta, transferência, conclusão). | Hipótese — não formalizada |
| H-008 | **Reabertura** pode ocorrer por novo gatilho sobre jornada encerrada — como reativação ou nova jornada, a definir. | Hipótese — não formalizada |
| H-009 | Telemedicina compartilha gatilhos com ambulatorial, diferindo no tipo de Attendance. | ADR-0001, capability Executar |
| H-010 | Medicina ocupacional inicia por vínculo empresa–paciente + evento admissional ou programa PCMSO. | `architecture-foundation.md` §2 |
| H-011 | Hospital (futuro) usará admissão/urgência como gatilho; detalhes operacionais ficam para sessão futura. | ADR-0001 |
| H-012 | Estados da jornada alimentarão timeline clínica, Clinical Workspace e eventos de negócio candidatos — sem definir Event Model (Q-003 Deferred). | Q-001, ADR-0001 |

---

## Conceitos a Clarificar

### Patient Life Journey

História ampla de saúde da pessoa, potencialmente distribuída por múltiplas instituições e sistemas. A Health Platform **não presume** conhecer essa jornada integralmente.

### Institution Care Journey

Trecho da jornada representado e gerenciado por uma instituição dentro da plataforma. É o objeto central desta sessão.

### Care Journey vs Care Episode vs Attendance

| Nível | Pergunta da sessão |
|---|---|
| **Care Journey** | Qual o ciclo de vida institucional completo? |
| **Care Episode** | Quando nasce em relação à jornada? Pode haver múltiplos episódios simultâneos? |
| **Attendance** | Qual interação concreta pertence a qual episódio? Pode existir sem episódio? |

Referência ADR-0001:

- **Care Journey** — história de cuidado gerenciada pela instituição; pode durar anos.
- **Care Episode** — problema, condição ou objetivo assistencial específico (ex.: gestação, diabetes).
- **Attendance** — interação concreta (consulta, coleta, visita, teleconsulta).

### Estados do ciclo de vida (proposta de investigação)

Estados candidatos — **não decididos**:

| Estado | Descrição candidata |
|---|---|
| **Active** | Instituição mantém responsabilidade assistencial ativa. |
| **Paused** | Responsabilidade suspensa temporariamente. |
| **Closed** | Responsabilidade encerrada (alta, transferência, conclusão). |
| **Reopened** | Jornada previamente encerrada retomada — ou nova jornada criada (a definir). |

### Responsabilidade institucional vs vínculo cadastral

Relacionar estabelece vínculo operacional. A sessão deve definir se isso é distinto de assumir responsabilidade pelo cuidado.

---

## Investigação por Modelo Operacional

Matriz obrigatória da sessão — preencher no workspace `AS-003/`:

| Modelo | Gatilhos candidatos (fontes) | Perguntas específicas |
|---|---|---|
| **Clínica ambulatorial** | Agendamento, encaminhamento, primeiro atendimento | Início no agendamento, check-in ou atendimento? |
| **Telemedicina** | Solicitação, agendamento, início da sessão | Início na solicitação, agendamento ou teleconsulta? |
| **Laboratório** | Solicitação de exame, recebimento de pedido, coleta | Início no pedido, na coleta ou na admissão da amostra? |
| **Centro de imagem** | Solicitação, agendamento, realização | Início no agendamento ou na realização do exame? |
| **Home care** | Admissão no programa, primeira visita, prescrição domiciliar | Início na admissão ou na primeira visita? |
| **Hospital (futuro)** | Admissão, urgência, internação | Reservar conceito; não decidir operação nesta sessão |
| **Medicina ocupacional** | Vínculo empresarial, exame admissional, programa PCMSO | Início por vínculo, exame ou programa contínuo? |

Para cada modelo, investigar: **início · manutenção · pausa · encerramento · reabertura**.

---

## Alternativas para Q-001 (início da jornada)

### Alternativa A — Primeiro vínculo operacional

A jornada inicia no momento em que o paciente é vinculado à instituição (cadastro + vínculo em Relacionar).

**Vantagens:** simples; jornada existe cedo.

**Desvantagens:** confunde cadastro com responsabilidade assistencial; jornadas "vazias" sem cuidado real.

---

### Alternativa B — Primeiro evento de Organizar

A jornada inicia no primeiro evento de planejamento: agendamento, fila, encaminhamento aceito.

**Vantagens:** separa vínculo de intenção operacional; alinhado com capability Organizar.

**Desvantagens:** laboratório/imagem podem receber pedido sem agendamento prévio; urgência pode pular Organizar.

---

### Alternativa C — Primeiro evento de Executar

A jornada inicia apenas quando o cuidado é efetivamente realizado (primeiro atendimento, coleta, visita).

**Vantagens:** responsabilidade assistencial concreta.

**Desvantagens:** perde rastreabilidade de preparação e comunicação pré-atendimento; telemedicina e agendamento ficam sem âncora.

---

### Alternativa D — Aceite explícito de responsabilidade

A jornada inicia em evento formal: admissão, aceite de encaminhamento, entrada em programa, pedido aceito.

**Vantagens:** clareza semântica; auditável.

**Desvantagens:** exige definir o que é "aceite" em cada modelo; pode ser tardio em alguns fluxos.

---

### Alternativa E — Gatilho configurável por instituição

O Core define família de gatilhos válidos; cada instituição configura qual gatilho inicia a jornada para seu modelo operacional (ADR-0006).

**Vantagens:** flexível; suporta múltiplos modelos sem alterar o Core.

**Desvantagens:** maior complexidade de configuração; risco de inconsistência entre tenants.

---

## Trade-offs

- **Simplicidade vs precisão semântica:** Alternativa A é simples, mas arrisca confundir cadastro com cuidado.
- **Antecipação vs concretude:** Organizar cedo (B) permite timeline completa; Executar tarde (C) garante cuidado real mas perde preparação.
- **Uniformidade vs flexibilidade:** um gatilho único simplifica o Core; gatilhos por modelo operacional refletem a realidade mas exigem configuração (E).
- **Jornada longa vs curta:** ambulatorial e ocupacional tendem a jornadas longas; diagnóstico tende a jornadas curtas — o modelo deve suportar ambos sem exceções no Core.

---

## Riscos

### Risco 1 — Confundir cadastro com início de jornada

Pacientes cadastrados sem nenhum cuidado ativo gerariam jornadas fictícias.

**Mitigação:** distinguir explicitamente vínculo (Relacionar) de responsabilidade assistencial (início da jornada).

---

### Risco 2 — Múltiplas jornadas ativas simultâneas

Um paciente na mesma instituição pode ter mais de uma jornada ativa (ex.: programa ocupacional + consulta eletiva).

**Mitigação:** definir regra de cardinalidade (uma vs múltiplas jornadas) na sessão.

---

### Risco 3 — Pausa vs inatividade vs perda de responsabilidade

Estados mal definidos geram ambiguidade em monitoramento e comunicação.

**Mitigação:** vocabulário explícito de estados e transições; registrar no documento oficial.

---

### Risco 4 — Modelos diagnósticos com jornada mínima

Laboratório e imagem podem ter ciclos muito curtos — risco de over-engineering.

**Mitigação:** validar que jornada curta com episódio único é caso válido, não exceção.

---

### Risco 5 — Tensão ADR-0001 vs architecture-foundation

Enunciados distintos sobre escopo da Care Journey podem gerar inconsistência documental.

**Mitigação:** alinhar terminologia nesta sessão; complementar ADR-0001 ou documentação oficial se necessário.

---

## Critérios de Decisão

| Critério | Peso |
|---|---|
| Funciona em todos os modelos operacionais investigados | Alto |
| Não confunde vínculo cadastral com responsabilidade assistencial | Alto |
| Suporta timeline clínica e Clinical Workspace | Alto |
| Auditável e rastreável | Alto |
| Configurável sem quebrar o Core (ADR-0006) | Médio |
| Não exige customização por cliente | Médio |
| Compatível com ADR-0001 sem reabri-lo | Alto |

---

## Impactos Arquiteturais

### Modelo Hierárquico do Cuidado (ADR-0001)

- Estados de ciclo de vida provavelmente pertencem ao nível **Care Journey**; episódios podem ter sub-estados próprios (a investigar).
- Invariantes a preservar: Attendance pertence a um Episode; Episode pertence a uma Journey.
- Pode exigir ADR complementar ou seção em documentação oficial — não reabrir ADR-0001 sem justificativa.

### Core Business Capabilities

| Capability | Impacto esperado |
|---|---|
| **Relacionar** | Vínculo paciente–instituição como pré-requisito, não necessariamente início da jornada |
| **Organizar** | Gatilhos de início candidatos (agendamento, encaminhamento, fila) |
| **Executar** | Attendance como interação concreta dentro da jornada ativa |
| **Registrar** | Timeline clínica ancorada na jornada e seus estados |
| **Monitorar** | Pendências e SLAs dependem de jornada ativa vs pausada vs encerrada |
| **Comunicar** | Comunicações pré e pós-atendimento vinculadas ao estado da jornada |
| **Governar** | Auditoria de transições de estado |

### Business Domains (AS-004 — futuro)

A resposta a Q-001 influenciará fronteiras do domínio clínico e possível domínio de "Care Management" ou equivalente. **Não definir domínios nesta sessão.**

### Eventos de negócio candidatos (conceitual — Q-003 Deferred)

Lista de investigação, **sem** definir Event Model:

- `InstitutionCareJourneyStarted`
- `InstitutionCareJourneyActivated` / `Maintained`
- `InstitutionCareJourneyPaused`
- `InstitutionCareJourneyClosed`
- `InstitutionCareJourneyReopened`
- `CareEpisodeStarted` / `CareEpisodeClosed`
- `AttendanceStarted` / `AttendanceCompleted`

### Clinical Workspace e timeline clínica

- Workspace deve refletir jornada ativa e episódios em andamento.
- Timeline deve ordenar eventos e artefatos no contexto da Institution Care Journey.
- Estados pausado/encerrado devem impactar o que é exibido e editável (a definir na sessão).

### Documentação derivada

| Documento | Ação esperada ao encerrar |
|---|---|
| `docs/02-business-processes/care-journey-lifecycle.md` | Criar — Draft |
| `docs/02-business-processes/healthcare-operating-model.md` | Completar seção de jornada |
| `ai-context/open-questions.md` | Q-001 → Answered (com referência) |
| `ARCHITECTURE_INDEX.md` | Atualizar status AS-003 |

---

## Entregáveis Esperados ao Encerrar

- [ ] Q-001 respondida (ou parcialmente, com open questions remanescentes documentadas)
- [ ] `workspace/AS-003/` populado (questions, hypotheses, decisions)
- [ ] `docs/02-business-processes/care-journey-lifecycle.md` — Draft
- [ ] ADR complementar ao ciclo de vida (se a sessão produzir decisão formal)
- [ ] `healthcare-operating-model.md` — seção de jornada completada
- [ ] `ai-context/open-questions.md` — status Q-001 atualizado
- [ ] `ARCHITECTURE_INDEX.md` — AS-003 encerrada

---

## Próximas Ações

### Imediatas (investigação)

1. Criar workspace `workspace/AS-003/` com README, questions e hypotheses.
2. Preencher matriz de gatilhos por modelo operacional.
3. Avaliar alternativas A–E para Q-001 com cenários concretos.
4. Resolver tensão Patient Life Journey vs Institution Care Journey.
5. Definir vocabulário de estados e transições.

### Ao encerrar a sessão

1. Produzir `care-journey-lifecycle.md` (documentação oficial).
2. Formalizar decisões em ADR se necessário.
3. Atualizar `open-questions.md` e `ARCHITECTURE_INDEX.md`.
4. Indicar AS-004 — Business Domain Map como próxima sessão.

---

## Próxima Architecture Session

**AS-004 — Business Domain Map**

Resolver Q-002 (agrupamento de sub-capabilities em Business Domains). **Não iniciar antes de Q-001 estar suficientemente respondida.**

---

## Questões em Aberto

### Objeto desta sessão

| ID | Pergunta | Status |
|---|---|---|
| Q-001 | Quando a instituição assume responsabilidade pelo cuidado? | **Answered** — ADR-0007, `care-journey-lifecycle.md` |

### Encaminhadas para sessões futuras

| ID | Pergunta | Destino |
|---|---|---|
| Q-002 | Como agrupar sub-capabilities em Business Domains? | AS-004 |
| Q-004 | Agregados do domínio clínico | AS-004 (parcial) |
| Q-003 | Event Model | AS-010 — Deferred |

### Novas perguntas

Perguntas emergentes durante a investigação devem ser registradas em `workspace/AS-003/questions.md` e, se relevantes, adicionadas a `ai-context/open-questions.md` com novo ID (próximo: Q-018).

---

## Checklist de Encerramento

- [x] A hipótese inicial foi confirmada — gatilho operacional + configuração por modelo (D + E).
- [x] As premissas foram validadas.
- [x] Os riscos foram tratados ou mitigados.
- [x] Q-001 foi respondida na documentação oficial.
- [x] Decisões registradas em `workspace/AS-003/decisions.md`.
- [x] ADR criado — ADR-0007 (Accepted).
- [x] `care-journey-lifecycle.md` criado.
- [x] `healthcare-operating-model.md` atualizado (§6).
- [x] `open-questions.md` atualizado.
- [x] Próxima sessão indicada — AS-004.
- [x] `ARCHITECTURE_INDEX.md` atualizado.

---

## Resultado da Sessão

A sessão foi **concluída com sucesso**.

A Institution Care Journey inicia com um **Care Journey Start Event** (aceite explícito de responsabilidade). O Core define tipos válidos; a instituição configura o gatilho por modelo operacional (ADR-0006). Estados: **Active**, **Paused**, **Closed**. Reabertura cria nova jornada.

Decisão formalizada no **ADR-0007**. Documentação oficial em `care-journey-lifecycle.md`.

**Q-018** registrada: múltiplas jornadas ativas simultâneas por paciente/instituição.

---
title: Healthcare Operating Model
status: Draft
version: 0.2.1
created: 2026-07-02
updated: 2026-07-02
author: Architecture Team
category: Business
phase: Product & Architecture Foundation
related:
  - docs/02-business-processes/care-journey-lifecycle.md
  - docs/03-capabilities/core-business-capabilities.md
  - docs/03-capabilities/business-capability-map.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md
  - ai-context/architecture-foundation.md
---

# Healthcare Operating Model

> Este documento descreve como a Health Platform modela uma instituição de saúde — sua operação, jornada de cuidado e derivação em capacidades de negócio.

---

## 1. Purpose

Este documento estabelece o **modelo operacional** que serve de base para toda a arquitetura da Health Platform.

A plataforma não é construída em torno de funcionalidades ou telas isoladas. Ela representa digitalmente como instituições de saúde operam — de forma configurável, modular e escalável — sem impor um fluxo operacional único às organizações.

Qualquer instituição de saúde deve conseguir mapear seus processos para dentro da Health Platform.

---

## 2. Conceptual Position

A arquitetura da plataforma segue uma cadeia de derivação obrigatória:

```text
Healthcare Operating Model
        ↓
Institution Care Journey
        ↓
Core Business Capabilities
        ↓
Business Capability Map
        ↓
Business Domains
        ↓
Modules
        ↓
Features
```

O **Healthcare Operating Model** descreve como uma instituição de saúde opera. A **Institution Care Journey** representa o trecho do cuidado em que a instituição assume responsabilidade assistencial dentro da plataforma. As **Core Business Capabilities** expressam o que qualquer instituição precisa ser capaz de fazer para operar nesse contexto.

Business Domains, Modules e Features são derivados — não são definidos neste documento.

---

## 3. What is a Healthcare Institution?

Para a Health Platform, uma **instituição de saúde** é uma organização responsável por prestar serviços relacionados à promoção, prevenção, diagnóstico, tratamento e acompanhamento da saúde.

Seu propósito é gerenciar e executar o ciclo de cuidado do paciente. Esse ciclo não começa necessariamente na consulta nem termina no atendimento isolado — pode envolver descoberta da necessidade, agendamento, preparação, atendimento, diagnóstico, exames, resultados, tratamento, acompanhamento, reavaliação, alta e monitoramento.

---

## 4. Fundamental Dimensions

Uma instituição de saúde é compreendida por quatro dimensões complementares:

| Dimensão | O que representa | Exemplos |
|---|---|---|
| **Pessoas** | Quem participa da operação e do cuidado | Paciente, profissional de saúde, recepcionista, gestor, convênio |
| **Recursos** | O que a instituição utiliza para operar | Salas, equipamentos, profissionais alocados, unidades |
| **Processos** | Como a instituição transforma necessidade em cuidado | Agendamento, check-in, atendimento, solicitação de exames, comunicação, retorno |
| **Governança** | Como a operação é controlada e auditada | Permissões, conformidade, rastreabilidade, configurações |

Os processos de uma instituição originam as capacidades que a plataforma precisa suportar — mas processo e capability não são a mesma coisa. Processos descrevem *como a instituição opera*; capabilities descrevem *o que a plataforma deve ser capaz de fazer*.

---

## 5. Operational Models

A plataforma não cresce adicionando "tipos de clientes". Ela cresce adicionando suporte a **modelos operacionais de saúde** — formas pelas quais o cuidado é organizado e executado.

| Modelo operacional | Escopo |
|---|---|
| **Ambulatory Care** | Consultórios, clínicas, policlínicas, centros médicos e atendimentos presenciais |
| **Diagnostic Care** | Laboratórios, centros de imagem, anatomia patológica e produção de resultados diagnósticos |
| **Therapeutic Care** | Psicologia, nutrição, fisioterapia, fonoaudiologia, terapia ocupacional e cuidado recorrente |
| **Home Care** | Atenção domiciliar, visitas, acompanhamento remoto e cuidado distribuído |
| **Occupational Health** | Saúde ocupacional, programas corporativos e acompanhamento de trabalhadores |
| **Telemedicine** | Atendimentos e acompanhamentos realizados à distância |
| **Hospital Care** *(futuro)* | Modelo de alta complexidade — evolução futura da plataforma |

Esses modelos não são mutuamente exclusivos. Uma mesma instituição pode operar com mais de um modelo simultaneamente. A arquitetura deve permitir que compartilhem uma base comum e possuam extensões específicas quando necessário (ADR-0003).

### Instituição vs modelo operacional

| Conceito | O que é | Exemplo |
|---|---|---|
| **Tipo de instituição** | A organização e seu perfil | Uma clínica, um laboratório, uma empresa de home care |
| **Modelo operacional** | A forma como o cuidado é executado | Ambulatory Care, Diagnostic Care, Telemedicine |

Uma clínica (tipo de instituição) opera predominantemente em **Ambulatory Care**, mas pode incorporar **Telemedicine**. Um laboratório (tipo de instituição) opera em **Diagnostic Care**. Confundir organização com modelo operacional limita a evolução da plataforma.

---

## 6. Institution Care Journey

A Health Platform é orientada pela **Jornada de Cuidado**. Dois conceitos devem ser distinguidos:

| Conceito | Significado |
|---|---|
| **Patient Life Journey** | História ampla de saúde da pessoa, potencialmente distribuída por todo o ecossistema de saúde — fora do escopo integral da plataforma. |
| **Institution Care Journey** | Trecho da jornada representado e gerenciado por uma instituição dentro da Health Platform. |

A necessidade de cuidado pode existir antes da plataforma (sintoma, condição crônica, encaminhamento, exame preventivo, programa de cuidado). A plataforma passa a representar a jornada **quando uma instituição assume responsabilidade por algum trecho daquele cuidado**.

A Health Platform não presume conhecer toda a vida clínica do paciente. Ela representa a participação institucional no cuidado.

### Conexão com o Care Journey Lifecycle

A jornada **inicia** com um **Care Journey Start Event** — aceite explícito de responsabilidade por uma demanda de cuidado. Cadastro em Relacionar é pré-requisito, mas não inicia a jornada.

Estados: **Active**, **Paused**, **Closed**. Reabertura após encerramento cria uma nova Institution Care Journey.

Detalhes completos — gatilhos por modelo operacional, transições e cardinalidade: [care-journey-lifecycle.md](./care-journey-lifecycle.md) e [ADR-0007](../05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md).

---

## 7. Hierarchical Care Model

Dentro de uma Institution Care Journey, o cuidado é representado pelo **Modelo Hierárquico do Cuidado** (ADR-0001):

```text
Patient
    ↓
Care Journey
    ↓
Care Episode
    ↓
Attendance
    ↓
Clinical Event
    ↓
Clinical Artifact
```

| Nível | Responsabilidade resumida |
|---|---|
| **Patient** | Pessoa que recebe cuidado |
| **Care Journey** | História de cuidado gerenciada pela instituição |
| **Care Episode** | Problema, condição ou objetivo assistencial específico |
| **Attendance** | Interação concreta entre paciente e instituição |
| **Clinical Event** | Ocorrência relevante durante a jornada ou atendimento |
| **Clinical Artifact** | Documento ou registro produzido (prescrição, laudo, evolução) |

Este modelo é único para diferentes modelos operacionais e orienta futuras decisões sobre timeline clínica, Clinical Workspace e registro do cuidado. Detalhes completos: [ADR-0001](../05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md).

---

## 8. From Operating Model to Capabilities

Toda instituição de saúde — independentemente do modelo operacional — precisa ser capaz de realizar oito competências permanentes de negócio. Na Health Platform, essas competências são as **Core Business Capabilities**:

```text
Relacionar → Organizar → Executar → Registrar → Comunicar → Integrar → Monitorar → Governar
```

| Capability | O que representa |
|---|---|
| **Relacionar** | Estabelecer vínculos operacionais e assistenciais entre participantes |
| **Organizar** | Planejar e coordenar como o cuidado será realizado |
| **Executar** | Realizar efetivamente o cuidado ou serviço de saúde |
| **Registrar** | Produzir e manter o conhecimento clínico e operacional gerado |
| **Comunicar** | Levar informações relevantes às pessoas envolvidas na jornada |
| **Integrar** | Trocar informações com sistemas externos |
| **Monitorar** | Acompanhar pendências, indicadores e alertas do cuidado |
| **Governar** | Garantir segurança, rastreabilidade, conformidade e controle |

Core Business Capabilities **não são** módulos, telas, microsserviços ou Business Domains. São o vocabulário de negócio estável a partir do qual a plataforma evolui (ADR-0002).

Detalhes: [core-business-capabilities.md](../03-capabilities/core-business-capabilities.md).

---

## 9. Relationship with Business Capability Map

O **Business Capability Map** nasce da decomposição das oito Core Business Capabilities em sub-capabilities de segundo nível — competências operacionais concretas que a plataforma deve suportar.

```text
Healthcare Operating Model
        ↓
Core Business Capabilities     (8 — permanentes)
        ↓
Business Capability Map        (39 sub-capabilities — estáveis, versionáveis)
        ↓
Business Domains               (16 domínios — ADR-0008, AS-004 ✅)
        ↓
Modules                        (pendente — AS-005)
```

O mapa detalha *o que* a plataforma precisa ser capaz de realizar, sem definir módulos ou implementação. Os **Business Domains** agrupam sub-capabilities em fronteiras de responsabilidade — detalhes em [business-domains.md](../04-domain/business-domains.md) e [domain-map.md](../04-domain/domain-map.md).

---

## 10. What This Document Does Not Define

Este documento **não define**:

- Business Domains nem Bounded Contexts
- Detalhes do ciclo de vida (ver `care-journey-lifecycle.md`)
- Modelagem de banco de dados, APIs ou schemas
- Interfaces de usuário (UI) ou Clinical Workspace
- Event Model ou tecnologia de mensageria
- Platform Services, módulos ou implementação técnica
- Escopo de MVP por sub-capability

Esses temas possuem documentos, sessões ou ADRs próprios na sequência da fundação arquitetural.

---

## 11. Related Documents

| Documento | Relação |
|---|---|
| [care-journey-lifecycle.md](./care-journey-lifecycle.md) | Ciclo de vida da Institution Care Journey |
| [ADR-0007 — Care Journey Lifecycle](../05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md) | Decisão formal do ciclo de vida |
| [core-business-capabilities.md](../03-capabilities/core-business-capabilities.md) | As oito Core Business Capabilities |
| [business-capability-map.md](../03-capabilities/business-capability-map.md) | Decomposição em 39 sub-capabilities |
| [business-domains.md](../04-domain/business-domains.md) | 16 Business Domains (ADR-0008) |
| [domain-map.md](../04-domain/domain-map.md) | Mapeamento sub-capability → domínio |
| [ADR-0008 — Business Domain Map](../05-architecture/adr/foundation/ADR-0008-business-domain-map.md) | Decisão formal do mapa de domínios |
| [ADR-0001 — Modelo Hierárquico do Cuidado](../05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md) | Hierarquia Patient → Clinical Artifact |
| [ADR-0002 — Capability-Driven Architecture](../05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md) | Organização por capabilities e sequência de derivação |
| [architecture-foundation.md](../../ai-context/architecture-foundation.md) | Contexto consolidado para IA e arquitetos |
| [open-questions.md](../../ai-context/open-questions.md) | Q-018 e demais perguntas em aberto |

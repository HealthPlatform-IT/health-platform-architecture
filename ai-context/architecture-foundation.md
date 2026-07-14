---
title: Architecture Foundation
status: Draft
version: 0.3.0
created: 2026-07-02
updated: 2026-07-03
author: Architecture Team
category: AI Context
phase: Product & Architecture Foundation
related:
  - docs/00-introduction/vision.md
  - docs/00-introduction/principles.md
  - docs/01-product/product-overview.md
  - docs/02-business-processes/healthcare-operating-model.md
  - docs/03-capabilities/business-capability-map.md
  - docs/03-capabilities/core-business-capabilities.md
  - docs/04-domain/business-domains.md
  - docs/04-domain/domain-map.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
---

# Architecture Foundation

> Este documento consolida os fundamentos conceituais e arquiteturais da Health Platform para orientar ferramentas de IA, arquitetos e desenvolvedores durante as próximas fases do projeto.

Este arquivo deve ser usado como uma fonte de contexto de alto nível para qualquer agente de IA, como Cursor, Claude, ChatGPT ou outras ferramentas de apoio ao desenvolvimento.

Ele não substitui os documentos oficiais da arquitetura. Seu objetivo é oferecer uma visão consolidada, objetiva e acionável sobre as principais decisões, princípios, conceitos e restrições já discutidos para a Health Platform.

---

# 1. Propósito deste Documento

A Health Platform é uma plataforma SaaS modular para saúde. Seu objetivo não é apenas informatizar clínicas, agendas ou prontuários, mas representar digitalmente a operação de instituições de saúde a partir de seus modelos operacionais, jornadas de cuidado, capacidades de negócio e domínios.

Este documento existe para evitar perda de contexto durante o desenvolvimento.

Qualquer IA ou desenvolvedor que atue no projeto deve entender que a Health Platform não deve ser construída a partir de telas, módulos isolados ou requisitos pontuais. A plataforma deve nascer de uma arquitetura orientada por negócio.

A sequência conceitual da plataforma é:

```text
Healthcare Operating Model
↓
Core Business Capabilities
↓
Business Domains
↓
Platform Services
↓
Modules
↓
Features
↓
Components
↓
Screens
```

---

# 2. Visão da Plataforma

A Health Platform é uma plataforma operacional de saúde, modular, multi-tenant, extensível e configurável, projetada para atender diferentes modelos de instituições de saúde.

A plataforma deve permitir que diferentes instituições configurem seus processos sobre uma arquitetura comum, sem exigir customizações específicas por cliente sempre que houver alternativa configurável.

A visão central é:

> Qualquer instituição de saúde deve conseguir mapear seus processos para dentro da Health Platform.

A plataforma deve ser capaz de atender, ao longo do tempo, modelos como:

- Clínicas ambulatoriais.
- Consultórios.
- Policlínicas.
- Centros médicos.
- Telemedicina.
- Laboratórios.
- Centros de imagem.
- Serviços terapêuticos.
- Home care.
- Medicina ocupacional.
- Vacinação.
- Hospitais em fases futuras.

A plataforma deve crescer por extensão, não por alteração constante do Core.

---

# 3. O que a Health Platform Não É

A Health Platform não deve ser tratada como:

- Um sistema específico para uma única clínica.
- Um prontuário eletrônico isolado.
- Um sistema de agenda com módulos adicionais.
- Um ERP genérico adaptado para saúde.
- Um conjunto de telas desconectadas.
- Uma aplicação construída a partir de features avulsas.
- Um produto dependente de customização por cliente.
- Um monólito rígido centrado em atendimento, consulta ou prontuário.

O prontuário é uma visão da jornada de cuidado. Ele não é o centro conceitual da plataforma.

A agenda é um mecanismo operacional. Ela também não é o centro da plataforma.

A consulta é um tipo de interação de cuidado. Ela não representa toda a jornada.

---

# 4. Princípio Central

A arquitetura deve representar a realidade da saúde, e não a forma limitada como muitos sistemas atuais foram construídos.

A plataforma deve ser orientada pelo cuidado, pela operação institucional e pelas responsabilidades assistenciais.

O código deve ser consequência do modelo de negócio compreendido, e não o ponto de partida da arquitetura.

---

# 5. Modelo Operacional da Saúde

Uma instituição de saúde é uma organização responsável por prestar serviços relacionados à promoção, prevenção, diagnóstico, tratamento e acompanhamento da saúde.

Para a Health Platform, uma instituição de saúde deve ser compreendida por quatro dimensões:

```text
Pessoas
Recursos
Processos
Governança
```

O propósito de uma instituição de saúde é gerenciar e executar o ciclo completo de cuidado do paciente.

Esse ciclo não começa necessariamente na consulta e não termina necessariamente no atendimento. O cuidado pode envolver descoberta da necessidade, agendamento, preparação, atendimento, diagnóstico, exames, resultados, tratamento, acompanhamento, reavaliação, alta e monitoramento.

---

# 6. Modelos Operacionais Atendidos

A plataforma não deve crescer simplesmente adicionando “tipos de clientes”. Ela deve crescer adicionando suporte a modelos operacionais de saúde.

Os principais modelos operacionais identificados são:

## Assistência Ambulatorial

Inclui consultórios, clínicas, policlínicas, centros médicos e atendimentos presenciais ou digitais.

## Diagnóstico

Inclui laboratórios, centros de imagem, anatomia patológica e serviços responsáveis pela produção de resultados diagnósticos.

## Terapêutico

Inclui psicologia, nutrição, fisioterapia, fonoaudiologia, terapia ocupacional e outras linhas de cuidado recorrentes.

## Atenção Domiciliar

Inclui home care, visitas domiciliares, acompanhamento remoto e cuidado distribuído.

## Hospitalar

Modelo de alta complexidade, considerado como evolução futura da plataforma.

A arquitetura deve permitir que esses modelos compartilhem uma base comum, mas possuam extensões específicas quando necessário.

---

# 7. Jornada de Cuidado

A Health Platform deve ser orientada pela Jornada de Cuidado do Paciente.

A jornada de cuidado representa a sequência de necessidades, interações, eventos, decisões, registros, comunicações e acompanhamentos relacionados ao cuidado de uma pessoa.

Uma conclusão importante é que a jornada de cuidado não nasce necessariamente dentro da plataforma. Ela nasce quando existe uma necessidade de cuidado.

Exemplos de gatilhos conceituais:

- Sintoma.
- Condição crônica.
- Encaminhamento.
- Exame preventivo.
- Acompanhamento periódico.
- Tratamento.
- Acidente.
- Programa de cuidado.
- Solicitação diagnóstica.

A plataforma passa a representar a jornada quando uma instituição assume responsabilidade por algum trecho daquele cuidado.

Por isso, deve-se diferenciar:

| Conceito | Significado |
|---|---|
| Patient Life Journey | História ampla de saúde da pessoa, conceitual e potencialmente distribuída por todo o ecossistema de saúde. |
| Institution Care Journey | Trecho da jornada representado e gerenciado por uma instituição dentro da Health Platform. |

A Health Platform não deve presumir que conhece toda a vida clínica do paciente. Ela representa a participação institucional no cuidado.

---

# 8. Modelo Hierárquico do Cuidado

A plataforma deve adotar um modelo hierárquico para representar o cuidado.

A estrutura conceitual é:

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

Em português:

```text
Paciente
↓
Jornada de Cuidado
↓
Episódio de Cuidado
↓
Atendimento
↓
Evento Clínico
↓
Artefato Clínico
```

## Patient

Representa a pessoa que recebe cuidado.

O paciente é o sujeito do cuidado, mas não deve ser confundido com a jornada em si.

## Care Journey

Representa a história de cuidado gerenciada pela instituição dentro de determinado contexto assistencial.

Ela conecta episódios, atendimentos, eventos e artefatos ao longo do tempo.

## Care Episode

Representa um problema, condição, objetivo ou contexto específico de cuidado.

Exemplos:

- Acompanhamento de diabetes.
- Gestação.
- Investigação cardiológica.
- Tratamento fisioterapêutico.
- Episódio de urgência.
- Acompanhamento pós-operatório.

## Attendance

Representa uma interação concreta entre paciente e instituição.

Exemplos:

- Consulta presencial.
- Teleconsulta.
- Sessão terapêutica.
- Coleta.
- Exame.
- Procedimento.
- Visita domiciliar.
- Internação futura.

## Clinical Event

Representa algo relevante que aconteceu durante a jornada ou atendimento.

Exemplos:

- Diagnóstico registrado.
- Prescrição emitida.
- Exame solicitado.
- Resultado recebido.
- Evolução clínica registrada.
- Encaminhamento criado.
- Alerta disparado.

## Clinical Artifact

Representa documentos, registros ou artefatos produzidos durante o cuidado.

Exemplos:

- Prescrição.
- Atestado.
- Solicitação de exame.
- Laudo.
- Evolução clínica.
- Relatório.
- Consentimento.
- Encaminhamento.

Esse modelo deve orientar futuras decisões sobre banco de dados, APIs, eventos, timeline clínica, prontuário, Clinical Workspace, telemedicina, documentos médicos, integrações e inteligência artificial.

---

# 9. Core Business Capabilities

A Health Platform será organizada em torno de oito Core Business Capabilities.

Essas capacidades representam o menor conjunto de competências necessárias para que qualquer instituição de saúde opere dentro do escopo da plataforma.

Elas não são módulos, telas, microsserviços ou entidades técnicas. Elas são capacidades permanentes de negócio.

```text
Relacionar
↓
Organizar
↓
Executar
↓
Registrar
↓
Comunicar
↓
Integrar
↓
Monitorar
↓
Governar
```

## 9.1 Relacionar

Estabelecer e manter vínculos operacionais e assistenciais entre todos os participantes do ecossistema de saúde.

Inclui pacientes, profissionais, responsáveis, cuidadores, organizações, convênios, empresas, parceiros, laboratórios e prestadores.

O objetivo não é apenas cadastrar pessoas. O objetivo é estabelecer vínculo operacional e assistencial.

## 9.2 Organizar

Planejar e coordenar como o cuidado será realizado.

Inclui agenda, disponibilidade, filas, recursos, salas, profissionais, prioridades, encaminhamentos e workflows operacionais.

Organizar transforma uma necessidade de cuidado em uma operação planejada.

## 9.3 Executar

Realizar efetivamente o cuidado ou serviço de saúde.

Inclui consultas, teleconsultas, sessões, procedimentos, coletas, exames, visitas domiciliares e, futuramente, internações ou atendimentos complexos.

Executar é a realização concreta do cuidado.

## 9.4 Registrar

Produzir e manter o conhecimento clínico e operacional gerado durante o cuidado.

Inclui evolução clínica, diagnósticos, prescrições, exames, laudos, documentos médicos, atestados, encaminhamentos, consentimentos e linha do tempo clínica.

Registrar transforma a execução do cuidado em conhecimento persistente.

## 9.5 Comunicar

Levar informações relevantes às pessoas envolvidas na jornada de cuidado.

Inclui e-mail, WhatsApp, SMS, push notification, notificações internas, portal do paciente, portal do profissional, templates de mensagens e histórico de envio.

Comunicar é voltado para pessoas.

Nenhum módulo deve enviar mensagens diretamente sem passar por serviços de comunicação.

## 9.6 Integrar

Permitir troca segura e padronizada de informações entre a Health Platform e sistemas externos.

Inclui FHIR, TISS, DICOM, PACS, LIS, ERPs, gateways de pagamento, webhooks, APIs externas, dispositivos médicos, IoT e wearables.

Integrar é voltado para sistemas.

Comunicação e integração são capabilities distintas porque possuem responsabilidades, contratos e riscos diferentes.

## 9.7 Monitorar

Acompanhar continuamente processos, eventos, pendências e indicadores relacionados ao cuidado.

Inclui exames pendentes, retornos, atrasos, status de laudos, riscos clínicos, SLAs, protocolos, alertas e dashboards operacionais.

Monitorar permite que a plataforma deixe de ser apenas reativa e passe a ser proativa.

Essa capability será base importante para automação e inteligência artificial.

## 9.8 Governar

Garantir segurança, rastreabilidade, conformidade e controle sobre toda a operação da plataforma.

Inclui auditoria, permissões, autorização, LGPD, compliance, versionamento, histórico de acesso, histórico de alterações, configurações, feature flags, logs e observabilidade.

Governar é transversal a todas as demais capabilities.

---

# 10. De Onde Nasce o Business Capability Map

O Business Capability Map não nasce de módulos, telas, banco de dados ou features.

Ele nasce da decomposição das capacidades fundamentais necessárias para operar uma instituição de saúde.

A relação correta é:

```text
Healthcare Operating Model
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

Esse é um dos fundamentos centrais da arquitetura.

Ao criar qualquer novo módulo ou funcionalidade, primeiro deve-se identificar a qual capability ela pertence.

Se uma funcionalidade parecer pertencer a várias capabilities, ela provavelmente envolve orquestração entre domínios ou deve ser decomposta.

---

# 10.1 Business Domains (AS-004 — ADR-0008)

As **39 sub-capabilities** do Business Capability Map foram agrupadas em **16 Business Domains** (Q-002 Answered):

| Tipo | Domínios |
|---|---|
| **Core (5)** | Care Delivery, Clinical Record, Clinical Orders, Clinical Documents, Care Monitoring |
| **Supporting (8)** | Patient Relationship, Professional Management, Organization Management, Payer & Insurance, Care Coordination, Communication, Integration, Operations Monitoring |
| **Extension (2)** | Diagnostic Operations, Home Care Operations |
| **Cross-cutting (1)** | Governance & Compliance |

**Read models:** Clinical Timeline, Analytics & Reporting.

**Deferred:** Hospital Operations, Billing & Financial (Q-005).

**Critérios de decomposição (D-002):**

| Pergunta | Destino |
|---|---|
| Regras de negócio e vocabulário ubíquo próprios? | Business Domain |
| Responsabilidade transversal, reutilizável e técnica? | Platform Service |
| Agregação de leitura cross-domain? | Read Model / View |

Documentação oficial: `docs/04-domain/business-domains.md`, `docs/04-domain/domain-map.md`, ADR-0008.

**Próximo passo:** ~~Module Strategy (AS-005)~~ ✅ — ver `docs/05-architecture/module-strategy.md`.

---

# 10.2 Core Platform e Module Strategy (AS-005 — Q-007 Answered)

## Core Platform (8 componentes)

Fundação estrutural mínima — **contratos e invariantes**, não implementações:

1. Hierarchical Care Model Contracts
2. Care Journey Type System
3. Tenant Context (contrato)
4. Module Registry
5. Extension Mechanism
6. Event Foundation (contrato limitado)
7. Runtime Context
8. Shared Kernel Types

**Reconciliação ADR-0003 ↔ ADR-0005:** Core Platform = contratos; Identity, Audit, Configuration etc. = Platform Services.

## Seis categorias arquiteturais (classification matrix)

| Categoria | Pergunta resumida |
|---|---|
| Core Platform | Fundação estrutural SaaS multi-tenant? |
| Platform Service | Transversal, reutilizável, sem vocabulário de domínio? |
| Business Domain | Regras de negócio e vocabulário ubíquo? |
| Read Model / View | Projeção cross-domain sem regras autônomas? |
| Module | Implementação funcional derivada de domínio(s)? |
| Extension | Módulo opcional por modelo operacional? |

## Módulos candidatos (15)

| Tier | Quantidade | Exemplos |
|---|---|---|
| Core product | 8 | Scheduling, Clinical Workspace (shell), Attendance, Portal |
| Supporting | 5 | Professional Admin, Payer, Integration Admin, Operations Dashboard, Governance |
| Extension | 2 | Diagnostic, Home Care |

**Clinical Workspace (M-02):** shell transversal que compõe módulos clínicos — sem regras de negócio próprias.

**Telemedicine:** Operational Mode em Attendance — não Extension Module.

Documentação: `docs/05-architecture/core-platform.md`, `docs/05-architecture/module-strategy.md`.

---

# 11. Platform Services

A Health Platform deve possuir Platform Services reutilizáveis para responsabilidades transversais ou compartilhadas entre múltiplos domínios.

Um Platform Service não pertence a um módulo específico. Ele oferece uma capacidade reutilizável por meio de contratos claros.

## Tiers AS-005 (ADR-0009 D-003)

| Tier | Qtd | Serviços |
|---|---|---|
| **Confirmed** | 15 | Identity … Observability, **Medical Form Engine**, **Document Engine**, **Event Bus** |
| **Strong Candidate** | 2 | Search, Template Service |
| **Needs Review** | 1 | Compliance Service (Q-019) |

> **Nota AS-006/007/010:** Medical Form Engine, Document Engine e Event Bus promovidos a **Confirmed** (ADR-0010, ADR-0011, ADR-0012). Tiers AS-005 em ADR-0009 permanecem como registro histórico da sessão.

## Medical Form Engine (AS-006 — ADR-0010)

Platform Service **Confirmed** de captura estruturada de formulários clínicos.

| Termo | Definição |
|---|---|
| **Form Definition** | Schema versionado de formulário — seções, campos, tipos (owner: Engine). |
| **Form Instance** | Preenchimento em runtime — sem ownership clínico. |
| **Clinical Content** | Conhecimento clínico persistido após aceite do domínio destino. |

**Regra:** Engine valida forma; domínio valida conteúdo clínico.

Documentação: `docs/05-architecture/platform-services.md`, `docs/05-architecture/medical-form-engine.md`, `docs/05-architecture/document-engine.md`.

## Document Engine (AS-007 — ADR-0011)

Platform Service **Confirmed** de geração e renderização formal de documentos clínicos.

| Termo | Definição |
|---|---|
| **Document Template** | Estrutura reutilizável de documento — owner: Template Service. |
| **Generation Instance** | Solicitação de render com template + dados — owner: Engine. |
| **Clinical Artifact** | Documento formal com ciclo de vida — owner: Clinical Documents Domain. |

**Regra:** Engine renderiza; domínio define regra e ciclo de vida do artefato.

Documentação: `docs/05-architecture/document-engine.md`.

## Event Strategy (AS-010 — ADR-0012)

Estratégia conceitual de eventos — **Q-003 Answered** (modelo); broker **Deferred** Sprint 3.

| Camada | Papel |
|---|---|
| **Event Foundation (Core)** | Contrato: envelope, contexto tenant, regras de publicação — sem catálogo (I-07) |
| **Event Bus (PS Confirmed)** | Transporte interno: pub/sub, roteamento, entrega |
| **Domain Event** | Owner: domínio publicador — tipos e payload semântico |
| **Read Models / Audit** | Assinantes; Audit também via API síncrona |

**Taxonomia:** (A) Clinical Event / Care Journey Start · (B) Domain Event · (C) Platform Event Message.

**Regra:** módulo orquestra; domínio valida, persiste e publica; bus só interna (Integration/Webhook fora — ADR-0004).

Documentação: `docs/05-architecture/event-strategy.md`.

## Catálogo (referência)

- Identity Service.
- Authorization Service.
- Audit Service.
- Communication Service.
- Notification Service.
- Integration Service.
- Webhook Service.
- File Service.
- Storage Service.
- Configuration Service.
- Feature Flag Service.
- Observability Service.
- Template Service.
- Document Engine.
- Medical Form Engine.
- Event Bus.
- Search Service.

## Regra de Decisão

Uma funcionalidade deve ser considerada Platform Service quando:

- É usada por mais de um módulo.
- Não pertence exclusivamente a um domínio de negócio.
- Precisa de contrato estável.
- Possui responsabilidade transversal.
- Deve ser auditável, configurável ou extensível.
- Pode ser reutilizada por diferentes modelos operacionais.

## Communication Service

Nenhum módulo deve enviar e-mail, WhatsApp, SMS, push ou notificação diretamente.

Todos devem solicitar comunicação por meio do Communication Service.

Esse serviço deve centralizar:

- Canais.
- Templates.
- Preferências.
- Consentimentos.
- Logs.
- Retries.
- Provedores.
- Histórico de envio.
- Regras de comunicação.

## Integration Service

Integrações entre sistemas devem ser tratadas separadamente da comunicação com pessoas.

O Integration Service deve centralizar conectores, APIs externas, webhooks, contratos de interoperabilidade e troca de dados com sistemas externos.

---

# 12. Princípios Arquiteturais

## 12.1 Core Pequeno e Protegido

O Core da plataforma deve permanecer pequeno, estável e protegido contra regras específicas de clientes ou modelos operacionais.

Regras específicas devem ser implementadas por extensão, configuração, módulos ou serviços especializados.

## 12.2 Crescimento por Extensão

A plataforma deve crescer por extensão, não por alteração estrutural constante do Core.

Novos modelos operacionais devem ser adicionados sem comprometer a fundação existente.

## 12.3 Configuração acima de Customização

Sempre que possível, diferenças entre clientes devem ser tratadas por configuração.

Customizações por cliente devem ser evitadas quando houver alternativa configurável.

## 12.4 Capabilities antes de Módulos

Novos módulos devem derivar de Business Capabilities e Business Domains.

Não se deve criar módulos apenas porque uma tela ou feature foi solicitada.

## 12.5 Separação de Responsabilidades

Capabilities, domínios, módulos e serviços devem possuir responsabilidades claras.

Comunicação com pessoas e integração entre sistemas são responsabilidades distintas.

## 12.6 Baixo Acoplamento

Módulos não devem conhecer diretamente detalhes internos de outros módulos.

A comunicação entre partes da plataforma deve ocorrer por contratos, APIs, eventos ou Platform Services.

## 12.7 Auditabilidade por Padrão

Toda ação relevante deve ser auditável.

A auditoria deve ser transversal e centralizada, não implementada de forma duplicada em cada módulo.

## 12.8 Segurança e LGPD desde a Fundação

Permissões, consentimentos, rastreabilidade e proteção de dados devem ser considerados desde o início da arquitetura.

## 12.9 AI Ready

A plataforma deve ser preparada para inteligência artificial, mas não dependente dela.

IA deve consumir dados estruturados, eventos, jornadas, documentos e indicadores gerados por uma arquitetura bem modelada.

## 12.10 Documentação antes de Implementação

A fundação conceitual e arquitetural deve ser consolidada antes do desenvolvimento dos domínios de negócio.

Código não deve ser usado para descobrir a arquitetura central da plataforma.

---

# 13. Architecture Sessions e Fluxo de Trabalho

A metodologia do projeto segue o fluxo:

```text
Architecture Session
↓
Workspace Draft
↓
Decision
↓
ADR
↓
Official Documentation
↓
AI Context
↓
Implementation
```

## Architecture Session

Espaço de investigação, perguntas, hipóteses, alternativas e discussão.

## Workspace

Área de trabalho da sessão. Pode conter perguntas, ideias, notas, decisões preliminares e rascunhos.

## ADR

Registra decisões arquiteturais importantes e suas justificativas.

ADR responde “por que decidimos isso?”.

## Documentação Oficial

Registra o conhecimento consolidado.

Documentação responde “como a plataforma deve ser compreendida?”.

## AI Context

Resume o conhecimento consolidado de forma útil para ferramentas de IA e desenvolvimento assistido.

---

# 14. Regras para Uso por IA

Ao atuar neste projeto, uma IA deve obedecer às seguintes regras:

1. Não tratar a Health Platform como um sistema simples de clínica.
2. Não propor implementação antes de entender a capability e o domínio envolvidos.
3. Não criar módulos baseados apenas em telas.
4. Não duplicar responsabilidades que deveriam pertencer a Platform Services.
5. Não misturar comunicação com pessoas e integração entre sistemas.
6. Não tratar prontuário como centro absoluto da plataforma.
7. Não tratar atendimento como unidade máxima do cuidado.
8. Não acoplar regras específicas de cliente ao Core.
9. Não propor customização quando configuração for suficiente.
10. Não ignorar auditoria, segurança, LGPD e rastreabilidade.
11. Sempre considerar a Jornada de Cuidado e o Modelo Hierárquico do Cuidado.
12. Sempre verificar se uma nova funcionalidade pertence a uma capability existente.
13. Sempre avaliar se uma funcionalidade deve ser domínio, módulo ou Platform Service.
14. Sempre preservar a arquitetura orientada por negócio.

---

# 15. Critérios para Novas Funcionalidades

Antes de propor ou implementar qualquer funcionalidade, responder:

## 15.1 Pergunta de Negócio

Qual problema real de uma instituição de saúde essa funcionalidade resolve?

## 15.2 Capability

A qual Core Business Capability ela pertence?

## 15.3 Domínio

Qual Business Domain será responsável por essa funcionalidade?

## 15.4 Core ou Extensão

Essa funcionalidade pertence ao Core ou a um modelo operacional específico?

## 15.5 Platform Service

Ela deve ser reutilizável por múltiplos módulos?

## 15.6 Eventos

Ela produz ou consome eventos relevantes?

## 15.7 Auditoria

Ela precisa ser auditada?

## 15.8 Configuração

Quais partes precisam ser configuráveis por tenant, instituição ou unidade?

## 15.9 Segurança

Quais permissões, papéis e restrições de acesso são necessárias?

## 15.10 Evolução

Essa decisão continuará válida se a plataforma crescer para hospital, home care, laboratório e telemedicina?

---

# 16. Estado Atual da Fundação

Até o momento, os seguintes fundamentos estão consolidados:

- Health Platform como plataforma operacional de saúde.
- Orientação por modelos operacionais.
- Jornada de Cuidado como centro conceitual.
- Prontuário como visão da jornada, não como centro da plataforma.
- Modelo Hierárquico do Cuidado.
- Core Business Capabilities.
- Separação entre Comunicação e Integração.
- Platform Services como responsabilidades reutilizáveis.
- Core pequeno e protegido.
- Crescimento por extensão.
- Configuração acima de customização.
- Business Domains (16 domínios — ADR-0008).
- Core Platform boundary e Module Strategy (AS-005 — Q-007).
- Documentação antes da implementação dos domínios de negócio.

Os seguintes fundamentos ainda precisam ser consolidados:

- Bounded Contexts.
- Event Model.
- Domain Aggregates.
- Technical Architecture.
- Backend Architecture.
- Frontend Architecture.
- Database Architecture.
- Security Architecture.
- Deployment Strategy.
- Development Guidelines.

---

# 17. Perguntas Arquiteturais em Aberto

As principais perguntas em aberto são:

## Q-001 — Quando uma instituição assume responsabilidade pelo cuidado de um paciente?

Essa pergunta define quando uma Institution Care Journey começa a ser representada pela plataforma.

Possíveis gatilhos:

- Agendamento.
- Encaminhamento.
- Admissão.
- Solicitação de exame.
- Programa de acompanhamento.
- Atendimento de urgência.

A resposta impactará domínio clínico, jornada, episódios, eventos, APIs, banco de dados e Clinical Workspace.

## Q-002 — Como decompor as Core Business Capabilities em Business Domains?

**Status:** Answered (ADR-0008, AS-004).

16 Business Domains ativos derivados do agrupamento de 39 sub-capabilities. Critérios Domain vs Platform Service vs Read Model definidos. Dois read models e domínios deferred documentados em `docs/04-domain/`.

## Q-007 — Qual a fronteira Core / Platform Services / Modules?

**Status:** Answered (ADR-0009, AS-005, 2026-07-03).

Core Platform = 8 contratos/invariantes. Platform Services = implementações transversais (15 Confirmed + 2 Strong + 1 Needs Review). 15 módulos candidatos com cardinalidade flexível. Classification matrix em 6 categorias. Clinical Workspace = shell M-02.

Documentação: `docs/05-architecture/core-platform.md`, `docs/05-architecture/module-strategy.md`, `docs/05-architecture/architecture-classification.md`, **ADR-0009**.

## Q-003 — Qual será o Event Model da plataforma?

**Status:** Answered (conceitual — ADR-0012, AS-010); tecnologia broker **Deferred** Sprint 3.

Event Foundation (Core) + Event Bus (PS Confirmed) + taxonomia em três camadas. Domínio publica; bus transporta internamente. Ver `event-strategy.md`.

## Q-004 — Quais são os principais agregados do domínio clínico?

Conceitos como Patient, Care Journey, Care Episode, Attendance, Clinical Event e Clinical Artifact precisam ser refinados em agregados de domínio.

---

# 18. Próximas Etapas Recomendadas

A sequência recomendada é:

```text
1. ~~Fechar ADR de Capability Driven Architecture.~~ ✅
2. ~~Consolidar Business Capability Map.~~ ✅
3. ~~Derivar Business Domains (AS-004, ADR-0008).~~ ✅
4. ~~AS-005 — Core Platform / Module Strategy.~~ ✅
5. Definir Bounded Contexts.
6. ~~Definir Event Model.~~ ✅ ADR-0012
7. ~~Consolidar Platform Services (doc oficial).~~ ✅
8. ~~Criar Development Guidelines.~~ ✅
9. ~~Criar regras para Cursor e Claude.~~ ✅
10. Sprint 3 — Technical Architecture 🟢 (AS-018 ✅ ADR-0021; próximo: DevOps / Observability).
11. Somente depois iniciar desenvolvimento propriamente dito.
```

---

# 19. Decisões que Não Devem Ser Reabertas sem Forte Justificativa

As seguintes decisões fazem parte da fundação e não devem ser alteradas sem novo ADR:

- Plataforma orientada por Jornada de Cuidado.
- Modelo Hierárquico do Cuidado.
- Core Business Capabilities como base do Business Capability Map.
- 16 Business Domains como organização de negócio (ADR-0008).
- Separação entre Comunicar e Integrar.
- Platform Services para responsabilidades compartilhadas.
- Core pequeno e protegido.
- Crescimento por extensão.
- Configuração acima de customização.
- Documentação conceitual antes do desenvolvimento dos domínios.
- Event Foundation (Core) vs Event Bus (PS); tipos no domínio publicador (ADR-0012).
- Tenant = isolamento SaaS; shared+discriminator padrão; Organization no domínio (ADR-0013).
- Modular monolith padrão; Module ≠ deploy; camadas API→App→Domain→Infra (ADR-0014).
- Shared DB + tenant_id; Outbox; Domain vs Storage/File (ADR-0015).
- Sync ≠ Domain Event; Product/Platform/Integration; REST-like + `/v{n}` (ADR-0016).
- At-least-once; ordering por stream; Outbox→broker; critérios de broker (ADR-0017).
- Aggregate roots clínicos; Clinical Event (A) ≠ Domain Event (B) (ADR-0018).
- MVP Must/Should/Later/Out; Journey ambulatorial+tele; Billing Out (ADR-0019).
- AuthN/AuthZ/Audit em PS; Role+Permission+Scope; deny-by-default; pipeline; I-10 (ADR-0020).
- Staff/Portal/Admin; shell M-02; OQ-C03 Module Registry; BFF opcional (ADR-0021).

---

# 20. Resumo para IA

A Health Platform é uma plataforma SaaS modular para saúde, orientada por modelos operacionais e jornadas de cuidado.

Ela não deve ser construída a partir de telas ou módulos isolados. Deve ser organizada por Core Business Capabilities, derivar **16 Business Domains** (ADR-0008) e só depois gerar módulos, features e componentes.

O centro conceitual da plataforma é a Jornada de Cuidado representada por uma instituição, e não o prontuário, a agenda ou a consulta.

O modelo hierárquico do cuidado é:

```text
Patient → Care Journey → Care Episode → Attendance → Clinical Event → Clinical Artifact
```

As oito Core Business Capabilities são:

```text
Relacionar → Organizar → Executar → Registrar → Comunicar → Integrar → Monitorar → Governar
```

A plataforma deve ter Core pequeno, crescer por extensão, favorecer configuração em vez de customização e usar Platform Services para responsabilidades compartilhadas.

Qualquer proposta de código, domínio, módulo, API ou banco de dados deve respeitar esses fundamentos.

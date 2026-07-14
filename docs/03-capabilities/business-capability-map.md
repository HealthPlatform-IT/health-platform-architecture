---
title: Business Capability Map
status: Draft
version: 0.1.0
created: 2026-07-02
updated: 2026-07-02
author: Architecture Team
category: Capabilities
phase: Product & Architecture Foundation
related:
  - docs/03-capabilities/core-business-capabilities.md
  - docs/02-business-processes/healthcare-operating-model.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - architecture-sessions/AS-002-business-capability-map.md
---

# Business Capability Map

> Este documento decompõe as oito Core Business Capabilities da Health Platform em sub-capabilities de negócio. Ele detalha **o que** a plataforma precisa ser capaz de realizar, sem definir módulos, domínios ou implementação.

---

# Objetivo

Fornecer o mapa oficial de Business Capabilities da Health Platform como ponte entre as Core Business Capabilities e os **Business Domains** (ADR-0008).

Este documento responde: *como as oito capacidades fundamentais se decompõem em competências operacionais concretas*.

---

# O que é uma Business Capability neste mapa

Neste documento, **Business Capability** refere-se a uma sub-capability de segundo nível — uma decomposição de uma Core Business Capability.

| Nível | Nome | Estabilidade |
|---|---|---|
| 1 | Core Business Capability | Permanente — 8 capacidades fundamentais |
| 2 | Business Capability (este mapa) | Estável, mas pode evoluir com versionamento |
| 3+ | Business Domain, Module, Feature | Derivados — evoluem com o produto |

## O que NÃO é

Uma Business Capability neste mapa **não é**:

- Um módulo, feature ou tela.
- Um Business Domain ou Bounded Context.
- Um Platform Service ou componente técnico.
- Um agrupamento funcional como "módulo de agenda" ou "módulo financeiro".

---

# Relação com outros artefatos

```text
Healthcare Operating Model
        ↓
Core Business Capabilities        ← docs/03-capabilities/core-business-capabilities.md
        ↓
Business Capability Map           ← este documento
        ↓
Business Domains                  ← ADR-0008 (16 domínios)
        ↓
Modules → Features
```

As Core Business Capabilities permanecem como fundação. Este mapa as detalha sem alterá-las.

---

# Visão geral do mapa

```text
Relacionar
├── Patient Relationship
├── Professional & Provider Relationship
├── Organization & Network Relationship
└── Insurance & Payer Relationship

Organizar
├── Care Scheduling
├── Resource Planning
├── Queue & Flow Management
├── Referral Coordination
└── Operational Workflow

Executar
├── Clinical Attendance
├── Therapeutic Care Delivery
├── Diagnostic Service Delivery
├── Home Care Delivery
└── (Future) Hospital Care Delivery

Registrar
├── Clinical Documentation
├── Diagnostic Recording
├── Prescription & Clinical Orders
├── Clinical Artifacts
└── Clinical Timeline

Comunicar
├── Patient Communication
├── Professional Communication
├── Internal Notifications
└── Communication Preferences & Consent

Integrar
├── Clinical Interoperability
├── Diagnostic Systems Integration
├── Financial & ERP Integration
├── Webhooks & External APIs
└── Medical Devices & IoT

Monitorar
├── Care Pendencies & Follow-up
├── Clinical Alerts & Protocols
├── Operational SLAs
├── Indicators & Dashboards
└── (Future) Intelligence & Automation

Governar
├── Identity & Access
├── Authorization & Permissions
├── Audit & Traceability
├── Configuration & Feature Management
├── Compliance & Data Protection
└── Observability
```

---

# Mapa detalhado

## Relacionar

Estabelecer e manter vínculos operacionais e assistenciais entre participantes do ecossistema de saúde.

### Patient Relationship

Gerenciar o vínculo entre pacientes e a instituição, incluindo cadastro, responsáveis, cuidadores e histórico de relacionamento institucional.

**Exemplos:** cadastro de paciente, vínculo com responsável legal, programa de acompanhamento.

### Professional & Provider Relationship

Gerenciar o vínculo com profissionais de saúde e prestadores que atuam na instituição.

**Exemplos:** cadastro de profissional, especialidades, vínculo com unidade, credenciamento de prestador.

### Organization & Network Relationship

Gerenciar a estrutura organizacional e redes de relacionamento entre instituições, unidades e parceiros.

**Exemplos:** organizações, clínicas, unidades, redes de atendimento, parcerias institucionais.

### Insurance & Payer Relationship

Gerenciar o vínculo com convênios, operadoras de saúde e pagadores.

**Exemplos:** cadastro de convênio, planos, regras de cobertura, vínculo paciente-convênio.

---

## Organizar

Planejar e coordenar como o cuidado será realizado.

### Care Scheduling

Planejar e agendar interações de cuidado entre pacientes e profissionais.

**Exemplos:** agendamento de consulta, reagendamento, bloqueio de horário, confirmação de presença.

### Resource Planning

Alocar e gerenciar recursos necessários para a operação do cuidado.

**Exemplos:** disponibilidade de profissional, salas, equipamentos, grade de atendimento.

### Queue & Flow Management

Controlar filas, prioridades e fluxo operacional de pacientes e atendimentos.

**Exemplos:** fila de espera, priorização, check-in, painel de chamada.

### Referral Coordination

Coordenar encaminhamentos entre profissionais, especialidades e instituições.

**Exemplos:** encaminhamento interno, encaminhamento externo, solicitação de parecer.

### Operational Workflow

Definir e executar fluxos operacionais configuráveis da instituição.

**Exemplos:** workflow de recepção, fluxo de preparação para exame, protocolo operacional.

---

## Executar

Realizar efetivamente o cuidado ou serviço de saúde.

### Clinical Attendance

Realizar interações clínicas entre paciente e profissional.

**Exemplos:** consulta presencial, teleconsulta, retorno, avaliação clínica.

Alinhado ao nível **Attendance** do Modelo Hierárquico do Cuidado (ADR-0001).

### Therapeutic Care Delivery

Realizar sessões e tratamentos terapêuticos recorrentes.

**Exemplos:** sessão de fisioterapia, psicoterapia, nutrição, fonoaudiologia.

### Diagnostic Service Delivery

Realizar procedimentos e serviços de diagnóstico.

**Exemplos:** coleta laboratorial, exame de imagem, procedimento diagnóstico.

### Home Care Delivery

Realizar cuidado fora do ambiente institucional.

**Exemplos:** visita domiciliar, acompanhamento remoto, cuidado distribuído.

### (Future) Hospital Care Delivery

Realizar atendimentos de alta complexidade hospitalar.

**Exemplos:** internação, centro cirúrgico, urgência e emergência hospitalar.

> Escopo futuro — não faz parte da fundação atual.

---

## Registrar

Produzir e manter o conhecimento clínico e operacional gerado durante o cuidado.

### Clinical Documentation

Registrar evoluções, anamneses e notas clínicas produzidas durante o cuidado.

**Exemplos:** evolução clínica, anamnese, nota de enfermagem, registro de sinais vitais.

### Diagnostic Recording

Registrar diagnósticos, hipóteses e conclusões clínicas.

**Exemplos:** diagnóstico CID, hipótese diagnóstica, conclusão de avaliação.

### Prescription & Clinical Orders

Registrar prescrições, solicitações e ordens clínicas.

**Exemplos:** prescrição de medicamento, solicitação de exame, ordem terapêutica.

### Clinical Artifacts

Produzir e manter artefatos clínicos formais gerados durante o cuidado.

**Exemplos:** atestado, laudo, relatório clínico, termo de consentimento, encaminhamento formal.

Alinhado ao nível **Clinical Artifact** do Modelo Hierárquico do Cuidado (ADR-0001).

### Clinical Timeline

Manter a linha do tempo clínica consolidada do paciente dentro da instituição.

**Exemplos:** timeline de eventos clínicos, histórico consolidado, visão cronológica do cuidado.

> O prontuário é uma **visão** desta capability — não o centro conceitual da plataforma.

---

## Comunicar

Levar informações relevantes às **pessoas** envolvidas na jornada de cuidado.

### Patient Communication

Comunicar informações ao paciente e seus responsáveis.

**Exemplos:** confirmação de consulta, lembrete de retorno, resultado disponível, orientação pós-atendimento.

**Canais:** e-mail, WhatsApp, SMS, push notification, portal do paciente.

### Professional Communication

Comunicar informações a profissionais de saúde.

**Exemplos:** notificação de encaminhamento, alerta de resultado crítico, comunicação entre equipes.

### Internal Notifications

Comunicar informações a usuários operacionais e administrativos da instituição.

**Exemplos:** notificação interna, alerta operacional, aviso de pendência.

### Communication Preferences & Consent

Gerenciar preferências de canal, consentimentos e regras de comunicação por pessoa.

**Exemplos:** opt-in de WhatsApp, preferência de e-mail, consentimento LGPD para comunicação.

> Nenhum módulo deve enviar mensagens diretamente — toda comunicação deve passar por Platform Services.

---

## Integrar

Permitir troca segura e padronizada de informações com **sistemas externos**.

### Clinical Interoperability

Integrar com padrões e protocolos clínicos de interoperabilidade.

**Exemplos:** FHIR, TISS, troca de prontuário, compartilhamento de dados clínicos.

### Diagnostic Systems Integration

Integrar com sistemas de diagnóstico e laboratório.

**Exemplos:** LIS, PACS, DICOM, recebimento de laudos externos.

### Financial & ERP Integration

Integrar com sistemas financeiros e de gestão empresarial.

**Exemplos:** gateway de pagamento, ERP hospitalar, sistemas de faturamento externos.

### Webhooks & External APIs

Expor e consumir interfaces programáticas com sistemas externos.

**Exemplos:** webhooks de eventos, APIs públicas, conectores de terceiros.

### Medical Devices & IoT

Integrar com dispositivos médicos e sensores.

**Exemplos:** wearables, equipamentos com saída digital, dispositivos IoT de monitoramento.

> Integrar é voltado para **sistemas**. Canais de comunicação com pessoas (e-mail, WhatsApp, SMS) pertencem a **Comunicar**.

---

## Monitorar

Acompanhar continuamente processos, eventos, pendências e indicadores relacionados ao cuidado.

### Care Pendencies & Follow-up

Monitorar pendências e acompanhar retornos previstos no cuidado.

**Exemplos:** exame pendente, retorno agendado não realizado, acompanhamento de condição crônica.

### Clinical Alerts & Protocols

Monitorar condições clínicas e disparar alertas baseados em protocolos.

**Exemplos:** alerta de resultado crítico, protocolo de risco, lembrete de vacinação.

### Operational SLAs

Monitorar prazos e níveis de serviço operacionais.

**Exemplos:** tempo de espera, prazo de laudo, SLA de atendimento.

### Indicators & Dashboards

Acompanhar indicadores operacionais e assistenciais.

**Exemplos:** dashboard operacional, indicadores de produtividade, métricas de atendimento.

### (Future) Intelligence & Automation

Monitorar e automatizar com base em inteligência artificial e modelos preditivos.

**Exemplos:** recomendação clínica, automação de processos, modelos preditivos.

> Escopo futuro — a capability Monitorar é fundação para IA, mas não depende dela.

---

## Governar

Garantir segurança, rastreabilidade, conformidade e controle sobre toda a operação.

### Identity & Access

Gerenciar identidade de usuários e acesso à plataforma.

**Exemplos:** autenticação, gestão de usuários, multi-tenant, sessões.

### Authorization & Permissions

Controlar o que cada usuário pode ver e fazer na plataforma.

**Exemplos:** papéis, permissões, controle de acesso por recurso, políticas de autorização.

### Audit & Traceability

Registrar e rastrear ações relevantes realizadas na plataforma.

**Exemplos:** log de auditoria, histórico de alterações, rastreabilidade de acesso a dados clínicos.

### Configuration & Feature Management

Gerenciar configurações da plataforma e controle de funcionalidades.

**Exemplos:** configuração por tenant, feature flags, parametrização institucional, branding.

### Compliance & Data Protection

Garantir conformidade regulatória e proteção de dados pessoais.

**Exemplos:** LGPD, consentimentos, políticas de retenção, anonimização.

### Observability

Monitorar a saúde técnica e operacional da plataforma.

**Exemplos:** logs, métricas, tracing, alertas de infraestrutura.

> Governar é **transversal** — permeia todas as demais Core Business Capabilities.

---

# Contagem oficial

| Métrica | Valor |
|---|---|
| Core Business Capabilities | 8 |
| Sub-capabilities (total) | **39** |
| Sub-capabilities ativas (mapeadas) | 35 |
| Sub-capabilities deferred/future | 4 |

As quatro sub-capabilities deferred/future são: Hospital Care Delivery, Financial & ERP Integration, Intelligence & Automation e o domínio futuro Billing & Financial (Q-005). Referências históricas a "35 sub-capabilities" indicam o catálogo ativo mapeado em [domain-map.md](../04-domain/domain-map.md).

---

# Princípios do mapa

| Princípio | Descrição |
|---|---|
| Estabilidade das cores | As 8 Core Business Capabilities não mudam; sub-capabilities podem evoluir com versionamento |
| Capability primária | Toda funcionalidade deve ter uma sub-capability principal de responsabilidade |
| Comunicar ≠ Integrar | Canais com pessoas pertencem a Comunicar; troca com sistemas pertence a Integrar |
| Sem módulos | Este mapa não nomeia módulos — eles serão derivados na fase de domínios |
| Jornada de Cuidado | Sub-capabilities de Executar e Registrar operam sobre o Modelo Hierárquico do Cuidado |

---

# Relação com Platform Services

Sub-capabilities serão suportadas por Platform Services reutilizáveis, mas são conceitos distintos:

| Conceito | Pergunta que responde |
|---|---|
| Business Capability | O que a plataforma precisa ser capaz de fazer? |
| Platform Service | Como a plataforma implementa uma responsabilidade transversal? |

O catálogo de Platform Services será documentado em `docs/05-architecture/platform-services.md`.

---

# Relação com Business Domains

Business Domains agrupam sub-capabilities relacionadas em fronteiras de responsabilidade.

```text
Business Capability Map (este documento — 39 sub-capabilities)
        ↓
Business Domains (ADR-0008 — 16 domínios)
        ↓
Bounded Contexts
```

A decomposição em domínios está formalizada em [ADR-0008](../05-architecture/adr/foundation/ADR-0008-business-domain-map.md) e [business-domains.md](../04-domain/business-domains.md) (Q-002 Answered).

---

# Decisões relacionadas

| Artefato | Relação |
|---|---|
| [Core Business Capabilities](core-business-capabilities.md) | Fonte das 8 capacidades fundamentais |
| [ADR-0001 — Modelo Hierárquico do Cuidado](../05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md) | Estrutura clínica para Executar e Registrar |
| AS-002 — Business Capability Map | Decisão de organizar a plataforma por capabilities |
| ADR-0002 — Capability-Driven Architecture | A ser criado |

---

# Open Questions

| ID | Pergunta | Situação |
|---|---|---|
| Q-002 | Como agrupar sub-capabilities em Business Domains? | ✅ Answered — ADR-0008, AS-004 |
| Q-005 | Onde posicionar capabilities administrativas e financeiras (faturamento, estoque, gestão financeira)? | Pendente — não consolidado nas 8 cores |
| Q-006 | Quais sub-capabilities pertencem ao escopo inicial (MVP) da plataforma? | ✅ Answered — ADR-0019, AS-017 |

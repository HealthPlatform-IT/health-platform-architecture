---
title: ADR-0004 - Communication and Integration Separation
status: Accepted
date: 2026-07-02
deciders:
  - Architecture Team
tags:
  - architecture
  - communication
  - integration
  - foundation
  - platform-services
related:
  - architecture-sessions/AS-002-business-capability-map.md
  - workspace/AS-002/decisions.md
  - docs/03-capabilities/core-business-capabilities.md
  - docs/03-capabilities/business-capability-map.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
---

# ADR-0004 — Communication and Integration Separation

## Status

Accepted

---

# Contexto

A Health Platform possui duas Core Business Capabilities distintas relacionadas à troca de informações:

- **Comunicar** — levar informações relevantes às **pessoas** envolvidas na jornada de cuidado.
- **Integrar** — permitir troca segura e padronizada de informações com **sistemas externos**.

Durante a **AS-002 — Business Capability Map**, a **Decisão 002** estabeleceu que toda comunicação com pessoas deve ser centralizada em um **Communication Service**, e que nenhum módulo deve enviar mensagens diretamente por canais externos.

Foi identificado que tratar comunicação com pessoas e integração com sistemas como uma única responsabilidade cria acoplamento, dificulta conformidade regulatória e impede evolução independente de cada capability.

---

# Problema

Como garantir que:

- Módulos **não enviem mensagens diretamente** a pacientes, profissionais ou usuários por e-mail, WhatsApp, SMS ou push.
- Comunicação com **pessoas** e troca com **sistemas** sejam tratadas como responsabilidades distintas.
- Novos canais de comunicação e novos conectores de integração possam ser adicionados **sem alterar módulos consumidores**.
- Consentimentos, preferências e LGPD sejam aplicados corretamente na comunicação com pessoas.
- Protocolos de interoperabilidade (FHIR, TISS, DICOM) sejam tratados na integração com sistemas.

---

# Decisão

A Health Platform manterá **Comunicar** e **Integrar** como capabilities distintas, cada uma suportada por um **Platform Service** dedicado.

## 1. Comunicar — voltado para pessoas

A capability **Comunicar** é responsável por levar informações a **pessoas** envolvidas na jornada de cuidado.

**Escopo:**

- Comunicação com pacientes e responsáveis.
- Comunicação com profissionais de saúde.
- Notificações internas a usuários operacionais e administrativos.
- Preferências de canal, consentimentos e regras de comunicação.

**Canais:** e-mail, WhatsApp, SMS, push notification, portal do paciente, portal do profissional.

**Platform Service:** Communication Service.

## 2. Integrar — voltado para sistemas

A capability **Integrar** é responsável por troca de informações com **sistemas externos**.

**Escopo:**

- Interoperabilidade clínica (FHIR, TISS).
- Sistemas de diagnóstico (PACS, DICOM, LIS).
- ERPs, gateways de pagamento e sistemas financeiros.
- Webhooks, APIs externas e conectores de terceiros.
- Dispositivos médicos e IoT.

**Platform Service:** Integration Service.

## 3. Communication Service centralizado

Nenhum módulo da plataforma deve enviar mensagens diretamente por e-mail, WhatsApp, SMS, push notification ou outros canais.

Todos os módulos devem solicitar comunicações por meio do **Communication Service**, responsável por centralizar:

- Canais e provedores.
- Templates de mensagem.
- Preferências do usuário.
- Consentimentos de comunicação.
- Filas, tentativas de reenvio e logs de entrega.
- Histórico de envio.
- Regras de comunicação.

## 4. Integration Service separado

Integrações com sistemas externos devem ser tratadas pelo **Integration Service**, responsável por centralizar:

- Conectores e adaptadores.
- Contratos de interoperabilidade.
- APIs externas e webhooks.
- Troca de dados com sistemas de terceiros.
- Transformação e validação de payloads.

## 5. Comparação

| Dimensão | Comunicar | Integrar |
|---|---|---|
| Destinatário | Pessoas | Sistemas |
| Exemplos | E-mail, WhatsApp, SMS, push, portal | FHIR, TISS, PACS, LIS, ERP, webhooks |
| Core Capability | Comunicar | Integrar |
| Platform Service | Communication Service | Integration Service |
| Riscos principais | LGPD, consentimento, preferências | Interoperabilidade, contratos, segurança |
| Consumidor | Módulo solicita envio a pessoa | Módulo/serviço troca dados com externo |

## 6. Regras de classificação

| Situação | Capability | Serviço |
|---|---|---|
| Enviar lembrete de consulta ao paciente por WhatsApp | Comunicar | Communication Service |
| Receber laudo de laboratório externo via HL7/FHIR | Integrar | Integration Service |
| Notificar profissional sobre resultado crítico | Comunicar | Communication Service |
| Enviar dados de faturamento para ERP externo | Integrar | Integration Service |
| Webhook recebido de gateway de pagamento | Integrar | Integration Service |
| Confirmação de agendamento por SMS | Comunicar | Communication Service |
| Integração com PACS para exame de imagem | Integrar | Integration Service |

---

# O que pertence a Comunicar

Sub-capabilities do Business Capability Map:

- Patient Communication
- Professional Communication
- Internal Notifications
- Communication Preferences & Consent

**Não pertence a Comunicar:** troca de dados com LIS, PACS, ERP, FHIR server, webhooks de sistemas, dispositivos IoT.

---

# O que pertence a Integrar

Sub-capabilities do Business Capability Map:

- Clinical Interoperability
- Diagnostic Systems Integration
- Financial & ERP Integration
- Webhooks & External APIs
- Medical Devices & IoT

**Não pertence a Integrar:** envio de e-mail, WhatsApp, SMS ou push a pessoas — mesmo quando o provedor do canal é externo (ex.: API do WhatsApp).

> O provedor do canal é um detalhe de implementação do Communication Service. A capability continua sendo **Comunicar**.

---

# Alternativas consideradas

## Alternativa A — Camada única de integração

Tratar comunicação com pessoas e integração com sistemas em uma única camada de "integrações".

**Vantagens:** menos serviços para gerenciar; aparente simplicidade.

**Desvantagens:** mistura responsabilidades distintas; dificulta LGPD e consentimento; acopla canais de mensagem a protocolos clínicos; dificulta evolução independente.

**Resultado:** rejeitada.

---

## Alternativa B — Cada módulo gerencia seus canais e integrações

Permitir que módulos enviem mensagens e integrem com sistemas externos diretamente.

**Vantagens:** autonomia por módulo; implementação inicial mais rápida.

**Desvantagens:** duplicação de código; inconsistência entre módulos; impossível trocar provedores centralmente; risco de violação de LGPD; acoplamento generalizado.

**Resultado:** rejeitada.

---

## Alternativa C — Capabilities e Platform Services separados

Manter Comunicar e Integrar como capabilities distintas, cada uma com seu Platform Service dedicado.

**Vantagens:** responsabilidades claras; conformidade regulatória; troca de provedores sem alterar módulos; evolução independente; contratos estáveis.

**Desvantagens:** exige disciplina para não misturar; casos limítrofes exigem critério arquitetural.

**Resultado:** adotada.

---

# Justificativa

Comunicar e Integrar possuem **destinatários, contratos, riscos e regulamentações diferentes**.

A comunicação com pessoas envolve consentimento, preferências de canal, LGPD e experiência do usuário. A integração com sistemas envolve protocolos de interoperabilidade, transformação de dados, conectores e contratos técnicos.

Centralizar cada responsabilidade em um Platform Service dedicado:

- Reduz acoplamento entre módulos.
- Facilita troca de provedores externos.
- Permite adicionar novos canais e conectores sem alterar consumidores.
- Garante consistência de auditoria, logs e rastreabilidade.
- Alinha a implementação às Core Business Capabilities (ADR-0002).

---

# Regras derivadas

1. **Nenhum módulo envia mensagem diretamente** — toda comunicação com pessoas passa pelo Communication Service.
2. **Nenhum módulo integra diretamente com sistemas externos** — integrações passam pelo Integration Service.
3. **Canal externo ≠ Integrar** — usar API do WhatsApp ou SendGrid é implementação do Communication Service, não Integrar.
4. **Webhook de sistema = Integrar** — receber eventos de ERP ou gateway de pagamento é Integrar.
5. **Notificação a pessoa = Comunicar** — alertar paciente ou profissional é Comunicar, independentemente do canal.
6. **Consentimento e preferências** — gerenciados no contexto de Comunicar, não de Integrar.

---

# Consequências

## Positivas

- Separação clara de responsabilidades entre pessoas e sistemas.
- Conformidade com LGPD e consentimentos centralizada na comunicação.
- Troca de provedores de canal e conectores sem impacto em módulos.
- Contratos estáveis para consumidores de Communication e Integration Services.
- Base sólida para auditoria e rastreabilidade de envios e integrações.
- Correção da confusão entre canais de mensagem e integrações de sistema.

## Negativas

- Exige disciplina arquitetural para classificar corretamente novos fluxos.
- Casos limítrofes podem exigir discussão arquitetural antes da implementação.
- Dois Platform Services para manter em vez de um serviço unificado.

---

# Impacto na Arquitetura

Este ADR influencia diretamente:

- Platform Services (ADR-0005)
- Business Capability Map
- Modules (todos os consumidores de comunicação e integração)
- LGPD e Compliance
- Event Model
- Development Guidelines
- AI Context

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| [ADR-0002 — Capability-Driven Architecture](ADR-0002-capability-driven-architecture.md) | Comunicar e Integrar são duas das oito Core Business Capabilities |
| [ADR-0003 — Core Protection and Extension Model](ADR-0003-core-protection-and-extension-model.md) | Communication e Integration Services são mecanismos de extensão transversal |
| ADR-0005 — Platform Services | Detalhará catálogo e responsabilidades dos serviços |

Nenhum módulo ou ADR futuro deverá misturar comunicação com pessoas e integração com sistemas sem que este documento seja revisado.

---

# Open Questions

| ID | Pergunta | Situação |
|---|---|---|
| Q-010 | Qual é a fronteira entre Communication Service e Notification Service? | Pendente — ambos listados em `architecture-foundation.md`, sem decisão formal |
| Q-011 | Como tratar comunicação clínica estruturada entre sistemas (ex.: FHIR Messaging) — Comunicar ou Integrar? | Pendente — provável Integrar, sujeito a análise por caso |

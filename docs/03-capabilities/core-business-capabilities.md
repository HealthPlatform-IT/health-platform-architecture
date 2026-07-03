---
title: Core Business Capabilities
status: Draft
version: 0.2.0
created: 2026-07-02
updated: 2026-07-02
author: Architecture Team
category: Capabilities
phase: Product & Architecture Foundation
related:
  - docs/02-business-processes/healthcare-operating-model.md
  - docs/03-capabilities/business-capability-map.md
  - docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md
  - architecture-sessions/AS-002-business-capability-map.md
---

# Core Business Capabilities

> Este documento define as oito capacidades fundamentais de negócio que organizam a Health Platform. Elas representam o menor conjunto de competências necessárias para que qualquer instituição de saúde opere dentro do escopo da plataforma.

---

# Objetivo

Estabelecer oficialmente as **Core Business Capabilities** da Health Platform como base para o Business Capability Map, a definição de Business Domains e todas as decisões arquiteturais subsequentes.

Este documento responde: *o que a plataforma precisa ser capaz de fazer*, independentemente de módulos, telas ou tecnologias.

---

# O que é uma Core Business Capability

Uma Core Business Capability representa uma competência **permanente de negócio** — aquilo que qualquer instituição de saúde precisa realizar para operar.

Ela descreve **o que** a plataforma faz, e não **como** isso será implementado.

## O que NÃO é

Uma Core Business Capability **não é**:

- Um módulo ou feature.
- Uma tela ou fluxo de interface.
- Um microsserviço ou componente técnico.
- Um Business Domain.
- Um Platform Service.
- Um agrupamento funcional como "módulo de agenda" ou "módulo de prontuário".

---

# Relação com outros artefatos

As Core Business Capabilities derivam do modelo operacional da saúde e precedem qualquer decisão de implementação.

```text
Healthcare Operating Model
        ↓
Core Business Capabilities        ← este documento
        ↓
Business Capability Map
        ↓
Business Domains
        ↓
Modules
        ↓
Features
```

A plataforma é orientada pela **Jornada de Cuidado** e pelo **Modelo Hierárquico do Cuidado** (ver ADR-0001). As capabilities fornecem o vocabulário de negócio para representar essa jornada de forma operacional.

---

# As Oito Core Business Capabilities

A Health Platform é organizada em torno de oito capacidades fundamentais:

```text
Relacionar → Organizar → Executar → Registrar → Comunicar → Integrar → Monitorar → Governar
```

A sequência representa o fluxo natural do cuidado: estabelecer vínculos, planejar a operação, realizar o cuidado, registrar o conhecimento gerado, comunicar com pessoas, integrar com sistemas, monitorar continuamente e governar toda a operação.

---

## Relacionar

Estabelecer e manter vínculos operacionais e assistenciais entre todos os participantes do ecossistema de saúde.

**Escopo:** pacientes, profissionais, responsáveis, cuidadores, organizações, convênios, empresas, parceiros, laboratórios e prestadores.

**Fronteira:** o objetivo não é apenas cadastrar pessoas — é estabelecer vínculo operacional e assistencial com contexto e responsabilidade.

---

## Organizar

Planejar e coordenar como o cuidado será realizado.

**Escopo:** agenda, disponibilidade, filas, recursos, salas, profissionais, prioridades, encaminhamentos e workflows operacionais.

**Fronteira:** Organizar transforma uma necessidade de cuidado em uma operação planejada. Não executa o cuidado — prepara para sua execução.

---

## Executar

Realizar efetivamente o cuidado ou serviço de saúde.

**Escopo:** consultas, teleconsultas, sessões terapêuticas, procedimentos, coletas, exames, visitas domiciliares e, futuramente, internações ou atendimentos de alta complexidade.

**Fronteira:** Executar é a realização concreta do cuidado, representada no Modelo Hierárquico como **Attendance**. Não confundir com o registro do que foi realizado (capability Registrar).

---

## Registrar

Produzir e manter o conhecimento clínico e operacional gerado durante o cuidado.

**Escopo:** evolução clínica, diagnósticos, prescrições, exames, laudos, documentos médicos, atestados, encaminhamentos, consentimentos e linha do tempo clínica.

**Fronteira:** Registrar transforma a execução do cuidado em conhecimento persistente. O prontuário é uma **visão** desta capability — não o centro conceitual da plataforma.

---

## Comunicar

Levar informações relevantes às **pessoas** envolvidas na jornada de cuidado.

**Escopo:** e-mail, WhatsApp, SMS, push notification, notificações internas, portal do paciente, portal do profissional, templates de mensagens e histórico de envio.

**Fronteira:** Comunicar é voltado para pessoas. Nenhum módulo deve enviar mensagens diretamente — toda comunicação deve ser solicitada por meio de Platform Services de comunicação.

---

## Integrar

Permitir troca segura e padronizada de informações entre a Health Platform e **sistemas externos**.

**Escopo:** FHIR, TISS, DICOM, PACS, LIS, ERPs, gateways de pagamento, webhooks, APIs externas, dispositivos médicos, IoT e wearables.

**Fronteira:** Integrar é voltado para sistemas. Comunicar e Integrar são capabilities distintas porque possuem responsabilidades, contratos e riscos diferentes.

---

## Monitorar

Acompanhar continuamente processos, eventos, pendências e indicadores relacionados ao cuidado.

**Escopo:** exames pendentes, retornos, atrasos, status de laudos, riscos clínicos, SLAs, protocolos, alertas e dashboards operacionais.

**Fronteira:** Monitorar permite que a plataforma deixe de ser apenas reativa e passe a ser proativa. Será base importante para automação e inteligência artificial.

---

## Governar

Garantir segurança, rastreabilidade, conformidade e controle sobre toda a operação da plataforma.

**Escopo:** auditoria, permissões, autorização, LGPD, compliance, versionamento, histórico de acesso, histórico de alterações, configurações, feature flags, logs e observabilidade.

**Fronteira:** Governar é **transversal** a todas as demais capabilities. Não representa um módulo isolado — permeia toda a plataforma.

---

# Princípios de uso

Ao avaliar qualquer nova funcionalidade, identificar primeiro a qual Core Business Capability ela pertence.

| Regra | Descrição |
|---|---|
| Uma capability primária | Toda funcionalidade deve ter uma capability principal de responsabilidade |
| Múltiplas capabilities | Se uma funcionalidade parece pertencer a várias, provavelmente envolve orquestração entre domínios ou deve ser decomposta |
| Capabilities antes de módulos | Novos módulos derivam de capabilities e domínios — não o contrário |
| Estabilidade | As oito capabilities permanecem estáveis; módulos e features evoluem sobre elas |

---

# Relação com Platform Services

Core Business Capabilities e Platform Services são conceitos distintos:

| Conceito | Representa |
|---|---|
| Core Business Capability | Competência de negócio permanente |
| Platform Service | Implementação reutilizável de responsabilidade transversal |

Uma capability pode ser suportada por um ou mais Platform Services. Por exemplo, a capability **Comunicar** é suportada por um Communication Service; a capability **Governar** é suportada por Audit Service, Authorization Service e outros.

O catálogo de Platform Services será documentado separadamente.

---

# Decisões relacionadas

| Artefato | Relação |
|---|---|
| [ADR-0001 — Modelo Hierárquico do Cuidado](../05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md) | Define a estrutura clínica que as capabilities Executar e Registrar representam |
| AS-002 — Business Capability Map | Decisão de organizar a plataforma por capabilities de negócio |
| ADR-0002 — Capability-Driven Architecture | A ser criado — formalizará esta decisão |

---

# Open Questions

| ID | Pergunta | Situação |
|---|---|---|
| Q-002 | Como decompor as Core Business Capabilities em Business Domains? | ✅ Answered — ADR-0008, AS-004 |

A decomposição detalhada das capabilities (sub-capabilities) será documentada em `business-capability-map.md`.

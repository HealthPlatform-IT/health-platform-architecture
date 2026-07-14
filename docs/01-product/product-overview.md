---
title: Product Overview
status: Draft
version: 0.3.0
created: 2026-07-02
updated: 2026-07-03
author: Architecture Team
category: Product
phase: Product & Architecture Foundation
related:
  - docs/00-introduction/vision.md
  - docs/00-introduction/principles.md
  - docs/02-business-processes/healthcare-operating-model.md
  - docs/03-capabilities/core-business-capabilities.md
  - docs/04-domain/business-domains.md
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - docs/05-architecture/adr/foundation/ADR-0010-medical-form-engine.md
  - docs/05-architecture/adr/foundation/ADR-0011-document-engine.md
  - ai-context/platform-overview.md
---

# Product Overview

# Health Platform

## Introdução

A Health Platform é uma plataforma SaaS multi-tenant desenvolvida para atender instituições de saúde de diferentes portes — de consultórios individuais a redes de clínicas e laboratórios.

Ao contrário dos sistemas tradicionais centrados em prontuário ou agenda, a plataforma foi concebida para representar digitalmente a **operação de instituições de saúde** a partir de modelos operacionais, jornadas de cuidado e capacidades de negócio.

A proposta não é apenas informatizar processos clínicos, mas oferecer uma infraestrutura tecnológica capaz de acompanhar a evolução do setor da saúde durante os próximos anos — com **configuração acima de customização** e **crescimento por extensão**.

---

# Objetivo da Plataforma

Fornecer um ecossistema completo para gestão clínica, operacional e assistencial, permitindo que diferentes instituições utilizem uma única plataforma adaptada às suas necessidades.

A arquitetura atende diferentes especialidades médicas, modelos de atendimento, fluxos operacionais e níveis de complexidade **sem exigir alterações estruturais no núcleo** da plataforma.

**Proposta central:** qualquer instituição de saúde deve conseguir mapear seus processos para dentro da Health Platform.

---

# Conceito Arquitetural

A plataforma é organizada por **capacidades de negócio** e **domínios de negócio**, não por telas ou módulos funcionais isolados.

```text
Healthcare Operating Model
        ↓
Core Business Capabilities (8)
        ↓
Business Capability Map (39 sub-capabilities)
        ↓
Business Domains (16)
        ↓
Core Platform (8 contratos — ADR-0009)
        ↓
Platform Services
        ↓
Modules (15 candidatos)
        ↓
Features
```

Cada camada possui responsabilidades bem definidas, reduzindo acoplamento e permitindo evolução independente.

Decisão formal: [ADR-0002](../05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md).

---

# Camadas da Plataforma

## 1. Core Business Capabilities

As oito competências permanentes que qualquer instituição de saúde precisa exercer:

```text
Relacionar → Organizar → Executar → Registrar → Comunicar → Integrar → Monitorar → Governar
```

São o vocabulário de negócio estável — **não são** módulos, telas ou microsserviços.

Detalhes: [core-business-capabilities.md](../03-capabilities/core-business-capabilities.md).

---

## 2. Business Domains

As 39 sub-capabilities do Business Capability Map agrupam-se em **16 Business Domains** com fronteiras de responsabilidade claras:

| Tipo | Domínios |
|---|---|
| **Core (5)** | Care Delivery, Clinical Record, Clinical Orders, Clinical Documents, Care Monitoring |
| **Supporting (8)** | Patient Relationship, Professional Management, Organization Management, Payer & Insurance, Care Coordination, Communication, Integration, Operations Monitoring |
| **Extension (2)** | Diagnostic Operations, Home Care Operations |
| **Cross-cutting (1)** | Governance & Compliance |

**Read models:** Clinical Timeline, Analytics & Reporting.

Detalhes: [business-domains.md](../04-domain/business-domains.md), [ADR-0008](../05-architecture/adr/foundation/ADR-0008-business-domain-map.md).

---

## 3. Core Platform

Fundação estrutural mínima — **8 contratos/invariantes** sem regras de negócio nem implementação transversal (ADR-0009):

Tenant Context · Module Registry · Hierarchical Care Model Contracts · Care Journey Type System · Extension Mechanism · Event Foundation · Runtime Context · Shared Kernel Types.

Detalhes: [core-platform.md](../05-architecture/core-platform.md), [ADR-0009](../05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md).

---

## 4. Platform Services

Responsabilidades **transversais e reutilizáveis** — sem interface de usuário final — consumidas por domínios e módulos:

- **Governar:** Identity, Authorization, Audit, Configuration, Feature Flag, Observability
- **Comunicar:** Communication, Notification, Template
- **Integrar:** Integration, Webhook
- **Infraestrutura:** Storage, File, Search, Event Bus
- **Registrar:** Document Engine, Medical Form Engine

Princípio: *dividir para conquistar, compartilhar para escalar*.

Detalhes: [platform-services.md](../05-architecture/platform-services.md), [ADR-0005](../05-architecture/adr/foundation/ADR-0005-platform-services.md), [medical-form-engine.md](../05-architecture/medical-form-engine.md), [document-engine.md](../05-architecture/document-engine.md), [event-strategy.md](../05-architecture/event-strategy.md).

---

## 5. Modules

Os **módulos** implementam funcionalidades derivadas dos Business Domains (AS-005 confirmada).

**15 módulos candidatos** — cardinalidade flexível (não 1:1 com domínios):

| Tier | Módulos |
|---|---|
| Core product | Scheduling, Clinical Workspace (shell), Attendance, Documentation, Orders, Documents, Care Monitoring, Patient Portal |
| Supporting | Professional & Org Admin, Payer, Integration Admin, Operations Dashboard, Governance Admin |
| Extension | Diagnostic, Home Care |

Módulos **consomem** Platform Services por contrato — não reimplementam transversais.

Detalhes: [module-strategy.md](../05-architecture/module-strategy.md).

---

## 6. Clinical Workspace

O **Clinical Workspace** (M-02) é o **shell transversal** do profissional de saúde — compõe módulos clínicos (Attendance, Documentation, Orders, etc.) conforme especialidade, tipo de atendimento, permissões e configuração da instituição.

Centraliza dados do paciente, atendimento atual, timeline clínica, documentos e alertas — sem ser o centro conceitual da plataforma (esse papel é da **Jornada de Cuidado**).

---

## 7. Integrações

A camada de integrações conecta a plataforma a sistemas externos via **Integration Service** e **Webhook Service**:

- Laboratórios, PACS, DICOM, FHIR
- TISS e operadoras de saúde
- ERPs hospitalares
- Gateways de pagamento
- Equipamentos médicos e IoT

Toda integração preserva o desacoplamento — Comunicar (pessoas) e Integrar (sistemas) são responsabilidades distintas (ADR-0004).

---

# Jornada de Cuidado

A plataforma é orientada pela **Institution Care Journey** — o trecho do cuidado em que a instituição assume responsabilidade assistencial.

```text
Descoberta da necessidade
        ↓
Agendamento / Admissão / Encaminhamento
        ↓
Atendimento (Care Delivery)
        ↓
Registro clínico (Clinical Record, Orders, Documents)
        ↓
Comunicação e integração
        ↓
Monitoramento e acompanhamento
        ↓
Alta / encerramento da jornada
```

O **Modelo Hierárquico do Cuidado** estrutura os dados clínicos:

```text
Patient → Care Journey → Care Episode → Attendance → Clinical Event → Clinical Artifact
```

Detalhes: [care-journey-lifecycle.md](../02-business-processes/care-journey-lifecycle.md), [ADR-0007](../05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md).

---

# Modelos Operacionais

A plataforma cresce adicionando suporte a **modelos operacionais**, não "tipos de clientes":

| Modelo | Escopo |
|---|---|
| Ambulatory Care | Consultórios, clínicas, policlínicas |
| Diagnostic Care | Laboratórios, centros de imagem |
| Therapeutic Care | Psicologia, fisioterapia, nutrição… |
| Home Care | Atenção domiciliar |
| Occupational Health | Saúde ocupacional |
| Telemedicine | Atendimentos à distância |
| Hospital Care *(futuro)* | Alta complexidade |

Uma instituição pode operar múltiplos modelos simultaneamente.

Detalhes: [healthcare-operating-model.md](../02-business-processes/healthcare-operating-model.md).

---

# Características Fundamentais

## Modularidade

Novos módulos derivam de capabilities e domínios — sem modificar o Core.

## Configurabilidade

Diferenças entre clientes tratadas por **configuração** sempre que possível (ADR-0006).

## Escalabilidade

Crescimento horizontal em clientes e funcionalidades.

## Extensibilidade

Novas funcionalidades respeitam contratos da plataforma — Core pequeno e protegido (ADR-0003).

## Baixo Acoplamento

Domínios e módulos comunicam-se por contratos, APIs, eventos e Platform Services.

## Evolução Contínua

Crescimento sem reestruturação arquitetural.

## AI Ready

Preparada para inteligência artificial sobre dados estruturados, eventos e jornadas.

---

# Benefícios da Arquitetura

- Crescimento sustentável por extensão.
- Facilidade de manutenção e reutilização.
- Menor custo de evolução e integração.
- Suporte a múltiplos modelos de negócio sobre base comum.
- Redução de customização por cliente.
- Base sólida para conformidade (LGPD, auditoria).

---

# Estado e Próximos Passos

| Área | Status |
|---|---|
| Core Business Capabilities | ✅ Consolidadas |
| Business Capability Map | 🟡 Draft (39 sub-capabilities) |
| Business Domains | 🟡 Draft (16 domínios — ADR-0008) |
| Platform Services | 🟢 15 Confirmed + 2 Strong (ADR-0009/0010/0011/0012) |
| Module Strategy | ✅ ADR-0009 (15 módulos) |
| Core Platform / Extension / Read Models | ✅ ADR-0009 + docs oficiais |
| Registrar engines | ✅ Medical Form Engine + Document Engine |
| Arquitetura técnica | 🟢 Sprint 3 (AS-011 ✅) |
| Multi-Tenant Strategy | ✅ ADR-0013 |
| Backend Architecture | ✅ ADR-0014 |
| Database Architecture | ✅ ADR-0015 |
| API Strategy | ✅ ADR-0016 |
| Event Bus Technical | ✅ ADR-0017 |
| Clinical Aggregates | ✅ ADR-0018 (Q-004) |
| MVP Scope | ✅ ADR-0019 (Q-006) |

**Próximo marco:** AS-009 Platform Security (AS-017 ✅ ADR-0019).

Documentação engines: [medical-form-engine.md](../05-architecture/medical-form-engine.md), [document-engine.md](../05-architecture/document-engine.md).  
Multi-tenant: [multi-tenant-strategy.md](../05-architecture/multi-tenant-strategy.md).  
Backend: [backend-architecture.md](../05-architecture/backend-architecture.md).  
Database: [database-architecture.md](../05-architecture/database-architecture.md).  
API: [api-strategy.md](../05-architecture/api-strategy.md).  
Agregados: [clinical-aggregates.md](../05-architecture/clinical-aggregates.md).  
MVP: [mvp-scope.md](../05-architecture/mvp-scope.md).  
Documentação AS-005: [module-strategy.md](../05-architecture/module-strategy.md), [extension-model.md](../05-architecture/extension-model.md), [read-models.md](../05-architecture/read-models.md).

---

# Documentos Relacionados

| Documento | Conteúdo |
|---|---|
| [vision.md](../00-introduction/vision.md) | Visão estratégica |
| [healthcare-operating-model.md](../02-business-processes/healthcare-operating-model.md) | Modelo operacional de saúde |
| [business-capability-map.md](../03-capabilities/business-capability-map.md) | 39 sub-capabilities |
| [domain-map.md](../04-domain/domain-map.md) | Mapeamento capability → domínio |
| [platform-overview.md](../../ai-context/platform-overview.md) | Resumo para IA |
| [ARCHITECTURE_INDEX.md](../../ARCHITECTURE_INDEX.md) | Painel de controle do projeto |

---
title: Glossary
status: Draft
version: 0.2.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Introduction
phase: Product & Architecture Foundation
related:
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/extension-model.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/medical-form-engine.md
  - docs/05-architecture/document-engine.md
  - docs/05-architecture/adr/foundation/ADR-0010-medical-form-engine.md
  - docs/05-architecture/adr/foundation/ADR-0011-document-engine.md
  - docs/00-introduction/principles.md
---

# Glossary

> Vocabulário ubíquo da Health Platform — termos confirmados até AS-007 (Sprint 2).

---

## Camadas arquiteturais

| Termo | Definição |
|---|---|
| **Core Platform** | Fundação estrutural — 8 contratos/invariantes (Tenant Context, Module Registry, etc.). Não implementa PS nem regras de negócio. |
| **Platform Service (PS)** | Implementação transversal reutilizável — Identity, Audit, Communication, etc. Sem UI de produto. |
| **Business Domain** | Responsabilidade de negócio com vocabulário ubíquo — 16 confirmados (ADR-0008). |
| **Module** | Unidade funcional implementável derivada de domínio(s) — 15 candidatos (AS-005). |
| **Extension Module** | Módulo opcional por modelo operacional — M-14, M-15. |
| **Extension Mechanism** | Infraestrutura do Core para composição — hooks, Module Registry. |
| **Extension Business Domain** | Domínio para modelo operacional — Diagnostic Operations, Home Care Operations. |
| **Read Model / View** | Projeção cross-domain de leitura — Clinical Timeline, Analytics. |

**Regra:** nunca usar "Extension" sem qualificador (Domain, Module ou Mechanism).

---

## Core Platform (8 componentes)

| Termo | Definição |
|---|---|
| **Hierarchical Care Model Contracts** | Níveis Patient → Artifact (ADR-0001). |
| **Care Journey Type System** | Tipos de start event (ADR-0007). |
| **Tenant Context** | Contrato multi-tenant obrigatório (I-01). Ver Multi-Tenant Strategy (ADR-0013). |
| **Tenant** | Unidade de isolamento e contratação SaaS (cliente da plataforma) — ADR-0013. |
| **Module Registry** | Registro e descoberta de módulos. |
| **Extension Mechanism** | Contratos de hooks e composição. |
| **Event Foundation** | Contrato limitado de eventos (envelope, contexto, regras) — sem catálogo no Core (I-07). Implementação: Event Bus (ADR-0012). |
| **Runtime Context** | Agregador tenant, organization (ref), user, escopo (I-02). |
| **Shared Kernel Types** | Referências mínimas estáveis entre camadas. |

---

## Módulos (referência)

| ID | Nome | Tipo |
|---|---|---|
| M-01 | Scheduling | Core product |
| M-02 | Clinical Workspace | Shell transversal |
| M-03 | Attendance | Core product |
| M-04 | Clinical Documentation | Core product |
| M-05 | Orders | Core product |
| M-06 | Documents | Core product |
| M-07 | Care Monitoring | Core product |
| M-08 | Patient Portal | Core product |
| M-09 | Professional & Org Admin | Supporting |
| M-10 | Payer & Insurance | Supporting |
| M-11 | Integration Admin | Supporting |
| M-12 | Operations Dashboard | Supporting |
| M-14 | Diagnostic | Extension |
| M-15 | Home Care | Extension |
| M-16 | Governance Admin | Cross-cutting admin |

---

## Conceitos operacionais

| Termo | Definição |
|---|---|
| **Operational Mode** | Variante dentro de módulo core — ex.: Telemedicine em Attendance. Não é Extension Module. |
| **Clinical Workspace** | M-02 — shell que compõe módulos clínicos sem regras próprias. |
| **Clinical Timeline** | Read Model — prontuário = visão cronológica. |
| **Institution Care Journey** | Trecho do cuidado com responsabilidade da instituição (ADR-0007). |

---

## Medical Form Engine (AS-006)

| Termo | Definição |
|---|---|
| **Medical Form Engine** | Platform Service Confirmed — mecanismo de captura estruturada de formulários clínicos (ADR-0010). |
| **Form Definition** | Schema versionado — seções, campos, tipos. Owner: Engine. |
| **Form Instance** | Preenchimento em runtime. Sem ownership clínico. |
| **Clinical Content** | Conhecimento clínico após aceite do domínio destino. |

---

## Document Engine (AS-007)

| Termo | Definição |
|---|---|
| **Document Engine** | Platform Service Confirmed — geração e renderização formal de documentos clínicos (ADR-0011). |
| **Document Template** | Estrutura reutilizável de documento. Owner: Template Service. |
| **Generation Instance** | Solicitação de render com template + dados. Owner: Engine. |
| **Clinical Artifact** | Documento formal com ciclo de vida. Owner: Clinical Documents Domain. |

---

## Event Strategy (AS-010)

| Termo | Definição |
|---|---|
| **Event Bus** | Platform Service Confirmed — mensageria assíncrona **interna**; implementa Event Foundation (ADR-0012). |
| **Domain Event** | Fato de negócio publicado pelo domínio após persistir — ex.: `Attendance.Started`. |
| **Platform Event Message** | Envelope transportado pelo Event Bus — sem semântica clínica própria. |
| **Clinical Event** | Nível do modelo hierárquico (ADR-0001) — **não** é mensagem na bus por si só. |

---

## Multi-Tenant Strategy (AS-011)

| Termo | Definição |
|---|---|
| **Multi-Tenant Strategy** | Isolamento SaaS: Tenant vs Organization; shared+discriminator padrão; silo = exceção (ADR-0013). |
| **Isolamento shared** | Infra compartilhada com discriminador de tenant obrigatório. |

---

## Backend Architecture (AS-012)

| Termo | Definição |
|---|---|
| **Modular monolith** | Estilo padrão do backend — boundaries lógicos Module/Domain/PS num (ou poucos) deployable(s) (ADR-0014). |
| **Backend Architecture** | Estilo, camadas e fronteiras de execução servidor — sem framework nesta doc (ADR-0014). |

---

## Database Architecture (AS-013)

| Termo | Definição |
|---|---|
| **Database Architecture** | Persistência e isolamento: shared+tenant_id, Outbox, ownership Domain vs Storage/File (ADR-0015). |
| **Outbox** | Registro no mesmo UoW da escrita de domínio para publicar no Event Bus após commit. |

---

## Platform Services — tiers (atualizado AS-010)

| Tier | Significado |
|---|---|
| **Confirmed** | Catálogo maduro — **15** serviços (incl. Medical Form Engine, Document Engine, Event Bus) |
| **Strong Candidate** | Direção forte — **2** serviços (Search, Template Service) |
| **Needs Review** | Compliance Service (Q-019) |

Coexiste com **Consolidado/Identificado** do ADR-0005 em nível de maturidade de implementação.

---

## Regra operacional (D-002)

```text
Core define invariantes → PS operam → Domínios definem política → PS executam mecanismo
```

---
title: ADR Summary
status: Draft
version: 0.4.0
created: 2026-07-03
updated: 2026-07-14
author: Architecture Team
category: AI Context
phase: Technical Architecture
related:
  - ARCHITECTURE_INDEX.md
  - ai-context/architecture-foundation.md
  - ai-context/open-questions.md
---

# ADR Summary

> Resumo executivo dos **Architecture Decision Records (ADR)** da série Foundation — para contexto rápido de IA e arquitetos.

Série oficial: `docs/05-architecture/adr/foundation/`. ADRs **0001–0019** Accepted.

---

## Foundation ADRs (0001–0019)

| ADR | Título | Decisão central |
|---|---|---|
| **0001** | Hierarchical Care Model | Modelo clínico em 6 níveis: Patient → Care Journey → Care Episode → Attendance → Clinical Event → Clinical Artifact |
| **0002** | Capability-Driven Architecture | Plataforma organizada por 8 Core Business Capabilities; módulos derivam de capabilities e domínios |
| **0003** | Core Protection and Extension Model | Core pequeno e protegido; crescimento por extensão (módulos, PS, configuração, eventos, integrações) |
| **0004** | Communication and Integration Separation | Comunicar = pessoas; Integrar = sistemas; fronteiras distintas e não intercambiáveis |
| **0005** | Platform Services | Catálogo de 17 PS; transversais por contratos estáveis |
| **0006** | Configuration over Customization | Variabilidade por configuração; customização por código é exceção governada |
| **0007** | Care Journey Lifecycle | Institution Care Journey inicia com Care Journey Start Event |
| **0008** | Business Domain Map | 16 Business Domains; critérios Domain / PS / Read Model |
| **0009** | Core Platform Boundary | Core Platform (8 contratos); 15 módulos; tiers PS; extension; classification — **Q-007 Answered** |
| **0010** | Medical Form Engine | PS Confirmed — captura estruturada — **Q-013** |
| **0011** | Document Engine | PS Confirmed — geração formal — **Q-013**, **Q-014** |
| **0012** | Event Strategy | Event Foundation (Core) + Event Bus (PS Confirmed) + taxonomia 3 camadas — **Q-003** (conceitual) |
| **0013** | Multi-Tenant Strategy | Tenant = isolamento SaaS; shared+discriminator padrão; Org no domínio — **Q-008** |
| **0014** | Backend Architecture | Modular monolith padrão; camadas API→App→Domain→Infra; Module ≠ deploy |
| **0015** | Database Architecture | Shared DB + tenant_id; Outbox; Domain vs Storage/File; vendor por critérios |
| **0016** | API Strategy | Sync ≠ event; Product/Platform/Integration; REST-like; `/v{n}`; Tenant+Correlation |
| **0017** | Event Bus Technical | At-least-once; ordering por stream; Outbox→broker; critérios; produto Deferred |
| **0018** | Clinical Aggregates | Roots clínicos; Clinical Event ≠ Domain Event; Q-004 |
| **0019** | MVP Scope | Must/Should/Later/Out; Journey ambulatorial+tele; Q-006 |

---

## Sequência de derivação

```text
Healthcare Operating Model
        ↓
Core Business Capabilities (ADR-0002)
        ↓
Business Capability Map (39 sub-capabilities)
        ↓
Business Domains (ADR-0008 — 16 domínios)
        ↓
Core Platform (ADR-0009 — 8 contratos)
        ↓
Platform Services (ADR-0005 / tiers ADR-0009 / 0010 / 0011 / 0012)
        ↓
Modules (ADR-0009 — 15 candidatos)
        ↓
Event Strategy (ADR-0012)
        ↓
Multi-Tenant Strategy (ADR-0013)
        ↓
Backend Architecture (ADR-0014)
        ↓
Database Architecture (ADR-0015)
        ↓
API Strategy (ADR-0016)
        ↓
Event Bus Technical (ADR-0017)
        ↓
Clinical Aggregates (ADR-0018)
        ↓
MVP Scope (ADR-0019)
```

---

## Decisões que não reabrir sem novo ADR

- Jornada de Cuidado como centro conceitual (não prontuário).
- Modelo Hierárquico do Cuidado (ADR-0001).
- 8 Core Business Capabilities como vocabulário estável.
- Separação Comunicar / Integrar (ADR-0004).
- Core Platform = contratos; PS = implementações (ADR-0009).
- 15 módulos — cardinalidade flexível (ADR-0009).
- Clinical Workspace = M-02 shell (ADR-0009).
- Configuração acima de customização (ADR-0006).
- 16 Business Domains do catálogo ADR-0008.
- Event Foundation (Core) vs Event Bus (PS); tipos no domínio publicador (ADR-0012).
- Tenant = isolamento SaaS; shared+discriminator padrão; Org no domínio (ADR-0013).
- Modular monolith padrão; Module ≠ deploy; camadas API→App→Domain→Infra (ADR-0014).
- Shared DB + tenant_id; Outbox; Domain≠blobs; schema-per-tenant não padrão (ADR-0015).
- Sync ≠ Domain Event; superfícies Product/Platform/Integration; REST-like + `/v{n}` (ADR-0016).
- At-least-once; ordering por stream; Outbox→broker; critérios de broker (ADR-0017).
- Aggregate roots clínicos; Clinical Event (A) ≠ Domain Event (B) (ADR-0018).
- MVP Must/Should/Later/Out; Journey ambulatorial+tele; Billing Out (ADR-0019).

---

## Open Questions por ADR

| ADR | Perguntas relacionadas |
|---|---|
| 0003 | Q-009 (parcial) |
| 0004 | Q-010, Q-011 |
| 0005 | Q-019 |
| 0006 | Q-015, Q-016, Q-017 |
| 0007 | Q-018 |
| 0008 | Q-005 (parcial), Q-020 (deferred) |
| 0009 | Q-019 (Compliance), OQ-C03 |
| 0010 | Q-013 *(MFE)*, OQ-PS05 |
| 0011 | Q-013 *(DE)*, Q-014, OQ-PS06 |
| 0012 | Q-003 *(conceitual Answered)* |
| 0013 | Q-008 *(Answered)*; vendor Deferred (critérios em ADR-0015) |
| 0014 | Stack/framework Deferred |
| 0015 | Vendor produto Deferred; Q-017; migrations |
| 0016 | OpenAPI / BFF Deferred |
| 0017 | Q-003 *(tech Answered)*; produto broker Deferred |
| 0018 | Q-004 *(Answered)*; DDL ClinicalEntry Deferred |
| 0019 | Q-006 *(Answered)*; Q-005 permanece Open |

## ADRs planejados

| Tema | Sessão |
|---|---|
| Platform Security | AS-009 |
| Telemedicine | AS-008 |

---

## Uso por IA

1. Consultar este resumo antes de propor arquitetura ou código.
2. Verificar `open-questions.md` para o que ainda não foi decidido.
3. Usar `architecture-classification.md` para classificar novas responsabilidades.
4. Não contradizer ADRs Accepted sem propor novo ADR.

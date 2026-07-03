---
title: ADR-0008 - Business Domain Map
status: Accepted
date: 2026-07-02
deciders:
  - Architecture Team
tags:
  - architecture
  - business-domains
  - domain-map
  - foundation
  - capability-driven
related:
  - architecture-sessions/AS-004-business-domain-map.md
  - docs/04-domain/business-domains.md
  - docs/04-domain/domain-map.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0007-care-journey-lifecycle.md
  - workspace/AS-004/confirmation-package.md
---

# ADR-0008 — Business Domain Map

## Status

Accepted

---

# Contexto

A Health Platform organiza-se por **Core Business Capabilities** (ADR-0002). O Business Capability Map (AS-002) define 39 sub-capabilities. A **AS-004** investigou **Q-002**: como agrupar sub-capabilities em **Business Domains** com fronteiras coerentes.

A AS-003 (ADR-0007) definiu o ciclo de vida da Institution Care Journey — pré-requisito para ancorar domínios centrais.

---

# Problema

Sem mapa formal de domínios:

- Sub-capabilities não têm ownership de negócio claro.
- Platform Services podem ser confundidos com domínios.
- Módulos e bounded contexts seriam derivados de forma inconsistente.
- Module Strategy, APIs e organização de equipes ficam bloqueadas.

---

# Decisão

## 1. Modelo de decomposição

Business Domains emergem do **agrupamento de sub-capabilities relacionadas**, combinado com **responsabilidades transversais via Platform Services** (ADR-0005).

**Rejeitado:** 8 domínios = 8 capabilities · ~35 domínios (1:1) · domínios por modelo operacional · domínios por tela/módulo · mega domínios (Clinical, Relationship, Monitoring, Execution).

## 2. Domain vs Platform Service vs Read Model

| Pergunta | Destino |
|---|---|
| Regras de negócio e vocabulário ubíquo próprios? | Business Domain |
| Responsabilidade transversal, reutilizável e técnica? | Platform Service |
| Agregação de leitura cross-domain? | Read Model / View |

Domínios **consomem** PS — não reimplementam transversais.

## 3. Catálogo — 16 Business Domains

### Core (5)

Care Delivery · Clinical Record · Clinical Orders · Clinical Documents · Care Monitoring

### Supporting (8)

Patient Relationship · Professional Management · Organization Management · Payer & Insurance · Care Coordination · Communication · Integration · Operations Monitoring

### Extension (2)

Diagnostic Operations · Home Care Operations

### Cross-cutting (1)

Governance & Compliance

## 4. Read models (não domínios)

- **Clinical Timeline** — projeção cronológica; prontuário = visão
- **Analytics & Reporting** — agregação de indicadores

## 5. Decisões por capability

| Capability | Decisão |
|---|---|
| Relacionar | 4 domínios de vínculo (1:1) |
| Organizar | Care Coordination único |
| Executar | Care Delivery + Diagnostic Operations + Home Care Operations; Hospital deferred |
| Registrar | Clinical Record + Clinical Orders + Clinical Documents |
| Comunicar | Communication Domain + PS entrega |
| Integrar | Integration Domain; financeiro deferred |
| Monitorar | Care Monitoring + Operations Monitoring; Analytics = read model |
| Governar | Governance & Compliance; demais → PS |

## 6. Regras de fronteira

**Registrar:** Record = conhecimento · Orders = intenções · Documents = artefatos formais · Timeline = read model.

**Monitorar:** Care Monitoring = clínico · Operations Monitoring = SLA operacional · Analytics = read model.

**Governança:** políticas no domínio; enforcement nos PS.

**Consentimentos:** Communication Consent (Communication) · Data Processing Consent (Governance) · Clinical Consent (Delivery + Documents) · Research Consent deferred.

## 7. Deferred

Hospital Operations · Billing & Financial · Financial & ERP Integration · Intelligence & Automation · Research Consent domain

---

# Consequências

## Positivas

- Q-002 respondida com catálogo estável de 16 domínios.
- Fronteiras validadas por capability e cenários cross-domain.
- Base para Module Strategy, bounded contexts e Q-004 (agregados).

## Negativas / trade-offs

- Open Questions residuais (Q-005, Q-010, P-REG-*, P-MON-02, etc.) exigem refinamento em sessões futuras.
- Dois read models exigem estratégia de projeção (Search Service hipótese).

---

# Documentação oficial derivada

| Documento | Relação |
|---|---|
| `docs/04-domain/business-domains.md` | Catálogo e responsabilidades |
| `docs/04-domain/domain-map.md` | Mapeamento sub-capability → domínio |

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| ADR-0001 | Domínios Core ancoram Attendance e ciclo clínico |
| ADR-0002 | Esta decisão implementa a derivação capability → domínio |
| ADR-0003 | Extension domains (Diagnostic, Home Care) |
| ADR-0004 | Communication e Integration como domínios distintos |
| ADR-0005 | PS não substituem domínios de negócio |
| ADR-0007 | Patient Relationship ≠ início de jornada |

---

# Open Questions

| ID | Situação pós-decisão |
|---|---|
| Q-002 | **Answered** por este ADR |
| Q-004 | Deferred — agregados após domínios |
| Q-005 | Open — Billing deferred |
| Q-007 | **Answered** — ADR-0009 (fronteira Core/PS/Modules/Extension/Read Models) |
| Q-010 | Open — Communication vs Notification |
| Q-019 | Open — Compliance Service no catálogo PS |

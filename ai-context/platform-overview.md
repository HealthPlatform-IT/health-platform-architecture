---
title: Platform Overview
status: Draft
version: 0.2.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: AI Context
phase: Product & Architecture Foundation
related:
  - docs/00-introduction/vision.md
  - docs/01-product/product-overview.md
  - ai-context/architecture-foundation.md
  - ai-context/adr-summary.md
---

# Platform Overview

> Visão consolidada da Health Platform para contexto rápido — o que é, como se organiza e o que ainda está em aberto.

---

## O que é

A **Health Platform** é uma plataforma SaaS multi-tenant para instituições de saúde — de consultórios a redes de clínicas. Não é um prontuário isolado nem um ERP adaptado: é uma **infraestrutura operacional de saúde** orientada por modelos operacionais e jornadas de cuidado.

**Proposta central:** qualquer instituição de saúde deve conseguir mapear seus processos na plataforma, com variabilidade por configuração — não por customização por cliente.

---

## Centro conceitual

| Conceito | Papel |
|---|---|
| **Institution Care Journey** | Trecho do cuidado em que a instituição assume responsabilidade na plataforma |
| **Modelo Hierárquico do Cuidado** | Patient → Care Journey → Care Episode → Attendance → Clinical Event → Clinical Artifact |
| **Core Business Capabilities** | 8 competências permanentes: Relacionar, Organizar, Executar, Registrar, Comunicar, Integrar, Monitorar, Governar |

O prontuário é **visão** da jornada — não o centro da plataforma.

---

## Organização arquitetural

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
Platform Services (tiers ADR-0009)
        ↓
Modules (15 candidatos — ADR-0009)
        ↓
Features → Components → Screens
```

---

## 16 Business Domains

| Tipo | Domínios |
|---|---|
| Core (5) | Care Delivery, Clinical Record, Clinical Orders, Clinical Documents, Care Monitoring |
| Supporting (8) | Patient Relationship, Professional Management, Organization Management, Payer & Insurance, Care Coordination, Communication, Integration, Operations Monitoring |
| Extension (2) | Diagnostic Operations, Home Care Operations |
| Cross-cutting (1) | Governance & Compliance |

**Read models:** Clinical Timeline, Analytics & Reporting.

**Deferred:** Hospital Operations, Billing & Financial.

---

## Princípios fundamentais

1. **Capabilities antes de módulos** — nunca criar módulo só por tela.
2. **Core pequeno e protegido** — regras de cliente ficam fora do Core.
3. **Crescimento por extensão** — módulos, PS, configuração, eventos.
4. **Configuração acima de customização** — variabilidade por config.
5. **Comunicar ≠ Integrar** — pessoas vs sistemas.
6. **Platform Services** — transversais reutilizáveis, sem UI.
7. **Documentação antes de implementação** — arquitetura antes de código.

---

## Modelos operacionais suportados (evolução)

Ambulatory Care · Diagnostic Care · Therapeutic Care · Home Care · Occupational Health · Telemedicine · Hospital Care *(futuro)*

Uma instituição pode operar múltiplos modelos simultaneamente.

---

## Estado atual (Sprint 2 — AS-007 concluída)

| Área | Status |
|---|---|
| ADRs Foundation | 15 Accepted (0001–0015) |
| Architecture Sessions | AS-001 a AS-007 concluídas |
| Core Platform | 8 componentes (ADR-0009) |
| Module Strategy | 15 módulos (ADR-0009) |
| Platform Services | 15 Confirmed + 2 Strong (ADR-0009/0010/0011/0012) |
| Registrar engines | Medical Form Engine + Document Engine formalizados |
| Development Guidelines | Draft v0.3.0 |
| Event Model | 🟢 ADR-0012 (conceitual); broker Deferred Sprint 3 |

---

## Perguntas críticas respondidas

| ID | Pergunta | Resposta |
|---|---|---|
| Q-001 | Quando a instituição assume o cuidado? | Care Journey Start Event (ADR-0007) |
| Q-002 | Como agrupar em Business Domains? | 16 domínios (ADR-0008) |
| Q-007 | Fronteira Core / PS / Modules? | **Answered** — ADR-0009 (8 contratos Core + 15 módulos) |
| Q-013 | Escopo Medical Form Engine e Document Engine? | **Answered** — ADR-0010 + ADR-0011 |
| Q-014 | Template Service independente? | **Answered** — ADR-0011 D-005 |
| Q-008 | Estratégia multi-tenant? | **Answered** — ADR-0013 |

---

## Próximo marco

Sprint 3 — API Strategy · AS-009 Security (AS-013 ✅ ADR-0015).

---

## Documentos de referência

| Documento | Uso |
|---|---|
| [architecture-foundation.md](architecture-foundation.md) | Contexto completo para IA |
| [adr-summary.md](adr-summary.md) | Resumo dos ADRs |
| [open-questions.md](open-questions.md) | O que ainda não foi decidido |
| [product-overview.md](../docs/01-product/product-overview.md) | Visão de produto |
| [ARCHITECTURE_INDEX.md](../ARCHITECTURE_INDEX.md) | Painel de controle do projeto |

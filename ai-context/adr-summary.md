---
title: ADR Summary
status: Draft
version: 0.2.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: AI Context
phase: Product & Architecture Foundation
related:
  - ARCHITECTURE_INDEX.md
  - ai-context/architecture-foundation.md
  - ai-context/open-questions.md
---

# ADR Summary

> Resumo executivo dos **Architecture Decision Records (ADR)** da série Foundation — para contexto rápido de IA e arquitetos.

Série oficial: `docs/05-architecture/adr/foundation/`. ADRs **0001–0009** Accepted.

---

## Foundation ADRs (0001–0009)

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
Platform Services (ADR-0005 / tiers ADR-0009)
        ↓
Modules (ADR-0009 — 15 candidatos)
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

---

## Open Questions por ADR

| ADR | Perguntas relacionadas |
|---|---|
| 0003 | Q-008, Q-009 (parcial) |
| 0004 | Q-010, Q-011 |
| 0005 | Q-003, Q-013, Q-014, Q-019 |
| 0006 | Q-015, Q-016, Q-017 |
| 0007 | Q-018 |
| 0008 | Q-005 (parcial), Q-020 (deferred) |
| 0009 | Q-019 (Compliance), OQ-C01, OQ-C03 |

Detalhes: [open-questions.md](open-questions.md).

---

## ADRs planejados

| Tema | Sessão |
|---|---|
| Medical Form Engine | AS-006 |
| Document Engine | AS-007 |
| Event Strategy | AS-010 |

---

## Uso por IA

1. Consultar este resumo antes de propor arquitetura ou código.
2. Verificar `open-questions.md` para o que ainda não foi decidido.
3. Usar `architecture-classification.md` para classificar novas responsabilidades.
4. Não contradizer ADRs Accepted sem propor novo ADR.

---
title: ADR-0019 - MVP Scope by Sub-Capability
status: Accepted
date: 2026-07-14
deciders:
  - Architecture Team
tags:
  - architecture
  - MVP
  - Q-006
  - foundation
  - sprint-3
related:
  - architecture-sessions/AS-017-mvp-scope.md
  - docs/05-architecture/mvp-scope.md
  - docs/03-capabilities/business-capability-map.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - workspace/AS-017/confirmation-package.md
---

# ADR-0019 — MVP Scope by Sub-Capability

## Status

Accepted

---

# Contexto

O mapa de capabilities (39 sub-capabilities) não distinguia MVP. **Q-006** pedia o recorte. A **AS-017** confirmou D-001 a D-010 em 2026-07-14 (*"confirmo o pacote"*). Billing permanece em **Q-005** (fora).

---

# Problema

Sem MVP:

- Backlog e cartões “Pronto p/ dev” sem recorte
- Risco de escopo = plataforma inteira
- Risco de cortar o centro Care Journey

---

# Decisão

## 1. Natureza (D-001)

**MVP** = escopo **arquitetural / produto v1** implementável — **não** roadmap comercial completo.

## 2. Tiers (D-002)

**Must** · **Should** · **Later** · **Out** — critérios em `mvp-scope.md`.

## 3. Centro obrigatório (D-003 / D-006)

MVP-Must preserva **Institution Care Journey** ambulatorial com **Telemedicine** como Operational Mode em Attendance (ADR-0009).

Loop Must: Paciente/Profissional/Org → Scheduling → Attendance → Documentation / Orders / Artifacts + Clinical Timeline.

## 4. Módulos (D-004 / D-005)

| Tier | Módulos |
|---|---|
| **Must** | M-01, M-02, M-03, M-04, M-05, M-06, M-09 |
| **Should** | M-07, M-08, M-11, M-12, M-16 |
| **Later** | M-10, M-14, M-15 |

## 5. Fora do MVP (D-005 / D-009)

- **Later:** Diagnostic, Home Care, payer profundo, FHIR pleno, analytics profundo
- **Out:** Billing/estoque/financeiro profundo (Q-005); Hospital Care Delivery (Future)

## 6. Platform Services Must (D-007)

Identity · Authorization · Audit · Configuration · Feature Flag · Event Bus · Storage · File · Observability · Medical Form Engine · Document Engine (+ Template Service se necessário aos engines).

## 7. Board (D-008)

Itens Later/Out **não** sobem para Pronto p/ desenvolvimento sem novo recorte; documentação permanente do mapa **não** é apagada.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| MVP = todos os 15 módulos | Inviável |
| MVP só Appointment sem Journey | Viola centro conceitual |
| Billing no MVP | Q-005 aberto; Out explícito |

---

# Consequências

## Positivas

- **Q-006 Answered**
- Priorização clara do board e da implementação
- Care Journey preservada

## Negativas / deferred

- Should ainda exige priorização fina
- AuthZ detalhada na AS-009
- Q-005 permanece Open

---

# Referências

- `docs/05-architecture/mvp-scope.md`
- `architecture-sessions/AS-017-mvp-scope.md`

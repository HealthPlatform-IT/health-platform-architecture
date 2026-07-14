---
title: Frontend Architecture
status: Draft
version: 0.1.0
created: 2026-07-14
updated: 2026-07-14
author: Architecture Team
category: Architecture
phase: Technical Architecture
related:
  - docs/05-architecture/adr/foundation/ADR-0021-frontend-architecture.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/api-strategy.md
  - docs/05-architecture/platform-security.md
  - docs/05-architecture/core-platform.md
  - architecture-sessions/AS-018-frontend-architecture.md
---

# Frontend Architecture

> Superfícies de aplicativo, shell Clinical Workspace, composição UI, BFF e Auth UX.

Decisão formal: [ADR-0021](adr/foundation/ADR-0021-frontend-architecture.md) · AS-018 · **OQ-C03 Answered**.

---

## 1. Purpose

Definir como a UI se organiza sobre módulos e APIs — **sem** escolher framework.

---

## 2. Fronteira

| Pertence | Não pertence |
|---|---|
| Apps Staff / Portal / Admin | Regra de domínio / AuthZ real |
| Shell M-02 e contributions | UI em Platform Services |
| Consumo Product API (± BFF) | Design system produto |
| Auth UX | Framework obrigatório |

---

## 3. Superfícies

| Superfície | Host / módulos |
|---|---|
| Staff App | M-02 shell + M-01, M-03…M-07 (+ extensions) |
| Patient Portal | M-08 |
| Admin | M-09, M-16, M-11… |

---

## 4. Clinical Workspace (shell)

```text
Staff App → M-02 Clinical Workspace
              ├── Layout / navegação / contexto de trabalho
              ├── Contributions habilitadas (Module Registry)
              └── Consome Clinical Timeline (read model)
```

Shell **não** possui regra clínica. Telemedicine = mode em Attendance (M-03).

---

## 5. Composição (OQ-C03)

| Elemento | Onde |
|---|---|
| Module contribution (rota/slot) | Contrato via Module Registry (Core) |
| Host | M-02 |
| Enablement | Feature Flag + Module Registry por tenant |

Core **não** embute componentes de UI.

---

## 6. API e BFF

- **Product API** = canônica (ADR-0016)
- **Thin BFF** = opcional (agregação Staff)
- GraphQL = Deferred

---

## 7. Auth UX

Login/sessão no cliente → Identity. Toda operação sensível → AuthZ no backend (ADR-0020). Hide/show ≠ autorização.

---

## 8. Cliente técnico

SPA web padrão. Sem microfrontends obrigatórios. Mobile Deferred.

---

## 9. Estado no cliente

Shell = referências de contexto. Módulo = estado de tela. SoT = backend/domínio.

---

## 10. Deferred

Framework · design system · GraphQL · mobile · IdP/storage token detalhado.

---

## 11. Referências

[ADR-0021](adr/foundation/ADR-0021-frontend-architecture.md) · [module-strategy.md](module-strategy.md) · [api-strategy.md](api-strategy.md) · [platform-security.md](platform-security.md)

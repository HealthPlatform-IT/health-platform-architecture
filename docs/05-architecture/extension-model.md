---
title: Extension Model
status: Draft
version: 0.1.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Architecture
phase: Product & Architecture Foundation
related:
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - architecture-sessions/AS-005-core-platform-boundary.md
---

# Extension Model

> Modelo oficial de **extensão** na Health Platform — três conceitos distintos e regras de habilitação.

Decisão formal: AS-005 D-005 · ADR-0009.

---

## 1. Purpose

Definir como extensões operam **sem** contaminar o Core Platform, confundir extensão com customização (ADR-0006) ou misturar Extension Domain, Extension Module e Extension Mechanism.

**Fora de escopo:** implementação técnica de hooks, APIs ou deploy (Q-009).

---

## 2. Três conceitos (H-EXT-001)

| Termo | O que é | Onde vive | Exemplos |
|---|---|---|---|
| **Extension Business Domain** | Domínio para modelo operacional específico | Camada de domínio (ADR-0008) | Diagnostic Operations, Home Care Operations |
| **Extension Module** | Módulo opcional que implementa Extension Domain | Camada de módulo | M-14 Diagnostic, M-15 Home Care |
| **Extension Mechanism** | Infraestrutura do Core para adicionar capacidade | Core Platform (D-001) | Module Registry, extension hooks |

**Regra:** usar qualificador — nunca "Extension" sozinho.

```text
Extension Mechanism (Core)  →  habilita registro e composição
Extension Domain (negócio)  →  define regras do modelo operacional
Extension Module (produto)  →  implementa o domínio para o usuário
```

---

## 3. Hierarquia de variabilidade (ADR-0006)

```text
1. Configuração              ← preferir sempre
2. Composição de módulos     ← habilitar/desabilitar módulos
3. Extensão                  ← Extension Modules + Extension Domains
4. Customização por código   ← exceção governada (Q-016)
5. Alteração do Core         ← proibida sem ADR
```

Extensão = níveis **2 e 3** — reutilizável para tenants que habilitam o modelo operacional.

---

## 4. Extension Domains e Modules

| Domínio Extension | Módulo | Modelo operacional |
|---|---|---|
| Diagnostic Operations | M-14 Diagnostic | Diagnostic Care |
| Home Care Operations | M-15 Home Care | Home Care |

**Deferred:** Hospital Operations · Billing & Financial (Q-005).

Novo Extension Domain exige sessão + ADR — inventário fechado (ADR-0008).

---

## 5. Extension vs Core product module

| Critério | Core product | Extension |
|---|---|---|
| Necessário para ambulatório básico? | Sim | Não |
| Desabilitável sem quebrar fundação? | Não *(operação mínima)* | Sim |
| Ligado a modelo operacional? | Não | Sim |

**Supporting modules** (M-09 a M-12, M-16) são composição (nível 2), não Extension Modules.

---

## 6. Operational Mode vs Extension Module

| Conceito | Exemplo |
|---|---|
| **Operational Mode** | Telemedicine em M-03 Attendance — Configuration + Feature Flag |
| **Extension Module** | M-14 Diagnostic — Extension Domain separado |

Telemedicine **não** é Extension Module — altera *como* Care Delivery executa, não adiciona domínio.

---

## 7. Habilitação (conceitual)

```text
Instituição → Configuration Service → Feature Flag (opcional)
         → Module Registry (Core) → Clinical Workspace compõe módulos
```

| Mecanismo | Camada |
|---|---|
| Configuration Service | PS |
| Feature Flag Service | PS |
| Module Registry | Core |
| Extension Mechanism | Core |
| Authorization Service | PS |

---

## 8. Composição com Clinical Workspace

```text
M-02 Clinical Workspace (shell)
├── M-03 a M-07 (core product, se habilitados)
└── M-14, M-15 (extension, se habilitados)
```

Extension Modules **não** alteram core product modules — apenas adicionam capacidade via Registry e Event Foundation.

---

## 9. O que extensão NÃO é

| Não é | É |
|---|---|
| Código por tenant | Customização (nível 4) |
| Alterar Core por cliente | Violação ADR-0003 |
| Read Model / PS | Projeção ou transversal |
| Telemedicine módulo | Operational Mode |

---

## 10. Open Questions

| ID | Assunto |
|---|---|
| Q-009 | Implementação técnica do Extension Mechanism |
| OQ-C03 | UI composition contract — **Answered** ADR-0021 (Module Registry contributions; host M-02) |
| Q-005 | Billing como Extension futuro |

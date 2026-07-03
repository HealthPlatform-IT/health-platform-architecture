---
title: Architecture Classification
status: Draft
version: 0.1.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Architecture
phase: Product & Architecture Foundation
related:
  - docs/05-architecture/core-platform.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/extension-model.md
  - docs/05-architecture/read-models.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
---

# Architecture Classification

> Árvore de decisão para classificar responsabilidades nas **seis categorias** arquiteturais da Health Platform.

Decisão formal: AS-005 D-008 · ADR-0009. Estende AS-004 D-002 (Domain / PS / View).

---

## 1. Seis categorias

| # | Categoria | Pergunta resumida |
|---|---|---|
| 1 | **Core Platform** | Contrato/invariante estrutural do SaaS multi-tenant? |
| 2 | **Platform Service** | Transversal, reutilizável, sem vocabulário de domínio? |
| 3 | **Business Domain** | Regras de negócio e vocabulário ubíquo? |
| 4 | **Read Model / View** | Projeção cross-domain sem regras autônomas? |
| 5 | **Module** | Funcionalidade ao usuário derivada de domínio(s)? |
| 6 | **Extension** | Módulo opcional por modelo operacional? |

---

## 2. Árvore de decisão

```text
START: Responsabilidade R

1. Contrato/invariante estrutural (não implementação operacional)?
   SIM → Core Platform

2. Transversal, ≥2 consumidores, sem vocabulário de domínio, sem UI?
   SIM → Platform Service

3. Regras de negócio e vocabulário ubíquo?
   SIM → Business Domain (novo domínio → sessão + ADR)

4. Projeção de leitura cross-domain sem regras autônomas?
   SIM → Read Model / View

5. Funcionalidade ao usuário ou shell de orquestração?
   SIM → passo 6
   NÃO → Open Question

6. Opcional por modelo operacional, desabilitável sem quebrar clínica básica?
   SIM → Extension Module
   NÃO → Module (core product ou supporting)
```

---

## 3. Quick reference

```text
Contrato estrutural?     → Core Platform
Transversal sem domínio? → Platform Service
Regras + vocabulário?    → Business Domain
Só leitura agregada?     → Read Model
Funcionalidade ao user?  → Module
Opcional por modelo op.? → Extension
```

---

## 4. Invariantes Core (I-01 a I-10)

| # | Invariante |
|---|---|
| I-01 | Tenant Context obrigatório |
| I-02 | Runtime Context propagado |
| I-03 | Hierarchical Care Model respeitado |
| I-04 | Care Journey types — PS não inventam tipos |
| I-05 | Module Registry — Read Models não registrados |
| I-06 | Extension Mechanism — Core imutável sem ADR |
| I-07 | Event Foundation — sem catálogo de eventos no Core |
| I-08 | Shared Kernel Types — referências, não modelos internos |
| I-09 | Auditabilidade via Audit Service |
| I-10 | Autorização obrigatória para dados sensíveis |

Detalhe: [core-platform.md](core-platform.md).

---

## 5. Exemplos validados

| Responsabilidade | Classificação |
|---|---|
| Tenant context contract | Core Platform |
| Login de usuário | Platform Service (Identity) |
| Ciclo de vida de prescrição | Business Domain (Clinical Orders) |
| Prontuário cronológico | Read Model (Clinical Timeline) |
| Tela de agenda | Module (M-01 Scheduling) |
| Módulo de laboratório | Extension (M-14) |
| Telemedicine | Operational Mode em M-03 — **não** Extension |
| Identity no Core | **Rejeitado** → PS |
| Timeline como módulo | **Rejeitado** → Read Model |
| Communication Module | **Rejeitado** → orquestrado por consumidores |

---

## 6. Anti-patterns

| Anti-pattern | Classificação correta |
|---|---|
| Timeline como domínio | Read Model |
| Identity no Core | Platform Service |
| Extension = customização por cliente | Customização ADR-0006 nível 4 |
| PS com regra clínica | Business Domain |
| Analytics como domínio (fase atual) | Read Model |

---

## 7. Testes de validação

| Teste | Passa se… |
|---|---|
| Core | Estrutura válida sem módulos habilitados |
| PS | ≥2 módulos precisam sem duplicar |
| Domain | Especialista usa vocabulário ubíquo |
| Module | Usuário interage como funcionalidade |
| Extension | Desligar por tenant sem quebrar clínica básica |
| View | Apenas leitura agregada de domínios fonte |

---

## 8. Zonas cinzentas resolvidas (AS-005)

| Zona | Resolução |
|---|---|
| Core amplo vs PS | Core = contratos; PS = implementação |
| Clinical Workspace | M-02 shell; slot mínimo no Core (OQ-C03) |
| Timeline vs Record | Record produz; Timeline projeta |
| Extension vs Configuration | Extension = módulo novo; Config = parâmetros |
| Compliance | Governance = domínio; Compliance PS = enforcement (Q-019) |

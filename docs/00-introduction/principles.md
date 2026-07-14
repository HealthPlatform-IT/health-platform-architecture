---
title: Principles
status: Draft
version: 0.1.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Introduction
phase: Product & Architecture Foundation
related:
  - docs/00-introduction/vision.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md
  - docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md
  - ARCHITECTURE_INDEX.md
---

# Principles

> Princípios arquiteturais e de produto da Health Platform — consolidados a partir de decisões já formalizadas (ADRs Foundation 0001–0016).

Este documento **não cria novas decisões**. Para detalhes formais, consultar os ADRs referenciados.

---

## 1. Orientação por jornada e modelo operacional

A plataforma modela a **operação de instituições de saúde** a partir de modelos operacionais e da **Institution Care Journey** — não a partir de telas ou módulos isolados.

O prontuário é **visão** da jornada; o centro conceitual é o cuidado sob responsabilidade da instituição (ADR-0001, ADR-0007).

---

## 2. Capabilities antes de módulos

A organização parte das **8 Core Business Capabilities** e do Business Capability Map — módulos, domínios e features **derivam** de capabilities, nunca o contrário (ADR-0002).

---

## 3. Core pequeno e protegido

O **Core Platform** contém apenas contratos, invariantes e mecanismos de extensão (8 componentes — ADR-0009). Regras de negócio, transversais operacionais e variabilidade por cliente **não pertencem ao Core** (ADR-0003).

---

## 4. Crescimento por extensão

Evolução da plataforma por **módulos**, **Platform Services**, **configuração**, **eventos** e **integrações** — sem alterar o núcleo estrutural (ADR-0003).

Mecanismos: Module Registry, Extension Mechanism, Feature Flags, Configuration Service.

---

## 5. Configuração acima de customização

Variabilidade entre instituições resolve-se preferencialmente por **configuração**. Hierarquia ADR-0006:

```text
1. Configuração
2. Composição de módulos
3. Extensão (módulos/domínios opcionais)
4. Customização por código (exceção governada)
5. Alteração do Core (último recurso — exige ADR)
```

---

## 6. Separação clara de responsabilidades

Cada camada tem papel definido: Core → Platform Services → Business Domains → Modules → Features.

**Comunicar ≠ Integrar** — comunicação com pessoas; integração com sistemas (ADR-0004).

**Domínio define política; Platform Service executa mecanismo** (ADR-0005, ADR-0009 D-002).

---

## 7. Platform Services para transversais

Responsabilidades reutilizáveis entre módulos (Identity, Audit, Communication, engines Registrar, etc.) são **Platform Services** com contratos estáveis — não duplicadas em módulos (ADR-0005).

Tiers atuais: 15 Confirmed + 2 Strong + 1 Needs Review (`platform-services.md` v0.5.0).

---

## 8. Baixo acoplamento entre domínios

Os 16 Business Domains possuem fronteiras e vocabulário próprios (ADR-0008). Módulos **não acessam estado interno** de outros módulos — consomem PS ou Event Foundation. Domínio publica Domain Events; Event Bus transporta internamente (ADR-0012).

---

## 9. Documentação antes da implementação

Toda decisão arquitetural segue o fluxo:

```text
Architecture Session → Workspace Draft → Decision → ADR → Documentação Oficial → AI Context → Implementação
```

Nenhuma implementação de stack técnica antes da fase correspondente (Sprint 3 para backend/frontend/banco).

---

## 10. Fluxo de decisão arquitetural

Conforme `ARCHITECTURE_INDEX.md` — toda evolução preserva:

- Orientação por Jornada de Cuidado e modelos operacionais.
- Organização por Core Business Capabilities.
- Separação clara de responsabilidades.
- Baixo acoplamento entre domínios.
- Configuração acima de customização.
- Core pequeno e protegido.
- Platform Services para responsabilidades transversais.
- Documentação antes da implementação.

---

## Documentos relacionados

| Documento | Conteúdo |
|---|---|
| [vision.md](vision.md) | Visão e missão |
| [glossary.md](glossary.md) | Vocabulário ubíquo |
| [architecture-foundation.md](../../ai-context/architecture-foundation.md) | Contexto consolidado para IA |

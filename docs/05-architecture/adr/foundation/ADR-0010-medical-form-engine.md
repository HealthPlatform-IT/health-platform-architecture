---
title: ADR-0010 - Medical Form Engine
status: Accepted
date: 2026-07-03
deciders:
  - Architecture Team
tags:
  - architecture
  - platform-services
  - medical-form-engine
  - clinical-record
  - foundation
related:
  - architecture-sessions/AS-006-medical-form-engine.md
  - docs/05-architecture/medical-form-engine.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - workspace/AS-006/confirmation-package.md
---

# ADR-0010 — Medical Form Engine

## Status

Accepted

---

# Contexto

A **AS-005** (ADR-0009) classificou o **Medical Form Engine** como **Strong Candidate** com fronteira **Needs Review** (**OQ-PS05**): *definição de formulário pode parecer domínio*.

O **ADR-0005** cataloga o serviço como **Identificado** — escopo preliminar em capability **Registrar** / **Executar**, sem contratos detalhados.

A **AS-006** investigou e confirmou o pacote D-001 a D-008 em 2026-07-03.

---

# Problema

Sem decisão sobre o Medical Form Engine:

- Risco de duplicar lógica de formulários em múltiplos módulos.
- Risco do Engine absorver regras clínicas ou virar prontuário.
- Risco de confundir captura dinâmica com Document Engine.
- **Q-013** (parte Medical Form Engine) e **OQ-PS05** permanecem abertas.

---

# Decisão

## 1. Classificação (D-001)

**Medical Form Engine** é **Platform Service** — mecanismo transversal reutilizável de **captura estruturada**.

**Não é:** Business Domain, Module, Read Model, Core Platform, prontuário.

**Tier:** promovido de **Strong Candidate** para **Confirmed** (13º PS confirmado pós-AS-006).

**OQ-PS05 Answered:** não é parte de Clinical Record Domain.

## 2. Definição

Prover que a plataforma **defina, versione, configure, componha e utilize** formulários clínicos dinâmicos de forma reutilizável — **sem** acoplar regras específicas de domínio ao Core ou aos módulos.

## 3. Modelo de três artefatos (D-002)

| Artefato | Owner |
|---|---|
| **Form Definition** | Medical Form Engine |
| **Form Instance** | Engine (runtime) — sem ownership clínico |
| **Clinical Content** | Business Domain destino |

```text
Módulo → render (Engine) → preencher → validar estrutura → submit → domínio valida conteúdo → persiste
```

## 4. Separação forma vs conteúdo (D-003)

| Tipo | Owner |
|---|---|
| Validação estrutural (tipo, formato, obrigatoriedade de campo) | **Medical Form Engine** |
| Validação clínica (semântica, protocolo, regra de negócio) | **Business Domain destino** |
| Autorização | **Authorization Service** |

**Regra:** Engine valida **forma**; domínio valida **conteúdo clínico**.

## 5. Versionamento (D-004)

- **Form Definition** imutável após publicação; alteração = nova versão.
- **Form Instance** referencia a version usada.
- **Clinical Content** independente do ciclo de definition.

## 6. Configuração (D-005)

Variação por tenant / instituição / unidade / especialidade via **Configuration Service** + metadados de definition (ADR-0006).

**Proibido:** fork de Engine ou schema por código cliente.

## 7. Consumidores (D-006)

| Módulo | Destino primário do submit |
|---|---|
| M-03 Attendance | Clinical Record |
| M-04 Clinical Documentation | Clinical Record |
| M-05 Orders | Clinical Orders |
| M-14 Diagnostic | Clinical Record + Diagnostic Operations |
| M-15 Home Care | Clinical Record + Home Care Operations |

M-02 Clinical Workspace = shell — não consome Engine diretamente.

Destino declarado por metadado da Form Definition / contexto do módulo.

## 8. Fronteiras com outros PS (D-007, D-008)

| PS | Fronteira |
|---|---|
| **Template Service** | Engine define estrutura; Template renderiza layout textual/documental — pode referenciar, não absorver *(Q-014 parcial)* |
| **Document Engine** | Engine captura; Document Engine gera artefato formal — ADR-0011 |
| **Configuration** | Habilita definitions; Engine armazena estrutura |
| **Authorization / Audit** | Política e rastreabilidade — não validação clínica |

## 9. O que pertence ao Engine

Definição estrutural, versionamento de definition, composição, metadados, validação estrutural, contratos conceituais de render/submit, coleta estruturada de respostas.

## 10. O que NÃO pertence

Significado clínico, diagnóstico, prescrição, decisão clínica, documento formal, assinatura, prontuário, timeline, UI final, APIs, schema técnico, persistência física.

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Formulários dentro de Clinical Record Domain | M-03 também consome; viola critério PS |
| Motor embutido em cada módulo | Duplicação; inconsistência |
| Engine como prontuário | Clinical Record mantém ownership |

---

# Consequências

## Positivas

- Q-013 **Partial Answered** (parte Medical Form Engine).
- OQ-PS05 **Answered**.
- Contrato conceitual para M-03, M-04, M-05, M-14, M-15.

## Negativas

- Q-014 e Q-013 (Document Engine) permanecem parciais/abertas.
- Q-015 (herança config) — detalhe técnico futuro.

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| ADR-0005 | Catálogo PS — Medical Form Engine detalhado |
| ADR-0006 | Configuração vs customização em definitions |
| ADR-0008 | Clinical Record, Orders, Documents — domínios destino |
| ADR-0009 | Strong Candidate → Confirmed; OQ-PS05 resolvida |

---

# Open Questions residuais

| ID | Assunto |
|---|---|
| Q-003 | Event Model — AS-010 |
| Q-015 | Herança config tenant → instituição |

---

# Documentação oficial

- `docs/05-architecture/medical-form-engine.md`
- `docs/05-architecture/document-engine.md` — fronteira MFE ↔ DE (ADR-0011)
- `docs/05-architecture/platform-services.md` (v0.4.0)
- `docs/05-architecture/module-strategy.md`

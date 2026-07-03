---
title: ADR-0011 - Document Engine
status: Accepted
date: 2026-07-03
deciders:
  - Architecture Team
tags:
  - architecture
  - platform-services
  - document-engine
  - clinical-documents
  - foundation
related:
  - architecture-sessions/AS-007-document-engine.md
  - docs/05-architecture/document-engine.md
  - docs/05-architecture/medical-form-engine.md
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0005-platform-services.md
  - docs/05-architecture/adr/foundation/ADR-0010-medical-form-engine.md
  - workspace/AS-007/confirmation-package.md
---

# ADR-0011 — Document Engine

## Status

Accepted

---

# Contexto

A **AS-005** classificou o **Document Engine** como **Strong Candidate**. O **ADR-0005** cataloga como **Identificado** — geração de documentos clínicos formais.

A **AS-006** (ADR-0010) definiu Medical Form Engine como captura estruturada e esboçou fronteira: captura ≠ documento formal (D-008).

A **AS-007** investigou e confirmou o pacote D-001 a D-008 em 2026-07-03.

---

# Problema

Sem decisão sobre o Document Engine:

- Risco de M-06 implementar renderização localmente.
- Risco do Engine absorver regras de Clinical Documents Domain.
- Risco de confundir com Medical Form Engine ou Template Service.
- **Q-013** (parte Document Engine) e **Q-014** permanecem abertas.

---

# Decisão

## 1. Classificação (D-001)

**Document Engine** é **Platform Service Confirmed** (14º PS). **Não** é Clinical Documents Domain.

**OQ-PS06 Answered:** PS dedicado de geração/renderização formal.

## 2. Definição

Gerar, renderizar e gerenciar **tecnicamente** documentos clínicos **formais** a partir de templates e dados estruturados — sem regras de negócio de documento nem ownership do artefato.

## 3. Modelo de três artefatos (D-002)

| Artefato | Owner |
|---|---|
| **Document Template** | Template Service |
| **Generation Instance** | Document Engine (runtime) |
| **Clinical Artifact** | Clinical Documents Domain |

## 4. Separação render vs regra (D-003)

- **Engine:** validação de renderização (dados para merge, formato de saída).
- **Clinical Documents:** regra de documento válido, ciclo de vida do artefato.
- **Clinical Orders:** validação de ordem antes de receita formal (quando aplicável).

## 5. Consumidores (D-004)

| Módulo | Uso |
|---|---|
| M-05 Orders | Receita, solicitação formal |
| M-06 Documents | Laudo, atestado, termo, relatório |

## 6. Template Service (D-005)

Template Service **permanece independente** (Confirmed ADR-0005). Document Engine **consome** templates — não absorve Template Service.

**Q-014 Answered.**

## 7. Fronteira Medical Form Engine (D-006)

Captura dinâmica (ADR-0010) ≠ geração formal. Fluxo E-12: MFE captura → domínio valida → DE gera artefato.

## 8. Versionamento (D-007)

Template versionada no Template Service; artefato clínico imutável pós-formalização no Clinical Documents.

## 9. Assinatura digital (D-008)

Ownership conceitual em **Clinical Documents**; implementação técnica de certificado — **fase futura**, fora deste ADR.

## 10. O que pertence / não pertence

**Pertence:** renderização formal, merge template+dados, contratos generate/preview, versionamento técnico de render.

**Não pertence:** regra de laudo, ciclo de vida artefato, assinatura, captura em formulário (MFE), storage de template, envio (Communication), blob (File Service).

---

# Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Document Engine dentro de Clinical Documents | Critério PS — M-05 também consome |
| Absorver Template Service no Engine | Communication também usa templates |
| M-06 com render próprio | Duplicação |

---

# Consequências

## Positivas

- **Q-013 Answered** (completo com ADR-0010).
- **Q-014 Answered**.
- Paridade conceitual MFE ↔ DE.

## Negativas

- Assinatura digital detalhada — sessão futura.
- Template Service tier Strong — pode promover depois.

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| ADR-0005 | Catálogo PS |
| ADR-0010 | Fronteira captura vs formal |
| ADR-0008 | Clinical Documents Domain |

---

# Documentação oficial

- `docs/05-architecture/document-engine.md`
- `docs/05-architecture/platform-services.md` (v0.4.0)

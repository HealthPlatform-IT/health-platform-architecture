---
title: Document Engine
status: Draft
version: 0.2.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Architecture
phase: Product & Architecture Foundation
related:
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/medical-form-engine.md
  - docs/05-architecture/module-strategy.md
  - docs/04-domain/business-domains.md
  - docs/05-architecture/adr/foundation/ADR-0011-document-engine.md
  - architecture-sessions/AS-007-document-engine.md
---

# Document Engine

> Platform Service de **geração e renderização formal** de documentos clínicos a partir de templates e dados estruturados.

Decisão formal: [ADR-0011](adr/foundation/ADR-0011-document-engine.md) · AS-007 D-001 a D-008.

---

## 1. Purpose

Permitir geração consistente de laudos, atestados, receitas, termos e relatórios formais sem acoplar regras de domínio ao mecanismo de renderização.

**Pergunta respondida:**

> O Engine gera e renderiza; Clinical Documents possui regras e o artefato clínico.

---

## 2. Classificação

| Campo | Valor |
|---|---|
| Tipo | Platform Service |
| Tier | **Confirmed** (promovido AS-007) |
| Capability | Registrar |
| Nível hierárquico | Clinical **Artifact** (ADR-0001) |

---

## 3. Três artefatos

| Artefato | Owner |
|---|---|
| **Document Template** | Template Service |
| **Generation Instance** | Document Engine |
| **Clinical Artifact** | Clinical Documents Domain |

---

## 4. Fluxo conceitual

```text
1. Domínio/módulo valida regra de negócio
2. Solicita generate(templateId, data, context)
3. Engine obtém template (Template Service)
4. Engine renderiza (merge + formato)
5. Clinical Documents persiste Clinical Artifact
6. File Service armazena blob (se aplicável)
7. Communication entrega (opcional)
```

---

## 5. Responsabilidades

### Pertence ao Engine

- Renderização formal (PDF/HTML conceitual)
- Merge template + dados estruturados
- Pré-visualização antes de formalizar
- Versionamento técnico de render
- Contrato de geração sob demanda

### Não pertence

| Item | Owner |
|---|---|
| Regra de documento válido | Clinical Documents |
| Ciclo de vida do artefato | Clinical Documents |
| Assinatura legal | Clinical Documents *(futuro)* |
| Captura em formulário | Medical Form Engine |
| Catálogo de template | Template Service |
| Armazenamento de arquivo | File Service |
| Envio ao paciente | Communication Service |

---

## 6. Validação

| Tipo | Owner |
|---|---|
| Render (dados completos, formato) | Document Engine |
| Regra e ciclo de vida do documento | Clinical Documents |
| Ordem/prescrição antes de receita | Clinical Orders |

---

## 7. Consumidores

| Módulo | Exemplos |
|---|---|
| M-05 Orders | Receita, solicitação formal |
| M-06 Documents | Laudo, atestado, termo, relatório |

---

## 8. Fronteiras

| PS / Domínio | Fronteira |
|---|---|
| **Medical Form Engine** | Captura ≠ documento formal |
| **Template Service** | Template storage; Engine consome |
| **Clinical Documents** | Regras + Clinical Artifact |
| **File Service** | Blob do PDF gerado |

---

## 9. Exemplos (conceitual)

Laudo · atestado · receita · termo de consentimento · relatório clínico · encaminhamento formal.

**Não são Document Engine:** anamnese, evolução SOAP (Medical Form Engine); e-mail de agendamento (Communication).

---

## 10. Anti-patterns

| Anti-pattern | Correção |
|---|---|
| Engine define regra de laudo | Clinical Documents |
| Formulário gera PDF direto | MFE → DE |
| Engine absorve Template Service | Template PS independente |

---

## 11. Fora de escopo

APIs, biblioteca PDF, assinatura digital implementada, schema técnico.

---

## 12. Documentos relacionados

- [medical-form-engine.md](medical-form-engine.md) — ADR-0010
- [platform-services.md](platform-services.md)
- [module-strategy.md](module-strategy.md)

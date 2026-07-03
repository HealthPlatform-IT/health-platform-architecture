# Document Engine — Boundary

> **Promovido para documentação oficial:** `docs/05-architecture/document-engine.md` (ADR-0011).  
> Este arquivo permanece como registro histórico da investigação NR-001/NR-002.

**Pré-requisitos:** ADR-0010 · ADR-0005 · Clinical Documents Domain · `module-strategy.md`  
**Última atualização:** 2026-07-03

---

## 1. Propósito

Definir **o que é o Document Engine**, **o que pertence a ele** e **como se diferencia** de Clinical Documents Domain, Medical Form Engine, Template Service e módulos M-05/M-06.

**Pergunta-guia:**

> O Engine gera e renderiza; o domínio define o que é documento válido e possui o artefato clínico.

---

## 2. Definição (NR-001)

**Document Engine** é um **Platform Service** responsável por **gerar, renderizar e gerenciar tecnicamente** documentos clínicos **formais** a partir de templates e dados estruturados — sem regras de negócio clínicas nem ownership do artefato assinável.

| Característica | Valor |
|---|---|
| Tipo | Platform Service (Strong Candidate → **Confirmed** proposto) |
| Capability | Registrar |
| Nível hierárquico | Clinical **Artifact** (ADR-0001, I-03) |
| UI | Não possui interface final — módulos orquestram |

### Por que é Platform Service

| Teste | Resultado |
|---|---|
| Transversal, ≥2 consumidores? | M-05, M-06 (+ fluxos via domínios) |
| Vocabulário ubíquo de domínio? | Não — regras em Clinical Documents |
| Funcionalidade ao usuário? | Não — é mecanismo |

**OQ-PS06 (investigativo):** Document Engine **não** é parte de Clinical Documents Domain.

---

## 3. Modelo de três artefatos (NR-002)

| Artefato | Owner | Descrição |
|---|---|---|
| **Document Template** | **Template Service** *(referência)* | Layout e variáveis reutilizáveis |
| **Render Request / Generation Instance** | **Document Engine** | Solicitação de geração com dados + template |
| **Clinical Artifact** | **Clinical Documents Domain** | Artefato formal — ciclo de vida, validade, assinatura |

```text
Módulo + domínio validam regra
        ↓
Document Engine: merge template + dados → render formal
        ↓
Clinical Documents: persiste Clinical Artifact + metadados
        ↓
File Service: blob PDF/HTML (se aplicável)
```

**Paralelo com ADR-0010:**

| Medical Form Engine | Document Engine |
|---|---|
| Form Definition | Document Template (Template PS) |
| Form Instance | Generation Instance |
| Clinical Content | Clinical Artifact |

---

## 4. O que pertence ao Document Engine

| # | Responsabilidade | Exemplo |
|---|---|---|
| R-01 | Orquestração de **renderização formal** | HTML/PDF conceitual |
| R-02 | Merge de template + dados estruturados | Variáveis em laudo |
| R-03 | Versionamento **técnico de render** | Render v2 com mesmo template |
| R-04 | Contrato de geração sob demanda | `generate(documentType, data, templateId)` |
| R-05 | Contrato de pré-visualização | Preview antes de formalizar |
| R-06 | Compatibilidade de formato de saída | PDF, HTML imprimível *(conceitual)* |
| R-07 | Registro técnico de geração | correlationId, templateVersion usada |

---

## 5. O que NÃO pertence

| # | Responsabilidade | Owner |
|---|---|---|
| X-01 | Regra de documento válido | Clinical Documents |
| X-02 | Ciclo de vida do artefato (rascunho → assinado → revogado) | Clinical Documents |
| X-03 | Assinatura legal / certificado digital | Clinical Documents *(futuro)* + PS especializado |
| X-04 | Conteúdo clínico capturado em formulário | Medical Form Engine → Clinical Record |
| X-05 | Definição de formulário dinâmico | Medical Form Engine |
| X-06 | Armazenamento de template | Template Service |
| X-07 | Envio ao paciente | Communication Service |
| X-08 | Metadados de arquivo persistido | File Service |
| X-09 | Prescrição como ordem clínica | Clinical Orders |
| X-10 | APIs, biblioteca PDF, schema | Fase técnica |

---

## 6. Fronteira Medical Form Engine (NR-004)

| Medical Form Engine | Document Engine |
|---|---|
| Captura dinâmica em fluxo | Documento formal finalizado |
| Form Definition | Document Template |
| Submit → domínio clínico | Generate → Clinical Artifact |
| Durante atendimento | Após dados válidos para formalização |

**E-12 (consentimento):** MFE captura → Clinical Documents valida → Document Engine gera termo formal.

---

## 7. Catálogo de exemplos

| # | Exemplo | Engine? | Módulo | Domínio |
|---|---|---|---|---|
| E-01 | Laudo de exame | ✓ render | M-06 | Clinical Documents |
| E-02 | Atestado médico | ✓ render | M-06 | Clinical Documents |
| E-03 | Receita / prescrição formal | ✓ render | M-05/M-06 | Clinical Orders + Documents |
| E-04 | Termo de consentimento | ✓ render | M-06 | Clinical Documents |
| E-05 | Relatório clínico formal | ✓ render | M-06 | Clinical Documents |
| E-06 | Encaminhamento formal | ✓ render | M-05/M-06 | Clinical Orders / Documents |
| E-07 | Anamnese | ✗ | M-04 | Medical Form Engine |
| E-08 | Evolução SOAP | ✗ | M-04 | Medical Form Engine |
| E-09 | E-mail de agendamento | ✗ | — | Communication + Template |

---

## 8. Critérios de classificação

| Pergunta | Engine | Domínio |
|---|---|---|
| É renderização formal de artefato? | ✓ | |
| É regra de validade do laudo? | | Clinical Documents |
| É captura de campos em atendimento? | | Medical Form Engine |
| É template reutilizável? | | Template Service |
| É armazenamento do PDF? | | File Service |

---

## 9. Anti-patterns

| Anti-pattern | Correção |
|---|---|
| Engine define regra de laudo | Clinical Documents |
| Engine armazena artefatos clínicos | Clinical Documents + File Service |
| M-06 implementa render próprio | Document Engine |
| Template com regra clínica embutida | Domínio valida antes de generate |
| Formulário gera PDF direto | MFE captura → DE gera |

---

## 10. Riscos

| ID | Risco |
|---|---|
| R-DE-01 | Engine vira módulo de documentos |
| R-DE-02 | Regra clínica no template/engine |
| R-DE-03 | Absorver Template Service (Q-014) |
| R-DE-04 | Confundir com Medical Form Engine |

---

## 11. Referências

- `domain-interactions.md`
- `template-service-boundary-draft.md`
- `medical-form-engine.md` (ADR-0010)

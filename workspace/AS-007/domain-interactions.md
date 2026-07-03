# Domain Interactions — Document Engine

> **Promovido para documentação oficial:** `docs/05-architecture/document-engine.md` (ADR-0011).  
> Este arquivo permanece como registro histórico da investigação NR-003.

**Última atualização:** 2026-07-03

---

## Matriz domínio ↔ Engine

| Business Domain | Relação | Domínio | Engine |
|---|---|---|---|
| **Clinical Documents** | Destino primário | Regras de artefato, ciclo de vida, assinatura | Renderização formal |
| **Clinical Orders** | Fonte de dados | Prescrição, ordem — intenção clínica | Receita/solicitação formal renderizada |
| **Clinical Record** | Fonte de dados | Conteúdo clínico para laudo/relatório | Merge em template |
| **Care Delivery** | Contexto | Consentimento contextual | Geração de termo após validação |
| **Communication** | Downstream | Entrega ao paciente | Não envia |

---

## Matriz módulo ↔ Engine

| Módulo | Consome Engine? | Uso |
|---|---|---|
| **M-05 Orders** | ✓ | Receita, solicitação formal |
| **M-06 Documents** | ✓ | Laudo, atestado, termo, relatório |
| M-03 Attendance | ○ | Consentimento — via M-06 ou domínio |
| M-02 Shell | ○ | Hospeda módulos |

---

## Fluxo — Laudo (E-01)

```text
Clinical Record (dados) + Clinical Documents (regra)
        ↓
M-06 solicita generate
        ↓
Template Service → template
Document Engine → render
        ↓
Clinical Documents persiste Clinical Artifact
File Service → blob
```

---

## Platform Services na cadeia

| PS | Papel |
|---|---|
| **Template Service** | Fornece template |
| **Document Engine** | Renderiza |
| **File Service** | Armazena arquivo gerado |
| **Medical Form Engine** | Upstream — captura, não gera formal |
| **Communication** | Entrega pós-artefato |
| **Audit** | Rastreio de geração e assinatura |

---

## Tensões

| ID | Tensão | Resolução investigativa |
|---|---|---|
| T-DE-01 | Orders vs Documents quem chama generate | Domínio/Módulo orquestra; Engine stateless |
| T-DE-02 | Template no Engine vs Template PS | Template PS; Engine consome (D-005) |

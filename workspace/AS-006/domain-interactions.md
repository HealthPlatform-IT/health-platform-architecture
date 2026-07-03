# Domain Interactions — Medical Form Engine

> **Promovido para documentação oficial:** `docs/05-architecture/medical-form-engine.md` (ADR-0010).  
> Registro histórico NR-003.

**Última atualização:** 2026-07-03

---

## 1. Propósito

Mapear como o **Medical Form Engine** se relaciona com **Business Domains**, **módulos** e **Platform Services** relacionados — quem solicita, quem valida, quem recebe submit.

---

## 2. Matriz domínio ↔ Engine

| Business Domain | Relação com Engine | O que o domínio faz | O que o Engine faz | Exemplo |
|---|---|---|---|---|
| **Care Delivery** | Contexto | Define *quando* capturar no Attendance | Renderiza no contexto Clinical Event | Triagem na consulta |
| **Clinical Record** | Destino primário | Recebe Clinical Content; valida semântica; persiste | Captura estruturada; submit payload | Anamnese, evolução |
| **Clinical Orders** | Destino secundário | Recebe intenção/ordem estruturada | Captura campos de solicitação/prescrição | Formulário de pedido de exame |
| **Clinical Documents** | Downstream | Artefato formal a partir de dados capturados | Fornece dados — **não** gera documento | Consentimento → atestado |
| **Care Monitoring** | Indireto | Protocolos e alertas — regras de domínio | Pode capturar checklist se configurado | Protocolo assistencial *(regra no domínio)* |
| **Diagnostic Operations** | Extension | Domínio extension; consome Engine | Definitions por modelo operacional | Formulário diagnóstico M-14 |
| **Home Care Operations** | Extension | Idem | Idem | Formulário visita domiciliar M-15 |

**Domínios sem relação direta com Engine:** Patient Relationship, Communication, Integration, Payer, Governance *(exceto políticas via Authorization/Audit)*.

---

## 3. Matriz módulo ↔ Engine

| Módulo | Domínio | Papel com Engine | Consome Engine? | Destino do submit |
|---|---|---|---|---|
| **M-02** Clinical Workspace | Shell | Hospeda módulos — **não** consome diretamente | ○ | — |
| **M-03** Attendance | Care Delivery | Orquestra captura **durante** atendimento | ✓ | Clinical Record *(majoritário)* |
| **M-04** Clinical Documentation | Clinical Record | Orquestra registro **estruturado** | ✓ | Clinical Record |
| **M-05** Orders | Clinical Orders | Orquestra captura de ordens | ✓ *(investigar)* | Clinical Orders |
| **M-06** Documents | Clinical Documents | Pode iniciar captura pré-documento | ○ / ✓ | Clinical Documents *(via Record)* |
| **M-14** Diagnostic | Extension | Formulários especializados | ✓ | Clinical Record + Diagnostic |
| **M-15** Home Care | Extension | Formulários domiciliares | ✓ | Clinical Record + Home Care |

**Atualização proposta para module-strategy (pós-confirmação):** incluir M-05 como consumidor do Medical Form Engine.

---

## 4. Fluxo por cenário

### Cenário A — Captura em atendimento (E-06, E-08)

```text
Care Delivery (contexto Attendance)
        ↓
M-03 solicita formDefinitionId + runtimeContext
        ↓
Medical Form Engine → render + validação estrutural
        ↓
Submit → Clinical Record valida conteúdo → persiste
        ↓
Clinical Timeline projeta (read model)
```

### Cenário B — Evolução estruturada (E-02)

```text
M-04 solicita renderização
        ↓
Medical Form Engine
        ↓
Submit → Clinical Record
```

### Cenário C — Solicitação clínica (E-11)

```text
M-05 solicita renderização
        ↓
Medical Form Engine
        ↓
Submit → Clinical Orders (não Clinical Record como destino primário)
```

**Tensão T-MFE-05:** destino do submit depende do **tipo de formulário**, não do módulo isoladamente. O contrato de submit deve declarar **domínio destino esperado**.

### Cenário D — Consentimento → documento (E-12)

```text
Engine captura consentimento estruturado
        ↓
Clinical Record ou Clinical Documents recebe conteúdo
        ↓
Clinical Documents + Document Engine geram artefato formal (ADR-0011)
```

---

## 5. Platform Services relacionados

| PS | Interação | Responsabilidade na cadeia de formulário |
|---|---|---|
| **Configuration Service** | Engine consulta definitions habilitadas por tenant/instituição | Variação sem customização |
| **Authorization Service** | Antes de render/preencher/submit | Quem pode ver/editar |
| **Audit Service** | Após alteração de definition e submit | Rastreabilidade |
| **Template Service** | Opcional — layout textual referenciado | Não substitui form definition |
| **File Service** | Campos de anexo | Metadados de arquivo |
| **Document Engine** | Downstream de captura | Artefato formal — ADR-0011 |
| **Feature Flag Service** | Habilitar definitions por rollout | Não é regra clínica |
| **Event Bus** | Futuro — evento pós-submit | Q-003 — fase técnica |

---

## 6. Regra de orquestração

```text
Módulo orquestra UX e contexto
Engine fornece estrutura e validação estrutural
Domínio destino valida conteúdo e persiste
PS transversais executam política, auditoria e configuração
Read Models projetam — não capturam
```

---

## 7. Perguntas abertas

| ID | Pergunta | Status |
|---|---|---|
| T-MFE-05 | Submit para Orders vs Record — quem decide? | Investigar — provável metadado na Form Definition |
| T-MFE-06 | M-06 consome Engine diretamente ou só via Record? | ○ provisório |
| T-MFE-07 | Extension Modules registram definitions próprias? | Hipótese: sim, via registry do Engine |

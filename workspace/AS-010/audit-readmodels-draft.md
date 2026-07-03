# Audit & Read Models — Event Strategy

> Rascunho AS-010 — NR-005. Investigação concluída — candidato a confirmação.

**Pré-requisitos:** `read-models.md` · ADR-0005 Audit · `domain-interactions.md` NR-003  
**Última atualização:** 2026-07-03

---

## 1. Problema (D-007)

Como **Audit Service** e **Read Models** (Clinical Timeline, Analytics) se relacionam com Domain Events e Event Bus?

---

## 2. Read Models — modelo de atualização

Alinhado a `read-models.md` V-05 e §7:

```text
Domínio persiste estado (fonte de verdade)
        ↓
Domínio publica Domain Event (B)
        ↓
Event Bus entrega
        ↓
Projeção Read Model atualiza visão
        ↓
Módulo (M-02, M-08, M-12) renderiza
```

| Princípio | Implicação |
|---|---|
| V-01 Ownership no domínio | Read Model **nunca** publica evento de negócio |
| V-03 Sem regra de negócio | Projeção não valida clínica — só materializa visão |
| V-06 Authorization | Filtro de visibilidade na projeção ou na leitura |

---

## 3. Clinical Timeline — assinaturas candidatas

| Domínio fonte | Eventos relevantes | Entrada na Timeline |
|---|---|---|
| Patient Relationship | `CareJourney.Started`, `CareJourney.Closed` | Marco de jornada |
| Care Delivery | `Attendance.Started`, `Attendance.Completed` | Consulta / atendimento |
| Clinical Record | `ClinicalContent.Recorded` | Evolução, anamnese |
| Clinical Orders | `Order.Prescribed` | Prescrição |
| Clinical Documents | `ClinicalArtifact.Formalized` | Laudo, atestado, termo |
| Care Monitoring | `ClinicalAlert.Raised` | Alerta clínico |
| Communication | `Message.Dispatched` *(se relevância clínica — OQ-V01)* | Comunicação na linha do tempo |
| Diagnostic Operations | `DiagnosticResult.Available` | Resultado *(Extension)* |

**OQ-V01** (`read-models.md`): critério Communication na Timeline — **permanece Open**; não bloqueia D-007.

---

## 4. Analytics & Reporting — assinaturas candidatas

| Fonte | Uso na projeção |
|---|---|
| Care Coordination | Volume de agendamentos, cancelamentos |
| Care Delivery | Throughput de atendimentos |
| Operations Monitoring | SLAs, filas |
| Care Monitoring | Agregação de alertas |
| Múltiplos domínios | KPIs compostos |

**Distinção:** M-07 Care Monitoring = risco **individual** · Analytics = agregação **institucional** (`read-models.md` §5).

---

## 5. Audit Service — dois padrões investigativos

ADR-0005: Audit registra ações relevantes — rastreabilidade, alterações, acesso a dados sensíveis.

| Padrão | Descrição | Quando |
|---|---|---|
| **A — Consumo de eventos** | Audit assina Domain Events e persiste trilha | Ações de negócio já modeladas como evento |
| **B — API síncrona dedicada** | Domínio/PS chama Audit explicitamente | Acesso a dado sensível, leitura, autenticação |

**Proposta D-007:** **ambos coexistem** — não mutuamente exclusivos.

```text
Domínio persiste + publica evento (B)
        ↓
        ├── Event Bus → Audit (padrão A) — trilha de negócio
        └── Audit.log(action) (padrão B) — quando exigência de compliance imediata
```

| Tipo de fato | Padrão preferido |
|---|---|
| `ClinicalArtifact.Formalized` | A — via evento |
| `ClinicalContent.Recorded` | A — via evento |
| Acesso a prontuário (leitura) | B — Audit síncrono |
| Login / falha auth | B — Identity + Audit |
| Exportação LGPD autorizada | A + B — domínio Governance |

**Regra:** Audit **não interpreta** regra clínica — registra *o que ocorreu* conforme payload autorizado pelo publicador.

---

## 6. Audit vs Observability

| PS | Foco | Event Bus? |
|---|---|---|
| **Audit Service** | Conformidade, trilha de negócio/acesso | Consumidor candidato |
| **Observability Service** | Métricas, traces, logs técnicos | Paralelo — não substitui Audit |

---

## 7. Search Service — consumidor candidato

| Evento | Reação Search |
|---|---|
| `ClinicalContent.Recorded` | Indexar documento clínico |
| `ClinicalArtifact.Formalized` | Indexar artefato |
| `Patient.Registered` | Indexar paciente |

Search = **Strong Candidate** — indexação assíncrona via eventos alinha com desacoplamento (AS-005).

---

## 8. Idempotência e ordenação (conceitual — sem tecnologia)

Questões para Sprint 3 — registradas sem decidir:

| Questão | Owner futuro |
|---|---|
| Ordenação de eventos por aggregate | Event Foundation contrato |
| Reprocessamento de projeção | Read Model + Event Bus |
| Duplicata na assinatura | Event Bus (mecanismo) |

AS-010 **não** responde implementação — apenas estabelece que Read Models e Audit são **assinantes legítimos**.

---

## 9. Impacto em decisões

| ID | Proposta NR-005 |
|---|---|
| D-007 | Read Models = assinantes; Audit = assinante + API síncrona complementar |
| D-003 | Reforça camada B → projeções camada C de leitura |

**Status D-007:** Ready for Confirmation

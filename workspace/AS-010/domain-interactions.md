# Domain Interactions — Event Strategy

> Rascunho AS-010 — NR-003. Investigação concluída — candidato a confirmação.

**Pré-requisitos:** `event-strategy-boundary.md` NR-001/002 · `business-domains.md` · `module-strategy.md`  
**Última atualização:** 2026-07-03

---

## 1. Regra de publicação (D-005 candidata)

```text
Módulo (UX/fluxo) → invoca domínio → domínio valida regra → domínio persiste → domínio publica Domain Event (B)
```

| Papel | Publica na bus? | Motivo |
|---|---|---|
| **Business Domain** | **Sim** — fonte de verdade | Após persistência e validação de negócio |
| **Module** | **Não diretamente** *(investigativo)* | Orquestra; não substitui semântica do domínio |
| **Platform Service** | **Não** — exceto eventos técnicos *(futuro Sprint 3)* | PS executa mecanismo; Audit via consumo |
| **Read Model** | **Não** | Apenas assinante — V-01 `read-models.md` |

**Exceção investigativa:** eventos puramente técnicos de infraestrutura (ex.: `Module.Enabled`) — owner Configuration/Module Registry — **fora do escopo clínico**; detalhar em Sprint 3 se necessário.

---

## 2. Matriz domínio — publicador

| Business Domain | Domain Events candidatos (exemplos) | Gatilho clínico (camada A) | Módulo orquestrador |
|---|---|---|---|
| **Patient Relationship** | `Patient.Registered`, `Caregiver.Linked` | — | M-08, M-09 |
| **Care Coordination** | `Appointment.Scheduled`, `Appointment.Cancelled` | — | M-01 |
| **Care Delivery** | `Attendance.Started`, `Attendance.Completed` | Clinical Event | M-03 |
| **Clinical Record** | `ClinicalContent.Recorded`, `Diagnosis.Recorded` | Clinical Event | M-04 |
| **Clinical Orders** | `Order.Prescribed`, `Order.Cancelled` | — | M-05 |
| **Clinical Documents** | `ClinicalArtifact.Formalized`, `Consent.Signed` | Clinical Artifact | M-06 |
| **Care Monitoring** | `ClinicalAlert.Raised`, `Pendency.Created` | — | M-07 |
| **Communication** | `Message.Dispatched` *(domínio decide quando)* | — | Consumidores diversos |
| **Integration** | `ExternalResult.Received` | — | M-11 |
| **Governance & Compliance** | `DataProcessingConsent.Granted` | — | M-16 |
| **Patient Relationship** *(jornada)* | `CareJourney.Started`, `CareJourney.Closed` | Care Journey Start Event (ADR-0007) | M-01, M-03, M-08 |

**Convenção investigativa de nome:** `{AggregateOrConcept}.{PastTense}` — vocabulário do domínio publicador, não do Event Bus.

---

## 3. Matriz domínio — consumidor (assinantes)

| Business Domain / Destino | Assina eventos de… | Reação esperada |
|---|---|---|
| **Clinical Record** | Care Delivery (`Attendance.Completed`) | Contexto para evolução |
| **Clinical Documents** | Clinical Orders, Clinical Record | Gerar artefato formal (via M-06 + DE) |
| **Care Monitoring** | Clinical Record, Clinical Orders, Care Delivery | Alertas, pendências |
| **Communication** | Múltiplos *(quando domínio Communication decide enviar)* | Orquestra Communication PS |
| **Operations Monitoring** | Care Coordination, Integration | SLAs operacionais *(Analytics)* |

---

## 4. Matriz módulo ↔ Event Bus

| Módulo | Publica? | Consome? | Padrão |
|---|---|---|---|
| M-01 Scheduling | Via domínios | ○ | Care Coordination publica |
| M-02 Clinical Workspace | Não | ✓ *(indireto)* | Hospeda UI; Timeline consome projeção |
| M-03 Attendance | Via Care Delivery | ○ | |
| M-04 Clinical Documentation | Via Clinical Record | ○ | |
| M-05 Orders | Via Clinical Orders | ○ | Pode reagir a eventos para UI |
| M-06 Documents | Via Clinical Documents | ○ | |
| M-07 Care Monitoring | Via Care Monitoring | ✓ | Alertas em tempo quase real |
| M-08 Patient Portal | Via Patient Relationship | ✓ | Notificações in-app |
| M-12 Operations Dashboard | — | ✓ *(via Analytics RM)* | |
| M-14 Diagnostic | Via Diagnostic Operations | ○ | Extension |
| M-15 Home Care | Via Home Care Operations | ○ | Extension |

✓ = esperado · ○ = opcional / indireto

---

## 5. Platform Services na cadeia de eventos

| PS | Publica? | Consome? | Papel |
|---|---|---|---|
| **Event Bus** | Não *(transporta)* | Não *(roteia)* | Mecanismo |
| **Audit Service** | Não | ✓ *(candidato)* | Rastreabilidade — ver NR-005 |
| **Search Service** | Não | ✓ *(candidato)* | Indexação assíncrona |
| **Notification Service** | Não | ✓ *(via handler)* | In-app após domínio decidir |
| **Communication Service** | Não | ✓ *(via handler)* | Canais externos após domínio |
| **Integration Service** | Não | ✓ *(bridge)* | Traduz evento interno → externo |
| **Webhook Service** | Não | ✓ *(bridge)* | HTTP callback externo |
| **Medical Form Engine** | Não | Não | Submit → domínio → evento |
| **Document Engine** | Não | Não | Generate → domínio persiste → evento |
| **Observability Service** | Não | ○ | Métricas técnicas — paralelo |

---

## 6. Fluxos integrados (exemplos)

### F-01 — Início de jornada (E-01)

```text
M-01/M-03 valida contexto
        ↓
Patient Relationship / Care Delivery — Care Journey Start Event (A)
        ↓
Domínio publica CareJourney.Started (B)
        ↓
Event Bus → Timeline, Audit, Care Monitoring (assinantes)
```

### F-02 — Submit formulário MFE (E-03)

```text
M-03/M-04 — Form Instance submit
        ↓
Medical Form Engine — validação estrutural
        ↓
Clinical Record — validação clínica + persistência
        ↓
ClinicalContent.Recorded (B)
        ↓
Event Bus → Timeline, Search
```

### F-03 — Documento formal (E-04)

```text
Clinical Documents valida regra
        ↓
M-06 + Document Engine — generate
        ↓
Clinical Documents persiste Clinical Artifact
        ↓
ClinicalArtifact.Formalized (B)
        ↓
Event Bus → Timeline, Communication *(se domínio decidir)*, Audit
```

### F-04 — Prescrição (E-02 / ordem)

```text
Clinical Orders — Order.Prescribed (B)
        ↓
Event Bus → M-05 UI refresh, Documents, Care Monitoring
```

---

## 7. Read Models — posição na matriz

| Read Model | Assina Domain Events? | Atualiza projeção? | Publica? |
|---|---|---|---|
| **Clinical Timeline** | ✓ — múltiplos domínios fonte | ✓ | **Não** |
| **Analytics & Reporting** | ✓ — agregações | ✓ | **Não** |

Alinhado a `read-models.md` V-01, V-05, §7.

---

## 8. Tensões resolvidas (investigativo)

| ID | Tensão | Resolução candidata |
|---|---|---|
| T-EV-06 | Módulo publica sem domínio | **Rejeitado** — domínio publica após persistir |
| T-EV-07 | Timeline como publicador | **Rejeitado** — Read Model só consome |
| T-EV-08 | MFE publica ClinicalContent | **Rejeitado** — Clinical Record publica pós-aceite |
| T-EV-09 | DE publica artefato | **Rejeitado** — Clinical Documents publica pós-formalização |

---

## 9. Impacto em decisões

| ID | Reforço NR-003 |
|---|---|
| D-004 | Tipos definidos no vocabulário do domínio publicador |
| D-005 | Módulo orquestra; domínio publica |
| D-007 | Read Models como assinantes primários |

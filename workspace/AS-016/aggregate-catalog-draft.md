# Aggregate Catalog (candidato)

> Rascunho AS-016 — NR-002.

---

## 1. Catálogo de Aggregate Roots

| Aggregate Root | Domínio (ADR-0008) | Responsabilidade de consistência | Nível ADR-0001 |
|---|---|---|---|
| **Patient** | Patient Relationship | Identidade/cadastro do paciente no tenant; vínculos básicos | Paciente |
| **CareJourney** | Care Coordination | Ciclo de vida da Institution Care Journey; start/end; vínculo Patient | Jornada |
| **CareEpisode** | Care Coordination | Problema/objetivo assistencial; vínculo Journey + Patient | Episódio |
| **Attendance** | Care Delivery | Interação clínica (presencial/tele/etc.); status; participantes (refs) | Atendimento |
| **ClinicalArtifact** | Clinical Documents | Artefato formal (metadados + refs Storage/File); ciclo de formalização | Artefato |
| **ClinicalOrder** | Clinical Orders | Ordem/prescrição e ciclo de vida da ordem | *(relacionado a Evento/Intenção)* |

---

## 2. Não são Aggregate Roots

| Conceito | Tratamento candidato |
|---|---|
| **Clinical Event (nível A)** | Entity / registro interno do **Attendance** e/ou fato no **Clinical Record** — não root |
| Evolução / anamnese estruturada | Entidade ou aggregate fino de **Clinical Record** *(ver §3)* |
| Domain Event (bus) | Mensagem — não entidade persistida de negócio |

---

## 3. Clinical Record

| Opção | Veredito candidato |
|---|---|
| Um único aggregate “Prontuário” com tudo | **Rejeitado** (god aggregate) |
| Entradas clínicas (evolução, diagnóstico estruturado) como aggregates/entries referenciando Attendance | **Preferido** — root candidato **ClinicalEntry** (ou nome ubíquo do domínio) no Clinical Record |

**Proposta:** root **ClinicalEntry** (Clinical Record) para conhecimento clínico persistido (evolução, hipótese, conclusão), referenciando `AttendanceId` / `PatientId`. Não substitui ClinicalArtifact (documento formal).

---

## 4. Contenção vs referência

```text
Patient  ←ref─ CareJourney  ←ref─ CareEpisode  ←ref─ Attendance
                                              ├─ Clinical Events (entities)
                                              ←ref─ ClinicalEntry
                                              ←ref─ ClinicalOrder
                                              ←ref─ ClinicalArtifact
```

Episode **não** embute todos Attendances como entities eternas se isso explodir o aggregate — Attendance é root; Episode guarda invariantes do episódio e refs/contagem conforme necessidade.

# MVP Sub-Capability Slice

> Rascunho AS-017 — NR-003. Candidato a confirmação.

---

## Relacionar

| Sub-capability | Tier |
|---|---|
| Patient Relationship | **Must** |
| Professional & Provider Relationship | **Must** |
| Organization & Network Relationship | **Must** (mínimo org/unidade) |
| Insurance & Payer Relationship | **Later** (cadastro leve Should opcional; convênio profundo Later) |

## Organizar

| Sub-capability | Tier |
|---|---|
| Care Scheduling | **Must** |
| Referral Coordination | **Should** |
| Resource Planning | **Later** |
| Queue & Flow Management | **Later** |
| Operational Workflow | **Should** (mínimo para Attendance) |

## Executar

| Sub-capability | Tier |
|---|---|
| Clinical Attendance *(+ Telemedicine mode)* | **Must** |
| Therapeutic Care Delivery | **Should** (sessões básicas se no mesmo Attendance) |
| Diagnostic Service Delivery | **Later** (Extension) |
| Home Care Delivery | **Later** (Extension) |
| Hospital Care Delivery | **Out** (Future) |

## Registrar

| Sub-capability | Tier |
|---|---|
| Clinical Documentation | **Must** |
| Prescription & Clinical Orders | **Must** (ciclo básico) |
| Clinical Artifacts | **Must** (geração básica + Storage/File) |
| Diagnostic Recording | **Later** |
| Clinical Timeline | **Must** (Read Model) |

## Comunicar

| Sub-capability | Tier |
|---|---|
| Patient Communication | **Should** |
| Professional Communication | **Should** |
| Internal Notifications | **Should** |
| Communication Preferences & Consent | **Should** (mínimo) |

## Integrar

| Sub-capability | Tier |
|---|---|
| Webhooks & External APIs | **Should** (saída mínima) |
| Clinical Interoperability (FHIR…) | **Later** |
| Diagnostic Systems Integration | **Later** |
| Financial & ERP Integration | **Out** (Q-005) |
| Medical Devices & IoT | **Later** |

## Monitorar

| Sub-capability | Tier |
|---|---|
| Care Pendencies & Follow-up | **Should** |
| Clinical Alerts & Protocols | **Later** |
| Operational SLAs | **Later** |
| Indicators & Dashboards | **Should** (ops mínimo) / Analytics profundo **Later** |
| Intelligence & Automation | **Out** (Future) |

## Governar

| Sub-capability | Tier |
|---|---|
| Access & Authorization *(produto)* | **Must** (via PS — detalhe AS-009) |
| Clinical Audit & Traceability | **Must** |
| Configuration Management | **Must** |
| Compliance & Policies | **Later** / Needs Review (Q-019) |
| Feature Flags / Module enablement | **Must** |

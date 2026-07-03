# Hipóteses — AS-004

> Hipóteses de trabalho — não são decisões.

## Hipótese principal (H-DOM-001)

Business Domains emergem do **agrupamento de sub-capabilities relacionadas**, não de cópia 1:1 das 8 Core Business Capabilities.

**Alternativa preferida:** C (agrupamento) + E (híbrido com Platform Services).

---

## Candidatos por Core Capability (conceitual — não definitivo)

### Relacionar

| Sub-capability | Domínio candidato |
|---|---|
| Patient Relationship | **Patient Relationship** |
| Professional & Provider Relationship | **Professional Management** |
| Organization & Network Relationship | **Organization Management** |
| Insurance & Payer Relationship | **Payer & Insurance** |

### Organizar

| Sub-capability | Domínio candidato |
|---|---|
| Care Scheduling | **Care Coordination** |
| Resource Planning | **Care Coordination** |
| Queue & Flow Management | **Care Coordination** |
| Referral Coordination | **Care Coordination** |
| Operational Workflow | **Care Coordination** ou **Operations** |

### Executar

| Sub-capability | Domínio candidato |
|---|---|
| Clinical Attendance | **Care Delivery** |
| Therapeutic Care Delivery | **Care Delivery** |
| Diagnostic Service Delivery | **Diagnostic Operations** |
| Home Care Delivery | **Home Care Operations** |
| Hospital Care Delivery *(future)* | **Hospital Operations** *(future)* |

### Registrar

| Sub-capability | Domínio candidato |
|---|---|
| Clinical Documentation | **Clinical Record** |
| Diagnostic Recording | **Clinical Record** |
| Prescription & Clinical Orders | **Clinical Orders** |
| Clinical Artifacts | **Clinical Documents** |
| Clinical Timeline | **Clinical Record** (visão) ou cross-domain |

### Comunicar

| Sub-capability | Domínio candidato |
|---|---|
| Patient Communication | **Communication** (domínio) ou Platform Service only |
| Professional Communication | idem |
| Internal Notifications | **Notification** → Platform Service (ADR-0005) |
| Communication Preferences & Consent | **Communication** ou **Compliance** |

### Integrar

| Sub-capability | Domínio candidato |
|---|---|
| Clinical Interoperability | **Integration** (domínio) + Integration Service |
| Diagnostic Systems Integration | **Integration** |
| Financial & ERP Integration | **Financial Integration** *(Q-005)* |
| Webhooks & External APIs | **Integration** |
| Medical Devices & IoT | **Integration** |

### Monitorar

| Sub-capability | Domínio candidato |
|---|---|
| Care Pendencies & Follow-up | **Care Monitoring** |
| Clinical Alerts & Protocols | **Care Monitoring** |
| Operational SLAs | **Operations Monitoring** |
| Indicators & Dashboards | **Analytics** *(suporte)* |
| Intelligence & Automation *(future)* | fora do escopo |

### Governar → provável Platform Service, não domínio

| Sub-capability | Destino candidato |
|---|---|
| Identity & Access | Identity Service |
| Authorization & Permissions | Authorization Service |
| Audit & Traceability | Audit Service |
| Configuration & Feature Management | Configuration Service, Feature Flag Service |
| Compliance & Data Protection | transversal / Compliance *(a avaliar)* |
| Observability | Observability Service |

---

## Faixa estimada de domínios

**Hipótese:** 10–18 Business Domains de negócio + Platform Services para transversal.

*A validar na investigação.*

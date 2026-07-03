# Capability → Domain Mapping — AS-004

> Refinamento por Core Business Capability (2026-07-02).  
> **Validação holística NR-001 concluída** — confirmado 2026-07-02. Q-002 **Answered**.

## Legenda

| Símbolo / valor | Significado |
|---|---|
| **DOM** | Business Domain candidato |
| **PS** | Platform Service (ADR-0005) |
| **VIS** | Visão / read model / projeção — não domínio |
| **DEF** | Deferred / Future — adiado |
| **REJ** | Candidato a domínio rejeitado |

**Confiança:** Alta · Média-alta · Média · Baixa

---

## 1. Relacionar

| Sub-capability | Destino | Tipo | Confiança | Fronteira / nota |
|---|---|---|---|---|
| Patient Relationship | **Patient Relationship** | DOM | Alta | Vínculo ≠ jornada (ADR-0007). Não inclui cuidado ativo |
| Professional & Provider Relationship | **Professional Management** | DOM | Alta | Profissionais e prestadores, não pacientes |
| Organization & Network Relationship | **Organization Management** | DOM | Alta | Estrutura org e redes |
| Insurance & Payer Relationship | **Payer & Insurance** | DOM | Média-alta | Q-005 pode expandir para domínio financeiro futuro |

**Ambiguidades:** Payer & Insurance vs futuro Billing Domain.  
**Pergunta pendente:** Q-005 — convênio é só relacionamento ou também faturamento?

---

## 2. Organizar

| Sub-capability | Destino | Tipo | Confiança | Fronteira / nota |
|---|---|---|---|---|
| Care Scheduling | **Care Coordination** | DOM | Alta | Planejar ≠ executar (Care Delivery) |
| Resource Planning | **Care Coordination** | DOM | Alta | Recursos alocados para cuidado |
| Queue & Flow Management | **Care Coordination** | DOM | Alta | Fluxo operacional do paciente |
| Referral Coordination | **Care Coordination** | DOM | Alta | Pode emitir ReferralAccepted (Start Event) |
| Operational Workflow | **Care Coordination** | DOM | Média | Workflow configurável; fronteira com Configuration Service |

**Rejeitado:** domínio "Scheduling" / "Agenda".  
**Pergunta pendente:** Operational Workflow 100% em Care Coordination?

---

## 3. Executar

| Sub-capability | Destino | Tipo | Confiança | Fronteira / nota |
|---|---|---|---|---|
| Clinical Attendance | **Care Delivery** | DOM | Alta | Nível Attendance (ADR-0001); inclui teleconsulta |
| Therapeutic Care Delivery | **Care Delivery** | DOM | Média-alta | Sessões terapêuticas no mesmo domínio de execução clínica |
| Diagnostic Service Delivery | **Diagnostic Operations** | DOM | Alta | Execução diagnóstica separada de consulta clínica |
| Home Care Delivery | **Home Care Operations** | DOM | Alta | Extension ADR-0003 — validado NR-001 |
| Hospital Care Delivery | **Hospital Operations** | DEF | — | Futuro — não decidir nesta AS-004 |

**Rejeitado:** domínio único "Care Execution"; "Consultation Domain".  
**Ambiguidade:** Care Delivery vs Diagnostic Operations quando exame na mesma unidade.

---

## 4. Registrar

> **NR-005:** Ready for Confirmation ✅

| Sub-capability | Destino | Tipo | Confiança | Fronteira / nota |
|---|---|---|---|---|
| Clinical Documentation | **Clinical Record** | DOM | Alta | Evoluções, anamneses, notas — conhecimento clínico |
| Diagnostic Recording | **Clinical Record** | DOM | Alta | Achados e conclusões estruturadas — laudo formal → Documents |
| Prescription & Clinical Orders | **Clinical Orders** | DOM | Alta | Prescrições, solicitações, ordens — ciclo de vida |
| Clinical Artifacts | **Clinical Documents** | DOM | Alta | Artefatos formais assináveis; Document Engine (PS) gera |
| Clinical Timeline | **Clinical Timeline** | VIS | Alta | Read model permanente; prontuário = visão |

### Fronteiras Registrar (D-012)

| Domínio | Pertence | Não pertence |
|---|---|---|
| **Clinical Record** | Conhecimento clínico, diagnósticos, resultados estruturados | Ordens, laudos formais, timeline como aggregate |
| **Clinical Orders** | Prescrições, solicitações, ordens, encaminhamento estruturado | Execução, resultado, laudo, entrega |
| **Clinical Documents** | Atestados, laudos, termos, consentimentos formalizados, encaminhamento formal | Notas; Document Engine (PS) |
| **Clinical Timeline** | Projeção cronológica para leitura | Regras autônomas; escrita duplicada |

### Platform Services (Registrar)

| Função | Destino |
|---|---|
| Geração PDF / renderização | Document Engine (PS) |
| Templates técnicos | Template Service (PS) |
| Projeção / busca timeline | Search Service *(hip.)* |

**Rejeitado:** mega domínio "Clinical"; Clinical Timeline como DOM; Document Engine como domínio.

### Cenários Registrar (validação)

| # | Cenário | Principal | Envolvidos | PS | Tipo | Risco |
|---|---|---|---|---|---|---|
| 1 | Evolução na consulta | Clinical Record | Care Delivery | — | Registro | Baixo |
| 2 | Hipótese diagnóstica | Clinical Record | — | — | Registro | Baixo |
| 3 | Solicitação exame lab | Clinical Orders | Care Delivery | — | Ordem | Baixo |
| 4 | Prescrição | Clinical Orders | — | — | Ordem | Baixo |
| 5 | Atestado | Clinical Documents | — | Document Engine | Documento | Baixo |
| 6 | Encaminhamento especialista | Clinical Orders | Care Coordination, Documents | Document Engine | Ordem+coord+doc | Médio |
| 7 | Pedido exame + doc formal | Clinical Orders | Clinical Documents | Document Engine | Ordem+doc | Médio |
| 8 | Resultado LIS estruturado | Clinical Record | Integration, Diagnostic Ops | Integration Service | Registro+integração | Médio |
| 9 | Laudo formal | Clinical Documents | Clinical Record | Document Engine | Documento | Baixo |
| 10 | Consentimento informado | Clinical Documents | Care Delivery | Document Engine | Documento+gate | Médio |
| 11 | Timeline consolidada | Clinical Timeline | múltiplos | Search Service | Read model | Baixo |
| 12 | Documento via WhatsApp | Communication | Clinical Documents | Communication Service | Comunicação | Baixo |

**Perguntas pendentes:** P-REG-01 parcial; P-REG-03; P-REG-04. **P-REG-02 respondida.**

---

## 5. Comunicar

| Sub-capability | Destino | Tipo | Confiança | Fronteira / nota |
|---|---|---|---|---|
| Patient Communication | **Communication** | DOM | Alta | Políticas do que/como comunicar ao paciente |
| Professional Communication | **Communication** | DOM | Alta | Políticas de comunicação entre profissionais |
| Internal Notifications | **Notification Service** | PS | Média-alta | In-app; regra pode originar em Communication ou Care Monitoring |
| Communication Preferences & Consent | **Communication** | DOM | Alta | **Communication Consent** apenas — não LGPD, não clínico |

### Platform Services relacionados

| Função | Destino |
|---|---|
| Envio por canais externos | Communication Service (PS) |
| Templates técnicos de mensagem | Template Service (PS) — conteúdo/regras no domínio |
| Histórico de entrega técnico | Communication Service (PS) — conceito de histórico no domínio |

**Rejeitado:** tratar Comunicar inteiro como PS; Communication Consent misturado com LGPD ou Clinical Consent.  
**NR-006:** Ready for Confirmation.  
**Pergunta pendente:** Q-010 — formalização Communication vs Notification Service.

---

## 6. Integrar

| Sub-capability | Destino | Tipo | Confiança | Fronteira / nota |
|---|---|---|---|---|
| Clinical Interoperability | **Integration** | DOM | Média-alta | FHIR, TISS, contratos clínicos — regras de negócio |
| Diagnostic Systems Integration | **Integration** | DOM | Média-alta | LIS, PACS, DICOM — políticas de integração |
| Financial & ERP Integration | — | DEF | — | **Q-005** — futura integração financeira / Billing Domain |
| Webhooks & External APIs | **Integration** | DOM | Média | Contratos de API; Webhook Service (PS) para infra |
| Medical Devices & IoT | **Integration** | DOM | Média | Dispositivos; adapters via Integration Service (PS) |

**Rejeitado:** Integration Domain = apenas Integration Service.  
**Pergunta pendente:** Q-005 — Financial & ERP: capability administrativa futura, Billing Domain ou permanece em Integrar?

---

## 7. Monitorar

> **NR-008:** Ready for Confirmation ✅

| Sub-capability | Destino | Tipo | Confiança | Fronteira / nota |
|---|---|---|---|---|
| Care Pendencies & Follow-up | **Care Monitoring** | DOM | Alta | Pendências e retornos com significado clínico |
| Clinical Alerts & Protocols | **Care Monitoring** | DOM | Alta | Protocolos e alertas — entrega via PS |
| Operational SLAs | **Operations Monitoring** | DOM | Média-alta | SLAs operacionais — domínio próprio validado |
| Indicators & Dashboards | **Analytics & Reporting** | VIS | Alta | Read model permanente (D-004) |
| Intelligence & Automation | — | DEF | — | Futuro |

### Fronteiras Monitorar (D-013)

| Domínio | Pertence | Não pertence |
|---|---|---|
| **Care Monitoring** | Pendências clínicas, protocolos, alertas de risco | SLAs operacionais, entrega técnica, dashboards |
| **Operations Monitoring** | SLAs, prazos institucionais, metas de unidade | Protocolos clínicos, filas, indicadores agregados |
| **Analytics & Reporting** | Projeção de indicadores | Regras autônomas de negócio |

### Platform Services (Monitorar)

| Função | Destino |
|---|---|
| Alerta in-app | Notification Service (PS) |
| Alerta por canal | Communication Service (PS) |
| Indicadores agregados | Search Service *(hip.)* |
| Infraestrutura | Observability Service (PS) — Governar |

**Rejeitado:** mega domínio "Monitoring"; Analytics como DOM; absorver Operations Monitoring em Care Coordination ou Care Monitoring.

### Cenários Monitorar (validação)

| # | Cenário | Principal | Envolvidos | PS | Tipo | Risco |
|---|---|---|---|---|---|---|
| 1 | Exame pendente 30 dias | Care Monitoring | Clinical Orders | Notification | Pendência | Baixo |
| 2 | Resultado crítico | Care Monitoring | Clinical Record | Notification | Alerta clínico | Baixo |
| 3 | Protocolo hipertensão | Care Monitoring | Clinical Record | Notification | Protocolo | Baixo |
| 4 | Fila urgência excede tempo | Operations Monitoring | Care Coordination | — | SLA | Baixo |
| 5 | Laudo atrasado (SLA) | Operations Monitoring | Care Monitoring | — | Dual | Médio |
| 6 | Dashboard indicadores | Analytics & Reporting | múltiplos | Search Service | Read model | Baixo |
| 7 | Alerta in-app enfermeiro | Care Monitoring | — | Notification | Regra+entrega | Baixo |
| 8 | Lembrete retorno paciente | Care Monitoring | Communication | Communication Service | Alerta+canal | Baixo |
| 9 | Meta atendimentos/hora | Operations Monitoring | Care Delivery | — | Métrica | Baixo |
| 10 | CPU/latência plataforma | — | — | Observability Service | Infra | Baixo |

**Perguntas pendentes:** P-MON-01 parcial; P-MON-02.

---

## 8. Governar

| Sub-capability | Destino | Tipo | Confiança | Fronteira / nota |
|---|---|---|---|---|
| Identity & Access | Identity Service | PS | Alta | Autenticação, usuários, multi-tenant |
| Authorization & Permissions | Authorization Service | PS | Alta | Papéis e permissões técnicas |
| Audit & Traceability | Audit Service | PS | Alta | Logs e rastreabilidade técnica |
| Configuration & Feature Management | Configuration Service, Feature Flag Service | PS | Alta | Parametrização técnica |
| Compliance & Data Protection | **Governance & Compliance** | DOM | Média-alta | Políticas LGPD, retenção, governança — **não** audit/identity/envio |
| Observability | Observability Service | PS | Alta | Métricas e logs de infraestrutura |

**Rejeitado:** Identity, Authorization, Audit, Configuration como domínios de negócio.  
**NR-009:** Ready for Confirmation — domínio único cross-cutting.  
**Hipótese PS:** Compliance Service — enforcement de políticas do domínio.  
**Pergunta pendente:** Q-019 — Compliance Service no catálogo ADR-0005.

---

## Taxonomia de consentimentos (cross-capability)

| Tipo | Descrição | Domínio | PS | Observações |
|---|---|---|---|---|
| Communication Consent | Preferência/autorização de canal | Communication | Communication Service, Notification Service | Cenários 1, 2 |
| Data Processing / Privacy Consent | LGPD — tratamento, exclusão, anonimização | Governance & Compliance | Compliance Service *(hip.)*, Audit Service | Cenários 3, 4, 8 |
| Clinical Consent | Procedimento, exame, intervenção | Care Delivery + Clinical Documents | Document Engine | Cenários 5, 6 |
| Research Consent | Pesquisa clínica | Deferred | — | Q-020 |

---

## Cenários de validação (NR-006 + NR-009)

| # | Cenário | Domínio | PS | Natureza |
|---|---|---|---|---|
| 1 | Aceita lembrete WhatsApp | Communication | Communication Service | Regra + execução |
| 2 | Revoga SMS | Communication | Communication Service | Regra de negócio |
| 3 | Exclusão/anonimização LGPD | Governance & Compliance | Compliance Service, Audit; exec. em Clinical Record/Documents | Política + processo |
| 4 | Política de retenção clínica | Governance & Compliance | Configuration Service | Política institucional |
| 5 | Médico assina documento clínico | Clinical Documents | Document Engine | Regra clínica |
| 6 | Consentimento informado procedimento | Care Delivery + Clinical Documents | Document Engine | Clinical Consent |
| 7 | Responsável legal recebe resultado | Patient Relationship + Communication | Communication Service | Coordenação cross-domain |
| 8 | Evidência de conformidade | Governance & Compliance | Audit Service | Evidência + logs |
| 9 | Revisão periódica de acesso | Governance & Compliance | Authorization, Identity | Processo de governança |
| 10 | Log técnico acesso prontuário | — | Audit Service | Execução técnica |

---

## Matriz resumo (35 sub-capabilities)

| # | Core | Sub-capability | Destino | Tipo |
|---|---|---|---|---|
| 1 | Relacionar | Patient Relationship | Patient Relationship | DOM |
| 2 | Relacionar | Professional & Provider Relationship | Professional Management | DOM |
| 3 | Relacionar | Organization & Network Relationship | Organization Management | DOM |
| 4 | Relacionar | Insurance & Payer Relationship | Payer & Insurance | DOM |
| 5 | Organizar | Care Scheduling | Care Coordination | DOM |
| 6 | Organizar | Resource Planning | Care Coordination | DOM |
| 7 | Organizar | Queue & Flow Management | Care Coordination | DOM |
| 8 | Organizar | Referral Coordination | Care Coordination | DOM |
| 9 | Organizar | Operational Workflow | Care Coordination | DOM |
| 10 | Executar | Clinical Attendance | Care Delivery | DOM |
| 11 | Executar | Therapeutic Care Delivery | Care Delivery | DOM |
| 12 | Executar | Diagnostic Service Delivery | Diagnostic Operations | DOM |
| 13 | Executar | Home Care Delivery | Home Care Operations | DOM |
| 14 | Executar | Hospital Care Delivery | Hospital Operations | DEF |
| 15 | Registrar | Clinical Documentation | Clinical Record | DOM |
| 16 | Registrar | Diagnostic Recording | Clinical Record | DOM |
| 17 | Registrar | Prescription & Clinical Orders | Clinical Orders | DOM |
| 18 | Registrar | Clinical Artifacts | Clinical Documents | DOM |
| 19 | Registrar | Clinical Timeline | Clinical Timeline | VIS |
| 20 | Comunicar | Patient Communication | Communication | DOM |
| 21 | Comunicar | Professional Communication | Communication | DOM |
| 22 | Comunicar | Internal Notifications | Notification Service | PS |
| 23 | Comunicar | Communication Preferences & Consent | Communication | DOM |
| 24 | Integrar | Clinical Interoperability | Integration | DOM |
| 25 | Integrar | Diagnostic Systems Integration | Integration | DOM |
| 26 | Integrar | Financial & ERP Integration | — | DEF |
| 27 | Integrar | Webhooks & External APIs | Integration | DOM |
| 28 | Integrar | Medical Devices & IoT | Integration | DOM |
| 29 | Monitorar | Care Pendencies & Follow-up | Care Monitoring | DOM |
| 30 | Monitorar | Clinical Alerts & Protocols | Care Monitoring | DOM |
| 31 | Monitorar | Operational SLAs | Operations Monitoring | DOM |
| 32 | Monitorar | Indicators & Dashboards | Analytics & Reporting | VIS |
| 33 | Monitorar | Intelligence & Automation | — | DEF |
| 34 | Governar | Identity & Access | Identity Service | PS |
| 35 | Governar | Authorization & Permissions | Authorization Service | PS |
| 36 | Governar | Audit & Traceability | Audit Service | PS |
| 37 | Governar | Configuration & Feature Management | Configuration / Feature Flag Service | PS |
| 38 | Governar | Compliance & Data Protection | Governance & Compliance | DOM |
| 39 | Governar | Observability | Observability Service | PS |

**Contagem:** **16 domínios únicos** · 25 mapeamentos DOM · 2 VIS · 4 DEF · 6 PS · 39 sub-capabilities

---

## Validação holística NR-001 — catálogo dos 16 domínios

| # | Domínio | Tipo | Capability primária | Holístico |
|---|---|---|---|---|
| 1 | Care Delivery | Core | Executar | ✅ |
| 2 | Clinical Record | Core | Registrar | ✅ |
| 3 | Clinical Orders | Core | Registrar | ✅ |
| 4 | Clinical Documents | Core | Registrar | ✅ |
| 5 | Care Monitoring | Core | Monitorar | ✅ |
| 6 | Patient Relationship | Supporting | Relacionar | ✅ |
| 7 | Professional Management | Supporting | Relacionar | ✅ |
| 8 | Organization Management | Supporting | Relacionar | ✅ |
| 9 | Payer & Insurance | Supporting | Relacionar | ✅ |
| 10 | Care Coordination | Supporting | Organizar | ✅ |
| 11 | Communication | Supporting | Comunicar | ✅ |
| 12 | Integration | Supporting | Integrar | ✅ |
| 13 | Operations Monitoring | Supporting | Monitorar | ✅ |
| 14 | Diagnostic Operations | Extension | Executar | ✅ |
| 15 | Home Care Operations | Extension | Executar | ✅ |
| 16 | Governance & Compliance | Cross-cutting | Governar | ✅ |

**Read models:** Clinical Timeline, Analytics & Reporting. **Deferred:** Hospital Operations, Billing & Financial, Intelligence & Automation, Financial & ERP Integration.

---

## Perguntas consolidadas para decisão posterior

| ID | Pergunta |
|---|---|
| Q-005 | Financial & ERP — futuro domínio, capability administrativa ou só Integrar? |
| Q-010 | Internal Notifications — Notification Service suficiente? |
| Q-018 | Múltiplas jornadas ativas — impacto em Care Delivery / Clinical Record |
| P-REG-01 | Encaminhamento tri-mode — padrão definido; config institucional Open |
| P-REG-02 | ~~Laudo vs Diagnostic Recording~~ → **Respondida:** Record estruturado, Documents laudo formal |
| P-GOV-01 | ~~Governance & Compliance único~~ → **NR-009 Ready for Confirmation** |
| P-GOV-02 | ~~Split consentimento~~ → **D-010 Ready for Confirmation** |
| P-COM-01 | Responsável legal + resultado — Patient Relationship + Communication |
| P-CLN-01 | Clinical Consent — gate Care Delivery + artefato Clinical Documents |
| P-REG-03 | Vínculo Order ↔ Document quando ordem gera artefato formal |
| P-REG-04 | Resultado externo LIS — handoff Integration → Clinical Record |
| Q-019 | Compliance Service no catálogo ADR-0005 |
| Q-020 | Research Consent — domínio futuro |
| P-MON-01 | Analytics — read model permanente (D-004); produto autônomo Deferred |
| P-MON-02 | Evento dual clínico+operacional — padrão aceito; correlação Open |
| P-ORG-01 | Operational Workflow — fronteira com Configuration Service |
| **NR-001** | Catálogo holístico — **Ready for Confirmation**; Q-002 aguarda confirmação explícita + docs |

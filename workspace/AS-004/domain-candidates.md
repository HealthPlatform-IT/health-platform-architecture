# Domain Candidates — AS-004

> Refinamento por Core Business Capability (2026-07-02).  
> **Validação holística NR-001 concluída** — 16 candidatos **confirmados** em 2026-07-02. Q-002 **Answered**.

## Legenda de status

| Status | Significado |
|---|---|
| **Fortalecido** | Fronteira coesa; alta confiança para Q-002 |
| **Candidato** | Viável; confiança média |
| **Fraco** | Fronteira instável; precisa mais análise |
| **Rejeitado** | Não deve ser Business Domain |
| **Adiado** | Future / Deferred — fora desta rodada |
| **PS** | Platform Service — não domínio |
| **Visão** | Read model / projeção — não domínio |

## Legenda de tipo

**Central** · **Suporte** · **Extensão** · **Transversal (negócio)**

---

## Visão consolidada

| Candidato | Tipo | Status | Confiança |
|---|---|---|---|
| Patient Relationship | Suporte | Fortalecido | Alta |
| Professional Management | Suporte | Fortalecido | Alta |
| Organization Management | Suporte | Fortalecido | Alta |
| Payer & Insurance | Suporte | Fortalecido | Média-alta |
| Care Coordination | Suporte | Fortalecido | Alta |
| Care Delivery | Central | Fortalecido | Alta |
| Diagnostic Operations | Extensão | Fortalecido | Alta |
| Home Care Operations | Extensão | Fortalecido | Alta |
| Hospital Operations | Extensão | Adiado | — |
| Clinical Record | Central | Fortalecido | Alta |
| Clinical Orders | Central | Fortalecido | Alta |
| Clinical Documents | Central | Fortalecido | Alta |
| Clinical Timeline | — | Visão (read model) | Alta |
| Communication | Suporte | Fortalecido | Alta |
| Integration | Suporte | Fortalecido | Média-alta |
| Care Monitoring | Central | Fortalecido | Alta |
| Operations Monitoring | Suporte | Fortalecido | Média-alta |
| Governance & Compliance | Transversal | Fortalecido | Média-alta |
| Analytics & Reporting | — | Visão (read model) | Alta |
| Financial Integration / Billing | — | Adiado | — |

---

## Validação holística NR-001

### Critérios (D-014)

Vocabulário ubíquo · Coesão · Fit com capability · Fronteira clara · Sustentabilidade · Extensibilidade (ADR-0003).

### Resultado

| Grupo | Qtd | Status holístico |
|---|---|---|
| Core | 5 | ✅ Validado |
| Supporting | 8 | ✅ Validado |
| Extension | 2 | ✅ Validado *(Home Care promovido)* |
| Cross-cutting | 1 | ✅ Validado |
| Read models | 2 | ✅ Confirmado (não domínio) |
| Deferred | 2+ | ✅ Registrado |

**16/16 candidatos ativos** passam validação holística. Nenhum ajuste estrutural necessário.

### Cenários holísticos (amostra)

| Cenário | Domínios |
|---|---|
| Jornada + encaminhamento | Patient Relationship, Care Coordination, Care Delivery |
| Consulta completa | Care Delivery, Clinical Record, Clinical Orders, Clinical Documents |
| Lab end-to-end | Clinical Orders, Diagnostic Operations, Integration, Clinical Record, Clinical Documents |
| Alerta + WhatsApp | Care Monitoring, Communication, Clinical Record |
| Visita domiciliar | Home Care Operations, Care Delivery, Clinical Record, Care Monitoring |

### Riscos residuais (não bloqueantes)

Q-005, Q-010, Q-018, Q-019, P-ORG-01, P-REG-*, P-MON-02, P-COM-01.

## 1. Relacionar

### Domínios candidatos

| Domínio | Sub-capabilities | Responsabilidade de negócio |
|---|---|---|
| **Patient Relationship** | Patient Relationship | Vínculo paciente–instituição: cadastro, responsáveis, cuidadores, histórico de relacionamento institucional. **Não** inicia Institution Care Journey (ADR-0007). |
| **Professional Management** | Professional & Provider Relationship | Vínculo com profissionais e prestadores: cadastro, especialidades, credenciamento, alocação institucional. |
| **Organization Management** | Organization & Network Relationship | Estrutura organizacional: instituições, unidades, redes, parcerias entre organizações. |
| **Payer & Insurance** | Insurance & Payer Relationship | Vínculo com pagadores: convênios, planos, cobertura, regras paciente–convênio. |

### Avaliação

| Pergunta | Resposta |
|---|---|
| Domínio, PS, módulo ou visão? | **Domínio** para os quatro candidatos |
| Confiança | **Alta** — 1 sub-capability : 1 domínio; vocabulário ubíquo distinto por ator |
| Fronteiras ambíguas | Patient Relationship vs início da jornada; Payer & Insurance vs futuro domínio financeiro (Q-005) |
| Rejeitados / adiados | **Rejeitado:** domínio único "Relationship" (mega domínio). **Rejeitado:** domínio por tela de cadastro |
| Perguntas pendentes | Q-005 impacta Payer & Insurance? Convênio é só relacionamento ou também faturamento? |

---

## 2. Organizar

### Domínios candidatos

| Domínio | Sub-capabilities | Responsabilidade de negócio |
|---|---|---|
| **Care Coordination** | Care Scheduling, Resource Planning, Queue & Flow Management, Referral Coordination, Operational Workflow *(parcial)* | Planejar e coordenar o cuidado: agenda, recursos, filas, fluxo, encaminhamentos, workflows operacionais configuráveis. |

### Avaliação

| Pergunta | Resposta |
|---|---|
| Domínio, PS, módulo ou visão? | **Care Coordination = Domínio**. Operational Workflow pode ter regras no domínio com execução via Configuration Service |
| Confiança | **Alta** para núcleo (scheduling, queue, referral); **média** para Operational Workflow |
| Fronteiras ambíguas | Care Coordination vs Care Delivery (planejar vs executar); Referral Coordination gera Start Event — fronteira com jornada |
| Rejeitados / adiados | **Rejeitado:** "Scheduling Domain" / "Agenda Domain" (nome de módulo/tela). **Rejeitado:** um domínio por sub-capability (fragmentação) |
| Perguntas pendentes | Operational Workflow: 100% Care Coordination ou extensão transversal? |

---

## 3. Executar

> **NR-004:** Ready for Confirmation ✅ — validado em NR-001 holística.

### Domínios candidatos

| Domínio | Sub-capabilities | Responsabilidade de negócio |
|---|---|---|
| **Care Delivery** | Clinical Attendance, Therapeutic Care Delivery | Realizar interações clínicas e terapêuticas: consultas, teleconsultas, sessões, avaliações. Nível **Attendance** (ADR-0001). |
| **Diagnostic Operations** | Diagnostic Service Delivery | Realizar serviços diagnósticos: coleta, exame, procedimento diagnóstico. |
| **Home Care Operations** | Home Care Delivery | Realizar cuidado domiciliar: visitas, acompanhamento remoto distribuído. |
| **Hospital Operations** | Hospital Care Delivery *(future)* | Alta complexidade hospitalar — **adiado**. |

### Avaliação

| Pergunta | Resposta |
|---|---|
| Domínio, PS, módulo ou visão? | **Domínio** por modelo operacional de execução, não por tela |
| Confiança | **Alta** Care Delivery, Diagnostic Operations e Home Care Operations *(NR-001)* |
| Fronteiras ambíguas | Care Delivery vs Clinical Record (executar vs registrar); Therapeutic dentro de Care Delivery vs domínio próprio |
| Rejeitados / adiados | **Rejeitado:** domínio único "Execution". **Rejeitado:** "Consultation Domain". **Adiado:** Hospital Operations |
| Perguntas pendentes | Telemedicina: apenas tipo de Attendance em Care Delivery? (hipótese: sim) |

---

## 4. Registrar

> **NR-005:** Ready for Confirmation ✅ — decomposição em 3 domínios validada (2026-07-02).

### Domínios candidatos

| Domínio | Sub-capabilities | Responsabilidade de negócio | Pertence | Não pertence |
|---|---|---|---|---|
| **Clinical Record** | Clinical Documentation, Diagnostic Recording | **Conhecimento clínico:** evoluções, anamneses, notas, diagnósticos, hipóteses, conclusões, registro longitudinal, histórico clínico institucional, dados estruturados e narrativos, resultados estruturados registrados | Registros clínicos vivos | Ordens, artefatos formais, laudos, PDFs, Timeline como domínio |
| **Clinical Orders** | Prescription & Clinical Orders | **Intenções e ordens clínicas:** prescrições, solicitações de exame, ordens terapêuticas, encaminhamentos estruturados, ciclo de vida da ordem | Solicitar, ativar, cancelar, renovar, cumprir ordem | Execução, resultado, laudo formal, entrega |
| **Clinical Documents** | Clinical Artifacts | **Artefatos formais:** atestados, laudos, declarações, relatórios formais, termos, consentimentos clínicos formalizados, encaminhamentos como documento | Produção, versionamento, assinatura, validade legal/institucional | Notas de evolução; Document Engine (PS) |
| **Clinical Timeline** | Clinical Timeline | — | — | **Não é domínio** — read model |

### Clinical Record — validação

| Pergunta | Resposta |
|---|---|
| Vocabulário próprio? | Sim — conhecimento clínico, evolução, diagnóstico |
| ≠ prontuário? | Sim — prontuário = visão (Timeline) |
| ≠ Clinical Timeline? | Sim — Record produz; Timeline projeta |
| Diagnostic Recording | **Clinical Record** — achados/conclusões estruturadas; laudo formal → Documents |

### Clinical Orders — validação

| Pergunta | Resposta |
|---|---|
| Prescription + Exam Request juntos? | Sim — mesmo ciclo de vida de ordem |
| Encaminhamento estruturado | Clinical Orders; coordenação → Care Coordination; formal → Documents |
| Relação Care Delivery | Delivery executa e dispara; Orders é dono do lifecycle |
| Relação Clinical Documents | Ordem pode gerar artefato; Documents referencia ordem |

### Clinical Documents — validação

| Pergunta | Resposta |
|---|---|
| Domínio vs Document Engine? | Domínio = regras de artefato; PS = geração técnica |
| Laudo, atestado, consentimento formal | Clinical Documents |
| Encaminhamento formalizado | Clinical Documents |

### Clinical Timeline — revalidação

| Aspecto | Decisão |
|---|---|
| Tipo | Read model / projeção cronológica permanente |
| Alimentado por | Record, Orders, Documents, Delivery, Monitoring, Communication |
| Consumido por | Clinical Workspace, profissionais |
| Domínio próprio? | **Rejeitado** — sem regras autônomas |

### Regras de fronteira (D-012)

1. Clinical Record registra conhecimento clínico.
2. Clinical Orders representa intenções e ordens.
3. Clinical Documents representa artefatos formais assináveis.
4. Document Engine = PS.
5. Clinical Timeline = read model.
6. Care Delivery executa — não é dono primário dos registros.
7. Communication entrega — não é dono do documento.
8. Integration transporta — não é dona do significado clínico.

**Padrão encaminhamento:** Orders (estruturado) + Care Coordination (fluxo) + Documents (formal, opcional).

### Platform Services (Registrar)

| Função | PS |
|---|---|
| Geração de PDF / renderização | Document Engine |
| Templates técnicos | Template Service |
| Projeção timeline / busca | Search Service *(hip.)* |
| Storage de arquivo | Infra PS — não domínio |

### Avaliação Registrar

| Pergunta | Resposta |
|---|---|
| Decomposição 3 domínios? | **Validada** — coerente e sustentável |
| Confiança | **Alta** para os três domínios e Timeline como visão |
| Cenários ambíguos | 6 (encaminhamento), 7 (ordem+doc), 8 (resultado LIS) — risco médio, não invalida |
| Perguntas pendentes | P-REG-01 parcial; P-REG-03 (Order↔Document); P-REG-04 (LIS→Record) |

---

## 5. Comunicar

### Domínios candidatos

| Domínio | Sub-capabilities | Responsabilidade de negócio |
|---|---|---|
| **Communication** | Patient Communication, Professional Communication, Communication Preferences & Consent *(Communication Consent apenas)* | Políticas de comunicação com pessoas; preferências de canal; destinatários autorizados; **Communication Consent**; regras de envio por contexto; histórico de comunicação como conceito de negócio; templates de mensagem *(negócio)*; comunicação paciente/profissional/responsável |

### O que Communication Domain **não** é

| Responsabilidade | Destino |
|---|---|
| Tratamento de dados pessoais (LGPD), retenção, anonimização | Governance & Compliance |
| Auditoria técnica, autorização/permissão | Audit Service, Authorization Service (PS) |
| Execução técnica (WhatsApp, SMS, e-mail, push) | Communication Service (PS) |
| Notificações in-app | Notification Service (PS) |
| Clinical Consent (procedimento, exame) | Clinical Documents + Care Delivery |
| Research Consent | Deferred |

### Platform Services (não domínio)

| Sub-capability / função | Destino |
|---|---|
| Entrega por canal (e-mail, WhatsApp, SMS, push) | **Communication Service** (PS) |
| Notificações in-app | **Notification Service** (PS) |
| Internal Notifications | **Notification Service** (PS) — regra pode originar em Communication ou Care Monitoring |
| Renderização técnica de templates | **Template Service** (PS) — conteúdo/regras no domínio |
| Histórico de entrega técnico | **Communication Service** (PS) — conceito de histórico no domínio |

### Taxonomia de consentimentos (Comunicar + Governar)

| Tipo | Domínio | PS relacionados |
|---|---|---|
| Communication Consent | Communication | Communication Service, Notification Service |
| Data Processing / Privacy Consent | Governance & Compliance | Compliance Service *(hip.)*, Audit Service |
| Clinical Consent | Clinical Documents + Care Delivery | Document Engine |
| Research Consent | Deferred | — |

### Avaliação

| Pergunta | Resposta |
|---|---|
| Comunicar só como PS? | **Rejeitado** — Communication Domain cobre políticas e Communication Consent; PS executa entrega |
| Confiança | **Alta** para Communication Domain *(após refinamento NR-006)* |
| Fronteiras ambíguas | Cenário responsável legal (Patient Relationship + Communication); Clinical Consent fora do escopo |
| Perguntas pendentes | Q-010 (Communication vs Notification Service) — direção alinhada |

---

## 6. Integrar

### Domínios candidatos

| Domínio | Sub-capabilities | Responsabilidade de negócio |
|---|---|---|
| **Integration** | Clinical Interoperability, Diagnostic Systems Integration, Webhooks & External APIs, Medical Devices & IoT | Políticas e contratos de integração: mapeamentos FHIR/TISS, conectores LIS/PACS, webhooks, APIs externas, IoT — **regras de negócio** de interoperabilidade |

### Adiados

| Item | Destino | Motivo |
|---|---|---|
| Financial & ERP Integration | **Adiado — Q-005** | Não decidir Billing/Financial Domain nesta etapa. Registrado como futura integração financeira ou futuro domínio administrativo |

### Platform Services

| Função | Destino |
|---|---|
| Conectores, adapters, protocolos | **Integration Service** (PS) |
| Webhooks técnicos | **Webhook Service** (PS) |

### Avaliação

| Pergunta | Resposta |
|---|---|
| Integration Domain = Integration Service? | **Rejeitado** — domínio define o quê integrar e contratos de negócio; PS implementa como |
| Confiança | **Média-alta** para Integration Domain |
| Perguntas pendentes | Q-005: Financial & ERP → futura capability administrativa, futuro Billing Domain ou permanece só em Integrar? |

---

## 7. Monitorar

> **NR-008:** Ready for Confirmation ✅ — dois domínios validados; Operations Monitoring permanece próprio (2026-07-02).

### Domínios candidatos

| Domínio | Sub-capabilities | Responsabilidade de negócio | Pertence | Não pertence |
|---|---|---|---|---|
| **Care Monitoring** | Care Pendencies & Follow-up, Clinical Alerts & Protocols | Monitorar **cuidado**: pendências clínicas, retornos, protocolos, alertas de risco clínico | Regras de disparo clínico, pendências com significado clínico | SLAs operacionais, entrega de alerta, agenda/fila |
| **Operations Monitoring** | Operational SLAs | Monitorar **operação**: SLAs, prazos institucionais, tempo de espera, metas de unidade | Definição e violação de SLA operacional | Protocolos clínicos, dashboards, coordenação de fluxo |

### Visão / read model

| Item | Sub-capability | Avaliação |
|---|---|---|
| **Analytics & Reporting** | Indicators & Dashboards | **Read model permanente** (D-004) — agrega indicadores de múltiplos domínios |

### Adiados

| Item | Destino |
|---|---|
| Intelligence & Automation | **Adiado — futuro** |

### Decisão: Operations Monitoring absorvido?

| Alternativa | Resultado |
|---|---|
| Absorver em Care Coordination | **Rejeitada** — Organizar ≠ Monitorar |
| Absorver em Care Monitoring | **Rejeitada** — clínico ≠ operacional |
| **Domínio próprio (Supporting)** | **Validada** |

### Regras de fronteira (D-013)

1. Care Monitoring = continuidade e risco **clínico**.
2. Operations Monitoring = SLAs **operacionais**.
3. Analytics = read model — não domínio.
4. Alerta clínico → Monitoring; entrega → Communication/Notification (PS).
5. Fila/agenda → Care Coordination; SLA da fila → Operations Monitoring.
6. Observability Service (PS) ≠ Operations Monitoring.
7. Intelligence & Automation → Deferred.

**Padrão evento dual:** laudo atrasado → Care Monitoring (pendência clínica) + Operations Monitoring (violação SLA).

### Platform Services (Monitorar)

| Função | PS |
|---|---|
| Entrega de alerta in-app | Notification Service |
| Entrega por canal externo | Communication Service |
| Agregação/indicadores | Search Service *(hip.)* — suporte a Analytics |
| Infraestrutura | Observability Service — Governar, não Monitorar |

### Avaliação Monitorar

| Pergunta | Resposta |
|---|---|
| Dois domínios? | **Validado** — Care Monitoring (Core) + Operations Monitoring (Supporting) |
| Confiança | **Alta** Care Monitoring; **Média-alta** Operations Monitoring |
| Cenários ambíguos | Cenário 5 (laudo atrasado) — evento dual; padrão aceito |
| Rejeitados | Mega domínio "Monitoring"; Analytics como DOM; absorção de Operations Monitoring |
| Perguntas pendentes | P-MON-01 parcial; P-MON-02 (correlação evento dual) |

---

## 8. Governar

### Princípio

**Governar** é capability **transversal**. Implementação técnica via **Platform Services**; políticas e obrigações de conformidade via **Governance & Compliance Domain**.

### Regra de fronteira

```text
Governance & Compliance  →  políticas, obrigações, evidências de conformidade
Platform Services        →  enforcement técnico, rastreabilidade, autorização, configuração
Domínios de negócio      →  consomem políticas e PS sem reimplementar transversais
```

### Platform Services (não domínio)

| Sub-capability | Platform Service |
|---|---|
| Identity & Access | Identity Service |
| Authorization & Permissions | Authorization Service |
| Audit & Traceability | Audit Service |
| Configuration & Feature Management | Configuration Service, Feature Flag Service |
| Observability | Observability Service |

### Domínio candidato (responsabilidade de negócio)

| Domínio | Sub-capability fonte | Responsabilidade de negócio |
|---|---|---|
| **Governance & Compliance** | Compliance & Data Protection *(parcial)* | Políticas institucionais de conformidade; governança LGPD; retenção e anonimização *(políticas)*; base legal; evidências de conformidade; **Data Processing Consent**; revisão periódica de acesso *(processo)*; compartilhamento de dados; requisitos regulatórios |

### O que Governance & Compliance **não** implementa

Autenticação, autorização técnica, logs técnicos, audit trail, observabilidade, feature flags, configuração técnica, envio de mensagens — todos **Platform Services**.

**Hipótese PS:** Compliance Service — enforcement técnico de políticas definidas pelo domínio.

### Avaliação

| Pergunta | Resposta |
|---|---|
| Governance & Compliance como domínio único? | **Sim** — Cross-cutting Business Domain *(NR-009 refinado)* |
| Compliance Service como PS? | Hipótese — enforcement técnico; domínio define políticas |
| Confiança | **Alta** para mapeamento PS; **Média-alta** para Governance & Compliance Domain |
| Fronteiras ambíguas | Orquestração LGPD (cenário 3) envolve Clinical Record/Documents para execução |
| Rejeitados | **Rejeitado:** Identity, Authorization, Audit, Configuration como domínios |
| Perguntas pendentes | Q-019 — Compliance Service no catálogo ADR-0005 |

---

## Resumo de rejeições e adiamentos

| Item | Tratamento |
|---|---|
| Clinical Timeline como domínio | **Rejeitado** → visão/read model |
| Analytics & Dashboards como domínio | **Fraco** → visão/read model |
| Mega domínios (Relationship, Clinical, Monitoring, Execution) | **Rejeitados** |
| Domínios por tela/módulo | **Rejeitados** |
| Hospital Operations | **Adiado** |
| Intelligence & Automation | **Adiado** |
| Financial & ERP Integration / Billing | **Adiado — Q-005** |

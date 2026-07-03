# Decision Package — AS-004

> **Status do pacote:** **Confirmado** em 2026-07-02.  
> **Confirmação:** Pacote AS-004 confirmado explicitamente pelo usuário.  
> **Formalização:** [ADR-0008](../../docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md) · [`business-domains.md`](../../docs/04-domain/business-domains.md) · [`domain-map.md`](../../docs/04-domain/domain-map.md)  
> **Documento de confirmação:** [`confirmation-package.md`](confirmation-package.md)

---

## 1. Purpose

Consolidar o resultado da investigação de fronteiras da **AS-004 — Business Domain Map**, separando:

- decisões maduras prontas para confirmação;
- hipóteses e candidatos que exigem revisão;
- itens adiados e riscos removidos;
- impacto em open questions — **sem** fechar Q-002 prematuramente.

Este pacote **não** define implementação (módulos, APIs, código).

### Distinções preservadas

| Conceito | Representa |
|---|---|
| **Business Domain** | Responsabilidade de negócio coesa com fronteira e vocabulário próprios |
| **Platform Service** | Implementação reutilizável e transversal (ADR-0005) |
| **Read Model / View** | Projeção de leitura cross-domain — não possui regras de negócio autônomas |
| **Module** | Solução implementável futura — derivada de domínio + capability |
| **Feature** | Funcionalidade futura — derivada de módulo |

---

## 2. Decision Status Legend

| Status | Significado |
|---|---|
| **Ready for Confirmation** | Maduro; pode ser confirmado sem nova Architecture Session |
| **Needs Review** | Direção forte; exige validação antes de confirmação |
| **Deferred** | Conscientemente adiado — não decidir nesta fase |
| **Rejected** | Alternativa descartada |

---

## 3. Ready for Confirmation

### D-001 — Modelo de decomposição

**Status:** Ready for Confirmation

Business Domains emergem do **agrupamento de sub-capabilities relacionadas** (Alternativa C), combinado com **responsabilidades transversais via Platform Services** (Alternativa E).

**Não adotar:** 8 domínios espelhando cores · ~35 domínios · domínios por modelo operacional · domínios por tela/módulo.

---

### D-002 — Regra Domain vs Platform Service vs Read Model (Q-007 parcial)

**Status:** Ready for Confirmation

| Pergunta | Destino |
|---|---|
| Regras de negócio e vocabulário ubíquo próprios? | Business Domain |
| Responsabilidade transversal, reutilizável e técnica? | Platform Service |
| Agregação de leitura cross-domain? | Read Model / View |
| Domínio consome PS? | Sim — domínios **não reimplementam** transversais |

---

### D-003 — Clinical Timeline: Read Model / View

**Status:** Ready for Confirmation

**Clinical Timeline** não é Business Domain. É **visão/read model** — projeção cronológica de eventos e artefatos de múltiplos domínios.

O prontuário é uma **visão** da timeline institucional — não domínio nem centro da plataforma.

---

### D-004 — Analytics & Reporting: Read Model / View (fase atual)

**Status:** Ready for Confirmation

**Indicators & Dashboards** não é Business Domain nesta fase. É **visão/read model** agregando indicadores de múltiplos domínios.

Futuro domínio Analytics só se surgir responsabilidade de negócio autônoma — hoje **Deferred**.

---

### D-005 — Comunicar não é apenas Platform Service

**Status:** Ready for Confirmation

A capability **Comunicar** exige responsabilidade de negócio além da entrega técnica. Platform Services (Communication Service, Notification Service) **executam**; políticas de comunicação pertencem a um domínio candidato.

---

### D-006 — Integrar não é apenas Platform Service

**Status:** Ready for Confirmation

A capability **Integrar** exige políticas de interoperabilidade e contratos de negócio. Integration Service e Webhook Service **implementam** conectores — não substituem o domínio candidato.

---

### D-007 — Internal Notifications → Notification Service

**Status:** Ready for Confirmation

Sub-capability **Internal Notifications** mapeia para **Notification Service** (PS), conforme direção ADR-0005. Regras de negócio que disparam a notificação podem originar em domínios (ex.: Care Monitoring).

---

### D-008 — Governar: sub-capabilities técnicas → Platform Services

**Status:** Ready for Confirmation

| Sub-capability | Destino |
|---|---|
| Identity & Access | Identity Service |
| Authorization & Permissions | Authorization Service |
| Audit & Traceability | Audit Service |
| Configuration & Feature Management | Configuration Service, Feature Flag Service |
| Observability | Observability Service |

Essas sub-capabilities **não** são Business Domains.

---

### D-009 — Alternativas estruturais rejeitadas

**Status:** Rejected (registro permanente)

| Alternativa | Motivo |
|---|---|
| 8 domínios = 8 Core Business Capabilities | Ignora granularidade do mapa |
| ~35 domínios = 1 por sub-capability | Fragmentação excessiva |
| Domínios por modelo operacional | Duplicação; viola base comum + extensão (ADR-0003) |
| Domínios por tela/módulo | Viola ADR-0002 |
| Clinical Timeline como domínio | Read model |
| Analytics como domínio (fase atual) | Read model |
| Mega domínios (Clinical, Relationship, Monitoring, Execution) | Baixa coesão |
| Identity / Authorization / Audit / Configuration como domínios | Platform Services |
| Comunicar inteiro como PS | Perde regras de negócio |
| Integrar inteiro como PS | Perde contratos de interoperabilidade |

---

### NR-006 — Communication Domain

**Status:** Ready for Confirmation ✅

Domínio de **negócio** para políticas de comunicação, preferências de canal, **Communication Consent**, destinatários autorizados, templates de negócio e histórico de comunicação como conceito institucional. Execução técnica via Communication Service e Notification Service (PS).

**Detalhe completo:** seção 4 (refinamento 2026-07-02).

**Open Questions residuais:** Q-010 (Communication vs Notification Service).

---

### NR-009 — Governance & Compliance Domain

**Status:** Ready for Confirmation ✅

**Cross-cutting Business Domain** para políticas institucionais, LGPD, retenção, anonimização, **Data Processing Consent**, evidências de conformidade e revisão periódica de acesso como processo de governança. Enforcement técnico via Platform Services — não implementação no domínio.

**Detalhe completo:** seção 4 (refinamento 2026-07-02).

**Open Questions residuais:** Q-019 (Compliance Service no catálogo ADR-0005).

---

### D-010 — Taxonomia de consentimentos

**Status:** Ready for Confirmation

Quatro tipos distintos e não intercambiáveis: Communication Consent, Data Processing/Privacy Consent, Clinical Consent, Research Consent (deferred).

**Detalhe completo:** seção 4.

**Open Questions residuais:** Q-020 (Research Consent — domínio futuro).

---

### D-011 — Regra de fronteira Governança

**Status:** Ready for Confirmation

Governance & Compliance define políticas; Platform Services executam enforcement; domínios consomem sem reimplementar transversais.

**Detalhe completo:** seção 4.

---

### NR-005 — Registrar: Clinical Record / Orders / Documents

**Status:** Ready for Confirmation ✅

Três domínios clínicos validados; Clinical Timeline permanece read model. Regras D-012.

**Detalhe completo:** seção 4 (refinamento 2026-07-02).

**Open Questions residuais:** P-REG-01 (parcial), P-REG-03, P-REG-04.

---

### D-012 — Regras de fronteira Registrar

**Status:** Ready for Confirmation

Oito regras de fronteira entre Record, Orders, Documents, Timeline, Delivery, Communication e Integration.

**Detalhe completo:** seção 4 (NR-005).

---

### NR-008 — Monitorar: Care Monitoring + Operations Monitoring

**Status:** Ready for Confirmation ✅

Dois domínios distintos validados. **Operations Monitoring permanece domínio próprio** — não absorvido por Care Coordination nem Care Monitoring. **Analytics & Reporting** permanece read model (D-004).

**Detalhe completo:** seção 4 (refinamento 2026-07-02).

**Open Questions residuais:** P-MON-01 (Analytics permanente); P-MON-02 (evento dual clínico+operacional).

---

### D-013 — Regras de fronteira Monitorar

**Status:** Ready for Confirmation

Sete regras de fronteira entre Care Monitoring, Operations Monitoring, Analytics, Communication e Care Coordination.

**Detalhe completo:** seção 4 (NR-008).

---

### NR-001 — Catálogo holístico dos 16 Business Domain Candidates

**Status:** Ready for Confirmation ✅

Validação holística do catálogo concluída. **16 candidatos ativos** coerentes, completos e sustentáveis. **2 read models** e **2 deferred** fora do catálogo ativo.

**Detalhe completo:** seção 4 (refinamento 2026-07-02).

**Open Questions residuais:** Q-005, Q-010, Q-018, Q-019, P-ORG-01, P-REG-*, P-MON-02, P-COM-01 — não bloqueiam confirmação do catálogo.

---

### D-014 — Critérios de validação holística do catálogo

**Status:** Ready for Confirmation

Seis critérios aplicados a cada candidato: vocabulário ubíquo, coesão, fit com capability, alinhamento ADR, ausência de overlap com PS, sustentabilidade.

**Detalhe completo:** seção 4 (NR-001).

---

## 4. Needs Review

> Hipóteses e candidatos consolidados — **não** são decisão final absoluta.

### NR-001 — Business Domain Candidates consolidados (16) *(revisado — validação holística)*

**Status:** Ready for Confirmation ✅

#### Objetivo da validação holística

Confirmar que o catálogo de **16 Business Domain Candidates** é:

- **Completo** — todas as sub-capabilities de negócio mapeadas (exceto deferred);
- **Coeso** — cada domínio com vocabulário e responsabilidade distintos;
- **Sustentável** — granularidade adequada (nem mega domínios, nem fragmentação);
- **Alinhado** — ADR-0001 (hierarquia), ADR-0002 (capability-driven), ADR-0003 (core + extensão), ADR-0005 (PS), ADR-0007 (jornada — referência apenas).

---

#### D-014 — Critérios de validação holística

| # | Critério | Pergunta |
|---|---|---|
| C1 | **Vocabulário ubíquo** | O domínio possui linguagem de negócio própria e reconhecível? |
| C2 | **Coesão** | As sub-capabilities agrupadas compartilham responsabilidade coerente? |
| C3 | **Fit com capability** | O agrupamento respeita a Core Business Capability fonte? |
| C4 | **Fronteira clara** | Há distinção explícita de domínios vizinhos e PS? |
| C5 | **Sustentabilidade** | O domínio sobrevive sem virar módulo de tela ou PS disfarçado? |
| C6 | **Extensibilidade** | Extensões (ADR-0003) não duplicam o core? |

**Regra:** candidato que falha em C1 ou C5 é rejeitado; falha parcial em C4 gera Open Question, não rejeição.

---

#### Matriz holística — 16 candidatos

| # | Candidato | Tipo | Capability | C1–C6 | Confiança | NR / origem |
|---|---|---|---|---|---|---|
| 1 | **Care Delivery** | Core | Executar | ✅ | Alta | NR-004 |
| 2 | **Clinical Record** | Core | Registrar | ✅ | Alta | NR-005 |
| 3 | **Clinical Orders** | Core | Registrar | ✅ | Alta | NR-005 |
| 4 | **Clinical Documents** | Core | Registrar | ✅ | Alta | NR-005 |
| 5 | **Care Monitoring** | Core | Monitorar | ✅ | Alta | NR-008 |
| 6 | **Patient Relationship** | Supporting | Relacionar | ✅ | Alta | NR-002 |
| 7 | **Professional Management** | Supporting | Relacionar | ✅ | Alta | NR-002 |
| 8 | **Organization Management** | Supporting | Relacionar | ✅ | Alta | NR-002 |
| 9 | **Payer & Insurance** | Supporting | Relacionar | ✅ | Média-alta | NR-002 *(Q-005 residual)* |
| 10 | **Care Coordination** | Supporting | Organizar | ✅ | Alta | NR-003 *(P-ORG-01 residual)* |
| 11 | **Communication** | Supporting | Comunicar | ✅ | Alta | NR-006 |
| 12 | **Integration** | Supporting | Integrar | ✅ | Média-alta | NR-007 |
| 13 | **Operations Monitoring** | Supporting | Monitorar | ✅ | Média-alta | NR-008 |
| 14 | **Diagnostic Operations** | Extension | Executar | ✅ | Alta | NR-004 |
| 15 | **Home Care Operations** | Extension | Executar | ✅ | Média-alta | NR-004 *(validado na holística)* |
| 16 | **Governance & Compliance** | Cross-cutting | Governar | ✅ | Alta | NR-009 |

**Resultado:** 16/16 candidatos passam nos critérios holísticos.

---

#### Fora do catálogo ativo (confirmado)

| Item | Tipo | Motivo |
|---|---|---|
| **Clinical Timeline** | Read model | D-003 — projeção cross-domain |
| **Analytics & Reporting** | Read model | D-004 — agregação de indicadores |
| **Hospital Operations** | Deferred | DEF-001 — escopo futuro |
| **Billing & Financial** | Deferred | DEF-002 — Q-005 Open |
| **Intelligence & Automation** | Deferred | DEF-004 — escopo futuro |
| **Financial & ERP Integration** | Deferred | DEF-003 — aguarda Q-005 |

---

#### Completude — cobertura de sub-capabilities

| Capability | Sub-capabilities | DOM | PS | VIS | DEF |
|---|---|---|---|---|---|
| Relacionar | 4 | 4 | — | — | — |
| Organizar | 5 | 5 | — | — | — |
| Executar | 5 | 3 | — | — | 2 *(Hospital, —)* |
| Registrar | 5 | 3 | — | 1 | — |
| Comunicar | 4 | 3 | 1 | — | — |
| Integrar | 5 | 4 | — | — | 1 |
| Monitorar | 5 | 2 | — | 1 | 1 |
| Governar | 6 | 1 | 5 | — | — |
| **Total** | **39** | **25** | **6** | **2** | **4** |

> Contagem de DOM reflete ownership de negócio; sub-capabilities podem ter regras originadas em domínios e execução em PS.

**Gap analysis:** nenhuma sub-capability de negócio ativa sem destino. Deferred e PS explicitamente registrados.

---

#### Validação por grupo (síntese NR-002 a NR-009)

| NR | Capability | Decisão holística | Status |
|---|---|---|---|
| NR-002 | Relacionar | 4 domínios de vínculo — 1:1 com sub-capabilities | ✅ Validado |
| NR-003 | Organizar | Care Coordination único — inclui Operational Workflow | ✅ Validado |
| NR-004 | Executar | Care Delivery + 2 extensões (Diagnostic, Home Care); telemedicina em Delivery | ✅ Validado |
| NR-005 | Registrar | Record / Orders / Documents + Timeline visão | ✅ Ready |
| NR-006 | Comunicar | Communication Domain + PS entrega | ✅ Ready |
| NR-007 | Integrar | Integration Domain; financeiro deferred | ✅ Validado |
| NR-008 | Monitorar | Care Monitoring + Operations Monitoring; Analytics visão | ✅ Ready |
| NR-009 | Governar | Governance & Compliance; demais → PS | ✅ Ready |

**NR-004 — Home Care Operations (decisão na holística):**

| Alternativa | Avaliação |
|---|---|
| Absorver em Care Delivery | **Rejeitada** — mistura execução ambulatorial com modelo domiciliar estendido |
| Domínio Extension próprio | **Validada** — alinhado ADR-0003; vocabulário: visita domiciliar, equipe de campo, cuidado remoto distribuído |
| Telemedicina em Home Care | **Rejeitada** — teleconsulta permanece Attendance em Care Delivery |

---

#### Cenários holísticos cross-domain

| # | Cenário | Domínios envolvidos | Coerência | Risco |
|---|---|---|---|---|
| H1 | Paciente inicia jornada após aceite de encaminhamento | Patient Relationship, Care Coordination, Care Delivery | ✅ | Baixo |
| H2 | Consulta ambulatorial com evolução, prescrição e atestado | Care Delivery, Clinical Record, Clinical Orders, Clinical Documents | ✅ | Baixo |
| H3 | Exame solicitado, executado em lab, resultado e laudo | Clinical Orders, Diagnostic Operations, Clinical Record, Clinical Documents, Integration | ✅ | Médio *(P-REG-04)* |
| H4 | Alerta de resultado crítico e lembrete WhatsApp | Care Monitoring, Communication, Clinical Record | ✅ | Baixo |
| H5 | Fila excede SLA e gestor vê dashboard | Operations Monitoring, Care Coordination, Analytics & Reporting | ✅ | Baixo |
| H6 | Visita domiciliar com registro e monitoramento | Home Care Operations, Care Delivery, Clinical Record, Care Monitoring | ✅ | Médio *(fronteira Home Care/Delivery)* |
| H7 | Compartilhamento FHIR com parceiro externo | Integration, Clinical Record, Governance & Compliance | ✅ | Baixo |
| H8 | Auditoria LGPD com evidências e logs | Governance & Compliance, Clinical Record/Documents, Audit Service | ✅ | Baixo |

**Resultado:** catálogo suporta jornadas end-to-end sem domínio faltante.

---

#### Riscos residuais do catálogo (não bloqueantes)

| ID | Risco | Impacto no catálogo |
|---|---|---|
| Q-005 | Billing/Financial futuro | Payer & Insurance permanece; Billing deferred |
| Q-018 | Múltiplas jornadas ativas | Não altera catálogo; impacto operacional futuro |
| Q-010 | Communication vs Notification | Não altera existência do domínio Communication |
| P-ORG-01 | Operational Workflow vs Configuration | Fronteira operacional; não invalida Care Coordination |
| P-REG-03/04 | Vínculos cross-domain Registrar | Padrões definidos em D-012 |
| P-MON-02 | Evento dual clínico+operacional | Padrão definido em D-013 |
| P-COM-01 | Responsável legal + comunicação | Coordenação Patient Relationship + Communication |

---

#### Conclusão NR-001

O catálogo de **16 Business Domain Candidates** está **pronto para confirmação explícita**. Não requer ajuste estrutural (adição, remoção ou fusão de domínios). Open Questions residuais são de **detalhe de fronteira ou implementação futura** — não de existência do catálogo.

> **Q-002 não marcada como Answered** — requer confirmação explícita do usuário + documentação oficial/ADR.

---

### NR-002 — Relacionar: quatro domínios de vínculo

**Status:** Ready for Confirmation ✅ *(validado em NR-001 holística)*

| Sub-capability | Candidato |
|---|---|
| Patient Relationship | Patient Relationship |
| Professional & Provider Relationship | Professional Management |
| Organization & Network Relationship | Organization Management |
| Insurance & Payer Relationship | Payer & Insurance |

**Hipótese:** Payer & Insurance permanece relacionamento; faturamento **não** pertence aqui (ver Deferred — Billing/Financial).

**Revisar:** fronteira Patient Relationship vs Institution Care Journey (referência conceitual em `care-journey-lifecycle.md` / ADR-0007 — **sem** importar regras de cardinalidade para esta sessão).

---

### NR-003 — Organizar: Care Coordination

**Status:** Ready for Confirmation ✅ *(validado em NR-001 holística)*

**Hipótese:** todas as sub-capabilities de Organizar em **Care Coordination**, incluindo Operational Workflow.

| Responsabilidade | Candidato domínio | PS |
|---|---|---|
| Workflows operacionais de negócio | Care Coordination | Configuration Service parametriza |
| Agenda, filas, encaminhamentos, recursos | Care Coordination | — |

---

### NR-004 — Executar: domínios por execução operacional

**Status:** Ready for Confirmation ✅ *(validado em NR-001 holística)*

| Sub-capability | Candidato |
|---|---|
| Clinical Attendance, Therapeutic Care Delivery | Care Delivery |
| Diagnostic Service Delivery | Diagnostic Operations |
| Home Care Delivery | Home Care Operations |

**Hipótese:** telemedicina = tipo de Attendance em Care Delivery (sem domínio próprio).

---

### NR-005 — Registrar: três domínios clínicos *(revisado)*

**Status:** Ready for Confirmation

A decomposição **Clinical Record / Clinical Orders / Clinical Documents** foi **validada** como coerente, coesa e sustentável. **Clinical Timeline** permanece read model — não domínio.

#### Parte 1 — Clinical Record

| Pergunta | Resposta |
|---|---|
| Vocabulário e regras próprias? | **Sim** — conhecimento clínico, evolução, anamnese, diagnóstico, hipótese, conclusão, registro longitudinal |
| Diferente de prontuário? | **Sim** — prontuário = **visão** (Clinical Timeline); Clinical Record = domínio fonte de conhecimento clínico |
| Diferente de Clinical Timeline? | **Sim** — Record **produz** registros; Timeline **projeta** cronologicamente eventos de múltiplos domínios |
| Clinical Timeline como read model? | **Sim** — permanente nesta fase (D-003 reforçado) |
| Diagnostic Recording onde? | **Clinical Record** — achados, impressões e conclusões diagnósticas **estruturadas**; **laudo formal** → Clinical Documents |

**Pertence:** evoluções, anamneses, notas, diagnósticos, hipóteses, conclusões clínicas, registro longitudinal, histórico clínico institucional, dados estruturados e narrativos, **resultados estruturados** recebidos e registrados como conhecimento clínico.

**Não pertence:** prescrições, solicitações, ordens, atestados, laudos formais, PDFs, geração técnica, Timeline como domínio.

**Sub-capabilities:** Clinical Documentation, Diagnostic Recording.

---

#### Parte 2 — Clinical Orders

| Pergunta | Resposta |
|---|---|
| Prescription e Exam Request no mesmo domínio? | **Sim** — ambos são **ordens clínicas** com ciclo de vida (solicitar, ativar, cancelar, renovar, cumprir) |
| Encaminhamento estruturado? | **Clinical Orders** quando é ordem clínica estruturada; **Care Coordination** quando é fluxo operacional; **Clinical Documents** quando é artefato formal — podem coexistir |
| Solicitação vira documento formal? | Quando a instituição exige artefato assinável: **Clinical Orders** mantém a ordem; **Clinical Documents** produz o artefato (referência à ordem, sem duplicar lifecycle) |
| Relação com Care Delivery? | Care Delivery **executa** o cuidado e **dispara** ordens; Clinical Orders **é dono** do lifecycle da ordem |
| Relação com Clinical Documents? | Ordem pode **gerar** documento formal; Documents referencia ordem; Orders não é dono do artefato |

**Pertence:** prescrições, solicitações de exame, ordens terapêuticas, encaminhamentos estruturados como ordem, ciclo de vida e status da ordem.

**Não pertence:** execução de exame/procedimento (Diagnostic Operations, Care Delivery), resultado diagnóstico (Clinical Record), laudo formal (Clinical Documents), entrega ao paciente (Communication).

**Sub-capability:** Prescription & Clinical Orders.

---

#### Parte 3 — Clinical Documents

| Pergunta | Resposta |
|---|---|
| Domínio de negócio ou só Document Engine? | **Domínio de negócio** — regras de artefato, validade, assinatura, versionamento |
| Diferença de Document Engine? | Documents = **o quê** formalizar e **quando** é válido; Document Engine (PS) = **como** gerar PDF/renderizar |
| Laudo? | **Clinical Documents** — artefato formal assinável |
| Atestado? | **Clinical Documents** |
| Consentimento clínico formalizado? | **Clinical Documents** *(artefato)* + Care Delivery *(gate clínico)* |
| Encaminhamento formalizado? | **Clinical Documents** — carta/termo formal; ordem estruturada permanece em Clinical Orders |

**Pertence:** atestados, laudos formais, declarações, relatórios médicos formais, termos, consentimentos clínicos formalizados, encaminhamentos como documento, artefatos assináveis com validade legal/institucional.

**Não pertence:** evolução comum, ordem sem artefato formal, PDF técnico, storage, envio (Communication).

**Sub-capability:** Clinical Artifacts.

---

#### Parte 4 — Clinical Timeline (revalidação)

| Pergunta | Resposta |
|---|---|
| Motivo forte para virar domínio? | **Não** — sem vocabulário autônomo nem regras de negócio exclusivas; risco de duplicar fontes |
| Domínios que alimentam | Clinical Record, Clinical Orders, Clinical Documents, Care Delivery, Care Monitoring, Communication *(eventos)* |
| Quem consome | Clinical Workspace, profissionais de saúde, possíveis visões para paciente |
| Read model permanente? | **Sim** — projeção cronológica; possível módulo de leitura ou Search Service no futuro |

---

#### D-012 — Regras de fronteira Registrar *(Ready for Confirmation)*

| # | Regra |
|---|---|
| 1 | **Clinical Record** registra conhecimento clínico |
| 2 | **Clinical Orders** representa intenções, solicitações e ordens clínicas |
| 3 | **Clinical Documents** representa artefatos formais, assináveis, legais ou institucionais |
| 4 | **Document Engine** não é domínio — é Platform Service |
| 5 | **Clinical Timeline** não é domínio — é read model/projeção |
| 6 | **Care Delivery** executa o cuidado — não é dono primário dos registros formais |
| 7 | **Communication** entrega documentos — não é dono do documento |
| 8 | **Integration** recebe/envia dados externos — não é dona do significado clínico interno |

**Padrão encaminhamento (tri-mode):**

| Modo | Domínio |
|---|---|
| Ordem clínica estruturada | Clinical Orders |
| Coordenação de fluxo (agenda, fila) | Care Coordination |
| Documento formal assinável | Clinical Documents |

---

#### Cenários de validação Registrar

| # | Cenário | Domínio principal | Envolvidos | PS | Natureza | Risco |
|---|---|---|---|---|---|---|
| 1 | Evolução clínica na consulta | Clinical Record | Care Delivery | — | Registro | Baixo |
| 2 | Hipótese diagnóstica | Clinical Record | — | — | Registro | Baixo |
| 3 | Solicitação exame laboratorial | Clinical Orders | Care Delivery | — | Ordem | Baixo |
| 4 | Prescrição de medicamento | Clinical Orders | — | — | Ordem | Baixo |
| 5 | Emissão de atestado | Clinical Documents | — | Document Engine | Documento | Baixo |
| 6 | Encaminhamento para especialista | Clinical Orders + Care Coordination | Clinical Documents *(se formal)* | Document Engine | Ordem + coordenação + doc. opcional | Médio |
| 7 | Pedido de exame + documento formal | Clinical Orders | Clinical Documents | Document Engine | Ordem + documento derivado | Médio |
| 8 | Laboratório publica resultado estruturado | Clinical Record | Integration, Diagnostic Operations | Integration Service | Registro + integração | Médio |
| 9 | Laudo formal | Clinical Documents | Clinical Record *(achados fonte)* | Document Engine | Documento | Baixo |
| 10 | Consentimento informado procedimento | Clinical Documents | Care Delivery | Document Engine | Documento + gate clínico | Médio |
| 11 | Timeline clínica consolidada | — *(read model)* | múltiplos domínios | Search Service *(hip.)* | Read model | Baixo |
| 12 | Paciente recebe documento por WhatsApp | Communication | Clinical Documents | Communication Service, Document Engine | Comunicação — doc. pertence a Documents | Baixo |

**Risco residual:** cenários 6, 7, 8 — coordenação cross-domain; não invalidam a decomposição.

---

### NR-006 — Communication Domain *(revisado — Ready for Confirmation ✅)*

**Status:** Ready for Confirmation ✅ *(promovido da revisão 2026-07-02)*

O **Communication Domain** é domínio de **negócio** — não substituto do Communication Service nem Notification Service.

#### Pertence ao Communication Domain

| Responsabilidade | Exemplos |
|---|---|
| Políticas de comunicação com pessoas | O que comunicar, a quem, quando, em qual contexto |
| Preferências de canal | Opt-in WhatsApp, preferência e-mail |
| Destinatários autorizados | Quem pode receber resultado, alerta ou mensagem |
| **Communication Consent** | Consentimento/preferência de **canal** |
| Regras de envio por contexto | Lembrete de consulta, resultado disponível, alerta operacional |
| Histórico de comunicação *(conceito de negócio)* | Registro institucional do que foi comunicado, a quem e por qual regra |
| Templates de mensagem *(negócio)* | Conteúdo e regras de uso de templates — não renderização técnica |
| Comunicação paciente / profissional / responsável | Patient Communication, Professional Communication |

#### Não pertence ao Communication Domain

| Responsabilidade | Destino |
|---|---|
| Tratamento de dados pessoais (LGPD) | Governance & Compliance |
| Retenção legal de dados | Governance & Compliance |
| Anonimização | Governance & Compliance |
| Auditoria técnica / audit trail | Audit Service (PS) |
| Autorização / permissão técnica | Authorization Service (PS) |
| Execução técnica de envio (WhatsApp, SMS, e-mail, push) | Communication Service (PS) |
| Notificações in-app | Notification Service (PS) |
| **Clinical Consent** (procedimento, exame, intervenção) | Clinical Documents + Care Delivery |
| **Research Consent** | Deferred — domínio futuro |

#### Sub-capabilities → Communication Domain

| Sub-capability | Candidato |
|---|---|
| Patient Communication | Communication |
| Professional Communication | Communication |
| Communication Preferences & Consent | Communication *(apenas Communication Consent)* |
| Internal Notifications | **Notification Service (PS)** — regra de negócio pode originar em Communication ou Care Monitoring |

**Risco residual:** Q-010 (fronteira Communication Service vs Notification Service) — direção alinhada; formalização em sessão futura.

---

### NR-007 — Integrar: Integration Domain (exceto financeiro)

**Status:** Ready for Confirmation ✅ *(validado em NR-001 holística)*

| Sub-capability | Candidato |
|---|---|
| Clinical Interoperability | Integration |
| Diagnostic Systems Integration | Integration |
| Webhooks & External APIs | Integration |
| Medical Devices & IoT | Integration |

---

### NR-008 — Monitorar: Care Monitoring + Operations Monitoring *(revisado)*

**Status:** Ready for Confirmation ✅

A capability **Monitorar** gera **dois domínios de negócio** + **um read model** + **um item deferred**.

| Destino | Sub-capabilities | Tipo |
|---|---|---|
| **Care Monitoring** | Care Pendencies & Follow-up, Clinical Alerts & Protocols | Core Business Domain |
| **Operations Monitoring** | Operational SLAs | Supporting Business Domain |
| **Analytics & Reporting** | Indicators & Dashboards | Read model / visão (D-004) |
| — | Intelligence & Automation | Deferred |

#### Decisão central: Operations Monitoring — domínio próprio ou absorvido?

| Alternativa | Avaliação |
|---|---|
| Absorver em **Care Coordination** | **Rejeitada** — mistura Organizar com Monitorar (ADR-0002); SLA é medição, não coordenação de fluxo |
| Absorver em **Care Monitoring** | **Rejeitada** — SLA operacional ≠ protocolo clínico; vocabulários distintos |
| **Domínio próprio (Supporting)** | **Validada** — 1 sub-capability : 1 domínio; responsabilidade coesa de métricas e SLAs operacionais |

---

#### Parte 1 — Care Monitoring

| Pergunta | Resposta |
|---|---|
| Vocabulário e regras próprias? | **Sim** — pendência clínica, retorno, protocolo clínico, alerta de risco, acompanhamento de condição |
| Diferente de Care Coordination? | **Sim** — Coordination **organiza** (agenda, fila); Monitoring **detecta** pendências e dispara alertas clínicos |
| Diferente de Communication? | **Sim** — Monitoring define **quando** e **por quê** alertar; Communication/Notification **entrega** |
| Diferente de Operations Monitoring? | **Sim** — foco no **significado clínico** do atraso/risco, não na métrica operacional institucional |

**Pertence:** pendências clínicas, retornos previstos não realizados, protocolos clínicos, alertas de resultado crítico, lembretes de acompanhamento de condição, regras de disparo clínico.

**Não pertence:** SLAs operacionais institucionais, tempo de fila como métrica, dashboards agregados, entrega técnica de alerta, agendamento de retorno (Care Coordination), registro clínico (Clinical Record).

**Sub-capabilities:** Care Pendencies & Follow-up, Clinical Alerts & Protocols.

---

#### Parte 2 — Operations Monitoring

| Pergunta | Resposta |
|---|---|
| Vocabulário e regras próprias? | **Sim** — SLA, prazo operacional, meta de atendimento, tempo de espera, produtividade de unidade |
| Por que domínio próprio com 1 sub-capability? | Responsabilidade coesa e distinta; alinhada ao mapa de capabilities; evita contaminar Organizar ou núcleo clínico |
| Diferente de Observability Service? | **Sim** — Observability = saúde **técnica** da plataforma (Governar/PS); Operations Monitoring = SLAs de **negócio** operacional |
| Diferente de Analytics & Reporting? | **Sim** — Operations Monitoring **define e monitora** SLAs; Analytics **agrega e exibe** indicadores |

**Pertence:** definição de SLAs operacionais, monitoramento de prazos (laudo, atendimento, espera), violação de meta, alertas operacionais institucionais.

**Não pertence:** protocolos clínicos de risco, pendências com significado clínico primário, dashboards (Analytics), filas e agendas (Care Coordination).

**Sub-capability:** Operational SLAs.

---

#### Parte 3 — Analytics & Reporting (revalidação)

| Pergunta | Resposta |
|---|---|
| Domínio de negócio? | **Não** nesta fase — read model permanente (D-004 reforçado) |
| Fontes | Care Monitoring, Operations Monitoring, Care Coordination, Care Delivery, outros domínios |
| Futuro domínio? | Só se surgir produto analítico autônomo com regras de negócio próprias — **Deferred** |

---

#### D-013 — Regras de fronteira Monitorar *(Ready for Confirmation)*

| # | Regra |
|---|---|
| 1 | **Care Monitoring** monitora continuidade e risco **clínico** do cuidado |
| 2 | **Operations Monitoring** monitora desempenho e SLAs **operacionais** institucionais |
| 3 | **Analytics & Reporting** agrega indicadores — não é domínio nesta fase |
| 4 | Regra de alerta clínico → Care Monitoring; entrega → Communication / Notification (PS) |
| 5 | Agenda, fila e encaminhamento → Care Coordination; medição de SLA da fila → Operations Monitoring |
| 6 | Observability Service (PS) monitora infraestrutura — não confundir com Operations Monitoring |
| 7 | Intelligence & Automation → **Deferred** — não absorver em Care Monitoring |

**Padrão evento dual (clínico + operacional):**

Um mesmo fato (ex.: laudo atrasado) pode gerar perspectivas em **dois domínios** sem duplicação de ownership:

| Perspectiva | Domínio |
|---|---|
| Pendência clínica — paciente aguarda resultado para decisão | Care Monitoring |
| Violação de SLA institucional — prazo de laudo excedido | Operations Monitoring |

---

#### Cenários de validação Monitorar

| # | Cenário | Domínio principal | Envolvidos | PS | Natureza | Risco |
|---|---|---|---|---|---|---|
| 1 | Exame pendente há 30 dias | Care Monitoring | Clinical Orders | Notification Service | Pendência clínica | Baixo |
| 2 | Resultado crítico de laboratório | Care Monitoring | Clinical Record | Notification Service | Alerta clínico | Baixo |
| 3 | Protocolo detecta hipertensão não controlada | Care Monitoring | Clinical Record | Notification Service | Protocolo clínico | Baixo |
| 4 | Fila de urgência excede tempo máximo | Operations Monitoring | Care Coordination | — | SLA operacional | Baixo |
| 5 | Prazo de laudo ultrapassado (SLA) | Operations Monitoring | Care Monitoring *(pendência)* | — | SLA + possível dual | Médio |
| 6 | Dashboard de indicadores assistenciais | Analytics & Reporting | múltiplos domínios | Search Service *(hip.)* | Read model | Baixo |
| 7 | Alerta in-app para enfermeiro | Care Monitoring | — | Notification Service | Regra + entrega | Baixo |
| 8 | Lembrete de retorno ao paciente | Care Monitoring | Communication | Communication Service | Alerta + canal | Baixo |
| 9 | Meta de atendimentos/hora da unidade | Operations Monitoring | Care Delivery | — | Métrica operacional | Baixo |
| 10 | Métricas de CPU/latência da plataforma | — | — | Observability Service | Infra técnica | Baixo |

**Risco residual:** cenário 5 — evento dual clínico+operacional (P-MON-02); padrão aceito, não invalida decomposição.

---

### NR-009 — Governance & Compliance Domain *(revisado — Ready for Confirmation ✅)*

**Status:** Ready for Confirmation ✅ *(promovido da revisão 2026-07-02)*

**Governance & Compliance** permanece como **Cross-cutting Business Domain** — responsabilidade de **negócio**, não implementação técnica.

#### Pertence ao Governance & Compliance Domain

| Responsabilidade | Exemplos |
|---|---|
| Políticas institucionais de conformidade | Regras da instituição, não do tenant técnico |
| Governança LGPD | Tratamento de dados, bases legais, direitos do titular |
| Políticas de retenção de dados | Quanto tempo manter documentos e registros |
| Políticas de anonimização | Quando e como anonimizar |
| Base legal | Fundamentação legal do tratamento |
| Evidências de conformidade | Pacote de evidência para auditoria institucional/regulatória |
| Regras institucionais de **Data Processing Consent** | Consentimento LGPD / privacidade — não canal |
| Revisão periódica de acesso *(processo de governança)* | Quem deve ser revisado, periodicidade, aprovação |
| Políticas de compartilhamento de dados | Com quem dados podem ser compartilhados e em quais condições |
| Requisitos regulatórios | LGPD, retenção, conformidade setorial |

#### Não pertence (Platform Services)

| Responsabilidade | PS |
|---|---|
| Autenticação | Identity Service |
| Autorização técnica | Authorization Service |
| Logs técnicos / audit trail | Audit Service |
| Observabilidade | Observability Service |
| Feature flags | Feature Flag Service |
| Configuração técnica | Configuration Service |
| Envio de mensagens | Communication Service, Notification Service |

**Hipótese PS complementar:** **Compliance Service** — enforcement técnico de políticas definidas pelo domínio (catálogo ADR-0005 a confirmar em sessão futura).

**Sub-capability fonte:** Compliance & Data Protection *(aspectos de política e governança — não logs técnicos)*.

---

### D-010 — Taxonomia de consentimentos *(nova — Ready for Confirmation)*

| Tipo | Descrição | Domínio responsável | Platform Services | Observações |
|---|---|---|---|---|
| **Communication Consent** | Autorização/preferência para receber comunicação por canal específico | **Communication** | Communication Service, Notification Service | Opt-in WhatsApp, revogação SMS. Não é LGPD de tratamento |
| **Data Processing Consent / Privacy Consent** | Consentimento ou base legal para tratamento de dados pessoais (LGPD) | **Governance & Compliance** | Compliance Service *(hipótese)*, Audit Service | Exclusão, anonimização, compartilhamento. Política no domínio; execução técnica via PS |
| **Clinical Consent** | Consentimento informado para procedimento, atendimento, exame ou intervenção | **Clinical Documents** *(artefato)* + **Care Delivery** *(contexto clínico)* | Document Engine (PS) | Termo assinado = Clinical Artifact. Gate clínico no atendimento/procedimento |
| **Research Consent** | Consentimento para pesquisa clínica | **Deferred** — domínio futuro | — | Fora do escopo desta fase; possível extensão de Clinical Documents |

**Regra:** tipos de consentimento **não são intercambiáveis**. Opt-in de WhatsApp ≠ consentimento LGPD ≠ termo de procedimento.

---

### D-011 — Regra de fronteira Governança *(Ready for Confirmation)*

```text
Governance & Compliance  →  define políticas, obrigações e evidências de conformidade
Platform Services        →  executam enforcement técnico, rastreabilidade, autorização,
                            auditoria e configuração
Domínios de negócio      →  consomem políticas e PS sem reimplementar transversais
```

| Camada | Pergunta que responde |
|---|---|
| Governance & Compliance | *O que a instituição deve cumprir e como demonstrar conformidade?* |
| Platform Services | *Como a plataforma executa tecnicamente?* |
| Domínios (ex.: Communication, Clinical Record) | *Qual regra de negócio do meu contexto?* |

---

### Cenários de validação de fronteira *(NR-006 + NR-009)*

| # | Cenário | Domínio | Platform Services | Natureza | Risco |
|---|---|---|---|---|---|
| 1 | Paciente aceita lembrete de consulta por WhatsApp | Communication | Communication Service | Regra de negócio + execução técnica | Baixo — Communication Consent claro |
| 2 | Paciente revoga preferência de SMS | Communication | Communication Service | Regra de negócio | Baixo |
| 3 | Paciente solicita exclusão/anonimização (LGPD) | Governance & Compliance | Compliance Service *(hip.)*, Audit Service; execução em Clinical Record/Documents | Política institucional + processo | Médio — orquestração cross-domain |
| 4 | Instituição define retenção de documentos clínicos | Governance & Compliance | Configuration Service *(param.)* | Política institucional | Baixo |
| 5 | Médico assina termo ou documento clínico | Clinical Documents | Document Engine | Regra de negócio clínica | Baixo — distinto de Communication |
| 6 | Paciente assina consentimento informado para procedimento | Care Delivery + Clinical Documents | Document Engine | Clinical Consent — gate + artefato | Médio — dois domínios coordenados |
| 7 | Responsável legal autorizado a receber resultado | Patient Relationship + Communication | Communication Service | Vínculo (Patient Relationship) + regra de destinatário (Communication) | Médio — coordenação entre domínios |
| 8 | Gestor gera evidência de conformidade para auditoria | Governance & Compliance | Audit Service | Evidência de negócio + logs técnicos | Baixo — política vs log separados |
| 9 | Revisão periódica de acesso do profissional | Governance & Compliance | Authorization Service, Identity Service | Processo de governança | Médio — processo no domínio, permissão no PS |
| 10 | Sistema registra log técnico de acesso a prontuário | — *(nenhum domínio de negócio)* | Audit Service | Execução técnica | Baixo — puramente PS |

---

## 5. Deferred Decisions

| ID | Item | Motivo |
|---|---|---|
| DEF-001 | **Hospital Operations** Domain | Hospital Care Delivery — escopo futuro |
| DEF-002 | **Billing / Financial Domain** | Q-005 Open — capabilities administrativas/financeiras não consolidadas |
| DEF-003 | **Financial & ERP Integration** → domínio | Aguarda Q-005; sub-capability permanece no mapa em Integrar |
| DEF-004 | **Intelligence & Automation** | Escopo futuro; capability Monitorar |
| DEF-005 | **Analytics** como Business Domain | Fase atual: read model; domínio só se surgir produto analítico autônomo |
| DEF-006 | **Q-018** — cardinalidade de jornadas | **Não é decisão da AS-004.** Múltiplas Institution Care Journeys Active permanece **Open** |
| DEF-007 | **Q-004** — agregados DDD | Após confirmação dos domínios |
| DEF-008 | Documentação oficial + ADR | Após confirmação do pacote |
| DEF-009 | ~~**Q-002 → Answered**~~ | ✅ Respondida — ADR-0008 (2026-07-02) |

### Nota sobre Q-018

A regra de cardinalidade da Institution Care Journey foi definida na **AS-003** (`care-journey-lifecycle.md`, ADR-0007 Accepted). A **AS-004 não deve reimportar nem estender** essa regra para fronteiras de domínio.

Cenários de programas paralelos (ocupacional + ambulatorial) permanecem em **Q-018 Open** — impacto em domínios centrais **não decidido** nesta sessão.

---

## 6. Rejected Alternatives

Consolidado em **D-009** (seção 3). Nenhuma alternativa rejeitada foi revertida nesta revisão.

---

## 7. Business Domain Candidates

> **Business Domain Candidates — validação holística NR-001 concluída (2026-07-02).**  
> 16 candidatos ativos prontos para confirmação explícita. Não são decisão final absoluta até confirmação do usuário e publicação oficial.

### Core Business Domains (5)

Âncora na Institution Care Journey e no ciclo clínico-operacional.

| Candidato | Core primária | Sub-capabilities | Confiança |
|---|---|---|---|
| **Care Delivery** | Executar | Clinical Attendance, Therapeutic Care Delivery | Alta |
| **Clinical Record** | Registrar | Clinical Documentation, Diagnostic Recording | Alta |
| **Clinical Orders** | Registrar | Prescription & Clinical Orders | Alta |
| **Clinical Documents** | Registrar | Clinical Artifacts | Alta |
| **Care Monitoring** | Monitorar | Care Pendencies & Follow-up, Clinical Alerts & Protocols | Alta |

### Supporting Business Domains (8)

Habilitam operação; não são o centro do cuidado.

| Candidato | Core primária | Sub-capabilities | Confiança |
|---|---|---|---|
| **Patient Relationship** | Relacionar | Patient Relationship | Alta |
| **Professional Management** | Relacionar | Professional & Provider Relationship | Alta |
| **Organization Management** | Relacionar | Organization & Network Relationship | Alta |
| **Payer & Insurance** | Relacionar | Insurance & Payer Relationship | Média-alta |
| **Care Coordination** | Organizar | Care Scheduling, Resource Planning, Queue & Flow, Referral Coordination, Operational Workflow | Alta |
| **Communication** | Comunicar | Patient/Professional Communication, Communication Preferences & Consent | Alta |
| **Integration** | Integrar | Clinical Interoperability, Diagnostic Systems, Webhooks & APIs, Medical Devices & IoT | Média-alta |
| **Operations Monitoring** | Monitorar | Operational SLAs | Média-alta |

### Extension Business Domains (2)

Modelos operacionais de execução com extensão específica.

| Candidato | Core primária | Sub-capabilities | Confiança |
|---|---|---|---|
| **Diagnostic Operations** | Executar | Diagnostic Service Delivery | Alta |
| **Home Care Operations** | Executar | Home Care Delivery | Alta |

### Cross-cutting Business Domains (1)

Responsabilidade de negócio transversal — distinta de Platform Services.

| Candidato | Core primária | Sub-capabilities | Confiança |
|---|---|---|---|
| **Governance & Compliance** | Governar | Compliance & Data Protection *(parcial)* | Alta |

### Deferred / Future Domains (2)

| Candidato | Origem | Status |
|---|---|---|
| **Hospital Operations** | Hospital Care Delivery | Deferred |
| **Billing & Financial** *(nome provisório)* | Financial & ERP Integration, Q-005 | Deferred |

**Total candidatos ativos:** 16 · **Deferred:** 2

---

## 8. Read Models / Views

| Nome | Sub-capability fonte | Status | Notas |
|---|---|---|---|
| **Clinical Timeline** | Clinical Timeline (Registrar) | Ready for Confirmation | Prontuário = visão; projeção cross-domain |
| **Analytics & Reporting** | Indicators & Dashboards (Monitorar) | Ready for Confirmation | Agregação de indicadores; não domínio nesta fase |

Implementação futura: módulo de leitura, projeção ou Search Service — fora do escopo desta sessão.

---

## 9. Platform Services Identified

Sub-capabilities ou funções que **não** devem virar Business Domain (ADR-0005).

| Origem | Sub-capability / função | Platform Service |
|---|---|---|
| Governar | Identity & Access | Identity Service |
| Governar | Authorization & Permissions | Authorization Service |
| Governar | Audit & Traceability | Audit Service |
| Governar | Configuration & Feature Management | Configuration Service, Feature Flag Service |
| Governar | Observability | Observability Service |
| Comunicar | Internal Notifications | Notification Service |
| Comunicar | Entrega por canais externos | Communication Service |
| Integrar | Conectores, adapters, protocolos | Integration Service |
| Integrar | Webhooks técnicos | Webhook Service |
| Registrar | Geração de documentos | Document Engine |
| Registrar | Templates técnicos | Template Service |
| Read models | Consulta / busca | Search Service *(candidato PS)* |

**Hipótese:** Compliance Service — enforcement técnico de políticas (PS), consumido por Governance & Compliance Domain.

---

## 10. Open Questions Impact

| ID | Impacto neste pacote | Pode fechar na AS-004? |
|---|---|---|
| **Q-002** | Objeto central — catálogo holístico validado | **Answered** — ADR-0008, 2026-07-02 |
| **Q-005** | Billing/Financial Deferred; Payer & Insurance hipótese de relacionamento | **Não** — permanece Open |
| **Q-007** | Regra Domain/PS/View em D-002 (parcial) | **Parcial** — critérios sim; Registrar reforça D-012 |
| **Q-018** | Removida decisão de cardinalidade desta sessão | **Não** — permanece Open; não importar para fronteiras de domínio |
| **Q-004** | Agregados dependem de domínios confirmados | **Não** — Deferred pós-confirmação |
| **Q-010** | NR-006 Ready for Confirmation; direção alinhada | **Open** — formalização Communication vs Notification |
| **Q-019** *(proposta)* | NR-009 Ready for Confirmation | **Open** — Compliance Service no catálogo ADR-0005 |
| **Q-020** *(proposta)* | Research Consent — domínio futuro | **Open** — Deferred |
| **P-REG-01** | Encaminhamento tri-mode (Orders + Coordination + Documents) | **Parcial** — padrão definido; configuração institucional Open |
| **P-REG-02** | Laudo formal vs conclusão diagnóstica | **Respondida** — Record = estruturado; Documents = laudo formal |
| **P-REG-03** *(nova)* | Vínculo Order ↔ Document quando ordem gera artefato | **Open** — referência vs cópia; não bloqueia NR-005 |
| **P-REG-04** *(nova)* | Resultado externo (LIS) — Integration ingere, Record registra | **Open** — contrato de handoff Integration → Record |
| **P-CLN-01** | Clinical Consent — gate Care Delivery vs artefato Documents | **Parcial** — padrão D-010; detalhe de implementação futuro |
| **P-MON-01** | Analytics & Dashboards — read model permanente? | **Parcial** — D-004 reforçado; produto analítico autônomo Deferred |
| **P-MON-02** *(nova)* | Evento dual clínico+operacional (ex.: laudo atrasado) | **Open** — padrão dual aceito; detalhe de correlação futuro |

---

## 11. Recommendation

### Pacote de confirmação final

O pacote consolidado para revisão e confirmação explícita está em **[`confirmation-package.md`](confirmation-package.md)**.

Contém **18 decisões** numeradas, catálogo dos 16 domínios, regras de fronteira, deferred, open questions residuais e checklist de confirmação.

### O pacote está pronto para confirmação?

**Sim.** Validação holística (NR-001) concluída. Nenhum NR pendente de validação estrutural. Aguarda apenas **confirmação explícita do usuário**.

### Confirmar agora (Ready for Confirmation)

- Modelo de decomposição C + E (D-001)
- Regras Domain / PS / Read Model (D-002)
- Clinical Timeline e Analytics como read models (D-003, D-004)
- Comunicar e Integrar não são apenas PS (D-005, D-006)
- Mapeamento Governar → PS (D-008)
- Alternativas rejeitadas (D-009)
- Internal Notifications → Notification Service (D-007)
- **NR-001 — Catálogo holístico dos 16 candidatos** ✅
- **D-014 — Critérios de validação holística**
- **NR-002 — Relacionar (4 domínios)** ✅
- **NR-003 — Care Coordination** ✅
- **NR-004 — Executar (Delivery + extensões)** ✅
- **NR-005 — Registrar (Record / Orders / Documents)** ✅
- **NR-006 — Communication Domain** ✅
- **NR-007 — Integration Domain** ✅
- **NR-008 — Care Monitoring + Operations Monitoring** ✅
- **NR-009 — Governance & Compliance Domain** ✅
- **D-010 a D-013** — taxonomia, fronteiras Governança, Registrar, Monitorar

### Revisar antes de confirmação final (Needs Review)

- *Nenhum NR pendente de validação estrutural*
- Open Questions de detalhe: Q-005, Q-010, Q-018, Q-019, P-ORG-01, P-REG-*, P-MON-02, P-COM-01

### Adiados (não decidir agora)

- Hospital Operations, Billing/Financial, Financial Integration, Intelligence & Automation
- Q-018, Q-004, Q-005
- Q-002 Answered, documentação oficial, ADR-0008

### Riscos removidos nesta revisão

| Risco removido | Como |
|---|---|
| 16 domínios tratados como decisão final absoluta | Rebaixado a candidatos para validação |
| AS-004 decidindo cardinalidade de jornada | Decisão 013 removida; Q-018 permanece Open |
| Q-002 marcada como respondida prematuramente | DEF-009 — aguarda validação final |
| ADR-0007 usado para impor regras de domínio | Apenas referência conceitual em NR-002; sem cardinalidade |

### Q-002 pode ser marcada como Answered?

**Não neste momento.**

Critério para Answered: documento oficial ou ADR Accepted respondendo claramente. Este pacote entrega **candidatos consolidados** e decisões parciais — suficiente para **avançar**, insuficiente para **fechar** Q-002.

**Condição para Answered:** confirmação **explícita do usuário** do catálogo NR-001 + publicação de `business-domains.md` e `domain-map.md` (e ADR recomendado). Validação holística do workspace **concluída** — insuficiente sozinha para fechar Q-002.

---

## Registro de confirmação

| Item | Status | Confirmado em |
|---|---|---|
| **Pacote AS-004 completo** | ✅ Confirmado | 2026-07-02 |
| ADR-0008 | Accepted | 2026-07-02 |
| Q-002 | Answered | 2026-07-02 |
| Sessão AS-004 | Completed | 2026-07-02 |

Open Questions residuais (Q-005, Q-010, P-REG-*, etc.) permanecem abertas — não invalidam o catálogo confirmado.

---

## Histórico de revisão

| Data | Alteração |
|---|---|
| 2026-07-02 | Pacote inicial (15 decisões) |
| 2026-07-02 | Revisão de segurança: classificação por maturidade; remoção Decisão 013; candidatos ≠ decisão final |
| 2026-07-02 | Refinamento NR-006, NR-009: taxonomia de consentimentos, regra de fronteira, 10 cenários |
| 2026-07-02 | NR-006, NR-009 promovidos a Ready for Confirmation; validação NR-005 Registrar + D-012 |
| 2026-07-02 | Validação NR-008 Monitorar + D-013; Operations Monitoring permanece domínio próprio |
| 2026-07-02 | Validação holística NR-001: 16 candidatos + D-014; NR-002/003/004/007 promovidos |
| 2026-07-02 | Pacote de confirmação final publicado (`confirmation-package.md`) |
| 2026-07-02 | **Pacote confirmado** — ADR-0008, docs oficiais, Q-002 Answered, AS-004 encerrada |

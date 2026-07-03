# Pacote de Confirmação Final — AS-004

> **Sessão:** AS-004 — Business Domain Map  
> **Data do pacote:** 2026-07-02  
> **Status:** **Confirmado** em 2026-07-02 pelo usuário.  
> **Formalização:** ADR-0008, `business-domains.md`, `domain-map.md`

---

## Confirmação registrada

**Confirmado em 2026-07-02** pelo usuário (*"Confirmo o pacote AS-004"*).

Formalização executada:

1. ✅ `decisions.md` — status Confirmado
2. ✅ `docs/04-domain/business-domains.md` e `domain-map.md`
3. ✅ **ADR-0008 — Business Domain Map** (Accepted)
4. ✅ **Q-002** → Answered em `open-questions.md`
5. ✅ Sessão AS-004 encerrada

**Fora do escopo desta confirmação:** código, módulos, APIs, agregados DDD (Q-004), cardinalidade de jornadas (Q-018).

---

## Resumo executivo

| Item | Decisão |
|---|---|
| Modelo de decomposição | Agrupamento de sub-capabilities (C) + PS transversais (E) |
| Business Domains ativos | **16** |
| Read models / views | **2** (Clinical Timeline, Analytics & Reporting) |
| Domínios deferred | **2** (Hospital Operations, Billing & Financial) |
| Sub-capabilities → PS | Identity, Authorization, Audit, Configuration, Observability, Notification, Communication Service, Integration Service, Webhook Service, Document Engine, Template Service |
| Alternativas rejeitadas | Mega domínios, 1 domínio por capability, domínios por tela/modelo operacional |

---

## Decisões estruturais

### Decisão 001 — Modelo de decomposição (D-001)

Business Domains emergem do **agrupamento de sub-capabilities relacionadas**, combinado com **responsabilidades transversais via Platform Services** (ADR-0005).

**Rejeitado:** 8 domínios = 8 capabilities · ~35 domínios · domínios por modelo operacional · domínios por tela/módulo.

**Status:** Confirmada  
**Formalização prevista:** ADR-0008, `business-domains.md`

---

### Decisão 002 — Domain vs Platform Service vs Read Model (D-002)

| Pergunta | Destino |
|---|---|
| Regras de negócio e vocabulário ubíquo próprios? | Business Domain |
| Responsabilidade transversal, reutilizável e técnica? | Platform Service |
| Agregação de leitura cross-domain? | Read Model / View |
| Domínio consome PS? | Sim — **não reimplementa** transversais |

**Status:** Confirmada · **Q-007 parcial**

---

### Decisão 003 — Clinical Timeline: Read Model (D-003)

**Clinical Timeline** não é Business Domain. É **visão/read model** — projeção cronológica cross-domain. O prontuário é visão da timeline institucional.

**Status:** Confirmada

---

### Decisão 004 — Analytics & Reporting: Read Model (D-004)

**Indicators & Dashboards** não é Business Domain nesta fase. Domínio Analytics futuro somente se surgir produto analítico autônomo.

**Status:** Confirmada · **P-MON-01 parcial**

---

### Decisão 005 — Comunicar não é apenas PS (D-005)

Capability **Comunicar** exige domínio de negócio (**Communication**) além de Communication Service e Notification Service.

**Status:** Confirmada

---

### Decisão 006 — Integrar não é apenas PS (D-006)

Capability **Integrar** exige domínio de negócio (**Integration**) além de Integration Service e Webhook Service.

**Status:** Confirmada

---

### Decisão 007 — Internal Notifications → Notification Service (D-007)

Sub-capability **Internal Notifications** → **Notification Service** (PS). Regras de disparo podem originar em domínios.

**Status:** Confirmada · **Q-010 Open**

---

### Decisão 008 — Governar técnico → Platform Services (D-008)

Identity, Authorization, Audit, Configuration, Observability → Platform Services. **Não** são Business Domains.

**Status:** Confirmada

---

## Decisão 009 — Catálogo dos 16 Business Domains (NR-001)

Validação holística concluída (D-014). **16/16 candidatos** aprovados.

### Core (5)

| Domínio | Capability | Sub-capabilities |
|---|---|---|
| **Care Delivery** | Executar | Clinical Attendance, Therapeutic Care Delivery |
| **Clinical Record** | Registrar | Clinical Documentation, Diagnostic Recording |
| **Clinical Orders** | Registrar | Prescription & Clinical Orders |
| **Clinical Documents** | Registrar | Clinical Artifacts |
| **Care Monitoring** | Monitorar | Care Pendencies & Follow-up, Clinical Alerts & Protocols |

### Supporting (8)

| Domínio | Capability | Sub-capabilities |
|---|---|---|
| **Patient Relationship** | Relacionar | Patient Relationship |
| **Professional Management** | Relacionar | Professional & Provider Relationship |
| **Organization Management** | Relacionar | Organization & Network Relationship |
| **Payer & Insurance** | Relacionar | Insurance & Payer Relationship |
| **Care Coordination** | Organizar | Care Scheduling, Resource Planning, Queue & Flow, Referral Coordination, Operational Workflow |
| **Communication** | Comunicar | Patient/Professional Communication, Communication Preferences & Consent |
| **Integration** | Integrar | Clinical Interoperability, Diagnostic Systems, Webhooks & APIs, Medical Devices & IoT |
| **Operations Monitoring** | Monitorar | Operational SLAs |

### Extension (2)

| Domínio | Capability | Sub-capabilities |
|---|---|---|
| **Diagnostic Operations** | Executar | Diagnostic Service Delivery |
| **Home Care Operations** | Executar | Home Care Delivery |

### Cross-cutting (1)

| Domínio | Capability | Sub-capabilities |
|---|---|---|
| **Governance & Compliance** | Governar | Compliance & Data Protection *(parcial — políticas)* |

**Status:** Confirmada

---

## Decisões por capability

### Decisão 010 — Relacionar: quatro domínios (NR-002)

1:1 entre sub-capabilities e domínios de vínculo. Payer & Insurance permanece relacionamento; faturamento **não** pertence aqui (deferred).

**Status:** Confirmada · **Q-005 Open**

---

### Decisão 011 — Organizar: Care Coordination (NR-003)

Todas as sub-capabilities de Organizar em **Care Coordination**, incluindo Operational Workflow (parametrização via Configuration Service).

**Status:** Confirmada · **P-ORG-01 Open**

---

### Decisão 012 — Executar: Delivery + extensões (NR-004)

| Sub-capability | Domínio |
|---|---|
| Clinical Attendance, Therapeutic Care Delivery | **Care Delivery** |
| Diagnostic Service Delivery | **Diagnostic Operations** |
| Home Care Delivery | **Home Care Operations** |
| Hospital Care Delivery | **Deferred** |

Telemedicina = tipo de Attendance em Care Delivery. Home Care = Extension (ADR-0003), não absorvido em Care Delivery.

**Status:** Confirmada

---

### Decisão 013 — Registrar: três domínios clínicos (NR-005)

| Domínio | Responsabilidade |
|---|---|
| **Clinical Record** | Conhecimento clínico — evoluções, diagnósticos, resultados estruturados |
| **Clinical Orders** | Intenções e ordens — prescrições, solicitações, ciclo de vida |
| **Clinical Documents** | Artefatos formais — atestados, laudos, termos, consentimentos formalizados |

Clinical Timeline = read model (Decisão 003). Document Engine = PS.

**Regras de fronteira (D-012):**

1. Clinical Record registra conhecimento clínico  
2. Clinical Orders representa intenções e ordens  
3. Clinical Documents representa artefatos formais assináveis  
4. Document Engine = PS  
5. Clinical Timeline = read model  
6. Care Delivery executa — não é dono primário dos registros  
7. Communication entrega — não é dono do documento  
8. Integration transporta — não é dona do significado clínico  

**Padrão encaminhamento:** Orders (estruturado) + Care Coordination (fluxo) + Documents (formal).

**Status:** Confirmada · **P-REG-01 parcial, P-REG-03, P-REG-04 Open**

---

### Decisão 014 — Comunicar: Communication Domain (NR-006)

Domínio de negócio para políticas de comunicação, preferências de canal, **Communication Consent**, destinatários autorizados, templates de negócio e histórico institucional.

**Não pertence:** LGPD/dados, auditoria técnica, autorização, envio técnico, consentimento clínico/pesquisa.

**Status:** Confirmada · **Q-010 Open**

---

### Decisão 015 — Integrar: Integration Domain (NR-007)

Políticas e contratos de interoperabilidade. Financial & ERP Integration → **Deferred** (Q-005).

**Status:** Confirmada

---

### Decisão 016 — Monitorar: dois domínios (NR-008)

| Domínio | Responsabilidade |
|---|---|
| **Care Monitoring** | Pendências clínicas, protocolos, alertas de risco |
| **Operations Monitoring** | SLAs operacionais — domínio próprio, não absorvido |

Analytics & Reporting = read model (Decisão 004).

**Regras de fronteira (D-013):**

1. Care Monitoring = risco e continuidade clínica  
2. Operations Monitoring = SLAs operacionais  
3. Analytics = read model  
4. Alerta clínico → Monitoring; entrega → PS  
5. Fila/agenda → Care Coordination; SLA da fila → Operations Monitoring  
6. Observability Service ≠ Operations Monitoring  
7. Intelligence & Automation → Deferred  

**Padrão evento dual:** laudo atrasado → Care Monitoring (pendência) + Operations Monitoring (SLA).

**Status:** Confirmada · **P-MON-02 Open**

---

### Decisão 017 — Governar: Governance & Compliance (NR-009)

**Cross-cutting Business Domain** para políticas institucionais, LGPD, retenção, **Data Processing Consent**, evidências de conformidade e revisão periódica de acesso.

**Não implementa:** auth, autorização técnica, logs, observabilidade, feature flags, envio de mensagens.

**Regra de fronteira (D-011):** Governance define políticas; PS executam enforcement; domínios consomem.

**Status:** Confirmada · **Q-019 Open** (Compliance Service)

---

### Decisão 018 — Taxonomia de consentimentos (D-010)

| Tipo | Domínio |
|---|---|
| Communication Consent | Communication |
| Data Processing / Privacy Consent | Governance & Compliance |
| Clinical Consent | Care Delivery + Clinical Documents |
| Research Consent | **Deferred** |

Tipos **não são intercambiáveis**.

**Status:** Confirmada · **Q-020 Deferred**

---

## Itens fora do catálogo

### Read models (confirmados, não domínios)

| Nome | Origem |
|---|---|
| Clinical Timeline | Registrar |
| Analytics & Reporting | Monitorar |

### Deferred (não decidir nesta fase)

| Item | Motivo |
|---|---|
| Hospital Operations | Escopo futuro |
| Billing & Financial | Q-005 Open |
| Financial & ERP Integration | Aguarda Q-005 |
| Intelligence & Automation | Escopo futuro |
| Research Consent domain | Q-020 |

### Alternativas rejeitadas (D-009)

Registro permanente em `decisions.md` seção 3 — não revertidas.

---

## Platform Services identificados

| Origem | Platform Service |
|---|---|
| Governar | Identity, Authorization, Audit, Configuration, Feature Flag, Observability |
| Comunicar | Notification Service, Communication Service |
| Integrar | Integration Service, Webhook Service |
| Registrar | Document Engine, Template Service |
| Read models | Search Service *(hipótese)* |
| Governança | Compliance Service *(hipótese — Q-019)* |

---

## Open Questions — permanecem após confirmação

| ID | Assunto | Bloqueia confirmação? |
|---|---|---|
| Q-005 | Billing / Financial futuro | Não |
| Q-010 | Communication vs Notification Service | Não |
| Q-018 | Múltiplas jornadas ativas | Não |
| Q-019 | Compliance Service no ADR-0005 | Não |
| Q-020 | Research Consent | Não (deferred) |
| Q-004 | Agregados DDD | Não (pós-confirmação) |
| P-ORG-01 | Operational Workflow vs Configuration | Não |
| P-REG-01/03/04 | Fronteiras Registrar cross-domain | Não |
| P-MON-02 | Evento dual clínico+operacional | Não |
| P-COM-01 | Responsável legal + comunicação | Não |
| P-CLN-01 | Clinical Consent — detalhe gate/artefato | Não |

**Q-002:** será candidata a **Answered** somente **após** confirmação explícita + publicação oficial.

---

## Checklist de confirmação

Marque mentalmente ou responda na conversa:

- [x] **Decisões 001–008** — modelo estrutural e PS
- [x] **Decisão 009** — catálogo dos 16 Business Domains
- [x] **Decisões 010–017** — fronteiras por capability
- [x] **Decisão 018** — taxonomia de consentimentos
- [x] **Read models** — Clinical Timeline e Analytics permanecem visões
- [x] **Deferred** — Hospital, Billing, Intelligence, Research aceitos como adiados
- [x] **Open Questions residuais** — aceitas como não bloqueantes

**Confirmação única:** *"Confirmo o pacote AS-004"* confirma o conjunto acima.

---

## Artefatos de referência no workspace

| Arquivo | Conteúdo |
|---|---|
| `decisions.md` | Pacote completo com detalhamento e cenários |
| `domain-candidates.md` | Candidatos por capability |
| `capability-to-domain-mapping.md` | Matriz 39 sub-capabilities |
| `hypotheses.md` | Hipóteses originais |
| `questions.md` | Perguntas investigadas |
| `risks.md` | Riscos |

---

## Pós-confirmação — entregáveis planejados

| Ordem | Artefato | Ação |
|---|---|---|
| 1 | `workspace/AS-004/decisions.md` | Status → Confirmada |
| 2 | `docs/04-domain/business-domains.md` | Criar v0.1.0 |
| 3 | `docs/04-domain/domain-map.md` | Criar v0.1.0 |
| 4 | `docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md` | Criar Accepted |
| 5 | `architecture-sessions/AS-004-business-domain-map.md` | Encerrar sessão |
| 6 | `ai-context/open-questions.md` | Q-002 → Answered |
| 7 | `ARCHITECTURE_INDEX.md` | Atualizar índice |

---

## Histórico

| Data | Evento |
|---|---|
| 2026-07-02 | Investigação e refinamento por capability (NR-005 a NR-009) |
| 2026-07-02 | Validação holística NR-001 |
| 2026-07-02 | Pacote de confirmação final publicado |
| 2026-07-02 | **Pacote confirmado** — formalização executada (ADR-0008, docs oficiais, Q-002 Answered) |

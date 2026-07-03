# Module Strategy Draft

> **Promovido para documentação oficial:** `docs/05-architecture/module-strategy.md` (ADR-0009).  
> Este arquivo permanece como registro histórico da investigação NR-004.

**Investigação:** NR-004 — estratégia de módulos e relação domínio → módulo  
**Pré-requisitos:** `core-boundary.md` v0.2.1 · `platform-services-boundary.md` v0.3.0  
**Status:** Validado pelo usuário em 2026-07-03  
**Última atualização:** 2026-07-03

---

## 1. Purpose

Derivar um **catálogo preliminar de módulos candidatos** a partir dos **16 Business Domains** confirmados (ADR-0008), definindo:

- o que é um **Module** na Health Platform;
- como módulos se relacionam com domínios, Platform Services e Core Platform;
- cardinalidade **domínio → módulo**;
- **Platform Services consumidos** por cada módulo candidato;
- regras de acoplamento e habilitação;
- posicionamento de **Clinical Workspace** (T-003).

Este documento **não** define MVP (Q-006), APIs, telas, bounded contexts ou implementação.

---

## 2. Definition of Module

Na Health Platform, um **Module** é uma **unidade funcional implementável** que:

- deriva de um ou mais **Business Domains** e de uma **Core Business Capability** primária;
- entrega **funcionalidade ao usuário** ou **orquestra** outras unidades funcionais (shell);
- **consome** Platform Services por contrato — **não** reimplementa transversais;
- pode ser **habilitado ou desabilitado** por tenant/instituição (Configuration + Feature Flag + Module Registry);
- **não** possui vocabulário de negócio autônomo equivalente a um domínio — implementa regras do(s) domínio(s) fonte.

**Definição AS-004 (reafirmada):** Module = solução implementável futura, derivada de domínio + capability.

```text
Business Domain  →  regras e vocabulário (O QUÊ)
Module           →  implementação funcional (COMO na UI/fluxo)
Platform Service →  mecanismo transversal (Identity, Audit, Communication…)
Core Platform    →  registry, contratos, composição
```

---

## 3. What Module Is Not

Module **não é**:

| Conceito | Diferença |
|---|---|
| **Business Domain** | Domínio define regras; módulo implementa |
| **Platform Service** | PS é transversal sem UI; módulo tem experiência de usuário |
| **Core Platform** | Core é fundação; módulo é extensão de produto |
| **Extension Mechanism** | Mecanismo do Core; módulo é unidade registrada |
| **Extension Business Domain** | Domínio Extension (Diagnostic, Home Care) — módulo *implementa* o domínio |
| **Read Model / View** | Timeline e Analytics são projeções — módulos *consomem*, não são a view |
| **Feature** | Feature é parte de um módulo |
| **Microsserviço** | Módulo é unidade lógica de produto — não unidade de deploy |
| **Tela isolada** | Módulo agrupa fluxo coeso derivado de domínio |

---

## 4. Relationship with Core Platform

Módulos interagem com o Core Platform via:

| Mecanismo Core | Papel para módulos |
|---|---|
| **Module Registry** | Registro, descoberta, metadados, dependências entre módulos |
| **Extension Mechanism** | Hooks de composição; Extension Modules opcionais |
| **Runtime Context** | Tenant, organization, user propagados a cada módulo |
| **Hierarchical Care Model** | Módulos clínicos operam nos níveis Attendance, Event, Artifact |
| **Care Journey types** | Módulos respeitam ciclo de vida da Institution Care Journey |
| **Event Foundation** | Módulos publicam/consomem eventos sem acoplamento direto |

**Módulos não alteram o Core.** Habilitação via Configuration Service + Feature Flag Service (PS), registro via Module Registry (Core).

---

## 5. Relationship with Business Domains

| Padrão | Descrição | Exemplo |
|---|---|---|
| **1:1** | Um domínio, um módulo principal | Clinical Orders → Orders Module *(não obrigatório)* |
| **1:N** | Um domínio, vários módulos | Care Coordination → Scheduling (+ fila futura) |
| **N:1** | Vários domínios, um módulo | Patient Portal → Patient Relationship + Communication |
| **1:0** | Domínio sem módulo dedicado | Communication — orquestrado por outros módulos + PS |
| **Extension** | Domínio Extension → módulo opcional | Diagnostic Operations → Diagnostic Module |

**Regra:** todo módulo declara **domínio primário** + capability primária. Domínios secundários são permitidos (N:1).

**Domínios sem módulo exclusivo neste catálogo:**

| Domínio | Estratégia |
|---|---|
| **Communication** | Regras no domínio; execução via PS; orquestração em Patient Portal, Attendance, Monitoring — **sem Communication Module dedicado** |
| **Integration** | Integration Admin Module (config/conectores); execução via Integration + Webhook PS |
| **Governance & Compliance** | Governance Admin Module (políticas, relatórios); enforcement via Audit/Authorization PS |

---

## 6. Relationship with Platform Services

Módulos **consomem** PS — nunca duplicam Identity, Audit, Communication, etc.

**PS universais** (praticamente todos os módulos autenticados):

- Identity Service, Authorization Service, Audit Service, Configuration Service

**Padrão de consumo:**

```text
Módulo → Authorization (pode?) → Domínio (regra) → PS (mecanismo) → resposta
```

Exemplo: Attendance Module decide enviar lembrete (Communication Domain) → Communication Service envia SMS.

---

## 7. Module Inclusion Criteria

| # | Critério |
|---|---|
| M-INC-01 | Deriva de pelo menos um **Business Domain** confirmado |
| M-INC-02 | Possui **capability primária** rastreável ao Business Capability Map |
| M-INC-03 | Entrega **funcionalidade de usuário** ou **shell de composição** justificável |
| M-INC-04 | **Não** é implementação de PS ou Read Model puro |
| M-INC-05 | Pode ser **habilitado/desabilitado** sem quebrar o Core Platform |
| M-INC-06 | Consumo de transversais apenas via **PS** |
| M-INC-07 | Rastreio claro em `domain-map.md` / `business-domains.md` |

---

## 8. Module Exclusion Criteria

| # | Critério de exclusão |
|---|---|
| M-EXC-01 | É Platform Service (Identity, Audit, Communication…) |
| M-EXC-02 | É Read Model (Clinical Timeline, Analytics) — módulo *consome* a projeção |
| M-EXC-03 | É apenas contrato do Core (Registry, Runtime Context) |
| M-EXC-04 | É regra de domínio sem superfície funcional própria — permanece no domínio |
| M-EXC-05 | É 1:1 forçado com domínio **sem** justificativa de UX ou fluxo |
| M-EXC-06 | Duplica outro módulo candidato sem fronteira clara |
| M-EXC-07 | É customização por cliente — viola ADR-0006 |

---

## 9. Domain → Module Map (16 domínios)

| # | Business Domain | Módulo(s) candidato(s) | Padrão |
|---|---|---|---|
| 1 | Care Delivery | M-02 (shell), M-03 (Attendance), M-13 *(capability)* | 1:N + shell |
| 2 | Clinical Record | M-04 (Clinical Documentation) | 1:1 |
| 3 | Clinical Orders | M-05 (Orders) | 1:1 |
| 4 | Clinical Documents | M-06 (Documents) | 1:1 |
| 5 | Care Monitoring | M-07 (Care Monitoring) | 1:1 |
| 6 | Patient Relationship | M-08 (Patient Portal) | N:1 |
| 7 | Professional Management | M-09 (Professional Admin) | N:1 |
| 8 | Organization Management | M-09 (Professional Admin) | N:1 |
| 9 | Payer & Insurance | M-10 (Payer) | 1:1 |
| 10 | Care Coordination | M-01 (Scheduling) | 1:1 *(1:N futuro)* |
| 11 | Communication | — *(sem módulo dedicado)* | orquestrado |
| 12 | Integration | M-11 (Integration Admin) | 1:1 |
| 13 | Operations Monitoring | M-12 (Operations Dashboard) | 1:1 |
| 14 | Diagnostic Operations | M-14 (Diagnostic) | Extension |
| 15 | Home Care Operations | M-15 (Home Care) | Extension |
| 16 | Governance & Compliance | M-16 (Governance Admin) | 1:1 |

**Read models consumidos (não módulos):** Clinical Timeline (M-02, M-08); Analytics (M-12).

---

## 10. Candidate Modules — Catálogo detalhado

### M-01 — Scheduling Module

**Status:** Strong Candidate

**Classification:** Module — Core product

| Campo | Valor |
|---|---|
| **Domínio primário** | Care Coordination |
| **Capability** | Organizar |
| **Sub-capabilities** | Care Scheduling, Resource Planning, Referral Coordination *(parcial)* |
| **Tipo** | Core product |

**Description:** Agendamento, alocação de recursos, encaminhamentos — fluxos de Organizar.

**Platform Services consumed:** Identity, Authorization, Audit, Configuration, Notification, Communication *(lembretes)*, Search *(opcional)*

**Does not implement:** fila em tempo real avançada *(pode evoluir sub-feature)*; regras de atendimento (Care Delivery).

**Risks:** Absorver Care Coordination inteiro em um monólito de "agenda".

**Recommendation:** Manter módulo focado em scheduling; queue/flow pode ser subdomínio futuro ou feature dentro do módulo.

---

### M-02 — Clinical Workspace Module

**Status:** Strong Candidate — **Needs Review** (composição vs absorção de M-03)

**Classification:** Module — Transversal shell

| Campo | Valor |
|---|---|
| **Domínios** | Care Delivery, Clinical Record, Clinical Orders, Clinical Documents, Care Monitoring *(orquestração)* |
| **Capabilities** | Executar, Registrar, Monitorar |
| **Tipo** | Transversal shell |

**Description:** Ambiente de trabalho do profissional de saúde — **shell** que compõe sub-módulos (Attendance, Documentation, Orders, Documents, Monitoring) e expõe **Clinical Timeline** (read model).

**Platform Services consumed:** Identity, Authorization, Audit, Configuration, Search, Notification

**What belongs here:** Composição de UI, contexto do atendimento ativo, navegação entre capacidades clínicas, slot para módulos registrados.

**What does NOT belong here:** Regras clínicas de atendimento (Care Delivery Domain → M-03); registro primário (domínios Registrar); Core UI framework.

**Clinical Workspace resolution (T-003 / OQ-C03):**

| Alternativa | Resultado |
|---|---|
| A — Módulo shell transversal | **Adotada provisoriamente** (H-WS-001) |
| B — Core Platform UI | **Rejeitada** — risco Core grande |
| C — Sem shell | **Rejeitada** — experiência fragmentada |

**Risks:** Super-módulo que engole M-03 a M-07; virar domínio disfarçado.

**Recommendation:** M-02 é **shell de composição** sem regras de negócio próprias. M-03–M-07 permanecem módulos registrados carregados pelo shell. **UI composition contract** mínimo pode existir no Core (slot/hook) — OQ-C03 parcial.

---

### M-03 — Attendance Module

**Status:** Strong Candidate

**Classification:** Module — Core product

| Campo | Valor |
|---|---|
| **Domínio primário** | Care Delivery |
| **Capability** | Executar |
| **Sub-capabilities** | Clinical Attendance, Therapeutic Care Delivery |
| **Tipo** | Core product |

**Description:** Realização de atendimentos — consultas, sessões terapêuticas, interações no nível Attendance (ADR-0001).

**Platform Services consumed:** Identity, Authorization, Audit, Medical Form Engine, Communication, Notification, File, Event Bus *(fut.)*

**Loaded by:** Clinical Workspace (M-02)

**Risks:** Duplicar Clinical Workspace; embutir registro clínico (→ M-04).

**Recommendation:** Separado de M-02. Captura via Medical Form Engine; conteúdo clínico → Clinical Record Domain.

---

### M-04 — Clinical Documentation Module

**Status:** Strong Candidate

**Classification:** Module — Core product

| Campo | Valor |
|---|---|
| **Domínio primário** | Clinical Record |
| **Capability** | Registrar |
| **Sub-capabilities** | Clinical Documentation, Diagnostic Recording |
| **Tipo** | Core product |

**Description:** Evoluções, anamneses, diagnósticos, registros estruturados de conhecimento clínico.

**Platform Services consumed:** Identity, Authorization, Audit, Medical Form Engine, File, Search, Template

**Loaded by:** Clinical Workspace (M-02)

**Recommendation:** Confirmado como módulo separado de Attendance (Executar vs Registrar).

---

### M-05 — Orders Module

**Status:** Strong Candidate

**Classification:** Module — Core product

| Campo | Valor |
|---|---|
| **Domínio primário** | Clinical Orders |
| **Capability** | Registrar |
| **Sub-capabilities** | Prescription & Clinical Orders |
| **Tipo** | Core product |

**Description:** Prescrições, solicitações de exames, ciclo de vida de ordens clínicas.

**Platform Services consumed:** Identity, Authorization, Audit, Template, Document Engine, Communication *(entrega ao paciente)*, Event Bus *(fut.)*

**Loaded by:** Clinical Workspace (M-02)

---

### M-06 — Documents Module

**Status:** Strong Candidate

**Classification:** Module — Core product

| Campo | Valor |
|---|---|
| **Domínio primário** | Clinical Documents |
| **Capability** | Registrar |
| **Sub-capabilities** | Clinical Artifacts |
| **Tipo** | Core product |

**Description:** Artefatos formais — laudos, atestados, termos, consentimentos formalizados.

**Platform Services consumed:** Identity, Authorization, Audit, Document Engine, Template, File, Storage

**Loaded by:** Clinical Workspace (M-02)

---

### M-07 — Care Monitoring Module

**Status:** Strong Candidate

**Classification:** Module — Core product

| Campo | Valor |
|---|---|
| **Domínio primário** | Care Monitoring |
| **Capability** | Monitorar |
| **Sub-capabilities** | Care Pendencies & Follow-up, Clinical Alerts & Protocols |
| **Tipo** | Core product |

**Description:** Pendências clínicas, protocolos, alertas de risco — continuidade do cuidado.

**Platform Services consumed:** Identity, Authorization, Audit, Notification, Communication, Configuration

**Loaded by:** Clinical Workspace (M-02); pode ter visão resumida em Operations Dashboard

**Note:** Entrega de alerta → Notification PS; regra de alerta → domínio.

---

### M-08 — Patient Portal Module

**Status:** Strong Candidate

**Classification:** Module — Supporting

| Campo | Valor |
|---|---|
| **Domínios primários** | Patient Relationship, Communication |
| **Capabilities** | Relacionar, Comunicar |
| **Tipo** | Supporting / Core product *(portal como canal)* |

**Description:** Portal do paciente — cadastro limitado, resultados autorizados, mensagens, agendamento self-service *(se habilitado)*.

**Platform Services consumed:** Identity, Authorization, Audit, Communication, Notification, Configuration

**Read models consumed:** Clinical Timeline *(visão parcial autorizada)*

**Risks:** Virar cópia do Clinical Workspace para paciente.

**Recommendation:** N:1 justificado — superfície distinta do profissional.

---

### M-09 — Professional & Organization Admin Module

**Status:** Strong Candidate

**Classification:** Module — Supporting

| Campo | Valor |
|---|---|
| **Domínios primários** | Professional Management, Organization Management |
| **Capability** | Relacionar |
| **Tipo** | Supporting |

**Description:** Administração de profissionais, especialidades, credenciamento, estrutura organizacional, unidades, redes.

**Platform Services consumed:** Identity, Authorization, Audit, Configuration, Search

**Note:** Cadastro de profissional é domínio; autenticação é Identity PS.

---

### M-10 — Payer & Insurance Module

**Status:** Weak Candidate

**Classification:** Module — Supporting

| Campo | Valor |
|---|---|
| **Domínio primário** | Payer & Insurance |
| **Capability** | Relacionar |
| **Tipo** | Supporting |

**Description:** Convênios, planos, cobertura, elegibilidade — **sem** faturamento (Q-005 deferred).

**Platform Services consumed:** Identity, Authorization, Audit, Configuration, Integration *(operadoras futuro)*

**Recommendation:** Weak Candidate — necessário para clínicas com convênio; pode ser fase 2.

---

### M-11 — Integration Admin Module

**Status:** Strong Candidate

**Classification:** Module — Supporting

| Campo | Valor |
|---|---|
| **Domínio primário** | Integration |
| **Capability** | Integrar |
| **Tipo** | Supporting |

**Description:** Configuração de conectores, endpoints, mapeamentos — administração de integrações.

**Platform Services consumed:** Identity, Authorization, Audit, Integration, Webhook, Configuration

**Note:** Execução de protocolo → PS; semântica clínica → Integration Domain.

---

### M-12 — Operations Dashboard Module

**Status:** Strong Candidate

**Classification:** Module — Supporting

| Campo | Valor |
|---|---|
| **Domínio primário** | Operations Monitoring |
| **Capability** | Monitorar |
| **Sub-capabilities** | Operational SLAs |
| **Tipo** | Supporting |

**Description:** Dashboards operacionais, SLAs, indicadores operacionais.

**Platform Services consumed:** Identity, Authorization, Audit, Search, Configuration

**Read models consumed:** Analytics & Reporting *(visão)*

**Note:** Care Monitoring (M-07) é clínico; M-12 é operacional.

---

### M-13 — Telemedicine Capability *(não módulo independente provisório)*

**Status:** Needs Review

**Classification:** Extension capability within M-03 (Attendance) — **não** módulo separado nesta fase

| Campo | Valor |
|---|---|
| **Domínio** | Care Delivery |
| **Capability** | Executar |
| **Modelo operacional** | Telemedicine |

**Description:** Atendimento à distância como **modo** do Attendance Module — habilitado por Configuration + Feature Flag quando instituição opera Telemedicine.

**Alternativa rejeitada provisoriamente:** M-13 como módulo independente — duplicaria Care Delivery.

**Open:** instituição 100% telemedicina pode exigir módulo Extension futuro — registrar em extension-model-draft.

---

### M-14 — Diagnostic Module

**Status:** Strong Candidate

**Classification:** Module — Extension

| Campo | Valor |
|---|---|
| **Domínio primário** | Diagnostic Operations |
| **Capability** | Executar |
| **Sub-capabilities** | Diagnostic Service Delivery |
| **Tipo** | Extension |
| **Modelo operacional** | Diagnostic Care |

**Description:** Operação diagnóstica — laboratório, imagem, produção de resultados.

**Platform Services consumed:** Identity, Authorization, Audit, Integration, File, Communication, Document Engine

**Habilitação:** Configuration + Feature Flag + Module Registry (Extension Module)

---

### M-15 — Home Care Module

**Status:** Strong Candidate

**Classification:** Module — Extension

| Campo | Valor |
|---|---|
| **Domínio primário** | Home Care Operations |
| **Capability** | Executar |
| **Sub-capabilities** | Home Care Delivery |
| **Tipo** | Extension |
| **Modelo operacional** | Home Care |

**Description:** Visitas domiciliares, planos de cuidado domiciliar, execução em Home Care.

**Platform Services consumed:** Identity, Authorization, Audit, Communication, Notification, File, Medical Form Engine

---

### M-16 — Governance Admin Module

**Status:** Strong Candidate

**Classification:** Module — Cross-cutting admin

| Campo | Valor |
|---|---|
| **Domínio primário** | Governance & Compliance |
| **Capability** | Governar |
| **Tipo** | Cross-cutting |

**Description:** Administração de políticas, consentimentos LGPD, relatórios de conformidade, revisão de acessos — UI de governança.

**Platform Services consumed:** Identity, Authorization, Audit, Configuration, Compliance Service *(hip. Q-019)*

**Note:** Política no domínio; enforcement nos PS.

---

## 11. Module Composition Model (Clinical Workspace)

```text
Clinical Workspace Module (M-02) — shell
├── Attendance Module (M-03)
├── Clinical Documentation Module (M-04)
├── Orders Module (M-05)
├── Documents Module (M-06)
├── Care Monitoring Module (M-07)
└── Clinical Timeline (read model — projeção, não módulo)

Extension modules (opcionais, carregados se habilitados):
├── Diagnostic Module (M-14)
└── Home Care Module (M-15)
```

**Regras de composição:**

1. Shell **não** contém regras clínicas — apenas orquestra.
2. Sub-módulos **registrados** no Module Registry (Core).
3. Habilitação por tenant via Configuration / Feature Flag.
4. Sub-módulos comunicam via **PS e Event Foundation** — não chamadas diretas internas.

---

## 12. Platform Services Consumption Matrix

| PS | M-01 | M-02 | M-03 | M-04 | M-05 | M-06 | M-07 | M-08 | M-09 | M-10 | M-11 | M-12 | M-14 | M-15 | M-16 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Identity | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Authorization | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Audit | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Configuration | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Feature Flag | ○ | ✓ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ✓ | ✓ | ○ |
| Communication | ✓ | ○ | ✓ | ○ | ✓ | ○ | ✓ | ✓ | ○ | ○ | ○ | ○ | ✓ | ✓ | ○ |
| Notification | ✓ | ✓ | ✓ | ○ | ○ | ○ | ✓ | ✓ | ○ | ○ | ○ | ○ | ○ | ✓ | ○ |
| Medical Form Engine | ○ | ○ | ✓ | ✓ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ✓ | ○ |
| Document Engine | ○ | ○ | ○ | ○ | ✓ | ✓ | ○ | ○ | ○ | ○ | ○ | ○ | ✓ | ○ | ○ |
| Template | ○ | ○ | ○ | ✓ | ✓ | ✓ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ |
| File / Storage | ○ | ○ | ○ | ✓ | ○ | ✓ | ○ | ○ | ○ | ○ | ○ | ○ | ✓ | ✓ | ○ |
| Search | ○ | ✓ | ○ | ✓ | ○ | ○ | ○ | ○ | ✓ | ○ | ○ | ✓ | ○ | ○ | ○ |
| Integration | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ✓ | ✓ | ○ | ✓ | ○ | ○ |
| Webhook | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ✓ | ○ | ○ | ○ | ○ |
| Event Bus | ○ | ○ | ✓ | ○ | ✓ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ |
| Compliance *(hip.)* | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ○ | ✓ |

✓ = consumo esperado · ○ = opcional ou indireto

---

## 13. Acoplamento entre módulos

| # | Regra |
|---|---|
| A-01 | Módulos **não** acessam estado interno de outro módulo |
| A-02 | Orquestração cross-module via **Event Foundation** (contrato Core) |
| A-03 | Dados compartilhados via **contratos de domínio** ou read models — não import direto |
| A-04 | Shell (M-02) compõe por **Module Registry** — não hardcode de dependências |
| A-05 | Extension modules **não** alteram Core modules — apenas adicionam capacidade |
| A-06 | Communication Domain orquestra envios — módulos solicitam via contrato, PS entrega |

---

## 14. Module Tiers Summary

| Tier | Módulos | Contagem |
|---|---|---|
| **Core product** | M-01, M-02, M-03, M-04, M-05, M-06, M-07, M-08 | 8 |
| **Supporting** | M-09, M-10, M-11, M-12, M-16 | 5 |
| **Extension** | M-14, M-15 | 2 |
| **Capability (não módulo)** | M-13 Telemedicine → dentro de M-03 | — |

**Total módulos candidatos:** **15** (não 16 domínios — Communication sem módulo dedicado; Telemedicine absorvido em Attendance).

---

## 15. What NOT to Create as Module

| Conceito | Classificação correta |
|---|---|
| Identity, Authorization, Audit | Platform Service |
| Clinical Timeline | Read Model — consumido por M-02, M-08 |
| Analytics & Reporting | Read Model — consumido por M-12 |
| Communication Domain rules | Business Domain — sem módulo dedicado |
| Event Bus | Platform Service |
| Module Registry | Core Platform |
| Billing & Financial | Deferred domain (Q-005) |

---

## 16. Risks

| # | Risco | Mitigação |
|---|---|---|
| R-MOD01 | 16 módulos = 16 domínios | Catálogo 15 com N:1 e 1:N |
| R-MOD02 | Clinical Workspace monólito | Shell sem regras; M-03–M-07 separados |
| R-MOD03 | Módulo duplica PS | Matriz de consumo; code review |
| R-MOD04 | Communication Module redundante | Orquestração nos módulos consumidores |
| R-MOD05 | Telemedicine módulo órfão | Capability dentro de Attendance |
| R-MOD06 | Acoplamento M-03 ↔ M-04 | Eventos + domínios distintos |
| R-MOD07 | Extension no core path | Feature Flag + Registry |

---

## 17. Open Questions

| ID | Pergunta | Status |
|---|---|---|
| OQ-M01 | M-02 absorve M-03 ou apenas compõe? | **Resolvido provisoriamente:** compõe, não absorve |
| OQ-M02 | Telemedicine módulo independente? | **Resolvido provisório:** capability em M-03 |
| OQ-M03 | Communication Module dedicado? | **Resolvido provisório:** não nesta fase |
| OQ-M04 | Queue/Flow módulo separado de Scheduling? | Open — pode ser feature de M-01 |
| OQ-M05 | Payer Module na fase inicial? | Weak — Q-006 MVP |
| OQ-C03 | UI composition contract no Core? | Parcial — slot no Core, shell em M-02 |
| Q-006 | MVP por módulo | Deferred — não decidido nesta sessão |
| Q-007 | Fronteira módulos | **Avançou** — falta confirmação do pacote |

---

## 18. Initial Recommendations

1. **D-004** e **D-007** → promover para **Ready for Confirmation** em `decisions.md`.
2. **D-003** (catálogo PS tiers) → Ready for Confirmation.
3. Catálogo de **15 módulos** + Telemedicine como capability — não expandir sem novo domínio.
4. **NR-005** — `extension-model-draft.md`: formalizar Extension Modules M-14, M-15.
5. **NR-006** — `read-models-boundary.md`: Timeline em M-02/M-08; Analytics em M-12.
6. **Q-007:** módulos investigados — Answered conceitualmente após confirmação do pacote.

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1.0 | 2026-07-03 | Esqueleto NR-004 |
| 0.2.0 | 2026-07-03 | Investigação completa — 15 módulos, matriz PS, Clinical Workspace, domain map |

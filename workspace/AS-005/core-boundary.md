# Core Platform Boundary

> **Promovido para documentação oficial:** `docs/05-architecture/core-platform.md` (ADR-0009).  
> Este arquivo permanece como registro histórico da investigação NR-001/NR-002.

**Investigação:** NR-001 / NR-002 — fronteira do Core Platform  
**Status:** Rascunho em revisão  
**Última atualização:** 2026-07-03

---

## 1. Purpose

Definir, como rascunho de trabalho, **o que é o Core Platform** da Health Platform, **o que pertence a ele**, **o que não pertence** e **quais riscos existem** se o Core crescer além do necessário.

Este documento:

- reconcilia o "Core" do ADR-0003 com a decomposição em Platform Services do ADR-0005;
- avalia criticamente cada candidato à fundação estrutural;
- **não** define módulos, APIs, banco de dados ou implementação;
- serve de base para `platform-services-boundary.md` e para a decisão D-001 em `decisions.md`.

**Pergunta central desta etapa:**

> O que permanece no Core Platform depois que responsabilidades transversais foram atribuídas a Platform Services e responsabilidades de negócio a Business Domains?

---

## 2. Definition of Core Platform

Na Health Platform, **Core Platform** é a camada de **fundação estrutural mínima, estável e protegida** sobre a qual Platform Services, Business Domains, Modules e Extensions são construídos.

O Core Platform:

- define **contratos, invariantes e mecanismos estruturais** compartilhados;
- permite que a plataforma **exista como SaaS multi-tenant extensível**;
- **não implementa** regras de negócio clínicas, operacionais ou de modelo operacional específico;
- **não substitui** Platform Services — fornece o chão comum que PS e módulos consomem.

**Metáfora operacional:**

```text
Core Platform     = o que a plataforma É estruturalmente (contratos + invariantes)
Platform Services = o que a plataforma FAZ de forma transversal (implementação reutilizável)
Business Domains  = o que o NEGÓCIO decide (regras + vocabulário)
Modules           = o que o USUÁRIO opera (funcionalidade derivada de domínios)
```

**Relação com ADR-0003:**

O ADR-0003 usa "Core" no sentido amplo de fundação protegida. Nesta investigação, **Core Platform** é o refinamento operacional: a parte **estrutural e contratual** que permanece após separar implementações transversais (PS) e negócio (domínios/módulos).

**Relação com Core Business Capabilities:**

As oito Core Business Capabilities são **vocabulário de negócio permanente** — não são o Core Platform. O Core Platform **não implementa** Relacionar, Executar ou Registrar; ele fornece estrutura para que domínios e módulos o façam.

**Elementos estruturais clínicos no Core (reafirmação ADR-0001 / ADR-0007):**

| Elemento | Papel no Core Platform |
|---|---|
| **Modelo Hierárquico do Cuidado** | Invariantes e tipos estruturais: Patient → Care Journey → Care Episode → Attendance → Clinical Event → Clinical Artifact |
| **Care Journey type system** | Tipos válidos de Care Journey Start Event e estados de ciclo de vida (Active, Paused, Closed) — Core define; instituição configura qual tipo usar |

---

## 3. What Core Platform Is Not

Core Platform **não é**:

| Conceito | Por quê |
|---|---|
| **Business Domain** | Domínios possuem regras de negócio e vocabulário ubíquo — o Core não decide prescrição, agenda ou LGPD |
| **Platform Service** | PS implementam transversal (Identity, Audit, Communication…) — Core define contratos, não executa |
| **Module** | Módulos entregam funcionalidade ao usuário — agenda, atendimento, portal |
| **Extension** | Extensões são módulos/domínios opcionais por modelo operacional |
| **Read Model / View** | Timeline e Analytics são projeções — não fundação |
| **Feature** | Feature deriva de módulo — não é estrutural |
| **UI** | Telas, navegação clínica, shell de atendimento — no máximo contrato de composição, não implementação |
| **API** | Contratos de domínio/API são derivados — Core pode definir contratos base, não endpoints |
| **Database model** | Schema, isolamento multi-tenant (Q-008) — fase técnica |

**Core Platform também não é:**

- regra específica de clínica, prontuário, agenda, telemedicina, laboratório ou home care;
- customização por cliente;
- implementação de autenticação, envio de e-mail, armazenamento ou busca.

---

## 4. Core Inclusion Criteria

Um item só deve pertencer ao Core Platform se **todos** os critérios abaixo forem atendidos (ou fortemente inclinados):

| # | Critério |
|---|---|
| C-01 | É **necessário** para a plataforma existir como SaaS multi-tenant extensível |
| C-02 | É usado por **praticamente todos** os domínios ou módulos |
| C-03 | É **estrutural** — não é regra específica de negócio |
| C-04 | Precisa ser **estável por muitos anos** — mudança exige ADR |
| C-05 | **Permite extensão** sem alteração do próprio Core |
| C-06 | **Não** pode pertencer melhor a um Platform Service (como implementação) |
| C-07 | **Não** pode pertencer melhor a um Business Domain |
| C-08 | **Não** representa feature ou módulo de negócio |

**Teste de inclusão complementar:**

> Se todos os módulos de negócio e Extension Domains forem desabilitados, o Core Platform ainda permite registrar tenants, compor módulos e manter invariantes estruturais válidos?

---

## 5. Core Exclusion Criteria

Um item **não** deve pertencer ao Core Platform se **qualquer** critério abaixo for verdadeiro:

| # | Critério de exclusão |
|---|---|
| E-01 | É regra específica de saúde (clínica, assistencial, operacional) |
| E-02 | É específico de um modelo operacional (ambulatorial, diagnóstico, home care…) |
| E-03 | É específico de um cliente ou tenant no código |
| E-04 | É apenas funcionalidade de um módulo |
| E-05 | É implementação técnica de serviço transversal |
| E-06 | É melhor classificado como **Platform Service** |
| E-07 | É melhor classificado como **Business Domain** |
| E-08 | É read model ou visão de leitura |
| E-09 | É detalhe de UI (layout, navegação clínica, componentes) |
| E-10 | É detalhe de banco, API, infraestrutura ou deploy |

---

## 6. Candidate Core Responsibilities

### Hierarchical Care Model Contracts

**Status:** Strong Candidate

**Classification:** Core Platform

**Description:**

Invariantes estruturais do modelo clínico central (ADR-0001): hierarquia Patient → Care Journey → Care Episode → Attendance → Clinical Event → Clinical Artifact; cardinalidades e níveis conceituais compartilhados por toda a plataforma.

**Why it may belong to Core:**

- Toda operação clínica na plataforma depende desta estrutura.
- Estável, independente de modelo operacional e de módulo.
- ADR-0001 Accepted como fundação clínica.

**Why it may not belong to Core:**

- Poderia ser interpretado como "domínio clínico" — mas é estrutura, não regra de atendimento.

**Risks:**

- Inflar o Core com regras clínicas disfarçadas de "invariante" (ex.: protocolo de prescrição no modelo).

**Recommendation:**

Incluir no Core Platform como **contratos e tipos estruturais** — não regras de conteúdo clínico.

---

### Care Journey Type System

**Status:** Strong Candidate

**Classification:** Core Platform

**Description:**

Tipos válidos de **Care Journey Start Event** (`ReferralAccepted`, `ScheduledCareCommitted`, etc.) e estados de ciclo de vida da Institution Care Journey (Active, Paused, Closed). O Core define o vocabulário; a instituição configura qual tipo aplica por modelo operacional (ADR-0007, ADR-0006).

**Why it may belong to Core:**

- ADR-0007 explicita: "O Core define tipos de evento válidos."
- Ancora toda Institution Care Journey — transversal a domínios.
- Estrutural, não fluxo de agendamento.

**Why it may not belong to Core:**

- Critérios de pausa/encerramento configuráveis poderiam ser confundidos com Core — pertencem a configuração/domínios.

**Risks:**

- Colocar regras de transição específicas por modelo operacional no Core.

**Recommendation:**

Incluir **tipos e estados** no Core; **critérios configuráveis** fora do Core (Configuration Service + domínios).

---

### Tenant Context

**Status:** Strong Candidate

**Classification:** Core Platform (contrato) — implementação via Identity / Configuration PS

**Description:**

Contrato estrutural de **isolamento e identificação de tenant** no SaaS multi-tenant: toda operação ocorre em um tenant; contexto de tenant propagado a módulos e PS.

**Why it may belong to Core:**

- Sem tenant, a plataforma SaaS não existe (C-01).
- Usado por todos os módulos e PS (C-02).
- Estratégia de schema/banco (Q-008) é técnica — o **contrato** de tenant é fundação.

**Why it may not belong to Core:**

- Gestão de usuários e login → Identity Service (PS).
- Branding por tenant → Configuration Service (PS).

**Risks:**

- Implementar multi-tenant no Core em vez de contrato + PS.

**Recommendation:**

**Contrato de Tenant Context** no Core Platform; **implementação** (autenticação, storage de config) nos PS.

---

### Organization Context

**Status:** Needs Review

**Classification:** Core Platform (contrato hierárquico) + Business Domain (Organization Management) para regras

**Description:**

Contexto de **instituição e unidade** dentro de um tenant — hierarquia tenant → instituição → unidade (ADR-0006). Propagação de escopo organizacional em operações da plataforma.

**Why it may belong to Core:**

- Hierarquia organizacional é estrutural para configuração e escopo.
- Múltiplos domínios dependem do escopo (Care Coordination, Governance…).

**Why it may not belong to Core:**

- Cadastro e gestão de organizações → Organization Management **Domain**.
- Herança de configuração (Q-015) pode ser responsabilidade do Configuration Service.

**Risks:**

- Duplicar Organization Management Domain no Core.
- Confundir "contexto propagado" com "regras de rede credenciada".

**Recommendation:**

Core Platform define **contrato de Organization Context** (escopos válidos na hierarquia); **dados e regras** de organização no domínio Organization Management.

---

### User Context

**Status:** Weak Candidate

**Classification:** Core Platform (contrato mínimo) + Platform Service (Identity Service — implementação)

**Description:**

Contexto do **usuário autenticado** em uma sessão: identidade resolvida, tenant e escopo organizacional ativos.

**Why it may belong to Core:**

- Toda operação autenticada precisa de user context propagado.
- Contrato de "quem está operando" é estrutural.

**Why it may not belong to Core:**

- Autenticação, sessões, tokens → **Identity Service** (ADR-0005).
- Papéis e permissões → **Authorization Service**.
- O Core precisa apenas do **contrato** de que user context existe e é propagado — não da implementação.

**Risks:**

- Replicar Identity Service dentro do Core.

**Recommendation:**

**Contrato de User Context** (propagação, não implementação) pode fazer parte do **Runtime Context** no Core; implementação 100% em Identity + Authorization PS.

---

### Module Registry

**Status:** Strong Candidate

**Classification:** Core Platform

**Description:**

Mecanismo estrutural de **registro, descoberta e composição de módulos** habilitados por tenant/instituição. Permite extensão por composição (ADR-0003, ADR-0006).

**Why it may belong to Core:**

- Necessário para "crescimento por extensão" sem alterar fundação (C-05).
- Module enablement é pré-requisito da arquitetura modular.
- ADR-0003 lista extensão via Modules como mecanismo central.

**Why it may not belong to Core:**

- Metadados de módulo específicos poderiam parecer "feature catalog" — mas o **registry** é infraestrutura de plataforma.

**Risks:**

- Registry carregar lógica de negócio de quais módulos uma clínica "deve" ter.

**Recommendation:**

Incluir **Module Registry** (contrato + mecanismo de registro/composição) no Core Platform.

---

### Extension Mechanism

**Status:** Strong Candidate

**Classification:** Core Platform

**Description:**

Infraestrutura de **hooks, contratos e pontos de extensão** que permitem adicionar módulos, PS e integrações sem alterar o Core (ADR-0003). Distinto de **Extension Module** ou **Extension Business Domain**.

**Why it may belong to Core:**

- Princípio central do ADR-0003: "crescimento por extensão."
- Sem mecanismo de extensão, cada novidade ameaça o Core.

**Why it may not belong to Core:**

- Implementação concreta de plugins pode ser técnica demais — Core define **contrato**, não engine de plugins.

**Risks:**

- Extension mechanism virar caminho para customização por cliente (viola ADR-0006).
- Confundir com Extension Domains (Diagnostic, Home Care).

**Recommendation:**

Incluir **Extension Mechanism** (contratos e hooks) no Core Platform; módulos extension são consumidores, não parte do Core.

---

### Configuration Foundation

**Status:** Rejected (como implementação) / Weak Candidate (contrato apenas)

**Classification:** Platform Service (Configuration Service, Feature Flag Service)

**Description:**

Parametrização por tenant, instituição e unidade — branding, workflows, módulos habilitados, políticas operacionais (ADR-0006).

**Why it may belong to Core:**

- Variabilidade é transversal; "fundação de configuração" soa estrutural.

**Why it may not belong to Core:**

- ADR-0005 cataloga **Configuration Service** e **Feature Flag Service** como PS consolidados.
- Armazenamento, schema e APIs de config → PS (Q-017 deferred).
- Core pode referenciar **níveis** de configuração (tenant/instituição/unidade) sem implementar.

**Risks:**

- Core virar repositório de parâmetros de negócio.

**Recommendation:**

**Rejeitar** implementação no Core. **Contrato de níveis de configuração** (referência ADR-0006) pode ser mencionado no Core como invariante — implementação no Configuration PS.

---

### Permission Foundation

**Status:** Rejected

**Classification:** Platform Service (Authorization Service)

**Description:**

Controle do que cada usuário pode ver e fazer — papéis, permissões, políticas de acesso (ADR-0005).

**Why it may belong to Core:**

- Toda operação precisa de autorização.

**Why it may not belong to Core:**

- E-05, E-06: implementação transversal explícita no ADR-0005 como **Authorization Service**.
- Políticas de conformidade clínica → Governance & Compliance Domain.

**Risks:**

- Duplicar Authorization Service no Core.

**Recommendation:**

**Rejeitar** do Core Platform. Core pode exigir invariante: "toda operação passa por autorização" — execução no Authorization PS.

---

### Event Foundation

**Status:** Strong Candidate (contrato limitado)

**Classification:** Core Platform (contrato de publicação/consumo) + Platform Service (Event Bus — identificado, Q-003)

**Description:**

Fundação para **comunicação desacoplada** entre módulos e serviços por eventos (ADR-0003). Contrato de publicação e assinatura — **não** Event Model detalhado nem tecnologia de bus.

**Why it may belong to Core:**

- Desacoplamento entre módulos é estrutural na arquitetura.
- ADR-0003 lista Events como mecanismo de extensão.

**Why it may not belong to Core:**

- Event Bus, tipos de eventos, tecnologia → Q-003 / AS-010.
- Implementação de mensageria → PS ou fase técnica.

**Risks:**

- Definir Event Model no Core por antecipação (escopo AS-010).

**Recommendation:**

Incluir **Event Foundation** como **contrato conceitual** (módulos podem publicar/consumir eventos de domínio); **não** incluir catálogo de eventos ou bus no Core nesta fase.

---

### Audit Foundation

**Status:** Rejected (como implementação)

**Classification:** Platform Service (Audit Service)

**Description:**

Rastreabilidade de ações, log de auditoria, histórico de acesso a dados sensíveis (ADR-0005).

**Why it may belong to Core:**

- "Auditabilidade por padrão" é princípio arquitetural.

**Why it may not belong to Core:**

- E-05, E-06: **Audit Service** consolidado no ADR-0005.
- Regras de *o que* auditar em contexto clínico → domínios + Governance.

**Risks:**

- Implementar logging no Core.

**Recommendation:**

**Rejeitar** implementação no Core. Invariante no Core: "ações relevantes são auditáveis" — execução no Audit PS.

---

### Feature Toggle Foundation

**Status:** Rejected

**Classification:** Platform Service (Feature Flag Service)

**Description:**

Habilitação gradual de funcionalidades por tenant, ambiente ou segmento (ADR-0005).

**Why it may belong to Core:**

- Rollout e composição de módulos usam flags.

**Why it may not belong to Core:**

- Feature Flag Service é PS consolidado, separado de Configuration Service.
- E-04, E-06: é implementação transversal, não fundação estrutural mínima.

**Risks:**

- Core decidir feature por tenant em vez de PS + configuração.

**Recommendation:**

**Rejeitar** do Core Platform. Module Registry + Configuration/Feature Flag PS governam habilitação.

---

### Navigation/Shell Foundation

**Status:** Needs Review

**Classification:** Module (Clinical Workspace — hipótese) ou Needs Review

**Description:**

Estrutura de navegação, shell da aplicação, área de trabalho do profissional — composição de módulos na UI.

**Why it may belong to Core:**

- Toda experiência autenticada precisa de shell mínimo (login → app).

**Why it may not belong to Core:**

- E-09: detalhe de UI.
- Clinical Workspace é candidato a **módulo shell transversal** (T-003, H-WS-001).
- Navegação clínica (prontuário, atendimento) é domínio/módulo — não fundação.

**Risks:**

- Core virar frontend monolítico.
- Confundir "shell técnico vazio" com "Clinical Workspace".

**Recommendation:**

**Não incluir** navegação clínica no Core. Investigar em NR-004 se existe apenas **contrato de composição de módulos na UI** (slot/hook) no Core vs shell completo como módulo. Status: **Needs Review** — depende T-003.

---

### Runtime Context

**Status:** Strong Candidate (como agregador conceitual)

**Classification:** Core Platform

**Description:**

**Agregação estrutural** do contexto de execução de uma operação na plataforma: Tenant + Organization + User + Module scope (e eventualmente Care context references). Propagação consistente a módulos e PS.

**Why it may belong to Core:**

- Unifica Tenant, Organization e User Context em um contrato coeso.
- Praticamente toda operação depende de runtime context (C-02).
- Estrutural — não decide regra clínica.

**Why it may not belong to Core:**

- Pode ser apenas composição de contextos já classificados — não item separado.

**Risks:**

- Runtime Context carregar objetos de domínio clínico (Patient, Journey) — deve carregar **referências**, não regras.

**Recommendation:**

Incluir **Runtime Context** como contrato unificador no Core Platform; subcontextos (tenant, org, user) como partes; **referências clínicas** como IDs, não agregados (Q-004 fora de escopo).

---

### Platform Metadata

**Status:** Weak Candidate

**Classification:** Core Platform (parcial — metadados de registry) / Needs Review

**Description:**

Metadados da plataforma: versão do Core, versão de contratos, metadados de módulos registrados, capacidades declaradas por módulo.

**Why it may belong to Core:**

- Module Registry precisa de metadados para composição.
- Versionamento de contratos é estabilidade.

**Why it may not belong to Core:**

- Pode ser subparte do Module Registry — não categoria separada.
- Versão de deploy/build → infraestrutura.

**Risks:**

- Metadados de negócio por módulo no Core.

**Recommendation:**

Absorver em **Module Registry** como metadados de registro — não item Core separado. Status: **Weak Candidate** — consolidar com Module Registry.

---

### Environment/Deployment Awareness

**Status:** Rejected

**Classification:** Needs Review (fase técnica — fora do escopo AS-005 produto)

**Description:**

Consciência de ambiente (dev, staging, prod), região, deployment, infraestrutura.

**Why it may belong to Core:**

- Feature flags e config às vezes variam por ambiente.

**Why it may not belong to Core:**

- E-10: infraestrutura e deploy — Sprint 3.
- Observability Service cobre saúde técnica operacional.

**Risks:**

- Contaminar Core Platform com DevOps.

**Recommendation:**

**Rejeitar** do Core Platform nesta fase. Tratar em arquitetura técnica futura.

---

### Shared Kernel Types (complementar — não estava na lista original)

**Status:** Strong Candidate

**Classification:** Core Platform

**Description:**

Tipos e identificadores estáveis compartilhados entre domínios: referências a tenant, patient, journey, module, sem regras de negócio embutidas.

**Why it may belong to Core:**

- Contratos estáveis entre módulos (ADR-0003).
- Evita acoplamento de modelos internos entre domínios.

**Why it may not belong to Core:**

- Detalhe de implementação de ID (UUID vs ULID) → fase técnica.

**Risks:**

- Shared kernel virar modelo de domínio completo (anti-pattern DDD).

**Recommendation:**

Incluir como **referências e tipos estruturais mínimos** no Core — não entidades de negócio completas.

---

## 7. Items That Must Not Enter Core

Os seguintes **nunca** devem entrar no Core Platform:

### Regras clínicas e operacionais

- Regras de prontuário, evolução clínica, anamnese
- Regras de atendimento, consulta, teleconsulta, sessão terapêutica
- Regras de agenda, fila, encaminhamento, workflow operacional
- Regras de prescrição, solicitação de exames, ciclo de vida de ordens
- Regras de documentos clínicos, laudos, atestados, artefatos formais
- Regras de laboratório, imagem, produção de resultado diagnóstico
- Regras de visita domiciliar, home care, plano de cuidado domiciliar
- Regras de comunicação com paciente (conteúdo, consentimento de canal)
- Regras de faturamento, convênio, cobertura, TISS
- Protocolos clínicos, alertas clínicos, pendências assistenciais
- Regras de LGPD e políticas de conformidade (→ Governance & Compliance Domain)

### Implementações transversais (→ Platform Services)

- Login, sessão, gestão de usuários
- Papéis, permissões, políticas de acesso
- Log de auditoria, rastreabilidade técnica
- Envio de e-mail, SMS, WhatsApp, push
- Armazenamento de arquivos e blobs
- Busca indexada
- Configuração persistida, feature flags
- Integração FHIR, webhooks, conectores

### Produto e extensão

- Módulos de negócio (Scheduling, Attendance, Portal…)
- Extension Modules (Diagnostic, Home Care)
- Clinical Timeline, Analytics (read models)
- Clinical Workspace como experiência clínica completa

### Variabilidade indevida

- Customizações por cliente no código
- Regras configuráveis por instituição **como código no Core** (devem ser config + domínio)
- Fluxos específicos de especialidade médica

---

## 8. Boundary Examples

| Responsibility | Classification | Reason |
|---|---|---|
| Tenant resolution | Core Platform (contrato) | Fundação SaaS multi-tenant — contexto estrutural |
| Patient registration | Business Domain (Patient Relationship) + Module | Regra de vínculo e cadastro — Relacionar |
| Medical prescription | Business Domain (Clinical Orders) + Module | Ciclo de vida da ordem — vocabulário clínico |
| Audit trail | Platform Service (Audit Service) | Transversal — implementação reutilizável |
| WhatsApp sending | Platform Service (Communication Service) | Canal externo — domínio Communication decide |
| Clinical Timeline | Read Model / View | Projeção cross-domain — sem regras autônomas |
| Module enablement | Core Platform (Module Registry) + Configuration PS | Registry no Core; valores configuráveis no PS |
| Home care visit | Business Domain (Home Care Operations) + Extension Module | Modelo operacional específico |
| Exam request | Business Domain (Clinical Orders) + Module | Ordem clínica — não fundação |
| Document generation | Platform Service (Document Engine) + Domain (Clinical Documents) | Engine transversal; regras no domínio |
| Feature flag | Platform Service (Feature Flag Service) | Rollout transversal |
| LGPD policy | Business Domain (Governance & Compliance) | Política de negócio; enforcement via Audit/Compliance PS |
| Care Journey Start Event types | Core Platform | ADR-0007 — Core define tipos válidos |
| Hierarchical Care levels | Core Platform | ADR-0001 — estrutura clínica central |
| Organization network rules | Business Domain (Organization Management) | Regras de rede — não estrutura |
| User login | Platform Service (Identity Service) | Implementação transversal |
| Runtime context propagation | Core Platform | Agregação estrutural de contextos |
| Navigation in clinical workspace | Module (Clinical Workspace) — Needs Review | UI/orquestração — T-003 |
| Observability metrics | Platform Service (Observability Service) | Saúde técnica |

---

## 9. Risks of a Large Core

| # | Risco | Consequência | Sinal de alerta |
|---|---|---|---|
| R-C01 | **Core monolítico** | Instabilidade, regressões em todo release | Regras clínicas no Core |
| R-C02 | **Core = todos os PS** | ADR-0005 ignorado; duplicação | Identity, Audit no Core |
| R-C03 | **Core = UI shell** | Frontend não evolui por módulo | Telas de atendimento no Core |
| R-C04 | **Core = domínios** | 16 domínios no núcleo | Prescrição, agenda no Core |
| R-C05 | **Core = configuração** | Parâmetros de negócio na fundação | Workflows no Core em código |
| R-C06 | **Core = customização** | Modelo SaaS quebrado | `if (tenant == X)` no Core |
| R-C07 | **Core inchado "por precaução"** | Medo de extensão paralisa evolução | "Já que todos usam, põe no Core" |
| R-C08 | **Contrato sem limite** | "Shared kernel" vira modelo completo | Entidades DDD no Core |
| R-C09 | **Event Model prematuro no Core** | Bloqueio técnico antes de AS-010 | Catálogo de eventos na fundação |
| R-C10 | **Extension mechanism como bypass** | Customização disfarçada de hook | Plugins por cliente no Core |

**Princípio de contenção:**

> Na dúvida entre Core e Platform Service → **Platform Service**.  
> Na dúvida entre Core e Business Domain → **Business Domain**.  
> Na dúvida entre Core e Module → **Module**.

---

## 10. Open Questions

| ID | Pergunta | Relação |
|---|---|---|
| OQ-C01 | Organization Context é contrato Core separado ou parte de Runtime Context? | Organization Context |
| OQ-C02 | User Context merece contrato Core explícito ou só via Identity PS? | User Context |
| OQ-C03 | Existe "UI composition contract" mínimo no Core ou tudo no Clinical Workspace Module? | Navigation/Shell — T-003 |
| OQ-C04 | Platform Metadata é parte do Module Registry ou artefato separado? | Platform Metadata |
| OQ-C05 | Quão detalhado pode ser Event Foundation sem invadir AS-010? | Event Foundation |
| OQ-C06 | Governar como capability vs Governance Domain — o que o Core "sabe" sobre Governar? | ADR-0003 linha Governar |
| OQ-C07 | Referências clínicas no Runtime Context — quais IDs são permitidos no contrato Core? | Runtime Context + Q-004 |
| OQ-C08 | Care Journey transições (pause/close rules) — invariante Core vs config vs domínio? | Care Journey Type System |

---

## 11. Initial Recommendations

### Para inclusão provisória no Core Platform (Strong Candidates)

**Status:** ✅ Validados pelo usuário em 2026-07-03 — pendentes confirmação formal do pacote AS-005.

1. **Hierarchical Care Model Contracts** (ADR-0001)
2. **Care Journey Type System** (ADR-0007)
3. **Tenant Context** (contrato)
4. **Module Registry**
5. **Extension Mechanism** (contratos/hooks)
6. **Event Foundation** (contrato limitado — sem Event Model)
7. **Runtime Context** (agregador de contextos estruturais)
8. **Shared Kernel Types** (referências mínimas estáveis)

### Para rejeição do Core (implementação → PS)

- Configuration Foundation → Configuration + Feature Flag PS
- Permission Foundation → Authorization PS
- Audit Foundation → Audit PS
- Feature Toggle Foundation → Feature Flag PS
- Environment/Deployment Awareness → fase técnica

### Para próxima etapa da AS-005

1. ~~**NR-001b:** Validar com usuário a lista de Strong Candidates~~ ✅ 2026-07-03
2. ~~**NR-003:** Atualizar `platform-services-boundary.md`~~ ✅ 2026-07-03
3. **NR-004:** Derivar módulos candidatos — `module-strategy-draft.md`
4. **NR-004:** Resolver **T-003** (Navigation/Shell) e **OQ-C01** (Organization Context)
5. Não expandir Core com candidatos **Weak** até confirmação do pacote

### Modelo reconciliado ADR-0003 / ADR-0005 (provisório)

```text
Core Platform
├── Contratos clínicos estruturais (Hierarchical Care, Care Journey types)
├── Contratos de contexto (Tenant, Organization, Runtime)
├── Module Registry + Extension Mechanism
├── Event Foundation (contrato)
└── Shared Kernel Types

Platform Services ← implementam transversal (Identity, Audit, Config, …)
Business Domains  ← regras de negócio
Modules           ← funcionalidade
```

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1.0 | 2026-07-03 | Rascunho inicial NR-001 |
| 0.2.0 | 2026-07-03 | Investigação completa dos 14 candidatos + estrutura AS-005 |
| 0.2.1 | 2026-07-03 | Strong Candidates validados pelo usuário |

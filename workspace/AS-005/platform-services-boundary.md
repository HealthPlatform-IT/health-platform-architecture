# Platform Services Boundary

> **Promovido para documentação oficial:** `docs/05-architecture/platform-services.md` (ADR-0009 D-003).  
> Este arquivo permanece como registro histórico da investigação NR-003.

**Investigação:** NR-003 — fronteira dos Platform Services  
**Pré-requisito:** `core-boundary.md` v0.2.1 — Core Platform validado  
**Status:** Rascunho em revisão  
**Última atualização:** 2026-07-03

---

## 1. Purpose

Definir, como rascunho de trabalho, **o que são Platform Services** na Health Platform, **o que pertence a eles**, **o que não pertence** e **como se diferenciam** de Core Platform, Business Domains, Modules, Extensions e Read Models.

Este documento:

- avalia criticamente cada serviço candidato do catálogo ADR-0005 (+ Compliance Service hipótese);
- estabelece critérios de inclusão e exclusão;
- documenta fronteiras com Core (D-002) e domínios (D-002 AS-004);
- **não** define APIs, tecnologia, schemas ou implementação;
- **não** altera o ADR-0005 Accepted — interpreta e refine fronteiras para a AS-005.

---

## 2. Definition of Platform Service

Na Health Platform, um **Platform Service** é uma **implementação reutilizável** de responsabilidade **transversal**, exposta por **contrato estável**, **sem interface de usuário final**, consumida por Business Domains, Modules e outros Platform Services.

| Característica | Descrição |
|---|---|
| Transversal | Usado por mais de um domínio ou módulo |
| Mecanismo, não política | Executa *como* — domínio define *o quê* e *por quê* |
| Contrato estável | API ou interface bem definida para consumidores |
| Sem UI | Não é tela, módulo ou feature de usuário |
| Protege o Core | Evita que implementações transversais inflacionem o Core Platform |
| Evoluível | Pode evoluir sem alterar contratos do Core Platform |

**Princípio (ADR-0005):** *dividir para conquistar, compartilhar para escalar*.

```text
Regra específica de domínio     → Module
Reutilizável e transversal      → Platform Service
Contrato estrutural / invariante → Core Platform
```

---

## 3. What Platform Service Is Not

Platform Service **não é**:

| Conceito | Diferença |
|---|---|
| **Core Platform** | Core define contratos e invariantes; PS implementa transversal operacional |
| **Business Domain** | Domínio possui vocabulário ubíquo e regras de negócio próprias |
| **Module** | Módulo entrega funcionalidade ao usuário derivada de domínio(s) |
| **Extension** | Extension Module é opcional por modelo operacional — não transversal da plataforma |
| **Read Model / View** | Projeção cross-domain sem regras autônomas (Timeline, Analytics) |
| **Feature** | Feature deriva de módulo |
| **UI** | Telas, navegação, shell clínico |
| **Domínio técnico genérico** | Biblioteca interna sem contrato de plataforma e ownership |

**Teste rápido:** se um especialista de saúde reconheceria vocabulário de negócio exclusivo do serviço → provavelmente **não** é Platform Service.

---

## 4. Relationship with Core Platform

O **Core Platform** (validado em `core-boundary.md`) define **contratos e invariantes estruturais**. Os **Platform Services** **implementam** responsabilidades transversais **dentro** desses invariantes.

```text
┌─────────────────────────────────────────────────────────────┐
│  CORE PLATFORM                                              │
│  Hierarchical Care contracts · Care Journey types           │
│  Tenant / Runtime Context · Module Registry                 │
│  Extension Mechanism · Event Foundation · Shared Kernel     │
└──────────────────────────┬──────────────────────────────────┘
                           │ PS honram invariantes I-01 a I-10
┌──────────────────────────▼──────────────────────────────────┐
│  PLATFORM SERVICES                                          │
│  Identity · Authorization · Audit · Config · Communication… │
└──────────────────────────┬──────────────────────────────────┘
                           │ consumidos por
┌──────────────────────────▼──────────────────────────────────┐
│  BUSINESS DOMAINS · MODULES · READ MODELS                   │
└─────────────────────────────────────────────────────────────┘
```

### Invariantes do Core que todo PS deve honrar

| # | Invariante | Implicação para PS |
|---|---|---|
| I-01 | Tenant Context obrigatório | Toda operação em escopo de tenant |
| I-02 | Runtime Context propagado | PS recebe tenant, org scope, user ref |
| I-03 | Hierarchical Care Model | PS clínicos respeitam níveis ADR-0001 |
| I-04 | Care Journey types | PS não inventam tipos de start event |
| I-05 | Module Registry | PS declarados; módulos consomem por contrato |
| I-06 | Extension Mechanism | Novo PS não altera Core sem ADR |
| I-07 | Event Foundation | PS publicam/consomem por contrato — sem catálogo de eventos no Core |
| I-08 | Shared Kernel Types | PS usam referências (IDs), não modelos internos de domínio |
| I-09 | Auditabilidade | Ações relevantes via Audit Service |
| I-10 | Autorização obrigatória | PS não bypassam Authorization para dados sensíveis |

**T-001 (ADR-0003 vs ADR-0005):** resolvida — Core = contratos; PS = implementações rejeitadas do Core (Identity, Audit, Config, etc.).

---

## 5. Relationship with Business Domains

Business Domains possuem **regras de negócio** e **vocabulário ubíquo**. Consomem Platform Services para **mecanismos transversais** — não os reimplementam.

```text
Domínio:  O QUE fazer · POR QUÊ · política · vocabulário
PS:       COMO executar · canal · protocolo · armazenamento · registro técnico
```

### Padrão Domain + PS (exemplos validados)

| Domínio | Política (domínio) | Mecanismo (PS) |
|---|---|---|
| **Communication** | Consentimento de canal, preferências, *quando* comunicar | Communication Service, Notification Service, Template Service |
| **Integration** | Contrato clínico, roteamento semântico, interoperabilidade | Integration Service, Webhook Service |
| **Governance & Compliance** | Política LGPD, retenção, *o que* auditar | Audit Service, Authorization Service, Compliance Service *(hip.)* |
| **Clinical Documents** | Regras de laudo, artefato formal, assinatura | Document Engine, Template Service, File Service |
| **Clinical Record** | Conteúdo clínico, evolução, diagnóstico | Medical Form Engine *(captura)*, File Service |
| **Clinical Orders** | Ciclo de vida da ordem | Template Service, Document Engine, Event Bus *(fut.)* |

### Sub-capabilities mapeadas para PS (domain-map — não viram domínio)

| Sub-capability | Platform Service |
|---|---|
| Identity & Access | Identity Service |
| Authorization & Permissions | Authorization Service |
| Audit & Traceability | Audit Service |
| Configuration & Feature Management | Configuration + Feature Flag Services |
| Internal Notifications | Notification Service |
| Observability | Observability Service |

**Regra D-002 (AS-004):** se há vocabulário de negócio coeso → domínio; se transversal técnico → PS.

---

## 6. Platform Service Inclusion Criteria

Um item deve ser **Platform Service** quando **todos** os critérios forem atendidos (ou fortemente inclinados):

| # | Critério |
|---|---|
| INC-01 | Implementa responsabilidade **transversal** |
| INC-02 | É **reutilizado** por múltiplos domínios ou módulos |
| INC-03 | Possui **contrato estável** para consumidores |
| INC-04 | **Não** representa regra de negócio principal |
| INC-05 | **Não** é tela ou módulo de usuário |
| INC-06 | Pode ser consumido por **Business Domains** |
| INC-07 | Pode ser consumido por **Modules** |
| INC-08 | **Protege o Core** de responsabilidades transversais |
| INC-09 | **Evita duplicação** de implementação entre módulos |
| INC-10 | Pode **evoluir** sem alterar contratos do Core Platform |

---

## 7. Platform Service Exclusion Criteria

Um item **não** deve ser Platform Service se **qualquer** critério for verdadeiro:

| # | Critério de exclusão |
|---|---|
| EXC-01 | Possui vocabulário de negócio próprio e regras de domínio |
| EXC-02 | Representa responsabilidade **central de negócio** (capability → domínio) |
| EXC-03 | É específico de **um modelo operacional** (→ Extension Module) |
| EXC-04 | É **feature de usuário** (→ Module) |
| EXC-05 | É **read model** ou visão (→ Clinical Timeline, Analytics) |
| EXC-06 | É **extensão opcional** por tenant, não transversal da plataforma |
| EXC-07 | É biblioteca técnica **sem contrato de plataforma** |
| EXC-08 | É **entidade de domínio** disfarçada (Patient, Order, Journey como "serviço") |
| EXC-09 | É implementação **específica de cliente** |
| EXC-10 | Pertence melhor a Core Platform (contrato), Domain, Module ou Read Model |

---

## 8. Candidate Platform Services

> **Legenda de status:**  
> **Confirmed Candidate** = ADR-0005 Consolidado + validação AS-005  
> **Strong Candidate** = ADR-0005 Identificado ou hipótese forte — detalhe em sessão futura  
> **Weak Candidate** = possível PS, evidência insuficiente  
> **Needs Review** = direção forte, decisão pendente  
> **Rejected** = não é Platform Service

---

### Identity Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Gerencia identidade de usuários, autenticação, sessões e contexto multi-tenant.

**Primary Responsibility:**

Autenticar usuários e estabelecer sessão com Tenant Context e User Context (contratos Core).

**Consumed by:**

Todos os módulos e domínios com usuário autenticado; Authorization Service; Audit Service.

**Related Business Domains:**

Professional Management (cadastro de profissional — domínio; Identity só autentica).

**Related Core Contracts:**

Tenant Context, User Context, Runtime Context (I-01, I-02).

**What belongs here:**

Login, tokens, sessões, resolução de identidade, contexto multi-tenant na sessão.

**What does not belong here:**

Cadastro de profissional/paciente (domínios); autorização (Authorization PS); políticas de acesso (Governance Domain).

**Risks:**

Virar "domínio de usuários"; duplicar Professional Management.

**Recommendation:**

Manter como PS consolidado. Domínio cuida de *quem é* o profissional/paciente no negócio; Identity cuida de *autenticação*.

---

### Authorization Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Controla o que cada usuário pode ver e fazer — papéis, permissões, políticas por recurso e contexto clínico.

**Primary Responsibility:**

Avaliar e aplicar permissões de acesso (mecanismo).

**Consumed by:**

Todos os módulos; PS que acessam dados sensíveis; domínios via módulos.

**Related Business Domains:**

Governance & Compliance (políticas de acesso clínico, LGPD); domínios clínicos (escopo de prontuário).

**Related Core Contracts:**

I-10 — autorização obrigatória; Runtime Context.

**What belongs here:**

Permission check, papéis, políticas de acesso por recurso, RBAC/ABAC técnico.

**What does not belong here:**

Definição de política institucional de LGPD (Governance Domain); login (Identity PS).

**Risks:**

Embutir regra clínica de acesso ("só médico assistente vê") como código fixo no PS — deve ser configurável/política do domínio.

**Recommendation:**

Manter PS. Domínio define política; Authorization executa.

---

### Audit Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Registra e rastreia ações relevantes — log de auditoria, histórico de alterações, rastreabilidade de acesso a dados sensíveis.

**Primary Responsibility:**

Registro técnico imutável de eventos de auditoria (mecanismo).

**Consumed by:**

Todos os módulos e PS relevantes; Governance Domain (evidência).

**Related Business Domains:**

Governance & Compliance (*o que* auditar, retenção, conformidade).

**Related Core Contracts:**

I-09 — auditabilidade.

**What belongs here:**

Audit trail, registro de acesso a prontuário, histórico de alteração técnica.

**What does not belong here:**

Política de retenção LGPD; decisão de conformidade; métricas de infraestrutura (Observability).

**Risks:**

Confundir com Observability; virar domínio de compliance.

**Recommendation:**

Manter PS. Governance define política; Audit registra.

---

### Configuration Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Gerencia configurações da plataforma por tenant, instituição e unidade — parametrização, branding, workflows configuráveis.

**Primary Responsibility:**

Persistir e entregar configuração por escopo hierárquico (mecanismo).

**Consumed by:**

Todos os módulos; Module Registry (habilitação); domínios via parametrização.

**Related Business Domains:**

Organization Management (decisões institucionais); Care Coordination (parâmetros operacionais).

**Related Core Contracts:**

Níveis tenant/instituição/unidade (ADR-0006); Module Registry (I-05).

**What belongs here:**

Tenant configuration, parâmetros operacionais, branding, módulos habilitados (valores).

**What does not belong here:**

Regras de negócio clínico; workflows como código; schema de config (Q-017).

**Risks:**

Virar repositório de regras de negócio; Core absorver config.

**Recommendation:**

Manter PS. Valores configuráveis sim; regras de domínio não.

---

### Feature Flag Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Controla habilitação gradual de funcionalidades por tenant, ambiente ou segmento — rollout, experimentos.

**Primary Responsibility:**

Avaliar feature flags e controlar rollout (mecanismo).

**Consumed by:**

Módulos na inicialização; Module Registry em conjunto com Configuration.

**Related Business Domains:**

— (rollout técnico; decisão de habilitar módulo pode ser Organization Management + config).

**Related Core Contracts:**

Module Registry (I-05).

**What belongs here:**

Feature flag evaluation, rollout gradual, experimentos controlados.

**What does not belong here:**

Configuração de negócio permanente (Configuration PS); habilitação de Extension Domain como regra clínica.

**Risks:**

Substituir Configuration Service; flags permanentes de negócio.

**Recommendation:**

Manter PS separado de Configuration (ADR-0005). Permanente → Config; rollout → Feature Flag.

---

### Communication Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Orquestra comunicação com **pessoas** por canais **externos** — e-mail, SMS, WhatsApp, push para destinatários externos à sessão in-app.

**Primary Responsibility:**

Entrega de mensagens por canais externos (mecanismo de canal).

**Consumed by:**

Communication Domain (orquestração); Patient Portal; Care Monitoring; módulos que notificam pacientes externos.

**Related Business Domains:**

**Communication** — política, consentimento de canal, *quando* e *o quê* comunicar.

**Related Core Contracts:**

Runtime Context (destinatário autorizado); I-10.

**What belongs here:**

WhatsApp delivery, e-mail, SMS, push externo, renderização de mensagem com template.

**What does not belong here:**

Message policy (Communication Domain); communication consent (Communication + Governance); alerta in-app (Notification PS).

**Risks:**

Absorver domínio Communication; violar fronteira ADR-0004 (pessoas vs sistemas).

**Recommendation:**

Manter PS. Q-010 parcialmente endereçada — canal externo = Communication Service.

---

### Notification Service

**Status:** Confirmed Candidate *(fronteira Q-010 em refinamento)*

**Classification:** Platform Service

**Description:**

Gerencia notificações **in-app** e alertas para usuários **autenticados** na plataforma — caixa de notificações, alertas em tempo real internos.

**Primary Responsibility:**

Entrega de notificações in-app (mecanismo de canal interno).

**Consumed by:**

Communication Domain; Care Monitoring; Operations Monitoring; módulos com alertas internos.

**Related Business Domains:**

**Communication** (decisão de notificar); Care Monitoring (alertas clínicos); sub-capability Internal Notifications → PS no domain-map.

**Related Core Contracts:**

Runtime Context (usuário autenticado); I-10.

**What belongs here:**

Notification in-app, caixa de notificações, alertas para profissional logado.

**What does not belong here:**

Push ao paciente no app externo (Communication PS); política de alerta clínico (Care Monitoring Domain).

**Risks:**

Duplicar Communication Service; confundir alerta clínico (domínio) com entrega (PS).

**Recommendation:**

Manter PS separado. **Q-010:** avançou conceitualmente (externo vs in-app) — formalização em ADR revisão pendente, não bloqueia catálogo.

---

### Integration Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Orquestra troca de dados com **sistemas externos** por conectores e protocolos — FHIR, TISS, PACS, LIS, ERPs.

**Primary Responsibility:**

Executar integração por protocolo/conector (mecanismo).

**Consumed by:**

Integration Domain; Diagnostic Operations; módulos com interoperabilidade.

**Related Business Domains:**

**Integration** — contratos clínicos, roteamento, semântica de interoperabilidade.

**Related Core Contracts:**

Extension Mechanism (I-06).

**What belongs here:**

Integration connector, transformação de payload, protocolo FHIR/TISS/PACS.

**What does not belong here:**

FHIR mapping **policy** clínica (Integration Domain); webhooks HTTP genéricos (Webhook PS); comunicação com pessoas (Communication PS).

**Risks:**

Absorber domínio Integration; FHIR Messaging em zona limítrofe (Q-011).

**Recommendation:**

Manter PS. Domínio define *o quê* integrar semanticamente; PS executa protocolo.

---

### Webhook Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Gerencia recebimento e despacho de **webhooks HTTP** — registro de endpoints, validação de assinatura, retry, roteamento.

**Primary Responsibility:**

Callbacks HTTP como padrão de integração (mecanismo).

**Consumed by:**

Integration Domain; módulos financeiros futuros; gateways externos.

**Related Business Domains:**

**Integration** — registro de endpoints, roteamento de eventos recebidos.

**Related Core Contracts:**

Extension Mechanism (I-06).

**What belongs here:**

Webhook HTTP entrada/saída, validação de assinatura, retry de callback.

**What does not belong here:**

Conector FHIR (Integration PS); eventos internos de domínio (Event Bus).

**Risks:**

Virar Integration Service genérico; confundir webhook externo com evento interno.

**Recommendation:**

Manter PS separado de Integration Service (ADR-0005).

---

### Storage Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Abstrai armazenamento de **blobs e objetos binários** — buckets, URLs assinadas, políticas de storage técnico.

**Primary Responsibility:**

Onde e como bytes são armazenados (mecanismo).

**Consumed by:**

File Service; Document Engine; módulos com upload binário.

**Related Business Domains:**

— (metadados clínicos de arquivo → File Service + Clinical Documents).

**Related Core Contracts:**

Tenant Context — isolamento (I-01).

**What belongs here:**

File storage de blob, bucket, URL assinada, retenção técnica de storage.

**What does not belong here:**

Metadados e ciclo de vida clínico de arquivo (File PS); regra de documento clínico (Clinical Documents Domain).

**Risks:**

File Service absorvido no Storage; vínculo clínico no blob layer.

**Recommendation:**

Manter PS. Storage = bytes; File = o que é o arquivo.

---

### File Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Gerencia ciclo de vida de **arquivos** — metadados, organização, controle de acesso a arquivos, vinculação a contextos clínicos e operacionais.

**Primary Responsibility:**

Metadados e ciclo de vida de arquivo (mecanismo).

**Consumed by:**

Clinical Documents, Clinical Record, Communication (anexos); Document Engine.

**Related Business Domains:**

Clinical Documents (artefatos formais); Clinical Record (anexos clínicos).

**Related Core Contracts:**

Shared Kernel Types (referências a arquivo); I-03, I-10.

**What belongs here:**

Metadados de arquivo, organização, vínculo técnico a contexto clínico autorizado.

**What does not belong here:**

Regra de laudo ou documento clínico (Clinical Documents Domain); blob storage (Storage PS).

**Risks:**

Embutir regra de "documento oficial" no File Service.

**Recommendation:**

Manter PS. Domínio define natureza clínica; File gerencia arquivo como entidade técnica.

---

### Search Service

**Status:** Strong Candidate

**Classification:** Platform Service

**Description:**

Prover busca global e indexada across domínios — full-text, pacientes, profissionais, documentos autorizados.

**Primary Responsibility:**

Indexação e busca (mecanismo).

**Consumed by:**

Read Models (Clinical Timeline); módulos com busca global; Clinical Workspace (hipótese).

**Related Business Domains:**

— (indexa dados de domínios; não possui vocabulário de negócio).

**Related Core Contracts:**

Shared Kernel Types; I-10 (resultados filtrados por autorização).

**What belongs here:**

Search indexing, busca full-text cross-domain.

**What does not belong here:**

Clinical Timeline como domínio; regras de o que é buscável clinicamente (Authorization + domínios).

**Risks:**

Virar domínio "Search"; confundir com infraestrutura pura sem contrato de plataforma (OQ-PS02).

**Recommendation:**

Manter como **Strong Candidate** PS. Infraestrutura de busca com contrato de plataforma — não read model.

---

### Document Engine

**Status:** Strong Candidate *(ADR-0005 Identificado — AS-007)*

**Classification:** Platform Service

**Description:**

Gera, renderiza e gerencia **documentos clínicos formais** a partir de templates e dados estruturados — laudos, atestados, relatórios, receitas.

**Primary Responsibility:**

Renderização e geração de documento formal (mecanismo).

**Consumed by:**

Clinical Documents Module; Care Delivery (consentimentos formais); Clinical Orders (receitas).

**Related Business Domains:**

**Clinical Documents** — regras de laudo, artefato, assinatura, ciclo de vida do documento.

**Related Core Contracts:**

Hierarchical Care Model — Clinical Artifact level (I-03).

**What belongs here:**

Document generation, renderização PDF/HTML formal, versionamento técnico de render.

**What does not belong here:**

Medical document rule (Clinical Documents Domain); formulário dinâmico em atendimento (Medical Form Engine); template storage (Template PS).

**Risks:**

Embutir regra clínica de laudo no engine; virar módulo de documentos.

**Recommendation:**

Manter como PS **Strong Candidate**. Detalhe de contratos: AS-007. Domínio define *o que* é documento válido; Engine renderiza.

---

### Medical Form Engine

**Status:** Strong Candidate *(ADR-0005 Identificado — AS-006)* — **Needs Review** (fronteira com domínios)

**Classification:** Platform Service

**Description:**

Prover formulários clínicos **dinâmicos e configuráveis** para registro durante o cuidado — definição por especialidade, renderização em atendimento, validação de campos.

**Primary Responsibility:**

Renderização e validação estrutural de formulários dinâmicos (mecanismo).

**Consumed by:**

Care Delivery Module; Clinical Record Module; Attendance flows.

**Related Business Domains:**

**Clinical Record** (conteúdo clínico capturado); **Care Delivery** (contexto de atendimento).

**Related Core Contracts:**

Hierarchical Care Model — Clinical Event level (I-03).

**What belongs here:**

Clinical form definition (estrutura de campos), renderização em atendimento, validação de tipo de campo.

**What does not belong here:**

Conteúdo clínico / evolução (Clinical Record Domain); regra de diagnóstico; documento formal finalizado (Document Engine).

**Risks:**

Engine virar "prontuário"; capturar regra clínica em definição de formulário sem separar domínio.

**Recommendation:**

Manter como **Strong Candidate** PS com **Needs Review** na fronteira: *definição de formulário* pode parecer domínio — Engine fornece **mecanismo de captura estruturada**; semântica clínica permanece em Clinical Record. AS-006 decidirá contratos.

---

### Template Service

**Status:** Strong Candidate — **Needs Review** (Q-014)

**Classification:** Platform Service

**Description:**

Gerencia templates reutilizáveis de mensagens e documentos — versionamento, variáveis dinâmicas.

**Primary Responsibility:**

Armazenamento e resolução de templates (mecanismo).

**Consumed by:**

Communication Service, Document Engine, Notification Service; domínios Communication e Clinical Documents.

**Related Business Domains:**

Communication (templates de mensagem); Clinical Documents (templates de documento).

**Related Core Contracts:**

Shared Kernel Types.

**What belongs here:**

Template rendering (estrutura), variáveis, versionamento de template.

**What does not belong here:**

Conteúdo da mensagem decidido pelo domínio; regra de documento clínico.

**Risks:**

Absorção por Document Engine ou Communication Service (Q-014); template com regra de negócio embutida.

**Recommendation:**

Manter **Strong Candidate** independente provisoriamente (ADR-0005). **Q-014** permanece Open — revisar em AS-006/AS-007.

---

### Observability Service

**Status:** Confirmed Candidate

**Classification:** Platform Service

**Description:**

Monitora saúde **técnica** e operacional da plataforma — logs, métricas, tracing, alertas de infraestrutura.

**Primary Responsibility:**

Observabilidade técnica (mecanismo).

**Consumed by:**

Operações de plataforma; DevOps (fase técnica); PS e módulos (instrumentação).

**Related Business Domains:**

— (Operations Monitoring é domínio de **SLA operacional de negócio**, não métricas técnicas).

**Related Core Contracts:**

— (não confundir com I-09 audit de negócio).

**What belongs here:**

Métricas de infraestrutura, tracing distribuído, health checks técnicos.

**What does not belong here:**

Audit trail de acesso clínico (Audit PS); SLA operacional de negócio (Operations Monitoring Domain).

**Risks:**

Confundir com Audit Service; absorver Operations Monitoring Domain.

**Recommendation:**

Manter PS. Audit = negócio/compliance; Observability = técnico.

---

### Event Bus / Event Service

**Status:** Strong Candidate *(ADR-0005 Identificado — Q-003 Deferred)*

**Classification:** Platform Service

**Description:**

Comunicação assíncrona desacoplada entre módulos e serviços por eventos de domínio — publicação, assinatura, roteamento, entrega.

**Primary Responsibility:**

Implementar mensageria assíncrona (mecanismo) — implementação do Event Foundation (Core).

**Consumed by:**

Todos os módulos e domínios que publicam/consomem eventos; PS entre si.

**Related Business Domains:**

— (significado do evento = domínio publicador; bus não interpreta negócio).

**Related Core Contracts:**

Event Foundation (I-07) — contrato Core; catálogo de eventos **não** no Core nesta fase.

**What belongs here:**

Event publishing, subscription, routing, delivery, retry técnico.

**What does not belong here:**

Clinical event meaning (Care Delivery / Clinical Record); Event Model catalog (AS-010); webhooks externos (Webhook PS).

**Risks:**

Catálogo de eventos no PS antes de AS-010; Event Bus no Core.

**Recommendation:**

Manter como **Strong Candidate** PS. **Q-003** permanece **Deferred** — tecnologia e Event Model em AS-010. Core tem Event Foundation (contrato); Bus é implementação.

---

### Compliance Service

**Status:** Needs Review *(hipótese AS-004 — Q-019 Open)*

**Classification:** Platform Service *(hipótese)* — alternativa: extensão do Audit Service

**Description:**

Enforcement técnico automatizado de políticas de conformidade definidas pelo domínio Governance & Compliance — LGPD, retenção, anonimização.

**Primary Responsibility:**

Executar enforcement de política (mecanismo) — *se* confirmado como PS separado.

**Consumed by:**

Governance & Compliance Domain; Clinical Record, Clinical Documents (execução de exclusão/anonimização autorizada).

**Related Business Domains:**

**Governance & Compliance** — LGPD policy, data processing consent, políticas de retenção.

**Related Core Contracts:**

I-09, I-10.

**What belongs here:**

Data anonymization enforcement técnico, aplicação automatizada de política de retenção *após* decisão do domínio.

**What does not belong here:**

LGPD policy (Governance Domain); audit trail (Audit PS); consentimento de negócio (Communication / Governance).

**Risks:**

Duplicar Audit; virar domínio Compliance separado; PS com regra jurídica embutida.

**Recommendation:**

**Needs Review.** Hipótese forte de PS complementar a Audit — **não confirmar** no catálogo sem decisão explícita. **Q-019** permanece **Open** — avançou análise na AS-005, sem Answered.

---

## 9. Service Boundary Examples

| Responsibility | Classification | Reason |
|---|---|---|
| Login | Platform Service (Identity) | Autenticação transversal — não regra de negócio |
| Permission check | Platform Service (Authorization) | Mecanismo de acesso |
| Audit trail | Platform Service (Audit) | Registro técnico transversal |
| WhatsApp delivery | Platform Service (Communication) | Canal externo — domínio decide envio |
| Message policy | Business Domain (Communication) | Regra de *quando* e *o quê* comunicar |
| Communication consent | Business Domain (Communication) | Consentimento de canal — vocabulário de negócio |
| LGPD policy | Business Domain (Governance & Compliance) | Política institucional |
| Data anonymization enforcement | Platform Service (Compliance — hip.) | Mecanismo após política do domínio |
| Document generation | Platform Service (Document Engine) | Renderização formal |
| Medical document rule | Business Domain (Clinical Documents) | O que constitui laudo válido |
| Template rendering | Platform Service (Template) | Mecanismo de template |
| Clinical form definition (structure) | Platform Service (Medical Form Engine — hip.) | Mecanismo de captura |
| Clinical record content | Business Domain (Clinical Record) | Evolução, diagnóstico — vocabulário clínico |
| Feature flag evaluation | Platform Service (Feature Flag) | Rollout técnico |
| Tenant configuration | Platform Service (Configuration) | Parametrização por escopo |
| File storage (blob) | Platform Service (Storage) | Bytes |
| Search indexing | Platform Service (Search) | Indexação cross-domain |
| Event publishing | Platform Service (Event Bus) | Mensageria assíncrona |
| Clinical event meaning | Business Domain (Care Delivery / Clinical Record) | Semântica clínica do evento |
| Notification in-app | Platform Service (Notification) | Canal interno autenticado |
| Integration connector | Platform Service (Integration) | Protocolo/conector |
| FHIR mapping policy | Business Domain (Integration) | Semântica de interoperabilidade |
| Patient registration | Business Domain (Patient Relationship) | Vínculo e cadastro — Relacionar |
| Clinical Timeline | Read Model / View | Projeção — não PS |
| Module enablement | Core Platform (Registry) + PS (Config/FF) | Registry no Core; valores no PS |

---

## 10. Risks of Misclassified Platform Services

| # | Risco | Consequência | Mitigação |
|---|---|---|---|
| R-M01 | PS vira domínio de negócio | Domínio vazio ou duplicado | Teste de vocabulário ubíquo; D-002 AS-004 |
| R-M02 | PS contém regra clínica | Lógica clínica não auditável no domínio | Domínio define; PS executa |
| R-M03 | PS vira módulo de usuário | UI no serviço transversal | PS sem UI — INC-05 |
| R-M04 | Domínio reimplementa transversal | Duplicação Identity/Audit/Comm em módulos | Regras R-01 a R-08 |
| R-M05 | Core absorve PS | Core inchado; ADR-0005 ignorado | Princípio de contenção core-boundary |
| R-M06 | PS duplicados | Communication + Notification sobrepostos | Fronteiras ADR-0005 seção 13 |
| R-M07 | Serviço genérico sem owner | "Utils service" sem contrato | Um PS = uma responsabilidade ADR-0005 |
| R-M08 | Engine vira prontuário | Medical Form Engine com semântica clínica | AS-006; domínio Clinical Record |
| R-M09 | Compliance PS com lei embutida | Regra jurídica no código transversal | Governance Domain define política |
| R-M10 | Event Bus interpreta negócio | Acoplamento entre domínios via bus | Evento opaco; significado no publicador |

---

## 11. Open Questions

| ID | Pergunta | Status pós-investigação |
|---|---|---|
| **Q-010** | Communication Service vs Notification Service | **Avançou** — fronteira provisória confirmada (externo vs in-app). Formalização em ADR ainda pendente → permanece **In Analysis** |
| **Q-019** | Compliance Service no catálogo ADR-0005? | **Avançou** — hipótese PS documentada; **não confirmado** → permanece **Open** |
| **Q-003** | Event Model / Event Bus | Event Bus = **Strong Candidate** PS; Event Model = AS-010 → permanece **Deferred** |
| **Q-014** | Template Service independente ou integrado? | **Strong Candidate** independente provisório → permanece **Open** até AS-006/007 |
| **OQ-PS02** | Search Service — PS ou infraestrutura? | Hipótese **PS** com contrato de plataforma |
| **OQ-PS05** | Medical Form Engine — PS ou parte de Clinical Record? | **Strong Candidate** PS; fronteira captura vs conteúdo → **Needs Review** até AS-006 |
| **OQ-PS06** | Authorization embute regra clínica configurável? | Como parametrizar política do Governance Domain |
| **Q-011** | FHIR Messaging — Comunicar ou Integrar? | Integração provável — não bloqueia catálogo PS |

---

## 12. Initial Recommendations

### Catálogo provisório AS-005

| Tier | Serviços |
|---|---|
| **Confirmed Candidate (12)** | Identity, Authorization, Audit, Configuration, Feature Flag, Communication, Notification, Integration, Webhook, Storage, File, Observability |
| **Strong Candidate (5)** | Search, Document Engine, Medical Form Engine, Template Service, Event Bus |
| **Needs Review (1)** | Compliance Service |

**Rejected como PS:** nenhum candidato da lista ADR-0005 foi rejeitado — todos permanecem no catálogo com tier adequado.

### Para `decisions.md`

- Propor **D-003** — Catálogo PS com tiers (Confirmed / Strong / Needs Review).
- Manter **D-002** Ready for Confirmation (relação Core ↔ PS).

### Próxima etapa AS-005

1. **NR-004** — `module-strategy-draft.md`: mapear módulos → PS consumidos por módulo.
2. **NR-007** — `classification-matrix.md`: incorporar critérios INC/EXC deste documento.
3. Não alterar ADR-0005 nesta fase — confirmação do pacote AS-005 primeiro.
4. **Q-010:** suficiente para direção; marcar como parcialmente resolvida na confirmação (não Answered até ADR).
5. **Q-019:** manter Open até decisão explícita sobre Compliance Service.

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1.0 | 2026-07-03 | Rascunho inicial NR-001 |
| 0.2.0 | 2026-07-03 | Reconciliação Core ↔ PS, invariantes I-01 a I-10 |
| 0.3.0 | 2026-07-03 | Investigação completa — 18 candidatos, critérios INC/EXC, exemplos, riscos |

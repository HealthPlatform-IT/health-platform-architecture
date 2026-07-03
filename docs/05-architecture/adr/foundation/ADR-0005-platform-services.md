---
title: ADR-0005 - Platform Services
status: Accepted
date: 2026-07-02
deciders:
  - Architecture Team
tags:
  - architecture
  - platform-services
  - foundation
  - extension
related:
  - architecture-sessions/AS-002-business-capability-map.md
  - workspace/AS-002/decisions.md
  - docs/03-capabilities/core-business-capabilities.md
  - docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md
  - docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md
  - docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md
---

# ADR-0005 — Platform Services

## Status

Accepted

---

# Contexto

A Health Platform precisa centralizar responsabilidades **transversais e reutilizáveis** — identidade, auditoria, comunicação, integração, armazenamento, busca, eventos e configuração — sem duplicá-las em cada módulo de negócio.

Durante a **AS-002 — Business Capability Map**, as **Decisões 001 e 003** estabeleceram o princípio *"dividir para conquistar, compartilhar para escalar"*:

- Funcionalidades específicas de um domínio permanecem no **módulo** correspondente.
- Funcionalidades reutilizáveis por múltiplos domínios devem ser avaliadas como **Platform Services**.

Este ADR complementa o [ADR-0003 — Core Protection and Extension Model](ADR-0003-core-protection-and-extension-model.md) e o [ADR-0004 — Communication and Integration Separation](ADR-0004-communication-and-integration-separation.md), definindo o padrão arquitetural e o **catálogo inicial revisado** de Platform Services.

---

# Problema

Como garantir que:

- Responsabilidades transversais **não sejam duplicadas** em módulos de domínio.
- Módulos consumam capacidades compartilhadas por **contratos estáveis**, sem acoplamento direto.
- Novos módulos e modelos operacionais reutilizem serviços existentes.
- A equipe saiba **quando criar um Platform Service** em vez de implementar localmente.
- Cada serviço identificado tenha **responsabilidade clara** e não sobreponha outro sem justificativa.

---

# Decisão

A Health Platform adotará **Platform Services** como padrão arquitetural para implementar responsabilidades transversais reutilizáveis.

## 1. O que é um Platform Service

Um Platform Service é uma **implementação reutilizável** de responsabilidade transversal, exposta por **contratos estáveis** e consumida por módulos, outros serviços ou engines da plataforma.

| Característica | Descrição |
|---|---|
| Reutilizável | Usado por mais de um módulo ou domínio |
| Transversal | Não pertence exclusivamente a um domínio de negócio |
| Sem UI | Não possui interface de usuário final |
| Contrato estável | API ou interface bem definida para consumidores |
| Independente de módulo | Não é propriedade de um módulo específico |

## 2. O que NÃO é um Platform Service

| Conceito | Diferença |
|---|---|
| Core Business Capability | Capability é competência de negócio; Platform Service é implementação |
| Business Domain | Domínio agrupa regras de negócio; serviço implementa transversal |
| Module | Módulo implementa funcionalidade de domínio específica |
| Core (núcleo) | Core é fundação protegida; Platform Service é mecanismo de extensão |

## 3. Princípio "dividir para conquistar, compartilhar para escalar"

```text
Regra específica de domínio    → Module
Reutilizável e transversal     → Platform Service
Fundação estável da plataforma → Core
```

## 4. Critérios de decisão

Uma funcionalidade deve ser considerada **Platform Service** quando:

1. É usada por **mais de um módulo**.
2. **Não pertence exclusivamente** a um domínio de negócio.
3. Precisa de **contrato estável** para múltiplos consumidores.
4. Possui responsabilidade **transversal**.
5. Deve ser **auditável, configurável ou extensível** de forma centralizada.
6. Pode ser **reutilizada** por diferentes modelos operacionais.

| Se... | Então... |
|---|---|
| Usado por um único módulo | Implementar no **módulo** |
| Reutilizável por múltiplos módulos | Avaliar **Platform Service** |
| Fundação estável e essencial | Avaliar **Core** (com ADR) |
| Regra específica de cliente | **Configuração** ou extensão — nunca Core |

## 5. Regras de consumo

1. Módulos **consomem** Platform Services por contrato — não reimplementam transversais localmente.
2. Platform Services **não dependem** de módulos de negócio específicos.
3. Novos Platform Services exigem **critério atendido** e registro neste catálogo.
4. Alterações de contrato em serviços consolidados exigem **versionamento** e comunicação aos consumidores.

---

# Catálogo inicial de Platform Services

Cada serviço está classificado como:

| Status | Significado |
|---|---|
| **Consolidado** | Decisão madura — responsabilidade definida |
| **Identificado** | Reconhecido como necessário — detalhamento em sessão ou documento futuro |

> **Nota AS-005 (ADR-0009):** Este ADR usa *Consolidado* / *Identificado* para maturidade de **responsabilidade** por serviço. A **AS-005** introduziu tiers operacionais distintos — **Confirmed** (12), **Strong Candidate** (5), **Needs Review** (1) — para priorização de fronteira Core/PS. Ambos coexistem: *Consolidado* aqui ≠ automaticamente *Confirmed* em AS-005. Ver `docs/05-architecture/platform-services.md` e ADR-0009 D-003.

---

## Governar

### Identity Service — Consolidado

**Capability:** Governar

**Responsabilidade:** Gerenciar identidade de usuários, autenticação, sessões e contexto multi-tenant.

**Escopo:** login, gestão de usuários, tokens, sessões, contexto de tenant.

**Não faz:** autorização de recursos (Authorization Service), auditoria (Audit Service).

---

### Authorization Service — Consolidado

**Capability:** Governar

**Responsabilidade:** Controlar o que cada usuário pode ver e fazer na plataforma.

**Escopo:** papéis, permissões, políticas de acesso, controle por recurso e contexto clínico.

**Não faz:** autenticação (Identity Service), registro de auditoria (Audit Service).

---

### Audit Service — Consolidado

**Capability:** Governar

**Responsabilidade:** Registrar e rastrear ações relevantes realizadas na plataforma.

**Escopo:** log de auditoria, histórico de alterações, rastreabilidade de acesso a dados sensíveis.

**Não faz:** autorização (Authorization Service), observabilidade técnica (Observability Service).

---

### Configuration Service — Consolidado

**Capability:** Governar

**Responsabilidade:** Gerenciar configurações da plataforma por tenant, instituição e unidade.

**Escopo:** parametrização institucional, configurações operacionais, branding por tenant.

**Não faz:** feature toggles de rollout (Feature Flag Service).

---

### Feature Flag Service — Consolidado

**Capability:** Governar

**Responsabilidade:** Controlar habilitação gradual de funcionalidades por tenant, ambiente ou segmento.

**Escopo:** flags de feature, rollout progressivo, experimentos controlados.

**Não faz:** configuração de negócio permanente (Configuration Service).

---

### Observability Service — Consolidado

**Capability:** Governar

**Responsabilidade:** Monitorar a saúde técnica e operacional da plataforma.

**Escopo:** logs técnicos, métricas, tracing, alertas de infraestrutura.

**Não faz:** auditoria de negócio (Audit Service), indicadores clínicos (domínio Monitorar).

---

## Comunicar

### Communication Service — Consolidado

**Capability:** Comunicar

**Responsabilidade:** Entregar mensagens a **pessoas** por canais externos.

**Escopo:** e-mail, WhatsApp, SMS, push notification; templates; preferências de canal; consentimentos; filas; retries; provedores; histórico de envio.

**Decisão relacionada:** [ADR-0004](ADR-0004-communication-and-integration-separation.md)

**Não faz:** notificações in-app (Notification Service), integração com sistemas (Integration Service).

---

### Notification Service — Consolidado

**Capability:** Comunicar

**Responsabilidade:** Gerenciar notificações **dentro da plataforma** para usuários autenticados.

**Escopo:** notificações in-app, caixa de notificações, alertas em tempo real para usuários logados, notificações internas operacionais.

**Fronteira com Communication Service:**

| Situação | Serviço |
|---|---|
| Alerta in-app para profissional logado | Notification Service |
| E-mail de confirmação de consulta ao paciente | Communication Service |
| Push notification no app do paciente | Communication Service |
| Aviso interno na plataforma para recepcionista | Notification Service |

> Resolve parcialmente Q-010 — fronteira baseada no **canal de entrega** (in-app vs. externo).

---

### Template Service — Consolidado

**Capability:** Comunicar (primário), Registrar (secundário)

**Responsabilidade:** Gerenciar templates reutilizáveis de mensagens e documentos.

**Escopo:** templates de comunicação (e-mail, SMS, WhatsApp), templates de documentos clínicos, versionamento de templates, variáveis dinâmicas.

**Não faz:** envio de mensagens (Communication Service), renderização final de documentos clínicos complexos (Document Engine).

---

## Integrar

### Integration Service — Consolidado

**Capability:** Integrar

**Responsabilidade:** Orquestrar troca de dados com **sistemas externos** por conectores e protocolos.

**Escopo:** FHIR, TISS, PACS, DICOM, LIS, ERPs, gateways de pagamento, transformação de payloads, contratos de interoperabilidade.

**Decisão relacionada:** [ADR-0004](ADR-0004-communication-and-integration-separation.md)

**Não faz:** webhooks HTTP genéricos de alto volume (Webhook Service), comunicação com pessoas (Communication Service).

---

### Webhook Service — Consolidado

**Capability:** Integrar

**Responsabilidade:** Gerenciar recebimento e despacho de **webhooks HTTP** entre a plataforma e sistemas externos.

**Escopo:** registro de endpoints, validação de assinatura, retry, roteamento de eventos recebidos, despacho de webhooks de saída.

**Fronteira com Integration Service:**

| Situação | Serviço |
|---|---|
| Receber webhook de gateway de pagamento | Webhook Service |
| Enviar dados clínicos via FHIR para sistema externo | Integration Service |
| Registrar endpoint para notificação de evento | Webhook Service |
| Conector TISS com operadora de saúde | Integration Service |

> Integration Service trata **conectores e protocolos**; Webhook Service trata **callbacks HTTP** como padrão de integração.

---

## Infraestrutura transversal

### Storage Service — Consolidado

**Capability:** Governar (primário)

**Responsabilidade:** Abstrair armazenamento de blobs e objetos binários.

**Escopo:** upload, download, buckets, políticas de retenção de storage, URLs assinadas.

**Não faz:** metadados e ciclo de vida de arquivos clínicos (File Service).

---

### File Service — Consolidado

**Capability:** Governar (primário), Registrar (secundário)

**Responsabilidade:** Gerenciar o ciclo de vida de **arquivos** na plataforma.

**Escopo:** metadados de arquivo, organização, controle de acesso a arquivos, vinculação a contextos clínicos e operacionais.

**Fronteira com Storage Service:** File Service gerencia **o que é o arquivo**; Storage Service gerencia **onde e como é armazenado**.

---

### Search Service — Consolidado

**Capability:** Governar (primário), Monitorar (secundário)

**Responsabilidade:** Prover busca global e indexada across domínios da plataforma.

**Escopo:** indexação, busca full-text, busca de pacientes, profissionais, documentos e registros autorizados.

**Não faz:** regras de negócio dos domínios indexados.

---

### Event Bus — Identificado

**Capability:** Monitorar (primário), Integrar (secundário)

**Responsabilidade:** Prover comunicação assíncrona desacoplada entre módulos e serviços por eventos de domínio.

**Escopo:** publicação, assinatura, roteamento e entrega de eventos internos da plataforma.

**Situação:** identificado como necessário — **Event Model** e tecnologia ainda não definidos (Q-003).

**Não faz:** webhooks externos (Webhook Service), integração com sistemas externos (Integration Service).

---

## Registrar

### Document Engine — Identificado

**Capability:** Registrar

**Responsabilidade:** Gerar, renderizar e gerenciar **documentos clínicos formais** a partir de templates e dados estruturados.

**Escopo:** laudos, atestados, relatórios clínicos, receitas, termos — renderização, versionamento, assinatura digital (futuro).

**Situação:** identificado — decisão detalhada pendente (**AS-007 — Document Engine**).

**Não faz:** formulários clínicos dinâmicos de atendimento (Medical Form Engine), templates genéricos (Template Service).

---

### Medical Form Engine — Identificado

**Capability:** Registrar (primário), Executar (secundário)

**Responsabilidade:** Prover formulários clínicos **dinâmicos e configuráveis** para registro durante o cuidado.

**Escopo:** definição de formulários por especialidade, renderização em atendimento, validação, composição de campos clínicos.

**Situação:** identificado — decisão detalhada pendente (**AS-006 — Medical Form Engine**).

**Não faz:** geração de documentos formais finalizados (Document Engine).

---

# Visão geral do catálogo

```text
Governar
├── Identity Service              [Consolidado]
├── Authorization Service         [Consolidado]
├── Audit Service                 [Consolidado]
├── Configuration Service         [Consolidado]
├── Feature Flag Service          [Consolidado]
└── Observability Service         [Consolidado]

Comunicar
├── Communication Service         [Consolidado]
├── Notification Service          [Consolidado]
└── Template Service              [Consolidado]

Integrar
├── Integration Service           [Consolidado]
└── Webhook Service               [Consolidado]

Infraestrutura transversal
├── Storage Service               [Consolidado]
├── File Service                  [Consolidado]
├── Search Service                [Consolidado]
└── Event Bus                     [Identificado]

Registrar
├── Document Engine               [Identificado — AS-007]
└── Medical Form Engine           [Identificado — AS-006]
```

**Total:** 17 Platform Services — 14 consolidados, 3 identificados.

---

# Alternativas consideradas

## Alternativa A — Cada módulo implementa transversais localmente

**Resultado:** rejeitada — duplicação, inconsistência, acoplamento.

## Alternativa B — Tudo centralizado no Core monolítico

**Resultado:** rejeitada — Core instável, difícil de evoluir (ADR-0003).

## Alternativa C — Platform Services dedicados com contratos

**Resultado:** adotada — reutilização, consistência, extensão protegida.

---

# Justificativa

Platform Services permitem que a Health Platform:

- Centralize responsabilidades transversais sem inflar o Core.
- Ofereça contratos estáveis para módulos de negócio.
- Evolua canais, conectores e infraestrutura independentemente dos módulos.
- Mantenha consistência de auditoria, comunicação, integração e armazenamento.
- Suporte múltiplos modelos operacionais sobre serviços compartilhados.

---

# Regras derivadas

1. **Não duplicar transversal em módulos** — se dois módulos precisam, avaliar Platform Service.
2. **Contrato antes de implementação** — todo Platform Service consolidado expõe contrato definido.
3. **Sem UI em Platform Services** — interface de usuário pertence a módulos.
4. **Registrar no catálogo** — novo serviço deve ser adicionado a este ADR ou a `platform-services.md`.
5. **Respeitar fronteiras** — Communication ≠ Notification ≠ Integration ≠ Webhook conforme definido neste catálogo.
6. **Identificado ≠ implementar** — serviços marcados como Identificado aguardam sessão ou ADR específico antes de implementação.

---

# Consequências

## Positivas

- Catálogo claro com 17 serviços revisados e responsabilidades definidas.
- Fronteiras entre serviços similares documentadas (Communication/Notification, Integration/Webhook, Storage/File).
- Base para `platform-services.md` e Development Guidelines.
- Consumo padronizado por módulos via contratos.

## Negativas

- 17 serviços exigem governança e manutenção de contratos.
- Serviços identificados (Event Bus, Document Engine, Medical Form Engine) ainda precisam de decisões detalhadas.
- Disciplina necessária para não criar serviços desnecessários.

---

# Documentação derivada

| Documento | Relação |
|---|---|
| `docs/05-architecture/platform-services.md` | Catálogo detalhado com contratos conceituais |
| `ai-context/platform-services.md` | Resumo para IAs — a criar |

---

# Relação com outros ADRs

| ADR | Relação |
|---|---|
| [ADR-0002](ADR-0002-capability-driven-architecture.md) | Capabilities definem o quê; Platform Services definem implementação transversal |
| [ADR-0003](ADR-0003-core-protection-and-extension-model.md) | Platform Services são mecanismo principal de extensão |
| [ADR-0004](ADR-0004-communication-and-integration-separation.md) | Communication e Integration Services |
| ADR-0006 — Configuration over Customization | Configuration Service como mecanismo de variabilidade |
| [ADR-0008 — Business Domain Map](ADR-0008-business-domain-map.md) | Critérios Domain vs PS vs Read Model; 16 Business Domains |

---

# Open Questions

| ID | Pergunta | Situação |
|---|---|---|
| Q-003 | Qual será o Event Model e a tecnologia do Event Bus? | Pendente |
| Q-007 | Fronteira exata Core / Platform Services / Modules | **Answered** — ADR-0009, docs AS-005 |
| Q-013 | Document Engine e Medical Form Engine — escopo e contratos detalhados | Pendente — AS-006/AS-007 |
| Q-014 | Template Service deve ser independente ou parte do Communication/Document Engine? | Pendente — revisar após AS-006/AS-007 |

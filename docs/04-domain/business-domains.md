---
title: Business Domains
status: Draft
version: 0.1.0
created: 2026-07-02
updated: 2026-07-02
author: Architecture Team
category: Domain
phase: Product & Architecture Foundation
related:
  - docs/04-domain/domain-map.md
  - docs/03-capabilities/core-business-capabilities.md
  - docs/03-capabilities/business-capability-map.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - architecture-sessions/AS-004-business-domain-map.md
  - workspace/AS-004/confirmation-package.md
---

# Business Domains

> Catálogo oficial dos **16 Business Domains** da Health Platform — responsabilidades de negócio derivadas do Business Capability Map.

Responde **Q-002**. Decisão formal: [ADR-0008](../05-architecture/adr/foundation/ADR-0008-business-domain-map.md).

---

## 1. Purpose

Este documento define os **Business Domains** da Health Platform: responsabilidades de negócio coesas, com vocabulário ubíquo e fronteiras claras, derivadas do agrupamento de sub-capabilities (ADR-0002).

**Não define:** módulos, APIs, agregados DDD, implementação.

---

## 2. Modelo de decomposição

Business Domains emergem do **agrupamento de sub-capabilities relacionadas**, combinado com **responsabilidades transversais via Platform Services** (ADR-0005).

| Conceito | Representa |
|---|---|
| **Business Domain** | Regras de negócio e vocabulário próprios |
| **Platform Service** | Implementação transversal reutilizável |
| **Read Model / View** | Projeção de leitura cross-domain |

Domínios **consomem** Platform Services — não reimplementam responsabilidades transversais.

---

## 3. Catálogo — 16 Business Domains

### 3.1 Core Business Domains (5)

Âncora na Institution Care Journey e no ciclo clínico-operacional.

#### Care Delivery

| Campo | Valor |
|---|---|
| **Capability** | Executar |
| **Sub-capabilities** | Clinical Attendance, Therapeutic Care Delivery |
| **Responsabilidade** | Realizar interações clínicas e terapêuticas — consultas, teleconsultas, sessões, avaliações (nível Attendance, ADR-0001) |
| **Não pertence** | Registros formais primários, ordens, artefatos, execução diagnóstica especializada, home care domiciliar |

#### Clinical Record

| Campo | Valor |
|---|---|
| **Capability** | Registrar |
| **Sub-capabilities** | Clinical Documentation, Diagnostic Recording |
| **Responsabilidade** | Conhecimento clínico — evoluções, anamneses, diagnósticos, hipóteses, conclusões, resultados estruturados |
| **Não pertence** | Ordens, laudos formais, timeline como domínio |

#### Clinical Orders

| Campo | Valor |
|---|---|
| **Capability** | Registrar |
| **Sub-capabilities** | Prescription & Clinical Orders |
| **Responsabilidade** | Intenções e ordens clínicas — prescrições, solicitações, ciclo de vida da ordem |
| **Não pertence** | Execução, resultados, laudos formais, entrega ao paciente |

#### Clinical Documents

| Campo | Valor |
|---|---|
| **Capability** | Registrar |
| **Sub-capabilities** | Clinical Artifacts |
| **Responsabilidade** | Artefatos formais assináveis — atestados, laudos, termos, consentimentos formalizados |
| **Não pertence** | Notas de evolução, geração técnica de PDF (Document Engine PS) |

#### Care Monitoring

| Campo | Valor |
|---|---|
| **Capability** | Monitorar |
| **Sub-capabilities** | Care Pendencies & Follow-up, Clinical Alerts & Protocols |
| **Responsabilidade** | Continuidade e risco clínico — pendências, protocolos, alertas de risco |
| **Não pertence** | SLAs operacionais, entrega técnica de alertas, dashboards agregados |

---

### 3.2 Supporting Business Domains (8)

Habilitam operação; não são o centro do cuidado.

#### Patient Relationship

Vínculo paciente–instituição: cadastro, responsáveis, cuidadores. **Não** inicia Institution Care Journey (ADR-0007).

#### Professional Management

Vínculo com profissionais e prestadores: cadastro, especialidades, credenciamento.

#### Organization Management

Estrutura organizacional: instituições, unidades, redes, parcerias.

#### Payer & Insurance

Vínculo com pagadores: convênios, planos, cobertura. Faturamento **não** pertence aqui (deferred — Q-005).

#### Care Coordination

Planejar e coordenar o cuidado: agenda, recursos, filas, fluxo, encaminhamentos, workflows operacionais.

#### Communication

Políticas de comunicação, preferências de canal, **Communication Consent**, destinatários autorizados, templates de negócio. Execução via Communication Service e Notification Service (PS).

#### Integration

Políticas e contratos de interoperabilidade: FHIR, TISS, LIS/PACS, webhooks, IoT. Execução via Integration Service e Webhook Service (PS).

#### Operations Monitoring

SLAs operacionais institucionais — tempo de espera, prazo de laudo, metas de unidade. Distinto de Care Monitoring (clínico) e Observability Service (infra).

---

### 3.3 Extension Business Domains (2)

Modelos operacionais de execução com extensão específica (ADR-0003).

#### Diagnostic Operations

Execução de serviços diagnósticos: coleta, exame, procedimento diagnóstico.

#### Home Care Operations

Cuidado domiciliar: visitas, equipe de campo, acompanhamento remoto distribuído. Telemedicina permanece em Care Delivery.

---

### 3.4 Cross-cutting Business Domain (1)

#### Governance & Compliance

Políticas institucionais, LGPD, retenção, **Data Processing Consent**, evidências de conformidade, revisão periódica de acesso. Enforcement técnico via Platform Services — não implementação no domínio.

---

## 4. Read Models / Views (não domínios)

| Nome | Origem | Descrição |
|---|---|---|
| **Clinical Timeline** | Registrar | Projeção cronológica cross-domain; prontuário = visão |
| **Analytics & Reporting** | Monitorar | Agregação de indicadores de múltiplos domínios |

---

## 5. Deferred / Future Domains

| Domínio | Motivo |
|---|---|
| **Hospital Operations** | Escopo futuro |
| **Billing & Financial** | Q-005 Open |

---

## 6. Regras de fronteira consolidadas

### Registrar

1. Clinical Record registra conhecimento clínico  
2. Clinical Orders representa intenções e ordens  
3. Clinical Documents representa artefatos formais assináveis  
4. Document Engine = PS · Clinical Timeline = read model  

### Monitorar

1. Care Monitoring = risco e continuidade clínica  
2. Operations Monitoring = SLAs operacionais  
3. Analytics = read model  
4. Alerta clínico → domínio; entrega → PS  

### Governança

Governance & Compliance define políticas; Platform Services executam enforcement; domínios consomem.

### Consentimentos (tipos não intercambiáveis)

| Tipo | Domínio |
|---|---|
| Communication Consent | Communication |
| Data Processing / Privacy Consent | Governance & Compliance |
| Clinical Consent | Care Delivery + Clinical Documents |
| Research Consent | Deferred |

---

## 7. Relacionamentos

- [Domain Map](domain-map.md) — mapeamento capability → domínio
- [Business Capability Map](../03-capabilities/business-capability-map.md)
- [Care Journey Lifecycle](../02-business-processes/care-journey-lifecycle.md)

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1.0 | 2026-07-02 | Versão inicial — AS-004 confirmada |

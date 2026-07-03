# Event Strategy — Boundary

> Rascunho de trabalho AS-010 — NR-001/NR-002. Decisões formalizadas em ADR-0012.

**Pré-requisitos:** ADR-0009 · ADR-0005 · ADR-0004 · ADR-0003 · `core-platform.md`  
**Última atualização:** 2026-07-03

---

## 1. Propósito

Definir **o que é a estratégia de eventos** da Health Platform, **o que pertence a cada camada** e **como se diferencia** de:

- **Event Foundation** (Core)
- **Event Bus** (Platform Service)
- **Clinical Event** / **Care Journey Start Event** (modelo clínico — ADR-0001, ADR-0007)
- **Webhook Service** e **Integration Service** (Integrar)
- **Audit Service**, **Read Models**, **Communication**

---

## 2. Definição proposta (investigativa)

**Event Strategy** = conjunto de decisões sobre **como módulos e domínios** se comunicam de forma **assíncrona e desacoplada** na plataforma — via contrato Core (Event Foundation) e mecanismo PS (Event Bus) — **sem** acoplar semântica de negócio ao transporte.

| Pergunta | Direção investigativa (AS-005) |
|---|---|
| O que é Event Bus? | PS de mensageria interna — publicação, assinatura, roteamento, entrega |
| Por que PS? | Transversal — múltiplos módulos e domínios publicam/consomem |
| Por que não Core? | Implementação operacional; Core só contrato (ADR-0009) |

**OQ-EV02 (investigativo):** Event Bus **não** é parte do Core operacional.

---

## 3. Event Foundation (Core) vs Event Bus (PS)

| Aspecto | Event Foundation (Core) | Event Bus (PS) |
|---|---|---|
| **Natureza** | Contrato / invariante | Implementação / mecanismo |
| **Conteúdo** | Envelope, contexto tenant, regras de publicação | Transporte, retry técnico, roteamento |
| **Catálogo de tipos** | **Não** (I-07) | **Não** interpreta tipos — transporta |
| **Owner semântico** | — | Domínio publicador define significado |

```text
Domínio decide "o que aconteceu"
        ↓
Monta payload + tipo (vocabulário do domínio)
        ↓
Event Foundation valida envelope/contexto
        ↓
Event Bus entrega a assinantes
        ↓
Consumidores (módulos, Read Models, Audit) reagem
```

---

## 4. Taxonomia — três camadas de "evento" (NR-002)

Evitar homonímia entre vocabulário clínico e mensageria.

| Camada | Termo | Exemplo | Owner |
|---|---|---|---|
| **A — Hierárquico clínico** | Clinical Event | Consulta, procedimento no prontuário | ADR-0001 — nível do modelo |
| **A — Ciclo de vida** | Care Journey Start Event | Início da Institution Care Journey | ADR-0007 — gatilho de domínio |
| **B — Domínio (plataforma)** | Domain Event | `Attendance.Started`, `Order.Prescribed` | Business Domain publicador |
| **C — Mensagem** | Platform Event Message | Envelope na bus | Event Bus transporta |

**Regra investigativa:** Camada A **não é** mensagem na bus por si só — domínio pode **emitir** Domain Event (B) quando A ocorre.

---

## 5. O que pertence ao Event Bus

| Pertence | Não pertence |
|---|---|
| Publicação / assinatura / entrega assíncrona | Significado clínico do payload |
| Roteamento técnico entre assinantes internos | Webhooks para sistemas externos |
| Retry / dead-letter *(conceitual)* | Integration com FHIR/HL7 |
| Implementação do contrato Event Foundation | Catálogo de tipos no Core |
| Desacoplamento módulo ↔ módulo | Orquestração de regra de negócio |

---

## 6. Fronteiras com outros PS

| PS / Domínio | Fronteira |
|---|---|
| **Webhook Service** | Entrega **externa** — HTTP callback; não substitui bus interna |
| **Integration Service** | Sistemas externos — pode **consumir** eventos e traduzir, mas Integrar ≠ bus |
| **Communication Service** | Pessoas — domínio pode reagir a evento e **chamar** Communication |
| **Notification Service** | In-app — idem, orquestrado por consumidor |
| **Audit Service** | Registro de conformidade — consumidor ou paralelo síncrono *(NR-005)* |

---

## 7. Exemplos conceituais (classificação)

| # | Situação | Camada A/B | Bus? |
|---|---|---|---|
| E-01 | Paciente inicia jornada na instituição | Care Journey Start Event (A) → Domain Event (B) | Provável publicação |
| E-02 | Médico abre atendimento | Clinical Event (A) + `Attendance.Started` (B) | Provável |
| E-03 | Submit formulário MFE aceito pelo domínio | `ClinicalContent.Accepted` (B) | Provável |
| E-04 | Receita formalizada (DE) | `ClinicalArtifact.Formalized` (B) | Provável |
| E-05 | Atualizar Clinical Timeline | Consumo de B por Read Model | Assinante |
| E-06 | Registrar auditoria LGPD | Audit consome B ou API dedicada | A validar NR-005 |
| E-07 | Notificar paciente por SMS | Domínio → Communication PS | **Não** é evento de bus para canal externo direto |
| E-08 | Enviar resultado para laboratório externo | Integration PS | Fora da bus interna |
| E-09 | Callback HTTP para parceiro | Webhook PS | Fora da bus interna |

---

## 8. Decisões candidatas (NR-001)

| ID | Proposta investigativa | Status |
|---|---|---|
| D-001 | Event Bus = PS (**Confirmed** — 15º) | ✅ Confirmed — ADR-0012 |
| D-002 | Foundation (Core) vs Bus (PS) conforme §3 | ✅ Confirmed — ADR-0012 |
| D-003 | Taxonomia três camadas §4 | ✅ Confirmed — ADR-0012 |
| D-004 | Tipos de evento — ownership no **domínio publicador** | ✅ Confirmed — ADR-0012 |
| D-005 | Módulo orquestra; domínio publica | ✅ Confirmed — ADR-0012 |
| D-006 | Webhook/Integration fora da bus | ✅ Confirmed — ADR-0012 |
| D-007 | Read Models + Audit — ver `audit-readmodels-draft.md` | ✅ Confirmed — ADR-0012 |
| D-008 | Tecnologia broker — **Deferred Sprint 3** | ✅ Confirmed — ADR-0012 |

---

## 9. Anti-patterns

| Anti-pattern | Correção |
|---|---|
| Catálogo de eventos no Core | Event Foundation só contrato |
| Event Bus no Core | PS dedicado |
| Bus valida regra clínica | Domínio valida antes de publicar |
| Clinical Event = mensagem Kafka | Separar camadas A e C |
| Webhook substitui bus | PS distintos |
| Communication na bus para SMS | Domínio orquestra Communication |

---

## 10. Open Questions desta sessão

| ID | Pergunta |
|---|---|
| Q-003 | Event Model + papel do Bus — **objetivo da sessão** |
| OQ-EV01 | Catálogo — domínio vs registry centralizado *(sem inventar implementação)* |
| Q-009 | Mecanismos de extensão — eventos confirmados como mecanismo ADR-0003 |

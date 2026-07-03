# Integration Boundary — Event Strategy

> Rascunho AS-010 — NR-004 (ADR-0004). Investigação concluída — candidato a confirmação.

**Pré-requisitos:** ADR-0004 · ADR-0005 · `event-strategy-boundary.md`  
**Última atualização:** 2026-07-03

---

## 1. Problema (Q-003 / D-006)

Onde termina a **mensageria interna** (Event Bus) e começam **Integration Service** e **Webhook Service**?

---

## 2. Princípio ADR-0004 aplicado a eventos

| Fronteira | Pessoas | Sistemas |
|---|---|---|
| **Capability** | Comunicar | Integrar |
| **PS principal** | Communication, Notification | Integration, Webhook |
| **Event Bus** | **Não** — não é Comunicar nem Integrar | **Não** — é infraestrutura interna de desacoplamento |

Event Bus = **terceira categoria** — mecanismo transversal interno (ADR-0005, ADR-0009), não substitui Comunicar nem Integrar.

---

## 3. Catálogo de mecanismos assíncronos

| Mecanismo | Escopo | Direção | Exemplo |
|---|---|---|---|
| **Event Bus** | Processos **internos** da plataforma | Pub/sub entre domínios, módulos, RM, Audit | `Order.Prescribed` → Timeline |
| **Integration Service** | **Sistemas externos** — protocolos, conectores | Bidirecional conforme conector | Envio FHIR, TISS, PACS |
| **Webhook Service** | **Callbacks HTTP** para parceiros | Saída HTTP configurável | Gateway pagamento notifica plataforma |
| **Communication Service** | **Pessoas** — canais | Saída para paciente/profissional | SMS, e-mail, WhatsApp |

---

## 4. Fronteira Integration vs Webhook (ADR-0005)

| Situação | PS correto |
|---|---|
| Receber webhook de gateway de pagamento | **Webhook Service** |
| Enviar dados clínicos via FHIR para sistema externo | **Integration Service** |
| Registrar endpoint para notificação de evento a parceiro | **Webhook Service** |
| Conector TISS com operadora | **Integration Service** |

Integration = **conectores e protocolos** · Webhook = **callbacks HTTP** como padrão.

---

## 5. Padrão bridge — evento interno → mundo externo

Eventos na bus **não atravessam** a fronteira externa diretamente.

```text
Domínio publica Domain Event (B)
        ↓
Event Bus (interno)
        ↓
┌───────────────────┬───────────────────────┐
│ Assinante interno │ Bridge externo          │
│ (Timeline, Audit) │                         │
└───────────────────┴───────────────────────┘
                            ↓
              Integration Admin / handler dedicado
                            ↓
              Integration PS ou Webhook PS
                            ↓
                    Sistema externo
```

**D-006 candidata:** Integration e Webhook **consomem** ou são **invocados por** handlers internos — **não** substituem Event Bus.

---

## 6. FHIR Messaging e Q-011 (fronteira)

| Cenário | Classificação investigativa |
|---|---|
| Mensagem clínica para **paciente** | Communication (Comunicar) |
| Mensagem clínica **estruturada entre sistemas** (FHIR) | Integration (Integrar) |
| Evento interno `ExternalResult.Received` após integração | Domain Event (B) — Integration Domain publica |

Q-011 permanece **Open** — AS-010 esboça que FHIR entre sistemas ≠ Event Bus.

---

## 7. Communication / Notification vs Event Bus

| Pergunta | Resposta investigativa |
|---|---|
| Event Bus envia SMS? | **Não** |
| Quem envia SMS? | Communication Service — após domínio Communication decidir |
| Papel do evento? | Domínio clínico publica fato → handler Communication chama PS |

Alinhado a E-07 em `event-strategy-boundary.md` e ADR-0004.

**Relação com Q-010:** Communication vs Notification permanece **In Analysis** — independente da taxonomia de Event Bus.

---

## 8. Exemplos classificados

| # | Cenário | Mecanismo | Bus? |
|---|---|---|---|
| I-01 | Timeline atualiza após evolução | Event Bus → Read Model | ✓ interno |
| I-02 | Laboratório envia HL7 | Integration Service | ✗ |
| I-03 | Parceiro recebe POST quando laudo pronto | Webhook Service *(após handler)* | ✗ saída |
| I-04 | Paciente recebe SMS de resultado | Communication Service | ✗ |
| I-05 | Notificação in-app "novo laudo" | Notification Service | ✗ |
| I-06 | ERP externo polling API | Integration *(API)* — não bus | ✗ |

---

## 9. Anti-patterns

| Anti-pattern | Correção |
|---|---|
| Publicar na bus esperando entrega HTTP externa | Webhook ou Integration bridge |
| Integration Service como message broker interno | Event Bus |
| Communication Service inscrito em todos os eventos clínicos | Domínio Communication filtra relevância |
| Webhook substituir Integration para FHIR | Integration — protocolo clínico |

---

## 10. Decisão candidata D-006

**Proposta:** Event Bus = **somente interno**. Integration e Webhook = **Integrar**, acionados por handlers ou domínio Integration — **fronteiras preservadas** (ADR-0004).

**Status:** Ready for Confirmation

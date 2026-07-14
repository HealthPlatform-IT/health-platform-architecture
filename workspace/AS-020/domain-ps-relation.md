# NR-003 — Domínio × PS × Template

> Rascunho AS-020. Candidato a confirmação.

## Relação

| Camada | Papel |
|---|---|
| **Communication Domain** | Políticas, preferências, Communication Consent, destinatários autorizados, histórico de negócio |
| **Communication Service** | Entrega externa (provedores, filas, retries) |
| **Notification Service** | Entrega in-app (inbox, presença, realtime) |
| **Template Service** | Templates reutilizáveis — **independente** (ADR-0011); consumido por Communication e Document Engine |
| **Event Bus** | Fan-out técnico após Domain Event |

## O que cada PS não faz

| PS | Não faz |
|---|---|
| Communication | Inbox in-app; integração sistema↔sistema |
| Notification | E-mail/SMS/WhatsApp/push device; AuthZ |
| Template | Envio; decisão de canal |

## MVP

Communication = **Should** (ADR-0019). Notification in-app útil ao Staff App — tipicamente junto aos módulos Must; sem exigir provedor externo no Must.

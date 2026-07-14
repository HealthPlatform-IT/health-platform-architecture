# NR-001 — Critério de fronteira

> Rascunho AS-020. Candidato a confirmação.

## Critério definitivo (Q-010)

| Serviço | Critério |
|---|---|
| **Notification Service** | Entrega **dentro da plataforma** para **usuário autenticado** (inbox, alertas in-app, real-time na sessão) |
| **Communication Service** | Entrega a **pessoas** por **canais externos** (e-mail, SMS, WhatsApp, push device, etc.) |

**Não** usar: “é notificação?” (palavra ambígua) · tipo de conteúdo clínico · módulo de origem.

## Distinção Comunicar × Integrar (preservada)

| Comunicar (pessoas) | Integrar (sistemas) |
|---|---|
| Communication + Notification | Integration + Webhook |

## Teste rápido

1. Destinatário é sistema? → Integrar (fora destes dois PS).
2. Destinatário é pessoa **na UI autenticada**? → Notification.
3. Destinatário é pessoa **fora da sessão** (canal externo)? → Communication.
4. Ambos 2 e 3? → fan-out: Notification **e** Communication.

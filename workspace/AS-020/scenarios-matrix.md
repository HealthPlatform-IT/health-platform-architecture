# NR-002 — Matriz de cenários

> Rascunho AS-020. Candidato a confirmação.

| # | Cenário | Serviço |
|---|---|---|
| 1 | Bell/inbox: “consulta em 15 min” para profissional logado | **Notification** |
| 2 | E-mail de confirmação de consulta ao paciente | **Communication** |
| 3 | WhatsApp de lembrete | **Communication** |
| 4 | SMS OTP / código | **Communication** |
| 5 | Push no app do paciente (device) | **Communication** |
| 6 | Toast “ordem assinada” no Clinical Workspace | **Notification** |
| 7 | E-mail + inbox in-app para o mesmo evento | **Ambos** (fan-out) |
| 8 | Webhook para EHR externo | **Webhook / Integration** (não estes PS) |
| 9 | FHIR Message para sistema | **Q-011 / Integration** — fora |
| 10 | Preferência “não receber e-mail” | Communication Domain + Communication PS |

## Push: por que Communication?

Push sai da sessão web logada e usa provedor externo / device OS — canal **externo**, mesmo sendo “push notification” no jargão de produto.

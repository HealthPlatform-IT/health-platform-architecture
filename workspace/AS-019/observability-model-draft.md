# NR-003 — Modelo Observability

> Rascunho AS-019. Candidato a confirmação.

## 1. Pilares (Observability Service)

| Pilar | Uso |
|---|---|
| **Logs técnicos** | Diagnóstico operacional |
| **Metrics** | SLI/SLO técnicos (latência, erro, saturação) |
| **Traces** | Request/job através de camadas |
| **Alerts** | Saúde de infra/serviço — não alerta clínico |

## 2. Contexto na telemetria

Quando existir Runtime Context / mensagem:

| Campo | Regra |
|---|---|
| **Correlation-Id / Request-Id** | Propagar (ADR-0016) |
| **Tenant** | Incluir como dimensão (não PHI) |
| **User id** | Opcional; evitar PII desnecessário |

**Proibido:** dump de payload clínico / PHI em logs de Observability.

## 3. Fronteiras

| Sistema | Responsabilidade |
|---|---|
| **Observability** | Saúde técnica da plataforma |
| **Audit** | Quem fez o quê em dados/ações de negócio |
| **Care Monitoring / Ops Dashboard** | Indicadores de cuidado / operação clínica |

## 4. Event Bus / workers

Consumers e jobs usam o mesmo padrão de correlation/tenant. Falhas de broker/outbox = alertas técnicos (não Audit clínico).

## 5. Produto APM / stack

Critérios: retenção configurável · multi-tenant safe · PII controls · integra com traces/logs. **Produto Deferred.**

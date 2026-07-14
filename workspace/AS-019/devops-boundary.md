# NR-001 — Fronteira DevOps / Observability

> Rascunho AS-019. **Não é decisão** até confirmação.

## 1. Pertence

| Tema | Papel |
|---|---|
| Ambientes e promoção | Isolamento de risco |
| CI/CD princípios | Qualidade e gate de release |
| Observability Service (operacional) | Logs, metrics, traces, alerts técnicos |
| Secrets runtime | Critérios alinhados ADR-0020 S-01…S-04 |
| Deployables | Alinhar ADR-0014 |
| Critérios de ferramenta | Sem lock-in prematuro |

## 2. Não pertence

| Tema | Onde |
|---|---|
| Audit de negócio / PHI access | Audit Service |
| Indicadores clínicos / Care Monitoring | Domínio Monitorar / M-07 |
| Regra clínica / módulo | Domínios / Modules |
| Escolha de cloud/K8s/APM produto | Deferred (critérios apenas) |
| Q-019 Compliance Service | Continua Open |

## 3. Proibições

- Observability no Core
- Usar logs técnicos como substituto de Audit
- Expor secrets em telemetria ou pipelines públicos
- Module ID = microsserviço automático (ADR-0014)

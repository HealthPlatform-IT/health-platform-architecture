# Notas — AS-019

Pós AS-018. Fecha o último bloco do Sprint 3 no INDEX (DevOps / Observability).

**Já decidido:**
- Observability = PS Confirmed; ≠ Audit; ≠ Care Monitoring (ADR-0005 / 0020)
- Module ≠ deploy; deployables: platform-api + workers opcionais (ADR-0014)
- Outbox → broker; produto broker Deferred (ADR-0017)
- Secrets S-01…S-04; vault/KMS Deferred para esta sessão (ADR-0020)
- Deploy/infra fora do Core (ADR-0009)

**Escopo:** princípios operacionais — ambientes, pipeline, o que Observability cobre, critérios de ferramenta — **não** Terraform, K8s ou vendor APM.

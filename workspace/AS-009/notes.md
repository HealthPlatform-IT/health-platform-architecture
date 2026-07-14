# Notas — AS-009

Cartão [QSOD1Ikx](https://trello.com/c/QSOD1Ikx). Pós AS-017 (MVP).

**Já decidido (não reabrir):**
- Identity / Authorization / Audit = PS Confirmed (ADR-0005) — **não** Core
- Tenant no Runtime Context; operação sem tenant inválida (ADR-0013)
- Cross-tenant = exceção + AuthZ + Audit
- Pipeline backend: Identity → Runtime Context → AuthZ (ADR-0014)
- Paths públicos governados (ADR-0016)
- AuthZ fina Deferred até esta sessão (ADRs 0013–0016)
- Compliance Service = Q-019 Needs Review — **fora** do fechamento desta sessão
- MVP Must inclui Identity/AuthZ/Audit (ADR-0019)

**Pergunta da sessão:** como AuthN/AuthZ/Audit se encadeiam tecnicamente e o que I-10 exige — sem código, IdP produto ou matriz completa de papéis.

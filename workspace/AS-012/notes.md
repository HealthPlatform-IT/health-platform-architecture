# Notas — AS-012

## 2026-07-14 — Kickoff

- Pós AS-011 (ADR-0013). Cartão Trello [YyAkiKRW](https://trello.com/c/YyAkiKRW).
- Sessão **AS-012 — Backend Architecture**.
- Ordem do board: Backend → Database → API → Event Bus tech → …
- Sem código; sem ADR até pacote confirmado.

## 2026-07-14 — Confirmação e formalização

- Usuário: *"Confirmado o pacote"*
- ADR-0014 Accepted; `backend-architecture.md`
- Cartão Trello → **08 — Concluído**
- Próximo: Database Architecture · API Strategy · AS-009 Security

### Premissas herdadas a não reabrir

- Module ≠ microsserviço (ADR-0009 / module-strategy)
- Event Bus só interna; tipos no domínio (ADR-0012)
- Tenant = SaaS; shared+discriminator (ADR-0013)
- Core = contratos (ADR-0009)

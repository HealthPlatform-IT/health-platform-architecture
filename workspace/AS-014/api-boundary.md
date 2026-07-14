# API Strategy — Boundary

> Rascunho AS-014 — NR-001. **Não é decisão** até confirmação.

---

## 1. Definição candidata

**API Strategy** = decisões sobre **contratos síncronos** entre clientes, módulos, Platform Services e integrações — estilo, fronteira sync vs eventos, versionamento conceitual e Tenant Context — **sem** especificação OpenAPI completa nesta sessão.

---

## 2. Inclui / não inclui

| Inclui | Não inclui |
|---|---|
| Estilo de API (conceitual) | Controllers / código |
| Sync vs Domain Event | Broker Event Bus (Q-003 tech) |
| Superfícies Product / Platform / Integration | Schemas DDL |
| Versionamento conceitual | OpenAPI file final |
| Tenant + correlation em requests | AuthZ policies (AS-009) |
| Contrato mínimo de erro | Desenho BFF detalhado |

---

## 3. Herança

- Sync ≠ evento (ADR-0012)
- Modular monolith; API = camada Presentation (ADR-0014)
- Runtime Context / tenant (ADR-0013)
- Comunicar ≠ Integrar (ADR-0004) — Integration API ≠ Communication

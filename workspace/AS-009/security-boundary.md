# NR-001 — Fronteira Security

> Rascunho AS-009. **Não é decisão** até confirmação.

## 1. Camadas

| Conceito | Camada | Papel |
|---|---|---|
| Tenant Context / Runtime Context contratos | **Core** | Invariantes I-01, I-02, I-10 |
| Identity Service | **PS** | AuthN, sessão, user+tenant na sessão |
| Authorization Service | **PS** | Decisão allow/deny por papel/permissão/escopo |
| Audit Service | **PS** | Rastreabilidade de acesso e ação |
| Observability | **PS** | Logs/metrics técnicos — ≠ Audit de negócio |
| Governance & Compliance Domain | **Domínio** | Políticas institucionais (LGPD etc.) — enforcement técnico nos PS |
| Compliance Service | **PS Needs Review** | Q-019 — **fora** desta sessão |

## 2. Proibições

- Identity / AuthZ / Audit **no Core** (já rejeitado ADR-0009)
- AuthZ embutida só em módulo clínico (duplicação) — módulos **consomem** Authorization
- Usar Event Bus para fechar AuthZ sync no request
- Tratar Observability como Audit clínico

## 3. Relação com domínios

Módulos e Application Services pedem autorização; **não** reinventam papéis. Políticas de negócio (quem “pode” clinicamente em sentido institucional) podem originar-se em Governance & Compliance; **enforcement** = Authorization Service.

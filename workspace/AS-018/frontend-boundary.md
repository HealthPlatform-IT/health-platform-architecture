# NR-001 — Fronteira Frontend

> Rascunho AS-018. **Não é decisão** até confirmação.

## 1. Pertence ao Frontend Architecture

| Tema | Papel |
|---|---|
| Superfícies de aplicativo | Staff / Patient / Admin |
| Shell Clinical Workspace | Composição de UI de módulos |
| Contrato de composição | Contribuições (rotas/slots) registradas |
| Consumo de Product API (± BFF) | Sync UI |
| Auth UX | Login/sessão no cliente — consome Identity |
| Limites de estado no cliente | Sem domínio compartilhado ilegal entre módulos |

## 2. Não pertence

| Tema | Onde |
|---|---|
| Autorização real | Authorization PS (ADR-0020) |
| Contratos de domínio / PS | Backend / Platform Services |
| Design system / tokens visuais | Fase de produto / design |
| Framework (React, Vue…) | Deferred produto |
| Telemetry produto detalhado | Observability / DevOps |
| Microfrontends obrigatórios | Não padrão MVP |

## 3. Regras

- Platform Services **não** possuem UI.
- Módulos entregam UI (ou orquestram via shell).
- Shell **não** contém regra clínica.

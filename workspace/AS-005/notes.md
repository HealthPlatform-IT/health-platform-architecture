# Notas — AS-005

> Log de investigação. Notas não são decisões.

---

## 2026-07-03 — Início da sessão

Sessão iniciada após confirmação da preparação AS-005.

**Inputs consolidados:**
- 16 Business Domains (ADR-0008)
- 17 Platform Services catalogados (ADR-0005)
- Q-007 Partial — critérios Domain/PS/View da AS-004 (D-002)
- ADR-0006 — hierarquia configuração → composição → extensão → customização

**Primeira ação:** NR-001 — reconciliar tensão T-001 (ADR-0003 vs ADR-0005).

---

## Tensões registradas

| ID | Tensão | Status |
|---|---|---|
| T-001 | Core lista identidade/autorização/auditoria; ADR-0005 = PS | ✅ Resolvida — D-002 (Core=contratos, PS=implementação) |
| T-002 | Extension Domain vs Extension Mechanism | ✅ Resolvida — H-EXT-001 em extension-model-draft |
| T-003 | Clinical Workspace — classificação | ✅ Resolvido provisório — M-02 shell |
| T-004 | Communication Domain + Communication Service | ⚪ Pendente |
| T-005 | Compliance Service (Q-019) | ⚪ Pendente |

---

## Riscos da sessão (R-01 a R-12)

| # | Risco | Mitigação |
|---|---|---|
| R-01 | Core grande demais | Critério "mínimo necessário" em `core-boundary.md` |
| R-02 | PS virando domínios | Reaplicar D-002; política no domínio, execução no PS |
| R-03 | Domínios 1:1 módulos | Aceitar 1:N e N:1 |
| R-04 | Módulos acoplados | Regra: PS, eventos ou contratos de domínio |
| R-05 | Extensão = customização | Hierarquia ADR-0006 |
| R-06 | Read Model com regra de negócio | Timeline = projeção |
| R-07 | Regras operacionais no Core | Extension Domains + módulos opcionais |
| R-08 | Colisão terminológica Extension | Glossário na sessão |
| R-09 | Clinical Workspace mal classificado | M-02 shell — T-003 resolvido provisoriamente |
| R-10 | Reabrir AS-004 | Não reagrupar domínios |
| R-11 | Módulos órfãos | Rastreio domínio + capability obrigatório |
| R-12 | Prematuridade técnica | Gate: conceito sim, tecnologia não |

---

## Referências cruzadas úteis

- AS-004 `decisions.md` D-002 — Domain vs PS vs Read Model
- AS-004 distinção Module = "solução implementável futura — derivada de domínio + capability"
- ADR-0003 seção "O que pertence ao Core" — lista infraestrutura que ADR-0005 externaliza
- ADR-0006 seção 1 — hierarquia de variabilidade

---

## Confirmação final (2026-07-03)

**Confirmado pelo usuário** (*"Confirmo o pacote AS-005"*):

- D-005 — Modelo de extensão
- D-006 — Read Models boundary
- D-008 — Classification matrix
- **Q-007 → Answered**
- Sessão AS-005 encerrada
- Formalização: ADR-0009 + docs oficiais (core-platform, module-strategy, extension-model, read-models, architecture-classification, platform-services v0.2.0, glossary, development-guidelines, CLAUDE.md, Cursor rules)

---

## Fechamento documental (2026-07-03)

**Onda de consolidação executada** após confirmação do pacote:

- ADR-0009 Accepted
- Docs oficiais AS-005 promovidos do workspace
- Rascunhos workspace marcados como histórico (banners)
- Q-007 → Answered em ADR-0003, ADR-0005, ADR-0008, open-questions
- Correção 14→12 Confirmed em `platform-services-boundary.md`
- Próximo marco: **AS-006 — Medical Form Engine**

## Confirmação parcial (2026-07-03)

**Confirmado pelo usuário** (*"Pacote revisado"*):

- D-001 — Core Platform (8 itens)
- D-002 — Reconciliação Core ↔ PS
- D-003 — Catálogo PS com tiers
- D-004 — Relação domínio → módulo (15 módulos)
- D-007 — Clinical Workspace shell (M-02)

`module-strategy-draft.md` → validado.

**Próximo:** NR-008 — confirmação final (`confirmation-package.md`)

---

## Log de investigação

_(Adicionar entradas por NR conforme avanço)_

### NR-001 — Core vs PS (início)

ADR-0003 (Accepted) descreve Core abrigando "infraestrutura essencial transversal: identidade, autorização, auditoria, multi-tenant, configuração base".

ADR-0005 (Accepted) cataloga Identity, Authorization, Audit, Configuration como **Platform Services** distintos do Core.

**Interpretação preliminar (hipótese H-CORE-002):** ADR-0003 usa "Core" no sentido amplo de "fundação da plataforma"; ADR-0005 refinou a decomposição colocando implementações em PS. A AS-005 deve introduzir **Core Platform** como termo operacional para a camada mínima estrutural, separando-a dos PS.

*Aguarda validação — não é decisão.*

### NR-007 — Classification Matrix (2026-07-03)

`classification-matrix.md` v0.2.0 — árvore 6 categorias + I-01 a I-10 + anti-patterns.

- D-008 → Ready for Confirmation

### NR-006 — Read Models (2026-07-03)

`read-models-boundary.md` v0.2.0 — Timeline + Analytics consolidados.

- Timeline → M-02/M-08; Analytics → M-12
- D-006 → Ready for Confirmation

### NR-005 — Extension Model (2026-07-03)

`extension-model-draft.md` v0.2.0 — H-EXT-001 formalizado.

- Extension Domains: Diagnostic, Home Care → M-14, M-15
- Telemedicine = Operational Mode (não Extension Module)
- T-002 resolvida
- D-005 → Ready for Confirmation

### NR-004 — Module Strategy (2026-07-03)

`module-strategy-draft.md` v0.2.0 — 15 módulos candidatos + Telemedicine como capability em Attendance.

- Clinical Workspace = shell M-02 (T-003)
- Communication Domain sem módulo dedicado
- D-004, D-007 → **Confirmed** (pacote revisado 2026-07-03)

### NR-002 — Core Platform (2026-07-03)

`core-boundary.md` v0.2.1 — 14 candidatos analisados. **Strong Candidates validados pelo usuário.**

### NR-003 — Core ↔ PS (2026-07-03)

`platform-services-boundary.md` v0.2.0 — T-001 resolvida conceitualmente:

- Core Platform = contratos + invariantes (8 itens D-001)
- PS = implementações transversais ADR-0005
- Invariantes I-01 a I-10 definidos
- D-001 e D-002 → Ready for Confirmation em `decisions.md`

**Próximo:** NR-005 — `extension-model-draft.md`

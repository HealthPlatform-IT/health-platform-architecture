# Pacote de Confirmação Final — AS-005

> **Sessão:** AS-005 — Core Platform Boundary  
> **Data do pacote:** 2026-07-03  
> **Status:** **Confirmado** em 2026-07-03 pelo usuário.  
> **Formalização:** Executada

---

## Confirmação registrada

**Confirmado em 2026-07-03** pelo usuário (*"Confirmo o pacote AS-005"*).

Formalização executada:

1. ✅ `decisions.md` — D-001 a D-008 Confirmados
2. ✅ **ADR-0009** — Core Platform Boundary (Accepted)
3. ✅ Docs oficiais: `core-platform.md`, `module-strategy.md`, `extension-model.md`, `read-models.md`, `architecture-classification.md`
4. ✅ `platform-services.md` v0.2.0 — tiers D-003
5. ✅ `ai-context/` — architecture-foundation, open-questions, development-guidelines, adr-summary
6. ✅ `glossary.md`, `CLAUDE.md`, `.cursor/rules/health-platform.mdc`
7. ✅ Sessão AS-005 encerrada

**Fora do escopo desta confirmação:** código, APIs, Event Model (Q-003), agregados DDD (Q-004), implementação de extensão (Q-009).

---

## Confirmação pendente

~~Este pacote consolida **D-001 a D-008** para confirmação final e encerramento da AS-005.~~

**Confirmado integralmente em 2026-07-03.**

---

## Resumo executivo

| Item | Decisão |
|---|---|
| **Core Platform** | 8 contratos/invariantes estruturais — sem implementação operacional |
| **Platform Services** | 12 Confirmed + 5 Strong + 1 Needs Review (Compliance) |
| **Reconciliação Core ↔ PS** | Core = contratos; PS = implementações (T-001 resolvida) |
| **Business Domains** | 16 confirmados AS-004 — inventário fechado |
| **Modules** | 15 candidatos — cardinalidade flexível |
| **Extension** | M-14 Diagnostic, M-15 Home Care; Telemedicine = Operational Mode |
| **Read Models** | 2 — Clinical Timeline, Analytics & Reporting |
| **Clinical Workspace** | M-02 shell transversal (T-003 resolvida) |
| **Classification** | Árvore de 6 categorias + invariantes I-01 a I-10 |

---

## Decisões estruturais

### D-001 — Definição de Core Platform

Core Platform = fundação estrutural mínima — contratos, invariantes, mecanismos de extensão.

**Catálogo (8):** Hierarchical Care Model · Care Journey Types · Tenant Context · Module Registry · Extension Mechanism · Event Foundation · Runtime Context · Shared Kernel Types

**Status:** ✅ Confirmado 2026-07-03

---

### D-002 — Reconciliação Core ↔ Platform Services

ADR-0003 (sentido amplo) e ADR-0005 **não se contradizem**.

```text
Core define invariantes → PS operam dentro dos invariantes → Domínios definem política → PS executam mecanismo
```

**Status:** ✅ Confirmado 2026-07-03

---

### D-003 — Catálogo Platform Services

| Tier | Qtd | Serviços |
|---|---|---|
| Confirmed | 12 | Identity, Authorization, Audit, Configuration, Feature Flag, Communication, Notification, Integration, Webhook, Storage, File, Observability |
| Strong | 5 | Search, Document Engine, Medical Form Engine, Template, Event Bus |
| Needs Review | 1 | Compliance Service |

**Status:** ✅ Confirmado 2026-07-03

---

### D-004 — Relação domínio → módulo

- Cardinalidade flexível (1:1, 1:N, N:1, 1:0, Extension)
- **15 módulos** derivados de 16 domínios
- Communication Domain sem módulo dedicado

**Status:** ✅ Confirmado 2026-07-03

---

### D-005 — Modelo de extensão

| Conceito | Papel |
|---|---|
| Extension Business Domain | Diagnostic Operations, Home Care Operations |
| Extension Module | M-14, M-15 |
| Extension Mechanism | Module Registry + hooks (Core) |

Telemedicine = Operational Mode em Attendance — **não** Extension Module.

**Status:** ✅ Confirmado 2026-07-03

---

### D-006 — Read Models boundary

| Read Model | Consumidores | Papel |
|---|---|---|
| Clinical Timeline | M-02, M-08 (parcial) | Projeção cronológica — prontuário = visão |
| Analytics & Reporting | M-12 | Agregação de indicadores |

Ownership nos domínios fonte. Timeline **não** é módulo.

**Status:** ✅ Confirmado 2026-07-03

---

### D-007 — Clinical Workspace

M-02 = módulo shell transversal que compõe M-03 a M-07 sem regras de negócio próprias.

**Status:** ✅ Confirmado 2026-07-03

---

### D-008 — Classification matrix unificada

Árvore de decisão em 6 categorias + invariantes I-01 a I-10 + testes de validação.

**Status:** ✅ Confirmado 2026-07-03

---

## Catálogo de módulos (15)

| Tier | Módulos |
|---|---|
| Core product | M-01 Scheduling, M-02 Clinical Workspace, M-03 Attendance, M-04 Documentation, M-05 Orders, M-06 Documents, M-07 Care Monitoring, M-08 Patient Portal |
| Supporting | M-09 Professional & Org Admin, M-10 Payer, M-11 Integration Admin, M-12 Operations Dashboard, M-16 Governance Admin |
| Extension | M-14 Diagnostic, M-15 Home Care |

---

## Tensões resolvidas

| ID | Tensão | Resolução |
|---|---|---|
| T-001 | ADR-0003 vs ADR-0005 | Core = contratos; PS = implementação |
| T-002 | Extension Domain vs Mechanism | H-EXT-001 — três conceitos distintos |
| T-003 | Clinical Workspace | M-02 shell transversal |

---

## Needs Review (não bloqueiam confirmação)

| Item | Motivo |
|---|---|
| Organization Context (OQ-C01) | Runtime Context vs Organization Management |
| UI composition contract (OQ-C03) | Slot mínimo no Core |
| Compliance Service (Q-019) | PS Needs Review |
| Communication na Timeline (OQ-V01) | Critério de inclusão |

---

## Deferred

| Item | Motivo |
|---|---|
| Event Model (Q-003) | AS-010 |
| DDD Aggregates (Q-004) | Sessão futura |
| MVP por módulo (Q-006) | Produto |
| Extension implementation (Q-009) | Fase técnica |
| Domínio Analytics autônomo | P-MON-01 |
| Billing & Financial | Q-005 |

---

## Alternativas rejeitadas

| Alternativa | Motivo |
|---|---|
| Core monolítico com regras clínicas | ADR-0003 |
| Identity/Audit/Config no Core | ADR-0005 |
| 16 módulos = 16 domínios | R-03 |
| Timeline como domínio ou módulo | AS-004 D-003 + D-006 |
| Telemedicine como Extension Module | Duplica Care Delivery |
| Communication Module dedicado | Orquestrado por consumidores |
| Clinical Workspace no Core | Risco Core grande |

---

## Open Questions — impacto pós-confirmação

| ID | Status esperado |
|---|---|
| **Q-007** | **Answered** — fronteiras Core/PS/Module/Extension/View definidas |
| Q-009 | Parcial — Extension Mechanism no Core confirmado |
| Q-019 | Needs Review — Compliance PS |
| Q-003 | Open — Event Model |

---

## Formalização — ✅ Executada (2026-07-03)

Ver seção "Formalização executada" no topo do documento e `decisions.md` §4.

---

## Checklist de confirmação

- [x] D-005 — Modelo de extensão
- [x] D-006 — Read Models boundary
- [x] D-008 — Classification matrix
- [x] Encerramento AS-005
- [x] Q-007 → Answered

---

## Artefatos de investigação

| NR | Artefato | Versão |
|---|---|---|
| NR-001/002 | `core-boundary.md` | v0.2.1 |
| NR-003 | `platform-services-boundary.md` | v0.3.0 |
| NR-004 | `module-strategy-draft.md` | v0.2.0 |
| NR-005 | `extension-model-draft.md` | v0.2.0 |
| NR-006 | `read-models-boundary.md` | v0.2.0 |
| NR-007 | `classification-matrix.md` | v0.2.0 |

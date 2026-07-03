# Read Models Boundary

> **Promovido para documentação oficial:** `docs/05-architecture/read-models.md` (ADR-0009).  
> Este arquivo permanece como registro histórico da investigação NR-006.

**Investigação:** NR-006 — Clinical Timeline e Analytics & Reporting  
**Pré-requisitos:** AS-004 D-003/D-004 · `module-strategy-draft.md` v0.2.0 (validado)  
**Status:** Rascunho em revisão  
**Última atualização:** 2026-07-03

---

## 1. Purpose

Consolidar como **Read Models / Views** se relacionam com:

- Business Domains (ownership dos dados);
- Modules (consumidores da projeção);
- Platform Services (infraestrutura de suporte);
- Core Platform (Event Foundation para atualização).

Reafirma AS-004 D-003 e D-004 **sem** promover views a domínios nem a módulos de negócio.

Este documento **não** define Event Model detalhado (Q-003), pipelines de dados ou implementação de CQRS.

---

## 2. Definition of Read Model / View

Na Health Platform, um **Read Model** (ou **View**) é uma **projeção de leitura cross-domain** que:

- **agrega e apresenta** dados de múltiplos domínios fonte;
- **não possui** vocabulário ubíquo de negócio autônomo;
- **não é** fonte primária de registro — ownership permanece nos domínios;
- **não decide** regras clínicas ou operacionais — apenas projeta o que os domínios produziram.

**Prontuário** = visão institucional da **Clinical Timeline** — não é domínio (AS-004 D-003).

```text
Domínios fonte  →  produzem registros e eventos
Read Model      →  projeta leitura unificada
Módulos         →  consomem a projeção na UI
PS              →  suportam (busca, autorização, auditoria)
```

---

## 3. What Read Model Is Not

| Conceito | Diferença |
|---|---|
| **Business Domain** | Domínio define regras; Read Model apenas lê |
| **Module** | Módulo implementa funcionalidade; Read Model é projeção consumida |
| **Platform Service** | PS é mecanismo transversal; Read Model é visão de negócio |
| **Clinical Record** | Record **produz** conhecimento clínico; Timeline **projeta** |
| **Clinical Workspace** | Shell orquestra módulos; Timeline é componente de visualização |
| **Search Service** | PS indexa; Timeline é semântica de apresentação clínica |

---

## 4. Confirmed Read Models (AS-004)

| Read Model | Origem (sub-capability) | Capability | Papel |
|---|---|---|---|
| **Clinical Timeline** | Clinical Timeline | Registrar | Projeção cronológica cross-domain — prontuário = visão |
| **Analytics & Reporting** | Indicators & Dashboards | Monitorar | Agregação de indicadores cross-domain |

**Total:** 2 read models confirmados — **não** são Business Domains (ADR-0008).

**Futuro:** domínio Analytics autônomo somente se surgir produto analítico com regras próprias (P-MON-01 — Deferred).

---

## 5. Principles (H-VIEW-001)

| # | Princípio | Implicação |
|---|---|---|
| V-01 | **Ownership** nos domínios fonte | Escrita sempre no domínio; Timeline nunca é fonte primária |
| V-02 | Read Model **agrega e projeta** | Ordenação cronológica, filtros, layout — permitido |
| V-03 | Read Model **não decide** regra de negócio | Ex.: qual versão é registro oficial → Clinical Record |
| V-04 | **Apresentação ≠ regra** | Filtros de UI não alteram política clínica |
| V-05 | Atualização via **Event Foundation** | Domínios publicam; projeção consome — Event Model fora de escopo (Q-003) |
| V-06 | **Authorization** antes da projeção | O que o usuário vê → Authorization PS + políticas de domínio |
| V-07 | Read Model **não é módulo** | Consumido por módulos — não registrado no Module Registry como módulo de negócio |

---

## 6. Ownership Model

```text
                    ┌─────────────────────┐
                    │  Clinical Timeline   │  ← Read Model (projeção)
                    └──────────┬──────────┘
                               │ lê de
        ┌──────────────────────┼──────────────────────┐
        ▼                      ▼                      ▼
 Care Delivery          Clinical Record         Clinical Orders
 Clinical Documents      Care Monitoring         Communication (*)
        │                      │                      │
        └──────────────────────┴──────────────────────┘
                               │
                    ownership permanece aqui
                    (escrita nos domínios)
```

**Regra de ouro:** se a operação **cria ou altera** dado clínico/operacional → **domínio fonte**. Se apenas **lê e apresenta** → Read Model.

---

## 7. Clinical Timeline

### 7.1 Role

Projeção cronológica institucional de eventos e artefatos clínicos — a visão unificada do prontuário para profissionais e pacientes (parcial).

### 7.2 Data Sources (domains)

| Domínio fonte | Contribuição na timeline | Tipo de entrada |
|---|---|---|
| **Care Delivery** | Attendances, sessões, interações | Evento / registro de execução |
| **Clinical Record** | Evoluções, anamneses, diagnósticos | Registro de conhecimento |
| **Clinical Orders** | Ordens, prescrições, status | Intenção clínica |
| **Clinical Documents** | Laudos, atestados, termos formalizados | Artefato assinável |
| **Care Monitoring** | Pendências, alertas clínicos | Continuidade / risco |
| **Communication** | Comunicações clinicamente relevantes *(filtro)* | Mensagem com contexto clínico |
| **Diagnostic Operations** *(se habilitado)* | Resultados diagnósticos | Extension Domain |
| **Integration** | Dados interoperados *(metadados)* | Integração externa |

**Communication na timeline:** apenas comunicações com **relevância clínica contextual** — não todo e-mail. Política de inclusão → Communication Domain; projeção → Timeline.

### 7.3 Consumers (modules)

| Módulo | Visão | Escopo |
|---|---|---|
| **M-02 Clinical Workspace** | Principal — profissional de saúde | Completa (autorizada) |
| **M-08 Patient Portal** | Parcial — paciente/responsável | Subconjunto autorizado |
| **M-12 Operations Dashboard** | Não — usa Analytics | — |
| **Integrações externas** | Via Integration Domain | Contrato de interoperabilidade |

### 7.4 Platform Services involved

| PS | Papel |
|---|---|
| **Authorization Service** | Filtra o que cada usuário pode ver na timeline |
| **Audit Service** | Registra acesso à timeline |
| **Search Service** | Indexação e busca dentro da timeline |
| **Identity Service** | Contexto do usuário |

**Search Service suporta Timeline** — não a substitui. Timeline = semântica clínica de apresentação; Search = infraestrutura de indexação.

### 7.5 Projection placement (resolved)

| Alternativa | Resultado |
|---|---|
| **A — Timeline Module dedicado** | **Rejeitada** — Read Model não é módulo de negócio |
| **B — Projeção exposta pelo Clinical Workspace** | **Adotada** — M-02 consome e apresenta Timeline |
| **C — Serviço técnico de projeção separado** | **Deferred** — decisão de implementação (Q-003) |

**Clinical Workspace** hospeda a **experiência** da Timeline — a projeção em si permanece Read Model, não regra de negócio do shell.

### 7.6 Boundaries (what Timeline does NOT do)

| Responsabilidade | Onde pertence |
|---|---|
| Registrar evolução clínica | Clinical Record Domain → M-04 |
| Definir registro oficial | Clinical Record / Clinical Documents |
| Enviar comunicação | Communication Domain → Communication PS |
| Decidir alerta clínico | Care Monitoring Domain |
| Indexar full-text | Search Service (PS) |

---

## 8. Analytics & Reporting

### 8.1 Role

Agregação de indicadores clínicos e operacionais cross-domain — dashboards e relatórios institucionais.

### 8.2 Data Sources

| Fonte | Tipo de indicador |
|---|---|
| **Care Monitoring** | Pendências, protocolos, alertas clínicos |
| **Operations Monitoring** | SLAs operacionais, filas, tempos |
| **Care Coordination** | Agendamentos, encaminhamentos |
| **Care Delivery** | Volumes de atendimento |
| **Clinical Record / Orders / Documents** | Indicadores derivados *(agregados)* |
| **Múltiplos domínios** | KPIs institucionais compostos |

### 8.3 Consumers

| Módulo | Papel |
|---|---|
| **M-12 Operations Dashboard** | Principal consumidor — dashboards operacionais |
| **M-16 Governance Admin** | Relatórios de conformidade *(parcial)* |
| **Gestores institucionais** | Visão agregada |

**Care Monitoring (M-07)** ≠ Analytics — M-07 monitora **risco clínico individual**; Analytics agrega **indicadores institucionais**.

### 8.4 Platform Services involved

| PS | Papel |
|---|---|
| **Authorization Service** | Acesso a indicadores por perfil |
| **Audit Service** | Exportação e acesso a relatórios |
| **Search Service** | Consultas analíticas *(opcional)* |
| **Configuration Service** | Parâmetros de dashboards |

### 8.5 Projection placement (resolved)

| Alternativa | Resultado |
|---|---|
| **Analytics Module dedicado** | **Rejeitada** — Read Model, não módulo |
| **M-12 consome projeção Analytics** | **Adotada** — Operations Dashboard apresenta Analytics |
| **Domínio Analytics futuro** | **Deferred** — P-MON-01 |

---

## 9. Read Model vs Module vs Platform Service

| Aspecto | Read Model | Module | Platform Service |
|---|---|---|---|
| **Escrita primária** | Não | Sim (via domínio) | Não (mecanismo) |
| **Regras de negócio** | Nenhuma autônoma | Implementa domínio | Nenhuma |
| **UI** | Consumida por módulo | Possui UI | Sem UI |
| **Registry** | Não registrado como módulo | Module Registry | Declarado em PS catalog |
| **Exemplo** | Clinical Timeline | Clinical Workspace | Search Service |

---

## 10. Read Model vs Domain — Border Tests

| Teste | Clinical Timeline | Clinical Record |
|---|---|---|
| Vocabulário ubíquo próprio? | Não — agrega vocabulários | Sim — evolução, anamnese |
| Pode existir sem outros domínios? | Não — depende de fontes | Sim |
| Cria registro primário? | Não | Sim |
| Especialista nomearia como domínio? | Não — "visão do prontuário" | Sim |

| Teste | Analytics & Reporting | Operations Monitoring |
|---|---|---|
| Define SLAs operacionais? | Não — exibe | Sim — domínio |
| Agrega cross-domain? | Sim | Parcial |
| Regras autônomas? | Não | Sim |

---

## 11. Update Mechanism (conceptual)

```text
Domínio fonte persiste registro
        ↓
Event Foundation (contrato Core) — evento de mudança
        ↓
Projeção Timeline / Analytics atualiza leitura
        ↓
Módulo consumidor re-renderiza
```

**Fora de escopo:** formato de eventos, eventual consistency, tecnologia (Q-003 — AS-010).

**Invariante I-07:** Event Foundation no Core — sem catálogo de eventos; Read Models consomem por contrato.

---

## 12. Risks

| # | Risco | Mitigação |
|---|---|---|
| R-VIEW01 | Timeline vira domínio (R-06) | Ownership nos domínios fonte; V-01 a V-03 |
| R-VIEW02 | Timeline Module criado | Rejeitado — projeção consumida por M-02 |
| R-VIEW03 | Analytics absorve Operations Monitoring | Fronteiras AS-004 D-004 reforçadas |
| R-VIEW04 | Read Model com regra de negócio | Teste de border — seção 10 |
| R-VIEW05 | Search Service = Timeline | PS suporta; Timeline é semântica clínica |
| R-VIEW06 | Patient Portal vê timeline completa | Authorization + visão parcial |

---

## 13. Open Questions

| ID | Pergunta | Status |
|---|---|---|
| OQ-V01 | Communication na timeline — critério de inclusão | Needs Review — política Communication Domain |
| OQ-V02 | Event Model e projeção | Q-003 Deferred — AS-010 |
| OQ-V03 | Domínio Analytics futuro | P-MON-01 Deferred |
| OQ-V04 | Serviço técnico de projeção separado | Implementação — fora de escopo |
| OQ-V05 | Timeline em integrações HL7/FHIR | Integration Domain + contrato |

---

## 14. Initial Recommendations

1. **D-006** → promover para **Ready for Confirmation** em `decisions.md`.
2. Reafirmar AS-004 D-003 e D-004 — sem alteração de classificação.
3. Clinical Timeline → projeção consumida por **M-02** e **M-08** (parcial).
4. Analytics → projeção consumida por **M-12**.
5. **NR-007** — incluir Read Model na classification matrix.
6. **Q-007** — Read Models boundary fecha terceiro pilar da fronteira.

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1.0 | 2026-07-03 | Esqueleto NR-006 |
| 0.2.0 | 2026-07-03 | Investigação completa — ownership, consumidores, fronteiras |

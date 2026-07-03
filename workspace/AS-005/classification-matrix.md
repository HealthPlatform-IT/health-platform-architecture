# Classification Matrix — AS-005

> **Promovido para documentação oficial:** `docs/05-architecture/architecture-classification.md` (ADR-0009).  
> Este arquivo permanece como registro histórico da investigação NR-007.

**Investigação:** NR-007 — matriz consolidada  
**Pré-requisitos:** `core-boundary.md` · `platform-services-boundary.md` · `module-strategy-draft.md` · `extension-model-draft.md` · `read-models-boundary.md`  
**Status:** Rascunho em revisão  
**Última atualização:** 2026-07-03

---

## 1. Purpose

Fornecer critérios **repetíveis e testáveis** para classificar qualquer responsabilidade em uma das **seis categorias arquiteturais** da Health Platform:

1. Core Platform  
2. Platform Service  
3. Business Domain  
4. Module  
5. Extension  
6. Read Model / View  

Estende AS-004 D-002 (Domain / PS / View) com Core, Module e Extension (D-008).

---

## 2. The Six Categories

| # | Categoria | Pergunta resumida | Exemplo |
|---|---|---|---|
| 1 | **Core Platform** | É fundação estrutural (contrato/invariante) sem a qual o SaaS multi-tenant extensível não existe? | Module Registry, Tenant Context |
| 2 | **Platform Service** | É transversal, reutilizável, sem UI e sem vocabulário de domínio? | Identity, Audit, Communication |
| 3 | **Business Domain** | Possui regras de negócio e vocabulário ubíquo coeso? | Clinical Orders, Care Delivery |
| 4 | **Module** | É implementação funcional derivada de domínio(s), entregue ao usuário? | Scheduling, Attendance |
| 5 | **Extension** | É módulo/domínio opcional por modelo operacional, desabilitável sem quebrar clínica básica? | Diagnostic Module, Home Care Module |
| 6 | **Read Model / View** | É projeção cross-domain de leitura sem regras autônomas? | Clinical Timeline, Analytics |

---

## 3. Decision Tree

```text
START: Nova responsabilidade R

1. R é CONTRATO ou INVARIANTE estrutural necessário para o SaaS
   multi-tenant extensível existir — NÃO implementação operacional?
   SIM → Core Platform
   NÃO → 2

2. R é transversal, reutilizável por ≥2 módulos/domínios,
   sem vocabulário de negócio próprio, sem UI de produto?
   SIM → Platform Service
   NÃO → 3

3. R possui regras de negócio e vocabulário ubíquo que definem
   responsabilidade de negócio coesa?
   SIM → Business Domain
         → se NOVO domínio: REQUER sessão/ADR — inventário fechado (16)
   NÃO → 4

4. R é agregação/projeção de LEITURA cross-domain
   sem regras de negócio autônomas?
   SIM → Read Model / View
   NÃO → 5

5. R é funcionalidade implementável entregue a usuário
   ou shell de orquestração de módulos?
   SIM → 6
   NÃO → Open Question ou reclassificar (pode ser feature dentro de módulo)

6. R é opcional por modelo operacional/tenant e pode ser desabilitada
   sem afetar operação ambulatorial básica?
   SIM → Extension (Module implementando Extension Domain)
   NÃO → Module (core product ou supporting)
```

### Ordem importa

A árvore é **sequencial** — não pular etapas. Ex.: Authentication parece "core" mas a **implementação** é PS; o **contrato de sessão** pode estar no Core via Runtime Context.

---

## 4. Category Reference Matrix

### 4.1 Core Platform (D-001 — 8 itens)

| Pertence | Não pertence |
|---|---|
| Hierarchical Care Model Contracts | Implementação Identity/Audit |
| Care Journey Type System | Regras clínicas |
| Tenant Context (contrato) | Fluxos de agendamento |
| Module Registry | UI de produto |
| Extension Mechanism (hooks) | Storage de arquivos |
| Event Foundation (contrato limitado) | Event Model / catálogo de eventos |
| Runtime Context | Organization data ownership |
| Shared Kernel Types | Configuration values |

### 4.2 Platform Service (D-003 — tiers)

| Pertence | Não pertence |
|---|---|
| Identity, Authorization, Audit | Política LGPD (→ Governance Domain) |
| Configuration, Feature Flag | Ciclo de vida de ordem clínica |
| Communication, Notification | Consentimento de canal (→ Communication Domain) |
| Integration, Webhook | Semântica clínica de interoperabilidade |
| Storage, File, Observability | Registro primário clínico |
| Search, Document Engine, Medical Form Engine, Template, Event Bus *(Strong)* | UI de módulo |
| Compliance Service *(Needs Review)* | Regras de governança |

### 4.3 Business Domain (16 — ADR-0008)

| Pertence | Não pertence |
|---|---|
| 16 domínios confirmados AS-004 | UI, deploy units |
| Regras e vocabulário ubíquo | Transversais técnicos |
| Política de negócio | Projeção de leitura |

### 4.4 Module (D-004 — 15 candidatos)

| Pertence | Não pertence |
|---|---|
| Scheduling, Attendance, Portal | PS, Read Models |
| Clinical Workspace (shell) | Regras clínicas próprias |
| Implementação de domínio | Regras de outro domínio |
| Supporting admin modules | Extension Mechanism |

### 4.5 Extension (D-005)

| Pertence | Não pertence |
|---|---|
| Diagnostic Module (M-14) | Customização por tenant |
| Home Care Module (M-15) | Alteração do Core |
| Extension Domains habilitados | Telemedicine *(Operational Mode)* |
| Composição opcional via Registry | Novo domínio sem ADR |

### 4.6 Read Model / View (D-006)

| Pertence | Não pertence |
|---|---|
| Clinical Timeline, Analytics | Registro primário |
| Projeção, agregação, layout | Decisão de negócio |
| Consumida por módulos | Módulo de negócio |
| Atualizada via Event Foundation | Domínio autônomo |

---

## 5. Core Invariants (I-01 a I-10) — Classification Guardrails

Invariantes do Core que **orientam** classificação — todo PS e projeção deve honrar:

| # | Invariante | Implicação na classificação |
|---|---|---|
| I-01 | Tenant Context obrigatório | Nenhuma categoria bypassa tenant |
| I-02 | Runtime Context propagado | Core — não duplicar em PS como domínio |
| I-03 | Hierarchical Care Model | Domínios/módulos clínicos respeitam níveis |
| I-04 | Care Journey types | Core define tipos — domínios instanciam |
| I-05 | Module Registry | Módulos e Extension registrados — Read Models não |
| I-06 | Extension Mechanism | Extension ≠ customização; Core imutável |
| I-07 | Event Foundation | Read Models atualizam via contrato — sem Event Model no Core |
| I-08 | Shared Kernel Types | PS usam referências — não modelos internos de domínio |
| I-09 | Auditabilidade | PS Audit — não regra de auditoria de negócio |
| I-10 | Autorização obrigatória | PS Authorization — política nos domínios |

---

## 6. Classification Examples (validated)

| Responsabilidade | Classificação | Justificativa |
|---|---|---|
| Tenant context contract | Core Platform | Fundação SaaS — D-001 |
| Module Registry | Core Platform | Composição e extensão |
| Login de usuário | Platform Service (Identity) | Transversal — implementação |
| Política de retenção LGPD | Business Domain (Governance) | Regra de negócio |
| Envio de SMS ao paciente | Platform Service (Communication) | Canal — Communication Domain decide |
| Consentimento de canal | Business Domain (Communication) | Política de negócio |
| Ciclo de vida de prescrição | Business Domain (Clinical Orders) | Vocabulário clínico |
| Geração de PDF de laudo | Platform Service (Document Engine) | Mecanismo — Clinical Documents define artefato |
| Tela de agenda | Module (M-01 Scheduling) | Implementação funcional |
| Ambiente do profissional | Module (M-02 Clinical Workspace) | Shell — sem regras próprias |
| Módulo de laboratório | Extension (M-14 Diagnostic) | Modelo operacional opcional |
| Telemedicine | Operational Mode em M-03 | Não Extension — modo de Care Delivery |
| Prontuário cronológico | Read Model (Clinical Timeline) | Projeção cross-domain |
| Dashboard de SLAs | Read Model (Analytics) + M-12 | Agregação — Operations Monitoring é domínio |
| Busca de pacientes | Platform Service (Search) | Infraestrutura |
| Timeline como módulo | **Rejeitado** | Read Model — não Module Registry |
| Identity no Core | **Rejeitado** | Implementação → PS (D-002) |
| 16 módulos = 16 domínios | **Rejeitado** | Cardinalidade flexível (D-004) |
| Communication Module | **Rejeitado** | Domínio sem módulo dedicado — orquestrado |
| Feature Flag | Platform Service | Transversal — rollout |
| Care Journey start event | Core (type system) + Domain (instância) | Tipo no Core; regra no domínio |
| Organization cadastro | Business Domain (Organization Management) | Vocabulário organizacional |
| Organization no Runtime Context | Core (contrato) vs Domain (dados) | OQ-C01 — Needs Review |

---

## 7. Gray Zones — Resolved Guidance

| Zona cinzenta | Resolução AS-005 |
|---|---|
| Core amplo (ADR-0003) vs PS (ADR-0005) | Core = contratos; PS = implementação (D-002) |
| Clinical Workspace: Core UI vs Module | Module shell M-02; slot mínimo no Core (OQ-C03) |
| Communication: Domain vs PS | Domain = política; PS = canal (AS-004 D-005) |
| Integration: Domain vs PS | Domain = semântica; PS = protocolo (AS-004 D-006) |
| Timeline vs Clinical Record | Record produz; Timeline projeta (D-006) |
| Analytics vs Operations Monitoring | Monitoring = domínio; Analytics = projeção (D-006) |
| Extension vs Configuration | Extension = novo módulo/domínio; Config = parâmetros (D-005) |
| Extension vs Customization | Extension reutilizável; Customization por cliente (ADR-0006) |
| Search vs Timeline | Search = PS; Timeline = Read Model |
| Compliance: Domain vs PS | Governance = domínio; Compliance PS = enforcement (Q-019) |

---

## 8. Validation Tests

| Teste | Pergunta | Passa se… |
|---|---|---|
| **Teste Core** | Desabilitar todos os módulos — estrutura ainda válida? | Core define contratos independentes |
| **Teste PS** | ≥2 módulos precisam sem duplicar? | Sim → PS candidato |
| **Teste Domain** | Especialista usa vocabulário ubíquo? | Sim → Domain |
| **Teste Module** | Usuário interage como funcionalidade de produto? | Sim → Module |
| **Teste Extension** | Desligar por tenant sem quebrar clínica básica? | Sim → Extension |
| **Teste View** | É só leitura agregada de domínios fonte? | Sim → Read Model |
| **Teste Anti-Domain** | É apenas projeção/agregação? | Não promover a Domain |
| **Teste Anti-Module** | É transversal sem UI de produto? | Não promover a Module |

---

## 9. Anti-Patterns

| Anti-pattern | Por que errado | Classificação correta |
|---|---|---|
| Timeline como domínio | Não produz registro primário | Read Model |
| Timeline Module | Read Model ≠ módulo de negócio | Projeção em M-02 |
| Identity no Core | Implementação operacional | Platform Service |
| Notification como domínio | Transversal sem vocabulário clínico | Platform Service |
| 1 domínio = 1 módulo rígido | Ignora N:1 e 1:N | Cardinalidade flexível |
| Extension = customização por cliente | Viola ADR-0006 | Customização nível 4 |
| PS com regra de negócio clínica | PS = mecanismo | Business Domain |
| Module que implementa Audit | Duplica PS | Consumir Audit PS |
| Analytics como domínio (fase atual) | Sem regras autônomas | Read Model |
| Core com regras de agendamento | Regra operacional | Care Coordination Domain |

---

## 10. Quick Reference Card

```text
Contrato estrutural?     → Core Platform
Transversal sem domínio? → Platform Service
Regras + vocabulário?    → Business Domain
Só leitura agregada?     → Read Model
Funcionalidade ao user?  → Module
Opcional por modelo op.? → Extension
```

---

## 11. Open Questions

| ID | Pergunta | Impacto na matriz |
|---|---|---|
| OQ-C01 | Organization Context — Core vs Domain | Exemplo na seção 6 |
| OQ-C03 | UI composition contract no Core | Gray zone Clinical Workspace |
| Q-019 | Compliance Service tier | PS Needs Review |
| Q-003 | Event Model | Read Model update mechanism |

---

## 12. Initial Recommendations

1. **D-008** → promover para **Ready for Confirmation** em `decisions.md`.
2. Usar esta matriz como **gate** para novas responsabilidades — sem inventar domínios.
3. Formalização em documentação oficial aguarda NR-008.
4. Incorporar quick reference em `architecture-foundation.md` na formalização pós-sessão.

---

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1.0 | 2026-07-03 | Esqueleto NR-007 |
| 0.2.0 | 2026-07-03 | Matriz consolidada — I-01 a I-10, exemplos, anti-patterns |

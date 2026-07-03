---
title: Read Models
status: Draft
version: 0.1.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Architecture
phase: Product & Architecture Foundation
related:
  - docs/04-domain/business-domains.md
  - docs/05-architecture/module-strategy.md
  - docs/05-architecture/adr/foundation/ADR-0008-business-domain-map.md
  - docs/05-architecture/adr/foundation/ADR-0009-core-platform-boundary.md
  - architecture-sessions/AS-005-core-platform-boundary.md
---

# Read Models

> Catálogo oficial de **Read Models / Views** — projeções cross-domain sem regras de negócio autônomas.

Decisão formal: AS-004 D-003/D-004 · AS-005 D-006 · ADR-0009.

---

## 1. Definition

**Read Model** = projeção de leitura cross-domain que:

- agrega e apresenta dados de múltiplos domínios fonte;
- **não** possui vocabulário ubíquo autônomo;
- **não** é fonte primária de registro — ownership nos domínios;
- **não** decide regras clínicas ou operacionais.

**Prontuário** = visão institucional da **Clinical Timeline** — não é domínio.

```text
Domínios fonte → produzem registros
Read Model     → projeta leitura
Módulos        → consomem na UI
PS             → suportam (Authorization, Search, Audit)
```

---

## 2. Catálogo confirmado

| Read Model | Origem | Capability | Consumidores |
|---|---|---|---|
| **Clinical Timeline** | Clinical Timeline | Registrar | M-02, M-08 (parcial) |
| **Analytics & Reporting** | Indicators & Dashboards | Monitorar | M-12 |

**Não são** Business Domains nem Modules.

---

## 3. Princípios (H-VIEW-001)

| # | Princípio |
|---|---|
| V-01 | Ownership nos domínios fonte — escrita sempre no domínio |
| V-02 | Read Model agrega e projeta — layout e filtros permitidos |
| V-03 | Read Model não decide regra de negócio |
| V-04 | Apresentação ≠ regra clínica |
| V-05 | Atualização via Event Foundation (Q-003 deferred) |
| V-06 | Authorization antes da projeção |
| V-07 | Read Model não é módulo no Module Registry |

---

## 4. Clinical Timeline

### Fontes (domínios)

Care Delivery · Clinical Record · Clinical Orders · Clinical Documents · Care Monitoring · Communication *(relevância clínica)* · Diagnostic Operations *(se habilitado)* · Integration *(metadados)*

### O que Timeline NÃO faz

| Responsabilidade | Destino |
|---|---|
| Registrar evolução | Clinical Record → M-04 |
| Registro oficial | Clinical Record / Clinical Documents |
| Enviar comunicação | Communication Domain + PS |
| Indexação full-text | Search Service (PS) |

### Posicionamento

Projeção **consumida** pelo Clinical Workspace (M-02) — **não** Timeline Module dedicado.

---

## 5. Analytics & Reporting

Agregação de indicadores cross-domain — dashboards institucionais.

| Fonte | Exemplos |
|---|---|
| Care Monitoring | Pendências, alertas clínicos |
| Operations Monitoring | SLAs operacionais |
| Care Coordination | Agendamentos |
| Múltiplos domínios | KPIs compostos |

**Care Monitoring (M-07)** ≠ Analytics — M-07 é risco clínico individual; Analytics é agregação institucional.

Consumidor principal: **M-12 Operations Dashboard**.

---

## 6. Read Model vs Module vs PS

| Aspecto | Read Model | Module | PS |
|---|---|---|---|
| Escrita primária | Não | Sim (via domínio) | Não |
| UI | Consumida por módulo | Possui UI | Sem UI |
| Registry | Não | Module Registry | Catálogo PS |

---

## 7. Atualização (conceitual)

```text
Domínio persiste → Event Foundation → Projeção atualiza → Módulo renderiza
```

Event Model detalhado: Q-003 — AS-010.

---

## 8. Open Questions

| ID | Assunto |
|---|---|
| OQ-V01 | Critério de Communication na Timeline |
| Q-003 | Event Model e projeção |
| P-MON-01 | Domínio Analytics autônomo futuro |

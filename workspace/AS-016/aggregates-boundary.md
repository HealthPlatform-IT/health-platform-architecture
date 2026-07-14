# Clinical Aggregates — Boundary

> Rascunho AS-016 — NR-001. **Não é decisão** até confirmação.

---

## 1. Definição

**Clinical Domain Aggregates** = consistency boundaries DDD que materializam o Modelo Hierárquico (ADR-0001) nos Business Domains (ADR-0008) — **sem** DDL e **sem** reclassificar níveis hierárquicos como “serviços”.

---

## 2. Inclui / não inclui

| Inclui | Não inclui |
|---|---|
| Catálogo de Aggregate Roots clínicos | Schemas SQL |
| Fronteiras de consistência | Código / classes |
| Relação nível A vs Domain Event | Event Sourcing |
| Referências entre roots | Bounded Context map completo (pode sugerir alinhamento a domínio) |
| Invariantes-chave | UI / módulos |

---

## 3. Princípios

- Preferir aggregates **pequenos**
- Referência entre aggregates = ID (+ tenant)
- Um comando modifica **um** aggregate por UoW (padrão)
- Domain Events após persistência (ADR-0012 / 0015)

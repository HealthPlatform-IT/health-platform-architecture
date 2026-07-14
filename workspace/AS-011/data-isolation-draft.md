# Data Isolation & Read Models — Multi-Tenant

> Rascunho AS-011 — NR-005. Candidato a confirmação — **não é ADR**.

**Pré-requisitos:** NR-002 · ADR-0012 · `read-models.md`  
**Última atualização:** 2026-07-14

---

## 1. Problema

Como isolamento de dados e projeções (Read Models) respeitam o tenant — em nível de **estratégia**, sem DDL.

---

## 2. Modelo de isolamento recomendado (candidato D-003)

| Padrão | Uso |
|---|---|
| **A — Shared + discriminador** | **Padrão** da plataforma |
| **C — Database/instância por tenant** | **Exceção governada** (compliance extremo) |
| **B — Schema por tenant** | Não preferido como padrão; pode ser tática de implementação de A/C em Database Architecture |

**OQ-MT01 Answered (candidato):** shared + tenant discriminator como padrão.

**Implementação física (RLS, índices, connection strings):** Deferred — cartão/sessão Database Architecture.

---

## 3. Invariantes de dados (candidatos)

| ID | Invariante |
|---|---|
| I-DATA-01 | Todo registro de negócio pertence a **exatamente um** tenant |
| I-DATA-02 | Query/comando sem filtro/contexto de tenant = **inválido** |
| I-DATA-03 | Shared Kernel Types referenciam IDs — não vazam dados cross-tenant |
| I-DATA-04 | Blobs (Storage/File) no escopo do tenant |

---

## 4. Read Models

| Read Model | Regra candidata |
|---|---|
| **Clinical Timeline** | Projeção **por tenant** (e por paciente no tenant) |
| **Analytics & Reporting** | Agregação **dentro do tenant**; multi-tenant só com papel plataforma + Audit |

Alinhado ADR-0012: projeção alimentada por Domain Events cujo envelope já carrega tenant.

```text
Domain Event (tenant T) → Event Bus → Projeção atualiza store do tenant T → Módulo lê no contexto T
```

Read Models **não** publicam Domain Events (ADR-0012).

---

## 5. Audit e busca

| PS | Isolamento |
|---|---|
| **Audit Service** | Trilha com tenant; consultas no escopo do tenant (exceto operador plataforma auditado) |
| **Search Service** *(Strong)* | Índice/partição lógica por tenant — a detalhar na promoção do PS |

---

## 6. Exceção silo (modelo C)

Critérios **candidatos** para permitir DB dedicado:

- exigência contratual/regulatória documentada;
- aprovação de governança;
- custo operacional aceito;
- mesmos contratos Tenant Context / Event Foundation.

Sem esses critérios, permanece modelo A.

---

## 7. Fora de escopo

- Escolha de PostgreSQL/MySQL/etc.
- Migrations, particionamento físico
- Implementação RLS

---

## 8. Decisões alimentadas

D-003 · D-007 (Database Deferred) · OQ-MT01

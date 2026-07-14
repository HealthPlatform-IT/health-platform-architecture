# Isolation & Persistence — Database

> Rascunho AS-013 — NR-002. Candidato a confirmação.

---

## 1. Modelo padrão (herda ADR-0013)

| Modelo | Papel |
|---|---|
| **A — Shared database + discriminador `tenant_id`** | **Padrão** |
| **C — Database / instância por tenant** | **Exceção governada** |
| **B — Schema por tenant** | **Não padrão** — evitar como estratégia default |

---

## 2. Invariantes de persistência

| ID | Invariante |
|---|---|
| I-DB-01 | Toda tabela de negócio no shared modelo inclui `tenant_id` (ou equivalente documentado) |
| I-DB-02 | Repository/query path exige Runtime Context; ausência = falha |
| I-DB-03 | Índices compostos relevantes incluem `tenant_id` (orientação) |
| I-DB-04 | Cross-tenant query só em superfície plataforma + Audit (AS-009) |
| I-DB-05 | Silo C preserva os mesmos contratos de Tenant Context / Event Foundation |

---

## 3. RLS (orientação)

- **Application-enforced tenant filter** = obrigatório no monólito
- **RLS no SGBD** = **recomendado** como defesa em profundidade quando o vendor permitir — detalhe de política = Deferred na implementação
- RLS **não** substitui Application Context

---

## 4. Silo (modelo C) — critérios

Permitir DB dedicado se documentados:

1. Exigência contratual/regulatória
2. Aprovação de governança
3. Custo operacional aceito
4. Contratos Core/ADR-0013/0012 inalterados na API de domínio

---

## 5. Database no monólito

- **Um database lógico principal** como default do modular monolith
- Múltiplos databases só com critério (ex.: isolamento de projecção pesada, silo C)
- Schema versioning / migration tool = Deferred (implementação)

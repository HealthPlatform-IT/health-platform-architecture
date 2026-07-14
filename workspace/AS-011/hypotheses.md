# Hipóteses — AS-011

> Hipóteses de investigação — **não são decisões** até confirmação do pacote.

## H-MT-001 — Tenant Context no Core

**Status:** ✅ Validada — D-001/D-004.

Tenant Context permanece **contrato Core** (ADR-0009). Implementação de autenticação/sessão = **Identity Service**.

## H-MT-002 — I-01 universal

**Status:** ✅ Validada — D-004/D-005.

Toda operação de negócio e toda mensagem na Event Bus carrega tenant — sem exceção para fluxos clínicos/operacionais.

## H-MT-003 — Runtime Context ≠ domínio

**Status:** ✅ Validada — OQ-C01 / D-002.

Runtime Context **agrega** referências (tenant, organization, user, escopo). Ownership de cadastro de organização = domínio Organization Management.

## H-MT-004 — Isolamento preferencial shared + discriminador

**Status:** ✅ Validada — D-003 (Ready for Confirmation).

**Shared infrastructure** com isolamento lógico por tenant; silo (DB dedicado) = **exceção governada**.

## H-MT-005 — Configuração por tenant

**Status:** ✅ Validada — D-006.

Configuration Service e Feature Flag operam por tenant (e níveis inferiores: instituição/unidade) — alinhado ADR-0005/0006.

## H-MT-006 — Detalhe físico Deferred

**Status:** ✅ Validada — D-007.

DDL, RLS, particionamento, connection pooling — **sessão Database Architecture**.

## H-MT-007 — Cross-tenant excepcional

**Status:** ✅ Validada — NR-003.

Acesso cross-tenant (operador da plataforma) é **exceção governada** com Authorization + Audit — não padrão de produto clínico.

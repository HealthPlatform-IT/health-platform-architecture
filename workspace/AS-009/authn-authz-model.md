# NR-002 — AuthN / AuthZ Model

> Rascunho AS-009. Candidato a confirmação.

## 1. AuthN (Identity)

| Responsabilidade | Detalhe |
|---|---|
| Autenticar ator | Credencial / token / IdP (produto Deferred) |
| Estabelecer sessão | User Context + Tenant Context obrigatórios |
| Emitir/validar credencial de sessão | Token opaco ou JWT — escolha de produto Deferred |
| Recusar sessão sem tenant | Alinhado ADR-0013 |

**Não faz:** allow/deny de recurso clínico; trilha de auditoria de negócio.

## 2. AuthZ (Authorization)

| Dimensão | MVP / v1 arquitetural |
|---|---|
| **Role** | Papéis por tenant (ex.: clinician, receptionist, admin) — catálogo concreto Deferred |
| **Permission** | Ações sobre tipos de recurso (`order:prescribe`, `record:read`, …) |
| **Scope** | tenant (obrigatório) + organization/unit quando aplicável + resource id |
| **Default** | **Deny-by-default** |
| **Cross-tenant** | Papel de plataforma + política explícita + Audit (ADR-0013) |

**Não é exigido agora:** motor ABAC completo, OPA produto, matriz fina de dezenas de papéis.

## 3. Relação AuthN ↔ AuthZ

```text
Identity autentica → Runtime Context (tenant, user, org ref, …)
Authorization avalia (role, permission, scope, resource) → allow | deny
```

Sem Identity válida / sem tenant → AuthZ nem avalia (rejeição no pipeline).

## 4. Superfícies

| Superfície | AuthN | AuthZ |
|---|---|---|
| Product API | Sim | Sim (exceto allowlist pública) |
| Platform API | Sim (ator plataforma) | Sim + Audit se cross-tenant |
| Integration API | Credencial de integração | Escopos de integração |
| Event consumer / job | Contexto da mensagem / job | Mesmas regras no tenant do envelope |

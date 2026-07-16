# Known gaps / bugs — bootstrap Auth & Staff Session

> Achados em integração Staff Web ↔ Identity (dev mint).  
> **Regra:** documentar primeiro; implementar só quando priorizado.  
> Atualizado: 2026-07-16

---

## Convenção de status

| Status | Significado |
|---|---|
| **Open** | Acordado / observado; ainda sem correção de código |
| **Fixed (front)** | Corrigido no client; aguardando merge/PR se aplicável |
| **Fixed (api)** | Corrigido no backend |
| **Deferred** | Consciente; depende de IdP produto ou sessão futura |

---

## Catálogo

### BUG-001 — Staff Web lia `token` em vez de `accessToken`

| | |
|---|---|
| **Status** | Fixed (front) — pendente publicar na branch Staff Web v2 |
| **Superfície** | `health-platform-front` · Sessão / `POST /auth/dev/token` |
| **Sintoma** | Mint sucedia na API, mas a UI não atribuía o JWT à sessão |
| **Causa** | Contrato da API: `{ accessToken, tokenType, expiresIn }`. Client gravava `res.token` |
| **Ajuste feito** | Client passa a usar `accessToken` (fallback legado `token`); mock alinhado ao contrato; mint força rede e desliga `useMock` em sucesso |
| **Implementação restante** | Commit/PR no `health-platform-front` (não bloqueia documentação) |

---

### GAP-001 — Resposta de mint sem `refreshToken`

| | |
|---|---|
| **Status** | **Open** (acordado — **não implementar agora**) |
| **Superfície** | `health-platform-api` · `POST /auth/dev/token` · Identity adapter (bootstrap) |
| **Sintoma** | Resposta atual só traz `accessToken` (+ `tokenType`, `expiresIn`). Não há `refreshToken` |
| **Por que importa** | Ciclo de sessão Staff precisa renovar credencial sem novo mint manual; alinhado a AuthN/sessão do Identity PS (ADR-0020 / bootstrap Auth) |
| **Direção acordada** | Backend **deve** passar a emitir e retornar `refreshToken` (e contrato/DTO correspondente). Front só consome depois que o contrato existir |
| **Fora deste item** | IdP/SSO produto (continua Deferred). Escopo aqui = adapter de **dev/bootstrap** + contrato mínimo de sessão |
| **Critério de pronto (quando priorizar)** | (1) DTO/response com `accessToken` + `refreshToken` (+ metadados já existentes); (2) endpoint ou fluxo de refresh documentado; (3) Staff Session grava e usa o par; (4) testes de contrato no API |
| **Refs** | [platform-security.md](../../docs/05-architecture/platform-security.md) · [technical-bootstrap.md](../../docs/05-architecture/technical-bootstrap.md) · ADR-0020 · ADR-0024 |

---

### GAP-002 — Mint / sessão Staff ainda DX-only

| | |
|---|---|
| **Status** | Deferred (explícito no bootstrap) |
| **Nota** | `AUTH_DEV_TOKEN_ENABLED` + `/auth/dev/token` não são produto. Refresh (GAP-001) melhora DX e antecipa o contrato de sessão; não substitui IdP |

---

## Fila sugerida (quando for ajustar)

1. ~~BUG-001~~ (front — publicar)
2. **GAP-001** — API: emitir `refreshToken` + contrato; depois Staff Web consumir
3. GAP-002 — permanece até IdP; não misturar com GAP-001

---

## Log

| Data | Evento |
|---|---|
| 2026-07-16 | BUG-001 identificado e corrigido no working tree Staff Web; GAP-001 acordado como Open sem implementação imediata |

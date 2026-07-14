# NR-004 — Auth UX

> Rascunho AS-018. Candidato a confirmação.

## 1. Princípio

**UI não é perímetro de segurança.** Hide/show e desabilitar controles são UX; **allow/deny** = Authorization Service (ADR-0020).

## 2. Fluxo

```text
Login UI → Identity (AuthN) → sessão/token com Tenant + User
        → Staff/Portal carrega Runtime Context da API
        → Chamadas Product API (+ BFF se houver) com credencial
        → Backend AuthZ em toda operação sensível
```

## 3. Responsabilidades do cliente

| Faz | Não faz |
|---|---|
| Login / logout / refresh UX | Decidir permissão real |
| Guardas de rota grosseiras (UX) | Confiar só em “menu oculto” |
| Propagar token / tenant headers | Inventar tenant |
| Tratar 401/403 | Bypass de AuthZ |

## 4. Deferred

IdP/SSO produto · SDK Auth · detalhes de storage de token (httpOnly cookie vs bearer) — critérios: alinhar a S-01…S-04 e ADR-0020; escolha de produto em implementação/DevOps.

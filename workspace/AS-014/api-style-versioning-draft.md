# API Style & Versioning

> Rascunho AS-014 — NR-003.

---

## 1. Estilo padrão

| Superfície | Estilo candidato | Notas |
|---|---|---|
| **Product API** | HTTP **resource-oriented** (REST-like) | Recursos estáveis; verbs HTTP |
| **Platform / PS** | HTTP ou ports in-proc | Mesmos contratos Core; transporte flexível |
| **Integration API** | HTTP + padrões inbound | Separado de Communication (ADR-0004); Webhook PS outbound |

RPC-only como padrão externo: **rejeitado**.

---

## 2. Versionamento (conceitual)

- Versão **maior** incompatível via path: `/v{n}/...`
- Evolução compatível preferida (additive) sem bump
- Deprecação documentada; prazo mínimo a definir na implementação
- Detalhe header `Accept-Version` = opcional complementar — não substitui path major

---

## 3. Idempotência (orientação)

- Comandos mutáveis: suporte a **Idempotency-Key** (header) quando aplicável
- Detalhe de store de idempotência = implementação

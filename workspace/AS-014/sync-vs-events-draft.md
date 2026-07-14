# Sync vs Events

> Rascunho AS-014 — NR-002.

---

## Regra candidata

| Usar **API sync** quando | Usar **Domain Event** quando |
|---|---|
| Cliente precisa da resposta agora (UI, comando) | Fato de negócio **já persistiu** e outros reagem |
| Orquestração Module → Domain no mesmo request | Desacoplar assinantes (Timeline, Audit, Communication) |
| Consulta/leitura sob Runtime Context | Fan-out pós-commit (Outbox → Bus) |
| Chamada Module↔PS de baixa latência in-proc ou remote | Integração **interna** reativa |

## Anti-padrões

- Publicar evento para evitar desenhar endpoint sync legítimo
- Esperar reply via Event Bus para fechar request HTTP do usuário
- Module publicar Domain Event (domínio publica)
- Usar Event Bus para webhook externo (ADR-0012)

```text
Client → API → Application → Domain (+ persist + Outbox)
                              ↓
                         Event Bus → consumers
```

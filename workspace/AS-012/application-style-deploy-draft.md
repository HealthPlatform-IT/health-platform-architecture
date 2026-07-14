# Application Style & Deploy Units — Backend

> Rascunho AS-012 — NR-002. Candidato a confirmação — **não é ADR**.

---

## 1. Decisão candidata (estilo)

| Opção | Veredito candidato |
|---|---|
| **A — Modular monolith** (default) | **Preferido** — um (ou poucos) deployables; boundaries lógicos Module/Domain/PS |
| **B — Microsserviços por módulo (15)** | **Rejeitado** — contradiz ADR-0009 (módulo ≠ deploy) |
| **C — Híbrido controlado** | **Aceito como evolução** — extrair PS ou vertical só com critério |

**Proposta D-00x:** Backend padrão = **modular monolith**; microsserviço = **exceção justificada**.

---

## 2. Critérios para extrair deployable separado (exceção)

Extrair **somente** se ≥ 2 forem verdadeiros:

1. Escala/latência incompatível com o monólito
2. Isolamento de falha crítico (ex.: Integration gateway, telemedicina media)
3. Ciclo de release independente exigido por risco regulatório
4. Ownership de time distinto com contrato estável (anti-corruption)

**Não** extrair porque: existe Module ID, “fica mais limpo”, ou “todo PS é serviço separado” sem necessidade.

---

## 3. Unidades lógicas vs deploy

| Unidade | É deploy? | Nota |
|---|---|---|
| Business Domain | Não (default) | Pacote/biblioteca / bounded context no monólito |
| Module | Não (default) | Orquestra use cases / UI backends |
| Platform Service | Às vezes | Pode ser lib in-proc **ou** serviço — decisão por PS |
| Read Model | Não (inicial) | Projection workers no mesmo deployable |
| Core Platform | Nunca “Core Service” | Contratos/shared kernel packages |

---

## 4. Deployables iniciais (candidatos)

Proposta mínima Sprint 3 / MVP técnico:

| Deployable | Conteúdo |
|---|---|
| **platform-api** (nome lógico) | Modules + Domains + PS in-proc + porta Event Bus |
| **(opcional) workers** | Mesmo código, processo separado para consumers/jobs — mesmo tenant pipeline |

Silo DB (ADR-0013 exceção) **não** implica split de serviço automático.

---

## 5. Alinhamento Event Bus / sync

```text
Client → API (sync use case)
            ↓
     Application / Domain (regra + persist)
            ↓
     Outbox / publish → Event Bus (async)
            ↓
     Consumers (mesmo ou outro processo)
```

Não usar Event Bus para esconder chamada sync entre módulos no mesmo deployable sem motivo.

---

## 6. Deferred (explicito)

| Tema | Destino |
|---|---|
| Linguagem / framework | Após critérios desta sessão + preferência equipe |
| Topologia K8s / mesh | DevOps session |
| BFF | Frontend Architecture |
| Particionamento DB | Database Architecture |

---

## 7. Riscos

- Chamar monólito de “legado” antes de existir
- Criar 10 repos sem produto
- Workers sem Tenant Context

---

## Próximo

**NR-003** — Camadas e fronteiras Module ↔ Application ↔ Domain ↔ PS adapters.

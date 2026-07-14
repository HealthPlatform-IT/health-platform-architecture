# Context Propagation — Multi-Tenant

> Rascunho AS-011 — NR-003. Candidato a confirmação — **não é ADR**.

**Pré-requisitos:** `multi-tenant-boundary.md` · ADR-0009 · ADR-0012  
**Última atualização:** 2026-07-14

---

## 1. Problema

Como o **Tenant Context** (e demais elementos do Runtime Context) se propaga em:

- chamadas síncronas (módulos → domínios → PS);
- Domain Events / Event Bus;
- jobs / processamento assíncrono interno;
- Read Models e Audit.

Sem isso, I-01/I-02 ficam só no papel.

---

## 2. Runtime Context — conteúdos

| Campo | Origem | Obrigatório |
|---|---|---|
| **tenant** | Identity na sessão / contrato Core | **Sim** (I-01) |
| **organization** | Seleção operacional + cadastro Organization Management | Conforme fluxo (referência ID) |
| **user** | Identity | Sim em fluxos autenticados |
| **escopo** | Contexto da operação (unidade, módulo, etc.) | Conforme necessidade |

**OQ-C01 Answered (candidato):** Organization no Runtime Context = **referência** (ID). Ownership do cadastro = **Organization Management Domain** — **não** no Core.

---

## 3. Fluxo síncrono (API / módulo)

```text
Request autenticado
        ↓
Identity valida sessão → Tenant Context + User Context
        ↓
Runtime Context montado (tenant obrigatório)
        ↓
Módulo orquestra no escopo do tenant
        ↓
Domínio valida + persiste (dados isolados por tenant)
        ↓
PS consumidos com o mesmo contexto (I-01/I-02)
```

| Regra candidata | Conteúdo |
|---|---|
| R-PROP-01 | Operação sem tenant → **rejeitada** |
| R-PROP-02 | PS não inventa tenant — recebe do contexto |
| R-PROP-03 | Módulo não “impersona” outro tenant sem Authorization + Audit |

---

## 4. Fluxo assíncrono (Event Bus — ADR-0012)

```text
Domínio persiste no tenant T
        ↓
Domain Event + envelope com tenant T (Event Foundation)
        ↓
Event Bus entrega a assinantes
        ↓
Assinante processa **somente** no tenant T do envelope
```

| Regra candidata | Conteúdo |
|---|---|
| R-EV-01 | Envelope **sempre** carrega tenant (D-005) |
| R-EV-02 | Bus **não** entrega cross-tenant por padrão |
| R-EV-03 | Assinante que ignorar tenant do envelope = violação de invariante |

---

## 5. Jobs / workers internos

Jobs internos (projeções, outbox futuro, etc.) devem:

1. receber tenant explicitamente (mensagem/contexto); ou
2. derivar do envelope do evento que os disparou.

**Proibido (candidato):** job “varre todos os tenants” sem política de plataforma + Audit.

---

## 6. Cross-tenant (exceção)

| Caso | Tratamento candidato |
|---|---|
| Operador da plataforma (suporte) | Authorization explícita + Audit obrigatório |
| Relatório multi-tenant SaaS admin | Fora do produto clínico; sessão Security / Ops |
| Integração externa | Bridge Integration — não via Event Bus cross-tenant |

Alinhado H-MT-007.

---

## 7. Exemplos

| Situação | Tenant no contexto? |
|---|---|
| Login clínico | Sim — Identity resolve |
| `Attendance.Started` na bus | Sim — envelope |
| Atualização Clinical Timeline | Sim — do evento |
| Preview Document Engine | Sim — do Runtime Context da chamada |
| Webhook outbound | Tenant do domínio que acionou Integration |

---

## 8. Fora de escopo (NR-003)

- Headers HTTP específicos, JWT claims schema
- Implementação de middleware
- AuthZ detalhada (AS-009)

---

## 9. Decisões alimentadas

D-004 (propagação) · D-005 (eventos) · OQ-C01

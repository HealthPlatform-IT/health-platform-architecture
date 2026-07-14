# Backend Architecture — Boundary

> Rascunho AS-012 — NR-001. **Não é decisão** até confirmação do pacote.

---

## 1. Definição candidata

**Backend Architecture** = conjunto de decisões sobre **como o software servidor organiza execução** da Health Platform: estilo de aplicação, camadas lógicas, fronteiras Module / Business Domain / Platform Service em runtime, e encaixe com Tenant Context e Event Bus — **sem** definir framework, DDL ou OpenAPI nesta sessão.

---

## 2. O que pertence a esta sessão

| Inclui | Não inclui |
|---|---|
| Estilo: modular monolith / híbrido / critérios de split | Escolha de linguagem/framework |
| Deploy unit vs logical unit | Infra Kubernetes detalhada / DevOps |
| Camadas lógicas do backend | Schemas de banco (Database Architecture) |
| Orquestração Module ↔ Domain ↔ PS | Contratos HTTP/gRPC finos (API Strategy) |
| Pipeline de Runtime Context no request/job | AuthZ policies detalhadas (AS-009) |
| Relação sync use-case vs publish event | Broker vendor (Q-003 tech) |
| Critérios para futuro ADR técnico | Código / repositórios de app |

---

## 3. Relação com categorias oficiais (ADR-0009)

```text
Core Platform      → contratos / invariantes (não é “serviço backend Core”)
Business Domain    → regras e eventos de domínio
Platform Service   → mecanismos transversais (Identity, Event Bus, Audit…)
Module             → superfície de produto / orquestração de fluxos
Read Model         → projeções; ownership nos domínios fonte
```

Backend Architecture **não reclassifica** essas categorias — define **como elas se executam** no servidor.

---

## 4. Herança AS-005 / ADR-0009 (não reabrir)

- Módulo é unidade **lógica** de produto — **não** unidade de deploy por definição.
- Não transformar PS em módulos nem domínio em deploy unit automático.
- Identity / Audit / Config **fora do Core** (são PS).

---

## 5. Herança ADR-0012 / ADR-0013

- Request e workers carregam **Tenant Context** (I-01).
- Após persistir, domínio pode publicar **Domain Event** via Event Bus.
- API síncrona ≠ substituto de evento.
- Isolamento de dados: alinhado a shared+discriminator (detalhe DDL → Database Architecture).

---

## 6. Fronteiras com cartões vizinhos

| Tema | Cartão / sessão | Frontend desta sessão |
|---|---|---|
| Persistência / DDL | Database Architecture | Só: “domínio não vaza tenant”; adapter Isolation |
| Contratos sync / versionamento | API Strategy | Só: camadas expõem portas; estilo sync vs event |
| AuthN/AuthZ | AS-009 | Só: Identity/Authorization como PS no pipeline |
| Broker | Q-003 tech | Só: publish via porta Event Bus |

---

## 7. Riscos a evitar

- Declarar 15 microsserviços porque há 15 módulos
- Colocar regra clínica em adapters de PS
- “Core Service” monolítico com tudo
- Escolher Nest/Spring/Quarkus **antes** do estilo e das fronteiras
- Absorver Database + API nesta sessão (escopo inchado)

---

## 8. Próximo NR

**NR-002** — Estilo de aplicação e unidades de deploy (hipóteses H-01, H-02, H-10).

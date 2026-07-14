# Configuration & Modules Tenancy — Multi-Tenant

> Rascunho AS-011 — NR-004. Candidato a confirmação — **não é ADR**.

**Pré-requisitos:** ADR-0005 · ADR-0006 · ADR-0009 · `multi-tenant-boundary.md`  
**Última atualização:** 2026-07-14

---

## 1. Problema

Como Configuration, Feature Flag e Module Registry variam por tenant (e níveis inferiores) **sem** customização por código (ADR-0006).

---

## 2. Hierarquia de configuração (conceitual)

```text
Tenant
  └── Organization / Institution
        └── Unidade / Site
              └── (especialidade / contexto operacional — se aplicável)
```

| Nível | Responsável típico | Exemplos |
|---|---|---|
| **Tenant** | Cliente SaaS | Modules habilitados, branding base, Feature Flags de produto |
| **Institution** | Organization Management + Config | Políticas locais, preferências de comunicação |
| **Unidade** | Config operacional | Agenda por unidade, recursos locais |

**Q-015** (herança tenant → instituição → unidade) permanece **Open** — AS-011 fixa que a hierarquia **existe**; regras detalhadas de herança/override ficam para sessão de Configuration ou resposta futura a Q-015.

---

## 3. Platform Services envolvidos

| PS / Core | Papel multi-tenant |
|---|---|
| **Configuration Service** | Parâmetros por tenant / institution / unidade |
| **Feature Flag Service** | Rollout / habilitação por tenant (e opcionalmente segmento) |
| **Module Registry** | Módulos declarados; habilitação efetiva por tenant |
| **Identity / Authorization** | Quem pode alterar config (escopo tenant) |

---

## 4. Módulos por tenant

Alinhado ADR-0009 / module-strategy:

```text
Configuration + Feature Flag + Module Registry
        ↓
Módulo habilitado ou não para o tenant
```

| Regra candidata | Conteúdo |
|---|---|
| R-MOD-01 | Core product pode ser default-on; Extension default-off |
| R-MOD-02 | Desabilitar módulo não altera Core Platform |
| R-MOD-03 | Customização por fork de código por tenant = **proibida** como padrão (ADR-0006) |

---

## 5. Engines e configuração

| Engine | Variação por tenant |
|---|---|
| **Medical Form Engine** | Form Definitions via Configuration + metadados (ADR-0010) |
| **Document Engine** | Templates via Template Service; não fork do engine |
| **Template Service** | Templates versionados por escopo (tenant/institution — detalhe futuro) |

---

## 6. O que AS-011 decide vs Deferred

| Decide agora (D-006) | Deferred |
|---|---|
| Config/Flags/Modules são **por tenant** (mínimo) | Schema físico de config (Q-017) |
| Hierarquia tenant → institution → unidade **reconhecida** | Regras de herança (Q-015) |
| Sem customização por código | Processo de aprovação de exceção (Q-016) |

---

## 7. Decisões alimentadas

D-006 · ligação Q-015 / Q-016 / Q-017 (não responde integralmente)

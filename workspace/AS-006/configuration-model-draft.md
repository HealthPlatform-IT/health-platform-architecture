# Configuration Model Draft — Medical Form Engine

> **Promovido para documentação oficial:** `docs/05-architecture/medical-form-engine.md` §7 (ADR-0010 D-005).  
> Registro histórico NR-006.

**Última atualização:** 2026-07-03

---

## 1. Propósito

Descrever como formulários clínicos podem **variar por tenant, instituição, especialidade ou modelo operacional** usando **configuração** (ADR-0006) — sem **customização por código** por cliente.

---

## 2. Hierarquia de configuração (ADR-0006)

```text
Tenant → Instituição → Unidade
```

Aplicado a form definitions:

| Nível | O que configura | Exemplo |
|---|---|---|
| **Tenant** | Catálogo base de definitions habilitadas | Clínica SaaS habilita anamnese geral |
| **Instituição** | Subconjunto + defaults | Hospital A usa anamnese pediátrica v3 |
| **Unidade** | Override limitado | UPA usa triagem simplificada |
| **Especialidade** | Metadado de seleção — não nível hierárquico ADR-0006 | Cardiologia → form set cardiológico |
| **Modelo operacional** | Extension + Feature Flag | Home Care habilita forms M-15 |

**Q-015 (herança detalhada):** implementação de precedência — fase técnica; conceito aceito.

---

## 3. O que é configuração (permitido)

| Item | Mecanismo |
|---|---|
| Habilitar/desabilitar form definition | Configuration + Feature Flag |
| Escolher versão default (v2 vs v3) | Configuration |
| Associar definition a especialidade/contexto | Metadados + Configuration |
| Habilitar forms de Extension Module | Module Registry + Feature Flag |
| Parâmetros de apresentação (ordem de seções) | Configuration sobre definition publicada |

---

## 4. O que é customização (proibido como padrão)

| Item | Tratamento |
|---|---|
| Schema de formulário único por cliente em código | **Customização** — exceção governada Q-016 |
| Regra clínica específica de um hospital no Engine | **Business Domain** ou configuração institucional no domínio |
| Fork de Engine por tenant | **Rejeitado** |
| Campo clínico com semântica proprietária sem domínio | **Rejeitado** — domínio valida |

---

## 5. Relação Engine ↔ Configuration Service

```text
Configuration Service armazena QUAIS definitions estão habilitadas e com quais parâmetros
Medical Form Engine armazena AS definitions (estrutura)
Módulo consulta Configuration → obtém formDefinitionId → solicita render ao Engine
```

O Engine **não substitui** Configuration Service — consome política de habilitação.

---

## 6. Variação por contexto clínico

| Dimensão | Como variar (conceitual) |
|---|---|
| Especialidade | Metadado `specialty` na definition + filtro Configuration |
| Tipo de atendimento | Contexto Care Delivery / Attendance type |
| Telemedicine (mode) | Mesma definition ou variante — Configuration |
| Extension (Diagnostic, Home Care) | Definitions adicionais registradas; habilitadas por Feature Flag |
| Jornada / Episode | Runtime context do Hierarchical Care Model (I-03) |

---

## 7. Evitar customização por cliente

| Prática | Alternativa configurável |
|---|---|
| "Cliente X quer 3 campos a mais" | Nova versão de definition + Configuration por instituição |
| "Cliente Y quer fluxo diferente" | Módulo orquestra; não alterar Engine |
| "Cliente Z quer regra CID específica" | Clinical Record valida no domínio |

---

## 8. Decisão candidata

**D-006 (rascunho):** Variação de formulários por tenant/instituição/unidade/especialidade via Configuration + metadados de definition; proibido fork de Engine ou schema por código cliente.

**Status:** Ready for Confirmation

**Depende de:** Q-015 (detalhe herança) — parcial; Q-016 (customização excepcional) — fora de escopo completo.

# Medical Form Engine — Boundary Draft

> Este documento é um rascunho de trabalho da AS-006.  
> Ele não representa decisão final até confirmação e possível formalização em ADR/documentação oficial.

**Investigação:** NR-001 a NR-005 — fronteira do Medical Form Engine  
**Pré-requisitos:** ADR-0009 · ADR-0005 · `module-strategy.md` · Clinical Record Domain  
**Status:** Rascunho inicial — preparação  
**Última atualização:** 2026-07-03

---

## 1. Propósito deste documento

Definir, como rascunho de trabalho, **o que é o Medical Form Engine**, **o que pertence a ele**, **o que não pertence** e **como se diferencia** de Clinical Record Domain, Document Engine, Template Service e módulos M-03/M-04.

---

## 2. Posição provisória (herdada AS-005)

| Campo | Valor |
|---|---|
| **Classificação** | Platform Service |
| **Tier AS-005** | Strong Candidate |
| **Status ADR-0005** | Identificado |
| **Fronteira** | Needs Review (OQ-PS05) |
| **Capability** | Registrar (primário), Executar (secundário) |

---

## 3. Definição provisória

> Prover **mecanismo de captura estruturada** — formulários clínicos dinâmicos e configuráveis para registro durante o cuidado.

**Não é:** prontuário, domínio de negócio, módulo, documento formal.

---

## 4. Três artefatos conceituais (hipótese H-MFE-002)

| Artefato | Descrição provisória | Owner provisório |
|---|---|---|
| **Form Definition** | Schema de campos, layout, validações de tipo | Medical Form Engine *(a validar)* |
| **Form Instance** | Preenchimento em contexto (atendimento, evolução) | Engine em runtime; conteúdo → domínio |
| **Clinical Content** | Conhecimento clínico persistido (evolução, diagnóstico) | Clinical Record Domain |

```text
┌─────────────────┐     render      ┌──────────────────────┐
│  M-03 / M-04    │ ──────────────► │ Medical Form Engine  │
└─────────────────┘                 └──────────┬───────────┘
                                               │ submit (structured)
                                               ▼
                                    ┌──────────────────────┐
                                    │ Clinical Record Domain │
                                    └──────────────────────┘
```

---

## 5. O que pertence ao Medical Form Engine (provisório)

| Responsabilidade | Exemplo |
|---|---|
| Catálogo / registry de form definitions | "Formulário de anamnese pediátrica v2" |
| Renderização de formulário em contexto | Campos, seções, condicionais de UI |
| Validação estrutural | Tipo, obrigatoriedade de campo, formato |
| Versionamento de definition | v1 → v2 sem perder instâncias históricas |
| Contrato de submit para domínio | Payload estruturado pós-validação |

---

## 6. O que NÃO pertence

| Responsabilidade | Pertence a |
|---|---|
| Semântica clínica (diagnóstico, CID, evolução) | Clinical Record Domain |
| Regras clínicas de negócio | Clinical Record Domain |
| Documento formal (laudo, atestado PDF) | Document Engine + Clinical Documents |
| Template de mensagem / e-mail | Template Service + Communication |
| Fluxo de atendimento (abrir, fechar consulta) | M-03 Attendance |
| Orquestração shell do profissional | M-02 Clinical Workspace |
| Persistência longitudinal do prontuário | Clinical Record Domain |

---

## 7. Consumidores (módulos)

| Módulo | Domínio | Uso provisório do Engine |
|---|---|---|
| **M-03 Attendance** | Care Delivery | Captura durante atendimento — triagem, formulário de consulta |
| **M-04 Clinical Documentation** | Clinical Record | Registro estruturado — anamnese, evolução SOAP |
| M-02 Clinical Workspace | Shell | Hospeda M-03/M-04 — não consome Engine diretamente |

**Tensão T-MFE-02:** mesmo Engine, contextos distintos — investigar em NR-003.

---

## 8. Fronteira com Clinical Record Domain

| Critério | Medical Form Engine | Clinical Record Domain |
|---|---|---|
| Pergunta central | *Como* capturar estruturado? | *O que* foi registrado clinicamente? |
| Ownership de dados clínicos | Não | Sim |
| Validação clínica | Não | Sim |
| Timeline / histórico | Não | Sim (via projeção) |
| Form definition por especialidade | Provável Engine | Semântica dos valores → domínio |

**Regra provisória:** Engine valida **forma**; domínio valida **conteúdo clínico**.

---

## 9. Fronteira com Document Engine (preview — AS-007)

| Medical Form Engine | Document Engine |
|---|---|
| Captura em fluxo | Renderização de documento formal |
| Dados em campos editáveis | Template + merge → artefato |
| Input para Clinical Record | Input para Clinical Documents |

*Não decidir AS-007 nesta sessão — apenas manter fronteira explícita.*

---

## 10. Fronteira com Template Service (preview — Q-014)

| Hipótese H-MFE-004 | Descrição |
|---|---|
| Form definition | Pode referenciar layout template |
| Template Service | Mensagens e documentos — não absorver form engine |
| Overlap | "Template de layout de formulário" — NR-004 |

---

## 11. Riscos

| ID | Risco | Sinal de alerta |
|---|---|---|
| R-MFE-01 | Engine vira prontuário | Engine armazena evoluções e diagnósticos |
| R-MFE-02 | Regra clínica no schema | CID obrigatório definido só no Engine sem domínio |
| R-MFE-03 | Duplicação M-03/M-04 | Cada módulo com motor próprio de formulário |

---

## 12. Invariantes AS-005 aplicáveis (candidatas)

| ID | Aplicação provisória |
|---|---|
| I-03 | Clinical Event level — formulário em contexto de Attendance |
| I-05 | Domínio define política; PS executa mecanismo |
| I-07 | PS não possui regra de negócio de domínio |

*Validar lista completa em NR-001.*

---

## 13. Perguntas abertas neste rascunho

1. Form definition é configurada via Configuration Service ou API do Engine?
2. Engine persiste instâncias em trânsito ou stateless até submit?
3. Extension Modules registram form definitions customizados?
4. Telemedicine (mode em M-03) altera contrato de formulário?

---

## 14. Próximos passos (NR)

| NR | Ação |
|---|---|
| NR-001 | Validar seção 4–8 com exemplos reais |
| NR-002 | Refinar três artefatos e fluxo de submit |
| NR-003 | Matriz M-03 vs M-04 |
| NR-004 | Overlap Template Service |
| NR-005 | Esboçar fronteira Document Engine |

---

## 15. Exemplos a validar na investigação

| Cenário | Módulo | Engine? | Domínio destino |
|---|---|---|---|
| Triagem na recepção | M-03 | Captura | Clinical Record |
| Evolução médica SOAP | M-04 | Captura | Clinical Record |
| Formulário pré-consulta paciente | M-08? | *Fora de escopo inicial* | — |
| Laudo de exame | — | Não | Document Engine (AS-007) |

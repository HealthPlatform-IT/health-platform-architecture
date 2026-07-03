# Medical Form Engine — Boundary

> **Promovido para documentação oficial:** `docs/05-architecture/medical-form-engine.md` (ADR-0010).  
> Este arquivo permanece como registro histórico da investigação NR-001/NR-002.

**Investigação:** NR-001 / NR-002 — definição e fronteira do Medical Form Engine  
**Pré-requisitos:** ADR-0009 · ADR-0005 · `module-strategy.md` · `business-domains.md`  
**Status:** Em investigação — NR-001 concluída conceitualmente  
**Última atualização:** 2026-07-03

---

## 1. Propósito

Definir **o que é o Medical Form Engine**, **o que pertence a ele**, **o que não pertence** e **como se diferencia** de Business Domains clínicos, módulos, Document Engine e Template Service.

**Pergunta-guia da sessão:**

> O que pertence ao Medical Form Engine, o que pertence aos domínios clínicos e o que deve ficar fora do engine?

---

## 2. Definição (investigação NR-001)

### 2.1 O que é

**Medical Form Engine** é um **Platform Service** responsável por permitir que a plataforma **defina, versione, configure, componha e utilize** formulários clínicos dinâmicos de forma **reutilizável**, fornecendo mecanismo de **captura estruturada** sem acoplar regras específicas de domínio ao Core ou aos módulos.

| Característica | Descrição |
|---|---|
| Tipo | Platform Service (Strong Candidate → candidato a **Confirmed** pós-AS-006) |
| Capability primária | Registrar |
| Capability secundária | Executar *(captura durante atendimento)* |
| Consumidores | ≥2 módulos — critério PS atendido |
| UI | Não possui interface final — módulos orquestram experiência |

### 2.2 Por que é Platform Service e não Business Domain

Aplicação da árvore de `architecture-classification.md`:

| Teste | Resultado |
|---|---|
| Contrato estrutural do SaaS? | Não → não é Core |
| Transversal, ≥2 consumidores, sem vocabulário ubíquo de domínio? | **Sim** → Platform Service |
| Regras de negócio e vocabulário clínico ubíquo? | Não — semântica fica nos domínios |
| Projeção cross-domain? | Não → não é Read Model |
| Funcionalidade ao usuário? | Não → não é Module |

**Conclusão investigativa:** O Engine **estrutura e captura**; domínios **interpretam e persistem** o significado clínico. Isso resolve **OQ-PS05** provisoriamente — *não é parte de Clinical Record*.

### 2.3 Posição no ecossistema

```text
                    ┌─────────────────────┐
                    │  Core Platform      │
                    │  (contratos I-03…)  │
                    └──────────┬──────────┘
                               │
    ┌──────────────────────────┼──────────────────────────┐
    │                          │                          │
    ▼                          ▼                          ▼
┌─────────┐           ┌─────────────────┐         ┌──────────────┐
│ M-02    │ compõe    │ Medical Form    │         │ Business     │
│ Shell   │──────────►│ Engine (PS)     │◄────────│ Domains      │
└────┬────┘           └────────┬────────┘  submit └──────────────┘
     │                           │
     ▼                           │ estrutura + captura
┌─────────┐                      │
│ M-03    │──────────────────────┘
│ M-04    │
│ M-05*   │  *a investigar
└─────────┘
```

---

## 3. Modelo de três artefatos (NR-002)

| Artefato | Definição investigativa | Owner |
|---|---|---|
| **Form Definition** | Schema versionado: seções, campos, tipos, obrigatoriedade estrutural, metadados, vínculo a contexto clínico | **Medical Form Engine** |
| **Form Instance** | Preenchimento em runtime vinculado a contexto (Attendance, Episode, Journey) | Engine em execução; **sem ownership clínico** |
| **Clinical Content** | Conhecimento clínico com semântica de domínio após aceite | **Business Domain destino** |

### Fluxo conceitual de submit

```text
1. Módulo solicita renderização (context + formDefinitionId)
2. Engine retorna estrutura + validação estrutural em tempo real
3. Profissional preenche → Form Instance
4. Submit → Engine valida estrutura
5. Payload estruturado → domínio consumidor
6. Domínio valida regra clínica + persiste Clinical Content
7. Audit registra rastreabilidade
```

**Regra central:** após o passo 6, o Engine **não é dono** do conteúdo clínico.

---

## 4. O que pertence ao Medical Form Engine

| # | Responsabilidade | Exemplo |
|---|---|---|
| R-01 | Registry / catálogo de form definitions | "Anamnese pediátrica v3" |
| R-02 | Composição estrutural (seções, campos, ordem) | Seção "Antecedentes" com 5 campos |
| R-03 | Metadados de formulário | especialidade, contexto, tags operacionais |
| R-04 | Versionamento de **definition** | v2 → v3 sem apagar instâncias v2 |
| R-05 | Validação **estrutural** | tipo, formato, obrigatoriedade de campo |
| R-06 | Compatibilidade entre versões de definition | instância v2 legível após v3 publicada |
| R-07 | Contrato de renderização conceitual | módulo solicita, Engine devolve estrutura |
| R-08 | Contrato de submit estruturado | payload tipado pós-validação estrutural |
| R-09 | Suporte a variação configurável | habilitar definition por tenant/instituição *(via Configuration)* |
| R-10 | Coleta estruturada de respostas | valores por fieldId, não semântica clínica |

---

## 5. O que NÃO pertence

| # | Responsabilidade | Owner correto |
|---|---|---|
| X-01 | Significado clínico do dado | Clinical Record / domínio destino |
| X-02 | Diagnóstico, CID, hipótese clínica | Clinical Record |
| X-03 | Prescrição, dose, interação medicamentosa | Clinical Orders |
| X-04 | Decisão clínica / protocolo assistencial | Care Monitoring / domínio |
| X-05 | Documento formal assinável | Clinical Documents + Document Engine |
| X-06 | Assinatura legal / certificado | Clinical Documents *(futuro)* |
| X-07 | Prontuário / histórico longitudinal | Clinical Record |
| X-08 | Timeline cronológica | Read Model (Clinical Timeline) |
| X-09 | UI final / shell do profissional | M-02 Clinical Workspace |
| X-10 | Fluxo de atendimento (abrir/fechar) | M-03 + Care Delivery |
| X-11 | Template de mensagem ou e-mail | Template Service + Communication |
| X-12 | Geração de PDF formal | Document Engine |
| X-13 | Política de acesso a dados | Authorization Service |
| X-14 | Rastreabilidade de auditoria | Audit Service |
| X-15 | Persistência física / APIs / schema técnico | Fase técnica futura |

---

## 6. Critérios para classificar responsabilidades (formulários)

Derivado de `architecture-classification.md` + critérios específicos AS-006:

| Pergunta | Se SIM → | Se NÃO → |
|---|---|---|
| É estrutura/captura reutilizável por ≥2 módulos? | Engine | Módulo ou domínio |
| Envolve vocabulário clínico ubíquo? | Business Domain | Engine *(se só estrutura)* |
| É regra clínica (protocolo, diagnóstico obrigatório)? | Business Domain | — |
| É apenas tipo/formato/obrigatoriedade de campo? | Engine | — |
| É documento formal finalizado? | Clinical Documents + Document Engine | — |
| É projeção de leitura agregada? | Read Model | — |
| Varia por tenant sem alterar código? | Configuration + Engine definitions | Customização *(proibida)* |

---

## 7. Catálogo de exemplos (validação NR-001)

Considerados como casos de teste conceitual — **não** implicam implementação imediata.

| # | Exemplo | Engine? | Módulo orquestrador | Domínio destino do conteúdo | Observação |
|---|---|---|---|---|---|
| E-01 | Anamnese | ✓ captura | M-04 | Clinical Record | Definition no Engine; conteúdo no domínio |
| E-02 | Evolução clínica (SOAP) | ✓ captura | M-04 | Clinical Record | Mesmo padrão E-01 |
| E-03 | Exame físico | ✓ captura | M-03 ou M-04 | Clinical Record | Durante ou pós-atendimento |
| E-04 | Escalas clínicas (Glasgow, PHQ-9) | ✓ captura | M-03/M-04 | Clinical Record | Engine: campos numéricos; domínio: interpretação clínica |
| E-05 | Questionários | ✓ captura | M-03/M-04 | Clinical Record | Estrutura no Engine |
| E-06 | Triagem | ✓ captura | M-03 | Clinical Record | Contexto Attendance (Care Delivery) |
| E-07 | Avaliação terapêutica | ✓ captura | M-03 | Clinical Record | Therapeutic Care Delivery |
| E-08 | Formulário de teleconsulta | ✓ captura | M-03 | Clinical Record | Operational Mode — mesma Engine |
| E-09 | Formulário de home care | ✓ captura | M-15 *(Extension)* | Clinical Record / Home Care | Extension consome Engine por contrato |
| E-10 | Formulário de procedimento | ✓ captura | M-03/M-04 | Clinical Record | — |
| E-11 | Formulário de solicitação | ✓ captura | M-05 | **Clinical Orders** | *Tensão T-MFE-05* — destino pode ser Orders, não só Record |
| E-12 | Consentimento clínico | Parcial | M-04/M-06 | Clinical Record → **Clinical Documents** | Captura no Engine; formalização no Documents + Document Engine |
| E-13 | Protocolo assistencial | ✗ estrutura | — | Care Monitoring | Protocolo = regra de domínio; Engine só captura checklist se configurado |

### Padrão extraído dos exemplos

- **E-01 a E-10:** captura → Clinical Record (via M-03/M-04/M-15)
- **E-11:** captura → **Clinical Orders** — expande consumidores além de M-03/M-04
- **E-12:** captura (Engine) → transição para artefato formal (Documents/Document Engine)
- **E-13:** regra de protocolo **não** no Engine — apenas estrutura de checklist se necessário

---

## 8. Diferenças obrigatórias (resumo)

### vs Clinical Record

| Medical Form Engine | Clinical Record |
|---|---|
| Estrutura e captura | Significado e persistência do conhecimento clínico |
| *Como* registrar | *O que* foi registrado |

### vs Care Delivery

| Care Delivery | Medical Form Engine |
|---|---|
| Executa o cuidado (Attendance) | Fornece mecanismo de formulário |
| Contexto de atendimento | Não controla ciclo de vida do atendimento |

### vs Clinical Orders

| Clinical Orders | Medical Form Engine |
|---|---|
| Intenção e ciclo de vida da ordem | Estrutura de captura para solicitação/prescrição |
| Regra de prescrição | Campos estruturados |

### vs Clinical Documents

| Clinical Documents | Medical Form Engine |
|---|---|
| Artefato formal assinável | Dados que podem alimentar documento |
| Regra de documento válido | Não gera documento |

### vs Document Engine

| Medical Form Engine | Document Engine |
|---|---|
| Formulário dinâmico + respostas | Documento formal renderizado |
| Captura em fluxo | Geração de artefato |

### vs Template Service

| Medical Form Engine | Template Service |
|---|---|
| Estrutura de formulário clínico | Template textual/documental reutilizável |
| Schema de campos | Merge de variáveis em layout |

### vs Clinical Workspace (M-02)

| M-02 | Medical Form Engine |
|---|---|
| Shell / orquestração de experiência | Serviço consumido por módulos carregados no shell |
| Sem regras de negócio | Sem UI final |

---

## 9. Invariantes aplicáveis

| ID | Aplicação |
|---|---|
| I-03 | Formulário vinculado a Clinical Event / Attendance no modelo hierárquico |
| I-05 | Domínio define política clínica; Engine executa mecanismo de captura |
| I-09 | Submit e alterações de definition auditáveis |
| I-10 | Authorization antes de renderizar/preencher formulários sensíveis |

---

## 10. Anti-patterns

| Anti-pattern | Correção |
|---|---|
| Engine armazena evoluções clínicas | Clinical Record |
| CID obrigatório como regra clínica no schema sem domínio | Domínio valida no submit |
| Formulário vira laudo PDF | Document Engine + Clinical Documents |
| Cada módulo com motor próprio | Medical Form Engine único |
| Definition por cliente fora de Configuration | ADR-0006 — customização proibida |
| Timeline alimentada diretamente pelo Engine | Domínio produz; Timeline projeta |

---

## 11. Perguntas remanescentes (outras NR)

| # | Pergunta | NR |
|---|---|---|
| 1 | M-05 consome mesmo contrato que M-03/M-04? | NR-003 |
| 2 | Overlap Template Service em layout | NR-007 |
| 3 | Fronteira exata com Document Engine em E-12 | NR-008 |
| 4 | Herança tenant → instituição → unidade em definitions | NR-006 |
| 5 | Instância em trânsito — Engine persiste ou stateless? | Fase técnica |

---

## 12. Referências cruzadas

- `domain-interactions.md` — matriz domínios e módulos
- `validation-strategy-draft.md` — estrutural vs clínica
- `versioning-strategy-draft.md` — definition vs histórico
- `configuration-model-draft.md` — variação sem customização

# Hipóteses — AS-006

> Hipóteses de trabalho — **não são decisões**. Cada hipótese deve ser validada, refinada ou rejeitada na investigação.

---

## Hipótese principal (H-MFE-001)

Medical Form Engine é **Platform Service** que fornece **mecanismo de captura estruturada**:

| Pertence ao Engine | Pertence ao Clinical Record Domain |
|---|---|
| Estrutura de campos (schema) | Semântica clínica do conteúdo |
| Renderização em UI (contrato) | Evolução, diagnóstico, hipóteses |
| Validação de **tipo** de campo | Validação de **regra clínica** |
| Versionamento de **form definition** | Persistência de conhecimento clínico |

**A validar:** se form definition é artefato do Engine ou do domínio com Engine executando.

---

## Hipótese de três artefatos (H-MFE-002)

```text
Form Definition     →  schema configurável (Engine)
Form Instance       →  preenchimento em contexto de atendimento (Engine + runtime)
Clinical Content    →  conhecimento clínico persistido (Clinical Record Domain)
```

Fluxo conceitual:

```text
M-03/M-04 → solicita renderização → Engine
Usuário preenche → Engine valida estrutura
Submit → domínio Clinical Record recebe Clinical Content
```

---

## Hipótese de consumo dual (H-MFE-003)

M-03 e M-04 consomem o **mesmo Medical Form Engine** com **contextos distintos**:

| Módulo | Contexto | Exemplo |
|---|---|---|
| M-03 Attendance | Captura **durante** atendimento | formulário de consulta, triagem |
| M-04 Clinical Documentation | Registro **estruturado** pós ou durante | anamnese, evolução SOAP |

**Alternativa:** contratos separados `AttendanceForm` vs `DocumentationForm` no mesmo Engine.

---

## Hipótese Template Service (H-MFE-004)

Form definitions **podem referenciar** Template Service para layouts reutilizáveis, mas **não** absorvem responsabilidade de template de mensagem ou documento formal.

- Medical Form Engine ≠ Template Service
- Overlap parcial em "template de layout de formulário" — NR-004 decide

---

## Hipótese Document Engine (H-MFE-005)

| Medical Form Engine | Document Engine |
|---|---|
| Captura dinâmica em fluxo clínico | Documento formal finalizado |
| Dados estruturados em campos | Laudo, atestado, receita renderizada |
| Input para domínio | Input de templates + dados para PDF |

**Fronteira:** submit de formulário → Clinical Record; geração de PDF formal → Document Engine (AS-007).

---

## Hipótese anti-prontuário (H-MFE-006)

O Engine **não persiste** histórico clínico longitudinal. Após submit bem-sucedido, ownership passa ao **Clinical Record Domain**. O Engine pode reter apenas:

- catálogo de form definitions (configuração)
- metadados de instância em trânsito (se necessário — a validar)

**Rejeitar:** Engine como repositório de evoluções clínicas.

---

## Alternativas de classificação (OQ-PS05)

| Alternativa | Descrição | Tendência AS-005 |
|---|---|---|
| A | PS dedicado (H-MFE-001) | **Strong Candidate** — favorecida |
| B | Parte do Clinical Record Domain | Rejeitar — M-03 também consome |
| C | Embutido em M-04 apenas | Rejeitar — M-03 precisa captura em atendimento |

---

## Decisões prováveis (rascunho — não confirmadas)

| ID provisório | Tema | Hipótese inicial |
|---|---|---|
| D-001 | Classificação final do PS | Confirmed PS |
| D-002 | Modelo Form Definition / Instance / Content | H-MFE-002 |
| D-003 | Consumo M-03 / M-04 | H-MFE-003 |
| D-004 | Fronteira Template Service | Parcial — avança Q-014 |
| D-005 | Fronteira Document Engine | Esboço para AS-007 |

*Promover para `decisions.md` após investigação.*

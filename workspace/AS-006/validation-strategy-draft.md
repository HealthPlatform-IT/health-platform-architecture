# Validation Strategy Draft — Medical Form Engine

> **Promovido para documentação oficial:** `docs/05-architecture/medical-form-engine.md` §6 (ADR-0010).  
> Registro histórico NR-004.

**Última atualização:** 2026-07-03

---

## 1. Propósito

Classificar **tipos de validação** em formulários clínicos e definir **ownership** — o que o Medical Form Engine valida vs o que o Business Domain valida.

---

## 2. Taxonomia de validações

| Tipo | Descrição | Owner | Exemplo |
|---|---|---|---|
| **V-01 Estrutural — tipo** | Campo é do tipo esperado | **Engine** | Número em campo numérico |
| **V-02 Estrutural — formato** | Padrão sintático | **Engine** | Data ISO, CPF formato |
| **V-03 Estrutural — obrigatoriedade** | Campo preenchido quando marcado obrigatório | **Engine** | Campo "nome" não vazio |
| **V-04 Estrutural — cardinalidade** | Min/max de seleções, tamanho de texto | **Engine** | Máx. 500 caracteres |
| **V-05 Estrutural — condicional de UI** | Mostrar/ocultar campo por valor de outro | **Engine** *(apresentação)* | Se "sim" → mostrar detalhe |
| **V-06 Clínica — semântica** | Significado do valor no contexto clínico | **Business Domain** | Diagnóstico válido para especialidade |
| **V-07 Clínica — protocolo** | Conformidade com protocolo assistencial | **Care Monitoring** / domínio | Escala de risco acima do limiar |
| **V-08 Clínica — ordem** | Prescrição permitida, interação | **Clinical Orders** | Dose dentro de política institucional |
| **V-09 Clínica — documento** | Artefato atende requisitos legais | **Clinical Documents** | Consentimento completo para termo |
| **V-10 Autorização** | Usuário pode executar ação | **Authorization Service** | Médico pode prescrever |
| **V-11 Integridade referencial** | Referências a entidades existentes | **Domínio destino** | PatientId válido no tenant |

---

## 3. Regra de separação

```text
Engine valida FORMA (estrutura, tipo, sintaxe, obrigatoriedade estrutural)
Domínio valida CONTEÚDO CLÍNICO (semântica, protocolo, regra de negócio)
Authorization valida PERMISSÃO
```

### Teste prático

| Pergunta | Se "estrutura/tipo/formato" → Engine | Se "significado clínico" → Domínio |
|---|---|---|
| O campo está preenchido? | Engine (se obrigatório estrutural) | — |
| O CID informado é válido para o caso? | — | Clinical Record |
| A dose está dentro do permitido? | — | Clinical Orders |
| A data está no formato correto? | Engine | — |
| O protocolo exige segundo campo? | Engine (condicional UI) | Domínio define *se* protocolo exige |

---

## 4. Zona cinzenta — condicionais

| Situação | Classificação investigativa |
|---|---|
| "Se idade < 18, exigir responsável" | **Estrutural-condicional** no Engine (UI + obrigatoriedade); domínio pode reforçar em submit |
| "Se diagnóstico = X, exigir exame Y" | **Clínica** — Clinical Record / Orders no submit |
| "Escala PHQ-9 > 10 → alerta" | **Clínica** — Care Monitoring; Engine só captura score |

**Princípio:** condicionais de **apresentação** no Engine; condicionais de **decisão clínica** no domínio.

---

## 5. Momento da validação

| Fase | Engine | Domínio |
|---|---|---|
| Durante preenchimento | V-01 a V-05 | — |
| No submit (pré-persistência) | V-01 a V-05 (revalidação) | V-06 a V-09, V-11 |
| Pós-persistência | — | Regras de ciclo de vida do domínio |

---

## 6. Anti-patterns

| Anti-pattern | Correção |
|---|---|
| CID válido validado só no Engine | Domínio valida semântica |
| Protocolo clínico inteiro no schema | Care Monitoring + domínio |
| Obrigatoriedade clínica disfarçada de obrigatoriedade estrutural | Documentar em metadados da definition quem valida |

---

## 7. Decisão candidata

**D-004 (rascunho):** Validações V-01 a V-05 pertencem ao Engine; V-06 a V-09 ao domínio destino; V-10 ao Authorization Service.

**Status:** Ready for Confirmation

# Perguntas — AS-006

> Q-013 é a pergunta central (parte Medical Form Engine). Respostas definitivas vão para documentação oficial após confirmação.

---

## Q-013 — Medical Form Engine — escopo e contratos

**Status:** Open  
**Fonte:** `ai-context/open-questions.md`, ADR-0005

### Pergunta-guia

> Qual será o escopo detalhado e os contratos do Platform Service Medical Form Engine?

### Subperguntas — Fronteira PS ↔ Domínio

1. O que é **form definition**, **form instance** e **clinical content**?
2. Quem é owner de **semântica clínica** de um campo?
3. O Engine armazena **definições de formulário** ou apenas executa schemas do domínio?
4. Validações clínicas (obrigatoriedade de diagnóstico, ranges clínicos) — Engine ou Clinical Record Domain?
5. O Engine persiste dados ou apenas entrega captura ao domínio consumidor?

### Subperguntas — Módulos

6. M-03 (Attendance) e M-04 (Clinical Documentation) usam o **mesmo contrato**?
7. Captura durante atendimento vs evolução estruturada — diferenças de contrato?
8. M-02 Clinical Workspace apenas hospeda ou também orquestra formulários?

### Subperguntas — Outros Platform Services

9. Overlap com **Template Service** — formulário usa template? *(Q-014)*
10. Onde termina Medical Form Engine e começa **Document Engine**?
11. **File Service** — anexos em formulários: quem orquestra?
12. **Configuration Service** — formulários por especialidade/tenant: onde configurar?

### Subperguntas — Extensão

13. Extension Modules (M-14 Diagnostic, M-15 Home Care) registram schemas via Engine?
14. Formulários customizados por cliente — configuração ou customização proibida (ADR-0006)?

### Subperguntas — Proteção

15. Como garantir que o Engine **não vira prontuário** (R-MFE-01)?
16. Quais invariantes I-01 a I-10 (AS-005) se aplicam diretamente?

---

## Relacionadas (parcial ou influência)

| ID | Pergunta | Relação |
|---|---|---|
| Q-014 | Template Service independente ou integrado? | NR-004 — não decidir inteiro |
| Q-013 *(AS-007)* | Document Engine | NR-005 — apenas fronteira |
| OQ-PS05 | PS ou parte de Clinical Record? | Resolver nesta sessão |
| Q-009 | Mecanismos de extensão na implementação | Fora de escopo |

---

## Novas perguntas emergentes

_(Adicionar durante investigação; promover para `open-questions.md` se necessário)_

---
title: Título do Contexto para IA
status: Draft
version: 0.1.0
created: YYYY-MM-DD
updated: YYYY-MM-DD

author: Architecture Team

category: AI Context
phase: Foundation

related:
  - Related Document
---

# Título do Contexto para IA

> Este arquivo resume informações essenciais da Health Platform para uso com ferramentas de IA.

---

# Objetivo

Descreva qual contexto este documento fornece para ferramentas de IA.

Exemplo:

- Contextualizar uma parte da arquitetura.
- Orientar geração de código.
- Padronizar nomenclaturas.
- Evitar decisões inconsistentes.

---

# Resumo

Apresente um resumo direto e objetivo do tema.

Este conteúdo deve ser claro o suficiente para que uma IA compreenda rapidamente o contexto antes de gerar código, revisar arquitetura ou sugerir melhorias.

---

# Conceitos Principais

Liste os principais conceitos relacionados.

Exemplo:

- Core
- Tenant
- Clinical Workspace
- Medical Form Engine
- Document Engine

---

# Responsabilidades

Descreva o que este componente, domínio ou módulo deve fazer.

---

# O que NÃO deve fazer

Liste limites claros.

Exemplo:

- Não deve conter regra de negócio que pertence a outro domínio.
- Não deve acessar dados sem validação de tenant.
- Não deve acoplar módulos diretamente.

---

# Regras Importantes

Liste regras que a IA deve respeitar ao trabalhar com este contexto.

Exemplo:

- Sempre considerar arquitetura multi-tenant.
- Sempre validar permissões no backend.
- Nunca assumir que o frontend é responsável por segurança.
- Preservar baixo acoplamento entre módulos.
- Preferir configuração em vez de customização.

---

# Entidades Relacionadas

Liste entidades importantes.

| Entidade | Descrição |
|----------|-----------|
| EntityName | Descrição da entidade |

---

# Fluxo Principal

Descreva o fluxo principal em formato simples.

```text
Entrada
↓
Processamento
↓
Resultado
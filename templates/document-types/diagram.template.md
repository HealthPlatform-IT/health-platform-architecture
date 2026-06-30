---
title: Título do Diagrama
status: Draft
version: 0.1.0

created: YYYY-MM-DD
updated: YYYY-MM-DD

author: Architecture Team

category: Diagram
phase: Foundation

related:
  - Related Document
---

# Título do Diagrama

> Breve descrição do objetivo deste diagrama.

---

# Objetivo

Explique por que este diagrama foi criado.

Descreva qual aspecto da arquitetura ele representa.

---

# Contexto

Explique em qual parte da plataforma este diagrama se aplica.

Descreva quando ele deve ser consultado.

---

# Tipo de Diagrama

Selecione o tipo mais adequado.

Exemplos:

- Arquitetura Geral
- Fluxo de Negócio
- Fluxo de Atendimento
- Fluxo de Dados
- Sequência
- Componentes
- Domínio
- Infraestrutura
- Banco de Dados
- Implantação
- Integração

---

# Diagrama

Insira o código do diagrama.

Exemplo utilizando Mermaid:

````text
graph TD

A[Paciente]

--> B[Agendamento]

--> C[Atendimento]

--> D[Workspace Clínico]

--> E[Prontuário]
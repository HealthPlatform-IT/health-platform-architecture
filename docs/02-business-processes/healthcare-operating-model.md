---
title: Healthcare Operating Model
status: Draft
version: 0.1.0

created: YYYY-MM-DD
updated: YYYY-MM-DD

author: Architecture Team

category: Business
phase: Business Modeling
---

# Healthcare Operating Model

> Este documento descreve como instituições de saúde operam e como seus processos serão representados dentro da Health Platform.

---

# Objetivo

O objetivo deste documento é estabelecer o modelo operacional que servirá como base para toda a arquitetura da Health Platform.

A plataforma não será construída em torno de funcionalidades específicas, mas sim em torno dos processos reais existentes nas instituições de saúde.

Independentemente do porte ou especialidade da organização, a plataforma deverá ser capaz de representar seus processos de forma configurável, modular e escalável.

---

# Princípio Fundamental

A Health Platform não existe para impor um fluxo operacional às instituições de saúde.

Ela existe para representar digitalmente seus processos de negócio.

Toda decisão arquitetural deverá respeitar este princípio.

---

# Visão Geral

Toda instituição de saúde pode ser compreendida como um conjunto de processos que transformam uma necessidade do paciente em um atendimento concluído.

Esses processos variam conforme o tipo de instituição, mas seguem uma estrutura semelhante.

A missão da Health Platform é fornecer uma plataforma capaz de representar esses processos sem depender de regras específicas para cada cliente.

---

# Componentes do Modelo Operacional

O modelo operacional da Health Platform será composto pelos seguintes elementos:

## Atores

Representam quem participa dos processos.

Exemplos:

- Paciente
- Profissional de Saúde
- Recepcionista
- Enfermeiro
- Gestor
- Laboratório
- Convênio
- Sistemas Externos

---

## Jornadas

Representam experiências completas.

Exemplos:

- Jornada do Paciente
- Jornada do Profissional
- Jornada Administrativa
- Jornada Diagnóstica

Cada jornada poderá envolver diversos processos.

---

## Processos

Representam como a instituição opera.

Exemplos:

- Agendamento
- Check-in
- Atendimento
- Solicitação de Exames
- Emissão de Documentos
- Faturamento
- Comunicação
- Retorno

Os processos originam as capacidades da plataforma.

---

## Capacidades

Cada processo gera uma ou mais Business Capabilities.

As capacidades representam aquilo que a plataforma deve ser capaz de realizar.

---

## Domínios

As Business Capabilities serão posteriormente agrupadas em domínios de negócio.

---

# Modelo Conceitual

```text
Instituição de Saúde

↓

Atores

↓

Jornadas

↓

Processos

↓

Business Capabilities

↓

Domains

↓

Modules

↓

Features
---
title: ADR-0001 - Modelo Hierárquico do Cuidado
status: Accepted

date: 2026-07-02

deciders:
  - Architecture Team

tags:
  - architecture
  - domain
  - healthcare
  - core
---

# ADR-0001 — Modelo Hierárquico do Cuidado

## Status

Accepted

---

# Contexto

Durante a fase de modelagem do negócio da Health Platform foi identificado que diferentes instituições de saúde possuem modelos operacionais distintos.

Apesar dessas diferenças, todas compartilham uma característica comum:

o cuidado é contínuo e acontece através de diversas interações ao longo do tempo.

Modelar a plataforma apenas em torno de consultas, prontuários ou atendimentos criaria limitações para representar corretamente diferentes modelos operacionais, como:

- Assistência Ambulatorial
- Telemedicina
- Laboratórios
- Centros de Diagnóstico
- Home Care
- Hospitais (futuramente)

Era necessário definir um modelo conceitual único capaz de representar qualquer fluxo de cuidado.

---

# Decisão

A Health Platform adotará um Modelo Hierárquico do Cuidado como estrutura central do domínio clínico.

A representação do cuidado será organizada em seis níveis.

```text
Paciente
    │
    ▼
Jornada de Cuidado
    │
    ▼
Episódio de Cuidado
    │
    ▼
Atendimento
    │
    ▼
Evento Clínico
    │
    ▼
Artefato Clínico
```

Cada nível possui responsabilidades específicas e complementares.

---

# Descrição dos níveis

## Paciente

Representa a pessoa que recebe o cuidado.

É o elemento permanente da plataforma.

---

## Jornada de Cuidado

Representa toda a história de relacionamento clínico do paciente com a instituição.

Uma jornada pode durar anos e agrupar diversos episódios de cuidado.

---

## Episódio de Cuidado

Representa um problema, condição clínica ou objetivo assistencial específico.

Exemplos:

- Hipertensão
- Gestação
- Fratura
- Diabetes
- Check-up

Um episódio pode possuir diversos atendimentos.

---

## Atendimento

Representa uma interação entre o paciente e a instituição.

Exemplos:

- Consulta
- Teleconsulta
- Sessão
- Coleta
- Procedimento
- Internação

Cada atendimento pertence a um único episódio.

---

## Evento Clínico

Representa tudo o que acontece durante um atendimento.

Exemplos:

- Evolução
- Diagnóstico
- Solicitação de exame
- Prescrição
- Encaminhamento
- Alta
- Comunicação clínica

---

## Artefato Clínico

Representa os registros produzidos pelos eventos clínicos.

Exemplos:

- Receita
- Atestado
- Exame
- Laudo
- Consentimento
- Documento clínico
- Relatórios

---

# Justificativa

Esse modelo permite representar diferentes modelos operacionais utilizando a mesma arquitetura.

O foco deixa de ser uma consulta isolada e passa a ser o cuidado contínuo do paciente.

Além disso, essa estrutura favorece:

- reutilização entre módulos;
- escalabilidade da plataforma;
- rastreabilidade clínica;
- integração com padrões internacionais;
- evolução do produto sem alteração do Core.

---

# Consequências

## Positivas

- Modelo único para diferentes instituições.
- Separação clara entre contexto clínico e operacional.
- Base sólida para prontuário eletrônico.
- Facilita auditoria e rastreabilidade.
- Facilita analytics e inteligência artificial.
- Suporta crescimento futuro da plataforma.

## Negativas

- A modelagem inicial é mais complexa.
- Exige disciplina arquitetural para evitar atalhos.
- Requer que todos os módulos respeitem essa hierarquia.

---

# Impacto na Arquitetura

Este ADR influencia diretamente:

- Business Domains
- Business Capabilities
- Modelagem do Banco de Dados
- APIs
- Clinical Workspace
- Timeline Clínica
- Document Engine
- Event Engine
- Platform Services

---

# Relação com outros ADRs

Este ADR deverá servir como base para todos os ADRs relacionados ao domínio clínico.

Nenhum ADR futuro deverá contrariar este modelo sem que este documento seja revisado.
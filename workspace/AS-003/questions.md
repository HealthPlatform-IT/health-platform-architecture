# Questões Arquiteturais — AS-003

> Perguntas em investigação nesta sessão. Respostas definitivas devem ir para documentação oficial ou ADR — não apenas neste arquivo.

---

## Q-001 — Quando a instituição assume responsabilidade pelo cuidado?

**Status:** In Analysis  
**Prioridade:** Critical  
**Fonte centralizada:** `ai-context/open-questions.md`

### Contexto

A Health Platform é orientada pela Jornada de Cuidado, mas a jornada institucional não nasce necessariamente no cadastro do paciente. Diferentes modelos operacionais podem assumir responsabilidade em momentos distintos.

Gatilhos candidatos (das fontes):

- Agendamento de consulta
- Encaminhamento aceito
- Admissão em unidade ou programa
- Solicitação de exame aceita
- Programa de acompanhamento
- Atendimento de urgência

### Subperguntas

- Cadastro/vínculo em Relacionar é suficiente, necessário ou apenas pré-requisito?
- Existe um gatilho universal ou família de gatilhos configuráveis (ADR-0006)?
- Quem dispara o início (recepção, profissional, sistema externo, paciente)?
- Qual alternativa (A–E da sessão) melhor equilibra simplicidade e precisão semântica?

### Impacto

Modelo de domínio, Institution Care Journey, Care Episode, Clinical Workspace, timeline clínica, eventos de negócio candidatos.

---

## Ciclo de vida — manutenção, pausa, encerramento, reabertura

### O que mantém uma jornada ativa?

- Quais eventos ou condições renovam a responsabilidade institucional?
- Episódios ou atendimentos em andamento mantêm a jornada ativa?
- Existe período de inatividade antes de transição automática (pausa ou encerramento)?

### Quando uma jornada pode ser pausada?

- Pausa é estado explícito ou inferido por inatividade?
- Quais modelos operacionais usam pausa (programas, acompanhamento)?
- Pausa suspende monitoramento e comunicação?

### Quando uma jornada pode ser encerrada?

- Alta assistencial, transferência, óbito, desistência, conclusão de programa?
- Laboratório/imagem: encerramento na entrega do resultado?
- Encerramento é terminal até reabertura?

### Quando uma jornada pode ser reaberta?

- Reativação da mesma jornada vs criação de nova jornada?
- Quais gatilhos de reabertura (retorno espontâneo, novo encaminhamento)?

---

## Conceitos — Patient Life Journey vs Institution Care Journey

- O que a plataforma representa e o que permanece fora do escopo?
- Um paciente pode ter múltiplas Institution Care Journeys ativas na mesma instituição?
- Como registrar continuidade entre jornadas encerradas e reabertas?
- Como alinhar ADR-0001 ("história com a instituição") com architecture-foundation ("trecho assistencial")?

---

## Conceitos — Care Journey vs Care Episode vs Attendance

- Quando nasce o primeiro Care Episode em relação ao início da jornada?
- Um Attendance pode existir sem Care Episode prévio?
- Pode haver múltiplos episódios simultâneos dentro de uma jornada?
- Qual a fronteira Organizar → Executar → Registrar em cada nível?

---

## Estados do ciclo de vida

- Estados candidatos (Active, Paused, Closed, Reopened) são suficientes?
- Estados pertencem apenas a Care Journey ou também a Care Episode?
- Quais transições são válidas entre estados?

---

## Cardinalidade e invariantes

- Um paciente : quantas jornadas ativas por instituição?
- Invariantes ADR-0001: Attendance → Episode → Journey — há exceções por modelo operacional?

---

## Perguntas por modelo operacional

Detalhamento em `operating-model-triggers.md`.

| Modelo | Pergunta-chave |
|---|---|
| Clínica ambulatorial | Início no agendamento, check-in ou atendimento? |
| Telemedicina | Início na solicitação, agendamento ou teleconsulta? |
| Laboratório | Início no pedido, coleta ou admissão da amostra? |
| Centro de imagem | Início no agendamento ou na realização? |
| Home care | Início na admissão do programa ou na primeira visita? |
| Hospital (futuro) | Admissão vs urgência — reserva conceitual apenas |
| Medicina ocupacional | Vínculo empresarial, exame admissional ou programa PCMSO? |

---

## Impactos futuros (registrar, não resolver nesta sessão)

- Quais Business Domains prováveis na AS-004?
- Quais eventos de negócio candidatos (sem Event Model — Q-003)?
- O que o Clinical Workspace exibe por estado da jornada?

---

## Novas perguntas emergentes

_(Adicionar aqui durante a investigação; promover para `open-questions.md` com ID Q-018+ se relevante.)_

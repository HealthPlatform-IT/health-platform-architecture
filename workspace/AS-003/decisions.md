# Decisões — AS-003

> Decisões confirmadas em 2026-07-02. Formalização: `care-journey-lifecycle.md` e ADR-0007.

## Decisão 001 — Patient Life Journey vs Institution Care Journey

**Patient Life Journey** é a história ampla de saúde da pessoa, potencialmente distribuída por múltiplas instituições — fora do escopo integral da Health Platform.

**Institution Care Journey** é a história de cuidado gerenciada por uma instituição na plataforma. Corresponde ao nível **Care Journey** do Modelo Hierárquico (ADR-0001) no contexto institucional.

**Status:** Confirmada  
**Relaciona:** H-001, tensão documental ADR-0001 vs architecture-foundation  
**Formalização:** ADR-0007, `care-journey-lifecycle.md`

---

## Decisão 002 — Vínculo cadastral ≠ início da jornada

Cadastro e vínculo em **Relacionar** são pré-requisito, mas **não iniciam** a Institution Care Journey.

A jornada só começa com um evento de assunção de responsabilidade operacional pelo cuidado.

**Status:** Confirmada  
**Relaciona:** H-002, Alternativa A (rejeitada)  
**Formalização:** ADR-0007

---

## Decisão 003 — Critério de início (Q-001)

A Institution Care Journey **inicia** quando a instituição registra um **Care Journey Start Event** — aceite explícito de responsabilidade por uma demanda de cuidado.

O Core define uma família de tipos de evento válidos. Qual tipo dispara o início em cada modelo operacional é **configurável** por instituição (ADR-0006).

**Tipos válidos no Core:**

| Tipo | Descrição |
|---|---|
| `ReferralAccepted` | Encaminhamento aceito pela instituição |
| `CareProgramEnrolled` | Paciente admitido em programa de cuidado |
| `ServiceRequestAccepted` | Pedido de serviço aceito (exame, procedimento) |
| `ScheduledCareCommitted` | Agendamento confirmado com compromisso institucional |
| `EmergencyCareAssumed` | Responsabilidade assumida em urgência |
| `HomeCareAdmitted` | Admissão em programa de home care |
| `OccupationalCareEnrolled` | Ingresso em programa de saúde ocupacional |

**Status:** Confirmada  
**Relaciona:** Q-001, Alternativas D + E  
**Formalização:** ADR-0007, `care-journey-lifecycle.md`

---

## Decisão 004 — Gatilho por modelo operacional

| Modelo operacional | Care Journey Start Event padrão |
|---|---|
| Clínica ambulatorial | `ReferralAccepted` ou `ScheduledCareCommitted` |
| Telemedicina | Igual ao ambulatorial |
| Laboratório | `ServiceRequestAccepted` |
| Centro de imagem | `ServiceRequestAccepted` ou `ScheduledCareCommitted` |
| Home care | `HomeCareAdmitted` |
| Medicina ocupacional | `OccupationalCareEnrolled` |
| Hospital *(futuro)* | `EmergencyCareAssumed` — reserva conceitual |

O primeiro **Attendance** não é obrigatório para iniciar a jornada.

**Status:** Confirmada  
**Relaciona:** H-003, H-009, H-010, H-011  
**Formalização:** `care-journey-lifecycle.md`

---

## Decisão 005 — Care Journey, Care Episode e Attendance

| Nível | Regra |
|---|---|
| **Care Journey** | Container institucional com ciclo de vida (estados) |
| **Care Episode** | Nasce com a jornada ou logo após o Start Event; representa condição ou objetivo assistencial |
| **Attendance** | Interação concreta; sempre pertence a um Care Episode |

Jornadas diagnósticas curtas com um único episódio são **válidas**.

**Status:** Confirmada  
**Relaciona:** H-004, H-005, H-006  
**Formalização:** ADR-0007

---

## Decisão 006 — Estados do ciclo de vida

Estados no nível **Institution Care Journey**:

| Estado | Significado |
|---|---|
| **Active** | Instituição mantém responsabilidade assistencial |
| **Paused** | Responsabilidade suspensa temporariamente |
| **Closed** | Responsabilidade encerrada |

**Manutenção Active:** episódios ou atendimentos em andamento, programas ativos, pendências monitoradas.

**Paused:** programa suspenso ou inatividade assistencial prolongada (critério configurável).

**Closed:** alta, transferência, conclusão de programa, entrega de resultado diagnóstico, desistência.

**Reabertura:** após `Closed`, um novo Care Journey Start Event cria uma **nova** Institution Care Journey — não reativa o registro anterior.

**Status:** Confirmada  
**Relaciona:** H-007, H-008  
**Formalização:** `care-journey-lifecycle.md`

---

## Decisão 007 — Cardinalidade

Por paciente e instituição: **uma Institution Care Journey Active** por vez.

Cenários com programas paralelos (ex.: ocupacional + ambulatorial) permanecem em aberto (**Q-018**).

**Status:** Confirmada (com ressalva Q-018)  
**Formalização:** ADR-0007

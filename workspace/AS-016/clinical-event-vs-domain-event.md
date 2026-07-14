# Clinical Event (nível A) vs Domain Event (B)

> Rascunho AS-016 — NR-003. Alinha ADR-0001 e ADR-0012.

| | **Clinical Event (A)** | **Domain Event (B)** |
|---|---|---|
| Natureza | Nível do modelo hierárquico / fato clínico no cuidado | Fato de negócio publicado na bus |
| Persistência | Parte do modelo do domínio (ex. dentro de Attendance / Record) | Mensagem Outbox → Event Bus |
| Exemplos | evolução ocorrida no atendimento; “solicitação” como ato | `Attendance.Started`, `Order.Prescribed` |
| Owner | Domínio clínico persistente | Domínio publicador do tipo |

**Regra:** quando um Clinical Event (A) (ou mudança de Attendance/Order/Artifact) é confirmado e persistido, o domínio pode emitir Domain Event (B) correspondente. A ≠ B.

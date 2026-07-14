# Invariants & References

> Rascunho AS-016 — NR-004.

---

## 1. Invariantes candidatos

| ID | Invariante |
|---|---|
| I-AGG-01 | Todo aggregate clínico carrega **tenant_id** |
| I-AGG-02 | CareJourney referencia exatamente um Patient |
| I-AGG-03 | CareEpisode referencia CareJourney (+ Patient) |
| I-AGG-04 | Attendance referencia CareEpisode (+ Patient); modo telemedicina = Operational Mode (ADR-0009) |
| I-AGG-05 | ClinicalArtifact / ClinicalOrder / ClinicalEntry referenciam Patient (+ Attendance quando aplicável) |
| I-AGG-06 | Sem navegação “share-nothing” que carregue outro aggregate inteiro na mesma UoW de escrita |
| I-AGG-07 | Stream de ordering Event Bus usa aggregate id do root que publicou (ADR-0017) |

---

## 2. Start Journey

Regras de início de Institution Care Journey permanecem **ADR-0007** — não reabrir aqui.

---

## 3. Deferred

- DDL / tabelas
- Lista fechada de ClinicalEntry subtypes
- Métodos/comandos de cada aggregate
- Mapa completo BC ↔ módulo

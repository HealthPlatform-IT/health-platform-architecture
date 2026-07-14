# Hipóteses — AS-016

| ID | Hipótese | Status |
|---|---|---|
| H-01 | Níveis hierárquicos ≠ 1:1 com Aggregate Roots | Candidata |
| H-02 | Roots: Patient, CareJourney, CareEpisode, Attendance, ClinicalArtifact (+ ClinicalOrder) | Candidata |
| H-03 | Clinical Event (nível A) = **entity/coleção dentro de Attendance** (e/ou fato em Clinical Record) — **não** Aggregate Root | Candidata |
| H-04 | Clinical Event (A) ≠ Domain Event (B) | Confirmada ADR-0012 |
| H-05 | Episode e Journey separados (Journey enorme se embutir tudo) | Candidata |
| H-06 | Referências só por ID + tenant | Candidata |
| H-07 | Ownership por domínio ADR-0008 | Candidata |
| H-08 | Sem agregados “god” (Journey com todo prontuário) | Candidata |
| H-09 | DDL / tabelas Deferred (ADR-0015) | Confirmada |
| H-10 | Q-001 start rules já em ADR-0007 — não reabrir | Confirmada |

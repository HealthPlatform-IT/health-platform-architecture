# Hipóteses — AS-013

| ID | Hipótese | Status |
|---|---|---|
| H-01 | Discriminador `tenant_id` obrigatório em tabelas de negócio do shared DB | Candidata |
| H-02 | Acesso a dados sempre via contexto de tenant (app + opcional RLS) | Candidata |
| H-03 | Schema-por-tenant **não** é padrão; silo C = exceção governada | Candidata |
| H-04 | Domínio possui estado relacional estruturado; blobs → Storage; metadados arquivo → File | Candidata |
| H-05 | Read Models em store/schema de projeção separado logicamente; mesmo deployable OK | Candidata |
| H-06 | Outbox (ou equivalente) no mesmo UoW da escrita de domínio | Candidata |
| H-07 | Vendor: critérios agora; produto Deferred pós-confirmação / PoC | Candidata |
| H-08 | Um database lógico principal no monólito; splits só com critério | Candidata |
| H-09 | Q-017 config schema permanece Deferred | Confirmada (open-questions) |
| H-10 | Migrations reais e código fora desta sessão | Confirmada |

# Perguntas — AS-011

> Perguntas de investigação. Respostas vivem em drafts e, após confirmação, em ADR/docs oficiais.

## Q-008 — Estratégia multi-tenant

1. O que é um **Tenant** na Health Platform?
2. Qual a diferença entre Tenant, Organization, Institution e Unidade?
3. Quem resolve o Tenant Context (Identity vs Core)?
4. Como o Runtime Context se relaciona com Organization Management?
5. Quais modelos de isolamento de dados são candidatos?
6. O isolamento é único para toda a plataforma ou pode variar por PS?
7. Como o tenant se propaga em chamadas síncronas?
8. Como o tenant se propaga em Domain Events (ADR-0012)?
9. Configuration e Feature Flag — granularidade por tenant?
10. Módulos habilitados por tenant — quem decide?
11. Read Models — isolamento por tenant obrigatório?
12. Cross-tenant é permitido em algum caso (ex.: super-admin plataforma)?
13. O que fica para Database Architecture e Security?
14. Q-008 será Answered completo ou parcial (estratégia vs DDL)?

## OQ-C01 — Organization Context

Organization Context vive só no Runtime Context (Core) ou no domínio Organization Management?

## OQ-MT01 — Isolamento

Shared database com discriminador de tenant vs schema/database por tenant — qual direção conceitual?

# Perguntas — AS-013 Database Architecture

1. Como materializar **shared + discriminator** na persistência (coluna, RLS candidata, repositories)?
2. Quando permitir **silo** (DB/instância por tenant) e quais invariantes se mantêm?
3. Schema-por-tenant é padrão, exceção ou rejeitado?
4. Quem ownership de dados: Domain vs Storage/File PS?
5. Read Models: mesmo store, schema separado, ou store dedicado?
6. Outbox / idempotência de publicação vs persistência (ADR-0012)?
7. Soft delete, auditoria de dados, retenção — fronteira com Audit PS?
8. Critérios de **vendor** (não escolha final)?
9. Multi-database no monólito (um DB vs vários) — orientação?
10. O que permanece Deferred (pooling, migrations tool, Q-017 config schema)?

# Perguntas — AS-012 Backend Architecture

## Perguntas centrais

1. Qual o **estilo de aplicação** padrão (modular monolith vs microsserviços vs híbrido)?
2. O que é **unidade de deploy** vs unidade lógica (Module / PS / Domain)?
3. Quais **camadas** o backend expõe (API / Application / Domain / Infrastructure)?
4. Como Module orquestra Domínio e consome PS sem virar “God service”?
5. Onde vivem os **contratos Core** no runtime (libs? packages? kernel)?
6. Como Runtime Context / Tenant entra em **todo** request e job (ADR-0013)?
7. Qual a fronteira com **API Strategy** e **Database Architecture** (não absorver)?
8. Event Bus: backend publica após commit — quem orquestra sync vs async?
9. Read Models: processo dedicado vs mesmo deployable?
10. Critérios para **quando** extrair um microsserviço (exceção)?
11. Frontend BFF — faz parte desta sessão ou Deferred?
12. O que fica Deferred para Security (AS-009) e broker (Q-003 tech)?

## Fora deste cartão

- DDL / vendor DB → Database Architecture
- OpenAPI / versionamento fino → API Strategy
- AuthN/AuthZ detalhada → AS-009

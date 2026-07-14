# Hipóteses — AS-014

| ID | Hipótese | Status |
|---|---|---|
| H-01 | API HTTP resource-oriented (REST-like) como estilo **padrão** externo | Candidata |
| H-02 | Sync = comando/consulta imediata; Event = fato já ocorrido / reação | Candidata |
| H-03 | Três superfícies: Product API, Platform/PS API, Integration API | Candidata |
| H-04 | Versionamento maior via URL path `/v{n}` (conceitual) | Candidata |
| H-05 | Tenant obrigatório em Product/Platform (exceto paths públicos Security) | Candidata |
| H-06 | Correlation-Id / Request-Id em toda resposta de erro e sucesso | Candidata |
| H-07 | OpenAPI e serialização finais Deferred | Candidata |
| H-08 | Webhooks outbound = Integration/Webhook PS — não misturar com Product API | Candidata |
| H-09 | BFF = Frontend Architecture / Deferred | Candidata |
| H-10 | Não usar Event Bus para esconder chamada sync no monólito | Confirmada (ADR-0014) |

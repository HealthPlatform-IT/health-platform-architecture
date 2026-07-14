# Hipóteses — AS-012

| ID | Hipótese | Status |
|---|---|---|
| H-01 | **Modular monolith** como default — Module/PS/Domain são boundaries lógicos | Candidata |
| H-02 | Microsserviço só com critério explícito (escala/isolamento/team) — nunca = Module ID | Candidata |
| H-03 | Camadas: API → Application (use cases) → Domain → Infrastructure (PS adapters) | Candidata |
| H-04 | Core Platform = pacotes/contratos compartilhados, não serviço “Core” runtime | Candidata (ADR-0009) |
| H-05 | Platform Services = modules técnicos / libs / serviços internos — consumidos por contrato | Candidata |
| H-06 | Tenant Context obrigatório no pipeline de request e workers (ADR-0013) | Confirmada pela fundação |
| H-07 | Publicação Event Bus pós-persistência no mesmo bounded context do escritório | Candidata (ADR-0012) |
| H-08 | Framework/language = Deferred até critérios; sessão define estilo, não stack | Candidata |
| H-09 | BFF / Frontend Architecture = sessão separada | Candidata |
| H-10 | Read Models podem ser projections no mesmo deployable inicialmente | Candidata |

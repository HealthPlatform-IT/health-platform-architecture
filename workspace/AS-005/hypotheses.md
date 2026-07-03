# Hipóteses — AS-005

> Hipóteses de trabalho — **não são decisões**. Cada hipótese deve ser validada, refinada ou rejeitada na investigação.

---

## Hipótese principal (H-CORE-001)

O **Core Platform** contém apenas fundamentos estruturais necessários para a plataforma existir como SaaS multi-tenant extensível — **não** regras de negócio clínicas ou operacionais.

**A validar:** reconciliação com ADR-0003 (que lista identidade/autorização no Core).

---

## Hipótese de reconciliação Core ↔ PS (H-CORE-002)

O Core Platform define **contratos e invariantes estruturais**; os **Platform Services implementam** responsabilidades transversais consumíveis por módulos e domínios.

| Camada | Papel |
|---|---|
| Core Platform | Contratos, tipos base, mecanismos de extensão, registry |
| Platform Service | Implementação reutilizável com API estável |

**Exemplo:** Core define o contrato de `TenantContext` e tipos de Care Journey Start Event; Identity Service implementa autenticação e sessão.

**Alternativa a avaliar:** Core e PS como camadas distintas no mesmo bounded context técnico (sem decidir implementação).

---

## Hipótese de módulos (H-MOD-001)

Módulos são **unidades funcionais implementáveis** derivadas de um ou mais Business Domains, habilitáveis por configuração/composição.

- **Não** há relação 1:1 obrigatória domínio → módulo.
- Módulos **transversais** são possíveis (Clinical Workspace, Patient Portal).

---

## Hipótese de extensão (H-EXT-001)

Três conceitos distintos — não intercambiáveis:

| Termo | Significado |
|---|---|
| **Extension Business Domain** | Domínio de negócio para modelo operacional específico (Diagnostic Operations, Home Care Operations) |
| **Extension Module** | Módulo opcional habilitado por tenant/instituição que implementa domínio Extension |
| **Extension Mechanism** | Mecanismo arquitetural do Core (hooks, registry, composição) — ADR-0003 |

---

## Hipótese de Read Models (H-VIEW-001)

Clinical Timeline e Analytics & Reporting são **projeções de leitura** — dados de ownership nos domínios fonte; a view agrega sem regras de negócio autônomas.

O prontuário é **visão** da timeline, não domínio.

---

## Módulos candidatos (conceitual — não definitivo)

| Módulo candidato | Domínio(s) primário(s) | Capability | Tipo |
|---|---|---|---|
| Scheduling Module | Care Coordination | Organizar | Core product |
| Clinical Workspace Module | Care Delivery + Clinical Record + … | Executar, Registrar | Transversal (shell) |
| Attendance Module | Care Delivery | Executar | Core product |
| Clinical Documentation Module | Clinical Record | Registrar | Core product |
| Orders Module | Clinical Orders | Registrar | Core product |
| Documents Module | Clinical Documents | Registrar | Core product |
| Patient Portal Module | Patient Relationship, Communication | Relacionar, Comunicar | Supporting |
| Telemedicine Module | Care Delivery | Executar | Extension-capable |
| Diagnostic Module | Diagnostic Operations | Executar | Extension |
| Home Care Module | Home Care Operations | Executar | Extension |
| Integration Admin Module | Integration | Integrar | Supporting |
| Monitoring Module | Care Monitoring, Operations Monitoring | Monitorar | Supporting |

*Catálogo preliminar — validar em `module-strategy-draft.md`.*

---

## Hipótese Clinical Workspace (H-WS-001)

Clinical Workspace é um **módulo shell transversal** — orquestra experiência do profissional carregando outros módulos conforme contexto, sem possuir regras de negócio clínicas próprias.

**Alternativa a avaliar:** camada acima de módulos (não módulo), parte do Core Platform UI shell.

---

## Faixa estimada de módulos

**Hipótese:** 8–15 módulos candidatos na fase inicial conceitual — menos que 16 domínios (agrupamento e transversais).

*A validar na NR-004.*

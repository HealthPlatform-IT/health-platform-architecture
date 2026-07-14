# Roles Must mínimos — AS-021

> Catálogo **mínimo** para deny-by-default (ADR-0020). Não substitui matriz completa.

## D-008 — Papéis iniciais (por tenant)

| Role ID | Uso MVP | Escopo típico |
|---|---|---|
| `platform_admin` | Operação da plataforma (cross-tenant controlado) | Platform |
| `org_admin` | Admin da organização / unidade | Organization |
| `clinician` | Atendimento e documentação clínica | Organization / Care |
| `receptionist` | Agenda / check-in operacional | Organization |
| `patient` | Portal (quando M-08 entrar; prep. contrato) | Self |

Permissions: semântica `resource:action` (ex.: `appointment:create`, `attendance:start`) — catálogo fino cresce com módulos Must.

**Regra:** ausência de grant = **deny**.

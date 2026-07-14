# Layers & Module ↔ Domain ↔ PS — Backend

> Rascunho AS-012 — NR-003. Candidato a confirmação — **não é ADR**.

---

## 1. Camadas lógicas (candidato)

```text
┌─────────────────────────────────────────────┐
│ Presentation / API ports                     │  ← API Strategy detalha contratos
├─────────────────────────────────────────────┤
│ Application (use cases / Module orchestration)│
├─────────────────────────────────────────────┤
│ Domain (Business Domain rules + Domain Events)│
├─────────────────────────────────────────────┤
│ Infrastructure adapters                      │
│  PS clients · persistence · Event Bus · files │
└─────────────────────────────────────────────┘
```

Direção de dependência: **API → Application → Domain**; Infrastructure implementa ports usados por Application/Domain.

---

## 2. Responsabilidades

| Camada | Faz | Não faz |
|---|---|---|
| **API** | AuthN handshake, Tenant no pipeline, validação de entrada, map DTO | Regra clínica |
| **Application / Module** | Orquestra fluxo de produto; coordena ≥1 domínio; chama PS | Persistência crua ad hoc; catálogo de eventos |
| **Domain** | Invariantes, estados, Domain Events (tipos) | HTTP, SQL, “enviar e-mail” direto |
| **Infrastructure** | Adapters PS, DB, bus, clock | Decidir política de negócio |

---

## 3. Module vs Domain no runtime

- **Module** = composição de casos de uso voltados ao usuário / workspace.
- **Domain** = ownership da verdade de negócio e dos **tipos** de Domain Event.
- Um use case de módulo **pode** atravessar >1 domínio via aplicação — sem fundir modelos.

Exemplo: Attendance Module inicia atendimento → Domain Care Delivery muda estado → publica `Attendance.Started` → Communication / Timeline reagem via bus.

---

## 4. Platform Services no backend

| Padrão | Quando |
|---|---|
| **In-process adapter** | PS leve / low-latency (Config, Feature Flag) no monólito |
| **Remote PS** | Identity federation, Event Bus broker, Storage externo |
| **Nunca** | Reimplementar Audit/Identity dentro do domínio clínico |

Contratos Core (Tenant Context, Event Foundation envelope) válidos **seja** in-proc ou remote.

---

## 5. Anti-padrões

- Domain importar controllers HTTP
- Module burlar Domain e escrever SQL
- PS com regra de Jornada de Cuidado
- “Shared kernel” inchado com entidades clínicas

---

## Próximo

**NR-004** — Runtime Context / multi-tenant no pipeline backend.

# NR-002 — Apps e Shell

> Rascunho AS-018. Candidato a confirmação.

## 1. Superfícies de aplicativo (MVP arquitetural)

| Superfície | Papel | Módulos típicos |
|---|---|---|
| **Staff App** | Profissional clínico / operacional | Shell **M-02** + M-01, M-03…M-07, modes |
| **Patient Portal** | Paciente / cuidador | **M-08** (Should no MVP) |
| **Admin surfaces** | Cadastro org/profissional, governança | **M-09**, **M-16**, Integration Admin etc. |

Admin pode ser área dentro do Staff App (navegação) **ou** app lógico separado — composição via mesmos contratos; **não** exige deploy separado.

## 2. Clinical Workspace (M-02)

```text
Staff App
  └── Clinical Workspace (shell)
        ├── Layout / navegação / contexto (patient, journey, attendance)
        ├── Slot: Scheduling (M-01)
        ├── Slot: Attendance (+ Telemedicine mode) (M-03)
        ├── Slot: Documentation / Orders / Documents (M-04…M-06)
        ├── Slot: Care Monitoring (M-07)
        ├── Consome Clinical Timeline (read model)
        └── Extension slots (M-14/M-15 se habilitados)
```

Shell = **orquestração de UI**, não dono de regra de Care Delivery.

## 3. Telemedicine

**Operational Mode** em Attendance (ADR-0009) → UI dentro do Staff App / M-03 — **não** app Telemedicine separado.

## 4. Cliente técnico

| Cliente | MVP |
|---|---|
| SPA web | **Padrão** |
| Mobile nativo / PWA avançado | Deferred |
| Microfrontends multi-repo | **Não** obrigatório |

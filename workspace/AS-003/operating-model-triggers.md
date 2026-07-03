# Matriz de Gatilhos por Modelo Operacional — AS-003

> Preenchida ao encerramento da sessão (2026-07-02). Fonte: `decisions.md` Decisão 004.

## Visão consolidada

| Modelo | Início | Mantém ativo | Pausa | Encerramento | Reabertura |
|---|---|---|---|---|---|
| Clínica ambulatorial | `ReferralAccepted` ou `ScheduledCareCommitted` | Episódios/atendimentos ativos, retornos | Programa suspenso, inatividade | Alta, transferência, desistência | Novo Start Event → nova Journey |
| Telemedicina | Igual ambulatorial | Igual ambulatorial | Igual ambulatorial | Igual ambulatorial | Igual ambulatorial |
| Laboratório | `ServiceRequestAccepted` | Processamento, laudo pendente | — | Resultado entregue | Novo pedido → nova Journey |
| Centro de imagem | `ServiceRequestAccepted` ou `ScheduledCareCommitted` | Exame agendado, laudo pendente | — | Laudo entregue | Novo pedido → nova Journey |
| Home care | `HomeCareAdmitted` | Visitas programadas, monitoramento | Suspensão do programa | Alta domiciliar, transferência | Novo Start Event → nova Journey |
| Hospital | `EmergencyCareAssumed` *(futuro)* | Internação ativa *(futuro)* | — *(futuro)* | Alta, óbito, transferência *(futuro)* | Nova admissão *(futuro)* |
| Medicina ocupacional | `OccupationalCareEnrolled` | PCMSO ativo, exames periódicos | Afastamento, suspensão vínculo | Demissão, conclusão programa | Novo vínculo → nova Journey |

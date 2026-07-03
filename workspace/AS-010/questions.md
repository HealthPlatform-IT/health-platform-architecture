# Perguntas — AS-010

## Q-003 — Event Model e Event Bus

1. O que é Event Foundation (Core) vs Event Bus (PS)?
2. O Event Bus deve ser **Confirmed** ou permanece Strong Candidate?
3. Onde vive o catálogo/taxonomia de tipos de evento de plataforma?
4. Como distinguir **Clinical Event** (ADR-0001) de **mensagem de evento** na bus?
5. Como distinguir **Care Journey Start Event** (ADR-0007) de evento na bus?
6. Quem publica — domínio, módulo ou PS?
7. Quem consome — módulos, Read Models, Audit, Communication?
8. Qual a fronteira com **Webhook Service** (externo)?
9. Qual a fronteira com **Integration Service** (sistemas externos)?
10. Eventos disparam Communication/Notification ou domínio orquestra?
11. O que fica para Sprint 3 (tecnologia)?
12. Q-003 será **Answered** completo ou **Partial** (conceitual + tech deferred)?

## OQ-EV01 — Catálogo de eventos

O catálogo de tipos de evento pertence ao Core, ao Event Bus ou aos domínios?

*Hipótese inicial:* domínios definem tipos; Foundation define envelope; Bus transporta — **a validar**.

## OQ-EV02 — Tier Event Bus

Event Bus promove a **Confirmed** nesta sessão?

*Hipótese inicial:* sim, se fronteira estiver madura como MFE/DE — **a validar**.

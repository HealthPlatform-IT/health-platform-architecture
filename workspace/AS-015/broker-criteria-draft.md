# Broker Criteria

> Rascunho AS-015 — NR-003.

---

## 1. Classe de solução

Broker de mensagens / log de eventos com:

- pub/sub ou consumer groups
- persistência de mensagens adequada a at-least-once
- suporte a múltiplos consumidores independentes
- ops SaaS (monitoramento, retenção configurável)

**Não** usar banco da aplicação como bus principal (fora do Outbox transitório).

---

## 2. Critérios de escolha (produto futuro)

O produto deve preferir satisfazer:

1. At-least-once + acknowledgements claros  
2. Consumer groups / competing consumers  
3. Particionamento ou equivalente para ordering por chave  
4. Retenção configurável e DLQ (nativo ou padrão)  
5. Multi-tenant ops (isolamento lógico; métricas por tenant quando possível)  
6. Ecossistema alinhável ao modular monolith / stack (ADR-0014 Deferred)  
7. Maturidade operacional (backup, upgrade, security)

---

## 3. Produto concreto

**Deferred** (PoC pós-critérios) — igual padrão ADR-0015 (vendor DB).

Não fechar Kafka vs RabbitMQ vs cloud queue nesta sessão.

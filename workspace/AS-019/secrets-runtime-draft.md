# NR-004 — Secrets runtime e critérios

> Rascunho AS-019. Candidato a confirmação.

## 1. Confirma S-01…S-04 (ADR-0020)

| ID | Aplicação DevOps |
|---|---|
| S-01 | Secrets fora de git / ADRs / docs |
| S-02 | Injetados no runtime por ambiente |
| S-03 | Cofre/KMS/rotação = **critérios agora**; produto Deferred |
| S-04 | Env vars locais ≠ política de produção |

## 2. Critérios de vault/KMS (produto Deferred)

1. Isolamento por ambiente
2. Rotação e auditoria de acesso a secrets
3. Integração com pipeline sem imprimir secret
4. Separação de papéis (quem gera ≠ quem usa em prod)

## 3. Critérios cloud / orquestração (Deferred)

1. Isolamento de rede e dados alinhado multi-tenant
2. Observabilidade exportável
3. Custo/operacionalidade do modular monolith
4. Não forçar microsserviços

## 4. Fora desta sessão

Lista de vendors · Terraform modules · runbooks detalhados · Q-019.

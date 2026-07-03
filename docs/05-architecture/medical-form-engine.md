---
title: Medical Form Engine
status: Draft
version: 0.1.0
created: 2026-07-03
updated: 2026-07-03
author: Architecture Team
category: Architecture
phase: Product & Architecture Foundation
related:
  - docs/05-architecture/platform-services.md
  - docs/05-architecture/module-strategy.md
  - docs/04-domain/business-domains.md
  - docs/05-architecture/adr/foundation/ADR-0010-medical-form-engine.md
  - docs/05-architecture/adr/foundation/ADR-0011-document-engine.md
  - architecture-sessions/AS-006-medical-form-engine.md
---

# Medical Form Engine

> Platform Service de **captura estruturada** — formulários clínicos dinâmicos, configuráveis e reutilizáveis.

Decisão formal: [ADR-0010](adr/foundation/ADR-0010-medical-form-engine.md) · AS-006 D-001 a D-008.

---

## 1. Purpose

Permitir que a plataforma defina, versione, configure e utilize formulários clínicos sem acoplar regras de domínio ao Core ou duplicar mecanismos em módulos.

**Pergunta respondida:**

> O Engine estrutura e captura; domínios possuem significado clínico e persistência.

---

## 2. Classificação

| Campo | Valor |
|---|---|
| Tipo | Platform Service |
| Tier | **Confirmed** (promovido AS-006) |
| Capability primária | Registrar |
| Capability secundária | Executar |

**Não é:** Business Domain · Module · Read Model · Core Platform · prontuário.

---

## 3. Três artefatos

| Artefato | Owner | Descrição |
|---|---|---|
| **Form Definition** | Medical Form Engine | Schema versionado: seções, campos, tipos, metadados |
| **Form Instance** | Engine (runtime) | Preenchimento em contexto — sem ownership clínico |
| **Clinical Content** | Business Domain destino | Conhecimento clínico após aceite do domínio |

---

## 4. Fluxo conceitual

```text
1. Módulo solicita render (context + formDefinitionId)
2. Engine retorna estrutura + validação estrutural
3. Profissional preenche → Form Instance
4. Submit → Engine valida estrutura
5. Payload → domínio destino
6. Domínio valida conteúdo clínico + persiste
7. Audit registra rastreabilidade
```

---

## 5. Responsabilidades

### Pertence ao Engine

- Registry de form definitions
- Composição estrutural (seções, campos)
- Versionamento de definition (imutável pós-publicação)
- Validação estrutural (tipo, formato, obrigatoriedade de campo)
- Contratos conceituais de render e submit
- Coleta estruturada de respostas

### Não pertence

| Item | Owner |
|---|---|
| Significado clínico, diagnóstico | Clinical Record |
| Prescrição, ciclo de ordem | Clinical Orders |
| Artefato formal assinável | Clinical Documents + Document Engine |
| Fluxo de atendimento | M-03 + Care Delivery |
| Shell do profissional | M-02 Clinical Workspace |
| Template mensagem/e-mail | Template Service |
| Política de acesso | Authorization Service |
| APIs, schema técnico, UI | Fase técnica futura |

---

## 6. Validação

| Tipo | Owner | Exemplos |
|---|---|---|
| Estrutural | Engine | Tipo de campo, formato data, campo obrigatório |
| Clínica | Domínio destino | CID válido, protocolo, dose permitida |
| Autorização | Authorization Service | Quem pode preencher/ver |

---

## 7. Configuração (ADR-0006)

Variação por tenant / instituição / unidade / especialidade via **Configuration Service** + metadados de definition.

**Proibido:** customização por código cliente ou fork de Engine por tenant.

---

## 8. Consumidores (módulos)

| Módulo | Uso | Submit primário |
|---|---|---|
| M-03 Attendance | Captura durante atendimento | Clinical Record |
| M-04 Clinical Documentation | Registro estruturado | Clinical Record |
| M-05 Orders | Captura de solicitação/ordem | Clinical Orders |
| M-14 Diagnostic | Formulários extension | Clinical Record + Diagnostic |
| M-15 Home Care | Formulários domiciliares | Clinical Record + Home Care |

---

## 9. Relação com domínios

| Domínio | Relação |
|---|---|
| **Care Delivery** | Contexto de Attendance — quando capturar |
| **Clinical Record** | Destino primário de conteúdo clínico |
| **Clinical Orders** | Destino de formulários de ordem/solicitação |
| **Clinical Documents** | Downstream — dados podem alimentar artefatos formais |

---

## 10. Fronteiras com PS

| PS | Fronteira |
|---|---|
| **Document Engine** | Captura vs geração de documento formal — ADR-0011 |
| **Template Service** | Estrutura de formulário vs template textual — independente (Q-014 Answered) |
| **Configuration** | Quais definitions habilitadas |
| **File Service** | Anexos em campos |

---

## 11. Exemplos de formulário (conceitual)

Anamnese · evolução clínica · exame físico · escalas · questionários · triagem · avaliação terapêutica · teleconsulta · home care · procedimento · solicitação · consentimento clínico · protocolo assistencial *(regra no domínio; Engine só estrutura de checklist se configurado)*.

---

## 12. Anti-patterns

| Anti-pattern | Correção |
|---|---|
| Engine armazena evoluções | Clinical Record |
| Regra clínica só no schema | Domínio valida no submit |
| Formulário vira PDF | Document Engine |
| Motor por módulo | Medical Form Engine único |

---

## 13. Fora de escopo

Código, APIs, schema JSON, UI React, bibliotecas, persistência física, Event Model detalhado (Q-003).

---

## 14. Documentos relacionados

- [platform-services.md](platform-services.md)
- [module-strategy.md](module-strategy.md)
- [architecture-classification.md](architecture-classification.md)

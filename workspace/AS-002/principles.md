# Princípios — AS-002 Workspace

> Princípios arquiteturais consolidados durante a AS-002.
> Arquivo de workspace — sessão encerrada em 2026-07-02.
> Documentação oficial derivada: ADRs foundation e docs em `docs/03-capabilities/`.

## Status

Arquivo de arquivo / workspace. Não substitui ADRs nem `docs/00-introduction/principles.md`.

---

## Princípios consolidados na AS-002

### 1. Orientação por capacidades de negócio

A plataforma deve ser organizada por **Business Capabilities** — o que a organização precisa fazer — e não por módulos, telas ou features isoladas.

**Formalização:** [ADR-0002](../../docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md)

### 2. Capabilities antes de módulos

Novos módulos devem derivar de capacidades e domínios de negócio identificados. Não se cria módulo apenas porque uma tela ou feature foi solicitada.

**Formalização:** [ADR-0002](../../docs/05-architecture/adr/foundation/ADR-0002-capability-driven-architecture.md)

### 3. Jornada de Cuidado como centro conceitual

A plataforma é organizada em torno da **Jornada de Cuidado do Paciente** — fluxo contínuo de relacionamento, organização, execução, registro, compartilhamento, monitoramento e governança do cuidado.

Atendimentos, exames, prescrições e comunicações são eventos ou artefatos associados à jornada, não o centro da arquitetura.

**Fonte workspace:** [decisions.md](./decisions.md) — Decisão 005  
**Formalização complementar:** [ADR-0001](../../docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md)

### 4. Core pequeno e protegido

O Core da plataforma deve permanecer pequeno, estável e protegido contra regras específicas de clientes, módulos ou fluxos clínicos.

**Fonte workspace:** [decisions.md](./decisions.md) — Decisão 004  
**Formalização:** [ADR-0003](../../docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md)

### 5. Crescimento por extensão

A plataforma evolui prioritariamente por extensão — módulos, serviços compartilhados, configurações, eventos e integrações — e não por alteração estrutural do Core.

**Fonte workspace:** [decisions.md](./decisions.md) — Decisão 004  
**Formalização:** [ADR-0003](../../docs/05-architecture/adr/foundation/ADR-0003-core-protection-and-extension-model.md)

### 6. Configuração acima de customização

Diferenças entre clientes e modelos operacionais devem ser tratadas por configuração sempre que possível. Customizações por cliente são exceção, não padrão.

**Formalização:** [ADR-0006](../../docs/05-architecture/adr/foundation/ADR-0006-configuration-over-customization.md)

### 7. Platform Services para transversal

Funcionalidades reutilizáveis por múltiplos domínios ou módulos devem ser avaliadas como **Platform Services**, com contratos bem definidos e responsabilidades transversais centralizadas.

**Fonte workspace:** [decisions.md](./decisions.md) — Decisões 001 e 003  
**Formalização:** [ADR-0005](../../docs/05-architecture/adr/foundation/ADR-0005-platform-services.md)

### 8. Comunicar ≠ Integrar

Comunicação com pessoas (e-mail, WhatsApp, SMS, push) e integração entre sistemas são responsabilidades distintas. Nenhum módulo envia mensagens diretamente por canais externos.

**Fonte workspace:** [decisions.md](./decisions.md) — Decisão 002  
**Formalização:** [ADR-0004](../../docs/05-architecture/adr/foundation/ADR-0004-communication-and-integration-separation.md)

### 9. Documentação antes de implementação

Decisões arquiteturais devem ser registradas e formalizadas em ADRs e documentação oficial antes de iniciar implementação.

**Fonte:** metodologia do projeto — Architecture Session → Workspace → Decision → ADR → Official Documentation

---

## Princípio relacionado (fora do escopo direto da AS-002)

### Modelo Hierárquico do Cuidado

O cuidado é representado hierarquicamente: Paciente → Jornada de Cuidado → Episódio → Atendimento → Evento Clínico → Artefato Clínico.

Consolidado em sessão anterior/adjacente; referenciado pela AS-002 como fundação do modelo de cuidado.

**Formalização:** [ADR-0001](../../docs/05-architecture/adr/foundation/ADR-0001-hierarchical-care-model.md)

---

## Onde cada princípio foi formalizado

| Princípio | Fonte workspace | Formalização |
|---|---|---|
| Orientação por capacidades | Sessão AS-002 | ADR-0002 |
| Capabilities antes de módulos | Sessão AS-002 | ADR-0002 |
| Jornada de Cuidado como centro | Decisão 005 | ADR-0001 (complementar) |
| Core pequeno e protegido | Decisão 004 | ADR-0003 |
| Crescimento por extensão | Decisão 004 | ADR-0003 |
| Configuração acima de customização | Emergente na sessão | ADR-0006 |
| Platform Services para transversal | Decisões 001, 003 | ADR-0005 |
| Comunicar ≠ Integrar | Decisão 002 | ADR-0004 |
| Documentação antes de implementação | Metodologia | — |
| Modelo Hierárquico do Cuidado | Sessão adjacente | ADR-0001 |

---

## Próximo passo documental

- Documento oficial de princípios: `docs/00-introduction/principles.md` (Onda 1.4 — pendente)

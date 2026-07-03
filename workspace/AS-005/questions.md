# Perguntas — AS-005

> Q-007 é a pergunta central. Respostas definitivas vão para documentação oficial após confirmação.

---

## Q-007 — Fronteira Core / Platform Services / Modules

**Status:** In Analysis  
**Fonte:** `ai-context/open-questions.md` (Partial desde AS-004)

### Pergunta-guia

> Qual é a fronteira exata entre Core Platform, Platform Services, Business Domains e Modules?

### Subperguntas — Core Platform

1. O que é **Core Platform** — e como se diferencia do "Core" do ADR-0003?
2. Qual é o **tamanho aceitável** do Core?
3. O que **nunca** deve entrar no Core?
4. Identidade, autorização e auditoria ficam no Core ou nos Platform Services? *(T-001)*
5. O Modelo Hierárquico do Cuidado pertence ao Core?
6. O Core define **contratos** e os PS **implementam**?

### Subperguntas — Platform Services

7. Quando uma responsabilidade vira PS em vez de Core ou módulo?
8. Como o domínio Communication coexistiria com Communication/Notification Services?
9. Compliance Service (Q-019) — PS, extensão do Audit ou indefinido?
10. Document Engine e Medical Form Engine — PS consumidos por módulos ou parte de módulos?

### Subperguntas — Business Domains

11. Um domínio pode ser implementado por zero módulos na fase inicial?
12. Extension Domains (Diagnostic, Home Care) são obrigatoriamente módulos separados?

### Subperguntas — Modules

13. Quando algo deve virar **módulo**?
14. Qual a cardinalidade **domínio → módulo** (1:1, 1:N, N:1)?
15. **Clinical Workspace** — módulo, shell ou camada acima de módulos? *(T-003)*
16. Módulos podem pertencer a múltiplos domínios?
17. Como módulos consomem PS sem acessar dados internos de outros domínios?

### Subperguntas — Extensions

18. Quando algo deve virar **extensão** em vez de módulo core do produto?
19. Como diferenciar Extension Business Domain, módulo extension e mecanismo Extension? *(T-002)*
20. Extensões são habilitadas por Configuration, Feature Flag, composição ou todos?

### Subperguntas — Read Models

21. Como Clinical Timeline e Analytics são produzidos e consumidos?
22. Quem é o owner dos dados — domínios fonte ou a view?
23. Read Models podem ter lógica de apresentação sem virar domínio?

### Subperguntas — Proteção e acoplamento

24. Como evitar customização por cliente disfarçada de extensão?
25. Como preservar baixo acoplamento entre módulos?
26. Orquestração cross-domain via eventos (conceitual) — permitida?
27. Como Configuration over Customization (ADR-0006) se aplica à composição de módulos?

---

## Relacionadas (parcial ou influência nesta sessão)

| ID | Pergunta | Relação |
|---|---|---|
| Q-004 | Agregados DDD | Fora de escopo — não bloqueia fronteiras |
| Q-005 | Capabilities administrativas/financeiras | Domínios deferred — não bloqueia |
| Q-009 | Mecanismos de extensão na implementação | Parcial conceitual |
| Q-015 | Herança de configuração | Influencia composição de módulos |
| Q-016 | Processo de customização excepcional | Influencia extensões |
| Q-019 | Compliance Service no catálogo PS | Fronteira PS |

---

## Novas perguntas emergentes

_(Adicionar aqui durante a investigação; promover para `open-questions.md` com Q-021+)_

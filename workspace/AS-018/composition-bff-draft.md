# NR-003 — Composição (OQ-C03) e BFF

> Rascunho AS-018. Candidato a confirmação.

## 1. UI Composition Contract (OQ-C03 → Answered candidato)

**Core** expõe contrato mínimo via **Module Registry** (não componentes de UI):

| Elemento | Descrição |
|---|---|
| **Module contribution** | Módulo declara rotas / áreas / slots que oferece ao shell |
| **Shell host** | M-02 resolve contribuições habilitadas no tenant |
| **Enablement** | Feature Flag + Module Registry por tenant |
| **Boundary** | Contribuição não altera Core nem outro módulo interno |

Core **não** embute framework UI, design system nem HTML. Slot = contrato de composição, não React.

**OQ-C03 Answered (candidato):** slot/contribution mínimo no Core via Module Registry; shell no módulo M-02.

## 2. BFF

| Opção | Papel |
|---|---|
| **Product API** | Contrato canônico (ADR-0016) |
| **Thin BFF** | **Opcional** — agregação/adaptação para Staff SPA (menos round-trips) |
| **GraphQL** | Deferred |

BFF **não** substitui Product API como fonte de verdade; **não** concentra regra de domínio; AuthZ permanece no backend.

## 3. Estado no cliente

- Shell: navegação, contexto de trabalho (paciente/jornada selecionados) — referências, não cópia de agregado
- Módulo: estado de tela próprio
- Proibido: cache cross-módulo de invariantes de domínio como “fonte da verdade”

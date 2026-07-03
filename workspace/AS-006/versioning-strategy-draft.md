# Versioning Strategy Draft — Medical Form Engine

> **Promovido para documentação oficial:** `docs/05-architecture/medical-form-engine.md` (ADR-0010 D-004).  
> Registro histórico NR-005.

**Última atualização:** 2026-07-03

---

## 1. Propósito

Definir conceitualmente como **versionar form definitions** sem quebrar **histórico clínico** nem confundir definition com conteúdo clínico persistido.

---

## 2. O que é versionado

| Entidade | Versionada? | Owner | Nota |
|---|---|---|---|
| **Form Definition** | **Sim** | Medical Form Engine | v1, v2, v3… imutáveis após publicação |
| **Form Instance** | Referência fixa | Engine (runtime) | Aponta para definitionId + version no momento da criação |
| **Clinical Content** | Imutável após aceite | Business Domain | Registro clínico não muda quando definition evolui |

---

## 3. Princípios

1. **Definition publicada é imutável** — alteração cria nova versão.
2. **Instância retém referência** à versão usada no preenchimento.
3. **Conteúdo clínico aceito** não é reescrito quando definition v2 é publicada.
4. **Compatibilidade retroativa** — instâncias antigas permanecem legíveis.
5. **Versionamento de definition ≠ versionamento de prontuário** — domínios versionam conteúdo por suas próprias regras.

---

## 4. Ciclo de vida conceitual da Form Definition

```text
Draft → Published (v1) → [alteração] → Published (v2) → Deprecated
```

| Estado | Comportamento |
|---|---|
| **Draft** | Editável; não disponível para produção |
| **Published** | Imutável; disponível para novas instâncias |
| **Deprecated** | Não para novas instâncias; legado ainda legível |

---

## 5. Cenários

### Cenário 1 — Nova versão de anamnese

- v1 publicada; 1000 atendimentos usaram v1
- v2 publicada com campo adicional
- Novos atendimentos usam v2 por padrão (Configuration)
- Registros clínicos de v1 **inalterados** — Clinical Record mantém payload v1

### Cenário 2 — Correção estrutural

- Erro de tipo em campo → nova versão v1.1 ou v2
- Instâncias em draft podem migrar; instâncias submitted **não** retroagem

### Cenário 3 — Descontinuação

- Definition deprecated — não aparece para novos formulários
- Histórico permanece consultável

---

## 6. O que o Engine NÃO versiona

| Item | Owner |
|---|---|
| Evolução clínica do paciente | Clinical Record |
| Ordem clínica emitida | Clinical Orders |
| Documento formal assinado | Clinical Documents |
| Linha na timeline | Read Model |

---

## 7. Riscos

| ID | Risco | Mitigação |
|---|---|---|
| R-MFE-08 | Alterar definition apaga histórico | Imutabilidade pós-publicação + referência em instância |
| R-MFE-08b | Migrar conteúdo clínico ao publicar v2 | Proibido — domínio é fonte de verdade |

---

## 8. Decisão candidata

**D-005 (rascunho):** Form Definition versionada e imutável após publicação; Form Instance referencia version; Clinical Content independente do ciclo de definition.

**Status:** Ready for Confirmation

**Fase técnica:** formato de versionamento (semver, incremental), migração de drafts — fora de escopo.

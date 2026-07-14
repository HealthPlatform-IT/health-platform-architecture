# Ownership — Domain vs Storage / File

> Rascunho AS-013 — NR-003. Candidato a confirmação.

---

## 1. Regra

| Tipo de dado | Ownership | Persistência |
|---|---|---|
| Estado de negócio estruturado | **Business Domain** | Store relacional (ou documento tipado) do domínio |
| Blob / objeto binário | **Storage Service (PS)** | Object storage no escopo do tenant |
| Metadados de arquivo clínico | **File Service (PS)** | Metadados + referência ao Storage |
| Configuração tipada | **Configuration Service** | Store do PS — schema detalhado **Q-017 Deferred** |
| Trilha de auditoria | **Audit Service** | Store do PS (tenant no registro) |

Domínio **não** grava blob cru contornando Storage. Domínio guarda **referências** (IDs/URIs internas) quando precisa associar artefato.

---

## 2. Fronteira Module

Module orquestra use cases; **não** é dono do modelo de dados persistido do domínio.

---

## 3. Clinical Artifact / Documents

- Conteúdo estruturado / formulário → domínio + Medical Form Engine conforme ADRs engines
- Documento formal gerado → Document Engine + Storage/File conforme ADR-0011
- Persistência “fonte da verdade” do fato clínico permanece no **domínio** responsável

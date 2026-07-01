# Business Capability Map

> O Business Capability Map representa a visão de mais alto nível da Health Platform. Seu objetivo é identificar **o que a plataforma precisa ser capaz de realizar**, independentemente de tecnologias, módulos ou telas.

---

# Objetivo

Definir as capacidades fundamentais da Health Platform que servirão como base para toda a arquitetura do produto.

As capacidades representam habilidades permanentes da plataforma. Elas permanecem estáveis ao longo do tempo, enquanto módulos, funcionalidades e tecnologias podem evoluir.

Este mapa será utilizado como referência para descoberta dos domínios de negócio, organização dos módulos, definição das APIs, modelagem do banco de dados e evolução futura da plataforma.

---

# O que é uma Business Capability?

Uma Business Capability representa uma competência que a plataforma deve possuir para entregar valor ao negócio.

Ela descreve **o que** a plataforma precisa fazer, e não **como** isso será implementado.

Por esse motivo, uma capability não representa um módulo, uma tela, um serviço ou uma tecnologia.

Uma única capability pode originar diversos domínios, módulos e funcionalidades ao longo da evolução da plataforma.

---

# Estrutura Geral

A Health Platform será organizada inicialmente em seis grandes grupos de capacidades.

```text
Health Platform

├── Platform Capabilities
├── Clinical Capabilities
├── Operational Capabilities
├── Administrative Capabilities
├── Intelligence Capabilities
└── Integration Capabilities
```

Cada grupo representa um conjunto de responsabilidades específicas dentro da plataforma.

---

# 1. Platform Capabilities

Representa as capacidades responsáveis por sustentar toda a infraestrutura da plataforma.

Essas capacidades não possuem conhecimento clínico e devem permanecer desacopladas das regras de negócio relacionadas à assistência em saúde.

Exemplos de capacidades deste grupo:

* Multi-Tenant
* Gestão de Usuários
* Autenticação
* Autorização
* Configurações
* Auditoria
* Observabilidade
* Eventos
* Notificações
* Armazenamento de Arquivos
* Busca Global
* Feature Flags

Estas capacidades constituem a fundação técnica da plataforma.

---

# 2. Clinical Capabilities

Representa o núcleo assistencial da Health Platform.

É responsável por todas as capacidades relacionadas ao cuidado do paciente e ao trabalho dos profissionais de saúde.

Exemplos de capacidades deste grupo:

* Jornada Clínica do Paciente
* Workspace Clínico
* Documentação Clínica
* Gestão de Medicamentos
* Gestão de Exames
* Gestão de Laudos
* Telemedicina
* Suporte à Decisão Clínica

Este grupo representa o principal diferencial da plataforma.

---

# 3. Operational Capabilities

Representa as capacidades relacionadas à operação diária das instituições de saúde.

São responsáveis por organizar e controlar os fluxos operacionais necessários para o funcionamento da clínica.

Exemplos:

* Agendamento
* Recepção
* Atendimento
* Filas
* Comunicação
* Portal do Paciente
* Workflow Assistencial

Essas capacidades conectam pacientes, profissionais e operação clínica.

---

# 4. Administrative Capabilities

Representa as capacidades responsáveis pela gestão administrativa das instituições.

Exemplos:

* Organizações
* Clínicas
* Unidades
* Profissionais
* Convênios
* Financeiro
* Faturamento
* Estoque
* Relatórios Gerenciais

Essas capacidades garantem o funcionamento administrativo da plataforma.

---

# 5. Intelligence Capabilities

Representa as capacidades voltadas à análise de dados, automação e inteligência da plataforma.

Este grupo foi concebido desde o início da arquitetura para permitir evolução contínua sem necessidade de mudanças estruturais.

Exemplos:

* Inteligência Artificial
* Analytics
* Indicadores
* Dashboards
* Recomendações Clínicas
* Automação de Processos
* Modelos Preditivos

A expectativa é que este grupo cresça significativamente ao longo da evolução da plataforma.

---

# 6. Integration Capabilities

Representa todas as capacidades responsáveis pela comunicação da plataforma com sistemas externos.

Exemplos:

* FHIR
* TISS
* PACS
* DICOM
* Laboratórios
* Gateways de Pagamento
* WhatsApp
* E-mail
* SMS
* APIs Externas
* Dispositivos Médicos
* IoT

Nenhuma integração deverá depender diretamente dos módulos clínicos. Toda comunicação deverá ocorrer através de contratos bem definidos.

---

# Princípios Arquiteturais

O Business Capability Map segue os seguintes princípios:

* Capabilities representam competências de negócio, nunca funcionalidades.
* Uma capability pode originar diversos domínios.
* Um domínio pode conter diversos módulos.
* Módulos podem evoluir sem alterar as capabilities.
* As capabilities devem permanecer estáveis ao longo do tempo.
* A arquitetura deve evoluir preservando este mapa.

---

# Relação com os Demais Artefatos

O Business Capability Map será utilizado como ponto de partida para os próximos níveis da arquitetura.

A sequência de evolução da plataforma será:

```text
Business Capabilities
        ↓
Business Domains
        ↓
Modules
        ↓
Features
        ↓
Components
        ↓
Screens
```

Cada etapa detalhará a anterior, mantendo a consistência arquitetural da plataforma.

---

# Considerações Finais

O Business Capability Map representa a visão mais abstrata da Health Platform.

Ele não descreve funcionalidades específicas nem decisões de implementação.

Seu propósito é estabelecer uma base conceitual sólida que permita à plataforma evoluir continuamente, preservando sua organização, modularidade e capacidade de adaptação às futuras demandas do setor da saúde.

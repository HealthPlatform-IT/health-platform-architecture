## Decisão 001

Toda Platform Capability poderá expor um ou mais Platform Services reutilizáveis.

Os Platform Services representam capacidades compartilhadas da plataforma e não pertencem a um domínio específico.

Eles devem ser consumidos por qualquer módulo através de contratos bem definidos, evitando acoplamento direto entre módulos e centralizando responsabilidades transversais, como auditoria, identidade, notificações, armazenamento e busca.

## Decisão 002

A Health Platform deverá possuir uma capacidade centralizada de comunicação.

Nenhum módulo da plataforma deverá enviar mensagens diretamente por e-mail, WhatsApp, SMS, push notification ou outros canais.

Todos os módulos deverão solicitar comunicações através de um Communication Service, responsável por centralizar templates, provedores, filas, tentativas de reenvio, logs de entrega, preferências do usuário, consentimentos e regras de comunicação.

Essa decisão reduz acoplamento, facilita a troca de provedores externos e permite que novos canais de comunicação sejam adicionados no futuro sem alterar os módulos consumidores.

## Decisão 003

A Health Platform adotará o princípio "dividir para conquistar, compartilhar para escalar".

Funcionalidades específicas de um domínio permanecerão dentro do domínio correspondente.

Funcionalidades reutilizáveis por múltiplos domínios ou módulos deverão ser avaliadas como Platform Services.

Platform Services devem expor contratos bem definidos e centralizar capacidades transversais da plataforma, como comunicação, auditoria, armazenamento, busca, notificações, configurações, eventos e permissões.

Essa decisão evita duplicação de código, reduz acoplamento, aumenta consistência entre módulos e facilita a evolução futura da plataforma.


## Decisão 004

A Health Platform deverá evoluir prioritariamente por extensão, e não por alteração do Core.

O Core da plataforma deverá permanecer estável, pequeno e protegido contra regras específicas de módulos, clientes ou fluxos clínicos.

Novas funcionalidades deverão ser adicionadas por meio de módulos, serviços compartilhados, configurações, eventos ou integrações, evitando mudanças estruturais no núcleo da plataforma.

Essa decisão tem como objetivo reduzir riscos futuros, preservar a escalabilidade da arquitetura e permitir que a plataforma evolua continuamente sem comprometer sua base principal.

## Decisão 005

A Health Platform será organizada em torno da Jornada de Cuidado do Paciente.

A Jornada de Cuidado representa o fluxo contínuo de relacionamento, organização, execução, registro, compartilhamento, monitoramento e governança do cuidado.

Atendimentos, exames, prescrições, laudos, documentos e comunicações serão tratados como eventos ou artefatos associados a essa jornada.

Essa decisão evita que a plataforma seja centrada apenas em consultas ou prontuários, permitindo representar cuidado contínuo, histórico clínico, acompanhamento e evolução do paciente ao longo do tempo.
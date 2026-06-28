🛠️ Engenharia de Software e Padrões de Projeto
1. Gerenciamento de Conexões (Padrão Singleton)
Prevenção contra o esgotamento do Connection Pool e picos de RAM em WebSockets:

Prisma Client: Instanciado em uma única conexão global.

WhatsApp Gateway (Baileys): Túnel de comunicação reaproveitado em toda a aplicação, barrando múltiplas autenticações simultâneas.

2. Orquestração e Autocura (Self-Healing)
A aplicação opera de forma autônoma e resiliente via PM2:

Zero Downtime: Em caso de falha de rede ou queda de thread, o Kernel é reiniciado instantaneamente em milissegundos, mantendo o sistema da clínica online.

3. Estado e Prevenção de Vazamentos (Redis)
Controle rigoroso do estado das conversas hospedado na memória RAM (Fast-Path):

Cache Prevention: Invalidação cirúrgica de estados anteriores a cada interação do usuário, prevenindo respostas desatualizadas (Stale Data).

Watchdog (Short Polling): Motor de Garbage Collection operando a cada 5 segundos. Sessões abandonadas no meio do fluxo são destruídas, prevenindo Memory Leaks.

4. Segurança e Resiliência
Infraestrutura blindada para exposição segura ao ambiente público:

Helmet.js: Ocultação da stack Node.js e proteção de cabeçalhos HTTP.

Rate Limiting: Bloqueio ativo contra ataques DDoS e Força Bruta.

Sanitização de Inputs: Filtros Regex rigorosos nas mensagens recebidas para neutralizar injeções de código (XSS/SQLi).

5. Motor de Decisão: Lógica Determinística
O sistema não utiliza Inteligência Artificial para responder aos clientes, priorizando a segurança do paciente:

100% de Precisão: Eliminação de "alucinações" de IA no agendamento médico.

Controle Absoluto: O roteamento e o processamento de linguagem operam exclusivamente via fluxos lógicos pré-programados e Fuzzy Matching.

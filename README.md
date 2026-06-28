# calixto-omnisystem-architecture

🛠️ Engenharia de Software e Padrões de Projeto
1. Padrão Singleton (Gerenciamento de Conexões)
Em aplicações baseadas em WebSockets, o vazamento de conexões é o caminho mais rápido para derrubar um servidor. Para evitar o esgotamento do Connection Pool do banco de dados e picos de RAM, o Calixto OmniSystem aplica estritamente o Design Pattern Singleton.

Prisma Client: Instanciado uma única vez globalmente.

WhatsApp Gateway (Baileys): O túnel de comunicação é único e reaproveitado por todos os módulos da aplicação, evitando múltiplas autenticações simultâneas.

2. Infraestrutura e Autocura (Self-Healing)
A aplicação não depende de execuções manuais. Em produção, o sistema é orquestrado pelo PM2 (Process Manager).

Self-Healing: Caso ocorra uma queda abrupta de rede ou um erro fatal de thread, o PM2 reinicia o Kernel automaticamente em milissegundos, garantindo que o SaaS mantenha uma altíssima taxa de Uptime e zero Downtime percebido pelas clínicas.

3. Gerenciamento de Estado e Prevenção de Vazamento (Memory Leaks)
Como o estado das conversas roda em memória RAM (Redis) para garantir baixa latência (Fast-Path), criei duas travas de segurança:

Cache Prevention: A cada nova interação do usuário, o estado anterior é cirurgicamente invalidado. Isso previne o Stale Data (dados velhos), garantindo que o bot nunca fique preso em um loop ou retorne respostas desatualizadas.

Watchdog de Memória (Short Polling): Um serviço de Garbage Collection operando em background. A cada 5 segundos, ele executa um Short Polling no Redis. Se identificar que um usuário abandonou a conversa no meio do fluxo, o Watchdog "mata" a sessão ociosa, devolvendo a memória RAM para o servidor.

4. Segurança e Resiliência
Para expor a aplicação em um ambiente público de forma segura, a infraestrutura conta com middlewares de proteção pesada:

Helmet.js: Blindagem de cabeçalhos HTTP, ocultando a stack tecnológica de varreduras maliciosas.

Rate Limiting: Trava contra ataques de Força Bruta e DDoS na camada de rede.

Sanitização Rigorosa: Todos os inputs vindos do WhatsApp passam por limpadores de strings e regex para impedir ataques de injeção (XSS/SQLi) antes de tocarem na lógica de negócios.

5. Decisão de Negócio: Lógica Determinística vs. IA Generativa
Uma das decisões arquiteturais mais importantes deste projeto foi não utilizar Inteligência Artificial Generativa (LLMs) para formular as respostas aos pacientes. O ecossistema de saúde e agendamentos médicos exige previsibilidade absoluta e não tolera "alucinações" de IA.
Por isso, todo o sistema de PNL (Processamento de Linguagem Natural) foi construído em cima de lógica determinística, fluxos estruturados e Fuzzy Matching, garantindo 100% de precisão e controle sobre a jornada do cliente.

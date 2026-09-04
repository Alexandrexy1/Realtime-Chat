# Realtime Chat

Projeto de chat em tempo real desenvolvido com **Java, Spring Boot, React e TypeScript**.

A proposta do projeto é construir, de forma incremental, uma aplicação inspirada em ferramentas como Discord e Slack, começando pelo menor experimento possível e evoluindo conforme novas necessidades técnicas surgirem.

O objetivo principal não é apenas finalizar um chat funcional, mas usar o projeto para aprofundar conhecimentos de backend e arquitetura de sistemas em tempo real.

---

## Objetivos de aprendizado

Durante o desenvolvimento, o projeto será usado para estudar e aplicar conceitos como:

* WebSocket
* Diferenças entre HTTP e WebSocket
* Lifecycle de conexões
* Sessões WebSocket
* Broadcast
* Comunicação privada
* Autenticação de conexões
* Spring Security
* Persistência de mensagens
* Concorrência
* Ordenação de eventos
* Reconexão
* Presença online/offline
* Eventos efêmeros
* Redis Pub/Sub
* Escalabilidade horizontal
* Testes de integração
* Observabilidade

A ideia é introduzir cada tecnologia apenas quando existir um problema real que justifique sua utilização.

---

## Stack planejada

### Backend

* Java
* Spring Boot
* Spring Web
* Spring WebSocket
* Spring Security
* Spring Data JPA
* PostgreSQL

### Frontend

* React
* TypeScript

### Infraestrutura

Mais adiante:

* Docker
* Redis
* Observabilidade
* Deploy com múltiplas instâncias

---

## Filosofia do projeto

O projeto começa como um **monólito bem estruturado**.

Não serão utilizados microserviços, Kafka, RabbitMQ, Redis ou outras tecnologias distribuídas antes de existir uma necessidade concreta.

A evolução seguirá aproximadamente este fluxo:

```text
problema
   ↓
conceito
   ↓
solução
   ↓
tecnologia
```

Em vez de:

```text
tecnologia
   ↓
onde posso usar isso?
```

O objetivo é entender por que cada componente existe e qual problema ele resolve.

---

# Roadmap

## Fase 1 — WebSocket puro

Primeiro contato com comunicação em tempo real.

Objetivos:

* abrir uma conexão WebSocket;
* enviar mensagens do navegador para o servidor;
* receber mensagens do servidor;
* implementar um Echo WebSocket;
* implementar broadcast;
* entender `WebSocketSession`;
* observar abertura e encerramento de conexões;
* inspecionar handshake e frames WebSocket.

Arquitetura inicial:

```text
Browser
   │
   │ WebSocket
   ▼
Spring Boot
   │
   ├── recebe mensagem
   │
   └── envia mensagem
```

Sem:

* banco de dados;
* autenticação;
* React;
* usuários;
* salas;
* Redis;
* STOMP.

A intenção é entender WebSocket sem abstrações adicionais.

---

## Fase 2 — React

Criar o primeiro cliente real da aplicação.

Objetivos:

* conectar React ao WebSocket;
* enviar mensagens;
* receber eventos;
* controlar estado da conexão;
* compreender lifecycle do WebSocket no frontend.

---

## Fase 3 — Usuários e autenticação

Adicionar identidade às conexões.

Objetivos:

* cadastro;
* login;
* Spring Security;
* autenticação;
* associar uma conexão WebSocket a um usuário;
* entender autenticação durante o handshake e durante a conexão.

---

## Fase 4 — Conversations e Messages

Introduzir persistência.

Objetivos:

* PostgreSQL;
* Spring Data JPA;
* conversas;
* mensagens;
* histórico de mensagens;
* timestamps;
* relacionamento entre usuários, conversas e mensagens.

---

## Fase 5 — Mensagens privadas e grupos

Adicionar roteamento de eventos.

Objetivos:

* conversa privada entre dois usuários;
* salas;
* grupos;
* broadcast seletivo;
* roteamento por conversa.

---

## Fase 6 — Presença

Utilizar o lifecycle da conexão para representar estado dos usuários.

Objetivos:

* online/offline;
* múltiplas conexões do mesmo usuário;
* última vez online;
* conexão e desconexão inesperada.

---

## Fase 7 — Eventos efêmeros

Introduzir eventos que não precisam necessariamente ser persistidos.

Exemplos:

* `typing`;
* read receipts;
* presença;
* eventos temporários.

---

## Fase 8 — Robustez

Estudar comportamento da aplicação em situações menos ideais.

Objetivos:

* reconexão;
* mensagens duplicadas;
* perda de conexão;
* concorrência;
* ordenação de mensagens;
* testes de integração.

---

## Fase 9 — Redis Pub/Sub

Introduzir infraestrutura distribuída apenas quando houver múltiplas instâncias da aplicação.

Arquitetura esperada:

```text
                 ┌── Spring Instance A
Clients ─────────┤
                 └── Spring Instance B
                         │
                         ▼
                      Redis
                      Pub/Sub
```

Objetivos:

* entender por que conexões WebSocket são locais à instância;
* comunicação entre instâncias;
* Redis Pub/Sub;
* escalabilidade horizontal.

---

## Fase 10 — Infraestrutura e observabilidade

Preparar a aplicação para um ambiente mais próximo de produção.

Objetivos:

* Docker;
* deploy;
* logs estruturados;
* métricas;
* health checks;
* observabilidade;
* testes de carga.

---

# MVP

O MVP deverá permitir:

* cadastro de usuário;
* login;
* autenticação;
* criação de conversas;
* conversa privada entre dois usuários;
* salas e grupos;
* envio de mensagens em tempo real;
* histórico de mensagens;
* usuários online/offline;
* timestamp das mensagens.

Depois do MVP poderão ser adicionados:

* indicador de digitação;
* mensagem lida/não lida;
* última vez online;
* reconexão;
* notificações;
* edição de mensagens;
* exclusão de mensagens;
* anexos.

---

# Estado atual

## Fase 1

Neste momento o projeto contém somente a infraestrutura necessária para estudar WebSocket.

Dependências iniciais:

```text
Spring Web
Spring WebSocket
```

Não serão adicionados ainda:

```text
Spring Security
Spring Data JPA
PostgreSQL
Redis
STOMP
Docker
```

Essas dependências serão introduzidas gradualmente.

---

# Primeiro experimento

O primeiro objetivo técnico do projeto é atingir o seguinte fluxo:

```text
Browser
   │
   │ conecta em ws://localhost:8080/ws
   ▼
Spring Boot
   │
   │ conexão estabelecida
   ▼
Browser
   │
   │ "hello"
   ▼
Spring Boot
   │
   │ recebe "hello"
   │
   │ envia "hello"
   ▼
Browser
```

Depois desse experimento, serão estudados:

* handshake HTTP;
* resposta `101 Switching Protocols`;
* frames WebSocket;
* `WebSocketSession`;
* abertura da conexão;
* fechamento da conexão;
* comportamento ao atualizar ou fechar uma aba.

---

# Estrutura inicial

```text
realtime-chat/
├── src/
│   └── main/
│       ├── java/
│       │   └── .../
│       │       ├── RealtimeChatApplication.java
│       │       └── websocket/
│       └── resources/
└── pom.xml
```

A estrutura crescerá conforme novas responsabilidades surgirem.

Não serão criadas antecipadamente dezenas de entidades, controllers ou services sem necessidade.

---

# WebSocket vs HTTP

Uma requisição HTTP normalmente segue o modelo:

```text
Cliente
   │
   │ request
   ▼
Servidor
   │
   │ response
   ▼
Cliente
```

Já uma conexão WebSocket permanece aberta:

```text
Cliente
   ⇅
   ⇅
   ⇅
Servidor
```

Após o handshake inicial, cliente e servidor podem enviar mensagens independentemente.

Isso permite comunicação **full duplex**, fundamental para aplicações em tempo real.

---

# STOMP

Inicialmente o projeto utilizará WebSocket sem STOMP.

Isso é intencional.

Antes de usar abstrações como:

```java
@MessageMapping
@SendTo
```

o objetivo é entender diretamente:

* conexão;
* sessão;
* frames;
* envio;
* recebimento;
* broadcast;
* desconexão.

STOMP poderá ser estudado posteriormente e comparado com uma implementação WebSocket direta.

---

# Princípio importante

Sempre que uma nova tecnologia for adicionada ao projeto, deverão ser respondidas as seguintes perguntas:

1. Qual problema estamos enfrentando?
2. Como essa tecnologia resolve esse problema?
3. Por que estamos escolhendo essa solução?
4. Quais alternativas existem?
5. Quais são as vantagens?
6. Quais são as desvantagens?
7. Como isso aparece em sistemas reais?

Se não houver uma boa resposta para a primeira pergunta, provavelmente ainda não precisamos da tecnologia.

---

## Status

🚧 Em desenvolvimento — Fase 1: WebSocket puro.

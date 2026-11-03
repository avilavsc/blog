+++
title = '01 — RabbitMQ + webhooks + eventos'
date = '2026-10-12'
draft = false
weight = 1
tags = ['semana-03', 'rabbitmq', 'eventos']
categories = ['estudo']
+++
# RabbitMQ + webhooks + eventos · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Mensageria PDPJ (RabbitMQ / webhooks / eventos)  
**Escopo deste arquivo:** mensageria e webhooks no eixo Arquitetura PDPJ-Br do edital — RabbitMQ (AMQP, broker, exchange/queue/binding, producers/consumers), evento negocial, webhooks e APIs reversas, contraste com REST síncrono, papel em microsserviços.  
**Fora de escopo aqui:** Docker/Kubernetes/Rancher (arquivos 02–04); CI/CD e Git (05–06); DevSecOps/Selenium (07); detalhes profundos de Kafka/ActiveMQ (não citados no edital); OAuth/Keycloak (semana 2).

---

## 1. Material

### 1.1 Por que isso cai na PDPJ / edital

No edital (Arquitetura PDPJ-Br), aparecem explicitamente: **mensageria e webhooks**, **RabbitMQ**, **evento negocial**, **APIs reversas**, ao lado de microsserviços, REST/JSON, gateway e persistência.

Leitura cobrável:

| Peça | Papel em prova |
|------|----------------|
| **Mensageria** | Comunicação **assíncrona** via broker (fila/tópico) |
| **RabbitMQ** | Broker de mensagens citado no edital (protocolo AMQP) |
| **Evento negocial** | Fato de negócio publicado para quem se interessar |
| **Webhook** | Callback HTTP: o provedor **chama** a URL do consumidor |
| **API reversa** | Ideia de o provedor **empurrar** notificação (em vez de só o cliente puxar) |

Flash: na semana 1, REST+JSON era o eixo **síncrono**. Aqui entra o eixo **assíncrono / push** do mesmo ecossistema PDPJ.

### 1.2 Síncrono × assíncrono (âncora mental)

| | REST (síncrono) | Mensageria (assíncrono) |
|---|-----------------|-------------------------|
| Modelo | Cliente **espera** resposta HTTP | Produtor **publica**; consumidor processa depois |
| Acoplamento temporal | Ambos precisam estar disponíveis **no momento** da chamada | Produtor e consumidor podem não estar online ao mesmo tempo (com buffer no broker) |
| Contrato típico | URL + verbo + JSON | Mensagem + fila/exchange + payload |
| Uso típico | Consulta/comando com resposta imediata | Notificação, integração desacoplada, carga em picos |

Armadilha: afirmar que microsserviços **proíbem** REST, ou que **só** existem com mensageria. O edital cobra **ambos** no stack PDPJ.

### 1.3 Mensageria — ideias centrais

**Mensageria** = troca de mensagens intermediada por um **broker** (servidor de filas), em vez de chamada direta serviço-a-serviço obrigatoriamente acoplada no tempo.

Papéis clássicos:

| Papel | Função |
|-------|--------|
| **Producer / publisher** | Publica a mensagem |
| **Broker** | Recebe, roteia, armazena temporariamente, entrega |
| **Consumer / subscriber** | Consome e processa |

Benefícios cobráveis em prova:

1. **Desacoplamento** entre produtores e consumidores.  
2. **Amortecimento de picos** (fila acumula; consumidor processa no ritmo dele).  
3. **Resiliência relativa** — se o consumidor cair, mensagens podem permanecer no broker até a retomada (conforme configuração).  
4. **Fan-out** — um evento pode alimentar vários interessados (conforme topologia).

Não confundir:

- Mensageria **≠** banco de dados (não é “persistência JPA”).  
- Mensageria **≠** API Gateway (gateway é borda HTTP; broker é fila).  
- Mensageria **≠** Service Discovery (Eureka descobre instâncias; broker entrega mensagens).

### 1.4 RabbitMQ e AMQP

**RabbitMQ** = broker de mensagens open source amplamente usado; no edital PDPJ é o nome **explícito**.

**AMQP** (Advanced Message Queuing Protocol) = protocolo de mensageria que o RabbitMQ implementa (visão de prova: “RabbitMQ fala AMQP”).

Modelo mental cobrável do RabbitMQ:

```
Producer → Exchange → (binding/roteamento) → Queue → Consumer
```

| Peça | Ideia |
|------|--------|
| **Exchange** | Recebe a mensagem do producer e decide **para onde** enviar |
| **Queue** | Fila onde a mensagem fica até o consumer consumir |
| **Binding** | Regra que liga exchange à queue (com éventual routing key) |
| **Routing key** | Chave usada no roteamento (conforme tipo de exchange) |

Tipos de exchange (visão cobrável, sem virar manual):

| Tipo | Comportamento típico |
|------|----------------------|
| **direct** | Roteia pela igualdade da routing key |
| **fanout** | Copia para **todas** as queues ligadas (broadcast) |
| **topic** | Roteia por padrões na routing key (ex.: `pedido.*`) |
| **headers** | Roteia por cabeçalhos (menos cobrado; só noção) |

Flash: no RabbitMQ, o producer em regra **não** “escolhe a queue” diretamente no modelo clássico — publica no **exchange**; o roteamento leva às queues.

### 1.5 Entrega, ACK e durabilidade (visão)

Ideias que a FCC pode misturar:

| Conceito | Leitura |
|----------|---------|
| **ACK (acknowledge)** | Consumer confirma que processou; sem ACK, a mensagem pode ser reentregue |
| **NACK / reject** | Indica falha no processamento (requeue ou descarte, conforme política) |
| **Durabilidade** | Exchange/queue/mensagem marcados para sobreviver a restart do broker (visão) |
| **Prefetch** | Quantas mensagens o consumer “pega” de uma vez antes de ACK (controle de carga — noção) |

Armadilha: achar que “publicou = processou com sucesso no destino”. Publicar no broker **não** garante, sozinho, o efeito de negócio no consumer — por isso ACK, retries e idempotência entram na conversa de prova em nível conceitual.

### 1.6 Evento negocial

**Evento negocial** = registro de que **algo relevante no domínio aconteceu** (ex.: “petição protocolada”, “audiência redesignada”, “pagamento confirmado”), tipicamente publicado para outros componentes reagirem.

Características cobráveis:

1. Descreve um **fato** (passado), não um “pedido genérico de CRUD”.  
2. Permite **desacoplar** quem gera o fato de quem reage a ele.  
3. Alinha-se a microsserviços: serviço A publica; B e C consomem sem JOIN no banco de A.  
4. Pode circular via **mensageria** (RabbitMQ) e/ou ser notificado via **webhook**.

Contraste útil:

| | Comando / RPC | Evento |
|---|---------------|--------|
| Intenção | “Faça X para mim” (pedido) | “X aconteceu” (fato) |
| Acoplamento | Quem chama espera efeito específico | Quem publica não precisa conhecer todos os reagentes |
| Exemplo mental | `POST /agendar-audiencia` | `AudienciaAgendada` publicado |

Flash: **evento negocial** ≠ log técnico de aplicação; é fato de **negócio** no domínio.

### 1.7 Webhooks

**Webhook** = mecanismo em que o **provedor** envia uma requisição HTTP (em geral **POST** com JSON) para uma **URL cadastrada** pelo consumidor, quando ocorre um evento.

Fluxo típico:

1. Consumidor **registra** uma URL de callback no provedor.  
2. Ocorre o evento no provedor.  
3. Provedor **chama** a URL (push).  
4. Consumidor processa o payload e responde HTTP (ex.: 2xx).

| Aspecto | Webhook | Polling |
|---------|---------|---------|
| Quem inicia | Provedor (push) | Cliente (puxa periodicamente) |
| Latência típica | Baixa após o evento | Depende do intervalo de poll |
| Carga | Sob demanda do evento | Pode gerar tráfego inútil |

Pontos de prova:

- Webhook usa **HTTP** — não substitui o RabbitMQ; são peças **complementares** no edital (“mensageria **e** webhooks”).  
- Exige URL **alcançável** pelo provedor (API exposta / endpoint de callback).  
- Segurança em visão: HTTPS, segredo/HMAC, validação de origem (sem aprofundar OWASP aqui).  
- Falha no callback → retries / dead-letter conforme desenho (noção).

### 1.8 APIs reversas

No vocabulário do edital, **APIs reversas** aproximam-se da ideia de o sistema **notificar** o interessado (empurrar o evento), em contraste com a API “clássica” em que o cliente **consulta** o provedor.

Leitura cobrável para a FCC:

| Modelo | Ideia |
|--------|--------|
| API “direta” / pull | Cliente chama o provedor quando quer dados |
| **API reversa** / push | Provedor chama o cliente (callback / webhook) quando há novidade |

Em muitas bancas e materiais, webhook ≈ implementação prática de “API reversa” / callback. Não invente protocolo novo: associe **API reversa ↔ notificação push / webhook**.

Armadilha: dizer que API reversa “substitui REST” ou “é o nome do RabbitMQ”. São conceitos distintos no mesmo bloco de integração.

### 1.9 Como encaixa em microsserviços / PDPJ

Ponte com a semana 1:

- Comunicação **preferencial cobrada** entre serviços: HTTP REST + JSON.  
- Mensageria/eventos: integração **assíncrona**, desacoplada, típica quando o fato de negócio deve propagar sem travar a requisição do usuário.  
- **Dados por serviço:** em vez de JOIN no schema vizinho, publica-se **evento** ou notifica-se via **webhook**.  
- Gateway/Zuul/Eureka: borda e descoberta HTTP — **não** confundir com broker RabbitMQ.

Cenário mental de prova:

> Serviço de protocolo registra petição → publica evento negocial no RabbitMQ → serviço de notificação consome e envia e-mail; paralelamente, sistema externo cadastrado recebe webhook.

### 1.10 O que a FCC tipicamente testa neste tema

- Mensageria = assíncrono via **broker**; RabbitMQ = broker **AMQP** citado.  
- Exchange → binding → queue → consumer (não “SQL no lugar da fila”).  
- Evento negocial = **fato de negócio**, não log genérico.  
- Webhook = **callback HTTP** (push); oposto relativo ao polling.  
- API reversa ≈ provedor **notifica** (push), alinhada a webhooks.  
- Não confundir com Gateway, Eureka, JPA, OAuth.  
- REST e mensageria **coexistem** no stack PDPJ do edital.

### 1.11 Mini-síntese (cartão mental)

> **RabbitMQ** = broker AMQP (exchange/queue) · **evento negocial** = fato de domínio publicado · **webhook** = POST HTTP de callback · **API reversa** = push do provedor ao interessado · complementa REST síncrono em microsserviços PDPJ.

---

## 2. Questões de treino (1–15)

**1.** No eixo de Arquitetura PDPJ-Br do edital, RabbitMQ é corretamente associado a:

A) Banco relacional substituto do PostgreSQL  
B) Broker de mensagens (mensageria), tipicamente via AMQP  
C) Servidor de identidade SSO equivalente ao Keycloak  
D) Ferramenta exclusiva de migração de schema (Flyway)  
E) API Gateway idêntico ao Zuul  

**2.** Mensageria, em visão de prova, caracteriza-se principalmente por:

A) Comunicação tipicamente **assíncrona** intermediada por broker  
B) Obrigatoriedade de JOIN entre schemas de todos os microsserviços  
C) Substituição total e exclusiva de qualquer HTTP/REST  
D) Ser sinônimo de Hibernate Envers  
E) Funcionar apenas sem producer nem consumer  

**3.** No modelo clássico do RabbitMQ, a ordem conceitual mais adequada é:

A) Consumer → Tabela SQL → Producer  
B) Producer → Exchange → Queue → Consumer  
C) Eureka → Zuul → MapStruct  
D) EntityManager → Flyway → H2  
E) Keycloak → OAuth2 → JWT apenas  

**4.** Um **exchange** do tipo **fanout**, em regra:

A) Entrega a mensagem a **todas** as queues vinculadas (broadcast)  
B) Exige igualdade estrita de uma única routing key e ignora bindings  
C) Substitui o banco PostgreSQL  
D) É o nome do protocolo OAuth2  
E) Apaga automaticamente o persistence context JPA  

**5.** Binding, no RabbitMQ, refere-se a:

A) Regra que associa exchange a queue (roteamento)  
B) Anotação `@Entity` do JPA  
C) Certificado TLS do tribunal  
D) Grant type do OAuth2  
E) Dockerfile de build  

**6.** Evento negocial, no vocabulário do edital, aproxima-se melhor de:

A) Um fato relevante de **negócio/domínio** publicado para integração  
B) Um dump binário do sistema operacional  
C) Um script exclusivo de CircleCI  
D) Um tipo de exchange headers obrigatório  
E) Um dialeto SQL do H2  

**7.** A diferença conceitual mais útil entre **comando/RPC** e **evento** é:

A) Comando pede que se faça algo; evento declara que algo **aconteceu**  
B) Evento sempre usa SOAP; comando sempre usa FTP  
C) São termos rigorosamente intercambiáveis em qualquer frase  
D) Evento só existe dentro do Hibernate  
E) Comando só existe no Rancher  

**8.** Webhook caracteriza-se por:

A) O provedor **enviar** HTTP (callback) a uma URL cadastrada quando há evento  
B) O cliente obrigatoriamente fazer JOIN no banco do provedor  
C) Ser o nome interno do Eureka  
D) Substituir a necessidade de qualquer payload  
E) Ser exclusivamente um banco NoSQL  

**9.** Em relação ao **polling**, o webhook tipicamente:

A) Reduz a necessidade de o cliente ficar consultando periodicamente (modelo push)  
B) Obriga polling a cada milissegundo  
C) Proíbe o uso de HTTPS  
D) É idêntico ao Flyway  
E) Elimina a ideia de URL de callback  

**10.** “API reversa”, na leitura cobrável do bloco mensageria/webhooks, associa-se a:

A) Modelo em que o provedor **notifica/empurra** o interessado (ex.: webhook)  
B) Inversão de bits em memória RAM  
C) Substituição obrigatória do Spring Boot por COBOL  
D) Apenas o algoritmo de paginação virtual  
E) Somente o protocolo SMTP  

**11.** Constitui confusão típica a ser evitada:

A) Tratar RabbitMQ como broker de mensagens  
B) Equiparar RabbitMQ a API Gateway (Zuul) como se fossem a mesma peça  
C) Distinguir evento negocial de log técnico genérico  
D) Relacionar webhook a callback HTTP  
E) Reconhecer coexistência de REST e mensageria no edital  

**12.** ACK (acknowledge) no consumo de mensagens indica, em visão de prova:

A) Confirmação de que o consumer processou (ou aceitou) a mensagem, evitando reentrega indevida  
B) Criação automática de um Persistence Unit  
C) Registro de usuário no Active Directory  
D) Build Maven obrigatório  
E) Deploy exclusivo via FTP  

**13.** Em microsserviços alinhados à ideia de dados por serviço, mensageria/eventos servem tipicamente para:

A) Integrar sem depender de JOIN direto no schema do serviço vizinho  
B) Obrigar um único banco compartilhado com escrita cruzada irrestrita  
C) Eliminar qualquer contrato entre serviços  
D) Substituir a necessidade de identificadores  
E) Proibir JSON em qualquer integração  

**14.** AMQP, no contexto do RabbitMQ, é:

A) Protocolo de mensageria associado ao broker citado no edital  
B) Dialeto exclusivo do PostgreSQL  
C) Framework de mapeamento DTO (MapStruct)  
D) Norma CNJ de sustentabilidade  
E) Sistema de arquivos do Linux  

**15.** A síntese mais adequada deste subtópico no edital PDPJ é:

A) Mensageria (RabbitMQ/AMQP) + eventos negociais + webhooks/APIs reversas, complementando REST  
B) Apenas JPA e Flyway, sem integração assíncrona  
C) Apenas cabeamento e Wi-Fi  
D) Apenas interpretação de texto  
E) Apenas UML obrigatória sem mensageria  

---

### Gabarito comentado (treino 1–15)

1. **B** — RabbitMQ = broker de mensagens (AMQP).  
2. **A** — mensageria assíncrona via broker.  
3. **B** — producer → exchange → queue → consumer.  
4. **A** — fanout = broadcast para queues ligadas.  
5. **A** — binding liga exchange à queue.  
6. **A** — evento negocial = fato de negócio.  
7. **A** — comando pede; evento anuncia fato.  
8. **A** — webhook = callback HTTP do provedor.  
9. **A** — push vs polling.  
10. **A** — API reversa ≈ notificação push/webhook.  
11. **B** — RabbitMQ ≠ API Gateway.  
12. **A** — ACK confirma consumo/processamento.  
13. **A** — integração sem JOIN no schema vizinho.  
14. **A** — AMQP = protocolo de mensageria.  
15. **A** — bloco completo do edital + complemento ao REST.

---

## 3. Questões de concurso (reais) (16–25)

> Questões reais de provas (preferência FCC/TRT Analista TI). Gabarito só ao final.

**16.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Em uma aplicação web, um Tribunal Regional do Trabalho decide usar RabbitMQ para gerenciar notificações de processos trabalhistas. A configuração recomendada que garante que mensagens não sejam perdidas em caso de reinício do servidor RabbitMQ é

A) configurar retries automáticos no consumidor.  
B) configurar o TTL das mensagens para infinito.  
C) habilitar filas duráveis e mensagens persistentes.  
D) configurar Webhooks para reenvio.  
E) usar um banco de dados externo para armazenar mensagens.

**17.** (FCC – TRT-5/2022 – Analista Judiciário – TI)  
No RabbitMQ, o nome dado a uma caixa postal interna que funciona como um grande buffer de mensagem limitado apenas pelos limites de memória e disco do host é

A) producer.  
B) queue.  
C) consumer.  
D) broker.  
E) storage.

**18.** (FCC – TRT-17/2022 – Técnico Judiciário – TI)  
Um Técnico fez a instalação padrão do RabbitMQ em um computador, em condições ideais. O RabbitMQ escutará, na porta padrão

A) 3306  
B) 8081  
C) 1575  
D) 8080  
E) 5672

**19.** (FCC – TRT-22/2022 – Analista Judiciário – TI)  
Uma aplicação envia ao advogado uma notificação avisando que houve uma atualização em um processo, mesmo sem o advogado abrir o aplicativo para procurar a informação. Para que a informação chegue até o sistema do aplicativo do Tribunal em tempo real e até o advogado, da forma relatada, sem a necessidade de ação externa, pode ser utilizado um recurso conhecido como

A) Swagger.  
B) Webhook.  
C) Mockito Pulling.  
D) Eureka Messages.  
E) Elastic Messages.

**20.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Considere que, no ambiente de mensageria da Plataforma Digital do Poder Judiciário PDPJ-Br, os serviços se comunicam entre si por meio de troca de mensagens, fazendo uso de um Message Broker. Neste ambiente, em condições ideais, um Analista, dentre as ações necessárias, está escrevendo o código para envio de uma mensagem usando Java. Ele utiliza Spring Boot como framework de sua aplicação Java e Maven como gerenciador de dependências. Dentre as ações para viabilizar o envio da mensagem, ele adicionou no arquivo POM.XML da aplicação a dependência `spring-boot-starter-amqp`. Já na classe responsável por enviar mensagens, ele acrescentou:

```java
@Autowired
private RabbitTemplate rabbitTemplate;
public void sendMessage(String json) {
    I (this.getRoutingKey(), json);
}
```

A lacuna I deve ser corretamente preenchida com

A) `this.RabbitTemplate.convert`  
B) `this.rabbitTemplate.Send`  
C) `this.RabbitTemplate.send`  
D) `this.rabbitTemplate.convertAndSend`  
E) `this.RabbitTemplate.convert.send`

**21.** (FCC – TRT-19/2022 – Técnico Judiciário – TI)  
Internamente à Plataforma Digital do Poder Judiciário PDPJ-Br, é encorajado que os serviços se comuniquem entre si por meio de troca de mensagens, fazendo uso de um Message Broker. No caso da PDPJ, utiliza-se para isso a solução open source conhecida como

A) Keycloak.  
B) Kubernetes.  
C) RabbitMQ.  
D) TIBCO Enterprise Message Service.  
E) RabbitMB.

**22.** (FCC – TRT-19/2022 – Técnico Judiciário – TI)  
Para o mundo externo que se comunica com a Plataforma Digital do Poder Judiciário Brasileiro (PDPJ-Br), através de chamadas diretas de REST APIs, a comunicação a respeito de eventos disparados pelos serviços, sistemas ou módulos da PDPJ será feita através de um mecanismo que realiza a indicação, ao serviço de notificações da PDPJ, de endpoints dos sistemas processuais dos Tribunais que serão acionados quando os referidos eventos ocorrerem. Este mecanismo é denominado

A) API reversa, que costuma ser chamada de bearer token.  
B) webhook, que costuma ser chamado de API reversa.  
C) bearer token.  
D) evento negocial.  
E) routing keys.

**23.** (FCC – TRT-23/2022 – Técnico Judiciário – TI)  
RabbitMQ é um message broker escrito em Java que

A) suporta apenas o protocolo de mensagens AMQP para atender aos requisitos de alta disponibilidade e escalabilidade.  
B) não permite escolher um mecanismo de persistência, mas oferece opções para personalizar a segurança para autenticação e autorização.  
C) suporta autenticação e autorização conectáveis, suporta LDAP e TLS e pode ser implantado em nuvens públicas e privadas.  
D) pode ser implantado como clusters para alta disponibilidade e alta taxa de transferência, mas não pode ser federado em várias zonas e regiões de disponibilidade.  
E) fornece APIs de cliente somente para a plataforma Java e frameworks relacionados.

**24.** (FGV – TRF-1/2024 – Técnico Judiciário – Desenvolvimento de Sistemas)  
Adriana está desenvolvendo um sistema assíncrono, que deve receber pacotes de solicitações via mensageria RabbitMQ, e agora precisa definir a lógica de roteamento das mensagens. Foi estabelecido que o nome do exchange será logis, roteando para a fila log_1 com a chave tp1 e para log_2 com a chave de roteamento tp2.  
Para criar o exchange e efetuar a ligação com as filas, Adriana utilizou os comandos:

A)
```
rabbitmqadmin declare exchange name=logis type=fanout
rabbitmqadmin declare binding origin=logis routing_key=tp1 type=queue destination=log_1
rabbitmqadmin declare binding origin=logis routing_key=tp2 type=queue destination=log_2
```
B)
```
rabbitmqadmin declare exchange name=logis type=direct
rabbitmqctl connect log_1 to logis with routing_key=tp1
rabbitmqctl connect log_2 to logis with routing_key=tp2
```
C)
```
rabbitmqadmin declare exchange name=logis type=direct
rabbitmqadmin declare binding source=logis routing_key=tp1 destination_type=queue destination=log_1
rabbitmqadmin declare binding source=logis routing_key=tp2 destination_type=queue destination=log_2
```
D)
```
rabbitmqctl create exchange logis type=topic
rabbitmqctl connect log_1 to logis with routing_key=tp1
rabbitmqctl connect log_2 to logis with routing_key=tp2
```
E)
```
rabbitmqctl create exchange logis type=fanout
rabbitmqadmin declare binding source=logis routing_key=tp1 destination_type=queue destination=log_1
rabbitmqadmin declare binding source=logis routing_key=tp2 destination_type=queue destination=log_2
```

**25.** (FGV – TRF-1/2024 – Analista Judiciário – Análise de Sistemas)  
Em um ambiente de microsserviços, é muito comum utilizar o RabbitMQ como plataforma de mensageria. Trata-se de um produto de fácil configuração para execução em contêineres. Sobre o RabbitMQ, é correto afirmar que:

A) por ter sido criado na linguagem Erlang, exige a utilização de consumidores implementados na mesma linguagem;  
B) permite trabalhar apenas com AMQP 0.9.1, inviabilizando qualquer forma de utilização do STOMP ou MQTT;  
C) embora gerencie internamente os privilégios dos usuários, permite o uso de LDAP para autenticação;  
D) trabalha apenas com filas duráveis, o que garante que todas as mensagens se recuperem de eventuais quedas do servidor;  
E) oferece diversos tipos de exchange, como topic exchange, que direciona a mensagem de forma automática para todas as filas associadas, no modelo broadcast.

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **C** | FCC TRT-1/2025 — filas duráveis + mensagens persistentes |
| 17 | **B** | FCC TRT-5/2022 — queue = caixa postal / buffer |
| 18 | **E** | FCC TRT-17/2022 — porta AMQP 5672 |
| 19 | **B** | FCC TRT-22/2022 — webhook (push em tempo real) |
| 20 | **D** | FCC TRT-19/2022 — `rabbitTemplate.convertAndSend` |
| 21 | **C** | FCC TRT-19/2022 — Message Broker PDPJ = RabbitMQ |
| 22 | **B** | FCC TRT-19/2022 — webhook = API reversa |
| 23 | **C** | FCC TRT-23/2022 — LDAP/TLS e nuvens (apesar do enunciado “Java”) |
| 24 | **C** | FGV TRF-1/2024 — exchange `direct` + binding `source=` |
| 25 | **C** | FGV TRF-1/2024 — LDAP para autenticação |

**Notas rápidas:** 16 C (durável + persistente). 17 B (queue). 18 E (5672). 19 B (webhook). 20 D (convertAndSend). 21 C (RabbitMQ). 22 B (API reversa). 23 C (LDAP/TLS). 24 C (direct). 25 C (LDAP).

---

*Arquivo 01 · RabbitMQ + webhooks + eventos · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 Docker.*

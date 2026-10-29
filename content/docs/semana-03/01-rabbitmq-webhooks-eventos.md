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

## 3. Questões estilo concurso FCC (16–25)

**16.** Na Arquitetura PDPJ-Br referida no edital, a menção a RabbitMQ indica, em regra:

A) a adoção conceitual de broker de mensagens para integração assíncrona, tipicamente associada ao protocolo AMQP, sem confundir essa peça com gateway HTTP ou banco relacional  
B) a substituição obrigatória de qualquer API REST por planilhas  
C) a identificação do RabbitMQ como implementação de SSO Keycloak  
D) o uso exclusivo do RabbitMQ como ferramenta de migração Flyway  
E) a proibição de JSON em microsserviços  

**17.** Acerca de webhooks, é correto afirmar que:

A) consistem, em essência, em o provedor disparar requisição HTTP a URL previamente cadastrada quando ocorre determinado evento, em modelo de notificação push  
B) equivalem rigorosamente a um Persistence Context JPA  
C) eliminam a necessidade de qualquer endpoint no consumidor  
D) são o nome oficial do Service Discovery Eureka  
E) funcionam apenas em redes sem IP  

**18.** Evento negocial, no contexto de integração entre serviços:

A) representa fato de domínio relevante, podendo ser publicado para que outros componentes reajam de forma desacoplada  
B) é sinônimo estrito de cabeçalho TCP  
C) substitui, por definição, a chave primária das entidades  
D) coincide termo a termo com checksum do Flyway  
E) existe somente como dialeto H2  

**19.** Considere as afirmativas abaixo.

I. No RabbitMQ, exchange, binding e queue compõem o modelo clássico de roteamento.  
II. Webhooks e mensageria aparecem lado a lado no edital PDPJ como peças de integração.  
III. API Gateway e RabbitMQ desempenham exatamente a mesma função de borda HTTP.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**20.** A expressão “API reversa”, alinhada ao bloco de mensageria e webhooks do edital, corresponde melhor a:

A) abordagem em que o provedor notifica o interessado (push/callback), em contraste com o cliente apenas consultar o provedor sob demanda  
B) inversão física de servidores de arquivos  
C) algoritmo exclusivo de paginação de memória  
D) norma que proíbe HTTPS  
E) substituto obrigatório do protocolo AMQP pelo SMTP  

**21.** Em arquitetura de microsserviços com autonomia de dados, o uso de eventos/mensageria:

A) favorece integração sem depender de acesso direto e irrestrito ao banco do serviço vizinho  
B) obriga todos os serviços a compartilharem um único schema com JOINs cruzados como regra definidora  
C) dispensa qualquer noção de contrato ou payload  
D) elimina a possibilidade de uso de REST no mesmo ecossistema  
E) implica abandono do RabbitMQ em favor exclusivo do Eureka  

**22.** Assinale a opção que descreve corretamente o papel do acknowledge (ACK) no consumo de mensagens:

A) sinalizar ao broker que a mensagem foi aceita/processada no âmbito do consumer, reduzindo risco de reentrega indevida conforme a semântica adotada  
B) criar automaticamente um cluster Kubernetes  
C) documentar API no Swagger de forma exclusiva  
D) mapear DTO com MapStruct  
E) autenticar usuário via grant type password obrigatoriamente  

**23.** Constitui equívoco típico em prova afirmar que:

A) RabbitMQ atua como broker de mensagens  
B) webhook implementa notificação HTTP de callback  
C) evento negocial descreve fato de negócio  
D) mensageria assíncrona e REST síncrono são conceitos distintos e ambos previstos no eixo PDPJ  
E) RabbitMQ e Zuul são a mesma componente com nomes intercambiáveis em qualquer contexto  

**24.** No modelo fanout do RabbitMQ, a mensagem publicada no exchange:

A) tende a ser encaminhada a todas as filas a ele vinculadas, em lógica de difusão  
B) é gravada obrigatoriamente como entidade JPA `@Entity`  
C) autentica o cliente no Keycloak sem token  
D) executa migração Flyway automaticamente  
E) substitui o papel do API Gateway na terminação TLS  

**25.** Para fins de preparação em Analista Judiciário – Área de TI (código 14), a síntese mais adequada deste subtópico é:

A) RabbitMQ como broker AMQP; exchange/queue/binding; evento negocial como fato de domínio; webhooks e APIs reversas como notificação push; coexistência com REST em microsserviços PDPJ  
B) apenas OAuth2 e Keycloak, sem mensageria  
C) apenas CNJ 400 e sustentabilidade  
D) apenas porcentagem e regra de três  
E) apenas cabeamento estruturado  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **A** |
| 17 | **A** |
| 18 | **A** |
| 19 | **C** |
| 20 | **A** |
| 21 | **A** |
| 22 | **A** |
| 23 | **E** |
| 24 | **A** |
| 25 | **A** |

**Notas rápidas:** 16 A (broker AMQP ≠ gateway/BD); 17 A (webhook = callback HTTP push); 18 A (fato de domínio); 19 C (I e II; III confunde Gateway×RabbitMQ); 20 A (API reversa ≈ push); 21 A (integração sem JOIN no vizinho); 22 A (ACK ao broker); 23 E (armadilha RabbitMQ≠Zuul); 24 A (fanout = todas as queues); 25 A (síntese do arquivo 01).

---

*Arquivo 01 · RabbitMQ + webhooks + eventos · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 Docker.*

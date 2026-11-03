+++
title = '07 — Spring Boot × Spring Cloud'
date = '2026-10-07'
draft = false
weight = 7
tags = ['semana-01', 'pdpj', 'spring']
categories = ['estudo']
+++
# Spring Boot × Spring Cloud · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 07 — Spring Boot vs Spring Cloud  
**Escopo deste arquivo:** contraste cobrável **Spring Boot** (como se **constrói/roda** o serviço) × **Spring Cloud** (padrões de **sistemas distribuídos**: discovery, gateway, config…), convivência na mesma app, mapa mental Boot/Cloud/Zuul/Eureka, e armadilhas FCC de sinônimo e inversão de papel.  
**Fora de escopo aqui:** Eureka em profundidade (05), filtros Zuul linha a linha (06), API Gateway só no padrão abstrato (04), Swagger (08), MapStruct (09), REST/JSON (03).

---

## 1. Material

### 1.1 A pergunta que a FCC faz (e confunde)

Em prova, “Spring Boot” e “Spring Cloud” aparecem no mesmo parágrafo — e o distrator clássico é tratar os dois como **o mesmo produto** ou trocar os papéis:

| Nome | Pergunta mental | Resposta curta |
|------|-----------------|----------------|
| **Spring Boot** | Como eu **monto e executo** esta aplicação? | Runtime / empacotamento / auto-config do **serviço** |
| **Spring Cloud** | Como vários serviços **se coordenam** em rede? | Libs/padrões de **arquitetura distribuída** |

Cartão seco (decorar):

> **Boot constrói (e sobe) o serviço.**  
> **Cloud acrescenta padrões distribuídos** (descobrir, entrar pela borda, configurar de forma central…).

Boot **não** “é” o Eureka. Cloud **não** “é” apenas um jar Boot com outro nome. São **camadas de responsabilidade** que **coexistem**.

### 1.2 O que é Spring Boot (leitura de prova)

**Spring Boot** é o jeito moderno de **criar, configurar e empacotar** aplicações Spring com o mínimo de cerimônia:

| Ideia | Frase cobrável |
|-------|----------------|
| **Auto-configuration** | Dependências no classpath → configuração sensata automática |
| **Starter** | Pacotes de dependências (`spring-boot-starter-web` etc.) |
| **Servidor embutido** | Tomcat/Jetty/Undertow **dentro** do jar (visão típica) |
| **Executable jar** | “Um artefato que sobe a app” — microsserviço, monólito, gateway… |
| **Opinionated** | Convenções que aceleram o bootstrap |

Na prova, Boot responde: **como a unidade de deploy sobe**. Cada microsserviço, o Zuul, o Eureka Server — na stack clássica — costumam ser **apps Spring Boot**. Isso **não** transforma Boot em discovery nem em gateway.

Flash: enunciado fala em **starter**, **embedded server**, **jar executável**, **auto-config**, “subir a aplicação Spring rapidamente” → **Spring Boot**.

O que Boot **não** é (ouro anti-distrator):

1. Não é o **Service Discovery** (Eureka).  
2. Não é o **API Gateway** (Zuul / Spring Cloud Gateway).  
3. Não é o **Config Server** centralizado.  
4. Não é documentação OpenAPI (Swagger) nem mapper Entity↔DTO (MapStruct).  
5. Não é sinônimo de “microsserviços” — microsserviços são **estilo arquitetural**; Boot é **ferramenta** para implementar serviços (e outras apps).

### 1.3 O que é Spring Cloud (leitura de prova)

**Spring Cloud** é um **conjunto de projetos/bibliotecas** que trazem para o ecossistema Spring os **padrões de sistemas distribuídos** cobrados no edital PDPJ / microsserviços:

| Padrão (prova) | Face clássica no edital | Pergunta que resolve |
|-----------------|-------------------------|----------------------|
| **Service Discovery** | **Eureka** | Onde está a réplica saudável do serviço X? |
| **API Gateway** | **Zuul** (clássico Netflix) | Por onde o cliente externo entra e é roteado? |
| **Configuração distribuída** | **Spring Cloud Config** | Como centralizar/versionar config fora do jar? |
| Outros (reconhecer nome) | Circuit breaker, client load balancing… | Resiliência / balanceamento no cliente |

Leitura FCC:

- Cloud **não** substitui o Boot: tipicamente **usa** Boot como base da aplicação.  
- Cloud **não** é um único componente: é o **guarda-chuva** de padrões (gateway + discovery + config…).  
- Quando o enunciado cita “padrões Netflix no Spring Cloud”, associe **Zuul + Eureka** (e, se aparecer, Config) — sem inventar migração para Spring Cloud Gateway moderno se o texto não pedir.

Flash: enunciado fala em **descoberta de serviços**, **gateway de borda**, **config server**, “problemas de nuvem/microsserviços” → **Spring Cloud** (padrão), não “Boot sozinho”.

### 1.4 Boot × Cloud: tabela de ouro (anti-sinônimo)

| | **Spring Boot** | **Spring Cloud** |
|---|-----------------|------------------|
| Foco | **Uma** aplicação: bootstrap, config local, empacote, rode | **Várias** aplicações: coordenação distribuída |
| Pergunta | Como este serviço **existe** e sobe? | Como serviços **se acham**, **entram** e **se configuram** em rede? |
| Entrega típica | Starters, auto-config, jar + servidor embutido | Discovery, gateway, config distribuída… |
| Sozinho resolve microsserviços? | Sobe **cada** serviço; **não** traz sozinho o catálogo/gateway | Traz os **padrões**; cada peça ainda é uma app (em geral Boot) |
| Erro FCC | Chamar Boot de Eureka/Zuul | Chamar Cloud de “só o nome novo do Boot” |

Síntese em uma linha:

> Boot = **tijolo do serviço**. Cloud = **planta da cidade distribuída** (ruas = discovery, portaria = gateway, quadro de avisos = config).

### 1.5 Convivência: a mesma app pode ser Boot + Cloud

Modelo mental cobrável (não código):

```
┌─────────────────────────────────────────┐
│  Aplicação (ex.: microsserviço pedidos) │
│  ┌───────────────────────────────────┐  │
│  │ Spring Boot  → sobe, starters, jar│  │
│  └───────────────────────────────────┘  │
│  ┌───────────────────────────────────┐  │
│  │ Spring Cloud → ex.: Eureka Client │  │
│  │              → (opcional) config  │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

Pontos de prova:

1. Um **microsserviço de negócio** = tipicamente **Boot** + (se no ecossistema) **cliente Cloud** (Eureka Client etc.).  
2. O **Eureka Server** = app **Boot** com papel **Cloud** de registry.  
3. O **Zuul** = app **Boot** com papel **Cloud** de API Gateway.  
4. Portanto: “é Boot” e “usa Cloud” **não se excluem** — a FCC testa se você separa **runtime** de **padrão distribuído**.

Armadilha: “Se usa Eureka, deixa de ser Spring Boot.” **Falso** — continua Boot; **ganha** capacidade Cloud.

### 1.6 Mapa Boot / Cloud / Zuul / Eureka

Tabela anti-inversão (alinhar com arquivos 05 e 06):

| Nome | Camada | Papel na prova |
|------|--------|----------------|
| **Spring Boot** | Fundação da app | Empacota/executa o serviço |
| **Spring Cloud** | Guarda-chuva de padrões | Discovery, gateway, config… |
| **Eureka** | Peça **Cloud** | Service Discovery / registry |
| **Zuul** | Peça **Cloud** | API Gateway (borda, rotas, filtros) |
| **Spring Cloud Config** | Peça **Cloud** | Configuração centralizada/versionada |

Fluxo clássico (repetir):

```
Cliente externo
      │
      ▼
  Zuul (Cloud — gateway)     ← app Boot + Cloud
      │
      ▼
 Eureka (Cloud — discovery)  ← app Boot + Cloud (server)
      │
      ▼
 Microsserviço (Boot + Cloud client)
```

Regras de ouro:

1. Boot **não** é Zuul; Boot **não** é Eureka.  
2. Cloud **não** é um sinônimo de Boot.  
3. Zuul e Eureka são **componentes** sob o guarda-chuva Cloud.  
4. Quase tudo nessa topologia **roda como Boot** — isso não apaga os papéis.

### 1.7 Configuração: local no Boot × Config no Cloud

Contraste cobrável (apareceu pouco nos arquivos 05–06; aqui é o lugar):

| | **Config “clássica” Boot** | **Spring Cloud Config** |
|---|---------------------------|-------------------------|
| Onde mora | `application.yml` / properties **no** artefato ou env da instância | Servidor de config (**Config Server**) + clientes |
| Escopo mental | Uma app / um deploy | Vários serviços; config **central** e frequentemente **versionada** (ex.: Git) |
| Pergunta | Quais props **desta** app? | Como **compartilhar/atualizar** config no ecossistema? |

Leitura FCC:

- Boot **sempre** tem configuração de aplicação; isso **não** torna Boot = Config Server.  
- **Cloud Config** é o padrão de **configuração distribuída** — peça de Cloud, não “o Boot em si”.  
- Distrator: “Spring Boot é o servidor central de configuração de todos os microsserviços.” → **não**; isso aponta para **Config** (Cloud).

### 1.8 O que cada um resolve (e o que não resolve)

| Necessidade | Quem responde na prova |
|-------------|------------------------|
| Subir app rápido com starters e jar | **Spring Boot** |
| Achar instância por nome lógico | **Cloud → Eureka** |
| Entrada única + rotas + filtros | **Cloud → Zuul** |
| Config centralizada entre serviços | **Cloud → Config** |
| Documentar API / try-it | **Swagger** (arquivo 08) — não Boot nem Cloud |
| Mapear Entity↔DTO | **MapStruct** (arquivo 09) |

Boot sozinho **não** “resolve microsserviços”: resolve **implementar e operar cada unidade**. Os **problemas de distribuição** (localizar, bordar, config compartilhada) são território **Cloud**.

Cloud **não** elimina a necessidade de Boot na stack clássica do edital: as peças Cloud **são** aplicações Boot.

### 1.9 O que a FCC tipicamente testa neste tema

- Boot = **construção/execução** do serviço (starters, auto-config, jar embutido).  
- Cloud = **padrões distribuídos** (discovery, gateway, config…).  
- **Não são sinônimos**; **convivem** na mesma aplicação.  
- Eureka/Zuul/Config são **peças Cloud**, não “nomes alternativos do Boot”.  
- “App Boot com Eureka Client” continua sendo Boot — **com** discovery.  
- Separar **ferramenta de bootstrap** × **padrão de arquitetura distribuída**.  
- Não confundir Boot/Cloud com Swagger, MapStruct, banco compartilhado.

### 1.10 Armadilhas FCC (lista de ouro)

1. Tratar **Spring Boot** e **Spring Cloud** como **o mesmo produto**.  
2. Dizer que Boot **é** o API Gateway (Zuul).  
3. Dizer que Boot **é** o Service Discovery (Eureka).  
4. Afirmar que Cloud **substitui** Boot (em vez de **complementar**).  
5. Afirmar que “usar Eureka” **desliga** o Boot.  
6. Reduzir Cloud a “só um starter web”.  
7. Chamar Config Server de “feature exclusiva do Boot sem Cloud”.  
8. Inverter: Cloud = jar embutido; Boot = discovery/gateway.  
9. Misturar com Swagger / MapStruct / Hibernate Envers.  
10. Achar que Boot sozinho implementa o **catálogo dinâmico** e a **borda** do ecossistema.

### 1.11 Mini-síntese (cartão mental)

> **Spring Boot** = como o serviço **é construído e sobe** (starters, auto-config, jar + servidor embutido).  
> **Spring Cloud** = padrões **distribuídos** sobre (tipicamente) Boot: **Eureka** (onde?), **Zuul** (entrada?), **Config** (config central?).  
> Boot **≠** Cloud · Boot **+** Cloud · Cloud **≠** Eureka sozinho · Eureka/Zuul ⊂ Cloud.

---

## 2. Questões de treino (1–15)

**1.** Em termos cobrados pela FCC no eixo PDPJ/microsserviços, Spring Boot descreve prioritariamente:

A) O registry de heartbeats de todas as réplicas  
B) A forma de construir, auto-configurar e empacotar/executar a aplicação Spring  
C) O único nome correto para API Gateway Netflix  
D) O gerador compile-time Entity↔DTO  
E) O dialect SQL do PostgreSQL  

**2.** Spring Cloud, na leitura de concurso, é melhor caracterizado como:

A) Sinônimo estrito e exclusivo de Spring Boot  
B) Apenas o servidor embutido Tomcat  
C) Conjunto de libs/padrões para sistemas distribuídos (discovery, gateway, config…)  
D) Ferramenta exclusiva de documentação OpenAPI  
E) Mapper MapStruct obrigatório  

**3.** Assinale a associação **correta**:

A) Boot = empacota/roda o serviço; Cloud = padrões distribuídos  
B) Boot = Eureka; Cloud = apenas Swagger  
C) Boot = Zuul; Cloud = apenas H2  
D) Boot = Config Server exclusivo; Cloud = jar sem starters  
E) Boot e Cloud são nomes do mesmo filtro Zuul  

**4.** Um microsserviço Spring que sobe como jar com starters e ainda se registra no Eureka:

A) Deixa de ser Spring Boot automaticamente  
B) Vira somente MapStruct  
C) Substitui o protocolo HTTP por SMTP  
D) Continua sendo tipicamente uma app Spring Boot que usa capacidade Spring Cloud  
E) Passa a ser, por definição, o API Gateway  

**5.** No mapa mental do edital, Eureka e Zuul situam-se como:

A) Substitutos do conceito de Spring Boot  
B) Nomes comerciais do Swagger UI  
C) Peças/padrões tipicamente associados ao Spring Cloud  
D) Dialetos JPA  
E) Únicos starters web do Boot, sem relação com discovery/gateway  

**6.** Qual necessidade é respondida **diretamente** pelo Spring Boot (e não pelo papel de discovery)?

A) Bootstrap rápido da aplicação com auto-config e servidor embutido  
B) Localizar dinamicamente host:porta de uma réplica via registry  
C) Expor ponto único de entrada com filtros pre/route/post  
D) Versionar config central de dezenas de serviços no Config Server  
E) Gerar UI try-it OpenAPI  

**7.** Qual necessidade é território típico do **Spring Cloud**, não do Boot “sozinho”?

A) Empacotar um fat-jar com Tomcat embutido  
B) Escolher a annotation `@SpringBootApplication`  
C) Compilar mappers MapStruct  
D) Service Discovery / API Gateway / configuração distribuída entre serviços  
E) Rodar Flyway migrate em um monólito isolado sem rede  

**8.** Afirmar que “Spring Boot é o API Gateway do ecossistema” é:

A) Correto em qualquer enunciado FCC  
B) Correto porque Boot = Eureka  
C) Incorreto: o papel de gateway clássico cobrado é tipicamente o Zuul (Cloud); Boot é a forma de subir a app  
D) Correto porque Boot = Config Server  
E) Correto somente se o payload for XML  

**9.** Sobre Spring Cloud Config em contraste com `application.yml` local do Boot:

A) São sempre o mesmo artefato sem diferença cobrável  
B) Todo yml local transforma a app em Eureka Server  
C) Config Server é função exclusiva do MapStruct  
D) Cloud Config substitui HTTP por SOAP  
E) Config Cloud aponta para configuração centralizada/distribuída; o yml local é config da app — Boot ≠ Config Server  

**10.** Qual afirmação é **incorreta**?

A) Boot e Cloud podem coexistir na mesma aplicação  
B) Eureka é face clássica de discovery no Cloud  
C) Zuul é face clássica de gateway no Cloud Netflix do edital  
D) Spring Boot, por definição, é o catálogo que registra heartbeats (papel do Eureka)  
E) Cloud não é sinônimo de Boot  

**11.** “O componente oferece starters, auto-configuration e jar executável com servidor embutido.” O enunciado descreve:

A) Spring Boot  
B) Eureka Server como conceito puro de registry, sem Boot  
C) Somente o filtro error do Zuul  
D) Swagger Codegen obrigatório  
E) Hibernate Envers  

**12.** “Diversos microsserviços precisam localizar uns aos outros e o cliente externo entra por uma borda com rotas.” O guarda-chuva de padrões mais alinhado é:

A) Apenas MapStruct  
B) Somente o dialect H2  
C) Spring Cloud (ex.: Eureka + Zuul)  
D) Apenas `@RestController` sem noção de distribuição  
E) Flyway Exclusive Edition  

**13.** Relativamente à convivência Boot × Cloud, assinale a opção correta:

A) Em stack clássica, peças Cloud (Zuul, Eureka…) costumam ser elas mesmas aplicações Boot  
B) Usar Cloud obriga abandonar o modelo de app Boot  
C) Boot elimina qualquer necessidade conceitual de discovery  
D) Cloud é apenas o nome antigo do Spring MVC  
E) Zuul e Eureka existem apenas fora do universo Spring  

**14.** Distrator clássico da FCC neste subtópico é:

A) Separar claramente bootstrap do serviço e padrões distribuídos  
B) Associar Eureka a discovery  
C) Associar Zuul a gateway  
D) Tratar Boot e Cloud como sinônimos ou inverter seus papéis  
E) Lembrar que Config Server ≠ jar local isolado  

**15.** Qual linha resume melhor o cartão mental deste arquivo?

A) Boot = gateway; Cloud = mapper; Eureka = docs  
B) Cloud substitui Boot; Eureka é a porta pública; Zuul é o registry  
C) Boot = constrói/sobe o serviço; Cloud = discovery + gateway + config (distribuído); Eureka/Zuul ⊂ Cloud  
D) Boot sozinho é o Config Server de todos os serviços e o Zuul ao mesmo tempo  
E) Boot = Swagger; Cloud = MapStruct  

---

### Gabarito comentado (treino 1–15)

1. **B** — Boot = construir/auto-config/empacotar/rodar.  
2. **C** — Cloud = padrões distribuídos.  
3. **A** — Tabela de ouro Boot × Cloud.  
4. **D** — Coexistem: Boot + cliente Cloud.  
5. **C** — Eureka/Zuul sob o guarda-chuva Cloud.  
6. **A** — Bootstrap/jar/servidor embutido = Boot.  
7. **D** — Discovery/gateway/config = Cloud.  
8. **C** — Gateway clássico = Zuul; Boot sobe a app.  
9. **E** — Config central (Cloud) ≠ só yml local.  
10. **D** — Heartbeat/registry = Eureka, não Boot.  
11. **A** — Starters + auto-config + jar = Boot.  
12. **C** — Localizar + borda = Cloud (Eureka+Zuul).  
13. **A** — Peças Cloud tipicamente são apps Boot.  
14. **D** — Sinônimo/inversão = armadilha clássica.  
15. **C** — Síntese do arquivo 07.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60)

Correspondem, correta e respectivamente, a um componente do Spring Cloud e à principal função do Spring Cloud Config:

A) Spring Security — implementar um padrão de design para a comunicação entre serviços baseados em REST.

B) Spring Boot Actuator — gerenciar o ciclo de vida e o monitoramento de aplicações distribuídas em uma arquitetura de microsserviços.

C) Spring Data JPA — oferecer uma solução para o balanceamento de carga entre instâncias de microsserviços.

D) Spring Cloud Stream — centralizar a configuração de aplicações e serviços, permitindo que configurações sejam atualizadas dinamicamente.

E) Spring Web MVC — fornecer um serviço de autenticação e autorização para microsserviços e aplicações.


**17.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 43 de 60)

As versões mais recentes do Spring Boot permitem a configuração de inicialização lenta (lazy initialization), que faz com que os beans sejam criados à medida que são necessários e não durante a inicialização do aplicativo. Uma das maneiras de habilitar a inicialização lenta é colocar

A) `@Lazy(true)` no arquivo `application.properties`

B) `spring.main.lazy-initialization=true` no arquivo `application.properties`

C) `set-lazy-initialization=true` no arquivo `spring-boot.xml`

D) `lazy-initialization=true` no arquivo `spring-application-builder.xml`

E) `@SpringMainLazyInitialization` na classe `SpringApplicationBuilder`


**18.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Uma equipe de desenvolvimento está utilizando Spring Boot para criar uma API RESTful. Durante os testes, observaram que a aplicação demora a inicializar, principalmente devido à configuração do banco de dados. Para melhorar o tempo de inicialização, decidiram desabilitar a criação automática do esquema de banco de dados ao iniciar a aplicação. A configuração, no `application.properties`, que desabilita a criação automática do esquema de banco de dados pelo Spring Boot é a

A) `spring.jpa.hibernate.ddl-auto=none`

B) `spring.datasource.database-auto=false`

C) `spring.jpa.db-sql=disable`

D) `spring.jpa.properties.hibernate.db-auto=disable`

E) `spring.jpa.create-database-init=false`


**19.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60)

Na classe principal de uma aplicação criada com Spring Boot, para fazer com que a aplicação atue como um cliente Eureka utiliza-se a anotação

A) `@EurekaClient`

B) `@EnableEurekaClient`

C) `@EnableDiscoveryEurekaClient`

D) `@EurekaDiscoveryClient`

E) `@IsEurekaClient`


**20.** (FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60)

O Spring Eureka é uma implementação do padrão de registro de serviços que permite que os microsserviços se registrem automaticamente e se descubram uns aos outros. A porta padrão em que o servidor Eureka irá executar na aplicação Spring Boot e o arquivo em que essa porta é configurada são, respectivamente,

A) `80` e `eureka-server.xml`

B) `15000` e `application-server.yml`

C) `8080` e `pom.xml`

D) `8005` e `eureka-registry.xml`

E) `8761` e `application.properties`


**21.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60)

Um Analista de TI está desenvolvendo uma API com Spring Boot e deseja integrar Swagger para facilitar a documentação e testes dessa API. Para integrar no Swagger com um projeto Spring Boot a abordagem mais adequada é

A) adicionar as dependências do Swagger manualmente no classpath do projeto.

B) adicionar as dependências do Swagger no arquivo `pom.xml` ou `build.gradle` do projeto Spring Boot.

C) criar um arquivo separado para todas as configurações do Swagger sem importar as dependências no projeto.

D) utilizar um plugin externo para o IDE que automaticamente gera a documentação do Swagger sem configuração adicional.

E) configurar o servidor de aplicação para disponibilizar a documentação do Swagger sem modificar o código do projeto.


**22.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22)

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.

B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.

C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.

D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.

E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.


**23.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60)

Na abordagem do Spring para construir serviços web RESTful, as solicitações HTTP são tratadas por um controlador, que é uma classe identificada com a anotação

A) `@RestfulController`

B) `@RestfulMapping`

C) `@RestApplicationController`

D) `@RestController`

E) `@RequestController`


**24.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 30 de 60)

Considere que, no ambiente de mensageria da Plataforma Digital do Poder Judiciário PDPJ-Br, os serviços se comunicam entre si por meio de troca de mensagens, fazendo uso de um Message Broker. Neste ambiente, em condições ideais, um Analista, dentre as ações necessárias, está escrevendo o código para envio de uma mensagem usando Java. Ele utiliza Spring Boot como framework de sua aplicação Java e Maven como gerenciador de dependências. Dentre as ações para viabilizar o envio da mensagem, ele adicionou no arquivo POM.XML da aplicação:```<dependency> <groupId>org.springframework.boot</groupId> <artifactId>spring-boot-starter-amqp</artifactId></dependency>```Já na classe responsável por enviar mensagens da sua aplicação, ele acrescentou:```@Autowiredprivate RabbitTemplate rabbitTemplate;public void sendMessage(String json) { I (this.getRoutingKey(), json);}```A lacuna I deve ser corretamente preenchida com

A) `this.RabbitTemplate.convert`

B) `this.rabbitTemplate.Send`

C) `this.RabbitTemplate.send`

D) `this.rabbitTemplate.convertAndSend`

E) `this.RabbitTemplate.convert.send`


**25.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60) *[aproximação temática]*

Ao usar o Gateway API com o Service Discovery no contexto da PDPJ-Br, a principal consequência para o frontend em uma aplicação baseada em microsserviços é que o

A) Gateway API diminui a necessidade de autenticação, uma vez que o Service Discovery é integrado ao SSO.

B) frontend deve configurar manualmente os IPs e portas dos serviços disponíveis na plataforma.

C) frontend se torna o responsável pela chamada direta de cada microsserviço individualmente.

D) Gateway API reduz o número de chamadas externas que o frontend precisa fazer, tornando a comunicação mais eficiente.

E) frontend deve gerenciar manualmente as atualizações de endpoint de cada serviço individual registrado.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **D** | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 17 | **B** | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 43 de 60 |
| 18 | **A** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 19 | **B** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60 |
| 20 | **E** | FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60 |
| 21 | **B** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 22 | **E** | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22 |
| 23 | **D** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60 |
| 24 | **D** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 30 de 60 |
| 25 | **D** \* | FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q53
- Q17: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q43
- Q18: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q38
- Q19: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q32
- Q20: https://www.quizinho.com.br/questao/fcc-trt15-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q31
- Q21: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q53
- Q22: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22
- Q23: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q31
- Q24: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q30
- Q25: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q39

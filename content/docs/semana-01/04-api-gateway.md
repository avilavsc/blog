+++
title = '04 — API Gateway'
date = '2026-10-04'
draft = false
weight = 4
tags = ['semana-01', 'pdpj', 'api-gateway']
categories = ['estudo']
+++
# API Gateway · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — API Gateway  
**Escopo deste arquivo:** papel de entrada externa, roteamento, preocupações transversais (auth, rate limit etc.), ideia de agregação, contraste anti-confusão com Service Discovery e relação de alto nível com Zuul.  
**Fora de escopo aqui:** Eureka em profundidade (arquivo 05), filtros Zuul detalhados / Spring Cloud Gateway moderno (arquivo 06), Spring Boot × Cloud (07), Swagger, MapStruct, REST/JSON aprofundado (03).

---

## 1. Material

### 1.1 O que é API Gateway (leitura de prova)

**API Gateway** é o **ponto único de entrada** (*single entry point*) pelo qual o **cliente externo** (browser, app, outro sistema) acessa o ecossistema de microsserviços. Em vez de o cliente conhecer e chamar dezenas de URLs internas de serviços, ele fala com **uma borda**; o gateway **encaminha** a chamada ao serviço adequado.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Posição | **Borda / fachada** do ecossistema para o mundo externo |
| Ação central | **Roteia** a requisição ao microsserviço-alvo |
| Extra típico | Aplica **políticas transversais** (auth, limite de taxa, log…) |

API Gateway **não** é: banco de dados, catálogo de instâncias (isso é Discovery), documentação interativa (Swagger), nem mapper Entity↔DTO (MapStruct).

### 1.2 Papel como entrada externa

Em microsserviços, há muitos serviços com hosts/portas internas. Expor todos diretamente ao cliente externo traz:

- acoplamento do cliente à topologia interna;
- dificuldade de padronizar autenticação, logs e cotas;
- risco de superfície de ataque ampla.

O gateway resolve isso como **fachada**:

```
Cliente externo  →  API Gateway  →  microsserviço(s)
```

Leitura de prova:

- O cliente **não precisa** saber a topologia interna completa.
- A “porta pública” / URL estável do sistema é a do **gateway**.
- Serviços internos permanecem atrás da borda (visão típica cobrada).

Flash: **pergunta do Gateway** = “por onde o cliente **externo** entra?”

### 1.3 Roteamento

Função nuclear: **rotear** (encaminhar) a requisição com base em regras — tipicamente path, host, cabeçalhos ou metadados — para o serviço responsável.

Exemplos mentais (não decorar produto):

| Entrada no gateway | Destino típico |
|--------------------|----------------|
| `/api/processos/**` | serviço de processos |
| `/api/partes/**` | serviço de partes |
| `/api/auth/**` | serviço de identidade |

Pontos cobráveis:

- Roteamento **não** significa “o gateway implementa toda a regra de negócio”.
- O gateway **delega** ao serviço; ele é **borda**, não monólito escondido.
- Em stack dinâmica, o destino concreto (IP/porta da réplica) costuma vir do **Service Discovery** — o gateway **usa** o catálogo; **não o substitui**.

### 1.4 Preocupações transversais (*cross-cutting*)

Além de rotear, o gateway concentra políticas que, senão, se repetiriam em **todo** serviço:

| Política | Ideia cobrável |
|----------|----------------|
| **Autenticação / autorização na borda** | Validar token/credencial **antes** de chegar ao serviço (ou padronizar o fluxo) |
| **Rate limiting** | Limitar taxa de chamadas (anti-abuso, proteção de capacidade) |
| **Logging / auditoria de borda** | Registrar entrada/saída, correlação de requests |
| **SSL termination / TLS na borda** | Terminar HTTPS no gateway (visão frequente de desenho) |
| **Transformação leve de request/response** | Cabeçalhos, path rewrite — sem virar “ESB pesado” no discurso de prova |

Síntese: *cross-cutting* = “faz **uma vez na borda** o que senão faria N vezes nos serviços”.

Armadilha: achar que autenticação **só** pode existir no gateway e **jamais** nos serviços — em prova, o foco é o **papel típico** do gateway como local natural dessas políticas, não dogma absoluto de segurança.

### 1.5 Ideia de agregação (*API composition* / BFF leve)

Além do “1 request → 1 serviço”, o gateway (ou camada de borda) pode **agregar**:

- Cliente pede “tela do processo” com um único endpoint de borda.
- Gateway chama **vários** microsserviços internos.
- Compõe uma resposta única para o cliente.

Leitura de prova:

| Sem agregação | Com agregação (ideia) |
|---------------|------------------------|
| Cliente faz N chamadas a N serviços | Cliente faz **1** chamada à borda |
| Cliente conhece mais da topologia | Cliente vê **fachada** estável |
| Mais round-trips na rede do cliente | Composição fica **do lado servidor/borda** |

Cuidado cobrável: agregação **não** transforma o gateway em “dono de todo o domínio”. Continua sendo padrão de **composição na borda**, não substituição dos serviços.

### 1.6 API Gateway × Service Discovery (anti-confusão — ouro da FCC)

A banca **adora** inverter esses papéis. Decore esta tabela:

| | **API Gateway** | **Service Discovery** |
|---|-----------------|------------------------|
| Pergunta | Por onde o **cliente externo** entra? | **Onde** está a instância saudável do serviço X? |
| Papel | **Ponto único de entrada**; roteia; cross-cutting | **Catálogo dinâmico** de instâncias (IP/porta mudam) |
| Face clássica no edital (Spring/Netflix) | **Zuul** | **Eureka** |
| Erro típico | Chamar Eureka de “entrada única” | Chamar Zuul de “registry/catálogo” |

Fluxo mental cobrável:

```
Cliente externo → API Gateway (Zuul) → (consulta) Discovery (Eureka) → microsserviço
```

Regras de ouro:

1. Gateway **não** substitui o registry.  
2. Discovery **não** é a porta pública única.  
3. Um componente **entra**; o outro **localiza**.

### 1.7 Relação com Zuul (alto nível apenas)

No eixo do edital (PDPJ / Spring Cloud clássico Netflix), **Zuul** aparece como a **implementação de API Gateway** cobrada:

| Conceito | Associação de prova |
|----------|---------------------|
| Padrão / papel | **API Gateway** |
| Nome de produto no edital | **Zuul** |
| Par clássico | **Zuul = entrada** · **Eureka = localização** |

O que basta neste arquivo 04:

- Zuul = **gateway de borda** (roteamento + filtros / políticas na entrada).
- Não aprofundar tipos de filtro (pre/route/post), configuração YAML, nem migração para Spring Cloud Gateway — isso é arquivo **06**.
- Não confundir Zuul com Eureka, Swagger ou MapStruct.

Flash: se o enunciado diz “entrada única / roteamento de borda / filtros na borda” → **Gateway / Zuul**. Se diz “registro de instâncias / heartbeat / catálogo” → **Discovery / Eureka**.

### 1.8 O que a FCC tipicamente testa neste tema

- Gateway = **entrada única** externa.
- Roteia para microsserviços; **não** é o registry.
- Aplica **cross-cutting**: auth, rate limit, log (lista típica).
- Pode **agregar** respostas (ideia).
- Contraste seco com **Service Discovery**.
- Associação **Zuul ↔ API Gateway** (papel, não tutorial).
- Fluxo: Cliente → Gateway → (Discovery) → serviço.

### 1.9 Armadilhas FCC (lista de ouro)

1. Chamar **Eureka** de “ponto único de entrada”.
2. Chamar **Zuul** de “catálogo / registry de instâncias”.
3. Dizer que Gateway **elimina** a necessidade de Discovery em qualquer topologia.
4. Afirmar que Gateway **implementa** toda a regra de negócio dos serviços.
5. Confundir Gateway com **Swagger** (documentação) ou **MapStruct** (mapper).
6. Tratar Gateway e Discovery como **sinônimos**.
7. Achar que rate limit / auth na borda “proíbem” qualquer checagem nos serviços (exagero).
8. Inverter o fluxo: Cliente → Eureka (borda pública) → Zuul (catálogo).

### 1.10 Mini-síntese (cartão mental)

> **API Gateway** = **entrada única** + **roteamento** + **políticas de borda** (+ ideia de **agregação**).  
> **Service Discovery** = **onde** está a réplica.  
> **Zuul** (edital) = face clássica do **Gateway**.  
> **Eureka** (edital) = face clássica do **Discovery**.  
> Cliente → **Zuul** → (consulta) **Eureka** → serviço.

---

## 2. Questões de treino (1–15)

**1.** Em arquitetura de microsserviços, o API Gateway desempenha, prioritariamente, o papel de:

A) Banco relacional compartilhado por todos os serviços  
B) Ponto único de entrada externo que roteia requisições aos serviços internos  
C) Catálogo exclusivo de heartbeats de instâncias mortas  
D) Gerador compile-time de mappers Entity↔DTO  
E) Protocolo binário que substitui o HTTP  

**2.** A pergunta mental que melhor distingue o API Gateway é:

A) “Qual o dialect SQL do PostgreSQL?”  
B) “Por onde o cliente externo entra no ecossistema?”  
C) “Qual annotation gera o OpenAPI UI?”  
D) “Qual jar embute o Tomcat no Boot?”  
E) “Qual tabela do Envers guarda auditoria?”  

**3.** Sobre roteamento no API Gateway, é correto afirmar que:

A) O gateway substitui integralmente a lógica de domínio de todos os microsserviços  
B) O gateway encaminha a requisição ao serviço-alvo conforme regras (ex.: path), sem ser, em si, o dono de todo o negócio  
C) Roteamento significa gravar JOIN direto no banco de outro serviço  
D) Roteamento só existe se houver WSDL  
E) Roteamento elimina a necessidade de códigos HTTP de status  

**4.** Autenticação na borda e rate limiting são exemplos típicos de:

A) Persistência JPA por serviço  
B) Preocupações transversais (*cross-cutting*) concentradas no gateway  
C) Funções exclusivas do MapStruct  
D) Papéis do Eureka Server como registry  
E) Obrigações do Flyway em migração de schema  

**5.** A ideia de agregação (composição) associada ao gateway consiste em:

A) O cliente chamar N serviços internos e o gateway apenas observar  
B) O gateway poder orquestrar/consultar vários serviços e devolver uma resposta composta ao cliente  
C) Obrigar um único banco compartilhado entre todos os microsserviços  
D) Substituir REST por stored procedures  
E) Proibir qualquer política de autenticação  

**6.** Assinale a associação correta no contraste anti-confusão:

A) API Gateway responde “onde está a instância?”; Discovery responde “por onde o cliente entra?”  
B) API Gateway = entrada/roteamento; Service Discovery = catálogo dinâmico de instâncias  
C) Ambos são sinônimos estritos de Swagger  
D) Discovery é a única porta pública; Gateway só documenta APIs  
E) Gateway = MapStruct; Discovery = Hibernate Envers  

**7.** No fluxo típico cobrado (cliente externo + stack Spring Cloud clássica do edital):

A) Cliente → Eureka (borda pública) → Zuul (catálogo) → serviço  
B) Cliente → API Gateway/Zuul → (consulta) Discovery/Eureka → microsserviço  
C) Cliente → MapStruct → Swagger UI → H2  
D) Cliente → Flyway → Envers → Eureka Dashboard  
E) Cliente → SOAP exclusivamente, sem gateway  

**8.** Zuul, no nível cobrado neste subtópico, associa-se principalmente a:

A) Service Discovery / registry de instâncias  
B) API Gateway / entrada e roteamento de borda  
C) Documentação OpenAPI interativa  
D) Mapeamento compile-time de POJOs  
E) Migração versionada de schema SQL  

**9.** Qual afirmação sobre API Gateway é **incorreta**?

A) Pode centralizar rate limiting  
B) Atua como fachada para o cliente externo  
C) É, por definição, o catálogo que registra heartbeats das réplicas (papel do Discovery)  
D) Pode aplicar checagens de autenticação na borda  
E) Encaminha tráfego aos microsserviços conforme rotas  

**10.** Service Discovery **não** se confunde com API Gateway porque:

A) Discovery localiza instâncias; Gateway é a entrada/roteamento externo  
B) Ambos apenas geram DTOs em tempo de compilação  
C) Discovery documenta contratos OpenAPI; Gateway grava auditoria JPA  
D) São nomes comerciais do mesmo jar do MapStruct  
E) Discovery substitui o HTTP  

**11.** Um enunciado diz: “O componente de borda valida o token, aplica limite de requisições por minuto e encaminha `/api/processos` ao serviço correspondente.” O papel descrito é tipicamente o de:

A) Eureka como registry puro, sem borda  
B) API Gateway  
C) MapStruct  
D) Swagger Codegen obrigatório  
E) Hibernate Envers  

**12.** Afirmar que “o API Gateway elimina a necessidade de Service Discovery em qualquer topologia” é:

A) Sempre verdadeiro em todas as provas  
B) Impreciso: o gateway pode *usar* o discovery para achar réplicas; um não apaga o papel do outro  
C) Correto, porque Eureka e Zuul são o mesmo processo  
D) Correto, porque microsserviços proíbem catálogo dinâmico  
E) Correto somente se o payload for XML  

**13.** Em prova FCC alinhada ao edital PDPJ, a face clássica de API Gateway citada junto à stack Netflix/Spring Cloud é:

A) Eureka  
B) Zuul  
C) MapStruct  
D) Flyway  
E) H2  

**14.** Qual opção melhor resume o cartão mental deste subtópico?

A) Gateway = docs; Eureka = entrada; Zuul = mapper  
B) Gateway/Zuul = entrada + roteamento + políticas de borda; Discovery/Eureka = onde está a réplica  
C) Gateway = banco único; Discovery = SOAP  
D) Zuul = OpenAPI; Eureka = Entity↔DTO  
E) Agregação obriga sticky session como definição de REST  

**15.** Misturar papéis é erro clássico. Qual linha está **correta**?

A) Swagger roteia na borda; Zuul documenta try-it  
B) Eureka é ponto único de entrada público; Zuul só lista instâncias  
C) Zuul (gateway) entra; Eureka (discovery) localiza  
D) MapStruct aplica rate limit; Flyway autentica JWT  
E) Envers substitui o API Gateway  

---

### Gabarito comentado (treino 1–15)

1. **B** — Entrada única + roteamento; não é BD, registry, mapper nem protocolo.  
2. **B** — “Por onde o externo entra?” = pergunta do Gateway.  
3. **B** — Encaminha; não engole o domínio inteiro.  
4. **B** — Auth e rate limit = *cross-cutting* de borda.  
5. **B** — Composição/agregação na borda para o cliente.  
6. **B** — Tabela anti-confusão clássica.  
7. **B** — Fluxo Cliente → Gateway → Discovery → serviço.  
8. **B** — Zuul = face Gateway no edital.  
9. **C** — Catálogo/heartbeat = Discovery, não Gateway.  
10. **A** — Localiza × entra/roteia.  
11. **B** — Token + rate limit + path = Gateway.  
12. **B** — Papéis complementares; um não “mata” o outro.  
13. **B** — Zuul.  
14. **B** — Síntese do arquivo 04.  
15. **C** — Duo Zuul/Eureka sem inverter.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60)

Ao usar o Gateway API com o Service Discovery no contexto da PDPJ-Br, a principal consequência para o frontend em uma aplicação baseada em microsserviços é que o

A) Gateway API diminui a necessidade de autenticação, uma vez que o Service Discovery é integrado ao SSO.

B) frontend deve configurar manualmente os IPs e portas dos serviços disponíveis na plataforma.

C) frontend se torna o responsável pela chamada direta de cada microsserviço individualmente.

D) Gateway API reduz o número de chamadas externas que o frontend precisa fazer, tornando a comunicação mais eficiente.

E) frontend deve gerenciar manualmente as atualizações de endpoint de cada serviço individual registrado.


**17.** (FCC | TRF4 2025 | Técnico Judiciário, Área Apoio Especializado, Especialidade Desenvolvimento de Sistema da Informação | Questão 38 de 50)

Durante a modernização de sistemas de um tribunal, uma equipe técnica adotou arquiteturas distribuídas com microsserviços, utilizando APIs para comunicação entre componentes. Para garantir escalabilidade, segurança e gerenciamento eficiente das comunicações, o ambiente de produção também incorpora API Gateway, autenticação, Service Discovery (Eureka) e proxy reverso (Nginx ou HAProxy). Considerando essas práticas de arquitetura, a estruturação adequada da solução deve garantir que

A) a comunicação entre microsserviços seja realizada por meio de rotas fixas e predefinidas, simplificando a topologia da rede em ambientes estáticos.

B) as APIs REST utilizem tokens JWT para autenticação e autorização, fortalecendo a segurança na troca de mensagens.

C) o API Gateway concentre o controle de entrada, isentando da necessidade de acoplar funções de autenticação, roteamento dinâmico ou controle de taxa de requisições.

D) a arquitetura cliente-servidor seja mantida em serviços intermediários como padrão de comunicação entre microsserviços, garantindo isolamento lógico.

E) o proxy reverso atue prioritariamente na camada de encaminhamento de requisições HTTP, sem interferência direta no balanceamento de carga ou caching de respostas.


**18.** (FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Em um cenário de sistema de microsserviços um técnico deseja expor alguns dos microsserviços para acesso externo e ocultar outros. Como ele está usando o Spring Cloud, para proteger os microsserviços expostos contra solicitações de clientes mal-intencionados, ele pode usar o

A) Galaxy Eureka como edge server.

B) Apache Toncat como service discovery.

C) Apache Wildfly como security server.

D) Netflix Zuul como edge server.

E) Swagger como security server.


**19.** (FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Para fazer um aplicativo criado com Spring Boot, em condições ideais, funcionar como um servidor Zuul Proxy deve-se anotar a classe principal com

A) `@EnableZuulProxy`

B) `@ZuulServerApplication`

C) `@EnabledZuulEdgeServer`

D) `@ZuulEdgeServerOn`

E) `@ZuulProxyApplication`


**20.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22) *[aproximação temática]*

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.

B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.

C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.

D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.

E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.


**21.** (FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60) *[aproximação temática]*

O Spring Eureka é uma implementação do padrão de registro de serviços que permite que os microsserviços se registrem automaticamente e se descubram uns aos outros. A porta padrão em que o servidor Eureka irá executar na aplicação Spring Boot e o arquivo em que essa porta é configurada são, respectivamente,

A) `80` e `eureka-server.xml`

B) `15000` e `application-server.yml`

C) `8080` e `pom.xml`

D) `8005` e `eureka-registry.xml`

E) `8761` e `application.properties`


**22.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60) *[aproximação temática]*

Correspondem, correta e respectivamente, a um componente do Spring Cloud e à principal função do Spring Cloud Config:

A) Spring Security — implementar um padrão de design para a comunicação entre serviços baseados em REST.

B) Spring Boot Actuator — gerenciar o ciclo de vida e o monitoramento de aplicações distribuídas em uma arquitetura de microsserviços.

C) Spring Data JPA — oferecer uma solução para o balanceamento de carga entre instâncias de microsserviços.

D) Spring Cloud Stream — centralizar a configuração de aplicações e serviços, permitindo que configurações sejam atualizadas dinamicamente.

E) Spring Web MVC — fornecer um serviço de autenticação e autorização para microsserviços e aplicações.


**23.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 42 de 60) *[aproximação temática]*

Uma aplicação envia ao advogado uma notificação avisando que houve uma atualização em um processo, mesmo sem o advogado abrir o aplicativo para procurar a informação. Para que a informação chegue até o sistema do aplicativo do Tribunal em tempo real e até o advogado, da forma relatada, sem a necessidade de ação externa, pode ser utilizado um recurso conhecido como

A) Swagger.

B) Webhook.

C) Mockito Pulling.

D) Eureka Messages.

E) Elastic Messages.


**24.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60) *[aproximação temática]*

A autenticação na Plataforma Digital do Poder Judiciário PDPJ-Br ocorre por meio de uma solução A, baseada em um aplicativo open source de gestão de identidades e acesso, denominado B, com base no protocolo C. De acordo com o texto acima, A, B e C são, correta e respectivamente:

A) Keycloak – Docker – Simple Object Access Protocol (SOAP)

B) Single Sign-On (SSO) – Keycloak – OAuth2 (RFC 6749)

C) Single Sign-On (SSO) – RabbitMQ – Advanced Message Queuing Protocol (AMQP)

D) de access token – Keycloak – Simple Object Access Protocol (SOAP)

E) Keycloak – RabbitMQ – OAuth2 (RFC 4967)


**25.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 21) *[aproximação temática]*

De acordo com a documentação da Plataforma Digital do Poder Judiciário – PDPJ-Br, para que uma API seja considerada do tipo RESTful, ela precisa

A) ter uma arquitetura cliente/servidor formada por clientes e servidores, com solicitações gerenciadas por meio dos protocolos HTTP e FTP.

B) estabelecer uma comunicação statefull entre cliente e servidor, ou seja, as solicitações não são separadas e/ou desconectadas.

C) armazenar dados em cache para otimizar as interações entre cliente e servidor.

D) ter uma interface diferente para cada componente para que as informações possam ser transferidas em diferentes formatos.

E) ter um sistema sem camadas que organiza os tipos de servidores envolvidos na recuperação das informações solicitadas sem hierarquias.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **D** | FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60 |
| 17 | **B** | FCC | TRF4 2025 | Técnico Judiciário, Área Apoio Especializado, Especialidade Desenvolvimento de Sistema da Informação | Questão 38 de 50 |
| 18 | **D** | FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 19 | **A** | FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 20 | **E** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22 |
| 21 | **E** \* | FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60 |
| 22 | **D** \* | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 23 | **B** \* | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 42 de 60 |
| 24 | **B** \* | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60 |
| 25 | **C** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 21 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q39
- Q17: https://www.quizinho.com.br/questao/fcc-trf4-2025-tecnico-judiciario-area-apoio-especializado-especialidade-desenvolvimento-de-sistema-da-informacao-2025-q38
- Q18: https://www.quizinho.com.br/questao/fcc-trt4-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q38
- Q19: https://www.quizinho.com.br/questao/fcc-trt23-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q38
- Q20: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22
- Q21: https://www.quizinho.com.br/questao/fcc-trt15-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q31
- Q22: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q53
- Q23: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q42
- Q24: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q28
- Q25: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q21

*Arquivo 04 · API Gateway · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 05 Service Discovery / Eureka.*

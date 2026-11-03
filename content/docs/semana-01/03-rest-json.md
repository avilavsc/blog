+++
title = '03 — REST / JSON'
date = '2026-10-03'
draft = false
weight = 3
tags = ['semana-01', 'pdpj', 'rest']
categories = ['estudo']
+++
# REST / JSON · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — API REST / JSON  
**Escopo deste arquivo:** estilo arquitetural REST, recursos e URI, verbos HTTP, códigos de status, restrição *stateless*, JSON como representação, contraste JSON × XML e armadilhas típicas da FCC neste eixo.  
**Fora de escopo aqui:** API Gateway/Zuul, Eureka, Swagger/OpenAPI em profundidade, MapStruct, microsserviços como tema completo (arquivos 02, 04–09).

---

## 1. Material

### 1.1 O que é REST (leitura de prova)

**REST** (*Representational State Transfer*) é um **estilo arquitetural** para sistemas distribuídos, tipicamente sobre **HTTP**. Em prova FCC/PDPJ, cobre-se como: interação centrada em **recursos** identificados por **URI**, manipulados por **verbos HTTP**, com **representação** no corpo (quase sempre **JSON**) e servidor **sem estado de sessão** entre requisições (*stateless*).

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Organização | Recursos + URI + verbos HTTP |
| Contrato de resultado | Status HTTP + corpo (JSON) |
| Escala / simplicidade | Comunicação *stateless* |

REST **não** é: um framework Java, um produto Spring, um protocolo binário proprietário, nem sinônimo de “qualquer Web Service” (SOAP/WSDL é outro eixo).

### 1.2 Recursos e URI

Em REST, a unidade lógica é o **recurso** (processo, parte, documento, usuário etc.). A **URI** (*Uniform Resource Identifier*) **identifica** o recurso; o **verbo** diz **o que fazer** com ele.

Padrões cobráveis de modelagem:

| Ideia | Exemplo típico | Leitura |
|-------|----------------|---------|
| Coleção | `/processos` | Lista / criação na coleção |
| Item | `/processos/123` | Um recurso específico |
| Sub-recurso | `/processos/123/partes` | Relação aninhada |
| Query | `/processos?situacao=ativo` | Filtro/consulta (não muda o “tipo” do recurso) |

Boas práticas cobradas em enunciados (não decorar RFC linha a linha):

- Preferir **substantivos** na URI (`/processos`), não verbos de ação (`/criarProcesso`, `/getProcesso`).
- A **ação** vai no **método HTTP**; a URI nomeia **o quê**.
- Hierarquia e IDs espelham a identificação do recurso, não a lógica de negócio escondida em path “RPC-like”.

Armadilha: confundir **URI do recurso** com **payload**. O ID na URI identifica; o JSON descreve o estado/representação.

### 1.3 Verbos HTTP (métodos)

Tabela-núcleo para FCC:

| Verbo | Uso típico em REST | Idempotente? | Corpo request comum | Sucesso típico |
|-------|--------------------|--------------|---------------------|----------------|
| **GET** | Consultar / ler recurso ou coleção | Sim | Em geral não | `200` |
| **POST** | Criar recurso (na coleção) ou disparar operação não idempotente | Não | Sim | `201` (criação) ou `200`/`202` |
| **PUT** | Substituir / atualizar recurso completo (ou criar em URI conhecida, em alguns desenhos) | Sim | Sim | `200` / `204` |
| **PATCH** | Atualização **parcial** | Em geral sim* | Sim (parcial) | `200` / `204` |
| **DELETE** | Remover recurso | Sim | Opcional | `200` / `204` |

\*Em prova: foque no contraste **PUT = representação completa** × **PATCH = parcial**. Não entre em debate filosófico de RFC salvo se o enunciado exigir.

Flash de associação rápida:

- **GET** → “quero ver”
- **POST** → “quero criar / submeter”
- **PUT** → “quero colocar este recurso inteiro neste endereço”
- **PATCH** → “quero alterar só estes campos”
- **DELETE** → “quero remover”

**Idempotência (conceito cobrável):** repetir a mesma requisição produz o **mesmo efeito** no recurso (não “dobra” a criação). GET, PUT e DELETE são tipicamente tratados como idempotentes; **POST de criação** não é. A FCC gosta de distrair com “GET altera banco” ou “POST é sempre idempotente”.

**Segurança no sentido REST (safe):** métodos *safe* não devem alterar estado no servidor — **GET** é o exemplo clássico. Não confundir *safe* com HTTPS/TLS (segurança de transporte é outro eixo).

### 1.4 Códigos de status HTTP

O status comunica o **resultado da operação**, independente do texto do JSON. Memorize faixas + códigos “estrela” de prova:

| Faixa | Significado | Códigos estrela |
|-------|-------------|-----------------|
| **2xx** | Sucesso | `200` OK · `201` Created · `204` No Content |
| **3xx** | Redirecionamento | (menos cobrado no núcleo REST do edital) |
| **4xx** | Erro do **cliente** | `400` Bad Request · `401` Unauthorized · `403` Forbidden · `404` Not Found · `409` Conflict |
| **5xx** | Erro do **servidor** | `500` Internal Server Error · `503` Service Unavailable |

Associação verbo ↔ status que a banca explora:

| Situação | Status esperado |
|----------|-----------------|
| GET bem-sucedido | `200` + representação |
| POST criou recurso | `201` (+ frequentemente `Location` com URI do novo recurso) |
| DELETE/PUT sem corpo de resposta | `204` possível |
| JSON malformado / validação | `400` |
| Sem autenticação / credencial inválida | `401` |
| Autenticado, mas sem permissão | `403` |
| Recurso inexistente | `404` |
| Conflito de estado (ex.: duplicidade) | `409` |
| Falha interna não atribuível ao cliente | `500` |

Armadilha clássica: tratar **401** e **403** como sinônimos. Em prova:

- **401** → problema de **autenticação** (quem é você?).
- **403** → autenticado, mas **autorização** negada (você não pode).

Outra: dizer que todo erro de API REST “é sempre 500” — mistura falha de cliente com falha de servidor.

### 1.5 Stateless (sem estado de sessão no servidor)

Restrição REST cobrada: cada requisição carrega **tudo o que o servidor precisa** para processá-la (credenciais/token, identificadores, dados). O servidor **não depende** de “sessão grudada” (*sticky session* clássica) entre chamadas para lembrar o contexto da conversa anterior.

Implicações cobráveis:

- Facilita **escala horizontal** (qualquer réplica pode atender o próximo request).
- Autenticação tipicamente via **token** (ex.: Bearer) ou credenciais no request — detalhe OAuth/Keycloak fica em outro eixo do edital; aqui basta o princípio.
- **Não** significa “API sem autenticação” nem “sem banco de dados”. *Stateless* refere-se ao **estado de sessão da conversa HTTP**, não à ausência de persistência de negócio.

Contraste útil:

| Com sessão server-side clássica | REST *stateless* |
|---------------------------------|------------------|
| Servidor guarda conversa entre requests | Cada request é autocontido |
| Afinidade de instância mais crítica | Qualquer nó pode atender |
| “Lembra” do passo anterior na sessão | Cliente reenvia o necessário |

### 1.6 Representação: JSON

**JSON** (*JavaScript Object Notation*) é o formato de **representação** mais associado às APIs REST da stack PDPJ/edital: leve, textual, baseado em objetos/arrays/valores, tipicamente `Content-Type: application/json`.

Características cobráveis:

- Estruturas: objeto `{ }`, array `[ ]`, string, número, boolean, `null`.
- Usado em **request** e **response**.
- Independente de linguagem (não é “só JavaScript em runtime”).
- Mais **enxuto** e legível para APIs web modernas do que XML verboso típico de SOAP.

JSON **não** é: o protocolo de transporte (isso é HTTP), o discovery (Eureka), o gateway (Zuul), nem a documentação interativa (Swagger).

### 1.7 JSON × XML (contraste FCC)

| Aspecto | JSON | XML |
|---------|------|-----|
| Verbosidade | Em geral mais compacto | Mais verboso (tags de abertura/fechamento) |
| Tipagem / schema | Schema opcional (JSON Schema etc.); prova raramente exige detalhe | XSD / XML Schema fortemente associados |
| Ecossistema clássico | APIs REST modernas | SOAP, documentos, integrações legadas |
| Atributos / namespaces | Modelo mais simples (sem atributos XML nativos) | Atributos, namespaces, hierarquia rica |
| Uso no discurso PDPJ/edital | Payload padrão REST | Presente no edital (XML/XSD), mas **não** como sinônimo de API RESTful da stack Spring |

Em enunciados mistos: se falar **API RESTful + microsserviços Spring da PDPJ**, o caminho padrão é **JSON**. Se falar **XML Schema / documentos estruturados**, é outro bloco do edital — não force REST+JSON onde o tema é XML puro.

### 1.8 REST × SOAP (anti-confusão rápida)

Só o necessário para não cair em pegadinha:

| | REST (HTTP + JSON) | SOAP |
|--|--------------------|------|
| Estilo | Recursos + verbos HTTP | Envelope XML / operações |
| Contrato típico | URI + método + status (+ OpenAPI/Swagger em outro arquivo) | WSDL |
| Payload usual no edital PDPJ | JSON | XML |
| Sensação de prova | “leve”, web, microsserviços | “pesado”, enterprise clássico |

Não precisa virar especialista em SOAP aqui; basta **não** dizer que REST exige WSDL ou que JSON é envelope SOAP.

### 1.9 O que a FCC tipicamente testa neste tema

- Recurso identificado por **URI**; ação pelo **verbo**.
- Mapa **GET/POST/PUT/PATCH/DELETE** ↔ consulta/cria/substitui/parcial/remove.
- **201** em criação; **404** recurso ausente; **401≠403**; **4xx** cliente × **5xx** servidor.
- **Stateless** = request autocontido; **não** = “sem banco”.
- **JSON** como representação padrão de APIs REST no eixo PDPJ.
- Contraste **JSON × XML** sem transformar um no outro.
- Armadilhas: URI com verbo de ação; GET que “cria”; POST sempre idempotente; misturar REST com Eureka/Zuul/Swagger; achar que REST exige sessão sticky.

### 1.10 Armadilhas FCC (lista de ouro)

1. Colocar a ação na URI (`/api/criarUsuario`) e ignorar o método HTTP.
2. Afirmar que REST **exige** sessão de servidor entre requests.
3. Trocar **401** por **403** (ou vice-versa).
4. Dizer que **POST** é idempotente como regra.
5. Tratar **PUT** e **PATCH** como idênticos (completo × parcial).
6. Confundir **JSON** (formato) com **HTTP** (protocolo) ou com **REST** (estilo).
7. Afirmar que API REST da PDPJ “só fala XML/SOAP”.
8. Usar **500** para erro de validação do cliente (deveria ser **400**).
9. Confundir *stateless* com “API pública sem autenticação”.
10. Misturar papéis: Swagger documenta; Eureka descobre; Zuul é gateway — **nenhum deles substitui** o entendimento de verbos/URI/status/JSON.

### 1.11 Mini-síntese (cartão mental)

> **REST** = recursos em **URI** + **verbos HTTP** + **status** + representação (**JSON**) + servidor **stateless**.  
> **GET** lê · **POST** cria · **PUT** troca inteiro · **PATCH** parcial · **DELETE** remove.  
> **2xx** ok · **4xx** cliente · **5xx** servidor · **401≠403** · JSON ≠ XML/SOAP.

---

## 2. Questões de treino (1–15)

**1.** No estilo arquitetural REST cobrado em provas de TI do Judiciário, a organização típica da interação é:

A) Operações nomeadas em envelope SOAP obrigatório, sem uso de URI  
B) Recursos identificados por URI, manipulados por métodos HTTP, com representação no corpo  
C) Chamadas exclusivas por stored procedure no banco, sem HTTP  
D) Troca apenas de arquivos binários proprietários, sem status HTTP  
E) Sessão sticky obrigatória em todo endpoint como definição de REST  

**2.** Em uma API RESTful, a URI `/processos/45` representa, em regra:

A) O verbo HTTP que cria o processo 45  
B) A identificação de um recurso específico (processo de id 45)  
C) O código de status de sucesso da operação  
D) O formato JSON Schema nacional obrigatório  
E) O serviço de discovery Eureka  

**3.** Assinale a associação mais adequada entre verbo HTTP e intenção:

A) GET — criar recurso novo na coleção  
B) DELETE — consultar representação sem efeito colateral esperado  
C) POST — criar recurso (tipicamente na coleção)  
D) PATCH — remover definitivamente o recurso  
E) PUT — apenas autenticar o cliente, sem alterar recurso  

**4.** Sobre idempotência no contexto REST de prova, é correto afirmar que:

A) POST de criação é o exemplo clássico de método sempre idempotente  
B) GET, PUT e DELETE são tipicamente tratados como idempotentes; POST de criação, não  
C) Nenhum método HTTP pode ser idempotente  
D) Apenas PATCH é idempotente por definição absoluta em todos os casos de prova  
E) Idempotência significa “usar XML em vez de JSON”  

**5.** Um cliente envia `POST /partes` com JSON válido e o servidor cria o registro. O status de sucesso **mais alinhado** à criação é:

A) `404`  
B) `401`  
C) `201`  
D) `503`  
E) `500`  

**6.** Diferencie corretamente `401` e `403`:

A) São sinônimos absolutos em HTTP  
B) `401` relaciona-se a autenticação; `403` a autorização (permissão)  
C) `403` significa recurso inexistente; `401` significa erro interno  
D) Ambos indicam exclusivamente falha de DNS  
E) `401` é sucesso parcial; `403` é criação  

**7.** A restrição *stateless* em REST significa, em essência:

A) Que a API não pode persistir dados de negócio em banco  
B) Que cada requisição deve ser autocontida, sem depender de sessão de conversa no servidor entre chamadas  
C) Que é proibido usar HTTPS  
D) Que JSON é ilegal  
E) Que todo endpoint deve manter sticky session obrigatória  

**8.** JSON, no discurso típico de APIs REST da stack cobrada (PDPJ/Spring), é melhor descrito como:

A) Protocolo de transporte que substitui o HTTP  
B) Formato de representação/payload textual para request e response  
C) Ferramenta de service discovery  
D) API Gateway da Netflix  
E) Mecanismo de documentação OpenAPI  

**9.** No contraste JSON × XML para enunciados de API REST moderna, a leitura mais coerente é:

A) JSON costuma ser mais compacto e é o payload padrão associado a REST no eixo PDPJ; XML permanece relevante em outros contextos (ex.: documentos/SOAP), sem ser sinônimo de REST+JSON  
B) XML e JSON são protocolos de roteamento de gateway  
C) REST exige XML Schema em todo endpoint Spring Boot  
D) JSON só funciona dentro do navegador e não em APIs servidor  
E) XML substitui obrigatoriamente JSON em toda API RESTful do edital  

**10.** Qual modelagem de URI está **mais alinhada** ao estilo REST orientado a recursos?

A) `POST /criarProcesso`  
B) `GET /obterListaDePartesAgora`  
C) `GET /processos` e `POST /processos`  
D) `POST /executarAcaoDeNegocioGenerica`  
E) `GET /deleteProcesso?id=1`  

**11.** Um enunciado afirma: “Em REST, PUT atualiza a representação completa do recurso na URI; PATCH aplica alteração parcial.” Essa afirmação é:

A) Incorreta, porque PUT e PATCH são sempre idênticos  
B) Correta no nível cobrado em concursos  
C) Incorreta, porque apenas DELETE atualiza recursos  
D) Correta somente se o payload for XML  
E) Incorreta, porque GET também atualiza o recurso por definição  

**12.** Cliente autenticado tenta `DELETE /processos/9`, mas a política de autorização nega a operação. O status mais adequado entre as opções é:

A) `201`  
B) `403`  
C) `200` com criação implícita  
D) `204` de criação  
E) `301` de payload JSON  

**13.** Assinale a afirmação **incorreta**:

A) `404` indica recurso não encontrado  
B) `400` costuma indicar requisição inválida (ex.: JSON malformado/validação)  
C) `500` aponta falha no lado do servidor  
D) `201` é o status típico de criação bem-sucedida  
E) `401` significa, em regra, que o recurso foi criado com sucesso  

**14.** Em arquitetura de microsserviços alinhada ao edital, a comunicação REST + JSON entre serviços caracteriza-se por:

A) Obrigar WSDL em toda chamada  
B) Usar HTTP com recursos/verbos/status e corpo JSON, sem confundir isso com o papel do Eureka ou do Zuul  
C) Proibir códigos de status HTTP  
D) Exigir sessão sticky como condição de conformidade REST  
E) Substituir URI por nomes de stored procedures  

**15.** Qual alternativa melhor resume o núcleo cobrável deste subtópico?

A) Eureka documenta OpenAPI; JSON é gateway; REST exige SOAP  
B) Recursos/URI + verbos HTTP + status + JSON + *stateless*; sem misturar com discovery/gateway/docs  
C) MapStruct define verbos HTTP; XML é o único payload REST  
D) REST = sticky session + ausência de URI  
E) Status `2xx` indica sempre erro do cliente  

---

### Gabarito comentado (treino 1–15)

1. **B** — Recursos + URI + verbos + representação; A é SOAP; E nega *stateless*.  
2. **B** — URI identifica o recurso; verbo/status/formato são outras dimensões.  
3. **C** — POST cria (tipicamente); GET não cria; DELETE não consulta.  
4. **B** — Idempotência clássica: GET/PUT/DELETE sim; POST de criação não.  
5. **C** — Criação → `201`.  
6. **B** — 401 autenticação × 403 autorização.  
7. **B** — Request autocontido; não “sem banco”.  
8. **B** — JSON = formato de payload/representação.  
9. **A** — JSON padrão REST no eixo PDPJ; XML em outros contextos.  
10. **C** — Substantivos na URI; ação no verbo.  
11. **B** — PUT completo × PATCH parcial — contraste padrão de prova.  
12. **B** — Autenticado sem permissão → `403`.  
13. **E** — `401` não é sucesso de criação.  
14. **B** — HTTP+JSON; Eureka/Zuul são outros papéis.  
15. **B** — Cartão mental do arquivo 03.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60)

De acordo com a Plataforma Digital do Poder Judiciário - PDPJ-Br, para que uma API seja considerada do tipo RESTful, ela precisa estar em conformidade, dentre outros, com o seguinte critério:

A) ter uma arquitetura cliente/servidor formada por clientes, servidores e recursos, com solicitações gerenciadas por meio de HTTP.

B) possuir o sistema de comunicação entre os componentes por meio de troca de mensagens REST-Enq, fazendo uso de um Message Broker. No caso da PDPJ, utiliza-se a solução BrokerMQ.

C) ter uma interface ampla e livre entre os componentes, desde que sejam usados os parâmetros RESTful Config para que as informações sejam transferidas em um formato padronizado.

D) ter autenticação por meio de uma solução de RFPJ Sign On baseada no protocolo PDPJ Auth2.

E) permitir que as APIs registradas sejam acessíveis por meio do Gateway RESTful-API-Sync.


**17.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 21)

De acordo com a documentação da Plataforma Digital do Poder Judiciário – PDPJ-Br, para que uma API seja considerada do tipo RESTful, ela precisa

A) ter uma arquitetura cliente/servidor formada por clientes e servidores, com solicitações gerenciadas por meio dos protocolos HTTP e FTP.

B) estabelecer uma comunicação statefull entre cliente e servidor, ou seja, as solicitações não são separadas e/ou desconectadas.

C) armazenar dados em cache para otimizar as interações entre cliente e servidor.

D) ter uma interface diferente para cada componente para que as informações possam ser transferidas em diferentes formatos.

E) ter um sistema sem camadas que organiza os tipos de servidores envolvidos na recuperação das informações solicitadas sem hierarquias.


**18.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 34 de 60)

Ao implementar operações de manipulação de dados na API RESTful da PDPJ-Br, uma desenvolvedora deseja alterar apenas alguns atributos de um processo existente, sem substituir todos os atributos da entidade. Considerando as diretrizes para o uso de verbos HTTP da PDPJ-Br, o verbo HTTP mais adequado para essa operação é o

A) PATCH.

B) UPDATE.

C) GET.

D) POST.

E) PUT.


**19.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60)

Na abordagem do Spring para construir serviços web RESTful, as solicitações HTTP são tratadas por um controlador, que é uma classe identificada com a anotação

A) `@RestfulController`

B) `@RestfulMapping`

C) `@RestApplicationController`

D) `@RestController`

E) `@RequestController`


**20.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 25)

Ao tentar acessar um serviço por meio de uma requisição a uma API RESTful, a aplicação cliente recebeu como resposta o código HTTP `500`. Com base nas convenções do protocolo HTTP, esse código indica que

A) a requisição foi malformada ou continha parâmetros inválidos, e o servidor não conseguiu processá-la.

B) o recurso solicitado não foi encontrado no servidor.

C) a requisição foi bem-sucedida, mas o servidor optou por não retornar nenhum conteúdo.

D) o servidor recebeu a requisição, mas não tem autorização para executá-la.

E) ocorreu um erro interno no servidor que o impediu de concluir a requisição.


**21.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 27 de 60) *[aproximação temática]*

*Documentação OpenAPI/Swagger de API REST.*

Uma Analista está desenvolvendo uma API REST para um aplicativo e deseja criar a documentação usando o Swagger. Essa documentação é criada adequadamente através

A) do Swagger Codegen, que permite a criação manual da especificação OpenAPI.

B) da interface Swagger Editor, que permite criar a especificação OpenAPI interativamente.

C) da elaboração de um arquivo de documentação PDF com o Swagger PDF Editor contendo a descrição das APIs.

D) da interface Swagger GetJet, que cria e exibe automaticamente a documentação.

E) da elaboração de um arquivo XML de forma manual, contendo a especificação OpenDocumentAPI.


**22.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60) *[aproximação temática]*

Um Analista de TI está desenvolvendo uma API com Spring Boot e deseja integrar Swagger para facilitar a documentação e testes dessa API. Para integrar no Swagger com um projeto Spring Boot a abordagem mais adequada é

A) adicionar as dependências do Swagger manualmente no classpath do projeto.

B) adicionar as dependências do Swagger no arquivo `pom.xml` ou `build.gradle` do projeto Spring Boot.

C) criar um arquivo separado para todas as configurações do Swagger sem importar as dependências no projeto.

D) utilizar um plugin externo para o IDE que automaticamente gera a documentação do Swagger sem configuração adicional.

E) configurar o servidor de aplicação para disponibilizar a documentação do Swagger sem modificar o código do projeto.


**23.** (FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60) *[aproximação temática]*

*JSON no ecossistema REST.*

Em um código Swagger escrito em JSON, a URL Base é formada por

A) host, baseForms e schemes.

B) consumes, host e produces.

C) host, basePath e schemes.

D) produces, schemes e baseForms.

E) application, get e basePath.


**24.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60) *[aproximação temática]*

Ao usar o Gateway API com o Service Discovery no contexto da PDPJ-Br, a principal consequência para o frontend em uma aplicação baseada em microsserviços é que o

A) Gateway API diminui a necessidade de autenticação, uma vez que o Service Discovery é integrado ao SSO.

B) frontend deve configurar manualmente os IPs e portas dos serviços disponíveis na plataforma.

C) frontend se torna o responsável pela chamada direta de cada microsserviço individualmente.

D) Gateway API reduz o número de chamadas externas que o frontend precisa fazer, tornando a comunicação mais eficiente.

E) frontend deve gerenciar manualmente as atualizações de endpoint de cada serviço individual registrado.


**25.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60) *[aproximação temática]*

Nos projetos da Plataforma Digital do Poder Judiciário PDPJ-Br,

A) os ambientes de deployment estão instalados em clusters Kubernetes, gerenciados por meio da ferramenta open source de orquestração de containers Rancher.

B) a aplicação deve possuir os arquivos `yanl`, para que o deploy ocorra de forma automática por meio de pipelines de CI/CD (Continuous Delivery e Continuous Integration) executados pelo GitLab do STF (Supremo Tribunal Federal).

C) a aplicação deve possuir um arquivo `Rancherfile`, especificando o modo pelo qual ela deve ser empacotada.

D) deve haver o arquivo `.gitlab-ci.xml` definindo a pipeline de deploy automático.

E) deve haver, na pasta `kubernets`, o arquivo de configuração do ingress da aplicação, que é obrigatório.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **A** | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60 |
| 17 | **C** | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 21 |
| 18 | **A** | FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 34 de 60 |
| 19 | **D** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60 |
| 20 | **E** | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 25 |
| 21 | **B** \* | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 27 de 60 |
| 22 | **B** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 23 | **C** \* | FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 24 | **D** \* | FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60 |
| 25 | **A** \* | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q52
- Q17: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q21
- Q18: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q34
- Q19: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q31
- Q20: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q25
- Q21: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q27
- Q22: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q53
- Q23: https://www.quizinho.com.br/questao/fcc-trt4-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q53
- Q24: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q39
- Q25: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q29

*Arquivo 03 · REST / JSON · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 04 API Gateway.*

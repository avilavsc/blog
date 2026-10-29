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

## 3. Questões estilo concurso FCC (16–25)

**16.** Em APIs projetadas segundo o estilo REST, é correto afirmar que:

A) os recursos são identificados por URI e as operações são expressas, em regra, pelos métodos do protocolo HTTP  
B) a conformidade REST exige, necessariamente, o uso de envelope SOAP e contrato WSDL em todas as operações  
C) a ausência de códigos de status HTTP é condição essencial do estilo  
D) a URI deve conter, obrigatoriamente, o verbo da ação de negócio em linguagem natural, em substituição ao método HTTP  
E) o formato JSON é um protocolo de transporte que dispensa o HTTP  

**17.** Acerca dos métodos HTTP empregados em APIs RESTful, assinale a alternativa correta.

A) O método GET destina-se, em regra, à criação de novos recursos na coleção  
B) O método POST é tipicamente utilizado para criação de recursos e, em geral, não é tratado como idempotente  
C) O método DELETE é reservado exclusivamente à autenticação do cliente  
D) O método PUT serve apenas para consulta de coleções, sem alterar estado  
E) O método PATCH remove o recurso e impede qualquer atualização parcial  

**18.** No contexto de respostas HTTP de uma API REST, o código `201` associa-se mais diretamente a:

A) falha interna não especificada do servidor  
B) ausência de autenticação do cliente  
C) criação bem-sucedida de um recurso  
D) recurso inexistente na URI informada  
E) redirecionamento permanente de DNS  

**19.** Com relação à restrição de comunicação sem estado (*stateless*) no estilo REST, é correto concluir que:

A) o servidor não deve depender de sessão de conversa mantida entre requisições, cabendo a cada chamada carregar as informações necessárias ao processamento  
B) fica vedada qualquer persistência de dados de negócio em sistemas de banco de dados  
C) a autenticação do cliente torna-se tecnicamente impossível  
D) a restrição obriga o uso exclusivo de XML em substituição ao JSON  
E) a conformidade REST exige sticky session em todas as instâncias do serviço  

**20.** Considere as afirmações abaixo sobre JSON e XML em cenários de integração.

I. JSON é amplamente utilizado como formato de representação em APIs REST.  
II. XML, embora relevante em diversos contextos (incluindo paradigmas legados e documentos), não se confunde com a definição do estilo REST baseado em recursos/HTTP.  
III. Em APIs RESTful do discurso técnico associado à stack da PDPJ-Br, o JSON figura como payload típico de request/response.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e III, apenas  
D) II e III, apenas  
E) I, II e III  

**21.** Assinale a opção que apresenta interpretação **incompatível** com boas práticas de modelagem REST cobradas em concursos.

A) Utilizar substantivos para nomear recursos na URI  
B) Empregar o método HTTP para expressar a ação sobre o recurso  
C) Retornar códigos de status coerentes com o resultado da operação  
D) Substituir sistematicamente o método HTTP por caminhos do tipo `/criar`, `/atualizar` e `/deletar` como única forma de expressar a operação  
E) Adotar JSON como representação usual do estado do recurso  

**22.** Um cliente não envia credenciais ao acessar endpoint protegido e recebe resposta de falha. O código HTTP que melhor expressa problema de **autenticação**, dentre os abaixo, é:

A) `201`  
B) `204`  
C) `401`  
D) `200`  
E) `301`  

**23.** Diferenciar `PUT` e `PATCH` em prova de Analista de TI é relevante porque:

A) ambos são protocolos distintos que substituem o HTTP  
B) `PUT` associa-se, em regra, à substituição/atualização da representação completa; `PATCH`, à alteração parcial  
C) `PATCH` é sinônimo estrito de `GET`  
D) `PUT` serve apenas para apagar coleções inteiras  
E) nenhum dos dois métodos pode enviar corpo JSON  

**24.** Em enunciado que descreve falha causada por JSON inválido enviado pelo consumidor da API, o status mais coerente entre as opções é:

A) `500`, por ser sempre o único código permitido em APIs REST  
B) `201`, pois todo JSON inválido cria recurso  
C) `400`, por se tratar de erro atribuível à requisição do cliente  
D) `403`, por significar criação parcial  
E) `204`, por indicar autenticação bem-sucedida  

**25.** Para fins de preparação no eixo API RESTful / JSON do edital (PDPJ-Br / Analista TI), a síntese mais adequada é:

A) REST resume-se a sticky session + WSDL + proibição de status HTTP  
B) o núcleo cobrável articula recursos/URI, verbos HTTP, códigos de status, comunicação *stateless* e JSON como representação típica, distinguindo-se de XML/SOAP e de papéis como gateway, discovery e documentação  
C) JSON substitui o HTTP e elimina a necessidade de URI  
D) `401` e `403` são indistinguíveis e equivalem a sucesso de criação  
E) a modelagem REST exige caminhos `/get` e `/post` na URI em substituição aos métodos HTTP  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **A** |
| 17 | **B** |
| 18 | **C** |
| 19 | **A** |
| 20 | **E** |
| 21 | **D** |
| 22 | **C** |
| 23 | **B** |
| 24 | **C** |
| 25 | **B** |

**Notas rápidas:** 16 A (URI+métodos); 17 B (POST cria, não idempotente); 18 C (`201` criação); 19 A (*stateless* = request autocontido); 20 E (I–III corretas); 21 D (ação na URI no lugar do verbo = anti-padrão); 22 C (`401` autenticação); 23 B (PUT completo × PATCH parcial); 24 C (JSON inválido → `400`); 25 B (síntese do subtópico).

---

*Arquivo 03 · REST / JSON · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 04 API Gateway.*

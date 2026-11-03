+++
title = '09 — MapStruct'
date = '2026-10-09'
draft = false
weight = 9
tags = ['semana-01', 'pdpj', 'mapstruct']
categories = ['estudo']
+++
# MapStruct · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 09 — MapStruct  
**Escopo deste arquivo:** MapStruct como **mapper compile-time** entre objetos Java (**Entity ↔ DTO** e afins), geração de código no build, por que aparece em **APIs REST / microsserviços**, contraste com **mapeamento manual** e com **Swagger/OpenAPI**.  
**Fora de escopo aqui:** documentação OpenAPI em profundidade (arquivo 08), Spring Boot × Cloud (07), Eureka/Zuul (05–06), REST/JSON como tema completo (03), JPA/Hibernate/Flyway.

---

## 1. Material

### 1.1 O que é MapStruct (leitura de prova)

**MapStruct** é um **gerador de código** (annotation processor) que, em **tempo de compilação**, produz implementações de mapeamento entre beans Java — tipicamente **Entity ↔ DTO** (também records, value objects, etc.).

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | **Mapper** entre modelos de objeto (Entity ↔ DTO) |
| Quando gera | **Compile-time** (no build), não “mágica” por reflexão em runtime |
| O que **não** é | Documentação de API (Swagger), discovery, gateway, ORM |

Flash: enunciado fala em **mapear Entity/DTO**, **código gerado na compilação**, **evitar getters/setters manuais em massa** → **MapStruct**.

MapStruct **não** é: Swagger/OpenAPI, Eureka, Zuul, Hibernate, Flyway, “o Spring Boot”.

### 1.2 Entity × DTO — por que mapear?

Em APIs e microsserviços, raramente se expõe a **entidade de persistência** crua na borda HTTP.

| Conceito | Papel cobrável |
|----------|----------------|
| **Entity** | Modelo ligado à **persistência** (JPA/Hibernate, tabelas, relações) |
| **DTO** | Modelo de **troca** (request/response da API; contrato externo) |
| **Mapper** | Traduz um no outro sem misturar camadas |

Por quê separar?

1. **Contrato estável** da API ≠ schema do banco (campos internos, lazy loads, auditoria).  
2. **Segurança / encapsulamento**: não vazar senha, flags internas, grafos profundos.  
3. **Evolução independente**: DTO da API pode mudar sem arrastar a Entity (e vice-versa).  
4. Em **microsserviços**, cada serviço tem seu modelo; DTO é a face da fronteira.

Sem mapper (manual ou gerado), o serviço ainda precisa **copiar** campos Entity→DTO e DTO→Entity. MapStruct **automatiza** essa cópia de forma tipada.

### 1.3 Compile-time: o diferencial cobrável

MapStruct entra no compilador Java como **annotation processor**:

```
Você declara interface @Mapper (métodos Entity↔DTO)
        │
        ▼  (javac / Maven / Gradle — build)
Código Java gerado (implementação com getters/setters)
        │
        ▼
Runtime: chamadas diretas — sem reflexão típica de mappers “dinâmicos”
```

Leitura FCC (ouro):

| Aspecto | MapStruct | Mapeamento “só na mão” | Mappers por reflexão (ex.: ModelMapper — contraste conceitual) |
|---------|-----------|-------------------------|----------------------------------------------------------------|
| Quando monta o mapeamento | **Compilação** | Você escreve cada `setX(getX())` | Em geral **runtime** |
| Feedback de erro | Falha no **build** se mapeamento incompleto/inválido | Erro humano silencioso fácil | Erro mais tarde, em execução |
| Performance típica | Código **direto** (próximo do manual) | Boa, mas verboso | Overhead de reflexão |
| Manutenção | Interface + anotações; gera o boilerplate | Boilerplate cresce com o domínio | Menos código declarado, menos checagem em compile |

Frase de prova: MapStruct = **type-safe** + **rápido** + **gera na compilação**.

Não precisa decorar a API linha a linha (`@Mapping(source=…, target=…)`) salvo se o enunciado citar — foque no **papel** e no **momento** (compile-time).

### 1.4 Como “parece” no projeto (visão de prova, sem tutorial)

Ideia mental (não decorar sintaxe):

1. Dependência MapStruct + **processor** no build.  
2. Interface anotada (ex.: `@Mapper`) com métodos `toDto(Entity)`, `toEntity(Dto)`.  
3. Build gera a classe de implementação.  
4. No serviço/controller Spring, injeta-se o mapper (ex.: `componentModel = "spring"`) e converte na borda da aplicação.

Pontos cobráveis:

- Sem o **processor** no build, a implementação **não aparece** — MapStruct não “roda sozinho” só com jar de API.  
- Nomes iguais mapeiam por convenção; nomes diferentes pedem configuração (`@Mapping`).  
- Pode mapear **listas**, aninhar mappers, ignorar campos (`ignore`), etc. — detalhes só se o enunciado pedir.

### 1.5 Por que MapStruct em APIs / microsserviços?

| Cenário | Leitura cobrável |
|---------|------------------|
| Controller REST devolve JSON | Preferível **DTO** estável → precisa Entity→DTO |
| Request cria/atualiza recurso | Body DTO → Entity (ou comando) → persistência |
| Muitos campos / vários endpoints | Boilerplate manual vira fonte de bug → MapStruct reduz erro |
| Microsserviços | Fronteiras claras: **não vazar** modelo interno; mapper na borda do serviço |
| Stack PDPJ / Spring | Edital cita MapStruct ao lado de Swagger, Boot/Cloud — papel **complementar**, não substituto |

MapStruct **não** substitui: REST, JSON, Gateway, Discovery, documentação OpenAPI, JPA.

Flash de arquitetura:

```
Cliente HTTP (JSON)
      │
      ▼
 Controller (DTO)  ←→  MapStruct  ←→  Domain/Entity  ←→  JPA/BD
      │
   Swagger documenta o contrato (DTOs/endpoints) — outro papel
```

### 1.6 MapStruct × Swagger (anti-confusão ouro)

| | **MapStruct** | **Swagger / OpenAPI** |
|---|---------------|------------------------|
| Pergunta | Como **converter** objetos Java entre camadas? | Como **documentar/expor o contrato** da API? |
| Artefato | Código de mapper gerado no build | Spec / UI (endpoints, schemas, try-it) |
| Momento típico | Compilação → runtime de mapeamento | Design/docs + consumo por humanos/ferramentas |
| Face no edital | Entity ↔ DTO | Documentação REST |
| Erro típico FCC | Chamar de “documentação da API” | Chamar de “mapper Entity↔DTO” |

Ambos convivem: Swagger descreve o **DTO** na API; MapStruct **preenche** o DTO a partir da Entity (e o inverso).

### 1.7 MapStruct × mapeamento manual

| Critério | Manual | MapStruct |
|----------|--------|-----------|
| Controle total | Sim (você escreve tudo) | Sim, via interface + regras; gera o miolo |
| Risco de esquecer campo | Alto em DTOs grandes | Build pode **reclamar** de mapeamento incompleto |
| Volume de código | Alto | Baixo na mão do desenvolvedor |
| Debugging | Direto no seu código | Código gerado é Java legível (chamadas plain) |

Em prova: “evitar repetição de getters/setters / mapeamento tipado na compilação” → MapStruct.  
“Documentar endpoints / try-it” → Swagger.  
**Não** diga que MapStruct “substitui o mapeamento” no sentido de eliminar a necessidade de Entity/DTO — ele **implementa** o mapeamento.

### 1.8 O que MapStruct **não** faz (lista seca)

1. Não é Service Discovery (Eureka).  
2. Não é API Gateway (Zuul).  
3. Não documenta OpenAPI (Swagger).  
4. Não é o ORM (Hibernate/JPA) nem migração (Flyway).  
5. Não define verbos HTTP nem status REST.  
6. Não é sinônimo de Spring Boot nem de Spring Cloud.  
7. Não “registra” microsserviços nem roteia na borda.

### 1.9 Armadilhas FCC (lista de ouro)

1. Trocar MapStruct e Swagger de papel.  
2. Dizer que MapStruct mapeia **em runtime por reflexão** como regra típica (o diferencial cobrado é **compile-time**).  
3. Afirmar que MapStruct **documenta** a API.  
4. Afirmar que Swagger **gera** o mapper Entity↔DTO (papel do MapStruct).  
5. Confundir MapStruct com Eureka / Zuul / Envers / Flyway.  
6. Achar que usar MapStruct **obriga** a expor Entity no JSON.  
7. Achar que DTO e Entity são a mesma coisa “porque o MapStruct unifica”.  
8. Reduzir MapStruct a “biblioteca de JSON” (JSON = serialização HTTP; MapStruct = beans Java).  
9. Dizer que mapeamento manual é “proibido” — é só mais verboso/propenso a erro.  
10. Inventar que MapStruct substitui Gateway ou Discovery.

### 1.10 Mini-síntese (cartão mental)

> **MapStruct** = mapper **Entity ↔ DTO** com código gerado na **compilação** (rápido, tipado).  
> **Swagger** = **documenta** o contrato REST.  
> Manual = mesmo papel de mapear, sem o gerador.  
> Em microsserviços/APIs: DTO na borda, Entity por dentro, MapStruct no meio.

---

## 2. Questões de treino (1–15)

**1.** No eixo PDPJ / stack Spring do edital, o MapStruct desempenha prioritariamente o papel de:

A) API Gateway / ponto de entrada único  
B) Service Discovery / registry de instâncias  
C) Mapper compile-time entre objetos Java (ex.: Entity ↔ DTO)  
D) Documentação interativa OpenAPI  
E) Banco embutido H2 obrigatório  

**2.** A principal diferença cobrável entre MapStruct e mapeadores baseados em reflexão em runtime é que o MapStruct:

A) Só funciona com XML/SOAP  
B) Gera o código de mapeamento em **tempo de compilação**  
C) Substitui o Eureka no heartbeat  
D) É o nome comercial do Swagger UI  
E) Persiste entidades no lugar do JPA  

**3.** Em uma API REST tipica, Entity e DTO se relacionam assim:

A) São sempre a mesma classe, por definição do HTTP  
B) Entity liga-se à persistência; DTO à troca/contrato da API; o mapper traduz entre eles  
C) DTO é o registry Eureka; Entity é o Zuul  
D) Ambos existem apenas no Swagger Codegen  
E) Entity é exclusiva de microsserviços; DTO só de monólitos  

**4.** Assinale a associação **correta**:

A) MapStruct = docs OpenAPI; Swagger = Entity↔DTO  
B) MapStruct = Entity↔DTO; Swagger = documentação de API  
C) MapStruct = discovery; Swagger = gateway  
D) MapStruct = Flyway; Swagger = Envers  
E) MapStruct e Swagger são o mesmo jar  

**5.** Usar MapStruct em microsserviços ajuda sobretudo a:

A) Registrar réplicas no Eureka Server  
B) Manter fronteira clara (DTO na API × modelo interno), com mapeamento tipado e menos boilerplate  
C) Eliminar a necessidade de JSON  
D) Substituir o API Gateway  
E) Documentar try-it no lugar do Swagger  

**6.** Sobre mapeamento **manual** Entity→DTO, é correto afirmar que:

A) É conceitualmente impossível sem MapStruct  
B) É possível, porém verboso e propenso a esquecer campos; MapStruct automatiza com geração no build  
C) É sinônimo estrito de Service Discovery  
D) Só existe se o payload for XML  
E) Substitui integralmente o Hibernate  

**7.** Qual afirmação é **incorreta**?

A) MapStruct gera implementações de mapper na compilação  
B) MapStruct tipicamente mapeia beans (Entity/DTO)  
C) MapStruct é, por definição, a ferramenta de documentação OpenAPI da API  
D) MapStruct pode reduzir getters/setters manuais em massa  
E) Falhas de mapeamento podem aparecer no build  

**8.** No fluxo mental cobrado (API Spring):

A) Cliente → Eureka Dashboard → MapStruct como gateway → H2  
B) Controller recebe/devolve DTO; MapStruct converte para/de Entity; JPA persiste  
C) Swagger UI roteia na borda; MapStruct registra heartbeat  
D) Zuul gera DTOs; MapStruct documenta OpenAPI  
E) Flyway mapeia Entity↔DTO em runtime por reflexão obrigatória  

**9.** “Ferramenta que, no build, gera código Java tipado para copiar campos entre Entity e DTO.” Descreve:

A) Eureka  
B) Zuul  
C) MapStruct  
D) Swagger UI  
E) Keycloak  

**10.** Swagger e MapStruct convivem porque:

A) São mutuamente excludentes no edital  
B) Swagger descreve o contrato (muitas vezes sobre DTOs); MapStruct realiza a conversão Entity↔DTO no código  
C) Ambos são registries de instâncias  
D) MapStruct renderiza o try-it; Swagger gera mappers  
E) Só um deles pode existir por microsserviço  

**11.** MapStruct **não** se destina a:

A) Mapear Entity para DTO  
B) Mapear DTO para Entity  
C) Atuar como catálogo dinâmico de instâncias (papel do Eureka)  
D) Reduzir boilerplate de conversão entre modelos  
E) Oferecer checagens relacionadas ao mapeamento no compile  

**12.** Em prova, confundir MapStruct com Swagger costuma errar porque:

A) Ambos documentam apenas SOAP  
B) Um mapeia objetos na aplicação; o outro documenta/expõe o contrato da API  
C) Não há diferença cobrável  
D) MapStruct é gateway; Swagger é discovery  
E) Swagger só existe sem DTOs  

**13.** A frase mais alinhada ao diferencial de performance/segurança de tipos do MapStruct é:

A) “Usa sobretudo reflexão pesada em cada request como regra de ouro do produto”  
B) “Gera chamadas diretas (get/set) na compilação, com checagem tipada no build”  
C) “Substitui JSON por protobuf obrigatoriamente”  
D) “É o dialect SQL do Hibernate 5”  
E) “Só mapeia se houver Zuul na frente”  

**14.** Qual linha resume melhor o cartão mental deste subtópico?

A) MapStruct = docs; Swagger = mapper; Eureka = DTO  
B) MapStruct = Entity↔DTO (compile-time); Swagger = contrato/docs; manual = mesmo papel sem gerador  
C) MapStruct = entrada única; Swagger = heartbeat  
D) MapStruct elimina Entity e DTO  
E) MapStruct = Flyway + Envers  

**15.** Afirmar que “MapStruct documenta endpoints REST e substitui o Swagger” é:

A) Correto em toda prova FCC  
B) Incorreto: documentação/contrato interativo associa-se ao Swagger/OpenAPI; MapStruct mapeia objetos  
C) Correto somente com H2  
D) Correto porque ambos são Spring Cloud Netflix  
E) Correto se o gateway for Zuul  

---

### Gabarito comentado (treino 1–15)

1. **C** — Mapper compile-time Entity↔DTO.  
2. **B** — Diferencial: geração na compilação.  
3. **B** — Persistência × contrato; mapper no meio.  
4. **B** — Tabela anti-inversão MapStruct × Swagger.  
5. **B** — Fronteira + menos boilerplate.  
6. **B** — Manual possível; MapStruct automatiza no build.  
7. **C** — Docs OpenAPI = Swagger, não MapStruct.  
8. **B** — DTO ↔ MapStruct ↔ Entity ↔ JPA.  
9. **C** — Definição cobrável de MapStruct.  
10. **B** — Papéis complementares.  
11. **C** — Catálogo = Eureka.  
12. **B** — Mapper ≠ documentação.  
13. **B** — Código direto + type-safety no build.  
14. **B** — Síntese do arquivo 09.  
15. **B** — Não troca papéis com Swagger.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60)

Em uma aplicação Java que utiliza MapStruct, em condições ideais, um usuário tem uma classe chamada `Processo` e uma chamada `ProcessoDto`. Deseja criar uma interface MapStruct chamada `ProcessoMap` para mapear de `ProcessoDto` para `Processo` e vice-versa. Essa interface deve ser anotada, imediatamente antes de sua declaração, com

A) `@Map(type="MapStruct")`

B) `@Mapping`

C) `@MapStruct`

D) `@Mapper`

E) `@InterfaceMapper`


**17.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 46 de 60)

Em uma aplicação, considere a existência de uma classe chamada Advogado com o atributo nome e outra classe AdvogadoDto com o atributo nomeCompleto. Usando MapStruct, uma Analista criou a interface abaixo.```Ipublic interface AdvogadoMapper { II(target = "nomeCompleto", source = "nome") AdvogadoDto advogadoToAdvogadoDto(Advogado advogado);}```Para que o mapeamento seja realizado corretamente, as lacunas I e II devem ser preenchidas, correta e respectivamente, por

A) `@Mapper` e `@Mapping`

B) `@Mapping` e `@Equals`

C) `@MapStruct` e `@Mappings`

D) `@MapStruct` e `@Map`

E) `@Mapper` e `@Equity`


**18.** (FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 37 de 60)

Para definir um mapeador de bean com MapStruct deve-se definir uma interface Java com os métodos de mapeamento necessários e anotá-la com a anotação

A) `@MapStruct`

B) `@EntityMapper`

C) `@GeneratedMapper`

D) `@Mapper`

E) `@BeanMapping`


**19.** (FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60)

No componente controller de uma aplicação criada com recursos Spring Boot foram incluídas as anotações abaixo a um método REST.```@ApiOperation(value = "Obter a lista de processos no sistema", response = Iterable.class, tags = "getProcessos")@ApiResponses(value = {@ApiResponse(code = 200, message = "Successo|OK"),@ApiResponse(code = 401, message = "Não autorizado"),@ApiResponse(code = 403, message = "Proibido"),@ApiResponse(code = 404, message = "Não encontrado") })@RequestMapping(value = "/getProcessos")public List<Processos> getProcessos() {return processos;}````@ApiOperation` e `@ApiResponses` são anotações

A) MapStruct.

B) Hibernate.

C) Spring Eureka.

D) Spring Zuul.

E) Swagger.


**20.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22) *[aproximação temática]*

*Stack Java/PDPJ; MapStruct raro isolado.*

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.

B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.

C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.

D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.

E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.


**21.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60) *[aproximação temática]*

*Anotações Java em entidades.*

Um Analista de um Tribunal necessita habilitar o Hibernate Envers em uma entidade. Para isso, ele deve

A) utilizar o método `Session.saveOrUpdate()` para a entidade.

B) anotar a entidade com `@Audited`.

C) configurar `hibernate.envers.enabled` como `true` no arquivo de configuração.

D) estender a classe `AuditedEntity` fornecida pelo Hibernate Envers.

E) configurar o `audit-log` no arquivo `persistence.xml`.


**22.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60) *[aproximação temática]*

Uma analista está desenvolvendo um sistema usando JPA e deseja configurar a utilização do framework Hibernate como provedor de persistência, por meio da instrução `<provider>org.hibernate.ejb.HibernatePersistence</provider>` que deve ser colocada no arquivo

A) `hibernate.xml`

B) `persistence-unit.xml`

C) `persistence.xml`

D) `orm-database-config.xml`

E) `server.xml`


**23.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60) *[aproximação temática]*

Uma equipe de desenvolvimento está utilizando Spring Boot para criar uma API RESTful. Durante os testes, observaram que a aplicação demora a inicializar, principalmente devido à configuração do banco de dados. Para melhorar o tempo de inicialização, decidiram desabilitar a criação automática do esquema de banco de dados ao iniciar a aplicação. A configuração, no `application.properties`, que desabilita a criação automática do esquema de banco de dados pelo Spring Boot é a

A) `spring.jpa.hibernate.ddl-auto=none`

B) `spring.datasource.database-auto=false`

C) `spring.jpa.db-sql=disable`

D) `spring.jpa.properties.hibernate.db-auto=disable`

E) `spring.jpa.create-database-init=false`


**24.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60) *[aproximação temática]*

Um Analista de TI está desenvolvendo uma API com Spring Boot e deseja integrar Swagger para facilitar a documentação e testes dessa API. Para integrar no Swagger com um projeto Spring Boot a abordagem mais adequada é

A) adicionar as dependências do Swagger manualmente no classpath do projeto.

B) adicionar as dependências do Swagger no arquivo `pom.xml` ou `build.gradle` do projeto Spring Boot.

C) criar um arquivo separado para todas as configurações do Swagger sem importar as dependências no projeto.

D) utilizar um plugin externo para o IDE que automaticamente gera a documentação do Swagger sem configuração adicional.

E) configurar o servidor de aplicação para disponibilizar a documentação do Swagger sem modificar o código do projeto.


**25.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60) *[aproximação temática]*

Na abordagem do Spring para construir serviços web RESTful, as solicitações HTTP são tratadas por um controlador, que é uma classe identificada com a anotação

A) `@RestfulController`

B) `@RestfulMapping`

C) `@RestApplicationController`

D) `@RestController`

E) `@RequestController`


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **D** | FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60 |
| 17 | **A** | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 46 de 60 |
| 18 | **D** | FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 37 de 60 |
| 19 | **E** | FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60 |
| 20 | **E** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22 |
| 21 | **B** \* | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60 |
| 22 | **C** \* | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60 |
| 23 | **A** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 24 | **B** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 25 | **D** \* | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt15-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q32
- Q17: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q46
- Q18: https://www.quizinho.com.br/questao/fcc-trt23-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q37
- Q19: https://www.quizinho.com.br/questao/fcc-trt23-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q39
- Q20: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22
- Q21: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q54
- Q22: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q41
- Q23: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q38
- Q24: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q53
- Q25: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q31

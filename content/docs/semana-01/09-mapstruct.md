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

## 3. Questões estilo concurso FCC (16–25)

**16.** Em aplicações Java alinhadas à stack referida no contexto PDPJ-Br, o MapStruct caracteriza-se, em regra, por:

A) funcionar como catálogo exclusivo de registro e heartbeat de réplicas  
B) gerar, em tempo de compilação, mapeamentos tipados entre tipos Java (como Entity e DTO)  
C) expor, por si só, a documentação interativa OpenAPI da API  
D) substituir integralmente o protocolo HTTP por SOAP  
E) constituir o dialect SQL utilizado pelo Hibernate  

**17.** Relativamente à separação Entity/DTO em APIs REST, assinale a opção correta:

A) A Entity deve ser, por definição, o único payload JSON público em todo microsserviço  
B) O DTO costuma representar o contrato de entrada/saída; a Entity, o modelo de persistência; o MapStruct facilita a conversão  
C) DTO e Entity são nomes comerciais do Eureka  
D) A existência de DTO torna o MapStruct conceitualmente impossível  
E) MapStruct obriga banco único compartilhado entre serviços  

**18.** Comparando MapStruct ao mapeamento manual campo a campo, a vantagem tipicamente cobrada é:

A) eliminar a necessidade de controllers REST  
B) reduzir boilerplate e antecipar inconsistências de mapeamento no build, mantendo desempenho próximo ao código escrito à mão  
C) substituir o Service Discovery  
D) documentar try-it automaticamente no lugar do Swagger  
E) registrar a aplicação no Zuul como filtro error  

**19.** Assinale a alternativa que apresenta associação **incorreta**:

A) MapStruct — mapeamento Entity↔DTO  
B) Swagger — documentação/contrato de API  
C) Eureka — Service Discovery  
D) MapStruct — ponto único de entrada e roteamento de borda (papel de API Gateway)  
E) Zuul — API Gateway na stack clássica Netflix/Spring Cloud  

**20.** Em enunciado que contraste ferramentas da stack, a FCC tipicamente considera **correto** que:

A) Swagger gera mappers Entity↔DTO na compilação e MapStruct documenta endpoints  
B) MapStruct ocupa-se da conversão entre modelos de objeto, ao passo que Swagger documenta a API  
C) MapStruct e Swagger são intercambiáveis em qualquer item  
D) MapStruct aplica rate limit na borda  
E) Swagger realiza heartbeat das réplicas  

**21.** A expressão “annotation processor que produz implementações de mapper durante a compilação” descreve com mais precisão:

A) Eureka Server  
B) Zuul filter post  
C) MapStruct  
D) Flyway migrate  
E) Hibernate Envers exclusivamente  

**22.** Em arquitetura de microsserviços com API JSON, o uso de MapStruct **não** implica, por si só:

A) possibilidade de isolar o modelo interno do contrato externo  
B) substituição do API Gateway e do Service Discovery  
C) conversão tipada entre DTO e Entity  
D) menor volume de código manual de cópia de campos  
E) integração natural com serviços Spring que expõem DTOs  

**23.** Considere as afirmações:

I. MapStruct documenta schemas OpenAPI no navegador (try-it).  
II. MapStruct mapeia objetos Java com código gerado no build.  
III. Swagger não substitui o papel de mapper Entity↔DTO do MapStruct.

Assinale:

A) Apenas I  
B) Apenas II  
C) II e III  
D) I e II  
E) I, II e III  

**24.** Diferenciar MapStruct de frameworks de mapeamento por reflexão em runtime é relevante em provas porque:

A) não há qualquer diferença conceitual cobrável  
B) o MapStruct enfatiza geração compile-time, tipagem e desempenho por chamadas diretas  
C) MapStruct só existe fora do ecossistema Java  
D) reflexão compile-time é exclusividade do Eureka  
E) MapStruct é apenas o nome antigo do Swagger Codegen  

**25.** A síntese mais alinhada ao edital, sobre MapStruct, é:

A) MapStruct = discovery; Swagger = gateway; Entity = docs  
B) MapStruct = mapper Entity↔DTO com geração na compilação; útil em APIs/microsserviços; distinto de Swagger (docs) e de mapeamento só manual  
C) MapStruct obriga expor Entity no JSON e elimina DTO  
D) MapStruct substitui Zuul, Eureka e OpenAPI  
E) MapStruct é o banco H2 do Spring Boot  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Compile-time mapper Entity/DTO.  
17. **B** — DTO = contrato; Entity = persistência; MapStruct converte.  
18. **B** — Menos boilerplate + checagem no build + perf.  
19. **D** — Gateway ≠ MapStruct.  
20. **B** — Papéis complementares, sem inverter.  
21. **C** — Definição de annotation processor mapper.  
22. **B** — Não substitui Gateway/Discovery.  
23. **C** — I falsa (Swagger); II e III verdadeiras.  
24. **B** — Compile-time × reflexão runtime.  
25. **B** — Cartão mental completo do arquivo 09.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **Entity↔DTO / mapper / compile-time**? → MapStruct.  
2. É **documentar API / OpenAPI / try-it**? → Swagger.  
3. É **entrada/rota/filtro**? → Gateway/Zuul.  
4. É **onde está a réplica**? → Eureka.  
5. Manual ainda mapeia? → Sim — só sem o gerador.
)

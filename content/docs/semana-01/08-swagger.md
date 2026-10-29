+++
title = '08 — Swagger / OpenAPI'
date = '2026-10-08'
draft = false
weight = 8
tags = ['semana-01', 'pdpj', 'swagger']
categories = ['estudo']
+++
# Swagger / OpenAPI · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 08 — Swagger  
**Escopo deste arquivo:** documentação de API REST, especificação **OpenAPI**, família **Swagger** (UI, contrato), ideia de **contrato** da API, integração mental no ecossistema **PDPJ / Spring**, e o que Swagger **não** é (≠ gateway, ≠ discovery, ≠ mapper).  
**Fora de escopo aqui:** Zuul/filtros (06), Eureka (05), API Gateway abstrato (04), Spring Boot × Cloud em profundidade (07), MapStruct (09), REST/JSON como tema completo (03 — aqui só o elo “API documentada”).

---

## 1. Material

### 1.1 O que é Swagger / OpenAPI (leitura de prova)

Em provas FCC no eixo PDPJ / Spring, **Swagger** aparece como a face clássica de **documentação (e contrato) da API REST**: descreve endpoints, verbos, parâmetros, corpos, schemas e respostas de forma legível por humanos e por ferramentas.

Hoje o padrão aberto chama-se **OpenAPI Specification** (OAS). Historicamente a marca **Swagger** era o nome do ecossistema; em enunciados de concurso os termos costumam aparecer **juntos ou como sinônimos práticos**:

| Nome na prova | Leitura cobrável |
|---------------|------------------|
| **OpenAPI** | Especificação / formato do **contrato** da API |
| **Swagger** | Família de ferramentas + nome “de prova” para docs OpenAPI |
| **Swagger UI** | Interface **interativa** (browse + *try it out*) |

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | **Documentar / expor o contrato** da API REST |
| Artefato | Spec OpenAPI (YAML/JSON) descrevendo paths, métodos, schemas |
| Face humana | **Swagger UI** — documentação navegável e testável |

Flash: enunciado fala em **documentar endpoints**, **contrato da API**, **UI interativa**, **try-it-out** → **Swagger / OpenAPI**.

Swagger **não** é: API Gateway (Zuul), Service Discovery (Eureka), nem mapper Entity↔DTO (MapStruct).

### 1.2 Documentação de API — por que a banca cobra

Em microsserviços / PDPJ, muitos serviços expõem **APIs REST + JSON**. Sem documentação estruturada:

- consumidores (outros serviços, frontends, integrações) **não sabem** o contrato;
- onboarding e testes manuais ficam caros;
- divergência entre “código real” e “o que o time acha que a API faz”.

**Documentação OpenAPI/Swagger** responde: *quais recursos, com quais verbos, que parâmetros, que JSON de entrada/saída, que status?*

Leitura FCC:

- Documentar ≠ implementar o gateway.  
- Documentar ≠ descobrir réplicas.  
- Documentar ≠ mapear Entity↔DTO.  
- Documentar = **descrever o contrato HTTP/REST** da API.

### 1.3 OpenAPI como especificação (contrato máquina-legível)

A **OpenAPI Specification** é um documento (tipicamente **YAML** ou **JSON**) que descreve a API de forma padronizada. Conteúdo típico cobrado em visão de prova (sem decorar schema OAS linha a linha):

| Bloco conceitual | O que descreve |
|------------------|----------------|
| **Info** | Título, versão da API |
| **Servers / base** | URL(s) base |
| **Paths** | Recursos (`/processos`, `/processos/{id}`…) |
| **Operations** | Métodos HTTP (GET, POST…) por path |
| **Parameters** | Path, query, header… |
| **Request body** | Schema do JSON de entrada |
| **Responses** | Status + schema do corpo de saída |
| **Components / schemas** | Modelos reutilizáveis (DTOs documentados) |

Pontos de prova:

1. OpenAPI = **especificação do contrato**, não o runtime do microsserviço.  
2. A spec pode **gerar** UI, clientes, stubs — mas o papel nuclear em prova é **documentar/descrever**.  
3. Em stack Spring, anotações / springdoc / springfox (nomes variam) **produzem ou expõem** essa spec; não precisa decorar dependência Maven salvo se o enunciado citar.

Síntese: **OpenAPI = o contrato escrito**; **Swagger UI = a vitrine desse contrato**.

### 1.4 Swagger UI (a face interativa)

**Swagger UI** renderiza a especificação OpenAPI em uma página web:

- lista endpoints agrupados;
- mostra parâmetros, exemplos, schemas;
- permite **enviar requisições de teste** (*try it out*) contra a API (quando habilitado).

Tabela mental:

| Conceito | Pergunta que responde |
|----------|----------------------|
| OpenAPI (spec) | “Qual é o **contrato** formal?” |
| Swagger UI | “Como **navego e experimento** esse contrato?” |
| Código Spring do controller | “O que a API **realmente executa**?” |

Armadilha: achar que Swagger UI **é** o API Gateway. A UI **documenta e pode chamar** a API; **não** substitui Zuul como ponto de entrada/roteamento de borda do ecossistema.

### 1.5 Ideia de contrato (ouro FCC)

**Contrato da API** = acordo explícito entre provedor e consumidor sobre:

- recursos e URIs;
- verbos HTTP;
- formatos de entrada/saída (JSON + schemas);
- códigos de status e erros esperados.

OpenAPI/Swagger **formaliza** esse contrato. Em discurso de prova:

| Sem contrato documentado | Com OpenAPI/Swagger |
|--------------------------|---------------------|
| Integração “de boca” / código-fonte como única fonte | Spec compartilhada e inspecionável |
| Teste ad hoc | UI / clientes gerados a partir do contrato |
| Divergência silenciosa | Base para alinhar times e ferramentas |

Leitura fina (não exagerar):

- Contrato OpenAPI **descreve** a interface HTTP; **não** garante sozinho que a implementação está correta (pode haver *drift* se a spec estiver desatualizada).  
- Em prova, a associação pedida é: Swagger/OpenAPI ↔ **documentação/contrato da API**, não “substitui testes” nem “substitui gateway”.

Flash de associação:

> **REST/JSON** = como a API fala.  
> **Swagger/OpenAPI** = como essa fala é **documentada/contratada**.

### 1.6 Swagger no contexto Spring / PDPJ

No edital (Java, Spring Boot/Cloud, REST/JSON, Swagger):

| Peça | Papel |
|------|--------|
| **Spring Boot** | Empacota/roda o serviço que expõe a API |
| **Controllers REST** | Implementam endpoints |
| **Swagger / OpenAPI** | Documenta esses endpoints (spec + UI) |
| **Zuul** | Gateway de borda (entrada/rotas/filtros) |
| **Eureka** | Discovery (onde está a réplica) |
| **MapStruct** | Mapper compile-time Entity↔DTO |

Fluxo mental (não é topologia obrigatória):

```
Consumidor / desenvolvedor
        │
        ├─ consulta Swagger UI / OpenAPI  →  entende o contrato
        │
        └─ chama a API (direto ou via Zuul) → Spring Boot + REST/JSON
                                              (Eureka localiza réplicas, se houver)
```

Pontos cobráveis:

1. Em microsserviço Spring, Swagger documenta a **API daquele serviço** (ou agregada, se o enunciado disser).  
2. Ter Swagger **não** torna o serviço um gateway.  
3. Ter Swagger **não** registra a instância no Eureka.  
4. DTOs que aparecem nos schemas OpenAPI **não** são gerados pelo MapStruct — MapStruct mapeia objetos Java; OpenAPI **descreve** o JSON do contrato.

### 1.7 O que Swagger **NÃO** é (tabela anti-confusão)

Repetir até vir reflexo — a FCC **mistura** nomes da stack:

| | **Swagger / OpenAPI** | **Zuul (Gateway)** | **Eureka (Discovery)** | **MapStruct** |
|---|----------------------|--------------------|------------------------|---------------|
| Pergunta | Como a API está **documentada/contratada**? | Por onde o **externo entra**? | **Onde** está a réplica? | Como **mapear** Entity↔DTO? |
| Artefato | Spec + UI | Rotas + filtros | Registry / heartbeat | Código gerado em compile-time |
| Erro típico | Chamar de gateway/discovery/mapper | Chamar de docs OpenAPI | Chamar de Swagger UI | Chamar de documentação de API |

Distratores clássicos a marcar **errado**:

1. “Swagger é o API Gateway do Spring Cloud.”  
2. “Swagger registra heartbeats das réplicas.”  
3. “Swagger mapeia Entity↔DTO em tempo de compilação.”  
4. “Swagger UI substitui o Zuul na borda.”  
5. “OpenAPI = Service Discovery.”  
6. “Sem Swagger não existe REST.” (REST existe; Swagger **documenta**.)  
7. “Swagger e MapStruct são o mesmo jar.”  
8. “Eureka Dashboard = contrato OpenAPI.”

Cartão seco:

> **Swagger** = **docs/contrato** da API.  
> **Zuul** = **gateway**.  
> **Eureka** = **discovery**.  
> **MapStruct** = **mapper**.

### 1.8 O que a FCC tipicamente testa neste tema

- Swagger/OpenAPI = **documentação / contrato** de API REST.  
- Swagger UI = visualização **interativa** (+ try-it).  
- OpenAPI = especificação (YAML/JSON) dos paths/métodos/schemas.  
- Contraste seco com **Zuul**, **Eureka**, **MapStruct**, **Boot**.  
- Elo com REST/JSON: documenta o que o estilo REST expõe.  
- Não confundir “descrever contrato” com “rotear”, “descobrir” ou “mapear objetos”.

### 1.9 Armadilhas FCC (lista de ouro)

1. Trocar Swagger por Zuul (docs ↔ gateway).  
2. Trocar Swagger por Eureka (docs ↔ registry).  
3. Trocar Swagger por MapStruct (docs ↔ mapper).  
4. Afirmar que Swagger UI **é** o ponto único de entrada do ecossistema.  
5. Afirmar que OpenAPI **localiza** instâncias.  
6. Dizer que Swagger **gera** Entity↔DTO no lugar do MapStruct.  
7. Tratar Swagger e OpenAPI como **inimigos** (em prova, família/continuidade).  
8. Achar que documentar a API **substitui** autenticação de gateway ou discovery.  
9. Reduzir Swagger a “só comentários no código” sem a ideia de **contrato/spec**.  
10. Invertir: “MapStruct documenta a API; Swagger mapeia entidades.”

### 1.10 Mini-síntese (cartão mental)

> **Swagger / OpenAPI** = **documentação e contrato** da API REST (spec + **Swagger UI** interativa).  
> **Não** é gateway (**Zuul**), **não** é discovery (**Eureka**), **não** é mapper (**MapStruct**).  
> No Spring/PDPJ: Boot **roda** o serviço; Controllers **implementam**; Swagger **documenta**; Zuul **entra**; Eureka **localiza**.

---

## 2. Questões de treino (1–15)

**1.** No eixo PDPJ / Spring do edital, Swagger / OpenAPI desempenha prioritariamente o papel de:

A) API Gateway / roteamento de borda  
B) Documentação e contrato da API REST  
C) Service Discovery / registry de instâncias  
D) Mapper compile-time Entity↔DTO  
E) Banco embutido H2 obrigatório  

**2.** A OpenAPI Specification, em leitura de prova, é melhor descrita como:

A) O filtro **pre** do Zuul  
B) Documento (YAML/JSON) que descreve paths, operações, parâmetros e schemas da API  
C) O heartbeat enviado ao Eureka Server  
D) O processador de anotações do MapStruct  
E) O dialect SQL do Hibernate  

**3.** Swagger UI caracteriza-se por:

A) Registrar réplicas e remover instâncias mortas do catálogo  
B) Renderizar a spec OpenAPI de forma navegável, frequentemente com *try it out*  
C) Encaminhar `/api/**` aos microsserviços com filtros de borda  
D) Gerar getters/setters Entity↔DTO em compile-time  
E) Substituir o protocolo HTTP por SOAP  

**4.** A ideia de **contrato** da API, associada a OpenAPI/Swagger, refere-se sobretudo a:

A) JOIN obrigatório entre bancos de todos os microsserviços  
B) Acordo explícito sobre recursos, verbos, payloads e respostas esperadas  
C) Heartbeat periódico das réplicas  
D) Pipeline pre → route → post do Zuul  
E) Migração Flyway automática  

**5.** Assinale a associação **correta**:

A) Swagger = discovery; Eureka = documentação OpenAPI  
B) Swagger = docs/contrato da API; Zuul = API Gateway; Eureka = Service Discovery  
C) Swagger = MapStruct; Zuul = OpenAPI UI  
D) Swagger = Spring Boot; Eureka = Hibernate Envers  
E) Swagger e Eureka são o mesmo jar de gateway  

**6.** Qual afirmação é **incorreta**?

A) OpenAPI descreve endpoints e schemas  
B) Swagger UI pode permitir testar chamadas interativamente  
C) Swagger, por definição, é o catálogo que registra e monitora heartbeats das réplicas (papel do Eureka)  
D) Swagger documenta APIs REST tipicamente com JSON  
E) Em prova, Swagger e OpenAPI costumam aparecer na mesma família conceitual  

**7.** “O componente exibe paths, métodos HTTP, schemas JSON e permite experimentar a API no browser.” O papel descrito é tipicamente:

A) Eureka Server  
B) Swagger UI (OpenAPI)  
C) Zuul como registry  
D) MapStruct Processor  
E) Flyway  

**8.** Relativamente a MapStruct e Swagger:

A) Ambos são sinônimos estritos de API Gateway  
B) Swagger documenta o contrato HTTP; MapStruct mapeia objetos Java (Entity↔DTO)  
C) MapStruct gera a OpenAPI UI; Swagger gera mappers  
D) Ambos registram heartbeats no Eureka  
E) Nenhum dos dois aparece no edital PDPJ  

**9.** Afirmar que “Swagger substitui o Zuul na borda do ecossistema” é:

A) Correto em toda prova FCC  
B) Incorreto: Swagger documenta; Zuul é o API Gateway de entrada/roteamento/filtros  
C) Correto somente se o payload for XML  
D) Correto porque ambos são Spring Boot e Boot = gateway  
E) Correto porque Eureka unifica os dois  

**10.** No contraste com Service Discovery:

A) Swagger localiza host:porta das réplicas via heartbeat  
B) Eureka documenta schemas OpenAPI; Swagger é o registry  
C) Swagger descreve o contrato da API; Eureka responde “onde está a instância”  
D) Não há diferença cobrável entre Swagger e Eureka  
E) OpenAPI é o nome antigo do Eureka Dashboard  

**11.** Em um microsserviço Spring Boot que expõe REST/JSON, o papel típico do Swagger é:

A) Empacotar o JAR no lugar do Boot  
B) Documentar os endpoints expostos (contrato OpenAPI / UI)  
C) Aplicar filtros pre/route/post na borda de todo o cluster  
D) Manter o service registry do tribunal  
E) Executar migrations SQL em cada deploy  

**12.** Qual linha resume melhor o cartão mental deste subtópico?

A) Boot = docs; Swagger = gateway; Eureka = mapper  
B) Swagger/OpenAPI = documentação/contrato (+ UI); Zuul = entrada; Eureka = réplicas; MapStruct = Entity↔DTO  
C) Eureka = ponto único de entrada; Swagger = heartbeat  
D) MapStruct documenta try-it; Swagger mapeia entidades  
E) Swagger substitui Eureka, Zuul e Boot em qualquer desenho  

**13.** Um enunciado cita “especificação YAML com paths `/processos`, operação POST e schema do corpo”. Trata-se tipicamente de:

A) Filtro error do Zuul  
B) Artefato OpenAPI (contrato da API)  
C) Registro Eureka Client  
D) Annotation exclusiva do MapStruct para discovery  
E) Arquivo de dialect H2  

**14.** Qual alternativa mistura **incorretamente** os papéis?

A) Swagger — documentação de API  
B) Zuul — API Gateway  
C) Eureka — Service Discovery  
D) MapStruct — mapper Entity↔DTO  
E) Swagger — registry de heartbeats; Eureka — Swagger UI  

**15.** REST/JSON e Swagger relacionam-se assim na leitura cobrada:

A) Swagger substitui REST; JSON deixa de existir  
B) REST/JSON são o estilo/formato da API; Swagger/OpenAPI documentam esse contrato  
C) JSON é o gateway; Swagger é o discovery  
D) Sem Swagger UI o HTTP não funciona  
E) OpenAPI obriga SOAP/WSDL  

---

### Gabarito comentado (treino 1–15)

1. **B** — Docs/contrato da API REST.  
2. **B** — Spec YAML/JSON (paths, ops, schemas).  
3. **B** — UI navegável + try-it.  
4. **B** — Contrato = acordo explícito da interface HTTP.  
5. **B** — Tabela anti-inversão da stack.  
6. **C** — Catálogo/heartbeat = Eureka, não Swagger.  
7. **B** — Face interativa = Swagger UI.  
8. **B** — Docs ≠ mapper.  
9. **B** — Swagger ≠ gateway (Zuul).  
10. **C** — Contrato × localização.  
11. **B** — Documentar endpoints do serviço Boot.  
12. **B** — Síntese do arquivo 08.  
13. **B** — Spec OpenAPI.  
14. **E** — Inversão clássica de distratores.  
15. **B** — REST fala; Swagger documenta.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** Em arquiteturas de microsserviços alinhadas ao contexto PDPJ / Spring do edital, a utilização de Swagger / OpenAPI caracteriza-se, em regra, por:

A) funcionar como catálogo exclusivo de registro e heartbeat de todas as réplicas  
B) documentar e formalizar o contrato da API REST (endpoints, parâmetros, schemas)  
C) gerar, em tempo de compilação, mapeamentos Entity↔DTO  
D) atuar como único API Gateway de roteamento e filtros de borda  
E) constituir o dialect SQL utilizado pelo Hibernate  

**17.** A interface que apresenta a documentação OpenAPI de forma visual e permite, frequentemente, a execução de chamadas de teste a partir do navegador denomina-se, em geral:

A) Eureka Dashboard como registry puro  
B) Swagger UI  
C) Zuul filter chain  
D) MapStruct Processor  
E) Flyway Community  

**18.** Relativamente à convivência entre Swagger e Zuul, assinale a opção correta:

A) O Swagger substitui o Zuul como única porta pública de roteamento do cluster  
B) O Zuul substitui o Swagger como especificação OpenAPI  
C) São papéis distintos: Swagger documenta o contrato; Zuul atua como API Gateway  
D) Ambos são nomes comerciais do Eureka Server  
E) A existência do Swagger torna o API Gateway conceitualmente impossível  

**19.** Diferenciar OpenAPI de Service Discovery é relevante em provas porque:

A) OpenAPI e Eureka são sinônimos estritos em todo enunciado FCC  
B) OpenAPI descreve o contrato da API, ao passo que Discovery localiza instâncias dinamicamente  
C) Discovery gera Swagger UI; OpenAPI registra heartbeats  
D) Não há diferença cobrável entre os termos  
E) OpenAPI é o único mecanismo de discovery do Spring Boot  

**20.** Um enunciado afirma: “A equipe versionou um documento YAML com os paths da API, os verbos HTTP e os schemas JSON de request/response.” No vocabulário do edital, esse artefato corresponde, em regra, a:

A) filtro post do Zuul  
B) especificação OpenAPI (contrato da API)  
C) registro de instância no Eureka Server  
D) mapper MapStruct gerado  
E) migration Flyway  

**21.** Assinale a alternativa que apresenta associação **incorreta**:

A) Swagger / OpenAPI — documentação/contrato de API  
B) Swagger UI — visualização interativa da spec  
C) Zuul — API Gateway  
D) MapStruct — documentação OpenAPI interativa  
E) Eureka — Service Discovery  

**22.** Em questão que contraste papéis na stack Spring citada no PDPJ, a FCC tipicamente considera **correto** que:

A) o Swagger concentra entrada/roteamento/filtros e o Eureka documenta try-it  
B) o Swagger formaliza a documentação/contrato REST e não se confunde com gateway, discovery ou mapper  
C) Swagger, Zuul e Eureka são intercambiáveis em qualquer enunciado  
D) o MapStruct realiza o heartbeat das réplicas documentadas no OpenAPI  
E) Spring Boot elimina a noção de documentação de API  

**23.** Quando o objetivo da ferramenta é permitir que consumidores entendam rapidamente quais endpoints existem, quais parâmetros enviar e qual JSON esperar — sem ser responsável por localizar réplicas nem por filtrar a borda —, trata-se tipicamente de:

A) Eureka exclusivamente  
B) Swagger / OpenAPI  
C) Zuul exclusivamente como registry  
D) Hibernate Envers  
E) Rancher como contrato YAML da API  

**24.** Afirmar que “Swagger e MapStruct resolvem o mesmo problema de documentação interativa da API” é:

A) Correto, pois ambos geram OpenAPI UI  
B) Incorreto: MapStruct mapeia objetos; Swagger documenta o contrato HTTP  
C) Correto somente com banco H2  
D) Correto porque ambos são filtros Zuul  
E) Correto porque Eureka unifica mapper e docs  

**25.** A síntese mais alinhada ao edital, sobre Swagger no contexto Spring / PDPJ, é:

A) Swagger = discovery; Boot = gateway; Eureka = documentação  
B) Swagger / OpenAPI = documentação e contrato da API REST (com UI interativa), distinto de Zuul, Eureka e MapStruct  
C) Swagger obriga banco único compartilhado entre microsserviços  
D) OpenAPI existe apenas como sinônimo de filtro error do Zuul  
E) Eureka é a especificação OpenAPI; Swagger é apenas o nome antigo do MapStruct  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Documentar/formalizar contrato REST.  
17. **B** — Swagger UI = face interativa.  
18. **C** — Docs ≠ gateway; papéis distintos.  
19. **B** — Contrato × localização de instâncias.  
20. **B** — YAML de paths/verbos/schemas = OpenAPI.  
21. **D** — MapStruct ≠ OpenAPI UI (mapper).  
22. **B** — Cartão mental sem misturar papéis.  
23. **B** — Entender endpoints/JSON = Swagger/OpenAPI.  
24. **B** — Mapper ≠ documentação.  
25. **B** — Síntese completa do arquivo 08.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **docs / contrato / UI try-it**? → Swagger / OpenAPI.  
2. É **entrada / rota / filtro**? → Zuul (Gateway).  
3. É **onde está a réplica / registry**? → Eureka.  
4. É **Entity↔DTO compile-time**? → MapStruct.  
5. É **como a app sobe/jar**? → Boot.

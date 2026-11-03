+++
title = '06 — Zuul Gateway (Spring)'
date = '2026-10-06'
draft = false
weight = 6
tags = ['semana-01', 'pdpj', 'zuul']
categories = ['estudo']
+++
# Zuul como API Gateway (Spring Cloud) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — Zuul Gateway (Spring)  
**Escopo deste arquivo:** Zuul como implementação clássica de **API Gateway** no Spring Cloud (eixo Netflix do edital), papel de borda, **filtros** (pre / route / post / error) em alto nível, **roteamento** a microsserviços, relação com **Eureka**, e armadilhas **Zuul × Eureka × Spring Boot**.  
**Fora de escopo aqui:** API Gateway só no padrão abstrato (arquivo 04), Eureka em profundidade (05), Spring Boot × Cloud como contraste de produto (07), Spring Cloud Gateway moderno linha a linha, Swagger, MapStruct, REST/JSON (03).

---

## 1. Material

### 1.1 O que é Zuul (leitura de prova)

**Zuul** é o **API Gateway** clássico cobrado no edital PDPJ / Spring Cloud (família Netflix OSS). Na prova, Zuul = **ponto de entrada** do ecossistema de microsserviços: o cliente externo fala com o Zuul; o Zuul **roteia** para o serviço interno adequado e pode aplicar **filtros** (políticas) na borda.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | **API Gateway** / entrada única de borda |
| Mecânica | **Rotas** + **filtros** (pipeline de request/response) |
| Par clássico | **Zuul entra** · **Eureka localiza** |

Zuul **não** é: registry/catálogo de instâncias (Eureka), “o Spring Boot em si”, documentação OpenAPI (Swagger), nem mapper Entity↔DTO (MapStruct).

Flash: enunciado fala em **gateway Netflix / Spring Cloud**, **filtros de borda**, **roteamento `/serviço/**`** → **Zuul**.

### 1.2 Papel no Spring Cloud

No desenho clássico Spring Cloud Netflix:

| Peça | Papel |
|------|--------|
| **Spring Boot** | Forma de **empacotar/rodar** cada aplicação (serviço, gateway, etc.) |
| **Spring Cloud** | Conjunto de padrões de **sistemas distribuídos** (gateway, discovery, config…) |
| **Zuul** | Componente de **API Gateway** nessa stack |
| **Eureka** | Componente de **Service Discovery** (registry) |

Leitura cobrável:

- Um **serviço Zuul** tipicamente é uma app **Spring Boot** com dependências **Spring Cloud** + Zuul — isso **não** faz Boot = Zuul.
- Zuul **usa** o Cloud (roteamento dinâmico, integração com discovery); **não substitui** o Boot nem o Eureka.
- Em prova: “Zuul no Spring Cloud” = **gateway**; “Eureka no Spring Cloud” = **discovery**.

```
Cliente externo
      │
      ▼
   Zuul (API Gateway — borda)
      │  (consulta nomes/instâncias)
      ▼
   Eureka (Service Discovery)
      │
      ▼
 Microsserviço(s) Spring Boot
```

### 1.3 Filtros Zuul (alto nível — ouro da FCC)

Zuul processa a requisição em um **pipeline de filtros**. Tipos cobrados (sem virar tutorial de código):

| Tipo | Momento | Ideia cobrável | Exemplos mentais |
|------|---------|----------------|------------------|
| **pre** | **Antes** do encaminhamento ao serviço | Autenticação, validação, enriquecer contexto, checagens de borda | Validar token; rejeitar cedo; log de entrada |
| **route** | **Roteamento** propriamente dito | Encaminha a request ao destino (serviço/URL) | Escolher destino; proxy da chamada |
| **post** | **Depois** da resposta do serviço | Ajustar resposta, headers, métricas de saída | Adicionar header; log de saída |
| **error** | Quando ocorre **erro** no pipeline | Tratamento / resposta de falha na borda | Resposta padronizada de erro |

Flash de associação:

- **pre** → “ainda **não** foi ao serviço”  
- **route** → “**vai** ao serviço”  
- **post** → “já **voltou** do serviço”  
- **error** → “deu **errado** no caminho”

Pontos de prova:

1. Filtros = mecanismo pelo qual o Zuul aplica **cross-cutting** na borda.  
2. Ordem conceitual típica: **pre → route → post**; **error** quando falha.  
3. Autenticação “na entrada” associa-se naturalmente a filtro **pre** (visão cobrada).  
4. Não precisa decorar classes Java (`ZuulFilter`, `filterType()`) salvo se o enunciado citar — foque nos **quatro tipos** e no **momento**.

Armadilha: dizer que **todo** filtro Zuul é “só pós-resposta” ou que Zuul **não** tem estágio de erro.

### 1.4 Roteamento a serviços

Função nuclear do Zuul: **mapear** caminhos (ou regras) da borda para **microsserviços** de destino.

Ideia mental (não decorar YAML):

| Path na borda (exemplo) | Destino típico |
|-------------------------|----------------|
| `/api/processos/**` | serviço de processos |
| `/api/partes/**` | serviço de partes |
| `/usuario-service/**` | serviço de usuários |

Leitura FCC:

- O cliente chama **uma URL estável** (Zuul); o Zuul **encaminha**.
- Roteamento **não** significa que o Zuul implementa a regra de negócio do domínio.
- Em topologia dinâmica, o destino concreto (host:porta da réplica) costuma ser resolvido via **Eureka** (nome do serviço → instância).
- Duas leituras complementares: **rota** = “qual serviço?”; **discovery** = “qual instância desse serviço agora?”.

Zuul **proxy** / **edge service**: nome que aparece em textos oficiais — em prova, traduza mentalmente para **gateway de borda**.

### 1.5 Relação com Eureka

Tabela anti-inversão (repetir até vir reflexo):

| | **Zuul** | **Eureka** |
|---|----------|------------|
| Pergunta | Por onde o **externo** entra? | **Onde** está a instância do serviço X? |
| Papel | API Gateway (entrada + rotas + filtros) | Service Discovery (registry) |
| Face no edital | Gateway Netflix/Spring Cloud | Discovery Netflix/Spring Cloud |
| Erro típico FCC | Chamar de registry/catálogo | Chamar de ponto único de entrada |

Fluxo cobrável (completo):

```
Cliente → Zuul (rota + filtros) → consulta Eureka → instância do microsserviço
```

Regras de ouro:

1. Zuul **não** substitui Eureka.  
2. Eureka **não** é a porta pública do cliente externo.  
3. Em stack clássica, o gateway **depende** do discovery para achar réplicas saudáveis (visão típica).  
4. Um **entra**; o outro **localiza**.

### 1.6 Zuul × Eureka × Spring Boot (armadilhas FCC)

A banca mistura **produto/componente** com **forma de empacotar**:

| Nome | O que é na prova | NÃO é |
|------|------------------|-------|
| **Zuul** | API Gateway (borda, rotas, filtros) | Registry; “o Boot”; Swagger |
| **Eureka** | Service Discovery (catálogo, registro, heartbeat) | Gateway; entrada única pública |
| **Spring Boot** | Runtime/empacotamento da app (jar + servidor embutido etc.) | Sinônimo de Gateway nem de Discovery |
| **Spring Cloud** | Padrões/libs de nuvem/microsserviços (inclui gateway, discovery…) | Um único jar que “é o Zuul” |

Distratores clássicos:

1. “Eureka faz o papel de API Gateway.”  
2. “Zuul registra heartbeats das instâncias (papel do Eureka).”  
3. “Spring Boot *é* o API Gateway do ecossistema.”  
4. “Zuul e Eureka são o mesmo componente com dois nomes.”  
5. “Sem Spring Boot não existe conceito de gateway” (conceito × ferramenta).  
6. Confundir Zuul com **Spring Cloud Gateway** moderno — no edital clássico, a face cobrada de gateway Netflix é **Zuul**; não invente migração se o enunciado não pedir.

Cartão seco:

> **Boot** = como a app **roda**.  
> **Cloud** = padrões **distribuídos**.  
> **Zuul** = **gateway**.  
> **Eureka** = **discovery**.

### 1.7 O que a FCC tipicamente testa neste tema

- Zuul = **API Gateway** na stack Spring Cloud/Netflix do edital.  
- Quatro tipos de filtro: **pre**, **route**, **post**, **error** (momento, não código).  
- Roteamento de path → serviço; cliente não precisa da topologia interna.  
- Integração mental com Eureka (fluxo Cliente → Zuul → Eureka → serviço).  
- Contraste seco **Zuul ≠ Eureka ≠ Boot**.  
- Filtro **pre** ≈ políticas antes do destino (auth típica).  
- Zuul **não** é dono de todo o domínio dos microsserviços.

### 1.8 Armadilhas FCC (lista de ouro)

1. Trocar Zuul e Eureka de papel.  
2. Dizer que filtro **post** roda **antes** do encaminhamento.  
3. Esquecer o tipo **error**.  
4. Afirmar que Zuul **é** o registry.  
5. Afirmar que Eureka **é** a entrada única externa.  
6. Reduzir Spring Boot a “o gateway”.  
7. Tratar Zuul e Eureka como **sinônimos**.  
8. Achar que roteamento no Zuul **elimina** a necessidade de discovery em qualquer desenho.  
9. Confundir Zuul com Swagger / MapStruct / banco compartilhado.  
10. Invertir o fluxo: Cliente → Eureka (borda) → Zuul (catálogo).

### 1.9 Mini-síntese (cartão mental)

> **Zuul** = **API Gateway** Spring Cloud (Netflix) = **entrada** + **rotas** + **filtros** (**pre / route / post / error**).  
> **Eureka** = **onde** está a réplica.  
> **Spring Boot** = **como** a aplicação sobe/empacota — **não** é o nome do gateway.  
> Cliente → **Zuul** → (consulta) **Eureka** → microsserviço.

---

## 2. Questões de treino (1–15)

**1.** No Spring Cloud clássico do edital PDPJ, o Zuul desempenha prioritariamente o papel de:

A) Service Discovery / registry de instâncias  
B) API Gateway / ponto de entrada e roteamento de borda  
C) Gerador compile-time de mappers Entity↔DTO  
D) Banco embutido H2 obrigatório  
E) Ferramenta exclusiva de migração SQL  

**2.** A sequência conceitual dos filtros Zuul em uma requisição bem-sucedida é tipicamente:

A) post → pre → route  
B) error → post → pre  
C) pre → route → post  
D) route → error → pre (sempre nesta ordem fixa mesmo sem falha)  
E) apenas post, sem pre nem route  

**3.** Um filtro Zuul do tipo **pre** atua:

A) Somente depois que o microsserviço já respondeu  
B) Antes do encaminhamento ao serviço de destino (ex.: autenticação na borda)  
C) Apenas como registry Eureka  
D) Somente para gerar OpenAPI UI  
E) Exclusivamente no banco JPA do serviço  

**4.** O filtro do tipo **route** está associado a:

A) Documentação Swagger try-it  
B) Encaminhamento / proxy da requisição ao destino  
C) Heartbeat de instâncias no Eureka Server  
D) Mapeamento MapStruct  
E) Flyway migrate  

**5.** Filtros **post** no Zuul servem, em regra, para:

A) Registrar a instância no Eureka antes de qualquer request  
B) Atuar após a resposta do serviço (ex.: ajustar headers/logs de saída)  
C) Substituir o Service Discovery  
D) Compilar DTOs em tempo de build  
E) Eliminar a necessidade de status HTTP  

**6.** O filtro **error** no Zuul:

A) Substitui o Eureka em todos os cenários  
B) Trata falhas ocorridas no pipeline de filtros/roteamento  
C) É sinônimo estrito de filtro pre  
D) Só existe no MapStruct  
E) Documenta contratos OpenAPI  

**7.** Sobre roteamento no Zuul, é correto afirmar que:

A) O Zuul implementa, por definição, toda a regra de negócio de todos os serviços  
B) O Zuul encaminha requisições aos microsserviços conforme rotas (ex.: path), sem ser o dono do domínio  
C) Roteamento significa gravar JOIN entre schemas de serviços distintos  
D) Roteamento só existe com SOAP/WSDL  
E) Roteamento torna Eureka desnecessário em qualquer topologia absoluta  

**8.** No fluxo típico cobrado (cliente externo + Zuul + Eureka):

A) Cliente → Eureka (borda pública) → Zuul (catálogo) → serviço  
B) Cliente → Zuul → (consulta) Eureka → microsserviço  
C) Cliente → MapStruct → Eureka Dashboard → H2  
D) Cliente → Swagger UI → Flyway → Zuul como registry  
E) Cliente → Boot puro, sem conceito de gateway no edital  

**9.** Assinale a associação **correta**:

A) Zuul = discovery; Eureka = API Gateway  
B) Zuul = API Gateway; Eureka = Service Discovery  
C) Zuul = MapStruct; Eureka = Swagger  
D) Zuul = Spring Boot; Eureka = Hibernate Envers  
E) Zuul e Eureka são nomes do mesmo jar de mapper  

**10.** Spring Boot, em contraste com Zuul, é melhor descrito como:

A) O registry de heartbeats do ecossistema  
B) A forma típica de empacotar/executar a aplicação — não o papel de API Gateway em si  
C) O único nome correto para Service Discovery  
D) A documentação OpenAPI interativa  
E) O filtro error do Zuul  

**11.** Autenticação validada na borda **antes** de chegar ao microsserviço associa-se, no discurso de filtros Zuul, sobretudo a:

A) Filtro post exclusivamente  
B) Filtro pre  
C) Papel exclusivo do Eureka Server  
D) Annotation do MapStruct  
E) Dialect do PostgreSQL  

**12.** Qual afirmação é **incorreta**?

A) Zuul pode aplicar filtros na borda  
B) Zuul roteia para microsserviços  
C) Zuul é, por definição, o catálogo que registra e monitora heartbeats das réplicas (papel do Eureka)  
D) Zuul costuma integrar-se ao discovery para resolver instâncias  
E) Zuul aparece no edital como face clássica de API Gateway  

**13.** “O componente recebe `/api/processos/**`, aplica filtro de token e encaminha ao serviço de processos.” O papel descrito é tipicamente:

A) Eureka como registry puro, sem borda  
B) Zuul (API Gateway)  
C) MapStruct  
D) Swagger Codegen obrigatório  
E) Hibernate Envers  

**14.** Afirmar que “Zuul e Eureka são o mesmo componente” é:

A) Correto em toda prova FCC  
B) Incorreto: um é gateway (entrada/rotas/filtros); o outro é discovery (catálogo)  
C) Correto somente se o payload for XML  
D) Correto porque ambos são Spring Boot e Boot = gateway  
E) Correto porque Swagger unifica os dois  

**15.** Qual linha resume melhor o cartão mental deste subtópico?

A) Boot = gateway; Zuul = docs; Eureka = mapper  
B) Zuul = entrada + rotas + filtros (pre/route/post/error); Eureka = onde está a réplica; Boot = como a app sobe  
C) Eureka = ponto único de entrada; Zuul = heartbeat  
D) Filtro post autentica antes do route; error não existe  
E) Zuul substitui Eureka e Boot em qualquer desenho  

---

### Gabarito comentado (treino 1–15)

1. **B** — Zuul = API Gateway na stack clássica.  
2. **C** — pre → route → post (sucesso); error à parte.  
3. **B** — pre = antes do destino.  
4. **B** — route = encaminhar/proxy.  
5. **B** — post = depois da resposta.  
6. **B** — error = falha no pipeline.  
7. **B** — roteia; não engole o domínio.  
8. **B** — Cliente → Zuul → Eureka → serviço.  
9. **B** — Tabela anti-inversão.  
10. **B** — Boot empacota/roda; não é o nome do gateway.  
11. **B** — Auth na entrada ≈ pre.  
12. **C** — Catálogo/heartbeat = Eureka.  
13. **B** — Path + token + encaminhar = Zuul.  
14. **B** — Papéis distintos.  
15. **B** — Síntese do arquivo 06.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Para fazer um aplicativo criado com Spring Boot, em condições ideais, funcionar como um servidor Zuul Proxy deve-se anotar a classe principal com

A) `@EnableZuulProxy`

B) `@ZuulServerApplication`

C) `@EnabledZuulEdgeServer`

D) `@ZuulEdgeServerOn`

E) `@ZuulProxyApplication`


**17.** (FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Em um cenário de sistema de microsserviços um técnico deseja expor alguns dos microsserviços para acesso externo e ocultar outros. Como ele está usando o Spring Cloud, para proteger os microsserviços expostos contra solicitações de clientes mal-intencionados, ele pode usar o

A) Galaxy Eureka como edge server.

B) Apache Toncat como service discovery.

C) Apache Wildfly como security server.

D) Netflix Zuul como edge server.

E) Swagger como security server.


**18.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60)

Ao usar o Gateway API com o Service Discovery no contexto da PDPJ-Br, a principal consequência para o frontend em uma aplicação baseada em microsserviços é que o

A) Gateway API diminui a necessidade de autenticação, uma vez que o Service Discovery é integrado ao SSO.

B) frontend deve configurar manualmente os IPs e portas dos serviços disponíveis na plataforma.

C) frontend se torna o responsável pela chamada direta de cada microsserviço individualmente.

D) Gateway API reduz o número de chamadas externas que o frontend precisa fazer, tornando a comunicação mais eficiente.

E) frontend deve gerenciar manualmente as atualizações de endpoint de cada serviço individual registrado.


**19.** (FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 40 de 60)

Integração e implantação contínuas (CI/CD) é uma coleção de práticas usadas para integrar e entregar código rapidamente. Usando a prática de

A) integração contínua (CI), em um cronograma pré-acordado, provavelmente no final do ciclo de sprint, o pacote de código é liberado e testado manualmente pelo time de desenvolvimento.

B) implantação contínua (CD), depois que todo o código é validado com sucesso pelo time de desenvolvimento e qualidade, implanta-se o código manualmente no ambiente de teste ou de produção.

C) implantação contínua (CD) em que o grupo controle de qualidade realiza testes manuais para todos os códigos novos ou modificados. Se for bem-sucedido, o código será liberado para implantação; caso contrário, o lançamento é rejeitado e colocado em espera até que os bugs sejam corrigidos.

D) integração contínua (CI), os desenvolvedores são incentivados a fazer pequenos ajustes de código, mesclar seu código na ramificação de código principal e validá-lo usando testes automatizados.

E) integração contínua (CI), para implementar pipelines devem ser utilizadas ferramentas como Zuul e Eureka.


**20.** (FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60)

No componente controller de uma aplicação criada com recursos Spring Boot foram incluídas as anotações abaixo a um método REST.```@ApiOperation(value = "Obter a lista de processos no sistema", response = Iterable.class, tags = "getProcessos")@ApiResponses(value = {@ApiResponse(code = 200, message = "Successo|OK"),@ApiResponse(code = 401, message = "Não autorizado"),@ApiResponse(code = 403, message = "Proibido"),@ApiResponse(code = 404, message = "Não encontrado") })@RequestMapping(value = "/getProcessos")public List<Processos> getProcessos() {return processos;}````@ApiOperation` e `@ApiResponses` são anotações

A) MapStruct.

B) Hibernate.

C) Spring Eureka.

D) Spring Zuul.

E) Swagger.


**21.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22) *[aproximação temática]*

*Zuul nos distratores.*

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.

B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.

C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.

D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.

E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.


**22.** (FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60) *[aproximação temática]*

O Spring Eureka é uma implementação do padrão de registro de serviços que permite que os microsserviços se registrem automaticamente e se descubram uns aos outros. A porta padrão em que o servidor Eureka irá executar na aplicação Spring Boot e o arquivo em que essa porta é configurada são, respectivamente,

A) `80` e `eureka-server.xml`

B) `15000` e `application-server.yml`

C) `8080` e `pom.xml`

D) `8005` e `eureka-registry.xml`

E) `8761` e `application.properties`


**23.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60) *[aproximação temática]*

Correspondem, correta e respectivamente, a um componente do Spring Cloud e à principal função do Spring Cloud Config:

A) Spring Security — implementar um padrão de design para a comunicação entre serviços baseados em REST.

B) Spring Boot Actuator — gerenciar o ciclo de vida e o monitoramento de aplicações distribuídas em uma arquitetura de microsserviços.

C) Spring Data JPA — oferecer uma solução para o balanceamento de carga entre instâncias de microsserviços.

D) Spring Cloud Stream — centralizar a configuração de aplicações e serviços, permitindo que configurações sejam atualizadas dinamicamente.

E) Spring Web MVC — fornecer um serviço de autenticação e autorização para microsserviços e aplicações.


**24.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60) *[aproximação temática]*

Na classe principal de uma aplicação criada com Spring Boot, para fazer com que a aplicação atue como um cliente Eureka utiliza-se a anotação

A) `@EurekaClient`

B) `@EnableEurekaClient`

C) `@EnableDiscoveryEurekaClient`

D) `@EurekaDiscoveryClient`

E) `@IsEurekaClient`


**25.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 42 de 60)

Uma aplicação envia ao advogado uma notificação avisando que houve uma atualização em um processo, mesmo sem o advogado abrir o aplicativo para procurar a informação. Para que a informação chegue até o sistema do aplicativo do Tribunal em tempo real e até o advogado, da forma relatada, sem a necessidade de ação externa, pode ser utilizado um recurso conhecido como

A) Swagger.

B) Webhook.

C) Mockito Pulling.

D) Eureka Messages.

E) Elastic Messages.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **A** | FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 17 | **D** | FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 18 | **D** | FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60 |
| 19 | **D** | FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 40 de 60 |
| 20 | **E** | FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60 |
| 21 | **E** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22 |
| 22 | **E** \* | FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60 |
| 23 | **D** \* | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 24 | **B** \* | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60 |
| 25 | **B** | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 42 de 60 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt23-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q38
- Q17: https://www.quizinho.com.br/questao/fcc-trt4-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q38
- Q18: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q39
- Q19: https://www.quizinho.com.br/questao/fcc-trt4-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q40
- Q20: https://www.quizinho.com.br/questao/fcc-trt23-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q39
- Q21: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22
- Q22: https://www.quizinho.com.br/questao/fcc-trt15-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q31
- Q23: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q53
- Q24: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q32
- Q25: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q42

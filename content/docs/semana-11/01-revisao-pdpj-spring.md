+++
title = '01 — Revisão PDPJ-Br + Spring stack'
date = '2026-10-27'
draft = false
weight = 1
tags = ['semana-11', 'revisao']
categories = ['estudo']
+++
# Revisão PDPJ-Br + Spring stack · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Semana 11 · revisão dirigida (não matéria nova)  
**Escopo deste arquivo:** reforço denso da **PDPJ-Br** + stack Spring clássica do edital — **API Gateway × Service Discovery**, **Eureka**, **Zuul**, **Boot × Cloud**, flash **OAuth2/Keycloak**; mapa de **erros típicos FCC**. Pressupõe semanas 1–2.  
**Fora de escopo aqui:** SQL/JPA/Flyway (arquivo 02 desta semana); Rabbit/Docker/K8s; Swagger/MapStruct linha a linha; normativos CNJ artigo a artigo.

**Modo:** cartões anti-confusão + treino + bloco real 16–25. Meta: reacender papéis trocados que a FCC repetiu em TRT/TRF.

---

## 1. Material (revisão com margem de segurança)

### 1.1 PDPJ-Br em 60 segundos (não monólito)

**PDPJ-Br** = **Plataforma Digital do Poder Judiciário Brasileiro**: ecossistema nacional de **integração**, **reuso** e **padronização** de capacidades digitais sob diretrizes do **CNJ**. Tribunais **participam e operam**; não “viram” a plataforma.

| Leitura correta | Leitura errada (distrator) |
|-----------------|----------------------------|
| Plataforma / ecossistema de serviços e APIs | Um único monólito nacional que substitui todos os sistemas |
| Interoperabilidade por contratos/padrões | Banco físico único obrigatório para tudo |
| CNJ governa o ecossistema; tribunal integra | CNJ “dona do código” de cada TRT |
| Stack (Eureka, Zuul…) = **meios** técnicos | PDPJ = sinônimo de Eureka/Zuul/Swagger |

Flash: **plataforma compartilhada + autonomia operacional dos tribunais**.

### 1.2 Mapa mental da stack (o que a prova troca)

```
Cliente externo
      │
      ▼
  Zuul / API Gateway   ← “por onde ENTRA?”  (borda, rotas, filtros)
      │  consulta nomes
      ▼
  Eureka               ← “ONDE está a réplica?” (registry / discovery)
      │
      ▼
 Microsserviço Spring Boot (+ Cloud client)
      │
      ▼
 Keycloak (AS/IdP)     ← “quem é / token?”  (SSO OAuth2)
```

| Peça | Pergunta FCC | NÃO é |
|------|--------------|-------|
| **API Gateway / Zuul** | Entrada única, roteamento, filtros (pre/route/post/error) | Registry |
| **Eureka** | Catálogo dinâmico de instâncias; heartbeat | Gateway / edge |
| **Spring Boot** | Como a app **sobe** (starters, jar, auto-config) | Discovery nem gateway |
| **Spring Cloud** | Padrões **distribuídos** (Eureka, Zuul, Config…) | “outro nome do Boot” |
| **Keycloak** | IdP / Authorization Server do SSO | Gateway nem Eureka |
| **Flyway** *(vizinho)* | Migration versionada de **schema** | Envers / Swagger |
| **Envers** *(vizinho)* | Auditoria de **entidades** (`@Audited`) | Flyway / discovery |

### 1.3 Gateway × Discovery (ouro anti-confusão)

| | **API Gateway (Zuul)** | **Service Discovery (Eureka)** |
|---|------------------------|--------------------------------|
| Papel | **Borda** / fachada para o mundo externo | **Catálogo** de quem está onde |
| Cliente típico | Browser, app, sistema externo | Gateway, outros serviços, LB |
| Ação | Roteia + políticas transversais | Registro + lookup + remoção de mortos |
| Endereço | URL **estável** pública | Host:porta **dinâmicos** das réplicas |
| Frase | “Entra por mim” | “Pergunte onde está o serviço X” |

Complementares: o gateway **usa** o discovery para achar a réplica; **não o substitui**.

Consequência cobrada (TRT-6/2025): com Gateway + Discovery, o frontend **não** configura IP/porta de cada serviço; a borda reduz/abstrai chamadas externas.

### 1.4 Eureka — cartão seco

- **Eureka Server** = registry.  
- **Eureka Client** = app que se registra e/ou consulta (`@EnableEurekaClient` na classe Boot).  
- Instância sobe → **registra**; manda **heartbeat**; some do catálogo se morrer.  
- Porta padrão clássica do server: **8761** em `application.properties` / `application.yml` (não `pom.xml`, não `eureka-server.xml`).  
- Nome lógico (`servico-processos`) ≠ IP fixo.

Erro FCC: chamar Eureka de “ponto único de entrada” (isso é Zuul/Gateway).

### 1.5 Zuul — cartão seco

- Zuul = **API Gateway** Netflix no Spring Cloud clássico do edital.  
- Anotação clássica: **`@EnableZuulProxy`** na classe principal Boot.  
- Filtros: **pre** (antes — auth típica) → **route** → **post**; **error** se falha.  
- Roteia path → serviço; destino físico via Eureka.  
- Em prova TRT-4/2022: **Netflix Zuul como edge server** (proteger/expor seletivamente microsserviços).

Erro FCC: `@ZuulServerApplication`, `@EnabledZuulEdgeServer`, trocar Zuul por Eureka/Swagger.

### 1.6 Spring Boot × Spring Cloud

| Boot | Cloud |
|------|-------|
| Uma aplicação: bootstrap, starters, embedded server, jar | Várias apps: discovery, gateway, config distribuída… |
| “Como este serviço existe e sobe?” | “Como serviços se acham / entram / se configuram?” |

Convívio: a **mesma** app é Boot **e** pode ser Cloud client (ex.: Eureka Client). Boot ≠ Eureka; Cloud ≠ “só Boot rebatizado”.

Config Server / Stream: reconheça nomes; a FCC já cobrou componente Cloud + função do **Config** (centralizar config, atualização dinâmica) — leia o enunciado com cuidado (às vezes o componente e a função vêm “pareados” em alternativas longas).

Flash Boot: `spring.main.lazy-initialization=true`; `spring.jpa.hibernate.ddl-auto=none` (desliga schema auto).

### 1.7 OAuth2 / Keycloak (flash PDPJ)

| Papel OAuth | Encaixe típico |
|-------------|----------------|
| Authorization Server | **Keycloak** |
| Resource Server | API / microsserviço |
| Client | Frontend / módulo cadastrado |
| Resource Owner | Usuário |

- SSO = uma autenticação → vários sistemas.  
- Access token no header: `Authorization: Bearer …`  
- Preferir **Authorization Code** (+ PKCE na higiene moderna / RFC 9700); **Implicit** não é “o mais seguro”.  
- Keycloak **emite** tokens (AS); Gateway pode **validar** na borda — papéis distintos.  
- Refresh token: Keycloak **suporta** (distrator TRT-1/2025 diz o contrário).

### 1.8 Mapa de erros típicos FCC (checklist)

1. Eureka = gateway / ponto de entrada.  
2. Zuul = discovery / “cliente do Eureka” como definição principal.  
3. Boot = Cloud (sinônimo).  
4. Swagger = migration / Flyway = documentação REST (inversão TRF4/2025).  
5. Keycloak “só federação externa”; Implicit “sempre preferido”; sem refresh token.  
6. Frontend gerencia IP/porta de cada microsserviço com Gateway+Discovery.  
7. Anotações inventadas (`@EurekaClient` sem Enable; `@ZuulProxyApplication`).  
8. Porta Eureka = 8080 / arquivo = `pom.xml`.  
9. PDPJ = monólito único ou “só o PJe local”.  
10. Auth Server = API Gateway.

### 1.9 Ordem de estudo nesta revisão

1. Decorar tabela Gateway × Eureka × Boot × Cloud.  
2. Anotações: `@EnableZuulProxy`, `@EnableEurekaClient`.  
3. Flash Keycloak/OAuth (AS, Bearer, grants).  
4. Resolver treino 1–15 sem olhar material.  
5. Bloco real 16–25; anotar no caderno de erros só o que errou.

---

## 2. Questões de treino (1–15)

**1.** Na leitura cobrável da PDPJ-Br, a plataforma é prioritariamente:

A) Um monólito nacional que substitui todos os sistemas judiciais  
B) Um ecossistema/plataforma de integração e serviços sob diretrizes nacionais  
C) Sinônimo exclusivo de Eureka  
D) Apenas o portal institucional do CNJ  
E) Um único banco físico obrigatório para todos os TRTs  

**2.** A pergunta mental do API Gateway é:

A) Onde está a réplica saudável do serviço X?  
B) Por onde o cliente externo entra e é roteado?  
C) Qual script Flyway falta aplicar?  
D) Qual `@Entity` auditar?  
E) Qual `groupId` do Maven?  

**3.** Service Discovery (Eureka) resolve prioritariamente:

A) Documentação OpenAPI  
B) Mapeamento Entity↔DTO  
C) Localização dinâmica de instâncias via registry  
D) Emissão de refresh tokens  
E) Substituição do Spring Boot  

**4.** Relação correta Gateway × Eureka:

A) São sinônimos  
B) Eureka é a borda pública; Zuul é o registry  
C) Complementares: gateway usa o catálogo; não o substitui  
D) Eureka elimina a necessidade de gateway sempre  
E) Gateway registra heartbeats no lugar do client  

**5.** Anotação clássica para app Boot atuar como Zuul Proxy:

A) `@ZuulServerApplication`  
B) `@EnableZuulProxy`  
C) `@EnabledZuulEdgeServer`  
D) `@ZuulProxyApplication`  
E) `@IsZuulGateway`  

**6.** Porta padrão clássica do Eureka Server e arquivo típico:

A) 8080 e `pom.xml`  
B) 80 e `eureka-server.xml`  
C) 8761 e `application.properties`  
D) 8005 e `eureka-registry.xml`  
E) 15000 e `application-server.yml`  

**7.** Para a app Boot atuar como **cliente** Eureka, a anotação cobrada é:

A) `@EurekaClient`  
B) `@EnableEurekaClient`  
C) `@IsEurekaClient`  
D) `@EurekaDiscoveryClient`  
E) `@EnableDiscoveryEurekaClient`  

**8.** Spring Boot descreve prioritariamente:

A) O registry de microsserviços  
B) O edge server Netflix  
C) Como montar/empacotar/subir a aplicação  
D) O Authorization Server OAuth  
E) A migração versionada de schema  

**9.** Spring Cloud acrescenta prioritariamente:

A) Apenas um novo nome para o Boot  
B) Padrões de sistemas distribuídos (discovery, gateway, config…)  
C) Substituição obrigatória do Hibernate  
D) O SGBD PostgreSQL  
E) MapStruct  

**10.** Em Zuul, autenticação “ainda na borda, antes do serviço” associa-se tipicamente a filtro:

A) post  
B) route apenas  
C) pre  
D) só error  
E) swagger  

**11.** No desenho SSO/PDPJ, o Keycloak atua tipicamente como:

A) Eureka Server  
B) Authorization Server / IdP (emissão de tokens)  
C) Flyway  
D) Resource Server exclusivo sem emitir token  
E) Service Discovery  

**12.** Forma usual de enviar access token a uma API:

A) Cookie `Flyway-Token`  
B) `Authorization: Bearer <access_token>`  
C) Query `?eureka=`  
D) Header `Zuul-Only`  
E) Body obrigatório sem header  

**13.** Distrator clássico: dizer que Netflix Eureka tem como função principal:

A) Registry / descoberta de serviços  
B) Ser ponto único de entrada distribuindo requisições (papel de gateway)  
C) Heartbeat de instâncias  
D) Lookup por nome lógico  
E) Remoção de instâncias mortas  

**14.** Com Gateway + Discovery, o frontend tipicamente:

A) Configura IP/porta de cada microsserviço à mão  
B) Gerencia endpoint de cada réplica individual  
C) Fala com a borda estável; não precisa da topologia interna completa  
D) Substitui o Keycloak  
E) Desliga o Eureka  

**15.** Síntese da revisão:

A) Boot sobe o serviço; Cloud traz padrões; Zuul entra; Eureka localiza; Keycloak autentica/autoriza com tokens  
B) Eureka = Zuul = Boot = Keycloak  
C) PDPJ = monólito + Implicit Grant obrigatório  
D) Swagger versiona schema; Flyway documenta REST  
E) `@EnableZuulProxy` habilita o Eureka Server  

---

### Gabarito comentado (treino 1–15)

1. **B** — Ecossistema/plataforma, não monólito.  
2. **B** — Entrada/roteamento externo.  
3. **C** — Catálogo dinâmico.  
4. **C** — Complementares.  
5. **B** — `@EnableZuulProxy`.  
6. **C** — 8761 + `application.properties`.  
7. **B** — `@EnableEurekaClient`.  
8. **C** — Runtime/bootstrap da app.  
9. **B** — Padrões distribuídos.  
10. **C** — Filtro pre.  
11. **B** — AS/IdP.  
12. **B** — Bearer.  
13. **B** — Inversão gateway×discovery (erro a evitar).  
14. **C** — Abstração pela borda.  
15. **A** — Síntese correta.

Letras treino: B, B, C, C, B, C, B, C, B, C, B, B, B, C, A.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** FCC (preferência Analista TI / TRT-TRF). Sem invenção. Gabarito só no final.

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

**18.** (FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60)

O Spring Eureka é uma implementação do padrão de registro de serviços que permite que os microsserviços se registrem automaticamente e se descubram uns aos outros. A porta padrão em que o servidor Eureka irá executar na aplicação Spring Boot e o arquivo em que essa porta é configurada são, respectivamente,

A) `80` e `eureka-server.xml`  
B) `15000` e `application-server.yml`  
C) `8080` e `pom.xml`  
D) `8005` e `eureka-registry.xml`  
E) `8761` e `application.properties`  

**19.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60)

Na classe principal de uma aplicação criada com Spring Boot, para fazer com que a aplicação atue como um cliente Eureka utiliza-se a anotação

A) `@EurekaClient`  
B) `@EnableEurekaClient`  
C) `@EnableDiscoveryEurekaClient`  
D) `@EurekaDiscoveryClient`  
E) `@IsEurekaClient`  

**20.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22)

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.  
B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.  
C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.  
D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.  
E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.  

**21.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60)

Ao usar o Gateway API com o Service Discovery no contexto da PDPJ-Br, a principal consequência para o frontend em uma aplicação baseada em microsserviços é que o

A) Gateway API diminui a necessidade de autenticação, uma vez que o Service Discovery é integrado ao SSO.  
B) frontend deve configurar manualmente os IPs e portas dos serviços disponíveis na plataforma.  
C) frontend se torna o responsável pela chamada direta de cada microsserviço individualmente.  
D) Gateway API reduz o número de chamadas externas que o frontend precisa fazer, tornando a comunicação mais eficiente.  
E) frontend deve gerenciar manualmente as atualizações de endpoint de cada serviço individual registrado.  

**22.** (FCC | TRT-1 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 55 de 60)

Um Tribunal Regional do Trabalho deseja implementar um sistema de autenticação centralizado para seus diversos aplicativos internos e externos. Decidiu, para isso, utilizar o Keycloak como Identity Provider (IdP) e configurar a autenticação e autorização dos usuários por meio do OAuth2. Durante a implementação, a equipe de segurança precisa garantir que os fluxos de autenticação estejam corretamente configurados e que os aplicativos tenham acesso seguro aos recursos protegidos. Nesse cenário,

A) o Keycloak pode atuar como um Authorization Server dentro do fluxo OAuth2, permitindo a emissão de tokens de acesso e refresh tokens para aplicações cliente.  
B) o Keycloak apenas suporta OAuth2 para autenticação federada, sendo necessário utilizar um provedor externo como o Azure AD ou Google Identity para permitir login unificado.  
C) no OAuth2, ao utilizar Keycloak como IdP, o fluxo de Implicit Grant deve ser sempre a opção preferida para aplicações web modernas, pois é o mais seguro e recomendado atualmente.  
D) aplicações cliente que precisam acessar APIs protegidas devem utilizar o Authorization Code Lazy (ACL) combinado com Proof Authority for Code Exchange (PACE) para aumentar a segurança na troca de tokens.  
E) o protocolo OAuth2, implementado no Keycloak, permite a geração de Access Tokens e ID Tokens, mas não suporta Refresh Tokens para renovação de sessão.  

**23.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 43 de 60)

As versões mais recentes do Spring Boot permitem a configuração de inicialização lenta (lazy initialization), que faz com que os beans sejam criados à medida que são necessários e não durante a inicialização do aplicativo. Uma das maneiras de habilitar a inicialização lenta é colocar

A) `@Lazy(true)` no arquivo `application.properties`  
B) `spring.main.lazy-initialization=true` no arquivo `application.properties`  
C) `set-lazy-initialization=true` no arquivo `spring-boot.xml`  
D) `lazy-initialization=true` no arquivo `spring-application-builder.xml`  
E) `@SpringMainLazyInitialization` na classe `SpringApplicationBuilder`  

**24.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60)

Correspondem, correta e respectivamente, a um componente do Spring Cloud e à principal função do Spring Cloud Config:

A) Spring Security — implementar um padrão de design para a comunicação entre serviços baseados em REST.  
B) Spring Boot Actuator — gerenciar o ciclo de vida e o monitoramento de aplicações distribuídas em uma arquitetura de microsserviços.  
C) Spring Data JPA — oferecer uma solução para o balanceamento de carga entre instâncias de microsserviços.  
D) Spring Cloud Stream — centralizar a configuração de aplicações e serviços, permitindo que configurações sejam atualizadas dinamicamente.  
E) Spring Web MVC — fornecer um serviço de autenticação e autorização para microsserviços e aplicações.  

**25.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60)

Na abordagem do Spring para construir serviços web RESTful, as solicitações HTTP são tratadas por um controlador, que é uma classe identificada com a anotação

A) `@RestfulController`  
B) `@RestfulMapping`  
C) `@RestApplicationController`  
D) `@RestController`  
E) `@RequestController`  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Citação |
|---|------|---------|
| 16 | **A** | FCC \| TRT23 2022 \| Analista TI \| Q38 |
| 17 | **D** | FCC \| TRT4 2022 \| Técnico TI \| Q38 |
| 18 | **E** | FCC \| TRT15 2023 \| Analista TI \| Q31 |
| 19 | **B** | FCC \| TRT19 2022 \| Analista TI \| Q32 |
| 20 | **E** | FCC \| TRF4 2025 \| Analista Análise de Sistemas \| Q22 |
| 21 | **D** | FCC \| TRT6 2025 \| Analista TI \| Q39 |
| 22 | **A** | FCC \| TRT-1 2025 \| Analista TI \| Q55 |
| 23 | **B** | FCC \| TRT22 2022 \| Analista TI \| Q43 |
| 24 | **D** | FCC \| TRT20 2024 \| Analista TI \| Q53 |
| 25 | **D** | FCC \| TRT19 2022 \| Analista TI \| Q31 |

Letras: A, D, E, B, E, D, A, B, D, D.

**Fontes (Quizinho / bases de prova):**
- Q16: https://www.quizinho.com.br/questao/fcc-trt23-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q38
- Q17: https://www.quizinho.com.br/questao/fcc-trt4-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q38
- Q18: https://www.quizinho.com.br/questao/fcc-trt15-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q31
- Q19: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q32
- Q20: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22
- Q21: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q39
- Q22: https://www.quizinho.com.br/questao/fcc-trt-1-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q55
- Q23: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q43
- Q24: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q53
- Q25: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q31

---

### Verificação interna

| Checagem | Status |
|----------|--------|
| Revisão Gateway×Discovery / Eureka / Zuul / Boot×Cloud / Keycloak | OK |
| Mapa de erros FCC | §1.8 |
| Treino 1–15 gabarito distribuído | OK |
| 16–25 reais + citação | 10/10 |
| Gabarito só no final | OK |

*Arquivo 01 · Revisão PDPJ-Br + Spring · Semana 11 · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02-revisao-sql-jpa.md.*

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

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto de microsserviços alinhado à stack Spring referida no PDPJ, Spring Boot e Spring Cloud relacionam-se, em regra, da seguinte forma:

A) o Boot concentra o bootstrap/empacotamento da aplicação, enquanto o Cloud agrega padrões de sistemas distribuídos  
B) são nomes intercambiáveis para o mesmo artefato de discovery  
C) o Cloud substitui integralmente o Boot em qualquer serviço  
D) o Boot é, por definição, o API Gateway Netflix  
E) ambos equivalem ao MapStruct  

**17.** Considere um enunciado que destaque “servidor embutido”, “starters” e “aplicação Spring executável em jar”. A associação mais adequada é:

A) Spring Cloud Config Server exclusivamente  
B) Eureka Dashboard como único significado  
C) Spring Boot  
D) Filtro post do Zuul  
E) Swagger UI  

**18.** Em arquitetura com vários serviços elásticos, a necessidade de **localizar dinamicamente** instâncias e de **expor borda única** com roteamento remete, no guarda-chuva Spring, principalmente a:

A) recursos exclusivos do Boot sem qualquer padrão Cloud  
B) apenas MapStruct e Flyway  
C) dialect SQL embutido no Boot  
D) padrões Spring Cloud (ex.: Eureka e Zuul)  
E) OpenAPI sem gateway nem discovery  

**19.** Assinale a alternativa que apresenta associação **incorreta**:

A) Spring Boot — empacotamento e execução da app  
B) Spring Cloud — padrões distribuídos  
C) Eureka — Service Discovery  
D) Zuul — API Gateway  
E) Spring Boot — catálogo central de heartbeats de réplicas (papel típico do Eureka)  

**20.** Um serviço de negócios desenvolvido com Spring Boot passa a utilizar cliente de Service Discovery. É correto concluir que:

A) o serviço deixa automaticamente de ser aplicação Boot  
B) o Boot torna-se o Zuul  
C) o serviço permanece Boot e passa a empregar capacidade associada ao Spring Cloud  
D) o discovery elimina a noção de HTTP  
E) MapStruct assume o papel de registry  

**21.** Diferenciar Spring Boot de Spring Cloud em provas da FCC é relevante porque:

A) a banca costuma explorar a confusão entre ferramenta de bootstrap da app e padrões de coordenação distribuída  
B) não há diferença conceitual cobrável entre os termos  
C) Cloud é apenas o nome do servidor Tomcat embutido  
D) Boot é o único nome correto para Eureka e Zuul  
E) a diferença só existe em SOAP  

**22.** Relativamente ao Spring Cloud Config, a leitura mais alinhada ao edital é:

A) trata-se de sinônimo de `@SpringBootApplication`  
B) substitui o Eureka como registry de heartbeats  
C) é o nome antigo do filtro pre do Zuul  
D) refere-se a padrão/peça de configuração distribuída/centralizada no ecossistema Cloud, distinto do mero yml local do Boot  
E) documenta contratos OpenAPI interativos  

**23.** Em questão que contraste papéis, a FCC tipicamente considera **correto** que:

A) Spring Boot, sozinho, é o ponto único de entrada pública e o registry ao mesmo tempo  
B) Zuul e Eureka são features internas exclusivas do MapStruct  
C) Spring Cloud fornece o ferramental de padrões (gateway, discovery, config), usualmente sobre aplicações Boot  
D) usar Boot torna impossível registrar-se no Eureka  
E) Cloud e Boot nunca coexistem no mesmo processo  

**24.** Qual enunciado descreve **incorretamente** o Spring Boot?

A) Favorece auto-configuration e starters  
B) Permite empacotar aplicação com servidor embutido  
C) É a forma típica de subir microsserviços na stack Spring do edital  
D) É, por definição, o componente de Service Discovery denominado Eureka  
E) Não se confunde com Swagger nem com MapStruct  

**25.** A síntese mais alinhada ao edital, sobre Spring Boot × Spring Cloud, é:

A) Boot constrói/sobe o serviço; Cloud acrescenta padrões distribuídos (discovery, gateway, config); Eureka e Zuul são peças Cloud, não sinônimos de Boot  
B) Boot = Cloud = Eureka = Zuul (quatro nomes, um papel)  
C) Cloud elimina Boot; Boot é só documentação de API  
D) Boot é gateway; Cloud é apenas o heartbeat  
E) A distinção Boot/Cloud foi abolida em todas as bancas  

---

### Gabarito comentado (concurso 16–25)

16. **A** — Boot bootstrap; Cloud padrões distribuídos.  
17. **C** — Starters/jar/servidor embutido = Boot.  
18. **D** — Localizar + borda = Cloud (Eureka/Zuul).  
19. **E** — Catálogo/heartbeat = Eureka, não Boot.  
20. **C** — Continua Boot + ganha Cloud.  
21. **A** — FCC explora confusão bootstrap × distribuição.  
22. **D** — Config distribuída ≠ só yml local.  
23. **C** — Cloud sobre (tipicamente) Boot.  
24. **D** — Discovery = Eureka; Boot ≠ Eureka.  
25. **A** — Cartão mental completo do arquivo 07.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **como a app sobe / starters / jar / auto-config**? → **Boot**.  
2. É **padrão entre vários serviços** (achar, entrar, config central)? → **Cloud**.  
3. É **onde está a réplica**? → **Eureka** (Cloud).  
4. É **entrada/rota/filtro**? → **Zuul** (Cloud).  
5. Boot e Cloud **sinônimos**? → **Nunca** na leitura de prova deste arquivo.

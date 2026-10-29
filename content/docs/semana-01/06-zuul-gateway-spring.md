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

## 3. Questões estilo concurso FCC (16–25)

**16.** Em arquiteturas de microsserviços baseadas na stack Spring Cloud Netflix referida no contexto PDPJ, o Zuul caracteriza-se, em regra, por:

A) funcionar como catálogo exclusivo de registro e heartbeat de todas as réplicas  
B) atuar como API Gateway, expondo ponto de entrada e encaminhando requisições aos serviços  
C) gerar, em tempo de compilação, mapeamentos Entity↔DTO  
D) substituir integralmente o protocolo HTTP por SOAP  
E) constituir o dialect SQL utilizado pelo Hibernate  

**17.** No pipeline de filtros do Zuul, o tipo responsável por encaminhar a requisição ao serviço de destino é o:

A) pre  
B) post  
C) route  
D) error  
E) discovery  

**18.** Considere que um componente de borda valida credenciais antes de qualquer chamada ao microsserviço-alvo e, em seguida, realiza o proxy da chamada. No modelo de filtros do Zuul, essa validação antecipada associa-se tipicamente ao filtro:

A) post  
B) pre  
C) exclusivamente ao Eureka Client  
D) MapStruct  
E) Flyway  

**19.** Relativamente à convivência entre Zuul e Eureka, assinale a opção correta:

A) O Eureka substitui o Zuul como única porta pública do cliente externo  
B) O Zuul substitui o Eureka como registry de instâncias  
C) Em desenho clássico, o Zuul pode utilizar o Eureka para localizar instâncias ao rotear  
D) Ambos são nomes comerciais do Swagger UI  
E) A existência do Zuul torna o Service Discovery conceitualmente impossível  

**20.** Diferenciar Spring Boot de Zuul é relevante em provas porque:

A) Spring Boot é sinônimo estrito de API Gateway Netflix  
B) Spring Boot refere-se ao empacotamento/execução da aplicação, ao passo que Zuul designa o papel de gateway  
C) Zuul é o único mecanismo de discovery do Boot  
D) Boot registra heartbeats; Zuul só documenta APIs  
E) Não há diferença cobrável entre os termos  

**21.** Um enunciado afirma: “Após a resposta do microsserviço, o gateway acrescenta cabeçalhos de correlação e registra métricas de saída.” No vocabulário de filtros Zuul, a etapa descrita corresponde, em regra, ao filtro:

A) pre  
B) route  
C) post  
D) Eureka Server  
E) error, necessariamente, mesmo em sucesso  

**22.** Assinale a alternativa que apresenta associação **incorreta**:

A) Zuul — API Gateway  
B) Eureka — Service Discovery  
C) Filtro error — tratamento de falhas no pipeline Zuul  
D) Spring Boot — catálogo dinâmico de instâncias (papel do Eureka)  
E) Filtro pre — intervenções antes do encaminhamento  

**23.** Em questão que contraste papéis na borda e no registry, a FCC tipicamente considera **correto** que:

A) o cliente externo utiliza o Eureka como única URL pública e o Zuul apenas lista instâncias mortas  
B) o Zuul concentra entrada/roteamento/filtros e o Eureka responde pela localização dinâmica de instâncias  
C) Zuul e Eureka são intercambiáveis em qualquer enunciado  
D) o filtro route do Zuul realiza o heartbeat das réplicas  
E) Spring Boot elimina a noção de API Gateway  

**24.** Quando ocorre falha durante o processamento dos filtros ou do roteamento no Zuul, o tipo de filtro previsto para lidar com essa situação é o:

A) pre exclusivamente  
B) post exclusivamente  
C) error  
D) MapStruct  
E) Swagger  

**25.** A síntese mais alinhada ao edital, sobre Zuul no Spring Cloud, é:

A) Zuul = discovery; Boot = gateway; Eureka = documentação  
B) Zuul = API Gateway com rotas e filtros (pre/route/post/error), podendo integrar-se ao Eureka, sem confundir-se com Spring Boot  
C) Zuul obriga banco único compartilhado entre microsserviços  
D) Filtros Zuul existem apenas na variante post  
E) Eureka é ponto único de entrada; Zuul é apenas o nome antigo do MapStruct  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Zuul = API Gateway (entrada + encaminhamento).  
17. **C** — route = encaminhar ao destino.  
18. **B** — Validação antecipada ≈ pre.  
19. **C** — Complementares: gateway usa discovery.  
20. **B** — Boot ≠ papel de gateway; Boot empacota/roda.  
21. **C** — Após resposta = post.  
22. **D** — Catálogo = Eureka, não Boot.  
23. **B** — Duo clássico sem inverter.  
24. **C** — error no pipeline.  
25. **B** — Cartão mental completo do arquivo 06.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **entrada/rota/filtro**? → Zuul.  
2. É **onde está a réplica / registry**? → Eureka.  
3. É **como a app sobe/jar**? → Boot.  
4. Momento do filtro: ainda não foi / está indo / já voltou / falhou → **pre / route / post / error**.
)

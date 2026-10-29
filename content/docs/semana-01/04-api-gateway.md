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

## 3. Questões estilo concurso FCC (16–25)

**16.** Em arquiteturas baseadas em microsserviços, o API Gateway caracteriza-se, em regra, por:

A) constituir o ponto de entrada único para consumidores externos, com encaminhamento das requisições aos serviços competentes  
B) manter, como função exclusiva e sinônima, o registro de instâncias e a remoção por ausência de *heartbeat*  
C) substituir integralmente a necessidade de contratos HTTP e de códigos de status  
D) atuar apenas como biblioteca de mapeamento entre entidades e DTOs em tempo de compilação  
E) eliminar qualquer possibilidade de autenticação na borda da aplicação  

**17.** Acerca das funções tipicamente associadas a um API Gateway, assinale a alternativa correta.

A) Compete-lhe, em desenho usual, apenas a persistência transacional de todos os agregados de negócio  
B) Pode concentrar políticas transversais, a exemplo de autenticação na borda e limitação de taxa de requisições, além do roteamento  
C) Sua existência torna tecnicamente impossível a coexistência com mecanismos de descoberta de serviços  
D) Destina-se exclusivamente à geração de documentação OpenAPI interativa  
E) Equivale, por definição, ao servidor de *service discovery*  

**18.** No contraste entre API Gateway e Service Discovery, é correto afirmar que:

A) ambos respondem à mesma pergunta operacional e são intercambiáveis em qualquer enunciado  
B) o Gateway ocupa-se da entrada/roteamento perante o cliente externo, ao passo que o Discovery responde pela localização dinâmica das instâncias  
C) o Discovery é a fachada pública obrigatória e o Gateway apenas cataloga IPs  
D) o Gateway substitui o protocolo HTTP pelo acesso direto ao filesystem  
E) o Discovery implementa, por si só, rate limiting como definição do padrão  

**19.** Considere o uso de **Zuul** em stack Spring Cloud alinhada ao conteúdo programático de Analista de TI de tribunal. Assinale a alternativa correta.

A) Zuul funciona primariamente como servidor de descoberta e registro de instâncias  
B) Zuul atua como gateway, com papel de borda para roteamento (e filtros/políticas) perante os serviços  
C) Zuul é biblioteca de mapeamento *compile-time* entre POJOs  
D) Zuul substitui o Spring Boot como *runtime* embutido da aplicação  
E) Zuul é o padrão OpenAPI de documentação interativa  

**20.** Considere as afirmações abaixo.

I. O API Gateway pode reduzir o acoplamento do cliente à topologia interna dos microsserviços.  
II. Políticas como autenticação na borda e *rate limiting* são exemplos de preocupações transversais frequentemente situadas no gateway.  
III. O Service Discovery responde, em essência, à pergunta sobre a localização das instâncias disponíveis.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**21.** Assinale a opção que **inverte** indevidamente os papéis típicos de Zuul e Eureka no ecossistema Spring Cloud clássico cobrado em concursos.

A) Zuul = gateway/roteamento de borda; Eureka = discovery/registro  
B) Zuul = registry de instâncias; Eureka = filtro/entrada de borda HTTP  
C) Cliente externo utiliza o gateway como fachada; o discovery informa endereços das réplicas  
D) Eureka mantém catálogo; Zuul encaminha tráfego na borda  
E) A associação de prova liga Zuul à entrada e Eureka à localização  

**22.** A composição de respostas de múltiplos microsserviços em uma única resposta devolvida ao cliente, quando realizada na borda, relaciona-se à ideia de:

A) exclusividade do banco H2 em produção  
B) agregação / composição via API Gateway (ou camada de borda equivalente)  
C) proibição de JSON em APIs REST  
D) função exclusiva do Hibernate Envers  
E) substituição do gateway pelo MapStruct  

**23.** Em enunciado que descreve componente responsável por “saber quais réplicas do serviço de petições estão saudáveis e em qual host/porta”, sem enfatizar a fachada pública ao usuário final, o papel mais adequado é o de:

A) API Gateway como única porta documental Swagger  
B) MapStruct  
C) Service Discovery  
D) Flyway  
E) Zuul pré-filtro como sinônimo de registry  

**24.** Para fins de preparação no eixo API Gateway do edital (PDPJ-Br / Analista TI), a síntese mais adequada é:

A) Gateway e Discovery são o mesmo artefato; Zuul documenta OpenAPI; Eureka mapeia Entity↔DTO  
B) Gateway (Zuul, no edital) = entrada única, roteamento e políticas de borda, podendo agregar; Discovery (Eureka) = catálogo de instâncias — papéis distintos e complementares  
C) Rate limiting só existe dentro do banco PostgreSQL  
D) O cliente externo deve, por definição, conhecer todos os IPs internos sem passar por fachada  
E) Agregação obriga o uso de ESB clássico e proíbe HTTP  

**25.** Assinale a afirmação **incompatível** com o papel típico de um API Gateway em microsserviços.

A) Oferecer ponto estável de entrada ao consumidor externo  
B) Encaminhar chamadas conforme rotas definidas  
C) Aplicar controles transversais na borda, como autenticação e limitação de taxa  
D) Servir como catálogo dinâmico de registro/remoção de instâncias por *heartbeat*, confundindo-se com o Service Discovery  
E) Possibilitar composição de chamadas internas em cenários de agregação  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **A** |
| 17 | **B** |
| 18 | **B** |
| 19 | **B** |
| 20 | **E** |
| 21 | **B** |
| 22 | **B** |
| 23 | **C** |
| 24 | **B** |
| 25 | **D** |

**Notas rápidas:** 16 A (entrada única + encaminhamento); 17 B (cross-cutting + roteamento); 18 B (entra × localiza); 19 B (Zuul = gateway); 20 E (I–III ok); 21 B (única que **inverte** Zuul/Eureka); 22 B (agregação na borda); 23 C (localização = Discovery); 24 B (síntese do subtópico); 25 D (registry/heartbeat ≠ Gateway).

---

*Arquivo 04 · API Gateway · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 05 Service Discovery / Eureka.*

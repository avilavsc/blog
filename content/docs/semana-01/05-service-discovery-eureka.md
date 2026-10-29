+++
title = '05 — Service Discovery / Eureka'
date = '2026-10-05'
draft = false
weight = 5
tags = ['semana-01', 'pdpj', 'eureka']
categories = ['estudo']
+++
# Service Discovery / Eureka · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — Service Discovery / Eureka  
**Escopo deste arquivo:** o problema que a descoberta resolve, *service registry*, papéis Eureka Server × Eureka Client, saúde / *heartbeat* / remoção de instâncias mortas, contraste seco com API Gateway e comparação configuração estática × descoberta dinâmica.  
**Fora de escopo aqui:** filtros Zuul / Spring Cloud Gateway (arquivo 06), Spring Boot × Cloud em profundidade (07), Swagger, MapStruct, REST/JSON aprofundado (03). Gateway já foi tratado no 04 — aqui só o contraste anti-confusão.

---

## 1. Material

### 1.1 O problema: por que “descobrir” serviços?

Em monólito, o cliente interno “sabe” onde chamar: mesmo processo, mesma JVM, ou poucos hosts fixos. Em **microsserviços**, a topologia muda o tempo todo:

- várias **réplicas** do mesmo serviço (escala horizontal);
- **IPs e portas** que mudam a cada deploy/container;
- instâncias que **sobem e caem** (falha, rolling update, autoscaling).

Se cada consumidor gravar “sempre chame `192.168.10.44:8083`”, o desenho **quebra** assim que a réplica muda. O problema cobrável é:

> **Como um cliente (serviço, gateway, load balancer) encontra, em tempo de execução, uma instância saudável do serviço X?**

**Service Discovery** responde a essa pergunta: mantém (ou consulta) um **catálogo dinâmico** de “quem está onde” e permite **localizar** instâncias sem hardcode de endereço.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Pergunta | **Onde** está (agora) uma réplica saudável do serviço X? |
| Artefato | **Registry / catálogo** de instâncias (nome → host/porta/metadados) |
| Dinâmica | Endereços **mudam**; o catálogo **acompanha** o ciclo de vida |

Discovery **não** é: ponto único de entrada pública (isso é Gateway), documentação OpenAPI (Swagger), nem mapper Entity↔DTO (MapStruct).

### 1.2 Service Registry (o catálogo)

O **service registry** é o componente (ou papel) que **guarda o mapa** das instâncias registradas:

| Conceito | Ideia de prova |
|----------|----------------|
| **Registro** | Ao subir, a instância **anuncia-se**: “sou o serviço `pedidos`, estou em host:porta” |
| **Consulta / lookup** | Consumidores perguntam: “quais instâncias de `pedidos` existem?” |
| **Atualização** | Catálogo reflete **alta/baixa** de réplicas |
| **Metadados** | Além de IP/porta: nome lógico, status, às vezes zona, versão etc. |

Leitura FCC: o registry é o **coração** do discovery. Sem algum mecanismo de catálogo (centralizado ou equivalente), a “descoberta dinâmica” vira só slogan.

Flash: **nome lógico do serviço** (ex.: `servico-processos`) ≠ endereço físico. O cliente resolve o nome via registry → obtém endereço(s) concreto(s).

### 1.3 Configuração estática × descoberta dinâmica

Contraste cobrável (ouro de prova):

| | **Configuração estática** | **Descoberta dinâmica (Service Discovery)** |
|---|---------------------------|---------------------------------------------|
| Endereços | Fixos em arquivo, YAML, DNS estático “na mão” | Obtidos do **registry** em tempo de execução |
| Escala / fail | Mudar IP → **reeditar** config em N lugares | Nova réplica **registra-se**; morta **some** do catálogo |
| Acoplamento | Consumidor acoplado a topologia física | Consumidor acoplado a **nome lógico** + registry |
| Adequação | Poucos hosts estáveis | Microsserviços com réplicas e deploys frequentes |

Armadilha: achar que “estático = sempre errado”. Em prova, o ponto é: em ecossistema de microsserviços **elástico**, hardcode de host/porta **não escala** operacionalmente; discovery é o padrão cobrado para **localização dinâmica**.

Síntese: estático = “eu já sei o endereço”; dinâmico = “pergunto ao catálogo **agora**”.

### 1.4 Modelos mentais de discovery (visão de prova)

Sem virar tutorial de produto, a banca espera reconhecer a ideia:

1. **Cliente consulta o registry** (direta ou via biblioteca/cliente) e chama a instância.  
2. Em stacks Netflix/Spring Cloud do edital, **Eureka** é a face clássica desse registry.  
3. Gateway / balanceador **pode usar** o discovery para escolher a réplica — papéis **complementares**, não sinônimos.

Há literaturas com “client-side discovery” vs “server-side discovery”; para FCC neste eixo, basta:

- existe um **catálogo**;
- instâncias **registram** e são **encontráveis**;
- o consumidor **não depende** de IP fixo eternamente.

### 1.5 Eureka no edital (Spring Cloud / Netflix)

No conteúdo programático alinhado à PDPJ / stack clássica Spring Cloud Netflix:

| Conceito | Associação de prova |
|----------|---------------------|
| Padrão | **Service Discovery** |
| Produto clássico cobrado | **Eureka** |
| Par anti-confusão | **Eureka = localização** · **Zuul = entrada/gateway** |

Eureka, na leitura de concurso, é o **servidor de registro** (e o ecossistema de clientes que se registram e consultam). Não precisa decorar propriedade YAML linha a linha; precisa dominar **papéis e fluxo**.

### 1.6 Eureka Server × Eureka Client

Divisão cobrável:

| Papel | Função |
|-------|--------|
| **Eureka Server** | Mantém o **registry**: recebe registros, responde lookups, acompanha saúde via *heartbeats* |
| **Eureka Client** | Aplicação (microsserviço) que **se registra** no server e/ou **consulta** o catálogo para achar outros serviços |

Pontos de prova:

- O **Server** é o “lugar do catálogo” (pode haver réplicas do próprio Eureka em desenho HA — detalhe avançado; o essencial é o papel de registry).  
- O **Client** faz duas coisas típicas: **register** (eu existo) e **discover** (onde está o outro).  
- Um microsserviço “só de negócio” costuma ser **client** do Eureka; o processo dedicado ao registry é o **server**.

Flash: se o enunciado fala “catálogo central / servidor de discovery” → **Eureka Server**. Se fala “serviço que se anuncia e busca peers” → **Eureka Client**.

### 1.7 Ciclo de vida: registro, heartbeat e saúde

Fluxo mental cobrável:

```
1. Instância sobe
2. Registra-se no Eureka Server (nome lógico + host/porta)
3. Periodicamente envia heartbeat (“ainda estou viva”)
4. Outros clientes/gateway consultam o registry e obtêm a lista
5. Se a instância para de pulsar / fica unhealthy → é removida (ou marcada indisponível)
```

| Mecanismo | Ideia cobrável |
|-----------|----------------|
| **Registro** | Entrada no catálogo ao iniciar |
| **Heartbeat** | Sinal periódico de vida |
| **Health / disponibilidade** | Só (ou preferencialmente) instâncias **saudáveis** devem ser escolhidas |
| **Eviction / deregistro** | Sem heartbeat a tempo → saída do catálogo; shutdown ordenado também **desregistra** |

Leitura FCC: discovery **não** é só “lista de IPs”; é lista **viva**, filtrada pela noção de **saúde**. Chamar uma réplica morta que ainda aparece no YAML estático é exatamente o tipo de dor que o padrão mitiga.

Nuance: health check “profundo” (actuator, readiness) pode existir no ecossistema Spring; em prova, o núcleo é **heartbeat + remoção de instâncias mortas** + ideia de **só usar saudáveis**.

### 1.8 Quem consulta o Eureka? (clientes do catálogo)

Consumidores típicos do registry:

- **outro microsserviço** que precisa chamar `servico-X`;
- **API Gateway** (ex.: Zuul) que roteia para uma réplica concreta;
- componentes de **balanceamento** que precisam da lista de targets.

Isso reforça o contraste com Gateway:

- Gateway **usa** o discovery para achar o destino.  
- Discovery **não** substitui a fachada pública.

### 1.9 Service Discovery × API Gateway (anti-confusão — ouro da FCC)

A banca **adora** inverter Eureka e Zuul. Memorize:

| | **Service Discovery (Eureka)** | **API Gateway (Zuul)** |
|---|-------------------------------|-------------------------|
| Pergunta | **Onde** está a instância saudável? | **Por onde** o cliente **externo** entra? |
| Papel | **Catálogo / registry** dinâmico | **Ponto único de entrada** + roteamento + cross-cutting |
| Face clássica | **Eureka** | **Zuul** |
| Erro típico | Chamar Eureka de “entrada única” | Chamar Zuul de “registry” |

Fluxo mental cobrável (mesmo do arquivo 04):

```
Cliente externo → API Gateway (Zuul) → (consulta) Discovery (Eureka) → microsserviço
```

Regras de ouro:

1. Discovery **localiza**; Gateway **entra/roteia**.  
2. Um **não** é sinônimo do outro.  
3. Em stack dinâmica, o gateway **depende** (tipicamente) do catálogo — mas **não vira** o catálogo.

### 1.10 O que a FCC tipicamente testa neste tema

- Problema: IPs/portas **voláteis** em microsserviços → precisa de discovery.  
- Registry = **catálogo** nome → instâncias.  
- **Eureka** = face clássica de Service Discovery no edital.  
- **Server** = registry; **Client** = registra e/ou consulta.  
- **Heartbeat** / remoção de instâncias mortas / saúde.  
- Estático (hardcode) × **dinâmico** (lookup).  
- Contraste seco com **API Gateway / Zuul**.  
- Fluxo: Cliente → Gateway → Eureka → serviço.

### 1.11 Armadilhas FCC (lista de ouro)

1. Chamar **Eureka** de “ponto único de entrada” / fachada pública.  
2. Chamar **Zuul** de “service registry / catálogo de instâncias”.  
3. Tratar Discovery e Gateway como **sinônimos**.  
4. Achar que discovery **substitui** autenticação, rate limit ou documentação.  
5. Confundir Eureka com **Swagger**, **MapStruct**, **Flyway** ou banco H2.  
6. Dizer que microsserviços **proíbem** qualquer endereço conhecido (exagero); o ponto é evitar hardcode **frágil** em topologia elástica.  
7. Esquecer o **heartbeat**: registry “morto” sem evicção.  
8. Inverter o fluxo: Cliente → Eureka (borda pública) → Zuul (catálogo).  
9. Afirmar que configuração estática e discovery são a **mesma** estratégia.  
10. Misturar Eureka Server (catálogo) com “servidor HTTP de regra de negócio do domínio”.

### 1.12 Mini-síntese (cartão mental)

> **Service Discovery** = **onde** está a réplica **saudável** (catálogo dinâmico).  
> **Registry** = mapa nome lógico → instâncias.  
> **Eureka** (edital) = face clássica do discovery (Server = catálogo; Client = registra/consulta).  
> **Heartbeat** = “ainda vivo”; sem pulso → fora da lista.  
> **Estático** = endereço fixo; **dinâmico** = lookup no registry.  
> **Zuul** = entra; **Eureka** = localiza.  
> Cliente → **Zuul** → (consulta) **Eureka** → serviço.

---

## 2. Questões de treino (1–15)

**1.** Service Discovery resolve, prioritariamente, o problema de:

A) Documentar contratos OpenAPI com try-it-out  
B) Localizar, em tempo de execução, instâncias (host/porta) de um serviço cujo endereço muda  
C) Mapear Entity↔DTO em tempo de compilação  
D) Migrar schema SQL versionado  
E) Substituir HTTP por stored procedures  

**2.** A pergunta mental que melhor distingue o Service Discovery é:

A) “Por onde o cliente externo entra?”  
B) “Onde está agora uma instância saudável do serviço X?”  
C) “Qual annotation gera o Swagger UI?”  
D) “Qual jar embute o Tomcat no Boot?”  
E) “Qual tabela do Envers guarda revisão?”  

**3.** O *service registry* é, em essência:

A) Um banco único compartilhado com JOIN entre todos os microsserviços  
B) O catálogo que associa nomes lógicos de serviço a instâncias (endereços/metadados)  
C) O ponto único de entrada pública com rate limiting  
D) A biblioteca MapStruct  
E) O protocolo SOAP obrigatório  

**4.** Em comparação com configuração estática de hosts, a descoberta dinâmica caracteriza-se por:

A) Exigir que todo IP seja reescrito manualmente a cada escala  
B) Obter endereços do registry em tempo de execução, acompanhando alta/baixa de réplicas  
C) Proibir qualquer nome lógico de serviço  
D) Eliminar a necessidade de APIs HTTP  
E) Ser sinônimo estrito de Swagger Codegen  

**5.** No eixo Spring Cloud Netflix cobrado em concursos alinhados ao edital, a face clássica de Service Discovery é:

A) Zuul  
B) Eureka  
C) MapStruct  
D) Flyway  
E) Hibernate Envers  

**6.** Assinale a associação correta:

A) Eureka Server = ponto único de entrada externa; Eureka Client = rate limit  
B) Eureka Server = mantém o registry; Eureka Client = registra-se e/ou consulta o catálogo  
C) Eureka Server = mapper compile-time; Eureka Client = OpenAPI UI  
D) Eureka = apenas documentação; Zuul = apenas heartbeat  
E) Eureka Client substitui o banco PostgreSQL  

**7.** O *heartbeat* no contexto Eureka/discovery serve principalmente para:

A) Gerar DTOs automaticamente  
B) Sinalizar periodicamente que a instância segue viva, permitindo evicção se parar de pulsar  
C) Autenticar o usuário final na borda pública  
D) Versionar scripts SQL  
E) Renderizar Swagger UI  

**8.** Assinale a associação correta no contraste anti-confusão:

A) Discovery = entrada/roteamento; Gateway = catálogo de instâncias  
B) Discovery/Eureka = catálogo/localização; Gateway/Zuul = entrada/roteamento de borda  
C) Ambos são sinônimos de MapStruct  
D) Eureka documenta OpenAPI; Zuul mapeia Entity↔DTO  
E) Gateway responde “onde está a réplica?”; Discovery responde “por onde entra o externo?”  

**9.** No fluxo típico cobrado (cliente externo + stack clássica do edital):

A) Cliente → Eureka (borda pública) → Zuul (catálogo) → serviço  
B) Cliente → API Gateway/Zuul → (consulta) Discovery/Eureka → microsserviço  
C) Cliente → MapStruct → Flyway → H2  
D) Cliente → Swagger UI → Envers → Eureka Dashboard como única borda  
E) Cliente → SOAP exclusivamente, sem discovery  

**10.** Qual afirmação sobre Service Discovery é **incorreta**?

A) Ajuda a lidar com IPs/portas voláteis  
B) Mantém (via registry) visão atualizada das instâncias  
C) É, por definição, o ponto único de entrada pública do cliente externo (papel do Gateway)  
D) Relaciona-se a heartbeat e remoção de instâncias mortas  
E) Contrasta com hardcode estático de endereços  

**11.** Um enunciado diz: “Ao iniciar, o microsserviço anuncia nome lógico e host:porta; periodicamente confirma que está ativo; consumidores perguntam ao catálogo quais réplicas usar.” O papel descrito é tipicamente o de:

A) API Gateway como sinônimo exclusivo de Swagger  
B) Service Discovery / Eureka  
C) MapStruct  
D) Flyway  
E) Hibernate Envers  

**12.** Afirmar que “Eureka e Zuul são o mesmo componente com nomes diferentes” é:

A) Correto em toda a doutrina FCC  
B) Incorreto: Eureka localiza (discovery); Zuul atua como gateway de borda  
C) Correto somente se o payload for XML  
D) Correto porque ambos geram OpenAPI  
E) Correto porque ambos são mappers Entity↔DTO  

**13.** Configuração estática de URL de outro microsserviço, em topologia com autoscaling frequente, tende a ser problemática porque:

A) O JSON deixa de ser válido  
B) Endereços físicos mudam e o hardcode não acompanha o ciclo de vida das réplicas  
C) O REST proíbe nomes lógicos  
D) O Gateway deixa de existir por definição  
E) O MapStruct invalida o registry  

**14.** Qual opção melhor resume o cartão mental deste subtópico?

A) Eureka = entrada única; Zuul = heartbeat; Swagger = registry  
B) Discovery/Eureka = onde está a réplica saudável (registry + heartbeat); Gateway/Zuul = por onde o externo entra  
C) Discovery = banco único; Gateway = SOAP  
D) Eureka = OpenAPI; Zuul = Entity↔DTO  
E) Heartbeat obriga sticky session como definição de REST  

**15.** Misturar papéis é erro clássico. Qual linha está **correta**?

A) Swagger registra heartbeats; Eureka documenta try-it  
B) Eureka é fachada pública obrigatória; Zuul só lista instâncias no registry  
C) Eureka (discovery) localiza; Zuul (gateway) entra/roteia  
D) MapStruct aplica eviction; Flyway autentica JWT na borda  
E) Envers substitui o service registry  

---

### Gabarito comentado (treino 1–15)

1. **B** — Localizar instâncias com endereço dinâmico; não é docs/mapper/migração.  
2. **B** — “Onde está a réplica saudável?” = pergunta do Discovery.  
3. **B** — Registry = catálogo nome → instâncias.  
4. **B** — Lookup em runtime vs hardcode.  
5. **B** — Eureka = face Discovery no edital.  
6. **B** — Server = registry; Client = registra/consulta.  
7. **B** — Pulso de vida + base para remoção.  
8. **B** — Tabela anti-confusão clássica.  
9. **B** — Fluxo Cliente → Gateway → Discovery → serviço.  
10. **C** — Entrada pública = Gateway, não Discovery.  
11. **B** — Registro + heartbeat + lookup = Discovery/Eureka.  
12. **B** — Papéis distintos (localiza × entra).  
13. **B** — Topologia elástica × endereço fixo.  
14. **B** — Síntese do arquivo 05.  
15. **C** — Duo Eureka/Zuul sem inverter.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** Em arquiteturas de microsserviços com instâncias efêmeras, o Service Discovery caracteriza-se, em regra, por:

A) permitir a localização dinâmica de instâncias por meio de um catálogo (*registry*), reduzindo a dependência de endereços físicos fixos  
B) constituir, como função exclusiva e sinônima, o ponto único de entrada com *rate limiting* perante o usuário final  
C) substituir integralmente a necessidade de contratos HTTP e de códigos de status  
D) atuar apenas como biblioteca de mapeamento entre entidades e DTOs em tempo de compilação  
E) eliminar qualquer possibilidade de coexistência com um API Gateway  

**17.** Acerca do Eureka no ecossistema Spring Cloud clássico cobrado em concursos, assinale a alternativa correta.

A) Eureka funciona primariamente como API Gateway de borda, sinônimo de Zuul  
B) Eureka atua como mecanismo de *service discovery*, com servidor de registro e clientes que se anunciam e consultam o catálogo  
C) Eureka é biblioteca de mapeamento *compile-time* entre POJOs  
D) Eureka substitui o Spring Boot como *runtime* embutido da aplicação  
E) Eureka é o padrão OpenAPI de documentação interativa  

**18.** No contraste entre configuração estática de endpoints e descoberta dinâmica, é correto afirmar que:

A) ambas respondem da mesma forma a autoscaling contínuo, sem diferença operacional relevante  
B) na descoberta dinâmica, os endereços tendem a ser obtidos do *registry* em tempo de execução, acompanhando o ciclo de vida das réplicas  
C) a configuração estática é a única estratégia admitida em microsserviços  
D) a descoberta dinâmica proíbe nomes lógicos de serviço  
E) o *registry* equivale, por definição, à documentação Swagger  

**19.** Considere o uso conjugado de **Zuul** e **Eureka** em stack alinhada ao conteúdo programático de Analista de TI de tribunal. Assinale a alternativa correta.

A) Zuul mantém o catálogo de instâncias; Eureka é a fachada pública de entrada  
B) Eureka localiza instâncias no *registry*; Zuul atua como gateway de borda, podendo apoiar-se no discovery para encaminhar o tráfego  
C) Ambos são exclusivamente geradores de scripts Flyway  
D) Eureka aplica *rate limiting* como definição do padrão de discovery  
E) Zuul remove instâncias por ausência de *heartbeat* como função exclusiva de registry  

**20.** Considere as afirmações abaixo.

I. O Service Discovery responde, em essência, à pergunta sobre a localização das instâncias disponíveis.  
II. O *heartbeat* contribui para que instâncias que deixaram de responder possam ser retiradas ou consideradas indisponíveis no catálogo.  
III. API Gateway e Service Discovery desempenham papéis distintos e tipicamente complementares.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**21.** Assinale a opção que **inverte** indevidamente os papéis típicos de Eureka e Zuul no ecossistema Spring Cloud clássico cobrado em concursos.

A) Eureka = discovery/registro; Zuul = gateway/roteamento de borda  
B) Eureka = filtro/entrada de borda HTTP; Zuul = *registry* de instâncias  
C) Cliente externo utiliza o gateway como fachada; o discovery informa endereços das réplicas  
D) Eureka mantém catálogo; Zuul encaminha tráfego na borda  
E) A associação de prova liga Eureka à localização e Zuul à entrada  

**22.** A remoção (ou indisponibilização) de uma réplica que deixou de enviar sinais periódicos de vida relaciona-se, no discurso de *service discovery*, à ideia de:

A) exclusividade do banco H2 em produção  
B) *heartbeat* / evicção de instâncias mortas no *registry*  
C) proibição de JSON em APIs REST  
D) função exclusiva do Hibernate Envers  
E) substituição do discovery pelo MapStruct  

**23.** Em enunciado que descreve componente responsável por “oferecer ponto estável de entrada ao consumidor externo, validar token na borda e encaminhar `/api/peticoes`”, sem enfatizar o catálogo de heartbeats, o papel mais adequado é o de:

A) Eureka Server como registry puro  
B) Service Discovery como sinônimo de Swagger  
C) API Gateway (ex.: Zuul)  
D) MapStruct  
E) Flyway  

**24.** Para fins de preparação no eixo Service Discovery / Eureka do edital (PDPJ-Br / Analista TI), a síntese mais adequada é:

A) Eureka e Zuul são o mesmo artefato; Discovery documenta OpenAPI; Gateway mapeia Entity↔DTO  
B) Discovery (Eureka) = catálogo dinâmico, registro, heartbeat e localização de réplicas; Gateway (Zuul) = entrada/roteamento — papéis distintos; estático ≠ lookup dinâmico  
C) Heartbeat só existe dentro do PostgreSQL  
D) O cliente externo deve, por definição, conhecer todos os IPs internos sem catálogo nem fachada  
E) Registry obriga ESB clássico e proíbe HTTP  

**25.** Assinale a afirmação **incompatível** com o papel típico do Service Discovery / Eureka.

A) Manter catálogo de instâncias associadas a nomes lógicos de serviço  
B) Permitir que consumidores obtenham host/porta atualizados em tempo de execução  
C) Apoiar-se em sinais de vida (*heartbeat*) para refletir saúde/disponibilidade no registry  
D) Servir como ponto único de entrada pública com políticas transversais de borda, confundindo-se com o API Gateway  
E) Contrastar com a estratégia de endereços fixos em arquivo de configuração  

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

**Notas rápidas:** 16 A (localização dinâmica via registry); 17 B (Eureka = discovery server/client); 18 B (lookup runtime); 19 B (Eureka localiza; Zuul entra); 20 E (I–III ok); 21 B (única que **inverte** Eureka/Zuul); 22 B (heartbeat/eviction); 23 C (borda = Gateway); 24 B (síntese do subtópico); 25 D (entrada/cross-cutting ≠ Discovery).

---

*Arquivo 05 · Service Discovery / Eureka · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 06 Zuul Gateway (Spring).*

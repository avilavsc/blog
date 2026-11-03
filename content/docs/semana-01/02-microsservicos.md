+++
title = '02 — Microsserviços'
date = '2026-10-02'
draft = false
weight = 2
tags = ['semana-01', 'pdpj', 'microsservicos']
categories = ['estudo']
+++
# Microsserviços · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Microsserviços  
**Escopo deste arquivo:** definição, contraste monólito × SOA × microsserviços, deploy/escala independentes, comunicação, ideia de dados por serviço, benefícios/riscos e o que a FCC testa. Contexto leve da PDPJ-Br.  
**Fora de escopo aqui:** Eureka, Zuul, Swagger, MapStruct e detalhes profundos de Spring Boot/Cloud (arquivos 05–09). REST/JSON aprofundado = arquivo 03; API Gateway = arquivo 04.

---

## 1. Material

### 1.1 O que são microsserviços

**Microsserviços** são um estilo de arquitetura em que a aplicação é decomposta em **serviços pequenos**, cada um com **responsabilidade de negócio bem delimitada**, implantável e escalável **de forma independente**, e que se comunicam tipicamente por **APIs leves** (em prova: sobretudo **HTTP REST + JSON**).

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Granularidade | Serviço pequeno, foco em um domínio/capacidade |
| Independência | Deploy e escala **por serviço**, não “tudo ou nada” |
| Integração | Comunicação por contratos/APIs (não por um único processo monolítico) |

Em prova, “microsserviço” **não** significa “qualquer classe Java pequena” nem “qualquer container Docker”. O ponto é **unidade de deploy + limite de negócio + autonomia relativa**.

### 1.2 Contexto leve PDPJ-Br (sem aprofundar a stack)

Na PDPJ-Br, microsserviços aparecem como **modelo técnico** do ecossistema: capacidades digitais que podem ser **expostas, reutilizadas e evoluídas** com certa independência, alinhadas a padrões nacionais. A plataforma **não** é um monólito único nacional; o discurso de prova combina **plataforma/ecossistema** com **serviços** que interoperam.

Flash: **PDPJ = plataforma/ecossistema**; **microsserviços = estilo arquitetural** típico desse desenho. Não confundir a definição institucional da PDPJ com o detalhe de uma ferramenta (Eureka, Zuul etc.).

### 1.3 Microsserviços × monólito

| Aspecto | **Monólito** | **Microsserviços** |
|---------|--------------|---------------------|
| Empacotamento | Um artefato / um processo dominante | Vários serviços implantáveis |
| Deploy | Em geral **único** (muda uma parte → sobe o todo) | **Independente** por serviço |
| Escala | Escala o **conjunto** (mesmo o que não precisa) | Escala **só** o serviço sob carga |
| Falha | Risco de “tudo ou nada” no mesmo processo | Isolamento **relativo** (um serviço pode falhar sem derrubar todos) |
| Evolução | Times acoplados ao mesmo ciclo de release | Times podem evoluir serviços em ritmos distintos |
| Complexidade | Mais simples no início; cresce acoplamento interno | Mais partes móveis; complexidade **distribuída** |

Erro clássico: achar que monólito = “sempre ruim” e microsserviços = “sempre melhor”. Em concurso, cobre-se o **contraste de características**, não julgamento absoluto. Monólito pode ser adequado a escopo pequeno; microsserviços brilham quando se quer **independência de deploy/escala** e limites de domínio claros — à custa de complexidade operacional.

### 1.4 Microsserviços × SOA clássica

A FCC gosta do triângulo **monólito × SOA × microsserviços**.

| | **SOA clássica (visão de prova)** | **Microsserviços (visão de prova)** |
|---|-----------------------------------|--------------------------------------|
| Integração | Frequentemente **ESB** / middleware pesado, orquestração central | **APIs leves** (REST/JSON), menos “barramento único” obrigatório |
| Contratos | Ênfase em contratos formais, às vezes SOAP/WSDL no imaginário de prova | Contratos de API HTTP; JSON como formato típico no edital PDPJ |
| Acoplamento | Risco de acoplamento via **ESB central** | Serviços mais autônomos; integração ponto a ponto ou via gateway (detalhe no arq. 04) |
| Deploy | Serviços podem existir, mas o desenho clássico cobrado enfatiza **infra de integração pesada** | Ênfase em **deploy independente** e discovery dinâmico (detalhe no arq. 05) |

Síntese cobrável:

- **Monólito** → um deploy.  
- **SOA clássica** → serviços + integração frequentemente **pesada** (ESB/contratos pesados).  
- **Microsserviços** → serviços pequenos + **APIs leves** + independência de deploy/escala (+ discovery no ecossistema Spring Cloud do edital).

Não é necessário virar especialista em história da SOA: basta saber o **contraste que a banca espera**.

### 1.5 Independência de deploy e de escala

Dois benefícios mais cobrados:

1. **Deploy independente** — alterar e publicar o serviço de “consulta processual” sem republicar o de “autenticação”, desde que o contrato seja respeitado.  
2. **Escala independente** — se só o serviço de busca satura, sobem **réplicas desse serviço**, não necessariamente de todos os outros.

Consequência mental: o monólito escala “o pacote inteiro”; microsserviços permitem **investimento de capacidade onde dói**.

Limite de prova: independência **não** é anarquia. Ainda há **contratos**, versionamento, monitoramento e coordenação. Independência de *deploy* ≠ ausência de integração.

### 1.6 Comunicação entre serviços

No edital PDPJ / FCC deste eixo, a comunicação típica cobrada é:

- **Síncrona:** HTTP **REST** com corpo **JSON** (detalhes de verbos/status no arquivo 03).  
- Ideia geral: serviços **expostos como APIs**; cliente (outro serviço ou gateway) consome o contrato.

Pontos de prova sem aprofundar gateway/discovery:

- Comunicação **não** costuma ser “compartilhar a mesma JVM / mesmo processo” como regra.  
- **Não** é obrigatório SOAP em microsserviços do discurso PDPJ (JSON/REST é o eixo).  
- Mensageria/eventos podem existir no ecossistema maior do edital, mas **neste subtópico** priorize REST+JSON como padrão de comunicação cobrável entre serviços.

Armadilha: dizer que microsserviços “proíbem” qualquer comunicação síncrona, ou que “só existem se houver ESB”.

### 1.7 Dados por serviço (persistência)

Ideia central: em arquitetura de microsserviços, o desenho **típico** é **cada serviço dono do seu modelo de dados / persistência**, evitando um **único banco compartilhado** que todos escrevem à vontade (isso recria acoplamento de monólito no nível de dados).

| Abordagem | Leitura em prova |
|-----------|------------------|
| **BD por serviço** (ou schema fortemente isolado por serviço) | Alinhada a microsserviços |
| **Um BD único** compartilhado por todos os serviços com tabelas cruzadas à vontade | **Incompatível** com a ideia típica; cheira a monólito disfarçado |
| Integração de dados | Via **API/eventos**, não via `JOIN` direto no banco do vizinho |

Nuance: “dados por serviço” é **princípio/ideia** cobrável, não dogma absoluto de infraestrutura. Em prova, se a alternativa disser “todos os microsserviços devem obrigatoriamente compartilhar o mesmo banco físico único”, desconfie.

### 1.8 Benefícios (o que costuma cair como “vantagem”)

1. **Deploy independente** de partes do sistema.  
2. **Escala seletiva** sob carga.  
3. **Isolamento relativo de falhas** (um serviço degradado não implica, automaticamente, queda de todo o processo único).  
4. **Evolução por domínio** — times podem trabalhar em serviços distintos com ciclos menos acoplados.  
5. **Alinhamento a plataforma/ecossistema** (PDPJ): capacidades reutilizáveis e integráveis por API.  
6. Possibilidade de **tecnologias distintas por serviço** (poliglota) — cobrada como *possibilidade*, não obrigação.

### 1.9 Riscos e trade-offs (o que costuma cair como “desvantagem / cuidado”)

1. **Complexidade distribuída:** rede, latência, timeouts, retries, consistência eventual.  
2. **Operação mais pesada:** observabilidade, deploy de muitos artefatos, versionamento de APIs.  
3. **Testes de integração** mais difíceis que no monólito inicial.  
4. **Governança de contratos:** breaking changes quebram consumidores.  
5. **Não é “grátis”:** para sistemas pequenos, o custo operacional pode superar o benefício.  
6. Risco de **“monólito distribuído”**: muitos serviços, mas com BD único compartilhado e acoplamento forte — pior dos mundos.

Flash de prova: microsserviços **trocam** simplicidade de um processo por **flexibilidade de deploy/escala** e pagam com **complexidade operacional**.

### 1.10 O que a FCC tipicamente testa neste tema

- Definição: serviços **pequenos**, limites de negócio, **deploy/escala independentes**, comunicação por **APIs leves**.  
- Contraste: **monólito** (um deploy) × **SOA** (ESB/contratos pesados) × **microsserviços** (APIs leves + autonomia).  
- Persistência: ideia de **dados por serviço** × rejeição do “BD único compartilhado por todos” como regra.  
- Benefícios vs riscos: independência e escala seletiva × complexidade distribuída.  
- Ligação leve à PDPJ: ecossistema de serviços, **não** monólito nacional.  
- Armadilhas: confundir microsserviço com ferramenta pontual (Eureka/Zuul/Swagger/MapStruct); achar que microsserviços exigem um único banco; achar que monólito e microsserviços são sinônimos.

Para este arquivo, memorize o **conceito arquitetural**. Gateway, discovery e frameworks entram nos próximos arquivos como **meios**, não como definição de microsserviço.

### 1.11 Mini-síntese (cartão mental)

> **Microsserviços** = serviços pequenos e autônomos → **deploy e escala independentes** → falam por **APIs leves (REST/JSON)** → em geral **dados por serviço** → vantagens de flexibilidade × custo de sistema distribuído. Na PDPJ: encaixa no ecossistema de capacidades integráveis, **não** no monólito único.

---

## 2. Questões de treino (1–15)

**1.** Em arquitetura de microsserviços, a característica mais distintiva em relação a um monólito clássico é:

A) Obrigatoriedade de um único artefato de deploy para toda a aplicação  
B) Serviços com limites de responsabilidade e possibilidade de deploy/escala independentes  
C) Proibição absoluta de qualquer API HTTP  
D) Uso obrigatório de um ESB central como único meio de comunicação  
E) Compartilhamento obrigatório do mesmo banco físico por todos os módulos  

**2.** No contraste cobrável monólito × microsserviços, o monólito caracteriza-se principalmente por:

A) Vários deploys independentes por domínio  
B) Escala seletiva de apenas um módulo sem republicar o restante, como regra típica  
C) Empacotamento dominante em um artefato/processo, com ciclo de release mais acoplado  
D) Persistência necessariamente isolada por serviço desde o primeiro dia  
E) Comunicação exclusiva por mensageria assíncrona  

**3.** Em relação à SOA clássica, no nível usualmente explorado em provas FCC ligadas à PDPJ, microsserviços enfatizam:

A) Integração tipicamente via ESB pesado e contratos SOAP como regra exclusiva  
B) APIs leves (ex.: REST/JSON) e maior autonomia de deploy dos serviços  
C) Fusão obrigatória de todos os serviços em um único WAR  
D) Abolição de qualquer contrato entre partes  
E) Substituição de serviços por planilhas compartilhadas  

**4.** “Escala independente” em microsserviços significa, em essência:

A) Sempre escalar todos os serviços na mesma proporção  
B) Poder aumentar capacidade do serviço sob pressão sem necessariamente escalar os demais  
C) Escalar apenas o banco único compartilhado, nunca as instâncias de serviço  
D) Proibir réplicas de qualquer serviço  
E) Escalar somente em horário comercial por norma de prova  

**5.** Assinale a afirmação **incorreta** sobre microsserviços:

A) Favorecem deploy independente de partes do sistema  
B) Tipicamente comunicam-se por APIs  
C) Exigem, como regra tipicamente cobrada, um único banco compartilhado escrito por todos os serviços  
D) Introduzem complexidade de sistema distribuído  
E) Permitem isolamento relativo de falhas entre serviços  

**6.** A ideia de “dados por serviço” aponta para:

A) Um SGBD nacional obrigatório para todos os microsserviços do Judiciário  
B) Cada serviço como dono de seu modelo/persistência, evitando acoplamento via BD compartilhado indiscriminado  
C) JOINs diretos obrigatórios entre tabelas de serviços distintos  
D) Ausência total de persistência em microsserviços  
E) Armazenamento exclusivo em cookies do navegador  

**7.** Qual benefício é mais coerente com o discurso de microsserviços?

A) Eliminação completa de qualquer necessidade de monitoramento  
B) Deploy e evolução de um serviço sem republicar obrigatoriamente todos os outros  
C) Garantia matemática de zero latência de rede  
D) Extinção automática de testes  
E) Proibição de versionamento de APIs  

**8.** Um risco típico da adoção de microsserviços é:

A) Complexidade operacional e de integração em ambiente distribuído  
B) Impossibilidade conceitual de usar JSON  
C) Obrigação de abandonar HTTP  
D) Transformação automática de todo monólito em ESB  
E) Fim da necessidade de contratos entre serviços  

**9.** No contexto leve da PDPJ-Br, microsserviços aparecem como:

A) Sinônimo exato de “site de notícias do CNJ”  
B) Estilo arquitetural alinhado a um ecossistema de serviços/capacidades integráveis, em oposição à leitura de monólito único nacional  
C) Proibição de interoperabilidade entre tribunais  
D) Substituição do CNJ por um único microsserviço  
E) Apenas o nome comercial do MapStruct  

**10.** Comunicação tipicamente associada a microsserviços no eixo PDPJ/edital deste pack é:

A) Troca exclusiva de mídias USB entre datacenters  
B) HTTP REST com JSON como formato usual de carga  
C) Apenas chamadas JDBC diretas ao banco do serviço vizinho como padrão de integração  
D) Obrigação de SOAP/WSDL em toda chamada  
E) Comunicação apenas por memória compartilhada no mesmo processo  

**11.** “Monólito distribuído” descreve bem o anti-padrão em que:

A) Há serviços nomeados como microsserviços, porém com forte acoplamento (ex.: BD único compartilhado e deploys ainda acoplados na prática)  
B) Cada serviço tem deploy e dados verdadeiramente independentes  
C) Não existe rede entre componentes  
D) Só há um único serviço bem delimitado  
E) Aplica-se exclusivamente a mainframes sem rede  

**12.** Deploy independente pressupõe, de forma realista:

A) Que contratos entre serviços possam ser ignorados sem consequência  
B) Que um serviço possa ser publicado isoladamente, desde que respeite os contratos com consumidores/provedores  
C) Que todos os serviços sejam republicados a cada vírgula alterada em qualquer um deles, por definição  
D) Que não existam ambientes de homologação  
E) Que DNS seja proibido  

**13.** Comparando monólito e microsserviços quanto a falhas:

A) No monólito, falha em um módulo do mesmo processo pode afetar o conjunto; em microsserviços há isolamento relativo entre processos/serviços  
B) Microsserviços tornam falhas de rede impossíveis  
C) Monólito nunca falha  
D) Em microsserviços, a falha de um serviço derruba obrigatoriamente todos os demais por definição matemática  
E) Isolamento de falha só existe em SOA com ESB e nunca em microsserviços  

**14.** Qual alternativa melhor resume o triângulo monólito × SOA clássica × microsserviços para revisão rápida?

A) Monólito: um deploy · SOA clássica: integração frequentemente pesada (ESB/contratos) · Microsserviços: serviços pequenos + APIs leves + autonomia de deploy/escala  
B) Os três termos são sinônimos rigorosos  
C) Microsserviços = apenas ESB; monólito = apenas REST  
D) SOA clássica = dados por serviço; microsserviços = BD único obrigatório  
E) Monólito exige discovery; microsserviços proíbem APIs  

**15.** O que a FCC mais provavelmente testa neste subtópico (02), e não nos arquivos de ferramentas?

A) Sintaxe linha a linha de anotações MapStruct como definição de microsserviço  
B) Conceito, contrastes (monólito/SOA), deploy/escala, comunicação por API, dados por serviço, benefícios/riscos  
C) Configuração detalhada de Eureka Server como único conteúdo possível  
D) Comandos exclusivos do Rancher sem conceito arquitetural  
E) Que microsserviço = apenas Swagger UI  

---

### Gabarito comentado (treino 1–15)

1. **B** — Limites + deploy/escala independentes; A/E são monólito/acoplamento; C/D negam o modelo típico.  
2. **C** — Artefato/processo dominante e release acoplado.  
3. **B** — APIs leves e autonomia; A descreve SOA clássica de prova.  
4. **B** — Escala seletiva do serviço sob carga.  
5. **C** — BD único compartilhado por todos **não** é a regra tipicamente cobrada.  
6. **B** — Ownership de dados por serviço.  
7. **B** — Deploy/evolução independente.  
8. **A** — Trade-off clássico: complexidade distribuída/operacional.  
9. **B** — Estilo no ecossistema PDPJ; não monólito nacional.  
10. **B** — REST + JSON no eixo do edital.  
11. **A** — Serviços “no nome”, acoplamento de monólito na prática.  
12. **B** — Independência com respeito a contratos.  
13. **A** — Isolamento relativo vs processo único.  
14. **A** — Cartão mental do triângulo.  
15. **B** — Foco conceitual deste arquivo; ferramentas vêm depois.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60)

Ao usar o Gateway API com o Service Discovery no contexto da PDPJ-Br, a principal consequência para o frontend em uma aplicação baseada em microsserviços é que o

A) Gateway API diminui a necessidade de autenticação, uma vez que o Service Discovery é integrado ao SSO.

B) frontend deve configurar manualmente os IPs e portas dos serviços disponíveis na plataforma.

C) frontend se torna o responsável pela chamada direta de cada microsserviço individualmente.

D) Gateway API reduz o número de chamadas externas que o frontend precisa fazer, tornando a comunicação mais eficiente.

E) frontend deve gerenciar manualmente as atualizações de endpoint de cada serviço individual registrado.


**17.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22)

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.

B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.

C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.

D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.

E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.


**18.** (FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Em um cenário de sistema de microsserviços um técnico deseja expor alguns dos microsserviços para acesso externo e ocultar outros. Como ele está usando o Spring Cloud, para proteger os microsserviços expostos contra solicitações de clientes mal-intencionados, ele pode usar o

A) Galaxy Eureka como edge server.

B) Apache Toncat como service discovery.

C) Apache Wildfly como security server.

D) Netflix Zuul como edge server.

E) Swagger como security server.


**19.** (FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60)

O Spring Eureka é uma implementação do padrão de registro de serviços que permite que os microsserviços se registrem automaticamente e se descubram uns aos outros. A porta padrão em que o servidor Eureka irá executar na aplicação Spring Boot e o arquivo em que essa porta é configurada são, respectivamente,

A) `80` e `eureka-server.xml`

B) `15000` e `application-server.yml`

C) `8080` e `pom.xml`

D) `8005` e `eureka-registry.xml`

E) `8761` e `application.properties`


**20.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60)

Na classe principal de uma aplicação criada com Spring Boot, para fazer com que a aplicação atue como um cliente Eureka utiliza-se a anotação

A) `@EurekaClient`

B) `@EnableEurekaClient`

C) `@EnableDiscoveryEurekaClient`

D) `@EurekaDiscoveryClient`

E) `@IsEurekaClient`


**21.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60)

Correspondem, correta e respectivamente, a um componente do Spring Cloud e à principal função do Spring Cloud Config:

A) Spring Security — implementar um padrão de design para a comunicação entre serviços baseados em REST.

B) Spring Boot Actuator — gerenciar o ciclo de vida e o monitoramento de aplicações distribuídas em uma arquitetura de microsserviços.

C) Spring Data JPA — oferecer uma solução para o balanceamento de carga entre instâncias de microsserviços.

D) Spring Cloud Stream — centralizar a configuração de aplicações e serviços, permitindo que configurações sejam atualizadas dinamicamente.

E) Spring Web MVC — fornecer um serviço de autenticação e autorização para microsserviços e aplicações.


**22.** (FCC | TRT20 2024 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 59 de 60)

A Resolução CNJ nº 522/2023 em seu Art. 2º estabelece que os sistemas informatizados de gestão de processos e documentos utilizados em atividades judiciais e administrativas dos órgãos integrantes do Poder Judiciário, ainda que segmentados em aplicações, módulos e microsserviços, deverão aderir aos requisitos do MoReq-Jus, com o objetivo de

A) acompanhar o fiel cumprimento desta Resolução e a coordenação do Programa de avaliação do grau de aderência dos sistemas ao MoReq-Jus e de atualização permanente (Programa MoReq-Aval).

B) avaliar o grau de aderência dos requisitos ao MoReq-Jus e de sua atualização permanente especificada no Programa MoReq-Aval/2023.

C) assegurar a concordância, a segurança, a especificidade, o grau de complexidade, a integridade, a razoabilidade e a não duplicidade de processos e documentos do Poder Judiciário.

D) identificar os requisitos funcionais, dados e metadados que serão tratados por serviços interligados ao sistema principal e as referências ao requisito funcional implementado e aos requisitos não funcionais observados por ocasião da especificação de novas funcionalidades ou de melhorias daquelas existentes.

E) assegurar a confidencialidade, a integridade, a disponibilidade, a autenticidade, o não repúdio, a conformidade e a preservação de processos e documentos do Poder Judiciário.


**23.** (FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60)

Um Técnico está participando de um projeto de microsserviços distribuídos onde centenas de contêineres são utilizados. Para fazer a orquestração destes contêineres, ou seja, a coordenação e o monitoramento, ele precisa utilizar um recurso que possa permitir que a atualização da aplicação em um contêiner não interrompa o serviço. O recurso sugerido corretamente por ele, nesse caso, foi o

A) Qubes.

B) VirtualBox.

C) VMWare.

D) Docker, em modo padrão.

E) Kubernetes.


**24.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60) *[aproximação temática]*

*SSO no ecossistema de microsserviços PDPJ.*

A autenticação na Plataforma Digital do Poder Judiciário PDPJ-Br ocorre por meio de uma solução A, baseada em um aplicativo open source de gestão de identidades e acesso, denominado B, com base no protocolo C. De acordo com o texto acima, A, B e C são, correta e respectivamente:

A) Keycloak – Docker – Simple Object Access Protocol (SOAP)

B) Single Sign-On (SSO) – Keycloak – OAuth2 (RFC 6749)

C) Single Sign-On (SSO) – RabbitMQ – Advanced Message Queuing Protocol (AMQP)

D) de access token – Keycloak – Simple Object Access Protocol (SOAP)

E) Keycloak – RabbitMQ – OAuth2 (RFC 4967)


**25.** (FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 40 de 60) *[aproximação temática]*

*CI/CD em contexto Eureka/Zuul.*

Integração e implantação contínuas (CI/CD) é uma coleção de práticas usadas para integrar e entregar código rapidamente. Usando a prática de

A) integração contínua (CI), em um cronograma pré-acordado, provavelmente no final do ciclo de sprint, o pacote de código é liberado e testado manualmente pelo time de desenvolvimento.

B) implantação contínua (CD), depois que todo o código é validado com sucesso pelo time de desenvolvimento e qualidade, implanta-se o código manualmente no ambiente de teste ou de produção.

C) implantação contínua (CD) em que o grupo controle de qualidade realiza testes manuais para todos os códigos novos ou modificados. Se for bem-sucedido, o código será liberado para implantação; caso contrário, o lançamento é rejeitado e colocado em espera até que os bugs sejam corrigidos.

D) integração contínua (CI), os desenvolvedores são incentivados a fazer pequenos ajustes de código, mesclar seu código na ramificação de código principal e validá-lo usando testes automatizados.

E) integração contínua (CI), para implementar pipelines devem ser utilizadas ferramentas como Zuul e Eureka.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **D** | FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60 |
| 17 | **E** | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22 |
| 18 | **D** | FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 19 | **E** | FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60 |
| 20 | **B** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60 |
| 21 | **D** | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 22 | **E** | FCC | TRT20 2024 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 59 de 60 |
| 23 | **E** | FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60 |
| 24 | **B** \* | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60 |
| 25 | **D** \* | FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 40 de 60 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q39
- Q17: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22
- Q18: https://www.quizinho.com.br/questao/fcc-trt4-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q38
- Q19: https://www.quizinho.com.br/questao/fcc-trt15-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q31
- Q20: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q32
- Q21: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q53
- Q22: https://www.quizinho.com.br/questao/fcc-trt20-2024-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q59
- Q23: https://www.quizinho.com.br/questao/fcc-trt4-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q31
- Q24: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q28
- Q25: https://www.quizinho.com.br/questao/fcc-trt4-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q40

*Arquivo 02 · Microsserviços · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 03 REST / JSON.*

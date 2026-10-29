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

## 3. Questões estilo concurso FCC (16–25)

**16.** Em concursos da área de Tecnologia da Informação do Poder Judiciário, a arquitetura de microsserviços é corretamente associada à ideia de:

A) um único artefato monolítico cuja republicação é obrigatória a cada alteração pontual de qualquer módulo interno  
B) serviços de responsabilidade delimitada, com possibilidade de implantação e escalonamento independentes, integrados por interfaces leves  
C) extinção de quaisquer interfaces entre componentes de software  
D) uso exclusivo de barramento ESB como único padrão permitido de comunicação, com vedação a APIs HTTP  
E) compartilhamento indiscriminado e obrigatório de um único banco de dados físico por todos os serviços, como regra definidora do estilo  

**17.** Constitui diferença cobrável entre o monólito clássico e o estilo de microsserviços:

A) no monólito, a escala pode ser aplicada de forma seletiva a um único domínio sem afetar o empacotamento dominante, como regra típica; nos microsserviços, a escala é sempre do artefato único  
B) nos microsserviços, costuma-se enfatizar autonomia de deploy por serviço; no monólito, o ciclo de entrega tende a acoplar o conjunto em um artefato dominante  
C) ambos inviabilizam, por definição, o uso de JSON  
D) o monólito exige obrigatoriamente service discovery; o microsserviço proíbe HTTP  
E) não há qualquer diferença conceitual entre os dois modelos em provas de TI  

**18.** No contraste entre SOA clássica e microsserviços, tal como usualmente explorado em enunciados alinhados à PDPJ-Br, é correto afirmar que:

A) microsserviços, no discurso típico de prova, enfatizam APIs leves e maior autonomia dos serviços, ao passo que a SOA clássica é frequentemente associada a integração mais pesada (por exemplo, via ESB/contratos mais formais)  
B) microsserviços e SOA clássica são termos tecnicamente idênticos e intercambiáveis em qualquer alternativa  
C) a SOA clássica dispensa qualquer forma de serviço, reduzindo-se a um monólito único  
D) microsserviços exigem, por definição, a presença obrigatória de um ESB central  
E) a PDPJ-Br rejeita qualquer forma de serviço em favor exclusivo de aplicações desktop isoladas  

**19.** A noção de persistência “por serviço”, no contexto de microsserviços, implica principalmente:

A) a obrigatoriedade de um único esquema SQL nacional compartilhado por todas as justiças, com escrita cruzada irrestrita  
B) que cada serviço detenha autonomia sobre seu modelo de dados, evitando o acoplamento típico de um banco único indiscriminadamente compartilhado  
C) a proibição de qualquer armazenamento persistente  
D) a substituição de bancos de dados por arquivos de texto enviados exclusivamente por correio eletrônico  
E) a necessidade de que todos os serviços efetuem JOIN direto nas tabelas uns dos outros como padrão de integração  

**20.** Considere as afirmativas abaixo.

I. Microsserviços favorecem deploy e escala com maior independência entre partes do sistema.  
II. A adoção de microsserviços elimina por completo a complexidade operacional, tornando desnecessários monitoramento e gestão de contratos.  
III. A comunicação entre microsserviços, no eixo técnico associado à PDPJ no edital, é tipicamente pensada por meio de APIs leves, com REST e JSON como referências usuais.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e III, apenas  
D) II e III, apenas  
E) I, II e III  

**21.** Assinale a opção que apresenta característica **incompatível** com o estilo de microsserviços usualmente cobrado.

A) Limites de responsabilidade por serviço  
B) Possibilidade de escalonar seletivamente um serviço sob carga  
C) Definição do estilo pela obrigatoriedade de um único banco compartilhado por todos os serviços com forte acoplamento de escrita  
D) Integração por contratos de API  
E) Isolamento relativo de falhas entre serviços distintos  

**22.** Acerca dos trade-offs da arquitetura de microsserviços, é correto concluir que:

A) há ganho potencial de flexibilidade de deploy e escala, acompanhado de maior complexidade própria de sistemas distribuídos  
B) microsserviços garantem latência zero e ausência total de falhas de rede  
C) o modelo dispensa qualquer preocupação com versionamento de interfaces  
D) benefícios existem apenas se for adotado um monólito simultaneamente idêntico  
E) riscos operacionais são conceitualmente impossíveis nesse estilo  

**23.** Em enunciado que relacione a PDPJ-Br a microsserviços, a leitura mais adequada é:

A) a PDPJ-Br equivale a um monólito nacional único, sendo microsserviços uma tese incompatível com a plataforma  
B) microsserviços constituem estilo arquitetural coerente com um ecossistema de capacidades/serviços integráveis, em contraste com a ideia de sistema monolítico único  
C) microsserviços significam apenas a publicação de notícias em sítio eletrônico do CNJ  
D) a existência de microsserviços extingue a necessidade de padrões nacionais de integração  
E) PDPJ-Br e microsserviços são sinônimos estritos de MapStruct  

**24.** Um analista afirma que “migrar para microsserviços consiste apenas em dividir o código em pacotes Java dentro do mesmo artefato de deploy único, mantendo um banco compartilhado com escrita cruzada irrestrita e o mesmo ciclo de release para tudo”. Tal afirmação:

A) descreve corretamente e de forma suficiente o estilo de microsserviços  
B) está correta apenas se o artefato único for nomeado de “microsserviço”  
C) está incorreta, pois microsserviços pressupõem autonomia relevante de deploy/escala e, em regra, evitam o acoplamento típico de persistência compartilhada indiscriminada  
D) está correta, pois monólito e microsserviços coincidem na doutrina de prova  
E) está incorreta somente porque faltou citar a obrigatoriedade de ESB  

**25.** Para fins de preparação em Analista Judiciário – Área de Tecnologia da Informação, a síntese mais adequada de microsserviços é:

A) um deploy único + BD compartilhado obrigatório + ESB como única integração + ausência de APIs  
B) serviços pequenos e autônomos, com deploy/escala independentes, comunicação típica por APIs leves (REST/JSON), tendência a dados por serviço, benefícios de flexibilidade e custos de complexidade distribuída  
C) apenas uma ferramenta de documentação de API  
D) apenas o mecanismo de descoberta de instâncias  
E) política exclusiva de proibir JSON no Judiciário  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **B** |
| 17 | **B** |
| 18 | **A** |
| 19 | **B** |
| 20 | **C** |
| 21 | **C** |
| 22 | **A** |
| 23 | **B** |
| 24 | **C** |
| 25 | **B** |

**Notas rápidas:** 16 B (limites + deploy/escala + APIs leves); 17 B (autonomia de deploy × artefato dominante); 18 A (APIs leves × SOA/ESB pesada); 19 B (dados por serviço); 20 C (I e III; II nega o trade-off); 21 C (BD único compartilhado incompatível com a ideia típica); 22 A (flexibilidade × complexidade distribuída); 23 B (estilo no ecossistema, não monólito); 24 C (dividir pacotes no mesmo artefato ≠ microsserviços); 25 B (síntese completa do subtópico).

---

*Arquivo 02 · Microsserviços · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 03 REST / JSON.*

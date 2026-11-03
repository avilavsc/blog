+++
title = '01 — Visão PDPJ-Br'
date = '2026-10-01'
draft = false
weight = 1
tags = ['semana-01', 'pdpj']
categories = ['estudo']
+++
# Visão PDPJ-Br · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Visão da Plataforma Digital do Poder Judiciário  
**Escopo deste arquivo:** o que é a PDPJ-Br, finalidade, ecossistema nacional, interoperabilidade, ideia de governança, relação com os tribunais e o que ela **não** é.  
**Fora de escopo aqui:** Eureka, Zuul, Swagger, MapStruct, detalhes de Spring Boot/Cloud e microsserviços profundos (arquivos 02–09).

---

## 1. Material

### 1.1 O que é a PDPJ-Br

**PDPJ-Br** significa **Plataforma Digital do Poder Judiciário Brasileiro**. Em prova, trate-a como um **ecossistema nacional de integração digital** entre órgãos do Judiciário — não como “um software único” instalado em todos os tribunais.

A ideia central cobrada pela FCC é: o CNJ articula uma **plataforma comum** para que tribunais compartilhem **serviços, padrões e capacidades digitais**, reduzindo a lógica de cada tribunal reinventar o mesmo módulo isoladamente. Em outras palavras: **plataforma compartilhada + autonomia operacional dos tribunais**, sob diretrizes nacionais.

Três âncoras mentais úteis:

| Âncora | Frase cobrável |
|--------|----------------|
| Natureza | Ecossistema / plataforma de integração |
| Vetor técnico | Serviços, APIs e padrões comuns |
| Vetor institucional | Diretrizes e governança do CNJ sobre o digital do PJ |

### 1.2 Para que serve (finalidade)

A finalidade típica em enunciados FCC:

1. **Interoperar** sistemas e serviços entre tribunais e entre justiça e outros atores (quando o contexto exigir).
2. **Reutilizar** capacidades digitais (evitar silos absolutos e reescritas paralelas do mesmo problema).
3. **Padronizar** interfaces e práticas técnicas mínimas para troca de dados e consumo de serviços.
4. **Modernizar** a entrega de serviços judiciais digitais sob uma visão nacional coerente.
5. **Governar** o ecossistema digital do Poder Judiciário com regras e diretrizes comuns (sem transformar o CNJ em “dono do código de cada TRT”).

Não é necessário decorar discurso institucional longo: basta saber **integração + reuso + padronização + governança nacional**.

### 1.3 Ecossistema nacional (não monólito)

Contraste que a banca gosta de explorar:

| Modelo | Ideia | Encaixe na PDPJ? |
|--------|-------|------------------|
| **Monólito único nacional** | Um só sistema gigante substitui todos os sistemas judiciais | **Não** — leitura errada típica |
| **Silos absolutos** | Cada tribunal 100% isolado, sem padrão compartilhado | **Não** — oposto do discurso da plataforma |
| **Plataforma / ecossistema** | Capacidades comuns + integração + padrões; tribunais continuam existindo e operando | **Sim** — leitura correta |

A PDPJ-Br é cobrada como **plataforma**: componentes e serviços que se conectam, podem ser reaproveitados e evoluem com alguma independência. O Judiciário brasileiro é federado na prática (CNJ + tribunais), então a arquitetura mental da prova acompanha isso: **comum o que deve ser comum; local o que é de cada tribunal**.

### 1.4 Interoperabilidade (conceito-chave)

**Interoperabilidade** = capacidade de sistemas distintos **trocarem informação e cooperarem** de forma previsível, com contratos/padrões mínimos.

Na PDPJ, isso aparece como:

- tribunais **consumindo e expondo** serviços digitais alinhados a padrões nacionais;
- redução de integrações “ad hoc” sem contrato;
- foco em **APIs e serviços** como língua comum (detalhe REST/JSON fica no arquivo 03);
- visão de **ecossistema**, não de pasta compartilhada ou banco único nacional.

Erro clássico de prova: confundir interoperabilidade com “todos usam o mesmo monólito” ou “todos compartilham o mesmo banco de dados físico”. Interoperar **não exige** fundir sistemas em um só produto.

### 1.5 Ideia de governança

Governança, no nível cobrado neste subtópico, não é organograma completo nem texto de resolução artigo a artigo (isso é o arquivo de normativos). É a ideia de que:

- o **CNJ** define **diretrizes, padrões e marcos** do ecossistema digital;
- os **tribunais** aderem, implementam, operam e adaptam no seu contexto;
- há **evolução normativa** ao longo do tempo (base → atualizações), sem precisar listar números aqui;
- governança visa **coerência nacional** sem anular a realidade institucional dos tribunais.

Flash: **CNJ governa o ecossistema; tribunal opera e integra**.

### 1.6 Relação com os tribunais

Leitura cobrável:

- A PDPJ **não apaga** os sistemas e competências dos tribunais.
- Tribunais **participam** do ecossistema: consomem serviços comuns, disponibilizam capacidades, seguem padrões.
- Um TRT (como o TRT4) não “vira PDPJ”; ele **se relaciona** com a plataforma e com o padrão nacional de integração.
- Benefício esperado em prova: **menos isolamento**, mais capacidade de integração e reuso, sob regras comuns.

Analogia útil (não literal de edital): a PDPJ é mais “rede + padrões + serviços compartilháveis” do que “substituto único do PJe de cada tribunal”.

### 1.7 O que a PDPJ-Br **não** é

Lista de negações que costumam virar alternativa correta ou distrator:

1. **Não** é um monólito único que substitui todos os sistemas judiciais do país.
2. **Não** é apenas um site institucional do CNJ.
3. **Não** é sinônimo de “qualquer sistema de processo eletrônico local” (ex.: tratar PJe local como se fosse a PDPJ inteira).
4. **Não** é apenas uma biblioteca Java ou um framework Spring.
5. **Não** se reduz a uma ferramenta isolada (gateway, discovery, swagger etc.) — essas são peças técnicas do ecossistema/stack, não a definição da plataforma.
6. **Não** significa ausência de sistemas nos tribunais.
7. **Não** é política de segurança cibernética (ENSEC é outro eixo; não misturar).
8. **Não** é “banco de dados nacional único obrigatório para tudo”.

### 1.8 Monólito × plataforma (contraste FCC)

| Aspecto | Monólito (visão errada da PDPJ) | Plataforma / ecossistema (visão correta) |
|---------|----------------------------------|------------------------------------------|
| Deploy | Um artefato único nacional | Capacidades/serviços que integram e evoluem |
| Falha | Risco de “tudo ou nada” | Isolamento relativo de partes |
| Tribunais | Absorvidos num sistema só | Participam e interoperam |
| Padrão | Código único imposto | Padrões + serviços + governança |
| Integração | Internamente acoplada | Por contratos/APIs entre partes |

Em prova, se o enunciado falar em **ecossistema**, **interoperabilidade**, **serviços compartilhados** e **diretrizes nacionais**, a resposta caminha para **plataforma**. Se falar em **substituição total por um único sistema**, desconfie.

### 1.9 O que a FCC tipicamente testa neste tema

- Definição: PDPJ = **plataforma/ecossistema digital nacional** do PJ.
- Finalidade: **interoperabilidade**, reuso, padronização, modernização sob governança.
- Natureza: **não monólito**; não “só um portal”.
- Papéis: **CNJ (diretrizes/governança)** × **tribunais (adesão/operação/integração)**.
- Contrastes: silo × plataforma; monólito × ecossistema de serviços.
- Armadilhas: misturar PDPJ com ferramenta pontual da stack; misturar com ENSEC; dizer que tribunais deixam de ter sistemas próprios.

Para este arquivo, memorize o **conceito institucional-arquitetural**. Stack (Eureka/Zuul/Swagger/MapStruct) entra depois, como meios técnicos — não como definição da PDPJ.

### 1.10 Mini-síntese (cartão mental)

> **PDPJ-Br** = plataforma digital nacional do Judiciário → **interoperar + reusar + padronizar** serviços entre tribunais, sob **governança CNJ**, sem ser monólito único nem substituir a existência dos tribunais.

---

## 2. Questões de treino (1–15)

**1.** A leitura mais adequada da PDPJ-Br, no nível cobrado em concurso de Analista de TI do Judiciário, é:

A) Um único sistema monolítico obrigatório que substitui todos os sistemas processuais dos tribunais  
B) Um ecossistema/plataforma nacional de integração digital do Poder Judiciário  
C) Apenas o portal de notícias do CNJ na internet  
D) Sinônimo estrito de banco de dados nacional único para todos os tribunais  
E) Política exclusiva de segurança cibernética do PJ  

**2.** Em relação à finalidade típica atribuída à PDPJ-Br, assinale a alternativa que melhor reúne os eixos cobráveis:

A) Isolar cada tribunal em silos absolutos sem padrão comum  
B) Interoperabilidade, reuso de capacidades digitais e padronização sob diretrizes nacionais  
C) Eliminar a existência institucional dos tribunais regionais  
D) Substituir o CNJ pelas secretarias de TI de cada tribunal  
E) Proibir o uso de APIs entre órgãos do Judiciário  

**3.** Sobre a relação entre a PDPJ-Br e os tribunais, é correto afirmar que:

A) A plataforma extingue a necessidade de qualquer sistema local nos tribunais  
B) Os tribunais participam do ecossistema, aderindo a padrões e integrando/consumindo serviços, sem deixar de existir como órgãos  
C) Cada tribunal é obrigado a abandonar qualquer governança interna de TI  
D) A PDPJ-Br é propriedade exclusiva de um único TRT  
E) Tribunais não se relacionam com a plataforma; só o Executivo federal  

**4.** Interoperabilidade, no discurso típico da PDPJ-Br, significa principalmente:

A) Fusionar todos os códigos-fonte dos tribunais em um único repositório privado  
B) Capacidade de sistemas distintos cooperarem e trocarem informações com padrões/contratos previsíveis  
C) Compartilhar obrigatoriamente o mesmo servidor físico em Brasília  
D) Usar apenas planilhas enviadas por e-mail entre varas  
E) Impedir qualquer integração entre tribunais  

**5.** Assinale a afirmação **incorreta** sobre a PDPJ-Br:

A) É associada a um ecossistema digital nacional do Judiciário  
B) Busca reduzir isolamento extremo entre soluções judiciais digitais  
C) É corretamente descrita como monólito único substituto de todos os sistemas processuais  
D) Relaciona-se a padrões e integração entre órgãos  
E) Envolve ideia de governança nacional sobre o digital do PJ  

**6.** No contraste monólito × plataforma aplicado à PDPJ-Br, a visão de **plataforma** enfatiza:

A) Um único artefato de software nacional com deploy indivisível para todo o país  
B) Capacidades e serviços que se integram sob padrões comuns, com participação dos tribunais  
C) A proibição de qualquer serviço compartilhado  
D) A centralização física de todos os datacenters judiciais em um único prédio  
E) A substituição do CNJ por uma empresa privada de software  

**7.** Qual papel institucional combina melhor com a ideia de governança da PDPJ-Br no nível deste subtópico?

A) CNJ define diretrizes/padrões do ecossistema; tribunais operam e integram no seu contexto  
B) Cada vara cria o padrão nacional sem coordenação  
C) O CNJ apaga administrativamente todos os tribunais  
D) Apenas o STF escreve código das APIs de todos os TRTs  
E) Governança digital é responsabilidade exclusiva do Poder Executivo  

**8.** Um candidato afirma: “PDPJ-Br é só o nome comercial do Spring Boot.” A correção adequada é:

A) Correto: PDPJ-Br e Spring Boot são sinônimos no edital  
B) Incorreto: a PDPJ-Br é a plataforma/ecossistema digital do Judiciário; frameworks são meios técnicos, não a definição da plataforma  
C) Correto: PDPJ-Br é apenas MapStruct  
D) Correto: PDPJ-Br é apenas um gateway HTTP  
E) Incorreto apenas porque o nome certo seria Eureka  

**9.** A alternativa que melhor expressa o que a PDPJ-Br **não** é:

A) Ecossistema nacional de integração digital do PJ  
B) Iniciativa associada a interoperabilidade entre órgãos judiciais  
C) Meramente um site institucional de notícias do CNJ  
D) Plataforma sob diretrizes nacionais de governança digital  
E) Ambiente em que tribunais podem reutilizar capacidades digitais comuns  

**10.** Em um enunciado FCC que contraste “silo absoluto” e “plataforma nacional”, a PDPJ-Br alinha-se a:

A) Silo absoluto sem padrão compartilhado  
B) Plataforma/ecossistema com interoperabilidade e padrões comuns  
C) Ausência total de sistemas digitais no Judiciário  
D) Proibição de reuso de serviços entre tribunais  
E) Substituição do processo judicial por planilha offline  

**11.** Sobre padronização no contexto da PDPJ-Br, a leitura mais coerente é:

A) Padronizar interfaces/práticas mínimas para integração, sem exigir monólito único  
B) Padronizar significa obrigar o mesmo código-fonte idêntico em todos os tribunais, sem exceção conceitual  
C) Padronização é irrelevante para interoperabilidade  
D) Padronização equivale a unificar fisicamente todos os bancos em uma só instância  
E) Padronização só existe fora do Poder Judiciário  

**12.** Um analista diz que “adotar a PDPJ-Br implica que o TRT4 deixa de ter sistemas próprios e passa a usar apenas um monólito nacional”. Essa afirmação é:

A) Correta, porque plataforma e monólito são conceitos idênticos  
B) Correta, porque tribunais são extintos pela PDPJ  
C) Incorreta: a plataforma pressupõe integração/ecossistema, não extinção automática dos sistemas e da existência operacional dos tribunais  
D) Correta apenas para a Justiça do Trabalho  
E) Incorreta somente porque o monólito seria estadual, não nacional  

**13.** Qual combinação de ideias melhor resume a PDPJ-Br para revisão rápida?

A) Portal de notícias + proibição de APIs + silos absolutos  
B) Ecossistema nacional + interoperabilidade + reuso/padronização + governança CNJ + participação dos tribunais  
C) Banco único obrigatório + fim dos tribunais + ENSEC como sinônimo  
D) Apenas documentação Swagger sem qualquer dimensão institucional  
E) Apenas service discovery, sem finalidade de integração  

**14.** Em prova, misturar PDPJ-Br com política de segurança cibernética do PJ (ENSEC) é problema porque:

A) São exatamente o mesmo instituto normativo e técnico  
B) A PDPJ, neste eixo, é plataforma/ecossistema de integração digital; segurança cibernética é outro eixo, ainda que ambos digam respeito ao digital do PJ  
C) ENSEC substitui a PDPJ em todos os editais  
D) A PDPJ existe só para criptografia de disco  
E) Não há qualquer tema digital no CNJ além de ENSEC  

**15.** A afirmação mais alinhada ao contraste “o que a FCC testa” neste subtópico é:

A) Cobrança de sintaxe detalhada de anotações MapStruct como definição da PDPJ  
B) Cobrança do conceito de plataforma/ecossistema, finalidade de interoperabilidade e relação CNJ–tribunais, evitando a leitura monolítica  
C) Cobrança exclusiva de comandos Linux do datacenter do CNJ  
D) Cobrança de memorizar o organograma completo de todos os TRTs  
E) Cobrança de que PDPJ = apenas uma ferramenta de gateway  

---

### Gabarito comentado (treino 1–15)

1. **B** — Ecossistema/plataforma nacional; A/D são monólito/banco único; C reduz a site; E confunde com ENSEC.  
2. **B** — Eixos clássicos: interoperar, reusar, padronizar sob diretrizes nacionais.  
3. **B** — Tribunais participam; não são apagados (A/C/D/E erram o papel institucional).  
4. **B** — Interoperar = cooperação/troca previsível entre sistemas distintos.  
5. **C** — Justamente a negação: PDPJ **não** é monólito substituto total.  
6. **B** — Plataforma = capacidades/serviços + padrões + participação.  
7. **A** — CNJ dirige o ecossistema; tribunais operam/integram.  
8. **B** — Framework ≠ definição da plataforma.  
9. **C** — Negação correta: não é “só site de notícias”.  
10. **B** — Alinha-se à plataforma, não ao silo.  
11. **A** — Padrões mínimos de integração ≠ monólito idêntico.  
12. **C** — Participação/integração ≠ extinção automática de sistemas/tribunais.  
13. **B** — Cartão mental completo do subtópico.  
14. **B** — Eixos diferentes; não sinônimos.  
15. **B** — Foco conceitual institucional-arquitetural deste arquivo.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT17 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 69 de 70)

No Art. 4º da Resolução nº 335 de 29/09/2020 é estabelecido que a Plataforma Digital do Poder Judiciário – PDPJ-Br adotará, obrigatoriamente, soluções que abranjam, dentre outros, os seguintes conceitos: modalidade, acessibilidade, usabilidade, microsserviços, computação em nuvem,

A) e processo eletrônico em plataforma privada.

B) ampla cobertura de testes, alto acoplamento, baixa coesão e modularização.

C) e desenvolvimento centralizado na esfera principal do Poder Judiciário.

D) e substituição das atividades rotineiras ou sequenciais por ações manuais.

E) e autenticação uniformizada.


**17.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60)

Nos projetos da Plataforma Digital do Poder Judiciário PDPJ-Br,

A) os ambientes de deployment estão instalados em clusters Kubernetes, gerenciados por meio da ferramenta open source de orquestração de containers Rancher.

B) a aplicação deve possuir os arquivos `yanl`, para que o deploy ocorra de forma automática por meio de pipelines de CI/CD (Continuous Delivery e Continuous Integration) executados pelo GitLab do STF (Supremo Tribunal Federal).

C) a aplicação deve possuir um arquivo `Rancherfile`, especificando o modo pelo qual ela deve ser empacotada.

D) deve haver o arquivo `.gitlab-ci.xml` definindo a pipeline de deploy automático.

E) deve haver, na pasta `kubernets`, o arquivo de configuração do ingress da aplicação, que é obrigatório.


**18.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 45 de 60)

Segundo a Portaria do CNJ nº 131, de 5 de maio de 2021, um dos objetivos do Grupo Revisor de Código-Fonte é

A) executar teste de qualidade de código-fonte e garantias operacionais da funcionalidade desenvolvida ou alterada.

B) realizar a análise sintática automatizada de qualidade de código, segundo parâmetros de complexidade, manutenibilidade e acessibilidade.

C) realizar testes somente nas soluções disponibilizadas na PDPJ, descartando as soluções concorrentes.

D) promover a análise das mudanças de código-fonte implementadas somente pelo CNJ e PDPJ.

E) corrigir e aprovar códigos-fonte que não alcançaram os critérios mínimos definidos durante a análise sintática.


**19.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 58 de 60)

Considere a Portaria CNJ nº 252/2020, que institui a Rede de Governança da Plataforma Judicial do Poder Judiciário Brasileiro (PDPJ-Br) e seu artigo 4º.Na composição da Rede,

A) inclui-se a Gerência Executiva da Secretaria Especial de Informática.

B) inclui-se o Comitê Gestor Regional da CNJ.

C) inclui-se a Comissão Permanente de Licitação em Tecnologia da Informação.

D) incluem-se os Comitês Gestores dos Tribunais.

E) incluem-se os Comitês Gestores dos Órgãos Públicos das três esferas de Governo.


**20.** (FGV | TJTO 2022 | Técnico Judiciário, Informática | Questão 78 de 80)

De acordo com o Art. 5º da Portaria CNJ nº 253/2020, os serviços que implementam as funcionalidades essenciais básicas para um sistema de processo judicial de tramitação eletrônica, bem como aqueles serviços necessários à integração, à coreografia e à interoperabilidade entre os serviços e soluções que compõem a PDPJ-Br (Plataforma Digital do Poder Judiciário) são os serviços:

A) negociais;

B) estruturantes;

C) de integração com sistemas externos;

D) de aplicações da comunidade externa ao Judiciário;

E) de nuvem computacional.


**21.** (FCC | TRF4 2025 | Técnico Judiciário, Área Apoio Especializado, Especialidade Desenvolvimento de Sistema da Informação | Questão 50 de 50)

O Art. 11 da Portaria CNJ nº 253/2020 estabelece que o acesso aos microsserviços deverá ser protegido com mecanismos de autenticação e autorização

A) baseados em OAuth2 a ser provido pelo serviço de Single SignOn da PDPJ-Br.

B) por meio de autenticação de usuário já cadastrado no sistema de processo eletrônico específico do tribunal em que o processo tramita.

C) e utilizar exclusivamente navegadores de internet homologados pelo Conselho Nacional de Justiça (CNJ).

D) baseados em SSL a serem providos pelo serviço de suporte operacional da PDPJ-Br.

E) mediante concórdia com os termos de uso da plataforma no primeiro acesso.


**22.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60)

A autenticação na Plataforma Digital do Poder Judiciário PDPJ-Br ocorre por meio de uma solução A, baseada em um aplicativo open source de gestão de identidades e acesso, denominado B, com base no protocolo C. De acordo com o texto acima, A, B e C são, correta e respectivamente:

A) Keycloak – Docker – Simple Object Access Protocol (SOAP)

B) Single Sign-On (SSO) – Keycloak – OAuth2 (RFC 6749)

C) Single Sign-On (SSO) – RabbitMQ – Advanced Message Queuing Protocol (AMQP)

D) de access token – Keycloak – Simple Object Access Protocol (SOAP)

E) Keycloak – RabbitMQ – OAuth2 (RFC 4967)


**23.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60)

Ao usar o Gateway API com o Service Discovery no contexto da PDPJ-Br, a principal consequência para o frontend em uma aplicação baseada em microsserviços é que o

A) Gateway API diminui a necessidade de autenticação, uma vez que o Service Discovery é integrado ao SSO.

B) frontend deve configurar manualmente os IPs e portas dos serviços disponíveis na plataforma.

C) frontend se torna o responsável pela chamada direta de cada microsserviço individualmente.

D) Gateway API reduz o número de chamadas externas que o frontend precisa fazer, tornando a comunicação mais eficiente.

E) frontend deve gerenciar manualmente as atualizações de endpoint de cada serviço individual registrado.


**24.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60)

De acordo com a Plataforma Digital do Poder Judiciário - PDPJ-Br, para que uma API seja considerada do tipo RESTful, ela precisa estar em conformidade, dentre outros, com o seguinte critério:

A) ter uma arquitetura cliente/servidor formada por clientes, servidores e recursos, com solicitações gerenciadas por meio de HTTP.

B) possuir o sistema de comunicação entre os componentes por meio de troca de mensagens REST-Enq, fazendo uso de um Message Broker. No caso da PDPJ, utiliza-se a solução BrokerMQ.

C) ter uma interface ampla e livre entre os componentes, desde que sejam usados os parâmetros RESTful Config para que as informações sejam transferidas em um formato padronizado.

D) ter autenticação por meio de uma solução de RFPJ Sign On baseada no protocolo PDPJ Auth2.

E) permitir que as APIs registradas sejam acessíveis por meio do Gateway RESTful-API-Sync.


**25.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22) *[aproximação temática]*

*Stack PDPJ (Spring/Zuul/Eureka/Swagger/Flyway/Envers).*

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.

B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.

C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.

D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.

E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **E** | FCC | TRT17 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 69 de 70 |
| 17 | **A** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60 |
| 18 | **A** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 45 de 60 |
| 19 | **D** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 58 de 60 |
| 20 | **B** | FGV | TJTO 2022 | Técnico Judiciário, Informática | Questão 78 de 80 |
| 21 | **A** | FCC | TRF4 2025 | Técnico Judiciário, Área Apoio Especializado, Especialidade Desenvolvimento de Sistema da Informação | Questão 50 de 50 |
| 22 | **B** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60 |
| 23 | **D** | FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60 |
| 24 | **A** | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60 |
| 25 | **E** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt17-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q69
- Q17: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q29
- Q18: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q45
- Q19: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q58
- Q20: https://www.quizinho.com.br/questao/fgv-tjto-2022-tecnico-judiciario-informatica-2022-q78
- Q21: https://www.quizinho.com.br/questao/fcc-trf4-2025-tecnico-judiciario-area-apoio-especializado-especialidade-desenvolvimento-de-sistema-da-informacao-2025-q50
- Q22: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q28
- Q23: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q39
- Q24: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q52
- Q25: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22

*Arquivo 01 · Visão PDPJ-Br · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 microsserviços.*

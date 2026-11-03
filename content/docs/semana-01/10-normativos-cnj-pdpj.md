+++
title = '10 — Normativos CNJ / PDPJ'
date = '2026-10-10'
draft = false
weight = 10
tags = ['semana-01', 'pdpj', 'cnj']
categories = ['estudo']
+++
# Normativos CNJ / PDPJ · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 10 — Normativos CNJ ligados à PDPJ  
**Escopo deste arquivo:** Resoluções **443/2022**, **522/2023**, **574/2024**; Portarias **253/2020**, **252/284** (pacote 2020–2021), **131/2021**; mapa “decorar × reconhecer”; anti-confusão com **ENSEC (396)**, **CNJ 400** e **LGPD**.  
**Fora de escopo aqui:** visão institucional da PDPJ (arquivo 01); stack Eureka/Zuul/Swagger/MapStruct (02–09); aprofundamento ENSEC (semana 8); sustentabilidade CNJ 400 (arquivo 11).

---

## 1. Material

### 1.1 Como a FCC cobra normativos neste eixo

Critério de estudo da semana 1:

| Nível | O que fazer | O que **não** fazer |
|-------|-------------|---------------------|
| **Decorar** | Número + **1 frase-âncora** + eixo (PDPJ / MoReq / dados-serviços / portaria técnica) | Ler DOU inteiro; decorar anexos e prazos internos |
| **Reconhecer** | Se a questão citar o número, apontar o **tema** | Comparar parágrafo a parágrafo entre normas |
| **Anti-confusão** | Separar eixos vizinhos (segurança, sustentabilidade, LGPD) | Tratar tudo “digital do CNJ” como sinônimo |

Em prova de Analista TI, o distrator clássico é **trocar o número certo pelo tema errado** (ex.: 443 → sustentabilidade; 396 → microsserviços; 522 → ENSEC).

Flash: **número + eixo > texto integral**.

### 1.2 Linha mental do pacote do edital

O edital do código 14 cita, no bloco PDPJ/Judiciário: **574/2024**, **522/2023**, **443/2022**, **396/2021 (ENSEC)** e as portarias **284/2021**, **162/2021**, **131/2021**, **253/2020**.

Para este arquivo, foque:

```
Portarias (operacionalizam a plataforma)
        ↓
443/2022  →  PDPJ nos editais/capacitações de TIC
522/2023  →  MoReq-Jus (requisitos de sistemas de processos/documentos)
574/2024  →  dados públicos + serviços na PDPJ + portal unificado (marco recente)
```

**396/2021** e **Portaria 162/2021** = **ENSEC** → só anti-confusão aqui (segurança ≠ arquitetura/plataforma).  
**CNJ 400/2021** = sustentabilidade → Gerais (arquivo 11), não misturar com 443.

Contexto útil (não precisa decorar como “do edital PDPJ”): a **Res. CNJ 335/2020** é o marco que **institui** a política da PDPJ-Br; a **574/2024** atualiza esse regime. Em prova, se perguntarem “qual cria a PDPJ”, a leitura correta aponta **335**; no pacote cobrado do edital para TI, **443/522/574 + portarias** são as âncoras nominadas.

### 1.3 Resolução CNJ nº 443/2022

**Frase-âncora:** dispõe sobre a **aplicação e disseminação dos conhecimentos** sobre a **PDPJ-Br** em **editais de concursos**, seleções e **capacitações** de cargos de **TIC** do Poder Judiciário.

O que a FCC testa:

- Número **443/2022** amarrado a **PDPJ + concurso/capacitação de TI**.
- Ideia: o servidor/candidato de TIC do PJ precisa conhecer a plataforma (por isso o tema cai no edital).
- **Não** é: política de sustentabilidade; ENSEC; LGPD; MoReq-Jus; regimento do TRT4.

| Certo | Errado (distrator) |
|-------|--------------------|
| PDPJ nos editais/capacitações de TIC | “Institui a ENSEC” |
| Disseminação de conhecimento sobre a plataforma | “Política de sustentabilidade 400” |
| Ligada ao ecossistema digital do PJ | “Revoga a LGPD” |

### 1.4 Resolução CNJ nº 522/2023

**Frase-âncora:** institui o **MoReq-Jus** — **Modelo de Requisitos** para sistemas informatizados de **gestão de processos e documentos** do Poder Judiciário — e disciplina a **obrigatoriedade** de sua utilização no desenvolvimento e na manutenção desses sistemas (atividades judiciárias e administrativas).

O que a FCC testa:

- Número **522/2023** ↔ **MoReq-Jus** / requisitos de sistemas de processos e documentos.
- Ideia de **padronização de requisitos** (não “só um guia opcional de marketing”).
- Relação com o ecossistema digital do PJ (sistemas de gestão), **sem** ser sinônimo de ENSEC nem de sustentabilidade.

| Certo | Errado (distrator) |
|-------|--------------------|
| MoReq-Jus / modelo de requisitos | “Só segurança cibernética (396)” |
| Obrigatoriedade no desenvolvimento/manutenção | “Apenas recomendação voluntária sem efeito” |
| Processos e documentos judiciais/administrativos | “Código de Conduta ético do TRT4” |

### 1.5 Resolução CNJ nº 574/2024

**Frase-âncora:** marco **2024** do ecossistema — trata de **acesso a dados judiciais públicos** consolidados pelo CNJ, prevê **depósito/oferta de serviços** (inclusive privados, sob condições) na **PDPJ-Br** e institui **portal unificado** para usuários internos; atualiza o regime da política da plataforma (linha da 335).

Pontos cobráveis em nível “frase + eixo”:

1. **Dados judiciais públicos** / acesso consolidado (APIs / interfaces — ideia de consumo controlado).  
2. **Serviços na PDPJ-Br** (fomento a soluções úteis no marketplace/plataforma).  
3. **Portal unificado** para usuários internos.  
4. É o **marco mais recente** da trinca 443 → 522 → 574 no pacote do edital.

O que **não** é:

- Não “revoga a LGPD”.
- Não “é a ENSEC”.
- Não “é a política de sustentabilidade 400”.
- Não exige decorar bilhetagem, SEI interno ou anexos longos na semana 1.

### 1.6 Portarias do pacote PDPJ

Trate portaria como **camada operacional/técnica** abaixo das resoluções de política.

| Norma | Frase-âncora (decorar) | Não confundir com |
|-------|------------------------|-------------------|
| **Portaria CNJ 253/2020** | **Critérios e diretrizes técnicas** para desenvolvimento de **módulos e serviços** na PDPJ-Br | ENSEC / sustentabilidade |
| **Portaria CNJ 252** *(modelo de governança/gestão da PDPJ)* **+ 284/2021** *(alteração; número do edital)* | **Modelo de governança e gestão** da PDPJ-Br; amarrar **284/2021** como portaria do pacote edital | **162/2021** (ENSEC) |
| **Portaria CNJ 131/2021** | **Grupo Revisor de Código-Fonte** da PDPJ-Br e do PJe (governança de código/revisão) | Gateway/Eureka como “definição da portaria” |
| **Portaria CNJ 162/2021** *(só anti-confusão)* | ENSEC — segurança cibernética | Pacote de arquitetura/plataforma |

Leitura de prova: **253 = técnico/módulos**; **252/284 = governança/gestão da plataforma**; **131 = revisão de código**; **162 = segurança (outro eixo)**.

### 1.7 Tabela-mestra (cartão único)

| Norma | Ano | Eixo em 1 linha |
|-------|-----|-----------------|
| Res. **443** | 2022 | PDPJ nos **editais/capacitações** de TIC |
| Res. **522** | 2023 | **MoReq-Jus** (requisitos de sistemas de processos/documentos) |
| Res. **574** | 2024 | **Dados públicos** + **serviços na PDPJ** + portal unificado |
| Port. **253** | 2020 | Diretrizes técnicas de **módulos/serviços** PDPJ |
| Port. **252/284** | 2020–21 | **Governança/gestão** da PDPJ (284 no edital) |
| Port. **131** | 2021 | **Revisor de código-fonte** PDPJ/PJe |
| Res. **396** | 2021 | **ENSEC-PJ** — segurança (não misturar) |
| Res. **400** | 2021 | **Sustentabilidade** — Gerais (arquivo 11) |
| **LGPD** | Lei 13.709/2018 | Proteção de **dados pessoais** (eixo próprio) |

### 1.8 Anti-confusão: ENSEC × CNJ 400 × LGPD × PDPJ

| Instituto | Número típico | Tema | Onde cai |
|-----------|---------------|------|----------|
| **PDPJ / plataforma** | 335 (criação); 443, 522, 574 + portarias 253/131/284 | Plataforma, requisitos, dados/serviços, TIC | Específicas TI |
| **ENSEC-PJ** | Res. **396/2021** + Port. **162/2021** | **Segurança cibernética** do PJ | Específicas (depois); agora só separar |
| **Sustentabilidade** | Res. **400/2021** | Política ambiental/social/econômica no PJ | **Gerais / Legislação** |
| **LGPD** | Lei **13.709/2018** (+ atos de adequação no PJ) | Dados **pessoais**, bases legais, direitos do titular | Pode cruzar com TI, mas **não** é sinônimo de 443/574 |

Armadilhas FCC:

1. **443 = sustentabilidade** → errado (isso é **400**).  
2. **396 = MoReq-Jus / microsserviços** → errado (396 = **ENSEC**).  
3. **574 = revoga LGPD** → errado (574 convive com proteção de dados; não “acaba” com a LGPD).  
4. **284 = ENSEC** → errado (**162** é a portaria ENSEC do pacote).  
5. **522 = Código de Conduta TRT4** → errado (522 = **MoReq-Jus**).  
6. Tratar **portaria** como se fosse a **definição institucional** da PDPJ inteira → portaria operacionaliza; política/plataforma têm resoluções próprias.

### 1.9 O que a FCC tipicamente testa neste subtópico

- Amarrar **número ↔ tema** (443/522/574/portarias).  
- Linha **base operacional → requisitos → marco recente** sem exigir artigo a artigo.  
- Separar **PDPJ** de **ENSEC**, **sustentabilidade** e **LGPD**.  
- Saber que **portarias** detalham técnica/governança/código; **resoluções** do pacote dão o marco cobrado no edital.  
- Reconhecer que conhecimento de PDPJ em **concurso de TIC** tem lastro normativo (**443**).

### 1.10 Mini-síntese (cartão mental)

> **443** = PDPJ nos editais de TI · **522** = MoReq-Jus · **574** = dados/serviços/portal na plataforma (2024) · **253/252-284/131** = técnica, governança e revisão de código · **396/162** = ENSEC · **400** = sustentabilidade · **LGPD** = dados pessoais — **não misturar os eixos**.

---

## 2. Questões de treino (1–15)

**1.** A Resolução CNJ nº **443/2022**, no eixo cobrável para Analista de TI, relaciona-se principalmente a:

A) Política de sustentabilidade do Poder Judiciário  
B) Disseminação/aplicação dos conhecimentos sobre a PDPJ-Br em editais, seleções e capacitações de TIC  
C) Estratégia Nacional de Segurança Cibernética (ENSEC-PJ)  
D) Código de Conduta ético exclusivo do TRT4  
E) Revogação integral da LGPD  

**2.** A Resolução CNJ nº **522/2023** é corretamente associada a:

A) ENSEC e Portaria 162/2021 apenas  
B) Instituição do **MoReq-Jus** (modelo de requisitos para sistemas de gestão de processos e documentos)  
C) Agenda 2030 e ODS exclusivamente  
D) Regimento Interno do TRT4  
E) Apenas cabeamento estruturado ABNT  

**3.** Sobre a Resolução CNJ nº **574/2024**, a leitura mais adequada em nível de concurso é:

A) Trata só de Windows Server e proíbe APIs  
B) É o marco 2024 ligado a dados judiciais públicos, serviços na PDPJ-Br e portal unificado  
C) Substitui a CNJ 400 como política de sustentabilidade  
D) É sinônimo rigoroso da ENSEC-PJ  
E) Extingue a PDPJ-Br  

**4.** A Portaria CNJ nº **253/2020**, no pacote PDPJ, amarra-se sobretudo a:

A) Critérios/diretrizes técnicas para módulos e serviços na PDPJ-Br  
B) Política de assédio moral no PJ  
C) Neutralidade de carbono obrigatória como único objeto  
D) Código penal militar  
E) Exclusivamente à LGPD federal sem relação com a plataforma  

**5.** No contraste entre Portaria **284/2021** (pacote edital) e Portaria **162/2021**, a distinção cobrável é:

A) Ambas são rigorosamente ENSEC e nada mais  
B) 284 integra o pacote de governança/gestão da PDPJ; 162 liga-se à ENSEC (segurança)  
C) 162 institui o MoReq-Jus  
D) 284 revoga a Constituição  
E) Não há diferença temática possível em prova  

**6.** A Portaria CNJ nº **131/2021** é melhor descrita como ligada a:

A) Grupo revisor de código-fonte da PDPJ-Br e do PJe  
B) Política nacional de sustentabilidade 400  
C) Exclusivamente a coleta seletiva solidária  
D) Substituição do CNJ pelo Executivo  
E) Banimento de microsserviços no Judiciário  

**7.** Assinale a associação **correta**:

A) 396/2021 — MoReq-Jus  
B) 400/2021 — ENSEC-PJ  
C) 443/2022 — PDPJ em editais/capacitações de TIC  
D) 522/2023 — só sustentabilidade ambiental  
E) 574/2024 — Código de Conduta TRT4  

**8.** Em relação à linha **443 → 522 → 574**, a leitura mais adequada é:

A) Todas tratam exclusivamente de ENSEC  
B) Formam marcos sucessivos do pacote digital/PDPJ-requisitos-dados no edital (2022–2024), com eixos distintos  
C) 574 revoga a LGPD e a 400 ao mesmo tempo  
D) 443 trata só de mobiliário sustentável  
E) Nenhuma guarda relação com o digital do PJ  

**9.** Misturar a Res. **396/2021 (ENSEC)** com a arquitetura/plataforma PDPJ é problema porque:

A) São o mesmo ato normativo com dois nomes  
B) ENSEC é eixo de **segurança cibernética**; PDPJ/443–574/portarias operam o eixo de plataforma/requisitos/dados digitais  
C) A ENSEC substitui o MoReq-Jus em todo edital  
D) Não existe norma de segurança no CNJ  
E) A 396 institui o MapStruct  

**10.** Assinale a afirmação **incorreta**:

A) A 522/2023 institui o MoReq-Jus  
B) A 443/2022 relaciona PDPJ a concursos/capacitações de TIC  
C) A 400/2021 é a política de sustentabilidade (Gerais), não o sinônimo da 443  
D) A 574/2024 é corretamente descrita como “apenas regimento interno do TRT4”  
E) A Portaria 253/2020 traz diretrizes técnicas de módulos/serviços na PDPJ  

**11.** A LGPD, no anti-confusão deste subtópico:

A) É idêntica à Res. 443/2022  
B) É lei de proteção de dados pessoais; convive com o ecossistema digital, mas não se confunde com 443/522/574  
C) Foi revogada expressamente pela 574/2024  
D) Substitui o MoReq-Jus  
E) É o nome popular da ENSEC  

**12.** Um candidato afirma: “Portaria 253/2020 é a política de sustentabilidade do PJ.” A correção é:

A) Correto: 253 = CNJ 400  
B) Incorreto: 253 amarra diretrizes técnicas da PDPJ; sustentabilidade é eixo da Res. 400/2021  
C) Correto: 253 = ENSEC  
D) Correto apenas na Justiça Eleitoral  
E) Incorreto somente porque o número certo seria 162  

**13.** Qual combinação resume melhor o pacote de portarias cobrado?

A) 253 técnica/módulos · 252/284 governança-gestão PDPJ · 131 revisor de código · (162 = ENSEC, outro eixo)  
B) Todas = sustentabilidade 400  
C) Todas = LGPD pura  
D) 131 = ENSEC e 162 = MoReq-Jus  
E) Portarias substituem todas as resoluções do CNJ  

**14.** A Res. **400/2021** aparece neste arquivo principalmente para:

A) Ser decorada artigo a artigo como se fosse Eureka  
B) Servir de **anti-confusão**: sustentabilidade ≠ PDPJ 443/522/574  
C) Substituir a 574 no edital de TI  
D) Definir microsserviços Spring Cloud  
E) Instituir o MoReq-Jus  

**15.** A síntese mais alinhada ao que a FCC testa aqui é:

A) Sintaxe de anotações MapStruct como definição da 443  
B) Número + eixo das resoluções/portarias do pacote PDPJ, sem misturar ENSEC, 400 e LGPD  
C) Organograma completo de todos os TRTs  
D) Comandos Linux do datacenter do CNJ  
E) Que 522 = apenas site de notícias  

---

### Gabarito comentado (treino 1–15)

1. **B** — 443 = PDPJ em editais/capacitações de TIC; A é 400; C é 396; D é TRT4; E inventa revogação da LGPD.  
2. **B** — 522 = MoReq-Jus.  
3. **B** — 574 = dados públicos + serviços PDPJ + portal (marco 2024).  
4. **A** — 253 = diretrizes técnicas módulos/serviços.  
5. **B** — 284 no pacote PDPJ/governança; 162 = ENSEC.  
6. **A** — 131 = grupo revisor de código-fonte.  
7. **C** — única amarração correta.  
8. **B** — linha temporal com eixos distintos no pacote do edital.  
9. **B** — segurança ≠ plataforma/requisitos.  
10. **D** — justamente a incorreta: 574 não é regimento do TRT4.  
11. **B** — LGPD = dados pessoais; eixo próprio.  
12. **B** — 253 ≠ 400.  
13. **A** — mapa das portarias.  
14. **B** — anti-confusão com sustentabilidade.  
15. **B** — foco deste arquivo.

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


**17.** (FCC | TRF4 2025 | Técnico Judiciário, Área Apoio Especializado, Especialidade Desenvolvimento de Sistema da Informação | Questão 50 de 50)

O Art. 11 da Portaria CNJ nº 253/2020 estabelece que o acesso aos microsserviços deverá ser protegido com mecanismos de autenticação e autorização

A) baseados em OAuth2 a ser provido pelo serviço de Single SignOn da PDPJ-Br.

B) por meio de autenticação de usuário já cadastrado no sistema de processo eletrônico específico do tribunal em que o processo tramita.

C) e utilizar exclusivamente navegadores de internet homologados pelo Conselho Nacional de Justiça (CNJ).

D) baseados em SSL a serem providos pelo serviço de suporte operacional da PDPJ-Br.

E) mediante concórdia com os termos de uso da plataforma no primeiro acesso.


**18.** (FGV | TJTO 2022 | Técnico Judiciário, Informática | Questão 78 de 80)

De acordo com o Art. 5º da Portaria CNJ nº 253/2020, os serviços que implementam as funcionalidades essenciais básicas para um sistema de processo judicial de tramitação eletrônica, bem como aqueles serviços necessários à integração, à coreografia e à interoperabilidade entre os serviços e soluções que compõem a PDPJ-Br (Plataforma Digital do Poder Judiciário) são os serviços:

A) negociais;

B) estruturantes;

C) de integração com sistemas externos;

D) de aplicações da comunidade externa ao Judiciário;

E) de nuvem computacional.


**19.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 47 de 60)

Estabelecer governança de segurança cibernética e fortalecer a gestão e coordenação integrada de ações de segurança cibernética nos órgãos do Poder Judiciário é um dos objetivos da Estratégia Nacional de Segurança da Informação e Cibernética do Poder Judiciário (ENSEC-PJ), que consta na Resolução nº

A) 335/2020.

B) 131/2021.

C) 253/2020.

D) 162/2021.

E) 396/2021.


**20.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 60 de 60)

A Resolução CNJ nº 396/2021, Capítulo VI – Da Rede Nacional de Cooperação do Poder Judiciário na Área de Segurança Cibernética, em seu artigo 18, estabelece que a Rede de Cooperação do Judiciário na área de segurança cibernética tem os seguintes objetivos: Promover ambiente participativo, colaborativo e seguro entre os órgãos do Poder Judiciário, por meio do acompanhamento contínuo e proativo das ameaças e dos ataques cibernéticos; Estimular o compartilhamento de informações sobre incidentes e vulnerabilidades cibernéticas; Realizar exercícios cibernéticos com a participação de múltiplos entes; Fortalecer o Centro de Prevenção, Tratamento e Resposta a Incidentes Cibernéticos (CPTRIC-PJ) do CNJ; Aperfeiçoar a estrutura judiciária para o aprimoramento de investigações de crimes cibernéticos; Incentivar a criação e a atuação de ETIR em cada órgão do Poder Judiciário; Emitir alertas e recomendações de segurança cibernética; e, também,

A) Ampliar parceria com outros órgãos do Poder Executivo, do Poder Legislativo, do Ministério Público, da polícia judiciária, do setor privado e do meio acadêmico, com vistas a elevar, de modo geral, o nível de segurança cibernética.

B) Ampliar ações destinadas a assegurar a disponibilidade, a integridade, a confidencialidade e a autenticidade de dados e de informações.

C) Executar ações de comunicação, de conscientização, de formação de cultura e de direcionamento institucional com vistas à segurança cibernética.

D) Preservar a segurança física e proteção de dados pessoais e institucionais, nos aspectos relacionados à cibersegurança.

E) Aprimorar o nível de maturidade em segurança cibernética nos órgãos do Poder Judiciário, abrangendo os aspectos fundamentais da segurança da informação para o aperfeiçoamento necessário à consecução desse propósito.


**21.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 58 de 60)

Considere a Portaria CNJ nº 252/2020, que institui a Rede de Governança da Plataforma Judicial do Poder Judiciário Brasileiro (PDPJ-Br) e seu artigo 4º.Na composição da Rede,

A) inclui-se a Gerência Executiva da Secretaria Especial de Informática.

B) inclui-se o Comitê Gestor Regional da CNJ.

C) inclui-se a Comissão Permanente de Licitação em Tecnologia da Informação.

D) incluem-se os Comitês Gestores dos Tribunais.

E) incluem-se os Comitês Gestores dos Órgãos Públicos das três esferas de Governo.


**22.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 45 de 60)

Segundo a Portaria do CNJ nº 131, de 5 de maio de 2021, um dos objetivos do Grupo Revisor de Código-Fonte é

A) executar teste de qualidade de código-fonte e garantias operacionais da funcionalidade desenvolvida ou alterada.

B) realizar a análise sintática automatizada de qualidade de código, segundo parâmetros de complexidade, manutenibilidade e acessibilidade.

C) realizar testes somente nas soluções disponibilizadas na PDPJ, descartando as soluções concorrentes.

D) promover a análise das mudanças de código-fonte implementadas somente pelo CNJ e PDPJ.

E) corrigir e aprovar códigos-fonte que não alcançaram os critérios mínimos definidos durante a análise sintática.


**23.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 50)

Considere o seguinte teor da Portaria CNJ nº 162/2021:5. Competência de atuação5.1 Deverá ser formalmente instituída Equipe de Tratamento e Resposta a Incidentes de Segurança Cibernética (ETIR), em todos os órgãos do Poder Judiciário, à exceção do STF.5.2 A ETIR poderá solicitar apoio multidisciplinar para responder aos incidentes de segurança de maneira adequada e tempestiva,

A) em áreas como: tecnologia da informação, segurança da informação, jurídica, pesquisas judiciárias, comunicação, controle interno, segurança institucional, entre outras.

B) ao setor de segurança pública das três esferas administrativas governamentais, quando cabível e à área de comunicação federal quando o incidente contar com o risco de alastramento a nível país.

C) ao setor de segurança da informação, excetuando-se tecnologia da informação e comunicação, quando cabível, e à área de comunicação federal quando o incidente contar com o risco de alastramento a nível país.

D) em áreas como: gestão de dados em nuvem, segurança corporativa, administrativo-financeiras, excetuando-se gerência de materiais, tecnologia da informação e segurança institucional que por si só já atuam nesse mister.

E) ao setor de segurança pública das três esferas administrativas governamentais e dos três poderes, quando cabível, e à área de comunicação federal quando o incidente contar com o risco de alastramento a nível país.


**24.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60)

A autenticação na Plataforma Digital do Poder Judiciário PDPJ-Br ocorre por meio de uma solução A, baseada em um aplicativo open source de gestão de identidades e acesso, denominado B, com base no protocolo C. De acordo com o texto acima, A, B e C são, correta e respectivamente:

A) Keycloak – Docker – Simple Object Access Protocol (SOAP)

B) Single Sign-On (SSO) – Keycloak – OAuth2 (RFC 6749)

C) Single Sign-On (SSO) – RabbitMQ – Advanced Message Queuing Protocol (AMQP)

D) de access token – Keycloak – Simple Object Access Protocol (SOAP)

E) Keycloak – RabbitMQ – OAuth2 (RFC 4967)


**25.** (FCC | TRT20 2024 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 59 de 60)

A Resolução CNJ nº 522/2023 em seu Art. 2º estabelece que os sistemas informatizados de gestão de processos e documentos utilizados em atividades judiciais e administrativas dos órgãos integrantes do Poder Judiciário, ainda que segmentados em aplicações, módulos e microsserviços, deverão aderir aos requisitos do MoReq-Jus, com o objetivo de

A) acompanhar o fiel cumprimento desta Resolução e a coordenação do Programa de avaliação do grau de aderência dos sistemas ao MoReq-Jus e de atualização permanente (Programa MoReq-Aval).

B) avaliar o grau de aderência dos requisitos ao MoReq-Jus e de sua atualização permanente especificada no Programa MoReq-Aval/2023.

C) assegurar a concordância, a segurança, a especificidade, o grau de complexidade, a integridade, a razoabilidade e a não duplicidade de processos e documentos do Poder Judiciário.

D) identificar os requisitos funcionais, dados e metadados que serão tratados por serviços interligados ao sistema principal e as referências ao requisito funcional implementado e aos requisitos não funcionais observados por ocasião da especificação de novas funcionalidades ou de melhorias daquelas existentes.

E) assegurar a confidencialidade, a integridade, a disponibilidade, a autenticidade, o não repúdio, a conformidade e a preservação de processos e documentos do Poder Judiciário.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **E** | FCC | TRT17 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 69 de 70 |
| 17 | **A** | FCC | TRF4 2025 | Técnico Judiciário, Área Apoio Especializado, Especialidade Desenvolvimento de Sistema da Informação | Questão 50 de 50 |
| 18 | **B** | FGV | TJTO 2022 | Técnico Judiciário, Informática | Questão 78 de 80 |
| 19 | **E** | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 47 de 60 |
| 20 | **A** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 60 de 60 |
| 21 | **D** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 58 de 60 |
| 22 | **A** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 45 de 60 |
| 23 | **A** | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 50 |
| 24 | **B** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60 |
| 25 | **E** | FCC | TRT20 2024 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 59 de 60 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt17-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q69
- Q17: https://www.quizinho.com.br/questao/fcc-trf4-2025-tecnico-judiciario-area-apoio-especializado-especialidade-desenvolvimento-de-sistema-da-informacao-2025-q50
- Q18: https://www.quizinho.com.br/questao/fgv-tjto-2022-tecnico-judiciario-informatica-2022-q78
- Q19: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q47
- Q20: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q60
- Q21: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q58
- Q22: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q45
- Q23: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q50
- Q24: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q28
- Q25: https://www.quizinho.com.br/questao/fcc-trt20-2024-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q59

*Arquivo 10 · Normativos CNJ / PDPJ · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 11 CNJ 400.*

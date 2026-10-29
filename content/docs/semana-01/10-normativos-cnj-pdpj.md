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

## 3. Questões estilo concurso FCC (16–25)

**16.** No âmbito dos normativos do Conselho Nacional de Justiça relacionados à Plataforma Digital do Poder Judiciário e citados em editais de Analista de TI, a Resolução CNJ nº 443/2022 é corretamente caracterizada como ato que:

A) institui a política de sustentabilidade ambiental, social e econômica do Poder Judiciário  
B) dispõe sobre a aplicação e a disseminação dos conhecimentos acerca da PDPJ-Br em editais de concursos, seleções e capacitações para cargos de tecnologia da informação  
C) cria a Estratégia Nacional de Segurança Cibernética do Poder Judiciário em substituição à Portaria 253/2020  
D) revoga integralmente a Lei Geral de Proteção de Dados Pessoais  
E) aprova exclusivamente o Regimento Interno do Tribunal Regional do Trabalho da 4ª Região  

**17.** A Resolução CNJ nº 522/2023, no nível usualmente cobrado em provas da área de TI do Judiciário, relaciona-se à:

A) instituição do MoReq-Jus, modelo de requisitos para sistemas informatizados de gestão de processos e documentos, com disciplina de uso no desenvolvimento e na manutenção desses sistemas  
B) neutralidade de carbono como único objeto normativo, sem relação com sistemas  
C) substituição da PDPJ-Br por planilhas offline obrigatórias  
D) identidade absoluta com a Resolução CNJ nº 400/2021  
E) proibição de qualquer modelo de requisitos técnicos no PJ  

**18.** Acerca da Resolução CNJ nº 574/2024, assinale a alternativa correta.

A) Trata-se de marco recente que aborda acesso a dados judiciais públicos consolidados, previsão de serviços na PDPJ-Br e portal unificado para usuários internos  
B) Equivale, para todos os efeitos, à ENSEC-PJ (Res. 396/2021)  
C) Tem por objeto exclusivo o Código de Conduta ético do TRT4  
D) Extingue a possibilidade de uso de APIs no Poder Judiciário  
E) Confunde-se, por definição, com a política de sustentabilidade da Res. 400/2021  

**19.** Considere as afirmações abaixo.

I. A Portaria CNJ nº 253/2020 relaciona-se a critérios e diretrizes técnicas para módulos e serviços na PDPJ-Br.  
II. A Portaria CNJ nº 162/2021 integra o eixo da ENSEC e não deve ser tratada como sinônimo das portarias de arquitetura/governança da plataforma.  
III. A Resolução CNJ nº 400/2021 é o ato que institui o MoReq-Jus.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**20.** A associação correta entre norma e eixo temático é:

A) Res. 396/2021 — MoReq-Jus  
B) Res. 443/2022 — sustentabilidade no PJ  
C) Portaria 131/2021 — grupo revisor de código-fonte da PDPJ-Br e do PJe  
D) Res. 522/2023 — apenas ENSEC  
E) Res. 574/2024 — exclusivamente coleta seletiva solidária  

**21.** Assinale a opção **incompatível** com a leitura adequada dos normativos do pacote PDPJ do edital.

A) 443/2022 amarra conhecimento de PDPJ a concursos e capacitações de TIC  
B) 522/2023 institui modelo de requisitos (MoReq-Jus) para sistemas de processos e documentos  
C) 574/2024 dialoga com dados públicos, serviços na plataforma e portal unificado  
D) 443/2022 e 400/2021 são rigorosamente o mesmo instituto, ambos de sustentabilidade  
E) Portarias 253 e 284 operam camada técnica/de governança da plataforma, distinta da ENSEC  

**22.** Diferenciar a LGPD das Resoluções CNJ nº 443/2022, 522/2023 e 574/2024 é relevante em prova porque:

A) a LGPD é lei de proteção de dados pessoais, ao passo que aquelas resoluções tratam de eixos de plataforma digital, requisitos de sistemas e dados/serviços no ecossistema do PJ, sem se confundirem com a lei  
B) a 574/2024 revogou expressamente a LGPD em todo o território nacional  
C) a LGPD e a 443/2022 são sinônimos perfeitos  
D) a LGPD substitui o MoReq-Jus  
E) não há qualquer norma sobre dados no âmbito do Judiciário  

**23.** Um analista afirma que “a Portaria CNJ nº 284/2021 é idêntica à Portaria 162/2021, pois ambas criam a ENSEC e a PDPJ ao mesmo tempo”. Tal afirmação:

A) está correta, pois todos os números de 2021 são ENSEC  
B) está correta apenas para a Justiça Estadual  
C) está incorreta, pois 284 integra o pacote de governança/gestão da PDPJ no edital, enquanto 162 liga-se ao eixo ENSEC de segurança cibernética  
D) está correta, pois portarias não têm tema  
E) está incorreta somente porque o número da ENSEC seria 400  

**24.** No contraste entre Resolução CNJ nº 400/2021 e Resolução CNJ nº 443/2022, é correto concluir que:

A) ambas disciplinam exclusivamente microsserviços Spring Cloud  
B) a 400 trata de política de sustentabilidade no PJ (Gerais), ao passo que a 443 trata da PDPJ em editais/capacitações de TIC  
C) a 443 revoga a 400  
D) a 400 institui o MoReq-Jus  
E) não há diferença temática entre elas  

**25.** Para fins de preparação em Analista Judiciário – Tecnologia da Informação, a síntese mais adequada do pacote normativo deste subtópico é:

A) 443 = sustentabilidade; 522 = ENSEC; 574 = Conduta TRT4  
B) 443 = PDPJ nos editais de TIC; 522 = MoReq-Jus; 574 = dados/serviços/portal na plataforma; portarias 253/252-284/131 = técnica, governança e revisão de código; separando ENSEC (396/162), sustentabilidade (400) e LGPD  
C) apenas MapStruct e Swagger, sem dimensão normativa  
D) somente Portaria 162, ignorando 443/522/574  
E) monólito único nacional como definição de todas as resoluções  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **B** |
| 17 | **A** |
| 18 | **A** |
| 19 | **C** |
| 20 | **C** |
| 21 | **D** |
| 22 | **A** |
| 23 | **C** |
| 24 | **B** |
| 25 | **B** |

**Notas rápidas:** 16 B (443 = PDPJ em concursos de TIC); 17 A (MoReq-Jus); 18 A (574 = dados/serviços/portal); 19 C (I e II; III erra — MoReq é 522, não 400); 20 C (131 = revisor de código); 21 D (400 ≠ 443); 22 A (LGPD ≠ resoluções do pacote); 23 C (284 PDPJ × 162 ENSEC); 24 B (sustentabilidade × PDPJ/editais); 25 B (síntese completa).

---

*Arquivo 10 · Normativos CNJ / PDPJ · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 11 CNJ 400.*

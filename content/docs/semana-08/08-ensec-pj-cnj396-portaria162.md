+++
title = '08 — ENSEC-PJ · CNJ 396/2021 · Portaria 162/2021'
date = '2026-10-30'
draft = false
weight = 8
tags = ['semana-08', 'ensec', 'pj']
categories = ['estudo']
+++
# ENSEC-PJ · CNJ 396/2021 · Portaria 162/2021 · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 08 — Estratégia Nacional de Segurança Cibernética do Poder Judiciário  
**Escopo deste arquivo:** **Res. CNJ 396/2021** (ENSEC-PJ + PSEC-PJ + CGSI-PJ + ETIR/CPTRIC + estrutura SI desvinculada de TIC); **Portaria CNJ 162/2021** (protocolos PPINC / PGCRC / PIILC + manuais de referência); mapa anti-confusão com ENTIC-JUD (370), Portarias 253/131/284 e ISO/LGPD.  
**Já feito / vizinho:** segurança técnica ISO/LGPD/OWASP/firewall (semana 8 · 01–07); PDPJ/normativos CNJ (sem. 1).  
**Fora de escopo profundo:** checklist linha a linha de cada anexo da 162; implementação operacional do TRT4; MoReq/PDTIC (sem. 10).

**Edital (âncoras):** Resoluções CNJ nº **396/2021 (ENSEC-PJ)** · Portarias CNJ nº **162/2021** (entre outras).

---

## 1. Material

### 1.1 Onde cai no edital

| Bloco | Conteúdo típico | Não misturar |
|-------|-----------------|--------------|
| **Específicas — normativos** | **396** institui ENSEC-PJ; **162** aprova protocolos/manuais | 162 ≠ “outra resolução que cria a estratégia” |
| **Específicas — segurança** | ISO 27001, LGPD, OWASP, firewall… | ENSEC **alinha** a SI; não substitui a norma técnica |
| **Gerais** | quase nunca | — |

Flash: **396 = estratégia/política/governança** · **162 = instrumentos operacionais (protocolos + manuais)** · ambas **excetuam o STF**.

### 1.2 Frase-âncora da sessão (semana 8)

> Depois do bloco técnico (ISO/LGPD/OWASP/controles), a FCC fecha o eixo cobrando o **arranjo normativo do Judiciário**: a **Res. 396/2021** institui a **ENSEC-PJ** (maturidade, resiliência, governança, continuidade) e a **Portaria 162/2021** aprova os **protocolos e manuais** que materializam a Política de Segurança Cibernética (PSEC-PJ).

### 1.3 Identidade normativa (anti-confusão)

| Ato | O que é | Pegadinha |
|-----|---------|-----------|
| **Res. CNJ 396/2021** (07/06/2021) | Institui a **ENSEC-PJ** e estrutura a **PSEC-PJ**, CGSI-PJ, rede de cooperação, ETIR, estrutura SI | Não é Portaria |
| **Portaria CNJ 162/2021** (10/06/2021) | **Aprova** protocolos (Anexos I–III) e manuais (IV–VII) criados **pela** 396 | Não “institui” a ENSEC |
| **Res. CNJ 370/2021** | **ENTIC-JUD** (estratégia nacional de **TIC**) | Vizinha; 396 **revoga** arts. 39–40 da 370 |
| **Portaria 253/2020, 131/2021, 284/2021** | Outros atos PDPJ/TIC do edital | Não confundir número com “segurança = 162” |
| **ISO/IEC 27001** | Referência técnica citada nos considerandos (à época 2013; edital atual = 2024) | Alinhamento ≠ certificação automática de tribunal |
| **LGPD** | Consideranda + proteção de dados na PSEC | ENSEC ≠ LGPD |

Nomes longos (FCC troca letras):

| Sigla | Nome |
|-------|------|
| **ENSEC-PJ** | Estratégia Nacional de Segurança (da Informação e) Cibernética do PJ |
| **PSEC-PJ** | Política de Segurança Cibernética do PJ |
| **CGSI-PJ** | Comitê Gestor de Segurança da Informação do PJ (**nacional**, no CNJ) |
| **CGSI** (local) | Comitê de Governança de SI **de cada órgão** |
| **ETIR** | Equipe de Tratamento e Resposta a Incidentes (por órgão) |
| **CPTRIC-PJ** | Centro de Prevenção, Tratamento e Resposta a Incidentes Cibernéticos do PJ (CNJ) |
| **PCESC-PJ** | Política de Cultura e Educação em Segurança Cibernética |

### 1.4 Resolução 396 — âmbito e escopo (art. 1º)

**Âmbito:** órgãos do Poder Judiciário, **à exceção do STF**.

**Contempla (parágrafo único — visão de prova):**

1. SI ampla essencial à cibersegurança  
2. Segurança física + proteção de **dados pessoais e institucionais** (ângulo ciber)  
3. Segurança física e proteção de **ativos de TI**  
4. **DICÁ** — disponibilidade, integridade, confidencialidade, autenticidade  
5. Continuidade operacional (fim + administrativa)  
6. Planejamento / sistematização / normatização  
7. Comunicação, conscientização, **cultura**  
8. Formação/qualificação de profissionais de TIC em segurança

### 1.5 Objetivo, visão e objetivos cobráveis (arts. 2º–6º)

| Peça | Conteúdo |
|------|----------|
| **Objetivo-geral (art. 2º)** | Aprimorar o **nível de maturidade** em segurança cibernética |
| **Visão (art. 4º)** | Excelência em segurança cibernética no PJ |
| **Objetivos (art. 6º)** | (I) Judiciário mais **seguro e inclusivo** no digital · (II) **resiliência** às ameaças · (III) **governança** + gestão/coordenação integrada · (IV) **manutenção/continuidade** dos serviços ou restabelecimento no menor tempo |

FCC TRT-22/2022 cobrou literalmente o objetivo III amarrado ao número **396/2021** (distratores: 335, 131, 253, **162**).

### 1.6 Ações da ENSEC-PJ (art. 9º) — cartão quente

| Inciso | Ação correta | Distrator clássico |
|--------|--------------|--------------------|
| I | Fortalecer ações de **governança cibernética** | “governança **corporativa**” |
| II | Elevar segurança das **infraestruturas críticas** | “requisitos funcionais críticos” |
| III | Rede de **cooperação** do Judiciário para SI cibernética | — |
| IV | Modelo **centralizado** de governança cibernética **nacional** | “descentralizado por cartório” |

FCC TRT-17/2022: com I e II deturpados no enunciado, gabarito = **apenas III e IV**.

Desdobramentos cobráveis:

- **Art. 10:** SGSI baseado em **riscos** (recomendação CNJ).  
- **Art. 11 (infra crítica):** ETIR; processo de resposta + continuidade + comunicação; análise consolidada de logs/auditoria; threat intelligence; **backups atualizados e segregados**; requisitos para ativos (incl. notebooks/celulares); **trabalho remoto**; security by design (ex.: dupla verificação de acesso externo); testes de conformidade **ao menos semestrais**; exercícios de gestão de incidentes; troca com setor público/privado.

### 1.7 Governança nacional, CGSI-PJ e rede (arts. 12–18)

**Modelo centralizado (art. 12):** coordenar entes; analisar maturidade conjunta; padrão unificado + indicadores; rotinas de conformidade; convergência em apuração/capacitação. **CNJ coordena** (art. 13).

**CGSI-PJ (nacional — arts. 14–17):**

| Item | Flash |
|------|-------|
| Função | Assessorar o CNJ em SI |
| Composição | CNJ (2), STF (2), STJ, TSE, TST, CSJT, CJF, STM, TJs (2) — especialistas |
| Coordenação | Representante do **CNJ** designado pela Presidência |
| Reuniões | Ordinárias **semestrais**; extraordinárias por convocação |
| Competências-chave | Normas de gestão de risco; aprovar políticas/diretrizes; programas de conscientização; critérios de maturidade; norma do **CPTRIC-PJ**; troca com outros Poderes |

**Rede de cooperação (art. 18):** compartilhar incidentes/vulnerabilidades; exercícios multi-entes; fortalecer CPTRIC; incentivar **ETIR** em cada órgão; alertas; parcerias Executivo/Legislativo/MP/polícia/privado/academia.  
**Obrigação:** todo órgão que detectar incidente deve **reportar ao CPTRIC-PJ**.

### 1.8 Alta administração, CGSI local, estrutura SI e gestor (arts. 19–21)

| Peça | Obrigação cobrável |
|------|--------------------|
| **Alta administração** | Implementar PSEC no que couber; elaborar PSI interna; **orçamento específico**; capacitação; instituir **ETIR** (rede do CPTRIC); coordenar SI; sanções disciplinares |
| **CGSI (local)** | Assessorar alta administração; propor alterações/PSI; normas internas; GTs; consolidar auditorias de gestão de SI. Coordenado pela autoridade de SI nomeada pelo **presidente** do órgão |
| **Estrutura de SI** | Subordinada **diretamente à alta administração** e **desvinculada da área de TIC** |
| **Gestor de SI** | Titular da estrutura: SGSI, controles por risco, planejamento transversal, procedimento de incidentes, observância das normas |

Pegadinha: SI “dentro da STI como subsetor informal” **fere** o arranjo da 396.

### 1.9 PSEC-PJ — princípios, objetivos, instrumentos (arts. 22–28)

**Princípios (amostra cobrável):** segurança jurídica; DH/garantias (liberdade de expressão, dados, privacidade, acesso à informação); visão sistêmica; cooperação; educação/inovação/cultura; gestão de riscos; prevenção/tratamento/resposta; articulação ciber × proteção de dados/ativos; sigilo necessário + inviolabilidade da vida privada.

**Instrumentos da PSEC (art. 25):**

1. **ENSEC-PJ**  
2. **PPINC-PJ** — Prevenção de Incidentes  
3. **PGCRC-PJ** / PGCC — Gerenciamento de **Crises** Cibernéticas  
4. **PIILC-PJ** — Investigação de **Ilícitos** Cibernéticos  

(+ manuais de referência por ato do Presidente do CNJ — é aí que entra a **162**).

Lógica operacional (art. 26):

| Protocolo | Quando |
|-----------|--------|
| **PPINC** | Prevenção em alto nível |
| **PGCRC** | Complementa o PPINC quando o incidente **não** é mitigado rápido / pode durar tempo indeterminado → **crise**; Comitê de Crise |
| **PIILC** | Coleta/preservação de evidências + comunicação a órgãos de persecução |

PSI local deve prever (art. 28): gestão de ativos + controle de acesso; informações com restrição; treinamento contínuo/certificação internacional dos profissionais de ciber; requisitos mínimos em contratações/acordos; criptografia/assinatura eletrônica; articulação com a alta administração.

### 1.10 Gestão de usuários, cultura, orçamento (arts. 29–31)

- Gestão de usuários = **identidades + acessos + privilégios** (padrão por ato do Presidente — login único / UX uniforme).  
- **PCESC-PJ** — cultura e educação (ato do Presidente).  
- Orçamento: recursos necessários em **rubrica específica** (transparência para a governança nacional).

### 1.11 Portaria CNJ 162/2021 — o que aprova

| Anexo | Conteúdo |
|-------|----------|
| **I** | **PPINC-PJ** — Prevenção de Incidentes |
| **II** | **PGCRC-PJ** — Gerenciamento de Crises |
| **III** | **PIILC-PJ** — Investigação de Ilícitos |
| **IV** | Manual — Proteção de **Infraestruturas Críticas** de TIC |
| **V** | Manual — Prevenção/Mitigação de Ameaças + Confiança Digital |
| **VI** | Manual — **Gestão de Identidades** |
| **VII** | Manual — Política de **Educação e Cultura** em SI cibernética |

Implementação: **todos os órgãos do PJ, exceto STF**. Atualizáveis por indicação do Comitê Gestor. Na prática dos tribunais: planos de ação para implantar 162 alinhados à PSI/ENSEC.

### 1.12 Engajamento da alta administração (art. 8º, parágrafo único)

Essencial — inclusive para **suspender rapidamente o acesso ao público** e conter alastramento de ataque. FCC gosta da ideia: decisão de contenção **não** é só “decisão do analista de rede”.

### 1.13 Revogações e vizinhança

396 revoga Res. **360, 361 e 362/2020** e os arts. **39 e 40** da Res. **370/2021** (ENTIC). Em prova: ENSEC é o marco cibernético “de casa”; ENTIC continua como estratégia de TIC — papéis distintos.

### 1.14 Cartão final

> **396** institui ENSEC/PSEC/CGSI-PJ/ETIR/estrutura SI fora da TIC · objetivos: seguro-inclusivo + resiliência + governança + continuidade · ações: governança cibernética + infra crítica + cooperação + modelo nacional · **162** aprova PPINC + PGCRC + PIILC + 4 manuais · **exceto STF** · reportar incidentes ao **CPTRIC** · não trocar 396↔162↔370↔ISO↔LGPD.

---

## 2. Questões de treino (1–15)

**1.** A ENSEC-PJ foi instituída pela:

A) Resolução CNJ 396/2021  
B) Portaria CNJ 162/2021  
C) Resolução CNJ 400/2021  
D) Resolução CNJ 492/2023  
E) Portaria CNJ 253/2020  

**2.** A Portaria CNJ 162/2021, em relação à ENSEC-PJ:

A) institui a estratégia no lugar da 396  
B) aprova protocolos e manuais previstos/criados no arranjo da 396  
C) revoga a LGPD  
D) cria o STF  
E) substitui a ISO 27001  

**3.** Excetuado expressamente do âmbito da 396/162:

A) TRT4  
B) TJ de Estado  
C) STJ  
D) STF  
E) CSJT  

**4.** Objetivo-geral da ENSEC (art. 2º):

A) aprimorar o nível de maturidade em segurança cibernética  
B) abolir microsserviços  
C) substituir o PDTIC  
D) tornar opcional o backup  
E) extinguir o CGSI  

**5.** NÃO é objetivo do art. 6º:

A) resiliência às ameaças  
B) governança e coordenação integrada  
C) continuidade/restabelecimento rápido  
D) Judiciário mais seguro e inclusivo no digital  
E) exclusividade de software proprietário em todo o PJ  

**6.** Ação correta do art. 9º:

A) fortalecer governança **corporativa** (texto literal do inciso I)  
B) estabelecer rede de cooperação do Judiciário para a segurança cibernética  
C) elevar segurança dos **requisitos funcionais críticos**  
D) extinguir o CPTRIC  
E) proibir ETIR  

**7.** Sobre a estrutura de SI no órgão (art. 21):

A) subordinada diretamente à alta administração e desvinculada da área de TIC  
B) deve ficar informalmente dentro da STI, sem vínculo com a alta administração  
C) é facultativa se houver firewall  
D) só existe no STF  
E) substitui o presidente do tribunal  

**8.** ETIR é:

A) protocolo de redação oficial  
B) sinônimo de LGPD  
C) manual de identidade visual  
D) Equipe de Tratamento e Resposta a Incidentes de Segurança Cibernética  
E) comitê só de sustentabilidade  

**9.** Incidentes detectados pelos órgãos devem ser reportados ao:

A) Cartório Eleitoral municipal  
B) INSS  
C) Bacen exclusivamente  
D) CPTRIC-PJ  
E) cartório de registro civil  

**10.** Instrumentos da PSEC-PJ incluem:

A) ENSEC + PPINC + protocolo de crises + PIILC  
B) apenas o Scrum Guide  
C) somente a DUDH  
D) apenas o Regimento do TRT4  
E) só o OWASP Top 10  

**11.** PGCRC/PGCC complementa o PPINC quando:

A) não há nenhum risco  
B) o tribunal troca de logo  
C) a prova objetiva termina  
D) se publica o edital  
E) o incidente não será mitigado rapidamente e pode durar tempo indeterminado (crise)  

**12.** Reuniões ordinárias do CGSI-PJ (nacional):

A) diárias  
B) semestrais  
C) mensais obrigatórias sem exceção  
D) a cada 10 anos  
E) nunca  

**13.** Avaliação/testes de conformidade em segurança cibernética (art. 11):

A) ao menos semestralmente  
B) nunca  
C) apenas na posse do presidente  
D) só após extinção do tribunal  
E) somente em papel sem TI  

**14.** Gestão de usuários na 396 compreende:

A) só impressão de crachá  
B) apenas Wi-Fi de visitante  
C) só e-mail pessoal  
D) identidades, acessos e privilégios  
E) exclusão de MFA  

**15.** Confundir 396 com 162 é erro porque:

A) 396 institui a estratégia/política; 162 aprova protocolos/manuais de implementação  
B) ambas são ISO  
C) 162 é ENTIC-JUD  
D) 396 é só Portaria  
E) nenhuma se aplica ao PJ  

---

## 3. Questões de concurso FCC (16–25)

**16.** (FCC – TRT-22/2022 – Analista Judiciário – TI)  
Estabelecer governança de segurança cibernética e fortalecer a gestão e coordenação integrada de ações de segurança cibernética nos órgãos do Poder Judiciário é um dos objetivos da Estratégia Nacional de Segurança da Informação e Cibernética do Poder Judiciário (ENSEC-PJ), que consta na Resolução nº

A) 335/2020.  
B) 131/2021.  
C) 253/2020.  
D) 162/2021.  
E) 396/2021.

**17.** (FCC – TRT-17/2022 – Analista Judiciário – TI)  
A Resolução CNJ nº 396/2021, em seu Art. 9º, define que são ações da ENSEC-PJ: I. Fortalecer as ações de governança corporativa. II. Elevar o nível de segurança dos requisitos funcionais críticos. III. Estabelecer rede de cooperação do Judiciário para a segurança cibernética. IV. Estabelecer modelo centralizado de governança cibernética nacional. Está correto o que se afirma APENAS em

A) II e III.  
B) I e II.  
C) III e IV.  
D) II e IV.  
E) I e III.

**18.** (FCC – TRT-5/2022 – Analista Judiciário – TI)  
Considere o contexto da Resolução nº 396 de 07/06/2021. A ENSEC-PJ prevista nesta Resolução contempla:  
I. temas relacionados à segurança da informação, de forma ampla, que sejam essenciais para segurança cibernética.  
II. segurança lógica e proteção de dados específicos do nível operacional, nos aspectos relacionados aos requisitos funcionais.  
III. segurança física e proteção de ativos de tecnologia da informação de forma geral;  
IV. ações destinadas a assegurar, de forma automatizada, a não invasão física de localidades e de informações.  
Está correto o que consta APENAS de

A) I e II.  
B) I e III.  
C) II, III e IV.  
D) III.  
E) II e IV.

**19.** (FCC – TRT-5/2022 – Técnico Judiciário – TI)  
Considere a Resolução nº 396 de 07/06/2021. No seu Art. 18 ela estabelece que a Rede de Cooperação do Judiciário na área de segurança cibernética tem, dentre outros, os seguintes objetivos:  
I. promover ambiente participativo, colaborativo e seguro entre os órgãos do Poder Judiciário e, por meio exclusivo de auditorias independentes, avaliar as ameaças e os ataques cibernéticos;  
II. estimular a centralização e sigilo de informações sobre incidentes e vulnerabilidades cibernéticas;  
III. realizar exercícios cibernéticos com a participação de múltiplos entes;  
IV. fortalecer o Centro de Prevenção, Tratamento e Resposta a Incidentes Cibernéticos (CPTRIC-PJ) do CNJ;  
Está correto o que consta APENAS de

A) I e II.  
B) II e III.  
C) I, III e IV.  
D) II, III e IV.  
E) III e IV.

**20.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
A Resolução CNJ nº 396/2021, Capítulo VI, art. 18, a Rede de Cooperação tem os objetivos listados na norma e, também,

A) Ampliar parceria com outros órgãos do Poder Executivo, do Poder Legislativo, do Ministério Público, da polícia judiciária, do setor privado e do meio acadêmico, com vistas a elevar, de modo geral, o nível de segurança cibernética.  
B) Ampliar ações destinadas a assegurar a disponibilidade, a integridade, a confidencialidade e a autenticidade de dados e de informações.  
C) Executar ações de comunicação, de conscientização, de formação de cultura e de direcionamento institucional com vistas à segurança cibernética.  
D) Preservar a segurança física e proteção de dados pessoais e institucionais, nos aspectos relacionados à cibersegurança.  
E) Aprimorar o nível de maturidade em segurança cibernética nos órgãos do Poder Judiciário, abrangendo os aspectos fundamentais da segurança da informação para o aperfeiçoamento necessário à consecução desse propósito.

**21.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Segundo a Resolução CNJ nº 396/2021, cada Tribunal, com exceção do STF, deverá estabelecer em sua Política de Segurança da Informação ações para: realizar a Gestão dos Ativos de Informação e da Política de Controle de Acesso; criar controles para o tratamento de informações com restrição de acesso; promover treinamento contínuo e certificação internacional dos profissionais diretamente envolvidos na área de segurança cibernética; estabelecer requisitos mínimos de segurança cibernética nas contratações e nos acordos que envolvam a comunicação com outros órgãos; utilizar os recursos de soluções de criptografia, ampliando o uso de assinatura eletrônica, conforme legislações específicas; e

A) tratar riscos de segurança da informação de forma a garantir que as informações dos órgãos do Poder Judiciário sejam mantidas totalmente seguras.  
B) garantir a melhoria contínua dos métodos e procedimentos no que tange à segurança física e lógica dos ambientes de tecnologia do Poder Judiciário.  
C) assegurar o projeto e desenvolvimento de softwares seguros que garantam a segurança das informações dos órgãos do Poder Judiciário.  
D) garantir a segregação de funções no controle de acesso a todos os sistemas do Poder Judiciário.  
E) comunicar e articular as ações de segurança da informação com a alta administração do órgão.

**22.** (FCC – TRF4/2025 – Analista Judiciário – Análise de Sistemas de Informação)  
Considere o teor da Portaria CNJ nº 162/2021: “5.2 A ETIR poderá solicitar apoio multidisciplinar para responder aos incidentes de segurança de maneira adequada e tempestiva,”

A) em áreas como: tecnologia da informação, segurança da informação, jurídica, pesquisas judiciárias, comunicação, controle interno, segurança institucional, entre outras.  
B) ao setor de segurança pública das três esferas administrativas governamentais, quando cabível e à área de comunicação federal quando o incidente contar com o risco de alastramento a nível país.  
C) ao setor de segurança da informação, excetuando-se tecnologia da informação e comunicação, quando cabível, e à área de comunicação federal quando o incidente contar com o risco de alastramento a nível país.  
D) em áreas como: gestão de dados em nuvem, segurança corporativa, administrativo-financeiras, excetuando-se gerência de materiais, tecnologia da informação e segurança institucional que por si só já atuam nesse mister.  
E) ao setor de segurança pública das três esferas administrativas governamentais e dos três poderes, quando cabível, e à área de comunicação federal quando o incidente contar com o risco de alastramento a nível país.

**23.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
A Portaria CNJ nº 162/2021, dentre seus princípios críticos, estabelece que o protocolo de prevenção a incidentes cibernéticos… O princípio base de conhecimento de defesa tem por propósito

A) resistir aos efeitos de um incidente, bem como impedir a reincidência secundária do incidente identificado.  
B) fornecer bases fundamentais ao aprendizado contínuo com apoio em eventos ocorridos.  
C) definir métricas comuns que forneçam linguagem compartilhada e de compreensão abrangente para magistrados e demais atores do sistema de segurança.  
D) incentivar a busca de soluções automatizadas de segurança cibernética para que as organizações obtenham medições confiáveis, escaláveis e contínuas.  
E) contribuir para a definição de prioridades e para os próximos passos a serem tomados.

**24.** (FCC – TRT-23/2022 – Técnico Judiciário – TI)  
Observe a figura abaixo, que propõe o uso de diversos sistemas de proteção para a rede do Tribunal Regional do Trabalho.  
Para garantir uma maior proteção à rede, I, II, III e IV são, correta e respectivamente:

A) proxy direto – WAF – UTM – proxy reverso  
B) proxy direto – proxy reverso – DMZ – WAF  
C) IPS – IDS – dual firewall – DMZ  
D) IDS – IPS – firewall – WAF  
E) IDS – proxy reverso – WAF – UTM

**25.** (FCC – TRT-22/2022 – Analista Judiciário – TI)  
Considere que uma pessoa, detentora de um segredo:  
I. Garante que ele está conforme seu conteúdo original.  
II. Garante a um grupo o acesso sempre que quiserem.  
III. Autoriza somente um grupo em determinado local.  
I, II e III correspondem, respectivamente, a

A) integridade, não repúdio e disponibilidade.  
B) autenticidade, não repúdio e confidencialidade.  
C) autenticidade, disponibilidade e autoridade.  
D) integridade, disponibilidade e confidencialidade.  
E) não repúdio, autenticidade e autoridade.

---

### Gabarito comentado (treino 1–15)

1. **A** — Res. 396 institui.  
2. **B** — 162 aprova protocolos/manuais.  
3. **D** — exceto STF.  
4. **A** — maturidade.  
5. **E** — não é objetivo.  
6. **B** — inciso III literal.  
7. **A** — SI fora da TIC, sob alta administração.  
8. **D** — equipe de resposta.  
9. **D** — CPTRIC-PJ.  
10. **A** — art. 25.  
11. **E** — crise cibernética.  
12. **B** — semestral.  
13. **A** — ao menos semestral.  
14. **D** — IAM tríade.  
15. **A** — papéis distintos 396×162.

### Gabarito (concurso 16–25)

| Q | Gab | Nota |
|---|-----|------|
| 16 | **E** | FCC TRT-22/2022 — ENSEC = 396 |
| 17 | **C** | FCC TRT-17/2022 — III e IV |
| 18 | **B** | FCC TRT-5/2022 — I e III |
| 19 | **E** | FCC TRT-5/2022 — III e IV |
| 20 | **A** | FCC TRT-12/2023 — parceria ampliada |
| 21 | **E** | FCC TRT-19/2022 — articular com alta administração |
| 22 | **A** | FCC TRF4/2025 — ETIR multidisciplinar |
| 23 | **B** | FCC TRT-20/2024 — conhecimento de defesa |
| 24 | **E** | FCC TRT-23/2022 — IDS / proxy reverso / WAF / UTM |
| 25 | **D** | FCC TRT-22/2022 — I/D/C |

---

## 4. Gabarito consolidado

| Q | Resp | Q | Resp | Q | Resp | Q | Resp | Q | Resp |
|---|------|---|------|---|------|---|------|---|------|
| 1 | A | 2 | B | 3 | D | 4 | A | 5 | E |
| 6 | B | 7 | A | 8 | D | 9 | D | 10 | A |
| 11 | E | 12 | B | 13 | A | 14 | D | 15 | A |
| 16 | E | 17 | C | 18 | B | 19 | E | 20 | A |
| 21 | E | 22 | A | 23 | B | 24 | E | 25 | D |

Distribuição concurso 16–25 (A–E): A=2 · B=2 · C=1 · D=1 · E=4.

---

## 5. Checklist rápida (casa)

- [ ] 396 institui · 162 aprova · exceto STF  
- [ ] Objetivos art. 6º · ações art. 9º (sem “corporativa” / “requisitos funcionais”)  
- [ ] SI desvinculada de TIC · ETIR · reportar CPTRIC  
- [ ] PPINC → prevenção · PGCRC → crise · PIILC → ilícitos  
- [ ] Não misturar 370 (ENTIC), ISO, LGPD, Portarias 253/131/284  

---

*Arquivo 08 · ENSEC-PJ · CNJ 396/2021 · Portaria 162/2021 · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 8.*

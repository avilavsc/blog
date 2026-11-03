+++
title = '02 — LGPD (Lei nº 13.709/2018)'
date = '2026-10-24'
draft = false
weight = 2
tags = ['semana-08', 'lgpd']
categories = ['estudo']
+++
# LGPD (Lei nº 13.709/2018) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Segurança da informação / proteção de dados  
**Escopo deste arquivo:** Lei Geral de Proteção de Dados Pessoais (**Lei nº 13.709/2018**): conceitos (dado pessoal, sensível, anonimizado), princípios, bases legais (arts. 7 e 11), agentes (controlador/operador), encarregado, direitos do titular, tratamento pelo poder público, crianças/adolescentes, segurança e incidentes, RIPD, transferência internacional, ANPD e sanções.  
**Fora de escopo aqui:** ISO 27001/27002/27701 a fundo (arquivo 01 da semana); OWASP Top 10:2025 (arquivo 03); firewall/WAF/IDS/VPN/cripto (arquivos seguintes); ENSEC-PJ / Portaria 162 (normativo de segurança do PJ — pasta própria da semana).

**Edital (âncora):** Segurança da informação → **LGPD (Lei nº 13.709/2018)**.

---

## 1. Material

### 1.1 Onde cai e por que a FCC cobra “no fio”

No código 14 a LGPD está em **Específicas → Segurança da informação**, mas a banca já cobrou o texto legal **em bloco** (ex.: TRT4/2022 TI com dezenas de itens de LGPD em outras provas da mesma família; TRT-23/2022 TI; TRF4/2025 Análise de Sistemas). O padrão FCC:

| Tipo de item | O que testa |
|--------------|-------------|
| Definição trocada | controlador × operador × encarregado × titular × ANPD |
| Base legal errada | consentimento como “única” base; legítimo interesse em dado **sensível** |
| Prazo/percentual | multa 2% / R$ 50 mi; suspensão **6 meses prorrogável por igual período** |
| Direito do titular | o que **não** está no art. 18 (ex.: “anonimização sob requisição” como direito genérico) |
| Poder público | hipóteses do art. 23 sem “consentimento obrigatório” |

Flash: **LGPD ≠ LAI** (Lei de Acesso à Informação) e **≠ Marco Civil**. Em TI do Judiciário, tratamento processual costuma ancorar em **obrigação legal / exercício regular de direitos / política pública**, não em “pedir consentimento do jurisdicionado para cada ato do processo”.

### 1.2 Frase-âncora

> A LGPD disciplina o **tratamento** de **dados pessoais** (identificados ou identificáveis) por pessoa natural ou jurídica de direito público ou privado, com o objetivo de proteger os direitos fundamentais de liberdade, privacidade e o livre desenvolvimento da personalidade — sob princípios, bases legais e fiscalização da **ANPD**.

### 1.3 Conceitos do art. 5º (cartão cobrável)

| Termo | Núcleo FCC |
|-------|------------|
| **Dado pessoal** | Informação relacionada a pessoa natural **identificada** ou **identificável** |
| **Dado pessoal sensível** | Origem racial/étnica; convicção religiosa; opinião política; filiação a sindicato ou organização religiosa/filosófica/política; saúde; vida sexual; **genético** ou **biométrico** vinculado a pessoa natural |
| **Dado anonimizado** | Titular **não** identificável com meios técnicos **razoáveis** disponíveis; em regra **não** é dado pessoal (salvo reversão) |
| **Titular** | Pessoa natural a quem se referem os dados |
| **Controlador** | Quem **decide** referentes ao tratamento (finalidades/meios essenciais) |
| **Operador** | Quem realiza o tratamento **em nome** do controlador (segue instruções — art. 39) |
| **Encarregado** | Pessoa indicada pelo **controlador** para atuar como canal de comunicação entre controlador, titulares e ANPD (art. 5º, VIII / art. 37) |
| **Tratamento** | Toda operação: coleta, produção, recepção, classificação, utilização, acesso, reprodução, transmissão, distribuição, processamento, arquivamento, armazenamento, eliminação, avaliação, controle, modificação, comunicação, transferência, difusão, extração… |
| **Consentimento** | Manifestação **livre, informada e inequívoca** para finalidade **determinada** |
| **Bloqueio** | Suspensão **temporária** do tratamento, com **guarda** do dado |
| **Eliminação** | Exclusão do dado/banco, qualquer procedimento |
| **RIPD** | Documentação do controlador: processos que podem gerar risco a liberdades/direitos + medidas de mitigação (art. 5º, XVII; art. 38) |
| **ANPD** | Autoridade Nacional de Proteção de Dados — órgão da administração pública federal, fiscaliza e aplica sanções administrativas da LGPD |

**Anonimização × pseudonimização (pegadinha):** anonimização **remove** (de forma razoável) a possibilidade de associação ao indivíduo; pseudonimização **ainda** permite reidentificação com informação adicional mantida separadamente — continua sob regime de dado pessoal.

### 1.4 Princípios (art. 6º) — mapa anti-confusão

Memorize o **rótulo + 1 frase**. A FCC troca rótulos.

| Princípio | Ideia |
|-----------|-------|
| **Finalidade** | Propósitos legítimos, específicos, explícitos; sem tratamento incompatível |
| **Adequação** | Compatibilidade com as finalidades informadas |
| **Necessidade** | Mínimo necessário (minimização) |
| **Livre acesso** | Consulta facilitada e gratuita sobre forma e duração do tratamento |
| **Qualidade dos dados** | Exatidão, clareza, relevância, atualização |
| **Transparência** | Informações claras, precisas e facilmente acessíveis |
| **Segurança** | Medidas técnicas/administrativas contra acessos não autorizados e situações acidentais/ilícitas |
| **Prevenção** | Medidas para prevenir danos |
| **Não discriminação** | Vedação a fins discriminatórios ilícitos ou abusivos |
| **Responsabilização e prestação de contas** (*accountability*) | Demonstrar eficácia das medidas e cumprimento da lei |

Erro clássico: confundir **necessidade** (minimização) com **adequação** (compatibilidade com a finalidade).

### 1.5 Bases legais — dados pessoais “comuns” (art. 7º)

Consentimento **não** é a única base. Rol cobrável (visão de prova):

1. Consentimento do titular  
2. Cumprimento de **obrigação legal ou regulatória** pelo controlador  
3. Execução de **políticas públicas** (admin. pública — lei/regulamento)  
4. Estudos por órgão de pesquisa (anonimização quando possível)  
5. Execução de **contrato** ou procedimentos preliminares (a pedido do titular)  
6. Exercício regular de direitos em processo **judicial, administrativo ou arbitral**  
7. Proteção da **vida** ou incolumidade física do titular/terceiro  
8. Tutela da **saúde** (profissional/serviço de saúde / autoridade sanitária — redação atual)  
9. **Legítimo interesse** do controlador ou de terceiro (com salvaguardas; art. 10)  
10. Proteção ao **crédito** (disposições pertinentes)

**Poder Judiciário / TRT:** cadastros processuais, intimação, cumprimento de decisões e interoperabilidade (ex.: PDPJ) tipicamente ancoram em obrigação legal / exercício regular de direitos / política pública — **não** “consentimento genérico do advogado no login”.

### 1.6 Dados sensíveis (art. 11) — o que **some** do rol

Para sensíveis: consentimento **específico e destacado** **ou** hipóteses **indispensáveis** (obrigação legal; política pública; pesquisa; exercício regular de direitos; vida; tutela da saúde; prevenção à fraude/segurança do titular em autenticação eletrônica — com ressalvas).

**Não entram** no art. 11 (distratores favoritos):

| Base do art. 7º | Em dado sensível? |
|-----------------|-------------------|
| Legítimo interesse | **Não** |
| Proteção ao crédito | **Não** |
| Execução de contrato (como base autônoma tipicamente cobrada) | **Não** no rol do 11 |

Biometria no ponto eletrônico / controle de acesso: frequentemente discute-se prevenção à fraude/segurança do titular + necessidade + RIPD — a prova gosta de exigir **base correta** e **medida de segurança**, não “biometria = sempre ilícita”.

### 1.7 Agentes × encarregado (anti-troca de papéis)

```
Titular ──reclama/consulta──► Encarregado ◄──canal──► ANPD
                                 ▲
Controlador (decide) ──instruções──► Operador (executa)
```

| Afirmação | Verdade |
|-----------|---------|
| Agentes de tratamento = controlador + operador | Sim (art. 5º) |
| Encarregado é agente de tratamento | **Não** |
| Encarregado é indicado pelo **titular** | **Não** — pelo **controlador** (art. 37) |
| Operador define a finalidade final | **Não** — segue o controlador |
| Operador responde se descumprir instruções/lei | Pode haver responsabilidade (arts. 42+) |

Atividades típicas do encarregado (art. 41): aceitar reclamações/comunicações dos titulares; prestar esclarecimentos; receber comunicações da ANPD; orientar funcionários/contratados; demais atribuições do controlador ou de normas complementares.

### 1.8 Direitos do titular (art. 18) — lista quente

O titular pode obter do controlador, a qualquer momento e mediante requisição, entre outros:

- confirmação da existência de tratamento  
- acesso aos dados  
- correção de dados incompletos/inexatos/desatualizados  
- anonimização, bloqueio ou eliminação de dados **desnecessários, excessivos ou tratados em desconformidade**  
- portabilidade (observada regulamentação da ANPD)  
- eliminação dos dados tratados com consentimento (com ressalvas legais de conservação)  
- informação sobre entidades públicas/privadas com as quais o controlador compartilhou dados  
- informação sobre a possibilidade de não consentir e consequências  
- **revogação** do consentimento  

**Pegadinha FCC (TRT4/2022 Técnico — linha art. 18):** “anonimização” **como direito absoluto genérico** (item solto “II. Anonimização de dados”) **não** equivale a exigir anonimização de qualquer dado a qualquer tempo; o art. 18 amarra anonimização/bloqueio/eliminação a dados desnecessários/excessivos/em desconformidade. Em prova de múltipla escolha com “I, III e IV apenas”, o distrator costuma ser o item que inventa direito amplo de anonimização.

### 1.9 Crianças e adolescentes (art. 14)

- Interesse superior da criança/adolescente  
- Tratamento com consentimento **específico e em destaque** dado por **pelo menos um** dos pais ou responsável legal (regra; há hipóteses de coleta sem consentimento para contatar pais/responsável, **sem** armazenamento além do necessário e **sem** repasse a terceiro sem consentimento)  
- Informações em linguagem clara, acessível  

Distrator clássico FCC: “consentimento dos **dois** pais obrigatório” / “consentimento genérico vale”.

### 1.10 Segurança, incidentes e RIPD

**Art. 46:** agentes devem adotar medidas de segurança, técnicas e administrativas aptas a proteger dados de acessos não autorizados e de situações acidentais ou ilícitas (destruição, perda, alteração, comunicação, difusão).

**Art. 48:** o controlador deve comunicar à ANPD e ao titular a ocorrência de incidente de segurança que possa acarretar risco ou dano relevante — comunicação em prazo razoável, com descrição, riscos, medidas… (detalhes regulamentáveis pela ANPD).

**RIPD (art. 38):** autoridade nacional pode determinar ao controlador a elaboração de relatório de impacto, inclusive para dados sensíveis — descrição dos tipos de dados, metodologia, salvaguardas/mitigações.

**Privacy by design / by default:** lógica cobrável — proteção desde a concepção e por padrão (mínimo necessário), alinhada aos princípios de necessidade/segurança/prevenção (a lei dialoga com boas práticas; FCC pode misturar com ISO 27701 na mesma prova — **não** confundir “princípio LGPD” com “controle ISO”).

### 1.11 Tratamento pelo poder público (visão)

Órgãos públicos tratam para execução de competências legais/políticas públicas, com transparência (hipóteses, finalidade) e vedações a uso incompatível. Transferência a entidades privadas exige hipóteses legais. Hipótese de dispensa de consentimento **não** dispensa princípios (finalidade, necessidade, segurança, accountability).

### 1.12 Transferência internacional (art. 33 — flash)

Permitida, entre outras, quando: país/organismo com nível **adequado**; garantias (cláusulas específicas/padrão, BCRs, selos/certificados); hipóteses de cooperação/investigação; proteção da vida; autorização ANPD; consentimento específico e em destaque; obrigação legal; execução de contrato; exercício regular de direitos; política pública etc. **Não** é “proibida sempre” nem “livre se houver VPN”.

### 1.13 ANPD e sanções (art. 52) — números que caem

Sanções administrativas **pela ANPD** (rol):

- advertência (com prazo corretivo)  
- multa simples: até **2%** do faturamento no Brasil (excluídos tributos), limitada a **R$ 50.000.000,00** por infração  
- multa diária (mesmo teto)  
- publicização da infração  
- bloqueio dos dados até regularização  
- eliminação dos dados a que se refere a infração  
- suspensão **parcial** do funcionamento do banco: máx. **6 meses**, prorrogável por igual período  
- suspensão do exercício da atividade de tratamento: máx. **6 meses**, prorrogável por igual período  
- proibição parcial ou total de atividades relacionadas a tratamento  

**FCC TRT-23/2022 TI (gabarito A):** suspensão do exercício da atividade de tratamento → **6 meses, prorrogável por igual período** (não “12 meses” nem “improrrogável”).

Sanções da LGPD **não excluem** sanções civis/penais/CDC em legislação própria. Critérios de dosimetria: gravidade, boa-fé, vantagem, condição econômica, reincidência, grau do dano, cooperação, boas práticas, prontidão corretiva etc.

### 1.14 Mapa anti-confusão (semana 8)

| Tema | Pasta mental |
|------|--------------|
| LGPD | Este arquivo — lei de proteção de dados |
| ISO 27001/27002/27701 | SGSI / controles / PII — **não** substituem bases legais |
| OWASP Top 10:2025 | Riscos de **aplicação web** (arquivo 03) |
| ENSEC-PJ (CNJ 396) | Estratégia de segurança cibernética do PJ |
| LAI | Acesso a informação pública — outro eixo |

### 1.15 Checklist FCC (1 minuto antes da prova)

1. Controlador decide · Operador executa · Encarregado comunica · ANPD fiscaliza  
2. Consentimento ≠ única base · Sensível ≠ legítimo interesse  
3. Multa 2% / R$ 50 mi · Suspensão tratamento: 6 + 6  
4. Art. 18: confirme o que **está** e o que é distrator  
5. Criança: consentimento específico/destaque do responsável (regra)  
6. Incidente relevante → comunicar ANPD e titular  
7. Judiciário: obrigação legal / processo / política pública > “consentimento genérico”

---

## 2. Questões de treino (1–15)

**1.** Na LGPD, agente de tratamento que toma as decisões referentes ao tratamento de dados pessoais é o:

A) controlador  
B) operador  
C) titular  
D) encarregado  
E) auditor interno obrigatório da ANPD  

**2.** Dado biométrico vinculado a pessoa natural, na LGPD, classifica-se tipicamente como:

A) dado anonimizado  
B) dado pessoal sensível  
C) dado público irrestrito  
D) segredo de justiça automático  
E) metadado fora da lei  

**3.** Consentimento, na definição legal, deve ser manifestação:

A) tácita e genérica para qualquer finalidade futura  
B) livre, informada e inequívoca, para finalidade determinada  
C) exclusiva dos pais, mesmo para adultos  
D) presumida pelo silêncio do titular  
E) válida só se registrada em cartório  

**4.** Base legal **não** prevista no art. 11 para dados sensíveis é:

A) consentimento específico e destacado  
B) cumprimento de obrigação legal ou regulatória  
C) proteção da vida ou incolumidade física  
D) exercício regular de direitos em processo judicial  
E) legítimo interesse do controlador  

**5.** O operador, em regra:

A) define sozinho as finalidades finais do tratamento  
B) aplica multas da ANPD  
C) substitui o titular no exercício dos direitos do art. 18  
D) realiza o tratamento em nome do controlador, segundo suas instruções  
E) é sinônimo de encarregado  

**6.** O encarregado:

A) é agente de tratamento junto com o titular  
B) é indicado pelo titular dos dados  
C) julga ações penais por vazamento  
D) dispensa o controlador de bases legais  
E) atua como canal de comunicação entre controlador, titulares e ANPD  

**7.** Princípio da necessidade aproxima-se de:

A) maximizar a coleta “por precaução”  
B) limitar o tratamento ao mínimo necessário para a finalidade  
C) publicar todos os dados em portal aberto  
D) vedar qualquer medida de segurança  
E) tornar o consentimento a única base  

**8.** Multa simples prevista na LGPD para pessoa jurídica de direito privado limita-se, por infração, a:

A) 2% do faturamento no Brasil (excluídos tributos), até R$ 50.000.000,00  
B) 10% do faturamento global, sem teto em reais  
C) R$ 500.000,00 fixos  
D) 5% do faturamento, até R$ 5.000.000,00  
E) apenas advertência, nunca multa  

**9.** Suspensão do exercício da atividade de tratamento dos dados a que se refere a infração, na LGPD, tem período máximo de:

A) 30 dias, improrrogável  
B) 3 meses, prorrogável por 12 meses  
C) 12 meses, prorrogável por 24 meses  
D) 6 meses, prorrogável por igual período  
E) 1 ano, prorrogável por 6 meses  

**10.** Dados anonimizados, em regra, para fins da LGPD:

A) continuam sempre dados pessoais  
B) não são considerados dados pessoais, salvo reversão do processo de anonimização  
C) equivalem a dados sensíveis  
D) exigem consentimento específico e destacado  
E) só existem no setor privado  

**11.** Comunicação de incidente de segurança que possa acarretar risco ou dano relevante cabe, em regra, ao:

A) controlador, perante a ANPD e o titular  
B) operador, sem ciência do controlador  
C) titular exclusivamente  
D) Ministério Público, apenas  
E) encarregado, sem envolvimento do controlador  

**12.** No tratamento de dados de crianças, a regra geral de consentimento exige:

A) consentimento genérico da criança maior de 8 anos  
B) consentimento específico e em destaque de pelo menos um dos pais ou do responsável legal  
C) dispensa total de base legal  
D) autorização prévia da ANPD em todo caso  
E) consentimento obrigatório de ambos os pais em cartório  

**13.** RIPD é documentação que:

A) substitui a ANPD  
B) descreve tratamentos que podem gerar riscos a liberdades/direitos e medidas de mitigação  
C) é elaborada exclusivamente pelo titular  
D) se confunde com o edital do concurso  
E) elimina a necessidade de base legal  

**14.** Par correto:

A) ANPD — agente de tratamento que decide finalidades  
B) operador — autoridade sancionadora  
C) encarregado — aplica multa de 2%  
D) controlador — decisões referentes ao tratamento  
E) titular — indica o encarregado por lei  

**15.** Misturar LGPD com OWASP Top 10 é erro porque:

A) ambas são leis federais brasileiras  
B) LGPD é marco legal de proteção de dados pessoais; OWASP Top 10 lista riscos críticos de aplicações web  
C) OWASP revoga a LGPD  
D) LGPD só vale para XSS  
E) ambas são controles da ISO 27001 exclusivamente  

---

## 3. Questões de concurso FCC (16–25)

**16.** (FCC · TRF4/2025 · Analista — Análise de Sistemas)  
No âmbito da LGPD (Lei Geral de Proteção de Dados), existem as figuras de Controlador, Operador e Encarregado. O papel de tais figuras e a relação entre elas no tratamento de dados pessoais e a Autoridade Nacional de Proteção de Dados (ANPD), de acordo com LGPD, podem ser realizados por dois agentes de tratamento, o

A) Controlador e o Operador, enquanto o Encarregado é a pessoa indicada pelo controlador e operador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
B) Controlador e o Encarregado, enquanto o Operador é a pessoa indicada pelo encarregado e controlador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
C) Encarregado e o Operador, enquanto o controlador é a pessoa indicada pelo Encarregado e operador para atuar como canal de comunicação entre o operador, os titulares dos dados e a ANPD.  
D) Controlador e o Operador, enquanto o Encarregado é a pessoa indicada pelo titular e operador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
E) Controlador e o Encarregado, enquanto o Encarregado é a pessoa indicada pelo controlador e titulares para atuar como canal de comunicação entre o controlador, o encarregado dos dados e a ANPD.  

**17.** (FCC · TRT-23/2022 · Analista Judiciário — TI)  
Dentre as sanções administrativas aplicáveis pela autoridade nacional aos agentes de tratamento de dados, em razão das infrações cometidas às normas previstas na Lei Geral de Proteção de Dados (Lei nº 13.709/2018), está a suspensão do exercício da atividade de tratamento dos dados pessoais a que se refere a infração pelo período máximo de

A) 6 meses, prorrogável por igual período.  
B) 6 meses, prorrogável por até 12 meses.  
C) 12 meses, prorrogável por até 24 meses.  
D) 10 meses, prorrogável por até 24 meses.  
E) 1 ano, prorrogável por até 6 meses.  

**18.** (FCC · TRT-4/2022 · Analista — Contabilidade)  
Nos termos da Lei Geral de Proteção de Dados (Lei nº 13.709/2018), é necessário expresso consentimento do titular para tratamento de dados pessoais

A) para exercício regular de direito em processo administrativo.  
B) para exercício regular de direito em processo judicial.  
C) para exercício regular de direito em processo arbitral.  
D) em qualquer caso.  
E) se necessário compartilhamento com outro controlador, ainda que o titular já tenha consentido com o primeiro tratamento.  

**19.** (FCC · TRT-7/2024 · Técnico — Administrativa)  
Em conformidade com a Lei nº 13.709/2018 (Lei Geral de Proteção de Dados Pessoais – LGPD), considere: I. É vedado às operadoras de planos privados de assistência à saúde o tratamento de dados de saúde para a prática de seleção de riscos na contratação de qualquer modalidade, assim como na contratação e exclusão de beneficiários. II. A comunicação ou o uso compartilhado de dados pessoais sensíveis entre controladores com objetivo de obter vantagem econômica poderá ser objeto de vedação ou de regulamentação por parte da autoridade nacional, ouvidos os órgãos setoriais do Poder Público, no âmbito de suas competências. III. O tratamento de dados pessoais sensíveis somente poderá ocorrer quando o titular ou seu responsável legal consentir, de forma específica e destacada, para finalidades específicas. Com relação ao tratamento de dados pessoais sensíveis, está correto o que se afirma em

A) II, apenas.  
B) I e III, apenas.  
C) I, II e III.  
D) II e III, apenas.  
E) I e II, apenas.  

**20.** (FCC · PMSP/2025 · Analista TIC)  
Uma startup de tecnologia sediada em São Paulo desenvolveu um aplicativo de aluguel de bicicletas. O aplicativo coleta dados pessoais como localização em tempo real, histórico de viagens e dados bancários dos usuários. Recentemente, um cliente solicitou que seus dados fossem eliminados após desinstalar o aplicativo, mas a empresa negou o pedido, alegando que os dados eram necessários para fins estatísticos e de melhoria dos serviços. De acordo com a LGPD, ao negar o pedido de eliminação dos dados, foi violado pela empresa o direito do titular

A) à portabilidade dos dados sob os quais o titular pode solicitar qualquer tipo de ação, inclusive a exclusão.  
B) à anonimização, ao bloqueio ou à eliminação de dados desnecessários para a finalidade informada ou tratados em desconformidade com a lei.  
C) à informação sobre o compartilhamento de dados com terceiros e à soberania sobre seus dados pessoais.  
D) de acessar e remover ele mesmo os dados armazenados de uma maneira segura, simplificada e a qualquer momento que desejar fazê-lo.  
E) de ser informado sobre as bases legais para o tratamento dos dados e sobre sua utilização.  

**21.** (FCC · PMSP/2025 · Analista TIC)  
Observe as seguintes informações veiculadas pela CNN em 18/09/2023: Três anos de LGPD: mais de 600 casos já foram registrados na Agência Nacional de Proteção de Dados. Os incidentes mais comuns envolveram o sequestro de dados, exploração de vulnerabilidades, acesso não autorizado a sistemas de informações e roubo de credenciais. I. Sequestro de dados (ramsonware): com transferência de informações (40 comunicados); sem transferência de informações (34 comunicados). II. Exploração de vulnerabilidade em sistemas de informação: 24 comunicados. III. Acesso não autorizado a sistemas de informação: 19 comunicados. IV. Roubo de credenciais: 9 comunicados. Diante dos desafios requeridos para a proteção de dados, se um Analista pertencente ao quadro de servidores de uma Prefeitura Municipal fosse tomar medidas corretas de compliance LGPD, uma delas seria

A) definir os elementos de dados como qualquer informação estritamente relacionada às pessoas jurídicas identificadas ou não para evitar os riscos relacionados aos itens III e IV.  
B) nomear um Data Protection Officer (DPO) responsável por supervisionar a conformidade com a LGPD, gerenciar solicitações de titulares de dados e atuar como ponto de contato com a Agência Nacional de Proteção de Dados (ANPD).  
C) especificar as bases legais que legitimam o tratamento de dados pessoais, como a identificação do titular (riscos relacionados aos itens I e II), cumprimento de obrigação legal com a LGPD e exoneração de servidores públicos envolvidos, conforme estabelece a ANPD.  
D) nomear um Gerador de Dados e Registros (GDR), gestor responsável pelas informações legais passíveis de serem repassadas a terceiros não autorizados nacionais ou internacionais conforme artigo 4º da LGPD.  
E) responsabilizar os servidores públicos que se enquadrarem nas irregularidades previstas nos artigos 6º da LGPD e definidas em detalhes no parágrafo 4º desse mesmo artigo, observadas as recomendações específicas da ANPD.  

**22.** (FCC · TJ-CE/2022 · Analista — SI)  
Considere o texto abaixo, adaptado da Lei Geral de Proteção de Dados (LGPD). "Os agentes de tratamento de dados, em razão das infrações cometidas às normas previstas nesta Lei, ficam sujeitos às sanções administrativas aplicáveis pela autoridade nacional, dentre elas, multa simples sobre o faturamento da pessoa jurídica de direito privado, grupo ou conglomerado no Brasil no seu último exercício, excluídos os tributos, limitada, no total, a um determinado valor em reais por infração ." (grifos nossos). O percentual de multa simples e o valor em reais por infração, referidos no texto, são, correta e respectivamente, de até

A) 3% e R$ 60.000.000,00.  
B) 4% e R$ 50.000.000,00.  
C) 4% e R$ 100.000.000,00.  
D) 2% e R$ 50.000.000,00.  
E) 2% e R$ 100.000.000,00.  

**23.** (FCC · TRT-15/2024 · Analista — TI)  
Uma empresa de tecnologia foi denunciada por um cliente à Autoridade Nacional de Proteção de Dados (ANPD) por não proteger adequadamente os dados pessoais coletados. Após investigação, a ANPD constatou que a empresa não adotou medidas de segurança adequadas e não informou os titulares sobre o tratamento de seus dados. Nesse caso, uma provável sanção que a empresa pode sofrer, de acordo com a LGPD, é

A) advertência, com indicação de prazo para adoção de medidas corretivas.  
B) multa de até 10% do faturamento anual da empresa.  
C) prisão dos responsáveis pela empresa.  
D) proibição definitiva de tratar dados pessoais.  
E) suspensão das atividades da empresa por tempo indeterminado.  

**24.** (FCC · TRT-4/2022 · Técnico Judiciário)  
Considere os seguintes itens:
I. Confirmação da existência de tratamento.
II. Anonimização de dados.
III. Portabilidade dos dados a outro fornecedor de serviço ou produto.
IV. Revogação do consentimento.

Conforme estabelece a LGPD, o titular dos dados pessoais tem direito a obter do controlador, em relação aos dados do titular por ele tratados, a qualquer momento e mediante requisição, o que consta em

A) I, III e IV, apenas.  
B) II e IV, apenas.  
C) I, II, III e IV.  
D) I, II e III, apenas.  
E) I e IV, apenas.  

**25.** (FCC · TRT-4/2022 · Analista Judiciário (várias especialidades))  
Nos termos da Lei Geral de Proteção de Dados Pessoais (LGPD), Lei nº 13.709/2018, no que concerne ao tratamento de dados pessoais de crianças e de adolescentes,

A) os dados podem ser coletados, se para sua proteção, e podem ser repassados a terceiros, ambos sem consentimento.  
B) o consentimento pode ser genérico ou específico.  
C) o consentimento deve ser dado por ambos os pais ou responsável legal.  
D) os dados podem ser coletados sem o consentimento, se para contatar os pais ou responsável legal, podendo ser utilizados a essa finalidade.  
E) os dados podem ser coletados sem consentimento, desde que para contatar os pais ou responsável legal, não podendo ficar armazenados.  

---

## 4. Gabarito comentado

| # | Gab | Comentário curto |
|---|-----|------------------|
| 1 | **A** | Controlador decide |
| 2 | **B** | Biometria vinculada = sensível |
| 3 | **B** | Livre, informada, inequívoca + finalidade determinada |
| 4 | **E** | Legítimo interesse fora do art. 11 |
| 5 | **D** | Art. 39 — instruções do controlador |
| 6 | **E** | Canal; não é agente de tratamento |
| 7 | **B** | Minimização |
| 8 | **A** | 2% / R$ 50 mi |
| 9 | **D** | 6 + igual período |
| 10 | **B** | Art. 12 — regra + exceção de reversão |
| 11 | **A** | Art. 48 — controlador |
| 12 | **B** | Art. 14 — ao menos um responsável; específico/destaque |
| 13 | **B** | Art. 5º, XVII / 38 |
| 14 | **D** | Definição de controlador |
| 15 | **B** | Lei × lista de riscos web |
| Questão | Gabarito | Fonte / nota |
|---|---|---|
| 16 | **A** | Agentes × encarregado |
| 17 | **A** | Suspensão 6 meses |
| 18 | **E** | Consentimento e compartilhamento |
| 19 | **E** | Dados sensíveis I+II |
| 20 | **B** | Eliminação/anonimização |
| 21 | **B** | Nomear DPO |
| 22 | **D** | Multa 2% / R$ 50 mi |
| 23 | **A** | Advertência |
| 24 | **A** | Direitos do titular I/III/IV |
| 25 | **E** | Crianças — sem armazenamento |

**Distribuição treino (1–15):** A3 B7 C0 D3 E2.  
**Distribuição concurso (16–25):** A4 B2 C0 D1 E3.

**Notas:** 16 A · 17 A · 18 E · 19 E · 20 B · 21 B · 22 D · 23 A · 24 A · 25 E.


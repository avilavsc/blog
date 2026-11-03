+++
title = '01 — MoReq-Jus 2ª edição · Analista TI (código 14)'
date = '2026-10-26'
draft = false
weight = 1
tags = ['semana-10', 'moreq']
categories = ['estudo']
+++
# MoReq-Jus 2ª edição · Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — MoReq-Jus (2ª edição)  
**Escopo deste arquivo:** o que é o MoReq-Jus; Res. CNJ **91/2009** (instituição) × **522/2023** (atualização/obrigatoriedade); **GestãoDoc**; RF × RNF; O/D; captura; metadados; prazos art. 6; conflito → segurança; MoReq-Aval; RDC-Arq; ligação PDPJ/Proname; anti-confusão com ENSEC, ENTIC, LGPD.  
**Fora de escopo aqui:** PDTIC TRT4 capítulo a capítulo (arquivo 02); Modelo de Governança TIC TRT4 em profundidade (03); checklist linha a linha dos centenas de requisitos numerados do PDF.

---

## 1. Material

### 1.1 Por que a FCC cobra

No edital (código 14), MoReq-Jus aparece no bloco de normativos do Judiciário / gestão documental eletrônico. A banca quase nunca pede “decorar o PDF página a página”: ela quer **qual resolução faz o quê**, **objetivos da adesão**, **alcance (módulos/microsserviços)**, **RF × RNF**, **prazos**, **prioridade da segurança em conflito**, **captura/GestãoDoc** e **não misturar** com ENSEC (396), ENTIC (370) ou Conduta.

Flash:

| Instrumento | Ideia |
|-------------|--------|
| **MoReq-Jus** | Modelo de **requisitos** para sistemas de gestão de **processos e documentos** |
| **GestãoDoc** | Nome genérico do sistema aderente ao modelo |
| **91/2009** | **Instituiu** o MoReq-Jus |
| **522/2023** | **Atualizou** / tornou adesão obrigatória no regramento vigente + MoReq-Aval |
| **Proname / 324/2020** | Política de gestão documental e memória (contexto) |

### 1.2 Frase-âncora e definição

**Frase-âncora:** MoReq-Jus = **Modelo de Requisitos** para Sistemas Informatizados de Gestão de Processos e Documentos do Poder Judiciário — critérios mínimos de captura, produção, classificação, tramitação, guarda, avaliação, armazenamento, indexação, preservação, arquivamento e recebimento, para **confiabilidade, autenticidade e acesso**, em documentos **digitais, não digitais ou híbridos**, **independente da plataforma**.

Um **GestãoDoc** registra operações sobre documentos digitais e, para físicos, pelo menos as **referências** e operações (pode haver versões digitais dos físicos).

Gestão documental **não** é só “arquivo”: permeia protocolo → distribuição → classificação → uso → tramitação → baixa → arquivamento → destinação (eliminação ou guarda permanente).

### 1.3 Histórico normativo cobrável

1. **Res. CNJ 91/2009** — institui o MoReq-Jus (1ª linha histórica; ainda cai “quem instituiu”).  
2. Evolução com Proname, PJe, Justiça 4.0, Res. **324/2020** (gestão documental/memória).  
3. **Res. CNJ 522/2023** — aprova a **2ª edição** e disciplina adesão, prazos, MoReq-Aval, preservação em descontinuação.  
4. Ligação com **PDPJ-Br**: sistemas/módulos/microsserviços que ingressam na plataforma devem observar MoReq-Jus naquilo que couber.

Distrator clássico: dizer que MoReq = **396** (ENSEC), **370** (ENTIC), **400** (sustentabilidade) ou **Conduta**.

### 1.4 Art. 2º da 522/2023 — ouro de prova

Sistemas de gestão de processos e documentos (judiciais **e** administrativos), **ainda que** segmentados em **aplicações, módulos e microsserviços**, **deverão aderir** ao MoReq-Jus para assegurar:

**confidencialidade · integridade · disponibilidade · autenticidade · não repúdio · conformidade · preservação**

Memorize o **rol completo**. Distratores trocam por “concordância / especificidade / razoabilidade / não duplicidade” ou omitem preservação/conformidade.

Armadilha C/E: “à **exceção** dos segmentados em microsserviços” → **errado** (a norma inclui, não exclui).

### 1.5 RF × RNF e obrigatoriedade O/D

| Tipo | Sigla | Ideia |
|------|-------|--------|
| **Funcional** | **RF** | O que o sistema **deve ser capaz de fazer** (função/comportamento operacional) |
| **Não funcional** | **RNF** | Qualidade/restrições: desempenho, usabilidade, interoperabilidade, segurança estrutural, disponibilidade, manutenção… |
| RNF de produto | RNF-P | Desempenho, usabilidade, interoperabilidade |
| RNF organizacional | RNF-O | Ambiente, operação, desenvolvimento, backup, redes… |
| RNF externo | RNF-E | Legal, normativo, ético |

Níveis:

- **O** (obrigatório) — imprescindível.  
- **D** (desejável) — pode haver razão válida para não adotar; examinar implicações; interoperabilidade não pode “exigir” D do outro sistema.

Capítulos típicos (mapa, não decore todos os códigos):

| Eixo | Exemplos de siglas |
|------|--------------------|
| RF | RPC (plano de classificação), RCA (captura), RFT (fluxo/tramitação), RAD (avaliação/destinação), RPA (pesquisa), RSC/RTA (segurança/trilha) |
| RNF | RAR (armazenamento), RPR (preservação), RSE, RDI, RUS, RIN, RDE, RME |

### 1.6 Conflito de requisitos (FCC TRT-6/2025)

Na especificação/desenvolvimento, se houver **conflito entre requisitos**, aplicam-se os **RNF relacionados à segurança em detrimento de outros**.

Isso é regra de **prioridade**, não “cancelar todos os funcionais”.

Também devem ser identificados: RF, dados e **metadados** tratados por serviços interligados; e referências aos RF/RNF nas novas funcionalidades.

### 1.7 Prazos de adequação (art. 6º — 522/2023)

Para sistemas **em utilização**:

| Prazo | Objeto |
|-------|--------|
| **até 1 ano** | Recebimento, armazenamento e envio de **100% dos metadados** |
| **até 2 anos** | Requisitos **funcionais** |
| **até 3 anos** | Requisitos **não funcionais** |

Início dos prazos: após publicação dos instrumentos de avaliação (art. 8º).  
Distratores: inverter (funcionais em 1 ano; metadados em 3; “6 meses”).

### 1.8 Captura e GestãoDoc (clássico FCC)

**Captura** = incorporação do documento/processo ao GestãoDoc, passando a seguir tramitação/fluxo.

Na linha cobrada (Res. 91 / enunciados FCC): captura envolve, entre outras, **Protocolo**, **atribuição de restrição de acesso**, **arquivamento**, **autuação**, **classificação** e **indexação**.

Distratores: “conservação” como ação imediata de captura; “desmonte pós-digitalização”; temporalização/hierarquização como núcleo da captura.

### 1.9 Metadados, preservação e RDC-Arq

Metadados descrevem contexto (jurídico-administrativo, proveniência, procedimentos, documental, tecnológico) e sustentam autenticidade/preservação.

Documentos de **guarda longa/permanente**: armazenamento em **RDC-Arq** (Repositório Arquivístico Digital Confiável), com referência a modelo **OAIS**.

Sistemas em **desuso/descontinuados**: preservar dados/metadados; garantir certidão/acesso; classificar/avaliar/destinar (Proname) **ou** migrar para sistema aderente.  
**É vedada** substituir por sistema **menos aderente** ao MoReq-Jus (estudo prévio gestão documental + TI).

### 1.10 MoReq-Aval

Programa de avaliação de aderência e atualização permanente.

- Coordenação/acompanhamento: Comissão Permanente de Gestão Documental e Memória (+ apoio Comissão de TI).  
- **Execução:** **Departamento de Tecnologia da Informação do CNJ**, com apoio do **Comitê do Proname**.  

Distrator: inverter (Secretaria Executiva executa; Proname só executa sem DTI).

Órgãos informam periodicamente requisitos **atendidos ou não**, **obrigatórios e desejáveis** (sistema principal + serviços interligados) — inclusive quem usa sistema de terceiro.

### 1.11 Princípios cobráveis (visão PDPJ / 2ª ed.)

Em prova recente (FCC TRF4/2025): ênfase em **colaboração entre tribunais**, desenvolvimento de sistemas **públicos e gratuitos**, **interoperabilidade** e **segurança** dos dados processuais.

Anti-lista:

- Priorizar “só sistemas privados para independência” → errado.  
- Interoperabilidade “secundária” → errado.  
- Colaboração “opcional” → errado.  
- Segurança “só Justiça Federal” → errado (PJ todo).

### 1.12 Anti-confusão rápida

| Instituto | Tema |
|-----------|------|
| **MoReq-Jus / 522** | Requisitos GestãoDoc / processos e documentos |
| **ENTIC-JUD / 370** | Estratégia nacional de TIC + PDTIC |
| **ENSEC / 396** | Segurança cibernética do PJ |
| **PDPJ / 443…** | Plataforma digital nacional |
| **324/2020 Proname** | Gestão documental e memória |
| **LGPD** | Dados pessoais (lei) — não é o MoReq |

### 1.13 Mini-síntese

```
91/2009 institui → 522/2023 atualiza/obriga
GestãoDoc = digital + físico/híbrido + metadados
RF (fazer) × RNF (qualidade) · O/D
Conflito → RNF segurança prevalece
Prazos: metadados 1a · RF 2a · RNF 3a
Captura: protocolo + restrição + arquivamento + autuação/classificação/indexação
≠ ENSEC ≠ ENTIC ≠ Conduta
```

---

## 2. Questões de treino (1–15)

**1.** MoReq-Jus, na essência cobrável, é:

A) Framework ISACA idêntico ao COBIT 2019  
B) Modelo de **requisitos** para sistemas de gestão de processos e documentos do PJ  
C) Código de Conduta do TRT4  
D) Protocolo de roteamento OSPF  
E) Sinônimo literal da LGPD  

**2.** A resolução que **instituiu originariamente** o MoReq-Jus foi a:

A) CNJ 396/2021  
B) CNJ 370/2021  
C) CNJ 400/2021  
D) CNJ 91/2009  
E) CNJ 351/2020  

**3.** A Res. CNJ 522/2023, no papel cobrável atual, sobretudo:

A) Revoga a ENTIC-JUD  
B) Substitui o Regimento Interno do TRT4  
C) Atualiza/disciplina a adesão ao MoReq-Jus (2ª edição) e mecanismos correlatos  
D) Cria o PJe em 2009  
E) Define apenas cabeamento estruturado  

**4.** O rol de objetivos da adesão (art. 2º da 522) inclui:

A) Apenas “concordância e razoabilidade”  
B) Confidencialidade, integridade, disponibilidade, autenticidade, não repúdio, conformidade e preservação  
C) Somente story points  
D) Exclusivamente WEP/WPA  
E) Apenas crase e concordância  

**5.** Sistemas segmentados em módulos/microsserviços:

A) Estão excluídos do MoReq-Jus  
B) Também devem aderir, na linha do art. 2º  
C) Só precisam de ITIL Version 5  
D) Dispensam metadados para sempre  
E) Equivalem automaticamente à ENSEC  

**6.** RF × RNF, leitura correta:

A) RF = restrição legal; RNF = botão na tela  
B) São idênticos e sempre O obrigatórios  
C) RF especifica funcionalidade; RNF afeta qualidade/comportamento geral  
D) RNF elimina a necessidade de captura  
E) RF = apenas firewall  

**7.** Em conflito entre requisitos na especificação/desenvolvimento:

A) Prevalece sempre a usabilidade cosmética  
B) Ignora-se a segurança  
C) Aplicam-se RNF de **segurança** em detrimento de outros  
D) Cancela-se o GestãoDoc  
E) Adota-se automaticamente o PMBOK 8  

**8.** Prazo correto (art. 6º) para 100% dos metadados:

A) Até 1 ano  
B) Até 10 anos fixos  
C) Até 3 anos  
D) Até 2 anos  
E) Sem prazo algum  

**9.** Prazo para requisitos **funcionais** (art. 6º):

A) Até 1 ano  
B) Até 2 anos  
C) Até 6 meses  
D) Até 3 anos  
E) Imediato sem transição  

**10.** Captura, no sentido clássico cobrado, **não** se completa tipicamente com:

A) Autuação  
B) Classificação  
C) Indexação  
D) Conservação como sinônimo imediato da captura  
E) Protocolo (já no núcleo do conceito)  

**11.** GestãoDoc deve gerenciar:

A) Só papel, nunca digital  
B) Documentos/processos digitais, não digitais e híbridos  
C) Apenas e-mails pessoais  
D) Somente logs de syslog  
E) Exclusivamente certificados SSL  

**12.** MoReq-Aval, execução típica na 522:

A) Exclusivamente pelo TCU sem CNJ  
B) Pelo DTI do CNJ, com apoio do Comitê do Proname  
C) Pela OAB Federal  
D) Pelo cartório extrajudicial privado  
E) Pelo Service Desk municipal  

**13.** Substituição de sistema por outro **menos aderente** ao MoReq-Jus:

A) É livre e incentivada  
B) É vedada na linha da 522 (estudo prévio exigido)  
C) É obrigatória a cada semestre  
D) Equivale a adotar a Res. 400  
E) Dispensa qualquer metadado  

**14.** Contraste correto:

A) MoReq = ENSEC  
B) MoReq = requisitos de gestão de processos/documentos; 396 = ENSEC  
C) 370 = MoReq  
D) Conduta = GestãoDoc  
E) LGPD = captura de protocolo  

**15.** Síntese adequada:

A) 91 institui · 522 atualiza/obriga · RF/RNF · prazos 1/2/3 · segurança em conflito · ≠ ENTIC/ENSEC  
B) MoReq substitui COBIT e ITIL mundialmente  
C) Microsserviços estão fora do alcance  
D) Metadados têm prazo de 3 anos e RF de 1 ano  
E) Captura = apenas desmonte físico pós-digitalização  

---

### Gabarito comentado (treino 1–15)

1. **B** — Modelo de requisitos GestãoDoc.  
2. **D** — 91/2009 institui.  
3. **C** — 522 atualiza/disciplina.  
4. **B** — Rol CID + autenticidade + não repúdio + conformidade + preservação.  
5. **B** — Inclui segmentados.  
6. **C** — RF função × RNF qualidade.  
7. **C** — Segurança prevalece.  
8. **A** — Metadados 1 ano.  
9. **B** — RF 2 anos.  
10. **D** — Conservação ≠ núcleo imediato da captura.  
11. **B** — Digital/não digital/híbrido.  
12. **B** — DTI CNJ + Proname.  
13. **B** — Vedada substituição menos aderente.  
14. **B** — Eixos distintos.  
15. **A** — Cartão mental.

---

## 3. Questões de concurso (16–25)

> Questões **reais** (preferência FCC). Gabarito só no final.

**16.** (FCC · TRT-20/2024 · Técnico Judiciário — TI)  
A Resolução CNJ nº 522/2023 em seu Art. 2º estabelece que os sistemas informatizados de gestão de processos e documentos utilizados em atividades judiciais e administrativas dos órgãos integrantes do Poder Judiciário, ainda que segmentados em aplicações, módulos e microsserviços, deverão aderir aos requisitos do MoReq-Jus, com o objetivo de

A) acompanhar o fiel cumprimento desta Resolução e a coordenação do Programa de avaliação do grau de aderência dos sistemas ao MoReq-Jus e de atualização permanente (Programa MoReq-Aval).  
B) avaliar o grau de aderência dos requisitos ao MoReq-Jus e de sua atualização permanente especificada no Programa MoReq-Aval/2023.  
C) assegurar a concordância, a segurança, a especificidade, o grau de complexidade, a integridade, a razoabilidade e a não duplicidade de processos e documentos do Poder Judiciário.  
D) identificar os requisitos funcionais, dados e metadados que serão tratados por serviços interligados ao sistema principal e as referências ao requisito funcional implementado e aos requisitos não funcionais observados por ocasião da especificação de novas funcionalidades ou de melhorias daquelas existentes.  
E) assegurar a confidencialidade, a integridade, a disponibilidade, a autenticidade, o não repúdio, a conformidade e a preservação de processos e documentos do Poder Judiciário.  

**17.** (FCC · TRT-6/2025 · Analista Judiciário — TI)  
Agindo em conformidade com a Resolução CNJ nº 522/2023, um Analista Judiciário deve

A) providenciar a adequação ao MoReq-Jus dos sistemas informatizados de gestão de processos e documentos em utilização nos órgãos do Poder Judiciário, no prazo máximo de 6 meses quanto aos requisitos funcionais e de 1 ano quanto aos requisitos não funcionais.  
B) armazenar no Repositório de Arquivos Digitais em Nuvem (RAD-Cloud), os processos, dossiês e documentos de guarda longa ou permanente de sistemas legados menos aderentes ao MoReq-Jus.  
C) informar semanalmente, durante o desenvolvimento, a relação dos requisitos não atendidos, identificando se pertencem ao sistema principal ou aos serviços em nuvem interligados.  
D) providenciar a adequação ao MoReq-Jus dos sistemas informatizados de gestão de processos e documentos em utilização nos órgãos do Poder Judiciário, no prazo máximo de 3 anos, quanto ao recebimento, armazenamento e envio de 100% dos metadados.  
E) aplicar os requisitos não funcionais relacionados à segurança em detrimento de outros, na especificação e no desenvolvimento de funcionalidade em que se constate conflito entre requisitos.  

**18.** (FCC · TRT-17/2018–2022 · Analista Judiciário — TI)  
O Modelo de Requisitos para Sistemas Informatizados de Gestão de Processos e Documentos do Judiciário brasileiro (MoReq-Jus), instituído pela Resolução nº 91 de 29/09/2009, estabelece que: “Captura é a incorporação de um documento/processo ao GestãoDoc, quando passará a seguir as rotinas de tramitação. Uma vez capturado, o documento será incluído no fluxo de trabalho”. A captura consiste nas ações de: Protocolo, Atribuição de restrição de acesso, Arquivamento,

A) Classificação, Indexação e Conservação.  
B) Autuação, Classificação e Indexação.  
C) Classificação, Correlacionamento de metadados e Desmonte pós digitalização.  
D) Análise, Autuação e Permissão diferenciada.  
E) Indexação, Temporalização e Hierarquização.  

**19.** (FCC · TRF-4/2025 · Técnico Judiciário — Desenvolvimento de Sistemas)  
O Modelo de Requisitos para Sistemas Informatizados de Gestão de Processos e Documentos do Poder Judiciário (MoReq-Jus) 2ª edição, parte dos normativos da Plataforma Digital do Poder Judiciário – PDPJ-Br, estabelece diretrizes para o desenvolvimento e a manutenção de sistemas judiciais. Dentre os princípios e diretrizes, o MoReq-Jus

A) permite a contratação de sistemas privados para a gestão de processos judiciais, visando a independência tecnológica dos tribunais.  
B) considera a interoperabilidade entre diferentes sistemas judiciais como um requisito secundário, priorizando sistemas federais.  
C) prioriza a utilização de sistemas desenvolvidos externamente, sendo que a necessidade de colaboração entre os tribunais é opcional.  
D) estabelece critérios para a segurança dos dados processuais priorizando os sistemas de justiça no âmbito federal.  
E) enfatiza a importância da colaboração entre os tribunais para o desenvolvimento de sistemas públicos e gratuitos, garantindo a interoperabilidade e a segurança dos dados processuais.  

**20.** (FCC · TRT-15/2025 · Analista Judiciário — Arquivologia)  
O Modelo de Requisitos para Sistemas Informatizados de Gestão de Documentos e Processos do Poder Judiciário (MoReq-Jus) foi instituído pela Resolução CNJ nº

A) 91/2009.  
B) 396/2021.  
C) 324/2020.  
D) 469/2022.  
E) 370/2021.  

**21.** (CEBRASPE · STM/2025 · Analista — Análise de Sistemas)  
Com base na Resolução CNJ nº 522/2023, julgue o item:  
Os sistemas informatizados de gestão de processos e documentos utilizados em atividades judiciais e administrativas dos órgãos integrantes do Poder Judiciário deverão aderir aos requisitos do MoReq-Jus, com o objetivo de assegurar, entre outros, a confidencialidade, a integridade, a disponibilidade, a autenticidade e o não repúdio de processos e documentos do Poder Judiciário.

A) Certo  
B) Errado  
C) Certo apenas para atividades judiciais  
D) Errado, pois a adesão é facultativa  
E) Certo apenas se houver PETIC  

**22.** (CEBRASPE · STM/2025 · Analista — Suporte em TI)  
Com base na Resolução CNJ nº 522/2023, julgue o item:  
Os sistemas informatizados de gestão de processos e documentos utilizados em atividades judiciais e administrativas dos órgãos integrantes do Poder Judiciário, à exceção dos que forem segmentados em aplicações, módulos e microsserviços, deverão atender aos requisitos do MoReq-Jus.

A) Certo  
B) Errado  
C) Certo só para módulos  
D) Errado só na Justiça Eleitoral  
E) Certo se houver RDC-Arq  

**23.** (CEBRASPE · STM/2025 · Analista — Análise de Sistemas)  
Com base na Resolução CNJ nº 522/2023, julgue o item:  
O programa de avaliação do grau de aderência dos sistemas ao MoReq-Jus e de atualização permanente é executado pela Secretaria Executiva do CNJ, com o apoio do Comitê do Programa Nacional de Gestão Documental e Memória do Poder Judiciário (PRONAME).

A) Certo  
B) Errado  
C) Certo quanto ao executor, errado quanto ao apoio  
D) Errado apenas no nome Proname  
E) Certo se houver PDTIC  

**24.** (CEBRASPE · TRT-10/2025 · Analista — Arquivologia)  
No que se refere à regulamentação do MoReq-Jus e de seu uso no Poder Judiciário, julgue o item:  
A adequação ao MoReq-Jus pelos sistemas informatizados de gestão de processos e documentos em utilização nos órgãos do Poder Judiciário deverá ser efetuada, quanto aos requisitos funcionais, em até um ano.

A) Certo  
B) Errado  
C) Certo para metadados e funcionais  
D) Errado apenas para RNF  
E) Certo após 2026  

**25.** (CEBRASPE · TRF-6/2025 · Analista — Governança e Gestão de TI)  
Com base na Resolução CNJ nº 522/2023, julgue o item:  
Os sistemas em desuso podem ser substituídos por sistemas menos aderentes ao MoReq-Jus, desde que garantam o armazenamento em repositório arquivístico digital confiável.

A) Certo  
B) Errado  
C) Certo se houver OAIS  
D) Errado apenas para guarda temporária  
E) Certo na Justiça do Trabalho  

---

### Gabarito comentado (concurso 16–25)

| Q | Gab. | Fonte / chave |
|---|------|----------------|
| 16 | **E** | FCC TRT-20/2024 — rol completo art. 2º |
| 17 | **E** | FCC TRT-6/2025 — RNF segurança em conflito |
| 18 | **B** | FCC TRT-17 — captura: autuação, classificação, indexação |
| 19 | **E** | FCC TRF-4/2025 — colaboração / públicos / interoperabilidade |
| 20 | **A** | FCC TRT-15/2025 — instituição pela 91/2009 |
| 21 | **A** | CEBRASPE STM/2025 — adesão + objetivos (Certo) |
| 22 | **B** | CEBRASPE STM/2025 — não há exceção a microsserviços (Errado) |
| 23 | **B** | CEBRASPE STM/2025 — executor é DTI/CNJ (+ Proname), não Secretaria Executiva |
| 24 | **B** | CEBRASPE TRT-10/2025 — funcionais = até **2 anos** (não 1) |
| 25 | **B** | CEBRASPE TRF-6/2025 — vedada substituição menos aderente |

---

## 4. Como usar este arquivo

1. Fechar de cor: **91 × 522** · **rol art. 2º** · **RF/RNF** · **prazos 1/2/3** · **segurança em conflito** · **captura**.  
2. Treino 1–15 → gabarito.  
3. Concurso 16–25 sem consultar (priorizar FCC 16–20).  
4. Ligar depois a PDTIC/Governança (02–03) sem fundir instrumentos.  
5. Flash anti-mistura: MoReq ≠ ENSEC ≠ ENTIC ≠ Conduta.

---

*Arquivo 01 · MoReq-Jus 2ª ed. · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 10 — governança/documental.*

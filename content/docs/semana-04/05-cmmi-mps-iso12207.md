+++
title = '05 — CMMI v3.0 Dev · MPS-SW 2024 · ISO 12207'
date = '2026-10-19'
draft = false
weight = 5
tags = ['semana-04', 'cmmi', 'mps']
categories = ['estudo']
+++
# CMMI v3.0 Dev · MPS-SW 2024 · ISO 12207 · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — CMMI v3.0 (domínio Development), MR-MPS-SW 2024 e ABNT NBR ISO/IEC/IEEE 12207:2021 (visão) (Semana 4 · Engenharia de software)  
**Escopo deste arquivo:** **Modelos/normas de processo de software** cobrados no edital: CMMI **v3.0** no domínio **Development**; **MR-MPS-SW versão 2024** (níveis, lógica SOFTEX / MPS.BR); **ISO/IEC/IEEE 12207** (visão de processos de ciclo de vida). Contrastes entre os três e flash cobrável FCC.  
**Fora de escopo aqui:** BPMN e UML a fundo (outros arquivos); Scrum/Kanban/XP detalhados; ITIL/COBIT/PMBOK; low-code/IA; auditoria completa de avaliação CMMI/MPS; texto normativo integral.

---

## 1. Material

### 1.1 Mapa dos três (leitura de prova)

O edital junta três peças do bloco **processo de desenvolvimento de software**. Não são sinônimos:

| Peça | Natureza | Flash |
|------|----------|-------|
| **CMMI v3.0 — domínio Development** | Modelo de **capacidade/maturidade** de práticas (ISACA / CMMI Institute) | “Quão bem a organização executa práticas de desenvolvimento” |
| **MR-MPS-SW 2024** | Modelo de referência **brasileiro** (SOFTEX) para melhoria de processo de software | Níveis **G → A** (crescente) |
| **ISO/IEC/IEEE 12207:2021** (ABNT NBR) | Norma de **processos de ciclo de vida** de software/sistemas de software | “Quais processos existem no ciclo de vida” |

Frase ouro: **12207 descreve processos**; **CMMI/MPS avaliam/melhoram capacidade e maturidade** na execução (visões complementares).

### 1.2 CMMI v3.0 — o que mudou na cabeça de prova

**CMMI** = Capability Maturity Model Integration.

Pontos de versão (visão cobrável, sem manual de avaliador):

| Ideia | Leitura |
|-------|---------|
| **v3.0** | Evolução do modelo (edital cita **v3.0** — domínio Development) |
| **Domínios** | Em vez de “constelações” clássicas isoladas na cabeça antiga; há domínio **Development** (Dev) entre outros |
| **Practice Areas (PAs)** | Áreas de prática (evolução da ideia de process areas) |
| **Capability × Maturity** | Capacidade (por área) × maturidade (perfil organizacional / níveis) |
| **Níveis** | Escala de maturidade clássica cobrada: tipicamente **1 a 5** (e noção de 0/incompleto em leituras modernas) |

Flash: enunciado fala em **maturidade**, **capacidade**, **práticas de desenvolvimento**, **níveis 2/3/4/5**, **CMMI Development** → **CMMI**.

CMMI **não** é: BPMN, UML, ISO 12207 (embora relate-se), MPS-SW (embora haja mapeamentos), Scrum puro, ITIL.

### 1.3 Níveis de maturidade CMMI (cartão clássico FCC)

A banca adora o **significado** dos níveis (visão estável de prova):

| Nível | Nome mental | Ideia cobrável |
|-------|-------------|----------------|
| **1** | Inicial / ad hoc | Sucesso depende de heróis; pouco previsível |
| **2** | Gerenciado | Projetos planejados/controlados no essencial |
| **3** | Definido | Processos **padronizados** na organização |
| **4** | Quantitativamente gerenciado | Processo medido / estatisticamente controlado (visão) |
| **5** | Em otimização | Melhoria contínua baseada em dados / inovação |

Anti-confusão:

- Nível **mais alto** = mais maduro (5 > 1).  
- **Não** confundir com níveis **G–A** do MPS (outra escala).  
- Domínio **Development** = foco em **desenvolver** produtos/soluções de software (visão do edital).

### 1.4 Capability vs maturity (margem de segurança)

| Conceito | Foco |
|----------|------|
| **Capability level** | Quão capaz está uma **prática/área** específica |
| **Maturity level** | Perfil **organizacional** agregado (estágios 1–5) |

Prova: reconhecer a **distinção**; não precisa montar appraisal SCAMPI.

### 1.5 Practice areas — visão (não decorar catálogo)

Em CMMI moderno fala-se em **áreas de prática** agrupadas (governança, gerenciamento, desenvolvimento técnico, etc. — nomes variam na exposição didática).

Para FCC, basta:

1. Existem **áreas de prática** cobrindo planejamento, requisitos, desenho, verificação, gestão de configuração, medição, etc. (idéias).  
2. Maturidade exige **implementar e institucionalizar** práticas, não só “ter documento”.  
3. Domínio **Dev** enfatiza engenharia/desenvolvimento de produto.

Armadilha: achar que CMMI **prescreve** uma metodologia ágil ou waterfall exclusiva — CMMI é **o quê** de prática/capacidade; o **como** (Scrum, etc.) é escolha.

### 1.6 MR-MPS-SW 2024 — modelo brasileiro

**MR-MPS-SW** = Modelo de Referência MPS para **Software** (programa **MPS.BR** / SOFTEX).

| Âncora | Frase |
|--------|-------|
| Origem | Iniciativa **brasileira** de melhoria de processo |
| Versão do edital | **2024** |
| Lógica | Compatível/alinhável a boas práticas internacionais (incl. diálogo com CMMI/ISO) — visão |
| Escala | Níveis de maturidade **G, F, E, D, C, B, A** (do mais básico ao mais avançado) |

Flash: **G → A** = crescente. **G** é entrada; **A** é o topo.

MPS-SW **não** é: notação BPMN; diagrama UML; norma 12207 em si (embora use/alinhe processos).

### 1.7 Níveis MPS-SW (tabela ouro)

Memorização útil (ordem e sentido):

| Nível | Ideia didática (visão de prova) |
|-------|----------------------------------|
| **G** | Parcialmente gerenciado — base (ex.: gerência de projetos / requisitos em visão introdutória) |
| **F** | Gerenciado — reforço de gerências (ex.: configuração, qualidade — visão) |
| **E** | Parcialmente definido — começa definição organizacional / engenharia |
| **D** | Largamente definido — mais processos de engenharia definidos |
| **C** | Definido — processo definido amplo |
| **B** | Gerenciado quantitativamente — medições/controle quantitativo (visão) |
| **A** | Em otimização — melhoria contínua |

Não precisa decorar **todos** os processos por nível como avaliador; precisa:

1. Ordem **G < F < E < D < C < B < A**.  
2. Que é modelo **brasileiro** de **software**.  
3. Que **não** usa a numeração 1–5 do CMMI (embora haja **correspondência conceitual** aproximada em materiais didáticos).

### 1.8 CMMI × MPS-SW (contraste cobrável)

| | **CMMI v3.0 Dev** | **MPS-SW 2024** |
|--|-------------------|-----------------|
| Origem | Modelo internacional (CMMI Institute / ISACA) | Modelo **BR** (SOFTEX / MPS.BR) |
| Escala típica cobrada | Maturidade **1–5** | Maturidade **G–A** |
| Foco edital | Domínio **Development** | **Software** (MR-MPS-SW) |
| Relação | Referência global de práticas/capacidade | Adequação nacional; mapeável a práticas internacionais |

Frase de prova: **não** são o mesmo documento; **são correlacionáveis**.

### 1.9 ISO/IEC/IEEE 12207:2021 — visão

**12207** = norma que define um **conjunto de processos de ciclo de vida** para software (e contexto de sistemas de software), adotada como **ABNT NBR ISO/IEC/IEEE 12207:2021** no edital.

| Âncora | Leitura |
|--------|---------|
| Papel | **Catalogar / descrever** processos do ciclo de vida |
| Não é | Escala de maturidade 1–5 ou G–A |
| Uso | Base para definir, adaptar e integrar processos na organização |
| Parentesco | Família de normas de ciclo de vida / engenharia de sistemas-software (visão) |

Grupos de processos (visão cobrável — nomes para reconhecer):

| Grupo (ideia) | Exemplos mentais |
|---------------|------------------|
| **Acordo** | Aquisição, fornecimento |
| **Organizacionais habilitadores** | Gestão de RH, qualidade, conhecimento, etc. (visão) |
| **Gestão técnica / de projeto** | Planejamento, avaliação/controle, decisão, risco, config… |
| **Técnicos** | Requisitos, arquitetura/design, implementação, integração, verificação, validação, operação, manutenção, descarte… |

Flash: enunciado fala em **processos de ciclo de vida**, **aquisição/fornecimento**, **verificação/validação**, **norma ISO de software life cycle** → **12207**.

### 1.10 12207 × CMMI × MPS (triangulação)

| Pergunta da prova | Resposta mental |
|-------------------|-----------------|
| “Quais processos existem no ciclo de vida?” | **ISO 12207** |
| “Qual o grau de capacidade/maturidade das práticas de desenvolvimento?” | **CMMI Dev** |
| “Qual o nível G–A da organização no modelo brasileiro de software?” | **MPS-SW** |
| “Como desenhar o fluxo do processo de negócio?” | **BPMN** (outro arquivo) |
| “Como modelar classes/casos de uso do software?” | **UML** (outro arquivo) |

Complementaridade didática:

```
ISO 12207  →  define o MAPA de processos do ciclo de vida
CMMI / MPS →  dizem o QUÃO MADURO/CAPAZ está a execução
Ágeis      →  como o time trabalha no dia a dia (Scrum/Kanban/XP)
BPMN       →  desenha o fluxo de negócio/workflow
```

### 1.11 Armadilhas clássicas

1. Trocar escala **1–5 (CMMI)** por **G–A (MPS)**.  
2. Dizer que ISO 12207 **é** um nível de maturidade.  
3. Afirmar que CMMI v3.0 Dev **é** o MPS-SW 2024.  
4. Confundir CMMI com **ITIL/COBIT** (governança/serviço — outros arquivos).  
5. Achar que adotar Scrum **equivale** automaticamente a CMMI 5 / MPS A.  
6. Tratar 12207 como notação gráfica (isso é BPMN/UML).

### 1.12 Cartão mental (fechamento do material)

```
CMMI v3.0 Dev  = maturidade/capacidade de práticas (níveis 1–5; domínio Development)
MPS-SW 2024    = modelo BR softex (níveis G→A)
ISO 12207:2021 = norma de PROCESSOS de ciclo de vida (não é escala G–A nem 1–5)
Complementam · não são sinônimos · ≠ BPMN · ≠ UML · ≠ Scrum
```

---

## 2. Questões de treino (1–15)

**1.** O CMMI v3.0 no domínio Development caracteriza-se, em regra, por:

A) Ser notação gráfica de processo de negócio (BPMN)  
B) Ser modelo de **capacidade/maturidade** de práticas de desenvolvimento  
C) Substituir obrigatoriamente o diagrama de classes UML  
D) Ser a escala G–A do MPS.BR exclusivamente  
E) Ser o protocolo OAuth2  

**2.** Na escala clássica de maturidade CMMI cobrada em provas, o nível associado a processos **padronizados na organização** é tipicamente o:

A) 1  
B) 2  
C) **3 (Definido)**  
D) Gateway XOR  
E) Pool BPMN  

**3.** Assinale a associação **correta**:

A) MPS-SW usa níveis 1–5; CMMI usa G–A  
B) CMMI Dev — maturidade/capacidade (visão 1–5); MPS-SW — níveis G–A  
C) ISO 12207 = nível A do MPS  
D) CMMI = apenas diagrama de atividades UML  
E) 12207 = Kanban board  

**4.** O MR-MPS-SW 2024 é:

A) Norma de ciclo de vida ISO que elimina CMMI  
B) Modelo de referência **brasileiro** (SOFTEX) para melhoria de processo de software  
C) Motor de containers Docker  
D) API Gateway Zuul  
E) Broker RabbitMQ  

**5.** Relativamente à ordem dos níveis MPS-SW, é correto afirmar que:

A) A é o mais básico e G o mais avançado  
B) A ordem crescente de maturidade é **G → … → A**  
C) Os níveis são idênticos aos eventos BPMN  
D) Só existem os níveis 4 e 5  
E) F vem depois de A  

**6.** Qual afirmação é **incorreta**?

A) ISO 12207 descreve processos de ciclo de vida de software  
B) CMMI Dev avalia capacidade/maturidade de práticas  
C) BPMN é a escala oficial de maturidade do CMMI v3.0  
D) MPS-SW 2024 é modelo brasileiro de software  
E) CMMI e MPS são correlacionáveis, mas não idênticos  

**7.** Comparando ISO 12207 e CMMI na leitura de edital:

A) São mutuamente o mesmo artefato com outro nome  
B) 12207 mapeia processos do ciclo de vida; CMMI foca maturidade/capacidade das práticas  
C) CMMI substitui 12207 em todo o mundo como notação gráfica  
D) Ambos são apenas nomes comerciais do Keycloak  
E) 12207 é o nível 5 obrigatório  

**8.** “Norma ABNT NBR ISO/IEC/IEEE que define processos de ciclo de vida de software.” Descreve:

A) BPMN 2  
B) Scrum Guide  
C) **ISO/IEC/IEEE 12207:2021**  
D) Dockerfile  
E) MapStruct  

**9.** No CMMI, a ideia de **nível 5** associa-se sobretudo a:

A) Trabalho ad hoc sem processo  
B) Melhoria contínua / otimização baseada em compreensão quantitativa (visão)  
C) Apenas o primeiro projeto da empresa  
D) Exclusivamente o uso de H2  
E) Substituição do PostgreSQL  

**10.** Sobre o domínio Development no CMMI v3.0, a leitura cobrável mais segura é:

A) É o nome do gateway paralelo BPMN  
B) É o domínio voltado às práticas de **desenvolvimento** de produtos/soluções  
C) É a escala G–A do MPS  
D) É o único processo da ISO 12207  
E) É o board Kanban físico  

**11.** O MPS-SW **não** se destina a:

A) Orientar melhoria de processo de software no contexto brasileiro  
B) Definir níveis de maturidade G a A  
C) Substituir a notação BPMN como único desenho de fluxo de negócio  
D) Dialogar com boas práticas internacionais (visão)  
E) Servir de referência cobrada no edital TRT4  

**12.** Em prova, confundir nível 3 CMMI com nível G MPS costuma errar porque:

A) Não há diferença de escala entre os modelos  
B) São escalas **diferentes** (1–5 vs G–A), ainda que conceitualmente correlacionáveis  
C) G é sempre idêntico a 5  
D) Ambos são eventos de timer  
E) ISO 12207 unifica os nomes em “lane”  

**13.** A frase mais alinhada ao papel da ISO 12207 no edital é:

A) “Define o board Scrum e os WIP limits”  
B) “Estabelece processos de ciclo de vida de software (visão normativa)”  
C) “É o dialect JPA do Hibernate”  
D) “Substitui OAuth2”  
E) “Só funciona com Eureka”  

**14.** Qual linha resume melhor o cartão mental deste subtópico?

A) CMMI = BPMN; MPS = UML; 12207 = Kanban  
B) CMMI Dev = maturidade 1–5; MPS-SW = G→A (BR); 12207 = processos de ciclo de vida  
C) Os três são a mesma escala com nomes trocados  
D) Todos eliminam requisitos e testes  
E) Todos são apenas backup de banco  

**15.** Afirmar que “atingir CMMI nível 2 é automaticamente o mesmo que MPS nível A” é:

A) Correto em toda prova FCC  
B) Incorreto: escalas e modelos diferem; correspondências são aproximadas/mapeadas, não identidade automática  
C) Correto somente com H2  
D) Correto porque ambos são BPMN  
E) Correto se houver gateway XOR  

---

### Gabarito comentado (treino 1–15)

1. **B** — Capacidade/maturidade Dev.  
2. **C** — Nível 3 = Definido.  
3. **B** — Escalas corretas por modelo.  
4. **B** — Modelo BR SOFTEX 2024.  
5. **B** — G→A crescente.  
6. **C** — BPMN ≠ escala CMMI.  
7. **B** — Processos × maturidade.  
8. **C** — 12207:2021.  
9. **B** — Otimização / nível 5.  
10. **B** — Domínio Development.  
11. **C** — Não substitui BPMN.  
12. **B** — Escalas distintas.  
13. **B** — Ciclo de vida / processos.  
14. **B** — Triangulação do arquivo.  
15. **B** — Sem identidade automática de níveis.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto do processo de desenvolvimento de software previsto no edital, o CMMI v3.0 — domínio Development — caracteriza-se, em regra, por:

A) fornecer exclusivamente a notação de pools e lanes de processo de negócio  
B) reunir práticas e níveis orientados à capacidade/maturidade no desenvolvimento  
C) substituir, por si só, a LGPD como norma de privacidade  
D) definir os níveis G a A do MR-MPS-SW como numeração oficial do CMMI  
E) documentar apenas o diagrama entidade-relacionamento  

**17.** Relativamente ao MR-MPS-SW versão 2024, assinale a opção correta:

A) Adota a escala de maturidade G a A, sendo A o nível mais avançado  
B) É idêntico, termo a termo, à ISO 12207 sem qualquer diferença de propósito  
C) Substitui a UML 2.5.1 em diagramas de classe  
D) É o nome comercial do Docker Hub  
E) Possui apenas os níveis 4 e 5 da ITIL  

**18.** Comparando a ISO/IEC/IEEE 12207:2021 com modelos de maturidade, é correto afirmar que:

A) a 12207 é, por definição, o nível 5 do CMMI  
B) a 12207 descreve processos de ciclo de vida, ao passo que CMMI/MPS tratam de maturidade/capacidade  
C) a 12207 elimina a necessidade de requisitos  
D) a 12207 é a escala G–A renomeada  
E) a 12207 é apenas um gateway inclusivo  

**19.** Assinale a alternativa que apresenta associação **incorreta**:

A) CMMI Dev — domínio de desenvolvimento / maturidade-capacidade  
B) MPS-SW 2024 — modelo brasileiro de software (G–A)  
C) ISO 12207 — processos de ciclo de vida  
D) CMMI nível 1 — tipicamente ad hoc / inicial  
E) ISO 12207 — escala obrigatória G–A do MPS.BR  

**20.** Em enunciado que contraste CMMI e MPS-SW, a FCC tipicamente considera **correto** que:

A) Ambos usam exatamente os mesmos rótulos de nível (1–5 = G–A sem mapeamento)  
B) São modelos distintos (internacional × brasileiro), correlacionáveis, com escalas próprias  
C) MPS-SW é apenas o diagrama de sequência UML  
D) CMMI é exclusivo de redes Wi-Fi  
E) Nenhum dos dois se relaciona a processo de software  

**21.** A expressão “processos de acordo, organizacionais, de gestão e técnicos no ciclo de vida de software” descreve com mais precisão o propósito da:

A) Notação BPMN exclusiva  
B) **ISO/IEC/IEEE 12207** (visão)  
C) Board Kanban físico  
D) Image Docker  
E) Token OAuth2  

**22.** Em organização que adota práticas ágeis, o uso de Scrum **não** implica, por si só:

A) possibilidade de planejar e entregar em iterações  
B) certificação automática em CMMI nível 5 ou MPS nível A  
C) colaboração do time com Product Owner e eventos do framework  
D) inspeção e adaptação  
E) foco em incremento de valor  

**23.** Considere as afirmações:

I. O nível G do MPS-SW é mais maduro que o nível A.  
II. O CMMI Dev e o MPS-SW podem ser correlacionados, mas não são o mesmo modelo.  
III. A ISO 12207 não substitui, por si só, um modelo de maturidade G–A ou 1–5.

Assinale:

A) Apenas I  
B) Apenas II  
C) II e III  
D) I e II  
E) I, II e III  

**24.** Diferenciar capacidade e maturidade no discurso CMMI é relevante porque:

A) não há qualquer diferença conceitual cobrável  
B) capacidade tende a olhar práticas/áreas; maturidade, o estágio organizacional agregado (visão)  
C) maturidade só existe no BPMN  
D) capacidade é exclusividade do RabbitMQ  
E) ambos são apenas anotações em diagrama ER  

**25.** A síntese mais alinhada ao edital, sobre o trio CMMI / MPS-SW / 12207, é:

A) Os três são notações gráficas idênticas ao BPMN  
B) CMMI Dev = maturidade/capacidade (1–5); MPS-SW 2024 = G→A (BR); 12207 = processos de ciclo de vida — complementares  
C) Todos obrigam stack apenas .NET e eliminam Java  
D) Todos substituem segurança, redes e SQL no edital  
E) Todos são nomes do API Gateway  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Práticas + maturidade/capacidade Dev.  
17. **A** — G–A; A no topo.  
18. **B** — Processos × maturidade.  
19. **E** — 12207 ≠ escala G–A.  
20. **B** — Modelos distintos correlacionáveis.  
21. **B** — 12207 = ciclo de vida.  
22. **B** — Ágil ≠ maturidade automática.  
23. **C** — I falsa (G < A); II e III verdadeiras.  
24. **B** — Capability × maturity.  
25. **B** — Cartão mental do arquivo 05.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **nível 1–5 / capacidade / Development**? → **CMMI**.  
2. É **nível G–A / SOFTEX / modelo brasileiro de software**? → **MPS-SW**.  
3. É **processos de ciclo de vida / aquisição / V&V / norma ISO**? → **12207**.  
4. É **desenho gráfico de processo de negócio**? → **BPMN** (não estes três).  
5. É **diagrama de software (classes, casos de uso…)**? → **UML**.  
6. É **como o time itera no dia a dia**? → **Ágil** (Scrum/Kanban/XP) — não confunda com maturidade.  
7. Adotou Scrum? → **não** marca sozinho CMMI 5 / MPS A.

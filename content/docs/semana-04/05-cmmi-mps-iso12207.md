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

## 3. Questões de concurso (16–25)

> **Nota:** questões **reais** de concurso (preferência FCC / TRT Analista TI), com banca, órgão e ano citados. Treino 1–15 permanece estilo didático.

**16.** (FCC – TRT-15/2025 – Analista Judiciário – Tecnologia da Informação)

A equipe de desenvolvimento de um Tribunal Regional do Trabalho está se orientando pelo CMMI-DEV v2.0 para melhorar a maturidade dos processos de desenvolvimento. Uma das Analistas de TI verificou, corretamente, que a área de capacidade (Capability Area) Engineering & Developing Products (EDP) inclui as áreas de prática (Practices Areas)

A) Technical Solution e Peer Review.
B) Estimating, Planning e Product Integration.
C) Technical Solution e Product Integration.
D) Process Quality Assurance, Risk and Oportunity Management e Process Management.
E) Monitor and Control e Governance.

**17.** (FCC – TRT-20/2024 – Analista Judiciário – Tecnologia da Informação)

Considere, por hipótese, que um Tribunal está adotando o CMMI-DEV v2.0. Para aumentar a previsibilidade dos resultados dos projetos, reduzir o retrabalho e garantir que os requisitos do cliente sejam cumpridos de forma mais eficiente, é necessário, dentre outras ações,

A) estabelecer um controle rigoroso de qualidade com foco em auditorias periódicas nos processos, utilizando a área de prática IMP - Improving Performance da Área de Capacitação PCM - Process Management, para garantir que todas as mudanças sejam aprovadas e documentadas formalmente.
B) priorizar a entrega rápida de funcionalidades, utilizando as áreas de prática MPM - Managing Performance & Measurement e PCM - Process Management da Área de Capacitação PMW - Planning & Managing Work, para aumentar a satisfação do cliente e reduzir o tempo de mercado (time to market).
C) investir em treinamentos intensivos de desenvolvimento ágil para toda a equipe e implementar o Scrum como principal metodologia, como recomenda a área de prática OT - Organizational Training da Área de Capacitação ENQ - Ensuring Quality.
D) adotar as áreas de prática CAR - Causal Analysis and Resolution da Área de Capacitação SI - Supporting Implementation e RDM - Requirements Development and Management da Área de Capacitação ENQ - Ensuring Quality, que contribuirão para identificação das causas raízes dos defeitos e cumprimento dos requisitos do cliente de forma eficiente e eficaz.
E) concentrar todos os esforços na automação de testes de software para reduzir defeitos, conforme recomenda a área de prática VAL - Validation da Área de Capacitação PMW - Planning & Managing Work, para garantir que o produto final atenda aos requisitos dos clientes.

**18.** (FCC – TRT-15/2023 – Analista Judiciário – Tecnologia da Informação)

Considere, por hipótese, que o Analista do Tribunal Regional do Trabalho está trabalhando na melhoria de processos orientado pelo guia MR-MPS-SW e sabe que a implementação do processo Gerência de Projetos - GPR inicia no nível de maturidade G e evolui em alguns dos níveis seguintes e que, na evolução atingida a partir do nível

A) B (GPR20), as técnicas estatísticas e quantitativas são utilizadas para definir e manter as atividades do projeto alinhadas aos objetivos de qualidade e desempenho definidos para o projeto.
B) E (GPR22), as avaliações de soluções são realizadas utilizando técnicas estatísticas e quantitativas para determinar se a solução pode ser aplicada na organização.
C) C (GPR14), os resultados positivos ou negativos significativos do projeto são analisados e tratados em relação à causa-raiz, utilizando um procedimento organizacional e documentando os seus resultados.
D) A (GPR19), os resultados atrasados do projeto são analisados e tratados em relação à causa-raiz, utilizando um procedimento de auditoria e revendo os seus resultados.
E) D (GPR15), o processo definido para o projeto, derivado da estratégia para adaptação do processo da organização, é estabelecido, mantido atualizado e utilizado.

**19.** (FCC – TRT-14/2016 – Analista Judiciário – Tecnologia da Informação)

O modelo de qualidade de software MPS.BR

A) não é compatível com a Norma NBR ISO/IEC 12207:2009.
B) adota a ITIL v3 como padrão de gerenciamento da qualidade de serviços de TI.
C) não pode ser aplicado em nenhum Tribunal Regional do Trabalho.
D) não atende às necessidades das empresas brasileiras, que preferem a adoção do CMMI.
E) não conflita com as metodologias ágeis de software, podendo ser utilizado junto com o Scrum.

**20.** (FCC – TRT-22/2010 – Analista Judiciário – Tecnologia da Informação)

NÃO se trata de um nível de maturidade adotado no MPS.Br, o

A) Definido.
B) Gerenciado.
C) Em Otimização.
D) Largamente Definido.
E) Executado.

**21.** (FCC – TRT-15/2025 – Analista Judiciário – Tecnologia da Informação)

Uma Analista de TI está revisando seus processos de desenvolvimento de software para alinhá-los à norma ABNT NBR ISO/IEC/IEEE 12207:2021. Durante a análise, ela identificou a necessidade de implementar processos para assegurar a qualidade do produto final. Um dos processos primários (ou fundamentais) conforme descrito na norma ABNT NBR ISO/IEC/IEEE 12207:2021 é o Processo de

A) Auditoria de Software.
B) Avaliação de Fornecedores.
C) Gestão de Configuração.
D) Desenvolvimento.
E) Engenharia de Sistemas.

**22.** (FCC – TRT-6/2025 – Analista Judiciário – Tecnologia da Informação)

De acordo com a Norma ABNT NBR ISO/IEC/IEEE 12207:2021, as atividades que podem ser executadas durante o ciclo de vida de um sistema de software são agrupadas em

A) diversos grupos de processos, dentre os quais encontram-se os processos de acordo, que transformam as necessidades dos stakeholders em produtos e serviços.
B) 4 grupos de processos, dentre os quais encontram-se os processos de gerenciamento técnico, que estão relacionados ao esforço técnico dos projetos, em particular ao planejamento de custos, prazos e compromissos.
C) 4 grupos de processos, dentro dos quais encontram-se 36 processos, que a Norma considera suficientes, desencorajando o uso de processos adicionais.
D) diversos grupos de processos, dentre os quais se encontram os processos técnicos, que estão relacionados ao nível estratégico com a gestão e melhoria dos negócios, visando atender as necessidades dos stakeholders da organização.
E) 6 grupos de processos, com destaque aos processos organizacionais habilitadores de projeto, que têm aplicação tanto fora do ciclo de vida do projeto quanto ao longo de sua vida útil.

**23.** (FCC – TRT-19/2022 – Analista Judiciário – Tecnologia da Informação)

No processo técnico de definição dos requisitos de sistema/software, um resultado esperado, segundo a norma ABNT NBR ISO/IEC/IEEE 12207:2021, é que

A) as necessidades dos stakeholders sejam definidas.
B) as medidas críticas de desempenho sejam definidas.
C) todas as restrições do sistema sejam identificadas pelos stakeholders.
D) a rastreabilidade dos requisitos dos stakeholders para suas necessidades sejam estabelecidas.
E) os elementos do sistema e suas interfaces sejam identificados e construídos.

**24.** (FCC – TRT-1/2025 – Analista Judiciário – Tecnologia da Informação)

No contexto do processo de desenvolvimento de software, diferentes frameworks, normas e metodologias podem ser aplicados para garantir qualidade, conformidade e eficiência, como

A) o CMMI e o MR-MPS-SW, que não recomendam o desenvolvimento low-code e no-code, uma vez que estes podem comprometer a qualidade do software.
B) a notação BPMN, que substituiu a UML 2.5 por ter sido descontinuada em processos de Engenharia de Software modernos, independentemente do domínio de aplicação do sistema em desenvolvimento.
C) o MR-MPS-SW 2023, que eliminou a necessidade do processo Engenharia de Requisitos do nível G, uma vez que essa responsabilidade foi transferida para o processo Projeto e Construção Ágil do Produto do nível F, que preconiza o uso de metodologias como Scrum e XP.
D) o CMMI-DEV v2.0, no qual as práticas de Engenharia de Software são abordadas de forma independente dos níveis de maturidade organizacional, permitindo que uma organização alcance a certificação em um único processo de avaliação direta.
E) a norma ABNT NBR ISO/IEC/IEEE 12207:2021, que define que o processo de definição de requisitos de sistema/software tem como objetivo transformar as capacidades desejadas dos stakeholders, visão orientada ao usuário, em uma visão técnica que atenda às necessidades operacionais do usuário.

**25.** (FCC – TRT-20/2024 – Analista Judiciário – Tecnologia da Informação)

De acordo com o MR-MPS-SW: 2023, que é compatível com o CMMI-DEV v2.0, o propósito do processo REQ - Engenharia de Requisitos é definir, gerenciar e manter atualizados os requisitos das partes interessadas e do produto, garantindo que inconsistências entre os requisitos, os planos e os produtos de trabalho sejam identificadas e tratadas. Os resultados esperados, a partir de nível D, incluem que

A) as necessidades, expectativas e restrições das partes interessadas ligadas à área de TI, tanto em relação ao produto quanto a suas interfaces, sejam identificadas.
B) os requisitos sejam validados e que a validação possa ser realizada, entre outras formas, com uso de protótipos, demonstrações ou revisões.
C) os planos, as atividades e os requisitos de trabalho relacionados sejam revisados.
D) as avaliações de soluções sejam realizadas utilizando técnicas estatísticas e quantitativas para determinar se a solução pode ser aplicada na organização.
E) os recursos materiais e o ambiente de trabalho necessários para executar o projeto e seus requisitos sejam estabelecidos e mantidos atualizados.

---

### Gabarito comentado (concurso 16–25)

| Questão | Gabarito | Fonte/nota |
|---------|----------|------------|
| 16 | **C** | Quizinho TRT-15 2025/2024 Q55 — EDP |
| 17 | **D** | Quizinho TRT-20 2024 Q23 |
| 18 | **A** | Quizinho TRT-15 2023 Q52 |
| 19 | **E** | EstudeGrátis TRT-14 2016 |
| 20 | **E** | EstudeGrátis/PDF TRT-22 2010 — Executado |
| 21 | **D** | Quizinho TRT-15 2025/2024 Q56 |
| 22 | **B** | Quizinho TRT-6 2025 Q48 |
| 23 | **B** | Quizinho TRT-19 2022 Q35 |
| 24 | **E** | Quizinho TRT-1 2025 Q32 |
| 25 | **B** | Quizinho TRT-20 2024 Q21 |

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **nível 1–5 / capacidade / Development**? → **CMMI**.  
2. É **nível G–A / SOFTEX / modelo brasileiro de software**? → **MPS-SW**.  
3. É **processos de ciclo de vida / aquisição / V&V / norma ISO**? → **12207**.  
4. É **desenho gráfico de processo de negócio**? → **BPMN** (não estes três).  
5. É **diagrama de software (classes, casos de uso…)**? → **UML**.  
6. É **como o time itera no dia a dia**? → **Ágil** (Scrum/Kanban/XP) — não confunda com maturidade.  
7. Adotou Scrum? → **não** marca sozinho CMMI 5 / MPS A.

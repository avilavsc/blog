+++
title = '01 — Ágeis: Scrum / Kanban / XP'
date = '2026-10-15'
draft = false
weight = 1
tags = ['semana-04', 'ageis', 'scrum']
categories = ['estudo']
+++
# Ágeis: Scrum / Kanban / XP · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Métodos Ágeis (Scrum, Kanban, XP etc.)  
**Escopo deste arquivo:** Manifesto Ágil (valores/princípios cobráveis), Scrum (papéis, eventos, artefatos, Definition of Done), Kanban (fluxo, WIP, pull), XP (práticas clássicas), contraste ágil × cascata e entre os três métodos.  
**Fora de escopo aqui:** Engenharia de requisitos detalhada (arquivo 02); UML/BPMN (03–04); CMMI/MPS/ISO 12207 (05); low-code/IA (06); ITIL/COBIT/PMBOK/Governança TRT4 (07–10); legislação (11).

---

## 1. Material

### 1.1 Por que isso cai no edital

No edital (Processo de desenvolvimento de software), aparecem explicitamente: **Métodos Ágeis (Scrum, Kanban, XP etc.)**, ao lado de requisitos, UML, BPMN, CMMI Dev, MPS-SW e ISO 12207.

Leitura cobrável:

| Peça | Papel em prova |
|------|----------------|
| **Manifesto Ágil** | Valores e princípios que fundamentam os métodos |
| **Scrum** | Framework com papéis, eventos e artefatos bem delimitados |
| **Kanban** | Gestão visual de fluxo; limite de WIP; pull |
| **XP** | Práticas técnicas intensivas (pares, TDD, integração contínua…) |

Flash: a FCC gosta de **misturar** papéis do Scrum com práticas do XP e métricas do Kanban — saber o “dono” de cada conceito evita a pegadinha.

### 1.2 Manifesto Ágil — âncora mental

Quatro valores (lado esquerdo valorizado **mais** que o direito, sem anular o direito):

| Valorizamos mais | Do que |
|------------------|--------|
| **Indivíduos e interações** | Processos e ferramentas |
| **Software em funcionamento** | Documentação abrangente |
| **Colaboração com o cliente** | Negociação contratual |
| **Responder a mudanças** | Seguir um plano |

Armadilha: dizer que ágil **proíbe** documentação, contrato ou plano. Valoriza o lado esquerdo **mais**; o direito ainda existe.

Princípios cobráveis (amostra de prova):

1. Entregar software funcionando **cedo e continuamente**.  
2. Mudanças de requisito são **bem-vindas**, mesmo tardiamente.  
3. Entregar com frequência (semanas, não meses).  
4. Pessoas de negócio e desenvolvedores **juntos**, diariamente.  
5. Projetos em torno de indivíduos **motivados**.  
6. Conversação **face a face** (melhor canal).  
7. Software funcionando = medida principal de progresso.  
8. Ritmo sustentável (desenvolvimento contínuo sem burnout).  
9. Excelência técnica e bom design.  
10. Simplicidade (arte de maximizar o trabalho **não** feito).  
11. Equipes **auto-organizadas**.  
12. Reflexão regular para ajustar o comportamento (inspect & adapt).

### 1.3 Ágil × cascata (visão de prova)

| | Cascata (waterfall) | Ágil |
|---|---------------------|------|
| Ciclo | Fases sequenciais longas | Iterações/incrementos curtos |
| Feedback | Tarde (após grandes fases) | Cedo e frequente |
| Mudança | Cara / resistida no fim | Esperada e acomodada |
| Entrega | Grande “big bang” | Incrementos utilizáveis |
| Documentação | Ênfase forte upfront | Just enough + software rodando |

Ágil **não** é “sem processo” nem “sem disciplina”. Scrum, Kanban e XP têm regras e práticas claras.

### 1.4 Scrum — visão geral

**Scrum** = framework leve para desenvolver, entregar e sustentar produtos complexos, baseado em empirismo (**transparência**, **inspeção**, **adaptação**) e auto-organização.

Peças cobráveis:

| Categoria | Elementos |
|-----------|-----------|
| **Papéis (accountabilities)** | Product Owner, Scrum Master, Developers |
| **Eventos** | Sprint, Sprint Planning, Daily Scrum, Sprint Review, Sprint Retrospective |
| **Artefatos** | Product Backlog, Sprint Backlog, Increment |
| **Compromissos** | Product Goal, Sprint Goal, Definition of Done |

Flash: Scrum **não** é metodologia completa de engenharia (não manda TDD ou pair programming — isso é XP). Scrum define **o que** organizar em torno do produto; práticas técnicas vêm à parte.

### 1.5 Papéis do Scrum

| Papel | Responsabilidade cobrável |
|-------|---------------------------|
| **Product Owner (PO)** | Maximizar valor do produto; **gerencia** o Product Backlog (ordenar, clarificar, visível); única pessoa responsável pelo backlog (visão clássica de prova) |
| **Scrum Master (SM)** | Servir o time e a organização; **facilitar** eventos; remover impedimentos; ensinar/coach Scrum; proteger o time de interferências indevidas |
| **Developers** | Entregar o Increment pronto a cada Sprint; estimam e puxam trabalho; são multidisciplinares; **não** há subpapéis hierárquicos “programador vs tester” oficiais no Scrum |

Armadilhas clássicas:

- PO **não** é “chefe do time” no sentido de gerenciar pessoas do RH.  
- SM **não** é “gerente de projeto tradicional” nem dono do backlog.  
- Developers **incluem** quem contribui para o Increment (dev, QA, etc.), sem cargos oficiais separados no framework.

### 1.6 Sprint e eventos

**Sprint** = coração do Scrum: container de tempo fixo (**time-box**, tipicamente ≤ 1 mês; comum 1–2 semanas) em que se cria um Increment utilizável.

Durante a Sprint:

- Não se muda o Sprint Goal de forma a invalidá-lo.  
- Qualidade não cai.  
- Product Backlog pode ser refinado.  
- Escopo pode ser renegociado entre PO e Developers conforme aprendizado (visão Scrum Guide).

| Evento | Time-box típico (visão) | Propósito |
|--------|-------------------------|-----------|
| **Sprint Planning** | Até 8 h para Sprint de 1 mês (proporcional) | O que entregar e como; nasce Sprint Goal + Sprint Backlog |
| **Daily Scrum** | 15 min | Inspect do progresso **rumo ao Sprint Goal**; plano das próximas 24 h; só Developers (SM facilita se pedido) |
| **Sprint Review** | Até 4 h (Sprint 1 mês) | Inspecionar o Increment; feedback de stakeholders; adaptar Product Backlog |
| **Sprint Retrospective** | Até 3 h (Sprint 1 mês) | Inspect do processo/pessoas/relações/ferramentas; plano de melhoria para a próxima Sprint |

Ordem mental: Planning → (Dailies) → Review → Retrospective → próxima Sprint.

Contraste Review × Retrospective:

| | Sprint Review | Sprint Retrospective |
|---|---------------|----------------------|
| Foco | **Produto** (Increment) + stakeholders | **Processo** da equipe |
| Pergunta | O que entregamos / o que vem a seguir? | Como trabalhamos melhor? |

### 1.7 Artefatos e Definition of Done

| Artefato | Ideia | Compromisso associado |
|----------|-------|------------------------|
| **Product Backlog** | Lista ordenada de tudo que pode ser necessário no produto; único; evolutivo | **Product Goal** |
| **Sprint Backlog** | Itens selecionados da Sprint + plano para entregar o Increment + Sprint Goal | **Sprint Goal** |
| **Increment** | Soma de todos os itens Done na Sprint (e Sprints anteriores); utilizável | **Definition of Done (DoD)** |

**Definition of Done** = descrição formal do padrão de qualidade que o Increment deve atender para ser considerado pronto. Sem DoD cumprido, **não** é Increment “Done”.

**Product Backlog Refinement** (refinamento): atividade contínua de detalhar, estimar e ordenar itens — **não** é um quinto evento oficial com time-box fixo no Guide, mas é prática cobrada.

Estimativas: story points, planning poker — noção; o Guide atual enfatiza menos a métrica específica, mas a FCC ainda cobra “estimativa relativa / pontos”.

### 1.8 Kanban — fluxo e WIP

**Kanban** (no contexto de desenvolvimento) = método de gestão de fluxo de trabalho baseado em:

1. **Visualizar** o trabalho (quadro: To Do / Doing / Done — colunas customizáveis).  
2. **Limitar WIP** (Work In Progress) — quantos itens podem estar em cada coluna.  
3. **Gerenciar fluxo** (lead time, cycle time, throughput).  
4. Políticas explícitas, feedback loops, melhoria evolutiva.

Características cobráveis:

| Ideia | Leitura |
|-------|---------|
| **Pull** | Time puxa trabalho quando há capacidade (não “empurram” excesso) |
| **WIP limit** | Evita multitarefa e gargalos; acelera fluxo |
| **Cadência** | Em geral **não** exige Sprint time-boxed; entrega contínua / sob demanda |
| **Mudança** | Evolutiva: comece com o processo atual e melhore |

Kanban **≠** Scrum:

| | Scrum | Kanban |
|---|-------|--------|
| Iteração | Sprint com time-box | Fluxo contínuo (sem Sprint obrigatória) |
| Papéis | PO, SM, Developers | Não define esses papéis |
| Mudança de WIP | Itens tipicamente fechados na Sprint | WIP limitado por coluna; itens fluem |
| Métricas | Velocity (comum na prática) | Lead/cycle time, throughput |

Flash: dá para usar **Scrumban** (híbrido) — noção; em prova, se perguntarem o método “puro”, respeite as regras de cada um.

### 1.9 XP — Extreme Programming

**XP** = método ágil focado em **práticas de engenharia** e feedback técnico rápido, adequado a requisitos voláteis.

Valores clássicos do XP: comunicação, simplicidade, feedback, coragem, respeito.

Práticas cobráveis (núcleo de prova):

| Prática | Ideia |
|---------|--------|
| **Pair programming** | Dois no mesmo código (driver/navigator); revisão contínua |
| **TDD** (Test-Driven Development) | Teste → código → refactor (red-green-refactor) |
| **Integração contínua** | Integrar e testar frequentemente (várias vezes ao dia) |
| **Refactoring** | Melhorar design sem mudar comportamento observável |
| **Simple design** | Design mais simples que funcione agora |
| **Coding standards** | Código coletivo legível |
| **Collective code ownership** | Qualquer um pode alterar qualquer parte |
| **Sustainable pace** | Ritmo sustentável (sem overtime crônico) |
| **Customer on-site / close** | Cliente próximo / disponível |
| **Small releases** | Entregas pequenas e frequentes |
| **Metaphor / shared vision** | Linguagem comum do sistema (visão clássica) |
| **Spike** | Experimento time-boxed para reduzir incerteza (noção) |

Contraste com Scrum:

- Scrum **não** exige pair programming nem TDD.  
- XP **não** define Product Owner / Scrum Master da mesma forma.  
- Em times reais, Scrum + práticas XP é combinação frequente — em prova, separe a origem de cada regra.

### 1.10 User stories e critérios (visão ágil)

Embora detalhe de requisitos esteja no arquivo 02, a FCC mistura stories com Scrum:

| Conceito | Leitura rápida |
|----------|----------------|
| **User story** | Requisito em linguagem de usuário: “Como &lt;papel&gt;, quero &lt;ação&gt;, para &lt;benefício&gt;” |
| **Acceptance criteria** | Condições para aceitar a story |
| **INVEST** | Independent, Negotiable, Valuable, Estimable, Small, Testable (visão clássica) |
| **Épico** | Item grande depois decomposto |

Stories vivem tipicamente no **Product Backlog**; na Sprint entram as selecionadas (Sprint Backlog).

### 1.11 O que a FCC tipicamente testa neste tema

1. Quem gerencia o Product Backlog (PO).  
2. Diferença Review (produto) × Retrospective (processo).  
3. Daily = 15 min, foco no Sprint Goal — não “relatório ao chefe”.  
4. DoD × critérios de aceitação de uma story (DoD é do Increment/time; critérios são do item).  
5. Kanban = WIP + fluxo; Scrum = Sprint + papéis.  
6. Pair programming / TDD = XP, não Scrum “puro”.  
7. Manifesto: valorizar software funcionando **mais** que documentação — sem abolir documentação.  
8. Scrum Master remove impedimentos; não “manda” no backlog.

### 1.12 Mini-síntese (cartão mental)

> **Manifesto** = indivíduos, software rodando, colaboração, mudança · **Scrum** = PO/SM/Dev + Sprint + Backlogs/Increment + DoD · **Kanban** = quadro + WIP + pull + fluxo · **XP** = pares, TDD, CI, refactor, releases pequenas.

---

## 2. Questões de treino (1–15)

**1.** No edital de Analista Judiciário – TI do TRT4, Scrum, Kanban e XP aparecem corretamente associados a:

A) Métodos/frameworks no eixo de processo de desenvolvimento de software (métodos ágeis)  
B) Protocolos de rede equivalentes a TCP/IP  
C) Normas exclusivas de cabeamento estruturado  
D) Ferramentas de SSO equivalentes ao Keycloak  
E) Bancos relacionais substitutos do PostgreSQL  

**2.** Segundo o Manifesto Ágil, valoriza-se mais:

A) Software em funcionamento do que documentação abrangente  
B) Documentação abrangente do que qualquer software  
C) Seguir um plano a qualquer custo, sem responder a mudanças  
D) Negociação contratual acima de qualquer colaboração  
E) Processos e ferramentas acima de indivíduos  

**3.** No Scrum, o Product Owner é tipicamente responsável por:

A) Gerenciar o Product Backlog e maximizar o valor do produto  
B) Escrever todo o código da Sprint sozinho  
C) Substituir o Definition of Done por “commit sem teste”  
D) Ser o protocolo AMQP do RabbitMQ  
E) Administrar exclusivamente o cluster Kubernetes  

**4.** O Scrum Master, em visão de prova, caracteriza-se por:

A) Facilitar o Scrum, remover impedimentos e ajudar o time a aplicar o framework  
B) Ordenar sozinho o Product Backlog no lugar do PO  
C) Proibir Daily Scrum  
D) Ser o único desenvolvedor autorizado a fazer commit  
E) Equivaler ao papel de Product Owner  

**5.** A Daily Scrum tem, em regra, time-box de:

A) 15 minutos  
B) 8 horas fixas todos os dias  
C) 1 mês contínuo sem pausa  
D) 4 horas exclusivas com todos os stakeholders externos  
E) Tempo ilimitado até esgotar o backlog  

**6.** A diferença mais útil entre Sprint Review e Sprint Retrospective é:

A) Review foca no Increment/produto com stakeholders; Retrospective no processo da equipe  
B) São eventos idênticos com nomes intercambiáveis  
C) Review só trata de políticas de RH; Retrospective só de firewall  
D) Retrospective entrega o Increment aos usuários finais  
E) Review proíbe qualquer feedback  

**7.** Definition of Done (DoD) refere-se a:

A) Padrão de qualidade que o Increment deve atender para ser considerado pronto  
B) Senha do banco PostgreSQL  
C) Tipo de exchange fanout do RabbitMQ  
D) Grant type do OAuth2  
E) Norma CNJ de sustentabilidade  

**8.** No Kanban aplicado a desenvolvimento, limitar WIP serve principalmente para:

A) Reduzir multitarefa e melhorar o fluxo (menos gargalo)  
B) Obrigar Sprint de exatamente 30 dias  
C) Criar os papéis PO e SM automaticamente  
D) Substituir o Product Backlog por um dump SQL  
E) Eliminar qualquer visualização do trabalho  

**9.** Em relação ao Scrum, o Kanban tipicamente:

A) Enfatiza fluxo contínuo e limites de WIP, sem exigir Sprint time-boxed nem os papéis Scrum  
B) Exige obrigatoriamente Product Owner, Scrum Master e Sprint Review  
C) Proíbe o uso de quadro visual  
D) É sinônimo estrito de waterfall  
E) Substitui toda integração contínua do XP  

**10.** Pair programming e TDD são práticas classicamente associadas a:

A) Extreme Programming (XP)  
B) Apenas ao COBIT 2019  
C) Exclusivamente ao ITIL 5  
D) Somente ao protocolo LDAP  
E) Apenas à norma de cabeamento  

**11.** No ciclo TDD (visão clássica), a ordem conceitual é:

A) Escrever o teste que falha → implementar o mínimo → refatorar  
B) Refatorar → publicar em produção sem teste → apagar o código  
C) Deploy → escrever requisitos em COBOL → pular testes  
D) Daily Scrum → Kanban WIP → Manifesto  
E) Product Goal → Sprint Goal → DoD apenas, sem testes  

**12.** Constitui confusão típica a ser evitada:

A) Atribuir ao Scrum Master a gestão do Product Backlog no lugar do Product Owner  
B) Distinguir Review (produto) de Retrospective (processo)  
C) Associar WIP ao Kanban  
D) Relacionar TDD ao XP  
E) Reconhecer time-box da Daily em 15 minutos  

**13.** “Software em funcionamento é a medida principal de progresso” aproxima-se de:

A) Princípio/valor do pensamento ágil (Manifesto)  
B) Regra exclusiva do protocolo SMTP  
C) Definição de API Gateway  
D) Política de firewall de borda  
E) Tipo de exchange headers do RabbitMQ  

**14.** No Scrum, o Increment:

A) É o resultado utilizável que atende à Definition of Done ao fim da Sprint (somando o Done anterior)  
B) É o nome do Scrum Master  
C) Substitui o Product Owner  
D) É um banco H2 obrigatório  
E) É a ferramenta Flyway  

**15.** A síntese mais adequada deste subtópico no edital é:

A) Manifesto Ágil + Scrum (papéis/eventos/artefatos) + Kanban (fluxo/WIP) + XP (práticas técnicas)  
B) Apenas redes e Wi-Fi  
C) Apenas interpretação de texto  
D) Apenas OAuth2 e Keycloak  
E) Apenas UML sem qualquer método ágil  

---

### Gabarito comentado (treino 1–15)

1. **A** — ágeis no eixo de processo de desenvolvimento do edital.  
2. **A** — software funcionando > documentação abrangente.  
3. **A** — PO gerencia Product Backlog e valor.  
4. **A** — SM facilita, remove impedimentos, coach Scrum.  
5. **A** — Daily = 15 min.  
6. **A** — Review = produto; Retro = processo.  
7. **A** — DoD = padrão de pronto do Increment.  
8. **A** — WIP limita sobrecarga e melhora fluxo.  
9. **A** — Kanban = fluxo/WIP; sem papéis/Sprint obrigatórios do Scrum.  
10. **A** — pair e TDD = XP.  
11. **A** — red → green → refactor.  
12. **A** — backlog é do PO, não do SM.  
13. **A** — princípio ágil / Manifesto.  
14. **A** — Increment Done e utilizável.  
15. **A** — bloco completo Scrum + Kanban + XP + Manifesto.

---

## 3. Questões de concurso (16–25)

> **Nota:** questões **reais** de concurso (preferência FCC / TRT Analista TI), com banca, órgão e ano citados. Treino 1–15 permanece estilo didático.

**16.** (FCC – TJ-CE/2022 – Analista Judiciário – Ciência da Computação / Sistemas da Informação)

Quanto à metodologia Scrum, considere as definições a seguir:
I. Time-boxed de um mês ou menos, durante o qual um “Pronto”, versão incremental potencialmente utilizável do produto, é criada.
II. Responsável por garantir que o Scrum seja entendido e aplicado, para garantir que o Time Scrum adira à teoria, práticas e regras do Scrum.
III. Responsável por maximizar o valor do produto e do trabalho do Time de Desenvolvimento.
IV. Um conjunto de itens selecionados, juntamente com o plano para entregar o incremento do produto e atingir o objetivo. Previsão do Time de Desenvolvimento sobre qual funcionalidade estará no próximo incremento e sobre o trabalho necessário para entregar essa funcionalidade em um incremento “Pronto”.
Os itens I, II, III e IV correspondem, correta e respectivamente, às definições de

A) Sprint, Scrum Master, Product Owner e Sprint Backlog.
B) Product Sprint, Product Owner, Scrum Master e Product Backlog.
C) Sprint, Senior Scrum, Product Owner e Product Backlog.
D) Backlog Review, Product Master, Scrum Owner e Sprint Backlog.
E) Product Backlog, Sprint Master, Scrum Master e Increment.

**17.** (FCC – TRF-3/2019 – Analista Judiciário – Informática)

A Reunião Diária do Scrum é

A) executada no final da Sprint para inspecionar o incremento e adaptar o Backlog do Produto, se necessário.
B) um time-boxed de 15 minutos, durante o qual um “Pronto”, versão incremental potencialmente utilizável do produto, é criado.
C) uma oportunidade para o Time Scrum inspecionar a si próprio e criar um plano para melhorias a serem aplicadas na próxima Sprint.
D) um time-boxed de 15 minutos, para que o Time de Desenvolvimento possa sincronizar as atividades e criar um plano para as próximas 24 horas.
E) um time-boxed de 60 minutos, durante o qual os produtos de uma Sprint são definidos.

**18.** (FCC – TRT-9/2013 – Analista Judiciário – Tecnologia da Informação)

Os modelos de processos tradicionais surgiram em um cenário muito diferente do atual, baseado em mainframes e terminais remotos. Já os modelos de processos ágeis são adequados para situações atuais nas quais a mudança de requisitos é frequente. Dentre os modelos de processos ágeis mais comuns temos: Extreme Programming (XP), Scrum e Feature Driven Development (FDD).

Algumas das práticas e características desses modelos de processo são descritas a seguir:
I. Programação em pares, ou seja, a implementação do código é feita em dupla.
II. Desenvolvimento dividido em ciclos iterativos de até 30 dias chamados de sprints.
III. Faz uso do teste de unidades como sua tática de testes primária.
IV. A atividade de levantamento de requisitos conduz à criação de um conjunto de histórias de usuários.
V. O ciclo de vida é baseado em três fases: pre-game phase, game-phase, post-game phase.
VI. Tem como único artefato de projeto os cartões CRC.
VII. Realiza reuniões diárias de acompanhamento de aproximadamente 15 minutos.
VIII. Define seis marcos durante o projeto e a implementação de uma funcionalidade: walkthroughs do projeto, projeto, inspeção do projeto, codificação, inspeção de código e progressão para construção.
IX. Os requisitos são descritos em um documento chamado backlog e são ordenados por prioridade.

A relação correta entre o modelo de processo ágil e a prática/característica é:

| | XP | Scrum | FDD |
|---|---|---|---|
| A | II, V e VII | II, IV e VIII | VII e IX |
| B | I, III, IV e VI | II, V, VII e IX | VIII |
| C | II, VII e VIII | III, IV, VI e IX | I e V |
| D | II, VII e VIII | I, III, IV e V | VI e IX |
| E | I, III, IV e VI | II, VIII, VII e IX | V |

**19.** (FCC – TRT-24/2017 – Técnico Judiciário – Tecnologia da Informação)

Uma metodologia de desenvolvimento, ou combinação de metodologias, e a sequência de atividades ou componentes pertencentes a esta metodologia é corretamente indicada em:

A) Scrum e XP: Sprint Backlog – Sprint Planning – TDD – Reunião em pé – Classe proprietária.
B) RUP: Iniciação – Metáfora – Transição – Construção – Refatoração.
C) FDD: Lista de Features – Planejamento – Modelo de domínio – Detalhamento da Feature – Propriedade coletiva.
D) Scrum: Product Backlog – Sprint Planning – Sprint Backlog – Daily Scrum – Sprint Review.
E) TDD: Red Green Refactoring – Feedback – Group Responsibility Principle – Product Owner.

**20.** (FCC – TRT-20/2024 – Técnico Judiciário – Tecnologia da Informação)

A equipe de um Tribunal está debatendo a adequação de metodologias ágeis, considerando a natureza dos diversos projetos institucionais. Um Técnico de TI afirmou que a metodologia

A) Scrum é melhor em projetos que requerem a auto-organização da equipe e iterações regulares, permitindo que os desenvolvedores façam ajustes nas prioridades e no escopo do projeto a cada Sprint. Além disso, o Scrum se concentra na implementação de um ciclo de desenvolvimento linear e sequencial, mais próximo da realidade.
B) XP deve ser escolhida na maior parte dos projetos, pois permite um planejamento detalhado a longo prazo, com entregas frequentes em sprints de 3-4 semanas ou um mês. Além disso, como a colaboração com o cliente é fundamental, essa metodologia incentiva reuniões de revisão mensais com as partes interessadas.
C) XP é mais indicada quando há alta necessidade de refatoração contínua e testes automatizados, o que permite melhor controle da qualidade do código durante o desenvolvimento.
D) XP deve ser escolhida quando o time deseja trabalhar sem sprints fixos e priorizar documentações mais detalhadas e extensas. Além disso, projetos com baixa variabilidade de requisitos se beneficiam com as práticas XP.
E) Scrum deve ser preferida em projetos em que há necessidade de ciclos curtos com entregas de incremento de software a cada semana, garantindo uma integração contínua do produto.

**21.** (FCC – SEFAZ-BA/2019 – Auditor Fiscal – Tecnologia da Informação – Prova II)

Scrum e XP são metodologias ágeis usadas no contexto de desenvolvimento de software que

A) podem ser usadas em conjunto, criando uma metodologia híbrida própria, de forma a unir as práticas de gestão do Scrum com as práticas de desenvolvimento da XP.
B) utilizam reuniões diárias (Daily Meetings) de 15 minutos em pé para ajudar a tornar as reuniões mais produtivas, colaborativas e úteis.
C) podem ser usadas juntas, desde que em um contexto em que o gerenciamento do projeto seja realizado por um framework como o PMBoK.
D) não podem ser usadas em conjunto por serem ambas voltadas para a gestão do processo de desenvolvimento de software.
E) podem ser usadas em conjunto, já que a XP foca mais na gestão do processo de desenvolvimento e a Scrum, no desenvolvimento propriamente dito.

**22.** (FCC – PGE-AM/2022 – Técnico em Gestão Procuratorial – Engenharia de Software)

No Scrum, os eventos são usados para criar regularidade e minimizar a necessidade de reuniões não definidas. Considere as atividades e/ou objetivos dos eventos do Scrum:
I. O propósito deste evento é inspecionar o resultado da Sprint e determinar as adaptações futuras. O Scrum Team apresenta os resultados de seu trabalho para os principais stakeholders e o progresso em direção à Meta do Produto é discutido.
II. O propósito deste evento é inspecionar o progresso em direção à Meta da Sprint e adaptar o Sprint Backlog conforme necessário, ajustando o próximo trabalho planejado. Este evento melhora as comunicações, identifica os impedimentos e promove a rápida tomada de decisões.
III. O Scrum Team inspeciona como foi a última Sprint em relação a indivíduos, interações, processos, ferramentas e sua definição de Pronto. As suposições que os desviaram são identificadas e suas origens exploradas. O Scrum Team discute o que deu certo durante a Sprint, quais problemas encontraram e como esses problemas foram (ou não) resolvidos.
IV. O Product Owner propõe como o produto pode aumentar seu valor e utilidade na Sprint atual. Todo o Scrum Team colabora para definir uma Meta da Sprint. A meta da Sprint deve ser finalizada antes do final deste evento.
A associação dos itens de I a IV com os eventos Scrum é, correta e respectivamente,

A) Sprint Review − Daily Scrum − Sprint Retrospective − Sprint Planning.
B) Daily Scrum − Sprint Planning − Sprint Review − Sprint Retrospective.
C) Sprint Retrospective − Daily Scrum − Sprint Review − Sprint Planning.
D) Sprint Review − Sprint Planning − Sprint Retrospective − Daily Scrum.
E) Sprint Retrospective − Sprint Planning − Sprint Review − Daily Scrum.

**23.** (FCC – TJ-CE/2026 – Analista Judiciário – Sistemas da Informação)

Uma equipe de analistas de um Tribunal de Justiça decidiu avaliar a eficiência do fluxo de processamento de recursos utilizando métricas ágeis adaptadas à gestão processual. Durante um período de 20 dias úteis, foram coletados os seguintes dados:
No 1º dia do período havia 40 recursos aguardando análise inicial.
Ao longo do período, ingressaram mais 120 recursos.
No 20º dia do período havia 60 recursos ainda não concluídos.
Durante o período 100 recursos foram concluídos.
A análise do fluxo mostrou que, em média, havia 50 recursos simultaneamente em tramitação (Work in Progress – WIP).
Nesse cenário, o throughput médio foi de

A) 2,5 recursos por dia útil, o Cycle Time médio foi de aproximadamente 8 dias e o CFD indicaria estreitamento das faixas entre estados do fluxo, caracterizando crescimento do WIP médio.
B) 5 recursos por dia útil, o Lead Time médio foi de aproximadamente 10 dias e o CFD indicaria aumento do número de recursos em tramitação ao longo do período analisado.
C) 6 recursos por dia útil, o Lead Time médio foi de aproximadamente 8,3 dias e o CFD indicaria tendência de redução do número de recursos em tramitação ao longo do período.
D) 5 recursos por dia útil, o Cycle Time médio foi de aproximadamente 12 dias e o CFD indicaria estabilidade do WIP, pois a diferença entre a entrada e a saída total de recursos foi pequena.
E) 4 recursos por dia útil, o Lead Time médio foi de aproximadamente 10 dias e o CFD indicaria aumento do número de recursos em tramitação decorrente da redução do fluxo de entrada.

**24.** (FCC – TJ-CE/2026 – Analista Judiciário – Sistemas da Informação)

Uma equipe de um Tribunal de Justiça precisa modelar o fluxo de cumprimento de mandados judiciais do sistema judicial. O processo se inicia com o recebimento do mandado pela secretaria, segue para distribuição ao oficial de justiça, inclui uma etapa de cumprimento externo e retorna ao magistrado, que decide entre homologar o cumprimento ou determinar nova diligência. Paralelamente, a equipe enfrenta acúmulo de tarefas e dificuldades relacionadas à qualidade de código ao implementar melhorias no sistema. Nesse contexto, a estratégia mais adequada é

A) representar o fluxo em BPMN com subprocessos de início, intermediários e de fim, bem como anotações para representar as decisões do magistrado e aplicar práticas Scrum com priorização do trabalho baseada em itens sequenciais do backlog definidos para cada Sprint.
B) modelar o fluxo em BPMN utilizando tarefas, gateways para decisões e eventos de mensagem para comunicação entre participantes e empregar Kanban para visualizar o fluxo, com práticas Scrum voltadas à integração frequente e melhoria contínua do código.
C) representar o fluxo usando um processo em BPMN utilizando eventos e atividades para indicar comunicações e decisões entre participantes e aplicar práticas Scrum com limites de WIP baseados no tempo previsto no fluxo modelado.
D) modelar o fluxo em BPMN por meio de tarefas e eventos de mensagem entre participantes representados em diferentes diagramas de coreografia e adotar práticas Scrum voltadas às entregas incrementais em Sprints de 40 dias e à entrega contínua de funcionalidades.
E) utilizar BPMN com processos e subprocessos colapsados para evidenciar as interações entre secretaria, oficial de justiça e magistrado e aplicar Kanban com limites de WIP definidos a partir da quantidade de participantes envolvidos em cada etapa do processo.

**25.** (FCC – TRT-14/2016 – Analista Judiciário – Tecnologia da Informação)

O modelo de qualidade de software MPS.BR

A) não é compatível com a Norma NBR ISO/IEC 12207:2009.
B) adota a ITIL v3 como padrão de gerenciamento da qualidade de serviços de TI.
C) não pode ser aplicado em nenhum Tribunal Regional do Trabalho.
D) não atende às necessidades das empresas brasileiras, que preferem a adoção do CMMI.
E) não conflita com as metodologias ágeis de software, podendo ser utilizado junto com o Scrum.

---

### Gabarito comentado (concurso 16–25)

| Questão | Gabarito | Fonte/nota |
|---------|----------|------------|
| 16 | **A** | Quizinho TJ-CE 2022 Q51 |
| 17 | **D** | Questionei TRF-3 2019 |
| 18 | **B** | Questionei TRT-9 2013 |
| 19 | **D** | Questionei TRT-24 2017 Téc. |
| 20 | **C** | Quizinho TRT-20 2024 Téc. Q32 |
| 21 | **A** | Questionei SEFAZ-BA 2019 |
| 22 | **A** | Quizinho PGE-AM 2022 Q28 |
| 23 | **B** | Questionei TJ-CE 2026 — TP=5; LT≈10 |
| 24 | **B** | QConcursos TJ-CE 2026 — BPMN+Kanban |
| 25 | **E** | EstudeGrátis TRT-14 2016 — MPS+Scrum |

---

*Arquivo 01 · Ágeis: Scrum / Kanban / XP · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 Requisitos / engenharia de software.*

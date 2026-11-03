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

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto dos métodos ágeis referidos no edital de Analista Judiciário – Área de TI, o Scrum caracteriza-se, em regra, por:

A) framework com papéis (Product Owner, Scrum Master e Developers), eventos time-boxed e artefatos (Product Backlog, Sprint Backlog e Increment), orientado a empirismo e entregas incrementais  
B) protocolo de roteamento IP idêntico ao OSPF  
C) substituição obrigatória de qualquer teste automatizado  
D) proibição absoluta de feedback de stakeholders  
E) identidade com o papel exclusivo de API Gateway  

**17.** Acerca do Kanban em desenvolvimento de software, é correto afirmar que:

A) enfatiza visualização do fluxo, limitação de trabalho em progresso (WIP) e puxada (pull), podendo operar em cadência contínua sem a obrigatoriedade de Sprints do Scrum  
B) exige, por definição, a existência formal de Product Owner e Scrum Master  
C) elimina qualquer necessidade de políticas explícitas de fluxo  
D) coincide termo a termo com o modelo em cascata clássico  
E) substitui integralmente a Definition of Done por dumps de banco  

**18.** No Extreme Programming (XP), práticas como programação em par e desenvolvimento orientado a testes (TDD):

A) integram o núcleo clássico de práticas técnicas do método, visando feedback rápido e qualidade do código  
B) são papéis oficiais do Scrum Guide com time-box de 8 horas diárias  
C) equivalem rigorosamente a objetivos de controle do COBIT 2019  
D) substituem a necessidade de qualquer colaboração com o cliente  
E) existem somente como dialetos SQL do H2  

**19.** Considere as afirmativas abaixo.

I. O Product Owner é o responsável por maximizar o valor do produto e gerenciar o Product Backlog.  
II. A Sprint Retrospective concentra-se na inspeção do Increment com stakeholders externos como foco principal.  
III. A Daily Scrum é um evento curto (tipicamente 15 minutos) para inspecionar o progresso em direção ao Sprint Goal.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e III, apenas  
D) II e III, apenas  
E) I, II e III  

**20.** Segundo a leitura cobrável do Manifesto para Desenvolvimento Ágil de Software:

A) valorizam-se indivíduos e interações, software em funcionamento, colaboração com o cliente e resposta a mudanças, em maior grau do que processos/ferramentas, documentação abrangente, negociação contratual e seguimento rígido a um plano, sem anular estes últimos  
B) a documentação é proibida em qualquer circunstância  
C) planos e contratos são abolidos por norma legal  
D) somente ferramentas importam, nunca pessoas  
E) mudanças de requisito são sempre recusadas após o dia zero  

**21.** Assinale a opção que diferencia corretamente critérios de aceitação de uma user story e Definition of Done:

A) critérios de aceitação dizem respeito às condições do item/story; a DoD é o padrão de qualidade do Increment/time que deve ser atendido para considerar o trabalho pronto  
B) DoD e critérios de aceitação são sempre a mesma lista idêntica em qualquer contexto  
C) DoD substitui o Product Owner  
D) critérios de aceitação são exclusivos do protocolo AMQP  
E) DoD existe apenas no Kanban e nunca no Scrum  

**22.** Constitui equívoco típico em prova afirmar que:

A) o Scrum Master facilita a adoção do Scrum e ajuda a remover impedimentos  
B) o Kanban utiliza limites de WIP para gerenciar fluxo  
C) TDD e pair programming são práticas clássicas de XP  
D) a Sprint Review inspeciona o Increment e coleta feedback  
E) o Scrum Master, e não o Product Owner, é o gestor natural do Product Backlog em qualquer formulação do framework  

**23.** Em um time que combina framework Scrum com práticas de XP, a interpretação mais adequada para fins de concurso é:

A) o Scrum organiza papéis, eventos e artefatos; práticas como TDD e programação em par podem ser adotadas como engenharia, sem confundir a origem de cada regra  
B) o Scrum Guide passa a exigir pair programming como evento oficial de 4 horas  
C) XP elimina Product Backlog e Sprint Goal por definição  
D) Kanban torna-se obrigatório e substitui a Daily  
E) Manifesto Ágil deixa de se aplicar  

**24.** No gerenciamento de fluxo Kanban, lead time e cycle time:

A) são métricas típicas associadas ao tempo de atravessamento do trabalho no sistema, úteis para inspecionar e melhorar o fluxo  
B) são papéis oficiais equivalentes a Product Owner e Scrum Master  
C) substituem a necessidade de visualizar o quadro  
D) equivalem exclusivamente a story points do Planning Poker no Scrum Guide  
E) existem somente na norma de cabeamento estruturado  

**25.** Para fins de preparação em Analista Judiciário – Área de TI (código 14), a síntese mais adequada deste subtópico é:

A) Manifesto Ágil; Scrum com PO/SM/Developers, Sprint e artefatos/DoD; Kanban com quadro, WIP e pull; XP com práticas técnicas (pares, TDD, CI, refactor); distinção clara entre os três  
B) apenas PMBOK 8, sem métodos ágeis  
C) apenas CNJ 400 e sustentabilidade  
D) apenas porcentagem e regra de três  
E) apenas Docker e Kubernetes  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **A** |
| 17 | **A** |
| 18 | **A** |
| 19 | **C** |
| 20 | **A** |
| 21 | **A** |
| 22 | **E** |
| 23 | **A** |
| 24 | **A** |
| 25 | **A** |

**Notas rápidas:** 16 A (framework Scrum completo); 17 A (Kanban = fluxo/WIP/pull); 18 A (XP = pares/TDD); 19 C (I e III; II confunde Review×Retro); 20 A (valores do Manifesto com nuance); 21 A (aceitação do item × DoD do Increment); 22 E (backlog é do PO); 23 A (Scrum + práticas XP sem misturar origem); 24 A (métricas de fluxo); 25 A (síntese do arquivo 01).

---

*Arquivo 01 · Ágeis: Scrum / Kanban / XP · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 Requisitos / engenharia de software.*

+++
title = '07 — ITIL Version 5'
date = '2026-10-21'
draft = false
weight = 7
tags = ['semana-04', 'itil', '5']
categories = ['estudo']
+++
# ITIL Version 5 · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 07 — ITIL Version 5 (gestão de serviços / produtos digitais)  
**Escopo deste arquivo:** o que o edital chama de **ITIL Version 5** — evolução PeopleCert (2026) sobre a base ITIL 4; **ITIL Value System**; **Product and Service Lifecycle Model** (8 atividades); **7 guiding principles**; dimensões; **34 practices** (com ênfase nas que a FCC mais cobra: incident, problem, change, service desk, request, SLM, continual improvement); contraste ITIL × COBIT; anti-confusão com PMBOK/ágeis.  
**Fora de escopo aqui:** memorizar as 34 practices linha a linha; simulados de certificação PeopleCert; COBIT detalhado (arquivo 08); Modelo de Governança TIC TRT4 em profundidade (outro material da semana 4); operação de ferramenta específica de ITSM.

> **Nota de edital:** o Anexo II traz *ITIL Version 5*. Em prova, retenha a nomenclatura do edital e o núcleo cobrável abaixo. Conceitos herdados do ITIL 4 (princípios, practices clássicas, valor) **continuam** válidos; o modelo operacional central na v5 é o **ciclo de vida de produto e serviço** (não a *service value chain* de 6 atividades da v4).

---

## 1. Material

### 1.1 O que é ITIL (definição cobrável)

**ITIL** (*Information Technology Infrastructure Library*) = conjunto de **boas práticas** para **gerenciamento de serviços** (e, na Version 5, de **produtos e serviços digitais**), orientado a **valor** para stakeholders.

| É | Não é |
|---|--------|
| Framework / biblioteca de práticas | Norma ISO obrigatória por si só |
| Foco em **como** entregar e operar serviços/produtos com valor | Framework de **governança corporativa de I&T** (isso é mais COBIT) |
| Adotável e adaptável | Receita única “copie 100% ou falha” |

Flash FCC: **ITIL = gestão de serviços (ITSM)**; **COBIT = governança e gestão de I&T** (guarda-chuva). Complementares.

### 1.2 De ITIL 4 para ITIL Version 5 (mapa rápido)

| Aspecto | ITIL 4 (herança) | ITIL Version 5 (edital) |
|---------|------------------|-------------------------|
| Escopo | IT service management | Digital **product and service** management |
| Sistema de valor | Service Value System (SVS) | **ITIL Value System** (produtos + serviços) |
| Modelo operacional | Service value chain (**6** atividades) | **Product and Service Lifecycle Model** (**8** atividades) |
| Princípios | 7 guiding principles | **Mantidos** |
| Practices | 34 (3 grupos) | **34** (reagrupadas em 2 grupos) |
| IA | Tópico emergente / HVIT | **AI-native** no desenho + governança de IA |

Leitura de prova: não “jogue fora” incident/problem/change — a FCC **adora** essas practices. Some o vocabulário de **ciclo de vida** e **produto + serviço**.

### 1.3 ITIL Value System (ideia)

O sistema de valor reúne elementos que, juntos, permitem cocriar valor:

- **Guiding principles** (orientação de decisão);
- **Governance** (direção e controle — no sentido ITIL, não o modelo COBIT inteiro);
- **Modelo operacional** (na v5: lifecycle de produto/serviço);
- **Practices** (capacidades organizacionais);
- **Continual improvement** (melhoria contínua atravessando o sistema).

Objetivo mental: entrada de demanda/oportunidade → trabalho guiado por práticas e lifecycle → **valor** (não “só ticket fechado”).

### 1.4 Sete guiding principles (decore)

1. **Focus on value** — foque no valor  
2. **Start where you are** — comece de onde está  
3. **Progress iteratively with feedback** — avance iterativamente com feedback  
4. **Collaborate and promote visibility** — colabore e promova visibilidade  
5. **Think and work holistically** — pense e trabalhe de forma holística  
6. **Keep it simple and practical** — mantenha simples e prático  
7. **Optimize and automate** — otimize e automatize  

Armadilha: trocar “optimize **and** automate” por “automatize primeiro sem otimizar” — a ordem mental é **otimizar**, depois automatizar o que faz sentido.

### 1.5 Quatro dimensões (produto e serviço)

1. **Organizations and people**  
2. **Information and technology**  
3. **Partners and suppliers**  
4. **Value streams and processes**

Holística: falha típica = só tecnologia, ignorando pessoas, fornecedores e fluxos.

### 1.6 Product and Service Lifecycle Model (8 atividades)

Modelo operacional central da Version 5:

| # | Atividade | Ideia cobrável |
|---|-----------|----------------|
| 1 | **Discover** | Explorar e priorizar necessidades/oportunidades |
| 2 | **Design** | Conceber soluções de produto/serviço |
| 3 | **Acquire** | Obter/alocar recursos para construir |
| 4 | **Build** | Criar, configurar e testar |
| 5 | **Transition** | Colocar em ambiente live |
| 6 | **Operate** | Operar o produto com desempenho acordado |
| 7 | **Deliver** | Entregar serviços digitais a partir do produto live |
| 8 | **Support** | Resolver issues e manter qualidade para usuários |

Contraste com a *service value chain* (ITIL 4) — 6 atividades: Plan, Improve, Engage, Design & transition, Obtain/build, Deliver & support. Em prova alinhada ao edital **Version 5**, priorize as **8** do lifecycle; saiba que a v4 existiu com as 6.

### 1.7 Practices que mais caem (núcleo FCC)

Mesmo com 34 practices, a banca costuma orbitar:

| Practice | Núcleo |
|----------|--------|
| **Incident management** | Restaurar o serviço o **mais rápido** possível |
| **Problem management** | Causa raiz / erros conhecidos; reduzir reincidência |
| **Change enablement** (change) | Mudanças com risco controlado (não “mudar sem processo”) |
| **Service request management** | Pedidos padrão (acesso, reset, informação) |
| **Service desk** | Ponto único de contato com usuários |
| **Service level management** | Acordos de nível (SLA) e monitoramento |
| **Continual improvement** | Melhoria contínua (não projeto único) |
| **Monitoring and event management** | Eventos → alertas → ação |
| **Release / Deployment** | Empacotar e colocar em produção (visão) |
| **Information security management** | Segurança como practice (encaixa com ISO/LGPD do edital) |

Diferenciação ouro:

```
Incidente  → interrupção/degradação → restaurar AGORA
Problema   → causa / tendência → investigar e eliminar/mitigar
Requisição → pedido padrão (não é necessariamente falha)
Mudança    → alteração controlada no ambiente
```

### 1.8 Valor, utilidade e garantia (herança útil)

- **Utility** (utilidade) — “o que o serviço faz” (fit for purpose)  
- **Warranty** (garantia) — “como o serviço é entregue” (fit for use: disponibilidade, capacidade, continuidade, segurança)  
- **Value** — percepção de benefício, influenciada por utility + warranty (+ experiência na v5)

### 1.9 ITIL × COBIT × PMBOK × Ágeis (anti-confusão)

| Framework | Pergunta principal |
|-----------|-------------------|
| **ITIL** | Como gerenciar **serviços/produtos** de TI com valor e practices operacionais? |
| **COBIT** | Como **governar e gerir** I&T alinhado a objetivos empresariais (objetivos EDM/APO/BAI/DSS/MEA)? |
| **PMBOK** | Como gerenciar **projetos**? |
| **Scrum/Kanban/XP** | Como desenvolver/entregar de forma **ágil**? |

Um tribunal pode usar **ágeis** para construir, **ITIL** para operar o serviço, **COBIT** para governança — sem serem sinônimos.

### 1.10 Armadilhas FCC (checklist)

- Confundir **incidente** com **problema**.  
- Achar que Service Desk **resolve a causa raiz** (isso é problem management).  
- Tratar ITIL como **norma ISO** ou como **COBIT**.  
- Decorar só ferramentas (ServiceNow etc.) em vez de concepts.  
- Na Version 5: citar só as 6 atividades da value chain e ignorar o **lifecycle de 8**.  
- “Optimize and automate” = automatizar bagunça.  
- Achar que ITIL proíbe ágeis ou DevOps.

### 1.11 Cartão-resumo (antes das questões)

| Tema | Âncora |
|------|--------|
| ITIL v5 | Boas práticas de produto **e** serviço digitais |
| Value System | Princípios + governance + lifecycle + practices + melhoria |
| Lifecycle | Discover→Design→Acquire→Build→Transition→Operate→Deliver→Support |
| Princípios | 7 (value … optimize and automate) |
| Incidente × Problema | Restaurar × causa raiz |
| vs COBIT | Serviços/operação × governança I&T |

---

## 2. Questões de treino (1–15)

**1.** ITIL, em definição cobrável, é melhor descrito como:

A) Linguagem de programação  
B) Conjunto de boas práticas de gerenciamento de serviços (e, na v5, produtos e serviços digitais)  
C) Banco de dados relacional  
D) Norma que substitui a LGPD  
E) Sinônimo exato de COBIT 2019  

**2.** No ITIL Version 5, o modelo operacional central destacado é:

A) Apenas o ciclo Deming isolado  
B) Product and Service Lifecycle Model (8 atividades)  
C) Modelo OSI de 7 camadas  
D) Cascata obrigatória do PMBOK  
E) Somente Pair Programming  

**3.** Qual sequência pertence ao lifecycle da Version 5?

A) Plan → Do → Check → Act apenas  
B) Discover → Design → Acquire → Build → Transition → Operate → Deliver → Support  
C) Initiation → Planning → Executing → Closing apenas  
D) Commit → Push → Merge apenas  
E) EDM → APO → BAI → DSS → MEA  

**4.** O princípio “Start where you are” orienta a:

A) Descartar sempre todo o legado antes de qualquer melhoria  
B) Avaliar o estado atual e reutilizar o que for útil  
C) Automatizar antes de entender o processo  
D) Ignorar feedback  
E) Proibir colaboração  

**5.** Incident management tem como foco principal:

A) Redesenhar a estratégia de negócio do tribunal  
B) Restaurar o serviço normal o mais rápido possível  
C) Substituir o COBIT  
D) Treinar redes neurais  
E) Elaborar o Regimento Interno  

**6.** Problem management diferencia-se de incident management porque:

A) Ignora reincidência  
B) Busca causa / redução de reincidência; incidente prioriza restauração rápida  
C) É o nome do Service Desk  
D) Só existe no PMBOK  
E) Trata apenas de pedidos de reset de senha  

**7.** Service Desk, em visão ITIL, é tipicamente:

A) O Authorization Server OAuth  
B) Ponto (único) de contato entre usuários e a gestão de serviços  
C) O cluster Kubernetes  
D) O framework COBIT inteiro  
E) A practice que elimina mudanças  

**8.** Change enablement (mudança) visa principalmente:

A) Mudar produção sem registro  
B) Viabilizar mudanças com avaliação e controle de risco  
C) Proibir qualquer alteração  
D) Substituir continuous improvement  
E) Ser sinônimo de incidente  

**9.** Os sete guiding principles:

A) Foram abolidos na Version 5  
B) Permanecem na Version 5 (incluindo Focus on value e Optimize and automate)  
C) São exclusivos do COBIT EDM  
D) São as 8 atividades do lifecycle  
E) Substituem as 34 practices  

**10.** Assinale a associação **incorreta**:

A) Utility ≈ fit for purpose  
B) Warranty ≈ fit for use  
C) ITIL ≈ apenas cabeamento estruturado  
D) Continual improvement ≈ melhoria contínua  
E) SLA ≈ acordos de nível de serviço  

**11.** Relação ITIL × COBIT mais adequada:

A) São o mesmo framework com nomes comerciais diferentes  
B) ITIL foca práticas de serviço/produto; COBIT foca governança/gestão de I&T  
C) COBIT só serve para redes; ITIL só para Java  
D) Ambos substituem a Constituição  
E) ITIL é obrigatoriamente anterior ao nascimento do COBIT em qualquer órgão  

**12.** “Optimize and automate” recomenda, em espírito:

A) Automatizar processos caóticos sem análise  
B) Otimizar e, então, automatizar o que fizer sentido  
C) Nunca automatizar  
D) Automatizar apenas impressoras  
E) Eliminar pessoas da dimensão Organizations and people  

**13.** Qual das opções é activity do lifecycle v5?

A) EDM03  
B) Sprint Retrospective exclusiva  
C) Discover  
D) Normalização 3FN  
E) ARP spoofing  

**14.** Service request management trata tipicamente de:

A) Queda total do data center como se fosse pedido padrão sem prioridade  
B) Pedidos padronizados do usuário (ex.: acesso, informação, reset)  
C) Apenas causa raiz de falhas recorrentes  
D) Elaboração do PDTIC  
E) Certificação PeopleCert obrigatória a cada commit  

**15.** Síntese correta deste arquivo:

A) ITIL v5 = Value System + lifecycle 8 + 7 princípios + practices (incidente≠problema); ≠ COBIT/PMBOK  
B) ITIL v5 = apenas UML 2.5.1  
C) Lifecycle = só Plan/Do/Check/Act do COBIT  
D) Service Desk = problem management  
E) Version 5 removeu todas as practices  

---

### Gabarito comentado (treino 1–15)

1. **B** — Boas práticas de serviços/produtos.  
2. **B** — Lifecycle de 8 atividades.  
3. **B** — Sequência Discover…Support.  
4. **B** — Partir do estado atual.  
5. **B** — Restaurar rápido.  
6. **B** — Causa × restauração.  
7. **B** — Ponto de contato.  
8. **B** — Mudança com controle de risco.  
9. **B** — 7 princípios mantidos.  
10. **C** — ITIL ≠ cabeamento (incorreta).  
11. **B** — Serviços × governança I&T.  
12. **B** — Otimizar depois automatizar.  
13. **C** — Discover.  
14. **B** — Pedidos padrão.  
15. **A** — Síntese do arquivo.  

---

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto de gerenciamento de serviços de TI alinhado ao ITIL Version 5, o framework é corretamente descrito como:

A) Sistema gerenciador de banco de dados oficial do CNJ  
B) Conjunto de boas práticas orientadas a valor na gestão de produtos e serviços digitais  
C) Protocolo de roteamento IPv6  
D) Substituição normativa da ISO 27001  
E) Metodologia exclusiva de estimativa de pontuação de história ágil  

**17.** Considere as afirmativas:

I. O ITIL Version 5 mantém os sete guiding principles.  
II. Incident management prioriza a restauração rápida do serviço.  
III. ITIL e COBIT 2019 são nomenclaturas distintas para o mesmo conjunto de 40 objetivos EDM/APO/BAI/DSS/MEA.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**18.** Relacione:

(1) Incident management  
(2) Problem management  
(3) Service desk  

( ) Ponto de contato com usuários  
( ) Restaurar serviço o mais breve possível  
( ) Tratar causa / reduzir reincidência

A sequência correta é:

A) 1, 2, 3  
B) 3, 1, 2  
C) 2, 3, 1  
D) 3, 2, 1  
E) 1, 3, 2  

**19.** No Product and Service Lifecycle Model do ITIL Version 5, a atividade que corresponde a explorar e priorizar necessidades e oportunidades denomina-se:

A) Support  
B) Operate  
C) Discover  
D) MEA01  
E) Warranty  

**20.** Acerca da relação entre ITIL e outros referenciais do edital, assinale a alternativa correta:

A) PMBOK substitui integralmente o Service Desk  
B) Scrum é o nome antigo do ITIL 4  
C) COBIT 2019 concentra-se na governança e gestão de I&T; ITIL nas practices de serviço/produto  
D) BPMN é obrigatoriamente a única practice do ITIL  
E) Rancher é um guiding principle  

**21.** A distinção clássica entre *utility* e *warranty* no discurso ITIL indica, respectivamente:

A) Custo e prazo de projeto apenas  
B) Adequação ao propósito (o que faz) e adequação ao uso (como entrega — disponibilidade, capacidade, continuidade, segurança)  
C) Incidente e problema  
D) Discover e Support  
E) APO e DSS do COBIT  

**22.** Assinale a alternativa que contém **erro conceitual**:

A) Change enablement busca controlar risco em mudanças  
B) Continual improvement perpassa a melhoria contínua  
C) As quatro dimensões incluem organizations and people  
D) “Optimize and automate” recomenda automatizar processos sem antes analisá-los/otimizá-los  
E) Service level management lida com acordos e desempenho de serviço  

**23.** Em prova alinhada ao Anexo II (ITIL Version 5), a menção a um modelo de oito atividades Discover…Support refere-se prioritariamente a:

A) Domínios do COBIT 2019  
B) Product and Service Lifecycle Model  
C) Camadas do modelo OSI  
D) Fases exclusivas do ciclo de vida do software da ISO 12207 linha a linha  
E) Grupos de processos do PMBOK 8  

**24.** Um usuário reporta lentidão intermitente há semanas; a equipe restaura o desempenho a cada episódio, mas a causa profunda permanece. Em termos ITIL, a restauração rápida de cada episódio alinha-se a **incidente**; a investigação da causa recorrente alinha-se a:

A) Problem management  
B) Somente release management  
C) Apenas supplier management sem registro  
D) Strategy management exclusivo  
E) Architecture management como Service Desk  

**25.** Analise: “Framework X oferece practices como incident, problem e service desk, guiding principles (focus on value, start where you are…) e, na Version 5, um lifecycle de produto e serviço com oito atividades, distinto do modelo de governança por objetivos EDM/APO/BAI/DSS/MEA.”

O framework X corresponde a:

A) COBIT 2019  
B) PMBOK 8  
C) ITIL Version 5  
D) CMMI Dev  
E) MPS-SW 2024  

---

### Gabarito comentado (FCC 16–25)

16. **B** — Boas práticas / valor / produtos e serviços.  
17. **C** — I e II ok; III falsa (ITIL ≠ COBIT).  
18. **B** — Desk; incidente; problema.  
19. **C** — Discover.  
20. **C** — COBIT governança; ITIL serviços.  
21. **B** — Utility × warranty.  
22. **D** — Erro: automatizar sem otimizar.  
23. **B** — Lifecycle Model.  
24. **A** — Problem management.  
25. **C** — ITIL Version 5.  

---

## 4. Como usar este arquivo

1. Decorar: 7 princípios + incidente≠problema + lifecycle 8.  
2. Treino 1–15 → gabarito.  
3. FCC 16–25 em modo prova.  
4. Cruzar com `08-cobit-2019.md` (anti-sinônimo).  
5. Ligar mentalmente ao Modelo de Governança de TIC do TRT4 (mapa da semana 4).

---

*Arquivo 07 · ITIL Version 5 · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 4 — governança / gestão de serviços.*

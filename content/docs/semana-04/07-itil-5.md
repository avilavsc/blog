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

## 3. Questões de concurso (16–25)

**16.** (FCC – TRT-5/2013 – Analista Judiciário – TI)  
Atenção: As questões de números 57 a 60 devem ser respondidas considerando-se as atualizações realizadas na ITIL v3 em 2011. Considere:  
I. Restaurar a operação normal do serviço o mais rápido possível.  
II. Minimizar o impacto adverso nas operações do negócio.  
III. Monitorar o serviço de TI e detectar quando o desempenho cai abaixo dos limites aceitáveis.  
IV. Encontrar a causa raiz do problema e aplicar uma solução definitiva.  
São objetivos do processo de Gerenciamento de Incidentes da ITIL v3 o que consta APENAS em

A) III e IV.  
B) II e III.  
C) I e II.  
D) I e IV.  
E) I, II e IV.

**17.** (FCC – TRT-2/2014 – Analista Judiciário – TI)  
Na Operação de Serviço da ITIL v3 são definidos muitos conceitos como Incidente, Problema, Evento e Solicitação de Serviço. Considere, em uma empresa que utiliza corretamente a ITIL v3, as solicitações de usuários por:  
1. informação.  
2. aconselhamento.  
3. realização de uma Mudança Padrão.  
4. acesso a um serviço de TI.  
5. atendimento de um incidente.  
6. reset de uma senha.  
São consideradas Solicitação de Serviço APENAS os itens

A) 1, 2 e 3.  
B) 1, 3, 4 e 6.  
C) 4, 5 e 6.  
D) 3, 4, 5 e 6.  
E) 1, 2, 3, 4 e 6.

**18.** (FCC – TRT-2/2018 – Técnico Judiciário – TI)  
Considere que um Técnico de TI fez o atendimento de 1º nível a partir de uma Central de Serviço. Tendo seguido o fluxo de atividades, ele não conseguiu aplicar uma solução ou resolver o incidente. O Gerenciamento de Incidentes da ITIL v3 recomenda que

A) seja realizada uma categorização hierárquica: o incidente é passado para um profissional especializado e com nível gerencial, o que requer Acordos de Nível Operacional para garantir o atendimento do Acordo de Nível de Serviço.  
B) o status do incidente seja alterado para “aberto”.  
C) seja realizada uma escalação funcional: o incidente é passado para uma equipe ou profissional com mais experiência, o que geralmente requer Acordos de Nível Operacional para garantir o atendimento do Acordo de Nível de Serviço.  
D) o Técnico consulte o Banco de Dados de Resoluções Conhecidas e tente cada uma das resoluções antes de fazer uma escalação.  
E) o Técnico consulte o Acordo de Nível de Serviço e, se não estiver prevista a escalação nem a categorização, o status do incidente seja alterado para “sem solução”.

**19.** (FCC – TJ-MA/2019 – Técnico Judiciário – Hardware)  
A Central de Serviços (CS) recebeu as seguintes solicitações: − Substituição do toner da impressora a laser. − Instalação de ferramentas do pacote Microsoft Office, que já são pré-aprovadas, no notebook corporativo. − Dúvidas sobre utilização do sistema de Processo Judicial Eletrônico. − Solicitação do manual para uso do sistema de antivírus. As solicitações acima, de acordo com a ITIL v3 edição 2011,

A) devem ser tratadas pelo processo de Gerenciamento de Incidentes, pois foram recebidas pela Central de Serviços.  
B) foram recebidas pelo mesmo sistema de registro de incidentes e devem ser fechadas pela CS, mas são tratadas pelo processo de Cumprimento das Requisições.  
C) como já têm causa-raiz conhecida, devem ser tratadas pelo processo Gerenciamento de Problemas.  
D) têm necessidade de passar pelo planejamento e aprovação do processo de Gerenciamento de Mudanças.  
E) foram recebidas pelo mesmo sistema de registro de incidentes, mas são tratadas pelo processo de Gerenciamento de Acesso.

**20.** (FCC – TRT-6/2018 – Analista Judiciário – TI)  
Um Analista especializado em Tecnologia da Informação está trabalhando em uma prestadora de Serviços para atender ao pedido de um novo serviço de TI, que envolve a necessidade de arquiteturas tecnológicas para o serviço e de requisitos de nível de serviço. Considerando o uso da ITIL v3 edição de 2011, a fase do ciclo de vida do serviço que terá maior relação com a Engenharia de Requisitos é:

A) Estratégia de Serviço.  
B) Transição de Serviço.  
C) Operação de Serviço.  
D) Desenho de Serviço.  
E) Planejamento de Serviço.

**21.** (FCC – TRT-11/2017 – Técnico Judiciário – TI)  
Considere que uma equipe de técnicos do TRT está discutindo os seguintes aspectos no processo de implantação das melhores práticas da ITIL no Tribunal:  
- Como projetar a infraestrutura de forma que se mantenha segura e tolerante a falhas.  
- Como identificar e gerenciar riscos e criar um mapeamento completo dos riscos possíveis.  
- Quais documentos devem ser usados para registrar os planos, políticas, arquiteturas e treinamento da equipe.  
- Como alertar a Central de Serviços com antecedência sobre novos serviços e como preparar e treinar o pessoal de suporte.  
- Como contribuir para a melhoria continuada do serviço assegurando que está sendo implantada qualidade do serviço no ambiente de produção.  
De acordo com a ITIL v3 atualizada em 2011, a equipe está trabalhando nos aspectos que são pertinentes ao Estágio

A) Melhoria Contínua de Serviços.  
B) Operação de Serviços.  
C) Transição de Serviços.  
D) Desenho de Serviços.  
E) Estratégia de Serviços.

**22.** (FCC – TRT-20/2024 – Analista Judiciário – TI)  
São um conjunto de princípios ITIL V4 que orientam uma organização em todo o seu ciclo de vida de gestão de serviços, independentemente das mudanças que ocorram nos objetivos, estratégias ou na estrutura da organização. Um, dentre os sete princípios orientadores, é

A) manter uma abordagem inflexível para todas as mudanças.  
B) evitar a comunicação com os stakeholders.  
C) minimizar a documentação do serviço.  
D) colaborar e promover visibilidade.  
E) focar exclusivamente nos resultados financeiros.

**23.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
No ITIL V4, o principal propósito do Service Value System – SVS é

A) definir as funções e responsabilidades de todos os membros da equipe, fator implementado pela gestão de comunicação.  
B) prover uma estrutura para garantir que todos os componentes e atividades de uma organização trabalhem juntos para facilitar a criação de valor.  
C) focar exclusivamente na automação de serviço visando compatibilidade com a missão, a visão e os valores estratégicos da organização.  
D) implementar novas tecnologias de forma rápida e eficiente para garantir que o produto entregue aos stakeholders seja o mais atual possível.  
E) garantir que os serviços sejam sempre disponibilizados o mais cedo possível.

**24.** (FCC – TCE-GO/2022 – Analista de Controle Externo – TI)  
Na ITIL v4, as quatro dimensões especificadas são: Informação e tecnologia; Organizações e pessoas;

A) Infraestrutura e suporte; Gestão de clientes.  
B) Fluxos de valor e processos; Suporte técnico e serviços.  
C) Infraestrutura e suporte; Atendimento a clientes.  
D) Fluxos de valor e processos; Parceiros e fornecedores.  
E) Suporte técnico e serviços; Parceiros e fornecedores.

**25.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Um Tribunal Regional do Trabalho precisa aprimorar a utilização das práticas da ITIL 4 para melhorar a gestão e a governança de TI, visando garantir a continuidade dos serviços digitais utilizados por magistrados, servidores e jurisdicionados. Para isso, a equipe deve utilizar a prática

A) Gestão Técnica visando coordenar o registro, a categorização e a resolução rápida de falhas nos serviços digitais do Tribunal, garantindo que os usuários sejam atendidos de forma ágil e eficiente.  
B) Gerenciamento da Configuração do Serviço para coletar e gerenciar informações sobre os diversos itens de configuração que compõem os serviços digitais do Tribunal, incluindo hardware, software, redes e fornecedores, visando garantir maior estabilidade e confiabilidade dos sistemas.  
C) Gestão de Eventos para analisar as causas raízes de falhas recorrentes nos serviços digitais do Tribunal, identificando padrões e prevenindo incidentes futuros.  
D) Gestão de Nível de Serviço visando controlar e minimizar os impactos de mudanças nos sistemas do Tribunal, garantindo que todas as alterações sejam avaliadas e aprovadas antes da implementação.  
E) Gestão de Problemas para definir e monitorar acordos entre a TI e os usuários do Tribunal, estabelecendo expectativas sobre qualidade, disponibilidade e desempenho dos serviços digitais.

---

### Gabarito comentado (concurso 16–25)

| Q | Gab. | Fonte |
|---|------|-------|
| 16 | **C** | FCC TRT-5/2013 Analista TI — objetivos de incidente = restaurar rápido + minimizar impacto (I e II); III/IV são de outros processos |
| 17 | **E** | FCC TRT-2/2014 Analista TI — service request = info, aconselhamento, mudança padrão, acesso, reset; **não** atendimento de incidente |
| 18 | **C** | FCC TRT-2/2018 Técnico TI — escalação **funcional** (mais experiência), tipicamente com OLA |
| 19 | **B** | FCC TJ-MA/2019 Técnico Hardware — Cumprimento das Requisições (mesmo sistema de registro) |
| 20 | **D** | FCC TRT-6/2018 Analista TI — Desenho de Serviço × engenharia de requisitos / SLR / arquitetura |
| 21 | **D** | FCC TRT-11/2017 Técnico TI — aspectos de projetar segurança, riscos, docs e preparação = Desenho |
| 22 | **D** | FCC TRT-20/2024 Analista TI — guiding principle “colaborar e promover visibilidade” |
| 23 | **B** | FCC TRT-20/2024 Técnico TI — propósito do SVS = componentes trabalhando juntos para valor |
| 24 | **D** | FCC TCE-GO/2022 Analista TI — 4 dimensões: + fluxos de valor/processos + parceiros/fornecedores |
| 25 | **B** | FCC TRT-1/2025 Analista TI — prática correta = Gerenciamento da Configuração do Serviço (demais opções com propósitos trocados) |

---

## 4. Como usar este arquivo

1. Decorar: 7 princípios + incidente≠problema + lifecycle 8.  
2. Treino 1–15 → gabarito.  
3. FCC 16–25 em modo prova.  
4. Cruzar com `08-cobit-2019.md` (anti-sinônimo).  
5. Ligar mentalmente ao Modelo de Governança de TIC do TRT4 (mapa da semana 4).

---

*Arquivo 07 · ITIL Version 5 · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 4 — governança / gestão de serviços.*

+++
title = '06 — Low-code / No-code + IA no desenvolvimento'
date = '2026-10-20'
draft = false
weight = 6
tags = ['semana-04', 'lowcode', 'nocode']
categories = ['estudo']
+++
# Low-code / No-code + IA no desenvolvimento · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — low-code, no-code e assistentes de IA no desenvolvimento  
**Escopo deste arquivo:** definições cobráveis de **low-code** e **no-code**; quando cada um serve; papéis (citizen developer × profissional de TI); riscos (shadow IT, segurança, LGPD, vendor lock-in, dívida técnica); **uso de assistentes de IA** no ciclo de software (código, testes, documentação, revisão) — alinhado ao Anexo II (*low-code e no-code; uso de assistentes de IA no desenvolvimento de software*).  
**Fora de escopo aqui:** ML/redes neurais/LLMs em profundidade (semana 9 — bloco IA do edital); BPMN/UML/ágeis detalhados (outros arquivos da semana 4); COBIT/ITIL (arquivos 07–08); produto comercial linha a linha.

---

## 1. Material

### 1.1 Por que cai no edital

O Anexo II, em **Processo de desenvolvimento de software**, fecha o bloco clássico (CMMI, ISO 12207, MPS-SW, UML, BPMN, ágeis, requisitos) com duas frentes modernas:

| Frente | Leitura FCC |
|--------|-------------|
| **Low-code / no-code** | Acelerar entrega com plataformas visuais; menos (ou nenhum) código tradicional |
| **Assistentes de IA** | Apoiar o desenvolvedor (gerar, completar, explicar, testar, documentar) — **não** substituir governança |

Flash: FCC gosta de **diferenciar** low-code × no-code × programação tradicional × “IA que escreve código sozinha sem controle”.

### 1.2 No-code (definição cobrável)

**No-code** = desenvolvimento de aplicações **sem escrever código** (ou com código residual mínimo/oculto), via:

- interfaces **visuais** (arrastar e soltar);
- formulários, fluxos, regras declarativas;
- conectores prontos (e-mail, planilha, APIs, banco).

Público típico: **usuário de negócio** / *citizen developer* com supervisão de TI.

O que no-code **não** é:

- substituir obrigatoriamente todo sistema crítico do tribunal;
- “sem necessidade de requisitos, testes ou segurança”;
- sinônimo de low-code (há diferença de **profundidade de customização**).

### 1.3 Low-code (definição cobrável)

**Low-code** = plataformas que **reduzem** a quantidade de código tradicional, mas **permitem** (e muitas vezes **exigem**) extensões em código, scripts, APIs e componentes customizados.

Público típico: **desenvolvedores profissionais** + times mistos (negócio + TI).

| | **No-code** | **Low-code** | **Código tradicional** |
|---|-------------|----------------|------------------------|
| Código escrito | Quase nenhum | Reduzido, mas presente | Principal meio |
| Customização profunda | Limitada ao que a plataforma oferece | Alta (extensões) | Máxima |
| Velocidade inicial | Muito alta | Alta | Depende do time |
| Complexidade / integração crítica | Cuidado | Adequado com governança | Adequado |
| Perfil | Citizen developer | Dev + negócio | Engenheiro de software |

Síntese de prova:

```
No-code  → visual, pouca/nenhuma programação
Low-code → visual + extensões em código
Tradicional → código como eixo
IA assistente → acelera qualquer um dos três (com governança)
```

### 1.4 Citizen developer e papel da TI

**Citizen developer** = profissional de negócio que cria apps com plataformas low/no-code **sob políticas** da organização (não “TI clandestina”).

Papel da TI / área de sistemas (visão cobrável):

- definir **plataformas homologadas**;
- **governança** (acesso, dados, integração, ciclo de vida);
- revisão de segurança, LGPD, disponibilidade;
- integrar ao ecossistema (SSO, APIs, bancos oficiais).

**Shadow IT** = soluções criadas **fora** do controle/governança — risco clássico quando no-code explode sem política.

### 1.5 Quando usar (e quando não)

| Cenário | Tendência |
|---------|-----------|
| Protótipo, MVP, formulário interno, workflow simples | No-code / low-code |
| Integração leve com APIs estáveis | Low-code (ou no-code com conectores) |
| Núcleo judiciário crítico, alta complexidade, performance extrema, regras muito específicas | Código tradicional (ou low-code só como fachada + serviços robustos) |
| Automação de tarefa repetitiva de escritório | No-code / RPA leve (noção) |

FCC: plataforma low/no-code **não elimina** engenharia de requisitos, testes, versionamento nem segurança.

### 1.6 Riscos e controles (caderno mental)

| Risco | Controle típico |
|-------|-----------------|
| Shadow IT | Catálogo de plataformas + política de uso |
| Vendor lock-in | Avaliar portabilidade, exportação, padrões |
| Segurança / LGPD | Classificação de dados, SSO, trilhas de auditoria |
| Dívida técnica visual | Padrões, revisão, limite de complexidade na plataforma |
| Indisponibilidade do fornecedor | SLA, contingência, backup de configurações |
| “App de planilha” em produção | Governança de ciclo de vida (como qualquer software) |

### 1.7 Assistentes de IA no desenvolvimento

O edital cita **uso de assistentes de IA no desenvolvimento de software** — leitura prática (não é o bloco teórico de ML da semana 9):

| Uso | Exemplo cobrável |
|-----|------------------|
| Completar / sugerir código | Autocomplete inteligente no IDE |
| Gerar trechos a partir de prompt | Função, teste unitário, SQL de consulta |
| Explicar código legado | “O que este método faz?” |
| Refatorar / corrigir | Sugestão de patch, lint inteligente |
| Documentar | Comentários, README, OpenAPI rascunho |
| Apoiar revisão | Destacar risco, estilo, possível bug |

O que a IA **não** faz sozinha em prova séria:

- assumir **responsabilidade** jurídica/técnica do servidor público;
- garantir correção sem **revisão humana**;
- substituir testes, code review e normas do tribunal;
- autorizar envio de **dados sigilosos** a serviços externos sem política.

Princípio ouro FCC: IA = **assistente**; humano = **responsável** pelo artefato entregue.

### 1.8 Boas práticas com assistentes de IA (visão cobrável)

1. **Não colar** dados pessoais, segredos, processos sigilosos ou código proprietário sensível em ferramentas não homologadas.  
2. **Revisar** sempre o que foi gerado (alucinação, licença, vulnerabilidade).  
3. Preferir ferramentas **aprovadas** pela organização.  
4. Manter testes automatizados e revisão por pares.  
5. Documentar uso relevante quando a política exigir (transparência).  
6. Tratar sugestões como **rascunho**, não como verdade.

### 1.9 Relação com ágeis, requisitos e governança

- Low/no-code **encaixa** em entregas curtas (Scrum/Kanban), mas o **backlog** e os critérios de aceite continuam.  
- Requisitos ruins viram app visual ruim — mais rápido, não mais certo.  
- Governança (ITIL/COBIT / modelo TIC TRT4) cobre **serviços e controles**; a plataforma é só o meio.  
- PDPJ/microsserviços: low-code pode ser **camada de experiência** consumindo APIs; não “reescreve” a arquitetura sozinha.

### 1.10 Armadilhas FCC (checklist)

- Tratar no-code e low-code como **sinônimos**.  
- Dizer que no-code **elimina** a necessidade de TI/governança.  
- Afirmar que assistente de IA **substitui** o desenvolvedor ou o analista.  
- Confundir low-code com **UML**, **BPMN** ou **Kanban**.  
- Confundir “IA no desenvolvimento” com o bloco teórico completo de **ML/LLM** (outro tópico do edital).  
- Ignorar **LGPD / sigilo** ao usar cloud de IA.  
- Achar que low-code = sempre inferior (ou sempre superior) ao código tradicional.

### 1.11 Cartão-resumo (antes das questões)

| Tema | Âncora |
|------|--------|
| No-code | Visual, quase sem código; citizen developer |
| Low-code | Visual + extensões em código; times mistos |
| Tradicional | Código como eixo; complexidade alta |
| Shadow IT | App fora da governança |
| IA assistente | Acelera; humano revisa e responde |
| Não é | Substituição cega de TI, UML ou ML teórico |

---

## 2. Questões de treino (1–15)

**1.** No-code, em definição cobrável, caracteriza-se principalmente por:

A) Obrigar programação em assembly  
B) Construção visual com pouca ou nenhuma escrita de código  
C) Ser sinônimo exato de Kubernetes  
D) Substituir a LGPD  
E) Eliminar requisitos e testes por definição  

**2.** Low-code diferencia-se do no-code, em regra, porque:

A) Proíbe qualquer interface visual  
B) É exclusivo de mainframe  
C) Permite/exige mais **extensões em código** e customização profunda  
D) É o nome comercial do ITIL  
E) Só existe sem banco de dados  

**3.** Citizen developer, sob governança adequada, é:

A) Atacante externo autorizado  
B) Usuário de negócio que cria apps em plataformas homologadas com políticas de TI  
C) Substituto legal do juiz  
D) Sinônimo de cluster Rancher  
E) Protocolo OAuth  

**4.** Shadow IT no contexto low/no-code refere-se tipicamente a:

A) Backup oficial em fita  
B) Soluções criadas **fora** do controle/governança da organização  
C) Service Desk ITIL  
D) Objetivo EDM01 do COBIT  
E) Namespace Kubernetes  

**5.** Assistente de IA no desenvolvimento, segundo a leitura do edital, serve sobretudo para:

A) Substituir integralmente a responsabilidade do servidor  
B) Apoiar tarefas como sugerir código, testes e documentação, com revisão humana  
C) Emitir sentenças judiciais  
D) Remover a necessidade de SSO  
E) Proibir o uso de Git  

**6.** Qual associação está correta?

A) No-code = máxima customização em C; low-code = zero integração  
B) Low-code = só planilha; no-code = só COBOL  
C) IA assistente = Authorization Server OAuth  
D) No-code = quase sem código; low-code = código reduzido + extensões  
E) Tradicional = arrastar e soltar obrigatório  

**7.** Risco típico de plataformas low/no-code em órgãos públicos:

A) Impossibilidade teórica de existir formulário  
B) Vendor lock-in, shadow IT e exposição de dados sem controle  
C) Obrigação de abandonar TCP/IP  
D) Proibição de usar PostgreSQL  
E) Eliminação automática de todos os bugs  

**8.** Em sistema judiciário crítico de alta complexidade, a escolha mais alinhada à prudência técnica costuma ser:

A) No-code sem revisão para o núcleo  
B) Código tradicional (ou arquitetura robusta) para o núcleo, podendo low-code na borda  
C) Planilha compartilhada anônima como banco oficial  
D) IA generativa sem testes  
E) Desligar governança para acelerar  

**9.** Assinale a afirmativa **incorreta**:

A) Low-code reduz volume de código tradicional  
B) No-code usa interfaces visuais  
C) Assistentes de IA dispensam qualquer revisão humana por definição legal  
D) Governança de TI continua necessária  
E) Requisitos ruins prejudicam apps low/no-code  

**10.** Boa prática ao usar assistente de IA em ambiente de tribunal:

A) Colar processos sigilosos em qualquer chat público  
B) Aceitar todo código gerado sem leitura  
C) Usar só ferramentas homologadas e revisar artefatos; proteger dados sensíveis  
D) Desativar autenticação  
E) Publicar secrets no repositório para “ajudar a IA”  

**11.** Low-code/no-code em relação a métodos ágeis:

A) Eliminam o backlog  
B) Proíbem Kanban  
C) Podem acelerar entregas, mantendo priorização e critérios de aceite  
D) Substituem o Product Owner por um algoritmo obrigatório  
E) São incompatíveis com sprints  

**12.** Qual contraste está alinhado ao edital?

A) UML = low-code; BPMN = no-code  
B) Low/no-code = meios de construir apps; assistente de IA = apoio ao desenvolvimento  
C) ITIL = linguagem de programação  
D) COBIT = IDE  
E) MapStruct = plataforma no-code  

**13.** Dívida técnica em apps low-code surge sobretudo quando:

A) Há versionamento e padrões  
B) Complexidade e gambiarras visuais crescem sem revisão/padrões  
C) Existe apenas um formulário trivial bem governado  
D) A TI homologa a plataforma  
E) Há testes automatizados  

**14.** O bloco “IA no desenvolvimento” deste arquivo **não** se confunde prioritariamente com:

A) Autocomplete inteligente no IDE  
B) Geração de teste unitário assistida  
C) Estudo profundo de redes neurais e treinamento de modelos (bloco IA teórico do edital)  
D) Explicação de código legado por assistente  
E) Sugestão de refatoração  

**15.** Síntese correta do subtópico:

A) No-code ≈ visual sem código; low-code ≈ visual + código; IA = assistente com humano responsável; governança obrigatória  
B) No-code = COBIT; low-code = ITIL  
C) IA substitui analista e juiz  
D) Shadow IT é política oficial recomendada  
E) Low-code elimina LGPD  

---

### Gabarito comentado (treino 1–15)

1. **B** — Visual, pouco/nenhum código.  
2. **C** — Extensões em código / customização.  
3. **B** — Negócio + plataforma homologada.  
4. **B** — Fora da governança.  
5. **B** — Apoio com revisão humana.  
6. **D** — Diferença no-code × low-code.  
7. **B** — Lock-in, shadow IT, dados.  
8. **B** — Núcleo robusto; borda pode ser low-code.  
9. **C** — Incorreta: IA não dispensa revisão.  
10. **C** — Homologação + revisão + dados.  
11. **C** — Acelera; backlog permanece.  
12. **B** — Meios × assistência.  
13. **B** — Complexidade sem padrão.  
14. **C** — ML teórico é outro bloco.  
15. **A** — Síntese do arquivo.  

---

## 3. Questões de concurso (16–25)

> **Nota:** questões **reais** de concurso (preferência FCC / TRT Analista TI), com banca, órgão e ano citados. Treino 1–15 permanece estilo didático.

**16.** (FCC – TRT-15/2025 – Analista Judiciário – Tecnologia da Informação)

Um Tribunal Regional do Trabalho implementou uma plataforma no-code para permitir que diferentes departamentos criem seus próprios aplicativos de gestão de tarefas e projetos. Após seis meses de uso, a administração avaliou que os resultados obtidos no uso de plataformas no-code está em consonância com o que é frequentemente associado com

A) o aumento da dependência da equipe de TI para manutenção de sistemas.
B) a redução na capacidade de personalização das soluções.
C) a maior complexidade na integração com sistemas existentes.
D) a necessidade contínua de treinamento intensivo para todos os funcionários.
E) a redução do tempo de desenvolvimento e entrega de soluções, permitindo uma resposta mais rápida às necessidades do negócio.

**17.** (FCC – TRT-15/2025 – Técnico Judiciário – Tecnologia da Informação)

Ao ser questionado sobre a segurança em plataformas low-code, uma Técnica de um Tribunal Regional do Trabalho respondeu, corretamente que

A) a segurança em low-code depende exclusivamente das práticas de desenvolvimento do usuário.
B) a integração com sistemas existentes pode introduzir vulnerabilidades específicas.
C) plataformas low-code possuem segurança nativa que elimina todos os riscos de segurança.
D) todas as plataformas low-code garantem a mesma qualidade de segurança.
E) low-code não pode ser utilizado para desenvolver aplicações que exigem alta segurança.

**18.** (FCC – TRT-20/2024 – Analista Judiciário – Tecnologia da Informação)

A equipe de TI de um Tribunal estava sobrecarregada com demandas de novos sistemas, além da necessidade de manutenção de aplicações legadas. Foi definida, então, uma estratégia de utilização de plataformas Low-code e No-code visando aumentar a agilidade no desenvolvimento, além de permitir que setores não técnicos desenvolvessem suas próprias ferramentas. O conjunto de aplicações adequadas para desenvolvimento utilizando estas plataformas, considerando as vantagens e limitações dessas abordagens, inclui

A) migração de sistemas legados de mainframes para cloud, com recursos customizados de controle sobre desempenho e segurança.
B) cálculo automatizado de sentenças judiciais e sistemas voltados para análise preditiva avançada de processos trabalhistas críticos e confidenciais.
C) sistemas de controle de processos judiciais em tempo real, com lógicas precisas de automação de decisões judiciais, integração com múltiplos bancos de dados e alto desempenho em processamento de grandes volumes de dados.
D) funcionalidades para o sistema de gestão de casos judiciais, incluindo a integração com APIs governamentais, bancos de dados distribuídos e lógica de negócios customizada.
E) automação de fluxos de trabalho internos, como solicitações de férias e folgas, gestão de inventário de TI, aplicativos para coleta de dados através de questionários e dashboards para visualização de relatórios gerenciais.

**19.** (FCC – TRT-1/2025 – Analista Judiciário – Tecnologia da Informação)

No contexto do processo de desenvolvimento de software, diferentes frameworks, normas e metodologias podem ser aplicados para garantir qualidade, conformidade e eficiência, como

A) o CMMI e o MR-MPS-SW, que não recomendam o desenvolvimento low-code e no-code, uma vez que estes podem comprometer a qualidade do software.
B) a notação BPMN, que substituiu a UML 2.5 por ter sido descontinuada em processos de Engenharia de Software modernos, independentemente do domínio de aplicação do sistema em desenvolvimento.
C) o MR-MPS-SW 2023, que eliminou a necessidade do processo Engenharia de Requisitos do nível G, uma vez que essa responsabilidade foi transferida para o processo Projeto e Construção Ágil do Produto do nível F, que preconiza o uso de metodologias como Scrum e XP.
D) o CMMI-DEV v2.0, no qual as práticas de Engenharia de Software são abordadas de forma independente dos níveis de maturidade organizacional, permitindo que uma organização alcance a certificação em um único processo de avaliação direta.
E) a norma ABNT NBR ISO/IEC/IEEE 12207:2021, que define que o processo de definição de requisitos de sistema/software tem como objetivo transformar as capacidades desejadas dos stakeholders, visão orientada ao usuário, em uma visão técnica que atenda às necessidades operacionais do usuário.

**20.** (FCC – TRT-15/2025 – Analista Judiciário – Tecnologia da Informação)

A equipe de desenvolvimento de um Tribunal Regional do Trabalho está se orientando pelo CMMI-DEV v2.0 para melhorar a maturidade dos processos de desenvolvimento. Uma das Analistas de TI verificou, corretamente, que a área de capacidade (Capability Area) Engineering & Developing Products (EDP) inclui as áreas de prática (Practices Areas)

A) Technical Solution e Peer Review.
B) Estimating, Planning e Product Integration.
C) Technical Solution e Product Integration.
D) Process Quality Assurance, Risk and Oportunity Management e Process Management.
E) Monitor and Control e Governance.

**21.** (FCC – TRT-15/2025 – Analista Judiciário – Tecnologia da Informação)

Um Tribunal Regional do Trabalho está desenvolvendo um novo sistema para gerenciar o processo de tramitação de ações trabalhistas para automatizar a distribuição de processos, agendar audiências, registrar decisões, gerar relatórios e fazer a integração com outros sistemas do judiciário. Durante a fase de modelagem dos processos de negócios, a equipe de TI cogitou utilizar a notação BPMN (Business Process Model and Notation) pensando em garantir uma visualização clara e eficiente dos fluxos de trabalho. Nessas circunstâncias, um dos Analistas apontou, sobre a utilização desse modelo, que ele

A) não é adequado para representar processos com atividades de decisão complexas, como a avaliação das evidências e argumentos apresentados durante as audiências, pois ele não é capaz de modelar condições de negócios em profundidade.
B) é ideal para modelar os fluxos de trabalho do sistema, pois permite representar de forma clara atividades, decisões, subprocessos, eventos e fluxos de trabalho, além de facilitar a comunicação entre os stakeholders do sistema judicial.
C) deve ser utilizado exclusivamente para modelar processos automatizados, não sendo recomendado para processos judiciais que envolvem atividades manuais, como a análise de documentos ou decisões por parte de juízes e servidores.
D) é ideal, mas deve ser usado apenas para modelar o fluxo de informações entre os sistemas envolvidos no TRT, como a integração com o banco de dados e sistemas externos, todavia não é recomendado para a modelagem dos processos manuais da tramitação de processos.
E) é inadequado para modelar processos no contexto judicial, pois a notação não permite a inclusão de eventos e condições específicas exigidas pelas normas jurídicas e regulatórias que regem o Tribunal Regional do Trabalho.

**22.** (FCC – TRT-15/2025 – Analista Judiciário – Tecnologia da Informação)

Um sistema de gestão de processos judiciais está sendo desenvolvido para um Tribunal Regional do Trabalho e deve permitir o cadastro de processos, a definição de fluxos de trabalho, a gestão de documentos e a geração de relatórios estatísticos. A equipe de desenvolvimento está utilizando a UML 2.5 para modelar a interação entre os diversos objetos do sistema, como processos, documentos e usuários, enfatizando a troca de mensagens e a ordem temporal das ações. Para isso, o mais adequado é utilizar o diagrama de

A) sequência.
B) classe.
C) máquina de estado.
D) atividade.
E) componente.

**23.** (FCC – TRT-15/2025 – Analista Judiciário – Tecnologia da Informação)

Uma Analista de TI está revisando seus processos de desenvolvimento de software para alinhá-los à norma ABNT NBR ISO/IEC/IEEE 12207:2021. Durante a análise, ela identificou a necessidade de implementar processos para assegurar a qualidade do produto final. Um dos processos primários (ou fundamentais) conforme descrito na norma ABNT NBR ISO/IEC/IEEE 12207:2021 é o Processo de

A) Auditoria de Software.
B) Avaliação de Fornecedores.
C) Gestão de Configuração.
D) Desenvolvimento.
E) Engenharia de Sistemas.

**24.** (FCC – TRT-20/2024 – Analista Judiciário – Tecnologia da Informação)

Considere, por hipótese, que um Tribunal está adotando o CMMI-DEV v2.0. Para aumentar a previsibilidade dos resultados dos projetos, reduzir o retrabalho e garantir que os requisitos do cliente sejam cumpridos de forma mais eficiente, é necessário, dentre outras ações,

A) estabelecer um controle rigoroso de qualidade com foco em auditorias periódicas nos processos, utilizando a área de prática IMP - Improving Performance da Área de Capacitação PCM - Process Management, para garantir que todas as mudanças sejam aprovadas e documentadas formalmente.
B) priorizar a entrega rápida de funcionalidades, utilizando as áreas de prática MPM - Managing Performance & Measurement e PCM - Process Management da Área de Capacitação PMW - Planning & Managing Work, para aumentar a satisfação do cliente e reduzir o tempo de mercado (time to market).
C) investir em treinamentos intensivos de desenvolvimento ágil para toda a equipe e implementar o Scrum como principal metodologia, como recomenda a área de prática OT - Organizational Training da Área de Capacitação ENQ - Ensuring Quality.
D) adotar as áreas de prática CAR - Causal Analysis and Resolution da Área de Capacitação SI - Supporting Implementation e RDM - Requirements Development and Management da Área de Capacitação ENQ - Ensuring Quality, que contribuirão para identificação das causas raízes dos defeitos e cumprimento dos requisitos do cliente de forma eficiente e eficaz.
E) concentrar todos os esforços na automação de testes de software para reduzir defeitos, conforme recomenda a área de prática VAL - Validation da Área de Capacitação PMW - Planning & Managing Work, para garantir que o produto final atenda aos requisitos dos clientes.

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
| 16 | **E** | QConcursos/Quizinho TRT-15 2025 — no-code |
| 17 | **B** | QConcursos TRT-15 2025 Téc. — low-code segurança |
| 18 | **E** | Quizinho TRT-20 2024 Q22 — usos adequados |
| 19 | **E** | Quizinho TRT-1 2025 — CMMI/MPS não proíbem low-code (alt. A falsa) |
| 20 | **C** | Quizinho TRT-15 2025 — CMMI EDP (prova irmã) |
| 21 | **B** | Quizinho TRT-15 2025 — BPMN |
| 22 | **A** | QConcursos TRT-15 2025 — UML sequência |
| 23 | **D** | Quizinho TRT-15 2025 — ISO 12207 Desenvolvimento |
| 24 | **D** | Quizinho TRT-20 2024 Q23 |
| 25 | **E** | EstudeGrátis TRT-14 2016 |

## 4. Como usar este arquivo

1. Ler §1 (cartão §1.11).  
2. Fechar treino 1–15 sem gabarito → conferir.  
3. Fazer 16–25 em modo prova.  
4. Anotar erros (no-code × low-code; shadow IT; IA ≠ responsabilidade automática).  
5. Semana 9: aprofundar ML/LLM; aqui fica só o **assistente no ciclo de software**.

---

*Arquivo 06 · Low-code / no-code + IA no desenvolvimento · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 4 — engenharia de software.*

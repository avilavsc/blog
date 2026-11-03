+++
title = '02 — Engenharia de requisitos'
date = '2026-10-16'
draft = false
weight = 2
tags = ['semana-04', 'engenharia', 'requisitos']
categories = ['estudo']
+++
# Engenharia de requisitos · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Engenharia de requisitos (Semana 4 · Engenharia de software)  
**Escopo deste arquivo:** **engenharia de requisitos** cobrável: conceito, tipos (funcional × não funcional), elicitation/análise/especificação/validação/gerência, stakeholders, qualidade (SMART/IEEE), rastreabilidade, baseline/mudança, encaixe com ágeis e com CMMI/MPS/ISO (visão).  
**Fora de escopo aqui:** UML 2.5.1 a fundo (arquivo 03); BPMN; Scrum/Kanban/XP em profundidade (arquivo ágeis); CMMI/MPS/ISO detalhados; ITIL/COBIT; low-code/IA.

---

## 1. Material

### 1.1 O que é engenharia de requisitos (leitura de prova)

**Engenharia de requisitos** é o conjunto de atividades para **descobrir, analisar, documentar, validar e gerenciar** o que o sistema deve fazer e sob quais restrições — antes (e durante) a construção.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | Transformar **necessidade** do negócio/usuário em **requisitos** verificáveis |
| Ciclo | Elicitar → analisar → especificar → validar → **gerenciar** (mudanças/rastreio) |
| Produto típico | Documento/ backlog / casos de uso / histórias — o **quê**, não o **como** de implementação |

Flash: enunciado fala em **necessidade**, **stakeholder**, **funcional/não funcional**, **rastreabilidade**, **baseline**, **validação** → **engenharia de requisitos**.

Engenharia de requisitos **não** é: desenhar classes UML (é análise/projeto), implementar código, orquestrar containers, aplicar migration Flyway.

### 1.2 Por que a FCC cobra (problema cobrável)

Sem requisitos bem tratados:

1. Entrega “certa” do **problema errado**.  
2. Escopo **explode** sem controle de mudança.  
3. Testes não têm **oráculo** (o que validar?).  
4. Conflitos entre áreas (negócio × TI × jurídico) ficam **latentes**.  
5. Em Judiciário/PDPJ: alinhamento a **normativos** e qualidade do serviço público exige clareza do “o quê”.

Frase de prova: requisito ruim custa **mais caro** quanto mais tarde for descoberto.

### 1.3 Requisito × necessidade × restrição (anti-confusão)

| Conceito | Leitura |
|----------|---------|
| **Necessidade** | Dor/objetivo do stakeholder (pode ser vaga) |
| **Requisito** | Declaração **clara, verificável** do que o sistema/produto deve cumprir |
| **Restrição** | Limite imposto (prazo, lei, tecnologia, orçamento, integração obrigatória) |
| **Regra de negócio** | Política do domínio (ex.: “prazo recursal X”) — pode virar requisito |

Armadilha: tratar “o usuário quer algo moderno” como requisito pronto — falta **precisão** e critério de aceite.

### 1.4 Tipos clássicos (tabela ouro)

| Tipo | O quê descreve | Exemplos cobráveis |
|------|----------------|--------------------|
| **Funcional (RF)** | **O que** o sistema faz (comportamento, funções, dados) | “O sistema deve autenticar o usuário via SSO” |
| **Não funcional (RNF)** | **Como** / qualidades / restrições de qualidade | Desempenho, segurança, usabilidade, disponibilidade, manutenibilidade |
| **De domínio / negócio** | Regras do domínio / legislação aplicável | Prazos processuais, perfis de acesso Judiciário |
| **De interface / integração** | Contratos com outros sistemas | API REST/JSON, eventos, formatos |
| **De dados** | Persistência, retenção, qualidade | “Auditoria de alterações”; LGPD (visão) |

Flash FCC:

- Funcional = **comportamento** observável.  
- Não funcional = **qualidade/restrição** (performance, segurança, etc.).  
- Confundir RNF com RF é erro clássico.

### 1.5 Categorias comuns de RNF (margem de segurança)

Não precisa decorar ISO 25010 inteira; reconhecer eixos:

| Eixo | Ideia |
|------|-------|
| Desempenho / tempo de resposta | Latência, throughput |
| Disponibilidade / confiabilidade | Uptime, tolerância a falha |
| Segurança | Confidencialidade, integridade, autenticação/autorização |
| Usabilidade | Facilidade de uso, acessibilidade |
| Portabilidade / interoperabilidade | Rodar em ambientes / trocar dados |
| Escalabilidade / manutenibilidade | Crescer / evoluir código |

Armadilha: “deve ser seguro” sem métrica — RNF fraco; prova valoriza **mensurável** quando o enunciado pede qualidade.

### 1.6 Stakeholders e fontes de requisitos

**Stakeholder** = pessoa/grupo **afetado** ou **interessado** no sistema (usuário final, gestor, TI, jurídico, cidadão, órgão externo).

Fontes típicas:

1. Entrevistas / workshops  
2. Observação / shadowing  
3. Documentos (normativos, manuais, tickets)  
4. Sistemas legados e logs  
5. Protótipos / MVPs  
6. Análise de concorrentes / benchmarks (com cuidado)

Erro clássico: elicitar **só** com um perfil e ignorar conflito (ex.: produtividade do servidor × auditoria).

### 1.7 Processo em cinco blocos (cartão mental)

```
Elicitação  →  Análise  →  Especificação  →  Validação  →  Gerência
   (obter)      (organizar/     (documentar)     (confirmar      (mudança,
                 priorizar/                       com stakeholder)  rastreio,
                 negociar)                                          baseline)
```

| Atividade | Foco cobrável |
|-----------|---------------|
| **Elicitação** | Descobrir necessidades (técnicas de coleta) |
| **Análise** | Consistência, conflitos, priorização, viabilidade |
| **Especificação** | Documentar de forma clara (SRS, casos de uso, user stories…) |
| **Validação** | “É isso mesmo?” — revisão, protótipo, inspeção, aceite |
| **Gerência** | Controle de mudança, versão, rastreabilidade, baseline |

Não confundir **validação** (requisito certo?) com **verificação** (sistema atende o requisito documentado?) — distinção clássica IEEE/prova.

### 1.8 Técnicas de elicitation (reconhecer nomes)

| Técnica | Quando a prova cita |
|---------|---------------------|
| Entrevista | Perguntas abertas/fechadas com stakeholder |
| Questionário / survey | Muitos respondentes, menos profundidade |
| Workshop / JAD | Grupo facilita consenso e priorização |
| Brainstorming | Gerar ideias (depois filtrar) |
| Observação | Ver o trabalho real (não só o discurso) |
| Análise documental | Leis, normas, manuais, tickets |
| Protótipo | Validar cedo (descartável ou evolutivo) |
| Casos de uso / histórias | Estruturar interações ator–sistema |

Flash: **elicitar** ≠ **especificar** — um coleta; o outro formaliza.

### 1.9 Qualidade do requisito (lista de ouro)

Requisito “bom” tende a ser:

1. **Completo** (no escopo acordado)  
2. **Consistente** (sem contradição)  
3. **Não ambíguo**  
4. **Verificável / testável**  
5. **Rastreável** (origem ↔ desenho ↔ teste)  
6. **Priorizado**  
7. **Factível** (viável técnica/economicamente)

Mnemônico leve (SMART, visão): Specific, Measurable, Achievable, Relevant, Time-bound — útil para RNF e critérios de aceite.

Armadilha: “o sistema deve ser amigável” sem critério → **ambíguo**.

### 1.10 Priorização (visão cobrável)

Técnicas/nomes que aparecem:

| Nome | Ideia |
|------|-------|
| MoSCoW | Must / Should / Could / Won’t |
| Kano | Básico × performance × encantamento |
| Valor × esforço | Matriz simples de prioridade |
| Risco / dependência | O que desbloqueia ou ameaça o projeto |

Em ágeis: Product Owner prioriza backlog; requisitos evoluem em **incrementos** — mas **gerência** e **rastreio** não desaparecem.

### 1.11 Especificação: formas cobráveis

| Forma | Uso típico |
|-------|------------|
| SRS / documento de requisitos | Tradicional / cascata (visão) |
| Casos de uso | Atores + fluxos (liga a UML) |
| User stories + critérios de aceite | Ágil (INVEST: Independent, Negotiable, Valuable, Estimable, Small, Testable — visão) |
| Modelos (UML/BPMN) | Complementam texto; não substituem clareza |
| Protótipos / wireframes | Validação de interface |

Frase: especificação descreve o **problema/solução desejada**, não o código.

### 1.12 Rastreabilidade e baseline

| Conceito | Leitura de prova |
|----------|------------------|
| **Rastreabilidade** | Ligação requisito ↔ origem ↔ artefato (caso de uso, classe, teste) |
| **Matriz de rastreabilidade** | Tabela origem → requisito → implementação → teste |
| **Baseline** | Versão **congelada** acordada; mudanças passam por controle |
| **Controle de mudança** | Avaliar impacto (custo, prazo, risco) antes de aceitar |

Flash: sem rastreabilidade, mudança vira **efeito colateral** invisível.

### 1.13 Validação × verificação (anti-armadilha ouro)

| | **Validação** | **Verificação** |
|---|---------------|-----------------|
| Pergunta | Estamos construindo o **produto certo**? | Estamos construindo o produto **certo** (conforme spec)? |
| Foco | Adequação à necessidade do stakeholder | Conformidade com o documento/modelo |
| Exemplos | Revisão com usuário, protótipo aceito | Inspeção, teste contra RF/RNF documentados |

FCC gosta de trocar os termos — memorize a pergunta-chave.

### 1.14 Requisitos em ágil × “tradicional” (visão)

| Aspecto | Mais tradicional | Mais ágil |
|---------|------------------|-----------|
| Momento | Ênfase up-front | Evolutivo / just-in-time |
| Artefato | SRS longo | Backlog + stories + aceite |
| Mudança | Comitê / formal | Esperada, mas ainda **gerenciada** |
| Aceite | Homologação formal | Demo / DoD / critérios de aceite |

Armadilha: “ágil = sem requisitos” — **falso**. Ágil muda **quando** e **como** documenta; não elimina a engenharia.

### 1.15 Encaixe com edital (CMMI / MPS / ISO — só ponte)

No edital: engenharia de requisitos convive com **CMMI Dev**, **MR-MPS-SW 2024**, **ISO/IEC/IEEE 12207**, **UML**, **BPMN**, **ágeis**.

Leitura cobrável (sem aprofundar frameworks aqui):

- Processos maduros **exigem** práticas de requisitos (obter, analisar, gerir).  
- UML/BPMN **modelam**; não substituem elicitation e gerência.  
- PDPJ/Judiciário: requisitos de integração, segurança e conformidade **pesam** como RNF/restrições.

### 1.16 O que engenharia de requisitos **não** é (lista seca)

1. Não é programar a solução.  
2. Não é diagramar UML por si só (embora casos de uso ajudem).  
3. Não é BPMN (modelagem de processo de negócio — arquivo próprio).  
4. Não é Scrum (método ágil — arquivo próprio).  
5. Não é ITIL/COBIT (governança de serviços/TIC).  
6. Não é “só entrevista” — inclui análise, spec, validação e gerência.  
7. Não elimina testes: requisitos **alimentam** testes.  
8. Não é glossário vazio sem critérios verificáveis.

### 1.17 Armadilhas FCC (lista de ouro)

1. Trocar **funcional** por **não funcional**.  
2. Trocar **validação** por **verificação**.  
3. Dizer que ágil **dispensa** requisitos.  
4. Tratar necessidade vaga como requisito pronto.  
5. Ignorar **stakeholders** conflitantes.  
6. Esquecer **rastreabilidade** / baseline / mudança.  
7. Confundir requisito com **design** (como implementar).  
8. Achar que RNF “bonito” sem métrica é suficiente quando o item pede qualidade.  
9. Reduzir engenharia de requisitos a “escrever casos de uso UML”.  
10. Confundir elicitation com gerência de configuração de código.

### 1.18 Mini-síntese (cartão mental)

> **Engenharia de requisitos** = elicitar, analisar, especificar, validar e **gerenciar** o que o sistema deve cumprir.  
> **RF** = o que faz; **RNF** = qualidades/restrições.  
> Bom requisito: claro, verificável, rastreável, priorizado.  
> **Validação** = produto certo?; **Verificação** = conforme a spec?  
> Ágil **não** mata requisitos — muda a cadência.  
> Liga a UML (modelagem), BPMN (processo) e frameworks de processo (CMMI/MPS/ISO) sem se confundir com eles.

---

## 2. Questões de treino (1–15)

**1.** No edital de Analista TI TRT4, a engenharia de requisitos ocupa-se prioritariamente de:

A) Empacotar microsserviços em imagens Docker  
B) Descobrir, analisar, documentar, validar e gerenciar o que o sistema deve cumprir  
C) Orquestrar Pods no Kubernetes  
D) Aplicar migrations Flyway no PostgreSQL  
E) Configurar filtros Zuul de borda  

**2.** Assinale a associação **correta**:

A) Requisito funcional = desempenho e disponibilidade apenas  
B) Requisito funcional = comportamento/funções do sistema; não funcional = qualidades/restrições  
C) Requisito não funcional = exclusivamente casos de uso UML  
D) Necessidade vaga e requisito verificável são sinônimos estritos  
E) Rastreabilidade = apenas o Dockerfile  

**3.** “O sistema deve autenticar usuários via SSO institucional” é, em regra:

A) Requisito não funcional de usabilidade estética  
B) Requisito funcional (comportamento de autenticação)  
C) Apenas restrição de cabo de rede  
D) Atividade exclusiva de ITIL Service Desk  
E) Comando `docker pull`  

**4.** “O tempo de resposta da consulta X deve ser ≤ 2 s em 95% das requisições” exemplifica melhor:

A) Regra de negócio sem relação com software  
B) Requisito não funcional mensurável (desempenho)  
C) Diagrama de classes UML obrigatório  
D) Baseline de código-fonte Git  
E) Broker RabbitMQ  

**5.** A atividade de **elicitation** visa sobretudo:

A) Compilar o binário de produção  
B) Obter/descobrir necessidades e informações junto a fontes/stakeholders  
C) Substituir a validação com o usuário  
D) Gerar automaticamente o cluster Rancher  
E) Documentar apenas o “como” de implementação em bytecode  

**6.** Relativamente a validação e verificação de requisitos/produto, a leitura cobrável é:

A) São sempre sinônimos absolutos na FCC  
B) Validação pergunta se é o produto certo; verificação, se está conforme a especificação  
C) Verificação só existe em Docker  
D) Validação elimina qualquer teste  
E) Ambas significam apenas “deploy em produção”  

**7.** Rastreabilidade de requisitos serve principalmente para:

A) Ligar origem ↔ requisito ↔ artefatos/testes e avaliar impacto de mudanças  
B) Substituir o Product Owner  
C) Eliminar stakeholders  
D) Transformar todo RNF em RF automaticamente  
E) Orquestrar containers  

**8.** Qual afirmação é **incorreta**?

A) Baseline é versão acordada/congelada sob controle de mudança  
B) Ágil dispensa totalmente qualquer forma de requisito ou critério de aceite  
C) MoSCoW é técnica de priorização  
D) Stakeholder é parte interessada/afetada  
E) Ambiguidade prejudica a verificabilidade do requisito  

**9.** Em enunciado que fala em Must/Should/Could/Won’t, a técnica alinhada é:

A) MoSCoW  
B) Dijkstra  
C) CAP theorem  
D) Blue-green deploy exclusivo  
E) MapStruct  

**10.** Diferenciar requisito de design (solução) é relevante porque:

A) Não há diferença cobrável  
B) Requisito foca o **quê**/restrições acordadas; design detalha o **como** implementar  
C) Design sempre precede qualquer necessidade  
D) Requisito é apenas o nome do Pod Kubernetes  
E) Design elimina a gerência de mudança  

**11.** Protótipo na engenharia de requisitos serve tipicamente a:

A) Substituir permanente o código de produção sem validação  
B) Ajudar a **validar** cedo entendimento de interface/comportamento com stakeholders  
C) Aplicar DDL no lugar do Flyway  
D) Registrar serviço no Eureka  
E) Gerar token OAuth2  

**12.** Em contexto ágil com user stories, o elemento que torna a story testável/aceitável costuma ser:

A) A ausência total de critérios  
B) Critérios de aceite claros  
C) Apenas o diagrama de implantação UML  
D) O nome do registry Docker  
E) A porta do Zuul  

**13.** Análise de requisitos inclui, em visão de prova:

A) Só digitar código Java  
B) Resolver conflitos, checar consistência, priorizar e avaliar viabilidade  
C) Apenas `docker compose up`  
D) Somente configuração de SNMP  
E) Exclusivamente desenhar logo institucional  

**14.** Qual linha resume melhor o cartão mental deste subtópico?

A) Requisitos = só UML; ágil = zero documentação  
B) Elicitar → analisar → especificar → validar → gerenciar; RF ≠ RNF; validação ≠ verificação  
C) Requisitos = Docker + Rancher  
D) Requisitos = apenas COBIT  
E) Requisitos = somente criptografia TLS  

**15.** Afirmar que “requisito não funcional nunca precisa ser mensurável” é:

A) Sempre correto em prova FCC  
B) Problemático: RNF cobráveis de qualidade tendem a critérios verificáveis/mensuráveis  
C) Correto porque RNF = RF  
D) Correto somente com H2  
E) Correto se houver Swagger  

---

### Gabarito comentado (treino 1–15)

1. **B** — Ciclo clássico da engenharia de requisitos.  
2. **B** — RF = comportamento; RNF = qualidade/restrição.  
3. **B** — Autenticação como função/comportamento.  
4. **B** — Desempenho com métrica.  
5. **B** — Descoberta junto a fontes.  
6. **B** — Produto certo × conforme spec.  
7. **B** — Ligação e impacto de mudança.  
8. **B** — Ágil não elimina requisitos/aceite.  
9. **A** — MoSCoW.  
10. **B** — Quê × como.  
11. **B** — Validação precoce.  
12. **B** — Critérios de aceite.  
13. **B** — Consistência/prioridade/viabilidade.  
14. **B** — Síntese do arquivo.  
15. **B** — RNF deve ser verificável quando a qualidade importa.

---

## 3. Questões de concurso (16–25)

> **Nota:** questões **reais** de concurso (preferência FCC / TRT Analista TI), com banca, órgão e ano citados. Treino 1–15 permanece estilo didático.

**16.** (FCC – TRT-15/2023 – Analista Judiciário – Tecnologia da Informação)

Na Engenharia de Requisitos, um processo genérico de

A) especificação e validação de requisitos não funcionais se compõe de atividades como a análise e coleta, a modelagem de visão, a recuperação de dados e o rastreamento.
B) priorização de requisitos se compõe de atividades como a identificação de fontes de informação, a elicitação e a comunicação.
C) elicitação de requisitos se compõe de atividades como o rastreamento, a verificação e a validação.
D) modelagem e decomposição de requisitos se compõe de atividades como a validação, a especificação e a modelagem top-down.
E) elicitação e análise de requisitos se compõe de atividades como a descoberta, a classificação e organização, a priorização e negociação e a especificação.

**17.** (FCC – TRT-15/2023 – Analista Judiciário – Tecnologia da Informação)

Considere, por hipótese, que uma Analista de um Tribunal Regional do Trabalho está levantando os requisitos para o sistema de Controle de Processos e respectivas situações. Os usuários definem o que consta da seguinte lista:
1. O sistema deve estar disponível para todos os consulentes durante as horas normais de trabalho (segunda a sexta-feira, das 9:00 às 18:00).
2. Um usuário deve ser capaz de pesquisar os processos por número e por nome do processante para todas as categorias.
3. O sistema deve gerar a cada dia, para cada categoria de processo, a lista dos processos e respectivos processantes, bem como a situação e localização física e a distribuição, por juiz, dos processos, para as consultas daquele dia.
4. Períodos de não operação dentro do horário normal de trabalho não podem exceder a cinco segundos, cumulativamente, em um dia.
5. Cada processo deve ser cadastrado com número, nome do processante, juiz a quem foi distribuído e os respectivos advogados e promotores.
6. O sistema deve implementar as condições de privacidade dos processantes, tal como estabelecido nas normas gerais e regulamentos do órgão.
Relacione, correta e respectivamente, em ordem por tipo de requisito, os números referentes a três requisitos funcionais seguidos por três requisitos não funcionais da lista definida pelos usuários:

A) 2, 3, 6, 1, 4 e 5.
B) 1, 2, 4, 3, 5 e 6.
C) 2, 3, 5, 1, 4 e 6.
D) 1, 3, 5, 2, 4 e 6.
E) 1, 3, 6, 2, 4 e 5.

**18.** (FCC – TRT-20/2024 – Analista Judiciário – Tecnologia da Informação)

De acordo com o MR-MPS-SW: 2023, que é compatível com o CMMI-DEV v2.0, o propósito do processo REQ - Engenharia de Requisitos é definir, gerenciar e manter atualizados os requisitos das partes interessadas e do produto, garantindo que inconsistências entre os requisitos, os planos e os produtos de trabalho sejam identificadas e tratadas. Os resultados esperados, a partir de nível D, incluem que

A) as necessidades, expectativas e restrições das partes interessadas ligadas à área de TI, tanto em relação ao produto quanto a suas interfaces, sejam identificadas.
B) os requisitos sejam validados e que a validação possa ser realizada, entre outras formas, com uso de protótipos, demonstrações ou revisões.
C) os planos, as atividades e os requisitos de trabalho relacionados sejam revisados.
D) as avaliações de soluções sejam realizadas utilizando técnicas estatísticas e quantitativas para determinar se a solução pode ser aplicada na organização.
E) os recursos materiais e o ambiente de trabalho necessários para executar o projeto e seus requisitos sejam estabelecidos e mantidos atualizados.

**19.** (FCC – TRT-19/2022 – Analista Judiciário – Tecnologia da Informação)

No processo técnico de definição dos requisitos de sistema/software, um resultado esperado, segundo a norma ABNT NBR ISO/IEC/IEEE 12207:2021, é que

A) as necessidades dos stakeholders sejam definidas.
B) as medidas críticas de desempenho sejam definidas.
C) todas as restrições do sistema sejam identificadas pelos stakeholders.
D) a rastreabilidade dos requisitos dos stakeholders para suas necessidades sejam estabelecidas.
E) os elementos do sistema e suas interfaces sejam identificados e construídos.

**20.** (FCC – TRT-1/2025 – Analista Judiciário – Tecnologia da Informação)

No contexto do processo de desenvolvimento de software, diferentes frameworks, normas e metodologias podem ser aplicados para garantir qualidade, conformidade e eficiência, como

A) o CMMI e o MR-MPS-SW, que não recomendam o desenvolvimento low-code e no-code, uma vez que estes podem comprometer a qualidade do software.
B) a notação BPMN, que substituiu a UML 2.5 por ter sido descontinuada em processos de Engenharia de Software modernos, independentemente do domínio de aplicação do sistema em desenvolvimento.
C) o MR-MPS-SW 2023, que eliminou a necessidade do processo Engenharia de Requisitos do nível G, uma vez que essa responsabilidade foi transferida para o processo Projeto e Construção Ágil do Produto do nível F, que preconiza o uso de metodologias como Scrum e XP.
D) o CMMI-DEV v2.0, no qual as práticas de Engenharia de Software são abordadas de forma independente dos níveis de maturidade organizacional, permitindo que uma organização alcance a certificação em um único processo de avaliação direta.
E) a norma ABNT NBR ISO/IEC/IEEE 12207:2021, que define que o processo de definição de requisitos de sistema/software tem como objetivo transformar as capacidades desejadas dos stakeholders, visão orientada ao usuário, em uma visão técnica que atenda às necessidades operacionais do usuário.

**21.** (FCC – TRT-19/2011 – Analista Judiciário – Tecnologia da Informação)

A avaliação do impacto de mudança de um requisito, muitas vezes, faz com que seja necessário retornar à sua fonte. Na validação dos requisitos, a equipe deve estar atenta, portanto, à

A) rastreabilidade.
B) adaptabilidade.
C) qualidade.
D) facilidade de compreensão.
E) facilidade de verificação.

**22.** (FCC – TRT-8/2010 – Analista Judiciário – Tecnologia da Informação)

No âmbito da gerência de Configuração e Mudança de Software, ao conjunto de um ou mais itens de configuração identificados e liberados para uso, independente de suas versões, dá-se o nome de

A) baseline.
B) metadados.
C) relatório de status.
D) release request.
E) biblioteca.

**23.** (FCC – TRT-15/2023 – Analista Judiciário – Tecnologia da Informação)

Considere o texto a seguir: Ter os dados de configuração organizados e adicionados a um repositório permite a colaboração e a visibilidade da configuração do sistema. O fluxo de trabalho de solicitação pull que as equipes de software usam para revisar e editar código pode ser aplicado aos arquivos de dados de configuração. Assim, fica mais fácil construir um sistema de rastreio. Quaisquer alterações aplicadas à configuração devem ser revisadas e aceitas pela equipe. Assim, as mudanças de configuração ganham responsabilidade e visibilidade. Dentre os passos do processo de gerenciamento que monitora itens de configuração individuais de um sistema de TI, o texto trata da implementação do gerenciamento de configuração em ambientes DevOps e correlatos e se refere à atividade de

A) balanceamento de carga.
B) linha de base.
C) implantação.
D) auditoria.
E) renderização.

**24.** (FCC – TRT-20/2024 – Analista Judiciário – Tecnologia da Informação)

Considere, por hipótese, que um Tribunal está adotando o CMMI-DEV v2.0. Para aumentar a previsibilidade dos resultados dos projetos, reduzir o retrabalho e garantir que os requisitos do cliente sejam cumpridos de forma mais eficiente, é necessário, dentre outras ações,

A) estabelecer um controle rigoroso de qualidade com foco em auditorias periódicas nos processos, utilizando a área de prática IMP - Improving Performance da Área de Capacitação PCM - Process Management, para garantir que todas as mudanças sejam aprovadas e documentadas formalmente.
B) priorizar a entrega rápida de funcionalidades, utilizando as áreas de prática MPM - Managing Performance & Measurement e PCM - Process Management da Área de Capacitação PMW - Planning & Managing Work, para aumentar a satisfação do cliente e reduzir o tempo de mercado (time to market).
C) investir em treinamentos intensivos de desenvolvimento ágil para toda a equipe e implementar o Scrum como principal metodologia, como recomenda a área de prática OT - Organizational Training da Área de Capacitação ENQ - Ensuring Quality.
D) adotar as áreas de prática CAR - Causal Analysis and Resolution da Área de Capacitação SI - Supporting Implementation e RDM - Requirements Development and Management da Área de Capacitação ENQ - Ensuring Quality, que contribuirão para identificação das causas raízes dos defeitos e cumprimento dos requisitos do cliente de forma eficiente e eficaz.
E) concentrar todos os esforços na automação de testes de software para reduzir defeitos, conforme recomenda a área de prática VAL - Validation da Área de Capacitação PMW - Planning & Managing Work, para garantir que o produto final atenda aos requisitos dos clientes.

**25.** (FCC – TCE-GO/2022 – Analista de Controle Externo – Tecnologia da Informação)

Um analista deseja mostrar na forma de diagrama: I. A interação dos usuários com o sistema, a partir da observação dos requisitos funcionais. II. O fluxo de execução das ações de uma operação em um programa, de forma semelhante a um fluxograma. Utilizando a UML 2.5, deverá utilizar respectivamente, para I e II, os diagramas de

A) Classe e de Sequência.
B) Caso de Uso e de Atividades.
C) Componentes e de Atividades.
D) Implantação e de Colaboração.
E) Caso de Uso e de Sequência.

---

### Gabarito comentado (concurso 16–25)

| Questão | Gabarito | Fonte/nota |
|---------|----------|------------|
| 16 | **E** | Quizinho TRT-15 2023 Q50 |
| 17 | **C** | Quizinho TRT-15 2023 Q57 |
| 18 | **B** | Quizinho TRT-20 2024 Q21 |
| 19 | **B** | Quizinho TRT-19 2022 Q35 |
| 20 | **E** | Quizinho TRT-1 2025 Q32 |
| 21 | **A** | EstudeGrátis/Gabarite TRT-19 2011 |
| 22 | **A** | CFP Concursos TRT-8 2010 |
| 23 | **D** | Quizinho TRT-15 2023 Q48 |
| 24 | **D** | Quizinho TRT-20 2024 Q23 — RDM |
| 25 | **B** | Quizinho TCE-GO 2022 Q76 |

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **o quê o sistema deve fazer/cumprir** (ciclo elicitar…gerenciar)? → Engenharia de requisitos.  
2. É **comportamento/função**? → RF.  
3. É **qualidade/restrição mensurável**? → RNF.  
4. É **produto certo** vs **conforme spec**? → Validação × verificação.  
5. É **ligação origem–teste / impacto de mudança**? → Rastreabilidade / baseline.  
6. É **diagrama UML**? → Arquivo 03 (modelagem), não substitui requisitos.  
7. É **Scrum/Kanban**? → Ágeis (complementar).  
8. É **BPMN**? → Processo de negócio (outro arquivo).

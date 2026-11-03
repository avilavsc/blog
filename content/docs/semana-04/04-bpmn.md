+++
title = '04 — BPMN'
date = '2026-10-18'
draft = false
weight = 4
tags = ['semana-04', 'bpmn']
categories = ['estudo']
+++
# BPMN · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — BPMN (Semana 4 · Engenharia de software)  
**Escopo deste arquivo:** **BPMN** (Business Process Model and Notation): notação de **processos de negócio**, elementos gráficos cobráveis (eventos, atividades, gateways, fluxos, pools/lanes), leitura de diagrama, contraste com UML, encaixe com requisitos/ágeis e visão executável.  
**Fora de escopo aqui:** UML 2.5.1 a fundo (arquivo próprio); Scrum/Kanban/XP a fundo; CMMI / MPS-SW / ISO 12207 (arquivo 05); ITIL/COBIT/PMBOK; low-code/no-code; ferramentas específicas de BPMS além da noção.

---

## 1. Material

### 1.1 O que é BPMN (leitura de prova)

**BPMN** = **Business Process Model and Notation** — padrão (OMG) para **modelar processos de negócio** de forma gráfica, legível por negócio e por TI.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | **Modelar / comunicar** o fluxo de um processo de negócio |
| Unidade | Diagrama com **atividades, eventos, gateways e fluxos** |
| O que **não** é | Diagrama de classes UML; modelo ER; código-fonte; CMMI |

Flash: enunciado fala em **processo de negócio**, **pool/lane**, **gateway exclusivo/paralelo**, **evento de início/fim**, **fluxo de sequência/mensagem** → **BPMN**.

BPMN **não** é: UML (embora convivam), CMMI, MPS-SW, ISO 12207, Scrum, Kanban, ITIL, COBIT.

### 1.2 Por que BPMN? (problema cobrável)

Organizações (incl. Judiciário) precisam descrever **como o trabalho flui**: quem faz o quê, em que ordem, com quais decisões e handoffs.

Sem notação comum:

1. Processos ficam só na cabeça / e-mail.  
2. Negócio e TI **não compartilham** a mesma figura.  
3. Automação (BPMS / workflow / low-code) fica **ambígua**.  
4. Melhoria contínua (medir gargalos, retrabalho) perde **mapa**.

BPMN oferece **vocabulário visual padrão** para desenhar, discutir e, em níveis avançados, **executar** processos.

### 1.3 Níveis de uso (visão cobrável)

| Nível (ideia) | Foco |
|---------------|------|
| **Descritivo** | Comunicação com negócio; fluxo “feliz” e decisões principais |
| **Analítico** | Detalhe para análise/melhoria (exceções, timeouts, mensagens) |
| **Executável** | Modelo rico o bastante para motor de processo (BPMS) |

Prova: não precisa decorar catálogo OMG inteiro; precisa saber que BPMN vai de **comunicação** até **automação**.

### 1.4 Elementos-base (tabela ouro)

| Elemento | Forma mental | Papel |
|----------|--------------|-------|
| **Evento** | Círculo | Algo que **acontece** (inicia, intermediário, encerra) |
| **Atividade / Task** | Retângulo arredondado | Trabalho a ser **feito** |
| **Gateway** | Losango (diamante) | **Decisão / divisão / junção** de caminhos |
| **Fluxo de sequência** | Seta contínua | Ordem **dentro** do mesmo pool |
| **Fluxo de mensagem** | Seta tracejada | Comunicação **entre** participantes (pools) |
| **Pool** | Retângulo container | Participante (organização / sistema / ator externo) |
| **Lane** | Faixa dentro do pool | Papel / área / responsável **dentro** do participante |
| **Objeto de dados** | Documento | Dados produzidos/consumidos (visão) |
| **Anotação** | Texto | Comentário sem alterar o fluxo |

Flash:

```
[Evento início] → [Atividade] → <Gateway> → [Atividade] → [Evento fim]
                      ↑
              (fluxo de sequência)
```

### 1.5 Eventos (anti-confusão)

| Tipo | Borda / ideia | Leitura de prova |
|------|---------------|------------------|
| **Início (Start)** | Círculo fino | Onde o processo **começa** |
| **Intermediário** | Círculo duplo | Algo no **meio** (timer, mensagem, etc.) |
| **Fim (End)** | Círculo grosso | Onde o processo (ou caminho) **termina** |

Gatilhos comuns a reconhecer (nomes, não tutorial):

| Gatilho | Ideia |
|---------|-------|
| **None** | Sem tipo específico (genérico) |
| **Message** | Chegada/envio de mensagem |
| **Timer** | Tempo / prazo / agenda |
| **Error** | Erro / exceção |
| **Signal** | Sinal broadcast (visão) |
| **Terminate** | Encerra o processo imediatamente (fim especial) |

Armadilha: achar que “todo círculo é gateway” — **gateway é losango**; **evento é círculo**.

### 1.6 Atividades e subprocessos

| Conceito | Leitura |
|----------|---------|
| **Task** | Unidade atômica de trabalho |
| **Subprocesso** | Atividade que **esconde** um fluxo interno (pode expandir) |
| **Loop / Multi-instância** (visão) | Repetir ou várias instâncias da mesma atividade |
| **Tipos de task** (reconhecer) | User, Service, Script, Manual, Send/Receive — nomes para flash, não checklist de prova profunda |

Frase de prova: atividade = **trabalho**; evento = **ocorrência**; gateway = **controle de fluxo**.

### 1.7 Gateways (ouro da FCC)

| Gateway | Ideia | Comportamento típico |
|---------|-------|----------------------|
| **Exclusivo (XOR)** | “**Ou** um caminho” | Escolhe **um** ramo (decisão) |
| **Paralelo (AND)** | “**E** / fork-join” | Divide em caminhos **simultâneos**; junta quando todos chegam |
| **Inclusivo (OR)** | “Um ou **mais**” | Ativa um ou vários ramos conforme condições |
| **Event-based** (visão) | Espera **evento** | O caminho segue conforme qual evento ocorrer primeiro |

Anti-confusão:

| Confusão | Correção |
|----------|----------|
| XOR = paralelo | **Não**: XOR é exclusivo (um caminho) |
| AND = decisão “sim/não” | **Não**: AND é paralelismo |
| OR inclusivo = XOR | **Não**: OR pode ativar **vários** ramos |

### 1.8 Pools, lanes, sequência × mensagem

| Conceito | Papel |
|----------|-------|
| **Pool** | Participante do processo (ex.: “Tribunal”, “Advogado”, “Sistema X”) |
| **Lane** | Divisão interna (ex.: “Secretaria”, “Magistrado”, “TI”) |
| **Fluxo de sequência** | Dentro do **mesmo** pool — ordem do trabalho |
| **Fluxo de mensagem** | **Entre** pools — troca de mensagem (não “continua” o token interno) |

Regra de ouro cobrável:

- **Não** se desenha fluxo de sequência **cruzando** pools como se fosse o mesmo token.  
- Comunicação entre participantes → **mensagem** (tracejada).

### 1.9 BPMN × UML (contraste cobrável)

| | **BPMN** | **UML (visão)** |
|--|----------|-----------------|
| Foco principal | **Processo de negócio** / workflow | Modelagem de **software** (vários diagramas) |
| Diagrama “parecido” | Processo BPMN | Atividade UML (semântica e notação **diferentes**) |
| Público típico | Negócio + TI | Mais centrado em engenharia de software |
| Padrão | OMG BPMN | OMG UML 2.x |

Frase de prova: BPMN **não** substitui UML; **complementa** (negócio × software). Confundir “diagrama de classes” com BPMN é erro clássico.

### 1.10 Encaixe com requisitos, ágeis e Judiciário (margem)

| Tema | Encaixe mental |
|------|----------------|
| **Requisitos** | Processo modelado ajuda a elicitar regras, atores, exceções |
| **Ágeis** | User story descreve valor; BPMN pode detalhar o **fluxo operacional** |
| **Automação** | Modelo → BPMS / workflow / low-code (visão) |
| **Governança** | Processos documentados apoiam melhoria e compliance (sem virar ITIL) |

No contexto TRT/PDPJ: BPMN aparece no bloco de **engenharia de software / processos**, não como stack Java/Spring.

### 1.11 Armadilhas clássicas

1. Trocar **evento** (círculo) por **gateway** (losango).  
2. Dizer que fluxo de **mensagem** ordena o trabalho **dentro** do mesmo pool.  
3. Tratar XOR e AND como sinônimos.  
4. Afirmar que BPMN = UML ou = CMMI.  
5. Achar que todo processo BPMN é automaticamente **executável** sem detalhe.  
6. Colocar **classe/atributo/herança** como elemento central do BPMN.

### 1.12 Cartão mental (fechamento do material)

```
BPMN = notação de PROCESSO DE NEGÓCIO
  Evento (○)  ·  Atividade (▭)  ·  Gateway (◇)
  Sequência (→) dentro do pool
  Mensagem (- - →) entre pools
  Pool = participante · Lane = papel interno
  ≠ UML classes · ≠ CMMI · ≠ Scrum
```

---

## 2. Questões de treino (1–15)

**1.** BPMN caracteriza-se, em regra, por:

A) Ser o modelo de maturidade CMMI Development  
B) Ser notação para **modelar processos de negócio**  
C) Substituir obrigatoriamente o diagrama de classes UML  
D) Ser o framework ITIL 4  
E) Ser o protocolo OAuth2  

**2.** Em BPMN, o elemento tipicamente representado por um **losango (diamante)** é:

A) Evento de início  
B) Pool  
C) **Gateway**  
D) Objeto de dados  
E) Fluxo de mensagem  

**3.** Assinale a associação **correta**:

A) Evento = trabalho a executar; Atividade = ocorrência no tempo  
B) Evento = ocorrência; Atividade = trabalho; Gateway = controle de caminhos  
C) Gateway = participante externo; Pool = decisão XOR  
D) Lane = fluxo entre organizações distintas  
E) Mensagem = ordem interna obrigatória no mesmo pool  

**4.** O gateway **exclusivo (XOR)** tipicamente:

A) Ativa **todos** os caminhos em paralelo sem condição  
B) Escolhe **um** caminho dentre alternativas  
C) É sinônimo estrito de evento de timer  
D) Substitui pools e lanes  
E) Representa apenas subclasses UML  

**5.** Relativamente a pools e lanes, é correto afirmar que:

A) Lane é o participante externo; pool é sempre uma task  
B) Pool representa participante; lane divide responsabilidades **dentro** do pool  
C) Pool e lane são nomes do CMMI nível 5  
D) Lane cruza obrigatoriamente vários pools com fluxo de sequência  
E) Pool elimina a necessidade de eventos de fim  

**6.** Qual afirmação é **incorreta**?

A) Fluxo de sequência indica ordem dentro do mesmo pool  
B) Fluxo de mensagem comunica participantes (entre pools)  
C) BPMN é sinônimo estrito de diagrama entidade-relacionamento  
D) Evento de início marca o começo do processo  
E) Atividade representa trabalho a ser realizado  

**7.** Comparando BPMN e UML na leitura de edital:

A) São mutuamente excludentes em qualquer prova  
B) BPMN foca processo de negócio; UML modela software (vários diagramas) — convivem  
C) UML substitui BPMN em todo processo de negócio  
D) Ambos são apenas nomes comerciais do MPS-SW  
E) BPMN é o diagrama de classes obrigatório  

**8.** “Notação gráfica padrão para desenhar fluxo de processo com atividades, eventos e gateways.” Descreve:

A) CMMI v3.0 Dev  
B) ISO 12207  
C) BPMN  
D) Kanban board  
E) Flyway  

**9.** O gateway **paralelo (AND)** serve sobretudo para:

A) Escolher exatamente um ramo mutuamente exclusivo  
B) Dividir/juntar caminhos que correm **em paralelo**  
C) Documentar atributos de classe  
D) Registrar heartbeat no Eureka  
E) Aplicar migration V1/V2  

**10.** Sobre fluxo de mensagem em BPMN, a leitura cobrável mais segura é:

A) É a seta contínua que ordena tasks no mesmo lane sem exceção  
B) Representa comunicação **entre** participantes (pools), tipicamente tracejada  
C) É o único elemento permitido em diagramas UML de sequência  
D) Substitui o evento de fim  
E) É obrigatoriamente um gateway inclusivo  

**11.** BPMN **não** se destina a:

A) Comunicar o fluxo de um processo de negócio  
B) Apoiar análise e, em níveis avançados, automação em BPMS  
C) Substituir o modelo de maturidade CMMI como escala G–A  
D) Representar decisões com gateways  
E) Separar participantes em pools  

**12.** Em prova, confundir evento com gateway costuma errar porque:

A) Não há diferença cobrável  
B) Evento é ocorrência (círculo); gateway controla caminhos (losango)  
C) Gateway só existe fora do BPMN  
D) Evento é sempre paralelismo AND  
E) Ambos são pools  

**13.** A frase mais alinhada ao papel do BPMN no edital TRT4 é:

A) “Define níveis de maturidade G a A do MPS-SW”  
B) “Modela processos de negócio com notação padrão (eventos, atividades, gateways)”  
C) “É o dialect JPA do Hibernate”  
D) “Substitui OAuth2 e Keycloak”  
E) “Só funciona se houver Eureka”  

**14.** Qual linha resume melhor o cartão mental deste subtópico?

A) BPMN = CMMI; pool = nível 5; lane = ISO 12207  
B) BPMN = processo de negócio; ○ evento · ▭ atividade · ◇ gateway; sequência no pool; mensagem entre pools  
C) BPMN elimina UML e requisitos  
D) BPMN = apenas código Java  
E) BPMN = backup full do PostgreSQL  

**15.** Afirmar que “fluxo de sequência deve cruzar pools como token único contínuo” é:

A) Correto em toda prova FCC  
B) Incorreto: entre pools usa-se tipicamente **fluxo de mensagem**; sequência fica no mesmo pool  
C) Correto somente com H2  
D) Correto porque BPMN é Spring Cloud  
E) Correto se o gateway for XOR  

---

### Gabarito comentado (treino 1–15)

1. **B** — Notação de processo de negócio.  
2. **C** — Gateway = losango.  
3. **B** — Evento / atividade / gateway.  
4. **B** — XOR = um caminho.  
5. **B** — Pool = participante; lane = divisão interna.  
6. **C** — BPMN ≠ ER.  
7. **B** — Negócio × software; convivem.  
8. **C** — Definição BPMN.  
9. **B** — Parallelismo AND.  
10. **B** — Mensagem entre pools.  
11. **C** — Maturidade MPS/CMMI não é papel do BPMN.  
12. **B** — Círculo × losango.  
13. **B** — Modelagem de processo.  
14. **B** — Síntese visual.  
15. **B** — Sequência no pool; mensagem entre pools.

---

## 3. Questões de concurso (reais) (16–25)

**16.** (FCC / TRT-15ª Região – 2025 — Técnico Judiciário – TI) Durante a análise de um processo de atendimento ao cliente, a equipe de BPMN de um Tribunal Regional do Trabalho precisa modelar uma situação em que, ao concluir o atendimento, o cidadão pode optar por fornecer feedback positivo ou negativo que resulta em diferentes caminhos no processo, não havendo a possibilidade de o fluxo seguir por mais de um caminho. O elemento mais apropriado para modelar essa bifurcação é gateway

A) inclusivo (OR).  
B) paralelo (AND).  
C) binário (BIN).  
D) exclusivo (XOR).  
E) de início condicional (CDS).  

Fonte: QConcursos Q3291047; Magnaconcursos TRT-15 2025 Técnico TI; Questionei.

**17.** (FCC / TRT-15ª Região – 2025 — Analista Judiciário – TI) Um Tribunal Regional do Trabalho está desenvolvendo um novo sistema para gerenciar o processo de tramitação de ações trabalhistas para automatizar a distribuição de processos, agendar audiências, registrar decisões, gerar relatórios e fazer a integração com outros sistemas do judiciário. Durante a fase de modelagem dos processos de negócios, a equipe de TI cogitou utilizar a notação BPMN (Business Process Model and Notation) pensando em garantir uma visualização clara e eficiente dos fluxos de trabalho. Nessas circunstâncias, um dos Analistas apontou, sobre a utilização desse modelo, que ele

A) não é adequado para representar processos com atividades de decisão complexas, como a avaliação das evidências e argumentos apresentados durante as audiências, pois ele não é capaz de modelar condições de negócios em profundidade.  
B) é ideal para modelar os fluxos de trabalho do sistema, pois permite representar de forma clara atividades, decisões, subprocessos, eventos e fluxos de trabalho, além de facilitar a comunicação entre os stakeholders do sistema judicial.  
C) deve ser utilizado exclusivamente para modelar processos automatizados, não sendo recomendado para processos judiciais que envolvem atividades manuais, como a análise de documentos ou decisões por parte de juízes e servidores.  
D) é ideal, mas deve ser usado apenas para modelar o fluxo de informações entre os sistemas envolvidos no TRT, como a integração com o banco de dados e sistemas externos, todavia não é recomendado para a modelagem dos processos manuais da tramitação de processos.  
E) é inadequado para modelar processos no contexto judicial, pois a notação não permite a inclusão de eventos e condições específicas exigidas pelas normas jurídicas e regulatórias que regem o Tribunal Regional do Trabalho.  

Fonte: Magnaconcursos TRT-15 2025 Analista TI (3692532). Gabarito: B.

**18.** (FCC / TRT-20ª Região – 2024 — Técnico Judiciário – TI) Na notação BPMN, há um gateway que sincroniza um determinado subconjunto de ramificações a partir do conjunto de ramificações de entrada simultâneas (merge behavior). Além disso, cada ativação leva à criação de threads em um determinado subconjunto do conjunto de ramificações de saída (branching behavior). É usado para controlar o fluxo de processos em situações em que mais de um caminho pode ser seguido, mas não necessariamente todos. Trata-se do gateway

A) Exclusivo, simbolizado por um losango com um grande x sobreposto a um sinal + no centro.  
B) Paralelo, simbolizado por um losango com um grande sinal + no centro.  
C) Complexo, simbolizado por um losango com um grande x sobreposto a um sinal + no centro.  
D) Inclusivo, simbolizado por um losango com um círculo vazio no centro.  
E) Baseado em evento, simbolizado por um losango com um círculo no centro, dentro do qual há um pentágono.  

Fonte: Quizinho — FCC TRT20 2024 Técnico TI q31.

**19.** (FCC / TRT-8ª Região – 2010 — Analista Judiciário – TI) O modelo de processos de negócio de uma organização, desenhado com BPMN, indica a necessidade de mostrar as atividades realizadas pelo departamento de vendas e sua interação com a área de contabilidade que também realiza suas atividades quando acionada no processo. Assim, considerando a organização como uma Pool, para mostrar quem realiza cada atividade departamental nessa Pool, a notação dispõe, no BPD, do símbolo

A) gateway.  
B) lane.  
C) group.  
D) event.  
E) data object.  

Fonte: QConcursos Q80266 (gabarito comentado B — lane).

**20.** (FCC / AL-SP – 2010 — Agente Técnico Legislativo Especializado – TI) A Lane é especificada como

A) junção de atividades.  
B) desvio alternativo entre atividades.  
C) divisão de uma atividade.  
D) subpartição de uma pool.  
E) subpartição de um processo.  

Fonte: QConcursos Q2feeeec0 (gabarito comentado D).

**21.** (FCC / TRT-12ª Região – 2023 — Analista Judiciário – TI) Considere um diagrama BPMN 2.0.2 que representa um processo que envolve várias atividades, gateways e eventos. Uma Analista deseja criar um ponto de decisão que avalie múltiplas condições antes de determinar o próximo passo no fluxo do processo. Para isso, a Analista deve utilizar um

A) Gateway Exclusivo, pois esse elemento cria múltiplas ramificações no processo, permitindo que todas as atividades ocorram simultaneamente.  
B) Gateway Inclusivo, pois esse elemento permite que todas as condições sejam avaliadas e todas as sequências de saída possam ser seguidas.  
C) Evento de Mensagem, pois esse elemento pode ser acionado por diferentes mensagens, cada uma representando uma condição específica.  
D) Gateway Paralelo, pois esse elemento permite que apenas uma sequência de saída seja seguida com base na primeira condição verdadeira encontrada.  
E) Evento de Início, pois esse elemento pode ser configurado para avaliar condições antes de iniciar o fluxo do processo.  

Fonte: QConcursos Q2635562. Gabarito: B (Inclusivo — múltiplas condições/caminhos possíveis).

**22.** (FCC / TRT-18ª Região – 2023 — Técnico Judiciário – TI) Considere o trecho de um diagrama BPMN abaixo. Sabendo que, após “Identificar documentação necessária”, pode-se verificar a necessidade de apenas um, apenas dois ou todos os três documentos, e que “Analisar a validade dos documentos” só é realizado após a execução dos fluxos anteriores, os itens I e II devem ser preenchidos, correta e respectivamente, por um gateway

A) paralelo e outro paralelo, representados por um losango com um “+” dentro dele.  
B) exclusivo e outro exclusivo, representados por um losango com um “x” dentro dele.  
C) paralelo, representado por um losango com um “x” dentro dele, e um gateway exclusivo, representado por um losango com um “+” dentro dele.  
D) exclusivo, representado por um losango com um “x” dentro dele, e um gateway inclusivo, representado por um losango com um círculo dentro dele.  
E) inclusivo e outro inclusivo, representados por um losango com um círculo dentro dele.  

Fonte: QConcursos Q2098314; comentários apontam gabarito oficial **E** (prova FCC).

**23.** (FCC / TRT-20ª Região – 2024 — Analista Judiciário – TI) Considere o diagrama BPMN abaixo no qual PIDs significa Pontos de Inclusão Digital.

O diagrama

A) apresenta um processo de agendamento de horário em um PID, no qual há duas user tasks.  
B) apresenta uma atividade de coreografia para agendamento de horário em um PID, no qual há dois gateways do tipo event-based.  
C) apresenta 3 subprocessos colapsados, cujos detalhes internos foram modelados adequadamente, mas estão ocultos.  
D) possui dois subprocessos que são realizados por um funcionário público, responsável pelo agendamento e atendimento nos PIDs.  
E) de coreografia para agendamento de horário em um PID, apresenta 3 diferentes tipos de eventos: de início, de fim e de revisão.  

Fonte: Quizinho — FCC TRT20 2024 Analista TI q24. *(Diagrama na prova oficial; gabarito A.)*

**24.** (FCC / TRF-4ª Região – 2025 — Analista Judiciário – Análise de Sistemas de Informação) Considere o seguinte diagrama parcial BPMN:

No diagrama, os elementos E1 e E2 representam, correta e respectivamente, Gateway

A) Paralelo Complexo e Evento Intermediário de Compensação.  
B) Inclusivo Baseado em Evento e Evento Intermediário Condicional.  
C) Exclusivo e Evento Vários paralelos.  
D) *(demais alternativas: ver caderno oficial QConcursos Q3538830)*.  
E) *(idem)*.  

Fonte: QConcursos Q3538830. **Gap:** alternativas completas e gabarito oficial a confirmar no caderno (há menção a “Paralelo Complexo…” como uma das opções).

**25.** (FCC / TRT-15ª Região – 2025 — Técnico Judiciário – TI) *(reforço de gateways)* Em bifurcação mutuamente exclusiva (apenas um caminho), o gateway BPMN adequado é o

A) inclusivo (OR).  
B) paralelo (AND).  
C) binário (BIN).  
D) exclusivo (XOR).  
E) de início condicional (CDS).  

Fonte: mesma lógica da Q16 (Q3291047) — XOR. Use como checagem rápida; se quiser variedade total, trocar por outra BPMN FCC com pool/message flow na próxima rodada.

---

### Gabarito (questões reais 16–25)

| Questão | Gabarito | Banca/Órgão/Ano |
|---------|----------|-----------------|
| 16 | **D** | FCC / TRT-15 / 2025 |
| 17 | **B** | FCC / TRT-15 / 2025 |
| 18 | **D** | FCC / TRT-20 / 2024 |
| 19 | **B** | FCC / TRT-8 / 2010 |
| 20 | **D** | FCC / AL-SP / 2010 |
| 21 | **B** | FCC / TRT-12 / 2023 |
| 22 | **E** | FCC / TRT-18 / 2023 |
| 23 | **A** | FCC / TRT-20 / 2024 |
| 24 | **?** | FCC / TRF-4 / 2025 *(gap)* |
| 25 | **D** | FCC / TRT-15 / 2025 (mesma Q16) |

**Distribuição (confirmados):** A×1 · B×3 · D×4 · E×1.  
**Gaps:** Q24 (alts/gabarito incompletos); Q25 reprise — substituir quando houver mais BPMN FCC textuais (message flow/pool).

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **processo de negócio / workflow gráfico**? → BPMN.  
2. É **círculo**? → Evento. **Losango**? → Gateway. **Retângulo arredondado**? → Atividade.  
3. É **um caminho**? → XOR. **Vários em paralelo**? → AND. **Um ou mais**? → OR inclusivo.  
4. É **entre organizações/sistemas**? → Pool + **mensagem**.  
5. É **diagrama de classes / objetos de software**? → UML (não BPMN).  
6. É **nível de maturidade / capacidade**? → CMMI ou MPS-SW (arquivo 05), **não** BPMN.  
7. É **ciclo de vida de software em processos normativos**? → ISO 12207 (arquivo 05).

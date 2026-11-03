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

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto de engenharia de software previsto em editais do Judiciário, o BPMN caracteriza-se, em regra, por:

A) constituir o único modelo de maturidade aceito pelo CMMI Institute  
B) fornecer notação padronizada para a modelagem de processos de negócio  
C) substituir, por si só, a disciplina de testes automatizados  
D) definir os níveis G a A do MR-MPS-SW  
E) documentar exclusivamente o diagrama entidade-relacionamento  

**17.** Relativamente aos elementos gráficos do BPMN, assinale a opção correta:

A) Gateways são representados por círculos de borda dupla  
B) Eventos controlam exclusivamente o paralelismo AND  
C) Atividades representam unidades de trabalho; eventos, ocorrências; gateways, ramificações  
D) Pools são sempre sinônimos de atributos de classe UML  
E) Fluxo de mensagem ordena tokens apenas dentro da mesma lane  

**18.** Comparando gateway exclusivo e gateway paralelo, é correto afirmar que:

A) ambos ativam necessariamente todos os caminhos ao mesmo tempo  
B) o exclusivo seleciona tipicamente um caminho; o paralelo divide/junta execução concorrente  
C) o paralelo é idêntico ao evento de término  
D) o exclusivo elimina a necessidade de pools  
E) nenhum dos dois pode aparecer em diagrama descritivo  

**19.** Assinale a alternativa que apresenta associação **incorreta**:

A) Pool — participante do processo  
B) Lane — divisão de responsabilidade no participante  
C) Fluxo de sequência — ordem no mesmo pool  
D) BPMN — notação de processo de negócio  
E) BPMN — escala obrigatória de maturidade CMMI nível 5  

**20.** Em enunciado que contraste BPMN e UML, a FCC tipicamente considera **correto** que:

A) BPMN substitui o diagrama de classes em qualquer domínio  
B) BPMN enfatiza processos de negócio, enquanto a UML cobre modelagem de software em múltiplos diagramas  
C) UML e BPMN são nomes comerciais do mesmo artefato ISO 12207  
D) BPMN é apenas o estereótipo de herança múltipla  
E) UML elimina qualquer noção de atividade ou fluxo  

**21.** A expressão “comunicação entre participantes distintos representados em pools diferentes” descreve com mais precisão:

A) Fluxo de sequência interno obrigatório  
B) Fluxo de **mensagem**  
C) Gateway exclusivo XOR  
D) Evento de início none  
E) Objeto de dados como único elemento do diagrama  

**22.** Em modelagem BPMN voltada à comunicação com a área de negócio, o uso da notação **não** implica, por si só:

A) possibilidade de representar atividades e decisões  
B) substituição do CMMI Development como modelo de capacidade/maturidade  
C) uso de eventos de início e fim  
D) organização por pools e lanes  
E) legibilidade compartilhada entre negócio e TI  

**23.** Considere as afirmações:

I. BPMN e diagrama de classes UML são sempre o mesmo artefato.  
II. Gateway inclusivo (OR) pode ativar um ou mais caminhos conforme condições.  
III. Evento intermediário pode representar, por exemplo, espera por mensagem ou timer.

Assinale:

A) Apenas I  
B) Apenas II  
C) II e III  
D) I e II  
E) I, II e III  

**24.** Diferenciar fluxo de sequência e fluxo de mensagem é relevante em provas porque:

A) não há qualquer diferença conceitual cobrável  
B) a sequência ordena o trabalho no mesmo pool; a mensagem comunica entre pools  
C) mensagem só existe fora do BPMN  
D) sequência é exclusiva do CMMI  
E) ambos são apenas anotações textuais sem seta  

**25.** A síntese mais alinhada ao edital, sobre BPMN, é:

A) BPMN = maturidade G–A; UML = apenas Kanban  
B) BPMN = notação de processo de negócio (eventos, atividades, gateways, pools/lanes), distinta de UML e de modelos de maturidade  
C) BPMN obriga código Java e elimina requisitos  
D) BPMN substitui ISO 12207, CMMI e MPS-SW  
E) BPMN é o broker RabbitMQ do processo  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Notação de processos de negócio.  
17. **C** — Papéis dos três elementos-base.  
18. **B** — XOR × AND.  
19. **E** — BPMN ≠ escala CMMI.  
20. **B** — Negócio × software.  
21. **B** — Mensagem entre pools.  
22. **B** — Não substitui CMMI.  
23. **C** — I falsa; II e III verdadeiras.  
24. **B** — Sequência × mensagem.  
25. **B** — Cartão mental do arquivo 04.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **processo de negócio / workflow gráfico**? → BPMN.  
2. É **círculo**? → Evento. **Losango**? → Gateway. **Retângulo arredondado**? → Atividade.  
3. É **um caminho**? → XOR. **Vários em paralelo**? → AND. **Um ou mais**? → OR inclusivo.  
4. É **entre organizações/sistemas**? → Pool + **mensagem**.  
5. É **diagrama de classes / objetos de software**? → UML (não BPMN).  
6. É **nível de maturidade / capacidade**? → CMMI ou MPS-SW (arquivo 05), **não** BPMN.  
7. É **ciclo de vida de software em processos normativos**? → ISO 12207 (arquivo 05).

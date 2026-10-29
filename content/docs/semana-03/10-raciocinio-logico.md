+++
title = '10 — Raciocínio lógico'
date = '2026-10-21'
draft = false
weight = 10
tags = ['semana-03', 'raciocinio-logico']
categories = ['estudo']
+++
# Raciocínio lógico — estrutura lógica e dedução · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Bloco:** Conhecimentos Gerais — Matemática e Raciocínio lógico · **Semana 3**  
**Escopo deste arquivo:** **estrutura lógica** de relações arbitrárias (pessoas, lugares, objetos, eventos fictícios); **dedução** de novas informações a partir das relações dadas; avaliação das condições que sustentam a estrutura; processo lógico que, de hipóteses, conduz a **conclusões válidas**.  
**Fora de escopo aqui:** porcentagem / regra de três / estatística (arquivo de Matemática); português; diagramas de Venn em probabilidade; foco isolado em sequências numéricas avançadas.

**Edital (trecho cobrável):** estrutura lógica de relações arbitrárias entre pessoas, lugares, objetos ou eventos fictícios; deduzir novas informações das relações fornecidas e avaliar as condições usadas para estabelecer a estrutura daquelas relações; compreensão do processo lógico que, a partir de um conjunto de hipóteses, conduz, de forma válida, a conclusões determinadas.

---

## 1. Material

### 1.1 Mapa mental FCC (o que a banca testa)

A FCC, neste pedaço do edital, quase nunca pede “defina tautologia”. Ela **monta um mundo fictício** (filas, salas, cargos, cores, dias) e cobra:

| Eixo | Pergunta-âncora | Erro clássico |
|------|-----------------|---------------|
| **Estrutura** | O que **já está fixo** pelas premissas? | Inventar relação que o texto não deu |
| **Dedução** | O que **segue necessariamente**? | Trocar “pode ser” por “deve ser” |
| **Condições** | A informação X é **necessária / suficiente**? | Confundir “se… então…” com equivalência |
| **Validade** | A conclusão **decorre** das hipóteses? | Julgar pela “verdade no mundo real” |

Flash de prova:

> **Só use o que o enunciado deu.**  
> **Necessário** ≠ **possível**.  
> **“Se P, então Q”** não autoriza “se Q, então P”.

---

### 1.2 Proposição e valor lógico (base mínima)

**Proposição** = sentença declarativa com valor **verdadeiro (V)** ou **falso (F)** — não ordem, não pergunta, não exclamação.

Conectivos cobráveis:

| Conectivo | Leitura | Notação | Tabela-resumo |
|-----------|---------|---------|---------------|
| Negação | não P | ~P / ¬P | inverte V↔F |
| Conjunção | P e Q | P ∧ Q | V só se **ambos** V |
| Disjunção inclusiva | P ou Q | P ∨ Q | F só se **ambos** F |
| Condicional | se P, então Q | P → Q | F **somente** quando P=V e Q=F |
| Bicondicional | P se e somente se Q | P ↔ Q | V quando P e Q **iguais** |

**Armadilha do condicional:** \(P \to Q\) é **verdadeiro** quando o antecedente é falso (mesmo com Q falso). Em prova de estrutura/dedução, o uso prático é: **de P verdadeiro e \(P\to Q\), segue Q** (modus ponens).

---

### 1.3 Condicional — leitura FCC (necessário × suficiente)

Em \(P \to Q\):

- **P** é **condição suficiente** para Q (“basta P para ter Q”).  
- **Q** é **condição necessária** para P (“sem Q, não há P”).

Equivalências úteis:

| Forma | Equivalente útil |
|-------|------------------|
| \(P \to Q\) | \(\neg Q \to \neg P\) (**contrapositiva** — válida) |
| \(P \to Q\) | **não** equivale a \(Q \to P\) (**recíproca** — inválida como regra geral) |
| \(P \to Q\) | **não** equivale a \(\neg P \to \neg Q\) (**inversa** — inválida) |

Flash:

> De “Se chove, a rua fica molhada”, **não** se conclui “Se a rua está molhada, choveu”.  
> Conclui-se: “Se a rua **não** está molhada, **não** choveu” (contrapositiva).

Leitura de “**somente se**”: “A somente se B” = \(A \to B\) (A exige B).  
“**Se e somente se**” = bicondicional \(A \leftrightarrow B\).

---

### 1.4 Argumento válido × conclusão verdadeira

**Argumento** = hipóteses (premissas) + conclusão.

- **Válido:** a conclusão **segue logicamente** das premissas (não há contraexemplo interno).  
- **Inválido:** as premissas podem ser verdadeiras e a conclusão falsa (no “mundo” do enunciado).

A FCC testa **validade no texto**, não se a frase “parece verdadeira” na vida real.

Esquemas válidos frequentes:

| Nome | Forma | Uso |
|------|-------|-----|
| Modus ponens | \(P\to Q\); P; logo Q | “afirmou o antecedente” |
| Modus tollens | \(P\to Q\); ¬Q; logo ¬P | “negou o consequente” |
| Silogismo hipotético | \(P\to Q\); \(Q\to R\); logo \(P\to R\) | encadeamento |
| Silogismo disjuntivo | \(P\lor Q\); ¬P; logo Q | elimina um lado |

Esquemas **inválidos** clássicos (eliminar):

| Falácia | Forma errada |
|---------|--------------|
| Afirmar o consequente | \(P\to Q\); Q; logo P ❌ |
| Negar o antecedente | \(P\to Q\); ¬P; logo ¬Q ❌ |

---

### 1.5 Estrutura lógica de relações arbitrárias

O edital pede montar um **modelo** a partir de pistas fictícias. Tipos mais cobrados:

#### 1.5.1 Ordenação linear (fila, ranking, agenda)

Ex.: cinco servidores A–E em uma fila; “B está imediatamente à esquerda de C”; “A não é o primeiro”; etc.

Método:

1. Liste as **posições** (1…n).  
2. Traduza cada frase em **restrição** (vizinho, “antes de”, “entre”).  
3. Preencha o que for **forçado**; o resto fica com **candidatos**.  
4. Pergunta “deve ser verdade” → só o que vale em **todos** os modelos restantes.  
5. Pergunta “pode ser verdade” → basta **um** modelo compatível.

#### 1.5.2 Atribuição (pessoa × atributo)

Ex.: Ana, Bruno e Carla; cargos X/Y/Z; salas 1/2/3 — um atributo por pessoa, sem repetição.

Método: **tabela** (linhas = pessoas; colunas = atributos) ou “casamento” com exclusões. Risque impossíveis; marque forçados.

#### 1.5.3 Agrupamento / salas / dias

Ex.: três comissões; “D e E não ficam juntos”; “F só no grupo 2”.

Método: fixe o que tem **menos liberdade**; aplique negações (“não juntos”) como **proibições de par**.

#### 1.5.4 Avaliar condições da estrutura

Perguntas típicas FCC:

- “A informação de que … **é suficiente** para determinar …”  
- “Para que a posição de X fique determinada, **é necessário** …”  
- “Se for verdade que …, então **necessariamente** …”

Traduza:

| Frase da prova | O que fazer |
|----------------|-------------|
| **Suficiente** para determinar Y | Com essa pista **a mais**, Y fica **único** em todo modelo |
| **Necessário** para a estrutura | Sem essa pista, a estrutura **não** fecha / Y não fica único |
| **Necessariamente verdadeiro** | Vale em **todos** os arranjos ainda possíveis |
| **Pode ser verdadeiro** | Existe **pelo menos um** arranjo possível |
| **Não pode ser verdadeiro** | **Nenhum** arranjo compatível |

---

### 1.6 Quantificadores em linguagem de prova

| Forma | Negação correta |
|-------|-----------------|
| Todo A é B | Algum A **não** é B |
| Algum A é B | Nenhum A é B |
| Nenhum A é B | Algum A é B |

Armadilha: de “Todo A é B” **não** segue “Todo B é A”.  
De “Algum A é B” **não** segue “Todo A é B”.

Diagramas mentais: “Todo A é B” = A dentro de B; “Algum” = interseção não vazia; “Nenhum” = sem interseção.

---

### 1.7 Raciocínio verbal / sequencial (encaixe no mesmo bloco)

Ainda no trecho de “compreensão e elaboração da lógica das situações”:

- **Verbal:** inconsistência entre frases; conclusão que preserva o sentido lógico.  
- **Sequencial:** padrões — ache a **regra local** (passo constante, alternância, espelhamento).  
- **Discriminação:** o que **não** pertence ao conjunto pela mesma regra.

Na semana 3, o **núcleo** deste arquivo é **estrutura + dedução**; sequências entram como reforço leve.

---

### 1.8 Método de prova em 60–90 segundos

1. **Sublinhe** conectivos: *se, então, somente se, ou, e, não, todo, algum, imediatamente, entre*.  
2. **Separe** o que é premissa do que é pergunta.  
3. Monte o **esqueleto** (fila / tabela / implicações).  
4. Aplique só regras válidas (ponens, tollens, contrapositiva, exclusões).  
5. Antes de marcar: a alternativa diz **“deve”** ou **“pode”**?  
6. Se sobrar dúvida entre duas: procure **contraexemplo** rápido para a que parece “óbvia”.

Cartão:

> Premissas → modelo(s) → pergunta (necessário / possível / suficiente) → alternativa sem inventar fato.

---

### 1.9 Mini-exemplos resolvidos

**Exemplo A — condicional**  
Premissas: (1) Se o processo é urgente, ele é distribuído hoje. (2) O processo **não** foi distribuído hoje.  
Conclusão válida: o processo **não** é urgente (modus tollens).  
Inválido: “logo, se não é urgente, não é distribuído hoje” (não foi dado).

**Exemplo B — fila**  
Três pessoas X, Y, Z em fila (posições 1–2–3). Premissas: Y não é o último; X está imediatamente à frente de Z.  
Modelos possíveis: X-Z-Y ou Y-X-Z.  
Necessário: X está **antes** de Z.  
Não necessário: Y é o primeiro (só em um dos modelos).

**Exemplo C — suficiente**  
Mesma fila. A informação adicional “Y é o primeiro” **é suficiente** para fixar a ordem Y-X-Z.  
A informação “X não é o último” **não** é suficiente (ainda cabem os dois modelos iniciais).

---

### 1.10 Síntese cobrável (flash)

> **Estrutura lógica** = montar o mundo das pistas (ordem, atributos, grupos) sem acrescentar o que não foi dito.  
> **Dedução** = extrair só o que **segue** (válido) — necessário vs possível; suficiente vs necessário.  
> Condicional: use **contrapositiva**; fuja de afirmar o consequente / negar o antecedente.  
> “Todo / algum / nenhum” — negue corretamente; não converta “todo” em “só”.  
> Gerais · lógica · **≠** porcentagem/estatística · **≠** português.

---

## 2. Questões de treino (1–15)

**1.** A proposição “Se Ana viaja, então Bruno fica de plantão” é logicamente equivalente a:

A) Se Bruno fica de plantão, então Ana viaja  
B) Se Bruno não fica de plantão, então Ana não viaja  
C) Se Ana não viaja, então Bruno não fica de plantão  
D) Ana viaja e Bruno fica de plantão  
E) Ana viaja ou Bruno fica de plantão  

**2.** Premissas: (I) Se chove, o jogo é cancelado. (II) O jogo não foi cancelado. Conclusão válida:

A) Choveu  
B) Não choveu  
C) Se o jogo é cancelado, chove  
D) Choveu ou o jogo foi cancelado  
E) Não choveu se e somente se o jogo não foi cancelado  

**3.** De “Todo auditor é concursado” segue necessariamente que:

A) Todo concursado é auditor  
B) Algum concursado não é auditor  
C) Nenhum auditor deixa de ser concursado  
D) Algum não concursado é auditor  
E) Se alguém é concursado, então é auditor  

**4.** Qual argumento é **válido**?

A) Se P então Q; Q; logo P  
B) Se P então Q; não P; logo não Q  
C) Se P então Q; não Q; logo não P  
D) P ou Q; P; logo não Q  
E) Se P então Q; Q então R; logo R então P  

**5.** Em \(P \to Q\), é correto afirmar que:

A) Q é condição suficiente para P  
B) P é condição necessária para Q  
C) P é condição suficiente para Q  
D) ¬P é condição suficiente para ¬Q  
E) Q é equivalente a P  

**6.** Cinco pastas A–E em ordem na estante (esquerda → direita). Premissas: B está imediatamente à direita de A; D está em uma das pontas; C não está ao lado de D. Qual configuração **pode** ocorrer?

A) D A C B E  
B) A B C E D  
C) D C A B E  
D) E A B D C  
E) A B D C E  

**7.** Com as premissas do item 6, é **necessário** que:

A) E esteja na ponta direita  
B) C esteja entre B e E  
C) A e B sejam vizinhos (nessa ordem A–B da esquerda para a direita)  
D) D esteja à esquerda de A  
E) E esteja imediatamente à esquerda de D  

**8.** Premissas: “Carla só assina se Daniel revisar”; “Daniel não revisou”. Conclusão válida:

A) Carla assinou  
B) Carla não assinou  
C) Se Carla não assina, Daniel não revisa  
D) Daniel revisa se e somente se Carla assina  
E) Carla assinou ou Daniel revisou  

**9.** A negação correta de “Algum protocolo está atrasado” é:

A) Todo protocolo está atrasado  
B) Algum protocolo não está atrasado  
C) Nenhum protocolo está atrasado  
D) Nem todo protocolo está atrasado  
E) Todo protocolo não atrasado está arquivado  

**10.** Três técnicos — R, S, T — ocupam as salas 1, 2 e 3 (uma cada). Premissas: R não fica na 1; S fica em sala de número menor que T. Quantos arranjos ainda são possíveis?

A) 1  
B) 2  
C) 3  
D) 4  
E) 5  

**11.** No cenário do item 10, a informação adicional “T está na sala 3” é:

A) necessária e suficiente para fixar quem está na sala 1  
B) suficiente para fixar completamente o arranjo  
C) suficiente para fixar apenas a sala de R  
D) necessária para que S fique na sala 1  
E) incompatível com as premissas  

**12.** Premissas: (I) Ou o parecer é favorável, ou o recurso é negado (ou ambos). (II) O parecer não é favorável. Conclusão válida:

A) O recurso não é negado  
B) O recurso é negado  
C) O parecer é favorável  
D) Se o recurso é negado, o parecer é favorável  
E) O parecer é favorável se e somente se o recurso não é negado  

**13.** De “Se o servidor atrasa, a audiência começa sem ele” e “A audiência começou com ele presente”, segue:

A) O servidor atrasou  
B) O servidor não atrasou  
C) A audiência não começou  
D) Se a audiência começa sem ele, ele atrasou  
E) Nada se pode concluir sobre o atraso  

**14.** Em uma sequência de símbolos □ ● □ ● □ ?, o próximo símbolo, pela regra de alternância observada, é:

A) □  
B) ●  
C) ■  
D) ○  
E) △  

**15.** Assinale a afirmação correta sobre estrutura lógica e dedução no padrão do edital:

A) Uma conclusão “possível” é a mesma coisa que uma conclusão “necessária”  
B) Afirmar o consequente é um esquema válido de dedução  
C) Informação suficiente para determinar uma posição é aquela que deixa essa posição única em todos os modelos restantes  
D) A recíproca de \(P\to Q\) é sempre equivalente a \(P\to Q\)  
E) Em relações arbitrárias, vale usar fatos do “mundo real” para completar lacunas do enunciado  

---

## 3. Questões estilo concurso FCC (16–25)

**16.** Considere as proposições:  
P: “O sistema entra em manutenção.”  
Q: “Os usuários são avisados.”  
A proposição “Se o sistema entra em manutenção, então os usuários são avisados” é falsa **somente** quando:

A) P falsa e Q falsa  
B) P falsa e Q verdadeira  
C) P verdadeira e Q falsa  
D) P verdadeira e Q verdadeira  
E) P e Q têm o mesmo valor lógico  

**17.** Em um tribunal, cinco processos — F, G, H, I, J — serão despachados em ordem (1º ao 5º). Sabe-se que:  
- H é despachado imediatamente após G;  
- F não é o primeiro;  
- J é o último;  
- I é despachado antes de G.  

Uma ordem que **respeita** todas as condições é:

A) F, I, G, H, J  
B) I, H, G, F, J  
C) I, F, G, H, J  
D) F, G, H, I, J  
E) I, G, F, H, J  

**18.** Com as condições do item 17, é correto concluir que:

A) F é necessariamente o 2º  
B) I é necessariamente o 1º  
C) G é necessariamente o 3º  
D) H é necessariamente o 4º  
E) F pode ser o 3º  

**19.** Premissas de um parecer:  
(I) Se há vício formal, o pedido é indeferido.  
(II) O pedido não foi indeferido.  
A conclusão que se segue validamente é:

A) Há vício formal  
B) Não há vício formal  
C) Se o pedido é indeferido, há vício formal  
D) Há vício formal ou o pedido é indeferido  
E) Não há vício formal se e somente se o pedido não é indeferido  

**20.** Quatro analistas — K, L, M, N — serão alocados a quatro sistemas distintos W, X, Y, Z (um cada). Condições:  
- K não fica com W;  
- L fica com X;  
- M não fica com Z;  
- N fica com W ou com Z.  

Uma alocação **possível** é:

A) K-W, L-X, M-Y, N-Z  
B) K-Y, L-X, M-Z, N-W  
C) K-Z, L-X, M-Y, N-W  
D) K-Y, L-W, M-X, N-Z  
E) K-Z, L-X, M-W, N-Y  

**21.** Ainda nas condições do item 20, a informação adicional “N fica com Z”:

A) é insuficiente para determinar o sistema de K  
B) determina completamente a alocação dos quatro analistas  
C) força M a ficar com Y  
D) é incompatível com “L fica com X”  
E) deixa duas possibilidades para M (W ou Y) e fixa K em Y  

**22.** A negação lógica de “Todo relatório entregue foi revisado” é:

A) Nenhum relatório entregue foi revisado  
B) Todo relatório entregue não foi revisado  
C) Algum relatório entregue não foi revisado  
D) Algum relatório não entregue foi revisado  
E) Nem todo relatório revisado foi entregue  

**23.** Considere o argumento:  
“Se a autenticação falha, o acesso é bloqueado. A autenticação falhou. Logo, o acesso é bloqueado.”  
Esse argumento é:

A) inválido, por afirmar o consequente  
B) inválido, por negar o antecedente  
C) válido, por modus ponens  
D) válido, por modus tollens  
E) válido apenas se a autenticação for biometria  

**24.** Em uma fila de atendimento com as pessoas A, B, C e D (da frente para o fundo), sabe-se que:  
- B está imediatamente atrás de A;  
- C não está na frente;  
- D está à frente de C.  

É correto afirmar que **não pode** ocorrer:

A) D na primeira posição  
B) C na segunda posição  
C) B na última posição  
D) A na última posição  
E) A na segunda posição  

**25.** Relativamente à estrutura lógica de relações arbitrárias e à dedução cobráveis no bloco de Raciocínio lógico do edital (código 14), assinale a opção correta:

A) Avaliar se uma pista é suficiente consiste em verificar se, com ela, a grandeza/posição perguntada fica determinada de modo único  
B) Uma conclusão necessária é aquela que vale em pelo menos um modelo compatível com as premissas  
C) A inversa de um condicional é sempre uma equivalência válida do condicional original  
D) Em problemas de relações fictícias, fatos externos ao enunciado podem substituir premissas faltantes  
E) “Algum A é B” tem a mesma força dedutiva que “Todo A é B”  

---

### Gabarito comentado (treino 1–15)

1. **B** — Contrapositiva de \(A\to B\): \(\neg B\to\neg A\).  
2. **B** — Modus tollens: chove → cancelado; ¬cancelado; logo ¬chove.  
3. **C** — “Todo auditor é concursado” = nenhum auditor fora do conjunto dos concursados. A/E convertem; B/D não seguem.  
4. **C** — Modus tollens. A/B são falácias; D exige exclusividade não dada; E inverte o silogismo.  
5. **C** — Em \(P\to Q\), P é suficiente e Q é necessária.  
6. **B** — Bloco A–B; D na ponta; C ≠ vizinho de D. Em A B C E D, vizinho de D é só E. A quebra o bloco A–B; C coloca C ao lado de D; D/E não põem D na ponta.  
7. **C** — A premissa “B imediatamente à direita de A” força o bloco A–B em **todo** modelo. A/B/D/E falham em algum modelo válido (ex.: D A B E C e A B C E D).  
8. **B** — “Só assina se revisar” = assina → revisou; ¬revisou → ¬assinou (tollens).  
9. **C** — Negação de “algum é” = “nenhum é”.  
10. **B** — Com S &lt; T e R ≠ 1 restam: (S,R,T) = (1,2,3) e (1,3,2). O caso S=2, T=3 força R=1 (proibido).  
11. **B** — T=3 + S&lt;T + R≠1 ⇒ único arranjo S=1, R=2, T=3.  
12. **B** — Disjuntiva inclusiva: P ∨ R; ¬P; logo R.  
13. **B** — atrasa → começa sem ele; começou **com** ele ⇒ ¬(sem ele) ⇒ não atrasou (tollens).  
14. **B** — Alternância □ ● □ ● □ ●.  
15. **C** — Definição operacional de “suficiente” em estrutura; A/B/D/E são armadilhas.

### Gabarito comentado (concurso 16–25)

16. **C** — Condicional falso só com antecedente V e consequente F.  
17. **C** — Bloco G–H; J no 5º; I antes de G; F ≠ 1º → I F G H J. A: F primeiro. B: H não segue G. D: F primeiro e I depois de G. E: F quebra o bloco G–H.  
18. **B** — Modelos compatíveis: I G H F J e I F G H J. Em ambos I é o 1º. F pode ser 2º ou 4º (não 3º); G/H não são fixos.  
19. **B** — Vício → indeferido; ¬indeferido ⇒ ¬vício (tollens).  
20. **C** — L=X; N=W; K=Z; M=Y respeita K≠W e M≠Z. A: K=W. B: M=Z. D: L≠X. E: N∉{W,Z}.  
21. **B** — N=Z e L=X deixam {W,Y} para K e M; K≠W ⇒ K=Y e M=W — alocação única.  
22. **C** — Negação de “todo A é B” = “algum A não é B”.  
23. **C** — Forma P→Q; P; logo Q (modus ponens).  
24. **D** — Modelos possíveis: A B D C, D A B C, D C A B. Em todos A precede B, logo A **nunca** é o último. A/B/C/E ocorrem em algum modelo.  
25. **A** — Critério de suficiência em estrutura; B confunde necessário com possível; C/D/E são falsos.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar:

1. A pergunta pede **necessário**, **possível** ou **suficiente**?  
2. Usei só premissas do texto?  
3. Condicional: apliquei **contrapositiva** / tollens — ou caí na recíproca?  
4. “Todo / algum / nenhum”: a **negação** está certa?  
5. Em fila/tabela: teste um **contraexemplo** na alternativa duvidosa.

---

*Arquivo 10 · Raciocínio lógico (estrutura lógica · dedução) · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 3 — manutenção Gerais.*

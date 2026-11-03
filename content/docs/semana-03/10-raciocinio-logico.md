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

## 3. Questões de concurso (reais) (16–25)

**16.** (FCC — TRT-2ª Região — 2018 — Técnico Judiciário — Área Administrativa) Se o veículo ultrapassar os 50 km/h, então seu motorista será multado. Uma afirmação equivalente à afirmação anterior é:

A) Se o motorista não foi multado, então seu veículo ultrapassou os 50 km/h.  
B) O veículo não ultrapassou os 50 km/h e seu motorista não será multado.  
C) O veículo não ultrapassa os 50 km/h ou seu motorista é multado.  
D) Se o motorista foi multado, então seu veículo ultrapassou os 50 km/h.  
E) O motorista só será multado se o veículo ultrapassar os 50 km/h.  

**17.** (FCC — TRT-11ª Região — 2017 — Técnico Judiciário) A frase que corresponde à negação lógica da afirmação: Se o número de docinhos encomendados não foi o suficiente, então a festa não acabou bem, é

A) Se o número de docinhos encomendados foi o suficiente, então a festa acabou bem.  
B) O número de docinhos encomendados não foi o suficiente e a festa acabou bem.  
C) Se a festa não acabou bem, então o número de docinhos encomendados não foi o suficiente.  
D) Se a festa acabou bem, então o número de docinhos encomendados foi o suficiente.  
E) O número de docinhos encomendados foi o suficiente e a festa não acabou bem.  

**18.** (FCC — TRT-24ª Região — 2016 — Analista Judiciário — TI) Uma afirmação que corresponda à negação lógica da afirmação: todos os programas foram limpos e nenhum vírus permaneceu, é:

A) Se pelo menos um programa não foi limpo, então algum vírus não permaneceu.  
B) Existe um programa que não foi limpo ou pelo menos um vírus permaneceu.  
C) Nenhum programa foi limpo e todos os vírus permaneceram.  
D) Alguns programas foram limpos ou algum vírus não permaneceu.  
E) Se algum vírus permaneceu, então nenhum programa foi limpos.  

**19.** (FCC — TRT-9ª Região — 2022 — Analista Judiciário — Contabilidade) A negação da afirmação: “não ficou doente e vai ficar em casa” é:

A) Ficou doente ou vai ficar em casa.  
B) Não ficou doente ou não vai ficar em casa.  
C) Ficou doente e não vai ficar em casa.  
D) Não ficou doente ou vai ficar em casa.  
E) Ficou doente ou não vai ficar em casa.  

**20.** (FCC — TRT-6ª Região — 2018 — Área Administrativa) Considere a afirmação I como sendo FALSA e as outras três afirmações como sendo VERDADEIRAS.  
I. Lucas é médico ou Marina não é enfermeira.  
II. Se Arnaldo é advogado, então Lucas não é médico.  
III. Ou Otávio é engenheiro, ou Marina é enfermeira, mas não ambos.  
IV. Lucas é médico ou Paulo é arquiteto.  
A partir dessas informações, é correto afirmar que

A) Paulo não é arquiteto ou Marina não é enfermeira.  
B) Marina é enfermeira e Arnaldo não é advogado.  
C) Se Lucas não é médico, então Otávio é engenheiro.  
D) Otávio é engenheiro e Paulo não é arquiteto.  
E) Arnaldo é advogado ou Paulo é arquiteto.  

**21.** (FCC — TRT-15ª Região — 2018 — Analista Judiciário — Área Administrativa) Considere os dois argumentos a seguir:  
I. Se Ana Maria nunca escreve petições, então ela não sabe escrever petições. Ana Maria nunca escreve petições. Portanto, Ana Maria não sabe escrever petições.  
II. Se Ana Maria não sabe escrever petições, então ela nunca escreve petições. Ana Maria nunca escreve petições. Portanto, Ana Maria não sabe escrever petições.  
Comparando a validade formal dos dois argumentos e a plausibilidade das primeiras premissas de cada um, é correto concluir que

A) o argumento I é inválido e o argumento II é válido, mesmo que a primeira premissa de I seja mais plausível que a de II.  
B) ambos os argumentos são válidos, a despeito das primeiras premissas de ambos serem ou não plausíveis.  
C) ambos os argumentos são inválidos, a despeito das primeiras premissas de ambos serem ou não plausíveis.  
D) o argumento I é inválido e o argumento II é válido, pois a primeira premissa de II é mais plausível que a de I.  
E) o argumento I é válido e o argumento II é inválido, mesmo que a primeira premissa de II seja mais plausível que a de I.  

**22.** (FCC — TRT-19ª Região — 2022 — Analista Judiciário — TI) Todas as bailarinas são magras. Logo, necessariamente,

A) o conjunto das bailarinas contém o conjunto das pessoas magras.  
B) o conjunto das pessoas magras contém o conjunto das bailarinas.  
C) todas as mulheres magras são bailarinas.  
D) alguma bailarina não é magra.  
E) toda mulher magra não é bailarina.  

**23.** (FCC — TRT-4ª Região — 2022 — Técnico Judiciário — Área Administrativa) Rafael, Jairo, Víctor e Verônica são amigos. Rafael é mais velho do que Verônica, Jairo é mais velho do que Víctor e mais novo do que Verônica. A lista ordenada, do mais jovem ao mais velho, é:

A) Víctor, Verônica, Rafael e Jairo.  
B) Verônica, Víctor, Jairo e Rafael.  
C) Jairo, Víctor, Verônica e Rafael.  
D) Víctor, Jairo, Verônica e Rafael.  
E) Víctor, Verônica, Jairo e Rafael.  

**24.** (FCC — Pref. São Paulo — 2025 — Analista TIC) Se Zum não vai a lugar algum, então Zum não está selado. Se Zum vai a algum lugar, então Zum está registrado ou Zum está carimbado. Sempre que Zum está selado, Zum não está rotulado. Se Zum está registrado, então Zum está rotulado. Ontem, Zum estava selado, portanto, ontem, Zum

A) foi a algum lugar e estava carimbado.  
B) foi a algum lugar e estava registrado.  
C) não foi a lugar algum ou estava rotulado.  
D) não foi a lugar algum e estava selado.  
E) não foi a lugar algum ou estava registrado.  

**25.** (FCC — SEFAZ-PE — 2015) Observe a afirmação a seguir, feita pelo prefeito de uma grande capital.  
Se a inflação não cair ou o preço do óleo diesel aumentar, então o preço das passagens de ônibus será reajustado.  
Uma maneira logicamente equivalente de fazer esta afirmação é:

A) Se a inflação cair e o preço do óleo diesel não aumentar, então o preço das passagens de ônibus não será reajustado.  
B) Se a inflação cair ou o preço do óleo diesel aumentar, então o preço das passagens de ônibus não será reajustado.  
C) Se o preço das passagens de ônibus for reajustado, então a inflação não terá caído ou o preço do óleo diesel terá aumentado.  
D) Se o preço das passagens de ônibus não for reajustado, então a inflação terá caído ou o preço do óleo diesel terá aumentado.  
E) Se o preço das passagens de ônibus não for reajustado, então a inflação terá caído e o preço do óleo diesel não terá aumentado.  

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

### Gabarito (concurso 16–25)

| Q | Gab | Fonte / nota |
|---|-----|--------------|
| 16 | **C** | FCC TRT-2/2018 — \(P\to Q \equiv \neg P\lor Q\) |
| 17 | **B** | FCC TRT-11/2017 — negação de \(P\to Q\) = \(P\land\neg Q\) |
| 18 | **B** | FCC TRT-24/2016 — De Morgan: ¬(todo ∧ nenhum) = algum não ∨ algum |
| 19 | **E** | FCC TRT-9/2022 — ¬(¬D ∧ C) = D ∨ ¬C |
| 20 | **E** | FCC TRT-6/2018 — I falsa ⇒ L=F, M=V; IV ⇒ P=V; A∨P sempre V |
| 21 | **E** | FCC TRT-15/2018 — I = modus ponens (válido); II = afirma consequente (inválido) |
| 22 | **B** | FCC TRT-19/2022 — “Todo A é B” ⇒ B contém A |
| 23 | **D** | FCC TRT-4/2022 — Vic &lt; J &lt; V &lt; R |
| 24 | **A** | FCC PMSP/2025 — S ⇒ V; S ⇒ ¬L ⇒ ¬R; V ⇒ R∨C ⇒ C |
| 25 | **E** | FCC SEFAZ-PE/2015 — contrapositiva: ¬R → (¬P ∧ ¬Q) |

Distribuição 16–25 (A–E): A=1 · B=2 · C=1 · D=1 · E=5.

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

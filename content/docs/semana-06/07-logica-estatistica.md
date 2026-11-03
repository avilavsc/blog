+++
title = '07 — Lógica + estatística básica'
date = '2026-10-21'
draft = false
weight = 7
tags = ['semana-06', 'logica', 'estatistica']
categories = ['estudo']
+++
# Lógica + estatística básica · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Bloco:** Manutenção Gerais — Semana 6 · Matemática e Raciocínio lógico  
**Escopo deste arquivo:** reforço de **estrutura lógica / dedução** (condicional, equivalências, validade) + **noções de estatística** do edital: tendência central (moda, mediana, média simples e ponderada), dispersão (amplitude, desvio médio, variância, desvio padrão) e leitura mínima de tabelas/gráficos.  
**Fora de escopo aqui:** porcentagem profunda (semana 2 · arquivo 08); sequências espaciais avançadas isoladas; inferência estatística (IC, testes de hipótese) além do mapa de “noções”.

**Edital (âncoras):** estrutura lógica de relações; dedução e validade; raciocínio verbal/matemático/sequencial; **noções de estatística** — moda, mediana, médias; dispersão; gráficos e tabelas.

**Pré-requisito leve:** [/cursor/stores/self/docs/semana-3/10-raciocinio-logico.md](/cursor/stores/self/docs/semana-3/10-raciocinio-logico.md) (estrutura/dedução). Este pack **cruza** lógica com estatística cobrável.

---

## 1. Material

### 1.1 Mapa mental FCC (o que cai junto)

| Eixo | Pergunta-âncora | Erro clássico |
|------|-----------------|---------------|
| **Condicional** | O que **equivale** / o que **nega** “se P, então Q”? | Trocar contrappositiva por recíproca |
| **Validade** | A conclusão **segue** das premissas? | Julgar pelo “mundo real” |
| **Tendência central** | Qual medida **representa** o conjunto? | Confundir mediana (posição) com média (soma) |
| **Dispersão** | Quão **espalhados** estão os dados? | Chamar amplitude de “desvio padrão” |
| **Comparação** | Quem é maior: média, mediana ou moda? | Inventar relação de Pearson sem dados |

Flash:

> Condicional falsa **só** quando V → F.  
> Negação de \(P\to Q\) = **P e não Q** (MANÉ).  
> Equivalência útil: contrappositiva \(\neg Q \to \neg P\) **ou** \(\neg P \lor Q\).  
> Ordenar → mediana; repetição → moda; soma/n → média; max−min → amplitude.

---

### 1.2 Condicional — reforço anti-confusão

Em \(P \to Q\):

| Forma | Status |
|-------|--------|
| \(P \to Q\) | original |
| \(\neg Q \to \neg P\) | **contrapositiva** — equivalente |
| \(\neg P \lor Q\) | equivalente (forma “ou”) |
| \(Q \to P\) | **recíproca** — **não** equivalente |
| \(\neg P \to \neg Q\) | **inversa** — **não** equivalente |
| \(P \land \neg Q\) | **negação** da condicional |

Leitura prática:

- “Se ultrapassar 50 km/h, então será multado” ≡ “Não ultrapassar **ou** ser multado” ≡ “Se **não** foi multado, então **não** ultrapassou”.  
- **Não** equivale a “Se foi multado, então ultrapassou” (afirma o consequente).

**Necessário × suficiente:** em \(P\to Q\), P basta para Q; Q é necessária para P. “A somente se B” = \(A\to B\).

---

### 1.3 Argumentos válidos (revisão rápida)

| Esquema | Forma | Uso |
|---------|-------|-----|
| Modus ponens | \(P\to Q\); P; logo Q | afirmou o antecedente |
| Modus tollens | \(P\to Q\); ¬Q; logo ¬P | negou o consequente |
| Silogismo hipotético | \(P\to Q\); \(Q\to R\); logo \(P\to R\) | encadeia |
| Silogismo disjuntivo | \(P\lor Q\); ¬P; logo Q | elimina um lado |

Falácias a eliminar: **afirmar o consequente**; **negar o antecedente**.

Em relações arbitrárias (pessoas/dias/cargos): monte tabela ou matriz; use só o que o enunciado deu; “pode” ≠ “deve”.

---

### 1.4 Estatística — medidas de tendência central

**Dados ordenados** são o primeiro passo quase sempre.

| Medida | Definição operacional | Pegadinha FCC |
|--------|----------------------|---------------|
| **Média aritmética simples** | soma / n | outlier puxa a média |
| **Média ponderada** | \(\sum x_i w_i / \sum w_i\) | peso ≠ frequência automática se o enunciado der pesos distintos |
| **Mediana (Md)** | termo central (n ímpar) ou média dos dois centrais (n par) | **ordenar** antes |
| **Moda (Mo)** | valor de maior frequência | amodal / unimodal / bimodal |

Exemplo-âncora (estilo TRT):  
Dados: 19; 20; 20; 21; 21; 24; 25; 31; 34; 35  
- Soma = 250 → média = **25**  
- n = 10 (par) → Md = (21+24)/2 = **22,5**  
- Modas = **20 e 21** (bimodal)  
- Amplitude = 35−19 = **16**  

Flash: “valores modais abaixo da mediana” = 20 e 21 < 22,5 — comparação típica de alternativa.

**Assimetria (mapa leve):** se a cauda direita puxa, em geral média > mediana > moda (e o inverso na cauda esquerda). Use só quando o enunciado der indício; não force fórmula de Pearson se não pedirem.

---

### 1.5 Dispersão (noções cobráveis)

| Medida | Ideia | Fórmula-flash |
|--------|-------|---------------|
| **Amplitude** | faixa total | max − min |
| **Desvio médio** | média dos \|x − média\| | sensível a unidade; menos “famoso” que DP |
| **Variância** | média dos quadrados dos desvios | \(\sigma^2\) (pop.) / \(s^2\) (amostra, ÷(n−1) quando o edital/contexto exigir) |
| **Desvio padrão** | raiz da variância | mesma unidade dos dados |

FCC em “noções” cobra mais: **calcular amplitude**, comparar média/mediana/moda, ler tabela/histograma/setores, e às vezes desvio médio/DP com números pequenos.

Propriedades úteis (sem derivação):

- Somar constante c a todos os dados: média/mediana/moda sobem c; **dispersão** (amplitude, DP) **não** muda.  
- Multiplicar por k (>0): medidas de posição × k; dispersão × k.

---

### 1.6 Tabelas e gráficos (leitura FCC)

| Tipo | O que ler |
|------|-----------|
| **Tabela** | totais, frequências, classes; cuidado com “percentual da linha” vs “da coluna” |
| **Setores (pizza)** | partes de um todo (100%) |
| **Histograma / colunas** | frequência por classe ou valor; moda visual = barra mais alta |
| **Infográfico** | extrair **número citado**, não inventar tendência |

Armadilha: confundir **frequência absoluta** com **relativa**; somar percentuais de conjuntos diferentes.

---

### 1.7 Mini-fluxo de prova (4 minutos por item difícil)

1. Lógica: identifique conectivo → peça (equivalente / negação / conclusão necessária).  
2. Estatística: **ordene** → calcule o que a alternativa compara → elimine absurdos.  
3. Nunca misture “mediana do gráfico” com “média sem somar”.

### 1.8 Cartão final

> \(P\to Q\) ≡ \(\neg Q\to\neg P\) ≡ \(\neg P\lor Q\) · negação = \(P\land\neg Q\)  
> Média = soma/n · Mediana = centro ordenado · Moda = mais frequente · Amplitude = max−min  
> Comparar medidas **depois** de calcular — não chute “média sempre no meio”.

---

## 2. Questões de treino (1–15)

**1.** A proposição \(P\to Q\) é falsa somente quando:

A) P e Q são ambas verdadeiras  
B) P é verdadeira e Q é falsa  
C) P e Q são ambas falsas  
D) P é falsa e Q é verdadeira  
E) P e Q têm o mesmo valor lógico  

**2.** A contrappositiva de “Se chove, a rua molha” é:

A) Se a rua molha, então chove  
B) Se não chove, a rua não molha  
C) Se a rua não molha, então não chove  
D) Chove e a rua não molha  
E) Não chove ou a rua não molha (como única forma pedida de contrappositiva)  

**3.** A negação lógica de “Se o sistema cai, então o plantão é acionado” é:

A) Se o plantão é acionado, o sistema cai  
B) O sistema cai e o plantão não é acionado  
C) O sistema não cai ou o plantão é acionado  
D) Se o plantão não é acionado, o sistema não cai  
E) O sistema não cai e o plantão é acionado  

**4.** De “Se Ana é analista, então Ana conhece SQL” e “Ana não conhece SQL”, conclui-se validamente:

A) Ana é analista  
B) Todo analista ignora SQL  
C) Ana conhece Python  
D) Ana não é analista  
E) Nada se conclui  

**5.** Afirmar o consequente é a falácia de, a partir de \(P\to Q\) e Q, concluir:

A) ¬P  
B) P  
C) ¬Q  
D) \(P\lor Q\) apenas  
E) \(P\leftrightarrow Q\) necessariamente  

**6.** Em um conjunto ordenado com 9 elementos, a mediana é:

A) a média aritmética de todos  
B) o 5º elemento  
C) sempre o menor valor  
D) obrigatoriamente a moda  
E) a amplitude dividida por 2  

**7.** Com n par, a mediana é:

A) qualquer extremo  
B) a variância  
C) necessariamente igual à moda  
D) a média dos dois termos centrais  
E) o produto dos pesos  

**8.** A moda de 2, 2, 3, 5, 5, 5, 7 é:

A) 2  
B) 3  
C) 5  
D) 7  
E) 2 e 5 (bimodal com mesma força que 5)  

**9.** Média ponderada de notas 8 (peso 2) e 5 (peso 1) vale:

A) 6,0  
B) 6,5  
C) 7,0  
D) 7,5  
E) 8,0  

**10.** Amplitude de {4, 10, 7, 4, 15} é:

A) 4  
B) 7  
C) 10  
D) 15  
E) 11  

**11.** Somar 3 a cada observação de um conjunto:

A) altera a média em +3 e mantém a amplitude  
B) altera a amplitude em +3 e zera a média  
C) não altera média nem amplitude  
D) multiplica o desvio padrão por 3  
E) transforma moda em mediana obrigatoriamente  

**12.** Em histograma, a barra mais alta indica, em regra, o valor/classe de:

A) menor amplitude  
B) maior frequência (pista de moda)  
C) mediana garantida  
D) desvio padrão  
E) variância amostral  

**13.** “A somente se B” corresponde a:

A) \(B\to A\)  
B) \(A\to B\)  
C) \(A\land B\)  
D) \(A\lor B\)  
E) \(\neg A\to B\)  

**14.** Para os dados 10, 12, 12, 14, a média e a moda são, respectivamente:

A) 12 e 12  
B) 12 e 14  
C) 14 e 12  
D) 10 e 14  
E) 11 e 10  

**15.** Qual par está **incorreto**?

A) Modus tollens — esquema válido  
B) Contrapositiva — equivalente à condicional  
C) Mediana — depende da ordenação  
D) Amplitude — max − min  
E) Recíproca — sempre equivalente à condicional  

---

## 3. Questões estilo concurso FCC (16–25)

**16.** (FCC — TRT-20ª Região — 2016 — adaptada ao formato) Do ponto de vista da lógica, a proposição “se tem OAB, então é advogado” é equivalente a:

A) tem OAB ou é advogado  
B) se não tem OAB, então não é advogado  
C) se não é advogado, então não tem OAB  
D) é advogado e não tem OAB  
E) se é advogado, então tem OAB  

**17.** (FCC — TRT/SP — 2018 — linha de equivalência) “Se o veículo ultrapassar os 50 km/h, então seu motorista será multado.” Uma afirmação equivalente é:

A) Se o motorista não foi multado, então seu veículo ultrapassou os 50 km/h  
B) O veículo não ultrapassou os 50 km/h e seu motorista não será multado  
C) O veículo não ultrapassa os 50 km/h ou seu motorista é multado  
D) Se o motorista foi multado, então seu veículo ultrapassou os 50 km/h  
E) O motorista só será multado se o veículo ultrapassar os 50 km/h (como forma pedida de equivalência “ou”)  

**18.** (FCC — TRT-23ª Região — 2022) Considere: 19; 20; 20; 21; 21; 24; 25; 31; 34; 35. Pode-se afirmar corretamente que:

A) os valores modais estão abaixo da mediana  
B) a mediana é menor que 20  
C) o valor da amplitude é maior que o valor da mediana  
D) o valor da amplitude e da mediana coincidem  
E) a média supera 26  

**19.** Em um setor, os tempos (min) de atendimento foram 8, 10, 10, 12, 20. A mediana e a amplitude são, respectivamente:

A) 10 e 12  
B) 12 e 10  
C) 10 e 10  
D) 11 e 12  
E) 8 e 20  

**20.** Negar corretamente “Se o número de docinhos não foi suficiente, então a festa não acabou bem” (estrutura FCC) resulta em:

A) Se a festa acabou bem, então o número de docinhos foi suficiente  
B) O número de docinhos foi suficiente e a festa não acabou bem  
C) Se o número de docinhos foi suficiente, então a festa acabou bem  
D) Se a festa não acabou bem, então o número de docinhos não foi suficiente  
E) O número de docinhos não foi suficiente e a festa acabou bem  

**21.** Notas 7, 7, 7, 8, 9, 10. A média, a moda e a mediana são, nesta ordem:

A) 8; 7; 7,5  
B) 7,5; 7; 7,5  
C) 8; 7,5; 8  
D) 7; 8; 7,5  
E) 8; 7; 8  

**22.** De \(P\to Q\), \(Q\to R\) e P, a conclusão **válida** é:

A) ¬R  
B) R  
C) ¬P  
D) apenas Q, nunca R  
E) \(R\to P\) necessariamente  

**23.** Em tabela de frequências, o valor com maior frequência absoluta é a melhor pista para:

A) amplitude  
B) moda  
C) desvio padrão amostral exato sem cálculo  
D) variância populacional automática  
E) mediana quando n é par, sem ordenar  

**24.** Multiplicar todos os dados por 2:

A) dobra a média e dobra a amplitude  
B) mantém média e dobra só a moda  
C) zera o desvio padrão  
D) não altera nenhuma medida de posição  
E) troca mediana por moda  

**25.** Assinale a alternativa **correta** sobre o edital deste concurso (Gerais — estatística):

A) Cobra noções de tendência central e de dispersão, além de leitura de gráficos/tabelas  
B) Exige apenas regressão linear múltipla obrigatória  
C) Substitui estatística por prova oral de R  
D) Vedada qualquer menção a mediana  
E) Cobra somente ANOVA de dois fatores  

---

### Gabarito comentado (treino 1–15)

1. **B** — única linha F da condicional: V→F.  
2. **C** — contrappositiva.  
3. **B** — MANÉ: mantém P e nega Q.  
4. **D** — modus tollens.  
5. **B** — falácia: de Q concluir P.  
6. **B** — 5º de 9.  
7. **D** — dois centrais.  
8. **C** — 5 aparece 3 vezes.  
9. **C** — (16+5)/3 = 7.  
10. **E** — 15−4 = 11.  
11. **A** — transladação.  
12. **B** — moda visual.  
13. **B** — “somente se”.  
14. **A** — média 12; moda 12.  
15. **E** — recíproca **não** é equivalente.

### Gabarito (estilo concurso 16–25)

| Q | Gab | Nota |
|---|-----|------|
| 16 | **C** | contrappositiva (FCC TRT-20/2016) |
| 17 | **C** | forma \(\neg P\lor Q\) (FCC TRT/SP/2018) |
| 18 | **A** | Mo 20/21 < Md 22,5; Amp 16; média 25 (FCC TRT-23/2022) |
| 19 | **A** | Md=10; Amp=12 |
| 20 | **E** | negação: P ∧ ¬Q (estrutura FCC TRT-11/2017) |
| 21 | **A** | média 8; moda 7; mediana 7,5 |
| 22 | **B** | silogismo + ponens → R |
| 23 | **B** | moda |
| 24 | **A** | escala |
| 25 | **A** | texto do edital |

---

## 4. Gabarito consolidado

| Q | Resp | Q | Resp | Q | Resp | Q | Resp | Q | Resp |
|---|------|---|------|---|------|---|------|---|------|
| 1 | B | 2 | C | 3 | B | 4 | D | 5 | B |
| 6 | B | 7 | D | 8 | C | 9 | C | 10 | E |
| 11 | A | 12 | B | 13 | B | 14 | A | 15 | E |
| 16 | C | 17 | C | 18 | A | 19 | A | 20 | E |
| 21 | A | 22 | B | 23 | B | 24 | A | 25 | A |

---

## 5. Mini-treino da semana

Refaça **18** sem olhar: calcule média, mediana, moda e amplitude em 90 segundos. Depois, uma condicional: escreva contrappositiva e negação lado a lado.

---

*Arquivo 07 · Lógica + estatística básica · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 6 (manutenção Gerais).*

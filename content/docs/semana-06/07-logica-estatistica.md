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

## 3. Questões de concurso (16–25)

> Questões reais de provas (FCC). Gabarito só ao final.

**16.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Em uma repartição pública, um funcionário atende 18 pessoas em uma média de 90 minutos. Supondo que os atendimentos sejam semelhantes, espera-se que esse mesmo funcionário atenda 24 pessoas em

A) 2h00min  
B) 2h10min  
C) 2h30min  
D) 3h00min  
E) 2h20min  

**17.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
As idades dos amigos Ana, Bruno, Carla, Denise, Eduardo e Fábio são crescentes e nessa ordem, ou seja, Ana é mais nova do que Bruno que é mais novo do que Carla e assim por diante. A diferença entre as idades de Ana e Fábio é de 12 anos, entre Ana e Carla é de 4 anos, entre Carla e Eduardo é de 6 anos, entre Bruno e Denise é de 4 anos e entre Denise e Fábio é de 5 anos. Se Ana tem 15 anos, a idade média dos 6 amigos é

A) 22  
B) 19  
C) 23  
D) 20  
E) 21  

**18.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
O valor da expressão numérica 120 ÷ { 9 × [ − 9 + 17 ÷ ( − 2 + 5 ) ] } − [ 7 × ( − 3 ) − 16 ÷ ( − 2 ) + 2 ] 120 \div \left\{\sqrt{9} \times \left[-9 + 17 \div (-2 + 5)\right]\right\} - \left[7 \times (-3) - 16 \div (-2) + 2\right] 120 ÷ { 9 ​ × [ − 9 + 17 ÷ ( − 2 + 5 ) ] } − [ 7 × ( − 3 ) − 16 ÷ ( − 2 ) + 2 ] é

A) 0  
B) -2  
C) 2  
D) 1  
E) -1  

**19.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Bruno dividiu suas bolinhas de gude em grupos de 3 bolinhas e sobraram duas. Depois as dividiu em grupos de 5 e sobrou uma bolinha. O menor número de bolinhas que Bruno deve adquirir para poder dividir o total de bolinhas em grupos de 3 ou em grupos de 5 sem sobrar bolinhas é

A) 5  
B) 4  
C) 2  
D) 1  
E) 3  

**20.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
José ganhou na loteria R$ 15.000,00. Deu 1/3 de presente para sua esposa e 1/5 do que sobrou para sua filha. A quantia do prêmio que sobrou para José após presentear a esposa e a filha foi de

A) R$ 6.500,00  
B) R$ 7.000,00  
C) R$ 8.000,00  
D) R$ 3.500,00  
E) R$ 5.000,00  

**21.** (FCC – TRT-6/2025 – Analista Judiciário – TI)  
Uma família sempre consome a mesma quantidade diária de leite. Se essa família consome 5 litros de leite a cada 9 dias, então em três semanas a quantidade de leite que ela consome é, aproximadamente, de

A) 12,7 litros.  
B) 12,3 litros.  
C) 11,5 litros.  
D) 11,7 litros.  
E) 11,9 litros.  

**22.** (FCC – TRT-6/2025 – Analista Judiciário – TI)  
Em um setor de uma fábrica, há 5 empregados: o supervisor e quatro assistentes. O salário de um assistente é R$ 5.000,00 mensais e o do supervisor é de R$ 15.000,00 mensais. A diferença entre a média e a moda desses cinco salários mensais é

A) R$ 7.000,00  
B) R$ 2.000,00  
C) R$ 5.000,00  
D) R$ 3.000,00  
E) R$ 4.000,00  

**23.** (FCC – TRT-6/2025 – Analista Judiciário – TI)  
Em uma pesquisa sobre estilos musicais, obteve-se que 40% dos entrevistados gostam de samba, 75% gostam de rock e 53% gostam de música clássica. Além disso, 35% gostam de samba e rock, 30% gostam de rock e música clássica, 5% gostam apenas de samba e música clássica e 10% gostam dos três estilos musicais. O percentual de pessoas que NÃO gostam de nenhum dos três estilos é de

A) 1%  
B) 0%  
C) 4%  
D) 3%  
E) 2%  

**24.** (FCC – TRT-6/2025 – Analista Judiciário – TI)  
Considere um dado piramidal com 4 faces numeradas de 1 a 4. A face 2 é duas vezes mais provável do que a 1. A face 3 é 3 vezes mais provável que a face 2 e a face 4 é 4 vezes mais provável que a face 3. A probabilidade condicional de sair a face 1 ou a face 3 sabendo-se que saiu a face 1 ou a face 4 é de

A) 1/4  
B) 1/50  
C) 1/25  
D) 1/6  
E) 1/2  

**25.** (FCC – TRT-6/2025 – Analista Judiciário – TI)  
Pedro tinha 18 figurinhas no sábado. No domingo e na segunda-feira, ele comprou 10 figurinhas em cada dia. Comprou algumas figurinhas na terça-feira e na quarta-feira comprou o dobro de figurinhas do que havia comprado na terça-feira. Na quinta-feira, não comprou figurinhas, ficando com um total de 74 figurinhas. Pedro comprou na quarta-feira

A) 24 figurinhas.  
B) 22 figurinhas.  
C) 28 figurinhas.  
D) 36 figurinhas.  
E) 12 figurinhas.  

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **A** | FCC TRT-1/2025 Q11 — Regra de três simples |
| 17 | **E** | FCC TRT-1/2025 Q12 — Medidas de tendência central |
| 18 | **E** | FCC TRT-1/2025 Q13 — Expressões numéricas |
| 19 | **B** | FCC TRT-1/2025 Q14 — Múltiplos e divisores de números naturais |
| 20 | **C** | FCC TRT-1/2025 Q15 — Frações e operações com frações |
| 21 | **D** | FCC TRT-6/2025 Q11 — Razão e proporção |
| 22 | **B** | FCC TRT-6/2025 Q12 — Estatística |
| 23 | **E** | FCC TRT-6/2025 Q13 — Conjuntos |
| 24 | **C** | FCC TRT-6/2025 Q14 — Probabilidade |
| 25 | **A** | FCC TRT-6/2025 Q15 — Raciocínio lógico-matemático |

**Notas rápidas:** 16 A; 17 E; 18 E; 19 B; 20 C; 21 D; 22 B; 23 E; 24 C; 25 A.

+++
title = '08 — Matemática: porcentagem'
date = '2026-10-27'
draft = false
weight = 8
tags = ['semana-02', 'matematica']
categories = ['estudo']
+++
# Matemática — porcentagem, regra de três, acréscimos/descontos · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Bloco:** Conhecimentos Gerais — Matemática e Raciocínio lógico  
**Escopo deste arquivo:** **porcentagem**; **razões e proporções**; **regra de três simples**; **acréscimos e descontos** (simples e sucessivos); problemas mistos típicos de prova.  
**Fora de escopo aqui:** regra de três composta em profundidade; estatística (média/moda/mediana/dispersão — outro bloco); lógica proposicional / relações artificiais; português.

**Edital (trecho cobrável):** números e grandezas proporcionais; razões e proporções; regra de três; porcentagem; problemas com regra de três simples, cálculos de porcentagem, acréscimos e descontos.

---

## 1. Material

### 1.1 Porcentagem — o que a FCC cobra

**Por cento** = “por cem”. O símbolo **%** é uma fração de denominador 100:

\[
p\% = \frac{p}{100}
\]

Três formas equivalentes (troque livremente na prova):

| Forma | Exemplo (25%) |
|-------|----------------|
| Percentual | 25% |
| Fração | \(\frac{25}{100} = \frac{1}{4}\) |
| Decimal | 0,25 |

**Operação-mãe** (quase todo problema reduz a isto):

\[
\text{valor parcial} = \text{total} \times \frac{p}{100}
\]

ou, na linguagem da prova: **“p% de N”** = \(N \times \frac{p}{100}\).

Flash:

> Achar **p% de N** → multiplique N por \(p/100\).  
> Achar **quanto %** A é de B → \(\frac{A}{B}\times 100\).  
> Achar o **todo** sabendo que p% vale V → \(N = V \div (p/100) = V \times \frac{100}{p}\).

### 1.2 Conversões rápidas (decorar as “âncoras”)

| % | Fração | Decimal |
|---|--------|---------|
| 10% | 1/10 | 0,10 |
| 12,5% | 1/8 | 0,125 |
| 20% | 1/5 | 0,20 |
| 25% | 1/4 | 0,25 |
| 33⅓% | 1/3 | ≈ 0,333… |
| 40% | 2/5 | 0,40 |
| 50% | 1/2 | 0,50 |
| 75% | 3/4 | 0,75 |
| 100% | 1 | 1 |

Atalho mental: **10%** = deslocar a vírgula uma casa à esquerda; **1%** = duas casas; **5%** = metade de 10%.

### 1.3 Três problemas-base (sempre os mesmos)

**Tipo A — “quanto é p% de N?”**  
Ex.: 15% de 800 = \(800 \times 0,15 = 120\).

**Tipo B — “A é quanto % de B?”**  
Ex.: 45 é quanto % de 180? \(\frac{45}{180}\times 100 = 25\%\).

**Tipo C — “p% de um número é V; qual o número?”**  
Ex.: 20% de N = 70 → \(N = 70 \times \frac{100}{20} = 350\).

Armadilha clássica: confundir **“A é p% de B”** com **“A é p% a mais que B”** (este último é acréscimo — §1.6).

### 1.4 Razão e proporção (base da regra de três)

**Razão** \(a:b\) (ou \(\frac{a}{b}\)) compara duas grandezas da mesma natureza (ou convertidas à mesma unidade).

**Proporção:** igualdade de razões:

\[
\frac{a}{b} = \frac{c}{d} \quad\Leftrightarrow\quad a\cdot d = b\cdot c
\]

(propriedade fundamental — “produto dos meios = produto dos extremos”).

Em prova: monte a proporção **antes** de cruzar; confira se as grandezas estão **na mesma ordem** e **na mesma unidade**.

### 1.5 Regra de três simples

Usada quando há **duas grandezas** ligadas por proporção e **três valores** conhecidos; acha-se o quarto.

#### 1.5.1 Diretamente proporcional

Se uma **aumenta** e a outra **aumenta** na mesma razão (ex.: quantidade × preço total a preço unitário fixo; horas × produção a ritmo constante):

\[
\frac{A_1}{A_2} = \frac{B_1}{B_2}
\]

Esquema típico:

| Grandeza 1 | Grandeza 2 |
|------------|------------|
| \(a\) | \(b\) |
| \(c\) | \(x\) |

\[
\frac{a}{c} = \frac{b}{x} \quad\Rightarrow\quad x = \frac{b\cdot c}{a}
\]

#### 1.5.2 Inversamente proporcional

Se uma **aumenta** e a outra **diminui** (ex.: mais operários → menos dias para a mesma obra, ritmo individual igual; velocidade ↑ → tempo ↓ no mesmo trajeto):

\[
A_1\cdot B_1 = A_2\cdot B_2
\]

ou, na montagem:

\[
\frac{a}{c} = \frac{x}{b} \quad\Rightarrow\quad x = \frac{a\cdot b}{c}
\]

**Teste de prova (10 segundos):** “Se **dobrar** a primeira, a segunda **dobra** ou **cai pela metade**?”  
- Dobra → **direta**.  
- Cai pela metade → **inversa**.

#### 1.5.3 Porcentagem via regra de três

“p% de N” é regra de três direta:

| Parte | Total |
|-------|-------|
| \(p\) | 100 |
| \(x\) | \(N\) |

\[
\frac{p}{100} = \frac{x}{N} \quad\Rightarrow\quad x = N\cdot\frac{p}{100}
\]

### 1.6 Acréscimos e descontos (simples)

Fator multiplicativo (ouro FCC — evita somar/subtrair “na mão” e errar o base):

| Operação | Fator | Resultado |
|----------|-------|-----------|
| Acréscimo de \(p\%\) | \(1 + \frac{p}{100}\) | \(N' = N\left(1+\frac{p}{100}\right)\) |
| Desconto de \(p\%\) | \(1 - \frac{p}{100}\) | \(N' = N\left(1-\frac{p}{100}\right)\) |

Exemplos:

- Aumentar 20%: multiplique por **1,20**.  
- Descontar 15%: multiplique por **0,85**.  
- Aumentar 8%: multiplique por **1,08**.  
- Descontar 100%: fator 0 → zera (raro, mas lógico).

**Variação percentual** (de \(N\) para \(N'\)):

\[
\Delta\% = \frac{N' - N}{N}\times 100
\]

- \(N' > N\) → acréscimo (positivo).  
- \(N' < N\) → redução/desconto (negativo em sinal; em enunciado, “diminuiu X%”).

### 1.7 Acréscimos / descontos sucessivos (armadilha nº 1 da FCC)

**Não some as taxas** quando elas incidem **em sequência** sobre bases diferentes.

Dois acréscimos sucessivos de \(p\%\) e \(q\%\):

\[
N'' = N\left(1+\frac{p}{100}\right)\left(1+\frac{q}{100}\right)
\]

Dois descontos sucessivos:

\[
N'' = N\left(1-\frac{p}{100}\right)\left(1-\frac{q}{100}\right)
\]

Acréscimo seguido de desconto (ou vice-versa): multiplique os fatores na ordem do enunciado.

**Equivalente único** (só se a banca pedir “um único percentual equivalente”):

- Dois acréscimos \(p\) e \(q\): fator \((1+\frac{p}{100})(1+\frac{q}{100}) - 1\), depois ×100.  
  Ex.: +10% e +10% → \(1,1\times 1,1 = 1,21\) → **+21%**, **não** +20%.  
- Dois descontos 10% e 10% → \(0,9\times 0,9 = 0,81\) → **−19%**, **não** −20%.

Flash:

> Sucessivos → **multiplique fatores**.  
> Somar % só quando o enunciado deixa claro que incidem **sobre a mesma base** (e mesmo assim confira).

### 1.8 Aumentar e depois voltar — “não volta ao original”

Se o preço sobe \(p\%\) e depois cai \(p\%\) (mesma taxa):

\[
N\left(1+\frac{p}{100}\right)\left(1-\frac{p}{100}\right) = N\left(1-\left(\frac{p}{100}\right)^2\right) < N
\]

Sempre fica **abaixo** do original. Ex.: +20% e −20% → fator \(1,2\times 0,8 = 0,96\) → perda líquida de **4%**.

Inverso (cai \(p\%\) e sobe \(p\%\)): mesmo fator \(1-(p/100)^2\) — também **não** recupera o original.

### 1.9 “A mais / a menos” vs “de”

| Enunciado | Significado | Fórmula |
|-----------|-------------|---------|
| A é **p% de** B | \(A = B\cdot p/100\) | fração de B |
| A é **p% a mais** que B | \(A = B(1+p/100)\) | B acrescido |
| A é **p% a menos** que B | \(A = B(1-p/100)\) | B reduzido |
| A é **p% maior** que B | igual a “a mais” | idem |
| Quanto % A é **maior** que B | \(\frac{A-B}{B}\times 100\) | base = B |
| Quanto % A é **menor** que B | \(\frac{B-A}{B}\times 100\) | base = B |

**Base errada = questão errada.** Sempre pergunte: “% **em relação a quê**?”

Caso clássico: se A é 25% maior que B, então B é **20% menor** que A (não 25%):

\[
A = 1,25\,B \Rightarrow B = \frac{A}{1,25} = 0,8\,A \Rightarrow 20\%\text{ a menos}.
\]

### 1.10 Misturas úteis na prova (ponte %)

**Parte / todo / resto**

- Se \(p\%\) têm certa propriedade, \((100-p)\%\) não têm (universo fechado de 100%).  
- “Dos aprovados, 40%…” → a base mudou: agora o total é o subconjunto “aprovados”, não o grupo inicial.

**Impostos / contribuições / “sobre o preço”**

- “Acrescido de 12% de imposto” → ×1,12.  
- “Preço com desconto de 8% sobre a tabela” → ×0,92.  
- “Desconto de 10% e depois acréscimo de 5% de frete” → ×0,90×1,05 (ordem importa se o frete incide sobre o valor já descontado — leia o enunciado).

**Variação relativa entre dois percentuais**

Não diga que “passou de 20% para 30% = +10%” no sentido relativo sem cuidado: a **diferença em pontos percentuais** é 10 p.p.; o **aumento relativo** da taxa é \(\frac{30-20}{20}\times 100 = 50\%\). A FCC gosta de enunciar com clareza, mas o distrator “10%” vs “50%” aparece.

### 1.11 Método operacional (checklist de 40 segundos)

1. **O que pedem?** Valor absoluto, percentual, total, fator único, diferença.  
2. **Qual a base?** Total inicial, valor após o 1º desconto, “em relação a B”…  
3. **Direta ou inversa?** (se regra de três).  
4. **Simples ou sucessivo?** → um fator ou produto de fatores.  
5. **Unidades iguais?** (meses/dias, R$/centavos, kg/g).  
6. **Teste com número fácil** (100 ou 1000) se o enunciado for abstrato.

### 1.12 Armadilhas FCC (lista de ouro)

1. Somar percentuais sucessivos (10%+10% ≠ 20% efetivo).  
2. Achar que +p% e −p% restauram o valor.  
3. Trocar “p% de” por “p% a mais”.  
4. Usar a base errada na variação \(\frac{\Delta}{\text{base}}\).  
5. Tratar grandezas inversas como diretas (mais gente → mais dias).  
6. Esquecer de alinhar unidades antes da proporção.  
7. Calcular “quanto % A é maior que B” dividindo por A em vez de B.  
8. Confundir **pontos percentuais** com **variação percentual relativa**.  
9. Aplicar desconto sobre valor já com imposto quando o enunciado diz “sobre o preço de tabela”.  
10. Em “restou 70%”, achar que o desconto foi 70% (foi **30%**).

### 1.13 Mini-síntese (cartão mental)

> **%** = fração /100 · **p% de N** = \(N\times p/100\).  
> **Regra de três:** direta se “dobra-dobra”; inversa se “dobra-metade”.  
> **Acréscimo/desconto:** fatores \(1\pm p/100\); **sucessivos → multiplique**.  
> **+p% e −p% ≠ original**; base da % é o que o enunciado fixar.

---

## 2. Questões de treino (1–15)

**1.** Quanto é 12% de 250?

A) 25  
B) 30  
C) 32  
D) 35  
E) 40  

**2.** O número 36 corresponde a 15% de qual total?

A) 180  
B) 200  
C) 220  
D) 240  
E) 260  

**3.** 48 é quanto por cento de 160?

A) 20%  
B) 25%  
C) 30%  
D) 32%  
E) 35%  

**4.** Um produto de R$ 80,00 sofre acréscimo de 25%. O novo preço é:

A) R$ 90,00  
B) R$ 95,00  
C) R$ 100,00  
D) R$ 105,00  
E) R$ 110,00  

**5.** Um preço de R$ 200,00 recebe desconto de 15%. O valor a pagar é:

A) R$ 160,00  
B) R$ 165,00  
C) R$ 170,00  
D) R$ 175,00  
E) R$ 180,00  

**6.** Em uma regra de três simples **direta**, se 4 canetas custam R$ 12,00, então 7 canetas (mesmo preço unitário) custam:

A) R$ 18,00  
B) R$ 20,00  
C) R$ 21,00  
D) R$ 24,00  
E) R$ 28,00  

**7.** Uma obra é feita por 6 pedreiros em 10 dias (mesmo ritmo). Mantidas as demais condições, 4 pedreiros farão a mesma obra em:

A) 12 dias  
B) 14 dias  
C) 15 dias  
D) 16 dias  
E) 18 dias  

**8.** Um valor aumenta 10% e, em seguida, mais 10% sobre o novo valor. O acréscimo percentual total equivalente, em relação ao original, é:

A) 10%  
B) 20%  
C) 21%  
D) 22%  
E) 25%  

**9.** Um preço sofre dois descontos sucessivos de 10%. Em relação ao preço inicial, a redução efetiva é:

A) 10%  
B) 19%  
C) 20%  
D) 21%  
E) 25%  

**10.** Um artigo custa R$ 150,00. Após aumento de 20% e desconto posterior de 20% sobre o preço já aumentado, o preço final é:

A) R$ 144,00  
B) R$ 150,00  
C) R$ 156,00  
D) R$ 160,00  
E) R$ 180,00  

**11.** Se A é 25% maior que B, então B é quanto por cento menor que A?

A) 15%  
B) 20%  
C) 25%  
D) 30%  
E) 33⅓%  

**12.** De um grupo de 200 pessoas, 40% são mulheres. Dentre as mulheres, 25% são concursadas. Quantas mulheres concursadas há no grupo?

A) 15  
B) 20  
C) 25  
D) 30  
E) 40  

**13.** Um capital de R$ 500,00 rende um acréscimo de 8%. O valor acrescido (só o aumento) é:

A) R$ 30,00  
B) R$ 35,00  
C) R$ 40,00  
D) R$ 45,00  
E) R$ 50,00  

**14.** Se 30% de um número é 90, então 70% desse mesmo número é:

A) 180  
B) 200  
C) 210  
D) 240  
E) 270  

**15.** Um motorista percorre 240 km em 4 horas a velocidade constante. Mantida a velocidade, em 7 horas percorrerá:

A) 360 km  
B) 380 km  
C) 400 km  
D) 420 km  
E) 480 km  

---

## 3. Questões estilo concurso FCC (16–25)

**16.** Em um órgão público, 35% das 1.200 solicitações de um mês foram respondidas em até 5 dias. O número de solicitações respondidas nesse prazo foi:

A) 360  
B) 380  
C) 400  
D) 420  
E) 450  

**17.** O preço de um equipamento de informática, cotado em R$ 3.500,00, sofreu reajuste de 12%. O preço reajustado é:

A) R$ 3.780,00  
B) R$ 3.850,00  
C) R$ 3.920,00  
D) R$ 3.960,00  
E) R$ 4.000,00  

**18.** Uma loja oferece desconto de 18% sobre o preço de tabela de R$ 250,00. O valor pago pelo cliente, em reais, é:

A) 200  
B) 205  
C) 210  
D) 215  
E) 220  

**19.** Sabe-se que 4 impressoras iguais, trabalhando no mesmo ritmo, imprimem certo lote em 6 horas. Mantidas as condições, o tempo necessário para que 3 dessas impressoras concluam o mesmo lote é:

A) 6 h  
B) 7 h  
C) 8 h  
D) 9 h  
E) 10 h  

**20.** Um servidor recebe um aumento de 5% e, no mês seguinte, outro aumento de 4% sobre o novo vencimento. O percentual total de aumento, em relação ao vencimento original, é mais próximo de:

A) 9,00%  
B) 9,10%  
C) 9,20%  
D) 9,30%  
E) 9,50%  

**21.** Um produto que custava R$ 80,00 passou a custar R$ 68,00. A redução percentual sobre o preço antigo foi de:

A) 12%  
B) 15%  
C) 16%  
D) 18%  
E) 20%  

**22.** Em um pregão, o lance inicial de R$ 10.000,00 sofreu desconto de 10% e, em seguida, novo desconto de 5% sobre o valor já reduzido. O lance final foi:

A) R$ 8.450,00  
B) R$ 8.500,00  
C) R$ 8.550,00  
D) R$ 8.600,00  
E) R$ 8.650,00  

**23.** Se 45% dos processos de um setor correspondem a 135 processos, o total de processos do setor é:

A) 250  
B) 280  
C) 300  
D) 320  
E) 350  

**24.** Um técnico afirma: “Após aumentar o preço em 25% e depois conceder desconto de 20% sobre o preço aumentado, o valor voltou exatamente ao original.” Essa afirmação é:

A) verdadeira, porque \(1,25\times 0,80 = 1\) (os fatores se cancelam neste par específico de taxas)  
B) verdadeira, porque 25 − 20 = 5 e todo residual percentual se anula por subtração  
C) verdadeira, porque qualquer acréscimo seguido de desconto de taxa menor restaura o original  
D) falsa, porque acréscimo e desconto nunca devolvem o preço inicial  
E) falsa, porque \(1,25\times 0,80 = 0,95\) e há perda líquida de 5%  

**25.** Relativamente a porcentagens e proporções cobráveis no bloco de Matemática do edital (código 14), assinale a opção correta:

A) Dois descontos sucessivos de 10% equivalem sempre a um único desconto de 20% sobre a base inicial  
B) Em grandezas inversamente proporcionais, dobrar uma das grandezas dobra a outra  
C) O fator de um acréscimo de \(p\%\) é \(1+\frac{p}{100}\); o de um desconto de \(p\%\) é \(1-\frac{p}{100}\)  
D) “A é 30% a mais que B” significa o mesmo que “A é 30% de B”  
E) A regra de três simples só se aplica a grandezas inversamente proporcionais  

---

### Gabarito comentado (treino 1–15)

1. **B** — \(250\times 0,12 = 30\).  
2. **D** — \(36\div 0,15 = 240\).  
3. **C** — \(\frac{48}{160}\times 100 = 30\%\).  
4. **C** — \(80\times 1,25 = 100\).  
5. **C** — \(200\times 0,85 = 170\).  
6. **C** — direta: \(x = 12\times 7/4 = 21\).  
7. **C** — inversa: \(6\times 10 = 4\times x \Rightarrow x = 15\).  
8. **C** — \(1,1\times 1,1 = 1,21\) → +21%.  
9. **B** — \(0,9\times 0,9 = 0,81\) → −19%.  
10. **A** — \(150\times 1,2\times 0,8 = 144\).  
11. **B** — \(B = A/1,25 = 0,8A\) → 20% menor.  
12. **B** — \(200\times 0,40\times 0,25 = 20\).  
13. **C** — \(500\times 0,08 = 40\).  
14. **C** — \(N = 90/0,30 = 300\); \(70\% = 210\).  
15. **D** — direta: \(240\times 7/4 = 420\).

### Gabarito comentado (concurso 16–25)

16. **D** — \(1200\times 0,35 = 420\).  
17. **C** — \(3500\times 1,12 = 3920\).  
18. **B** — \(250\times 0,82 = 205\).  
19. **C** — inversa: \(4\times 6 = 3\times x \Rightarrow x = 8\).  
20. **C** — \(1,05\times 1,04 = 1,092\) → **9,20%**.  
21. **B** — \(\frac{80-68}{80}\times 100 = 15\%\).  
22. **C** — \(10000\times 0,90\times 0,95 = 8550\).  
23. **C** — \(135/0,45 = 300\).  
24. **A** — \(1,25\times 0,80 = 1\): neste par, volta ao original. B/C generalizam errado; D/E negam o cálculo.  
25. **C** — Definição operacional correta dos fatores; A/B/D/E são armadilhas clássicas.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar:

1. Pediram **valor**, **%** ou **total**?  
2. A base do % é o **inicial** ou o **já modificado**?  
3. Sucessivos? → **produto de fatores**, não soma de taxas.  
4. Regra de três: **direta** ou **inversa**?  
5. “% de” ≠ “% a mais / a menos”.

---

*Arquivo 08 · Matemática (porcentagem / regra de três / acréscimos-descontos) · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 2 — manutenção Gerais.*

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

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT-15 2018 | Analista Judiciário, Área Administrativa | Questão 18 de 60)

Quando se diz que um imposto com alíquota de 20% incide sobre um produto cujo preço inicial é R$ 100,00, é usual concluir que, com o acréscimo desse imposto, o preço final do produto seria de R$ 120,00. Isso é chamado de cálculo “por fora”. Porém, há impostos em que se utiliza o chamado “cálculo por dentro”. Nesses casos, se uma alíquota de 20% incide sobre um produto cujo preço inicial é R$ 100,00, então o preço final é de R$ 125,00, pois 20% do valor final deve ser relativo ao imposto. Com um imposto de alíquota 18% sobre um produto cujo valor inicial é de R$ 1.640,00, a diferença entre os preços finais calculados por dentro e por fora é de

A) R$ 128,40.

B) R$ 32,40.

C) R$ 360,00.

D) R$ 64,80.

E) R$ 640,00.


**17.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 12 de 60)

Uma empresa distribui o valor que entra mensalmente da seguinte maneira: metade é destinada a gastos fixos e, para os gastos variáveis, 25% do valor é destinado aos investimentos e 15% para o pagamento do pró-labore. O restante é depositado em um fundo emergencial. O dono da empresa fica com 90% do pró-labore. Se, no mês de novembro, o valor depositado no fundo emergencial foi de R$ 32.000,00, então o dono da empresa recebeu nesse mês

A) R$ 36.000,00

B) R$ 43.200,00

C) R$ 48.000,00

D) R$ 16.000,00

E) R$ 72.000,00


**18.** (FCC | TRT-15 2018 | Analista Judiciário, Área Administrativa | Questão 16 de 60)

Lúcio, Maurício e Júlio são sócios em uma empresa, sendo as partes de cada um: 15%, 25% e 60%, não necessariamente nessa ordem. Cada um dos sócios atua na empresa, mas em diferentes setores: Administração, Vendas e Informática. Além disso, sabe-se que: − o que atua na Administração não é o que tem 15% da empresa; − o que atua em Informática é amigo de infância do que tem 60%; − Júlio não atua em vendas; − quem atua em vendas é o sócio majoritário; − Lúcio tem 25% da empresa. Dessa forma, se a empresa está avaliada em R$ 120.000,00, então o valor da parte de Júlio é

A) R$ 30.000,00.

B) R$ 18.000,00.

C) R$ 72.000,00.

D) R$ 36.000,00

E) R$ 12.000,00.


**19.** (FCC | TRT9 2022 | Técnico Judiciário, Área Administrativa | Questão 16 de 60)

João e Gilberto são muito amigos. Quando Gilberto pediu um empréstimo sem juros a João, este concordou e deu-lhe 25% do que tinha na poupança. Gilberto já pagou 80% do valor emprestado e ainda faltam R$ 100,00. Antes de emprestar o dinheiro a Gilberto, o saldo da poupança de João, em reais, era

A) 2.000,00.

B) 500,00.

C) 3.000,00.

D) 3.500,00.

E) 5.000,00.


**20.** (FCC | TRT-15 2018 | Analista Judiciário, Área Apoio Especializado, Especialidade Arquitetura | Questão 18 de 60)

O valor do equipamento A, adquirido por R$ 10.000,00, se deprecia 5% após o primeiro ano de uso e 6% após o segundo ano de uso. O valor do equipamento B, adquirido no mesmo dia em que A foi adquirido, se deprecia 7% após o primeiro ano de uso e 9% após o segundo ano de uso. Sabe-se que, após 2 anos da aquisição e uso dos dois equipamentos, seus valores, já depreciados, são iguais. Uma expressão numérica que, se resolvida corretamente em uma calculadora, fornecerá o valor de aquisição do equipamento B, em reais, é

A) (950 × 940) ÷ (0,93 × 0,91).

B) (105 × 106) ÷ (1,07 × 1,09).

C) (95 × 94) ÷ (0,93 × 0,91).

D) (9500 × 9400) ÷ (0,93 × 0,91).

E) (1050 × 1060) ÷ (1,07 × 1,09).


**21.** (FCC | TRT-15 2018 | Analista Judiciário, Área Apoio Especializado, Especialidade Arquitetura | Questão 14 de 60)

Um Tribunal Regional do Trabalho celebrou acordos conciliatórios no valor de R$ 1,210 milhão. Em 55 audiências independentes umas das outras, o percentual de audiências com acordo foi de 40%. Considerando apenas as audiências em que houve acordo, o valor médio dos acordos por audiência foi de

A) R$ 5.500,00.

B) R$ 55.000,00.

C) R$ 88.000,00.

D) R$ 8.800,00.

E) R$ 36.600,00.


**22.** (FCC | TRT6 2025 | Técnico Judiciário, Área Administrativa | Questão 11 de 60)

Um fabricante de sabonetes vende cada caixa por R$ 254,40. Uma caixa contém uma dúzia de embalagens e cada embalagem contém 4 sabonetes. Se um comprador solicita mais de 100 caixas, o fabricante faz um desconto de 5% no preço total. O fabricante recebeu um pedido de 6000 sabonetes. O valor a ser pago pelo comprador é

A) R$ 28.620,00.

B) R$ 32.500,00.

C) R$ 31.800,00.

D) R$ 30.210,00.

E) R$ 33.390,00.


**23.** (FCC | TRT9 2022 | Técnico Judiciário, Área Administrativa | Questão 17 de 60)

Aldo, Bernardo e Cristiano são corretores de imóveis e combinaram de dividir a comissão de uma venda da seguinte maneira: metade da comissão cabe a Aldo e a outra metade será dividida em partes iguais entre Bernardo e Cristiano. A comissão total da venda será de 4% sobre o valor do imóvel, que é de R$ 810.000,00. A comissão de Bernardo será, em reais, de

A) 12.000,00.

B) 8.100,00.

C) 3.000,00.

D) 7.500,00.

E) 1.000,00.


**24.** (FCC | TRF3 2024 | Analista Judiciário, Área Judiciária | Questão 24 de 60) *[aproximação temática]*

*Juros/percentual correlato.*

Um banco concedeu à empresa AJM Ltda. um empréstimo de R$ 10.000,00 a juros de 2% ao mês, para utilização como capital de giro. No contrato, formalizado por instrumento particular, a empresa se comprometeu a devolver integralmente o capital emprestado acrescido dos juros. Nesse mesmo instrumento contratual, Beatriz figurou como fiadora da empresa. Contudo, a empresa deixou de honrar a dívida. Nesse caso, de acordo com o Código Civil a pretensão do banco de cobrar a dívida prescreve em

A) 5 anos, e a interrupção da prescrição contra a empresa não prejudica Beatriz.

B) 10 anos, e a interrupção da prescrição contra a empresa não prejudica Beatriz.

C) 3 anos, e a interrupção da prescrição contra a empresa prejudica Beatriz.

D) 3 anos, e a interrupção da prescrição contra a empresa não prejudica Beatriz.

E) 5 anos, e a interrupção da prescrição contra a empresa prejudica Beatriz.


**25.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 11 de 60) *[aproximação temática]*

*Proporção (vizinha de porcentagem).*

O volume de suco contido em uma garrafa cheia corresponde exatamente ao volume de seis copos idênticos cheios. Após um lanche coletivo em uma escola, sobraram 15 garrafas de suco vazias, 3 garrafas contendo exatamente o volume para encher 2 copos em cada uma, e 2 garrafas contendo exatamente o volume para encher um copo em cada uma. Em relação ao volume das garrafas de suco que foram abertas, a proporção de suco consumida foi de

A) 9/11.

B) 11/12.

C) 7/8.

D) 14/15.

E) 8/9.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **D** | FCC | TRT-15 2018 | Analista Judiciário, Área Administrativa | Questão 18 de 60 |
| 17 | **B** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 12 de 60 |
| 18 | **B** | FCC | TRT-15 2018 | Analista Judiciário, Área Administrativa | Questão 16 de 60 |
| 19 | **A** | FCC | TRT9 2022 | Técnico Judiciário, Área Administrativa | Questão 16 de 60 |
| 20 | **C** | FCC | TRT-15 2018 | Analista Judiciário, Área Apoio Especializado, Especialidade Arquitetura | Questão 18 de 60 |
| 21 | **B** | FCC | TRT-15 2018 | Analista Judiciário, Área Apoio Especializado, Especialidade Arquitetura | Questão 14 de 60 |
| 22 | **D** | FCC | TRT6 2025 | Técnico Judiciário, Área Administrativa | Questão 11 de 60 |
| 23 | **B** | FCC | TRT9 2022 | Técnico Judiciário, Área Administrativa | Questão 17 de 60 |
| 24 | **E** \* | FCC | TRF3 2024 | Analista Judiciário, Área Judiciária | Questão 24 de 60 |
| 25 | **D** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 11 de 60 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt-15-2018-analista-judiciario-area-administrativa-2018-q18
- Q17: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q12
- Q18: https://www.quizinho.com.br/questao/fcc-trt-15-2018-analista-judiciario-area-administrativa-2018-q16
- Q19: https://www.quizinho.com.br/questao/fcc-trt9-2022-tecnico-judiciario-area-administrativa-2022-q16
- Q20: https://www.quizinho.com.br/questao/fcc-trt-15-2018-analista-judiciario-area-apoio-especializado-especialidade-arquitetura-2018-q18
- Q21: https://www.quizinho.com.br/questao/fcc-trt-15-2018-analista-judiciario-area-apoio-especializado-especialidade-arquitetura-2018-q14
- Q22: https://www.quizinho.com.br/questao/fcc-trt6-2025-tecnico-judiciario-area-administrativa-2025-q11
- Q23: https://www.quizinho.com.br/questao/fcc-trt9-2022-tecnico-judiciario-area-administrativa-2022-q17
- Q24: https://www.quizinho.com.br/questao/fcc-trf3-2024-analista-judiciario-area-judiciaria-2024-q24
- Q25: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q11

*Arquivo 08 · Matemática (porcentagem / regra de três / acréscimos-descontos) · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 2 — manutenção Gerais.*

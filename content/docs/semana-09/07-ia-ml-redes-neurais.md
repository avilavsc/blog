+++
title = '07 — Aprendizado de máquina + Redes neurais'
date = '2026-10-30'
draft = false
weight = 7
tags = ['semana-09', 'ia']
categories = ['estudo']
+++
# Aprendizado de máquina + Redes neurais · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 07 — aprendizado de máquina e redes neurais  
**Escopo deste arquivo:** conceitos cobráveis de **ML** (supervisionado × não supervisionado × reforço; overfitting/underfitting; treino/validação/teste; métricas básicas de classificação e regressão) + **redes neurais** (perceptron, camadas, pesos, ativação, backpropagation, deep learning em noção). Alinhado ao Anexo II (*aprendizado de máquina; redes neurais*).  
**Fora de escopo aqui:** IA generativa, LLMs, tokens, prompt, hallucination, RAG (arquivo `08`); low-code/assistentes de IA no ciclo de software (semana 4 · `06`); MLOps profundo além do necessário para distinguir fases; frameworks comerciais linha a linha.

---

## 1. Material

### 1.1 Por que cai no edital

O bloco de IA do edital TRT4/FCC separa duas frentes: (1) **conceitos de ML e redes neurais** (este arquivo); (2) **IA generativa e LLMs** (arquivo 08). Flash: a banca cobra **distinções**, não implementação em PyTorch.

```
ML clássico     → aprender padrão a partir de dados
Rede neural     → modelo com neurônios artificiais / camadas
Deep learning   → redes com várias camadas ocultas
Generativa/LLM  → outro arquivo (08)
```

### 1.2 Aprendizado de máquina — mapa mental

**Aprendizado de máquina (ML)** = algoritmos que melhoram desempenho em uma tarefa a partir de **dados**, sem programar regra explícita para cada caso.

| Paradigma | O que o modelo recebe | Objetivo típico | Exemplos cobráveis |
|-----------|----------------------|-----------------|-------------------|
| **Supervisionado** | Pares (entrada, **rótulo**/alvo) | Predizer rótulo ou valor | Classificação, regressão |
| **Não supervisionado** | Só entradas (sem rótulo) | Descobrir estrutura | Clustering, associação, redução de dimensionalidade |
| **Por reforço** | Agente + recompensa/punição | Maximizar recompensa ao longo do tempo | Jogos, controle (menos cobrado em Analista TI FCC, mas existe) |

Síntese de prova:

```
Tem rótulo “certo” no treino? → Supervisionado
Sem rótulo, busca padrão/grupo? → Não supervisionado
Aprende por tentativa + recompensa? → Reforço
```

### 1.3 Supervisionado: classificação × regressão

| | **Classificação** | **Regressão** |
|---|-------------------|---------------|
| Saída | Classe / categoria (discreta) | Valor contínuo |
| Exemplo judiciário | Processo “procedente/improcedente”; risco alto/médio/baixo | Estimar prazo médio, valor de verba, volume de demanda |
| Métricas típicas | Acurácia, precisão, recall, F1, ROC/AUC | MAE, MSE, RMSE, R² |

**Classificação binária** = duas classes (ex.: fraude/não fraude).  
**Multiclasse** = várias classes mutuamente exclusivas.  
**Multirrótulo** = várias classes podem ser verdadeiras ao mesmo tempo (menos cobrado).

Algoritmos clássicos (reconhecer nome × papel):

- **Regressão logística** — classificação (apesar do nome); saída em probabilidade [0,1]; clássica em binária.
- **Árvore / Random Forest / Gradient Boosting** — tabulares, interpretabilidade relativa.
- **SVM** — classificação (ou regressão na variante SVR — FCC já cobrou a pegadinha “SVM for regression”).
- **k-NN** — classifica pelo vizinho mais próximo.
- **Naive Bayes** — probabilístico, texto/spam clássico.

### 1.4 Não supervisionado (caderno FCC)

| Técnica | Ideia | Pegadinha |
|---------|-------|-----------|
| **Clustering** (k-means, hierárquico) | Agrupar por similaridade | Não prevê rótulo conhecido no treino |
| **Regras de associação** (Apriori etc.) | “Quem compra X também compra Y” | Modelo **descritivo**, sem classe explícita |
| **PCA / redução de dimensionalidade** | Comprimir features | Pré-processamento, não “detector de fraude” sozinho |
| **Anomaly detection** (às vezes híbrido) | Pontos atípicos | Pode ser supervisionado se houver rótulo de anomalia |

**Regras de associação** (FCC TRT-4/2022 cobrou):

- representam padrões em **transações**;
- são tipicamente **descritivas** (não preditivas de uma classe);
- **não** exigem definição explícita de classe;
- dados em tabela costumam ser convertidos para formato de **transação**.

### 1.5 Conjuntos: treino, validação, teste

| Conjunto | Uso |
|----------|-----|
| **Treino** | Ajustar parâmetros do modelo |
| **Validação** | Escolher hiperparâmetros / early stopping (sem “vazar” o teste) |
| **Teste** | Estimar desempenho final em dados **não vistos** |

Divisão típica didática: 70/30 ou 60/20/20 — o número exato raramente é o ponto; o ponto é **não avaliar generalização só no treino**.

**Validação cruzada (k-fold):** particiona o treino em *k* folds; roda *k* vezes; reduz dependência de um único split.

### 1.6 Overfitting × underfitting (ouro FCC)

| | **Overfitting (sobreajuste)** | **Underfitting (subajuste)** |
|---|------------------------------|------------------------------|
| Ideia | Memoriza treino (inclui ruído) | Modelo simples demais |
| Treino | Erro baixo / desempenho alto | Erro alto |
| Teste/validação | Erro alto / desempenho baixo | Erro também alto |
| Viés × variância (noção) | Baixo viés, **alta variância** | **Alto viés**, baixa variância |

Sinais clássicos cobrados:

- excelente no treino, ruim na validação/teste → **overfitting**;
- ruim em ambos → suspeitar **underfitting** (ou dados ruins).

Mitigações de overfitting (lista cobrável):

- mais dados / data augmentation;
- **regularização** (L1/L2, weight decay);
- **dropout** (redes);
- **early stopping** (parada antecipada);
- reduzir complexidade do modelo;
- validação cruzada.

**Não** mitiga overfitting: aumentar complexidade à toa; olhar só acurácia no treino.

### 1.7 Matriz de confusão e métricas de classificação

Para classe positiva “Sim”:

|  | Predito Sim | Predito Não |
|--|-------------|-------------|
| **Real Sim** | TP (verdadeiro positivo) | FN (falso negativo) |
| **Real Não** | FP (falso positivo) | TN (verdadeiro negativo) |

Fórmulas cobráveis:

```
Acurácia      = (TP + TN) / (TP + TN + FP + FN)
Precisão (P)  = TP / (TP + FP)          ← “dos que marquei positivo, quantos eram?”
Recall / Sensibilidade (R) = TP / (TP + FN)  ← “dos positivos reais, quantos peguei?”
Especificidade = TN / (TN + FP)
F1            = 2 · P · R / (P + R)     ← média harmônica
```

Flash anti-confusão (FCC adora trocar nomes):

- **Recall = sensibilidade = revocação** (positivos reais recuperados);
- **Especificidade ≠ recall**;
- **Precisão ≠ acurácia**;
- **F1 ≠ sensibilidade** (F1 combina precisão e recall).

**Quando a acurácia engana:** classes desbalanceadas (99% classe majoritária → 99% acurácia “burra”). Aí priorize precisão/recall/F1/AUC.

**Curva ROC / AUC:** ROC plota **TPR (sensibilidade)** × **FPR (1 − especificidade)**. AUC alto = melhor discriminação. Não é “média de acurácia”.

### 1.8 Métricas de regressão (básico)

| Métrica | Ideia |
|---------|-------|
| **MAE** | Erro médio absoluto — quanto **menor**, melhor |
| **MSE / RMSE** | Penaliza erros grandes (quadrado) |
| **R²** | Fração da variância explicada (contexto importa) |

Pegadinha: “quanto maior o MAE, melhor” → **falso**.

### 1.9 Rede neural artificial — peças

**Neurônio artificial** recebe entradas \(x_i\), multiplica por **pesos** \(w_i\), soma **viés** \(b\) (bias), aplica **função de ativação** \(f\):

```
z = Σ (w_i · x_i) + b
y = f(z)
```

| Peça | Papel |
|------|-------|
| **Peso** | Força da conexão; **ajustado no treino** |
| **Viés** | Desloca o limiar de ativação |
| **Ativação** | Introduz **não linearidade** (ReLU, sigmoide, tanh, softmax…) |
| **Camada** | Conjunto de neurônios no mesmo nível |

**A ativação não armazena pesos** — quem “guarda” pesos são as conexões; ativação só transforma.

### 1.10 Perceptron e camadas

**Perceptron (clássico / camada única):**

- entrada → neurônio(s) de saída;
- **sem** camadas ocultas (no sentido clássico);
- fronteira de decisão **linear**;
- só resolve problemas **linearmente separáveis**;
- fluxo tipicamente **feedforward** (unidirecional).

**MLP (Multilayer Perceptron):**

- camada de **entrada** + uma ou mais **ocultas** + **saída**;
- ocultas permitem fronteiras **não lineares**;
- base do deep learning “denso”.

Hierarquia em deep learning (cobrável):

```
Camadas rasas  → características mais simples
Camadas profundas → combinações / abstrações mais complexas
```

### 1.11 Treinamento: loss, gradiente, backpropagation

1. Forward: calcular saída.  
2. Comparar com alvo → **função de perda (loss)**.  
3. **Backpropagation**: propagar o erro para trás usando a **regra da cadeia**, calculando gradientes ∂loss/∂w.  
4. **Otimizador** (SGD, Adam…) atualiza pesos na direção que reduz a loss (**gradient descent**).

Objetivo do treino: **minimizar erro** ajustando **pesos** — **não** “aumentar número de camadas até acurácia 100%”. Número de camadas é **hiperparâmetro de arquitetura**, definido antes.

Distinção FGV clássica:

- **Batch GD** usa (quase) todo o conjunto por passo;
- escolher aleatoriamente um **mini-lote** a cada passo = variante (mini-batch / SGD), **não** a definição pura de GD.

Regularização L1/L2: mitiga **overfitting** (pesos grandes), não é “cura mágica de inicialização ruim” isolada.

### 1.12 Arquiteturas — só o que a FCC costuma diferenciar

| Família | Gatilho de prova |
|---------|------------------|
| **MLP / densa** | Dados tabulares, problemas gerais |
| **CNN** | Imagens / estrutura espacial |
| **RNN / LSTM** | Sequências (legado; hoje menos “único”) |
| **Transformer** | Atenção; base de LLMs (detalhe no arquivo 08) |
| **Autoencoder** | Compressão / representação; não é classificador binário direto |

Exemplo cobrado: prever probabilidade favorável/desfavorável com features de processo → **regressão logística** (ou classificador tabular) costuma ser mais “adequado” na chave FCC do que CNN/autoencoder/k-means.

Redes neurais brilham quando há **padrões complexos/não lineares** e volume (ex.: fraude com relações sutis) — FCC PMSP/2025 cobrou RNA como técnica adequada para fraude complexa.

### 1.13 Cartão mental (antes das questões)

```
S = rótulo no treino | NS = sem rótulo (cluster/associação)
Overfit = treino ↑ teste ↓ | Underfit = ambos ↓
Acurácia ≠ precisão ≠ recall | F1 = harmônica P e R
ROC = TPR × FPR
Perceptron simples = linear, sem ocultas profundas
Ativação ≠ armazenar peso | Backprop = gradientes via regra da cadeia
Camadas ocultas = representações progressivamente complexas
```

---

## 2. Questões de treino (1–15)

**1.** Em aprendizado supervisionado, o elemento indispensável no conjunto de treinamento é:

A) Ausência total de rótulos  
B) Pares entrada–alvo (rótulos)  
C) Apenas recompensa do ambiente  
D) Obrigatoriamente imagens  
E) Somente regras de associação  

**2.** Segmentar clientes em grupos sem rótulo prévio de “tipo de cliente” é tipicamente:

A) Classificação supervisionada  
B) Regressão linear  
C) Aprendizado não supervisionado (clustering)  
D) RLHF  
E) Tokenização de LLM  

**3.** Overfitting caracteriza-se principalmente por:

A) Bom desempenho no treino e mau em dados novos  
B) Mau desempenho só no treino e ótimo no teste  
C) MAE sempre crescente no treino  
D) Ausência de pesos na rede  
E) Uso exclusivo de validação cruzada  

**4.** Underfitting ocorre quando o modelo:

A) Memoriza ruído do treino  
B) É simples demais e falha em capturar o padrão (treino e teste ruins)  
C) Tem AUC = 1 sempre  
D) Usa apenas regularização L2  
E) É necessariamente uma CNN  

**5.** Na matriz de confusão, recall (sensibilidade) mede:

A) TN / (TN + FP)  
B) TP / (TP + FN)  
C) (TP + TN) / total  
D) TP / (TP + FP)  
E) FP / (FP + TN)  

**6.** Precisão (precision) prioriza olhar para:

A) Dos positivos preditos, quantos são verdadeiros (TP/(TP+FP))  
B) Dos negativos reais, quantos acertou  
C) Só a diagonal da matriz  
D) Apenas o MAE  
E) O número de camadas ocultas  

**7.** Em base muito desbalanceada, confiar só na acurácia é perigoso porque:

A) Acurácia não existe em classificação  
B) Classe majoritária pode inflar acurácia sem utilidade real  
C) Recall passa a ser sempre 100%  
D) ROC deixa de existir  
E) F1 torna-se ilegal  

**8.** Perceptron de camada única clássico:

A) Resolve qualquer problema não linear com uma época  
B) Tem fronteira linear e limita-se a casos linearmente separáveis  
C) Armazena pesos na função softmax apenas  
D) É sinônimo de Transformer  
E) Não usa pesos ajustáveis  

**9.** Função de ativação em rede neural:

A) Armazena os pesos para a retropropagação  
B) Introduz transformação (em geral não linear) na saída do neurônio  
C) Substitui o conjunto de teste  
D) É o nome do otimizador Adam  
E) Mede apenas o MAE  

**10.** Backpropagation, em essência:

A) Aumenta camadas ocultas até acurácia 100%  
B) Calcula gradientes do erro em relação aos pesos (regra da cadeia) para atualizá-los  
C) Remove a necessidade de função de perda  
D) Converte clustering em supervisão automaticamente  
E) É exclusiva de regras de associação  

**11.** Em deep learning, camadas ocultas mais profundas tendem a:

A) Aprender apenas ruído de sensor obrigatoriamente  
B) Extrair representações progressivamente mais abstratas/complexas  
C) Eliminar a camada de saída  
D) Substituir o conjunto de validação  
E) Garantir ausência de overfitting  

**12.** MAE em regressão:

A) Quanto maior, melhor o modelo  
B) É a área sob a ROC  
C) Mede erro médio absoluto; quanto menor, melhor (em geral)  
D) É sinônimo de precisão  
E) Só existe em clustering  

**13.** Regras de associação são tipicamente:

A) Predição de classe com rótulo obrigatório  
B) Tarefas descritivas sobre padrões em transações  
C) O mesmo que regressão logística  
D) Treino de LLM por RLHF  
E) Cálculo de F1  

**14.** Técnica clássica para mitigar overfitting em redes:

A) Remover o conjunto de teste  
B) Regularização e/ou early stopping (e/ou dropout)  
C) Aumentar complexidade sem validação  
D) Avaliar só no treino  
E) Trocar recall por especificidade no enunciado  

**15.** Síntese correta deste subtópico:

A) S usa rótulos; NS busca estrutura; overfit = treino≠teste; métricas ≠ só acurácia; rede = pesos+ativação+camadas; backprop ajusta pesos  
B) Perceptron = LLM generativo  
C) F1 = especificidade  
D) Clustering exige rótulo binário  
E) Ativação armazena pesos  

---

### Gabarito comentado (treino 1–15)

1. **B** — Supervisionado = entradas com alvo.  
2. **C** — Clustering sem rótulo.  
3. **A** — Memoriza treino, generaliza mal.  
4. **B** — Modelo pobre demais.  
5. **B** — TP/(TP+FN).  
6. **A** — Precisão olha FP.  
7. **B** — Desbalanceamento.  
8. **B** — Linearmente separável.  
9. **B** — Transformação; não memória de pesos.  
10. **B** — Gradientes via regra da cadeia.  
11. **B** — Hierarquia de features.  
12. **C** — Menor MAE costuma ser melhor.  
13. **B** — Descritivo / transações.  
14. **B** — Regularização / early stop / dropout.  
15. **A** — Cartão do arquivo.

---

## 3. Questões de concurso (16–25)

> **Nota:** questões **reais** de concurso (preferência FCC / Analista TI), com banca, órgão e ano. Treino 1–15 é didático. Gabarito só no final.

**16.** (FCC – TRT-22/2022 – Analista Judiciário – Tecnologia da Informação)

Considere, abaixo, as situações que envolvem métodos de aprendizado de máquina que podem ser supervisionados (S) ou não supervisionados (NS).

I. Uma empresa monitora e coleta dados em tempo real de mais de 1.000 veículos de divulgação, 100 mil blogs e das principais redes sociais do mundo. Utiliza um método capaz de identificar e segmentar grupos de eleitores a favor ou contra determinado assunto, de acordo com o interesse do cliente.

II. A partir de uma base de dados na qual são identificadas pessoas que estão fumando e pessoas que não estão fumando, são formados dois subconjuntos disjuntos: a base de treino (contendo 70% dos dados originais) e a base de teste (contendo o restante dos dados originais, 30%). Em seguida, a base de treino é submetida ao modelo para que seus parâmetros sejam calibrados e, após esta etapa, ocorre a predição de classes.

III. O sistema de recomendação de um site de comércio eletrônico monitora todos os itens vendidos e, quando um cliente está realizando uma compra, apresenta para ele itens semelhantes frequentemente comprados juntos.

Os itens I, II e III, são exemplos, respectivamente, de métodos

A) S − S − NS.  
B) NS − NS − S.  
C) S − NS − S.  
D) NS − S − NS.  
E) S − NS − NS.

**17.** (FCC – TRT-20/2024 – Analista Judiciário – Tecnologia da Informação)

A equipe de analistas de um órgão público está avaliando o desempenho de dois modelos de machine learning: um modelo de classificação e outro de regressão. O modelo de classificação utiliza uma curva ROC para analisar a performance, enquanto o modelo de regressão utiliza o erro médio absoluto (MAE) como métrica principal. Durante a avaliação, a equipe percebe que o modelo de classificação está apresentando sobreajuste (overfitting). Uma Analista da equipe afirmou, corretamente, que o sobreajuste ocorre quando o modelo

A) é simples demais e não consegue capturar as complexidades dos dados. A curva ROC compara a acurácia geral do modelo de classificação, enquanto o MAE avalia o desempenho: quanto maior o MAE, melhor é o desempenho do modelo. Para resolver o problema de sobreajuste, deve-se aumentar o número de variáveis no modelo.  
B) generaliza excessivamente para dados novos e não consegue se adequar aos dados do treinamento. A curva ROC é capaz de calcular o desempenho do modelo, por meio da média entre verdadeiros positivos (especificidade) e falsos positivos (sensibilidade). O MAE é uma métrica de erro quadrático médio que pode ser usada para melhorar o desempenho do modelo. Para eliminar o sobreajuste, deve-se aplicar a técnica de balanceamento de dados conhecida como avaliação cruzada.  
C) aprende os padrões específicos do conjunto de treinamento, fornecendo previsões precisas para estes dados, mas com baixo desempenho em novos dados. A curva ROC avalia a relação entre a taxa de verdadeiros positivos e falsos negativos, enquanto o MAE mede o desvio médio absoluto entre os valores previstos e os reais. Para mitigar o sobreajuste, técnicas como regularização e parada antecipada podem ser usadas.  
D) aprende as características gerais do conjunto de treinamento, generalizando bem para dados novos. A curva ROC mede a precisão de um modelo de classificação e o MAE mede a precisão de um modelo de regressão. Para reduzir o sobreajuste, deve-se aumentar a complexidade do modelo.  
E) não consegue aprender os padrões do conjunto de treinamento. A curva ROC mostra a relação entre a taxa de falsos positivos e a taxa de verdadeiros negativos, enquanto o MAE mede a diferença entre a previsão mais provável e o valor real. Para reduzir o sobreajuste, deve-se remover os dados desbalanceados do conjunto de treinamento.

**18.** (FCC – TRF-4/2025 – Analista Judiciário – Análise de Sistemas de Informação)

Em um projeto de implantação de sistemas de apoio à decisão judicial, a equipe de analistas de um tribunal se depara com a necessidade de avaliar o desempenho de diferentes modelos de aprendizado de máquina. Durante os testes, observa-se que um dos modelos apresenta excelente desempenho nos dados de treinamento, mas performance insatisfatória nos dados de validação. Para diagnosticar e corrigir problemas como esse e selecionar o modelo mais adequado, a equipe deve considerar conceitos como

A) normalização de variáveis de entrada, ajuste de hiperparâmetros e ênfase na acurácia como principal métrica para todos os conjuntos de dados.  
B) overfitting, análise de precisão e recall, uso do F1-score e interpretação da curva ROC.  
C) ampliação do conjunto de treino e avaliação do desempenho com base em métricas simples, como o percentual de acertos em dados conhecidos.  
D) substituição de validação cruzada por divisão fixa dos dados, acompanhada de repetição de testes em conjuntos balanceados.  
E) análise do tempo de processamento em paralelo às métricas de avaliação, priorizando modelos com melhor desempenho computacional em tarefas específicas.

**19.** (FCC – TRT-15/2023 – Analista Judiciário – Tecnologia da Informação)

Um Advogado deseja aplicar uma técnica de Machine Learning para prever a probabilidade de uma decisão judicial ser favorável ou desfavorável a uma das partes com base em variáveis como jurisprudência, detalhes do caso e histórico de decisões anteriores. O recurso de Machine Learning mais adequado para essa finalidade é

A) Convolutional Neural Network (CNN).  
B) a técnica de Logistic Regression.  
C) o algoritmo de K-Means Clustering.  
D) Support Vector Machines (SVM) for regression.  
E) Autoencoders.

**20.** (FCC – TRT-4/2022 – Técnico Judiciário – Tecnologia da Informação)

Um Técnico necessitou estudar a respeito de aprendizado de máquina. Durante as pesquisas observou, corretamente, que

A) regras de associação são tarefas inerentes a modelos descritivos.  
B) regras de associação não representam padrões existentes nas transações de um banco de dados.  
C) para usar os algoritmos de regras de associação os dados no formato de tabela não necessitam ser convertidos para o formato de transação.  
D) regras de associação são tarefas inerentes a modelos preditivos.  
E) em uma regra de associação deve existir uma definição explícita de classe.

**21.** (FCC – TRT-18/2023 – Analista Judiciário – Tecnologia da Informação)

Considere a matriz de confusão abaixo, que mostra o desempenho de um modelo de classificação, na qual TP = Verdadeiro positivo, TN = Verdadeiro negativo, FP = Falso positivo e FN = Falso negativo:

|  | Predição Sim | Predição Não |
|--|--------------|--------------|
| Realidade Sim | 600 (TP) | 400 (FN) |
| Realidade Não | 100 (FP) | 900 (TN) |

Para se fazer a avaliação do modelo, utilizam-se as métricas de classificação acurácia, especificidade e sensibilidade, assim definidas:

Acurácia = (TP + TN) / (TP + TN + FP + FN)  
Especificidade = TN / (TN + FP)  
Sensibilidade = TP / (TP + FN)

Com base na matriz de confusão e nas métricas indicadas, afirma-se que a

A) especificidade, também conhecida como recall ou revocação, que avalia a capacidade do modelo de detectar com sucesso resultados classificados como positivos, é de 0,9%.  
B) sensibilidade, que avalia a capacidade do modelo de detectar com sucesso resultados classificados como positivos, é de 90%.  
C) acurácia, ou precision, que mostra a razão entre a quantidade de exemplos classificados corretamente como positivos e o total de exemplos classificados como positivos, dando maior ênfase para os erros FP, é de 0.75.  
D) sensibilidade, também conhecida como F-measure, F-score ou score F1, que avalia a capacidade do modelo de detectar com sucesso resultados classificados como negativos, é de 0,6%.  
E) acurácia, que indica a assertividade de classificações positivas e negativas, independente da classe, é de 75%.

**22.** (FCC – Pref. São Paulo/2025 – Analista de Planejamento e Desenvolvimento Organizacional – TIC)

Com respeito à renegociação de valores do IPTU ocorrida na esfera municipal de uma grande cidade, a técnica de machine learning mais adequada para detectar uma possível fraude aplicada por cidadãos é:

A) Análise de cluster: Agrupar as transações em clusters e identificar aqueles com características atípicas.  
B) Regressão linear: Modelar a relação entre as variáveis para prever o valor de uma transação potencialmente fraudulenta e identificar desvios.  
C) Análise de componentes principais (PCA): Reduzir a dimensionalidade dos dados para visualizar padrões e anomalias características de fraudes.  
D) Redes neurais artificiais: Construir um modelo capaz de aprender padrões complexos nas transações e identificar aquelas com alto risco de fraude.  
E) Análise de sentimento: Aplicar Regard Review on ML (R2ML) nos textos dos processos impetrados por cidadãos supostamente envolvidos em fraudes do IPTU, para identificar possíveis características sentimentais indicativas de fraudulência.

**23.** (FCC – Pref. São Paulo/2025 – Analista de Planejamento e Desenvolvimento Organizacional – TIC)

Uma equipe de ciência de dados de uma prefeitura está utilizando um modelo de classificação binária para prever a aprovação de projetos de mobilidade urbana. O desempenho do modelo é avaliado com base em algumas métricas. Considere o seguinte código em Python, que foi executado para calcular as métricas a partir dos rótulos verdadeiros e previstos.

```python
from sklearn.metrics import precision_score, recall_score, f1_score
true_labels = [1, 0, 1, 1, 0, 1, 0, 0, 1, 0]
predicted_labels = [1, 0, 1, 0, 0, 1, 1, 0, 1, 0]
precision = precision_score(true_labels, predicted_labels)
recall = recall_score(true_labels, predicted_labels)
f1 = f1_score(true_labels, predicted_labels)
print(f"Precisão: {precision:.2f}")
print(f"Recall: {recall:.2f}")
print(f"F1-Score: {f1:.2f}")
```

Com os dados fornecidos, o código gerou os seguintes resultados: Precisão: 0,80, Recall: 0,80, F1-Score: 0,80. Com base nessas métricas, o modelo

A) apresenta alta precisão, mas o recall é insuficiente, indicando que ele detecta a maioria dos casos positivos, mas também faz muitas classificações incorretas.  
B) apresenta um bom equilíbrio entre precisão e recall, indicando que ele identifica bem os casos positivos e evita erros ao classificar dados negativos.  
C) apresenta baixa precisão e recall, indicando que ele frequentemente erra tanto ao prever positivos quanto ao evitar falsos negativos.  
D) é ótimo para cenários em que precisão não é tão importante quanto recall, como em problemas de emergência pública, pois F1-score é alto.  
E) tem precisão aceitável, mas o baixo F1-score sugere um desempenho desequilibrado, com recall bem superior à precisão.

**24.** (FGV – DATAPREV/2024 – Analista de Tecnologia da Informação – Inteligência da Informação)

Um conjunto de dados foi particionado em dois subconjuntos, sendo um de treinamento e outro de testagem, ambos utilizados exclusivamente para serem usados em seus objetivos originais (dados de treino para treinamento, e de teste para testagem). Em relação ao ajuste e validação de modelos em aprendizado de máquina, um modelo sofre overfitting quando

A) apresenta bom desempenho nos dados de treinamento, mas tem baixo desempenho nos dados de teste.  
B) é simples demais para capturar os padrões subjacentes nos dados.  
C) minimiza o erro nos dados de treinamento e os de teste, aumentando a correlação entre esses dois conjuntos.  
D) oferece uma matriz de confusão para um classificador detalhando seu desempenho.  
E) relaciona-se aos altos viés e variância do conjunto de dados.

**25.** (FGV – EPE/2024 – Analista de Gestão Corporativa – TI – Ciência de Dados)

O treinamento em redes neurais artificiais é, essencialmente, um problema de otimização, em que se busca minimizar o erro entre as predições do modelo neural e os valores alvo preexistentes no conjunto de dados de treinamento. Com respeito aos métodos relacionados à otimização de parâmetros em redes neurais artificiais, analise as afirmativas a seguir.

I. No algoritmo backpropagation, a aplicação da regra da cadeia é fundamental para o cálculo dos gradientes dos erros com respeito aos pesos (parâmetros) da rede neural artificial.  
II. O método do gradiente (também chamado de método do máximo declive ou Gradient Descent – GD) escolhe aleatoriamente um pequeno número de instâncias de dados de treinamento a cada passo, aumentando a velocidade inicial de minimização das métricas de erro.  
III. As técnicas de regularização L1 (Lasso) e L2 (Ridge) podem ser utilizadas para mitigar problemas relacionados a inicialização indevida de pesos (parâmetros) em redes neurais.

Está correto o que se afirma em

A) I, apenas.  
B) II, apenas.  
C) I e III, apenas.  
D) II e III, apenas.  
E) I, II e III.

---

### Gabarito comentado (concurso 16–25)

| Questão | Gabarito | Fonte/nota |
|---------|----------|------------|
| 16 | **D** | Quizinho FCC TRT-22 2022 — NS / S / NS |
| 17 | **C** | Quizinho FCC TRT-20 2024 Q28 — overfitting + ROC/MAE |
| 18 | **B** | Quizinho FCC TRF-4 2025 Q43 — overfit + P/R/F1/ROC |
| 19 | **B** | Quizinho FCC TRT-15 2023 Q25 — regressão logística |
| 20 | **A** | Estude Grátis / Gran FCC TRT-4 2022 Téc. — associação descritiva |
| 21 | **E** | QConcursos/CFP FCC TRT-18 2023 — acurácia 75% (1500/2000) |
| 22 | **D** | Quizinho FCC PMSP 2025 Q89 — redes neurais / fraude |
| 23 | **B** | Quizinho FCC PMSP 2025 Q88 — P=R=F1=0,80 |
| 24 | **A** | Quizinho FGV DATAPREV 2024 Q58 — overfit treino×teste |
| 25 | **A** | Quizinho FGV EPE 2024 Q63 — backprop / regra da cadeia |

## 4. Como usar este arquivo

1. Ler §1 (cartão §1.13).  
2. Fechar treino 1–15 sem gabarito → conferir.  
3. Fazer 16–25 em modo prova.  
4. Anotar erros: S×NS; overfit×underfit; precisão×recall×acurácia; perceptron×MLP; ativação×peso.  
5. Seguir para `08-ia-generativa-llms.md` (generativa / LLM / RAG / ética).

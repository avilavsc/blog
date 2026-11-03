+++
title = '08 — IA generativa + LLMs'
date = '2026-10-31'
draft = false
weight = 8
tags = ['semana-09', 'ia']
categories = ['estudo']
+++
# IA generativa + LLMs · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 08 — IA generativa e grandes modelos de linguagem  
**Escopo deste arquivo:** conceitos cobráveis de **IA generativa** e **LLMs** (tokens, embeddings, contexto, prompt / prompt engineering, zero-shot × few-shot, hallucination, **RAG** em noção, RLHF/alinhamento, Transformer/atenção em flash) + **riscos/ética** no serviço público e judiciário (LGPD, supervisão humana, desinformação, direitos autorais, vieses). Alinhado ao Anexo II (*IA generativa e LLMs*).  
**Fora de escopo aqui:** ML clássico / métricas / perceptron em profundidade (arquivo `07`); low-code e assistentes de IDE no ciclo de software (semana 4 · `06`); fine-tuning linha a linha de frameworks; política CNJ de IA além do nível cobrável de conceito.

---

## 1. Material

### 1.1 Por que cai no edital

FCC e bancas irmãs passaram a cobrar o **vocabulário operacional** de LLMs — não treinar GPT do zero. No judiciário, a chave é: **ferramenta de apoio** + **humano responsável** + **risco controlado**.

```
IA discriminativa / preditiva  → classifica ou estima (arquivo 07)
IA generativa                  → produz texto, imagem, código, áudio…
LLM                            → modelo de linguagem em larga escala (texto)
RAG / prompt / RLHF            → técnicas de uso e alinhamento
```

### 1.2 IA generativa — definição cobrável

**IA generativa** = sistemas capazes de **criar** conteúdo novo (texto, imagem, código, áudio, vídeo) a partir de padrões aprendidos em grandes volumes de dados.

Não é “só classificar spam”. Não “decide processo” sozinha em órgão sério. Gera **candidatos** a resposta/minuta/resumo que exigem **revisão**.

Riscos clássicos (FCC TRF-4/2025 cobrou no Judiciário):

- **desinformação** / conteúdo falso convincente;
- **violação de direitos autorais** (treino/saída);
- vieses e opacidade (“caixa-preta”);
- exposição de **dados sensíveis** se mal governada;
- **não** elimina impactos sociais da automação — pode criá-los.

### 1.3 LLM — o que é

**LLM (Large Language Model)** = rede (hoje tipicamente **Transformer**) treinada em escala massiva de texto para modelar linguagem: completar, resumir, traduzir, responder, gerar código etc.

Como aprende (visão cobrável):

- em geral **auto-supervisionado** sobre texto bruto (ex.: prever próximo token);
- **não** é “só regras sintáticas anotadas à mão” (CESPE STM/2025 cobrou o contrário como **errado**);
- depois pode haver **fine-tuning** e **alinhamento** (ex.: RLHF).

Capacidades típicas em órgão público (FCC MPE-SE/2026):

- interpretar linguagem natural;
- classificar / triar demandas;
- gerar **respostas preliminares**;
- **desde que supervisionados por especialistas**.

O que LLM **não** é na chave FCC/judiciário:

- juiz/promotor automático;
- garantia de verdade factual;
- desculpa para colar segredo de justiça em chat público;
- substituto de responsabilidade funcional.

### 1.4 Tokens, embeddings, janela de contexto

| Conceito | Ideia cobrável |
|----------|----------------|
| **Token** | Unidade de texto que o modelo processa (subpalavra/palavra/símbolo). Custo e limite costumam ser em tokens. |
| **Tokenização** | Quebrar texto em tokens (pré-processamento ainda existe — Transformer **não** “lê PDF mágico sem pipeline”). |
| **Embedding** | Vetor numérico que representa token/texto no espaço do modelo. |
| **Contexto / context window** | Quantidade máxima de tokens que o modelo “enxerga” por vez (prompt + histórico + resposta). |
| **Temperatura** (noção) | Controla aleatoriedade da amostragem; alta → mais criativo/instável. |

Flash: “aumentar tokens/embeddings” **não** é, sozinho, a cura de alucinação/viés alinhado a política institucional — FCC TRT-6/2025 usou isso como distrator frente a **RLHF**.

### 1.5 Prompt e prompt engineering

**Prompt** = instrução/entrada textual (e multimodal) fornecida ao modelo.

**Prompt engineering** = elaborar prompts com **papel**, **contexto**, **formato**, **restrições** e **exemplos** para obter saída mais útil — **sem** necessariamente retreinar o modelo.

FCC MPE-SE/2026: chatbot jurídico genérico demais → técnico deve aplicar **prompt engineering** (papéis, contexto, exemplos), não “clusterização mágica” nem só filtro de moderação.

Padrões cobráveis:

| Modo | O que se dá ao modelo |
|------|------------------------|
| **Zero-shot** | Só a instrução/tarefa, **sem** exemplos |
| **One-shot** | Um exemplo |
| **Few-shot** | Poucos exemplos no prompt |
| **Chain-of-thought** (noção) | Pedir raciocínio passo a passo (cuidado: não garante verdade) |

CESPE Câmara/2025: zero-shot = instrução inicial **sem exemplos adicionais**, explorando generalização do pré-treino → **Certo**.

### 1.6 Hallucination (alucinação)

**Alucinação** = modelo gera conteúdo **incorreto, inventado ou não verificável** com aparência fluente/confiante.

Por que importa no judiciário: minuta com jurisprudência inventada, citação falsa, prazo errado — risco operacional e ético.

Mitigações cobráveis (camadas):

1. **Supervisão humana** obrigatória em decisões/peças sensíveis.  
2. **Prompt** restritivo (“responda só com base no trecho”; “se não souber, diga que não sabe”).  
3. **RAG** — ancorar em base documental confiável.  
4. **RLHF / alinhamento** — reduzir comportamento indesejado.  
5. **Governança**: logs, avaliação, políticas de uso, LGPD.

Alucinação **não** some só “reduzindo camadas da rede”.

### 1.7 RAG — Geração Aumentada por Recuperação

**RAG (Retrieval-Augmented Generation)** = combinar:

1. **Recuperação** de trechos relevantes em base externa (normas, jurisprudência, doutrina, base interna);  
2. **Geração** pelo LLM condicionada a esses trechos.

Objetivo principal (FGV STN/2024): **enriquecer** a geração com informação de bases externas **no momento da consulta**, elevando precisão/atualidade e reduzindo alucinação factual.

Fluxo mental:

```
Pergunta → busca (vetorial/BM25/híbrida) → trechos → prompt com evidências → LLM gera → (citar fontes)
```

Vantagens cobráveis:

- conhecimento **atualizável** sem retreinar o modelo-base a cada norma nova;
- melhor **rastreabilidade** (quando bem implementado);
- adequação a domínio (tribunal, MP).

Armadilhas:

- RAG **não** é a mesma coisa que RLHF;
- RAG **não** é geração de imagem;
- afirmativa absoluta “RAG **nunca** altera parâmetros do generativo” pode ser considerada **categórica demais** (FGV TCE-PA/2024: I e II certas; III falsa — há variantes com fine-tuning).

### 1.8 Transformer, atenção, RLHF (flash)

| Termo | Ideia |
|-------|-------|
| **Transformer** | Arquitetura com **mecanismo de atenção**; base dos LLMs modernos |
| **Atenção / self-attention** | Pesar relevância entre tokens no contexto |
| **Fine-tuning** | Ajuste adicional em dados específicos do domínio |
| **RLHF** | Reinforcement Learning from Human Feedback — alinhamento com preferências humanas |

FCC TRT-6/2025: para reduzir respostas inapropriadas/incorretas, alucinações e vieses em LLM de TRT → **RLHF** com feedback humano. Distratores: zero-shot “no treinamento”, fine-tuning **sem atenção**, só “mais tokens”.

MLOps (noção, mesma prova): Design (requisitos) → Experimentação/Desenvolvimento (modelo, features, tuning; transformers úteis em sequências) → Operações (deploy CI/CD + monitoramento). Não embaralhar as fases.

### 1.9 Riscos, ética e judiciário (caderno FCC)

| Risco | Controle típico |
|-------|-----------------|
| Alucinação / erro factual | Revisão humana; RAG; recusar inventar fonte |
| Viés algorítmico | Avaliação; dados; monitoramento; não automatizar decisão sensível às cegas |
| LGPD / sigilo | Ferramentas homologadas; minimizar dados; não colar PII/segredo em SaaS não autorizado |
| Desinformação | Transparência de uso; disclaimer; curadoria |
| Direitos autorais | Política de uso de saída; cuidado com treino/conteúdo |
| Decisão automatizada indevida | Humano no loop; LLM **não** substitui magistrado/membro |
| Shadow AI | Catálogo de ferramentas; política institucional |

Princípio ouro (repete o da semana 4, agora no bloco teórico):

```
LLM = assistente
Servidor público = responsável pelo artefato
```

### 1.10 Cartão mental (antes das questões)

```
Generativa = cria conteúdo | LLM = modelo de linguagem em escala
Token/embedding/contexto = unidade, vetor, janela
Prompt eng. = papel + contexto + exemplos (zero/few-shot)
Alucinação = fluente ≠ verdadeiro
RAG = recupera evidência + gera (mitiga factual drift)
RLHF = alinhamento com feedback humano
Judiciário: supervisão, LGPD, sem decisão automática “solta”
```

---

## 2. Questões de treino (1–15)

**1.** IA generativa, em definição cobrável, caracteriza-se por:

A) Apenas classificar e-mails em spam/não spam  
B) Gerar conteúdo novo (texto/imagem/código etc.) a partir de padrões aprendidos  
C) Ser sinônimo exclusivo de firewall  
D) Eliminar a LGPD  
E) Substituir obrigatoriamente o juiz  

**2.** LLM tipicamente:

A) Opera só com planilhas estritamente estruturadas e falha em linguagem natural  
B) Modela linguagem em larga escala e pode apoiar triagem/respostas com supervisão  
C) Garante decisão judicial automatizada com força de coisa julgada  
D) Dispensa qualquer política de dados  
E) É idêntico a k-means  

**3.** Token, no contexto de LLM, é melhor descrito como:

A) Certificado digital ICP-Brasil  
B) Unidade de texto processada pelo modelo  
C) Camada oculta da MLP clássica  
D) Métrica MAE  
E) Tipo de clustering  

**4.** Zero-shot prompting:

A) Exige dezenas de exemplos rotulados no prompt  
B) Fornece só a instrução/tarefa, sem exemplos adicionais  
C) É o nome do otimizador Adam  
D) Remove a tokenização  
E) É obrigatoriamente RLHF  

**5.** Prompt engineering adequado a chatbot jurídico genérico demais costuma incluir:

A) Apagar a camada de atenção do Transformer  
B) Definir papel, contexto institucional e exemplos explícitos na instrução  
C) Proibir qualquer revisão humana  
D) Treinar k-means nos prompts  
E) Aumentar só o MAE  

**6.** Alucinação em LLM significa sobretudo:

A) Overfitting clássico de regressão linear tabular  
B) Geração fluente de conteúdo incorreto ou não verificável  
C) Backup de banco  
D) VLAN mal configurada  
E) Acurácia de 100% no teste  

**7.** RAG, em essência:

A) Substitui o LLM por PCA  
B) Combina recuperação de informações externas com geração pelo modelo  
C) É apenas aprendizado por reforço sem retrieval  
D) Gera só imagens a partir de áudio  
E) Elimina a necessidade de prompt  

**8.** Objetivo principal do RAG frente a alucinação factual:

A) Reduzir o número de camadas para acelerar sempre  
B) Ancorar a resposta em evidências recuperadas de bases confiáveis no momento da consulta  
C) Remover a supervisão humana  
D) Proibir embeddings  
E) Trocar recall por especificidade  

**9.** RLHF serve principalmente para:

A) Tokenizar PDFs sem OCR  
B) Alinhar o comportamento do modelo com feedback/preferências humanas (segurança, utilidade)  
C) Calcular F1 em clustering  
D) Configurar STP  
E) Substituir a LGPD  

**10.** Afirmação alinhada ao uso institucional de LLM em órgão público:

A) Pode apoiar triagem e respostas preliminares **com supervisão** de especialistas  
B) Deve decidir processos sem humano  
C) Só funciona se treinado exclusivamente com dados internos, sem qualquer base generalista  
D) Dispensa fine-tuning/contexto sempre  
E) Garante autoria individual reforçada automaticamente  

**11.** Risco típico de IA generativa no Poder Judiciário:

A) Aumento automático e garantido de transparência e fim de vieses  
B) Disseminação de desinformação e questões de direitos autorais  
C) Eliminação de impactos sociais da automação  
D) Proteção intrínseca de dados sensíveis sem política  
E) Substituição da Constituição  

**12.** Mecanismo de atenção em Transformers:

A) Armazena pesos na função ReLU apenas  
B) Permite ponderar relevância entre elementos do contexto  
C) É proibido em LLMs modernos  
D) É o nome da matriz de confusão  
E) Mede só especificidade  

**13.** Boa prática com dados sensíveis e LLM SaaS não homologado:

A) Colar inteiro o processo sob segredo de justiça para “melhor contexto”  
B) Evitar exfiltrar PII/sigilo; usar canais homologados e minimizar dados  
C) Publicar prompts com senhas no Git  
D) Desligar logs de auditoria  
E) Automatizar sentença sem revisão  

**14.** Few-shot difere de zero-shot porque:

A) Não usa tokens  
B) Inclui alguns exemplos no prompt  
C) Remove o modelo generativo  
D) É sinônimo de k-means  
E) Calcula apenas MAE  

**15.** Síntese correta deste subtópico:

A) Generativa cria; LLM modela linguagem; prompt orienta; alucinação ≠ verdade; RAG ancora; RLHF alinha; humano responsável no judiciário  
B) RAG = apenas reduzir camadas  
C) Zero-shot exige 100 exemplos  
D) LLM decide com força de lei  
E) Token = certificado A1  

---

### Gabarito comentado (treino 1–15)

1. **B** — Criação de conteúdo.  
2. **B** — Linguagem + supervisão.  
3. **B** — Unidade de texto.  
4. **B** — Sem exemplos.  
5. **B** — Prompt engineering.  
6. **B** — Conteúdo falso fluente.  
7. **B** — Retrieve + generate.  
8. **B** — Evidências externas.  
9. **B** — Alinhamento humano.  
10. **A** — Apoio supervisionado.  
11. **B** — Desinformação / autorais.  
12. **B** — Atenção contextual.  
13. **B** — LGPD / homologação.  
14. **B** — Exemplos no prompt.  
15. **A** — Cartão do arquivo.

---

## 3. Questões de concurso (16–25)

> **Nota:** questões **reais** (preferência FCC; outras bancas em nível Analista TI / ciência de dados quando FCC ainda é escassa no tópico). Gabarito só no final. Itens CESPE mantêm formato Certo/Errado.

**16.** (FCC – TRT-6/2025 – Analista Judiciário – Tecnologia da Informação)

Uma equipe de Analistas está implementando um modelo LLM para uso no Tribunal Regional do Trabalho e precisa garantir que o modelo não gere respostas inapropriadas ou incorretas, mantendo a relevância e coerência do conteúdo. Para ajustar o desempenho do modelo e evitar problemas como alucinações e vieses, eles optaram por

A) reduzir o número de camadas de rede neural para minimizar o processamento e aumentar a velocidade de inferência, pois isso reduziria erros causados por dados não estruturados.  
B) usar o aprendizado zero-shot durante o treinamento, pois ele é suficiente para que o modelo aprenda a generalizar padrões sem supervisão.  
C) utilizar Reinforcement Learning from Human Feedback (RLHF) para treinar o modelo a partir de respostas corrigidas por humanos.  
D) implementar fine-tuning com um conjunto de dados menor e mais específico, sem utilizar nenhum mecanismo de atenção para evitar sobrecarga de processamento.  
E) aumentar o número de tokens e embeddings durante o treinamento para que o modelo tenha mais dados contextuais disponíveis.

**17.** (FCC – MPE-SE/2026 – Técnico do Ministério Público – Manutenção e Suporte de Equipamentos de Informática e Softwares)

Durante o desenvolvimento de um chatbot interno no Ministério Público, a equipe de TI percebeu que as respostas da IA, embora corretas, eram genéricas e pouco adaptadas ao contexto jurídico da instituição. O objetivo passou a ser obter respostas mais detalhadas, com terminologia adequada e foco nas normas e procedimentos internos. O técnico responsável foi orientado a revisar a formulação das instruções dadas ao modelo, ajustando a forma e a estrutura dos comandos textuais. Para isso, o técnico deve fazer

A) a aplicação de técnicas de prompt-engineering, definindo papéis, contexto e exemplos explícitos na instrução.  
B) uso de mecanismos e filtros de moderação de conteúdo para restringir a saída do modelo a temas jurídicos.  
C) o refinamento de clusters do modelo de linguagem para aumentar a precisão estatística das respostas.  
D) a expansão do conjunto de treinamento com documentos públicos e decisões judiciais disponíveis em bases abertas.  
E) a implementação de um pipeline de aprendizado de esforço com a curadoria de promotores e procuradores.

**18.** (FCC – MPE-SE/2026 – Técnico do Ministério Público – Manutenção e Suporte de Equipamentos de Informática e Softwares)

Ao analisar a viabilidade da implementação de um assistente virtual baseado em IA para auxiliar na triagem de demandas no Ministério Público (MP), um Técnico foi consultado e afirmou que os LLMs

A) podem ser integrados a sistemas de atendimento para interpretar linguagem natural, classificar demandas e gerar respostas preliminares, desde que supervisionados por especialistas.  
B) aplicados em órgãos públicos, como é o caso do MP, requerem que os modelos sejam treinados apenas com dados internos, sem uso de bases externas ou generalistas.  
C) operam de forma otimizada apenas com dados estruturados, o que limita sua aplicação em ambientes jurídicos com documentos em linguagem natural.  
D) têm como principal vantagem, em ambientes públicos, como é o caso do MP, a capacidade de gerar decisões jurídicas automatizadas com base em jurisprudência consolidada.  
E) utilizados em chatbots institucionais podem dispensar a necessidade de treinamento adicional, uma vez que os modelos conseguem aprender com todos os contextos legais.

**19.** (FCC – TRF-4/2025 – Conhecimentos comuns / Técnico)

No exercício das atividades de desenvolvimento e supervisão de soluções de Inteligência Artificial (IA) no Poder Judiciário, a equipe de inovação precisa avaliar cuidadosamente os riscos associados ao uso de sistemas de IA generativa. Considerando os principais desafios identificados na literatura e nas normativas sobre o tema, o uso de IA generativa pode implicar riscos relacionados

A) ao aumento da transparência nos dados e redução de vieses algorítmicos.  
B) ao fortalecimento automático da autoria individual em conteúdos gerados.  
C) à disseminação de desinformação e violação de direitos autorais.  
D) à eliminação de impactos sociais decorrentes da automação de tarefas.  
E) à proteção intrínseca dos dados sensíveis suprimindo a regulamentação adicional.

**20.** (FGV – TCE-PA/2024 – Auditor de Controle Externo – Ciência de Dados)

Alguns dos primeiros modelos de linguagem de larga escala desenvolvidos tendiam a gerar resultados incorretos com excesso de confiança, caracterizando o que se convencionou chamar de alucinações dos modelos. Uma estratégia de mitigação das alucinações é o uso da técnica de Geração Aumentada por Recuperação, ou Retrieval-Augmented Generation (RAG). A respeito da RAG, avalie as afirmativas a seguir.

I. Baseia-se na combinação de sistemas de recuperação de informações e de modelos generativos capazes de produzir novos textos.  
II. Permite aos modelos buscarem informações relevantes em bases de dados mais confiáveis durante o processamento das consultas dos usuários (user queries), viabilizando melhor adequação a contextos e melhor qualidade das respostas.  
III. Não altera os parâmetros dos modelos generativos, e, portanto, não influencia o treinamento das redes neurais com informações recuperadas de bases de dados externas.

Está correto o que se afirma em

A) I, apenas.  
B) II, apenas.  
C) III, apenas.  
D) I e II, apenas.  
E) I, II e III.

**21.** (FGV – STN/2024 – Auditor Federal de Finanças e Controle – Tecnologia da Informação – Transformação Digital)

No contexto do Processamento de Linguagem Natural, a geração aumentada de recuperação (ou RAG, do inglês Retrieval-Augmented Generation) é uma técnica que visa a melhorar a acurácia e a confiabilidade de modelos de IA Generativa. O objetivo principal dessa técnica é

A) facilitar a interação entre modelos de IA generativa e interfaces de usuário, melhorando a experiência do usuário ao gerar respostas mais interativas.  
B) diminuir o volume de dados necessário para o treinamento de modelos de IA concentrando-se exclusivamente em dados de alta qualidade.  
C) aumentar a capacidade dos modelos de IA generativa de produzir conteúdo visual, como imagens e vídeos, a partir de descrições textuais.  
D) integrar técnicas de aprendizado por reforço para permitir que modelos de IA generativa aprendam com seus próprios erros e sucessos durante a geração de texto.  
E) enriquecer os modelos de IA generativa com informações obtidas de bases de dados externas no momento da geração, elevando a precisão e a riqueza dos textos gerados.

**22.** (CEBRASPE – Câmara dos Deputados/2025 – Técnico Legislativo – Assistente Legislativo e Administrativo)

Acerca de inteligência artificial, técnicas de prompts, transformação e análise de dados, julgue o item seguinte.

No zero-shot prompting, é fornecido ao modelo apenas a instrução inicial, sem exemplos adicionais, a fim de se explorar a capacidade do modelo de fazer generalizações a partir do conhecimento obtido no treinamento.

( ) Certo ( ) Errado

**23.** (CEBRASPE – STM/2025 – Analista Judiciário – Análise de Sistemas)

Julgue o item seguinte, relativo a ciência de dados e inteligência artificial (IA).

Modelos de linguagem de grande escala, como os do tipo transformer, são treinados exclusivamente com base em regras sintáticas explícitas extraídas de dados linguísticos anotados manualmente, o que assegura maior controle semântico.

( ) Certo ( ) Errado

**24.** (FCC – TRT-6/2025 – Analista Judiciário – Tecnologia da Informação)

Em um pipeline MLOps, várias etapas são seguidas para o desenvolvimento, implementação e monitoramento de modelos de Machine Learning. Com relação a este processo, especialmente considerando o uso de redes neurais transformer e suas características, as fases e procedimentos técnicos envolvidos no ciclo completo de MLOps são assim descritos:

A) No pipeline MLOps, as redes neurais transformer são aplicadas na fase de Design para realizar Feature Engineering e tuning de parâmetros. Na fase de Operações, implementa-se o modelo em produção através de pipelines automatizados, e, por fim, a fase Experimentação e Desenvolvimento é dedicada ao monitoramento de dados de produção.  
B) No ciclo MLOps, os modelos transformer são implementados diretamente na fase de Operações para otimização em produção. A fase de Experimentação e Desenvolvimento é dedicada ao versionamento de dados e modelos e, na fase de Design, é realizado o monitoramento de desempenho do modelo e a coleta de dados para retreino futuro.  
C) O pipeline MLOps começa com a fase de Operações, na qual são definidos os requisitos técnicos e estes são alinhados com os requisitos de negócio. Em seguida, na fase de Experimentação e Desenvolvimento, o modelo é colocado em produção utilizando práticas de CI/CD. A etapa final, Design, foca na criação de embeddings e ajuste de hiperparâmetros.  
D) O ciclo de MLOps começa com a fase de Design, na qual são definidos os requisitos e alinhadas as necessidades de negócio. Na fase de Experimentação e Desenvolvimento, ocorre a construção do modelo, aplicação de Feature Engineering e tuning de parâmetros, podendo ser utilizadas redes neurais transformer para processar dados sequenciais com atenção contextual. Na etapa de Operações, o modelo é colocado em produção com pipelines de CI/CD e monitorado para ajuste contínuo.  
E) Durante a fase de Experimentação e Desenvolvimento, o alinhamento dos requisitos de negócio e os requisitos técnicos do modelo são realizados. Na fase de Design, faz-se o deploy do modelo com CI/CD, enquanto a fase de Operações se concentra em treinar modelos transformer com autoatenção para ajuste contextual.

**25.** (CEBRASPE – TJ-PA/2025 – Analista Judiciário – Análise de Sistemas)

A respeito da proteção de dados pessoais no Brasil, dos grandes modelos de linguagem (LLMs) e do uso de redes neurais no setor jurídico, julgue o item que se segue.

Os LLMs se destacaram recentemente em decorrência do aprimoramento das técnicas de treinamento com feedback humano; da maior acessibilidade via interfaces de uso simples como ChatGPT e Gemini; do avanço da potência computacional com GPUs; e da melhoria dos dados de treinamento.

( ) Certo ( ) Errado

---

### Gabarito comentado (concurso 16–25)

| Questão | Gabarito | Fonte/nota |
|---------|----------|------------|
| 16 | **C** | Quizinho FCC TRT-6 2025 Q57 — RLHF |
| 17 | **A** | LEVEL/Gran FCC MPE-SE 2026 — prompt engineering |
| 18 | **A** | LEVEL FCC MPE-SE 2026 — LLM com supervisão |
| 19 | **C** | Quizinho FCC TRF-4 2025 Q14 — riscos generativa |
| 20 | **D** | Quizinho FGV TCE-PA 2024 Q89 — RAG I e II |
| 21 | **E** | Quizinho FGV STN 2024 Q10 — RAG enriquece com bases externas |
| 22 | **Certo** | Quizinho CEBRASPE Câmara 2025 Q150 — zero-shot |
| 23 | **Errado** | Quizinho CEBRASPE STM 2025 Q111 — não é regra sintática manual |
| 24 | **D** | Quizinho FCC TRT-6 2025 Q55 — MLOps + transformer |
| 25 | **Certo** | Questionei/Magna + gabarito definitivo CEBRASPE TJ-PA 2025 (Análise de Sistemas) — LLMs / feedback humano / GPUs |

## 4. Como usar este arquivo

1. Ler §1 (cartão §1.10).  
2. Fechar treino 1–15 → conferir.  
3. Fazer 16–25 em modo prova.  
4. Anotar erros: prompt × fine-tuning × RAG × RLHF; alucinação; supervisão humana; riscos no Judiciário.  
5. Revisar `07` se misturar métricas/perceptron com generativa.

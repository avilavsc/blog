+++
title = '04 — MER e Normalização'
date = '2026-10-18'
draft = false
weight = 4
tags = ['semana-06', 'mer', 'normalizacao']
categories = ['estudo']
+++
# MER e Normalização · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — MER e normalização  
**Escopo deste arquivo:** Modelo Entidade-Relacionamento (conceitual), elementos cobráveis (entidade, atributo, relacionamento, cardinalidade, entidade fraca/forte, associativa, especialização), passagem para o modelo relacional e formas normais **1FN / 2FN / 3FN** (com visão leve de BCNF/4FN quando a banca cita).  
**Fora de escopo aqui:** SQL DML/DDL/DCL aprofundado (semana 2 · 05), Oracle 19c (05 desta semana), PostgreSQL 15–18 e migração (06), H2/JPA.

---

## 1. Material

### 1.1 Âncora de edital

No bloco **Banco de Dados** do edital: **MER; normalização**; depois SQL, transações, PL/SQL, PostgreSQL, Oracle 19c, H2. Em prova TRT/FCC, MER + normalização costumam vir como:

1. classificação de elementos no diagrama (entidade fraca × associativa × especialização);  
2. cardinalidade/grau do relacionamento;  
3. mapeamento N:M → tabela de ligação;  
4. definição precisa de 1FN / 2FN / 3FN (trocar “parcial” por “transitiva” é o clássico).

Flash: **MER modela o mundo; normalização limpa o esquema relacional**.

### 1.2 MER — o que a FCC cobra

**Modelo Entidade-Relacionamento (MER / ER)** descreve o domínio em nível **conceitual**: entidades (conjuntos de coisas), atributos (propriedades) e relacionamentos (associações). Não é SQL ainda; é o desenho antes das tabelas.

| Conceito | Ideia cobrável | Armadilha |
|----------|----------------|-----------|
| **Entidade** | Conjunto de objetos com identidade no domínio (Processo, Vara, Servidor) | Confundir com “tabela” já física |
| **Atributo** | Propriedade (simples, composto, multivalorado, derivado, identificador) | Multivalorado “dentro” da entidade ≠ 1FN |
| **Relacionamento** | Associação entre entidades; tem **grau** e **cardinalidade** | Chamar relacionamento de entidade sem critério |
| **Chave / identificador** | Atributo(s) que distinguem instâncias | Inventar chave composta sem necessidade |
| **Entidade fraca** | Identificação depende da entidade **proprietária/forte** | Confundir com associativa ou especialização |
| **Entidade associativa** | Relacionamento N:M “promovido” a entidade (com atributos próprios) | Usar associativa onde há só 1:N |
| **Especialização / generalização** | “é um tipo de” (herança conceitual) | Volume ≠ especialização de Processo |

#### Grau e cardinalidade

- **Grau:** unário (auto-relacionamento), binário (2 entidades), ternário (3)…  
- **Cardinalidade:** 1:1, 1:N (N:1), N:M — “quantos de um lado para quantos do outro”.  
- **Participação (visão):** total (obrigatória) × parcial (opcional) — aparece menos que cardinalidade, mas ajuda a ler enunciados (“podem ter ou não dependentes”).

Flash TRT: “um processo tem vários volumes; o número do volume só faz sentido **dentro** do processo” → **Volume = entidade fraca**, cardinalidade **N:1** em relação a Processo (muitos volumes para um processo).

#### Entidade fraca (ouro FCC)

A entidade fraca **não tem identificador suficiente sozinha**. Sua chave no modelo relacional costuma ser: **PK da forte + discriminador local**.

Exemplos de prova:

| Fraca | Proprietária | Discriminador típico |
|-------|--------------|----------------------|
| Dependente | Funcionário / Servidor | nº sequencial do dependente |
| Volume | Processo | nº do volume (01…nn) |
| ItemPedido | Pedido | nº do item |

**Não é** especialização (“Volume é um tipo de Processo” — falso).  
**Não é** associativa N:M só porque “há vários volumes”.

#### N:M → tabela de ligação

No relacional, N:M **não** se implementa colocando FK “nas duas pontas” reciprocamente. Cria-se **tabela associativa / de ligação** com as PKs das duas entidades como FKs (e, em geral, PK composta por elas). O N:M vira **dois 1:N**.

Se o relacionamento tiver atributo próprio (ex.: `cargo` do servidor em cada empresa terceirizada), esse atributo fica na **tabela de ligação**, não “perdido” em uma das pontas.

### 1.3 Do MER ao relacional (mapa mental)

| No MER | No relacional |
|--------|----------------|
| Entidade forte | Tabela; PK = identificador |
| Entidade fraca | Tabela; PK = (PK_forte, discriminador); FK para a forte |
| Relacionamento 1:N | FK do lado N apontando para o lado 1 |
| Relacionamento 1:1 | FK em um dos lados (ou união, conforme regra de negócio) |
| Relacionamento N:M | Tabela de ligação + dois 1:N |
| Atributo multivalorado | Em regra: nova tabela (ou estrutura normalizada) — não lista na célula |
| Atributo composto | Colunas separadas (ou tipo composto, se o SGBD permitir — em prova, pense em colunas) |

Flash: **MER decide o significado; normalização decide a qualidade do esquema relacional**.

### 1.4 Dependência funcional (pré-requisito das FNs)

**Dependência funcional** \(X \rightarrow Y\): conhecendo \(X\), determina-se \(Y\). Exemplos:

- `CodVara → NomeVara`  
- `(Matricula, CodDependente) → NomeDependente`  
- `CodCargo → NomeCargo` (se cargo for entidade/tabela própria)

Tipos que a FCC nomeia o tempo todo:

| Tipo | Significado | Forma que ataca |
|------|-------------|-----------------|
| **Parcial** | Atributo não-chave depende de **parte** da chave composta | **2FN** |
| **Total** | Depende da **chave inteira** | exigida na 2FN |
| **Transitiva** | Não-chave → outro não-chave (indireto via chave) | **3FN** |

Atributo **não-primo / não-chave:** não participa de nenhuma chave candidata (definição clássica em enunciados FCC antigos).

### 1.5 Formas normais — 1FN, 2FN, 3FN (núcleo)

#### 1FN — Primeira Forma Normal

Relação em **1FN** quando:

- valores são **atômicos** (sem listas/conjuntos na célula);  
- **sem grupos repetitivos** / tabelas aninhadas;  
- cada célula = um valor do domínio.

Em outras palavras: estrutura “plana” de tabela relacional. Em muitas definições modernas, **estar em 1FN já é parte da definição de relação** — a FCC ainda cobra a formulação explícita.

**Maior redundância típica** no discurso de prova: cenário ainda em **1FN** (ou pré-1FN) com dados repetidos e grupos — por isso questões do tipo “maior redundância / eliminação parcial / eliminação transitiva = **1FN, 2FN, 3FN**” na ordem certa.

Flash 1FN: **átomos, sem repetição aninhada**.

#### 2FN — Segunda Forma Normal

Está em **2FN** se e somente se:

1. está em **1FN**; e  
2. todo atributo **não-chave** depende da **totalidade** da chave primária (sem dependência **parcial**).

Se a chave for **simples** (um só atributo), não há espaço para dependência parcial → quem já está em 1FN com PK simples já satisfaz o “coração” da 2FN quanto a parciais.

Formulação FCC recente (TRT-12/2023, ideia): *não contém tabelas aninhadas nem atributos multivalorados **e** todos os atributos não-chave dependem da totalidade da chave*.

Armadilha: alternativa que descreve **só** a 1FN e omite a dependência total — parece certa, mas **não** é a 2FN completa.

Flash 2FN: **1FN + sem dependência parcial**.

#### 3FN — Terceira Forma Normal

Está em **3FN** se e somente se:

1. está em **2FN**; e  
2. não há dependência **transitiva** de atributo não-chave em relação à chave (não-chave não determina outro não-chave).

Exemplo clássico: `Pedido(CodPedido, CodCliente, NomeCliente, …)` com `CodPedido → CodCliente` e `CodCliente → NomeCliente` → `NomeCliente` depende transitivamente de `CodPedido`. Solução: extrair `Cliente(CodCliente, NomeCliente)`.

Flash 3FN: **2FN + sem transitiva**.

#### Cartão anti-confusão (decore)

| Forma | Elimina / exige | Frase de prova |
|-------|-----------------|----------------|
| 1FN | Multivalorados, aninhamentos, não-atômicos | “valores atômicos / sem grupos repetitivos” |
| 2FN | Dependência **parcial** | “não-chave depende da **chave inteira**” |
| 3FN | Dependência **transitiva** | “não-chave não depende de outro não-chave” |

Ordem FCC favorita (TRE-PI/2009, ideia): eliminação de parcial; estrutura com maior redundância; eliminação de transitiva → **2FN, 1FN, 3FN**.

### 1.6 BCNF e 4FN (visão de segurança)

- **BCNF (Boyce-Codd):** reforço da 3FN — todo determinante de dependência funcional não trivial deve ser **superchave**. Cobra-se menos que 3FN, mas aparece.  
- **4FN:** trata **dependência multivalorada** (ainda na 3FN/BCNF, mas com MVD). Em prova: “4FN quando não ocorre dependência multivalorada indevida”.  
- **5FN / PJ/NF:** raríssima; se cair, costuma ser definição de “não dá para decompor sem perda”.

Prioridade TRT4: **domine 1–3FN + entidade fraca + N:M**. BCNF/4FN = segunda linha.

### 1.7 Anomalias e por que normalizar

Sem normalização adequada:

| Anomalia | Exemplo mental |
|----------|----------------|
| **Inserção** | Não cadastrar cargo novo sem inventar um “funcionário fantasma” |
| **Alteração** | Mudar nome do departamento em N linhas e esquecer uma |
| **Exclusão** | Apagar último funcionário do setor e perder o setor |

Normalizar **reduz redundância controlada** e anomalias; denormalizar depois é decisão de desempenho — em prova de fundamentos, a resposta “certa” é a forma normal pedida, não o atalho.

### 1.8 Checklist rápido (caderno de erros)

- [ ] Entidade fraca ≠ associativa ≠ especialização  
- [ ] N:M → tabela de ligação (dois 1:N); atributo do relacionamento na ligação  
- [ ] 1FN = atômico / sem aninhamento; 2FN = + sem parcial; 3FN = + sem transitiva  
- [ ] Não trocar “parcial” ↔ “transitiva”  
- [ ] PK simples → parcial da 2FN some; transitiva da 3FN ainda pode existir  
- [ ] MER conceitual ≠ SQL físico (mas mapeia)

---

## 2. Questões de treino (1–15)

**1.** No MER, uma **entidade fraca** caracteriza-se, em regra, por:

A) ser sempre especialização total da entidade genérica  
B) implementar sozinha todo relacionamento N:M do modelo  
C) depender, para identificação, de uma entidade proprietária/forte  
D) equivaler a um auto-relacionamento unário  
E) proibir a existência de chave estrangeira no modelo relacional  

**2.** Volume identificado só junto com o número do processo (ex.: P01V01, P01V02) é, no MER:

A) especialização de Processo  
B) entidade associativa N:M com Processo  
C) auto-relacionamento N:M de Processo  
D) entidade fraca com cardinalidade N:1 em relação a Processo  
E) relacionamento complementar 1:1  

**3.** Relacionamento **binário 1:N** entre Tribunal e Funcionário (“um tribunal, muitos funcionários; cada funcionário em um tribunal”) implica, no relacional:

A) FK de Tribunal apontando para Funcionário em ambas as direções  
B) tabela de ligação obrigatória mesmo sem N:M  
C) chave de Tribunal composta por matrícula do funcionário  
D) proibição de entidade Dependente  
E) em regra, FK em Funcionário referenciando Tribunal  

**4.** Para implementar cardinalidade **N:M** entre Juiz e Assistente no modelo relacional, o caminho clássico é:

A) colocar a PK de cada um como FK na tabela do outro, reciprocamente, sem tabela extra  
B) forçar cardinalidade 1:1 entre as duas tabelas  
C) criar tabela de ligação, transformando N:M em dois relacionamentos 1:N  
D) unir Juiz e Assistente em uma única tabela sem chaves  
E) usar apenas índice bitmap, sem FK  

**5.** Atributo que pertence ao **relacionamento** N:M (ex.: cargo do servidor em cada empresa) deve, em regra, ficar:

A) só na tabela da empresa  
B) só na tabela do servidor  
C) em uma view materializada obrigatória  
D) na tabela de ligação  
E) no dicionário de dados, nunca em tabela  

**6.** A **1FN** exige, fundamentalmente:

A) eliminação de toda dependência transitiva  
B) que todo determinante seja superchave (BCNF)  
C) ausência de dependência multivalorada (4FN)  
D) chave estrangeira em todas as colunas  
E) valores atômicos e ausência de grupos repetitivos/tabelas aninhadas  

**7.** A **2FN** exige, além da 1FN:

A) apenas a existência de atributo multivalorado  
B) apenas a eliminação de dependências transitivas  
C) que atributos não-chave dependam da **totalidade** da chave (sem dependência parcial)  
D) proibição de chave composta  
E) equivalência com a 5FN  

**8.** A **3FN** caracteriza-se por:

A) permitir tabelas aninhadas desde que indexadas  
B) estar na 2FN e eliminar dependências transitivas entre atributos não-chave e a chave  
C) ser idêntica à 1FN por definição formal moderna sempre cobrada assim  
D) tratar somente dependências multivaloradas  
E) exigir denormalização obrigatória  

**9.** Dependência **parcial** é atacada prioritariamente pela:

A) 1FN  
B) 3FN  
C) 5FN  
D) 2FN  
E) apenas pela BCNF, nunca pela 2FN  

**10.** Dependência **transitiva** (não-chave determinando não-chave) é alvo clássico da:

A) 1FN  
B) 2FN  
C) apenas da 4FN  
D) normalização física de índices  
E) 3FN  

**11.** “Eliminação de dependência parcial; estrutura com maior redundância; eliminação de dependência transitiva” corresponde, respectivamente, a:

A) 3FN, 2FN, 1FN  
B) 3FN, 1FN, 2FN  
C) 1FN, 2FN, 3FN  
D) 2FN, 3FN, 1FN  
E) 2FN, 1FN, 3FN  

**12.** Uma relação com **chave primária simples** e já em 1FN:

A) ainda pode violar a 2FN por dependência parcial da chave composta inexistente  
B) está automaticamente em 5FN  
C) satisfaz automaticamente o aspecto “sem parcial” da 2FN; ainda pode violar a 3FN por transitiva  
D) não pode ter atributos não-chave  
E) está sempre denormalizada  

**13.** No mapeamento MER → relacional, entidade fraca Dependente de Funcionário tipicamente gera:

A) tabela sem qualquer referência a Funcionário  
B) apenas um atributo multivalorado na tabela Funcionário, mantido na 3FN  
C) especialização total com herança obrigatória no SQL padrão  
D) PK composta (PK_Funcionario, discriminador) e FK para Funcionário  
E) relacionamento ternário Tribunal–Funcionário–Dependente  

**14.** Qual afirmação **mistura** indevidamente os papéis?

A) 2FN elimina dependência parcial  
B) 3FN elimina dependência transitiva  
C) Entidade fraca depende da forte para identificação  
D) N:M vira tabela de ligação  
E) 1FN é definida como “todo determinante é superchave”  

**15.** Síntese correta deste subtópico:

A) MER = apenas SQL; normalização = só índices  
B) Entidade fraca = sempre N:M associativa  
C) 3FN prescinde da 2FN  
D) MER modela entidades/relacionamentos; 1FN/2FN/3FN removem, nesta ordem, não-atomicidade/parcial/transitiva  
E) Cardinalidade 1:N exige duas tabelas de ligação  

---

### Gabarito comentado (treino 1–15)

1. **C** — Fraca depende da proprietária para identificação.  
2. **D** — Padrão Volume/Processo (FCC TRT).  
3. **E** — FK no lado N.  
4. **C** — Tabela de ligação; dois 1:N.  
5. **D** — Atributo do relacionamento na ligação.  
6. **E** — Atomicidade / sem aninhamento.  
7. **C** — Dependência total da chave.  
8. **B** — 2FN + sem transitiva.  
9. **D** — Parcial → 2FN.  
10. **E** — Transitiva → 3FN.  
11. **E** — 2FN, 1FN, 3FN (clássico TRE-PI).  
12. **C** — Sem parcial possível; transitiva ainda pode.  
13. **D** — PK composta + FK.  
14. **E** — Isso é discurso de BCNF, não de 1FN.  
15. **D** — Síntese do arquivo.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** (FCC / TRT-12 · 2023 — adaptação) Uma tabela está na segunda forma normal (2FN) se e somente se:

A) não contém atributos multivalorados, mas contém tabelas aninhadas e atributos candidatos que dependem de somente uma parte da chave primária  
B) contém atributos multivalorados, mas não contém tabelas aninhadas e nem atributos candidatos que dependem de somente uma parte da chave primária  
C) contém tabelas aninhadas, mas não contém atributos multivalorados e nem atributos não-chave que dependem de somente uma parte da chave primária  
D) não contém tabelas aninhadas e nem atributos multivalorados  
E) não contém tabelas aninhadas nem atributos multivalorados e todos os seus atributos não-chave são dependentes da totalidade da chave primária  

**17.** (FCC / TRE-PI · 2009 — adaptação) No processo de normalização, a eliminação de dependência parcial da chave, a estrutura com maior redundância e a eliminação de dependência transitiva representam, respectivamente, as formas normais:

A) 3FN, 2FN e 1FN  
B) 3FN, 1FN e 2FN  
C) 1FN, 2FN e 3FN  
D) 2FN, 1FN e 3FN  
E) 2FN, 3FN e 1FN  

**18.** (FCC / TJ-PE · 2012 — adaptação) É correto que uma relação está na:

A) 2FN se, e somente se, todos os domínios básicos contiverem mais de um valor discreto periódico  
B) 2FN se, e somente se, estiver na primeira e todos os atributos não-chave forem totalmente dependentes da totalidade da chave primária  
C) 2FN se, e somente se, estiver na primeira e todos os atributos não-chave forem dependentes não transitivos da chave primária  
D) 3FN se, e somente se, ela estiver na segunda e todos os atributos não-chave contiverem mais de um valor discreto periódico  
E) 3FN se, e somente se, todos os domínios básicos forem multivalorados  

**19.** (FCC / TRT-20 · 2024 — adaptação) Considere que um processo trabalhista pode ser distribuído em diversos volumes identificados pelo número do processo principal; cada volume tem número sequencial 01…nn. Em um MER, Volume é:

A) uma especialização da entidade Processo, que é a entidade genérica  
B) uma entidade associativa com cardinalidade N:M em relação à entidade Processo  
C) uma entidade fraca com cardinalidade N:1 em relação à entidade Processo  
D) um auto-relacionamento com cardinalidade N:M  
E) um relacionamento complementar com cardinalidade 1:N em relação à entidade Processo  

**20.** (FCC / TRT-5 · 2022 — adaptação de ideia) Tribunal 1:N Funcionário; funcionário reconhecido por registro funcional; funcionários podem ter ou não dependentes. Assinale a leitura correta do MER:

A) relacionamento Tribunal–Funcionário binário, cardinalidade 1:N no sentido tribunal–funcionário, e Dependente como entidade fraca ligada a Funcionário  
B) identificador de Tribunal = código + registro funcional, e lista de dependentes como atributo multivalorado válido na 3FN  
C) relacionamento unário Tribunal–Funcionário e Dependente como entidade associativa  
D) relacionamento ternário por causa de Dependente  
E) Dependente como especialização total de Tribunal  

**21.** (FCC / ideia TRT — N:M) Se Juiz e Assistente se relacionam N:M, na criação das tabelas:

A) a PK de Juiz será FK em Assistente e a PK de Assistente será FK em Juiz, sem tabela extra  
B) estabelece-se relacionamento bidirecional 1:1 entre as tabelas  
C) as PKs de ambas devem ser compostas por IdJuiz+IdAssistente em cada tabela de ponta  
D) será necessário criar tabela de ligação, e o N:M dará lugar a dois relacionamentos 1:N  
E) cria-se tabela de ligação gerando dois relacionamentos 1:1  

**22.** Com respeito aos fundamentos relacionais (FCC / TRT-3 · 2005 — ideia), um esquema está na 2FN se:

A) qualquer atributo não-primo for total e funcionalmente dependente de qualquer chave do esquema  
B) estiver na 2FN e qualquer atributo não-primo for transitivamente independente de qualquer chave — como definição **só** de 2FN  
C) qualquer atributo não-primo for total e funcionalmente **independente** de qualquer chave  
D) qualquer atributo (inclusive chave) for funcionalmente independente de qualquer chave  
E) todos os domínios forem obrigatoriamente multivalorados  

**23.** Assinale a opção que melhor distingue 2FN e 3FN:

A) 2FN remove transitivas; 3FN remove parciais  
B) 2FN remove parciais (após 1FN); 3FN remove transitivas (após 2FN)  
C) ambas removem apenas atributos multivalorados  
D) 3FN é pré-requisito da 1FN  
E) 2FN exige BCNF completa  

**24.** No mapeamento de entidade fraca, é correto afirmar que:

A) em regra, incorpora a chave da forte (como parte da PK e como FK) mais um discriminador local  
B) sua identificação no relacional prescinde da chave da entidade forte  
C) equivale sempre a uma view sem materialização  
D) impede qualquer cardinalidade N:1 com a proprietária  
E) só existe no modelo físico Oracle, não no MER  

**25.** Para o eixo MER + normalização do edital TRT4/FCC, a síntese mais adequada é:

A) bastam índices B-tree; formas normais são irrelevantes  
B) toda entidade fraca é N:M  
C) classificar corretamente fraca/associativa/especialização e aplicar 1FN→2FN→3FN sem inverter parcial e transitiva  
D) 1FN já elimina dependências transitivas  
E) tabela de ligação é proibida em modelos judiciais  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **E** |
| 17 | **D** |
| 18 | **B** |
| 19 | **C** |
| 20 | **A** |
| 21 | **D** |
| 22 | **A** |
| 23 | **B** |
| 24 | **A** |
| 25 | **C** |

**Notas rápidas:** 16 E (2FN completa — TRT-12/2023); 17 D (2FN, 1FN, 3FN — TRE-PI/2009); 18 B (TJ-PE/2012); 19 C (Volume fraca — TRT-20/2024); 20 A (binário 1:N + Dependente fraca); 21 D (ligação); 22 A (definição clássica 2FN); 23 B (parcial × transitiva); 24 A (PK composta); 25 C (síntese).

---

*Arquivo 04 · MER e normalização · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo na trilha BD da semana: 05 Oracle 19c.*

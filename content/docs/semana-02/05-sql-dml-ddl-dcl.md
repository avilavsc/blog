+++
title = '05 — SQL DML/DDL/DCL'
date = '2026-10-24'
draft = false
weight = 5
tags = ['semana-02', 'sql']
categories = ['estudo']
+++
# SQL — DML, DDL, DCL + transações + noções PL/SQL · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — SQL (DML/DDL/DCL), gerência de transações e noções de PL/SQL  
**Escopo deste arquivo:** classificação dos comandos SQL; DDL / DML / DCL; noção de TCL (COMMIT/ROLLBACK/SAVEPOINT); propriedades ACID e ideia de isolamento; noções cobráveis de PL/SQL (blocos, procedures, functions, packages, triggers, cursores, exceções); armadilhas FCC (TRUNCATE × DELETE, GRANT × DML, PL/SQL × PL/pgSQL).  
**Fora de escopo aqui:** JPA/Hibernate/Envers (01–02); Flyway (03); detalhe de engine PostgreSQL/H2 e sintaxe avançada de índices/particionamento (04); MER/normalização profundos e Oracle 19c “produto” (semana 6); SSO/Keycloak/OAuth (06).

---

## 1. Material

### 1.1 Mapa mental: famílias de comandos SQL

O edital cobra **SQL (DML, DDL, DCL)** e, à parte, **gerência de transações** + **SQL e PL/SQL**. Em prova, a FCC testa sobretudo **classificar o comando** e **saber o efeito** (dados × estrutura × privilégios × transação).

| Família | Sigla | Objeto típico | Exemplos cobráveis |
|---------|-------|---------------|--------------------|
| **Data Definition Language** | **DDL** | **Estrutura** (objetos do schema) | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` |
| **Data Manipulation Language** | **DML** | **Dados** nas tabelas | `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `MERGE` |
| **Data Control Language** | **DCL** | **Privilégios / autorização** | `GRANT`, `REVOKE` |
| **Transaction Control Language** | **TCL** *(visão de prova; às vezes agrupada à “gerência de transações”)* | **Unidade de trabalho** | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

Flash de prova:

- **DDL** → define/muda/apaga **objetos** (tabela, view, índice, sequence…).  
- **DML** → lê/escreve **linhas**.  
- **DCL** → quem **pode** fazer o quê.  
- **TCL** → confirmar ou desfazer a **transação**.

Armadilha clássica: classificar `GRANT` como DML, ou `TRUNCATE` como “só DML porque apaga dados”. `TRUNCATE` é **DDL** na classificação usual de concurso (mesmo efeito prático de “esvaziar tabela”).

### 1.2 DDL — definir a estrutura

**DDL** cria e mantém o **schema**. Em muitos SGBDs (Oracle, PostgreSQL com comportamento típico de prova), comandos DDL **confirmam automaticamente** a transação corrente (*auto-commit* implícito) — nuance útil: após `CREATE`/`DROP` bem-sucedido, em geral **não dá** para `ROLLBACK` do DDL como se fosse um `UPDATE`.

| Comando | Ideia cobrável |
|---------|----------------|
| `CREATE` | Cria objeto (`TABLE`, `VIEW`, `INDEX`, `SEQUENCE`, `SCHEMA`…) |
| `ALTER` | Altera objeto existente (coluna, constraint, rename de coluna…) |
| `DROP` | Remove o objeto (e, tipicamente, os dados que dependem dele) |
| `TRUNCATE` | Remove **todas as linhas** de forma rápida; **estrutura permanece**; costuma ser **não rollbackável** como DML; classificado como **DDL** |
| `RENAME` | Renomeia objeto (quando o dialeto oferece o comando; senão via `ALTER`) |

Contraste **DROP TABLE** × **TRUNCATE** × **DELETE**:

| | Remove estrutura? | Remove linhas? | Família usual | Rollback típico (visão de prova) |
|---|-------------------|----------------|---------------|----------------------------------|
| `DROP TABLE` | Sim | Sim (junto com a tabela) | DDL | Em geral irrevogável após commit implícito do DDL |
| `TRUNCATE` | Não | Todas | DDL | Em geral **não** como DML |
| `DELETE` | Não | Selecionadas (ou todas, se sem `WHERE`) | DML | Sim, se ainda na mesma transação |

Constraint cobrável em DDL: `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `CHECK`, `NOT NULL` — criadas no `CREATE`/`ALTER`. Isso é **definição estrutural**, não manipulação de linha.

### 1.3 DML — manipular dados

**DML** opera sobre o **conteúdo**:

| Comando | Função |
|---------|--------|
| `SELECT` | Consulta (leitura). Em muitas classificações de concurso entra como DML; algumas bancas isolam “DQL”, mas no edital TRT4 o bloco é **DML/DDL/DCL** — trate `SELECT` como **DML/leitura** quando a alternativa for “manipulação/consulta de dados” |
| `INSERT` | Inclui linhas |
| `UPDATE` | Altera valores de colunas em linhas existentes |
| `DELETE` | Remove linhas (pode filtrar com `WHERE`) |
| `MERGE` | “Upsert” / sincronização condicional (INSERT ou UPDATE conforme critério) — cobrável como DML |

Pontos de prova:

- `DELETE` **sem** `WHERE` apaga **todas** as linhas, mas a tabela **continua existindo** (ainda é DML).  
- `UPDATE`/`DELETE` **com** `WHERE` inadequado = risco clássico de questão situacional.  
- `SELECT` **não altera** dados; se a alternativa disser que todo DML “sempre modifica” o banco, desconfie (por causa do `SELECT`).  
- Em Oracle/PostgreSQL de prova: DML participa de **transação**; só vira permanente após `COMMIT` (salvo autocommit da ferramenta/cliente).

### 1.4 DCL — controlar privilégios

**DCL** gerencia **autorização**:

| Comando | Função |
|---------|--------|
| `GRANT` | Concede privilégio (ex.: `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `ALL PRIVILEGES`) a usuário/role |
| `REVOKE` | Revoga privilégio previamente concedido |

Flash:

- `GRANT SELECT ON processo TO analista;` → **DCL**, não DML.  
- Conceder privilégio **não** é o mesmo que executar o `SELECT`.  
- Roles/usuários e privilégios de objeto são o coração do DCL em prova.

Não confundir com **autenticação de aplicação** (SSO/Keycloak — arquivo 06). DCL é **no SGBD**: quem pode ler/escrever/alterar objetos SQL.

### 1.5 Transações (gerência de transações / TCL)

**Transação** = unidade lógica de trabalho que leva o banco de um estado **consistente** a outro. Comandos típicos:

| Comando | Efeito |
|---------|--------|
| `BEGIN` / início implícito | Abre a unidade de trabalho (sintaxe varia: Oracle inicia com o 1º DML; PostgreSQL usa `BEGIN`) |
| `COMMIT` | **Confirma** permanentemente as alterações da transação |
| `ROLLBACK` | **Desfaz** as alterações ainda não confirmadas |
| `SAVEPOINT` | Marca um ponto intermediário; permite `ROLLBACK TO SAVEPOINT` parcial |

#### ACID (âncora obrigatória)

| Letra | Nome | Ideia de prova |
|-------|------|----------------|
| **A** | **Atomicidade** | Tudo ou nada: ou todos os efeitos da transação valem, ou nenhum |
| **C** | **Consistência** | Regras/constraints preservadas; estado válido → estado válido |
| **I** | **Isolamento** | Transações concorrentes não “se enxergam” indevidamente (graus variam) |
| **D** | **Durabilidade** | Após `COMMIT`, o efeito **persiste** mesmo com falha posterior do servidor (dentro das garantias do SGBD) |

Níveis de isolamento (visão — nomes cobráveis, sem virar DBA):

| Nível (ANSI) | Ideia |
|--------------|-------|
| Read Uncommitted | Pode ler dado ainda não commitado (dirty read) |
| Read Committed | Só lê commitado; default comum em PostgreSQL |
| Repeatable Read | Leituras repetidas estáveis na mesma transação (visão simplificada) |
| Serializable | Isolamento mais forte; serializa o efeito |

Fenômenos clássicos (saber o nome basta na maioria das questões): **dirty read**, **non-repeatable read**, **phantom read**.

Armadilhas:

- `COMMIT` **não** é DML nem DDL nem DCL — é **controle de transação**.  
- `ROLLBACK` após `COMMIT` **não** desfaz o que já foi confirmado.  
- DDL com auto-commit implícito “quebra” a expectativa de rollback de quem só pensou em DML.

### 1.6 Noções de PL/SQL (Oracle) — o que a FCC costuma cobrar

O edital lista **SQL e PL/SQL**. **PL/SQL** = extensão procedural da Oracle (*Procedural Language / Structured Query Language*). No PostgreSQL o análogo é **PL/pgSQL** — **não** são a mesma linguagem; em prova, se disser PL/SQL, pense **Oracle**.

#### Bloco anônimo

Estrutura canônica:

```text
DECLARE    -- opcional: variáveis, cursores, tipos
  ...
BEGIN      -- obrigatório: comandos executáveis
  ...
EXCEPTION  -- opcional: tratamento de erros
  ...
END;
```

Flash: **DECLARE / BEGIN / EXCEPTION / END** — bloco estruturado; SQL “puro” não tem essa casca procedural.

#### Unidades nomeadas (visão)

| Objeto | Ideia |
|--------|-------|
| **Procedure** | Rotina nomeada; executa ações; **não** retorna valor pelo mecanismo de função (pode ter parâmetros `OUT`/`IN OUT`) |
| **Function** | Rotina nomeada que **retorna** um valor; tipicamente usável em expressões SQL |
| **Package** | Agrupa procedures/functions/tipos relacionados (especificação + corpo) |
| **Trigger** | Dispara automaticamente em evento de tabela (`BEFORE`/`AFTER` + `INSERT`/`UPDATE`/`DELETE`) ou em certos eventos de sistema |
| **Cursor** | Mecanismo para percorrer resultado de consulta linha a linha no bloco procedural |
| **Exception** | Tratamento de erro (`NO_DATA_FOUND`, `TOO_MANY_ROWS`, `WHEN OTHERS`, exceções definidas pelo usuário) |

Pontos de prova:

- Trigger **não** se chama como procedure comum: **reage** a evento DML (ou DDL, em alguns casos).  
- Function **retorna** valor; procedure **executa** (e pode ter parâmetros de saída).  
- Package organiza e permite estado de sessão / encapsulamento (visão alta).  
- PL/SQL permite **variáveis**, **IF/LOOP**, **cursores** e **exceções** — por isso é “procedural”.  
- SQL puro (DML/DDL/DCL) **não** substitui PL/SQL: um é linguagem de consulta/definição/controle; o outro acrescenta **controle de fluxo** no servidor Oracle.

Relação com o restante do edital (sem aprofundar): em apps Java/JPA da PDPJ a lógica costuma ficar na aplicação; ainda assim o edital cobra **noções** de PL/SQL como competência de BD (Oracle no pacote de bancos).

### 1.7 Cartão rápido anti-pegadinha FCC

| Afirmação | Veredito |
|-----------|----------|
| `GRANT` é DML | **Falso** — DCL |
| `TRUNCATE` é DML igual a `DELETE` | **Falso** — classificação **DDL**; comportamento/transação distintos |
| `COMMIT` é DDL | **Falso** — TCL / gerência de transação |
| `SELECT` nunca entra em DML | Nuance: no edital o trio é DML/DDL/DCL; trate como manipulação/consulta de **dados** |
| Atomicidade = isolamento | **Falso** — letras diferentes do ACID |
| PL/SQL = PL/pgSQL | **Falso** — Oracle × PostgreSQL |
| Toda falha de disco após `COMMIT` apaga o efeito | Contradiz **durabilidade** (visão de prova) |
| `DELETE` remove a tabela | **Falso** — remove **linhas**; `DROP` remove objeto |

### 1.8 Encaixe no edital / PDPJ (leve)

No pack de persistência da semana 2: JPA/Hibernate falam com o banco via SQL sob o capô; Flyway versiona **DDL/DML** de migração; PostgreSQL/H2 são engines. Este arquivo é a **base SQL + transação + PL/SQL** que a FCC cobra no bloco **Banco de Dados**, independentemente do ORM.

---

## 2. Questões de treino (1–15)

**1.** Comando tipicamente classificado como **DDL**:

A) `UPDATE`  
B) `GRANT`  
C) `CREATE TABLE`  
D) `COMMIT`  
E) `SELECT`

**2.** Comando tipicamente classificado como **DCL**:

A) `ALTER TABLE`  
B) `REVOKE`  
C) `INSERT`  
D) `ROLLBACK`  
E) `TRUNCATE`

**3.** Diferença cobrável entre `DELETE` e `TRUNCATE`:

A) Ambos são DCL e removem privilégios  
B) `DELETE` é DML (pode filtrar linhas e participar de transação); `TRUNCATE` costuma ser tratado como DDL e esvazia a tabela sem a seletividade típica do `WHERE`  
C) `TRUNCATE` remove a estrutura da tabela; `DELETE` remove o schema inteiro  
D) São sinônimos rigorosos em qualquer SGBD e classificação  
E) `DELETE` é DDL; `TRUNCATE` é DCL

**4.** O comando `GRANT SELECT ON processo TO auditor` exemplifica:

A) DML  
B) DDL  
C) DCL  
D) Apenas TCL  
E) Exclusivamente PL/SQL

**5.** Acerca de transações, `COMMIT` e `ROLLBACK`:

A) `COMMIT` desfaz; `ROLLBACK` confirma  
B) Ambos são DDL  
C) `COMMIT` confirma as alterações da transação; `ROLLBACK` as desfaz (se ainda não confirmadas)  
D) Após `COMMIT`, `ROLLBACK` sempre desfaz o commit anterior  
E) São privilégios concedidos por `GRANT`

**6.** No acrônimo ACID, a **atomicidade** significa principalmente:

A) Leituras sujas sempre permitidas  
B) Tudo ou nada: a transação efetiva-se por completo ou não deixa efeito parcial  
C) Que o banco nunca tem índices  
D) Que `GRANT` é obrigatório a cada `INSERT`  
E) Que DDL e DML são a mesma família

**7.** A **durabilidade** (ACID) associa-se à ideia de que:

A) Dados não commitados são sempre visíveis a todos  
B) Após confirmação bem-sucedida, o efeito permanece apesar de falhas posteriores (garantias do SGBD)  
C) Transações concorrentes são impossíveis  
D) `ROLLBACK` confirma alterações  
E) Constraints não existem

**8.** Estrutura típica de um bloco PL/SQL inclui as seções:

A) Somente `GRANT` / `REVOKE`  
B) `DECLARE` (opcional), `BEGIN`, `EXCEPTION` (opcional), `END`  
C) Apenas `CREATE TABLE`  
D) Somente HTML e CSS  
E) Exclusivamente `MERGE` e `TRUNCATE`

**9.** Em PL/SQL, uma **function** difere de uma **procedure** principalmente porque:

A) Function é DCL; procedure é DDL  
B) Function tipicamente **retorna** um valor; procedure executa ações (podendo usar parâmetros de saída)  
C) Procedure sempre retorna exatamente um valor escalar obrigatório pelo mecanismo de função  
D) Ambas são sinônimos de `COMMIT`  
E) Function só existe em PostgreSQL com o nome PL/SQL

**10.** Um **trigger** em PL/SQL / BD Oracle, na visão de prova:

A) É chamado manualmente como único meio de executar `SELECT`  
B) Dispara automaticamente em resposta a eventos (ex.: `INSERT`/`UPDATE`/`DELETE` na tabela)  
C) É idêntico a `GRANT`  
D) Substitui o conceito de ACID  
E) É sempre classificado como DCL

**11.** Classifique corretamente: `ALTER TABLE servidor ADD COLUMN email VARCHAR(100);`

A) DML  
B) DCL  
C) DDL  
D) Apenas TCL  
E) PL/SQL package

**12.** O comando `SAVEPOINT ponto1` relaciona-se a:

A) Concessão de privilégios  
B) Criação de tabela  
C) Marcação intermediária na transação, permitindo rollback parcial  
D) Remoção definitiva do schema  
E) Autenticação SSO

**13.** Assinale a afirmação **incorreta**:

A) `INSERT` é DML  
B) `DROP` é DDL  
C) `REVOKE` é DCL  
D) `COMMIT` é DCL  
E) `UPDATE` é DML

**14.** PL/SQL e PL/pgSQL:

A) São nomes intercambiáveis e idênticos em qualquer questão  
B) PL/SQL associa-se ao ecossistema Oracle; PL/pgSQL ao PostgreSQL — linguagens procedurais distintas  
C) Ambos são apenas sinônimos de DCL  
D) PL/SQL proíbe blocos `BEGIN`/`END`  
E) PL/pgSQL é a única linguagem citada no edital sob o nome “PL/SQL”

**15.** Melhor síntese cobrável deste subtópico:

A) Só `SELECT` importa; DDL/DCL/transações/PL/SQL nunca caem  
B) DDL estrutura · DML dados · DCL privilégios · transações (ACID/COMMIT/ROLLBACK) · PL/SQL acrescenta procedural (blocos, rotinas, triggers, cursores, exceções)  
C) Todo comando SQL é DCL  
D) ACID elimina a necessidade de SQL  
E) Trigger = GRANT

---

### Gabarito comentado (treino 1–15)

1. **C** — `CREATE TABLE` define estrutura (DDL).  
2. **B** — `REVOKE` revoga privilégio (DCL).  
3. **B** — DML seletivo/transacional × DDL de esvaziamento típico.  
4. **C** — Concessão de privilégio = DCL.  
5. **C** — Confirmar × desfazer; rollback pós-commit não “desfaz o commit”.  
6. **B** — Tudo ou nada.  
7. **B** — Persistência após commit.  
8. **B** — Estrutura clássica do bloco.  
9. **B** — Retorno de valor × rotina de ação.  
10. **B** — Disparo automático por evento.  
11. **C** — `ALTER TABLE` = DDL.  
12. **C** — Ponto de salvamento na transação.  
13. **D** — `COMMIT` é TCL / controle de transação, não DCL.  
14. **B** — Oracle × PostgreSQL.  
15. **B** — Mapa completo do arquivo.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60)

Considere o seguinte caso:Utilizando comandos SQL, um analista criou uma tabela de cidadão dando permissão de acesso ao usuário Roberto. Posteriormente inseriu dados nessa tabela, mas logo em seguida teve que deletá-los, retirar a permissão de Roberto que estava saindo do tribunal e dar permissão a Carla.A sequência de comandos SQL utilizada no caso é, correta e respectivamente, categorizada como

A) DML - DCL - DDL - DDL - DCL - DCL.

B) DML - DCL - DML - DDL - DML - DCL.

C) DDL - DCL - DML - DML - DCL - DCL.

D) DDL - DCL - DDL - DML - DML - DCL.

E) DCL - DDL - DML - DML - DCL - DCL.


**17.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60)

Considere uma tabela denominada `Cidadao` e as colunas `Nome_Cidadao` e `Valor_Recebido`.Para obter a média dos valores maiores que 200 recebidos pelos cidadãos, um Analista deve utilizar o comando

A) `SELECT AVG(Valor_Recebido > 200) FROM Cidadao;`

B) `SELECT AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`

C) `SELECT AVG(Valor_Recebido) > 200 FROM Cidadao;`

D) `SELECT FROM Cidadao AVG(Valor_Recebido) > 200;`

E) `SELECT Nome_Cidadao and AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`


**18.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 23 de 60)

Considere a tabela `t_audiencias` que armazena informações sobre audiências marcadas.```CREATE TABLE t_audiencias ( id_audiencia INT PRIMARY KEY, numero_processo VARCHAR(50) NOT NULL, data_audiencia DATETIME NOT NULL, status VARCHAR(20) NOT NULL, vara_trabalho INT NOT NULL);```Considerando que esta tabela está criada e contém valores adequados, em um banco de dados MySQL aberto e funcionando em condições ideais, o comando que seleciona todas as audiências que estão marcadas como `'Pendente'`, cuja data de audiência seja posterior ao dia e hora atuais e que pertençam às varas de trabalho 1, 2 ou 3, é:`SELECT * FROM t_audiencias WHERE status = 'Pendente' AND`

A) `data_audiencia > CURRENT_DATE_TIME() AND vara_trabalho IN (1, 2, 3);`

B) `data_audiencia > NOW() AND vara_trabalho IN (1, 2, 3);`

C) `data_audiencia > SYSDATE() AND vara_trabalho BETWEEN 1..3;`

D) `data_audiencia > NOW() OR vara_trabalho = 1 OR vara_trabalho = 2 OR vara_trabalho = 3;`

E) `data_audiencia > CURDATE() AND vara_trabalho IN (1..3);`


**19.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 21 de 60)

Em um banco de dados Oracle 19c, aberto e funcionando em condições ideais, existe uma tabela tprocessos com dados válidos, que tem a seguinte estrutura: id_processo (número único de identificação do processo) data_abertura (data em que o processo foi aberto) id_vara (identificador da vara do trabalho onde o processo tramita) valor_processo (valor monetário associado ao processo) O comando SQL que lista o id_vara, o número total de processos por vara e o valor total dos processos, em ordem decrescente, somente das varas que possuem mais de 100 processos abertos antes de 1º de janeiro de 2020, é:

A) ```SELECT id_vara, COUNT() AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura < '2020-01-01'GROUP BY id_varaHAVING COUNT() > 100ORDER BY SUM(valor_processo) DESC;```

B) ```SELECT id_vara, COUNT() AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura < '2020-01-01'GROUP BY id_varaORDER BY SUM(valor_processo) DESCHAVING COUNT() > 100;```

C) ```SELECT id_vara, COUNT(id_processo) AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura < '2020-JAN-01'GROUP BY id_varaHAVING COUNT(id_processo) > 100ORDER BY COUNT(valor_total);```

D) ```SELECT id_vara, COUNT(*) AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura <= '2020-01-01'GROUP BY id_varaORDER BY valor_total;```

E) ```SELECT id_vara, COUNT(*) AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura < '2020-01-01'GROUP BY id_varaHAVING SUM(valor_processo) > 100000ORDER BY valor_total DESC;```


**20.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 22 de 60)

Considere que exista um banco de dados PostgreSQL aberto e funcionando em condições ideais, e que a tabela t_processos exista e esteja corretamente preenchida com valores adequados. Neste contexto, observe a função abaixo. CREATE FUNCTION atualizar_status_processo(num_processo VARCHAR) RETURNS INTEGER AS $$ DECLARE total INTEGER; BEGIN UPDATE t_processos SET status_processo = 'Encerrado' WHERE numero_processo = num_processo; SELECT COUNT(*) INTO total FROM t_processos WHERE status_processo = 'Encerrado'; RETURN total; END; $$ LANGUAGE plpgsql; Em condições ideais, esta função seria corretamente chamada utilizando-se o comando:

A) `CALL atualizar_status_processo('12345-67.2023.5.01.0001');`

B) `EXECUTE atualizar_status_processo('12345-67.2023.5.01.0001');`

C) DO $$ BEGIN PERFORM atualizar_status_processo('12345-67.2023.5.01.0001'); END; $$ LANGUAGE plpgsql;

D) `SELECT atualizar_status_processo('12345-67.2023.5.01.0001');`

E) `EXEC atualizar_status_processo '12345-67.2023.5.01.0001';`


**21.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 26 de 60)

Considere que um Analista está trabalhando em um banco de dados PostgreSQL aberto e funcionando em condições ideais. O Analista inseriu a seguinte sequência de comandos:```DROP TABLE I TRTContatos;CREATE TABLE TRTContatos ( cod serial PRIMARY KEY, orgao VARCHAR(255) NOT NULL, email VARCHAR(255) NOT NULL, descricao VARCHAR(255));INSERT INTO TRTContatos (orgao, email)VALUES ('Ouvidoria','ouvidoria@trt22.jus.br:'), ('Escola Judicial','ejud@trt22.jus.br'), ('Corregedoria','secor@trt22.jus.br')II ;```É correto afirmar que

A) haverá erro, pois não é possível fazer inserção de múltiplas linhas em tabelas do PostgreSQL.

B) haverá erro devido ao fato de as 3 linhas a serem inseridas na tabela TRTContatos não terem os 4 campos informados em CREATE TABLE.

C) I deve ser preenchido com IF EXISTS e II deve ser preenchido com RETURNING * para que as linhas inseridas sejam apresentadas.

D) I deve ser preenchido com ALL ROWS e II deve ficar sem nenhum preenchimento.

E) faltou um _ entre cod e serial, pois o certo seria cod_serial PRIMARY KEY.


**22.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 34 de 60)

No PostgreSQL 13 um trigger

A) permite chamar uma função se uma cláusula INSERT, UPDATE, DELETE ou TRUNCATE ocorrer em uma tabela.

B) pode modificar os dados na tabela virtual somente após a modificação ocorrer na tabela real.

C) deve ser único em uma tabela, ou seja, não pode haver mais de um trigger para a mesma tabela.

D) pode chamar funções que exibem dados, mas não funções para modificá-los.

E) pode ser acionado para uma tabela, mas não para uma view, chamando sempre uma procedure após o acionamento.


**23.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 56 de 60) *[aproximação temática]*

Dentre as técnicas para otimizar o desempenho de consultas que envolvem a análise de grandes volumes de dados geográficos no PostgreSQL 13, uma Analista está estudando aquela que é a mais adequada, recomendando:

A) utilizar a extensão PostGIS, para todas as operações espaciais.

B) utilizar índices B-tree em colunas de tipo geométrico.

C) fragmentar tabelas geográficas, com base em atributos espaciais.

D) criar materialized views, para pré-calcular resultados de consultas espaciais.

E) criar índices GIST em colunas de tipo geométrico.


**24.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60) *[aproximação temática]*

Uma equipe de desenvolvimento está utilizando Spring Boot para criar uma API RESTful. Durante os testes, observaram que a aplicação demora a inicializar, principalmente devido à configuração do banco de dados. Para melhorar o tempo de inicialização, decidiram desabilitar a criação automática do esquema de banco de dados ao iniciar a aplicação. A configuração, no `application.properties`, que desabilita a criação automática do esquema de banco de dados pelo Spring Boot é a

A) `spring.jpa.hibernate.ddl-auto=none`

B) `spring.datasource.database-auto=false`

C) `spring.jpa.db-sql=disable`

D) `spring.jpa.properties.hibernate.db-auto=disable`

E) `spring.jpa.create-database-init=false`


**25.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 39) *[aproximação temática]*

*Injeção SQL / DML inseguro.*

Um analista consultou a OWASP Top 10 (Riscos de Segurança em Aplicações Web) de 2021 para, se necessário, corrigir as vulnerabilidades apontadas no ambiente de um Tribunal. Referente ao número 1 da lista, uma das vulnerabilidades comuns é a

A) falta de restrições em métodos como POST, PUT e DELETE, permitindo ações não autorizadas.

B) ocorrência dos dados fornecidos pelo usuário não serem validados, filtrados ou sanitizados pela aplicação.

C) falta de proteção de segurança adequada em qualquer parte da stack da aplicação ou permissões configuradas incorretamente em serviços de nuvem.

D) falta de testes na compatibilidade de bibliotecas atualizadas, modificadas ou corrigidas.

E) existência de eventos auditáveis, como logins, tentativas de login malsucedidas e transações de alto valor, não são registrados em logs.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **C** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60 |
| 17 | **B** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 18 | **B** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 23 de 60 |
| 19 | **A** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 21 de 60 |
| 20 | **D** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 22 de 60 |
| 21 | **C** | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 26 de 60 |
| 22 | **A** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 34 de 60 |
| 23 | **E** \* | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 56 de 60 |
| 24 | **A** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 25 | **A** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 39 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q52
- Q17: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q53
- Q18: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q23
- Q19: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q21
- Q20: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q22
- Q21: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q26
- Q22: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q34
- Q23: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q56
- Q24: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q38
- Q25: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q39

*Arquivo 05 · SQL DML/DDL/DCL + transações + noções PL/SQL · Claudionir · TRT4 Analista TI (código 14) · FCC. Anterior: 04 PostgreSQL/H2 · Próximo: 06 SSO / Keycloak / OAuth2.*

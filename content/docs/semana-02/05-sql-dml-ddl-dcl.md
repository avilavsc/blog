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

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto de linguagem SQL, os comandos utilizados para definir ou modificar a estrutura de objetos de banco de dados, tais como tabelas e índices, pertencem tipicamente à categoria:

A) DML  
B) DCL  
C) DDL  
D) exclusivamente PL/SQL, sendo vedado o uso de SQL padrão  
E) apenas TCL, sem relação com schema

**17.** Constitui exemplo correto de comando da família DML:

A) `CREATE INDEX`  
B) `GRANT UPDATE`  
C) `UPDATE servidor SET lotacao = 'TI' WHERE id = 10`  
D) `REVOKE SELECT`  
E) `DROP VIEW`

**18.** Acerca do controle de privilégios em SQL, é correto afirmar que:

A) `GRANT` e `REVOKE` classificam-se como DCL e atuam sobre autorização de usuários/roles a objetos ou operações  
B) `GRANT` é DML porque altera linhas de tabelas de negócio  
C) `REVOKE` é DDL porque remove fisicamente a tabela  
D) privilégios SQL dispensam qualquer noção de usuário ou role  
E) DCL e DDL são termos rigorosamente sinônimos

**19.** Considere as afirmativas abaixo.

I. Uma transação deve apresentar comportamento “tudo ou nada”, propriedade conhecida como atomicidade.  
II. O comando `COMMIT` confirma as alterações efetuadas na transação corrente.  
III. O comando `TRUNCATE`, na classificação usual de concursos, é tratado como DML idêntico ao `DELETE` em família e em comportamento transacional.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III

**20.** No modelo ACID, a propriedade de isolamento refere-se, em essência, a:

A) garantir que, após o `COMMIT`, os dados possam ser descartados a qualquer momento sem impacto  
B) assegurar que a execução concorrente de transações não produza interferências indevidas entre elas, conforme o nível de isolamento adotado  
C) eliminar a necessidade de constraints  
D) transformar todo DML em DDL automaticamente  
E) conceder privilégios por meio de `INSERT`

**21.** Em relação a PL/SQL, no nível de noções cobráveis em concurso, assinale a opção correta.

A) PL/SQL é a linguagem procedural associada ao Oracle, permitindo blocos com declaração de variáveis, comandos executáveis e tratamento de exceções  
B) PL/SQL é estritamente idêntico ao SQL puro, sem qualquer extensão procedural  
C) Packages, procedures e functions são conceitos exclusivos de DCL  
D) Triggers substituem integralmente o papel do `GRANT`  
E) PL/SQL proíbe o uso de cursores

**22.** Assinale a opção que apresenta classificação **incorreta** do comando indicado.

A) `DELETE FROM agenda` — DML  
B) `ALTER TABLE agenda ADD CONSTRAINT ...` — DDL  
C) `COMMIT` — DCL  
D) `GRANT SELECT ON agenda TO leitura` — DCL  
E) `CREATE SEQUENCE` — DDL

**23.** Um analista precisa remover todas as linhas de uma tabela mantendo a estrutura, com comando frequentemente associado a operação DDL de esvaziamento rápido. A opção mais alinhada a essa descrição, na ótica típica de prova, é:

A) `REVOKE ALL`  
B) `TRUNCATE TABLE ...`  
C) `GRANT ALL`  
D) `SAVEPOINT`  
E) `CREATE SCHEMA`

**24.** Acerca de procedures e functions em PL/SQL, é correto concluir que:

A) ambas são indistinguíveis e nunca retornam valores de qualquer forma  
B) a function é concebida para retornar um valor, podendo ser utilizada em contextos de expressão; a procedure concentra-se na execução de um procedimento, com parâmetros conforme a assinatura  
C) procedure é sempre DCL e function é sempre DDL  
D) nenhuma delas pode constar de um package  
E) triggers são o único meio de declarar variáveis no Oracle

**25.** Para fins de preparação em Analista Judiciário – Área de Tecnologia da Informação (FCC / TRT4), a síntese adequada do bloco SQL DML/DDL/DCL + transações + noções PL/SQL é:

A) DDL define objetos · DML manipula dados · DCL gerencia privilégios · transações asseguram ACID via COMMIT/ROLLBACK/SAVEPOINT · PL/SQL acrescenta lógica procedural (blocos, rotinas, triggers, cursores, exceções) no ecossistema Oracle  
B) apenas autenticação Keycloak, sem SQL  
C) SQL resume-se a `SELECT`, sendo as demais famílias inexistentes em edital  
D) ACID e PL/SQL excluem-se mutuamente  
E) `GRANT` confirma transações e `COMMIT` concede privilégios

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **C** |
| 17 | **C** |
| 18 | **A** |
| 19 | **C** |
| 20 | **B** |
| 21 | **A** |
| 22 | **C** |
| 23 | **B** |
| 24 | **B** |
| 25 | **A** |

**Notas rápidas:** 16 C (DDL = estrutura); 17 C (`UPDATE` = DML); 18 A (`GRANT`/`REVOKE` = DCL); 19 C (I e II; III falsa — `TRUNCATE` ≠ DML idêntico a `DELETE`); 20 B (isolamento × concorrência); 21 A (PL/SQL Oracle + bloco); 22 C (`COMMIT` não é DCL); 23 B (`TRUNCATE`); 24 B (retorno × procedimento); 25 A (síntese do subtópico).

---

*Arquivo 05 · SQL DML/DDL/DCL + transações + noções PL/SQL · Claudionir · TRT4 Analista TI (código 14) · FCC. Anterior: 04 PostgreSQL/H2 · Próximo: 06 SSO / Keycloak / OAuth2.*

+++
title = '05 — Oracle 19c'
date = '2026-10-19'
draft = false
weight = 5
tags = ['semana-06', 'oracle', '19c']
categories = ['estudo']
+++
# Oracle 19c · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — Oracle 19c  
**Escopo deste arquivo:** Oracle Database **19c** no nível cobrável FCC/TRT — arquitetura mental (instância × banco, SGA/PGA, buffer cache), SQL/PL/SQL de prova (funções de string, DDL `ALTER TABLE`, agregação `GROUP BY`/`HAVING`/`ORDER BY`), parâmetros (`ALTER SYSTEM` × `ALTER DATABASE`, SPFILE/PFILE), contraste cobrável com PostgreSQL.  
**Fora de escopo aqui:** MER/normalização (04), PostgreSQL 15–18 e migração detalhada (06), administração RAC/Data Guard em profundidade, instalação Enterprise completa.

---

## 1. Material

### 1.1 Âncora de edital

O conteúdo de **Banco de Dados** cita explicitamente **Oracle 19c** ao lado de PostgreSQL 15–18, H2, MER, normalização, SQL e PL/SQL. Em provas recentes da FCC para TRT (Analista/Técnico TI), Oracle 19c aparece com enunciados do tipo:

- “em um banco de dados **Oracle 19c**, aberto e funcionando em condições ideais…”  
- comandos SQL/PL-SQL com tabela fictícia de vara/processo;  
- ajuste de **Buffer Cache** via parâmetro e **SPFILE**;  
- distinção fina de sintaxe (`REPLACE` × `SUBSTR`/`SUBST`, `ALTER TABLE ADD` × `UPDATE`/`MODIFY`).

Flash: **19c = long-term release** de referência no edital; cobram SQL + noção de memória/instância, não o manual inteiro de DBA.

### 1.2 Instância × banco (mapa mental)

| Peça | Ideia de prova |
|------|----------------|
| **Banco (database)** | Arquivos físicos: datafiles, control files, redo logs… |
| **Instância** | Processos + áreas de memória (**SGA**, **PGA**) que abrem/operam o banco |
| **SGA** | Memória compartilhada da instância (inclui **Database Buffer Cache**, Shared Pool, etc.) |
| **PGA** | Memória de processo/sessão (sorts, hash, variáveis de sessão — visão) |
| **SPFILE** | *Server Parameter File* binário — parâmetros persistentes da instância |
| **PFILE** | Arquivo texto clássico de parâmetros (init.ora) |

**Buffer Cache:** área da SGA que guarda **blocos de dados** lidos do disco para reuso (menos I/O físico). Parâmetro clássico do *default buffer pool*: **`DB_CACHE_SIZE`**.

Flash anti-confusão:

- Mudar tamanho de cache / parâmetros de instância → em regra **`ALTER SYSTEM`**, não `ALTER DATABASE`.  
- `ALTER DATABASE` → operações de **estrutura/estado do banco** (datafiles, open/mount etc.), não o nome “certo” para `DB_CACHE_SIZE`.

### 1.3 ALTER SYSTEM, SCOPE e SPFILE

Comando cobrado (FCC TRT-15/2023, ideia):

```sql
ALTER SYSTEM SET DB_CACHE_SIZE=2G SCOPE=SPFILE;
```

Leitura de prova:

| Elemento | Papel |
|----------|--------|
| `ALTER SYSTEM SET …` | Altera parâmetro de **instância/sistema** |
| `DB_CACHE_SIZE` | Tamanho do **default buffer cache** (bloco padrão) |
| `SCOPE=SPFILE` | Grava no SPFILE → vale nas **próximas** inicializações (sem efeito imediato só com SPFILE) |
| `SCOPE=MEMORY` | Só memória atual (some no restart) |
| `SCOPE=BOTH` | Memória **e** SPFILE (imediato + persistente), quando o parâmetro permitir |

Distratores clássicos:

| Errado | Por quê |
|--------|---------|
| `ALTER DATABASE SET DB_CACHE_SIZE…` | Comando errado para parâmetro de instância |
| `CACHE_SIZE` / `DATABASE_CACHE_SIZE` | Nome de parâmetro inventado/errado |
| `PARAM=SPFILE` | Cláusula inexistente; o correto é `SCOPE=` |

Flash: **`ALTER SYSTEM` + `DB_CACHE_SIZE` + `SCOPE=SPFILE`**.

### 1.4 SQL no Oracle 19c — o que a FCC realmente pergunta

#### Funções de string

| Função | Uso |
|--------|-----|
| **`REPLACE(expr, old, new)`** | Substitui ocorrências de `old` por `new` |
| **`SUBSTR(expr, pos [, len])`** | Extrai substring (não substitui) |
| `UPPER` / `LOWER` / `TRIM` / `LENGTH` | Manipulação clássica |

Exemplo de prova (TRT-22/2022):

```sql
SELECT REPLACE(Local, 'Teresina', 'TRT-22') FROM TRTServ;
```

Armadilha: **`SUBST`** não é a função de substituição; **`SUBSTR`** extrai, não troca.

#### DDL — adicionar colunas

Padrão mental Oracle:

```sql
ALTER TABLE TRTVaras ADD (
  NomeCidade VARCHAR2(50),
  Email      VARCHAR2(50) DEFAULT 'stp@trt22.jus.br'
);
```

Pontos cobráveis:

- **`ADD`** cria coluna; **`MODIFY`** altera definição de coluna existente; **`UPDATE`** muda dados, não estrutura.  
- Tipo texto clássico Oracle: **`VARCHAR2`**.  
- `DEFAULT` na definição da coluna nova é válido.  
- Distratores misturam `UPDATE … INCLUDE`, `ADD` com valor no lugar do tipo, `AND` fora da lista de colunas etc.

#### Agregação: WHERE × HAVING × ORDER BY

Ordem lógica cobrada:

1. `FROM` / `JOIN`  
2. `WHERE` — filtra **linhas** antes do grupo  
3. `GROUP BY`  
4. `HAVING` — filtra **grupos** (pode usar `COUNT`, `SUM`…)  
5. `SELECT`  
6. `ORDER BY` — por último  

Exemplo típico (FCC TRT — ideia 2025): listar `id_vara`, total de processos e soma de valores, só varas com **mais de 100** processos abertos **antes** de 01/01/2020, ordenando pelo valor total **desc**:

```sql
SELECT id_vara,
       COUNT(*) AS total_processos,
       SUM(valor_processo) AS valor_total
  FROM tprocessos
 WHERE data_abertura < DATE '2020-01-01'  -- ou literal aceito no enunciado
 GROUP BY id_vara
HAVING COUNT(*) > 100
 ORDER BY SUM(valor_processo) DESC;
```

Armadilhas:

- `HAVING` **antes** de `GROUP BY` / depois de `ORDER BY` na sintaxe apresentada;  
- filtrar agregação no `WHERE`;  
- `ORDER BY` usando alias inexistente no dialeto da alternativa;  
- omitir coluna não agregada do `GROUP BY`.

### 1.5 PL/SQL — visão de prova (não curso completo)

**PL/SQL** = extensão procedimental da Oracle (blocos, procedures, functions, packages, triggers, cursores, exceções).

Estrutura mental de bloco anônimo:

```text
DECLARE  -- opcional
  -- variáveis
BEGIN
  -- comandos
EXCEPTION
  -- tratamento
END;
```

Para o edital TRT4, misturado a “SQL e PL/SQL”:

- saber que PL/SQL **não** é o mesmo que SQL puro;  
- reconhecer `REPLACE`/`SELECT` em contexto “PL/SQL” quando a banca usa o rótulo de forma ampla;  
- contrastar com **PL/pgSQL** do PostgreSQL (outro dialeto, mesma *ideia* de linguagem procedimental embutida).

Flash: **Oracle → PL/SQL; PostgreSQL → PL/pgSQL** (nomes parecidos, produtos diferentes).

### 1.6 Oracle 19c × PostgreSQL (anti-confusão do edital)

O edital cobra **os dois**. Não precisa virar DBA dual; fixe o cartão:

| Tema | Oracle 19c | PostgreSQL (15–18) |
|------|------------|---------------------|
| Natureza no edital | SGBD comercial de referência citada | SGBD OSS de referência + PDPJ |
| Linguagem procedimental | PL/SQL | PL/pgSQL |
| String “trocar texto” | `REPLACE` | `replace` (função) |
| Texto variável típico | `VARCHAR2` | `varchar` / `text` |
| Memória / cache (prova) | SGA, `DB_CACHE_SIZE`, SPFILE | shared_buffers etc. (menos cobrado FCC TRT recente) |
| Upgrade major | path próprio Oracle | `pg_upgrade` / dump / logical (arquivo 06) |

Flash PDPJ: stack de desenvolvimento da plataforma → **PostgreSQL**; Oracle permanece **cobrável no bloco BD geral** do edital.

### 1.7 Outros ganchos 19c (segurança, sem virar manual)

Úteis se a banca citar “19c” sem pedir flag obscura:

- **Multitenant (CDB/PDB):** arquitetura de container — visão: banco raiz (CDB) hospeda PDBs; parâmetros de memória em PDB têm ressalvas (ex.: cuidado com `DB_CACHE_SIZE` em PDB vs `SGA_TARGET`).  
- **AWR / estatísticas / advisor** de buffer (`V$DB_CACHE_ADVICE`) — raro em Analista, mas “Buffer Cache = blocos quentes na SGA” basta.  
- **Undo / redo** — noção: redo protege mudanças; undo viabiliza consistent read / rollback (alto nível).  
- **Usuários/esquemas e privilégios** — encaixa em DCL (GRANT/REVOKE) já visto na semana 2.

Não decore centenas de *views* `V$`; decore **papéis** e **comandos** que já caíram.

### 1.8 Checklist rápido (caderno de erros)

- [ ] Instância (SGA/PGA) × banco (arquivos)  
- [ ] Buffer Cache ↔ `DB_CACHE_SIZE` ↔ `ALTER SYSTEM … SCOPE=SPFILE`  
- [ ] `ALTER SYSTEM` ≠ `ALTER DATABASE` para parâmetros de cache  
- [ ] `REPLACE` substitui; `SUBSTR` extrai; `SUBST` não é a função  
- [ ] `ALTER TABLE … ADD` / `MODIFY` / `UPDATE` — papéis distintos  
- [ ] `WHERE` (linha) → `GROUP BY` → `HAVING` (grupo) → `ORDER BY`  
- [ ] PL/SQL (Oracle) ≠ PL/pgSQL (Postgres); 19c ≠ “banco da PDPJ”

---

## 2. Questões de treino (1–15)

**1.** No Oracle, a área da SGA que mantém cópias de **blocos de dados** frequentemente acessados é o:

A) Redo Log Buffer exclusivo como sinônimo de datafile  
B) PGA de sistema operacional apenas  
C) Database Buffer Cache  
D) SPFILE em disco como área de sort  
E) Control file em memória compartilhada  

**2.** O parâmetro clássico do *default buffer cache* é:

A) `CACHE_SIZE`  
B) `DATABASE_CACHE_SIZE`  
C) `DB_CACHE_SIZE`  
D) `PGA_CACHE_SIZE`  
E) `REDO_CACHE_SIZE`  

**3.** Para persistir no SPFILE a alteração de `DB_CACHE_SIZE` para 2G, o comando adequado é:

A) `ALTER DATABASE SET DB_CACHE_SIZE=2G SCOPE=SPFILE;`  
B) `ALTER SYSTEM SET CACHE_SIZE=2G PARAM=SPFILE;`  
C) `ALTER DATABASE SET DATABASE_CACHE_SIZE=2G PARAM=SPFILE;`  
D) `UPDATE SYSTEM SET DB_CACHE_SIZE=2G;`  
E) `ALTER SYSTEM SET DB_CACHE_SIZE=2G SCOPE=SPFILE;`  

**4.** `SCOPE=SPFILE` em `ALTER SYSTEM SET` significa, em regra:

A) mudança só em memória, descartada no próximo start  
B) exclusão do PFILE  
C) gravação no SPFILE, valendo tipicamente após reinício da instância  
D) flush imediato obrigatório do redo  
E) conversão automática para PostgreSQL  

**5.** A função Oracle correta para **substituir** texto em uma coluna é:

A) `SUBST`  
B) `SUBSTR` (como sinônimo de substituição)  
C) `SWAPSTR`  
D) `REPLACE`  
E) `RENAME VALUE`  

**6.** `SUBSTR` serve, fundamentalmente, para:

A) substituir todas as ocorrências de uma string por outra  
B) criar coluna com DEFAULT  
C) extrair uma porção de uma string  
D) alterar `DB_CACHE_SIZE`  
E) abrir o banco em modo MOUNT  

**7.** Para **adicionar** colunas novas a uma tabela existente no Oracle, usa-se em regra:

A) `UPDATE TABLE … INCLUDE`  
B) `ALTER TABLE … MODIFY` sempre, mesmo para coluna inexistente  
C) `CREATE COLUMN`  
D) `INSERT TABLE ADD`  
E) `ALTER TABLE … ADD (…)`  

**8.** Filtrar grupos após agregação (`COUNT(*) > 100`) é papel típico de:

A) `WHERE`  
B) `SCOPE`  
C) `REPLACE`  
D) `HAVING`  
E) `PGA`  

**9.** Filtrar linhas **antes** do agrupamento (`data_abertura < data limite`) é papel de:

A) `HAVING`  
B) `ORDER BY`  
C) `WHERE`  
D) `SPFILE`  
E) `GROUP BY` exclusivo sem predicado  

**10.** A linguagem procedimental nativa associada ao Oracle no contraste cobrável com o PostgreSQL é:

A) PL/pgSQL  
B) T-SQL exclusivo  
C) Flyway DSL  
D) MapStruct  
E) PL/SQL  

**11.** Assinale a associação **correta**:

A) `ALTER SYSTEM` → datafile novo; `ALTER DATABASE` → `DB_CACHE_SIZE`  
B) Buffer Cache ⊂ apenas PGA  
C) SPFILE é área de sort da sessão  
D) `ALTER SYSTEM` → parâmetros de instância; Buffer Cache ⊂ SGA  
E) `REPLACE` extrai substring sem substituir  

**12.** Em `SELECT id_vara, COUNT(*), SUM(valor) … GROUP BY id_vara`, omitir `id_vara` do `GROUP BY` tende a:

A) gerar erro / ser inválido no padrão relacional cobrado  
B) ser sempre válido em qualquer SQL ANSI/Oracle  
C) aumentar automaticamente o Buffer Cache  
D) converter a consulta em PL/SQL package  
E) gravar o plano no SPFILE  

**13.** No contraste edital PDPJ × bloco BD geral:

A) Oracle 19c substitui obrigatoriamente o PostgreSQL na PDPJ  
B) Ambos são H2  
C) PostgreSQL é o SGBD de referência da persistência PDPJ; Oracle 19c permanece cobrável no edital de BD  
D) Oracle 19c é o Service Discovery  
E) SPFILE existe só no H2  

**14.** Qual opção **inverte** papéis?

A) `WHERE` filtra linhas; `HAVING` filtra grupos  
B) `REPLACE` substitui; `SUBSTR` extrai  
C) `ALTER SYSTEM` para `DB_CACHE_SIZE`; Buffer Cache na SGA  
D) `ADD` cria coluna; `UPDATE` altera dados  
E) `HAVING` filtra linhas antes do `FROM`; `WHERE` filtra grupos após `ORDER BY`  

**15.** Síntese correta deste subtópico:

A) Oracle 19c no edital = só instalação gráfica  
B) Oracle 19c elimina a necessidade de normalização  
C) `SUBST` é a função oficial de substituição  
D) Toda agregação usa apenas `WHERE`, nunca `HAVING`  
E) Cobrança típica: SQL/PL-SQL de prova + Buffer Cache/`DB_CACHE_SIZE`/`ALTER SYSTEM`/`SCOPE=SPFILE` + distinções DDL/DML  

---

### Gabarito comentado (treino 1–15)

1. **C** — Buffer Cache na SGA.  
2. **C** — `DB_CACHE_SIZE`.  
3. **E** — Comando clássico FCC.  
4. **C** — Persistência no SPFILE / próximo start.  
5. **D** — `REPLACE`.  
6. **C** — Extração.  
7. **E** — `ALTER TABLE … ADD`.  
8. **D** — `HAVING`.  
9. **C** — `WHERE`.  
10. **E** — PL/SQL.  
11. **D** — Sistema + SGA.  
12. **A** — Coluna não agregada deve estar no `GROUP BY`.  
13. **C** — Papéis distintos no edital.  
14. **E** — Única que inverte WHERE/HAVING.  
15. **E** — Síntese do arquivo.

---

## 3. Questões de concurso (16–25)

> Questões reais de provas (FCC). Gabarito só ao final.

**16.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Considere o seguinte trecho de código em PL/SQL: ```sql SELECT nome, FLOOR(MONTHS_BETWEEN(SYSDATE, data_admissao) / 12) AS anos_servico, MOD(MONTHS_BETWEEN(SYSDATE, data_admissao), 12) AS meses_servico FROM funcionarios WHERE status = 'Ativo'; ``` Ao ser executado em condições ideais, este comando

A) erra ao calcular o tempo de serviço, pois deveria ter sido usado `EXTRACT(YEAR FROM SYSDATE) - EXTRACT(YEAR FROM data_admissao)`, uma vez que `EXTRACT` já retorna a diferença correta de anos e meses.  
B) erra ao calcular o tempo de serviço, pois deveria ter sido usado `DATEDIFF(YEAR, data_admissao, SYSDATE)`, que retorna diretamente os anos completos.  
C) calcula o tempo de serviço dos funcionários ativos somando diretamente os meses obtidos pela função `MONTHS_BETWEEN`, sem necessidade de divisão ou modulação.  
D) calcula o tempo de serviço dos funcionários ativos, obtendo a diferença total de meses entre a data atual `SYSDATE` e `data_admissao`, convertendo os valores em anos e meses utilizando `FLOOR` e `MOD`.  
E) erra ao calcular o tempo de serviço, pois deveria ter sido utilizado apenas `ROUND(MONTHS_BETWEEN(SYSDATE, data_admissao) / 12)`, pois não há necessidade de calcular separadamente os anos e meses.  

**17.** (FCC – TRT-17/2022 – Analista Judiciário – TI)  
Em um banco de dados Oracle 19, funcionando em condições ideais, uma Analista deseja remover da tabela Funcionarios todos aqueles registros cujo campo `Data_Contratacao` seja 2 de janeiro de 2022 (dia-mês-ano). O comando PL/SQL correto para isso é: `DELETE FROM Funcionarios WHERE Data_Contratacao =`

A) CHECK_DATE('02-JAN-22','dd-mm-yy');  
B) VALIDATE('02-JAN-22','dd-mon-yy');  
C) TO_DATE('02-JAN-22','dd-mon-yy');  
D) "02-JAN-22";  
E) VAL_DATE('02-JAN-22','dd-mm-yy');  

**18.** (FCC – TRT-22/2022 – Analista Judiciário – TI)  
Considere que em um banco de dados Oracle 19c aberto e funcionando em condições ideais, exista a tabela TRTServ: Para atualizar e mostrar os termos Teresina para TRT-22 do campo Local, deve-se utilizar o seguinte comando PL/SQL:

A) SELECT FROM TRTServ, Local REPLACE ("Teresina", "TRT-22");  
B) SELECT REPLACE (Local, 'Teresina', 'TRT-22') FROM TRTServ;  
C) SELECT FROM TRTServ REPLACE (Local, "Teresina", "TRT-22");  
D) SELECT SUBST (Local, 'Teresina', 'TRT-22') FROM TRTServ;  
E) SELECT FROM TRTServ, Local SUBST ('Teresina', 'TRT-22');  

**19.** (FCC – TRT-22/2022 – Analista Judiciário – TI)  
Considere que em um banco de dados Oracle 19c, aberto e funcionando em condições ideais, exista a tabela TRTVaras: Para que as cidades cujos CodCidade comecem com 2 tenham a Jurisdicao alterada de Parnaíba para Teresina, deve-se utilizar o seguinte comando PL/SQL:

A) UPDATE TRTVaras SET Jurisdicao = 'Teresina' WHERE REGEXP_LIKE (CodCidade, '2%');  
B) UPDATE TRTVaras SET Jurisdicao = "Teresina" WHERE LIKE (CodCidade, "2%");  
C) UPDATE TRTVaras WHERE Jurisdicao = 'Parnaíba' SET INTO 'Teresina';  
D) UPDATE TRTVaras WHERE REGEXP_LIKE (CodCidade, '^2(*)' SET Jurisdicao = 'Teresina');  
E) UPDATE TRTVaras SET Jurisdicao = 'Teresina' WHERE REGEXP_LIKE (CodCidade, '^2(*)');  

**20.** (FCC – TRT-20/2024 – Analista Judiciário – TI)  
Um Analista de um Tribunal do trabalho realiza consultas de dados em movimento, como streams de dados em tempo real no Oracle 19c, usando a solução mais adequada que é o Oracle

A) Parse Streams.  
B) Database In-Memory.  
C) Advanced Queuing.  
D) Change Data Capture.  
E) Flashback.  

**21.** (FCC – TRT-6/2025 – Analista Judiciário – TI)  
Em um banco de dados Oracle aberto e em condições ideais, o comando utilizado para adicionar uma coluna custas do tipo NUMBER(10,2) à tabela processo, estabelecendo uma restrição que impede que valores de custas menores ou iguais a 10000 sejam inseridos, é

A) ADD COLUMN custas NUMBER(10,2) TO processo WHERE custas > 10000;  
B) ALTER TABLE processo ADD custas NUMBER(10,2) CONSTRAINT processo_custas_ck CHECK (custas > 10000);  
C) ADD COLUMN custas NUMBER(10,2) TO processo WITH CONSTRAINT processo_custas_ck CHECK (custas > 10000);  
D) ALTER TABLE processo ADD custas NUMBER(10,2) ADD CONSTRAINT processo_custas_ck CHECK (custas >= 10000);  
E) ALTER TABLE processo ADD custas NUMBER(10,2) CONSTRAINT processo_custas_ck WHERE (custas > 10000);  

**22.** (FCC – TRT-6/2025 – Analista Judiciário – TI)  
Considere que as tabelas, relacionamentos, atributos e restrições utilizadas no comando abaixo existem em um banco de dados Oracle aberto e em condições ideais. ```sql SELECT d.departamento_id, d.departamento_nome, d.local_id, COUNT(e.colaborador_id) FROM colaboradores e RIGHT OUTER JOIN departamentos d ON e.departamento_id = d.departamento_id GROUP BY d.departamento_id, d.departamento_nome, d.local_id; ``` Este comando

A) contará o número total de colaboradores e exibirá como se fosse de cada departamento, juntamente com o id, nome e local do departamento, pois há um erro na junção.  
B) exibirá o nome, local e número de colaboradores de cada departamento somente se a instrução RIGHT OUTER JOIN for trocada por LEFT OUTER JOIN.  
C) está incorreto, pois não existe a instrução RIGHT OUTER JOIN no Oracle, mas sim OUTER JOIN.  
D) exibirá o id, nome, local e número de colaboradores de cada departamento, incluindo aqueles sem colaboradores.  
E) exibirá o id, nome, local dos departamentos e o id dos colaboradores agrupando os colaboradores por departamento.  

**23.** (FCC – TRT-15/2023 – Analista Judiciário – TI)  
Um Analista possui uma tabela chamada `processo` em um banco de dados Oracle 19c aberto e em condições ideais, que possui os campos `num_processo` (chave primária) e `ano`. Para acelerar as consultas que buscam processos por ano, ele deseja criar um índice na coluna `ano`, denominado `index_ano_processo`, por meio do comando

A) CREATE INDEX index_ano_processo INTO processo ON COLUMN (ano);  
B) CREATE INDEX index_ano_processo REFERENCES processo (ano);  
C) CREATE INDEX index_ano_processo ON processo (ano);  
D) ALTER TABLE processo ADD INDEX index_ano_processo ON ano;  
E) ALTER TABLE processo ADD INDEX index_ano_processo COLUMN (ano);  

**24.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
Em uma aplicação Java que utiliza JPA, em condições ideais, considere `manager` um objeto válido criado por meio da instrução abaixo.

```java
EntityManager manager =
Persistence.createEntityManagerFactory("trtPU").createEntityManager();
```

A instrução `manager.createNamedQuery("Acesso.findByUserSenha", Acesso.class);` irá retornar um objeto do tipo

A) List<Acesso>  
B) Acesso  
C) Query<Acesso>  
D) TypedQuery<Acesso>  
E) ArrayList<Acesso>  

**25.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Um Analista de um Tribunal Regional do Trabalho foi solicitado a consultar os benefícios concedidos a funcionários de diferentes empresas e apresentar a quantidade total de benefícios pagos por empresa para aquelas que tenham concedido mais de 5 benefícios nos últimos 12 meses. Utilizando um banco de dados Oracle 19c, aberto e em condições ideais, o comando que faz a consulta é:

A) ``sqlSELECT empresa, COUNT() AS total_beneficiosFROM beneficiosWHERE data_concessao BETWEEN SYSDATE AND SYSDATE - 365GROUP BY empresaHAVING COUNT() > 5;``  
B) ``sqlSELECT empresa, COUNT(*) AS total_beneficiosFROM beneficiosWHERE data_concessao <= ADD_MONTHS(SYSDATE, -12)GROUP BY empresaHAVING COUNT( ) > 5;``  
C) ``sqlSELECT empresa, COUNT() AS total_beneficiosFROM beneficiosWHERE data_concessao >= ADD_MONTHS(TRUNC(SYSDATE), -12)GROUP BY empresaHAVING COUNT() > 5;``  
D) ``sqlSELECT empresa, COUNT() AS total_beneficiosFROM beneficiosWHERE EXTRACT(YEAR FROM data_concessao) := EXTRACT(YEAR FROM SYSDATE) - 1GROUP BY empresaHAVING COUNT() > 5;``  
E) ``sqlSELECT empresa, SUM(beneficio) AS total_beneficiosFROM beneficiosWHERE data_concessao >= ADD_MONTHS(SYSDATE, -12)GROUP BY empresaHAVING SUM(beneficio) > 5;``  

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **D** | FCC TRT-1/2025 Q38 — Banco de Dados |
| 17 | **C** | FCC TRT-17/2022 Q35 — Banco de Dados |
| 18 | **B** | FCC TRT-22/2022 Q23 — Banco de Dados |
| 19 | **E** | FCC TRT-22/2022 Q25 — Banco de Dados |
| 20 | **C** | FCC TRT-20/2024 Q57 — Banco de Dados |
| 21 | **B** | FCC TRT-6/2025 Q32 — SQL |
| 22 | **D** | FCC TRT-6/2025 Q36 — SQL |
| 23 | **C** | FCC TRT-15/2023 Q21 — SQL Oracle - Índices |
| 24 | **D** | FCC TRT-12/2023 Q34 — Banco de Dados |
| 25 | **C** | FCC TRT-1/2025 Q40 — Banco de Dados |

**Notas rápidas:** 16 D; 17 C; 18 B; 19 E; 20 C; 21 B; 22 D; 23 C; 24 D; 25 C.

---

*Arquivo 05 · Oracle 19c · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 06 PostgreSQL 15–18 e migração da 14.*

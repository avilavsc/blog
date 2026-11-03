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

## 3. Questões estilo concurso FCC (16–25)

**16.** (FCC / TRT-15 · 2023 — adaptação) No SQL*Plus, um Analista com as devidas permissões pretende ampliar o Buffer Cache do Oracle 19c para 2 gigabytes, persistindo a alteração no SPFILE. O comando adequado é:

A) `ALTER SYSTEM SET CACHE_SIZE=2G PARAM=SPFILE;`  
B) `ALTER DATABASE SET DB_CACHE_SIZE=2G SCOPE=SPFILE;`  
C) `ALTER SYSTEM SET DATABASE_CACHE_SIZE='2G' SCOPE='SPFILE';`  
D) `ALTER SYSTEM SET DB_CACHE_SIZE=2G SCOPE=SPFILE;`  
E) `ALTER DATABASE SET DATABASE_CACHE_SIZE=2G PARAM=SPFILE;`  

**17.** (FCC / TRT-22 · 2022 — adaptação) Em Oracle 19c, para exibir o campo `Local` trocando a substring Teresina por TRT-22, o comando adequado é:

A) `SELECT REPLACE (Local, 'Teresina', 'TRT-22') FROM TRTServ;`  
B) `SELECT FROM TRTServ, Local REPLACE ("Teresina", "TRT-22");`  
C) `SELECT FROM TRTServ REPLACE (Local, "Teresina", "TRT-22");`  
D) `SELECT SUBST (Local, 'Teresina', 'TRT-22') FROM TRTServ;`  
E) `SELECT FROM TRTServ, Local SUBST ('Teresina', 'TRT-22');`  

**18.** Acerca do Database Buffer Cache no Oracle 19c, assinale a correta.

A) É área típica da PGA usada só para ordenação de cursores privados  
B) Equivale ao arquivo SPFILE em disco  
C) Armazena blocos de dados frequentemente acessados na SGA, visando reduzir I/O físico  
D) Substitui integralmente os redo logs  
E) É configurado exclusivamente com `ALTER DATABASE RENAME FILE`  

**19.** Considere a necessidade de criar, em tabela já existente, as colunas `NomeCidade VARCHAR2(50)` e `Email VARCHAR2(50)` com default de e-mail institucional. O caminho DDL correto é do tipo:

A) `UPDATE … INCLUDE` das colunas  
B) `ALTER TABLE … MODIFY` de colunas que ainda não existem, com `AND` fora da lista  
C) `UPDATE … MODIFY` com valor default no lugar do tipo  
D) `ALTER TABLE … ADD` passando só o literal do e-mail no lugar da definição tipada  
E) `ALTER TABLE … ADD (NomeCidade VARCHAR2(50), Email VARCHAR2(50) DEFAULT '…')`  

**20.** Em consulta analítica sobre processos por vara, o predicado `COUNT(*) > 100` sobre o grupo deve aparecer, em regra, em:

A) `WHERE`  
B) `SCOPE`  
C) `FROM`  
D) `HAVING`  
E) `CONNECT BY` obrigatório  

**21.** Assinale a sequência sintática mais coerente para: filtrar processos abertos antes de certa data, agregar por vara, manter só varas com mais de 100 processos e ordenar pelo valor total decrescente.

A) `WHERE` → `GROUP BY` → `HAVING` → `ORDER BY`  
B) `ORDER BY` → `HAVING` → `GROUP BY` → `WHERE`  
C) `HAVING` → `WHERE` → `ORDER BY` → `GROUP BY`  
D) `GROUP BY` → `WHERE` → `ORDER BY` → `HAVING`  
E) `WHERE` → `ORDER BY` → `HAVING` → `GROUP BY`  

**22.** Relativamente a `REPLACE` e `SUBSTR` no Oracle:

A) ambas servem apenas para alterar `DB_CACHE_SIZE`  
B) `SUBSTR` é o nome oficial da substituição de strings  
C) `REPLACE` substitui ocorrências; `SUBSTR` extrai trecho — não são sinônimos  
D) `SUBST` é função documentada equivalente a `REPLACE`  
E) nenhuma delas pode aparecer em `SELECT`  

**23.** No contraste Oracle 19c × PostgreSQL no edital TRT4:

A) ambos são irrelevantes; só H2 conta  
B) Oracle 19c é o único SGBD citado no edital  
C) PostgreSQL não possui linguagem procedimental  
D) SPFILE é recurso exclusivo do PostgreSQL 18  
E) Oracle 19c figura no bloco BD; PostgreSQL também (e na PDPJ); PL/SQL ≠ PL/pgSQL  

**24.** `SCOPE=MEMORY` em `ALTER SYSTEM SET`, em visão de prova, indica:

A) alteração na memória da instância atual, sem persistir por si só no SPFILE  
B) gravação exclusiva no SPFILE sem tocar a instância atual  
C) remoção do Buffer Cache  
D) conversão de PFILE em tablespace  
E) comando inválido em qualquer versão 19c  

**25.** Para o subtópico Oracle 19c no TRT4/FCC, a preparação mais adequada prioriza:

A) memorizar todos os pacotes `DBMS_*` sem SQL  
B) apenas teoria de MER, ignorando comandos  
C) somente comparação de preços de licença  
D) SQL/PL-SQL de prova (funções, DDL, agregação), Buffer Cache/`DB_CACHE_SIZE`/`ALTER SYSTEM`/`SPFILE` e distinções anti-distrator  
E) substituição total do estudo de PostgreSQL  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **D** |
| 17 | **A** |
| 18 | **C** |
| 19 | **E** |
| 20 | **D** |
| 21 | **A** |
| 22 | **C** |
| 23 | **E** |
| 24 | **A** |
| 25 | **D** |

**Notas rápidas:** 16 D (TRT-15/2023 — `ALTER SYSTEM`+`DB_CACHE_SIZE`+`SCOPE=SPFILE`); 17 A (TRT-22/2022 — `REPLACE`); 18 C (SGA/blocos); 19 E (`ADD`); 20 D (`HAVING`); 21 A (ordem); 22 C (REPLACE×SUBSTR); 23 E (dual edital); 24 A (MEMORY); 25 D (síntese).

---

*Arquivo 05 · Oracle 19c · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 06 PostgreSQL 15–18 e migração da 14.*

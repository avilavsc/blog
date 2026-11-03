+++
title = '06 — PostgreSQL 15–18 e migração da 14'
date = '2026-10-20'
draft = false
weight = 6
tags = ['semana-06', 'postgresql', '15']
categories = ['estudo']
+++
# PostgreSQL 15–18 e migração da 14 · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — PostgreSQL majors 15 a 18 + noções de migração/compatibilidade com a 14  
**Escopo deste arquivo:** âncoras cobráveis das majors **15, 16, 17 e 18**; diferença **major × minor**; caminhos de migração a partir da **14** (`pg_upgrade`, dump/restore, replicação lógica); checklist de compatibilidade; encaixe com PDPJ/H2 sem repetir o arquivo da semana 2.  
**Fora de escopo aqui:** MER/normalização (04), Oracle 19c (05), SQL DML/DDL completo (semana 2 · 05), administração avançada de HA.

---

## 1. Material

### 1.1 Âncora de edital

Trecho programático: **PostgreSQL 15, 16, 17 e 18** (e **noções de migração/compatibilidade com a 14** em legados). Na PDPJ, PostgreSQL é o SGBD relacional de referência de produção; H2 fica em teste/dev (semana 2 · 04).

Leitura de prova:

1. saber **qual major** trouxe o quê (cartão 15→18);  
2. saber que **14 → 15/16/17/18** é **upgrade de major** (procedimento);  
3. não confundir major com *patch* minor (`14.x → 14.y`).

Flash: **conhecer a escada 15–18 + como sair da 14**.

### 1.2 Versionamento mental

| Tipo | Exemplo | Exige `pg_upgrade` / dump major? |
|------|---------|----------------------------------|
| **Major** | 14 → 15; 15 → 16; 14 → 18 | **Sim** (um dos métodos de major) |
| **Minor / patch** | 16.1 → 16.4 | Em regra **não** — atualização da mesma série |

Pode-se subir **direto** 14 → 18 (não é obrigatório instalar 15, 16 e 17 no meio). Ainda assim, a documentação manda **ler as notas de migração** de cada major intermediária (comportamentos que quebram app).

A major **14** entra em fim de suporte comunitário em **novembro/2026** — dado de calendário útil para entender “legado” no edital (prova em jan/2027).

### 1.3 Cartão das majors 15 → 18

#### 15 (out/2022)

- Comando SQL padrão **`MERGE`** (ajuste condicional insert/update/delete em um statement; próximo de *upsert* em lote).  
- Publicação seletiva na **replicação lógica** (lista de colunas / filtro de linhas).  
- Compressão **zstd** (incl. visão em `pg_basebackup`).  
- Logs de servidor em formato **JSON** estruturado.  
- Melhorias de *sorting*.

Flash 15: **`MERGE` + logical publication seletiva**.

#### 16 (set/2023)

- Paralelismo ampliado (ex. *hash joins* `FULL` / *outer*).  
- **Replicação lógica a partir de standby**.  
- Aplicação paralela de grandes transações no *subscriber*.  
- Visão **`pg_stat_io`**.  
- Avanços **SQL/JSON**; regex em `pg_hba.conf` / `pg_ident.conf` (visão admin).

Flash 16: **logical replication / standby + observabilidade I/O + SQL/JSON**.

#### 17 (set/2024)

- Novo gerenciamento de memória do **`VACUUM`**.  
- **`JSON_TABLE()`** + construtores/funções SQL/JSON (`JSON()`, `JSON_EXISTS()`, `JSON_QUERY()`, `JSON_VALUE()`…).  
- `MERGE` enriquecido: `RETURNING`, `merge_action()`, `WHEN NOT MATCHED BY SOURCE`, views atualizáveis (visão).  
- Replicação lógica com controles de failover / utilitários; `pg_upgrade` passa a preservar melhor estado de replicação lógica (a partir desta linha).  
- **`pg_basebackup` incremental**; `COPY … ON_ERROR ignore`.

Flash 17: **`JSON_TABLE` + VACUUM novo + backup incremental**.

#### 18 (set/2025)

- Subsistema de **I/O assíncrono (AIO)** — scans sequenciais, bitmap heap, vacuum etc.; parâmetro mental `io_method` (`worker` / `io_uring` / `sync`).  
- `pg_upgrade` **retém estatísticas** do otimizador (menos “plano frio” pós-upgrade).  
- *Skip scan* em índices B-tree multicoluna.  
- **`uuidv7()`** (UUID ordenável no tempo); `uuidv4()` como alias explícito.  
- Colunas geradas **virtuais** (cálculo na leitura; padrão novo).  
- Autenticação **OAuth** (`pg_hba` método `oauth`).  
- `RETURNING` com **`OLD` / `NEW`** em DML/`MERGE`.  
- **Constraints temporais** (ranges): `WITHOUT OVERLAPS` / `PERIOD` em PK/UK/FK.

Flash 18: **AIO + stats no upgrade + uuidv7 + OAuth + temporais**.

#### Tabela-resumo

| Major | Uma frase de prova |
|-------|-------------------|
| 15 | Entra o `MERGE` SQL padrão |
| 16 | Logical replication / standby e SQL/JSON avançam |
| 17 | Manutenção (`VACUUM`) + `JSON_TABLE` + backup incremental |
| 18 | AIO, upgrade com stats, uuidv7, OAuth, constraints temporais |

### 1.4 Migração / compatibilidade a partir da 14

O edital pede **noções**, não runbook de DBA. Fixe três caminhos e o checklist.

#### Três caminhos clássicos

| Método | Ideia | Quando pensar |
|--------|-------|----------------|
| **`pg_upgrade`** | Recria catálogos do sistema; **reaproveita** arquivos de dados do usuário quando compatível — tipicamente **mais rápido** que dump completo | Janela curta; clusters no mesmo host/storage |
| **Dump/restore** (`pg_dump` / `pg_dumpall` + restore) | Exporta lógica/dados e recria no cluster novo | Simples de raciocinar; janela maior; bom para “limpar” legado |
| **Replicação lógica** | Replica para cluster novo e faz *cutover* | Menos downtime (visão de prova) |

#### Ritual mental do `pg_upgrade`

1. Instalar **binários da major nova** ao lado da antiga.  
2. `initdb` de um cluster novo **compatível** (atenção a checksums na 18: default pode diferir da 14 — alinhar com `--no-data-checksums` se o antigo não usava).  
3. Rodar **`pg_upgrade --check`** (não altera dados; pode rodar com o antigo no ar, com portas distintas).  
4. Corrigir extensões faltantes / incompatibilidades apontadas.  
5. Parar servidores conforme o procedimento e executar o upgrade real.  
6. Analisar (*ANALYZE*) / validar apps — na 18, stats tendem a **sobreviver** melhor ao `pg_upgrade`.

Pontos de ouro:

- Major **≠** minor.  
- H2 **não** migra cluster PostgreSQL.  
- Extensões, collations, slots de replicação lógica e *breaking changes* das release notes entram no **plano de compatibilidade**.  
- Slots lógicos: preservação plena no `pg_upgrade` é linha **recente** (a partir da 17 no discurso de release); sair da **14** exige atenção — em prova: “planejar replicação/slots”, não decorar cada flag.

Flash migração: **14 → N via `pg_upgrade` *ou* dump/restore *ou* logical; sempre revisar incompatibilidade**.

### 1.5 Compatibilidade de aplicação (o que quebra)

Além do binário, a FCC pode cobrar a **ideia** de que apps/scripts precisam ser revisados:

| Área | Exemplo mental |
|------|----------------|
| SQL / funções | Mudança de comportamento em edge cases; novos reserved words |
| Extensões | Binário da extensão precisa existir na major nova |
| Auth / `pg_hba` | Novos métodos (ex. OAuth na 18) ≠ remoção dos antigos |
| Drivers JDBC/ODBC | Versão do driver alinhada ao servidor |
| ORM / Flyway | Scripts testados no **Postgres alvo**, não só no H2 |

Flash PDPJ: teste no H2 **não** prova migração 14→18.

### 1.6 Encaixe com o restante do estudo

| Tema | Relação |
|------|---------|
| Semana 2 · 04 (PG+H2) | Papéis produção × teste; este arquivo **aprofunda** 15–18 e migração |
| Semana 2 · 05 (SQL) | `MERGE`/`JSON_TABLE` são SQL — caem na fronteira BD |
| Arquivo 05 (Oracle 19c) | Contraste de produto; upgrade Oracle ≠ `pg_upgrade` |
| Flyway / JPA | Schema versionado deve rodar no Postgres **alvo** pós-migração |

### 1.7 Checklist rápido (caderno de erros)

- [ ] 15=`MERGE`; 16=logical/standby+SQL/JSON; 17=`JSON_TABLE`+VACUUM+backup incr.; 18=AIO+uuidv7+OAuth+temporais+stats no upgrade  
- [ ] Major exige procedimento; minor da mesma série, em regra, não  
- [ ] 14→18 pode ser direto; ler notas 15–18 mesmo assim  
- [ ] Caminhos: `pg_upgrade` / dump-restore / logical  
- [ ] `--check` antes do upgrade real  
- [ ] H2 ≠ caminho de migração de cluster

---

## 2. Questões de treino (1–15)

**1.** O edital de Analista TI cita explicitamente as majors PostgreSQL:

A) apenas 9.6 e 10  
B) somente a 18, sem legado  
C) 15, 16, 17 e 18, com noções de migração/compatibilidade envolvendo a 14  
D) apenas H2 2.x  
E) somente Oracle 19c renomeado  

**2.** Passar de PostgreSQL **14.2** para **14.5** caracteriza, em regra:

A) upgrade de major com `pg_upgrade` obrigatório  
B) migração obrigatória via H2  
C) conversão automática para Oracle 19c  
D) atualização *minor*/patch da mesma série, sem o rito típico de major  
E) perda obrigatória de todos os data files  

**3.** Âncora típica da major **15**:

A) remoção total do SQL  
B) fim da replicação lógica  
C) obrigatoriedade do H2 em produção  
D) exclusividade de `JSON_TABLE` só nesta major  
E) introdução ampla do comando **`MERGE`** no PostgreSQL  

**4.** Âncora típica da major **16**:

A) proibição de `MERGE`  
B) substituição do Postgres pelo MapStruct  
C) avanços em **replicação lógica** (incl. a partir de standby) e SQL/JSON / observabilidade  
D) remoção de `pg_upgrade`  
E) apenas mudança de logotipo  

**5.** Na major **17**, associação cobrável inclui:

A) `JSON_TABLE` / SQL/JSON ampliado e melhorias fortes de `VACUUM` / backup incremental  
B) apenas retorno à 9.6  
C) H2 como motor de vacuum  
D) fim do `COPY`  
E) OAuth como único método de auth, removendo os demais  

**6.** Na major **18**, âncoras de prova incluem:

A) somente remoção do I/O  
B) exclusividade do banco H2  
C) proibição de índices B-tree  
D) AIO, retenção de estatísticas no `pg_upgrade`, `uuidv7()`, OAuth, constraints temporais (visão)  
E) fim de `RETURNING`  

**7.** Sobre subir de 14 para 18:

A) é obrigatório instalar e rodar 15, depois 16, depois 17, nesta ordem, no mesmo cluster  
B) o projeto permite upgrade direto 14→18, ainda recomendando ler notas das majors intermediárias  
C) só é possível via Oracle GoldenGate  
D) basta trocar o número na documentação  
E) só o H2 Console executa o upgrade  

**8.** `pg_upgrade` caracteriza-se por:

A) ser o nome do H2 Console  
B) documentar OpenAPI  
C) registrar microsserviços no Eureka  
D) gerar DTOs em tempo de compilação  
E) permitir upgrade major reaproveitando arquivos de dados do usuário (quando compatível), tipicamente mais rápido que dump completo  

**9.** Antes do upgrade real, a prática cobrável é:

A) apagar o cluster antigo sem checagem  
B) renomear apenas o SPFILE do Oracle  
C) converter todas as tabelas para H2  
D) executar `pg_upgrade --check` e corrigir incompatibilidades apontadas  
E) desabilitar SQL permanentemente  

**10.** Dump/restore como caminho de major:

A) é inválido entre majors  
B) serve apenas entre minors  
C) exporta e recria no cluster novo — caminho clássico, em geral com janela maior que `pg_upgrade`  
D) substitui o conceito de normalização  
E) é sinônimo de AIO  

**11.** Replicação lógica na migração major serve, em visão de prova, para:

A) documentar Swagger  
B) eliminar a necessidade de backups para sempre  
C) substituir o VACUUM  
D) alimentar cluster novo e reduzir downtime no *cutover*  
E) obrigar uso exclusivo de Oracle 19c  

**12.** Afirmar que “teste passando no H2 garante migração 14→18 sem revisão” é:

A) sempre verdadeiro  
B) verdadeiro se houver Flyway  
C) verdadeiro se o profile for `prod`  
D) verdadeiro porque H2 recompila o núcleo do Postgres  
E) falso: H2 não valida o cluster Postgres nem paridade total de dialeto  

**13.** Qual par major→âncora está **correto**?

A) 15 → só AIO; 18 → só `MERGE`  
B) 16 → fim da replicação lógica  
C) 15 → `MERGE`; 17 → `JSON_TABLE`; 18 → AIO/`uuidv7`  
D) 17 → remoção de JSON  
E) 18 → obrigatoriedade do H2  

**14.** Qual afirmação sobre estatísticas pós-`pg_upgrade` na linha da **18** é a mais adequada?

A) a 18 tornou impossível qualquer upgrade  
B) estatísticas só existem no Oracle SPFILE  
C) `ANALYZE` foi removido do produto  
D) a 18 destaca retenção de estatísticas do otimizador no `pg_upgrade`, reduzindo o “plano frio”  
E) stats são gravadas apenas no H2  

**15.** Síntese correta deste subtópico:

A) majors 15–18 são irrelevantes; migração da 14 é feita pelo Zuul  
B) minor e major exigem exatamente o mesmo rito sempre  
C) `JSON_TABLE` existe só no H2  
D) OAuth na 18 elimina PostgreSQL da PDPJ  
E) conhecer âncoras 15–18 e migrar 14→N via `pg_upgrade`/dump/logical com revisão de compatibilidade  

---

### Gabarito comentado (treino 1–15)

1. **C** — Texto do edital.  
2. **D** — Minor ≠ major.  
3. **E** — `MERGE` na 15.  
4. **C** — Logical/SQL/JSON na 16.  
5. **A** — `JSON_TABLE`/VACUUM/backup incr.  
6. **D** — Pacote 18.  
7. **B** — Direto 14→18 + ler notas.  
8. **E** — Reaproveita data files.  
9. **D** — `--check` primeiro.  
10. **C** — Dump/restore clássico.  
11. **D** — Cutover com menos downtime.  
12. **E** — H2 não valida Postgres.  
13. **C** — Cartão 15/17/18.  
14. **D** — Stats no upgrade (18).  
15. **E** — Síntese.

---

## 3. Questões de concurso (16–25)

> Questões reais de provas (FCC). Gabarito só ao final.

**16.** (FCC – TRT-20/2024 – Analista Judiciário – TI)  
Dentre as técnicas para otimizar o desempenho de consultas que envolvem a análise de grandes volumes de dados geográficos no PostgreSQL 13, uma Analista está estudando aquela que é a mais adequada, recomendando:

A) utilizar a extensão PostGIS, para todas as operações espaciais.  
B) utilizar índices B-tree em colunas de tipo geométrico.  
C) fragmentar tabelas geográficas, com base em atributos espaciais.  
D) criar materialized views, para pré-calcular resultados de consultas espaciais.  
E) criar índices GIST em colunas de tipo geométrico.  

**17.** (FCC – TRT-17/2022 – Analista Judiciário – TI)  
Considere que em um banco de dados PostgreSQL 13, aberto e funcionando em condições ideais, existam duas tabelas: Cidades (com os campos NomeCidade e Estado) e Populacao (com os campos Cidade, NumHabitantes, NumVaras). A tabela Cidades foi criada pelo comando: `CREATE TABLE Cidades (NomeCidade VARCHAR(80) PRIMARY KEY, Estado VARCHAR(40));` Para que, ao se inserir ou atualizar algum registro na tabela Populacao, seja verificado se o campo Cidade corresponde a uma cidade existente na tabela Cidades, deve-se utilizar uma chave estrangeira. Isso é feito utilizando o comando SQL na criação da tabela Populacao: CREATE TABLE Populacao (Cidade VARCHAR(80) I , NumHabitantes INT, NumVaras INT); A lacuna I é corretamente preenchida com

A) FOREIGN KEY (Cidade) REFERENCES Cidades(NomeCidade)  
B) FOREIGN KEY REFERENCES Cidades(NomeCidade)  
C) REFERENCES Cidades(NomeCidade)  
D) FOREIGN KEY Cidades(NomeCidade)  
E) REFERENCES FOREIGN KEY (Cidade) TO Cidades(NomeCidade)  

**18.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Um Tribunal Regional do Trabalho deseja consultar a quantidade de audiências realizadas nos últimos 3 meses para o tipo de processo trabalhista `'RESCISÃO CONTRATUAL'`. Em um banco de dados PostgreSQL, aberto e funcionando em condições ideais, a consulta é obtida usando-se o comando:

A) ``sqlSELECT COUNT(*)FROM audienciasWHERE tipo_processo = 'RESCISAO CONTRATUAL'AND data_audiencia >= MAKE_INTERVAL(months := 3);``  
B) ``sqlSELECT COUNT(*)FROM audienciasWHERE tipo_processo = 'RESCISAO CONTRATUAL'AND data_audiencia >= CURRENT_DATE - INTERVAL '3 MONTH';``  
C) ``sqlSELECT COUNT(*) AS totalFROM audienciasWHERE tipo_processo = 'RESCISAO CONTRATUAL'AND data_audiencia >= CURRENT_DATE - '90 DAYS';``  
D) ``sqlSELECT COUNT(*)FROM audienciasWHERE tipo_processo = 'RESCISAO CONTRATUAL'AND data_audiencia >= NOWDATE() - INTERVAL '3 MONTH';``  
E) ``sqlSELECT COUNT(*)FROM audienciasWHERE tipo_processo = 'RESCISAO CONTRATUAL'AND data_audiencia >= DATE_TRUNC('MONTH', NOW()) - '3 MONTHS';``  

**19.** (FCC – TRT-22/2022 – Analista Judiciário – TI)  
Considere que um Analista está trabalhando em um banco de dados PostgreSQL aberto e funcionando em condições ideais. O Analista inseriu a seguinte sequência de comandos: ``` DROP TABLE I TRTContatos; CREATE TABLE TRTContatos ( cod serial PRIMARY KEY, orgao VARCHAR(255) NOT NULL, email VARCHAR(255) NOT NULL, descricao VARCHAR(255) ); INSERT INTO TRTContatos (orgao, email) VALUES ('Ouvidoria','ouvidoria@trt22.jus.br:'), ('Escola Judicial','ejud@trt22.jus.br'), ('Corregedoria','secor@trt22.jus.br') II ; ``` É correto afirmar que

A) haverá erro, pois não é possível fazer inserção de múltiplas linhas em tabelas do PostgreSQL.  
B) haverá erro devido ao fato de as 3 linhas a serem inseridas na tabela TRTContatos não terem os 4 campos informados em CREATE TABLE.  
C) I deve ser preenchido com IF EXISTS e II deve ser preenchido com RETURNING * para que as linhas inseridas sejam apresentadas.  
D) I deve ser preenchido com ALL ROWS e II deve ficar sem nenhum preenchimento.  
E) faltou um _ entre cod e serial, pois o certo seria cod_serial PRIMARY KEY.  

**20.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
Considere o seguinte caso:

 Utilizando comandos SQL, um analista criou uma tabela de cidadão dando permissão de acesso ao usuário Roberto. Posteriormente inseriu dados nessa tabela, mas logo em seguida teve que deletá-los, retirar a permissão de Roberto que estava saindo do tribunal e dar permissão a Carla. 

A sequência de comandos SQL utilizada no caso é, correta e respectivamente, categorizada como

A) DML - DCL - DDL - DDL - DCL - DCL.  
B) DML - DCL - DML - DDL - DML - DCL.  
C) DDL - DCL - DML - DML - DCL - DCL.  
D) DDL - DCL - DDL - DML - DML - DCL.  
E) DCL - DDL - DML - DML - DCL - DCL.  

**21.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
Considere uma tabela denominada `Cidadao` e as colunas `Nome_Cidadao` e `Valor_Recebido`.

Para obter a média dos valores maiores que 200 recebidos pelos cidadãos, um Analista deve utilizar o comando

A) SELECT AVG(Valor_Recebido > 200) FROM Cidadao;  
B) SELECT AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;  
C) SELECT AVG(Valor_Recebido) > 200 FROM Cidadao;  
D) SELECT FROM Cidadao AVG(Valor_Recebido) > 200;  
E) SELECT Nome_Cidadao and AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;  

**22.** (FCC – TRT-20/2024 – Analista Judiciário – TI)  
Considere que um processo trabalhista pode ser distribuído em diversos volumes que são identificados pelo número do processo principal a que se referem. Cada volume tem um número que vai de 01 a nn, sequencialmente, na medida em que são anexados ao processo principal, como por exemplo: P01V01, P01V02, P02V01, P02V02 e P02V03, onde P é o número do processo e V é o número do volume. Em um modelo conceitual entidade-relacionamento, Volume é

A) uma especialização da entidade Processo, que é a entidade genérica.  
B) uma entidade associativa com cardinalidade n:m em relação à entidade Processo.  
C) uma entidade fraca com cardinalidade n:1 em relação à entidade Processo.  
D) um auto relacionamento com cardinalidade n:m.  
E) um relacionamento complementar com cardinalidade 1:n em relação à entidade Processo.  

**23.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Considere a classe abaixo em uma aplicação que utiliza JPA e Hibernate.

```
@Entity
public class Cliente {
 @Id
 @GeneratedValue(strategy = GenerationType.
 I
 )
 private Long id;
 // atributos e métodos restantes
}
```

Para informar ao provedor de persistência que os valores a serem atribuídos ao identificador único (id) serão gerados pela coluna de autoincremento da tabela do banco de dados, utiliza-se, na lacuna I , o valor

A) TABLE  
B) AUTO_GENERATED  
C) AUTO_INCREMENT  
D) IDENTITY  
E) SELF_GENERATED  

**24.** (FCC – TRT-6/2025 – Analista Judiciário – TI)  
Uma equipe de Analistas, considerando as características específicas de cada modelo, optou pelo uso do banco de dados NoSQL orientado a

A) grafos, pois, além de implementar algoritmos como Depth-First Search (DFS) e Breadth-First Search (BFS), que otimizam consultas complexas, também pode fazer uma prevenção sofisticada contra fraudes.  
B) documentos, pois organiza dados em tabelas semelhantes ao modelo relacional, mas armazenando-as de forma independente, o que permite maior compressão e desempenho em operações de leitura.  
C) grafos, pois estrutura dados em coleções, com suporte a esquemas flexíveis, ideais para o armazenamento de objetos JSON ou BSON.  
D) colunas, que armazena dados em pares de chave-valor, o que permite uma estrutura altamente flexível para armazenar diferentes tipos de dados estruturados e semiestruturados.  
E) documentos, pois é ideal para o armazenamento de séries temporais, otimizando a leitura sequencial de grandes volumes de dados, como eventos de sensores.  

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
| 16 | **E** | FCC TRT-20/2024 Q56 — Banco de Dados |
| 17 | **C** | FCC TRT-17/2022 Q36 — Banco de Dados |
| 18 | **B** | FCC TRT-1/2025 Q39 — Banco de Dados |
| 19 | **C** | FCC TRT-22/2022 Q26 — Banco de Dados |
| 20 | **C** | FCC TRT-12/2023 Q52 — Banco de Dados |
| 21 | **B** | FCC TRT-12/2023 Q53 — Banco de Dados |
| 22 | **C** | FCC TRT-20/2024 Q60 — Banco de Dados |
| 23 | **D** | FCC TRT-19/2022 Q33 — Banco de Dados |
| 24 | **A** | FCC TRT-6/2025 Q54 — NoSQL |
| 25 | **C** | FCC TRT-1/2025 Q40 — Banco de Dados |

**Notas rápidas:** 16 E; 17 C; 18 B; 19 C; 20 C; 21 B; 22 C; 23 D; 24 A; 25 C.

---

*Arquivo 06 · PostgreSQL 15–18 e migração da 14 · Claudionir · TRT4 Analista TI (código 14) · FCC.*

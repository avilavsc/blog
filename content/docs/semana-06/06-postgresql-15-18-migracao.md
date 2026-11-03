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

## 3. Questões estilo concurso FCC (16–25)

**16.** No conteúdo programático de Banco de Dados do edital TRT4 (FCC), relativamente ao PostgreSQL, é correto afirmar que:

A) citam-se apenas versões anteriores à 12, sem menção a migração  
B) citam-se as majors 15 a 18, bem como noções de migração/compatibilidade com a 14 em legados  
C) o PostgreSQL foi substituído integralmente pelo H2 em todos os eixos  
D) a única major cobrada é a 19c, rótulo exclusivo Oracle  
E) migração entre majors é proibida pelo edital  

**17.** Considere as afirmações sobre evolução do PostgreSQL.

I. A major 15 introduziu suporte amplo ao comando SQL `MERGE`.  
II. A major 17 destacou `JSON_TABLE` e melhorias relevantes de manutenção (`VACUUM`) / backup incremental.  
III. A major 18 trouxe, entre outras âncoras, subsistema de I/O assíncrono e `uuidv7()`.

Está correto o que se afirma em:

A) I, apenas  
B) II e III, apenas  
C) I e II, apenas  
D) I, II e III  
E) III, apenas  

**18.** A passagem de um cluster PostgreSQL da major 14 para uma major posterior (15–18) relaciona-se, em visão de concurso, a:

A) procedimento de *upgrade* de major — por exemplo `pg_upgrade`, dump/restore ou estratégias com replicação lógica — com verificação de incompatibilidades  
B) exclusividade do banco H2 em produção após a migração  
C) proibição de uso de SQL após a 14  
D) função exclusiva do Hibernate Envers como motor de upgrade do binário  
E) substituição do PostgreSQL pelo MapStruct  

**19.** Assinale a opção que melhor descreve o papel de `pg_upgrade --check`:

A) apaga definitivamente o cluster antigo  
B) converte automaticamente Oracle 19c em PostgreSQL  
C) executa verificação de compatibilidade **sem** alterar os dados, permitindo corrigir problemas antes do upgrade real  
D) documenta a API OpenAPI do microsserviço  
E) substitui o `ANALYZE` após a 18  

**20.** Sobre atualização **minor** versus **major** no PostgreSQL:

A) 14.1 → 14.6 exige, em regra, o mesmo rito completo de 14 → 18  
B) majors (ex.: 14 → 16) exigem migração; minors da mesma série, em regra, não usam `pg_upgrade`  
C) minors sempre exigem dump/restore completo entre quaisquer patches  
D) majors nunca precisam de revisão de *breaking changes*  
E) H2 define oficialmente o que é major no PostgreSQL  

**21.** Qual associação major → âncora está correta?

A) 15 → somente remoção do SQL; 16 → fim da replicação; 17 → proibição de JSON; 18 → obrigatoriedade do H2  
B) 18 → apenas retorno à 9.6  
C) 15 → `MERGE`; 16 → avanços em replicação lógica/SQL/JSON; 17 → `JSON_TABLE`/manutenção; 18 → AIO/`uuidv7`/OAuth (visão)  
D) 15 a 18 são *minors* da série 14  
E) Todas as majors listadas existem apenas no dialeto H2  

**22.** Em cenário de migração 14 → 18 com baixa tolerância a downtime, além de `pg_upgrade` e dump/restore, a estratégia que a literatura de prova costuma citar é:

A) reescrita completa da aplicação em Delphi obrigatória  
B) uso exclusivo de SPFILE Oracle  
C) replicação lógica para um cluster novo seguida de *cutover*  
D) remoção de todas as FKs antes do dump  
E) conversão de todas as tabelas em entidades fracas  

**23.** Acerca de compatibilidade pós-migração major, assinale a correta.

A) extensões, collations, drivers e scripts SQL/ORM devem ser revisados; passar no H2 não basta  
B) nenhuma extensão precisa existir na major nova  
C) o edital proíbe leitura das release notes  
D) `MODE=PostgreSQL` do H2 executa `pg_upgrade`  
E) estatísticas do otimizador são irrelevantes após qualquer upgrade  

**24.** No contraste com Oracle 19c (mesmo eixo BD do edital):

A) `pg_upgrade` é o comando Oracle para Buffer Cache  
B) ambos os produtos são cobráveis; caminhos de upgrade e dialetos (PL/pgSQL × PL/SQL) não se confundem  
C) PostgreSQL 18 elimina o Oracle do conteúdo programático  
D) SPFILE é parâmetro do `pg_hba.conf`  
E) `DB_CACHE_SIZE` é âncora da major 15 do Postgres  

**25.** Para fins de preparação no eixo PostgreSQL 15–18 + migração da 14, a síntese mais adequada é:

A) H2 e PostgreSQL são o mesmo artefato; majors 15–18 são irrelevantes  
B) Rate limiting de API Gateway só existe dentro do Postgres 14  
C) `pg_upgrade` aplica-se apenas entre *minors* e nunca entre *majors*  
D) OAuth na 18 substitui o papel do Keycloak em toda a PDPJ por definição do edital  
E) conhecer o mapa 15–18, distinguir major/minor e dominar os três caminhos clássicos de migração com checklist de compatibilidade  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **B** |
| 17 | **D** |
| 18 | **A** |
| 19 | **C** |
| 20 | **B** |
| 21 | **C** |
| 22 | **C** |
| 23 | **A** |
| 24 | **B** |
| 25 | **E** |

**Notas rápidas:** 16 B (edital 15–18 + 14); 17 D (I–III); 18 A (major upgrade); 19 C (`--check`); 20 B (major≠minor); 21 C (mapa); 22 C (logical cutover); 23 A (compatibilidade); 24 B (dual edital); 25 E (síntese).

---

*Arquivo 06 · PostgreSQL 15–18 e migração da 14 · Claudionir · TRT4 Analista TI (código 14) · FCC.*

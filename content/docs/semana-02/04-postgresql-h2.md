+++
title = '04 — PostgreSQL + H2'
date = '2026-10-23'
draft = false
weight = 4
tags = ['semana-02', 'postgresql', 'h2']
categories = ['estudo']
+++
# PostgreSQL + H2 · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — PostgreSQL e H2  
**Escopo deste arquivo:** papel do PostgreSQL na PDPJ-Br, papel do H2 em testes/desenvolvimento, diferenças cobráveis entre os dois, visão das versões 15–18 e noções de migração/compatibilidade a partir da 14.  
**Fora de escopo aqui:** JPA/Hibernate/Envers em profundidade (01–02), Flyway aprofundado (03), SQL DML/DDL/DCL + PL/SQL (05), Oracle 19c comparativo (semana 6).

---

## 1. Material

### 1.1 Âncora de edital (por que este duo aparece)

No eixo **PDPJ-Br / persistência**, o conteúdo programático cita explicitamente **PostgreSQL** e **H2**. No eixo geral de **Banco de Dados**, o edital ainda cobra **PostgreSQL 15, 16, 17 e 18** e **noções de migração/compatibilidade com a 14** em legados, além do **H2 Database**.

Leitura de prova:

| Banco | Papel típico no discurso FCC/PDPJ |
|-------|-----------------------------------|
| **PostgreSQL** | SGBD relacional de **produção / ambiente real** dos serviços |
| **H2** | Banco **leve / embutido** para **teste e desenvolvimento** |

Flash: **PostgreSQL carrega o dado “de verdade”; H2 acelera o ciclo de teste/dev**. Não inverta.

### 1.2 PostgreSQL na PDPJ-Br

**PostgreSQL** é o banco relacional de referência da arquitetura de desenvolvimento da PDPJ-Br (junto com o duo de persistência JPA/Hibernate/Flyway). Em prova, associe-o a:

1. **Persistência relacional** dos microsserviços / módulos da plataforma.  
2. **Ambiente operacional** (homologação/produção e stacks reais), não como “banco de brinquedo”.  
3. **SQL completo** + recursos de SGBD “de verdade” (transações ACID, índices, tipos, extensões, replicação etc. — sem virar manual de DBA aqui).  
4. **Coexistência** com outros armazenamentos quando o domínio exigir (ex.: grafo, cache, busca) — mas o **relacional padrão citado no edital PDPJ** é PostgreSQL.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Natureza | SGBD relacional *open source* de produção |
| Lugar na PDPJ | Persistência “oficial” dos serviços alinhados ao stack |
| Contraste | ≠ H2 (teste/dev); ≠ Eureka/Zuul/Swagger (outros eixos) |

O que PostgreSQL **não** é na prova:

- não é Service Discovery, API Gateway, MapStruct nem Swagger;  
- não é “substituto” de mensageria (RabbitMQ);  
- não obriga um **único banco nacional compartilhado** por todos os tribunais (visão de ecossistema ≠ monólito de dados).

Flash PDPJ: **microsserviço + JPA/Hibernate (+ Flyway) → PostgreSQL em ambiente real**.

### 1.3 H2: testes e desenvolvimento

**H2 Database** é um SGBD **leve**, tipicamente usado de forma **embutida** (*embedded*) ou **em memória** (*in-memory*) em aplicações Java/Spring.

Papel cobrável na PDPJ / Spring:

| Uso | Ideia |
|-----|--------|
| **Testes** | Subir banco rápido no classpath; testes de repositório/serviço sem instalar PostgreSQL local |
| **Desenvolvimento** | Prototipar / rodar a app localmente com setup mínimo |
| **Não-papel** | Substituir PostgreSQL como banco de **produção** da plataforma |

Por que a banca cita H2 junto com PostgreSQL:

- o stack Java/Spring da PDPJ prevê **dois contextos de banco**: o real (PostgreSQL) e o de **ciclo rápido** (H2);  
- questões gostam de perguntar **onde** cada um vive (prod × teste/dev).

Modos mentais (não precisa decorar API):

| Modo | Leitura de prova |
|------|------------------|
| `mem` (em memória) | Some ao fim do processo; ideal para teste unitário/rápido |
| Arquivo / embutido | Persistência leve em disco local — ainda assim **dev/teste**, não “produção PDPJ” |
| Console web (visão) | Facilidade de inspção em dev — detalhe acessório |

Flash: pergunta do H2 = “banco **embutido/leve** para **testar ou desenvolver**?”

### 1.4 Diferenças PostgreSQL × H2 (ouro anti-confusão)

Decore esta tabela — a FCC **adora** inverter papéis:

| | **PostgreSQL** | **H2** |
|---|----------------|--------|
| Papel no edital PDPJ | Banco **relacional de referência** / produção | Banco para **teste e desenvolvimento** |
| Forma típica | Servidor (ou gerenciado, ex. RDS) | Embutido / in-memory no processo da app |
| Fidelidade SQL | Dialeto e recursos **completos** do Postgres | Subconjunto; modo `MODE=PostgreSQL` é **emulação parcial** |
| Objetivo do teste | Homologar comportamento **real** | Velocidade e simplicidade do ciclo |
| Risco clássico | — | Teste “passa no H2” e **quebra** no Postgres (sintaxe/recursos específicos) |

Pontos cobráveis extras:

1. **Compatibilidade ≠ identidade.** H2 em modo PostgreSQL **aproxima** comportamento; **não garante** paridade com `ON CONFLICT`, tipos avançados, extensões, JSONB pleno, etc.  
2. **Mesmo projeto, dois profiles.** Profile `prod`/`hom` → PostgreSQL; profile `test`/`dev` → H2 (padrão mental Spring Boot).  
3. **H2 não “vira” PostgreSQL** só porque a URL tem `MODE=PostgreSQL`.  
4. Em arquitetura de microsserviços, cada serviço pode ter **seu** schema/banco PostgreSQL; H2 não muda essa lógica — só o **ambiente** de execução do teste.

Armadilhas clássicas:

| Enunciado enganoso | Correção |
|--------------------|----------|
| “H2 é o banco de produção da PDPJ” | **Falso** — produção = PostgreSQL |
| “PostgreSQL só serve para unit test” | **Falso** — é o SGBD operacional |
| “H2 e PostgreSQL são sinônimos no edital” | **Falso** — duo complementar, papéis distintos |
| “Flyway/Envers/Swagger = H2” | **Falso** — eixos diferentes |

### 1.5 PostgreSQL 15–18 (visão cobrável, alta)

O edital cita as **majors 15 a 18**. Em prova, cobram **visão de evolução** e âncoras de *release*, não o changelog completo. Use o mapa abaixo (nível de segurança: o suficiente para marcar alternativa sem virar DBA).

#### 15 (out/2022) — âncoras

- Comando SQL padrão **`MERGE`** (inserir/atualizar/apagar condicionalmente em um statement).  
- Publicação seletiva na **replicação lógica** (listas de colunas / filtros de linha).  
- Compressão adicional (ex. **zstd**) e melhorias de *sorting*.  
- Logs de servidor em formato estruturado **JSON** (visão operacional).

Flash 15: **`MERGE` + refinamentos de replicação lógica**.

#### 16 (set/2023) — âncoras

- Mais paralelismo (ex. *hash joins* `FULL` / *outer*).  
- **Replicação lógica a partir de standby**.  
- Aplicação paralela de grandes transações no *subscriber*.  
- Visão de monitoramento **`pg_stat_io`**.  
- Construtores / funções **SQL/JSON**; regex em `pg_hba.conf` / `pg_ident.conf` (visão de admin).

Flash 16: **logical replication + observabilidade I/O + SQL/JSON**.

#### 17 (set/2024) — âncoras

- Novo gerenciamento de memória do **`VACUUM`** (menos memória, melhor manutenção).  
- **`JSON_TABLE`** e ampliação SQL/JSON (construtores/funções de consulta).  
- Melhorias de desempenho (I/O em streaming, escrita concorrente, `IN` em B-tree).  
- Replicação lógica com **failover** / utilitários; `pg_upgrade` preserva estado de replicação lógica (visão).  
- **`pg_basebackup` incremental**; `COPY ... ON_ERROR ignore`.

Flash 17: **VACUUM novo + JSON_TABLE + backup incremental**.

#### 18 (set/2025) — âncoras

- Subsistema de **I/O assíncrono (AIO)** (scans, vacuum etc.).  
- `pg_upgrade` **retém estatísticas** do otimizador.  
- *Skip scan* em índices B-tree multicoluna.  
- **`uuidv7()`**; colunas geradas **virtuais** (cálculo na leitura; padrão novo).  
- Autenticação **OAuth**; `RETURNING` com `OLD`/`NEW`; **constraints temporais** (ranges em PK/UK/FK).

Flash 18: **AIO + uuidv7 + OAuth + constraints temporais**.

#### Cartão mental 15→18

| Versão | Uma frase |
|--------|-----------|
| 15 | `MERGE` entra no SQL padrão do Postgres |
| 16 | Replicação lógica / standby e SQL/JSON avançam |
| 17 | Manutenção (`VACUUM`) + `JSON_TABLE` + backup incremental |
| 18 | AIO, upgrade com stats, uuidv7, OAuth, temporais |

**Regra de prova:** minor (15.x → 15.y) ≠ major (15 → 16). Majors **exigem** procedimento de migração; minors, em regra, não.

### 1.6 Migração / compatibilidade a partir da 14 (alto nível)

O edital pede **noções** de migração/compatibilidade com a **14** em legados. Não decore flags; fixe o **modelo mental**.

#### Por que migrar da 14

- Ciclo de vida: majors antigas **saem de suporte** (a 14 tem fim de suporte comunitário em **nov/2026** — dado de calendário útil para contextualizar “legado”).  
- Novas majors trazem recursos (15–18) e correções; ficar na 14 é cenário de **legado**.

#### Três caminhos clássicos (visão)

| Método | Ideia cobrável | Quando pensar |
|--------|----------------|---------------|
| **`pg_upgrade`** | Reaproveita arquivos de dados do usuário; recria catálogos do sistema; **rápido** entre majors compatíveis | Upgrade *in-place* / janela curta |
| **Dump/restore** (`pg_dump` / `pg_dumpall` + restore) | Exporta lógica/dados e recria no cluster novo | Mais simples de entender; janela maior |
| **Replicação lógica** | Replica mudanças para cluster novo e corta (*cutover*) | Migração com menos downtime (visão) |

Pontos de ouro:

1. **Major upgrade ≠ minor patch.** De 14.x para 14.y: caminho de *minor*. De 14 → 15/16/17/18: **major** → um dos métodos acima.  
2. `pg_upgrade` **não** é necessário entre *minors* da mesma major.  
3. Sempre há **checklist de incompatibilidade** da *release notes* da major-alvo (comportamentos que quebram apps). Em prova: “é preciso **revisar breaking changes**”, não “basta trocar o binário”.  
4. Extensões, *collations*, checksums, slots de replicação e estatísticas podem exigir atenção — nível: **planejar**, não memorizar cada caso.  
5. Na 18, detalhe cobrável leve: `pg_upgrade` pode **manter estatísticas** do otimizador (menos “plano frio” pós-upgrade).  
6. H2 **não** entra no plano de migração de cluster PostgreSQL 14→N.

Flash migração: **14 legada → major nova via `pg_upgrade` *ou* dump/restore *ou* logical replication; revisar compatibilidade**.

### 1.7 Encaixe com o restante da semana 2

| Tema | Relação com este arquivo |
|------|---------------------------|
| JPA / Hibernate | ORM fala com **PostgreSQL** (prod) ou **H2** (teste) via dialeto/driver |
| Flyway | Scripts versionados devem ser válidos no **alvo real** (PostgreSQL); H2 pode mascarar incompatibilidades |
| SQL / PL/SQL (arquivo 05) | Sintaxe e procedures cobradas no dialeto de prova — Postgres no centro |
| Keycloak / SSO (06) | Em stacks locais PDPJ, Keycloak frequentemente usa **PostgreSQL** como store — reforça Postgres como BD operacional |

### 1.8 Checklist rápido (caderno de erros)

- [ ] PostgreSQL = produção/PDPJ; H2 = teste/dev  
- [ ] H2 embutido/in-memory; Postgres = servidor SGBD  
- [ ] `MODE=PostgreSQL` ≠ paridade total  
- [ ] 15=`MERGE`; 16=logical/standby+SQL/JSON; 17=`JSON_TABLE`+VACUUM+backup incr.; 18=AIO+uuidv7+OAuth+temporais  
- [ ] 14→N: `pg_upgrade` / dump-restore / logical; major ≠ minor  
- [ ] Não confundir BD com Eureka, Zuul, Swagger, MapStruct, Flyway

---

## 2. Questões de treino (1–15)

**1.** No eixo PDPJ-Br / persistência do edital, o banco relacional de referência para ambientes reais é:

A) H2 em memória como padrão de produção  
B) PostgreSQL  
C) Eureka Server  
D) MapStruct  
E) Swagger UI  

**2.** O papel típico do **H2** no stack citado junto à PDPJ é:

A) SGBD exclusivo de produção nacional único  
B) Banco leve/embutido para **testes e desenvolvimento**  
C) Service Discovery de instâncias  
D) API Gateway de borda  
E) Substituição obrigatória do RabbitMQ  

**3.** Assinale a associação **correta**:

A) PostgreSQL = só unit test; H2 = produção PDPJ  
B) PostgreSQL = produção/operacional; H2 = teste/dev  
C) Ambos são sinônimos estritos no edital  
D) H2 = Zuul; PostgreSQL = Eureka  
E) PostgreSQL documenta OpenAPI  

**4.** Afirmar que “H2 em `MODE=PostgreSQL` garante 100% de paridade com o PostgreSQL de produção” é:

A) Sempre verdadeiro  
B) Impreciso: a emulação é **parcial**; recursos específicos podem falhar  
C) Verdadeiro somente se o profile for `prod`  
D) Verdadeiro porque H2 recompila o núcleo do Postgres  
E) Verdadeiro se houver Flyway  

**5.** Qual característica combina com o H2 em cenário de teste Spring?

A) Cluster com replicação lógica multi-região como definição  
B) Subida rápida embutida/in-memory no classpath da aplicação de teste  
C) Obrigação de ser o único banco do Judiciário  
D) Substituição do Keycloak  
E) Função exclusiva de API Gateway  

**6.** Entre as âncoras da **versão 15**, destaca-se:

A) Apenas a remoção total do SQL  
B) Introdução ampla do comando **`MERGE`** no PostgreSQL  
C) Substituição do PostgreSQL pelo H2 em produção  
D) Fim da replicação lógica  
E) Obrigação de usar apenas Oracle 19c  

**7.** Âncora típica da **versão 16**:

A) Somente interface gráfica obrigatória  
B) Avanços em **replicação lógica** (incl. a partir de standby) e SQL/JSON / observabilidade  
C) Proibição de `MERGE`  
D) H2 vira padrão de produção  
E) Eliminação de `pg_upgrade`  

**8.** Na **versão 17**, associação cobrável inclui:

A) `JSON_TABLE` / SQL/JSON ampliado e melhorias fortes de `VACUUM` / backup incremental  
B) Apenas mudança de logo  
C) Remoção de JSON do produto  
D) H2 substituindo `pg_basebackup`  
E) Fim do SQL padrão  

**9.** Na **versão 18**, âncoras de prova incluem:

A) Somente retorno à versão 9.6  
B) I/O assíncrono (AIO), `uuidv7()`, OAuth, constraints temporais (visão)  
C) Exclusividade do banco H2  
D) Proibição de `pg_upgrade`  
E) Substituição de microsserviços por monólito de dados  

**10.** Migração **major** de PostgreSQL 14 para 16 exige, em regra:

A) Apenas trocar o número na documentação, sem procedimento  
B) Procedimento de major (`pg_upgrade` e/ou dump/restore e/ou replicação lógica), com atenção a incompatibilidades  
C) Uso obrigatório e exclusivo do H2 como ponte  
D) Reinstalação do Eureka  
E) Conversão automática pelo MapStruct  

**11.** `pg_upgrade` caracteriza-se, em visão de prova, por:

A) Ser o único nome do H2 Console  
B) Permitir upgrade major reaproveitando arquivos de dados do usuário (quando compatível), de forma tipicamente mais rápida que dump completo  
C) Documentar OpenAPI  
D) Registrar microsserviços no Eureka  
E) Gerar DTOs em tempo de compilação  

**12.** Qual afirmação sobre **minor** × **major** está correta?

A) 14.2 → 14.5 exige o mesmo rito de 14 → 18  
B) Majors (ex.: 14 → 15) exigem migração; minors da mesma série, em regra, não usam `pg_upgrade`  
C) Minors sempre exigem dump/restore completo  
D) Majors nunca precisam de revisão de *breaking changes*  
E) H2 define o que é major no PostgreSQL  

**13.** Em enunciado: “profile de teste sobe banco embutido em memória; homologação usa SGBD servidor relacional da plataforma.” A dupla tipicamente descrita é:

A) Swagger e MapStruct  
B) H2 e PostgreSQL  
C) Zuul e Eureka invertidos como bancos  
D) Flyway e Envers como SGBDs  
E) RabbitMQ e Keycloak como dialetos SQL  

**14.** Qual opção **inverte** indevidamente os papéis?

A) PostgreSQL em produção; H2 em testes  
B) H2 como banco de produção PDPJ; PostgreSQL só para brinquedo local sem papel no edital  
C) Major 15 traz `MERGE`  
D) 14 legada pode migrar via `pg_upgrade`  
E) Emulação H2 ≠ paridade total  

**15.** Síntese correta deste subtópico:

A) H2 = produção; Postgres = apenas docs  
B) PostgreSQL = SGBD operacional PDPJ (versões 15–18 no edital; noções 14→N); H2 = teste/dev embutido — papéis distintos  
C) Ambos são API Gateway  
D) Migração 14→18 é feita pelo Swagger UI  
E) `JSON_TABLE` existe só no H2  

---

### Gabarito comentado (treino 1–15)

1. **B** — PostgreSQL é o BD relacional de referência/produção no eixo PDPJ.  
2. **B** — H2 = teste/desenvolvimento (leve/embutido).  
3. **B** — Tabela anti-confusão clássica.  
4. **B** — Modo Postgres do H2 é parcial.  
5. **B** — Embutido/in-memory no ciclo de teste.  
6. **B** — `MERGE` é âncora da 15.  
7. **B** — Logical replication / SQL/JSON / I/O stats na 16.  
8. **A** — `JSON_TABLE`, VACUUM, backup incremental na 17.  
9. **B** — AIO, uuidv7, OAuth, temporais na 18.  
10. **B** — Major = procedimento + compatibilidade.  
11. **B** — `pg_upgrade` reaproveita data files.  
12. **B** — Major ≠ minor.  
13. **B** — H2 (teste) + PostgreSQL (homolog/prod).  
14. **B** — Única que **inverte** produção × teste.  
15. **B** — Síntese do arquivo 04.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 56 de 60)

Dentre as técnicas para otimizar o desempenho de consultas que envolvem a análise de grandes volumes de dados geográficos no PostgreSQL 13, uma Analista está estudando aquela que é a mais adequada, recomendando:

A) utilizar a extensão PostGIS, para todas as operações espaciais.

B) utilizar índices B-tree em colunas de tipo geométrico.

C) fragmentar tabelas geográficas, com base em atributos espaciais.

D) criar materialized views, para pré-calcular resultados de consultas espaciais.

E) criar índices GIST em colunas de tipo geométrico.


**17.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 34 de 60)

No PostgreSQL 13 um trigger

A) permite chamar uma função se uma cláusula INSERT, UPDATE, DELETE ou TRUNCATE ocorrer em uma tabela.

B) pode modificar os dados na tabela virtual somente após a modificação ocorrer na tabela real.

C) deve ser único em uma tabela, ou seja, não pode haver mais de um trigger para a mesma tabela.

D) pode chamar funções que exibem dados, mas não funções para modificá-los.

E) pode ser acionado para uma tabela, mas não para uma view, chamando sempre uma procedure após o acionamento.


**18.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 22 de 60)

Considere que exista um banco de dados PostgreSQL aberto e funcionando em condições ideais, e que a tabela t_processos exista e esteja corretamente preenchida com valores adequados. Neste contexto, observe a função abaixo. CREATE FUNCTION atualizar_status_processo(num_processo VARCHAR) RETURNS INTEGER AS $$ DECLARE total INTEGER; BEGIN UPDATE t_processos SET status_processo = 'Encerrado' WHERE numero_processo = num_processo; SELECT COUNT(*) INTO total FROM t_processos WHERE status_processo = 'Encerrado'; RETURN total; END; $$ LANGUAGE plpgsql; Em condições ideais, esta função seria corretamente chamada utilizando-se o comando:

A) `CALL atualizar_status_processo('12345-67.2023.5.01.0001');`

B) `EXECUTE atualizar_status_processo('12345-67.2023.5.01.0001');`

C) DO $$ BEGIN PERFORM atualizar_status_processo('12345-67.2023.5.01.0001'); END; $$ LANGUAGE plpgsql;

D) `SELECT atualizar_status_processo('12345-67.2023.5.01.0001');`

E) `EXEC atualizar_status_processo '12345-67.2023.5.01.0001';`


**19.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 26 de 60)

Considere que um Analista está trabalhando em um banco de dados PostgreSQL aberto e funcionando em condições ideais. O Analista inseriu a seguinte sequência de comandos:```DROP TABLE I TRTContatos;CREATE TABLE TRTContatos ( cod serial PRIMARY KEY, orgao VARCHAR(255) NOT NULL, email VARCHAR(255) NOT NULL, descricao VARCHAR(255));INSERT INTO TRTContatos (orgao, email)VALUES ('Ouvidoria','ouvidoria@trt22.jus.br:'), ('Escola Judicial','ejud@trt22.jus.br'), ('Corregedoria','secor@trt22.jus.br')II ;```É correto afirmar que

A) haverá erro, pois não é possível fazer inserção de múltiplas linhas em tabelas do PostgreSQL.

B) haverá erro devido ao fato de as 3 linhas a serem inseridas na tabela TRTContatos não terem os 4 campos informados em CREATE TABLE.

C) I deve ser preenchido com IF EXISTS e II deve ser preenchido com RETURNING * para que as linhas inseridas sejam apresentadas.

D) I deve ser preenchido com ALL ROWS e II deve ficar sem nenhum preenchimento.

E) faltou um _ entre cod e serial, pois o certo seria cod_serial PRIMARY KEY.


**20.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60) *[aproximação temática]*

Uma equipe de desenvolvimento está utilizando Spring Boot para criar uma API RESTful. Durante os testes, observaram que a aplicação demora a inicializar, principalmente devido à configuração do banco de dados. Para melhorar o tempo de inicialização, decidiram desabilitar a criação automática do esquema de banco de dados ao iniciar a aplicação. A configuração, no `application.properties`, que desabilita a criação automática do esquema de banco de dados pelo Spring Boot é a

A) `spring.jpa.hibernate.ddl-auto=none`

B) `spring.datasource.database-auto=false`

C) `spring.jpa.db-sql=disable`

D) `spring.jpa.properties.hibernate.db-auto=disable`

E) `spring.jpa.create-database-init=false`


**21.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60)

Considere o seguinte caso:Utilizando comandos SQL, um analista criou uma tabela de cidadão dando permissão de acesso ao usuário Roberto. Posteriormente inseriu dados nessa tabela, mas logo em seguida teve que deletá-los, retirar a permissão de Roberto que estava saindo do tribunal e dar permissão a Carla.A sequência de comandos SQL utilizada no caso é, correta e respectivamente, categorizada como

A) DML - DCL - DDL - DDL - DCL - DCL.

B) DML - DCL - DML - DDL - DML - DCL.

C) DDL - DCL - DML - DML - DCL - DCL.

D) DDL - DCL - DDL - DML - DML - DCL.

E) DCL - DDL - DML - DML - DCL - DCL.


**22.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60)

Considere uma tabela denominada `Cidadao` e as colunas `Nome_Cidadao` e `Valor_Recebido`.Para obter a média dos valores maiores que 200 recebidos pelos cidadãos, um Analista deve utilizar o comando

A) `SELECT AVG(Valor_Recebido > 200) FROM Cidadao;`

B) `SELECT AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`

C) `SELECT AVG(Valor_Recebido) > 200 FROM Cidadao;`

D) `SELECT FROM Cidadao AVG(Valor_Recebido) > 200;`

E) `SELECT Nome_Cidadao and AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`


**23.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 23 de 60)

Considere a tabela `t_audiencias` que armazena informações sobre audiências marcadas.```CREATE TABLE t_audiencias ( id_audiencia INT PRIMARY KEY, numero_processo VARCHAR(50) NOT NULL, data_audiencia DATETIME NOT NULL, status VARCHAR(20) NOT NULL, vara_trabalho INT NOT NULL);```Considerando que esta tabela está criada e contém valores adequados, em um banco de dados MySQL aberto e funcionando em condições ideais, o comando que seleciona todas as audiências que estão marcadas como `'Pendente'`, cuja data de audiência seja posterior ao dia e hora atuais e que pertençam às varas de trabalho 1, 2 ou 3, é:`SELECT * FROM t_audiencias WHERE status = 'Pendente' AND`

A) `data_audiencia > CURRENT_DATE_TIME() AND vara_trabalho IN (1, 2, 3);`

B) `data_audiencia > NOW() AND vara_trabalho IN (1, 2, 3);`

C) `data_audiencia > SYSDATE() AND vara_trabalho BETWEEN 1..3;`

D) `data_audiencia > NOW() OR vara_trabalho = 1 OR vara_trabalho = 2 OR vara_trabalho = 3;`

E) `data_audiencia > CURDATE() AND vara_trabalho IN (1..3);`


**24.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 21 de 60) *[aproximação temática]*

*SQL relacional (aproximação PG/H2).*

Em um banco de dados Oracle 19c, aberto e funcionando em condições ideais, existe uma tabela tprocessos com dados válidos, que tem a seguinte estrutura: id_processo (número único de identificação do processo) data_abertura (data em que o processo foi aberto) id_vara (identificador da vara do trabalho onde o processo tramita) valor_processo (valor monetário associado ao processo) O comando SQL que lista o id_vara, o número total de processos por vara e o valor total dos processos, em ordem decrescente, somente das varas que possuem mais de 100 processos abertos antes de 1º de janeiro de 2020, é:

A) ```SELECT id_vara, COUNT() AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura < '2020-01-01'GROUP BY id_varaHAVING COUNT() > 100ORDER BY SUM(valor_processo) DESC;```

B) ```SELECT id_vara, COUNT() AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura < '2020-01-01'GROUP BY id_varaORDER BY SUM(valor_processo) DESCHAVING COUNT() > 100;```

C) ```SELECT id_vara, COUNT(id_processo) AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura < '2020-JAN-01'GROUP BY id_varaHAVING COUNT(id_processo) > 100ORDER BY COUNT(valor_total);```

D) ```SELECT id_vara, COUNT(*) AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura <= '2020-01-01'GROUP BY id_varaORDER BY valor_total;```

E) ```SELECT id_vara, COUNT(*) AS total_processos, SUM(valor_processo) AS valor_totalFROM tprocessosWHERE data_abertura < '2020-01-01'GROUP BY id_varaHAVING SUM(valor_processo) > 100000ORDER BY valor_total DESC;```


**25.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 51 de 60) *[aproximação temática]*

Um Tribunal Regional do Trabalho está desenvolvendo um sistema de gestão de processos judiciais que inclui um módulo de auditoria para rastrear mudanças nos registros dos processos. É essencial que qualquer alteração nos dados dos processos judiciais (por exemplo, petições, decisões, movimentações) seja auditada para garantir a integridade e a transparência. Um Analista de TI decide utilizar Hibernate Envers para atender aos requisitos desse módulo de auditoria e, para isso, deve

A) configurar Hibernate Envers para criar uma tabela de auditoria que registre todas as alterações em entidades anotadas, incluindo quem fez a alteração e quando foi feita.

B) usar Hibernate Envers para criar automaticamente backups diários de todas as tabelas do banco de dados.

C) utilizar Hibernate Envers para migrar dados auditáveis entre diferentes esquemas de banco de dados de forma transparente.

D) implementar Hibernate Envers para validar os dados de auditoria dos processos judiciais antes de serem inseridos no banco de dados.

E) configurar Hibernate Envers para enviar notificações à auditoria, por e-mail, a cada alteração de dados no banco de dados.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **E** | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 56 de 60 |
| 17 | **A** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 34 de 60 |
| 18 | **D** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 22 de 60 |
| 19 | **C** | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 26 de 60 |
| 20 | **A** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 21 | **C** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60 |
| 22 | **B** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60 |
| 23 | **B** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 23 de 60 |
| 24 | **A** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 21 de 60 |
| 25 | **A** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 51 de 60 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q56
- Q17: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q34
- Q18: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q22
- Q19: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q26
- Q20: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q38
- Q21: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q52
- Q22: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q53
- Q23: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q23
- Q24: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q21
- Q25: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q51

*Arquivo 04 · PostgreSQL + H2 · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 05 SQL DML/DDL/DCL + noções PL/SQL.*

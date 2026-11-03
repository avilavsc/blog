+++
title = '02 — Revisão SQL + JPA/Hibernate/Flyway'
date = '2026-10-28'
draft = false
weight = 2
tags = ['semana-11', 'revisao']
categories = ['estudo']
+++
# Revisão SQL + JPA/Hibernate/Flyway · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Semana 11 · revisão dirigida (não matéria nova)  
**Escopo deste arquivo:** reforço **SQL** (DML/DDL/DCL + TCL/ACID, PostgreSQL flash) + **JPA/Hibernate/Envers/Flyway** — pegadinhas FCC. Pressupõe semana 2 (e JPA da PDPJ).  
**Fora de escopo aqui:** PDPJ/Gateway/Eureka/Zuul/Keycloak (arquivo 01); Oracle 19c produto profundo; redes/segurança.

**Modo:** cartões de classificação + anti-inversão Flyway×Envers×JPA + treino + bloco real 16–25.

---

## 1. Material (revisão com margem de segurança)

### 1.1 Famílias SQL (classificar = ouro FCC)

| Família | Objeto | Comandos âncora |
|---------|--------|-----------------|
| **DDL** | **Estrutura** | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME` |
| **DML** | **Dados** (linhas) | `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `MERGE` |
| **DCL** | **Privilégios** | `GRANT`, `REVOKE` |
| **TCL** | **Transação** | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |

Flash de prova:

- Criar tabela → **DDL**. Dar/retirar permissão → **DCL**. Inserir/apagar **linhas** → **DML**.  
- Sequência clássica (TRT-12/2023): criou tabela (**DDL**) → permissão (**DCL**) → insert (**DML**) → delete dados (**DML**) → revoke (**DCL**) → grant (**DCL**) = **DDL–DCL–DML–DML–DCL–DCL**.

### 1.2 TRUNCATE × DELETE × DROP (pegadinha nº 1)

| | Estrutura some? | Linhas | Família usual | Rollback típico (visão concurso) |
|---|-----------------|--------|---------------|----------------------------------|
| `DELETE` | Não | Selecionadas / todas sem `WHERE` | **DML** | Sim, na mesma transação |
| `TRUNCATE` | Não | Todas (rápido) | **DDL** | Em geral **não** como DML |
| `DROP TABLE` | Sim | Vai junto | **DDL** | Após DDL, em geral irrevogável |

Erro: classificar `TRUNCATE` como “só DML porque apaga dados”. Em FCC (ex.: MERGE/TRUNCATE/REVOKE): **MERGE=DML, TRUNCATE=DDL, REVOKE=DCL**.

### 1.3 DCL no PostgreSQL (flash)

- Conceder: `GRANT … ON … TO …`  
- Revogar todos os direitos na tabela: **`REVOKE ALL ON produtos FROM user001;`** (TRT4/2022).  
- Distratores: `GRANT REVOKE`, `REVOKE PERMISSIONS`, `ALL PERMISSIONS`, `APPLY REVOKE`.

DCL ≠ SSO/Keycloak: DCL é **privilégio no SGBD**; Keycloak é identidade da aplicação.

### 1.4 Consultas que a FCC repete

- Média com filtro: agregação no `SELECT`, filtro no `WHERE` —  
  `SELECT AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`  
  (não `AVG(coluna > 200)`).  
- `IN (1,2,3)` × `BETWEEN`; `NOW()` em MySQL de prova; `HAVING` **depois** de `GROUP BY` para filtrar agregados.  
- Função PL/pgSQL que `RETURNS INTEGER`: chamar com **`SELECT nome_func(...);`** (não `CALL`/`EXEC` estilo procedure, na visão cobrada TRT15/2024).  
- Trigger PG: pode disparar função em `INSERT`/`UPDATE`/`DELETE`/`TRUNCATE` (visão TRT19/2022).

### 1.5 PostgreSQL × H2 (flash PDPJ)

| | **PostgreSQL** | **H2** |
|---|----------------|--------|
| Papel | Produção / ambiente real dos serviços | Teste / dev, leve, embutido/mem |
| Erro | Tratar H2 como prod PDPJ | Tratar PG como “só brinquedo” |

H2 CSV (TRT20/2024 Técnico, útil no radar):  
`INSERT INTO tabela SELECT * FROM CSVREAD('dado.csv');`  
(não `LOAD DATA` MySQL, não `COPY` PG).

### 1.6 JPA × Hibernate × Envers × Flyway (ouro anti-inversão)

| Peça | Natureza | Frase cobrável |
|------|----------|----------------|
| **JPA** | **Especificação** (API) | Contrato de persistência ORM |
| **Hibernate** | **Implementação** ORM | Provider concreto; `Session` / dialects |
| **Envers** | Auditoria de **entidades** | `@Audited` → histórico de revisões |
| **Flyway** | Migration de **schema** | Scripts `V1__…sql` + histórico |

Inversão TRF4/2025 (distratores): Swagger “versiona SQL”; Flyway “documenta REST”; Eureka “gateway”; Zuul “cliente do Eureka”. Gabarito limpo no eixo persistência: **Envers audita entities**.

Frase de ouro:

> **Flyway versiona o schema; Hibernate persiste entidades; Envers audita mudanças em entidades; JPA é a especificação.**

### 1.7 Cartão JPA (o que reaparece)

| Tema | Âncora |
|------|--------|
| Mapeamento padrão | Anotações `@Entity`, `@Id`, `@Table`, `@Column` (não `@Database`/`@Field`) |
| Provider no XML | `<provider>…Hibernate…</provider>` em **`persistence.xml`** |
| `createNamedQuery(…, Classe.class)` | Retorna **`TypedQuery<T>`** |
| Auto-incremento coluna | `GenerationType.IDENTITY` |
| Schema auto Hibernate | `hibernate.hbm2ddl.auto=update` (dev); em Boot: `spring.jpa.hibernate.ddl-auto=none` para **desligar** |
| Datas Java 8 (Hibernate 5+) | `LocalDate`, `LocalTime`, `LocalDateTime` |
| Habilitar Envers na entidade | **`@Audited`** (não `AuditedEntity`, não “Flyway já auditou”) |

`ddl-auto` / `hbm2ddl` **não** substituem Flyway em produção madura: ORM gera/atualiza schema à moda antiga; Flyway **versiona** de forma controlada entre ambientes.

### 1.8 ACID (1 cartão)

| | Ideia |
|---|--------|
| **A**tomicidade | Tudo ou nada |
| **C**onsistência | Regras/invariantes preservadas |
| **I**solamento | Transações concorrentes não se “atropelam” indevidamente |
| **D**urabilidade | Após `COMMIT`, sobrevive a falha |

`COMMIT` confirma; `ROLLBACK` desfaz; `SAVEPOINT` permite rollback parcial.

### 1.9 Mapa de pegadinhas FCC (checklist)

1. `TRUNCATE` = DML.  
2. `GRANT` = DML.  
3. `SELECT` “sempre modifica” o banco.  
4. `@Audited` = Flyway / MapStruct / Eureka.  
5. Flyway = documentação REST; Swagger = migration.  
6. JPA = implementação; Hibernate = só especificação.  
7. `persistence.xml` trocado por `hibernate.xml` / `server.xml`.  
8. `IDENTITY` trocado por `AUTO_INCREMENT` / `SELF_GENERATED`.  
9. `AVG(col > 200)` no lugar de `WHERE`.  
10. `REVOKE ALL PERMISSIONS` / `GRANT REVOKE`.  
11. Envers = backup diário / e-mail / migração de schema.  
12. H2 = banco de produção da PDPJ.

### 1.10 Ordem nesta revisão

1. Classificar 10 comandos em DDL/DML/DCL de olhos fechados.  
2. Decorar tabela Flyway × Envers × JPA × Hibernate.  
3. Anotações JPA + `@Audited` + `ddl-auto=none`.  
4. Treino 1–15; depois reais 16–25; caderno de erros.

---

## 2. Questões de treino (1–15)

**1.** `CREATE TABLE` é tipicamente:

A) DML  
B) DCL  
C) DDL  
D) TCL apenas  
E) OAuth  

**2.** `GRANT SELECT ON t TO u` é:

A) DCL  
B) DML  
C) DDL  
D) Envers  
E) Flyway  

**3.** Classificação cobrável de `TRUNCATE`:

A) DML puro, sempre rollbackável como `DELETE`  
B) DCL  
C) Só TCL  
D) DDL (esvazia linhas; estrutura permanece)  
E) JPQL  

**4.** São respectivamente DML, DDL e DCL:

A) `DROP`, `GRANT`, `INSERT`  
B) `CREATE`, `INSERT`, `UPDATE`  
C) `RENAME`, `DELETE`, `DROP`  
D) `UPDATE`, `CALL`, `ALTER`  
E) `MERGE`, `TRUNCATE`, `REVOKE`  

**5.** Sequência: criar tabela → conceder acesso → inserir → deletar linhas → revogar → conceder a outro. Categorias:

A) DML–DCL–DDL–DDL–DCL–DCL  
B) DCL–DDL–DML–DML–DCL–DCL  
C) DDL–DCL–DDL–DML–DML–DCL  
D) DDL–DCL–DML–DML–DCL–DCL  
E) DML–DCL–DML–DDL–DML–DCL  

**6.** JPA é prioritariamente:

A) Implementação ORM exclusiva da Oracle  
B) Ferramenta de migration  
C) API Gateway  
D) Especificação/API de persistência  
E) Service Discovery  

**7.** Hibernate, no discurso de prova, é:

A) Implementação ORM (provider) da JPA  
B) Apenas o arquivo `swagger.yaml`  
C) O Authorization Server  
D) Sinônimo de Flyway  
E) O Eureka Client  

**8.** `@Audited` em entidade JPA sinaliza:

A) Flyway que a entidade já foi migrada  
B) MapStruct  
C) Eureka  
D) Zuul  
E) Hibernate Envers que a entidade será auditada  

**9.** Flyway serve para:

A) Auditar campos de entidade com `@Audited`  
B) Documentar REST como OpenAPI  
C) Versionar/migrar o **schema** com scripts ordenados  
D) Registrar instâncias no Eureka  
E) Emitir Bearer tokens  

**10.** Forma padrão de mapear POJO→tabela no JPA:

A) Anotações `@Entity`, `@Id`, `@Table`, `@Column`  
B) Só `entity.xml` obrigatório  
C) `@Database`, `@Primary`, `@Field`  
D) Classes `DataMapper` obrigatórias  
E) Anotação `@EnableZuulProxy`  

**11.** Provider Hibernate no XML clássico fica em:

A) `server.xml`  
B) `hibernate.xml` exclusivo  
C) `pom.java`  
D) `persistence.xml`  
E) `eureka-server.xml`  

**12.** Para desabilitar criação automática de schema no Spring Boot:

A) `spring.jpa.db-sql=disable`  
B) `spring.datasource.database-auto=false`  
C) `spring.jpa.hibernate.ddl-auto=none`  
D) `spring.jpa.create-database-init=false`  
E) `flyway.enabled=eureka`  

**13.** Auto-incremento da coluna do BD no `@GeneratedValue` → estratégia:

A) `TABLE`  
B) `IDENTITY`  
C) `SELF_GENERATED`  
D) `AUTO_GENERATED`  
E) `AUTO_INCREMENT`  

**14.** No PostgreSQL, revogar todos os direitos de `user001` em `produtos`:

A) `GRANT REVOKE ALL ON produtos FROM user001;`  
B) `REVOKE PERMISSIONS ON produtos FROM user001;`  
C) `APPLY REVOKE ON produtos FROM user001;`  
D) `REVOKE ALL ON produtos FROM user001;`  
E) `REVOKE ALL PERMISSIONS ON produtos FROM USER user001;`  

**15.** Síntese da revisão:

A) Flyway = Envers = Swagger  
B) DDL estrutura; DML dados; DCL privilégios; JPA especifica; Hibernate implementa; Flyway migra schema; Envers audita entidades  
C) `@Audited` habilita Eureka  
D) H2 é o SGBD de produção padrão da PDPJ  
E) `TRUNCATE` = DCL  

---

### Gabarito comentado (treino 1–15)

1. **C** — DDL.  
2. **A** — DCL.  
3. **D** — TRUNCATE como DDL.  
4. **E** — MERGE / TRUNCATE / REVOKE.  
5. **D** — Sequência TRT-12.  
6. **D** — Especificação.  
7. **A** — Implementação ORM.  
8. **E** — Envers.  
9. **C** — Migrations.  
10. **A** — Anotações JPA.  
11. **D** — `persistence.xml`.  
12. **C** — `ddl-auto=none`.  
13. **B** — `IDENTITY`.  
14. **D** — `REVOKE ALL ON … FROM …`.  
15. **B** — Síntese.

Letras treino: C, A, D, E, D, D, A, E, C, A, D, C, B, D, B.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** FCC. Sem invenção. Gabarito só no final.

**16.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60)

Considere o seguinte caso: Utilizando comandos SQL, um analista criou uma tabela de cidadão dando permissão de acesso ao usuário Roberto. Posteriormente inseriu dados nessa tabela, mas logo em seguida teve que deletá-los, retirar a permissão de Roberto que estava saindo do tribunal e dar permissão a Carla. A sequência de comandos SQL utilizada no caso é, correta e respectivamente, categorizada como

A) DML - DCL - DDL - DDL - DCL - DCL.  
B) DML - DCL - DML - DDL - DML - DCL.  
C) DDL - DCL - DML - DML - DCL - DCL.  
D) DDL - DCL - DDL - DML - DML - DCL.  
E) DCL - DDL - DML - DML - DCL - DCL.  

**17.** (FCC | TRT-23 2022 | Técnico Judiciário, Área Apoio, Tecnologia da Informação)

Na linguagem SQL são respectivamente comandos DML, DDL e DCL:

A) `DROP`, `LOCK TABLE` e `GRANT`  
B) `UPDATE`, `CALL` e `ALTER`  
C) `CREATE`, `INSERT` e `GRANT`  
D) `MERGE`, `TRUNCATE` e `REVOKE`  
E) `RENAME`, `DELETE` e `DROP`  

**18.** (FCC | TRT4 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60)

No PostgreSQL, em condições ideais, para revogar todos os direitos do usuário user001 na tabela produtos, um Analista deve utilizar o comando

A) `GRANT REVOKE ALL ON produtos FROM user001;`  
B) `REVOKE PERMISSIONS ON produtos FROM user001;`  
C) `APPLY REVOKE ON produtos FROM user001;`  
D) `REVOKE ALL ON produtos FROM user001;`  
E) `REVOKE ALL PERMISSIONS ON produtos FROM USER user001;`  

**19.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60)

Considere uma tabela denominada `Cidadao` e as colunas `Nome_Cidadao` e `Valor_Recebido`. Para obter a média dos valores maiores que 200 recebidos pelos cidadãos, um Analista deve utilizar o comando

A) `SELECT AVG(Valor_Recebido > 200) FROM Cidadao;`  
B) `SELECT AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`  
C) `SELECT AVG(Valor_Recebido) > 200 FROM Cidadao;`  
D) `SELECT FROM Cidadao AVG(Valor_Recebido) > 200;`  
E) `SELECT Nome_Cidadao and AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`  

**20.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Uma equipe de desenvolvimento está utilizando Spring Boot para criar uma API RESTful. Durante os testes, observaram que a aplicação demora a inicializar, principalmente devido à configuração do banco de dados. Para melhorar o tempo de inicialização, decidiram desabilitar a criação automática do esquema de banco de dados ao iniciar a aplicação. A configuração, no `application.properties`, que desabilita a criação automática do esquema de banco de dados pelo Spring Boot é a

A) `spring.jpa.hibernate.ddl-auto=none`  
B) `spring.datasource.database-auto=false`  
C) `spring.jpa.db-sql=disable`  
D) `spring.jpa.properties.hibernate.db-auto=disable`  
E) `spring.jpa.create-database-init=false`  

**21.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60)

Um Analista de um Tribunal necessita habilitar o Hibernate Envers em uma entidade. Para isso, ele deve

A) utilizar o método `Session.saveOrUpdate()` para a entidade.  
B) anotar a entidade com `@Audited`.  
C) configurar `hibernate.envers.enabled` como `true` no arquivo de configuração.  
D) estender a classe `AuditedEntity` fornecida pelo Hibernate Envers.  
E) configurar o `audit-log` no arquivo `persistence.xml`.  

**22.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 23)

O JPA é uma especificação da plataforma Java que define um padrão para mapeamento objeto-relacional (ORM) e gerenciamento de persistência de dados. No JPA, a forma padrão de realizar o mapeamento de objetos Java (POJOs) para tabelas de bancos de dados relacionais é por meio de

A) configurações XML como `persistence.xml`, `entity.xml` e `mapping.xml`.  
B) anotações como `@Entity`, `@Id`, `@Table` e `@Column`.  
C) interfaces como `EntityFactory`, `QueryBuilder` e `PersistenceUnit`.  
D) classes como `HibernateSession`, `EntityManagerFactory` e `DataMapper`.  
E) anotações como `@Database`, `@Primary`, `@Relation` e `@Field`.  

**23.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60)

Uma analista está desenvolvendo um sistema usando JPA e deseja configurar a utilização do framework Hibernate como provedor de persistência, por meio da instrução `<provider>org.hibernate.ejb.HibernatePersistence</provider>` que deve ser colocada no arquivo

A) `hibernate.xml`  
B) `persistence-unit.xml`  
C) `persistence.xml`  
D) `orm-database-config.xml`  
E) `server.xml`  

**24.** (FCC | TRT14 2022 | Analista Judiciário, Tecnologia da Informação)

O uso da anotação `@Audited` antes do nome da classe de entidade do banco de dados, em uma aplicação que utiliza JPA, sinaliza ao

A) Flyway que a entidade já foi auditada.  
B) Hibernate Envers que a entidade será alvo de auditoria.  
C) Map Struct que a entidade já foi auditada.  
D) Spring Cloud que a entidade registra logs de transações com o banco de dados.  
E) Service Discovery o caminho do arquivo de auditoria.  

**25.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 51 de 60)

Um Tribunal Regional do Trabalho está desenvolvendo um sistema de gestão de processos judiciais que inclui um módulo de auditoria para rastrear mudanças nos registros dos processos. É essencial que qualquer alteração nos dados dos processos judiciais (por exemplo, petições, decisões, movimentações) seja auditada para garantir a integridade e a transparência. Um Analista de TI decide utilizar Hibernate Envers para atender aos requisitos desse módulo de auditoria e, para isso, deve

A) configurar Hibernate Envers para criar uma tabela de auditoria que registre todas as alterações em entidades anotadas, incluindo quem fez a alteração e quando foi feita.  
B) usar Hibernate Envers para criar automaticamente backups diários de todas as tabelas do banco de dados.  
C) utilizar Hibernate Envers para migrar dados auditáveis entre diferentes esquemas de banco de dados de forma transparente.  
D) implementar Hibernate Envers para validar os dados de auditoria dos processos judiciais antes de serem inseridos no banco de dados.  
E) configurar Hibernate Envers para enviar notificações à auditoria, por e-mail, a cada alteração de dados no banco de dados.  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Citação |
|---|------|---------|
| 16 | **C** | FCC \| TRT-12 2023 \| Analista TI \| Q52 |
| 17 | **D** | FCC \| TRT-23 2022 \| Técnico TI \| MERGE/TRUNCATE/REVOKE (QConcursos Q1968354) |
| 18 | **D** | FCC \| TRT4 2022 \| Analista TI \| Q41 |
| 19 | **B** | FCC \| TRT-12 2023 \| Analista TI \| Q53 |
| 20 | **A** | FCC \| TRT15 2024 \| Analista TI \| Q38 |
| 21 | **B** | FCC \| TRT20 2024 \| Analista TI \| Q54 |
| 22 | **B** | FCC \| TRF4 2025 \| Analista Análise de Sistemas \| Q23 |
| 23 | **C** | FCC \| TRT22 2022 \| Analista TI \| Q41 |
| 24 | **B** | FCC \| TRT14 2022 \| Analista TI \| @Audited→Envers |
| 25 | **A** | FCC \| TRT15 2024 \| Analista TI \| Q51 |

Letras: C, D, D, B, A, B, B, C, B, A.

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q52
- Q17: https://www.qconcursos.com/questoes-de-concursos/questoes/9e72cb79-46
- Q18: https://www.quizinho.com.br/questao/fcc-trt4-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q41
- Q19: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q53
- Q20: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q38
- Q21: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q54
- Q22: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q23
- Q23: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q41
- Q24: https://www.qconcursos.com/questoes-de-concursos/questoes/e959246f-8d · https://questoes.grancursosonline.com.br/questoes-de-concursos/tecnologia-da-informacao-14/2651473
- Q25: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q51

---

### Verificação interna

| Checagem | Status |
|----------|--------|
| Revisão DDL/DML/DCL + PG + JPA/Hibernate/Flyway/Envers | OK |
| Pegadinhas FCC (TRUNCATE, @Audited, inversões) | §1.9 |
| Treino 1–15 | OK |
| 16–25 reais + citação | 10/10 |
| Gabarito só no final | OK |

*Arquivo 02 · Revisão SQL + JPA · Semana 11 · Claudionir · TRT4 Analista TI (código 14) · FCC. Anterior: 01-revisao-pdpj-spring.md.*

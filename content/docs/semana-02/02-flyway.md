+++
title = '02 — Flyway'
date = '2026-10-21'
draft = false
weight = 2
tags = ['semana-02', 'flyway']
categories = ['estudo']
+++
# Flyway · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Flyway (Semana 2 · Persistência PDPJ)  
**Escopo deste arquivo:** **Flyway** como ferramenta de **migração e versionamento de schema** de banco; scripts versionados; tabela de histórico; relação com **JPA / Hibernate** (e Envers) na stack PDPJ; contraste com `ddl-auto` / evolução só via ORM.  
**Fora de escopo aqui:** JPA/Hibernate/Envers em profundidade (arquivo 01 da semana 2), PostgreSQL/H2/SQL como tema completo, Keycloak/OAuth2, MapStruct, Swagger, Eureka/Zuul.

---

## 1. Material

### 1.1 O que é Flyway (leitura de prova)

**Flyway** é uma ferramenta de **migração de banco de dados** (database migration): aplica **scripts versionados** (em geral SQL) para criar e evoluir o **schema** de forma **repetível**, **rastreável** e **ordenada** entre ambientes (dev, homolog, prod).

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | **Versionar e migrar o schema** do BD |
| Como | Scripts **versionados** (ex.: `V1__...sql`, `V2__...sql`) aplicados em ordem |
| O que **não** é | ORM (JPA/Hibernate), auditoria de entidades (Envers), mapper Entity↔DTO, gateway |

Flash: enunciado fala em **migrations**, **versionamento de banco/schema**, **scripts SQL versionados**, **histórico de migrações** → **Flyway**.

Flyway **não** é: Hibernate, JPA, Envers, MapStruct, Swagger, Eureka, Zuul, Keycloak.

### 1.2 Por que versionar o banco? (problema cobrável)

Em microsserviços / APIs Spring (stack PDPJ), o código da aplicação evolui no Git. O **schema** do PostgreSQL (ou H2 em testes) também precisa evoluir **junto**, sem “alguém rodou um ALTER na mão em produção e ninguém sabe”.

Problemas sem migração versionada:

1. Ambientes **divergem** (dev ≠ homolog ≠ prod).  
2. Deploy quebra porque a Entity JPA espera coluna que o BD não tem.  
3. Rollback / auditoria de mudanças de schema fica **opaca**.  
4. Em time: ninguém sabe **qual** script já rodou onde.

Flyway (e ferramentas da mesma família) trata o schema como **artefato versionado**, alinhado ao ciclo de release.

### 1.3 Modelo mental: migrations versionadas

Ideia cobrável (não precisa decorar CLI linha a linha):

```
Código da app (Git)  +  Scripts Flyway (V1, V2, V3…)
                              │
                              ▼  (startup / migrate)
                    Banco (PostgreSQL / H2)
                              │
                    Tabela de histórico (ex.: flyway_schema_history)
                    → quais versões já foram aplicadas
```

Pontos de prova:

| Conceito | Leitura |
|----------|---------|
| **Migration** | Unidade de mudança de schema (e às vezes dados de apoio) |
| **Versão** | Ordem: V1 antes de V2; não “pula” no fluxo normal |
| **Checksum / histórico** | Flyway registra o que já aplicou; reexecução controlada |
| **Baseline** | Marca estado inicial já existente (BD legado) sem reaplicar tudo do zero |
| **Repeatable** | Scripts que podem reaplicar sob condição (ex.: views) — só se o enunciado citar |

Convenção clássica de nome (flash): `V<versão>__<descrição>.sql` — dois underscores. Ex.: `V1__criar_tabela_processo.sql`.

### 1.4 O que Flyway faz × o que JPA/Hibernate fazem

Tabela **ouro** anti-confusão (FCC adora inverter):

| | **Flyway** | **JPA / Hibernate** |
|---|------------|---------------------|
| Pergunta | Como **evoluir o schema** de forma versionada? | Como **mapear** objetos Java ↔ tabelas e persistir dados? |
| Artefato típico | Scripts SQL (migrations) | Entities, repositories, EntityManager / Session |
| Foco | **DDL** (e dados de migração) sob controle de versão | **CRUD** / consultas / ciclo de vida da Entity |
| Momento típico | Deploy / migrate (antes ou na subida da app) | Runtime da aplicação |
| Histórico de schema | Tabela de migrations (ex. `flyway_schema_history`) | Não é o papel principal do ORM |
| Auditoria de entidades | Não (isso é **Envers**, se cobrado) | Envers versiona **entidades/dados** de auditoria |

Frase de prova: **Flyway versiona o schema; Hibernate persiste entidades; Envers audita mudanças em entidades.**

### 1.5 Flyway na stack PDPJ (encaixe)

Edital (Arquitetura PDPJ-Br) cita, lado a lado: **persistência; JPA 2.0; Hibernate 5+; Hibernate Envers; Flyway** (+ noções Jakarta Persistence 3.2 / Hibernate ORM 7.x).

Leitura cobrável do “kit persistência”:

```
Cliente HTTP (JSON)
      │
      ▼
 Controller (DTO) ← MapStruct → Entity
      │
      ▼
 JPA / Hibernate  →  PostgreSQL (prod) / H2 (teste)
      │
 Flyway: garante que o schema no BD está na versão certa
 Envers (se usado): auditoria de entidades — outro papel
```

| Peça | Papel na persistência PDPJ |
|------|----------------------------|
| **JPA** | API/especificação de persistência |
| **Hibernate** | Implementação ORM (mapeamento, SQL sob o capô) |
| **Envers** | Auditoria/versionamento de **entidades** |
| **Flyway** | Migração/versionamento de **schema** |
| **PostgreSQL** | BD típico de serviço |
| **H2** | BD embutido / testes (schema ainda pode ser migrado) |

Flyway **não** substitui JPA/Hibernate: a app continua mapeando Entity ↔ tabela. Flyway **alinha** o schema físico com o que o ORM espera.

### 1.6 `ddl-auto` / Hibernate “criar tabelas sozinho” × Flyway

Hibernate pode, em alguns perfis, **gerar/atualizar** schema a partir das Entities (`ddl-auto` / `hbm2ddl` — conceitos, não decorar enums).

Contraste cobrável:

| Abordagem | Quando a FCC “gosta” na leitura corporativa/microsserviços |
|-----------|--------------------------------------------------------------|
| Só ORM gera schema | Rápido em protótipo; **fraco** para controle fino, auditoria de mudança e prod madura |
| **Flyway (migrations explícitas)** | Schema **explícito**, reviewável, versionado com o código — padrão de prova para “evolução controlada” |

Armadilha: dizer que “com JPA não precisa de Flyway”. Em stack edital/PDPJ, **ambos aparecem**: ORM para persistência de objetos; Flyway para **governar** o schema.

Outra armadilha: dizer que Flyway **é** o Hibernate ou que “Flyway mapeia Entities”.

### 1.7 Fluxo típico Spring Boot + Flyway (visão de prova)

Sem tutorial de YAML:

1. Dependência Flyway no projeto.  
2. Scripts em pasta convencional (ex.: `db/migration`).  
3. Na subida (ou comando migrate): Flyway lê o histórico, aplica **só o que falta**, na ordem.  
4. Depois (ou em paralelo conceitual): JPA/Hibernate usa o schema já migrado.  
5. Nova feature que precisa de coluna → **nova migration** no Git + Entity atualizada.

Pontos cobráveis:

- Migrations **já aplicadas** não devem ser reescritas à toa em ambientes já migrados (quebra checksum / disciplina de equipe).  
- Nova mudança = **nova versão**, não editar V1 em produção.  
- Falha na migration pode **impedir** a aplicação de subir “no escuro” com schema errado — isso é **desejável** em disciplina de deploy.

### 1.8 Flyway × Envers (anti-confusão ouro)

| | **Flyway** | **Hibernate Envers** |
|---|------------|----------------------|
| O que versiona | **Schema** (estrutura: tabelas, colunas, índices…) | **Entidades / dados** (histórico de alterações em objetos auditados) |
| Pergunta típica | “Como controlamos a evolução do DDL?” | “Como auditamos quem mudou o que na Entity?” |
| Artefato | Scripts + tabela de histórico de migrations | Tabelas de auditoria / revisões Envers |
| Erro FCC | Chamar Flyway de “auditoria de Entity” | Chamar Envers de “migration de schema” |

Ambos podem coexistir no mesmo serviço: Flyway cria/evolui tabelas (inclusive as de auditoria, se modeladas assim); Envers grava histórico de negócio/persistência.

### 1.9 PostgreSQL, H2 e scripts

| BD | Leitura com Flyway |
|----|--------------------|
| **PostgreSQL** | Alvo comum em serviços PDPJ; scripts SQL alinhados ao dialeto |
| **H2** | Testes / embutido; migrations ainda fazem sentido para **mesmo caminho** de schema que a app espera |
| Cuidado | SQL específico demais de um dialeto pode falhar no outro — conceito de **portabilidade** dos scripts |

Flyway **não** é o banco; é o **mecanismo** que aplica mudanças no banco.

### 1.10 O que Flyway **não** faz (lista seca)

1. Não é o ORM (não substitui JPA/Hibernate no mapeamento Entity↔tabela).  
2. Não é Envers (não é auditoria de entidades).  
3. Não é MapStruct / Swagger / Eureka / Zuul / Keycloak.  
4. Não “descobre” microsserviços nem roteia na borda.  
5. Não documenta OpenAPI.  
6. Não define sozinho regras de negócio da aplicação.  
7. Não elimina a necessidade de Entity/Repository quando a app usa JPA.

### 1.11 Armadilhas FCC (lista de ouro)

1. Trocar Flyway e Hibernate de papel (schema × persistência de objetos).  
2. Trocar Flyway e Envers (schema × auditoria de Entity).  
3. Afirmar que Flyway mapeia Entity↔DTO (MapStruct).  
4. Afirmar que `ddl-auto` e Flyway são a mesma coisa.  
5. Dizer que Flyway substitui PostgreSQL/H2.  
6. Dizer que migrations são “só para monólito” — microsserviços também versionam schema **por serviço**.  
7. Achar que editar migration antiga já aplicada é o fluxo normal de evolução.  
8. Confundir versionamento de **schema** com versionamento de **API** (Swagger) ou de **código** (Git) — Git versiona o arquivo da migration; Flyway aplica e registra no BD.  
9. Inventar que Flyway é Service Discovery ou gateway.  
10. Reduzir Flyway a “backup do banco”.

### 1.12 Mini-síntese (cartão mental)

> **Flyway** = **migrations versionadas** do **schema** (DDL sob controle).  
> **JPA/Hibernate** = persistência **objeto ↔ tabela**.  
> **Envers** = auditoria de **entidades**.  
> Na PDPJ: os três aparecem no kit de persistência; papéis **complementares**, não intercambiáveis.  
> Evolução madura: nova coluna → **nova migration** + Entity alinhada — não “só confiar no ORM criar sozinho” em prod.

---

## 2. Questões de treino (1–15)

**1.** No eixo de persistência da stack PDPJ citada no edital, o Flyway desempenha prioritariamente o papel de:

A) API Gateway / ponto de entrada único  
B) Mapper compile-time Entity ↔ DTO  
C) Ferramenta de migração e versionamento de schema de banco  
D) Service Discovery / registry de instâncias  
E) Documentação interativa OpenAPI  

**2.** A principal diferença cobrável entre Flyway e Hibernate (ORM) é que o Flyway:

A) Substitui o Eureka no heartbeat  
B) Ocupa-se de aplicar/evoluir o **schema** via migrations versionadas; o Hibernate mapeia e persiste **entidades**  
C) É o nome comercial do Swagger UI  
D) Documenta endpoints REST no navegador  
E) Registra réplicas de microsserviço  

**3.** Em enunciado que fala em scripts `V1__...sql`, `V2__...sql` aplicados em ordem com histórico no banco, a ferramenta alinhada é:

A) MapStruct  
B) Zuul  
C) Flyway  
D) Keycloak  
E) Eureka  

**4.** Assinale a associação **correta**:

A) Flyway = auditoria de Entity; Envers = migration de schema  
B) Flyway = schema versionado; Hibernate = ORM/persistência de objetos  
C) Flyway = discovery; Hibernate = gateway  
D) Flyway = MapStruct; Envers = Swagger  
E) Flyway e Hibernate são o mesmo jar com o mesmo papel  

**5.** Usar Flyway em microsserviços Spring ajuda sobretudo a:

A) Eliminar a necessidade de JSON na API  
B) Manter o schema de cada serviço **evoluído de forma controlada e rastreável** entre ambientes  
C) Substituir o API Gateway  
D) Documentar try-it no lugar do Swagger  
E) Registrar heartbeat no Eureka  

**6.** Sobre deixar **somente** o Hibernate gerar/atualizar o schema (`ddl-auto` / abordagem só ORM), em contraste com Flyway, é correto afirmar que:

A) É a única forma permitida pelo edital PDPJ  
B) Pode bastar em protótipo, mas migrations explícitas (Flyway) oferecem versionamento e controle mais adequados a evolução em equipe/produção  
C) É sinônimo estrito de Service Discovery  
D) Substitui integralmente o PostgreSQL  
E) Torna o Flyway conceitualmente impossível em qualquer projeto  

**7.** Qual afirmação é **incorreta**?

A) Flyway aplica migrations versionadas ao banco  
B) Flyway tipicamente registra o histórico do que já foi aplicado  
C) Flyway é, por definição, a implementação JPA que mapeia Entity ↔ tabela no lugar do Hibernate  
D) Flyway convive com JPA/Hibernate na mesma aplicação  
E) Nova mudança de schema costuma ser nova migration, não reescrita caótica do passado já aplicado  

**8.** No fluxo mental cobrado (serviço Spring + persistência):

A) Cliente → Eureka Dashboard → Flyway como gateway → MapStruct como BD  
B) Flyway alinha o schema; JPA/Hibernate persiste Entities; MapStruct (se houver) mapeia Entity↔DTO na borda  
C) Swagger UI roteia na borda; Flyway registra heartbeat  
D) Zuul gera DDL; Flyway documenta OpenAPI  
E) Envers substitui obrigatoriamente o Flyway em todo DDL  

**9.** “Ferramenta que versiona e aplica scripts de evolução do schema do banco.” Descreve:

A) Eureka  
B) Zuul  
C) Flyway  
D) Swagger UI  
E) MapStruct  

**10.** Flyway e Hibernate Envers convivem porque:

A) São mutuamente excludentes no edital  
B) Flyway versiona **schema**; Envers audita mudanças em **entidades** — papéis distintos  
C) Ambos são registries de instâncias  
D) Envers aplica V1/V2 SQL; Flyway grava revisões de Entity  
E) Só um deles pode existir por microsserviço  

**11.** Flyway **não** se destina a:

A) Criar tabelas via migration versionada  
B) Evoluir colunas/índices de forma ordenada  
C) Atuar como catálogo dinâmico de instâncias (papel do Eureka)  
D) Registrar histórico de migrations aplicadas  
E) Apoiar o mesmo caminho de schema em ambientes diferentes  

**12.** Em prova, confundir Flyway com Envers costuma errar porque:

A) Ambos documentam apenas SOAP  
B) Um controla evolução de **estrutura** do BD; o outro audita **entidades**/dados versionados no sentido Envers  
C) Não há diferença cobrável  
D) Flyway é gateway; Envers é discovery  
E) Envers só existe sem banco  

**13.** A frase mais alinhada ao papel do Flyway na stack PDPJ é:

A) “Gera mappers Entity↔DTO na compilação”  
B) “Aplica migrations versionadas para manter o schema alinhado ao ciclo de entrega”  
C) “É o dialect SQL exclusivo do Hibernate 5”  
D) “Substitui OAuth2 e Keycloak”  
E) “Só funciona se houver Zuul na frente”  

**14.** Qual linha resume melhor o cartão mental deste subtópico?

A) Flyway = docs; Hibernate = mapper; Envers = gateway  
B) Flyway = schema versionado (migrations); Hibernate/JPA = ORM; Envers = auditoria de Entity  
C) Flyway elimina Entity e Repository  
D) Flyway = Eureka + Zuul  
E) Flyway = apenas backup full do PostgreSQL  

**15.** Afirmar que “Flyway mapeia Entities e substitui o Hibernate” é:

A) Correto em toda prova FCC  
B) Incorreto: mapeamento/persistência de objetos associa-se a JPA/Hibernate; Flyway migra/versiona schema  
C) Correto somente com H2  
D) Correto porque ambos são Spring Cloud Netflix  
E) Correto se o gateway for Zuul  

---

### Gabarito comentado (treino 1–15)

1. **C** — Migração/versionamento de schema.  
2. **B** — Schema (Flyway) × entidades/ORM (Hibernate).  
3. **C** — Convenção V1/V2 + histórico = Flyway.  
4. **B** — Tabela anti-inversão Flyway × Hibernate.  
5. **B** — Schema controlado por serviço/ambiente.  
6. **B** — Só ORM frágil em prod; Flyway = controle explícito.  
7. **C** — ORM/JPA ≠ papel do Flyway.  
8. **B** — Schema → ORM → (DTO via MapStruct).  
9. **C** — Definição cobrável de Flyway.  
10. **B** — Schema × auditoria de Entity.  
11. **C** — Catálogo = Eureka.  
12. **B** — Estrutura ≠ auditoria Envers.  
13. **B** — Migrations alinhadas ao deploy.  
14. **B** — Síntese do arquivo 02.  
15. **B** — Não troca papéis com Hibernate.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT19 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 40 de 60)

Flyway é uma biblioteca de migração de banco de dados que usa uma tabela para rastrear o estado do banco de dados, cujo nome padrão é

A) `flyway_ledger_history`.

B) `flyway_schema_history`.

C) `flw_history`.

D) `flyway_state_history`.

E) `flw_database_history`.


**17.** (FCC | TRT14 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60)

O uso da anotação `@Audited` antes do nome da classe de entidade do banco de dados, em uma aplicação que utiliza JPA, sinaliza ao

A) Flyway que a entidade já foi auditada.

B) Hibernate Envers que a entidade será alvo de auditoria.

C) Map Struct que a entidade já foi auditada.

D) Spring Cloud que a entidade registra logs de transações com o banco de dados.

E) Service Discovery o caminho do arquivo de auditoria.


**18.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22) *[aproximação temática]*

*Flyway nos distratores (função × trocada).*

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.

B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.

C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.

D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.

E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.


**19.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 51 de 60)

Um Tribunal Regional do Trabalho está desenvolvendo um sistema de gestão de processos judiciais que inclui um módulo de auditoria para rastrear mudanças nos registros dos processos. É essencial que qualquer alteração nos dados dos processos judiciais (por exemplo, petições, decisões, movimentações) seja auditada para garantir a integridade e a transparência. Um Analista de TI decide utilizar Hibernate Envers para atender aos requisitos desse módulo de auditoria e, para isso, deve

A) configurar Hibernate Envers para criar uma tabela de auditoria que registre todas as alterações em entidades anotadas, incluindo quem fez a alteração e quando foi feita.

B) usar Hibernate Envers para criar automaticamente backups diários de todas as tabelas do banco de dados.

C) utilizar Hibernate Envers para migrar dados auditáveis entre diferentes esquemas de banco de dados de forma transparente.

D) implementar Hibernate Envers para validar os dados de auditoria dos processos judiciais antes de serem inseridos no banco de dados.

E) configurar Hibernate Envers para enviar notificações à auditoria, por e-mail, a cada alteração de dados no banco de dados.


**20.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60) *[aproximação temática]*

*Controle de schema vs migrações.*

Uma equipe de desenvolvimento está utilizando Spring Boot para criar uma API RESTful. Durante os testes, observaram que a aplicação demora a inicializar, principalmente devido à configuração do banco de dados. Para melhorar o tempo de inicialização, decidiram desabilitar a criação automática do esquema de banco de dados ao iniciar a aplicação. A configuração, no `application.properties`, que desabilita a criação automática do esquema de banco de dados pelo Spring Boot é a

A) `spring.jpa.hibernate.ddl-auto=none`

B) `spring.datasource.database-auto=false`

C) `spring.jpa.db-sql=disable`

D) `spring.jpa.properties.hibernate.db-auto=disable`

E) `spring.jpa.create-database-init=false`


**21.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60) *[aproximação temática]*

Um Analista de um Tribunal necessita habilitar o Hibernate Envers em uma entidade. Para isso, ele deve

A) utilizar o método `Session.saveOrUpdate()` para a entidade.

B) anotar a entidade com `@Audited`.

C) configurar `hibernate.envers.enabled` como `true` no arquivo de configuração.

D) estender a classe `AuditedEntity` fornecida pelo Hibernate Envers.

E) configurar o `audit-log` no arquivo `persistence.xml`.


**22.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60) *[aproximação temática]*

Uma analista está desenvolvendo um sistema usando JPA e deseja configurar a utilização do framework Hibernate como provedor de persistência, por meio da instrução `<provider>org.hibernate.ejb.HibernatePersistence</provider>` que deve ser colocada no arquivo

A) `hibernate.xml`

B) `persistence-unit.xml`

C) `persistence.xml`

D) `orm-database-config.xml`

E) `server.xml`


**23.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 22 de 60) *[aproximação temática]*

Considere que exista um banco de dados PostgreSQL aberto e funcionando em condições ideais, e que a tabela t_processos exista e esteja corretamente preenchida com valores adequados. Neste contexto, observe a função abaixo. CREATE FUNCTION atualizar_status_processo(num_processo VARCHAR) RETURNS INTEGER AS $$ DECLARE total INTEGER; BEGIN UPDATE t_processos SET status_processo = 'Encerrado' WHERE numero_processo = num_processo; SELECT COUNT(*) INTO total FROM t_processos WHERE status_processo = 'Encerrado'; RETURN total; END; $$ LANGUAGE plpgsql; Em condições ideais, esta função seria corretamente chamada utilizando-se o comando:

A) `CALL atualizar_status_processo('12345-67.2023.5.01.0001');`

B) `EXECUTE atualizar_status_processo('12345-67.2023.5.01.0001');`

C) DO $$ BEGIN PERFORM atualizar_status_processo('12345-67.2023.5.01.0001'); END; $$ LANGUAGE plpgsql;

D) `SELECT atualizar_status_processo('12345-67.2023.5.01.0001');`

E) `EXEC atualizar_status_processo '12345-67.2023.5.01.0001';`


**24.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60) *[aproximação temática]*

Nos projetos da Plataforma Digital do Poder Judiciário PDPJ-Br,

A) os ambientes de deployment estão instalados em clusters Kubernetes, gerenciados por meio da ferramenta open source de orquestração de containers Rancher.

B) a aplicação deve possuir os arquivos `yanl`, para que o deploy ocorra de forma automática por meio de pipelines de CI/CD (Continuous Delivery e Continuous Integration) executados pelo GitLab do STF (Supremo Tribunal Federal).

C) a aplicação deve possuir um arquivo `Rancherfile`, especificando o modo pelo qual ela deve ser empacotada.

D) deve haver o arquivo `.gitlab-ci.xml` definindo a pipeline de deploy automático.

E) deve haver, na pasta `kubernets`, o arquivo de configuração do ingress da aplicação, que é obrigatório.


**25.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 23) *[aproximação temática]*

O JPA é uma especificação da plataforma Java que define um padrão para mapeamento objeto-relacional (ORM) e gerenciamento de persistência de dados. No JPA, a forma padrão de realizar o mapeamento de objetos Java (POJOs) para tabelas de bancos de dados relacionais é por meio de

A) configurações XML como `persistence.xml`, `entity.xml` e `mapping.xml`.

B) anotações como `@Entity`, `@Id`, `@Table` e `@Column`.

C) interfaces como `EntityFactory`, `QueryBuilder` e `PersistenceUnit`.

D) classes como `HibernateSession`, `EntityManagerFactory` e `DataMapper`.

E) anotações como `@Database`, `@Primary`, `@Relation` e `@Field`.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **B** | FCC | TRT19 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 40 de 60 |
| 17 | **B** | FCC | TRT14 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60 |
| 18 | **E** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22 |
| 19 | **A** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 51 de 60 |
| 20 | **A** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 21 | **B** \* | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60 |
| 22 | **C** \* | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60 |
| 23 | **D** \* | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 22 de 60 |
| 24 | **A** \* | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60 |
| 25 | **B** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 23 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt19-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q40
- Q17: https://www.quizinho.com.br/questao/fcc-trt14-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q29
- Q18: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22
- Q19: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q51
- Q20: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q38
- Q21: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q54
- Q22: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q41
- Q23: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q22
- Q24: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q29
- Q25: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q23

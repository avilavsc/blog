+++
title = '01 — JPA 2.0 + Hibernate 5+'
date = '2026-10-20'
draft = false
weight = 1
tags = ['semana-02', 'jpa', 'hibernate']
categories = ['estudo']
+++
# JPA 2.0 + Hibernate 5+ · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Persistência PDPJ (JPA / Hibernate / Envers visão)  
**Escopo deste arquivo:** JPA 2.0 e Hibernate 5+ como eixo cobrável do edital PDPJ — entidades, mapeamento ORM básico, persistence context / EntityManager, ciclo de vida, relacionamentos, JPQL visão, Envers em alto nível. Menção leve a Flyway só como peça vizinha do stack.  
**Fora de escopo aqui:** Jakarta Persistence 3.2 + Hibernate ORM 7.x em profundidade (arquivo 02); Flyway aprofundado (arquivo 03); PostgreSQL/H2 e SQL/PL-SQL (arquivos 04–05); SSO/Keycloak/OAuth2 (arquivo 06).

---

## 1. Material

### 1.1 Por que isso cai na PDPJ / edital

No edital (Arquitetura PDPJ-Br), a persistência aparece com nomes explícitos: **JPA 2.0**, **Hibernate 5+**, **Hibernate Envers**, **Flyway**, além de **noções** de Jakarta Persistence 3.2 e Hibernate ORM 7.x (respeitando versões dos normativos).

Leitura cobrável:

| Peça | Papel em prova |
|------|----------------|
| **JPA 2.0** | Especificação (API) de persistência Java — o “contrato” |
| **Hibernate 5+** | Implementação ORM mais citada no stack PDPJ do edital |
| **Envers** | Auditoria de entidades (histórico de revisões) |
| **Flyway** | Migração versionada de schema (vizinho; detalhe no arq. 03) |

Flash: **JPA = especificação**; **Hibernate = implementação ORM**. Em prova, “usar JPA” não significa abandonar o Hibernate — o Hibernate **implementa** a JPA (e tem APIs nativas extras).

### 1.2 ORM em uma frase

**ORM (Object-Relational Mapping)** mapeia **objetos** (classes Java) para **relações** (tabelas/colunas) e vice-versa, reduzindo JDBC “na mão” para o CRUD típico.

| Mundo OO | Mundo relacional |
|----------|------------------|
| Classe / entidade | Tabela |
| Instância | Linha (registro) |
| Atributo | Coluna |
| Relacionamento (@ManyToOne etc.) | FK / tabela de junção |

Erro clássico: achar que ORM **elimina** a necessidade de saber SQL. Em concurso e na prática, SQL continua cobrável (semana 2, arquivos 04–05). ORM **abstrai**; não substitui o modelo relacional na cabeça do analista.

### 1.3 JPA 2.0 × Hibernate 5+

| Aspecto | JPA 2.0 | Hibernate 5+ |
|---------|---------|--------------|
| Natureza | **Especificação** (javax.persistence.*) | **Implementação** (+ extensões) |
| API típica | `EntityManager`, anotações `@Entity`, JPQL | Implementa JPA; `Session` nativa; dialects; cache 2º nível etc. |
| Portabilidade | Código escrito contra a API JPA tende a ser mais portável entre providers | Vendor específico; rico em recursos |
| No edital PDPJ | Versão **2.0** citada | **5+** citada |

Âncoras mentais:

1. Código tipicamente “fala” com **EntityManager** (JPA).  
2. Por baixo, no stack cobrado, costuma rodar **Hibernate**.  
3. Anotações JPA (`@Entity`, `@Id`, `@Table`…) são o vocabulário principal de prova.  
4. Recursos só-Hibernate existem, mas a FCC prioriza o **núcleo JPA + papel do Hibernate como ORM**.

### 1.4 Entidade (entity)

**Entidade** = classe cujo estado é persistido; em JPA, marcada com **`@Entity`**.

Requisitos/ideias cobráveis:

1. **Identidade:** chave (`@Id`) — em geral surrogate (`Long`, UUID…) ou natural, conforme modelo.  
2. **Construtor sem argumentos** (exigência clássica da JPA para instanciação pelo provider).  
3. Estado mapeado em campos/propriedades (`@Column`, defaults).  
4. Não confundir entidade com **DTO** / projeção de API: entidade vive no **modelo de persistência**; DTO na borda (MapStruct na semana 1).

Anotações básicas frequentes:

| Anotação | Ideia |
|----------|--------|
| `@Entity` | Classe é entidade |
| `@Table(name=…)` | Nome da tabela (se diferente do default) |
| `@Id` | Chave primária |
| `@GeneratedValue` | Estratégia de geração (IDENTITY, SEQUENCE, TABLE, AUTO) |
| `@Column` | Nome, nullable, length, unique… |
| `@Transient` | Atributo **não** persistido |
| `@Enumerated` | Enum → ORDINAL ou STRING |
| `@Temporal` / tipos data | Mapeamento de data/hora (visão; APIs modernas variam) |

### 1.5 Persistence Unit e Persistence Context

Dois conceitos que a banca adora misturar:

| Conceito | O que é | Analogia útil |
|----------|---------|---------------|
| **Persistence Unit** | Conjunto de classes + config de um “banco/unidade” (ex.: `persistence.xml` / Spring Boot `spring.jpa.*`) | A “fábrica” / configuração do contexto de persistência |
| **Persistence Context** | Conjunto de instâncias de entidade **gerenciadas** em um momento (1ª nível de cache) | A “mesa de trabalho” das entidades na sessão |

**EntityManager** é a API principal para interagir com o persistence context: `persist`, `find`, `merge`, `remove`, `createQuery` etc.

Flash:

> **Unit** = configuração / escopo de mapeamento.  
> **Context** = entidades gerenciadas agora.  
> **EntityManager** = porta de entrada da API JPA.

### 1.6 Ciclo de vida da entidade

Estados cobráveis:

| Estado | Significado |
|--------|-------------|
| **New / transient** | Instância nova; ainda **não** associada ao persistence context |
| **Managed** | Associada ao context; mudanças rastreadas (dirty checking) tipicamente sincronizam no flush/commit |
| **Detached** | Já foi persistida, mas **não** está mais no context atual |
| **Removed** | Marcada para remoção; refletida no BD no flush/commit |

Operações típicas:

| Método | Efeito típico (visão de prova) |
|--------|--------------------------------|
| `persist(e)` | Transient → Managed (agenda INSERT) |
| `find(Class, id)` | Busca; retorna Managed (ou null) |
| `merge(e)` | Copia estado de detached/new para Managed no context |
| `remove(e)` | Managed → Removed |
| `flush()` | Sincroniza o context com o BD **sem** necessariamente encerrar a transação |
| `clear()` / fechar EM | Entidades deixam de ser managed (viram detached) |

Armadilhas:

- Alterar campo de entidade **managed** dentro de transação → UPDATE implícito no flush (não precisa chamar `persist` de novo).  
- Alterar **detached** e esperar sync automático → **não** acontece; use `merge` (ou reanexe conforme desenho).  
- `persist` em detached com id já existente → comportamento problemático / erro típico de uso.

### 1.7 Relacionamentos ORM

Cardinalidades clássicas:

| Anotação | Significado |
|----------|-------------|
| `@OneToOne` | 1 ↔ 1 |
| `@OneToMany` / `@ManyToOne` | 1 ↔ N (lado N costuma ter a FK) |
| `@ManyToMany` | N ↔ N (tabela de junção) |

Ideias cobráveis:

- **Dono do relacionamento (owning side):** lado que possui a FK / que “manda” no mapeamento.  
- **`mappedBy`:** lado inverso (não dono) em associações bidirecionais.  
- **Cascade:** propagar `persist`/`remove` etc. (cuidado: cascade agressivo em prova = distrator perigoso se enunciado falar em exclusão em cascata “sempre”).  
- **Fetch:** `EAGER` vs `LAZY` — quando carregar o associado. Lazy é comum em coleções; EAGER em excesso → risco de performance / joins pesados (visão).

| Fetch | Ideia |
|-------|--------|
| **LAZY** | Carrega sob demanda (proxy); cuidado fora da sessão (LazyInitializationException — conceito) |
| **EAGER** | Carrega junto; pode trazer mais dados do que o necessário |

### 1.8 JPQL (visão)

**JPQL** = linguagem de consulta orientada a **entidades** (não a tabelas SQL cruas), via `EntityManager.createQuery`.

Contraste rápido:

| | JPQL | SQL nativo |
|---|------|------------|
| Alvo | Entidades / atributos | Tabelas / colunas |
| Portabilidade | Mais alinhada ao modelo JPA | Depende do dialeto do BD |
| Uso | Consultas ORM tipadas ao domínio | Relatórios, SQL específico, otimizações |

Em prova: JPQL **não** é SQL puro; nomes de entidade/campos seguem o **modelo Java**, não necessariamente o nome físico da tabela (embora possam coincidir).

### 1.9 Transações e Unit of Work (visão)

Persistência JPA em aplicações típicas (Spring) vive **dentro de transações**:

- Abre-se transação → operações no EntityManager → commit (flush + commit) ou rollback.  
- Sem transação adequada, `persist`/`flush` podem falhar ou não ter o efeito esperado conforme o ambiente.  
- Ideia **Unit of Work**: acumular mudanças no persistence context e aplicar de uma vez no flush/commit.

Não é preciso decorar anotações Spring (`@Transactional`) como se fossem JPA pura — mas a FCC pode misturar stack. Lembre: **contexto + transação** andam juntos na prática.

### 1.10 Hibernate Envers (alto nível)

**Hibernate Envers** = módulo de **auditoria** que versiona entidades: quem/quando/o quê mudou, em revisões.

Ideias cobráveis (sem virar manual):

1. Entidades auditadas geram **histórico** (tabelas de auditoria / revisões).  
2. Objetivo: **rastreabilidade** e consulta a estados passados — alinhado a sistemas judiciais/administrativos.  
3. Não confundir Envers com:  
   - **Flyway** (migração de schema);  
   - **log de aplicação** genérico;  
   - **backup** de banco.  
4. Envers é peça **Hibernate** citada no edital PDPJ junto a JPA/Hibernate.

Flash: **Envers = auditoria/versionamento de entidades**; **Flyway = versionamento de schema**.

### 1.11 Flyway — menção leve (vizinho do stack)

No mesmo bloco de persistência do edital aparece **Flyway**: ferramenta de **migrações versionadas** de banco (scripts SQL ou Java ordenados, aplicados de forma controlada).

Neste arquivo, basta:

- Saber que faz parte do **stack de persistência PDPJ** do edital.  
- Não misturar com Envers nem com “ORM”.  
- Detalhe (baseline, checksum, `V1__…`, repair) → **arquivo 03**.

### 1.12 Contexto PDPJ / microsserviços (ponte)

Na semana 1: microsserviços tendem a **dados por serviço**. JPA/Hibernate entram como **tecnologia de persistência dentro do serviço**, não como “banco único nacional da PDPJ”.

Leitura correta:

- Serviço A com seu modelo JPA + BD (ex.: PostgreSQL).  
- Serviço B não faz JOIN no schema do A; integra via API/eventos.  
- H2 aparece no edital como opção típica de **teste/dev** leve (aprofundamento no arq. 04).

### 1.13 O que a FCC tipicamente testa neste tema

- JPA = **especificação**; Hibernate = **implementação ORM**.  
- `@Entity`, `@Id`, ciclo de vida (new/managed/detached/removed).  
- Persistence **context** × persistence **unit**.  
- Relacionamentos e ideia de owning side / fetch.  
- JPQL ≠ SQL nativo (orientação a entidades).  
- Envers = **auditoria** de entidades (não migração).  
- Flyway = migração de schema (só noção aqui).  
- Não confundir entidade com DTO; não achar que ORM dispensa SQL.

### 1.14 Mini-síntese (cartão mental)

> **JPA 2.0** = API/especificação de persistência · **Hibernate 5+** = ORM que a implementa · **Entity** + **Persistence Context** + ciclo de vida · relacionamentos/fetch · **Envers** audita entidades · **Flyway** versiona schema (vizinho).

---

## 2. Questões de treino (1–15)

**1.** No eixo de persistência do edital PDPJ-Br, a leitura mais adequada de JPA 2.0 é:

A) Um banco de dados relacional proprietário do CNJ  
B) Uma especificação/API Java de persistência (mapeamento objeto-relacional)  
C) Sinônimo estrito de Flyway  
D) Apenas um servidor de identidade (SSO)  
E) Um protocolo de rede equivalente a HTTP  

**2.** Hibernate 5+, no mesmo contexto, é corretamente associado a:

A) Implementação ORM amplamente usada, alinhada à JPA (com extensões próprias)  
B) Ferramenta exclusiva de migração de schema  
C) Substituição obrigatória de qualquer API REST  
D) Protocolo OAuth2  
E) Formato de documentação OpenAPI  

**3.** A relação típica cobrável entre JPA e Hibernate é:

A) São o mesmo produto com nomes intercambiáveis em qualquer frase  
B) JPA é a especificação; Hibernate é uma implementação (provider) ORM  
C) Hibernate é a especificação; JPA é apenas um dialeto SQL do PostgreSQL  
D) JPA proíbe o uso de Hibernate  
E) Hibernate só existe fora da JPA e nunca a implementa  

**4.** Uma classe marcada com `@Entity` representa, em JPA:

A) Um DTO de borda de API, por definição  
B) Uma entidade cujo estado pode ser persistido conforme o mapeamento ORM  
C) Obrigatoriamente um microsserviço completo  
D) Um script Flyway  
E) Um token OAuth2  

**5.** Persistence **context**, em JPA, corresponde melhor a:

A) O conjunto de entidades **gerenciadas** em determinado momento (1º nível)  
B) O protocolo TCP do banco  
C) Apenas o arquivo `README` do repositório  
D) O gateway Zuul  
E) A política de SSO do tribunal  

**6.** Persistence **unit** refere-se principalmente a:

A) Uma única linha de uma tabela  
B) A unidade de configuração/mapeamento (classes + settings) de persistência  
C) Um único método `hashCode`  
D) O certificado TLS do servidor  
E) A RFC 6749  

**7.** Uma instância **new/transient** é aquela que:

A) Já está gerenciada e sincronizada automaticamente sem qualquer persist  
B) Existe na memória, ainda **não** associada ao persistence context  
C) Foi removida do banco e não pode mais ser mencionada  
D) É sempre um enum `@Enumerated`  
E) Equivale a um registro já commitado e detached ao mesmo tempo, por definição  

**8.** Chamar `persist` em uma entidade transient tipicamente:

A) Remove a tabela do banco  
B) Associa a entidade ao context (managed) e agenda a inserção  
C) Converte a aplicação em monólito  
D) Emite um token Keycloak  
E) Substitui JPQL por GraphQL obrigatoriamente  

**9.** Dirty checking, no contexto de entidade **managed**, indica que:

A) Mudanças em atributos rastreados podem gerar UPDATE no flush/commit sem novo `persist`  
B) O banco nunca pode ser atualizado  
C) Só funciona com arquivos CSV  
D) Exige obrigatoriamente Envers em toda entidade  
E) Proíbe o uso de transações  

**10.** Em relacionamentos JPA, `mappedBy` costuma indicar:

A) O lado **inverso** (não dono) de uma associação bidirecional  
B) O dialeto SQL do H2  
C) A porta do Eureka  
D) O checksum do Flyway  
E) O grant type do OAuth2  

**11.** Fetch **LAZY** significa, em visão de prova:

A) Carregar o associado sempre imediatamente, sem exceção  
B) Adiar o carregamento do associado até o acesso (sob demanda)  
C) Apagar a entidade automaticamente  
D) Migrar o schema com Flyway  
E) Auditar com Envers  

**12.** JPQL distingue-se do SQL nativo principalmente porque:

A) Consulta o modelo em termos de **entidades/atributos**, não tabelas/colunas cruas  
B) É um protocolo de autenticação  
C) Só funciona sem EntityManager  
D) É idêntico ao YAML do Kubernetes  
E) Substitui completamente a necessidade de chave primária  

**13.** Hibernate Envers, em alto nível, serve para:

A) Versionar/auditar estados de entidades ao longo do tempo  
B) Fazer deploy de containers Rancher  
C) Descobrir instâncias no Eureka  
D) Documentar APIs no Swagger  
E) Mapear DTOs com MapStruct  

**14.** Confundir Envers com Flyway é erro porque:

A) Ambos são bancos de dados  
B) Envers audita entidades; Flyway versiona migrações de schema  
C) Flyway é especificação JPA; Envers é SQL puro obrigatório  
D) Não há diferença conceitual em prova  
E) Envers só migra schema; Flyway só audita entidades  

**15.** No desenho típico de microsserviços + JPA (ponte semana 1):

A) Todos os serviços devem compartilhar um único schema com escrita cruzada irrestrita como regra definidora  
B) Cada serviço tende a ser dono do seu modelo/persistência; JPA opera dentro desse limite  
C) JPA proíbe qualquer banco relacional  
D) Hibernate obriga ESB central  
E) Entidades JPA substituem API Gateway  

---

### Gabarito comentado (treino 1–15)

1. **B** — JPA = especificação/API de persistência.  
2. **A** — Hibernate = implementação ORM (5+ no edital).  
3. **B** — especificação × provider.  
4. **B** — `@Entity` = entidade persistível.  
5. **A** — context = entidades gerenciadas (1º nível).  
6. **B** — unit = configuração/unidade de persistência.  
7. **B** — transient ainda fora do context.  
8. **B** — persist → managed + INSERT agendado.  
9. **A** — mudanças em managed sincronizam no flush.  
10. **A** — `mappedBy` = lado inverso.  
11. **B** — LAZY = sob demanda.  
12. **A** — JPQL orientada a entidades.  
13. **A** — Envers = auditoria/versionamento de entidades.  
14. **B** — auditoria ≠ migração de schema.  
15. **B** — dados por serviço; JPA no limite do serviço.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60)

Um Analista de um Tribunal necessita habilitar o Hibernate Envers em uma entidade. Para isso, ele deve

A) utilizar o método `Session.saveOrUpdate()` para a entidade.

B) anotar a entidade com `@Audited`.

C) configurar `hibernate.envers.enabled` como `true` no arquivo de configuração.

D) estender a classe `AuditedEntity` fornecida pelo Hibernate Envers.

E) configurar o `audit-log` no arquivo `persistence.xml`.


**17.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60)

Uma analista está desenvolvendo um sistema usando JPA e deseja configurar a utilização do framework Hibernate como provedor de persistência, por meio da instrução `<provider>org.hibernate.ejb.HibernatePersistence</provider>` que deve ser colocada no arquivo

A) `hibernate.xml`

B) `persistence-unit.xml`

C) `persistence.xml`

D) `orm-database-config.xml`

E) `server.xml`


**18.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Uma equipe de desenvolvimento está utilizando Spring Boot para criar uma API RESTful. Durante os testes, observaram que a aplicação demora a inicializar, principalmente devido à configuração do banco de dados. Para melhorar o tempo de inicialização, decidiram desabilitar a criação automática do esquema de banco de dados ao iniciar a aplicação. A configuração, no `application.properties`, que desabilita a criação automática do esquema de banco de dados pelo Spring Boot é a

A) `spring.jpa.hibernate.ddl-auto=none`

B) `spring.datasource.database-auto=false`

C) `spring.jpa.db-sql=disable`

D) `spring.jpa.properties.hibernate.db-auto=disable`

E) `spring.jpa.create-database-init=false`


**19.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 23)

O JPA é uma especificação da plataforma Java que define um padrão para mapeamento objeto-relacional (ORM) e gerenciamento de persistência de dados. No JPA, a forma padrão de realizar o mapeamento de objetos Java (POJOs) para tabelas de bancos de dados relacionais é por meio de

A) configurações XML como `persistence.xml`, `entity.xml` e `mapping.xml`.

B) anotações como `@Entity`, `@Id`, `@Table` e `@Column`.

C) interfaces como `EntityFactory`, `QueryBuilder` e `PersistenceUnit`.

D) classes como `HibernateSession`, `EntityManagerFactory` e `DataMapper`.

E) anotações como `@Database`, `@Primary`, `@Relation` e `@Field`.


**20.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 30)

Hibernate 5.x introduziu suporte nativo para as classes da API de Data e Hora do Java 8 (`java.time`), como

A) `Year`, `Month`, `Day` e `Instant`.

B) `LocalDate`, `LocalTime` e `LocalDateTime`.

C) `DateTime`, `TimeStamp` e `DateInterval`.

D) `DateTimeZone`, `DateTimeImmutable` e `DateTimeOffset`.

E) `NodaTime`, `ZonedDateTime` e `DurationTime`.


**21.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 34 de 60)

Em uma aplicação Java que utiliza JPA, em condições ideais, considere `manager` um objeto válido criado por meio da instrução abaixo.```javaEntityManager manager =Persistence.createEntityManagerFactory("trtPU").createEntityManager();```A instrução `manager.createNamedQuery("Acesso.findByUserSenha", Acesso.class);` irá retornar um objeto do tipo

A) `List<Acesso>`

B) `Acesso`

C) `Query<Acesso>`

D) `TypedQuery<Acesso>`

E) `ArrayList<Acesso>`


**22.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 37 de 60)

Uma equipe de desenvolvimento está implementando uma aplicação Java com persistência de dados utilizando o Hibernate como Object Relational Mapping (ORM). Durante o desenvolvimento, a equipe precisa garantir a geração automática de tabelas no banco de dados com base nas entidades Java mapeadas. No arquivo XML de configuração do Hibernate, a propriedade que garante a geração automática do esquema de banco de dados durante o desenvolvimento é a

A) `<property name="hibernate.dialect" value="org.hibernate.dialect.MySQLDialect.Auto"/>`

B) `<property name="hibernate.create_sql" value="auto"/>`

C) `<property name="hibernate.hbm2ddl.auto" value="update"/>`

D) `<property name="hibernate.connection.url.auto" value="jdbc:mysql://localhost:3306/db"/>`

E) `<property name="hibernate.id.new_generator_mappings" value="auto"/>`


**23.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 33 de 60)

Considere a classe abaixo em uma aplicação que utiliza JPA e Hibernate.```@Entitypublic class Cliente { @Id @GeneratedValue(strategy = GenerationType. I ) private Long id; // atributos e métodos restantes}```Para informar ao provedor de persistência que os valores a serem atribuídos ao identificador único (id) serão gerados pela coluna de autoincremento da tabela do banco de dados, utiliza-se, na lacuna I, o valor

A) `TABLE`

B) `AUTO_GENERATED`

C) `AUTO_INCREMENT`

D) `IDENTITY`

E) `SELF_GENERATED`


**24.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 51 de 60)

Um Tribunal Regional do Trabalho está desenvolvendo um sistema de gestão de processos judiciais que inclui um módulo de auditoria para rastrear mudanças nos registros dos processos. É essencial que qualquer alteração nos dados dos processos judiciais (por exemplo, petições, decisões, movimentações) seja auditada para garantir a integridade e a transparência. Um Analista de TI decide utilizar Hibernate Envers para atender aos requisitos desse módulo de auditoria e, para isso, deve

A) configurar Hibernate Envers para criar uma tabela de auditoria que registre todas as alterações em entidades anotadas, incluindo quem fez a alteração e quando foi feita.

B) usar Hibernate Envers para criar automaticamente backups diários de todas as tabelas do banco de dados.

C) utilizar Hibernate Envers para migrar dados auditáveis entre diferentes esquemas de banco de dados de forma transparente.

D) implementar Hibernate Envers para validar os dados de auditoria dos processos judiciais antes de serem inseridos no banco de dados.

E) configurar Hibernate Envers para enviar notificações à auditoria, por e-mail, a cada alteração de dados no banco de dados.


**25.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22) *[aproximação temática]*

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.

B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.

C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.

D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.

E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **B** | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60 |
| 17 | **C** | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60 |
| 18 | **A** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 19 | **B** | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 23 |
| 20 | **B** | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 30 |
| 21 | **D** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 34 de 60 |
| 22 | **C** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 37 de 60 |
| 23 | **D** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 33 de 60 |
| 24 | **A** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 51 de 60 |
| 25 | **E** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q54
- Q17: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q41
- Q18: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q38
- Q19: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q23
- Q20: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q30
- Q21: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q34
- Q22: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q37
- Q23: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q33
- Q24: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q51
- Q25: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22

*Arquivo 01 · JPA 2.0 + Hibernate 5+ · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 Flyway.*

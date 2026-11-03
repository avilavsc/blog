+++
title = '03 — Jakarta Persistence + Hibernate 7'
date = '2026-10-22'
draft = false
weight = 3
tags = ['semana-02', 'jakarta', 'hibernate']
categories = ['estudo']
+++
# Jakarta Persistence 3.2 + Hibernate ORM 7.x · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — Jakarta Persistence 3.2 + Hibernate ORM 7.x (visão cobrável × legado)  
**Escopo deste arquivo:** contraste **cobrável** entre o **legado do normativo PDPJ** (**JPA 2.0** + **Hibernate 5+**) e a linha moderna **Jakarta Persistence 3.2** + **Hibernate ORM 7.x** — o que a prova pode perguntar sem virar tutorial de migração.  
**Fora de escopo aqui:** Hibernate Envers (arquivo próprio), Flyway (arquivo próprio), PostgreSQL/H2 em profundidade, SQL/PL-SQL, MapStruct, Swagger, OAuth/Keycloak.

---

## 1. Material

### 1.1 Âncora do edital (ouro FCC)

O edital da arquitetura PDPJ-Br cita **persistência** com **duas camadas de versão**:

| Camada | O que aparece | Leitura de prova |
|--------|---------------|------------------|
| **Normativo / legado cobrado** | **JPA 2.0** · **Hibernate 5+** | Baseline “clássico” ainda referido nos normativos / stack histórica PDPJ |
| **Visão atualizada** | **noções** de **Jakarta Persistence 3.2** e **Hibernate ORM 7.x** | “respeitando versões dos normativos” — não apaga o legado; **complementa** |

Flash: a FCC **não** exige que você migre um monólito Hibernate 5 em 40 minutos. Exige que você **não confunda**:

1. **JPA / Jakarta Persistence** = **especificação** (API padrão de ORM).  
2. **Hibernate** = **implementação** (provider) dessa especificação (e APIs próprias).  
3. **JPA 2.0 + Hibernate 5+** = linha **legado/normativo**.  
4. **Jakarta Persistence 3.2 + Hibernate 7.x** = linha **moderna** (Java 17+, `jakarta.*`).

Frase de cartão:

> Normativo PDPJ ancora **JPA 2.0 / Hibernate 5+**; o edital ainda pede **noções** de **Jakarta Persistence 3.2 / Hibernate 7.x** — contraste de **namespace, baseline Java e API**, não “um substitui o outro na prova”.

### 1.2 O que é JPA / Jakarta Persistence (papel cobrável)

**Java Persistence API (JPA)** → depois da transferência Eclipse Foundation → **Jakarta Persistence**.

| Conceito | Papel |
|----------|--------|
| **Especificação** | Contratos: `EntityManager`, anotações (`@Entity`, `@Id`…), JPQL/Criteria, ciclo de vida |
| **Provider / implementação** | Hibernate, EclipseLink, etc. — quem de fato fala com o JDBC/BD |
| **Unidade de persistência** | Conjunto de entidades + config (`persistence.xml` / Spring Boot auto-config) |

Anti-confusão:

| Afirmação errada comum | Correção cobrável |
|------------------------|-------------------|
| “JPA é o Hibernate” | JPA/Jakarta Persistence = **spec**; Hibernate = **implementação** (muito usada) |
| “Jakarta Persistence apagou o JPA 2.0 do edital” | Edital cita **ambos** (legado + noções modernas) |
| “Hibernate 7 é só um rename do 5” | Mudança de **pacote**, **Java mínimo**, **API** (remove legado próprio) |

### 1.3 Tabela-mãe: JPA 2.0 / Hibernate 5+ × Jakarta Persistence 3.2 / Hibernate 7.x

| Aspecto | **JPA 2.0 + Hibernate 5+** (legado PDPJ) | **Jakarta Persistence 3.2 + Hibernate ORM 7.x** |
|---------|------------------------------------------|--------------------------------------------------|
| Namespace das anotações/API | Tipicamente **`javax.persistence.*`** (JPA clássico) | **`jakarta.persistence.*`** |
| Nome da spec | Java Persistence / JPA | **Jakarta Persistence** |
| Baseline Java (Hibernate) | Hibernate 5.x: Java **8** (série típica cobrada) | Hibernate **7.x**: Java **17+** |
| Papel do Hibernate | Implementa JPA 2.x + APIs **próprias** amplas (`Session#save`…) | Implementa **Jakarta Persistence 3.2**; **remove** APIs Hibernate que já têm equivalente JPA |
| Licença Hibernate (série 7) | Histórico LGPL (séries antigas) | **Apache License 2.0** a partir de 7.0 (fato de release; raro em prova, mas possível “atualidade”) |
| Coordenadas Maven (visão) | `org.hibernate:hibernate-core` + `javax.persistence` | `org.hibernate.orm:hibernate-core` + `jakarta.persistence` 3.2 |
| Foco cobrável | EntityManager, anotações, lazy/eager, persist/merge/remove, JPQL | Mesmo núcleo **+** `jakarta.*`, SchemaManager, Query/`getSingleResultOrNull`, alinhamento save→persist etc. |

Memorize só o contraste **útil em item**:

```
javax.persistence  +  Hibernate 5+   ≈  “legado normativo”
jakarta.persistence  +  Hibernate 7.x ≈  “linha moderna (noções)”
```

### 1.4 `javax` → `jakarta`: a pergunta mais barata da FCC

Linha do tempo mental (sem decorar datas):

1. **JPA** sob `javax.persistence`.  
2. Spec migra para Eclipse → **Jakarta Persistence 3.0** = sobretudo **rename** `javax` → `jakarta` (breaking de pacote).  
3. **3.1 / 3.2** = features novas (não só rename).  
4. Hibernate **6** já entra no mundo Jakarta; Hibernate **7** fecha em **Jakarta Persistence 3.2** + Java 17.

Leitura de prova:

- Trocar import `javax.persistence.Entity` por `jakarta.persistence.Entity` **não** é “outra ORM”.  
- É a **mesma família** de API com **namespace novo** (e, nas versões ≥3.1/3.2, recursos a mais).  
- Código Hibernate 5 com `javax.*` **não** compila “do nada” contra Jakarta Persistence 3.2 sem ajuste de dependências/imports.

### 1.5 Núcleo que **não muda** de ideia (ainda cai)

Independente da versão, a FCC continua cobrando o **modelo mental JPA**:

| Ideia | Frase cobrável |
|-------|----------------|
| `@Entity` | Classe mapeada a tabela (persistência ORM) |
| `@Id` / geração | Identidade; `GenerationType` (IDENTITY, SEQUENCE, TABLE, UUID — UUID mais “moderno”) |
| `EntityManager` | API padrão: `persist`, `merge`, `remove`, `find`, queries |
| `Session` (Hibernate) | API nativa Hibernate; em 7, **alinhar** ao JPA (`persist`/`merge`/`remove`/`find`/`getReference`) |
| Contexto de persistência | Entidades **managed** × **detached** × **transient** × **removed** |
| Lazy × Eager | Carregamento sob demanda × imediato (cuidado com N+1 / LazyInitializationException) |
| JPQL / Criteria | Consultas orientadas a entidades (não SQL “cru” obrigatório) |
| Transação | Persistência tipicamente sob transação (Spring `@Transactional` na prática) |

Ouro: **Jakarta Persistence 3.2 não inventa “outro paradigma”** — evolui a **mesma** família ORM.

### 1.6 O que a **Jakarta Persistence 3.2** acrescenta (noções cobráveis)

Não decore a especificação inteira. Foque no que vira **alternativa de item**:

| Novidade / reforço (3.2) | Leitura FCC |
|--------------------------|-------------|
| Pacote **`jakarta.persistence`** | Diferencia de JPA 2.0/`javax` |
| **`SchemaManager`** via `EntityManagerFactory#getSchemaManager()` | API **programática** de create/drop/validate/truncate do schema da PU |
| **`getSingleResultOrNull()`** | Evita exceção quando zero resultados (contraste com `getSingleResult`) |
| Operações JPQL/Criteria reforçadas | `union` / `intersect` / `except`, concat `\|\|`, funções `id`/`version`, null precedence em `ORDER BY` |
| Java **records** como **embeddables** | Record **não** vira `@Entity` “só porque é record”; embutível sim |
| Tipos `java.time` | `Instant`, `Year` etc. mais bem mapeados; versão pode usar `LocalDateTime`/`Instant` |
| `EntityManager` com opções | Overloads de `find`/`refresh`/`lock` com `FindOption`/`Timeout` etc. |
| `runInTransaction` / `callInTransaction` no EMF | Helpers de transação na fábrica |
| `PersistenceConfiguration` | Configuração programática da PU (além de só `persistence.xml`) |
| Entity Graph / Criteria | Generics e API de grafo mais “padrão JPA” (Hibernate deixa de ser a única casa de várias extensões) |

Flash anti-pegadinha:

- **`SchemaManager` (Jakarta 3.2)** ≠ **Flyway**. Flyway = **migração versionada** de scripts; SchemaManager = API do **provider** para ciclo de schema da unidade.  
- **`getSingleResultOrNull`** ≠ “sempre retorna lista”.

### 1.7 O que o **Hibernate ORM 7.x** muda em relação ao **Hibernate 5+** (visão de prova)

Tema geral da série 7: **adiar para o JPA** — remover APIs Hibernate que já têm equivalente padrão.

| Hibernate 5 (legado comum) | Hibernate 7 (substituto cobrável) |
|----------------------------|-----------------------------------|
| `session.save(obj)` | `session.persist(obj)` |
| `session.update(obj)` / `saveOrUpdate` | `session.merge(...)` (detached → managed) / `persist` se transient |
| `session.delete(obj)` | `session.remove(obj)` |
| `session.load(Class, id)` | `session.getReference(...)` (proxy/ref) |
| `session.get(Class, id)` (legado) | Preferir `find` (padrão JPA); `get` ainda pode existir, mas a direção é JPA |
| `createSQLQuery` / Criteria antigo (`org.hibernate.criterion`) | `createNativeQuery` / HQL/JPQL / Criteria JPA |
| Cascata `SAVE_UPDATE` / `DELETE` (anotações Hibernate) | `PERSIST` / `MERGE` / `REMOVE` |
| Java 8 + `javax.persistence` | Java **17+** + **Jakarta Persistence 3.2** |

Cartão:

> Hibernate 7 **não** “troca EntityManager por outra coisa”. Ele **empurra** o código para a **API JPA/Jakarta** e corta o dialeto proprietário antigo (`save`/`delete`…).

Requisitos de release (âncora factual, sem virar trivia):

- Java **17+**  
- Jakarta Persistence **3.2**  
- Substituição interna HCANN → **Hibernate Models** (baixo nível; raro cair — saiba que 7 **moderniza** o stack interno)

### 1.8 Como a FCC provavelmente **formula** o contraste

Padrões de enunciado esperáveis:

1. **Associação correta de versões**  
   - JPA 2.0 ↔ Hibernate 5+ (legado normativo)  
   - Jakarta Persistence 3.2 ↔ Hibernate 7.x (noções modernas)

2. **Namespace**  
   - `javax.persistence` × `jakarta.persistence`

3. **Papel**  
   - Spec × implementação (Hibernate)

4. **API removida / alinhada**  
   - `save`/`delete` → `persist`/`remove`

5. **O que NÃO confundir**  
   - Jakarta Persistence **≠** Flyway ≠ Envers ≠ MapStruct ≠ Swagger ≠ Eureka  
   - Hibernate 7 **≠** “só muda o número da versão sem breaking”

6. **Respeito ao normativo**  
   - Saber a linha **moderna** **não** autoriza dizer que o edital “abandonou” JPA 2.0/Hibernate 5+.

### 1.9 Relação com o restante da semana 2 (mapa rápido)

```
[Este arquivo 03]
  Jakarta 3.2 + Hibernate 7  ×  JPA 2.0 + Hibernate 5+
           │
           ├─► Envers ........ auditoria de entidades (outro arquivo)
           ├─► Flyway ........ migração versionada de schema (outro)
           ├─► PostgreSQL/H2 . bancos citados no edital
           └─► SQL ........... DML/DDL/DCL (camada abaixo do ORM)
```

Regra: se o item fala **versionamento de scripts SQL**, pense **Flyway**; se fala **histórico de alteração de entidade**, **Envers**; se fala **mapeamento objeto-relacional / EntityManager**, **JPA/Jakarta + Hibernate**.

### 1.10 Armadilhas (lista de erro)

1. Dizer que Jakarta Persistence **substituiu** Hibernate.  
2. Dizer que Hibernate 7 **ainda exige** `javax.persistence` como padrão.  
3. Afirmar que JPA 2.0 = Jakarta Persistence 3.2 “só o nome”. (Há rename **e** features; edital separa legado × noções.)  
4. Confundir `persist` (nova/transitória) com `merge` (reativa detached).  
5. Tratar `SchemaManager` como Flyway.  
6. Achar que record Java = entidade JPA automática.  
7. Inventar que Hibernate 7 roda em Java 8.  
8. Misturar com Eureka/Zuul/Swagger/MapStruct.  
9. Afirmar que “noções de 3.2/7.x” anulam a cobrança de JPA 2.0/Hibernate 5+.  
10. Confundir JPQL com SQL nativo obrigatório.

### 1.11 Mini-síntese (cartão mental)

> **JPA/Jakarta Persistence** = especificação ORM. **Hibernate** = implementação.  
> **Edital PDPJ:** ancora **JPA 2.0 + Hibernate 5+**; pede **noções** de **Jakarta Persistence 3.2 + Hibernate 7.x**.  
> Moderno = **`jakarta.*`**, Java **17+**, API alinhada (`persist`/`merge`/`remove`/`find`), extras (SchemaManager, `getSingleResultOrNull`…).  
> Legado = **`javax.*`**, Hibernate 5 com APIs próprias (`save`/`delete`…).  
> Não confundir com Flyway, Envers, MapStruct ou gateway/discovery.

---

## 2. Questões de treino (1–15)

**1.** No edital da arquitetura PDPJ-Br, a leitura mais correta sobre versões de persistência é:

A) Apenas Hibernate Envers 7, sem JPA  
B) Somente Jakarta Persistence 3.2, banindo qualquer menção a JPA 2.0  
C) JPA 2.0 e Hibernate 5+ como âncora normativa, com **noções** de Jakarta Persistence 3.2 e Hibernate ORM 7.x  
D) Apenas JDBC puro, sem ORM  
E) Apenas Flyway como única camada de persistência  

**2.** A diferença de **namespace** mais cobrável entre o JPA clássico (2.0) e a Jakarta Persistence moderna é:

A) `org.springframework` × `org.hibernate`  
B) `javax.persistence` × `jakarta.persistence`  
C) `java.sql` × `javax.sql` exclusivamente  
D) `com.sun` × `oracle.jdbc`  
E) `io.swagger` × `org.mapstruct`  

**3.** Assinale a associação **correta**:

A) JPA/Jakarta Persistence = implementação; Hibernate = especificação  
B) JPA/Jakarta Persistence = especificação; Hibernate = implementação (provider)  
C) Ambos são apenas bancos embutidos H2  
D) Hibernate = API Gateway; JPA = Eureka  
E) Jakarta Persistence = MapStruct  

**4.** Em relação ao Hibernate ORM **7.x**, é correto afirmar que, em linha geral de release:

A) Exige Java 8 e `javax.persistence` 2.0 como único contrato  
B) Alinha-se a **Jakarta Persistence 3.2** e baseline **Java 17+**  
C) É apenas um rename cosmético do Hibernate 3.0  
D) Substitui o PostgreSQL pelo H2 obrigatoriamente  
E) É o nome novo do Flyway  

**5.** O movimento típico de API no Hibernate 7 (“defer to JPA”) substitui `session.save(entity)` por:

A) `session.delete(entity)`  
B) `session.persist(entity)`  
C) `session.createSQLQuery(...)`  
D) `eureka.register(entity)`  
E) `flyway.migrate()`  

**6.** No Hibernate 7, o substituto cobrável de `session.delete(entity)` é:

A) `session.save(entity)`  
B) `session.update(entity)`  
C) `session.remove(entity)`  
D) `session.load(entity)`  
E) `entityManager.detach` exclusivamente como sinônimo  

**7.** Qual recurso é associado de forma típica à **Jakarta Persistence 3.2** (e não ao “núcleo mínimo” do JPA 2.0 clássico)?

A) Anotação `@Entity` em si  
B) Conceito de chave primária `@Id`  
C) API **`SchemaManager`** obtida do `EntityManagerFactory`  
D) Existência de `EntityManager`  
E) Ideia de JPQL  

**8.** Sobre Java **records** e Jakarta Persistence 3.2, a afirmação mais segura para prova é:

A) Todo record é automaticamente `@Entity` sem mapeamento  
B) Records podem ser usados como **embeddables**; não se conclui que todo record seja entidade  
C) Records proíbem qualquer uso com JPA  
D) Records substituem o Hibernate Envers  
E) Records são o dialeto SQL do PostgreSQL  

**9.** `getSingleResultOrNull()` (família Query na linha moderna) serve sobretudo para:

A) Registrar o serviço no Eureka  
B) Obter um resultado único **ou null** quando não há linha, sem o padrão estrito de exceção de `getSingleResult` em “zero hits”  
C) Gerar scripts Flyway automaticamente  
D) Documentar OpenAPI  
E) Fazer merge de DTO com MapStruct  

**10.** Qual contraste está **incorreto**?

A) Legado normativo: JPA 2.0 / Hibernate 5+  
B) Linha moderna (noções): Jakarta Persistence 3.2 / Hibernate 7.x  
C) Spec × provider: Jakarta Persistence × Hibernate  
D) `javax.persistence` é o pacote padrão da Jakarta Persistence 3.2  
E) Hibernate 7 empurra APIs proprietárias antigas para equivalentes JPA  

**11.** SchemaManager (Jakarta Persistence 3.2) **não** deve ser confundido com:

A) create/drop/validate/truncate programáticos do schema da PU  
B) Flyway (migração **versionada** por scripts)  
C) Um contrato acessível via `EntityManagerFactory`  
D) Recurso ligado à especificação moderna de persistência  
E) Ferramenta do ecossistema ORM/JPA  

**12.** No ciclo de vida JPA (válido no legado e no moderno), `merge` tipicamente:

A) É sinônimo estrito de `DELETE CASCADE` no Eureka  
B) Reassocia/copia estado de entidade **detached** para o contexto (retornando managed)  
C) Só existe no Hibernate 3 e foi banido na 5  
D) Substitui o API Gateway  
E) Gera OpenAPI a partir da entidade  

**13.** A frase mais alinhada ao edital é:

A) “Basta estudar Hibernate 7; JPA 2.0 foi retirado de qualquer menção”  
B) “Estude o legado JPA 2.0/Hibernate 5+ e tenha **noções** do contraste com Jakarta Persistence 3.2/Hibernate 7.x”  
C) “Persistência PDPJ = apenas RabbitMQ”  
D) “Jakarta Persistence 3.2 = Keycloak”  
E) “Hibernate 5+ proíbe EntityManager”  

**14.** Assinale a substituição **correta** na linha Hibernate 7:

A) `load` → manter só `createCriteria` clássico obrigatório  
B) `load` → `getReference` (mesma ideia de referência/proxy)  
C) `delete` → `save`  
D) `persist` → `delete`  
E) `find` → `eureka.getInstances`  

**15.** “Especificação ORM sob `jakarta.persistence`, com noções de SchemaManager e alinhamento a Hibernate 7.” Descreve melhor:

A) Zuul  
B) MapStruct  
C) Jakarta Persistence 3.2 (visão moderna do edital)  
D) Swagger UI  
E) Rancher  

---

### Gabarito comentado (treino 1–15)

1. **C** — Legado normativo + noções 3.2/7.x.  
2. **B** — `javax` × `jakarta`.  
3. **B** — Spec × implementação.  
4. **B** — JP 3.2 + Java 17+.  
5. **B** — `save` → `persist`.  
6. **C** — `delete` → `remove`.  
7. **C** — SchemaManager é marco 3.2.  
8. **B** — Records como embeddables; não “entity automática”.  
9. **B** — Único resultado ou null.  
10. **D** — 3.2 usa `jakarta.persistence`, não `javax`.  
11. **B** — Não é Flyway.  
12. **B** — Detached → managed via merge.  
13. **B** — Estratégia cobrável do edital.  
14. **B** — `load` → `getReference`.  
15. **C** — Definição do subtópico moderno.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 23)

O JPA é uma especificação da plataforma Java que define um padrão para mapeamento objeto-relacional (ORM) e gerenciamento de persistência de dados. No JPA, a forma padrão de realizar o mapeamento de objetos Java (POJOs) para tabelas de bancos de dados relacionais é por meio de

A) configurações XML como `persistence.xml`, `entity.xml` e `mapping.xml`.

B) anotações como `@Entity`, `@Id`, `@Table` e `@Column`.

C) interfaces como `EntityFactory`, `QueryBuilder` e `PersistenceUnit`.

D) classes como `HibernateSession`, `EntityManagerFactory` e `DataMapper`.

E) anotações como `@Database`, `@Primary`, `@Relation` e `@Field`.


**17.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 30)

Hibernate 5.x introduziu suporte nativo para as classes da API de Data e Hora do Java 8 (`java.time`), como

A) `Year`, `Month`, `Day` e `Instant`.

B) `LocalDate`, `LocalTime` e `LocalDateTime`.

C) `DateTime`, `TimeStamp` e `DateInterval`.

D) `DateTimeZone`, `DateTimeImmutable` e `DateTimeOffset`.

E) `NodaTime`, `ZonedDateTime` e `DurationTime`.


**18.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60)

Uma analista está desenvolvendo um sistema usando JPA e deseja configurar a utilização do framework Hibernate como provedor de persistência, por meio da instrução `<provider>org.hibernate.ejb.HibernatePersistence</provider>` que deve ser colocada no arquivo

A) `hibernate.xml`

B) `persistence-unit.xml`

C) `persistence.xml`

D) `orm-database-config.xml`

E) `server.xml`


**19.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60)

Um Analista de um Tribunal necessita habilitar o Hibernate Envers em uma entidade. Para isso, ele deve

A) utilizar o método `Session.saveOrUpdate()` para a entidade.

B) anotar a entidade com `@Audited`.

C) configurar `hibernate.envers.enabled` como `true` no arquivo de configuração.

D) estender a classe `AuditedEntity` fornecida pelo Hibernate Envers.

E) configurar o `audit-log` no arquivo `persistence.xml`.


**20.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Uma equipe de desenvolvimento está utilizando Spring Boot para criar uma API RESTful. Durante os testes, observaram que a aplicação demora a inicializar, principalmente devido à configuração do banco de dados. Para melhorar o tempo de inicialização, decidiram desabilitar a criação automática do esquema de banco de dados ao iniciar a aplicação. A configuração, no `application.properties`, que desabilita a criação automática do esquema de banco de dados pelo Spring Boot é a

A) `spring.jpa.hibernate.ddl-auto=none`

B) `spring.datasource.database-auto=false`

C) `spring.jpa.db-sql=disable`

D) `spring.jpa.properties.hibernate.db-auto=disable`

E) `spring.jpa.create-database-init=false`


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
| 16 | **B** | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 23 |
| 17 | **B** | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 30 |
| 18 | **C** | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60 |
| 19 | **B** | FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60 |
| 20 | **A** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60 |
| 21 | **D** | FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 34 de 60 |
| 22 | **C** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 37 de 60 |
| 23 | **D** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 33 de 60 |
| 24 | **A** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 51 de 60 |
| 25 | **E** \* | FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q23
- Q17: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q30
- Q18: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q41
- Q19: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q54
- Q20: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q38
- Q21: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q34
- Q22: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q37
- Q23: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q33
- Q24: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q51
- Q25: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22

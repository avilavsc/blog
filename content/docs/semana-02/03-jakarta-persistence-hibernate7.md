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

## 3. Questões estilo concurso FCC (16–25)

**16.** Relativamente à persistência referida no contexto PDPJ-Br, assinale a opção correta:

A) O edital menciona apenas NoSQL, excluindo JPA e Hibernate  
B) Constam JPA 2.0 e Hibernate 5+, bem como noções de Jakarta Persistence 3.2 e Hibernate ORM 7.x, observados os normativos  
C) Hibernate 7.x substitui, por si só, o Keycloak e o OAuth2  
D) Jakarta Persistence 3.2 é sinônimo estrito de API Gateway  
E) JPA 2.0 e Jakarta Persistence 3.2 são bancos de dados embutidos  

**17.** A migração conceitual de código Hibernate 5 que utiliza `session.save` / `session.delete` para a linha Hibernate 7, alinhada à especificação, aponta tipicamente para:

A) `persist` / `remove`  
B) `register` / `deregister` no Eureka  
C) `migrate` / `repair` no Flyway  
D) `map` / `unmap` no MapStruct  
E) `document` / `undeclare` no Swagger  

**18.** Acerca do pacote das anotações de mapeamento, é correto afirmar que:

A) Jakarta Persistence 3.2 padroniza o uso de `javax.persistence` como namespace exclusivo  
B) A linha moderna utiliza `jakarta.persistence`, em contraste com o `javax.persistence` do JPA clássico  
C) Ambos os pacotes foram abolidos em favor de `org.yaml`  
D) O pacote correto passou a ser apenas `io.kubernetes`  
E) Não há diferença cobrável de namespace entre JPA 2.0 e Jakarta Persistence 3.2  

**19.** Considere as afirmações:

I. Hibernate é, em regra, uma implementação (provider) da especificação JPA/Jakarta Persistence.  
II. Jakarta Persistence 3.2 e JPA 2.0 são citáveis no mesmo edital como legado normativo versus noções modernas.  
III. Hibernate ORM 7.x tem como baseline típico de release o Java 8 exclusivamente.

Assinale:

A) Apenas I  
B) Apenas II  
C) I e II  
D) II e III  
E) I, II e III  

**20.** O `SchemaManager` introduzido na linha Jakarta Persistence 3.2 destina-se, em essência, a:

A) versionar scripts de migração com checksums como o Flyway  
B) oferecer API programática para operações de schema (create/drop/validate/truncate) da unidade de persistência  
C) registrar heartbeats de microsserviços  
D) gerar UI OpenAPI  
E) mapear Entity↔DTO em tempo de compilação  

**21.** Assinale a alternativa que apresenta associação **incorreta**:

A) JPA 2.0 — legado referido nos normativos PDPJ  
B) Hibernate 5+ — implementação clássica citada com JPA 2.0  
C) Jakarta Persistence 3.2 — noções modernas no edital  
D) Hibernate ORM 7.x — alinhado a Jakarta Persistence 3.2  
E) Jakarta Persistence 3.2 — substituto direto e exclusivo do RabbitMQ  

**22.** Em enunciado que contraste ferramentas da stack de persistência, a FCC tipicamente considera **correto** que:

A) Flyway implementa EntityManager e substitui a especificação JPA  
B) Envers versiona scripts SQL no lugar do Flyway  
C) JPA/Jakarta Persistence define o contrato ORM; Hibernate o realiza; Flyway e Envers têm papéis distintos (migração × auditoria)  
D) Hibernate 7 elimina a necessidade de banco relacional  
E) `javax.persistence` e `jakarta.persistence` são jars do Eureka  

**23.** A exigência de Java 17+ no Hibernate ORM 7.x, contrastada com o Hibernate 5 típico de provas, indica principalmente:

A) Que a prova aboliu qualquer menção a JPA 2.0  
B) Um salto de **baseline de plataforma** na linha moderna, além da mudança para Jakarta Persistence 3.2  
C) Que apenas H2 é suportado  
D) Que MapStruct passou a ser obrigatório no EntityManager  
E) Que SQL foi proibido  

**24.** Quanto ao método `EntityManager#persist` (família JPA/Jakarta), a leitura cobrável mais segura é:

A) Sempre idêntico semanticamente a HTTP DELETE  
B) Torna persistente uma instância nova (transitória) no contexto de persistência, sob regras da especificação  
C) Serve apenas para documentar Swagger  
D) Substitui o Service Discovery  
E) É exclusivo do Hibernate 3 e inexistente no JPA 2.0  

**25.** A síntese mais alinhada ao edital e a este subtópico é:

A) Ignore JPA 2.0/Hibernate 5+; memorize só Rancher  
B) Domine o papel ORM (spec × Hibernate), o legado JPA 2.0/Hibernate 5+ (`javax`, APIs save/delete) e as **noções** Jakarta Persistence 3.2/Hibernate 7 (`jakarta`, Java 17+, persist/remove, SchemaManager etc.), sem confundir com Flyway/Envers  
C) Jakarta Persistence 3.2 = Zuul; Hibernate 7 = Eureka  
D) Toda prova FCC exige código completo de migração Hibernate 5→7  
E) `getSingleResultOrNull` é função do Keycloak  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Dupla camada do edital (legado + noções).  
17. **A** — `save`/`delete` → `persist`/`remove`.  
18. **B** — Namespace `jakarta` na linha moderna.  
19. **C** — I e II verdadeiras; III falsa (7.x = Java 17+).  
20. **B** — SchemaManager ≠ Flyway.  
21. **E** — Persistência ≠ mensageria RabbitMQ.  
22. **C** — Papéis ORM × migração × auditoria.  
23. **B** — Baseline Java + Jakarta 3.2.  
24. **B** — Persist de entidade transitória.  
25. **B** — Cartão mental completo do arquivo 03.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **spec ORM** ou **implementação**? → Jakarta/JPA × Hibernate.  
2. É **legado normativo** ou **noção moderna**? → 2.0/5+ × 3.2/7.x.  
3. É **`javax`** ou **`jakarta`**?  
4. É API antiga Hibernate (`save`/`delete`) ou JPA (`persist`/`remove`)?  
5. É **SchemaManager** ou **Flyway**? Entidade histórica → **Envers**.  
6. É MapStruct / Swagger / Eureka / Zuul? → **sair** deste arquivo.

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

## 3. Questões estilo concurso FCC (16–25)

**16.** Na arquitetura de persistência citada para a PDPJ-Br, JPA 2.0 e Hibernate 5+ relacionam-se de forma que:

A) a JPA constitui especificação de persistência e o Hibernate figura como implementação ORM compatível com essa API, sem que os termos sejam sinônimos absolutos  
B) Hibernate é a especificação oficial e a JPA é apenas um driver JDBC do PostgreSQL  
C) ambos se referem exclusivamente a ferramentas de autenticação SSO  
D) a menção a JPA no edital exclui qualquer uso de Hibernate  
E) Hibernate 5+ equivale, por definição, a um API Gateway  

**17.** Acerca do persistence context em JPA, é correto afirmar que:

A) corresponde ao conjunto de instâncias de entidade gerenciadas em determinado momento, funcionando como cache de primeiro nível da unidade de trabalho  
B) equivale exclusivamente ao cluster Kubernetes do tribunal  
C) substitui a necessidade de chave primária nas entidades  
D) é sinônimo rigoroso de Flyway  
E) existe apenas em bancos NoSQL e nunca em modelo relacional  

**18.** Uma entidade no estado **detached**:

A) permanece automaticamente sincronizada com o banco a cada alteração de campo, sem reassociação ao contexto  
B) já foi persistida em algum momento, porém não se encontra gerenciada pelo persistence context atual, de modo que alterações locais não se propagam automaticamente  
C) nunca pode ter possuído identificador  
D) é obrigatoriamente um DTO de MapStruct  
E) indica falha definitiva e irreversível do Hibernate Envers  

**19.** Considere as afirmativas abaixo.

I. `@Entity` marca classe cujo estado pode ser persistido segundo o mapeamento JPA.  
II. `persist` tipicamente torna transient em managed, agendando a inserção.  
III. Envers e Flyway desempenham exatamente a mesma função de migração de schema.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**20.** Em mapeamentos de associação bidirecional, a cláusula `mappedBy` indica, em regra:

A) o lado que não detém a responsabilidade de dono do relacionamento (lado inverso)  
B) o algoritmo de hash do token OAuth2  
C) a versão obrigatória do Rancher  
D) o modo EAGER exclusivo e irrenunciável de toda coleção  
E) a proibição de uso de `@Id`  

**21.** Assinale a opção que descreve corretamente o papel do Hibernate Envers no stack citado no edital:

A) prover auditoria e histórico de revisões de entidades mapeadas  
B) substituir o Service Discovery (Eureka)  
C) gerar documentação OpenAPI automaticamente a partir de controllers  
D) implementar o protocolo SMTP  
E) funcionar como único mecanismo permitido de balanceamento de carga  

**22.** A linguagem JPQL caracteriza-se por:

A) expressar consultas em termos do modelo de entidades/atributos da JPA, distinguindo-se do SQL nativo voltado a tabelas e colunas físicas  
B) ser um dialeto exclusivo de configuração do Keycloak  
C) eliminar a necessidade de EntityManager em qualquer consulta  
D) coincidir, termo a termo, com YAML de Compose  
E) auditar schemas no lugar do Envers  

**23.** Constitui confusão típica a ser evitada em prova:

A) tratar JPA como especificação e Hibernate como implementação  
B) afirmar que ORM mapeia objetos a relações  
C) equiparar Envers (auditoria de entidades) a Flyway (migração versionada de schema) como se fossem a mesma ferramenta com o mesmo propósito  
D) reconhecer estados new, managed, detached e removed  
E) distinguir persistence unit de persistence context  

**24.** No contexto de microsserviços alinhados à PDPJ, o uso de JPA/Hibernate em um serviço:

A) obriga todos os microsserviços do ecossistema a compartilharem o mesmo banco físico com escrita cruzada irrestrita  
B) situa-se, em regra, no interior do serviço dono do respectivo modelo de dados, preservando a ideia de autonomia de persistência entre serviços  
C) dispensa qualquer identificador de entidade  
D) substitui a necessidade de APIs REST entre serviços  
E) implica abandono obrigatório de bancos relacionais em favor exclusivo de planilhas  

**25.** Para fins de preparação em Analista Judiciário – Área de TI (código 14), a síntese mais adequada deste subtópico é:

A) JPA 2.0 como API/especificação; Hibernate 5+ como ORM; entidades e persistence context; ciclo de vida; relacionamentos/fetch; JPQL em visão; Envers para auditoria; Flyway apenas como vizinho de migração de schema  
B) apenas OAuth2 e Keycloak, sem qualquer menção a ORM  
C) apenas UML e BPMN  
D) apenas cabeamento estruturado e Wi-Fi  
E) apenas redação dissertativa sem conteúdo técnico  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **A** |
| 17 | **A** |
| 18 | **B** |
| 19 | **C** |
| 20 | **A** |
| 21 | **A** |
| 22 | **A** |
| 23 | **C** |
| 24 | **B** |
| 25 | **A** |

**Notas rápidas:** 16 A (especificação × implementação); 17 A (context = managed / 1º nível); 18 B (detached não sincroniza sozinho); 19 C (I e II; III confunde Envers/Flyway); 20 A (`mappedBy` = inverso); 21 A (auditoria de entidades); 22 A (JPQL orientada a entidades); 23 C (armadilha Envers≠Flyway); 24 B (JPA no serviço dono dos dados); 25 A (síntese do arquivo 01).

---

*Arquivo 01 · JPA 2.0 + Hibernate 5+ · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 Jakarta Persistence 3.2 + Hibernate ORM 7.x.*

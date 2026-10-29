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

## 3. Questões estilo concurso FCC (16–25)

**16.** Em aplicações Java alinhadas à stack referida no contexto PDPJ-Br, o Flyway caracteriza-se, em regra, por:

A) funcionar como catálogo exclusivo de registro e heartbeat de réplicas  
B) aplicar scripts versionados de migração para evoluir e rastrear o schema do banco de dados  
C) expor, por si só, a documentação interativa OpenAPI da API  
D) gerar, na compilação, mapeamentos tipados Entity ↔ DTO  
E) constituir o mecanismo SSO/OAuth2 da plataforma  

**17.** Relativamente à convivência entre Flyway e JPA/Hibernate, assinale a opção correta:

A) A existência de Flyway torna o uso de Entities conceitualmente impossível  
B) O Flyway governa a evolução do schema; o JPA/Hibernate mapeia e persiste objetos sobre esse schema  
C) Hibernate e Flyway são nomes comerciais do Eureka  
D) Flyway obriga banco único compartilhado entre todos os microsserviços  
E) JPA substitui integralmente qualquer necessidade de SQL de migration em todos os enunciados FCC  

**18.** Comparando a evolução de schema apenas via recursos do ORM com o uso de Flyway, a vantagem tipicamente cobrada das migrations explícitas é:

A) eliminar a necessidade de controllers REST  
B) tornar mudanças de schema versionadas, revisáveis e alinhadas ao processo de entrega entre ambientes  
C) substituir o Service Discovery  
D) documentar try-it automaticamente no lugar do Swagger  
E) registrar a aplicação no Zuul como filtro error  

**19.** Assinale a alternativa que apresenta associação **incorreta**:

A) Flyway — migração/versionamento de schema  
B) Hibernate — implementação ORM  
C) Hibernate Envers — auditoria de entidades  
D) Flyway — ponto único de entrada e roteamento de borda (papel de API Gateway)  
E) JPA — especificação/API de persistência  

**20.** Em enunciado que contraste ferramentas da stack de persistência, a FCC tipicamente considera **correto** que:

A) Envers aplica V1/V2 de DDL e Flyway audita revisões de Entity  
B) Flyway ocupa-se da evolução versionada do schema, ao passo que o Hibernate persiste entidades  
C) Flyway e Hibernate são intercambiáveis em qualquer item  
D) Flyway aplica rate limit na borda  
E) Hibernate realiza heartbeat das réplicas  

**21.** A expressão “ferramenta de database migration que mantém histórico das versões de schema aplicadas” descreve com mais precisão:

A) Eureka Server  
B) Zuul filter post  
C) Flyway  
D) MapStruct  
E) Swagger UI exclusivamente  

**22.** Em arquitetura de microsserviços com PostgreSQL por serviço, o uso de Flyway **não** implica, por si só:

A) possibilidade de versionar o DDL de cada serviço  
B) substituição do API Gateway e do Service Discovery  
C) aplicação ordenada de scripts ainda não executados  
D) rastreabilidade do que já rodou em cada ambiente  
E) alinhamento do schema esperado pelas Entities JPA  

**23.** Considere as afirmações:

I. Flyway documenta schemas OpenAPI no navegador (try-it).  
II. Flyway aplica migrations versionadas ao banco.  
III. Hibernate Envers não substitui o papel de migration de schema do Flyway.

Assinale:

A) Apenas I  
B) Apenas II  
C) II e III  
D) I e II  
E) I, II e III  

**24.** Diferenciar Flyway de Hibernate Envers é relevante em provas porque:

A) não há qualquer diferença conceitual cobrável  
B) o Flyway enfatiza versionamento de **schema**; o Envers, auditoria/versionamento de **entidades**  
C) Flyway só existe fora do ecossistema Java  
D) auditoria de Entity é exclusividade do Eureka  
E) Flyway é apenas o nome antigo do MapStruct  

**25.** A síntese mais alinhada ao edital, sobre Flyway, é:

A) Flyway = discovery; Hibernate = gateway; Envers = docs  
B) Flyway = migrations versionadas de schema; complementa JPA/Hibernate (ORM) e distingue-se do Envers (auditoria de Entity)  
C) Flyway obriga expor Entity no JSON e elimina DTO  
D) Flyway substitui Zuul, Eureka e OpenAPI  
E) Flyway é o banco H2 do Spring Boot  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Migrations versionadas de schema.  
17. **B** — Schema (Flyway) + ORM (JPA/Hibernate).  
18. **B** — Versionamento/revisão/ambientes.  
19. **D** — Gateway ≠ Flyway.  
20. **B** — Papéis complementares, sem inverter.  
21. **C** — Definição de database migration + histórico.  
22. **B** — Não substitui Gateway/Discovery.  
23. **C** — I falsa (Swagger); II e III verdadeiras.  
24. **B** — Schema × auditoria Envers.  
25. **B** — Cartão mental completo do arquivo 02.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **schema / DDL / migration / V1__sql / histórico de versão no BD**? → Flyway.  
2. É **Entity, Session, mapeamento objeto↔tabela, CRUD ORM**? → JPA/Hibernate.  
3. É **auditoria de Entity / revisões de dados de domínio**? → Envers.  
4. É **Entity↔DTO / compile-time**? → MapStruct.  
5. É **docs OpenAPI / try-it**? → Swagger.  
6. É **entrada/rota** ou **onde está a réplica**? → Gateway/Zuul ou Eureka.

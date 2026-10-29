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

## 3. Questões estilo concurso FCC (16–25)

**16.** No âmbito da arquitetura de desenvolvimento alinhada à PDPJ-Br, o PostgreSQL caracteriza-se, em regra, por:

A) constituir o sistema gerenciador de banco de dados relacional de referência para a persistência dos serviços em ambientes reais  
B) atuar exclusivamente como banco embutido em memória para testes unitários, sem papel operacional  
C) substituir integralmente o API Gateway e o *service discovery*  
D) equivaler, por definição, à ferramenta de mapeamento *compile-time* entre entidades e DTOs  
E) eliminar a necessidade de contratos HTTP/JSON entre microsserviços  

**17.** Acerca do H2 Database no contexto cobrado em concursos de TI do Judiciário (PDPJ / Spring), assinale a alternativa correta.

A) Destina-se, tipicamente, a cenários de teste e desenvolvimento, podendo operar de forma embutida ou em memória  
B) É o SGBD obrigatório e exclusivo de produção de todos os módulos nacionais da PDPJ  
C) Implementa, por si só, o registro de instâncias e *heartbeat* das réplicas  
D) Substitui o protocolo OAuth2 como mecanismo de autenticação  
E) Corresponde ao padrão OpenAPI de documentação interativa  

**18.** No contraste entre PostgreSQL e H2, é correto afirmar que:

A) ambos respondem ao mesmo papel operacional e são intercambiáveis em produção sem ressalvas  
B) o PostgreSQL ocupa o papel de SGBD operacional/de referência, ao passo que o H2 figura como alternativa leve para teste e desenvolvimento  
C) o H2 é a fachada pública HTTP e o PostgreSQL apenas cataloga IPs de microsserviços  
D) o PostgreSQL existe apenas como modo de emulação dentro do Eureka  
E) o H2 define as majors 15–18 do PostgreSQL  

**19.** Considere o uso de H2 com opção de compatibilidade voltada ao PostgreSQL. Assinale a alternativa correta.

A) Tal modo garante identidade completa de sintaxe e recursos com qualquer versão 15–18 em produção  
B) Trata-se de emulação parcial: testes podem passar no H2 e falhar no PostgreSQL diante de recursos específicos  
C) Torna desnecessária qualquer migração major entre versões do PostgreSQL  
D) Converte automaticamente clusters 14 em 18 via `pg_upgrade` embutido no H2  
E) Substitui o Flyway como ferramenta de versionamento de schema  

**20.** Considere as afirmações abaixo.

I. O edital de Analista de TI menciona PostgreSQL e H2 no eixo associado à PDPJ-Br.  
II. As majors 15 a 18 do PostgreSQL constam do conteúdo de banco de dados, com noções de migração/compatibilidade envolvendo a 14.  
III. O H2, no discurso típico de prova, não se confunde com o papel de banco de produção da plataforma.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**21.** Assinale a opção que melhor associa âncoras de *release* do PostgreSQL às majors indicadas.

A) 15 → somente remoção do SQL; 16 → fim da replicação; 17 → proibição de JSON; 18 → obrigatoriedade do H2  
B) 15 → `MERGE`; 16 → avanços em replicação lógica/SQL/JSON; 17 → `JSON_TABLE`/melhorias de manutenção; 18 → AIO/`uuidv7`/OAuth (visão)  
C) 15 a 18 são *minors* da série 14 e não exigem qualquer atenção  
D) Todas as majors listadas existem apenas no dialeto H2  
E) A 18 elimina a necessidade de `pg_upgrade` em qualquer cenário  

**22.** A passagem de um cluster PostgreSQL da major 14 para uma major posterior (15–18), em visão de concurso, relaciona-se a:

A) procedimento de *upgrade* de major — por exemplo `pg_upgrade`, dump/restore ou estratégias com replicação lógica — com verificação de incompatibilidades  
B) exclusividade do banco H2 em produção após a migração  
C) proibição de uso de SQL após a 14  
D) função exclusiva do Hibernate Envers como motor de upgrade do binário  
E) substituição do PostgreSQL pelo MapStruct  

**23.** Em enunciado que descreve componente responsável por “persistir dados relacionais do microsserviço em homologação/produção com SQL completo do dialeto Postgres”, o papel mais adequado é o de:

A) H2 Console como único SGBD nacional  
B) Swagger UI  
C) PostgreSQL  
D) Eureka Dashboard  
E) Zuul pré-filtro como sinônimo de banco embutido  

**24.** Para fins de preparação no eixo PostgreSQL + H2 do edital (PDPJ-Br / Analista TI), a síntese mais adequada é:

A) H2 e PostgreSQL são o mesmo artefato; majors 15–18 são irrelevantes; migração da 14 é feita pelo Zuul  
B) PostgreSQL = SGBD operacional (conhecer visão 15–18 e migração a partir da 14); H2 = teste/dev embutido; papéis distintos e complementares no stack  
C) Rate limiting de API Gateway só existe dentro do H2  
D) O cliente externo deve consultar o filesystem do banco sem JDBC  
E) `pg_upgrade` aplica-se apenas entre *minors* e nunca entre *majors*  

**25.** Assinale a afirmação **incompatível** com o papel típico do PostgreSQL e do H2 no conteúdo programático em tela.

A) PostgreSQL figura como banco relacional de referência da persistência PDPJ em ambientes reais  
B) H2 aparece tipicamente ligado a testes e desenvolvimento  
C) Majors 15–18 e noções de compatibilidade com a 14 são cobráveis no eixo de banco de dados  
D) H2, por ser embutido, substitui por definição o Service Discovery e o API Gateway em produção  
E) `pg_upgrade` e dump/restore são caminhos clássicos de migração major  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **A** |
| 17 | **A** |
| 18 | **B** |
| 19 | **B** |
| 20 | **E** |
| 21 | **B** |
| 22 | **A** |
| 23 | **C** |
| 24 | **B** |
| 25 | **D** |

**Notas rápidas:** 16 A (Postgres = SGBD referência/real); 17 A (H2 = teste/dev); 18 B (opera × leve); 19 B (emulação parcial); 20 E (I–III ok); 21 B (mapa 15–18); 22 A (major upgrade); 23 C (Postgres); 24 B (síntese); 25 D (H2 ≠ Discovery/Gateway).

---

*Arquivo 04 · PostgreSQL + H2 · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 05 SQL DML/DDL/DCL + noções PL/SQL.*

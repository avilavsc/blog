+++
title = '01 — Revisão ativa — PDPJ + SQL'
date = '2026-10-27'
draft = false
weight = 1
tags = ['semana-12', 'revisao']
categories = ['estudo']
+++
# Revisão ativa — PDPJ + SQL · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Semana 12 · revisão ativa (resumos + questões dos erros)  
**Escopo deste arquivo:** resumos **ultra-focados** PDPJ-Br/Spring stack + SQL/JPA/Flyway/Envers; checklist de erros típicos; treino 1–15; bloco REAL 16–25 (citados).  
**Fora de escopo aqui:** redes/segurança (arquivo 02); método do caderno de erros (arquivo 03); matéria nova.

**Priors:** `docs/semana-11/01`–`02`; high-yield `docs/semana-1/` e `docs/semana-2/`.  
**Modo:** ler o resumo → marcar o que ainda erra no checklist → fazer 1–15 sem olhar → retestar 16–25.

---

## 1. Material (revisão ativa)

### 1.1 PDPJ-Br em 8 linhas

| Certo | Errado (distrator) |
|-------|--------------------|
| Plataforma / ecossistema CNJ de integração e reuso | Monólito nacional único |
| Tribunais participam e operam | CNJ “dona do código” de cada TRT |
| Eureka/Zuul/Swagger/Flyway = **meios** da stack | PDPJ = sinônimo de Eureka |

### 1.2 Papéis que a FCC troca (ouro)

```
Cliente → Zuul/API Gateway (borda: “por onde ENTRA?”)
              ↓ consulta
         Eureka (registry: “ONDE está a réplica?”)
              ↓
         Microsserviço Boot (+ Cloud client)
              ↓
         Keycloak AS/IdP (“quem é / token?”)
```

| Peça | Frase cobrável | NÃO é |
|------|----------------|-------|
| **Zuul / Gateway** | Entrada, rotas, filtros pre/route/post/error · `@EnableZuulProxy` | Registry |
| **Eureka** | Catálogo + heartbeat · server **8761** em `application.properties` · `@EnableEurekaClient` | Edge / ponto de entrada |
| **Boot** | Como a app sobe (starters, jar, auto-config) · lazy: `spring.main.lazy-initialization=true` | Discovery |
| **Cloud** | Padrões distribuídos (Eureka, Zuul, Config…) | “outro nome do Boot” |
| **Keycloak** | Authorization Server / IdP · Bearer · refresh **sim** · Implicit **não** é o mais seguro | Gateway / Eureka |
| **Config** | Centralizar config; atualização dinâmica (ler enunciado: componente × função) | Actuator = Config |

Gateway **usa** Eureka; não o substitui. Frontend fala com a **borda estável** — não IP/porta de cada réplica.

### 1.3 SQL em 1 cartão (classificar = acerto)

| Família | Âncora |
|---------|--------|
| **DDL** | Estrutura: `CREATE` `ALTER` `DROP` **`TRUNCATE`** `RENAME` |
| **DML** | Dados: `SELECT` `INSERT` `UPDATE` `DELETE` **`MERGE`** |
| **DCL** | Privilégios: `GRANT` `REVOKE` |
| **TCL** | `COMMIT` `ROLLBACK` `SAVEPOINT` |

Sequência clássica: criar tabela (**DDL**) → permissão (**DCL**) → insert (**DML**) → delete linhas (**DML**) → revoke (**DCL**) → grant (**DCL**) = **DDL–DCL–DML–DML–DCL–DCL**.

`REVOKE ALL ON produtos FROM user001;` (não `GRANT REVOKE`, não `ALL PERMISSIONS`).  
Média com filtro: `SELECT AVG(col) FROM t WHERE col > 200;` — **não** `AVG(col > 200)`.

### 1.4 JPA × Hibernate × Envers × Flyway (anti-inversão)

| | Natureza |
|---|----------|
| **JPA** | Especificação ORM (`@Entity` `@Id` `@Table` `@Column`) |
| **Hibernate** | Implementação / provider em **`persistence.xml`** |
| **Envers** | Auditoria de **entidades** → **`@Audited`** (tabelas de revisão) |
| **Flyway** | Migration de **schema** · histórico `flyway_schema_history` |

> Flyway versiona schema · Hibernate persiste · Envers audita entities · Swagger documenta API (não versiona SQL).

Boot: `spring.jpa.hibernate.ddl-auto=none` desliga schema auto. H2 = teste; PG = prod típica.

### 1.5 Checklist de erros típicos (marque o que ainda erra)

**PDPJ / Spring**

- [ ] Eureka = gateway / ponto único de entrada  
- [ ] Zuul = discovery / “cliente do Eureka” como definição principal  
- [ ] Boot = Cloud (sinônimo)  
- [ ] Porta Eureka 8080 / arquivo `pom.xml` ou `eureka-server.xml`  
- [ ] `@ZuulServerApplication` / `@EnabledZuulEdgeServer`  
- [ ] Keycloak “não emite refresh” / Implicit Grant “sempre preferido”  
- [ ] Frontend configura IP/porta de cada microsserviço com Gateway+Discovery  
- [ ] Swagger versiona SQL; Flyway documenta REST; Eureka = borda  

**SQL / JPA**

- [ ] `TRUNCATE` = DML  
- [ ] `GRANT`/`REVOKE` = DML  
- [ ] `AVG(col > 200)` no lugar de `WHERE`  
- [ ] `REVOKE ALL PERMISSIONS` / `GRANT REVOKE`  
- [ ] JPA = implementação; Hibernate = só especificação  
- [ ] `@Audited` = Flyway / MapStruct / Eureka  
- [ ] Envers = backup diário / e-mail / migração de schema  
- [ ] Provider Hibernate fora de `persistence.xml`  

### 1.6 Ordem nesta sessão

1. Releia só as linhas do checklist marcadas.  
2. Treino 1–15 cronometrado (~25 min).  
3. Anote erros no caderno (arquivo 03).  
4. Bloco REAL 16–25 **sem olhar gabarito**.  
5. Só então abrir gabarito no final.

---

## 2. Questões de treino (1–15)

**1.** PDPJ-Br, na leitura cobrável, é principalmente:

A) Ecossistema/plataforma de integração e padronização sob diretrizes do CNJ  
B) Um monólito Java único obrigatório em todos os TRTs  
C) Sinônimo exclusivo de Netflix Eureka  
D) Banco físico único nacional  
E) Substituição do Keycloak pelo Zuul  

**2.** “Por onde a requisição externa ENTRA?” aponta para:

A) Eureka Server  
B) Flyway  
C) API Gateway / Zuul  
D) Envers  
E) `persistence.xml`  

**3.** “ONDE está a réplica do serviço X?” aponta para:

A) Zuul como definição principal  
B) Swagger UI  
C) Service Discovery / Eureka  
D) TRUNCATE  
E) DMZ  

**4.** Anotação clássica para a app Boot atuar como Zuul Proxy:

A) `@ZuulServerApplication`  
B) `@EnabledZuulEdgeServer`  
C) `@EnableEurekaClient`  
D) `@EnableZuulProxy`  
E) `@Audited`  

**5.** Porta padrão clássica do Eureka Server e arquivo típico:

A) 8080 e `pom.xml`  
B) 80 e `eureka-server.xml`  
C) 8761 e `application.properties`  
D) 8005 e `eureka-registry.xml`  
E) 443 e `keycloak.conf`  

**6.** Relação correta Boot × Cloud:

A) São sinônimos absolutos  
B) Cloud substitui Hibernate  
C) Boot é só o registry  
D) Cloud = DCL  
E) Boot sobe a app; Cloud traz padrões distribuídos (discovery, gateway, config…)  

**7.** No SSO/PDPJ, Keycloak atua tipicamente como:

A) Eureka Server  
B) Flyway  
C) Resource Server exclusivo sem emitir token  
D) `TRUNCATE`  
E) Authorization Server / IdP (emissão de tokens)  

**8.** Classificação correta: `MERGE`, `TRUNCATE`, `REVOKE`:

A) DDL, DML, DCL  
B) DCL, DDL, DML  
C) DML, DCL, DDL  
D) DML, DDL, DCL  
E) TCL, DML, DDL  

**9.** Criar tabela → conceder permissão → inserir → deletar linhas → revogar → conceder a outro. Sequência:

A) DML–DCL–DDL–DDL–DCL–DCL  
B) DCL–DDL–DML–DML–DCL–DCL  
C) DDL–DML–DCL–DML–DCL–DCL  
D) DDL–DCL–DDL–DML–DML–DCL  
E) DDL–DCL–DML–DML–DCL–DCL  

**10.** Revogar todos os direitos de `user001` em `produtos` (PostgreSQL, visão FCC):

A) `GRANT REVOKE ALL ON produtos FROM user001;`  
B) `REVOKE PERMISSIONS ON produtos FROM user001;`  
C) `REVOKE ALL ON produtos FROM user001;`  
D) `APPLY REVOKE ON produtos FROM user001;`  
E) `REVOKE ALL PERMISSIONS ON produtos FROM USER user001;`  

**11.** Média dos valores > 200 na coluna `Valor_Recebido`:

A) `SELECT AVG(Valor_Recebido > 200) FROM Cidadao;`  
B) `SELECT AVG(Valor_Recebido) > 200 FROM Cidadao;`  
C) `SELECT FROM Cidadao AVG(Valor_Recebido) > 200;`  
D) `SELECT AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`  
E) `SELECT Nome_Cidadao and AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`  

**12.** Habilitar Envers em uma entidade:

A) Anotar com `@Audited`  
B) Estender `AuditedEntity`  
C) Configurar `audit-log` no `server.xml`  
D) Anotar com `@EnableZuulProxy`  
E) Usar só Flyway  

**13.** Forma padrão de mapeamento JPA POJO → tabela:

A) Só `entity.xml` obrigatório  
B) `@Database`, `@Primary`, `@Field`  
C) Interfaces `EntityFactory` apenas  
D) Scripts Flyway no lugar das anotações  
E) Anotações `@Entity`, `@Id`, `@Table`, `@Column`  

**14.** Distrator clássico a **rejeitar**: Netflix Eureka tem como função principal

A) Registry / descoberta  
B) Heartbeat de instâncias  
C) Lookup por nome lógico  
D) Remoção de instâncias mortas  
E) Ser ponto único de entrada distribuindo requisições (papel de gateway)  

**15.** Síntese ativa correta:

A) Eureka = Zuul = Boot = Keycloak; TRUNCATE = DML; Envers = Flyway  
B) Swagger versiona SQL; Implicit Grant é o mais seguro  
C) Zuul entra; Eureka localiza; Keycloak autentica; DDL/DML/DCL classifica; Envers audita; Flyway versiona schema  
D) Frontend configura IP de cada réplica com Gateway+Discovery  
E) `GRANT` = DML e `@Audited` = MapStruct  

---

### Gabarito comentado (treino 1–15)

1. **A** — Plataforma/ecossistema.  
2. **C** — Borda/gateway.  
3. **C** — Discovery.  
4. **D** — `@EnableZuulProxy`.  
5. **C** — 8761 + `application.properties`.  
6. **E** — Papéis distintos e complementares.  
7. **E** — AS/IdP.  
8. **D** — MERGE=DML; TRUNCATE=DDL; REVOKE=DCL.  
9. **E** — DDL–DCL–DML–DML–DCL–DCL.  
10. **C** — `REVOKE ALL ON … FROM …`.  
11. **D** — Agregação + `WHERE`.  
12. **A** — `@Audited`.  
13. **E** — Anotações padrão.  
14. **E** — Inversão gateway×discovery (erro a evitar).  
15. **C** — Síntese correta.

Letras treino: A, C, C, D, C, E, E, D, E, C, D, A, E, E, C.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** FCC (preferência Analista TI / TRT-TRF). Sem invenção. Gabarito só no final. Use como **reteste** dos erros de semanas 1–2 e 11.

**16.** (FCC | TRT23 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Para fazer um aplicativo criado com Spring Boot, em condições ideais, funcionar como um servidor Zuul Proxy deve-se anotar a classe principal com

A) `@EnableZuulProxy`  
B) `@ZuulServerApplication`  
C) `@EnabledZuulEdgeServer`  
D) `@ZuulEdgeServerOn`  
E) `@ZuulProxyApplication`  

**17.** (FCC | TRT4 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 38 de 60)

Em um cenário de sistema de microsserviços um técnico deseja expor alguns dos microsserviços para acesso externo e ocultar outros. Como ele está usando o Spring Cloud, para proteger os microsserviços expostos contra solicitações de clientes mal-intencionados, ele pode usar o

A) Galaxy Eureka como edge server.  
B) Apache Toncat como service discovery.  
C) Apache Wildfly como security server.  
D) Netflix Zuul como edge server.  
E) Swagger como security server.  

**18.** (FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60)

O Spring Eureka é uma implementação do padrão de registro de serviços que permite que os microsserviços se registrem automaticamente e se descubram uns aos outros. A porta padrão em que o servidor Eureka irá executar na aplicação Spring Boot e o arquivo em que essa porta é configurada são, respectivamente,

A) `80` e `eureka-server.xml`  
B) `15000` e `application-server.yml`  
C) `8080` e `pom.xml`  
D) `8005` e `eureka-registry.xml`  
E) `8761` e `application.properties`  

**19.** (FCC | TRT-1 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 55 de 60)

Um Tribunal Regional do Trabalho deseja implementar um sistema de autenticação centralizado para seus diversos aplicativos internos e externos. Decidiu, para isso, utilizar o Keycloak como Identity Provider (IdP) e configurar a autenticação e autorização dos usuários por meio do OAuth2. Durante a implementação, a equipe de segurança precisa garantir que os fluxos de autenticação estejam corretamente configurados e que os aplicativos tenham acesso seguro aos recursos protegidos. Nesse cenário,

A) o Keycloak pode atuar como um Authorization Server dentro do fluxo OAuth2, permitindo a emissão de tokens de acesso e refresh tokens para aplicações cliente.  
B) o Keycloak apenas suporta OAuth2 para autenticação federada, sendo necessário utilizar um provedor externo como o Azure AD ou Google Identity para permitir login unificado.  
C) no OAuth2, ao utilizar Keycloak como IdP, o fluxo de Implicit Grant deve ser sempre a opção preferida para aplicações web modernas, pois é o mais seguro e recomendado atualmente.  
D) aplicações cliente que precisam acessar APIs protegidas devem utilizar o Authorization Code Lazy (ACL) combinado com Proof Authority for Code Exchange (PACE) para aumentar a segurança na troca de tokens.  
E) o protocolo OAuth2, implementado no Keycloak, permite a geração de Access Tokens e ID Tokens, mas não suporte Refresh Tokens para renovação de sessão.  

**20.** (FCC | TRF4 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Análise de Sistemas de Informação | Questão 22)

Os microsserviços da Plataforma Digital do Poder Judiciário (PDPJ) são desenvolvidos com tecnologias de código aberto, são implementados com o uso do Spring framework, e Java é a principal linguagem de programação. De acordo com a documentação da PDPJ, faz parte do ecossistema do Spring Framework o

A) Swagger, que organiza os scripts SQL que são executados no banco de dados, funcionando como um controle de versão.  
B) Flyway, serviço de documentação que descreve APIs RESTful expressas usando JSON.  
C) Zuul, que fornece os recursos de cliente para o serviço de descoberta Eureka.  
D) Netflix Eureka, que tem como principal função ser um ponto único de entrada, distribuindo as requisições entre as diversas instâncias de serviços.  
E) Data Envers, que é um módulo Hibernate que adiciona funcionalidades de auditoria às entities JPA.  

**21.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60)

Considere o seguinte caso: Utilizando comandos SQL, um analista criou uma tabela de cidadão dando permissão de acesso ao usuário Roberto. Posteriormente inseriu dados nessa tabela, mas logo em seguida teve que deletá-los, retirar a permissão de Roberto que estava saindo do tribunal e dar permissão a Carla. A sequência de comandos SQL utilizada no caso é, correta e respectivamente, categorizada como

A) DML - DCL - DDL - DDL - DCL - DCL.  
B) DML - DCL - DML - DDL - DML - DCL.  
C) DDL - DCL - DML - DML - DCL - DCL.  
D) DDL - DCL - DDL - DML - DML - DCL.  
E) DCL - DDL - DML - DML - DCL - DCL.  

**22.** (FCC | TRT-23 2022 | Técnico Judiciário, Área Apoio, Tecnologia da Informação)

Na linguagem SQL são respectivamente comandos DML, DDL e DCL:

A) `DROP`, `LOCK TABLE` e `GRANT`  
B) `UPDATE`, `CALL` e `ALTER`  
C) `CREATE`, `INSERT` e `GRANT`  
D) `MERGE`, `TRUNCATE` e `REVOKE`  
E) `RENAME`, `DELETE` e `DROP`  

**23.** (FCC | TRT4 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60)

No PostgreSQL, em condições ideais, para revogar todos os direitos do usuário user001 na tabela produtos, um Analista deve utilizar o comando

A) `GRANT REVOKE ALL ON produtos FROM user001;`  
B) `REVOKE PERMISSIONS ON produtos FROM user001;`  
C) `APPLY REVOKE ON produtos FROM user001;`  
D) `REVOKE ALL ON produtos FROM user001;`  
E) `REVOKE ALL PERMISSIONS ON produtos FROM USER user001;`  

**24.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 54 de 60)

Um Analista de um Tribunal necessita habilitar o Hibernate Envers em uma entidade. Para isso, ele deve

A) utilizar o método `Session.saveOrUpdate()` para a entidade.  
B) anotar a entidade com `@Audited`.  
C) configurar `hibernate.envers.enabled` como `true` no arquivo de configuração.  
D) estender a classe `AuditedEntity` fornecida pelo Hibernate Envers.  
E) configurar o `audit-log` no arquivo `persistence.xml`.  

**25.** (FCC | TRT19 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 40 de 60)

Flyway é uma biblioteca de migração de banco de dados que usa uma tabela para rastrear o estado do banco de dados, cujo nome padrão é

A) `flyway_ledger_history`  
B) `flyway_schema_history`  
C) `flw_history`  
D) `flyway_state_history`  
E) `flw_database_history`  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Citação |
|---|------|---------|
| 16 | **A** | FCC \| TRT23 2022 \| Analista TI \| Q38 |
| 17 | **D** | FCC \| TRT4 2022 \| Técnico TI \| Q38 |
| 18 | **E** | FCC \| TRT15 2023 \| Analista TI \| Q31 |
| 19 | **A** | FCC \| TRT-1 2025 \| Analista TI \| Q55 |
| 20 | **E** | FCC \| TRF4 2025 \| Analista Análise de Sistemas \| Q22 |
| 21 | **C** | FCC \| TRT-12 2023 \| Analista TI \| Q52 |
| 22 | **D** | FCC \| TRT-23 2022 \| Técnico TI \| MERGE/TRUNCATE/REVOKE |
| 23 | **D** | FCC \| TRT4 2022 \| Analista TI \| Q41 |
| 24 | **B** | FCC \| TRT20 2024 \| Analista TI \| Q54 |
| 25 | **B** | FCC \| TRT19 2022 \| Técnico TI \| Q40 |

Letras: A, D, E, A, E, C, D, D, B, B.

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt23-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q38
- Q17: https://www.quizinho.com.br/questao/fcc-trt4-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q38
- Q18: https://www.quizinho.com.br/questao/fcc-trt15-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q31
- Q19: https://www.quizinho.com.br/questao/fcc-trt-1-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q55
- Q20: https://www.quizinho.com.br/questao/fcc-trf4-2025-analista-judiciario-area-apoio-especializado-especialidade-analise-de-sistemas-de-informacao-2025-q22
- Q21: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q52
- Q22: https://www.qconcursos.com/questoes-de-concursos/questoes/9e72cb79-46
- Q23: https://www.quizinho.com.br/questao/fcc-trt4-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q41
- Q24: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q54
- Q25: https://www.quizinho.com.br/questao/fcc-trt19-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q40

---

*Arquivo 01 · Revisão ativa PDPJ+SQL · Semana 12 · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: `02-revisao-ativa-redes-seguranca.md`.*

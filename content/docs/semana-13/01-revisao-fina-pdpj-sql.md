+++
title = '01 — Revisão fina — PDPJ + SQL'
date = '2026-10-28'
draft = false
weight = 1
tags = ['semana-13', 'revisao']
categories = ['estudo']
+++
# Revisão fina — PDPJ + SQL · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Semana 13 = pré-prova (18/01–24/01)  
**Escopo deste arquivo:** flash **fino** — só pegadinhas/contrastes PDPJ-Br/Spring stack + SQL/JPA/Flyway/Envers. Sem matéria nova.  
**Fora de escopo aqui:** redes; segurança/ágeis (`02`); ITIL/governança (`03`); tutorial longo.

**Priors:** `docs/semana-11/01`–`02` · `docs/semana-12/01` · high-yield `docs/semana-1/` e `docs/semana-2/`.  
**Modo:** cartão anti-troca → treino 1–15 → REAL 16–25 → gabarito só no final.

---

## 1. Material (revisão fina — pegadinhas)

### 1.1 Uma frase cada (PDPJ / stack)

| Peça | FCC cobra | Distrator clássico |
|------|-----------|-------------------|
| **PDPJ-Br** | Plataforma/ecossistema CNJ (integração/reuso) | Monólito nacional único |
| **Zuul / Gateway** | “Por onde **ENTRA**?” · `@EnableZuulProxy` | Registry / Eureka |
| **Eureka** | “**ONDE** está a réplica?” · **8761** · `@EnableEurekaClient` | Edge / ponto de entrada |
| **Boot** | Como a app **sobe** · lazy: `spring.main.lazy-initialization=true` | Discovery |
| **Cloud** | Padrões distribuídos (Eureka, Zuul, Config…) | “outro nome do Boot” |
| **Keycloak** | AS/IdP · Bearer · **refresh sim** · Implicit **não** é o mais seguro | Gateway |
| **Config** | Centralizar config / atualização dinâmica | Actuator = Config |
| **REST** | `@RestController` | `@RestfulController` etc. |

```
Cliente → Zuul (borda) → consulta Eureka → Boot (+Cloud) → Keycloak (token)
Gateway USA Eureka; NÃO o substitui.
Frontend fala com a BORDA estável — não IP/porta de cada réplica.
```

### 1.2 Trocas que matam (PDPJ)

- Eureka ≠ gateway ≠ Keycloak ≠ Swagger ≠ Flyway.  
- Porta Eureka **não** é 8080; arquivo **não** é `pom.xml` / `eureka-server.xml`.  
- `@ZuulServerApplication` / `@EnabledZuulEdgeServer` / `@EurekaClient` (sem Enable) = isca.  
- Swagger **documenta** API; Flyway **versiona schema**; Envers **audita** entities (`@Audited`).  
- Na PDPJ (TRF4/2025): distratores trocam Swagger↔Flyway↔Zuul↔Eureka; gabarito costuma ser o papel **certo** (ex.: Envers = auditoria JPA).

### 1.3 SQL — classificar = acertar

| Família | Âncora |
|---------|--------|
| **DDL** | `CREATE` `ALTER` `DROP` **`TRUNCATE`** `RENAME` |
| **DML** | `SELECT` `INSERT` `UPDATE` `DELETE` **`MERGE`** |
| **DCL** | `GRANT` `REVOKE` |
| **TCL** | `COMMIT` `ROLLBACK` `SAVEPOINT` |

Sequência clássica: criar tabela → permissão → insert → delete dados → revoke → grant = **DDL–DCL–DML–DML–DCL–DCL**.  
`insert, create, delete, drop` = **DML–DDL–DML–DDL**.  
DML/DDL/DCL tipicamente: **MERGE · TRUNCATE · REVOKE**.

`REVOKE ALL ON produtos FROM user001;` — nunca `GRANT REVOKE` / `ALL PERMISSIONS`.  
Média filtrada: `SELECT AVG(col) FROM t WHERE col > 200;` — **nunca** `AVG(col > 200)`.

### 1.4 JPA × Hibernate × Envers × Flyway

| | Natureza |
|---|----------|
| **JPA** | Spec (`@Entity` `@Id`…) |
| **Hibernate** | Provider → tag em **`persistence.xml`** |
| **Envers** | `@Audited` (auditoria de entidades) |
| **Flyway** | Migration · `flyway_schema_history` |

> Flyway versiona schema · Hibernate persiste · Envers audita · Swagger documenta REST.

### 1.5 Cartão único (se só restar 2 min)

```
Zuul ENTRA · Eureka LOCALIZA · Boot SOBE · Keycloak TOKEN · Config CENTRALIZA
DDL estrutura · DML dados · DCL privilégio · TRUNCATE=DDL · MERGE=DML
AVG + WHERE · REVOKE ALL ON … FROM …
@Audited=Envers · provider=persistence.xml · flyway_schema_history
```

---

## 2. Questões de treino (1–15)

**1.** “Por onde a requisição externa ENTRA?” aponta para:

A) Eureka Server  
B) Flyway  
C) API Gateway / Zuul  
D) Envers  
E) `@Audited`  

**2.** Porta padrão clássica do Eureka Server e arquivo típico:

A) 8080 e `pom.xml`  
B) 80 e `eureka-server.xml`  
C) 8761 e `application.properties`  
D) 443 e `zuul.yml`  
E) 8005 e `eureka-registry.xml`  

**3.** Anotação para app Boot atuar como cliente Eureka:

A) `@EurekaClient`  
B) `@EnableEurekaClient`  
C) `@IsEurekaClient`  
D) `@EnableZuulProxy`  
E) `@RestfulController`  

**4.** Anotação para Boot funcionar como Zuul Proxy:

A) `@ZuulProxyApplication`  
B) `@EnableZuulProxy`  
C) `@ZuulServerApplication`  
D) `@EnabledZuulEdgeServer`  
E) `@EnableEurekaClient`  

**5.** Lazy initialization global no Boot:

A) `@Lazy(true)` em `application.properties`  
B) `spring.main.lazy-initialization=true` em `application.properties`  
C) `set-lazy-initialization=true` em `spring-boot.xml`  
D) `@SpringMainLazyInitialization`  
E) Porta 8761  

**6.** Keycloak no fluxo OAuth2 — leitura cobrável:

A) Não emite refresh token  
B) Implicit Grant é sempre o mais seguro  
C) Pode atuar como Authorization Server e emitir access + refresh  
D) Substitui Eureka  
E) É o mesmo que Zuul  

**7.** Com Gateway + Discovery, consequência típica para o frontend:

A) Configurar IP/porta de cada microsserviço  
B) Frontend gerencia heartbeat do Eureka  
C) Borda reduz/abstrai chamadas externas (não “caçar” réplicas)  
D) Swagger vira registry  
E) Flyway autentica  

**8.** `TRUNCATE` é tipicamente:

A) DML  
B) DCL  
C) DDL  
D) TCL exclusivo  
E) Envers  

**9.** `MERGE`, `TRUNCATE` e `REVOKE` são, respectivamente:

A) DDL, DML, DCL  
B) DML, DDL, DCL  
C) DCL, DDL, DML  
D) DML, DCL, DDL  
E) TCL, DML, DDL  

**10.** Média dos valores > 200:

A) `SELECT AVG(Valor_Recebido > 200) FROM Cidadao;`  
B) `SELECT AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`  
C) `SELECT AVG(Valor_Recebido) > 200 FROM Cidadao;`  
D) `SELECT FROM Cidadao AVG(Valor_Recebido) > 200;`  
E) `SELECT Nome_Cidadao and AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`  

**11.** Revogar todos os direitos de `user001` em `produtos` (PG):

A) `GRANT REVOKE ALL ON produtos FROM user001;`  
B) `REVOKE ALL PERMISSIONS ON produtos FROM USER user001;`  
C) `REVOKE ALL ON produtos FROM user001;`  
D) `APPLY REVOKE ON produtos FROM user001;`  
E) `REVOKE PERMISSIONS ON produtos FROM user001;`  

**12.** Habilitar Hibernate Envers em entidade:

A) `Session.saveOrUpdate()`  
B) Anotar com `@Audited`  
C) `hibernate.envers.enabled=true` como única via  
D) Estender `AuditedEntity`  
E) `audit-log` em `persistence.xml`  

**13.** Provider Hibernate no JPA (`org.hibernate.ejb.HibernatePersistence`) vai em:

A) `hibernate.xml`  
B) `persistence-unit.xml`  
C) `persistence.xml`  
D) `orm-database-config.xml`  
E) `server.xml`  

**14.** Tabela padrão de histórico do Flyway:

A) `flyway_ledger_history`  
B) `flyway_schema_history`  
C) `flw_history`  
D) `flyway_state_history`  
E) `flw_database_history`  

**15.** Contraste correto:

A) Eureka = borda; Zuul = registry  
B) Flyway documenta REST; Swagger versiona SQL  
C) JPA = spec; Hibernate = provider; Envers = `@Audited`; Flyway = schema  
D) Boot = Cloud (sinônimo)  
E) Keycloak = Service Discovery  

---

### Gabarito comentado (treino 1–15)

1. **C** — entrada = Gateway/Zuul.  
2. **C** — 8761 + `application.properties`.  
3. **B** — `@EnableEurekaClient`.  
4. **B** — `@EnableZuulProxy`.  
5. **B** — propriedade global de lazy.  
6. **C** — AS + access/refresh.  
7. **C** — abstração pela borda.  
8. **C** — TRUNCATE = DDL.  
9. **B** — MERGE/TRUNCATE/REVOKE.  
10. **B** — AVG + WHERE.  
11. **C** — `REVOKE ALL ON … FROM`.  
12. **B** — `@Audited`.  
13. **C** — `persistence.xml`.  
14. **B** — `flyway_schema_history`.  
15. **C** — síntese anti-inversão.

Letras treino: C, C, B, B, B, C, C, C, B, B, C, B, C, B, C.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** FCC (preferência Analista/Técnico TI · TRT/TRF). **Sem invenção.** Gabarito só no final.

**16.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 32 de 60)

Na classe principal de uma aplicação criada com Spring Boot, para fazer com que a aplicação atue como um cliente Eureka utiliza-se a anotação

A) `@EurekaClient`  
B) `@EnableEurekaClient`  
C) `@EnableDiscoveryEurekaClient`  
D) `@EurekaDiscoveryClient`  
E) `@IsEurekaClient`  

**17.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 43 de 60)

As versões mais recentes do Spring Boot permitem a configuração de inicialização lenta (lazy initialization), que faz com que os beans sejam criados à medida que são necessários e não durante a inicialização do aplicativo. Uma das maneiras de habilitar a inicialização lenta é colocar

A) `@Lazy(true)` no arquivo `application.properties`  
B) `spring.main.lazy-initialization=true` no arquivo `application.properties`  
C) `set-lazy-initialization=true` no arquivo `spring-boot.xml`  
D) `lazy-initialization=true` no arquivo `spring-application-builder.xml`  
E) `@SpringMainLazyInitialization` na classe `SpringApplicationBuilder`  

**18.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60)

Ao usar o Gateway API com o Service Discovery no contexto da PDPJ-Br, a principal consequência para o frontend em uma aplicação baseada em microsserviços é que o

A) Gateway API diminui a necessidade de autenticação, uma vez que o Service Discovery é integrado ao SSO.  
B) frontend deve configurar manualmente os IPs e portas dos serviços disponíveis na plataforma.  
C) frontend se torna o responsável pela chamada direta de cada microsserviço individualmente.  
D) Gateway API reduz o número de chamadas externas que o frontend precisa fazer, tornando a comunicação mais eficiente.  
E) frontend deve gerenciar manualmente as atualizações de endpoint de cada serviço individual registrado.  

**19.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31 de 60)

Na abordagem do Spring para construir serviços web RESTful, as solicitações HTTP são tratadas por um controlador, que é uma classe identificada com a anotação

A) `@RestfulController`  
B) `@RestfulMapping`  
C) `@RestApplicationController`  
D) `@RestController`  
E) `@RequestController`  

**20.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 53 de 60)

Considere uma tabela denominada `Cidadao` e as colunas `Nome_Cidadao` e `Valor_Recebido`. Para obter a média dos valores maiores que 200 recebidos pelos cidadãos, um Analista deve utilizar o comando

A) `SELECT AVG(Valor_Recebido > 200) FROM Cidadao;`  
B) `SELECT AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`  
C) `SELECT AVG(Valor_Recebido) > 200 FROM Cidadao;`  
D) `SELECT FROM Cidadao AVG(Valor_Recebido) > 200;`  
E) `SELECT Nome_Cidadao and AVG(Valor_Recebido) FROM Cidadao WHERE Valor_Recebido > 200;`  

**21.** (FCC | TRT-12 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 52 de 60)

Considere o seguinte caso: Utilizando comandos SQL, um analista criou uma tabela de cidadão dando permissão de acesso ao usuário Roberto. Posteriormente inseriu dados nessa tabela, mas logo em seguida teve que deletá-los, retirar a permissão de Roberto que estava saindo do tribunal e dar permissão a Carla. A sequência de comandos SQL utilizada no caso é, correta e respectivamente, categorizada como

A) DML - DCL - DDL - DDL - DCL - DCL.  
B) DML - DCL - DML - DDL - DML - DCL.  
C) DDL - DCL - DML - DML - DCL - DCL.  
D) DDL - DCL - DDL - DML - DML - DCL.  
E) DCL - DDL - DML - DML - DCL - DCL.  

**22.** (FCC | TRT5 2022 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 49 de 60)

Um técnico utilizou os comandos SQL insert, create, delete e drop em um programa que são, correta e respectivamente, comandos

A) DDL − DDL − DCL − DDL.  
B) DML − DCL − DCL − DML.  
C) DCL − DML − DCL − DCL.  
D) DDL − DML − DCL − DML.  
E) DML − DDL − DML − DDL.  

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

**25.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 41 de 60)

Uma analista está desenvolvendo um sistema usando JPA e deseja configurar a utilização do framework Hibernate como provedor de persistência, por meio da instrução `<provider>org.hibernate.ejb.HibernatePersistence</provider>` que deve ser colocada no arquivo

A) `hibernate.xml`  
B) `persistence-unit.xml`  
C) `persistence.xml`  
D) `orm-database-config.xml`  
E) `server.xml`  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Citação |
|---|------|---------|
| 16 | **B** | FCC \| TRT19 2022 \| Analista TI \| Q32 |
| 17 | **B** | FCC \| TRT22 2022 \| Analista TI \| Q43 |
| 18 | **D** | FCC \| TRT6 2025 \| Analista TI \| Q39 |
| 19 | **D** | FCC \| TRT19 2022 \| Analista TI \| Q31 |
| 20 | **B** | FCC \| TRT-12 2023 \| Analista TI \| Q53 |
| 21 | **C** | FCC \| TRT-12 2023 \| Analista TI \| Q52 |
| 22 | **E** | FCC \| TRT5 2022 \| Técnico TI \| Q49 |
| 23 | **D** | FCC \| TRT4 2022 \| Analista TI \| Q41 |
| 24 | **B** | FCC \| TRT20 2024 \| Analista TI \| Q54 |
| 25 | **C** | FCC \| TRT22 2022 \| Analista TI \| Q41 |

Letras: B, B, D, D, B, C, E, D, B, C.

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q32
- Q17: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q43
- Q18: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q39
- Q19: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q31
- Q20: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q53
- Q21: https://www.quizinho.com.br/questao/fcc-trt-12-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q52
- Q22: https://www.quizinho.com.br/questao/fcc-trt5-2022-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q49
- Q23: https://www.quizinho.com.br/questao/fcc-trt4-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q41
- Q24: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q54
- Q25: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q41

---

*Arquivo 01 · Revisão fina PDPJ+SQL · Semana 13 · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: `02-revisao-fina-seguranca-ageis.md`.*

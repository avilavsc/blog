+++
title = '06 — Selenium + Maven · Revisão FCC'
date = '2026-10-31'
draft = false
weight = 6
tags = ['semana-10', 'selenium']
categories = ['estudo']
+++
# Selenium + Maven · Revisão FCC · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — Selenium e Maven (relembrar DevOps/testes; nível revisão)  
**Escopo deste arquivo:** Recapitulação cobrável: papéis no pipeline; ciclo Maven/`pom.xml`/coordenadas/`scope`; Selenium WebDriver, esperas (implicit/explicit), Grid; anti-confusão com Jenkins/Git/CI. Pressupõe semana 3 (DevSecOps/CI/CD).  
**Fora de escopo aqui:** YAML profundo GitLab/CircleCI; Docker/K8s; inglês técnico (04); legado VB/JSF (05); teoria ágil completa.

**Grafia do edital:** *Sellenium* (erro). Em prova/mercado: **Selenium**.

---

## 1. Material

### 1.1 Por que revisar agora

Cronograma semana 10: **cauda longa + relembrar** Selenium/Maven já vistos na semana 3. Objetivo: reacender o mapa de **papéis** e as 2–3 pegadinhas que a FCC repete (pom.java, Selenium=CI, Implicit×Explicit, Grid).

### 1.2 Mapa de papéis (não misturar)

| Ferramenta | Papel | Arquivo / marca |
|------------|-------|-----------------|
| **Maven** | Build + deps Java | `pom.xml` |
| **Selenium** | Teste UI/E2E no browser | WebDriver / Grid |
| **Jenkins / GitLab CI / CircleCI / Actions** | Orquestra pipeline CI/CD | Jenkinsfile / `.yml` |
| **Git** | Versionamento | commits/branches |
| **JUnit** | Testes de unidade Java (visão) | `@Test`, scope `test` |

Flash anti-distrator (TRT-20/2024):

- Selenium **não** automatiza builds/pipelines nem “gestão de repositórios Git”.  
- Maven **não** é VCS nem CI completo; arquivo **não** é `pom.java`.  
- Jenkins **não** é “escrito em PHP exclusivo para testes”.

### 1.3 Maven — cartão de revisão

**POM** = *Project Object Model* → arquivo **`pom.xml`**.

Coordenadas mínimas de dependência:

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
  <scope>test</scope>
</dependency>
```

| Elemento | Função |
|----------|--------|
| `groupId` | organização/grupo |
| `artifactId` | nome do artefato |
| `version` | versão |
| `scope` | quando entra no classpath |

**`scope` test:** disponível na compilação/execução de **testes**; **não** vai para o artefato final de runtime típico.

Ciclo de vida mental (visão): `validate` → `compile` → `test` → `package` → `verify` → `install` → `deploy` (Maven deploy ≠ “CD cloud” sozinho).

Onde se declara starter JPA/Postgres etc. em Spring Boot + Maven? No **`pom.xml`**, não em `web.xml` / `server.xml` / `manifest.xml` / `data.xml`.

### 1.4 Selenium — cartão de revisão

| Peça | Ideia |
|------|-------|
| **WebDriver** | API que controla o browser (Chrome, Firefox…) |
| **IDE** | gravação/auxiliar (não é o Grid) |
| **RC** | legado (*Remote Control*) |
| **Grid** | distribuir testes em **múltiplas máquinas**/ambientes; escalar suítes |

Selenium = automação de **interface** / testes funcionais E2E no navegador. Encaixa no pipeline como **job de teste**, não como orquestrador CI.

### 1.5 Esperas ( Implicit × Explicit × sleep )

| Tipo | Ideia FCC | Pegadinha |
|------|-----------|-----------|
| **Implicit wait** | Timeout **global** para achar elementos; padrão **0** (falha rápido se não achar) | Não é `Thread.sleep` |
| **Explicit wait** | Espera **condição** específica (`WebDriverWait` + presença/clicável) | Melhor p/ elemento dinâmico pós-animação |
| **`Thread.sleep`** | Pausa fixa cega | Nem implicit nem explicit “de verdade” |

Cenário TRT-1/2025: botão aparece após animação → `NoSuchElementException` → preferir **Explicit Wait** (não ignorar exceção; não “só atualizar IDE”; Implicit ajuda menos que wait condicional).

Cenário TRT-14/2016: controle de espera — alternativa correta descreve **implícito**, valor padrão **0**, tempo baixo para não ficar esperando demais antes do erro.

### 1.6 Selenium Grid — uma frase

> **Grid** = distribuir/paralelizar suítes em várias máquinas/browsers/ambientes → reduz tempo e escala testes.

Não confundir com SELECT/TYPE (comandos legados) nem achar que IDE = Grid.

### 1.7 Encaixe no pipeline (revisão DevOps)

```
commit → CI clona (Git)
      → mvn test / package (Maven + JUnit)
      → (opcional) Selenium UI/E2E
      → artefato / deploy
```

DevSecOps: scans cedo (*shift left*); Selenium continua na fatia **teste**, não substitui SAST.

### 1.8 Checklist rápido pré-questão

1. A alternativa troca **papel** (Selenium↔Maven↔Jenkins↔Git)?  
2. Fala `pom.java` ou Maven “proprietário com CI nativo tipo GitLab”?  
3. Espera: sleep rotulado como implicit/explicit?  
4. Elemento dinâmico → Explicit?  
5. Distribuir em várias máquinas → Grid?

---

## 2. Questões de treino (1–15)

**1.** O arquivo central de um projeto Maven é:

A) `pom.java`  
B) `pom.xml`  
C) `selenium.yml` exclusivo  
D) `faces-config.xml`  
E) `wsus.conf`  

**2.** Coordenadas mínimas de uma dependência Maven:

A) Só `scope`  
B) `groupId`, `artifactId`, `version`  
C) Só URL do GitHub  
D) Apenas nome do browser  
E) Só `Thread.sleep`  

**3.** `scope` **test** no Maven indica que a dependência:

A) Vai obrigatoriamente no JAR final de produção  
B) Fica disponível para testes e tipicamente fora do artefato de runtime  
C) Substitui o Git  
D) Compila Delphi  
E) É sinônimo de SaaS  

**4.** Selenium, no mapa do edital, serve principalmente para:

A) Orquestrar pipelines CI/CD e hospedar Git  
B) Automatizar interações/testes de UI no browser  
C) Filtrar pacotes L3  
D) Gerir Active Directory  
E) Substituir o `pom.xml`  

**5.** Maven × Selenium — contraste correto:

A) Ambos são VCS  
B) Maven builda/gerencia deps Java; Selenium testa UI  
C) Selenium gera `pom.xml` automaticamente como único fim  
D) Maven controla ChromeDriver nativamente como browser engine  
E) São protocolos DiffServ  

**6.** Selenium **Grid** é a peça que:

A) Grava macros apenas no IDE  
B) Distribui testes em múltiplas máquinas/ambientes  
C) Substitui o handshake TLS  
D) É o nome do `scope` Maven  
E) Compila JSP  

**7.** Em elemento carregado dinamicamente após animação, a abordagem mais adequada costuma ser:

A) Ignorar todas as exceções do WebDriver  
B) Explicit Wait pela presença/clicabilidade do elemento  
C) Remover o Maven do projeto  
D) Trocar Java por VB obrigatoriamente  
E) Usar apenas `ping`  

**8.** Implicit wait no WebDriver:

A) É sempre idêntico a `Thread.sleep`  
B) É configuração global de espera ao localizar elementos; padrão tipicamente 0  
C) Orquestra Jenkinsfile  
D) É o nome do artefato `junit`  
E) Significa *shift left*  

**9.** Afirmação **errada** típica de distrator:

A) Selenium pode rodar em job de CI  
B) Maven usa `pom.xml`  
C) Selenium automatiza builds e pipelines CI/CD e o CircleCI só gerencia repositórios Git  
D) Grid escala suítes  
E) JUnit costuma vir com `scope` test  

**10.** Declarar dependências Spring Boot + driver PostgreSQL no projeto Maven ocorre em:

A) `server.xml` do Tomcat como único lugar  
B) `pom.xml`  
C) `data.xml` padrão Maven  
D) `manifest.xml` de dependências  
E) `web.xml` listando groupId  

**11.** JUnit, em relação ao Selenium:

A) São a mesma ferramenta  
B) JUnit = unidade (Java); Selenium = UI/browser (podem combinar)  
C) JUnit só roda em Delphi  
D) Selenium substitui o Surefire  
E) JUnit é NGFW  

**12.** No pipeline DevOps, a ordem mental mais coerente é:

A) Deploy cego → depois versionar  
B) Versionar → build (Maven) → testes (JUnit/Selenium) → promover artefato  
C) Só Selenium sem repositório  
D) Só QoS sem código  
E) Apagar o `pom` antes do commit  

**13.** `NoSuchElementException` após animação sugere, antes de tudo:

A) Problema de sincronização/espera do elemento  
B) Obrigar uso de OSPF  
C) Trocar HTTPS por Telnet  
D) Remover Explicit Wait sempre  
E) Que Maven virou VCS  

**14.** Qual NÃO é papel do Maven?

A) Gerir dependências  
B) Empacotar projeto Java  
C) Servir como controle de versão distribuído completo tipo Git  
D) Rodar fase `test`  
E) Descrever o projeto no POM  

**15.** Síntese da revisão:

A) Maven = `pom.xml` + coords + scopes; Selenium = UI/WebDriver/esperas/Grid; ambos no CI, papéis distintos  
B) Selenium = Maven = Jenkins  
C) Grid = groupId  
D) Explicit wait = firewall  
E) `pom.java` é o padrão atual  

---

### Gabarito comentado (treino 1–15)

1. **B** — `pom.xml`.  
2. **B** — GAV.  
3. **B** — Só testes / fora do artefato típico.  
4. **B** — UI/E2E.  
5. **B** — Build × teste UI.  
6. **B** — Distribuição/escala.  
7. **B** — Explicit Wait.  
8. **B** — Global; default 0.  
9. **C** — Distrator clássico FCC.  
10. **B** — Dependências no POM.  
11. **B** — Unidade × UI.  
12. **B** — Fluxo CI típico.  
13. **A** — Sincronização.  
14. **C** — Maven ≠ Git.  
15. **A** — Síntese.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** FCC (preferência Analista TI), com banca/órgão/ano. Sem invenção.

**16.** (FCC – TRT-1/2025 – Analista Judiciário – TI) O time de TI configurou o Selenium WebDriver para testes no Chrome. O script falha ao clicar em "Enviar" com `NoSuchElementException`; o botão é carregado dinamicamente após animação. A abordagem mais adequada é

A) configurar o WebDriver para ignorar exceções e continuar a execução.  
B) atualizar o Selenium WebDriver e o Selenium IDE para as versões mais recentes.  
C) implementar Implicit Wait no script para executar após o carregamento do botão.  
D) aumentar o tempo de execução do script para garantir que o botão seja carregado.  
E) utilizar Explicit Wait para aguardar a presença do botão na página antes de interagir.  

**17.** (FCC – TRT-20/2024 – Analista Judiciário – TI) Uma equipe deseja adotar DevOps para automatizar CI/CD. Dentre as ferramentas, encontra-se o

A) Selenium, que é utilizado para automação de builds, testes de integração e execução de pipelines de CI/CD, que integra-se ao CircleCI, que faz a gestão de repositórios Git. O CircleCI é compatível apenas com as linguagens Go, Java e Ruby, apresentando incompatibilidade com Python, Ruby, Scala, Node.js, PHP e Haskell.  
B) Git. Esta ferramenta precisa ser integrada ao GitHub, uma vez que são interdependentes. Usadas em conjunto, conseguem realizar o controle de versão e proporcionar a integração contínua.  
C) GitHub Actions, que permite a criação de workflows de CI/CD diretamente no GitHub. Os workflows são definidos no diretório `.github/workflows` de um repositório, que pode ter vários workflows, cada um dos quais podendo executar um conjunto diferente de tarefas, como criar e testar pull requests.  
D) Jenkins, que é uma ferramenta escrita em PHP, exclusiva para automação de testes, que integra-se ao Git para armazenar o histórico dos testes automatizados executados.  
E) Maven, que é uma ferramenta proprietária para automação de builds, que também possui capacidades nativas de controle de versões e pipelines de CI/CD, similar ao GitLab CI/CD. Um projeto Maven fica atrelado a um arquivo principal: o pom.java.  

**18.** (FCC – TRT-24/2017 – Analista Judiciário – TI) Considere que um Analista está utilizando a ferramenta Maven e precisa definir dependências no POM, na tag `<dependencies>`. Para depender do JUnit 4.12 **apenas** na execução dos testes, sem incluir a dependência no artefato gerado, a caixa I deve ser preenchida com

A) `<groupId>junit</groupId> <version>4.12</version> <type>compile</type>`  
B) `<groupId>junit</groupId> <artifactId>junit 4.12</artifactId> <type>test</type>`  
C) `<groupId>junit</groupId> <artifactId>junit</artifactId> <version>4.12</version> <scope>test</scope>`  
D) `<groupId>junit</groupId> <version>4.12</version> <exclusions>system</exclusions>`  
E) `<groupId>junit 4.12</groupId> <artifactId>junit</artifactId> <type>test</type> <scope>system</scope>`  

**19.** (FCC – TRT-23/2016 – Analista Judiciário – TI) Para utilizar o OpenSource Selenium, um Analista deve conhecer suas principais ferramentas. Uma delas permite distribuir os testes em múltiplas máquinas, reduzindo o tempo de uma suíte; é ideal para escalonar suítes grandes ou executá-las em múltiplos ambientes. Essa ferramenta é o Selenium

A) SELECT.  
B) RC.  
C) IDE.  
D) TYPE.  
E) GRID.  

**20.** (FCC – TRT-14/2016 – Analista Judiciário – TI) Ao escrever testes funcionais, um Analista precisa de controle de espera para disparar uma ação depois de outra. No Selenium WebDriver, pode optar por um controle de espera

A) implícito, como o `Thread.sleep()`, que determina um tempo fixo que alguma ação no teste precisa esperar antes de ser executada. Se este tempo for ultrapassado, o teste não falhará.  
B) explícito, como o `Thread.sleep()`. Se a ação estiver pronta para ser executada antes do tempo fixado, o teste continuará.  
C) explícito, como o `WebDriver.manage().timeouts()`, que permite esperar por um certo elemento na tela por um tempo de espera mínimo. Se este tempo for ultrapassado, o teste falhará.  
D) implícito, cujo valor padrão é 0. Este tempo deve ser baixo, para evitar que o Selenium fique esperando por um elemento na tela antes de acusar um erro de tempo de espera excedido.  
E) explícito, cujo valor padrão é 10. Seu valor não deve ser baixo para evitar problemas em controle de tempo entre ações no teste.  

**21.** (FCC – TJ-CE/2022 – Analista Judiciário – Sistemas da Informação) Em uma aplicação Spring Boot que conecta a PostgreSQL, um Analista inseriu configurações do tipo:

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
</dependency>
```

Tais configurações foram inseridas corretamente no arquivo

A) `manifest.xml`.  
B) `web.xml`.  
C) `pom.xml`.  
D) `server.xml`.  
E) `data.xml`.  

**22.** (FCC – TRT-15/2024 – Analista Judiciário – TI) Maria quer testar o código antes da produção e decidiu utilizar o JUnit, que

A) é um framework utilizado para testes de desempenho e carga em aplicativos Java.  
B) é um framework de teste de unidade para Java que facilita a criação e execução de testes automatizados.  
C) permite a geração automática de relatórios de cobertura de código sem precisar de ferramentas adicionais.  
D) é especificamente projetado para testar interfaces de usuário em aplicações Java Swing.  
E) é uma ferramenta que permite a execução paralela de testes de integração distribuídos.  

**23.** (FCC – TRT-19/2022 – Analista Judiciário – TI) O teste automatizado de software que simula situações que normalmente não aconteceriam (como toques acidentais em áreas não interativas), para analisar se nenhum ponto da interface apresenta problemas, é conhecido como teste de

A) Unit.  
B) Stress.  
C) GUI.  
D) Block.  
E) API.  

**24.** (FCC – TRT-15/2023 – Analista Judiciário – TI) Considere os textos:  
I. Verifica se diferentes módulos ou serviços usados pelo aplicativo funcionam bem juntos… custo maior… várias partes ativas.  
II. Verifica recursos, funcionalidades e usabilidade de negócios críticos… valida a aplicação em relação ao SRS.  
Os tipos de teste são, correta e respectivamente,

A) Unitário e Funcional.  
B) Funcional e Integração.  
C) Unitário e Integração.  
D) Integração e Funcional.  
E) Funcional e Unitário.  

**25.** (FCC – TRT-12/2023 – Analista Judiciário – TI) Na abordagem DevOps, a integração e a entrega contínuas (CI/CD) juntas são comumente tratadas como um processo relacionado, conhecido como

A) hub register.  
B) pipeline.  
C) git hub.  
D) container register.  
E) end-to-end.  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Fonte / nota |
|---|------|----------------|
| 16 | **E** | FCC TRT-1/2025 — Explicit Wait — Quizinho |
| 17 | **C** | FCC TRT-20/2024 — Actions; Selenium/Maven como distratores — Quizinho |
| 18 | **C** | FCC TRT-24/2017 — GAV + `scope` test — QConcursos Q839364 / Quizinho |
| 19 | **E** | FCC TRT-23/2016 — Selenium GRID — Estude Grátis / Questionei |
| 20 | **D** | FCC TRT-14/2016 — implicit default 0 — Estude Grátis; gabarito oficial Q44=D |
| 21 | **C** | FCC TJ-CE/2022 — deps no `pom.xml` — Quizinho |
| 22 | **B** | FCC TRT-15/2024 — JUnit unidade — Quizinho (semana 3) |
| 23 | **C** | FCC TRT-19/2022 — teste GUI — Quizinho |
| 24 | **D** | FCC TRT-15/2023 — Integração + Funcional — Quizinho |
| 25 | **B** | FCC TRT-12/2023 — CI/CD = pipeline — Quizinho |

Letras: E, C, C, E, D, C, B, C, D, B.

---

### Verificação interna

| Checagem | Status |
|----------|--------|
| Revisão Maven + Selenium nível FCC | 1.2–1.8 |
| Anti-confusão papéis (Q17) | OK |
| Explicit / Implicit / Grid | Q16, Q19, Q20 |
| Treino 1–15 | OK |
| 16–25 reais + citação | 10/10 |
| Gabarito só no final | OK |
| Grafia *Sellenium* do edital | Cabeçalho |

+++
title = '05 — CI/CD (GitLab, Jenkins, Maven, CircleCI)'
date = '2026-10-16'
draft = false
weight = 5
tags = ['semana-03', 'cicd']
categories = ['estudo']
+++
# CI/CD · GitLab CI · Jenkins · Maven · CircleCI · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — CI/CD (conceitos) + GitLab CI/CD, Jenkins, Maven, CircleCI  
**Escopo deste arquivo:** Integração Contínua / Entrega Contínua / Deploy Contínuo; pipeline-as-code; **GitLab CI** (`.gitlab-ci.yml`, stages, jobs, `default`, `include`); **Jenkins** (Jenkinsfile, Declarative/Scripted, Groovy DSL); **Maven** (`pom.xml`, ciclo de vida, papel no build Java); **CircleCI** (`config.yml`, workflows/jobs); contraste com GitHub Actions e anti-confusão com Git/Docker/Selenium.  
**Fora de escopo aqui:** Git/GitHub em profundidade (outro arquivo da semana 3); Docker/K8s/Rancher; RabbitMQ; Selenium além do encaixe “teste no pipeline”; OWASP/DevSecOps profundo (semana de segurança).

---

## 1. Material

### 1.1 CI / CD / CD — o trio cobrável

| Sigla | Nome | Ideia de prova |
|-------|------|----------------|
| **CI** | *Continuous Integration* | Integrar código com frequência; build + testes automatizados a cada *push*/MR |
| **CD** | *Continuous Delivery* | Artefato sempre **pronto** para ir a produção (deploy pode ser manual/aprovado) |
| **CD** | *Continuous Deployment* | Deploy em produção **automático** após o pipeline verde (sem gate humano) |

Flash FCC: as duas “CD” compartilham a sigla — o enunciado distingue **entrega** (pronto + liberação controlada) de **implantação** (automática).

Pergunta mental do pipeline:

> **A cada alteração versionada, o sistema constrói, testa e prepara (e, se for o caso, implantar) o software de forma repetível e auditável?**

### 1.2 Por que CI/CD no eixo DevOps / PDPJ

No edital: **DevOps e DevSecOps** listam Jenkins, Maven, CircleCI, Git, GitHub, **GitLab CI/CD**, Selenium, testes automatizados; na arquitetura PDPJ-Br aparece **CI/CD** como prática do ciclo de entrega.

Três âncoras:

| Âncora | Frase cobrável |
|--------|----------------|
| Cultura | DevOps: Dev + Ops colaboram; automação reduz *hand-off* lento |
| Prática | Pipeline que valida o commit antes (e depois) de promover ambientes |
| Segurança | DevSecOps = inserir checagens de segurança **no** pipeline (visão; não detalhe OWASP aqui) |

Anti-padrão cobrado por distrator: “CI/CD = só ferramenta X” — CI/CD é **prática**; Jenkins/GitLab CI/CircleCI são **orquestradores**; Maven é **build** Java; Selenium é **teste** (UI/E2E).

### 1.3 Pipeline-as-code — mapa mental

| Ferramenta | Arquivo típico | Linguagem / formato |
|------------|----------------|---------------------|
| **GitLab CI/CD** | `.gitlab-ci.yml` (raiz do repo) | **YAML** |
| **Jenkins** | `Jenkinsfile` (raiz ou path configurado) | **Groovy** (DSL Declarative ou Scripted) |
| **CircleCI** | `.circleci/config.yml` | **YAML** |
| **GitHub Actions** | `.github/workflows/*.yml` | **YAML** (1 arquivo ≈ 1 workflow) |

Regra de ouro: o pipeline **vive no repositório** com o código → versionado, revisável, reproduzível.

### 1.4 Peças comuns (independentes da ferramenta)

| Peça | Função |
|------|--------|
| **Trigger** | Evento que dispara: push, MR/PR, tag, schedule, manual |
| **Stage / Stage group** | Fase lógica (build → test → package → deploy) |
| **Job** | Unidade de trabalho executável (pode haver vários por stage) |
| **Step / script** | Comando concreto (`mvn test`, `docker build`…) |
| **Agent / Runner / Executor** | Máquina/container que **roda** o job |
| **Artifact** | Saída persistida (JAR, relatório, imagem) para stages seguintes |
| **Cache** | Acelera builds (deps Maven, *node_modules*…) |

Ordem típica (visão de prova):

```
commit/MR → checkout → build (Maven) → testes → empacotar → (scan/qualidade) → publicar → deploy
```

### 1.5 GitLab CI/CD — o que a FCC gosta

**Arquivo:** `.gitlab-ci.yml` na raiz (ou *includes*).

Conceitos-chave:

| Conceito | Ideia |
|----------|-------|
| **`stages`** | Ordem global das fases; jobs do mesmo stage podem rodar em paralelo |
| **Job** | Bloco nomeado com `script:` (obrigatório na forma clássica) |
| **`image`** | Imagem Docker do job (ex.: `maven:3.9-eclipse-temurin-21`) |
| **`only` / `rules`** | Quando o job roda (branch, MR, variáveis) |
| **`artifacts`** | Arquivos guardados após o job |
| **`cache`** | Cache entre pipelines |
| **Runner** | Agente GitLab que executa jobs (shared / específico / tags) |

**Global keywords** cobradas (TRT22/FCC e afins):

| Keyword | Papel |
|---------|-------|
| **`default`** | Valores padrão herdados pelos jobs (`interruptible`, `timeout`, `image`…) |
| **`include`** | Importa YAML local (`local:`) ou remoto (`remote:`) para compor o pipeline |
| **`variables`** | Variáveis globais |
| **`workflow`** | Regras de quando o pipeline inteiro é criado |
| **`stages`** | Lista ordenada de estágios |

Flash: `default` ≠ `tags`; `include` ≠ `directories` (distrator clássico). `dependencies` / `environment` / `pages` são de **job** (ou job reservado), não o par “default + include”.

Esqueleto mental:

```yaml
stages:
  - build
  - test
  - deploy

default:
  image: maven:3.9-eclipse-temurin-21
  interruptible: true

include:
  - local: .gitlab/ci/*.gitlab-ci.yml

build-job:
  stage: build
  script:
    - mvn -B clean package -DskipTests

test-job:
  stage: test
  script:
    - mvn test
  artifacts:
    reports:
      junit: target/surefire-reports/TEST-*.xml
```

### 1.6 Jenkins — servidor de automação

| Fato | Detalhe cobrável |
|------|------------------|
| O que é | Servidor de automação **open source** para CI/CD (escrito em **Java**, não PHP) |
| Pipeline-as-code | **`Jenkinsfile`** no SCM |
| DSL | Tipicamente **Groovy** — *Declarative Pipeline* (`pipeline { ... }`) ou *Scripted* |
| Extensão | Ecossistema enorme de **plugins** (Git, Maven, Docker, JUnit…) |
| Agentes | Master/controller + agents; `agent any` / label / Docker |

**Declarative** (visão prova):

```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn test'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }
  }
}
```

Distratores FCC frequentes:

- Jenkins **não** é “só teste” nem “escrito em PHP”;
- Jenkinsfile **não** é `pom.java` nem substituto do Maven;
- Declarative ≠ “Pipejenkins” inventado — o nome cobrado é **Jenkinsfile** + Groovy DSL.

### 1.7 Maven — build Java (não é orquestrador CI)

| Ideia | Conteúdo |
|-------|----------|
| O que é | Ferramenta de **build/gestão de projeto** Java (Apache, open source) |
| Arquivo | **`pom.xml`** (*Project Object Model*) — **não** `pom.java` |
| Coordenadas | `groupId` : `artifactId` : `version` |
| Ciclo de vida padrão (treino) | `validate` → `compile` → `test` → `package` → `verify` → `install` → `deploy` |
| Comandos típicos no CI | `mvn clean package`, `mvn test`, `mvn verify` |

Papéis na prova:

| Ferramenta | Papel |
|------------|-------|
| **Maven** | Compila, testa unitário (Surefire), empacota JAR/WAR, resolve deps |
| **Jenkins / GitLab CI / CircleCI** | **Orquestra** quando/como chamar Maven e o que fazer depois |
| **Git** | Versiona código + `pom.xml` + `Jenkinsfile` / `.gitlab-ci.yml` |

Flash: Maven **não** é “proprietário”, **não** faz controle de versão nativo como Git, **não** substitui GitLab CI.

Fases cobráveis em uma linha:

- **`compile`** — compila código principal  
- **`test`** — testes unitários (código de teste)  
- **`package`** — gera artefato (`target/*.jar`)  
- **`install`** — instala no repositório local `~/.m2`  
- **`deploy`** — publica em repositório remoto (Nexus/Artifactory etc.)

### 1.8 CircleCI — CI/CD em YAML

| Ideia | Conteúdo |
|-------|----------|
| Config | **`.circleci/config.yml`** |
| Estrutura | `workflows` → `jobs` → `steps` |
| Executor | Docker / machine / macOS (visão) |
| Orbs | Pacotes reutilizáveis de config (noção) |

Contraste FCC clássico (TJ-CE 2022 e afins):

| | **CircleCI** | **GitHub Actions** |
|--|--------------|-------------------|
| Arquivo | Um `config.yml` pode definir **vários** workflows | Em geral **um arquivo YAML por workflow** em `.github/workflows/` |
| Formato | YAML | YAML |
| Destaque citado | Paralelismo / *test splitting* automático inteligente | Workflows por evento (PR, push) |

CircleCI **não** é “gestor de repositório Git” (isso é Git/GitHub/GitLab); **não** se limita a Go/Java/Ruby — roda muitas linguagens via imagens.

### 1.9 Tabela anti-confusão (ouro de prova)

| Afirmação errada (distrator) | Correção |
|------------------------------|----------|
| Maven = pipeline CI/CD completo | Maven = **build**; CI orquestra Maven |
| `pom.java` | **`pom.xml`** |
| Jenkins escrito em PHP / só testes | Java; build+test+deploy via plugins/pipeline |
| CircleCI só gerencia Git | CircleCI = **CI/CD**; Git versiona |
| Git sozinho = CI | Git = VCS; CI = Jenkins/GitLab CI/CircleCI/Actions |
| Selenium = build Maven | Selenium = **automação de testes** (UI); encaixa **no** pipeline |
| `default` + `directories` no GitLab | Par cobrado: **`default`** + **`include`** |
| GitLab CI em Groovy | GitLab CI = **YAML**; Groovy = Jenkinsfile |

### 1.10 Encaixe DevSecOps / testes (visão curta)

No pipeline moderno (visão edital):

```
build (Maven) → testes unitários → (análise estática / SCA) → testes integração/E2E (ex.: Selenium) → artefato → deploy
```

- **DevSecOps:** segurança *shift-left* — scans e políticas **dentro** do CI, não só no fim.  
- **Selenium:** ferramenta de testes automatizados de interface; **não** substitui Jenkins/Maven.  
- Artefato aprovado no CI alimenta CD (entrega/implantação).

### 1.11 Cartão mental (síntese)

> **CI/CD** = prática de integrar e entregar continuamente.  
> **GitLab CI** = `.gitlab-ci.yml` (YAML; `stages`, jobs, `default`, `include`).  
> **Jenkins** = Jenkinsfile (Groovy Declarative/Scripted) + plugins.  
> **Maven** = `pom.xml` + ciclo de vida Java.  
> **CircleCI** = `.circleci/config.yml` (workflows/jobs).  
> Não misturar: build ≠ orquestrador ≠ VCS ≠ teste UI.

---

## 2. Questões de treino (1–15)

**1.** Integração Contínua (CI), em essência, consiste em:

A) Implantar automaticamente em produção sem qualquer teste  
B) Integrar mudanças com frequência, tipicamente com build e testes automatizados  
C) Substituir o Git por Maven  
D) Eliminar a necessidade de `pom.xml`  
E) Ser sinônimo exclusivo de Kubernetes  

**2.** Continuous Delivery difere de Continuous Deployment, em regra, porque:

A) Delivery nunca executa testes  
B) Delivery deixa o artefato pronto para produção, podendo haver aprovação humana no deploy; Deployment automatiza o deploy em produção após o pipeline verde  
C) Deployment proíbe o uso de YAML  
D) Delivery só existe no Jenkins; Deployment só no Maven  
E) São termos idênticos e nunca distinguidos  

**3.** O arquivo de configuração padrão do GitLab CI/CD no repositório é:

A) `Jenkinsfile`  
B) `pom.java`  
C) `.gitlab-ci.yml`  
D) `.github/workflows/ci.yml` exclusivamente  
E) `config.groovy`  

**4.** No GitLab CI, as *global keywords* que definem, respectivamente, padrões herdados pelos jobs e a composição do pipeline a partir de outros YAML são:

A) `tags` e `directories`  
B) `dependencies` e `environment`  
C) `dependencies` e `pages`  
D) `default` e `include`  
E) `default` e `directories`  

**5.** Jenkins, no contexto de CI/CD cobrado em concursos, é tipicamente:

A) Ferramenta de build Java cujo arquivo principal é `pom.java`  
B) Servidor de automação open source (Java) que usa **Jenkinsfile** (Groovy DSL)  
C) Linguagem proprietária exclusiva de testes UI  
D) Substituto do protocolo OAuth2  
E) Sistema de arquivos do H2  

**6.** Assinale a afirmação correta sobre o Maven:

A) É proprietário e equivale ao GitLab CI  
B) Gerencia build Java via **`pom.xml`** e ciclo de vida (`compile`, `test`, `package`…)  
C) Orquestra runners do GitLab no lugar do `.gitlab-ci.yml`  
D) Seu arquivo principal é o Jenkinsfile  
E) Substitui o controle de versão Git  

**7.** CircleCI define workflows, em regra, no arquivo:

A) `pom.xml`  
B) `Jenkinsfile`  
C) `.circleci/config.yml`  
D) `application.properties`  
E) `.gitlab-ci.yml` obrigatoriamente  

**8.** Em comparação citada pela FCC entre CircleCI e GitHub Actions, é correto afirmar que:

A) Ambas usam apenas Groovy e um único Jenkinsfile  
B) CircleCI usa `config.yml` (podendo agrupar workflows); GitHub Actions tipicamente usa um YAML por workflow em `.github/workflows/`  
C) Maven e Kubernetes são as ferramentas 1 e 2 desse contraste  
D) CircleCI não usa YAML  
E) GitHub Actions exige `pom.java`  

**9.** Qual linha melhor separa papéis?

A) Maven = VCS; Git = build; Jenkins = banco H2  
B) Git = versionamento; Maven = build Java; Jenkins/GitLab CI/CircleCI = orquestração CI/CD  
C) Selenium = empacotamento JAR; Maven = teste UI  
D) CircleCI = único IdP da PDPJ  
E) GitLab CI = ciclo de vida `install` do Maven  

**10.** No ciclo de vida padrão do Maven, a fase que gera o artefato empacotado (JAR/WAR) é tipicamente:

A) `validate` apenas  
B) `package`  
C) `clean` (que não empacota)  
D) `site` exclusivamente  
E) `help`  

**11.** Um `Jenkinsfile` Declarative costuma:

A) Ser escrito em YAML puro idêntico ao `.gitlab-ci.yml`  
B) Usar bloco `pipeline { agent ... stages { stage(...) { steps { ... } } } }` em Groovy  
C) Substituir obrigatoriamente o `pom.xml`  
D) Existir apenas como `Pipejenkins`  
E) Ser o nome do runner do GitLab  

**12.** Selenium, no eixo DevOps do edital, encaixa-se melhor como:

A) Orquestrador CI substituto do Jenkins  
B) Ferramenta de automação de **testes** (tipicamente UI/E2E) que pode rodar **dentro** do pipeline  
C) Arquivo `pom.java`  
D) Global keyword `include` do GitLab  
E) Servidor Eureka  

**13.** Assinale a opção **incorreta**:

A) GitLab CI usa YAML e runners  
B) Jenkinsfile tipicamente usa Groovy  
C) Maven usa `pom.xml`  
D) CircleCI usa `.circleci/config.yml`  
E) Maven é o único orquestrador de pipeline CI/CD listado no edital no lugar de Jenkins/GitLab CI/CircleCI  

**14.** Em DevSecOps aplicado ao CI/CD, a ideia central cobrável é:

A) Remover todos os testes do pipeline  
B) Incorporar práticas/checagens de segurança ao longo da entrega automatizada (*shift-left*), não só no fim  
C) Trocar Git por FTP obrigatório  
D) Proibir o uso de `mvn test`  
E) Confiar apenas em deploy manual sem build  

**15.** Qual opção melhor resume este subtópico?

A) CI/CD = prática; GitLab CI (YAML + `default`/`include`); Jenkins (Jenkinsfile/Groovy); Maven (`pom.xml`/ciclo de vida); CircleCI (`config.yml`)  
B) Eureka = CI; Zuul = Maven; Keycloak = CircleCI  
C) `pom.java` + Jenkins em PHP + CircleCI só Git  
D) Continuous Deployment proíbe Continuous Integration  
E) Selenium substitui Git, Maven e Jenkins simultaneamente  

---

### Gabarito comentado (treino 1–15)

1. **B** — CI = integração frequente + build/testes.  
2. **B** — Delivery = pronto (gate possível); Deployment = auto em produção.  
3. **C** — `.gitlab-ci.yml`.  
4. **D** — `default` + `include` (padrão FCC/TRT).  
5. **B** — Jenkins = automação + Jenkinsfile/Groovy.  
6. **B** — Maven + `pom.xml` + lifecycle.  
7. **C** — `.circleci/config.yml`.  
8. **B** — Contraste CircleCI × Actions.  
9. **B** — VCS × build × orquestrador.  
10. **B** — `package` empacota.  
11. **B** — Declarative Groovy.  
12. **B** — Teste UI no pipeline.  
13. **E** — Maven **não** é o orquestrador CI; a incorreta é E.  
14. **B** — Segurança no fluxo CI/CD.  
15. **A** — Síntese do arquivo 05.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** de provas FCC (preferência TRT Analista TI), com banca/órgão/ano citados. Textos conferidos em Quizinho, QConcursos, Estude Grátis e/ou prova oficial.

**16.** (FCC – TRT-20/2024 – Analista Judiciário – Tecnologia da Informação) Uma equipe deseja adotar abordagem DevOps para automatizar o processo de integração e entrega contínua (CI/CD). Para isso, é necessário integrar diversas ferramentas para otimizar a automação dos testes e a entrega do produto. Dentre essas ferramentas, encontra-se o

A) Selenium, que é utilizado para automação de builds, testes de integração e execução de pipelines de CI/CD, que integra-se ao CircleCI, que faz a gestão de repositórios Git. O CircleCI é compatível apenas com as linguagens Go, Java e Ruby, apresentando incompatibilidade com Python, Ruby, Scala, Node.js, PHP e Haskell.  
B) Git. Esta ferramenta precisa ser integrada ao GitHub, uma vez que são interdependentes. Usadas em conjunto, conseguem realizar o controle de versão e proporcionar a integração contínua.  
C) GitHub Actions, que permite a criação de workflows de CI/CD diretamente no GitHub. Os workflows são definidos no diretório `.github/workflows` de um repositório, que pode ter vários workflows, cada um dos quais podendo executar um conjunto diferente de tarefas, como criar e testar pull requests.  
D) Jenkins, que é uma ferramenta escrita em PHP, exclusiva para automação de testes, que integra-se ao Git para armazenar o histórico dos testes automatizados executados.  
E) Maven, que é uma ferramenta proprietária para automação de builds, que também possui capacidades nativas de controle de versões e pipelines de CI/CD, similar ao GitLab CI/CD. Um projeto Maven fica atrelado a um arquivo principal: o pom.java.  

**17.** (FCC – TRT-22/2022 – Analista Judiciário – Tecnologia da Informação) Considere o recorte de um arquivo `.gitlab-ci.yml` que traz algumas opções de configuração de um pipeline GitLab CI/CD, nas seções que são separadas pelas *global keywords*.

```yaml
stages:
  - sync
  - prepare
  - build-images

I:
  interruptible: true
  timeout: 90m

workflow:
  # regras

variables:
  # variáveis

II:
  - local: .gitlab/ci/*.gitlab-ci.yml
  - remote: 'https://gitlab.com/gitlab-org/frontend/-/merge_request_pipelines.yml'
```

As *global keywords* I e II correspondem, correta e respectivamente, a

A) `tags` e `directories`  
B) `dependencies` e `environment`  
C) `dependencies` e `pages`  
D) `default` e `include`  
E) `default` e `directories`  

**18.** (FCC – TRT-17/2022 – Analista Judiciário – Tecnologia da Informação) Para construir um pipeline como código no Jenkins, um analista utilizou um arquivo de texto simples conhecido como

A) Pipejenkins que especifica o encadeamento dos estágios do processo DevSecOps.  
B) Scripted que especifica o enlace crítico entre as etapas primárias DevOps.  
C) JenkinsFile que normalmente contém um código Groovy Domain Specific Language.  
D) JenkinsGroovy que especifica o enlace crítico usando código DomainPline.  
E) Declarative que especifica o enlace crítico usando código GroovyPline.  

**19.** (FCC – TRT-15/2024 – Analista Judiciário – Tecnologia da Informação) Uma equipe de desenvolvimento utiliza o GitLab CI/CD para automação de builds e deploys. Para otimizar o processo de integração contínua (CI), foi configurado um webhook para disparar automaticamente um pipeline de build assim que um commit é enviado ao branch main. No entanto, durante uma atualização do ambiente de produção, percebeu-se que o webhook falhou e o pipeline não foi executado, causando atraso no deploy. Após uma análise técnica, verificou-se que o problema ocorreu devido a uma falha no endpoint do webhook, que não conseguiu validar a mensagem enviada. A solução técnica que deve ser implementada para garantir que o webhook não falhe novamente devido a erros de validação é

A) configurar uma API reversa para verificar e redirecionar as mensagens do webhook para um endpoint alternativo.  
B) implementar SSL offloading no servidor para garantir que as mensagens webhook trafeguem com criptografia SSL.  
C) configurar um balanceador de carga para distribuir as requisições do webhook entre múltiplos endpoints redundantes de validação de mensagens.  
D) criar um proxy reverso com logs detalhados para monitorar a comunicação e diagnosticar falhas no endpoint do webhook.  
E) habilitar o recurso de verificação de assinatura (usando token) para checar a integridade das mensagens enviadas pelo webhook.  

**20.** (FCC – TJ-CE/2022 – Analista Judiciário – Ciência da Computação / Sistemas da Informação) Considere duas ferramentas 1 e 2 que permitem a criação de workflows e criam, testam, publicam, fazem o release e implantam código automaticamente.  
— Similaridades entre as ferramentas 1 e 2 para a configuração de workflow: os arquivos são escritos em YAML e armazenados no repositório; workflows incluem um ou mais jobs; jobs incluem um ou mais passos ou comandos individuais; passos ou atividades podem ser reutilizados ou compartilhados com a comunidade.  
— Diferenças principais ao se fazer a migração da ferramenta 1 para a ferramenta 2: o paralelismo de teste automático da ferramenta 1 agrupa automaticamente os testes de acordo com as regras especificadas pelo usuário ou informações históricas de tempo e essa funcionalidade não está incorporada na ferramenta 2.  
— Ao fazer a migração de workflows: a ferramenta 1 define workflows no arquivo `config.yml`, que permite configurar mais de um workflow. A ferramenta 2 requer um arquivo de workflow para cada workflow e será necessário criar um novo arquivo de workflow para cada workflow configurado em `config.yml`.  
Pelas características apresentadas, as ferramentas 1 e 2 correspondem, correta e respectivamente, a

A) Docker e Jenkins.  
B) CircleCI e GitHub Actions.  
C) Maven e Kubernetes.  
D) Docker e Selenium.  
E) CircleCI e Jenkins.  

**21.** (FCC – TRT-24/2017 – Analista Judiciário – Tecnologia da Informação) Considere que um Analista está utilizando a ferramenta Maven e precisa definir que seu projeto depende de algumas outras ferramentas. Esta definição é feita inteiramente no POM, usando a tag `<dependencies>`, dentro da qual cada dependência é definida. Considere o trecho de código abaixo.

```xml
<dependency>
I
</dependency>
```

Em condições ideais, para que seja definido que o projeto depende do JUnit 4.12 e que o Maven adicione a dependência do JUnit apenas no momento em que os testes forem executados, garantindo que a dependência não seja adicionada no artefato que será gerado, a caixa I deve ser corretamente preenchida com

A)
```xml
<groupId>junit</groupId>
<version>4.12</version>
<test>compile</test>
```  
B)
```xml
<groupId>junit</groupId>
<artifactId>junit 4.12</artifactId>
<compile>test</compile>
```  
C)
```xml
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>4.12</version>
<scope>test</scope>
```  
D)
```xml
<groupId>junit</groupId>
<version>4.12</version>
<exclusion>system</exclusion>
```  
E)
```xml
<groupId>junit 4.12</groupId>
<artifactId>junit</artifactId>
<system>test</system>
<scope>system</scope>
```  

**22.** (FCC – TRT-12/2023 – Analista Judiciário – Tecnologia da Informação) Na abordagem DevOps, a integração e a entrega contínuas (CI/CD) juntas são comumente tratadas como um processo relacionado, conhecido como

A) hub register.  
B) pipeline.  
C) git hub.  
D) container register.  
E) end-to-end.  

**23.** (FCC – TRT-3/2015 – Técnico Judiciário – Tecnologia da Informação) Um técnico de TI está editando um arquivo XML simplificado, que tem o código fonte mostrado abaixo.

```xml
<project>
  <modelVersion>2.0.0</modelVersion>
  <groupId>br.com.trt3</groupId>
  <artifactId>Conect_Project</artifactId>
  <version>1.0</version>
</project>
```

Este arquivo é a unidade de configuração básica do Maven e deve ficar na raiz do projeto para que se possa chamar as targets de build do projeto. Nele, pode-se declarar a estrutura, dependências e características do projeto. Trata-se do arquivo

A) build.xml  
B) log4j.xml  
C) maven.xml  
D) mvn.xml  
E) pom.xml  

**24.** (FCC – TJ-CE/2022 – Analista Judiciário – Ciência da Computação / Sistemas da Informação) Em uma aplicação que utiliza Spring Boot e conecta a um banco de dados PostgreSQL, um Analista realizou as configurações abaixo.

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

**25.** **(FCC / TJ-CE / 2022)**  
Para usar o Gitlab CI/CD são necessários os seguintes passos iniciais:  
— Certificar-se que haja runners disponíveis para executar os jobs. Se não houver um runner, instalar o GitLab Runner e registrar um runner para a instância, projeto ou grupo.  
— Criar um arquivo .. I.. na raiz do repositório. Este arquivo é o local onde se definem os jobs CI/CD.  
— Quando for feito o commit deste arquivo no repositório, o runner executará os jobs.  
Preenche corretamente a lacuna I:

A) `.gitlab-cicd.yml`  
B) `.gitlab-ci.yml`  
C) `.gitlab-cd.yaml`  
D) `.gitlab-cd.yml`  
E) `.gitlab-cicd.yaml`  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Fonte / nota |
|---|------|----------------|
| 16 | **C** | FCC TRT-20/2024 — GitHub Actions em `.github/workflows` (Quizinho) |
| 17 | **D** | FCC TRT-22/2022 — `default` + `include` (Quizinho / QConcursos) |
| 18 | **C** | FCC TRT-17/2022 — Jenkinsfile / Groovy DSL (Quizinho / Estude Grátis) |
| 19 | **E** | FCC TRT-15/2024 — assinatura de webhook com token (Quizinho) |
| 20 | **B** | FCC TJ-CE/2022 — CircleCI (`config.yml`) × GitHub Actions (Quizinho) |
| 21 | **C** | FCC TRT-24/2017 — Maven `groupId`/`artifactId`/`version` + `scope` test (QConcursos / Questionei) |
| 22 | **B** | FCC TRT-12/2023 — CI/CD = pipeline (Quizinho) |
| 23 | **E** | FCC TRT-3/2015 — arquivo Maven = `pom.xml` (prova / Estude Grátis) |
| 24 | **C** | FCC TJ-CE/2022 — dependências Spring/PostgreSQL no `pom.xml` (Quizinho) |
| 25 | **B** | FCC TJ-CE/2022 — arquivo `.gitlab-ci.yml` (Estude Grátis / QConcursos) |

Letras do gabarito neste bloco: **C D C E B C B E C B** (16–25).

---

### Verificação interna (consistência)

| Checagem | Status |
|----------|--------|
| Edital: Jenkins; Maven; CircleCI; GitLab CI/CD; CI/CD PDPJ; testes/Selenium noção | Coberto |
| Conceitos CI × Delivery × Deployment | Seção 1.1 |
| GitLab: `.gitlab-ci.yml`, stages/jobs, `default`, `include` | 1.5 + Q4/Q17 |
| Jenkins: Jenkinsfile, Groovy Declarative/Scripted, Java | 1.6 + Q5/Q11/Q18 |
| Maven: `pom.xml`, lifecycle, papel ≠ orquestrador | 1.7 + Q6/Q10/Q21/Q23/Q24 |
| CircleCI: `config.yml`, contraste Actions | 1.8 + Q7/Q8/Q20 |
| Anti-confusão Git/Selenium/Docker/distratores FCC | 1.9 + Q9/Q12/Q16 |
| Treino 1–15 com gabarito comentado | 15/15 intacto |
| Concurso 16–25 = questões **reais** + tabela Fonte/nota | 10/10 |
| Gabaritos oficiais (letras A–E presentes) | Conferidos |

---

*Arquivo 05 · CI/CD · GitLab CI · Jenkins · Maven · CircleCI · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 3 — DevOps / entrega.*

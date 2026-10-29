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

## 3. Questões estilo concurso FCC (16–25)

**16.** Uma equipe deseja automatizar integração e entrega contínua (CI/CD). Sobre as ferramentas do eixo DevOps, assinale a alternativa correta.

A) Selenium automatiza builds e pipelines CI/CD e o CircleCI gerencia apenas repositórios Git, sendo incompatível com Python e Node.js  
B) Git depende obrigatoriamente do GitHub para existir e, sozinho, realiza integração contínua completa  
C) GitHub Actions permite workflows de CI/CD definidos em `.github/workflows`, podendo criar/testar *pull requests* entre outras tarefas  
D) Jenkins é escrito em PHP, exclusivo para testes, e apenas armazena histórico de testes no Git  
E) Maven é proprietário, possui CI/CD nativo idêntico ao GitLab CI e usa o arquivo principal `pom.java`  

**17.** Considere o recorte conceitual de um `.gitlab-ci.yml` com seções separadas por *global keywords*. A seção que define configurações padrão aplicáveis aos jobs (ex.: `interruptible`, `timeout`) e a que importa outros arquivos YAML (`local:` / `remote:`) correspondem, respectivamente, a:

A) `tags` e `directories`  
B) `dependencies` e `environment`  
C) `dependencies` e `pages`  
D) `default` e `include`  
E) `default` e `directories`  

**18.** No Jenkins, o arquivo que tipicamente especifica o pipeline como código, em Domain Specific Language baseada em Groovy, é o:

A) `pom.java`  
B) `.gitlab-ci.yml`  
C) `Jenkinsfile`  
D) `config.yml` exclusivo do Maven  
E) `Pipejenkins`  

**19.** Acerca do Apache Maven, é correto afirmar:

A) Substitui o Git como sistema de controle de versão distribuído  
B) Utiliza o *Project Object Model* em **`pom.xml`** para gerenciar dependências, compilação, testes e empacotamento  
C) É o runner oficial exclusivo do GitLab CI  
D) Define workflows YAML em `.github/workflows`  
E) Foi escrito para substituir o Keycloak na PDPJ  

**20.** Considere duas ferramentas de CI/CD cujas configs são YAML no repositório; ambas organizam jobs e steps. A ferramenta 1 concentra workflows em `config.yml` e é citada por paralelismo/agrupamento inteligente de testes; a ferramenta 2 tipicamente exige um arquivo de workflow por workflow. As ferramentas 1 e 2 são, respectivamente:

A) Docker e Jenkins  
B) CircleCI e GitHub Actions  
C) Maven e Kubernetes  
D) Docker e Selenium  
E) CircleCI e Jenkins  

**21.** Assinale a opção que **inverte** indevidamente os papéis das ferramentas.

A) Git versiona; Maven constrói; Jenkins orquestra pipeline  
B) GitLab CI lê `.gitlab-ci.yml`; runners executam jobs  
C) CircleCI usa `.circleci/config.yml` com workflows/jobs  
D) Maven = orquestrador CI; Jenkins = `pom.xml`; GitLab CI = Groovy obrigatório; CircleCI = único VCS  
E) Selenium pode ser etapa de teste automatizado no pipeline  

**22.** Em um pipeline Java típico orquestrado por Jenkins ou GitLab CI, o comando Maven mais alinhado a “compilar, testar e gerar o artefato” em sequência do ciclo de vida é, em regra, alcançado por fases até:

A) apenas `mvn help:effective-pom` sem efeitos de build  
B) `package` (após `compile`/`test` conforme invocação), gerando o artefato em `target/`  
C) somente `mvn archetype:generate` em produção  
D) `git commit` executado pelo Maven nativamente  
E) `docker swarm init`  

**23.** Relacione corretamente arquivo e ferramenta.

I. `.gitlab-ci.yml`  
II. `Jenkinsfile`  
III. `pom.xml`  
IV. `.circleci/config.yml`

A) I-Jenkins; II-Maven; III-CircleCI; IV-GitLab  
B) I-GitLab CI; II-Jenkins; III-Maven; IV-CircleCI  
C) I-Maven; II-CircleCI; III-GitLab; IV-Jenkins  
D) I-Selenium; II-Git; III-Docker; IV-Eureka  
E) Todos equivalem ao mesmo formato Groovy  

**24.** Para fins de preparação no eixo CI/CD do edital (DevOps / PDPJ-Br), a síntese mais adequada é:

A) CI integra com build/testes; CD entrega/implanta; GitLab CI e CircleCI em YAML; Jenkins com Jenkinsfile/Groovy; Maven com `pom.xml` no build Java  
B) Continuous Deployment elimina a necessidade de testes; Maven é o GitLab CI  
C) `default` e `directories` são o par global keyword do GitLab cobrado pela FCC  
D) Jenkinsfile deve ser `pom.java`; CircleCI não usa YAML  
E) Selenium substitui Jenkins, Maven e GitLab CI  

**25.** Assinale a afirmação **incompatível** com o conteúdo programático e o uso correto das ferramentas.

A) Pipelines devem ser versionados junto ao código (*pipeline-as-code*)  
B) Jobs de CI podem publicar artefatos e relatórios de teste  
C) DevSecOps recomenda inserir verificações de segurança no fluxo de entrega  
D) O arquivo principal de um projeto Maven é o `pom.java`, e o Jenkins é ferramenta escrita em PHP exclusiva para UI tests  
E) Runners/agents executam os jobs definidos na configuração do pipeline  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **C** |
| 17 | **D** |
| 18 | **C** |
| 19 | **B** |
| 20 | **B** |
| 21 | **D** |
| 22 | **B** |
| 23 | **B** |
| 24 | **A** |
| 25 | **D** |

**Notas rápidas:** 16 C (Actions em `.github/workflows` — as demais repetem erros clássicos FCC); 17 D (`default` + `include`); 18 C (Jenkinsfile/Groovy); 19 B (`pom.xml`); 20 B (CircleCI × Actions); 21 D (única que **inverte** tudo); 22 B (`package`/artefato); 23 B (mapa arquivo→ferramenta); 24 A (síntese); 25 D (incompatível: `pom.java` + Jenkins PHP).

---

### Verificação interna (consistência)

| Checagem | Status |
|----------|--------|
| Edital: Jenkins; Maven; CircleCI; GitLab CI/CD; CI/CD PDPJ; testes/Selenium noção | Coberto |
| Conceitos CI × Delivery × Deployment | Seção 1.1 |
| GitLab: `.gitlab-ci.yml`, stages/jobs, `default`, `include` | 1.5 + Q4/Q17 |
| Jenkins: Jenkinsfile, Groovy Declarative/Scripted, Java | 1.6 + Q5/Q11/Q18 |
| Maven: `pom.xml`, lifecycle, papel ≠ orquestrador | 1.7 + Q6/Q10/Q19/Q22 |
| CircleCI: `config.yml`, contraste Actions | 1.8 + Q7/Q8/Q20 |
| Anti-confusão Git/Selenium/Docker/distratores FCC | 1.9 + Q9/Q12/Q16/Q21/Q25 |
| Treino 1–15 com gabarito comentado | 15/15 |
| FCC 16–25 com tabela + notas | 10/10 |
| Gabaritos únicos (sem ambiguidade intencional) | Conferidos |

---

*Arquivo 05 · CI/CD · GitLab CI · Jenkins · Maven · CircleCI · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 3 — DevOps / entrega.*

+++
title = '07 — DevSecOps + Selenium'
date = '2026-10-18'
draft = false
weight = 7
tags = ['semana-03', 'devsecops', 'selenium']
categories = ['estudo']
+++
# DevOps / DevSecOps + testes automatizados (Selenium — noção) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 07 — DevOps/DevSecOps + Selenium (Semana 3)  
**Escopo deste arquivo:** **DevOps** e **DevSecOps** (ideia cobrável); **CI/CD**; mapa das ferramentas do edital — **Jenkins**, **Maven**, **CircleCI**, **Git**, **GitHub**, **GitLab CI/CD**; **testes automatizados** e **Selenium** em **nível de noção** (o que é / o que não é); *shift left*; encaixe no pipeline.  
**Fora de escopo aqui:** Docker / Kubernetes / Rancher em profundidade (outros arquivos da semana 3); RabbitMQ / webhooks / eventos; Keycloak/OAuth2; JPA/Flyway; OWASP/LGPD em profundidade (semana de segurança); ágeis CMMI/ITIL (semana 4).

**Grafia do edital:** consta *Sellenium* (erro de digitação). Em prova e no mercado a forma correta é **Selenium**.

---

## 1. Material

### 1.1 DevOps — o problema e a ideia

**DevOps** = cultura + práticas + automação que aproximam **Desenvolvimento (Dev)** e **Operações (Ops)** para entregar software com **mais frequência**, **mais qualidade** e **menos atrito** entre “código pronto” e “rodando em produção”.

Anti-padrão clássico (cobrável):

- Dev “joga o pacote por cima do muro”;
- Ops sobe em produção sem o mesmo ritmo de testes;
- feedback de falha chega **tarde** (só em homolog/prod manual).

DevOps responde à pergunta:

> **Como integrar build, teste, empacotamento e entrega de forma contínua e automatizada, com colaboração Dev ↔ Ops?**

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Cultura | Colaboração Dev + Ops (e outros papéis) |
| Prática | Automação do ciclo: build → teste → entrega/deploy |
| Resultado | Entregas frequentes, feedback rápido, menos retrabalho manual |

Flash: DevOps **não** é “só usar Docker”, **não** é sinônimo de uma única ferramenta, **não** é “substituir testes por deploy cego”.

### 1.2 DevSecOps — segurança no ciclo (não no final)

**DevSecOps** = DevOps **com segurança integrada de ponta a ponta** no ciclo de vida (planejar → codificar → build → teste → release → operar → monitorar), e **não** só auditoria no final.

Ideia-ouro de prova: **shift left** (*deslocar para a esquerda*) = puxar checagens de segurança **para mais cedo** (já no desenvolvimento / no pipeline de CI), em vez de só na homologação tardia ou pós-incidente.

| Conceito | Leitura FCC |
|----------|-------------|
| DevOps | Automação + colaboração Dev/Ops + CI/CD |
| DevSecOps | Mesmo ciclo **+ segurança contínua** (SAST/DAST/sca/secrets etc. *como noção*) |
| Shift left | Segurança e qualidade **cedo** e **automatizadas** no pipeline |

Anti-padrão DevSecOps: criar um “time de segurança isolado” que só audita **depois** de tudo pronto, sem automação no pipeline — isso **não** é o espírito cobrado.

Flash: enunciado fala em **incorporar segurança ao pipeline**, **automatizar scans**, **treinar o time**, **detectar vulnerabilidades cedo** → **DevSecOps / shift left**.

### 1.3 CI e CD — não misturar

| Sigla | Nome | Ideia cobrável |
|-------|------|----------------|
| **CI** | *Continuous Integration* | Integrar código com frequência; build + testes automatizados a cada *commit*/MR/PR |
| **CD** (*Delivery*) | *Continuous Delivery* | Artefato **sempre pronto** para ir a produção (deploy pode ser um clique / aprovação) |
| **CD** (*Deployment*) | *Continuous Deployment* | Deploy em produção **automático** após o pipeline verde (sem gate humano a cada release) |

Em prova FCC, “CI/CD” costuma aparecer como **pacote**: integrar, testar e entregar/implantar de forma contínua. Se o enunciado distinguir Delivery × Deployment, use a tabela acima.

Pipeline mental mínimo:

```
Commit / Merge Request
        │
        ▼
   Build (ex.: Maven)
        │
        ▼
   Testes (unitários, integração, UI/Selenium…)
        │
        ▼
   Checagens de segurança (DevSecOps)
        │
        ▼
   Empacotar / publicar artefato
        │
        ▼
   Deploy (homolog / prod — conforme CD)
```

### 1.4 Mapa das ferramentas do edital (ouro de prova)

O edital lista ferramentas **com papéis diferentes**. A armadilha clássica da FCC é **trocar o papel** (ex.: dizer que Selenium orquestra pipeline, ou que Maven é VCS).

| Ferramenta | Papel cobrável | O que **não** é |
|------------|----------------|-----------------|
| **Git** | Controle de versão **distribuído** (histórico, branches, commits) | Plataforma de hospedagem; orquestrador de CI sozinho |
| **GitHub** | Hospedagem de repositórios Git + colaboração (PRs, Issues) + **Actions** (CI/CD nativo) | O mesmo que “Git” (Git funciona sem GitHub) |
| **GitLab CI/CD** | Pipelines CI/CD do **GitLab** (jobs/stages em YAML no repo, tipicamente `.gitlab-ci.yml`) | Ferramenta de teste de UI; build Java “puro” |
| **Jenkins** | Servidor de **automação / CI/CD** (pipelines, jobs, plugins); código aberto; tipicamente **Java** | Exclusivo de testes; escrito em PHP; “só Selenium” |
| **CircleCI** | Plataforma **CI/CD** (cloud/self-hosted); workflows em **YAML** (`config.yml`) | Contêiner orquestrador (K8s); VCS; teste de UI |
| **Maven** | Automação de **build** Java (ciclo de vida, dependências); arquivo **`pom.xml`** | Controle de versão; orquestrador CI completo; proprietário “tipo GitLab” |
| **Selenium** | Automação de **testes de interface (UI) / E2E** no browser | Orquestrador de pipeline; build tool; VCS |

Cartão anti-confusão (decorar):

| Precisa de… | Aponta para… |
|-------------|--------------|
| Versionar código | **Git** (+ host **GitHub** / GitLab) |
| Orquestrar pipeline CI/CD | **Jenkins** / **CircleCI** / **GitLab CI/CD** / GitHub Actions |
| Compilar/empacotar projeto Java, gerir deps | **Maven** (`pom.xml`) |
| Automatizar clique/navegação no browser | **Selenium** |

### 1.5 Git × GitHub (não são interdependentes)

- **Git** = ferramenta/protocolo de VCS distribuído (local + remoto qualquer).  
- **GitHub** = serviço que **hospeda** repositórios Git e adiciona PR, Issues, Actions etc.

Afirmação **errada** típica de distrator: “Git precisa ser integrado ao GitHub porque são interdependentes” / “Git sozinho faz CI/CD completo”.

Git **alimenta** o pipeline (o CI clona o repo); **não substitui** Jenkins/CircleCI/GitLab CI.

### 1.6 Jenkins (visão cobrável)

- Automação de build, teste, deploy via **jobs / pipelines**.  
- Ecossistema forte de **plugins**.  
- Escrito tipicamente em **Java** (não PHP).  
- Integra-se a Git para **obter o código**, mas **não** é “só histórico de testes”.  
- Configuração clássica: UI + **Jenkinsfile** (pipeline as code; frequentemente Groovy/declarative).

Flash FCC: Jenkins = **orquestrador CI/CD**, não “exclusivo de testes”, não Maven, não Selenium.

### 1.7 Maven (visão cobrável)

- Ferramenta de **build** e gestão de **dependências** no ecossistema **Java**.  
- Descritor principal: **`pom.xml`** (não `pom.java`).  
- Ciclo de vida mental: `validate` → `compile` → `test` → `package` → `verify` → `install` → `deploy` (não precisa decorar todos; saiba que há **fases** e que testes unitários entram no ciclo).  
- Roda **dentro** de um pipeline Jenkins/GitLab/CircleCI — Maven **não** substitui o orquestrador CI.

Flash: enunciado fala em **pom.xml**, **dependências Java**, **artefato JAR/WAR** → **Maven**.

### 1.8 CircleCI e GitLab CI/CD (visão cobrável)

**CircleCI**

- CI/CD com configuração em **YAML** (arquivo típico **`config.yml`** / `.circleci/config.yml`).  
- Suporta **várias linguagens** (não “só Go/Java/Ruby”).  
- Workflows, jobs, steps; paralelismo de testes é tema recorrente em comparações FCC.

**GitLab CI/CD**

- CI/CD **integrado ao GitLab**.  
- Arquivo típico: **`.gitlab-ci.yml`** no repositório.  
- Conceitos: **stages** (ex.: build → test → deploy), **jobs**, **runners** que executam os jobs.  
- O mesmo produto (GitLab) pode unir **VCS + CI/CD** — diferente de “Git puro”.

**GitHub Actions** (aparece em questões FCC mesmo quando o edital lista GitHub):

- Workflows em **`.github/workflows/*.yml`**.  
- Um repositório pode ter **vários** workflows (build, testar PRs, release…).  
- Não confundir Actions com “Git sozinho”.

Comparação mental (já caiu em FCC): CircleCI costuma concentrar vários workflows em `config.yml`; GitHub Actions usa **arquivo por workflow**.

### 1.9 Testes automatizados — pirâmide e encaixe no CI

Testes automatizados = scripts/ferramentas que **executam verificações sem intervenção manual repetida**, tipicamente a cada commit/pipeline.

Pirâmide (visão de prova — não precisa desenhar UML):

| Camada | Ideia | Exemplo mental |
|--------|-------|----------------|
| Unitários | Rápidos, isolados | JUnit / equivalente |
| Integração / API | Componentes juntos | Testcontainers, REST Assured… |
| UI / E2E | Browser ponta a ponta | **Selenium** |

No DevOps: o **pipeline CI** *dispara* os testes; a **ferramenta de teste** (JUnit, Selenium…) *executa* a verificação. Orquestrador ≠ runner de UI.

Em **DevSecOps**, além de testes funcionais: scans de dependências, análise estática, checagem de secrets — **automatizados cedo**.

### 1.10 Selenium — noção (nível edital)

**Selenium** = conjunto de ferramentas para **automatizar interações com navegadores** (abrir URL, preencher formulário, clicar, assertar texto/elemento) — foco em **testes de UI / ponta a ponta (E2E)**.

Ideias cobráveis (sem aprofundar API):

| Ideia | Frase |
|-------|-------|
| Domínio | Automação de **browser / interface web** |
| Uso típico | Regressão E2E; fluxos críticos de tela |
| No pipeline | Job de CI **invoca** suíte Selenium (após build) |
| Limite | Mais **lento/frágil** que unitário; não substitui toda a pirâmide |

Componentes (só nomes se o enunciado citar):

- **WebDriver** — API que controla o browser;  
- **IDE** — gravação/exportação auxiliar (visão);  
- **Grid** — execução distribuída/paralela em vários nós/browsers (visão).

O que Selenium **não** é (distratores FCC):

- orquestrador de **CI/CD** (Jenkins/CircleCI/GitLab CI);  
- ferramenta de **build** Java (Maven);  
- **controle de versão** (Git);  
- orquestrador de **contêineres** (Kubernetes);  
- “gestão de repositórios Git”.

### 1.11 DevSecOps no pipeline — checklist mental

Sem virar aula de AppSec, memorize o **encaixe**:

1. Código versionado no **Git** (host GitHub/GitLab).  
2. Pipeline (**Jenkins** / **CircleCI** / **GitLab CI** / Actions) clona e orquestra.  
3. **Maven** (Java) compila e roda testes de build.  
4. Testes automatizados (unitário → integração → **Selenium** se houver UI).  
5. Controles de segurança (**shift left**): SAST, SCA, secrets, políticas — *como noção*.  
6. Artefato promovido (Delivery/Deployment conforme política).

Frase de síntese:

> **DevOps automatiza a entrega; DevSecOps coloca segurança contínua no mesmo trilho; Selenium automatiza a UI; Maven builda Java; Jenkins/CircleCI/GitLab CI orquestram.**

### 1.12 Anti-confusão com a semana 3 (containers / mensageria)

| Tema | Onde encaixa | Não misturar com |
|------|--------------|------------------|
| Docker / K8s / Rancher | Empacotar/orquestrar runtime | Orquestrar *pipeline* CI (papel do Jenkins etc.) |
| RabbitMQ / eventos | Mensageria assíncrona | CI/CD ou Selenium |
| Keycloak / OAuth2 | Identidade (semana 2) | Build/Maven |
| Selenium | Teste UI | Pipeline as code |

Containers **podem** ser usados **pelos runners** do CI — mas Docker ≠ Jenkins ≠ Selenium.

### 1.13 Erros clássicos FCC (caderno mental)

1. Selenium = CI/CD / build / Git.  
2. Maven = controle de versão ou pipeline completo.  
3. Jenkins escrito em PHP / “só testes”.  
4. Git e GitHub interdependentes; Git sozinho = CI.  
5. CircleCI “só 3 linguagens” / “só gerencia repo Git”.  
6. DevSecOps = time isolado só no fim (sem shift left).  
7. DevOps = “deploy sem testes”.  
8. `pom.java` no lugar de **`pom.xml`**.  
9. Confundir Continuous Delivery com Continuous Deployment.  
10. Achar que GitLab CI e Maven são a mesma ferramenta.

### 1.14 Cartão-relâmpago (1 minuto antes da prova)

| Termo | 5 palavras |
|-------|------------|
| DevOps | Dev+Ops, automação, entrega contínua |
| DevSecOps | Segurança contínua + shift left |
| CI | Integrar, buildar, testar sempre |
| CD | Entregar/implantar de forma contínua |
| Git | VCS distribuído |
| GitHub | Host Git + colaboração (+ Actions) |
| GitLab CI/CD | Pipeline YAML no GitLab |
| Jenkins | Servidor automação CI/CD (Java) |
| CircleCI | CI/CD YAML (config.yml) |
| Maven | Build Java + pom.xml |
| Selenium | Automação de testes UI/browser |

### 1.15 Encaixe PDPJ / Judiciário (visão leve)

No eixo PDPJ-Br o edital também cita **Git**, **CI/CD**, **Docker**, **Kubernetes**, **Rancher**. Para **este** arquivo: trate CI/CD + Git como **prática DevOps** que sustenta entrega de microsserviços; segurança no pipeline dialoga com a cultura DevSecOps — sem confundir com Gateway/Eureka/Keycloak.

---

## 2. Questões de treino (1–15)

**1.** DevOps, em essência cobrável, relaciona-se a:

A) Substituir Git por MapStruct  
B) Colaboração Dev/Ops + automação do ciclo de entrega (build/teste/deploy)  
C) Apenas auditoria manual anual de segurança  
D) Eliminar todos os testes automatizados  
E) Sinônimo exclusivo de RabbitMQ  

**2.** DevSecOps difere do DevOps “puro” principalmente por:

A) Remover CI do ciclo  
B) Integrar segurança de forma contínua no ciclo (shift left), e não só no final  
C) Proibir o uso de Maven  
D) Exigir que Selenium seja o único teste  
E) Substituir Git por Eureka  

**3.** Continuous Integration (CI) enfatiza:

A) Deploy automático obrigatório em produção a cada commit, sem testes  
B) Integração frequente do código com build e testes automatizados  
C) Apenas documentação UML  
D) Gestão exclusiva de DNS  
E) Emissão de tokens OAuth  

**4.** No mapa do edital, a ferramenta tipicamente associada a **automação de testes de interface web (browser)** é:

A) Maven  
B) Git  
C) Selenium  
D) Eureka  
E) Flyway  

**5.** O arquivo descritor clássico de um projeto **Maven** é:

A) `pom.java`  
B) `pom.xml`  
C) `.gitlab-ci.yml` apenas  
D) `Jenkinsfile.php`  
E) `selenium.git`  

**6.** Assinale a associação **correta**:

A) Jenkins → VCS distribuído; Git → orquestrador CI exclusivo de PHP  
B) Git → controle de versão; Jenkins/CircleCI/GitLab CI → orquestração CI/CD; Maven → build Java; Selenium → UI  
C) Selenium → pipelines YAML; Maven → hospedagem GitHub  
D) CircleCI → ORM; GitLab CI → Envers  
E) GitHub → substituto obrigatório do Git local  

**7.** Afirmação **incorreta** sobre Git e GitHub:

A) Git é VCS distribuído  
B) GitHub hospeda repositórios Git e oferece colaboração  
C) Git e GitHub são interdependentes: Git não funciona sem GitHub  
D) GitHub Actions pode definir workflows CI/CD no repositório  
E) Pipelines costumam clonar o código a partir de um remoto Git  

**8.** Jenkins é melhor descrito como:

A) Ferramenta escrita em PHP exclusiva para armazenar histórico de testes  
B) Servidor de automação / CI/CD (tipicamente Java) que orquestra builds, testes e deploys  
C) Mecanismo de Service Discovery  
D) Banco H2 embutido  
E) Protocolo OAuth 2.0  

**9.** CircleCI, no discurso de prova, caracteriza-se por:

A) Ser orquestrador de pods Kubernetes exclusivamente  
B) Plataforma CI/CD com workflows tipicamente em YAML (`config.yml`), suportando diversas linguagens  
C) Substituir Selenium em todos os testes unitários  
D) Ser incompatível com Python/Node  
E) Gerir apenas repositórios Git sem executar jobs  

**10.** GitLab CI/CD tipicamente usa, no repositório:

A) Apenas `pom.xml` como definição de stages  
B) Arquivo YAML de pipeline (ex.: `.gitlab-ci.yml`) com jobs/stages executados por runners  
C) Somente WebDriver sem YAML  
D) `application-eureka.properties` como CI  
E) Scripts Flyway como orquestrador de deploy  

**11.** No pipeline, o papel mais adequado do **Maven** é:

A) Orquestrar todos os runners do GitLab no lugar do YAML  
B) Compilar/empacotar o projeto Java e gerenciar dependências conforme `pom.xml`  
C) Autenticar usuários no Keycloak  
D) Versionar branches no lugar do Git  
E) Renderizar páginas como browser engine  

**12.** Selenium **não** deve ser descrito como:

A) Automação de interações com navegador para testes UI/E2E  
B) Suíte que pode ser invocada por um job de CI  
C) Orquestrador nativo de pipelines CI/CD e gestão de repositórios Git  
D) Ferramenta da camada de testes de interface na pirâmide  
E) Apoio a regressão de fluxos web críticos  

**13.** *Shift left* em DevSecOps significa, em regra:

A) Adiar toda segurança para pós-produção  
B) Antecipar práticas/checagens de segurança (e qualidade) para fases iniciais, com automação no ciclo  
C) Remover SAST do pipeline  
D) Proibir Continuous Integration  
E) Trocar Git por FTP obrigatório  

**14.** Continuous Delivery × Continuous Deployment — assinale a leitura mais adequada:

A) São sempre sinônimos absolutos e proíbem testes  
B) Delivery: artefato pronto para produção (pode haver aprovação); Deployment: promoção automática frequente após pipeline verde  
C) Delivery elimina CI; Deployment elimina Git  
D) Ambos equivalem apenas a Selenium Grid  
E) Deployment é exclusivo do Maven `pom.xml`  

**15.** Qual opção melhor resume este subtópico?

A) DevOps = automação Dev/Ops + CI/CD; DevSecOps = segurança contínua/shift left; Git/GitHub versionam/hospedam; Jenkins/CircleCI/GitLab CI orquestram; Maven builda Java; Selenium testa UI  
B) Selenium = Jenkins = Maven = Git (mesma função)  
C) DevSecOps proíbe pipelines; Git exige GitHub  
D) CircleCI só gerencia DNS; Maven é VCS  
E) Shift left = auditar só no fim com time isolado  

---

### Gabarito comentado (treino 1–15)

1. **B** — Colaboração + automação do ciclo; não é mensageria/mapper.  
2. **B** — Segurança contínua / shift left.  
3. **B** — Integrar + build/testes frequentes.  
4. **C** — Selenium = UI/browser.  
5. **B** — `pom.xml`.  
6. **B** — Mapa completo do edital.  
7. **C** — Git funciona sem GitHub (única incorreta).  
8. **B** — Orquestrador CI/CD (Java), não “só PHP/testes”.  
9. **B** — CI/CD YAML multilinguagem.  
10. **B** — `.gitlab-ci.yml` + runners.  
11. **B** — Build/deps Java.  
12. **C** — Selenium **não** é orquestrador CI/VCS.  
13. **B** — Antecipar segurança com automação.  
14. **B** — Delivery (pronto/aprovável) × Deployment (auto).  
15. **A** — Síntese do arquivo 07.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** de provas FCC (preferência TRT Analista TI), com banca/órgão/ano citados. Textos conferidos em Quizinho, QConcursos, Estude Grátis e/ou prova oficial.

**16.** (FCC – TRT-20/2024 – Analista Judiciário – Tecnologia da Informação) Uma equipe deseja adotar abordagem DevOps para automatizar o processo de integração e entrega contínua (CI/CD). Para isso, é necessário integrar diversas ferramentas para otimizar a automação dos testes e a entrega do produto. Dentre essas ferramentas, encontra-se o

A) Selenium, que é utilizado para automação de builds, testes de integração e execução de pipelines de CI/CD, que integra-se ao CircleCI, que faz a gestão de repositórios Git. O CircleCI é compatível apenas com as linguagens Go, Java e Ruby, apresentando incompatibilidade com Python, Ruby, Scala, Node.js, PHP e Haskell.  
B) Git. Esta ferramenta precisa ser integrada ao GitHub, uma vez que são interdependentes. Usadas em conjunto, conseguem realizar o controle de versão e proporcionar a integração contínua.  
C) GitHub Actions, que permite a criação de workflows de CI/CD diretamente no GitHub. Os workflows são definidos no diretório `.github/workflows` de um repositório, que pode ter vários workflows, cada um dos quais podendo executar um conjunto diferente de tarefas, como criar e testar pull requests.  
D) Jenkins, que é uma ferramenta escrita em PHP, exclusiva para automação de testes, que integra-se ao Git para armazenar o histórico dos testes automatizados executados.  
E) Maven, que é uma ferramenta proprietária para automação de builds, que também possui capacidades nativas de controle de versões e pipelines de CI/CD, similar ao GitLab CI/CD. Um projeto Maven fica atrelado a um arquivo principal: o pom.java.  

**17.** (FCC – TRT-6/2025 – Analista Judiciário – Tecnologia da Informação) A equipe de desenvolvimento do Tribunal Regional do Trabalho está trabalhando em um novo produto de software que será implementado em um ambiente de nuvem. A organização utiliza práticas de DevOps e DevSecOps para acelerar o desenvolvimento e, ao mesmo tempo, melhorar a segurança desde as primeiras fases do ciclo de vida do software. Um dos principais desafios para a equipe é integrar testes de segurança contínuos sem impactar a velocidade de entrega, o que pode ser conseguido

A) garantindo que a equipe de desenvolvimento seja treinada em práticas de DevSecOps, aplicando na fase de produção, testes IAST (Interactive Agile Security Testing), que combinam testes SAST e DAST, usando software para monitorar a performance da aplicação.  
B) concentrando-se principalmente em automação de testes de performance e de funcionalidades, com foco em segurança após a fase de homologação, aplicando testes RAST (Rapid Agile Security Testing).  
C) criando uma equipe de segurança separada que realiza auditorias manuais de segurança em intervalos regulares, após cada fase do desenvolvimento.  
D) implementando verificações de segurança apenas na fase final do desenvolvimento, para garantir que todos os testes de segurança sejam realizados apenas uma vez.  
E) adotando a abordagem Shift Left Security e integrando ferramentas automatizadas de segurança diretamente nos pipelines de CI/CD, permitindo que vulnerabilidades sejam detectadas nas primeiras fases do desenvolvimento.  

**18.** (FCC – TRT-1/2025 – Analista Judiciário – Tecnologia da Informação) O time de TI de um Tribunal Regional do Trabalho que está desenvolvendo uma aplicação web configurou o Selenium WebDriver para realizar testes de funcionalidades no navegador Chrome. Durante os testes, identificaram que o script falha ao tentar interagir com o botão de "Enviar", apresentando o erro NoSuchElementException. O desenvolvedor analisou o código e percebeu que o botão é carregado dinamicamente após a conclusão de uma animação. Nesse contexto, a abordagem mais adequada que o time deve adotar para resolver o problema e garantir que o botão seja corretamente identificado e clicado nos testes é

A) configurar o WebDriver para ignorar exceções e continuar a execução.  
B) atualizar o Selenium WebDriver e o Selenium IDE para as versões mais recentes.  
C) implementar Implicit Wait no script para executar após o carregamento do botão.  
D) aumentar o tempo de execução do script para garantir que o botão seja carregado.  
E) utilizar Explicit Wait para aguardar a presença do botão na página antes de interagir.  

**19.** (FCC – TRT-15/2024 – Analista Judiciário – Tecnologia da Informação) Maria é uma analista de TI em um Tribunal Regional do Trabalho. Ela está implementando um novo sistema para gerenciamento de processos trabalhistas e quer realizar testes no código do sistema para garantir que todas as funcionalidades atendam aos requisitos corretamente antes de lançar o sistema em produção e decidiu utilizar o JUnit que

A) é um framework utilizado para testes de desempenho e carga em aplicativos Java.  
B) é um framework de teste de unidade para Java que facilita a criação e execução de testes automatizados.  
C) permite a geração automática de relatórios de cobertura de código sem precisar de ferramentas adicionais.  
D) é especificamente projetado para testar interfaces de usuário em aplicações Java Swing.  
E) é uma ferramenta que permite a execução paralela de testes de integração distribuídos.  

**20.** (FCC – TRT-19/2022 – Analista Judiciário – Tecnologia da Informação) O teste automatizado de software que simula situações que normalmente não aconteceriam (como toques acidentais em áreas não interativas), para analisar se nenhum ponto da interface apresenta problemas, é conhecido como teste de

A) Unit.  
B) Stress.  
C) GUI.  
D) Block.  
E) API.  

**21.** (FCC – TRT-15/2023 – Analista Judiciário – Tecnologia da Informação) Suponha que um Analista de um tribunal está realizando testes de software e estudou os seguintes textos:  
I. Verifica se diferentes módulos ou serviços usados pelo seu aplicativo funcionam bem juntos. Envolve testar diferentes módulos de um aplicativo de software como um grupo. Um aplicativo de software é composto de diferentes submódulos que trabalham juntos para diferentes funcionalidades. A execução desses tipos de testes tem um custo maior, uma vez que exigem que várias partes do aplicativo estejam ativas e em execução.  
II. Verifica os recursos, funcionalidades e usabilidade de negócios críticos. Garante que os recursos e funcionalidades do software estejam se comportando como esperado sem qualquer falha. Valida principalmente toda a aplicação em relação às especificações mencionadas no documento Software Requirement Specification (SRS), (especificação de requisitos de software).  
Os tipos de teste relatados nos textos acima são, correta e respectivamente,

A) Unitário e Funcional.  
B) Funcional e Integração.  
C) Unitário e Integração.  
D) Integração e Funcional.  
E) Funcional e Unitário.  

**22.** (FCC – TRT-12/2023 – Analista Judiciário – Tecnologia da Informação) Na abordagem DevOps, a integração e a entrega contínuas (CI/CD) juntas são comumente tratadas como um processo relacionado, conhecido como

A) hub register.  
B) pipeline.  
C) git hub.  
D) container register.  
E) end-to-end.  

**23.** (FCC – TRT-17/2022 – Analista Judiciário – Tecnologia da Informação) Para construir um pipeline como código no Jenkins, um analista utilizou um arquivo de texto simples conhecido como

A) Pipejenkins que especifica o encadeamento dos estágios do processo DevSecOps.  
B) Scripted que especifica o enlace crítico entre as etapas primárias DevOps.  
C) JenkinsFile que normalmente contém um código Groovy Domain Specific Language.  
D) JenkinsGroovy que especifica o enlace crítico usando código DomainPline.  
E) Declarative que especifica o enlace crítico usando código GroovyPline.  

**24.** (FCC – TRT-22/2022 – Analista Judiciário – Tecnologia da Informação) Considere o recorte de um arquivo `.gitlab-ci.yml` que traz algumas opções de configuração de um pipeline GitLab CI/CD, nas seções que são separadas pelas *global keywords*.

```
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

**25.** (FCC – TRT-15/2024 – Analista Judiciário – Tecnologia da Informação) Uma equipe de desenvolvimento utiliza o GitLab CI/CD para automação de builds e deploys. Para otimizar o processo de integração contínua (CI), foi configurado um webhook para disparar automaticamente um pipeline de build assim que um commit é enviado ao branch main. No entanto, durante uma atualização do ambiente de produção, percebeu-se que o webhook falhou e o pipeline não foi executado, causando atraso no deploy. Após uma análise técnica, verificou-se que o problema ocorreu devido a uma falha no endpoint do webhook, que não conseguiu validar a mensagem enviada. A solução técnica que deve ser implementada para garantir que o webhook não falhe novamente devido a erros de validação é

A) configurar uma API reversa para verificar e redirecionar as mensagens do webhook para um endpoint alternativo.  
B) implementar SSL offloading no servidor para garantir que as mensagens webhook trafeguem com criptografia SSL.  
C) configurar um balanceador de carga para distribuir as requisições do webhook entre múltiplos endpoints redundantes de validação de mensagens.  
D) criar um proxy reverso com logs detalhados para monitorar a comunicação e diagnosticar falhas no endpoint do webhook.  
E) habilitar o recurso de verificação de assinatura (usando token) para checar a integridade das mensagens enviadas pelo webhook.  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Fonte / nota |
|---|------|----------------|
| 16 | **C** | FCC TRT-20/2024 — GitHub Actions; Selenium/Maven/Jenkins como distratores (Quizinho) |
| 17 | **E** | FCC TRT-6/2025 — Shift Left Security no CI/CD (Quizinho / QConcursos) |
| 18 | **E** | FCC TRT-1/2025 — Selenium Explicit Wait (Quizinho) |
| 19 | **B** | FCC TRT-15/2024 — JUnit = testes de unidade Java (Quizinho) |
| 20 | **C** | FCC TRT-19/2022 — teste de GUI (Quizinho) |
| 21 | **D** | FCC TRT-15/2023 — Integração e Funcional (Quizinho) |
| 22 | **B** | FCC TRT-12/2023 — CI/CD = pipeline (Quizinho) |
| 23 | **C** | FCC TRT-17/2022 — Jenkinsfile / Groovy (Quizinho) |
| 24 | **D** | FCC TRT-22/2022 — `default` e `include` no `.gitlab-ci.yml` (Quizinho) |
| 25 | **E** | FCC TRT-15/2024 — assinatura de webhook GitLab (Quizinho) |

Letras do gabarito neste bloco: C, E, E, B, C, D, B, C, D, E (A–E: A ausente neste bloco — ver gap).

---

### Verificação interna (consistência)

| Checagem | Status |
|----------|--------|
| Edital: Jenkins; Maven; CircleCI; Git; GitHub; GitLab CI/CD; Selenium; testes automatizados | Coberto |
| DevOps × DevSecOps × shift left | Seções 1.1–1.2, 1.11 + Q2/Q13/Q17 |
| CI × CD (Delivery/Deployment) | Seção 1.3 + Q3/Q14/Q22 |
| Mapa anti-confusão ferramentas | Seção 1.4 + Q6/Q12/Q16 |
| Selenium noção (UI/E2E; não CI) | Seção 1.10 + Q4/Q12/Q18 |
| Grafia *Sellenium* do edital | Nota no cabeçalho |
| Treino 1–15 com gabarito comentado | 15/15 intacto |
| Concurso 16–25 = questões **reais** + tabela Fonte/nota | 10/10 |
| Distratores clássicos (papéis trocados; pom.java; Git↔GitHub; Jenkins PHP; Selenium=CI) | Presentes em Q16 |
| Gabaritos oficiais | Conferidos |
| Fora de escopo: Docker/K8s/Rancher profundos; Rabbit; OAuth | Declarado + 1.12 |

---

*Arquivo 07 · DevOps / DevSecOps + Selenium (noção) · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 3 — entrega contínua e testes.*

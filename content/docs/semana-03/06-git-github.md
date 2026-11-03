+++
title = '06 — Git / GitHub'
date = '2026-10-17'
draft = false
weight = 6
tags = ['semana-03', 'git', 'github']
categories = ['estudo']
+++
# Git / GitHub · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — Git / GitHub (Semana 3 · DevOps / PDPJ)  
**Escopo deste arquivo:** **Git** como sistema de controle de versão distribuído; objetos e áreas de trabalho; commits, branches, merge/rebase (visão cobrável); remotes (`clone` / `fetch` / `pull` / `push`); **GitHub** como plataforma de hospedagem e colaboração (PR, fork, Issues); contraste com **GitLab CI**, Jenkins, Maven, CircleCI e com containers (Docker/K8s).  
**Fora de escopo aqui:** pipelines CI/CD em profundidade (Jenkins/GitLab CI/CircleCI — outros arquivos da semana 3); Docker/Kubernetes/Rancher; RabbitMQ/webhooks; Selenium; detalhes de servidor Git self-hosted além do necessário para diferenciar produto × protocolo.

---

## 1. Material

### 1.1 O que é Git (leitura de prova)

**Git** é um **sistema de controle de versão distribuído** (*Distributed Version Control System* — DVCS): registra o histórico de mudanças de arquivos (código, scripts, docs versionáveis) de forma que **cada clone** carrega, em regra, o histórico local completo.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | **Versionar** o código / artefatos textuais ao longo do tempo |
| Modelo | **Distribuído**: cada desenvolvedor tem repositório local com histórico |
| Unidade | **Commit** = snapshot imutável (com metadados) de um estado do projeto |

Flash: enunciado fala em **repositório**, **commit**, **branch**, **merge**, **clone**, **push/pull**, **histórico local** → **Git**.

Git **não** é: Jenkins, Maven, CircleCI, Docker, Eureka, Keycloak, Flyway.

### 1.2 Git × GitHub (não misturar)

Tabela **ouro** anti-confusão (FCC adora inverter):

| | **Git** | **GitHub** |
|---|--------|------------|
| Natureza | Ferramenta / protocolo de **controle de versão** | **Plataforma** web de hospedagem + colaboração sobre repositórios Git |
| Onde roda | Local (e em qualquer host Git) | Nuvem GitHub (produto) |
| Artefatos típicos | commits, branches, tags, remotes | Pull Request, Issues, Actions, Releases, fork |
| Pergunta | Como **versionar** e sincronizar histórico? | Onde **hospedar**, revisar e colaborar em cima do Git? |

Frase de prova: **Git versiona; GitHub hospeda e colabora em cima do Git.**

Corolários cobráveis:

- Dá para usar **Git sem GitHub** (GitLab, Bitbucket, Azure DevOps, servidor bare próprio, só local).
- **GitHub sem Git** não faz sentido como “só site”: o núcleo do projeto versionado continua sendo um **repositório Git**.
- No edital, **Git** e **GitHub** aparecem juntos em DevOps/PDPJ; **GitLab CI/CD** é outra peça (pipeline), não sinônimo de GitHub.

### 1.3 Por que versionar? (problema cobrável)

Sem controle de versão:

1. “Quem alterou o que e quando?” fica opaco.  
2. Rollback vira caça a zip/`copia-final-v3`.  
3. Trabalho em paralelo gera sobrescrita.  
4. Auditoria e revisão de mudança somem.  
5. CI/CD não tem **ponto estável** (commit/tag) para buildar.

Git responde: **histórico rastreável + ramificação + sincronização entre máquinas**.

No eixo PDPJ / microsserviços Spring: o código da app (e scripts Flyway, Dockerfiles, manifests) vive no Git; o deploy/CI **consome** um commit/tag — Git não “é” o pipeline, mas é a **fonte versionada** do artefato.

### 1.4 Áreas de trabalho (modelo mental)

Fluxo clássico cobrável (não precisa decorar todos os flags):

```
Working Directory  →  Staging Area (index)  →  Repositório local (.git)
   (arquivos)            (git add)                 (git commit)
                              │
                              ▼
                    Remote (origin)  ←→  git push / git fetch / git pull
                    (ex.: GitHub)
```

| Área | Ideia |
|------|--------|
| **Working tree** | Arquivos como estão no disco agora |
| **Staging / index** | O que entrará no **próximo** commit (`git add`) |
| **Repositório local** | Commits já gravados no histórico local |
| **Remote** | Cópia/referência em outro host (ex. `origin` no GitHub) |

Flash: **add** prepara; **commit** grava local; **push** envia ao remote; **fetch** traz referências sem integrar; **pull** ≈ fetch + integração (merge/rebase, conforme config).

### 1.5 Commit, branch, tag

| Conceito | Leitura de prova |
|----------|------------------|
| **Commit** | Snapshot + mensagem + autor + ponteiros para ancestrais; identifica-se por **hash** |
| **Branch** | Ponteiro móvel para um commit; permite desenvolver em paralelo |
| **HEAD** | Onde você “está” (branch/commit atual) |
| **Tag** | Marca (muitas vezes) um ponto de **release** / versão estável |
| **main / master** | Branch principal convencional (nome varia; conceito = linha principal) |

Modelo mental de branch:

```
main:    C1 ── C2 ── C5
               \
feature:        C3 ── C4
```

Desenvolver em **feature branch** e integrar na principal via **merge** (ou PR no GitHub) é o fluxo mais cobrado em enunciados de equipe.

### 1.6 Merge × conflito × rebase (visão)

| Operação | Ideia cobrável |
|----------|----------------|
| **Merge** | Une históricos; pode gerar commit de merge |
| **Conflito** | Mesmo trecho alterado em linhas divergentes; exige resolução humana/ferramenta |
| **Rebase** | Reaplica commits “em cima” de outra base — histórico mais linear; **não** é o mesmo que merge |
| **Fast-forward** | Só avança o ponteiro quando não há divergência |

FCC raramente exige digitar flags; cobra **conceito**: conflito = mudanças incompatíveis no mesmo ponto; merge integra; rebase reescreve a sequência relativa à nova base (cuidado em branches já compartilhados).

### 1.7 Remotes e sincronização

| Comando (conceito) | Efeito |
|--------------------|--------|
| `git clone` | Copia repositório remoto → local (já com remote tipicamente `origin`) |
| `git fetch` | Atualiza referências remotas **sem** misturar no working tree |
| `git pull` | Traz e **integra** mudanças do remote na branch atual |
| `git push` | Envia commits locais para o remote |
| `git remote -v` | Lista remotes (ideia: ver onde está o `origin`) |

Flash anti-erro:

- **Push não é commit**: commit é local; push publica no remote.  
- **Pull não é clone**: clone cria o local; pull atualiza o que já existe.  
- **Fetch ≠ pull**: fetch só atualiza o conhecimento do remoto.

### 1.8 GitHub — colaboração

**GitHub** acrescenta camada social/operacional sobre Git:

| Recurso | Papel cobrável |
|---------|----------------|
| **Repository** | Hospeda o `.git` (e interface web) |
| **Fork** | Cópia do repositório sob outra conta/organização (contribuição externa típica) |
| **Pull Request (PR) / Merge Request** | Proposta de integrar branch (revisão, CI, discussão) — no GitLab o nome clássico é **Merge Request** |
| **Issues** | Rastreio de bugs/tarefas (não é commit) |
| **Releases / tags** | Empacota versões a partir de tags |
| **Actions** (noção) | Automação/CI no ecossistema GitHub — **não** confundir com GitLab CI nem com “o Git em si” |
| **Branch protection** | Exige PR, review, status checks antes de merge na main |

Fluxo cobrável de equipe:

```
clone/fork → branch feature → commits locais → push → Pull Request → review → merge na main
```

### 1.9 Git no edital: DevOps e PDPJ-Br

O edital cita **Git** e **GitHub** em **DevOps/DevSecOps** e de novo **Git** na **Arquitetura PDPJ-Br** (ao lado de Docker, Kubernetes, Rancher, CI/CD).

Leitura cobrável do “kit entrega”:

```
Código + configs versionados (Git / GitHub)
        │
        ▼
  CI/CD (Jenkins · GitLab CI · CircleCI · Maven no build)
        │
        ▼
  Imagem/container (Docker) → orquestração (Kubernetes / Rancher)
```

| Peça | Papel |
|------|--------|
| **Git** | Controle de versão |
| **GitHub** | Host + colaboração (PRs) |
| **GitLab CI** | Pipeline CI/CD (produto GitLab) — **≠** “Git” e **≠** “GitHub” |
| **Jenkins** | Servidor/automação CI/CD clássico |
| **Maven** | Build/dependências Java (não é VCS) |
| **CircleCI** | Serviço CI/CD |
| **Docker / K8s / Rancher** | Empacotar e orquestrar — consomem artefatos construídos a partir do Git |

Frase: **Git guarda a fonte; CI/CD constrói/testa/publica; containers rodam o resultado.**

### 1.10 `.gitignore`, segredos e boa higiene (DevSecOps leve)

| Prática | Por quê na prova |
|---------|------------------|
| **`.gitignore`** | Evita versionar `target/`, `node_modules/`, `.env`, binários inúteis |
| **Não commit de segredo** | Senhas, tokens, chaves privadas no Git = incidente; rotação + remoção do histórico se vazou |
| **Mensagens de commit úteis** | Rastreabilidade (auditoria / blame) |
| **PR + review** | Controle de mudança (encaixa DevSecOps / qualidade) |
| **Tag de release** | Ponto reproduzível para deploy |

Git **versiona o que você adiciona**; não é cofre de segredos. Vault/secret manager ≠ papel do Git.

### 1.11 Armadilhas FCC (anti-gabarito invertido)

1. **Git = GitHub** → errado (ferramenta × plataforma).  
2. **Git = GitLab CI** → errado (VCS × pipeline).  
3. **Maven versiona código** → errado (build).  
4. **Push grava o commit** → errado (commit é local; push sincroniza).  
5. **Issue = commit** → errado (Issue rastreia; commit versiona conteúdo).  
6. **Fork = branch** → relacionados, mas fork é **cópia de repositório**; branch é linha dentro de um repo.  
7. **Pull Request só existe no Git** CLI puro → PR é recurso de **plataforma** (GitHub/similares).  
8. **Docker substitui Git** → errado (empacota runtime; não versiona o histórico do código no lugar do Git).

### 1.12 Cartão mental (síntese)

| Flash | Resposta |
|-------|----------|
| Versionar código / histórico / branch / commit | **Git** |
| Hospedar + PR + Issues + fork | **GitHub** (plataforma) |
| Pipeline no GitLab | **GitLab CI/CD** |
| Build Java | **Maven** |
| Automação CI clássica | **Jenkins** / CircleCI |
| Empacotar app | **Docker** |
| Orquestrar | **Kubernetes** / Rancher |

---

## 2. Questões de treino (1–15)

**1.** Git caracteriza-se, em regra, como:

A) Servidor de Service Discovery de microsserviços  
B) Sistema de **controle de versão distribuído**  
C) Ferramenta exclusiva de orquestração de containers  
D) Protocolo OAuth2 / IdP  
E) Mapper Entity ↔ DTO em tempo de compilação  

**2.** A principal diferença cobrável entre Git e GitHub é que:

A) São nomes comerciais idênticos da mesma CLI  
B) Git é o VCS; GitHub é plataforma de hospedagem/colaboração sobre repositórios Git  
C) GitHub versiona localmente; Git só existe na nuvem  
D) Git substitui Maven e Jenkins  
E) GitHub elimina a necessidade de commits  

**3.** A sequência conceitual mais alinhada ao fluxo local clássico é:

A) push → commit → add  
B) add (staging) → commit (local) → push (remote)  
C) pull → docker build → commit no Eureka  
D) merge → clone → apagar o remote  
E) tag → Issue → fork obrigatório antes de qualquer add  

**4.** Assinale a associação **correta**:

A) Commit = proposta de PR na web; Issue = snapshot imutável do código  
B) Branch = linha de desenvolvimento; commit = snapshot versionado  
C) Fork = comando interno do Maven  
D) Fetch = envio obrigatório ao remote  
E) Tag = sinônimo estrito de Issue  

**5.** Em enunciado que fala em clonar um repositório, criar branch, enviar commits e abrir Pull Request para revisão, o encaixe típico é:

A) Apenas Flyway + Envers  
B) Git (versionamento) + GitHub (colaboração/PR)  
C) Apenas Eureka Dashboard  
D) Apenas RabbitMQ  
E) Apenas Keycloak realm  

**6.** Sobre `git fetch` e `git pull`, é correto afirmar que:

A) São sempre sinônimos absolutos em qualquer configuração  
B) Fetch atualiza referências do remote sem, por si, integrar no working tree; pull traz e integra  
C) Pull só existe no GitHub Actions  
D) Fetch envia commits locais ao origin  
E) Ambos substituem o `git commit`  

**7.** Qual afirmação é **incorreta**?

A) Git permite histórico e ramificação local  
B) É possível usar Git com hosts que não sejam GitHub  
C) GitHub Pull Request é recurso de plataforma sobre o fluxo Git  
D) Maven é o sistema de controle de versão exigido no lugar do Git  
E) Commit identifica-se tipicamente por hash  

**8.** No kit mental DevOps do edital (fonte → build → runtime):

A) Git versiona a fonte; CI/CD (Jenkins/GitLab CI/CircleCI) automatiza build/teste; Docker/K8s empacotam/orquestram  
B) Docker versiona commits; Git orquestra pods; Maven hospeda PRs  
C) Eureka aplica merge; Zuul faz push  
D) Keycloak substitui o remote origin  
E) Flyway é o único remote permitido  

**9.** “Plataforma que hospeda repositórios Git e oferece Pull Requests e Issues.” Descreve com mais precisão:

A) Git (somente a CLI)  
B) GitHub  
C) Hibernate Envers  
D) MapStruct  
E) H2  

**10.** Conflito de merge ocorre tipicamente quando:

A) O repositório não tem remote  
B) Alterações divergentes tocam o mesmo trecho e o Git não consegue integrar automaticamente  
C) Se usa PostgreSQL em vez de H2  
D) O Swagger UI está aberto  
E) O único remote se chama `origin`  

**11.** Git **não** se destina, por si só, a:

A) Registrar histórico de mudanças de código  
B) Ramificar desenvolvimento em paralelo  
C) Orquestrar cluster Kubernetes no lugar do kube-scheduler  
D) Sincronizar commits com um remote  
E) Marcar releases via tags  

**12.** Diferenciar fork e branch é relevante porque:

A) Não há diferença conceitual cobrável  
B) Branch é linha dentro de um repositório; fork é cópia do repositório (em geral sob outro namespace)  
C) Fork é exclusivo do protocolo OAuth2  
D) Branch só existe no Jenkins  
E) Fork apaga o histórico Git obrigatoriamente  

**13.** A frase mais alinhada ao papel do Git na stack PDPJ/DevOps é:

A) “É o API Gateway da borda”  
B) “Versiona o código-fonte e artefatos textuais que alimentam o ciclo de entrega”  
C) “Substitui o PostgreSQL em produção”  
D) “Documenta OpenAPI no navegador”  
E) “Registra heartbeat das réplicas”  

**14.** Qual linha resume melhor o cartão mental deste subtópico?

A) Git = docs OpenAPI; GitHub = ORM; GitLab CI = Entity  
B) Git = VCS; GitHub = host/colaboração; GitLab CI/Jenkins/CircleCI = pipelines; Maven = build  
C) Git = Eureka + Zuul  
D) GitHub elimina commits e branches  
E) Git = apenas backup full de banco  

**15.** Afirmar que “fazer push é a mesma operação que criar o commit local” é:

A) Correto em toda prova FCC  
B) Incorreto: commit grava no repositório local; push envia ao remote  
C) Correto somente com tags anotadas  
D) Correto porque ambos são Spring Cloud Netflix  
E) Correto se o remote for Docker Hub  

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** de provas FCC (preferência TRT Analista TI), com banca/órgão/ano citados. Textos conferidos em Quizinho, QConcursos, Estude Grátis e/ou prova oficial.

**16.** (FCC – TRT-5/2022 – Analista Judiciário – Tecnologia da Informação) Considere que um analista adicionou o repositório Git remoto por meio do comando abaixo.

```
git remote add analista https://github.com/analista/projeto.git
```

Para remover essa conexão do repositório remoto utiliza-se a instrução

A) `git remote remove analista`  
B) `git remote delete origin`  
C) `git remote remove projeto.git`  
D) `git remote prune analista`  
E) `git remote delete https://github.com/analista/projeto.git`  

**17.** (FCC – TRT-5/2022 – Técnico Judiciário – Tecnologia da Informação) Utilizando a ferramenta de versionamento Git, para mostrar, em linha de comando, a relação de arquivos com alterações pendentes para serem adicionados a um commit, um técnico deve utilizar o comando

A) `git staged`  
B) `git status`  
C) `git remote show`  
D) `git show files`  
E) `git fetch files`  

**18.** (FCC – TRT-17/2022 – Analista Judiciário – Tecnologia da Informação) No prompt de comandos do Windows, a partir da pasta do diretório local ao qual o diretório remoto do Git está vinculado, um analista digitou os comandos abaixo.

```
git add tribunal.txt
git status
git commit -m "Cria o arquivo tribunal.txt"
```

Considerando a existência do arquivo `tribunal.txt` no diretório e todas as condições ideais, para efetivar as alterações no repositório remoto, de forma que o arquivo possa ser visualizado por meio do link do repositório no navegador web, o analista deve digitar o comando

A) `git persist`  
B) `git merge`  
C) `git push`  
D) `git branch`  
E) `git pull`  

**19.** (FCC – TRT-12/2023 – Analista Judiciário – Tecnologia da Informação) Em um ambiente de versionamento Git, um Analista possui duas branches no repositório: a principal chamada `main` e uma secundária chamada `novaFunc`. Após certificar-se de estar na `main` por meio do comando `git checkout main`, para combinar as alterações feitas na branch `novaFunc` na `main` ele deve utilizar o comando

A) `git pull novaFunc merge main`  
B) `git push novaFunc merge main`  
C) `git join novaFunc`  
D) `git merge novaFunc`  
E) `git git add novaFunc to main`  

**20.** (FCC – TRT-15/2024 – Analista Judiciário – Tecnologia da Informação) A equipe de desenvolvimento de um Tribunal adota o modelo GitFlow para gerenciar o versionamento de código em um repositório GitLab. Durante o desenvolvimento de uma nova funcionalidade, a equipe percebeu que algumas mudanças feitas no branch feature impactam diretamente no branch develop e precisam ser incorporadas imediatamente para integração contínua (CI). Contudo, um dos desenvolvedores cometeu um erro ao realizar um merge, levando à sobrescrita de código existente no develop. O procedimento que a equipe deve adotar para resolver o problema de sobrescrita e prevenir a repetição desse erro no futuro é

A) realizar um `git reset --hard` no branch develop para voltar ao último commit correto e forçar os desenvolvedores a refazerem o merge.  
B) configurar políticas de revisão opcionais (Operation Request Approvals) no Github para que todas as operações sejam aprovadas por, no mínimo, um revisor.  
C) fazer um rollback do branch develop usando `git stepback`, garantindo que apenas os commits problemáticos sejam desfeitos sem afetar o histórico.  
D) configurar um Webhook no GitLab para rejeitar automaticamente merges que causam conflitos ou sobrescritas de código no develop.  
E) utilizar a estratégia rebase no branch feature para reorganizar os commits e alinhar o histórico com o branch develop antes de realizar um novo merge.  

**21.** (FCC – TRT-15/2023 – Analista Judiciário – Tecnologia da Informação) Considere os dados abaixo.  
Personal Access Token (PAT) = `ghp_RCnTgfJovGy6b2WX1WaVyrcfGoHMbdl20Myk`  
Repositório = `developer.git`  
Usuário: `trt15`  

Como administrador no prompt de um computador com o Windows, com o Git devidamente instalado e em condições ideais, para criar uma cópia local do repositório na máquina, permitindo a colaboração no projeto, um Analista deve usar o comando

A) `git get https://ghp_RCnTgfJovGy6b2WX1WaVyrcfGoHMbdl20Myk@github.com/trt15/developer.git`  
B) `git clone https://github.com/trt15@ghp_RCnTgfJovGy6b2WX1WaVyrcfGoHMbdl20Myk/developer.git`  
C) `git clone https://github.com/user:trt15;pass:ghp_RCnTgfJovGy6b2WX1WaVyrcfGoHMbdl20Myk/developer.git`  
D) `git clone https://ghp_RCnTgfJovGy6b2WX1WaVyrcfGoHMbdl20Myk@github.com/trt15/developer.git`  
E) `git connect https://github.com/trt15@ghp_RCnTgfJovGy6b2WX1WaVyrcfGoHMbdl20Myk/developer.git`  

**22.** (FCC – TJ-CE/2022 – Analista Judiciário – Ciência da Computação / Sistemas da Informação) Considere que o Git foi configurado com sucesso, em condições ideais, em ambiente Linux. Depois, o usuário, a partir de seu projeto, em sua máquina local, digitou o seguinte comando: `>> git branch`. O resultado apresentado foi o nome do primeiro branch criado automaticamente pelo Git:

A) `* main`  
B) `* principal`  
C) `* master`  
D) `* root`  
E) `* base`  

**23.** (FCC – TRT-19/2022 – Técnico Judiciário – Tecnologia da Informação) O Git é um sistema de controle de versões utilizado no âmbito da Plataforma Digital do Poder Judiciário Brasileiro (PDPJ-Br). Uma vez que o Git esteja instalado e em condições ideais, um Técnico digita um comando que aciona o Git no seu diretório atual, criando a branch master e permitindo a ele executar os demais comandos do Git. Este comando é:

A) `git add`  
B) `git initialize`  
C) `git commit`  
D) `git init`  
E) `git master`  

**24.** (FCC – TRT-15/2023 – Analista Judiciário – Tecnologia da Informação) O Gitflow trabalha com dois branches principais: Develop e Master, que são permanentes, e

A) dois branches opcionais, Catalog e Basic, que são contínuos durante todo o fluxo CI/CD.  
B) três branches de suporte, Feature, Release e Hotfix, que são temporários e duram até realizar o merge com os branches principais.  
C) dois branches operacionais, Deploy e Release, que são temporários e duram até realizar o merge com os branches principais.  
D) quatro branches, Hotfix, Feature, Release e Deploy, que são contínuos durante todo o fluxo CI/CD.  
E) três branches de desenvolvimento, Feature, Release e Deploy, que são temporários e duram até realizar o merge com os branches principais.  

**25.** (FCC – TRF4/2025 – Analista Judiciário – Análise de Sistemas de Informação) Na implantação de um novo sistema de gestão processual, a equipe de analistas de um tribunal foi orientada a adotar práticas alinhadas aos princípios de DevOps e DevSecOps, além de estabelecer um fluxo organizado de versionamento de código-fonte utilizando Git e GitLab. No planejamento do pipeline de integração contínua e entrega contínua (CI/CD), optou-se por implementar uma estratégia de ramificação baseada no Gitflow para gerenciar versões de produção e desenvolvimento simultaneamente. Considerando essas práticas, a estruturação adequada desse fluxo envolve

A) a manutenção de uma branch principal voltada à estabilização do código, combinada com merges diretos de funcionalidades em produção sempre que aprovadas.  
B) a criação de fluxos de entrega contínua estruturados por desenvolvedor, com controle individual de versão descartando a dependência de branches colaborativas.  
C) a definição de pipelines segmentados por ambiente, com branches de longa duração integrando demandas operacionais e funcionais em ciclos contínuos.  
D) o uso de uma abordagem centralizada em uma única branch de desenvolvimento principal, com validações manuais antes da publicação das versões.  
E) a utilização de branches específicas como `"develop"` para integração de novas funcionalidades e `"master/main"` para versões estáveis de produção.  

---

## 4. Gabarito comentado

### Treino (1–15)

1. **B** — DVCS / controle de versão distribuído.  
2. **B** — Ferramenta (Git) × plataforma (GitHub).  
3. **B** — add → commit → push.  
4. **B** — Branch = linha; commit = snapshot.  
5. **B** — Fluxo Git + PR no GitHub.  
6. **B** — Fetch atualiza refs; pull integra.  
7. **D** — Maven ≠ VCS; Git versiona.  
8. **A** — Fonte Git → CI/CD → containers.  
9. **B** — Definição de GitHub.  
10. **B** — Mesmo trecho divergente.  
11. **C** — Orquestração ≠ papel do Git.  
12. **B** — Branch interno × fork = cópia de repo.  
13. **B** — Versiona fonte do ciclo de entrega.  
14. **B** — Cartão mental completo.  
15. **B** — Commit local ≠ push remote.

### Concurso (16–25)

| Q | Gab. | Fonte / nota |
|---|------|----------------|
| 16 | **A** | FCC TRT-5/2022 Analista — `git remote remove analista` (prova oficial) |
| 17 | **B** | FCC TRT-5/2022 Técnico — `git status` (Quizinho) |
| 18 | **C** | FCC TRT-17/2022 — `git push` após commit (Quizinho) |
| 19 | **D** | FCC TRT-12/2023 — `git merge novaFunc` (Quizinho) |
| 20 | **E** | FCC TRT-15/2024 — rebase no feature antes do merge (Quizinho / QConcursos) |
| 21 | **D** | FCC TRT-15/2023 — `git clone` com PAT na URL (Quizinho) |
| 22 | **C** | FCC TJ-CE/2022 — branch padrão histórico `* master` (Quizinho) |
| 23 | **D** | FCC TRT-19/2022 Técnico — `git init` (Estude Grátis / CFP) |
| 24 | **B** | FCC TRT-15/2023 — Feature, Release e Hotfix (Quizinho) |
| 25 | **E** | FCC TRF4/2025 — develop × master/main (Quizinho) |

Letras do gabarito neste bloco: A, B, C, D, E, D, C, D, B, E (A–E presentes).

---

## 5. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **histórico / commit / branch / merge / clone / push-pull**? → **Git**.  
2. É **PR, Issues, fork, hospedagem web do repo**? → **GitHub** (plataforma).  
3. É **pipeline / job / stage de CI** no GitLab? → **GitLab CI**.  
4. É **build Java / dependências**? → **Maven**.  
5. É **automação CI** clássica ou SaaS genérica? → **Jenkins** / **CircleCI**.  
6. É **imagem / pod / cluster**? → **Docker** / **Kubernetes** / **Rancher** — não confundir com VCS.

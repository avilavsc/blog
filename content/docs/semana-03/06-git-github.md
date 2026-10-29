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

## 3. Questões estilo concurso FCC (16–25)

**16.** Em contextos de desenvolvimento alinhados ao edital (DevOps e arquitetura PDPJ-Br), o Git caracteriza-se, em regra, por:

A) funcionar como catálogo exclusivo de registro e heartbeat de réplicas de microsserviço  
B) prover controle de versão distribuído do código e de artefatos versionáveis ao longo do tempo  
C) expor, por si só, a documentação interativa OpenAPI da API  
D) gerar, na compilação, mapeamentos tipados Entity ↔ DTO  
E) constituir o mecanismo SSO/OAuth2 da plataforma  

**17.** Relativamente à relação entre Git e GitHub, assinale a opção correta:

A) A existência de GitHub torna o uso de commits locais conceitualmente impossível  
B) O Git realiza o versionamento; o GitHub tipicamente hospeda repositórios e oferece recursos de colaboração como Pull Requests  
C) Git e GitHub são nomes comerciais do Eureka  
D) GitHub obriga banco único compartilhado entre todos os microsserviços  
E) Git substitui integralmente qualquer necessidade de CI/CD em todos os enunciados FCC  

**18.** Comparando o uso isolado de cópias manuais de pastas com o uso de Git em equipe, a vantagem tipicamente cobrada do controle de versão é:

A) eliminar a necessidade de controllers REST  
B) tornar mudanças rastreáveis, ramificáveis e sincronizáveis entre ambientes/desenvolvedores  
C) substituir o Service Discovery  
D) documentar try-it automaticamente no lugar do Swagger  
E) registrar a aplicação no Zuul como filtro error  

**19.** Assinale a alternativa que apresenta associação **incorreta**:

A) Git — controle de versão  
B) GitHub — hospedagem/colaboração sobre Git  
C) Maven — build/gerência de dependências Java  
D) Git — ponto único de entrada e roteamento de borda (papel de API Gateway)  
E) Jenkins / GitLab CI / CircleCI — automação CI/CD  

**20.** Em enunciado que contraste ferramentas do ciclo de entrega, a FCC tipicamente considera **correto** que:

A) Docker aplica commits e Git orquestra pods no cluster  
B) O Git versiona a fonte, ao passo que pipelines CI/CD automatizam build, teste e publicação a partir de pontos do histórico  
C) Git e Jenkins são intercambiáveis em qualquer item  
D) GitHub aplica rate limit na borda no lugar do gateway  
E) Maven realiza heartbeat das réplicas  

**21.** A expressão “proposta de integração de uma branch com revisão e discussão antes do merge na linha principal, em plataforma web” descreve com mais precisão:

A) Um `git add` isolado sem commit  
B) Um Pull Request (ou Merge Request, nomenclatura análoga em outras plataformas)  
C) Um conflito de merge já resolvido automaticamente sem revisão  
D) Um remote sem URL  
E) Um `.gitignore` vazio  

**22.** Em arquitetura de microsserviços com código em repositório Git e imagem Docker publicada via CI, o uso de Git **não** implica, por si só:

A) possibilidade de versionar o código de cada serviço  
B) substituição do orquestrador Kubernetes e do registry de containers  
C) histórico de commits e branches  
D) base para gatilhar pipelines a partir de push/tag  
E) rastreabilidade de quem alterou trechos do código  

**23.** Considere as afirmações:

I. GitHub Actions / CI de plataforma documentam, por si, o schema OpenAPI no lugar do Swagger.  
II. Git mantém histórico versionado e permite ramificação.  
III. GitLab CI não se confunde com o conceito de controle de versão do Git.

Assinale:

A) Apenas I  
B) Apenas II  
C) II e III  
D) I e II  
E) I, II e III  

**24.** Diferenciar Git de GitLab CI é relevante em provas porque:

A) não há qualquer diferença conceitual cobrável  
B) o Git enfatiza versionamento do código; o GitLab CI, automação de pipeline CI/CD  
C) Git só existe fora do ecossistema de software  
D) pipeline CI/CD é exclusividade do Eureka  
E) Git é apenas o nome antigo do MapStruct  

**25.** A síntese mais alinhada ao edital, sobre Git/GitHub, é:

A) Git = discovery; GitHub = gateway; Maven = docs  
B) Git = VCS distribuído; GitHub = hospedagem/colaboração; distingue-se de CI/CD (Jenkins, GitLab CI, CircleCI) e de build (Maven)  
C) Git obriga expor Entity no JSON e elimina DTO  
D) GitHub substitui Zuul, Eureka e OpenAPI  
E) Git é o banco H2 do Spring Boot  

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

16. **B** — Controle de versão distribuído.  
17. **B** — Git versiona; GitHub hospeda/colabora.  
18. **B** — Rastreio + branches + sync.  
19. **D** — Gateway ≠ Git.  
20. **B** — Git fonte; CI/CD automatiza a partir do histórico.  
21. **B** — PR / MR.  
22. **B** — Não substitui K8s/registry.  
23. **C** — I falsa; II e III verdadeiras.  
24. **B** — VCS × pipeline.  
25. **B** — Síntese do arquivo 06.

---

## 5. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **histórico / commit / branch / merge / clone / push-pull**? → **Git**.  
2. É **PR, Issues, fork, hospedagem web do repo**? → **GitHub** (plataforma).  
3. É **pipeline / job / stage de CI** no GitLab? → **GitLab CI**.  
4. É **build Java / dependências**? → **Maven**.  
5. É **automação CI** clássica ou SaaS genérica? → **Jenkins** / **CircleCI**.  
6. É **imagem / pod / cluster**? → **Docker** / **Kubernetes** / **Rancher** — não confundir com VCS.

+++
title = '04 — Rancher'
date = '2026-10-15'
draft = false
weight = 4
tags = ['semana-03', 'rancher']
categories = ['estudo']
+++
# Rancher · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — Rancher (visão cobrável)  
**Escopo deste arquivo:** o que o **Rancher** é no eixo Docker / Kubernetes / Rancher do edital; papel de **plataforma de gestão** de clusters; hierarquia **cluster → project → namespace**; **RKE / RKE2** em noção; provisionar × importar/registrar clusters; RBAC / multi-tenancy em alto nível; anti-confusão com Docker, K8s “puro”, CI/CD e Eureka.  
**Fora de escopo aqui:** instalação HA, tuning de etcd, troubleshooting profundo, Cattle/agent linha a linha, Istio/pipelines Rancher a fundo, Docker/K8s detalhados (outros arquivos da semana 3), GitLab CI/Jenkins, RabbitMQ. Cronograma: **cortar Rancher profundo** — aqui só o cobrável.

---

## 1. Material

### 1.1 Por que Rancher aparece no edital

O Anexo II cita, no eixo PDPJ / DevOps: **Git · Docker · Kubernetes · Rancher · CI/CD**. A leitura FCC típica empilha três camadas:

| Camada | Pergunta | Ferramenta de prova |
|--------|----------|---------------------|
| Empacotar / isolar | Como rodo a app em unidade portátil? | **Docker** (container) |
| Orquestrar | Como escalo, reinicio, expõe, agenda workloads? | **Kubernetes** |
| Governar / operar vários clusters | Como **gerencio** clusters K8s de forma central (UI/API, acessos, projetos)? | **Rancher** |

Rancher **não substitui** Kubernetes nem Docker: **sente-se em cima** do ecossistema de containers/orquestração para **gerir** clusters em produção, inclusive **vários** clusters e times.

Flash: se o enunciado fala em “plataforma de gerenciamento de clusters Kubernetes”, a resposta-alvo costuma ser **Rancher** — não Eureka, não Zuul, não MapStruct.

### 1.2 O que é Rancher (definição cobrável)

**Rancher** = plataforma (SUSE) de **gestão de containers / Kubernetes** voltada a organizações que precisam:

- rodar Kubernetes **em vários lugares** (on-prem, cloud, híbrido);
- **provisionar** clusters ou **importar/registrar** clusters já existentes;
- centralizar **autenticação, autorização (RBAC), projetos e namespaces**;
- oferecer **UI e API** para operações que, no K8s “cru”, ficam espalhadas em `kubectl` + vários YAMLs.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | **Gestão** de clusters Kubernetes (não o runtime do container sozinho) |
| Escala org. | Um painel / API para **múltiplos** clusters e equipes |
| Conceito próprio | **Project** (agrupa namespaces) — ideia **do Rancher**, não nativa do K8s |

Anti-definição (errar em prova):

- “Rancher = substituto do Kubernetes” → **falso**  
- “Rancher = só Docker Compose com outro nome” → **falso**  
- “Rancher = Service Discovery tipo Eureka” → **falso**  
- “Rancher = ferramenta exclusiva de CI/CD” → **falso** (pode integrar pipelines/catálogo; o núcleo cobrado é **gestão de clusters**)

### 1.3 Docker × Kubernetes × Rancher (mapa anti-confusão)

| | **Docker** | **Kubernetes (K8s)** | **Rancher** |
|---|------------|----------------------|-------------|
| Foco | Imagem / container / runtime | Orquestração de containers (Pods, Deployments, Services…) | **Plataforma de gestão** de clusters K8s |
| Unidade mental | Container | Cluster + workloads | **Vários clusters** + governança (projetos, RBAC, UI) |
| Pergunta | Como empacoto e executo? | Como orquestro em escala? | Como **administro** o(s) cluster(s) com times e políticas? |

Síntese de prova:

```
App → imagem Docker → workloads no Kubernetes → gestão multi-cluster / multi-equipe no Rancher
```

Complementares, **não** sinônimos. Gateway/Eureka (semana 1) e Keycloak (semana 2) resolvem **outras** perguntas (entrada, discovery, identidade de aplicação).

### 1.4 Cluster “local” (upstream) × clusters gerenciados (downstream)

Vocabulário útil (sem virar glossário de ops):

| Termo | Ideia cobrável |
|-------|----------------|
| **Rancher Server** / cluster **local** (upstream) | Onde o Rancher **roda**; controla os demais |
| **Clusters gerenciados** (downstream) | Clusters Kubernetes **geridos** pelo Rancher (onde as apps de negócio tipicamente rodam) |
| **Provisionado** | Rancher **cria/instala** o K8s (ex.: via RKE/RKE2 em nós) |
| **Importado / registrado** | Cluster **já existente** (outra distribuição / GKE / EKS / AKS / on-prem) passa a ser **gerido** pelo Rancher |

Leitura FCC: Rancher serve tanto para **nascer** clusters quanto para **abraçar** clusters prontos — o valor é a **gestão centralizada**, não “só um instalador”.

### 1.5 RKE e RKE2 (noção, não deep ops)

**RKE** (*Rancher Kubernetes Engine*) = distribuição / instalador Kubernetes associado ao ecossistema Rancher (linha clássica frequentemente chamada RKE1).  
**RKE2** = linha mais recente, alinhada a requisitos de segurança/compliance mais rígidos.

Para a prova, retenha:

- RKE/RKE2 = forma do Rancher (ou do ecossistema) de **obter um Kubernetes certificável**;
- Rancher **Manager** = a **plataforma de gestão**; RKE/RKE2 = **engine/distribuição** de cluster;
- Não precisa decorar flags de instalação, versões de CNI ou HA etcd.

Flash: “RKE = Kubernetes do Rancher”; “Rancher = painel/gestão em cima dos clusters”.

### 1.6 Hierarquia ouro: Cluster → Project → Namespace

No Kubernetes nativo:

- **Cluster** → contém **namespaces** (isolamento lógico de recursos).

No Rancher, entra o **Project**:

| Nível | Origem | Ideia |
|-------|--------|-------|
| **Cluster** | Kubernetes (+ gestão Rancher) | Conjunto de nós / plano de controle |
| **Project** | **Conceito Rancher** | **Grupo de namespaces** + políticas/membros compartilhados |
| **Namespace** | Kubernetes | Partição lógica dentro do cluster |

Hierarquia cobrável:

```
Cluster
  └── Project(s)     ← Rancher (não existe “Project” nativo no K8s vanilla)
        └── Namespace(s)
```

Para quê o Project?

- aplicar **RBAC / membros** a **vários** namespaces de uma vez;
- apoiar **multi-tenancy** (times compartilham o cluster sem “ver” uns aos outros);
- cotas / políticas no nível do projeto (visão);
- reduzir o trabalho repetitivo de configurar namespace a namespace.

Ao criar um cluster no Rancher, costumam existir projetos iniciais como **Default** e **System** (só reconhecer o nome; sem ops).

**Armadilha clássica:** tratar Project como sinônimo de Namespace, de Pod, ou de “repositório Git”.

### 1.7 O que a UI/API do Rancher costuma centralizar (visão)

Sem checklist de menu:

| Capacidade | Leitura de prova |
|------------|------------------|
| **Ciclo de vida do cluster** | Criar, atualizar, importar/registrar, observar saúde (alto nível) |
| **Usuários e autenticação** | Integração com provedores (AD, GitHub, etc.) + usuários locais |
| **Autorização / RBAC** | Papéis em nível global, de cluster e de **project** |
| **Workloads / apps** | Deploy e operação via UI (e catálogo / Helm — noção) |
| **Projetos e namespaces** | Organizar multi-equipe |

Catálogo / Helm: Rancher facilita **repetir deploys** a partir de charts — ideia de “catálogo de aplicações”, não substituir Maven/Jenkins como pipeline completo do edital.

### 1.8 RBAC e multi-tenancy (só o cobrável)

Em cluster K8s compartilhado por várias equipes:

- sem Project: políticas e acessos **namespace a namespace** (repetitivo);
- com Project: membro / owner / read-only (e papéis custom) no **grupo** de namespaces.

Pergunta mental da banca:

> Como isolar times no **mesmo** cluster Kubernetes com gestão centralizada?

Resposta-alvo: **projetos Rancher** (+ RBAC), não “um Eureka por time” nem “um Docker Compose por servidor”.

### 1.9 Encaixe no eixo PDPJ / tribunal (leitura de concurso)

O edital lista Rancher junto de Docker e Kubernetes no pacote de **plataformas e práticas** de entrega/operação — não como autenticador da PDPJ (isso é **Keycloak/SSO**) nem como discovery (**Eureka**).

Mapa rápido do ecossistema de prova:

| Necessidade | Ferramenta |
|-------------|------------|
| Identidade / tokens | Keycloak + OAuth2 |
| Entrada HTTP | API Gateway (Zuul / Spring Cloud Gateway) |
| Onde está a réplica do MS? | Eureka / Service Discovery |
| Container | Docker |
| Orquestração | Kubernetes |
| Gestão de clusters K8s | **Rancher** |
| Pipeline build/deploy | CI/CD (GitLab CI, Jenkins, …) |

Flash: Rancher **opera a plataforma** de containers/K8s; não responde “quem é o usuário do PJe”.

### 1.10 O que NÃO estudar agora (corte deliberado)

Alinhado ao cronograma (“cortar Rancher profundo”):

- HA do Rancher Server, backup etcd, certificados internos linha a linha;
- detalhe de agentes, Cattle, drivers de cloud;
- Istio “embutido”, monitoring stack completo só via Rancher;
- comparação comercial Rancher Prime vs open source além da noção de produto SUSE;
- comandos de instalação e troubleshooting de nó.

Se sobrar tempo na semana 3: reforçar **Docker + K8s conceitos** — pesam mais que ops Rancher.

### 1.11 Armadilhas FCC (checklist)

- Dizer que Rancher **substitui** Kubernetes.  
- Confundir Rancher com **Docker** (só runtime/imagem).  
- Trocar Rancher por **Eureka**, **Zuul**, **Keycloak** ou **Flyway**.  
- Tratar **Project** como recurso nativo do Kubernetes vanilla.  
- Achar que Rancher = **único** CI/CD do edital (GitLab CI / Jenkins / CircleCI / Maven têm arquivo próprio).  
- Reduzir Rancher a “interface gráfica do `kubectl`” sem a ideia de **multi-cluster / projects / RBAC**.  
- Misturar **namespace** (K8s) com **project** (Rancher) como se fossem iguais.

### 1.12 Cartão-resumo (antes das questões)

| Tema | Âncora |
|------|--------|
| Rancher | Plataforma de **gestão** de clusters Kubernetes |
| Pilha | Docker → Kubernetes → **Rancher** |
| RKE/RKE2 | Engine/distribuição K8s do ecossistema Rancher |
| Hierarquia | Cluster → **Project** → Namespace |
| Project | Conceito **Rancher**; agrupa namespaces + RBAC |
| Modos | Provisionar **ou** importar/registrar clusters |
| Não é | Eureka, Gateway, Keycloak, CI/CD exclusivo, substituto do K8s |

---

## 2. Questões de treino (1–15)

**1.** No eixo Docker / Kubernetes / Rancher do edital, o Rancher é melhor descrito como:

A) Protocolo de autenticação OAuth 2.0  
B) Service registry com heartbeat (Eureka)  
C) Migrador de schema SQL (Flyway)  
D) Plataforma de **gestão** de clusters Kubernetes  
E) Mapper compile-time Entity↔DTO  

**2.** Assinale a associação correta das camadas:

A) Docker = gestão multi-cluster; Kubernetes = só imagem; Rancher = Service Discovery  
B) Docker = container; Kubernetes = orquestração; Rancher = gestão de clusters  
C) Rancher = substituto obrigatório do Docker; Kubernetes é opcional sempre  
D) Eureka = Rancher; Zuul = RKE  
E) Keycloak = orquestrador de Pods  

**3.** Sobre a relação Rancher × Kubernetes:

A) Rancher tipicamente **gerencia** clusters Kubernetes; não os “apaga” do desenho  
B) Rancher elimina a necessidade de qualquer orquestrador  
C) Kubernetes é plugin exclusivo do MapStruct  
D) Rancher só funciona se Eureka estiver desligado  
E) Todo cluster Rancher proíbe o uso de namespaces  

**4.** O conceito de **Project** no Rancher:

A) É idêntico ao namespace nativo e existe com o mesmo nome no Kubernetes vanilla  
B) É o nome do access token OAuth  
C) Substitui o PostgreSQL na PDPJ  
D) É o único tipo de Pod permitido  
E) É um **agrupamento de namespaces** (conceito Rancher) para RBAC/políticas compartilhadas  

**5.** A hierarquia cobrável no Rancher é:

A) Cluster → Project → Namespace  
B) Namespace → Project → Cluster  
C) Pod → Eureka → Gateway  
D) Realm → Client → Flyway  
E) Commit → Branch → Namespace  

**6.** RKE / RKE2, em visão de prova, referem-se principalmente a:

A) RFCs de OAuth  
B) Bancos H2 embutidos  
C) Filtros Zuul  
D) Distribuição / engine Kubernetes do ecossistema Rancher  
E) Anotações JPA `@Entity`  

**7.** Importar ou registrar um cluster no Rancher significa, em regra:

A) Passar a **gerenciar** um cluster Kubernetes já existente a partir do Rancher  
B) Apagar o Kubernetes e trocar por Docker Compose  
C) Converter Eureka em Authorization Server  
D) Gerar DTOs com MapStruct  
E) Obrigar o uso exclusivo de Implicit grant  

**8.** Qual necessidade de negócio o Rancher atende melhor entre as opções?

A) Emitir JWT de SSO da PDPJ  
B) Documentar OpenAPI/Swagger UI  
C) Governar **múltiplos** clusters K8s e equipes com UI/API e projetos  
D) Versionar migrações Flyway  
E) Resolver nome lógico de microsserviço via heartbeat  

**9.** Assinale a linha **incorreta**:

A) Project é conceito Rancher  
B) Namespace é conceito Kubernetes  
C) Rancher pode provisionar clusters (ex.: via RKE)  
D) Rancher = sinônimo exato de Service Discovery Eureka  
E) Docker empacota a aplicação em container  

**10.** Multi-tenancy no mesmo cluster, com times isolados via Rancher, apoia-se tipicamente em:

A) Um Keycloak por Pod obrigatório  
B) Desligar o API Gateway  
C) Usar só H2 em produção  
D) **Projects** + RBAC sobre grupos de namespaces  
E) Trocar REST por FTP  

**11.** Qual contraste está alinhado ao edital?

A) Keycloak = SSO/tokens; Rancher = gestão de clusters containers/K8s  
B) Rancher autentica usuários finais da PDPJ; Keycloak gerencia clusters K8s  
C) Eureka = RKE2; Rancher = Envers  
D) Flyway = orquestração; Docker = RBAC de project  
E) Zuul = Project; Namespace = OAuth AS  

**12.** Catálogo / Helm no Rancher (noção) serve principalmente para:

A) Substituir o protocolo TCP  
B) Emitir authorization codes  
C) Facilitar deploys repetíveis de aplicações (charts) no cluster gerido  
D) Fazer discovery no lugar do Eureka  
E) Normalizar banco até 5FN automaticamente  

**13.** “Rancher Server” / cluster local (upstream), em vocabulário cobrável, é:

A) O microsserviço de pedidos da PDPJ  
B) O único namespace permitido no K8s  
C) O Authorization Server da RFC 6749  
D) O repositório Git obrigatório  
E) Onde o Rancher **roda** e a partir de onde **gerencia** outros clusters  

**14.** Qual afirmação melhor resume o corte de estudo deste arquivo?

A) Decoração de HA etcd e flags de CNI é prioridade máxima sobre Docker/K8s  
B) Rancher substitui toda a semana de redes  
C) Visão: gestão multi-cluster, Project, RKE noção, anti-confusão — **sem** deep ops  
D) É obrigatório memorizar todos os drivers de cloud  
E) FCC só cobra comandos `rke up` linha a linha  

**15.** Qual opção sintetiza o cartão mental deste subtópico?

A) Rancher = gestão K8s (cluster→project→namespace; RKE noção; ≠ Docker/Eureka/Keycloak/CI exclusivo)  
B) Rancher = Eureka + Zuul + Flyway  
C) Project = Pod; Namespace = OAuth client  
D) RKE = PostgreSQL; Rancher = H2  
E) Docker elimina Kubernetes e Rancher  

---

### Gabarito comentado (treino 1–15)

1. **D** — Gestão de clusters K8s.  
2. **B** — Container → orquestração → gestão.  
3. **A** — Gerencia K8s; não o remove.  
4. **E** — Project agrupa namespaces (Rancher).  
5. **A** — Cluster → Project → Namespace.  
6. **D** — Engine/distribuição K8s Rancher.  
7. **A** — Cluster existente sob gestão.  
8. **C** — Multi-cluster / times / UI-API.  
9. **D** — Rancher ≠ Eureka (incorreta = resposta).  
10. **D** — Projects + RBAC.  
11. **A** — Keycloak identidade; Rancher clusters.  
12. **C** — Catálogo/Helm = deploys repetíveis.  
13. **E** — Server/upstream gerencia downstream.  
14. **C** — Visão cobrável, sem deep ops.  
15. **A** — Síntese do arquivo 04.  

---

## 3. Questões estilo concurso FCC (16–25)

**16.** Em ambientes que adotam containers e orquestração Kubernetes, o Rancher é empregado, em regra, para:

A) Substituir integralmente o protocolo HTTP/REST da PDPJ  
B) Atuar exclusivamente como banco relacional embutido  
C) Eliminar a necessidade de imagens Docker  
D) Oferecer plataforma de **gerenciamento** de clusters Kubernetes, com recursos de organização (como projetos) e controle de acesso  
E) Cumprir o papel de Service Discovery no lugar de qualquer registry  

**17.** Considere as afirmativas:

I. O Rancher pode gerenciar múltiplos clusters Kubernetes.  
II. O conceito de *project* no Rancher permite agrupar *namespaces* para fins de administração e acesso.  
III. O Rancher é, por definição, o Authorization Server OAuth 2.0 da RFC 6749.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**18.** Relacione corretamente:

(1) Docker  
(2) Kubernetes  
(3) Rancher  

( ) Orquestração de containers em cluster  
( ) Empacotamento e execução em containers  
( ) Gestão centralizada de clusters Kubernetes (UI/API, projetos, RBAC)

A sequência correta é:

A) 1, 2, 3  
B) 2, 1, 3  
C) 3, 2, 1  
D) 2, 3, 1  
E) 1, 3, 2  

**19.** No Rancher, a estrutura que **não** corresponde a um objeto nativo do Kubernetes “vanilla”, sendo introduzida pela plataforma para agrupar *namespaces*, denomina-se:

A) Pod  
B) Service  
C) Project  
D) Deployment  
E) ConfigMap  

**20.** A utilização do RKE (*Rancher Kubernetes Engine*) / RKE2, no discurso cobrável do ecossistema Rancher, associa-se principalmente a:

A) Distribuição / instalação de clusters Kubernetes gerenciáveis no ecossistema Rancher  
B) Emissão de *refresh tokens* OAuth  
C) Mapeamento JPA Entity↔DTO  
D) Documentação OpenAPI obrigatória  
E) Substituição do TCP/IP  

**21.** Um tribunal já possui clusters Kubernetes em operação e deseja **centralizar** administração, acessos e organização por equipes sem reescrever as aplicações. Uma abordagem alinhada ao papel do Rancher é:

A) Descartar Kubernetes e adotar apenas scripts shell locais  
B) Trocar todos os bancos por H2 em memória  
C) Remover namespaces e rodar tudo no mesmo Pod  
D) **Importar/registrar** os clusters no Rancher para gestão unificada  
E) Usar somente Eureka como orquestrador de containers  

**22.** Assinale a alternativa que apresenta **confusão conceitual**:

A) Namespace isola recursos no cluster Kubernetes  
B) Project no Rancher agrupa namespaces  
C) Rancher Server (cluster local) hospeda a plataforma que gerencia outros clusters  
D) Rancher e Eureka são nomes comerciais do mesmo componente Spring Cloud Netflix  
E) Docker produz imagens/containers consumidos no cluster  

**23.** Em prova alinhada ao edital do TRT4 (código 14), a menção conjunta a Docker, Kubernetes e Rancher sugere, prioritariamente:

A) Pilha de **containers → orquestração → gestão** de plataforma  
B) Pilha exclusiva de cabeamento estruturado  
C) Substituição de SSO/Keycloak pelo Rancher  
D) Obrigatoriedade de VB.NET em todos os microsserviços  
E) Uso de Rancher como único banco de dados do PJe  

**24.** Acerca de controle de acesso em clusters compartilhados geridos pelo Rancher, é correto afirmar que:

A) RBAC e membership em **projects** permitem autorizar equipes sobre conjuntos de namespaces  
B) Apenas o protocolo FTP define papéis de cluster  
C) Todo usuário anônimo recebe role de cluster-admin por padrão obrigatório  
D) Project elimina a existência de namespaces  
E) Autorização no Rancher substitui integralmente a LGPD  

**25.** Analise o trecho: “Ferramenta X não substitui o orquestrador; oferece painel e API para operar um ou vários clusters Kubernetes, organizando namespaces em projects e integrando autenticação/autorização corporativa.”

A ferramenta X corresponde, no vocabulário do edital, a:

A) Hibernate Envers  
B) MapStruct  
C) Rancher  
D) Flyway  
E) H2  

---

### Gabarito comentado (FCC 16–25)

16. **D** — Gestão de clusters + organização/acesso.  
17. **C** — I e II verdadeiras; III falsa (OAuth/AS ≠ Rancher).  
18. **B** — K8s orquestra; Docker empacota; Rancher gerencia.  
19. **C** — Project é do Rancher.  
20. **A** — Engine/distribuição K8s.  
21. **D** — Importar/registrar para gestão central.  
22. **D** — Confusão Rancher≠Eureka (alternativa incorreta = gabarito).  
23. **A** — Containers → orquestração → gestão.  
24. **A** — Projects + RBAC multi-equipe.  
25. **C** — Definição de Rancher.  

---

## 4. Como usar este arquivo

1. Ler §1 (cartão §1.12 no fim).  
2. Fechar treino 1–15 sem gabarito → conferir.  
3. Fazer 16–25 em modo prova (sem consultar o material).  
4. Anotar erros no caderno (especialmente Project × Namespace e Rancher × Eureka/Keycloak/CI).  
5. Se o tempo apertar na semana 3: este arquivo é **visão**; priorize Docker/K8s conceituais e CI/CD antes de ops Rancher.

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

## 3. Questões de concurso (reais) (16–25)

> Questões reais (Rancher é escasso na FCC: inclui as que citam Rancher + mais próximas de K8s/PDPJ/containers). Gabarito só ao final.

**16.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Nos projetos da Plataforma Digital do Poder Judiciário PDPJ-Br,

A) os ambientes de deployment estão instalados em clusters Kubernetes, gerenciados por meio da ferramenta open source de orquestração de containers Rancher.  
B) a aplicação deve possuir os arquivos `yanl`, para que o deploy ocorra de forma automática por meio de pipelines de CI/CD executados pelo GitLab do STF.  
C) a aplicação deve possuir um arquivo `Rancherfile`, especificando o modo pelo qual ela deve ser empacotada.  
D) deve haver o arquivo `.gitlab-ci.xml` definindo a pipeline de deploy automático.  
E) deve haver, na pasta `kubernets`, o arquivo de configuração do ingress da aplicação, que é obrigatório.

**17.** (FCC – TRT-23/2022 – Técnico Judiciário – TI)  
Um Técnico precisa administrar e monitorar containers Docker em produção. Para isso pode utilizar a ferramenta

A) Rancher  
B) Webhooker  
C) Swagger  
D) Zuul  
E) Flyway

**18.** (FGV – TJ-SC/2026 – Analista de Sistemas)  
O setor de tecnologia de um tribunal desenvolveu uma aplicação web composta por três serviços: cadastro de usuários, consulta processual e geração de relatórios. Cada serviço possui dependências próprias e deve ser implantado de forma padronizada nos ambientes de desenvolvimento, homologação e produção. Além disso, a equipe precisa distribuir réplicas desses serviços em diferentes nós de um cluster, reiniciar automaticamente instâncias com falha, ampliar a quantidade de réplicas em períodos de maior demanda e administrar, por uma interface centralizada, mais de um cluster Kubernetes utilizado pela instituição.  
Nesse contexto, é correto afirmar que

A) o Docker permite empacotar aplicações e dependências em containers, o Kubernetes orquestra a implantação e o escalonamento desses containers, e o Rancher auxilia na gestão centralizada de clusters Kubernetes.  
B) o Docker permite empacotar aplicações e dependências em containers, o Kubernetes orquestra a implantação e o escalonamento desses containers, e o Rancher é um formato de imagem utilizado para distribuir containers.  
C) o Docker permite empacotar aplicações e dependências em containers, o Kubernetes é utilizado principalmente para criar imagens de containers, e o Rancher auxilia na gestão centralizada de clusters Kubernetes.  
D) o Docker é utilizado principalmente para empacotar aplicações e dependências em máquinas virtuais completas, o Kubernetes orquestra a implantação e o escalonamento de containers, e o Rancher auxilia na gestão centralizada de clusters Kubernetes.  
E) o Docker permite empacotar aplicações e dependências em containers, o Kubernetes orquestra a implantação e o escalonamento desses containers, e o Rancher substitui a necessidade de clusters Kubernetes por execução local isolada.

**19.** (FGV – MPE-ES/2026 – Analista de Infraestrutura / DevOps)  
A orquestração de contêineres permite automatizar a implantação, o escalonamento e o gerenciamento de aplicações distribuídas. Analise as afirmativas a seguir.  
I. O Kubernetes é uma plataforma de orquestração de contêineres que permite automatizar a implantação, o escalonamento e a gestão de aplicações em clusters de nós computacionais.  
II. O Rancher é uma ferramenta nativa do Kubernetes utilizada para o gerenciamento de clusters.  
III. Em um cluster Kubernetes, os nós de trabalho executam os pods, enquanto o plano de controle é responsável pelo gerenciamento do estado do cluster.  
Está correto o que se afirma em

A) I, apenas.  
B) I e II, apenas.  
C) I e III, apenas.  
D) II e III, apenas.  
E) I, II e III.

**20.** (FCC – TRT-14/2022 – Analista Judiciário – TI)  
Para coordenar e agendar vários contêineres, habilitar comunicações entre eles e escalar instâncias de contêiner, um analista pode utilizar o

A) Kubernetes, solução utilizada para vários tipos de contêiner, incluindo Docker.  
B) Swagger, solução exclusiva para contêineres Docker.  
C) Kubernetes, solução exclusiva para contêineres Docker.  
D) Swagger, solução exclusiva para contêineres Zuul.  
E) Keycloak, solução utilizada para qualquer tipo de contêiner.

**21.** (FCC – TRT-4/2022 – Técnico Judiciário – TI)  
Um Técnico está participando de um projeto de microsserviços distribuídos onde centenas de contêineres são utilizados. Para fazer a orquestração destes contêineres, ou seja, a coordenação e o monitoramento, ele precisa utilizar um recurso que possa permitir que a atualização da aplicação em um contêiner não interrompa o serviço. O recurso sugerido corretamente por ele, nesse caso, foi o

A) Qubes.  
B) VirtualBox.  
C) VMWare.  
D) Docker, em modo padrão.  
E) Kubernetes.

**22.** (FGV – TJ-RJ/2026 – Analista de Infraestrutura de TIC)  
No contexto dos conceitos de Docker e Kubernetes, é correto afirmar que:

A) o Kubernetes é utilizado exclusivamente na etapa de desenvolvimento para a construção das imagens, cabendo ao Docker a responsabilidade pelo balanceamento de carga em produção;  
B) eles são tecnologias mutuamente excludentes, uma vez que a adoção do orquestrador Kubernetes elimina, por definição, a necessidade de qualquer mecanismo de empacotamento de contêineres;  
C) o Docker atua como um hipervisor de tipo 1 para virtualizar o hardware físico, enquanto o Kubernetes opera como o sistema operacional convidado responsável pela execução das aplicações;  
D) o Docker provê os mecanismos para criação e execução de contêineres individuais, enquanto o Kubernetes gerencia a orquestração, o escalonamento e a resiliência desses contêineres em cluster;  
E) o Kubernetes substitui integralmente a necessidade de um container runtime subjacente, executando os processos da aplicação diretamente sobre o processador do servidor sem camadas intermediárias.

**23.** (FCC – TRT-17/2022 – Técnico Judiciário – TI)  
Considere as seguintes definições:  
I. Gerencia os containers em execução e, por isso, ele também é chamado de Orquestrador de Containers. Através dele se pode definir o estado de um sistema completo, por exemplo, baseado em Microservices, seguindo boas práticas de infraestrutura como código, permitindo balanceamento de carga, alta disponibilidade, atualizações em lote e rollbacks.  
II. Refere-se a um grupo de servidores distribuídos geograficamente que trabalham em conjunto para oferecer uma rápida entrega de conteúdo de internet. Permite uma transferência rápida dos ativos necessários para carregar o conteúdo da internet, incluindo páginas HTML, arquivos em JavaScript, folhas de estilo, imagens e vídeos.  
III. Projeto opensource escrito em Go, que torna a criação e o gerenciamento de containers Linux mais fácil. O container é construído usando namepspaces, cgroups, chroot entre outras funcionalidades do kernel para criar uma área isolada para a aplicação. É executado como um processo isolado no sistema operacional hospedeiro, compartilhando o kernel com outros recipientes.  
As definições I, II e III correspondem, correta e respectivamente, a

A) Kubernetes, Docker e Content Delivery Network (CDN).  
B) Content Delivery Network (CDN), Kubernetes e Docker.  
C) Content Delivery Network (CDN), Docker e Kubernetes.  
D) Docker, Content Delivery Network (CDN) e Kubernetes.  
E) Kubernetes, Content Delivery Network (CDN) e Docker.

**24.** **(FGV / DATAPREV / 2026)**  
Uma sociedade empresária executa microsserviços com `docker run` manual em cada servidor. Quando um container falha, fica parado até reinício manual, e não há distribuição de tráfego entre réplicas.  
A equipe quer uma solução nativa para cluster com múltiplos servidores, capaz de reiniciar falhas, manter réplicas desejadas e balancear requisições.  
Assinale a opção que apresenta uma solução adequada a esse cenário.

A) Migrar os containers para máquinas virtuais dedicadas, eliminando a necessidade de qualquer camada de orquestração.  
B) Configurar um script cron que verifica periodicamente, a cada minuto, se os containers estão em execução, reiniciando aqueles que estiverem parados.  
C) Adotar o Docker Compose, orquestrando os containers da aplicação a partir de um único arquivo `docker-compose.yml`, capaz de distribuir automaticamente as réplicas entre múltiplos servidores físicos.  
D) Adotar o Rancher isoladamente, sem um cluster Kubernetes ou similar subjacente, já que o Rancher por si só provê o agendamento (scheduling) e a recuperação automática de containers entre múltiplos nós.  
E) Adotar o Kubernetes, cujos controladores monitoram continuamente o estado dos pods, recriando instâncias em caso de falha e mantendo o número de réplicas configurado, além de distribuir o tráfego entre elas por meio de um Service.

**25.** **(FCC / PMSP / 2025)**  
Durante a implementação de um sistema de atendimento ao cidadão em nuvem, a equipe de TI da prefeitura observou picos de demanda em horários de expediente. Para garantir que os custos sejam otimizados e o desempenho não seja impactado durante esses picos, deve-se

A) configurar auto-escalabilidade no cluster Kubernetes para ajustar automaticamente o número de pods em resposta à carga.  
B) configurar a escalabilidade manual do cluster para economizar custos, ajustando os pods apenas no início e no final do expediente.  
C) definir alertas de custo no painel da nuvem para reduzir automaticamente os recursos quando o orçamento for excedido.  
D) implementar instâncias reservadas em nuvem para atender a picos de demanda, mesmo que o custo seja mais alto.  
E) utilizar pipelines CI/CD para ajustar os recursos computacionais automaticamente provendo escalabilidade vertical.

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **A** | FCC TRT-19/2022 — PDPJ: clusters K8s gerenciados pelo Rancher |
| 17 | **A** | FCC TRT-23/2022 — administrar/monitorar containers → Rancher |
| 18 | **A** | FGV TJ-SC/2026 — Docker + Kubernetes + Rancher (gestão multi-cluster) |
| 19 | **C** | FGV MPE-ES/2026 — I e III; II falsa (Rancher **não** é nativo do K8s) |
| 20 | **A** | FCC TRT-14/2022 — Kubernetes orquestra contêineres |
| 21 | **E** | FCC TRT-4/2022 — Kubernetes (orquestração sem interrupção) |
| 22 | **D** | FGV TJ-RJ/2026 — Docker empacota; K8s orquestra |
| 23 | **E** | FCC TRT-17/2022 — K8s / CDN / Docker |
| 24 | **E** | FGV DATAPREV/2026 — K8s (+ Service); Rancher isolado não orquestra |
| 25 | **A** | FCC PMSP/2025 — auto-escalabilidade de pods no cluster |

**Notas rápidas:** 16 A (Rancher na PDPJ). 17 A (Rancher). 18 A (tríade Docker/K8s/Rancher). 19 C (Rancher ≠ nativo). 20 A (K8s). 21 E (K8s). 22 D (papéis). 23 E (K8s/CDN/Docker). 24 E (K8s, não Rancher solo). 25 A (HPA).

---

## 4. Como usar este arquivo

1. Ler §1 (cartão §1.12 no fim).  
2. Fechar treino 1–15 sem gabarito → conferir.  
3. Fazer 16–25 em modo prova (sem consultar o material).  
4. Anotar erros no caderno (especialmente Project × Namespace e Rancher × Eureka/Keycloak/CI).  
5. Se o tempo apertar na semana 3: este arquivo é **visão**; priorize Docker/K8s conceituais e CI/CD antes de ops Rancher.

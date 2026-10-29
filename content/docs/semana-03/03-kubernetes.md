+++
title = '03 — Kubernetes'
date = '2026-10-14'
draft = false
weight = 3
tags = ['semana-03', 'kubernetes']
categories = ['estudo']
+++
# Kubernetes · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — Kubernetes (orquestração de containers)  
**Escopo deste arquivo:** o que é **Kubernetes (K8s)**; papel no eixo **Docker → K8s → Rancher**; arquitetura do **cluster** (control plane × worker nodes); objetos cobráveis (**Pod**, **ReplicaSet**, **Deployment**, **Service**, **Namespace**, **ConfigMap**, **Secret**, noção de **Ingress** / **StatefulSet** / **DaemonSet**); estado desejado e reconciliação; labels/seletores; rolling update; `kubectl` mental; anti-confusão com Docker, Eureka, Gateway e Rancher.  
**Fora de escopo aqui:** mensageria RabbitMQ (outro arquivo); Docker a fundo (arquivo 02); Rancher a fundo (arquivo dedicado); CI/CD GitLab/Jenkins; Git; segurança OWASP/LGPD em profundidade; YAML linha a linha de operadores avançados.

---

## 1. Material

### 1.1 O problema que o Kubernetes resolve

**Docker** (visão de prova) empacota e executa **containers**. Em produção com microsserviços (PDPJ-Br / tribunais), sobram perguntas:

- como **escalar** N réplicas?
- como **substituir** containers que morreram?
- como **balancear** tráfego entre réplicas?
- como **atualizar** sem derrubar o serviço de uma vez?
- como **declarar** o estado desejado e deixar a plataforma convergir?

**Kubernetes** = plataforma de **orquestração** de workloads conteinerizados: implantar, escalar, auto-reparar e gerenciar ciclo de vida em **cluster**.

| Camada | Frase cobrável |
|--------|----------------|
| Docker / runtime | Empacota e roda o container |
| **Kubernetes** | Orquestra muitos containers/Pods no cluster |
| Rancher (visão) | Plataforma de **gestão**/operação de clusters K8s (UI, multi-cluster) |

Flash: K8s **não** substitui a ideia de imagem/container; **orquestra** workloads que já são conteinerizados.

### 1.2 Kubernetes no eixo do edital / PDPJ

No conteúdo programático (Arquitetura PDPJ-Br): **Git · Docker · Kubernetes · Rancher · CI/CD**.

Leitura FCC:

- **Docker** = build/run de containers;
- **Kubernetes** = orquestração em cluster;
- **Rancher** = gestão/operacionalização de K8s (não é “outro motor de Pod” distinto do K8s no discurso de prova).

Anti-confusão com semana 1:

| Componente | Função |
|------------|--------|
| Eureka | Service Discovery de microsserviços Spring |
| API Gateway / Zuul | Entrada/borda HTTP |
| Keycloak | SSO / tokens |
| **Kubernetes** | Onde os containers/Pods **correm** e são geridos |

K8s **pode** oferecer Service interno estável; isso **não** torna Eureka “obsoleto por definição” no discurso Spring Cloud do edital — são camadas diferentes.

### 1.3 Cluster: control plane × worker nodes

```
        CONTROL PLANE                          WORKER NODES
 ┌─────────────────────────────┐         ┌──────────────────┐
 │ kube-apiserver              │         │ kubelet          │
 │ etcd                        │◄───────►│ kube-proxy       │
 │ kube-scheduler              │         │ container runtime│
 │ kube-controller-manager     │         │   [Pods…]        │
 └─────────────────────────────┘         └──────────────────┘
```

| Componente | Função cobrável |
|------------|-----------------|
| **kube-apiserver** | Porta de entrada da API; `kubectl` e componentes falam com o cluster por aqui |
| **etcd** | Armazena o **estado** do cluster (chave-valor consistente); backup é crítico |
| **kube-scheduler** | **Decide** em qual nó o Pod novo será colocado (não “executa” o container) |
| **kube-controller-manager** | Loops de controle: mantém o **estado desejado** (réplicas, nós etc.) |
| **kubelet** | Agente no nó: garante que os containers do Pod estejam rodando |
| **kube-proxy** | Regras de rede no nó; apoia a abstração de **Service** |
| **Container runtime** | Executa containers (ex.: containerd, CRI-O — via CRI) |

Pegadinhas de arquitetura (ouro de prova):

1. **Scheduler escolhe o nó; kubelet executa.**  
2. Em regra, **só o API Server fala diretamente com o etcd.**  
3. Perder etcd sem backup ≈ perder a “memória” do cluster.  
4. Docker Engine como runtime via *dockershim* saiu do caminho moderno; imagens OCI/Docker **continuam** usáveis com runtimes CRI.

### 1.4 Estado desejado e reconciliação

Modelo mental:

> Você declara o **desejado** (ex.: 3 réplicas do Deployment `api-pje`). Controllers **convergem** o real para o desejado (cria/mata Pods, reprograma etc.).

Isso explica **auto-healing** e **escalonamento** sem “SSH manual em 40 VMs”.

### 1.5 Pod — unidade mínima implantável

**Pod** = menor objeto implantável no Kubernetes.

- Pode ter **um ou mais** containers.
- Containers do mesmo Pod compartilham **rede** (mesmo IP / namespace de rede; `localhost` entre si) e **volumes** definidos no Pod.
- Pods são **efêmeros**: morreu → em geral **substituto novo** (novo IP), não “ressuscita o mesmo”.
- Em apps típicos: **1 container principal por Pod**; multi-container = padrões (*sidecar* etc.).

Flash de concurso: a menor unidade **não** é o “container solto”; é o **Pod**.

Fases (visão): `Pending` → `Running` → `Succeeded` / `Failed` (e `Unknown`).

### 1.6 Labels e seletores

| Conceito | Ideia |
|----------|-------|
| **Label** | Par chave/valor no objeto (`app=pedido`, `tier=backend`) |
| **Selector** | Como Services / Deployments **encontram** Pods |

Sem labels/seletores coerentes, Service não “acha” as réplicas. Pegadinha: Service **não** aponta para um IP fixo eterno de um Pod; seleciona Pods vivos pelo label.

### 1.7 ReplicaSet e Deployment

| Objeto | Função |
|--------|--------|
| **ReplicaSet** | Garante N cópias de um template de Pod |
| **Deployment** | Abstração de alto nível: gerencia ReplicaSets; **rolling update**, rollback, escala, histórico |

Hierarquia cobrável:

```
Deployment → ReplicaSet → Pods
```

Na prática de prova: quase sempre fala-se em **Deployment** para app *stateless*; ReplicaSet “por baixo”.

**Rolling update:** troca gradual de Pods da versão antiga pela nova, visando disponibilidade. **Rollback:** voltar revisão anterior se a nova falhar.

### 1.8 Service — IP/DNS estável e balanceamento

Pods mudam de IP. **Service** oferece:

- endpoint **estável** (ClusterIP / DNS interno);
- **balanceamento** entre Pods selecionados.

Tipos (mapa curto):

| Tipo | Uso mental |
|------|------------|
| **ClusterIP** | Acesso **interno** ao cluster (padrão) |
| **NodePort** | Expõe porta em cada nó |
| **LoadBalancer** | Integra balanceador externo (nuvem/infra) |
| **ExternalName** | Alias DNS (caso especial) |

**Ingress** (visão): regras HTTP(S) de entrada (host/path) na borda — **não** confundir Ingress com “todo Service” nem com API Gateway Spring/Zuul do edital (conceito vizinho, stack diferente).

### 1.9 Namespace, ConfigMap, Secret

| Objeto | Função |
|--------|--------|
| **Namespace** | Isolamento **lógico** de recursos (equipes, ambientes `dev`/`hom`/`prod` no mesmo cluster) |
| **ConfigMap** | Configuração **não secreta** desacoplada da imagem |
| **Secret** | Dados sensíveis (senhas, tokens) — ainda exige higiene de RBAC/criptografia em repouso; não é “cofre mágico” |

Flash: Namespace **≠** isolamento de rede automático completo só por existir; é partição lógica de objetos (NetworkPolicy é outro assunto).

### 1.10 StatefulSet e DaemonSet (noção)

| Objeto | Quando a FCC cita |
|--------|-------------------|
| **StatefulSet** | Identidade estável / ordem / storage por réplica (apps *stateful*) |
| **DaemonSet** | **Um Pod por nó** elegível (agente de log, monitoramento, rede) |

Pegadinha clássica: “rodar em **todos** os nós” → pense **DaemonSet**, não Deployment de 1 réplica.

### 1.11 `kubectl` — comandos mentais

Não precisa virar SRE; saiba o **verbo**:

| Ação | Exemplo mental |
|------|----------------|
| Aplicar manifesto | `kubectl apply -f app.yaml` |
| Listar Pods | `kubectl get pods` |
| Descrever | `kubectl describe pod …` |
| Logs | `kubectl logs …` |
| Escala | `kubectl scale deployment … --replicas=5` |

Manifestos declaram `apiVersion`, `kind`, `metadata`, `spec` — ideia YAML declarativa, sem decorar campos exóticos.

### 1.12 Docker × Kubernetes × Rancher (cartão anti-confusão)

| Afirmação | Veredito |
|-----------|----------|
| Docker cria imagens/containers | Correto (visão prova) |
| K8s orquestra Pods no cluster | Correto |
| Rancher gerencia/opera clusters K8s | Correto (visão) |
| K8s = substituto do Keycloak | Errado |
| Pod = menor unidade; container sozinho no modelo K8s | Errado (unidade = Pod) |
| Scheduler executa o container no nó | Errado (escolhe; kubelet/runtime executam) |
| Service “congela” o IP de um Pod eterno | Errado (abstrai sobre Pods selecionados) |

### 1.13 Erros clássicos (caderno)

- Chamar **container** de menor unidade do K8s.  
- Achar que **Deployment** = “um Pod único sem réplicas/controle”.  
- Confundir **Service** com **Ingress** e com **Zuul/API Gateway**.  
- Confundir **K8s** com **Eureka** (discovery Spring) ou **Keycloak** (SSO).  
- Dizer que **scheduler** “sobe o container”; quem garante no nó é o **kubelet** + runtime.  
- Tratar **Namespace** como VPN.  
- Confundir **DaemonSet** (por nó) com **Deployment** (N réplicas no cluster).  
- Achar que **Rancher** substitui a API do Kubernetes no sentido de “não é K8s”.

### 1.14 Cartão-resumo (antes das questões)

| Tema | Âncora |
|------|--------|
| K8s | Orquestração de containers/Pods em cluster |
| Control plane | API Server · etcd · scheduler · controller-manager |
| Nó | kubelet · kube-proxy · runtime · Pods |
| Unidade mínima | **Pod** |
| App stateless | **Deployment** → ReplicaSet → Pods |
| Rede estável | **Service** (+ Ingress na borda HTTP) |
| Config | ConfigMap / Secret · Namespace lógico |
| Por nó | DaemonSet |
| Stack edital | Docker → Kubernetes → Rancher |

---

## 2. Questões de treino (1–15)

**1.** No contexto do edital (Docker, Kubernetes, Rancher), o Kubernetes caracteriza-se principalmente por:

A) Ser o protocolo OAuth 2.0 da PDPJ  
B) Orquestrar implantação, escalonamento e gestão de workloads conteinerizados em cluster  
C) Substituir integralmente o PostgreSQL  
D) Mapear Entity↔DTO em tempo de compilação  
E) Emitir *access tokens* Bearer como IdP  

**2.** A menor unidade implantável no Kubernetes é:

A) A imagem Docker sozinha, sem objeto de runtime no cluster  
B) O **Pod** (um ou mais containers com rede/volumes compartilhados)  
C) O único objeto chamado “RancherSet”  
D) O Eureka Client  
E) O arquivo `pom.xml` do Maven  

**3.** Assinale a associação correta dos componentes do control plane:

A) etcd = escolhe o nó do Pod; scheduler = banco de estado  
B) kube-apiserver = porta de entrada da API; etcd = estado do cluster; scheduler = decide o nó  
C) kubelet = único componente do control plane; etcd = runtime de container  
D) kube-proxy = armazena o estado no etcd diretamente  
E) controller-manager = substituto do Keycloak  

**4.** Quem, em regra, **garante a execução** dos containers descritos no Pod em um worker node?

A) Apenas o kube-scheduler, sem kubelet  
B) O **kubelet** (com o container runtime)  
C) O MapStruct  
D) O Flyway  
E) O Authorization Server OAuth  

**5.** A hierarquia típica para aplicação *stateless* é:

A) Service → etcd → kubelet  
B) **Deployment → ReplicaSet → Pods**  
C) Secret → Ingress → Eureka  
D) Namespace → Docker Hub → Zuul  
E) DaemonSet → Authorization Code → PKCE  

**6.** O objeto **Service** serve principalmente para:

A) Compilar imagens OCI no lugar do Docker  
B) Fornecer identidade/endpoint estável e balancear tráfego entre Pods selecionados por labels  
C) Armazenar o estado do cluster no lugar do etcd  
D) Emitir JWT de SSO  
E) Versionar schema SQL  

**7.** Labels e seletores são importantes porque:

A) Substituem TLS obrigatoriamente  
B) Permitem que Deployments/Services identifiquem conjuntos de Pods  
C) Eliminam a necessidade de API Server  
D) São sinônimos de grants OAuth  
E) Apagam Namespaces automaticamente  

**8.** **Rolling update** em um Deployment significa, em essência:

A) Derrubar o cluster inteiro e reinstalar o SO  
B) Atualizar Pods de forma gradual, buscando manter disponibilidade  
C) Trocar o etcd por H2 embutido  
D) Migrar obrigatoriamente para Implicit grant  
E) Remover todos os Services do Namespace  

**9.** Um **Namespace** no Kubernetes é melhor descrito como:

A) Isolamento lógico de recursos/objetos no cluster  
B) Protocolo de camada física 802.1Q  
C) Banco relacional obrigatório da PDPJ  
D) Substituição do Rancher  
E) Tipo de Service LoadBalancer  

**10.** Para executar **um Pod em cada nó** elegível (ex.: agente de log), o objeto mais adequado é:

A) Deployment com `replicas: 1` apenas  
B) **DaemonSet**  
C) ConfigMap  
D) ClusterIP Service  
E) Job de MapStruct  

**11.** ConfigMap e Secret diferem, na leitura de prova, principalmente porque:

A) ConfigMap só existe no Rancher; Secret só no Docker  
B) ConfigMap carrega config tipicamente não secreta; Secret destina-se a dados sensíveis  
C) Ambos substituem o kube-apiserver  
D) Secret é o único jeito de expor NodePort  
E) ConfigMap escolhe o nó no lugar do scheduler  

**12.** Assinale a linha **correta** no contraste Docker × K8s × Rancher:

A) Docker orquestra o etcd; K8s só gera JWT; Rancher é Eureka  
B) Docker empacota/executa containers; K8s orquestra no cluster; Rancher apoia gestão de clusters K8s  
C) Rancher elimina Pods; Docker é control plane; K8s é só IDE  
D) K8s = Keycloak; Docker = Zuul; Rancher = Flyway  
E) Os três são sinônimos estritos de Service Discovery Spring  

**13.** Sobre o kube-scheduler, é correto afirmar que:

A) Executa diretamente os processos dentro do container no lugar do runtime  
B) Decide/programa em qual nó o Pod deve ser executado  
C) Armazena todo o estado do cluster sem etcd  
D) É o nome interno do Ingress Controller obrigatório  
E) Substitui o PostgreSQL da PDPJ  

**14.** Pods são ditos efêmeros, em regra, porque:

A) Nunca podem ser recriados pelo Deployment  
B) Pods reiniciados/substituídos tipicamente ganham nova identidade/IP; não se depende do IP individual  
C) O Service proíbe qualquer balanceamento  
D) Labels são proibidos em Pods  
E) O etcd apaga Deployments a cada minuto por padrão  

**15.** Qual opção melhor resume o cartão mental deste subtópico?

A) K8s orquestra Pods; control plane (API/etcd/scheduler/controllers) + nós (kubelet/proxy/runtime); Deployment/Service/Namespace; Docker→K8s→Rancher  
B) K8s = SSO Keycloak; Pod = Eureka Server  
C) Scheduler = etcd; Service = MapStruct  
D) DaemonSet = único grant OAuth permitido  
E) Rancher revoga a existência de containers  

---

### Gabarito comentado (treino 1–15)

1. **B** — Orquestração em cluster; não é OAuth/IdP/ORM.  
2. **B** — Unidade mínima = Pod.  
3. **B** — API Server + etcd + scheduler (papéis clássicos).  
4. **B** — kubelet (+ runtime) no nó.  
5. **B** — Deployment → ReplicaSet → Pods.  
6. **B** — Endpoint estável + balanceamento por seletor.  
7. **B** — Labels/seletores amarram objetos a Pods.  
8. **B** — Atualização gradual com disponibilidade.  
9. **A** — Partição lógica de recursos.  
10. **B** — DaemonSet = por nó.  
11. **B** — Config × segredo.  
12. **B** — Camadas Docker / K8s / Rancher.  
13. **B** — Scheduler só decide o nó.  
14. **B** — Efêmeros → não fixar IP de Pod.  
15. **A** — Síntese do arquivo 03.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** Em ambiente de microsserviços conteinerizados alinhado ao conteúdo programático da PDPJ-Br, a adoção do Kubernetes tem por finalidade precípua:

A) substituir o protocolo OAuth 2.0 e eliminar o Keycloak  
B) automatizar a orquestração de containers (implantação, escalonamento e gestão do ciclo de vida) em cluster  
C) versionar exclusivamente scripts DDL no lugar do Flyway  
D) mapear entidades JPA em DTOs em tempo de compilação  
E) atuar como único banco de dados relacional do tribunal  

**17.** Acerca da arquitetura de um cluster Kubernetes, assinale a alternativa correta.

A) O kube-scheduler armazena o estado persistente do cluster no lugar do etcd e executa os containers sem kubelet  
B) O kube-apiserver é a interface central da API; o etcd persiste o estado; o scheduler seleciona o nó para Pods novos; o kubelet no nó assegura a execução dos containers  
C) O kube-proxy é o único componente autorizado a gravar diretamente no etcd  
D) Worker nodes não executam Pods; apenas o control plane hospeda aplicações de negócio  
E) O container runtime é desnecessário quando há Deployment  

**18.** Considere as afirmações abaixo.

I. O Pod é a menor unidade implantável no Kubernetes e pode conter um ou mais containers.  
II. O Deployment gerencia réplicas e possibilita estratégias como *rolling update*.  
III. O Service oferece acesso estável a um conjunto de Pods selecionados, tipicamente por *labels*.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**19.** No contraste entre **Deployment** e **DaemonSet**, é correto afirmar que:

A) ambos existem apenas fora do Kubernetes, no Rancher exclusivo  
B) o Deployment define um número desejado de réplicas no cluster; o DaemonSet visa garantir um Pod por nó elegível  
C) o DaemonSet substitui o etcd; o Deployment emite tokens OAuth  
D) Deployment e DaemonSet são sinônimos estritos de ClusterIP  
E) somente o DaemonSet permite *rolling update* em apps *stateless* típicas  

**20.** Em enunciado que descreve “endereço virtual interno estável e distribuição de requisições entre réplicas de um microsserviço identificadas por rótulos”, o objeto Kubernetes mais adequado é:

A) ConfigMap  
B) Secret  
C) **Service** (ex.: ClusterIP)  
D) Namespace isolando fisicamente o cabo de rede  
E) MapStruct  

**21.** Assinale a opção que **inverte** indevidamente conceitos deste eixo.

A) Pod agrupa containers; Service abstrai IP volátil; Deployment controla réplicas  
B) API Server recebe requisições; etcd guarda estado; kubelet executa no nó  
C) Docker constrói/roda container; Kubernetes orquestra; Rancher apoia gestão de clusters  
D) Kubernetes = Service Discovery Eureka; Pod = Authorization Server; Service = Flyway  
E) Labels permitem seletores de Pods por Deployment/Service  

**22.** A utilização de **ConfigMap** e **Secret** em workloads Kubernetes relaciona-se à ideia de:

A) desacoplar configuração (e dados sensíveis, no caso de Secret) das imagens, injetando-os nos Pods conforme a política do cluster  
B) substituir o kube-scheduler por scripts SQL  
C) registrar instâncias no Eureka a cada *heartbeat* obrigatório do etcd  
D) gerar automaticamente o Swagger UI  
E) tornar desnecessário qualquer container runtime  

**23.** Considere o uso conjugado de **Docker**, **Kubernetes** e **Rancher** no texto do edital. Assinale a alternativa correta.

A) Rancher elimina a necessidade de Pods; Docker é IdP; Kubernetes é apenas IDE  
B) Docker fornece o empacotamento/execução de containers; Kubernetes orquestra no cluster; Rancher figura como plataforma de gestão/operação de Kubernetes  
C) Os três são grants da RFC 6749  
D) Kubernetes substitui JSON; Docker substitui HTTP; Rancher substitui TLS  
E) Apenas o Rancher cria imagens OCI; Kubernetes proíbe containers  

**24.** Para fins de preparação no eixo Kubernetes do edital (PDPJ-Br / Analista TI), a síntese mais adequada é:

A) cluster = control plane + nodes; unidade = Pod; apps *stateless* via Deployment; exposição interna via Service; config via ConfigMap/Secret; gestão complementar via Rancher na trilha Docker→K8s→Rancher  
B) etcd escolhe o nó; scheduler grava o estado; Pod é maior que Namespace  
C) Service Discovery Spring Cloud é idêntico ao kubelet  
D) DaemonSet serve só para emitir Bearer tokens  
E) Namespace é o único objeto que executa containers  

**25.** Assinale a afirmação **incompatível** com o modelo cobrável de Kubernetes.

A) Controllers reconciliam estado desejado e estado atual  
B) Pods são tipicamente efêmeros; Services estabilizam o acesso  
C) O scheduler seleciona nós; o kubelet assegura Pods no nó  
D) A menor unidade implantável é o container isolado, inexistindo o conceito de Pod no Kubernetes  
E) Deployments permitem escalar réplicas e atualizar de forma controlada  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **B** |
| 17 | **B** |
| 18 | **E** |
| 19 | **B** |
| 20 | **C** |
| 21 | **D** |
| 22 | **A** |
| 23 | **B** |
| 24 | **A** |
| 25 | **D** |

**Notas rápidas:** 16 B (orquestração); 17 B (arquitetura clássica); 18 E (I–III corretas); 19 B (réplicas × por nó); 20 C (Service); 21 D (única que **inverte** tudo); 22 A (ConfigMap/Secret); 23 B (Docker→K8s→Rancher); 24 A (síntese); 25 D (nega o Pod — incompatível).

---

### Verificação interna (consistência)

| Checagem | Status |
|----------|--------|
| Edital: Docker · Kubernetes · Rancher · CI/CD (K8s no eixo) | Coberto |
| Arquitetura control plane × nodes | Seções 1.3–1.4 |
| Objetos: Pod, RS, Deployment, Service, NS, ConfigMap, Secret, DS/STS, Ingress noção | Coberto |
| Treino 1–15 com gabarito comentado | 15/15 |
| FCC 16–25 com tabela + notas | 10/10 |
| Distratores (Eureka/Zuul/Keycloak/MapStruct/Flyway; Docker×K8s×Rancher; scheduler×kubelet) | Presentes |
| Gabaritos únicos (sem ambiguidade intencional) | Conferidos |
| Anti-confusão Service × Ingress × Gateway; Pod × container | 1.8, 1.12, 1.13 + Q12/Q21/Q23 |

**Checagem pontual dos gabaritos:** 1B 2B 3B 4B 5B 6B 7B 8B 9A 10B 11B 12B 13B 14B 15A · 16B 17B 18E 19B 20C 21D 22A 23B 24A 25D — alinhados ao material e à documentação oficial de arquitetura/objetos.

---

*Arquivo 03 · Kubernetes · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 3 — containers/orquestração.*

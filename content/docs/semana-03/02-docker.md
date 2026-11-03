+++
title = '02 — Docker'
date = '2026-10-13'
draft = false
weight = 2
tags = ['semana-03', 'docker']
categories = ['estudo']
+++
# Docker · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Docker (Semana 3 · DevOps / stack PDPJ)  
**Escopo deste arquivo:** **Docker** como plataforma de **containers**: imagem × container, contraste com VM, Dockerfile/camadas, registry, ciclo build/run, volumes e redes em visão de prova, encaixe em microsserviços/PDPJ/CI-CD, contraste leve com Kubernetes/Rancher.  
**Fora de escopo aqui:** Kubernetes e Rancher a fundo (outros arquivos da semana 3); RabbitMQ/webhooks; GitLab CI/Jenkins/Maven em profundidade; detalhes de cgroups/namespaces no kernel; segurança OWASP/LGPD (semana própria).

---

## 1. Material

### 1.1 O que é Docker (leitura de prova)

**Docker** é uma plataforma para **empacotar, distribuir e executar** aplicações em **containers**: unidades leves que isolam o processo e suas dependências, compartilhando o **kernel** do sistema operacional host.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | **Empacotar e rodar** apps de forma portável (imagem → container) |
| Unidade | **Container** = processo isolado a partir de uma **imagem** |
| O que **não** é | Orquestrador de cluster (Kubernetes), UI de gestão de cluster (Rancher), VM completa |

Flash: enunciado fala em **container**, **imagem**, **Dockerfile**, **portabilidade**, **isolamento leve**, **empacotar dependências** → **Docker**.

Docker **não** é: Kubernetes, Rancher, Eureka, Zuul, Flyway, Keycloak, RabbitMQ.

### 1.2 Por que containers? (problema cobrável)

Em microsserviços (stack PDPJ), cada serviço tem runtime, libs e config. Sem container:

1. “Funciona na minha máquina” — ambientes **divergem**.  
2. Dependências do SO host **contaminam** a app.  
3. Deploy manual é **lento** e pouco repetível.  
4. Escala e CI/CD ficam **opacas** (artefato ≠ ambiente).

Docker trata a app + dependências como **artefato empacotável** (imagem), executável de forma **repetível** em qualquer host com runtime Docker compatível.

### 1.3 Container × máquina virtual (tabela ouro)

A FCC adora o contraste **container × VM**:

| Aspecto | **Máquina virtual (VM)** | **Container (Docker)** |
|---------|--------------------------|-------------------------|
| Isolamento | Hipervisor + **SO convidado completo** | Isolamento de processo (namespaces/cgroups); **compartilha kernel** do host |
| Peso | Mais pesada (boot de SO) | Mais **leve** e rápida de subir |
| Densidade | Menos VMs por host (tipicamente) | Mais containers por host |
| Artefato | Disco/imagem de VM | **Imagem** de container |
| Uso típico de prova | Isolar SO inteiro | Empacotar **aplicação** e deps |

Frase de prova: **container ≠ VM**; container é mais leve porque **não leva SO completo** — compartilha o kernel do host.

Armadilha: dizer que Docker “é uma máquina virtual” ou que “cada container tem seu próprio kernel completo”.

### 1.4 Imagem × container (anti-confusão ouro)

| | **Imagem** | **Container** |
|---|------------|---------------|
| Analogia | “Classe” / template / receita congelada | “Instância” em execução (ou criada a partir da imagem) |
| Mutabilidade (visão de prova) | Em regra **imutável** (camadas); novas mudanças → nova imagem/tag | Estado de **execução** (processo, filesystem writable) |
| Comando mental | `docker build` / `docker pull` | `docker run` / `docker start` |
| Persistência | Guardada no disco/registry | Pode ser removida; dados voláteis somem sem volume |

Flash:

```
Dockerfile  →  docker build  →  Imagem  →  docker run  →  Container
                                      ↑
                               docker pull (registry)
```

Erro clássico: tratar “imagem” e “container” como sinônimos.

### 1.5 Dockerfile e camadas (visão cobrável)

**Dockerfile** = arquivo de instruções para **construir** uma imagem (FROM, RUN, COPY/ADD, ENV, EXPOSE, CMD/ENTRYPOINT — nomes para reconhecer, não tutorial).

Pontos de prova:

| Conceito | Leitura |
|----------|---------|
| **FROM** | Imagem base (ex.: `eclipse-temurin`, `alpine`) |
| **Camadas (layers)** | Cada instrução relevante gera camada; cache acelera rebuild |
| **Build** | `docker build` produz a imagem |
| **Tag** | Nome:versão (ex.: `meu-servico:1.2.0`) para versionar artefato |
| **Imagem imutável (ideia)** | Em pipeline maduro, o que sobe em prod é a **mesma** imagem testada |

Não precisa decorar sintaxe linha a linha; precisa saber que o Dockerfile **define** como a imagem é montada.

### 1.6 Registry, Hub e ciclo pull/push

| Peça | Papel |
|------|-------|
| **Registry** | Repositório de imagens (local ou remoto) |
| **Docker Hub** | Registry público clássico citado em provas |
| **pull** | Baixar imagem |
| **push** | Enviar imagem (após build/login) |
| **CI/CD** | Pipeline builda imagem → push no registry → ambiente puxa e sobe container |

Encaixe mental com edital: **Git** versiona código; **Docker** empacota runtime; **CI/CD** automatiza build/teste/deploy da imagem; **Kubernetes/Rancher** orquestram/gerem em escala (outros arquivos).

### 1.7 Comandos e ciclo de vida (flash, sem memorizar flags)

Visão de prova — reconhecer o **papel**, não a man page:

| Ação | Ideia |
|------|-------|
| build | Criar imagem a partir do Dockerfile |
| run | Criar e iniciar container a partir da imagem |
| ps / ps -a | Listar containers (em execução / todos) |
| stop / start / restart | Parar / iniciar / reiniciar |
| rm / rmi | Remover container / remover imagem |
| logs | Ver saída do processo no container |
| exec | Executar comando **dentro** de container já rodando |
| pull / push | Baixar / enviar imagem no registry |

Armadilha: achar que `docker build` **executa** a aplicação em produção — build gera **imagem**; `run` (ou orquestrador) **executa**.

### 1.8 Volumes e redes (margem de segurança)

**Volumes** (visão cobrável):

- Container tem filesystem **efêmero** por padrão: removeu o container → dados internos somem.  
- **Volume** (ou bind mount) **persiste** dados fora do ciclo de vida do container (ex.: dados de PostgreSQL).  
- Flash: “banco em container sem volume” = risco de **perder dados** ao recriar o container.

**Redes** (visão cobrável):

- Containers em mesma rede Docker podem se comunicar por **nome** (DNS interno do Docker).  
- Publicar porta (`-p host:container`) expõe serviço ao host/rede externa.  
- Isolamento de rede é **relativo** (não confundir com firewall corporativo completo).

Não aprofundar drivers de rede; basta: **isolamento + comunicação + publicação de porta**.

### 1.9 Docker Compose (noção cobrável)

**Docker Compose** = definir e subir **vários** containers como um conjunto (arquivo YAML típico: serviços, redes, volumes).

Leitura de prova:

| Sem Compose | Com Compose (ideia) |
|-------------|---------------------|
| Vários `docker run` manuais | Um manifesto descreve app + BD + fila etc. |
| Ambiente local/homolog “na mão” | `up` / `down` sobem/derrubam o **stack** |

Compose **não** substitui Kubernetes em cluster de produção em escala — é ferramenta de **definição multi-container** (muito usada em dev/homolog; enunciado pode citar “orquestração simples” sem confundir com K8s).

### 1.10 Docker na stack PDPJ / microsserviços

Edital (Arquitetura PDPJ-Br) cita, lado a lado: **Git; Docker; Kubernetes; Rancher; CI/CD** (+ microsserviços, Spring, etc.).

Leitura cobrável do “kit empacotamento/deploy”:

```
Código (Git)
    │
    ▼  CI (build/test)
 Dockerfile → Imagem Docker → Registry
    │
    ▼
 Container(s) no host  ···  ou  ···  Cluster (Kubernetes) gerido (Rancher)
    │
 Serviço Spring (API REST/JSON) + deps (JDK, etc.) isolados na imagem
```

| Peça | Papel |
|------|-------|
| **Git** | Versiona código (e Dockerfile) |
| **Docker** | Empacota e executa em **container** |
| **Kubernetes** | Orquestra containers em **cluster** (escala, health, rede de serviço…) |
| **Rancher** | Camada de **gestão/UI** sobre clusters (visão) |
| **CI/CD** | Automatiza build da imagem e promoção entre ambientes |

Docker **não** substitui Eureka (discovery de app Spring), Zuul/gateway, Flyway, Keycloak nem RabbitMQ — **empacota** o runtime em que esses componentes podem rodar.

### 1.11 Docker × Kubernetes × Rancher (anti-confusão)

| | **Docker** | **Kubernetes (K8s)** | **Rancher** |
|---|------------|----------------------|-------------|
| Pergunta | Como **empacotar/rodar** um container? | Como **orquestrar** muitos containers no cluster? | Como **gerir/operar** clusters (visão de gestão)? |
| Unidade mental | Imagem / container | Pod, Deployment, Service… | Cluster / projeto / UI operacional |
| Erro FCC | Chamar Docker de “orquestrador de cluster” | Chamar K8s de “só um Dockerfile” | Chamar Rancher de “runtime do container” |

Frase de prova: **Docker empacota; Kubernetes orquestra; Rancher facilita a gestão do cluster.**

### 1.12 Benefícios e limites (o que a FCC testa)

**Benefícios típicos:**

1. **Portabilidade** — mesma imagem em dev/homolog/prod.  
2. **Consistência** — dependências fixadas na imagem.  
3. **Densidade / leveza** vs VM.  
4. **Velocidade** de subir ambiente.  
5. Encaixe natural com **CI/CD** e microsserviços (um serviço ≈ uma imagem).

**Limites / cuidados cobráveis:**

1. Isolamento **não** é o mesmo de VM completa.  
2. Dados efêmeros sem **volume**.  
3. Imagens mal construídas (secrets no Dockerfile, root desnecessário) — noção DevSecOps.  
4. Um host Docker sozinho **não** resolve alta disponibilidade de cluster — aí entra **orquestração** (K8s).  
5. Container **não** substitui boa arquitetura (API, persistência, auth).

### 1.13 O que Docker **não** faz (lista seca)

1. Não é orquestrador de cluster (Kubernetes).  
2. Não é Rancher / painel de gestão de cluster.  
3. Não é Service Discovery Spring (Eureka).  
4. Não é API Gateway / Zuul.  
5. Não versiona schema de BD (Flyway).  
6. Não é SSO/OAuth/Keycloak.  
7. Não é broker de mensageria (RabbitMQ).  
8. Não elimina a necessidade de testar a aplicação.  
9. Não é “microsserviço” por si só — microsserviço é estilo arquitetural; Docker é **empacotamento**.

### 1.14 Armadilhas FCC (lista de ouro)

1. Tratar container e VM como iguais.  
2. Confundir **imagem** com **container**.  
3. Dizer que Docker orquestra cluster no lugar do Kubernetes.  
4. Dizer que Rancher é o runtime que substitui o Docker Engine.  
5. Afirmar que Docker = Eureka / Zuul / Swagger.  
6. Achar que todo dado do container é persistente sem volume.  
7. Confundir `build` (imagem) com `run` (execução).  
8. Reduzir Docker a “só desenvolvimento local” — também é artefato de **deploy**.  
9. Confundir Docker Compose com Kubernetes.  
10. Inventar que Docker substitui Git ou CI/CD (são complementares).

### 1.15 Mini-síntese (cartão mental)

> **Docker** = **imagens** + **containers** para empacotar e executar apps de forma **portável** e **leve** (kernel compartilhado ≠ VM).  
> **Dockerfile** constrói a imagem; **registry** distribui; **run** executa; **volume** persiste dados.  
> Na PDPJ/edital: Docker no kit com **Git, K8s, Rancher, CI/CD** — papéis **complementares**.  
> **Não** confundir empacotamento (Docker) com orquestração (Kubernetes) nem com gestão de cluster (Rancher).

---

## 2. Questões de treino (1–15)

**1.** No eixo de empacotamento/deploy da stack PDPJ citada no edital, o Docker desempenha prioritariamente o papel de:

A) Service Discovery / registry de instâncias Spring  
B) Plataforma para empacotar e executar aplicações em containers  
C) Ferramenta de migração versionada de schema (DDL)  
D) Broker de mensagens AMQP  
E) Provedor SSO/OAuth2  

**2.** A principal diferença cobrável entre container Docker e máquina virtual é que o container:

A) Sempre inclui um hipervisor e um SO convidado completo por app  
B) Isola o processo compartilhando o **kernel** do host, sendo tipicamente mais leve que a VM  
C) Substitui obrigatoriamente o Kubernetes em qualquer cluster  
D) É sinônimo estrito de imagem imutável no registry  
E) Elimina a necessidade de CI/CD  

**3.** Assinale a associação **correta**:

A) Imagem = processo em execução; container = template no registry  
B) Imagem = artefato/template; container = instância criada a partir da imagem  
C) Imagem = Eureka; container = Zuul  
D) Imagem e container são sempre o mesmo conceito na FCC  
E) Container = Dockerfile; imagem = apenas o comando `logs`  

**4.** Em enunciado que fala em arquivo de instruções FROM/RUN/COPY para gerar artefato executável empacotado, a peça alinhada é:

A) flyway_schema_history  
B) Dockerfile (construção da imagem)  
C) application.yml do Eureka Server  
D) swagger.json exclusivamente  
E) token OAuth2  

**5.** Relativamente a volumes em Docker, é correto afirmar que:

A) Todo arquivo escrito no container persiste eternamente após `docker rm`  
B) Volumes ajudam a **persistir** dados além do ciclo de vida efêmero do container  
C) Volume é o nome do orquestrador Kubernetes  
D) Volume substitui o registry de imagens  
E) Volume é obrigatoriamente o Rancher  

**6.** Qual afirmação é **incorreta**?

A) `docker build` produz uma imagem a partir do Dockerfile  
B) `docker run` cria/inicia um container a partir de uma imagem  
C) Docker, por definição, é o Service Discovery da stack Spring Cloud Netflix  
D) Imagens podem ser enviadas/baixadas de um registry (`push`/`pull`)  
E) Containers compartilham o kernel do host  

**7.** Docker e Kubernetes convivem na leitura de edital porque:

A) São mutuamente excludentes  
B) Docker enfatiza **empacotar/rodar** containers; Kubernetes **orquestra** containers em cluster  
C) Kubernetes gera apenas Dockerfiles; Docker gerencia Pods  
D) Ambos são apenas nomes comerciais do Flyway  
E) Rancher elimina qualquer noção de imagem  

**8.** “Ferramenta/plataforma associada a imagens, containers e Dockerfile para empacotar aplicações de forma portável.” Descreve:

A) Eureka  
B) Zuul  
C) Docker  
D) MapStruct  
E) Hibernate Envers  

**9.** Usar Docker em microsserviços Spring ajuda sobretudo a:

A) Eliminar a necessidade de JSON na API  
B) Empacotar cada serviço com suas dependências de forma **repetível** entre ambientes  
C) Substituir o API Gateway  
D) Documentar OpenAPI no lugar do Swagger  
E) Versionar schema no lugar do Flyway  

**10.** Sobre Docker Compose, a leitura cobrável mais segura é:

A) É o hipervisor que substitui o kernel Linux  
B) Permite definir e subir um **conjunto** de containers (stack) a partir de manifesto  
C) É o único orquestrador de produção em cluster massivo citado como sinônimo de K8s  
D) Substitui Git no versionamento de código  
E) É o broker RabbitMQ  

**11.** Docker **não** se destina a:

A) Empacotar aplicação e dependências em imagem  
B) Executar processos isolados como containers  
C) Atuar como catálogo dinâmico de instâncias Spring (papel do Eureka)  
D) Facilitar portabilidade entre ambientes  
E) Integrar-se a pipelines CI/CD que buildam e publicam imagens  

**12.** Em prova, confundir Docker com Rancher costuma errar porque:

A) Não há diferença cobrável  
B) Docker centra-se no runtime/empacotamento de containers; Rancher associa-se à **gestão** de clusters (visão)  
C) Rancher é o único que cria imagens  
D) Docker só existe sem registry  
E) Rancher é o nome antigo do Dockerfile  

**13.** A frase mais alinhada ao papel do Docker na stack PDPJ é:

A) “Aplica migrations V1/V2 no PostgreSQL”  
B) “Empacota o serviço em imagem/container para execução portável, alinhada a Git/CI/CD e orquestração”  
C) “É o dialect JPA do Hibernate 5”  
D) “Substitui OAuth2 e Keycloak”  
E) “Só funciona se houver Zuul na frente”  

**14.** Qual linha resume melhor o cartão mental deste subtópico?

A) Docker = discovery; K8s = mapper; Rancher = gateway  
B) Docker = imagens/containers (empacotar/rodar); K8s = orquestração; Rancher = gestão de cluster (visão)  
C) Docker elimina a necessidade de testes e de Git  
D) Docker = Eureka + Zuul  
E) Docker = apenas backup full do PostgreSQL  

**15.** Afirmar que “Docker é uma VM completa com SO convidado por container” é:

A) Correto em toda prova FCC  
B) Incorreto: container compartilha o kernel do host e é tipicamente mais leve que VM  
C) Correto somente com H2  
D) Correto porque Docker é Spring Cloud Netflix  
E) Correto se o gateway for Zuul  

---

### Gabarito comentado (treino 1–15)

1. **B** — Empacotar/executar em containers.  
2. **B** — Kernel compartilhado; leveza vs VM.  
3. **B** — Imagem = template; container = instância.  
4. **B** — Dockerfile constrói a imagem.  
5. **B** — Volume persiste além do ciclo do container.  
6. **C** — Discovery = Eureka, não Docker.  
7. **B** — Empacotar × orquestrar.  
8. **C** — Definição cobrável de Docker.  
9. **B** — Empacote repetível por serviço/ambiente.  
10. **B** — Manifesto multi-container / stack.  
11. **C** — Catálogo = Eureka.  
12. **B** — Runtime/empacote × gestão de cluster.  
13. **B** — Imagem/container + Git/CI/CD/orquestração.  
14. **B** — Síntese Docker × K8s × Rancher.  
15. **B** — Container ≠ VM completa.

---

## 3. Questões de concurso (reais) (16–25)

> Questões reais de provas (preferência FCC/TRT Analista TI). Gabarito só ao final.

**16.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Durante o deploy de uma aplicação com Docker, os logs mostram que o contêiner não inicia devido à falta de permissões no sistema de arquivos. Nesse caso, a solução mais apropriada é

A) reiniciar o Docker Daemon no servidor.  
B) configurar permissões adequadas no host para o volume montado.  
C) reconstruir a imagem Docker com um novo usuário root.  
D) alterar o diretório de logs para uma pasta temporária.  
E) usar o comando `docker exec` para corrigir permissões dentro do contêiner.

**17.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
A fim de utilizar o Docker em sua organização, um Analista necessitou conhecer os principais componentes dessa plataforma, tais como:  
I. Software que roda na máquina onde o Docker está instalado. Recebe comandos do cliente a partir de Command Line Interfaces ou API's REST.  
II. Mecanismo usado para criar imagens e containers.  
III. Coleção de imagens hospedadas e rotuladas que juntas permitem a criação do sistema de arquivos de um container. Pode ser público ou privado.  
IV. Repositório usado para hospedar e baixar diversas imagens. Pode ser visto como uma plataforma de Software as a Service (SaaS) de compartilhamento e gerenciamento de imagens.  
Os itens de I a IV correspondem, correta e respectivamente, a Docker

A) Conteiner − Compose − Swarm − Image.  
B) Daemon − Image − Container − Registry.  
C) Engine − Swarm − Hub − Registry.  
D) Daemon − Engine − Registry − Hub.  
E) Conteiner − Engine − Compose − Hub.

**18.** (FCC – TJ-CE/2026 – Analista Judiciário – Infraestrutura de TI)  
Uma equipe de infraestrutura precisava construir uma imagem Docker baseada na imagem oficial Ubuntu 24,04. O arquivo app.sh deveria ser copiado para /opt/app/app.sh e configurado como o comando executado por padrão quando o contêiner fosse iniciado, podendo ser substituído por outro comando informado no momento da execução do contêiner. Atende corretamente a esses requisitos:

A)
```
FROM ubuntu:24.04
COPY app.sh /opt/app/app.sh
CMD ["/opt/app/app.sh"]
```
B)
```
FROM ubuntu:24.04
COPY app.sh /opt/app/app.sh
ENTRYPOINT ["/opt/app"]
```
C)
```
FROM ubuntu:24.04
COPY app.sh /opt/app/app.sh
RUN ["/opt/app/app.sh"]
```
D)
```
FROM ubuntu:24.04
ADD app.sh /opt/app/app.sh
RUN ["/opt/app/app.sh"]
```
E)
```
FROM ubuntu:24.04
RUN cp app.sh /opt/app/app.sh
ENTRYPOINT ["/opt/app/app.sh"]
```

**19.** (FCC – TRT-22/2022 – Técnico Judiciário – TI)  
O Docker tornou muito mais fácil para os desenvolvedores entender e usar a tecnologia de contêineres. Para isso, oferece diversos recursos ou ferramentas, dentre as quais encontram-se:  
I. Ferramenta de clustering e scheduling para contêineres do Docker, que permite que os administradores e desenvolvedores de TI possam estabelecer e gerenciar um cluster de nós do Docker como um único sistema virtual.  
II. Reúne instruções necessárias para construir uma imagem de contêiner.  
III. Aplicativo para plataforma Mac ou Windows que permite criar e compartilhar microsserviços e containerized applications. Inclui diversas ferramentas como o cliente Docker, o Docker Compose, o Docker Content Trust, o Kubernetes e o Credential Helper.  
Os itens I, II e III correspondem, correta e respectivamente, a

A) Docherd – Docker Compose – Docker Hub.  
B) Docker Engine – Docker Compose – Docker Desktop.  
C) Docker Build – Docherd – Docker Swam.  
D) Docker Engine – Dockerfile – Docker Hub.  
E) Docker Swarm – Dockerfile – Docker Desktop.

**20.** (VUNESP – TJ-RS/2023 – Analista de TI – Análise de Sistemas)  
Deseja-se executar um container Docker no qual um diretório específico dentro do container seja mapeado em um diretório específico da máquina host. Esse diretório do host deve ser especificado pelo usuário do Docker por meio de seu caminho absoluto no comando de execução do container. A solução para esse problema consiste em utilizar

A) uma imagem.  
B) um volume.  
C) um bind mount.  
D) um tmpfs mount.  
E) uma rede virtual.

**21.** (FGV – MPE-ES/2026 – Analista de Infraestrutura)  
Considerando o uso da tecnologia Docker no contexto da virtualização por contêineres, assinale a afirmativa correta.

A) Bind mounts são gerenciados pelo Docker de forma independente da estrutura de diretórios do sistema hospedeiro.  
B) Os contêineres Docker permitem executar aplicações em contêineres que compartilham o kernel do sistema operacional do host, tornando sua execução mais leve que máquinas virtuais.  
C) O uso de mecanismos de armazenamento de dados compartilhados elimina o isolamento entre contêineres, fazendo com que passem a funcionar como uma única unidade.  
D) Os contêineres Docker não permitem o compartilhamento de dados entre si, em razão do isolamento proporcionado pelo sistema operacional.  
E) Os dados armazenados em contêineres Docker são automaticamente mantidos, independentemente de qualquer configuração adicional, mesmo após o encerramento do contêiner.

**22.** (FGV – TJ-RJ/2026 – Analista de Infraestrutura de TIC)  
No contexto dos conceitos de Docker e Kubernetes, é correto afirmar que:

A) o Kubernetes é utilizado exclusivamente na etapa de desenvolvimento para a construção das imagens, cabendo ao Docker a responsabilidade pelo balanceamento de carga em produção;  
B) eles são tecnologias mutuamente excludentes, uma vez que a adoção do orquestrador Kubernetes elimina, por definição, a necessidade de qualquer mecanismo de empacotamento de contêineres;  
C) o Docker atua como um hipervisor de tipo 1 para virtualizar o hardware físico, enquanto o Kubernetes opera como o sistema operacional convidado responsável pela execução das aplicações;  
D) o Docker provê os mecanismos para criação e execução de contêineres individuais, enquanto o Kubernetes gerencia a orquestração, o escalonamento e a resiliência desses contêineres em cluster;  
E) o Kubernetes substitui integralmente a necessidade de um container runtime subjacente, executando os processos da aplicação diretamente sobre o processador do servidor sem camadas intermediárias.

**23.** (FGV – TJ-SC/2026 – Analista de Sistemas)  
No contexto de desenvolvimento de software em nuvem e uso de containers, considere os itens a seguir.  
I. Recurso que pode ser compartilhado pelos containers com o sistema operacional do hospedeiro, mas não pelas máquinas virtuais.  
II. Orquestrador de containers.  
III. Novo modelo de desenvolvimento em que as aplicações são feitas para a nuvem, e não adaptadas a ela.  
Assinale a opção que apresenta, respectivamente, os recursos ou conceitos listados.

A) Kernel, Docker, IaaS.  
B) Cluster, Node, Microserviços.  
C) Kernel, Kubernetes, Cloud Native.  
D) Hardware, Hypervisor, Cluster.  
E) Sistema de arquivos base, Kubernetes, DevOps.

**24.** (FCC – TRT-17/2022 – Técnico Judiciário – TI)  
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

**25.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Nos projetos da Plataforma Digital do Poder Judiciário PDPJ-Br,

A) os ambientes de deployment estão instalados em clusters Kubernetes, gerenciados por meio da ferramenta open source de orquestração de containers Rancher.  
B) a aplicação deve possuir os arquivos `yanl`, para que o deploy ocorra de forma automática por meio de pipelines de CI/CD executados pelo GitLab do STF.  
C) a aplicação deve possuir um arquivo `Rancherfile`, especificando o modo pelo qual ela deve ser empacotada.  
D) deve haver o arquivo `.gitlab-ci.xml` definindo a pipeline de deploy automático.  
E) deve haver, na pasta `kubernets`, o arquivo de configuração do ingress da aplicação, que é obrigatório.

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **B** | FCC TRT-1/2025 — permissões do volume no host |
| 17 | **D** | FCC TRT-19/2022 — Daemon / Engine / Registry / Hub |
| 18 | **A** | FCC TJ-CE/2026 — CMD (substituível no `docker run`) |
| 19 | **E** | FCC TRT-22/2022 — Swarm / Dockerfile / Desktop |
| 20 | **C** | VUNESP TJ-RS/2023 — bind mount (caminho absoluto do host) |
| 21 | **B** | FGV MPE-ES/2026 — containers compartilham kernel |
| 22 | **D** | FGV TJ-RJ/2026 — Docker empacota; K8s orquestra |
| 23 | **C** | FGV TJ-SC/2026 — Kernel / Kubernetes / Cloud Native |
| 24 | **E** | FCC TRT-17/2022 — K8s / CDN / Docker |
| 25 | **A** | FCC TRT-19/2022 — K8s + Rancher na PDPJ (contexto Docker/deploy) |

**Notas rápidas:** 16 B (perms no host). 17 D (Daemon–Engine–Registry–Hub). 18 A (CMD). 19 E (Swarm/Dockerfile/Desktop). 20 C (bind mount). 21 B (kernel). 22 D (empacotar × orquestrar). 23 C (Kernel/K8s/Cloud Native). 24 E (K8s/CDN/Docker). 25 A (PDPJ: K8s+Rancher).

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **imagem / container / Dockerfile / registry / portabilidade leve**? → Docker.  
2. É **orquestração de cluster / Pod / escala de muitos containers**? → Kubernetes.  
3. É **gestão/UI de clusters**? → Rancher (visão).  
4. É **onde está a réplica Spring / heartbeat**? → Eureka.  
5. É **entrada/rota na borda**? → Gateway / Zuul.  
6. É **schema / migration DDL**? → Flyway.  
7. É **VM com SO convidado completo**? → **não** é o modelo mental do container.

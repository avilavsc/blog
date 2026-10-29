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

## 3. Questões estilo concurso FCC (16–25)

**16.** Em aplicações alinhadas à stack referida no contexto PDPJ-Br, o Docker caracteriza-se, em regra, por:

A) funcionar como catálogo exclusivo de registro e heartbeat de réplicas Spring  
B) permitir o empacotamento e a execução de aplicações em containers a partir de imagens  
C) expor, por si só, a documentação interativa OpenAPI da API  
D) gerar, na compilação, mapeamentos tipados Entity ↔ DTO  
E) constituir o mecanismo SSO/OAuth2 da plataforma  

**17.** Relativamente à relação entre imagem e container, assinale a opção correta:

A) A existência de uma imagem torna impossível criar mais de um container  
B) A imagem é o artefato modelo; o container é a instância em execução (ou criada) a partir dela  
C) Container e imagem são nomes comerciais do Eureka  
D) Docker obriga um único container global compartilhado por todos os microsserviços do país  
E) `docker pull` executa obrigatoriamente a aplicação em produção sem imagem  

**18.** Comparando containers Docker com máquinas virtuais, a vantagem tipicamente cobrada dos containers é:

A) eliminar a necessidade de qualquer isolamento  
B) menor sobrecarga relativa ao empacotar a aplicação compartilhando o kernel do host  
C) substituir o Service Discovery  
D) documentar try-it automaticamente no lugar do Swagger  
E) registrar a aplicação no Zuul como filtro error  

**19.** Assinale a alternativa que apresenta associação **incorreta**:

A) Docker — empacotamento/execução em containers  
B) Kubernetes — orquestração de containers em cluster  
C) Rancher — gestão/operação de clusters (visão)  
D) Docker — ponto único de entrada e roteamento de borda (papel de API Gateway)  
E) Dockerfile — instruções para construir imagem  

**20.** Em enunciado que contraste ferramentas do kit Git/Docker/Kubernetes/Rancher/CI-CD, a FCC tipicamente considera **correto** que:

A) Rancher aplica V1/V2 de DDL e Docker audita Entities  
B) Docker ocupa-se de imagens/containers, ao passo que Kubernetes orquestra a execução em escala no cluster  
C) Docker e Kubernetes são intercambiáveis em qualquer item  
D) Docker aplica rate limit na borda HTTP  
E) Kubernetes realiza apenas `docker logs` no notebook do desenvolvedor  

**21.** A expressão “artefato imutável (visão de prova) construído a partir de um Dockerfile e armazenável em registry” descreve com mais precisão:

A) Eureka Server  
B) Zuul filter post  
C) Imagem Docker  
D) MapStruct  
E) Swagger UI exclusivamente  

**22.** Em arquitetura de microsserviços com um container por serviço, o uso de Docker **não** implica, por si só:

A) possibilidade de empacotar dependências com a aplicação  
B) substituição do API Gateway e do Service Discovery  
C) execução isolada relativa no host  
D) publicação da imagem em registry para promoção entre ambientes  
E) alinhamento com pipelines que fazem build e deploy da mesma imagem  

**23.** Considere as afirmações:

I. Docker documenta schemas OpenAPI no navegador (try-it).  
II. Docker empacota aplicações em imagens executáveis como containers.  
III. Kubernetes não elimina o conceito de imagem/container; orquestra esses artefatos em cluster.

Assinale:

A) Apenas I  
B) Apenas II  
C) II e III  
D) I e II  
E) I, II e III  

**24.** Diferenciar Docker de Kubernetes é relevante em provas porque:

A) não há qualquer diferença conceitual cobrável  
B) o Docker enfatiza empacotamento/runtime de containers; o Kubernetes, orquestração em cluster  
C) Docker só existe fora do ecossistema de microsserviços  
D) orquestração de cluster é exclusividade do Eureka  
E) Docker é apenas o nome antigo do MapStruct  

**25.** A síntese mais alinhada ao edital, sobre Docker, é:

A) Docker = discovery; Kubernetes = gateway; Rancher = docs  
B) Docker = imagens/containers para empacotar e executar; complementa Git/CI-CD e distingue-se de Kubernetes (orquestração) e Rancher (gestão de cluster)  
C) Docker obriga expor Entity no JSON e elimina DTO  
D) Docker substitui Zuul, Eureka e OpenAPI  
E) Docker é o banco H2 do Spring Boot  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Imagens → containers.  
17. **B** — Artefato × instância.  
18. **B** — Leveza / kernel compartilhado.  
19. **D** — Gateway ≠ Docker.  
20. **B** — Empacotar × orquestrar.  
21. **C** — Imagem (Dockerfile + registry).  
22. **B** — Não substitui Gateway/Discovery.  
23. **C** — I falsa (Swagger); II e III verdadeiras.  
24. **B** — Empacote/runtime × orquestração.  
25. **B** — Cartão mental completo do arquivo 02.

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

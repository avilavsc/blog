+++
title = '04 — Inglês técnico · Vocabulário FCC de TI'
date = '2026-10-29'
draft = false
weight = 4
tags = ['semana-10', 'ingles']
categories = ['estudo']
+++
# Inglês técnico · Vocabulário FCC de TI · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — Inglês técnico (vocabulário FCC)  
**Escopo deste arquivo:** Léxico cobrável de TI em inglês (redes, segurança, banco, nuvem, SDLC/DevOps) no formato típico FCC: definição de termo, texto NIST/manual adaptado, QoS/throughput/jitter, IaaS/PaaS/SaaS, TLS/SSL/VPN, firewall/NGFW. Leitura mínima para achar o termo-chave — **não** interpretação literária.  
**Fora de escopo aqui:** Interpretação de texto literário/jornalístico; gramática avançada por si só; MoReq/PDTIC; legado VB/Delphi/JSF (arquivo 05); Selenium/Maven (arquivo 06).

---

## 1. Material

### 1.1 Como a FCC cobra “Inglês técnico”

No edital aparece só **Inglês técnico**. Em provas recentes de Analista TI, isso vira:

| Formato | O que fazer |
|---------|-------------|
| Texto em inglês (NIST, manual, livro técnico) + pergunta | Achar o **conceito** (IaaS, firewall, network policy…) |
| Termo inglês no enunciado PT + definição | Traduzir o **significado operacional** (throughput ≠ jitter) |
| Sigla + frase em inglês | Mapear IaaS/PaaS/SaaS, TLS vs SSL, QoS metrics |

Flash: não precisa “passar em TOEIC”; precisa reconhecer **vocabulário de edital** embutido em frases curtas.

### 1.2 Redes — cartão de prova

| Inglês | Português cobrável | Armadilha |
|--------|--------------------|-----------|
| **bandwidth** | capacidade / largura de banda (capacidade do canal) | Confundir com *throughput* (vazão efetiva) |
| **throughput** | vazão / taxa efetiva de transferência | “Variação de banda” (errado) |
| **latency / delay / retardo** | atraso ponta a ponta | Não é jitter |
| **jitter** | **variação** de latência | “Latência máxima” / “variação de timestamp entre bandas” |
| **packet loss** | perda de pacotes | |
| **QoS** (*Quality of Service*) | qualidade de serviço previsível (throughput, jitter, delay, loss…) | |
| **router** | roteador (L3) | ≠ *switch* (L2 tipicamente) |
| **switch** | comutador | |
| **gateway** | porta de saída / interconexão de redes | ≠ *firewall* (embora FW possa ser gateway) |
| **firewall** | controle de tráfego conforme política | |
| **packet filter** | filtro de pacotes (IP/porta/direção; L3/L4) | ≠ UTM/SIEM/IDS |
| **NGFW** (*Next-Generation Firewall*) | FW + inspeção profunda / comportamento / app | |
| **WAF** | *Web Application Firewall* (HTTP/HTTPS) | ≠ NGFW genérico |
| **VLAN** | rede lógica segmentada | |
| **VPN** | rede privada virtual | SSL VPN no browser → **TLS** (sucessor do SSL) |

Anti-confusão ouro (TRT-4/FCC):

> **Throughput** = vazão de dados. **Jitter** = variação de latência. **Não** inventar “throughput de banda = variação…”.

Unidade: **Mbps** = megabits/s; **MB** = megabytes. 1 byte = 8 bits → tempo ≈ `(MB × 8) / Mbps`.

### 1.3 Segurança — léxico mínimo

| Inglês | Ideia FCC |
|--------|-----------|
| **encryption / decrypt** | criptografar / decifrar — confidencialidade |
| **authentication** | autenticar (quem é) |
| **authorization** | autorizar (o que pode) |
| **TLS** | *Transport Layer Security* — sucessor do SSL; browser/SSL VPN |
| **SSL** | legado; em enunciados modernos “SSL or **TLS**” |
| **handshake protocol** | autentica partes, negocia crypto, troca chaves |
| **record protocol** | divide tráfego em registros protegidos com chaves de sessão |
| **malware / threat / vulnerability** | malware / ameaça / vulnerabilidade |
| **deep packet inspection (DPI)** | inspeção profunda — diferencial típico de **NGFW** |
| **deploy** (segurança) | implantar controle/appliance “at the forefront” / entre componentes |

Flash TLS (duas peças):

| Protocolo | Função |
|-----------|--------|
| **Record** | protege registros do tráfego |
| **Handshake** | autentica + negocia parâmetros + chaves |

### 1.4 Banco de dados — termos que aparecem em inglês

| Inglês | Português / nota |
|--------|------------------|
| **query** | consulta |
| **transaction** | transação |
| **commit / rollback** | confirmar / desfazer |
| **ACID** | Atomicity, Consistency, Isolation, Durability |
| **index** | índice |
| **schema** | esquema |
| **backup / restore** | cópia / restauração |
| **replication** | replicação |
| **stored procedure** | procedimento armazenado |
| **ORM** | *Object-Relational Mapping* |

Em prova mista: enunciado PT + termo EN (*ResultSet*, *commit*, *schema*) — trate o termo como **âncora**, não como “prova de inglês literário”.

### 1.5 Cloud (NIST SP 800-145) — inglês que a FCC cola

Cinco **essential characteristics**:

1. **On-demand self-service**  
2. **Broad network access**  
3. **Resource pooling**  
4. **Rapid elasticity**  
5. **Measured service**

Três **service models**:

| Modelo | Frase NIST (ideia) | Controle do consumidor |
|--------|--------------------|------------------------|
| **IaaS** | provisiona processing, storage, networks; *deploy and run arbitrary software* (OS + apps) | Controla SO, storage, apps; **não** a infra subjacente |
| **PaaS** | plataforma para *deploy* de apps | Sem gerir SO/infra; foca na app |
| **SaaS** | usa app pronta do provedor | Quase só uso; sem SO/infra |

Quatro **deployment models**: **Public**, **Private**, **Community**, **Hybrid** cloud.

Pergunta mental ao ler texto NIST em inglês:

> O consumidor controla **sistema operacional** e pode rodar software arbitrário? → **IaaS**.  
> Só sobe app na plataforma? → **PaaS**.  
> Só usa software pronto? → **SaaS**.

### 1.6 SDLC / DevOps — vocabulário de pipeline

| Inglês | Ideia |
|--------|-------|
| **requirement** | requisito |
| **design / implementation** | projeto / implementação |
| **build** | construir (compilar/empacotar) |
| **test / unit test** | testar / teste unitário |
| **release** | lançamento |
| **deploy / deployment** | implantar / implantação |
| **pipeline** | esteira CI/CD |
| **commit / push / pull request** | versionamento |
| **refactor** | refatorar |
| **backlog / sprint / stakeholder** | ágil |
| **shift left** | antecipar qualidade/segurança no ciclo |
| **legacy** | legado |

Cartão *deploy*:

> *to deploy* ≈ **implantar** (colocar em ambiente operacional). Em cloud NIST, “deploy and run arbitrary software” = subir e executar software no recurso provisionado.

### 1.7 Mini-glossário anti-distrator (decorar)

| Se o enunciado diz… | Não marque… |
|---------------------|-------------|
| *throughput* | “variação de largura de banda” |
| *jitter* | “latência máxima” |
| *IaaS* + control over OS | PaaS/SaaS |
| *SSL VPN* no browser | IPSec / OSPF / MPLS como crypto do browser |
| *NGFW* diferencial | só bloquear porta TCP (isso o FW tradicional já faz) |
| *packet filter* | SIEM / IDS / UTM (papéis diferentes) |
| *bandwidth* nominal Mbps × volume MB | esquecer o ×8 bits |

### 1.8 Estratégia de 30 segundos na prova

1. Sublinhado mental: **substantivo técnico** (*throughput*, *IaaS*, *handshake*, *deploy*).  
2. Elimine alternativas que **redefinem o termo errado**.  
3. Em texto longo NIST/manual: busque a frase de **controle** (*control over operating systems* → IaaS).  
4. Não traduza palavra por palavra o parágrafo inteiro.

---

## 2. Questões de treino (1–15)

**1.** Em redes, *throughput* significa, de forma cobrável:

A) Variação máxima do timestamp entre interfaces  
B) Vazão efetiva de dados transferidos/processados em um intervalo  
C) Sinônimo estrito de *jitter*  
D) Apenas o nome comercial de um switch  
E) Latência máxima garantida pelo ISP  

**2.** *Jitter*, em QoS, é melhor definido como:

A) Capacidade nominal do enlace em Mbps  
B) Perda total de pacotes em 24 h  
C) Variação de latência entre pacotes  
D) Criptografia simétrica de sessão  
E) Modelo SaaS da NIST  

**3.** *Bandwidth* × *throughput* — assinale a leitura correta:

A) São sempre idênticos em qualquer rede real  
B) Bandwidth é capacidade do canal; throughput é vazão efetiva observada  
C) Throughput é só sinônimo de jitter  
D) Bandwidth significa autenticação  
E) Ambos equivalem a *rollback*  

**4.** No vocabulário NIST, *Infrastructure as a Service (IaaS)* caracteriza-se por o consumidor:

A) Usar apenas um software pronto sem controle de SO  
B) Provisionar recursos fundamentais e poder *deploy*/rodar software arbitrário (incl. SO), sem gerir a infra subjacente  
C) Gerenciar apenas o hardware físico do datacenter do provedor  
D) Eliminar toda elasticidade  
E) Ser sinônimo obrigatório de SaaS  

**5.** *Platform as a Service (PaaS)* tipicamente implica que o consumidor:

A) Administra hipervisores e switches do provedor  
B) Implanta aplicações na plataforma, sem gerenciar o SO subjacente como em IaaS clássico  
C) É obrigado a escrever drivers de kernel  
D) Não pode usar rede  
E) Equivale a *packet filter*  

**6.** *Software as a Service (SaaS)*:

A) Entrega aplicação pronta; o consumidor não gerencia SO/infra  
B) Exige sempre que o usuário compile o hypervisor  
C) É o mesmo que VLAN  
D) Proíbe HTTPS  
E) Significa apenas *jitter* baixo  

**7.** Em inglês técnico de segurança, *authentication* × *authorization*:

A) São sinônimos perfeitos  
B) Authentication identifica/valida identidade; authorization define permissões  
C) Authorization só existe em Delphi  
D) Authentication = throughput  
E) Ambos significam *backup*  

**8.** *TLS* (*Transport Layer Security*):

A) É protocolo de roteamento como OSPF  
B) Sucessor do SSL, usado para comunicação segura (ex.: browser / SSL VPN)  
C) Substitui DNS obrigatoriamente  
D) É modelo de nuvem Private  
E) Significa “teste de unidade”  

**9.** No par TLS *record protocol* × *handshake protocol*:

A) Record autentica partes; handshake só conta pacotes perdidos  
B) Record protege registros do tráfego; handshake autentica, negocia crypto e estabelece chaves  
C) Ambos são protocolos de e-mail apenas  
D) Handshake = SaaS  
E) Record = VLAN tagging  

**10.** *Next-Generation Firewall (NGFW)* destaca-se, frente ao FW tradicional, por tipicamente incluir:

A) Apenas bloqueio estático de porta TCP/UDP (e nada mais)  
B) Inspeção profunda de pacotes / comportamento além de L3/L4 básico  
C) Exclusivamente OSPF  
D) Remoção de logs  
E) Proibição de DPI  

**11.** *Deploy* / *deployment*, em SDLC/cloud, aproxima-se de:

A) Apenas “desenhar UML”  
B) Implantar (colocar software/recurso em operação/ambiente)  
C) Sinônimo de *jitter*  
D) Apagar o *schema*  
E) Traduzir HTML para COBOL  

**12.** *Broad network access*, na NIST SP 800-145, é:

A) Modelo de serviço SaaS  
B) Característica essencial: acesso via rede por mecanismos padrão  
C) Sinônimo de *packet loss*  
D) Tipo de índice de BD  
E) Protocolo TLS handshake  

**13.** Em QoS, uma lista coerente de métricas inclui:

A) Throughput de dados, jitter e retardo (latency/delay)  
B) Apenas “cor do cabo”  
C) Somente *refactor*  
D) Apenas *stakeholder*  
E) Somente *faces-config*  

**14.** *Packet filter firewall* opera tipicamente com base em:

A) Somente análise semântica de HTML  
B) Endereços IP, portas e direção/interface (visão L3/L4)  
C) Apenas treinamento de LLM  
D) Exclusivamente SaaS billing  
E) Compilação Delphi  

**15.** Qual linha resume melhor o cartão deste arquivo?

A) Inglês técnico FCC = reconhecer bandwidth/throughput/jitter, IaaS/PaaS/SaaS, TLS/NGFW/deploy sem redefinir o termo  
B) Inglês técnico = só crase e vírgula  
C) Throughput = jitter = SaaS  
D) NIST não usa inglês  
E) NGFW = apenas OSPF  

---

### Gabarito comentado (treino 1–15)

1. **B** — Vazão efetiva.  
2. **C** — Variação de latência.  
3. **B** — Capacidade × vazão.  
4. **B** — IaaS NIST.  
5. **B** — PaaS.  
6. **A** — SaaS.  
7. **B** — Identidade × permissão.  
8. **B** — Sucessor SSL / comunicação segura.  
9. **B** — Record × Handshake.  
10. **B** — DPI / além do tradicional.  
11. **B** — Implantar.  
12. **B** — Característica essencial NIST.  
13. **A** — Métricas QoS clássicas.  
14. **B** — Filtro L3/L4.  
15. **A** — Síntese do pack.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** (preferência FCC Analista TI), com banca/órgão/ano. Não inventadas. Fontes: Quizinho, QConcursos, Estude Grátis, Gabarite, furafila / gran.

**16.** (FCC – TRT-22/2022 – Analista Judiciário – TI) Com vistas a implantar computação em nuvem em um Tribunal, uma Analista recorreu ao *The NIST Definition of Cloud Computing*, publicação SP 800-145, e leu o seguinte texto:  
*The capability provided to the consumer is to provision processing, storage, networks, and other fundamental computing resources where the consumer is able to deploy and run arbitrary software, which can include operating systems and applications. The consumer does not manage or control the underlying cloud infrastructure but has control over operating systems, storage, and deployed applications; and possibly limited control of select networking components (e.g., host firewalls).*  
O texto se refere à definição de

A) Software as a Service (SaaS).  
B) Infrastructure as a Service (IaaS).  
C) Elasticity of Service (EoS).  
D) On demand as Service (OdaS).  
E) Platform as a Service (PaaS).  

**17.** (FCC – TRT-15/2023 – Analista Judiciário – TI) Na definição do NIST SP 800-145, a computação em nuvem é um modelo composto de cinco características essenciais, três modelos de serviço e quatro modelos de implantação, sendo um exemplo de cada um deles, respectivamente,

A) PaaS, On-demand self-service e Resource pooling.  
B) SaaS, Private cloud e On-demand self-service.  
C) Broad network access, IaaS e Public cloud.  
D) Rapid elasticity, Private cloud e PaaS.  
E) Hybrid cloud, IaaS e Community cloud.  

**18.** (FCC – TRT-4/2022 – Analista Judiciário – TI) Em suas pesquisas sobre qualidade de serviço, um Analista observou, corretamente, que QoS é uma coleção de tecnologias que permite que aplicativos requisitem e recebam níveis de serviços previsíveis em termos de

A) conectividade de rede em transmissões multimídia, throughput de dados, taxa de perda de pacotes, sendo que throughput de dados significa variação das diferenças de largura de banda.  
B) conectividade de rede em transmissões multimídia, throughput de banda e taxa de perda de pacotes, sendo que throughput de banda significa variação das diferenças dos tipos de transmissão.  
C) capacidade de banda, jitter e taxa de perda de pacotes, sendo que jitter significa latência máxima durante uma transmissão.  
D) capacidade de throughput de dados, jitter e retardo, sendo que jitter significa variação de latência.  
E) capacidade de banda, jitter e taxa de perda de pacotes, sendo que jitter significa variação de timestamp entre as diferentes larguras de banda.  

**19.** (FCC – TRT-19/2022 – Analista Judiciário – TI) Todo o tráfego entre um navegador web e um dispositivo SSL VPN é criptografado com o protocolo SSL ou

A) IPSec.  
B) TLS.  
C) MPLS.  
D) SSH.  
E) OSPF.  

**20.** (FCC – TRT-19/2022 – Analista Judiciário – TI) O TLS utiliza dois protocolos principais, cujas finalidades são descritas abaixo.  
I. Divide o tráfego em uma série de registros, cada um dos quais é protegido independentemente usando as chaves de tráfego.  
II. Autentica as partes comunicantes, negocia modos e parâmetros criptográficos e estabelece compartilhamento de chaves.  
Os protocolos descritos em I e II são, correta e respectivamente,

A) record protocol e agreement protocol.  
B) split protocol e key-sharing protocol.  
C) split-traffic protocol e handshake protocol.  
D) split-package protocol e consensus protocol.  
E) record protocol e handshake protocol.  

**21.** (FCC – TRT-20/2024 – Analista Judiciário – TI) Uma Analista de redes sugere a implementação de um *Next-Generation Firewall* (NGFW) para proteger os ativos da organização, pois os NGFWs oferecem funcionalidades que vão além das capacidades dos firewalls tradicionais. Uma funcionalidade de um NGFW, que não é encontrada em um firewall tradicional, é

A) o gerenciamento centralizado com uma interface gráfica amigável além de interface de linha de comando.  
B) a capacidade de realizar logs e relatórios básicos de tráfego de rede, inclusive em tempo real.  
C) a inspeção profunda de pacotes, verificando dados e comportamentos de tráfego além das camadas básicas.  
D) o bloqueio de portas TCP/UDP específicas para controlar o tráfego de rede e impedir tráfego mal-intencionado.  
E) a redundância e failover para garantir alta disponibilidade do firewall em ambientes de alta disponibilidade.  

**22.** (FCC – TRT-15/2023 – Analista Judiciário – TI) Um Analista de segurança deve implementar um tipo de firewall que atenda APENAS os seguintes requisitos:  
- Opere na camada de rede.  
- Forneça controle de acesso entre as redes com base no endereço IP de origem e destino.  
- Estabeleça o controle com base na interface que está sendo percorrida pelo pacote e sua direção (entrada ou saída).  
O tipo de firewall que atende tais requisitos é

A) UTM.  
B) SIEM.  
C) IDS.  
D) Filtro de pacote.  
E) Baseado em estados.  

**23.** (FCC – TRT-15/2023 – Analista Judiciário – TI) Os nós de rede que implementam serviços diferenciados (Diffserv) em redes IPv4 utilizam um ponto de código no cabeçalho IP para selecionar um comportamento por salto (PHB) como tratamento de encaminhamento específico para esse pacote. O PHB mais adequado para criar um serviço de baixa perda, baixa latência, baixo jitter e largura de banda garantida para um serviço de fim a fim, considerado premium, é o

A) EF - Expedited Forwarding.  
B) AF - Assured Forwarding.  
C) BE - Best-Effort.  
D) FIFO - First In First Out.  
E) LLQ - Low Latency Queuing.  

**24.** (FCC – TRF 5ª Região – Analista Judiciário) Considerando somente os valores nominais, uma placa de rede de 100 Mbps (megabits/segundo) transmite 100 MB (megabytes) de dados em

A) 1,0 s.  
B) 4,0 s.  
C) 6,25 s.  
D) 8,0 s.  
E) 12,5 s.  

**25.** (FCC – TRT-20/2024 – Técnico Judiciário – TI) Um Técnico de redes recomendou a implementação de um *Web Application Firewall* (WAF) para proteger os sistemas web da organização contra ataques maliciosos. Uma característica de um WAF que o diferencia de outros tipos de firewalls, como o *Next-Generation Firewall* (NGFW), é

A) a prevenção contra exploração de vulnerabilidades em protocolos de rede.  
B) o monitoramento de tentativas de login inválidas e ataques de força bruta.  
C) a detecção e o bloqueio de tráfego malicioso em portas específicas.  
D) a análise e o bloqueio de tráfego com base no conteúdo HTTP/HTTPS.  
E) a proteção contra ataques DoS (Denial of Service) e DDoS (Distributed Denial of Service).  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Fonte / nota |
|---|------|----------------|
| 16 | **B** | FCC TRT-22/2022 — texto NIST IaaS (*deploy and run arbitrary software* + control over OS) — Quizinho |
| 17 | **C** | FCC TRT-15/2023 — característica / serviço / implantação — Quizinho |
| 18 | **D** | FCC TRT-4/2022 — throughput + jitter (= variação de latência) — QConcursos / furafila / Gran |
| 19 | **B** | FCC TRT-19/2022 — SSL VPN → TLS — Quizinho |
| 20 | **E** | FCC TRT-19/2022 — record + handshake — Quizinho |
| 21 | **C** | FCC TRT-20/2024 — NGFW / DPI — Quizinho |
| 22 | **D** | FCC TRT-15/2023 — packet filter — Quizinho |
| 23 | **A** | FCC TRT-15/2023 — EF (baixo jitter/latência) — Quizinho |
| 24 | **D** | FCC TRF-5 — 100 MB × 8 / 100 Mbps = 8 s — Gabarite Q611306 |
| 25 | **D** | FCC TRT-20/2024 Téc. — WAF = HTTP/HTTPS — Quizinho |

Letras do gabarito neste bloco: B, C, D, B, E, C, D, A, D, D.

---

### Verificação interna

| Checagem | Status |
|----------|--------|
| Vocabulário redes/segurança/DB/cloud/SDLC | Seções 1.2–1.6 |
| Treino 1–15 + gabarito no final do bloco | OK |
| 16–25 reais com citação | 10/10 |
| Distratores de redefinição (throughput/jitter) | Q18 + treino |
| Sem inventar 16–25 | OK |

+++
title = '02 — Equipamentos de rede'
date = '2026-10-16'
draft = false
weight = 2
tags = ['semana-07', 'equipamentos', 'rede']
categories = ['estudo']
+++
# Equipamentos de rede · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Elementos de interconexão (hub, bridge, switch, roteador e correlatos)  
**Escopo deste arquivo:** papéis OSI/TCP-IP cobráveis dos equipamentos de interconexão; domínio de colisão × domínio de broadcast; switch L2 × L3; AP como ponte wireless; anti-confusão clássica da FCC (ordem das camadas no enunciado).  
**Fora de escopo aqui:** TCP/IP detalhado, DNS/DHCP/HTTP (arquivo 01 da semana, se existir); VLANs 802.1Q, STP/RSTP e 802.1X (arquivo 03); Wi-Fi 802.11ac/ax/be (arquivo 04); firewall/WAF/IDS profundos (semana 8).

---

## 1. Material

### 1.1 Por que a FCC cobra equipamentos

No edital do TRT4 (código 14), a linha de redes traz **elementos de interconexão** junto com meios, tipos de transmissão e famílias IEEE. Em prova, isso vira: **em qual camada o dispositivo opera**, **o que ele usa para decidir** (sinal × MAC × IP), e **o que isso faz com colisão e broadcast**.

Cartão mental:

| Equipamento | Camada típica (prova) | Critério de decisão | Colisão | Broadcast |
|-------------|----------------------|---------------------|---------|-----------|
| Repetidor | Física (1) | Regenera sinal | Amplia o domínio | Não segmenta |
| Hub (repetidor multiporta) | Física (1) | Replica para todas as portas | **Um** domínio para todas | Um domínio |
| Bridge | Enlace (2) | Filtra/encaminha por **MAC** | Segmenta | Em geral **não** isola VLANs sozinha (conceito clássico: 1 domínio de broadcast por bridge “plana”) |
| Switch (L2) | Enlace (2) | Tabela MAC / forwarding | **Um domínio por porta** | Um domínio (sem VLAN) |
| Roteador | Rede (3) | Endereço **IP** / tabela de rotas | Isola | **Um domínio por interface** (cada rede IP) |
| Switch L3 | 2 + 3 | MAC dentro da VLAN; IP entre VLANs/redes | Como L2 por VLAN | Isola broadcast entre redes roteadas |

### 1.2 Repetidor e hub

O **repetidor** regenera o sinal elétrico/óptico para estender alcance. Opera na **camada física**: não “entende” quadro nem pacote.

O **hub** é o repetidor multiporta clássico das LANs Ethernet half-duplex:

- Tudo que chega em uma porta é **replicado** nas demais (salvo a origem).  
- Todas as portas compartilham o **mesmo domínio de colisão**.  
- Também compartilham o **mesmo domínio de broadcast**.  
- Em full-duplex moderno com switch, o hub ficou legado — mas a FCC ainda cobra a distinção pedagógica.

**Armadilha:** chamar hub de “switch barato”. Em prova, hub = física / colisão compartilhada; switch = enlace / colisão por porta.

### 1.3 Bridge

A **bridge (ponte)** opera na **camada de enlace**. Observa endereços **MAC**, aprende em qual porta está cada estação e **filtra ou encaminha** quadros. Efeito cobrável: **segmenta o domínio de colisão** (cada lado da ponte tende a ser um domínio distinto).

Em doutrina clássica, uma bridge “plana” ainda deixa o **broadcast** atravessar o domínio bridged — daí a evolução para switches com **VLANs** (domínio de broadcast lógico). Em prova FCC recente, bridge e switch L2 são ambos camada 2; o switch é, na prática, **bridge multiporta** com ASICs.

### 1.4 Switch (comutador) L2

O **switch L2**:

1. Aprende MAC de origem nas portas (tabela CAM/MAC).  
2. Encaminha unicast conhecido só para a porta de destino.  
3. Flooda unknown unicast, broadcast e (em geral) multicast para o domínio pertinente.  
4. Em Ethernet moderno: portas tipicamente **full-duplex** → domínio de colisão **por porta** (idealmente colisão “desaparece” no sentido clássico CSMA/CD).

**Sem VLAN**, o switch L2 ainda é **um domínio de broadcast**. Segmentar broadcast exige VLAN + isolamento, ou roteador/L3 entre redes.

**Gerenciável × não gerenciável (cobrável):**

| | Não gerenciável | Gerenciável |
|---|-----------------|-------------|
| Configuração | Plug-and-play | CLI/Web/SNMP |
| VLAN / QoS / 802.1X | Em geral **não** | Sim (conforme feature) |
| Uso típico | Acesso simples, SOHO | Campus, tribunal, datacenter |

### 1.5 Roteador

O **roteador** opera na **camada de rede**. Encaminha **pacotes** com base em endereços **IP** (ou outros endereços de rede) e na **tabela de roteamento**. Cada interface tipicamente define um **domínio de broadcast** distinto (uma sub-rede).

Funções cobráveis além do “encaminhar”:

- Interligar **redes distintas** (LAN–LAN, LAN–WAN).  
- Barreira natural a broadcast L2 (broadcast Ethernet **não** atravessa o roteador).  
- NAT, ACL, roteamento estático/dinâmico (nível de prova: saber que existem; detalhe de OSPF/BGP fica secundário aqui).

**Flash anti-confusão:** “comunicação entre VLANs” ≠ “a bridge do switch L2 resolve”. Entre VLANs (redes L3 distintas) precisa de **roteamento** (default gateway → roteador ou switch L3).

### 1.6 Switch L3 (multilayer)

O **switch L3** combina comutação L2 rápida com **roteamento entre VLANs/redes** (SVI, routed ports). Em prova:

- Dentro da mesma VLAN → comportamento de switch (MAC).  
- Entre VLANs → comportamento de roteador (IP), sem necessariamente um “roteador externo”.

Não confundir: ter switch L3 **disponível** ≠ toda porta já “rota”. Precisa de configuração (SVI, IP routing, etc.).

### 1.7 Access Point (AP) e modem (visão rápida)

- **AP (Access Point):** tipicamente atua como **bridge** entre o meio wireless (802.11) e a LAN cabeada (802.3). Em arquiteturas AAA (WPA-Enterprise), o AP costuma ser o **autenticador** 802.1X (detalhe no arquivo 03).  
- **Modem:** modula/demodula para o meio de acesso (ex.: cabo/óptica do provedor). Em prova, não misturar modem com roteador doméstico “tudo-em-um” sem ler o enunciado.

### 1.8 Domínio de colisão × domínio de broadcast

Memorize o par que a FCC adora:

| Conceito | O que limita | Quem segmenta |
|----------|--------------|---------------|
| **Colisão** | Quem “compete” no mesmo meio half-duplex / mesmo segmento clássico | Bridge/switch (por porta); full-duplex reduz o problema |
| **Broadcast** | Até onde um frame FF:FF:FF:FF:FF:FF se propaga | **Roteador** (e VLANs: cada VLAN = domínio de broadcast) |

Síntese de ouro:

- Hub: 1 colisão, 1 broadcast.  
- Switch L2 (sem VLAN): N colisões (por porta), **1** broadcast.  
- Roteador: isola broadcast entre interfaces.  
- VLANs no switch: múltiplos broadcasts lógicos no mesmo hardware.

### 1.9 Mapeamento OSI cobrado à exaustão

Ordem clássica FCC (TRT-6/2018 e clones):

**Bridge → Enlace · Hub → Física · Roteador → Rede.**

Armadilha recorrente: a alternativa lista as camadas **corretas** mas na **ordem errada** dos equipamentos do enunciado. Leia sempre a sequência pedida.

Família IEEE (gancho para o arquivo 03):

- **802.3** — Ethernet (não é “o padrão de VLAN”).  
- **802.1D** — bridges / STP clássico.  
- **802.1Q** — VLANs (tag).  
- **802.1w** — RSTP.  
- **802.1X** — controle de acesso baseado em porta (autenticação).

### 1.10 Checklist de prova (30 segundos)

1. Camada? (1 / 2 / 3)  
2. Decide com quê? (sinal / MAC / IP)  
3. Segmenta colisão? Segmenta broadcast?  
4. O enunciado pediu ordem Bridge-Hub-Roteador ou outra?  
5. Comunicação **entre** VLANs → gateway / L3, não “bridge L2 mágica”.

---

## 2. Questões de treino (1–15)

**1.** No modelo OSI, o equipamento que regenera o sinal elétrico sem interpretar endereços MAC ou IP opera tipicamente na camada:

A) de Rede  
B) de Transporte  
C) de Aplicação  
D) Física  
E) de Sessão  

**2.** Em uma LAN Ethernet clássica com hub, o domínio de colisão:

A) é compartilhado por todas as portas do hub  
B) é um por porta, como no switch full-duplex  
C) deixa de existir porque hub isola broadcast  
D) é determinado exclusivamente pelo endereço IP  
E) coincide sempre com um domínio por VLAN nativa 802.1Q  

**3.** A bridge, na visão cobrável FCC, encaminha/filtra quadros com base principalmente em:

A) portas TCP de destino  
B) nomes DNS  
C) endereços MAC  
D) rotas OSPF  
E) chaves WPA3-SAE  

**4.** Um switch L2 sem VLANs, em comparação com um hub:

A) mantém um único domínio de colisão para todas as portas e isola broadcasts por porta  
B) isola automaticamente cada porta em um domínio de broadcast distinto sem configuração  
C) opera apenas na camada 3  
D) substitui obrigatoriamente o roteador em qualquer WAN  
E) cria, em regra, um domínio de colisão por porta, mas permanece um domínio de broadcast  

**5.** O roteador, no mapeamento clássico, associa-se à camada:

A) Física  
B) Enlace  
C) Rede  
D) Apresentação  
E) Sessão  

**6.** Qual afirmação melhor descreve o efeito do roteador sobre broadcasts Ethernet?

A) Impede, em regra, que o broadcast de uma interface atravessem para outra rede  
B) Replica broadcasts L2 para todas as sub-redes conectadas, por definição  
C) Transforma todo broadcast em unicast MAC automaticamente  
D) Opera exclusivamente com CSMA/CD half-duplex  
E) Substitui a tabela MAC pela tabela ARP global única em toda a internet  

**7.** Em enunciado no estilo “Bridge, Hub e Roteador — camadas respectivas”, a sequência correta é:

A) Física, Física, Enlace  
B) Rede, Física, Rede  
C) Enlace, Física, Rede  
D) Física, Enlace, Rede  
E) Rede, Enlace, Rede  

**8.** Um analista precisa que estações em VLANs diferentes troquem pacotes IP. Com apenas um switch L2 (sem roteamento), o encaminhamento inter-VLAN:

A) ocorre pela “porta de broadcast” do switch  
B) é feito pela bridge embutida, que roteia por MAC entre VLANs  
C) exige envio ao **default gateway** (roteador ou L3)  
D) é garantido por qualquer porta access  
E) dispensa endereço IP nas estações  

**9.** Switch gerenciável, em contraste com não gerenciável, tipicamente permite:

A) apenas repetir sinal na camada física  
B) unicamente cabeamento fibra, nunca cobre  
C) abolir a necessidade de endereços MAC  
D) recursos como VLAN, SNMP e políticas, além do plug-and-play cego  
E) operar sem fonte de energia  

**10.** O Access Point, na LAN mista cabeada/wireless, é melhor descrito em prova como:

A) roteador exclusivo de camada 4  
B) hub que isola domínios de broadcast por SSID sem Ethernet  
C) substituto obrigatório do DNS  
D) dispositivo que elimina a necessidade de autenticação  
E) ponte entre o meio 802.11 e a LAN 802.3 (visão típica)  

**11.** Domínio de colisão e domínio de broadcast:

A) são conceitos distintos: colisão liga-se ao meio/segmento; broadcast, ao alcance de frames de difusão  
B) são sinônimos rigorosos em qualquer topologia  
C) só existem em redes Token Ring  
D) são eliminados pelo protocolo SMTP  
E) dependem apenas da marca do cabo  

**12.** Um switch L3, na leitura cobrável, diferencia-se do L2 puro sobretudo porque:

A) não possui tabela MAC  
B) opera somente na camada física  
C) proíbe o uso de IP  
D) é o nome IEEE oficial do hub  
E) pode rotear entre VLANs/redes (camada 3) além de comutar na 2  

**13.** Qual combinação está coerente?

A) Hub → isola broadcast por porta; Roteador → um domínio de colisão global único  
B) Bridge → rotas BGP; Hub → certificados digitais  
C) Modem → camada de aplicação HTTP; Switch → camada 7  
D) Switch L2 → MAC; Roteador → IP  
E) AP → apenas camada 4 UDP  

**14.** Em campus com enlaces redundantes entre switches L2, o risco clássico na ausência de STP/RSTP é:

A) loop de camada 2 com tempestade de broadcast  
B) esgotamento exclusivo de portas seriais V.35  
C) perda obrigatória de todas as rotas OSPF da internet  
D) conversão automática de IPv4 em IPv6  
E) autenticação 802.1X forçada em toda porta  

**15.** Para o edital TRT4 (“elementos de interconexão”), a síntese mais segura é:

A) Todos os equipamentos operam só na camada de aplicação  
B) VLAN e Ethernet 802.3 são o mesmo padrão IEEE  
C) Bridge e roteador são sempre sinônimos  
D) Domínio de broadcast é segmentado pelo hub  
E) Hub/repetidor na física; bridge/switch L2 no enlace (MAC); roteador (e L3) na rede (IP), com efeitos distintos em colisão e broadcast  

---

### Gabarito comentado (treino 1–15)

1. **D** — Repetidor/hub: física.  
2. **A** — Hub = colisão compartilhada.  
3. **C** — Bridge filtra/encaminha por MAC.  
4. **E** — Switch: colisão por porta; 1 broadcast sem VLAN.  
5. **C** — Roteador = rede.  
6. **A** — Broadcast L2 não atravessa o roteador.  
7. **C** — Bridge/Enlace, Hub/Física, Roteador/Rede (TRT-6/2018).  
8. **C** — Inter-VLAN via default gateway / L3.  
9. **D** — Gerenciável = VLAN/SNMP/políticas.  
10. **E** — AP como ponte wireless↔cabeada.  
11. **A** — Conceitos distintos.  
12. **E** — L3 = comuta + roteia.  
13. **D** — MAC no L2; IP no roteador.  
14. **A** — Loop L2 / broadcast storm.  
15. **E** — Cartão mental completo do subtópico.

---

## 3. Questões estilo concurso FCC (16–25)

> Bloco com questões reais de provas (preferência FCC/TRT Analista TI). Gabarito só ao final.

**16.** (FCC – TRT-6/2018 – Analista Judiciário – TI)  
Em uma rede de computadores, diferentes tipos de elementos e dispositivos de interconexão são utilizados. Cada um deles opera sobre protocolos específicos das camadas hierárquicas do modelo de referência OSI e da arquitetura TCP/IP. Considerando o modelo OSI, a Bridge, o Hub e o Roteador são mapeados, correta e respectivamente, nas camadas

A) de Enlace, Física e de Rede.  
B) Física, Física e de Enlace.  
C) de Rede, Física e de Rede.  
D) Física, de Enlace e de Rede.  
E) de Rede, de Enlace e de Rede.

**17.** (FCC – TRT-6/2018 – Analista Judiciário – TI)  
Considere uma instalação de uma rede local de computadores que inclui três VLANs (VLAN1, VLAN2 e VLAN3) em sua topologia. As VLANs foram segmentadas utilizando uma Switch L2. Considere, ainda, um pacote gerado por um computador pertencente à VLAN1 que deve ser encaminhado para um computador pertencente à VLAN3. Nessa situação, o encaminhamento do pacote na VLAN1 será realizado por meio

A) da Porta de expansão da Switch L2.  
B) do Default Gateway.  
C) da Bridge instalada na Switch L2.  
D) da Porta de Broadcast.  
E) do roteador embutido na Switch L2.

**18.** (FGV – TCE-GO/2024 – Analista de Controle Externo – TI)  
Normalmente as LANs não operam de forma isolada. Elas são conectadas entre si ou à Internet. Para interligar LANs, ou segmentos de LANs, usamos dispositivos de conexão, que podem operar em diferentes camadas da arquitetura TCP/IP. Relacione o dispositivo de conexão à respectiva camada na qual ele opera.

( ) Hub  
( ) Roteador  
( ) Switch  
( ) Ponte  

1. Física  
2. Enlace  
3. Rede  

Assinale a opção que indica a relação correta na ordem apresentada.

A) 1 – 3 – 2 – 2  
B) 1 – 2 – 2 – 3  
C) 2 – 3 – 2 – 1  
D) 2 – 2 – 3 – 1  
E) 2 – 3 – 2 – 3  

**19.** (FCC – TRT-6/2018 – Conhecimentos comuns)  
Um Analista comprou um roteador wireless e o conectou por cabo em um switch para acessar a estrutura de rede cabeada. Isso permitiu que todos os dispositivos sem fio conectados nesse roteador tivessem acesso a todos os serviços disponíveis na rede cabeada, como por exemplo, acesso à internet. Nesse caso, o roteador foi configurado pelo Analista para operar no modo

A) ponto-a-ponto.  
B) access point.  
C) bridge.  
D) modem.  
E) backbone.

**20.** (FCC – TRT-15/2023 – Analista Judiciário – TI)  
Um Analista de segurança deve implementar um tipo de firewall que atenda APENAS os seguintes requisitos:

- Opere na camada de rede.  
- Forneça controle de acesso entre as redes com base no endereço IP de origem e destino.  
- Estabeleça o controle com base na interface que está sendo percorrida pelo pacote e sua direção (entrada ou saída).  

O tipo de firewall que atende tais requisitos é

A) UTM.  
B) SIEM.  
C) IDS.  
D) Filtro de pacote.  
E) Baseado em estados.

**21.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Ao pesquisar sobre os padrões IEEE, um Analista observou, corretamente, que bridges que usam LANs virtuais (VLANs)

A) nunca fizeram parte do padrão IEEE 802.1D, mas foram especificadas em um padrão separado, o padrão IEEE 802.1Q.  
B) fazem parte do padrão IEEE 802.3.  
C) são especificadas no padrão IEEE 802.1s.  
D) nunca fizeram parte do padrão IEEE 802.1Q, mas foram especificadas em um padrão separado, o padrão IEEE 802.1D.  
E) são especificadas no padrão IEEE 802.1w.

**22.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Um Tribunal Regional do Trabalho deseja segmentar sua rede para melhorar a segurança e o desempenho. O administrador de redes decide configurar VLANs para separar o tráfego dos diferentes departamentos. Para que os switches consigam identificar corretamente os pacotes pertencentes a cada VLAN e permitir a segmentação lógica do tráfego, o padrão que deve ser implementado é o IEEE

A) 802.1Q  
B) 802.3af  
C) 802.3z  
D) 802.1D  
E) 802.1X  

**23.** (FCC – SEFAZ-SC/2018 – Auditor Fiscal – TI)  
Com o objetivo de melhorar a segurança e o desempenho de uma rede local de computadores (LAN), o Administrador da rede implantou o recurso de VLAN utilizando o padrão IEEE 802.1Q. A característica dessa implantação é a utilização

A) do campo Porta Destino do protocolo IP para identificar a VLAN.  
B) de tags nos quadros Ethernet para identificar a VLAN.  
C) do campo de Endereço Destino do protocolo IP para identificar a VLAN.  
D) de uma máscara de sub-rede para distinguir a VLAN.  
E) do conceito de Classe de endereço IP para distinguir a VLAN.

**24.** (FCC – TRT-6/2018 – Analista Judiciário – TI)  
Os tipos de redes de computadores, LAN e WAN, requerem tecnologias de transmissão adequadas às distâncias físicas envolvidas em cada tipo de rede. Uma correta escolha de tecnologias para a LAN e para a WAN é, respectivamente,

A) ATM e CAT5e.  
B) CAT5e e 802.11n.  
C) Token Ring e CAT6.  
D) Frame Relay e Token Ring.  
E) Fibra ótica multímodo e Frame Relay.

**25.** (FCC – TCE-GO/2022 – Analista de Controle Externo – TI)  
Para uma determinada rede wireless foram especificados os seguintes requisitos de segurança:

1. A implantação do padrão 802.1x com servidor RADIUS para controlar o acesso de usuários.  
2. O mecanismo de autenticação EAP com base em certificado digital do cliente e da rede.

Considerando esses requisitos, a alternativa que descreve, correta e respectivamente, o padrão de segurança e autenticação wireless capaz de atender às especificações da implantação é

A) WEP e EAP-MD-5.  
B) WPA-Personal e EAP-LEAP.  
C) WPA2-Enterprise e EAP-TLS.  
D) WPA3-Personal e EAP-SIM.  
E) WPA4-Enterprise e EAP-FAST.

---

### Gabarito comentado (FCC 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **A** | FCC TRT-6/2018 — Bridge/Enlace, Hub/Física, Roteador/Rede |
| 17 | **B** | FCC TRT-6/2018 — inter-VLAN via Default Gateway |
| 18 | **A** | FGV TCE-GO/2024 — Hub 1, Roteador 3, Switch 2, Ponte 2 |
| 19 | **B** | FCC TRT-6/2018 — modo access point |
| 20 | **D** | FCC TRT-15/2023 — filtro de pacote (APENAS) |
| 21 | **A** | FCC TRT-19/2022 — VLAN no 802.1Q |
| 22 | **A** | FCC TRT-1/2025 — 802.1Q |
| 23 | **B** | FCC SEFAZ-SC/2018 — tags Ethernet |
| 24 | **E** | FCC TRT-6/2018 — fibra multimodo (LAN) + Frame Relay (WAN) |
| 25 | **C** | FCC TCE-GO/2022 — WPA2-Enterprise + EAP-TLS (AP/RADIUS) |

**Notas rápidas:** 16 A. 17 B. 18 A. 19 B (AP). 20 D. 21 A. 22 A. 23 B. 24 E. 25 C.

Distribuição do gabarito (16–25): A, B, A, B, D, A, A, B, E, C — letras A–E presentes.

*Arquivo 02 · Equipamentos de rede · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 03 VLAN / STP / 802.1X.*

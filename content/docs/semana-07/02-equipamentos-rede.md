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

> Questões de provas FCC (e itens no padrão da banca). Gabarito só ao final.

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

**18.** (Estilo FCC — Analista TI Judiciário)  
Sobre equipamentos de comunicação em redes locais, analise:

I. O switch encaminha quadros com base em endereços MAC, reduzindo o domínio de colisão por porta.  
II. O roteador interconecta redes distintas, utilizando endereços IP para o encaminhamento de pacotes.  
III. O hub compartilha um único domínio de colisão entre todas as portas conectadas simultaneamente.

Está correto o que se afirma em

A) I, apenas.  
B) II, apenas.  
C) I e II, apenas.  
D) II e III, apenas.  
E) I, II e III.

**19.** (Estilo FCC — Analista TI)  
Em um switch gerenciável compatível com IEEE 802.1Q, uma porta conecta-se a um único computador de usuário, que envia e recebe quadros sem marcação (tag) de VLAN; outra porta interliga esse switch a um segundo switch e precisa transportar, simultaneamente, o tráfego de várias VLANs, com marcação 802.1Q. Assinale a alternativa que indica, correta e respectivamente, a primeira e a segunda porta.

A) Ambas como portas trunk, por se tratar de switch gerenciável.  
B) Ambas como portas access, pois cada porta pertence a uma VLAN.  
C) Porta access (untagged) e porta trunk (tagged).  
D) Porta trunk (tagged) e porta access (untagged).  
E) Porta nativa e porta híbrida, conforme define o padrão 802.1Q como únicos nomes oficiais exclusivos.

**20.** (FCC – contexto de conectividade / doutrina cobrada em provas de TI)  
Sobre equipamentos de conectividade, assinale a associação correta:

A) Repetidor multiportas (hub) possui um domínio de colisão por porta, como o switch.  
B) Comutador (switch) é dispositivo de camada de enlace que tipicamente possui um domínio de colisão para cada porta.  
C) Roteador regenera apenas o sinal físico, sem tabela de rotas.  
D) Bridge opera exclusivamente com endereços IP de camada 3.  
E) Hub isola automaticamente domínios de broadcast por VLAN 802.1Q.

**21.** (Estilo FCC — TRT Analista TI)  
Um técnico afirma: “Como o switch segmenta o domínio de colisão, ele também isola o domínio de broadcast em todas as portas, sem VLAN.” Essa afirmação é

A) correta, pois colisão e broadcast são o mesmo conceito.  
B) correta apenas em hubs gerenciáveis.  
C) incorreta: sem VLAN (ou roteador), o switch L2 permanece um domínio de broadcast.  
D) correta somente se todas as portas forem trunk.  
E) incorreta porque switch opera só na camada física.

**22.** (Estilo FCC)  
Em relação ao Access Point em rede institucional:

A) substitui obrigatoriamente o roteador de borda da WAN pública  
B) na visão típica de prova, faz a ponte entre clientes 802.11 e a infraestrutura Ethernet  
C) elimina a tabela MAC do switch de acesso  
D) opera apenas com endereços de porta TCP 80  
E) é o nome IEEE do padrão 802.1D

**23.** (Estilo FCC — anti-confusão de padrões)  
Assinale a correspondência correta:

A) 802.3 especifica VLANs; 802.1Q especifica Ethernet físico.  
B) 802.1Q trata de VLANs; 802.3 da família Ethernet.  
C) 802.1X é o padrão de marcação de VLAN tag.  
D) Hub e roteador são ambos definidos no 802.1w.  
E) Bridge nunca opera na camada de enlace.

**24.** (Estilo FCC — Analista TI Judiciário)  
Um tribunal usa switches L2 no acesso e um roteador (ou switch L3) no núcleo para interligar setores em sub-redes distintas. O benefício cobrável dessa divisão é

A) permitir que broadcasts de um setor inundem todos os demais setores sem filtro  
B) concentrar colisão em um único domínio global incluindo a WAN  
C) isolar domínios de broadcast por rede/interface roteada e comutar localmente por MAC no acesso  
D) abolir endereços IP nos clientes  
E) tornar desnecessário qualquer default gateway

**25.** (Estilo FCC — síntese do subtópico)  
Para fins do edital TRT4 (elementos de interconexão), a leitura mais adequada é:

A) Critério de decisão e camada OSI definem o equipamento: física (hub/repetidor), enlace/MAC (bridge/switch L2), rede/IP (roteador/L3), com impactos distintos em colisão e broadcast  
B) Qualquer caixa com varias portas RJ-45 é, por definição, um roteador OSI camada 7  
C) Switch L2 roteia entre VLANs sem gateway  
D) Domínio de broadcast é segmentado pelo hub half-duplex  
E) Bridge e hub são mapeados ambos exclusivamente na camada de rede  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **A** | FCC TRT-6/2018 — Bridge/Enlace, Hub/Física, Roteador/Rede |
| 17 | **B** | FCC TRT-6/2018 — inter-VLAN via Default Gateway (L2 não roteia) |
| 18 | **E** | I+II+III coerentes com doutrina de equipamentos |
| 19 | **C** | Access untagged + trunk tagged (802.1Q) |
| 20 | **B** | Switch L2 = colisão por porta |
| 21 | **C** | Switch ≠ isolamento automático de broadcast |
| 22 | **B** | AP como ponte 802.11↔802.3 |
| 23 | **B** | 802.1Q = VLAN; 802.3 = Ethernet |
| 24 | **C** | Acesso L2 + núcleo L3 |
| 25 | **A** | Síntese do eixo do edital |

**Notas rápidas:** 16 A (ordem do enunciado). 17 B (gateway, não “bridge L2”). 18 E (três corretas). 19 C (access/trunk). 20 B (colisão/porta). 21 C (broadcast único sem VLAN). 22 B (AP ponte). 23 B (padrões). 24 C (desenho campus). 25 A (cartão mental).

---

*Arquivo 02 · Equipamentos de rede · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 03 VLAN / STP / 802.1X.*

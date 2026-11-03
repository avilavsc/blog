+++
title = '03 — VLAN, STP/RSTP e 802.1X'
date = '2026-10-17'
draft = false
weight = 3
tags = ['semana-07', 'vlan', 'stp']
categories = ['estudo']
+++
# VLAN, STP/RSTP e 802.1X · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — IEEE 802.1Q (VLAN), STP/RSTP e 802.1X  
**Escopo deste arquivo:** VLANs e tagging 802.1Q (access/trunk, VID); STP (802.1D) e RSTP (802.1w) — root bridge, prioridade, estados; 802.1X — suplicante, autenticador, servidor AAA/RADIUS e gancho EAP.  
**Fora de escopo aqui:** equipamentos básicos isolados (arquivo 02); detalhes profundos de Wi-Fi PHY 802.11ac/ax/be (arquivo 04); WPA3/VPN/firewall (semana 8) — aqui só o necessário para amarrar 802.1X.

---

## 1. Material

### 1.1 Por que este trio cai junto

O edital TRT4 cita explicitamente **IEEE 802.1Q**, **802.1X** e **STP/RSTP**. A FCC já cobrou os três em provas de Analista TI de TRT: VLAN ≠ 802.1D; root bridge por **menor** prioridade em múltiplos de 4096; RSTP com **três** estados; papéis 802.1X (AP/switch = autenticador; RADIUS = servidor).

Cartão mental de padrões:

| Padrão | Assunto cobrável |
|--------|------------------|
| **802.1Q** | VLANs / tag no quadro Ethernet |
| **802.1D** | Bridges + STP clássico (não é o padrão de VLAN) |
| **802.1w** | RSTP (convergência rápida; 3 estados) |
| **802.1s** | MSTP (múltiplas árvores) — distrator frequente |
| **802.1X** | Controle de acesso baseado em porta (autenticação) |
| **802.3** | Ethernet — **não** define VLAN |

### 1.2 VLAN — ideia de prova

**VLAN (Virtual LAN)** segmenta logicamente uma LAN física: estações em VLANs distintas comportam-se como se estivessem em **LANs separadas**, mesmo no mesmo switch.

Efeitos cobráveis:

- Cada VLAN ≈ **domínio de broadcast** próprio.  
- Reduz tempestades de broadcast e melhora organização/segurança (isolamento).  
- Comunicação **entre** VLANs exige **roteamento** (gateway / switch L3) — não a bridge L2 “pura”.

Critérios comuns de agrupamento (prova): por **porta** (port-based), por MAC, por protocolo/sub-rede (conforme vendor) — **não** “apenas IP ponto a ponto” como única forma.

### 1.3 IEEE 802.1Q — a tag

O **802.1Q** inseriu um **tag** (rótulo) no quadro Ethernet para identificar a VLAN. Campos cobráveis em nível analista:

- **TPID** (tipicamente 0x8100) — indica quadro tagged 802.1Q.  
- **TCI**, contendo entre outros:  
  - **PCP** (prioridade — ligação histórica com 802.1p/QoS).  
  - **CFI/DEI**.  
  - **VID (VLAN ID)** — identifica a VLAN (12 bits → tipicamente 1–4094 utilizáveis; 0 e 4095 reservados na leitura clássica).

**Flash FCC (SEFAZ-SC/2018 e clones):** a característica da implantação 802.1Q é o uso de **tags nos quadros Ethernet**, **não** máscara de sub-rede, classe IP ou “porta destino do IP”.

**Anti-confusão TRT-19/2022:** bridges que usam VLANs **não** “vieram do 802.1D”; foram especificadas no **802.1Q** (padrão separado). Inverter Q↔D é a pegadinha favorita.

**TRT-1/2025:** para switches identificarem pacotes/quadros por VLAN na segmentação, o padrão é **802.1Q** (não 802.3af, 802.3z, 802.1D, 802.1X).

### 1.4 Access × Trunk

| Tipo de porta | Uso típico | Tag |
|---------------|------------|-----|
| **Access** | Host final (PC, impressora) | Em geral **untagged** na perspectiva do host; o switch associa a porta a **uma** VLAN |
| **Trunk** | Switch–switch, switch–roteador (subinterfaces) | Transporta **múltiplas** VLANs; quadros **tagged** (exceto VLAN nativa, em muitos designs) |

**VLAN nativa (Cisco-speak cobrado):** tráfego untagged no trunk associado a uma VLAN; mismatch de native VLAN entre pontas é clássico de incidente.

### 1.5 STP (IEEE 802.1D) — evitar loops L2

Com enlaces redundantes entre bridges/switches, sem controle há **loop de camada 2**: broadcasts circulam (broadcast storm), tabelas MAC oscilam.

O **Spanning Tree Protocol (STP)**:

1. Elege a **Root Bridge**.  
2. Calcula uma árvore livre de loops.  
3. Coloca portas redundantes em estado que **não encaminha** (blocking no STP clássico).

**Bridge ID = Prioridade + MAC.**  
Ganha a root a bridge de **menor** Bridge ID. Com prioridades iguais, desempatam pelo **menor MAC**.

**Prioridade:** valor configurável em **múltiplos de 4096** (0, 4096, 8192, …; default clássico **32768**).

**Caso FCC TRT-17/2022:** SW1=8192, SW2=12288; para SW3 ser raiz, prioridade válida **menor** que 8192 → **4096** (não 512 nem 1024 — não são múltiplos válidos de 4096 no discurso de prova).

Estados clássicos STP (5): **Disabled, Blocking, Listening, Learning, Forwarding.**

### 1.6 RSTP (IEEE 802.1w)

O **Rapid Spanning Tree** acelera a convergência. Diferença mais cobrada pela FCC (TRT-1/2014):

- STP: **cinco** estados de porta.  
- RSTP: **três** estados → **Discarding, Learning, Forwarding.**

`Discarding` absorve Disabled + Blocking + Listening do mundo 802.1D.

Funções de porta RSTP (visão cobrável): Root, Designated, **Alternate**, **Backup** (além do modelo antigo). Alternate/Backup permitem failover mais rápido que esperar timers longos do STP legado.

**Não confundir:**

- 802.1w = RSTP  
- 802.1s = MSTP  
- 802.1Q = VLAN (não “versão rápida do STP”)

### 1.7 IEEE 802.1X — porteiro da porta

**802.1X** controla se uma porta (cabeada ou, no wireless enterprise, via AP) libera acesso **após autenticação**.

Três papéis (decore):

| Papel | Quem é | Função |
|-------|--------|--------|
| **Suplicante (supplicant)** | Cliente / STA / notebook | Pede acesso; responde EAP |
| **Autenticador** | Switch ou **Access Point** | Controla a porta; relê EAPOL ↔ RADIUS |
| **Servidor de autenticação** | Em geral **RADIUS** | Decide aceitar/negar; políticas |

Fluxo mental: cliente ↔ autenticador (EAPOL) ; autenticador ↔ RADIUS (EAP encapsulado).

**FCC TRT-17/2022 (Técnico, mesmo mapa conceitual):** em WLAN WPA2-EAP, o AP é o **autenticador** e o RADIUS o **servidor de autenticação**.

**FCC TCE-GO/2022:** requisitos 802.1X+RADIUS + EAP com certificado de cliente e da rede → **WPA2-Enterprise + EAP-TLS** (não WPA-Personal/PSK; não WEP; “WPA4” não existe).

**FCC TRT-2/2025:** fluxo com EAP-Request/TLS Start após Identity → conclusão: **EAP-TLS** (não PSK, não MD5 “puro”, não PEAP só porque “tem túnel” sem ler o log).

**FCC TRT-20/2024:** falha de autenticação 802.1X permitindo dispositivo indevido → causa cobrada: **RADIUS mal configurado** (não “falta de firewall” como primeira resposta).

### 1.8 VLAN + 802.1X + STP — como a prova mistura

- VLAN segmenta **depois**/à parte da topologia lógica; 802.1X decide **se** a porta autentica (pode inclusive atribuir VLAN dinâmica via RADIUS — nível avançado).  
- STP/RSTP evita loop **L2** independentemente de “ter VLAN”, mas em redes VLAN há variantes (PVST, MST…). Para FCC analista: saber **por que** STP existe e **Q ≠ D ≠ w ≠ X**.  
- Tag 802.1Q **não** autentica usuário; 802.1X **não** é o padrão da tag.

### 1.9 Checklist de 30 segundos

1. VLAN padrão? → **802.1Q** (tags).  
2. Inter-VLAN? → gateway / L3.  
3. Root? → menor prioridade **válida** (×4096).  
4. RSTP estados? → Discarding / Learning / Forwarding.  
5. 802.1X? → suplicante / autenticador / RADIUS.  
6. Certificados mútuos no wireless enterprise? → EAP-TLS + WPA2/3-Enterprise.

---

## 2. Questões de treino (1–15)

**1.** O padrão IEEE que especifica o tagging de VLANs em quadros Ethernet é o:

A) 802.1D  
B) 802.1w  
C) 802.1Q  
D) 802.3af  
E) 802.1X  

**2.** Uma característica cobrável da implantação 802.1Q é:

A) inserir **tags** nos quadros Ethernet para identificar a VLAN  
B) usar a máscara de sub-rede IPv4 como único identificador de VLAN  
C) usar o campo “porta destino” do cabeçalho IP como VID  
D) abolir endereços MAC  
E) exigir OSPF em toda porta access  

**3.** Porta **access** tipicamente:

A) transporta dezenas de VLANs tagged para o PC do usuário  
B) é o nome IEEE oficial exclusivo de “VLAN nativa”  
C) substitui o STP  
D) associa o host a uma VLAN, com tráfego em geral untagged na visão do host  
E) autentica por definição sem RADIUS  

**4.** Porta **trunk** entre dois switches serve principalmente para:

A) ligar apenas um PC sem tag  
B) eliminar a necessidade de root bridge  
C) converter automaticamente IPv4 em IPv6  
D) desabilitar broadcast em todas as VLANs  
E) transportar tráfego de **múltiplas** VLANs, usualmente com tags 802.1Q  

**5.** Comunicação entre hosts em VLANs distintas exige, em regra:

A) roteamento (default gateway / switch L3)  
B) apenas a bridge L2 do mesmo switch, sem IP  
C) hub half-duplex entre as VLANs  
D) desligar o STP  
E) VID 4095 em todas as portas  

**6.** No STP, a Root Bridge é eleita, em essência, pela:

A) maior prioridade numérica sempre  
B) maior endereço MAC exclusivamente, ignorando prioridade  
C) menor número de portas físicas  
D) menor Bridge ID (prioridade + MAC), com prioridade em múltiplos de 4096  
E) maior throughput Wi-Fi 7  

**7.** SW1 prioridade 8192 e SW2 12288. Para SW3 ser raiz com valor **válido** típico de prova, usa-se:

A) 512  
B) 1024  
C) 16384  
D) 4096  
E) 12288  

**8.** Os três estados de porta do RSTP (802.1w) são:

A) Discarding, Learning, Forwarding  
B) Blocking, Listening, Forwarding  
C) Discarding, Listening, Disabled  
D) Listening, Learning, Disabled  
E) Blocking, Learning, Forwarding  

**9.** Em relação a 802.1D e 802.1Q:

A) 802.1D especifica VLANs; 802.1Q especifica apenas RSTP  
B) 802.1Q e 802.1D são o mesmo documento IEEE  
C) 802.1w é o padrão de tag VLAN  
D) 802.3 substitui o 802.1Q para VID  
E) VLANs com bridges foram especificadas no 802.1Q, separado do 802.1D  

**10.** No 802.1X, o servidor RADIUS desempenha o papel de:

A) suplicante  
B) autenticador  
C) servidor de autenticação  
D) root bridge  
E) VLAN nativa  

**11.** Em WLAN WPA2-EAP, o Access Point é tipicamente o:

A) suplicante  
B) root bridge STP  
C) cliente RADIUS do usuário final sem EAPOL  
D) autenticador  
E) padrão 802.1Q em si  

**12.** Requisitos: 802.1X com RADIUS + EAP com certificados do cliente e da rede. A combinação adequada é:

A) WEP e EAP-MD5  
B) WPA-Personal e EAP-LEAP  
C) WPA2-Enterprise e EAP-TLS  
D) WPA3-Personal e EAP-SIM  
E) WPA4-Enterprise e EAP-FAST  

**13.** No RSTP, em comparação com o STP clássico, a vantagem central cobrada é:

A) abolir qualquer necessidade de root bridge  
B) substituir Ethernet por Token Ring  
C) autenticar usuários via VID  
D) marcar tags 802.1Q automaticamente sem switch gerenciável  
E) convergência significativamente mais rápida e redução/reorganização dos estados de porta  

**14.** Um analista confunde padrões e afirma que 802.1X “é o padrão que coloca o VID no quadro”. A correção é:

A) ele está certo: 802.1X = tagging  
B) ambos são sinônimos de 802.1w  
C) o tagging de VLAN é 802.1Q; 802.1X é controle de acesso/autenticação  
D) o correto seria 802.3z para autenticação  
E) STP substitui 802.1Q e 802.1X  

**15.** Síntese alinhada ao edital TRT4 para este subtópico:

A) 802.1Q segmenta com tags; STP/RSTP evitam loops L2; 802.1X autentica antes do acesso pleno à porta/rede  
B) STP autentica usuários; 802.1X evita loops; 802.1Q elege a root  
C) Trunk é proibido em 802.1Q  
D) Prioridade STP válida inclui qualquer inteiro positivo  
E) RADIUS é o nome do estado Discarding  

---

### Gabarito comentado (treino 1–15)

1. **C** — 802.1Q.  
2. **A** — tags Ethernet.  
3. **D** — access / uma VLAN / untagged no host.  
4. **E** — trunk multi-VLAN tagged.  
5. **A** — precisa L3.  
6. **D** — menor Bridge ID; ×4096.  
7. **D** — 4096 (TRT-17/2022).  
8. **A** — Discarding, Learning, Forwarding (TRT-1/2014).  
9. **E** — Q separado de D (TRT-19/2022).  
10. **C** — servidor de autenticação.  
11. **D** — AP = autenticador.  
12. **C** — WPA2-Enterprise + EAP-TLS (TCE-GO/2022).  
13. **E** — convergência rápida.  
14. **C** — Q = tag; X = auth.  
15. **A** — cartão mental do trio.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Ao pesquisar sobre os padrões IEEE, um Analista observou, corretamente, que bridges que usam LANs virtuais (VLANs)

A) nunca fizeram parte do padrão IEEE 802.1D, mas foram especificadas em um padrão separado, o padrão IEEE 802.1Q.  
B) fazem parte do padrão IEEE 802.3.  
C) são especificadas no padrão IEEE 802.1s.  
D) nunca fizeram parte do padrão IEEE 802.1Q, mas foram especificadas em um padrão separado, o padrão IEEE 802.1D.  
E) são especificadas no padrão IEEE 802.1w.

**17.** (FCC – TRT-17/2022 – Analista Judiciário – TI)  
Ao ligar três switches (SW1, SW2 e SW3) entre si, o analista em tecnologia da informação observou que os frames em broadcast estavam sendo encaminhados infinitamente, caracterizando um loop de rede. Para evitar o loop, o analista habilitou o protocolo STP nos switches e alterou a prioridade da bridge dos switches SW1 e SW2, respectivamente, para 8192 e 12288. Para definir o SW3 como switch raiz, a prioridade deve ser definida em

A) 512  
B) 12288  
C) 16384  
D) 4096  
E) 1024  

**18.** (FCC – TRT-1/2014 – Analista Judiciário – TI)  
O Rapid Spanning Tree Protocol − RSTP, definido pela norma IEEE 802.1w, possui como vantagem apresentar velocidade de convergência significativamente maior que o antecessor Spanning Tree Protocol − STP. Dentre as diferenças entre o RSTP e o STP está a quantidade de estados definidos para a Porta da Switch, que passou de cinco no STP para três no RSTP, e que são

A) Blocking, Listening e Forwarding.  
B) Discarding, Learning e Forwarding.  
C) Blocking, Learning e Forwarding.  
D) Discarding, Listening e Disabled.  
E) Listening, Learning e Disabled.

**19.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Um Tribunal Regional do Trabalho deseja segmentar sua rede para melhorar a segurança e o desempenho. O administrador de redes decide configurar VLANs para separar o tráfego dos diferentes departamentos. Para que os switches consigam identificar corretamente os pacotes pertencentes a cada VLAN e permitir a segmentação lógica do tráfego, o padrão que deve ser implementado é o IEEE

A) 802.1Q  
B) 802.3af  
C) 802.3z  
D) 802.1D  
E) 802.1X  

**20.** (FCC – SEFAZ-SC/2018 – Auditor Fiscal – TI)  
Com o objetivo de melhorar a segurança e o desempenho de uma rede local de computadores (LAN), o Administrador da rede implantou o recurso de VLAN utilizando o padrão IEEE 802.1Q. A característica dessa implantação é a utilização

A) do campo Porta Destino do protocolo IP para identificar a VLAN.  
B) de tags nos quadros Ethernet para identificar a VLAN.  
C) do campo de Endereço Destino do protocolo IP para identificar a VLAN.  
D) de uma máscara de sub-rede para distinguir a VLAN.  
E) do conceito de Classe de endereço IP para distinguir a VLAN.

**21.** (FCC – TRT-17/2022 – Técnico Judiciário – TI; mesmo mapa 802.1X)  
Considere uma rede WLAN configurada com WPA2-EAP. Nessa arquitetura, o Access Point é considerado o I e o servidor RADIUS o II. As lacunas I e II devem ser preenchidas, correta e respectivamente, por:

A) supliante – cliente  
B) supliante – autenticador  
C) autenticador – supliante  
D) autenticador – servidor de autenticação  
E) cliente – autenticador  

**22.** (FCC – TCE-GO/2022 – Analista de Controle Externo – TI)  
Para uma determinada rede wireless foram especificados os seguintes requisitos de segurança:

1. A implantação do padrão 802.1x com servidor RADIUS para controlar o acesso de usuários.  
2. O mecanismo de autenticação EAP com base em certificado digital do cliente e da rede.

Considerando esses requisitos, a alternativa que descreve, correta e respectivamente, o padrão de segurança e autenticação wireless capaz de atender às especificações da implantação é

A) WEP e EAP-MD-5.  
B) WPA-Personal e EAP-LEAP.  
C) WPA2-Enterprise e EAP-TLS.  
D) WPA3-Personal e EAP-SIM.  
E) WPA4-Enterprise e EAP-FAST.

**23.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Durante uma auditoria de segurança, foi identificado que um dispositivo não autorizado conseguiu acessar a rede. O Técnico de rede suspeita que houve uma falha no processo de autenticação IEEE 802.1X, que pode ter sido causada por

A) falta de um firewall na rede.  
B) configuração incorreta do servidor RADIUS.  
C) uso de um switch não gerenciável.  
D) falta de criptografia WPA2.  
E) configuração de VLANs inadequada.

**24.** (FCC – TRT-2/2025 – Analista Judiciário – TI)  
Um analista de segurança de um Tribunal está investigando um possível acesso não autorizado à rede Wi-Fi interna. Durante a análise de pacotes capturados, ele identificou um fluxo de autenticação, conforme o log:

1. Cliente (STA) → AP : EAPOL-Start  
2. AP → Cliente (STA) : EAP-Request/Identity  
3. Cliente (STA) → AP : EAP-Response/Identity  
4. AP → Servidor RADIUS : Encapsula EAP-Response/Identity no RADIUS  
5. Servidor RADIUS → AP: EAP-Request/TLS Start  
6. AP → Cliente (STA) : Encaminha EAP-Request/TLS Start  
7. Cliente (STA) → AP : EAP-Response/TLS Client Hello  
8. AP → Servidor RADIUS : Encapsula EAP-Response/TLS Client Hello no RADIUS

Com base na análise do fluxo, o analista concluiu que

A) a rede opera com WPA2-PSK, indicado pelo fluxo simplificado de autenticação.  
B) o servidor RADIUS solicitou através do protocolo TCP o certificado do cliente para completar a autenticação.  
C) o processo utiliza EAP-TLS, evidenciado pela negociação TLS iniciada após a identificação do cliente.  
D) a autenticação ocorre através de EAP-MD5, com base na troca de identidade sem certificados.  
E) o método de autenticação é PEAP, pois o servidor estabelece um túnel seguro antes da autenticação.

**25.** (Estilo FCC — síntese VLAN/STP/802.1X)  
Assinale a alternativa correta:

A) 802.1D é o padrão de tag VLAN; 802.1Q elege a root bridge.  
B) RSTP possui cinco estados idênticos aos do STP clássico, apenas renomeados para “rápido”.  
C) Em 802.1X, o autenticador (switch/AP) intermedia o suplicante e o servidor de autenticação (RADIUS).  
D) Trunk access são sinônimos rigorosos no 802.1Q.  
E) Prioridade STP 1024 é sempre a escolha válida preferencial da FCC quando se deseja ser root.

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **A** | FCC TRT-19/2022 — VLAN no 802.1Q, não no 802.1D |
| 17 | **D** | FCC TRT-17/2022 — prioridade 4096 (múltiplo válido < 8192) |
| 18 | **B** | FCC TRT-1/2014 — Discarding, Learning, Forwarding |
| 19 | **A** | FCC TRT-1/2025 — 802.1Q para identificar VLAN |
| 20 | **B** | FCC SEFAZ-SC/2018 — tags Ethernet |
| 21 | **D** | FCC TRT-17/2022 — AP autenticador; RADIUS servidor |
| 22 | **C** | FCC TCE-GO/2022 — WPA2-Enterprise + EAP-TLS |
| 23 | **B** | FCC TRT-20/2024 — RADIUS mal configurado |
| 24 | **C** | FCC TRT-2/2025 — EAP-TLS pelo TLS Start |
| 25 | **C** | Papéis 802.1X |

**Notas rápidas:** 16 A (Q≠D). 17 D (×4096). 18 B (3 estados RSTP). 19 A (802.1Q). 20 B (tags). 21 D (papéis). 22 C (Enterprise+TLS). 23 B (RADIUS). 24 C (EAP-TLS). 25 C (suplicante–autenticador–servidor).

---

*Arquivo 03 · VLAN / STP / 802.1X · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 04 Wi-Fi 802.11.*

+++
title = '03 — Revisão — Redes (TCP/IP · VLAN/802.1X · Wi-Fi · serviços · monitoramento)'
date = '2026-10-29'
draft = false
weight = 3
tags = ['semana-11', 'revisao']
categories = ['estudo']
+++
# Revisão — Redes (TCP/IP · VLAN/802.1X · Wi-Fi · serviços · monitoramento) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — Semana 11 = revisão · reforço redes  
**Escopo deste arquivo:** cartões de contraste cobráveis de TCP/IP (camadas, TCP×UDP, IPv4/IPv6), DNS/DHCP/HTTP(S)/SSH/SMTP/FTP/LDAP, VLAN/802.1Q × STP/RSTP × 802.1X, Wi-Fi 802.11ac/ax/be (flash), monitoramento (Zabbix/Grafana/Prometheus/ELK + SNMP).  
**Fora de escopo aqui:** segurança profunda (ISO/LGPD/OWASP/VPN/TLS/ENSEC → `04-revisao-seguranca.md`); ágeis/ITIL/COBIT/SO/AD/IA → `05-mapa-temas-vermelhos.md`; tutorial longo de configuração de switch/AP.

**Priors:** semana 7 (`01`–`04`, `07`–`08`).

---

## 1. Material (revisão densa)

### 1.1 Cartão-mestre TCP/IP

| Camada TCP/IP | Mora aqui (prova) |
|---------------|-------------------|
| **Aplicação** | HTTP(S), DNS, DHCP, SSH, SMTP, FTP, LDAP, POP, IMAP, SNMP |
| **Transporte** | **TCP**, **UDP** |
| **Internet/Rede** | **IP**, ICMP, **IPSec** (ARP costuma cair em Rede) |
| **Interface/Enlace** | Ethernet, Wi-Fi |

Anti-confusão clássica (FCC SEGEP-MA/2018): **ARP–Rede; UDP–Transporte; LDAP–Aplicação**. Errados: FTP no transporte; SMTP na rede; SSH no transporte; UDP na aplicação.

| | TCP | UDP |
|---|-----|-----|
| Conexão | Orientado (SYN/SYN-ACK/ACK) | Sem conexão |
| Entrega | Confiável, ordenada | Best-effort |
| Uso típico | HTTP(S), SSH, SMTP, FTP, LDAP | DNS (consulta), DHCP, mídia, SNMP típico |

**IPv4** = 32 bits · privados RFC 1918 · broadcast. **IPv6** = 128 bits · **sem broadcast** · DNS **AAAA** (A = IPv4). Campo IPv6 de 20 bits para fluxo/QoS = **Flow Label** (FCC TRT-12/2023).

### 1.2 Portas e papéis (memória de prova)

| Serviço | Porta / nota |
|---------|--------------|
| HTTP / HTTPS | 80 / **443** |
| SSH | **22**/TCP |
| DNS | **53** (UDP consulta; TCP zone/grande) |
| DHCP | **67**/UDP servidor · **68**/UDP cliente |
| SMTP | **25** (envio); submission **587** |
| FTP | Controle **21** (+ dados **20** ativo clássico) |
| LDAP / LDAPS | **389** / **636** |
| SMB clássico | 445 (e 139 legado) |

**DNS:** A → IPv4; AAAA → IPv6; MX → mail; CNAME → alias.  
**DHCP IPv4 — DORA:** Discover → Offer → Request → Ack. Após OFFER o cliente manda **REQUEST**. Relay elimina DHCP em cada sub-rede (FCC TRT-17/2022). Opções cobradas: **6** = DNS; **7** = log servers (FCC TRT-15/2024).  
**SMTP × POP × IMAP:** envio × download × sincronização no servidor.  
**LDAP:** diretório hierárquico; **incorreto** clássico: “toda info de um item em um único registro” (FCC MPE-RN/2010).

### 1.3 VLAN · STP/RSTP · 802.1X (trio do edital)

| Padrão | Assunto |
|--------|---------|
| **802.1Q** | Tag VLAN no quadro Ethernet |
| **802.1D** | Bridge + STP (não é VLAN) |
| **802.1w** | RSTP (3 estados) |
| **802.1s** | MSTP (distrator) |
| **802.1X** | Controle de acesso por porta |
| **802.3af** | PoE (distrator VLAN) |

Flash FCC: VLAN ≠ 802.1D; tagging = **802.1Q** (TRT-1/2025; TRT-19/2022). Access = host/1 VLAN; Trunk = multi-VLAN tagged. Entre VLANs → **roteamento** (L3).

**STP:** Root = **menor** Bridge ID (prioridade + MAC); prioridade em **múltiplos de 4096** (default 32768). Estados STP clássicos: 5. **RSTP:** Discarding / Learning / Forwarding (3).

**802.1X:** Suplicante (cliente) · Autenticador (switch/AP) · Servidor AAA (**RADIUS**). Falha típica de autenticação → RADIUS mal configurado (FCC TRT-20/2024). 802.1X **não** identifica VLAN (isso é 802.1Q).

### 1.4 Wi-Fi flash (802.11)

| Nome comercial | IEEE | Flash |
|----------------|------|-------|
| Wi-Fi 5 | **802.11ac** | OFDM; foco 5 GHz |
| Wi-Fi 6 | **802.11ax** | **OFDMA**, MU-MIMO aprimorado, **TWT** |
| Wi-Fi 7 | **802.11be** | Densidade/latência (edital: ax/be) |

FCC TRT-1/2025: 802.11ax **não** é Wi-Fi 5; não inventar “MAM”; MIMO já existia antes — o salto cobrado é OFDMA + MU-MIMO + TWT. Segurança WPA2/WPA3 → pack segurança.

### 1.5 Monitoramento flash

| Peça | Papel cobrável |
|------|----------------|
| **SNMP** | Agente × **gerente**; OIDs/MIB |
| **Zabbix** | Gerente SNMP / monitoramento (coleta + alerta) |
| **Prometheus** | Séries temporais / métricas (pull) |
| **Grafana** | **Dashboards** (não é gerente SNMP) |
| **ELK** | Elasticsearch (índice) · Logstash (pipeline) · Kibana (viz) |

FCC TRT-1/2025: gerente SNMP exemplo = **Zabbix** (não Grafana/Kibana/Logstash/ES). Grafana + Prometheus = stack típica de métricas em tempo real (FCC PMSP/2025).

### 1.6 Checklist anti-erro (antes do simulado)

1. Camada do protocolo (UDP ≠ aplicação).  
2. DORA e 67/68 UDP (nunca TCP 67 no renew).  
3. A × AAAA; 802.1Q × 802.1D × 802.1X.  
4. Root = menor prioridade válida (×4096).  
5. Wi-Fi 6 = ax = OFDMA/TWT.  
6. Zabbix coleta; Grafana visualiza.

---

## 2. Questões de treino (1–15)

**1.** Relacionamento correto protocolo–camada TCP/IP:

A) UDP–Aplicação; IP–Transporte  
B) ARP–Aplicação; TCP–Rede  
C) LDAP–Aplicação; UDP–Transporte; IP–Rede  
D) SSH–Transporte; SMTP–Rede  
E) FTP–Transporte; DNS–Enlace  

**2.** Diferença essencial TCP × UDP:

A) Ambos orientados a conexão  
B) TCP confiável/orientado; UDP best-effort/sem conexão  
C) UDP garante ordem; TCP não  
D) UDP roteia pacotes; TCP só cifra  
E) TCP usa apenas porta 53  

**3.** Registro DNS nome → IPv6:

A) A  
B) MX  
C) CNAME  
D) AAAA  
E) SOA  

**4.** Sequência DORA correta:

A) Request → Offer → Discover → Ack  
B) Discover → Request → Offer → Ack  
C) Discover → Offer → Request → Ack  
D) Offer → Discover → Ack → Request  
E) Ack → Discover → Offer → Request  

**5.** Portas DHCP padrão:

A) 53/TCP servidor e cliente  
B) 67/UDP servidor e 68/UDP cliente  
C) 67/TCP servidor e 68/TCP cliente  
D) 80/UDP e 443/UDP  
E) 389/UDP e 636/UDP  

**6.** SMTP × IMAP, respectivamente:

A) Download × envio  
B) Envio × sincronização no servidor  
C) DNS × DHCP  
D) Tag VLAN × STP  
E) SSH × Telnet  

**7.** Padrão que permite switches identificarem VLAN no quadro:

A) 802.1D  
B) 802.1X  
C) 802.3af  
D) 802.1Q  
E) 802.1w  

**8.** RSTP (802.1w) — número típico de estados de porta cobrado:

A) Dois  
B) Três  
C) Cinco  
D) Sete  
E) Quatorze  

**9.** No 802.1X, o switch/AP atua tipicamente como:

A) Suplicante  
B) Autenticador  
C) Servidor RADIUS  
D) Root Bridge STP  
E) Agente SNMP  

**10.** IEEE 802.11ax introduz, entre outros:

A) Apenas MIMO “clássico” e nome Wi-Fi 5  
B) OFDMA, MU-MIMO aprimorado e TWT  
C) Somente PoE 802.3af  
D) Tag 802.1Q no rádio  
E) Substituição do DNS por DHCP  

**11.** Após DHCPOFFER, o cliente tipicamente envia:

A) DHCPACK  
B) DHCPDISCOVER  
C) DHCPREQUEST  
D) DHCPNACK  
E) DHCPINFORM apenas  

**12.** Campo de 20 bits no cabeçalho IPv6 ligado a fluxo/QoS:

A) Traffic Class  
B) Type of Service  
C) Next Header  
D) Flow Label  
E) Hop Limit  

**13.** Exemplo de gerente SNMP entre as opções:

A) Kibana como coletor de OID  
B) Grafana como gerente SNMP  
C) Zabbix centralizando coleta/análise de dispositivos  
D) Logstash como agente RADIUS  
E) Elasticsearch como autenticador 802.1X  

**14.** Comunicação entre VLANs distintas exige, em geral:

A) Apenas STP  
B) Apenas tagging sem roteamento  
C) Roteamento (gateway / L3)  
D) Apenas DHCP Relay  
E) Apenas WPA3  

**15.** Afirmação **incorreta** sobre LDAP:

A) Pode organizar informações em árvore hierárquica  
B) Toda informação de um item pode ser colocada em um único registro  
C) Permite segurança em ramos diferentes  
D) Porta típica 389/TCP  
E) Serve a localizar usuários/recursos  

---

### Gabarito comentado (treino 1–15)

1. **C** — LDAP app; UDP transporte; IP rede.  
2. **B** — TCP×UDP clássico.  
3. **D** — AAAA.  
4. **C** — DORA.  
5. **B** — 67/68 UDP.  
6. **B** — SMTP envia; IMAP sincroniza.  
7. **D** — 802.1Q.  
8. **B** — 3 estados RSTP.  
9. **B** — Autenticador.  
10. **B** — OFDMA/MU-MIMO/TWT.  
11. **C** — REQUEST.  
12. **D** — Flow Label.  
13. **C** — Zabbix.  
14. **C** — Roteamento inter-VLAN.  
15. **B** — Clássico “incorreto” LDAP.

---

## 3. Questões de concurso FCC (16–25)

> Questões reais com citação. Gabarito só ao final.

**16.** (FCC – SEGEP-MA/2018 – Analista Executivo – Programador de Sistemas)  
Os diversos protocolos utilizados para prover os serviços na internet e também realizar o gerenciamento da rede de computadores são mapeados nas camadas hierarquicamente distribuídas no modelo/arquitetura TCP/IP. Um correto relacionamento entre o protocolo e a respectiva camada é:

A) ARP-Rede; UDP-Transporte; LDAP-Aplicação.  
B) TCP-Transporte; FTP-Transporte; SNMP-Aplicação.  
C) ARP-Enlace; DNS-Aplicação; IMAP-Aplicação.  
D) IPSec-Rede; SMTP-Rede; POP3-Aplicação.  
E) SIP-Enlace; SSH-Transporte; DHCP-Aplicação.

**17.** (FCC – TRT-18/2023 – Analista Judiciário – TI)  
Considere as seguintes afirmações sobre os protocolos DNS e DHCPv4:

I. Por padrão, servidor DNS escuta na porta 53/TCP e o servidor DHCPv4 na 68/TCP.  
II. As solicitações de endereços IPs e resolução de endereços são encaminhadas pelos clientes por meio de endereços de multicast.  
III. Um registro DNS do tipo A vincula o nome de domínio a um endereço IPv4.  
IV. Em resposta a um DHCPOFFER, um cliente responde com uma mensagem DHCPREQUEST.

Está correto o que se afirma APENAS em

A) I, III e IV.  
B) II e III.  
C) I e II.  
D) III e IV.  
E) I, II e IV.

**18.** (FCC – DETRAN-AP/2022 – Analista de Tecnologia da Informação)  
Considere as seguintes características padrão dos protocolos da pilha TCP/IP:

1. É utilizado para envio de e-mail do cliente para o servidor.  
2. Utiliza duas portas, uma para transferência de dados e outra para controle.  
3. O servidor escuta na 67/UDP enquanto o cliente na porta 68/UDP.  
4. Permite que todas as alterações na caixa de e-mail sejam sincronizadas por todos os dispositivos conectados.

As características 1, 2, 3 e 4 estão relacionadas, respectivamente, aos protocolos

A) POP − DHCP − FTP − SMTP.  
B) SMTP − DHCP − FTP − POP.  
C) IMAP − SSH − DNS − POP.  
D) SMTP − FTP − DHCP − IMAP.  
E) POP − DHCP − DNS − IMAP.

**19.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Ao pesquisar sobre os padrões IEEE, um Analista observou, corretamente, que bridges que usam LANs virtuais (VLANs)

A) nunca fizeram parte do padrão IEEE 802.1D, mas foram especificadas em um padrão separado, o padrão IEEE 802.1Q.  
B) fazem parte do padrão IEEE 802.3.  
C) são especificadas no padrão IEEE 802.1s.  
D) nunca fizeram parte do padrão IEEE 802.1Q, mas foram especificadas em um padrão separado, o padrão IEEE 802.1D.  
E) são especificadas no padrão IEEE 802.1w.

**20.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Um Tribunal Regional do Trabalho deseja segmentar sua rede para melhorar a segurança e o desempenho. O administrador de redes decide configurar VLANs para separar o tráfego dos diferentes departamentos. Para que os switches consigam identificar corretamente os pacotes pertencentes a cada VLAN e permitir a segmentação lógica do tráfego, o padrão que deve ser implementado é o IEEE

A) 802.1Q  
B) 802.3af  
C) 802.3z  
D) 802.1D  
E) 802.1X  

**21.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
As redes sem fio (Wi-Fi) operam com base em padrões desenvolvidos pelo Institute of Electrical and Electronics Engineers (IEEE), dentro da família 802.11. Nessa família de padrões, o IEEE 802.11ax

A) continua vulnerável à interferência, pois ainda opera na faixa de 2,4 GHz, atingindo velocidade máxima de até 6,9 Gbps.  
B) introduz Multiple Input Multiple Output (MIMO), Dual-band (2,4 GHz e 5 GHz) e Multiple Amplitude Modulation (MAM).  
C) introduz Orthogonal Frequency Division Multiple Access (OFDMA), Multi-User MIMO (MU-MIMO) aprimorado e Target Wake Time (TWT).  
D) opera na frequência de 2,4 GHz e 5 GHz, com velocidade máxima de 600 Mbps.  
E) é conhecido como Wi-Fi 5 e utiliza modulação Orthogonal Frequency Division Multiplexing (OFDM) com 256-QAM (Quadrature Amplitude Modulation).

**22.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
O funcionamento do SNMP se baseia na comunicação entre agentes e gerentes. Um exemplo de gerente SNMP é o

A) Logstash, que é uma ferramenta de busca e análise de dados baseado em índices Object Identifiers (OIDs).  
B) Elasticsearch, que é um mecanismo de coleta, processamento e envio de logs para os agentes SNMP.  
C) Grafana, que é uma ferramenta para visualização e análise de dados em tempo real, focada na emissão de alertas de segurança.  
D) Zabbix, que permite centralizar a coleta e análise de dados dos dispositivos de rede.  
E) Kibana, que é uma ferramenta de visualização de dados e dashboards com funcionalidade de análise automática de logs.

**23.** (FCC – TRT-15/2024 – Analista Judiciário – TI)  
Um Analista de um Tribunal Regional do Trabalho está configurando um servidor DHCP para fornecer uma lista de servidores de nome do Domain Name System (DNS) e de servidores de logs para fins de monitoramento a serem fornecidos aos clientes da rede, tendo configurado corretamente, e respectivamente, os números de opções padrão DHCP correspondentes aos códigos

A) 4 e 6.  
B) 6 e 7.  
C) 7 e 5.  
D) 8 e 9.  
E) 4 e 5.

**24.** (FCC – TRT-17/2022 – Analista Judiciário – TI)  
O DHCP inclui os seguintes recursos para reduzir a administração de rede: – configuração TCP/IP centralizada e automatizada; – capacidade de definir configurações de TCP/IP a partir de um local central; – capacidade de atribuir um intervalo completo de valores de configuração TCP/IP adicionais por meio de opções DHCP; – tratamento eficiente de alterações de endereço IP para clientes que devem ser atualizados com frequência; e mais:

A) capacidade de diferenciação de endereçamentos IPv4 e IPv6 diretamente obtidos do serviço DNS.  
B) encaminhamento de endereços das máscaras de sub-rede automaticamente por meio do web proxy.  
C) tratamento dos endereços DNS interativamente, quando da obtenção de endereços IP do web proxy.  
D) encaminhamento de mensagens iniciais por meio de um agente de retransmissão, o que elimina a necessidade de um servidor DHCP em cada sub-rede.  
E) capacidade de atribuir endereços proxy diretamente nas áreas de carga útil dos quadros de endereçamento DHCP, de forma concatenada.

**25.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
No cabeçalho IPv6, há um campo de 20 bits utilizado para fornecer um identificador para pacotes que pertencem ao mesmo fluxo de tráfego. Esse campo pode ser usado pelos dispositivos de rede para aplicar políticas de QoS, como garantir uma determinada largura de banda, latência ou priorização para pacotes de um mesmo fluxo. Trata-se do campo

A) Type of Service.  
B) Next Flow.  
C) Traffic Flow.  
D) Traffic Class.  
E) Flow Label.

---

### Gabarito comentado (FCC 16–25)

| Q | Gab. | Nota |
|---|------|------|
| 16 | **A** | SEGEP-MA/2018 — ARP–Rede; UDP–Transporte; LDAP–Aplicação |
| 17 | **D** | TRT-18/2023 — III e IV (A + REQUEST); I/II errados |
| 18 | **D** | DETRAN-AP/2022 — SMTP, FTP, DHCP, IMAP |
| 19 | **A** | TRT-19/2022 — VLAN = 802.1Q (não 802.1D) |
| 20 | **A** | TRT-1/2025 — 802.1Q |
| 21 | **C** | TRT-1/2025 — ax = OFDMA/MU-MIMO/TWT |
| 22 | **D** | TRT-1/2025 — Zabbix gerente SNMP |
| 23 | **B** | TRT-15/2024 — opções 6 (DNS) e 7 (logs) |
| 24 | **D** | TRT-17/2022 — DHCP Relay |
| 25 | **E** | TRT-12/2023 — Flow Label 20 bits |

Distribuição concurso: A, D, D, A, A, C, D, B, D, E.

---

*Arquivo 03 · Revisão redes · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 11. Próximo sugerido: `04-revisao-seguranca.md`.*

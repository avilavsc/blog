+++
title = '02 — Revisão ativa — Redes + Segurança'
date = '2026-10-28'
draft = false
weight = 2
tags = ['semana-12', 'revisao']
categories = ['estudo']
+++
# Revisão ativa — Redes + Segurança · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Semana 12 · revisão ativa (resumos + questões dos erros)  
**Escopo deste arquivo:** resumos **ultra-focados** TCP/IP·VLAN·Wi-Fi·monitoramento + ISO/LGPD/OWASP·firewall/VPN/TLS·ENSEC; checklist de erros; treino 1–15; bloco REAL 16–25 citados.  
**Fora de escopo aqui:** PDPJ/SQL (arquivo 01); método do caderno (arquivo 03); tutorial longo de switch/AP.

**Priors:** `docs/semana-11/03`–`04`; high-yield `docs/semana-7/` e `docs/semana-8/`.  
**Modo:** resumo → checklist → treino → reteste REAL.

---

## 1. Material (revisão ativa)

### 1.1 TCP/IP — cartão-mestre

| Camada | Mora aqui |
|--------|-----------|
| **Aplicação** | HTTP(S), DNS, DHCP, SSH, SMTP, FTP, LDAP, SNMP |
| **Transporte** | **TCP**, **UDP** |
| **Internet/Rede** | **IP**, ICMP, **IPSec** (ARP costuma cair em Rede) |
| **Enlace** | Ethernet, Wi-Fi |

Anti-confusão clássica: **ARP–Rede; UDP–Transporte; LDAP–Aplicação**.

| | TCP | UDP |
|---|-----|-----|
| | Orientado, confiável | Sem conexão, best-effort |
| Uso | HTTP(S), SSH, SMTP, FTP, LDAP | DNS consulta, DHCP, SNMP típico |

**IPv4** 32 bits · **IPv6** 128 bits · **sem broadcast** · DNS **AAAA** (A = IPv4) · campo 20 bits QoS/fluxo = **Flow Label**.

### 1.2 Portas / DNS / DHCP (memória de prova)

| Serviço | Porta |
|---------|-------|
| HTTP / HTTPS | 80 / **443** |
| SSH | **22**/TCP |
| DNS | **53** (UDP consulta; TCP zone) |
| DHCP | **67**/UDP servidor · **68**/UDP cliente |
| SMTP / LDAP / LDAPS | 25 · 389 · 636 |

DNS: **A**→IPv4; **AAAA**→IPv6; MX→mail; CNAME→alias.  
DHCP **DORA:** Discover → Offer → **Request** → Ack. Relay elimina DHCP em cada sub-rede. Opções: **6**=DNS; **7**=log servers.

### 1.3 VLAN · STP · 802.1X

| Padrão | Assunto |
|--------|---------|
| **802.1Q** | Tag VLAN |
| **802.1D** | Bridge + STP (**não** VLAN) |
| **802.1w** | RSTP (3 estados) |
| **802.1X** | Controle de acesso por porta (Suplicante · Autenticador · **RADIUS**) |
| **802.3af** | PoE (distrator VLAN) |

Root Bridge = **menor** Bridge ID (prioridade ×4096 + MAC). Entre VLANs → roteamento L3.

### 1.4 Wi-Fi + monitoramento flash

| Nome | IEEE | Flash |
|------|------|-------|
| Wi-Fi 5 | **802.11ac** | OFDM; foco 5 GHz |
| Wi-Fi 6 | **802.11ax** | **OFDMA**, MU-MIMO, **TWT** |
| Wi-Fi 7 | **802.11be** | Densidade/latência |

| Peça | Papel |
|------|-------|
| **Zabbix** | Gerente SNMP (coleta + alerta) |
| **Grafana** | Dashboards (≠ gerente SNMP) |
| **Prometheus** | Métricas / séries |
| **ELK** | ES índice · Logstash pipeline · Kibana viz |

### 1.5 Segurança — contrastes cobráveis

| Norma | Flash |
|-------|-------|
| **27001** | Requisitos do **SGSI** (certificável) · política = **Alta Direção** |
| **27002** | Orientação / controles |
| **27701** | Privacidade (**PIMS**) — ≠ LGPD |
| Anexo A 2022 | **93** controles · **4** temas · **SoA** |

| Figura LGPD | Papel |
|-------------|-------|
| **Controlador** | Decide finalidades/meios |
| **Operador** | Trata em nome do controlador |
| **Encarregado** | Canal controlador ↔ titulares ↔ **ANPD** |

Agentes de tratamento = Controlador + Operador. Multa: até **2%** / **R$ 50 mi** por infração.

| Controle | FCC quer |
|----------|----------|
| **Firewall** | IP/porta/estado |
| **WAF** | HTTP: SQLi, XSS, session… |
| **IDS** | Detecta + **alerta** |
| **IPS** | Detecta + **bloqueia** |
| **DMZ** | **Zona** (não appliance) |
| **NAC** | Quem entra na rede |
| **SIEM** | Correlaciona **logs** |

VPN: **IPsec** = transporte + túnel · **AH** + **ESP**. VPN-SSL = tipicamente via **navegador HTTPS** · cifra com SSL/**TLS**. Hash = **integridade**; assinar com **privada** / verificar com **pública**.

**ENSEC-PJ** = Res. CNJ **396**/2021 (ampla SI + proteção de ativos de TI). Portaria **162** operacionaliza — não confundir com 400/443.

### 1.6 Checklist de erros típicos

**Redes**

- [ ] UDP na camada de aplicação / FTP no transporte  
- [ ] DHCP 68/TCP · DNS só TCP · DORA sem REQUEST  
- [ ] AAAA = IPv4; A = IPv6  
- [ ] VLAN = 802.1D / 802.1X / 802.3af  
- [ ] Root = maior prioridade  
- [ ] Wi-Fi 6 = ac / “MAM”  
- [ ] Grafana = gerente SNMP  

**Segurança**

- [ ] 27002 certifica sozinha; 27701 = LGPD  
- [ ] Política SI “só TI/CIO”  
- [ ] Encarregado = agente de tratamento no mesmo sentido  
- [ ] Multa 4%/R$ 100 mi  
- [ ] IDS = IPS; Firewall substitui IPS; WAF = NAC = SIEM  
- [ ] DMZ = produto/appliance  
- [ ] VPN-SSL “sempre cliente dedicado” / “exclusivamente SSL” (ignora TLS)  
- [ ] ENSEC = só requisitos funcionais operacionais / só 162  

### 1.7 Ordem nesta sessão

Resumo → checklist marcado → treino 1–15 → caderno → REAL 16–25 → gabarito só no fim.

---

## 2. Questões de treino (1–15)

**1.** Relacionamento correto protocolo–camada TCP/IP:

A) UDP–Aplicação; IP–Transporte  
B) ARP–Aplicação; TCP–Rede  
C) LDAP–Aplicação; UDP–Transporte; IP–Rede  
D) SSH–Transporte; SMTP–Rede  
E) FTP–Transporte; DNS–Enlace  

**2.** Registro DNS nome → IPv6:

A) A  
B) MX  
C) CNAME  
D) AAAA  
E) PTR apenas  

**3.** Em resposta a um DHCPOFFER, o cliente tipicamente envia:

A) DHCPDISCOVER  
B) DHCPACK  
C) DHCPREQUEST  
D) DHCPNAK  
E) DNS AAAA  

**4.** Padrão IEEE de tagging VLAN:

A) 802.1D  
B) 802.1X  
C) 802.3af  
D) 802.1Q  
E) 802.1w  

**5.** IEEE 802.11ax (Wi-Fi 6) introduz, na visão cobrada:

A) Apenas OFDM legado como salto principal  
B) OFDMA, MU-MIMO aprimorado e TWT  
C) Exclusivamente 2,4 GHz até 6,9 Gbps  
D) “MAM” como padrão IEEE  
E) Equivalência a Wi-Fi 5  

**6.** Exemplo de gerente SNMP cobrado pela FCC:

A) Kibana  
B) Logstash  
C) Grafana  
D) Zabbix  
E) Elasticsearch  

**7.** Norma de **requisitos** do SGSI:

A) 27002  
B) 27001  
C) 27701 isolada como LGPD  
D) ITIL 5  
E) OWASP Top 10  

**8.** Quem estabelece a política de SI apropriada ao propósito da organização (visão 27001 cobrada):

A) Área de TI apenas  
B) Área de Qualidade  
C) Alta Direção  
D) Gerência de Projetos  
E) CIO isolado como regra  

**9.** Agentes de tratamento na LGPD:

A) Só o Encarregado  
B) Controlador e Operador (Encarregado = canal)  
C) Titular e ANPD  
D) Só o Operador  
E) Firewall e WAF  

**10.** IDS × IPS:

A) IDS bloqueia; IPS só alerta  
B) São intercambiáveis sem diferença  
C) IDS monitora/alerta; IPS pode bloquear ativamente  
D) Ambos só filtram por porta como firewall  
E) IPS = DMZ  

**11.** Proteção típica de app web contra SQLi/XSS em runtime de rede:

A) NAC  
B) DMZ isolada sem WAF  
C) WAF  
D) Catálogo global AD  
E) Flow Label  

**12.** VPN-SSL, na leitura FCC, tipicamente:

A) Exige sempre cliente dedicado em todo dispositivo  
B) Usa exclusivamente SSL e nunca TLS  
C) Pode ser acessada via navegador com HTTPS  
D) É incompatível com dispositivos móveis  
E) Substitui o RADIUS do 802.1X  

**13.** IPsec — modos e protocolos principais:

A) Aplicação e rede; SSL e IR  
B) Transporte e túnel; AH e ESP  
C) Só túnel; apenas AH  
D) Transporte e aplicação; MPLS  
E) Só SSL VPN  

**14.** ENSEC-PJ (Res. CNJ 396) contempla, corretamente:

A) Apenas requisitos funcionais operacionais estreitos  
B) Temas amplos de SI essenciais à cibernética + segurança física/proteção de ativos de TI  
C) Só automatizar não-invasão física  
D) Substituição da LGPD  
E) Equivalência a Portaria 400  

**15.** Síntese ativa correta:

A) 802.1D = VLAN; Grafana = SNMP manager; IDS = IPS; 27701 = LGPD  
B) 802.1Q tag; DORA+67/68 UDP; ax=OFDMA; Zabbix gerente; 27001 requisitos; Controlador/Operador; IPS bloqueia; ENSEC 396 ampla  
C) AAAA = IPv4; Implicit Grant recomendado  
D) WAF = SIEM = NAC  
E) Alta Direção não define política SI  

---

### Gabarito comentado (treino 1–15)

1. **C** — LDAP app; UDP transp.; IP rede.  
2. **D** — AAAA.  
3. **C** — REQUEST após OFFER.  
4. **D** — 802.1Q.  
5. **B** — OFDMA/MU-MIMO/TWT.  
6. **D** — Zabbix.  
7. **B** — 27001.  
8. **C** — Alta Direção.  
9. **B** — Agentes + Encarregado canal.  
10. **C** — Alerta × bloqueio.  
11. **C** — WAF.  
12. **C** — Via HTTPS/navegador.  
13. **B** — Transporte/túnel; AH/ESP.  
14. **B** — I e III do enunciado clássico.  
15. **B** — Síntese.

Letras: C, D, C, D, B, D, B, C, B, C, C, C, B, B, B.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** com citação. Reteste de erros de redes/segurança. Gabarito só no final.

**16.** (FCC – SEGEP-MA/2018 – Analista de Sistemas)  
Na arquitetura TCP/IP, a associação correta entre protocolo e camada é:

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

**18.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Ao pesquisar sobre os padrões IEEE, um Analista observou, corretamente, que bridges que usam LANs virtuais (VLANs)

A) nunca fizeram parte do padrão IEEE 802.1D, mas foram especificadas em um padrão separado, o padrão IEEE 802.1Q.  
B) fazem parte do padrão IEEE 802.3.  
C) são especificadas no padrão IEEE 802.1s.  
D) nunca fizeram parte do padrão IEEE 802.1Q, mas foram especificadas em um padrão separado, o padrão IEEE 802.1D.  
E) são especificadas no padrão IEEE 802.1w.

**19.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Um Tribunal Regional do Trabalho deseja segmentar sua rede para melhorar a segurança e o desempenho. O administrador de redes decide configurar VLANs para separar o tráfego dos diferentes departamentos. Para que os switches consigam identificar corretamente os pacotes pertencentes a cada VLAN e permitir a segmentação lógica do tráfego, o padrão que deve ser implementado é o IEEE

A) 802.1Q  
B) 802.3af  
C) 802.3z  
D) 802.1D  
E) 802.1X  

**20.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
O funcionamento do SNMP se baseia na comunicação entre agentes e gerentes. Um exemplo de gerente SNMP é o

A) Logstash, que é uma ferramenta de busca e análise de dados baseado em índices Object Identifiers (OIDs).  
B) Elasticsearch, que é um mecanismo de coleta, processamento e envio de logs para os agentes SNMP.  
C) Grafana, que é uma ferramenta para visualização e análise de dados em tempo real, focada na emissão de alertas de segurança.  
D) Zabbix, que permite centralizar a coleta e análise de dados dos dispositivos de rede.  
E) Kibana, que é uma ferramenta de visualização de dados e dashboards com funcionalidade de análise automática de logs.

**21.** (FCC · TRT-19/2022 · Analista Judiciário — TI)  
Segundo a norma ABNT NBR ISO/IEC 27001:2013, quem deve estabelecer uma política de segurança da informação que seja apropriada ao propósito da organização é:

A) Área de Tecnologia da Informação.  
B) Área de Qualidade.  
C) Alta Direção.  
D) Gerência de Projetos.  
E) Chief Information Officer (CIO).  

**22.** (FCC · TRF4/2025 · Analista — Análise de Sistemas)  
No âmbito da LGPD (Lei Geral de Proteção de Dados), existem as figuras de Controlador, Operador e Encarregado. O papel de tais figuras e a relação entre elas no tratamento de dados pessoais e a Autoridade Nacional de Proteção de Dados (ANPD), de acordo com LGPD, podem ser realizados por dois agentes de tratamento, o

A) Controlador e o Operador, enquanto o Encarregado é a pessoa indicada pelo controlador e operador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
B) Controlador e o Encarregado, enquanto o Operador é a pessoa indicada pelo encarregado e controlador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
C) Encarregado e o Operador, enquanto o controlador é a pessoa indicada pelo Encarregado e operador para atuar como canal de comunicação entre o operador, os titulares dos dados e a ANPD.  
D) Controlador e o Operador, enquanto o Encarregado é a pessoa indicada pelo titular e operador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
E) Controlador e o Encarregado, enquanto o Encarregado é a pessoa indicada pelo controlador e titulares para atuar como canal de comunicação entre o controlador, o encarregado dos dados e a ANPD.  

**23.** (FCC · TRT-1/2025 · Analista — TI)  
Um Tribunal Regional do Trabalho precisa reforçar a segurança da informação em seus sistemas, garantindo a proteção dos dados processuais e a continuidade dos serviços digitais. A equipe de TI promoveu um debate sobre a utilização de um Sistema de Prevenção de Intrusão (IPS) comparando-o a outras soluções. Eles concluíram corretamente que o

A) Firewall de borda substitui a necessidade de um IPS, pois sua função é suficiente para evitar todos os tipos de ataques e filtrar o tráfego malicioso antes que ele chegue aos servidores internos.  
B) Sniffer de Rede é uma ferramenta mais eficaz do que um IPS, pois permite capturar e analisar pacotes de dados sem causar impacto na latência da rede do Tribunal.  
C) IPS analisa o tráfego de rede em tempo real, identificando padrões suspeitos e bloqueando automaticamente atividades maliciosas antes que comprometam os sistemas do Tribunal, não sendo equivalente a um IDS, que apenas alerta sobre ameaças sem intervir ativamente.  
D) WAF ( Web Application Firewall ) analisa registros de eventos e detecta anomalias de segurança em tempo real, além de bloquear ataques automaticamente, sendo uma alternativa ao IPS.  
E) NAC ( Network Access Control ) é uma solução equivalente ao IPS, pois impede qualquer acesso não autorizado à rede do Tribunal, monitorando continuamente os dispositivos conectados.  

**24.** (FCC · TRT-12/2023 · Analista — TI)  
A VPN-SSL é um tipo específico de VPN que

A) é mais adequada em ambientes que exigem configurações avançadas e controle granular sobre políticas de segurança e recursos de rede.  
B) utiliza exclusivamente o protocolo SSL ( Secure Sockets Layer ) para estabelecer uma conexão segura entre o cliente e o servidor.  
C) requer, geralmente, a instalação de um software cliente dedicado em cada dispositivo para ser utilizada.  
D) não é compatível com dispositivos móveis e tablets , pois necessita de software cliente no dispositivo desktop para sua utilização.  
E) pode ser acessada através de um navegador web padrão, usando o protocolo HTTPS.  

**25.** (FCC · TRT-5/2022 · Analista Judiciário — TI)  
Considere o contexto da Resolução no 396 de 07/06/2021. A ENSEC-PJ prevista nesta Resolução contempla: I. temas relacionados à segurança da informação, de forma ampla, que sejam essenciais para segurança cibernética. II. segurança lógica e proteção de dados específicos do nível operacional, nos aspectos relacionados aos requisitos funcionais. III. segurança física e proteção de ativos de tecnologia da informação de forma geral; IV. ações destinadas a assegurar, de forma automatizada, a não invasão física de localidades e de informações. Está correto o que consta APENAS de

A) I e II.  
B) I e III.  
C) II, III e IV.  
D) III.  
E) II e IV.  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Citação |
|---|------|---------|
| 16 | **A** | FCC \| SEGEP-MA 2018 \| Analista de Sistemas |
| 17 | **D** | FCC \| TRT-18 2023 \| Analista TI |
| 18 | **A** | FCC \| TRT-19 2022 \| Analista TI |
| 19 | **A** | FCC \| TRT-1 2025 \| Analista TI \| VLAN 802.1Q |
| 20 | **D** | FCC \| TRT-1 2025 \| Analista TI \| Zabbix |
| 21 | **C** | FCC \| TRT-19 2022 \| Analista TI \| Alta Direção |
| 22 | **A** | FCC \| TRF4 2025 \| Analista Análise de Sistemas \| LGPD |
| 23 | **C** | FCC \| TRT-1 2025 \| Analista TI \| IPS≠IDS |
| 24 | **E** | FCC \| TRT-12 2023 \| Analista TI \| VPN-SSL |
| 25 | **B** | FCC \| TRT-5 2022 \| Analista TI \| ENSEC I e III |

Letras: A, D, A, A, D, C, A, C, E, B.

**Fontes (bases de prova / Quizinho):** enunciados alinhados aos packs de revisão das semanas 7–8 e 11 (`03-revisao-redes.md`, `04-revisao-seguranca.md`) e às páginas Quizinho correspondentes (TRT-1/2025 Q57 VLAN; TRT-19/2022 Q54 VLAN; TRT-18/2023 DNS/DHCP; TRF4/2025 Q37 LGPD; etc.).

---

*Arquivo 02 · Revisão ativa redes+segurança · Semana 12 · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: `03-caderno-erros-metodologia.md`.*

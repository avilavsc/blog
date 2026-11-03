+++
title = '07 — SNMP, MIBs e QoS'
date = '2026-10-21'
draft = false
weight = 7
tags = ['semana-07', 'snmp', 'qos']
categories = ['estudo']
+++
# SNMP, MIBs e QoS · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 07 — SNMP · MIBs · QoS  
**Escopo deste arquivo:** **SNMP** (gerente × agente, GET/SET/TRAP, versões v1/v2c/v3), **MIB** (árvore, OID), noções de métricas/desempenho e **QoS** (IntServ × DiffServ, DSCP, EF/AF/CS/BE, marcação VoIP × controle de rede).  
**Fora de escopo aqui:** ferramentas Zabbix/Grafana/Prometheus/ELK a fundo (arquivo 08 — mas Zabbix como gerente SNMP aparece); proxy/Apache/WildFly (05–06); criptografia TLS profunda (semana 8).

---

## 1. Material

### 1.1 Por que cai junto

Edital: **Gerenciamento de redes: SNMP, MIBs; QoS; métricas e avaliação de desempenho** (+ ferramentas no arquivo 08).

A FCC separa bem:

| Bloco | Pergunta mental |
|-------|-----------------|
| SNMP | Quem pergunta? Quem responde? O que é trap? |
| MIB | O que se gerencia? É protocolo? |
| QoS | Como priorizar VoIP/controle sob congestionamento? |

### 1.2 SNMP — papéis

**SNMP (Simple Network Management Protocol)** gerencia dispositivos (roteadores, switches, servidores, impressoras…) via modelo:

| Papel | Função |
|-------|--------|
| **Gerente (NMS)** | Consulta (GET), altera (SET), recebe alertas; consolida visão da rede |
| **Agente** | Roda no dispositivo gerenciado; responde consultas; pode emitir TRAP/Inform |
| **MIB** | “Catálogo” dos objetos gerenciáveis (não é o protocolo) |

Frase FCC TRT-24/2016 (gabarito):

> No SNMP o item a ser monitorado ou gerenciado é um **agente**. Quem consulta (GET) ou solicita modificações (SET) é um **gerente**. O agente também gera alertas (**TRAP**).

Exemplo de gerente cobrado em TRT-1/2025: **Zabbix** (centraliza coleta/análise). **Não** são gerentes SNMP: Logstash, Elasticsearch, Grafana, Kibana (papéis de log/visualização — arquivo 08).

### 1.3 Operações SNMP

| Operação | Direção típica | Uso |
|----------|----------------|-----|
| **GET** / GetNext / GetBulk | Gerente → Agente | Ler objeto(s) |
| **SET** | Gerente → Agente | Alterar objeto gravável |
| **TRAP** | Agente → Gerente | Alerta assíncrono (não “solicitado” na hora) |
| **Inform** (v2+) | Agente → Gerente com ACK | Alerta com confirmação |

Armadilha: chamar o **gerente** de “agente” ou dizer que TRAP é enviado pelo NMS ao dispositivo.

Portas clássicas: **UDP 161** (agente escuta GETs/SETs); **UDP 162** (gerente escuta TRAPs). Números caem em provas de redes.

### 1.4 Versões: v1, v2c, v3

| Versão | Autenticação típica | Segurança |
|--------|---------------------|-----------|
| **SNMPv1** | Community string (texto claro) | Fraca |
| **SNMPv2c** | Community (ainda texto claro); GetBulk | Melhorias operacionais, **sem** cripto forte |
| **SNMPv3** | Usuário + auth/priv (USM); VACM | Autenticação e opcionalmente **criptografia** |

Flash de prova (CiscoView TRT-15/2024 e clones):

- Comunicação **segura** → pense **SNMPv3**, não v1.  
- v2c **não** implementa o pacote de segurança do v3 (cripto + controle fino).

Communities clássicas `public`/`private` em produção = mau cheiro ( Trocar + restringir ACL + preferir v3).

### 1.5 MIB e OID

**MIB (Management Information Base)** =

> estrutura **hierárquica** de objetos gerenciáveis e suas definições.

Gabarito literal FCC TRT-20/2024 (alternativa E). MIB **não** é:

- protocolo de comunicação (isso é SNMP);  
- algoritmo de compressão;  
- arquitetura de balanceamento;  
- conjunto de regras de criptografia.

**OID (Object Identifier)** = caminho numérico na árvore (ex.: `1.3.6.1.2.1.1.5` — sysName sob MIB-II). OID **≠** endereço IP.

MIB-II (RFC clássica) — objetos básicos (system, interfaces, ip, icmp, tcp, udp…). Fabricantes adicionam **MIBs privadas** (enterprise).

Ferramentas de NMS (ex.: CiscoView) usam **pacotes de dispositivo** para interpretar MIBs/capacidades de cada modelo (TRT-15/2024).

### 1.6 Métricas e avaliação de desempenho

SNMP alimenta métricas cobráveis:

| Métrica | Exemplo via SNMP/ideia |
|---------|------------------------|
| Disponibilidade | uptime, status de interface ifOperStatus |
| Utilização | octetos in/out na interface → % de banda |
| Erros | ifInErrors / ifOutErrors |
| Recursos de host | CPU, memória (MIBs de host/vendor) |

Ciclo de gerenciamento (visão FCAPS leve): falha, configuração, contabilização, desempenho, segurança — SNMP brilha em **desempenho** e **falha** (traps).

Política boa: baseline → limiar → alarme → ticket. Sem baseline, todo alarme vira ruído.

### 1.7 QoS — o problema

**QoS (Quality of Service)** = conjunto de mecanismos para tratar classes de tráfego de forma **diferenciada** (latência, jitter, perda, banda), especialmente sob **congestionamento**.

Sem QoS: fila FIFO / best effort — VoIP sofre igual a download de ISO.

Requisitos típicos:

| Tráfego | Sensível a |
|---------|------------|
| VoIP | Latência, jitter, perda |
| Vídeo interativo | Banda + jitter |
| Bulk transfer | Mais tolerante; quer throughput |
| Controle de rede (roteamento, etc.) | Deve sobreviver ao congestionamento |

### 1.8 IntServ × DiffServ

| Modelo | Ideia | Escalabilidade |
|--------|-------|----------------|
| **IntServ** | Reserva por fluxo (RSVP); caminho sinalizado | Pouco escalável em backbone grande |
| **DiffServ** | Classifica/marca pacotes; PHB por classe | Escala melhor — o cobrado em redes corporativas |

DiffServ marca o campo **DSCP** (6 bits) no IP (evolução do ToS). Roteadores aplicam **PHB (Per-Hop Behavior)**.

### 1.9 Classes DiffServ cobráveis

| Código / classe | Uso mental |
|-----------------|------------|
| **EF (Expedited Forwarding)** | Baixa latência/jitter — **VoIP** |
| **AF (Assured Forwarding)** | Classes com garantia relativa (AFxy); drop precedence |
| **CS (Class Selector)** | Compatível com precedência IP; **CS6/CS7** altos — **controle de rede** |
| **BE (Best Effort / DF)** | Sem garantia especial |

**Questão ouro FCC TRT-17/2022:** priorizar VoIP e controle de rede → marcações **EF** e **CS6**, respectivamente.

Distratores: CS1/CS2 (baixos), AF51 (classe AF inválida na numeração usual 1–4), BE para VoIP (péssimo).

### 1.10 Ferramentas de QoS na caixa (visão)

Sem virar curso Cisco:

1. **Classificação / marcação** (DSCP, CoS 802.1p em VLAN).  
2. **Filas** (LLQ/priority queue para EF).  
3. **Policing** — corta/excede taxa.  
4. **Shaping** — suaviza envio.  
5. **Congestion avoidance** (WRED etc.).

CoS (L2) × DSCP (L3): em redes 802.1Q, mapear CoS↔DSCP nas bordas.

### 1.11 SNMP × QoS — ligação de prova

- SNMP **monitora** se filas/interfaces saturam, drops sobem, latência de path (indireto).  
- QoS **age** no encaminhamento.  
- Não diga que MIB “é” DiffServ ou que EF é OID.

### 1.12 Anti-confusão rápida

| Afirmação | Certo? |
|-----------|--------|
| MIB = protocolo | Não |
| SNMP = MIB | Não (protocolo × base de info) |
| Zabbix pode ser gerente SNMP | Sim |
| Grafana é gerente SNMP | Não (TRT-1/2025) |
| VoIP → EF | Sim |
| Controle de rede → CS6 | Sim |
| SNMPv1 seguro por padrão | Não |
| TRAP sai do agente | Sim |

### 1.13 Mapa mental

```
[Gerente NMS: Zabbix etc.]
        │  GET/SET (UDP 161)
        │  TRAP  (UDP 162)
        ▼
[Agente] ── lê/escreve ──► [MIB / OIDs]

QoS DiffServ: classifica → marca DSCP → PHB (EF/AF/CS/BE)
VoIP: EF · Controle: CS6 · Bulk: BE/AF baixa
```

---

## 2. Questões de treino (1–15)

**1.** No modelo SNMP, quem tipicamente envia GET/SET é:

A) O agente apenas
B) O gerente (NMS)
C) Exclusivamente o Kibana
D) O DocumentRoot
E) O DSCP EF

**2.** TRAP SNMP é:

A) Consulta periódica do gerente ao agente
B) Arquivo de configuração do IIS
C) Classe DiffServ
D) Alerta assíncrono tipicamente do **agente** ao gerente
E) Sinônimo de GETBULK

**3.** A MIB é corretamente definida como:

A) Protocolo de troca entre servidores  
B) Estrutura hierárquica de objetos gerenciáveis e definições  
C) Arquitetura de balanceamento  
D) Conjunto de algoritmos de compressão  
E) Regras de criptografia para dados em trânsito  

**4.** OID, em SNMP/MIB:

A) É sempre um endereço IPv4
B) Substitui o TCP
C) É a community string
D) É a marcação EF
E) Identifica objeto na árvore hierárquica da MIB

**5.** SNMPv3 destaca-se em relação a v1/v2c principalmente por:

A) Recursos de autenticação/privacidade (segurança) bem mais robustos
B) Usar apenas community em texto claro como único mecanismo
C) Eliminar a necessidade de agentes
D) Proibir TRAPs
E) Ser idêntico ao HTTP

**6.** Portas UDP clássicas agente × traps:

A) 80 e 443
B) 25 e 587
C) 161 e 162
D) 9990 e 8080
E) 53 e 53

**7.** Exemplo de **gerente** SNMP cobrado pela FCC TRT-1/2025:

A) Logstash como indexador de OIDs  
B) Elasticsearch coletando SET nos agentes  
C) Grafana como gerente SNMP nativo  
D) Zabbix, centralizando coleta/análise de dispositivos  
E) Kibana emitindo GETBULK obrigatório  

**8.** QoS DiffServ baseia-se sobretudo em:

A) Reserva RSVP por cada microfluxo em todo o backbone (IntServ puro)
B) Apenas NAT
C) Somente SNMP SET
D) Classificação/marcação (DSCP) e PHB por classes
E) DocumentRoot priorizado

**9.** Marcação recomendada na FCC para **VoIP** em DiffServ:

A) BE  
B) EF  
C) CS1  
D) AF51  
E) CS2  

**10.** Marcação recomendada na mesma prova para **controle de rede**:

A) BE  
B) EF apenas  
C) CS6  
D) AF53  
E) CS1  

**11.** IntServ, face ao DiffServ em backbones grandes, é visto como:

A) Menos escalável por reserva por fluxo
B) Sempre mais escalável
C) Independente de RSVP historicamente
D) Idêntico ao SNMPv3
E) Apenas uma MIB

**12.** ifInOctets / ifOutOctets em MIB de interfaces servem para estimar:

A) Community strings
B) DocumentRoot
C) DSCP exclusivamente
D) Senha do WildFly
E) Utilização/tráfego na interface

**13.** Community `public` sem restrição em SNMPv2c na borda é:

A) Risco (leitura ampla em texto claro)
B) Melhor prática obrigatória
C) Equivalente a SNMPv3 com priv
D) Necessária para EF
E) Arquivo do Apache

**14.** Policing × shaping (visão):

A) São o mesmo que TRAP
B) Ambos são OIDs
C) Só existem no IIS
D) Policing descarta/excede; shaping atrasa/suaviza para caber na taxa
E) Substituem o DNS

**15.** Síntese SNMP + QoS:

A) MIB é o protocolo; EF monitora agentes
B) Zabbix é PHB
C) TRAP marca DSCP
D) CS6 é community string
E) SNMP gerencia via gerente/agente/MIB; QoS DiffServ prioriza classes (VoIP→EF, controle→CS6)


---

### Gabarito comentado (treino 1–15)

1. **B** — Gerente.  
2. **D** — Trap do agente.  
3. **B** — Definição TRT-20/2024.  
4. **E** — Identificador na árvore.  
5. **A** — Segurança.  
6. **C** — 161/162.  
7. **D** — Zabbix (TRT-1/2025).  
8. **D** — DiffServ por classes.  
9. **B** — EF.  
10. **C** — CS6.  
11. **A** — Escalabilidade.  
12. **E** — Tráfego/utilização.  
13. **A** — Risco.  
14. **D** — Police vs shape.  
15. **E** — Cartão mental completo.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** (FCC – TRT-20/2024 – Analista Judiciário – TI)  
A MIB — Management Information Base é

A) uma arquitetura para balanceamento de carga em redes distribuídas  
B) uma composição de regras de criptografia para dados em trânsito  
C) um protocolo de comunicação para troca de dados entre servidores  
D) um conjunto de algoritmos de compressão de pacotes de dados  
E) uma estrutura hierárquica de objetos gerenciáveis e suas definições  

**17.** (FCC – TRT-24/2016 – Analista Judiciário – TI)  
Sobre monitoramento/gerência, é correto afirmar:

A) O Zabbix não permite monitoramento agentless  
B) Como o servidor Zabbix é obrigatoriamente Unix/Linux, não há agentes para Windows  
C) O ICMP, como TCP e UDP, transmite dados de aplicação e o ping é exclusivo do Linux  
D) No SNMP o item monitorado/gerenciado é um agente; quem consulta (GET) ou solicita modificações (SET) é um gerente; o agente também gera alertas (TRAP)  
E) No IPTables, `-s ALL` aplica-se simultaneamente a SSH, HTTP e HTTPS sem regra por protocolo  

**18.** (FCC – TRT-17/2022 – Analista Judiciário – TI)  
Para priorizar telefonia VoIP e controle de rede em congestionamento, as marcações DiffServ recomendadas, respectivamente, são:

A) CS1 e CS2  
B) AF51 e AF53  
C) EF e CS6  
D) BE e A41  
E) BE e EF  

**19.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
O funcionamento do SNMP baseia-se na comunicação entre agentes e gerentes. Um exemplo de gerente SNMP é o

A) Logstash, ferramenta de busca/análise baseada em índices OID  
B) Elasticsearch, mecanismo de coleta/processamento/envio de logs aos agentes SNMP  
C) Grafana, ferramenta de visualização em tempo real focada em alertas de segurança  
D) Zabbix, que permite centralizar a coleta e análise de dados dos dispositivos de rede  
E) Kibana, visualização/dashboards com análise automática de logs  

**20.** (FCC – TRT-15/2024 – Analista Judiciário – TI)  
Sobre o CiscoView no gerenciamento de roteadores/switches, o Analista deve saber que ele

A) utiliza SNMPv1 para comunicação segura  
B) é instalado em cada dispositivo e o pacote de dispositivo no CiscoWorks Client  
C) é aplicação autônoma sem integração com CiscoWorks  
D) utiliza pacotes de dispositivo para compreender definições de MIBs e funcionalidades de cada tipo de dispositivo  
E) utiliza SNMPv2 para criptografia e políticas avançadas de acesso  

**21.** (Estilo FCC)  
Assinale a opção que distingue corretamente SNMP e MIB:

A) SNMP é a base hierárquica; MIB é o protocolo GET/SET  
B) SNMP é o protocolo de gerência; MIB descreve os objetos gerenciáveis  
C) Ambos são classes DiffServ  
D) MIB escuta na UDP 162 obrigatoriamente  
E) SNMP é apenas uma community string  

**22.** (Estilo FCC)  
Em DiffServ, Best Effort (BE) para um fluxo de VoIP institucional sensível a jitter:

A) É a marcação ideal, equivalente a EF  
B) Impõe CS7 automaticamente  
C) Configura o agente SNMP  
D) Substitui RSVP em IntServ  
E) É inadequado; VoIP tipicamente exige tratamento tipo EF  

**23.** (Estilo FCC)  
GetBulk no SNMP associa-se historicamente de modo mais forte a:

A) Melhoria operacional a partir de SNMPv2 (eficiente para muitos objetos)  
B) Apenas SNMPv1 sem extensões  
C) Exclusivamente QoS EF  
D) Arquivo host.xml  
E) ProxyPass  

**24.** (Estilo FCC — métricas)  
Um NMS observa crescimento sustentado de `ifInErrors` em uplink. A leitura operacional mais adequada é:

A) Ignorar, pois MIB não serve a desempenho  
B) Marcar o link como EF automaticamente resolve  
C) Investigar problemas de camada física/enlace/erros na interface  
D) Trocar apenas a community para `private`  
E) Desligar traps  

**25.** (Estilo FCC — síntese)  
Para o edital (SNMP, MIBs, QoS, métricas), a síntese correta é:

A) Gerente/agente/MIB via SNMP; OID na árvore; DiffServ marca DSCP (VoIP EF, controle CS6); métricas de interface/host avaliam desempenho  
B) MIB = Zabbix; QoS = apenas NAT; traps saem do gerente  
C) SNMPv1 é obrigatório para segurança  
D) CS6 é compressão de pacotes  
E) EF é um tipo de TRAP  


---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **E** | FCC TRT-20/2024 — definição de MIB |
| 17 | **D** | FCC TRT-24/2016 — agente/gerente/GET/SET/TRAP |
| 18 | **C** | FCC TRT-17/2022 — EF e CS6 |
| 19 | **D** | FCC TRT-1/2025 — Zabbix como gerente |
| 20 | **D** | FCC TRT-15/2024 — device packages / MIBs |
| 21 | **B** | Protocolo × base de objetos |
| 22 | **E** | VoIP ≠ BE |
| 23 | **A** | GetBulk no v2+ |
| 24 | **C** | Erros de interface |
| 25 | **A** | Síntese do eixo |

**Notas:** 16 E. 17 D. 18 C. 19 D (ELK/Grafana ≠ gerente SNMP). 20 D (v1 inseguro; v2c sem cripto forte). 21 B. 22 B. 23 B. 24 B. 25 B.

---

*Arquivo 07 · SNMP / MIB / QoS · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 08 Monitoramento (Zabbix, Grafana, Prometheus, ELK).*

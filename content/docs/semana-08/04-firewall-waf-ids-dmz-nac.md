+++
title = '04 — Firewall, WAF, IDS/IPS, DMZ e NAC'
date = '2026-10-26'
draft = false
weight = 4
tags = ['semana-08', 'firewall', 'waf']
categories = ['estudo']
+++
# Firewall, WAF, IDS/IPS, DMZ e NAC · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — Firewall, WAF, UTM, IDS/IPS, DMZ, NAC  
**Escopo deste arquivo:** papéis e distinções cobráveis entre firewall de rede, WAF, UTM, IDS × IPS, arquitetura DMZ e NAC (controle de admissão); encaixe com Proxy/antivírus/antispam do edital.  
**Fora de escopo aqui:** VPN/ZTNA (arquivo 05); criptografia/TLS (arquivo 06); WPA2/3 (arquivo 07); ISO 27001/LGPD/OWASP (outros packs da semana 8); proxy Apache/IIS profundos (semana 7).

---

## 1. Material

### 1.1 Por que esse pacote cai junto

O edital TRT4 lista, no mesmo bloco de segurança de redes: **firewall, WAF, UTM, DMZ, Proxy, NAC, antivírus, antispam; IDS/IPS**. A FCC cobra **troca de papéis**: WAF ≠ IDS; IPS ≠ firewall de borda “puro”; DMZ ≠ produto; NAC ≠ IPS; UTM ≠ WAF.

Cartão mental (prova):

| Controle | O que a FCC espera |
|----------|--------------------|
| **Firewall (rede/borda)** | Filtra por regras (IP, porta, estado); camada rede/transporte tipicamente |
| **WAF** | Protege **aplicação web** (HTTP/HTTPS): SQLi, XSS, session hijack, buffer overflow em app |
| **UTM** | “Caixa única” — firewall + AV + IDS/IPS + filtragem etc. |
| **IDS** | Detecta e **alerta** (passivo / sem bloquear por padrão) |
| **IPS** | Detecta e **bloqueia** em tempo real (ativo) |
| **DMZ** | **Zona** (arquitetura), não appliance — serviços públicos isolados |
| **NAC** | Controla **quem/o que** entra na rede (postura, autenticação, conformidade) |
| **SIEM** | Centraliza/correlaciona **logs** — não é WAF nem firewall |

### 1.2 Firewall de rede

**Firewall** aplica política de filtragem entre zonas de confiança (Internet ↔ DMZ ↔ LAN). Em prova:

- Regras clássicas: origem/destino IP, porta, protocolo; **stateful** lembra conexões estabelecidas.  
- Não substitui IPS (não “entende” todo padrão de ataque de conteúdo) nem WAF (não é especialista em HTTP de aplicação).  
- **NGFW** (next-gen): inspeção mais profunda, aplicação, identidade — ainda assim, WAF permanece o especialista de app web.

**Anti-confusão FCC (TRT-1/2025):** firewall de borda **não** elimina a necessidade de IPS; são **complementares**.

### 1.3 WAF — Web Application Firewall

O **WAF** inspeciona requisições/respostas **HTTP/HTTPS** em busca de ataques à **camada de aplicação** (OWASP: SQLi, XSS, CSRF, path traversal, etc.).

**Flash FCC TRT-5/2022:** mitigar injeção SQL, sequestro de sessão, alteração de URL e estouro de buffer no servidor web/e-mail → **WAF** (não NAC, IDS, DMZ nem Proxy genérico).

**Flash FCC TRF-4/2025:** SIEM = correlacionar logs; WAF = proteger app (SQLi/XSS); Firewall = restringir por IP/porta.

Posicionamento típico: à frente do servidor web (ou como serviço cloud) — muitas vezes **proxy reverso especializado**. Proxy “puro” (cache/anonimato) **não** é, por si só, a resposta certa para SQLi/XSS.

### 1.4 UTM × WAF × DMZ

**UTM (Unified Threat Management):** appliance que **consolida** várias funções (firewall, antivírus, filtragem de conteúdo/e-mail, IDS/IPS…). Foco: proteção **abrangente da rede** em uma caixa.

**WAF:** foco **exclusivo** (na leitura de prova) em **aplicativos web**.

**DMZ (Demilitarized Zone):** sub-rede física ou lógica entre Internet e rede interna. Hospeda serviços **expostos** (web, e-mail, DNS público). Isola: se o servidor da DMZ for comprometido, o atacante ainda esbarra em firewalls antes da LAN.

**Flash FCC TRT-12/2023:** UTM protege a rede com funções integradas; WAF protege apps web; DMZ é **camada adicional de isolamento** — não é proxy reverso e não fica “redundante” só porque há UTM/WAF.

### 1.5 IDS × IPS × sniffer

| | IDS | IPS |
|---|-----|-----|
| Ação | Detecta + **alerta** | Detecta + **bloqueia** |
| Posição típica | Espelho/TAP (fora do caminho) ou inline em modo alerta | Em linha (**inline**) no fluxo |
| Relação | IPS ≠ IDS: “não equivalente” (FCC TRT-1/2025) | Complementa firewall |

Métodos: assinaturas, anomalia/heurística, comportamento. Base de assinaturas **precisa atualizar** (distrator: “não precisa atualizar”).

**Sniffer:** captura/análise passiva de pacotes — útil forense/monitoramento; **não** previne ataques como IPS.

### 1.6 NAC — Network Access Control

**NAC** decide se um dispositivo **pode entrar** na rede e com que perfil: autenticação (ex.: 802.1X/RADIUS), verificação de postura (AV atualizado, patch, disk encryption), quarentena/VLAN de remediação.

- NAC **≠** IPS: NAC é **admissão**; IPS é **tráfego em andamento**.  
- NAC **≠** WAF: não inspeciona SQLi no HTTP do portal público.  
- Liga com **AAA/RBAC** e 802.1X (semana 7) — em WLAN Enterprise, o “gate” de acesso e o NAC corporativo se conversam.

### 1.7 Proxy, antivírus, antispam (mapa do edital)

- **Proxy:** intermediário (forward/reverse); pode filtrar URL, cache; reverse proxy ≠ WAF automático.  
- **Antivírus / antimalware:** endpoint ou gateway — assinaturas/heurística de malware.  
- **Antispam:** e-mail (SPF/DKIM/DMARC + filtros).  
No UTM, vários desses módulos convivem; na prova, se o enunciado fala **SQLi/XSS em app web**, a resposta é **WAF**.

### 1.8 Arquitetura típica de tribunal (visão cobrável)

Internet → Firewall/UTM de borda → **DMZ** (portal, e-mail edge) com **WAF** na frente do web → outro firewall → LAN (AD, apps internos, BD). **IPS** no caminho crítico. **NAC** na admissão cabeada/Wi-Fi. **SIEM** correlaciona logs de todos.

### 1.9 Stateful, proxy e UTM no mesmo desenho

Em bancas, “firewall stateful” significa lembrar o estado da conexão (ex.: resposta a um TCP já permitido). Isso **não** vira inspeção profunda de SQL no body HTTP — daí o WAF. Já o **proxy** (forward) intermedia clientes internos para a Internet (URL filtering, cache); o **reverse proxy** recebe a Internet em nome do servidor. Um WAF costuma ser um reverse proxy **com** regras de app; um reverse proxy “burro” não herda automaticamente essas regras.

**Antivírus/antimalware** no gateway ou no host complementam, mas não segmentam zonas como a DMZ nem controlam admissão como o NAC. Em prova: se o verbo for **segmentar/isolar serviço público**, pense DMZ; se for **bloquear padrão de ataque no tráfego**, IPS/WAF; se for **permitir ou não o endpoint**, NAC.

### 1.10 Distratores favoritos da FCC

1. Trocar WAF ↔ IDS ↔ NAC ↔ DMZ.  
2. Dizer que firewall “substitui” IPS.  
3. Tratar DMZ como produto que “bloqueia SQLi”.  
4. Dizer que UTM e WAF fazem a mesma coisa.  
5. Confundir SIEM (logs) com WAF (HTTP app).  
6. Tratar sniffer como IPS “mais eficaz”.

### 1.11 Mini-roteiro de escolha (15 segundos na prova)

1. Enunciado fala SQLi/XSS/sessão HTTP? → **WAF**.  
2. Fala alertar vs bloquear intrusão na rede? → **IDS** vs **IPS**.  
3. Fala zona entre Internet e LAN? → **DMZ**.  
4. Fala conformidade do dispositivo na entrada? → **NAC**.  
5. Fala caixa única com FW+AV+IPS? → **UTM**.  
6. Fala correlacionar logs? → **SIEM** (não está no título deste arquivo, mas cai como distrator).

---

## 2. Questões de treino (1–15)

**1.** Solução especializada em inspecionar HTTP/HTTPS contra SQLi e XSS:

A) NAC  
B) DMZ  
C) WAF  
D) Sniffer  
E) OSPF  

**2.** Zona que isola servidores públicos da LAN:

A) DMZ  
B) WAF  
C) IPS  
D) RADIUS  
E) STP  

**3.** Diferença cobrada IDS × IPS:

A) IDS bloqueia; IPS só alerta  
B) Ambos são sinônimos rigorosos de WAF  
C) IDS exige UTM; IPS dispensa assinaturas  
D) IPS só existe em Wi-Fi  
E) IPS analisa e pode bloquear; IDS tipicamente alerta sem intervir  

**4.** UTM, em relação ao WAF:

A) UTM só protege camada 7 web; WAF protege toda a rede  
B) São o mesmo produto com nomes comerciais  
C) UTM substitui a necessidade de DMZ sempre  
D) UTM integra várias funções de rede; WAF concentra-se em apps web  
E) WAF é obrigatoriamente um módulo de roteamento OSPF  

**5.** Controle de admissão por postura/autenticação do dispositivo:

A) WAF  
B) SNMP  
C) MPLS  
D) CDN  
E) NAC  

**6.** Firewall de borda com regras IP/porta:

A) filtra tráfego segundo política; não elimina o valor de IPS/WAF  
B) substitui completamente o IPS  
C) é equivalente ao SIEM  
D) só opera na camada de apresentação OSI  
E) é o nome técnico de DMZ  

**7.** Centralizar e correlacionar logs de segurança:

A) SIEM  
B) WAF  
C) VLAN nativa  
D) EAP-TLS  
E) QoS  

**8.** Sniffer de rede, frente a um IPS:

A) é mais eficaz para bloquear ataques em tempo real  
B) substitui o WAF em SQLi  
C) captura/analisa; não previne como IPS  
D) é o padrão IEEE 802.1X  
E) define a DMZ automaticamente  

**9.** Servidor web voltado à Internet, arquitetura clássica:

A) deve ficar na LAN sem segmentação  
B) só pode existir se houver WPA3  
C) tipicamente na DMZ, com firewalls controlando o acesso  
D) dispensa qualquer firewall se houver antivírus no PC do usuário  
E) deve usar apenas NAC sem firewall  

**10.** Ataques listados: SQLi, session hijack, URL tampering, buffer overflow em app web. Solução de rede apontada pela FCC:

A) NAC  
B) IDS apenas passivo sem WAF  
C) DMZ (como “produto”)  
D) Proxy genérico sem inspeção de app  
E) WAF  

**11.** NAC é equivalente ao IPS?

A) Sim, ambos só alertam  
B) Sim, ambos só fazem SQLi  
C) Não: NAC controla acesso/admissão; IPS age sobre tráfego malicioso em curso  
D) Sim, ambos são zonas de rede  
E) Sim, ambos são hashes  

**12.** Antispam no mapa do edital:

A) substitui o WAF no portal HTTPS  
B) foca filtragem de e-mail indesejado/malicioso  
C) elege a root bridge  
D) é o modo túnel do IPsec  
E) é sinônimo de DMZ  

**13.** NGFW em relação ao WAF:

A) NGFW torna o WAF proibido por norma  
B) podem coexistir; WAF segue especialista em app web  
C) NGFW é apenas antivírus de desktop  
D) WAF opera só na camada física  
E) NGFW = exclusivamente NAC  

**14.** Assinaturas de IDS/IPS:

A) nunca precisam atualização  
B) devem ser mantidas atualizadas para cobrir ameaças conhecidas  
C) são proibidas em UTM  
D) só existem em WEP  
E) substituem certificados TLS  

**15.** Síntese alinhada ao edital para este subtópico:

A) Firewall/UTM filtram a rede; WAF protege app web; IDS alerta e IPS bloqueia; DMZ isola serviços públicos; NAC controla admissão  
B) DMZ autentica usuários; NAC cifra HTTP; WAF elege VLAN  
C) IPS = apenas sniffer; IDS sempre bloqueia  
D) UTM e WAF são zonas L2 idênticas  
E) Firewall substitui SIEM, WAF, NAC e DMZ  

---

### Gabarito comentado (treino 1–15)

1. **C** — WAF.  
2. **A** — DMZ.  
3. **E** — IPS ativo; IDS alerta.  
4. **D** — UTM amplo; WAF app.  
5. **E** — NAC.  
6. **A** — complementar.  
7. **A** — SIEM.  
8. **C** — passivo.  
9. **C** — web na DMZ.  
10. **E** — WAF (TRT-5/2022).  
11. **C** — papéis distintos.  
12. **B** — e-mail.  
13. **B** — coexistência.  
14. **B** — atualizar assinaturas.  
15. **A** — cartão mental.

---

## 3. Questões de concurso FCC (16–25)

**16.** (FCC · TRT-5/2022 · Técnico — TI)  
Com o objetivo mitigar ataques contra ameaças de aplicações web, tais como, injeção de SQL, sequestro de sessão, alteração de URL e estouro de buffer , um técnico instalou uma solução de segurança baseada em rede para proteção do servidor web e e-mail voltados para internet. A solução de segurança que é capaz de realizar tal tarefa é o

A) WAF.  
B) NAC.  
C) IDS.  
D) DMZ.  
E) Proxy .  

**17.** (FCC · TRT-1/2025 · Analista — TI)  
Um Tribunal Regional do Trabalho precisa reforçar a segurança da informação em seus sistemas, garantindo a proteção dos dados processuais e a continuidade dos serviços digitais. A equipe de TI promoveu um debate sobre a utilização de um Sistema de Prevenção de Intrusão (IPS) comparando-o a outras soluções. Eles concluíram corretamente que o

A) Firewall de borda substitui a necessidade de um IPS, pois sua função é suficiente para evitar todos os tipos de ataques e filtrar o tráfego malicioso antes que ele chegue aos servidores internos.  
B) Sniffer de Rede é uma ferramenta mais eficaz do que um IPS, pois permite capturar e analisar pacotes de dados sem causar impacto na latência da rede do Tribunal.  
C) IPS analisa o tráfego de rede em tempo real, identificando padrões suspeitos e bloqueando automaticamente atividades maliciosas antes que comprometam os sistemas do Tribunal, não sendo equivalente a um IDS, que apenas alerta sobre ameaças sem intervir ativamente.  
D) WAF ( Web Application Firewall ) analisa registros de eventos e detecta anomalias de segurança em tempo real, além de bloquear ataques automaticamente, sendo uma alternativa ao IPS.  
E) NAC ( Network Access Control ) é uma solução equivalente ao IPS, pois impede qualquer acesso não autorizado à rede do Tribunal, monitorando continuamente os dispositivos conectados.  

**18.** (FCC · TRT-12/2023 · Analista — TI)  
Em uma reunião relativa à segurança da informação, uma Analista do Tribunal Regional do Trabalho foi solicitada a apresentar as diferenças entre os sistemas de proteção UTM e WAF, e explicar como esses se relacionam com uma DMZ. A Analista afirmou, corretamente, que o UTM

A) oferece proteção exclusiva para o tráfego de entrada, enquanto o WAF se concentra no tráfego de saída. A DMZ age como um proxy reverso para inspecionar todo o tráfego antes que este alcance a rede interna.  
B) e o WAF agem de forma conjunta e centralizada, oferecendo recursos como antivírus, filtragem de conteúdo, filtragem de e-mail , antispam etc., enquanto a DMZ fornece segregação entre redes internas e externas.  
C) é projetado para proteger apenas a camada de aplicação das ameaças da web , enquanto o WAF abrange todas as ameaças de rede. Havendo esses sistemas de segurança implantados, a DMZ ficaria como uma proteção redundante.  
D) é voltado para a proteção de toda a rede, incluindo funções de firewall , antivírus e IDS/IPS, enquanto o WAF se concentra exclusivamente em proteger aplicativos web . A DMZ atua como uma camada adicional de isolamento entre a rede interna e a externa.  
E) e o WAF oferecem mecanismos de proteção à rede interna contra ameaças internas, enquanto a DMZ lida principalmente com a filtragem de tráfego externo não autorizado, devendo ser utilizados em conjunto.  

**19.** (FCC · TRF4/2025 · Analista — Análise de Sistemas)  
Um Analista precisa implementar controles de segurança para proteger uma infraestrutura que inclui servidores web, banco de dados e comunicação com a nuvem. Controle 1: Centralizar e correlacionar logs de eventos de segurança de diferentes dispositivos para identificar possíveis incidentes. Controle 2: Proteger o aplicativo web contra ataques como SQL Injection e Cross-Site Scripting (XSS). Controle 3: Restringir o acesso a serviços internos com base em regras de endereços IP e portas. Os controles 1, 2 e 3 se referem, respectivamente, ao

A) WAF – IDS – IPS.  
B) WAF – IPS – SIEM.  
C) IPS – SIEM – IDS.  
D) SIEM – WAF – Firewall.  
E) IDS – Firewall – WAF.  

**20.** (FCC · TRT-20/2024 · Técnico — TI)  
Um Técnico de redes recomendou a implementação de um Web Application Firewall (WAF) para proteger os sistemas web da organização contra ataques maliciosos. Uma característica de um WAF que o diferencia de outros tipos de firewalls , como o Next-Generation Firewall (NGFW), é

A) a prevenção contra exploração de vulnerabilidades em protocolos de rede.  
B) o monitoramento de tentativas de login inválidas e ataques de força bruta.  
C) a detecção e o bloqueio de tráfego malicioso em portas específicas.  
D) a análise e o bloqueio de tráfego com base no conteúdo HTTP/HTTPS.  
E) a proteção contra ataques DoS ( Denial of Service ) e DDoS ( Distributed Denial of Service ).  

**21.** (FCC · TRT-20/2024 · Técnico — TI)  
Durante uma auditoria de segurança, foi identificado que um dispositivo não autorizado conseguiu acessar a rede. O Técnico de rede suspeita que houve uma falha no processo de autenticação IEEE 802.1X, que pode ter sido causada por

A) falta de um firewall na rede.  
B) configuração incorreta do servidor RADIUS.  
C) uso de um switch não gerenciável.  
D) falta de criptografia WPA2.  
E) configuração de VLANs inadequada.  

**22.** (FCC · SEFAZ-SC/2018 · TI)  
Considere os seguintes controles da política de segurança estabelecida em uma empresa: I. Controlar o acesso de pessoas às áreas em que se encontram os servidores computacionais da empresa. II. Bloquear acesso dos funcionários para sites inseguros da internet. III. Instalar Firewall para controlar os acessos externos para a rede local da empresa. Os controles mencionados são, respectivamente, tipificados como de Segurança

A) Física, Lógica e Lógica.  
B) Lógica, Lógica e Lógica.  
C) Física, Lógica e Física.  
D) Lógica, Lógica e Física.  
E) Física, Física e Lógica.  

**23.** (FCC · TRT-5/2022 · Analista — TI)  
Um analista entrou em um site e desejava saber se este utilizava conexão segura. Para isto clicou em um ícone de cadeado ao lado da URL no navegador, a partir do qual obteve informações sobre

A) a assinatura digital, tais como chave pública, chave privada e autorização de acesso.  
B) o certificado digital, tais como chave secreta, hash público e período de validade.  
C) o certificado digital, tais como emissão (para e por quem), período de validade e assinaturas digitais.  
D) a assinatura eletrônica, tais como assinaturas digitais, contraprova e autorização de acesso.  
E) o hash criptográfico, tais como validade do certificado digital, redirecionamentos seguros e hash público.  

**24.** (FCC · TRF4/2025 · Analista — Análise de Sistemas)  
Uma Analista está implementando um sistema seguro de troca de mensagens via HTTPS e tem dúvidas sobre os conceitos de criptografia e a infraestrutura de chaves públicas (PKI). Ao discutir com outro analista, um deles esclareceu adequadamente que

A) a criptografia assimétrica, exemplo RSA, é usada para cifrar toda a comunicação diretamente, pois é mais rápida que a simétrica, enquanto os certificados digitais servem para autenticar o cliente.  
B) certificados digitais dispensam o uso de criptografia assimétrica, pois já incluem chaves simétricas pré-compartilhadas, reduzindo a complexidade da PKI.  
C) tanto a criptografia simétrica quanto a assimétrica são usadas para autenticação, enquanto os certificados digitais garantem a confidencialidade através da PKI.  
D) a criptografia simétrica é utilizada para cifrar o conteúdo da mensagem; a criptografia assimétrica é utilizada para criar a chave simétrica de forma segura; e os certificados digitais validam a identidade do servidor por meio de uma Autoridade Certificadora (AC).  
E) a PKI elimina a necessidade de criptografia simétrica, uma vez que as chaves públicas da criptografia assimétrica são suficientes para proteger toda a comunicação com desempenho ideal.  

**25.** (FCC · PMSP/2025 · Analista TIC)  
A Prefeitura de São Paulo está hipoteticamente desenvolvendo um sistema integrado de segurança urbana, chamado Segurança 360°, que conecta câmeras de vigilância, sensores de tráfego e dispositivos IoT em áreas públicas. O objetivo é monitorar em tempo real eventos críticos, como congestionamentos, atos de vandalismo e emergências médicas. Esse sistema manipula dados sensíveis, como informações de placas de veículos, padrões de movimento de pedestres e eventos registrados por câmeras. Para proteger o sistema contra ataques cibernéticos, a equipe técnica decidiu implementar uma arquitetura Zero Trust (ZTA) com práticas DevSecOps . Para viabilizar o sistema Segurança 360°, o consenso foi optar por

A) armazenar dados de sensores IoT e câmeras em servidores centralizados, para facilitar a administração e garantir controle total de acesso, e utilizar Virtual Private Networks (VPNs) para controlar o acesso às redes e aos aplicativos.  
B) um modelo de segurança baseado na proteção de perímetro de rede, que estabelece alguns macroperímetros em torno de ativos protegidos, usa mecanismos de segurança como 2FA e possibilita o acesso a aplicativos e serviços com base em autorização por voz do usuário.  
C) priorizar o uso de firewalls de perímetro robustos e reduzir o foco em autenticação e controle granular para acelerar o tráfego entre dispositivos. Desta forma, os invasores ficam impedidos de comprometer as credenciais do usuário e obter acesso aos sistemas locais por trás do firewall .  
D) cancelar serviços que usam modelos de implantação híbridos ou multi-nuvem , dispositivos não gerenciados ou aplicativos de SaaS. Em todos esses casos, há recursos que estão fora do controle direto, inviabilizando soluções ZTA, uma vez que podem comprometer as políticas e práticas de segurança.  
E) implementar soluções que segmentam a rede para restringir o movimento lateral e conter os infiltradores, como ZTNA e Cloud Security Access Broker (CASB). Incluir MFA para todos os acessos, automação no pipeline CI/CD com scanners de vulnerabilidade e realizar monitoramento contínuo utilizando ferramentas SIEM.  

---

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| Questão | Gabarito | Fonte / nota |
|---|---|---|
| 16 | **A** | WAF |
| 17 | **C** | IPS ≠ IDS |
| 18 | **D** | UTM × WAF × DMZ |
| 19 | **D** | SIEM – WAF – Firewall |
| 20 | **D** | WAF analisa HTTP/HTTPS |
| 21 | **B** | RADIUS/802.1X |
| 22 | **A** | Física × lógica |
| 23 | **C** | Certificado no navegador |
| 24 | **D** | HTTPS híbrido |
| 25 | **E** | Zero Trust / ZTNA |
*Arquivo 04 · Firewall / WAF / IDS-IPS / DMZ / NAC · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 05 VPN / ZTNA.*

*Arquivo 04 · Firewall / WAF / IDS-IPS / DMZ / NAC · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 05 VPN / ZTNA.*

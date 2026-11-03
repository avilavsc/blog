+++
title = '05 — VPN, VPN-SSL e ZTNA'
date = '2026-10-27'
draft = false
weight = 5
tags = ['semana-08', 'vpn', 'ztna']
categories = ['estudo']
+++
# VPN, VPN-SSL e ZTNA · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — VPN, VPN-SSL/TLS, ZTNA  
**Escopo deste arquivo:** conceitos de VPN; IPsec (AH/ESP, túnel × transporte, site-to-site × acesso remoto); VPN-SSL/TLS (HTTPS/navegador); contraste com ZTNA (confiança zero, acesso a recurso específico).  
**Fora de escopo aqui:** firewall/WAF/DMZ/NAC profundos (arquivo 04); detalhes de cifras/handshake TLS 1.3 (arquivo 06 — só o necessário para VPN-SSL); WPA (arquivo 07).

---

## 1. Material

### 1.1 VPN — ideia de prova

**VPN (Virtual Private Network)** cria canal lógico **privado** sobre rede **pública** (Internet), tipicamente com **tunelamento + criptografia/autenticação**, de modo que o tráfego se comporte como se estivesse em rede corporativa.

Usos cobráveis:

- **Site-to-site:** une duas redes (sede ↔ fórum / datacenter) via gateways.  
- **Acesso remoto (client-to-site):** usuário externo conecta ao gateway corporativo.

Não confundir VPN com **MPLS** do provedor (encaminhamento/labels — não é “VPN de navegador”) nem com **SSH** (acesso remoto a shell, não o modelo genérico de VPN corporativa completa).

### 1.2 IPsec — AH, ESP, modos

**IPsec** é suíte de segurança na **camada de rede (IP)**. Protocolos cobrados:

| Protocolo | Função na leitura FCC |
|-----------|------------------------|
| **AH (Authentication Header)** | Autenticação e integridade do pacote; **não** oferece confidencialidade (não cifra o payload) |
| **ESP (Encapsulating Security Payload)** | **Criptografia** (confidencialidade) + tipicamente autenticação/integridade |

Vantagem clássica ESP × AH: **ESP oferece encriptação; AH não**.

**Modos:**

| Modo | O que protege | Cabeçalho |
|------|---------------|-----------|
| **Transporte** | Sobretudo a **carga útil** do IP original | Mantém IP original visível (em linhas gerais de prova) |
| **Túnel** | **Pacote IP inteiro** encapsulado | **Novo** cabeçalho IP + cabeçalhos IPsec |

**Flash FCC TRT-17/2022:** tráfego atrás de firewalls de borda; precisar de cabeçalho de autenticação **e** de criptografia; pacote original cifrado/autenticado com **novo** cabeçalho IP → modo **túnel**, protocolos **AH e ESP** (nessa ordem no enunciado: autenticação e criptografia).

Site-to-site com redes atrás de firewalls casa naturalmente com **túnel**. Acesso remoto também pode usar IPsec com **cliente dedicado**.

IKE/IKEv2 (negociação de SAs) aparece como contexto; foque AH/ESP e túnel/transporte na primeira leitura.

### 1.3 VPN-SSL / VPN-TLS

Apesar do nome comercial **VPN-SSL**, o protocolo vigente é **TLS** (sucessor do SSL). Em prova:

- Tráfego entre **navegador** e dispositivo SSL VPN cifra com **SSL ou TLS**.  
- **Flash FCC TRT-19/2022:** navegador ↔ SSL VPN → criptografia com SSL **ou TLS** (gabarito TLS quando a alternativa contrapõe IPSec/MPLS/SSH/OSPF).  
- **Flash FCC TRT-12/2023:** VPN-SSL **pode ser acessada por navegador padrão via HTTPS** — característica distintiva (modo *clientless*).  
- **Flash FCC SEFAZ-SC/2018:** VPN acessível por navegador típico → **VPN SSL** (não IPsec/PPTP/MPLS/L2TP).

Distratores:

- “Utiliza **exclusivamente** SSL” — falso: hoje é TLS; “exclusivamente” é pegadinha.  
- “Sempre exige cliente dedicado” — falso como regra geral (clientless via browser).  
- “Incompatível com móveis” — falso.

Comparativo cobrável:

| | IPsec | VPN-SSL/TLS |
|---|-------|-------------|
| Camada | Rede (IP) | Sessão/transporte (TLS), acesso via HTTPS |
| Cliente | Frequentemente software/config OS | Navegador (*clientless*) ou cliente leve |
| Uso clássico | Site-to-site / full tunnel | Acesso remoto a apps/intranet |
| Granularidade | Rede / túneis amplos | Mais simples para apps web |

### 1.4 ZTNA — Zero Trust Network Access

**ZTNA** aplica o princípio **zero trust**: **não confiar por padrão** (deny-by-default). Acesso a **aplicativo/recurso específico** só **depois** de autenticar usuário **e** avaliar contexto (dispositivo, postura, localização, hora…).

Diferença central × VPN tradicional (discurso de prova / CEBRASPE TRT-8 e clones cobráveis):

- **VPN clássica:** após autenticação, usuário tende a obter **acesso amplo à rede** (lateral movement vira risco).  
- **ZTNA:** concede acesso **só ao app/serviço** autorizado, em túnel criptografado; reduz superfície de ataque; apps podem ficar **ocultos** (não expostos com IP público amplo).

ZTNA **não** é sinônimo de PAM (privilégios) nem de SIEM (logs) — são famílias distintas do mesmo ecossistema de segurança moderna.

### 1.5 Encaixe no Judiciário / edital

Edital: **VPN, VPN-SSL, ZTNA**. Ordem mental:

1. Precisa unir redes ou full-tunnel L3 → pense **IPsec** (túnel, ESP/AH).  
2. Usuário remoto via browser HTTPS → **VPN-SSL/TLS**.  
3. Acesso por identidade/contexto a app específico, deny default → **ZTNA**.

Complementa NAC (admissão LAN) e MFA/AAA do edital: ZTNA reforça identidade contínua; VPN-SSL ainda pode exigir MFA no portal.

### 1.6 Split-tunnel, full-tunnel e política

Em acesso remoto, **full-tunnel** manda todo o tráfego do cliente pelo gateway corporativo (mais controle, mais carga). **Split-tunnel** manda só destinos corporativos pela VPN (melhor UX, maior risco se o endpoint estiver comprometido). A FCC raramente cobra o termo inglês, mas a ideia de “todo o tráfego” vs “só recursos internos” ajuda a ler ZTNA: ZTNA é ainda mais fino — **por aplicativo**, não “metade da Internet”.

IKEv2 aparece em VPNs IPsec modernas (negociação de associações de segurança). Para o nível analista TRT4, priorize **AH/ESP** e **túnel/transporte**; se aparecer IKE, trate como fase de estabelecimento, não como cifra do payload.

### 1.7 Armadilhas

1. AH cifra dados (não).  
2. Modo transporte = novo IP externo (não — isso é túnel).  
3. VPN-SSL “só SSL, nunca TLS”.  
4. ZTNA = VPN que libera a LAN inteira.  
5. MPLS/OSPF como “protocolo de cifra do navegador”.  
6. Confundir VPN-SSL clientless com obrigatoriedade de cliente IPsec.

### 1.8 Mini-roteiro de escolha

1. Novo cabeçalho IP + AH + ESP? → **túnel IPsec**.  
2. Browser + HTTPS no portal? → **VPN-SSL/TLS**.  
3. Negar por padrão + app específico + contexto? → **ZTNA**.  
4. ESP vs AH e “quem cifra”? → **ESP**.

---

## 2. Questões de treino (1–15)

**1.** VPN, em essência cobrável:

A) canal privado sobre rede pública, em geral com túnel e proteção criptográfica  
B) protocolo de roteamento interno como OSPF  
C) apenas antivírus de gateway  
D) padrão IEEE de VLAN tagging  
E) sinônimo rigoroso de DMZ  

**2.** Protocolo IPsec que oferece criptografia do payload:

A) AH  
B) OSPF  
C) STP  
D) ESP  
E) CDP  

**3.** AH em relação ao ESP:

A) AH cifra e ESP nunca autentica  
B) São hashes SHA apenas  
C) AH só existe em WPA3  
D) ESP é exclusivo de camada de aplicação HTTP  
E) AH não oferece confidencialidade; ESP oferece encriptação  

**4.** Modo túnel IPsec caracteriza-se por:

A) proteger só a porta TCP 80 sem novo IP  
B) abolir autenticação  
C) encapsular o pacote original e adicionar novo cabeçalho IP  
D) usar apenas WEP  
E) ser idêntico ao modo transporte em todos os aspectos  

**5.** Site-to-site típico:

A) é apenas e-mail SMTP aberto  
B) exige obrigatoriamente WPA2-Personal  
C) é o nome do certificado raiz ICP  
D) une duas redes via gateways  
E) substitui DNS  

**6.** Navegador ↔ dispositivo SSL VPN: protocolo de cifra cobrado além do nome SSL:

A) OSPF  
B) MPLS  
C) RIP  
D) IGMP  
E) TLS  

**7.** Vantagem típica da VPN-SSL na FCC:

A) incompatibilidade total com tablets  
B) obrigatoriedade de cliente IPsec full sempre  
C) uso exclusivo de AH sem ESP  
D) acesso via navegador padrão usando HTTPS  
E) substituição do Active Directory  

**8.** Afirmação incorreta sobre VPN-SSL:

A) pode operar em modo clientless  
B) hoje apoia-se em TLS  
C) facilita acesso remoto a recursos internos  
D) aparece no edital TRT4 junto de VPN e ZTNA  
E) requer sempre, em todos os casos, cliente dedicado desktop e é incompatível com móveis  

**9.** ZTNA parte de qual postura?

A) deny-by-default / zero trust, liberando recurso específico após identidade e contexto  
B) trust-all na LAN após VPN  
C) open Wi-Fi sem cifra  
D) só AH sem autenticação  
E) broadcast storm control  

**10.** Diferença ZTNA × VPN tradicional:

A) ZTNA sempre libera a rede inteira; VPN só um app  
B) São o mesmo produto com dois stickers  
C) ZTNA proíbe criptografia  
D) VPN exige obrigatoriamente WEP  
E) VPN costuma ampliar acesso à rede; ZTNA restringe a apps/recursos autorizados  

**11.** MPLS, no distrator de VPN via browser:

A) é técnica de encaminhamento; não é a VPN clientless por browser  
B) é o protocolo HTTPS do navegador  
C) substitui TLS 1.3  
D) é o nome do ESP  
E) é o padrão 802.1X  

**12.** Requisitos: autenticação + criptografia IPsec; pacote original encapsulado com novo IP →

A) transporte, só AH  
B) SSL exclusivo sem IPsec  
C) WPA2-PSK  
D) túnel, AH e ESP  
E) SNMP v1  

**13.** PAM e SIEM em relação a ZTNA:

A) são sinônimos literais de ZTNA  
B) substituem IPsec AH  
C) famílias distintas (privilégios / logs) no ecossistema, não definem ZTNA sozinhas  
D) são modos do ESP  
E) são cifras RC4  

**14.** Acesso remoto de magistrado via portal HTTPS do tribunal, sem cliente IPsec:

A) só é possível com AH puro  
B) exige obrigatoriamente MPLS no notebook  
C) é função exclusiva de STP  
D) é WEP  
E) casa com VPN-SSL/TLS  

**15.** Síntese do subtópico:

A) ZTNA = AH; VPN-SSL = OSPF; IPsec = apenas antispam  
B) Túnel IPsec proíbe novo cabeçalho IP  
C) TLS nunca aparece em VPN-SSL  
D) IPsec (AH/ESP, túnel/transporte) para VPNs L3; VPN-SSL/TLS via HTTPS/browser; ZTNA com acesso granular zero trust  
E) Site-to-site só existe em Wi-Fi aberto  

---

### Gabarito comentado (treino 1–15)

1. **A** — túnel privado.  
2. **D** — ESP.  
3. **E** — AH sem cifra.  
4. **C** — novo IP.  
5. **D** — redes via gateway.  
6. **E** — TLS (TRT-19/2022).  
7. **D** — browser HTTPS.  
8. **E** — falsa (clientless/móveis).  
9. **A** — zero trust.  
10. **E** — escopo de acesso.  
11. **A** — MPLS ≠ VPN browser.  
12. **D** — túnel AH+ESP.  
13. **C** — famílias distintas.  
14. **E** — VPN-SSL.  
15. **D** — mapa.

---

## 3. Questões de concurso FCC (16–25)

**16.** (FCC · TRT-17/2022 · Analista — TI)  
Com o objetivo de oferecer disponibilidade de serviço em caso de falha do link principal, o analista de tecnologia da informação do TRT precisa estabelecer uma VPN IPSec, conforme o cenário e os requisitos apresentados abaixo: O tráfego de interesse, em ambos os lados da VPN, está localizado atras do firewall de borda entre a rede local e a internet. A VPN deve utilizar um cabeçalho de autenticação e outro para criptografia. Na VPN estabelecida, todo pacote original deve ser criptografado e autenticado, e um novo cabeçalho IP e o cabeçalho IPSec devem ser adicionados. Com base no cenário e nos requisitos apresentados acima, o tipo de VPN e os protocolos de autenticação e criptografia envolvidos, respectivamente, são modo

A) site-to-site, SHA-256 e AH.  
B) transporte, ESP e AH.  
C) acesso remoto, AH e RSA.  
D) SSL, AES-256 e RADIUS.  
E) túnel, AH e ESP.  

**17.** (FCC · TRT-19/2022 · Analista — TI)  
O IPSec pode ser usado nos modos

A) aplicação e rede e, para proteger a comunicação via rede, utiliza, principalmente, o cabeçalho de autenticação (AH) e a carga de dados de segurança ( payload ).  
B) aplicação e túnel e, para proteger a comunicação via rede, utiliza, principalmente, o protocolo Secure Sockets Layer (SSL) e a carga de empacotamento (ESP).  
C) transporte e túnel e, para proteger a comunicação via rede, utiliza, principalmente, o cabeçalho de autenticação (AH) e a carga de empacotamento (ESP).  
D) transporte e aplicação e, para proteger a comunicação via rede, utiliza, principalmente, o cabeçalho inter-rede (IR) e a carga de empacotamento (ESP).  
E) transporte e rede e, para proteger a comunicação via rede utiliza, principalmente, o cabeçalho inter-rede (IR), o protocolo Secure Sockets Layer (SSL) e a carga de dados de segurança ( payload ).  

**18.** (FCC · TRT-12/2023 · Analista — TI)  
A VPN-SSL é um tipo específico de VPN que

A) é mais adequada em ambientes que exigem configurações avançadas e controle granular sobre políticas de segurança e recursos de rede.  
B) utiliza exclusivamente o protocolo SSL ( Secure Sockets Layer ) para estabelecer uma conexão segura entre o cliente e o servidor.  
C) requer, geralmente, a instalação de um software cliente dedicado em cada dispositivo para ser utilizada.  
D) não é compatível com dispositivos móveis e tablets , pois necessita de software cliente no dispositivo desktop para sua utilização.  
E) pode ser acessada através de um navegador web padrão, usando o protocolo HTTPS.  

**19.** (FCC · PMSP/2025 · Analista TIC)  
A Prefeitura de São Paulo está hipoteticamente desenvolvendo um sistema integrado de segurança urbana, chamado Segurança 360°, que conecta câmeras de vigilância, sensores de tráfego e dispositivos IoT em áreas públicas. O objetivo é monitorar em tempo real eventos críticos, como congestionamentos, atos de vandalismo e emergências médicas. Esse sistema manipula dados sensíveis, como informações de placas de veículos, padrões de movimento de pedestres e eventos registrados por câmeras. Para proteger o sistema contra ataques cibernéticos, a equipe técnica decidiu implementar uma arquitetura Zero Trust (ZTA) com práticas DevSecOps . Para viabilizar o sistema Segurança 360°, o consenso foi optar por

A) armazenar dados de sensores IoT e câmeras em servidores centralizados, para facilitar a administração e garantir controle total de acesso, e utilizar Virtual Private Networks (VPNs) para controlar o acesso às redes e aos aplicativos.  
B) um modelo de segurança baseado na proteção de perímetro de rede, que estabelece alguns macroperímetros em torno de ativos protegidos, usa mecanismos de segurança como 2FA e possibilita o acesso a aplicativos e serviços com base em autorização por voz do usuário.  
C) priorizar o uso de firewalls de perímetro robustos e reduzir o foco em autenticação e controle granular para acelerar o tráfego entre dispositivos. Desta forma, os invasores ficam impedidos de comprometer as credenciais do usuário e obter acesso aos sistemas locais por trás do firewall .  
D) cancelar serviços que usam modelos de implantação híbridos ou multi-nuvem , dispositivos não gerenciados ou aplicativos de SaaS. Em todos esses casos, há recursos que estão fora do controle direto, inviabilizando soluções ZTA, uma vez que podem comprometer as políticas e práticas de segurança.  
E) implementar soluções que segmentam a rede para restringir o movimento lateral e conter os infiltradores, como ZTNA e Cloud Security Access Broker (CASB). Incluir MFA para todos os acessos, automação no pipeline CI/CD com scanners de vulnerabilidade e realizar monitoramento contínuo utilizando ferramentas SIEM.  

**20.** (FCC · TRT-20/2024 · Analista — TI)  
Uma organização frequentemente troca e-mails sensíveis entre seus funcionários e clientes. Para garantir que os e-mails enviados não sejam alterados e para autenticar os remetentes, a equipe de TI decidiu implementar uma solução de assinatura digital para e-mails corporativos, que também assegure a integridade e a autenticidade das mensagens. Nesse contexto, a solução mais adequada para proteger e-mails com assinatura digital é o uso de

A) AES-GCM.  
B) HTTP.  
C) PGP-IMAP.  
D) S/MIME.  
E) VPN.  

**21.** (FCC · TRF4/2025 · Analista — Análise de Sistemas)  
Uma Analista está implementando um sistema seguro de troca de mensagens via HTTPS e tem dúvidas sobre os conceitos de criptografia e a infraestrutura de chaves públicas (PKI). Ao discutir com outro analista, um deles esclareceu adequadamente que

A) a criptografia assimétrica, exemplo RSA, é usada para cifrar toda a comunicação diretamente, pois é mais rápida que a simétrica, enquanto os certificados digitais servem para autenticar o cliente.  
B) certificados digitais dispensam o uso de criptografia assimétrica, pois já incluem chaves simétricas pré-compartilhadas, reduzindo a complexidade da PKI.  
C) tanto a criptografia simétrica quanto a assimétrica são usadas para autenticação, enquanto os certificados digitais garantem a confidencialidade através da PKI.  
D) a criptografia simétrica é utilizada para cifrar o conteúdo da mensagem; a criptografia assimétrica é utilizada para criar a chave simétrica de forma segura; e os certificados digitais validam a identidade do servidor por meio de uma Autoridade Certificadora (AC).  
E) a PKI elimina a necessidade de criptografia simétrica, uma vez que as chaves públicas da criptografia assimétrica são suficientes para proteger toda a comunicação com desempenho ideal.  

**22.** (FCC · TRT-20/2024 · Técnico — TI)  
Muitas organizações implementam sistemas de autenticação remota que utilizam criptografia simétrica e assimétrica para proteger o tráfego de dados entre os servidores e usuários. O tráfego pode ser criptografado com AES, enquanto a autenticidade dos usuários é verificada usando algoritmos de criptografia assimétrica em certificados digitais. Para autenticação de usuários, são utilizados algoritmos como

A) S/MIME ou ChaCha256.  
B) AES-CERT ou AES-GCM.  
C) SHA-256 ou SHA-512.  
D) SSL ou TLS.  
E) RSA ou ECDSA.  

**23.** (FCC · SEFAZ-SC/2018 · TI)  
O estabelecimento e a implantação da segurança da informação requer o uso de diferentes mecanismos e recursos para atender os atributos de segurança da informação. Nesse contexto, o Hash tem a função de

A) realizar a transformação reversível da informação de forma a torná-la ininteligível a terceiros.  
B) criar uma chave criptográfica a partir do certificado digital.  
C) comprovar a identidade do emissor de um documento.  
D) atestar a validade de um documento.  
E) garantir a integridade de um documento.  

**24.** (FCC · TRT-6/2025 · Analista — TI)  
Uma Analista de nome Maria Cecília criptografa uma mensagem com sua chave privada e envia pela internet para João Miguel, que recebe a mensagem e a descriptografa com a chave pública de Maria Cecília para ler a mensagem. O uso da criptografia nesse caso está garantindo o princípio da

A) autenticidade.  
B) disponibilidade.  
C) integridade.  
D) confidencialidade.  
E) confiabilidade.  

**25.** (FCC · TRT-1/2025 · Analista — TI)  
Um Tribunal Regional do Trabalho precisa reforçar a segurança da informação em seus sistemas, garantindo a proteção dos dados processuais e a continuidade dos serviços digitais. A equipe de TI promoveu um debate sobre a utilização de um Sistema de Prevenção de Intrusão (IPS) comparando-o a outras soluções. Eles concluíram corretamente que o

A) Firewall de borda substitui a necessidade de um IPS, pois sua função é suficiente para evitar todos os tipos de ataques e filtrar o tráfego malicioso antes que ele chegue aos servidores internos.  
B) Sniffer de Rede é uma ferramenta mais eficaz do que um IPS, pois permite capturar e analisar pacotes de dados sem causar impacto na latência da rede do Tribunal.  
C) IPS analisa o tráfego de rede em tempo real, identificando padrões suspeitos e bloqueando automaticamente atividades maliciosas antes que comprometam os sistemas do Tribunal, não sendo equivalente a um IDS, que apenas alerta sobre ameaças sem intervir ativamente.  
D) WAF ( Web Application Firewall ) analisa registros de eventos e detecta anomalias de segurança em tempo real, além de bloquear ataques automaticamente, sendo uma alternativa ao IPS.  
E) NAC ( Network Access Control ) é uma solução equivalente ao IPS, pois impede qualquer acesso não autorizado à rede do Tribunal, monitorando continuamente os dispositivos conectados.  

---

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| Questão | Gabarito | Fonte / nota |
|---|---|---|
| 16 | **E** | IPsec túnel AH+ESP |
| 17 | **C** | Modos transporte e túnel |
| 18 | **E** | VPN-SSL via HTTPS |
| 19 | **E** | ZTA/ZTNA |
| 20 | **D** | S/MIME |
| 21 | **D** | Modelo híbrido TLS |
| 22 | **E** | RSA/ECDSA |
| 23 | **E** | Hash = integridade |
| 24 | **A** | Chave privada → autenticidade |
| 25 | **C** | IPS complementar |
*Arquivo 05 · VPN / VPN-SSL / ZTNA · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 06 Criptografia / certificados / TLS.*

*Arquivo 05 · VPN / VPN-SSL / ZTNA · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 06 Criptografia / certificados / TLS.*

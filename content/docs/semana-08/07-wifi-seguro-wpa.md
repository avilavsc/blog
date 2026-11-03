+++
title = '07 — Wi-Fi seguro — WEP/WPA legados, WPA2/WPA3 e EAP'
date = '2026-10-29'
draft = false
weight = 7
tags = ['semana-08', 'wifi', 'seguro']
categories = ['estudo']
+++
# Wi-Fi seguro — WEP/WPA legados, WPA2/WPA3 e EAP · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 07 — Wireless seguro (EAP; WEP/WPA legados; WPA2/WPA3)  
**Escopo deste arquivo:** evolução WEP → WPA → WPA2 → WPA3; Personal × Enterprise; AES-CCMP, TKIP, SAE, OWE, PMF; EAP (TLS, PEAP, etc.) com 802.1X/RADIUS.  
**Fora de escopo aqui:** PHY 802.11ac/ax/be em profundidade (semana 7 arquivo 04); papéis 802.1X já cobertos lá (gancho só); firewall/VPN (04–05); TLS handshake completo (06).

---

## 1. Material

### 1.1 Linha do tempo cobrável

| Geração | Ideia FCC | Cifra / auth típica | Status |
|---------|-----------|---------------------|--------|
| **WEP** | “Privacidade equivalente ao fio” — falhou | RC4 fraco, IV curto | **Obsoleto / proibido** em desenho sério |
| **WPA** | Ponte de migração | **TKIP** (RC4 melhorado) | Legado |
| **WPA2** | Padrão sólido longo tempo | **AES-CCMP**; integridade **CBC-MAC** | Ainda cobrado e usado |
| **WPA3** | Geração atual | **SAE** (Personal); PMF; reforços Enterprise | Edital explícito |

Edital: *wireless (EAP, WEP/WPA legados; WPA2/WPA3)*.

### 1.2 Personal × Enterprise

| Modo | Autenticação | Quando a FCC cita |
|------|--------------|-------------------|
| **Personal (PSK)** | Chave pré-compartilhada (senha do SSID) | Casa, SOHO; WPA3-Personal usa **SAE** no lugar do PSK “puro” vulnerável a offline dictionary do WPA2 |
| **Enterprise** | **802.1X + RADIUS + EAP** | Órgãos, tribunais — usuário/certificado individual |

**Flash FCC TCE-GO/2022:** 802.1X + RADIUS + EAP com certificados cliente e rede → **WPA2-Enterprise + EAP-TLS** (não WEP+MD5, não Personal+LEAP, não WPA3-Personal+SIM, não “WPA4”).

**Flash FCC TRT-15/2023:** AES-CCMP + CBC-MAC + autenticação **enterprise** → escolher **WPA2** (não TKIP, WEP, WPA genérico, nem EAP sozinho como “o protocolo”).

EAP é **framework** de autenticação, não o “pacote completo” de segurança wireless no lugar de WPA2.

### 1.3 WPA2 — CCMP e modos

- **CCMP** baseado em **AES** — confidencialidade.  
- Integridade associada a **CBC-MAC** no discurso da questão TRT-15.  
- **TKIP** é do WPA/transição — não é a resposta quando o enunciado pede AES-CCMP.  
- Enterprise: AP = **autenticador**; STA = **suplicante**; RADIUS = **servidor de autenticação** (semana 7).

### 1.4 WPA3 — o que muda na prova

**WPA3-Personal — SAE (Simultaneous Authentication of Equals):**

- Substitui o handshake PSK do WPA2 que permitia ataques de dicionário **offline** sobre o captura do 4-way.  
- Autenticação mútua baseada em senha com resistência bem maior a offline dictionary.  
- Hash-to-Element (H2E) entra no discurso técnico moderno (Wi-Fi 6E/7); para FCC, memorize **SAE** como marca WPA3-Personal.

**PMF (Protected Management Frames):** proteção de frames de gerenciamento — **obrigatória** em WPA3 (e exigida em 6 GHz).

**WPA3-Enterprise:**

- 802.1X com reforços (incl. AKM com SHA-256 no mapa vendor).  
- Modo **192-bit**: exige **EAP-TLS**, suites fortes (AES-256-GCM / SHA-384), certificados robustos — visão “alta segurança”; não precisa decorar todas as cipher suites, mas saiba: **não é PEAP-MSCHAPv2**.

**OWE (Opportunistic Wireless Encryption):**

- Rede “aberta” **com** cifra do meio (Diffie-Hellman na associação) — protege contra sniffing casual sem senha de portal.  
- Não substitui Enterprise em tribunal; é o upgrade de “open” → “open encrypted”.

**Transition mode:** WPA3 + WPA2 no mesmo BSS para migração; em **6 GHz**, segurança WPA3 (sem misturar legado WPA2).

### 1.5 EAP — métodos que caem

| Método | Ideia | Pegadinha |
|--------|-------|-----------|
| **EAP-TLS** | Certificados cliente **e** servidor | Resposta clássica “certificados dos dois lados” |
| **PEAP / EAP-TTLS** | Túnel TLS + credencial interna (senha) | Servidor com cert; cliente muitas vezes senha |
| **EAP-MD5** | Fraco, legado | Pareia com WEP nos distratores |
| **LEAP** | Proprietário Cisco legado | Não é a resposta “certificados mútuos” |
| **EAP-SIM** | SIM de celular | Não atende requisito de cert cliente+rede |

Fluxo cobrado (TRT-2/2025 estilo): EAPOL-Start → Identity → RADIUS encapsula → **TLS Start** / Client Hello → conclusão **EAP-TLS** (não PSK, não MD5, não PEAP só porque há túnel — o enunciado com TLS Start após identity aponta TLS).

### 1.6 Ataques e por que legado morre

- **WEP:** IV/RC4 quebráveis — nunca usar.  
- **WPA-TKIP:** melhor que WEP, inferior a WPA2-AES.  
- **WPA2-PSK:** vulnerável a ataque offline se a senha for fraca e o handshake capturado; KRACK é contexto histórico de handshake.  
- **WPA3-SAE:** mitiga o offline dictionary clássico do PSK.

### 1.7 Desenho típico TRT / órgão

SSID corporativo: **WPA3-Enterprise** (ou WPA2-Enterprise em transição) + **RADIUS** (NPS/ISE/FreeRADIUS) + **EAP-TLS** ou PEAP conforme PKI. Convidados: portal + segmentação VLAN; idealmente OWE/WPA3 se política permitir. Gestão: PMF on; desabilitar WEP/TKIP; SSID 6 GHz só com stack WPA3.

### 1.8 Captive portal × criptografia do ar

Portal cativo (aceite de termo / login web) **não** substitui WPA2/WPA3: sem cifra de link, o meio continua audível. Combinações típicas: open+portal (ruim), OWE+portal (melhor para visitante), ou Enterprise em VLAN de convidados. Em prova, se pedirem **AES-CCMP + enterprise**, a resposta continua **WPA2** (ou WPA3-Enterprise se o enunciado apontar a geração 3) — não “portal”.

### 1.9 Distratores

1. EAP no lugar de WPA2 quando o enunciado pede cifra+integridade+enterprise.  
2. WPA4 (não existe no padrão).  
3. WPA3-Personal quando o requisito é 802.1X/RADIUS.  
4. WEP “ainda aceitável com senha forte”.  
5. Confundir OWE com Enterprise.  
6. Achar que PMF é opcional no WPA3.

### 1.10 Mini-roteiro

1. AES-CCMP + CBC-MAC + enterprise? → **WPA2**.  
2. 802.1X + certs mútuos? → **WPA2-Enterprise + EAP-TLS**.  
3. Senha doméstica moderna? → **WPA3-Personal / SAE**.  
4. Open com cifra? → **OWE**.  
5. 6 GHz? → **só WPA3 (+ PMF)**.

---

## 2. Questões de treino (1–15)

**1.** Protocolo wireless legado considerado inseguro e obsoleto:

A) WEP  
B) WPA3-Enterprise  
C) WPA2 com AES-CCMP  
D) SAE  
E) OWE  

**2.** AES-CCMP + CBC-MAC + enterprise →

A) TKIP  
B) WEP  
C) WPA (só TKIP)  
D) WPA2  
E) EAP isolado como resposta completa  

**3.** 802.1X + RADIUS + EAP com certificados cliente e rede:

A) WEP e EAP-MD5  
B) WPA-Personal e EAP-LEAP  
C) WPA3-Personal e EAP-SIM  
D) WPA4-Enterprise e EAP-FAST  
E) WPA2-Enterprise e EAP-TLS  

**4.** EAP, corretamente:

A) é framework de autenticação usado com WPA2/WPA3-Enterprise  
B) é o substituto completo do WPA2 em todas as questões de cifra  
C) é algoritmo de hash SHA-512  
D) é o modo túnel IPsec  
E) cifra apenas com RC4 WEP  

**5.** WPA3-Personal marca registrada na prova:

A) WEP  
B) PPTP  
C) SAE  
D) OSPF  
E) MPLS  

**6.** PMF em WPA3:

A) é opcional e raramente citado  
B) é o nome do RADIUS  
C) substitui AES  
D) protege frames de gerenciamento e é exigido no WPA3  
E) só existe em WEP  

**7.** OWE serve para:

A) Enterprise com certificados obrigatórios  
B) eleger root bridge  
C) substituir TLS 1.3 na web  
D) autenticar via STP  
E) cifrar redes abertas sem senha compartilhada clássica  

**8.** TKIP relaciona-se sobretudo a:

A) WPA3-SAE puro  
B) AES-CCMP exclusivo do WPA2  
C) WPA / transição, baseado em RC4  
D) OWE  
E) IPsec ESP  

**9.** No WPA2-EAP, o AP é tipicamente:

A) autenticador  
B) suplicante  
C) root CA ICP-Brasil  
D) servidor RADIUS do usuário final  
E) VLAN nativa  

**10.** Ataque de dicionário offline sobre handshake PSK captura:

A) só existe em fibra óptica  
B) é evitado exclusivamente com WEP  
C) é função do STP  
D) é o principal cenário que o SAE do WPA3-Personal mitiga  
E) não se relaciona a Wi-Fi  

**11.** Modo 192-bit WPA3-Enterprise, visão cobrável:

A) usa WEP  
B) usa só PSK de 8 caracteres  
C) proíbe certificados  
D) é idêntico a OWE  
E) apoia-se em EAP-TLS com suítes criptográficas fortes  

**12.** Em 6 GHz (Wi-Fi 6E), segurança L2:

A) exige stack WPA3 (SAE/OWE/802.1X moderno) com PMF — sem legado WPA2 misto  
B) permite WEP livremente  
C) exige apenas TKIP  
D) dispensa qualquer autenticação  
E) usa só PPTP  

**13.** PEAP em relação a EAP-TLS:

A) PEAP típico: túnel TLS + autenticação interna (muitas vezes senha); EAP-TLS: certificados mútuos  
B) são sempre idênticos em exigir certificado de cliente  
C) PEAP é WEP  
D) EAP-TLS não usa RADIUS  
E) PEAP é o nome do SAE  

**14.** “WPA4-Enterprise” em alternativa de prova:

A) padrão IEEE oficial desde 2018  
B) obrigatório no edital TRT4  
C) substituto do TLS 1.3  
D) modo do AH  
E) distrator — não há WPA4 no mapa cobrado  

**15.** Síntese:

A) WEP é a escolha FCC para tribunais  
B) EAP substitui AES  
C) Personal e Enterprise são sempre iguais  
D) Evitar WEP/TKIP; WPA2 = AES-CCMP/Enterprise+EAP; WPA3 = SAE/PMF/OWE/Enterprise reforçado  
E) OWE exige WPA4  

---

### Gabarito comentado (treino 1–15)

1. **A** — WEP.  
2. **D** — WPA2 (TRT-15).  
3. **E** — Enterprise+EAP-TLS.  
4. **A** — framework.  
5. **C** — SAE.  
6. **D** — PMF obrigatório WPA3.  
7. **E** — open cifrado.  
8. **C** — WPA/TKIP.  
9. **A** — autenticador.  
10. **D** — SAE mitiga.  
11. **E** — EAP-TLS forte.  
12. **A** — só WPA3 em 6 GHz.  
13. **A** — PEAP × TLS.  
14. **E** — WPA4 fictício.  
15. **D** — mapa.

---

## 3. Questões de concurso FCC (16–25)

**16.** (FCC · TRT-15/2023 · Analista — TI)  
Um Analista deve escolher, entre os diferentes protocolos de rede sem fio, aquele que utiliza: AES-CCMP como padrão de encriptação; CBC-MAC como padrão para integridade; e autenticação em modo enterprise. Para atender às especificações, o Analista deve escolher o

A) TKIP.  
B) WPA2.  
C) WEP.  
D) WPA.  
E) EAP.  

**17.** (FCC · TCE-GO/2022 · ACE — TI)  
Para uma determinada rede wireless foram especificados os seguintes requisitos de segurança: A implantação do padrão 802.1x com servidor RADIUS para controlar o acesso de usuários. O mecanismo de autenticação EAP com base em certificado digital do cliente e da rede. Considerando esses requisitos, a alternativa que descreve, correta e respectivamente, o padrão de segurança e autenticação wireless capaz de atender às especificações da implantação é

A) WEP e EAP-MD-5.  
B) WPA-Personal e EAP-LEAP.  
C) WPA2-Enterprise e EAP-TLS.  
D) WPA3-Personal e EAP-SIM.  
E) WPA4- Enterprise e EAP-FAST.  

**18.** (FCC · TRT-17/2022 · Técnico Judiciário — TI)  
Em rede WLAN com WPA2-EAP, o Access Point é o I e o servidor RADIUS o II:

A) supliante – cliente  
B) supliante – autenticador  
C) autenticador – supliante  
D) autenticador – servidor de autenticação  
E) cliente – autenticador  

**19.** (FCC · TRT-15/2024 · Analista — TI)  
Um Tribunal implementou uma rede sem fio corporativa para facilitar a conexão de dispositivos móveis e estações de trabalho dos funcionários. Após a configuração inicial, o setor de TI notou uma grande quantidade de tráfego desconhecido, perda de conectividade e tentativas de acesso não autorizado. Uma análise de segurança indicou que o problema estava relacionado a um ataque direcionado à rede sem fio. O ataque mais provável que foi utilizado é o de

A) força bruta WPA2, que pode ser mitigado utilizando autenticação 802.1ax baseada em EAP-TLS.  
B) desautenticação ( Deauth Attack ), que pode ser mitigado utilizando 802.11w (proteção de gerenciamento de quadros).  
C) Men-In-The-Middle (MIM), que pode ser mitigado utilizando WANs isoladas para segmentos de rede.  
D) sniffing passivo, que pode ser mitigado utilizando ocultação do Service Set Identifier (SSID).  
E) negação de serviço (DoS) via flooding de pacotes, que pode ser mitigado utilizando implementação de filtros no firewall .  

**20.** (FCC · Fundação Florestal-SP/2026 · Analista de Gestão — Análise e Desenvolvimento de Sistemas)  
Em um cenário no qual uma Fundação para Conservação e Produção Florestal implantou uma rede Wi-Fi corporativa para equipes de fiscalização ambiental em Unidades de Conservação remotas, utilizando WPA3-Personal para proteger o acesso sem fio, e que pretende reduzir riscos associados à captura de handshakes para ataques de descoberta de senha, o mecanismo do WPA3 que deve ser utilizado é

A) a criptografia AES em modo Galois/Counter Mode (GCM) aplicada ao tráfego de dados, que elimina a necessidade de mecanismos específicos para proteção contra tentativas offline de descoberta de senha.  
B) a autenticação baseada em Pre-Shared Key (PSK) com derivação expandida de chaves de sessão, que dificulta a interceptação do tráfego ao manter inalterado o processo tradicional de estabelecimento de credenciais.  
C) o Simultaneous Authentication of Equals (SAE), que estabelece autenticação baseada em troca autenticada de chaves resistente à obtenção de material reutilizável para ataques offline de adivinhação de senha.  
D) o Temporal Key Integrity Protocol (TKIP) combinado com renovação dinâmica de chaves temporárias, que reduz a exposição das credenciais mediante atualização periódica dos parâmetros criptográficos da sessão.  
E) o Counter Mode with Cipher Block Chaining Message Authentication Code Protocol (CCMP) associado ao uso de quadros de gerenciamento protegidos, que impede a coleta de material criptográfico no processo inicial de autenticação dos usuários.  

**21.** (FCC · TRT-12/2023 · Analista — TI)  
Atenção: Para responder às questões de números 37 e 38, examine o programa C# abaixo. using System; using System.Security.Cryptography; using System.Text; class Program{ static void Main(){ string senha = "Tribunal Regional do Trabalho 12a região"; string senhaCriptografada = CriptografarSenha(senha); Console.WriteLine($"Senha: {senha}"); Console.WriteLine($"Senha Criptografada: {senhaCriptografada}"); } static string CriptografarSenha(string senha){ using (SHA256 sha256 = SHA256.Create()){ byte[] bytes = Encoding.UTF8.GetBytes(senha); I ...... StringBuilder builder = new StringBuilder(); for (int i = 0; i < hashBytes.Length; i++) builder.Append(hashBytes[i].ToString("x2")); return builder.ToString(); } } } O programa C# faz referência ao SHA-256. Associando a criptografia com a segurança em redes sem fio, é correto afirmar que o Este texto de apoio vale também pra questão 38 .

A) SHA-256 pode ser utilizado para criar hashes dos certificados digitais, que são usados para autenticar os dispositivos na rede, em uma implementação de WPA2 com autenticação baseada em certificados (como o EAP-TLS).  
B) SHA-256 é um protocolo de segurança para redes sem fio, que utiliza um hash criptográfico pertencente à família das funções hash SHA-2 ( Security Hash Algorithm 2nd generation ). O hash resultante é uma sequência de bytes que é única para os dados de entrada.  
C) WPA2 não contém mecanismos de autenticação; assim, é necessário acrescentar o uso de chaves pré-compartilhadas (PSK) e a autenticação baseada em servidores, usando protocolos criptográficos como o SHA-256.  
D) WPA2 é um método de hash criptográfico para redes sem fio, que utiliza o protocolo de criptografia AES-CCMP ( Advanced Encryption Security – Counter Mode Cipher Block Chaining Message Authentication Ciphered Protocol ) para proteger os dados transmitidos entre dispositivos na rede.  
E) WPA ( Wi-Fi Protected Access ) utiliza o protocolo SHA-256 para criptografar dados e o método EAP ( Extensible Authentication Protocol ) para autenticação.  

**22.** (FCC · TRT-20/2024 · Técnico — TI)  
Durante uma auditoria de segurança, foi identificado que um dispositivo não autorizado conseguiu acessar a rede. O Técnico de rede suspeita que houve uma falha no processo de autenticação IEEE 802.1X, que pode ter sido causada por

A) falta de um firewall na rede.  
B) configuração incorreta do servidor RADIUS.  
C) uso de um switch não gerenciável.  
D) falta de criptografia WPA2.  
E) configuração de VLANs inadequada.  

**23.** (FCC · SEFAZ-SC/2018 · TI)  
Considere os seguintes controles da política de segurança estabelecida em uma empresa: I. Controlar o acesso de pessoas às áreas em que se encontram os servidores computacionais da empresa. II. Bloquear acesso dos funcionários para sites inseguros da internet. III. Instalar Firewall para controlar os acessos externos para a rede local da empresa. Os controles mencionados são, respectivamente, tipificados como de Segurança

A) Física, Lógica e Lógica.  
B) Lógica, Lógica e Lógica.  
C) Física, Lógica e Física.  
D) Lógica, Lógica e Física.  
E) Física, Física e Lógica.  

**24.** (FCC · TRT-5/2022 · Técnico — TI)  
Com o objetivo mitigar ataques contra ameaças de aplicações web, tais como, injeção de SQL, sequestro de sessão, alteração de URL e estouro de buffer , um técnico instalou uma solução de segurança baseada em rede para proteção do servidor web e e-mail voltados para internet. A solução de segurança que é capaz de realizar tal tarefa é o

A) WAF.  
B) NAC.  
C) IDS.  
D) DMZ.  
E) Proxy .  

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
| 16 | **B** | WPA2 AES-CCMP |
| 17 | **C** | WPA2-Enterprise + EAP-TLS |
| 18 | **D** | Papéis EAP |
| 19 | **B** | Deauth / 802.11w |
| 20 | **C** | SAE no WPA3-Personal |
| 21 | **A** | SHA-256 + certificados |
| 22 | **B** | Falha RADIUS |
| 23 | **A** | Segurança física de servidores |
| 24 | **A** | WAF (camada app) |
| 25 | **E** | Zero Trust |
*Arquivo 07 · Wi-Fi seguro WPA2/WPA3/EAP · Claudionir · TRT4 Analista TI (código 14) · FCC.*

*Arquivo 07 · Wi-Fi seguro WPA2/WPA3/EAP · Claudionir · TRT4 Analista TI (código 14) · FCC.*

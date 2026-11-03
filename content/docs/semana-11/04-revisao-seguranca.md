+++
title = '04 — Revisão — Segurança (ISO/LGPD/OWASP · firewall/VPN/TLS · ENSEC)'
date = '2026-10-30'
draft = false
weight = 4
tags = ['semana-11', 'revisao']
categories = ['estudo']
+++
# Revisão — Segurança (ISO/LGPD/OWASP · firewall/VPN/TLS · ENSEC) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — Semana 11 = revisão · reforço segurança  
**Escopo deste arquivo:** contrastes cobráveis ISO 27001/27002/27701, LGPD, OWASP Top 10, firewall/WAF/IDS/IPS/DMZ/NAC, VPN (IPsec × SSL/TLS) + TLS/certificados flash, ENSEC-PJ (CNJ 396) + Portaria 162 flash.  
**Fora de escopo aqui:** redes L2/L3 detalhadas (`03-revisao-redes.md`); mapa ágeis/ITIL/COBIT/SO/AD/IA (`05-mapa-temas-vermelhos.md`); tutorial de hardening linha a linha.

**Priors:** semana 8 (`01`–`08`).

---

## 1. Material (revisão densa)

### 1.1 ISO 27001 × 27002 × 27701

| Norma | Flash |
|-------|-------|
| **27001** | **Requisitos** do **SGSI** (certificável) |
| **27002** | **Orientação** / código de prática dos **controles** |
| **27701** | Gestão de **privacidade** (**PIMS**) — ≠ LGPD |
| Anexo A (2022) | **93** controles · **4** temas |
| **SoA** | Declaração de Aplicabilidade (inclui/exclui com justificativa) |

**CID:** Confidencialidade · Integridade · Disponibilidade.  
**Alta direção** estabelece a **política de SI** (FCC TRT-19/2022) — não “só TI/CIO/Qualidade”.  
**PDCA do SGSI:** Estabelecer → Implementar/Operar → Monitorar/Analisar → Manter/Melhorar.  
**Não:** 27002 certifica sozinha; Anexo A = checklist cego de 93; 27701 = lei brasileira.

### 1.2 LGPD flash

| Figura | Papel |
|--------|-------|
| **Controlador** | Decide finalidades/meios do tratamento |
| **Operador** | Trata em nome do controlador |
| **Encarregado (DPO)** | Canal controlador ↔ titulares ↔ **ANPD** |

Agentes de tratamento = **Controlador** e **Operador** (Encarregado não é “terceiro agente” no mesmo sentido).  
**Dados sensíveis** × pessoais “comuns”; bases legais (consentimento não é a única).  
Sanções: advertência; multa simples até **2%** faturamento, limitada a **R$ 50 milhões** por infração (FCC TJ-CE/2022). Suspensão de atividade: **6 meses**, prorrogável por igual período (FCC TRT-23/2022).  
Direitos do titular: confirmação, acesso, correção, anonimização/bloqueio/eliminação, portabilidade, informação, revogação do consentimento…  
Crianças: coleta sem consentimento só para **contatar** pais/responsável — **sem armazenar** (FCC TRT-4/2022).

### 1.3 OWASP Top 10 (visão de prova)

FCC cobra **papéis** e mitigações, não decorar o PDF inteiro. Âncoras:

| Ideia | Flash |
|-------|-------|
| Broken Access Control / injecção | Autorização server-side; prepared statements |
| XSS / CSRF | Validação/encoding; tokens anti-CSRF |
| Supply chain / componentes | Dependências conhecidas e atualizadas |
| SSDF / Secure SDLC | Segurança desde o ciclo (NIST SSDF + OWASP) |

Anti-confusão: WAF mitiga muitos ataques web **em runtime**; não substitui código seguro. Front-end “escondendo” botão ≠ controle de acesso.

### 1.4 Firewall · WAF · IDS/IPS · DMZ · NAC

| Controle | O que a FCC quer |
|----------|------------------|
| **Firewall** | Regras IP/porta/estado; borda |
| **WAF** | HTTP(S): SQLi, XSS, session hijack… |
| **UTM** | Caixa única (FW+AV+IDS/IPS…) |
| **IDS** | Detecta + **alerta** |
| **IPS** | Detecta + **bloqueia** (≠ IDS) |
| **DMZ** | **Zona** de isolamento (não appliance) |
| **NAC** | Quem/o que **entra** na rede |
| **SIEM** | Correlaciona **logs** |

FCC: firewall **não** substitui IPS; WAF ≠ NAC ≠ SIEM; UTM protege rede ampla; WAF foca app web; DMZ isola (TRT-12/2023; TRT-1/2025; TRF4/2025).

### 1.5 VPN · TLS · certificados

| Tema | Flash |
|------|-------|
| **IPsec** | Modos **transporte** e **túnel**; **AH** + **ESP** |
| **VPN-SSL/TLS** | Acesso via **navegador HTTPS** (clientless típico) |
| **TLS** | Sucessor do SSL; cifra sessão do browser ↔ gateway SSL VPN |
| **HTTPS** | Simétrica cifra conteúdo; assimétrica ajuda a estabelecer chave; certificado = identidade via AC |
| **Hash** | **Integridade** (não “cifra reversível”) |
| Assinar com **privada** / verificar com **pública** | **Autenticidade** (FCC TRT-6/2025) |

Anti-confusão: VPN-SSL **não** “exige sempre cliente dedicado”; “exclusivamente SSL” é armadilha (hoje TLS). IPsec site-to-site ≠ ZTNA (ZTNA = acesso por app/identidade, MFA, microsegmentação — FCC PMSP/2025).

### 1.6 ENSEC-PJ flash (CNJ 396/2021 + Portaria 162)

**ENSEC-PJ** = Estratégia Nacional de Segurança Cibernética do Poder Judiciário.

Contempla (FCC TRT-5/2022): SI **ampla** essencial à cibernética + segurança física/proteção de **ativos de TI** — **não** “só requisitos funcionais operacionais” nem “só automatizar não-invasão”.

Ações típicas cobradas (Art. 9º — FCC TRT-17/2022): rede de **cooperação** do Judiciário para segurança cibernética; modelo **centralizado** de governança cibernética nacional (ler enunciado: banca pega em “corporativa” genérica vs “cibernética”).

**Portaria 162/2021:** operacionaliza/detalha no ecossistema CNJ — não confundir número da Resolução (**396**) com Portaria (**162**), nem com CNJ 400/443 (PDPJ/segurança da informação em outros packs).

### 1.7 Checklist anti-erro

1. 27001 requisitos × 27002 orientação × 27701 privacidade.  
2. Alta direção / SoA / 93÷4.  
3. Controlador × Operador × Encarregado × ANPD.  
4. IDS alerta × IPS bloqueia × WAF HTTP × NAC admissão.  
5. IPsec AH/ESP × VPN-SSL via HTTPS.  
6. ENSEC = 396; ampla + ativos — não checklist operacional estreito.

---

## 2. Questões de treino (1–15)

**1.** Norma de **requisitos** do SGSI:

A) 27002  
B) 27001  
C) 27701 isolada como LGPD  
D) ITIL 5  
E) OWASP Top 10  

**2.** Contagem cobrável do Anexo A pós-2022:

A) 114 controles / 14 domínios  
B) 93 controles / 4 temas  
C) 34 practices / 7 princípios  
D) 10 categorias OWASP apenas  
E) 162 controles / Portaria  

**3.** Quem estabelece a política de SI apropriada ao propósito da organização (visão 27001):

A) Somente o CIO  
B) Somente a área de Qualidade  
C) Alta direção  
D) Somente Gerência de Projetos  
E) Somente o Service Desk  

**4.** Encarregado (LGPD) atua como canal entre:

A) Só ANPD e Operador, sem titulares  
B) Controlador, titulares e ANPD  
C) Apenas firewall e WAF  
D) Só Product Owner e Scrum Master  
E) Apenas Root Bridge e RADIUS  

**5.** Multa simples LGPD (pessoa jurídica) — percentual e teto clássicos de prova:

A) 10% e R$ 100 milhões  
B) 4% e R$ 100 milhões  
C) 2% e R$ 50 milhões  
D) 3% e R$ 60 milhões  
E) 1% sem teto  

**6.** IDS × IPS, respectivamente:

A) Bloqueia × apenas alerta  
B) Alerta × bloqueia  
C) Equivalentes e intercambiáveis  
D) Ambos só físicos  
E) Ambos substituem WAF  

**7.** Melhor encaixe para mitigar SQLi/XSS em app web na borda HTTP:

A) NAC puro  
B) DMZ como produto  
C) WAF  
D) Sniffer passivo  
E) DHCP Relay  

**8.** UTM × WAF × DMZ — síntese correta:

A) UTM = só app web; WAF = rede inteira; DMZ = proxy reverso  
B) UTM protege rede com funções integradas; WAF foca apps web; DMZ isola zona  
C) DMZ substitui UTM e WAF  
D) WAF = SIEM  
E) UTM = apenas 802.1X  

**9.** IPsec — modos cobráveis:

A) Apenas aplicação e sessão  
B) Transporte e túnel (AH e ESP)  
C) Apenas SMTP e IMAP  
D) Apenas WPA2 e WPA3  
E) Apenas Discover e Offer  

**10.** VPN-SSL tipicamente:

A) Só via Telnet 23  
B) Exige sempre cliente full IPsec  
C) Pode ser acessada por navegador com HTTPS  
D) Usa OSPF para cifrar  
E) Substitui LGPD  

**11.** Hash, em prova de SI, garante principalmente:

A) Disponibilidade do link  
B) Integridade  
C) Broadcast IPv6  
D) Root Bridge  
E) Product Backlog  

**12.** ENSEC-PJ está na Resolução CNJ:

A) 400/2021  
B) 443/2021  
C) 396/2021  
D) 370/2021  
E) 492/2023  

**13.** OWASP + SSDF — leitura cobrável:

A) Substituem 27001  
B) Orientam desenvolvimento/testes seguros (ex.: Top 10 + práticas SSDF)  
C) São sinônimos de ENSEC  
D) Eliminam necessidade de WAF e código seguro  
E) São práticas ITIL de mudança  

**14.** Controle SIEM × WAF × Firewall, respectivamente:

A) HTTP · logs · IP/porta  
B) Logs/correlação · app web · IP/porta  
C) VLAN · STP · 802.1X  
D) AAAA · A · MX  
E) Scrum · XP · Kanban  

**15.** Síntese correta da revisão:

A) 27002 certifica SGSI sozinha; IDS = IPS; VPN-SSL exige sempre cliente; ENSEC = Portaria 400  
B) 27001=SGSI; LGPD=lei; OWASP=app; FW/WAF/IDS/IPS distintos; VPN-SSL via HTTPS; ENSEC=396  
C) 27701 = LGPD artigo a artigo  
D) NAC = WAF  
E) Alta direção não participa da política de SI  

---

### Gabarito comentado (treino 1–15)

1. **B** — 27001.  
2. **B** — 93/4.  
3. **C** — Alta direção.  
4. **B** — Canal clássico.  
5. **C** — 2% / R$ 50 mi.  
6. **B** — Alerta × bloqueia.  
7. **C** — WAF.  
8. **B** — UTM/WAF/DMZ.  
9. **B** — Transporte/túnel.  
10. **C** — Browser HTTPS.  
11. **B** — Integridade.  
12. **C** — 396/2021.  
13. **B** — Dev seguro.  
14. **B** — SIEM/WAF/FW.  
15. **B** — Síntese.

---

## 3. Questões de concurso FCC (16–25)

> Questões reais com citação. Gabarito só ao final.

**16.** (FCC · TRT-19/2022 · Analista Judiciário — TI)  
Segundo a norma ABNT NBR ISO/IEC 27001:2013, quem deve estabelecer uma política de segurança da informação que seja apropriada ao propósito da organização é:

A) Área de Tecnologia da Informação.  
B) Área de Qualidade.  
C) Alta Direção.  
D) Gerência de Projetos.  
E) Chief Information Officer (CIO).  

**17.** (FCC · TRT-5/2013 · Analista Judiciário — TI)  
A Norma NBR ISO/IEC 27001 adota o modelo Plan-Do-Check-Act (PDCA) aplicado aos processos do SGSI. A sequência correta é

A) Implementação e Operação → Monitoramento e Análise Crítica → Estabelecimento → Manutenção e Melhoria  
B) Estabelecimento → Manutenção e Melhoria → Monitoramento e Análise Crítica → Implementação e Operação  
C) Monitoramento e Análise Crítica → Implementação e Operação → Estabelecimento → Manutenção e Melhoria  
D) Estabelecimento → Implementação e Operação → Monitoramento e Análise Crítica → Manutenção e Melhoria  
E) Implementação e Operação → Monitoramento e Análise Crítica → Manutenção e Melhoria → Estabelecimento  

**18.** (FCC · TRF4/2025 · Analista — Análise de Sistemas)  
No âmbito da LGPD (Lei Geral de Proteção de Dados), existem as figuras de Controlador, Operador e Encarregado. O papel de tais figuras e a relação entre elas no tratamento de dados pessoais e a Autoridade Nacional de Proteção de Dados (ANPD), de acordo com LGPD, podem ser realizados por dois agentes de tratamento, o

A) Controlador e o Operador, enquanto o Encarregado é a pessoa indicada pelo controlador e operador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
B) Controlador e o Encarregado, enquanto o Operador é a pessoa indicada pelo encarregado e controlador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
C) Encarregado e o Operador, enquanto o controlador é a pessoa indicada pelo Encarregado e operador para atuar como canal de comunicação entre o operador, os titulares dos dados e a ANPD.  
D) Controlador e o Operador, enquanto o Encarregado é a pessoa indicada pelo titular e operador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
E) Controlador e o Encarregado, enquanto o Encarregado é a pessoa indicada pelo controlador e titulares para atuar como canal de comunicação entre o controlador, o encarregado dos dados e a ANPD.  

**19.** (FCC · TJ-CE/2022 · Analista — SI)  
Considere o texto abaixo, adaptado da Lei Geral de Proteção de Dados (LGPD). "Os agentes de tratamento de dados, em razão das infrações cometidas às normas previstas nesta Lei, ficam sujeitos às sanções administrativas aplicáveis pela autoridade nacional, dentre elas, multa simples sobre o faturamento da pessoa jurídica de direito privado, grupo ou conglomerado no Brasil no seu último exercício, excluídos os tributos, limitada, no total, a um determinado valor em reais por infração ." (grifos nossos). O percentual de multa simples e o valor em reais por infração, referidos no texto, são, correta e respectivamente, de até

A) 3% e R$ 60.000.000,00.  
B) 4% e R$ 50.000.000,00.  
C) 4% e R$ 100.000.000,00.  
D) 2% e R$ 50.000.000,00.  
E) 2% e R$ 100.000.000,00.  

**20.** (FCC · TRT-5/2022 · Técnico — TI)  
Com o objetivo mitigar ataques contra ameaças de aplicações web, tais como, injeção de SQL, sequestro de sessão, alteração de URL e estouro de buffer , um técnico instalou uma solução de segurança baseada em rede para proteção do servidor web e e-mail voltados para internet. A solução de segurança que é capaz de realizar tal tarefa é o

A) WAF.  
B) NAC.  
C) IDS.  
D) DMZ.  
E) Proxy .  

**21.** (FCC · TRT-1/2025 · Analista — TI)  
Um Tribunal Regional do Trabalho precisa reforçar a segurança da informação em seus sistemas, garantindo a proteção dos dados processuais e a continuidade dos serviços digitais. A equipe de TI promoveu um debate sobre a utilização de um Sistema de Prevenção de Intrusão (IPS) comparando-o a outras soluções. Eles concluíram corretamente que o

A) Firewall de borda substitui a necessidade de um IPS, pois sua função é suficiente para evitar todos os tipos de ataques e filtrar o tráfego malicioso antes que ele chegue aos servidores internos.  
B) Sniffer de Rede é uma ferramenta mais eficaz do que um IPS, pois permite capturar e analisar pacotes de dados sem causar impacto na latência da rede do Tribunal.  
C) IPS analisa o tráfego de rede em tempo real, identificando padrões suspeitos e bloqueando automaticamente atividades maliciosas antes que comprometam os sistemas do Tribunal, não sendo equivalente a um IDS, que apenas alerta sobre ameaças sem intervir ativamente.  
D) WAF ( Web Application Firewall ) analisa registros de eventos e detecta anomalias de segurança em tempo real, além de bloquear ataques automaticamente, sendo uma alternativa ao IPS.  
E) NAC ( Network Access Control ) é uma solução equivalente ao IPS, pois impede qualquer acesso não autorizado à rede do Tribunal, monitorando continuamente os dispositivos conectados.  

**22.** (FCC · TRT-12/2023 · Analista — TI)  
A VPN-SSL é um tipo específico de VPN que

A) é mais adequada em ambientes que exigem configurações avançadas e controle granular sobre políticas de segurança e recursos de rede.  
B) utiliza exclusivamente o protocolo SSL ( Secure Sockets Layer ) para estabelecer uma conexão segura entre o cliente e o servidor.  
C) requer, geralmente, a instalação de um software cliente dedicado em cada dispositivo para ser utilizada.  
D) não é compatível com dispositivos móveis e tablets , pois necessita de software cliente no dispositivo desktop para sua utilização.  
E) pode ser acessada através de um navegador web padrão, usando o protocolo HTTPS.  

**23.** (FCC · TRT-19/2022 · Analista — TI)  
Todo o tráfego entre um navegador web e um dispositivo SSL VPN é criptografado com o protocolo SSL ou

A) IPSec.  
B) TLS.  
C) MPLS.  
D) SSH.  
E) OSPF.  

**24.** (FCC · TRT-17/2022 · Analista — TI)  
O IPSec pode ser usado nos modos

A) aplicação e rede e, para proteger a comunicação via rede, utiliza, principalmente, o cabeçalho de autenticação (AH) e a carga de dados de segurança ( payload ).  
B) aplicação e túnel e, para proteger a comunicação via rede, utiliza, principalmente, o protocolo Secure Sockets Layer (SSL) e a carga de empacotamento (ESP).  
C) transporte e túnel e, para proteger a comunicação via rede, utiliza, principalmente, o cabeçalho de autenticação (AH) e a carga de empacotamento (ESP).  
D) transporte e aplicação e, para proteger a comunicação via rede, utiliza, principalmente, o cabeçalho inter-rede (IR) e a carga de empacotamento (ESP).  
E) transporte e rede e, para proteger a comunicação via rede utiliza, principalmente, o cabeçalho inter-rede (IR), o protocolo Secure Sockets Layer (SSL) e a carga de dados de segurança ( payload ).  

**25.** (FCC · TRT-5/2022 · Analista Judiciário — TI)  
Considere o contexto da Resolução no 396 de 07/06/2021. A ENSEC-PJ prevista nesta Resolução contempla: I. temas relacionados à segurança da informação, de forma ampla, que sejam essenciais para segurança cibernética. II. segurança lógica e proteção de dados específicos do nível operacional, nos aspectos relacionados aos requisitos funcionais. III. segurança física e proteção de ativos de tecnologia da informação de forma geral; IV. ações destinadas a assegurar, de forma automatizada, a não invasão física de localidades e de informações. Está correto o que consta APENAS de

A) I e II.  
B) I e III.  
C) II, III e IV.  
D) III.  
E) II e IV.  

---

### Gabarito comentado (FCC 16–25)

| Q | Gab. | Nota |
|---|------|------|
| 16 | **C** | TRT-19/2022 — Alta Direção |
| 17 | **D** | TRT-5/2013 — PDCA SGSI |
| 18 | **A** | TRF4/2025 — Controlador+Operador; Encarregado canal |
| 19 | **D** | TJ-CE/2022 — 2% e R$ 50 mi |
| 20 | **A** | TRT-5/2022 — WAF |
| 21 | **C** | TRT-1/2025 — IPS ≠ IDS |
| 22 | **E** | TRT-12/2023 — VPN-SSL via HTTPS |
| 23 | **B** | TRT-19/2022 — SSL ou TLS |
| 24 | **C** | TRT-17/2022 — transporte/túnel; AH/ESP |
| 25 | **B** | TRT-5/2022 — ENSEC I e III |

Distribuição concurso: C, D, A, D, A, C, E, B, C, B.

---

*Arquivo 04 · Revisão segurança · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 11. Próximo sugerido: `05-mapa-temas-vermelhos.md`.*

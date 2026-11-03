+++
title = '01 — TCP/IP e protocolos de aplicação'
date = '2026-10-15'
draft = false
weight = 1
tags = ['semana-07', 'tcp', 'ip']
categories = ['estudo']
+++
# TCP/IP e protocolos de aplicação · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — TCP/IP (IPv4/IPv6, DNS) · TCP, UDP · SSH, SMTP, HTTP, FTP, LDAP, DNS, DHCP (e contraste leve com POP/IMAP)  
**Escopo deste arquivo:** modelo TCP/IP cobrável; endereçamento IPv4/IPv6; papéis e portas de DNS, DHCP, HTTP(S), SSH, SMTP, FTP e LDAP; anti-confusões clássicas da FCC (camada × protocolo, TCP × UDP, DORA, registro A × AAAA, SMTP × POP/IMAP).  
**Fora de escopo aqui:** equipamentos de interconexão (arquivo 02); VLANs/802.1Q/802.1X/STP (03); Wi-Fi 802.11 (04); proxy/Apache/IIS (05); JBoss/WildFly (06); SNMP/QoS e monitoramento (07–08); segurança profunda (VPN, WPA3, IDS — semana 8).

---

## 1. Material

### 1.1 Por que a FCC cobra este bloco

No edital do TRT4 (código 14), a linha de **Redes** é explícita: TCP/IP (IPv4/IPv6, DNS); TCP, UDP, IPSec, ARP, SSH, SMTP, HTTP, FTP, LDAP, DNS, DHCP, POP, IMAP. Em Analista TI, a banca quase nunca pede “decorar RFC inteira”: ela quer **camada correta**, **porta padrão**, **papel do protocolo** e **distinção entre pares parecidos** (TCP×UDP, SMTP×IMAP, Discover×Request, HTTP×HTTPS, registro A×AAAA).

Cartão mental:

| Tema | Ideia cobrável |
|------|----------------|
| TCP/IP | 4 camadas; cada protocolo “mora” em uma |
| TCP × UDP | Confiável/orientado a conexão × best-effort/sem conexão |
| IPv4 × IPv6 | 32 × 128 bits; sem broadcast no IPv6 |
| DNS | Nome → IP; porta 53; tipos A/AAAA/MX/CNAME |
| DHCP | DORA; 67/UDP servidor, 68/UDP cliente |
| HTTP(S) | 80 / 443 |
| SSH | Acesso remoto criptografado; 22/TCP |
| SMTP / POP / IMAP | Envio × download × sincronização no servidor |
| FTP | Controle 21 + dados 20 (modo ativo clássico) |
| LDAP | Diretório hierárquico; 389 (LDAPS 636) |

### 1.2 Modelo TCP/IP (visão de prova)

A arquitetura TCP/IP clássica de concurso tem **quatro** camadas (de cima para baixo):

| Camada TCP/IP | Função cobrável | Exemplos |
|---------------|-----------------|----------|
| **Aplicação** | Serviços ao usuário/processo | HTTP, HTTPS, DNS, DHCP, SSH, SMTP, FTP, LDAP, POP, IMAP, SNMP |
| **Transporte** | Segmentação, portas, fim a fim | **TCP**, **UDP** |
| **Internet / Rede** | Endereçamento e roteamento | **IP** (v4/v6), ICMP, **IPSec**, (ARP costuma ser cobrado aqui ou na interface — ver abaixo) |
| **Interface de rede / Enlace** | Acesso ao meio físico/lógico | Ethernet, Wi-Fi, drivers |

Flash anti-confusão de camadas (FCC SEGEP-MA/2018 e clones):

- **UDP** → Transporte (nunca “aplicação”).  
- **LDAP, DNS, SMTP, HTTP, SSH, DHCP, FTP, SNMP** → Aplicação.  
- **IP / IPSec** → Rede.  
- **FTP não é transporte** (armadilha frequente: “TCP e FTP na mesma linha”).  
- **SMTP não é rede**.  
- **SSH não é transporte**.

Sobre **ARP**: resolve IP → MAC na LAN. Em alguns materiais fica na “rede/internet”; em outros, na “enlace/interface”. Em prova FCC, quando a alternativa correta coloca **ARP–Rede**, aceite; se a banca disser “enlace”, leia o conjunto. O que **não** passa: ARP na aplicação ou no transporte.

Comparação rápida com OSI (só o necessário):

| TCP/IP | OSI aproximado |
|--------|----------------|
| Aplicação | Aplicação + Apresentação + Sessão |
| Transporte | Transporte |
| Internet | Rede |
| Interface | Enlace + Física |

### 1.3 TCP × UDP

**TCP (Transmission Control Protocol)**

- Orientado a **conexão** (three-way handshake: SYN → SYN-ACK → ACK).  
- Entrega **confiável**, ordenada, com controle de fluxo/congestão e retransmissão.  
- Usado por HTTP(S), SSH, SMTP, FTP (controle/dados), LDAP típico, etc.

**UDP (User Datagram Protocol)**

- **Sem conexão**; datagrama “melhor esforço”.  
- Sem garantia de entrega/ordem; overhead menor → adequado a DNS (consultas curtas), DHCP, RTP/VoIP (mídia), SNMP em muitos cenários.  
- Aplicação assume o risco (ou usa reenvio próprio).

Anti-confusão:

| Afirmação | Verdade? |
|-----------|----------|
| “UDP é orientado a conexão como o TCP” | **Não** |
| “TCP é o protocolo de rede que roteia pacotes” | **Não** (isso é IP) |
| “DNS só usa TCP” | **Não** (consultas tipicamente UDP/53; zone transfer e respostas grandes usam TCP/53) |
| “DHCP usa TCP 67” | **Não** (UDP 67/68) |

### 1.4 IPv4

- Endereço: **32 bits** (4 octetos decimais, ex.: `192.168.10.45`).  
- Máscara/CIDR define rede × host (`/24` = 255.255.255.0).  
- Classes A/B/C são legado de prova; o que importa hoje em Analista é **CIDR**, privado vs público, e papéis:

| Tipo | Exemplos cobráveis |
|------|--------------------|
| Privados (RFC 1918) | 10.0.0.0/8; 172.16.0.0/12; 192.168.0.0/16 |
| Loopback | 127.0.0.0/8 (127.0.0.1) |
| Broadcast | todos 1s no host (ex.: 192.168.1.255 /24) |
| Gateway padrão | roteador da sub-rede local |

**IP** oferece serviço **não confiável** (best-effort): sem garantia fim a fim; confiabilidade, se necessária, sobe para o TCP.

### 1.5 IPv6

- Endereço: **128 bits**, 8 grupos hexadecimais de 16 bits (`2001:0db8:85a3:0000:0000:8a2e:0370:7334`).  
- Compressão: zeros à esquerda e **um** `::` para sequência contínua de zeros.  
- **Não há broadcast** no IPv6; funções equivalentes usam **multicast** (e anycast).  
- Autoconfiguração (SLAAC) + DHCPv6 (cenários distintos — não misturar com DORA IPv4 sem ler o enunciado).  
- Registro DNS: **AAAA** = nome → IPv6; **A** = nome → IPv4.

Flash:

| | IPv4 | IPv6 |
|---|------|------|
| Bits | 32 | 128 |
| Broadcast | Sim | **Não** |
| NAT massivo | Comum | Menos “necessário” no desenho original |
| DNS | A | AAAA |

### 1.6 DNS (Domain Name System)

Função: traduzir **nomes** em **endereços** (e vice-versa em consultas reversas).

- Porta padrão: **53** (UDP para consultas típicas; TCP quando necessário).  
- Hierarquia: raiz → TLD (`.br`, `.gov`) → domínios → hosts.  
- Tipos cobráveis:

| Tipo | Liga |
|------|------|
| **A** | Nome → IPv4 |
| **AAAA** | Nome → IPv6 |
| **MX** | Domínio → servidor de e-mail |
| **CNAME** | Alias → nome canônico |
| **NS** | Domínio → servidor autoritativo |
| **PTR** | IP → nome (reverso) |

Armadilhas FCC:

- Servidor DNS **não** “escuta na 68/TCP”.  
- Resolução típica do cliente **não** é “só multicast” como regra geral IPv4 (consulta unicast ao resolvedor configurado; broadcast/multicast aparecem em outros protocolos — DHCP/IPv6 neighbor).  
- Registro **A** ≠ AAAA.

### 1.7 DHCP (Dynamic Host Configuration Protocol)

Atribui dinamicamente IP, máscara, gateway, DNS etc.

**Portas IPv4 (decorar):**

| Papel | Porta |
|-------|-------|
| Servidor DHCP | **67/UDP** |
| Cliente DHCP | **68/UDP** |

**DORA** (ordem cobrada até a morte):

1. **Discover** — cliente procura servidor (broadcast).  
2. **Offer** — servidor oferece IP (e parâmetros).  
3. **Request** — cliente pede/aceita a oferta.  
4. **Acknowledge** — servidor confirma o lease.

O servidor mantém um **pool** de endereços válidos e endereços **excluídos/reservados** (servidores estáticos, impressoras, gateway). Não confunda: DHCP não “armazena protocolos de comunicação IP válidos para todos os clientes” nem “IP de clientes HTTP/TCP” — fala de **interfaces/hosts** na camada de rede.

Anti-confusão (TRT-17/2022, TRT-18/2023, TRT-12/2023):

- Primeiro pacote para obter IP novo após release típico → **DISCOVER**, não REQUEST.  
- Após OFFER → cliente manda **REQUEST**.  
- Após DISCOVER → servidor manda **OFFER**.  
- Protocolo de transporte: **UDP**, não TCP.  
- Porta do **servidor** é 67; a **68** é do cliente (não inverter).

### 1.8 HTTP e HTTPS

| | HTTP | HTTPS |
|---|------|-------|
| Porta | **80/TCP** | **443/TCP** |
| Segurança | Texto claro (sem TLS) | HTTP sobre **TLS** |
| Uso institucional | Legado / redirecionamento | Padrão para páginas autenticadas e dados sensíveis |

HTTP é protocolo de **aplicação** para transferência de recursos web (métodos GET/POST etc. — detalhe fino costuma cair mais em desenvolvimento; em redes, porta + camada + contraste com HTTPS bastam).

### 1.9 SSH (Secure Shell)

- Acesso remoto **criptografado** (substitui Telnet em prova de “boa prática”).  
- Porta padrão: **22/TCP**.  
- Modelo cliente/servidor: qualquer host com o serviço SSH ativo pode ser alvo de conexão remota por linha de comando.  
- Não confundir SSH (22) com Telnet (23) nem com HTTPS (443).

### 1.10 Correio: SMTP × POP × IMAP

| Protocolo | Papel cobrável | Porta clássica |
|-----------|----------------|----------------|
| **SMTP** | **Envio** (cliente → servidor; servidor → servidor) | 25/TCP (submissão moderna: 587; SMTPS legado 465 — saber 25 é o mínimo) |
| **POP3** | **Recebimento**; tipicamente baixa e (por padrão) remove do servidor | 110 / 995 (seguro) |
| **IMAP** | Acesso e **sincronização** da caixa no servidor (pastas, flags, multi-dispositivo) | 143 / 993 (seguro) |

Flash FCC DETRAN-AP/2022: “envio cliente→servidor” = **SMTP**; “sincroniza alterações em todos os dispositivos” = **IMAP**.

### 1.11 FTP (File Transfer Protocol)

- Transferência de arquivos (upload/download).  
- **Duas** conexões clássicas: **controle** (porta **21**) e **dados** (porta **20** no modo ativo tradicional).  
- Modo passivo inverte quem inicia a conexão de dados (firewall-friendly) — se cair, lembre: “duas portas / controle + dados” já resolve a maioria das questões.  
- FTP “puro” não é criptografado (FTPS/SFTP são variantes; SFTP costuma ir sobre SSH).

### 1.12 LDAP (Lightweight Directory Access Protocol)

- Protocolo de **diretório** (consulta/atualização de informações hierárquicas: usuários, grupos, recursos).  
- Derivado do modelo X.500 (versão “leve”).  
- Porta padrão: **389/TCP**; LDAPS tipicamente **636**.  
- Entradas identificadas por **DN** (Distinguished Name); árvore (DIT); atributos.  
- Uso típico: autenticação/autorização, catálogo corporativo, base para serviços de diretório (em muitos cenários, integração com autenticação de rede).

O que a FCC já cobrou como **INCORRETO** (MPE-RN/2010): afirmar que “**toda** informação de um item pode ser colocada em um **único** registro” — simplificação falsa frente ao modelo hierárquico/atributos.  
Afirmações coerentes em prova: localiza/gerencia usuários e recursos; permite segurança por ramos; consulta pode focar em ramos da hierarquia; relação histórica com soluções de diretório (cuidado com enunciados que misturam DNS e LDAP).

### 1.13 Tabela-mestra de portas (mínimo TRT4)

| Serviço | Porta(s) | Transporte típico |
|---------|----------|-------------------|
| FTP dados / controle | 20 / 21 | TCP |
| SSH | 22 | TCP |
| Telnet | 23 | TCP |
| SMTP | 25 | TCP |
| DNS | 53 | UDP/TCP |
| DHCP servidor / cliente | 67 / 68 | **UDP** |
| HTTP | 80 | TCP |
| POP3 | 110 | TCP |
| IMAP | 143 | TCP |
| HTTPS | 443 | TCP |
| LDAP / LDAPS | 389 / 636 | TCP |

### 1.14 Mapa de erros clássicos (caderno)

1. Colocar FTP/SMTP/SSH na camada de **transporte**.  
2. DHCP em **TCP** ou inverter 67↔68.  
3. Dizer que o primeiro passo do DORA é REQUEST.  
4. Confundir SMTP (envio) com IMAP (sincroniza no servidor).  
5. Achar que IPv6 tem broadcast igual ao IPv4.  
6. Registro A para IPv6 (é AAAA).  
7. HTTP = 443 (é 80; 443 é HTTPS).  
8. SSH = 23 (é 22; 23 é Telnet).

---

## 2. Questões de treino (1–15)

**1.** Na arquitetura TCP/IP de quatro camadas, o protocolo **UDP** pertence à camada de:

A) Transporte  
B) Aplicação  
C) Enlace físico exclusivo  
D) Apresentação OSI apenas, nunca transporte  
E) Sessão obrigatória do modelo TCP/IP  

**2.** Relacione corretamente protocolo e camada no modelo TCP/IP:

A) SMTP–Rede; UDP–Aplicação; LDAP–Transporte  
B) ARP–Aplicação; TCP–Enlace; DNS–Rede  
C) IPSec–Rede; SMTP–Rede; POP3–Transporte  
D) FTP–Transporte; SNMP–Transporte; SSH–Rede  
E) ARP–Rede; UDP–Transporte; LDAP–Aplicação  

**3.** Sobre TCP e UDP, assinale a afirmação **correta**:

A) UDP garante entrega ordenada e retransmissão automática como o TCP  
B) TCP é orientado a conexão e oferece confiabilidade fim a fim típica  
C) Ambos residem na camada de rede junto com o IP  
D) TCP é usado exclusivamente por DHCP e DNS  
E) UDP estabelece three-way handshake antes de cada datagrama  

**4.** Em IPv4, o tamanho do endereço é:

A) 32 bits  
B) 128 bits  
C) 16 bits  
D) 64 bits  
E) 256 bits  

**5.** Em IPv6, é correto afirmar que:

A) O endereço tem 32 bits e usa notação decimal pontilhada obrigatória  
B) Existe broadcast idêntico ao IPv4 em todas as sub-redes  
C) O endereço tem 128 bits e o broadcast clássico é substituído por mecanismos como multicast  
D) O registro DNS tipo A mapeia diretamente o nome para IPv6  
E) A compressão `::` pode aparecer quantas vezes forem necessárias no mesmo endereço  

**6.** Um registro DNS do tipo **A** vincula:

A) Nome de domínio a um endereço IPv6  
B) Nome de domínio a um endereço IPv4  
C) Endereço MAC a um gateway padrão  
D) Porta TCP 25 a um servidor SSH  
E) Domínio exclusivamente a um servidor LDAP  

**7.** No DHCPv4, as portas padrão do **servidor** e do **cliente** são, respectivamente:

A) 53/TCP e 53/UDP  
B) 68/TCP e 67/TCP  
C) 67/UDP e 68/UDP  
D) 80/TCP e 443/TCP  
E) 389/TCP e 636/TCP  

**8.** A sequência correta do processo DORA é:

A) Request → Offer → Discover → Acknowledge  
B) Offer → Discover → Acknowledge → Request  
C) Acknowledge → Request → Offer → Discover  
D) Discover → Offer → Request → Acknowledge  
E) Discover → Request → Offer → Acknowledge  

**9.** Em resposta a um **DHCPOFFER**, o cliente tipicamente envia:

A) DHCPDISCOVER  
B) DHCPACK  
C) DHCPREQUEST  
D) DHCPNACK obrigatório  
E) DNS QUERY tipo AAAA  

**10.** As portas padrão de HTTP, HTTPS e SSH são, respectivamente:

A) 443, 80 e 23  
B) 80, 443 e 22  
C) 22, 80 e 443  
D) 25, 110 e 143  
E) 53, 67 e 68  

**11.** O protocolo adequado para **envio** de e-mail do cliente ao servidor de correio é:

A) SMTP  
B) POP3  
C) FTP  
D) IMAP  
E) SNMP  

**12.** Qual protocolo permite que alterações na caixa de e-mail (pastas, flags) permaneçam sincronizadas entre vários dispositivos conectados ao mesmo servidor?

A) SMTP exclusivo  
B) ARP  
C) IMAP  
D) Telnet  
E) DHCP  

**13.** Sobre o FTP, a característica mais cobrada em prova é:

A) Usar uma única porta UDP 53 para tudo  
B) Utilizar duas portas, uma para controle e outra para dados  
C) Substituir o DNS em redes IPv6  
D) Operar somente na camada de transporte  
E) Escutar, por padrão, nas portas 67 e 68  

**14.** Em relação ao LDAP, assinale a afirmação **incorreta**:

A) Localiza e gerencia usuários e recursos na rede  
B) Toda informação de um item pode ser colocada em um único registro  
C) Permite a obtenção de informações de apenas um ramo da hierarquia  
D) Possibilita implantar segurança em ramos diferentes da árvore  
E) Normalmente o cliente conecta-se pela porta 389/TCP  

**15.** Um analista precisa de acesso remoto criptografado por linha de comando a um servidor Linux do tribunal. O protocolo e a porta padrão mais alinhados a essa necessidade são:

A) Telnet / 23  
B) HTTP / 80  
C) FTP / 21  
D) SMTP / 25  
E) SSH / 22  

---

### Gabarito comentado (treino 1–15)

1. **A** — UDP = transporte.  
2. **E** — ARP–Rede; UDP–Transporte; LDAP–Aplicação (padrão FCC).  
3. **B** — TCP orientado a conexão + confiável.  
4. **A** — IPv4 = 32 bits.  
5. **C** — 128 bits; sem broadcast clássico.  
6. **B** — A → IPv4 (AAAA → IPv6).  
7. **C** — 67/UDP servidor; 68/UDP cliente.  
8. **D** — DORA.  
9. **C** — Após OFFER vem REQUEST.  
10. **B** — 80, 443, 22.  
11. **A** — SMTP envia.  
12. **C** — IMAP sincroniza no servidor.  
13. **B** — Controle + dados.  
14. **B** — Afirmação incorreta clássica sobre LDAP.  
15. **E** — SSH/22.

---

## 3. Questões estilo concurso FCC (16–25)

> Bloco com questões de provas FCC (e itens no padrão da banca). Gabarito só ao final.

**16.** (FCC – SEGEP-MA/2018 – Analista Executivo – Programador de Sistemas)  
Os diversos protocolos utilizados para prover os serviços na internet e também realizar o gerenciamento da rede de computadores são mapeados nas camadas hierarquicamente distribuídas no modelo/arquitetura TCP/IP. Um correto relacionamento entre o protocolo e a respectiva camada é:

A) ARP-Rede; UDP-Transporte; LDAP-Aplicação.  
B) TCP-Transporte; FTP-Transporte; SNMP-Aplicação.  
C) ARP-Enlace; DNS-Aplicação; IMAP-Aplicação.  
D) IPSec-Rede; SMTP-Rede; POP3-Aplicação.  
E) SIP-Enlace; SSH-Transporte; DHCP-Aplicação.

**17.** (FCC – TRT-17/2022 – Analista Judiciário – TI)  
Um analista em tecnologia da informação, utilizando o PowerShell em um computador com Windows 10, emite sequencialmente os comandos `ipconfig /release` e `ipconfig /renew`. A respeito desse processo e do protocolo de rede envolvido, considere:

I. Um pacote em broadcast com a porta TCP de destino configurada em 67 é gerado ao emitir o comando `ipconfig /renew`.  
II. Ao emitir o comando `ipconfig /release` o cliente aloca uma porta UDP com valores superiores a 1023.  
III. O primeiro pacote encaminhado em uma solicitação DHCP IPv4 é conhecido como DHCP REQUEST.  
IV. Em resposta à solicitação DHCP IPv4 do tipo DISCOVER, o servidor responde com pacote DHCP OFFER.

Está correto o que se afirma APENAS em

A) III.  
B) IV.  
C) I e II.  
D) II e III.  
E) I, III e IV.

**18.** (FCC – TRT-18/2023 – Analista Judiciário – TI)  
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

**19.** (FCC – DETRAN-AP/2022 – Analista de Tecnologia da Informação)  
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

**20.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
O servidor DHCP armazena as informações de configuração em um banco de dados que inclui:

I. Os protocolos de comunicação IP válidos para todos os clientes na rede.  
II. Endereços IP válidos, mantidos em um pool para atribuição a clientes, bem como endereços excluídos.  
III. Endereços IP associados a clientes HTTP. Isso permite a atribuição consistente de um único endereço IP a um único cliente TCP.

Está correto o que se afirma APENAS em

A) II e III.  
B) I.  
C) II.  
D) III.  
E) I e II.

**21.** (FCC – PGE-AM/2022 – Técnico em Gestão Procuratorial – Informática)  
Durante pesquisa sobre protocolos de comunicação de dados, um Técnico observou as seguintes características:

I. A leitura da estrutura de pastas e do conteúdo contido nelas é feita em tempo real, ou seja, há a sincronização entre o programa usado e a conta de e-mail. Toda ação usando esse protocolo afeta diretamente o conteúdo no servidor de e-mail. Assim, a criação ou a remoção de pastas, as alterações nos seus conteúdos, modificam a estrutura e o conteúdo da conta de e-mail no servidor.  
II. Determina de forma dinâmica e automática a atribuição de um endereço IP para seu dispositivo. Assim, a cada conexão feita, o dispositivo recebe um IP que pode ser diferente da conexão anterior, de acordo com a disponibilidade de endereços IPs livres.  
III. Permite a conexão criptografada entre um dispositivo remoto e um servidor e a execução de comandos nesse servidor, tal como se o usuário estivesse presencialmente manipulando a máquina. Mas por basear-se no modelo cliente/servidor e ser também um serviço, se um dispositivo qualquer na rede rodar esse serviço, mesmo não sendo um servidor típico, ele pode ser acessado por outro dispositivo da rede e, por meio de linhas de comando, ser utilizado remotamente.

Estas características correspondem, correta e respectivamente, aos protocolos

A) POP, DHCP e SLH.  
B) SMTP, DHLC e SHL.  
C) IMAP, DHCP e SSH.  
D) POP, SNMP e SSL.  
E) IMAP, DHTP e SSL.

**22.** (FCC – MPE-RN/2010 – Analista de Tecnologia da Informação)  
Em relação ao LDAP é INCORRETO afirmar que

A) serve como base para uma solução DNS (Domain Name System).  
B) toda informação de um item pode ser colocada em um único registro.  
C) permite a obtenção de informações de apenas um ramo da hierarquia.  
D) possibilita implantar segurança em ramos diferentes da árvore.  
E) localiza e gerencia usuários e recursos na rede.

**23.** (FCC – TRT-6/2018 – Analista Judiciário – TI)  
Com a introdução e uso do IPv6, o DNS se tornou extremamente importante para facilitar o acesso aos recursos da internet. Dentre os tipos de registro DNS, o utilizado para vincular um domínio ou subdomínio a um endereço IPv6 é o

A) AFSDB.  
B) AAAA.  
C) CNAME.  
D) A.  
E) SOA.

**24.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
Normalmente um servidor de saída utiliza a porta 25 como padrão para SMTP. Todavia, alguns IPs bloqueiam sua utilização por conta do tráfego massivo de spam e malware. Para se evitar esse bloqueio, é possível utilizar a porta

A) 25025 que é suportada por diversos servidores de saída SMTP e é útil para conexões encriptadas ou TLS.  
B) 52025 que é suportada por diversos servidores de saída SMTP e é útil para conexões não encriptadas ou SSL.  
C) 2525 que é suportada por diversos servidores de saída SMTP e é útil para conexões encriptadas ou TLS.  
D) 645 que é suportada por diversos servidores de saída SMTP e é útil para conexões não encriptadas ou SSL.  
E) 587 que é suportada por diversos servidores de saída SMTP e é útil para conexões não encriptadas ou TLS.

**25.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
No cabeçalho IPv6, há um campo de 20 bits utilizado para fornecer um identificador para pacotes que pertencem ao mesmo fluxo de tráfego. Esse campo pode ser usado pelos dispositivos de rede para aplicar políticas de QoS, como garantir uma determinada largura de banda, latência ou priorização para pacotes de um mesmo fluxo. Trata-se do campo

A) Type of Service.  
B) Next Flow.  
C) Traffic Flow.  
D) Traffic Class.  
E) Flow Label.

---

### Gabarito comentado (FCC 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **A** | FCC SEGEP-MA/2018 — ARP–Rede; UDP–Transporte; LDAP–Aplicação |
| 17 | **B** | FCC TRT-17/2022 — apenas IV (DISCOVER→OFFER; I usa TCP; II/III errados) |
| 18 | **D** | FCC TRT-18/2023 — III e IV (I: DHCP não é 68/TCP; II: multicast genérico falso) |
| 19 | **D** | FCC DETRAN-AP/2022 — SMTP, FTP, DHCP, IMAP |
| 20 | **C** | FCC TRT-12/2023 — apenas II (pool + excluídos) |
| 21 | **C** | FCC PGE-AM/2022 — IMAP, DHCP, SSH |
| 22 | **B** | FCC MPE-RN/2010 — incorreto: “tudo em um único registro” |
| 23 | **B** | FCC TRT-6/2018 — registro AAAA → IPv6 |
| 24 | **E** | FCC TRT-12/2023 — SMTP submission porta 587 |
| 25 | **E** | FCC TRT-12/2023 — Flow Label (20 bits) no IPv6 |

**Notas rápidas:** 16 A (camadas). 17 B (só OFFER após DISCOVER). 18 D (A + REQUEST). 19 D (SMTP/FTP/DHCP/IMAP). 20 C (pool). 21 C (IMAP/DHCP/SSH). 22 B (LDAP). 23 B (AAAA). 24 E (587). 25 E (Flow Label).

Distribuição do gabarito neste arquivo (treino + concurso): A–E presentes (concurso: A, B, C, D, E).

---

*Arquivo 01 · TCP/IP e protocolos · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 equipamentos.*

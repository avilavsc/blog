+++
title = '04 — Windows 10/11 e Windows Server 2019/2022/2025'
date = '2026-10-27'
draft = false
weight = 4
tags = ['semana-09', 'windows']
categories = ['estudo']
+++
# Windows 10/11 e Windows Server 2019/2022/2025 · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — Windows 10/11 (legado/suporte) + Windows Server 2019/2022/2025 (edições, roles, features)  
**Escopo deste arquivo:** ciclo de vida do cliente (10 legado/ESU × 11); edições Server (Standard × Datacenter); instalação Desktop Experience × Server Core; **funções e recursos** (Hyper-V, File/SMB, IIS, DHCP/DNS, AD DS como role); administração local (msconfig, drivers, firewall avançado, gpupdate no cliente).  
**Fora de escopo aqui:** AD profundo (floresta/OU/GPO/FSMO — arquivo 05); PowerShell genérico e WSUS (arquivo 06); Linux (arquivo 03); processos/memória (01–02).

---

## 1. Material

### 1.1 Por que esse pacote cai junto

O edital TRT4 lista **Windows 11 / Windows 10 (legado/suporte estendido) / Windows Server 2019, 2022 e 2025**, além de AD, PowerShell e WSUS (packs 05–06). Neste arquivo a FCC espera: **fim de suporte do 10**, diferença de **edição Server**, o que é **função × recurso**, **Hyper-V**, compartilhamento **SMB**, e utilitários do **cliente** (msconfig, Gerenciador de Dispositivos, firewall avançado, `gpupdate`).

Cartão mental:

| Tema | Flash cobrável |
|------|----------------|
| **Windows 10** | Fim do suporte geral **14/10/2025**; PC continua; ESU = patches de segurança pagos |
| **Windows 11** | Linha atual; requisitos de hardware (TPM 2.0 etc.) |
| **Standard** | Pouca virtualização (direitos típicos: **2** OSE/VM com isolamento Hyper-V por licença completa) |
| **Datacenter** | Datacenter altamente virtualizado; **VMs ilimitadas** (com host licenciado) |
| **Server Core** | Sem GUI completa → menor superfície de ataque |
| **Role** | Papel de servidor (AD DS, DHCP, DNS, Hyper-V, IIS, File…) |
| **Feature** | Recurso adicional (ex.: Containers, .NET) instalável com/ sem role |

### 1.2 Windows 10 — legado e suporte estendido

- **Windows 10 22H2** foi a última feature update da linha “clássica”.  
- **Fim do suporte geral:** **14 de outubro de 2025** (Microsoft Lifecycle). Depois disso: sem atualizações gratuitas de qualidade/segurança/suporte técnico regular.  
- O equipamento **continua funcionando**; não “desliga a Internet” por decreto.  
- **ESU (Extended Security Updates):** programa **pago** com atualizações de segurança críticas/importantes por tempo limitado; **não** traz features novas nem suporte técnico amplo.  
- Migração típica: Windows 11 (se hardware elegível) ou hardware novo.

Distratores: “obrigado a comprar PC novo mesmo com hardware compatível”; “após o fim do suporte o SO deixa de bootar”; confundir data com fim do Windows 7 (14/01/2020).

### 1.3 Windows 11 — noções cobráveis

- Cadência anual de versão; Home/Pro tipicamente **24 meses** de manutenção por release; Enterprise/Education **36 meses** (Lifecycle FAQ).  
- UI: Menu Iniciar centralizado por padrão; sem Live Tiles do 10.  
- Em rede corporativa: ainda entra em domínio AD, aplica GPO, usa SMB, RDP, etc.  
- Para a prova TRT4, o peso maior no cliente é **legado 10 + suporte** e comandos administrativos que a FCC já cobrou em Windows 10 (`msconfig`, firewall avançado, `gpupdate`, drivers).

### 1.4 Windows Server 2019 / 2022 / 2025 — edições

Família atual do edital: **2019, 2022 e 2025**. Conceitos estáveis entre versões:

| Edição | Cenário | Virtualização (leitura de prova) |
|--------|---------|----------------------------------|
| **Standard** | Físico ou pouco virtualizado | Direitos limitados de VM (clássico: **2** OSE/VM Hyper-V por atribuição completa de núcleos) |
| **Datacenter** | Alta densidade / software-defined | **VMs ilimitadas** no host devidamente licenciado |
| **Datacenter: Azure Edition** | Só Azure (contexto); hotpatch etc. | Fora do núcleo cobrável on-prem, só menção |

Licenciamento moderno: **por núcleo** (mínimos típicos de documentação Microsoft: 8 por processador / 16 por servidor) + **CAL** de acesso (exceto cenários de assinatura pay-as-you-go). Em prova: foque **Standard × Datacenter × densidade de VM**, não decore tabela de preço.

Recursos “pesados” de datacenter (Storage Spaces Direct, Shielded VMs, SDN…) historicamente **diferenciam** Datacenter — a banca raramente pede checklist completo; basta saber que **Datacenter = virtualização ilimitada / SDDC**.

### 1.5 Opções de instalação: Desktop Experience × Server Core

- **Desktop Experience (GUI):** Server Manager gráfico, ferramentas MMC.  
- **Server Core:** sem shell gráfico completo; administra via sconfig, PowerShell, WinRM, Windows Admin Center / RSAT remoto.  
- Motivo cobrável de segurança: **reduz superfície de ataque** (menos componentes/GUI). Em hardening de DC, recomendações de mercado/bancas citam preferir Core quando viável.

### 1.6 Funções (roles) e recursos (features)

Instalação: **Gerenciador do Servidor → Gerenciar → Adicionar Funções e Recursos** (ou `Install-WindowsFeature` / `Add-WindowsFeature` no PowerShell — detalhe de cmdlets no arquivo 06).

Roles cobráveis (mapa):

| Função | Papel |
|--------|-------|
| **AD DS** | Domínio / DC (identidade) — aprofundar no 05 |
| **DNS** | Resolução de nomes (crítico para AD) |
| **DHCP** | Lease de IP |
| **Hyper-V** | Hipervisor / VMs |
| **Web Server (IIS)** | Sites/apps HTTP |
| **File and Storage Services** | Compartilhamentos, NTFS, quotas… |
| **Remote Desktop Services** | Sessões/apps remotos |
| **WSUS** | Patches intranet — arquivo 06 |
| **Print and Document Services** | Impressão |

**Hyper-V:** infraestrutura de virtualização de servidores; em cenários VDI, desktops virtuais para clientes. Não confundir com antivírus, desfragmentação ou cofre de senhas.

### 1.7 SMB, firewall e portas no Server

Compartilhamento de arquivos Windows usa **SMB**. Portas clássicas cobradas:

- **TCP 445** (SMB direto)  
- **TCP 139** (NetBIOS session, legado)

**Flash FCC TCE-GO/2022:** bloquear 445 e 139 no firewall do Windows Server → clientes **não** acessam pastas compartilhadas / serviços SMB.  
Não é NTP (UDP 123), nem HTTPS (443), nem DHCP (67/68), nem RADIUS.

### 1.8 Cliente Windows 10/11 — utilitários que a FCC cobra

| Necessidade | Ferramenta / comando |
|-------------|----------------------|
| Modos de inicialização (Normal / Diagnóstico / Seletiva) | `Win+R` → **`msconfig`** |
| Regra de firewall por porta TCP | Firewall → **Configurações Avançadas** → Regras de Entrada → Nova Regra |
| Atualizar driver manualmente | Gerenciador de Dispositivos → Atualizar driver → **Procurar software de driver no computador** |
| Aplicar GPO agora | **`gpupdate /force`** (não `gpresult`, que só **mostra** resultado) |

`gpresult` = relatório; `gpupdate` = atualiza. `/force` reaplica mesmo políticas já processadas.

### 1.9 AD DS no mapa deste arquivo (só o encaixe)

Aqui basta: AD DS é **função** do Server; serviço clássico **NTDS**; iniciar via `net start ntds` (FCC SEFAZ-BA/2019). Grupos administrativos sensíveis (Domain Admins, Administrators BUILTIN, Schema Admins, Enterprise Admins) — FCC TRT-20/2024. Detalhe de OU/GPO/FSMO → **arquivo 05**.

### 1.10 Distratores favoritos

1. Fim do suporte Windows 10 = máquina “morre” ou perde Internet.  
2. Standard = VMs ilimitadas (isso é Datacenter).  
3. Hyper-V = antivírus / desfragmentador.  
4. `gpresult` no lugar de `gpupdate`.  
5. Liberar porta TCP só em “Permitir aplicativo”, sem Configurações Avançadas.  
6. Portas 445/139 = DHCP ou HTTPS.  
7. Trocar Domain Admins ↔ Enterprise Admins ↔ Schema Admins.

### 1.11 Mini-roteiro (15 segundos)

1. Cliente legado / data 14/10/2025? → **Windows 10 EOS** (+ ESU se enunciado).  
2. Muitas VMs no host? → **Datacenter**.  
3. Sem GUI / hardening? → **Server Core**.  
4. Papel de servidor? → **Role** no Server Manager.  
5. Virtualização Microsoft no Server? → **Hyper-V**.  
6. Pasta compartilhada falhou com firewall? → **SMB / 445–139**.  
7. GPO imediata no Windows 10/11? → **`gpupdate /force`**.

---

## 2. Questões de treino (1–15)

**1.** Data amplamente associada ao fim do suporte geral do Windows 10:

A) 14/01/2020  
B) 05/10/2021  
C) 14/10/2025  
D) 31/12/2030  
E) não há data publicada  

**2.** Após o fim do suporte geral do Windows 10, é correto afirmar que:

A) o PC deixa de inicializar obrigatoriamente  
B) o SO pode continuar em uso, porém sem o ciclo normal de atualizações gratuitas  
C) a Microsoft obriga a compra de hardware novo mesmo com PC elegível ao 11  
D) ESU passa a ser gratuito e ilimitado para sempre  
E) SMB deixa de existir  

**3.** Edição Windows Server indicada para ambiente altamente virtualizado com direitos amplos de VM:

A) Home  
B) Standard apenas  
C) Datacenter  
D) Windows 10 Pro  
E) MS-DOS  

**4.** Server Core, em relação à instalação com Desktop Experience:

A) aumenta a superfície de ataque pela GUI extra  
B) remove a necessidade de licença  
C) reduz componentes/GUI e é citado em hardening  
D) impede Hyper-V  
E) é exclusivo do Windows 11 Home  

**5.** Hyper-V no Windows Server é, tipicamente:

A) antimalware de endpoint  
B) função/recurso de virtualização de máquinas  
C) protocolo de e-mail  
D) substituto do NTFS  
E) modo de energia  

**6.** Bloqueio das portas TCP 445 e 139 no firewall do Windows Server tende a impedir:

A) NTP  
B) HTTPS público  
C) acesso SMB a pastas compartilhadas  
D) DHCP  
E) RADIUS  

**7.** Para forçar a aplicação imediata de GPO em estação Windows 10:

A) gpresult /sync  
B) gpfixup /force  
C) gpupdate /force  
D) msconfig /gpo  
E) net start gpo  

**8.** Caminho cobrado para criar regra de entrada por porta no firewall do Windows 10:

A) apenas “Permitir um aplicativo” sem avançadas  
B) Configurações Avançadas → Regras de Entrada → Nova Regra  
C) msconfig → Serviços → Firewall  
D) Paint → Rede  
E) Disk Management → Nova Partição  

**9.** Comando `Win+R` para modos Normal/Diagnóstico/Seletiva de inicialização:

A) msinit  
B) winsetup  
C) msconfig  
D) regedit /boot  
E) dxdiag obrigatório  

**10.** Standard × Datacenter (leitura de prova):

A) Standard sempre tem VMs ilimitadas  
B) Datacenter é só para desktops Home  
C) Datacenter cobre cenários de alta densidade de virtualização; Standard, poucos OSEs/VMs  
D) são idênticos em direitos de VM  
E) Standard exige Linux  

**11.** AD DS no Server Manager é:

A) um recurso opcional de desfragmentação  
B) uma função (role) de serviços de domínio  
C) um driver de vídeo  
D) sinônimo de WSUS  
E) apenas um tema do Windows 11  

**12.** Iniciar o serviço de Domínio do AD via linha de comando (clássico FCC):

A) activate instance Active Directory  
B) net start ntds  
C) netsh start instance ntds  
D) netstart set Active Directory on  
E) dnf start ntds  

**13.** No Gerenciador de Dispositivos, opção para instalar driver a partir de arquivo já no disco:

A) Localizar software de driver na instalação do Windows  
B) Procurar software de driver no computador  
C) Pesquisar software de driver no disco de instalação (como única rótulo oficial)  
D) Procurar software de driver na Internet (como única)  
E) Format C:  

**14.** CAL no modelo clássico Windows Server Standard/Datacenter:

A) nunca é necessária  
B) tipicamente requerida para usuários/dispositivos que acessam o servidor (há exceções documentadas)  
C) substitui a licença de núcleo  
D) é um tipo de Hyper-V  
E) só existe no Windows 95  

**15.** Síntese alinhada ao edital para este subtópico:

A) Win10 legado/EOS+ESU; Win11 linha atual; Server 2019/2022/2025 Standard×Datacenter; roles (Hyper-V, File/SMB, IIS…); Core×GUI; utilitários cliente  
B) Windows 10 não tem data de fim de suporte  
C) Hyper-V = apenas antivírus  
D) Server Core aumenta a GUI  
E) SMB usa só a porta 443  

---

### Gabarito comentado (treino 1–15)

1. **C** — 14/10/2025.  
2. **B** — continua; sem ciclo normal de updates.  
3. **C** — Datacenter.  
4. **C** — menos superfície.  
5. **B** — virtualização.  
6. **C** — SMB.  
7. **C** — gpupdate /force.  
8. **B** — avançadas / entrada.  
9. **C** — msconfig.  
10. **C** — densidade.  
11. **B** — role AD DS.  
12. **B** — net start ntds.  
13. **B** — procurar no computador.  
14. **B** — CAL típica.  
15. **A** — cartão mental.

---

## 3. Questões de concurso FCC (16–25)

**16.** (FCC · TRT-5/2022 · Técnico — TI)  
Um técnico deseja que uma política de grupo aplicada ao domínio entre em operação imediatamente em uma máquina com Windows 10, em condições ideais. Para tanto, ele deve utilizar o comando

A) gpresult /sync  
B) gpupdate /sync  
C) gpfixup /force  
D) gpupdate /force  
E) gpresult /f  

**17.** (FCC · TJ-MA/2019 · Técnico Judiciário — Informática Hardware)  
Para alterar as configurações do firewall do Windows 10, um Técnico clicou nas opções Painel de Controle > Sistema e Segurança > Windows Defender Firewall. A partir da janela que apareceu, ele pretende criar uma regra de entrada para liberar uma porta TCP a fim de que um aplicativo se comunique com o seu computador. Ele conseguirá acessar a janela, a partir da qual será possível configurar essa regra, clicando em:

A) Permitir um Aplicativo através do Firewall > Criar Regra > Abrir Porta > TCP.  
B) Permitir um Aplicativo através do Firewall > Definir Porta > TCP.  
C) Alterar Configurações > Criar Regra > Nova regra de entrada.  
D) Permitir um Aplicativo através do Firewall > Incluir Regra > Nova Regra.  
E) Configurações Avançadas > Regras de Entrada > Nova Regra.  

**18.** (FCC · TJ-MA/2019 · Técnico Judiciário — Informática Hardware)  
No gerenciador de dispositivos do Windows 10, em português, após selecionar um dispositivo e clicar na ferramenta Atualizar driver de dispositivo, aparecerá uma janela onde será possível escolher a forma como se deseja pesquisar pelo driver correto. Dentre outras, a opção que permite localizar e instalar o software de driver manualmente é:

A) Localizar software de driver na instalação do Windows.  
B) Procurar software de driver no computador.  
C) Pesquisar software de driver no disco de instalação.  
D) Procurar software de driver na Internet.  
E) Localizar software de driver em uma pasta ou na rede.  

**19.** (FCC · SPPREV/2019 · Analista em Gestão Previdenciária)  
O Windows 10, em português, possui os modos de inicialização normal, que carrega todos os drivers de dispositivos e serviços; de diagnóstico, que carrega apenas dispositivos e serviços básicos; e seletiva, em que o usuário pode selecionar o que quer carregar na inicialização. Após notar um problema no funcionamento do Windows, um usuário deseja definir o modo de inicialização de diagnóstico. Para chegar à janela onde será possível escolher tal modo de inicialização, pressionou a combinação de teclas `Windows + R` e, na janela de execução, digitou um comando e clicou no botão OK. O comando correto digitado pelo usuário foi

A) msinit  
B) msmode  
C) restartmode  
D) msconfig  
E) winsetup  

**20.** (FCC · TCE-GO/2022 · Analista de Controle Externo — TI)  
Ao verificar a configuração do firewall no Windows Server 2019, um analista verificou que as portas TCP 445 e 139 estavam bloqueadas. O bloqueio dessas portas fará com que

A) o servidor não tenha o horário sincronizado com o servidor NTP de referência, impactando no horário dos computadores da rede local.  
B) os clientes da rede não possam acessar pastas compartilhadas no servidor e outros serviços de rede com base no protocolo SMB.  
C) os clientes da rede não consigam acessar sites baseados em https e realizar chamadas de procedimento (RPC) para o servidor.  
D) os clientes da rede não consigam adquirir endereços IPs IPv4 e IPv6 por meio do protocolo DHCP do servidor.  
E) o servidor não possa autenticar os clientes da rede por meio dos protocolos RADIUS e TACACs.  

**21.** (FCC · TRT-20/2024 · Técnico — TI)  
No ambiente Windows Server 2019, instalado e funcionando em condições ideais, um Técnico ficou encarregado de atribuir privilégio mínimo, que restringe os direitos de acesso apenas ao que é necessário para a execução de uma tarefa ou função de trabalho específica. Para iniciar a tarefa, ele procurou determinar quais entidades de segurança (usuários e grupos) pertencem a grupos administrativos. No AD DS (Active Directory Domain Services), há vários grupos administrativos confidenciais, quais sejam, Grupo de segurança

I. global que reside na pasta Usuários de cada domínio em sua floresta AD DS. Os membros podem executar todas as tarefas administrativas em qualquer domínio local.  
II. do domínio local que reside na pasta BUILTIN em AD DS. O grupo local também existe em todos os computadores em sua floresta AD DS. Eles têm acesso completo e irrestrito ao domínio ou computador.  
III. universal que reside na pasta Usuários do domínio raiz da floresta. Os membros podem modificar as propriedades do esquema de AD DS. As alterações de esquema não são frequentes, mas têm um efeito muito significativo.  
IV. universal que reside na pasta Usuários do domínio raiz da floresta AD DS. Os membros podem executar todas as tarefas administrativas em qualquer lugar da floresta. Há algumas tarefas de gerenciamento que exigem associação a este grupo.

Os grupos administrativos de I a IV são, correta e respectivamente:

A) I – Administradores de esquemas; II – Administradores; III – Administradores corporativos; IV – Administradores do domínio.  
B) I – Administradores do domínio; II – Administradores; III – Administradores de esquemas; IV – Administradores corporativos.  
C) I – Administradores corporativos; II – Administradores do domínio; III – Administradores de esquemas; IV – Administradores.  
D) I – Administradores do domínio; II – Administradores de esquemas; III – Administradores; IV – Administradores corporativos.  
E) I – Administradores corporativos; II – Administradores de esquemas; III – Administradores do domínio; IV – Administradores.  

**22.** (FCC · SEFAZ-BA/2019 · Auditor Fiscal — TI)  
Para iniciar o Serviço de Domínio do Active Directory via linha de comando, um Administrador de Redes Windows Server pode utilizar o comando

A) activate instance Active Directory  
B) net start ntds  
C) net start set instance ad  
D) netsh start instance ntds  
E) netstart set Active Directory on  

**23.** (FCC · TRT-24/2016 · Analista — TI)  
Consider these statements.

I. OpenSSL is an open source Project that provides a robust, commercial-grade, and full-featured toolkit for the Transport Layer Security (TLS) and Secure Sockets Layer (SSL) protocols. It is also a general-purpose cryptography library.  
II. OpenLDAP stands for Open Light Directory Access Protocol. It is a lightweight protocol for accessing directory services, specifically X.500-based directory services. LDAP runs only over TCP/IP services. The LDAP information model is based on entries. An entry is a collection of commands that has a globally-unique Distinguished Name (DN). The DN is used to refer to the entry ambiguously. In LDAP, directory entries are arranged in a non hierarchical tree-like structure.  
III. The Active Directory module for Windows PowerShell consolidates a group of cmdlets. These cmdlets can be used to manage Active Directory domains, Active Directory Lightweight Directory Services (AD LDS) configuration sets, and Active Directory Database Mounting Tool instances in a single, self-contained package.

The correct statement(s) is (are)

A) III, only.  
B) II and III, only.  
C) I, II and III.  
D) I and III, only.  
E) I, only.  

**24.** (FCC · TRF-4/2025 · Técnico — Desenvolvimento de Sistema da Informação)  
A adoção de tecnologias de computação em nuvem e de virtualização no ambiente de TI de um tribunal busca otimizar o uso de recursos, garantir escalabilidade e facilitar a manutenção de sistemas. Nesse contexto, foram implementadas soluções baseadas em modelos de serviço como IaaS, PaaS e SaaS, além da utilização de plataformas de virtualização, como VMware e Hyper-V, e de conteinerização com Docker. Para assegurar a eficiência operacional, é fundamental que as práticas de gestão da infraestrutura envolvam

A) a configuração de ambientes virtualizados utilizando VMware ou Hyper-V garantindo o provisionamento ágil de recursos e maior controle sobre a distribuição da carga de trabalho em servidores.  
B) a adoção do modelo IaaS para determinadas soluções de produtividade, reduzindo a necessidade de gerenciamento local, mesmo desprezando o uso de camadas inferiores da infraestrutura.  
C) a implantação de contêineres Docker em ambientes controlados, com orquestração simplificada, mesmo na ausência de integração imediata com sistemas de monitoramento centralizado.  
D) o uso de mecanismos de autoescalabilidade configurados com base em métricas de utilização, isento do acompanhamento financeiro periódico e monitoramento em tempo real.  
E) a combinação de conteinerização com infraestrutura virtualizada, ainda que dispense isolamento dedicado em casos que envolvam dependência direta de hardware ou configurações específicas de sistema operacional.  

**25.** (FCC · AL-PE/2014 · Analista Legislativo — Infraestrutura)  
O Centro Administrativo do Active Directory no Windows Server 2008 R2

A) combina os recursos que os administradores usam para visitar múltiplas áreas, em uma única interface coesa. Com base em line command over P-State ele permite que os administradores enxerguem de modo transparente as atividades rodando em PowerShell over IIS.  
B) compartilha os recursos que os administradores usam para monitorar a rede. Com base em interface gráfica do usuário (GUI) ele permite que os administradores enxerguem de modo transparente as atividades rodando em P-State.  
C) compartilha os recursos que os administradores usam para monitorar a rede. Com base em command line ele permite que os administradores enxerguem de modo transparente as atividades rodando em P-State over IIS.  
D) combina os recursos que os administradores usam para visitar múltiplas áreas, em uma única interface coesa. Com base em network command over P-State ele permite que os administradores enxerguem de modo transparente as atividades rodando em P-State.  
E) combina os recursos que os administradores usam para visitar múltiplas áreas, em uma única interface coesa. Ele foi escrito inteiramente com base no PowerShell, ou seja, todas as tarefas podem ser feitas tanto na linha de comando como na interface gráfica do usuário (GUI).  

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **D** | gpupdate /force |
| 17 | **E** | Firewall avançado / Nova Regra |
| 18 | **B** | Procurar driver no computador |
| 19 | **D** | msconfig |
| 20 | **B** | SMB / 445–139 |
| 21 | **B** | Domain Admins → Admins → Schema → Enterprise |
| 22 | **B** | net start ntds |
| 23 | **D** | I e III (II erra OpenLDAP/LDAP) |
| 24 | **A** | Hyper-V/VMware provisionamento |
| 25 | **E** | ADAC baseado em PowerShell + GUI |

*Arquivo 04 · Windows 10/11 + Server 2019/2022/2025 · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 05 Active Directory.*

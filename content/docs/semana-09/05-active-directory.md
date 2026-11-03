+++
title = '05 — Active Directory'
date = '2026-10-28'
draft = false
weight = 5
tags = ['semana-09', 'active']
categories = ['estudo']
+++
# Active Directory · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — Active Directory (AD DS)  
**Escopo deste arquivo:** floresta / árvore / domínio / site; OU; objetos; GPO (vínculo, LSDOU, herança); DNS para AD (zonas integradas, SRV); autenticação (Kerberos/NTLM, KDC, GC); papéis FSMO; cmdlets/dsadd cobráveis.  
**Fora de escopo aqui:** PowerShell genérico e WSUS (arquivo 06); Linux/IdM; Azure AD / Entra ID em profundidade; AD CS/AD FS detalhado.

---

## 1. Material

### 1.1 Por que a FCC cobra AD

No bloco de **Windows Server** do edital TRT4 (código 14), Active Directory é o núcleo de **identidade on-premises**: login único no domínio, organização de usuários/computadores, aplicação de políticas e dependência crítica de **DNS**. A banca raramente pede “clicar no snap-in”: ela quer **hierarquia correta**, **onde se vincula GPO**, **papel da OU**, **Kerberos × NTLM**, **catálogo global** e **comando certo** (`dsadd ou` / `New-ADOrganizationalUnit`).

Cartão mental:

| Conceito | Flash cobrável |
|----------|----------------|
| **Floresta** | Limite de segurança; esquema/configuração/GC comuns |
| **Árvore** | Domínios em namespace DNS contínuo |
| **Domínio** | Limite de administração/replicação de partição de domínio |
| **Site** | Topologia física (LAN/WAN) para replicação e localização de DC |
| **OU** | Contêiner administrativo **dentro** do domínio; GPO + delegação |
| **GPO** | Vincula a **site, domínio, OU** (não a grupo direto) |
| **DNS** | Localiza DC via registros **SRV** (`_ldap`, `_kerberos`, `_msdcs`) |

### 1.2 Hierarquia: floresta → árvore → domínio → OU

**Floresta (*forest*):** conjunto de uma ou mais árvores que compartilham **esquema**, **configuração** e **catálogo global**. Mesmo com um único domínio, a floresta **existe** (erro clássico: “floresta some se há só uma árvore”).

**Árvore (*tree*):** domínios em relação pai–filho com **namespace DNS contínuo** (ex.: `tribunal.jus.br` → `poa.tribunal.jus.br`).

**Domínio:** unidade de autenticação e de partição de diretório. Objetos (usuários, grupos, computadores, OUs) vivem no domínio. Confiança **transitiva** automática entre domínios da mesma floresta.

**Site:** agrupamento de sub-redes IP bem conectadas. Não é “pasta de usuários”: serve a **replicação intersite**, **localização de DC** e vínculo de GPO por localização física.

Anti-confusão FCC:

1. **OU ≠ domínio** — OU não contém outros domínios.  
2. **OU ≠ floresta** — nomes diferentes, conceitos diferentes.  
3. **Site ≠ OU** — site = topologia; OU = estrutura lógica administrativa.  
4. **Grupo ≠ OU** — grupo agrega SIDs para ACL; OU organiza e recebe GPO.

### 1.3 Unidade Organizacional (OU)

OU é o **recipiente** dentro do domínio que pode conter usuários, grupos, computadores e **outras OUs** (hierarquia). Papéis cobráveis:

| Uso | Por quê |
|-----|---------|
| Organizar | Espelhar organograma / local / função |
| **Delegar** administração | ACL na OU (sem dar Domain Admin) |
| **Vincular GPO** | Escopo fino de políticas |

**Controle** sobre a OU e objetos internos: determinado pelas **listas de controle de acesso (ACL)** na OU e nos objetos (clássico TRT-14/2022). Não é “só o GC”, “só replicação” ou “catálogo local do cliente”.

Criação em linha de comando (legado `ds*` ainda cai):

```text
dsadd ou "ou=Impressoras,dc=Advogados,dc=com,dc=br"
dsadd ou "ou=Servidores,dc=tjma,dc=com,dc=br"
```

PowerShell (módulo AD):

```powershell
New-ADOrganizationalUnit -Name "Judicial" -Path "DC=tribunal,DC=jus"
```

Distratores FCC: `New-ADComputer`, `New-ADGroup`, `Set-ADUser`, `Get-ADOU` (consulta, não cria), `dsset`/`adadd` inventados, vírgulas trocadas por `;`.

### 1.4 Objetos e nomes

Objetos típicos: usuário, grupo, computador, impressora, contato, OU, conta de serviço. Identificadores:

| Forma | Exemplo | Uso |
|-------|---------|-----|
| **DN** (Distinguished Name) | `CN=Ana,OU=TI,DC=trt4,DC=jus,DC=br` | Caminho LDAP completo |
| **SAM** | `TRT4\ana` | Logon legado |
| **UPN** | `ana@trt4.jus.br` | Logon moderno |
| **SID** | `S-1-5-21-…` | Segurança (ACL) |
| **GUID** | identificador global | Referências estáveis |

Grupos: **segurança** (ACL) × **distribuição** (e-mail). Escopos: **Domain Local**, **Global**, **Universal** — cobráveis em combinação com GC (grupos universais exigem GC no logon em florestas multi-domínio).

### 1.5 Controladores de domínio, GC e FSMO

**DC (Domain Controller):** autentica, autoriza pesquisas LDAP e replica partições do AD. Modelo **multi-master** para a maior parte das escritas (desde Windows 2000+).

**Catálogo global (GC):** DC com **réplica parcial** de **todos** os objetos da floresta — acelera buscas e logon (grupos universais). Pode-se colocar GC em vários DCs para **equilibrar tráfego** de autenticação/consultas (clássico TCE-PR). Em floresta de domínio único, costuma-se marcar todos os DCs como GC.

**FSMO** (Flexible Single Master Operations) — 5 papéis:

| Papel | Escopo | Ideia |
|-------|--------|-------|
| **Schema Master** | Floresta | Altera o esquema |
| **Domain Naming Master** | Floresta | Adiciona/remove domínios |
| **PDC Emulator** | Domínio | Senhas/lockout; tempo; apps que “acham” PDC |
| **RID Master** | Domínio | Aloca blocos de RID |
| **Infrastructure Master** | Domínio | Atualiza referências cross-domain |

Flash: Schema + Domain Naming = **floresta**; os outros três = **por domínio**. Infrastructure Master e GC no mesmo DC é problema **exceto** domínio único / todos DCs são GC.

**RODC:** DC somente leitura (filiais); não confundir com GC.

### 1.6 DNS e Active Directory (ouro de prova)

AD **depende** de DNS para localizar serviços. Clientes consultam registros **SRV**, tipicamente sob `_msdcs`:

- `_ldap._tcp.dc._msdcs.<domínio>`  
- `_kerberos._tcp.dc._msdcs.<domínio>`  
- GC: registros sob `gc._msdcs.<floresta>`

**Zona integrada ao AD:** dados da zona guardados no diretório e **replicados com o AD** (multi-master entre DCs DNS); favorece atualizações dinâmicas **seguras**. DNS **pode** existir sem AD (solução autônoma), mas AD **não** funciona bem sem DNS correto.

Erros típicos cobrados indiretamente: cliente apontando DNS público em vez do DC; SRV ausentes → logon/GPO quebram; confundir porta LDAP (389) com GC (3268).

### 1.7 Autenticação: Kerberos, NTLM, KDC

| | **Kerberos** (padrão no domínio) | **NTLM** (legado/fallback) |
|---|----------------------------------|----------------------------|
| Modelo | Tickets; KDC confiável | Challenge/response |
| Servidor central | **KDC** no DC (conta **KRBTGT**) | Conta local/domínio |
| Rede | Mais eficiente; SSO | Mais antigo; ainda aparece |

Fluxo mental Kerberos: cliente pede **TGT** ao KDC → usa TGT para pedir **ticket de serviço** → apresenta ao servidor de recurso. Conta **KRBTGT** é criada com o domínio; senha conhecida pelo serviço Kerberos.

AD oferece **autenticação unificada** (SSO no domínio) **e** Kerberos (AL-SP/2010). LDAP consulta/atualiza o diretório; **não** substitui Kerberos como protocolo de autenticação de logon.

### 1.8 GPO — Group Policy Object

GPO armazena configurações de **usuário** e **computador**. **Vincula-se** a:

- **Site**  
- **Domínio**  
- **OU** (incluindo OUs aninhadas)

**Não** se “liga” GPO diretamente a um **grupo de segurança** como contêiner de vínculo (filtragem por segurança/WMI é outro mecanismo). Vários GPOs podem vincular-se à mesma OU.

**Ordem de aplicação (LSDOU)** — quem ganha por último prevalece (salvo Enforce/Block):

1. **L**ocal  
2. **S**ite  
3. **D**omain  
4. **OU** (da mais alta à mais profunda)

Níveis clássicos citados em prova: segurança local, site, domínio, OU pai, OU filha (TRE-MS/2007).

Configurações típicas: papel de parede, política de senha (também Domain Policy), desativar prompt, mapeamento de drive, WSUS via GPO (`Specify intranet Microsoft update service location`).

Armadilhas:

- “OU é o **maior** recipiente do AD” → **falso** (floresta/domínio maiores).  
- “GPO no site atinge todos os domínios da floresta automaticamente como escopo ideal de senha” → cuidado: site é físico; política de senha de domínio tem regras próprias.  
- Delegar autoridade administrativa à **OU** (ACL), não “delegar a um GPO” como se GPO fosse contêiner de direitos.

### 1.9 Replicação e sites (visão cobrável)

Dentro do site: replicação frequente. Entre sites: **site links**, calendário e custo. Conhecimento mínimo: problemas de GPO “não aplica em uma cidade” podem ser **replicação/site**, DNS ou filtragem — não “ligar GPO a um grupo”.

### 1.10 Família AD (só para não cair em distrator)

| Papel | Função |
|-------|--------|
| **AD DS** | Domínio / diretório principal |
| **AD LDS** | Diretório LDAP leve (app), sem domínio completo |
| **AD CS** | Certificados PKI |
| **AD FS** | Federação / SSO entre organizações |
| **AD RMS** | Rights Management |

Quando a prova pede login único no domínio e objetos da rede → **AD DS (Domain Services)**.

### 1.11 Mapa anti-confusão FCC

| Confunde | Com | Flash |
|----------|-----|-------|
| OU | Domínio / floresta | OU só dentro do domínio |
| GPO link | Grupo | Link = site/domínio/OU |
| GC | FSMO | GC é função de catálogo; FSMO são 5 papéis |
| DNS zona secundária | Zona AD-integrated | Integrada ≠ exigir primária+secundária clássicas |
| Kerberos | “AD não usa Kerberos” | Usa Kerberos + unificação |
| `New-ADOrganizationalUnit` | `New-ADGroup` / `New-ADComputer` | Verbo + substantivo do objeto |

### 1.12 Síntese cobrável

Floresta ⊃ árvore ⊃ domínio ⊃ OU; GPO em site/domínio/OU (LSDOU); DNS/SRV obrigatório; Kerberos via KDC; GC para floresta; FSMO 2 floresta + 3 domínio; criar OU com `dsadd ou "ou=…,dc=…"` ou `New-ADOrganizationalUnit`.

---

## 2. Questões de treino (1–15)

**1.** No Active Directory, a Unidade Organizacional (OU) é melhor descrita como:

A) Um domínio filho automático  
B) Sinônimo exato de floresta  
C) Contêiner **dentro do domínio** que pode aninhar usuários, grupos, computadores e outras OUs  
D) Registro SRV de Kerberos  
E) Papel FSMO de floresta  

**2.** GPOs podem ser vinculados, no modelo clássico, a:

A) Apenas grupos de distribuição  
B) Sites, domínios e OUs  
C) Somente partições de esquema  
D) Apenas contas KRBTGT  
E) Somente zonas DNS secundárias  

**3.** A ordem padrão de processamento de políticas (LSDOU) é:

A) OU → Domain → Site → Local  
B) Site → Local → OU → Domain  
C) Domain → Site → Local → OU  
D) Local → Site → Domain → OU  
E) Forest → Tree → Domain → Site  

**4.** O protocolo de autenticação padrão preferencial em domínio AD moderno é:

A) FTP anônimo  
B) Kerberos (com KDC no DC)  
C) Telnet sem cifra  
D) SMB1 obrigatório  
E) POP3  

**5.** O Catálogo Global (GC) armazena, em essência:

A) Somente senhas em texto claro  
B) Réplica **parcial** de objetos de **toda a floresta**  
C) Apenas a partição de configuração sem usuários  
D) Exclusivamente zonas DNS externas  
E) Somente GPOs locais do cliente  

**6.** Schema Master e Domain Naming Master são papéis FSMO de escopo:

A) Por site apenas  
B) Por OU apenas  
C) De **floresta**  
D) Exclusivos de workgroup  
E) Apenas de RODC  

**7.** DNS é crítico para o AD principalmente porque:

A) Substitui o LDAP em todas as consultas  
B) Clientes localizam DCs/serviços via registros **SRV** (ex.: `_ldap`, `_kerberos` em `_msdcs`)  
C) Elimina a necessidade de Kerberos  
D) Impede qualquer uso de GPO  
E) Remove a floresta automaticamente  

**8.** Assinale a afirmativa **incorreta**:

A) Uma OU pode conter outras OUs  
B) Uma OU pode conter múltiplos **domínios**  
C) GPOs vinculam-se a OUs  
D) ACLs controlam delegação na OU  
E) `dsadd ou` cria Unidade Organizacional  

**9.** A conta KRBTGT relaciona-se a:

A) Serviço de impressão spooler apenas  
B) Autenticação **Kerberos** / KDC no domínio  
C) Substituição do DHCP  
D) Criação de VLAN nativa  
E) Certificação CMMI  

**10.** Em floresta com um único domínio, é correto afirmar que:

A) A floresta deixa de existir  
B) A floresta **existe**; o domínio único ainda pertence a uma floresta  
C) Não há esquema  
D) DNS torna-se proibido  
E) Não há DC possível  

**11.** O comando PowerShell adequado para criar a OU “Judicial” na raiz do domínio `tribunal.jus` é:

A) `New-ADComputer -Name "Judicial" -Path "OU=Judicial,DC=tribunal,DC=jus"`  
B) `New-ADGroup -Name "Judicial" -GroupScope Global`  
C) `Set-ADUser -Identity "Judicial"`  
D) `New-ADOrganizationalUnit -Name "Judicial" -Path "DC=tribunal,DC=jus"`  
E) `Get-ADOU -Identity "OU=Judicial,DC=tribunal,DC=jus"`  

**12.** Relação correta AD DS × AD FS:

A) São o mesmo papel com nomes comerciais idênticos  
B) AD DS = domínio/diretório; AD FS = federação/SSO entre entidades  
C) AD FS substitui DNS  
D) AD DS só emite certificados  
E) Ambos são apenas antivírus  

**13.** Filtrar quem recebe uma GPO por grupo de segurança:

A) É o mesmo que vincular a GPO ao grupo como contêiner  
B) É mecanismo de **segurança/filtragem**, distinto do vínculo a site/domínio/OU  
C) Elimina a necessidade de DC  
D) Remove o Kerberos  
E) Cria automaticamente uma floresta nova  

**14.** Qual opção lista apenas conceitos de topologia/localização no AD?

A) Site e site link  
B) Spring e Eureka  
C) MEA e DSS  
D) POP e IMAP  
E) Rancher e CircleCI  

**15.** Síntese correta:

A) Floresta/domínio/OU; GPO em site/domínio/OU (LSDOU); DNS/SRV; Kerberos+KDC; GC; FSMO; `dsadd ou` / `New-ADOrganizationalUnit`  
B) OU contém florestas inteiras  
C) GPO só vincula a grupo  
D) AD não usa DNS  
E) Kerberos inexistente no AD  

---

### Gabarito comentado (treino 1–15)

1. **C** — Contêiner no domínio, aninhável.  
2. **B** — Site, domínio, OU.  
3. **D** — LSDOU.  
4. **B** — Kerberos/KDC.  
5. **B** — Réplica parcial florestal.  
6. **C** — Escopo floresta.  
7. **B** — SRV/_msdcs.  
8. **B** — Incorreta: OU não contém domínios.  
9. **B** — Kerberos/KRBTGT.  
10. **B** — Floresta existe mesmo com 1 domínio.  
11. **D** — `New-ADOrganizationalUnit`.  
12. **B** — DS × FS.  
13. **B** — Filtragem ≠ vínculo.  
14. **A** — Site/site link.  
15. **A** — Síntese do arquivo.

---

## 3. Questões de concurso (16–25)

**16.** (FCC · TRT-6/2025 · Analista Judiciário — TI)  
Uma equipe de Analistas configurou o ambiente de rede do Tribunal Regional do Trabalho no Windows Server 2019, instalado e funcionando em condições ideais. Eles precisam configurar o Active Directory para centralizar a administração de usuários e computadores. Um dos administradores, com as devidas permissões, deseja criar uma nova Unidade Organizacional (OU) chamada "Judicial" dentro do Active Directory, para organizar os recursos de maneira estruturada. O comando PowerShell para isso é:

A) `New-ADComputer -Name "Judicial" -Path "OU=Judicial,DC=tribunal,DC=jus"`  
B) `Get-ADOU -Identity "OU=Judicial,DC=tribunal,DC=jus"`  
C) `New-ADGroup -Name "Judicial" -GroupScope Global -Path "OU=Users,DC=tribunal,DC=jus"`  
D) `New-ADOrganizationalUnit -Name "Judicial" -Path "DC=tribunal,DC=jus"`  
E) `Set-ADUser -Identity "Judicial" -OrganizationalUnit "OU=Judicial,DC=tribunal,DC=jus"`  

**17.** (FCC · TRT-14/2022 · Analista Judiciário — TI)  
As Unidades Organizacionais (UOs) no Active Directory podem ser usadas para formar uma hierarquia de contêineres dentro de um domínio. O controle sobre uma UO e os objetos dentro dela é determinado

A) pelas listas de controle de acesso na UO e pelos objetos na UO.  
B) pelo mecanismo de consulta e pelo índice.  
C) pelo serviço de replicação que distribui dados de diretório em uma rede.  
D) por um catálogo global que contém informações sobre cada objeto no diretório.  
E) pelo catálogo local de máquina cliente Windows.  

**18.** (FCC · TRE-MS/2007 · Técnico Judiciário — Operação de Computadores)  
Em relação a infra-estrutura de domínios do Active Directory, considere as afirmativas abaixo.

I. Uma OU (Unidade Organizacional) é um recipiente dentro do domínio do Active Directory, que pode conter usuários, grupos, computadores e outras unidades organizacionais, conhecidas como OUs filhas.  
II. É possível vincular vários GPOs (Objetos de Diretiva de Grupo) a uma OU, que é o maior recipiente na hierarquia do Active Directory, como também, é possível delegar autoridade administrativa a um GPO.  
III. As configurações da Diretiva de Grupo estão armazenadas nos GPOs dos controladores de domínio do ambiente. Os GPOs estão vinculados a recipientes, o que inclui sites, domínios e OUs do Active Directory.

É correto o que se afirma APENAS em:

A) I.  
B) I e II.  
C) I e III.  
D) II.  
E) III.  

**19.** (FCC · CREMESP/2016 · Analista de Suporte)  
Considere que um Analista de Suporte, como administrador, realizou as seguintes configurações de GPO em uma rede Windows Server baseada em Active Directory:

I. Especificar o papel de parede padrão do desktop.  
II. Os usuários mudarem a senha a cada 50 dias.  
III. Desativar o prompt de comando.

A GPO

A) III, para atingir todos os seus usuários e grupos, deve ser configurada dentro de uma OU − Organizational Unit.  
B) I, para atingir apenas os seus usuários, deve ser configurada dentro do domínio.  
C) III está na categoria Hardware Settings e II na categoria Windows Settings, embora sejam iguais para usuários e computadores.  
D) pode ser configurada como Active ou Inactive para ser ativada ou desativada e isso é válido para I, II e III.  
E) II, para atingir todos os domínios, deve ser configurada no site.  

**20.** (FCC · AL-SP/2010 · Agente Técnico Legislativo Especializado — Segurança de Redes)  
A tecnologia Microsoft Active Directory é um serviço de diretório que

A) oferece autenticação unificada de usuários para acesso aos recursos da rede, mas não oferece autenticação baseada no protocolo Kerberos.  
B) oferece tanto autenticação unificada de usuários para acesso aos recursos da rede quanto autenticação baseada no protocolo Kerberos.  
C) oferece autenticação baseada no protocolo Kerberos mas não oferece autenticação unificada de usuários para acesso aos recursos da rede.  
D) não oferece sincronização de diretórios entre diferentes servidores.  
E) oferece seus serviços por meio do armazenamento distribuído dos dados das aplicações.  

**21.** (FCC · TRE-MS/2007 · Analista Judiciário — Análise de Sistemas)  
Na arquitetura de segurança do Windows 2003 Server, é possível utilizar os recursos do Active Directory para segmentar a gestão de servidores, clusters de usuários e grupos de segurança através da criação de Objetos de Diretivas de Grupo (GPO’s) e sua delegação a grupos administrativos para uma gestão descentralizada. Estas GPO’s podem ser definidas nos níveis de

A) segurança local, site, domínio, unidade organizacional pai, unidade organizacional filha.  
B) servidor local, servidor pai, servidor distribuído, servidor de comunicação, servidor de domínio.  
C) computador local, servidor de aplicações, servidor de banco de dados, servidor web.  
D) segurança local, cluster, workgroup, domínio, site.  
E) segurança imediata, segurança tardia, segurança distribuída, segurança total.  

**22.** (FCC · TRF-3/2019 · Técnico Judiciário — Informática)  
No Windows Server 2008 R2, em inglês, para instalar o Active Directory abre-se o Server Manager e clica-se em Add Roles para adicionar uma função (ou papel). Aparecerá um aviso onde se deve clicar no botão Next para se chegar à janela de seleção da Server Role que se deseja instalar no servidor. A Role que deve ser selecionada quando se quer armazenar informações sobre objetos na rede e disponibilizar essas informações para usuários e administradores, usando controladores para conceder aos usuários o acesso a recursos permitidos em qualquer local da rede por meio de um processo de login único, é a Active Directory

A) Domain Services.  
B) Certificate Services.  
C) Federation Services.  
D) Lightning Directory Services.  
E) Rights Management Services.  

**23.** (FCC · TRF-3/2019 · Técnico Judiciário — Informática)  
Um Analista foi encarregado de criar, em linha de comando, a Unidade Organizacional "Impressoras" na raiz de um domínio. Considerando que a rede utiliza o Windows Server 2008 R2 e que a organização tem nome de domínio Advogados.com.br, utilizou corretamente o comando

A) `dsset ou = "Impressoras" dc= "Advogados" dc= "com " dc = " br "`  
B) `dsadd ou "ou=Impressoras,dc=Advogados,dc=com,dc=br"`  
C) `adset ou = " Impressoras " dc = "Advogados.com.br "`  
D) `adadd" ou = Impressoras" "dc= Advogados ,dc= com ,dc= br "`  
E) `addou "ou=Impressoras" "dc=Advogados;dc=com;dc=br"`  

**24.** (FCC · TJ-MA/2019 · Analista Judiciário — Analista de Sistemas Suporte e Rede)  
Uma organização que utiliza o Active Directory no Windows Server 2008 possui o nome de domínio tjma.com.br. Em linha de comando, em condições ideais, para criar uma Unidade Organizacional chamada Servidores na raiz do domínio, utiliza-se o comando:

A) `dsadd unit "unit=Servidores,domain=tjma,domain=com,domain=br"`  
B) `dsadd ou "ou=Servidores,dc=tjma,dc=com,dc=br"`  
C) `adaddunit ou "unit=Servidores,domain=tjma,subdomain=com,subdomain=br"`  
D) `dsadd ou "unit=Servidores,group=tjma,group=com,group=br"`  
E) `dsaddunit ou "ou=Servidores,dc=tjma,dc=com,dc=br"`  

**25.** (FCC · TCE-PR/2011 · Analista de Controle)  
Em relação ao Active Directory é correto afirmar:

A) A floresta inexiste quando há apenas uma árvore de domínio.  
B) Servidores de catálogo global podem ser configurados para equilibrar o tráfego de autenticação de logon e consultas.  
C) A definição do servidor de catálogo global por padrão, é atribuição exclusiva das contas de Administrador.  
D) Zonas primárias e secundárias de serviços DNS são requeridas, para serem replicadas em conjunto com o AD.  
E) As contas de Administrador têm permissão de acesso tanto nos recursos do domínio quanto nos recursos da floresta.  

---

### Gabarito comentado (concurso 16–25)

16. **D** — `New-ADOrganizationalUnit` na raiz do domínio.  
17. **A** — Controle via ACLs na OU e nos objetos.  
18. **C** — I e III; II erra ao chamar OU de “maior recipiente” e ao “delegar a um GPO”.  
19. **A** — Escopo de GPO III via OU.  
20. **B** — Autenticação unificada **e** Kerberos.  
21. **A** — Local / site / domínio / OU pai / OU filha.  
22. **A** — AD DS (Domain Services).  
23. **B** — `dsadd ou` com DN correto (vírgulas).  
24. **B** — `dsadd ou "ou=Servidores,dc=tjma,dc=com,dc=br"`.  
25. **B** — GC para equilibrar autenticação/consultas.

---

**Caderno de erros (sugerido):** OU×domínio×floresta · vínculo GPO × grupo · LSDOU · SRV/`_msdcs` · Kerberos/KDC/KRBTGT · GC×FSMO · `dsadd ou` × `New-ADOrganizationalUnit` × distratores New-AD*.

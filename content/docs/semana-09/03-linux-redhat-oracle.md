+++
title = '03 — Linux Red Hat e Oracle Linux'
date = '2026-10-26'
draft = false
weight = 3
tags = ['semana-09', 'linux']
categories = ['estudo']
+++
# Linux Red Hat e Oracle Linux · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — Linux (Red Hat Enterprise Linux e Oracle Linux)  
**Escopo deste arquivo:** administração cobrável em RHEL/Oracle Linux — usuários/grupos, permissões DAC, systemd, pacotes (rpm/yum/dnf), SELinux (modos e política), firewalld/Cockpit, fstab/NFS noções.  
**Fora de escopo aqui:** processos/threads/memória virtual (arquivo 01–02 da semana 9); Windows 10/11/Server (arquivo 04); Docker/K8s (semana 3); proxy Apache (semana 7).

---

## 1. Material

### 1.1 Por que RHEL e Oracle Linux caem juntos

O edital TRT4 cita **Linux (Red Hat e Oracle Linux)**. Em prova, trate-os como a **família RPM + systemd + SELinux**:

| Peça | Leitura FCC |
|------|-------------|
| **RHEL** | Distribuição enterprise Red Hat; base de referência |
| **Oracle Linux** | Compatível em **binários de aplicação** com RHEL; kernels UEK (padrão Oracle) e RHCK (compatível Red Hat) |
| **Pacotes** | `rpm` (baixo nível) · `yum` (legado alto nível) · `dnf` (atual em RHEL 8+) |
| **Init** | **systemd** / `systemctl` (não SysVinit/`chkconfig` como resposta “moderna”) |
| **MAC** | **SELinux** (enforcing / permissive / disabled) — **não** é firewall de rede |

Cartão mental: se o enunciado fala **Debian/Ubuntu**, a resposta com `apt`/`dpkg` é distrator; se fala **RHEL/Oracle/CentOS/Fedora**, pense `dnf`/`rpm`/`systemctl`/`SELinux`.

### 1.2 Usuários, grupos e arquivos críticos

Criar conta e senha (clássico FCC): **`useradd`** depois **`passwd`**. Remoção: `userdel`; grupos: `groupadd` / `usermod -g` / `usermod -aG`.

Arquivos cobráveis:

| Arquivo | Conteúdo |
|---------|----------|
| **`/etc/passwd`** | login, UID, GID, GECOS, home, shell — **legível**; campo de senha costuma ser `x` |
| **`/etc/shadow`** | **senhas criptografadas** + aging (root-only) |
| **`/etc/group`** | grupos e membros |
| **`/etc/sudoers`** (e `sudoers.d/`) | privilégio elevado via sudo |

Linha típica de `/etc/passwd`:

```text
usuario:x:1001:1001:Nome Completo:/home/usuario:/bin/bash
```

Campos: login · indicador de senha · **UID** · **GID** · comentário · home · shell.  
**Flash FCC:** `x` → senha em **`/etc/shadow`**; confundir UID↔GID é distrator favorito (terceiro campo = UID; quarto = GID).

### 1.3 Permissões DAC (chmod / chown / umask)

Três classes: **u** (dono), **g** (grupo), **o** (outros). Bits: **r=4, w=2, x=1**.

| Octal | Significado |
|-------|-------------|
| 7 | rwx |
| 6 | rw- |
| 5 | r-x |
| 4 | r-- |
| 0 | --- |

Exemplo clássico: dono total, grupo só leitura, outros nada → **`chmod 740 arquivo`**.  
`ls -l` mostra `-rw-rw-r--`: sem `x` → **não há execução** nem para o dono.  
`chown usuario:grupo` muda dono/grupo. Em diretório, `x` = atravessar/entrar.

**DAC × SELinux:** `chmod`/`chown` são **discricionários**. SELinux é **MAC** — política adicional; **não** torna `chmod` inoperante e **não** é firewall de rede.

### 1.4 systemd — systemctl

No RHEL moderno o init é **systemd**. Comandos cobráveis:

| Objetivo | Comando |
|----------|---------|
| Status | `systemctl status nome` |
| Iniciar agora | `systemctl start nome` |
| Parar | `systemctl stop nome` |
| Reiniciar (stop+start) | `systemctl restart nome` |
| Recarregar config **sem** reinício completo | `systemctl reload nome` |
| Habilitar no boot | `systemctl enable nome` |
| Desabilitar no boot | `systemctl disable nome` |
| Enable + start | `systemctl enable --now nome` |

**Anti-confusão FCC (TCE-GO/2022):** `systemctl` invoca **systemd**, não SysVinit; **`reload` ≠ `restart`** (“reinicializar”).  
Legado: `service`, `chkconfig` — aparecem como distratores frente a `systemctl`.

Logs: `journalctl`; também `tail -f /var/log/messages | grep -i error` (case-insensitive com `-i`).

### 1.5 Pacotes: rpm · yum · dnf

| Camada | Ferramenta | Papel |
|--------|------------|-------|
| Baixo nível | **`rpm`** | instala/consulta/verifica `.rpm` (dependências manuais) |
| Alto nível (legado) | **`yum`** | resolve dependências via repositórios |
| Alto nível (atual) | **`dnf`** | sucessor do yum no RHEL 8+ / Fedora Linux recente |

Flash moderno (TRT-1/2025): instalar e habilitar no boot → `dnf install nginx` + `systemctl enable nginx`.  
Distratores: `apt-get` (Debian), `systemctl nginx start` (ordem errada), `chkconfig`, `grep -all`.

Mapa Debian × Red Hat (para não trocar):

- Debian: `dpkg` (baixo) + `apt`/`apt-get` (alto)  
- Red Hat: `rpm` (baixo) + `yum`/`dnf` (alto)

### 1.6 SELinux — o que a FCC cobra

**SELinux** = Mandatory Access Control: processos e arquivos têm **contexto** (user:role:type:level); a **política** diz o que o type do processo pode fazer com o type do objeto. Complementa DAC; root também pode ser restringido pela política.

| Modo | Comportamento |
|------|----------------|
| **enforcing** | política aplicada; violações **bloqueadas** e logadas (AVC) |
| **permissive** | violações **só logadas** (não bloqueia) — útil para depurar |
| **disabled** | política não carregada (evitar em produção) |

Comandos: `getenforce`, `sestatus`, `setenforce 1|0` (temporário). Persistente: editar **`/etc/selinux/config`** (`SELINUX=enforcing`) e **reiniciar**.

**Flash FCC SEFAZ-PI/2025:** SELinux **não** é firewall de rede; políticas são **conjuntos de regras**; **não** substituem `chmod`/`chown`; **não** “só valem para não-root”.  
**Flash FCC TRF-4/2025:** em modo permissivo, reforçar segurança → `SELINUX=enforcing` em `/etc/selinux/config` + reboot (não `service selinux reload` em path inventado).

### 1.7 firewalld e Cockpit (RHEL)

**firewalld** — firewall dinâmico; CLI: `firewall-cmd`. Persistência: `--permanent` + `--reload`.  
Cockpit (console web RHEL, porta **9090**): instalar pacote, `systemctl enable --now cockpit.socket`, liberar no firewall:

```bash
firewall-cmd --add-service=cockpit --permanent
firewall-cmd --reload
```

Distratores: `firewalld-cmd`, `systemctl enable --service` sem iniciar, editar `cockpit.conf` no lugar de instalar o RPM.

### 1.8 fstab e montagem (noção)

`/etc/fstab` define montagens no boot. Ordem dos campos: **dispositivo/origem · ponto de montagem · tipo · opções · dump · fsck**.  
NFS: `servidor:/caminho/remoto /ponto local nfs opções...` — origem **antes** do destino (FCC TRT-11/2017).

### 1.9 Oracle Linux — diferenças cobráveis (rasas)

- Compatibilidade binária de aplicação com RHEL (apps certificados RHEL tipicamente rodam sem recompilação).  
- Escolha de kernel **UEK** (Unbreakable Enterprise Kernel, padrão Oracle) vs **RHCK** (Red Hat Compatible Kernel).  
- Mesma família de administração: `dnf`/`rpm`, systemd, SELinux, firewalld.  
Não invente “Oracle Linux = só banco Oracle” nem “proibido SELinux”.

### 1.10 Distratores favoritos da FCC

1. Trocar `dnf`/`yum` ↔ `apt-get`.  
2. `systemctl` = SysVinit; `reload` = `restart`.  
3. SELinux = firewall; SELinux “anula” chmod.  
4. UID ↔ GID em `/etc/passwd`.  
5. Senha em `/etc/passwd` (moderno: `shadow`).  
6. `firewalld-cmd` / sintaxe inventada de `systemctl`.  
7. Ordem invertida em linha NFS do `fstab`.

### 1.11 Mini-roteiro (15 segundos)

1. Pacote em RHEL atual? → **`dnf`** (+ `systemctl enable`).  
2. Serviço agora × no boot? → **`start`** × **`enable`**.  
3. Recarregar config sem derrubar? → **`reload`**.  
4. Dono/grupo/outros octal? → rwx = 4+2+1.  
5. Senha onde? → **`/etc/shadow`**.  
6. SELinux bloqueia × só loga? → **enforcing** × **permissive**.  
7. Persistência SELinux? → **`/etc/selinux/config`** + reboot.

---

## 2. Questões de treino (1–15)

**1.** Em RHEL 9 / Oracle Linux recente, o gerenciador de pacotes de alto nível preferido é:

A) apt-get  
B) dpkg  
C) dnf  
D) pacman  
E) snap apenas  

**2.** Comando para criar usuário e, em seguida, definir senha:

A) usernew ; setpass  
B) useradd ; passwd  
C) adduser.exe ; net user  
D) New-ADUser ; passwd  
E) usradd ; password  

**3.** Em `/etc/passwd`, o campo `x` após o login indica tipicamente que:

A) a conta está sempre desabilitada  
B) a senha em claro está no próprio passwd  
C) a senha criptografada está em `/etc/shadow`  
D) o UID é inválido  
E) o shell é obrigatoriamente `/bin/false`  

**4.** `chmod 740 relatorio.txt` concede:

A) rwx ao dono, r ao grupo, nada aos outros  
B) rwx a todos  
C) só leitura ao dono  
D) execução apenas aos outros  
E) rw ao grupo e rwx aos outros  

**5.** Diferença cobrada `systemctl reload` × `systemctl restart`:

A) são sinônimos rigorosos  
B) reload reinicia a máquina inteira  
C) reload tenta reaplicar config sem ciclo stop/start completo; restart para e sobe de novo  
D) restart só existe no SysVinit  
E) reload exige apt-get  

**6.** SELinux em modo permissive:

A) bloqueia e não registra  
B) é um firewall de borda L3  
C) substitui chmod tornando-o inoperante  
D) registra violações (AVC) sem negar o acesso por padrão  
E) só vale para usuários não-root  

**7.** Persistência do modo enforcing do SELinux:

A) apenas `getenforce` sem reboot  
B) editar `/etc/selinux/config` (`SELINUX=enforcing`) e reiniciar  
C) `apt-get install selinux`  
D) alterar Loaded policy name em `/sys/fs/selinux` com `service selinux reload`  
E) desligar o firewalld  

**8.** Sequência alinhada a RHEL moderno para instalar nginx e habilitá-lo no boot:

A) apt-get install nginx ; chkconfig nginx on  
B) dnf install nginx ; systemctl enable nginx  
C) rpm -ivh nginx ; systemctl nginx start  
D) yum remove nginx ; service enable nginx  
E) dnf install nginx ; systemctl check nginx  

**9.** Oracle Linux, frente ao RHEL, na leitura cobrável:

A) é incompatível com pacotes RPM  
B) usa apenas apt  
C) costuma ser citado como alternativa com compatibilidade binária de aplicação com RHEL  
D) proíbe systemd  
E) elimina a necessidade de permissões rwx  

**10.** Liberar Cockpit de forma persistente no firewalld:

A) `firewalld-cmd --add-service=cockpit --additional`  
B) `firewall-cmd --add-service=cockpit --permanent` e `firewall-cmd --reload`  
C) `systemctl enableservice cockpit`  
D) `iptables -P INPUT DROP` apenas  
E) `chmod 777 /usr/bin/cockpit`  

**11.** UID 0 no Linux:

A) é sempre um usuário sem senha  
B) identifica tipicamente o root  
C) é o GID padrão de guests  
D) desativa o SELinux  
E) equivale ao grupo Domain Admins  

**12.** `systemctl enable firewalld` sem `start`:

A) inicia o serviço imediatamente e no boot  
B) apenas marca para iniciar nos próximos boots (não sobe “agora”)  
C) remove o pacote  
D) equivale a `setenforce 0`  
E) formata `/etc/shadow`  

**13.** Em `ls -l`, arquivo `-rw-rw-r--`:

A) pode ser executado pelo dono  
B) não possui bit de execução para dono, grupo nem outros  
C) é sempre um diretório  
D) tem sticky bit obrigatório  
E) equivale a chmod 777  

**14.** Ferramenta de baixo nível da família Red Hat para pacotes `.rpm`:

A) apt  
B) dpkg  
C) rpm  
D) brew  
E) winget  

**15.** Síntese alinhada ao edital para este subtópico:

A) RHEL/Oracle Linux: usuários/shadow, chmod DAC, systemd/systemctl, dnf/rpm, SELinux MAC, firewalld  
B) Oracle Linux exige apt e SysVinit  
C) SELinux substitui firewall, WAF e NAC  
D) dnf é exclusivo Debian  
E) `/etc/passwd` guarda hashes modernos no lugar do shadow  

---

### Gabarito comentado (treino 1–15)

1. **C** — dnf.  
2. **B** — useradd + passwd.  
3. **C** — shadow.  
4. **A** — 7/4/0.  
5. **C** — reload ≠ restart.  
6. **D** — permissive loga.  
7. **B** — config + reboot.  
8. **B** — dnf + enable.  
9. **C** — compatibilidade RHEL.  
10. **B** — firewall-cmd permanente + reload.  
11. **B** — root UID 0.  
12. **B** — enable ≠ start.  
13. **B** — sem x.  
14. **C** — rpm.  
15. **A** — cartão mental.

---

## 3. Questões de concurso FCC (16–25)

**16.** (FCC · TRF-4/2010 · Técnico — Informática)  
No Linux, para que o arquivo "exemplo.doc" tenha as permissões de acesso configuradas, de modo que o dono tenha direitos totais, o grupo dono somente tenha direito de leitura e os demais usuários nenhum direito, o comando chmod, com números octais, que deve ser utilizado é:

A) chmod 327 exemplo.doc  
B) chmod 431 exemplo.doc  
C) chmod 534 exemplo.doc  
D) chmod 637 exemplo.doc  
E) chmod 740 exemplo.doc  

**17.** (FCC · TRF-4/2010 · Técnico — Informática)  
Como resultado do comando ls, o Linux exibiu o arquivo:

`-rw-rw-r-- tucano users 5,3 K Jun 9 9:40 exemplo.doc`

A respeito das informações contidas no arquivo acima, é correto afirmar:

A) exemplo.doc pode ser executado por tucano, mas somente lido e modificado pelos demais usuários.  
B) as permissões exibidas conferem a tucano o controle total do arquivo exemplo.doc.  
C) não há permissão de execução do arquivo exemplo.doc nem para seu dono.  
D) exemplo.doc pode ser executado por tucano, lido e gravado pelo seu grupo, mas somente lido pelos outros.  
E) exemplo.doc pode ser lido e modificado por todos.  

**18.** (FCC · TRT-1/2025 · Analista — TI)  
Considere três situações comuns em ambientes Linux relacionados à instalação, configuração e administração de pacotes e serviços.

I. Instalar um pacote específico chamado nginx e garantir que ele seja iniciado automaticamente na inicialização do sistema.  
II. Verificar o status de um serviço chamado firewalld e reiniciá-lo.  
III. Monitorar o arquivo de log do sistema /var/log/messages, filtrando apenas as linhas que contêm a palavra "error" (independentemente de maiúsculas ou minúsculas).

Em condições ideais e com as devidas permissões, uma Analista criou a sequência correta de comandos que resolvem as tarefas I, II e III, que é:

A)
```bash
dnf install nginx
systemctl enable nginx
systemctl status firewalld
systemctl restart firewalld
tail -f /var/log/messages | grep -i error
```  
B)
```bash
rpm -ivh nginx.rpm
systemctl nginx start
service status firewalld
service restart firewalld
egrep 'error|ERROR' /var/log/messages
```  
C)
```bash
yum install nginx
chkconfig nginx on
systemctl check firewalld
systemctl restart firewalld
egrep 'error|ERROR' /var/log/messages
```  
D)
```bash
dnf install nginx
systemctl enable nginx
systemctl status firewalld
service restart firewalld
tail -f /var/log/messages | grep -all error
```  
E)
```bash
apt-get install nginx
systemctl enable nginx
chkconfig status firewalld
systemctl restart firewalld
grep -f /var/log/messages error
```  

**19.** (FCC · SEFAZ-PI/2025 · Analista do Tesouro — TI)  
O Administrador da rede Linux de uma Secretaria da Fazenda definiu controles de acesso para as aplicações, processos e arquivos do servidor Red Hat Enterprise Linux por meio do SELinux,

A) que funciona como um firewall para toda a rede Linux permitindo ou não a passagem do tráfego pelo servidor.  
B) que são aplicados por meio das permissões rwx dos comandos chmod e chown.  
C) que são conjuntos de regras que indicam ao SELinux o que pode ou não ser acessado conforme as permissões de uma política.  
D) que são aplicados apenas a usuários não-root, pois os usuários root têm sempre acesso total ao sistema.  
E) cujas regras substituem completamente as permissões tradicionais do Linux (rwx), tornando comandos como chmod e chown inoperantes.  

**20.** (FCC · TRF-4/2025 · Analista — Suporte em TI)  
No Red Hat Enterprise Linux (última versão) um analista de suporte precisa verificar em qual modo o SELinux está operando para, se necessário, reforçar a segurança do sistema. Para tanto, ele aplicou o comando sestatus, tendo como saída (trecho relevante: Current mode permissive / política carregada). Sobre a saída e o comportamento do sistema, o analista concluiu que

A) a política do SELinux não está carregada. Para carregá-la, por medida de segurança, o parâmetro Loaded policy name deve ser alterado para enforcing no arquivo /sys/fs/selinux e na sequência aplicar o comando service selinux reload para recarregar as configurações.  
B) a política SELinux está aplicada. O SELinux não nega nenhuma operação, mas registra mensagens AVC em /etc/selinux.  
C) as mensagens AVC não podem ser utilizadas para solução de problemas, depuração e melhorias na política SELinux. Para ativá-las, é necessário aplicar o comando getenforce e reiniciar o sistema.  
D) para aumentar a segurança é recomendado mudar para o modo enforcing, configurando a opção SELINUX=enforcing, localizada no arquivo /etc/selinux/config e na sequência reiniciar o sistema.  
E) o SELinux está configurado no modo permissivo. Para ativá-lo, é necessário aplicar o comando getenforce e reiniciar o sistema.  

**21.** (FCC · TRT-20/2024 · Técnico — TI)  
Um Técnico, utilizando uma máquina com o sistema operacional Linux Red Hat funcionando em condições ideais e com as devidas permissões, deseja instalar o console web RHEL 8. Para isso ele precisa seguir os seguintes passos: I. Instalar manualmente o pacote cockpit. II. Habilitar e iniciar o serviço cockpit.socket, que roda um servidor web. III. Adicionar o serviço cockpit ao firewalld para abrir a porta 9090 no firewall. O(s) comando(s) para executar o passo

A) II é: `systemctl enableservice -add-service=cockpit.socket`  
B) II é: `systemctl enable --service cockpit.socket`  
C) III é: `firewalld-cmd --add-service=cockpit --additional`  
D) I são: `sudo vi cockpit.conf` e `sudo install cockpit.service`  
E) III são: `firewall-cmd --add-service=cockpit --permanent` e `firewall-cmd --reload`  

**22.** (FCC · TCE-GO/2022 · Analista de Controle Externo — TI)  
Durante o processo de solução de problema em um determinado serviço Linux, o administrador da rede executou o comando systemctl reload cups.service. Acerca do comando executado e do daemon de inicialização invocado, avalie as seguintes afirmativas e a relação proposta entre elas.

I. O daemon de inicialização que está sendo invocado é o Sysvinit.  
PORQUE  
II. O comando executado tem como objetivo reinicializar o serviço do servidor de impressão.

A respeito dessas afirmativas, é correto afirmar que

A) a primeira proposição está incorreta e a segunda, correta.  
B) a primeira proposição está correta e a segunda, incorreta.  
C) as duas proposições estão incorretas.  
D) as duas proposições são corretas e a segunda é uma justificativa correta da primeira.  
E) as duas proposições são corretas, mas a segunda não é uma justificativa correta da primeira.  

**23.** (FCC · DPE-RS/2017 · Analista de Segurança da Informação)  
No sistema operacional Linux, quando um usuário é criado, uma linha com suas informações é adicionada no arquivo /etc/passwd. Considere que há a seguinte linha neste arquivo:

`dprs1:x:1001:1234:analista de SI matricula 1234:/home/dprs1:/bin/bash`

O campo

A) dprs1 indica o nome de login do usuário, geralmente limitado a 64 caracteres, não sendo case-senstitive.  
B) x indica que a senha está armazenada e protegida no arquivo /etc/shadow. Se houver um asterisco (*) no lugar, significa que a conta está desativada.  
C) 1001 indica o Group IDentification − GID do usuário, isto é, o número de identificação do grupo do qual ele faz parte. Geralmente o usuário 0 é indicado para o grupo do usuário root.  
D) 1234 indica o User IDentification − UID do usuário. O sistema também pode criar automaticamente usuários para a execução de certas rotinas e atribuir a eles UIDs baixos, como 1, 2, 3 e assim por diante.  
E) /home/dprs1:/bin/bash indica a pasta para uso exclusivo do usuário.  

**24.** (FCC · TRT-9/2015 · Analista — TI)  
No sistema operacional Linux, o diretório /etc é utilizado, por padrão, para armazenar as configurações do sistema. Nesse diretório existe o arquivo /etc/shadow que é utilizado para

A) armazenar as senhas criptografadas dos usuários.  
B) configurar os paths para os sistemas de arquivos em rede.  
C) armazenar os links para as pastas de backup do sistema.  
D) listar os arquivos de cópia de configuração do sistema.  
E) armazenar os nomes DNS equivalentes do sistema.  

**25.** (FCC · TRT-11/2017 · Técnico — TI)  
O Técnico Administrador do servidor com sistema operacional Linux Red Hat 6 deseja configurar o sistema para que, no momento do boot, ocorra a montagem do diretório `/usr/local/app` localizado no `servidor0` ao diretório `/app` do servidor local. Considerando o uso do NFS, a correta sintaxe da linha a ser acrescentada no arquivo `/etc/fstab` é:

A) `/app servidor0:/usr/local/app nfs rsize=8192,wsize=8192,timeo=14,intr`  
B) `servidor0:/usr/local/app /app nfs rsize=8192,wsize=8192,timeo=14,intr`  
C) `/app nfs servidor0:/usr/local/app rsize=8192,wsize=8192,timeo=14,intr`  
D) `servidor0: nfs /usr/local/app /app rsize=8192,wsize=8192,timeo=14,intr`  
E) `/usr/local/app:servidor0 nfs /app rsize=8192,wsize=8192,timeo=14,intr`  

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **E** | chmod 740 |
| 17 | **C** | sem bit x |
| 18 | **A** | dnf + systemctl + grep -i |
| 19 | **C** | política SELinux ≠ firewall/DAC |
| 20 | **D** | SELINUX=enforcing em /etc/selinux/config |
| 21 | **E** | firewall-cmd --permanent + --reload |
| 22 | **C** | systemd; reload ≠ restart |
| 23 | **B** | x → /etc/shadow |
| 24 | **A** | shadow = hashes |
| 25 | **B** | origem NFS antes do mount point |

*Arquivo 03 · Linux Red Hat / Oracle Linux · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 04 Windows 10/11 + Server.*

+++
title = '06 — PowerShell + WSUS'
date = '2026-10-29'
draft = false
weight = 6
tags = ['semana-09', 'powershell']
categories = ['estudo']
+++
# PowerShell + WSUS · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — PowerShell (cmdlets, pipeline, remoting) + WSUS (patch management)  
**Escopo deste arquivo:** sintaxe Verb-Noun; pipeline e objetos; cmdlets cobráveis; ExecutionPolicy; remoting (WinRM, Enter-PSSession, Invoke-Command); WSUS (papel, modos, aprovações, sincronização, portas, GPO cliente, cmdlets UpdateServices).  
**Fora de escopo aqui:** Active Directory profundo (arquivo 05); Bash/Linux; Intune/WUfB em profundidade (só menção contextual); desenvolvimento C#/.NET.

---

## 1. Material

### 1.1 Por que a FCC cobra este bloco

No Windows Server do edital TRT4, **PowerShell** é a linguagem de automação cobrada com **cmdlet certo**, **pipeline** e **remoting**. **WSUS** fecha o ciclo de **patch management** corporativo: sincronizar da Microsoft, aprovar, segmentar por grupos, apontar clientes via GPO. Provas recentes (TRT-19/2022, TRT-20/2024, TRT-1/2025) misturam WSUS com pipeline (`Get-WsusClassification | Where-Object | Set-WsusClassification`).

Cartão mental:

| Tema | Flash |
|------|-------|
| Cmdlet | `Verb-Noun` (`Get-Service`, `Set-ExecutionPolicy`) |
| Pipeline `|` | Passa **objetos**, não só texto |
| Remoting | WinRM; `Enable-PSRemoting`; `Enter-PSSession` × `Invoke-Command` |
| WSUS | Intranet update; aprovar; grupos; sync Microsoft Update / pai |
| Cliente | WUA + GPO “intranet Microsoft update service location” |

### 1.2 PowerShell: modelo mental

PowerShell = shell + linguagem orientada a **objetos .NET**. Saída de cmdlet é objeto (com propriedades/métodos), não apenas string do `cmd.exe`. Por isso `Get-Process | Where-Object { $_.CPU -gt 100 }` filtra por propriedade.

Padrão de nomes: **Verb-Noun** aprovados (`Get`, `Set`, `New`, `Remove`, `Start`, `Stop`, `Invoke`, `Enter`…). Ajuda descoberta:

| Cmdlet | Função |
|--------|--------|
| `Get-Command` | Lista comandos / filtra por nome ou parâmetro |
| `Get-Help` | Documentação (`Update-Help` atualiza) |
| `Get-Member` | Propriedades/métodos do objeto no pipeline |

Aliases existem (`gci` = `Get-ChildItem`), mas a FCC prefere o **nome canônico** do cmdlet.

### 1.3 Pipeline e filtragem

Operador `|` envia a saída do comando à esquerda como entrada do da direita.

| Cmdlet | Papel no pipe |
|--------|----------------|
| `Where-Object` (`?`) | Filtra (`-FilterScript { $_.Status -eq 'Running' }`) |
| `ForEach-Object` (`%`) | Age em cada objeto (`{ $_.Name }`) |
| `Select-Object` | Projetar propriedades / `-First` / `-Last` |
| `Sort-Object` | Ordenar |
| `Measure-Object` | Contar/medir |

`$_` (ou `$PSItem`) = objeto atual no scriptblock.

Armadilha FCC: usar `||` (lógica de outro shell) no lugar de `|`; misturar `Get-Process` com `Stop-Service` (tipos incompatíveis); parâmetros com `--Name` estilo Linux em vez de `-Name`.

Exemplo cobrável (WSUS + pipe):

```powershell
Get-WsusClassification |
  Where-Object -FilterScript { $_.Classification.Title -eq "Drivers" } |
  Set-WsusClassification -Disable
```

Efeito: **desabilita sincronização futura** da classificação Drivers; **não** apaga pacotes já baixados; **não** exclui permanentemente a categoria; **não** afeta outras classificações.

### 1.4 Cmdlets de administração frequentes

| Área | Exemplos |
|------|----------|
| Processos | `Get-Process`, `Stop-Process`, `Start-Process` |
| Serviços | `Get-Service`, `Start-Service`, `Stop-Service` |
| Arquivos | `Get-ChildItem`, `Get-Content`, `Set-Content`, `New-Item -ItemType` |
| Rede | `Test-Connection` (ping orientado a objeto; `-ComputerName` / `-TargetName`) |
| Elevação | `Start-Process Powershell -Verb RunAs` (UAC / admin) |

`Test-Connection -ComputerName "Server01","Server02"` — clássico SEGEP/FCC para “ping” em PowerShell (não inventar `Get-Service -PingConnection`).

`New-Item` usa **`-ItemType`**, não `-TypeItem`. `Get-Content` **lê**; alterar = `Set-Content` / `Add-Content`.

### 1.5 Execution Policy

Política de execução controla se scripts (`.ps1`) e perfis podem rodar. Em **clientes Windows**, padrão histórico citado em prova: **Restricted** (não executa scripts).

| Política | Ideia |
|----------|-------|
| **Restricted** | Sem scripts |
| **RemoteSigned** | Locais ok; baixados da Internet precisam assinatura |
| **AllSigned** | Tudo assinado |
| **Unrestricted** / **Bypass** | Permissivos (cuidado) |

Comandos cobrados (TRT-23/2022):

```powershell
Get-ExecutionPolicy
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

Distratores: `Show-ExecutionPolicy`, `Define-ExecutionPolicy`, `Get-CurrentExecutionPolicy`, `set +ExecutionPolicy`, espaços quebrando Verb-Noun.

Escopo: `LocalMachine` (padrão, precisa admin), `CurrentUser`, `Process`. GPO pode sobrescrever.

### 1.6 Remoting (básico cobrável)

Base: **WinRM** (WS-Management). Em Server, remoting costuma vir habilitado; em cliente, habilitar:

```powershell
Enable-PSRemoting
```

(faz quick config: serviço WinRM, listener, firewall).

| Cenário | Cmdlet |
|---------|--------|
| Sessão **interativa** 1:1 | `Enter-PSSession -ComputerName srv` → sai com `Exit-PSSession` |
| Comando em **um ou muitos** hosts | `Invoke-Command -ComputerName a,b -ScriptBlock { ... }` |
| Sessão persistente | `New-PSSession` + `-Session` no `Invoke-Command` |

FCC DPE-RS/2017: cmdlet para sessão remota = **`Enter-PSSession`** (não `remote-ps`, `ps-remote`, `start-remote`, `remote-session`).

Objetos retornados por remoting são tipicamente **desserializados** (snapshot; métodos “vivos” limitados) — detalhe avançado raro, mas útil para não achar que `Stop()` local funciona no objeto remoto.

### 1.7 WSUS — o que é e para que serve

**Windows Server Update Services**: função de servidor que sincroniza metadados/pacotes do **Microsoft Update** (ou de um WSUS pai) e permite **aprovar** atualizações para grupos de computadores na intranet. Benefícios: economia de banda, controle de quais patches entram, relatórios de conformidade.

Cliente: **Windows Update Agent (WUA)** aponta para o WSUS (URL), em geral via **GPO**.

Papel: adiciona/remove pelo **Server Manager** / `Install-WindowsFeature` (UpdateServices) — não “comando include do gerenciador de arquivos”.

Integridade: documentação/provas citam uso de **hash SHA-256** nos pacotes (TRT-19/2022).

### 1.8 Modos de implantação

| Modo | Ideia FCC |
|------|-----------|
| **Autônomo (Standalone)** / administração **distribuída** | **Padrão de instalação**: cada admin do servidor escolhe idiomas, cria grupos, aprova e verifica instalação nos grupos (TRT-20/2024) |
| **Réplica / administração centralizada** | Servidor filho herda aprovações do pai |

Hierarquia: um WSUS pode ser **fonte** de outros (pai → filhos). Sync entre WSUS na prática moderna usa com frequência **8530** (HTTP) / **8531** (HTTPS) no próprio serviço; comunicação com Microsoft Update na Internet tipicamente **80/443** (HTTP/HTTPS) — a FCC já cobrou **inversão** 80↔HTTPS / 443↔HTTP como distrator.

### 1.9 Produtos, classificações e aprovações

Na sync, escolhe-se **produtos** (Windows Server, Office…) e **classificações** (Critical, Security, Updates, Drivers, Service Packs…).

Ações de aprovação típicas:

| Ação | Efeito |
|------|--------|
| **Install** | Aprova instalação (pode ter deadline) |
| **Remove** | Desinstalação se o update permitir |
| **Detect only / Somente detectar** | Cliente **avalia necessidade** e reporta; **não** instala |
| **Decline** | Recusa |

Grupos de computadores: aprovar para “All Computers”, “Unassigned” ou grupos custom (ex.: Pilot / Produção). GPO + targeting lado cliente ou server-side targeting.

Download diferido: baixar metadados primeiro; baixar binários ao aprovar Install — economiza disco/banda.

### 1.10 Cmdlets UpdateServices (amostra cobrável)

| Cmdlet | Uso |
|--------|-----|
| `Get-WsusServer` | Instância WSUS |
| `Get-WsusClassification` / `Set-WsusClassification` | Ligar/desligar classificação (`-Disable` / enable) |
| `Get-WsusProduct` / `Set-WsusProduct` | Produtos sincronizados |
| `Get-WsusUpdate` / `Approve-WsusUpdate` | Consulta/aprovação |
| `Invoke-WsusServerCleanup` | Limpeza |

Distrator TRT-20/2024: “não há cmdlets PowerShell para WSUS” → **falso**. Outro: exigir “.NET Framework 10.0” → versão inventada. UUP local: não é “adicionar arquivos `.mime` e `.win`”.

### 1.11 Cliente e GPO

Configurações clássicas de Automatic Updates / intranet:

- Especificar servidor WSUS (HTTP/HTTPS URL + porta se não padrão)  
- Frequência de detecção  
- Comportamento de download/instalação  
- Targeting de grupo (client-side)

Sem GPO/apontamento, o cliente segue Windows Update público (ou política de nuvem moderna) — fora do controle do WSUS.

### 1.12 Contexto 2024–2026 (margem, sem inventar prova)

Microsoft marca WSUS como **deprecated** para *novas* features, mas o papel **continua** em Windows Server (incl. 2025) e segue recebendo conteúdo no canal WSUS. Em prova FCC recente, cobre-se o **comportamento clássico** (standalone, classificações, PowerShell). Alternativas de mercado (WUfB/Intune) podem aparecer como contexto futuro, não como substituto automático do que a banca ainda cobra.

### 1.13 Anti-confusões

| Confunde | Com | Flash |
|----------|-----|-------|
| `\|` pipeline | `\|\|` | Pipe de objetos |
| `Get-Process` | `Get-Service` | Processo ≠ serviço |
| `Enter-PSSession` | `Invoke-Command` | Interativo × one-shot/muitos |
| `-Disable` classificação | Apagar updates do disco | Só para sync futura da categoria |
| Standalone | “não é o padrão” | É o padrão / admin distribuída |
| Porta 80 = HTTPS | Inversão | 80 HTTP / 443 HTTPS (Update); 8530/8531 no serviço WSUS |

### 1.14 Síntese cobrável

PowerShell = Verb-Noun + pipeline de objetos; `Get/Set-ExecutionPolicy`; remoting WinRM com `Enter-PSSession`/`Invoke-Command`; WSUS = patch intranet, standalone padrão, aprovações/grupos/sync, SHA-256, cmdlets `*-Wsus*`, cliente via GPO.

---

## 2. Questões de treino (1–15)

**1.** O padrão de nomenclatura típico de cmdlets PowerShell é:

A) noun.verb  
B) Verb-Noun  
C) snake_case obrigatório  
D) apenas aliases de 1 letra  
E) XML-only  

**2.** O operador `|` no PowerShell:

A) Concatena strings SQL  
B) Encadeia comandos passando **objetos** pelo pipeline  
C) É idêntico a `||` do Bash em todos os casos  
D) Desliga o WinRM  
E) Remove o módulo UpdateServices  

**3.** Para filtrar objetos no pipeline usa-se tipicamente:

A) `Where-Object`  
B) `Format-Hex` apenas  
C) `Shutdown /r`  
D) `nslookup`  
E) `chkdsk`  

**4.** Sessão remota **interativa** 1:1 no PowerShell clássico obtém-se com:

A) `remote-ps`  
B) `ps-remote`  
C) `Enter-PSSession`  
D) `start-remote`  
E) `remote-session`  

**5.** Em clientes Windows, a política de execução que **impede** scripts, citada como padrão em prova FCC, é:

A) Bypass  
B) Unrestricted  
C) Restricted  
D) RemoteSigned apenas em Linux  
E) AllSigned obrigatória em todo Server sem exceção documental  

**6.** Comandos corretos para (I) ver e (II) definir ExecutionPolicy RemoteSigned:

A) `Show-ExecutionPolicy` e `Define-ExecutionPolicy RemoteSigned`  
B) `Get-ExecutionPolicy` e `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`  
C) `Get-CurrentExecutionPolicy` e `Set-CurrentExecutionPolicy`  
D) `set +ExecutionPolicy RemoteSign`  
E) `ping ExecutionPolicy`  

**7.** WSUS serve principalmente para:

A) Substituir o Active Directory  
B) Gerenciar/distribuir atualizações Microsoft na intranet com aprovação administrativa  
C) Compilar kernels Linux  
D) Emitir NF-e  
E) Configurar STP  

**8.** O modo de instalação **padrão** do WSUS, segundo a linha cobrada pela FCC (TRT-20), é:

A) Sempre somente réplica sem aprovações locais  
B) Autônomo / administração distribuída  
C) Exclusivamente nuvem Intune sem servidor  
D) Apenas Detect-only permanente  
E) Sem grupos de computadores possíveis  

**9.** `Set-WsusClassification -Disable` após filtrar "Drivers":

A) Apaga do disco todos os drivers já baixados  
B) Remove permanentemente a categoria do produto  
C) Desabilita sync/distribuição **futura** de Drivers sem afetar outras classificações  
D) Ativa Drivers automaticamente  
E) Formata o volume SYSVOL  

**10.** `Test-Connection -ComputerName "Server01","Server02"` equivale funcionalmente, na ideia de prova, a:

A) Enviar pings/testes de eco ICMP a esses hosts via PowerShell  
B) Parar o serviço Spooler  
C) Criar OU no AD  
D) Assinar script AllSigned  
E) Desabilitar WinRM  

**11.** `Start-Process Powershell -Verb RunAs` serve para:

A) Listar serviços parados  
B) Abrir PowerShell **elevado** (admin), com UAC  
C) Remover o WSUS  
D) Converter NTFS em FAT  
E) Desligar o KDC  

**12.** Assinale a afirmativa **incorreta**:

A) Existe módulo/cmdlets PowerShell para operações WSUS  
B) Pipeline usa `|`  
C) `Enter-PSSession` é sessão interativa  
D) Não existem cmdlets PowerShell para WSUS, por segurança  
E) `Get-Help` documenta comandos  

**13.** Relação correta `Invoke-Command` × `Enter-PSSession`:

A) São aliases idênticos sem diferença  
B) `Invoke-Command` executa scriptblocks (inclusive em vários hosts); `Enter-PSSession` é shell interativo remoto  
C) Ambos só funcionam sem WinRM  
D) `Enter-PSSession` só lista impressoras  
E) `Invoke-Command` cria florestas AD  

**14.** Qual opção é classificação típica de update no WSUS?

A) Drivers / Security / Critical  
B) Eureka / Zuul  
C) MEA / DSS  
D) POP / IMAP  
E) Scrum / XP  

**15.** Síntese correta:

A) Verb-Noun + pipeline; ExecutionPolicy; WinRM/`Enter-PSSession`/`Invoke-Command`; WSUS standalone, aprovações, `*-Wsus*`, cliente via GPO  
B) PowerShell só texto sem objetos  
C) WSUS não tem modo autônomo  
D) `-Disable` apaga a floresta  
E) Remoting não usa WinRM  

---

### Gabarito comentado (treino 1–15)

1. **B** — Verb-Noun.  
2. **B** — Pipeline de objetos.  
3. **A** — `Where-Object`.  
4. **C** — `Enter-PSSession`.  
5. **C** — Restricted.  
6. **B** — Get/Set-ExecutionPolicy.  
7. **B** — Patch intranet com aprovação.  
8. **B** — Standalone/distribuída.  
9. **C** — Desabilita Drivers futuros.  
10. **A** — Ping/Test-Connection.  
11. **B** — RunAs/admin.  
12. **D** — Incorreta: há cmdlets WSUS.  
13. **B** — Papéis distintos.  
14. **A** — Classificações WSUS.  
15. **A** — Síntese do arquivo.

---

## 3. Questões de concurso (16–25)

**16.** (FCC · TRT-1/2025 · Analista Judiciário — TI)  
Considere o seguinte comando PowerShell que será executado em um ambiente Microsoft Windows Server, com as devidas permissões, em condições ideais.

```powershell
Get-WsusClassification | Where-Object -FilterScript {$_.Classification.Title -Eq "Drivers"} |
Set-WsusClassification -Disable
```

Esse comando configura o WSUS (Windows Server Update Services) para

A) remover todas as atualizações de drivers já baixadas no WSUS, liberando espaço em disco e impedindo futuras sincronizações desses pacotes.  
B) aplicar automaticamente atualizações de drivers apenas para dispositivos que já possuem versões anteriores instaladas.  
C) ativar a sincronização de atualizações de drivers, garantindo que drivers de dispositivos sejam distribuídos para os clientes automaticamente.  
D) excluir permanentemente a categoria de atualizações de drivers do WSUS, impedindo qualquer possibilidade de ativação futura sem reinstalar o serviço.  
E) desabilitar as atualizações de drivers, de forma que novos drivers não sejam baixados e distribuídos pelo WSUS, mas sem afetar outras categorias de atualizações.  

**17.** (FCC · TRT-20/2024 · Analista Judiciário — TI)  
Uma equipe de Analistas está planejando a implantação do WSUS (Windows Server Update Services) no ambiente Windows Server 2019. De acordo com o fabricante (Microsoft),

A) o servidor do WSUS usa a porta 80 para o protocolo HTTPS e a porta 443 para o protocolo HTTP para obter atualizações da Microsoft. Essa é a configuração nativa de portas quando há um firewall corporativo entre a rede e a Internet, permitindo a comunicação direta com o Microsoft Update.  
B) o modo Autônomo, também chamado administração distribuída, é a opção de instalação padrão para o WSUS. Cada administrador do servidor do WSUS escolhe os idiomas das atualizações, cria grupos de computadores e verifica se as atualizações corretas estão instaladas nos grupos de computadores, dentre outras atividades.  
C) o Microsoft .NET Framework 10.0 precisa estar instalado no servidor em que o WSUS server master será instalado.  
D) não há cmdlets do Windows PowerShell disponíveis para operações do WSUS, para aumentar a segurança dos servidores.  
E) dois tipos de arquivos precisam ser adicionados aos servidores do WSUS para dar suporte à UUP (Unified Update Platform) local e fazer o gerenciamento de atualizações: os arquivos cujas extensões são .mime e .win.  

**18.** (FCC · DPE-RS/2017 · Analista — TI Infraestrutura)  
Deseja-se acessar uma sessão remota de PowerShell no Windows Server 2012 R2 de uma máquina em que o direito de gerenciamento está habilitado. Para isso, é necessária a execução do cmdlet

A) remote-ps.  
B) ps-remote.  
C) start-remote.  
D) remote-session.  
E) enter-pssession.  

**19.** (FCC · SEGEP-MA/2018 · Analista Executivo — Programador de Sistemas)  
Após executar o Windows PowerShell no prompt de comando do Windows, um Programador de Sistemas deseja enviar pings do computador local para dois computadores remotos identificados como Server01 e Server02. Para isso terá que utilizar o comando

A) `Test-Connection -ComputerName "Server01", "Server02"`  
B) `Get-Service -PingConnection "Server01", "Server02"`  
C) `Start-Call -ComputerName "Server01", "Server02"`  
D) `Get-Request ping 'Server01', ping 'Server02'`  
E) `Ping-Request -i 'Server01', 'Server02'`  

**20.** (FCC · TRE-SP/2017 · Analista Judiciário — Análise de Sistemas)  
Um Analista do TRE-SP utiliza um computador com Windows 7, em português, e deseja realizar alguns testes usando Powershell. Neste cenário,

A) é preciso criar scripts Powershell utilizando uma ferramenta que não vem, nativamente, instalada no Windows 7, então nenhum teste pode ser realizado nesta plataforma.  
B) para acionar o Powershell basta pressionar as teclas Windows e R e, na janela que se abre, digitar powershell.  
C) ao digitar o script `foreach($servidor in $listaServidores) { Get-WMIObject -ComputerName $servidor Win32_LogicalDisk }` não ocorrerá erro e o teste será realizado.  
D) ao digitar `get-process || foreach (write $_.Name)`, em que símbolo do pipeline permite encadear instruções, não ocorrerão erros e os testes serão realizados.  
E) após criar o script em um arquivo com a extensão .ps1, para que o Analista possa executá-lo em seu computador, ele deve digitar no terminal do Powershell: `set +ExecutionPolicy RemoteSign`.  

**21.** (FCC · TRT-20/2024 · Analista Judiciário — TI)  
Um Analista está utilizando uma máquina com o sistema operacional Windows 10 instalado e funcionando em condições ideais. Estando no prompt de comandos, com as devidas permissões, utilizou o comando

A) `Get-Process --Name Spooler | Stop-Service` para exibir informações sobre o Spooler e em seguida pausar o serviço de impressão.  
B) `New-Item -Path "C:\Backup" -TypeItem File` para criar um novo arquivo chamado Backup no diretório C:\.  
C) `Set-Process | Stop-Process` para exibir todos os processos em execução no sistema e finalizar todos eles.  
D) `Get-Content --Path C:\Users\trt20.txt` para alterar o conteúdo do arquivo trt20.txt no diretório C:\Users\.  
E) `Start-Process Powershell -verb runAs` para confirmar e acessar o PowerShell como Administrador.  

**22.** (FCC · TRT-23/2022 · Técnico Judiciário — TI)  
Os scripts do PowerShell não poderão ser executados quando a política de execução estiver definida como Restricted. De acordo com a Microsoft, essa é a configuração padrão em todos os sistemas operacionais do cliente Windows. Diante disso, um Técnico foi solicitado a:

I. Verificar a política de execução atual.  
II. Mudar a configuração para RemoteSigned.

Os comandos I e II que o Técnico deve utilizar no Powershell do Windows 10, em condições ideais, são correta e respectivamente:

A) Show-ExecutionPolicy e Define-ExecutionPolicy RemoteSigned  
B) get -executionpolicy e set -executionpolicy remotesigned  
C) Show ExecutionPolicy e Define ExecutionPolicy RemoteSigned  
D) Get-CurrentExecutionPolicy e Set-CurrentExecutionPolicy RemoteSigned  
E) Get-ExecutionPolicy e Set-ExecutionPolicy -ExecutionPolicy RemoteSigned  

**23.** (FCC · TRT-19/2022 · Analista Judiciário — TI)  
Utilizando o Windows Server Update Services, uma Analista observou, corretamente, que este consiste em uma função de servidor interna que

A) pode ser adicionado e removido usando o comando include do gerenciador de arquivos.  
B) contempla o recurso de hash SHA-256 para aumentar a segurança.  
C) usa servlets do Windows update para gerenciar suas tarefas de conexão mais importantes.  
D) permite a união concatenada entre cliente e servidor nas versões do WUA (Windows Update Agent) no Windows Active Directory.  
E) usa uplets do Windows update para gerenciar as tarefas de conexão mais importantes no LDAP.  

**24.** (FCC · TRT-6/2025 · Analista Judiciário — TI)  
Uma equipe de Analistas configurou o ambiente de rede do Tribunal Regional do Trabalho no Windows Server 2019, instalado e funcionando em condições ideais. Eles precisam configurar o Active Directory para centralizar a administração de usuários e computadores. Um dos administradores, com as devidas permissões, deseja criar uma nova Unidade Organizacional (OU) chamada "Judicial" dentro do Active Directory, para organizar os recursos de maneira estruturada. O comando PowerShell para isso é:

A) `New-ADComputer -Name "Judicial" -Path "OU=Judicial,DC=tribunal,DC=jus"`  
B) `Get-ADOU -Identity "OU=Judicial,DC=tribunal,DC=jus"`  
C) `New-ADGroup -Name "Judicial" -GroupScope Global -Path "OU=Users,DC=tribunal,DC=jus"`  
D) `New-ADOrganizationalUnit -Name "Judicial" -Path "DC=tribunal,DC=jus"`  
E) `Set-ADUser -Identity "Judicial" -OrganizationalUnit "OU=Judicial,DC=tribunal,DC=jus"`  

**25.** (CESPE/CEBRASPE · TCE-PR/2024 · Auditor de Controle Externo — Informática)  
Julgue os próximos itens, a respeito do WSUS (Windows Server Update Services), que é a ferramenta de gerenciamento de atualizações desenvolvida pela Microsoft para ambiente Windows.

I. Um servidor do WSUS pode ser a fonte de atualização de outros servidores do WSUS existentes em uma organização.  
II. É possível aprovar a instalação de atualizações para todos os computadores na rede do WSUS ou para grupos de computadores diferentes.  
III. O WSUS destina-se a instalar atualizações, não sendo possível a remoção de uma atualização que já esteja instalada.

Assinale a opção correta.

A) Apenas o item I está certo.  
B) Apenas o item III está certo.  
C) Apenas os itens I e II estão certos.  
D) Apenas os itens II e III estão certos.  
E) Todos os itens estão certos.  

---

### Gabarito comentado (concurso 16–25)

16. **E** — `-Disable` na classificação Drivers: sem sync futura dessa categoria; demais intactas.  
17. **B** — Standalone / administração distribuída é o padrão.  
18. **E** — `Enter-PSSession`.  
19. **A** — `Test-Connection` com os dois hosts.  
20. **B** — Win+R → `powershell` (nativo no Windows 7).  
21. **E** — `Start-Process … -Verb RunAs`.  
22. **E** — `Get-ExecutionPolicy` e `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`.  
23. **B** — Hash SHA-256 no WSUS.  
24. **D** — `New-ADOrganizationalUnit` (cmdlet PowerShell AD; cruzamento útil com semana AD).  
25. **C** — I e II corretas; III falsa (há aprovação Remove/uninstall quando o update suporta).

---

**Caderno de erros (sugerido):** `|`×`||` · Get-Process×Get-Service · Enter-PSSession×Invoke-Command · Restricted/RemoteSigned · Standalone WSUS · `-Disable`≠apagar disco · portas 80/443 invertidas · SHA-256 · pai/filho WSUS · RunAs.

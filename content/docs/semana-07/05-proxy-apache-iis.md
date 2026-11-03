+++
title = '05 — Proxy, Apache HTTP Server e IIS'
date = '2026-10-19'
draft = false
weight = 5
tags = ['semana-07', 'proxy', 'apache']
categories = ['estudo']
+++
# Proxy, Apache HTTP Server e IIS · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — Web Proxy · Apache HTTP Server · IIS  
**Escopo deste arquivo:** conceitos cobráveis de **proxy** (forward × reverse, cache, autenticação), **Apache HTTP Server** (httpd.conf, VirtualHost, DocumentRoot, seções Directory/Location/Files/Proxy, mod_proxy/ProxyPass/ProxyPassReverse, SSL/logs) e **IIS** (papéis, applicationHost.config, sites/app pools, contraste com Apache).  
**Fora de escopo aqui:** JBoss/WildFly (arquivo 06); SNMP/QoS (07); Zabbix/Grafana/Prometheus/ELK (08); TCP/IP/DNS/HTTP profundos dos arquivos 01–04; segurança OWASP/WAF a fundo (semana 8).

---

## 1. Material

### 1.1 Por que a FCC cobra este bloco

No edital (código 14): **Web Proxy; Apache HTTP Server e IIS** — desempenho, usuários, configuração, administração e logs. A banca mistura:

1. Papel do **proxy** (quem inicia a requisição? o cliente “vê” o backend?).  
2. Arquivos e diretivas **Apache** (httpd.conf, DocumentRoot, ProxyPass).  
3. Arquivos e papéis do **IIS** (applicationHost.config, Application Pool).  
4. Comandos **falsos** misturados a nomes verdadeiros (clássico TRT-20/2024).

Cartão mental:

| Peça | Frase de prova |
|------|----------------|
| Forward proxy | Cliente **configurado** usa o proxy para sair à Internet |
| Reverse proxy | Cliente fala com o proxy **como se fosse** o site; backend oculto |
| Apache | Servidor HTTP; `httpd.conf`; `mod_proxy` |
| IIS | Servidor web **Microsoft**; `applicationHost.config` |

### 1.2 Proxy — definição e papéis

**Proxy** = intermediário entre cliente e servidor de origem. Em prova, o ponto não é “máquina na DMZ”; é **quem configura o quê** e **quem fica visível**.

| Tipo | Cliente | Backend | Uso típico de banca |
|------|---------|---------|---------------------|
| **Forward (direto)** | Precisa apontar o proxy (browser/sistema) | Origem externa qualquer | Saída controlada, filtro, cache, anonimato relativo |
| **Reverse (reverso / gateway)** | **Não** precisa saber do backend | App server / farm interna | Publicar app, TLS offload, balanceamento, unificar URL |

Flash anti-confusão:

- Forward → “funcionários saem pela Internet **via** proxy”.  
- Reverse → “Internet chega no Apache/Nginx/IIS e ele **encaminha** ao WildFly/IIS interno”.

**Proxy aberto (open proxy)** sem controle de quem pode usá-lo é risco clássico (abuso, amplificação). Em Apache, ativar forward sem restringir cliente é armadilha de segurança.

### 1.3 Cache, autenticação e filtragem (visão cobrável)

Proxies frequentemente agregam:

- **Cache** — reduzir banda/latência de objetos repetidos.  
- **Autenticação / ACL** — só redes/usuários autorizados usam o forward.  
- **Filtragem** — bloquear URLs/categorias (mais típico no forward corporativo).  
- **Inspeção TLS** (quando cobrada) — quebra TLS no proxy; exige confiança no certificado do proxy (tema de segurança, semana 8).

Não confundir proxy com:

| Conceito | Diferença |
|----------|-----------|
| NAT | Traduz endereço; não é “HTTP intermediário” de aplicação |
| Firewall de pacote | Filtra por IP/porta; proxy opera em nível de aplicação (HTTP etc.) |
| VPN | Túnel criptografado ponta a ponta; proxy é intermediário de aplicação |
| Load balancer L4 | Distribui conexões; reverse proxy HTTP costuma operar em L7 (URL, host, header) |

### 1.4 Apache HTTP Server — âncoras

**Apache HTTP Server** (httpd) = servidor web multiplataforma, configuração baseada em **arquivos de texto** e **módulos**.

Arquivo principal cobrado: **`httpd.conf`** (em distros Debian/Ubuntu a lógica se espalha em `apache2.conf` + `sites-enabled` — a FCC em TRT-1/2025 associou Apache ↔ **httpd.conf**).

Diretivas de ouro:

| Diretiva | Papel |
|----------|-------|
| `Listen` | Porta/interface em que o httpd escuta |
| `ServerName` | Nome canônico do host/virtual host |
| `DocumentRoot` | Diretório raiz dos arquivos do site |
| `DirectoryIndex` | Arquivo padrão (index.html etc.) |
| `VirtualHost` | Hospedagem de vários sites no mesmo Apache |
| `ErrorLog` / `CustomLog` | Logs de erro e acesso |

**DocumentRoot** se altera **editando o arquivo de configuração** (ou include de VirtualHost), **não** com `apachectl DocumentRoot '...'` (comando inventado — TRT-20/2024).

`apachectl` / `apache2ctl` = controlar o daemon (start/stop/graceful, **`configtest` / `-t`** para validar sintaxe). Não “gera certificado SSL” sozinho via `apachectl enable mod_ssl --generate-ssl-cert` (também inventado na mesma prova).

### 1.5 Seções de configuração Apache (Directory × Location × Files × Proxy)

A FCC (TJ-PE/2012) cobrou o significado das seções. Memorize o eixo **filesystem × webspace**:

| Seção | Escopo |
|-------|--------|
| `<Directory>` | Caminho no **sistema de arquivos** (+ subdirs) |
| `<DirectoryMatch>` | Idem, com **expressão regular** |
| `<Files>` / `<FilesMatch>` | Por **nome de arquivo** |
| `<Location>` / `<LocationMatch>` | Por **caminho de URL** (webspace), não “localização física do servidor” |
| `<Proxy>` / `<ProxyMatch>` | Conteúdo acessado **via proxy** (mod_proxy) |

Armadilha: dizer que `<Location>` “configura a localização corrente do servidor” — formulação **incorreta** (TJ-PE/2012). Location aplica diretivas a um **prefixo de URL**.

### 1.6 mod_proxy: ProxyPass e ProxyPassReverse

Para **proxy reverso** no Apache:

```apache
ProxyPass        "/api" "http://10.20.0.15:8080/api"
ProxyPassReverse "/api" "http://10.20.0.15:8080/api"
```

| Diretiva | Função cobrável |
|----------|-----------------|
| **ProxyPass** | Mapeia caminho local → URL do backend (reverse proxy) |
| **ProxyPassReverse** | Reescreve cabeçalhos de redirecionamento (`Location` etc.) do backend para o namespace público |
| **ProxyRequests On** | Ativa **forward** proxy — **não** é necessário para reverse |

Frase de documentação (e de prova): ProxyPass mapeia servidores remotos no **espaço de endereços/URL do servidor local**; o Apache **parece** o originário.

Boas práticas cobráveis:

- Em reverse, mantenha `ProxyRequests Off`.  
- Restrinja quem pode usar o proxy (`Require`, `<Proxy>`).  
- Combine com VirtualHost + TLS no front.

### 1.7 Virtual hosts, módulos e logs

**Name-based VirtualHost** — vários sites no mesmo IP/porta, distinguidos por `Host` / `ServerName`.  
**IP-based** — IP (ou porta) distintos.

Módulos relevantes em prova:

| Módulo | Uso |
|--------|-----|
| `mod_ssl` | HTTPS/TLS |
| `mod_proxy` (+ `mod_proxy_http`) | Proxy |
| `mod_rewrite` | Reescrita de URL (flag `[P]` também pode proxyar) |
| `mod_status` / `mod_info` | Diagnóstico (cuidado em produção) |

Logs: **access** (quem acessou o quê) × **error** (falhas). Em tribunais, logs alimentam SIEM/ELK (arquivo 08).

### 1.8 IIS — Internet Information Services

**IIS** = servidor web da plataforma **Windows Server**. Em prova, contraste com Apache:

| Aspecto | Apache | IIS |
|---------|--------|-----|
| SO típico | Linux/Unix (também Windows) | Windows |
| Config principal (FCC TRT-1/2025) | `httpd.conf` | **`applicationHost.config`** |
| Unidade de app | VirtualHost / Directory | **Site** + **Application** + **Application Pool** |
| Administração | Arquivos + `apachectl` | IIS Manager, PowerShell (`Get-IISSite` etc.) |

**Application Pool** isola o processo da aplicação (identidade, reciclagem, pipeline). Reciclagem e identidade do pool são temas cobráveis de administração/desempenho.

Arquivo **`applicationHost.config`** = configuração em nível de servidor (sites, pools, módulos). Sites individuais também usam `web.config` (escopo da aplicação) — não confundir com `httpd.conf`.

Armadilhas TRT-20/2024:

- `Add-WindowsFeature Web-Server-PHP` **não** é o caminho padrão “oficial” simplificado que a banca apresentou como correto para “habilitar PHP no IIS”. PHP no IIS tipicamente envolve FastCGI/handler, não um feature inventado com esse nome.

### 1.9 Desempenho, usuários e administração (edital)

Pontos que o edital cita e a FCC traduz em cenários:

1. **Desempenho** — keep-alive, compressão, cache no reverse, número de workers/threads, reciclagem de app pool.  
2. **Usuários** — autenticação no proxy/site (Basic, Windows Auth no IIS, integração AD).  
3. **Configuração** — editar arquivos certos; validar (`nginx -t` / `apachectl configtest`); reload gracioso.  
4. **Logs** — rotação, retenção, correlação com monitoramento.

Padrão moderno em tribunais: **Apache ou IIS na borda (reverse)** + **JBoss/WildFly** (ou outro app server) atrás — tema do arquivo 06.

### 1.10 Nginx no papel de reverse (apareceu na FCC)

Embora o edital cite Apache/IIS, a FCC (TJ-CE/2022) cobrou **Nginx** como reverse na frente do Apache:

1. Editar vhost em `sites-available`.  
2. Symlink em `sites-enabled`.  
3. **`sudo nginx -t`** — testar configuração.  
4. **`sudo systemctl reload nginx`** — aplicar sem derrubar conexões (idealmente).

Não invente `nginx ProxyPass` — ProxyPass é dialeto **Apache**.

### 1.11 Tabela síntese anti-confusão

| Afirmação | Verdadeiro? |
|-----------|-------------|
| Reverse proxy exige configurar o browser do usuário | Não |
| Forward proxy exige apontar o cliente ao proxy | Sim (em geral) |
| `ProxyPass` = reverse | Sim |
| `ProxyRequests On` = forward | Sim |
| DocumentRoot muda com `apachectl DocumentRoot` | Não |
| IIS ↔ `applicationHost.config` | Sim (TRT-1/2025) |
| Apache ↔ `httpd.conf` | Sim (mesma prova) |
| `<Location>` = filesystem path | Não (é URL-path) |

### 1.12 Mapa rápido para a prova

```
Internet / Intranet
        │
   [Reverse Proxy]
   Apache | Nginx | IIS
        │
   [App Server / Conteúdo]
   WildFly | estático | API
```

Forward corporativo:

```
Cliente ──► Forward Proxy ──► Internet
```

---

## 2. Questões de treino (1–15)

**1.** Em um **proxy reverso**, a característica cobrável é:

A) O browser de cada usuário precisa apontar manualmente o IP do WildFly
B) O cliente enxerga o proxy como o servidor de origem; o backend fica oculto
C) O proxy só funciona se `ProxyRequests On` estiver ativo no Apache
D) É sinônimo rigoroso de NAT de porta
E) Impede qualquer uso de TLS na borda

**2.** Já no **proxy forward**, tipicamente:

A) O cliente **não** precisa de configuração especial  
B) O Apache usa apenas `ProxyPass` sem restrição de clientes  
C) O cliente é configurado para enviar pedidos ao proxy, que busca o conteúdo na origem  
D) O backend interno publica a URL pública diretamente na Internet  
E) Substitui obrigatoriamente o DNS  

**3.** A diretiva Apache que mapeia um caminho local a um backend remoto (reverse) é:

A) `DocumentRoot`  
B) `ServerName`  
C) `ProxyPass`  
D) `Listen`  
E) `Add-WindowsFeature`  

**4.** `ProxyPassReverse` existe principalmente para:

A) Ativar open proxy na Internet  
B) Gerar certificado SSL automaticamente  
C) Reescrever cabeçalhos de redirecionamento do backend para o namespace público  
D) Substituir o `httpd.conf` pelo `applicationHost.config`  
E) Desligar o módulo `mod_ssl`  

**5.** Sobre `ProxyRequests` no Apache:

A) Deve estar On para todo reverse proxy bem configurado
B) É o nome do arquivo de log do IIS
C) Equivale a `DocumentRoot`
D) Só existe no IIS
E) Ativa o modo **forward**; em reverse costuma ficar Off

**6.** O arquivo de configuração principal associado ao **Apache HTTP Server** na correlação FCC TRT-1/2025 é:

A) `applicationHost.config`  
B) `standalone.xml`  
C) `domain.xml`  
D) `httpd.conf`  
E) `web.config` exclusivo do WildFly  

**7.** O arquivo associado ao **IIS** na mesma correlação é:

A) `httpd.conf`
B) `applicationHost.config`
C) `standalone.xml`
D) `jboss-cli.sh`
E) `nginx.conf`

**8.** Alterar o DocumentRoot do site no Apache, em condições ideais de prova, implica:

A) `sudo apachectl DocumentRoot '/var/www/new_trtsite'`
B) `Add-WindowsFeature Web-Server-PHP`
C) `jboss-cli.sh --connect --command=shutdown`
D) Editar a diretiva no arquivo de configuração (ex.: httpd.conf / vhost) e recarregar o serviço
E) Criar apenas um arquivo vazio em `/etc/nginx/sites-enabled`

**9.** Em relação às seções do Apache, `<Directory>` aplica-se a:

A) Caminho no sistema de arquivos  
B) Apenas cabeçalhos SNMP  
C) Exclusivamente Application Pools do IIS  
D) OIDs da MIB  
E) Scripts em `bin/` do WildFly  

**10.** A afirmação **incorreta** sobre `<Location>` (linha TJ-PE/2012) é tratá-la como se:

A) Operasse sobre caminho de URL (webspace)  
B) Pudesse restringir acesso a um prefixo de URL  
C) “Configurasse a localização corrente do servidor” (fórmula imprecisa/errada da banca)  
D) Tivesse variante `LocationMatch` com regex  
E) Pudesse conter `ProxyPass` em cenários de reverse  

**11.** No IIS, o **Application Pool** serve sobretudo para:

A) Substituir o DNS corporativo
B) Ser o único arquivo de log do Apache
C) Definir OIDs SNMPv3
D) Compilar código C no kernel
E) Isolar o processo/identidade de execução das aplicações do site

**12.** Um Analista valida a configuração do Nginx e, sem erros, aplica as mudanças. Os comandos corretos e respectivos são:

A) `sudo nginx ProxyPass /blog http://blog.domain.com` e `sudo system reload nginx`  
B) `sudo nginx -t` e `sudo systemctl reload nginx`  
C) `sudo apachectl DocumentRoot '/var/www'` e `sudo nginx test`  
D) `jboss-cli.sh --connect` e `sudo nginx reloading`  
E) `Add-WindowsFeature Web-Server` e `sudo nginx proxy http ssl`  

**13.** Qual combinação descreve melhor o uso típico de Apache na frente de um app server Java em tribunal?

A) Apache/Nginx/IIS como **reverse**, encaminhando ao JBoss/WildFly
B) Apache como forward obrigatório configurado em todo browser da Internet pública
C) IIS apenas como cliente SNMP
D) Apache armazenando MIBs no DocumentRoot
E) ProxyPass desnecessário se houver apenas HTTP

**14.** “Open proxy” sem restrição de clientes é perigoso porque:

A) Impede logs de acesso
B) Obriga o uso exclusivo de IPv6
C) Desativa automaticamente o TLS 1.3
D) Permite que terceiros abusem do servidor para navegar/atacar escondidos atrás do seu IP
E) Converte DocumentRoot em Application Pool

**15.** Distinguir Apache de IIS na FCC:

A) Ambos usam obrigatoriamente só `standalone.xml`
B) IIS não possui sites nem pools
C) Apache não gera logs
D) IIS é o nome do módulo `mod_proxy`
E) Apache ↔ arquivos texto/`httpd.conf`; IIS ↔ plataforma Windows/`applicationHost.config` e app pools


---

### Gabarito comentado (treino 1–15)

1. **B** — Cliente vê o proxy como origem.  
2. **C** — Cliente aponta ao forward; proxy busca a origem.  
3. **C** — ProxyPass.  
4. **C** — Reescrita de Location etc. do backend.  
5. **E** — Forward On; reverse tipicamente Off.  
6. **D** — httpd.conf (TRT-1/2025).  
7. **B** — applicationHost.config.  
8. **D** — Editar config + reload (não apachectl DocumentRoot).  
9. **A** — Filesystem.  
10. **C** — Formulção cobrada como incorreta.  
11. **E** — Isolamento de processo/identidade.  
12. **B** — nginx -t + systemctl reload (TJ-CE/2022).  
13. **A** — Reverse na borda → app server.  
14. **D** — Abuso / open proxy.  
15. **E** — Contraste de plataforma e arquivos.

---

## 3. Questões estilo concurso FCC (16–25)

> Questões de provas FCC (e itens no padrão da banca). Gabarito só ao final.

**16.** (FCC – TRT-20/2024 – Analista Judiciário – TI)  
Uma equipe de TI está conversando sobre os comandos adequados para a configuração de servidores para hospedar diversas aplicações de um Tribunal Regional do Trabalho. Uma Analista esclareceu que, em condições ideais e com as devidas permissões, no servidor de aplicação

A) IIS, o comando para habilitar o suporte ao PHP, configurando o IIS para rodar aplicações PHP é: `Add-WindowsFeature Web-Server-PHP`  
B) Apache HTTP Server, o comando para gerar um novo certificado SSL utilizando o módulo `mod_ssl` é: `sudo apachectl enable mod_ssl --generate-ssl-cert`  
C) JBoss, o comando para habilitar a porta de administração remota e permitir que o servidor seja acessado externamente é: `./jboss-cli.sh --connect --command=:enable-remote-access`  
D) Apache HTTP Server, o arquivo de configuração principal pode ser editado diretamente para alterar o diretório raiz (DocumentRoot). O comando para alterar o diretório raiz para `/var/www/new_trtsite` é: `sudo apachectl DocumentRoot '/var/www/new_trtsite'`  
E) JBoss, para se conectar a uma instância CLI destacada e desligar o servidor, o comando correto é: `jboss-cli.sh --connect --command=shutdown`

**17.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Considere os servidores de aplicação e os arquivos de configuração a seguir:

Servidor:  
1. JBoss (WildFly)  
2. Apache HTTP Server  
3. Internet Information Services (IIS)

Arquivo de configuração:  
a. domain.xml  
b. applicationHost.config  
c. standalone.xml  
d. httpd.conf

A correta correlação entre servidor e arquivo de configuração é:

A) 1-a − 2-d − 3-c − 3-b  
B) 1-a − 1-c − 2-d − 3-b  
C) 1-b − 2-a − 3-c − 3-d  
D) 1-d − 2-c − 3-a − 3-b  
E) 1-c − 1-d − 2-b − 3-a  

**18.** (FCC – TJ-PE/2012 – Analista Judiciário – TI/Suporte)  
Em relação aos arquivos de configuração do servidor web Apache, é **INCORRETO** afirmar que a diretiva

A) `<Files>` é utilizada para agrupar diretivas por um nome de arquivo.  
B) `<Directory>` é utilizada para agrupar diretivas que serão utilizadas para o diretório nomeado, seus subdiretórios e arquivos dentro dos respectivos diretórios.  
C) `<Location>` é utilizada para configurar a localização corrente do servidor.  
D) `<Proxy>` é utilizada para limitar o escopo por conteúdo relacionado a um determinado proxy.  
E) `<DirectoryMatch>` é utilizada para agrupar diretivas que serão utilizadas para o diretório nomeado, seus subdiretórios e arquivos dentro dos respectivos diretórios.

**19.** (FCC – TJ-CE/2022 – Analista Judiciário – Ciência da Computação – Infraestrutura de TI)  
Considere que um Analista instalou o site principal no domínio example.com e o site proxy no subdomínio blog.domain.com, ambos alimentados pelo Apache. Em seguida configurou o Nginx como proxy reverso, ativou o vhost com symlink em `sites-available`/`sites-enabled` e quer: (I) testar o Nginx quanto a erros de configuração; (II) sem erros, recarregar o Nginx. Os comandos corretos e respectivos são:

A) `sudo nginx ProxyPass /blog http://blog.domain.com` · `sudo system reload nginx`  
B) `sudo nginx ProxyPass /blog http://example.com` · `sudo nginx systemctl reload`  
C) `sudo nginx -t` · `sudo systemctl reload nginx`  
D) `sudo nginx test` · `sudo nginx proxy proxy http ssl`  
E) `sudo nginx -t` · `sudo nginx reloading`

**20.** (Estilo FCC / documentação Apache — ProxyPass)  
No servidor web Apache, a diretiva `ProxyPass` do módulo `mod_proxy` tem a função de:

A) especificar os servidores proxy que terão acesso ao servidor web  
B) garantir que um site possa ser acessado por clientes que o estejam acessando por meio de servidores proxy  
C) indicar o endereço do servidor proxy que o servidor deve utilizar para ter acesso à internet  
D) mapear endereços de servidores remotos no espaço de endereços do servidor local  
E) substituir integralmente o arquivo `applicationHost.config` do IIS  

**21.** (Estilo FCC — Analista TI Judiciário)  
Um órgão usa Apache para portal estático em `/var/www/portal` e encaminha `/api` a `http://10.20.0.15:8080/api` via ProxyPass. É correto afirmar que:

A) trata-se de uso típico de Apache como **proxy reverso** para a API  
B) `ProxyRequests On` é obrigatório para esse ProxyPass funcionar  
C) o DocumentRoot passa a ser automaticamente `10.20.0.15`  
D) o cenário descreve apenas forward proxy corporativo  
E) IIS applicationHost.config deve ser editado no Linux para o ProxyPass  

**22.** (Estilo FCC)  
Sobre proxy forward × reverse, assinale a opção correta:

A) No reverse, cada usuário da Internet precisa cadastrar o IP do WildFly no browser  
B) No forward, o proxy aparece ao cliente externo como o site público institucional  
C) No reverse, o cliente não precisa configuração especial de proxy; solicita a URL pública do front  
D) Forward e reverse são rigorosamente o mesmo modo no Apache (`ProxyRequests` = `ProxyPass`)  
E) Reverse proxy não pode terminar TLS  

**23.** (Estilo FCC — IIS)  
Em administração IIS cobrável em Analista TI, o Application Pool:

A) é o nome do protocolo SNMP  
B) substitui o `httpd.conf` do Apache em todo Linux  
C) é obrigatoriamente armazenado em `domain.xml`  
D) equivale ao DocumentRoot  
E) isola aplicações em processos com identidade e reciclagem próprias  

**24.** (Estilo FCC — segurança operacional)  
Antes de ativar um forward proxy Apache em rede institucional, a medida mais alinhada à doutrina de prova é:

A) restringir clientes autorizados e evitar open proxy  
B) deixar `<Proxy "*">` aberto sem autenticação na Internet  
C) publicar o `mod_status` sem restrição na borda  
D) desativar todos os logs de acesso  
E) usar apenas `apachectl DocumentRoot` para ACL  

**25.** (Estilo FCC — síntese do subtópico)  
Para o eixo do edital (Web Proxy · Apache · IIS), a síntese correta é:

A) Proxy só existe no IIS; Apache não faz reverse; logs são irrelevantes  
B) ProxyPass ativa apenas forward; applicationHost.config é do WildFly  
C) DocumentRoot e Application Pool são sinônimos  
D) Forward: cliente usa o proxy para sair; reverse: front publica e encaminha ao backend; Apache (`httpd.conf`, ProxyPass) e IIS (`applicationHost.config`, sites/pools) são plataformas cobráveis de serviço web  
E) Nginx -t é comando do JBoss CLI  


---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **E** | FCC TRT-20/2024 — `jboss-cli.sh --connect --command=shutdown` |
| 17 | **B** | FCC TRT-1/2025 — WildFly: domain+standalone; Apache: httpd.conf; IIS: applicationHost.config |
| 18 | **C** | FCC TJ-PE/2012 — Location **não** é “localização corrente do servidor” |
| 19 | **C** | FCC TJ-CE/2022 — `nginx -t` + `systemctl reload nginx` |
| 20 | **D** | Função clássica do ProxyPass (doc Apache / bancas) |
| 21 | **A** | Reverse para API |
| 22 | **C** | Cliente sem config especial no reverse |
| 23 | **E** | Papel do App Pool |
| 24 | **A** | Não abrir open proxy |
| 25 | **D** | Síntese do eixo |

**Notas rápidas:** 16 E (CLI shutdown). 17 B (1-a,1-c,2-d,3-b). 18 C (Location). 19 C (nginx -t/reload). 20 D (mapear remoto no espaço local). 21 A. 22 C. 23 B. 24 B. 25 B.

---

*Arquivo 05 · Proxy / Apache / IIS · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 06 JBoss/WildFly.*

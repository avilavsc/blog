+++
title = '06 — JBoss EAP / WildFly'
date = '2026-10-20'
draft = false
weight = 6
tags = ['semana-07', 'jboss', 'wildfly']
categories = ['estudo']
+++
# JBoss EAP / WildFly · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — JBoss EAP / WildFly  
**Escopo deste arquivo:** servidor de aplicação Java cobrável no edital (**JBoss EAP / WildFly**): modos standalone × domain, arquivos `standalone.xml` / `domain.xml` / `host.xml`, diretório `bin`, CLI (`jboss-cli`), datasources, contraste com Apache/IIS, evolução EAP 5→6 (itens clássicos).  
**Fora de escopo aqui:** proxy/Apache/IIS a fundo (arquivo 05); SNMP/QoS (07); stack de monitoramento (08); código Java de negócio; Kubernetes (semana 3).

---

## 1. Material

### 1.1 O que a FCC chama de “servidor de aplicação”

No edital: **JBoss EAP/WildFly**. Em prova:

| Nome | Leitura |
|------|---------|
| **WildFly** | Projeto upstream (community) do servidor de aplicação Java |
| **JBoss EAP** | Produto **Red Hat** (Enterprise Application Platform) baseado na linhagem JBoss/WildFly, com suporte comercial |
| Papel | Hospeda apps **Java EE / Jakarta EE** (WAR/EAR), datasources, messaging, segurança, clustering |

Não confundir com:

| Não é | Por quê |
|-------|---------|
| Apache HTTP Server | Servidor **web** HTTP; frequentemente **na frente** (reverse) do WildFly |
| IIS | Servidor web Microsoft |
| Tomcat “puro” | Servlet container; WildFly/EAP é app server **completo** (mais subsistemas) |
| Spring Boot embutido | App com servidor embutido — outro modelo de deploy |

Arquitetura típica de tribunal:

```
Cliente → Apache/Nginx/IIS (TLS, estático, reverse)
              ↓
         WildFly / JBoss EAP (aplicação Java)
              ↓
         PostgreSQL / Oracle
```

### 1.2 Standalone × Managed Domain

Dois modos cobrados sem piedade:

| Modo | Ideia | Arquivo(s) de configuração |
|------|-------|----------------------------|
| **Standalone** | Uma instância autônoma; você gerencia cada servidor | `standalone/configuration/standalone.xml` (e variantes) |
| **Domain (managed domain)** | Gerenciamento **centralizado** de várias instâncias | `domain/configuration/domain.xml` + `host.xml` por host |

Flash:

- Precisa de **um** servidor simples → **standalone**.  
- Precisa **sincronizar** vários servers/grupos → **domain**.

No domain:

- **Domain Controller** — autoridade da configuração do domínio (`domain.xml`).  
- **Host Controller** — processo por máquina física/VM (`host.xml`: quais servers sobem naquele host, interfaces locais).  
- **Server groups** — conjuntos de servers que compartilham perfil/deployments.

Armadilha EAP 5×6 (TRT-24/2016): atribuir “managed domain” ao EAP 5 e dizer que o EAP 6 “só” roda standalone — **invertido**. Domain moderno é marca do EAP 6+.

### 1.3 Arquivos de configuração (ouro TRT-1/2025)

Correlação cobrada:

| Servidor | Arquivos |
|----------|----------|
| JBoss / WildFly | **`domain.xml`** e **`standalone.xml`** |
| Apache HTTP Server | `httpd.conf` |
| IIS | `applicationHost.config` |

Detalhes de prova:

| Arquivo | Onde | Contém (visão) |
|---------|------|----------------|
| `standalone.xml` | `standalone/configuration/` | Subsistemas, interfaces, socket-bindings, datasources da instância standalone |
| `standalone-full.xml` / `standalone-ha.xml` / `standalone-full-ha.xml` | Idem | Perfis com mais subsistemas (messaging completo, HA…) |
| `domain.xml` | `domain/configuration/` | Perfis, server-groups, deployments do domínio |
| `host.xml` | `domain/configuration/` | Servers locais, Domain Controller remoto ou local, IPs/paths do host |

Alterações via **CLI** ou Console tendem a **persistir** de volta no XML. Editar XML com o processo rodando pode ser sobrescrito — preferir CLI/console com servidor online.

### 1.4 Diretório `bin` (TRT-22/2022 — WildFly 26)

No WildFly, o diretório **`bin`** contém:

- Scripts de inicialização: `standalone.sh` / `.bat`, `domain.sh` / `.bat`  
- Utilitários: **`jboss-cli`**, **`add-user`**, **`elytron-tool`**, relatórios de diagnóstico  
- Arquivos de ambiente: `standalone.conf`, etc.

Outros diretórios (anti-confusão):

| Dir | Função |
|-----|--------|
| `modules` | Módulos/bibliotecas do servidor (classloading modular) |
| `standalone` / `domain` | Dados e config do modo respectivo |
| `appclient` | Cliente de aplicação Java EE — **não** é onde ficam os scripts principais de start |
| `docs` | Documentação |

Gabarito clássico: scripts + utilitários CLI → **`bin`**.

### 1.5 CLI — jboss-cli

**Management CLI** (`jboss-cli.sh` / `jboss-cli.bat`):

```bash
jboss-cli.sh --connect --command=shutdown
```

Esse comando foi o **gabarito** da FCC TRT-20/2024 (conectar e desligar).

Operações típicas cobráveis (reconhecer, não memorizar tutorial):

- Deploy/undeploy  
- Data sources  
- Interfaces / portas de management  
- Reload / shutdown  

Comandos **inventados** que a banca planta:

- `:enable-remote-access` como operação mágica única  
- Misturar sintaxe Apache (`apachectl DocumentRoot`) no JBoss  

Para **abrir management** a redes externas, o caminho real envolve interface de management (`0.0.0.0` vs `127.0.0.1`), firewall e usuários de management — não um único comando fictício.

### 1.6 Iniciar domain com config alternativa (TST/2017)

Cenário Windows, a partir de `EAP_HOME\bin`:

```bat
domain.bat --domain-config=config_extra.xml
```

Arquivo alternativo em `EAP_HOME/domain/configuration/`.

Distratores: `domain.msi`, `domain.sh` no Windows, `run jboss`, parâmetros inventados.

Standalone equivalente mental: `standalone.bat --server-config=standalone-full.xml` (ou `-c=`).

### 1.7 Datasources

No EAP 6+ (TRT-24/2016), datasource da instância **standalone** configura-se em:

`standalone/configuration/standalone.xml`

(ou via CLI, que persiste nesse arquivo).

Em domain, datasources entram no perfil adequado em `domain.xml` (visão de prova: “arquivo de configuração do servidor/modo”).

Não procure datasource “só” em `EAP_HOME/configuration/` no estilo EAP 5.

### 1.8 Classloading e messaging (EAP 5 × 6)

Itens históricos ainda recicláveis:

| Tema | EAP 5 (visão banca) | EAP 6+ |
|------|---------------------|--------|
| Classloading | Hierárquico clássico | **JBoss Modules** (modular; carrega o necessário) |
| Domain | Não era o modelo “managed domain” moderno | **Managed domain** disponível **e** standalone |
| Messaging | JBoss Messaging | **HornetQ** (na época do EAP 6; depois linhagem Artemis em versões seguintes — se cobrarem “substituído”, lembre a direção 5→6: Messaging → HornetQ) |

Armadilha: inverter Modules × hierárquico; inverter HornetQ × JBoss Messaging.

### 1.9 Usuários de management e Elytron

`add-user` (em `bin`) cria usuários para:

- Management (console/CLI)  
- Application realm (quando aplicável)

**Elytron** = framework de segurança do WildFly/EAP moderno (substituindo gradualmente o security subsystem legado). `elytron-tool` aparece na lista de utilitários do `bin` (TRT-22).

Para prova: saber que **existem** realm/usuários de management distintos do SO; não deixar management aberto na Internet sem autenticação.

### 1.10 Portas e interfaces (visão)

Valores default históricos (reconhecer ordem de grandeza):

| Uso | Porta típica (default clássico) |
|-----|----------------------------------|
| HTTP | 8080 |
| HTTPS | 8443 |
| Management HTTP | 9990 |
| Management native (legado) | 9999 |

Em produção institucional: reverse proxy na 443 → 8080/8443 interno; management **não** exposto na borda.

### 1.11 Deploy

Formas cobráveis:

1. Copiar WAR/EAR para `standalone/deployments/` (scanner).  
2. CLI / Console / Maven plugin.  
3. Em domain: deploy no **server-group**.

Marker files (`*.dodeploy`, `*.deployed`, `*.failed`) — noção de scanner standalone.

### 1.12 Desempenho, logs e administração (edital)

O edital agrupa “desempenho; usuários; configuração, administração e logs” junto aos servidores. No WildFly:

- **Logs**: `standalone/log/server.log` (visão); handlers no subsistema logging.  
- **Usuários**: management + security domains / Elytron.  
- **Desempenho**: JVM heap, pools de datasource, thread pools Undertow, HA.  
- **Admin**: CLI > edição manual online; profiles no domain.

### 1.13 Tabela síntese

| Situação | Resposta |
|----------|----------|
| Um servidor, config local | standalone.xml |
| Vários servers centralizados | domain.xml + host.xml |
| Scripts start/CLI | diretório **bin** |
| Desligar via CLI | `jboss-cli.sh --connect --command=shutdown` |
| Domain Windows + XML extra | `domain.bat --domain-config=...` |
| Datasource standalone | standalone.xml |
| HTTP front | Apache/IIS — não confundir arquivos |

---

## 2. Questões de treino (1–15)

**1.** WildFly/JBoss EAP, no edital TRT4, é cobrado sobretudo como:

A) Servidor DNS autoritativo
B) Servidor de **aplicação** Java EE/Jakarta EE
C) Cliente SNMP exclusivo
D) Substituição do PostgreSQL
E) Protocolo QoS DiffServ

**2.** O modo **standalone** caracteriza-se por:

A) Obrigar Domain Controller remoto
B) Instância autônoma com configuração tipicamente em `standalone.xml`
C) Usar somente `applicationHost.config`
D) Proibir datasources
E) Ser exclusivo do IIS

**3.** O modo **managed domain** serve principalmente para:

A) Compilar código C
B) Substituir o Apache HTTP Server
C) Definir DocumentRoot
D) Gerenciar de forma centralizada múltiplas instâncias/server groups
E) Gerar certificados via `apachectl`

**4.** Na correlação FCC TRT-1/2025, JBoss (WildFly) associa-se a:

A) Apenas `httpd.conf`
B) Apenas `applicationHost.config`
C) Apenas `nginx.conf`
D) Somente arquivos `.bat` sem XML
E) `domain.xml` e `standalone.xml`

**5.** Scripts `standalone.sh`, `add-user` e `elytron-tool` ficam no diretório:

A) `modules`  
B) `bin`  
C) `appclient`  
D) `lib/ext`  
E) `docs`  

**6.** O comando correto para conectar via CLI e desligar o servidor (TRT-20/2024) é:

A) `sudo apachectl DocumentRoot '/tmp'`  
B) `./jboss-cli.sh --connect --command=:enable-remote-access`  
C) `Add-WindowsFeature Web-Server-PHP`  
D) `jboss-cli.sh --connect --command=shutdown`  
E) `sudo nginx -t --shutdown`  

**7.** No Windows, para iniciar domínio com `config_extra.xml` em `domain/configuration/`:

A) `domain.bat --domain-config=config_extra.xml`
B) `domain.msi --server config-file='config_extra.xml'`
C) `./domain.sh` obrigatoriamente (mesmo no Windows)
D) `run jboss --domain-file=config_extra.xml`
E) `jboss.bat --domain-mode file-config='config_extra.xml'`

**8.** Datasource em EAP 6 **standalone** configura-se tipicamente em:

A) `EAP_HOME/domain/server/domain.xml` como único arquivo possível
B) `httpd.conf`
C) `standalone/configuration/standalone.xml`
D) `applicationHost.config`
E) MIB SNMP

**9.** Sobre classloading EAP 5 × 6, a leitura correta é:

A) EAP 5 já usava JBoss Modules; EAP 6 voltou ao hierárquico puro
B) Nenhum usa classloader
C) Modules é exclusivo do IIS
D) EAP 6 introduziu/consolidou **JBoss Modules** (modular); EAP 5 era hierárquico clássico
E) Modules fica no DocumentRoot

**10.** Em relação a messaging na passagem EAP 5 → 6 (visão da banca):

A) EAP 6 incluiu JBoss Messaging pela primeira vez; EAP 5 usava HornetQ
B) Ambos usavam apenas RabbitMQ nativo obrigatório
C) Messaging foi removido no EAP 6
D) HornetQ é arquivo do Apache
E) EAP 5: JBoss Messaging; EAP 6: HornetQ como provedor integrado típico da época

**11.** `host.xml` no domain contém sobretudo:

A) Configuração **específica do host** (servers locais, contato com Domain Controller, mapeamentos locais)
B) DocumentRoot do Apache
C) Toda a MIB
D) Apenas certificados do IIS
E) O código-fonte da aplicação

**12.** Expor a console de management WildFly diretamente na Internet sem restrição é:

A) Prática recomendada de desempenho
B) Obrigatório para ProxyPass
C) Risco operacional/segurança; management deve ficar restrito
D) Necessário para DocumentRoot
E) Equivalente a `nginx -t`

**13.** Deploy em domain, na visão cobrável, associa-se a:

A) Apenas copiar para `C:\Windows`
B) Edição do `httpd.conf` somente
C) Uso exclusivo de FTP anônimo
D) Distribuição via server-group / Domain Controller
E) Alterar OID 1.3.6

**14.** Undertow, no WildFly moderno, relaciona-se a:

A) Subsistema de **HTTP** (web) do servidor de aplicação  
B) Protocolo QoS EF  
C) Arquivo exclusivo do IIS  
D) Cliente Zabbix  
E) Substituição do PostgreSQL  

**15.** Síntese correta Apache × WildFly:

A) São o mesmo binário
B) WildFly substitui o DNS
C) Apache usa só `standalone.xml`
D) IIS e WildFly compartilham obrigatoriamente o mesmo `domain.xml`
E) Apache frequentemente faz reverse/TLS na frente; WildFly hospeda a aplicação Java


---

### Gabarito comentado (treino 1–15)

1. **B** — App server Java.  
2. **B** — Autônomo / standalone.xml.  
3. **D** — Gestão centralizada.  
4. **E** — domain + standalone.  
5. **B** — bin (TRT-22).  
6. **D** — shutdown via CLI (TRT-20).  
7. **A** — domain.bat --domain-config (TST/2017).  
8. **C** — standalone.xml (TRT-24).  
9. **D** — Modules no EAP 6.  
10. **E** — Messaging → HornetQ.  
11. **A** — Config do host.  
12. **C** — Não expor management.  
13. **D** — Server-group/domain.  
14. **A** — HTTP subsystem.  
15. **E** — Front web × app server.

---

## 3. Questões estilo concurso FCC (16–25)

> Questões reais de provas. Gabarito só ao final.

**16.** (FCC – TRT-20/2024 – Analista Judiciário – TI)  
Uma equipe de TI está conversando sobre os comandos adequados para a configuração de servidores para hospedar diversas aplicações de um Tribunal Regional do Trabalho. Uma Analista esclareceu que, em condições ideais e com as devidas permissões, no servidor de aplicação

A) IIS, o comando para habilitar o suporte ao PHP, configurando o IIS para rodar aplicações PHP é: `Add-WindowsFeature Web-Server-PHP`  
B) Apache HTTP Server, o comando para gerar um novo certificado SSL utilizando o módulo `mod_ssl` é: `sudo apachectl enable mod_ssl --generate-ssl-cert`  
C) JBoss, o comando para habilitar a porta de administração remota e permitir que o servidor seja acessado externamente é: `./jboss-cli.sh --connect --command=:enable-remote-access`  
D) Apache HTTP Server, o arquivo de configuração principal pode ser editado diretamente para alterar o diretório raiz (DocumentRoot). O comando para alterar o diretório raiz para /var/www/new_trtsite é: `sudo apachectl DocumentRoot '/var/www/new_trtsite'`  
E) JBoss, para se conectar a uma instância CLI destacada e desligar o servidor, o comando correto é: `jboss-cli.sh --connect --command=shutdown`

**17.** (FCC – TRT-15/2023 – Analista Judiciário – TI)  
Por padrão, o WildFly é distribuído com a segurança ativada para acesso à interface de gerenciamento. Isso significa que, antes de se conectar usando o console de administração ou, remotamente, usando a CLI, será necessário adicionar um novo usuário ao WildFly. Isso pode ser feito executando o

A) script `add-user.sh` contido no diretório `bin`  
B) comando `adduser` no diretório `tmp/auth`  
C) comando `adduser -a` seguido do nome do usuário e senha.  
D) script `adduser.sh` contido no diretório `server`  
E) comando `adduser -a` seguido do nome do usuário

**18.** (FCC – TRT-22/2022 – Analista Judiciário – TI)  
Na estrutura de diretórios do WildFly 26, de acordo com seu manual, encontra-se um diretório que contém scripts de inicialização, arquivos de configuração de inicialização e vários utilitários de linha de comando, como elytron-tool, add-user e relatório de diagnóstico Java disponível para ambientes Unix e Windows. Esse diretório é denominado

A) bin.  
B) modules.  
C) app/jboss.  
D) lib/ext.  
E) appclient.

**19.** (FCC – TRT-24/2016 – Analista Judiciário – TI)  
Ao comparar os servidores de aplicação Red Hat JBoss EAP versões 5 e 6, visando a instalação e configuração da versão 6, um Analista afirma corretamente que

A) no JBoss EAP 5 a arquitetura de carregamento de classe era baseado nos JBoss Modules, mas no JBoss EAP6, o carregamento de classe é hierárquico, ocultando as classes de implementação do servidor e carregando apenas as classes que o aplicativo necessita.  
B) no JBoss EAP 5, o servidor podia ser executado em um managed domain, mas no JBoss EAP 6 executa como um servidor autônomo, permitindo configurar todos os grupos dos servidores de uma só vez, mantendo as configurações sincronizadas por toda a rede de servidores.  
C) o JBoss EAP 5 usava perfis baseados na configuração de implantação, localizados no diretório `EAP_HOME/configuration/`. No JBoss EAP 6, a configuração da implantação é feita usando um arquivo e um servidor autônomo, que é configurado usando o arquivo `EAP_HOME/domain/server/domain.xml`.  
D) no JBoss EAP 6 a fonte de dados é configurada no arquivo de configuração do servidor. Caso a instância do JBoss EAP estiver sendo executada como servidor autônomo, a fonte de dados é configurada no arquivo `standalone/configuration/standalone.xml`.  
E) o JBoss Messaging passou a ser incluído no JBoss EAP 6, já que o JBoss EAP 5 usava o HornetQ como provedor messaging.

**20.** (FCC – TST/2017 – Analista Judiciário – Suporte em TI)  
Arquivo alternativo `config_extra.xml` em `EAP_HOME/domain/configuration/`. No Windows, a partir de `C:\EAP_HOME\bin>`, para iniciar como domínio gerenciado com esse arquivo:

A) `./domain.sh --domain-config=config_extra.xml`  
B) `domain.msi --server config-file='config_extra.xml'`  
C) `run jboss --domain-file=config_extra.xml`  
D) `jboss.bat --domain-mode file-config='config_extra.xml'`  
E) `domain.bat --domain-config=config_extra.xml`

**21.** (FCC – TRT-6/2018 – Analista Judiciário – TI)  
Na estrutura de arquivos de configuração do JBoss Enterprise Application Platform 6, o arquivo `standalone-ha.xml`

A) é necessário nos domínios autônomo e gerenciado, podendo ser lido somente pelo mestre do domínio.  
B) habilita os subsistemas `mod_cluster` e JGroups para um servidor autônomo, para que ele possa participar de um cluster de alta disponibilidade ou de balanceamento de carga.  
C) inclui apenas os detalhes de configuração necessários para executar um servidor como um servidor mestre de domínio autônomo, não estando presente nos servidores com domínio gerenciado.  
D) inclui detalhes de configuração específicos para um host físico em um domínio gerenciado, como interfaces de rede, conexões de socket, o nome do host e outros detalhes específicos do host.  
E) inclui apenas os detalhes de configuração necessários para executar um servidor como um servidor escravo de domínio autônomo, não estando presente em servidores de domínio gerenciado.

**22.** (FCC – TRT-15/2024 – Técnico Judiciário – TI)  
Uma equipe técnica está utilizando o servidor de aplicações WildFly para hospedar uma aplicação Java EE crítica. Para monitorar os eventos de execução e falhas, decidiram integrar a aplicação com o Elastic Stack (Elasticsearch, Logstash e Kibana). Em condições ideais, a melhor abordagem para capturar e analisar os logs da aplicação é

A) configurar o WildFly para enviar logs diretamente ao Elasticsearch, pois isso evita sobrecarga com intermediários como Kibana ou JSONfile, agilizando o acesso ao dashboard ELK.  
B) configurar o Logstash para enviar logs diretamente ao Kibana, que armazenará e analisará os dados em XML sem a necessidade do Elasticsearch.  
C) configurar o WildFly para usar o Filebeat como agente, enviando os logs para o Logstash, que fará o pré-processamento e encaminhará os dados ao Elasticsearch.  
D) direcionar os logs do WildFly para o Kibana, o que permitirá análises em tempo real, e depois sincronizar com o Elasticsearch para o armazenamento dos logs em JSONscript.  
E) configurar o Elasticsearch para buscar e analisar os logs do WildFly em XML, depois realizar consultas no console do Logstash para análise.

**23.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Considere os servidores de aplicação e os arquivos de configuração a seguir:

Servidor:  
1. Apache HTTP Server  
2. Nginx  
3. Tomcat  

Arquivo de configuração:  
a. httpd.conf  
b. server.xml  
c. .htaccess  
d. nginx.conf  

A correta correlação entre servidor e arquivo de configuração é:

A) 1-a – 2-d – 3-c – 3-b.  
B) 1-a – 1-c – 2-d – 3-b.  
C) 1-b – 2-a – 3-c – 3-d.  
D) 1-d – 2-c – 3-a – 3-b.  
E) 1-c – 1-d – 2-b – 3-a.

**24.** (FGV – CVM/2024 – Analista CVM – TI Infraestrutura e Segurança)  
Gomes foi contratado pela Comissão de Valores Mobiliários (CVM) para fazer uma alteração nas diretivas do servidor Apache da CVM. Ele solicitou uma modificação na pasta raiz indicando a partir de qual pasta do servidor os arquivos e pastas contidos na URL solicitada serão pesquisados. Desse modo, assumindo o caminho como /opt/www, se a URL solicitada for http://www.cvm.com/9.10/index.html, o arquivo index.html será procurado na pasta /opt/www/9.10. Gomes deverá alterar a configuração na diretiva:

A) DocumentRoot  
B) DirectoryIndex  
C) ServerRoot /etc/apache2  
D) MaxSpareServers  
E) UserDir public_html

**25.** (FCC – TJ-CE/2022 – Analista Judiciário – Ciência da Computação – Infraestrutura de TI)  
Considere que um Analista instalou o site principal no domínio example.com e o site proxy no subdomínio blog.domain.com, ambos alimentados pelo Apache. Em seguida configurou o Nginx como proxy reverso, ativou o vhost com symlink em `sites-available`/`sites-enabled` e quer: (I) testar o Nginx quanto a erros de configuração; (II) sem erros, recarregar o Nginx. Os comandos corretos e respectivos são:

A) `sudo nginx ProxyPass /blog http://blog.domain.com` · `sudo system reload nginx`  
B) `sudo nginx ProxyPass /blog http://example.com` · `sudo nginx systemctl reload`  
C) `sudo nginx -t` · `sudo systemctl reload nginx`  
D) `sudo nginx test` · `sudo nginx proxy proxy http ssl`  
E) `sudo nginx -t` · `sudo nginx reloading`

---

### Gabarito comentado (FCC 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **E** | FCC TRT-20/2024 — jboss-cli shutdown |
| 17 | **A** | FCC TRT-15/2023 — add-user.sh em bin |
| 18 | **A** | FCC TRT-22/2022 — diretório bin |
| 19 | **D** | FCC TRT-24/2016 — datasource em standalone.xml |
| 20 | **E** | FCC TST/2017 — domain.bat --domain-config |
| 21 | **B** | FCC TRT-6/2018 — standalone-ha.xml |
| 22 | **C** | FCC TRT-15/2024 — Filebeat → Logstash → ES |
| 23 | **B** | FCC TRT-1/2025 — correlação Apache/Nginx/Tomcat |
| 24 | **A** | FGV CVM/2024 — DocumentRoot (contexto servidor web na frente do app server) |
| 25 | **C** | FCC TJ-CE/2022 — nginx -t + reload |

**Notas rápidas:** 16 E. 17 A. 18 A. 19 D. 20 E. 21 B. 22 C. 23 B. 24 A. 25 C.

Distribuição do gabarito (16–25): E, A, A, D, E, B, C, B, A, C — letras A–E presentes.

*Arquivo 06 · JBoss EAP / WildFly · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 07 SNMP e QoS.*

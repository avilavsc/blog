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

**16.** (FCC – TRT-20/2024 – Analista Judiciário – TI)  
Uma equipe discute comandos para servidores de um TRT. Em condições ideais, no servidor de aplicação

A) IIS: `Add-WindowsFeature Web-Server-PHP` habilita PHP  
B) Apache: `sudo apachectl enable mod_ssl --generate-ssl-cert` gera certificado  
C) JBoss: `./jboss-cli.sh --connect --command=:enable-remote-access` habilita admin remoto  
D) Apache: `sudo apachectl DocumentRoot '/var/www/new_trtsite'` altera DocumentRoot  
E) JBoss: `jboss-cli.sh --connect --command=shutdown` conecta via CLI e desliga o servidor  

**17.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Correlação servidor × arquivo:

1. JBoss (WildFly) · 2. Apache HTTP Server · 3. IIS  
a. domain.xml · b. applicationHost.config · c. standalone.xml · d. httpd.conf  

A) 1-a − 2-d − 3-c − 3-b  
B) 1-a − 1-c − 2-d − 3-b  
C) 1-b − 2-a − 3-c − 3-d  
D) 1-d − 2-c − 3-a − 3-b  
E) 1-c − 1-d − 2-b − 3-a  

**18.** (FCC – TRT-22/2022 – Analista Judiciário – TI)  
Na estrutura de diretórios do WildFly 26, o diretório que contém scripts de inicialização, arquivos de configuração de inicialização e utilitários de linha de comando (elytron-tool, add-user, relatório de diagnóstico Java) para Unix e Windows é:

A) bin  
B) modules  
C) app/jboss  
D) lib/ext  
E) appclient  

**19.** (FCC – TRT-24/2016 – Analista Judiciário – TI)  
Ao comparar JBoss EAP 5 e 6, visando instalação/configuração da versão 6, afirma corretamente que

A) no EAP 5 o classloading era baseado em JBoss Modules; no EAP 6 voltou a ser hierárquico ocultando classes  
B) no EAP 5 havia managed domain; no EAP 6 só existe servidor autônomo  
C) no EAP 5 os perfis ficavam em `EAP_HOME/configuration/`; no EAP 6 o standalone usa `EAP_HOME/domain/server/domain.xml`  
D) no EAP 6 a fonte de dados do servidor autônomo é configurada em `standalone/configuration/standalone.xml`  
E) o JBoss Messaging passou a ser incluído no EAP 6, pois o EAP 5 usava HornetQ  

**20.** (FCC – TST/2017 – Analista Judiciário – Suporte em TI)  
Arquivo alternativo `config_extra.xml` em `EAP_HOME/domain/configuration/`. No Windows, a partir de `C:\EAP_HOME\bin>`, para iniciar como domínio gerenciado com esse arquivo:

A) `./domain.sh --domain-config=config_extra.xml`  
B) `domain.msi --server config-file='config_extra.xml'`  
C) `run jboss --domain-file=config_extra.xml`  
D) `jboss.bat --domain-mode file-config='config_extra.xml'`  
E) `domain.bat --domain-config=config_extra.xml`  

**21.** (Estilo FCC)  
Um analista precisa de um único WildFly de desenvolvimento sem Domain Controller. O caminho mais direto é:

A) Usar modo **standalone** com `standalone.xml`  
B) Subir obrigatoriamente domain com dois Host Controllers  
C) Instalar IIS e renomear para WildFly  
D) Configurar apenas `httpd.conf`  
E) Usar `applicationHost.config` como único XML do WildFly  

**22.** (Estilo FCC)  
Sobre `domain.xml` × `host.xml`:

A) Ambos são arquivos exclusivos do Apache  
B) `host.xml` substitui o PostgreSQL  
C) `domain.xml` só existe no IIS  
D) `domain.xml` traz política/perfis do domínio; `host.xml` traz aspectos do host local e servers da máquina  
E) São sinônimos rigorosos de DocumentRoot  

**23.** (Estilo FCC)  
A porta de management HTTP clássica do WildFly/EAP (default de muitas instalações) é da ordem de:

A) 53  
B) 161  
C) 9990  
D) 25  
E) 123  

**24.** (Estilo FCC — anti-confusão)  
Afirmação correta:

A) `ProxyPass` é diretiva nativa do `standalone.xml`  
B) `add-user` fica em `modules` obrigatoriamente  
C) EAP não possui modo standalone  
D) WildFly não pode ficar atrás de reverse proxy  
E) `jboss-cli` tipicamente reside em `bin` e gerencia o app server  

**25.** (Estilo FCC — síntese)  
No eixo JBoss EAP/WildFly do edital TRT4, o cartão mental adequado é:

A) Só memorizar Apache; WildFly não cai  
B) WildFly = protocolo SNMP  
C) Standalone × domain; XML standalone/domain/host; `bin` + CLI; datasources no XML do modo; contraste com Apache/IIS na frente  
D) domain.xml = applicationHost.config  
E) Shutdown apenas via `nginx reloading`  


---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **E** | FCC TRT-20/2024 |
| 17 | **B** | FCC TRT-1/2025 |
| 18 | **A** | FCC TRT-22/2022 — bin |
| 19 | **D** | FCC TRT-24/2016 — standalone.xml datasource |
| 20 | **E** | FCC TST/2017 — domain.bat --domain-config |
| 21 | **A** | Standalone para um servidor |
| 22 | **D** | Papéis domain.xml / host.xml |
| 23 | **C** | 9990 management HTTP |
| 24 | **E** | CLI em bin |
| 25 | **C** | Síntese do subtópico |

**Notas:** 16 E. 17 B. 18 A. 19 D (demais invertem Modules/domain/messaging). 20 E. 21 B. 22 B. 23 C. 24 B. 25 B.

---

*Arquivo 06 · JBoss EAP / WildFly · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 07 SNMP e QoS.*

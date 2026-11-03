+++
title = '08 — Monitoramento: Zabbix, Grafana, Prometheus e ELK'
date = '2026-10-22'
draft = false
weight = 8
tags = ['semana-07', 'monitoramento', 'zabbix']
categories = ['estudo']
+++
# Monitoramento: Zabbix, Grafana, Prometheus e ELK · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 08 — Zabbix · Grafana · Prometheus · ELK (+ Fluentd no edital)  
**Escopo deste arquivo:** papéis cobráveis de **monitoramento/observabilidade**: Zabbix (NMS/gerente, agent/agentless, SNMP), Prometheus (métricas/séries temporais/scrape/alertas), Grafana (dashboards/visualização), stack **ELK** (Elasticsearch · Logstash · Kibana) e menção a **Fluentd**; contrastes anti-confusão entre métricas × logs × visualização.  
**Fora de escopo aqui:** SNMP/QoS a fundo (arquivo 07 — só o encaixe); instalação passo a passo; APM comercial completo; segurança OWASP (semana 8).

---

## 1. Material

### 1.1 O que o edital lista

**Gerenciamento de redes:** … métricas e avaliação de desempenho; **Zabbix, Elasticsearch, Logstash, Kibana, Grafana, Prometheus, Fluentd**.

A FCC não pede admin diário de cluster Elastic. Pede **quem faz o quê**:

| Ferramenta | Papel de prova |
|------------|----------------|
| **Zabbix** | Monitoramento completo / **gerente SNMP**; coleta, triggers, alertas |
| **Prometheus** | Coleta/armazena **métricas** (time series); regras de alerta |
| **Grafana** | **Visualização** (dashboards); alerta sobre fontes; não é o coletor clássico |
| **Elasticsearch** | **Indexação/busca** de dados (logs) |
| **Logstash** | **Pipeline** de ingestão: coleta → transforma → envia |
| **Kibana** | **Visualização/análise** sobre o Elasticsearch |
| **Fluentd** | Coletor/roteador de logs (alternativa/complemento ao Logstash) |

### 1.2 Três pilares mentais (métricas × logs × traces)

Para Analista TI FCC (sem exagerar em OpenTelemetry):

| Pilar | Pergunta | Ferramentas do edital |
|-------|----------|------------------------|
| **Métricas** | CPU, latência p99, taxa de erros HTTP | Prometheus, Zabbix |
| **Logs** | O que aconteceu neste request/erro? | ELK, Fluentd |
| **Visualização** | Painel único para humanos | Grafana, Kibana |

Traces distribuídos caem pouco no texto literal do edital — foque métricas/logs/NMS.

### 1.3 Zabbix

**Zabbix** = plataforma de monitoramento de redes, servidores, serviços e aplicações.

Capacidades cobráveis:

- **Agente** Zabbix nos hosts (Windows, Linux…).  
- **Agentless**: SNMP, ICMP, SSH, HTTP, IPMI etc. (TRT-24/2016 — é **falso** dizer que não há agentless).  
- Servidor tipicamente em Linux/Unix, **mas** há agentes Windows (falso dizer que não existem).  
- Templates, items, triggers, actions, mapas, dashboards próprios.  
- Atua como **gerente SNMP** (TRT-1/2025).

Ciclo mental:

```
Host/dispositivo → (agente | SNMP | ICMP…) → Zabbix Server → triggers → alerta (e-mail, script…)
```

Não confundir Zabbix com:

| Ferramenta | Diferença |
|------------|-----------|
| Grafana | Zabbix **coleta e decide** alerta; Grafana **mostra** (pode alertar sobre datasources) |
| ELK | Foco Zabbix = métricas/disponibilidade/triggers; ELK = logs/busca |
| Prometheus | Ambos métricas; modelos diferentes (pull scrape × modelo Zabbix) |

### 1.4 Prometheus

**Prometheus** = sistema de monitoramento e **banco de séries temporais**, modelo **pull** (scrape) de endpoints HTTP (`/metrics`), com linguagem de consulta **PromQL** e **Alertmanager** (alertas).

Responsabilidades corretas (padrão de prova recente):

| Faz | Não é o foco |
|-----|----------------|
| Coletar métricas | Substituir ELK de logs |
| Armazenar time series | Ser só “tela bonita” |
| Avaliar regras de alerta | Substituir DNS |
| Service discovery (K8s etc.) | Indexar logs full-text como ES |

Exporters: node_exporter, blackbox, mysqld, etc. — noção de “expor métricas no formato Prometheus”.

Contraste ouro com Grafana:

> Prometheus: coleta + storage de métricas + regras de alerta.  
> Grafana: dashboards/visualização (e alertas sobre a fonte).

Inverter os papéis é distrator clássico.

### 1.5 Grafana

**Grafana** = plataforma de **observabilidade visual**: dashboards, variáveis, alertas, muitas **data sources** (Prometheus, Zabbix, Elasticsearch, MySQL, Loki…).

FCC PMSP/2025: para tempo quase real + alertas, combinar Grafana com fonte adequada como **Prometheus** — não CSV manual, nem “importar Tableau”, nem Power BI Embedded como solução nativa.

Flash:

- Grafana **não** é gerente SNMP (TRT-1/2025).  
- Grafana **não** coleta scrape como Prometheus por padrão.  
- Grafana **brilha** na camada de apresentação.

### 1.6 ELK Stack

**ELK** = **E**lasticsearch + **L**ogstash + **K**ibana.

| Peça | Função |
|------|--------|
| **Elasticsearch** | Motor de busca/análise; **indexa** documentos (logs); busca rápida |
| **Logstash** | Pipeline: input → filter → output; **transforma** antes de enviar ao destino |
| **Kibana** | UI: dashboards, Discover, visualizações sobre índices ES |

Fluxo canônico:

```
Apps/syslog/beats → Logstash (parse/grok) → Elasticsearch ← Kibana (humano)
```

FCC TRF-4/2025: equipe DevSecOps com ELK — prática correta associada a **Elasticsearch para indexação/busca** e **Kibana para visualização/análise gráfica**.

Armadilhas:

- Logstash **não** é gerente SNMP.  
- Elasticsearch **não** “envia logs aos agentes SNMP”.  
- Kibana **não** substitui Zabbix como NMS.  
- Permissões amplas no ES “para facilitar” = mau (DevSecOps).  
- Assumir fontes de log sempre confiáveis e ACL só no dashboard = falha de segurança.

**Beats** (Filebeat etc.) e **Elastic Agent** aparecem no ecossistema — se a banca citar, são shippers leves; o trio do edital continua ELK.

### 1.7 Fluentd

**Fluentd** = coletor unificado de logs (CNCF), roteia para múltiplos destinos (ES, arquivo, Kafka…). No edital aparece ao lado do ELK: trate como **alternativa/complemento de ingestão**, não como Kibana.

Comparação rápida:

| | Logstash | Fluentd |
|---|----------|---------|
| Papel | Pipeline ETL de logs | Coletor/roteador de logs |
| Ecossistema | Elastic | Plugins amplos / cloud-native |
| Em prova | Peça do ELK | Nome a reconhecer no mapa de ingestão |

### 1.8 Tabela anti-confusão (decorar)

| Preciso… | Escolha mental |
|----------|----------------|
| Gerente SNMP + triggers de infra | **Zabbix** |
| Métricas K8s/microsserviços, PromQL | **Prometheus** |
| Dashboard unificado bonito | **Grafana** (fonte Prometheus/Zabbix/ES…) |
| Buscar texto em milhões de linhas de log | **Elasticsearch** (+ Kibana) |
| Parsear syslog e enriquecer campos | **Logstash** (ou Fluentd) |
| Só “ver” índices ES | **Kibana** |

### 1.9 Integrações típicas em tribunal / PDPJ

Cenários cobráveis sem inventar produto:

1. Zabbix monitora hosts, switches (SNMP), serviços HTTP do PJe/PDPJ.  
2. Prometheus + Grafana em cluster Kubernetes (semana 3).  
3. ELK centraliza logs de Apache/WildFly/aplicações (arquivos 05–06).  
4. Grafana lê Prometheus **e** pode ler Elasticsearch — papéis permanecem distintos.

### 1.10 Alertas: onde moram?

| Sistema | Alerta |
|---------|--------|
| Zabbix | Triggers + actions (nativo e forte) |
| Prometheus | Regras + Alertmanager |
| Grafana | Alert rules sobre painéis/datasources |
| Kibana/ES | Watcher / regras (visão: possível, mas Kibana ≠ NMS SNMP) |

Prova gosta de: “quem **coleta** métricas?” ≠ “quem **desenha** o gráfico?”.

### 1.11 Agent × agentless (reforço Zabbix)

| Modo | Exemplos |
|------|----------|
| Com agente | CPU detalhada, inventário, scripts no host |
| Agentless | SNMP em switch, ping ICMP, HTTP check, SSH |

Afirmações **falsas** TRT-24:

- “Zabbix não permite agentless”.  
- “Não há agente Windows”.

### 1.12 Desempenho e métricas (fecha o edital)

Avaliação de desempenho no gerenciamento de redes usa:

- Coleta contínua (Zabbix/Prometheus).  
- Baselines e SLOs mentais (disponibilidade, latência).  
- Logs para root cause (ELK).  
- Dashboards para operação (Grafana/Kibana/Zabbix).

Não misture: **alta cardinalidade** de métricas mal desenhadas derruba Prometheus; **logs sem retenção/ACL** derrubam Elastic — visão de prova = responsabilidade de cada peça.

### 1.13 Mapa único

```
          ┌── SNMP/ICMP/Agent ──► Zabbix ──► alertas/NMS
Hosts ────┤
Apps  ────┼── /metrics scrape ──► Prometheus ──► Alertmanager
          │                           │
          │                      Grafana ◄── dashboards
Logs  ────┴── Fluentd/Logstash ──► Elasticsearch ──► Kibana
```

---

## 2. Questões de treino (1–15)

**1.** No edital TRT4, Zabbix aparece sobretudo como ferramenta de:

A) Edição de `httpd.conf`
B) Monitoramento de redes/servidores/serviços (NMS)
C) Substituição do WildFly
D) Protocolo DiffServ
E) Compilação C

**2.** É **falso** afirmar sobre o Zabbix:

A) Pode usar SNMP  
B) Não permite monitoramento agentless em hipótese alguma  
C) Pode monitorar via agente  
D) Pode disparar alertas por triggers  
E) Pode atuar como gerente SNMP  

**3.** Prometheus, na distinção correta frente ao Grafana:

A) Só desenha gráficos; Grafana faz scrape
B) Indexa logs full-text como Elasticsearch
C) É gerente SNMP exclusivo no lugar do Zabbix segundo a FCC TRT-1
D) Substitui o Kibana obrigatoriamente
E) Coleta/armazena métricas em séries temporais e pode definir alertas; Grafana visualiza dashboards

**4.** Grafana é melhor descrito como:

A) Banco relacional
B) Protocolo UDP 161
C) Arquivo `standalone.xml`
D) Plataforma de visualização/dashboards sobre múltiplas fontes
E) Community SNMP

**5.** No ELK, o Elasticsearch:

A) Indexa e permite busca/análise eficiente dos dados/logs
B) Apenas desenha pizza charts sem armazenar dados
C) É o nome do agente Zabbix no Windows
D) Faz scrape Prometheus nativo exclusivo
E) Marca pacotes com DSCP EF

**6.** Logstash concentra-se em:

A) Ser o Domain Controller do WildFly
B) Substituir o DNS
C) Pipeline de coleta/transformação/envio de eventos
D) Gerar DocumentRoot
E) Autenticar SNMPv3 apenas

**7.** Kibana relaciona-se tipicamente a:

A) Gerente SNMP cobrado na TRT-1/2025 no lugar do Zabbix  
B) Visualização/análise sobre dados no Elasticsearch  
C) Arquivo `applicationHost.config`  
D) Marcação CS6  
E) `jboss-cli shutdown`  

**8.** Fluentd, no mapa do edital, aproxima-se de:

A) Apenas um tema de QoS
B) Substituição do TCP
C) Módulo `mod_ssl`
D) App Pool do IIS
E) Coletor/roteador de logs (ingestão)

**9.** Para microsserviços em Kubernetes com métricas e painéis, o par clássico é:

A) Prometheus + Grafana
B) Only Kibana + SNMP v1
C) DocumentRoot + MIB
D) IIS + domain.xml apenas
E) AF51 + Logstash

**10.** Segundo a linha FCC TRT-1/2025, exemplo de gerente SNMP:

A) Logstash  
B) Elasticsearch  
C) Grafana  
D) Zabbix  
E) Kibana  

**11.** Configurar Grafana lendo CSV com atualização manual para “tempo real” institucional:

A) Atende perfeitamente requisitos de tempo real + alertas dinâmicos
B) Transforma Grafana em Zabbix NMS
C) É obrigatório no ELK
D) Não atende bem o requisito de atualização em tempo real
E) Equivale a PromQL

**12.** Em DevSecOps com ELK, prática alinhada à FCC TRF-4/2025:

A) Permissões amplas no ES priorizando só visibilidade  
B) ACL apenas no dashboard, fontes sempre confiáveis  
C) Elasticsearch para indexação/busca e Kibana para visualização/análise  
D) Evitar qualquer correlação entre fontes de log  
E) Usar Kibana como único controle de acesso sem segmento de perfis no ES  

**13.** Triggers no Zabbix servem para:

A) Compilar Java
B) Substituir ProxyPass
C) Definir DSCP
D) Criar VirtualHost
E) Avaliar condições sobre items e disparar ações/alertas

**14.** Modelo de coleta típico do Prometheus:

A) Scrape (pull) de endpoints de métricas HTTP
B) Apenas push SMTP
C) Somente traps SNMP UDP 162
D) Apenas leitura de `httpd.conf`
E) GET exclusivos em MIB sem HTTP

**15.** Síntese correta do subtópico:

A) Todas as ferramentas do edital fazem a mesma coisa
B) Kibana é PHB DiffServ
C) Zabbix=NMS/SNMP; Prometheus=métricas TSDB; Grafana=dashboards; ELK=logs (ES busca, Logstash pipeline, Kibana UI); Fluentd=ingestão de logs
D) Prometheus indexa logs no lugar do ES
E) Grafana é o único gerente SNMP aceito pela FCC


---

### Gabarito comentado (treino 1–15)

1. **B** — NMS/monitoramento.  
2. **B** — Agentless existe (afirmação falsa).  
3. **E** — Papéis Prometheus × Grafana.  
4. **D** — Visualização.  
5. **A** — Indexação/busca.  
6. **C** — Pipeline.  
7. **B** — UI do ES.  
8. **E** — Coletor de logs.  
9. **A** — Par clássico.  
10. **D** — Zabbix (TRT-1).  
11. **D** — CSV manual ≠ tempo real.  
12. **C** — Papéis ES/Kibana.  
13. **E** — Triggers→ações.  
14. **A** — Scrape pull.  
15. **C** — Mapa completo.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
O funcionamento do SNMP se baseia na comunicação entre agentes e gerentes. Um exemplo de gerente SNMP é o

A) Logstash, que é uma ferramenta de busca e análise de dados baseado em índices Object Identifiers (OIDs)  
B) Elasticsearch, que é um mecanismo de coleta, processamento e envio de logs para os agentes SNMP  
C) Grafana, que é uma ferramenta para visualização e análise de dados em tempo real, focada na emissão de alertas de segurança  
D) Zabbix, que permite centralizar a coleta e análise de dados dos dispositivos de rede  
E) Kibana, que é uma ferramenta de visualização de dados e dashboards com funcionalidade de análise automática de logs  

**17.** (FCC – TRT-24/2016 – Analista Judiciário – TI)  
Assinale a afirmativa correta:

A) O Zabbix é uma ferramenta de monitoramento que **não** permite monitoramento agentless  
B) Como o servidor Zabbix é obrigatoriamente instalado em Unix/Linux, **não** há agentes Zabbix para Windows  
C) O ICMP, assim como TCP e UDP, é protocolo de controle também usado para transmissão de dados de aplicação, com ping exclusivo do Linux  
D) No SNMP o item a ser monitorado ou gerenciado é um agente. Quem consulta (GET) ou solicita modificações (SET) é um gerente. O agente também tem a função de gerar alertas (TRAP)  
E) No IPTables, `-s ALL` aplica-se a SSH, HTTP e HTTPS sem regra separada por protocolo  

**18.** (FCC – PMSP/2025 – Analista TIC)  
Painel com múltiplas fontes, atualização em tempo real e alertas críticos; equipe optou pelo Grafana. A configuração que atende tempo real e alertas é:

A) Conectar Grafana a MySQL com atualizações/alertas a cada 5 minutos  
B) Importar relatórios do Tableau e alertar em intervalos regulares  
C) Utilizar Grafana com fonte como o Prometheus para tempo real e alertas  
D) Conectar a CSV com atualizações manuais  
E) Configurar Grafana com Power BI Embedded para alertas/tempo real  

**19.** (FCC – TRF-4/2025 – Analista – Análise de Sistemas de Informação)  
Em modernização com DevSecOps e ELK Stack, a equipe deverá adotar práticas que

A) configurem o Logstash priorizando apenas regras genéricas sem observabilidade  
B) usem Logstash na ingestão, mas concentrem ACL só no dashboard, assumindo fontes sempre confiáveis  
C) empreguem o Elasticsearch para indexação e busca eficiente de logs, utilizando o Kibana para visualização e análise gráfica dos dados  
D) integrem o Kibana ao CI/CD evitando correlações entre logs de diferentes fontes  
E) mantenham o Elasticsearch com permissões amplas priorizando visibilidade sobre segmentação por perfis  

**20.** (Estilo FCC / observabilidade em microsserviços)  
Em arquitetura Kubernetes com Prometheus e Grafana, a distinção tecnicamente correta é:

A) Prometheus e Grafana têm funções idênticas, só mudam a UI e a licença  
B) Prometheus realiza coleta e armazenamento de métricas em séries temporais e pode definir regras de alerta, enquanto o Grafana é utilizado para visualização e construção de dashboards  
C) Prometheus visualiza; Grafana faz scrape e guarda as séries  
D) Prometheus armazena logs estruturados e tracing; Grafana só faz SQL em SGDB da aplicação  
E) Grafana faz scraping periódico dos endpoints; Prometheus só consome para gráficos  

**21.** (Estilo FCC)  
Fluentd no conjunto do edital TRT4 associa-se principalmente a:

A) Marcação DiffServ CS6  
B) Arquivo `domain.xml`  
C) Substituição do SNMP gerente  
D) Módulo exclusivo do IIS  
E) Coleta/roteamento de logs em pipelines de observabilidade  

**22.** (Estilo FCC)  
Um analista quer buscar “NullPointerException” em 30 dias de logs de várias JVMs. A peça central mais adequada do ELK é:

A) Elasticsearch (busca/indexação), com Kibana na interface  
B) Apenas Grafana sem backend  
C) Somente node_exporter  
D) Apenas Alertmanager sem storage  
E) ProxyPass  

**23.** (Estilo FCC)  
Sobre alertas:

A) Só o Kibana pode alertar em qualquer cenário do edital  
B) Prometheus não avalia regras  
C) Zabbix não possui triggers  
D) Grafana proíbe data source Prometheus  
E) Zabbix (triggers), Prometheus/Alertmanager e Grafana (alert rules) são caminhos cobráveis de alerta, cada um no seu papel  

**24.** (Estilo FCC — anti-confusão ELK × métricas)  
Afirmação correta:

A) Elasticsearch não substitui, por si, o papel de NMS SNMP do Zabbix  
B) Logstash é o substituto nativo do Zabbix como gerente SNMP na FCC TRT-1/2025  
C) Kibana coleta SNMP GET na UDP 161  
D) ELK marca pacotes EF  
E) Fluentd é PHB  

**25.** (Estilo FCC — síntese do edital)  
Mapa correto das ferramentas listadas (Zabbix, ES, Logstash, Kibana, Grafana, Prometheus, Fluentd):

A) Todas são protocolos de camada 2  
B) Grafana = único coletor SNMP; ES = DiffServ  
C) Prometheus = Kibana; Zabbix = apenas CSV  
D) Logstash = applicationHost.config  
E) Zabbix monitora/alerta (NMS); Prometheus métricas TSDB; Grafana visualiza; ES+Logstash+Kibana = busca/pipeline/UI de logs; Fluentd apoia ingestão de logs  


---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **D** | FCC TRT-1/2025 — Zabbix gerente SNMP |
| 17 | **D** | FCC TRT-24/2016 — SNMP agente/gerente (+ derruba mitos Zabbix) |
| 18 | **C** | FCC PMSP/2025 — Grafana + Prometheus |
| 19 | **C** | FCC TRF-4/2025 — ES indexa/busca; Kibana visualiza |
| 20 | **B** | Distinção Prometheus × Grafana |
| 21 | **E** | Fluentd = logs ingest |
| 22 | **A** | ES (+ Kibana) |
| 23 | **E** | Alertas nos papéis certos |
| 24 | **A** | ELK ≠ NMS SNMP |
| 25 | **E** | Síntese do eixo do edital |

**Notas:** 16 D. 17 D (A/B falsas sobre Zabbix). 18 C. 19 C (segurança/ACL nas outras). 20 B. 21 B. 22 B. 23 B. 24 B. 25 B.

---

*Arquivo 08 · Monitoramento Zabbix / Grafana / Prometheus / ELK · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 7 — bloco serviços/gerência.*

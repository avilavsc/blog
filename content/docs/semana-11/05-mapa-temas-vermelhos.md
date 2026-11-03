+++
title = '05 — Mapa — Temas vermelhos FCC (<60%)'
date = '2026-10-31'
draft = false
weight = 5
tags = ['semana-11', 'mapa']
categories = ['estudo']
+++
# Mapa — Temas vermelhos FCC (<60%) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — Semana 11 = revisão · mapa único de alto risco  
**Escopo deste arquivo:** **contrastes cobráveis** de Ágeis · ITIL · COBIT · SO (processos/memória) · Windows/AD · IA/ML/LLM — para quem está abaixo de ~60% de acerto. Formato **mapa**, não tutorial longo.  
**Fora de escopo aqui:** reforço redes (`03`) e segurança (`04`); reescrever packs inteiros das semanas 4 e 9; PDTIC/MoReq (semana 10).

**Priors:** semana 4 (`01`, `07`, `08`); semana 9 (`01`–`02`, `04`–`05`, `07`–`08`).

---

## 1. Material — mapa de contrastes

### 1.1 Como usar este mapa

1. Abra o caderno de erros: marque só o que ainda erra.  
2. Decore a **coluna da direita** (anti-confusão), não o livro.  
3. Faça treino 1–15 sem olhar; depois FCC 16–25.  
4. Se acertar ≥80% no bloco, tire o tema do “vermelho” e volte só no simulado.

### 1.2 Ágeis — Scrum × Kanban × XP

| | Scrum | Kanban | XP |
|---|-------|--------|-----|
| Ênfase | Papéis/eventos/artefatos · Sprint | Fluxo · **WIP** · pull | Práticas de **engenharia** |
| Cadência | Sprint time-boxed | Contínuo | Iterações curtas + rituais técnicos |
| Dono típico de “refatoração/TDD/pares” | Não é “do Scrum” | Não | **XP** |
| Métrica clássica | Velocity / DoD | Lead/cycle time · WIP | Qualidade via testes |

**Manifesto:** valoriza mais indivíduos, software funcionando, colaboração, responder a mudanças — **sem anular** processos, docs, contratos, plano.

**Scrum flash:** PO · SM · Developers · Planning · Daily · Review · Retro · Backlogs · Increment · DoD.  
Armadilha FCC: Scrum **não** é ciclo linear/sequencial (waterfall disfarçado).

**XP flash:** TDD, pair programming, refatoração contínua, integração contínua — quando a banca fala qualidade de código intensa → XP (FCC TRT-20/2024).

### 1.3 ITIL × COBIT (ouro de prova)

| | **ITIL** (edital: Version 5 / base 4) | **COBIT 2019** |
|---|--------------------------------------|----------------|
| Foco | **Serviços/produtos** · valor · practices | **Governança + gestão** de I&T |
| Pergunta | Como entregar/operar serviço com valor? | I&T alinhada, dirigida, monitorada? |
| Peças quentes | Incident × Problem × Change × SLM × Config × Service Desk | EDM × APO × BAI × DSS × MEA |
| Não é | Norma ISO de SGSI | Manual do Service Desk |

**ITIL — papéis trocados (FCC TRT-1/2025):**

| Descrição | Practice correta |
|-----------|------------------|
| Restaurar rápido / registrar falha | **Incident** |
| Causa raiz / reduzir recorrência | **Problem** |
| Avaliar/autorizar alteração | **Change** (habilitação) |
| Acordos de qualidade/disponibilidade | **SLM** |
| Itens de configuração (CI) | **Service Configuration** |
| Expertise de infra | Technical Management (≠ “fechar ticket”) |

**COBIT 2019 — 5×40:**

| Domínio | Verbos | Qtde |
|---------|--------|------|
| **EDM** | Evaluate, Direct, Monitor (**governança**) | 5 |
| **APO** | Align, Plan, Organize | 14 |
| **BAI** | Build, Acquire, Implement | 11 |
| **DSS** | Deliver, Service, Support | 6 |
| **MEA** | Monitor, Evaluate, Assess | 4 |

Legado COBIT (ainda cai): PO / AI / DS / ME — “Identificar soluções de automação” ≈ **PO**; “Gerenciar performance e capacidade” ≈ **DS** (FCC MPE-RN/2010).

### 1.4 SO — processos · threads · deadlock · starvation · memória

| Conceito | Flash |
|----------|-------|
| **Processo** | Programa em execução (espaço de endereçamento) |
| **Thread** | Fluxo dentro do processo (compartilha memória do processo) |
| **Deadlock** | Espera circular por recursos (4 condições clássicas) |
| **Starvation** | Processo **nunca** avança (ex.: prioridade alta sempre “atropela”) |
| Correção starvation típica de prova | Política justa (**FIFO**), aging — **não** LIFO |

**Memória virtual:** processo pode usar mais que a RAM; páginas em disco.  
**Paginação:** blocos de tamanho **fixo** (páginas ↔ frames).  
**Segmentação:** blocos de tamanho **variável** (lógicos).  
Otimizar page fault: **mais RAM** / menos faltas — **não** “desligar memória virtual” como regra geral (FCC TRT-20/2024).

### 1.5 Windows · AD (flash)

| Tema | Flash |
|------|-------|
| Windows 10 | Fim suporte geral **14/10/2025** (ESU = patches pagos) |
| Server | Função × recurso; **Hyper-V** = virtualização / consolidação |
| **AD DS** | Diretório: usuários, computadores, OUs, GPO |
| Controle sobre OU/objetos | **ACLs** (não catálogo global / replicação / índice) |
| Aplicar GPO já | `gpupdate /force` (não `gpresult`) |
| Grupos sensíveis | Domain Admins, Enterprise Admins, Schema Admins… |

Anti-confusão: catálogo global **localiza**; ACL **autoriza**; replicação **propaga**.

### 1.6 IA · ML · LLM (flash)

| Termo | Flash |
|-------|-------|
| **IA** | Campo amplo |
| **ML** | Aprende com dados (supervisionado / não / reforço) |
| **Rede neural / deep learning** | Camadas; pesos |
| **LLM** | Modelo de linguagem em escala (Transformer / atenção) |
| **Alucinação** | Resposta fluente **incorreta** |
| **RLHF** | Ajuste com **feedback humano** (reduz vieses/alucinações — FCC TRT-6/2025) |
| **Fine-tuning** | Ajuste em dados específicos (**sem** remover atenção) |
| **Zero-shot** | Capacidade de tarefa sem treino explícito — **≠** método de alinhamento de segurança |

No Judiciário: governança, privacidade, validação humana — LLM **não** substitui magistrado/analista.

### 1.7 Cartão único “se só lembrar 12 linhas”

```
Scrum = papéis/eventos · XP = TDD/refatoração · Kanban = WIP
ITIL = serviço (Incident≠Problem≠Change≠SLM≠Config)
COBIT = EDM(gov) + APO/BAI/DSS/MEA (40) · ≠ ITIL
Deadlock = impasse circular · Starvation = nunca atende (FIFO)
MV + páginas fixas · ↑RAM ↓page faults
AD: ACL na OU · gpupdate /force
LLM: RLHF alinha · atenção fica · alucinação ≠ fato
```

---

## 2. Questões de treino (1–15)

**1.** Prática XP mais alinhada a qualidade contínua de código:

A) Somente Daily Scrum sem testes  
B) Refatoração contínua e testes automatizados  
C) WIP ilimitado  
D) Cascata com fase única de teste no fim  
E) EDM03 do COBIT  

**2.** Incident × Problem no ITIL:

A) Ambos = causa raiz  
B) Incident restaura rápido; Problem trata causa/recorrência  
C) Problem = SLM  
D) Incident = só Change  
E) São sinônimos de Config  

**3.** Domínio COBIT 2019 de **governança**:

A) DSS  
B) BAI  
C) EDM  
D) APO  
E) MEA apenas como “board”  

**4.** Scrum **não** se caracteriza por:

A) Sprints time-boxed  
B) Inspect & adapt  
C) Ciclo linear sequencial tipo cascata  
D) Product Owner  
E) Definition of Done  

**5.** Starvation × deadlock:

A) São o mesmo  
B) Starvation = preterição contínua; deadlock = espera circular por recursos  
C) Deadlock corrige com LIFO  
D) Starvation = AH/ESP  
E) Ambos = page fault  

**6.** Paginação usa tipicamente blocos de tamanho:

A) Variável como segmentos  
B) Fixo (páginas)  
C) Sempre 1 bit  
D) Só IPv6  
E) Só VLAN  

**7.** Controle sobre UO e objetos no AD é determinado por:

A) Catálogo global apenas  
B) ACLs na UO e nos objetos  
C) Índice de consulta apenas  
D) Catálogo local do cliente Windows  
E) Apenas DHCP option 6  

**8.** Comando para forçar atualização imediata de GPO no cliente:

A) `gpresult /sync`  
B) `gpupdate /force`  
C) `ipconfig /renew`  
D) `net start ntds`  
E) `nslookup`  

**9.** ITIL × COBIT — contraste correto:

A) ITIL governa I&T; COBIT fecha ticket  
B) COBIT governa/gestão I&T; ITIL practices de serviço  
C) São a mesma norma ISO 27001  
D) COBIT = WAF  
E) ITIL = Anexo A 93 controles  

**10.** RLHF em LLM serve principalmente para:

A) Remover mecanismo de atenção  
B) Alinhar respostas com feedback humano (reduzir alucinações/vieses)  
C) Substituir DNS  
D) Eleger Root Bridge  
E) Criar VLAN nativa  

**11.** Kanban — ideia central cobrável:

A) Sprint obrigatória de 4 semanas sem WIP  
B) Limitar WIP e puxar fluxo  
C) Product Goal do Scrum apenas  
D) EDM05  
E) AAAA record  

**12.** Hyper-V, em prova Windows Server, associa-se a:

A) Apenas SMTP submission  
B) Plataforma de virtualização / consolidação de servidores  
C) Substituto do 802.1Q  
D) Encarregado LGPD  
E) Service Desk ITIL  

**13.** Practice ITIL que gerencia informações de CIs (hardware, software, redes…):

A) SLM  
B) Problem  
C) Service Configuration Management  
D) Continual Improvement apenas  
E) Zero-shot learning  

**14.** Alucinação de LLM:

A) Sempre igual a page fault  
B) Resposta fluente factualmente errada/inadequada  
C) Tag 802.1Q  
D) Bridge ID  
E) SoA  

**15.** Síntese do mapa vermelho:

A) Scrum=XP=Kanban; Incident=Problem; COBIT=ITIL; ACL=catálogo global; RLHF remove atenção  
B) Distinguir Scrum/XP/Kanban; ITIL practices; COBIT EDM×gestão; starvation≠deadlock; ACL/gpupdate; RLHF alinha LLM  
C) Desligar memória virtual sempre  
D) Windows 10 sem fim de suporte  
E) COBIT 40 objetivos todos no domínio DSS  

---

### Gabarito comentado (treino 1–15)

1. **B** — XP.  
2. **B** — Incident × Problem.  
3. **C** — EDM.  
4. **C** — Não é cascata.  
5. **B** — Starvation × deadlock.  
6. **B** — Páginas fixas.  
7. **B** — ACLs.  
8. **B** — gpupdate /force.  
9. **B** — COBIT × ITIL.  
10. **B** — RLHF.  
11. **B** — WIP/pull.  
12. **B** — Virtualização.  
13. **C** — Configuration.  
14. **B** — Alucinação.  
15. **B** — Síntese.

---

## 3. Questões de concurso FCC (16–25)

> Questões reais com citação. Gabarito só ao final.

**16.** (FCC · TRT-20/2024 · Técnico Judiciário — TI)  
A equipe de um Tribunal está debatendo a adequação de metodologias ágeis, considerando a natureza dos diversos projetos institucionais. Um Técnico de TI afirmou que a metodologia

A) Scrum é melhor em projetos que requerem a auto-organização da equipe e iterações regulares, permitindo que os desenvolvedores façam ajustes nas prioridades e no escopo do projeto a cada Sprint. Além disso, o Scrum se concentra na implementação de um ciclo de desenvolvimento linear e sequencial, mais próximo da realidade.  
B) XP deve ser escolhida na maior parte dos projetos, pois permite um planejamento detalhado a longo prazo, com entregas frequentes em sprints de 3-4 semanas ou um mês. Além disso, como a colaboração com o cliente é fundamental, essa metodologia incentiva reuniões de revisão mensais com as partes interessadas.  
C) XP é mais indicada quando há alta necessidade de refatoração contínua e testes automatizados, o que permite melhor controle da qualidade do código durante o desenvolvimento.  
D) XP deve ser escolhida quando o time deseja trabalhar sem sprints fixos e priorizar documentações mais detalhadas e extensas. Além disso, projetos com baixa variabilidade de requisitos se beneficiam com as práticas XP.  
E) Scrum deve ser preferida em projetos em que há necessidade de ciclos curtos com entregas de incremento de software a cada semana, garantindo uma integração contínua do produto.

**17.** (FCC · TRT-1/2025 · Analista Judiciário — TI)  
Um Tribunal Regional do Trabalho precisa aprimorar a utilização das práticas da ITIL 4 para melhorar a gestão e a governança de TI, visando garantir a continuidade dos serviços digitais utilizados por magistrados, servidores e jurisdicionados. Para isso, a equipe deve utilizar a prática

A) Gestão Técnica visando coordenar o registro, a categorização e a resolução rápida de falhas nos serviços digitais do Tribunal, garantindo que os usuários sejam atendidos de forma ágil e eficiente.  
B) Gerenciamento da Configuração do Serviço para coletar e gerenciar informações sobre os diversos itens de configuração que compõem os serviços digitais do Tribunal, incluindo hardware, software, redes e fornecedores, visando garantir maior estabilidade e confiabilidade dos sistemas.  
C) Gestão de Eventos para analisar as causas raízes de falhas recorrentes nos serviços digitais do Tribunal, identificando padrões e prevenindo incidentes futuros.  
D) Gestão de Nível de Serviço visando controlar e minimizar os impactos de mudanças nos sistemas do Tribunal, garantindo que todas as alterações sejam avaliadas e aprovadas antes da implementação.  
E) Gestão de Problemas para definir e monitorar acordos entre a TI e os usuários do Tribunal, estabelecendo expectativas sobre qualidade, disponibilidade e desempenho dos serviços digitais.

**18.** (FCC · MPE-RN/2010 · Analista de Tecnologia da Informação)  
No COBIT, “Identificar as soluções de automação” e “Gerenciar a performance e capacidade do ambiente” são processos pertencentes, respectivamente, aos domínios

A) Planejamento e Organização; Entrega e Suporte.  
B) Aquisições e Implantação; Entrega e Suporte.  
C) Planejamento e Organização; Monitoração.  
D) Planejamento e Organização; Aquisições e Implantação.  
E) Monitoração; Entrega e Suporte.

**19.** (FCC · TRT-12/2023 · Analista Judiciário — TI)  
Durante a execução de processos em um sistema operacional, uma Analista observou que processos de maior prioridade apareciam sempre que um processo de menor prioridade ativo queria chamar o kernel. Nesse caso, estava ocorrendo o

A) starvation, que é o mesmo que thread interference, mas uma forma de correção disso é a utilização da ordem de atendimento dos processos escritores por meio de uma fila do tipo último que entra, primeiro que sai (LIFO - last in, first out).  
B) deadlock, que é o mesmo que intervention, pois um processo trava o outro mantendo o impasse. Para evitar isso utiliza-se o thread attempt para análise e correção.  
C) thread lock interference afetando a prioridade do fluxo de execução. Uma possível solução para essa interferência é priorizar as threads utilizando uma fila do tipo último que entra, primeiro que sai (LIFO - last in, first out).  
D) starvation, mas é possível evitá-lo mantendo a ordem de atendimento dos processos escritores utilizando uma fila do tipo primeiro que entra, primeiro que sai (FIFO - first in, first out).  
E) deadlock, que é o mesmo que lock interference, pois um processo trava o outro mantendo o impasse. Para evitar isso utiliza-se o release lock para liberação.

**20.** (FCC · TRT-20/2024 · Analista Judiciário — TI)  
Durante a execução de um programa crítico em um sistema operacional, foi observado que a memória estava sendo usada de forma ineficiente, comprometendo o desempenho da máquina. Esse problema foi tema de discussão e um Analista interveio, esclarecendo que

A) paginação é um método de alocação de memória que divide o processo em segmentos grandes para otimizar o uso da memória. A memória virtual é usada para permitir que os processos utilizem mais CPU, aumentando sua capacidade de processamento. A solução mais comum para melhorar o desempenho é desativar a memória virtual e confiar exclusivamente na RAM, já que o acesso à RAM é mais rápido e elimina a dependência do disco rígido.  
B) segmentação e paginação são técnicas equivalentes de gerenciamento de memória, ambas usadas para dividir a memória em blocos de tamanho variável. A memória virtual depende fortemente do disco rígido, o que provoca quedas significativas no desempenho. Uma solução adequada seria desativar a memória virtual e utilizar apenas a memória RAM para garantir uma execução mais eficiente dos processos.  
C) segmentação é uma técnica de gerenciamento de memória que divide o processo em blocos de tamanho fixo, enquanto a paginação divide o processo em blocos de tamanho variável. A memória virtual é usada para permitir que o sistema operacional execute processos maiores do que a memória física disponível, eliminando a necessidade de trocas entre memória e disco.  
D) memória virtual é uma técnica que permite que processos usem mais memória do que a RAM física disponível, armazenando parte dos dados em disco. A paginação divide a memória em blocos fixos chamados de páginas e mapeia essas páginas para quadros de memória. Uma solução para otimizar o desempenho seria aumentar a quantidade de RAM ou reduzir o número de page faults.  
E) a alocação de memória dinâmica é realizada por meio da técnica de segmentação, que divide a memória em blocos de tamanho fixo para evitar fragmentação interna. A memória virtual é utilizada apenas em sistemas com suporte para segmentação e é sempre mais eficiente do que a memória física.

**21.** (FCC · TRT-14/2022 · Analista Judiciário — TI)  
As Unidades Organizacionais (UOs) no Active Directory podem ser usadas para formar uma hierarquia de contêineres dentro de um domínio. O controle sobre uma UO e os objetos dentro dela determinados

A) pelas listas de controle de acesso na UO e pelos objetos na UO.  
B) pelo mecanismo de consulta e pelo índice.  
C) pelo serviço de replicação que distribui dados de diretório em uma rede.  
D) por um catálogo global que contém informações sobre cada objeto no diretório.  
E) pelo catálogo local de máquina cliente Windows.

**22.** (FCC · TRT-5/2022 · Técnico Judiciário — TI)  
Um técnico deseja que uma política de grupo aplicada ao domínio entre em operação imediatamente em uma máquina com Windows 10, em condições ideais. Para tanto, ele deve utilizar o comando

A) gpresult /sync  
B) gpupdate /sync  
C) gpfixup /force  
D) gpupdate /force  
E) gpresult /f  

**23.** (FCC · TRT-9/2013 · Analista Judiciário — TI)  
O Hyper-V fornece uma plataforma de virtualização dinâmica, confiável e escalonável combinada com um conjunto único de ferramentas de gerenciamento integrado para gerenciar recursos físicos e virtuais. Dentre os recursos oferecidos pelo Hyper-V, encontram-se

A) firewall dinâmico e bloqueio de ataques virtuais.  
B) consolidação de servidores e datacenter dinâmico.  
C) serviço automático B2B e controle de processos ativos.  
D) mapeamento de discos virtuais e interpretador PPPoE.  
E) automatização de VPN e virtualização remota de DNS.

**24.** (FCC · TRT-6/2025 · Analista Judiciário — TI)  
Uma equipe de Analistas está implementando um modelo LLM para uso no Tribunal Regional do Trabalho e precisa garantir que o modelo não gere respostas inapropriadas ou incorretas, mantendo a relevância e coerência do conteúdo. Para ajustar o desempenho do modelo e evitar problemas como alucinações e vieses, eles optaram por

A) reduzir o número de camadas de rede neural para minimizar o processamento e aumentar a velocidade de inferência, pois isso reduziria erros causados por dados não estruturados.  
B) usar o aprendizado zero-shot durante o treinamento, pois ele é suficiente para que o modelo aprenda a generalizar padrões sem supervisão.  
C) utilizar Reinforcement Learning from Human Feedback (RLHF) para treinar o modelo a partir de respostas corrigidas por humanos.  
D) implementar fine-tuning com um conjunto de dados menor e mais específico, sem utilizar nenhum mecanismo de atenção para evitar sobrecarga de processamento.  
E) aumentar o número de tokens e embeddings durante o treinamento para que o modelo tenha mais dados contextuais disponíveis.

**25.** (FCC · TRT-23/2016 · Técnico Judiciário — TI)  
Um Técnico do Tribunal executou o comando gpupdate no prompt de comandos do Windows 2008 Server com o objetivo de

A) identificar a instalação de algum driver gráfico.  
B) atualizar as configurações de diretiva de grupo.  
C) instalar uma nova versão do gerenciador de processos.  
D) listar os pacotes de software instalados no sistema.  
E) atualizar a lista de drivers de vídeo.

---

### Gabarito comentado (FCC 16–25)

| Q | Gab. | Nota |
|---|------|------|
| 16 | **C** | TRT-20/2024 — XP = refatoração + testes automatizados |
| 17 | **B** | TRT-1/2025 — Service Configuration (papéis trocados nas outras) |
| 18 | **A** | MPE-RN/2010 — PO; DS (legado COBIT) |
| 19 | **D** | TRT-12/2023 — starvation + FIFO |
| 20 | **D** | TRT-20/2024 — MV + paginação + ↑RAM/↓faults |
| 21 | **A** | TRT-14/2022 — ACL na UO |
| 22 | **D** | TRT-5/2022 — gpupdate /force |
| 23 | **B** | TRT-9/2013 — consolidação / datacenter dinâmico |
| 24 | **C** | TRT-6/2025 — RLHF |
| 25 | **B** | TRT-23/2016 — gpupdate = diretiva de grupo |

Distribuição concurso: C, B, A, D, D, A, D, B, C, B.

---

*Arquivo 05 · Mapa temas vermelhos · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 11.*

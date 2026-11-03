+++
title = '04 — Mapa ITIL 4 × COBIT 2019 · Analista TI (código 14)'
date = '2026-10-30'
draft = false
weight = 4
tags = ['semana-12', 'mapa']
categories = ['estudo']
+++
# Mapa ITIL 4 × COBIT 2019 · Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — Semana 12 = revisão ativa · contraste cobrável  
**Escopo deste arquivo:** **mapa** ITIL 4 (base do edital ITIL Version 5) × **COBIT 2019** — papéis, peças quentes e anti-fusão. Formato contraste, **não** tutorial longo.  
**Fora de escopo aqui:** decorar as 34 practices / 40 objetivos nome a nome; PMBOK página a página; Modelo TRT4/ENTIC (arquivo `05`); Gerais (`06`).

**Priors:** `/cursor/stores/self/docs/semana-4/07-itil-5.md` · `/cursor/stores/self/docs/semana-4/08-cobit-2019.md` · flash ITIL×COBIT em `/cursor/stores/self/docs/semana-11/05-mapa-temas-vermelhos.md`.

---

## 1. Material — mapa de contrastes

### 1.1 Uma pergunta cada (ouro FCC)

| | **ITIL** (edital: Version 5 / base 4) | **COBIT 2019** (ISACA) |
|---|--------------------------------------|-------------------------|
| Pergunta | Como **entregar/operar** serviços (e produtos digitais) com **valor**? | I&T está **alinhada, dirigida e monitorada** aos objetivos do órgão? |
| Natureza | Biblioteca de **boas práticas** de ITSM | Framework **guarda-chuva** de governança **e** gestão de I&T |
| Não é | Norma ISO de SGSI · substituto do COBIT | Manual do Service Desk · sinônimo de ITIL |
| No tribunal | Catálogo, incidentes, mudanças, SLA, desk | Comitês, PDTIC, risco/benefício/recursos, assurance |

Flash: **complementares**. TRT4 usa **ambos** no modelo local — não “escolhe um”.

### 1.2 Peças quentes lado a lado

| ITIL — o que a FCC troca | COBIT — o que a FCC troca |
|--------------------------|---------------------------|
| **Incident** = restaurar **agora** | **EDM** = única **governança** (Evaluate, Direct, Monitor) |
| **Problem** = causa / recorrência | **APO** Align·Plan·Organize (14) |
| **Change** = alteração controlada | **BAI** Build·Acquire·Implement (11) |
| **Service request** = pedido padrão | **DSS** Deliver·Service·Support (6) ≈ “cheira” a operação |
| **SLM** = acordos de nível | **MEA** Monitor·Evaluate·Assess (4) = desempenho/conformidade |
| **Service Desk** = ponto de contato | Total **5+14+11+6+4 = 40** |
| **Configuration** = CIs (HW/SW/rede…) | **Design factors** = customizar (não one-size) |
| 7 **guiding principles** | Componentes: processos, estruturas, políticas, info, cultura, pessoas, serviços/infra |

```
Incidente ≠ Problema ≠ Requisição ≠ Mudança ≠ SLM ≠ Config
EDM ≠ APO/BAI/DSS/MEA
ITIL ≠ COBIT ≠ PMBOK ≠ ISO 27001
```

### 1.3 ITIL — cartão mínimo (v4 herança / v5 edital)

| Tema | Âncora |
|------|--------|
| Valor | Utility (fit for purpose) + Warranty (fit for use) → valor percebido |
| 7 princípios | Focus on value · Start where you are · Progress iteratively · Collaborate & visibility · Holistic · Keep simple · **Optimize and automate** |
| 4 dimensões | Orgs/people · Info/tech · Partners/suppliers · Value streams/processes |
| SVS (v4) | Componentes + atividades juntos para **criar valor** |
| Value chain (v4) | 6 atividades (Plan… Deliver & support) |
| Lifecycle (v5 edital) | 8: Discover→Design→Acquire→Build→Transition→Operate→Deliver→Support |

Armadilhas: automatizar bagunça; Service Desk “faz causa raiz”; ITIL = ISO.

### 1.4 COBIT — cartão mínimo

| Tema | Âncora |
|------|--------|
| Governança | **EDM** — board avalia/dirige/monitora |
| Gestão | APO · BAI · DSS · MEA |
| Cascata | Stakeholder needs → enterprise goals → alignment goals → objetivos COBIT |
| Design factors | Estratégia, risco, compliance, sourcing, tamanho… → **priorizar** |
| Performance | Capacidade de processo (0–5) · maturidade por **área de foco** |

Armadilhas: EDM = gestão; COBIT “só auditoria”; 40 objetivos com peso idêntico; COBIT = ITIL.

### 1.5 Tabela anti-fusão (prova)

| Afirmação FCC típica | Veredito |
|----------------------|----------|
| “COBIT fecha o ticket de senha” | ❌ (ITIL / service desk) |
| “ITIL define EDM/APO/BAI…” | ❌ (COBIT) |
| “DSS ≈ mundo operacional; ITIL practices dialogam” | ✅ |
| “PMBOK substitui COBIT/ITIL no tribunal” | ❌ (projetos ≠ gov/serviço) |
| “Modelo TRT4 usa COBIT + ITIL + BPMN” | ✅ (ver arquivo 05) |

### 1.6 Cartão único “se só lembrar 10 linhas”

```
ITIL = serviço/valor/practices (Incident≠Problem≠Change≠SLM≠Config)
COBIT = EDM(gov) + APO/BAI/DSS/MEA (40) + design factors
DSS “cheira” ITIL · EDM = board · MEA = checagem
Complementares no TRT4 · ≠ PMBOK · ≠ ISO 27001
Optimize THEN automate · Start where you are
```

---

## 2. Questões de treino (1–15)

**1.** Contraste correto ITIL × COBIT:

A) ITIL governa I&T; COBIT fecha ticket  
B) COBIT governa/gestão de I&T; ITIL practices de serviço/produto  
C) São a mesma norma ISO 27001  
D) COBIT = WAF  
E) ITIL = Anexo A de 93 controles  

**2.** Domínio COBIT 2019 exclusivo de **governança**:

A) DSS  
B) BAI  
C) EDM  
D) APO  
E) MEA como “board”  

**3.** Incident × Problem:

A) Ambos = causa raiz  
B) Incident restaura rápido; Problem trata causa/recorrência  
C) Problem = SLM  
D) Incident = só Change  
E) Sinônimos de Config  

**4.** Soma correta dos objetivos COBIT 2019:

A) 5  
B) 20  
C) 34  
D) 40  
E) 37  

**5.** Practice ITIL que gerencia informações de CIs:

A) SLM  
B) Problem  
C) Service Configuration Management  
D) Continual Improvement apenas  
E) EDM03  

**6.** “Optimize and automate” orienta a:

A) Automatizar bagunça primeiro  
B) Otimizar o que faz sentido e então automatizar  
C) Proibir automação  
D) Ignorar valor  
E) Substituir o COBIT  

**7.** Domínio COBIT que “cheira” a operação/suporte (perto do ITIL):

A) EDM  
B) DSS  
C) Só PMBOK  
D) Apenas BAI  
E) Conduta ética  

**8.** Design factors no COBIT 2019 servem para:

A) Aplicar os 40 com peso idêntico sempre  
B) Adaptar/priorizar o sistema de governança ao contexto  
C) Extinguir o ITIL  
D) Definir crase  
E) Criar o PDPJ  

**9.** Propósito mental do SVS (ITIL 4):

A) Só listar senhas Wi-Fi  
B) Componentes e atividades trabalhando juntos para criar valor  
C) Substituir a CF  
D) Certificar ISO 27001  
E) Eliminar Service Desk  

**10.** Assinale a **inadequada**:

A) ITIL e COBIT são complementares  
B) EDM = Evaluate, Direct, Monitor  
C) ITIL = sinônimo exato de COBIT 2019  
D) Incident ≠ Problem  
E) MEA monitora desempenho/conformidade  

**11.** Quatro dimensões ITIL — par que completa Orgs/people e Info/tech:

A) DNS e DHCP  
B) Fluxos de valor/processos; Parceiros e fornecedores  
C) EDM e APO  
D) Scrum e Kanban  
E) WEP e WPA  

**12.** APO01 (estrutura de gestão de I&T), no mapa mental, fica em:

A) Domínio de **gestão** APO  
B) Único domínio de governança EDM  
C) Só ITIL Service Desk  
D) Só PMBOK  
E) Conduta TRT4  

**13.** Requisição de serviço típica (ITIL):

A) Sempre causa raiz  
B) Pedido padrão (ex.: reset de senha, acesso, informação)  
C) Sinônimo de mudança emergencial sem registro  
D) Objetivo EDM05  
E) Extinção do catálogo  

**14.** MEA foca tipicamente em:

A) Só Discover do lifecycle  
B) Monitorar/avaliar desempenho e conformidade (gestão)  
C) Abrir ticket de toner  
D) Substituir ENTIC  
E) Definir cabeamento  

**15.** Síntese do mapa:

A) ITIL=serviço/practices · COBIT=EDM+gestão(40) · complementares · ≠PMBOK/ISO27001  
B) ITIL=COBIT=PMBOK  
C) EDM fecha ticket  
D) Config = SLM  
E) Design factors = proibir adaptação  

---

### Gabarito comentado (treino 1–15)

1. **B** — COBIT gov/gestão × ITIL serviços.  
2. **C** — EDM.  
3. **B** — restaurar × causa.  
4. **D** — 40.  
5. **C** — Configuration / CIs.  
6. **B** — otimizar depois automatizar.  
7. **B** — DSS.  
8. **B** — customizar.  
9. **B** — valor via SVS.  
10. **C** — inadequada (≠ sinônimo).  
11. **B** — 4 dimensões.  
12. **A** — APO = gestão.  
13. **B** — request padrão.  
14. **B** — MEA.  
15. **A** — cartão do mapa.

---

## 3. Questões de concurso (16–25)

> Questões **reais** com citação. Gabarito só no final. **Não inventar.**

**16.** (FGV · TCE-GO/2024 · Analista de Controle Externo — TI)  
Assinale a opção que descreve corretamente um ponto principal característico de COBIT 2019, ITIL v4 e PMBOK.

A) COBIT 2019 é centrado na governança de TI, ITIL v4 foca na gestão de serviços de TI e PMBOK orienta sobre a gestão de projetos tradicionais.  
B) COBIT 2019 é um guia para auditoria de TI, ITIL v4 é um framework para desenvolvimento de software, e PMBOK é uma metodologia ágil.  
C) COBIT 2019, ITIL v4 e PMBOK são frameworks que priorizam a implementação de práticas ágeis e DevOps em suas abordagens.  
D) COBIT 2019, ITIL v4 e PMBOK são estruturas voltadas exclusivamente para o controle financeiro de projetos de TI.  
E) COBIT 2019 define princípios de governança corporativa, ITIL v4 prioriza a gestão de serviços de TI focada no cliente, e PMBOK foca na entrega de valor através da gestão de projetos.

**17.** (FCC · TRT-1/2025 · Analista Judiciário — TI)  
Um Tribunal Regional do Trabalho precisa aprimorar a utilização das práticas da ITIL 4 para melhorar a gestão e a governança de TI, visando garantir a continuidade dos serviços digitais utilizados por magistrados, servidores e jurisdicionados. Para isso, a equipe deve utilizar a prática

A) Gestão Técnica visando coordenar o registro, a categorização e a resolução rápida de falhas nos serviços digitais do Tribunal, garantindo que os usuários sejam atendidos de forma ágil e eficiente.  
B) Gerenciamento da Configuração do Serviço para coletar e gerenciar informações sobre os diversos itens de configuração que compõem os serviços digitais do Tribunal, incluindo hardware, software, redes e fornecedores, visando garantir maior estabilidade e confiabilidade dos sistemas.  
C) Gestão de Eventos para analisar as causas raízes de falhas recorrentes nos serviços digitais do Tribunal, identificando padrões e prevenindo incidentes futuros.  
D) Gestão de Nível de Serviço visando controlar e minimizar os impactos de mudanças nos sistemas do Tribunal, garantindo que todas as alterações sejam avaliadas e aprovadas antes da implementação.  
E) Gestão de Problemas para definir e monitorar acordos entre a TI e os usuários do Tribunal, estabelecendo expectativas sobre qualidade, disponibilidade e desempenho dos serviços digitais.

**18.** (FCC · TRT-19/2022 · Analista Judiciário — TI)  
Com respeito aos Princípios para um Sistema de Governança estabelecidos no Cobit 2019,

A) um Sistema de Governança deve ser adaptado às necessidades dos clientes, com a utilização de um conjunto de fatores de desempenho como parâmetros para personalizar e priorizar os componentes do Sistema de Governança.  
B) toda organização precisa de um método sistemático para satisfazer as necessidades da área de suporte ao cliente e gerar valor a partir do uso da TI. O valor reflete um equilíbrio entre produtos e entregas.  
C) um Sistema de Governança Corporativa de TI é construído a partir de uma série de componentes que podem ser de diferentes tipos e que trabalham juntos de uma forma holística.  
D) cada organização precisa de um controle de gestão para satisfazer as necessidades do cliente e gerar valor a partir do uso da TI. O valor reflete um equilíbrio entre cronograma, escopo e custo.  
E) um Sistema de Governança deve ser estático e estável. Isso significa que cada vez que um ou mais dos fatores de desenho são alterados (por exemplo, uma mudança de estratégia ou tecnologia), o impacto dessas mudanças no Sistema de Governança Corporativa não deve afetar o modelo estabelecido.

**19.** (FCC · TRT-20/2024 · Analista Judiciário — TI)  
São um conjunto de princípios ITIL V4 que orientam uma organização em todo o seu ciclo de vida de gestão de serviços, independentemente das mudanças que ocorram nos objetivos, estratégias ou na estrutura da organização. Um, dentre os sete princípios orientadores, é

A) manter uma abordagem inflexível para todas as mudanças.  
B) evitar a comunicação com os stakeholders.  
C) minimizar a documentação do serviço.  
D) colaborar e promover visibilidade.  
E) focar exclusivamente nos resultados financeiros.

**20.** (FCC · TRT-17/2022 · Analista Judiciário — TI)  
Considere, por hipótese, que uma equipe de analistas de um TRT está trabalhando na implantação do sistema de governança com base no COBIT 2019. Os analistas listaram, dentre outras, as seguintes situações:  
I. Ocorre frustração entre diferentes setores no TRT devido a uma percepção de baixa contribuição da TI para o valor da instituição.  
II. Há constatações negativas advindas de auditorias e relatórios de avaliação sobre o desempenho da TI ou sobre a qualidade questionável de serviços entregues no TRT.  
III. É recorrente a ocorrência de duplicações ou sobreposições de iniciativas da equipe de TI ou outras formas de desperdício de recursos, que contribuem para o alto custo do setor.  
Essas situações estão relacionadas ao Design Factor denominado

A) I&T risk profile categories.  
B) Threat landscape.  
C) IT implementation methods.  
D) IT-related problem analysis.  
E) I&T-related issues.

**21.** (FCC · TRT-5/2013 · Analista Judiciário — TI)  
Atenção: As questões de números 57 a 60 devem ser respondidas considerando-se as atualizações realizadas na ITIL v3 em 2011. Considere:  
I. Restaurar a operação normal do serviço o mais rápido possível.  
II. Minimizar o impacto adverso nas operações do negócio.  
III. Monitorar o serviço de TI e detectar quando o desempenho cai abaixo dos limites aceitáveis.  
IV. Encontrar a causa raiz do problema e aplicar uma solução definitiva.  
São objetivos do processo de Gerenciamento de Incidentes da ITIL v3 o que consta APENAS em

A) III e IV.  
B) II e III.  
C) I e II.  
D) I e IV.  
E) I, II e IV.

**22.** (FCC · TRT-20/2024 · Técnico Judiciário — TI)  
No ITIL V4, o principal propósito do Service Value System – SVS é

A) definir as funções e responsabilidades de todos os membros da equipe, fator implementado pela gestão de comunicação.  
B) prover uma estrutura para garantir que todos os componentes e atividades de uma organização trabalhem juntos para facilitar a criação de valor.  
C) focar exclusivamente na automação de serviço visando compatibilidade com a missão, a visão e os valores estratégicos da organização.  
D) implementar novas tecnologias de forma rápida e eficiente para garantir que o produto entregue aos stakeholders seja o mais atual possível.  
E) garantir que os serviços sejam sempre disponibilizados o mais cedo possível.

**23.** (FCC · TCE-GO/2022 · Analista de Controle Externo — TI)  
Na ITIL v4, as quatro dimensões especificadas são: Informação e tecnologia; Organizações e pessoas;

A) Infraestrutura e suporte; Gestão de clientes.  
B) Fluxos de valor e processos; Suporte técnico e serviços.  
C) Infraestrutura e suporte; Atendimento a clientes.  
D) Fluxos de valor e processos; Parceiros e fornecedores.  
E) Suporte técnico e serviços; Parceiros e fornecedores.

**24.** (FGV · SEFAZ-AM/2022 · Analista de Tecnologia da Informação da Fazenda Estadual)  
O COBIT®2019 descreve um modelo corporativo para a governança e gestão de TI. Segundo este modelo de referência, os objetivos de governança estão agrupados no domínio

A) avaliar, dirigir e monitorar.  
B) alinhar, planejar e organizar.  
C) construir, adquirir e implementar.  
D) entregar, serviços e suporte.  
E) implementar, testar e corrigir.

**25.** (FCC · TCE-GO/2022 · Analista de Controle Externo — TI)  
Os objetivos de gestão do COBIT 2019 estão agrupados em domínios. O domínio que tem foco no monitoramento do desempenho e na conformidade da TI com objetivos de controles internos e requisitos externos é o

A) Monitor, Plan and Organize (MPO).  
B) Plan, Do and Act (PDC).  
C) Align, Plan and Organize (APO).  
D) Performance, Evaluation and Analyze (PEA).  
E) Monitor, Evaluate and Assess (MEA).

---

### Gabarito comentado (concurso 16–25)

| Q | Gab. | Fonte / chave |
|---|------|---------------|
| 16 | **A** | FGV TCE-GO/2024 — COBIT gov · ITIL serviços · PMBOK projetos |
| 17 | **B** | FCC TRT-1/2025 — Configuration (papéis trocados nas demais) |
| 18 | **C** | FCC TRT-19/2022 — componentes holísticos |
| 19 | **D** | FCC TRT-20/2024 — collaborate and promote visibility |
| 20 | **E** | FCC TRT-17/2022 — Design Factor I&T-related issues |
| 21 | **C** | FCC TRT-5/2013 — Incident = I e II (restaurar + minimizar impacto) |
| 22 | **B** | FCC TRT-20/2024 Téc. — SVS = valor |
| 23 | **D** | FCC TCE-GO/2022 — 4 dimensões |
| 24 | **A** | FGV SEFAZ-AM/2022 — EDM = governança |
| 25 | **E** | FCC TCE-GO/2022 — MEA |

Distribuição concurso: A, B, C, D, E, C, B, D, A, E.

---

## 4. Como usar

1. 10 min no cartão 1.6 + tabela 1.5.  
2. Treino 1–15 → gabarito.  
3. Concurso 16–25 em modo prova.  
4. Cruzar com `05-mapa-governanca-tic-trt4.md` (encaixe local).  
5. Erros → caderno (Incident×Problem; EDM×DSS; ITIL≠COBIT).

---

*Arquivo 04 · Mapa ITIL × COBIT · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 12.*

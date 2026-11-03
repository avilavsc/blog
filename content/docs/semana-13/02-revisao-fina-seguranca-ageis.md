+++
title = '02 — Revisão fina — Segurança + Ágeis'
date = '2026-10-29'
draft = false
weight = 2
tags = ['semana-13', 'revisao']
categories = ['estudo']
+++
# Revisão fina — Segurança + Ágeis · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Semana 13 = pré-prova (18/01–24/01)  
**Escopo deste arquivo:** flash **fino** — pegadinhas/contrastes ISO·LGPD·OWASP·FW/WAF/IDS/IPS/VPN + Scrum/Kanban/XP. Sem matéria nova.  
**Fora de escopo aqui:** PDPJ/SQL (`01`); ITIL/governança (`03`); redes L2/L3 detalhadas.

**Priors:** `docs/semana-11/04` · `docs/semana-12/02` · `docs/semana-8/` · `docs/semana-4/01`.  
**Modo:** cartões → treino → REAL → gabarito no final.

---

## 1. Material (revisão fina — pegadinhas)

### 1.1 ISO × LGPD (anti-fusão)

| Norma | Flash |
|-------|-------|
| **27001** | Requisitos do **SGSI** (certificável) · política = **Alta Direção** |
| **27002** | Orientação / controles (não certifica sozinha) |
| **27701** | **PIMS** / privacidade — **≠** LGPD |
| Anexo A 2022 | **93** controles · **4** temas · **SoA** |
| PDCA SGSI | Estabelecer → Implementar/Operar → Monitorar → Manter/Melhorar |

| Figura LGPD | Papel |
|-------------|-------|
| **Controlador** | Decide finalidades/meios |
| **Operador** | Trata em nome do controlador |
| **Encarregado** | Canal controlador ↔ titulares ↔ **ANPD** |

Agentes de tratamento = Controlador + Operador. Multa: até **2%** / **R$ 50 mi** por infração.

### 1.2 Controles de rede/app (ouro)

| Controle | FCC quer |
|----------|----------|
| **Firewall** | IP/porta/estado |
| **WAF** | HTTP: SQLi, XSS, session… |
| **UTM** | Caixa única (FW+AV+IDS/IPS…) |
| **IDS** | Detecta + **alerta** |
| **IPS** | Detecta + **bloqueia** |
| **DMZ** | **Zona** (não appliance) |
| **NAC** | Quem entra na rede |
| **SIEM** | Correlaciona **logs** |

VPN: **IPsec** = transporte + túnel · **AH** + **ESP**. VPN-SSL tipicamente via **navegador HTTPS** · cifra SSL/**TLS**.  
Hash = **integridade**; assinar com **privada** / verificar com **pública**.  
**ENSEC-PJ** = Res. CNJ **396**/2021 (ampla SI + proteção de ativos). Portaria **162** operacionaliza — ≠ 400/443.

### 1.3 Ágeis — donos dos conceitos

| | Scrum | Kanban | XP |
|---|-------|--------|-----|
| Âncora | Papéis · Sprint · eventos · artefatos | Fluxo · **WIP** · pull · visual | Pair · TDD · CI · refatoração |
| Não é | TDD obrigatório | Sprint/papéis obrigatórios | Framework de gestão do produto |

**Scrum (ouro):**

| | Flash |
|---|-------|
| **PO** | Valor + Product Backlog |
| **SM** | Facilita · impede · coach Scrum (**≠** dono do backlog) |
| **Developers** | Increment Done |
| **Sprint** | Time-box ≤ 1 mês · coração do Scrum |
| **Daily** | 15 min · sincronizar próximas 24 h |
| **Review** | Inspecionar **incremento** / produto |
| **Retro** | Inspecionar **processo**/equipe |
| **DoD** | Padrão de “Pronto” do Increment |

Manifesto: valoriza mais software funcionando / indivíduos / colaboração / responder a mudanças — **não anula** o lado direito.

### 1.4 Cartão único (2 min)

```
27001=SGSI · 27002=orientação · 27701≠LGPD · Alta Direção
Controlador+Operador · Encarregado=canal · 2%/R$50mi
FW≠WAF≠IDS≠IPS≠DMZ≠NAC≠SIEM · VPN-SSL=HTTPS · ENSEC=396
PO=backlog · SM=facilita · Daily=15 · Review=produto · Retro=processo
Kanban=WIP/fluxo · XP=pair/TDD/refactor · Scrum+XP podem híbridos
```

---

## 2. Questões de treino (1–15)

**1.** Quem estabelece a política de SI adequada ao propósito da organização (27001)?

A) Só a área de TI  
B) Área de Qualidade  
C) Alta Direção  
D) Só o CIO  
E) Gerência de Projetos  

**2.** 27001 × 27002 × 27701:

A) 27002 certifica o SGSI sozinha  
B) 27701 = LGPD artigo a artigo  
C) 27001 = requisitos SGSI; 27002 = orientação; 27701 = PIMS ≠ LGPD  
D) Anexo A 2022 = 114 controles / 14 domínios  
E) SoA é opcional e sem justificativa  

**3.** Agentes de tratamento na LGPD:

A) Só o Encarregado  
B) Controlador e Operador (Encarregado = canal)  
C) Controlador e Encarregado como únicos agentes  
D) Operador e ANPD  
E) Titular e Encarregado  

**4.** Multa simples LGPD (teto típico cobrado):

A) 4% e R$ 100 mi  
B) 3% e R$ 60 mi  
C) 2% e R$ 50 mi  
D) 2% e R$ 100 mi  
E) 4% e R$ 50 mi  

**5.** IDS × IPS:

A) IDS bloqueia; IPS só alerta  
B) São intercambiáveis  
C) IDS monitora/alerta; IPS bloqueia ativamente  
D) WAF substitui ambos sempre  
E) NAC = IPS  

**6.** Solução típica contra SQLi/XSS/session hijack em app web:

A) NAC  
B) DMZ (como appliance)  
C) WAF  
D) Só sniffer  
E) STP  

**7.** VPN-SSL — leitura cobrável:

A) Exige sempre cliente dedicado desktop  
B) Incompatível com mobile  
C) Pode ser acessada via navegador com HTTPS  
D) Usa só OSPF  
E) É sinônimo de AH  

**8.** ENSEC-PJ (contexto Res. 396):

A) = CNJ 400  
B) = MoReq  
C) Estratégia ampla de SI + proteção de ativos de TI  
D) Só Portaria 162 sem 396  
E) Substitui LGPD  

**9.** Product Owner:

A) Dono do Sprint Backlog técnico exclusivo  
B) Maximiza valor e gerencia Product Backlog  
C) Substitui Developers  
D) É o gerente de RH do time  
E) Faz TDD obrigatório  

**10.** Daily Scrum:

A) 60 min para definir Product Backlog  
B) Time-box de 15 min para sincronizar e planejar as próximas 24 h  
C) Review do incremento com stakeholders  
D) Retro de processo  
E) Sprint Planning completo  

**11.** Review × Retrospective:

A) Ambos = só causa raiz ITIL  
B) Review = produto/incremento; Retro = processo/equipe  
C) Retro = priorizar backlog do PO  
D) Review = 15 min em pé  
E) Sinônimos  

**12.** WIP no Kanban:

A) Obriga Sprint de 1 mês  
B) Limita trabalho em andamento e melhora fluxo  
C) É papel do Scrum Master  
D) É Definition of Done  
E) Substitui Pair Programming  

**13.** Prática típica de XP (não do Scrum puro):

A) Product Owner  
B) Sprint Review  
C) Programação em pares / TDD / refatoração contínua  
D) Daily de 15 min como única prática  
E) EDM do COBIT  

**14.** Scrum + XP:

A) Nunca podem coexistir  
B) Podem híbridos: gestão Scrum + práticas técnicas XP  
C) XP gerencia o Product Backlog no lugar do PO  
D) Exigem PMBOK como intermediário obrigatório  
E) XP foca gestão; Scrum foca só código  

**15.** Síntese correta:

A) IDS=IPS; 27701=LGPD; SM=dono do backlog; Kanban exige Sprint  
B) Alta Direção·2%/50mi·IPS bloqueia·WAF web·PO backlog·Daily 15·XP pair/TDD  
C) ENSEC=400; VPN-SSL exige sempre IPsec AH  
D) Multa 4%/100 mi; Implicit Grant = Keycloak  
E) DoD = métrica exclusiva Kanban  

---

### Gabarito comentado (treino 1–15)

1. **C** — Alta Direção.  
2. **C** — contraste ISO.  
3. **B** — agentes + canal.  
4. **C** — 2% / R$ 50 mi.  
5. **C** — alerta × bloqueia.  
6. **C** — WAF.  
7. **C** — HTTPS/browser.  
8. **C** — 396 ampla.  
9. **B** — PO/valor/backlog.  
10. **B** — Daily 15 min.  
11. **B** — Review×Retro.  
12. **B** — WIP.  
13. **C** — XP.  
14. **B** — híbrido.  
15. **B** — cartão.

Letras treino: C, C, B, C, C, C, C, C, B, B, B, B, C, B, B.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** com banca/órgão/ano. **Sem invenção.** Gabarito só no final.

**16.** (FCC · TRT-19/2022 · Analista Judiciário — TI)  
Segundo a norma ABNT NBR ISO/IEC 27001:2013, quem deve estabelecer uma política de segurança da informação que seja apropriada ao propósito da organização é:

A) Área de Tecnologia da Informação.  
B) Área de Qualidade.  
C) Alta Direção.  
D) Gerência de Projetos.  
E) Chief Information Officer (CIO).  

**17.** (FCC · TRT-5/2013 · Analista Judiciário — TI)  
A Norma NBR ISO/IEC 27001 adota o modelo Plan-Do-Check-Act (PDCA) aplicado aos processos do SGSI. A sequência correta é

A) Implementação e Operação → Monitoramento e Análise Crítica → Estabelecimento → Manutenção e Melhoria  
B) Estabelecimento → Manutenção e Melhoria → Monitoramento e Análise Crítica → Implementação e Operação  
C) Monitoramento e Análise Crítica → Implementação e Operação → Estabelecimento → Manutenção e Melhoria  
D) Estabelecimento → Implementação e Operação → Monitoramento e Análise Crítica → Manutenção e Melhoria  
E) Implementação e Operação → Monitoramento e Análise Crítica → Manutenção e Melhoria → Estabelecimento  

**18.** (FCC · TRF4/2025 · Analista — Análise de Sistemas)  
No âmbito da LGPD (Lei Geral de Proteção de Dados), existem as figuras de Controlador, Operador e Encarregado. O papel de tais figuras e a relação entre elas no tratamento de dados pessoais e a Autoridade Nacional de Proteção de Dados (ANPD), de acordo com LGPD, podem ser realizados por dois agentes de tratamento, o

A) Controlador e o Operador, enquanto o Encarregado é a pessoa indicada pelo controlador e operador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
B) Controlador e o Encarregado, enquanto o Operador é a pessoa indicada pelo encarregado e controlador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
C) Encarregado e o Operador, enquanto o controlador é a pessoa indicada pelo Encarregado e operador para atuar como canal de comunicação entre o operador, os titulares dos dados e a ANPD.  
D) Controlador e o Operador, enquanto o Encarregado é a pessoa indicada pelo titular e operador para atuar como canal de comunicação entre o controlador, os titulares dos dados e a ANPD.  
E) Controlador e o Encarregado, enquanto o Encarregado é a pessoa indicada pelo controlador e titulares para atuar como canal de comunicação entre o controlador, o encarregado dos dados e a ANPD.  

**19.** (FCC · TJ-CE/2022 · Analista — SI)  
Considere o texto abaixo, adaptado da Lei Geral de Proteção de Dados (LGPD). "Os agentes de tratamento de dados, em razão das infrações cometidas às normas previstas nesta Lei, ficam sujeitos às sanções administrativas aplicáveis pela autoridade nacional, dentre elas, multa simples sobre o faturamento da pessoa jurídica de direito privado, grupo ou conglomerado no Brasil no seu último exercício, excluídos os tributos, limitada, no total, a um determinado valor em reais por infração ." (grifos nossos). O percentual de multa simples e o valor em reais por infração, referidos no texto, são, correta e respectivamente, de até

A) 3% e R$ 60.000.000,00.  
B) 4% e R$ 50.000.000,00.  
C) 4% e R$ 100.000.000,00.  
D) 2% e R$ 50.000.000,00.  
E) 2% e R$ 100.000.000,00.  

**20.** (FCC · TRT-1/2025 · Analista — TI)  
Um Tribunal Regional do Trabalho precisa reforçar a segurança da informação em seus sistemas, garantindo a proteção dos dados processuais e a continuidade dos serviços digitais. A equipe de TI promoveu um debate sobre a utilização de um Sistema de Prevenção de Intrusão (IPS) comparando-o a outras soluções. Eles concluíram corretamente que o

A) Firewall de borda substitui a necessidade de um IPS, pois sua função é suficiente para evitar todos os tipos de ataques e filtrar o tráfego malicioso antes que ele chegue aos servidores internos.  
B) Sniffer de Rede é uma ferramenta mais eficaz do que um IPS, pois permite capturar e analisar pacotes de dados sem causar impacto na latência da rede do Tribunal.  
C) IPS analisa o tráfego de rede em tempo real, identificando padrões suspeitos e bloqueando automaticamente atividades maliciosas antes que comprometam os sistemas do Tribunal, não sendo equivalente a um IDS, que apenas alerta sobre ameaças sem intervir ativamente.  
D) WAF (Web Application Firewall) analisa registros de eventos e detecta anomalias de segurança em tempo real, além de bloquear ataques automaticamente, sendo uma alternativa ao IPS.  
E) NAC (Network Access Control) é uma solução equivalente ao IPS, pois impede qualquer acesso não autorizado à rede do Tribunal, monitorando continuamente os dispositivos conectados.  

**21.** (FCC – TJ-CE/2022 – Analista Judiciário – Ciência da Computação / Sistemas da Informação)  
Quanto à metodologia Scrum, considere as definições a seguir:  
I. Time-boxed de um mês ou menos, durante o qual um “Pronto”, versão incremental potencialmente utilizável do produto, é criada.  
II. Responsável por garantir que o Scrum seja entendido e aplicado, para garantir que o Time Scrum adira à teoria, práticas e regras do Scrum.  
III. Responsável por maximizar o valor do produto e do trabalho do Time de Desenvolvimento.  
IV. Um conjunto de itens selecionados, juntamente com o plano para entregar o incremento do produto e atingir o objetivo. Previsão do Time de Desenvolvimento sobre qual funcionalidade estará no próximo incremento e sobre o trabalho necessário para entregar essa funcionalidade em um incremento “Pronto”.  
Os itens I, II, III e IV correspondem, correta e respectivamente, às definições de

A) Sprint, Scrum Master, Product Owner e Sprint Backlog.  
B) Product Sprint, Product Owner, Scrum Master e Product Backlog.  
C) Sprint, Senior Scrum, Product Owner e Product Backlog.  
D) Backlog Review, Product Master, Scrum Owner e Sprint Backlog.  
E) Product Backlog, Sprint Master, Scrum Master e Increment.  

**22.** (FCC – TRF-3/2019 – Analista Judiciário – Informática)  
A Reunião Diária do Scrum é

A) executada no final da Sprint para inspecionar o incremento e adaptar o Backlog do Produto, se necessário.  
B) um time-boxed de 15 minutos, durante o qual um “Pronto”, versão incremental potencialmente utilizável do produto, é criado.  
C) uma oportunidade para o Time Scrum inspecionar a si próprio e criar um plano para melhorias a serem aplicadas na próxima Sprint.  
D) um time-boxed de 15 minutos, para que o Time de Desenvolvimento possa sincronizar as atividades e criar um plano para as próximas 24 horas.  
E) um time-boxed de 60 minutos, durante o qual os produtos de uma Sprint são definidos.  

**23.** (FCC – TRT-20/2024 – Técnico Judiciário – Tecnologia da Informação)  
A equipe de um Tribunal está debatendo a adequação de metodologias ágeis, considerando a natureza dos diversos projetos institucionais. Um Técnico de TI afirmou que a metodologia

A) Scrum é melhor em projetos que requerem a auto-organização da equipe e iterações regulares, permitindo que os desenvolvedores façam ajustes nas prioridades e no escopo do projeto a cada Sprint. Além disso, o Scrum se concentra na implementação de um ciclo de desenvolvimento linear e sequencial, mais próximo da realidade.  
B) XP deve ser escolhida na maior parte dos projetos, pois permite um planejamento detalhado a longo prazo, com entregas frequentes em sprints de 3-4 semanas ou um mês. Além disso, como a colaboração com o cliente é fundamental, essa metodologia incentiva reuniões de revisão mensais com as partes interessadas.  
C) XP é mais indicada quando há alta necessidade de refatoração contínua e testes automatizados, o que permite melhor controle da qualidade do código durante o desenvolvimento.  
D) XP deve ser escolhida quando o time deseja trabalhar sem sprints fixos e priorizar documentações mais detalhadas e extensas. Além disso, projetos com baixa variabilidade de requisitos se beneficiam com as práticas XP.  
E) Scrum deve ser preferida em projetos em que há necessidade de ciclos curtos com entregas de incremento de software a cada semana, garantindo uma integração contínua do produto.  

**24.** (FCC – TRT-24/2017 – Técnico Judiciário – Tecnologia da Informação)  
Uma metodologia de desenvolvimento, ou combinação de metodologias, e a sequência de atividades ou componentes pertencentes a esta metodologia é corretamente indicada em:

A) Scrum e XP: Sprint Backlog – Sprint Planning – TDD – Reunião em pé – Classe proprietária.  
B) RUP: Iniciação – Metáfora – Transição – Construção – Refatoração.  
C) FDD: Lista de Features – Planejamento – Modelo de domínio – Detalhamento da Feature – Propriedade coletiva.  
D) Scrum: Product Backlog – Sprint Planning – Sprint Backlog – Daily Scrum – Sprint Review.  
E) TDD: Red Green Refactoring – Feedback – Group Responsibility Principle – Product Owner.  

**25.** (FCC – PGE-AM/2022 – Técnico em Gestão Procuratorial – Engenharia de Software)  
No Scrum, os eventos são usados para criar regularidade e minimizar a necessidade de reuniões não definidas. Considere as atividades e/ou objetivos dos eventos do Scrum:  
I. O propósito deste evento é inspecionar o resultado da Sprint e determinar as adaptações futuras. O Scrum Team apresenta os resultados de seu trabalho para os principais stakeholders e o progresso em direção à Meta do Produto é discutido.  
II. O propósito deste evento é inspecionar o progresso em direção à Meta da Sprint e adaptar o Sprint Backlog conforme necessário, ajustando o próximo trabalho planejado. Este evento melhora as comunicações, identifica os impedimentos e promove a rápida tomada de decisões.  
III. O Scrum Team inspeciona como foi a última Sprint em relação a indivíduos, interações, processos, ferramentas e sua definição de Pronto. As suposições que os desviaram são identificadas e suas origens exploradas. O Scrum Team discute o que deu certo durante a Sprint, quais problemas encontraram e como esses problemas foram (ou não) resolvidos.  
IV. O Product Owner propõe como o produto pode aumentar seu valor e utilidade na Sprint atual. Todo o Scrum Team colabora para definir uma Meta da Sprint. A meta da Sprint deve ser finalizada antes do final deste evento.  
A associação dos itens de I a IV com os eventos Scrum é, correta e respectivamente,

A) Sprint Review − Daily Scrum − Sprint Retrospective − Sprint Planning.  
B) Daily Scrum − Sprint Planning − Sprint Review − Sprint Retrospective.  
C) Sprint Retrospective − Daily Scrum − Sprint Review − Sprint Planning.  
D) Sprint Review − Sprint Planning − Sprint Retrospective − Daily Scrum.  
E) Sprint Retrospective − Sprint Planning − Sprint Review − Daily Scrum.  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Citação |
|---|------|---------|
| 16 | **C** | FCC · TRT-19/2022 · Analista TI — Alta Direção |
| 17 | **D** | FCC · TRT-5/2013 · Analista TI — PDCA SGSI |
| 18 | **A** | FCC · TRF4/2025 · Analista Análise de Sistemas — agentes LGPD |
| 19 | **D** | FCC · TJ-CE/2022 · Analista SI — 2% / R$ 50 mi |
| 20 | **C** | FCC · TRT-1/2025 · Analista TI — IPS ≠ IDS |
| 21 | **A** | FCC · TJ-CE/2022 — Sprint/SM/PO/Sprint Backlog |
| 22 | **D** | FCC · TRF-3/2019 — Daily 15 min |
| 23 | **C** | FCC · TRT-20/2024 Téc. — XP + refactor/testes |
| 24 | **D** | FCC · TRT-24/2017 Téc. — sequência Scrum |
| 25 | **A** | FCC · PGE-AM/2022 — Review/Daily/Retro/Planning |

Letras: C, D, A, D, C, A, D, C, D, A.

**Fontes (amostra):** Quizinho / QConcursos / packs verificados `docs/semana-8/` e `docs/semana-4/01` (mesmos enunciados oficiais FCC).

---

*Arquivo 02 · Revisão fina Segurança + Ágeis · Semana 13 · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: `03-revisao-fina-itil-governanca.md`.*

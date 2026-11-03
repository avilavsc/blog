+++
title = '03 — Revisão fina — ITIL/COBIT + Governança TIC TRT4'
date = '2026-10-30'
draft = false
weight = 3
tags = ['semana-13', 'revisao']
categories = ['estudo']
+++
# Revisão fina — ITIL/COBIT + Governança TIC TRT4 · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — Semana 13 = pré-prova (18/01–24/01)  
**Escopo deste arquivo:** flash **fino** — pegadinhas ITIL 4/5 × COBIT 2019 + ENTIC-JUD (370) / modelo Governança TIC TRT4. Sem matéria nova.  
**Fora de escopo aqui:** PDPJ/SQL (`01`); segurança/ágeis (`02`); decorar 34 practices / 40 objetivos nome a nome.

**Priors:** `docs/semana-12/04`–`05` · `docs/semana-4/07`–`08` · `10` · `docs/semana-10/03`.  
**Modo:** contraste → treino → REAL → gabarito no final.

---

## 1. Material (revisão fina — pegadinhas)

### 1.1 ITIL × COBIT (uma pergunta cada)

| | **ITIL** (edital Version 5 / base 4) | **COBIT 2019** |
|---|--------------------------------------|----------------|
| Pergunta | Como **entregar/operar** serviços com **valor**? | I&T **alinhada, dirigida e monitorada**? |
| Natureza | Boas práticas de **ITSM** | Framework **guarda-chuva** gov + gestão de I&T |
| No tribunal | Catálogo, incidentes, mudanças, SLA, desk | Comitês, PDTIC, risco/benefício, assurance |

**Complementares.** TRT4 usa **ambos** — não “escolhe um”. ≠ PMBOK ≠ ISO 27001.

### 1.2 Peças que a FCC troca

| ITIL | COBIT |
|------|-------|
| **Incident** = restaurar **agora** | **EDM** = única **governança** (Evaluate·Direct·Monitor) |
| **Problem** = causa / recorrência | **APO** Align·Plan·Organize (14) |
| **Change** = alteração controlada | **BAI** Build·Acquire·Implement (11) |
| **Service request** = pedido padrão | **DSS** Deliver·Service·Support (6) ≈ operação |
| **SLM** = acordos de nível | **MEA** Monitor·Evaluate·Assess (4) = desempenho/conformidade |
| **Service Desk** = ponto de contato | Total **5+14+11+6+4 = 40** |
| **Configuration** = CIs (HW/SW/rede…) | **Design factors** = customizar (não one-size) |
| 7 guiding principles | Componentes holísticos do sistema de governança |

```
Incidente ≠ Problema ≠ Requisição ≠ Mudança ≠ SLM ≠ Config
EDM ≠ APO/BAI/DSS/MEA
Optimize THEN automate · Start where you are · Collaborate & visibility
SVS = componentes+atividades juntos para criar valor
4 dimensões: orgs/people · info/tech · partners · value streams/processes
```

### 1.3 Governança TIC TRT4 + ENTIC (flash)

```
Nacional   ENTIC-JUD (CNJ 370/2021) → PDTIC · Comitês · iGov · perspectivas
    ↓
JT         CSJT 397/2024 (PGSERV-TIC) → política de serviços
    ↓
Local      Modelo / Portal Governança TIC TRT4
           diretrizes + estruturas + processos + controles
    ↓
Guias      COBIT · ITIL · BPMN · PMBOK  (referências; não substituem o local)
```

| Exigência 370 | Flash |
|---------------|-------|
| **PDTIC** | Alinhado a PEI + Estratégia Nacional PJ + ENTIC |
| **Comitê de Governança de TIC** | Multidisciplinar (estratégia, projetos, riscos…) |
| **Comitê de Gestão de TIC** | Áreas de TIC; coordenado pelo **titular da TIC**; carteira/portfólio |
| **iGovTIC-JUD** | Medir **maturidade** em TIC (art. 11) |
| Perspectivas | **Processos internos · aprendizado e crescimento · sociedade** |

**Governança** (direcionar/avaliar/monitorar) × **Gestão** (planejar/construir/operar).  
Modelo local: **diretrizes + estruturas + processos + mecanismos de controle**.

| Instância | Ideia |
|-----------|-------|
| **SGTI** | Executa/entrega |
| **CGTIC** (RI art. **227-A**) | Governança: PDTIC, estratégias, PSI… |
| **CSIPD** | Segurança / proteção de dados |

**Mudança** = alterar item de configuração (controlado). **Liberação** = aplicar no ambiente. Catálogo **vivo**. PDTIC **revisável**.

### 1.4 Anti-fusão (prova)

| ≠ Modelo TIC TRT4 | Porque |
|-------------------|--------|
| Código de Conduta | Ética (Gerais) |
| CNJ **400** | Sustentabilidade |
| CNJ **351** / CSJT **360** | Assédio/discriminação |
| MoReq-Jus | Gestão documental |
| PDPJ (443…) | Plataforma nacional |
| PMBOK puro | Projetos — convive, **não** é o modelo |

### 1.5 Cartão único (2 min)

```
ITIL=serviço/valor · Incident≠Problem≠Change≠Config
COBIT=EDM(gov)+APO/BAI/DSS/MEA(40)+design factors
370→PDTIC+comitês+iGov(maturidade)+perspectivas
SGTI opera · CGTIC governa · Mudança≠Liberação
≠Conduta ≠400 ≠MoReq ≠PDPJ-plataforma
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

**4.** Soma dos objetivos COBIT 2019:

A) 5  
B) 20  
C) 34  
D) 40  
E) 93  

**5.** Design factors no COBIT 2019:

A) Garantem processo idêntico em toda organização  
B) Eliminam planejamento  
C) Adaptam/customizam o sistema de governança ao contexto  
D) São sinônimo de Sprint  
E) Substituem EDM  

**6.** SVS (ITIL 4) — propósito principal:

A) Automatizar tudo sem valor  
B) Estrutura para componentes/atividades trabalharem juntos na criação de valor  
C) Definir só cargos de RH  
D) Substituir COBIT  
E) Certificar SGSI  

**7.** Quatro dimensões ITIL 4 incluem:

A) Infraestrutura e suporte; gestão de clientes  
B) Fluxos de valor e processos; parceiros e fornecedores  
C) Só hardware e software  
D) EDM e MEA  
E) PO e SM  

**8.** iGovTIC-JUD (art. 11 da 370):

A) Só disseminar boas práticas  
B) Medir maturidade em TIC dos órgãos sob controle do CNJ  
C) Substituir o PDTIC  
D) Código de Conduta  
E) MoReq  

**9.** Perspectivas dos objetivos estratégicos (370):

A) Processos internos; aprendizado e crescimento; sociedade  
B) Só recursos humanos  
C) Só eventos de atendimento  
D) Eureka, Zuul, Flyway  
E) Conduta, 400, 360  

**10.** Comitê de Gestão de TIC (visão 370):

A) Substitui a Presidência  
B) Coordenado pelo titular da TIC; carteira/portfólio  
C) É o mesmo que Conduta CJF  
D) Aprova só a ENSEC  
E) Elimina o PDTIC  

**11.** Definição cobrável de governança de TIC:

A) Só compra de hardware  
B) Diretrizes, estruturas, processos e controles alinhados às necessidades institucionais  
C) Sinônimo exclusivo de PMBOK  
D) Só Service Desk  
E) Só ISO 27002  

**12.** Mudança × Liberação (modelo local / linha ITIL):

A) Sinônimos  
B) Mudança = alterar CI controlado; Liberação = aplicar no ambiente  
C) Liberação = só causa raiz  
D) Mudança = iGov  
E) Ambos = EDM  

**13.** Modelo TIC TRT4 × Conduta:

A) São a mesma norma  
B) Modelo = organização/processos/controle de TIC; Conduta = ética/valores  
C) Conduta = PDTIC  
D) Modelo = CNJ 360  
E) Não há diferença  

**14.** Plano de contratações STIC (linha 370/468):

A) Dispensa alinhamento a PDTIC/ENTIC  
B) Deve alinhar-se a PDTIC, PEI, Estratégia Nacional do PJ e ENTIC  
C) Substitui a ENTIC nacional  
D) A 370 trata só de assédio  
E) Alinhamento é facultativo  

**15.** Síntese correta:

A) COBIT fecha ticket; ITIL define EDM; iGov = Conduta  
B) ITIL≠COBIT complementares · EDM=gov · Incident≠Problem · 370→PDTIC/iGov · ≠400/MoReq/PDPJ  
C) Portal substitui ENTIC  
D) PMBOK elimina catálogo  
E) DSS = única governança  

---

### Gabarito comentado (treino 1–15)

1. **B** — papéis complementares.  
2. **C** — EDM.  
3. **B** — restaurar × causa.  
4. **D** — 40.  
5. **C** — customizar.  
6. **B** — SVS/valor.  
7. **B** — 4 dimensões.  
8. **B** — maturidade.  
9. **A** — perspectivas.  
10. **B** — titular TIC / portfólio.  
11. **B** — definição 4 elementos.  
12. **B** — mudança × liberação.  
13. **B** — TIC × ética.  
14. **B** — alinhamento STIC.  
15. **B** — cartão.

Letras treino: B, C, B, D, C, B, B, B, A, B, B, B, B, B, B.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** com banca/órgão/ano. Sem Q FCC do portal TRT4 local: usam-se itens reais ENTIC/370/ITIL/COBIT. **Sem invenção.** Gabarito só no final.

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

**23.** (FCC · TRT-18/2023 · Analista Judiciário — TI)  
De acordo com o artigo 11 da Resolução CNJ nº 370/2021, será aplicado periodicamente o Índice de Governança, Gestão e Infraestrutura de Tecnologia da Informação e Comunicação do Poder Judiciário (iGovTIC-JUD), que tem por finalidade

A) realizar a medição da maturidade em TIC dos órgãos submetidos ao controle administrativo e financeiro do CNJ.  
B) disseminar as boas práticas das iniciativas de TIC nos órgãos ligados ao CNJ e realizar a medição das principais entregas, avanços e realizações da área de TIC.  
C) promover a melhoria da governança, da gestão e da colaboração tecnológica no âmbito do Poder Judiciário, visando maximizar os resultados com a medição relativa à otimização do uso dos recursos de TIC.  
D) apoiar o estabelecimento de estratégias, indicadores e metas institucionais; realizar a medição dos riscos da área de TIC; permitir que os investimentos tecnológicos no âmbito institucional fiquem dentro das metas definidas.  
E) permitir que sejam medidas as ações relacionadas à transformação digital de serviços, à integração de canais digitais, à interoperabilidade de sistemas e à estratégia de monitoramento.

**24.** (FCC · TRT-19/2022 · Técnico Judiciário — TI)  
O Art. 2º da Resolução CNJ nº 370/2021 estabelece que a Estratégia Nacional de Tecnologia da Informação e Comunicação do Poder Judiciário é orientada, em seu preâmbulo, nos componentes: objetivos estratégicos e metas.  
Os objetivos estratégicos são distribuídos nas perspectivas:

A) processos internos; aprendizado em tecnologia; recursos tecnológicos.  
B) processos organizacionais; cultura; sociedade.  
C) eventos de aprendizado; sociedade; atendimento ao cidadão.  
D) processos internos; aprendizado e crescimento; sociedade.  
E) eventos organizacionais; aprendizado e crescimento; recursos humanos.

**25.** (FGV · TJ-RJ/2026 · Analista — Gestão de TIC)  
A Estratégia Nacional de Tecnologia da Informação e Comunicação do Poder Judiciário (ENTIC-JUD) é o principal instrumento de promoção da governança ágil e da transformação digital do Poder Judiciário. De acordo com a ENTIC-JUD, cada órgão do Poder Judiciário deverá elaborar e manter o Plano Diretor de Tecnologia da Informação e Comunicação (PDTIC). De acordo com o previsto na Resolução CNJ nº 370/2021, é correto afirmar que:

A) as aquisições de bens e a contratação de serviços de Tecnologia da Informação e Comunicação (TIC) deverão atender às determinações do Tribunal de Contas da União;  
B) as despesas de Tecnologia da Informação e Comunicação (TIC) terão preferência sobre as demais quando houver determinação expressa da administração do Tribunal;  
C) os órgãos que possuem em seu planejamento o Plano Estratégico de Tecnologia da Informação e Comunicação (PETIC) poderão continuar a utilizar esse instrumento até o término de sua vigência;  
D) os órgãos do Poder Judiciário deverão constituir ou manter um Comitê de Governança de Tecnologia da Informação e Comunicação composto por representantes da área de Tecnologia da Informação e Comunicação (TIC);  
E) as áreas de TIC de todos os órgãos do Poder Judiciário deverão constituir ou manter Comitê de Gestão de Tecnologia da Informação e Comunicação, coordenado pelo titular da área de Tecnologia da Informação e Comunicação (TIC).

---

### Gabarito (concurso 16–25)

| Q | Gab. | Fonte / chave |
|---|------|---------------|
| 16 | **A** | FGV TCE-GO/2024 — COBIT gov · ITIL serviços · PMBOK projetos |
| 17 | **B** | FCC TRT-1/2025 — Configuration (papéis trocados nas demais) |
| 18 | **C** | FCC TRT-19/2022 — componentes holísticos |
| 19 | **D** | FCC TRT-20/2024 — collaborate and promote visibility |
| 20 | **E** | FCC TRT-17/2022 — Design Factor I&T-related issues |
| 21 | **C** | FCC TRT-5/2013 — Incident = I e II |
| 22 | **B** | FCC TRT-20/2024 Téc. — SVS = valor |
| 23 | **A** | FCC TRT-18/2023 — iGov = maturidade |
| 24 | **D** | FCC TRT-19/2022 Téc. — perspectivas |
| 25 | **E** | FGV TJ-RJ/2026 — Comitê de Gestão / titular TIC |

Letras: A, B, C, D, E, C, B, A, D, E.

**Fontes:** Quizinho / QConcursos / packs verificados `docs/semana-12/04`–`05` e `docs/semana-4/07`–`10` (enunciados oficiais).

---

## 4. Como usar (pré-prova)

1. 8–10 min nos cartões 1.5 + 1.4.  
2. Treino 1–15 cronometrado.  
3. REAL 16–25 sem olhar gabarito.  
4. Erros → caderno (Incident×Problem; EDM×DSS; iGov; Mudança×Liberação).  
5. Cruzar mentalmente com `01`/`02` só se sobrar energia — **sem matéria nova**.

---

*Arquivo 03 · Revisão fina ITIL/COBIT + Governança TIC TRT4 · Semana 13 · Claudionir · TRT4 Analista TI (código 14) · FCC.*

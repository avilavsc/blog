+++
title = '05 — Mapa Governança TIC TRT4 + ENTIC · Analista TI (código 14)'
date = '2026-10-31'
draft = false
weight = 5
tags = ['semana-12', 'mapa']
categories = ['estudo']
+++
# Mapa Governança TIC TRT4 + ENTIC · Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — Semana 12 = revisão ativa · mapa visual/textual cobrável  
**Escopo deste arquivo:** **mapa** do Modelo/Portal de Governança de TIC do TRT4 + **ENTIC-JUD (CNJ 370)** + encaixe COBIT/ITIL/BPMN/PMBOK · anti-confusão Conduta/400/PDPJ/MoReq.  
**Fora de escopo aqui:** ITIL/COBIT linha a linha (arquivo `04`); MoReq/PDTIC capítulo a capítulo (semana 10); Gerais (`06`).

**Priors:** `/cursor/stores/self/docs/semana-4/10-governanca-tic-trt4.md` · `/cursor/stores/self/docs/semana-10/03-governanca-tic-trt4.md`.

---

## 1. Material — mapa cobrável

### 1.1 Camadas (visão de prova)

```
Nacional     ENTIC-JUD (Res. CNJ 370/2021)  → PDTIC · Comitês · iGov · perspectivas
    ↓
JT 1º/2º     CSJT 397/2024 (PGSERV-TIC)     → política de serviços
    ↓
Local        Modelo / Portal Governança TIC TRT4
             diretrizes + estruturas + processos + controles
    ↓
Guias        COBIT 2019 · ITIL 5/4 · BPMN · PMBOK 8  (referências, não substituem o local)
```

Flash: **370 manda a estratégia; o modelo TRT4 opera a governança no tribunal.**

### 1.2 Definição (4 elementos)

> **Governança de TIC** = **diretrizes** + **estruturas organizacionais** + **processos de trabalho** + **mecanismos de controle** → decisões/ações de TIC **alinhadas** às necessidades institucionais e à **missão/metas**.

**Governança** (direcionar/avaliar/monitorar alinhamento) × **Gestão** (planejar/construir/executar/operar no dia a dia).  
COBIT dialoga com a 1ª; ITIL com a 2ª — o modelo **usa ambos**.

### 1.3 ENTIC-JUD — checklist FCC

| Exigência 370 | Flash |
|---------------|-------|
| **PDTIC** | Alinhado a PEI + Estratégia Nacional PJ + ENTIC |
| **Comitê de Governança de TIC** | Multidisciplinar (estratégia, projetos, riscos…) |
| **Comitê de Gestão de TIC** | Nas áreas de TIC; coordenado pelo **titular da TIC**; carteira/portfólio |
| **iGovTIC-JUD** | Medir **maturidade** em TIC (art. 11) — evidências |
| Perspectivas dos objetivos | **Processos internos · aprendizado e crescimento · sociedade** |

Distratores clássicos de iGov: “só disseminar boas práticas”, “só transformação digital”, “só riscos”.

### 1.4 Portal TRT4 — blocos a reconhecer

| Bloco | O que cair |
|-------|------------|
| Organização | **SGTI** e unidades |
| Serviços/produtos | Catálogo **vivo** |
| Planos / PDTIC | Ciclo ~2 anos + revisão; **não** estático |
| Projetos | Portfólio / priorização / alinhamento |
| Indicadores | iGov, satisfação, SLA, projetos, orçamento… |
| Orçamento / aquisições | Alinhados ao PDTIC |
| Processos | Modelagem BPMN, mudanças/liberações, problemas, continuidade, catálogo |

### 1.5 Estruturas (cadeia, não nomes de pessoas)

| Instância | Ideia |
|-----------|-------|
| Presidência / Alta Administração | Aprova planos/processos relevantes |
| **SGTI** | Executa/entrega |
| **CGTIC** (RI art. **227-A**) | Governança: PDTIC, estratégias, PSI… |
| Subcomitê de TIC | Articulação técnica |
| Comitê de Gestão (visão 370) | Carteira/portfólio · titular TIC |
| **CSIPD** | Segurança / proteção de dados |
| Divisão Conformidade/Processos | Modelagem · publicação no portal |

### 1.6 Processos — flash anti-troca

| Processo | Flash |
|----------|-------|
| Modelagem | Planejar → Executar → Regulamentar · publicar |
| **Mudança** | Alterar item de configuração (controlado) |
| **Liberação** | Aplicar a mudança no ambiente |
| Catálogo | Atualizado; aprovações Subcomitê → Comitê → Presidência |
| Problemas | Causa raiz / reduzir recorrência (linha ITIL) |
| Continuidade | Disponibilidade / recuperação de críticos |
| PDTIC | Diagnóstico → plano → monitoração |

### 1.7 Anti-fusão (semana 12)

| ≠ Modelo TIC TRT4 | Porque |
|-------------------|--------|
| Código de Conduta | Ética (Gerais) |
| CNJ **400** | Sustentabilidade (Gerais) |
| CNJ **351** / CSJT **360** | Assédio/discriminação |
| MoReq-Jus | Gestão documental |
| PDPJ (443…) | Plataforma nacional |
| PMBOK puro | Projetos — convive, **não** é o modelo |

### 1.8 Cartão único

```
370 → PDTIC + Comitês + iGov (maturidade) + perspectivas
Portal TRT4 = diretrizes + estruturas + processos + controle
SGTI opera · CGTIC governa (227-A) · CSIPD segura · BPMN publica
COBIT ⊃ alinhamento · ITIL ⊃ serviços · PMBOK ⊃ projetos
≠ Conduta ≠ 400 ≠ MoReq ≠ PDPJ-plataforma
Mudança ≠ Liberação · Catálogo vivo · PDTIC revisável
```

---

## 2. Questões de treino (1–15)

**1.** Ligação ENTIC ↔ modelo local:

A) 370 é irrelevante ao portal  
B) 370 exige PDTIC/comitês/iGov que o portal materializa  
C) Portal substitui a ENTIC nacional  
D) 370 = Conduta  
E) 370 = MoReq  

**2.** Definição cobrável de governança de TIC inclui:

A) Só compra de hardware sem alinhamento  
B) Diretrizes, estruturas, processos e controles alinhados às necessidades institucionais  
C) Apenas CNJ 400  
D) Só crase  
E) Proibição de indicadores  

**3.** iGovTIC-JUD (art. 11 da 370) — finalidade:

A) Medir maturidade em TIC dos órgãos sob CNJ  
B) Substituir o PDTIC  
C) Definir senhas Wi-Fi  
D) Criar o MoReq  
E) Revogar o COBIT  

**4.** Perspectivas dos objetivos ENTIC:

A) Processos internos; aprendizado e crescimento; sociedade  
B) Apenas DNS, DHCP, LDAP  
C) Só Scrum/Kanban/XP  
D) EDM/APO/BAI como “perspectivas ENTIC”  
E) WEP/WPA  

**5.** Comitê de Gestão de TIC (370) — coordenação:

A) Pelo titular da área de TIC  
B) Exclusivamente pela OAB  
C) Pelo cartório extrajudicial  
D) Pelo Service Desk municipal  
E) Pelo autor do PMBOK  

**6.** Contraste COBIT × ITIL no modelo TRT4:

A) Ambos = Conduta  
B) COBIT tipicamente governança/controle; ITIL tipicamente serviços  
C) COBIT = 351; ITIL = 400  
D) Nenhum dialoga com o TRT4  
E) ITIL certifica SGSI  

**7.** “Liberação”, no processo de mudanças:

A) Demitir a SGTI  
B) Aplicar a mudança planejada no ambiente  
C) Criar a PDPJ-Br  
D) Aprovar a DUDH  
E) Extinguir o catálogo  

**8.** PGSERV-TIC (CSJT 397/2024):

A) Política de gerenciamento de serviços de TIC na JT 1º/2º  
B) Código de Conduta  
C) DUDH  
D) Apenas MapStruct  
E) Interpretação de texto  

**9.** Art. 227-A (CGTIC) — ponto TI:

A) Participar de estratégias/PDTIC e opinar sobre PSI  
B) Substituir o STF  
C) Extinguir a SGTI  
D) Criar a LGPD  
E) Definir apenas WEP  

**10.** Assinale a **inadequada**:

A) Modelo apoia-se em COBIT/ITIL/BPMN  
B) PDTIC é instrumento-chave do portal  
C) Modelo TRT4 = sinônimo de Conduta ética  
D) SGTI opera serviços alinhados à estratégia  
E) Catálogo deve estar atualizado  

**11.** PDTIC — inadequado afirmar:

A) Pode ser revisado  
B) Acompanha ações, orçamento, níveis de serviço  
C) É imutável para sempre, sem revisão  
D) Alinha-se a TRT4/CSJT/CNJ  
E) Publica-se no portal após trâmites  

**12.** BPMN no Portal serve principalmente para:

A) Criptografar discos  
B) Modelar/representar processos de TIC publicados  
C) Substituir Keycloak  
D) Revogar a ENTIC  
E) Definir crase  

**13.** Distinção correta:

A) Modelo TIC = organização/processos/controle de TIC; Conduta = ética/valores  
B) Conduta = PDTIC  
C) Modelo = CNJ 360  
D) Modelo = apenas ENSEC  
E) Não há diferença  

**14.** Plano de contratações de STIC (linha 370/468), ideia cobrável:

A) Dispensa alinhamento a PDTIC/ENTIC  
B) Deve alinhar-se a PDTIC, PEI, Estratégia Nacional do PJ e ENTIC  
C) Substitui a ENTIC nacional  
D) A 370 trata só de assédio  
E) Alinhamento é facultativo  

**15.** Síntese do mapa:

A) 370→PDTIC/comitês/iGov · Portal materializa · COBIT/ITIL/BPMN · SGTI/CGTIC · ≠Conduta/MoReq/PDPJ  
B) Governança TIC = só 400  
C) Portal substitui ENTIC  
D) Mudança = opinião sem registro  
E) PMBOK elimina catálogo  

---

### Gabarito comentado (treino 1–15)

1. **B** — 370 materializada no portal.  
2. **B** — definição.  
3. **A** — maturidade.  
4. **A** — perspectivas.  
5. **A** — titular TIC.  
6. **B** — COBIT × ITIL.  
7. **B** — liberação no ambiente.  
8. **A** — serviços JT.  
9. **A** — 227-A.  
10. **C** — inadequada (≠ Conduta).  
11. **C** — PDTIC não é imutável.  
12. **B** — modelagem.  
13. **A** — TIC × ética.  
14. **B** — alinhamento STIC.  
15. **A** — cartão.

---

## 3. Questões de concurso (16–25)

> Sem Q FCC do portal TRT4 local: usam-se questões **reais** sobre ENTIC/370/PDTIC/comitês/contratações/governança. Gabarito só no final.

**16.** (FCC · TRT-18/2023 · Analista Judiciário — TI)  
De acordo com o artigo 11 da Resolução CNJ nº 370/2021, será aplicado periodicamente o Índice de Governança, Gestão e Infraestrutura de Tecnologia da Informação e Comunicação do Poder Judiciário (iGovTIC-JUD), que tem por finalidade

A) realizar a medição da maturidade em TIC dos órgãos submetidos ao controle administrativo e financeiro do CNJ.  
B) disseminar as boas práticas das iniciativas de TIC nos órgãos ligados ao CNJ e realizar a medição das principais entregas, avanços e realizações da área de TIC.  
C) promover a melhoria da governança, da gestão e da colaboração tecnológica no âmbito do Poder Judiciário, visando maximizar os resultados com a medição relativa à otimização do uso dos recursos de TIC.  
D) apoiar o estabelecimento de estratégias, indicadores e metas institucionais; realizar a medição dos riscos da área de TIC; permitir que os investimentos tecnológicos no âmbito institucional fiquem dentro das metas definidas.  
E) permitir que sejam medidas as ações relacionadas à transformação digital de serviços, à integração de canais digitais, à interoperabilidade de sistemas e à estratégia de monitoramento.

**17.** (FCC · TRT-19/2022 · Técnico Judiciário — TI)  
O Art. 2º da Resolução CNJ nº 370/2021 estabelece que a Estratégia Nacional de Tecnologia da Informação e Comunicação do Poder Judiciário é orientada, em seu preâmbulo, nos componentes: objetivos estratégicos e metas.  
Os objetivos estratégicos são distribuídos nas perspectivas:

A) processos internos; aprendizado em tecnologia; recursos tecnológicos.  
B) processos organizacionais; cultura; sociedade.  
C) eventos de aprendizado; sociedade; atendimento ao cidadão.  
D) processos internos; aprendizado e crescimento; sociedade.  
E) eventos organizacionais; aprendizado e crescimento; recursos humanos.

**18.** (FGV · TJ-RJ/2026 · Analista — Gestão de TIC)  
A Estratégia Nacional de Tecnologia da Informação e Comunicação do Poder Judiciário (ENTIC-JUD) é o principal instrumento de promoção da governança ágil e da transformação digital do Poder Judiciário. De acordo com a ENTIC-JUD, cada órgão do Poder Judiciário deverá elaborar e manter o Plano Diretor de Tecnologia da Informação e Comunicação (PDTIC). De acordo com o previsto na Resolução CNJ nº 370/2021, é correto afirmar que:

A) as aquisições de bens e a contratação de serviços de Tecnologia da Informação e Comunicação (TIC) deverão atender às determinações do Tribunal de Contas da União;  
B) as despesas de Tecnologia da Informação e Comunicação (TIC) terão preferência sobre as demais quando houver determinação expressa da administração do Tribunal;  
C) os órgãos que possuem em seu planejamento o Plano Estratégico de Tecnologia da Informação e Comunicação (PETIC) poderão continuar a utilizar esse instrumento até o término de sua vigência;  
D) os órgãos do Poder Judiciário deverão constituir ou manter um Comitê de Governança de Tecnologia da Informação e Comunicação composto por representantes da área de Tecnologia da Informação e Comunicação (TIC);  
E) as áreas de TIC de todos os órgãos do Poder Judiciário deverão constituir ou manter Comitê de Gestão de Tecnologia da Informação e Comunicação, coordenado pelo titular da área de Tecnologia da Informação e Comunicação (TIC).

**19.** (FGV · TJ-RJ/2026 · Analista — Gestão de TIC)  
As áreas de Tecnologia da Informação e Comunicação (TIC) de todos os órgãos do Poder Judiciário deverão constituir ou manter um Comitê de Gestão de Tecnologia da Informação com a atribuição de:

A) definir a carteira de projetos e a gestão de portfólio de serviços de TIC;  
B) aprovar planos estratégicos, disseminando a importância da área de TIC nos tribunais;  
C) envolver a área de negócio nas decisões estratégicas que incidem sobre os serviços de TIC;  
D) estruturar escritório de projetos que favoreça o emprego de práticas de gestão de projetos especificadas pelo tribunal;  
E) estabelecer Plano Estratégico de Tecnologia da Informação e Comunicação (PETIC) para iniciativas de curta duração ou escopo simplificado.

**20.** (CEBRASPE · STM/2025 · Analista — Análise de Sistemas)  
Com base nas Resoluções CNJ nº 370/2021 e nº 468/2022, assinale a afirmativa **correta**.

A) As contratações de soluções de TIC por órgãos do Poder Judiciário dispensam plano de contratações de STIC.  
B) O plano de contratações de STIC deve estar alinhado ao PDTIC, ao planejamento estratégico institucional, à Estratégia Nacional do Poder Judiciário e à ENTIC-JUD.  
C) O PDTIC substitui a ENTIC-JUD no âmbito nacional.  
D) A Resolução CNJ 370/2021 trata exclusivamente de contratações de STIC, e não de estratégia de TIC.  
E) O alinhamento do plano de contratações à ENTIC-JUD é facultativo.

**21.** (CEBRASPE · STM/2025 · Analista — Suporte em TI)  
A Resolução CNJ nº 370/2021 estabelece que a Estratégia Nacional de TIC do Poder Judiciário (ENTIC-JUD) deve ser implementada com base em princípios de governança e gestão de TIC alinhados às diretrizes estratégicas do CNJ, ao passo que a Resolução CNJ nº 468/2022 e o Guia de Contratações de STIC reforçam a necessidade de transparência e eficiência nas aquisições de TIC.

A) Certo  
B) Errado  
C) Certo apenas quanto à 370/2021  
D) Certo apenas quanto à 468/2022  
E) Errado, pois a 370/2021 trata só de assédio  

**22.** (CEBRASPE · STM/2025 · Analista — Análise de Sistemas)  
A Estratégia Nacional de TIC do Poder Judiciário (ENTIC-JUD) determina que o plano de transformação digital seja elaborado pela unidade competente dos órgãos, respeitadas suas especificidades, e que seja aprovado pelo Comitê de Gestão de TIC.

A) Certo  
B) Errado  
C) Certo, se houver PDTIC  
D) Certo, se houver PETIC  
E) Errado apenas na Justiça Eleitoral  

**23.** (FCC · TRT-3/MG · Analista Judiciário — TI; Res. CNJ 182/2013)  
Na Gestão de Aquisições e Contratos de TI, quanto ao plano e planejamento das contratações, a Resolução CNJ nº 182/2013 recomenda que as contratações sejam precedidas de Planejamento elaborado em harmonia com o Planejamento Estratégico

A) de Tecnologia da Informação e Comunicação alinhado com os requisitos de infraestrutura estabelecidos pela Administração de TI.  
B) do Poder Judiciário, alinhado com o Planejamento Estratégico Governamental.  
C) Institucional ou Planejamento Estratégico de Tecnologia da Informação e Comunicação, alinhados com o Planejamento Estratégico do Poder Judiciário.  
D) Institucional e com os requisitos de infraestrutura estabelecidos pela Administração de TI, alinhados com o Planejamento Estratégico Governamental.  
E) do Poder Judiciário e com o Planejamento Estratégico de Tecnologia da Informação, alinhados com o Planejamento Estratégico Governamental.

**24.** (FGV · TJ-MS/2024 · Analista de Governança)  
O Plano Diretor de Tecnologia da Informação e Comunicação (PDTIC) é um documento que passa por uma série de transformações ao longo de sua utilização. No processo de elaboração do PDTIC, a análise estratégica e o levantamento de necessidades são realizados no(a):

A) preparação;  
B) diagnóstico;  
C) execução;  
D) monitoramento;  
E) avaliação.

**25.** (FCC · PMSP/2025 · Analista TIC)  
A equipe de Analistas de uma Prefeitura Municipal implementou um novo sistema de gerenciamento eletrônico de documentos. Após a implementação, unidades usavam padrões diferentes para nomear e arquivar documentos. Com base no COBIT 2019, a ação alinhada aos princípios de governança e gestão é

A) realizar treinamentos sobre a operação do sistema com todos os servidores, destacando os benefícios do novo sistema para aumentar a adesão.  
B) atualizar os contratos com os fornecedores do sistema para incluir cláusulas de suporte técnico mais rigorosas.  
C) estabelecer e implementar políticas e procedimentos padronizados para a classificação, nomeação e arquivamento de documentos, garantindo alinhamento com os objetivos organizacionais.  
D) delegar a cada unidade administrativa a responsabilidade de definir seus próprios padrões de arquivamento, respeitando as especificidades locais.  
E) contratar consultores externos para realizar auditorias periódicas do uso do sistema em cada unidade administrativa.

---

### Gabarito comentado (concurso 16–25)

| Q | Gab. | Fonte / chave |
|---|------|---------------|
| 16 | **A** | FCC TRT-18/2023 — iGov = maturidade |
| 17 | **D** | FCC TRT-19/2022 — perspectivas |
| 18 | **E** | FGV TJ-RJ/2026 — Comitê de Gestão / titular TIC |
| 19 | **A** | FGV TJ-RJ/2026 — carteira/portfólio |
| 20 | **B** | CEBRASPE STM/2025 — plano STIC alinhado |
| 21 | **A** | CEBRASPE STM/2025 — item certo (370 + 468) |
| 22 | **B** | CEBRASPE STM/2025 — aprovação ≠ Comitê de Gestão (autoridade máxima) |
| 23 | **C** | FCC TRT-3 — PEI ou PETIC alinhados ao PE do PJ |
| 24 | **B** | FGV TJ-MS/2024 — diagnóstico |
| 25 | **C** | FCC PMSP/2025 — políticas padronizadas |

Distribuição: A, D, E, A, B, A, B, C, B, C.

---

## 4. Como usar

1. 12 min no cartão 1.8 + anti-fusão 1.7.  
2. Religar `04` (ITIL×COBIT) em 5 min.  
3. Treino 1–15 → gabarito.  
4. Concurso 16–25.  
5. Erros: iGov; Comitê Gestão×Governança; Mudança×Liberação; ≠Conduta/400/MoReq/PDPJ.

---

*Arquivo 05 · Mapa Governança TIC TRT4 + ENTIC · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 12.*

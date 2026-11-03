+++
title = '10 — Modelo de Governança de TIC do TRT4 · Analista TI (código 14)'
date = '2026-10-24'
draft = false
weight = 10
tags = ['semana-04', 'governanca', 'tic']
categories = ['estudo']
+++
# Modelo de Governança de TIC do TRT4 · Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 10 — Modelo de Governança de TIC do TRT4  
**Escopo deste arquivo:** o que é o **Modelo / Portal de Governança de TIC** do TRT4; definição de **Governança de TIC**; encaixe com **COBIT** e **ITIL**; **PDTIC 2024–2026**; estruturas (SGTI, comitês); processos exemplares (mudanças, catálogo, problemas, continuidade, modelagem); anti-confusão com PMBOK, PDPJ e Gerais.  
**Fora de escopo aqui:** PMBOK 8 em profundidade (arquivo 09); ITIL/COBIT linha a linha (07–08); PDTIC capítulo a capítulo (reforço na semana 10); texto integral de todas as portarias.

---

## 1. Material

### 1.1 Onde cai no edital (Específicas)

No código 14, o **Modelo de Governança de TIC do TRT4** aparece junto de normativos PDPJ/Judiciário/TRT4 (MoReq-Jus, resoluções/portarias CNJ, **PDTIC 2024–2026**). Bloco de **Específicas**.

| Bloco | Tema | Não misturar |
|-------|------|--------------|
| **Específicas — modelo local** | **Governança TIC TRT4** + PDTIC | Conduta ética (Gerais) |
| **Específicas — guias** | COBIT 2019 · ITIL 5 · PMBOK 8 | “só teoria sem TRT4” |
| **Específicas — PDPJ** | 443/522/574 + stack | portal local ≠ plataforma nacional |
| **Gerais** | 400 / 351 / 360 / Conduta | governança de TIC |

Flash: **Modelo TRT4 = como o tribunal organiza decisões, processos e serviços de TIC** · dialoga com COBIT/ITIL · **≠** Código de Conduta.

### 1.2 Frase-âncora e definição cobrável

**Frase-âncora:** o TRT4 mantém um **Modelo / Portal de Governança de TIC** que documenta organização, serviços/produtos, planos (PDTIC), projetos, indicadores, orçamento e **processos** de TIC, alinhando a área de tecnologia à estratégia institucional.

Definição usada em portarias locais (ideia FCC):

> **Governança de TIC:** conjunto de **diretrizes**, **estruturas organizacionais**, **processos de trabalho** e **mecanismos de controle** que visam assegurar que as decisões e ações relativas à gestão e uso de TIC estejam **alinhadas às necessidades institucionais** e contribuam para a **missão** e as **metas** da instituição.

**TIC**, no mesmo vocabulário: ativo estratégico que conjuga recursos, processos e técnicas para obter, processar, armazenar, usar e disseminar informações.

### 1.3 Portal de Governança — mapa mental

O Portal (visão de prova) costuma agrupar:

| Bloco do portal | Ideia |
|-----------------|-------|
| Organização | Quem é a SGTI / estrutura |
| Serviços e produtos | O que a TIC entrega |
| Planos de TIC / PDTIC | Planejamento plurianual + revisões |
| Projetos | Portfólio / acompanhamento |
| Indicadores | Medição e resultados |
| Orçamento | Recursos e investimentos |
| Processos | Modelagem, mudanças, catálogo, problemas, continuidade… |

Não decore números operacionais de parque/chamados; reconheça a **lógica de transparência + processos publicados**.

### 1.4 Encaixe com COBIT e ITIL (ouro)

Referências técnicas recorrentes nos atos do TRT4:

| Guia | Papel no modelo local |
|------|------------------------|
| **COBIT 2019** (e menções históricas a COBIT 5 em atos antigos) | Modelo de **governança** / objetivos de controle e alinhamento |
| **ITIL 4** (atos locais; edital lista **ITIL Version 5**) | Boas práticas de **gerenciamento de serviços** (valor, eficiência operacional) |
| **BPMN** | Notação para **modelar** processos de TIC |
| **PMBOK** | Projetos (convive; não substitui o modelo de governança de TIC) |

Leitura cobrável:

1. O modelo TRT4 **não inventa** governança do zero: **apoia-se** em boas práticas (COBIT/ITIL/BPMN) + normativos CNJ/CSJT/TCU.  
2. **Governança** (diretrizes, estruturas, controle, alinhamento) × **Gestão/serviços** (operar e entregar valor no dia a dia).  
3. Edital: estude **ITIL 5** e **COBIT 2019** nos arquivos próprios; aqui basta o **encaixe**.  
4. Distrator: “Modelo TRT4 = apenas PMBOK” ou “= apenas Conduta ética”.

### 1.5 PDTIC 2024–2026 (instrumento-chave)

**PDTIC** = Plano Diretor de Tecnologia da Informação e Comunicação.

Objetivo (síntese oficial): reunir o desdobramento das estratégias **nacionais e regionais** para planejar ações e operação da **SGTI**, alocando recursos em ações/projetos que sustentem a **prestação jurisdicional** e a administração, alinhados a objetivos do **TRT4, CSJT e CNJ**.

Permite acompanhar: evolução das ações, execução orçamentária, resultados, **níveis de serviço** e melhorias.

Características cobráveis:

- **Não é estático** — pode sofrer alterações no ciclo.  
- Ciclo típico: novo PDTIC a cada **dois anos** (coincidindo com trocas de administração) + **revisão anual**.  
- Consolida, entre outros: referencial estratégico/normativo, portfólio de projetos, plano de contratações, capacitação, catálogo/níveis de serviço, operações, segurança da informação e proteção de dados, catálogo de sistemas.  
- Publicação no **Portal de Governança de TIC** (após trâmites/aprovação).

Flash: **PDTIC = plano diretor da TIC local** · reforço também na **semana 10**.

### 1.6 Estruturas e papéis (nível mapa)

| Papel / instância | Ideia |
|-------------------|-------|
| **Presidência** | Aprova planos/processos relevantes; interesse na atividade-fim |
| **SGTI** (Secretaria-Geral de Tecnologia e Inovação) | Braço executivo de TIC; opera serviços, produtos, processos |
| **Subcomitê de TIC** | Avalia proposições; articula áreas técnicas |
| **Comitê de Governança de TIC** | Instância de governança (aprovações/alinhamento — ex.: catálogo) |
| **CSIPD** (segurança/proteção de dados) | Assessora continuidade/segurança conforme o processo |
| **Divisão de Processos / Conformidade** | Modelagem, tramitação, publicação de processos |

Não decore nomes de pessoas; reconheça **hierarquia de decisão** e que processos têm **papéis e responsabilidades** publicados.

### 1.7 Processos exemplares (reconhecer o leque)

O modelo materializa-se em **processos formalizados** (portarias + documentação no portal):

| Processo | Ideia cobrável |
|----------|----------------|
| **Modelagem e Gestão de Processos** (ex.: Portaria 76/2025) | Planejar → Executar → Regulamentar; publicar no portal; revisão anual |
| **Gerenciamento de Mudanças e Liberações** (ex.: Portaria 1.722/2025) | Mudança = alteração de item de configuração; Liberação = aplicar no ambiente; etapas Abertura → Análise/aprovação → Liberação → Fechamento |
| **Catálogo de Serviços** | Manter catálogo atualizado/publicado; categorizar chamados; aprovações (Subcomitê → Comitê → Presidência) |
| **Gerenciamento de Problemas** | Causa raiz / redução de incidentes recorrentes (linha ITIL) |
| **Continuidade de TIC** | Disponibilidade e recuperação tempestiva de serviços críticos |

Definições úteis (mudanças):

- **Mudança:** acréscimo, modificação ou remoção de itens de configuração que possam afetar serviços de TIC.  
- **Liberação:** aplicação da mudança planejada no ambiente computacional.

Referências normativas frequentes nos processos: **Res. CSJT 397/2024** (PGSERV-TIC — política de gerenciamento de serviços na JT 1º/2º), **Res. CNJ 370/2021** (ENTIC-JUD), ITIL, COBIT, BPMN.

### 1.8 Princípios/diretrizes (PDTIC — cartão)

Sem decorar a lista inteira, reconheça o espírito:

1. Priorizar ações da **estratégia do TRT**.  
2. **Interoperabilidade** e dados abertos.  
3. Soluções **reutilizáveis** no PJ; reduzir legado desnecessário.  
4. Automatizar tarefas repetitivas de baixo valor.  
5. Alinhar a **ENTIC-JUD** / planejamento estratégico.  
6. Aquisições com **planejamento formal** (valor ao tribunal).  
7. Ampliar **acesso à Justiça** e celeridade.  
8. Teletrabalho com **segurança e desempenho**.  
9. Processos de trabalho + boas práticas de **governança**.  
10. Excelência no **atendimento** aos usuários.  
11. Mitigar riscos de **segurança** (CID — confidencialidade, integridade, disponibilidade).  
12. Capacitação contínua; orçamento equilibrando **estratégia + manutenção**.

### 1.9 Anti-confusão rápida (cartão)

| Instituto | Tema | Bloco |
|-----------|------|-------|
| **Modelo Governança TIC TRT4** | Diretrizes, estruturas, processos, controle de TIC | Específicas |
| **PDTIC 2024–2026** | Plano diretor plurianual da TIC | Específicas |
| **COBIT 2019** | Framework de governança/gestão de TI | Específicas |
| **ITIL 5** (edital) / ITIL 4 (atos) | Serviços de TI | Específicas |
| **PMBOK 8** | Projetos | Específicas |
| **Conduta TRT4** | Ética/valores | Gerais |
| **CNJ 400** | Sustentabilidade PJ | Gerais |
| **PDPJ (443…)** | Plataforma nacional | Específicas (outro eixo) |

Armadilhas FCC:

1. “Governança TIC = Código de Conduta” → **errado**.  
2. “PDTIC = PMBOK” → **errado** (plano local × guia de projetos).  
3. “Modelo TRT4 substitui COBIT/ITIL” → **errado** (usa/alinha).  
4. “Só existe governança sem processos publicados” → leitura pobre.  
5. “Mudança = qualquer ideia sem registro/aprovação” → contrário ao processo formal.  
6. “PDTIC é imutável por 10 anos” → **errado** (revisão/alterações).

### 1.10 O que a FCC tipicamente testa

- Definição de **Governança de TIC** (alinhamento + estruturas + processos + controle).  
- Existência do **Portal / modelo** e do **PDTIC**.  
- Encaixe **COBIT** (governança) × **ITIL** (serviços) × **BPMN** (modelagem).  
- Papel da **SGTI** e instâncias de aprovação.  
- Ideia de **processos** (mudança/liberação, catálogo, continuidade…).  
- Separação de **Conduta/400** e de **PDPJ** como eixos vizinhos.  
- Que o modelo **serve à prestação jurisdicional** (não “TI por TI”).

### 1.11 Mini-síntese (cartão mental)

> **Modelo de Governança de TIC do TRT4** = diretrizes + estruturas + processos + controles alinhados à missão · **Portal** + **PDTIC** · apoia-se em **COBIT/ITIL/BPMN** + normativos CNJ/CSJT · SGTI opera · Específicas · **≠** Conduta · **≠** PMBOK puro · **≠** PDPJ nacional.

---

## 2. Questões de treino (1–15)

**1.** No edital (código 14), o Modelo de Governança de TIC do TRT4 encaixa-se tipicamente em:

A) Apenas Português  
B) Específicas (normativos/governança TIC locais + PDTIC)  
C) Somente crase  
D) Exclusivamente DUDH  
E) Só cabeamento  

**2.** Governança de TIC, na definição cobrável das portarias locais, enfatiza:

A) Apenas compra de mouses sem alinhamento  
B) Diretrizes, estruturas, processos e controles alinhados às necessidades institucionais  
C) Exclusivamente o Código de Conduta  
D) Apenas a Res. CNJ 400  
E) Proibição de qualquer processo  

**3.** O PDTIC 2024–2026 é melhor descrito como:

A) Plano Diretor de TIC que desdobra estratégias e planeja ações/operação da SGTI  
B) Sinônimo de CNJ 351  
C) Regimento Interno completo  
D) Apenas um cartaz de sustentabilidade  
E) Framework PMI idêntico ao PMBOK  

**4.** Assinale o encaixe **correto**:

A) COBIT — tipicamente governança/controle de TI; ITIL — serviços  
B) COBIT — só assédio; ITIL — só Conduta  
C) COBIT = PDPJ; ITIL = ENSEC  
D) Ambos equivalem à LGPD  
E) Nenhum dialoga com o modelo TRT4  

**5.** BPMN, no contexto dos processos de TIC do TRT4, serve principalmente para:

A) Criptografar discos  
B) Modelar/representar processos de negócio  
C) Substituir o Keycloak  
D) Revogar a ENTIC-JUD  
E) Definir crase  

**6.** “Mudança”, no processo de mudanças/liberações, significa tipicamente:

A) Qualquer opinião informal sem registro  
B) Acréscimo, modificação ou remoção de itens de configuração que possam afetar serviços de TIC  
C) Apenas alteração do Código de Conduta  
D) Exclusivamente publicação de notícia  
E) Troca de mesa sem relação com TIC  

**7.** Liberação, no mesmo processo, relaciona-se a:

A) Aplicar a mudança planejada no ambiente computacional  
B) Demitir a SGTI  
C) Criar a PDPJ-Br  
D) Aprovar a DUDH  
E) Extinguir o catálogo  

**8.** O Portal de Governança de TIC costuma publicar, entre outros:

A) Apenas receitas culinárias  
B) Processos, planos (PDTIC), serviços/produtos, indicadores, projetos  
C) Somente o texto da CF/1988 sem TIC  
D) Apenas provas da FCC  
E) Nada relacionado a TIC  

**9.** Contraste correto:

A) Modelo TIC TRT4 = Conduta ética  
B) Modelo TIC TRT4 = organização/processos/controle de TIC; Conduta = ética/valores  
C) Conduta = PDTIC  
D) Modelo TIC = CNJ 360  
E) Não há diferença possível  

**10.** A SGTI, no mapa local, é melhor vista como:

A) Órgão que opera/entrega TIC alinhada à estratégia  
B) Substituta do STF  
C) Autora do PMBOK mundial  
D) Comissão exclusiva de assédio (351)  
E) Cartório extrajudicial privado  

**11.** Sobre o PDTIC, é inadequado afirmar que:

A) Pode ser revisado/alterado conforme necessidades  
B) Ajuda a acompanhar ações, orçamento e níveis de serviço  
C) É imutável para sempre, sem revisão anual/ciclo  
D) Alinha-se a estratégias do TRT4/CSJT/CNJ  
E) Publica-se no portal após trâmites  

**12.** Qual normativo nacional costuma aparecer como referência de estratégia de TIC do PJ?

A) Res. CNJ 370/2021 (ENTIC-JUD)  
B) Apenas a Lei de Diretrizes Orçamentárias de outro Poder sem relação  
C) Só o Estatuto da OAB  
D) Exclusivamente o Código Civil  
E) Nenhuma resolução CNJ existe  

**13.** PGSERV-TIC (linha CSJT 397/2024) relaciona-se melhor a:

A) Política de gerenciamento de **serviços** de TIC na JT de 1º/2º graus  
B) Código de Conduta do TRT4  
C) DUDH  
D) Apenas MapStruct  
E) Interpretação de texto  

**14.** Catálogo de Serviços, no modelo:

A) Deve permanecer secreto e desatualizado  
B) Precisa estar atualizado/publicado para categorizar e gerir serviços/chamados  
C) Substitui o Regimento Interno  
D) É idêntico à Res. 400  
E) Proíbe Service Desk  

**15.** A síntese mais adequada do Modelo de Governança de TIC do TRT4 é:

A) Conduta + 400 + 351 sem TIC  
B) Diretrizes/estruturas/processos/controles + Portal/PDTIC + COBIT/ITIL/BPMN · Específicas · ≠ Conduta/PMBOK puro  
C) Apenas Selenium  
D) Apenas crase  
E) Extinção da SGTI  

---

### Gabarito comentado (treino 1–15)

1. **B** — Específicas / modelo + PDTIC.  
2. **B** — definição de governança.  
3. **A** — plano diretor da SGTI.  
4. **A** — COBIT × ITIL.  
5. **B** — modelagem.  
6. **B** — definição de mudança.  
7. **A** — liberação no ambiente.  
8. **B** — conteúdo típico do portal.  
9. **B** — TIC × ética.  
10. **A** — braço de TIC.  
11. **C** — PDTIC não é imutável.  
12. **A** — ENTIC-JUD.  
13. **A** — serviços na JT.  
14. **B** — catálogo vivo.  
15. **B** — cartão mental.

---

## 3. Questões de concurso (16–25)

> Sem questões específicas do Modelo TRT4 em prova FCC local: usam-se questões **reais** FCC/FGV sobre ENTIC-JUD, PDTIC, Res. CNJ 370/2021 e correlatas de tribunais.

**16.** (FCC – TRT-18/2023 – Analista Judiciário – TI)  
De acordo com o artigo 11 da Resolução CNJ nº 370/2021, será aplicado periodicamente o Índice de Governança, Gestão e Infraestrutura de Tecnologia da Informação e Comunicação do Poder Judiciário (iGovTIC-JUD), que tem por finalidade

A) realizar a medição da maturidade em TIC dos órgãos submetidos ao controle administrativo e financeiro do CNJ.  
B) disseminar as boas práticas das iniciativas de TIC nos órgãos ligados ao CNJ e realizar a medição das principais entregas, avanços e realizações da área de TIC.  
C) promover a melhoria da governança, da gestão e da colaboração tecnológica no âmbito do Poder Judiciário, visando maximizar os resultados com a medição relativa à otimização do uso dos recursos de TIC.  
D) apoiar o estabelecimento de estratégias, indicadores e metas institucionais; realizar a medição dos riscos da área de TIC; permitir que os investimentos tecnológicos no âmbito institucional fiquem dentro das metas definidas.  
E) permitir que sejam medidas as ações relacionadas à transformação digital de serviços, à integração de canais digitais, à interoperabilidade de sistemas e à estratégia de monitoramento.

**17.** (FCC – TRT-19/2022 – Técnico Judiciário – TI)  
O Art. 2º da Resolução CNJ nº 370/2021 estabelece que a Estratégia Nacional de Tecnologia da Informação e Comunicação do Poder Judiciário é orientada, em seu preâmbulo, nos componentes: objetivos estratégicos e metas.  
Os objetivos estratégicos, são distribuídos nas perspectivas:

A) processos internos; aprendizado em tecnologia; recursos tecnológicos.  
B) processos organizacionais; cultura; sociedade.  
C) eventos de aprendizado; sociedade; atendimento ao cidadão.  
D) processos internos; aprendizado e crescimento; sociedade.  
E) eventos organizacionais; aprendizado e crescimento; recursos humanos.

**18.** (FGV – TJ-RJ/2026 – Analista Judiciário – Gestão de TIC)  
A Estratégia Nacional de Tecnologia da Informação e Comunicação do Poder Judiciário (ENTIC-JUD) é o principal instrumento de promoção da governança ágil e da transformação digital do Poder Judiciário. De acordo com a ENTIC-JUD, cada órgão do Poder Judiciário deverá elaborar e manter o Plano Diretor de Tecnologia da Informação e Comunicação (PDTIC). De acordo com o previsto na Resolução CNJ nº 370/2021, é correto afirmar que:

A) as aquisições de bens e a contratação de serviços de Tecnologia da Informação e Comunicação (TIC) deverão atender às determinações do Tribunal de Contas da União;  
B) as despesas de Tecnologia da Informação e Comunicação (TIC) terão preferência sobre as demais quando houver determinação expressa da administração do Tribunal;  
C) os órgãos que possuem em seu planejamento o Plano Estratégico de Tecnologia da Informação e Comunicação (PETIC) poderão continuar a utilizar esse instrumento até o término de sua vigência;  
D) os órgãos do Poder Judiciário deverão constituir ou manter um Comitê de Governança de Tecnologia da Informação e Comunicação composto por representantes da área de Tecnologia da Informação e Comunicação (TIC);  
E) as áreas de TIC de todos os órgãos do Poder Judiciário deverão constituir ou manter Comitê de Gestão de Tecnologia da Informação e Comunicação, coordenado pelo titular da área de Tecnologia da Informação e Comunicação (TIC).

**19.** (FGV – TJ-RJ/2026 – Analista Judiciário – Gestão de TIC)  
A Estratégia Nacional de Tecnologia da Informação e Comunicação do Poder Judiciário (ENTIC-JUD) tem como meta promover a melhoria da governança, da gestão e da colaboração tecnológica no âmbito do Poder Judiciário. As áreas de Tecnologia da Informação e Comunicação (TIC) de todos os órgãos do Poder Judiciário deverão constituir ou manter um Comitê de Gestão de Tecnologia da Informação com a atribuição de:

A) definir a carteira de projetos e a gestão de portfólio de serviços de TIC;  
B) aprovar planos estratégicos, disseminando a importância da área de TIC nos tribunais;  
C) envolver a área de negócio nas decisões estratégicas que incidem sobre os serviços de TIC;  
D) estruturar escritório de projetos que favoreça o emprego de práticas de gestão de projetos especificadas pelo tribunal;  
E) estabelecer Plano de Estratégico de Tecnologia da Informação e Comunicação (PETIC) para iniciativas de curta duração ou escopo simplificado.

**20.** (FCC – TRT-3/MG – Analista Judiciário – TI; Res. CNJ 182/2013)  
Na Gestão de Aquisições e Contratos de TI, quanto ao plano e planejamento das contratações, a Resolução CNJ nº 182/2013 recomenda que as contratações sejam precedidas de Planejamento elaborado em harmonia com o Planejamento Estratégico

A) de Tecnologia da Informação e Comunicação alinhado com os requisitos de infraestrutura estabelecidos pela Administração de TI.  
B) do Poder Judiciário, alinhado com o Planejamento Estratégico Governamental.  
C) Institucional ou Planejamento Estratégico de Tecnologia da Informação e Comunicação, alinhados com o Planejamento Estratégico do Poder Judiciário.  
D) Institucional e com os requisitos de infraestrutura estabelecidos pela Administração de TI, alinhados com o Planejamento Estratégico Governamental.  
E) do Poder Judiciário e com o Planejamento Estratégico de Tecnologia da Informação, alinhados com o Planejamento Estratégico Governamental.

**21.** (FCC – TRT-9/2015 – Analista Judiciário – TI)  
De acordo com a Resolução CNJ nº 90/2009, os tribunais devem constituir quadro de pessoal permanente de profissionais da área de TIC. Desta forma, um Analista da área de TI que atua no Tribunal Regional do Trabalho da 9ª Região, em observância a esta e às Resoluções CNJ nº 99/2009 e 182/2013 sabe que

A) devem existir links de transmissão entre as unidades e o Tribunal suficientes para suportar o tráfego de dados e informações e garantir a disponibilidade exigida pelos aplicativos, sendo o mínimo de 500 Kbps para download.  
B) não poderá ser objeto de contratação o suporte técnico aos processos de planejamento e avaliação da qualidade da Solução de Tecnologia da Informação e Comunicação.  
C) o planejamento das contratações de Solução de Tecnologia da Informação e Comunicação, não pode incluir a terceirização de nenhum tipo de atividade.  
D) o PETIC, no âmbito judiciário, tem entre seus objetivos estratégicos promover a capilaridade dos sistemas e serviços, bem como desenvolver sistemas de TIC interoperáveis e portáveis.  
E) deverão ser definidos processos para gestão dos ativos de infraestrutura de TIC do Tribunal, de acordo com as melhores práticas preconizadas pelo PMBOK 4ª e 5ª edições.

**22.** (CEBRASPE – STM/2025 – Analista Judiciário – Análise de Sistemas)  
Com base nas Resoluções CNJ nº 370/2021 e nº 468/2022, assinale a afirmativa **correta**.

A) As contratações de soluções de TIC por órgãos do Poder Judiciário dispensam plano de contratações de STIC.  
B) O plano de contratações de STIC deve estar alinhado ao PDTIC, ao planejamento estratégico institucional, à Estratégia Nacional do Poder Judiciário e à ENTIC-JUD.  
C) O PDTIC substitui a ENTIC-JUD no âmbito nacional.  
D) A Resolução CNJ 370/2021 trata exclusivamente de contratações de STIC, e não de estratégia de TIC.  
E) O alinhamento do plano de contratações à ENTIC-JUD é facultativo.

**23.** (CEBRASPE – STM/2025 – Analista Judiciário – Suporte em TI)  
A Resolução CNJ nº 370/2021 estabelece que a Estratégia Nacional de TIC do Poder Judiciário (ENTIC-JUD) deve ser implementada com base em princípios de governança e gestão de TIC alinhados às diretrizes estratégicas do CNJ, ao passo que a Resolução CNJ nº 468/2022 e o Guia de Contratações de STIC reforçam a necessidade de transparência e eficiência nas aquisições de TIC.

A) Certo  
B) Errado  
C) Certo apenas quanto à 370/2021  
D) Certo apenas quanto à 468/2022  
E) Errado, pois a 370/2021 trata só de assédio

**24.** (CEBRASPE – STM/2025 – Analista Judiciário – Análise de Sistemas)  
A Estratégia Nacional de TIC do Poder Judiciário (ENTIC-JUD) determina que o plano de transformação digital seja elaborado pela unidade competente dos órgãos, respeitadas suas especificidades, e que seja aprovado pelo Comitê de Gestão de TIC.

A) Certo  
B) Errado  
C) Certo, se houver PDTIC  
D) Certo, se houver PETIC  
E) Errado apenas na Justiça Eleitoral

**25.** (FCC – PMSP/2025 – Analista TIC)  
A equipe de Analistas de uma Prefeitura Municipal implementou um novo sistema de gerenciamento eletrônico de documentos para melhorar a eficiência dos processos internos e a transparência no atendimento ao cidadão. Após a implementação, foi identificado que várias unidades administrativas utilizavam padrões diferentes para nomear e arquivar os documentos, dificultando o acesso às informações e ao controle centralizado. Com base no COBIT 2019, a ação alinhada aos princípios de governança e gestão para resolver o problema identificado é

A) realizar treinamentos sobre a operação do sistema com todos os servidores, destacando os benefícios do novo sistema para aumentar a adesão.  
B) atualizar os contratos com os fornecedores do sistema para incluir cláusulas de suporte técnico mais rigorosas.  
C) estabelecer e implementar políticas e procedimentos padronizados para a classificação, nomeação e arquivamento de documentos, garantindo alinhamento com os objetivos organizacionais.  
D) delegar a cada unidade administrativa a responsabilidade de definir seus próprios padrões de arquivamento, respeitando as especificidades locais.  
E) contratar consultores externos para realizar auditorias periódicas do uso do sistema em cada unidade administrativa.

---

### Gabarito comentado (concurso 16–25)

| Q | Gab. | Fonte |
|---|------|-------|
| 16 | **A** | FCC TRT-18/2023 Analista TI — art. 11: iGovTIC-JUD = medição de maturidade |
| 17 | **D** | FCC TRT-19/2022 Técnico TI — perspectivas: processos internos; aprendizado e crescimento; sociedade |
| 18 | **E** | FGV TJ-RJ/2026 Analista Gestão TIC — art. 8: Comitê de Gestão coordenado pelo titular da TIC |
| 19 | **A** | FGV TJ-RJ/2026 Analista Gestão TIC — atribuição do Comitê de Gestão: carteira/portfólio |
| 20 | **C** | FCC TRT-3 (MG) Analista TI — Res. 182/2013: PEI ou PETIC alinhados ao PE do PJ |
| 21 | **D** | FCC TRT-9/2015 Analista TI — PETIC: capilaridade + interoperabilidade/portabilidade |
| 22 | **B** | CEBRASPE STM/2025 — plano de contratações STIC alinhado a PDTIC/PEI/Estratégia PJ/ENTIC |
| 23 | **A** | CEBRASPE STM/2025 — item certo: 370 (governança/ENTIC) + 468/Guia (contratações) |
| 24 | **B** | CEBRASPE STM/2025 — aprovação do plano de transformação digital = autoridade máxima (não Comitê de Gestão) |
| 25 | **C** | FCC PMSP Analista TIC/2025 — políticas/procedimentos padronizados (governança/gestão; encaixa modelo local) |

---

*Arquivo 10 · Modelo de Governança de TIC do TRT4 · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 4 — gestão/governança.*

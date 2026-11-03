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

## 3. Questões estilo concurso FCC (16–25)

**16.** O Modelo de Governança de TIC do TRT4 é corretamente caracterizado como:

A) substituto integral da Declaração Universal dos Direitos Humanos  
B) conjunto institucional de diretrizes, estruturas, processos e controles que alinham a TIC à missão e às metas do tribunal, documentado sobretudo via Portal/PDTIC/processos  
C) ato exclusivo de sustentabilidade ambiental idêntico à Res. CNJ 400/2021  
D) Código de Conduta e Valores Éticos  
E) framework que extingue COBIT e ITIL  

**17.** No vocabulário das portarias que regulamentam processos de TIC no TRT4, “Governança de TIC” associa-se a:

A) apenas marketing institucional sem processos  
B) diretrizes, estruturas organizacionais, processos de trabalho e mecanismos de controle voltados ao alinhamento institucional do uso de TIC  
C) proibição de qualquer indicador  
D) exclusividade de interpretação literal de crase  
E) identidade total com a Res. CSJT 360/2023  

**18.** Acerca do PDTIC 2024–2026, assinale a alternativa correta.

A) Trata-se do Plano Diretor de TIC que orienta planejamento e operação da SGTI, com acompanhamento de ações, orçamento e níveis de serviço, podendo ser revisado  
B) É norma de assédio moral do CNJ  
C) Revoga a Constituição  
D) Equivale ao Swagger  
E) Não dialoga com estratégias do CNJ/CSJT  

**19.** Considere as afirmações abaixo.

I. O modelo local apoia-se em boas práticas como COBIT e ITIL, além de normativos CNJ/CSJT.  
II. BPMN é tipicamente usado para representar processos.  
III. O Modelo de Governança de TIC é idêntico, para todos os efeitos, ao Código de Conduta do TRT4.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**20.** Assinale a opção **incompatível** com o processo de Gerenciamento de Mudanças e Liberações.

A) Mudanças devem ser planejadas, registradas, autorizadas, implantadas e revisadas  
B) Liberação aplica mudança no ambiente  
C) Qualquer alteração informal sem registro basta, pois governança veda processos  
D) Há etapas de abertura, análise/aprovação, liberação e fechamento  
E) O processo maximiza disponibilizações bem-sucedidas  

**21.** Em prova de Analista Judiciário – TI, confundir o Modelo de Governança de TIC do TRT4 com o PMBOK 8 é inadequado porque:

A) ambos são rigorosamente o mesmo texto do PMI  
B) o modelo TRT4 organiza governança/gestão local de TIC (portal, PDTIC, processos), ao passo que o PMBOK é guia genérico de gerenciamento de **projetos**  
C) o PMBOK é o Regimento Interno  
D) o modelo TRT4 extingue projetos  
E) nenhum aparece no edital  

**22.** A existência de Catálogo de Serviços no portal de governança visa, prioritariamente:

A) impedir qualquer atendimento a usuários  
B) manter serviços catalogados/atualizados para gestão e categorização adequada de demandas  
C) substituir a LGPD  
D) criar a PDPJ-Br  
E) aprovar a DUDH  

**23.** A alternativa que corretamente separa eixos é:

A) Modelo TIC TRT4 = governança local de TIC; Conduta = ética; CNJ 400 = sustentabilidade; PMBOK = projetos  
B) Modelo TIC = Conduta = 400 = PMBOK  
C) Modelo TIC = apenas Selenium  
D) Modelo TIC = CSJT 360  
E) Modelo TIC = interpretação de texto  

**24.** Um servidor afirma que “PDTIC é documento engavetado, sem revisão e sem relação com orçamento ou níveis de serviço”. Essa leitura:

A) está correta, pois o PDTIC veda indicadores  
B) está correta apenas fora do Judiciário  
C) está incorreta, pois o PDTIC é instrumento vivo de planejamento/acompanhamento, inclusive orçamentário e de serviços  
D) está correta, pois SGTI não planeja  
E) está incorreta somente porque o nome correto seria Eureka  

**25.** Para fins de preparação na semana 4, a síntese mais adequada do Modelo de Governança de TIC do TRT4 é:

A) Conduta + 351 + 360 sem processos de TIC  
B) governança local (diretrizes/estruturas/processos/controles) + Portal + PDTIC + encaixe COBIT/ITIL/BPMN, cobrada em Específicas, sem confundir com Conduta, 400 ou PMBOK puro  
C) apenas Portaria de crase  
D) apenas monólito sem SGTI  
E) extinção do catálogo de serviços  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **B** |
| 17 | **B** |
| 18 | **A** |
| 19 | **C** |
| 20 | **C** |
| 21 | **B** |
| 22 | **B** |
| 23 | **A** |
| 24 | **C** |
| 25 | **B** |

**Notas rápidas:** 16 B (modelo local); 17 B (definição); 18 A (PDTIC); 19 C (I e II; III = Conduta); 20 C (informalidade veda o processo); 21 B (local TIC × projetos); 22 B (catálogo); 23 A (separação); 24 C (PDTIC vivo); 25 B (síntese).

---

*Arquivo 10 · Modelo de Governança de TIC do TRT4 · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 4 — gestão/governança.*

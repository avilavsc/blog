+++
title = '03 — Modelo de Governança de TIC do TRT4 — reforço · Analista TI (código 14)'
date = '2026-10-28'
draft = false
weight = 3
tags = ['semana-10', 'governanca']
categories = ['estudo']
+++
# Modelo de Governança de TIC do TRT4 — reforço · Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — Modelo de Governança de TIC do TRT4 (reforço)  
**Escopo deste arquivo:** aprofundar o **Modelo / Portal de Governança de TIC** do TRT4; ligação **ENTIC-JUD (CNJ 370)** ↔ modelo local; governança × gestão; COBIT/ITIL/BPMN no encaixe; estruturas (SGTI, CGTIC/Subcomitê, CSIPD); processos publicados; PGSERV-TIC (CSJT 397); iGovTIC-JUD; RI art. 227-A; anti-confusão reforçada vs semana 4.  
**Fora de escopo aqui:** MoReq detalhado (01); PDTIC indicadores linha a linha (02 — usar em paralelo); ITIL/COBIT página a página (semana 4 arquivos 07–08).

**Pré-requisito:** `/cursor/stores/self/docs/semana-4/10-governanca-tic-trt4.md` (mapa inicial). Este arquivo **reforça e aprofunda**, não substitui aquele stub.

---

## 1. Material

### 1.1 Por que reforçar na semana 10

Semana 4 deu o **mapa**. Semana 10 fecha o bloco **MoReq + PDTIC + Governança** como a FCC costuma misturar: “modelo local + estratégia nacional + plano diretor”. O erro clássico é estudar só COBIT genérico e esquecer o **Portal TRT4** / **SGTI** / **processos portariados**.

| Camada | Instrumento | Papel |
|--------|-------------|--------|
| Nacional | **ENTIC-JUD (370)** | Estratégia + PDTIC + comitês + iGov |
| JT | **CSJT 397/2024** (PGSERV-TIC) | Política de gerenciamento de **serviços** 1º/2º |
| Local | **Modelo/Portal Governança TIC TRT4** | Diretrizes, estruturas, processos, indicadores, PDTIC |
| Guias | **COBIT 2019 · ITIL 5 (edital) · BPMN · PMBOK 8** | Referências — não substituem o modelo local |

### 1.2 Definição cobrável (reforço)

> **Governança de TIC:** conjunto de **diretrizes**, **estruturas organizacionais**, **processos de trabalho** e **mecanismos de controle** que asseguram que decisões e ações de gestão/uso de TIC estejam **alinhadas às necessidades institucionais** e contribuam para a **missão** e as **metas** da instituição.

**Governança** (direcionar/avaliar/monitorar o alinhamento) × **Gestão** (planejar/construir/executar/monitorar serviços e projetos no dia a dia).  
COBIT dialoga com a primeira; ITIL com a segunda — o modelo TRT4 **usa ambos**, não escolhe um.

### 1.3 ENTIC-JUD → modelo local (ligação obrigatória)

A Res. CNJ **370/2021** exige, entre outros:

1. **PDTIC** alinhado a PEI + Estratégia Nacional do PJ + ENTIC.  
2. **Comitê de Governança de TIC** multidisciplinar (estratégia, projetos, riscos…).  
3. **Comitê de Gestão de TIC** nas áreas de TIC, coordenado pelo **titular da TIC**.  
4. Aplicação periódica do **iGovTIC-JUD** (maturidade).  
5. Perspectivas dos objetivos: **processos internos · aprendizado e crescimento · sociedade**.

O Portal/Modelo TRT4 é a **materialização local** desse dever: publica organização, serviços, planos (PDTIC), projetos, indicadores, orçamento e processos.  
Não inventa governança “do zero”: apoia-se em COBIT/ITIL/BPMN + CNJ/CSJT/TCU.

Flash: **370 manda a estratégia; o modelo TRT4 opera a governança no tribunal**.

### 1.4 Portal — mapa mental aprofundado

| Bloco | O que a prova espera reconhecer |
|-------|----------------------------------|
| Organização | SGTI e unidades (desenvolvimento, IA, segurança, serviços…) |
| Serviços/produtos | Catálogo vivo; classificação de chamados |
| Planos / PDTIC | Ciclo 2 anos + revisão; não estático |
| Projetos | Portfólio, priorização, alinhamento ENTIC/CSJT, transformação digital |
| Indicadores | Satisfação, iGov, ISO 27002, orçamento, PAC, SLA, projetos, benefícios |
| Orçamento / contratações | Plano de aquisições alinhado ao PDTIC |
| Processos | Modelagem, mudanças/liberações, problemas, continuidade, catálogo… |

Transparência + processos publicados = sinal de maturidade (iGov).

### 1.5 Estruturas e papéis (aprofundar)

| Papel / instância | Ideia cobrável |
|-------------------|----------------|
| **Presidência / Alta Administração** | Aprova planos/processos relevantes; interesse na atividade-fim |
| **SGTI** | Secretaria-Geral de Tecnologia e Inovação — executa/entrega |
| **CGTIC / Comitê de Governança** | Instância de governança (RI TRT4 art. **227-A**: PDTIC, estratégias, PSI…) |
| **Subcomitê de TIC** | Articulação técnica; proposições |
| **Comitê de Gestão (visão 370)** | Carteira/portfólio; coordenado pelo titular da TIC |
| **CSIPD** | Segurança da informação e proteção de dados |
| **Divisão de Conformidade e Processos** | Modelagem BPMN, tramitação, publicação no portal |

Não decore nomes de pessoas; decore a **cadeia de decisão**.

### 1.6 Processos exemplares (mais densidade)

| Processo | Flash FCC |
|----------|-----------|
| **Modelagem e Gestão de Processos** | Planejar → Executar → Regulamentar; publicar; revisão |
| **Mudanças e Liberações** | Mudança = alterar item de configuração; Liberação = aplicar no ambiente; fluxo Abertura → Análise/aprovação → Liberação → Fechamento |
| **Catálogo de Serviços** | Manter atualizado; categorizar; aprovações (Subcomitê → Comitê → Presidência) |
| **Problemas** | Causa raiz; reduzir recorrência (ITIL) |
| **Continuidade** | Disponibilidade/recuperação de serviços críticos |
| **Elaboração/Revisão do PDTIC** | Diagnóstico → plano → monitoração de indicadores |

**Mudança** ≠ ideia informal. **Liberação** ≠ “commit sem aprovação”.

### 1.7 PGSERV-TIC e CSJT 397/2024

Na Justiça do Trabalho de 1º/2º graus, a política de **gerenciamento de serviços de TIC** (PGSERV-TIC) amarra o discurso local ao CSJT: catálogo, níveis de serviço, práticas de serviço — dialoga com ITIL e com o Portal.

Distrator: “PGSERV = Código de Conduta” ou “= MoReq”.

### 1.8 iGovTIC-JUD no modelo local

Finalidade (art. 11 da 370): **medição da maturidade** em TIC dos órgãos sob controle do CNJ.  
No TRT4, o indicador entra na monitoração do PDTIC (meta local publicada no portal).  
Respostas ao levantamento devem ter **evidências** e ficar disponíveis a auditorias.

Distratores FCC trocam a finalidade por “disseminar boas práticas”, “só transformação digital”, “só riscos” etc.

### 1.9 COBIT × ITIL × BPMN × PMBOK (anti-fusão)

| Guia | No modelo TRT4 |
|------|----------------|
| **COBIT 2019** | Governança/gestão de I&T; alinhamento e controle |
| **ITIL 5** (edital) / ITIL 4 (atos) | Serviços, valor, práticas operacionais |
| **BPMN** | Notação dos processos publicados |
| **PMBOK 8** | Projetos do portfólio — **não** é o modelo de governança |

Armadilhas:

1. “Modelo TRT4 = só PMBOK”  
2. “Governança TIC = Conduta / 400 / 351”  
3. “Portal = PDPJ nacional”  
4. “COBIT substitui a necessidade de PDTIC”  
5. “ITIL elimina Comitê de Governança”

### 1.10 Regimento Interno — ponto 227-A (ponte)

O RI do TRT4 institucionaliza o **Comitê de Governança de TIC** (CGTIC): participação em estratégias/indicadores/metas, **PDTIC**, investimentos, soluções, aquisições fora de padrão, opinião sobre **PSI**.  
Para o Analista TI: saber que o modelo **não é só portal** — tem ancoragem regimental.

### 1.11 O que a FCC tipicamente testa (checklist semana 10)

- Definição de governança de TIC (4 elementos).  
- 370: PDTIC, comitês, iGov, perspectivas.  
- Modelo local = Portal + processos + SGTI.  
- COBIT (gov) × ITIL (serviço) × BPMN (modelo) × PMBOK (projeto).  
- Mudança × liberação; catálogo vivo.  
- Separação Conduta / 400 / PDPJ / MoReq.  
- Que TIC serve à **prestação jurisdicional**.

### 1.12 Mini-síntese (cartão reforço)

```
370 ENTIC → PDTIC + Comitês + iGov
     ↓
Modelo TRT4 (Portal) = diretrizes + estruturas + processos + controle
     ↓
SGTI opera · CGTIC governa · CSIPD segura · BPMN publica
COBIT ⊃ alinhamento · ITIL ⊃ serviços · PMBOK ⊃ projetos
≠ Conduta ≠ MoReq ≠ PDPJ-plataforma
```

---

## 2. Questões de treino (1–15)

**1.** Em relação à semana 4, este reforço enfatiza sobretudo:

A) Apenas interpretação de texto  
B) Ligação ENTIC-JUD ↔ modelo local + processos/indicadores  
C) Somente crase  
D) Exclusivamente cabeamento  
E) Extinção do PDTIC  

**2.** Governança de TIC, definição cobrável, inclui:

A) Só compra de hardware sem alinhamento  
B) Diretrizes, estruturas, processos e controles alinhados às necessidades institucionais  
C) Apenas o Código de Conduta  
D) Só a Res. CNJ 400  
E) Proibição de indicadores  

**3.** A Res. CNJ 370/2021, no encaixe com o modelo TRT4:

A) É irrelevante  
B) Exige PDTIC/comitês/iGov que o portal local materializa  
C) Substitui o Regimento Interno  
D) É idêntica ao MoReq-Jus  
E) Trata só de assédio  

**4.** Contraste correto COBIT × ITIL no modelo:

A) Ambos são bancos H2  
B) COBIT tipicamente governança/controle; ITIL tipicamente serviços  
C) COBIT = Conduta; ITIL = 351  
D) Nenhum dialoga com o TRT4  
E) ITIL certifica SGSI ISO 27001  

**5.** BPMN, no Portal TRT4, serve principalmente para:

A) Criptografar discos  
B) Modelar/representar processos de TIC publicados  
C) Substituir Keycloak  
D) Revogar a ENTIC  
E) Definir crase  

**6.** “Liberação”, no processo de mudanças:

A) Demitir a SGTI  
B) Aplicar a mudança planejada no ambiente computacional  
C) Criar a PDPJ-Br  
D) Aprovar a DUDH  
E) Extinguir o catálogo  

**7.** iGovTIC-JUD tem por finalidade, na 370:

A) Medir maturidade em TIC dos órgãos sob CNJ  
B) Substituir o PDTIC  
C) Definir senhas Wi-Fi  
D) Criar o MoReq  
E) Revogar o COBIT  

**8.** PGSERV-TIC (CSJT 397/2024) relaciona-se a:

A) Política de gerenciamento de serviços de TIC na JT 1º/2º  
B) Código de Conduta  
C) DUDH  
D) Apenas MapStruct  
E) Interpretação de texto  

**9.** O Portal de Governança de TIC **não** costuma publicar:

A) PDTIC e indicadores  
B) Processos de mudanças/catálogo  
C) Receitas culinárias da cantina como eixo de TIC  
D) Serviços/produtos  
E) Portfólio de projetos  

**10.** Art. 227-A do RI (CGTIC), ponto TI cobrável:

A) Participar de estratégias/PDTIC e opinar sobre PSI  
B) Substituir o STF  
C) Extinguir a SGTI  
D) Criar a LGPD  
E) Definir apenas WEP  

**11.** Assinale a inadequada:

A) Modelo TRT4 apoia-se em boas práticas (COBIT/ITIL/BPMN)  
B) PDTIC é instrumento-chave do portal  
C) Modelo TRT4 = sinônimo de Conduta ética  
D) SGTI opera serviços alinhados à estratégia  
E) Catálogo deve estar atualizado/publicado  

**12.** Perspectivas dos objetivos estratégicos da ENTIC (FCC clássica):

A) Processos internos; aprendizado e crescimento; sociedade  
B) Apenas DNS, DHCP, LDAP  
C) Só Scrum, Kanban, XP  
D) EDM, APO, BAI apenas como “perspectivas ENTIC”  
E) WEP, WPA, WPA2  

**13.** Comitê de Gestão de TIC (370), coordenação:

A) Pelo titular da área de TIC  
B) Exclusivamente pela OAB  
C) Pelo cartório extrajudicial  
D) Pelo Service Desk municipal  
E) Pelo autor do PMBOK  

**14.** Distinção correta:

A) Modelo TIC TRT4 = organização/processos/controle de TIC; Conduta = ética/valores  
B) Conduta = PDTIC  
C) Modelo = CNJ 360  
D) Modelo = apenas ENSEC  
E) Não há diferença possível  

**15.** Síntese reforçada:

A) 370 → PDTIC/comitês/iGov · Portal TRT4 materializa · COBIT/ITIL/BPMN · SGTI/CGTIC · ≠ Conduta/MoReq/PDPJ-plataforma  
B) Governança TIC = só 400  
C) Portal substitui a ENTIC nacional  
D) Mudança = opinião sem registro  
E) PMBOK elimina necessidade de catálogo  

---

### Gabarito comentado (treino 1–15)

1. **B** — Ligação nacional↔local.  
2. **B** — Definição.  
3. **B** — 370 materializada no portal.  
4. **B** — COBIT × ITIL.  
5. **B** — Modelagem.  
6. **B** — Liberação no ambiente.  
7. **A** — Maturidade.  
8. **A** — Serviços JT.  
9. **C** — Fora do portal.  
10. **A** — 227-A.  
11. **C** — Inadequada (≠ Conduta).  
12. **A** — Perspectivas.  
13. **A** — Titular da TIC.  
14. **A** — TIC × ética.  
15. **A** — Cartão reforço.

---

## 3. Questões de concurso (16–25)

> Questões **reais** correlatas (ENTIC, PDTIC, iGov, comitês, governança). Gabarito só no final.

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
|---|------|----------------|
| 16 | **A** | FCC TRT-18/2023 — iGov = maturidade |
| 17 | **D** | FCC TRT-19/2022 — perspectivas |
| 18 | **E** | FGV TJ-RJ/2026 — Comitê de Gestão / titular TIC |
| 19 | **A** | FGV TJ-RJ/2026 — carteira/portfólio |
| 20 | **B** | CEBRASPE STM/2025 — plano STIC alinhado |
| 21 | **A** | CEBRASPE STM/2025 — item certo (370 + 468) |
| 22 | **B** | CEBRASPE STM/2025 — aprovação = autoridade máxima (não Comitê de Gestão) |
| 23 | **C** | FCC TRT-3 — PEI ou PETIC alinhados ao PE do PJ |
| 24 | **B** | FGV TJ-MS/2024 — diagnóstico |
| 25 | **C** | FCC PMSP/2025 — políticas padronizadas (governança) |

---

## 4. Como usar este arquivo

1. Releer semana 4 (`10-governanca-tic-trt4.md`) em 20 min.  
2. Fechar este reforço: **370 ↔ Portal** · **gov×gestão** · **processos** · **227-A**.  
3. Treino 1–15 → gabarito.  
4. Concurso 16–25 cruzando com `02-pdtic`.  
5. Flash final: Modelo local ≠ Conduta ≠ MoReq ≠ PDPJ-plataforma.

---

*Arquivo 03 · Governança TIC TRT4 (reforço) · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 10 — governança.*

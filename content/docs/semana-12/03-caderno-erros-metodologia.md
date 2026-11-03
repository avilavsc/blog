+++
title = '03 — Caderno de erros — metodologia + reteste misto TI'
date = '2026-10-29'
draft = false
weight = 3
tags = ['semana-12', 'caderno']
categories = ['estudo']
+++
# Caderno de erros — metodologia + reteste misto TI · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — Semana 12 · revisão ativa  
**Escopo deste arquivo:** método para **classificar · revisitar · retestar** erros; ~15 treinos de meta-estudo/aplicação; ~10 REAL Qs mistos de TI citadas (reteste).  
**Fora de escopo aqui:** reescrever packs de conteúdo (arquivos 01–02); matéria nova; Português/interpretação.

**Priors:** mapa vermelho `docs/semana-11/05`; high-yield semanas 3–4 e 9; preferência ≥80 questões/semana + caderno.

---

## 1. Material — método do caderno de erros

### 1.1 Por que o caderno (e não “refazer a semana inteira”)

Na Semana 12 o cronograma pede **revisão ativa: resumos + questões dos erros**. O caderno é o filtro: você só reabre o que ainda falha, com **contraste FCC**, não o PDF/pack completo.

Meta operacional Claudionir: **prioridade > cobertura**; cada erro vira 1 cartão retestável até ≥2 acertos seguidos (ou 1 acerto no simulado + 1 no reteste).

### 1.2 O que registrar (ficha mínima)

Para **cada** erro (treino ou REAL), uma linha/ficha:

| Campo | Exemplo |
|-------|---------|
| **Data / fonte** | 12/01 · Sem12-01 Q14 · FCC TRF4/2025 Q22 |
| **Tema âncora** | Eureka × Gateway |
| **Tipo de erro** | (ver §1.3) |
| **Distrator que engoliu** | “Eureka = ponto único de entrada” |
| **Âncora correta (1 frase)** | Eureka = registry; Gateway/Zuul = borda |
| **Tag de reteste** | `P0` / `P1` / `P2` |
| **Próximo reteste** | +1 dia / +3 dias / pós-simulado |
| **Status** | Aberto · Em cura · Travado (≥2 acertos) |

Não copie o enunciado inteiro. Copie o **contraste**.

### 1.3 Como classificar o erro (taxonomia FCC)

Use **uma** etiqueta principal (a causa raiz, não o sintoma):

| Código | Tipo | O que é | Ação |
|--------|------|---------|------|
| **INV** | Inversão de papéis | Eureka↔Zuul; Incident↔Problem; IDS↔IPS; Flyway↔Envers | Cartão de 2 colunas “é / não é” |
| **FAM** | Família/classe errada | TRUNCATE=DML; GRANT=DML; 802.1D=VLAN | Tabela de classificação |
| **NOR** | Norma/número trocado | 27002 certifica; ENSEC=162; 27701=LGPD | Flash número→função |
| **SIN** | Sintaxe/comando | `REVOKE ALL PERMISSIONS`; `@ZuulServerApplication` | Decore a forma cobrada |
| **LEC** | Leitura incompleta | Alternativa longa com 1 pedaço errado | Marcar verbos (“não”, “apenas”, “sempre”) |
| **MEM** | Porta/campo/opção | 8761; 67/68; opção DHCP 6; Flow Label | Micro-drill 30 s |
| **CTX** | Contexto de cargo | Técnico×Analista; legado COBIT×2019 | Anotar banca/ano |

Se errou a mesma questão duas vezes com tipos diferentes, mantenha o **mais estável** (quase sempre INV ou FAM).

### 1.4 Prioridade P0 / P1 / P2

| Tag | Critério | Ritmo |
|-----|----------|-------|
| **P0** | Tema high-yield TRT4/FCC + erro recorrente (PDPJ, SQL, redes, segurança, ITIL/COBIT) | Reteste **D+1** e **D+3** |
| **P1** | Erro isolado em tema cobrável | Reteste na sexta + no 6º simulado |
| **P2** | Distrator raro / questão antiga fora do radar | Só se sobrar tempo |

Semana 12: **não** abrir P2 enquanto houver P0 aberto.

### 1.5 Como revisitar (sem releitura passiva)

1. Cubra a âncora correta.  
2. Diga em voz alta: “é X; **não** é Y”.  
3. Só então releia a frase âncora.  
4. Se falhou de novo → reescreva a âncora com **menos palavras** (máx. 12).  
5. Não “estude o pack” inteiro por causa de 1 ficha.

### 1.6 Como retestar (fechamento)

| Gatilho | O que fazer |
|---------|-------------|
| Após revisar 5 fichas P0 | Mini-bloco 5 questões **do mesmo contraste** (treino ou REAL) |
| Sexta da Semana 12 | Reteste todos os P0 abertos |
| 6º simulado (cronograma) | Marque no caderno cada erro novo + reabra P0 que voltaram |
| ≥2 acertos seguidos no contraste | Status **Travado**; some da fila diária |

Regra de ouro: **reteste ≠ reler gabarito**. Refaça a questão (ou irmã) com alternativas cobertas.

### 1.7 Template pronto (copie no caderno físico/digital)

```text
[ ] P0 | INV | Eureka×Gateway | engoli: "Eureka=borda"
âncora: Eureka=registry; Zuul=entrada
reteste: __/__ → __/__ | status: Aberto
```

### 1.8 Checklist de erros típicos **de método** (meta-estudo)

- [ ] Anotar só “errei Q17” sem contraste  
- [ ] Releer o pack inteiro em vez de 1 cartão  
- [ ] Olhar gabarito no meio do bloco REAL  
- [ ] Tratar todo erro como P0 (fila infinita)  
- [ ] Não retestar depois do simulado  
- [ ] Confundir “acertei no treino” com “travado” sem 2º acerto  
- [ ] Misturar PT interpretação forçada (fora da preferência atual)  
- [ ] Pedir bifurcação de caminho (“estudo A ou B?”) — seguir o cobrável  

### 1.9 Ordem nesta sessão

1. Monte/atualize 10–20 fichas dos erros das semanas 11–12 (01–02).  
2. Classifique INV/FAM/… e tague P0–P2.  
3. Faça treino 1–15 (meta-estudo + aplicação).  
4. Reteste REAL 16–25 mistos **sem gabarito**.  
5. Feche fichas travadas; deixe P0 para D+1.

---

## 2. Questões de treino (1–15) — meta-estudo / aplicação

**1.** No caderno, o campo mais útil após errar uma questão FCC de papéis trocados é:

A) Registrar o contraste “é X / não é Y” em uma frase  
B) Copiar o enunciado completo  
C) Listar todas as alternativas sem marcar o distrator  
D) Anotar só o número da questão  
E) Reescrever o pack inteiro da semana  

**2.** Erro “TRUNCATE classificado como DML” deve receber, tipicamente, o código:

A) INV  
B) NOR  
C) CTX  
D) FAM  
E) LEC apenas  

**3.** Erro “Eureka = ponto único de entrada” é tipicamente:

A) FAM  
B) MEM  
C) NOR  
D) CTX  
E) INV  

**4.** Tag **P0** deve ir para:

A) Qualquer erro, inclusive raríssimo  
B) Só questões de 2010  
C) Tema high-yield + erro recorrente (ex.: Gateway×Discovery)  
D) Só Português interpretação  
E) Só quando acertar  

**5.** Ritmo mínimo sugerido para ficha P0 aberta:

A) Só no dia da prova  
B) Uma vez por mês  
C) Nunca retestar REAL  
D) Reteste D+1 e D+3 (além do simulado)  
E) Só releitura passiva do PDF  

**6.** “Travado” no método deste caderno significa, de forma operacional:

A) Errou uma vez e desistiu  
B) Leu o gabarito  
C) Marcou P2  
D) Copiou a questão no WhatsApp  
E) ≥2 acertos seguidos no contraste (ou acerto simulado + reteste)  

**7.** Durante o bloco REAL 16–25, a prática correta é:

A) Abrir o gabarito após cada item  
B) Trocar a alternativa após ver o comentário da internet no meio  
C) Completar o bloco e só então corrigir  
D) Pular todas as longas  
E) Classificar INV antes de escolher a letra  

**8.** Se sobrar tempo na Semana 12 e ainda houver P0 abertos, a prioridade é:

A) Abrir P2 antigos  
B) Matéria nova fora do edital  
C) Reescrever README  
D) Fechar P0 (reteste + âncora curta)  
E) Estudar interpretação de texto por insistência  

**9.** Alternativa longa FCC com um único verbo falso (“sempre”, “apenas”, “exclusivamente”) costuma ser erro tipo:

A) LEC  
B) MEM  
C) NOR  
D) CTX  
E) FAM  

**10.** Ao errar ITIL Incident×Problem, a ação correta no caderno é:

A) Decorar o manifesto ágil inteiro  
B) Estudar só Hyper-V  
C) Marcar P2 e esquecer  
D) Relacionar “restaurar rápido” × “causa raiz” em cartão INV  
E) Trocar ITIL por OWASP  

**11.** Mini-bloco de reteste após 5 fichas P0 deve:

A) Ser sobre temas aleatórios sem relação  
B) Ser só dissertativo  
C) Cobrir o **mesmo contraste** das fichas  
D) Substituir o 6º simulado  
E) Incluir obrigatoriamente Português  

**12.** Registrar “fonte: FCC | TRT-1/2025 | Q55” serve principalmente para:

A) Enfeite  
B) Substituir a âncora  
C) Classificar como P2 automático  
D) Provar memorização de URL  
E) Rastrear reteste e evitar “inventar” enunciado  

**13.** Se o mesmo contraste falha no simulado depois de “travado”, você deve:

A) Reabrir como P0 e encurtar a âncora  
B) Apagar o caderno  
C) Mudar de cargo  
D) Ignorar porque “já tinha travado”  
E) Estudar só Gerais  

**14.** Distinção útil para não inflar o caderno:

A) Todo distrator lido = nova ficha  
B) Só questões que acertou  
C) Só questões sem citação  
D) Só vira ficha o que **você errou** (ou quase errou com dúvida forte)  
E) Só DDL  

**15.** Síntese do método:

A) Cobertura total > prioridade; gabarito no meio; P2 primeiro  
B) Relê packs inteiros a cada erro  
C) Bifurcar sempre “A ou B?” de plano  
D) Inventar bloco 16–25  
E) Classificar causa (INV/FAM/…); priorizar P0; âncora curta; retestar sem olhar; travar com ≥2 acertos  

---

### Gabarito comentado (treino 1–15)

1. **A** — Contraste em 1 frase.  
2. **D** — Família SQL.  
3. **E** — Inversão de papéis.  
4. **C** — High-yield + recorrente.  
5. **D** — D+1 e D+3.  
6. **E** — Critério operacional de cura.  
7. **C** — Gabarito só no final do bloco.  
8. **D** — Fechar P0.  
9. **A** — Leitura / verbo armadilha.  
10. **D** — Cartão INV Incident×Problem.  
11. **C** — Mesmo contraste.  
12. **E** — Rastreio + fidelidade.  
13. **A** — Reabrir P0.  
14. **D** — Só o que falhou/duvidou.  
15. **E** — Método completo.

Letras: A, D, E, C, D, E, C, D, A, D, C, E, A, D, E.

---

## 3. Questões de concurso (reais) (16–25) — temas mistos TI (reteste)

> Questões **reais** FCC (mistura high-yield: ágeis, ITIL, COBIT, SO, AD, K8s, Rabbit, Gitflow, CI). Sem invenção. Gabarito só no final. Use para **retestar** fichas P0/P1 do mapa vermelho e semanas 3–4/9.

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

**20.** (FCC · TRT-14/2022 · Analista Judiciário — TI)  
As Unidades Organizacionais (UOs) no Active Directory podem ser usadas para formar uma hierarquia de contêineres dentro de um domínio. O controle sobre uma UO e os objetos dentro dela determinados

A) pelas listas de controle de acesso na UO e pelos objetos na UO.  
B) pelo mecanismo de consulta e pelo índice.  
C) pelo serviço de replicação que distribui dados de diretório em uma rede.  
D) por um catálogo global que contém informações sobre cada objeto no diretório.  
E) pelo catálogo local de máquina cliente Windows.

**21.** (FCC · TRT-5/2022 · Técnico Judiciário — TI)  
Um técnico deseja que uma política de grupo aplicada ao domínio entre em operação imediatamente em uma máquina com Windows 10, em condições ideais. Para tanto, ele deve utilizar o comando

A) gpresult /sync  
B) gpupdate /sync  
C) gpfixup /force  
D) gpupdate /force  
E) gpresult /f  

**22.** (FCC | TRT14 2022 | Analista Judiciário, Tecnologia da Informação | Questão 27)

Para coordenar e agendar vários contêineres, habilitar comunicações entre eles e escalar aplicações, um Analista pode utilizar o

A) Kubernetes, solução utilizada para vários tipos de contêiner, incluindo Docker.  
B) Swagger, solução exclusiva para contêineres Docker.  
C) Kubernetes, solução exclusiva para contêineres Docker.  
D) Swagger, solução exclusiva para contêineres Zuul.  
E) Keycloak, solução utilizada para qualquer tipo de contêiner.  

**23.** (FCC | TRT-1 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 49)

Em uma aplicação web, um Tribunal Regional do Trabalho decide usar RabbitMQ para gerenciar notificações de processos trabalhistas. A configuração recomendada que garante que mensagens não sejam perdidas em caso de reinício do servidor RabbitMQ é

A) configurar retries automáticos no consumidor.  
B) configurar o TTL das mensagens para infinito.  
C) habilitar filas duráveis e mensagens persistentes.  
D) configurar Webhooks para reenvio.  
E) usar um banco de dados externo para armazenar mensagens.  

**24.** (FCC | TRT15 2023 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 49)

O Gitflow trabalha com dois branches principais: Develop e Master, que são permanentes, e

A) dois branches opcionais, Catalog e Basic, que são contínuos durante todo o fluxo CI/CD.  
B) três branches de suporte, Feature, Release e Hotfix, que são temporários e duram até realizar o merge com os branches principais.  
C) dois branches operacionais, Deploy e Release, que são temporários e duram até realizar o merge com os branches principais.  
D) quatro branches, Hotfix, Feature, Release e Deploy, que são contínuos durante todo o fluxo CI/CD.  
E) três branches de desenvolvimento, Feature, Release e Deploy, que são temporários e duram até realizar o merge com os branches principais.  

**25.** (FCC | TRT20 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 31)

Visando automatizar testes e a entrega de um produto de software, um Analista pode utilizar o

A) Selenium, que é utilizado para automação de builds, testes de integração e execução de pipelines de CI/CD, que integra-se ao CircleCI, que faz a gestão de repositórios Git. O CircleCI é compatível apenas com as linguagens Go, Java e Ruby, apresentando incompatibilidade com Python, Ruby, Scala, Node.js, PHP e Haskell.  
B) Git. Esta ferramenta precisa ser integrada ao GitHub, uma vez que são interdependentes. Usadas em conjunto, conseguem realizar o controle de versão e proporcionar a integração contínua.  
C) GitHub Actions, que permite a criação de workflows de CI/CD diretamente no GitHub. Os workflows são definidos no diretório `.github/workflows` de um repositório, que pode ter vários workflows, cada um dos quais podendo executar um conjunto diferente de tarefas, como criar e testar pull requests.  
D) Jenkins, que é uma ferramenta escrita em PHP, exclusiva para automação de testes, que integra-se ao Git para armazenar o histórico dos testes automatizados executados.  
E) Maven, que é uma ferramenta proprietária para automação de builds, que também possui capacidades nativas de controle de versões e pipelines de CI/CD, similar ao GitLab CI/CD. Um projeto Maven fica atrelado a um arquivo principal: o pom.java.  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Citação |
|---|------|---------|
| 16 | **C** | FCC \| TRT-20 2024 \| Técnico TI \| XP refatoração/testes |
| 17 | **B** | FCC \| TRT-1 2025 \| Analista TI \| Service Configuration |
| 18 | **A** | FCC \| MPE-RN 2010 \| Analista TI \| COBIT PO/DS |
| 19 | **D** | FCC \| TRT-12 2023 \| Analista TI \| starvation + FIFO |
| 20 | **A** | FCC \| TRT-14 2022 \| Analista TI \| ACL na UO |
| 21 | **D** | FCC \| TRT-5 2022 \| Técnico TI \| gpupdate /force |
| 22 | **A** | FCC \| TRT14 2022 \| Analista TI \| Kubernetes |
| 23 | **C** | FCC \| TRT-1 2025 \| Analista TI \| RabbitMQ durable/persistent |
| 24 | **B** | FCC \| TRT15 2023 \| Analista TI \| Gitflow Feature/Release/Hotfix |
| 25 | **C** | FCC \| TRT20 2024 \| Analista TI \| GitHub Actions |

Letras: C, B, A, D, A, D, A, C, B, C.

**Fontes:**
- Q16–Q21: alinhadas a `docs/semana-11/05-mapa-temas-vermelhos.md` (Quizinho / bases TRT)
- Q22: https://www.quizinho.com.br/questao/fcc-trt14-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q27
- Q23: https://www.quizinho.com.br/questao/fcc-trt-1-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q49
- Q24: https://www.quizinho.com.br/questao/fcc-trt15-2023-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2023-q49
- Q25: https://www.quizinho.com.br/questao/fcc-trt20-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q31

---

*Arquivo 03 · Caderno de erros · Semana 12 · Claudionir · TRT4 Analista TI (código 14) · FCC.*

+++
title = '02 — Engenharia de requisitos'
date = '2026-10-16'
draft = false
weight = 2
tags = ['semana-04', 'engenharia', 'requisitos']
categories = ['estudo']
+++
# Engenharia de requisitos · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Engenharia de requisitos (Semana 4 · Engenharia de software)  
**Escopo deste arquivo:** **engenharia de requisitos** cobrável: conceito, tipos (funcional × não funcional), elicitation/análise/especificação/validação/gerência, stakeholders, qualidade (SMART/IEEE), rastreabilidade, baseline/mudança, encaixe com ágeis e com CMMI/MPS/ISO (visão).  
**Fora de escopo aqui:** UML 2.5.1 a fundo (arquivo 03); BPMN; Scrum/Kanban/XP em profundidade (arquivo ágeis); CMMI/MPS/ISO detalhados; ITIL/COBIT; low-code/IA.

---

## 1. Material

### 1.1 O que é engenharia de requisitos (leitura de prova)

**Engenharia de requisitos** é o conjunto de atividades para **descobrir, analisar, documentar, validar e gerenciar** o que o sistema deve fazer e sob quais restrições — antes (e durante) a construção.

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Papel | Transformar **necessidade** do negócio/usuário em **requisitos** verificáveis |
| Ciclo | Elicitar → analisar → especificar → validar → **gerenciar** (mudanças/rastreio) |
| Produto típico | Documento/ backlog / casos de uso / histórias — o **quê**, não o **como** de implementação |

Flash: enunciado fala em **necessidade**, **stakeholder**, **funcional/não funcional**, **rastreabilidade**, **baseline**, **validação** → **engenharia de requisitos**.

Engenharia de requisitos **não** é: desenhar classes UML (é análise/projeto), implementar código, orquestrar containers, aplicar migration Flyway.

### 1.2 Por que a FCC cobra (problema cobrável)

Sem requisitos bem tratados:

1. Entrega “certa” do **problema errado**.  
2. Escopo **explode** sem controle de mudança.  
3. Testes não têm **oráculo** (o que validar?).  
4. Conflitos entre áreas (negócio × TI × jurídico) ficam **latentes**.  
5. Em Judiciário/PDPJ: alinhamento a **normativos** e qualidade do serviço público exige clareza do “o quê”.

Frase de prova: requisito ruim custa **mais caro** quanto mais tarde for descoberto.

### 1.3 Requisito × necessidade × restrição (anti-confusão)

| Conceito | Leitura |
|----------|---------|
| **Necessidade** | Dor/objetivo do stakeholder (pode ser vaga) |
| **Requisito** | Declaração **clara, verificável** do que o sistema/produto deve cumprir |
| **Restrição** | Limite imposto (prazo, lei, tecnologia, orçamento, integração obrigatória) |
| **Regra de negócio** | Política do domínio (ex.: “prazo recursal X”) — pode virar requisito |

Armadilha: tratar “o usuário quer algo moderno” como requisito pronto — falta **precisão** e critério de aceite.

### 1.4 Tipos clássicos (tabela ouro)

| Tipo | O quê descreve | Exemplos cobráveis |
|------|----------------|--------------------|
| **Funcional (RF)** | **O que** o sistema faz (comportamento, funções, dados) | “O sistema deve autenticar o usuário via SSO” |
| **Não funcional (RNF)** | **Como** / qualidades / restrições de qualidade | Desempenho, segurança, usabilidade, disponibilidade, manutenibilidade |
| **De domínio / negócio** | Regras do domínio / legislação aplicável | Prazos processuais, perfis de acesso Judiciário |
| **De interface / integração** | Contratos com outros sistemas | API REST/JSON, eventos, formatos |
| **De dados** | Persistência, retenção, qualidade | “Auditoria de alterações”; LGPD (visão) |

Flash FCC:

- Funcional = **comportamento** observável.  
- Não funcional = **qualidade/restrição** (performance, segurança, etc.).  
- Confundir RNF com RF é erro clássico.

### 1.5 Categorias comuns de RNF (margem de segurança)

Não precisa decorar ISO 25010 inteira; reconhecer eixos:

| Eixo | Ideia |
|------|-------|
| Desempenho / tempo de resposta | Latência, throughput |
| Disponibilidade / confiabilidade | Uptime, tolerância a falha |
| Segurança | Confidencialidade, integridade, autenticação/autorização |
| Usabilidade | Facilidade de uso, acessibilidade |
| Portabilidade / interoperabilidade | Rodar em ambientes / trocar dados |
| Escalabilidade / manutenibilidade | Crescer / evoluir código |

Armadilha: “deve ser seguro” sem métrica — RNF fraco; prova valoriza **mensurável** quando o enunciado pede qualidade.

### 1.6 Stakeholders e fontes de requisitos

**Stakeholder** = pessoa/grupo **afetado** ou **interessado** no sistema (usuário final, gestor, TI, jurídico, cidadão, órgão externo).

Fontes típicas:

1. Entrevistas / workshops  
2. Observação / shadowing  
3. Documentos (normativos, manuais, tickets)  
4. Sistemas legados e logs  
5. Protótipos / MVPs  
6. Análise de concorrentes / benchmarks (com cuidado)

Erro clássico: elicitar **só** com um perfil e ignorar conflito (ex.: produtividade do servidor × auditoria).

### 1.7 Processo em cinco blocos (cartão mental)

```
Elicitação  →  Análise  →  Especificação  →  Validação  →  Gerência
   (obter)      (organizar/     (documentar)     (confirmar      (mudança,
                 priorizar/                       com stakeholder)  rastreio,
                 negociar)                                          baseline)
```

| Atividade | Foco cobrável |
|-----------|---------------|
| **Elicitação** | Descobrir necessidades (técnicas de coleta) |
| **Análise** | Consistência, conflitos, priorização, viabilidade |
| **Especificação** | Documentar de forma clara (SRS, casos de uso, user stories…) |
| **Validação** | “É isso mesmo?” — revisão, protótipo, inspeção, aceite |
| **Gerência** | Controle de mudança, versão, rastreabilidade, baseline |

Não confundir **validação** (requisito certo?) com **verificação** (sistema atende o requisito documentado?) — distinção clássica IEEE/prova.

### 1.8 Técnicas de elicitation (reconhecer nomes)

| Técnica | Quando a prova cita |
|---------|---------------------|
| Entrevista | Perguntas abertas/fechadas com stakeholder |
| Questionário / survey | Muitos respondentes, menos profundidade |
| Workshop / JAD | Grupo facilita consenso e priorização |
| Brainstorming | Gerar ideias (depois filtrar) |
| Observação | Ver o trabalho real (não só o discurso) |
| Análise documental | Leis, normas, manuais, tickets |
| Protótipo | Validar cedo (descartável ou evolutivo) |
| Casos de uso / histórias | Estruturar interações ator–sistema |

Flash: **elicitar** ≠ **especificar** — um coleta; o outro formaliza.

### 1.9 Qualidade do requisito (lista de ouro)

Requisito “bom” tende a ser:

1. **Completo** (no escopo acordado)  
2. **Consistente** (sem contradição)  
3. **Não ambíguo**  
4. **Verificável / testável**  
5. **Rastreável** (origem ↔ desenho ↔ teste)  
6. **Priorizado**  
7. **Factível** (viável técnica/economicamente)

Mnemônico leve (SMART, visão): Specific, Measurable, Achievable, Relevant, Time-bound — útil para RNF e critérios de aceite.

Armadilha: “o sistema deve ser amigável” sem critério → **ambíguo**.

### 1.10 Priorização (visão cobrável)

Técnicas/nomes que aparecem:

| Nome | Ideia |
|------|-------|
| MoSCoW | Must / Should / Could / Won’t |
| Kano | Básico × performance × encantamento |
| Valor × esforço | Matriz simples de prioridade |
| Risco / dependência | O que desbloqueia ou ameaça o projeto |

Em ágeis: Product Owner prioriza backlog; requisitos evoluem em **incrementos** — mas **gerência** e **rastreio** não desaparecem.

### 1.11 Especificação: formas cobráveis

| Forma | Uso típico |
|-------|------------|
| SRS / documento de requisitos | Tradicional / cascata (visão) |
| Casos de uso | Atores + fluxos (liga a UML) |
| User stories + critérios de aceite | Ágil (INVEST: Independent, Negotiable, Valuable, Estimable, Small, Testable — visão) |
| Modelos (UML/BPMN) | Complementam texto; não substituem clareza |
| Protótipos / wireframes | Validação de interface |

Frase: especificação descreve o **problema/solução desejada**, não o código.

### 1.12 Rastreabilidade e baseline

| Conceito | Leitura de prova |
|----------|------------------|
| **Rastreabilidade** | Ligação requisito ↔ origem ↔ artefato (caso de uso, classe, teste) |
| **Matriz de rastreabilidade** | Tabela origem → requisito → implementação → teste |
| **Baseline** | Versão **congelada** acordada; mudanças passam por controle |
| **Controle de mudança** | Avaliar impacto (custo, prazo, risco) antes de aceitar |

Flash: sem rastreabilidade, mudança vira **efeito colateral** invisível.

### 1.13 Validação × verificação (anti-armadilha ouro)

| | **Validação** | **Verificação** |
|---|---------------|-----------------|
| Pergunta | Estamos construindo o **produto certo**? | Estamos construindo o produto **certo** (conforme spec)? |
| Foco | Adequação à necessidade do stakeholder | Conformidade com o documento/modelo |
| Exemplos | Revisão com usuário, protótipo aceito | Inspeção, teste contra RF/RNF documentados |

FCC gosta de trocar os termos — memorize a pergunta-chave.

### 1.14 Requisitos em ágil × “tradicional” (visão)

| Aspecto | Mais tradicional | Mais ágil |
|---------|------------------|-----------|
| Momento | Ênfase up-front | Evolutivo / just-in-time |
| Artefato | SRS longo | Backlog + stories + aceite |
| Mudança | Comitê / formal | Esperada, mas ainda **gerenciada** |
| Aceite | Homologação formal | Demo / DoD / critérios de aceite |

Armadilha: “ágil = sem requisitos” — **falso**. Ágil muda **quando** e **como** documenta; não elimina a engenharia.

### 1.15 Encaixe com edital (CMMI / MPS / ISO — só ponte)

No edital: engenharia de requisitos convive com **CMMI Dev**, **MR-MPS-SW 2024**, **ISO/IEC/IEEE 12207**, **UML**, **BPMN**, **ágeis**.

Leitura cobrável (sem aprofundar frameworks aqui):

- Processos maduros **exigem** práticas de requisitos (obter, analisar, gerir).  
- UML/BPMN **modelam**; não substituem elicitation e gerência.  
- PDPJ/Judiciário: requisitos de integração, segurança e conformidade **pesam** como RNF/restrições.

### 1.16 O que engenharia de requisitos **não** é (lista seca)

1. Não é programar a solução.  
2. Não é diagramar UML por si só (embora casos de uso ajudem).  
3. Não é BPMN (modelagem de processo de negócio — arquivo próprio).  
4. Não é Scrum (método ágil — arquivo próprio).  
5. Não é ITIL/COBIT (governança de serviços/TIC).  
6. Não é “só entrevista” — inclui análise, spec, validação e gerência.  
7. Não elimina testes: requisitos **alimentam** testes.  
8. Não é glossário vazio sem critérios verificáveis.

### 1.17 Armadilhas FCC (lista de ouro)

1. Trocar **funcional** por **não funcional**.  
2. Trocar **validação** por **verificação**.  
3. Dizer que ágil **dispensa** requisitos.  
4. Tratar necessidade vaga como requisito pronto.  
5. Ignorar **stakeholders** conflitantes.  
6. Esquecer **rastreabilidade** / baseline / mudança.  
7. Confundir requisito com **design** (como implementar).  
8. Achar que RNF “bonito” sem métrica é suficiente quando o item pede qualidade.  
9. Reduzir engenharia de requisitos a “escrever casos de uso UML”.  
10. Confundir elicitation com gerência de configuração de código.

### 1.18 Mini-síntese (cartão mental)

> **Engenharia de requisitos** = elicitar, analisar, especificar, validar e **gerenciar** o que o sistema deve cumprir.  
> **RF** = o que faz; **RNF** = qualidades/restrições.  
> Bom requisito: claro, verificável, rastreável, priorizado.  
> **Validação** = produto certo?; **Verificação** = conforme a spec?  
> Ágil **não** mata requisitos — muda a cadência.  
> Liga a UML (modelagem), BPMN (processo) e frameworks de processo (CMMI/MPS/ISO) sem se confundir com eles.

---

## 2. Questões de treino (1–15)

**1.** No edital de Analista TI TRT4, a engenharia de requisitos ocupa-se prioritariamente de:

A) Empacotar microsserviços em imagens Docker  
B) Descobrir, analisar, documentar, validar e gerenciar o que o sistema deve cumprir  
C) Orquestrar Pods no Kubernetes  
D) Aplicar migrations Flyway no PostgreSQL  
E) Configurar filtros Zuul de borda  

**2.** Assinale a associação **correta**:

A) Requisito funcional = desempenho e disponibilidade apenas  
B) Requisito funcional = comportamento/funções do sistema; não funcional = qualidades/restrições  
C) Requisito não funcional = exclusivamente casos de uso UML  
D) Necessidade vaga e requisito verificável são sinônimos estritos  
E) Rastreabilidade = apenas o Dockerfile  

**3.** “O sistema deve autenticar usuários via SSO institucional” é, em regra:

A) Requisito não funcional de usabilidade estética  
B) Requisito funcional (comportamento de autenticação)  
C) Apenas restrição de cabo de rede  
D) Atividade exclusiva de ITIL Service Desk  
E) Comando `docker pull`  

**4.** “O tempo de resposta da consulta X deve ser ≤ 2 s em 95% das requisições” exemplifica melhor:

A) Regra de negócio sem relação com software  
B) Requisito não funcional mensurável (desempenho)  
C) Diagrama de classes UML obrigatório  
D) Baseline de código-fonte Git  
E) Broker RabbitMQ  

**5.** A atividade de **elicitation** visa sobretudo:

A) Compilar o binário de produção  
B) Obter/descobrir necessidades e informações junto a fontes/stakeholders  
C) Substituir a validação com o usuário  
D) Gerar automaticamente o cluster Rancher  
E) Documentar apenas o “como” de implementação em bytecode  

**6.** Relativamente a validação e verificação de requisitos/produto, a leitura cobrável é:

A) São sempre sinônimos absolutos na FCC  
B) Validação pergunta se é o produto certo; verificação, se está conforme a especificação  
C) Verificação só existe em Docker  
D) Validação elimina qualquer teste  
E) Ambas significam apenas “deploy em produção”  

**7.** Rastreabilidade de requisitos serve principalmente para:

A) Ligar origem ↔ requisito ↔ artefatos/testes e avaliar impacto de mudanças  
B) Substituir o Product Owner  
C) Eliminar stakeholders  
D) Transformar todo RNF em RF automaticamente  
E) Orquestrar containers  

**8.** Qual afirmação é **incorreta**?

A) Baseline é versão acordada/congelada sob controle de mudança  
B) Ágil dispensa totalmente qualquer forma de requisito ou critério de aceite  
C) MoSCoW é técnica de priorização  
D) Stakeholder é parte interessada/afetada  
E) Ambiguidade prejudica a verificabilidade do requisito  

**9.** Em enunciado que fala em Must/Should/Could/Won’t, a técnica alinhada é:

A) MoSCoW  
B) Dijkstra  
C) CAP theorem  
D) Blue-green deploy exclusivo  
E) MapStruct  

**10.** Diferenciar requisito de design (solução) é relevante porque:

A) Não há diferença cobrável  
B) Requisito foca o **quê**/restrições acordadas; design detalha o **como** implementar  
C) Design sempre precede qualquer necessidade  
D) Requisito é apenas o nome do Pod Kubernetes  
E) Design elimina a gerência de mudança  

**11.** Protótipo na engenharia de requisitos serve tipicamente a:

A) Substituir permanente o código de produção sem validação  
B) Ajudar a **validar** cedo entendimento de interface/comportamento com stakeholders  
C) Aplicar DDL no lugar do Flyway  
D) Registrar serviço no Eureka  
E) Gerar token OAuth2  

**12.** Em contexto ágil com user stories, o elemento que torna a story testável/aceitável costuma ser:

A) A ausência total de critérios  
B) Critérios de aceite claros  
C) Apenas o diagrama de implantação UML  
D) O nome do registry Docker  
E) A porta do Zuul  

**13.** Análise de requisitos inclui, em visão de prova:

A) Só digitar código Java  
B) Resolver conflitos, checar consistência, priorizar e avaliar viabilidade  
C) Apenas `docker compose up`  
D) Somente configuração de SNMP  
E) Exclusivamente desenhar logo institucional  

**14.** Qual linha resume melhor o cartão mental deste subtópico?

A) Requisitos = só UML; ágil = zero documentação  
B) Elicitar → analisar → especificar → validar → gerenciar; RF ≠ RNF; validação ≠ verificação  
C) Requisitos = Docker + Rancher  
D) Requisitos = apenas COBIT  
E) Requisitos = somente criptografia TLS  

**15.** Afirmar que “requisito não funcional nunca precisa ser mensurável” é:

A) Sempre correto em prova FCC  
B) Problemático: RNF cobráveis de qualidade tendem a critérios verificáveis/mensuráveis  
C) Correto porque RNF = RF  
D) Correto somente com H2  
E) Correto se houver Swagger  

---

### Gabarito comentado (treino 1–15)

1. **B** — Ciclo clássico da engenharia de requisitos.  
2. **B** — RF = comportamento; RNF = qualidade/restrição.  
3. **B** — Autenticação como função/comportamento.  
4. **B** — Desempenho com métrica.  
5. **B** — Descoberta junto a fontes.  
6. **B** — Produto certo × conforme spec.  
7. **B** — Ligação e impacto de mudança.  
8. **B** — Ágil não elimina requisitos/aceite.  
9. **A** — MoSCoW.  
10. **B** — Quê × como.  
11. **B** — Validação precoce.  
12. **B** — Critérios de aceite.  
13. **B** — Consistência/prioridade/viabilidade.  
14. **B** — Síntese do arquivo.  
15. **B** — RNF deve ser verificável quando a qualidade importa.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** No processo de desenvolvimento de software referido no edital do concurso, a engenharia de requisitos caracteriza-se, em regra, por:

A) limitar-se à orquestração de containers em cluster  
B) compreender atividades de obtenção, análise, documentação, validação e gerência do que o sistema deve atender  
C) substituir integralmente a modelagem UML e o BPMN  
D) consistir apenas na compilação automatizada em CI/CD  
E) equivaler ao Service Discovery da stack Spring Cloud  

**17.** Relativamente a requisitos funcionais e não funcionais, assinale a opção correta:

A) Ambos descrevem exclusivamente a cor da interface  
B) Os funcionais expressam comportamentos/funções; os não funcionais, atributos de qualidade ou restrições correlatas  
C) Não funcionais nunca podem ser testados  
D) Funcionais tratam apenas de latência de rede  
E) A distinção foi abolida pelo edital  

**18.** A rastreabilidade de requisitos contribui principalmente para:

A) eliminar a necessidade de stakeholders  
B) permitir acompanhar a origem e o desdobramento do requisito até artefatos e testes, apoiando análise de impacto  
C) substituir o controle de versão de código  
D) tornar desnecessária qualquer baseline  
E) obrigar o uso exclusivo de máquina virtual por requisito  

**19.** Assinale a alternativa que apresenta associação **incorreta**:

A) Elicitação — obtenção de informações/necessidades  
B) Validação — confirmar se o entendimento atende à necessidade (produto certo)  
C) Verificação — checar conformidade com a especificação  
D) Baseline — versão acordada sob controle de mudanças  
E) Engenharia de requisitos — sinônimo estrito de deploy Kubernetes  

**20.** Em enunciado que contraste práticas ágeis e engenharia de requisitos, a FCC tipicamente considera **correto** que:

A) métodos ágeis dispensam critérios de aceite e qualquer forma de requisito  
B) o trabalho com backlog/user stories continua a demandar clareza, priorização e validação do que será entregue  
C) ágil e requisitos são mutuamente excludentes por definição normativa  
D) requisitos só existem em cascata pura e nunca em Scrum  
E) Product Owner não participa de priorização  

**21.** A expressão “declaração clara e verificável do que o sistema deve cumprir, derivada de necessidades de stakeholders” descreve com mais precisão:

A) Imagem Docker imutável  
B) Requisito de software  
C) Filtro error do Zuul  
D) Pod Kubernetes vazio  
E) Registry Harbor exclusivamente  

**22.** Considere as afirmações:

I. “O sistema deve ser amigável” é, por si só, exemplo ideal de requisito não ambíguo e plenamente testável.  
II. Matriz de rastreabilidade apoia ligar requisitos a testes e mudanças.  
III. Conflitos entre stakeholders devem ser tratados na análise/negociação de requisitos.

Assinale:

A) Apenas I  
B) Apenas II  
C) II e III  
D) I e II  
E) I, II e III  

**23.** Em projeto com mudança de escopo frequente, o controle de mudança sobre requisitos baselineados serve para:

A) aceitar qualquer pedido sem avaliar impacto  
B) avaliar impacto (prazo, custo, risco, rastreios) antes de incorporar a alteração  
C) proibir qualquer evolução do produto  
D) substituir testes automatizados  
E) eliminar a especificação  

**24.** Diferenciar elicitation de especificação é relevante em provas porque:

A) não há qualquer diferença conceitual cobrável  
B) elicitar obtém/descobre; especificar documenta de forma estruturada o requisito acordado  
C) especificar ocorre apenas após o deploy em produção  
D) elicitar é exclusividade do Docker Hub  
E) ambas significam apenas desenhar diagrama de sequência  

**25.** A síntese mais alinhada ao edital, sobre engenharia de requisitos, é:

A) Requisitos = apenas ITIL; UML = apenas redes  
B) Ciclo elicitar–analisar–especificar–validar–gerenciar; RF ≠ RNF; validação ≠ verificação; ágil não elimina requisitos  
C) Requisitos obrigam eliminar rastreabilidade e baseline  
D) Requisitos substituem CMMI, MPS e ISO 12207  
E) Requisitos = somente criptografia de Wi-Fi  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Escopo clássico da disciplina.  
17. **B** — RF × RNF.  
18. **B** — Origem ↔ artefato ↔ impacto.  
19. **E** — Não é deploy K8s.  
20. **B** — Ágil mantém clareza/aceitação.  
21. **B** — Definição de requisito.  
22. **C** — I falsa (ambíguo); II e III ok.  
23. **B** — Impacto antes de aceitar.  
24. **B** — Obter × documentar.  
25. **B** — Cartão mental do arquivo 02.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **o quê o sistema deve fazer/cumprir** (ciclo elicitar…gerenciar)? → Engenharia de requisitos.  
2. É **comportamento/função**? → RF.  
3. É **qualidade/restrição mensurável**? → RNF.  
4. É **produto certo** vs **conforme spec**? → Validação × verificação.  
5. É **ligação origem–teste / impacto de mudança**? → Rastreabilidade / baseline.  
6. É **diagrama UML**? → Arquivo 03 (modelagem), não substitui requisitos.  
7. É **Scrum/Kanban**? → Ágeis (complementar).  
8. É **BPMN**? → Processo de negócio (outro arquivo).

+++
title = '06 — Low-code / No-code + IA no desenvolvimento'
date = '2026-10-20'
draft = false
weight = 6
tags = ['semana-04', 'lowcode', 'nocode']
categories = ['estudo']
+++
# Low-code / No-code + IA no desenvolvimento · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — low-code, no-code e assistentes de IA no desenvolvimento  
**Escopo deste arquivo:** definições cobráveis de **low-code** e **no-code**; quando cada um serve; papéis (citizen developer × profissional de TI); riscos (shadow IT, segurança, LGPD, vendor lock-in, dívida técnica); **uso de assistentes de IA** no ciclo de software (código, testes, documentação, revisão) — alinhado ao Anexo II (*low-code e no-code; uso de assistentes de IA no desenvolvimento de software*).  
**Fora de escopo aqui:** ML/redes neurais/LLMs em profundidade (semana 9 — bloco IA do edital); BPMN/UML/ágeis detalhados (outros arquivos da semana 4); COBIT/ITIL (arquivos 07–08); produto comercial linha a linha.

---

## 1. Material

### 1.1 Por que cai no edital

O Anexo II, em **Processo de desenvolvimento de software**, fecha o bloco clássico (CMMI, ISO 12207, MPS-SW, UML, BPMN, ágeis, requisitos) com duas frentes modernas:

| Frente | Leitura FCC |
|--------|-------------|
| **Low-code / no-code** | Acelerar entrega com plataformas visuais; menos (ou nenhum) código tradicional |
| **Assistentes de IA** | Apoiar o desenvolvedor (gerar, completar, explicar, testar, documentar) — **não** substituir governança |

Flash: FCC gosta de **diferenciar** low-code × no-code × programação tradicional × “IA que escreve código sozinha sem controle”.

### 1.2 No-code (definição cobrável)

**No-code** = desenvolvimento de aplicações **sem escrever código** (ou com código residual mínimo/oculto), via:

- interfaces **visuais** (arrastar e soltar);
- formulários, fluxos, regras declarativas;
- conectores prontos (e-mail, planilha, APIs, banco).

Público típico: **usuário de negócio** / *citizen developer* com supervisão de TI.

O que no-code **não** é:

- substituir obrigatoriamente todo sistema crítico do tribunal;
- “sem necessidade de requisitos, testes ou segurança”;
- sinônimo de low-code (há diferença de **profundidade de customização**).

### 1.3 Low-code (definição cobrável)

**Low-code** = plataformas que **reduzem** a quantidade de código tradicional, mas **permitem** (e muitas vezes **exigem**) extensões em código, scripts, APIs e componentes customizados.

Público típico: **desenvolvedores profissionais** + times mistos (negócio + TI).

| | **No-code** | **Low-code** | **Código tradicional** |
|---|-------------|----------------|------------------------|
| Código escrito | Quase nenhum | Reduzido, mas presente | Principal meio |
| Customização profunda | Limitada ao que a plataforma oferece | Alta (extensões) | Máxima |
| Velocidade inicial | Muito alta | Alta | Depende do time |
| Complexidade / integração crítica | Cuidado | Adequado com governança | Adequado |
| Perfil | Citizen developer | Dev + negócio | Engenheiro de software |

Síntese de prova:

```
No-code  → visual, pouca/nenhuma programação
Low-code → visual + extensões em código
Tradicional → código como eixo
IA assistente → acelera qualquer um dos três (com governança)
```

### 1.4 Citizen developer e papel da TI

**Citizen developer** = profissional de negócio que cria apps com plataformas low/no-code **sob políticas** da organização (não “TI clandestina”).

Papel da TI / área de sistemas (visão cobrável):

- definir **plataformas homologadas**;
- **governança** (acesso, dados, integração, ciclo de vida);
- revisão de segurança, LGPD, disponibilidade;
- integrar ao ecossistema (SSO, APIs, bancos oficiais).

**Shadow IT** = soluções criadas **fora** do controle/governança — risco clássico quando no-code explode sem política.

### 1.5 Quando usar (e quando não)

| Cenário | Tendência |
|---------|-----------|
| Protótipo, MVP, formulário interno, workflow simples | No-code / low-code |
| Integração leve com APIs estáveis | Low-code (ou no-code com conectores) |
| Núcleo judiciário crítico, alta complexidade, performance extrema, regras muito específicas | Código tradicional (ou low-code só como fachada + serviços robustos) |
| Automação de tarefa repetitiva de escritório | No-code / RPA leve (noção) |

FCC: plataforma low/no-code **não elimina** engenharia de requisitos, testes, versionamento nem segurança.

### 1.6 Riscos e controles (caderno mental)

| Risco | Controle típico |
|-------|-----------------|
| Shadow IT | Catálogo de plataformas + política de uso |
| Vendor lock-in | Avaliar portabilidade, exportação, padrões |
| Segurança / LGPD | Classificação de dados, SSO, trilhas de auditoria |
| Dívida técnica visual | Padrões, revisão, limite de complexidade na plataforma |
| Indisponibilidade do fornecedor | SLA, contingência, backup de configurações |
| “App de planilha” em produção | Governança de ciclo de vida (como qualquer software) |

### 1.7 Assistentes de IA no desenvolvimento

O edital cita **uso de assistentes de IA no desenvolvimento de software** — leitura prática (não é o bloco teórico de ML da semana 9):

| Uso | Exemplo cobrável |
|-----|------------------|
| Completar / sugerir código | Autocomplete inteligente no IDE |
| Gerar trechos a partir de prompt | Função, teste unitário, SQL de consulta |
| Explicar código legado | “O que este método faz?” |
| Refatorar / corrigir | Sugestão de patch, lint inteligente |
| Documentar | Comentários, README, OpenAPI rascunho |
| Apoiar revisão | Destacar risco, estilo, possível bug |

O que a IA **não** faz sozinha em prova séria:

- assumir **responsabilidade** jurídica/técnica do servidor público;
- garantir correção sem **revisão humana**;
- substituir testes, code review e normas do tribunal;
- autorizar envio de **dados sigilosos** a serviços externos sem política.

Princípio ouro FCC: IA = **assistente**; humano = **responsável** pelo artefato entregue.

### 1.8 Boas práticas com assistentes de IA (visão cobrável)

1. **Não colar** dados pessoais, segredos, processos sigilosos ou código proprietário sensível em ferramentas não homologadas.  
2. **Revisar** sempre o que foi gerado (alucinação, licença, vulnerabilidade).  
3. Preferir ferramentas **aprovadas** pela organização.  
4. Manter testes automatizados e revisão por pares.  
5. Documentar uso relevante quando a política exigir (transparência).  
6. Tratar sugestões como **rascunho**, não como verdade.

### 1.9 Relação com ágeis, requisitos e governança

- Low/no-code **encaixa** em entregas curtas (Scrum/Kanban), mas o **backlog** e os critérios de aceite continuam.  
- Requisitos ruins viram app visual ruim — mais rápido, não mais certo.  
- Governança (ITIL/COBIT / modelo TIC TRT4) cobre **serviços e controles**; a plataforma é só o meio.  
- PDPJ/microsserviços: low-code pode ser **camada de experiência** consumindo APIs; não “reescreve” a arquitetura sozinha.

### 1.10 Armadilhas FCC (checklist)

- Tratar no-code e low-code como **sinônimos**.  
- Dizer que no-code **elimina** a necessidade de TI/governança.  
- Afirmar que assistente de IA **substitui** o desenvolvedor ou o analista.  
- Confundir low-code com **UML**, **BPMN** ou **Kanban**.  
- Confundir “IA no desenvolvimento” com o bloco teórico completo de **ML/LLM** (outro tópico do edital).  
- Ignorar **LGPD / sigilo** ao usar cloud de IA.  
- Achar que low-code = sempre inferior (ou sempre superior) ao código tradicional.

### 1.11 Cartão-resumo (antes das questões)

| Tema | Âncora |
|------|--------|
| No-code | Visual, quase sem código; citizen developer |
| Low-code | Visual + extensões em código; times mistos |
| Tradicional | Código como eixo; complexidade alta |
| Shadow IT | App fora da governança |
| IA assistente | Acelera; humano revisa e responde |
| Não é | Substituição cega de TI, UML ou ML teórico |

---

## 2. Questões de treino (1–15)

**1.** No-code, em definição cobrável, caracteriza-se principalmente por:

A) Obrigar programação em assembly  
B) Construção visual com pouca ou nenhuma escrita de código  
C) Ser sinônimo exato de Kubernetes  
D) Substituir a LGPD  
E) Eliminar requisitos e testes por definição  

**2.** Low-code diferencia-se do no-code, em regra, porque:

A) Proíbe qualquer interface visual  
B) É exclusivo de mainframe  
C) Permite/exige mais **extensões em código** e customização profunda  
D) É o nome comercial do ITIL  
E) Só existe sem banco de dados  

**3.** Citizen developer, sob governança adequada, é:

A) Atacante externo autorizado  
B) Usuário de negócio que cria apps em plataformas homologadas com políticas de TI  
C) Substituto legal do juiz  
D) Sinônimo de cluster Rancher  
E) Protocolo OAuth  

**4.** Shadow IT no contexto low/no-code refere-se tipicamente a:

A) Backup oficial em fita  
B) Soluções criadas **fora** do controle/governança da organização  
C) Service Desk ITIL  
D) Objetivo EDM01 do COBIT  
E) Namespace Kubernetes  

**5.** Assistente de IA no desenvolvimento, segundo a leitura do edital, serve sobretudo para:

A) Substituir integralmente a responsabilidade do servidor  
B) Apoiar tarefas como sugerir código, testes e documentação, com revisão humana  
C) Emitir sentenças judiciais  
D) Remover a necessidade de SSO  
E) Proibir o uso de Git  

**6.** Qual associação está correta?

A) No-code = máxima customização em C; low-code = zero integração  
B) Low-code = só planilha; no-code = só COBOL  
C) IA assistente = Authorization Server OAuth  
D) No-code = quase sem código; low-code = código reduzido + extensões  
E) Tradicional = arrastar e soltar obrigatório  

**7.** Risco típico de plataformas low/no-code em órgãos públicos:

A) Impossibilidade teórica de existir formulário  
B) Vendor lock-in, shadow IT e exposição de dados sem controle  
C) Obrigação de abandonar TCP/IP  
D) Proibição de usar PostgreSQL  
E) Eliminação automática de todos os bugs  

**8.** Em sistema judiciário crítico de alta complexidade, a escolha mais alinhada à prudência técnica costuma ser:

A) No-code sem revisão para o núcleo  
B) Código tradicional (ou arquitetura robusta) para o núcleo, podendo low-code na borda  
C) Planilha compartilhada anônima como banco oficial  
D) IA generativa sem testes  
E) Desligar governança para acelerar  

**9.** Assinale a afirmativa **incorreta**:

A) Low-code reduz volume de código tradicional  
B) No-code usa interfaces visuais  
C) Assistentes de IA dispensam qualquer revisão humana por definição legal  
D) Governança de TI continua necessária  
E) Requisitos ruins prejudicam apps low/no-code  

**10.** Boa prática ao usar assistente de IA em ambiente de tribunal:

A) Colar processos sigilosos em qualquer chat público  
B) Aceitar todo código gerado sem leitura  
C) Usar só ferramentas homologadas e revisar artefatos; proteger dados sensíveis  
D) Desativar autenticação  
E) Publicar secrets no repositório para “ajudar a IA”  

**11.** Low-code/no-code em relação a métodos ágeis:

A) Eliminam o backlog  
B) Proíbem Kanban  
C) Podem acelerar entregas, mantendo priorização e critérios de aceite  
D) Substituem o Product Owner por um algoritmo obrigatório  
E) São incompatíveis com sprints  

**12.** Qual contraste está alinhado ao edital?

A) UML = low-code; BPMN = no-code  
B) Low/no-code = meios de construir apps; assistente de IA = apoio ao desenvolvimento  
C) ITIL = linguagem de programação  
D) COBIT = IDE  
E) MapStruct = plataforma no-code  

**13.** Dívida técnica em apps low-code surge sobretudo quando:

A) Há versionamento e padrões  
B) Complexidade e gambiarras visuais crescem sem revisão/padrões  
C) Existe apenas um formulário trivial bem governado  
D) A TI homologa a plataforma  
E) Há testes automatizados  

**14.** O bloco “IA no desenvolvimento” deste arquivo **não** se confunde prioritariamente com:

A) Autocomplete inteligente no IDE  
B) Geração de teste unitário assistida  
C) Estudo profundo de redes neurais e treinamento de modelos (bloco IA teórico do edital)  
D) Explicação de código legado por assistente  
E) Sugestão de refatoração  

**15.** Síntese correta do subtópico:

A) No-code ≈ visual sem código; low-code ≈ visual + código; IA = assistente com humano responsável; governança obrigatória  
B) No-code = COBIT; low-code = ITIL  
C) IA substitui analista e juiz  
D) Shadow IT é política oficial recomendada  
E) Low-code elimina LGPD  

---

### Gabarito comentado (treino 1–15)

1. **B** — Visual, pouco/nenhum código.  
2. **C** — Extensões em código / customização.  
3. **B** — Negócio + plataforma homologada.  
4. **B** — Fora da governança.  
5. **B** — Apoio com revisão humana.  
6. **D** — Diferença no-code × low-code.  
7. **B** — Lock-in, shadow IT, dados.  
8. **B** — Núcleo robusto; borda pode ser low-code.  
9. **C** — Incorreta: IA não dispensa revisão.  
10. **C** — Homologação + revisão + dados.  
11. **C** — Acelera; backlog permanece.  
12. **B** — Meios × assistência.  
13. **B** — Complexidade sem padrão.  
14. **C** — ML teórico é outro bloco.  
15. **A** — Síntese do arquivo.  

---

## 3. Questões estilo concurso FCC (16–25)

**16.** Em órgãos que adotam plataformas de desenvolvimento rápido, o *no-code* é corretamente caracterizado como abordagem que:

A) Exige, por definição, programação em linguagem de baixo nível  
B) Impede qualquer integração com APIs  
C) Substitui integralmente a necessidade de políticas de segurança da informação  
D) Permite construir aplicações predominantemente por meios visuais, com mínima ou nenhuma escrita de código pelo usuário  
E) Corresponde ao mesmo conceito de orquestração de containers  

**17.** Considere as afirmativas:

I. Low-code reduz a quantidade de código tradicional, admitindo extensões programáticas.  
II. No-code e low-code são sempre termos intercambiáveis sem qualquer distinção.  
III. O uso de assistentes de IA no desenvolvimento não elimina a responsabilidade de revisão humana dos artefatos.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e III, apenas  
D) II e III, apenas  
E) I, II e III  

**18.** Relacione corretamente:

(1) No-code  
(2) Low-code  
(3) Assistente de IA no IDE  

( ) Sugere/completa código e apoia tarefas do ciclo de desenvolvimento  
( ) Plataforma visual com pouca ou nenhuma programação explícita  
( ) Plataforma visual que tipicamente admite customização via código

A sequência correta é:

A) 1, 2, 3  
B) 3, 1, 2  
C) 2, 3, 1  
D) 3, 2, 1  
E) 1, 3, 2  

**19.** A criação de aplicações por usuários de negócio **sem** alinhamento às políticas, plataformas e controles da área de TI configura, em regra:

A) Continual improvement do ITIL  
B) Objetivo EDM02 do COBIT  
C) Shadow IT  
D) Service Level Agreement  
E) Definition of Done do Scrum  

**20.** Acerca do emprego de assistentes de IA generativa no desenvolvimento de software em ambiente institucional, é correto afirmar que:

A) Todo prompt com dados sigilosos é automaticamente seguro em serviços públicos de internet  
B) A ferramenta assume, por si, a responsabilidade funcional e jurídica pelo sistema em produção  
C) Devem ser observadas políticas de uso, proteção de dados e revisão humana do conteúdo gerado  
D) Testes automatizados tornam-se proibidos  
E) Versionamento de código deixa de ser necessário  

**21.** Em comparação com o desenvolvimento tradicional, plataformas low-code tendem a:

A) Aumentar artificialmente a obrigatoriedade de assembly  
B) Acelerar entregas em cenários adequados, mantendo necessidade de requisitos, testes e governança  
C) Eliminar por completo o risco de vendor lock-in  
D) Substituir bancos relacionais por papel impresso  
E) Tornar desnecessária a autenticação de usuários  

**22.** Assinale a alternativa que apresenta **confusão conceitual**:

A) Citizen developer atua com plataformas sob governança  
B) Low-code admite extensões em código  
C) Assistente de IA pode gerar rascunhos de testes  
D) No-code, low-code e ITIL Version 5 são nomes do mesmo framework de governança de TI  
E) Shadow IT foge ao controle institucional  

**23.** Para fins do Anexo II do edital (Analista TI / FCC), a menção conjunta a low-code, no-code e assistentes de IA no desenvolvimento sugere, prioritariamente:

A) Apenas cabeamento estruturado  
B) Meios modernos de construir e apoiar a construção de software, sem afastar engenharia e controles  
C) Substituição do COBIT pelo UML  
D) Obrigatoriedade de VB.NET em todo fluxo visual  
E) Proibição de APIs REST  

**24.** Em relação a dados pessoais e informações sigilosas do Judiciário, o uso de assistentes de IA baseados em nuvem externa **não homologada** é, em regra:

A) Irrelevante sob a LGPD  
B) Sempre obrigatório pelo edital  
C) Risco a ser tratado por política institucional (pode ser inadequado ou vedado conforme o caso)  
D) Equivalente a backup oficial em fita  
E) Substituto do SSO Keycloak  

**25.** Analise o trecho: “Abordagem X permite montar aplicações por interface gráfica com quase nenhuma programação; abordagem Y reduz código tradicional mas admite extensões; ferramenta Z sugere trechos de código no ambiente de desenvolvimento, cabendo ao profissional validar o resultado.”

A associação correta é:

A) X = low-code; Y = no-code; Z = COBIT  
B) X = no-code; Y = low-code; Z = assistente de IA  
C) X = ITIL; Y = PMBOK; Z = BPMN  
D) X = Kubernetes; Y = Docker; Z = Rancher  
E) X = UML; Y = CMMI; Z = Flyway  

---

### Gabarito comentado (FCC 16–25)

16. **D** — Visual, mínima/nenhuma escrita de código.  
17. **C** — I e III verdadeiras; II falsa (não são sinônimos).  
18. **B** — IA; no-code; low-code.  
19. **C** — Shadow IT.  
20. **C** — Política + dados + revisão humana.  
21. **B** — Acelera com requisitos/testes/governança.  
22. **D** — Confusão: não são o mesmo framework.  
23. **B** — Meios modernos + engenharia/controles.  
24. **C** — Risco / política (LGPD/sigilo).  
25. **B** — No-code / low-code / assistente.  

---

## 4. Como usar este arquivo

1. Ler §1 (cartão §1.11).  
2. Fechar treino 1–15 sem gabarito → conferir.  
3. Fazer 16–25 em modo prova.  
4. Anotar erros (no-code × low-code; shadow IT; IA ≠ responsabilidade automática).  
5. Semana 9: aprofundar ML/LLM; aqui fica só o **assistente no ciclo de software**.

---

*Arquivo 06 · Low-code / no-code + IA no desenvolvimento · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 4 — engenharia de software.*

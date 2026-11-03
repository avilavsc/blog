+++
title = '07 — Frameworks web: React, Angular, Vue, Node'
date = '2026-10-21'
draft = false
weight = 7
tags = ['semana-05', 'frameworks', 'react']
categories = ['estudo']
+++
# Frameworks web: React, Angular, Vue, Node · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 07 — Frameworks JS (conceitos e diferenças) · **Semana 5**  
**Escopo deste arquivo:** visão cobrável de **React**, **React Native**, **Angular**, **Vue.js** e **Node.js** — o que é cada um, onde roda, contraste rápido e armadilhas FCC. Sem aprofundar API, hooks avançados, RxJS, Nuxt etc.  
**Fora de escopo aqui:** HTML5/CSS3/Bootstrap/JS/TS em profundidade (outros arquivos da semana); jQuery/JSF/JSP (cauda longa); ASP.NET Core; microsserviços Java.

**Edital (trecho):** Desenvolvimento web e mobile — frameworks (React, React Native, Angular, jQuery, Node.js, Vue.js etc.); TypeScript.

---

## 1. Material

### 1.1 Mapa mental FCC (o que a banca testa)

A FCC, neste pedaço, quase nunca pede “escreva um componente”. Ela **classifica** a ferramenta:

| Pergunta-âncora | Resposta-flash |
|-----------------|----------------|
| Roda no **browser** ou no **servidor**? | React/Angular/Vue = front · Node = runtime JS no servidor (também CLI/tooling) |
| É **biblioteca** ou **framework**? | React = biblioteca de UI · Angular = framework completo · Vue = framework progressivo |
| Mobile? | **React Native** = apps nativos com paradigma React (não é o React web) |
| Quem “manda” no DOM? | Componentes + virtual DOM (React/Vue) ou change detection (Angular) |

Flash: **não misture** Node (backend/runtime) com React/Angular/Vue (UI). Node **pode** servir APIs que o front consome — são camadas diferentes.

---

### 1.2 Quadro comparativo (âncora única)

| | **React** | **Angular** | **Vue.js** | **Node.js** |
|---|-----------|-------------|------------|-------------|
| Natureza | Biblioteca de UI | Framework full (opiniado) | Framework progressivo | Runtime JS (V8) fora do browser |
| Camada típica | Front (SPA) | Front (SPA) | Front (SPA) | Back / tooling / SSR helpers |
| Linguagem “oficial” | JS + JSX; TS comum | **TypeScript** primeiro | JS/TS; SFCs `.vue` | JS/TS no servidor |
| Criado / ecossistema | Meta (Facebook) | Google | Evan You / comunidade | OpenJS / Node Foundation |
| Roteamento / DI / forms | Via libs (React Router, etc.) | Incluídos no core | Core + ecossistema oficial | N/A (HTTP via Express etc.) |
| Mobile irmão | **React Native** | Ionic / NativeScript (ecossistema) | NativeScript / Quasar etc. | Não é UI mobile |

Cartão: em prova, **Angular = “tudo incluso”**; **React = UI + você monta o resto**; **Vue = intermediário / progressivo**; **Node = JS no servidor**.

---

### 1.3 React (conceitos)

- **O quê:** biblioteca para construir interfaces com **componentes** reutilizáveis.  
- **JSX:** sintaxe que mistura aparência de HTML com JS (transpilada).  
- **Virtual DOM:** React calcula um “diff” e atualiza o DOM real com eficiência.  
- **Unidirecional:** dados fluem tipicamente de cima para baixo (props).  
- **Estado:** o que muda a UI; libs de estado (Redux, Zustand…) são **opcionais**, não “o React”.  
- **SPA:** Single Page Application — navegação no cliente sem reload completo clássico.

Armadilha: React **não** é linguagem nem banco; **não** substitui HTML/CSS/JS base.

---

### 1.4 React Native (só o contraste)

- Usa o **modelo mental** do React (componentes, props, estado).  
- Compila para **componentes nativos** (iOS/Android), não para DOM HTML do browser.  
- **Não** é “React rodando dentro do Chrome do celular” no sentido web clássico.  
- Em prova: se falar **app mobile nativo multiplataforma com paradigma React** → React Native; se falar **UI web** → React.

---

### 1.5 Angular (conceitos)

- **Framework** completo: módulos/componentes, **roteamento**, **injeção de dependência**, formulários, HTTP client.  
- **TypeScript** é o padrão de projeto oficial.  
- Arquitetura tipicamente **componentizada** + serviços injetáveis.  
- **Change detection** atualiza a view quando o modelo muda (conceito cobrável; sem detalhe Zone.js).  
- Versões modernas (= Angular, sem o sufixo “JS” do AngularJS 1.x legado). Em edital atual, leia **Angular** como a linha 2+.

Armadilha: **Angular ≠ AngularJS**. AngularJS = 1.x (legado). FCC pode citar só “Angular” no pacote de frameworks modernos.

---

### 1.6 Vue.js (conceitos)

- **Framework progressivo:** dá para começar só na view e ir crescendo (roteador, Pinia/Vuex, etc.).  
- **SFCs** (Single File Components): template + script + estilo no `.vue`.  
- Reatividade declarativa; também usa ideia de Virtual DOM nas versões modernas.  
- Curva frequentemente descrita como **mais suave** que Angular e **mais “estruturada de fábrica”** que React puro — útil para questões de comparação, não como dogma.

Armadilha: Vue **não** é backend; **não** é sinônimo de Node.

---

### 1.7 Node.js (conceitos)

- **Runtime** que executa JavaScript **fora do browser** (motor V8).  
- Uso típico: **APIs HTTP**, CLI, build tools (Webpack, Vite rodam em Node), BFF, microsserviços JS.  
- Modelo clássico cobrável: **event-driven** + I/O **não bloqueante** (bom para muita conexão concurrente I/O-bound).  
- **npm** (ou yarn/pnpm): ecossistema de pacotes — frequentemente associado a Node, mas “npm = Node” é simplificação de prova.  
- **Express** (e similares) = frameworks **web para Node**, não confundir com React/Angular/Vue.

Flash: Node **não** renderiza tela sozinho; quem monta a UI no browser são os frameworks/libs de front (ou HTML estático).

---

### 1.8 Diferenças que caem em prova (checklist)

| Afirmação | Verdade? |
|-----------|----------|
| React é framework completo com DI nativa como Angular | **Não** — biblioteca de UI |
| Angular exige/prioriza TypeScript | **Sim** (padrão oficial) |
| Vue roda tipicamente no servidor como substituto de PostgreSQL | **Não** |
| Node.js é framework de componentes de tela | **Não** — runtime |
| React Native = React para DOM web | **Não** — nativo mobile |
| Front React pode consumir API feita em Node | **Sim** — camadas distintas |
| jQuery = mesmo papel arquitetural de Angular hoje | **Não** — jQuery é lib DOM clássica (edital lista à parte) |

---

### 1.9 Encaixe no ecossistema do edital (1 parágrafo)

HTML5/CSS3/Bootstrap/JS/TS = base da página e da linguagem. **React / Angular / Vue** = organizar a UI em componentes (SPA). **Node** = executar JS no servidor (API/tooling). **React Native** = mobile com DNA React. REST (já visto na semana 1) costuma ser o “contrato” entre front e back — independente de qual dos quatro você escolheu.

---

### 1.10 Cartão final (decorar em 20 s)

> **React** = lib UI + JSX + Virtual DOM · **React Native** = mobile nativo “estilo React” · **Angular** = framework TS completo · **Vue** = framework progressivo · **Node** = JS no servidor (não é front).

---

## 2. Questões de treino (1–15)

**1.** No edital de Analista Judiciário – TI do TRT4, React, Angular, Vue.js e Node.js aparecem no eixo de:

A) Desenvolvimento web e mobile (frameworks / runtime listados)  
B) Cabeamento estruturado 802.3  
C) Normas exclusivas de LGPD  
D) Substitutos do PostgreSQL  
E) Protocolos SNMP/QoS  

**2.** Em classificação cobrável de prova, o React é tipicamente:

A) Biblioteca focada em interfaces de usuário (UI) baseadas em componentes  
B) Sistema operacional de servidor  
C) Banco NoSQL oficial do CNJ  
D) Framework de injeção de dependência idêntico ao Spring  
E) Protocolo de e-mail  

**3.** O Angular, em visão de edital moderno, caracteriza-se por:

A) Ser um framework completo, com TypeScript como linguagem padrão de projetos  
B) Ser apenas um pré-processador CSS  
C) Substituir o TCP/IP  
D) Ser o nome oficial do motor V8  
E) Rodar exclusivamente como stored procedure no Oracle  

**4.** O Vue.js é bem descrito como:

A) Framework progressivo para interfaces, com componentes (ex.: arquivos `.vue`)  
B) Hypervisor de containers  
C) Implementação do protocolo LDAP  
D) Ferramenta de discovery Eureka  
E) Linguagem substituta do SQL  

**5.** O Node.js, corretamente, é:

A) Runtime que executa JavaScript fora do navegador (ex.: APIs no servidor)  
B) Biblioteca exclusiva de Virtual DOM do browser  
C) Framework mobile idêntico ao React Native  
D) Dialeto do HTML5  
E) Motor de regras do COBIT  

**6.** A diferença essencial entre React e React Native é:

A) React mira UI web (DOM); React Native mira apps com componentes nativos mobile  
B) São sinônimos absolutos  
C) React Native só roda em mainframes  
D) React é banco; React Native é rede  
E) React Native é a versão server-side do PostgreSQL  

**7.** Qual associação está correta?

A) Node.js → frequentemente usado para backends/APIs em JavaScript  
B) Angular → apenas manipulação pontual de DOM estilo jQuery 1.x  
C) Vue → runtime de kernel Linux  
D) React → protocolo SMTP  
E) React Native → substituição do DNS  

**8.** Em SPA construída com React ou Vue, o papel típico do Node no mesmo sistema é:

A) Expor API/serviços ou tooling de build — não “ser” o componente de tela  
B) Desenhar cada botão no DOM no lugar do React  
C) Eliminar a necessidade de HTTP  
D) Substituir o certificado TLS automaticamente por HTML  
E) Ser o único meio de escrever CSS  

**9.** TypeScript no ecossistema Angular:

A) É a linguagem padrão fortemente associada aos projetos Angular oficiais  
B) É proibida pelo framework  
C) Só existe dentro do Oracle 19c  
D) Substitui o conceito de componente  
E) É idêntica ao Assembly  

**10.** Virtual DOM está mais diretamente associado, em discursinho de prova, a:

A) Estratégias de atualização eficiente de UI em libs/frameworks como React (e Vue moderno)  
B) Particionamento de disco  
C) Algoritmo de roteamento OSPF  
D) Modelo de governança ITIL  
E) Criptografia AES  

**11.** Assinale a afirmação **incorreta**.

A) Angular costuma trazer roteamento e DI “de fábrica”  
B) React, sozinho, não é um stack completo de backend  
C) Node.js renderiza obrigatoriamente a tela no lugar do browser  
D) Vue pode crescer com roteador e gerência de estado do ecossistema  
E) Front e API Node podem comunicar-se via REST/JSON  

**12.** jQuery, listado no edital junto aos frameworks, difere de Angular principalmente porque:

A) É biblioteca clássica de manipulação DOM/Ajax, não um framework SPA completo moderno  
B) É o novo nome do Angular  
C) Substitui TypeScript  
D) É o runtime V8  
E) É o React Native oficial do Google  

**13.** “Event-driven” e I/O não bloqueante são atributos cobráveis típicos de:

A) Node.js  
B) CSS3 puro  
C) Meramente o Bootstrap 5  
D) UML 2.5  
E) BPMN exclusivo  

**14.** Qual par “ferramenta → camada” está adequado?

A) Vue.js → interface no cliente · Node.js → execução JS no servidor  
B) React → firewall de borda · Angular → switch L2  
C) Node → Virtual DOM exclusivo no Chrome · React → kernel  
D) Angular → protocolo DHCP · Vue → STP  
E) React Native → MER/normalização  

**15.** Para uma questão que pede “framework opiniado, completo, com TypeScript e DI”, a melhor resposta entre os quatro é:

A) Angular  
B) Só Node.js sem UI  
C) Apenas o arquivo HTML5 estático  
D) React Native sem qualificação  
E) npm isoladamente  

---

### Gabarito comentado (treino 1–15)

1. **A** — eixo web/mobile do Anexo II.  
2. **A** — React = lib de UI/componentes.  
3. **A** — framework + TS.  
4. **A** — progressivo / componentes.  
5. **A** — runtime fora do browser.  
6. **A** — web DOM × nativo mobile.  
7. **A** — Node como backend JS.  
8. **A** — API/tooling ≠ componente de tela.  
9. **A** — TS padrão Angular.  
10. **A** — Virtual DOM / UI.  
11. **C** — Node não “obrigatoriamente renderiza a tela” no lugar do browser.  
12. **A** — jQuery ≠ Angular SPA.  
13. **A** — modelo clássico Node.  
14. **A** — front Vue · back/runtime Node.  
15. **A** — Angular = completo/opiniado/TS/DI.

---

## 3. Questões de concurso (16–25)

**16.** (FCC – TRT-12/2023 – Analista Judiciário – TI) No React, a sintaxe correta do método usado para atualizar o estado de um componente é:

A) `this.componentState({ novo_estado: novo_valor });`
B) `this.updateState({ novo_estado: novo_valor });`
C) `this.setState({ novo_estado: novo_valor });`
D) `this.changeState({ novo_estado: novo_valor });`
E) `this.modifyState({ novo_estado: novo_valor });`

**17.** (FCC – TRT-1/2025 – Analista Judiciário – TI) Para criar um projeto Angular chamado `site` (com Node.js, npm e Angular CLI instalados), utiliza-se:

A) `ng @angular/site`
B) `npm new site`
C) `npm @angular/site`
D) `ng new site`
E) `create new projet site`

**18.** (FCC – TRF-4/2025 – Analista Judiciário – Análise de Sistemas) O comando Angular CLI para gerar o serviço `processos` em `src/app/core/services/` é:

A) `ng generate component core/services/processos`
B) `npx g s core/services/processos`
C) `ng generate service core/services/processos`
D) `npm g s service core/services/processos`
E) `npm generate service core/services/processos`

**19.** (FCC – TJ-CE/2022 – Analista Judiciário – Sistemas da Informação) Acerca do Vue.js, é correto que:

A) Para renderizar condicionalmente um bloco, utilizam-se as diretivas `vueIf`, `vueElse` e `vueElseIf`.
B) Todo aplicativo Vue começa criando-se uma nova instância da aplicação com a função `createApp`.
C) Uma instância da aplicação renderiza todo o aplicativo antes do método `.mount()` ser chamado.
D) Para definir um manipulador de erros (`errorHandler`) utiliza-se um objeto `catchComponent`.
E) Deve haver somente uma instância de aplicação na mesma página.

**20.** (FCC – TRT-20/2024 – Analista Judiciário – TI) Em Node.js, para garantir sequência e tratamento de erros em chamadas assíncronas:

A) usar `Promise.all()` e ignorar erros.
B) envolver as chamadas em `try/catch` e usar `await`.
C) usar `then/catch/finally` e `execute()` em `Connection`.
D) fazer todas as chamadas assíncronas usando a função `async`/`await` com `try/catch` para tratar as exceções.
E) colocar todas as chamadas assíncronas em uma função `setTimeout` com `throws` para tratar exceções.

**21.** (FCC – TRT-20/2024 – Técnico Judiciário – TI) Em Express, a lacuna para POST `/webhook` é:

A) `msg.post('/webhook', (req, send)`
B) `app.post('/webhook,(req, http)`
C) `post.msg('/webhook',(http, send)`
D) `msg.post('/webhook', (req, res)`
E) `app.post('/webhook', (req, res)`

**22.** (FCC – TRT-20/2024 – Técnico Judiciário – TI) Em React funcional, para atualizar estado do input:

A) `this.setState` em componentes funcionais
B) `setState` em classe como abordagem ideal moderna
C) `useEffect` para definir o valor a cada `onChange`
D) Hook `useState` e atualizar no `onChange`
E) `useState` + `useEffect` obrigatórios juntos

**23.** (FCC – TRT-12/2023 – Analista Judiciário – TI) Tupla TypeScript para `['Paulo Freitas', 'Rua ...', 45]`:

A) `let pessoa[3]: [any];`
B) `let pessoa[3];`
C) `let pessoa: [string, string, number];`
D) `let pessoa: typeof[String, String, Number];`
E) `let pessoa[string, string, number];`

**24.** (FCC – TRT-20/2024 – Analista Judiciário – TI) `lks.classList.toggle("visible")`:

A) adiciona ou remove a classe `visible` a cada chamada.
B) lista as classes CSS na tela.
C) falha por erro em `getElementById`.
D) apenas torna o elemento visível.
E) erro: `toggle` não se aplica a `class`.

**25.** (FCC – TRT-6/2025 – Analista Judiciário – TI) Seleção do elemento com id `imagens`:

A) `document.querySelector('#imagens');`
B) `document.getElementFromId('imagens');`
C) `document.querySelector('.imagens');`
D) `document.getElementbyId('#imagens');`
E) `document.getIdentification('#imagens');`

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **C** | FCC TRT-12/2023 — `this.setState` |
| 17 | **D** | FCC TRT-1/2025 — `ng new site` |
| 18 | **C** | FCC TRF-4/2025 — `ng generate service` |
| 19 | **B** | FCC TJ-CE/2022 — Vue `createApp` |
| 20 | **B** | FCC TRT-20/2024 — `await` + `try/catch` |
| 21 | **E** | FCC TRT-20/2024 — Express `app.post` |
| 22 | **D** | FCC TRT-20/2024 — `useState` |
| 23 | **C** | FCC TRT-12/2023 — tupla TS (adjacente) |
| 24 | **A** | FCC TRT-20/2024 — `classList.toggle` (adjacente) |
| 25 | **A** | FCC TRT-6/2025 — `querySelector` (adjacente) |

**Notas rápidas:** 16–22 núcleo React/Angular/Vue/Node; 23–25 adjacentes JS/TS/DOM do mesmo eixo web.

---

## 4. Gabarito consolidado

| Q | Resp | Q | Resp | Q | Resp | Q | Resp | Q | Resp |
|---|------|---|------|---|------|---|------|---|------|
| 1 | A | 2 | A | 3 | A | 4 | A | 5 | A |
| 6 | A | 7 | A | 8 | A | 9 | A | 10 | A |
| 11 | C | 12 | A | 13 | A | 14 | A | 15 | A |
| 16 | C | 17 | D | 18 | C | 19 | B | 20 | B |
| 21 | E | 22 | D | 23 | C | 24 | A | 25 | A |

---

*Arquivo 07 · Frameworks React / Angular / Vue / Node (conceitos e diferenças) · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 5. Sem aprofundar APIs.*

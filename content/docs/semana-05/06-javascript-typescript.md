+++
title = '06 — JavaScript · TypeScript · Frameworks (visão)'
date = '2026-10-20'
draft = false
weight = 6
tags = ['semana-05', 'javascript', 'typescript']
categories = ['estudo']
+++
# JavaScript · TypeScript · Frameworks (visão) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — JavaScript, TypeScript e frameworks web (conceitos)  
**Escopo deste arquivo:** JavaScript no browser e além (ES moderno); DOM/eventos/assíncrono; TypeScript como JS tipado/compilado; contraste cobrável React × Angular × Vue × Node × jQuery; armadilhas FCC de sinônimo (JS×TS×Java) e de “framework X faz tudo”.  
**Fora de escopo aqui:** ASP.NET Core (04); HTML/CSS/Bootstrap aprofundado (05); React Native mobile em profundidade; JSP/JSF (cauda longa / semana 10).

---

## 1. Material

### 1.1 Por que isso cai no edital

No bloco **Desenvolvimento web e mobile** aparecem: **JavaScript**; **TypeScript**; **frameworks (React, React Native, Angular, jQuery, Node.js, Vue.js etc.)**.

Leitura da semana 5 (cronograma): **conceitos e diferenças**, sem aprofundar todos os frameworks linha a linha.

| Peça | Papel em prova |
|------|----------------|
| **JavaScript (JS)** | Linguagem dinâmica da web (e além, via engines) |
| **TypeScript (TS)** | Superset tipado que **compila** para JavaScript |
| **Frameworks/libs** | React, Angular, Vue, jQuery, Node… — papéis distintos |

Flash: a FCC mistura **Java** com **JavaScript** e trata **TypeScript** como “outra VM”. Separe os nomes.

### 1.2 JavaScript — cartão seco

**JavaScript** é linguagem de **programação** de alto nível, tipagem dinâmica, multiparadigma (prototipação/OOP + funcional), padronizada como **ECMAScript (ES)**.

| Onde roda | Exemplos cobráveis |
|-----------|-------------------|
| **Browser** | DOM, eventos, fetch, SPA |
| **Servidor** | **Node.js** (engine V8 + APIs de SO) |
| Outros | Desktop/mobile híbridos (reconhecer ecossistema) |

O que JS **não** é:

1. Não é **Java** (nome parecido; linguagens distintas).  
2. Não é HTML nem CSS (embora interaja com ambos).  
3. Não é o Bootstrap (framework CSS; JS de componentes é auxiliar).  
4. Não exige compilação obrigatória (engines interpretam/JIT) — distinto do fluxo típico do **TypeScript**.

### 1.3 ES moderno — ideias que caem

| Recurso | Frase de prova |
|---------|----------------|
| `let` / `const` | Escopo de bloco; `const` não reatribui a binding |
| `var` | Escopo de função / hoisting clássico (legado) |
| Arrow functions | Sintaxe curta; `this` lexical (visão) |
| Template strings | `` `Olá ${nome}` `` |
| Destructuring / spread | Extrair e copiar estruturas |
| Módulos (`import`/`export`) | Modularização ES |
| Classes (`class`) | Açúcar sintático sobre protótipos |

Tipagem: **dinâmica** / fraca na prática do dia a dia — valores têm tipo em runtime; variáveis não são declaradas com tipo estático obrigatório (contrastar com TS).

### 1.4 DOM, eventos e browser

| Conceito | Leitura FCC |
|----------|-------------|
| **DOM** | Representação em árvore do documento HTML; JS manipula nós |
| **Eventos** | `click`, `submit`, `load`…; listeners |
| **BOM** | `window`, `location`, `localStorage` (visão) |
| **fetch / XHR** | Chamadas HTTP assíncronas ao backend |

Cartão:

> JS no browser **altera** o DOM e **reage** a eventos — não “é” o HTML.

Armadilha: afirmar que mudar CSS só é possível via Java no servidor — no cliente, JS/CSSOM também atuam.

### 1.5 Assíncrono: callbacks, Promises, async/await

| Modelo | Ideia |
|--------|-------|
| **Callback** | Função passada para rodar depois (callback hell clássico) |
| **Promise** | Objeto que representa resultado futuro (`then`/`catch`) |
| **async/await** | Sintaxe que consome Promises de forma sequencial legível |
| **Event loop** | Modelo de concorrência cooperativa do JS (single-threaded + fila) |

Flash: enunciado “não bloqueia a UI enquanto espera HTTP” → assíncrono / não confundir com threads Java obrigatórias.

### 1.6 TypeScript — leitura de prova

**TypeScript** é um **superset** do JavaScript que adiciona **tipos estáticos opcionais** e recursos de linguagem; o fluxo típico é **compilar (transpile)** TypeScript → JavaScript executável nas engines.

| Ideia | Frase cobrável |
|-------|----------------|
| **Superset** | JS válido tende a ser TS válido (com ressalvas de config `strict`) |
| **Tipos** | `string`, `number`, `boolean`, unions, interfaces, generics (visão) |
| **Compile-time** | Erros de tipo **antes** de rodar |
| **Saída** | Ainda é **JavaScript** no runtime do browser/Node |

Tabela de ouro:

| | **JavaScript** | **TypeScript** |
|---|----------------|----------------|
| Tipos | Dinâmicos em runtime | Anotações estáticas + checagem |
| Execução direta nas engines | Sim | Após compile → JS |
| Papel | Linguagem runtime da web/Node | Ferramenta/linguagem de desenvolvimento tipada |

Síntese:

> **TS não substitui a engine** — gera JS.  
> **TS ≠ Java**.  
> **TS ≠ “outro HTML”**.

### 1.7 Frameworks e libs — mapa de papéis (sem aprofundar todos)

| Nome | Papel cobrável |
|------|----------------|
| **React** | Biblioteca/UI baseada em **componentes** e **Virtual DOM**; ecossistema rico; JSX |
| **Angular** | **Framework** completo (opinião forte): módulos/componentes, DI, RxJS; tipicamente **TypeScript** |
| **Vue.js** | Framework progressivo de componentes; template + reatividade |
| **jQuery** | Biblioteca clássica de DOM/Ajax; menos central em SPAs modernas; **≠** Bootstrap 5 obrigatório |
| **Node.js** | **Runtime** JS no servidor (não é framework de UI) |
| **React Native** | UI mobile com paradigma React (reconhecer no edital) |

Contrastes de ouro (FCC):

| Afirmação | Veredito |
|-----------|----------|
| Node.js é framework front-end como React | **Falso** — runtime servidor |
| Angular é só uma lib de DOM tipo jQuery | **Falso** — framework full |
| React = Angular = Vue (idênticos) | **Falso** — ideias próximas (componentes), stacks diferentes |
| jQuery é obrigatório em todo site moderno | **Falso** |
| TypeScript é exclusividade Angular | **Falso** — Angular **adota** fortemente; React/Vue **podem** usar TS |

### 1.8 SPA e consumo de APIs (visão)

**SPA** (*Single Page Application*): navegação no cliente com JS; troca de views sem reload completo clássico; consome **APIs REST/JSON** (eixo já visto na semana 1).

```
[ Browser: React/Angular/Vue ]
        │  HTTP/JSON
        ▼
[ API: Spring / ASP.NET Core / Node … ]
```

Flash: front JS **≠** substituir o backend — em geral **consome** a API.

### 1.9 Java × JavaScript × TypeScript (anti-distrator máximo)

| Nome | O que é |
|------|---------|
| **Java** | Linguagem tipada da JVM (seu forte Spring/PDPJ) |
| **JavaScript** | Linguagem das engines web/Node (ECMAScript) |
| **TypeScript** | JS + tipos; compila para JS |

Frase para decorar:

> Nome parecido **não** é parentesco. Java ≠ JavaScript ≠ TypeScript (embora TS ⊂ evolução do ecossistema JS).

### 1.10 O que a FCC tipicamente testa neste tema

1. JS manipula **DOM**/eventos; não é HTML.  
2. Assíncrono (Promise/async) vs bloqueio.  
3. TS = **tipado + compile para JS**.  
4. Node = **runtime servidor**, não lib de UI.  
5. React/Angular/Vue = componentes/SPA (diferenças de “lib vs framework”).  
6. jQuery = lib clássica DOM — não confundir com BS5 obrigatório.  
7. Nunca trocar Java por JavaScript no enunciado.

### 1.11 Mini-síntese (cartão mental)

> **JS** = linguagem da web/Node (DOM, eventos, async).  
> **TS** = JS tipado que **vira JS** no build.  
> **React/Angular/Vue** = UI componentes/SPA (papéis distintos).  
> **Node** = JS no servidor. **jQuery** = DOM clássico.  
> **Java ≠ JavaScript ≠ TypeScript**.

---

## 2. Questões de treino (1–15)

**1.** JavaScript, no eixo do edital, é melhor descrito como:

A) Linguagem de programação usada no browser e, via runtimes como Node.js, também no servidor  
B) Protocolo de enlace 802.1Q  
C) Único SGBD do edital  
D) Norma ISO 12207  
E) Cabo fibra óptica  

**2.** TypeScript caracteriza-se tipicamente por:

A) Ser um dialeto SQL do Oracle  
B) Acrescentar tipagem estática ao ecossistema JS e compilar/transpilar para JavaScript  
C) Substituir o TCP  
D) Ser o kernel do Linux  
E) Ser sinônimo de HTML5  

**3.** Assinale a associação **correta**:

A) Java ≡ JavaScript ≡ TypeScript (mesma linguagem)  
B) Java ≠ JavaScript; TypeScript relaciona-se ao ecossistema JavaScript  
C) TypeScript é o nome do Kestrel  
D) JavaScript é folha de estilo  
E) Node.js é protocolo SMTP  

**4.** O DOM, no contexto JS no browser, refere-se a:

A) Árvore/objeto que representa o documento; manipulável via script  
B) Disco óptico removível  
C) Protocolo DHCP  
D) Artefato PMBOK exclusivo  
E) Switch camada 2  

**5.** Promises e `async/await` no JavaScript atendem prioritariamente a:

A) Modelar operações assíncronas de forma mais estruturada que callbacks aninhados  
B) Configurar VLANs  
C) Assinar XML Schema obrigatoriamente  
D) Substituir o DNS  
E) Criar tabelas DDL  

**6.** Node.js, na leitura cobrável, é:

A) Um framework de componentes UI idêntico ao React  
B) Um runtime que executa JavaScript no servidor  
C) Um SGBD relacional  
D) Um padrão Scrum  
E) Um tipo de cabo UTP  

**7.** React é tipicamente associado a:

A) Componentes de UI e abordagem com Virtual DOM / ecossistema front-end  
B) Único protocolo BGP  
C) Ferramenta exclusiva de backup em fita  
D) Dialeto PL/SQL  
E) Norma COBIT EDM01 apenas  

**8.** Angular, em contraste com uma lib mínima de DOM, costuma ser descrito como:

A) Framework front-end mais completo/opinião forte, com uso intenso de TypeScript  
B) Apenas um arquivo CSS do Bootstrap  
C) O servidor embutido do ASP.NET  
D) O registry Eureka  
E) Um tipo `input` HTML  

**9.** Vue.js situa-se no mapa de prova como:

A) Framework/lib progressiva de UI baseada em componentes  
B) Protocolo FTP ativo  
C) Ferramenta exclusiva ITIL  
D) Sistema de arquivos NTFS  
E) Cipher suite TLS 1.0 apenas  

**10.** Qual afirmação é **incorreta**?

A) TypeScript em geral compila para JavaScript  
B) JavaScript pode manipular o DOM  
C) Node.js executa JS fora do browser  
D) JavaScript e Java são a mesma especificação ECMA  
E) React e Vue trabalham com componentes  

**11.** jQuery, na visão atual de concurso, é melhor lembrada como:

A) Biblioteca clássica para facilitar manipulação de DOM e Ajax  
B) Runtime servidor substituto do Node  
C) Obrigação legal em todo Bootstrap 5  
D) Linguagem tipada que substitui TypeScript  
E) Protocolo IPv6  

**12.** Em uma SPA que consome API REST JSON:

A) O front em React/Angular/Vue tipicamente chama o backend via HTTP; não “é” o banco  
B) A SPA substitui obrigatoriamente o TCP por UDP em todas as camadas  
C) JSON só existe em COBOL  
D) SPA proíbe qualquer uso de CSS  
E) SPA é sinônimo de Flyway  

**13.** Relativamente a `let`/`const` versus `var` (visão ES moderno):

A) `let`/`const` têm escopo de bloco; `var` tem modelo clássico de função/hoisting  
B) `const` permite reatribuir a binding livremente sempre  
C) `var` é o nome do TypeScript compiler  
D) `let` existe apenas no Java  
E) `const` é tag HTML5  

**14.** Distrator clássico neste subtópico é:

A) Separar Java de JavaScript e TypeScript  
B) Associar Node a runtime servidor  
C) Tratar TypeScript como outra VM que não gera JS, ou Java ≡ JS  
D) Lembrar que React/Angular/Vue são do eixo de UI componentes  
E) Associar async a Promises  

**15.** Qual linha resume melhor o cartão mental deste arquivo?

A) JS = Java; TS = HTML; Node = CSS; React = DHCP  
B) JS linguagem web/Node; TS tipado→JS; React/Angular/Vue UI; Node runtime servidor; Java ≠ JS ≠ TS  
C) TypeScript substitui o browser  
D) jQuery é obrigatório no BS5 e no Angular juntos sempre  
E) DOM é protocolo de e-mail  

---

### Gabarito comentado (treino 1–15)

1. **A** — Linguagem browser + Node.  
2. **B** — Tipado + compile para JS.  
3. **B** — Java ≠ JS; TS no ecossistema JS.  
4. **A** — Árvore do documento.  
5. **A** — Assíncrono estruturado.  
6. **B** — Runtime servidor.  
7. **A** — Componentes / Virtual DOM.  
8. **A** — Framework completo + TS.  
9. **A** — UI progressiva/componentes.  
10. **D** — Java ≠ JS/ECMA.  
11. **A** — Lib clássica DOM/Ajax.  
12. **A** — Front consome API.  
13. **A** — Escopo de bloco vs var.  
14. **C** — Sinônimos Java/JS/TS = armadilha.  
15. **B** — Síntese do arquivo 06.

---

## 3. Questões de concurso (16–25)

**16.** (FCC – TRT-20/2024 – Analista Judiciário – TI) Considere a função:

```javascript
function menu() {
  var lks = document.getElementById("links");
  lks.classList.toggle("visible");
}
```

A linha `lks.classList.toggle("visible");`

A) faz com que o elemento identificado como `links` tenha a classe `visible` adicionada ou removida cada vez que a função `menu()` for chamada.
B) torna visível a lista de classes CSS que o elemento contido em `lks` possui.
C) não será executada, pois a linha anterior possui erro em `getElementById`.
D) aplica o valor `visible` ao elemento cujo id é `links`, tornando-o visível.
E) gerará um erro, pois `toggle` não pode ser aplicada no atributo `class`.

**17.** (FCC – TRT-6/2025 – Analista Judiciário – TI) Para buscar o primeiro elemento no DOM com id `imagens`:

A) `document.querySelector('#imagens');`
B) `document.getElementFromId('imagens');`
C) `document.querySelector('.imagens');`
D) `document.getElementbyId('#imagens');`
E) `document.getIdentification('#imagens');`

**18.** (FCC – TRT-1/2025 – Analista Judiciário – TI) Substituindo o bloco `if` / `else if` / `else` por operador ternário, mantendo o mesmo resultado:

A) sintaxe com `->`
B) `acesso = tipoCaso === "criminal" ? "Audiência aberta ao público." : tipoCaso === "familiar" ? "Audiência sigilosa." : "Acesso analisado caso a caso.";`
C) sintaxe com `>>`
D) sintaxe com `|` no lugar de `:`
E) começar com `:` após a condição

**19.** (FCC – TRT-12/2023 – Analista Judiciário – TI) Para a tupla TypeScript `pessoa = ['Paulo Freitas', 'Rua Barreto de Freitas', 45]`, a definição correta é:

A) `let pessoa[3]: [any];`
B) `let pessoa[3];`
C) `let pessoa: [string, string, number];`
D) `let pessoa: typeof[String, String, Number];`
E) `let pessoa[string, string, number];`

**20.** (FCC – TRT-22/2022 – Analista Judiciário – TI) Em `let x: [ I ]; x = [2022, false, 'TRT DA 22ª REGIÃO'];`, I deve ser:

A) `3`
B) `number, boolean, string`
C) `new array(3)`
D) `int(4), boolean, string(17)`
E) `array="multitype"`

**21.** (FCC – TRT-20/2024 – Analista Judiciário – TI) Em Node.js, para operações assíncronas sequenciais com tratamento de erros, a abordagem mais apropriada é:

A) usar `Promise.all()` e ignorar tratamento de erros.
B) envolver as chamadas assíncronas em um bloco `try/catch` e usar `await`.
C) usar `then()`, `catch()`, `finally()` e `execute()` em todas as chamadas da classe `Connection`.
D) fazer todas as chamadas com `async`/`await` com `try/catch` (redação alternativa da banca; gabarito oficial aponta B).
E) colocar as chamadas em `setTimeout` com `throws`.

**22.** (FCC – TRT-20/2024 – Técnico Judiciário – TI) Em webhook Express/Node.js, a lacuna para POST em `/webhook` é:

A) `msg.post('/webhook', (req, send)`
B) `app.post('/webhook,(req, http)`
C) `post.msg('/webhook',(http, send)`
D) `msg.post('/webhook', (req, res)`
E) `app.post('/webhook', (req, res)`

**23.** (FCC – TRT-12/2023 – Analista Judiciário – TI) No React, a sintaxe correta para atualizar o estado de um componente é:

A) `this.componentState({ novo_estado: novo_valor });`
B) `this.updateState({ novo_estado: novo_valor });`
C) `this.setState({ novo_estado: novo_valor });`
D) `this.changeState({ novo_estado: novo_valor });`
E) `this.modifyState({ novo_estado: novo_valor });`

**24.** (FCC – TRT-20/2024 – Técnico Judiciário – TI) Em React (componente funcional), para atualizar o estado do campo de entrada:

A) `this.setState` em componentes funcionais
B) `setState` em classe com `this.state.input` como abordagem ideal moderna
C) Hook `useEffect` para definir o valor a cada alteração
D) Hook `useState` e atualizar no evento `onChange`
E) `useState` e `useEffect` obrigatoriamente juntos

**25.** (FCC – TRT-20/2024 – Analista Judiciário – TI) A media query `@media screen and (max-width: 480px)`:

A) executa em telas maiores que 480px.
B) é chamada via `@import query('@media')`.
C) executa só se a largura for exatamente 480px.
D) tem sintaxe incorreta por falta de `and`.
E) executa apenas se a largura for menor ou igual a 480px.

---

### Gabarito comentado (concurso 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **A** | FCC TRT-20/2024 — `classList.toggle` |
| 17 | **A** | FCC TRT-6/2025 — `querySelector('#imagens')` |
| 18 | **B** | FCC TRT-1/2025 — ternário aninhado |
| 19 | **C** | FCC TRT-12/2023 — tupla TS |
| 20 | **B** | FCC TRT-22/2022 — tipos da tupla |
| 21 | **B** | FCC TRT-20/2024 — `await` + `try/catch` |
| 22 | **E** | FCC TRT-20/2024 — `app.post` |
| 23 | **C** | FCC TRT-12/2023 — `setState` |
| 24 | **D** | FCC TRT-20/2024 — `useState` + `onChange` |
| 25 | **E** | FCC TRT-20/2024 — media query (adjacente CSS) |

**Notas rápidas:** 16–20 núcleo JS/TS/DOM; 21–24 Node/React; 25 CSS adjacente do edital web.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **linguagem dinâmica web/Node / DOM / async**? → **JavaScript**.  
2. É **tipos + compile para JS**? → **TypeScript**.  
3. É **runtime servidor JS**? → **Node.js** (não UI).  
4. É **UI componentes/SPA**? → React / Angular / Vue (papéis).  
5. Java ≡ JavaScript? → **Nunca** neste arquivo.

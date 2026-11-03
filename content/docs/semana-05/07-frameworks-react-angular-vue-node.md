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

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto do edital de Analista Judiciário – Área de TI, React, Angular e Vue.js têm em comum, em regra:

A) serem tecnologias voltadas prioritariamente à construção de interfaces web baseadas em componentes, tipicamente no cliente  
B) serem motores de banco relacional equivalentes ao PostgreSQL 18  
C) serem protocolos da camada de enlace  
D) substituírem integralmente o sistema operacional do servidor de aplicação  
E) serem sinônimos estritos de Node.js  

**17.** Acerca do Node.js, é correto afirmar:

A) Trata-se de ambiente de execução de JavaScript fora do navegador, amplamente usado em serviços HTTP e ferramentas de desenvolvimento  
B) É o nome da especificação oficial do HTML5  
C) É framework mobile nativo mantido exclusivamente como fork do Angular  
D) Impede qualquer uso de TypeScript no ecossistema npm  
E) Equivale, por definição, ao Virtual DOM do React  

**18.** Diferenciando React e Angular em prova objetiva, a descrição mais fiel é:

A) React é biblioteca de UI; Angular é framework mais completo (roteamento, DI, forms etc.), com TypeScript como padrão  
B) Ambos são apenas pré-processadores SASS  
C) Angular roda só no kernel; React só em firmware  
D) React inclui DI nativa idêntica ao Spring Boot por especificação W3C  
E) Não há qualquer diferença conceitual reconhecível  

**19.** Avalie os itens.

I. React Native permite desenvolver aplicações móveis com paradigma semelhante ao React, gerando componentes nativos.  
II. Vue.js é runtime de servidor que substitui o Node.js em todos os cenários.  
III. Uma SPA em Angular pode consumir API REST implementada em Node.js.

Está correto o que se afirma em:

A) I e III, apenas  
B) II, apenas  
C) I e II, apenas  
D) III, apenas  
E) I, II e III  

**20.** Assinale a opção em que a associação está **incorreta**.

A) JSX → sintaxe associada ao ecossistema React  
B) SFC `.vue` → componente de arquivo único no Vue  
C) TypeScript first → Angular  
D) Express → framework web tipicamente sobre Node.js  
E) Virtual DOM → definição exclusiva e privativa do protocolo FTP  

**21.** Em arquitetura web típica de concurso, o papel mais adequado para o Node.js é:

A) executar lógica de servidor / API em JavaScript, podendo integrar-se a fronts React, Angular ou Vue  
B) substituir o CSS3 como linguagem de estilo obrigatória  
C) ser o único meio de desenhar layout Bootstrap  
D) gerenciar VLANs 802.1Q no switch de acesso  
E) emitir pareceres de Direito Administrativo  

**22.** Sobre Vue.js, a afirmação compatível com o edital é:

A) Framework (progressivo) para interfaces, listado entre os frameworks web do Anexo II  
B) Norma ISO de cabeamento  
C) Ferramenta exclusiva de governança COBIT  
D) Dialeto PL/SQL  
E) Container runtime idêntico ao Docker  

**23.** A principal razão para **não** classificar o Node.js como “framework de UI equivalente ao React” é:

A) Node é runtime de execução; React é biblioteca de interface — finalidades distintas  
B) Node só existe dentro do browser Chrome  
C) React é exclusivamente servidor de e-mail  
D) Ambos são bancos relacionais  
E) A FCC proíbe citar os dois no mesmo edital  

**24.** Considerando o pacote “React, React Native, Angular, Node.js, Vue.js” do edital, assinale a alternativa correta.

A) React Native distingue-se do React web por mirar plataformas móveis nativas, ainda que compartilhe o modelo de componentes  
B) Angular e Node.js são a mesma ferramenta com dois nomes comerciais  
C) Vue.js é obrigatoriamente o banco H2  
D) Todo projeto React elimina a necessidade de JavaScript  
E) Node.js é a especificação do Bootstrap 5  

**25.** Para revisão rápida no padrão FCC, a sequência **correta** é:

A) React/Angular/Vue → UI no cliente · Node → JS no servidor · React Native → mobile nativo com DNA React  
B) React → SNMP · Angular → DHCP · Vue → STP · Node → QoS  
C) Todos os quatro são apenas sinônimos de jQuery  
D) Node renderiza JSX no lugar do Angular por norma CNJ  
E) Vue substitui LGPD e ENSEC-PJ  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **A** |
| 17 | **A** |
| 18 | **A** |
| 19 | **A** |
| 20 | **E** |
| 21 | **A** |
| 22 | **A** |
| 23 | **A** |
| 24 | **A** |
| 25 | **A** |

**Notas rápidas:** 16 A (UI/componentes no cliente); 17 A (runtime); 18 A (lib × framework+TS); 19 A (I e III; II falso — Vue ≠ substituto universal do Node); 20 E (Virtual DOM ≠ FTP); 21 A (API/servidor); 22 A; 23 A (runtime ≠ UI); 24 A; 25 A (mapa mental completo).

---

## 4. Gabarito consolidado

| Q | Resp | Q | Resp | Q | Resp | Q | Resp | Q | Resp |
|---|------|---|------|---|------|---|------|---|------|
| 1 | A | 2 | A | 3 | A | 4 | A | 5 | A |
| 6 | A | 7 | A | 8 | A | 9 | A | 10 | A |
| 11 | C | 12 | A | 13 | A | 14 | A | 15 | A |
| 16 | A | 17 | A | 18 | A | 19 | A | 20 | E |
| 21 | A | 22 | A | 23 | A | 24 | A | 25 | A |

---

*Arquivo 07 · Frameworks React / Angular / Vue / Node (conceitos e diferenças) · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 5. Sem aprofundar APIs.*

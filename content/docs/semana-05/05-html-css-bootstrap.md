+++
title = '05 — HTML5 · CSS3 · Bootstrap 5'
date = '2026-10-19'
draft = false
weight = 5
tags = ['semana-05', 'html', 'css']
categories = ['estudo']
+++
# HTML5 · CSS3 · Bootstrap 5 · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — HTML5, CSS3, Bootstrap 5  
**Escopo deste arquivo:** HTML5 semântico e formulários; modelo de caixa e seletores CSS3; Flexbox/Grid (visão); Bootstrap 5 (grid, componentes, utilities) e contraste cobrável com Bootstrap 4; armadilhas FCC de misturar HTML×CSS×JS e BS4×BS5.  
**Fora de escopo aqui:** ASP.NET Core (04); JavaScript/TypeScript e frameworks (06); acessibilidade profunda WCAG linha a linha; design visual avançado.

---

## 1. Material

### 1.1 Por que isso cai no edital

No bloco **Desenvolvimento web e mobile** aparecem: **HTML5**; **CSS3**; **Bootstrap 5**; ao lado de JavaScript, TypeScript e frameworks.

Leitura cobrável:

| Peça | Papel em prova |
|------|----------------|
| **HTML5** | Estrutura/semântica do documento e mídia |
| **CSS3** | Apresentação (layout, cores, responsivo) |
| **Bootstrap 5** | Framework CSS de componentes + grid responsivo |

Flash: a FCC ama enunciados do tipo “a tag X é CSS” ou “Bootstrap é linguagem de programação”. Separe **estrutura (HTML)**, **estilo (CSS/Bootstrap)** e **comportamento (JS — arquivo 06)**.

### 1.2 HTML5 — âncora mental

**HTML** (*HyperText Markup Language*) descreve a **estrutura** e o **significado** do conteúdo. HTML5 é a revisão moderna com semântica, APIs e mídia nativas.

| Ideia | Frase cobrável |
|-------|----------------|
| **Marcação** | Tags/elementos + atributos |
| **Semântica** | Elementos com significado (`header`, `nav`, `main`, `article`, `section`, `aside`, `footer`) |
| **Documento** | `<!DOCTYPE html>`, `html`, `head`, `body` |
| **Não é** | Linguagem de programação (não tem laço/variável como JS) |

Elementos semânticos — cartão seco:

| Elemento | Uso típico em prova |
|----------|---------------------|
| `header` | Cabeçalho da página ou de seção |
| `nav` | Navegação principal |
| `main` | Conteúdo principal único |
| `article` | Conteúdo independente/composable |
| `section` | Seção temática |
| `aside` | Conteúdo tangencial (sidebar) |
| `footer` | Rodapé |
| `figure` / `figcaption` | Mídia + legenda |

Armadilha: `div` e `span` **existem** e são genéricos — semântica ≠ “proibir div”; prova premia escolher a tag **com significado** quando o enunciado pede.

### 1.3 Formulários e inputs HTML5

| Tipo / recurso | O que cai |
|----------------|-----------|
| `input type="email"`, `url`, `number`, `date`, `tel`, `search` | Tipos novos/modernos vs `text` puro |
| `required`, `placeholder`, `pattern` | Validação/UX no markup |
| `label` + `for` / wrapping | Acessibilidade e associação ao campo |
| `select`, `textarea`, `button` | Controles clássicos |
| `form` method/action | Envio (visão; detalhe HTTP no eixo redes/REST) |

Flash: validação HTML5 **no cliente** ≠ garantia de segurança no servidor (distrator: “required no HTML basta para segurança”).

### 1.4 Mídia e APIs HTML5 (reconhecer)

| Recurso | Leitura de prova |
|---------|------------------|
| `<audio>`, `<video>` | Mídia nativa sem plugin Flash |
| `<canvas>` | Desenho bitmap via script |
| `<svg>` | Gráficos vetoriais (marcação/XML-like) |
| Local Storage / Session Storage | Persistência no browser (API web; citar se enunciado pedir) |
| Geolocation, etc. | APIs do browser — **reconhecer**, não aprofundar |

Cartão: HTML5 **reduz dependência de plugins** para áudio/vídeo — frase clássica de banca.

### 1.5 CSS3 — o que é e o que não é

**CSS** (*Cascading Style Sheets*) controla a **apresentação**: cores, tipografia, layout, responsividade.

| Conceito | Frase FCC |
|----------|-----------|
| **Seletor** | Quem recebe o estilo (`p`, `.classe`, `#id`, `[attr]`) |
| **Cascata + especificidade** | Quem ganha quando há conflito |
| **Box model** | content → padding → border → margin |
| **box-sizing** | `content-box` vs `border-box` (largura inclui ou não padding/borda) |
| **Display** | `block`, `inline`, `inline-block`, `none`, `flex`, `grid` |

Ouro do box model:

```
┌──────── margin ────────┐
│ ┌────── border ──────┐ │
│ │ ┌── padding ────┐  │ │
│ │ │   content     │  │ │
│ │ └───────────────┘  │ │
│ └────────────────────┘ │
└────────────────────────┘
```

Armadilha: confundir `margin` (fora da borda) com `padding` (dentro).

### 1.6 Seletores e unidades (amostra cobrável)

| Seletor | Significado |
|---------|-------------|
| `E F` | Descendente |
| `E > F` | Filho direto |
| `E + F` | Irmão adjacente |
| `E ~ F` | Irmãos seguintes |
| `.classe` / `#id` | Classe / id |
| `:hover`, `:focus`, `:nth-child` | Pseudo-classes |
| `::before`, `::after` | Pseudo-elementos |

Unidades: `px`, `%`, `em`, `rem`, `vh`/`vw` — **rem** relativo à raiz; **em** ao elemento pai (visão).

### 1.7 Flexbox e Grid (visão de prova)

| | **Flexbox** | **Grid** |
|---|-------------|----------|
| Ideia | Layout em **uma** dimensão (linha ou coluna) dominante | Layout em **duas** dimensões (linhas × colunas) |
| Palavras-chave | `display: flex`, `justify-content`, `align-items`, `flex-wrap` | `display: grid`, `grid-template-columns`, `gap`, áreas |
| Uso típico | Barras, alinhamento, componentes em fileira | Páginas / regiões 2D |

Flash: enunciado “eixo principal/cruzado, alinhar itens numa direção” → **Flex**. “Linhas e colunas explícitas, template de áreas” → **Grid**. Podem **combinar**.

Responsivo: media queries (`@media (max-width: …)`), viewport meta no HTML — trio clássico mobile-first.

### 1.8 Bootstrap 5 — leitura cobrável

**Bootstrap** é **framework front-end** (CSS + JS opcional de componentes) para UI responsiva rápida: grid, utilitários, componentes prontos (navbar, modal, cards…).

| Peça BS5 | Papel |
|----------|-------|
| **Grid** | Sistema de colunas (tipicamente 12) responsivo |
| **Containers** | `container` / `container-fluid` |
| **Breakpoints** | `sm`, `md`, `lg`, `xl`, `xxl` |
| **Utilities** | Classes utilitárias (spacing, flex, display, color…) |
| **Components** | Navbar, modal, dropdown, forms, alerts… |

Cartão seco:

> Bootstrap **não** substitui HTML — **usa** classes sobre marcação HTML.  
> Bootstrap **não** é linguagem de programação.  
> Bootstrap 5 = versão cobrada no edital.

### 1.9 Bootstrap 5 × Bootstrap 4 (armadilha FCC)

| Tema | BS4 (legado em prova) | BS5 (edital) |
|------|------------------------|--------------|
| jQuery | Dependência clássica de plugins | **Removida** — JS próprio / vanilla |
| Ícones | Glyphicons (histórico) | Não traz Glyphicons embutidos como no passado |
| Grid / utilities | Já responsivo | Evolução; classes e APIs atualizadas |
| Formulários / reboot | … | Ajustes de classes (reconhecer “mudou na v5”) |

Flash de ouro:

> Edital cita **Bootstrap 5**. Distrator comum: exigir **jQuery** como dependência obrigatória do BS5 — na linha oficial da v5, **jQuery não é requisito**.

Não invente lista completa de classes renomeadas; em prova, foque: **grid 12**, **responsivo**, **componentes**, **sem jQuery obrigatório na v5**.

### 1.10 HTML × CSS × Bootstrap × JS (mapa anti-confusão)

| Camada | Responsabilidade |
|--------|------------------|
| **HTML** | Estrutura e semântica |
| **CSS** | Estilo e layout “cru” |
| **Bootstrap** | Biblioteca/framework de estilo/componentes sobre HTML/CSS (+ JS de UI) |
| **JavaScript** | Comportamento dinâmico (arquivo 06) |

Armadilhas típicas:

1. Dizer que `<div class="row">` é “comando JavaScript”.  
2. Dizer que CSS “executa” no servidor como Java.  
3. Dizer que Bootstrap “compila TypeScript”.  
4. Trocar `padding` por `margin` no box model.  
5. Atribuir tag semântica a papel de folha de estilo.

### 1.11 O que a FCC tipicamente testa neste tema

1. Papel de **HTML5 semântico** vs `div` genérica.  
2. Tipos de `input` e formulários.  
3. **Box model** e diferença margin/padding.  
4. Flex vs Grid em alto nível.  
5. Bootstrap: **grid**, breakpoints, componentes.  
6. BS5 **sem jQuery obrigatório**.  
7. Separar marcação × estilo × script.

### 1.12 Mini-síntese (cartão mental)

> **HTML5** = estrutura/semântica (+ mídia/APIs).  
> **CSS3** = apresentação (caixa, seletores, flex/grid, media queries).  
> **Bootstrap 5** = framework de UI responsiva (grid 12, utilities, components); **≠** linguagem; **≠** jQuery obrigatório.  
> Nunca misture as três camadas no distrator.

---

## 2. Questões de treino (1–15)

**1.** HTML5, no eixo de desenvolvimento web do edital, descreve prioritariamente:

A) A linguagem de marcação da estrutura/semântica do documento  
B) O protocolo OSPF  
C) O servidor Kestrel exclusivamente  
D) O dialeto PL/SQL  
E) A norma ISO 27001  

**2.** A tag `nav`, em HTML5 semântico, indica tipicamente:

A) Folha de estilo CSS obrigatória  
B) Seção de navegação  
C) Servidor DHCP  
D) Registro DNS  
E) Cipher TLS  

**3.** CSS3 é melhor caracterizado como:

A) Linguagem de marcação estrutural substituta do HTML  
B) Mecanismo de folhas de estilo para apresentação/layout  
C) Banco relacional  
D) Orquestrador Kubernetes  
E) Framework Scrum  

**4.** No box model CSS, a ordem correta de dentro para fora é:

A) margin → border → padding → content  
B) content → padding → border → margin  
C) border → content → margin → padding  
D) padding → margin → content → border  
E) content → margin → padding → border (sem border)  

**5.** Assinale a associação **correta**:

A) `margin` — espaço interno entre content e border  
B) `padding` — espaço interno entre content e border  
C) `padding` — espaço externo além da border  
D) Bootstrap 5 — linguagem de programação tipada  
E) HTML — único responsável por media queries  

**6.** Flexbox, na visão cobrável, destaca-se por:

A) Layout predominantemente unidimensional (eixo principal)  
B) Ser o nome do IIS  
C) Substituir TCP  
D) Ser exclusivo do PostgreSQL  
E) Abolir o HTML  

**7.** CSS Grid, em contraste com Flexbox, é tipicamente associado a:

A) Apenas animações de cor  
B) Layout bidimensional (linhas e colunas)  
C) Único protocolo SMTP  
D) Service Discovery  
E) Flyway migrate  

**8.** Bootstrap 5, corretamente, é:

A) Um SGBD  
B) Um framework front-end de CSS/componentes para UI responsiva  
C) Um hypervisor  
D) Um protocolo de e-mail  
E) Um padrão COBIT exclusivo  

**9.** Sobre Bootstrap 5 e jQuery, a leitura alinhada à linha oficial da v5 é:

A) jQuery é dependência obrigatória insubstituível de todo componente BS5  
B) A v5 removeu a dependência de jQuery (JS próprio / abordagem moderna)  
C) Bootstrap 5 só funciona dentro do Oracle Forms  
D) Bootstrap 5 é um dialeto SQL  
E) Bootstrap 5 exige Flash Player  

**10.** Qual afirmação é **incorreta**?

A) HTML estrutura o documento  
B) CSS estiliza a apresentação  
C) Bootstrap oferece grid e componentes sobre HTML/CSS  
D) HTML5 é sinônimo estrito de JavaScript  
E) Media queries ajudam no design responsivo  

**11.** O sistema de grid do Bootstrap, em prova, costuma ser descrito como:

A) Baseado em colunas (clássico 12) com breakpoints responsivos  
B) Exclusivo de cabos UTP cat. 8  
C) Protocolo BGP  
D) Ferramenta de backup em fita  
E) Único modelo ER  

**12.** Elementos como `header`, `main`, `article` e `footer` ilustram:

A) Semântica HTML5  
B) Apenas propriedades CSS `display`  
C) Classes obrigatórias Bootstrap 3  
D) Anotações Java `@Entity`  
E) Eventos ITIL  

**13.** `input type="email"` no HTML5:

A) É um tipo de campo de formulário com semântica/validação de cliente típica de e-mail  
B) Compila TypeScript  
C) Cria uma VLAN  
D) Substitui o DNS root  
E) É exclusivo do COBOL  

**14.** Distrator clássico neste subtópico é:

A) Separar HTML (estrutura), CSS (estilo) e JS (comportamento)  
B) Associar BS5 a UI responsiva  
C) Tratar Bootstrap como linguagem de programação ou exigir jQuery na v5  
D) Lembrar box model content→padding→border→margin  
E) Reconhecer tags semânticas  

**15.** Qual linha resume melhor o cartão mental deste arquivo?

A) HTML = estilo; CSS = estrutura; Bootstrap = protocolo IP  
B) HTML5 estrutura/semântica; CSS3 apresenta; Bootstrap 5 UI responsiva (grid/components); camadas distintas  
C) Bootstrap 5 = jQuery obrigatório = Web Forms  
D) CSS Grid aboliu o HTML5  
E) Media queries existem só no ASP.NET Core  

---

### Gabarito comentado (treino 1–15)

1. **A** — Marcação estrutural/semântica.  
2. **B** — Navegação.  
3. **B** — Apresentação/layout.  
4. **B** — content → padding → border → margin.  
5. **B** — padding = interno.  
6. **A** — Flex ~ 1D.  
7. **B** — Grid ~ 2D.  
8. **B** — Framework front-end UI.  
9. **B** — BS5 sem jQuery obrigatório.  
10. **D** — HTML ≠ JS.  
11. **A** — Grid 12 + breakpoints.  
12. **A** — Semântica HTML5.  
13. **A** — Tipo de input.  
14. **C** — Linguagem/jQuery = armadilha.  
15. **B** — Síntese do arquivo 05.

---

## 3. Questões de concurso (16–25)

**16.** (FCC – TRT-5/2022 – Técnico Judiciário – TI) Considere o contêiner Bootstrap 5. Para definir que este contêiner será responsivo, de largura fixa e centralizado, utiliza-se a classe

A) `align-center`
B) `flex-box`
C) `container`
D) `fixed-box`
E) `container-fluid`

**17.** (FCC – TRT-6/2025 – Analista Judiciário – TI) Para aplicar fundo preto com 30% de transparência em um `div`, a instrução CSS correta é:

A) `transparency: 0.3;`
B) `background-color: filter(#000000, 30%);`
C) `background-color: rgba(0, 0, 0, 0.3);`
D) `filter: transparency(70%);`
E) `background-color: hsla(100, 100%, 100%, 0.3);`

**18.** (FCC – TRT-20/2024 – Analista Judiciário – TI) A media query `@media screen and (max-width: 480px) { ... }`

A) será executada em dispositivos cuja tela seja maior que 480 pixels.
B) será chamada por meio de `@import query('@media')`.
C) será executada apenas em telas com largura exatamente igual a 480 pixels.
D) não será executada, pois a sintaxe está incorreta (falta `and`).
E) será executada apenas em dispositivos cuja largura da tela seja menor ou igual a 480 pixels.

**19.** (FCC – TJ-CE/2022 – Analista Judiciário – Sistemas da Informação) Para colocar a cor de fundo vermelha apenas dos inputs do tipo text:

A) `input[type=text] {background-color:#00ff00}`
B) `input.type[text] {background-color:#ff0000}`
C) `input[type=text] {background-color:#ff0000}`
D) `input.type[text] {background: rgb(0,255,0)}}`
E) `input[type('text')] {background-color:rgb(255,0,0)}`

**20.** (FCC – TRT-20/2024 – Analista Judiciário – TI) A linha `lks.classList.toggle("visible");`

A) faz com que o elemento `links` tenha a classe `visible` adicionada ou removida a cada chamada.
B) torna visível a lista de classes CSS do elemento.
C) não será executada por erro em `getElementById`.
D) aplica o valor `visible` tornando o elemento visível.
E) gerará erro, pois `toggle` não se aplica a `class`.

**21.** (FCC – TRT-6/2025 – Analista Judiciário – TI) Para buscar o primeiro elemento com id `imagens`:

A) `document.querySelector('#imagens');`
B) `document.getElementFromId('imagens');`
C) `document.querySelector('.imagens');`
D) `document.getElementbyId('#imagens');`
E) `document.getIdentification('#imagens');`

**22.** (FCC – TRT-11/2017 – Técnico Judiciário – TI)  
No corpo de uma página HTML há `<p id="t1">TRT</p>` e `<p class="t2">11ª Região</p>`. Para definir a cor vermelha (`#ff0000`) desses parágrafos em CSS:

A) `.t1, #t2{color:#ff0000}`  
B) `p#t1, p.t2{color:#0000ff}`  
C) `p.t1, p#t2{font-color:#ff0000}`  
D) `p{font-color:#ff0000}`  
E) `#t1, .t2{color:#ff0000}`

**23.** (FCC – TRT-5/2022 – Técnico Judiciário – TI)  
jQuery: ao clicar no botão `#ocultar`, ocultar todos os `p`. A lacuna correta é

A) `$("p").display(none)`  
B) `$("p").hide()`  
C) `$("p").visible(false)`  
D) `$("p").show(false)`  
E) `$("p").setVisible(false)`

**24.** (FGV – DPE-RS/2023 – Técnico Programador)  
Bootstrap: 4 colunas em telas médias e 2 linhas × 2 colunas em telas menores. A configuração correta usa

A) `col-md-3 col-6` em cada item dentro de `row`  
B) classes `row-lg-4` / `row-sm-2`  
C) `col-md-4 col-2`  
D) `col` envolvendo `row` (hierarquia invertida)  
E) `row-3 row-sm-6`

**25.** (FCC – TRT-5/2022 – Técnico Judiciário – TI)  
(Bootstrap 5) A classe de contêiner de **largura fluida** (100% da viewport), em contraste com `container`, é

A) `align-center`  
B) `flex-box`  
C) `container`  
D) `fixed-box`  
E) `container-fluid`

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **C** | FCC TRT-5/2022 — Bootstrap `.container` |
| 17 | **C** | FCC TRT-6/2025 — `rgba(0,0,0,0.3)` |
| 18 | **E** | FCC TRT-20/2024 — `max-width: 480px` |
| 19 | **C** | FCC TJ-CE/2022 — `input[type=text]` |
| 20 | **A** | FCC TRT-20/2024 — `classList.toggle` (DOM/CSS) |
| 21 | **A** | FCC TRT-6/2025 — `querySelector` (DOM) |
| 22 | **E** | FCC TRT-11/2017 — `#id` e `.class` |
| 23 | **B** | FCC TRT-5/2022 — jQuery `hide` |
| 24 | **A** | FGV DPE-RS/2023 — grid Bootstrap |
| 25 | **E** | FCC TRT-5/2022 — `container-fluid` (contraste da 16) |

**Notas:** 16–19 e 22–25 HTML/CSS/Bootstrap; 20–21 DOM (mesmo eixo front). 24 FGV por escassez de grid Bootstrap com texto completo FCC.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **estrutura/tag/semântica**? → **HTML**.  
2. É **cor, caixa, flex, grid, media query**? → **CSS**.  
3. É **classe `row`/`col`/`navbar`, grid 12**? → **Bootstrap**.  
4. BS5 exige jQuery? → **Não** (linha v5).  
5. Bootstrap é linguagem de programação? → **Não**.

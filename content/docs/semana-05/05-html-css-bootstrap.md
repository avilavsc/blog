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

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto de desenvolvimento web do edital, HTML5, CSS3 e Bootstrap 5 relacionam-se, em regra, da seguinte forma:

A) HTML estrutura o conteúdo; CSS define a apresentação; Bootstrap oferece componentes/grid responsivos sobre essa base  
B) os três são protocolos de roteamento intercambiáveis  
C) Bootstrap substitui integralmente o TCP  
D) CSS é o único nome correto para tags `article`  
E) HTML5 é o runtime do .NET  

**17.** Considere o uso de elementos `header`, `nav`, `main` e `footer` em uma página. A interpretação mais adequada é:

A) aplicação de semântica HTML5 para regiões do documento  
B) declaração exclusiva de variáveis Java  
C) configuração de switch camada 3  
D) criação de stored procedure  
E) definição de sprint backlog  

**18.** Assinale a alternativa que apresenta associação **incorreta**:

A) padding — espaço interno entre conteúdo e borda  
B) margin — espaço externo além da borda  
C) Flexbox — layout com ênfase unidimensional  
D) CSS Grid — layout bidimensional  
E) Bootstrap 5 — linguagem de programação equivalente a Java  

**19.** Em enunciado que destaque “doze colunas”, “breakpoints `md`/`lg`” e “navbar/modal”, a associação mais adequada é:

A) Bootstrap (visão de framework CSS de UI)  
B) Somente SNMP  
C) Único significado de COBIT EDM  
D) Cabo coaxial 10BASE5  
E) PMBOK como folha de estilo  

**20.** Relativamente ao Bootstrap 5, é correto afirmar que:

A) a versão 5 mantém jQuery como dependência obrigatória de todos os plugins, segundo a linha clássica da v4  
B) a v5 removeu a dependência de jQuery, adotando abordagem de JavaScript própria/moderna  
C) Bootstrap 5 é um SGBD  
D) Bootstrap 5 substitui o HTML por binário  
E) Bootstrap 5 existe apenas no mainframe  

**21.** O modelo de caixa (box model) do CSS é relevante em provas porque:

A) a banca costuma cobrar a distinção entre content, padding, border e margin  
B) não há diferença entre margin e padding  
C) box model é exclusivo do protocolo FTP  
D) box model aboliu o HTML  
E) box model é sinônimo de Kanban  

**22.** Em design responsivo, media queries CSS servem prioritariamente para:

A) aplicar estilos condicionados a características do viewport/dispositivo  
B) autenticar usuários no Active Directory  
C) versionar schema Flyway  
D) descobrir serviços Eureka  
E) assinar XML com certificado de hardware  

**23.** Qual enunciado descreve **incorretamente** o HTML5?

A) Permite elementos semânticos de estrutura  
B) Oferece tipos de input modernos em formulários  
C) Possui elementos nativos de áudio e vídeo  
D) É, por definição, o motor de layout Flexbox do CSS  
E) Não se confunde com folha de estilo  

**24.** Em questão que contraste papéis, a FCC tipicamente considera **correto** que:

A) CSS controla apresentação; HTML marca estrutura; scripts tratam comportamento  
B) CSS é a única linguagem Turing-completa do browser e substitui o HTML  
C) Bootstrap elimina a necessidade de qualquer marcação HTML  
D) tags semânticas são propriedades `color` do CSS  
E) grid Bootstrap é protocolo IPv6  

**25.** A síntese mais alinhada ao edital é:

A) HTML5 estrutura; CSS3 apresenta (caixa/flex/grid/responsivo); Bootstrap 5 acelera UI responsiva — camadas distintas, BS5 sem jQuery obrigatório  
B) HTML = CSS = Bootstrap = JavaScript (quatro nomes, um papel)  
C) Bootstrap 5 exige Flash e jQuery juntos sempre  
D) CSS Grid é o nome do Kestrel  
E) A distinção das camadas web foi abolida nas bancas  

---

### Gabarito comentado (concurso 16–25)

16. **A** — Estrutura × estilo × framework UI.  
17. **A** — Semântica HTML5.  
18. **E** — Bootstrap ≠ linguagem tipo Java.  
19. **A** — Grid/breakpoints/componentes = Bootstrap.  
20. **B** — BS5 sem jQuery obrigatório.  
21. **A** — Content/padding/border/margin.  
22. **A** — Estilos por viewport.  
23. **D** — Flexbox é CSS, não “o HTML5”.  
24. **A** — Separação clássica das camadas.  
25. **A** — Cartão mental completo do arquivo 05.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **estrutura/tag/semântica**? → **HTML**.  
2. É **cor, caixa, flex, grid, media query**? → **CSS**.  
3. É **classe `row`/`col`/`navbar`, grid 12**? → **Bootstrap**.  
4. BS5 exige jQuery? → **Não** (linha v5).  
5. Bootstrap é linguagem de programação? → **Não**.

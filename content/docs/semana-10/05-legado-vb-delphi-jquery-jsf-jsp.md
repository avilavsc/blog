+++
title = '05 — Legado controlado · VB · Delphi · jQuery · JSF · JSP'
date = '2026-10-30'
draft = false
weight = 5
tags = ['semana-10', 'legado']
categories = ['estudo']
+++
# Legado controlado · VB · Delphi · jQuery · JSF · JSP · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 05 — Mapa único cauda longa (VB, Delphi, jQuery, JSF, JSP)  
**Escopo deste arquivo:** Conceitos **cobráveis** e contrastes (o que é / o que não é / onde roda). Nível “mapa mental”, não tutorial longo nem curso de cada IDE.  
**Fora de escopo aqui:** Spring/PDPJ profundo; React/Angular/Vue além do contraste com jQuery; Selenium/Maven (arquivo 06); inglês técnico (arquivo 04); ASP.NET Core detalhado (semana 5).

---

## 1. Material

### 1.1 Por que “cauda longa” no cronograma

O edital lista **Visual Basic**, **Delphi**, **jQuery**, **JSP**, **JSF** junto de Java/C#/Python/web moderno. Em prova FCC recente o peso é **reconhecer sintaxe/papel**, não construir sistema RAD do zero. Meta: **não zerar** a questão de legado.

Ordem mental:

```
Desktop legado:     VB / Delphi
Web servidor Java:  JSP  →  (evolui)  →  JSF (componentes/MVC)
Web cliente JS:     jQuery (lib DOM/Ajax)  ≠  framework SPA moderno
```

### 1.2 Visual Basic (VB) — cartão seco

| Ponto | Leitura FCC |
|-------|-------------|
| Origem | Família Microsoft / BASIC estruturado; clássico em apps Windows / VBA / VB.NET |
| Tipagem | Em VB.NET: tipagem + integração CLR/.NET (visão) |
| Função × Sub | **Function** retorna valor; **Sub** não retorna (procedimento) |
| Sintaxe típica | `Function Nome(x As Double) As Double` … `Return …` … `End Function` |
| Não confundir | Com Java (`public double`), PHP (`function … $x`), nem com `Sub` quando o enunciado pede retorno |

Flash TRT-1/2025: trecho Python `return salario * 0.08` → VB equivalente usa **Function** + **Return** + mesma conta (não dividir por 0.08; não virar `Sub`).

Anti-padrão: marcar alternativa em **Java** ou **PHP** quando o enunciado pede Visual Basic.

### 1.3 Delphi — cartão seco

| Ponto | Leitura FCC |
|-------|-------------|
| Base | Linguagem **Object Pascal**; RAD para **desktop** (histórico Borland/Embarcadero) |
| Compilação | **Compilada** (não “só interpretada web”) |
| Plataforma | Desktop clássico; linhagens puderam mirar **.NET** (enunciados antigos FCC cobram isso) |
| Contraste PHP | PHP web tipicamente **não** exige compilação pré-*deployment* como o item falso clássico |
| Contraste Java | Java → *bytecode*/JVM; dizer “só desktop Linux open-source” é distrator |

Afirmação cobrável (TRE-MS/FCC 2007, item II): Delphi é linguagem **compilada**, baseada em **Pascal**, desktop, podendo gerar apps no **.NET Framework**.

### 1.4 jQuery — cartão seco

| Ponto | Leitura FCC |
|-------|-------------|
| O que é | **Biblioteca** JavaScript *client-side* (não é “framework Java”) |
| Papel | Seletores CSS-like, manipular **DOM**/CSS, eventos, **Ajax** sem reload completo |
| Alias | Objeto jQuery; uso clássico via **`$`** |
| Ajax curto | `$.get(url, function(data, status){ … })` — callback com data + status |
| Ocultar | `$("p").hide()` (clássico FCC) |
| Não é | Linguagem Java; substituto de JSP/JSF no servidor; obrigatório no Bootstrap 5 |

Afirmação **INCORRETA** típica: “jQuery é framework client-side desenvolvido em **Java**”.

Contraste rápido:

| | jQuery | React/Angular/Vue (visão) |
|--|--------|---------------------------|
| Época/foco | Lib DOM/Ajax | Componentes / SPA |
| Papel edital | Ainda listado | Frameworks modernos |
| Servidor Java | Não substitui JSP/JSF | Consome APIs |

### 1.5 JSP (*JavaServer Pages*) — cartão seco

| Ponto | Leitura FCC |
|-------|-------------|
| O que é | Páginas no **servidor** que misturam HTML + Java / EL / tags; compiladas para **servlet** |
| Onde roda | Contêiner web (Tomcat etc.) — **não** na JVM do browser |
| Encaminhar dados | Servlet: `request.setAttribute("deps", lista);` + `RequestDispatcher.forward` |
| Receber na JSP | Cast: `(List<...>) request.getAttribute("deps")` — **não** `getParameter` para objeto setado |
| Expressão | `<%= resultado.getString("nome") %>` imprime no HTML; `<% … %>` só scriptlet (sem echo automático) |
| Tomcat | Implementa Servlet + JSP; pode atuar como HTTP server (visão FCC antiga) |

Armadilhas:

1. `getParameter` × `getAttribute`  
2. Falta de **cast** após `getAttribute`  
3. `<% expr %>` sem `=` quando precisa **escrever** no output  

### 1.6 JSF (*JavaServer Faces*) — cartão seco

| Ponto | Leitura FCC |
|-------|-------------|
| O que é | Framework **component-based** de UI Java EE/Jakarta; alinhado a **MVC** |
| Onde executa | Quase tudo no **servidor** (não “quase só no cliente”) |
| Config clássica | **`faces-config.xml`** — navegação, beans (legado), etc. |
| Ciclo de vida | Restore View → Apply Request Values → Process Validations → Update Model → Invoke Application → Render Response (visão) |
| Facelets | Tecnologia de view moderna do JSF (XHTML) — não “regras de negócio” |
| Ajax | Há suporte (libs/`f:ajax`) — distrator “JSF não faz Ajax” |
| × JSP | JSP = páginas/scriptlets; JSF = componentes + managed/backing beans + navegação |

Afirmação **INCORRETA** clássica (JSF 1.2): “a execução da aplicação se dá quase que totalmente no lado **cliente**”.

Servidor de aplicação (visão Java EE): tipicamente **contêiner web** + **contêiner EJB**. Tomcat = contêiner web (Servlet/JSP), **não** implementação de referência completa de servidor Java EE com EJB/JMS/etc.

### 1.7 Mapa único de contrastes (ouro)

| Tecnologia | Camada | Linguagem / stack | Arquivo / marca mental | Não é |
|------------|--------|-------------------|------------------------|-------|
| **VB** | Desktop / .NET | BASIC/VB.NET | `Function`/`Sub`/`End Function` | Java/PHP |
| **Delphi** | Desktop RAD | Object Pascal compilado | Pascal + .NET (visão) | PHP “compilado obrigatório” |
| **jQuery** | Cliente browser | JavaScript lib | `$`, `hide`, `$.get` | Framework Java |
| **JSP** | Servidor web | Java → servlet | `getAttribute` + cast; `<%= %>` | Código no browser |
| **JSF** | Servidor UI MVC | Java EE Faces | `faces-config.xml`; server-side | App “só cliente” |

Perguntas mentais:

1. Roda no **browser** ou no **servidor**?  
2. É **lib JS**, **página Java**, ou **framework de componentes**?  
3. O enunciado pediu **VB** e a alternativa está em outra linguagem?

### 1.8 Encadeamento Servlet → JSP (fluxo cobrável)

```
DAO busca lista
    → request.setAttribute("deps", lista)
    → RequestDispatcher("listarDeps.jsp").forward(...)
    → na JSP: (List<Departamento>) request.getAttribute("deps")
```

JSF (visão): FacesServlet + componentes + beans; navegação/`faces-config` (clássico) em vez de “só scriptlet JSP”.

### 1.9 O que cortar na revisão

Não decore IDE Delphi linha a linha, todos os eventos VB6, todos os plugins jQuery, nem as 6 fases JSF com código. Decore **papéis + 2–3 sintaxes** (VB Function, jQuery hide/get, JSP attribute/cast/expression, JSF server-side + faces-config).

---

## 2. Questões de treino (1–15)

**1.** Em Visual Basic, para calcular e **devolver** um valor numérico, a forma alinhada ao padrão cobrado é:

A) Apenas `Sub` sem retorno, alterando parâmetro sem `Return`  
B) `Function … As Double` com `Return` do resultado  
C) Obrigatoriamente sintaxe PHP com `$`  
D) Classe Java `public double` dentro de arquivo `.vb`  
E) Somente anotação `@Test` JUnit  

**2.** Delphi, em enunciados FCC clássicos, é corretamente associado a:

A) Linguagem interpretada exclusiva de browser sem compilação  
B) Linguagem compilada baseada em Pascal, típica de desktop, com linhagens capazes de gerar apps .NET  
C) Sinônimo de jQuery  
D) Contêiner EJB do Tomcat  
E) Protocolo TLS  

**3.** jQuery é, em essência:

A) Framework server-side escrito em Java  
B) Biblioteca JavaScript client-side para DOM/Ajax/CSS  
C) Substituta de Maven  
D) Dialeto de Delphi  
E) Contêiner EJB  

**4.** A afirmação **incorreta** sobre jQuery é:

A) Manipula a árvore DOM  
B) Usa frequentemente o alias `$`  
C) Permite Ajax sem recarregar a página inteira  
D) É framework client-side desenvolvido na linguagem **Java**  
E) Pode alterar CSS em runtime  

**5.** `$("p").hide()` em jQuery:

A) Compila Pascal  
B) Oculta elementos `p` selecionados  
C) Cria managed bean JSF  
D) Gera `pom.xml`  
E) Abre conexão JDBC  

**6.** JSP executa-se tipicamente:

A) Na JVM embutida no navegador do usuário  
B) No contêiner web do servidor (virando servlet)  
C) Apenas como arquivo `.css`  
D) Somente no Active Directory  
E) Como driver JDBC no cliente  

**7.** Após `request.setAttribute("deps", lista)` e `forward` para JSP, a forma correta de recuperar a lista tipada é:

A) `request.getParameter("deps")` sem cast  
B) `(List<Departamento>) request.getAttribute("deps")`  
C) `request.getAttribute("deps").toList<Departamento>()`  
D) `List<Departamento>() request.getAttribute("deps")`  
E) Ignorar o atributo e ler cookie obrigatoriamente  

**8.** Em JSP, para **imprimir** no HTML o nome vindo de `ResultSet resultado`, a lacuna em `value="I"` deve ser:

A) `<% resultado.getString("nome") %>`  
B) `<%= resultado.getString("nome") %>`  
C) `<%= resultado.getValue("nome") %>`  
D) `<% resultado.getValue("nome") %>`  
E) `<%= resultado.StringOf("nome") %>`  

**9.** Sobre JSF 1.x/clássico, é **INCORRETO** afirmar que:

A) Pode integrar CSS via IDE  
B) Tem componentes de UI com validação  
C) A execução da aplicação se dá quase que totalmente no lado **cliente**  
D) Inclui modelo de eventos no servidor  
E) Permite inserção de JavaScript via IDE  

**10.** Em aplicação JSF, o arquivo clássico para configurar fluxo/navegação (servlet de controle Faces) é:

A) `webfaces.xml`  
B) `actionform.xml`  
C) `faces-config.xml`  
D) `webcontext.xml`  
E) `serverconfig.xml`  

**11.** Em Java EE, um servidor de aplicação padrão normalmente contém:

A) Apenas DNS  
B) Contêiner web e contêiner EJB  
C) Somente Excel  
D) Apenas jQuery minificado  
E) Somente WSUS  

**12.** Tomcat, na visão cobrável frente a Java EE completo:

A) É a implementação de referência completa com EJB/JMS/JPA/JSF todos nativos como AS full  
B) Implementa Servlet/JSP (contêiner web) e pode servir HTTP; não substitui sozinho todo AS Java EE  
C) Compila Delphi  
D) É sinônimo de Selenium Grid  
E) Armazena apenas `faces-config` do Active Directory  

**13.** Contraste JSP × JSF mais adequado:

A) Ambos rodam só no cliente  
B) JSP = páginas/scriptlets no servidor; JSF = framework de componentes/MVC no servidor  
C) JSF substitui TCP  
D) JSP é biblioteca `$`  
E) São protocolos QoS  

**14.** Distrator clássico nesta cauda longa:

A) Separar VB Function de Sub  
B) Tratar jQuery como Java server-side ou JSF como “quase só cliente”  
C) Lembrar `getAttribute` + cast  
D) Associar Delphi a Pascal compilado  
E) Usar `faces-config.xml` para navegação JSF  

**15.** Qual linha resume o mapa deste arquivo?

A) VB/Delphi = desktop legado; jQuery = lib JS cliente; JSP/JSF = servidor Java (páginas × componentes)  
B) Tudo é SaaS NIST  
C) Delphi = jQuery = Maven  
D) JSP roda só no IE  
E) VB exige Kubernetes  

---

### Gabarito comentado (treino 1–15)

1. **B** — Function + Return.  
2. **B** — Pascal compilado / desktop / .NET.  
3. **B** — Lib JS cliente.  
4. **D** — Não é Java.  
5. **B** — hide.  
6. **B** — Servidor/contêiner web.  
7. **B** — getAttribute + cast.  
8. **B** — `<%= getString %>`.  
9. **C** — Server-side (incorreto dizer “quase só cliente”).  
10. **C** — faces-config.xml.  
11. **B** — Web + EJB containers.  
12. **B** — Servlet/JSP; não AS full sozinho.  
13. **B** — Páginas × componentes MVC.  
14. **B** — Armadilhas de camada/linguagem.  
15. **A** — Síntese do mapa.

---

## 3. Questões de concurso (reais) (16–25)

> Questões **reais** FCC (preferência Analista TI / afins), com banca/órgão/ano. Sem invenção.

**16.** (FCC – TRT-1/2025 – Analista Judiciário – TI) Considere o seguinte trecho de código em Python, que calcula o valor do FGTS com base no salário do trabalhador. O FGTS é calculado como 8% do salário.

```python
def calcular_fgts(salario):
    return salario * 0.08
```

Em condições ideais, o código equivalente em Visual Basic, que mantém a mesma lógica e cálculo, é:

A)
```vb
Function CalcularFGTS(salario As Double) As Double
    Return salario * 0.08
End Function
```
B)
```java
public double calcularFGTS(double salario) {
    return salario * 0.08;
}
```
C)
```vb
function FGTS(salario As Double) As Double
    return salario / 0.08
end function
```
D)
```vb
Sub CalcularFGTS(salario As Double)
    salario = salario * 0.08
End Sub
```
E) `function calcularFGTS($salario) { return $salario * 8; }`

**17.** (FCC – TRE-MS/2007 – Analista Judiciário – Analista de Sistemas) Considere as afirmativas abaixo sobre as linguagens de programação.  
I. PHP é uma linguagem direcionada para construção de websites cujo código-fonte deve ser compilado antes de ser realizado o deployment no servidor.  
II. Delphi é uma linguagem compilada, baseada em Pascal, direcionada ao desenvolvimento de aplicações de desktop e que pode gerar aplicações baseadas no .NET Framework.  
III. Java é uma linguagem interpretada baseada em bytecodes, é open-source, e é direcionada para a criação de aplicações de desktop em ambientes Linux.  
É correto o que se afirma APENAS em

A) I.  
B) II.  
C) III.  
D) I e II.  
E) I e III.  

**18.** (FCC – TRT-23/2011 – Técnico Judiciário – TI) Em relação ao JSF 1.2 é INCORRETO afirmar que

A) permite a inserção, via IDE, de folhas de estilo CSS.  
B) inclui um conjunto padrão de componentes de interface de usuário que possibilitam validação padronizada.  
C) a execução da aplicação se da quase que totalmente no lado cliente.  
D) inclui um modelo de eventos do lado servidor.  
E) permite a inserção, via IDE, de comandos em JavaScript.  

**19.** (FCC – TJ-PE/2012 – Técnico Judiciário – Programador) Em uma aplicação que utiliza JSF, para configurar o fluxo de comunicação presente na servlet de controle, é utilizado um arquivo de configuração

A) webfaces.xml.  
B) actionform.xml.  
C) faces-config.xml.  
D) webcontext.xml.  
E) serverconfig.xml.  

**20.** (FCC – TRT-17/2022 – Analista Judiciário – TI) Em uma servlet Java de uma aplicação web em condições ideais, uma lista de objetos do tipo Departamento foi encapsulada e enviada para uma página JSP chamada `listarDeps.jsp`, por meio de:

```
List<Departamento> deps = dao.buscarDeps();
request.setAttribute("deps", deps);
disp = request.getRequestDispatcher("listarDeps.jsp");
disp.forward(request, response);
```

Na página `listarDeps.jsp`, após os recursos necessários serem importados, para receber essa lista utiliza-se a instrução

A) `<% List<Departamento> deps = request.getAttribute("deps"); %>`  
B) `<% List<Departamento> deps = request.getAttribute("deps").toList<Departamento>(); %>`  
C) `<% List<Departamento> deps = List<Departamento>() request.getAttribute("deps"); %>`  
D) `<% List<Departamento> deps = (List<Departamento>) request.getParameter("deps"); %>`  
E) `<% List<Departamento> deps = (List<Departamento>) request.getAttribute("deps"); %>`  

**21.** (FCC – TRT-19/2022 – Analista Judiciário – TI) Considere `resultado` uma variável do tipo `ResultSet` com campo `nome`. Em formulário JSP:

`<input type="text" id="nome" name="nome" value="I">`

A lacuna I deve ser preenchida por

A) `<% resultado.getValue("nome") %>`  
B) `<%= resultado.getValue("nome") %>`  
C) `<%= resultado.getString("nome") %>`  
D) `<%= resultado.StringOf("nome") %>`  
E) `<% resultado.getString("nome") %>`  

**22.** (FCC – TRE-PR/2017 – Analista Judiciário – Análise de Sistemas) Aplicações Java EE usam o modelo cliente-servidor. Nestas aplicações,

A) componentes EJB e JPA são executados no lado do cliente, assim como páginas web criadas com HTML e JSP.  
B) componentes JSP e Servlet são executados na JVM instalada no lado cliente e integrada ao navegador do usuário.  
C) componentes de implantação WAR contém arquivos JAR, EAR, XML, EJB, JSF, servlet e páginas web.  
D) um servidor de aplicação padrão normalmente contém um contêiner web e um contêiner EJB.  
E) o Tomcat é a implementação de referência para servidor de aplicação, suportando JSP, Servlet, EJB, JPA, JSF e JMS.  

**23.** (FCC – TRE-MS/2007 – Analista Judiciário – Analista de Sistemas) O software Apache Tomcat é um servidor de páginas dinâmicas web que

A) está disponível apenas para as plataformas Windows e Linux.  
B) implementa a API de Servlets e JavaServer Pages mas exige a integração com servidores http para prover páginas e recursos estáticos.  
C) apesar de gratuito, seu código fonte não é aberto (open-source).  
D) implementa a API de Servlets e JavaServer Pages e pode atuar diretamente como servidor http.  
E) implementa a API de Servlets, JavaServer Pages e Enterprise Java Beans e pode atuar diretamente como servidor http.  

**24.** (FCC – SANASA Campinas/2019 – Analista de TI – Análise e Desenvolvimento) Considere a página abaixo, desenvolvida utilizando-se jQuery:

```html
<script>
$(document).ready(function(){
  $("button").click(function(){
    I ("sanasa.php", function(data, status){
      alert("Retorno: " + data + "\nStatus: " + status);
    });
  });
});
</script>
```

Para que, ao clicar, seja feita a requisição a `sanasa.php` com retorno em `data` e status em `status`, a lacuna I deve ser corretamente preenchida por

A) `$.get`  
B) `$post`  
C) `$.ajax`  
D) `$.send`  
E) `$request`  

**25.** (FCC – TRT-5/2022 – Técnico Judiciário – TI) jQuery: ao clicar no botão `#ocultar`, ocultar todos os `p`. A lacuna correta é

A) `$("p").display(none)`  
B) `$("p").hide()`  
C) `$("p").visible(false)`  
D) `$("p").show(false)`  
E) `$("p").setVisible(false)`  

---

### Gabarito (concurso 16–25)

| Q | Gab. | Fonte / nota |
|---|------|----------------|
| 16 | **A** | FCC TRT-1/2025 — VB Function equivalente — Quizinho |
| 17 | **B** | FCC TRE-MS/2007 — apenas II (Delphi) — prova/PDF Estude Grátis |
| 18 | **C** | FCC TRT-23/2011 — JSF não é “quase só cliente” — CFP Concursos / Gabarite |
| 19 | **C** | FCC TJ-PE/2012 — `faces-config.xml` — QConcursos / Gabarite Q230695 |
| 20 | **E** | FCC TRT-17/2022 — cast + `getAttribute` — Quizinho |
| 21 | **C** | FCC TRT-19/2022 — `<%= getString %>` — Quizinho |
| 22 | **D** | FCC TRE-PR/2017 — contêiner web + EJB — CFP Concursos |
| 23 | **D** | FCC TRE-MS/2007 — Tomcat Servlet/JSP + HTTP — prova PDF |
| 24 | **A** | FCC SANASA/2019 — `$.get(url, fn(data,status))` — Questionei |
| 25 | **B** | FCC TRT-5/2022 — `$("p").hide()` — pack/semana 5 / Gabarite |

Letras: A, B, C, C, E, C, D, D, A, B.

---

### Verificação interna

| Checagem | Status |
|----------|--------|
| VB + Delphi + jQuery + JSF + JSP no mapa | 1.2–1.7 |
| Contrastes cobráveis (não tutorial) | 1.7–1.9 |
| Treino 1–15 | OK |
| 16–25 reais citados | 10/10 |
| Gabarito só no final | OK |

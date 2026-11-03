+++
title = '04 — ASP.NET Core'
date = '2026-10-18'
draft = false
weight = 4
tags = ['semana-05', 'aspnet', 'core']
categories = ['estudo']
+++
# ASP.NET Core · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 04 — ASP.NET Core (visão)  
**Escopo deste arquivo:** o que é ASP.NET Core no .NET moderno; pipeline de middleware; MVC / Razor Pages / Minimal APIs; DI e configuração; Kestrel; contraste cobrável com ASP.NET clássico e com Spring Boot (âncora do seu perfil); armadilhas FCC de sinônimo (.NET × ASP.NET × Core).  
**Fora de escopo aqui:** Java/C#/Python OOP em profundidade (arquivos 01–03); HTML/CSS/Bootstrap (05); JavaScript/TypeScript e frameworks JS (06); servidores IIS/JBoss em detalhe de redes (semana 7).

---

## 1. Material

### 1.1 Por que isso cai no edital

No bloco **Desenvolvimento web e mobile** do Anexo II aparecem explicitamente: **ASP.NET Core**, **.NET**, ao lado de HTML5, CSS3, Bootstrap, JavaScript, TypeScript e frameworks.

Leitura cobrável (semana 5 = **visão**, não monografia):

| Peça | Papel em prova |
|------|----------------|
| **.NET** | Plataforma/runtime + bibliotecas (CLR moderno / CoreCLR) |
| **ASP.NET Core** | Framework web **sobre** .NET (HTTP, MVC, APIs, Razor…) |
| **Kestrel** | Servidor web embutido típico do ASP.NET Core |
| **Middleware** | Pipeline de request/response (análogo mental a filtros/filtros Spring) |
| **DI** | Injeção de dependência **nativa** no host |

Flash: a FCC gosta de misturar **.NET Framework legado**, **ASP.NET clássico (Web Forms/MVC antigo)** e **ASP.NET Core** — saber o “dono” de cada ideia evita a pegadinha.

### 1.2 Cartão seco: .NET × ASP.NET × ASP.NET Core

| Nome | Pergunta mental | Resposta curta |
|------|-----------------|----------------|
| **.NET** | Em que plataforma o código **roda**? | Runtime + BCL + ferramentas |
| **ASP.NET (clássico)** | Web no **.NET Framework** (Windows/IIS clássico) | Modelo legado (Web Forms, MVC 5…) |
| **ASP.NET Core** | Web **moderno, multiplataforma**, redesenhado | Framework web atual sobre .NET |

Síntese em uma linha:

> **.NET** = plataforma. **ASP.NET Core** = como se faz **aplicação web/API** nessa plataforma moderna.  
> ASP.NET Core **não** é sinônimo de “só Windows” nem de Web Forms.

O que ASP.NET Core **não** é (ouro anti-distrator):

1. Não é o banco de dados (SQL Server/PostgreSQL).  
2. Não é o front-end (React/Angular) — pode **servir** páginas/APIs consumidas por eles.  
3. Não é o IIS em si (pode hospedar atrás do IIS/Nginx, mas o servidor de app típico é **Kestrel**).  
4. Não é Java / Spring Boot (âncora: **papel parecido**, stack diferente).  
5. Não é “apenas Razor” — cobre **MVC, APIs REST, Minimal APIs, Blazor** (reconhecer nomes).

### 1.3 ASP.NET Core — leitura de prova

**ASP.NET Core** é o framework web **open source e multiplataforma** (Windows, Linux, macOS) para construir:

- APIs REST / HTTP  
- Apps MVC com views  
- Razor Pages  
- (reconhecer) Blazor, gRPC, SignalR  

Características cobráveis:

| Ideia | Frase de prova |
|-------|----------------|
| **Multiplataforma** | Roda fora do “só Windows” do Framework clássico |
| **Alto desempenho** | Pipeline enxuto; Kestrel |
| **Modular** | Pacotes NuGet; compõe o que precisa |
| **DI nativa** | Container de DI no host (`Program` / `Startup` legado) |
| **Middleware** | Cadeia que processa cada request |
| **Hospedagem flexível** | Kestrel sozinho ou atrás de proxy (IIS, Nginx, Apache) |

Flash: enunciado fala em **middleware**, **Kestrel**, **multiplataforma**, **injeção de dependência built-in**, “substituto moderno do ASP.NET no .NET Framework” → **ASP.NET Core**.

### 1.4 Pipeline de middleware (âncora mental)

Modelo cobrável (não código de produção):

```
Request → [Exception] → [HTTPS] → [Auth] → [Routing] → [Endpoint/MVC] → Response
              ↑______________ ordem configurada no pipeline ______________↑
```

| Conceito | O que a FCC testa |
|----------|-------------------|
| **Middleware** | Componente no pipeline; pode short-circuit (terminar cedo) |
| **Ordem importa** | Auth antes de endpoint protegido; exception handling cedo |
| **Endpoint** | Destino final (controller action, Minimal API, Razor Page) |

Analogia útil (seu perfil Spring): pense em **filtro de servlet / FilterChain / interceptors** — a ideia de cadeia, não a API Java.

Armadilha: dizer que middleware = **apenas** autenticação. Middleware é **qualquer** peça da cadeia (static files, CORS, compressão, routing…).

### 1.5 Modelos de aplicação cobráveis

| Modelo | Quando o enunciado aponta |
|--------|---------------------------|
| **MVC** | Controllers + Models + Views (Razor); separação clássica |
| **Razor Pages** | Páginas com handler (PageModel); foco em UI page-centric |
| **Web API / Controllers API** | JSON, HTTP verbs, sem view HTML obrigatória |
| **Minimal APIs** | Rotas enxutas em `Program` (estilo funcional/moderno) |
| **Blazor** | UI com C# no browser (WASM) ou server — **reconhecer nome** |

Tabela anti-confusão:

| Afirmação | Veredito |
|-----------|----------|
| ASP.NET Core “é só MVC” | **Falso** — vários modelos |
| Minimal API elimina Controllers | **Modelo alternativo**, não “morte” do MVC em prova |
| Razor = JavaScript | **Falso** — engine de views **.NET** (HTML + C#) |
| Web Forms = ASP.NET Core | **Falso** — Web Forms é clássico/.NET Framework |

### 1.6 Kestrel, hospedagem e IIS

| Peça | Papel |
|------|-------|
| **Kestrel** | Servidor HTTP **embutido** padrão do ASP.NET Core |
| **IIS / Nginx / Apache** | Frequentemente **reverse proxy** na frente |
| **IIS in-process / out-of-process** | Modos de hospedagem no Windows (reconhecer que **existem**) |

Cartão:

> Na stack Core, **Kestrel** costuma ser o servidor da aplicação; **IIS** (quando aparece) é hospedeiro/proxy — **não** confunda “ASP.NET Core exige IIS” com a verdade multiplataforma.

Contraste com Spring Boot: **Kestrel ≈ Tomcat/Jetty embutido** (papel de servidor embutido), não o nome do framework.

### 1.7 Configuração e DI (visão)

| Tema | Leitura FCC |
|------|-------------|
| **appsettings.json** | Configuração tipicamente hierárquica (Dev/Prod) |
| **Ambiente** | `Development`, `Staging`, `Production` |
| **Options pattern** | Tipar config em classes (reconhecer ideia) |
| **DI** | `AddSingleton` / `AddScoped` / `AddTransient` — tempos de vida |
| **Scoped** | Por request HTTP (analogia pedida em prova) |

Armadilha: tratar DI do ASP.NET Core como “biblioteca externa obrigatória” — no Core, DI é **primeira classe** no host.

### 1.8 ASP.NET Core × ASP.NET clássico (ouro de prova)

| | **ASP.NET (Framework)** | **ASP.NET Core** |
|---|-------------------------|------------------|
| Plataforma | .NET Framework (legado) | .NET (Core / 5+) |
| SO | Historicamente Windows/IIS | Multiplataforma |
| Pipeline | System.Web / módulos IIS | Middleware moderno |
| Web Forms | Presente no clássico | **Não** é o modelo Core |
| Performance / modularidade | Mais “pesado” System.Web | Pipeline enxuto |

Flash: “redesenhado, multiplataforma, sem System.Web clássico” → **ASP.NET Core**.  
“Web Forms / .NET Framework acoplado ao IIS clássico” → **legado**, não Core.

### 1.9 Âncora Spring Boot (só para mapear lacuna)

| Ideia | Spring Boot (seu domínio) | ASP.NET Core (prova) |
|-------|---------------------------|----------------------|
| Framework web | Spring MVC / Web | ASP.NET Core MVC / Minimal APIs |
| Servidor embutido | Tomcat/Jetty/Undertow | **Kestrel** |
| DI | Spring IoC | DI nativa do host |
| Config | `application.yml` | `appsettings.json` |
| Pacotes | Maven/Gradle starters | NuGet |
| API JSON | `@RestController` | Controllers API / Minimal APIs |

Isso **não** cai como tabela Spring×.NET literal — serve para você **não** deixar lacuna de vocabulário .NET.

### 1.10 O que a FCC tipicamente testa neste tema

1. ASP.NET Core **≠** ASP.NET Web Forms / Framework clássico.  
2. **Multiplataforma** e **Kestrel**.  
3. Papel do **middleware** e ordem do pipeline.  
4. Modelos: **MVC**, **Razor Pages**, **Web API**, Minimal APIs (reconhecer).  
5. DI e configuração como pilares do host.  
6. Relação **.NET** (plataforma) × **ASP.NET Core** (web).  
7. Hospedagem: Kestrel ± reverse proxy (IIS/Nginx).

### 1.11 Mini-síntese (cartão mental)

> **.NET** = plataforma. **ASP.NET Core** = web/API moderna multiplataforma.  
> Pipeline = **middleware** → endpoint (MVC / API / Razor / Minimal).  
> Servidor embutido típico = **Kestrel**. DI e config são nativos.  
> **≠** Web Forms · **≠** “só IIS” · **≠** front-end JS.

---

## 2. Questões de treino (1–15)

**1.** No edital de Analista TI, ASP.NET Core situa-se prioritariamente no eixo de:

A) Cabos e VLANs de camada 2  
B) Desenvolvimento web / aplicações sobre a plataforma .NET moderna  
C) Único dialeto SQL do PostgreSQL  
D) Único nome correto para Kubernetes  
E) Norma ISO 27001 exclusiva  

**2.** A distinção mais cobrável entre **.NET** e **ASP.NET Core** é:

A) São sempre sinônimos absolutos na FCC  
B) .NET é a plataforma/runtime; ASP.NET Core é o framework web sobre essa plataforma  
C) ASP.NET Core é o kernel do Linux  
D) .NET é apenas o IIS; Core é apenas o CSS  
E) Ambos equivalem a React Native  

**3.** O servidor HTTP embutido típico associado ao ASP.NET Core é:

A) Kestrel  
B) Eureka  
C) Flyway  
D) Hibernate Envers  
E) Zabbix  

**4.** No pipeline do ASP.NET Core, middleware descreve melhor:

A) Apenas o driver JDBC  
B) Componentes encadeados que processam a requisição/resposta HTTP  
C) O único nome do banco H2  
D) O manifesto ágil  
E) O protocolo SMTP obrigatório  

**5.** Assinale a associação **correta**:

A) Web Forms clássico = modelo padrão exclusivo do ASP.NET Core  
B) ASP.NET Core é multiplataforma; Web Forms é típico do ASP.NET no .NET Framework  
C) Kestrel = Service Discovery Netflix  
D) Minimal APIs = dialeto PL/SQL  
E) appsettings.json = protocolo STP  

**6.** Qual afirmação sobre hospedagem é a mais alinhada à visão de prova?

A) ASP.NET Core só roda se o IIS for o único processo possível em qualquer SO  
B) Kestrel pode servir a aplicação; IIS/Nginx frequentemente atuam como reverse proxy  
C) ASP.NET Core proíbe qualquer proxy reverso  
D) Kestrel é o nome do registry Eureka  
E) Hospedagem Core exige obrigatoriamente Oracle Forms  

**7.** Em contraste com o ASP.NET clássico acoplado ao System.Web, o ASP.NET Core caracteriza-se por:

A) Pipeline moderno de middleware e desenho multiplataforma  
B) Obrigar Web Forms em todo projeto  
C) Eliminar HTTP  
D) Ser apenas um tema Bootstrap  
E) Substituir TCP por FTP em todas as APIs  

**8.** Controllers que expõem JSON via verbos HTTP, sem view HTML obrigatória, apontam tipicamente para:

A) Apenas Web Forms  
B) Modelo de Web API / API Controllers no ASP.NET Core  
C) Somente CSS Grid  
D) Único uso do SNMP  
E) Apenas Kanban  

**9.** A injeção de dependência no ASP.NET Core, na leitura cobrável:

A) É inexistente; toda dependência deve ser estática global  
B) É recurso de primeira classe do host (tempos de vida como Scoped por request)  
C) Existe apenas no Hibernate  
D) É sinônimo de VLAN 802.1Q  
E) Substitui o DNS  

**10.** Qual item **não** descreve corretamente o ASP.NET Core?

A) Framework web moderno sobre .NET  
B) Suporta MVC e APIs  
C) Usa tipicamente Kestrel como servidor embutido  
D) É o nome oficial do protocolo IPv6  
E) Emprega pipeline de middleware  

**11.** Razor, no ecossistema ASP.NET Core, refere-se preferencialmente a:

A) Engine de views / sintaxe de templates .NET (HTML + C#)  
B) Banco NoSQL obrigatório  
C) Ferramenta exclusiva de captura de pacotes Wireshark  
D) Padrão ITIL de mudança  
E) Cipher suite TLS exclusiva  

**12.** “Ordem do pipeline importa: autenticação antes do endpoint protegido.” Isso ilustra:

A) Irrelevância total do middleware  
B) Característica típica do encadeamento de middleware  
C) Que ASP.NET Core não tem HTTP  
D) Que Core = apenas COBOL  
E) Que Kestrel é um ORM  

**13.** Relativamente a Minimal APIs no ASP.NET Core:

A) São um modelo enxuto de expor endpoints HTTP, coexistindo no ecossistema com Controllers  
B) Eliminam por decreto qualquer noção de HTTP status  
C) São o nome antigo do Web Forms  
D) Existem apenas fora do .NET  
E) Substituem o PostgreSQL  

**14.** Distrator clássico neste subtópico é:

A) Separar plataforma (.NET) de framework web (ASP.NET Core)  
B) Associar Kestrel a servidor embutido  
C) Tratar ASP.NET Core como sinônimo de Web Forms / “só IIS Windows legado”  
D) Lembrar que middleware forma um pipeline  
E) Reconhecer MVC e Web API como modelos possíveis  

**15.** Qual linha resume melhor o cartão mental deste arquivo?

A) ASP.NET Core = Web Forms + System.Web obrigatório + só Windows  
B) ASP.NET Core = web/API multiplataforma no .NET; middleware + Kestrel; ≠ Web Forms  
C) ASP.NET Core = Eureka + Zuul  
D) ASP.NET Core = apenas CSS Bootstrap  
E) ASP.NET Core = único nome do PMBOK  

---

### Gabarito comentado (treino 1–15)

1. **B** — Eixo desenvolvimento web/.NET.  
2. **B** — Plataforma × framework web.  
3. **A** — Kestrel.  
4. **B** — Cadeia request/response.  
5. **B** — Core multiplataforma; Web Forms = clássico.  
6. **B** — Kestrel ± proxy.  
7. **A** — Middleware + multiplataforma.  
8. **B** — Web API / Controllers API.  
9. **B** — DI nativa; Scoped ~ request.  
10. **D** — Não é IPv6.  
11. **A** — Views/templates .NET.  
12. **B** — Ordem do middleware.  
13. **A** — Modelo enxuto coexistente.  
14. **C** — Sinônimo Web Forms/só IIS = armadilha.  
15. **B** — Síntese do arquivo 04.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto de desenvolvimento web referido no edital, ASP.NET Core corresponde, em regra, a:

A) framework para construção de aplicações web e APIs sobre a plataforma .NET moderna, com suporte multiplataforma  
B) protocolo de roteamento OSPF exclusivo  
C) sistema de arquivos do ZFS  
D) nome alternativo obrigatório do jQuery  
E) norma única de cabeamento estruturado  

**17.** Considere um enunciado que destaque “servidor HTTP embutido”, “pipeline de middleware” e “appsettings.json”. A associação mais adequada é:

A) ASP.NET Core  
B) Somente ITIL Problem Management  
C) Único significado de BPMN  
D) Cabo fibra OM4  
E) Resolução CNJ 400 como runtime  

**18.** Assinale a alternativa que apresenta associação **incorreta**:

A) Kestrel — servidor embutido típico do ASP.NET Core  
B) Middleware — componentes do pipeline HTTP  
C) .NET — plataforma/runtime  
D) ASP.NET Core — framework web moderno  
E) ASP.NET Core — sinônimo estrito de ASP.NET Web Forms no .NET Framework  

**19.** Em arquitetura com reverse proxy na frente da aplicação ASP.NET Core, é correto afirmar que:

A) a presença de IIS ou Nginx na frente impede conceptualmente o uso de Kestrel  
B) Kestrel pode permanecer como servidor da aplicação, com o proxy na borda  
C) ASP.NET Core exige abolir HTTP  
D) middleware deixa de existir sob proxy  
E) Razor torna-se protocolo de rede  

**20.** Diferenciar ASP.NET clássico (System.Web / Web Forms) de ASP.NET Core é relevante em provas porque:

A) a banca costuma explorar a confusão entre o modelo legado Windows/IIS e o framework redesenhado multiplataforma  
B) não há qualquer diferença conceitual cobrável  
C) Core é apenas o nome do Web Forms  
D) clássico e Core são o mesmo que Bootstrap 5  
E) a diferença só existe em redes Token Ring  

**21.** Relativamente à injeção de dependência no host ASP.NET Core, a leitura mais alinhada ao edital é:

A) trata-se de recurso integrado ao modelo de hospedagem/aplicação, com tempos de vida configuráveis  
B) é proibida em qualquer API REST  
C) equivale exclusivamente ao protocolo DHCP  
D) substitui o papel do DNS root  
E) é o nome do cabeçalho `Content-Type`  

**22.** Um endpoint que devolve JSON e é mapeado por verbo/rota HTTP, sem view Razor obrigatória, situa-se tipicamente no modelo de:

A) apenas Web Forms postback  
B) API / Minimal API ou controller de API no ASP.NET Core  
C) único uso de COBOL em mainframe  
D) norma ABNT de papel A4  
E) somente Kanban WIP  

**23.** Em questão que contraste papéis, a FCC tipicamente considera **correto** que:

A) .NET é a plataforma; ASP.NET Core agrega o ferramental web/API sobre ela  
B) ASP.NET Core é o kernel do hypervisor  
C) Kestrel é o único banco relacional permitido  
D) middleware existe apenas em redes Token Bus  
E) Razor Pages aboliram o HTTP status 404  

**24.** Qual enunciado descreve **incorretamente** o ASP.NET Core?

A) Pode expor APIs REST  
B) Utiliza pipeline de middleware  
C) É multiplataforma na proposta moderna  
D) É, por definição, o protocolo IPv4 e o serviço DHCP ao mesmo tempo  
E) Pode usar Kestrel como servidor embutido  

**25.** A síntese mais alinhada ao edital, sobre ASP.NET Core, é:

A) framework web/API moderno no .NET, multiplataforma, com middleware e Kestrel; distinto de Web Forms e da mera plataforma .NET  
B) ASP.NET Core = Web Forms = IIS = jQuery (quatro nomes, um papel)  
C) Core elimina HTTP e vive só em desktop WinForms  
D) Core é apenas o nome comercial do PostgreSQL  
E) A distinção Core/clássico foi abolida em todas as bancas  

---

### Gabarito comentado (concurso 16–25)

16. **A** — Framework web/API no .NET moderno.  
17. **A** — Middleware + Kestrel + config = Core.  
18. **E** — Core ≠ Web Forms.  
19. **B** — Proxy na frente; Kestrel na app.  
20. **A** — FCC explora legado × Core.  
21. **A** — DI nativa com lifetimes.  
22. **B** — API / Minimal API.  
23. **A** — Plataforma × framework web.  
24. **D** — Não é IPv4/DHCP.  
25. **A** — Cartão mental completo do arquivo 04.

---

## 4. Auto-checagem rápida (opcional)

Antes de marcar a questão, pergunte:

1. É **plataforma/runtime**? → **.NET**.  
2. É **web/API, middleware, Kestrel**? → **ASP.NET Core**.  
3. É **Web Forms / System.Web clássico**? → **legado**, não Core.  
4. Servidor embutido típico? → **Kestrel** (IIS muitas vezes = proxy).  
5. Core = front React? → **Não** — pode servir API/UI; front JS é outro eixo (arquivo 06).

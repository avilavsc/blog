+++
title = '02 — C# / .NET'
date = '2026-10-16'
draft = false
weight = 2
tags = ['semana-05', 'csharp', 'dotnet']
categories = ['estudo']
+++
# C# / .NET · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — C#, arcabouço .NET e ASP.NET Core (visão)  
**Escopo deste arquivo:** Fundamentos de C# (sintaxe, tipos, operadores, fluxo), OOP (classes, interfaces, encapsulamento, herança, polimorfismo, sobrecarga/sobrescrita), exceções, acesso a banco, visão do ecossistema .NET e ASP.NET Core cobrável em prova.  
**Fora de escopo aqui:** Java/JVM (arquivo 01); Python (arquivo 03); HTML/CSS/JS e frameworks front (arquivos web da semana); VB/Delphi (mapa mental residual); redes e segurança.

---

## 1. Material

### 1.1 Por que isso cai no edital

No eixo **Programação**, o edital pede conhecimento profundo de **C#** e do **arcabouço .NET**, além de fundamentos: sintaxe, palavras-chave, estruturas básicas, compilação/execução, tipos primitivos, variáveis, literais e strings, operadores e precedência, controle de fluxo, classes/interfaces/métodos, encapsulamento, packages (namespaces), sobrecarga e sobrescrita, bibliotecas/componentes, **exceções** e **acesso a banco de dados**.

No eixo **Desenvolvimento web**, aparecem explicitamente **ASP.NET Core** e **.NET**.

Leitura cobrável:

| Peça | Papel em prova |
|------|----------------|
| **C#** | Linguagem tipada, OO, gerenciada (CLR) |
| **.NET / CLR / BCL** | Runtime, bibliotecas-base, compilação IL |
| **OOP em C#** | class, interface, abstract, virtual/override, sealed |
| **Exceções** | try/catch/finally, throw, Exception hierarchy |
| **ADO.NET / ORMs** | Acesso a BD (visão clássica + Entity Framework) |
| **ASP.NET Core** | Web APIs / MVC moderno no .NET |

Flash: a FCC gosta de **contrastar** C# com Java (sintaxe parecida, diferenças de `virtual`/`override`, `using`, propriedades, `async`/`await`) e de misturar **linguagem** com **framework web**.

### 1.2 Ecossistema .NET — âncora mental

| Conceito | Ideia cobrável |
|----------|----------------|
| **.NET** | Plataforma/runtime + bibliotecas para executar aplicações (hoje: .NET unificado; historicamente .NET Framework × .NET Core) |
| **CLR** | Common Language Runtime — máquina virtual que executa IL (Intermediate Language) |
| **CTS / CLS** | Type System comum / subset de interoperabilidade entre linguagens .NET |
| **BCL / FCL** | Bibliotecas-base (coleções, IO, networking, etc.) |
| **C#** | Linguagem principal do ecossistema Microsoft/.NET |
| **Assemblies** | Unidades de implantação (.dll / .exe) com metadados |

Fluxo clássico: código C# → **compilador** → **IL** → CLR (JIT) → código nativo.

Armadilha: confundir **.NET Framework** (legado, Windows-centric) com **.NET Core / .NET 5+** (cross-platform). Em prova moderna, “ASP.NET Core” aponta para o stack **cross-platform**.

### 1.3 Sintaxe e tipos — fundamentos

**Tipos de valor × referência (visão de prova):**

| | Valor (`struct`, primitivos) | Referência (`class`, `string`, arrays) |
|---|------------------------------|----------------------------------------|
| Armazenamento típico | Stack (conceito didático) | Heap; variável guarda referência |
| Cópia | Copia o valor | Copia a referência |
| Exemplos | `int`, `bool`, `double`, `decimal`, `DateTime`, enums | `string`, `object`, classes próprias |

Tipos primitivos cobráveis: `int`, `long`, `short`, `byte`, `float`, `double`, `decimal` (preciso para dinheiro), `bool`, `char`.  
`string` é **referência**, imutável; literais `"texto"`.  
`var` = inferência de tipo **em tempo de compilação** (ainda tipado estaticamente).  
`dynamic` = resolução em tempo de execução (diferente de `var`).

**Operadores e precedência (amostra):** aritméticos `+ - * / %`; comparação `== != < > <= >=`; lógicos `&& || !`; bitwise `& | ^ ~`; atribuição `= +=`; null-conditional `?.`, coalescência `??`, null-forgiving `!` (menos cobrado).

**Controle de fluxo:** `if/else`, `switch` (incluindo *switch expressions* em C# moderno), `for`, `foreach`, `while`, `do-while`, `break`/`continue`, `return`.

**Namespaces** ≈ organização de tipos (análogo a packages Java): `namespace App.Servicos;` + `using System;`.

### 1.4 Classes, membros e encapsulamento

```csharp
public class Processo
{
    private string _numero;           // campo
    public string Numero              // propriedade
    {
        get => _numero;
        set => _numero = value;
    }
    public DateTime DistribuidoEm { get; set; }  // auto-property

    public Processo(string numero)    // construtor
    {
        Numero = numero;
    }

    public virtual string Resumo() => $"Processo {Numero}";
}
```

Modificadores de acesso cobráveis:

| Modificador | Visibilidade |
|-------------|--------------|
| `public` | Qualquer código |
| `private` | Só a própria classe |
| `protected` | Classe + derivadas |
| `internal` | Mesmo assembly |
| `protected internal` | Assembly **ou** derivadas |
| `private protected` | Derivadas **no mesmo** assembly |

**Encapsulamento** = ocultar estado interno; expor via propriedades/métodos. Em C#, **properties** (`get`/`set`) são o idioma idiomático (não só getters/setters Java-style).

`static` = membro da **classe**, não da instância.  
`readonly` / `init` / `const` = imutabilidade em graus diferentes (prova costuma cobrar `const` em compile-time × `readonly` em runtime).

### 1.5 Herança, interfaces e polimorfismo

- Herança de **classe**: uma só classe-base (`class B : A`).  
- Interfaces: múltiplas (`class X : IRepo, ILog`).  
- `abstract class` = não instanciável; pode ter membros abstratos e concretos.  
- `interface` = contrato (em C# moderno pode ter implementação default — pegadinha avançada).  
- `virtual` na base + `override` na derivada = polimorfismo clássico.  
- `new` (ocultação) ≠ `override` (sobrescrita polimórfica) — FCC adora essa distinção.  
- `sealed` impede herança (classe) ou override adicional (método).  
- `base` chama membro da superclasse.

**Sobrecarga (overload)** = mesmo nome, assinaturas diferentes (parâmetros).  
**Sobrescrita (override)** = redefinição polimórfica de membro `virtual`/`abstract`.

Contraste rápido com Java (útil na revisão dirigida da semana 5):

| | C# | Java |
|---|----|------|
| Override explícito | `override` obrigatório sobre `virtual`/`abstract` | `@Override` opcional; métodos de instância *virtual* por padrão (não `final`) |
| Pacotes | `namespace` + `using` | `package` + `import` |
| Propriedades | nativas | getters/setters |
| Descarte | `using` / `IDisposable` | `try-with-resources` / `AutoCloseable` |

### 1.6 Exceções

Hierarquia mental: `System.Exception` → `SystemException` / `ApplicationException` (legado didático) → específicas (`ArgumentNullException`, `InvalidOperationException`, `IOException`…).

```csharp
try
{
    Validar(id);
}
catch (ArgumentException ex)
{
    // trata entrada inválida
}
catch (Exception ex)
{
    // genérico — último
}
finally
{
    // sempre executa (cleanup)
}
```

Regras cobráveis:

- `throw;` no catch **preserva** stack trace; `throw ex;` **reinicia** (armadilha clássica).  
- Preferir exceções específicas.  
- `finally` / `using` para liberar recursos (`IDisposable`).  
- Exceções **não** substituem controle de fluxo normal.

### 1.7 Coleções, LINQ e async (visão de prova)

Coleções frequentes: `List<T>`, `Dictionary<TKey,TValue>`, `HashSet<T>`, `Queue<T>`, `Stack<T>`, arrays `T[]`.  
**LINQ** = consultas sobre sequências (`Where`, `Select`, `FirstOrDefault`) — linguagem integrada a objetos.  
**async/await** + `Task`/`Task<T>` = modelo assíncrono idiomático em I/O (APIs, BD, HTTP). Não confundir com paralelismo CPU (`Parallel` / `Task.Run`).

### 1.8 Acesso a banco de dados

Camadas cobráveis:

| Abordagem | Ideia |
|-----------|-------|
| **ADO.NET** | Acesso “baixo nível”: `Connection`, `Command`, `DataReader`/`DataAdapter`, `DataSet` |
| **Provider** | SQL Server, PostgreSQL (Npgsql), Oracle, etc. |
| **ORM** | Entity Framework / EF Core — mapeia classes ↔ tabelas |
| **Boas práticas** | SQL parametrizado (anti SQL injection); `using` em conexões; transações |

Flash prova: **Connection string**, abrir/fechar conexão, comando com parâmetros, ler resultado. Em arquitetura moderna TRT/PDPJ, pensar também em **repositórios** e ORMs — mas o edital ancora em “acesso a banco”, então ADO.NET + ORM entram.

### 1.9 ASP.NET Core — visão cobrável

**ASP.NET Core** = framework web multiplataforma para **MVC**, **Razor Pages** e sobretudo **Web APIs** REST.

Peças típicas de prova:

| Peça | Função |
|------|--------|
| **Middleware** pipeline | Encadeia request/response (auth, HTTPS, routing, CORS…) |
| **Routing** | Mapeia URL → endpoint/controller |
| **Controllers** | Ações HTTP (`[HttpGet]`, `[HttpPost]`…) |
| **Model binding / validation** | Entrada → objetos; Data Annotations |
| **DI** | Injeção de dependências nativa (`AddScoped`/`Singleton`/`Transient`) |
| **Kestrel** | Servidor web embutido |
| **Hosting** | Pode ficar atrás de IIS/Nginx/Apache como reverse proxy |

Contraste didático: **ASP.NET clássico** (.NET Framework, System.Web) ≠ **ASP.NET Core** (stack moderno). Em edital atual, cobram **ASP.NET Core**.

### 1.10 Mapa mental rápido (revisão dirigida)

1. C# tipado + CLR + IL.  
2. Valor × referência; `string` imutável.  
3. Encapsulamento via properties; `public`/`private`/`protected`/`internal`.  
4. `virtual`/`override`/`abstract`/`interface`/`sealed`.  
5. Overload ≠ override.  
6. `try`/`catch`/`finally` + `using`/`IDisposable`.  
7. ADO.NET + parâmetros; EF Core como ORM.  
8. ASP.NET Core = APIs/MVC + middleware + DI.

---

## 2. Questões de treino (1–15)

**1.** No ecossistema .NET, o CLR é responsável principalmente por:

A) Executar IL (código intermediário), gerenciar memória (GC) e oferecer runtime às linguagens .NET  
B) Substituir o protocolo HTTP pelo SMTP  
C) Compilar exclusivamente código Python para bytecode CPython  
D) Gerenciar apenas firewalls de borda  
E) Definir o padrão UML 2.5.1  

**2.** Em C#, a diferença conceitual mais útil entre `var` e `dynamic` é:

A) `var` infere o tipo em compilação (ainda estático); `dynamic` resolve membros em execução  
B) Ambos eliminam o tipagem e viram JavaScript  
C) `var` só existe em Python  
D) `dynamic` é obrigatório em todo método `Main`  
E) São sinônimos estritos no CTS  

**3.** Um tipo adequado para valores monetários com precisão decimal em C#, em visão clássica de prova, é:

A) `decimal`  
B) `bool`  
C) `char`  
D) Exclusivamente `byte`  
E) Apenas `object` sem conversão  

**4.** Encapsulamento em C# é tipicamente expresso por:

A) Campos privados expostos de forma controlada via propriedades (`get`/`set`) ou métodos  
B) Tornar todos os campos `public` sem restrição  
C) Proibir namespaces  
D) Eliminar construtores  
E) Usar somente `goto`  

**5.** Em herança de classes em C#, a regra cobrável é:

A) Uma classe pode herdar de no máximo uma classe-base, podendo implementar várias interfaces  
B) Herança múltipla de classes concretas é sempre permitida como em C++ livre  
C) Interfaces são proibidas  
D) `sealed` obriga herança infinita  
E) `override` existe sem membro `virtual`/`abstract` correspondente  

**6.** A distinção correta entre sobrecarga e sobrescrita é:

A) Sobrecarga = mesmas nome com assinaturas diferentes; sobrescrita = redefinição polimórfica (`override`) de membro virtual/abstrato  
B) São termos idênticos no CTS  
C) Sobrescrita só existe em SQL  
D) Sobrecarga exige `sealed` obrigatório  
E) Sobrescrita remove o CLR  

**7.** O uso de `virtual` na classe-base e `override` na derivada serve a:

A) Polimorfismo: a implementação chamada depende do tipo real da instância  
B) Transformar o método em `static` automaticamente  
C) Impedir qualquer chamada ao método  
D) Compilar o programa como Python  
E) Criar um assembly COM obrigatório  

**8.** Em tratamento de exceções, o bloco `finally`:

A) Executa independentemente de ter havido exceção, tipicamente para liberar recursos  
B) Só roda se não existir `catch`  
C) Substitui o garbage collector  
D) É equivalente a `async void`  
E) Cancela o CLR  

**9.** A forma recomendada de relançar a mesma exceção preservando o stack trace é:

A) `throw;`  
B) `throw ex;` sempre (reinicia o stack) como única opção correta  
C) `return null;`  
D) `Environment.Exit(0);`  
E) `GC.Collect();`  

**10.** Em ADO.NET, a sequência conceitual típica de leitura é:

A) Abrir conexão → criar comando (de preferência parametrizado) → executar → ler resultado → fechar/dispor recursos  
B) Ignorar conexão e concatenar SQL com entrada do usuário sem parâmetros  
C) Usar somente FTP para consultar tabelas  
D) Substituir o banco por um arquivo hosts  
E) Compilar o SQL no CLR como IL de interface gráfica  

**11.** Entity Framework (EF Core), em visão de prova, caracteriza-se por:

A) Ser um ORM que mapeia classes/objetos a tabelas/relacionamentos, reduzindo SQL manual repetitivo  
B) Ser um servidor DNS  
C) Substituir o protocolo TLS  
D) Ser exclusivamente um orquestrador Kubernetes  
E) Ser o equivalente ao Scrum Master  

**12.** ASP.NET Core diferencia-se do ASP.NET clássico (.NET Framework / System.Web), em regra, por:

A) Ser multiplataforma, com pipeline de middleware, DI nativa e foco em Kestrel + hosting moderno  
B) Funcionar apenas em Windows XP sem CLR  
C) Proibir Web APIs REST  
D) Eliminar qualquer roteamento HTTP  
E) Ser um dialeto de COBOL  

**13.** No pipeline ASP.NET Core, middleware:

A) Compõe uma cadeia que processa a requisição/resposta (autenticação, HTTPS, CORS, routing etc.)  
B) É o nome do Product Owner  
C) Substitui o protocolo IP  
D) Existe somente em Delphi 7  
E) É um tipo de exchange fanout  

**14.** O modificador `internal` em C# torna o tipo/membro visível:

A) Dentro do mesmo assembly  
B) Em qualquer linguagem da Internet sem restrição  
C) Apenas em código JavaScript no browser  
D) Somente via reflexão em Python  
E) Exclusivamente em bancos Oracle  

**15.** A síntese mais adequada deste subtópico no edital é:

A) C# + .NET/CLR + OOP (classes/interfaces/virtual-override) + exceções + acesso a BD (ADO.NET/ORM) + ASP.NET Core  
B) Apenas interpretação de texto  
C) Apenas cabeamento estruturado  
D) Apenas direitos humanos  
E) Apenas porcentagem financeira  

---

### Gabarito comentado (treino 1–15)

1. **A** — CLR executa IL e hospeda o runtime.  
2. **A** — `var` estático inferido; `dynamic` em runtime.  
3. **A** — `decimal` para dinheiro/precisão.  
4. **A** — encapsulamento via properties/métodos.  
5. **A** — herança simples de classe + múltiplas interfaces.  
6. **A** — overload ≠ override.  
7. **A** — polimorfismo virtual/override.  
8. **A** — `finally` sempre para cleanup.  
9. **A** — `throw;` preserva stack.  
10. **A** — ciclo clássico ADO.NET parametrizado.  
11. **A** — EF Core = ORM.  
12. **A** — ASP.NET Core multiplataforma/middleware/DI.  
13. **A** — pipeline de middleware.  
14. **A** — `internal` = mesmo assembly.  
15. **A** — bloco completo do edital para C#/.NET.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto da plataforma .NET referida no edital de Analista Judiciário – Área de TI, é correto afirmar que:

A) programas em C# são tipicamente compilados para Intermediate Language (IL) e executados sob o Common Language Runtime (CLR), com apoio das bibliotecas-base do ecossistema  
B) o CLR é um protocolo de roteamento idêntico ao OSPF  
C) C# dispensa qualquer noção de tipo em tempo de compilação em todos os casos, inclusive com `var`  
D) assemblies .NET substituem integralmente a necessidade de sistemas operacionais  
E) o CTS existe apenas no interpretador CPython  

**17.** Acerca de orientação a objetos em C#, assinale a afirmativa correta.

A) classes podem herdar de uma única classe-base e implementar múltiplas interfaces; membros `virtual` podem ser redefinidos com `override` para polimorfismo  
B) herança múltipla de classes concretas é a regra obrigatória do CTS  
C) interfaces são proibidas no C# moderno  
D) `sealed` torna obrigatória a herança por qualquer assembly externo  
E) propriedades (`get`/`set`) são ilegais; só campos públicos são permitidos  

**18.** Em relação a exceções em C#, é correto afirmar que:

A) `try`/`catch`/`finally` permitem tratar falhas e garantir liberação de recursos; `throw;` no `catch` relança preservando o stack trace  
B) exceções devem substituir todo controle de fluxo normal (`if`/`return`)  
C) `finally` nunca executa quando há `catch`  
D) `throw ex;` é a única forma de preservar o stack trace original  
E) o bloco `using` de `IDisposable` é incompatível com limpeza de recursos  

**19.** Considere as afirmativas abaixo.

I. `decimal` é frequentemente indicado para valores monetários pela precisão decimal.  
II. `var` torna a variável dinamicamente tipada em tempo de execução, tal como `dynamic`.  
III. `internal` restringe a visibilidade, em regra, ao mesmo assembly.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e III, apenas  
D) II e III, apenas  
E) I, II e III  

**20.** No acesso a banco de dados a partir de aplicações .NET, a prática mais adequada entre as opções é:

A) utilizar comandos parametrizados (ADO.NET ou camada equivalente) e dispor corretamente conexões/comandos, podendo empregar ORM (ex.: EF Core) para mapeamento objeto-relacional  
B) concatenar entrada do usuário diretamente em SQL sem parâmetros  
C) manter conexões abertas indefinidamente sem política de ciclo de vida  
D) acessar tabelas exclusivamente via protocolo SMTP  
E) eliminar qualquer abstração de repositório/ORM por proibição do CTS  

**21.** A respeito do ASP.NET Core, é correto afirmar que:

A) trata-se de framework web multiplataforma com pipeline de middleware, injeção de dependências integrada e suporte a MVC/Web APIs, frequentemente hospedado com Kestrel atrás de proxy reverso  
B) coincide termo a termo com o ASP.NET clássico baseado apenas em System.Web do .NET Framework legado  
C) proíbe o uso de rotas HTTP e controladores  
D) elimina a necessidade de qualquer autenticação em sistemas judiciais  
E) é um dialeto oficial do Kanban  

**22.** Constitui equívoco típico em prova afirmar que:

A) sobrecarga (overload) e sobrescrita (override) são conceitos distintos em C#  
B) o CLR executa IL gerado a partir de linguagens .NET  
C) ASP.NET Core utiliza middleware no pipeline de requisições  
D) `private` limita o acesso ao próprio tipo  
E) em C#, uma classe pode herdar simultaneamente de duas classes-base concretas, como regra geral da linguagem  

**23.** Em um serviço REST escrito em ASP.NET Core que consulta PostgreSQL, a interpretação mais adequada para fins de concurso é:

A) C#/.NET fornece a linguagem e o runtime; ASP.NET Core expõe os endpoints HTTP; o acesso a dados pode ocorrer via ADO.NET/provider ou EF Core, com cuidado com parâmetros e ciclo de vida de conexões  
B) o CLR substitui o banco relacional  
C) middleware ASP.NET Core é o protocolo AMQP  
D) propriedades C# são inválidas em APIs  
E) exceções não podem ser usadas em I/O de banco  

**24.** Acerca de tipos e literais em C#, assinale a opção correta.

A) `string` é tipo referência imutável; tipos como `int` e `bool` são tipos de valor; literais e operadores seguem regras de precedência da linguagem  
B) todo tipo em C# é obrigatoriamente `dynamic`  
C) `foreach` é ilegal sobre `List<T>`  
D) namespaces são proibidos em assemblies públicos  
E) `const` e `readonly` são sempre semanticamente idênticos em qualquer contexto  

**25.** Para fins de preparação em Analista Judiciário – Área de TI (código 14), a síntese mais adequada deste subtópico é:

A) fundamentos de C#; plataforma .NET/CLR/BCL; OOP com classes, interfaces, encapsulamento, sobrecarga e sobrescrita; exceções; acesso a BD; ASP.NET Core em visão de Web API/MVC  
B) apenas PMBOK 8, sem programação  
C) apenas CNJ 400  
D) apenas Docker Swarm  
E) apenas crase e regência  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **A** |
| 17 | **A** |
| 18 | **A** |
| 19 | **C** |
| 20 | **A** |
| 21 | **A** |
| 22 | **E** |
| 23 | **A** |
| 24 | **A** |
| 25 | **A** |

**Notas rápidas:** 16 A (IL + CLR); 17 A (herança simples + interfaces + override); 18 A (try/catch/finally + `throw;`); 19 C (I e III; II confunde `var`×`dynamic`); 20 A (parâmetros + dispose + ORM); 21 A (ASP.NET Core moderno); 22 E (não há herança múltipla de classes); 23 A (API + dados); 24 A (string ref / value types); 25 A (síntese do arquivo 02).

---

*Arquivo 02 · C# / .NET · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 03 Python fundamentos.*

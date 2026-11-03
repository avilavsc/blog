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

## 3. Questões de concurso (16–25)

**16.** (FCC – TRT-12/2023 – Analista Judiciário – TI) Atenção: examine o programa C# abaixo (trecho).

```csharp
using (SHA256 sha256 = SHA256.Create()) {
  byte[] bytes = Encoding.UTF8.GetBytes(senha);
  I
  StringBuilder builder = new StringBuilder();
  for (int i = 0; i < hashBytes.Length; i++)
    builder.Append(hashBytes[i].ToString("x2"));
  return builder.ToString();
}
```

Para que a senha criptografada seja gerada corretamente, a lacuna `I` deve ser preenchida com:

A) `byte[] hashBytes = SHA256.GenerateHash(bytes);`
B) `hashBytes = sha256.ComputeHash(bytes);`
C) `byte[] hashBytes = sha256.GenerateHash(bytes);`
D) `hashBytes = SHA256.Hash(bytes);`
E) `byte[] hashBytes = sha256.ComputeHash(bytes);`

**17.** (FCC – TRT-17/2022 – Analista Judiciário – TI) Considere o programa C# abaixo.

```csharp
using System;
class Identifica {
  public void Identidade(String nome, int id){
    Console.WriteLine("Nome1: " + nome + ", " + "Id1: " + id);
  }
  public void Identidade(int id, String nome){
    Console.WriteLine("Nome2: " + nome + ", " + "Id2: " + id);
  }
  public static void Main(String[] args){
    I
  }
}
```

Considerando a saída esperada:
`Nome1: Carlos Silva, Id1: 1000`
`Nome2: Cristina Santos, Id2: 2000`
a lacuna I deve ser preenchida corretamente por:

A) `Identidade("Carlos Silva", 1000); Identidade(2000, "Cristina Santos");`
B) `Identifica obj = new Identifica(); obj.Identidade(1000,"Carlos Silva"); obj.Identidade("Cristina Santos",2000);`
C) `Identidade(1000, "Carlos Silva"); Identidade("Cristina Santos", 2000);`
D) `Identifica obj = new Identifica(); obj.Identidade("Carlos Silva", 1000); obj.Identidade(2000, "Cristina Santos");`
E) `Identifica obj = new Identifica(); obj.Identidade("Carlos Silva", 1000); Identifica obj = new Identifica(); obj.Identidade(2000, "Cristina Santos");`

**18.** (FCC – TRT-12/2023 – Analista Judiciário – TI) Em programas C# e PHP com tratamento de divisão por zero, as lacunas I (C#) e II (PHP) devem ser preenchidas, respectivamente, com:

A) I: `throw new DivideByZeroException("...");` II: `throw new Exception_DivideByZero("...");`
B) I: `throw new Exception("...")` II: `throw new Exception("...")`
C) I: `throw new DivideByZeroException("...")` II: `throw new DivideByZeroException("...")`
D) I: `throw new DivideByZeroException("Divisão por zero não é válida.");` II: `throw new Exception("Divisão por zero não é válida.");`
E) I: `throw new Exception("...");` II: `throw new DivideByZeroException("...");`

**19.** (FCC – TRT-1/2025 – Analista Judiciário – TI) O trecho de código C# a seguir mostra uma classe. As lacunas I e II são, correta e respectivamente, preenchidas com

```csharp
I {
  public static double CalcularRescisao(double salarioBase, int mesesTrabalhados) {
    double indenizacao = salarioBase * mesesTrabalhados / 12;
    return II;
  }
}
```

A) `public class CalculoRescisao : Object` e `indenizacao`
B) `override class Rescisao : Object` e `this.indenizacao`
C) `class CalculoRescisaoSalario extends Object` e `indenizacao*salarioBase`
D) `public class Rescisao : BaseClass` e `this.mesesTrabalhados*indenizacao`
E) `class BaseRescisao inherit Object` e `indenizacao`

**20.** (FCC – SEFAZ-SC/2018 – Auditor-Fiscal – TI) Um Auditor está trabalhando com uma aplicação em C# e a lista abaixo.

```csharp
List<Pessoa> lista = new List<Pessoa>();
lista.Add(p1); lista.Add(p2); lista.Add(p3);
```

Para percorrer os elementos desta lista obtendo cada objeto do tipo `Pessoa`, utiliza-se o comando

A) `foreach (Pessoa p in lista) { }`
B) `for(int indice=0; indice<=lista.Length(); indice++){lista.Get(Pessoa p);}`
C) `while(Pessoa p from lista) { }`
D) `for(int i=1; i<=lista.Length(); i++) {List.GetAt(Pessoa p);}`
E) `foreach(Get Pessoa in lista) { }`

**21.** (FCC – SEFAZ-SC/2018 – Auditor-Fiscal – TI) Entity Framework (EF) é um ORM para aplicações .NET. Após adicionar um objeto de domínio a um contexto, para salvar a entidade no banco de dados chama-se o método

A) `ExecuteUpdate()` da classe `DbTransaction`.
B) `SaveChanges()` da classe `DbContext`.
C) `InsertData()` da classe `DbConnection`.
D) `ExecuteQuery()` da classe `DbContext`.
E) `InsertRecord()` da classe `DbTransaction`.

**22.** (FCC – SEFAZ-SC/2018 – Auditor-Fiscal – TI) Considere a arquitetura de uma aplicação utilizando ASP.NET Web API. Nessa arquitetura,

A) o model pode ser serializado para JSON e XML.
B) o client, que consome a Web API, será obrigatoriamente um navegador web.
C) só pode haver um controller para manipular solicitações HTTP e representar os dados.
D) o model é representado por classes VBScript conhecidas como Plain Old Objects (POOs).
E) o controller permite requisições HTTP somente por meio do método GET.

**23.** (FCC – SEFAZ-SC/2018 – Auditor-Fiscal – TI) Considere código Razor (ASP.NET Web Pages) com lacunas I e II: verificar se o formulário foi submetido via POST e exibir a variável `resultado`. Os comandos corretos são, respectivamente,

A) `isset($_POST)` e `echo resultado`
B) `IsPost` e `@resultado`
C) `@POST` e `${resultado}`
D) `doPost()` e `out.print(resultado)`
E) `isPost()` e `${resultado}`

**24.** (FCC – TRT-12/2023 – Analista Judiciário – TI) Associando o SHA-256 do programa C# à segurança em redes sem fio, é correto afirmar que o

A) SHA-256 pode ser utilizado para criar hashes dos certificados digitais, que são usados para autenticar os dispositivos na rede, em uma implementação de WPA2 com autenticação baseada em certificados (como o EAP-TLS).
B) SHA-256 é um protocolo de segurança para redes sem fio, que utiliza um hash criptográfico pertencente à família das funções hash SHA-2.
C) WPA2 não contém mecanismos de autenticação; assim, é necessário acrescentar o uso de chaves pré-compartilhadas (PSK) e autenticação baseada em servidores usando SHA-256.
D) WPA2 é um método de hash criptográfico para redes sem fio, que utiliza AES-CCMP.
E) WPA utiliza o protocolo SHA-256 para criptografar dados e o método EAP para autenticação.

**25.** (FCC – SEFAZ-SC/2018 – Auditor-Fiscal – TI) Em trecho ASP.NET/ADO.NET, para adicionar na tabela `Funcionario` um campo inteiro chamado `Id`, a lacuna deve ser preenchida com

A) `dataset.Funcionario("int","Id");`
B) `Funcionario.AddField("int","Id");`
C) `dataset.AddColumn(Funcionario,"System.Int64", "Id");`
D) `AddNewColumn(Funcionario, "System.Int32", "Id");`
E) `AddColumn(Funcionario, "System.Integer64", "Id");`

---

### Gabarito comentado (concurso 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **E** | FCC TRT-12/2023 — `sha256.ComputeHash` |
| 17 | **D** | FCC TRT-17/2022 — sobrecarga + instância |
| 18 | **D** | FCC TRT-12/2023 — `DivideByZeroException` × `Exception` (PHP) |
| 19 | **A** | FCC TRT-1/2025 — herança `: Object` |
| 20 | **A** | FCC SEFAZ-SC/2018 — `foreach` em `List` |
| 21 | **B** | FCC SEFAZ-SC/2018 — `DbContext.SaveChanges` |
| 22 | **A** | FCC SEFAZ-SC/2018 — model JSON/XML |
| 23 | **B** | FCC SEFAZ-SC/2018 — Razor `IsPost` + `@resultado` |
| 24 | **A** | FCC TRT-12/2023 — SHA-256 e WPA2/EAP-TLS |
| 25 | **D** | FCC SEFAZ-SC/2018 — coluna `System.Int32` |

**Notas rápidas:** 16 E (`byte[] hashBytes = sha256.ComputeHash(bytes)`); 17 D (instanciar + ordem dos parâmetros); 18 D (exceção específica em C#; genérica em PHP); 19 A (`: Object` + return `indenizacao`); 20 A (`foreach`); 21 B (`SaveChanges`); 22 A (serialização); 23 B (Razor); 24 A (hash de certificados); 25 D (`AddNewColumn` + Int32).

---

*Arquivo 02 · C# / .NET · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 03 Python fundamentos.*

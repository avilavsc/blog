+++
title = '01 — Java — revisão dirigida'
date = '2026-10-15'
draft = false
weight = 1
tags = ['semana-05', 'java', 'revisao']
categories = ['estudo']
+++
# Java — revisão dirigida · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Java (revisão dirigida)  
**Escopo deste arquivo:** sintaxe e fundamentos cobráveis; tipos; operadores; fluxo; OO (classe, interface, encapsulamento, herança, polimorfismo, sobrecarga/sobrescrita); packages e modificadores; String/wrappers; coleções (visão); exceções; compilação/execução; JDBC/acesso a BD (visão de prova). Tom de **revisão** para quem já programa — fechar lacunas, não curso introdutório.  
**Fora de escopo aqui:** Spring/Boot/Cloud/JPA profundos (semanas 1–2); C#/.NET e Python (02–03); ASP.NET Core (04); HTML/JS/frameworks web (05–07).

---

## 1. Material

### 1.1 Por que isso cai no edital

No bloco **Programação**, o edital pede conhecimento profundo de **Java** (entre outras linguagens), com fundamentos explícitos: sintaxe, palavras-chave, estrutura e construções básicas, **compilação e execução**; tipos primitivos; variáveis; literais e strings; operadores e precedência; controle de fluxo e repetição; classes, interfaces, métodos e variáveis; **encapsulamento**, packages, **sobrecarga e sobrescrita**; bibliotecas e componentes; **exceções**; **acesso a banco de dados**.

No eixo **Arquitetura PDPJ-Br**, Java é a linguagem de referência do stack (Spring, JPA/Hibernate etc.). Esta semana = **revisão dirigida** da linguagem; o ecossistema Spring/JPA já foi (ou será reforçado) nos arquivos das semanas 1–2.

Flash: FCC mistura **conceitos OO**, **modificadores**, **exceções checked × unchecked**, **equals/hashCode**, **coleções** e **JDBC** com pegadinhas de “parece C#/Python”.

### 1.2 Compilação, execução e plataforma

| Peça | Papel |
|------|--------|
| **`.java`** | Código-fonte |
| **`javac`** | Compila → **bytecode** (`.class`) |
| **JVM** | Executa bytecode (interpretação/JIT) |
| **JRE** | Ambiente de execução (JVM + libs) |
| **JDK** | Desenvolvimento (javac + ferramentas + JRE) |
| **WORA** | *Write Once, Run Anywhere* — mesma bytecode em JVMs compatíveis |

Pontos cobráveis:

- Java **não** gera tipicamente executável nativo “puro” no fluxo clássico `javac` → JVM (embora existam AOT/Graal etc.; em prova clássica = bytecode + JVM).  
- `main` de entrada: `public static void main(String[] args)`.  
- Pacote padrão implícito se não houver `package`; boa prática = pacotes nomeados.

### 1.3 Tipos primitivos × referência

**Primitivos** (8): `byte`, `short`, `int`, `long`, `float`, `double`, `char`, `boolean`.

| Tipo | Ideia de prova |
|------|----------------|
| `int` / `long` | Inteiros; `long` literal com `L` |
| `float` / `double` | Ponto flutuante; `float` com `f` |
| `char` | Caractere Unicode (aspas simples) |
| `boolean` | `true` / `false` — **não** é 0/1 como em C |

**Referência:** objetos (`String`, arrays, coleções, suas classes). Variável guarda **referência**, não o objeto em si.

Wrappers: `Integer`, `Long`, `Double`, `Boolean`, `Character`… — **autoboxing/unboxing** entre primitivo e wrapper.

Armadilhas:

- Comparar wrappers com `==` pode falhar fora do cache de `Integer` (−128…127); preferir `equals` para igualdade de valor.  
- `null` só em referência; unboxing de `null` → `NullPointerException`.

### 1.4 Variáveis, literais, String e operadores

- Variáveis locais **devem** ser inicializadas antes do uso.  
- `final` em variável = não reatribuível (constante de referência; objeto referenciado ainda pode mutar se mutável).  
- `String` é **imutável**; concatenação repetida em loop → preferir `StringBuilder` (visão clássica).  
- Pool de strings: literais podem ser internados; `new String("x")` cria objeto distinto no heap.

Operadores (precedência mental útil):

1. Pós/pré `++` `--`  
2. Unários `+` `-` `!`  
3. `*` `/` `%`  
4. `+` `-`  
5. Relacionais `<` `>` `<=` `>=`  
6. Igualdade `==` `!=`  
7. Lógicos `&&` / `||` (curto-circuito) vs `&` / `|` (avaliam os dois lados)  
8. Ternário `? :`  
9. Atribuição `=` `+=` …

`==` em objetos compara **referências** (exceto quando a linguagem/contrato especial se aplica); conteúdo → `equals`.

### 1.5 Controle de fluxo e repetição

| Estrutura | Nota de prova |
|-----------|---------------|
| `if` / `else` / `else if` | Condição boolean |
| `switch` | Clássico: tipos limitados (`int`, `String`, enum…); `break` evita fall-through |
| `for` / `while` / `do-while` | `do-while` executa **ao menos uma vez** |
| `for-each` | `for (T item : colecao)` |
| `break` / `continue` | Sai do laço / próxima iteração |
| `return` | Sai do método |

Flash: `switch` sem `break` “cai” no próximo `case` (fall-through) — pegadinha clássica.

### 1.6 OO — classes, objetos, encapsulamento

**Classe** = molde; **objeto** = instância. Membros: campos (atributos), métodos, construtores, blocos estáticos/instância.

**Encapsulamento:** esconder estado interno; expor via métodos (`get`/`set` ou API controlada); modificadores restringem acesso.

| Modificador | Mesma classe | Mesmo pacote | Subclasse (outro pacote) | Mundo |
|-------------|--------------|--------------|--------------------------|-------|
| `private` | sim | não | não | não |
| *(default/package)* | sim | sim | não | não |
| `protected` | sim | sim | sim | não |
| `public` | sim | sim | sim | sim |

`static`: pertence à **classe**, não à instância. `this` = instância atual; `super` = membro da superclasse.

### 1.7 Herança, interfaces, abstração, polimorfismo

- Java: **herança simples** de classe (`extends`); múltiplas **interfaces** (`implements`).  
- `Object` é raiz de todas as classes.  
- Classe **`abstract`**: não instanciável; pode ter métodos abstratos e concretos.  
- **Interface** (visão moderna/clássica de prova): contrato; métodos abstratos (e, em versões recentes, `default`/`static`); campos tipicamente `public static final`.  
- **Polimorfismo:** referência da superclasse/interface aponta para subtipo; método sobrescrito chamado conforme o **objeto real** (despacho dinâmico).  
- **Sobrecarga (*overload*):** mesmo nome, **assinaturas** diferentes (parâmetros); resolvida em compilação.  
- **Sobrescrita (*override*):** redefine método da superclasse/interface; mesma assinatura compatível; `@Override` ajuda o compilador.

Regras úteis de override:

- Não reduzir visibilidade (ex.: `public` na mãe → não pode virar `private` na filha).  
- Exceções checked: não lançar checked **mais amplas** do que o método original.  
- `static`/`private`/`final` métodos **não** se sobrescrevem no sentido polimórfico clássico (`final` bloqueia override).

### 1.8 Packages, bibliotecas e organização

- `package` declara o namespace; `import` traz tipos (ou `import static` para estáticos).  
- Convenção: nomes invertidos de domínio (`br.jus.trt4...`).  
- JAR = empacotamento de classes/recursos; classpath define onde a JVM acha tipos.

### 1.9 equals, hashCode e contrato

Contrato clássico (prova adora):

- Se `a.equals(b)` → `a.hashCode() == b.hashCode()`.  
- Simetria, reflexividade, transitividade, consistência; `x.equals(null)` → `false`.  
- Sobrescrever `equals` ⇒ sobrescrever `hashCode` (coleções hash: `HashMap`, `HashSet`).

`==` ≠ `equals` para objetos (salvo identidade).

### 1.10 Coleções (visão cobrável)

| Interface / ideia | Uso mental |
|-------------------|------------|
| **List** | Sequência ordenada; índices; duplicatas ok (`ArrayList`, `LinkedList`) |
| **Set** | Sem duplicatas (`HashSet`, `TreeSet`) |
| **Map** | Chave → valor (`HashMap`, `TreeMap`) — **não** estende `Collection` |
| **Queue / Deque** | Filas / pontas duplas |

Generics (`List<String>`): type safety em compilação; type erasure em runtime (visão clássica).

### 1.11 Exceções

Hierarquia mental:

```
Throwable
├── Error          (falhas graves da JVM — em geral não capturar)
└── Exception
    ├── RuntimeException  (unchecked: NPE, IAE, IOBE…)
    └── demais Exception  (checked: IOException, SQLException…)
```

| Conceito | Prova |
|----------|--------|
| **Checked** | Devem ser declaradas (`throws`) ou tratadas (`try/catch`) em compilação |
| **Unchecked** | `RuntimeException` e subclasses — não obrigatório declarar |
| `try / catch / finally` | `finally` roda na saída do `try` (salvo casos extremos como `System.exit`) |
| try-with-resources | `AutoCloseable` — fecha recursos automaticamente |
| `throw` × `throws` | Lança × declara |

Flash: `Error` ≠ algo para “tratar no negócio”; `SQLException` é checked típica de JDBC.

### 1.12 Acesso a banco de dados (JDBC — visão)

Camada clássica Java SE:

| Peça | Papel |
|------|--------|
| **Driver JDBC** | Ponte com o SGBD |
| **`Connection`** | Conexão |
| **`Statement` / `PreparedStatement`** | SQL; preferir **PreparedStatement** (parâmetros, mitiga SQL injection) |
| **`ResultSet`** | Resultado de consulta |
| **URL JDBC** | Ex.: `jdbc:postgresql://host:5432/db` |

Fluxo mental: carregar/obter conexão → preparar SQL → executar (`executeQuery` / `executeUpdate`) → ler `ResultSet` → fechar recursos (try-with-resources).

No ecossistema PDPJ/TRT4 o dia a dia usa **JPA/Hibernate** + Spring — mas o edital de **Programação** cita acesso a BD na linguagem; JDBC continua cobrável como fundação.

### 1.13 Palavras-chave e “não confundir”

Amostra de palavras reservadas: `class`, `interface`, `extends`, `implements`, `new`, `this`, `super`, `static`, `final`, `abstract`, `void`, `return`, `try`, `catch`, `finally`, `throw`, `throws`, `package`, `import`, `synchronized`, `volatile`, `transient`, `native`, `enum`, `assert`…

Armadilhas:

- `goto` e `const` são **reservadas** mas **não usadas** como em C.  
- Não existe `unsigned` como em C.  
- `boolean` ≠ `Boolean` (primitivo × wrapper).  
- Sobrecarga ≠ sobrescrita.

### 1.14 O que a FCC tipicamente testa neste tema

1. `javac` → bytecode / JVM executa.  
2. Primitivos vs referência; wrappers e autoboxing.  
3. Encapsulamento + tabela `private` / default / `protected` / `public`.  
4. Overload × override.  
5. Interface × classe abstrata (contrato × herança parcial).  
6. Polimorfismo / despacho dinâmico.  
7. `String` imutável; `==` vs `equals`.  
8. Checked × unchecked; `finally`.  
9. List / Set / Map (Map não é Collection).  
10. JDBC: Connection, PreparedStatement, ResultSet.

### 1.15 Mini-síntese (cartão mental)

> **Java** = fonte → bytecode → JVM · **8 primitivos** + objetos · **OO**: encapsulamento, herança simples, interfaces, polimorfismo · **overload** (assinatura) × **override** (polimórfico) · **packages** + modificadores · **String** imutável · **exceções** checked/unchecked · **coleções** List/Set/Map · **JDBC** Connection/PreparedStatement/ResultSet.

---

## 2. Questões de treino (1–15)

**1.** No fluxo clássico de compilação/execução Java, o papel mais adequado de `javac` e da JVM é:

A) `javac` gera bytecode (`.class`); a JVM executa esse bytecode  
B) `javac` gera somente HTML; a JVM compila redes OSPF  
C) A JVM produz o `.java`; `javac` só formata CSS  
D) Não existe bytecode em Java  
E) `javac` substitui o PostgreSQL  

**2.** São tipos primitivos de Java:

A) `byte`, `short`, `int`, `long`, `float`, `double`, `char`, `boolean`  
B) `String`, `Object`, `List`, `Map` apenas  
C) Somente `integer` e `varchar` como no SQL  
D) Apenas `number` e `any` como em TypeScript  
E) `unsigned int` idêntico ao C  

**3.** Encapsulamento, em visão de prova OO, relaciona-se principalmente a:

A) Restringir acesso direto ao estado interno e expor comportamento controlado pela API da classe  
B) Obrigar herança múltipla de classes concretas  
C) Eliminar qualquer uso de packages  
D) Proibir exceções checked  
E) Substituir a JVM por um interpretador PHP  

**4.** Sobre modificadores de acesso, é correto afirmar que:

A) `private` restringe à própria classe; `public` é visível amplamente  
B) `private` é visível em qualquer pacote do classpath  
C) Não existe diferença entre `private` e `public`  
D) `protected` é mais restrito que `private`  
E) Default (package) é visível em todos os jars do mundo  

**5.** Sobrecarga (*overload*) difere de sobrescrita (*override*) porque:

A) Overload = mesmo nome com assinaturas diferentes; override = redefinição polimórfica compatível na subclasse/interface  
B) São sinônimos absolutos  
C) Override só existe em SQL  
D) Overload exige obrigatoriamente `throws IOException`  
E) Override elimina a JVM  

**6.** Em Java, uma classe pode:

A) Estender no máximo uma classe e implementar várias interfaces  
B) Estender ilimitadas classes concretas simultaneamente como herança múltipla clássica de classes  
C) Apenas implementar uma interface na história da linguagem  
D) Nunca usar `Object` como ancestral  
E) Proibir qualquer polimorfismo  

**7.** `String` em Java é tipicamente descrita como:

A) Imutável; operações de “alteração” geram novos objetos  
B) Sempre mutável byte a byte como `char[]` público  
C) Tipo primitivo idêntico a `int`  
D) Sinônimo de `StringBuilder`  
E) Exclusiva do pacote `java.sql`  

**8.** Comparar dois objetos pelo conteúdo relevante do domínio costuma exigir:

A) Uso adequado de `equals` (e `hashCode` coerente quando houver estruturas hash)  
B) Apenas `==`, que sempre compara conteúdo de `String` e de qualquer POJO  
C) Apenas o operador `===` do Java  
D) O comando SQL `JOIN`  
E) A anotação `@Entity` obrigatória  

**9.** Exceções *checked* caracterizam-se, em compilação, por:

A) Precisarem ser tratadas (`try/catch`) ou declaradas (`throws`), em regra  
B) Serem apenas `Error` da JVM  
C) Nunca poderem aparecer em JDBC  
D) Serem sinônimo exclusivo de `RuntimeException`  
E) Substituiem o `finally`  

**10.** O bloco `finally` em um `try/catch/finally`:

A) Executa-se na saída do `try` (com ou sem exceção), salvo situações extremas como encerramento abrupto da JVM  
B) Só roda se não houver `catch`  
C) Substitui a necessidade de `return` em qualquer método  
D) É obrigatório em toda classe abstrata  
E) Compila SQL no PostgreSQL  

**11.** Na hierarquia de coleções (visão clássica), `Map`:

A) Associa chaves a valores e **não** estende `Collection`  
B) É subinterface direta obrigatória de `List`  
C) Impede qualquer uso de generics  
D) Substitui a palavra-chave `class`  
E) É tipo primitivo  

**12.** Em JDBC, a combinação mais típica cobrada em fundamentos é:

A) `Connection` + `PreparedStatement`/`Statement` + `ResultSet`  
B) Somente `kubectl` e `Dockerfile`  
C) Apenas `Swing` e `AWT`  
D) Exclusivamente `Keycloak`  
E) Só `RabbitMQ` exchanges  

**13.** Preferir `PreparedStatement` em relação a concatenar SQL com input do usuário ajuda principalmente a:

A) Parametrizar a consulta e reduzir risco de SQL injection  
B) Eliminar a necessidade de driver JDBC  
C) Transformar Java em Python automaticamente  
D) Gerar bytecode a partir de HTML  
E) Criar microsserviços Eureka  

**14.** Polimorfismo de inclusão (visão OO Java) permite que:

A) Uma referência de superclasse/interface invoque a implementação do objeto real (subtipo)  
B) Apenas primitivos sejam passados por referência  
C) `private` se torne visível em qualquer pacote automaticamente  
D) `javac` seja desnecessário  
E) `Map` vire `List` sem tipos  

**15.** A síntese mais adequada deste subtópico no edital de Programação (Java) é:

A) Fundamentos de sintaxe/tipos/fluxo + OO (encapsulamento, packages, overload/override) + exceções + acesso a BD (ex.: JDBC), com Java também âncora do stack PDPJ  
B) Apenas cabeamento estruturado  
C) Apenas interpretação de texto  
D) Apenas ITIL 5  
E) Apenas Wi-Fi 802.11  

---

### Gabarito comentado (treino 1–15)

1. **A** — `javac` → `.class` (bytecode); JVM executa.  
2. **A** — os oito primitivos clássicos.  
3. **A** — ocultar estado + API controlada.  
4. **A** — `private` × `public` na tabela de visibilidade.  
5. **A** — overload = assinaturas; override = redefinição polimórfica.  
6. **A** — herança simples de classe + várias interfaces.  
7. **A** — `String` imutável.  
8. **A** — `equals`/`hashCode` para igualdade de conteúdo.  
9. **A** — checked = tratar ou declarar.  
10. **A** — `finally` na saída do `try`.  
11. **A** — `Map` não é `Collection`.  
12. **A** — trio JDBC clássico.  
13. **A** — parâmetros + mitigação de injection.  
14. **A** — despacho dinâmico / subtipo.  
15. **A** — bloco Programação + âncora PDPJ.

---

## 3. Questões de concurso (16–25)

**16.** (FCC – TRT-20/2024 – Analista Judiciário – TI) Um Tribunal, que está desenvolvendo um sistema de gerenciamento de processos, tem uma classe `Processo` com atributos `numero` e `ano`. Exemplifica o conceito de encapsulamento o fato de os atributos `numero` e `ano` serem

A) estáticos e compartilhados por todas as instâncias da classe `Processo`.
B) constantes e não poderem ser alterados para garantir integridade.
C) privados e acessados por métodos públicos `getNumero` e `getAno`.
D) públicos e acessados diretamente por meio dos métodos privados `getNumero` e `getAno`.
E) abstratos e acessados por classes derivadas, como `GerenciarProcesso`.

**17.** (FCC – TRT-17/2022 – Analista Judiciário – TI) Considere o programa em Java abaixo, que será executado em condições ideais.

```java
import java.util.Scanner;
public class Main {
  public static void main(String[] args) {
    Scanner s = new Scanner(System.in);
    try {
      Main obj = new Main();
      System.out.print("Digite o valor do dividendo: ");
      double dividendo = s.nextDouble();
      System.out.print("Digite o valor do divisor: ");
      double divisor = s.nextDouble();
      double resultado = obj.faz_Divisao(dividendo, divisor);
      System.out.println("O resultado da divisao é: " + resultado);
    } catch (Exception ex) {
      System.out.println(ex.getMessage());
    }
  }
  I {
    if(divisor == 0) {
      throw new Exception("Não é permitido fazer uma divisão por zero!");
    }
    return dividendo / divisor;
  }
}
```

Para que o programa, ao ser executado, funcione corretamente de acordo com a lógica prevista, a lacuna I deve ser corretamente preenchida por:

A) `public double faz_Divisao(double dividendo, double divisor)`
B) `public double faz_Divisao(double divisor, double dividendo) Exception ex`
C) `public double faz_Divisao(double d1, double d2) throws Exception`
D) `public double faz_Divisao(double divisor, double dividendo) Exception throws`
E) `public double faz_Divisao(double dividendo, double divisor) throws Exception`

**18.** (FCC – TRT-20/2024 – Analista Judiciário – TI) Em uma aplicação Java, considere as instruções abaixo.

```java
Class.forName("com.mysql.cj.jdbc.Driver");
Connection conectado = DriverManager.getConnection("jdbc:mysql://localhost:3306/bd", "root", "a9Kb08c");
```

Em condições ideais, as EXCEÇÕES que precisam ser tratadas nessas linhas são

A) `ClassNotFoundException` e `SQLException`.
B) `JDBCException` e `SQLException`.
C) `MySQLException` e `DatabaseException`.
D) `JDBCException` e `ConnectException`.
E) `DriverException` e `DatabaseException`.

**19.** (FCC – TRT-15/2023 – Analista Judiciário – TI) Considere os textos a seguir:

1. Trata-se de uma das principais técnicas que define a programação orientada a objetos. É embasada em um dos princípios que adicionam segurança à aplicação em uma programação orientada a objetos, pelo fato de esconder as propriedades, criando uma espécie de caixa preta. A maior parte das linguagens orientadas a objetos a implementam, baseada em propriedades privadas, ligadas a métodos especiais chamados getters e setters, que irão retornar e setar o valor da propriedade, respectivamente.
2. Objetos filhos herdam as características e ações de seus "ancestrais". Entretanto, em alguns casos, é necessário que as ações para um mesmo método sejam diferentes. Em outras palavras, essa técnica consiste na alteração do funcionamento interno de um método herdado de um objeto pai.

Os textos tratam, correta e respectivamente, de princípios e técnicas da orientação a objetos, que são:

A) persistência e polimorfismo.
B) visibilidade e persistência.
C) herança e visibilidade.
D) instanciação e encapsulamento.
E) encapsulamento e polimorfismo.

**20.** (FCC – TRT-20/2024 – Técnico Judiciário – TI) Uma equipe de TI está desenvolvendo um sistema de controle de funcionários. Criaram uma classe base `Funcionario` e as subclasses `Gerente` e `Vendedor`, que utilizam os atributos de `Funcionario` e possuem atributos adicionais específicos. O conceito de orientação a objetos aplicado quando `Gerente` e `Vendedor` são definidas como subclasses de `Funcionario` é

A) Objeto.
B) Polimorfismo.
C) Herança.
D) Encapsulamento.
E) Interface.

**21.** (FCC – TRT-11/2017 – Técnico Judiciário – TI) Considere a classe Java abaixo.

```java
public class Processo {
  private String numeroProcesso;
}
```

Um Técnico especializado em TI afirma, corretamente, que:

A) para incluir um valor no atributo `numeroProcesso` através de um objeto dessa classe será necessário criar um método privado `getNumeroProcesso`.
B) poderão ser incluídos nessa classe um construtor que não recebe parâmetros e um construtor que recebe como parâmetro o número do processo.
C) a instrução `Processo p = new Processo("10453");` instancia um objeto dessa classe utilizando o construtor padrão vazio.
D) para permitir encapsulamento, os novos atributos e métodos a serem incluídos nessa classe terão que ser privados.
E) não será possível instanciar um objeto dessa classe, pois ela não tem construtor.

**22.** (FCC – TRF-4/2025 – Analista Judiciário – Análise de Sistemas) Considere o exemplo abaixo:

```java
interface ServidorJudiciario {
  void analisarProcesso();
  void realizarAudiencia();
}
class AnalistaTI implements ServidorJudiciario { }
```

Segundo o Princípio da Segregação de Interface (ISP), esse código apresenta um problema de design, já que `AnalistaTI` é obrigada a implementar `realizarAudiencia()`. Para corrigir essa violação ao ISP, a abordagem mais adequada é

A) remover o método `realizarAudiencia()` da interface `ServidorJudiciario`, criando interfaces separadas para responsabilidades distintas, pois nenhum cliente deve ser forçado a depender de métodos que não utiliza.
B) manter a interface `ServidorJudiciario` como está, mas implementar o método `realizarAudiencia()` na classe `AnalistaTI` com um corpo vazio.
C) tornar o método `realizarAudiencia()` um método default com implementação vazia na interface.
D) substituir a interface `ServidorJudiciario` por uma classe abstrata que implemente `analisarProcesso()` e `realizarAudiencia()` com comportamento genérico.
E) implementar uma exceção no método `realizarAudiencia()` da classe `AnalistaTI`, lançando `UnsupportedOperationException` sempre que ele for chamado.

**23.** (FCC – TRT-19/2022 – Analista Judiciário – TI) Considere a classe abaixo em uma aplicação que utiliza JPA e Hibernate.

```java
@Entity
public class Cliente {
  @Id
  @GeneratedValue(strategy = GenerationType. I )
  private Long id;
}
```

Para informar ao provedor de persistência que os valores do identificador serão gerados pela coluna de autoincremento da tabela, utiliza-se, na lacuna I, o valor

A) `TABLE`
B) `AUTO_GENERATED`
C) `AUTO_INCREMENT`
D) `IDENTITY`
E) `SELF_GENERATED`

**24.** (FCC – TRT-17/2022 – Analista Judiciário – TI) Considere o método abaixo, em aplicação Java com JPA 2.0, em que `manager` é um `EntityManager` válido:

```java
TypedQuery query = manager.createNamedQuery("Departamento.findAll", Departamento.class);
```

Nesse método, `"Departamento.findAll"` refere-se

A) ao método `findAll` existente na classe `Departamento`.
B) a uma query nomeada por meio da anotação `@NamedQuery` na classe `Departamento`.
C) ao comando SELECT da classe `Departamento`, nomeado pela anotação `@SQLQuery`.
D) ao método `findAll`, que será criado na classe `Departamento`, quando esta instrução for executada.
E) a um método de uma stored procedure que será executada no banco de dados.

**25.** (FCC – TRT-11/2017 – Técnico Judiciário – TI) Considere a classe Java abaixo (ordenação por seleção). A lacuna I deverá ser preenchida por

```java
x = v[i];
I
v[min] = x;
```

A) `v[i] = v[x];`
B) `v[i] = v[j];`
C) `v[x] = v[i];`
D) `v[i] = v[min];`
E) `v[i] = v[min+1];`

---

### Gabarito comentado (concurso 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **C** | FCC TRT-20/2024 — encapsulamento (private + getters) |
| 17 | **E** | FCC TRT-17/2022 — `throws Exception` |
| 18 | **A** | FCC TRT-20/2024 — `ClassNotFoundException` + `SQLException` |
| 19 | **E** | FCC TRT-15/2023 — encapsulamento e polimorfismo |
| 20 | **C** | FCC TRT-20/2024 — herança |
| 21 | **B** | FCC TRT-11/2017 — sobrecarga de construtores |
| 22 | **A** | FCC TRF-4/2025 — ISP / interfaces |
| 23 | **D** | FCC TRT-19/2022 — `GenerationType.IDENTITY` |
| 24 | **B** | FCC TRT-17/2022 — `@NamedQuery` |
| 25 | **D** | FCC TRT-11/2017 — selection sort / swap |

**Notas rápidas:** 16 C (atributos private + getters públicos); 17 E (assinatura com `throws Exception` e nomes dos parâmetros); 18 A (forName + getConnection); 19 E (caixa-preta/getters × comportamento distinto); 20 C (subclasse = herança); 21 B (dois construtores); 22 A (segregar interfaces); 23 D (IDENTITY); 24 B (named query); 25 D (`v[i] = v[min]`).

---

*Arquivo 01 · Java — revisão dirigida · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 C# / .NET.*

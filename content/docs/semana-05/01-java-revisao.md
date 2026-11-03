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

## 3. Questões estilo concurso FCC (16–25)

**16.** Acerca do ambiente de execução Java, assinale a afirmação correta:

A) O compilador `javac` traduz, em regra, código-fonte `.java` em bytecode armazenado em arquivos `.class`, executado pela JVM em plataformas compatíveis  
B) Todo programa Java deve ser necessariamente convertido em assembly x86 único, sem JVM, para qualquer prova de concurso  
C) A JVM gera o código-fonte e o programador apenas desenha diagramas UML  
D) Arquivos `.class` são scripts Bash interpretados pelo kernel Linux exclusivamente  
E) Java dispensa tipos e compilação em qualquer formulação do edital  

**17.** No modelo de orientação a objetos da linguagem Java, encapsulamento e modificadores de acesso destinam-se, em essência, a:

A) controlar a visibilidade de membros e proteger o estado interno das classes, em consonância com o princípio de ocultação de informação  
B) garantir herança múltipla irrestrita de classes concretas  
C) abolir o uso de interfaces  
D) tornar todos os atributos automaticamente `public`  
E) substituir exceções por códigos de retorno obrigatórios do estilo C  

**18.** Considere as afirmativas abaixo sobre métodos em Java.

I. A sobrecarga permite métodos de mesmo nome com listas de parâmetros distintas.  
II. A sobrescrita permite redefinir em subclasse um comportamento herdado, preservando assinatura compatível para despacho polimórfico.  
III. Um método `final` de instância pode ser livremente sobrescrito em qualquer subclasse.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**19.** Relativamente a exceções em Java, é correto afirmar que:

A) exceções checked, em regra, devem ser tratadas ou declaradas na assinatura do método, ao passo que `RuntimeException` e subclasses são unchecked  
B) `Error` e `Exception` são idênticos e sempre devem ser capturados em código de negócio  
C) `SQLException` jamais pode ser checked  
D) o bloco `finally` é proibido na linguagem  
E) `throws` lança a exceção e `throw` apenas documenta  

**20.** Em estruturas de dados do pacote de coleções, assinale a opção correta:

A) `List` admite sequência ordenada e duplicatas; `Set` não admite duplicatas; `Map` associa chaves a valores e não estende `Collection`  
B) `Map` é subtipo obrigatório de `List`  
C) `Set` exige índices numéricos como `List`  
D) Generics existem apenas em tempo de execução, nunca em compilação  
E) `ArrayList` é tipo primitivo  

**21.** No acesso a banco de dados via JDBC, a sequência conceitual mais adequada é:

A) obter `Connection`, criar `PreparedStatement` (ou `Statement`), executar a operação e manipular `ResultSet` quando houver consulta, fechando recursos adequadamente  
B) abrir apenas um arquivo `.css` e executar `javac` sobre o SGBD  
C) usar exclusivamente `kubectl apply` no lugar do driver  
D) substituir SQL por YAML de Kubernetes em toda consulta  
E) dispensar URL JDBC e driver em qualquer SGBD relacional  

**22.** Constitui equívoco típico em prova de Java afirmar que:

A) `String` é imutável  
B) Java permite implementar múltiplas interfaces  
C) `private` restringe o membro à própria classe  
D) `==` entre referências compara identidade, não necessariamente igualdade de conteúdo  
E) Java admite herança múltipla irrestrita de classes concretas, no mesmo sentido clássico de herdar várias superclasses com estado  

**23.** Acerca de classes abstratas e interfaces (leitura cobrável para concurso), assinale a alternativa correta:

A) classe abstrata não é instanciável diretamente e pode misturar métodos abstratos e concretos; interface expressa contrato implementável por classes distintas, permitindo a uma classe implementar várias interfaces  
B) interfaces e classes abstratas são rigorosamente a mesma construção sintática  
C) toda interface exige herança múltipla de classes  
D) classe abstrata substitui a JVM  
E) interfaces não podem aparecer no edital de Programação  

**24.** Em um cenário em que duas instâncias de uma classe de domínio são usadas como chaves em um `HashMap`, a prática correta exige, em regra:

A) contratos coerentes de `equals` e `hashCode`, de modo que objetos iguais tenham o mesmo hash  
B) apenas o operador `++`  
C) proibição de qualquer método `equals`  
D) conversão obrigatória para `int` primitivo  
E) uso exclusivo de `System.out.println`  

**25.** Para fins de preparação em Analista Judiciário – Área de TI (código 14), a síntese mais adequada da revisão dirigida de Java é:

A) domínio dos fundamentos (tipos, fluxo, sintaxe), OO (encapsulamento, packages, sobrecarga/sobrescrita, interfaces), exceções e acesso a dados (JDBC como base), alinhado ao papel de Java no stack PDPJ-Br  
B) apenas PMBOK 8 e COBIT, sem linguagem  
C) apenas Direitos Humanos internacionais  
D) apenas Bootstrap 5, sem tipagem  
E) apenas Delphi/VB, excluindo Java do edital  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **A** |
| 17 | **A** |
| 18 | **C** |
| 19 | **A** |
| 20 | **A** |
| 21 | **A** |
| 22 | **E** |
| 23 | **A** |
| 24 | **A** |
| 25 | **A** |

**Notas rápidas:** 16 A (javac/bytecode/JVM); 17 A (encapsulamento + visibilidade); 18 C (I e II; III falso — `final` não sobrescreve); 19 A (checked × unchecked); 20 A (List/Set/Map); 21 A (fluxo JDBC); 22 E (não há herança múltipla de classes); 23 A (abstract × interface); 24 A (equals/hashCode); 25 A (síntese do arquivo 01).

---

*Arquivo 01 · Java — revisão dirigida · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 C# / .NET.*

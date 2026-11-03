+++
title = '02 — PHP (visão)'
date = '2026-10-16'
draft = false
weight = 2
tags = ['semana-06', 'php', 'visao']
categories = ['estudo']
+++
# PHP (visão) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — PHP (visão comparativa / fundamentos cobráveis)  
**Escopo deste arquivo:** Natureza da linguagem (interpretada, server-side), sintaxe e tipos, variáveis/superglobais, operadores e fluxo, arrays/strings, inclusão de arquivos, OOP básico, exceções, sessões/cookies, acesso a BD (mysql legado × MySQLi × PDO) e contraste cobrável com C/Java/C#.  
**Fora de escopo aqui:** Linguagem C em profundidade (arquivo 03); frameworks PHP (Laravel/Symfony) além de menção residual; front (HTML/CSS/JS detalhado — semana 5); arquiteturas SOA/microsserviços (outros arquivos da semana 6).

---

## 1. Material

### 1.1 Por que isso cai no edital

No eixo **Programação**, o edital exige conhecimento profundo de **PHP** junto com VB, Delphi, Python, C, Java e C#, cobrindo: sintaxe, palavras-chave, estrutura e construções básicas, **compilação/execução** (no PHP: interpretação pelo motor Zend/engine + opcache), tipos primitivos, variáveis, literais e strings, operadores e precedência, controle de fluxo e repetição, classes/interfaces/métodos, encapsulamento, packages (namespaces), sobrecarga e sobrescrita (com nuances), bibliotecas e componentes, **exceções** e **acesso a banco de dados**.

PHP também aparece no **desenvolvimento web** (lado servidor clássico). A FCC cobra sobretudo **fundamentos + armadilhas de API** (include×require, `$_POST`×`$_GET`, MySQLi×PDO×`mysql_*` removida), não um framework específico.

| Peça | Papel em prova |
|------|----------------|
| **Motor / interpretador** | Script `.php` executado no servidor (Apache/Nginx + FPM/CGI ou embutido) |
| **Tipagem dinâmica** | Tipo ligado ao valor; variáveis com `$` |
| **Superglobais** | `$_GET`, `$_POST`, `$_SESSION`, `$_SERVER`, `$_COOKIE`, `$_REQUEST`, `$GLOBALS` |
| **include / require** | Inclusão de arquivos; falha ≠ comportamento |
| **MySQLi / PDO** | Acesso a BD moderno; `mysql_*` = legado removido |
| **OOP + exceções** | `class`, `extends`, `try`/`catch`/`finally` |

Flash da semana 6: **visão comparativa PHP × C** — PHP dinâmico/web/interpretado; C estático/compilado/baixo nível (ponteiros, `malloc`). Não misturar idioms.

### 1.2 Modelo de execução e sintaxe mínima

PHP é linguagem **server-side**, tipicamente **interpretada** (com cache de opcodes em produção). Tags: `<?php ... ?>` (evitar short tags `<?` em material de prova moderna). Saída: `echo` / `print`; fim de instrução `;`.

```php
<?php
$nome = "Claudionir";
echo "Olá, $nome";          // interpolação em aspas duplas
echo 'Olá, $nome';         // aspas simples: literal
```

Variáveis **sempre** com `$`. Nomes case-sensitive (`$a` ≠ `$A`). Constantes: `define('PI', 3.14)` ou `const PI = 3.14` (em escopo de classe/namespace).

**Tipos escalares cobráveis:** `int`, `float`/`double`, `string`, `bool`, `null`. Compostos: `array`, `object`, `callable`, `resource` (legado). Conversão automática é comum; `===` compara valor **e** tipo; `==` faz coerção (armadilha clássica: `"0" == false`).

Operadores: aritméticos `+ - * / % **`; concatenação de string é `.` (não `+`); comparação `== != === !== < > <= >= <=>`; lógicos `&& || !` e também `and`/`or` (precedência diferente — pegadinha); incremento `$i++`; coalescência `??`; nullsafe `?->` (PHP 8+).

Fluxo: `if/elseif/else`, `switch`, `match` (PHP 8), `for`, `foreach`, `while`, `do-while`, `break`/`continue`, `return`. Funções: `function soma($a, $b) { return $a + $b; }` — parâmetros tipados e retorno tipado existem nas versões modernas, mas tipagem estrita exige `declare(strict_types=1);`.

### 1.3 Arrays, strings e superglobais

Arrays PHP são **mapas ordenados** (índice numérico ou chave associativa). Podem misturar:

```php
$a = [10, 20, "x" => 30];
$a[] = 40;                 // push
foreach ($a as $k => $v) { /* ... */ }
```

Funções cobráveis: `count`, `array_push`/`array_pop`, `isset`/`empty`/`unset`, `in_array`, `array_key_exists`, `explode`/`implode`, `strlen`, `substr`, `str_replace`, `trim`.

**Superglobais** (sempre disponíveis, qualquer escopo):

| Variável | Uso típico |
|----------|------------|
| `$_GET` | Query string (`?id=1`) |
| `$_POST` | Corpo de formulário method=POST |
| `$_REQUEST` | Mistura GET/POST/COOKIE (menos segura/previsível) |
| `$_SESSION` | Dados de sessão no servidor |
| `$_COOKIE` | Cookies enviados pelo cliente |
| `$_SERVER` | Ambiente HTTP (método, URI, IP, script…) |
| `$_FILES` | Upload de arquivos |
| `$GLOBALS` | Todas as variáveis globais |

Formulário HTML `method="post"` → ler com **`$_POST["campo"]`** (não `$_POST("campo")` — parênteses = erro; não `$_FORM`). Método GET → `$_GET`. Confundir POST/GET e função×array é distrator FCC.

Sessão:

```php
session_start();                 // obrigatório antes de usar
$_SESSION["usuario"] = $login;
session_unset();                 // remove variáveis da sessão
session_destroy();               // destrói a sessão
```

Ordem cobrável clássica (FCC MPE-PB 2015 e similares): remover variáveis com **`session_unset()`** e destruir com **`session_destroy()`**.

### 1.4 include, require e variantes `_once`

| Construto | Arquivo ausente | Reinclusão |
|-----------|-----------------|------------|
| `include` | **Warning**, script **continua** | Pode incluir de novo |
| `require` | **Fatal error**, script **para** | Pode incluir de novo |
| `include_once` | Warning + continua | Inclui **só uma vez** |
| `require_once` | Fatal + para | Inclui **só uma vez** |

Distratores: dizer que include/require são idênticos; que `require` “verifica se já foi incluído” (isso é o `_once`); que include é só HTML e require só PHP.

### 1.5 OOP, namespaces e exceções (visão)

```php
namespace App\Servico;

class Processo {
    private string $numero;
    public function __construct(string $n) { $this->numero = $n; }
    public function getNumero(): string { return $this->numero; }
}

class ProcessoEspecial extends Processo {
    public function getNumero(): string { return "ESP-" . parent::getNumero(); }
}
```

- `public` / `protected` / `private` — encapsulamento.  
- Herança simples de classe (`extends`); múltiplas **interfaces** (`implements`).  
- `abstract` / `final`; traits (`use TraitX`) para reuso horizontal.  
- Sobrescrita: redefinir método na filha; sobrecarga clássica de métodos **não** existe como em Java/C# (mesmo nome com assinaturas diferentes na mesma classe) — PHP 8 tem *named args* e sobrecarga via outras vias, mas a pegadinha de prova é: **não há overload estático clássico**.  
- Namespaces ≈ packages.

Exceções: `try { ... } catch (Exception $e) { ... } finally { ... }`; `throw new Exception("msg");`. Em PHP 7+ há `Throwable` (`Error` + `Exception`).

### 1.6 Acesso a banco: mysql × MySQLi × PDO

Três famílias históricas para MySQL:

| Extensão | Status / ideia |
|----------|----------------|
| **`mysql_*`** (`mysql_connect`…) | **Obsoleta** (5.5) e **removida** no PHP 7 — distrator de prova |
| **MySQLi** | “MySQL Improved”; API procedural **e** OO; só MySQL/MariaDB |
| **PDO** | PHP Data Objects; API OO; **vários** SGBDs (MySQL, PG, SQLite, Oracle…) |

**MySQLi procedural (padrão de questão FCC TJ-CE 2022 e afins):**

```php
$conn = mysqli_connect($server, $user, $pass, $db);
if (!$conn) {
    die("Erro: " . mysqli_connect_error());
}
// ... consultas com mysqli_query, mysqli_fetch_*, mysqli_num_rows ...
mysqli_close($conn);
```

OO: `$conn = new mysqli(...);`.

**PDO (padrão FCC CREMESP 2016 e afins):**

```php
$conn = new PDO(
    "mysql:host=$servername;dbname=meuDB",
    $username,
    $password
);
$conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
```

DSN correto: `mysql:host=...;dbname=...` — não `mysql_host=`, não `PDO_connect(...)`.

**Segurança cobrável:** prepared statements (`prepare`/`bind`/`execute`) contra SQL injection; nunca concatenar input cru em SQL. PDO costuma ser a resposta quando a prova fala em **portabilidade** entre bancos; MySQLi quando o enunciado é **só MySQL** e mostra API `mysqli_*`.

### 1.7 Contraste rápido PHP × C (visão da semana)

| | PHP | C |
|--|-----|---|
| Execução | Interpretada (server-side web) | Compilada para nativo |
| Tipagem | Dinâmica (opcionalmente tipada) | Estática, fraca em conversões implícitas |
| Memória | Gerenciada pelo engine | Manual (`malloc`/`free`) |
| Ponteiros | Não no modelo C | Núcleo da linguagem |
| Web | Nativo (superglobais, sessões) | Via libs (CGI etc.), não “idioma web” |
| OOP | Classes nativas | Structs; OOP é C++ |

### 1.8 Checklist FCC (antes das questões)

1. `$` em variável; `.` concatena; `===` sem coerção.  
2. `$_POST["campo"]` após formulário POST.  
3. `include` avisa e segue; `require` mata; `_once` evita duplicar.  
4. `session_start` → `$_SESSION`; limpar com `session_unset` + `session_destroy`.  
5. `mysql_*` morta; preferir **MySQLi** ou **PDO**.  
6. PDO DSN: `mysql:host=...;dbname=...`.  
7. Prepared statements = antídoto a SQL injection.  
8. OOP: herança simples + interfaces; sem overload clássico estilo Java.

---

## 2. Questões de treino (1–15)

**1.** Em PHP, a concatenação de strings usa o operador:

A) `+`  
B) `.`  
C) `&`  
D) `||`  
E) `::`

**2.** A principal diferença comportamental entre `include` e `require`, quando o arquivo não existe, é:

A) São idênticos em qualquer falha  
B) `require` só inclui HTML; `include` só PHP  
C) `include` gera aviso e continua; `require` gera erro fatal e interrompe  
D) `include` verifica se o arquivo já foi incluído; `require` não  
E) Ambos removem a extensão MySQLi

**3.** Após `session_start()`, o armazenamento típico de um dado de usuário na sessão é:

A) `$_POST["usuario"] = $login;` sem sessão  
B) `session_cookie($login);`  
C) `$_SERVER["usuario"] = $login;` como substituto oficial da sessão  
D) `$_SESSION["usuario"] = $login;`  
E) `mysql_session($login);`

**4.** Para ler o campo `nome` enviado por formulário HTML com `method="post"`, o idiom correto é:

A) `$nome = $_POST("nome");`  
B) `$nome = $_GET["nome"];` (única opção válida para POST)  
C) `$nome = $_POST["nome"];`  
D) `$nome = $_FORM["nome"];`  
E) `$nome = $_GET("nome");`

**5.** A extensão `mysql_*` (`mysql_connect` etc.), em PHP moderno (7+), encontra-se:

A) Como API recomendada oficial única  
B) Obrigatoriedade em todo script CLI  
C) Equivalente semântico a `session_start`  
D) Removida / indisponível — deve-se usar MySQLi ou PDO  
E) Substituta do protocolo TLS

**6.** Em PDO, a forma correta de conectar a um MySQL no banco `meuDB` é tipicamente:

A) `PDO_connect("host=$server;dbname=meuDB", $user, $pass)`  
B) `new PDO("mysql_host=$server;dbname=?", $user, $pass)`  
C) `mysqli_pdo_connect($server)`  
D) `mysql_connect($server, $user, $pass)` como DSN PDO  
E) `new PDO("mysql:host=$server;dbname=meuDB", $user, $pass)`

**7.** MySQLi diferencia-se do PDO, em visão clássica de prova, porque:

A) PDO só funciona com Oracle  
B) MySQLi foi removido no PHP 5  
C) MySQLi é específico para MySQL (procedural e/ou OO); PDO é abstração multi-SGBD  
D) Ambos são sinônimos estritos do `mysql_*`  
E) PDO não permite prepared statements

**8.** O operador `===` em PHP:

A) É idêntico a `=`  
B) Concatena strings  
C) Abre conexão PDO  
D) Compara valor e tipo, sem coerção  
E) Inicia sessão automaticamente

**9.** `require_once "lib.php";` garante principalmente que:

A) O script ignore a ausência do arquivo  
B) Apenas CSS seja carregado  
C) `$_GET` seja limpo  
D) O garbage collector do C seja acionado  
E) O arquivo seja incluído no máximo uma vez; se ausente, erro fatal

**10.** Em OOP PHP, assinale a afirmativa correta:

A) Herança múltipla de classes concretas é a regra obrigatória  
B) Não existem modificadores `private`/`protected`  
C) Uma classe pode estender uma única classe-base e implementar várias interfaces  
D) Namespaces são proibidos  
E) `extends` só funciona com structs C

**11.** Para remover as variáveis de sessão e em seguida destruir a sessão (PHP 5+ clássico de prova), usa-se respectivamente:

A) `session_unset()` e `session_destroy()`  
B) `session_commit()` e `session_close()`  
C) `session_clear()` e `session_reset()`  
D) `session_remove()` e `session_destroy()`  
E) `unset_all()` e `destroy_all()`

**12.** Prepared statements em MySQLi/PDO servem principalmente para:

A) Substituir o protocolo HTTP  
B) Mitigar SQL injection ao separar SQL de parâmetros  
C) Compilar PHP para assembly  
D) Criar cookies HttpOnly automaticamente  
E) Eliminar a necessidade de `session_start`

**13.** Em arrays PHP, é correto afirmar que:

A) Só admitem chaves inteiras consecutivas a partir de 0  
B) São imutáveis como tuplas Python  
C) Exigem declaração de tipo estática obrigatória em toda versão  
D) Não podem ser percorridos com `foreach`  
E) Podem misturar índices numéricos e chaves associativas no mesmo array

**14.** A leitura correta de um parâmetro de query string `?id=10` é tipicamente:

A) `$_POST["id"]` como única via para query string  
B) `$_FILES["id"]`  
C) `$_GET["id"]`  
D) `$_SESSION` sem `session_start`  
E) `$GLOBALS["GET"]["id"]` como única forma válida

**15.** A síntese mais adequada deste subtópico no edital é:

A) Apenas cabeamento estruturado  
B) Apenas CNJ 400  
C) Apenas ponteiros e `malloc`  
D) Fundamentos PHP (sintaxe, tipos, fluxo, arrays, OOP, exceções) + web (superglobais, sessão, include/require) + BD (MySQLi/PDO; legado `mysql_*` inválido)  
E) Apenas PMBOK 8

---

### Gabarito comentado (treino 1–15)

1. **B** — concatenação com `.`.  
2. **C** — include avisa; require é fatal.  
3. **D** — `$_SESSION` após `session_start`.  
4. **C** — `$_POST["nome"]` (colchetes).  
5. **D** — `mysql_*` removida.  
6. **E** — DSN `mysql:host=...;dbname=...`.  
7. **C** — MySQLi = MySQL; PDO = multi-SGBD.  
8. **D** — `===` valor+tipo.  
9. **E** — `_once` + fatal se faltar.  
10. **C** — herança simples + interfaces.  
11. **A** — `session_unset` + `session_destroy`.  
12. **B** — anti SQL injection.  
13. **E** — arrays mistos/associativos.  
14. **C** — query string via `$_GET`.  
15. **D** — bloco completo PHP do edital.

---

## 3. Questões de concurso (16–25)

> Questões reais de provas (FCC). Gabarito só ao final.

**16.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Considere o seguinte trecho de código PHP que utiliza herança de classe. O código calcula o valor das férias de um trabalhador, incluindo o adicional de 1/3 sobre o salário. <?php class FeriasBase { public function calcularFerias($salario, $dias) { return ($salario / 30) * $dias; } } class FeriasComBonus extends FeriasBase { public function calcularFeriasComBonus($salario, $dias) { $feriasBase = I; return $feriasBase + ($salario * 0.33); } } Para que o trecho de código acima possa ser executado em condições ideais, a lacuna I deve ser corretamente preenchida com:

A) FeriasBase::calcularFerias($salario, $dias)  
B) self::calcularFerias($salario, $dias)  
C) parent.calcularFerias($salario, $dias)  
D) $this.calcularFerias($salario, $dias)  
E) parent::calcularFerias($salario, $dias)  

**17.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Um desenvolvedor está criando uma aplicação React na qual o usuário pode inserir um texto e vê-lo imediatamente atualizado em tempo real em uma lista abaixo do campo de entrada. Para isso ele decide usar um estado local para armazenar o valor do campo de entrada e o conteúdo da lista. Nesse contexto, em condições ideais, a abordagem adequada para atualizar o estado quando o usuário digitar no campo de entrada é usar

A) `this.setState` em componentes funcionais para manter o estado atualizado a cada caractere digitado.  
B) `setState` dentro de um componente de classe com `this.state.input` para atualizar o estado do campo.  
C) o Hook `useEffect` para definir o valor de input toda vez que o campo tiver seu conteúdo alterado.  
D) o Hook `useState` e atualizar o valor do estado diretamente no evento `onChange` do campo de entrada.  
E) `useState` e `useEffect` em conjunto para garantir que o estado seja atualizado corretamente.  

**18.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Em um aplicativo móvel criado utilizando React Native, em condições ideais, um desenvolvedor precisa que um botão ocupe a largura total da tela, tenha um fundo azul e o texto branco centralizado. Para estilizar o botão no React Native, ele deve

A) usar o objeto `StyleSheet.create` com propriedades como `width: '100%'`, `backgroundColor: 'blue'` e `color: 'white'`.  
B) usar classes CSS tradicionais no arquivo `.css` vinculado ao componente com `width: '100%'`, `backgroundColor: blue` e `color: #000000`.  
C) adicionar os estilos `width: 'auto'`, `backgroundColor: 'blue'` e `color: '#ffffff'` dentro do próprio arquivo JavaScript separado e importar como CSS.  
D) usar o atributo `style` diretamente no componente com as propriedades CSS `width: auto`, `background-color: blue` e `color: #FFF`.  
E) usar `className`, adicionar uma classe para os estilos no componente e formatar a classe com `width: 'auto'`, `backgroundColor: 'blue'` e `color: '#ffffff'`.  

**19.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Usando JPA 2.0, um Técnico utilizou a anotação `@MappedSuperclass` que é útil para mapear uma classe de entidade

A) abstrata que não será diretamente persistida no banco de dados.  
B) que será persistida em uma tabela única com outras entidades.  
C) que será persistida em uma tabela separada.  
D) concreta persistida diretamente no banco de dados.  
E) que será persistida em uma tabela de junção.  

**20.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Uma Técnica de um Tribunal do Trabalho usou o seguinte código exemplo JSON para converter um objeto JavaScript em uma string JSON:

```javascript
const object = { nome: "Renata", idade: 25 };
const jsonString = __I__;
console.log(jsonString); // Saída: {"nome":"Renata","idade":25}
```

O código estará correto se a lacuna I for corretamente preenchida por

A) JSON.encode(object)  
B) JSON.stringify(object)  
C) stringify.JSON(object)  
D) object.stringify()  
E) object.toJSONString()  

**21.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Considere o seguinte código de exemplo de um webhook em Node.js:

```javascript
const express = require('express');

const app = express();
app.use(express.json());

__I__ => {
 console.log('Webhook received:', req.body);
 res.status(200).send('Webhook received successfully');
});

app.listen(3000, () => {
 console.log('Server is listening on port 3000');
});
```

Para receber e processar requisições http `post` para o endpoint `/webhook` é necessário que a lacuna I seja corretamente preenchida por

A) msg.post('/webhook', (req, send)  
B) app.post('/webhook,(req, http)  
C) post.msg('/webhook',(http, send)  
D) msg.post('/webhook', (req, res)  
E) app.post('/webhook', (req, res)  

**22.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Considere resultado uma variável do tipo ResultSet, que contém os dados de um produto, resultantes de uma consulta bem-sucedida na tabela produto do banco de dados. Considere que um dos campos dessa tabela é nome, referente ao nome do produto.

Neste contexto, uma Analista deseja exibir o nome do produto dentro do campo nome de um formulário criado em uma página JSP, por meio da linha abaixo.

`<input type="text" id="nome" name="nome" value="I">`

Para que isso seja possível, a lacuna I deve ser preenchida corretamente por

A) <% resultado.getValue("nome") %>  
B) <%= resultado.getValue("nome") %>  
C) <%= resultado.getString("nome") %>  
D) <%= resultado.StringOf("nome") %>  
E) <% resultado.getString("nome") %>  

**23.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
O `<schema>` é o elemento raiz de todo XML Schema e pode conter alguns atributos, como

A) `lang`, `namespace` e `type`.  
B) `xmlnsType`, `defaultNamespace` e `defaultLanguage`.  
C) `name`, `type` e `value`.  
D) `base`, `value` e `namespace`.  
E) `targetNamespace`, `xmlns` e `elementFormDefault`.  

**24.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
Uma Analista está desenvolvendo uma API REST para um aplicativo e deseja criar a documentação usando o Swagger. Essa documentação é criada adequadamente através

A) do Swagger Codegen, que permite a criação manual da especificação OpenAPI.  
B) da interface Swagger Editor, que permite criar a especificação OpenAPI interativamente.  
C) da elaboração de um arquivo de documentação PDF com o Swagger PDF Editor contendo a descrição das APIs.  
D) da interface Swagger GetJet, que cria e exibe automaticamente a documentação.  
E) da elaboração de um arquivo XML de forma manual, contendo a especificação OpenDocumentAPI.  

**25.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
Um Analista deseja realizar mudanças nos dados de um componente e atualizar a interface do usuário de acordo com essas mudanças. No React, a sintaxe correta do método usado para atualizar o estado de um componente é:

A) this.componentState({ novo_estado: novo_valor });  
B) this.updateState({ novo_estado: novo_valor });  
C) this.setState({ novo_estado: novo_valor });  
D) this.changeState({ novo_estado: novo_valor });  
E) this.modifyState({ novo_estado: novo_valor });  

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **E** | FCC TRT-1/2025 Q35 — Programação |
| 17 | **D** | FCC TRT-20/2024 Q45 — Desenvolvimento Web |
| 18 | **A** | FCC TRT-20/2024 Q46 — Desenvolvimento Web |
| 19 | **A** | FCC TRT-20/2024 Q56 — Desenvolvimento Web |
| 20 | **B** | FCC TRT-20/2024 Q57 — Desenvolvimento Web |
| 21 | **E** | FCC TRT-20/2024 Q58 — Desenvolvimento Web |
| 22 | **C** | FCC TRT-19/2022 Q41 — Desenvolvimento Web |
| 23 | **E** | FCC TRT-19/2022 Q42 — Desenvolvimento Web |
| 24 | **B** | FCC TRT-12/2023 Q27 — Desenvolvimento de Aplicações Web e Mobile |
| 25 | **C** | FCC TRT-12/2023 Q32 — Desenvolvimento de Aplicações Web e Mobile |

**Notas rápidas:** 16 E; 17 D; 18 A; 19 A; 20 B; 21 E; 22 C; 23 E; 24 B; 25 C.

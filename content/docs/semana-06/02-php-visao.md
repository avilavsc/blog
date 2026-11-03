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

## 3. Questões estilo concurso FCC (16–25)

> Adaptadas de padrões FCC (CREMESP 2016 PDO; TJ-CE 2022 MySQLi; MPE-PB 2015 sessão; questões de include/require e superglobais) — nível Analista TI.

**16.** (FCC · CREMESP/SP · Analista de TI · 2016 — adaptada) Há duas maneiras principais de conectar uma aplicação web em PHP a um MySQL: MySQLi ou PDO. No uso de PDO, para conectar ao banco `meuDB`, a opção correta é:

A) `$conn=new PDO("mysql_host=$servername;dbname=?", $username, $password); $conn->setParameter(1, "meuDB");`  
B) `$conn=new PDO("mysql:host=$servername;dbname=meuDB", $username, $password); $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);`  
C) `$conn=PDO_connect("host=$servername;dbname=meuDB", $username, $password);`  
D) `$conn=new PDO("odbc://$servername:3306/dbname", $username, $password);`  
E) `$conn=PDO_connect("host://$servername:3306;dbname=?", $username, $password);`

**17.** (FCC · TJ-CE · Analista Judiciário · 2022 — adaptada) No trecho que conecta via API procedural MySQLi, as lacunas equivalentes a conectar, obter erro de conexão e fechar são, respectivamente:

A) `mysql_connect` – `mysql_error()` – `mysql_close`  
B) `connect_mysql` – `error_mysql()` – `close_mysql`  
C) `pdo_connect` – `pdo_connect_error()` – `pdo_close`  
D) `connect_mysqli` – `error_mysqli()` – `close_mysqli`  
E) `mysqli_connect` – `mysqli_connect_error()` – `mysqli_close`

**18.** (FCC · MPE-PB · 2015 — adaptada) Para remover todas as variáveis de sessão e destruir a sessão no PHP 5, utilizam-se, respectivamente:

A) `session_commit` e `session_close()`  
B) `session_clear` e `session_reset()`  
C) `session_abort` e `session_reset()`  
D) `session_remove()` e `session_destroy()`  
E) `session_unset()` e `session_destroy()`

**19.** Em PHP, sobre `include` e `require`, é correto afirmar que:

A) o comportamento das duas é idêntico em falha de arquivo  
B) `include` gera aviso e continua; `require` gera erro e interrompe se o arquivo não for encontrado  
C) `require` verifica se o arquivo já foi incluído e, se sim, não inclui de novo (papel exclusivo do `require` simples)  
D) `require` só inclui PHP; `include` só HTML  
E) ambas foram removidas no PHP 7

**20.** Considere um formulário HTML submetido por POST ao script `processar.php`. O comando correto para obter o campo `nome` é:

A) `$nome = $_POST("nome");`  
B) `$nome = $_GET["nome"];`  
C) `$nome = $_GET("nome");`  
D) `$nome = $_POST["nome"];`  
E) `$nome = $_FORM["nome"];`

**21.** Acerca de superglobais em PHP, assinale a afirmativa correta.

A) `$_POST`, `$_GET`, `$_SESSION` e `$_SERVER` estão disponíveis independentemente do escopo local da função  
B) `$_SEND` e `$_HTTP` são superglobais padrão obrigatórias  
C) `$_SESSION` funciona plenamente sem qualquer chamada a `session_start()` em qualquer SAPI  
D) `$_FILES` armazena apenas cookies  
E) `$GLOBALS` foi removida em favor exclusiva de `$_REQUEST`

**22.** Considere as afirmativas.

I. PDO oferece interface orientada a objetos e pode trabalhar com vários SGBDs.  
II. MySQLi oferece APIs procedural e orientada a objetos, voltadas ao MySQL.  
III. As funções `mysql_connect` / `mysql_query` permanecem a API recomendada no PHP 8.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III

**23.** Constitui equívoco típico em prova afirmar que:

A) `===` compara tipo e valor  
B) `require_once` evita inclusão duplicada  
C) prepared statements ajudam contra SQL injection  
D) arrays PHP podem ser associativos  
E) em PHP, a concatenação de strings é feita com o operador `+`, como regra geral da linguagem

**24.** Em um serviço web PHP que autentica usuário e consulta PostgreSQL, a interpretação mais adequada é:

A) sessão via `session_start`/`$_SESSION`; acesso a dados preferencialmente com PDO (DSN adequado) e parâmetros vinculados; validar entrada em vez de concatenar SQL  
B) usar somente `mysql_connect` mesmo contra PostgreSQL  
C) armazenar senha em texto puro em `$_GET`  
D) substituir o SGBD por `include` de HTML  
E) eliminar `try`/`catch` por proibição da linguagem

**25.** Para o cargo de Analista Judiciário – TI (código 14), a síntese correta deste arquivo é:

A) apenas interpretação de texto  
B) apenas redes 802.1X  
C) PHP: tipagem dinâmica, fluxo, arrays, OOP/exceções, include/require, superglobais/sessão e BD com MySQLi/PDO (legado `mysql_*` inválido), em visão comparativa com C  
D) apenas Delphi VCL  
E) apenas ITIL 4 guiding principles

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **B** |
| 17 | **E** |
| 18 | **E** |
| 19 | **B** |
| 20 | **D** |
| 21 | **A** |
| 22 | **C** |
| 23 | **E** |
| 24 | **A** |
| 25 | **C** |

**Notas rápidas:** 16 B (DSN PDO clássico FCC); 17 E (`mysqli_*` procedural); 18 E (`unset`+`destroy`); 19 B (include×require); 20 D (`$_POST[]`); 21 A (superglobais); 22 C (III falso — `mysql_*` morta); 23 E (concatenação é `.`); 24 A (sessão+PDO/params); 25 C (síntese).

---

*Pack semana 6 · subtópico 02 · FCC TRT4 código 14 · visão PHP.*

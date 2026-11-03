+++
title = '03 — Linguagem C (visão)'
date = '2026-10-17'
draft = false
weight = 3
tags = ['semana-06', 'c', 'visao']
categories = ['estudo']
+++
# Linguagem C (visão) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — C (visão comparativa / fundamentos cobráveis)  
**Escopo deste arquivo:** Compilação e execução, sintaxe, tipos e operadores, fluxo, funções, arrays, **ponteiros**, structs/`typedef`, alocação dinâmica (`malloc`/`calloc`/`realloc`/`free` + `sizeof`), headers/biblioteca padrão, contraste com PHP/Java/C# e limites (C ≠ C++).  
**Fora de escopo aqui:** PHP em profundidade (arquivo 02); C++ STL/classes (só aviso de fronteira); structures de dados avançadas além do necessário para ponteiro/struct; redes e SO (semanas seguintes).

---

## 1. Material

### 1.1 Por que isso cai no edital

O edital lista **C** entre as linguagens de conhecimento profundo, com os mesmos eixos: sintaxe, palavras-chave, construções básicas, **compilação e execução**, tipos primitivos, variáveis, literais e strings, operadores e precedência, controle de fluxo, “classes/interfaces” (em C: o análogo cobrável são **structs**, funções e modularização por arquivos — **não** há classes), encapsulamento (visão via escopo/`static`), packages (headers/módulos), sobrecarga/sobrescrita (**não** existem como em Java/C#), bibliotecas, exceções (**C não tem try/catch**; erros via códigos de retorno/`errno`) e acesso a BD (via bibliotecas C, visão residual).

Na semana 6 o cronograma pede **PHP/C em visão comparativa**: FCC adora misturar “linguagem de sistema” com “linguagem web”.

| Peça | Papel em prova |
|------|----------------|
| **Compilação** | Pré-processador → compilador → linker → executável |
| **Tipagem estática** | Tipo declarado; checagem em compile-time |
| **Ponteiros** | `&` endereço; `*` desreferência |
| **`sizeof` / `malloc` / `free`** | Tamanho em bytes + heap |
| **`struct` / `typedef`** | Dados agregados |
| **stdio** | `printf` / `scanf` |

Flash: **C não é C++**. Distrator grave: falar em `class`, `new`/`delete`, herança ou overload de métodos como se fossem C puro.

### 1.2 Pipeline de compilação e programa mínimo

```c
#include <stdio.h>

int main(void) {
    printf("TRT4\n");
    return 0;
}
```

| Etapa | Ideia |
|-------|-------|
| **Pré-processador** | `#include`, `#define`, `#ifdef` — expansão de texto |
| **Compilação** | Traduz unidades `.c` para objeto (asm/máquina) |
| **Linkagem** | Resolve símbolos (`printf` na libc etc.) |
| **Execução** | Processo nativo no SO |

`#include <stdio.h>` busca header do sistema; `#include "meu.h"` busca local/projeto. Headers declaram protótipos; implementação fica em `.c` ou na biblioteca.

Palavras-chave cobráveis: `int`, `char`, `float`, `double`, `void`, `if`, `else`, `for`, `while`, `do`, `switch`, `case`, `break`, `continue`, `return`, `struct`, `typedef`, `sizeof`, `static`, `const`, `extern`, `enum`, `union`, `sizeof` (operador).

### 1.3 Tipos, literais, operadores e fluxo

Tipos inteiros: `char`, `short`, `int`, `long` (+ `signed`/`unsigned`). Ponto flutuante: `float`, `double`. Não há tipo `string` nativo: strings são **vetores de `char` terminados em `\0`**.

```c
char nome[] = "Ana";     // {'A','n','a','\0'}
int v[5] = {1, 2, 3, 4, 5};
```

Operadores: aritméticos `+ - * / %`; relacionais `== != < > <= >=`; lógicos `&& || !`; bit a bit `& | ^ ~ << >>`; atribuição `= += -=`; incremento `++ --`; ternário `?:`; **endereço** `&`; **indireção** `*`; `sizeof`.

Precedência (amostra): `[]` `.` `->` altos; unários `* & ! ~ ++ -- sizeof`; `* / %`; `+ -`; shifts; relacionais; `==`; `&`; `^`; `|`; `&&`; `||`; `?:`; atribuições.

Fluxo: `if/else`, `switch/case/default`, `for`, `while`, `do-while`, `break`/`continue`, `goto` (evitar; raro em boa prova). Funções: tipo de retorno + nome + parâmetros; protótipo antes do uso.

**Passagem de parâmetros:** por **valor** (cópia). Para “alterar” variável do chamador, passa-se **ponteiro** (endereço).

```c
void dobra(int *p) { *p = (*p) * 2; }
int x = 3;
dobra(&x);   // x vira 6
```

### 1.4 Ponteiros — núcleo cobrável

Ponteiro = variável que guarda **endereço**.

```c
int a = 10;
int *p = &a;     // p aponta para a
*p = 20;         // a passa a 20
printf("%d", a); // 20
```

| Expressão | Significado |
|-----------|-------------|
| `&x` | Endereço de `x` |
| `*p` | Conteúdo apontado por `p` |
| `p + 1` | Avança `sizeof(*p)` bytes (aritmética de ponteiros) |
| `NULL` | Ponteiro nulo (não aponta válido) |

Declarações (armadilha de leitura):

- `int *x, y, z;` → só `x` é ponteiro; `y` e `z` são `int`.  
- `int *x, *y, *z;` → três ponteiros.  

Arrays “decaem” para ponteiro ao primeiro elemento na maioria das expressões: `v` ≈ `&v[0]`. `scanf("%d", &x)` precisa do endereço; para vetor de `char`, `scanf("%s", buf)` já passa endereço base (cuidado com overflow — tema de segurança).

Operador `->` acessa membro via ponteiro para struct: `ptr->campo` ≡ `(*ptr).campo`.

### 1.5 `struct`, `typedef` e agregados

```c
struct Data {
    int dia, mes, ano;
};

typedef struct {
    int codigo;
    char nome[80];
    struct Data nasc;
} Aluno;

Aluno a;
a.codigo = 1;
```

- Cada elemento da struct é **membro/campo**; tipos **podem** ser heterogêneos.  
- `typedef` cria **apelido (alias)** para um tipo — facilita leitura; não “cria magia OOP”.  
- Structs aninhadas são comuns.  
- `union` compartilha o mesmo espaço de memória entre membros (menos cobrado, mas aparece).  
- `enum` nomeia constantes inteiras.

Não confundir: declarar `struct Cadastro aluno;` — `Cadastro` é o tag; `aluno` é a **variável**.

### 1.6 Alocação dinâmica: `sizeof`, `malloc`, `free`

| Peça | Função |
|------|--------|
| **`sizeof`** | Operador: tamanho em **bytes** de tipo ou expressão |
| **`malloc(n)`** | Aloca `n` bytes no heap; retorna `void *` (ou `NULL` se falhar) |
| **`calloc(q, tam)`** | Aloca e **zera** |
| **`realloc(p, n)`** | Redimensiona bloco |
| **`free(p)`** | Libera bloco previamente alocado |

Padrão cobrável:

```c
double *v = malloc(10 * sizeof(double));
if (v == NULL) { /* tratar falha */ }
/* usa v[i] ... */
free(v);
v = NULL;   // boa prática: evitar dangling
```

Para struct:

```c
struct Dados *prim = malloc(sizeof(struct Dados));
```

Armadilhas:

- `malloc` recebe **bytes**, não “número de blocos” abstrato sem `sizeof`.  
- `free` **não** usa `sizeof`.  
- `double[10]` é alocação **estática/automática** (conforme escopo), não dinâmica.  
- Esquecer `free` → vazamento; usar após `free` → undefined behavior.  
- Cast `(double *)malloc(...)` era comum em C89; em C99+ a conversão de `void *` é implícita — provas antigas ainda mostram o cast.

Alocação **estática/automática** (compile-time / stack de frame) × **dinâmica** (runtime / heap): a prova gosta de perguntar qual expressão é dinâmica → resposta com `malloc(...)`.

### 1.7 Biblioteca padrão e I/O

| Header | Conteúdo típico |
|--------|-----------------|
| `<stdio.h>` | `printf`, `scanf`, `fopen`, `fprintf`, `fgets` |
| `<stdlib.h>` | `malloc`, `free`, `atoi`, `exit`, `rand` |
| `<string.h>` | `strlen`, `strcpy`, `strcmp`, `strcat` |
| `<math.h>` | `sqrt`, `pow`, … |
| `<ctype.h>` | `isdigit`, `toupper` |

`printf("%d %f %s", i, x, s);` — especificadores cobráveis: `%d` int, `%f` float/double, `%c` char, `%s` string, `%p` ponteiro, `%.1f` uma casa decimal.

`scanf` com tipos numéricos exige `&variavel`. Buffer overflow em `%s` sem largura é risco clássico (liga com segurança OWASP na semana 8).

### 1.8 O que C **não** tem (pegadinhas)

| Recurso | Em C puro |
|---------|-----------|
| Classes / herança / `virtual` | Não (isso é C++) |
| `new` / `delete` | Não (`malloc`/`free`) |
| Exceções `try/catch` | Não |
| Sobrecarga de funções | Não (nomes devem diferir; ou `_Generic` em C11 — raro em FCC) |
| Gerenciador automático de memória | Não (manual) |
| Strings objeto | Não (`char[]` + `\0`) |

Contraste com PHP (arquivo 02): PHP gerencia memória, tem classes/exceções/superglobais web; C exige disciplina de ponteiros e `free`.

### 1.9 Checklist FCC

1. Compilar ≠ interpretar; headers + link.  
2. Ponteiro guarda endereço; `*` acessa valor.  
3. `int *x, y;` → só `x` é ponteiro.  
4. `sizeof` mede bytes; base do `malloc`.  
5. `malloc(n * sizeof(T))` + `free`.  
6. `struct` = campos heterogêneos; `typedef` = alias.  
7. Passagem por valor; “saída” via ponteiro/`&`.  
8. Não importar `class`/`new` do C++ para C.

---

## 2. Questões de treino (1–15)

**1.** No pipeline típico de um programa C, a ordem conceitual correta é:

A) Linkagem → pré-processamento → execução sem compilação  
B) Apenas interpretação linha a linha como PHP puro  
C) Pré-processador → compilação → linkagem → execução  
D) Somente JIT da CLR .NET  
E) Geração exclusiva de bytecode Python

**2.** A declaração `int *p, q;` significa:

A) `p` e `q` são ponteiros para `int`  
B) Apenas `q` é ponteiro  
C) Ambos são `double`  
D) Apenas `p` é ponteiro para `int`; `q` é `int`  
E) É sintaxe inválida em qualquer dialeto C

**3.** Dado `int a = 5; int *p = &a; *p = 9;`, o valor de `a` passa a ser:

A) 5  
B) endereço de `p`  
C) 9  
D) 0  
E) indefinido obrigatoriamente em todo compilador conforme enunciado mínimo

**4.** O operador `sizeof`, em C:

A) Libera memória alocada por `malloc`  
B) Inicia sessão HTTP  
C) É exclusivo de PHP  
D) Concatena strings com `.`  
E) Retorna o tamanho em bytes de um tipo ou expressão

**5.** A alocação dinâmica correta de um vetor de 10 `double` é:

A) `double[10]`  
B) `malloc(10 * sizeof(double) + 1)` como única forma válida  
C) `malloc(10 * sizeof(double))`  
D) `free(10 * sizeof(double))`  
E) `new double[10]` em C puro

**6.** Após uso de memória obtida com `malloc`, a liberação adequada é feita com:

A) `delete`  
B) `sizeof`  
C) `unset`  
D) `free`  
E) `session_destroy`

**7.** Em uma `struct`, é correto afirmar que:

A) Todos os campos devem ser do mesmo tipo obrigatoriamente  
B) Structs são proibidas fora de `main`  
C) `typedef` é ilegal junto de struct  
D) Struct equivale a `class` com herança múltipla em C puro  
E) Cada elemento é chamado membro/campo e pode haver tipos diferentes

**8.** O `typedef`, em visão de prova, serve para:

A) Criar um sinônimo (alias) de um tipo  
B) Alocar heap automaticamente  
C) Substituir o linker  
D) Declarar exclusivamente laços `for`  
E) Remover a necessidade de headers

**9.** Para que uma função altere a variável `int x` do chamador, o padrão em C é:

A) Passar `x` por valor e esperar alteração mágica  
B) Usar `try/catch`  
C) Passar o endereço (`&x`) e receber `int *` na função, escrevendo em `*p`  
D) Usar `$_POST`  
E) Declarar `x` como `class`

**10.** A aritmética `p + 1` sobre `int *p` avança, em regra:

A) Sempre exatamente 1 byte  
B) 1024 bytes fixos  
C) O tamanho de um frame PHP  
D) Nada; ponteiros não admitem soma  
E) `sizeof(int)` bytes (tamanho do tipo apontado)

**11.** Assinale a opção que **não** pertence ao C puro padrão de prova:

A) `struct`  
B) `malloc`  
C) `printf`  
D) `class` com herança `extends`  
E) `typedef`

**12.** Em `scanf("%d", &n);`, o `&` é necessário porque:

A) `scanf` só imprime  
B) `scanf` precisa do endereço para gravar o valor lido na variável  
C) `&` concatena strings  
D) `&` inicia o pré-processador  
E) `&` equivale a `free`

**13.** Relacionando alocação estática e dinâmica:

A) `double v[10];` é dinâmica com `malloc` implícito obrigatório  
B) `free` aloca novos bytes  
C) `malloc(10 * sizeof(double))` é alocação dinâmica em tempo de execução  
D) `sizeof` libera o vetor estático  
E) Toda variável local é alocada com `calloc` automaticamente

**14.** O acesso ao campo `codigo` via ponteiro `Aluno *p` é tipicamente escrito:

A) `p.codigo` apenas, nunca seta  
B) `p::codigo`  
C) `p=>codigo`  
D) `p->codigo`  
E) `$_POST["codigo"]`

**15.** A síntese mais adequada deste subtópico é:

A) Apenas LGPD artigos 1–6  
B) Apenas React hooks  
C) Apenas ITIL change enablement  
D) Apenas crase  
E) C: compilação, tipos, fluxo, funções, arrays, ponteiros, struct/typedef, sizeof/malloc/free, contraste com PHP e fronteira C≠C++

---

### Gabarito comentado (treino 1–15)

1. **C** — pré-processa, compila, linka, executa.  
2. **D** — só `*p` é ponteiro na declaração.  
3. **C** — `*p = 9` altera `a`.  
4. **E** — `sizeof` → bytes.  
5. **C** — `malloc(10 * sizeof(double))`.  
6. **D** — `free`.  
7. **E** — membros heterogêneos.  
8. **A** — alias de tipo.  
9. **C** — passagem por endereço.  
10. **E** — aritmética em unidades do tipo.  
11. **D** — `class`/`extends` não são C puro.  
12. **B** — `scanf` escreve no endereço.  
13. **C** — `malloc` = dinâmica.  
14. **D** — operador `->`.  
15. **E** — síntese do arquivo 03.

---

## 3. Questões estilo concurso FCC (16–25)

> Padrão FCC / analista TI: ponteiros, `sizeof`/`malloc`/`free`, structs, contraste de alocação; alinhadas a enunciados recorrentes em bancas (incl. FCC históricas de programação).

**16.** Referente à alocação dinâmica de memória em C, é correto afirmar que:

A) As funções `calloc` e `realloc` servem apenas para liberar arrays  
B) A função `free` é geralmente usada com o operador `sizeof` como par obrigatório  
C) A função `clear` é padrão para limpar ponteiros na libc  
D) `malloc` recebe o “número de blocos” sem relação com bytes  
E) As funções `malloc` e `free` e o operador `sizeof` são essenciais para a alocação dinâmica de memória

**17.** Assinale a alternativa que representa alocação dinâmica de um vetor de 10 `double`:

A) `double[10 * sizeof(double) - 1]`  
B) `double[10]`  
C) `malloc(10 * sizeof(double))`  
D) `new double[10]` em C puro ISO  
E) `free(10 * sizeof(double))`

**18.** Preencha as lacunas: a função/operador que determina o número de bytes de um tipo; a que aloca espaço e retorna ponteiro; a que libera o espaço são, respectivamente:

A) `sizeof` / `alloc` / `clear`  
B) `malloc` / `sizeof` / `free`  
C) `sizeof` / `malloc` / `free`  
D) `strlen` / `malloc` / `delete`  
E) `sizeof` / `new` / `dispose`

**19.** Acerca de ponteiros, considere:

```c
int a = 10, b = 20;
int *ponteiro = &a;
a = 10;
b = a + 20;
*ponteiro = b * 2;
printf("%d", a);
```

O valor impresso para `a` é:

A) 10  
B) 20  
C) 30  
D) 60  
E) 0

**20.** Com base no conceito de ponteiros, está correto:

A) `int x, y, z;` declara três ponteiros  
B) `int *x, y, z;` declara somente `x` como ponteiro  
C) `int *x, *y, *z;` declara três inteiros comuns, sem ponteiro  
D) `int x, *y, *z;` declara apenas `x` como ponteiro  
E) Nenhuma declaração com `*` é válida em C

**21.** Considere:

```c
struct cadastroDeAluno {
    char nome[40];
    char disciplina[20];
    float nota1;
    float nota2;
};
struct cadastroDeAluno aluno;
```

Assinale a afirmativa correta.

A) `cadastroDeAluno` é o nome de uma `union` obrigatória  
B) `aluno` é a variável que utiliza a struct  
C) Uma struct só pode conter variáveis do mesmo tipo  
D) A variável `aluno` é um membro interno da definição da struct  
E) Todas as alternativas anteriores estão corretas

**22.** O comando `typedef` é usado para criar:

A) Um sinônimo ou alias de tipo  
B) Exclusivamente uma variável automática  
C) Um novo laço de repetição  
D) Uma sessão PHP  
E) Um protocolo de rede

**23.** Considere as afirmativas sobre C.

I. Strings literais são tipicamente sequências de `char` terminadas em `\0`.  
II. A linguagem C puro padroniza `try`/`catch` como mecanismo principal de erro.  
III. `ptr->campo` acessa membro de struct via ponteiro.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e III, apenas  
D) II e III, apenas  
E) I, II e III

**24.** Constitui equívoco típico afirmar que:

A) `malloc` aloca bytes no heap  
B) `free` libera bloco previamente alocado  
C) `sizeof` mede tamanho em bytes  
D) ponteiros armazenam endereços  
E) em C puro, `new` e `delete` substituem obrigatoriamente `malloc` e `free` como única API válida

**25.** Para Analista Judiciário – TI (código 14), a síntese correta deste subtópico é:

A) apenas Bootstrap grid  
B) apenas Keycloak flows  
C) apenas direitos humanos — DUDH art. 1  
D) fundamentos de C (compilação, tipos, fluxo, ponteiros, struct/typedef, alocação dinâmica) em visão comparativa com PHP, sem importar C++  
E) apenas Kanban WIP limits

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **E** |
| 17 | **C** |
| 18 | **C** |
| 19 | **D** |
| 20 | **B** |
| 21 | **B** |
| 22 | **A** |
| 23 | **C** |
| 24 | **E** |
| 25 | **D** |

**Notas rápidas:** 16 E (trio clássico malloc/free/sizeof); 17 C (dinâmica 10 doubles); 18 C (sizeof/malloc/free); 19 D (`*ponteiro = 60` → `a=60`); 20 B (só `*x`); 21 B (variável `aluno`); 22 A (alias); 23 C (II falso — sem try/catch); 24 E (`new`/`delete` são C++); 25 D (síntese).

---

*Pack semana 6 · subtópico 03 · FCC TRT4 código 14 · visão C.*

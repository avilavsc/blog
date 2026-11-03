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

## 3. Questões de concurso (16–25)

> Questões reais de provas (FCC). Gabarito só ao final.

**16.** (FCC – TRT-17/2022 – Analista Judiciário – TI)  
Considere o programa em linguagem C abaixo, que será executado em condições ideais. #include <stdio.h> #include <conio.h> #define MAX 7 int vet[MAX] = {2, 5, 9, 14, 22, 31, 44}; int comeco, fim, meio, chave; void main(void) { I { printf("\nForneça o elemento a ser procurado (-1 finaliza): "); scanf("%d",&chave); if (chave == -1) II ; comeco = 0; fim = MAX-1; III { meio = (comeco + fim)/2; if (chave < vet[meio]) fim = meio -1; else comeco = meio +1; } while (vet[meio] != chave && comeco <= fim); if (vet[meio] == chave) printf("\nO elemento está na posição %d", meio); else printf("\nNão existe o elemento procurado no vetor"); } printf("\nFim do programa"); } Para que o programa funcione corretamente de acordo com a lógica prevista em um algoritmo de pesquisa em vetor ordenado, as lacunas I , II e III devem ser preenchidas, correta e respectivamente, por:

A) while (chave <> -1) – break repeat  
B) for (;;) – break do  
C) while (1)– continue - repeat  
D) while (true) – stop do  
E) for (int i=0; i<MAX; i++) – continue - do  

**17.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Um Técnico está codificando uma aplicação em C que manipula um array de inteiros e criou um ponteiro para o primeiro elemento do array . Ele deseja navegar pelos elementos subsequentes usando aritmética de ponteiros. O array contém os valores {20, 30, 40, 50, 60} . O Técnico declarou o ponteiro da seguinte maneira: int *ptr = &arr[0]; . Em seguida, ele incrementou o ponteiro utilizando a instrução ptr++; . Após o incremento, a instrução *ptr irá apontar para o valor

A) 40.  
B) 50.  
C) 30.  
D) 60.  
E) 20.  

**18.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Uma equipe de TI de uma empresa está desenvolvendo um sistema de controle de funcionários. Eles criaram uma classe base chamada `Funcionario`, que possui atributos como `nome`, `salario` e `cargo`. Além disso, criaram as subclasses `Gerente` e `Vendedor`, que utilizam os atributos de `Funcionario` e possuem atributos adicionais específicos. O conceito de orientação a objetos que está sendo aplicado quando as classes `Gerente` e `Vendedor` são definidas como subclasses de `Funcionario` é

A) Objeto.  
B) Polimorfismo.  
C) Herança.  
D) Encapsulamento.  
E) Interface.  

**19.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Considere o programa Java abaixo.

```
public class Verificar {

 public static int dados(int[] vetor, int inicio, int fim, int chave) {
 if (inicio > fim) {
 return -1;
 }
 int meio = (inicio + fim) / 2;
 if (vetor[meio] == chave) {
 return meio;
 }
 if (vetor[inicio] <= vetor[meio]) {
 if (chave >= vetor[inicio] && chave < vetor[meio]) {
 return dados(vetor, inicio, meio - 1, chave);
 } else {
 return dados(vetor, meio + 1, fim, chave);
 }
 } else {
 if (chave > vetor[meio] && chave <= vetor[fim]) {
 return dados(vetor, meio + 1, fim, chave);
 } else {
 return dados(vetor, inicio, meio - 1, chave);
 }
 }
 }

 public static void main(String[] args) {
 int[] vetor = {7, 8, 9, 1, 2, 3, 4, 5, 6};
 int resultado = dados(vetor, 0, 8, 5);
 System.out.println(resultado);
 }
}

```

 Ao executar o programa, em condições ideais, será exibido na tela o valor

A) 7.  
B) 6.  
C) 8.  
D) 3.  
E) 5.  

**20.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Um Técnico está processando grandes volumes de dados e precisa verificar se há valores duplicados em uma coleção de milhares de elementos. A ordem dos elementos não é importante, mas o processamento precisa ser o mais eficiente possível para identificar e eliminar quaisquer duplicatas. A estrutura de dados que deve ser usada em Python, nesse caso, é

A) Pilhas (`stack`).  
B) Listas (`list`).  
C) Tuplas (`tuple`).  
D) Conjuntos (`set`).  
E) Dicionários (`dict`).  

**21.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Um desenvolvedor PHP 8 quer garantir que a variável $u receba o valor da variável $_GET['user'] se ela existir e não for nula; caso contrário, a variável $u receberá o valor 'convidado' . Para isso ele deve usar o comando

A) $u = $_GET['user'] ?? $_GET['user'] :: 'convidado';  
B) $u = $_GET['user'] ?? 'convidado';  
C) $u = $_GET['user'] => isset($_GET['user']) ? $_GET['user'] : 'convidado';  
D) $u = if($_GET['user'] => isset($_GET['user'])) ? $_GET['user'] : 'convidado';  
E) $u = if($_GET['user']) ? $_GET['user'] : 'convidado';  

**22.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Considere que um método de ordenação tenha seu desempenho baseado no número de comparações que realiza para ordenar um vetor com N elementos em ordem crescente. Este método apresenta o seguinte resultado no melhor caso (NCmelhor), no caso médio (NCmédio) e no pior caso (NCpior):

NCmelhor = N-1
NCmédio ≅ (N (N-1))/4 - 1/2
NCpior ≅ (N (N-1)-1)/2

Com base nestes resultados, é correto afirmar que o método

A) é sempre ineficiente, mesmo para valores pequenos de N.  
B) é de ordem de complexidade quadrática ou O(N²).  
C) apresenta um desempenho muito eficiente no caso médio, quando o vetor está em ordem decrescente, por exemplo.  
D) é de ordem de complexidade linearítmica ou O(N-1) no melhor caso, quando o vetor está parcialmente ordenado.  
E) sempre realiza um número muito grande de trocas em todos os três casos, por isso é muito ineficiente.  

**23.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Considere os dados abaixo relativos a um método hashing .

 

Com base no método apresentado acima e na sua continuação, é correto afirmar que

A) não há colisões.  
B) Jorge ocupará a posição 7 do vetor e Fernando será deslocado para a posição 8.  
C) serão realizadas, na pesquisa por Adriano, apenas 2 comparações para encontrá-lo.  
D) ocorreu uma pseudocolisão entre Adriano e Luísa.  
E) Pedro ocupará a posição 4 do vetor.  

**24.** (FCC – TRT-19/2022 – Analista Judiciário – TI)  
Considere a Árvore Binária de Pesquisa (ABP) abaixo.

 

 Os elementos desta ABP serão armazenados em um vetor numerado de 1 a 7, seguindo estas regras:

a. a raiz sempre ficará na posição 1 do vetor;

b. um nó filho na subárvore esquerda receberá o dobro do valor da posição do seu pai; e

c. um nó filho na subárvore direita receberá o dobro do valor da posição de seu pai acrescido de 1.

O vetor resultante de 1 a 7 será, correta e respectivamente:

A) 6 – 8 – 4 – 5 – 3 – 7 – 9  
B) 6 – 4 – 3 – 5 – 8 – 7 – 9  
C) 6 – 4 – 3 – 8 – 7 – 5 – 9  
D) 6 – 8 – 9 – 4 – 5 – 3 – 7  
E) 6 – 4 – 8 – 3 – 5 – 7 – 9  

**25.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
Examine os programas em PHP, C# e C a seguir:

```
<?php
$num = "5oH";
$result = $num + 5;
echo "Result: " . $result;
?>

```

```
using System;
namespace Teste {
 class Testando {
 static void Main(string[] args) {
 int num = "5oH";
 int result = num + 5;
 Console.WriteLine($"Resultado: {r}");
 }
 }
}

```

```
#include <stdio.h>
int main(){
 int num, result;
 num= "5oH";
 result = num + 5;
 printf("\n Resultado: %d",
 result);
 return 0;
}

```

 Considerando que os programas serão executados em condições ideais,

A) os programas em PHP e em C não apresentarão erros, mesmo sem o uso explícito de cast.  
B) todos os programas executarão sem erros de compilação.  
C) os resultados apresentados pelos programas em C# e em C serão iguais, pois C# é compilada por um compilador escrito em C.  
D) todos os programas apresentarão erros, pois não é possível atribuir strings a variáveis numéricas.  
E) todos os programas deixarão de apresentar erros, caso seja colocado `(int)` antes de "5oH".  

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **B** | FCC TRT-17/2022 Q33 — Programação |
| 17 | **C** | FCC TRT-20/2024 Q44 — Programação |
| 18 | **C** | FCC TRT-20/2024 Q41 — Programação |
| 19 | **A** | FCC TRT-20/2024 Q42 — Programação |
| 20 | **D** | FCC TRT-20/2024 Q43 — Programação |
| 21 | **B** | FCC TRT-20/2024 Q47 — Programação |
| 22 | **B** | FCC TRT-19/2022 Q23 — Estruturas de Dados |
| 23 | **E** | FCC TRT-19/2022 Q24 — Estruturas de Dados |
| 24 | **E** | FCC TRT-19/2022 Q25 — Estruturas de Dados |
| 25 | **A** | FCC TRT-12/2023 Q42 — Fundamentos de Linguagens de Programação |

**Notas rápidas:** 16 B; 17 C; 18 C; 19 A; 20 D; 21 B; 22 B; 23 E; 24 E; 25 A.

+++
title = '03 — Python fundamentos'
date = '2026-10-17'
draft = false
weight = 3
tags = ['semana-05', 'python', 'fundamentos']
categories = ['estudo']
+++
# Python fundamentos · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 03 — Python (fundamentos, OOP, exceções, BD)  
**Escopo deste arquivo:** Sintaxe e construções básicas, tipagem dinâmica, tipos/coleções, operadores e fluxo, funções, módulos/pacotes, OOP (classes, herança, encapsulamento por convenção), exceções, bibliotecas/componentes e acesso a banco — alinhado ao bloco de Programação do edital (e menção a Python no eixo web).  
**Fora de escopo aqui:** Java (arquivo 01); C#/.NET (arquivo 02); frameworks web JS; Django/Flask em profundidade (só visão se couber em “bibliotecas”); ciência de dados avançada.

---

## 1. Material

### 1.1 Por que isso cai no edital

No eixo **Programação**, o edital lista **Python** entre as linguagens de conhecimento profundo, com os mesmos fundamentos cobráveis: sintaxe, palavras-chave, estrutura e construções básicas, compilação/execução (aqui: interpretação + bytecode), tipos, variáveis, literais e strings, operadores e precedência, controle de fluxo e repetição, classes/interfaces(protocolos)/métodos, encapsulamento, packages, sobrecarga e sobrescrita (com nuances da linguagem), bibliotecas e componentes, **exceções** e **acesso a banco de dados**.

Python também aparece no eixo de **desenvolvimento web** (ao lado de frameworks JS etc.) — em prova, cobram sobretudo **fundamentos da linguagem**, não um framework específico.

Leitura cobrável:

| Peça | Papel em prova |
|------|----------------|
| **CPython / interpretador** | Execução; `.py` → bytecode `.pyc` → VM |
| **Tipagem dinâmica** | Tipos ligados aos **objetos**, não às variáveis |
| **Coleções** | `list`, `tuple`, `dict`, `set` |
| **OOP** | `class`, herança, MRO, dunder methods |
| **Exceções** | `try`/`except`/`else`/`finally`, `raise` |
| **BD** | DB-API 2.0 (`sqlite3`, drivers); ORMs (SQLAlchemy — visão) |

Flash: FCC compara Python com Java/C# — saber o que Python **não** tem (interfaces formais clássicas, modificadores `private` de verdade, sobrecarga estática de métodos) evita pegadinha.

### 1.2 Execução e modelo mental

| Conceito | Ideia cobrável |
|----------|----------------|
| **Interpretada** (visão didática) | Código-fonte executado via interpretador (CPython) |
| **Bytecode** | Compilação sob demanda para `.pyc`, executado na VM |
| **GIL** (avançado) | Global Interpreter Lock — limita threads CPU-bound em CPython (raramente cobrado em detalhe) |
| **REPL** | Modo interativo para experimentação |
| **PEP 8** | Guia de estilo (indentação 4 espaços — **indentação é sintaxe**) |

Indentação delimita blocos (`if`, `for`, `def`, `class`). Esquecer indentação = `IndentationError`.

Shebang e módulos: `python app.py`; entrada típica `if __name__ == "__main__":`.

### 1.3 Tipos, variáveis e literais

Variáveis são **nomes** ligados a objetos. Não se declara tipo obrigatório (anotações `x: int` existem, mas são opcionais e não enforced em runtime pelo interpretador padrão).

Tipos built-in cobráveis:

| Categoria | Exemplos |
|-----------|----------|
| Numéricos | `int` (precisão arbitrária), `float`, `complex` |
| Texto | `str` (Unicode, imutável) |
| Booleanos | `bool` (`True`/`False`) — subclasse de `int` |
| Nulo | `None` |
| Sequências | `list` (mutável), `tuple` (imutável), `range` |
| Conjuntos | `set`, `frozenset` |
| Mapeamento | `dict` (pares chave→valor) |

Literais: `42`, `3.14`, `"texto"`, `'texto'`, `f"olá {nome}"` (f-string), `True`/`False`/`None`, `[1,2]`, `(1,2)`, `{1,2}`, `{"a": 1}`.

**Mutável × imutável (prova):**  
- Imutáveis: `int`, `float`, `str`, `tuple`, `frozenset`…  
- Mutáveis: `list`, `dict`, `set`…  

Armadilha clássica: usar objeto mutável como *default* de parâmetro (`def f(a=[])`) — compartilhado entre chamadas.

### 1.4 Operadores, fluxo e compreensão

Operadores: aritméticos `+ - * / // % **`; comparação `== != < > <= >=`; identidade `is` / `is not` (mesmo objeto); pertencimento `in` / `not in`; lógicos `and` `or` `not` (curto-circuito); bit a bit `& | ^ ~ << >>`.

Precedência (amostra mental): `**` alto; depois unários; `* / // %`; `+ -`; shifts; comparações; `not`; `and`; `or`.

Controle de fluxo:

```python
if condicao:
    ...
elif outra:
    ...
else:
    ...

for item in colecao:
    ...
else:  # executa se não houve break
    ...

while condicao:
    ...

match valor:  # Python 3.10+ structural pattern matching
    case 1:
        ...
```

`break` / `continue` / `pass`.  
**List/dict/set comprehensions:** `[x*2 for x in xs if x > 0]` — muito “pythonico” e cobrável como construção idiomática.

### 1.5 Funções, escopo e módulos

```python
def soma(a, b=0, *args, **kwargs):
    return a + b + sum(args)
```

| Ideia | Detalhe cobrável |
|-------|------------------|
| Argumentos | posicionais, default, `*args`, `**kwargs`, keyword-only |
| Retorno | `return` (tupla implícita para múltiplos valores) |
| Escopo | LEGB: Local → Enclosing → Global → Built-in |
| `lambda` | função anônima de uma expressão |
| Geradores | `yield` — produção lazy |

**Módulos e pacotes:** `import math`, `from math import sqrt`, `import pacote.modulo as m`. Pacote = diretório com estrutura de módulos (`__init__.py` em pacotes tradicionais).  
`__name__`, `__doc__`, caminho em `sys.path`.

Não há “overload” de funções por assinatura como em Java/C#: a definição **posterior** sobrescreve o nome. “Sobrecarga” em Python costuma ser simulada com defaults/`*args` ou `functools.singledispatch`.

### 1.6 OOP em Python

```python
class Processo:
    foro = "TRT4"  # atributo de classe

    def __init__(self, numero: str):
        self._numero = numero  # convenção "protegido"

    @property
    def numero(self):
        return self._numero

    def resumo(self):
        return f"Processo {self._numero}"

class ProcessoRecurso(Processo):
    def resumo(self):  # sobrescrita
        return f"Recurso {self._numero}"
```

Pontos cobráveis:

| Tema | Visão de prova |
|------|----------------|
| Tudo é objeto | Inclusive classes e funções |
| `__init__` | Inicializador (não é construtor de baixo nível como `new`) |
| `self` | Convenção para a instância (nome não é palavra-chave, mas é idiomático) |
| Herança | `class B(A):`; herança múltipla permitida |
| MRO | Method Resolution Order (C3) — ordem de busca de métodos |
| Sobrescrita | Redefinir método na subclasse; `super()` chama a base |
| Encapsulamento | Por **convenção**: `_interno`; *name mangling* `__attr` → `_Classe__attr` (não é private real) |
| `@property` | Atributo gerenciado (getter/setter) |
| `@staticmethod` / `@classmethod` | Métodos sem instância / com a classe (`cls`) |
| Dunder | `__str__`, `__repr__`, `__eq__`, `__len__`… |

**Interfaces:** Python usa **duck typing** (“se anda como pato…”) e, em tipagem estrutural moderna, `typing.Protocol`. ABC (`abc.ABC`, `@abstractmethod`) formaliza classes abstratas.

Contraste com C#/Java (revisão semana 5):

| | Python | C# / Java |
|---|--------|-----------|
| Tipagem | Dinâmica (anotações opcionais) | Estática |
| Visibilidade | Convenção `_` / mangling | `private`/`protected` reais |
| Herança múltipla | Classes: sim | Só interfaces (C#/Java) |
| Override | Por redefinição de nome | `override` / anotações |
| Overload estático | Não nativo | Sim |

### 1.7 Exceções

Hierarquia mental: `BaseException` → `Exception` → específicas (`ValueError`, `TypeError`, `KeyError`, `IndexError`, `OSError`, `RuntimeError`…).  
`SystemExit`, `KeyboardInterrupt` herdam de `BaseException` (não de `Exception`) — por isso `except Exception` não as engole.

```python
try:
    validar(id)
except ValueError as e:
    ...
except Exception:
    ...
else:
    # se não houve exceção
    ...
finally:
    # sempre
    ...
```

`raise ValueError("msg")` · `raise` sozinho relança no `except`.  
Context managers: `with open(path) as f:` garantem cleanup (`__enter__`/`__exit__`) — análogo conceitual a `using`/`try-with-resources`.

### 1.8 Bibliotecas e componentes (amostra de prova)

Biblioteca padrão (amostra): `os`, `sys`, `pathlib`, `json`, `datetime`, `re`, `collections`, `itertools`, `functools`, `urllib`, `http`, `sqlite3`, `logging`, `unittest`.  
Ecossistema: `pip` + PyPI; ambientes virtuais (`venv`).  
Web (visão): frameworks como Django/Flask/FastAPI — saber que **existem** e que Python serve back-end; detalhe de framework raramente é o núcleo do edital TRT4 para este item.

### 1.9 Acesso a banco de dados

Python segue a **DB-API 2.0** (PEP 249):

| Peça | Papel |
|------|-------|
| `connect(...)` | Abre conexão |
| `cursor()` | Executa SQL |
| `execute` / `executemany` | Comandos (usar **placeholders**, não concatenar) |
| `fetchone` / `fetchall` | Resultados |
| `commit` / `rollback` | Transação |
| `close` | Encerrar |

Exemplo mental com `sqlite3` (stdlib):

```python
import sqlite3
with sqlite3.connect("app.db") as conn:
    cur = conn.cursor()
    cur.execute("SELECT id FROM processo WHERE numero = ?", (numero,))
    row = cur.fetchone()
```

Drivers: `psycopg`/`psycopg2` (PostgreSQL), etc.  
ORMs: **SQLAlchemy**, Django ORM — mapeiam classes ↔ tabelas (visão análoga ao EF Core / JPA).

Flash: **parametrizar** queries (`?` ou `%s` conforme driver) = anti SQL injection.

### 1.10 Mapa mental rápido (revisão dirigida)

1. Indentação = sintaxe; tipagem dinâmica.  
2. `list`/`tuple`/`dict`/`set`; mutável × imutável.  
3. `def`, LEGB, módulos/`import`.  
4. `class`, herança (múltipla), `super()`, `_` por convenção.  
5. Sobrescrita sim; overload estático não.  
6. `try`/`except`/`else`/`finally` + `with`.  
7. DB-API 2.0 + placeholders; ORM opcional.  
8. Contraste consciente com Java/C#.

---

## 2. Questões de treino (1–15)

**1.** Em Python, a indentação:

A) Delimita blocos sintáticos (funções, classes, condicionais); indentação incorreta gera erro  
B) É puramente cosmética e nunca afeta a execução  
C) Só existe em arquivos `.cs`  
D) Substitui o interpretador  
E) É proibida pelo PEP 8  

**2.** Acerca de tipagem em Python (CPython padrão), é correto afirmar que:

A) tipos estão associados aos objetos; variáveis são nomes; anotações de tipo são opcionais e não enforced pelo interpretador padrão  
B) toda variável exige declaração estática obrigatória como em C  
C) `None` é ilegal  
D) `str` é sempre mutável  
E) não existem inteiros  

**3.** A coleção mutável que armazena pares chave→valor é:

A) `dict`  
B) `tuple`  
C) `frozenset`  
D) Exclusivamente `range`  
E) `complex`  

**4.** A diferença prática mais cobrada entre `list` e `tuple` é:

A) `list` é mutável; `tuple` é imutável  
B) `tuple` só aceita strings  
C) `list` não pode ser iterada  
D) São idênticas em mutabilidade  
E) `tuple` exige compilação CLR  

**5.** Em definição de funções, usar uma lista vazia como valor default de parâmetro:

A) É armadilha clássica: o objeto default é criado uma vez e compartilhado entre chamadas  
B) É obrigatório em toda função  
C) Impede o uso de `return`  
D) Converte a função em classe automaticamente  
E) Só funciona dentro do CLR  

**6.** O escopo de nomes LEGB refere-se a:

A) Local, Enclosing, Global, Built-in  
B) LDAP, Eureka, Gateway, Bootstrap  
C) List, Enum, Graph, Binary  
D) Linux, EFI, GRUB, BIOS  
E) Lead, Epic, Goal, Backlog  

**7.** Em OOP Python, encapsulamento “privado” absoluto como em Java/C#:

A) Não existe da mesma forma; usa-se convenção `_nome` e name mangling com `__nome`  
B) É feito com a palavra-chave `private` obrigatória  
C) É feito apenas com `internal`  
D) Impede herança múltipla por lei da linguagem  
E) Exige `@Override`  

**8.** Herança múltipla de classes em Python:

A) É permitida; a ordem de resolução de métodos segue o MRO (C3)  
B) É proibida em qualquer versão  
C) Só existe via CLR  
D) Substitui o interpretador por JVM obrigatória  
E) Elimina `super()`  

**9.** Sobrescrita de método em Python ocorre quando:

A) A subclasse redefine um método de mesmo nome; `super()` pode delegar à implementação da base  
B) O interpretador cria `virtual`/`override` automaticamente como palavras-chave obrigatórias  
C) Só via anotação `@Override` do Java  
D) É impossível por tipagem dinâmica  
E) Depende exclusivamente de ASP.NET Core  

**10.** No tratamento de exceções, o bloco `else` ligado ao `try`:

A) Executa se nenhuma exceção ocorreu no `try`  
B) Executa somente após `SystemExit`  
C) Substitui o `finally`  
D) É obrigatório em todo `except`  
E) Compila o código para IL  

**11.** A instrução adequada para garantir fechamento de arquivo mesmo com erro, de forma idiomática, é:

A) `with open(caminho) as f:` (context manager)  
B) Abrir o arquivo e nunca fechá-lo  
C) Usar apenas `goto`  
D) `Environment.Exit` do .NET  
E) `final` do Java sem `with`  

**12.** Segundo a DB-API 2.0, a prática correta ao montar SQL com filtro externo é:

A) Usar placeholders/parâmetros do driver em vez de concatenar a entrada na string SQL  
B) Concatenar sempre a entrada do usuário na query  
C) Evitar `commit` em qualquer escrita  
D) Usar somente FTP para SELECT  
E) Ignorar o cursor  

**13.** Em relação a módulos, `if __name__ == "__main__":` serve tipicamente para:

A) Executar código apenas quando o arquivo é o programa principal, não quando importado como módulo  
B) Impedir qualquer importação  
C) Ativar o CLR  
D) Declarar package Java  
E) Criar um assembly `internal`  

**14.** Duck typing em Python significa, em visão de prova, que:

A) A adequação de um objeto depende do comportamento (métodos/atributos), não de uma hierarquia de tipos rígida prévia  
B) Todo objeto deve herdar de `Duck`  
C) Tipagem estática é obrigatória  
D) Interfaces `private` são exigidas  
E) O interpretador rejeita qualquer polimorfismo  

**15.** A síntese mais adequada deste subtópico no edital é:

A) Fundamentos Python (sintaxe, tipos, fluxo, funções, pacotes) + OOP + exceções + bibliotecas + acesso a BD (DB-API/ORM)  
B) Apenas redes Wi-Fi  
C) Apenas crase  
D) Apenas ITIL 5  
E) Apenas cabeamento  

---

### Gabarito comentado (treino 1–15)

1. **A** — indentação é sintaxe.  
2. **A** — tipagem dinâmica; anotações opcionais.  
3. **A** — `dict`.  
4. **A** — list mutável × tuple imutável.  
5. **A** — default mutável compartilhado.  
6. **A** — LEGB.  
7. **A** — convenção `_` / mangling, sem `private` real.  
8. **A** — herança múltipla + MRO.  
9. **A** — redefinição + `super()`.  
10. **A** — `else` do `try` = sucesso sem exceção.  
11. **A** — `with` / context manager.  
12. **A** — SQL parametrizado.  
13. **A** — guarda de execução principal.  
14. **A** — duck typing = comportamento.  
15. **A** — bloco completo do edital para Python.

---

## 3. Questões de concurso (16–25)

**16.** (FCC – TRT-19/2022 – Analista Judiciário – TI) Considere o código Python abaixo.

```python
import json
x = '{ "nome":"Paulo", "idade":30, "cidade":"São Paulo"}'
```

Para converter o conteúdo de x para JSON e exibir a cidade devem-se utilizar, na sequência do código, os comandos

A) `y = json.loads(x)` / `print(y["cidade"])`
B) `y = json.read(x)` / `print(y["cidade"])`
C) `y = json.loads(x)` / `print[y("cidade")]`
D) `y = json.read(x)` / `print(y => "cidade")`
E) `y = json.convert(x)` / `print(y{"cidade"})`

**17.** (FCC – TRT-6/2025 – Analista Judiciário – TI) Em um programa Python, se o conteúdo da variável `nome` estiver presente em uma lista contendo "Alice" ou "Clara" deve ser exibida "Bem-vinda", senão "Olá". A instrução correta é:

A) `out.println(new List {"Alice", "Clara"}.Contains(nome) ? "Bem-vinda" : "Olá");`
B) `out.print(Arrays.asList("Alice", "Clara").contains(nome) ? "Bem-vinda" : "Olá");`
C) `print(nome in ["Alice", "Clara"] ? "Bem-vinda" : "Olá")`
D) `echo ($nome == "Alice" || $nome == "Clara") ? "Bem-vinda" : "Olá";`
E) `print("Bem-vinda" if nome in ["Alice", "Clara"] else "Olá")`

**18.** (FCC – TRT-13/2014 – Analista Judiciário – TI) Considere o código abaixo, digitado em Python.

```python
a, b = 0, 1
while b < 10:
  print b
  a, b = b, a+b
```

No final da execução, o último valor armazenado nas variáveis `a` e `b` serão, respectivamente,

A) 13 e 21.
B) 8 e 13.
C) 5 e 10.
D) 13 e 20.
E) 8 e 10.

**19.** (FCC – TRF-4/2019 – Analista Judiciário – Infraestrutura de TI) Considerando que em um programa Python há um array criado por `nomes = ["Maria", "Pedro", "João"]`, para exibir os valores contidos nesse array utiliza-se

A) `for x in nomes: out.print(x)`
B) `while x in nomes: print(x)`
C) `foreach x in nomes: print(x)`
D) `foreach x in nomes: system.println(x)`
E) `for x in nomes: print(x)`

**20.** (FCC – TJ-MA/2019 – Técnico Judiciário – Informática) Considere o programa Python abaixo:

```python
..I..
for x in pro:
  print(x)

processos = ["0456789908", "0875643087", "0897645109"]
exibir_processos(processos)
```

Para que o programa seja executado corretamente, a indicação I deve ser substituída por:

A) `private exibir_processos(pro):`
B) `public exibir_processos(pro):`
C) `function exibir_processos(pro):`
D) `definition exibir_processos(pro):`
E) `def exibir_processos(pro):`

**21.** (FCC – TJ-CE/2022 – Analista Judiciário – Sistemas da Informação) Observe trechos de código Python com método `print_tempo` cujo primeiro parâmetro é `tempo`. É correto afirmar:

A) Em I e em II, se for executado `>>> print_tempo(t1)` o resultado será 11:15:00
B) O Código 2 pode ser reescrito de maneira equivalente usando `def print_tempo(self):` e `self.hour` / `self.minute` / `self.second`
C) Em II, se for executado `>>> print_tempo(t1)` a função apresentará 11:15:00
D) Em I, se for executado `>>> Tempo.print_tempo(t1)` o método apresentará 11:15:00 (como única afirmativa correta da questão)
E) Em II, se for executado `>>> t1.print_tempo()` o resultado apresentará erro.

**22.** (FCC – TRT-6/2025 – Analista Judiciário – TI) Considere o método:

```python
def buscar_caso(self, numero_processo):
  return self.casos.get(numero_processo, "Caso não encontrado.")
```

Para testar esse método com pytest, pode ser usada a instrução:

A) `assert tribunal.buscar_caso(1) == "Caso encontrado"`
B) `assert tribunal.buscar_caso("999") == "Caso não encontrado"`
C) `assert tribunal.buscar_caso(0, "9999") == "Caso não encontrado"`
D) `pytest tribunal.buscar_caso("1") == "Caso Encontrado"`
E) `call tribunal.buscar_caso("0") == "Caso não encontrado"`

**23.** (FCC – TRT-15/2024 – Analista Judiciário – TI)  
Com `filtro = df['status'] == 'Pendente'`, a média ponderada Σ(valor×peso)/Σ(peso) preenche a lacuna com

A) `(df[filtro]['valor_processo'] * df['peso']).average.weighted()`  
B) `df[filtro][['valor_processo'].sum() * df['peso'].sum()] / 'peso'.sum()`  
C) `(df[filtro]['valor_processo'] / df[filtro]['peso']).mean()`  
D) `df[filtro]['valor_processo'].apply(lambda x: x.mean())`  
E) `(df[filtro]['valor_processo'] * df[filtro]['peso']).sum() / df[filtro]['peso'].sum()`

**24.** (FCC – TRT-20/2024 – Técnico Judiciário – TI)  
Para identificar e eliminar duplicatas com eficiência (ordem irrelevante) em grande volume, a estrutura Python adequada é

A) Pilhas (`stack`)  
B) Listas (`list`)  
C) Tuplas (`tuple`)  
D) Conjuntos (`set`)  
E) Dicionários (`dict`)

**25.** (FCC – TRF-4/2025 – Técnico Judiciário – Desenvolvimento de Sistemas)  
Em `return df.groupby("juiz")["tempo_tramitacao"].__I__().to_dict()`, completa a lacuna I o método

A) `avg`  
B) `median`  
C) `average`  
D) `midpoint`  
E) `mean`

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **A** | FCC TRT-19/2022 — `json.loads` |
| 17 | **E** | FCC TRT-6/2025 — ternário Python |
| 18 | **B** | FCC TRT-13/2014 — Fibonacci a,b → 8 e 13 |
| 19 | **E** | FCC TRF-4/2019 — `for x in nomes: print(x)` |
| 20 | **E** | FCC TJ-MA/2019 — `def` |
| 21 | **B** | FCC TJ-CE/2022 — `self` em método |
| 22 | **B** | FCC TRT-6/2025 — assert pytest / `.get` |
| 23 | **E** | FCC TRT-15/2024 — média ponderada pandas |
| 24 | **D** | FCC TRT-20/2024 Téc. — `set` |
| 25 | **E** | FCC TRF-4/2025 — `groupby().mean()` |

**Notas:** 10/10 reais e citadas (FCC). 16–22 núcleo linguagem; 23–25 pandas/coleções alinhados ao edital.

---

*Arquivo 03 · Python fundamentos · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: blocos web da semana 5 (HTML/CSS/JS…).*

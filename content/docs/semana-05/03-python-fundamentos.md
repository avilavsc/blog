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

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto da linguagem Python referida no edital de Analista Judiciário – Área de TI, é correto afirmar que:

A) trata-se de linguagem de tipagem dinâmica, com blocos delimitados por indentação, execução típica via interpretador (ex.: CPython) e amplo uso de módulos da biblioteca padrão  
B) exige declaração estática obrigatória de todos os tipos em tempo de compilação, como regra do interpretador padrão  
C) proíbe o uso de dicionários e listas  
D) coincide termo a termo com C# quanto a modificadores `private`/`protected`  
E) não possui mecanismo de tratamento de exceções  

**17.** Acerca de estruturas de dados built-in em Python, assinale a opção correta.

A) `list` é sequência mutável; `tuple` é sequência imutável; `dict` mapeia chaves a valores; `set` armazena elementos únicos e não ordenados de forma clássica  
B) `tuple` é sempre mutável e `list` sempre imutável  
C) `dict` não admite iteração  
D) `set` é idêntico semanticamente a `str`  
E) não existem literais para essas estruturas  

**18.** Em orientação a objetos em Python, é correto afirmar que:

A) classes suportam herança (inclusive múltipla), inicialização via `__init__`, sobrescrita de métodos e encapsulamento por convenção (`_` / name mangling), com resolução de métodos guiada pelo MRO  
B) herança múltipla de classes é proibida em qualquer circunstância  
C) `self` é palavra-chave reservada obrigatória pelo interpretador em qualquer nome de parâmetro  
D) `@property` é ilegal  
E) `super()` existe apenas no ecossistema .NET  

**19.** Considere as afirmativas abaixo.

I. O bloco `finally` em um `try` executa independentemente de ter ocorrido exceção.  
II. `except Exception` captura também, em regra, `KeyboardInterrupt` e `SystemExit`, pois estes herdam diretamente de `Exception`.  
III. O uso de `with` adequa-se à liberação determinística de recursos (context manager).

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e III, apenas  
D) II e III, apenas  
E) I, II e III  

**20.** No acesso a banco de dados a partir de código Python alinhado à DB-API 2.0, a prática mais adequada é:

A) obter conexão, usar cursor com SQL parametrizado (placeholders), tratar transação (`commit`/`rollback`) e fechar recursos, podendo empregar ORMs (ex.: SQLAlchemy) como abstração  
B) concatenar entrada externa diretamente na string SQL  
C) dispensar qualquer cursor e enviar SQL via SMTP  
D) manter transações sempre abertas sem commit em sistemas OLTP  
E) eliminar drivers e acessar arquivos `.mdb` apenas por FTP  

**21.** A respeito de funções e módulos em Python, é correto afirmar que:

A) funções são definidas com `def`, admitem parâmetros default/`*args`/`**kwargs`, e módulos são importados com `import`; o teste `if __name__ == "__main__"` distingue script principal de importação  
B) não é possível retornar múltiplos valores  
C) `lambda` permite blocos indentados arbitrários com vários `return`  
D) pacotes são proibidos pela PEP 8  
E) `import` só funciona dentro de classes `sealed`  

**22.** Constitui equívoco típico em prova afirmar que:

A) indentação delimita blocos em Python  
B) `dict` armazena pares chave-valor  
C) exceções usam `try`/`except`/`finally`  
D) SQL deve ser parametrizado na DB-API  
E) Python oferece sobrecarga estática de métodos por assinatura no mesmo sentido clássico de Java/C#, redefinindo o nome da função a cada nova definição no mesmo escopo  

**23.** Em um microsserviço que expõe API HTTP e consulta PostgreSQL usando Python, a leitura mais adequada para concurso é:

A) a linguagem oferece tipagem dinâmica, OOP e bibliotecas; o acesso ao banco segue DB-API/driver (ou ORM); a camada web pode usar frameworks (Flask/Django/FastAPI), sem confundir isso com o CLR/.NET  
B) o GIL substitui o banco relacional  
C) `tuple` é o protocolo HTTP/2  
D) name mangling equivale a JWT  
E) `match/case` é obrigatório em toda query SQL  

**24.** Acerca de mutabilidade e parâmetros default, assinale a opção correta.

A) objetos mutáveis como valor default de parâmetro são criados uma vez na definição da função e podem acumular estado indesejado entre chamadas — prática a evitar  
B) defaults mutáveis são recriados a cada chamada automaticamente pelo interpretador em qualquer caso  
C) apenas `tuple` pode ser default  
D) o problema só ocorre em C#  
E) PEP 8 obriga defaults mutáveis em toda API pública  

**25.** Para fins de preparação em Analista Judiciário – Área de TI (código 14), a síntese mais adequada deste subtópico é:

A) sintaxe e tipos Python; fluxo e funções; módulos/pacotes; OOP (classes, herança, sobrescrita, encapsulamento por convenção); exceções e `with`; bibliotecas; acesso a BD via DB-API/ORM  
B) apenas COBIT 2019  
C) apenas direitos humanos  
D) apenas Bootstrap 5 sem lógica  
E) apenas Delphi VCL  

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

**Notas rápidas:** 16 A (dinâmica + indentação + interpretador); 17 A (list/tuple/dict/set); 18 A (OOP + MRO + convenção); 19 C (I e III; II errada — KeyboardInterrupt/SystemExit ≠ Exception); 20 A (DB-API parametrizada); 21 A (def/import/__main__); 22 E (sem overload estático clássico); 23 A (Python + BD + web); 24 A (default mutável); 25 A (síntese do arquivo 03).

---

*Arquivo 03 · Python fundamentos · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: blocos web da semana 5 (HTML/CSS/JS…).*

---
name: organizar-semana
description: >-
  Organiza pastas de estudo semana-NN no Hugo/Hextra (content/docs): adiciona
  front matter, cria _index.md, remove README.md e atualiza a lista em
  docs/_index.md. Use when the user asks to organize a week (semana 01/02/03…),
  cola materiais em content/docs/semana-NN, or mentions organizar semana.
---

# Organizar semana de estudo

Site Hugo + Hextra em `blog/`. Materiais ficam em `content/docs/semana-NN/`.

## Quando rodar

Usuário colou/criou arquivos em `content/docs/semana-NN/` (com ou sem `README.md`) e pediu para organizar.

## Checklist

```
- [ ] Localizar pasta content/docs/semana-NN/
- [ ] Front matter em cada NN-*.md
- [ ] Criar _index.md da semana
- [ ] Remover README.md
- [ ] Atualizar content/docs/_index.md (tabela de semanas)
- [ ] Validar com hugo (página em public/docs/semana-NN/)
- [ ] Se pedido: commit + push
```

## Front matter (obrigatório)

Usar TOML (`+++`). Em cada `NN-tema.md`:

```toml
+++
title = 'NN — Título curto'
date = 'YYYY-MM-DD'
draft = false
weight = N
tags = ['semana-NN', 'tag-tema']
categories = ['estudo']
+++
```

Regras de `date`:
- **Nunca data futura** — Hugo não publica e a URL dá 404.
- Preferir datas ≤ hoje (ou no passado recente).
- `weight` = número do arquivo (1, 2, 3…).

Título: ler o `#` do markdown e enxugar (ex.: `# Docker · TRT4…` → `02 — Docker`).

Se o arquivo já tiver `+++`, não duplicar.

## `_index.md` da semana

Substituir o `README.md` por `content/docs/semana-NN/_index.md`:

```toml
+++
title = 'Semana NN — Resumo curto dos temas'
date = 'YYYY-MM-DD'
draft = false
weight = N
description = 'Uma linha com os temas.'
tags = ['semana-NN', 'estudo']
+++
```

Corpo:
- Janela do cronograma (se estiver no README)
- Ordem **01 → último**
- Tabela com links Hugo (`./01-slug/`, não `./01-slug.md`)

`weight` da semana = número da semana (1, 2, 3…).

## Lista em `content/docs/_index.md`

Manter a tabela **Semanas** e acrescentar uma linha:

```markdown
| [NN](./semana-NN/) | temas resumidos |
```

Não apagar semanas anteriores.

## Validação

Na pasta `blog/`:

```bash
hugo
```

Confirmar `public/docs/semana-NN/index.html` existe. Preview:

`http://localhost:1313/blog/docs/semana-NN/`

(Se `hugo server` estiver na pasta errada ou com cache velho, reiniciar em `blog/`.)

## Commit (só se o usuário pedir)

```text
Add semana NN study materials under docs.

Organize … topics with Hugo front matter and list semana NN on the study index.
```

Não versionar `public/` (está no `.gitignore`).

## Referência rápida de estrutura

```text
content/docs/
├── _index.md
├── semana-01/
├── semana-02/
└── semana-NN/
    ├── _index.md
    ├── 01-....md
    └── …
```

+++
date = '2026-10-27T10:21:33-03:00'
draft = true
title = 'Hugo Markdown Cheatsheet'
description = 'Referência rápida do que dá pra fazer num post'
tags = ['hugo', 'markdown', 'cheatsheet']
categories = ['referência']
ShowToc = true
+++

Referência rápida: formatação, código e elementos úteis no Hugo + PaperMod.

## Texto

**negrito**, *itálico*, ***os dois***, ~~riscado~~, `código inline`

> Citação / nota importante.

Link: [documentação do Hugo](https://gohugo.io/content-management/formats/)

---

## Listas

- Item A
- Item B
  - Subitem

1. Primeiro
2. Segundo

- [x] Feito
- [ ] Pendente

## Tabela

| Comando | O que faz |
|---|---|
| `hugo server -D` | Preview local (inclui drafts) |
| `hugo` | Gera a pasta `public/` |
| `hugo new content/posts/x.md` | Cria post |

## Código

Inline: `const x = 1`

Bloco com highlight:

```js
function hello(name) {
  return `Olá, ${name}`
}

console.log(hello('Avila'))
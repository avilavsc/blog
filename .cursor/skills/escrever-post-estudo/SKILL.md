---
name: escrever-post-estudo
description: >-
  Escreve artigos didáticos e completos em content/posts/ (Hugo/Hextra) sobre
  um tema de estudo do TRT4 (FCC), como base teórica mais robusta que os
  materiais de docs. Use quando o usuário pedir um post, artigo ou explicação
  completa de um tema (ex.: "semana 04 bpmn", "post sobre BPMN").
---

# Post de estudo didático

## Entrada

O usuário informa o tema, às vezes com a semana (ex.: `Semana 04-bpmn`).
Se houver semana, ler `content/docs/semana-NN/NN-tema.md` como **ponto de partida, não como limite**.
O post deve cobrir o tema **completo**, como o edital e a FCC cobram:

- incluir pré-requisitos que o material assume que o leitor já sabe;
- incluir conceitos vizinhos necessários para entender o tema (ex.: BPMN → processos, BPM, orquestração vs coreografia);
- corrigir ou complementar o material de docs quando ele estiver raso ou impreciso;
- não copiar o texto de docs; reexplicar com outra abordagem.

## Estrutura de arquivos (page bundle)

```text
content/posts/<slug>/
├── index.md
└── imagens (.png/.svg), se houver
```

## Front matter

```toml
+++
title = 'BPMN do zero: notação, elementos e pegadinhas FCC'
date = 'YYYY-MM-DD'   # nunca no futuro (Hugo não publica)
draft = false
tags = ['semana-04', 'bpmn']
categories = ['estudo']
summary = 'Uma linha.'
+++
```

## Estrutura do artigo

A estrutura segue o tema, não um molde. Organize os títulos na ordem que melhor ensina o assunto.

**Sempre incluir:**

- Abertura curta: o que é o tema e por que importa na prova
- Desenvolvimento do básico ao avançado
- Resumo final (bullets)
- Links de apoio (fontes oficiais ou confiáveis)
- Link para a lista de posts: `[Ver todos os posts]({{< relref "/posts" >}})`

**Usar só quando agregar** (não forçar):

- Analogia ou intuição antes da teoria
- Diagramas Mermaid (fluxos, hierarquias, sequências)
- Exemplo prático ou caso do Judiciário
- Tabelas comparativas "X vs Y"
- Pegadinhas FCC
- Linha do tempo ou versões (ex.: ITIL 4 → 5, PMBOK 7 → 8)
- Código ou comandos (temas técnicos)

Na dúvida, prefira clareza e profundidade a cobrir todos os itens.

## Estilo

- Português claro, didático, frases curtas
- Explicar termos novos na primeira vez que aparecem
- Usar exemplos quando facilitarem a compreensão
- Sem questões (elas ficam em docs); no máximo 3 "teste rápido" com resposta

## Imagens

- Preferir Mermaid (bloco de código com a linguagem `mermaid`)
- Imagens externas: apenas com fonte e licença claras, citando o link
- Imagens locais: dentro do bundle, `![alt](diagrama.png)`
- Links internos sempre com `relref`, nunca começando com `/` (o site está em `/blog/`)

## Validação

- Rodar `hugo` em `blog/` sem erros
- Conferir `public/posts/<slug>/index.html`
- Commit e push só se o usuário pedir
+++
title = '04 — Português + lógica leve (manutenção pré-prova)'
date = '2026-10-31'
draft = false
weight = 4
tags = ['semana-13', 'portugues']
categories = ['estudo']
+++
# Português + lógica leve (manutenção pré-prova) · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Bloco:** Fase 4 — Pré-prova · Semana 13  
**Escopo deste arquivo:** manutenção **curta** de Português cobrável (crase, concordância, regência, pontuação, conectivos, redação oficial em microdoses) + **lógica leve** (condicional, equivalência, negação, validade). **Sem** interpretação de texto longa.  
**Fora de escopo aqui:** redação nº 9 (arquivo 05); roteiro de simulado leve / blocos fracos (arquivo 06); matéria nova de TI; revisão fina PDPJ/SQL/segurança/ágeis/ITIL (01–03).

**Edital (âncoras):** Ortografia · Concordância · Regência · Crase · Pontuação · Coordenação/subordinação · Conectivos · Confronto/reorganização de frases · Redação Oficial · Estrutura lógica / dedução.

**Modo pré-prova (18/01–24/01):** 20–30 min/dia — 1 cartão do mapa → 4–6 questões → anotar só o erro no caderno. Sem “aula nova”.

**Continuidade:** [/cursor/stores/self/docs/semana-11/06-portugues-diario-curto.md](/cursor/stores/self/docs/semana-11/06-portugues-diario-curto.md) · [/cursor/stores/self/docs/semana-6/07-logica-estatistica.md](/cursor/stores/self/docs/semana-6/07-logica-estatistica.md) · [/cursor/stores/self/docs/semana-3/10-raciocinio-logico.md](/cursor/stores/self/docs/semana-3/10-raciocinio-logico.md).

---

## 1. Material

### 1.1 Protocolo de manutenção (não estudar “do zero”)

| Dia sugerido | Foco | Tempo |
|--------------|------|-------|
| Seg | Crase + 4 Q | 20–25 min |
| Ter | Concordância V/N + 4 Q | 20–25 min |
| Qua | Regência + 4 Q | 20–25 min |
| Qui | Pontuação / conectivos + 4 Q | 20–25 min |
| Sex | Lógica leve (condicional/negação) + 5 Q | 25–30 min |
| Sáb | Misto PT+lógica (este arquivo 1–15 ou erros) | 30–40 min |
| Dom | Só caderno de erros (≤15 Q) | leve |

**Regra:** se acertar ≥80% no eixo, **não** reabrir o mapa inteiro — só os cartões dos erros.

### 1.2 Português — cartões FCC (anti-confusão)

**Crase (dois “a”).** Teste: masculinize. Se vira **ao/aos**, no feminino há **à/às**.

| Obrigatória | Proibida |
|-------------|----------|
| *obedeceu **às** normas* · *às 14h* · *àquele* | masculino · verbo · *a ela* · indefinido (*a uma*) |
| locuções: *à medida que, à noite, às vezes* | palavras repetidas (*cara a cara*) |
| *à moda de* / *voltou **à** casa da tia* | *foi a casa* (própria, sem det.) |

**Concordância verbal.** Ache o **sujeito real** (pode estar invertido/distante).

- *Haver* (= existir) / *fazer* (tempo) → singular: *Houve atrasos* · *Faz dois anos*.  
- *Deve haver* (não *devem haver*). *Parece haver* (não *parecem haver*).  
- **Se** apassivador: *Vendem-se certidões*. **Se** indeterminador: *Precisa-se de analistas*.  
- Sujeito oracional → principal no singular: *Convém que…* / *É necessário analisar…*.

**Concordância nominal (flash).**

| Caso | Forma |
|------|-------|
| *É necessário atenção* (sem artigo) | invariável |
| *É necessária **a** atenção* | concorda |
| *em anexo* | invariável |
| *meio cansada* | *meio* invariável |
| *anexo(s)/anexa(s)* | concorda com o anexado |

**Regência (flash).**

| Item | Prep. segura | Pegadinha |
|------|--------------|-----------|
| *assistir* (= ver) | **a** | *assistir o* ❌ |
| *aspirar / visar* (= almejar) | **a** | TD no outro sentido |
| *preferir X **a** Y* | **a** | *do que / mais…* ❌ |
| *implicar* (= acarretar) | TD | *implicar em* ❌ (culto clássico) |
| *obedecer / chegar* | **a** | *chegar em* (coloquial) |
| *compatível* | **com** | *compatível a* ❌ |
| *responsável* | **por** | |
| *acesso / apto / referente* | **a** | |

**Pontuação.** Não separar sujeito×verbo · verbo×OD/OI · nome×complemento. Intercalação = **par** de vírgulas.

**Conectivos (valor).**

| Valor | Exemplos |
|-------|----------|
| causa | *porque, pois, uma vez que, porquanto* |
| concessão | *embora, conquanto, ainda que* |
| condição | *se, caso, desde que* |
| oposição | *mas, porém, contudo, entretanto* |
| conclusão | *logo, portanto, pois* (posposto) |
| explicação | *isto é, ou seja, a saber* |

### 1.3 Lógica leve — mapa de manutenção

Flash:

> Condicional falsa **só** quando V → F.  
> Negação de \(P\to Q\) = **P e não Q**.  
> Equivalência útil: contrappositiva \(\neg Q \to \neg P\) **≡** \(\neg P \lor Q\).  
> Negação de \(P\land Q\) = \(\neg P\lor\neg Q\); de \(P\lor Q\) = \(\neg P\land\neg Q\).

| Forma | Status vs \(P\to Q\) |
|-------|---------------------|
| \(\neg Q \to \neg P\) | **contrapositiva** — equivalente |
| \(\neg P \lor Q\) | equivalente |
| \(Q \to P\) | **recíproca** — **não** equivalente |
| \(\neg P \to \neg Q\) | **inversa** — **não** equivalente |
| \(P \land \neg Q\) | **negação** |

**Validade (só o esquema):**

| Esquema | Forma |
|---------|-------|
| Modus ponens | \(P\to Q\); P; logo Q |
| Modus tollens | \(P\to Q\); ¬Q; logo ¬P |
| Silogismo hipotético | \(P\to Q\); \(Q\to R\); logo \(P\to R\) |
| Silogismo disjuntivo | \(P\lor Q\); ¬P; logo Q |

**Falácias:** afirmar o consequente; negar o antecedente. Em relações (pessoas/dias/cargos): monte tabela; “pode” ≠ “deve”.

### 1.4 Ponte PT × lógica (o que a FCC mistura)

- Reescrita de frase = equivalência **linguística** (sentido + gramática), não só tabela-verdade.  
- “Está plenamente correta” = gramática **e** clareza; uma vírgula SV ou crase falsa derruba.  
- Em Gerais, 2–4 Q de PT + 1–3 de lógica é típico: **ritmo** > profundidade nesta semana.

### 1.5 Checklist pré-prova (PT + lógica)

- [ ] Cartões de crase / *haver* / *se* / *preferir* / *assistir* revisados 1×  
- [ ] Contrapositiva × recíproca × negação da condicional no caderno  
- [ ] 15–20 erros antigos de PT/lógica refeitos (não matéria nova)  
- [ ] Sem insistir em interpretação longa  

---

## 2. Questões de treino (1–15)

**1.** Em padrão culto clássico, assiste-se:

A) o julgamento  
B) **ao** julgamento  
C) no julgamento (único culto)  
D) pro julgamento  
E) sob o julgamento  

**2.** Prefiro revisar:

A) do que resolver questões  
B) mais que resolver questões  
C) **a** resolver questões  
D) que resolver questões  
E) contra resolver questões  

**3.** Forma correta:

A) Houveram muitos atrasos  
B) Devem haver soluções  
C) Parecem haver dúvidas  
D) **Houve** muitos atrasos  
E) Fazem dois anos que…  

**4.** Crase **proibida** em:

A) às 9 horas  
B) àquele servidor  
C) **a** estudar  
D) às normas  
E) à medida que  

**5.** *É necessário atenção* está correta porque:

A) o adjetivo sempre varia  
B) com substantivo **sem artigo**, a expressão fica invariável no masculino  
C) “atenção” é masculino  
D) FCC exige sempre feminino  
E) há crase oculta  

**6.** Negação de “se P, então Q”:

A) se não P, então não Q  
B) se Q, então P  
C) **P e não Q**  
D) P ou Q  
E) não P ou Q  

**7.** Equivalente a “se tem OAB, então é advogado” (estrutura):

A) se é advogado, então tem OAB  
B) se não tem OAB, então não é advogado  
C) **se não é advogado, então não tem OAB**  
D) tem OAB e não é advogado  
E) tem OAB ou é advogado  

**8.** Vírgula **incorreta**:

A) A norma, quando clara, orienta.  
B) **Os servidores, analisaram o relatório.**  
C) Ontem, o tribunal publicou o edital.  
D) O candidato, cansado, parou.  
E) Em Porto Alegre, a prova ocorre à tarde.  

**9.** Conectivo de **concessão**:

A) porquanto  
B) uma vez que  
C) **embora**  
D) portanto  
E) ou seja  

**10.** *Chegar* no padrão culto cobrado pela FCC:

A) chegar em casa (único culto)  
B) **chegar a** Porto Alegre / **à** conclusão  
C) chegar pra  
D) chegar sob  
E) chegar perante sempre  

**11.** Modus tollens correto:

A) Se chove, a rua molha; chove; logo a rua molha  
B) Se chove, a rua molha; a rua não molha; **logo não chove**  
C) Se chove, a rua molha; a rua molha; logo chove  
D) Se chove, a rua molha; não chove; logo a rua não molha  
E) Chove ou a rua molha; chove; logo a rua não molha  

**12.** Negação de “não estudou e passou”:

A) estudou e passou  
B) não estudou ou não passou  
C) **estudou ou não passou**  
D) estudou e não passou  
E) não estudou ou passou  

**13.** Em *Vendem-se certidões*, o “se” é:

A) indeterminador (verbo singular obrigatório)  
B) **apassivador** (verbo concorda com o paciente)  
C) reflexivo obrigatório  
D) partícula expletiva  
E) conectivo concessivo  

**14.** *Compatível*:

A) a  
B) **com**  
C) em  
D) sob  
E) perante  

**15.** Na manutenção pré-prova, a melhor política é:

A) abrir gramática completa do zero  
B) **cartões dos erros + poucas Q/dia**  
C) só interpretação longa  
D) abandonar lógica  
E) estudar só redação oficial sem objetiva  

---

## 3. Questões de concurso (16–25)

> Questões reais (**FCC**). Foco: gramática + lógica leve — **não** interpretação longa. Gabarito só ao final.

**16.** (FCC – TRT-20/2016 – Analista Judiciário – Área Administrativa)  
Do ponto de vista da lógica, a proposição “se tem OAB, então é advogado” é equivalente à

A) tem OAB ou é advogado.  
B) se não tem OAB, então não é advogado.  
C) se não é advogado, então não tem OAB.  
D) é advogado e não tem OAB.  
E) se é advogado, então tem OAB.  

**17.** (FCC – TRT-9/2022 – Analista Judiciário – Área Administrativa – Especialidade: Contabilidade)  
A negação da afirmação: “não ficou doente e vai ficar em casa” é:

A) Ficou doente ou vai ficar em casa.  
B) Não ficou doente ou não vai ficar em casa.  
C) Ficou doente e não vai ficar em casa.  
D) Não ficou doente ou vai ficar em casa.  
E) Ficou doente ou não vai ficar em casa.  

**18.** (FCC – TRT-1/2025 – Técnico Judiciário – Área Administrativa)  
Está plenamente correta a redação da seguinte frase:

A) Das memórias em que nos valemos nem sempre nos cabem reconhecer sua utilidade.  
B) Imprime-se nas memórias caracterizadas por Bergson a distinção entre suas duas naturezas.  
C) Resultam com o esforço da atenção e da repetição a utilidade desse tipo de memória.  
D) A resistência de que se aplica ao esquecimento aproximam essas imagens da poesia.  
E) Diante do aspecto prático da memória-trabalho opõem-se a poesia das imagens-lembranças.  

**19.** (FCC – TRT-6/2025 – Analista Judiciário – Área Apoio Especializado – Especialidade: Tecnologia da Informação)  
Está inteiramente regular a pontuação da frase:

A) Se há benefícios há também transtornos, no uso indiscriminado, das novas tecnologias.  
B) Há reflexos no cotidiano nosso, quando do mau uso, dos dispositivos digitais.  
C) Acredita-se sim que, mesmo as boas ferramentas, podem trazer prejuízo se mal utilizadas.  
D) A ansiedade e a frustração, com a conectividade exagerada podem intensificar-se.  
E) Na falta de moderação, o uso dessas ferramentas acarreta, sem dúvida, graves transtornos.  

**20.** (FCC – TRT-15/2025 – Analista Judiciário – Área Administrativa)  
As normas de concordância verbal estão plenamente atendidas na frase:

A) Destacam-se entre as características do tempo vivido a ruptura que fraciona os momentos.  
B) Atribui-se a narrativas equivocadas uma verdade que passa a ser consagrada entre os incautos.  
C) Não haveriam em nós impulsos para que viesse a se esclarecer os mistérios da vida.  
D) Costumam-se crer que os fatos relativos à morte são mais obscuros que os que à vida afeta.  
E) Se aos momentos coubessem perdurar sem qualquer ruptura, perfaria os anos uma linha contínua.  

**21.** (FCC – TRT-11/2024 – Técnico Judiciário – Área Administrativa)  
Considere as frases a seguir.  
I. Apresse-se! Sua reunião vai começar às 8 horas.  
II. Uso lapiseira, mas sempre preferi escrever à lápis.  
III. Não diga nada à ninguém sobre nossos planos futuros.  
IV. Aos domingos, minha família se reúne para saborear um bom filé à parmegiana.  
V. Voltei à casa da minha infância depois de muitos anos.  
O sinal indicativo de crase foi usado corretamente APENAS em

A) II, III e IV.  
B) I, II e IV.  
C) I, III e IV.  
D) I, IV e V.  
E) II, IV e V.  

**22.** (FCC – TRT-6/2025 – Analista Judiciário – Área Apoio Especializado – Especialidade: Tecnologia da Informação)  
*Ferramentas úteis e necessárias, quando mal utilizadas, podem tornar-se letais.*  
Reescrevendo-se o período acima começando por *A má utilização de ferramentas*, uma complementação correta e coerente será:

A) pode ser letal, uma vez que sejam úteis e necessárias.  
B) ainda quando úteis e necessárias, pode tornar-se letal.  
C) conquanto úteis e necessárias, podem tornar-se letais.  
D) a menos que úteis e necessárias, poderão tornar-se letais.  
E) se tornaria letal, quando fossem úteis e necessárias.  

**23.** (FCC – TRT-1/2025 – Analista Judiciário – Área Administrativa — enunciado difundido em materiais de revisão FCC)  
As normas de concordância verbal estão plenamente observadas na frase:

A) Eram no convívio das estrelas e no cruzar das neblinas que Amyr Klink fez sua travessia.  
B) Abrigaram-se nos porões de sua alma de navegante a graça de cruzar os mares.  
C) Não move a um navegante como Amyr Klink advertências como as de Sartre.  
D) A muitos navegadores tentou a façanha marítima enfim conseguida por Amyr Klink.  
E) São dadas a poucas pessoas gozarem da solidão como uma alternativa.  

**24.** (FCC – BANRISUL/2019 – Escriturário)  
Quanto à pontuação e à observância do emprego do sinal de crase, está plenamente correta a frase:

A) Tendo em vista à longevidade da atual geração, as seguintes pode beneficiar um horizonte ainda mais largo.  
B) Dada a condição dos moços de hoje, os moços de amanhã obterão mais facilidades.  
C) Uma vez alcançada, a imortalidade, será que à ela todos festejarão?  
D) É à longo prazo que muitas felicidades possíveis são alcançadas.  
E) Sempre haverá aqueles que, à todo custo, perseguem o ideal da imortalidade.  

**25.** (FCC – TRT/SP – 2018 – Analista Judiciário – Área Judiciária / OJAF — raciocínio)  
Nair e Mariana receberam, no total, 198 processos para arquivar. Desse total, a maior parte foi entregue para Mariana. Depois da entrega, Mariana disse corretamente à Nair: “− Se eu lhe der um quarto dos processos que me deram para arquivar, você ficará com metade dos processos que vão sobrar para eu arquivar”. Nair respondeu para Mariana: “− Então eu proponho que você me dê um quarto dos processos que deram a você para arquivar”. Mariana aceita a proposta de Nair, o que implica dizer que Nair terá que arquivar x processos a mais do que teria que arquivar com a distribuição original de processos entre elas. Nas condições descritas, x é igual a

A) 44  
B) 64  
C) 66  
D) 32  
E) 72  

---

## 4. Gabarito comentado

| # | Gab. | Comentário curto |
|---|------|------------------|
| 1 | **B** | *assistir* (= ver) → **a** |
| 2 | **C** | *preferir X **a** Y* |
| 3 | **D** | *haver* (= existir) singular |
| 4 | **C** | antes de verbo → sem crase |
| 5 | **B** | *é necessário* + subst. sem artigo |
| 6 | **C** | negação da condicional = P ∧ ¬Q |
| 7 | **C** | contrappositiva |
| 8 | **B** | vírgula entre sujeito e verbo |
| 9 | **C** | concessão |
| 10 | **B** | *chegar a* / *à* |
| 11 | **B** | modus tollens |
| 12 | **C** | ¬(¬E ∧ P) = E ∨ ¬P |
| 13 | **B** | se apassivador |
| 14 | **B** | compatível **com** |
| 15 | **B** | manutenção > matéria nova |
| 16 | **C** | FCC TRT-20/2016 — contrappositiva |
| 17 | **E** | FCC TRT-9/2022 — ¬(¬D ∧ C) = D ∨ ¬C |
| 18 | **B** | FCC TRT-1/2025 — sujeito *a distinção* (sg.) |
| 19 | **E** | FCC TRT-6/2025 TI — intercalação “sem dúvida” |
| 20 | **B** | FCC TRT-15/2025 — *Atribui-se … uma verdade* |
| 21 | **D** | FCC TRT-11/2024 — I, IV e V |
| 22 | **B** | FCC TRT-6/2025 TI — concessão + sg. com *má utilização* |
| 23 | **D** | FCC TRT-1/2025 — sujeito *a façanha* (sg.) anteposto invertido |
| 24 | **B** | FCC BANRISUL/2019 — sem crase indevida; pontuação ok |
| 25 | **A** | FCC TRT/SP 2018 — M=176; M/4=44 |

**Distribuição treino 1–15:** B, C, D, C, B, C, C, B, C, B, B, C, B, B, B — e concurso 16–25: C, E, B, E, B, D, B, D, B, A (varia A–E).

---

*Arquivo 04 · Português + lógica leve (manutenção pré-prova) · Claudionir · TRT4 Analista TI (código 14) · FCC · Semana 13. Próximo: `05-portugues-redacao-final.md`.*

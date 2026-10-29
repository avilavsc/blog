+++
title = '01 — Visão PDPJ-Br'
date = '2026-10-01'
draft = false
weight = 1
tags = ['semana-01', 'pdpj']
categories = ['estudo']
+++
# Visão PDPJ-Br · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Visão da Plataforma Digital do Poder Judiciário  
**Escopo deste arquivo:** o que é a PDPJ-Br, finalidade, ecossistema nacional, interoperabilidade, ideia de governança, relação com os tribunais e o que ela **não** é.  
**Fora de escopo aqui:** Eureka, Zuul, Swagger, MapStruct, detalhes de Spring Boot/Cloud e microsserviços profundos (arquivos 02–09).

---

## 1. Material

### 1.1 O que é a PDPJ-Br

**PDPJ-Br** significa **Plataforma Digital do Poder Judiciário Brasileiro**. Em prova, trate-a como um **ecossistema nacional de integração digital** entre órgãos do Judiciário — não como “um software único” instalado em todos os tribunais.

A ideia central cobrada pela FCC é: o CNJ articula uma **plataforma comum** para que tribunais compartilhem **serviços, padrões e capacidades digitais**, reduzindo a lógica de cada tribunal reinventar o mesmo módulo isoladamente. Em outras palavras: **plataforma compartilhada + autonomia operacional dos tribunais**, sob diretrizes nacionais.

Três âncoras mentais úteis:

| Âncora | Frase cobrável |
|--------|----------------|
| Natureza | Ecossistema / plataforma de integração |
| Vetor técnico | Serviços, APIs e padrões comuns |
| Vetor institucional | Diretrizes e governança do CNJ sobre o digital do PJ |

### 1.2 Para que serve (finalidade)

A finalidade típica em enunciados FCC:

1. **Interoperar** sistemas e serviços entre tribunais e entre justiça e outros atores (quando o contexto exigir).
2. **Reutilizar** capacidades digitais (evitar silos absolutos e reescritas paralelas do mesmo problema).
3. **Padronizar** interfaces e práticas técnicas mínimas para troca de dados e consumo de serviços.
4. **Modernizar** a entrega de serviços judiciais digitais sob uma visão nacional coerente.
5. **Governar** o ecossistema digital do Poder Judiciário com regras e diretrizes comuns (sem transformar o CNJ em “dono do código de cada TRT”).

Não é necessário decorar discurso institucional longo: basta saber **integração + reuso + padronização + governança nacional**.

### 1.3 Ecossistema nacional (não monólito)

Contraste que a banca gosta de explorar:

| Modelo | Ideia | Encaixe na PDPJ? |
|--------|-------|------------------|
| **Monólito único nacional** | Um só sistema gigante substitui todos os sistemas judiciais | **Não** — leitura errada típica |
| **Silos absolutos** | Cada tribunal 100% isolado, sem padrão compartilhado | **Não** — oposto do discurso da plataforma |
| **Plataforma / ecossistema** | Capacidades comuns + integração + padrões; tribunais continuam existindo e operando | **Sim** — leitura correta |

A PDPJ-Br é cobrada como **plataforma**: componentes e serviços que se conectam, podem ser reaproveitados e evoluem com alguma independência. O Judiciário brasileiro é federado na prática (CNJ + tribunais), então a arquitetura mental da prova acompanha isso: **comum o que deve ser comum; local o que é de cada tribunal**.

### 1.4 Interoperabilidade (conceito-chave)

**Interoperabilidade** = capacidade de sistemas distintos **trocarem informação e cooperarem** de forma previsível, com contratos/padrões mínimos.

Na PDPJ, isso aparece como:

- tribunais **consumindo e expondo** serviços digitais alinhados a padrões nacionais;
- redução de integrações “ad hoc” sem contrato;
- foco em **APIs e serviços** como língua comum (detalhe REST/JSON fica no arquivo 03);
- visão de **ecossistema**, não de pasta compartilhada ou banco único nacional.

Erro clássico de prova: confundir interoperabilidade com “todos usam o mesmo monólito” ou “todos compartilham o mesmo banco de dados físico”. Interoperar **não exige** fundir sistemas em um só produto.

### 1.5 Ideia de governança

Governança, no nível cobrado neste subtópico, não é organograma completo nem texto de resolução artigo a artigo (isso é o arquivo de normativos). É a ideia de que:

- o **CNJ** define **diretrizes, padrões e marcos** do ecossistema digital;
- os **tribunais** aderem, implementam, operam e adaptam no seu contexto;
- há **evolução normativa** ao longo do tempo (base → atualizações), sem precisar listar números aqui;
- governança visa **coerência nacional** sem anular a realidade institucional dos tribunais.

Flash: **CNJ governa o ecossistema; tribunal opera e integra**.

### 1.6 Relação com os tribunais

Leitura cobrável:

- A PDPJ **não apaga** os sistemas e competências dos tribunais.
- Tribunais **participam** do ecossistema: consomem serviços comuns, disponibilizam capacidades, seguem padrões.
- Um TRT (como o TRT4) não “vira PDPJ”; ele **se relaciona** com a plataforma e com o padrão nacional de integração.
- Benefício esperado em prova: **menos isolamento**, mais capacidade de integração e reuso, sob regras comuns.

Analogia útil (não literal de edital): a PDPJ é mais “rede + padrões + serviços compartilháveis” do que “substituto único do PJe de cada tribunal”.

### 1.7 O que a PDPJ-Br **não** é

Lista de negações que costumam virar alternativa correta ou distrator:

1. **Não** é um monólito único que substitui todos os sistemas judiciais do país.
2. **Não** é apenas um site institucional do CNJ.
3. **Não** é sinônimo de “qualquer sistema de processo eletrônico local” (ex.: tratar PJe local como se fosse a PDPJ inteira).
4. **Não** é apenas uma biblioteca Java ou um framework Spring.
5. **Não** se reduz a uma ferramenta isolada (gateway, discovery, swagger etc.) — essas são peças técnicas do ecossistema/stack, não a definição da plataforma.
6. **Não** significa ausência de sistemas nos tribunais.
7. **Não** é política de segurança cibernética (ENSEC é outro eixo; não misturar).
8. **Não** é “banco de dados nacional único obrigatório para tudo”.

### 1.8 Monólito × plataforma (contraste FCC)

| Aspecto | Monólito (visão errada da PDPJ) | Plataforma / ecossistema (visão correta) |
|---------|----------------------------------|------------------------------------------|
| Deploy | Um artefato único nacional | Capacidades/serviços que integram e evoluem |
| Falha | Risco de “tudo ou nada” | Isolamento relativo de partes |
| Tribunais | Absorvidos num sistema só | Participam e interoperam |
| Padrão | Código único imposto | Padrões + serviços + governança |
| Integração | Internamente acoplada | Por contratos/APIs entre partes |

Em prova, se o enunciado falar em **ecossistema**, **interoperabilidade**, **serviços compartilhados** e **diretrizes nacionais**, a resposta caminha para **plataforma**. Se falar em **substituição total por um único sistema**, desconfie.

### 1.9 O que a FCC tipicamente testa neste tema

- Definição: PDPJ = **plataforma/ecossistema digital nacional** do PJ.
- Finalidade: **interoperabilidade**, reuso, padronização, modernização sob governança.
- Natureza: **não monólito**; não “só um portal”.
- Papéis: **CNJ (diretrizes/governança)** × **tribunais (adesão/operação/integração)**.
- Contrastes: silo × plataforma; monólito × ecossistema de serviços.
- Armadilhas: misturar PDPJ com ferramenta pontual da stack; misturar com ENSEC; dizer que tribunais deixam de ter sistemas próprios.

Para este arquivo, memorize o **conceito institucional-arquitetural**. Stack (Eureka/Zuul/Swagger/MapStruct) entra depois, como meios técnicos — não como definição da PDPJ.

### 1.10 Mini-síntese (cartão mental)

> **PDPJ-Br** = plataforma digital nacional do Judiciário → **interoperar + reusar + padronizar** serviços entre tribunais, sob **governança CNJ**, sem ser monólito único nem substituir a existência dos tribunais.

---

## 2. Questões de treino (1–15)

**1.** A leitura mais adequada da PDPJ-Br, no nível cobrado em concurso de Analista de TI do Judiciário, é:

A) Um único sistema monolítico obrigatório que substitui todos os sistemas processuais dos tribunais  
B) Um ecossistema/plataforma nacional de integração digital do Poder Judiciário  
C) Apenas o portal de notícias do CNJ na internet  
D) Sinônimo estrito de banco de dados nacional único para todos os tribunais  
E) Política exclusiva de segurança cibernética do PJ  

**2.** Em relação à finalidade típica atribuída à PDPJ-Br, assinale a alternativa que melhor reúne os eixos cobráveis:

A) Isolar cada tribunal em silos absolutos sem padrão comum  
B) Interoperabilidade, reuso de capacidades digitais e padronização sob diretrizes nacionais  
C) Eliminar a existência institucional dos tribunais regionais  
D) Substituir o CNJ pelas secretarias de TI de cada tribunal  
E) Proibir o uso de APIs entre órgãos do Judiciário  

**3.** Sobre a relação entre a PDPJ-Br e os tribunais, é correto afirmar que:

A) A plataforma extingue a necessidade de qualquer sistema local nos tribunais  
B) Os tribunais participam do ecossistema, aderindo a padrões e integrando/consumindo serviços, sem deixar de existir como órgãos  
C) Cada tribunal é obrigado a abandonar qualquer governança interna de TI  
D) A PDPJ-Br é propriedade exclusiva de um único TRT  
E) Tribunais não se relacionam com a plataforma; só o Executivo federal  

**4.** Interoperabilidade, no discurso típico da PDPJ-Br, significa principalmente:

A) Fusionar todos os códigos-fonte dos tribunais em um único repositório privado  
B) Capacidade de sistemas distintos cooperarem e trocarem informações com padrões/contratos previsíveis  
C) Compartilhar obrigatoriamente o mesmo servidor físico em Brasília  
D) Usar apenas planilhas enviadas por e-mail entre varas  
E) Impedir qualquer integração entre tribunais  

**5.** Assinale a afirmação **incorreta** sobre a PDPJ-Br:

A) É associada a um ecossistema digital nacional do Judiciário  
B) Busca reduzir isolamento extremo entre soluções judiciais digitais  
C) É corretamente descrita como monólito único substituto de todos os sistemas processuais  
D) Relaciona-se a padrões e integração entre órgãos  
E) Envolve ideia de governança nacional sobre o digital do PJ  

**6.** No contraste monólito × plataforma aplicado à PDPJ-Br, a visão de **plataforma** enfatiza:

A) Um único artefato de software nacional com deploy indivisível para todo o país  
B) Capacidades e serviços que se integram sob padrões comuns, com participação dos tribunais  
C) A proibição de qualquer serviço compartilhado  
D) A centralização física de todos os datacenters judiciais em um único prédio  
E) A substituição do CNJ por uma empresa privada de software  

**7.** Qual papel institucional combina melhor com a ideia de governança da PDPJ-Br no nível deste subtópico?

A) CNJ define diretrizes/padrões do ecossistema; tribunais operam e integram no seu contexto  
B) Cada vara cria o padrão nacional sem coordenação  
C) O CNJ apaga administrativamente todos os tribunais  
D) Apenas o STF escreve código das APIs de todos os TRTs  
E) Governança digital é responsabilidade exclusiva do Poder Executivo  

**8.** Um candidato afirma: “PDPJ-Br é só o nome comercial do Spring Boot.” A correção adequada é:

A) Correto: PDPJ-Br e Spring Boot são sinônimos no edital  
B) Incorreto: a PDPJ-Br é a plataforma/ecossistema digital do Judiciário; frameworks são meios técnicos, não a definição da plataforma  
C) Correto: PDPJ-Br é apenas MapStruct  
D) Correto: PDPJ-Br é apenas um gateway HTTP  
E) Incorreto apenas porque o nome certo seria Eureka  

**9.** A alternativa que melhor expressa o que a PDPJ-Br **não** é:

A) Ecossistema nacional de integração digital do PJ  
B) Iniciativa associada a interoperabilidade entre órgãos judiciais  
C) Meramente um site institucional de notícias do CNJ  
D) Plataforma sob diretrizes nacionais de governança digital  
E) Ambiente em que tribunais podem reutilizar capacidades digitais comuns  

**10.** Em um enunciado FCC que contraste “silo absoluto” e “plataforma nacional”, a PDPJ-Br alinha-se a:

A) Silo absoluto sem padrão compartilhado  
B) Plataforma/ecossistema com interoperabilidade e padrões comuns  
C) Ausência total de sistemas digitais no Judiciário  
D) Proibição de reuso de serviços entre tribunais  
E) Substituição do processo judicial por planilha offline  

**11.** Sobre padronização no contexto da PDPJ-Br, a leitura mais coerente é:

A) Padronizar interfaces/práticas mínimas para integração, sem exigir monólito único  
B) Padronizar significa obrigar o mesmo código-fonte idêntico em todos os tribunais, sem exceção conceitual  
C) Padronização é irrelevante para interoperabilidade  
D) Padronização equivale a unificar fisicamente todos os bancos em uma só instância  
E) Padronização só existe fora do Poder Judiciário  

**12.** Um analista diz que “adotar a PDPJ-Br implica que o TRT4 deixa de ter sistemas próprios e passa a usar apenas um monólito nacional”. Essa afirmação é:

A) Correta, porque plataforma e monólito são conceitos idênticos  
B) Correta, porque tribunais são extintos pela PDPJ  
C) Incorreta: a plataforma pressupõe integração/ecossistema, não extinção automática dos sistemas e da existência operacional dos tribunais  
D) Correta apenas para a Justiça do Trabalho  
E) Incorreta somente porque o monólito seria estadual, não nacional  

**13.** Qual combinação de ideias melhor resume a PDPJ-Br para revisão rápida?

A) Portal de notícias + proibição de APIs + silos absolutos  
B) Ecossistema nacional + interoperabilidade + reuso/padronização + governança CNJ + participação dos tribunais  
C) Banco único obrigatório + fim dos tribunais + ENSEC como sinônimo  
D) Apenas documentação Swagger sem qualquer dimensão institucional  
E) Apenas service discovery, sem finalidade de integração  

**14.** Em prova, misturar PDPJ-Br com política de segurança cibernética do PJ (ENSEC) é problema porque:

A) São exatamente o mesmo instituto normativo e técnico  
B) A PDPJ, neste eixo, é plataforma/ecossistema de integração digital; segurança cibernética é outro eixo, ainda que ambos digam respeito ao digital do PJ  
C) ENSEC substitui a PDPJ em todos os editais  
D) A PDPJ existe só para criptografia de disco  
E) Não há qualquer tema digital no CNJ além de ENSEC  

**15.** A afirmação mais alinhada ao contraste “o que a FCC testa” neste subtópico é:

A) Cobrança de sintaxe detalhada de anotações MapStruct como definição da PDPJ  
B) Cobrança do conceito de plataforma/ecossistema, finalidade de interoperabilidade e relação CNJ–tribunais, evitando a leitura monolítica  
C) Cobrança exclusiva de comandos Linux do datacenter do CNJ  
D) Cobrança de memorizar o organograma completo de todos os TRTs  
E) Cobrança de que PDPJ = apenas uma ferramenta de gateway  

---

### Gabarito comentado (treino 1–15)

1. **B** — Ecossistema/plataforma nacional; A/D são monólito/banco único; C reduz a site; E confunde com ENSEC.  
2. **B** — Eixos clássicos: interoperar, reusar, padronizar sob diretrizes nacionais.  
3. **B** — Tribunais participam; não são apagados (A/C/D/E erram o papel institucional).  
4. **B** — Interoperar = cooperação/troca previsível entre sistemas distintos.  
5. **C** — Justamente a negação: PDPJ **não** é monólito substituto total.  
6. **B** — Plataforma = capacidades/serviços + padrões + participação.  
7. **A** — CNJ dirige o ecossistema; tribunais operam/integram.  
8. **B** — Framework ≠ definição da plataforma.  
9. **C** — Negação correta: não é “só site de notícias”.  
10. **B** — Alinha-se à plataforma, não ao silo.  
11. **A** — Padrões mínimos de integração ≠ monólito idêntico.  
12. **C** — Participação/integração ≠ extinção automática de sistemas/tribunais.  
13. **B** — Cartão mental completo do subtópico.  
14. **B** — Eixos diferentes; não sinônimos.  
15. **B** — Foco conceitual institucional-arquitetural deste arquivo.

---

## 3. Questões estilo concurso FCC (16–25)

**16.** No contexto das iniciativas digitais do Poder Judiciário brasileiro, a Plataforma Digital do Poder Judiciário (PDPJ-Br) é corretamente caracterizada como:

A) sistema monolítico único destinado a substituir, de forma integral e imediata, todos os sistemas processuais eletrônicos em operação nos tribunais  
B) conjunto exclusivo de normas de segurança cibernética, sem qualquer dimensão de integração entre serviços digitais  
C) ecossistema nacional voltado à integração e ao compartilhamento de capacidades digitais entre órgãos do Judiciário, sob diretrizes comuns  
D) repositório físico centralizado e obrigatório de todo o acervo documental em papel dos tribunais  
E) ferramenta de escritório utilizada apenas para edição de planilhas administrativas do CNJ  

**17.** Constitui objetivo coerente com a concepção da PDPJ-Br:

A) impedir a troca de informações entre tribunais, preservando silos tecnológicos absolutos  
B) promover interoperabilidade e reutilização de serviços digitais, com padronização técnica mínima de integração  
C) eliminar a autonomia administrativa de todos os tribunais regionais do trabalho  
D) impor um único banco de dados físico compartilhado por todas as justiças do país, sem contratos de serviço  
E) restringir o uso de tecnologias digitais às cortes superiores  

**18.** Acerca da relação entre o Conselho Nacional de Justiça e os tribunais no arranjo típico associado à PDPJ-Br, assinale a alternativa correta.

A) O CNJ extingue os tribunais para concentrar toda a operação judiciária em um único sistema  
B) Os tribunais não participam do ecossistema digital nacional, cabendo-lhes apenas impressão de processos  
C) O CNJ formula diretrizes e padrões do ecossistema; os tribunais aderem, implementam e operam integrações no respectivo contexto  
D) Cada tribunal define, isoladamente, o padrão nacional vinculante para os demais  
E) A governança digital do Judiciário é atribuída exclusivamente ao Poder Executivo federal  

**19.** A interoperabilidade perseguida no âmbito da PDPJ-Br pressupõe, em essência:

A) a fusão obrigatória de todos os códigos-fonte judiciais em um único monólito nacional  
B) a capacidade de sistemas heterogêneos cooperarem mediante padrões e interfaces previsíveis  
C) a proibição de quaisquer interfaces entre sistemas de tribunais distintos  
D) a substituição de APIs por troca exclusiva de mídias removíveis  
E) a inexistência de qualquer padrão técnico comum  

**20.** Considere as afirmações abaixo.

I. A PDPJ-Br deve ser compreendida como plataforma/ecossistema de integração digital do Poder Judiciário.  
II. A PDPJ-Br, na leitura correta para prova, equivale a um monólito único que extingue os sistemas dos tribunais.  
III. A participação dos tribunais no ecossistema é compatível com a existência de diretrizes nacionais de padronização e integração.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e III, apenas  
D) II e III, apenas  
E) I, II e III  

**21.** Assinale a opção que apresenta interpretação **incompatível** com a visão da PDPJ-Br usualmente cobrada em concursos da área de TI do Judiciário.

A) Busca reduzir o isolamento extremo entre soluções digitais dos órgãos  
B) Favorece padrões comuns para integração de serviços  
C) Identifica-se com a mera publicação de notícias em sítio eletrônico institucional, sem dimensão de plataforma de serviços  
D) Involui ideia de governança nacional sobre o ecossistema digital  
E) Permite reuso de capacidades digitais entre participantes do ecossistema  

**22.** No contraste entre arquitetura monolítica nacional e modelo de plataforma digital aplicados ao discurso da PDPJ-Br, é correto concluir que:

A) a PDPJ-Br corresponde, por definição, ao monólito único substitutivo de todos os sistemas processuais  
B) a noção de plataforma enfatiza integração de capacidades/serviços e padrões comuns, e não a extinção conceitual do ecossistema em um único artefato  
C) plataforma e monólito são termos tecnicamente idênticos na doutrina de prova  
D) a PDPJ-Br rejeita qualquer forma de padronização  
E) a PDPJ-Br existe apenas para unificar fisicamente datacenters em um único endereço IP  

**23.** Um analista de tribunal afirma que “aderir à PDPJ-Br significa que o tribunal deixa de existir como órgão e perde qualquer sistema próprio, passando a operar exclusivamente um software único nacional”. Tal afirmação:

A) está correta, pois plataforma digital implica extinção dos tribunais  
B) está correta apenas para a Justiça Estadual  
C) está incorreta, pois a PDPJ-Br articula ecossistema de integração e padrões, sem corresponder à tese de extinção institucional dos tribunais  
D) está correta, pois interoperabilidade exige banco de dados único e fim da autonomia  
E) está incorreta somente porque o software único seria estadual  

**24.** Diferenciar a PDPJ-Br de iniciativas ou eixos de segurança cibernética do Poder Judiciário é relevante em prova porque:

A) ambos os temas são rigorosamente sinônimos e intercambiáveis em qualquer enunciado  
B) a PDPJ-Br, no eixo aqui estudado, refere-se à plataforma/ecossistema de integração digital, ao passo que segurança cibernética constitui eixo distinto, ainda que correlato ao ambiente digital  
C) a segurança cibernética substitui integralmente a ideia de interoperabilidade  
D) a PDPJ-Br não possui qualquer finalidade de integração entre órgãos  
E) não existem normas ou políticas digitais no âmbito do CNJ  

**25.** Para fins de preparação em Analista Judiciário – Tecnologia da Informação, a síntese mais adequada da PDPJ-Br é:

A) site de notícias + monólito único + extinção dos tribunais  
B) ecossistema nacional de plataforma digital orientado a interoperabilidade, reuso e padronização, com governança de diretrizes nacionais e participação dos tribunais  
C) apenas uma biblioteca de mapeamento objeto-objeto em Java  
D) apenas um mecanismo de descoberta de instâncias de microsserviço  
E) política exclusiva de banimento de APIs no Judiciário  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **C** |
| 17 | **B** |
| 18 | **C** |
| 19 | **B** |
| 20 | **C** |
| 21 | **C** |
| 22 | **B** |
| 23 | **C** |
| 24 | **B** |
| 25 | **B** |

**Notas rápidas:** 16 C (ecossistema); 17 B (interoperar/reusar/padronizar); 18 C (CNJ dirige × tribunais operam); 19 B (cooperação por padrões); 20 C (I e III; II é a tese monolítica errada); 21 C (reduzir a “site de notícias” é incompatível); 22 B (plataforma ≠ monólito); 23 C (adesão ≠ extinção institucional); 24 B (eixos distintos); 25 B (síntese completa do subtópico).

---

*Arquivo 01 · Visão PDPJ-Br · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 microsserviços.*

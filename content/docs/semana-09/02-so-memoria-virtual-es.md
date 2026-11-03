+++
title = '02 — SO — Memória (alocação, segmentação, virtual, paginação) + E/S e armazenamento'
date = '2026-10-25'
draft = false
weight = 2
tags = ['semana-09', 'so']
categories = ['estudo']
+++
# SO — Memória (alocação, segmentação, virtual, paginação) + E/S e armazenamento · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 02 — Gerência de memória · memória virtual · paginação/segmentação · substituição de páginas · E/S · DMA · visão de armazenamento  
**Escopo deste arquivo:** alocação contígua/partições; fragmentação interna × externa; compactação; swapping; memória virtual; MMU; páginas × frames; segmentação; TLB; page fault; algoritmos (FIFO, NRU, Clock, LRU — visão); thrashing; E/S programada/interrupção/DMA; dispositivo de bloco; noção cobrável de armazenamento secundário.  
**Fora de escopo aqui:** processos/threads/deadlocks (arquivo 01); administração Linux/Windows Server (packs seguintes); RAID detalhado de storage enterprise; filesystems ext4/NTFS artigo a artigo.

---

## 1. Material

### 1.1 Por que a FCC cobra este bloco

No TRT4 código 14, gerência de memória e E/S aparecem como **distinções objetivas**: o que a memória virtual **faz** (desvincular endereço do programa do físico), **paginação = blocos fixos** × **segmentação = blocos variáveis**, papel da **MMU**, **TLB**, o que é **page fault**, e — no eixo E/S — que **DMA** acessa o barramento **com independência da CPU** (não “CPU fica ociosa esperando byte a byte”).

Cartão mental:

| Tema | Flash |
|------|--------|
| Memória virtual | Programa usa endereços lógicos/virtuais; físico pode ser menor |
| Paginação | Páginas e *frames* de **tamanho fixo** |
| Segmentação | Segmentos de **tamanho variável** (visão lógica: código, dados, pilha…) |
| MMU | Hardware que **traduz** lógico → físico |
| TLB | Cache associativa de traduções recentes |
| DMA | Transferência dispositivo ↔ memória **sem** CPU no caminho de cada dado |

### 1.2 Hierarquia e o problema da alocação

CPU vê registradores/cache; depois RAM; depois disco/SSD (*backing store*). O gerenciador de memória decide **onde** colocar processos/páginas e como **proteger** espaços.

**Alocação contígua / partições:**

- **Partições fixas:** memória dividida em N pedaços de tamanho fixo (podem ser desiguais entre si). Job vai para a **menor partição que caiba** (ou fila única — variante cobrada).  
- **Partições dinâmicas / variável:** aloca exatamente o necessário a partir de buracos (*holes*).

Algoritmos de escolha de buraco (visão):

| Algoritmo | Ideia |
|-----------|--------|
| **First-fit** | Primeiro buraco ≥ N |
| **Next-fit** | Continua a busca de onde parou |
| **Best-fit** | Buraco que sobra o menor resto |
| **Worst-fit** | Maior buraco (às vezes deixa restos utilizáveis) |

### 1.3 Fragmentação interna × externa

| Tipo | Onde aparece | Ideia |
|------|--------------|--------|
| **Interna** | Paginação / partições fixas | Espaço **dentro** da unidade alocada que sobra (última página pela metade) |
| **Externa** | Segmentação / alocação variável / partições dinâmicas | Buracos **entre** alocações; soma livre pode bastar, mas nenhum buraco contíguo basta |

**Compactação:** juntar processos em bloco contíguo, unindo buracos. Cobra CPU; em prova FCC (TRT-23/2022) a ideia é exatamente “junta em bloco contíguo = compactação” — não “fragmentação dirigida”.

### 1.4 Swapping

**Swapping:** transferir processo (ou partes) entre RAM e disco para liberar memória principal. Não confundir com:

- **Thrashing:** excesso de faltas de página → sistema gasta quase todo tempo paginando, pouco progresso útil.  
- **Overlay (histórico):** programador particiona o programa manualmente.

### 1.5 Memória virtual — ouro de prova

**Memória virtual** desvincula o **endereçamento feito pelo programa** dos **endereços físicos** da RAM. O processo “vê” um espaço lógico (pode ser maior que a RAM); o SO + hardware mantêm na RAM só o que está em uso e trazem o restante do disco sob demanda.

Consequências cobráveis:

1. Executar processos cujo espaço total **excede** a RAM.  
2. Isolamento/proteção entre processos.  
3. Compartilhamento controlado de páginas (bibliotecas).  
4. Custo: **page faults**, pressão de disco, risco de thrashing.

Anti-confusão (FCC TRT-20/2024 e TRT-5/2022):

- Memória virtual **não** “dá mais CPU”.  
- **Não** elimina trocas com disco — **gerencia** essas trocas.  
- Desativar MV “para ir mais rápido” **não** é solução geral (pode impedir executar o que não cabe).  
- Otimizar: **mais RAM** e/ou **menos page faults** (working set, algoritmo de substituição, menos overcommit cego).

### 1.6 MMU e tradução

A **MMU** (*Memory Management Unit*) é **hardware** (não “módulo de software que define endereços”). Ela traduz endereço lógico/virtual → físico, aplica proteção e detecta faltas.

Fluxo paginado típico:

1. CPU emite endereço virtual (nº página + deslocamento).  
2. Consulta **TLB**; se *hit*, obtém frame rápido.  
3. Se *miss*, consulta **tabela de páginas** na RAM; atualiza TLB.  
4. Se página inválida/ausente → **page fault** (trap ao SO).  
5. SO escolhe vítima (algoritmo de substituição), traz página do disco, atualiza tabelas, reinicia a instrução.

Flash FCC TRT-14/2022: afirmativas corretas típicas — MMU traduz lógico→físico; paginação usa partições/**blocos de tamanho fixo**. Erradas típicas — “MV vincula endereço do programa à memória secundária”; “CPU acessa RAM diretamente com endereço virtual sem tradução”.

### 1.7 Paginação

- Espaço virtual dividido em **páginas** de tamanho fixo (ex.: 4 KiB).  
- RAM dividida em **molduras / frames / page frames** do **mesmo** tamanho.  
- Tabela de páginas: página → frame (ou “não presente”) + bits (válido, referência R, modificação/dirty M, proteção).

**Page fault:** referência a página não mapeada na RAM. O SO:

1. Encontra/aloca frame.  
2. Se necessário, escreve vítima *dirty* no disco.  
3. Lê a página desejada.  
4. Atualiza tabela/TLB.  
5. Retoma o processo.

**Fragmentação na paginação:** tipicamente **interna** (última página). **Não** chame de fragmentação externa o pedaço vazio da última página (erro clássico de prova).

### 1.8 Segmentação

Segmentos refletem a **estrutura lógica** do programa (código, dados, pilha, heap…) com **tamanhos variáveis**. Endereço = (segmento, deslocamento). Tabela de segmentos guarda base, limite, proteção.

| | **Paginação** | **Segmentação** |
|---|---------------|-----------------|
| Tamanho dos blocos | Fixo | Variável |
| Visão | Física/gerência uniforme | Lógica do programador |
| Fragmentação típica | Interna | Externa |
| Combinação | Sistemas reais usam **segmentação + paginação** | |

Armadilha FCC: inverter fixo/variável entre as duas técnicas.

### 1.9 TLB (Translation Lookaside Buffer)

Sem aceleração, cada acesso poderia exigir **consulta à tabela de páginas + acesso ao dado**. A **TLB** (também chamada memória **associativa** nesse contexto) cacheia entradas recentes da tabela de páginas **dentro/junto da MMU**.

- **TLB hit:** tradução rápida.  
- **TLB miss:** busca na tabela de páginas (mais lento), depois preenche TLB.

FCC DPE-SP/2015: “translações mais frequentes ficam em pequena memória associativa chamada TLB” — correto. Errado típico: dizer que páginas de MV são de **tamanho variável** e trocadas com a **cache** (confunde níveis).

### 1.10 Substituição de páginas (visão cobrável)

Quando falta frame livre, escolhe-se **vítima**:

| Algoritmo | Ideia |
|-----------|--------|
| **Ótimo** | Substitui a que será usada mais tarde (referência teórica) |
| **FIFO** | Sai a que entrou há mais tempo (pode sofrer anomalia de Belady) |
| **Segunda chance / Clock** | FIFO + bit R; ponteiro circular |
| **LRU** | Menos recentemente usada |
| **NRU** | Classes por bits R e M; prefere não referenciada / não modificada |

FCC ARTESP/2017: NRU procura páginas **não referenciadas recentemente** e olha bit de **modificação**. Distratores distorcem FIFO/LIFO/Clock.

### 1.11 Thrashing e working set

Se muitos processos competem e o **working set** de cada um não cabe, sobe a taxa de page faults → **thrashing**. Mitigações cobráveis: reduzir multiprogramação, *working set model*, mais RAM, priorizar processos com locality boa.

### 1.12 E/S — visão cobrável

Três estilos clássicos de realizar E/S:

| Modo | Ideia |
|------|--------|
| **Programada / polling** | CPU fica interrogando o dispositivo |
| **Por interrupção** | Dispositivo avisa; CPU pode fazer outra coisa entre bytes/blocos |
| **DMA** | Controladora transfere bloco dispositivo ↔ memória; CPU só inicia/finaliza |

**DMA (*Direct Memory Access*):**

- Controladora de DMA tem acesso ao **barramento** de forma **independente da CPU**.  
- A **localização física** da controladora (perto/longe da CPU) **não** é o que define essa independência (FUNDATEC/BRDE).  
- Em disputa pelo barramento, pode ocorrer **roubo de ciclo** (*cycle stealing*): DMA usa o barramento por ciclos, CPU suspende temporariamente o uso do barramento — CPU **não** fica necessariamente ociosa a espera de toda a transferência.

**Dispositivo de bloco:** armazena informação em blocos de tamanho fixo endereçáveis (disco/SSD típicos). Contrasta com dispositivo de caractere (stream).

**Armazenamento (flash de prova):** disco/SSD como *backing store* da MV; buffer cache do SO; escalonamento de disco (visão); não misturar “DMA = algoritmo de substituição de página”.

### 1.13 Mini-síntese

```
Alocação → fragmentação (int/ext) → compactação / swapping
MV = endereço lógico ≠ físico | MMU traduz | TLB acelera
Paginação = fixo (página=frame) | Segmentação = variável
Page fault → substituição (FIFO/NRU/Clock/LRU) | thrashing = excesso de faltas
E/S: polling | interrupção | DMA (barramento independente)
```

---

## 2. Questões de treino (1–15)

**1.** Memória virtual, no sentido cobrável FCC, permite sobretudo:

A) Eliminar a necessidade de disco em qualquer carga  
B) Garantir que a CPU nunca sofra preempção  
C) Que o espaço lógico do processo possa exceder a RAM física, com mapeamento sob demanda  
D) Substituir a MMU por um semáforo  
E) Tornar paginação e segmentação técnicas idênticas  

**2.** Na paginação, páginas e frames são:

A) Sempre de tamanhos diferentes por definição  
B) Do mesmo tamanho (blocos fixos)  
C) Variáveis como segmentos lógicos do programador  
D) Armazenados apenas em registradores da ALU  
E) Independentes de qualquer tabela de mapeamento  

**3.** A segmentação pura caracteriza-se por:

A) Blocos de tamanho fixo iguais aos frames  
B) Divisão lógica em segmentos de tamanho variável  
C) Proibição de fragmentação externa  
D) Ausência de tabela de mapeamento  
E) Ser sinônimo de Round-Robin  

**4.** A MMU é corretamente descrita como:

A) Biblioteca Java obrigatória do edital  
B) Algoritmo SJF  
C) Sistema de arquivos NTFS  
D) Unidade de hardware que traduz endereços lógicos/virtuais em físicos  
E) Política de aging de processos  

**5.** Fragmentação **interna** é tipicamente associada a:

A) Somente redes TCP  
B) Exclusivamente compactação bem-sucedida  
C) Espaço desperdiçado **dentro** da última página/partição de tamanho fixo  
D) Buracos entre segmentos que somados bastariam  
E) Deadlock de Coffman  

**6.** Fragmentação **externa** aparece com mais naturalidade em:

A) Paginação pura com frames idênticos sem buracos entre alocações lógicas  
B) Cenários de alocação com blocos/segmentos de tamanho variável  
C) TLB hit rate de 100% permanente  
D) DMA cycle stealing  
E) Quantum de Round-Robin  

**7.** Page fault ocorre quando:

A) O processo termina normalmente  
B) Há TLB hit e dado na cache L1  
C) O escalonador escolhe FCFS  
D) A página referenciada não está presente/mapeada na RAM  
E) O mutex é liberado  

**8.** TLB serve para:

A) Substituir o disco rígido  
B) Acelerar a tradução cacheando entradas recentes da tabela de páginas  
C) Implementar pipes entre shells  
D) Detectar espera circular  
E) Garantir exclusão mútua de impressoras  

**9.** Thrashing indica, em essência:

A) Compactação perfeita da RAM  
B) Ausência total de E/S  
C) Uso exclusivo de registradores  
D) Algoritmo Round-Robin com quantum infinito  
E) Excessiva paginação com pouco trabalho útil  

**10.** Em E/S por DMA, a ideia central é:

A) A CPU transferir byte a byte em busy-wait obrigatório  
B) O dispositivo gravar apenas no TLB  
C) Transferência direta entre dispositivo e memória com pouca intervenção contínua da CPU  
D) Eliminar interrupções do sistema  
E) Trocar o conceito de frame pelo de segmento  

**11.** Relação correta paginação × segmentação:

A) Paginação = variável; segmentação = fixo  
B) Ambas são sempre idênticas em fragmentação  
C) Nenhuma usa tabela de mapeamento  
D) Paginação = fixo; segmentação = variável  
E) Segmentação elimina a MMU  

**12.** Compactação de memória, no vocabulário de concurso, consiste em:

A) Aumentar o quantum dos processos I/O-bound  
B) Juntar processos/alocações em região contígua para unificar buracos  
C) Apagar a tabela de páginas  
D) Forçar starvation proposital  
E) Desligar a MMU  

**13.** Assinale a afirmativa **incorreta**:

A) Swapping move processos entre RAM e secundária  
B) Best-fit escolhe o buraco que melhor se aproxima do pedido  
C) Memória virtual desvincula endereço do programa do físico  
D) NRU considera bits de referência/modificação  
E) Desativar memória virtual é sempre a melhor otimização de desempenho em qualquer carga  

**14.** Dispositivo de **bloco**, na visão clássica de E/S:

A) Só transmite caractere a caractere sem endereço  
B) É idêntico a um mutex  
C) Armazena informação em blocos de tamanho fixo endereçáveis  
D) Substitui a condição de espera circular  
E) É o nome do algoritmo Clock  

**15.** Síntese correta:

A) MV + paginação(fixo) + segmentação(variável) + MMU/TLB + page fault + DMA independente no barramento  
B) Fragmentação interna = buracos entre segmentos variáveis  
C) TLB armazena o disco inteiro  
D) Page fault = starvation de Coffman  
E) Segmentação usa sempre frames de 4 KiB idênticos ao código  

---

### Gabarito comentado (treino 1–15)

1. **C** — Espaço lógico pode > RAM.  
2. **B** — Mesmo tamanho.  
3. **B** — Segmentos variáveis.  
4. **D** — Hardware de tradução.  
5. **C** — Interna na unidade fixa.  
6. **B** — Externa com tamanhos variáveis.  
7. **D** — Página ausente.  
8. **B** — Cache de tradução.  
9. **E** — Thrashing.  
10. **C** — DMA.  
11. **D** — Fixo × variável.  
12. **B** — Compactação.  
13. **E** — Desativar MV não é panaceia.  
14. **C** — Bloco endereçável.  
15. **A** — Síntese do arquivo.

---

## 3. Questões de concurso (16–25)

**16.** (FCC · ARTESP/2017 · Especialista em Regulação — TI)  
No gerenciamento de memória, o mecanismo de paginação utiliza algoritmos de substituição de páginas, que são políticas definidas para escolher qual página da memória será removida para dar lugar a uma página que foi solicitada e que precisa ser carregada. Dentre estes encontra-se o algoritmo

A) FIFO − First In First Out que consiste em substituir a última página que foi carregada na memória. Esta escolha não leva em consideração se a página está sendo muito utilizada ou não, assim a quantidade de falta de páginas tende a diminuir quando o tamanho da memória também diminui.  
B) NRU − Not Recently Used que procura por páginas que não foram referenciadas nos últimos acessos para serem substituídas e também verifica, através de um bit de modificação, se a página teve seu conteúdo alterado durante sua permanência em memória.  
C) LIFO − Last In First Out, que consiste em substituir a página que foi carregada há mais tempo na memória. Na ocorrência de uma falta de página, a primeira página da lista será substituída e a nova será acrescentada ao final da lista.  
D) CLOCK, que faz a substituição da última página acessada. Este escolhe a última página acessada para ser substituída. Dessa forma, é possível explorar com mais eficiência o princípio de localidade temporal apresentada pelos acessos.  
E) MRR − Most Recently Removed, que mantém todas as páginas em uma lista circular. A ordem mantida segue a sequência em que elas foram carregadas em memória. Além disso, é adicionado um byte de uso que indica se a página foi referenciada novamente depois de ter sido carregada.  

**17.** (FCC · TRT-14/2022 · Técnico Judiciário — TI)  
Sobre gerência de memória realizada pelos sistemas operacionais, considere.  
I. A MMU (*Memory Management Unit*) tem a função de traduzir endereços lógicos para físicos.  
II. A técnica de paginação divide o espaço de endereçamento lógico e memória física em partições de tamanho fixo.  
III. O uso de memória virtual vincula o endereço feito pelo programa com os endereços da memória secundária.  
IV. Os processadores utilizam os endereços virtuais para acesso à memória principal.  
Está correto o que se afirma APENAS em

A) I.  
B) III.  
C) I e II.  
D) III e IV.  
E) I, II e IV.  

**18.** (FCC · TRT-23/2022 · Analista Judiciário — TI)  
Ao estudar sobre gerenciamento de memória, uma Analista verificou que a memória é particionada em tamanhos fixos (não necessariamente iguais). Quando um processo é trazido para dentro da memória, é colocado na menor partição que é suficiente para acomodá-lo. Com o tempo, a memória pode ficar fragmentada ao deixar pequenos buracos. Para resolver esse problema usa-se um processo em que o sistema operacional

A) separa os processos afins em áreas contíguas, denominado fragmentação.  
B) divide os processos maiores em blocos adjacentes, denominado racionalização.  
C) executa e deleta primeiro os processos em fase de conclusão, libera a memória e realiza a compactação dirigida.  
D) junta todos os processos em um mesmo bloco contíguo, denominado compactação.  
E) junta todos os processos em blocos adjacentes, denominado fragmentação dirigida.  

**19.** (FCC · TRT-20/2024 · Analista Judiciário — TI)  
Durante a execução de um programa crítico em um sistema operacional, foi observado que a memória estava sendo usada de forma ineficiente, comprometendo o desempenho da máquina. Esse problema foi tema de discussão e um Analista interveio, esclarecendo que

A) paginação é um método de alocação de memória que divide o processo em segmentos grandes para otimizar o uso da memória. A memória virtual é usada para permitir que os processos utilizem mais CPU, aumentando sua capacidade de processamento. A solução mais comum para melhorar o desempenho é desativar a memória virtual e confiar exclusivamente na RAM, já que o acesso à RAM é mais rápido e elimina a dependência do disco rígido.  
B) segmentação e paginação são técnicas equivalentes de gerenciamento de memória, ambas usadas para dividir a memória em blocos de tamanho variável. A memória virtual depende fortemente do disco rígido, o que provoca quedas significativas no desempenho. Uma solução adequada seria desativar a memória virtual e utilizar apenas a memória RAM para garantir uma execução mais eficiente dos processos.  
C) segmentação é uma técnica de gerenciamento de memória que divide o processo em blocos de tamanho fixo, enquanto a paginação divide o processo em blocos de tamanho variável. A memória virtual é usada para permitir que o sistema operacional execute processos maiores do que a memória física disponível, eliminando a necessidade de trocas entre memória e disco.  
D) memória virtual é uma técnica que permite que processos usem mais memória do que a RAM física disponível, armazenando parte dos dados em disco. A paginação divide a memória em blocos fixos chamados de páginas e mapeia essas páginas para quadros de memória. Uma solução para otimizar o desempenho seria aumentar a quantidade de RAM ou reduzir o número de page faults.  
E) a alocação de memória dinâmica é realizada por meio da técnica de segmentação, que divide a memória em blocos de tamanho fixo para evitar fragmentação interna. A memória virtual é utilizada apenas em sistemas com suporte para segmentação e é sempre mais eficiente do que a memória física.  

**20.** (FCC · TRT-5/2022 · Técnico Judiciário — TI)  
Em relação ao gerenciamento de memória é correto afirmar que

A) o módulo de software MMU (*Memory Management Unit*) define os endereços associados a cada processo.  
B) quanto mais processos em execução pior o nível de ocupação da CPU.  
C) a segmentação divide o espaço de endereçamento virtual em blocos de tamanhos fixos.  
D) a memória virtual por paginação divide o espaço de endereçamento virtual e real em blocos de tamanho variável.  
E) memória virtual desvincula o endereçamento feito pelo programa dos endereços físicos da memória principal.  

**21.** (FCC · DPE-SP/2015 · Administrador de Redes)  
Com respeito a memória virtual, considere:  
I. As translações mais frequentes ficam armazenadas em uma pequena memória associativa chamada TLB (*Translation Lookaside Buffer*).  
II. O método mais usual utilizado pela memória virtual é a divisão do espaço de endereçamento em páginas de tamanho variável, que são a unidade de transferência entre a memória principal e a memória cache.  
III. A memória virtual também faz a translação de endereços virtuais em endereços reais, já que os programas normalmente enxergam um espaço de endereçamento maior que a memória física.  
IV. A memória virtual é um conjunto de hardware e de rotinas do sistema operacional. Além do controle da hierarquia entre a memória principal e a memória secundária, ela realiza a proteção, evitando que um programa modifique informações que pertençam a algum outro.  
Está correto o que consta APENAS em

A) I, II e III.  
B) I e II.  
C) II, III e IV.  
D) III e IV.  
E) I, III e IV.  

**22.** (IFRS · Concurso Público Edital 19/2016 · Informática Geral)  
Leia as sentenças a seguir sobre Memória Virtual:  
I. Na técnica de Paginação, páginas (*pages*) e molduras de página (*page frames*) são sempre do mesmo tamanho.  
II. Quando ocorre uma falta de página, o sistema operacional precisa escolher, por meio de algoritmo de substituição de páginas, uma página a ser removida da memória.  
III. Chama-se de fragmentação externa quando parte da última página não é preenchido, resultando em espaço de memória desperdiçado.  
Assinale a alternativa em que todas a(s) sentença(s) está(ão) CORRETA(S):

A) Apenas I.  
B) Apenas II.  
C) Apenas I, II.  
D) Apenas I, III.  
E) Apenas II, III.  

**23.** (IFRS · Concurso Público Edital 19/2016 · Informática Geral)  
Leia as sentenças a seguir sobre sistemas de Entrada/Saída:  
I. O controlador de DMA (*Direct Memory Access*) tem acesso ao barramento do sistema independente da CPU.  
II. Controladoras de dispositivos são capazes de tratar no máximo 2 (dois) dispositivos idênticos.  
III. Um dispositivo de bloco é aquele que armazena informações em blocos de tamanho fixo, cada um com seu próprio endereço.  
Assinale a alternativa em que todas a(s) sentença(s) está(ão) CORRETA(S):

A) Apenas I.  
B) Apenas II.  
C) Apenas I, II.  
D) Apenas I, III.  
E) Apenas II, III.  

**24.** (FUNDATEC · BRDE/2015 · Analista de TI — Suporte)  
Em relação ao acesso direto à memória no gerenciamento de E/S de um sistema operacional, assinale a alternativa correta.

A) A controladora de DMA não permite um acesso independente ao barramento do sistema, dependendo sempre da CPU.  
B) A controladora de DMA só depende da CPU quando o processador possui mais de um núcleo.  
C) A localização física da controladora de DMA deve ficar próxima à CPU para que o acesso ao barramento do sistema seja independente da CPU.  
D) A localização física da controladora de DMA deve ficar distante da CPU para que o acesso ao barramento do sistema seja independente da CPU.  
E) Não importa a localização física da controladora de DMA, ela sempre terá acesso ao barramento do sistema de forma independente da CPU.  

**25.** (FUNRIO · FUNAI · Profissional de Nível Superior)  
O dispositivo em hardware para mapear os endereços virtuais para endereços físicos, sem passar pela tabela de páginas do sistema operacional, denominado TLB (*Translation Lookaside Buffer* ou tabela de tradução de endereços), é também conhecido como memória

A) associativa.  
B) paginada.  
C) secundária.  
D) segmentada.  
E) virtual.  

---

### Gabarito comentado (concurso 16–25)

16. **B** — Definição correta de NRU (demais distorcem FIFO/Clock etc.).  
17. **C** — Só I e II; III e IV invertem o papel da MV/CPU.  
18. **D** — Compactação = bloco contíguo.  
19. **D** — MV + paginação em frames; otimizar RAM / page faults.  
20. **E** — MV desvincula lógico de físico.  
21. **E** — I, III e IV; II erra (páginas são de tamanho **fixo**, unidade com disco/*backing*, não “variável com cache”).  
22. **C** — I e II; III descreve fragmentação **interna**, não externa.  
23. **D** — I e III; II é falsa (limite “no máximo 2” não é regra geral).  
24. **E** — Independência no barramento independe da “distância física” da controladora.  
25. **A** — TLB = memória associativa (cache de tradução).

---

## 4. Como usar este arquivo

1. Fechar de cor: **MV desvincula** · **paginação fixa × segmentação variável** · **MMU/TLB** · **interna × externa** · **DMA**.  
2. Treino 1–15 → gabarito.  
3. Concurso 16–25 sem consultar (priorizar 17–21, perfil TRT/FCC recente).  
4. Revisar junto com `01-so-processos-threads-deadlocks.md` sem misturar PCB/Coffman com frames/TLB.  
5. Caderno de erros: inverter fixo/variável e chamar interna de externa.

---

*Arquivo 02 · SO memória virtual + E/S · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 9.*

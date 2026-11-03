+++
title = '01 — SO — Processos, Threads, CPU, Comunicação, Sincronização, Deadlocks'
date = '2026-10-24'
draft = false
weight = 1
tags = ['semana-09', 'so']
categories = ['estudo']
+++
# SO — Processos, Threads, CPU, Comunicação, Sincronização, Deadlocks · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Processos · threads · escalonamento de CPU · IPC · sincronização · deadlocks · starvation  
**Escopo deste arquivo:** PCB e estados; processo × thread (compartilhado × privado); escalonamento (FCFS, SJF, prioridade, Round-Robin, filas multinível); IPC (pipes, mensagens, memória compartilhada); seção crítica, mutex, semáforos; condições de Coffman; prevenção/evitação/detecção; starvation × deadlock; aging.  
**Fora de escopo aqui:** memória virtual/paginação/segmentação e E/S-armazenamento (arquivo 02); Linux/Windows Server detalhado (packs seguintes da semana 9); redes e segurança.

---

## 1. Material

### 1.1 Por que a FCC cobra este bloco

No edital do TRT4 (código 14), **Sistemas Operacionais** pede compreensão cobrável de **processos**, **threads**, **concorrência** e **impasse**. A banca quase nunca pede “desenhar o kernel”: ela quer **distinções** (processo × thread; deadlock × starvation; privado × compartilhado), **condições de Coffman**, **papéis de mutex/semáforo** e **comportamento de Round-Robin** (quantum + preempção).

Cartão mental:

| Conceito | Flash cobrável |
|----------|----------------|
| **Processo** | Programa em execução + contexto; espaço de endereçamento próprio |
| **Thread** | Fluxo de execução *dentro* do processo; compartilha o espaço do processo |
| **Deadlock** | Conjunto bloqueado à espera mútua de recursos |
| **Starvation** | Processo/thread “passa fome” (nunca progride), sem necessariamente haver ciclo |
| **Seção crítica** | Trecho que acessa recurso compartilhado mutável — exige exclusão mútua |

### 1.2 Processo: o que o SO gerencia

**Processo** = programa em execução. O SO mantém o **PCB** (*Process Control Block*): PID, estado, prioridade, registradores salvos, ponteiros de memória, arquivos abertos, contabilidade etc.

Estados clássicos (visão cobrável):

| Estado | Ideia |
|--------|--------|
| **Novo** | Criado, ainda não elegível |
| **Pronto** (*ready*) | Pode rodar; espera a CPU |
| **Executando** | Tem a CPU |
| **Bloqueado / espera** | Aguarda evento (E/S, lock, mensagem) |
| **Terminado** | Finalizado |

Transições típicas: pronto → executando (despacho); executando → pronto (preempção/quantum); executando → bloqueado (espera evento); bloqueado → pronto (evento ocorre).

**Troca de contexto:** salvar o contexto do processo que sai e restaurar o do que entra. É **mais cara** entre processos do que entre threads do mesmo processo (threads já compartilham grande parte do contexto).

### 1.3 Thread: processo leve

**Thread** (*lightweight process*) = unidade escalonável de execução **dentro** de um processo. Multithreading permite concorrência/paralelismo com **custo menor** de criação e chaveamento.

Tabela ouro (Tanenbaum / FCC TRT-4/2022):

| **Privado por thread** | **Compartilhado entre threads do mesmo processo** |
|------------------------|-----------------------------------------------------|
| Contador de programa (PC) | Espaço de endereçamento |
| Registradores | Variáveis globais |
| Pilha | Arquivos abertos |
| Estado | Processos filhos; alarmes pendentes; sinais e tratadores; contabilidade |

Armadilhas FCC:

1. Dizer que thread tem **espaço de endereçamento próprio** (isso é processo).  
2. Incluir **arquivos abertos** ou **alarmes pendentes** como privados.  
3. Afirmar que “thread nunca influencia outra” — compartilham dados → precisam **sincronização**.

**User-level × kernel-level threads (visão):**

- *User-level:* biblioteca no processo; kernel vê 1 fluxo; chaveamento barato, mas E/S de uma thread pode bloquear o processo inteiro.  
- *Kernel-level:* SO conhece cada thread; pode escalonar em múltiplos núcleos; criação/chaveamento um pouco mais caros.

Benefícios cobráveis do multithreading: **capacidade de resposta**, **compartilhamento de recursos** (já dentro do processo), **economia** (criar thread < criar processo), **escalabilidade** em multicore.

### 1.4 Escalonamento de CPU

O **escalonador** escolhe, entre os **prontos**, quem recebe a CPU. Objetivos: utilização da CPU, *throughput*, tempo de resposta/espera, justiça.

| Algoritmo | Ideia | Preempção? | Risco típico |
|-----------|-------|------------|--------------|
| **FCFS / FIFO** | Ordem de chegada | Não (clássico) | Convoy effect (job longo na frente) |
| **SJF** | Menor job primeiro | Em geral não (SRTF = preemptivo) | **Starvation** de jobs longos |
| **Prioridade** | Maior prioridade primeiro | Pode ser | **Starvation** de baixa prioridade |
| **Round-Robin (RR)** | Fila circular + **quantum** | Sim | Quantum muito grande ≈ FCFS; muito pequeno → overhead |
| **Filas multinível / feedback** | Várias filas, políticas distintas; feedback ajusta | Sim (típico) | Complexidade de tuning |

Flash Round-Robin (FCC clássico): semelhante ao FCFS, **mas com preempção** por quantum; ao estourar o quantum, o processo volta ao **fim** da fila de prontos. Ideal para **tempo compartilhado**.

**Aging:** aumentar gradualmente a prioridade de quem espera muito — antídoto clássico para starvation por prioridade.

### 1.5 Comunicação entre processos (IPC)

Processos têm espaços separados → precisam de mecanismos explícitos:

| Mecanismo | Flash |
|-----------|-------|
| **Pipe** | Fluxo unidirecional; saída de um → entrada de outro |
| **Fila / troca de mensagens** | Send/receive; pode ser síncrona ou com buffer |
| **Memória compartilhada** | Região mapeada nos dois; **rápida**, exige sincronização |
| **Sinais** | Notificação assíncrona (mais “evento” que canal de dados) |

Threads do mesmo processo já compartilham memória → IPC “barato”, mas **race condition** se não sincronizar.

### 1.6 Sincronização: seção crítica, mutex, semáforo

**Condição de corrida:** resultado depende da intercalação das execuções.  
**Seção crítica:** trecho que lê/escreve recurso compartilhado mutável.  
**Exclusão mútua:** no máximo um fluxo na seção crítica por vez.

**Mutex:** trava binária (livre/ocupado) — uma thread/processo por vez.  
**Semáforo:** inteiro + operações atômicas clássicas **P/wait** (decrementa; pode bloquear) e **V/signal** (incrementa; pode acordar). Semáforo **binário** ≈ mutex; semáforo **contagem** controla N instâncias de um recurso.

Flash FCC/Cesgranrio: semáforos servem para garantir **exclusão mútua** (sinalizar antes e depois do uso do recurso comum). **Não** garantem sozinhos “zero deadlock” — uso incorreto pode **criar** impasse.

Outros: monitores, variáveis de condição, *spinlocks* (espera ocupada — útil em seções muito curtas / multiprocessadores).

### 1.7 Deadlock (impasse)

**Deadlock:** conjunto de processos/threads **permanentemente bloqueados**, cada um esperando recurso detido por outro do conjunto.

**Condições de Coffman** (todas **simultâneas**):

| # | Nome | Ideia |
|---|------|-------|
| 1 | **Exclusão mútua** | Recurso não compartilhável: um por vez |
| 2 | **Posse e espera** (*hold and wait*) | Segura recurso e pede outro |
| 3 | **Não preempção** | Recurso só sai voluntariamente |
| 4 | **Espera circular** | Ciclo P1→P2→…→P1 no grafo de espera |

Prevenir = **negar pelo menos uma** condição (ex.: ordenar tipos de recurso → quebra espera circular; exigir alocação “tudo de uma vez” → quebra posse-e-espera; permitir preempção de recursos → quebra não-preempção).

| Estratégia | Ideia |
|------------|-------|
| **Prevenção** | Estrutura o sistema para tornar impossível alguma condição |
| **Evitação** | Aloca com cuidado (ex.: banco do banqueiro) olhando estado seguro |
| **Detecção + recuperação** | Permite; detecta ciclo; aborta/rollback/preempta |
| **Avestruz** | Ignora (custo de tratar > benefício) — **não** “reinicia tudo automaticamente” |

### 1.8 Starvation × deadlock

| | **Deadlock** | **Starvation (inanição)** |
|---|--------------|---------------------------|
| Essência | Espera **circular**/mútua; ninguém do conjunto progride | Um (ou alguns) **nunca** são atendidos |
| Exemplo | A tem impressora, quer teclado; B tem teclado, quer impressora | P3 de baixa prioridade nunca roda enquanto P1↔P2 trocam mensagens |
| Mitigação | Quebrar Coffman; detecção | FIFO justo; **aging**; limitar prioridade absoluta |

FCC TRT-12/2023: processo de menor prioridade sempre preterido ao chamar o kernel → **starvation**; correção citada: atendimento em ordem **FIFO** (não LIFO).

### 1.9 O que a FCC tipicamente testa

- Itens **privados** de thread: PC, registradores, pilha, estado.  
- Deadlock = quatro condições; nomear **não preempção** pelo enunciado.  
- Starvation ≠ deadlock.  
- RR = quantum + preempção + fim da fila.  
- Semáforo/mutex → exclusão mútua (não “é pipe”, não “é PID”).  
- Troca de contexto entre threads do mesmo processo é tipicamente **mais rápida** que entre processos.

### 1.10 Mini-síntese

```
Processo = espaço + PCB
Thread  = PC+regs+pilha+estado | compartilha espaço/globais/arquivos/...
CPU     = FCFS · SJF · prioridade · RR(quantum) · multinível
Sync    = seção crítica → mutex/semáforo (P/V)
Deadlock= exclusão + hold&wait + não-preempção + espera circular
Starvation = fome sem (necessariamente) ciclo → aging / FIFO
```

---

## 2. Questões de treino (1–15)

**1.** A melhor descrição de **processo**, no nível cobrável de Analista TI, é:

A) Apenas o arquivo `.exe` no disco  
B) Sinônimo estrito de *thread* em todos os SO  
C) Somente o conjunto de registradores da CPU  
D) Uma partição de swap obrigatória  
E) Um programa em execução com contexto gerenciado pelo SO  

**2.** Entre threads do **mesmo** processo, o item tipicamente **compartilhado** é:

A) Contador de programa de cada thread  
B) Espaço de endereçamento (variáveis globais inclusas)  
C) Pilha privada de cada thread  
D) Conjunto de registradores de cada thread  
E) Estado de execução individual de cada thread  

**3.** Itens tipicamente **privados** por thread:

A) Contador de programa, registradores, pilha e estado  
B) Espaço de endereçamento e arquivos abertos  
C) Variáveis globais e processos filhos  
D) Sinais, tratadores e informação de contabilidade  
E) Alarmes pendentes e espaço de endereçamento  

**4.** No Round-Robin, se o quantum expira e o processo ainda não terminou:

A) O processo é abortado definitivamente  
B) Ganha automaticamente prioridade máxima permanente  
C) Continua na CPU até E/S, sem preempção possível  
D) Sofre preempção e volta ao fim da fila de prontos  
E) É convertido em thread de usuário obrigatoriamente  

**5.** A condição de Coffman em que o recurso **só é liberado voluntariamente** pelo detentor chama-se:

A) Exclusão mútua  
B) Posse e espera  
C) Não preempção  
D) Espera circular  
E) Aging  

**6.** Dois processos bloqueados, cada um esperando recurso que o outro segura, formando ciclo, caracteriza:

A) Apenas thrashing  
B) Apenas compactação de memória  
C) Segmentação pura  
D) Page fault obrigatório  
E) Deadlock (impasse)  

**7.** Processo de baixa prioridade que **nunca** executa porque há sempre outro de prioridade maior ativos ilustra:

A) Starvation (inanição)  
B) Obrigatoriamente as quatro condições de Coffman  
C) Swapping bem-sucedido  
D) TLB hit  
E) DMA  

**8.** Semáforos, no uso clássico de seção crítica, destinam-se sobretudo a garantir:

A) Que o PID seja único na rede  
B) Exclusão mútua no acesso ao recurso compartilhado  
C) Que pipes substituam o escalonador  
D) Eliminação automática de toda starvation  
E) Que a MMU ignore endereços virtuais  

**9.** Em relação a processo × thread, assinale a afirmativa **correta**:

A) Criar processo é sempre mais barato que criar thread  
B) Threads de processos distintos compartilham o mesmo PCB por padrão  
C) Processos distintos compartilham, por padrão, o mesmo espaço de endereçamento  
D) Chavear threads do mesmo processo costuma ser mais rápido que chavear processos  
E) Thread nunca precisa de sincronização porque não há estado compartilhado  

**10.** *Aging* serve principalmente para:

A) Aumentar o tamanho da página  
B) Detectar ciclone no grafo DNS  
C) Mitigar starvation elevando prioridade de quem espera muito  
D) Substituir a necessidade de mutex  
E) Garantir exclusão mútua física no disco  

**11.** Pipe, como IPC, é melhor descrito como:

A) Algoritmo idêntico ao SJF  
B) Quatro condições de Coffman  
C) Partição fixa de memória  
D) Mutex binário puro  
E) Canal que liga tipicamente a saída de um processo à entrada de outro  

**12.** Qual par está **corretamente** associado?

A) Hold and wait → processo retém recurso e pede outro  
B) Exclusão mútua → qualquer recurso é sempre compartilhado por N processos  
C) Espera circular → ausência total de dependências  
D) Não preempção → SO retira recurso a qualquer instante sem critério  
E) Starvation → sinônimo obrigatório de page fault  

**13.** Em sistema com prioridade estrita **sem** aging, o risco mais direto é:

A) Impossibilidade de usar Round-Robin em qualquer SO  
B) Starvation de processos de baixa prioridade  
C) Eliminação completa de deadlocks  
D) Transformação automática de threads em processos  
E) Proibição de IPC por mensagens  

**14.** Assinale a estratégia que **não** descreve tratamento clássico de deadlock:

A) Prevenir negando uma condição de Coffman  
B) Evitar com alocação cuidadosa (estado seguro)  
C) Detectar ciclo e recuperar (abortar/preemptar)  
D) Ignorar o problema (política do avestruz)  
E) Afirmar que deadlock só existe em máquinas mononucleares e nunca em SMP  

**15.** Síntese correta:

A) RR elimina Coffman automaticamente  
B) Mutex compartilha PC entre todas as threads  
C) Processo tem espaço próprio; threads do processo compartilham espaço; deadlock ≠ starvation; RR usa quantum com preempção  
D) Starvation exige necessariamente espera circular  
E) Semáforo substitui o conceito de processo  

---

### Gabarito comentado (treino 1–15)

1. **E** — Programa em execução + contexto.  
2. **B** — Espaço/globais compartilhados.  
3. **A** — PC, regs, pilha, estado.  
4. **D** — Preempção → fim da fila.  
5. **C** — Não preempção.  
6. **E** — Deadlock.  
7. **A** — Starvation.  
8. **B** — Exclusão mútua.  
9. **D** — Troca de contexto mais leve entre threads.  
10. **C** — Aging contra fome.  
11. **E** — Pipe liga fluxos.  
12. **A** — Hold and wait.  
13. **B** — Starvation por prioridade.  
14. **E** — Deadlock não é exclusivo de uniprocessador.  
15. **C** — Síntese do arquivo.

---

## 3. Questões de concurso (16–25)

**16.** (Cesgranrio · BR Distribuidora/2011 · Sistemas Operacionais)  
Os semáforos são usados em programação concorrente para evitar que dois ou mais processos acessem sua seção crítica simultaneamente. Assim, os semáforos são usados para garantir

A) a exclusão mútua, onde cada processo deverá sinalizar imediatamente antes e, imediatamente após, usar um recurso comum a ambos.  
B) os pipes, que funcionam ligando a saída de um processo com a entrada de outro.  
C) o PID, a identificação única de cada processo.  
D) que não haverá deadlock, ou seja, quando um processo espera indefinidamente por um recurso de outro.  
E) que a máquina virtual gerencie os processos do Sistema Operacional e o hardware da máquina corretamente.  

**17.** (FCC · TRT-14/2022 · Analista Judiciário — TI)  
No contexto de Sistemas Operacionais, quando ocorre um impasse, de dois ou mais processos, que impede a execução devido à espera por um evento que só pode ser gerado por algum dos mesmos processos, isso resulta em

A) starvation.  
B) deadlock.  
C) paginação.  
D) segmentação.  
E) swapping.  

**18.** (FCC · DPE-RS/2017 · Analista — Desenvolvimento de Sistemas)  
Considere a figura de gerenciamento de processos e fluxos de execução pelo SO (parte superior: um processo com um único fluxo; parte inferior: o mesmo processo com threads A e B). Neste contexto é correto afirmar que

A) a parte superior da figura indica que o SO é monoprocessado e multithread e a parte inferior indica que o SO é multi-processado e multithread.  
B) a parte superior da figura mostra um processo monothread e a parte inferior mostra o mesmo processo com as threads A e B. Geralmente é mais rápido chavear entre threads de um mesmo processo do que entre processos.  
C) é mais rápido criar um processo do que criar uma thread, bem como é mais rápido terminar um processo do que terminar uma thread.  
D) threads impedem o paralelismo de atividades e ajudam na melhor organização dos programas.  
E) a implementação de threads pode ser feita no nível de usuário (*user-level threads*) e no nível de processo (*process-level threads*) para todos os sistemas operacionais.  

**19.** (FCC · TRT-12/2023 · Analista Judiciário — TI)  
Durante a execução de processos em um sistema operacional, uma Analista observou que processos de maior prioridade apareciam sempre que um processo de menor prioridade ativo queria chamar o kernel. Nesse caso, estava ocorrendo o

A) starvation, que é o mesmo que thread interference, mas uma forma de correção disso é a utilização da ordem de atendimento dos processos escritores por meio de uma fila do tipo último que entra, primeiro que sai (LIFO — *last in, first out*).  
B) deadlock, que é o mesmo que intervention, pois um processo trava o outro mantendo o impasse. Para evitar isso utiliza-se o thread attempt para análise e correção.  
C) thread lock interference afetando a prioridade do fluxo de execução. Uma possível solução para essa interferência é priorizar as threads utilizando uma fila do tipo último que entra, primeiro que sai (LIFO — *last in, first out*).  
D) starvation, mas é possível evitá-lo mantendo a ordem de atendimento dos processos escritores utilizando uma fila do tipo primeiro que entra, primeiro que sai (FIFO — *first in, first out*).  
E) deadlock, que é o mesmo que lock interference, pois um processo trava o outro mantendo o impasse. Para evitar isso utiliza-se o *release lock* para liberação.  

**20.** (FCC · TRT-4/2022 · Analista Judiciário — TI)  
Considere os seguintes itens de informação: processos filhos; pilha; arquivos abertos; alarmes pendentes; contador de programa; registradores; estado; espaço de endereçamento; variáveis globais; sinais e tratadores de sinais; informação de contabilidade. São alguns dos itens de informação privados por threads,

A) contador de programa; estado e; arquivos abertos.  
B) informação de contabilidade; registradores e; pilha.  
C) pilha; alarmes pendentes; estado e; registradores.  
D) contador de programa; registradores; pilha e; estado.  
E) alarmes pendentes; sinais e tratadores de sinais e; contador de programa.  

**21.** (FCC · TCE-AP/2012 · Analista de Controle Externo — TI)  
Em relação às condições para que ocorra um deadlock, àquela em que recursos concedidos previamente a um processo não podem ser forçosamente tomados desse processo e sim, explicitamente liberados por ele, denomina-se condição de

A) preempção.  
B) exclusão mútua.  
C) posse e espera.  
D) não preempção.  
E) espera circular.  

**22.** (FCC · TRE-PR/2017 · Técnico Judiciário — Operação de Computadores)  
Considere as situações abaixo, no contexto de Sistemas Operacionais.  
I. Dois ou mais processos estão impedidos de continuar suas execuções ficando bloqueados, esperando uns pelos outros.  
II. Um processo nunca é executado, pois processos de prioridade maior sempre o impedem de entrar em execução.  
Nos casos I e II estão ocorrendo, respectivamente, o que é denominado

A) blockade e aging.  
B) deadlock e turnaround.  
C) throughput e starvation.  
D) preemption e turnaround.  
E) deadlock e starvation.  

**23.** (FCC · MPE-SE/2013 · Analista — Informática)  
Uma situação de deadlock pode surgir se as 4 condições a seguir ocorrerem simultaneamente em um sistema:  
I. Pelo menos um recurso deve ser alocado em modo não compartilhável, isto é, apenas um processo de cada vez pode usar o recurso. Se outro processo solicitar este recurso, o processo solicitante deve ser atrasado até o recurso ter sido liberado.  
II. Pelo menos um recurso deve estar alocado a um processo e este processo estar aguardando para adquirir recursos adicionais que, no momento, estejam sendo mantidos por outros processos.  
III. Recursos não podem ser interceptados, isto é, um recurso só pode ser liberado voluntariamente pelo processo que o estiver mantendo após esse processo ter concluído sua tarefa.  
IV. Deve existir um conjunto de processos (P1, P2, … Pn) em espera, de tal modo que P1 esteja esperando por um recurso que está alocado a P2, … e Pn esteja esperando por um recurso que está alocado a P1.  
As descrições das condições I, II, III e IV correspondem, correta e respectivamente, às denominações:

A) I — inclusão mútua; II — espera circular; III — existência de preempção; IV — posse e espera  
B) I — espera contínua; II — inclusão mútua; III — posse e espera; IV — existência de preempção  
C) I — posse e espera; II — exclusão mútua; III — espera circular; IV — inexistência de preempção  
D) I — posse e espera; II — existência de preempção; III — exclusão mútua; IV — espera contínua  
E) I — exclusão mútua; II — posse e espera; III — inexistência de preempção; IV — espera circular  

**24.** (FCC · DPE-AM/2018 · Assistente Técnico de Suporte)  
Em um sistema operacional típico de um computador, três processos se encontram na seguinte situação: o processo P1 envia mensagem a P2; P2 responde a P1; e assim sucessivamente; o processo P3 está bloqueado e só sai dessa situação quando receber mensagem de P1. Considerando que a prioridade de P3 é menor que as de P1 e P2, tem-se que

A) a cada troca de mensagens entre P1 e P2 as respectivas prioridades automaticamente serão reduzidas, e quando elas forem inferior à do processo P3, esse será executado.  
B) após passar 1 segundo bloqueado, o relógio de tempo real do sistema operacional automaticamente dará oportunidade de o processo P3 ser desbloqueado.  
C) o processo P3 sairá dessa situação assim que uma interrupção qualquer ocorra.  
D) ocorrerá uma situação conhecida como impasse (*deadlock*).  
E) ocorrerá uma situação conhecida como inanição (*starvation*).  

**25.** (FCC · TCE-CE/2008 · Analista de Controle Externo — Auditoria de TI)  
Criado para sistemas de tempo compartilhado e semelhante ao escalonamento FCFS, porém com preempção para alternar entre processos. Trata-se do algoritmo de escalonamento

A) Multilevel Queue.  
B) em tempo real.  
C) por prioridade.  
D) Multilevel Feedback-Queue.  
E) Round-Robin.  

---

### Gabarito comentado (concurso 16–25)

16. **A** — Semáforo → exclusão mútua (não garante sozinho ausência de deadlock).  
17. **B** — Impasse clássico = deadlock.  
18. **B** — Mono × multithread; chaveamento entre threads mais rápido.  
19. **D** — Starvation; mitigação com FIFO (não LIFO).  
20. **D** — Privados: PC, registradores, pilha, estado.  
21. **D** — Não preempção.  
22. **E** — I deadlock; II starvation.  
23. **E** — Quatro condições de Coffman na ordem canônica.  
24. **E** — P3 “morre de fome” enquanto P1↔P2 interagem.  
25. **E** — Round-Robin = FCFS + preempção por quantum.

---

## 4. Como usar este arquivo

1. Fechar de cor: **privados da thread** + **Coffman** + **deadlock × starvation** + **RR**.  
2. Treino 1–15 → gabarito.  
3. Concurso 16–25 sem consultar (priorizar 17–20 e 23, perfil TRT/FCC Analista).  
4. Ligar depois à **memória/E/S** (`02-so-memoria-virtual-es.md`) sem misturar conceitos.  
5. Caderno de erros: anotar confusão “arquivos abertos = privado” e “starvation = deadlock”.

---

*Arquivo 01 · SO processos/threads/deadlocks · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 9.*

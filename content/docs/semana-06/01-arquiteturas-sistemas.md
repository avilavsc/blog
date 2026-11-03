+++
title = '01 — Arquiteturas de sistemas'
date = '2026-10-15'
draft = false
weight = 1
tags = ['semana-06', 'arquiteturas', 'sistemas']
categories = ['estudo']
+++
# Arquiteturas de sistemas · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 01 — Arquiteturas multicamadas, cliente-servidor, SOA e objetos distribuídos  
**Escopo deste arquivo:** modelos cobráveis do edital em “Arquiteturas de sistemas”: multicamadas (2/3/n-tier), cliente-servidor (cliente magro/gordo), fundamentos de SOA (serviço, acoplamento fraco, ESB, contraste leve com microsserviços) e objetos distribuídos (interface, stub/esqueleto, CORBA/RMI no nível de prova).  
**Fora de escopo aqui:** detalhe de Eureka/Zuul/Swagger/MapStruct (semana 1); PHP/C; MER/normalização e SGBDs (arquivos 04–06 desta semana); redes e segurança (semanas 7–8).

---

## 1. Material

### 1.1 Por que a FCC cobra este bloco

No edital do TRT4 (código 14), a linha é explícita: **multicamadas, cliente-servidor, objetos distribuídos; fundamentos de SOA**. Não é “história da computação”: a banca quer **papéis das camadas**, **onde fica a regra de negócio**, **o que o cliente faz vs o servidor**, **o que é um serviço em SOA** e **o que o cliente enxerga em objetos distribuídos** (a interface, não a implementação).

Cartão mental:

| Modelo | Ideia cobrável |
|--------|----------------|
| Cliente-servidor | Papéis assimétricos: cliente solicita; servidor atende |
| Multicamadas | Separação de responsabilidades (apresentação / negócio / dados) |
| SOA | Funções de negócio como **serviços** reutilizáveis e fracamente acoplados |
| Objetos distribuídos | Objetos remotos acessados pela **interface**; localização/implementação encapsuladas |

### 1.2 Cliente-servidor: o núcleo

Na arquitetura **cliente-servidor**, há pelo menos dois papéis:

1. **Cliente** — inicia a interação; apresenta interface ao usuário; envia requisições.  
2. **Servidor** — aguarda requisições; processa (em maior ou menor grau); responde; tipicamente gerencia recursos compartilhados (dados, arquivos, impressão, autenticação).

Características cobráveis:

- Relação **assimétrica** (não é peer-to-peer: em P2P, nós tendem a papéis simétricos).  
- Um servidor pode atender **múltiplos** clientes.  
- Escalabilidade e administração tendem a se concentrar no lado servidor (dependendo do modelo magro/gordo).  
- Não confundir “cliente-servidor” com “só web”: desktop + SGBD remoto também é cliente-servidor.

**Armadilha:** dizer que “servidor = máquina grande e cliente = máquina pequena”. Em prova, o que define é o **papel**, não o porte do hardware.

### 1.3 Cliente magro × cliente gordo (thin × fat/thick)

Em arquiteturas de **duas camadas**, a distribuição típica cobrada (Sommerville e clones de banca) é:

| | **Cliente magro (thin)** | **Cliente gordo (fat/thick)** |
|---|--------------------------|-------------------------------|
| Cliente | Principalmente **apresentação** (ex.: browser) | **Apresentação + lógica** da aplicação |
| Servidor | Processamento da aplicação **e** gerenciamento de dados | Em geral só (ou sobretudo) **gerenciamento de dados** |
| Carga no servidor/rede | **Alta** (desvantagem clássica do magro) | Menor no servidor de aplicação; mais carga nos clientes |
| Atualização | Concentrada no servidor (mais simples operacionalmente) | Precisa atualizar muitos clientes |
| Legado | Cliente magro 2-tier é caminho típico ao evoluir mainframe/legado para C/S | Clientes ricos/desktop clássicos |

Flash:

- **Magro** → “cliente burro de apresentação”; servidor faz quase tudo.  
- **Gordo** → cliente “inteligente”; servidor vira sobretudo banco/arquivo.

**Erro clássico:** chamar de “mais complexa” a arquitetura de **duas** camadas. Em doutrina de prova, **duas camadas** é a forma **mais simples**; a complexidade cresce com **três ou n camadas** (servidor de aplicação intermediário etc.).

### 1.4 Multicamadas (multi-tier / n-tier)

**Camada** = agrupamento lógico de responsabilidades. Pode coincidir com máquina física distinta, mas **camada lógica ≠ necessariamente máquina física**.

Modelo mais cobrado: **3 camadas (3-tier)**:

| Camada | Responsabilidade típica |
|--------|-------------------------|
| **Apresentação** (UI / View) | Interface, captura de entrada, exibição |
| **Negócio** (lógica / aplicação) | Regras, validações, orquestração de casos de uso |
| **Dados** (persistência) | Acesso a SGBD, arquivos, repositórios |

Fluxo típico cobrado: apresentação → negócio → dados (e volta). **Não** é “triângulo” em que apresentação fala direto com o banco “sempre que quiser” — o ponto da multicamada é **isolar** a regra e o acesso a dados.

Vantagens cobráveis da separação em camadas:

- Manutenção e evolução localizadas (mudar UI sem reescrever regras; mudar SGBD sem reescrever telas).  
- Reutilização da lógica de negócio por diferentes clientes (web, mobile, outro sistema).  
- Escalabilidade seletiva (escalar servidor de aplicação sem necessariamente escalar o de apresentação).  
- Segurança: clientes não precisam de conexão JDBC/ODBC direta ao banco em 3-tier típico.

**2-tier vs 3-tier (anti-confusão):**

| Aspecto | 2-tier | 3-tier |
|---------|--------|--------|
| Onde está a regra | No cliente (gordo) ou misturada no servidor de dados | Preferencialmente na **camada de negócio** intermediária |
| Alteração no BD | Em cliente gordo, pode exigir republicação em **muitos** clientes | Alteração tende a ficar no servidor de aplicação/dados |
| Comunicação | Cliente ↔ servidor de dados (direta) | Cliente ↔ app server ↔ dados |

### 1.5 Multicamadas × MVC (armadilha FCC)

A FCC já misturou **3-tier** com **MVC** na mesma questão (TRE-PB/2015). Memorize a distinção:

| | **3-tier** | **MVC** |
|---|------------|---------|
| Natureza | Arquitetura em **camadas / tiers** de implantação/responsabilidade | Padrão de **organização de UI/aplicação** |
| Onde ficam as regras | Camada de **Negócio** | No **Model** (visão típica de prova) |
| Papéis | Apresentação / Negócio / Dados | Model / View / Controller |

Afirmações **falsas** que a banca planta:

- “MVC é o principal modelo de arquitetura em 3 camadas e o acesso a BD fica no Controller” → **não**.  
- “Em 3-tier o fluxo é triangular e todas as camadas falam direto com todas” → **não** (quebra o propósito do isolamento).  
- “Em 3-tier, qualquer alteração no BD obriga alterar todas as máquinas cliente” → isso cheira a **2-tier cliente gordo**, não ao benefício do 3-tier.

Síntese cobrável: **3-tier → regras na Camada de Negócio; MVC → regras no Model.**

### 1.6 SOA — fundamentos

**SOA (Service-Oriented Architecture)** organiza capacidades de negócio como **serviços**:

1. **Independente** — função com interface bem definida; não deve depender do estado interno de outros processos.  
2. **Reutilizável** — o mesmo serviço pode ser composto em vários processos/aplicações.  
3. **Fracamente acoplado** — consumidor conhece o **contrato** (interface), não a implementação.  
4. **Descoberta/composição** — serviços podem ser publicados, encontrados e orquestrados (visão de prova).

Definição de serviço (flash FCC clássica): função independente que **aceita requisições** e **devolve respostas** por interface padronizada; pode realizar partes discretas de um processo (ex.: validar crédito, emitir certidão) **sem depender do estado** de outras funções.

O que a SOA **não** é, em prova:

- Sinônimo obrigatório de “só SOAP”.  
- Sinônimo obrigatório de “só microsserviços”.  
- Licença para **forte acoplamento** (“serviços fortemente acoplados” = distrator clássico).  
- Oposto de Web Services: Web Services são **uma forma comum** de expor serviços; dizer “ao contrário das Web Services, na SOA…” costuma ser **falso**.

Benefícios cobráveis: flexibilidade, reutilização, integração entre sistemas heterogêneos, evolução modular, comunicação por **web services** ou **mensageria assíncrona**.

### 1.7 ESB e o discurso de prova

**ESB (Enterprise Service Bus)** = barramento de integração: roteamento, transformação de mensagens, mediação de protocolos, redução de conexões **ponto a ponto**.

Nuances FCC:

- Em enunciados antigos (ex.: METRÔ-SP/2008), a banca associou ESB a “implementa SOA”.  
- Em enunciado posterior (TJ-PE/2012), cobrou como **INCORRETO** dizer que “usar ESB transforma automaticamente a arquitetura em SOA / ESB implementa SOA”.

Para o TRT4/2026, o caminho seguro:

- ESB é **padrão/infra de integração** frequentemente ligado a SOA clássica.  
- **SOA ≠ ESB**: dá para ter serviços sem ESB; ESB sozinho não “batiza” SOA se não houver serviços com contratos e acoplamento fraco.  
- Em contraste com microsserviços (semana 1): SOA clássica de prova enfatiza **integração mais centralizada/pesada**; microsserviços enfatizam **APIs leves + autonomia de deploy**.

### 1.8 SOA × cliente-servidor × multicamadas × microsserviços

Não são mutuamente exclusivos — a banca gosta de **combinações**:

| Combinação | Leitura |
|------------|---------|
| Cliente-servidor + multicamadas | C/S descreve papéis; multicamadas descreve **como** fatiar o servidor/aplicação |
| SOA sobre C/S saturado | Modernização: serviços independentes, integração padronizada, reutilização |
| Microsserviços + princípios SOA | Granularidade fina + interoperabilidade por contratos; **BD por serviço** (não BD único compartilhado) |
| Multicamadas “só” | Separação apresentação/negócio/dados — **não** resolve sozinha falha isolada por módulo de negócio nem interoperabilidade institucional |

Quando o enunciado pede modernização com **desacoplamento**, **integração externa**, **falha isolada** e **escala**, a resposta tipicamente caminha para **SOA / microsserviços com persistência independente** — não para “microsserviços com BD compartilhado” nem “SOA com repositório único obrigatório de tudo”.

### 1.9 Objetos distribuídos

Em arquitetura de **objetos distribuídos**, componentes (objetos) residem em processos/máquinas distintas e são invocados **remotamente** como se fossem locais (transparência relativa).

Característica **mais cobrada** (FCC TJ-CE/2022):

> **Apenas a interface do objeto é visível** ao cliente.

Consequências:

- Implementação interna encapsulada.  
- Cliente usa **referência/proxy/stub**; o middleware resolve localização e comunicação.  
- A referência lógica **esconde** o endereço de rede do programador, mas o **sistema** precisa do endereço para entregar a mensagem — distrator: “a referência não necessita do endereço de rede” (falso no sentido absoluto).  
- Protocolos de alto nível (RMI, CORBA/IIOP etc.) existem precisamente para **não** forçar o desenvolvedor a lidar só com sockets/protocolos de baixo nível.  
- Cliente-servidor **não** impede objetos distribuídos; ao contrário, a interação remota frequentemente é C/S (ou cliente e servidor em papéis que se alternam em callbacks).

Vocabulário mínimo:

| Termo | Ideia |
|-------|-------|
| **Interface** | Contrato público do objeto remoto |
| **Stub / proxy** | Representante local no cliente |
| **Skeleton / servant** | Lado servidor que despacha a invocação |
| **CORBA** | Padrão clássico de ORB/objetos distribuídos heterogêneos |
| **RMI** | Invocação remota típica no ecossistema Java |

**Armadilha moderna:** enunciados inventam “objetos trapper” ou “compartilhar estado/métodos sem protocolos intermediários”. Objetos distribuídos **dependem** de middleware/protocolos; não são mágica sem camada de remoting.

### 1.10 O que a FCC tipicamente testa neste arquivo

1. Papéis cliente × servidor; magro × gordo.  
2. 2-tier × 3-tier: onde fica a regra; se alteração no BD “quebra” todos os clientes.  
3. 3-tier × MVC: Negócio × Model.  
4. SOA: serviço independente, reutilização, **acoplamento fraco**, interface padronizada.  
5. ESB: integração / redução ponto a ponto — sem confundir cegamente com “ESB = SOA”.  
6. Objetos distribuídos: **só a interface é visível**.  
7. Escolha arquitetural em modernização judiciária: SOA/serviços > monólito/P2P mal descrito/C-S tradicional “milagroso”.

### 1.11 Mini-síntese (cartão mental)

> **C/S** = quem pede × quem atende. **Magro** = UI no cliente; resto no servidor. **Gordo** = lógica no cliente; dados no servidor. **3-tier** = apresentação / **negócio** / dados. **MVC** ≠ 3-tier; regras no **Model**. **SOA** = serviços reutilizáveis, interface clara, acoplamento fraco (± ESB na visão clássica). **Objetos distribuídos** = cliente vê **só a interface**; stub esconde a rede.

---

## 2. Questões de treino (1–15)

**1.** Em arquitetura cliente-servidor, o elemento que melhor define o papel do **cliente** é:

A) Armazenar obrigatoriamente o banco corporativo único  
B) Iniciar requisições e, em geral, apresentar a interface ao usuário  
C) Impedir qualquer comunicação em rede  
D) Substituir o DNS em toda a organização  
E) Ser, por definição, a máquina de maior porte do datacenter  

**2.** No modelo de **cliente magro** de duas camadas, é correto afirmar que:

A) O cliente concentra a lógica de negócio e o servidor só exibe telas  
B) Não existe servidor  
C) O gerenciamento de dados ocorre exclusivamente no browser, sem servidor  
D) O cliente executa sobretudo a apresentação; processamento da aplicação e dados tendem a ficar no servidor  
E) Toda regra de negócio deve ser duplicada em cada estação e no mainframe ao mesmo tempo, por definição  

**3.** Uma desvantagem clássica do cliente magro é:

A) Impor carga elevada de processamento sobre o servidor e a rede  
B) Eliminar a necessidade de rede  
C) Impedir o uso de navegadores  
D) Tornar impossível qualquer autenticação  
E) Proibir o uso de HTTPS  

**4.** No modelo de **cliente gordo**, tipicamente:

A) O servidor executa toda a lógica e o cliente só renderiza HTML estático obrigatoriamente  
B) Não há gerenciamento de dados em lugar nenhum  
C) O cliente implementa lógica da aplicação e interação com o usuário; o servidor cuida sobretudo dos dados  
D) O cliente não pode exibir interface  
E) A arquitetura deixa de ser cliente-servidor e vira obrigatoriamente P2P  

**5.** Em arquitetura **3-tier**, as regras de negócio devem preferencialmente residir:

A) Exclusivamente no driver JDBC do browser  
B) Somente em planilhas locais do usuário  
C) Apenas no cabo de rede  
D) Obrigatoriamente na camada de apresentação, misturadas ao HTML  
E) Na camada de **negócio** (aplicação), intermediária entre apresentação e dados  

**6.** Assinale a afirmação **incorreta** sobre multicamadas:

A) Obriga, por definição, que apresentação, negócio e dados residam no **mesmo** processo único sem possibilidade de distribuição  
B) Favorece separação de responsabilidades  
C) Pode facilitar manutenção e reutilização da lógica  
D) Permite escalar camadas de forma mais seletiva que um empacotamento único indiferenciado  
E) Em 3-tier típico, o cliente não precisa falar JDBC direto com o SGBD  

**7.** Comparando 2-tier cliente gordo e 3-tier, uma vantagem típica do 3-tier é:

A) Exigir republicação da aplicação em todas as estações a cada mudança de schema  
B) Proibir qualquer servidor de aplicação  
C) Eliminar a camada de dados  
D) Concentrar regras no servidor de aplicação, reduzindo a necessidade de atualizar lógica em massa nos clientes  
E) Tornar o fluxo “triangular” obrigatório entre todas as camadas sem mediação  

**8.** No contraste cobrável **3-tier × MVC**:

A) São sinônimos rigorosos em qualquer enunciado  
B) No MVC, o Controller é o único lugar permitido para persistência e regras, por definição FCC  
C) MVC extingue a camada de apresentação  
D) 3-tier proíbe o uso de Model  
E) Em 3-tier, regras ficam na camada de Negócio; no MVC, a visão típica de prova coloca regras no Model  

**9.** Em SOA, um **serviço** é melhor descrito como:

A) Uma planilha compartilhada sem interface  
B) Um monólito único sem contratos  
C) Função independente com interface padronizada, que recebe requisições e devolve respostas, sem depender do estado de outras funções  
D) Qualquer classe privada sem exposição  
E) Um cabo cruzado entre switches  

**10.** Qual característica é **coerente** com fundamentos de SOA?

A) Reutilização de serviços em diferentes processos/áreas sem reescrever a tecnologia subjacente a cada uso  
B) Serviços fortemente acoplados à implementação do consumidor  
C) Proibição de interfaces padronizadas  
D) Obrigatoriedade de um único banco compartilhado por todos os serviços como definição de SOA  
E) Extinção de qualquer contrato entre provedor e consumidor  

**11.** Em relação a Web Services e SOA, a leitura mais segura em prova é:

A) SOA e Web Services são conceitos que se excluem mutuamente  
B) SOA só existe se for proibido HTTP  
C) Toda página HTML estática é, por definição, um ESB  
D) Web Services são mecanismo comum de exposição/integração de serviços em cenários SOA  
E) SOA exige abandono de qualquer interface  

**12.** O **ESB**, no discurso clássico de integração, associa-se principalmente a:

A) Aumentar ao máximo conexões ponto a ponto permanentes  
B) Substituir o conceito de serviço por arquivos .txt em pen-drive  
C) Proibir transformação de mensagens  
D) Ser o único nome possível para “cliente magro”  
E) Mediar integração (roteamento/transformação/conectividade), reduzindo a necessidade de dezenas de ligações ponto a ponto  

**13.** Em arquitetura de **objetos distribuídos**, a característica fundamental cobrada pela FCC é:

A) O cliente enxerga e altera livremente todo o código-fonte do objeto remoto  
B) Apenas a **interface** do objeto é visível ao cliente  
C) São necessários exclusivamente protocolos de baixo nível, sem abstrações  
D) O modelo cliente-servidor impede, por si só, o uso de objetos distribuídos  
E) A referência ao objeto dispensa qualquer mecanismo de localização em rede no sistema subjacente  

**14.** Stub/proxy em objetos distribuídos serve para:

A) Apagar o banco de dados do servidor  
B) Proibir qualquer chamada de método  
C) Representar localmente o objeto remoto, escondendo detalhes de remoting do código cliente  
D) Transformar SOA em monólito automaticamente  
E) Substituir a camada de apresentação por um ESB  

**15.** Um tribunal quer modernizar sistema saturado em cliente-servidor, buscando módulos independentes, integração com outros sistemas judiciais e melhor escala/manutenção. A opção mais alinhada ao discurso recente da FCC é:

A) Peer-to-peer com um host fixo “servidor” e demais “clientes”, chamado incorretamente de P2P  
B) SOA com módulos/serviços independentes comunicando-se por web services ou mensageria assíncrona  
C) Monólito único como melhor escolha para escala e evolução modular  
D) Objetos “trapper” sem protocolos intermediários  
E) Cliente-servidor tradicional como garantia de crescer sem qualquer mudança de infraestrutura e com desacoplamento máximo via REST isolado  

---

### Gabarito comentado (treino 1–15)

1. **B** — Cliente solicita e apresenta UI; porte da máquina não define o papel.  
2. **D** — Magro = apresentação no cliente; resto no servidor.  
3. **A** — Carga no servidor/rede é a desvantagem clássica.  
4. **C** — Gordo = lógica no cliente; dados no servidor.  
5. **E** — Regras na camada de negócio.  
6. **A** — Multicamada **permite** distribuição; não obriga processo único.  
7. **D** — Lógica centralizada no app server.  
8. **E** — Negócio (3-tier) × Model (MVC).  
9. **C** — Serviço independente + interface + respostas.  
10. **A** — Reutilização / compartilhamento sem reescrever o subjacente a cada uso.  
11. **D** — Web Services como meio comum, não oposto.  
12. **E** — Barramento de integração; não maximizar ponto a ponto.  
13. **B** — Só a interface é visível (TJ-CE/2022).  
14. **C** — Proxy local do objeto remoto.  
15. **B** — SOA (TRT-1/2025 e similares).

---

## 3. Questões de concurso (16–25)

> Bloco com questões **reais** de provas (preferência FCC / TRT Analista TI), citadas. Gabarito só ao final.

**16.** (FCC – TJ-CE/2022 – Analista Judiciário – Ciência da Computação – Sistemas da Informação)  
Considerando as características de uma arquitetura de objetos distribuídos,

A) o modelo cliente-servidor é, por si só, um impeditivo para seu uso.  
B) um objeto pode ser cliente e servidor no mesmo momento.  
C) são necessários apenas protocolos de baixo nível para a interação entre os objetos.  
D) a referência ao objeto não necessita do endereço de rede.  
E) apenas a interface do objeto é visível.

**17.** (FCC – TRT-1/2025 – Analista Judiciário – TI)  
Um Tribunal Regional do Trabalho está modernizando seu sistema de gestão processual para melhorar a escalabilidade, segurança e manutenibilidade. A equipe de TI está avaliando diferentes arquiteturas para a nova solução e chegou à conclusão correta que

A) uma arquitetura peer-to-peer (P2P) é ideal, pois designa um host como servidor e as outras máquinas como clientes. Como o servidor precisa estar online o tempo todo, oferece boa conectividade, garantindo a escalabilidade necessária aos sistemas judiciais.  
B) a melhor abordagem é utilizar arquitetura baseada em objetos distribuídos, pois permite que componentes remotos compartilhem diretamente seus estados e métodos, sem a necessidade de protocolos intermediários, através de objetos do tipo trapper, garantindo eficiência dos sistemas judiciais.  
C) uma arquitetura monolítica é a melhor escolha, pois centraliza toda a lógica do sistema judicial em um único código-fonte, facilitando a escalabilidade, a evolução modular e a manutenção ao evitar complexidades associadas à comunicação entre serviços.  
D) uma arquitetura orientada a serviços (SOA) é a mais indicada, pois permite a criação de módulos independentes que podem se comunicar por web services ou mensageria assíncrona, facilitando a reutilização, a escalabilidade e a integração com outros sistemas judiciais.  
E) o modelo cliente-servidor tradicional é o mais indicado, pois separa interface e processamento de dados, garantindo que a aplicação possa crescer sem necessidade de modificação na infraestrutura, além de oferecer desacoplamento adequado e modularidade necessária para integração com múltiplos serviços judiciais a sistemas externos usando o REST.

**18.** (FCC – TRE-PB/2015 – Técnico Judiciário – Programação de Sistemas)  
No desenvolvimento de aplicações cliente-servidor, como os sites, é comum a utilização da arquitetura multicamadas. No modelo de arquitetura

A) MVC, que é o principal modelo de arquitetura em 3 camadas, as operações de acesso ao banco de dados são implementadas na camada de Controle.  
B) 3-tier o fluxo de comunicação entre as camadas é triangular, ou seja, todas as camadas podem se comunicar umas com as outras diretamente.  
C) 2-tier o banco de dados e toda a lógica da aplicação, incluindo regras de negócio, são executados na camada de dados no servidor, e apenas os arquivos de apresentação, são executadas no cliente.  
D) 3-tier, para toda a alteração no banco de dados no servidor, a aplicação precisa ser alterada em todas as máquinas cliente.  
E) 3-tier as regras de negócio são implementadas na Camada de Negócio, já no padrão MVC, no grupo Model.

**19.** (FCC – METRÔ-SP/2008 – Analista Trainee – Ciências da Computação)  
Sobre a SOA – Service-oriented architecture, considere:

I. A SOA fornece, essencialmente, uma estrutura na qual a lógica do aplicativo pode ser decomposta em pedaços ou serviços que podem ser reutilizados e modificados e aplicados em diferentes áreas dentro e fora da empresa sem ajustar a tecnologia subjacente, otimizando, dessa forma, o compartilhamento e reutilização dos componentes de TI.  
II. Ao contrário das Web Services, na SOA, duas áreas diferentes, com aplicativos e dados diferentes, e possivelmente sistemas operacionais ainda mais diferentes, podem compartilhar o serviço sem uma grande quantidade de codificações customizadas para fazê-lo funcionar.  
III. Utilizando a SOA, uma empresa pode construir sistemas de TI flexíveis, com serviços fortemente acoplados que podem ser compartilhados, modificados e permutados sem enfrentar dificuldades com a customização de tecnologias subjacentes, mesmo levando em conta a existência de grande quantidade de linhas de códigos da linguagem WSDL.  
IV. Na SOA, um serviço pode ser definido como uma função independente que aceita uma ou mais requisições e devolve uma ou mais respostas através de uma interface padronizada e bem definida. Serviços podem também realizar partes discretas de um processo, tal como editar ou processar uma transação, e não devem depender do estado de outras funções ou processos.

É correto o que consta APENAS em

A) II e III.  
B) I e IV.  
C) I e III.  
D) I, II e III.  
E) II, III e IV.

**20.** (FCC – MPE-AL/2026 – Analista – Desenvolvimento de Sistemas)  
Uma equipe de desenvolvimento de sistemas de um órgão público pretende modernizar seu ambiente sistêmico, atualmente baseado em uma estrutura cliente-servidor saturada. A nova solução deve permitir o desacoplamento de funcionalidades críticas, facilitar a integração com órgãos externos via protocolos padronizados e garantir que falhas em módulos específicos não paralisem o sistema por completo, prevendo ainda estratégia técnica que atenda aos requisitos de escalabilidade e integração institucional, que é:

A) Evolução para uma arquitetura de microsserviços com bancos de dados independentes, utilizando princípios de SOA para interoperabilidade com sistemas externos e mantendo camadas lógicas para a organização dos serviços internos.  
B) Manutenção da arquitetura cliente-servidor para os sistemas legados e adoção de multicamadas para os novos módulos, centralizando a orquestração de processos no servidor de aplicação para minimizar a latência de rede entre as unidades.  
C) Transição para arquitetura multicamadas com foco na separação lógica entre apresentação e persistência, utilizando o modelo SOA para gerenciar a comunicação síncrona entre os módulos internos via barramento de serviços centralizado.  
D) Implementação de uma arquitetura orientada a serviços para substituir o modelo cliente-servidor, mantendo a lógica de negócio concentrada em um único repositório de dados para evitar a fragmentação da integridade referencial.  
E) Migração da estrutura cliente-servidor para um modelo de microsserviços onde cada componente compartilha a camada de persistência, garantindo a uniformidade dos dados sem necessidade de sincronização ou replicação.

**21.** (FCC – TRF-4/2025 – Analista Judiciário – Análise de Sistemas de Informação)  
De acordo com a documentação da Plataforma Digital do Poder Judiciário – PDPJ-Br, para que uma API seja considerada do tipo RESTful, ela precisa

A) ter uma arquitetura cliente/servidor formada por clientes e servidores, com solicitações gerenciadas por meio dos protocolos HTTP e FTP.  
B) estabelecer uma comunicação statefull entre cliente e servidor, ou seja, as solicitações não são separadas e/ou desconectadas.  
C) armazenar dados em cache para otimizar as interações entre cliente e servidor.  
D) ter uma interface diferente para cada componente para que as informações possam ser transferidas em diferentes formatos.  
E) ter um sistema sem camadas que organiza os tipos de servidores envolvidos na recuperação das informações solicitadas sem hierarquias.

**22.** (FCC – TJ-PE/2012 – Analista Judiciário – Analista de Sistemas)  
Com relação ao Barramento de Serviços Corporativos (ESB) é **INCORRETO** afirmar:

A) Algumas das capacidades consideradas essenciais para um barramento de serviço corporativo (ESB) são: Resolução de Descrições de Serviços, Transformação de Mensagens e Roteamento Dinâmico de Mensagens.  
B) Numa abordagem direcionada a API, o ESB define APIs específicas de plataforma e os fornecedores. Os consumidores utilizam essas APIs para implementar serviços e realizar chamadas. Um exemplo disso são as interfaces Java.  
C) Um dos principais objetivos do ESB é prover conectividade para integrar diferentes plataformas de hardware e software, mesmo diante de diferentes middleware e protocolos.  
D) Utilizar um ESB em uma arquitetura transforma-a em uma arquitetura orientada a serviços. Isso equivale a dizer que ESB implementa SOA.  
E) Numa abordagem direcionada a protocolo, o ESB define um protocolo e os fornecedores. Os consumidores utilizam esse protocolo para enviar e receber mensagens. Um exemplo disso é Web Service utilizando SOAP.

**23.** (FCC – TRT-12/2023 – Analista Judiciário – TI)  
SOA é um paradigma de design arquitetônico, utilizado em diversos sistemas de software, que

A) promove a separação das preocupações (SOC – separation of concerns) lógicas e funcionais, facilitando o gerenciamento e a manutenção dos sistemas.  
B) não suporta a integração com tecnologias legadas, tornando-se inviável para sistemas mais antigos.  
C) torna difícil a adaptação a mudanças nos requisitos devido à sua rigidez estrutural.  
D) requer configuração manual de descoberta de serviços, não suportando configuração dinâmica.  
E) não suporta a reutilização de serviços, o que acarreta maior esforço de desenvolvimento.

**24.** (FCC – TRE-PE/2011 – Analista Judiciário – Análise de Sistemas)  
Arquitetura padrão proposta pelo Object Management Group (OMG) para estabelecer e simplificar a troca de dados entre sistemas distribuídos heterogêneos por meio de uma estrutura comum para o gerenciamento de objetos distribuídos que é conhecida como Object Manager Architecture (OMA). Trata-se de

A) IDL.  
B) RPC.  
C) DCON.  
D) CORBA.  
E) COM.

**25.** (FCC – TRT-1/2025 – Técnico Judiciário – TI)  
Há diversos modelos de organização de arquiteturas de software que podem ser utilizados para construir sistemas robustos e escaláveis. Um Técnico Judiciário, ao ser convidado para opinar sobre esse tema, afirmou corretamente que:

A) Em uma arquitetura distribuída multicamadas, é possível combinar serverless computing para processar eventos na camada de negócios, utilizar objetos distribuídos para facilitar a comunicação entre diferentes servidores na camada de dados e implantar as diversas camadas em uma cloud computing pública para otimização de recursos.  
B) Em uma arquitetura multicamadas, a camada de negócios deve sempre estar no mesmo servidor da camada de dados para evitar latência na comunicação entre os componentes.  
C) O uso de objetos distribuídos deve ser evitado em arquiteturas cliente-servidor para garantir que a comunicação ocorra de forma síncrona e dentro do mesmo contexto de rede.  
D) Serverless computing não é recomendado em soluções que utilizam cloud computing, pois os ambientes de nuvem pública exigem o gerenciamento explícito de servidores para controlar custos de forma eficiente.  
E) Em uma arquitetura multicamadas, os componentes funcionais comunicam-se por meio de chamadas de procedimentos locais dentro de um único processo, ao invés de mensagens entre processos, o que garante seu alto desempenho. Por isso, a comunicação entre as camadas deve ser sempre síncrona, para garantir que as respostas sejam imediatas e não haja falhas de consistência nos dados.

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| 16 | **E** | FCC TJ-CE/2022 — só a interface é visível |
| 17 | **D** | FCC TRT-1/2025 — SOA na modernização |
| 18 | **E** | FCC TRE-PB/2015 — Negócio (3-tier) × Model (MVC) |
| 19 | **B** | FCC METRÔ-SP/2008 — I e IV |
| 20 | **A** | FCC MPE-AL/2026 — microsserviços + BD independentes + SOA |
| 21 | **C** | FCC TRF-4/2025 — REST: cache é restrição do estilo |
| 22 | **D** | FCC TJ-PE/2012 — **incorreto** igualar ESB a “é SOA” |
| 23 | **A** | FCC TRT-12/2023 — SOA e separation of concerns |
| 24 | **D** | FCC TRE-PE/2011 — CORBA / OMA |
| 25 | **A** | FCC TRT-1/2025 (Técnico TI) — multicamadas + objetos distribuídos + cloud |

**Distribuição:** A(3) B(1) C(1) D(3) E(2).

**Notas rápidas:** 16 E; 17 D; 18 E; 19 B; 20 A; 21 C; 22 D; 23 A; 24 D; 25 A.

---

*Arquivo 01 · Arquiteturas de sistemas · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 02 PHP — visão comparativa.*

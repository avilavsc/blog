+++
title = '06 — Criptografia, certificados, hashes e TLS'
date = '2026-10-28'
draft = false
weight = 6
tags = ['semana-08', 'criptografia', 'certificados']
categories = ['estudo']
+++
# Criptografia, certificados, hashes e TLS · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — Criptografia simétrica/assimétrica, certificados, assinaturas, hashes, TLS 1.2/1.3 (limites do SSL)  
**Escopo deste arquivo:** simétrica × assimétrica; papel em HTTPS/TLS; certificados/PKI/AC; assinatura digital (hash + chave privada); hashes (MD5/SHA); TLS 1.2 vs 1.3 e legado SSL.  
**Fora de escopo aqui:** VPN/ZTNA operacionais (arquivo 05 — só o gancho TLS da VPN-SSL); WPA/EAP (arquivo 07); ISO/LGPD/OWASP (outros packs).

---

## 1. Material

### 1.1 Dois motores, um sistema híbrido

| Tipo | Chaves | Uso típico em prova | Exemplos |
|------|--------|---------------------|----------|
| **Simétrica** | Mesma chave cifra/decifra | **Volume** de dados (rápida) | AES, ChaCha20 |
| **Assimétrica** | Par pública/privada | Troca de chave, assinatura, autenticação | RSA, ECDSA, ECDH |

**Flash FCC TRF-4/2025 (HTTPS/PKI):** simétrica cifra o **conteúdo**; assimétrica ajuda a estabelecer a **chave de sessão** com segurança; **certificados** validam a identidade do servidor via **Autoridade Certificadora (AC)**.

Distratores clássicos:

- Assimétrica é “mais rápida” e cifra **toda** a conversa HTTPS — **falso**.  
- Certificado traz chave simétrica pré-compartilhada e “dispensa” assimétrica — **falso**.  
- PKI elimina simétrica — **falso**.

### 1.2 Confidencialidade, integridade, autenticidade, não repúdio

- **Confidencialidade:** cifra (simétrica na sessão).  
- **Integridade:** hash / MAC / AEAD (ex.: AES-GCM).  
- **Autenticidade:** certificado + assinaturas; verificação com chave pública.  
- **Não repúdio:** assinatura com chave **privada** do signatário (associada a certificado).

### 1.3 Hashes criptográficos

Função hash: entrada de tamanho arbitrário → **resumo de tamanho fixo**. Propriedades cobráveis: resistência a colisão; unidirecionalidade (não “reverter” o hash para a mensagem).

Usos: integridade de arquivos, armazenamento de senhas (com sal — noção), e **assinatura digital** (assina-se o hash, não o arquivo inteiro byte a byte na narrativa de prova).

Algoritmos: **MD5** e **SHA-1** (legado/fracos para muitas aplicações); **SHA-256**, **SHA-512** (família SHA-2).

**Flash FCC TRT-24/2016:** SHA-256 e SHA-512 usam palavras internas de **32 bits** e **64 bits**, respectivamente — **não** confundir com o tamanho do digest (256/512), que é a pegadinha da alternativa E típica.

**Flash FCC TRT-12/2023:** assinatura digital envolve **cifrar o hash do documento com a chave privada** do signatário.  
- Não é a chave **pública** que cria a assinatura.  
- Hash sozinho não “prova identidade”; o pacote hash + assimétrica + certificado sim.  
- Hash **não** garante disponibilidade.

### 1.4 Assinatura digital — fluxo FCC

1. Calcular **hash** do documento.  
2. **Cifrar/assinar** esse hash com a **chave privada** do signatário → assinatura.  
3. Verificador usa a **chave pública** (do certificado) para checar a assinatura e recalcula o hash do documento recebido: se bate, integridade + autenticidade (no modelo).

Alteração de uma vírgula → hash diferente → assinatura inválida (se a verificação estiver correta).

### 1.5 Certificados e PKI

**Certificado digital:** liga uma **chave pública** a uma identidade (pessoa, servidor, código), assinado por uma **AC**.  
**PKI:** infraestrutura (AC raiz, intermediárias, políticas, CRL/OCSP, etc.).

No HTTPS típico: o servidor apresenta certificado; o cliente valida cadeia até uma raiz confiável; depois negociam chave de sessão.

**Flash FCC TRT-20/2024:** autenticação de usuários com certificados usa algoritmos assimétricos como **RSA ou ECDSA** — não SHA sozinho (hash), não AES-GCM (simétrico), não “SSL/TLS” como se fossem algoritmos de assinatura.

ICP-Brasil: ecossistema nacional de AC para assinaturas com validade jurídica ampla — útil como contexto; a mecânica cobrada continua sendo PKI clássica (hash + privada).

### 1.6 TLS 1.2, TLS 1.3 e o legado SSL

**SSL** (Secure Sockets Layer) é legado; **TLS** (Transport Layer Security) é o sucessor. O edital cita **TLS 1.2/1.3** e **limitações históricas do SSL**.

Pontos cobráveis TLS 1.3 (visão analista):

- Handshake mais simples/rápido; ênfase em DH para troca de chaves.  
- Remove cifras/modos fracos (ex.: RC4, CBC legado problemático no discurso de modernização).  
- Mais mensagens do handshake protegidas (incl. certificado do servidor após ServerHello, na narrativa segura).  
- Mitiga classes de **downgrade** que afetaram versões antigas.

TLS 1.2 ainda é amplamente aceito quando bem configurado (suites modernas); SSL 3.0 / TLS muito antigos = evitar.

HTTPS = HTTP sobre TLS (historicamente “SSL” no nome de produtos: VPN-SSL, certificados “SSL”).

### 1.7 AAA, RBAC, MFA (gancho do edital)

Aparecem no mesmo bloco de segurança:

- **AAA:** Authentication, Authorization, Accounting.  
- **RBAC:** autorização por papéis.  
- **MFA:** múltiplos fatores (algo que sabe / tem / é).

Não substituem criptografia; **compõem** o controle de acesso. Certificado cliente pode ser um fator (“algo que tem”).

### 1.8 CRL, OCSP e ciclo de vida (visão)

Além de emitir, a PKI precisa **revogar** certificados comprometidos ou expirados precocemente. **CRL** (lista de revogação) e **OCSP** (consulta online de status) aparecem em questões mais densas: o cliente HTTPS, além de validar a cadeia e as datas, pode checar se o certificado foi revogado. Para o TRT4, memorize o **papel**; não precisa montar OCSP responder.

Carimbo de tempo (timestamping) e políticas ICP-Brasil importam para assinatura com validade prolongada — contexto jurídico; a mecânica da prova continua: hash + privada + verificação com pública.

### 1.9 Mapa anti-confusão

| Conceito | Não é |
|----------|-------|
| Hash | Cifra reversível com chave |
| AES | Algoritmo de assinatura assimétrica |
| RSA/ECDSA | Hash puro |
| Certificado | Chave simétrica pré-compartilhada |
| TLS | Algoritmo de hash |
| SSL | Estado da arte obrigatório em 2026 |

### 1.10 Mini-roteiro

1. Bulk de dados na sessão? → **simétrica**.  
2. Troca de chave / assinatura / cert? → **assimétrica + PKI**.  
3. “Cifrou o hash com…” na criação da assinatura? → **chave privada**.  
4. SHA-256 vs SHA-512 “palavras”? → **32 / 64 bits** (não 256/512).  
5. SSL × TLS? → **TLS atual; SSL legado**.

---

## 2. Questões de treino (1–15)

**1.** Para cifrar grande volume de dados na sessão HTTPS, o mais adequado:

A) criptografia simétrica (ex.: AES) com chave de sessão  
B) apenas RSA em todo o payload indefinidamente  
C) apenas MD5  
D) OSPF  
E) VLAN  

**2.** Papel típico da criptografia assimétrica no HTTPS:

A) substituir totalmente a simétrica no bulk  
B) eleger root bridge  
C) ajudar a estabelecer chave simétrica com segurança e apoiar autenticação/assinatura  
D) comprimir HTTP  
E) filtrar spam  

**3.** Certificado digital, na PKI:

A) contém apenas chave simétrica AES pré-fixada  
B) dispensa qualquer assimetria  
C) é o nome do protocolo AH  
D) vincula chave pública a uma identidade, validado via AC  
E) substitui firewall  

**4.** Criação da assinatura digital:

A) cifrar o hash com a chave pública do signatário  
B) aplicar só MD5 sem chave  
C) usar WEP  
D) publicar a chave privada no DNS  
E) cifrar o hash com a chave privada do signatário  

**5.** SHA-256 e SHA-512 — palavras internas (FCC TRT-24):

A) 16 e 32 bits  
B) 1024 e 2128 bits  
C) 64 e 128 bits  
D) 32 e 64 bits  
E) 256 e 512 bits (como tamanho de palavra interna)  

**6.** Alternativa E da pegadinha SHA: 256 e 512 referem-se principalmente a

A) portas TCP  
B) VIDs de VLAN  
C) tamanho do digest de saída, não ao tamanho da palavra interna  
D) prioridade STP  
E) canais Wi-Fi  

**7.** Hash garante, no discurso básico:

A) disponibilidade do datacenter  
B) roteamento OSPF  
C) PoE  
D) QoS de voz  
E) integridade (detecção de alteração) quando usado corretamente  

**8.** Algoritmos para autenticação com certificados (assinatura):

A) RSA ou ECDSA  
B) SHA-256 ou SHA-512 isolados como “assimétricos”  
C) AES-CERT ou AES-GCM como assimétricos  
D) SSL ou TLS como algoritmos de assinatura  
E) S/MIME ou ChaCha como único par assimétrico  

**9.** SSL em relação a TLS:

A) SSL é o sucessor moderno do TLS 1.3  
B) São protocolos de VLAN  
C) SSL é obrigatório e TLS proibido  
D) TLS sucede o SSL; SSL traz limitações históricas  
E) TLS existe só em WEP  

**10.** TLS 1.3, em contraste com legado:

A) simplifica handshake e remove cifras/modos considerados inseguros  
B) mantém RC4 como cifra recomendada principal  
C) elimina qualquer autenticação de servidor  
D) proíbe AES-GCM  
E) opera apenas na camada física  

**11.** MFA:

A) é um algoritmo de hash  
B) substitui PKI integralmente  
C) é o modo túnel IPsec  
D) combina múltiplos fatores de autenticação  
E) é antispam  

**12.** RBAC:

A) cifra simétrica AES-only  
B) autorização baseada em papéis  
C) padrão 802.1Q  
D) sniffer  
E) root bridge  

**13.** Se o documento assinado muda uma cláusula e a verificação ainda diz “válida”, a causa plausível NÃO é:

A) verificador que não recalcula/compara o hash corretamente (bug/config)  
B) uso de chave/certificado errados na checagem  
C) software de verificação comprometido ou mal configurado  
D) “hash idêntico mesmo com pontuação diferente” sendo propriedade criptográfica normal do SHA-256  
E) validação que ignora a assinatura anexada  

**14.** Em HTTPS bem explicado (TRF-4/2025):

A) assimétrica cifra toda a conversa por ser mais rápida  
B) certificado dispensa assimétrica  
C) PKI elimina simétrica  
D) hashes substituem AC  
E) simétrica no conteúdo; assimétrica na chave de sessão; certificado autentica servidor via AC  

**15.** Síntese:

A) Só MD5 basta para HTTPS bancário moderno  
B) Chave pública cria a assinatura  
C) AES é assimétrico  
D) TLS 1.3 exige WEP  
E) Híbrido TLS: simétrica no bulk, assimétrica+certificados na confiança; hash na assinatura/integridade; preferir TLS 1.2/1.3 ao SSL legado  

---

### Gabarito comentado (treino 1–15)

1. **A** — simétrica na sessão.  
2. **C** — troca/autenticação.  
3. **D** — PKI/AC.  
4. **E** — privada assina.  
5. **D** — 32/64 (TRT-24).  
6. **C** — digest ≠ word size.  
7. **E** — integridade.  
8. **A** — RSA/ECDSA (TRT-20).  
9. **D** — TLS sucede SSL.  
10. **A** — modernização 1.3.  
11. **D** — MFA.  
12. **B** — papéis.  
13. **D** — falso: pontuação muda hash.  
14. **E** — TRF-4.  
15. **E** — mapa.

---

## 3. Questões de concurso FCC (16–25)

**16.** (FCC · TRF4/2025 · Analista — Análise de Sistemas)  
Uma Analista está implementando um sistema seguro de troca de mensagens via HTTPS e tem dúvidas sobre os conceitos de criptografia e a infraestrutura de chaves públicas (PKI). Ao discutir com outro analista, um deles esclareceu adequadamente que

A) a criptografia assimétrica, exemplo RSA, é usada para cifrar toda a comunicação diretamente, pois é mais rápida que a simétrica, enquanto os certificados digitais servem para autenticar o cliente.  
B) certificados digitais dispensam o uso de criptografia assimétrica, pois já incluem chaves simétricas pré-compartilhadas, reduzindo a complexidade da PKI.  
C) tanto a criptografia simétrica quanto a assimétrica são usadas para autenticação, enquanto os certificados digitais garantem a confidencialidade através da PKI.  
D) a criptografia simétrica é utilizada para cifrar o conteúdo da mensagem; a criptografia assimétrica é utilizada para criar a chave simétrica de forma segura; e os certificados digitais validam a identidade do servidor por meio de uma Autoridade Certificadora (AC).  
E) a PKI elimina a necessidade de criptografia simétrica, uma vez que as chaves públicas da criptografia assimétrica são suficientes para proteger toda a comunicação com desempenho ideal.  

**17.** (FCC · TRT-20/2024 · Técnico — TI)  
Muitas organizações implementam sistemas de autenticação remota que utilizam criptografia simétrica e assimétrica para proteger o tráfego de dados entre os servidores e usuários. O tráfego pode ser criptografado com AES, enquanto a autenticidade dos usuários é verificada usando algoritmos de criptografia assimétrica em certificados digitais. Para autenticação de usuários, são utilizados algoritmos como

A) S/MIME ou ChaCha256.  
B) AES-CERT ou AES-GCM.  
C) SHA-256 ou SHA-512.  
D) SSL ou TLS.  
E) RSA ou ECDSA.  

**18.** (FCC · TRT-20/2024 · Analista — TI)  
Uma organização frequentemente troca e-mails sensíveis entre seus funcionários e clientes. Para garantir que os e-mails enviados não sejam alterados e para autenticar os remetentes, a equipe de TI decidiu implementar uma solução de assinatura digital para e-mails corporativos, que também assegure a integridade e a autenticidade das mensagens. Nesse contexto, a solução mais adequada para proteger e-mails com assinatura digital é o uso de

A) AES-GCM.  
B) HTTP.  
C) PGP-IMAP.  
D) S/MIME.  
E) VPN.  

**19.** (FCC · TRT-6/2018 · Analista — TI)  
Considere que o Analista especializado em Tecnologia da Informação está especificando as técnicas e os recursos para a implantação da segurança da informação no Tribunal Regional do Trabalho da 6ª Região. Para alcançar o objetivo de garantir a integridade da informação transmitida pela internet, o Analista deve utilizar

A) a criptografia de chave simétrica.  
B) a criptografia de chave pública.  
C) a função Hash .  
D) o certificado digital.  
E) o Token digital.  

**20.** (FCC · TRT-6/2025 · Analista — TI)  
Uma Analista de nome Maria Cecília criptografa uma mensagem com sua chave privada e envia pela internet para João Miguel, que recebe a mensagem e a descriptografa com a chave pública de Maria Cecília para ler a mensagem. O uso da criptografia nesse caso está garantindo o princípio da

A) autenticidade.  
B) disponibilidade.  
C) integridade.  
D) confidencialidade.  
E) confiabilidade.  

**21.** (FCC · SEFAZ-SC/2018 · TI)  
O estabelecimento e a implantação da segurança da informação requer o uso de diferentes mecanismos e recursos para atender os atributos de segurança da informação. Nesse contexto, o Hash tem a função de

A) realizar a transformação reversível da informação de forma a torná-la ininteligível a terceiros.  
B) criar uma chave criptográfica a partir do certificado digital.  
C) comprovar a identidade do emissor de um documento.  
D) atestar a validade de um documento.  
E) garantir a integridade de um documento.  

**22.** (FCC · TRT-5/2022 · Analista — TI)  
Um analista entrou em um site e desejava saber se este utilizava conexão segura. Para isto clicou em um ícone de cadeado ao lado da URL no navegador, a partir do qual obteve informações sobre

A) a assinatura digital, tais como chave pública, chave privada e autorização de acesso.  
B) o certificado digital, tais como chave secreta, hash público e período de validade.  
C) o certificado digital, tais como emissão (para e por quem), período de validade e assinaturas digitais.  
D) a assinatura eletrônica, tais como assinaturas digitais, contraprova e autorização de acesso.  
E) o hash criptográfico, tais como validade do certificado digital, redirecionamentos seguros e hash público.  

**23.** (FCC · TRT-12/2023 · Analista — TI)  
Atenção: Para responder às questões de números 37 e 38, examine o programa C# abaixo. using System; using System.Security.Cryptography; using System.Text; class Program{ static void Main(){ string senha = "Tribunal Regional do Trabalho 12a região"; string senhaCriptografada = CriptografarSenha(senha); Console.WriteLine($"Senha: {senha}"); Console.WriteLine($"Senha Criptografada: {senhaCriptografada}"); } static string CriptografarSenha(string senha){ using (SHA256 sha256 = SHA256.Create()){ byte[] bytes = Encoding.UTF8.GetBytes(senha); I ...... StringBuilder builder = new StringBuilder(); for (int i = 0; i < hashBytes.Length; i++) builder.Append(hashBytes[i].ToString("x2")); return builder.ToString(); } } } O programa C# faz referência ao SHA-256. Associando a criptografia com a segurança em redes sem fio, é correto afirmar que o Este texto de apoio vale também pra questão 38 .

A) SHA-256 pode ser utilizado para criar hashes dos certificados digitais, que são usados para autenticar os dispositivos na rede, em uma implementação de WPA2 com autenticação baseada em certificados (como o EAP-TLS).  
B) SHA-256 é um protocolo de segurança para redes sem fio, que utiliza um hash criptográfico pertencente à família das funções hash SHA-2 ( Security Hash Algorithm 2nd generation ). O hash resultante é uma sequência de bytes que é única para os dados de entrada.  
C) WPA2 não contém mecanismos de autenticação; assim, é necessário acrescentar o uso de chaves pré-compartilhadas (PSK) e a autenticação baseada em servidores, usando protocolos criptográficos como o SHA-256.  
D) WPA2 é um método de hash criptográfico para redes sem fio, que utiliza o protocolo de criptografia AES-CCMP ( Advanced Encryption Security – Counter Mode Cipher Block Chaining Message Authentication Ciphered Protocol ) para proteger os dados transmitidos entre dispositivos na rede.  
E) WPA ( Wi-Fi Protected Access ) utiliza o protocolo SHA-256 para criptografar dados e o método EAP ( Extensible Authentication Protocol ) para autenticação.  

**24.** (FGV · CVM/2024 · Analista TI)  
A norma ISO/IEC 27001 é um padrão internacional que define requisitos a que um Sistema de Gestão de Segurança da Informação (SGSI) deve atender. Ela se divide em duas partes: o texto da norma em si, que descreve os requisitos de forma abrangente, e o Anexo A, mais detalhado e objetivo, que tem por função:

A) estabelecer a política de certificação digital;  
B) especificar os papéis e responsabilidades da alta administração;  
C) delinear o processo de avaliação de riscos;  
D) fornecer orientação sobre a implementação de controles de segurança da informação;  
E) definir o escopo do Sistema de Gestão de Segurança da Informação.  

**25.** (FCC · TRT-12/2023 · Analista — TI)  
A VPN-SSL é um tipo específico de VPN que

A) é mais adequada em ambientes que exigem configurações avançadas e controle granular sobre políticas de segurança e recursos de rede.  
B) utiliza exclusivamente o protocolo SSL ( Secure Sockets Layer ) para estabelecer uma conexão segura entre o cliente e o servidor.  
C) requer, geralmente, a instalação de um software cliente dedicado em cada dispositivo para ser utilizada.  
D) não é compatível com dispositivos móveis e tablets , pois necessita de software cliente no dispositivo desktop para sua utilização.  
E) pode ser acessada através de um navegador web padrão, usando o protocolo HTTPS.  

---

---

### Gabarito (concurso 16–25)

| Questão | Gabarito | Fonte / nota |
|---------|----------|--------------|
| Questão | Gabarito | Fonte / nota |
|---|---|---|
| 16 | **D** | Simétrica + assimétrica + AC |
| 17 | **E** | RSA ou ECDSA |
| 18 | **D** | S/MIME |
| 19 | **C** | Hash = integridade |
| 20 | **A** | Assinatura → autenticidade |
| 21 | **E** | Função do hash |
| 22 | **C** | Cadeado = certificado |
| 23 | **A** | SHA-256 e EAP-TLS |
| 24 | **D** | Anexo A (controles) |
| 25 | **E** | VPN-SSL/TLS |
*Arquivo 06 · Criptografia / certificados / hashes / TLS · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 07 Wi-Fi seguro WPA.*

*Arquivo 06 · Criptografia / certificados / hashes / TLS · Claudionir · TRT4 Analista TI (código 14) · FCC. Próximo: 07 Wi-Fi seguro WPA.*

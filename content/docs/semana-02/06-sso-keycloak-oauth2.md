+++
title = '06 — SSO / Keycloak / OAuth2'
date = '2026-10-25'
draft = false
weight = 6
tags = ['semana-02', 'sso', 'oauth2', 'keycloak']
categories = ['estudo']
+++
# SSO / Keycloak / OAuth2 · TRT4 Analista TI (código 14)

**Para:** Claudionir · **Banca:** FCC · **Subtópico:** 06 — SSO, Keycloak, OAuth2 (RFC 6749) + boas práticas (RFC 9700)  
**Escopo deste arquivo:** Single Sign-On; papel do **Keycloak** no eixo PDPJ-Br; papéis e *grants* do **OAuth 2.0 (RFC 6749)**; tokens; fluxo Authorization Code; contraste com Implicit e Resource Owner Password; **BCP de segurança RFC 9700** (PKCE, redirect exato, o que *não* usar); Bearer no header; anti-confusão com Gateway/Discovery.  
**Fora de escopo aqui:** JPA/Hibernate/Flyway (outros arquivos da semana 2); mensageria RabbitMQ; Docker/K8s; detalhes profundos de LDAP/AD (redes); LGPD/OWASP em profundidade (semana de segurança). OpenID Connect aparece só o necessário para encaixar Keycloak/PDPJ.

---

## 1. Material

### 1.1 SSO — o problema e a ideia

**SSO (*Single Sign-On*)** = autenticar **uma vez** e usar **vários** sistemas/serviços sem repetir login a cada módulo.

Em ecossistema de microsserviços (PDPJ-Br / tribunais), o anti-padrão é:

- cada sistema com **cadastro e senha próprios**;
- usuário digita credenciais N vezes;
- federação de identidades vira integração *ad hoc* frágil.

SSO responde à pergunta:

> **Como o usuário (interno ou externo) prova identidade uma vez e acessa vários serviços do ecossistema com a mesma sessão/credencial federada?**

Três âncoras mentais:

| Âncora | Frase cobrável |
|--------|----------------|
| Experiência | **Uma autenticação → vários sistemas** |
| Técnico | Provedor central (IdP / Authorization Server) + tokens |
| PDPJ | SSO baseado em **Keycloak**, protocolo **OAuth2** |

Flash: SSO **não** é sinônimo de “um banco único nacional de processos”; é **identidade/acesso unificado**.

### 1.2 SSO no eixo PDPJ-Br (leitura de prova)

Na documentação e normativos alinhados à PDPJ-Br:

- a autenticação da plataforma usa **SSO baseado em Keycloak**;
- o protocolo de referência do edital é **OAuth 2.0 (RFC 6749)**;
- o edital exige também **boas práticas (RFC 9700)** — não basta decorar só o *grant* clássico;
- sistemas/módulos novos na PDPJ devem **usar o SSO** da plataforma; sistemas processuais dos tribunais **aderem** via integração (provedores / federação);
- do ponto de vista da aplicação: autenticidade tipicamente via **`Authorization: Bearer <access_token>`**.

Leitura FCC: **Keycloak = produto/IdP do SSO da PDPJ**; **OAuth2 = protocolo**; **RFC 9700 = higiene de segurança moderna do OAuth**.

Federação (ideia cobrável): o usuário pode autenticar com identidade já existente em bases integradas (ex.: CNJ Corporativo / PJe com integração), **sem cadastro paralelo** desnecessário — o SSO **une** bases via provedores, não “apaga” todos os cadastros locais por magia.

### 1.3 Autenticação × autorização (não misturar)

| Conceito | Pergunta | Em OAuth/Keycloak |
|----------|----------|-------------------|
| **Autenticação** | Quem é você? | Login no IdP / SSO; prova de identidade |
| **Autorização** | O que você pode fazer? | *Scopes*, *roles*, *policies*; o token carrega (ou referencia) permissões |

Erro clássico: chamar OAuth de “só autenticação”. OAuth 2.0 é, na origem, **framework de autorização** (delegar acesso). Na prática com Keycloak + OpenID Connect, o pacote entrega **identidade + autorização** — mas em prova:

- OAuth2 (6749) → **autorização / tokens / grants**;
- SSO/Keycloak → **login único + emissão/gestão de tokens**;
- “validar Bearer na API” → **proteger recurso** (Resource Server).

### 1.4 OAuth 2.0 (RFC 6749) — papéis

Decorar os **quatro papéis** (ouro de prova):

| Papel | Nome técnico | Função |
|-------|--------------|--------|
| **Resource Owner** | Dono do recurso | Usuário (ou entidade) que autoriza o acesso |
| **Client** | Cliente | Aplicação que pede acesso em nome do usuário |
| **Authorization Server** | Servidor de autorização | Autentica o usuário, obtém consentimento, **emite tokens** |
| **Resource Server** | Servidor de recursos | API/serviço que **protege** os dados e exige token válido |

No mapa PDPJ/Keycloak (visão de concurso):

| Papel OAuth | Encaixe típico |
|-------------|----------------|
| Authorization Server | **Keycloak** (SSO) |
| Resource Server | Microsserviço / API da PDPJ ou do tribunal |
| Client | Frontend, BFF, módulo, integração cadastrada no realm |
| Resource Owner | Usuário (magistrado, servidor, externo autorizado etc.) |

Anti-confusão: **Authorization Server ≠ API Gateway**. Gateway pode **validar** token na borda; quem **emite** o token no desenho SSO é o Keycloak/AS.

### 1.5 Tokens essenciais

| Token | Ideia cobrável |
|-------|----------------|
| **Access token** | Credencial de **curta duração** para chamar APIs protegidas (Bearer) |
| **Refresh token** | Credencial para **obter novo access token** sem novo login interativo (com regras) |
| **Authorization code** | Código **intermediário** de curta vida, trocado no *token endpoint* (não é o access token) |

Flash: no Authorization Code Flow, o browser **não** recebe o access token no redirect “como se fosse Implicit”; recebe o **code**, e o client troca o code por tokens no *token endpoint* (back-channel).

Formato em APIs REST (visão PDPJ/prova):

```http
Authorization: Bearer eyJhbGciOi...
```

O *access token* costuma ser **JWT** em deploys Keycloak (assinado; o Resource Server valida assinatura, expiração, *audience* etc.) — não precisa decorar *claims* linha a linha; saiba que o RS **valida** o token, não “confia no client cegamente”.

### 1.6 Endpoints mentais (Authorization Code)

Fluxo mental mínimo:

```
1. Client redireciona o user agent ao Authorization Endpoint
2. Usuário autentica no AS (Keycloak) e consente (quando aplicável)
3. AS redireciona de volta com authorization code
4. Client troca code (+ autenticação do client / PKCE) no Token Endpoint
5. Client chama Resource Server com Authorization: Bearer <access_token>
```

| Endpoint | O que faz |
|----------|-----------|
| **Authorization** | Interação com o usuário (login/consent); devolve *code* (no fluxo recomendado) |
| **Token** | Emite *access* / *refresh* (troca de *grant*) |
| **(Introspection / JWKS)** | Detalhe avançado: validar/revogar/chaves — só noção se o enunciado pedir |

### 1.7 Grants da RFC 6749 (mapa de prova)

| Grant | Ideia | Status mental FCC (6749 + 9700) |
|-------|-------|----------------------------------|
| **Authorization Code** | Redirect + code + troca no token endpoint | **Preferido / padrão seguro** (com PKCE na 9700) |
| **Implicit** | Token no redirect (*response_type=token*) | **Evitar** (9700: SHOULD NOT) |
| **Resource Owner Password Credentials (ROPC)** | Client recebe user/senha e troca por token | **MUST NOT** na 9700 |
| **Client Credentials** | Client autentica-se sozinho (máquina a máquina) | Ok quando **não há** usuário final; escopo mínimo |

Armadilha de material antigo / docs hands-on: exemplos com `grant_type=password` ainda aparecem em tutoriais. Para a **prova TRT4**, o edital cita **RFC 9700** → trate ROPC como **proibido nas boas práticas modernas**, mesmo que a 6749 o tenha definido historicamente.

### 1.8 Authorization Code × Implicit (contraste seco)

| | **Authorization Code** | **Implicit** |
|---|------------------------|--------------|
| O que volta no redirect | **Code** | **Access token** (fragment/query) |
| Onde nasce o access token | **Token endpoint** (troca do code) | Direto na resposta de autorização |
| Exposição no browser | Menor (code de curta vida + troca) | **Alta** (token vaza fácil) |
| RFC 9700 | Usar **code** (+ PKCE) | **SHOULD NOT** usar |

Síntese: Implicit foi “atalho histórico” para SPAs; a BCP atual empurra **Authorization Code + PKCE**.

### 1.9 RFC 9700 — boas práticas cobráveis (BCP)

A **RFC 9700** (*Best Current Practice for OAuth 2.0 Security*) atualiza a higiene de segurança do OAuth. Pontos de ouro para FCC:

1. **PKCE (RFC 7636)**  
   - Clientes **públicos**: **MUST** usar PKCE no Authorization Code.  
   - Clientes **confidential**: **RECOMMENDED**.  
   - Authorization Server: **MUST** suportar PKCE.  
   - Preferir método **`S256`** (*code_challenge* = hash do *code_verifier*).  
   - Mitigar *downgrade*: se veio `code_verifier` no token, só aceitar se houve `code_challenge` na autorização.

2. **Implicit**  
   - Clients **SHOULD NOT** usar *response_type=token* (e similares que devolvem access token no redirect).

3. **ROPC (password grant)**  
   - **MUST NOT** ser usado: expõe senha ao client; aumenta superfície de ataque; treina usuário a digitar senha fora do AS; combina mal com MFA.

4. **Redirect URI**  
   - Comparação por **exact string matching** (exceto caso estreito de *localhost*/loopback em apps nativos).  
   - **MUST NOT** *open redirectors* (URLs que redirecionam para URI arbitrária de query).  
   - Respostas de autorização **MUST NOT** via conexão não criptografada; **http** em redirect só em exceções de loopback nativo.

5. **CSRF / state / nonce**  
   - Clients **MUST** prevenir CSRF no *redirect endpoint*.  
   - Com PKCE confiável, pode apoiar-se nele; senão, `state` one-time; em OIDC, `nonce` também protege.

6. **Tokens**  
   - Preferir **sender-constrained** tokens (mTLS / DPoP) quando viável.  
   - Refresh de clientes públicos: **sender-constrained** ou **rotação** de refresh.  
   - Princípio do **menor privilégio**: *scope* mínimo; *audience* restrita ao Resource Server certo.

Cartão mental: **6749 = gramática do OAuth**; **9700 = o que a banca espera como “fazer certo hoje”**.

### 1.10 PKCE em uma frase + passos

**PKCE** = amarra o *authorization code* a quem iniciou o fluxo, via *code_verifier* secreto no client.

```
1. Client gera code_verifier aleatório
2. Calcula code_challenge (preferência: S256)
3. Manda challenge no /authorize
4. Depois, no /token, manda o code_verifier
5. AS confere: challenge bate com o verifier → emite tokens
```

Sem o verifier, roubar só o *code* **não basta** para obter o access token.

### 1.11 Keycloak — o que a FCC cobra

**Keycloak** = solução *open source* de **IAM** (Identity and Access Management): login, SSO, federação, clientes, *realms*, papéis, emissão de tokens OAuth2/OIDC.

| Conceito Keycloak | Ideia de prova |
|-------------------|----------------|
| **Realm** | “Universo” / isolamento de usuários, clients e configs |
| **Client** | Aplicação registrada que obtém tokens |
| **User** | Identidade no realm (local ou federada) |
| **Role / Group** | Base de autorização (quem pode o quê) |
| **Identity Provider / federation** | Ligar a outras bases (LDAP, IdPs, provedores custom) |
| **Token (JWT)** | Access/ID tokens emitidos após autenticação |

No discurso PDPJ: Keycloak hospeda o **SSO**; módulos cadastram **client**; APIs validam **Bearer**.

Não precisa virar admin de console: precisa **encaixar** Keycloak como Authorization Server / IdP do SSO.

### 1.12 OpenID Connect (só o encaixe)

**OpenID Connect (OIDC)** = camada de **identidade** sobre OAuth2 (emite *ID Token* além do access token). Keycloak fala OIDC; a PDPJ documenta OAuth2 e usa fluxos OpenID na prática.

Para prova neste arquivo:

- OAuth2 sozinho → autorização/tokens de API;
- OIDC → “quem é o usuário” de forma padronizada (*id_token*);
- Não confundir OIDC com “substituto do Eureka” ou com Gateway.

### 1.13 Fluxo ponta a ponta (visão microsserviços + SSO)

```
Usuário → App/Client → redireciona → Keycloak (AS/SSO)
                ← authorization code ←
Client → Token Endpoint (code + PKCE/secret) → access_token
Client → API Gateway / Resource Server
         Authorization: Bearer <token>
Gateway/RS valida token → encaminha / atende
```

Papéis complementares (anti-confusão da semana 1):

| Componente | Pergunta |
|------------|----------|
| **Keycloak / SSO** | Quem é / que token emitir? |
| **API Gateway** | Por onde entra? (pode validar token na borda) |
| **Eureka / Discovery** | Onde está a réplica do serviço? |
| **Resource Server** | O dado/ação está autorizado para este token? |

### 1.14 Clientes públicos × confidential

| | **Confidential** | **Public** |
|---|------------------|------------|
| Segredo | Pode guardar *client_secret* com segurança (backend) | **Não** consegue guardar segredo (SPA, mobile) |
| PKCE (9700) | RECOMMENDED | **MUST** |
| Exemplo mental | Serviço backend / BFF | App no browser / nativo |

Flash: “SPA usa Implicit” é resposta **antiga**; a BCP atual aponta **Code + PKCE**.

### 1.15 Armadilhas FCC (checklist)

- Confundir **SSO** com monólito ou banco único.  
- Dizer que **Keycloak = API Gateway**.  
- Tratar **authorization code** como access token.  
- Defender **Implicit** ou **password grant** como “melhor prática” quando o edital cita **RFC 9700**.  
- Achar que **OAuth2 elimina** a necessidade de validar token no Resource Server.  
- Misturar **Discovery (Eureka)** com autenticação.  
- Redirect “com wildcard frouxo” como se fosse seguro (9700: *exact match*).  
- Esquecer **Bearer** no header `Authorization`.

### 1.16 Cartão-resumo (antes das questões)

| Tema | Âncora |
|------|--------|
| SSO | Login único → vários serviços |
| PDPJ | SSO = Keycloak; protocolo = OAuth2; higiene = RFC 9700 |
| Papéis 6749 | Resource Owner · Client · AS · RS |
| Fluxo preferido | Authorization Code (+ PKCE) |
| Evitar / proibir (9700) | Implicit (evitar); ROPC (MUST NOT) |
| Token na API | `Authorization: Bearer …` |
| Keycloak | Realm, clients, users, federation, tokens |
| Anti-confusão | AS emite; Gateway entra; Eureka localiza |

---

## 2. Questões de treino (1–15)

**1.** SSO (*Single Sign-On*), no sentido cobrado neste eixo, caracteriza-se por:

A) Exigir cadastro e senha distintos em cada microsserviço, sem federação  
B) Permitir autenticação única com acesso subsequente a múltiplos sistemas/serviços do ecossistema  
C) Substituir integralmente o Service Discovery (Eureka)  
D) Mapear Entity↔DTO em tempo de compilação  
E) Versionar schema SQL com Flyway  

**2.** No eixo PDPJ-Br alinhado ao edital, a solução de SSO tipicamente associada é:

A) Eureka Server  
B) Zuul como único IdP  
C) **Keycloak**  
D) MapStruct  
E) Hibernate Envers  

**3.** A RFC 6749 trata, prioritariamente, de:

A) Persistência JPA 2.0  
B) Framework de autorização OAuth 2.0 (papéis, grants, tokens, endpoints)  
C) Cabeamento estruturado ABNT  
D) OpenAPI/Swagger UI  
E) Orquestração Kubernetes  

**4.** Assinale a associação correta de papéis OAuth 2.0:

A) Authorization Server = dono do recurso; Resource Owner = emite tokens  
B) Client = API que guarda os dados; Resource Server = app que pede acesso  
C) Resource Owner = autoriza; Client = pede acesso; AS = emite tokens; RS = protege o recurso  
D) Eureka = Authorization Server; Keycloak = Service Discovery  
E) Gateway = único papel OAuth definido na 6749  

**5.** No desenho SSO/PDPJ de prova, o Keycloak encaixa-se principalmente como:

A) Service registry de heartbeats  
B) Authorization Server / IdP que autentica e emite tokens  
C) Mapper compile-time Entity↔DTO  
D) Banco H2 embutido obrigatório  
E) Protocolo de camada física  

**6.** O *authorization code* no Authorization Code Flow é:

A) O mesmo que o access token JWT definitivo  
B) Código intermediário de curta vida, trocado no *token endpoint* por tokens  
C) O refresh token de longa duração entregue no fragmento Implicit  
D) A chave privada do Resource Server  
E) O *heartbeat* do Eureka  

**7.** Qual header tipicamente carrega o access token nas chamadas às APIs protegidas?

A) `X-Eureka-Instance`  
B) `Content-Type: application/xml` apenas  
C) `Authorization: Bearer <token>`  
D) `X-MapStruct-Mapper`  
E) `Flyway-Migrate: true`  

**8.** Segundo a RFC 9700, o *Resource Owner Password Credentials grant* (password):

A) É o único grant recomendado para SPAs modernas  
B) **MUST NOT** ser usado  
C) Substitui obrigatoriamente o Authorization Code  
D) É sinônimo de PKCE S256  
E) É obrigatório em todo client confidential  

**9.** PKCE (*Proof Key for Code Exchange*) serve principalmente para:

A) Documentar OpenAPI  
B) Amarrar o authorization code ao client que iniciou o fluxo, mitigando roubo/injeção do code  
C) Substituir o PostgreSQL  
D) Registrar instâncias no Eureka  
E) Gerar DTOs automaticamente  

**10.** Sobre o Implicit grant (*response_type=token*), a leitura alinhada à RFC 9700 é:

A) É o grant preferido e obrigatório em 2024+  
B) Clients **SHOULD NOT** usá-lo; preferir Authorization Code (com PKCE)  
C) É idêntico ao Client Credentials  
D) Elimina a necessidade de HTTPS em redirects  
E) É o único grant que Keycloak aceita  

**11.** Exact string matching de *redirect_uri*, na RFC 9700, visa principalmente:

A) Acelerar o MapStruct  
B) Reduzir vazamento de codes/tokens e erros de validação frouxa (wildcards/padrões)  
C) Tornar o Eureka desnecessário  
D) Permitir open redirectors arbitrários  
E) Substituir Bearer por Basic Auth obrigatório  

**12.** Assinale a linha **correta** no contraste anti-confusão:

A) Keycloak localiza réplicas; Eureka emite JWT; Zuul é realm  
B) Keycloak/SSO = identidade/tokens; Gateway = entrada/borda; Eureka = discovery  
C) Eureka autentica usuários finais; Keycloak faz heartbeat  
D) Gateway emite authorization codes como AS exclusivo da 6749  
E) Flyway valida Bearer na borda pública  

**13.** Clientes públicos (ex.: SPA que não guarda segredo com segurança), segundo a 9700 no Authorization Code:

A) Podem ignorar PKCE  
B) **MUST** usar PKCE  
C) Devem preferir ROPC  
D) Devem expor *client_secret* no JavaScript  
E) Só podem usar Implicit  

**14.** Qual afirmação sobre access token × refresh token é mais adequada?

A) Refresh token é o único aceito no header `Authorization: Bearer` das APIs de negócio  
B) Access token autoriza chamadas às APIs (tipicamente curto); refresh obtém novo access sem novo login interativo (com restrições)  
C) Ambos são authorization codes  
D) Access token só existe no Implicit; refresh só no password grant  
E) Refresh token substitui o Resource Server  

**15.** Qual opção melhor resume o cartão mental deste subtópico?

A) SSO Keycloak + OAuth2 6749 (papéis/grants) + higiene 9700 (PKCE; evitar Implicit; MUST NOT ROPC; redirect exato; Bearer nas APIs)  
B) Eureka = SSO; Zuul = PKCE; MapStruct = RFC 9700  
C) Password grant é a BCP moderna; Code flow está obsoleto  
D) OAuth2 = Service Discovery; Keycloak = Flyway  
E) RFC 9700 revoga a existência de Resource Servers  

---

### Gabarito comentado (treino 1–15)

1. **B** — Login único → vários serviços; não é discovery/mapper/migração.  
2. **C** — Keycloak = SSO da PDPJ no discurso do edital.  
3. **B** — 6749 = OAuth2 (autorização/tokens/grants).  
4. **C** — Quatro papéis clássicos.  
5. **B** — Keycloak como AS/IdP.  
6. **B** — Code intermediário ≠ access token.  
7. **C** — Bearer no `Authorization`.  
8. **B** — ROPC MUST NOT (9700).  
9. **B** — Protege o code (verifier/challenge).  
10. **B** — Implicit SHOULD NOT.  
11. **B** — Exact match × vazamento/validação frouxa.  
12. **B** — Trio SSO / Gateway / Discovery.  
13. **B** — Público → PKCE obrigatório.  
14. **B** — Access chama API; refresh renova.  
15. **A** — Síntese do arquivo 06.

---

## 3. Questões reais de concurso (16–25)

> Bloco substituído por questões **reais** de concurso (FCC preferencialmente; TRT/TRF/TJ quando necessário). O treino 1–15 e o Material permanecem inalterados.

**16.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60)

A autenticação na Plataforma Digital do Poder Judiciário PDPJ-Br ocorre por meio de uma solução A, baseada em um aplicativo open source de gestão de identidades e acesso, denominado B, com base no protocolo C. De acordo com o texto acima, A, B e C são, correta e respectivamente:

A) Keycloak – Docker – Simple Object Access Protocol (SOAP)

B) Single Sign-On (SSO) – Keycloak – OAuth2 (RFC 6749)

C) Single Sign-On (SSO) – RabbitMQ – Advanced Message Queuing Protocol (AMQP)

D) de access token – Keycloak – Simple Object Access Protocol (SOAP)

E) Keycloak – RabbitMQ – OAuth2 (RFC 4967)


**17.** (FCC | TRT17 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 48 de 70)

As versões atuais do Keycloak podem carregar configurações (`db`, `db-username`, `db-password`, `db-url`) de quatro fontes diferentes: de parâmetros de linha de comando, de variáveis de ambiente, de arquivos de configuração `.conf` criados pelo usuário e

A) do arquivo `keycloak.conf`, localizado no diretório conf da instalação.

B) dos arquivos `standalone.conf` e `server.conf`, localizados na pasta de configuração do projeto.

C) do repositório GitKeyCloak disponibilizado por seus criadores.

D) do arquivo `keyconfig.xml`, localizado na pasta de configuração da aplicação.

E) de arquivos de configuração do servidor de sincronização do Keycloak.


**18.** (FCC | TRF4 2025 | Técnico Judiciário, Área Apoio Especializado, Especialidade Desenvolvimento de Sistema da Informação | Questão 50 de 50)

O Art. 11 da Portaria CNJ nº 253/2020 estabelece que o acesso aos microsserviços deverá ser protegido com mecanismos de autenticação e autorização

A) baseados em OAuth2 a ser provido pelo serviço de Single SignOn da PDPJ-Br.

B) por meio de autenticação de usuário já cadastrado no sistema de processo eletrônico específico do tribunal em que o processo tramita.

C) e utilizar exclusivamente navegadores de internet homologados pelo Conselho Nacional de Justiça (CNJ).

D) baseados em SSL a serem providos pelo serviço de suporte operacional da PDPJ-Br.

E) mediante concórdia com os termos de uso da plataforma no primeiro acesso.


**19.** (FCC | TRT20 2024 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 55 de 60)

O parâmetro `scope` no OAuth 2.0 é usado para especificar

A) o método de autenticação do cliente.

B) o tipo de token de acesso a ser emitido.

C) os recursos e permissões que o cliente deseja acessar.

D) o servidor de autorização a ser utilizado.

E) o tempo de vida do token de acesso.


**20.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 30 de 60)

Considere a situação abaixo.Carlos abre um aplicativo no qual são oferecidas 3 formas de acesso à plataforma: com Google, com Facebook e com E-mail. Carlos clica no opção "Acessar com Google". O aplicativo faz uma requisição ao Google Accounts, pedindo uma chave de acesso para consumir um recurso protegido. Quando o Google Accounts recebe o pedido de autorização para acessar um recurso protegido, inicia-se o processo de identificação e autenticação.Surge a tela do Google para Carlos digitar seu e-mail (@gmail.com), seguida da tela (do Google) para Carlos digitar a senha. Nesse passo, Carlos identifica-se, autentica-se e consente que o aplicativo acesse os recursos protegidos em seu nome. Então, o Google Accounts emite um access token (chave de acesso) para o aplicativo, que poderá acessar os recursos protegidos.Nessa situação, que ilustra o funcionamento inicial do OAuth2 (RFC 6749), o Google Accounts é o I, Carlos é o II e o aplicativo é o III.Os roles que preenchem, correta e respectivamente, as lacunas I, II e III são:

A) Resource Server − Grant Owner − Protected Resource

B) Authorization Server − Client − Protected Resource

C) Resource Server − Grant Owner − Client

D) Request Server − Client − Resource Owner

E) Authorization Server − Resource Owner − Client


**21.** (FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 33 de 60)

Um Tribunal implementou um sistema de controle de acesso lógico para seus funcionários, utilizando um modelo de autenticação e autorização baseado em papéis (RBAC). O sistema exige que os funcionários realizem a autenticação forte antes de acessar dados críticos. Durante uma auditoria interna, foi observado que alguns usuários com funções administrativas estavam acessando recursos sem que a autenticação forte fosse exigida, o que representava um risco. O procedimento que deve ser implementado primeiramente para corrigir a falha e assegurar que a autenticação forte seja exigida para todos os usuários ao acessar recursos críticos é

A) implementar um log de auditoria mais robusto para monitorar os acessos realizados pelos administradores a todos os sistemas.

B) redefinir todas as senhas dos usuários administrativos como medida corretiva temporária.

C) configurar regras de autorização para que os administradores também estejam sujeitos ao controle de autenticação forte, sem exceções.

D) configurar uma política de tempo limite (timeout) para encerrar as sessões de usuários administrativos inativos.

E) adotar uma solução de Single Sign-On (SSO) para centralizar as autenticações dos usuários.


**22.** (FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60) *[aproximação temática]*

Ao usar o Gateway API com o Service Discovery no contexto da PDPJ-Br, a principal consequência para o frontend em uma aplicação baseada em microsserviços é que o

A) Gateway API diminui a necessidade de autenticação, uma vez que o Service Discovery é integrado ao SSO.

B) frontend deve configurar manualmente os IPs e portas dos serviços disponíveis na plataforma.

C) frontend se torna o responsável pela chamada direta de cada microsserviço individualmente.

D) Gateway API reduz o número de chamadas externas que o frontend precisa fazer, tornando a comunicação mais eficiente.

E) frontend deve gerenciar manualmente as atualizações de endpoint de cada serviço individual registrado.


**23.** (FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 47 de 60) *[aproximação temática]*

Estabelecer governança de segurança cibernética e fortalecer a gestão e coordenação integrada de ações de segurança cibernética nos órgãos do Poder Judiciário é um dos objetivos da Estratégia Nacional de Segurança da Informação e Cibernética do Poder Judiciário (ENSEC-PJ), que consta na Resolução nº

A) 335/2020.

B) 131/2021.

C) 253/2020.

D) 162/2021.

E) 396/2021.


**24.** (FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60) *[aproximação temática]*

Nos projetos da Plataforma Digital do Poder Judiciário PDPJ-Br,

A) os ambientes de deployment estão instalados em clusters Kubernetes, gerenciados por meio da ferramenta open source de orquestração de containers Rancher.

B) a aplicação deve possuir os arquivos `yanl`, para que o deploy ocorra de forma automática por meio de pipelines de CI/CD (Continuous Delivery e Continuous Integration) executados pelo GitLab do STF (Supremo Tribunal Federal).

C) a aplicação deve possuir um arquivo `Rancherfile`, especificando o modo pelo qual ela deve ser empacotada.

D) deve haver o arquivo `.gitlab-ci.xml` definindo a pipeline de deploy automático.

E) deve haver, na pasta `kubernets`, o arquivo de configuração do ingress da aplicação, que é obrigatório.


**25.** (FCC | TRT17 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 69 de 70) *[aproximação temática]*

No Art. 4º da Resolução nº 335 de 29/09/2020 é estabelecido que a Plataforma Digital do Poder Judiciário – PDPJ-Br adotará, obrigatoriamente, soluções que abranjam, dentre outros, os seguintes conceitos: modalidade, acessibilidade, usabilidade, microsserviços, computação em nuvem,

A) e processo eletrônico em plataforma privada.

B) ampla cobertura de testes, alto acoplamento, baixa coesão e modularização.

C) e desenvolvimento centralizado na esfera principal do Poder Judiciário.

D) e substituição das atividades rotineiras ou sequenciais por ações manuais.

E) e autenticação uniformizada.


---

### Gabarito (concurso reais 16–25)

| Questão | Gabarito | Citação |
|---------|----------|---------|
| 16 | **B** | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 28 de 60 |
| 17 | **A** | FCC | TRT17 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 48 de 70 |
| 18 | **A** | FCC | TRF4 2025 | Técnico Judiciário, Área Apoio Especializado, Especialidade Desenvolvimento de Sistema da Informação | Questão 50 de 50 |
| 19 | **C** | FCC | TRT20 2024 | Técnico Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 55 de 60 |
| 20 | **E** | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 30 de 60 |
| 21 | **C** | FCC | TRT15 2024 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 33 de 60 |
| 22 | **D** \* | FCC | TRT6 2025 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 39 de 60 |
| 23 | **E** \* | FCC | TRT22 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 47 de 60 |
| 24 | **A** \* | FCC | TRT19 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 29 de 60 |
| 25 | **E** \* | FCC | TRT17 2022 | Analista Judiciário, Área Apoio Especializado, Especialidade Tecnologia da Informação | Questão 69 de 70 |

\* = aproximação temática (questão real correlata quando o nicho é raro em prova).

**Fontes:**
- Q16: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q28
- Q17: https://www.quizinho.com.br/questao/fcc-trt17-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q48
- Q18: https://www.quizinho.com.br/questao/fcc-trf4-2025-tecnico-judiciario-area-apoio-especializado-especialidade-desenvolvimento-de-sistema-da-informacao-2025-q50
- Q19: https://www.quizinho.com.br/questao/fcc-trt20-2024-tecnico-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q55
- Q20: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q30
- Q21: https://www.quizinho.com.br/questao/fcc-trt15-2024-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2024-q33
- Q22: https://www.quizinho.com.br/questao/fcc-trt6-2025-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2025-q39
- Q23: https://www.quizinho.com.br/questao/fcc-trt22-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q47
- Q24: https://www.quizinho.com.br/questao/fcc-trt19-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q29
- Q25: https://www.quizinho.com.br/questao/fcc-trt17-2022-analista-judiciario-area-apoio-especializado-especialidade-tecnologia-da-informacao-2022-q69

*Arquivo 06 · SSO / Keycloak / OAuth2 · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 2 — autenticação PDPJ.*

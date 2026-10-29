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

## 3. Questões estilo concurso FCC (16–25)

**16.** Em arquitetura de microsserviços alinhada à PDPJ-Br, o serviço de autenticação unificada (*Single Sign-On*) baseado em Keycloak tem por finalidade, em regra:

A) substituir o catálogo de instâncias do Service Discovery e eliminar a necessidade de nomes lógicos de serviço  
B) permitir que usuários autentiquem-se uma vez e consumam múltiplos sistemas/módulos mediante tokens, sob protocolo OAuth 2.0  
C) mapear entidades JPA em DTOs em tempo de compilação  
D) versionar automaticamente scripts DDL no lugar do Flyway  
E) atuar exclusivamente como balanceador L4 sem emissão de credenciais  

**17.** Acerca do OAuth 2.0 (RFC 6749), assinale a alternativa correta quanto aos papéis definidos pelo framework.

A) O *Authorization Server* armazena, por definição, todos os dados de negócio do Resource Owner no lugar do Resource Server  
B) O *Client* é a aplicação que solicita autorização; o *Authorization Server* autentica o Resource Owner e emite tokens; o *Resource Server* hospeda o recurso protegido  
C) O *Resource Owner* é sinônimo estrito de Eureka Client  
D) Não há distinção entre Client e Resource Server na RFC 6749  
E) O único papel existente é o API Gateway  

**18.** Considere as afirmações abaixo, à luz da RFC 9700 (*OAuth 2.0 Security Best Current Practice*).

I. Clientes públicos que utilizam o Authorization Code Flow devem empregar PKCE.  
II. O *Resource Owner Password Credentials grant* não deve ser utilizado.  
III. A comparação de URIs de redirecionamento deve, em regra, observar *exact string matching*.

Está correto o que se afirma em:

A) I, apenas  
B) II, apenas  
C) I e II, apenas  
D) II e III, apenas  
E) I, II e III  

**19.** No contraste entre o *Authorization Code grant* e o *Implicit grant*, é correto afirmar, em linha com as boas práticas atuais (RFC 9700), que:

A) o Implicit deve ser preferido porque entrega o access token diretamente no *user agent*, eliminando riscos de vazamento  
B) clients devem, em regra, evitar o Implicit e preferir o Authorization Code, tipicamente conjugado a PKCE  
C) ambos são proibidos de forma absoluta pela RFC 6749 desde a publicação original  
D) o Authorization Code dispensa qualquer *token endpoint*  
E) o Implicit é obrigatório para clientes confidential  

**20.** Em enunciado que descreve componente responsável por “autenticar o usuário no realm, federar identidades e emitir *access tokens* consumidos pelas APIs via Bearer”, o papel mais adequado é o de:

A) Eureka Server como *registry* puro de heartbeats  
B) MapStruct  
C) Keycloak / Authorization Server do SSO  
D) Flyway  
E) Hibernate Envers  

**21.** Assinale a opção que **inverte** indevidamente conceitos deste eixo.

A) Keycloak emite tokens; APIs validam Bearer; Gateway pode checar token na borda  
B) Eureka localiza réplicas; Keycloak autentica/autoriza via OAuth; Zuul encaminha na borda  
C) Authorization Code troca *code* por token no *token endpoint*  
D) Keycloak = Service Discovery; Eureka = emissor de JWT do SSO; password grant = BCP RFC 9700  
E) PKCE usa *code_verifier* / *code_challenge* (preferência S256)  

**22.** A utilização do cabeçalho HTTP `Authorization` com esquema Bearer, no consumo de microsserviços protegidos pelo SSO da PDPJ, relaciona-se à ideia de:

A) enviar o *access token* obtido junto ao Authorization Server para autenticação/autorização da requisição no Resource Server  
B) registrar a instância no Eureka a cada request  
C) gerar automaticamente o Swagger UI  
D) aplicar *soft delete* via Envers  
E) substituir TLS por plain HTTP obrigatório  

**23.** Considere o uso conjugado de **API Gateway**, **Keycloak** e **Eureka** em stack alinhada ao conteúdo programático. Assinale a alternativa correta.

A) Eureka emite authorization codes; Keycloak mantém o *registry*; Gateway é IdP exclusivo  
B) Keycloak cuida de SSO/tokens; Gateway de entrada/roteamento (podendo validar token); Eureka de localização de instâncias — papéis distintos e complementares  
C) Os três são sinônimos de Implicit grant  
D) Gateway substitui a RFC 6749; Keycloak substitui JSON  
E) Eureka obriga o uso de ROPC  

**24.** Para fins de preparação no eixo SSO / Keycloak / OAuth2 do edital (PDPJ-Br / Analista TI), a síntese mais adequada é:

A) RFC 6749 define papéis e grants; RFC 9700 endurece a segurança (PKCE, exact redirect, evitar Implicit, MUST NOT password); Keycloak materializa o SSO; APIs usam Bearer  
B) Password grant é a única prática recomendada pela 9700; Implicit é obrigatório; Keycloak equivale a Flyway  
C) OAuth2 resolve Discovery; SSO elimina Resource Servers  
D) Exact matching de redirect é desencorajado; open redirectors são recomendados  
E) Clientes públicos devem expor *client_secret* e evitar PKCE  

**25.** Assinale a afirmação **incompatível** com as boas práticas da RFC 9700 e o desenho SSO/Keycloak cobrado.

A) Preferir Authorization Code com PKCE (S256)  
B) Validar tokens no Resource Server / borda conforme o desenho  
C) Restringir *scopes* e *audience* ao mínimo necessário  
D) Adotar ROPC (*password*) como grant padrão de segurança moderna e abandonar o Authorization Code  
E) Evitar open redirectors e Implicit que devolvem access token no redirect  

---

### Gabarito (estilo concurso 16–25)

| Questão | Gabarito |
|---------|----------|
| 16 | **B** |
| 17 | **B** |
| 18 | **E** |
| 19 | **B** |
| 20 | **C** |
| 21 | **D** |
| 22 | **A** |
| 23 | **B** |
| 24 | **A** |
| 25 | **D** |

**Notas rápidas:** 16 B (SSO único → vários serviços via OAuth/tokens); 17 B (papéis 6749); 18 E (I–III alinhados à 9700); 19 B (evitar Implicit; preferir Code+PKCE); 20 C (Keycloak/AS); 21 D (única que **inverte** tudo); 22 A (Bearer = access token na API); 23 B (papéis complementares); 24 A (síntese 6749+9700+Keycloak); 25 D (ROPC como “padrão moderno” é incompatível com 9700).

---

### Verificação interna (consistência)

| Checagem | Status |
|----------|--------|
| Edital: SSO + Keycloak + OAuth2 RFC 6749 + boas práticas RFC 9700 | Coberto |
| PDPJ: SSO Keycloak; Bearer; federação; adesão de módulos | Coberto (visão cobrável) |
| Treino 1–15 com gabarito comentado | 15/15 |
| FCC 16–25 com tabela + notas | 10/10 |
| Distratores clássicos (Eureka/Zuul/MapStruct/Flyway/Envers; Implicit×Code; ROPC) | Presentes |
| Gabaritos únicos (sem ambiguidade intencional) | Conferidos |
| Anti-confusão Gateway × AS × Discovery | Seções 1.4, 1.13, 1.15 + Q12/Q21/Q23 |

---

*Arquivo 06 · SSO / Keycloak / OAuth2 · Claudionir · TRT4 Analista TI (código 14) · FCC. Semana 2 — autenticação PDPJ.*

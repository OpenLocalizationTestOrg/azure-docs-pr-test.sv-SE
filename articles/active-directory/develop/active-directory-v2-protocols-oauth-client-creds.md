---
title: "aaaUse Azure AD v2.0 tooaccess skydda resurser utan användaråtgärder | Microsoft Docs"
description: "Skapa webbprogram med hjälp av hello Azure AD-implementeringen av hello OAuth 2.0-autentiseringsprotokollet."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 9b7cfbd7-f89f-4e33-aff2-414edd584b07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 0003ec836d633a5466c48033adedac1108f27203
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Azure Active Directory v2.0 och hello OAuth 2.0 klientens autentiseringsuppgifter flöda
Du kan använda hello [OAuth 2.0 klientens autentiseringsuppgifter bevilja](http://tools.ietf.org/html/rfc6749#section-4.4), kallas ibland *tvåledade OAuth*, tooaccess webbaserat resurser med hjälp av hello identiteten för ett program. Den här typen av bevilja ofta används för server-till-server-interaktioner som måste köras i bakgrunden hello utan direkt interaktion med användaren. Dessa typer av program som ofta är refererad tooas *daemons* eller *tjänstkonton*.

> [!NOTE]
> hello v2.0-slutpunkten stöder inte alla Azure Active Directory-scenarier och funktioner. toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
>
>

I hello vanligare *treledade OAuth*, ett klientprogram är beviljade behörigheten tooaccess en resurs för en viss användares räkning. hello behörigheten delegerad hello användaren toohello, vanligtvis under programmet hello [medgivande](active-directory-v2-scopes.md) process. Men i hello klientautentiseringsuppgifter beviljas behörigheter direkt toohello programmet. När hello appen visas en token tooa resurs, tillämpar hello resurs att själva hello appen har tillståndet tooperform en åtgärd och inte hello användaren har behörighet.

## Protokollet diagram
hello hela klientautentiseringsuppgifter verkar liknande toohello nästa diagram. Vi beskrivs hello stegen nedan.

![Klientautentiseringsuppgifter](../../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## Hämta direkt auktorisering
En app vanligtvis tar emot direkt auktorisering tooaccess en resurs i ett av två sätt: via en åtkomstkontrollista (ACL) på hello resurs eller programmet behörighetstilldelningen i Azure Active Directory (AD Azure). Dessa metoder är hello vanligaste i Azure AD och rekommenderar vi dem för klienter och resurser för hello klientautentiseringsuppgifter. En resurs kan välja tooauthorize klienter på andra sätt, men. Varje resursservern kan välja hello-metod som passar hello för sina program.

### Listor för åtkomstkontroll
En resursleverantör kan tvinga en kontroll av tillståndet baserat på en lista över program-ID som den vet och ger en viss nivå av åtkomst till. När hello resurs tar emot en token från hello v2.0-slutpunkten, det kan avkoda hello token och extrahera hello klientens program-ID från hello `appid` och `iss` anspråk. Den jämför sedan hello programmet mot en ACL som den upprätthåller. Hej ACL granularitet och metod kan variera avsevärt mellan resurser.

Ett vanligt användningsfall är toouse en ACL-toorun testar för ett webbprogram eller för en webb-API. hello webb-API kan ge en delmängd av fullständig behörighet tooa viss klient. toorun slutpunkt till slutpunkt tester på hello API, skapa en Testklient som hämtar token från hello v2.0-slutpunkten och skickar dem sedan toohello API. hello API och sedan kontrollerar hello ACL för hello testa klientens program-ID för fullständig åtkomst toohello API: er hela funktioner. Om du använder den här typen av ACL, måste toovalidate inte bara hello anroparens `appid` värde. Verifiera att hello `iss` värdet för hello token är betrodd.

Den här typen av auktorisering är vanligt för Daemon och tjänstkonton som behöver tooaccess data som ägs av konsumentanvändare med personliga Microsoft-konton. För data som ägs av organisationer rekommenderar vi att du får hello nödvändiga auktorisering via behörigheter för program.

### Behörigheter för program
Du kan använda API: er tooexpose en uppsättning behörigheter för program i stället för med ACL: er. Ett program behörigheten tooan program av en organisations administratör och kan användas endast tooaccess data ägs av den organisationen och dess anställda. Till exempel visar Microsoft Graph flera program behörigheter toodo hello följande:

* Läsa e-post i alla postlådor
* Läsa och skriva e-post i alla postlådor
* Skicka e-post som valfri användare
* Läsa katalogdata

Mer information om behörigheter för program gå för[Microsoft Graph](https://graph.microsoft.io).

behörigheter för toouse program i din app hello steg diskuterar vi hello nästa avsnitt.

#### Begär hello behörighet i portalen för registrering av hello-app
1. Gå tooyour programmet hello [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller [skapa en app](active-directory-v2-app-registration.md), om du inte redan har gjort. Du behöver toouse minst en Programhemlighet när du skapar din app.
2. Leta upp hello **direkt programbehörigheter** avsnittet och Lägg sedan till hello behörigheter som krävs för din app.
3. **Spara** hello appregistrering.

#### Rekommenderat: Logga hello användaren i tooyour app
När du skapar ett program som använder behörigheter för program kräver normalt hello app en sida eller vy på vilken hello admin godkänner hello app-behörigheter. Den här sidan kan vara en del av hello app inloggning flödet, en del av hello app-inställningar, eller så kan vara ett dedikerat ”ansluta” flöde. I många fall klokt det för hello app tooshow detta ”ansluta” Visa endast när en användare har loggat in med ett arbets- eller skolkonto Microsoft-konto.

Om du registrerar hello användaren i tooyour app kan du identifiera hello organisation toowhich hello användaren tillhör innan du be hello användaren tooapprove hello programbehörigheter. Även om det inte är absolut nödvändigt, hjälper som dig att skapa en mer intuitiv upplevelse för användarna. toosign hello användare i Följ våra [v2.0-protokollet självstudier](active-directory-v2-protocols.md).

#### Begär hello behörighet från en katalogadministratör
När du är klar toorequest behörigheter från hello organisations administratör kan du omdirigera hello användaren toohello v2.0 *medgivande adminslutpunkten*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting hello following request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | Villkor | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |hello directory-klient som du vill toorequest tillstånd. Detta kan vara i GUID- eller format för eget namn. Om du inte vet vilken klient hello användaren tillhör tooand som du vill toolet dem logga in med en klient, Använd `common`. |
| client_id |Krävs |hello program-ID som hello [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tilldelade tooyour app. |
| redirect_uri |Krävs |hello omdirigerings-URI där du vill att hello svar toobe skickas för din app toohandle. Den måste matcha en hello omdirigerings-URI: er som du har registrerat i hello portal, förutom att det måste vara URL-kodade och det kan ha ytterligare sökvägssegment. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som också returneras hello token svar. Det kan vara en sträng med innehåll som du vill använda. hello tillstånd är används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller vy som de var på. |

Azure AD tillämpar nu att endast en Innehavaradministratör kan logga in toocomplete hello begäran. Hej administratör ombeds tooapprove alla hello direkt tillämpning behörigheter som du har begärt för din app i portalen för registrering av hello-app.

##### Lyckat svar
Om Hej administratör godkänner hello behörigheter för ditt program, hello lyckat svar som ser ut så här:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beskrivning |
| --- | --- | --- |
| Klient |hello directory-klient som beviljats behörighet programmet hello som den begärda, i GUID-format. |
| state |Ett värde som ingår i hello-begäran som också returneras hello token svar. Det kan vara en sträng med innehåll som du vill använda. hello tillstånd är används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller vy som de var på. |
| admin_consent |Ställa in också**SANT**. |

##### Felsvar
Om Hej administratör inte godkänner hello behörigheter för ditt program misslyckades hello svar ser ut så här:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beskrivning |
| --- | --- | --- |
| fel |En felsträng kod som du kan använda tooclassify typer av fel och som du kan använda tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa dig att identifiera hello orsaken till felet. |

När du har mottagit ett lyckat svar från hello app etablering slutpunkten kan har din app fått hello direkt tillämpning behörigheter som begärde. Nu kan du begära en token för hello-resurs som du vill använda.

## Hämta en token
När du har skaffat hello nödvändiga auktorisering för ditt program, fortsätter du med erhålla åtkomsttoken för API: er. tooget en token med hjälp av hello klientens autentiseringsuppgifter bevilja skicka en POST-begäran toohello `/token` v2.0-slutpunkten:

### Först fall: token åtkomst-begäran med en delad hemlighet

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parameter | Villkor | Beskrivning |
| --- | --- | --- |
| client_id |Krävs |hello program-ID som hello [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tilldelade tooyour app. |
| Omfång |Krävs |hello-värdet som skickades för hello `scope` parameter i den här begäran ska vara hello URI resource identifier (program-ID) av hello-resurs som du vill, försett med hello `.default` suffix. Exempelvis hello Microsoft Graph hello-värdet är `https://graph.microsoft.com/.default`. Det här värdet informerar hello v2.0-slutpunkten att alla hello direkt tillämpning behörigheter du har konfigurerat för din app det ska utfärda en token för hello som associerats med hello-resurs som du vill toouse. |
| client_secret |Krävs |Hej Programhemlighet som du skapade för din app i portalen för registrering av hello-app. |
| grant_type |Krävs |Måste vara `client_credentials`. |

### Andra fall: token åtkomst-begäran med ett certifikat

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

| Parameter | Villkor | Beskrivning |
| --- | --- | --- |
| client_id |Krävs |hello program-ID som hello [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tilldelade tooyour app. |
| Omfång |Krävs |hello-värdet som skickades för hello `scope` parameter i den här begäran ska vara hello URI resource identifier (program-ID) av hello-resurs som du vill, försett med hello `.default` suffix. Exempelvis hello Microsoft Graph hello-värdet är `https://graph.microsoft.com/.default`. Det här värdet informerar hello v2.0-slutpunkten att alla hello direkt tillämpning behörigheter du har konfigurerat för din app det ska utfärda en token för hello som associerats med hello-resurs som du vill toouse. |
| client_assertion_type |Krävs |hello-värdet måste vara`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Krävs | Ett intyg (en JSON Web Token) att du behöver toocreate och logga in med hello certifikat du registrerad som autentiseringsuppgifterna för ditt program. Läs mer om [certifikat autentiseringsuppgifter](active-directory-certificate-credentials.md) toolearn hur tooregister ditt certifikat och hello formatering av hello-kontrollen.|
| grant_type |Krävs |Måste vara `client_credentials`. |

Observera att hello parametrar är nästan hello samma som hello fallet med hello begäran av delad hemlighet förutom att hello client_secret parameter har ersatts av två parametrar: en client_assertion_type och client_assertion.

### Lyckat svar
Ett lyckat svar ser ut så här:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parameter | Beskrivning |
| --- | --- |
| access_token |hello begärda åtkomst-token. hello app kan använda den här token tooauthenticate toohello skyddad resurs, till exempel tooa Web API. |
| token_type |Anger hello tokentypen värdet. Hej typ som har stöd för Azure AD är `bearer`. |
| expires_in |Hur länge hello åtkomst-token är giltig (i sekunder). |

### Felsvar
Ett felsvar ser ut så här:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: hello provided value for hello input parameter 'scope' is not valid. hello scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som du kan använda tooclassify typer av fel som inträffar och tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa dig identifiera hello grundorsaken till ett autentiseringsfel. |
| error_codes |En lista över STS-specifika felkoder som kan hjälpa dig med diagnostik. |
| tidsstämpel |hello tid då hello-fel inträffade. |
| trace_id |En unik identifierare för hello-begäran som kan hjälpa dig med diagnostik. |
| correlation_id |En unik identifierare för hello-begäran som kan hjälpa dig med diagnostik för komponenter. |

## Använda en token
Nu när du har skaffat en token, använder du hello token toomake begäranden toohello resursen. När hello-token upphör att gälla, upprepa hello begäran toohello `/token` endpoint tooacquire en ny åtkomsttoken.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro tip: Try hello following command! (Replace hello token with your own.)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## Kodexempel
toosee ett exempel på ett program som implementerar hello klientens autentiseringsuppgifter bevilja med hjälp av hello admin medgivande slutpunkt, finns våra [v2.0 daemon kodexemplet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

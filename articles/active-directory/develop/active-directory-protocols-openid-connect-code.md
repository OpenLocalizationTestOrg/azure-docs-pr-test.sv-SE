---
title: "aaaUnderstand hello OpenID Connect kod autentiseringsflödet i Azure AD | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse HTTP meddelanden tooauthorize åt tooweb program och webb-API: er i din klient med hjälp av Azure Active Directory och OpenID Connect."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: fafd8ab906ee576c584fec2ef1e9de83ddb1f6e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Auktorisera åtkomst tooweb program med OpenID Connect och Azure Active Directory
[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) är en enkel Identitetslagret som bygger på hello OAuth 2.0-protokollet. OAuth 2.0 definierar metoder tooobtain och Använd **åtkomst till token** tooaccess skyddade resurser, men de inte definiera standardmetoderna tooprovide identitetsinformation. OpenID Connect implementerar autentisering som ett tillägg toohello OAuth 2.0 auktoriseringsprocessen. Den ger information om hello slutanvändaren i hello form av en `id_token` som verifierar hello hello användares identitet och ger grundläggande profilinformation om hello användare.

OpenID Connect är vår rekommendation om du skapar ett program som finns på en server och som nås via en webbläsare.


[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)] 

## Autentiseringsflödet med OpenID Connect
hello mest grundläggande inloggning flödet innehåller följande hello - dem beskrivs i detalj nedan.

![OpenId Connect Autentiseringsflödet](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## OpenID Connect Metadatadokumentet

OpenID Connect beskriver ett metadata-dokument som innehåller de flesta av hello information som krävs för en app tooperform inloggning. Detta omfattar information som hello URL: er toouse och hello plats signering offentliga nycklar som hello-tjänsten. Hej OpenID Connect Metadatadokumentet finns på:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
hello metadata är ett enkelt JavaScript Object Notation (JSON)-dokument. Se hello följande kodavsnitt som ett exempel. hello fragments innehållet fullständigt beskrivs i hello [OpenID Connect specifikationen](https://openid.net).

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/common/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/common/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    
    ...
}
```

## Skicka förfrågan om hello-inloggning
När ditt webbprogram måste tooauthenticate hello användare, måste den dirigera hello användaren toohello `/authorize` slutpunkt. Den här begäran är liknande toohello första ben hello [OAuth 2.0 auktorisering kod flöda](active-directory-protocols-oauth-code.md), med några viktiga skillnader:

* hello begäran måste innehålla hello omfånget `openid` i hello `scope` parameter.
* Hej `response_type` parameter måste innehålla `id_token`.
* hello begäran måste innehålla hello `nonce` parameter.

Så att en exempel-begäran skulle se ut så här:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parameter |  | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Hej `{tenant}` värdet hello sökvägen till hello begäran kan vara används toocontrol som kan logga in på programmet hello.  hello tillåtna värden är klient-ID: n, till exempel `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` eller `contoso.onmicrosoft.com` eller `common` för klient-oberoende token |
| client_id |Krävs |hello program-Id tilldelade tooyour app när du har registrerat med Azure AD. Du hittar du i hello Azure-portalen. Klicka på **Azure Active Directory**, klickar du på **App registreringar**, Välj hello program och leta upp hello program-Id på sidan för hello-programmet. |
| response_type |Krävs |Måste innehålla `id_token` för OpenID Connect inloggning.  Den kan också omfatta andra response_types som `code`. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope.  Det måste innehålla hello omfånget för OpenID Connect `openid`, som översätter toohello ”logga in dig på” behörighet i hello medgivande Användargränssnittet.  Du kan också omfatta andra scope i den här förfrågan för att begära godkännande. |
| temporärt ID |Krävs |Ett värde som ingår i hello-begäran som skapats av hello-app som ingår i hello resulterande `id_token` som ett anspråk.  hello app kan kontrollera det här värdet toomitigate token replay-attacker.  hello-värdet är vanligtvis en slumpmässig, unik sträng eller ett GUID som kan använda tooidentify hello ursprung hello-begäran. |
| redirect_uri |Rekommenderas |Hej redirect_uri för din app, där autentisering svar kan skickas och tas emot av din app.  Den måste matcha en hello redirect_uris som du har registrerat i hello portal, förutom det måste vara url-kodade. |
| response_mode |Rekommenderas |Anger hello-metod som ska använda toosend hello resulterande authorization_code tillbaka tooyour app.  Värden som stöds är `form_post` för *HTTP formuläret post* eller `fragment` för *URL-fragment*.  För webbprogram, bör du använda `response_mode=form_post` tooensure hello mest säker överföring av token tooyour program. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som returneras hello token svar.  Det kan vara en sträng med innehåll som du vill.  Ett slumpmässigt genererat unikt värde används vanligtvis för [förhindra attacker med förfalskning av begäran](http://tools.ietf.org/html/rfc6749#section-10.12).  hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller vy som de var på. |
| kommandotolk |Valfria |Anger hello användarinteraktion som krävs.  För närvarande hello endast giltiga värden är ”inloggning” none, och 'godkännande'.  `prompt=login`Tvingar hello användaren tooenter sina autentiseringsuppgifter på begäran, giltigt att negera enkel inloggning på.  `prompt=none`är hello motsatt - ser till att hello inte användaren med en interaktiv prompt helst.  Om hello begäran inte kan slutföras utan meddelanden via enkel inloggning på returnerar hello endpoint ett fel.  `prompt=consent`utlöser hello OAuth medgivande dialogrutan efter hello användaren loggar in, ber hello användaren toogrant behörigheter toohello app. |
| login_hint |Valfria |Kan vara toopre Fyll hello användarnamn/e-adressfältet i hello-inloggningssida för hello användaren, om du känner till sitt lösenord i förväg.  Ofta appar använder den här parametern under omautentisering som redan har extraherats hello användarnamn från en tidigare inloggning med hello `preferred_username` anspråk. |

Nu är hello användaren tillfrågas tooenter sina autentiseringsuppgifter och fullständig hello autentisering.

### Exempelsvar
En exempelsvar när hello användaren har autentiserats kan se ut så här:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beskrivning |
| --- | --- |
| id_token |Hej `id_token` hello appen begärdes. Du kan använda hello `id_token` tooverify hello användarens identitet och starta en session med hello användare. |
| state |Ett värde som ingår i hello-begäran som returneras också hello token svar. Ett slumpmässigt genererat unikt värde används vanligtvis för [förhindra attacker med förfalskning av begäran](http://tools.ietf.org/html/rfc6749#section-10.12).  hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller vy som de var på. |

### Felsvar
Felsvar kan också skickas toohello `redirect_uri` så hello app kan hantera dem på rätt sätt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |

#### Felkoder för auktorisering endpoint fel
hello följande tabell beskrivs hello olika felkoder som kan returneras i hello `error` parametern för hello felsvar.

| Felkod | Beskrivning | Klientåtgärd |
| --- | --- | --- |
| invalid_request |Protokollfel, till exempel en obligatorisk parameter saknas. |Åtgärda och skicka hello begäran igen. Detta är ett utvecklingsfel och vanligtvis fångas upp under inledande testningen. |
| unauthorized_client |hello klientprogrammet är inte tillåtna toorequest en Auktoriseringskoden. |Detta inträffar vanligtvis när hello klientprogrammet inte har registrerats i Azure AD eller läggs inte toohello användarens Azure AD-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |
| Om ACCESS_DENIED |Resursägare nekad medgivande |hello klientprogrammet kan meddela hello användare som det går inte att fortsätta om hello användaren godkänner. |
| unsupported_response_type |hello auktorisering servern stöder inte hello svarstyp i hello-begäran. |Åtgärda och skicka hello begäran igen. Detta är ett utvecklingsfel och vanligtvis fångas upp under inledande testningen. |
| server_error |hello-servern påträffade ett oväntat fel. |Försök igen med hello-begäran. Dessa fel kan bero på tillfälliga förhållanden. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av tooa tillfälligt fel. |
| temporarily_unavailable |hello-server är tillfälligt överbelastad toohandle hello begäran. |Försök igen med hello-begäran. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av tooa tillfälligt tillstånd. |
| invalid_resource |hello målresursen är ogiltig eftersom den inte finns, Azure AD kan inte hitta den eller det är inte korrekt konfigurerad. |Detta anger hello resursen om den finns inte har konfigurerats i hello-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |

## Validera hello id_token
Ta bara emot en `id_token` är inte tillräcklig tooauthenticate hello användare, måste du verifiera hello signatur och verifiera hello anspråk i hello `id_token` per krav som din app. hello Azure AD-slutpunkten använder JSON Web token (JWTs) och kryptering med offentlig nyckel toosign token och verifiera att de är giltiga.

Du kan välja toovalidate hello `id_token` i klientkod, men vanligt är toosend hello `id_token` tooa backend-servern och gör det hello-verifiering. När du har verifierats hello signaturen för hello `id_token`, det finns några anspråk som du är nödvändiga tooverify.

Du kan också toovalidate ytterligare anspråk beroende på ditt scenario. Några vanliga verifieringar inkluderar:

* Säkerställa hello användarorganisation har registrerat dig för hello app.
* Säkerställa hello användaren har rätt tillstånd/privilegier
* Säkerställa en vissa styrkan hos autentisering har inträffat, till exempel multifaktorautentisering.

När du har validerat hello `id_token`, du kan starta en session med hello användare och använda hello anspråk i hello `id_token` tooobtain information om hello användare i din app. Den här informationen kan användas för att visa, poster, tillstånd och så vidare. Mer information om hello typer av token och anspråk [stöds Token och anspråkstyper](active-directory-token-and-claims.md).

## Skicka en begäran om utloggning
När du vill toosign hello användare utanför hello appen är inte tillräckligt mycket tooclear appens cookies eller på annat sätt avsluta hello-session med hello användare.  Du måste också omdirigera hello användaren toohello `end_session_endpoint` för utloggning.  Om du inte toodo så kommer hello användaren att kunna tooreauthenticate tooyour app utan att ange sina autentiseringsuppgifter igen, eftersom de har en giltig inloggning session med hello Azure AD-slutpunkten.

Du kan bara omdirigera hello användaren toohello `end_session_endpoint` anges i hello OpenID Connect metadata dokument:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameter |  | Beskrivning |
| --- | --- | --- |
| post_logout_redirect_uri |Rekommenderas |hello-URL som hello användaren ska vara omdirigerade tooafter lyckats logga ut.  Om inte ingår, visas hello användare ett allmänt meddelande. |

## Enkel utloggning
När du omdirigerar hello användaren toohello `end_session_endpoint`, Azure AD rensar hello användarens session från hello webbläsare. Dock hello användaren kan fortfarande vara inloggad tooother program som använder Azure AD för autentisering. tooenable dessa program toosign Hej användaren ut samtidigt, Azure AD skickar en HTTP GET-begäran toohello registrerad `LogoutUrl` för alla program som hello hello användaren är inloggad till. Program måste svara toothis begäran genom att avmarkera alla sessioner som identifierar hello användar- och returnera ett `200` svar.  Om du vill ut toosupport för enkel inloggning i ditt program måste du implementera exempelvis en `LogoutUrl` i din programkod.  Du kan ange hello `LogoutUrl` från hello Azure-portalen:

1. Navigera toohello [Azure Portal](https://portal.azure.com).
2. Välj din Active Directory genom att klicka på ditt konto i hello övre högra hörnet av hello-sidan.
3. Hello vänstra navigeringsfönstret, Välj **Azure Active Directory**, Välj **App registreringar** och välj ditt program.
4. Klicka på **egenskaper** och hitta hello **logga ut URL** textruta. 

## Token förvärv
Många webbprogram måste toonot endast logga hello användare i, men också komma åt en webbtjänst som användaren använder sig av OAuth. Det här scenariot kombinerar OpenID Connect för autentisering av användare vid hämtning av samtidigt en `authorization_code` som kan vara används tooget `access_tokens` med hello OAuth Authorization kod flöda.

## Få åtkomst-token
tooacquire åtkomsttoken, behöver du toomodify hello inloggning begäran från ovan:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                     
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

Genom att inkludera behörighetsomfattningen i hello begäran och `response_type=code+id_token`, hello `authorize` endpoint garanterar att hello-användaren har godkänt toohello behörigheter anges i hello `scope` Frågeparametern och returnera ett auktoriseringskod för din app tooexchange för en åtkomst-token.

### Lyckat svar
Ett lyckat svar med `response_mode=form_post` ser ut som:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beskrivning |
| --- | --- |
| id_token |Hej `id_token` hello appen begärdes. Du kan använda hello `id_token` tooverify hello användarens identitet och starta en session med hello användare. |
| Koden |Hej authorization_code som hello app som begärdes. hello app kan använda hello auktorisering koden toorequest en åtkomst-token för hello målresursen. Authorization_codes är kort livslängd och vanligtvis ut efter 10 minuter. |
| state |Om en parametern state ingår i hello begäran ska hello samma värde visas i hello svar. hello app bör kontrollera att hello tillstånd värden i hello förfrågan och svar är identiska. |

### Felsvar
Felsvar kan också skickas toohello `redirect_uri` så hello app kan hantera dem på rätt sätt:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |

En beskrivning av hello felkoder och deras rekommenderade klientåtgärd finns [felkoder för auktorisering endpoint fel](#error-codes-for-authorization-endpoint-errors).

När du har tagit emot ett tillstånd `code` och en `id_token`, du kan hello användaren logga in och få åtkomst-token för deras räkning.  toosign hello användare i, måste du verifiera hello `id_token` exakt så som beskrivs ovan. tooget åtkomsttoken som du kan följa hello stegen som beskrivs i avsnittet ”använda hello auktorisering koden toorequest en åtkomst-token” Hej för i vår [dokumentationen för OAuth-protokollet](active-directory-protocols-oauth-code.md).

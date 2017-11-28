---
title: aaaAzure Active Directory v2.0 och hello OpenID Connect-protokollet | Microsoft Docs
description: "Skapa webbprogram med hjälp av hello Azure AD v2.0-implementeringen av hello OpenID Connect-autentiseringsprotokollet."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: a4875997-3aac-4e4c-b7fe-2b4b829151ce
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 277bb406dbb24ccefb09e4e4c928f0421755cf61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Azure Active Directory v2.0 och hello OpenID Connect-protokollet
OpenID Connect är ett autentiseringsprotokoll som bygger på OAuth 2.0 som du kan använda toosecurely tecken i ett användaren tooa webbprogram. När du använder hello v2.0 endpoint implementering av OpenID Connect kan du lägga till inloggnings- och API åtkomst tooyour webbaserade program. I den här artikeln visar vi dig hur toodo detta oberoende av språk. Vi beskriver hur toosend och ta emot HTTP-meddelanden utan att använda alla bibliotek för Microsoft öppen källkod.

> [!NOTE]
> hello v2.0-slutpunkten har inte stöd för alla Azure Active Directory-scenarier och funktioner. toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) utökar hello OAuth 2.0 *auktorisering* protokollet toouse som en *autentisering* protokoll, så att du kan använda enkel inloggning med hjälp av OAuth. OpenID Connect introducerar hello begreppet en *ID token*, vilket är en säkerhetstoken som tillåter hello tooverify hello användares hello identitet. hello-ID-token hämtar också grundläggande profilinformation om hello användare. Eftersom OpenID Connect utökar OAuth 2.0, appar på ett säkert sätt hämta *åtkomst till token*, vilket kan vara används tooaccess resurser som skyddas av en [auktorisering server](active-directory-v2-protocols.md#the-basics). Vi rekommenderar att du använder att OpenID Connect om du skapar en [webbprogrammet](active-directory-v2-flows.md#web-apps) som finns på en server och nås via en webbläsare.

## Protokollet diagram: Logga in
hello har mest grundläggande inloggning flöde hello steg som visas i nästa hello-diagram. Varje steg i detalj i den här artikeln beskrivs.

![OpenID Connect protocol: Logga in](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

## Hämta Metadatadokumentet för hello OpenID Connect
OpenID Connect beskriver ett metadata-dokument som innehåller de flesta av hello information som krävs för en app tooperform inloggning. Detta omfattar information som hello URL: er toouse och hello plats signering offentliga nycklar som hello-tjänsten. För hello v2.0-slutpunkten är hello OpenID Connect Metadatadokumentet bör du använda:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```

Hej `{tenant}` kan göra något av fyra värden:

| Värde | Beskrivning |
| --- | --- |
| `common` |Användare med både ett personligt microsoftkonto och ett arbets- eller skolkonto konto från Azure Active Directory (Azure AD) kan logga in toohello program. |
| `organizations` |Endast användare med arbets- eller skolkonton från Azure AD kan logga in toohello program. |
| `consumers` |Endast användare med ett personligt microsoftkonto kan logga in toohello program. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` eller `contoso.onmicrosoft.com` |Endast användare med ett arbets- eller skolkonto konto från en specifik Azure AD-klient kan logga in toohello program. Hello eget domännamn hello Azure AD-klient eller hello klient-GUID-identifierare kan användas. |

hello metadata är ett enkelt JavaScript Object Notation (JSON)-dokument. Se hello följande kodavsnitt som ett exempel. hello fragments innehållet fullständigt beskrivs i hello [OpenID Connect specifikationen](https://openid.net).

```
{
  "authorization_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/authorize",
  "token_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt"
  ],
  "jwks_uri": "https:\/\/login.microsoftonline.com\/common\/discovery\/v2.0\/keys",

  ...

}
```

Normalt använder du skulle detta metadata dokumentet tooconfigure ett bibliotek med OpenID Connect eller SDK; hello biblioteket använder hello metadata toodo sitt arbete. Om du inte använder ett före build OpenID Connect bibliotek, kan du följa hello stegen i hello resten av den här artikeln tooperform inloggning i en webbapp med hjälp av hello v2.0-slutpunkten.

## Skicka förfrågan om hello-inloggning
När ditt webbprogram måste tooauthenticate hello användare, den kan dirigera hello användaren toohello `/authorize` slutpunkt. Den här begäran är liknande toohello första ben hello [OAuth 2.0-auktoriseringskodflödet](active-directory-v2-protocols-oauth-code.md), med dessa viktiga skillnader:

* hello begäran måste innehålla hello `openid` scope i hello `scope` parameter.
* Hej `response_type` parameter måste innehålla `id_token`.
* hello begäran måste innehålla hello `nonce` parameter.

Exempel:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=678910
```

> [!TIP]
> Klicka på följande länk tooexecute hello denna begäran. När du loggar in kommer att vara omdirigerade toohttps://localhost/myapp/ med en ID-token i hello adressfältet i webbläsaren. Observera att denna begäran använder `response_mode=query` (endast i demonstrationssyfte). Vi rekommenderar att du använder `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=query&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Parameter | Villkor | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Du kan använda hello `{tenant}` värdet hello sökvägen till hello begäran toocontrol som kan logga in toohello program. hello tillåtna värden är `common`, `organizations`, `consumers`, och identifierare för innehavare. Mer information finns i [protokollet grunderna](active-directory-v2-protocols.md#endpoints). |
| client_id |Krävs |hello program-ID som hello [Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tilldelade tooyour app. |
| response_type |Krävs |Måste innehålla `id_token` för OpenID Connect inloggning. Det kan även innehålla andra `response_types` värden, till exempel `code`. |
| redirect_uri |Rekommenderas |hello omdirigerings-URI för appen där autentisering svar kan skickas och tas emot av din app. Den måste matcha en hello omdirigerings-URI: er som du har registrerat i hello portal, förutom att det måste vara URL-kodade. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope. Det måste innehålla hello omfånget för OpenID Connect `openid`, som översätter toohello ”logga in dig på” behörighet i hello medgivande Användargränssnittet. Du kan även innehålla andra scope i den här förfrågan för att begära godkännande. |
| temporärt ID |Krävs |Ett värde som ingår i hello-begäran som skapats av hello-app som ska inkluderas i hello id_token resultatvärdet som ett anspråk. hello app kan kontrollera det här värdet toomitigate token replay-attacker. hello-värdet är vanligtvis en slumpmässig, unik sträng som kan använda tooidentify hello ursprung hello-begäran. |
| response_mode |Rekommenderas |Anger hello-metod som ska använda toosend hello resulterande auktorisering kod tillbaka tooyour app. Kan vara något av `query`, `form_post`, eller `fragment`. För webbprogram, bör du använda `response_mode=form_post`, tooensure hello mest säker överföring av token tooyour program. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som också kommer att returneras i hello token svar. Det kan vara en sträng med innehåll. Ett slumpmässigt genererat unikt värde som normalt används för[förhindra attacker med förfalskning av begäran](http://tools.ietf.org/html/rfc6749#section-10.12). hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade som hello sida eller vy hello användaren utfördes. |
| kommandotolk |Valfri |Anger hello användarinteraktion som krävs. hello endast giltiga värden just nu är `login`, `none`, och `consent`. Hej `prompt=login` anspråk som framtvingar hello användaren tooenter sina autentiseringsuppgifter på den begäran som negeras enkel inloggning. Hej `prompt=none` anspråk är hello motsatt. Detta anspråk säkerställer att hello inte användaren med den interaktiva prompten som helst. Om hello begäran inte kan slutföras utan meddelanden via enkel inloggning, returneras ett fel i hello v2.0-slutpunkten. Hej `prompt=consent` anspråk utlösare hello OAuth-medgivande dialogrutan när hello användaren loggar in. hello dialogrutan frågar hello användaren toogrant behörigheter toohello app. |
| login_hint |Valfri |Du kan använda den här parametern toopre fill hello användarnamn och e-adressfältet i hello-inloggningssida för hello användaren, om du vet hello användarnamn i förväg. Ofta appar att använda den här parametern under återautentiseringen efter redan extraherar hello användarnamn från en tidigare inloggning med hjälp av hello `preferred_username` anspråk. |
| domain_hint |Valfri |Det här värdet kan vara `consumers` eller `organizations`. Om ingår, hoppar över hello e-postbaserad identifieringsprocessen hello användaren går igenom på hello v2.0-inloggningssida, en något mer effektiv användarupplevelse. Ofta appar att använda den här parametern under återautentiseringen genom att extrahera hello `tid` anspråk från hello-ID-token. Om hello `tid` anspråk värdet är `9188040d-6c67-4c5b-b112-36a304b66dad`, Använd `domain_hint=consumers`. Annars använder `domain_hint=organizations`. |

Nu hello användaren är tooenter ange sina autentiseringsuppgifter och fullständig hello-autentisering. hello v2.0-slutpunkten verifierar att hello-användaren har godkänt toohello behörigheter anges i hello `scope` Frågeparametern. Om hello användaren inte har godkänt tooany dessa behörigheter, uppmanas hello v2.0-slutpunkten hello användarbehörigheter tooconsent toohello krävs. Du kan läsa mer om [behörigheter, medgivande och flera appar](active-directory-v2-scopes.md).

När hello användare autentiserar och ger ditt medgivande, hello v2.0-slutpunkten returnerar ett svar tooyour app på hello anges omdirigerings-URI med hjälp av hello-metod som anges i hello `response_mode` parameter.

### Lyckat svar
Ett lyckat svar när du använder `response_mode=form_post` ser ut så här:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beskrivning |
| --- | --- |
| id_token |Hej ID-token som hello app som begärdes. Du kan använda hello `id_token` parametern tooverify hello användarens identitet och starta en session med hello användare. Mer information om ID-token och deras innehåll finns i hello [v2.0-slutpunkten tokens referens](active-directory-v2-tokens.md). |
| state |Om en `state` parametern ingår i hello begäran hello samma värde som ska visas i hello svar. hello app bör kontrollera att hello tillstånd värden i hello förfrågan och svar är identiska. |

### Felsvar
Felsvar kan också skickas toohello omdirigerings-URI så att hello app kan hanteras. Ett felsvar ser ut så här:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som du kan använda tooclassify typer av fel som inträffar och tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa dig att identifiera hello grundorsaken till ett autentiseringsfel. |

### Felkoder för auktorisering endpoint fel
hello följande tabell beskrivs felkoder som kan returneras i hello `error` parametern för hello felsvar:

| Felkod | Beskrivning | Klientåtgärd |
| --- | --- | --- |
| invalid_request |Protokollfel, till exempel det saknas nödvändiga parametern. |Åtgärda och skicka hello begäran igen. Detta är en utvecklingsfel vanligtvis som standard under inledande testningen. |
| unauthorized_client |hello klientprogrammet kan inte begära en auktoriseringskod. |Detta inträffar vanligtvis när hello klientprogrammet inte har registrerats i Azure AD eller läggs inte toohello användarens Azure AD-klient. hello program kan fråga hello användaren med instruktioner tooinstall hello program och lägga till den tooAzure AD. |
| Om ACCESS_DENIED |nekad medgivande hello resurs-ägare. |hello klientprogrammet kan meddela hello användare som det går inte att fortsätta om hello användaren godkänner. |
| unsupported_response_type |hello auktorisering servern stöder inte hello svarstyp i hello-begäran. |Åtgärda och skicka hello begäran igen. Detta är en utvecklingsfel vanligtvis som standard under inledande testningen. |
| server_error |hello-servern påträffade ett oväntat fel. |Försök igen med hello-begäran. Dessa fel kan bero på tillfälliga förhållanden. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av tooa tillfälligt fel. |
| temporarily_unavailable |hello-server är tillfälligt överbelastad toohandle hello begäran. |Försök igen med hello-begäran. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av tooa tillfälligt tillstånd. |
| invalid_resource |hello målresursen är ogiltig eftersom den inte finns, Azure AD kan inte hitta den eller det är inte korrekt konfigurerad. |Detta anger att hello resursen om den finns inte har konfigurerats i hello-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |

## Validera hello-ID-token
Ta emot en token med ID: T är inte tillräcklig tooauthenticate hello användare. Du måste också verifiera signaturen för hello-ID-token och kontrollera hello anspråk i hello token per krav som din app. hello v2.0-slutpunkten använder [JSON Web token (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) och offentlig nyckel kryptografi toosign token och kontrollera att de är giltiga.

Du kan välja toovalidate hello-ID-token i klientkod men vanligt toosend hello ID token tooa backend-servern och gör det hello-verifiering. När du har validerat hello signaturen för hello-ID-token, måste du tooverify några anspråk. Mer information, inklusive information om [verifiera token](active-directory-v2-tokens.md#validating-tokens) och [viktig information om att signera nyckelförnyelse](active-directory-v2-tokens.md#validating-tokens), se hello [v2.0 tokens referens](active-directory-v2-tokens.md). Vi rekommenderar att du använder ett bibliotek tooparse och validera token. Det finns minst en av dessa bibliotek för de flesta språk och plattformar.
<!--TODO: Improve hello information on this-->

Du kan också toovalidate ytterligare anspråk, beroende på ditt scenario. Några vanliga verifieringar inkluderar:

* Se till att hello användaren eller organisationen har registrerat dig för hello app.
* Se till att användaren hello krävs behörighet eller auktorisering.
* Se till att en viss styrkan hos autentisering har inträffat, till exempel multifaktorautentisering.

Mer information om hello anspråk i en ID-token finns hello [v2.0-slutpunkten tokens referens](active-directory-v2-tokens.md).

När du har fullständigt validerat hello-ID-token, kan du börja en session med hello användare. Använd hello anspråk i hello ID token tooget information om hello användare i din app. Du kan använda den här informationen för att visa, poster, tillstånd och så vidare.

## Skicka en begäran om utloggning
Om du vill toosign ut hello användare från din app är inte tillräckligt tooclear appens cookies eller annars avsluta hello användarsession. Du måste också omdirigera hello användaren toohello v2.0-slutpunkten toosign ut. Om du inte gör det hello autentiserar igen användaren tooyour app utan att ange sina autentiseringsuppgifter igen, eftersom de har en giltig inloggning session med hello v2.0-slutpunkten.

Du kan omdirigera hello användaren toohello `end_session_endpoint` anges i hello OpenID Connect metadata dokument:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | Villkor | Beskrivning |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Rekommenderas | hello-URL som användaren hello är omdirigerade tooafter har logga ut. Om hello parametern inte är inkluderad visas hello användare ett allmänt meddelande som genereras av hello v2.0-slutpunkten. URL: en måste matcha en av hello omdirigerings-URI: er som registrerats för ditt program i portalen för registrering av hello-app.  |

## Enkel utloggning
När du omdirigerar hello användaren toohello `end_session_endpoint`, hello v2.0-slutpunkten rensar hello användarens session från hello webbläsare. Dock hello användaren kan fortfarande vara inloggad tooother program som använder Microsoft-konton för autentisering. tooenable dessa program toosign hello användaren ut samtidigt, hello v2.0-slutpunkten skickar en HTTP GET-begäran toohello registrerad `LogoutUrl` för alla program som hello hello användaren är inloggad till. Program måste svara toothis begäran genom att avmarkera alla sessioner som identifierar hello användar- och returnera ett `200` svar.  Om du vill ut toosupport för enkel inloggning i ditt program måste du implementera exempelvis en `LogoutUrl` i din programkod.  Du kan ange hello `LogoutUrl` från portalen för registrering av hello-app.

## Protokollet diagram: Token förvärv
Många webbappar behöver toonot endast logga hello användaren i men tooaccess en webbtjänst för hello användares räkning genom att använda OAuth. Det här scenariot kombinerar OpenID Connect för autentisering av användare vid hämtning av samtidigt tillstånd tokens kod som du kan använda tooget åtkomst om du använder hello OAuth-auktoriseringskodflödet.

Hej fullständig OpenID Connect-inloggning och token förvärv flödet ser ut ungefär toohello nästa diagram. Vi beskriver varje steg i detalj i hello nästa avsnitt i hello artikel.

![OpenID Connect protocol: Token förvärv](../../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

## Få åtkomst-token
tooacquire åtkomsttoken, ändra hello inloggning begäran:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'query', 'form_post', or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Klicka på följande länk tooexecute hello denna begäran. När du loggar in är omdirigerade toohttps://localhost/myapp/ med en ID-token och koden i hello adressfältet i webbläsaren. Observera att denna begäran använder `response_mode=query` (endast i demonstrationssyfte). Vi rekommenderar att du använder `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

Genom att inkludera behörighetsomfattningen i hello begäran och med hjälp av `response_type=id_token code`, hello v2.0-slutpunkten garanterar att hello-användaren har godkänt toohello behörigheter anges i hello `scope` Frågeparametern. Den returnerar ett auktorisering kod tooyour app tooexchange för en åtkomst-token.

### Lyckat svar
Ett lyckat svar från att använda `response_mode=form_post` ser ut så här:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beskrivning |
| --- | --- |
| id_token |Hej ID-token som hello app som begärdes. Du kan använda hello ID token tooverify hello användarens identitet och starta en session med hello användare. Du hittar mer information om ID-token och deras innehåll i hello [v2.0-slutpunkten tokens referens](active-directory-v2-tokens.md). |
| Koden |Hej auktoriseringskod hello appen begärdes. hello app kan använda hello auktorisering koden toorequest en åtkomst-token för hello målresursen. En Auktoriseringskoden är mycket tillfällig. Normalt en auktoriseringskod upphör att gälla inom ca 10 minuter. |
| state |Om en parametern state ingår i hello begäran ska hello samma värde visas i hello svar. hello app bör kontrollera att hello tillstånd värden i hello förfrågan och svar är identiska. |

### Felsvar
Felsvar kan också skickas toohello omdirigerings-URI så att hello app kan hantera dem på lämpligt sätt. Ett felsvar ser ut så här:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som du kan använda tooclassify typer av fel som inträffar och tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa dig att identifiera hello grundorsaken till ett autentiseringsfel. |

En beskrivning av felkoder och rekommenderade Klientsvar finns [felkoder för auktorisering endpoint fel](#error-codes-for-authorization-endpoint-errors).

När du har en Auktoriseringskoden och en ID-token kan du få åtkomst-token åt hello användaren loggar in. toosign hello användare i, måste du verifiera hello-ID-token [exakt så som det beskrivs](#validate-the-id-token). tooget åtkomsttoken, följ hello stegen som beskrivs i vår [dokumentationen för OAuth-protokollet](active-directory-v2-protocols-oauth-code.md#request-an-access-token).

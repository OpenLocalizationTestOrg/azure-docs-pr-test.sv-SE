---
title: "aaaAzure AD v2.0 OAuth Authorization kod flöda | Microsoft Docs"
description: "Skapa webbprogram med hjälp av Azure AD-implementeringen av hello OAuth 2.0-autentiseringsprotokollet."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: dee58f2f5c627fef35cae279349728c3c7bf9421
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# v2.0 protokoll - OAuth 2.0-Auktoriseringskodflödet
hello OAuth 2.0 auktorisering kod bevilja kan användas i appar som är installerade på en enhet toogain åtkomst tooprotected resurser, till exempel web API: er.  Hello app model v2.0 's implementering av OAuth 2.0 kan, du använda logga in och API åt tooyour bärbara och stationära appar.  Den här handboken är språkoberoende och beskriver hur toosend och ta emot HTTP-meddelanden utan att använda någon av våra bibliotek med öppen källkod.

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

hello OAuth 2.0-auktoriseringskodflödet som beskrivs i i [avsnittet 4.1 av hello OAuth 2.0-specifikationen](http://tools.ietf.org/html/rfc6749).  Det är används tooperform autentisering och auktorisering hello merparten av apptyper, inklusive [webbappar](active-directory-v2-flows.md#web-apps) och [internt installerade appar](active-directory-v2-flows.md#mobile-and-native-apps).  Det gör det möjligt för appar toosecurely hämta access_tokens som kan vara används tooaccess resurser som skyddas med hjälp av hello v2.0-slutpunkten.  

## Protokollet diagram
På en hög nivå hello hela autentiseringsflödet för en intern/mobile-programmet ser ut så här:

![Flödet för OAuth-Auth-kod](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## Begära ett auktoriseringskod
hello-auktoriseringskodflödet börjar med hello klienten dirigera hello användaren toohello `/authorize` slutpunkt.  I den här förfrågan anger hello-klienten hello behörigheter måste tooacquire från hello användare:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [!TIP]
> Klicka på hello länkarna tooexecute denna begäran! När du har registrerat webbläsaren omdirigeras för`https://localhost/myapp/` med en `code` i hello adressfältet.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Parameter |  | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Hej `{tenant}` värdet hello sökvägen till hello begäran kan vara används toocontrol som kan logga in på programmet hello.  hello tillåtna värden är `common`, `organizations`, `consumers`, och identifierare för innehavare.  Mer information finns i [protokollet grunderna](active-directory-v2-protocols.md#endpoints). |
| client_id |Krävs |hello program-Id som hello-portalen för registrering ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) tilldelats din app. |
| response_type |Krävs |Måste innehålla `code` för hello-auktoriseringskodflödet. |
| redirect_uri |Rekommenderas |Hej redirect_uri för din app, där autentisering svar kan skickas och tas emot av din app.  Den måste matcha en hello redirect_uris som du har registrerat i hello portal, förutom det måste vara url-kodade.  För intern & mobila appar, bör du använda hello standardvärdet `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| Omfång |Krävs |En blankstegsavgränsad lista över [scope](active-directory-v2-scopes.md) hello användaren tooconsent till önskade. |
| response_mode |Rekommenderas |Anger hello-metod som ska använda toosend hello resulterande token tillbaka tooyour app.  Kan vara `query` eller `form_post`. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som returneras också hello token svar.  Det kan vara en sträng med innehåll som du vill.  Ett slumpmässigt genererat unikt värde används vanligtvis för [förhindra attacker med förfalskning av begäran](http://tools.ietf.org/html/rfc6749#section-10.12).  hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller vy som de var på. |
| kommandotolk |Valfria |Anger hello användarinteraktion som krävs.  Hej endast giltiga värden för tillfället är ”inloggning” none, och 'godkännande'.  `prompt=login`Tvingad kommer hello användaren tooenter sina autentiseringsuppgifter på begäran, giltigt att negera enkel inloggning på.  `prompt=none`är hello motsatt - säkerställer att hello inte användaren med den interaktiva prompten som helst.  Om hello begäran inte kan slutföras utan meddelanden via enkel inloggning på returneras ett fel hello v2.0-slutpunkten.  `prompt=consent`utlösaren hello OAuth kommer medgivande dialogrutan efter hello användaren loggar in, ber hello användaren toogrant behörigheter toohello app. |
| login_hint |Valfria |Vara kan toopre Fyll hello användarnamn/e-postadress fält av hello logga in för hello användaren, om du känner till sitt lösenord i förväg.  Ofta appar kommer att använda den här parametern under återautentiseringen redan har extraherats hello användarnamn från en tidigare inloggning med hello `preferred_username` anspråk. |
| domain_hint |Valfria |Kan vara något av `consumers` eller `organizations`.  Om den hoppar över hello e-postbaserad identifieringsprocessen användaren går igenom på hello v2.0 inloggningssidan, inledande tooa något mer effektiv användarupplevelse.  Ofta appar kommer att använda den här parametern under återautentiseringen genom att extrahera hello `tid` från en tidigare inloggning.  Om hello `tid` anspråk värdet är `9188040d-6c67-4c5b-b112-36a304b66dad`, bör du använda `domain_hint=consumers`.  Annars använder `domain_hint=organizations`. |

Nu hello användaren kommer att tillfrågas tooenter sina autentiseringsuppgifter och fullständig hello-autentisering.  hello v2.0-slutpunkten innebär också att hello-användaren har godkänt toohello behörigheter anges i hello `scope` Frågeparametern.  Om hello användaren inte har godkänt tooany dessa behörigheter, uppmanas hello användaren tooconsent toohello krävs behörigheter.  Information om [behörigheter, medgivande och flera innehavare appar finns här](active-directory-v2-scopes.md).

När hello användare autentiserar och ger ditt medgivande, hello v2.0-slutpunkten returnerar ett svar tooyour app på hello anges `redirect_uri`, med hjälp av hello-metod som anges i hello `response_mode` parameter.

#### Lyckat svar
Ett lyckat svar med `response_mode=query` ser ut som:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parameter | Beskrivning |
| --- | --- |
| Koden |Hej authorization_code som hello app som begärdes. hello app kan använda hello auktorisering koden toorequest en åtkomst-token för hello målresursen.  Authorization_codes är mycket kort livslängd, vanligtvis de går ut efter 10 minuter. |
| state |Om en parametern state ingår i hello begäran ska hello samma värde visas i hello svar. hello app bör kontrollera att hello tillstånd värden i hello förfrågan och svar är identiska. |

#### Felsvar
Felsvar kan också skickas toohello `redirect_uri` så hello app kan hantera dem på rätt sätt:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |

#### Felkoder för auktorisering endpoint fel
hello följande tabell beskrivs hello olika felkoder som kan returneras i hello `error` parametern för hello felsvar.

| Felkod | Beskrivning | Klientåtgärd |
| --- | --- | --- |
| invalid_request |Protokollfel, till exempel en obligatorisk parameter saknas. |Åtgärda och skicka hello begäran igen. Det här är en utveckling fel vanligtvis fångas upp under inledande testningen. |
| unauthorized_client |hello klientprogrammet är inte tillåtna toorequest en Auktoriseringskoden. |Detta inträffar vanligtvis när hello klientprogrammet inte har registrerats i Azure AD eller läggs inte toohello användarens Azure AD-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |
| Om ACCESS_DENIED |Resursägare nekad medgivande |hello klientprogrammet kan meddela hello användare som det går inte att fortsätta om hello användaren godkänner. |
| unsupported_response_type |hello auktorisering servern stöder inte hello svarstyp i hello-begäran. |Åtgärda och skicka hello begäran igen. Det här är en utveckling fel vanligtvis fångas upp under inledande testningen. |
| server_error |hello-servern påträffade ett oväntat fel. |Försök igen med hello-begäran. Dessa fel kan bero på tillfälliga förhållanden. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av ett tillfälligt fel. |
| temporarily_unavailable |hello-server är tillfälligt överbelastad toohandle hello begäran. |Försök igen med hello-begäran. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av ett tillfälligt tillstånd. |
| invalid_resource |hello målresursen är ogiltig eftersom den inte finns, Azure AD kan inte hitta den eller det är inte korrekt konfigurerad. |Detta anger hello resursen om den finns inte har konfigurerats i hello-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |

## Begär en åtkomst-token
Nu när du har skaffat ett authorization_code och har beviljats behörighet av hello användare kan du lösa in hello `code` för en `access_token` toohello önskad resurs, genom att skicka en `POST` begära toohello `/token` slutpunkt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Försök köra begäran i Postman! (Glöm inte tooreplace hello `code`) [ ![körs i Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parameter |  | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Hej `{tenant}` värdet hello sökvägen till hello begäran kan vara används toocontrol som kan logga in på programmet hello.  hello tillåtna värden är `common`, `organizations`, `consumers`, och identifierare för innehavare.  Mer information finns i [protokollet grunderna](active-directory-v2-protocols.md#endpoints). |
| client_id |Krävs |hello program-Id som hello-portalen för registrering ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) tilldelats din app. |
| grant_type |Krävs |Måste vara `authorization_code` för hello-auktoriseringskodflödet. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope.  hello scope begärt i denna del måste motsvarande tooor en delmängd av hello scope begäras i hello första del.  Om hello scope som anges i denna begäran sträcker sig över flera resursservrar, returnera en token för hello-resurs som angetts i hello första omfång hello v2.0-slutpunkten.  En mer detaljerad förklaring av scope finns för[behörigheter, medgivande och scope](active-directory-v2-scopes.md). |
| Koden |Krävs |Hej authorization_code som du har införskaffade i hello första del av hello flödet. |
| redirect_uri |Krävs |Hej samma redirect_uri-värde som har använt tooacquire hello authorization_code. |
| client_secret |krävs för webbprogram |Hej programhemlighet som du skapade i portalen för registrering av hello-app för din app.  Den bör inte användas i en intern app eftersom client_secrets inte kan lagras på ett tillförlitligt sätt på enheter.  Det krävs för webbappar och webb-API: er som har hello möjlighet toostore hello client_secret på ett säkert sätt på hello på serversidan. |

#### Lyckat svar
Ett lyckat token svar ser ut:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beskrivning |
| --- | --- |
| access_token |hello begärda åtkomst-token. hello app kan använda den här token tooauthenticate toohello skyddad resurs, till exempel ett webb-API. |
| token_type |Anger hello tokentypen värdet. hello är typ som har stöd för Azure AD ägar |
| expires_in |Hur länge hello åtkomst-token är giltig (i sekunder). |
| Omfång |hello scope som hello access_token är giltig för. |
| refresh_token |En token för uppdatering av OAuth 2.0. hello app kan använda denna token få ytterligare åtkomsttoken när hello aktuella åtkomst-token upphör att gälla.  Refresh_tokens är långlivade och kan vara används tooretain åtkomst tooresources för längre tid.  Mer information finns i toohello [v2.0 tokenreferens](active-directory-v2-tokens.md). |
| id_token |En osignerad JSON-Webbtoken (JWT). hello app kan base64Url avkoda hello segmenten i den här token toorequest information om hello-användare som är inloggad. hello app kan cachelagra hello värden och visa dem, men det bör inte förlita dig på dem för auktorisering eller säkerhetsgränser.  Mer information om id_tokens finns hello [v2.0-slutpunkten tokenreferens](active-directory-v2-tokens.md). |

#### Felsvar
Felsvar ser ut:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: hello provided value for hello input parameter 'scope' is not valid. hello scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |
| error_codes |En lista över STS-specifika felkoder som kan hjälpa dig i diagnostik. |
| tidsstämpel |hello tid då hello-fel inträffade. |
| trace_id |En unik identifierare för hello-begäran som kan hjälpa dig i diagnostik. |
| correlation_id |En unik identifierare för hello-begäran som kan hjälpa dig i diagnostik för komponenter. |

#### Felkoder för token för slutpunkt-fel
| Felkod | Beskrivning | Klientåtgärd |
| --- | --- | --- |
| invalid_request |Protokollfel, till exempel en obligatorisk parameter saknas. |Åtgärda och skicka hello begäran igen |
| invalid_grant |hello auktoriseringskod är ogiltigt eller har upphört att gälla. |Försök med en ny begäran toohello `/authorize` slutpunkt |
| unauthorized_client |hello autentiserade klienten har inte behörighet toouse detta tillstånd bevilja. |Detta inträffar vanligtvis när hello klientprogrammet inte har registrerats i Azure AD eller läggs inte toohello användarens Azure AD-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |
| invalid_client |Klientautentisering misslyckades. |hello klientens autentiseringsuppgifter är inte giltiga. toofix, hello programadministratör uppdaterar hello autentiseringsuppgifter. |
| unsupported_grant_type |hello auktorisering servern stöder inte hello authorization grant typen. |Ändra hello bevilja i hello-begäran. Den här typen av fel bör sker enbart under utveckling och identifieras under inledande testningen. |
| invalid_resource |hello målresursen är ogiltig eftersom den inte finns, Azure AD kan inte hitta den eller det är inte korrekt konfigurerad. |Detta anger hello resursen om den finns inte har konfigurerats i hello-klient. hello program kan ange hello användare med instruktioner för att installera programmet hello och lägger till den tooAzure AD. |
| interaction_required |hello begäran kräver interaktion från användaren. Till exempel krävs ett steg i ytterligare autentisering. |Försök igen med begäran hello med hello samma resurs. |
| temporarily_unavailable |hello-server är tillfälligt överbelastad toohandle hello begäran. |Försök igen med hello-begäran. hello klientprogrammet kan förklara toohello användaren att svaret är försenad på grund av ett tillfälligt tillstånd. |

## Använd hello åtkomst-token
Nu när du har skaffat har en `access_token`, du kan använda hello token i begäranden tooWeb API: er genom att inkludera i hello `Authorization` huvud:

> [!TIP]
> Kör denna begäran i Postman! (Ersätt hello `Authorization` huvud första) [ ![körs i Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## Uppdatera hello åtkomst-token
Access_tokens är kort livslängd och du måste uppdatera dem när de upphör att gälla toocontinue åtkomst till resurser.  Du kan göra det genom att skicka in en annan `POST` begära toohello `/token` slutpunkt, nu att tillhandahålla hello `refresh_token` i stället för hello `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Försök köra begäran i Postman! (Glöm inte tooreplace hello `refresh_token`) [ ![körs i Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parameter |  | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Hej `{tenant}` värdet hello sökvägen till hello begäran kan vara används toocontrol som kan logga in på programmet hello.  hello tillåtna värden är `common`, `organizations`, `consumers`, och identifierare för innehavare.  Mer information finns i [protokollet grunderna](active-directory-v2-protocols.md#endpoints). |
| client_id |Krävs |hello program-Id som hello-portalen för registrering ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) tilldelats din app. |
| grant_type |Krävs |Måste vara `refresh_token` för denna del av hello-auktoriseringskodflödet. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope.  hello scope begärt i denna del måste motsvarande tooor en delmängd av hello scope begäras i hello ursprungliga authorization_code begäran ben.  Om hello scope som anges i denna begäran sträcker sig över flera resursservrar, returnera en token för hello-resurs som angetts i hello första omfång hello v2.0-slutpunkten.  En mer detaljerad förklaring av scope finns för[behörigheter, medgivande och scope](active-directory-v2-scopes.md). |
| refresh_token |Krävs |Hej refresh_token som du har införskaffade i hello andra ben hello flödet. |
| redirect_uri |Krävs |Hej samma redirect_uri-värde som har använt tooacquire hello authorization_code. |
| client_secret |krävs för webbprogram |Hej programhemlighet som du skapade i portalen för registrering av hello-app för din app.  Den bör inte användas i en intern app eftersom client_secrets inte kan lagras på ett tillförlitligt sätt på enheter.  Det krävs för webbappar och webb-API: er som har hello möjlighet toostore hello client_secret på ett säkert sätt på hello på serversidan. |

#### Lyckat svar
Ett lyckat token svar ser ut:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beskrivning |
| --- | --- |
| access_token |hello begärda åtkomst-token. hello app kan använda den här token tooauthenticate toohello skyddad resurs, till exempel ett webb-API. |
| token_type |Anger hello tokentypen värdet. hello är typ som har stöd för Azure AD ägar |
| expires_in |Hur länge hello åtkomst-token är giltig (i sekunder). |
| Omfång |hello scope som hello access_token är giltig för. |
| refresh_token |En ny OAuth 2.0-uppdateringstoken. Du ska ersätta hello gamla uppdatera token med den här nya anskaffats uppdatera token tooensure uppdaterings-tokens vara giltigt så länge som möjligt. |
| id_token |En osignerad JSON-Webbtoken (JWT). hello app kan base64Url avkoda hello segmenten i den här token toorequest information om hello-användare som är inloggad. hello app kan cachelagra hello värden och visa dem, men det bör inte förlita dig på dem för auktorisering eller säkerhetsgränser.  Mer information om id_tokens finns hello [v2.0-slutpunkten tokenreferens](active-directory-v2-tokens.md). |

#### Felsvar
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: hello provided value for hello input parameter 'scope' is not valid. hello scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |
| error_codes |En lista över STS-specifika felkoder som kan hjälpa dig i diagnostik. |
| tidsstämpel |hello tid då hello-fel inträffade. |
| trace_id |En unik identifierare för hello-begäran som kan hjälpa dig i diagnostik. |
| correlation_id |En unik identifierare för hello-begäran som kan hjälpa dig i diagnostik för komponenter. |

En beskrivning av hello felkoder och hello rekommenderad klientåtgärd finns [felkoder för token för slutpunkt fel](#error-codes-for-token-endpoint-errors).


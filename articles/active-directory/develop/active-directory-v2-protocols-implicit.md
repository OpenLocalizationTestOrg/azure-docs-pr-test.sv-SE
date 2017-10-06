---
title: "aaaSecure sida program med hjälp av hello Azure AD v2.0 implicita flödet | Microsoft Docs"
description: "Skapa webbprogram med hjälp av Azure AD v2.0 implementering av hello implicita flödet för appar med enstaka sidor."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 2cdce4eee88be4af54966d15204b79fa4992a58e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# v2.0 protokoll - SPA med hello implicita flödet
Med hello v2.0-slutpunkten du loggar in användare i din ensidesappar med både personliga och arbete/skola konton från Microsoft.  Sida och andra JavaScript-appar som kör främst i en webbläsare yta några intressanta utmaningar när det kommer tooauthentication:

* hello säkerhetsegenskaper för dessa appar är skiljer sig från traditionella serverbaserad webbprogram.
* Många servrar för auktorisering och identitetsleverantörer har inte stöd för CORS-begäranden.
* Helsida webbläsaren omdirigeras från hello appen blir särskilt inkräktande toohello användarupplevelsen.

För dessa program (tror: AngularJS, Ember.js, React.js, osv) Azure AD stöder hello OAuth 2.0 Implicit Grant flödet.  hello implicita flödet beskrivs i hello [OAuth 2.0-specifikationen](http://tools.ietf.org/html/rfc6749#section-4.2).  Den största fördelen är att hello apptoken tooget från Azure AD utan att utföra en backend-servern autentiseringsuppgifter exchange.  Detta gör att hello app toosign i hello användare, hålla sessionen aktiv och hämta token tooother web API: er inuti hello klientens JavaScript-kod.  Finns några viktiga säkerhet överväganden tootake beaktas när du använder implicita flödet för hello - specifikt runt [klienten](http://tools.ietf.org/html/rfc6749#section-10.3) och [personifiering](http://tools.ietf.org/html/rfc6749#section-10.3).

Om du vill toouse hello implicita flödet och Azure AD tooadd autentisering tooyour JavaScript-app, rekommenderar vi du använder våra öppen källkod JavaScript-bibliotek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Det finns några AngularJS självstudier [här](active-directory-appmodel-v2-overview.md#getting-started) toohelp dig att komma igång.  

Om du vill använda inte toouse ett bibliotek i sida app och skicka protokollmeddelanden själv, följa hello allmänna stegen nedan.

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

## Protokollet diagram
hello hela implicit inloggning flödet ser ut ungefär så här: var och en av hello stegen beskrivs i detalj nedan.

![OpenId Connect spaltformat som illustrerar](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## Skicka förfrågan om hello-inloggning
tooinitially logga hello användare i din app, kan du skicka en [OpenID Connect](active-directory-v2-protocols-oidc.md) auktoriseringsbegäran och få en `id_token` från hello v2.0-slutpunkten:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Klicka på hello länkarna tooexecute denna begäran! När du har registrerat webbläsaren omdirigeras för`https://localhost/myapp/` med en `id_token` i hello adressfältet.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Parameter |  | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Hej `{tenant}` värdet hello sökvägen till hello begäran kan vara används toocontrol som kan logga in på programmet hello.  hello tillåtna värden är `common`, `organizations`, `consumers`, och identifierare för innehavare.  Mer information finns i [protokollet grunderna](active-directory-v2-protocols.md#endpoints). |
| client_id |Krävs |hello program-Id som hello-portalen för registrering ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) tilldelats din app. |
| response_type |Krävs |Måste innehålla `id_token` för OpenID Connect inloggning.  Det kan också innehålla hello response_type `token`. Med hjälp av `token` här gör att din app tooreceive en åtkomst-token direkt från hello godkänna endpoint utan toomake auktorisera en andra begäran toohello slutpunkt.  Om du använder hello `token` response_type, hello `scope` parameter måste innehålla ett scope som anger vilken resurs tooissue hello-token för. |
| redirect_uri |Rekommenderas |Hej redirect_uri för din app, där autentisering svar kan skickas och tas emot av din app.  Den måste matcha en hello redirect_uris som du har registrerat i hello portal, förutom det måste vara url-kodade. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope.  Det måste innehålla hello omfånget för OpenID Connect `openid`, som översätter toohello ”logga in dig på” behörighet i hello medgivande Användargränssnittet.  Om du vill du kanske också vill tooinclude hello `email` eller `profile` [scope](active-directory-v2-scopes.md) för att få åtkomst till tooadditional användardata.  Du kan också omfatta andra scope i den här förfrågan för att begära godkännande toovarious resurser. |
| response_mode |Rekommenderas |Anger hello-metod som ska använda toosend hello resulterande token tillbaka tooyour app.  Bör vara `fragment` för hello implicita flödet. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som returneras också hello token svar.  Det kan vara en sträng med innehåll som du vill.  Ett slumpmässigt genererat unikt värde används vanligtvis för [förhindra attacker med förfalskning av begäran](http://tools.ietf.org/html/rfc6749#section-10.12).  hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller vy som de var på. |
| temporärt ID |Krävs |Ett värde som ingår i hello-begäran som skapats av hello-app som ska inkluderas i hello resulterande id_token som ett anspråk.  hello app kan kontrollera det här värdet toomitigate token replay-attacker.  hello-värdet är vanligtvis en slumpmässig, unik sträng som kan använda tooidentify hello ursprung hello-begäran. |
| kommandotolk |Valfria |Anger hello användarinteraktion som krävs.  Hej endast giltiga värden för tillfället är ”inloggning” none, och 'godkännande'.  `prompt=login`Tvingad kommer hello användaren tooenter sina autentiseringsuppgifter på begäran, giltigt att negera enkel inloggning på.  `prompt=none`är hello motsatt - säkerställer att hello inte användaren med den interaktiva prompten som helst.  Om hello begäran inte kan slutföras utan meddelanden via enkel inloggning på returneras ett fel hello v2.0-slutpunkten.  `prompt=consent`utlösaren hello OAuth kommer medgivande dialogrutan efter hello användaren loggar in, ber hello användaren toogrant behörigheter toohello app. |
| login_hint |Valfria |Vara kan toopre Fyll hello användarnamn/e-postadress fält av hello logga in för hello användaren, om du känner till sitt lösenord i förväg.  Ofta appar kommer att använda den här parametern under återautentiseringen redan har extraherats hello användarnamn från en tidigare inloggning med hello `preferred_username` anspråk. |
| domain_hint |Valfria |Kan vara något av `consumers` eller `organizations`.  Om den hoppar över hello e-postbaserad identifieringsprocessen användaren går igenom på hello v2.0 inloggningssidan, inledande tooa något mer effektiv användarupplevelse.  Ofta appar kommer att använda den här parametern under återautentiseringen genom att extrahera hello `tid` anspråk från hello id_token.  Om hello `tid` anspråk värdet är `9188040d-6c67-4c5b-b112-36a304b66dad`, bör du använda `domain_hint=consumers`.  Annars använder `domain_hint=organizations`. |

Nu hello användaren kommer att tillfrågas tooenter sina autentiseringsuppgifter och fullständig hello-autentisering.  hello v2.0-slutpunkten innebär också att hello-användaren har godkänt toohello behörigheter anges i hello `scope` Frågeparametern.  Om hello användaren inte har godkänt tooany dessa behörigheter, uppmanas hello användaren tooconsent toohello krävs behörigheter.  Information om [behörigheter, medgivande och flera innehavare appar finns här](active-directory-v2-scopes.md).

När hello användare autentiserar och ger ditt medgivande, hello v2.0-slutpunkten returnerar ett svar tooyour app på hello anges `redirect_uri`, med hjälp av hello-metod som anges i hello `response_mode` parameter.

#### Lyckat svar
Ett lyckat svar med `response_mode=fragment` och `response_type=id_token+token` verkar vara hello följande med radbrytningar för läsbarhet:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parameter | Beskrivning |
| --- | --- |
| access_token |Inkluderade om `response_type` innehåller `token`. hello åtkomst-token som hello app krävs i det här fallet för hello Microsoft Graph.  hello åtkomst-token ska inte avkodas eller annars kontrolleras, den kan hanteras som en täckande sträng. |
| token_type |Inkluderade om `response_type` innehåller `token`.  Kommer alltid att `Bearer`. |
| expires_in |Inkluderade om `response_type` innehåller `token`.  Anger hello antal sekunder hello-token är giltig för cachelagring. |
| Omfång |Inkluderade om `response_type` innehåller `token`.  Anger hello område(n) vilka hello access_token ska gälla. |
| id_token |Hej id_token som hello app som begärdes. Du kan använda hello id_token tooverify hello användarens identitet och starta en session med hello användare.  Mer information om id_tokens och deras innehåll ingår i hello [v2.0-slutpunkten tokenreferens](active-directory-v2-tokens.md). |
| state |Om en parametern state ingår i hello begäran ska hello samma värde visas i hello svar. hello app bör kontrollera att hello tillstånd värden i hello förfrågan och svar är identiska. |

#### Felsvar
Felsvar kan också skickas toohello `redirect_uri` så hello app kan hantera dem på rätt sätt:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |

## Validera hello id_token
Bara tar emot en id_token är inte tillräcklig tooauthenticate hello användaren. Du måste verifiera hello id_token signatur och kontrollera hello anspråk i hello token per krav som din app.  hello v2.0-slutpunkten använder [JSON Web token (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) och offentlig nyckel kryptografi toosign token och kontrollera att de är giltiga.

Du kan välja toovalidate hello `id_token` i klientkod, men vanligt är toosend hello `id_token` tooa backend-servern och gör det hello-verifiering.  När du har validerat hello signaturen för hello id_token, finns det några anspråk, kommer du att nödvändiga tooverify.  Finns hello [v2.0 tokenreferens](active-directory-v2-tokens.md) mer information, inklusive [verifierar token](active-directory-v2-tokens.md#validating-tokens) och [viktig Information om signering nyckeln förnyelse](active-directory-v2-tokens.md#validating-tokens).  Vi rekommenderar utnyttjar ett bibliotek av parsning och validera token - det finns minst en tillgänglig för de flesta språk och plattformar.
<!--TODO: Improve hello information on this-->

Du kan också toovalidate ytterligare anspråk beroende på ditt scenario.  Några vanliga verifieringar inkluderar:

* Säkerställa hello användarorganisation har registrerat dig för hello app.
* Säkerställa hello användaren har rätt tillstånd/privilegier
* Säkerställa en vissa styrkan hos autentisering har inträffat, till exempel multifaktorautentisering.

Mer information om hello anspråk i en id_token finns hello [v2.0-slutpunkten tokenreferens](active-directory-v2-tokens.md).

När du har fullständigt validerat hello id_token, kan du starta en session med hello användare och använda hello anspråk i hello id_token tooobtain information om hello användare i din app.  Den här informationen kan användas för att visa, poster, tillstånd och så vidare.

## Få åtkomst-token
Nu när du har registrerat hello användare i din app för enstaka sida, kan du få åtkomst-token för anropande webb-API: er som skyddas av Azure AD, till exempel hello [Microsoft Graph](https://graph.microsoft.io).  Även om du redan har tagit emot en token med hello `token` response_type, du kan använda den här metoden tooacquire token tooadditional resurser utan att behöva tooredirect hello användaren toosign igen.

I hello normal OpenID Connect/OAuth-flöde, ska du göra detta genom att göra en förfrågan toohello v2.0 `/token` slutpunkt.  Hello v2.0-slutpunkten har dock inte stöd för CORS-begäranden, så gör AJAX-anrop tooget och uppdateringstoken som ligger utanför hello fråga.  Du kan i stället använda hello implicita flödet i en dold iframe tooget nya token för andra webb-API: er: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [!TIP]
> Försök att kopiera och klistra in hello nedan begäran i en webbläsarflik! (Glöm inte tooreplace hello `domain_hint` och hello `login_hint` värden med hello korrigera värden för dina användare)
> 
> 

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parameter |  | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Hej `{tenant}` värdet hello sökvägen till hello begäran kan vara används toocontrol som kan logga in på programmet hello.  hello tillåtna värden är `common`, `organizations`, `consumers`, och identifierare för innehavare.  Mer information finns i [protokollet grunderna](active-directory-v2-protocols.md#endpoints). |
| client_id |Krävs |hello program-Id som hello-portalen för registrering ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) tilldelats din app. |
| response_type |Krävs |Måste innehålla `id_token` för OpenID Connect inloggning.  Den kan också omfatta andra response_types som `code`. |
| redirect_uri |Rekommenderas |Hej redirect_uri för din app, där autentisering svar kan skickas och tas emot av din app.  Den måste matcha en hello redirect_uris som du har registrerat i hello portal, förutom det måste vara url-kodade. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope.  För att hämta token omfattar alla [scope](active-directory-v2-scopes.md) du behöver för hello resurs av intresse. |
| response_mode |Rekommenderas |Anger hello-metod som ska använda toosend hello resulterande token tillbaka tooyour app.  Kan vara något av `query`, `form_post`, eller `fragment`. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som returneras också hello token svar.  Det kan vara en sträng med innehåll som du vill.  Ett slumpmässigt genererat unikt värde används vanligtvis för att förhindra attacker med förfalskning av begäran.  hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sida eller vy som de var på. |
| temporärt ID |Krävs |Ett värde som ingår i hello-begäran som skapats av hello-app som ska inkluderas i hello resulterande id_token som ett anspråk.  hello app kan kontrollera det här värdet toomitigate token replay-attacker.  hello-värdet är vanligtvis en slumpmässig, unik sträng som kan använda tooidentify hello ursprung hello-begäran. |
| kommandotolk |Krävs |Du bör använda för att uppdatera och hämta token i en dold iframe `prompt=none` tooensure som hello iframe inte låser sig på hello v2.0 inloggningssidan och returnerar omedelbart. |
| login_hint |Krävs |För att uppdatera och hämta token i en dold iframe måste du inkludera hello användarnamnet för användaren hello i det här tipset i ordning toodistinguish mellan flera sessioner hello användaren kan ha vid en viss tidpunkt. Du kan extrahera hello användarnamn från en tidigare inloggning med hello `preferred_username` anspråk. |
| domain_hint |Krävs |Kan vara något av `consumers` eller `organizations`.  Uppdatera och hämta token i en dold iframe måste du inkludera hello domain_hint i hello-begäran.  Du bör extrahera hello `tid` anspråk från hello id_token av föregående toodetermine inloggning som värdet toouse.  Om hello `tid` anspråk värdet är `9188040d-6c67-4c5b-b112-36a304b66dad`, bör du använda `domain_hint=consumers`.  Annars använder `domain_hint=organizations`. |

Tack toohello `prompt=none` parameter för denna begäran antingen lyckas eller misslyckas omedelbart och returnera tooyour program.  Ett lyckat svar skickas tooyour app på hello anges `redirect_uri`, med hjälp av hello-metod som anges i hello `response_mode` parameter.

#### Lyckat svar
Ett lyckat svar med `response_mode=fragment` ser ut som:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parameter | Beskrivning |
| --- | --- |
| access_token |hello-token som hello app som begärdes. |
| token_type |Kommer alltid att `Bearer`. |
| state |Om en parametern state ingår i hello begäran ska hello samma värde visas i hello svar. hello app bör kontrollera att hello tillstånd värden i hello förfrågan och svar är identiska. |
| expires_in |Hur länge hello åtkomst-token är giltig (i sekunder). |
| Omfång |hello scope som hello åtkomst-token är giltig för. |

#### Felsvar
Felsvar kan också skickas toohello `redirect_uri` så hello app kan hantera dem på lämpligt sätt.  Hello gäller `prompt=none`, är ett förväntat fel:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som kan använda tooclassify typer av fel som inträffar och kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |

Om du får detta felmeddelande i hello iframe begäran måste hello användaren interaktivt inloggningen igen tooretrieve en ny token.  Du kan välja toohandle det här fallet i sättet som passar bäst för ditt program.

## Uppdatera token
Båda `id_token`s och `access_token`s upphör att gälla efter en kort tidsperiod, så att din app måste du förbereda toorefresh dessa säkerhetstoken med jämna mellanrum.  toorefresh skriver du antingen av token, som du kan utföra hello samma dolda iframe begäran ovan med hello `prompt=none` parametern toocontrol Azure AD-beteende.  Om du vill tooreceive en ny `id_token`, vara säker på att toouse `response_type=id_token` och `scope=openid`, samt en `nonce` parameter.

## Skicka en logga ut begäran
Hej OpenIdConnect `end_session_endpoint` kan din app toosend en begäran toohello v2.0-slutpunkten tooend en användares session och rensa cookies som hello v2.0-slutpunkten.  toofully loggar en användare utanför ett webbprogram, appen bör avsluta en egen session med hello användare (vanligtvis genom att avmarkera en token-cache eller släppa cookies) och sedan dirigera hello webbläsaren:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parameter |  | Beskrivning |
| --- | --- | --- |
| Klient |Krävs |Hej `{tenant}` värdet hello sökvägen till hello begäran kan vara används toocontrol som kan logga in på programmet hello.  hello tillåtna värden är `common`, `organizations`, `consumers`, och identifierare för innehavare.  Mer information finns i [protokollet grunderna](active-directory-v2-protocols.md#endpoints). |
| post_logout_redirect_uri | Rekommenderas | hello-URL som hello användaren ska returneras tooafter logga ut har slutförts. Det här värdet måste matcha en av hello omdirigerings-URI: er som registrerats för hello program. Om inte ingår, visas hello användare ett allmänt meddelande av hello v2.0-slutpunkten. |

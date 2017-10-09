---
title: "Azure Active Directory B2C: Single-page appar med implicita flödet | Microsoft Docs"
description: "Lär dig hur toobuild sida appar direkt med hjälp av OAuth 2.0 implicit flöda med Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: a45cc74c-a37e-453f-b08b-af75855e0792
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: parakhj
ms.openlocfilehash: 5e52abc18fe545f0f6d1745cdb2ea42c5b2698cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-single-page-app-sign-in-by-using-oauth-20-implicit-flow"></a>Azure AD B2C: Single-page app logga in med hjälp av implicita flödet för OAuth 2.0

> [!NOTE]
> Den här funktionen är i förhandsgranskningen.
> 

Många moderna appar innehåller en enda sida app-klientdel som främst är skriven i JavaScript. Hello app skrivs ofta, med hjälp av ett ramverk som AngularJS, Ember.js eller Durandal. En sida appar och andra JavaScript-appar som körs i första hand i en webbläsare har några ytterligare utmaningar för autentisering:

* hello säkerhetsegenskaper för dessa appar är skiljer sig från traditionella serverbaserade webbprogram.
* Många servrar för auktorisering och identitetsleverantörer stöder inte resursdelning för korsande ursprung (CORS)-begäranden.
* Hela sidan webbläsaren omdirigeras från hello app kan vara betydligt inkräktande toohello användarupplevelsen.

toosupport programmen, Azure Active Directory B2C (Azure AD B2C) använder hello OAuth 2.0 implicita flödet. hello OAuth 2.0 auktorisering implicit bevilja flödet beskrivs i [avsnittet 4.2 av hello OAuth 2.0-specifikationen](http://tools.ietf.org/html/rfc6749). I implicita flödet hello appen tar emot token direkt från hello Azure Active Directory (AD Azure) godkänna endpoint utan någon exchange servrar. Alla autentiseringslogiken och session hantering tar placera helt i hello JavaScript-klienten utan ytterligare sidomdirigeringar.

Azure AD B2C utökar hello standard OAuth 2.0 implicita flödet toomore än enkel autentisering och auktorisering. Azure AD B2C introducerar hello [parametern](active-directory-b2c-reference-policies.md). Med parametern hello, kan du använda OAuth 2.0 tooadd användaren upplever tooyour app som registrering, inloggning och profilhantering. I den här artikeln har visar vi hur toouse hello implicita flödet och Azure AD tooimplement varje dessa upplevelser i dina program på en sida. toohelp dig att komma igång, ta en titt på vårt [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi) och [Microsoft .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi) prover.

I hello exempel HTTP-begäranden i den här artikeln, vi använda vårt exempel Azure AD B2C-katalog, **fabrikamb2c.onmicrosoft.com**. Vi använder också våra egna exempelprogrammet och principer. Du kan också försöka hello begäranden med hjälp av dessa värden eller kan du ersätta dem med egna värden.
Lär dig hur för[hämta egna Azure AD B2C-katalog, program och principer](#use-your-own-b2c-tenant).


## <a name="protocol-diagram"></a>Protokollet diagram

hello implicita inloggning flödet ut ungefär så hello följande bild. Varje steg beskrivs i detalj i hello artikel.

![OpenID Connect spaltformat som illustrerar](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Skicka autentiseringsbegäranden
När ditt webbprogram måste tooauthenticate hello användare och köra en princip, det vidarebefordrar hello användaren toohello `/authorize` slutpunkt. Detta är hello interaktiva delen av hello flödet där hello användaren vidtar åtgärder beroende på hello princip. hello användaren får ett ID-token från hello Azure AD-slutpunkten.

I den här förfrågan hello-klienten anger i hello `scope` parametern hello behörigheter att den behöver tooacquire från hello användare. I hello `p` parametern indikerar det hello princip tooexecute. hello använda följande tre exempel (med radbrytningar för att läsa) varje en annan princip. tooget en känsla för hur fungerar varje begäran, försök klistra in hello i en webbläsare och körs.

### <a name="use-a-sign-in-policy"></a>Använda en princip för inloggning
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>Använda en princip för registrering
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Använda en princip för Redigera-profil
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Krävs? | Beskrivning |
| --- | --- | --- |
| client_id |Krävs |hello program-ID tilldelade tooyour app i hello [Azure-portalen](https://portal.azure.com). |
| response_type |Krävs |Måste innehålla `id_token` för OpenID Connect inloggning. Det kan också innehålla hello svarstyp `token`. Om du använder `token`, din app omedelbart kan ta emot en åtkomst-token från hello auktorisera slutpunkt, utan att göra en andra begäran toohello auktorisera slutpunkt.  Om du använder hello `token` svarstypen, hello `scope` parameter måste innehålla en omfattning som anger vilken resurs tooissue hello-token för. |
| redirect_uri |Rekommenderas |hello omdirigerings-URI för appen där autentisering svar kan skickas och tas emot av din app. Den måste matcha en hello omdirigerings-URI: er som du har registrerat i hello portal, förutom att det måste vara URL-kodade. |
| response_mode |Rekommenderas |Anger hello metoden toouse toosend hello resulterande token tillbaka tooyour app.  Implicit flöden, Använd `fragment`. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope. Ett enda scope-värde anger tooAzure AD båda hello behörigheter som tas emot. Hej `openid` omfång anger en behörighet toosign i hello användar- och hämta data om hello användaren i hello form av ID-token. (Du lär dig om detta mer senare i artikeln hello.) Hej `offline_access` scope är valfritt för webbprogram. Det innebär att din app måste en uppdateringstoken för långlivade åtkomst tooresources. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som också returneras hello token svar. Det kan vara en sträng med innehåll som du vill toouse. Vanligtvis ett slumpmässigt genererat, unikt värde används tooprevent attacker med förfalskning av begäran. hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade som hello sidan de befann sig i. |
| temporärt ID |Krävs |Ett värde som ingår i hello-begäran (genereras av hello app) som ingår i hello resulterande ID-token som ett anspråk. hello app kan kontrollera det här värdet toomitigate token replay-attacker. Hello-värdet är vanligtvis en slumpmässig, unik sträng som kan använda tooidentify hello ursprung hello-begäran. |
| P |Krävs |hello princip tooexecute. Hello namnet för en princip som skapas i din Azure AD B2C-klient. hello principvärdet namn måste börja med **b2c\_1\_**. Mer information finns i [Azure AD B2C inbyggda principer](active-directory-b2c-reference-policies.md). |
| kommandotolk |Valfri |hello typ av användarinteraktion som krävs. Hello enda giltiga värdet är för närvarande `login`. Detta tvingar hello användaren tooenter sina autentiseringsuppgifter på begäran. Enkel inloggning börjar inte gälla. |

Nu uppmanas användaren hello toocomplete hello princip arbetsflöde. Detta kan omfatta hello användaren att ange sina användarnamn och lösenord, logga in med en sociala identitet registrerar sig för hello katalog eller en annan siffra steg. Användaråtgärder beror på hur hello princip har definierats.

När hello användaren Slutför hello princip, Azure AD tillbaka ett svar tooyour app på hello-värde som du använde för `redirect_uri`. Den använder hello-metod som anges i hello `response_mode` parameter. hello svar är exakt hello samma för alla hello åtgärd användarscenarier, oberoende av hello-princip som har utförts.

### <a name="successful-response"></a>Lyckat svar
Ett lyckat svar som använder `response_mode=fragment` och `response_type=id_token+token` verkar vara hello följande med radbrytningar för läsbarhet:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Parameter | Beskrivning |
| --- | --- |
| access_token |hello åtkomst-token som hello app som begärdes.  hello åtkomst-token ska inte avkodas eller annars kontrolleras. Den kan hanteras som en täckande sträng. |
| token_type |hello tokentypen värde. hello Skriv endast att Azure AD stöder är ägar. |
| expires_in |hello tidslängd som hello åtkomst-token är giltig (i sekunder). |
| Omfång |hello scope som hello token är giltig för. Du kan också använda omfång toocache token för senare användning. |
| id_token |Hej ID-token som hello app som begärdes. Du kan använda hello ID token tooverify hello användarens identitet och starta en session med hello användare. Mer information om ID-token och deras innehåll finns i hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md). |
| state |Om en `state` parametern ingår i hello begäran hello samma värde som ska visas i hello svar. hello app bör kontrollera att hello `state` värden i hello förfrågan och svar är identiska. |

### <a name="error-response"></a>Felsvar
Felsvar kan också skickas toohello omdirigerings-URI så att hello app kan hantera dem på rätt sätt:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beskrivning |
| --- | --- |
| fel |En kod felsträng används tooclassify typer av fel som inträffar. Du kan också använda hello felkoden för felhantering. |
| error_description |Ett felmeddelande som kan hjälpa dig att identifiera hello grundorsaken till ett autentiseringsfel. |
| state |Finns hello fullständiga beskrivningen i föregående tabell hello. Om en `state` parametern ingår i hello begäran hello samma värde som ska visas i hello svar. hello app bör kontrollera att hello `state` värden i hello förfrågan och svar är identiska.|

## <a name="validate-hello-id-token"></a>Validera hello-ID-token
Ta emot en token med ID: T är inte tillräckligt med tooauthenticate hello-användare. Du måste verifiera signaturen för hello-ID-token och kontrollera hello anspråk i hello token per krav som din app. Azure AD B2C använder [JSON Web token (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) och offentlig nyckel kryptografi toosign token och kontrollera att de är giltiga.

Många bibliotek med öppen källkod som är tillgängliga för att validera JWTs, beroende på hello språket du föredrar toouse. Överväg att utforska tillgängliga bibliotek med öppen källkod i stället för att implementera din egen valideringslogik. Du kan använda hello informationen i den här artikeln toohelp du lära dig hur tooproperly använder dessa bibliotek.

Azure AD B2C har en slutpunkt för OpenID Connect metadata. En app kan använda hello endpoint toofetch information om Azure AD B2C vid körning. Informationen omfattar slutpunkter, token innehåll och nycklar för tokensignering. Det finns en JSON-dokumentet för metadata för varje princip i din Azure AD B2C-klient. Till exempel finns hello Metadatadokumentet för hello b2c_1_sign_in princip i hello fabrikamb2c.onmicrosoft.com klient på:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

En av hello egenskaperna för den här konfigurationsdokument är hello `jwks_uri`. hello värde för hello samma princip skulle vara:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

toodetermine vilken princip du har använt toosign en ID-token (och där toofetch hello metadata från), har du två alternativ. Först hello principnamn ingår i hello `acr` anspråk i `id_token`. Information om hur tooparse hello anspråk från en ID-token finns hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md). Ett annat alternativ är tooencode hello princip i hello värdet för hello `state` parameter när du skickar hello-begäran. Sedan kan avkoda hello `state` parametern toodetermine vilken princip som har använts. Antingen metoden är giltig.

När du har köpt hello Metadatadokumentet från hello OpenID Connect metadataslutpunkten kan kan du använda hello RSA-256 offentliga nycklar (som finns i den här slutpunkten) toovalidate hello signaturen för hello-ID-token. Det kan finnas flera nycklar som anges i den här slutpunkten vid en given tidpunkt, alla identifierade med en `kid`. hello-huvudet på `id_token` innehåller också en `kid` anspråk. Anger vilken av dessa nycklar har använt toosign hello-ID-token. Mer information, inklusive lära dig mer om [verifiera token](active-directory-b2c-reference-tokens.md#token-validation), se hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve hello information on this-->

När du validerar hello signaturen för hello-ID-token, Kräv verifiering av flera anspråk. Exempel:

* Validera hello `nonce` anspråk tooprevent token replay-attacker. Värdet ska du angav i hello inloggning begäran.
* Validera hello `aud` anspråk tooensure som hello-ID-token har utfärdats för din app. Värdet ska vara hello program-ID för din app.
* Validera hello `iat` och `exp` anspråk tooensure som hello-ID-token inte har upphört att gälla.

Flera mer verifieringar som du bör utföra beskrivs i detalj i hello [OpenID Connect Core Spec](http://openid.net/specs/openid-connect-core-1_0.html). Du kan också toovalidate ytterligare anspråk, beroende på ditt scenario. Några vanliga verifieringar inkluderar:

* Säkerställa hello användaren eller organisationen har registrerat dig för hello app.
* Säkerställa hello användaren har rätt behörighet och privilegier.
* Se till att en viss styrkan hos autentisering har uppstått, så som med Azure Multi-Factor Authentication.

Mer information om hello anspråk i en ID-token finns hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md).

När du har fullständigt validerat hello-ID-token, kan du börja en session med hello användare. Använd hello anspråk i hello ID token tooobtain information om hello användare i din app. Den här informationen kan användas för att visa, poster, auktorisering och så vidare.

## <a name="get-access-tokens"></a>Få åtkomst-token
Om hello enda som din web apps behov toodo är kör principer, kan du hoppa över hello följande avsnitten. hello informationen i följande avsnitt hello är tillämpliga tooweb appar som behöver toomake autentiserade anrop tooa webb-API, och som skyddas av Azure AD B2C.

Nu när du har registrerat hello användaren i tooyour sida app, kan du få åtkomst-token för anropande webb-API: er som skyddas av Azure AD. Även om du redan har fått en token med hjälp av hello `token` svarstypen, du kan använda den här metoden tooacquire token för ytterligare resurser utan att omdirigera hello användaren toosign i igen.

I en typisk web app-flöde, ska du göra detta genom att göra en förfrågan toohello `/token` slutpunkt.  Hello slutpunkt har dock inte stöd för CORS-begäranden, så gör AJAX-anrop tooget och uppdateringstoken är inte ett alternativ. Du kan i stället använda hello implicita flödet i en dold HTML iframe-element tooget nya token för andra webb-API: er. Här är ett exempel med radbrytningar för läsbarhet:

```

https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
&p=b2c_1_sign_in
```

| Parameter | Krävs? | Beskrivning |
| --- | --- | --- |
| client_id |Krävs |hello program-ID tilldelade tooyour app i hello [Azure-portalen](https://portal.azure.com). |
| response_type |Krävs |Måste innehålla `id_token` för OpenID Connect inloggning.  Det kan även innehålla hello svarstyp `token`. Om du använder `token` här appen kan omedelbart ta emot en åtkomst-token från hello auktorisera slutpunkt, utan att göra en andra begäran toohello auktorisera slutpunkt. Om du använder hello `token` svarstypen, hello `scope` parameter måste innehålla en omfattning som anger vilken resurs tooissue hello-token för. |
| redirect_uri |Rekommenderas |hello omdirigerings-URI för appen där autentisering svar kan skickas och tas emot av din app. Den måste matcha en hello omdirigerings-URI: er som du har registrerat i hello portal, förutom att det måste vara URL-kodade. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope.  Med alla omfattningar som du behöver för hello avsedd resurs för att hämta token. |
| response_mode |Rekommenderas |Anger hello-metod som används toosend hello resulterande token tillbaka tooyour App.  Kan vara `query`, `form_post`, eller `fragment`. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som returneras hello token svar.  Det kan vara en sträng med innehåll som du vill toouse.  Vanligtvis ett slumpmässigt genererat, unikt värde används tooprevent attacker med förfalskning av begäran.  hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade. Hello sida eller vy hello-användaren har till exempel på. |
| temporärt ID |Krävs |Ett värde som ingår i hello-begäran som skapats av hello-app som ingår i hello resulterande ID-token som ett anspråk.  hello app kan kontrollera det här värdet toomitigate token replay-attacker. Hello-värdet är vanligtvis en slumpmässig, unik sträng som identifierar hello ursprung hello-begäran. |
| kommandotolk |Krävs |Använd toorefresh och hämta token i en dold iframe `prompt=none` tooensure som hello iframe inte fastna på hello inloggningssidan och returnerar omedelbart. |
| login_hint |Krävs |toorefresh och hämta token i en dold iframe inkludera hello användarnamnet för användaren hello i det här tipset toodistinguish mellan flera sessioner hello användaren kan ha vid en given tidpunkt. Du kan extrahera hello användarnamn från en tidigare inloggning med hjälp av hello `preferred_username` anspråk. |
| domain_hint |Krävs |Kan vara `consumers` eller `organizations`.  För att uppdatera och hämta token i en dold iframe, måste du inkludera hello `domain_hint` värde i hello-begäran.  Extrahera hello `tid` anspråk från hello-ID-token för en tidigare inloggning toodetermine vilka värdet toouse.  Om hello `tid` anspråk värdet är `9188040d-6c67-4c5b-b112-36a304b66dad`, Använd `domain_hint=consumers`.  Annars använder `domain_hint=organizations`. |

Genom att ange hello `prompt=none` parameter för denna begäran antingen lyckas eller misslyckas omedelbart och returnerar tooyour program.  Ett lyckat svar skickas tooyour app på hello anges omdirigerings-URI, med hjälp av hello-metod som anges i hello `response_mode` parameter.

### <a name="successful-response"></a>Lyckat svar
Ett lyckat svar med hjälp av `response_mode=fragment` ser ut så här:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Parameter | Beskrivning |
| --- | --- |
| access_token |hello-token som hello app som begärdes. |
| token_type |hello tokentypen kommer alltid att innehavaren. |
| state |Om en `state` parametern ingår i hello begäran hello samma värde som ska visas i hello svar. hello app bör kontrollera att hello `state` värden i hello förfrågan och svar är identiska. |
| expires_in |Hur länge hello åtkomst-token är giltig (i sekunder). |
| Omfång |hello scope som hello åtkomst-token är giltig för. |

### <a name="error-response"></a>Felsvar
Felsvar också skickas toohello omdirigerings-URI så att hello app kan hantera dem på lämpligt sätt.  För `prompt=none`, ett förväntade fel ser ut så här:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som används tooclassify typer av fel som inträffar. Du kan också använda hello sträng tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa dig att identifiera hello grundorsaken till ett autentiseringsfel. |

Om du får detta felmeddelande i hello iframe begäran måste hello användaren interaktivt inloggningen igen tooretrieve en ny token. Du kan hantera det på ett sätt som passar för ditt program.

## <a name="refresh-tokens"></a>Uppdatera token
ID-token och åtkomst-token upphör att gälla efter en kort tidsperiod. Appen måste du förbereda toorefresh dessa säkerhetstoken med jämna mellanrum.  toorefresh båda typerna av token, utföra hello samma dolda iframe-begäran som vi använde vid ett tidigare exempel med hjälp av hello `prompt=none` parametern toocontrol Azure AD-stegen.  tooreceive en ny `id_token` värde, vara säker på att toouse `response_type=id_token` och `scope=openid`, och en `nonce` parameter.

## <a name="send-a-sign-out-request"></a>Skicka en begäran om utloggning
Om du vill toosign hello användare utanför hello appen kan omdirigera hello användaren tooAzure AD toosign ut. Om du inte gör det kanske hello användaren kan tooreauthenticate tooyour app utan att ange sina autentiseringsuppgifter igen. Det beror på att de har en giltig inloggning session med Azure AD.

Du kan bara omdirigera hello användaren toohello `end_session_endpoint` som är listade i hello samma OpenID Connect metadata dokument som beskrivs i [verifiera hello-ID-token](#validate-the-id-token). Exempel:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Krävs? | Beskrivning |
| --- | --- | --- |
| P |Krävs |hello princip toouse toosign hello användare utanför tillämpningsprogrammet. |
| post_logout_redirect_uri |Rekommenderas |hello Webbadress hello användaren måste vara omdirigerade tooafter lyckade utloggning. Om det inte finns, visar Azure AD B2C ett allmänt meddelande toohello. |

> [!NOTE]
> Dirigera hello användaren toohello `end_session_endpoint` tar bort vissa av hello användarens inloggning tillstånd med Azure AD B2C. Dock logga inte den hello användare utanför hello sociala identitet providern användarsession. Om hello användaren väljer hello samma identifiera providern under en efterföljande inloggning, hello användaren autentiseras utan att behöva ange sina autentiseringsuppgifter. Om en användare toosign utanför tillämpningsprogrammet Azure AD B2C, den inte nödvändigtvis de vill toocompletely logga ut från sina Facebook-konto, t.ex. Men för lokala konton avslutas hello användarens session korrekt.
> 
> 

## <a name="use-your-own-azure-ad-b2c-tenant"></a>Använda din egen Azure AD B2C-klient
dessa begäranden själv slutföra tootry hello följande tre steg. Ersätt exempelvärden för hello vi använder i den här artikeln med egna värden:

1. [Skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md). Använd hello namn för din klient i hello begäranden.
2. [Skapa ett program](active-directory-b2c-app-registration.md) tooobtain ett program-ID och ett `redirect_uri` värde. Inkludera en webbapp eller webb-API i din app. Du kan också kan du skapa en programhemlighet.
3. [Skapa dina principer](active-directory-b2c-reference-policies.md) tooobtain principens namn.

## <a name="samples"></a>Exempel

* [Skapa en enkel sida-app med Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi)
* [Skapa en enkel sida-app med hjälp av .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi)


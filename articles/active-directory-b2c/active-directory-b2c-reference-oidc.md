---
title: Webb-inloggning med OpenID Connect - Azure AD B2C | Microsoft Docs
description: "Skapa webbprogram med hjälp av hello Azure Active Directory-implementeringen av autentiseringsprotokollet för hello OpenID Connect"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 21d420c8-3c10-4319-b681-adf2e89e7ede
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 89e9cfa28e4e5c34304aea355cca2dd0c4b42abc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Web inloggning med OpenID Connect
OpenID Connect är ett autentiseringsprotokoll som bygger på OAuth 2.0 genom att använda toosecurely logga användare i tooweb program. Du kan flytta över registrering, inloggning med hjälp av hello Azure Active Directory B2C (Azure AD B2C) implementering av OpenID Connect och andra Identitetshantering upplever i ditt webbprogram program tooAzure Active Directory (AD Azure). Den här guiden visar hur toodo så på ett språkoberoende sätt. Beskriver hur toosend och ta emot HTTP-meddelanden utan att använda någon av våra bibliotek med öppen källkod.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) utökar hello OAuth 2.0 *auktorisering* protokoll som ska användas som en *autentisering* protokoll. Detta ger dig tooperform enkel inloggning med hjälp av OAuth. Hello begreppet införs en *ID token*, vilket är en säkerhetstoken som gör hello klienten tooverify hello hello användares identitet och få grundläggande profilinformation om hello användare.

Eftersom den utökar OAuth 2.0 kan också appar toosecurely hämta *åtkomst till token*. Du kan använda access_tokens tooaccess resurser som skyddas av en [auktorisering server](active-directory-b2c-reference-protocols.md#the-basics). Vi rekommenderar OpenID Connect om du utvecklar ett program som finns på en server och öppnas via en webbläsare. Om du vill tooadd identity management tooyour mobila eller stationära program med hjälp av Azure AD B2C kan du använda [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) i stället för OpenID Connect.

Azure AD B2C utökar hello standard OpenID Connect protokollet toodo över enkel autentisering och auktorisering. Det inför hello [parametern](active-directory-b2c-reference-policies.md), vilket gör att du toouse OpenID Connect tooadd användarupplevelser--som registrering, inloggning och profilhantering--tooyour app. Här kan vi hur du toouse OpenID Connect och principer tooimplement var och en av dessa funktioner i dina webbprogram. Vi också lära dig hur tooget åtkomst-token för åtkomst till webb-API: er.

hello exempel HTTP-begäranden i nästa avsnitt om hello använda våra exempel B2C-katalog, fabrikamb2c.onmicrosoft.com, samt våra exempelprogrammet, https://aadb2cplayground.azurewebsites.net och principer. Du är ledig tootry ut hello begäranden själv med hjälp av dessa värden eller ersätta dem med dina egna.
Lär dig hur för[kommer B2C-klient, program och principer](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Skicka autentiseringsbegäranden
När ditt webbprogram måste tooauthenticate hello användare och köra en princip, den kan dirigera hello användaren toohello `/authorize` slutpunkt. Detta är hello interaktiva delen av hello flödet där hello användaren vidtar åtgärder beroende på hello princip.

I den här förfrågan hello-klienten anger hello behörigheter måste tooacquire hello användaren i hello `scope` parametern och hello princip tooexecute i hello `p` parameter. Tre exempel tillhandahålls i hello följande avsnitt (med radbrytningar för att läsa), var och en med hjälp av en annan princip. tooget en känsla för hur fungerar varje begäran, försök klistra in hello i en webbläsare och körs.

#### <a name="use-a-sign-in-policy"></a>Använda en princip för inloggning
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Använda en princip för registrering
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Använda en princip för Redigera-profil
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Krävs? | Beskrivning |
| --- | --- | --- |
| client_id |Krävs |hello program-ID som hello [Azure-portalen](https://portal.azure.com/) tilldelade tooyour app. |
| response_type |Krävs |hello svarstyp som måste innehålla en ID-token för OpenID Connect. Om ditt webbprogram måste också token för att anropa ett webb-API, kan du använda `code+id_token`eftersom vi har gjort här. |
| redirect_uri |Rekommenderas |Hej `redirect_uri` parameter för din app, där autentisering svar kan skickas och tas emot av din app. Den måste matcha en hello `redirect_uri` parametrar som du har registrerat i hello portal, förutom att det måste vara URL-kodade. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope. Ett enda scope-värde som anger tooAzure AD både behörigheter som krävs. Hej `openid` omfång anger en behörighet toosign i hello användar- och hämta data om hello användaren i hello form av ID-token (mer toocome på den här hello nedan). Hej `offline_access` scope är valfritt för webbprogram. Anger det att din app måste en *uppdateringstoken* för långlivade åtkomst tooresources. |
| response_mode |Rekommenderas |hello-metod som ska använda toosend hello resulterande auktorisering kod tillbaka tooyour app. Det kan vara antingen `query`, `form_post`, eller `fragment`.  Hej `form_post` svar läge rekommenderas för bästa säkerhet. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som returneras också hello token svar. Det kan vara en sträng med innehåll som du vill använda. Ett slumpmässigt genererat unikt värde används vanligtvis för att förhindra attacker med förfalskning av begäran. hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade, exempelvis hello sidan de befann sig i. |
| temporärt ID |Krävs |Ett värde som ingår i hello-begäran (genereras av hello app) som ska tas med i hello resulterande ID-token som ett anspråk. hello app kan kontrollera det här värdet toomitigate token replay-attacker. hello-värdet är vanligtvis en slumpmässig unik sträng som kan använda tooidentify hello ursprung hello-begäran. |
| P |Krävs |hello-princip som kommer att utföras. Det är hello namnet på en princip som skapas i din B2C-klient. hello principvärdet namn måste börja med `b2c\_1\_`. Lär dig mer om principer och hello [expanderbara principramverk](active-directory-b2c-reference-policies.md). |
| kommandotolk |Valfri |hello typ av användarinteraktion som krävs. hello enda giltiga värdet just nu är `login`, vilket framtvingar hello användaren tooenter sina autentiseringsuppgifter på begäran. Enkel inloggning börjar inte gälla. |

Nu uppmanas användaren hello toocomplete hello princip arbetsflöde. Detta kan omfatta hello användaren att ange sina användarnamn och lösenord, logga in med en sociala identitet registrerar sig för hello directory eller ett annat nummer av steg, beroende på hur hello princip har definierats.

När hello användaren Slutför hello princip, Azure AD tillbaka ett svar tooyour app på hello anges `redirect_uri` parameter med hello-metod som har angetts i hello `response_mode` parameter. hello svar är hello samma för alla hello föregående fall, oberoende av hello-princip som körs.

Ett lyckat svar med `response_mode=fragment` skulle se ut:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beskrivning |
| --- | --- |
| id_token |Hej ID-token som hello app som begärdes. Du kan använda hello ID token tooverify hello användarens identitet och starta en session med hello användare. Mer information om ID-token och deras innehåll ingår i hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md). |
| Koden |hello auktorisering code hello appen begärs, om du har använt `response_type=code+id_token`. hello app kan använda hello auktorisering koden toorequest en åtkomst-token för en målresurs. Auktoriseringskoder är mycket tillfällig. Vanligtvis de går ut efter 10 minuter. |
| state |Om en `state` parametern ingår i hello begäran hello samma värde som ska visas i hello svar. hello app bör kontrollera att hello `state` värden i hello förfrågan och svar är identiska. |

Felsvar kan även skickas toohello `redirect_uri` parameter så hello appen kan hantera dem på rätt sätt:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beskrivning |
| --- | --- |
| fel |En felkod sträng som kan använda tooclassify typer av fel som inträffar och som kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |
| state |Se hello fullständig beskrivning i hello första tabellen i det här avsnittet. Om en `state` parametern ingår i hello begäran hello samma värde som ska visas i hello svar. hello app bör kontrollera att hello `state` värden i hello förfrågan och svar är identiska. |

## <a name="validate-hello-id-token"></a>Validera hello-ID-token
Bara tar emot en token med ID: T är inte tillräckligt med tooauthenticate hello-användare. Du måste verifiera signaturen för hello-ID-token och kontrollera hello anspråk i hello token per krav som din app. Azure AD B2C använder [JSON Web token (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) och offentlig nyckel kryptografi toosign token och kontrollera att de är giltiga.

Det finns många öppen källkod bibliotek som är tillgängliga för att validera JWTs, beroende på ditt språk preferensordning. Vi rekommenderar att utforska alternativen i stället för att implementera din egen valideringslogik. hello information här kan vara användbart i räkna ut hur tooproperly använder dessa bibliotek.

Azure AD B2C har OpenID Connect metadata slutpunkt, vilket gör att en app toofetch information om Azure AD B2C vid körning. Informationen omfattar slutpunkter, token innehåll och nycklar för tokensignering. Det finns en JSON-dokumentet för metadata för varje princip i din B2C-klient. Till exempel hello Metadatadokumentet för hello `b2c_1_sign_in` princip i `fabrikamb2c.onmicrosoft.com` finns på:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

En av hello egenskaperna för den här konfigurationsdokument är `jwks_uri`, vars värde för hello samma princip skulle vara:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

toodetermine vilken princip som användes i en ID-token-signering (och från där toofetch hello metadata), har du två alternativ. Först hello principnamn ingår i hello `acr` anspråk i hello-ID-token. Information om hur tooparse hello anspråk från en ID-token finns hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md). Ett annat alternativ är tooencode hello princip i hello värdet för hello `state` parameter när du skickar hello begäran och sedan avkoda toodetermine vilken princip som har använts. Antingen metoden är giltig.

När du har skaffat hello Metadatadokumentet från hello OpenID Connect metadataslutpunkten kan du använda hello 256 RSA-offentliga nycklar (som finns i den här slutpunkten) toovalidate hello signaturen för hello-ID-token. Det kan finnas flera nycklar som anges i den här slutpunkten vid en viss tidpunkt, alla identifierade med ett `kid` anspråk. hello rubriken för hello-ID-token innehåller också en `kid` anspråk, som anger vilken av dessa nycklar används toosign hello-ID-token. Mer information finns i hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md) (hello avsnitt på [verifiera token](active-directory-b2c-reference-tokens.md#token-validation), i synnerhet).
<!--TODO: Improve hello information on this-->

När du har verifierats hello signaturen för hello-ID-token, finns det flera anspråk som du behöver tooverify. Exempel:

* Du bör verifiera hello `nonce` anspråk tooprevent token replay-attacker. Värdet ska du angav i hello inloggning begäran.
* Du bör verifiera hello `aud` anspråk tooensure som hello-ID-token har utfärdats för din app. Värdet ska vara hello program-ID för din app.
* Du bör verifiera hello `iat` och `exp` anspråk tooensure som hello-ID-token inte har upphört att gälla.

Det finns också flera mer verifieringar som ska utföras. Dessa beskrivs i detalj i hello [OpenID Connect Core Spec](http://openid.net/specs/openid-connect-core-1_0.html).  Du kan också toovalidate ytterligare anspråk, beroende på ditt scenario. Några vanliga verifieringar inkluderar:

* Säkerställa som hello användare/organisation har registrerat dig för hello app.
* Säkerställa hello användaren har rätt behörighet/privilegier.
* Se till att en viss styrkan hos autentisering har inträffat, till exempel Azure Multi-Factor Authentication.

Mer information om hello anspråk i en ID-token finns hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md).

När du har validerat hello-ID-token kan du börja en session med hello användare. Du kan använda hello anspråk i hello ID token tooobtain information om hello användare i din app. Användningsområden för den här informationen omfattar bildskärm, poster och auktorisering.

## <a name="get-a-token"></a>Hämta en token
Om du behöver din web app tooonly köra principer, kan du hoppa över hello följande avsnitten. Dessa avsnitt är tillämpliga endast tooweb appar som behöver toomake autentiserade anrop tooa webb-API och också skyddas av Azure AD B2C.

Du kan lösa hello auktoriseringskod som du har köpt (med hjälp av `response_type=code+id_token`) för en token toohello önskad resursen genom att skicka en `POST` begära toohello `/token` slutpunkt. Är för närvarande hello resursen som du kan begära en token för ditt Apps egen backend-webb-API. hello-konventionen för att begära en token tooyourself är toouse appens klient-ID som hello omfattning:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameter | Krävs? | Beskrivning |
| --- | --- | --- |
| P |Krävs |Hej princip som har använt tooacquire hello auktorisering kod. Du kan inte använda en annan princip i den här förfrågan. Observera att du lägger till den här parametern toohello frågesträng, inte toohello `POST` brödtext. |
| client_id |Krävs |hello program-ID som hello [Azure-portalen](https://portal.azure.com/) tilldelade tooyour app. |
| grant_type |Krävs |Hej typ av bevilja som måste vara `authorization_code` för hello-auktoriseringskodflödet. |
| Omfång |Rekommenderas |En blankstegsavgränsad lista över scope. Ett enda scope-värde som anger tooAzure AD både behörigheter som krävs. Hej `openid` omfång anger en behörighet toosign i hello användar- och hämta data om hello användaren i hello form av id_token parametrar. Det kan vara används tooget token tooyour Apps egen backend-webb-API, som representeras av hello samma program-ID som hello-klient. Hej `offline_access` omfång anger att din app behöver en uppdateringstoken för långlivade åtkomst tooresources. |
| Koden |Krävs |hello auktoriseringskod som du har införskaffade i hello första del av hello flödet. |
| redirect_uri |Krävs |Hej `redirect_uri` parametern för hello program som du fick hello auktoriseringskod. |
| client_secret |Krävs |Hej programhemlighet som du genererade på hello [Azure-portalen](https://portal.azure.com/). Den här programhemligheten är en viktig säkerhetsuppgift artefakt. Du bör lagra den på ett säkert sätt på din server. Du bör också rotera denna klienthemlighet regelbundet. |

Det ser ut som ett lyckat token svar:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beskrivning |
| --- | --- |
| not_before |hello tid vid vilken hello token betraktas som giltigt epok tidpunkt. |
| token_type |hello tokentypen värde. Hej typ som har stöd för Azure AD är `Bearer`. |
| access_token |hello signerade JWT-token som du har begärt. |
| Omfång |hello scope för vilka hello token är giltig. Dessa kan användas för att cachelagra token för senare användning. |
| expires_in |hello tidslängd som hello åtkomst-token är giltig (i sekunder). |
| refresh_token |En token för uppdatering av OAuth 2.0. hello app kan använda den här token tooacquire ytterligare token när hello aktuella token upphör att gälla. Uppdatera token är långlivade och kan inte används tooretain åtkomst tooresources under längre tid. Mer information finns i toohello [B2C tokenreferens](active-directory-b2c-reference-tokens.md). Observera att du måste ha använt hello scope `offline_access` i både hello auktorisering och token begäranden i ordning tooreceive en uppdateringstoken. |

Felsvar som liknar:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Parameter | Beskrivning |
| --- | --- |
| fel |En felkod sträng som kan använda tooclassify typer av fel som inträffar och som kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |

## <a name="use-hello-token"></a>Använd hello-token
Nu när du har har skaffat ett åtkomsttoken, du kan använda hello token i begäranden tooyour backend-webb-API: er genom att inkludera i hello `Authorization` huvud:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-hello-token"></a>Uppdatera hello-token
ID-token är tillfällig. Du måste uppdatera dem när de upphör att gälla toocontinue som kan tooaccess resurser. Du kan göra det genom att skicka in en annan `POST` begära toohello `/token` slutpunkt. Den här tiden kan ge hello `refresh_token` parameter i stället för hello `code` parameter:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Krävs | Beskrivning |
| --- | --- | --- |
| P |Krävs |hello-princip som har använt tooacquire hello ursprungliga uppdateringstoken. Du kan inte använda en annan princip i den här förfrågan. Observera att du lägger till den här parametern toohello frågesträng, inte toohello POST brödtext. |
| client_id |Krävs |hello program-ID som hello [Azure-portalen](https://portal.azure.com/) tilldelade tooyour app. |
| grant_type |Krävs |bevilja som måste vara en uppdateringstoken för denna del av hello auktoriseringskodflödet hello typ. |
| Omfång |Rekommenderas |En blankstegsavgränsad lista över scope. Ett enda scope-värde som anger tooAzure AD både behörigheter som krävs. Hej `openid` omfång anger en behörighet toosign i hello användar- och hämta data om hello användaren i hello form av ID-token. Det kan vara används tooget token tooyour Apps egen backend-webb-API, som representeras av hello samma program-ID som hello-klient. Hej `offline_access` omfång anger att din app behöver en uppdateringstoken för långlivade åtkomst tooresources. |
| redirect_uri |Rekommenderas |Hej `redirect_uri` parametern för hello program som du fick hello auktoriseringskod. |
| refresh_token |Krävs |hello ursprungliga uppdateringstoken som du har införskaffade i hello andra ben hello flödet. Observera att du måste ha använt hello scope `offline_access` i både hello auktorisering och token begäranden i ordning tooreceive en uppdateringstoken. |
| client_secret |Krävs |Hej programhemlighet som du genererade på hello [Azure-portalen](https://portal.azure.com/). Den här programhemligheten är en viktig säkerhetsuppgift artefakt. Du bör lagra den på ett säkert sätt på din server. Du bör också rotera denna klienthemlighet regelbundet. |

Det ser ut som ett lyckat token svar:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beskrivning |
| --- | --- |
| not_before |hello tid vid vilken hello token betraktas som giltigt epok tidpunkt. |
| token_type |hello tokentypen värde. Hej typ som har stöd för Azure AD är `Bearer`. |
| access_token |hello signerade JWT-token som du har begärt. |
| Omfång |hello omfattning som hello token är giltig för, som kan användas för att cachelagra token för senare användning. |
| expires_in |hello tidslängd som hello åtkomst-token är giltig (i sekunder). |
| refresh_token |En token för uppdatering av OAuth 2.0. hello app kan använda den här token tooacquire ytterligare token när hello aktuella token upphör att gälla.  Uppdatera token är långlivade och kan inte används tooretain åtkomst tooresources under längre tid. Mer information finns i toohello [B2C tokenreferens](active-directory-b2c-reference-tokens.md). |

Felsvar som liknar:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Parameter | Beskrivning |
| --- | --- |
| fel |En felkod sträng som kan använda tooclassify typer av fel som inträffar och som kan vara används tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa utvecklare identifiera hello grundorsaken till ett autentiseringsfel. |

## <a name="send-a-sign-out-request"></a>Skicka en begäran om utloggning
Om du vill toosign hello användare utanför hello appen är inte tillräckligt med tooclear appens cookies eller annars avsluta hello-session med hello användare. Du måste också omdirigera hello användaren tooAzure AD toosign ut. Om du inte toodo så kanske hello användaren kan tooreauthenticate tooyour app utan att ange sina autentiseringsuppgifter igen. Det beror på att de har en giltig inloggning session med Azure AD.

Du kan bara omdirigera hello användaren toohello `end_session` slutpunkt som anges i hello OpenID Connect Metadatadokumentet som beskrivits tidigare i hello ”verifiera hello-ID-token” avsnittet:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Krävs? | Beskrivning |
| --- | --- | --- |
| P |Krävs |hello-princip som du vill toouse toosign hello användare utanför tillämpningsprogrammet. |
| post_logout_redirect_uri |Rekommenderas |hello Webbadress hello användaren måste vara omdirigerade tooafter lyckade utloggning. Om det inte finns, visar Azure AD B2C hello användaren ett allmänt meddelande. |

> [!NOTE]
> Även om dirigera hello användaren toohello `end_session` endpoint raderar vissa hello användarens inloggning tillstånd med Azure AD B2C, signerar inte hello användare utanför sin sociala identitet provider (IDP)-session. Om hello användaren väljer hello samma IDP under en efterföljande inloggning, de kommer att autentisera, utan att behöva ange sina autentiseringsuppgifter. Om en användare toosign utanför tillämpningsprogrammet B2C, den inte nödvändigtvis de vill toosign utanför deras Facebook-konto. Men i hello fallet för lokala konton avslutas hello användarens session korrekt.
> 
> 

## <a name="use-your-own-b2c-tenant"></a>Använda en egen B2C-klient
Om du vill tootry dessa begäranden själv, måste du först utför de här tre stegen och Skriv hello exempelvärden som beskrivs ovan med dina egna:

1. [Skapa en B2C-klient](active-directory-b2c-get-started.md), och använder hello namn för din klient i hello begäranden.
2. [Skapa ett program](active-directory-b2c-app-registration.md) tooobtain ett-ID. Inkludera en web app/webb-API i din app. Du kan också skapa ett programhemlighet.
3. [Skapa dina principer](active-directory-b2c-reference-policies.md) tooobtain principens namn.


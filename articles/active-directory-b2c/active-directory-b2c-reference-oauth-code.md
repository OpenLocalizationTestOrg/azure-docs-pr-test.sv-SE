---
title: "Auktoriseringskodflödet - Azure AD B2C | Microsoft Docs"
description: "Lär dig hur toobuild webbappar med hjälp av autentiseringsprotokollet Azure AD B2C och OpenID Connect."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: c371aaab-813a-4317-97df-b62e2f53d865
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 6bf9d37310bd45b39bda346441413556f9fd4fdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0-auktoriseringskodflödet
Du kan använda hello OAuth 2.0 auktorisering kod bevilja i appar som installeras på en enhet toogain åtkomst tooprotected resurser, till exempel web API: er. Du kan lägga till registrering, inloggning med hjälp av hello Azure Active Directory B2C (Azure AD B2C) implementering av OAuth 2.0 och andra Identitetshantering uppgifter tooyour bärbara och stationära appar. Den här artikeln är språkoberoende. I hello artikeln beskriver vi hur toosend och ta emot HTTP-meddelanden utan att använda alla bibliotek med öppen källkod.

<!-- TODO: Need link toolibraries -->

hello OAuth 2.0-auktoriseringskodflödet beskrivs i [avsnittet 4.1 av hello OAuth 2.0-specifikationen](http://tools.ietf.org/html/rfc6749). Du kan använda för autentisering och auktorisering i de flesta apptyper, inklusive [webbappar](active-directory-b2c-apps.md#web-apps) och [internt installerade appar](active-directory-b2c-apps.md#mobile-and-native-apps). Du kan använda hello OAuth 2.0 auktorisering kod flödet toosecurely hämta *åtkomst till token* för dina appar som kan vara används tooaccess resurser som skyddas av en [auktorisering server](active-directory-b2c-reference-protocols.md#the-basics).

Den här artikeln fokuserar på hello **offentliga klienter** OAuth 2.0-auktoriseringskodflödet. En offentlig klient är alla klientprogram som inte kan vara betrodda toosecurely Underhåll hello integriteten hos ett hemligt lösenord. Detta inkluderar mobilappar och skrivbordsappar i stort sett alla program som körs på en enhet och måste tooget åtkomsttoken. 

> [!NOTE]
> tooadd identity management tooa webb-app med hjälp av Azure AD B2C, Använd [OpenID Connect](active-directory-b2c-reference-oidc.md) i stället för OAuth 2.0.

Azure AD B2C utökar hello standard OAuth 2.0 flödar toodo mer än enkel autentisering och auktorisering. Det inför hello [parametern](active-directory-b2c-reference-policies.md). Med inbyggda principer kan du använda OAuth 2.0 tooadd användaren upplever tooyour app som registrering, inloggning och profilhantering. I den här artikeln visar vi dig hur toouse OAuth 2.0 och principer tooimplement var och en av dessa funktioner i dina interna program. Vi också visar hur tooget åtkomst-token för åtkomst till webb-API: er.

I hello exempel HTTP-begäranden i den här artikeln, vi använda vårt exempel Azure AD B2C-katalog, **fabrikamb2c.onmicrosoft.com**. Vi använder också våra exempelprogrammet och principer. Du kan också försöka hello begäranden med hjälp av dessa värden eller kan du ersätta dem med egna värden.
Lär dig hur för[hämta egna Azure AD B2C-katalog, program och principer](#use-your-own-azure-ad-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. Hämta en auktoriseringskod för
hello-auktoriseringskodflödet börjar med hello klienten dirigera hello användaren toohello `/authorize` slutpunkt. Detta tillhör hello interaktiva hello flödet där hello användaren vidtar åtgärder. I den här förfrågan hello-klienten anger i hello `scope` parametern hello behörigheter att den behöver tooacquire från hello användare. I hello `p` parametern indikerar det hello princip tooexecute. hello använda följande tre exempel (med radbrytningar för att läsa) varje en annan princip.

### <a name="use-a-sign-in-policy"></a>Använda en princip för inloggning
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>Använda en princip för registrering
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Använda en princip för Redigera-profil
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parameter | Krävs? | Beskrivning |
| --- | --- | --- |
| client_id |Krävs |hello program-ID tilldelade tooyour app i hello [Azure-portalen](https://portal.azure.com). |
| response_type |Krävs |hello svarstypen, måste innehålla `code` för hello-auktoriseringskodflödet. |
| redirect_uri |Krävs |hello omdirigerings-URI för appen där autentisering svar skickas och tas emot av din app. Den måste matcha en hello omdirigerings-URI: er som du har registrerat i hello portal, förutom att det måste vara URL-kodade. |
| Omfång |Krävs |En blankstegsavgränsad lista över scope. Ett enda scope-värde anger tooAzure Active Directory (AD Azure) både hello behörigheter som tas emot. Med hjälp av hello klient-ID som hello scope visar att din app behöver ett åtkomsttoken som kan användas med tjänsten eller webb-API, som representeras av hello samma klient-ID.  Hej `offline_access` scope visar att din app behöver en uppdateringstoken för långlivade åtkomst tooresources. Du kan också använda hello `openid` omfång toorequest en ID-token från Azure AD B2C. |
| response_mode |Rekommenderas |hello-metod som du använder toosend hello resulterande auktorisering kod tillbaka tooyour app. Det kan vara `query`, `form_post`, eller `fragment`. |
| state |Rekommenderas |Ett värde som ingår i hello-begäran som returneras hello token svar. Det kan vara en sträng med innehåll som du vill toouse. Vanligtvis ett slumpmässigt genererat unikt värde används tooprevent attacker med förfalskning av begäran. hello tillstånd är också används tooencode information om hello användarens tillstånd i hello app innan hello autentiseringsbegäran inträffade. Hello sidan hello användaren utfördes eller hello princip som kördes. |
| P |Krävs |hello-princip som körs. Hello namnet för en princip som skapas i din Azure AD B2C-katalog. hello principvärdet namn måste börja med **b2c\_1\_**. toolearn mer information om principer finns [Azure AD B2C inbyggda principer](active-directory-b2c-reference-policies.md). |
| kommandotolk |Valfri |hello typ av användarinteraktion som krävs. Hello enda giltiga värdet är för närvarande `login`, vilket framtvingar hello användaren tooenter sina autentiseringsuppgifter på begäran. Enkel inloggning börjar inte gälla. |

Nu uppmanas användaren hello toocomplete hello princip arbetsflöde. Detta kan omfatta hello användaren att ange sina användarnamn och lösenord, logga in med en sociala identitet registrerar sig för hello katalog eller en annan siffra steg. Användaråtgärder beror på hur hello princip har definierats.

När hello användaren Slutför hello princip, Azure AD tillbaka ett svar tooyour app på hello-värde som du använde för `redirect_uri`. Den använder hello-metod som anges i hello `response_mode` parameter. hello svar är exakt hello samma för alla hello åtgärd användarscenarier, oberoende av hello-princip som har utförts.

Ett lyckat svar som använder `response_mode=query` ser ut så här:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // hello authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // hello value provided in hello request
```

| Parameter | Beskrivning |
| --- | --- |
| Koden |Hej auktoriseringskod hello appen begärdes. hello app kan använda hello auktorisering koden toorequest en åtkomst-token för en målresurs. Auktoriseringskoder är mycket tillfällig. Vanligtvis de går ut efter 10 minuter. |
| state |Se hello fullständig beskrivning i hello-tabellen i föregående avsnitt hello. Om en `state` parametern ingår i hello begäran hello samma värde som ska visas i hello svar. hello app bör kontrollera att hello `state` värden i hello förfrågan och svar är identiska. |

Felsvar kan också skickas toohello omdirigerings-URI så att hello app kan hantera dem på rätt sätt:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som du kan använda tooclassify hello typer av fel som inträffar. Du kan också använda hello sträng tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa dig att identifiera hello grundorsaken till ett autentiseringsfel. |
| state |Finns hello fullständiga beskrivningen i föregående tabell hello. Om en `state` parametern ingår i hello begäran hello samma värde som ska visas i hello svar. hello app bör kontrollera att hello `state` värden i hello förfrågan och svar är identiska. |

## <a name="2-get-a-token"></a>2. Hämta en token
Nu när du har skaffat ett auktoriseringskod du lösa in hello `code` för token toohello avsedda resursen genom att skicka en POST-begäran toohello `/token` slutpunkt. I Azure AD B2C är hello resursen som du kan begära en token för din Apps egen backend-webb-API. hello-konventionen som används för att begära en token tooyourself är toouse appens klient-ID som hello omfattning:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameter | Krävs? | Beskrivning |
| --- | --- | --- |
| P |Krävs |Hej princip som har använt tooacquire hello auktorisering kod. Du kan inte använda en annan princip i den här förfrågan. Observera att lägga till den här parametern toohello *frågesträng*, inte i hello POST brödtext. |
| client_id |Krävs |hello program-ID tilldelade tooyour app i hello [Azure-portalen](https://portal.azure.com). |
| grant_type |Krävs |bevilja hello typ. För hello auktoriseringskodflödet hello bevilja typ måste vara `authorization_code`. |
| Omfång |Rekommenderas |En blankstegsavgränsad lista över scope. Ett enda scope-värde anger tooAzure AD båda hello behörigheter som tas emot. Med hjälp av hello klient-ID som hello scope visar att din app behöver ett åtkomsttoken som kan användas med tjänsten eller webb-API, som representeras av hello samma klient-ID.  Hej `offline_access` scope visar att din app behöver en uppdateringstoken för långlivade åtkomst tooresources.  Du kan också använda hello `openid` omfång toorequest en ID-token från Azure AD B2C. |
| Koden |Krävs |hello auktoriseringskod som du har införskaffade i hello första del av hello flödet. |
| redirect_uri |Krävs |hello omdirigerings-URI för hello program som du fick hello auktoriseringskod. |

Ett lyckat svar för token som ser ut så här:

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
| token_type |hello tokentypen värde. hello Skriv endast att Azure AD stöder är ägar. |
| access_token |hello signerade JSON-Webbtoken (JWT) som du har begärt. |
| Omfång |hello scope som hello token är giltig för. Du kan också använda omfång toocache token för senare användning. |
| expires_in |hello tidslängd som hello token är giltig (i sekunder). |
| refresh_token |En token för uppdatering av OAuth 2.0. hello app kan använda den här token tooacquire ytterligare token när hello aktuella token upphör att gälla. Uppdateringstoken är långlivade. Du kan använda dem tooretain åtkomst tooresources för längre tid. Mer information finns i hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md). |

Felsvar se ut så här:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som du kan använda tooclassify hello typer av fel som inträffar. Du kan också använda hello sträng tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa dig att identifiera hello grundorsaken till ett autentiseringsfel. |

## <a name="3-use-hello-token"></a>3. Använd hello-token
Nu när du har har skaffat ett åtkomsttoken, du kan använda hello token i begäranden tooyour backend-webb-API: er genom att inkludera i hello `Authorization` huvud:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-hello-token"></a>4. Uppdatera hello-token
Åtkomst-token och ID-token är tillfällig. När de upphör att gälla måste du uppdatera dem toocontinue tooaccess resurser. toodo kan skicka en annan POST-begäran toohello `/token` slutpunkt. Den här tiden kan ge hello `refresh_token` i stället för hello `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameter | Krävs? | Beskrivning |
| --- | --- | --- |
| P |Krävs |hello-princip som har använt tooacquire hello ursprungliga uppdateringstoken. Du kan inte använda en annan princip i den här förfrågan. Observera att lägga till den här parametern toohello *frågesträng*, inte i hello POST brödtext. |
| client_id |Rekommenderas |hello program-ID tilldelade tooyour app i hello [Azure-portalen](https://portal.azure.com). |
| grant_type |Krävs |bevilja hello typ. För denna del av hello auktoriseringskodflödet hello bevilja typ måste vara `refresh_token`. |
| Omfång |Rekommenderas |En blankstegsavgränsad lista över scope. Ett enda scope-värde anger tooAzure AD båda hello behörigheter som tas emot. Med hjälp av hello klient-ID som hello scope visar att din app behöver ett åtkomsttoken som kan användas med tjänsten eller webb-API, som representeras av hello samma klient-ID.  Hej `offline_access` omfång anger att din app behöver en uppdateringstoken för långlivade åtkomst tooresources.  Du kan också använda hello `openid` omfång toorequest en ID-token från Azure AD B2C. |
| redirect_uri |Valfri |hello omdirigerings-URI för hello program som du fick hello auktoriseringskod. |
| refresh_token |Krävs |hello ursprungliga uppdateringstoken som du har införskaffade i hello andra ben hello flödet. |

Ett lyckat svar för token som ser ut så här:

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
| token_type |hello tokentypen värde. hello Skriv endast att Azure AD stöder är ägar. |
| access_token |hello signerade JWT som du har begärt. |
| Omfång |hello scope som hello token är giltig för. Du kan också använda hello scope toocache token för senare användning. |
| expires_in |hello tidslängd som hello token är giltig (i sekunder). |
| refresh_token |En token för uppdatering av OAuth 2.0. hello app kan använda den här token tooacquire ytterligare token när hello aktuella token upphör att gälla. Uppdatera token är långlivade och kan vara används tooretain åtkomst tooresources för längre tid. Mer information finns i hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md). |

Felsvar se ut så här:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Parameter | Beskrivning |
| --- | --- |
| fel |Ett felkod sträng som du kan använda tooclassify typer av fel som inträffar. Du kan också använda hello sträng tooreact tooerrors. |
| error_description |Ett felmeddelande som kan hjälpa dig att identifiera hello grundorsaken till ett autentiseringsfel. |

## <a name="use-your-own-azure-ad-b2c-directory"></a>Använda din egen Azure AD B2C-katalog
tootry dessa begäranden själv fullständig hello följande steg. Ersätt exempelvärden för hello som vi använde i den här artikeln med egna värden.

1. [Skapa en Azure AD B2C-katalog](active-directory-b2c-get-started.md). Använd hello namn i din katalog i hello begäranden.
2. [Skapa ett program](active-directory-b2c-app-registration.md) tooobtain ett program-ID och omdirigerings-URI. Inkludera en intern klient i din app.
3. [Skapa dina principer](active-directory-b2c-reference-policies.md) tooobtain principens namn.


---
title: aaaChanges toohello Azure AD v2.0-slutpunkten | Microsoft Docs
description: "En beskrivning av de ändringar som görs toohello app model v2.0 förhandsversion protokoll."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: d7b28a481e12d5dbbc4a10110193bdbd754f4929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="important-updates-toohello-v20-authentication-protocols"></a>Viktiga uppdateringar toohello v2.0-autentiseringsprotokoll
Uppmärksamhet utvecklare! Över hello kommer nästa två veckor, vi att göra några uppdateringar tooour v2.0-autentiseringsprotokoll som kan innebära att bryta ändringar för alla program som du har skrivit under våra förhandsversionen.  

## <a name="who-does-this-affect"></a>Som påverkar detta?
Alla appar som har skrivits toouse hello v2.0 konvergerat autentisering slutpunkt

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Mer information om hello v2.0-slutpunkten kan hittas [här](active-directory-appmodel-v2-overview.md).

Om du har skapat en app med hello v2.0-slutpunkten genom att skriva direkt toohello v2.0-protokollet med hjälp av vår OpenID Connect och OAuth web middlewares eller genom att använda andra 3 part bibliotek tooperform, bör du vara beredd tootest ditt projekt och gör nödvändiga ändringar.

## <a name="who-doesnt-this-affect"></a>Som detta påverkar inte?
Alla appar som har skrivits mot hello produktion Azure AD authentication slutpunkt

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Det här protokollet anges i bricka och ska inte ha några ändringar.

Dessutom om din app **endast** använder våra ADAL-biblioteket tooperform autentisering, behöver du inte toochange något.  ADAL har skärmad appen från hello ändringar.  

## <a name="what-are-hello-changes"></a>Vad är hello ändringar?
### <a name="removing-hello-x5t-value-from-jwt-headers"></a>Ta bort hello x5t värdet från JWT rubriker
hello v2.0-slutpunkten använder JWT-token i stor utsträckning, som innehåller ett huvud parametrar avsnitt med relevanta metadata om hello-token.  Om du avkoda hello rubriken för en av våra nuvarande JWTs hittade något som liknar:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Där båda hello ”x5t” och ”barn” egenskaperna identifiera hello offentlig nyckel som ska använda toovalidate hello token signatur, som hämtas från hello OpenID Connect metadata endpoint.

hello ändra vi gör här är tooremove hello ”x5t”-egenskap.  Du kan fortsätta toouse hello samma mekanismer toovalidate token, men bör enbart använda hello ”barn” egenskapen tooretrieve hello rätt offentlig nyckel, som anges i hello OpenID Connect-protokollet. 

> [!IMPORTANT]
> **Jobbet: Kontrollera att din app inte är beroende av hello förekomsten av hello x5t värde.**
> 
> 

### <a name="removing-profileinfo"></a>Ta bort profile_info
Tidigare hello v2.0-slutpunkten har returnerar ett JSON-objekt med base64-kodade i token svar som anropas `profile_info`.  När du begär en åtkomst-token från hello v2.0-slutpunkten genom att skicka en begäran om att:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

hello svar ser ut som hello följande JSON-objekt:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Hej `profile_info` värdet finns information om hello användare inloggad hello app – deras visningsnamn, förnamn, efternamn, e-postadress, identifierare och så vidare.  Främst hello `profile_info` användes för cachelagring av token och visa syften.

Vi nu bort hello `profile_info` värdet – men oroa dig inte, vi fortfarande lämnar den här informationen toodevelopers i en något annan plats.  I stället för `profile_info`, hello v2.0-slutpunkten nu returnerar ett `id_token` i varje token svar:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Du kan avkoda och parsa hello id_token tooretrieve hello samma information som du har fått från profile_info.  Hej id_token är en JSON-Webbtoken (JWT), med innehåll som anges av OpenID Connect.  hello kod för att göra det bör vara mycket lik – du behöver tooextract hello mitten segmentet (hello brödtext) i hello id_token och base64 avkoda den tooaccess hello JSON-objekt i.

Över hello nästa två veckor, bör du skapa kod din app tooretrieve hello användarinformation från antingen hello `id_token` eller `profile_info`, beroende på vad som finns.  På så sätt när hello ändring görs kan din app kan sömlöst hantera hello övergången från `profile_info` för`id_token` utan avbrott.

> [!IMPORTANT]
> **Jobbet: Kontrollera att din app inte är beroende av hello förekomsten av hello `profile_info` värde.**
> 
> 

### <a name="removing-idtokenexpiresin"></a>Ta bort id_token_expires_in
Liknande för`profile_info`, vi också bort hello `id_token_expires_in` parametern från svar.  Tidigare hello v2.0-slutpunkten kan returnera ett värde för `id_token_expires_in` tillsammans med varje id_token svar, till exempel på ett auktorisera svar:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Eller i ett token svar:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Hej `id_token_expires_in` värdet visar hello antalet sekunder som hello id_token gälla för.  Nu ska vi bort hello `id_token_expires_in` värdet helt.  Du kan i stället använda hello OpenID Connect standard `nbf` och `exp` tooexamine hello giltigheten för en id_token-anspråk.  Se hello [v2.0 tokenreferens](active-directory-v2-tokens.md) mer information om dessa anspråk.

> [!IMPORTANT]
> **Jobbet: Kontrollera att din app inte är beroende av hello förekomsten av hello `id_token_expires_in` värde.**
> 
> 

### <a name="changing-hello-claims-returned-by-scopeopenid"></a>Ändra hello anspråk som returneras av omfånget = openid
Den här ändringen kommer att hello viktigaste – faktum, påverkar nästan alla program som använder hello v2.0-slutpunkten.  Många program skicka begäranden toohello v2.0-slutpunkten med hello `openid` omfång som:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Idag, när användaren hello ger ditt medgivande för hello `openid` omfång, appen tar emot en mängd information om hello användare i hello resulterande id_token.  Dessa anspråk kan innehålla användarens namn, prioriterade användarnamn, e-postadress, objekt-ID och mer.

I den här uppdateringen kan vi ändrar hello information som hello `openid` omfång ger din appåtkomst till toobetter comform med hello OpenID Connect-specifikationen.  Hej `openid` omfång kommer endast att appen toosign hello användare i och får en appspecifika identifierare för hello användare på hello `sub` anspråk av hello id_token.  Hej anspråk i en id_token med endast hello `openid` omfång beviljas kommer att vara fritt från personligt identifierbar information.  Exempel id_token anspråk är:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Om du vill tooobtain personligt identifierbar information (PII) om hello användare i din app behöver din app toorequest ytterligare behörigheter från hello användare.  Vi introducerar stöd för två nya scope från hello OpenID Connect spec – hello `email` och `profile` scope – som gör att du toodo så.

* Hej `email` scope är mycket enkelt – det gör att din app åtkomst toohello användarens primära e-postadress via hello `email` anspråk i hello id_token.  Observera att hello `email` anspråk alltid visas inte i id_tokens – den endast ska ingå om de är tillgängliga i hello användarprofil.
* Hej `profile` omfång ger din app åtkomst tooall andra grundläggande information om hello användare – deras namn, prioriterade username, objekt-ID och så vidare.

Detta gör att du toocode din app på en minimal avslöjande sätt – du kan be hello användaren för bara hello uppsättning information att appen kräver toodo sitt jobb.  Om du vill hämta hello hela uppsättningen av användarinformation som din app erhåller toocontinue, bör du inkludera alla tre scope i din auktoriseringsförfrågningar:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Din app kan börja skicka hello `email` och `profile` scope omedelbart och hello v2.0-slutpunkten accepterar dessa två scope och börja begär behörighet från användare efter behov.  Dock hello ändra i hello tolkning av hello `openid` scope börjar inte gälla för några veckor.

> [!IMPORTANT]
> **Jobbet: Lägg till hello `profile` och `email` scope om information om hello användare krävs för din app.**  Observera att ADAL innehåller båda dessa behörigheter i begäranden som standard. 
> 
> 

### <a name="removing-hello-issuer-trailing-slash"></a>Ta bort hello utfärdaren avslutande snedstreck.
Tidigare tog hello utfärdaren värde som visas i token från hello v2.0-slutpunkten hello formulär

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Hello guid var där hello tenantId för hello Azure AD-klienten som utfärdade hello-token.  Med de här ändringarna blir hello utfärdaren värdet

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

i båda token och hello OpenID Connect discovery-dokumentet.

> [!IMPORTANT]
> **Jobbet: Kontrollera att din app tillåter hello utfärdaren både med och utan avslutande snedstreck vid verifiering av utfärdare.**
> 
> 

## <a name="why-change"></a>Varför ändra?
hello Huvudsyftet med introduktion till dessa ändringar är toobe som är kompatibla med hello OpenID Connect standard specifikation.  Genom OpenID Connect kompatibla, hoppas vi toominimize skillnaderna mellan integrera med Microsoft identity-tjänster och andra tjänster identitet i hello bransch.  Vi vill tooenable utvecklare toouse sina favorit öppen källkod autentiseringsbibliotek utan tooalter hello bibliotek tooaccommodate Microsoft skillnader.

## <a name="what-can-you-do"></a>Vad kan du göra?
Från och med idag, kan du börja att göra alla hello ändringar som beskrivs ovan.  Du bör omedelbart:

1. **Ta bort eventuella beroenden på hello `x5t` huvud-parametern.**
2. **Hantera hello övergången från `profile_info` för`id_token` i token svar.**
3. **Ta bort eventuella beroenden på hello `id_token_expires_in` parametern svar.**
4. **Lägg till hello `profile` och `email` scope tooyour app om din app behöver grundläggande information.**
5. **Acceptera utfärdaren värden i token både med och utan avslutande snedstreck.**

Vår [v2.0-protokollet dokumentationen](active-directory-v2-protocols.md) redan har uppdaterats tooreflect ändringarna, så att du kan använda den som en referens i hjälpa uppdatera din kod.

Om du har fler frågor på hello omfattning hello ändringar kan du antingen ledigt tooreach out toous på Twitter på @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Hur ofta sker protokollet ändringar?
Vi förutser inte några ytterligare bryta ändras toohello autentiseringsprotokoll.  Vi avsiktligt paketering ändringarna i en versionen så att du inte toogo via den här typen av uppdateringen igen när snart.  Naturligtvis vi kommer att fortsätta tooadd funktioner toohello konvergerat v2.0 authentication-tjänsten som du kan dra nytta av, men ändringarna bör vara tillsatsen och inte break befintlig kod.

Slutligen vill vi toosay Tack för provat saker under hello förhandsversionen.  hello insikter och erfarenheter från våra tidiga brukare har ovärderlig hittills och vi hoppas att du kommer att fortsätta tooshare dina åsikter och idéer.

Kodning Grattis!

hello Microsoft Identity Division


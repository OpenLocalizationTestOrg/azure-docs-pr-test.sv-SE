---
title: aaaAzure Active Directory v2.0 tokens referens | Microsoft Docs
description: "hello typer av token och anspråk som sänds av hello Azure AD v2.0-slutpunkten"
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: dc58c282-9684-4b38-b151-f3e079f034fd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 29eb5c3402aeae302ee7c6234488520495f85904
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-v20-tokens-reference"></a>Azure Active Directory v2.0 token-referens
hello Azure Active Directory (AD Azure) v2.0-slutpunkten genererar flera typer av säkerhetstoken i varje [autentiseringsflödet](active-directory-v2-flows.md). Den här referensen beskriver hello format, säkerhet egenskaperna och innehållet i varje typ av token.

> [!NOTE]
> hello v2.0-slutpunkten har inte stöd för alla Azure Active Directory-scenarier och funktioner. toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
>
>

## <a name="types-of-tokens"></a>Typer av token
hello v2.0-slutpunkten stöder hello [OAuth 2.0-protokollet för auktorisering](active-directory-v2-protocols.md), som använder token uppdatera token. hello v2.0-slutpunkten stöder också autentisering och logga in via [OpenID Connect](active-directory-v2-protocols.md). OpenID Connect introducerar en tredje typ av token, hello-ID-token. Var och en av dessa token representeras som en *ägar* token.

Ett ägartoken är en förenklad säkerhetstoken som ger hello ägar åtkomst tooa skyddad resurs. hello ägar är någon part som kan utgöra hello-token. Även om en part måste autentisera med Azure AD tooreceive hello ägar-token om åtgärder inte vidtas toosecure hello token under överföringen och lagringen, kan de snappas upp och används av en oavsiktlig part. Vissa säkerhetstoken har en inbyggd mekanism tooprevent obehörig parter från inte använder dem, men vill för ägar-token. Ägar-token måste transporteras i en säker kanal, till exempel transport layer security (HTTPS). Om ett ägartoken överförs utan den här typen av säkerhet, kan en skadlig part använder en ”man-in-the-middle-attack” tooacquire hello token och använda den för obehörig åtkomst tooa skyddad resurs. hello tillämpas samma säkerhetsprinciper när lagring eller cachelagring ägar-token för senare användning. Se alltid till att din app på ett säkert sätt överför och lagrar ägar-token. Mer säkerhet för ägar-token finns [RFC 6750 avsnitt 5](http://tools.ietf.org/html/rfc6750).

Många av hello-token som utfärdats av hello v2.0-slutpunkten implementeras som JSON Web token (JWTs). En JWT är en komprimerad, URL-säkert sätt tootransfer information mellan två parter. hello informationen i en JWT kallas en *anspråk*. Det är ett intyg om hello ägar- och ämne hello token. hello anspråk i en JWT är JavaScript Object Notation (JSON)-objekt som är kodade och serialiseras för överföring. Eftersom hello JWTs utfärdas av hello v2.0-slutpunkten är signerad men inte krypterad, du kan enkelt inspektera hello innehållet i en JWT för felsökning. Mer information om JWTs finns hello [JWT-specifikationen](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID-token
En ID-token är en typ av inloggning säkerhetstoken som appen tar emot när autentisering utförs med hjälp av [OpenID Connect](active-directory-v2-protocols.md). ID-token representeras som [JWTs](#types-of-tokens), och de innehåller anspråk som du kan använda toosign hello användaren i tooyour app. Du kan använda hello anspråk i en ID-token på olika sätt. Normalt använder administratörer ID token toodisplay kontoinformation eller toomake besluten om åtkomstkontroll i en app. hello v2.0-slutpunkten utfärdar bara en typ av ID-token som har en enhetlig uppsättning anspråk oavsett hello typ av användare som har loggat in. hello format och innehåll i ID-token är hello samma för användare med personliga Microsoft-konton och arbets-eller skolkonton.

ID-token är för närvarande är signerad men inte krypterad. När appen tar emot en ID-token, måste [Validera hello signatur](#validating-tokens) tooprove hello-token är äkta och verifiera några anspråk i hello token tooprove dess giltighet. hello anspråk som verifieras av en app varierar beroende på krav för scenarier, men din app måste utföra vissa [vanliga anspråk verifieringar](#validating-tokens) i varje scenario.

Vi ger dig hello fullständig information om anspråk i ID-token i hello följande avsnitt innehåller dessutom tooa exempel ID-token. Observera att anspråk i ID-token inte returneras i en viss ordning. Dessutom kan nya anspråk vara introduceras i ID-token när som helst. Din app bryter bör inte när nya anspråk som införs. hello följande lista innehåller hello anspråk som din app för närvarande kan på ett tillförlitligt sätt tolka. Du hittar mer information i hello [OpenID Connect specifikationen](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-id-token"></a>Exempel-ID-token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [!TIP]
> Övningen tooinspect hello-anspråk i hello exempel ID-token, klistra in hello exempel ID-token till [calebb.net](http://calebb.net/).
>
>

#### <a name="claims-in-id-tokens"></a>Anspråk i ID-token
| Namn | Begär | Exempelvärde | Beskrivning |
| --- | --- | --- | --- |
| målgrupp |`aud` |`6731de76-14a6-49ae-97bc-6eba6914391e` |Identifierar hello avsedda mottagaren av hello-token. I ID-token är hello din app program-ID som tilldelats tooyour app i hello portalen för registrering av Microsoft-program. Din app ska verifiera det här värdet och avvisa hello token om hello värdet inte matchar. |
| Utfärdaren |`iss` |`https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` |Identifierar hello säkerhetstokentjänst (STS) som skapar och återställer hello token och hello Azure AD-klient i vilka hello användaren autentiserades. Appen bör verifiera hello utfärdaren anspråk tooensure hello token kom från hello v2.0-slutpunkten. Det bör också använda hello GUID-delen av hello toorestrict hello anspråksuppsättningen klienter som kan logga in toohello app. hello GUID som visar hello användaren är en konsument användare från en Microsoft-kontot är `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Utfärdat till |`iat` |`1452285331` |hello tid vid vilken hello token har utfärdats, representeras i epok tid. |
| Förfallotid |`exp` |`1452289231` |hello tid vid vilken hello token blir ogiltig representeras i epok tid. Din app ska använda det här anspråket tooverify hello giltighet hello livslängd för token. |
| Inte före |`nbf` |`1452285331` |hello tid vid vilken hello token börjar gälla, representeras i epok tid. Det är vanligtvis hello samma som hello utfärdande tid. Din app ska använda det här anspråket tooverify hello giltighet hello livslängd för token. |
| Version |`ver` |`2.0` |hello version av hello-ID-token som definierats av Azure AD. Hello-värdet är för hello v2.0-slutpunkten `2.0`. |
| Klient-ID |`tid` |`b9419818-09af-49c2-b0c3-653adc1f376e` |Det är ett GUID som representerar hello Azure AD-klient som hello användaren från. För arbets- och skolkonton konton är hello GUID hello ändras klient-ID för hello organisation som hello användaren tillhör. Hello-värdet är för personliga konton `9188040d-6c67-4c5b-b112-36a304b66dad`. Hej `profile` omfattning krävs beställa tooreceive denna begäran. |
| Koden hash |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |hello kod hash ingår i ID-token endast när hello-ID-token utfärdas med en OAuth 2.0-auktoriseringskod. Det kan vara används toovalidate hello äkthet en auktoriseringskod. Mer information om hur du utför den här verifieringen finns hello [OpenID Connect specifikationen](http://openid.net/specs/openid-connect-core-1_0.html). |
| Åtkomst-token hash |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |hello åtkomst-token hash ingår i ID-token endast när hello-ID-token utfärdas med en OAuth 2.0-åtkomsttoken. Det kan vara används toovalidate hello äkthet en åtkomst-token. Mer information om hur du utför den här verifieringen finns hello [OpenID Connect specifikationen](http://openid.net/specs/openid-connect-core-1_0.html). |
| temporärt ID |`nonce` |`12345` |hello temporärt ID är en strategi för att minimera token replay-attacker. Appen kan ange ett temporärt ID i en auktoriseringsbegäran om med hjälp av hello `nonce` Frågeparametern. hello-värde som du anger i hello begäran har genererats i hello ID token `nonce` anspråk som ska ändras. Din app kan kontrollera hello värde mot hello-värdet som det anges på hello begäran, som associerar hello app-session med en specifik ID-token. Din app ska utföra den här verifieringen under verifieringsprocessen för hello-ID-token. |
| namn |`name` |`Babe Ruth` |hello anspråket ger ett läsbart värde som identifierar hello ämnet för hello-token. hello-värdet är inte säkert toobe unika, är det föränderliga och är utformad toobe används endast för visning. Hej `profile` omfattning krävs beställa tooreceive denna begäran. |
| E-post |`email` |`thegreatbambino@nyy.onmicrosoft.com` |hello primära e-postadress som är associerad med hello användarkonto, om sådan finns. Värdet är föränderliga och kan ändras med tiden. Hej `email` omfattning krävs beställa tooreceive denna begäran. |
| prioriterade användarnamn |`preferred_username` |`thegreatbambino@nyy.onmicrosoft.com` |hello primära användarnamnet som representerar hello användare i hello v2.0-slutpunkten. Det kan vara en e-postadress, telefonnummer och ett allmänt användarnamn utan angivet format. Värdet är föränderliga och kan ändras med tiden. Hej `profile` omfattning krävs beställa tooreceive denna begäran. |
| Ämne |`sub` |`MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | hello principal om vilka hello token Assert information, till exempel hello användare av en app. Det här värdet är oföränderlig och kan inte tilldela om eller återanvänds. Det kan vara används tooperform auktoriseringskontroller på ett säkert sätt, till exempel när hello token är används tooaccess en resurs och kan användas som en nyckel i databastabeller. Eftersom hello ämne finns alltid i hello token som Azure AD utfärdar, bör du använda det här värdet i ett system med allmänna tillstånd. hello ämne är dock en pairwise identifierare – det är unikt tooa visst program-ID.  Därför om en användare loggar in på två olika appar som använder två olika klient-ID: N, får de apparna som två olika värden för hello ämne anspråk.  Detta kan eller inte vara önskvärt beroende på dina krav arkitektur och sekretess. |
| Objekt-ID |`oid` |`a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Hej ändras identifierare för ett objekt i hello Microsoft identitetssystem i det här fallet ett användarkonto.  Det kan också vara används tooperform auktoriseringskontroller på ett säkert sätt och som en nyckel i databastabeller. Detta ID identifierar hello användare i program - två olika program som loggar in hello samma användare får hello samma värde i hello `oid` anspråk.  Det innebär att den kan användas när du skapar frågor tooMicrosoft onlinetjänster som hello Microsoft Graph.  hello Microsoft Graph returnerar detta ID som hello `id` -egenskapen för ett givet användarkonto.  Eftersom hello `oid` tillåter flera appar toocorrelate användare hello `profile` omfattning krävs beställa tooreceive denna begäran. Observera att om en användare finns i flera innehavare, hello användaren innehåller ett annat objekt-ID i varje klient - de anses vara olika konton, även om hello användarloggar in varje konto med hello samma autentiseringsuppgifter. |

### <a name="access-tokens"></a>Åtkomst-token
Åtkomst-token som utfärdas av hello v2.0-slutpunkten kan för närvarande används endast av Microsoft Services. Apparna bör inte kräver tooperform alla verifiering eller kontroll av åtkomsttoken för någon av hello stöds för närvarande scenarier. Du kan hantera åtkomst-token som helt ogenomskinlig. De är bara strängar som din app kan skicka tooMicrosoft i HTTP-förfrågningar.

I hello nära framtida hello v2.0 introduceras endpoint hello möjligheten för din app tooreceive åtkomst-token från andra klienter. Hello informationen i det här referensavsnittet kommer att uppdateras med hello information som du behöver för din app tooperform åtkomst-token verifiering och andra liknande uppgifter som helst.

När du begär en åtkomst-token från hello v2.0-slutpunkten returnerar metadata om hello åtkomsttoken för din app toouse även hello v2.0-slutpunkten. Informationen omfattar hello förfallotiden för hello åtkomst-token och hello omfattningar som är giltig. Din app använder det här metadata tooperform intelligent cachelagring av åtkomsttoken utan åtkomsttoken för tooparse öppna hello sig själv.

### <a name="refresh-tokens"></a>Uppdatera token
Uppdatera token är säkerhetstoken som din app kan använda tooget nya åtkomst till token i en OAuth 2.0-flödet. Din app kan använda uppdatera token tooachieve långsiktiga åtkomst tooresources för en användares räkning utan interaktion med användaren hello.

Uppdaterings-tokens är flera resurs. En uppdateringstoken som togs emot under en Tokenbegäran för en resurs kan lösas för åtkomst-token tooa helt annan resurs.

tooreceive en uppdatering i en token svar din app måste begära och beviljas hello `offline_acesss` omfång. Mer om hello toolearn `offline_access` omfång, se hello [medgivande och scope](active-directory-v2-scopes.md) artikel.

Uppdatera token är och alltid kommer att vara, helt ogenomskinlig tooyour app. De kan har utfärdats av hello Azure AD v2.0-slutpunkten och bara kontrolleras och tolkas av hello v2.0-slutpunkten. De är långlivade, men din app ska inte skrivas tooexpect som en uppdateringstoken ska gälla för en viss tidsperiod. Uppdaterings-tokens kan vara inaktuella när som helst av olika skäl. Hej sätt för din app tooknow om en uppdateringstoken är giltig är tooattempt tooredeem den genom att göra en tokenbegäran toohello v2.0-slutpunkten.

När du lösa in en uppdateringstoken för en ny åtkomsttoken (och om din app har beviljats hello `offline_access` omfattning), du får en ny uppdateringstoken hello token svar. Spara hello nyligen utfärdade uppdatera token, tooreplace hello något du använde i hello-begäran. Detta garanterar att uppdaterings-tokens vara giltigt så länge som möjligt.

## <a name="validating-tokens"></a>Verifiera token
För närvarande hello endast token validering dina appar bör måste tooperform verifieras ID-token. toovalidate en ID-token appen bör verifiera både hello-ID-token signatur och hello anspråk i hello-ID-token.

<!-- TODO: Link -->
Microsoft tillhandahåller bibliotek och kodexempel som visar hur tooeasily hantera token validering. I nästa avsnitt hello beskrivs hello underliggande processen. Flera från tredje part öppen källkod bibliotek är tillgängliga för JWT-verifiering. Det finns minst ett bibliotek alternativ för nästan alla plattformar och språk.

### <a name="validate-hello-signature"></a>Validera hello signatur
En JWT innehåller tre segment som avgränsas med hello `.` tecken. hello första segmentet kallas hello *huvud*, hello andra segmentet är hello *brödtext*, och tredje hello-segment är hello *signatur*. hello signatur segment kan vara används toovalidate hello äkthet hello-ID-token så att den kan vara betrott av din app.

ID-token är signerade med branschstandard asymmetriska krypteringsalgoritmer, till exempel RSA-256. hello rubriken på hello-ID-token har information om hello nyckeln och krypteringsmetod använt toosign hello-token. Exempel:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Hej `alg` anspråk anger hello-algoritmen som har använt toosign hello-token. Hej `kid` anspråk anger hello offentlig nyckel som har använt toosign hello-token.

När som helst kan hello v2.0-slutpunkten logga en ID-token med hjälp av någon av en specifik uppsättning privat-offentligt nyckelpar. hello v2.0-slutpunkten roterar regelbundet hello möjliga uppsättning nycklar, så att din app ska skrivas toohandle dessa nyckel ändras automatiskt. En rimlig frekvens toocheck för uppdateringar toohello offentliga nycklar som används av hello v2.0-slutpunkten är 24 timmar.

Du kan skaffa hello signering viktiga data som du behöver toovalidate hello signatur med hjälp av hello OpenID Connect metadata dokument som finns på:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> Försök hello URL i en webbläsare!
>
>

Metadatadokumentet är en JSON-objekt som har flera användbara delar av information, till exempel hello platsen för hello olika slutpunkter som krävs för OpenID Connect-autentisering.  hello-dokumentet innehåller också en *jwks_uri*, vilket ger hello platsen för hello uppsättning offentliga nycklar används toosign token. hello JSON-dokumentet på hello jwks_uri har alla hello offentlig nyckelinformation som används för närvarande. Din app kan använda hello `kid` anspråk i hello JWT huvud tooselect vilka offentliga nyckeln i det här dokumentet har använt toosign en token. Signaturverifiering utför sedan med hjälp av hello rätt offentlig nyckel och hello angivna algoritm.

Utför signaturverifiering är utanför hello omfånget för det här dokumentet. Många öppen källkod bibliotek är tillgängligt toohelp du med den här.

### <a name="validate-hello-claims"></a>Validera hello anspråk
När appen tar emot en ID-token när användaren loggar in, bör det också utföra några kontroller mot hello anspråk i hello-ID-token. Dessa inkludera, men är inte begränsade till:

* **målgruppen** anspråk tooverify som hello-ID-token har avsedda toobe angivna tooyour app
* **inte före** och **förfallotid** anspråk, tooverify som hello-ID-token inte har upphört att gälla
* **utfärdaren** anspråk tooverify hello token har utfärdats tooyour app av hello v2.0-slutpunkten
* **temporärt ID**, som en token replay-attacker lösning

En fullständig lista över anspråk verifieringar som din app ska utföra finns hello [OpenID Connect specifikationen](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Information om hello förväntade värden för dessa anspråk som ingår i hello [ID-token](# ID tokens) avsnitt.

## <a name="token-lifetimes"></a>Livslängd för token
Vi ger hello följande token livslängd endast i informationssyfte. hello information kan hjälpa dig när du utvecklar och felsöka appar. Dina appar får inte vara skrivs tooexpect någon av dessa livslängd tooremain konstant. Token kan livslängd och kommer att ändras när som helst.

| Token | Livslängd | Beskrivning |
| --- | --- | --- |
| ID-token (arbets- eller skolkonto konton) |1 timme |ID-token är oftast giltigt för 1 timme. Ditt webbprogram kan använda den här samma livstid toomaintain sin egen session med hello användaren (rekommenderas), eller om du kan välja en helt annan session livslängd. Om din app måste tooget en ny ID-token, måste det toomake en ny inloggning begäran toohello v2.0 auktorisera slutpunkt. Om hello användare har en giltig webbläsarsession med hello v2.0-slutpunkten kan hello användaren kan inte vara nödvändiga tooenter sina autentiseringsuppgifter igen. |
| ID-token (personliga konton) |24 timmar |ID-token för personliga konton är oftast giltigt under 24 timmar. Ditt webbprogram kan använda den här samma livstid toomaintain sin egen session med hello användaren (rekommenderas), eller om du kan välja en helt annan session livslängd. Om din app måste tooget en ny ID-token, måste det toomake en ny inloggning begäran toohello v2.0 auktorisera slutpunkt. Om hello användare har en giltig webbläsarsession med hello v2.0-slutpunkten kan hello användaren kan inte vara nödvändiga tooenter sina autentiseringsuppgifter igen. |
| Åtkomst-token (arbets- eller skolkonto konton) |1 timme |Visas i token svar som en del av hello token metadata. |
| Åtkomst-token (personliga konton) |1 timme |Visas i token svar som en del av hello token metadata. Åtkomsttoken som utfärdas åt personliga konton kan konfigureras för ett annat livstid, men 1 timme är vanliga. |
| Uppdatera token (arbets- eller skolkonto konto) |In too14 dagar |En enskild uppdateringstoken är giltig för 14 dagar. Hej uppdateringstoken kan dock bli ogiltiga när som helst av olika skäl, så att din app ska fortsätta tootry toouse en uppdateringstoken tills det misslyckas, eller tills appen ersätts med en ny uppdateringstoken. En uppdateringstoken blir ogiltig om det har gått 90 dagar sedan hello användarens sina autentiseringsuppgifter. |
| Uppdatera token (personliga konton) |Too1 år |En enskild uppdateringstoken är giltig för högst 1 år. Dock hello uppdateringstoken kan bli ogiltiga när som helst av olika skäl, så att din app ska fortsätta tootry toouse en uppdateringstoken tills den misslyckas. |
| Auktoriseringskoder (arbets- eller skolkonto konton) |10 minuter |Auktoriseringskoder är ändamålsenligt tillfällig och bör omedelbart lösas för åtkomst-token och uppdateringstoken när hello token tas emot. |
| Auktoriseringskoder (personliga konton) |5 minuter |Auktoriseringskoder är ändamålsenligt tillfällig och bör omedelbart lösas för åtkomst-token och uppdateringstoken när hello token tas emot. Auktoriseringskoder som har utfärdats för personliga konton är för enstaka användning. |

---
title: 'Azure Active Directory B2C: Token referens | Microsoft Docs'
description: "hello typer av token som utfärdats i Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/17/2017
ms.author: dastrock
ms.openlocfilehash: 776bc88cc0308875ac6e576f19ac9edf50319d08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Token-referens
Azure Active Directory B2C (Azure AD B2C) genererar flera typer av säkerhetstoken som den behandlar varje [autentiseringsflödet](active-directory-b2c-apps.md). Det här dokumentet beskriver hello format, säkerhet egenskaperna och innehållet i varje typ av token.

## <a name="types-of-tokens"></a>Typer av token
Azure AD B2C stöder hello [OAuth 2.0-protokollet för auktorisering](active-directory-b2c-reference-protocols.md), som utnyttjar både token uppdatera token. Det stöder också autentisering och logga in via [OpenID Connect](active-directory-b2c-reference-protocols.md), som innehåller en tredje typ av token: hello-ID-token. Var och en av dessa token representeras som ett ägartoken.

Ett ägartoken är en förenklad säkerhetstoken som ger hello ”ägar” åtkomst tooa skyddad resurs. hello ägar är någon part som kan utgöra hello-token. Azure AD måste de först autentisera en part innan det kan ta emot ett ägartoken. Men om det behövs hello åtgärder inte vidtas toosecure hello token i överföringen och lagringen kan snappas upp och används av en oavsiktlig part. Vissa säkerhetstoken har en inbyggd mekanism för att förhindra att obehöriga personer använder dem, men ägar-token har inte den här mekanismen. De måste transporteras i en säker kanal, till exempel transport layer security (HTTPS).

Om ett ägartoken överförs utanför en säker kanal, kan en obehörig part använder en man-in-the-middle-attack tooacquire hello token och använda den toogain obehörig åtkomst tooa skyddade resursen. hello samma säkerhetsprinciper gäller när ägar-token lagras eller cachelagras för senare användning. Se alltid till att appen skickar och lagrar ägar-token på ett säkert sätt.

Ytterligare information om säkerhet på ägar-token finns [RFC 6750 avsnitt 5](http://tools.ietf.org/html/rfc6750).

Många av hello-token som Azure AD B2C utfärdar implementeras som JSON web token (JWTs). En JWT är ett kompakt, URL-säkert sätt att överföra information mellan två parter. JWTs innehåller information som kallas anspråk. Dessa är intyg för information om hello ägar- och hello ämnet för hello-token. hello anspråk i JWTs är JSON-objekt som är kodade och serialiseras för överföring. Eftersom hello JWTs som utfärdats av Azure AD B2C signerad men inte krypterad, kan du enkelt inspektera hello innehållet i en JWT toodebug den. Det finns flera verktyg som kan göra detta, inklusive [calebb.net](http://calebb.net). Mer information om JWTs finns för[JWT specifikationer](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID-token
En ID-token är en typ av säkerhetstoken som din app som tar emot från hello Azure AD B2C `authorize` och `token` slutpunkter. ID-token representeras som [JWTs](#types-of-tokens), och de innehåller anspråk som du kan använda tooidentify användare i din app. När du har köpt ID-token från hello `authorize` slutpunkt, de är ofta använda toosign i användare tooweb program. När du har köpt ID-token från hello `token` slutpunkt, de kan skickas i HTTP-begäranden under kommunikation mellan två komponenter i hello samma program eller tjänst. Du kan använda hello anspråk i en ID-token som du vill. De är vanliga toodisplay konto information eller toomake besluten om åtkomstkontroll i en app.  

ID-token är signerat, men de för närvarande är krypterade inte. När din app eller API får du en ID-token, måste [Validera hello signatur](#token-validation) tooprove hello token är autentiska. Din app eller API måste du också verifiera några anspråk i hello token tooprove att det är giltigt. Hello anspråk validerad av en app kan variera beroende på krav för scenarier med hello, men din app måste utföra vissa [vanliga anspråk verifieringar](#token-validation) i varje scenario.

#### <a name="sample-id-token"></a>Exempel-ID-token
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Åtkomst-token
En åtkomst-token är också en typ av säkerhetstoken som din app som tar emot från hello Azure AD B2C `authorize` och `token` slutpunkter. Åtkomsttoken även visas i form av [JWTs](#types-of-tokens), och de innehåller anspråk som du kan använda tooidentify hello beviljats behörighet tooyour API: er. Åtkomsttoken är signerat, men de för närvarande är krypterade inte. Åtkomst-token ska använda tooprovide tooAPIs och resurs klientåtkomstservrar. Läs mer om hur för[använder åtkomsttoken](active-directory-b2c-access-tokens.md). 

När din API tar emot en åtkomst-token, måste [Validera hello signatur](#token-validation) tooprove hello token är autentiska. Din API måste också verifiera några anspråk i hello token tooprove att det är giltigt. Hello anspråk validerad av en app kan variera beroende på krav för scenarier med hello, men din app måste utföra vissa [vanliga anspråk verifieringar](#token-validation) i varje scenario.

### <a name="claims-in-id-and-access-tokens"></a>Anspråk i ID och åtkomst-token
När du använder Azure AD B2C har detaljerad kontroll över hello innehåll av dina token. Du kan konfigurera [principer](active-directory-b2c-reference-policies.md) toosend vissa anger användardata i anspråk som appen kräver för åtgärderna. Dessa anspråk kan innehålla standardegenskaper till exempel hello användare `displayName` och `emailAddress`. De kan även inkludera [anpassade användarattribut](active-directory-b2c-reference-custom-attr.md) som du kan definiera i din B2C-katalog. Varje ID och åtkomst-token som visas innehåller en viss uppsättning säkerhetsrelaterade anspråk. Dina program kan använda dessa anspråk toosecurely autentisera användare och förfrågningar.

Observera att hello anspråk i ID-token inte returneras i någon särskild ordning. Dessutom kan du introduceras nya anspråk i ID-token när som helst. Din app bryter bör inte när nya anspråk introduceras. Här följer hello anspråk som du förväntar dig tooexist i ID och åtkomst-token som utfärdats av Azure AD B2C. Ytterligare anspråk bestäms av principer. Försök undersöks hello anspråk i hello exempel ID token genom att klistra in det i övningen [calebb.net](http://calebb.net). Mer information finns i hello [OpenID Connect specifikationen](http://openid.net/specs/openid-connect-core-1_0.html).

| Namn | Begär | Exempelvärde | Beskrivning |
| --- | --- | --- | --- |
| Målgrupp |`aud` |`90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` |En målgrupp anspråk identifierar hello avsedda mottagaren av hello-token. För Azure AD B2C är hello din app program-ID som tilldelats tooyour app i portalen för registrering av hello-app. Din app ska verifiera det här värdet och avvisa hello token om det inte matchar. |
| Utfärdaren |`iss` |`https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` |Det här anspråket identifierar hello säkerhetstokentjänst (STS) som skapar och returnerar hello-token. Den visar även hello Azure AD-katalog i vilka hello användaren autentiserades. Appen bör verifiera hello utfärdaren anspråk tooensure hello token kom från hello Azure Active Directory v2.0-slutpunkten. |
| Utfärdat till |`iat` |`1438535543` |Detta anspråk är hello tid vid vilken hello token har utfärdats, representeras i epok tid. |
| Förfallotid |`exp` |`1438539443` |hello finns Förfallodatum tid hello tid vid vilken hello token blir ogiltig, representeras i epok tid. Din app ska använda det här anspråket tooverify hello giltighet hello livslängd för token. |
| Inte före |`nbf` |`1438535543` |Detta anspråk är hello tid vid vilken hello token blir giltigt, representeras i epok tid. Detta är vanligtvis hello samma som hello tid hello token har utfärdats. Din app ska använda det här anspråket tooverify hello giltighet hello livslängd för token. |
| Version |`ver` |`1.0` |Detta är hello version av hello-ID-token som definierats av Azure AD. |
| Koden hash |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |En kod hash ingår i en ID-token endast när hello token utfärdas tillsammans med en kod för auktorisering av OAuth 2.0. En kod hash kan vara används toovalidate hello äkthet en auktoriseringskod. Mer information om hur tooperform denna validering kan se hello [OpenID Connect specifikationen](http://openid.net/specs/openid-connect-core-1_0.html).  |
| Åtkomst-token hash |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |En åtkomst-token hash ingår i en ID-token endast när hello token utfärdas tillsammans med en OAuth 2.0-åtkomsttoken. En åtkomst-token-hash kan vara används toovalidate hello äkthet en åtkomst-token. Mer information om hur tooperform denna validering kan se hello [OpenID Connect-specifikationen](http://openid.net/specs/openid-connect-core-1_0.html)  |
| temporärt ID |`nonce` |`12345` |Ett temporärt ID är en strategi används toomitigate token replay-attacker. Appen kan ange ett temporärt ID i en auktoriseringsbegäran om med hjälp av hello `nonce` Frågeparametern. hello-värde som du anger i hello begäran kommer orsakat oförändrade i hello `nonce` anspråk för en ID-token. Detta gör att din app tooverify hello värde mot hello-värdet som det anges på hello begäran, som associerar hello app-session med en viss ID-token. Din app ska utföra den här verifieringen under verifieringsprocessen för hello-ID-token. |
| Ämne |`sub` |`884408e1-2918-4cz0-b12d-3aa027d7563b` |Detta är hello principal om vilka hello token Assert information, till exempel hello användare av en app. Det här värdet är oföränderlig och kan inte tilldela om eller återanvänds. Det kan vara används tooperform auktoriseringskontroller på ett säkert sätt, till exempel när hello token används tooaccess en resurs. Som standard fylls hello ämne anspråk med hello objekt-ID för hello användare i hello-katalogen. Det finns fler toolearn [Azure Active Directory B2C: Token, session och enkel inloggning konfiguration](active-directory-b2c-token-session-sso.md). |
| Autentisering kontexten klassreferens |`acr` |Inte tillämpligt |Inte används för närvarande, förutom i hello skiftläget för äldre principer. Det finns fler toolearn [Azure Active Directory B2C: Token, session och enkel inloggning konfiguration](active-directory-b2c-token-session-sso.md). |
| Förtroende framework-princip |`tfp` |`b2c_1_sign_in` |Det här är hello hello-princip som har använt tooacquire hello-ID-token. |
| Autentisering |`auth_time` |`1438535543` |Detta anspråk är hello tid då en sista angivna autentiseringsuppgifter, som representeras i epok tid. |

### <a name="refresh-tokens"></a>Uppdatera token
Uppdatera token är säkerhetstokens som din app använder tooacquire nya ID-token och få åtkomst till token i en OAuth 2.0-flödet. De ger din app med långsiktig åtkomst tooresources åt användare utan interaktion med användare.

tooreceive en uppdateringstoken i ett token svar din app måste begära hello `offline_acesss` omfång. Mer om hello toolearn `offline_access` omfång, se toohello [protokollreferens för Azure AD B2C](active-directory-b2c-reference-protocols.md).

Uppdatera token är och ska alltid vara helt ogenomskinlig tooyour app. De kan har utfärdats av Azure AD och granskas och tolkas bara av Azure AD. De är långlivade, men din app ska inte skrivas med hello förväntan som en uppdateringstoken ska gälla för en viss tidsperiod. Uppdaterings-tokens kan vara inaktuella när som helst för en mängd olika skäl. Hej sätt för din app tooknow om en uppdateringstoken är giltig är tooattempt tooredeem den genom att göra en tokenbegäran tooAzure AD.

När du lösa in en uppdateringstoken för en ny token (och om din app har beviljats hello `offline_access` omfattning), du får en ny uppdateringstoken i hello token svar. Du bör spara hello nyligen utfärdade uppdateringstoken. Den ska ersätta hello uppdateringstoken som du tidigare använt i hello-begäran. Detta hjälper garantera att uppdaterings-tokens vara giltigt så länge som möjligt.

## <a name="token-validation"></a>Token-verifiering
toovalidate en token som din app ska kontrollera både hello signatur och anspråk för hello-token.

Många bibliotek med öppen källkod är tillgängliga för att validera JWTs, beroende på ditt språk. Vi rekommenderar att du utforska alternativen i stället för att implementera din egen valideringslogik. hello informationen i den här guiden hjälper dig Lär dig hur tooproperly använda dessa bibliotek.

### <a name="validate-hello-signature"></a>Validera hello signatur
En JWT innehåller tre segment, avgränsade med hello `.` tecken. första hello-segment är hello *huvud*, hello är andra hello *brödtext*, och hello tredje hello *signatur*. hello signatur segment kan vara används toovalidate hello äkthet hello token så att den kan vara betrott av din app.

Azure AD B2C-token är signerade med branschstandard asymmetriska krypteringsalgoritmer, till exempel RSA-256. hello-huvud för hello token innehåller information om hello nyckel och krypteringsmetod används toosign hello token:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

Hej `alg` anspråk anger hello-algoritmen som har använt toosign hello-token. Hej `kid` anspråk anger hello viss offentlig nyckel som har använt toosign hello-token.

Azure AD kan logga en token med hjälp av någon av en viss uppsättning privat-offentligt nyckelpar vid en given tidpunkt. Azure AD roterar hello möjliga uppsättning nycklar med jämna mellanrum, så att din app ska skrivas toohandle dessa nyckel ändras automatiskt. En rimlig frekvens toocheck för uppdateringar toohello offentliga nycklar som används av Azure AD är 24 timmar.

Azure AD B2C har en slutpunkt för OpenID Connect metadata. Detta gör apparna toofetch information om Azure AD B2C vid körning. Informationen omfattar slutpunkter, token innehåll och nycklar för tokensignering. Din B2C-katalog innehåller en JSON-dokumentet för metadata för varje princip. Till exempel hello Metadatadokumentet för hello `b2c_1_sign_in` princip i `fabrikamb2c.onmicrosoft.com` finns på:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`är hello B2C-katalog som används för tooauthenticate hello användaren och `b2c_1_sign_in` är hello principen används tooacquire hello-token. toodetermine vilken princip du har använt toosign en token (och där toogo toofetch hello metadata), har du två alternativ. Först hello principnamn ingår i hello `acr` anspråk i hello-token. Du kan parsa anspråk utanför hello brödtext hello JWT av Base64-avkodning hello brödtext och avserialisering av hello JSON sträng som uppstår. Hej `acr` anspråk kommer att hello namnet på hello-princip som har använt tooissue hello-token.  Ett annat alternativ är tooencode hello princip i hello värdet för hello `state` parameter när du skickar hello begäran och sedan avkoda toodetermine vilken princip som har använts. Antingen metoden är giltig.

hello Metadatadokumentet är ett JSON-objekt som innehåller flera användbara uppgifter. Dessa inkluderar hello platsen för hello slutpunkter krävs tooperform OpenID Connect autentisering. De omfattar också `jwks_uri`, vilket ger hello platsen för hello uppsättning offentliga nycklar som används toosign token. Den platsen som anges här, men det är bästa toofetch hello plats dynamiskt med hjälp av hello Metadatadokumentet och parsning ut `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

hello JSON-dokumentet på URL: en innehåller alla hello offentlig nyckelinformation används vid en viss tidpunkt. Din app kan använda hello `kid` anspråk i hello JWT huvud tooselect hello offentliga nyckeln i hello JSON-dokument som har använt toosign en viss token. Den kan utföra signaturverifiering med hjälp av hello rätt offentlig nyckel och hello angivna algoritm.

En beskrivning av hur tooperform signaturverifiering är utanför hello omfånget för det här dokumentet. Många bibliotek med öppen källkod är tillgängliga toohelp du med det här om du behöver den.

### <a name="validate-hello-claims"></a>Validera hello anspråk
När din app eller API får du en ID-token, bör det också utföra flera kontroller mot hello anspråk i hello-ID-token. Dessa inkludera, men är inte begränsade till:

* Hej **målgruppen** anspråk: Detta verifierar token som hello-ID var avsedda toobe angivna tooyour app.
* Hej **inte före** och **förfallotid** anspråk: dessa kontrollerar du att hello-ID-token inte har upphört att gälla.
* Hej **utfärdaren** anspråk: Detta verifierar att hello-token har utfärdats tooyour app av Azure AD.
* Hej **temporärt ID**: Detta är en strategi för tokenrepetition attack lösning.

En fullständig lista över verifieringar som din app ska utföra finns toohello [OpenID Connect specifikationen](https://openid.net). Information om hello förväntade värden för dessa anspråk som ingår i föregående hello [token avsnittet](#types-of-tokens).  

## <a name="token-lifetimes"></a>Livslängd för token
hello följande livstid för token är angivna toofurther din vetskap. De kan hjälpa dig när du utvecklar och felsöka appar. Observera att dina appar inte får vara skrivs tooexpect någon av dessa livslängd tooremain konstant. De kan och kommer att ändras. Läs mer om hello [anpassning av token livslängd](active-directory-b2c-token-session-sso.md) i Azure AD B2C.

| Token | Livslängd | Beskrivning |
| --- | --- | --- |
| ID-token |En timme |ID-token gäller vanligtvis i en timme. Ditt webbprogram kan använda den här livstid toomaintain sin egen sessioner med användare (rekommenderas). Du kan också välja en annan session livslängd. Om din app måste tooget en ny token ID, måste den helt enkelt toomake en ny begäran om inloggning tooAzure AD. Om en användare har en giltig webbläsarsession med Azure AD kan kanske användaren inte nödvändiga tooenter autentiseringsuppgifter igen. |
| Uppdatera token |In too14 dagar |En enskild uppdateringstoken är giltig för 14 dagar. En uppdateringstoken kan dock bli ogiltiga när som helst för en rad orsaker. Din app ska fortsätta tootry toouse en uppdateringstoken förrän hello begäran misslyckas eller din app ersätter hello uppdateringstoken med en ny. En uppdateringstoken kan också blir ogiltiga om 90 dagar har gått sedan hello användaren senast angivna autentiseringsuppgifter. |
| Auktoriseringskoder |Fem minuter |Auktoriseringskoder är avsiktligt tillfällig. De bör lösas omedelbart för åtkomst-token, ID-token eller uppdateringstoken när de tas emot. |


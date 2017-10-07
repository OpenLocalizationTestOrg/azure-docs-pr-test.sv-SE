---
title: "Azure Active Directory B2C: Begär åtkomst-token | Microsoft Docs"
description: "Den här artikeln visar hur toosetup ett klientprogram och få en åtkomst-token."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: 1c75f17f-5ec5-493a-b906-f543b3b1ea66
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: parakhj
ms.openlocfilehash: 410639424fd4e5119a5d58751dfdb6e8957fd100
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: Begär åtkomst-token

En åtkomst-token (betecknas som **åtkomst\_token** i hello svar från Azure AD B2C) är en typ av säkerhetstoken som en klient kan använda tooaccess resurser som skyddas av en [auktorisering server](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-protocols#the-basics), till exempel ett webb-API. Åtkomsttoken representeras som [JWTs](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#types-of-tokens) och innehåller information om hello avsedda resursservern och hello beviljade behörigheter toohello server. När du anropar hello resursservern måste hello åtkomsttoken finnas i hello HTTP-begäran.

Den här artikeln beskrivs hur tooconfigure ett klientprogram och webb-API i ordning tooobtain en **åtkomst\_token**.

> [!NOTE]
> **Webb-API kedjor (On-Behalf-Of) stöds inte av Azure AD B2C.**
>
> Många arkitekturer har ett webb-API som behöver toocall ett annat underordnat webb-API, både skyddas av Azure AD B2C. Det här scenariot är vanligt i interna klienter som har en webb-API-serverdel, som anropar en Microsoft online service som hello Azure AD Graph API.
>
> Det här scenariot med länkade webb-API kan användas med hjälp av hello OAuth 2.0 JWT ägar autentiseringsuppgifter bevilja, annars kallas hello på-flöde. Dock implementeras hello på-flöde för närvarande inte i Azure AD B2C.

## <a name="register-a-web-api-and-publish-permissions"></a>Registrera ett webb-API och publicera behörigheter

Innan du begär en åtkomst-token du först behöver tooregister ett webb-API och publicera behörigheter (scope) som kan beviljas toohello klientprogrammet.

### <a name="register-a-web-api"></a>Registrera ett webb-API

1. Klicka på hello B2C-funktionsbladet på hello Azure-portalen, **program**.
1. Klicka på **+ Lägg till** hello överst i hello-bladet.
1. Ange en **namn** för hello-programmet som beskriver ditt program tooconsumers. Du kan till exempel ange ”Contoso-API”.
1. Växla hello **ta med webbapp / webb-API** växla för**Ja**.
1. Ange ett godtyckligt värde för hello **Reply URL: er**. Ange till exempel `https://localhost:44316/`. hello värdet spelar ingen roll eftersom en API inte bör få hello token direkt från Azure AD B2C.
1. Ange en **App-ID-URI**. Detta är hello-identifierare som används för ditt webb-API. Ange till exempel 'anteckningar' hello i rutan. Hej **App-ID URI** blir `https://{tenantName}.onmicrosoft.com/notes`.
1. Klicka på **skapa** tooregister ditt program.
1. Klicka på hello-program som du just har skapat och kopiera hello globalt unik **klient-ID för programmet** som du vill använda senare i koden.

### <a name="publishing-permissions"></a>Publishing behörigheter

Scope, som är detsamma toopermissions, krävs när din app anropar en API. ”Läsa” eller ”write” är några exempel på scope. Anta att du vill att webbplatsen eller inbyggda appen för ”läsa” från en API. Appen skulle anropa Azure AD B2C och begäran om en åtkomst-token som ger åtkomstomfånget toohello ”läsa”. För Azure AD B2C tooemit en åtkomsttoken måste hello appen toobe beviljats behörighet för ”läsa” från hello specifika API: et. toodo, din API behöver först toopublish hello ”läsa” omfång.

1. Inom hello Azure AD B2C **program** bladet öppna hello web API-program (”Contoso-API”).
1. Klicka på alternativet för **publicerade omfång**. Det är där du definierar hello behörigheter (scope) som kan beviljas tooother program.
1. Lägg till **Scope-värden** vid behov (till exempel ”läsa”). Som standard definieras hello ”user_impersonation” omfång. Du kan ignorera detta om du vill. Ange en beskrivning av hello scope i hello **scopenamn** kolumn.
1. Klicka på **Spara**.

> [!IMPORTANT]
> Hej **scopenamn** är hello beskrivning av hello **Scope-värde**. När du använder hello omfång, se till att toouse hello **Scope-värde**.

## <a name="grant-a-native-or-web-app-permissions-tooa-web-api"></a>Ge ett inbyggt eller web app behörigheter tooa webb-API

När en API är konfigurerad toopublish scope, måste klientprogrammet hello toobe beviljas sådana omfattningar via hello Azure-portalen.

1. Navigera toohello **program** -menyn i hello B2C-funktionsbladet.
1. Registrera ett klientprogram ([webbapp](active-directory-b2c-app-registration.md#register-a-web-app) eller [native client](active-directory-b2c-app-registration.md#register-a-mobile-or-native-app)) om du inte redan har en. Om du följer den här guiden startar du nu behöver du tooregister ett klientprogram.
1. Klicka på **API-åtkomst**.
1. Klicka på **Lägg till**.
1. Välj din webb-API och hello scope (behörigheter) som toogrant.
1. Klicka på **OK**.

> [!NOTE]
> Azure AD B2C ombeds du inte klienten programanvändare sitt samtycke. I stället som alla medgivande Hej administratör, baserat på hello behörigheterna konfigurerade mellan hello-program som beskrivs ovan. En bevilja behörighet för ett program har återkallats, alla användare som har tidigare kunde tooacquire behörigheten kommer inte längre att kunna toodo så.

## <a name="requesting-a-token"></a>Begära ett token

När du begär en åtkomst-token hello-klientprogrammet måste toospecify hello önskade behörigheter i hello **omfång** parameter för hello-begäran. Till exempel toospecify hello **Scope-värde** ”läsa” för hello API som har hello **App-ID URI** av `https://contoso.onmicrosoft.com/notes`, hello Omfattningen skulle vara `https://contoso.onmicrosoft.com/notes/read`. Nedan visas ett exempel på en auktorisering kod begäran toohello `/authorize` slutpunkt.

> [!NOTE]
> Anpassade domäner kan inte användas tillsammans med åtkomsttoken. Du måste använda tenantName.onmicrosoft.com domänen i hello begärd URL.

```
https://login.microsoftonline.com/<tenantName>.onmicrosoft.com/oauth2/v2.0/authorize?p=<yourPolicyId>&client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
```

tooacquire flera behörigheter i hello samma begär, du kan lägga till flera poster i hello enda **omfång** parameter, avgränsade med blanksteg. Exempel:

Avkoda URL:

```
scope=https://contoso.onmicrosoft.com/notes/read openid offline_access
```

URL-kodade:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread%20openid%20offline_access
```

Du kan begära flera scope (behörigheter) för en resurs än vad beviljas för klientprogrammet. När så är fallet hello lyckas hello anropet om minst en behörighet. hello resulterande **åtkomst\_token** har dess ”scp”-anspråk med endast hello behörigheter som har beviljats.

> [!NOTE] 
> Vi stöder inte begär behörighet mot två olika webbresurser i hello samma begäran. Den här typen av förfrågan misslyckas.

### <a name="special-cases"></a>Särskilda fall

Hej OpenID Connect standard anger flera särskilda ”omfång”-värden. Hej följa särskilda scope representerar hello behörighet för ”åtkomst till hello användarprofil”:

* **openid**: detta begär en ID-token
* **offline\_åtkomst**: detta begär en uppdateringstoken (med hjälp av [Auth kod flödar](active-directory-b2c-reference-oauth-code.md)).

Om hello `response_type` parameter i en `/authorize` begäran innehåller `token`, hello `scope` parameter måste innehålla minst en resurs omfång (andra än `openid` och `offline_access`) som kan beviljas. Annars hello `/authorize` begäran avslutas med ett fel.

## <a name="hello-returned-token"></a>hello returnerade token

I har minted **åtkomst\_token** (från antingen hello `/authorize` eller `/token` endpoint), hello följande anspråk som kommer att finnas:

| Namn | Begär | Beskrivning |
| --- | --- | --- |
|Målgrupp |`aud` |Hej **program-ID** av hello enskild resurs hello säkerhetstoken som ger åtkomst till. |
|Omfång |`scp` |hello behörigheter beviljas toohello resurs. Flera beviljade behörigheter ska avgränsas med blanksteg. |
|Auktoriserad part |`azp` |Hej **program-ID** med hello klientprogram som initierade hello-begäran. |

När din API tar emot hello **åtkomst\_token**, den måste [Validera hello token](active-directory-b2c-reference-tokens.md) tooprove hello token är giltigt och har rätt hello-anspråk.

Vi är alltid öppna toofeedback och förslag! Om du har problem med det här avsnittet inlägg på Stack Overflow använda hello taggen ['azure-ad b2c-](https://stackoverflow.com/questions/tagged/azure-ad-b2c). För funktionsbegäranden, lägga till dem för[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

## <a name="next-steps"></a>Nästa steg

* Skapa ett web API med [.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)
* Skapa ett web API med [Node.JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)

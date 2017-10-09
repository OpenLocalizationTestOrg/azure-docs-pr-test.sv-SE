---
title: aaaAzure AD B2C | Microsoft Docs
description: hello typer av program som du kan skapa i hello Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: bb9d4abe-0db7-4bd9-b0c4-2f43b2c9cf33
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: dastrock
ms.openlocfilehash: 7dd3dac781fb7e1553dd0f2d112b1956489a7dfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Typer av program
Azure AD (Active Directory Azure) B2C stöder autentisering för en rad olika moderna apparkitekturer. Alla baseras på standardprotokollen hello [OAuth 2.0](active-directory-b2c-reference-protocols.md) eller [OpenID Connect](active-directory-b2c-reference-protocols.md). Det här dokumentet beskrivs kortfattat hello typer av appar som du kan skapa, oberoende av plattform eller hello språk du föredrar. Det hjälper dig att förstå hello övergripande scenarierna innan du också [börjar utveckla program](active-directory-b2c-overview.md#get-started).

## <a name="hello-basics"></a>hello-grunderna
Alla appar som använder Azure AD B2C måste registreras i din [B2C-katalog](active-directory-b2c-get-started.md) via hello [Azure Portal](https://portal.azure.com/). hello registreringsprocessen samlar in och tilldelar några värden tooyour app:

* Ett **program-ID** som identifierar din app unikt.
* En **omdirigerings-URI** som kan vara används toodirect svar tillbaka tooyour app.
* Eventuella andra scenariespecifika värden. Mer information lär du dig hur för[registrera en app](active-directory-b2c-app-registration.md).

När hello app har registrerats kommunicerar den med Azure AD genom att skicka begäranden toohello Azure AD v2.0-slutpunkten:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Varje begäran som skickas tooAzure AD B2C anger en **princip**. En princip styr hello beteendet i Azure AD. Du kan också använda dessa slutpunkter toocreate en anpassningsbar uppsättning användarupplevelser. Exempel på vanliga principer är registrerings-, inloggnings- och profilredigeringsprinciper. Om du inte är bekant med principer bör du läsa om hello Azure AD B2C [expanderbara principramverk](active-directory-b2c-reference-policies.md) innan du fortsätter.

hello interaktionen för alla appar med en v2.0-slutpunkt följer ett liknande övergripande mönster:

1. hello appen dirigerar hello användaren toohello v2.0-slutpunkten tooexecute en [princip](active-directory-b2c-reference-policies.md).
2. hello användaren Slutför hello principen enligt principdefinitionen toohello.
3. hello appen tar emot någon typ av säkerhetstoken från hello v2.0-slutpunkten.
4. hello app använder hello token tooaccess skyddade säkerhetsinformation eller en skyddad resurs.
5. hello resursservern verifierar hello säkerhet token tooverify som åtkomst kan beviljas.
6. hello appen uppdaterar regelbundet hur hello säkerhetstoken.

<!-- TODO: Need a page for libraries toolink too-->
De här stegen kan avvika något baserat på hello typen av app som du utvecklar. Bibliotek med öppen källkod kan adressera hello information för dig.

## <a name="web-apps"></a>Webbappar
För webbappar (inklusive .NET, PHP, Java, Ruby, Python och Node.js) som finns på en server och öppnas via en webbläsare stöder Azure AD B2C [OpenID Connect](active-directory-b2c-reference-protocols.md) i alla användarmiljöer, till exempel inloggning, registrering och profilhantering. I hello Azure AD B2C-implementering av OpenID Connect initierar ditt webbprogram dessa användarupplevelser genom att utfärda autentisering begäranden tooAzure AD. hello resultatet av hello-begäran är en `id_token`. Den här säkerhetstoken representerar hello användarens identitet. Det ger också information om hello användaren i hello form av anspråk:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Mer information om hello typer av token och anspråk tillgängliga tooan app i hello [B2C tokenreferens](active-directory-b2c-reference-tokens.md).

I en webbapp utförs följande övergripande steg för varje körning av en [princip](active-directory-b2c-reference-policies.md):

![Bild i spaltformat som illustrerar ett webbappsflöde](./media/active-directory-b2c-apps/webapp.png)

Validering av hello `id_token` med hjälp av en offentlig signeringsnyckel som fås från Azure AD är tillräckligt tooverify hello hello användares identitet. Konfigurerar även en sessions-cookie som kan använda tooidentify hello användaren vid efterföljande sidförfrågningar.

toosee det här scenariot i åtgärden, försök med något av hello web app inloggning kodexempel i vår [komma igång-avsnittet](active-directory-b2c-overview.md#get-started).

I tillägg toofacilitating enkel inloggning kanske också ett webbprogram server tooaccess en backend-webbtjänst. I det här fallet hello webbappen köra ett något annorlunda [OpenID Connect-flöde](active-directory-b2c-reference-oidc.md) och hämta token genom att använda auktoriseringskoder och uppdateringstoken. Det här scenariot illustreras i följande hello [webb-API: er avsnittet](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Webb-API:er
Du kan använda Azure AD B2C toosecure webbtjänster, till exempel appens RESTful-webb-API. Webb-API: er kan använda OAuth 2.0 toosecure sina data genom att autentisera inkommande HTTP-begäranden med token. hello som anropar ett webb-API lägger till en token i hello authorization-huvud för en HTTP-begäran:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

hello webb-API kan sedan använda hello token tooverify hello API Anroparens identitet och tooextract information om hello anroparen från anspråk som kodas i hello-token. Mer information om hello typer av token och anspråk tillgängliga tooan app i hello [Azure AD B2C tokenreferens](active-directory-b2c-reference-tokens.md).

> [!NOTE]
> Azure AD B2C stöder för närvarande endast webb-API:er som används av egna välkända klienter. Din fullständiga app kan exempelvis omfatta en iOS-app, en Android-app och ett backend-webb-API. Den här arkitekturen stöds fullt ut. Ge en partnerklient, till exempel en annan iOS-app hello tooaccess samma web API inte stöds för närvarande. Alla hello komponenter i en fullständig app måste dela ett enda program-ID.
>
>

Ett webb-API kan ta emot token från många typer av klienter, inklusive webbappar, skrivbordsappar och mobilappar, appar med en enda sida, server-deamon och andra webb-API:er. Här är ett exempel på hello fullständiga flödet för en webbapp som anropar ett webb-API:

![Bild i spaltformat som illustrerar webb-API:et för en webbapp](./media/active-directory-b2c-apps/webapi.png)

toolearn mer information om auktoriseringskoder, uppdateringstoken och hello steg för att hämta token Läs mer om hello [OAuth 2.0-protokollet](active-directory-b2c-reference-oauth-code.md).

toolearn hur toosecure webb-API med hjälp av Azure AD B2C, checka ut hello web API självstudiekurser i vår [komma igång-avsnittet](active-directory-b2c-overview.md#get-started).

## <a name="mobile-and-native-apps"></a>Mobila och interna appar
Appar som är installerade på enheter, till exempel appar och program behöver ofta tooaccess backend-tjänster eller webb-API: er för användare. Du kan lägga till anpassade identity management upplevelser tooyour interna appar och på ett säkert sätt anropa backend tjänster med hjälp av Azure AD B2C och hello [OAuth 2.0-auktoriseringskodflödet](active-directory-b2c-reference-oauth-code.md).  

I det här flödet kör hello appen [principer](active-directory-b2c-reference-policies.md) och tar emot en `authorization_code` från Azure AD när hello användaren Slutför hello princip. Hej `authorization_code` representerar hello appens behörighet toocall backend-tjänster för hello användare är inloggad för tillfället. hello appen kan sedan byta hello `authorization_code` i hello bakgrunden för en `id_token` och en `refresh_token`.  hello app kan använda hello `id_token` tooauthenticate tooa backend-webb-API i HTTP-begäranden. Det kan också använda hello `refresh_token` tooget en ny `id_token` när en äldre upphör att gälla.

> [!NOTE]
> Azure AD B2C stöder för närvarande endast token som används tooaccess en Apps egen backend-webbtjänst. Din fullständiga app kan exempelvis omfatta en iOS-app, en Android-app och ett backend-webb-API. Den här arkitekturen stöds fullt ut. Så att din iOS-app tooaccess partnerwebb-API med OAuth 2.0-åtkomsttoken stöds inte för närvarande. Alla hello komponenter i en fullständig app måste dela ett enda program-ID.
>
>

![Bild i spaltformat som illustrerar en intern app](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Aktuella begränsningar
Azure AD B2C stöder för närvarande inte hello följande typer av appar, men de är på hello Översikt. 

### <a name="daemonsserver-side-apps"></a>Daemon/appar på serversidan
Appar som innehåller tidskrävande processer eller som fungerar utan hello närvaron av en användare måste också tooaccess skyddade resurser, till exempel web API: er. De här apparna kan autentisera och hämta token genom att använda hello appens identitet (i stället för en användares delegerade identitet) och med hjälp av hello OAuth 2.0-klientautentiseringsuppgifter.

För närvarande stöds inte detta flöde av Azure AD B2C. De här apparna kan bara hämta token efter ett interaktivt användarflöde.

### <a name="web-api-chains-on-behalf-of-flow"></a>Webb-API-länkar (On-Behalf-Of-flöde)
Många arkitekturer har ett webb-API som behöver toocall ett annat underordnat webb-API, där både skyddas av Azure AD B2C. Det här scenariot är vanligt i interna klienter som har ett webb-API på serversidan. Detta anropar sedan en Microsoft online service som hello Azure AD Graph API.

Det här scenariot med länkade webb-API kan användas med hjälp av hello OAuth 2.0 JWT-ägarautentiseringsuppgifter, även kallat hello på-flöde.  Dock implementeras hello på-flöde för närvarande inte i hello Azure AD B2C.

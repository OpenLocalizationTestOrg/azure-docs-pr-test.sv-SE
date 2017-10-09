---
title: "aaaApp typer för hello Azure Active Directory v2.0-slutpunkten | Microsoft Docs"
description: "hello typer av appar och scenarier som stöds av hello Azure Active Directory v2.0-slutpunkten."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: db95c88d6865abac8ce80378ccd6b63cb38e0a01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="app-types-for-hello-azure-active-directory-v20-endpoint"></a>Apptyper för hello Azure Active Directory v2.0-slutpunkten
hello Azure Active Directory (AD Azure) v2.0-slutpunkten stöder autentisering för en rad olika moderna apparkitekturer alla baserat på branschstandardprotokollen [OAuth 2.0- eller OpenID Connect](active-directory-v2-protocols.md). Den här artikeln beskriver hello typer av appar som du kan skapa med hjälp av Azure AD v2.0, oavsett din önskat språk eller en plattform. hello informationen i den här artikeln är utformad toohelp du förstår övergripande scenarierna innan du [börjar arbeta med hello kod](active-directory-appmodel-v2-overview.md#getting-started).

> [!NOTE]
> hello v2.0-slutpunkten stöder inte alla Azure Active Directory-scenarier och funktioner. toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

## <a name="hello-basics"></a>hello-grunderna
Du måste registrera varje app som använder hello v2.0-slutpunkten i hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com). hello registreringsprocessen samlar in och tilldelar dessa värden för din app:

* En **program-ID** som unikt identifierar din app
* En **omdirigerings-URI** som du kan använda toodirect svar tillbaka tooyour app
* Några andra scenariespecifika värden

Mer information lär du dig hur för[registrera en app](active-directory-v2-app-registration.md).

När hello app har registrerats kommunicerar hello app med Azure AD genom att skicka begäranden toohello Azure AD v2.0-slutpunkten. Vi ger öppen källkod ramverk och bibliotek som ska hantera hello information om dessa begäranden. Du har också hello alternativet tooimplement hello autentiseringslogiken själv genom att skapa begäranden toothese slutpunkter:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries toolink too-->

## <a name="web-apps"></a>Webbappar
Du kan använda för webbprogram (.NET, PHP, Java, Ruby, Python, nod) som hello användare har åtkomst via en webbläsare, [OpenID Connect](active-directory-v2-protocols.md) för användarinloggning. Tar emot en ID-token i OpenID Connect hello webbprogrammet. Ett ID-token är en säkerhetstoken som verifierar hello användarens identitet och innehåller information om hello användare i hello form av anspråk:

```
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Du kan lära dig om alla hello typer av token och anspråk som är tillgängliga tooan app i hello [v2.0 tokens referens](active-directory-v2-tokens.md).

I web server apps utförs hello inloggning autentiseringsflödet följande övergripande steg:

![Autentiseringsflödet för Web app](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Du kan kontrollera hello användarens identitet genom att verifiera hello-ID-token med en offentlig signeringsnyckel som tas emot från hello v2.0-slutpunkten. En sessionscookie har angetts som kan vara används tooidentify hello användaren vid efterföljande sidförfrågningar.

exempel på toosee det här scenariot i åtgärden, försök med något av hello web app inloggning-kod i vår v2.0 [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.

Dessutom toosimple inloggning, en web server-app kan behöva tooaccess en annan webbtjänst, till exempel en REST-API. I det här fallet hello server webbprogrammet bedriver ett kombinerade OpenID Connect och OAuth 2.0-flöde med hjälp av hello [OAuth 2.0-auktoriseringskodflödet](active-directory-v2-protocols.md). Mer information om det här scenariot finns i avsnittet om [komma igång med webbappar och webb-API: er](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Webb-API:er
Du kan använda hello v2.0-slutpunkten toosecure webbtjänster, till exempel appens RESTful webb-API. I stället för ID-token och sessionscookies webb-API använder en OAuth 2.0 åtkomst-token toosecure dess data och tooauthenticate inkommande begäranden. hello som anropar ett webb-API lägger till en åtkomst-token i hello authorization-huvud för en HTTP-begäran, så här:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

hello Web API använder hello åtkomst-token tooverify hello API-Anroparens identitet och tooextract information om hello anroparen från anspråk som kodas i hello åtkomsttoken. toolearn om alla hello typer av token och anspråk som är tillgängliga tooan appen finns hello [v2.0 tokens referens](active-directory-v2-tokens.md).

En webb-API kan ge användare hello power tooopt i eller välja bort funktioner eller data genom att exponera behörigheter, även kallat [scope](active-directory-v2-scopes.md). För ett anropa app tooacquire behörighet tooa omfång godkänner hello användaren toohello omfång under ett flöde. hello v2.0-slutpunkten ställer hello användaren om behörighet och registrerar behörigheter i alla åtkomsttoken som hello Web API tar emot. hello Web API verifierar hello åtkomst-token tas emot på varje anrop och kontrollerar auktorisering.

En webb-API kan få åtkomst-token från alla typer av appar, inklusive server webbprogram, skrivbord och mobila appar, sida appar, server-deamon och även andra webb-API: er. hello övergripande flödet för en webb-API som ser ut så här:

![Autentiseringsflödet för webb-API](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

toolearn hur toosecure webb-API med hjälp av checka ut hello Web API-koden i åtkomsttoken OAuth2 exempel i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.

I många fall måste också web API: er toomake utgående förfrågningar tooother nedströms webb-API: er som skyddas av Azure Active Directory.  toodo så web API: er kan dra nytta av Azure AD **på uppdrag av** flöde, vilket gör att hello webb-API tooexchange en inkommande åtkomst-token för en annan åtkomst-token toobe används i utgående förfrågningar.  hello v2.0 slutpunktens uppdrag flödet beskrivs i [detaljerat här](active-directory-v2-protocols-oauth-on-behalf-of.md).

## <a name="mobile-and-native-apps"></a>Mobila och interna appar
Enheten installerade appar, till exempel appar och program behöver ofta tooaccess backend-tjänster eller webb-API: er som lagrar data och utföra funktioner för en användares räkning. De här apparna kan lägga till inloggning och auktorisering tooback tjänster med hjälp av hello [OAuth 2.0-auktoriseringskodflödet](active-directory-v2-protocols-oauth-code.md).

I det här flödet hello appen tar emot en Auktoriseringskoden från hello v2.0-slutpunkten när hello användare loggar in. hello auktorisering kod representerar hello appens behörighet toocall backend-tjänster för hello användare är inloggad. hello app kan utbyta hello auktoriseringskod i hello bakgrunden för en OAuth 2.0-åtkomsttoken och en uppdateringstoken. hello app kan använda hello åtkomst-token tooauthenticate tooWeb API: er i HTTP-förfrågningar och använda hello uppdatera token tooget ny åtkomsttoken när äldre åtkomst-token upphör att gälla.

![Autentiseringsflödet för inbyggda appen](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Single-page-appar (JavaScript)
Många moderna appar innehåller en enda sida app-klientdel som främst är skriven i JavaScript. Ofta skrivs med hjälp av ett ramverk som AngularJS, Ember.js eller Durandal.js. hello Azure AD v2.0-slutpunkten har stöd för dessa appar med hjälp av hello [implicita flödet för OAuth 2.0](active-directory-v2-protocols-implicit.md).

I det här flödet hello appen tar emot token direkt från hello v2.0 auktorisera slutpunkt, utan några server-till-server-utbyte. Alla autentiseringslogiken och session hantering tar placera helt i hello JavaScript-klienten utan extra sidomdirigeringar.

![Implicit autentiseringsflödet](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

toosee det här scenariot i åtgärden, försök med något av hello sida app kodexempel i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.

## <a name="daemons-and-server-side-apps"></a>Daemon och serversidan appar
Appar som har tidskrävande processer eller som fungerar utan interaktion med användaren måste också tooaccess skyddade resurser, till exempel webb-API: er. De här apparna kan autentisera och hämta token genom att använda hello appens identitet i stället en användares delegerade identitet med hello OAuth 2.0-klientautentiseringsuppgifter.

I det här flödet hello app interagerar direkt med hello `/token` endpoint tooobtain slutpunkter:

![Daemon för app-flöde för autentisering](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

toobuild en daemon app dokumentationen hello klienten autentiseringsuppgifter i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnittet eller försök med en [.NET sample-appen](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

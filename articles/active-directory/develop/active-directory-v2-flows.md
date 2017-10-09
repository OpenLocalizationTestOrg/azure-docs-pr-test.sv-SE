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
# <a name="app-types-for-hello-azure-active-directory-v20-endpoint"></a><span data-ttu-id="33ddd-103">Apptyper för hello Azure Active Directory v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="33ddd-103">App types for hello Azure Active Directory v2.0 endpoint</span></span>
<span data-ttu-id="33ddd-104">hello Azure Active Directory (AD Azure) v2.0-slutpunkten stöder autentisering för en rad olika moderna apparkitekturer alla baserat på branschstandardprotokollen [OAuth 2.0- eller OpenID Connect](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-104">hello Azure Active Directory (Azure AD) v2.0 endpoint supports authentication for a variety of modern app architectures, all of them based on industry-standard protocols [OAuth 2.0 or OpenID Connect](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="33ddd-105">Den här artikeln beskriver hello typer av appar som du kan skapa med hjälp av Azure AD v2.0, oavsett din önskat språk eller en plattform.</span><span class="sxs-lookup"><span data-stu-id="33ddd-105">This article describes hello types of apps that you can build by using Azure AD v2.0, regardless of your preferred language or platform.</span></span> <span data-ttu-id="33ddd-106">hello informationen i den här artikeln är utformad toohelp du förstår övergripande scenarierna innan du [börjar arbeta med hello kod](active-directory-appmodel-v2-overview.md#getting-started).</span><span class="sxs-lookup"><span data-stu-id="33ddd-106">hello information in this article is designed toohelp you understand high-level scenarios before you [start working with hello code](active-directory-appmodel-v2-overview.md#getting-started).</span></span>

> [!NOTE]
> <span data-ttu-id="33ddd-107">hello v2.0-slutpunkten stöder inte alla Azure Active Directory-scenarier och funktioner.</span><span class="sxs-lookup"><span data-stu-id="33ddd-107">hello v2.0 endpoint doesn't support all Azure Active Directory scenarios and features.</span></span> <span data-ttu-id="33ddd-108">toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-108">toodetermine whether you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="hello-basics"></a><span data-ttu-id="33ddd-109">hello-grunderna</span><span class="sxs-lookup"><span data-stu-id="33ddd-109">hello basics</span></span>
<span data-ttu-id="33ddd-110">Du måste registrera varje app som använder hello v2.0-slutpunkten i hello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="33ddd-110">You must register each app that uses hello v2.0 endpoint in hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="33ddd-111">hello registreringsprocessen samlar in och tilldelar dessa värden för din app:</span><span class="sxs-lookup"><span data-stu-id="33ddd-111">hello app registration process collects and assigns these values for your app:</span></span>

* <span data-ttu-id="33ddd-112">En **program-ID** som unikt identifierar din app</span><span class="sxs-lookup"><span data-stu-id="33ddd-112">An **Application ID** that uniquely identifies your app</span></span>
* <span data-ttu-id="33ddd-113">En **omdirigerings-URI** som du kan använda toodirect svar tillbaka tooyour app</span><span class="sxs-lookup"><span data-stu-id="33ddd-113">A **Redirect URI** that you can use toodirect responses back tooyour app</span></span>
* <span data-ttu-id="33ddd-114">Några andra scenariespecifika värden</span><span class="sxs-lookup"><span data-stu-id="33ddd-114">A few other scenario-specific values</span></span>

<span data-ttu-id="33ddd-115">Mer information lär du dig hur för[registrera en app](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-115">For details, learn how too[register an app](active-directory-v2-app-registration.md).</span></span>

<span data-ttu-id="33ddd-116">När hello app har registrerats kommunicerar hello app med Azure AD genom att skicka begäranden toohello Azure AD v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="33ddd-116">After hello app is registered, hello app communicates with Azure AD by sending requests toohello Azure AD v2.0 endpoint.</span></span> <span data-ttu-id="33ddd-117">Vi ger öppen källkod ramverk och bibliotek som ska hantera hello information om dessa begäranden.</span><span class="sxs-lookup"><span data-stu-id="33ddd-117">We provide open-source frameworks and libraries that handle hello details of these requests.</span></span> <span data-ttu-id="33ddd-118">Du har också hello alternativet tooimplement hello autentiseringslogiken själv genom att skapa begäranden toothese slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="33ddd-118">You also have hello option tooimplement hello authentication logic yourself by creating requests toothese endpoints:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries toolink too-->

## <a name="web-apps"></a><span data-ttu-id="33ddd-119">Webbappar</span><span class="sxs-lookup"><span data-stu-id="33ddd-119">Web apps</span></span>
<span data-ttu-id="33ddd-120">Du kan använda för webbprogram (.NET, PHP, Java, Ruby, Python, nod) som hello användare har åtkomst via en webbläsare, [OpenID Connect](active-directory-v2-protocols.md) för användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="33ddd-120">For web apps (.NET, PHP, Java, Ruby, Python, Node) that hello user accesses through a browser, you can use [OpenID Connect](active-directory-v2-protocols.md) for user sign-in.</span></span> <span data-ttu-id="33ddd-121">Tar emot en ID-token i OpenID Connect hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="33ddd-121">In OpenID Connect, hello web app receives an ID token.</span></span> <span data-ttu-id="33ddd-122">Ett ID-token är en säkerhetstoken som verifierar hello användarens identitet och innehåller information om hello användare i hello form av anspråk:</span><span class="sxs-lookup"><span data-stu-id="33ddd-122">An ID token is a security token that verifies hello user's identity and provides information about hello user in hello form of claims:</span></span>

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

<span data-ttu-id="33ddd-123">Du kan lära dig om alla hello typer av token och anspråk som är tillgängliga tooan app i hello [v2.0 tokens referens](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-123">You can learn about all hello types of tokens and claims that are available tooan app in hello [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="33ddd-124">I web server apps utförs hello inloggning autentiseringsflödet följande övergripande steg:</span><span class="sxs-lookup"><span data-stu-id="33ddd-124">In web server apps, hello sign-in authentication flow takes these high-level steps:</span></span>

![Autentiseringsflödet för Web app](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

<span data-ttu-id="33ddd-126">Du kan kontrollera hello användarens identitet genom att verifiera hello-ID-token med en offentlig signeringsnyckel som tas emot från hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="33ddd-126">You can ensure hello user's identity by validating hello ID token with a public signing key that is received from hello v2.0 endpoint.</span></span> <span data-ttu-id="33ddd-127">En sessionscookie har angetts som kan vara används tooidentify hello användaren vid efterföljande sidförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="33ddd-127">A session cookie is set, which can be used tooidentify hello user on subsequent page requests.</span></span>

<span data-ttu-id="33ddd-128">exempel på toosee det här scenariot i åtgärden, försök med något av hello web app inloggning-kod i vår v2.0 [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="33ddd-128">toosee this scenario in action, try one of hello web app sign-in code samples in our v2.0 [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="33ddd-129">Dessutom toosimple inloggning, en web server-app kan behöva tooaccess en annan webbtjänst, till exempel en REST-API.</span><span class="sxs-lookup"><span data-stu-id="33ddd-129">In addition toosimple sign-in, a web server app might need tooaccess another web service, such as a REST API.</span></span> <span data-ttu-id="33ddd-130">I det här fallet hello server webbprogrammet bedriver ett kombinerade OpenID Connect och OAuth 2.0-flöde med hjälp av hello [OAuth 2.0-auktoriseringskodflödet](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-130">In this case, hello web server app engages in a combined OpenID Connect and OAuth 2.0 flow, by using hello [OAuth 2.0 authorization code flow](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="33ddd-131">Mer information om det här scenariot finns i avsnittet om [komma igång med webbappar och webb-API: er](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-131">For more information about this scenario, read about [getting started with web apps and Web APIs](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span></span>

## <a name="web-apis"></a><span data-ttu-id="33ddd-132">Webb-API:er</span><span class="sxs-lookup"><span data-stu-id="33ddd-132">Web APIs</span></span>
<span data-ttu-id="33ddd-133">Du kan använda hello v2.0-slutpunkten toosecure webbtjänster, till exempel appens RESTful webb-API.</span><span class="sxs-lookup"><span data-stu-id="33ddd-133">You can use hello v2.0 endpoint toosecure web services, such as your app's RESTful Web API.</span></span> <span data-ttu-id="33ddd-134">I stället för ID-token och sessionscookies webb-API använder en OAuth 2.0 åtkomst-token toosecure dess data och tooauthenticate inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="33ddd-134">Instead of ID tokens and session cookies, a Web API uses an OAuth 2.0 access token toosecure its data and tooauthenticate incoming requests.</span></span> <span data-ttu-id="33ddd-135">hello som anropar ett webb-API lägger till en åtkomst-token i hello authorization-huvud för en HTTP-begäran, så här:</span><span class="sxs-lookup"><span data-stu-id="33ddd-135">hello caller of a Web API appends an access token in hello authorization header of an HTTP request, like this:</span></span>

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

<span data-ttu-id="33ddd-136">hello Web API använder hello åtkomst-token tooverify hello API-Anroparens identitet och tooextract information om hello anroparen från anspråk som kodas i hello åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="33ddd-136">hello Web API uses hello access token tooverify hello API caller's identity and tooextract information about hello caller from claims that are encoded in hello access token.</span></span> <span data-ttu-id="33ddd-137">toolearn om alla hello typer av token och anspråk som är tillgängliga tooan appen finns hello [v2.0 tokens referens](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-137">toolearn about all hello types of tokens and claims that are available tooan app, see hello [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="33ddd-138">En webb-API kan ge användare hello power tooopt i eller välja bort funktioner eller data genom att exponera behörigheter, även kallat [scope](active-directory-v2-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-138">A Web API can give users hello power tooopt in or opt out of specific functionality or data by exposing permissions, also known as [scopes](active-directory-v2-scopes.md).</span></span> <span data-ttu-id="33ddd-139">För ett anropa app tooacquire behörighet tooa omfång godkänner hello användaren toohello omfång under ett flöde.</span><span class="sxs-lookup"><span data-stu-id="33ddd-139">For a calling app tooacquire permission tooa scope, hello user must consent toohello scope during a flow.</span></span> <span data-ttu-id="33ddd-140">hello v2.0-slutpunkten ställer hello användaren om behörighet och registrerar behörigheter i alla åtkomsttoken som hello Web API tar emot.</span><span class="sxs-lookup"><span data-stu-id="33ddd-140">hello v2.0 endpoint asks hello user for permission, and then records permissions in all access tokens that hello Web API receives.</span></span> <span data-ttu-id="33ddd-141">hello Web API verifierar hello åtkomst-token tas emot på varje anrop och kontrollerar auktorisering.</span><span class="sxs-lookup"><span data-stu-id="33ddd-141">hello Web API validates hello access tokens it receives on each call and performs authorization checks.</span></span>

<span data-ttu-id="33ddd-142">En webb-API kan få åtkomst-token från alla typer av appar, inklusive server webbprogram, skrivbord och mobila appar, sida appar, server-deamon och även andra webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="33ddd-142">A Web API can receive access tokens from all types of apps, including web server apps, desktop and mobile apps, single-page apps, server-side daemons, and even other Web APIs.</span></span> <span data-ttu-id="33ddd-143">hello övergripande flödet för en webb-API som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="33ddd-143">hello high-level flow for a Web API looks like this:</span></span>

![Autentiseringsflödet för webb-API](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

<span data-ttu-id="33ddd-145">toolearn hur toosecure webb-API med hjälp av checka ut hello Web API-koden i åtkomsttoken OAuth2 exempel i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="33ddd-145">toolearn how toosecure a Web API by using OAuth2 access tokens, check out hello Web API code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="33ddd-146">I många fall måste också web API: er toomake utgående förfrågningar tooother nedströms webb-API: er som skyddas av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="33ddd-146">In many cases, web APIs also need toomake outbound requests tooother downstream web APIs secured by Azure Active Directory.</span></span>  <span data-ttu-id="33ddd-147">toodo så web API: er kan dra nytta av Azure AD **på uppdrag av** flöde, vilket gör att hello webb-API tooexchange en inkommande åtkomst-token för en annan åtkomst-token toobe används i utgående förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="33ddd-147">toodo so, web APIs can take advantage of Azure AD's **On Behalf Of** flow, which allows hello web API tooexchange an incoming access token for another access token toobe used in outbound requests.</span></span>  <span data-ttu-id="33ddd-148">hello v2.0 slutpunktens uppdrag flödet beskrivs i [detaljerat här](active-directory-v2-protocols-oauth-on-behalf-of.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-148">hello v2.0 endpoint's On Behalf Of flow is described in [detail here](active-directory-v2-protocols-oauth-on-behalf-of.md).</span></span>

## <a name="mobile-and-native-apps"></a><span data-ttu-id="33ddd-149">Mobila och interna appar</span><span class="sxs-lookup"><span data-stu-id="33ddd-149">Mobile and native apps</span></span>
<span data-ttu-id="33ddd-150">Enheten installerade appar, till exempel appar och program behöver ofta tooaccess backend-tjänster eller webb-API: er som lagrar data och utföra funktioner för en användares räkning.</span><span class="sxs-lookup"><span data-stu-id="33ddd-150">Device-installed apps, such as mobile and desktop apps, often need tooaccess back-end services or Web APIs that store data and perform functions on behalf of a user.</span></span> <span data-ttu-id="33ddd-151">De här apparna kan lägga till inloggning och auktorisering tooback tjänster med hjälp av hello [OAuth 2.0-auktoriseringskodflödet](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-151">These apps can add sign-in and authorization tooback-end services by using hello [OAuth 2.0 authorization code flow](active-directory-v2-protocols-oauth-code.md).</span></span>

<span data-ttu-id="33ddd-152">I det här flödet hello appen tar emot en Auktoriseringskoden från hello v2.0-slutpunkten när hello användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="33ddd-152">In this flow, hello app receives an authorization code from hello v2.0 endpoint when hello user signs in.</span></span> <span data-ttu-id="33ddd-153">hello auktorisering kod representerar hello appens behörighet toocall backend-tjänster för hello användare är inloggad.</span><span class="sxs-lookup"><span data-stu-id="33ddd-153">hello authorization code represents hello app's permission toocall back-end services on behalf of hello user who is signed in.</span></span> <span data-ttu-id="33ddd-154">hello app kan utbyta hello auktoriseringskod i hello bakgrunden för en OAuth 2.0-åtkomsttoken och en uppdateringstoken.</span><span class="sxs-lookup"><span data-stu-id="33ddd-154">hello app can exchange hello authorization code in hello background for an OAuth 2.0 access token and a refresh token.</span></span> <span data-ttu-id="33ddd-155">hello app kan använda hello åtkomst-token tooauthenticate tooWeb API: er i HTTP-förfrågningar och använda hello uppdatera token tooget ny åtkomsttoken när äldre åtkomst-token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="33ddd-155">hello app can use hello access token tooauthenticate tooWeb APIs in HTTP requests, and use hello refresh token tooget new access tokens when older access tokens expire.</span></span>

![Autentiseringsflödet för inbyggda appen](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a><span data-ttu-id="33ddd-157">Single-page-appar (JavaScript)</span><span class="sxs-lookup"><span data-stu-id="33ddd-157">Single-page apps (JavaScript)</span></span>
<span data-ttu-id="33ddd-158">Många moderna appar innehåller en enda sida app-klientdel som främst är skriven i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="33ddd-158">Many modern apps have a single-page app front end that primarily is written in JavaScript.</span></span> <span data-ttu-id="33ddd-159">Ofta skrivs med hjälp av ett ramverk som AngularJS, Ember.js eller Durandal.js.</span><span class="sxs-lookup"><span data-stu-id="33ddd-159">Often, it's written by using a framework like AngularJS, Ember.js, or Durandal.js.</span></span> <span data-ttu-id="33ddd-160">hello Azure AD v2.0-slutpunkten har stöd för dessa appar med hjälp av hello [implicita flödet för OAuth 2.0](active-directory-v2-protocols-implicit.md).</span><span class="sxs-lookup"><span data-stu-id="33ddd-160">hello Azure AD v2.0 endpoint supports these apps by using hello [OAuth 2.0 implicit flow](active-directory-v2-protocols-implicit.md).</span></span>

<span data-ttu-id="33ddd-161">I det här flödet hello appen tar emot token direkt från hello v2.0 auktorisera slutpunkt, utan några server-till-server-utbyte.</span><span class="sxs-lookup"><span data-stu-id="33ddd-161">In this flow, hello app receives tokens directly from hello v2.0 authorize endpoint, without any server-to-server exchanges.</span></span> <span data-ttu-id="33ddd-162">Alla autentiseringslogiken och session hantering tar placera helt i hello JavaScript-klienten utan extra sidomdirigeringar.</span><span class="sxs-lookup"><span data-stu-id="33ddd-162">All authentication logic and session handling takes place entirely in hello JavaScript client, without extra page redirects.</span></span>

![Implicit autentiseringsflödet](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

<span data-ttu-id="33ddd-164">toosee det här scenariot i åtgärden, försök med något av hello sida app kodexempel i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="33ddd-164">toosee this scenario in action, try one of hello single-page app code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

## <a name="daemons-and-server-side-apps"></a><span data-ttu-id="33ddd-165">Daemon och serversidan appar</span><span class="sxs-lookup"><span data-stu-id="33ddd-165">Daemons and server-side apps</span></span>
<span data-ttu-id="33ddd-166">Appar som har tidskrävande processer eller som fungerar utan interaktion med användaren måste också tooaccess skyddade resurser, till exempel webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="33ddd-166">Apps that have long-running processes or that operate without interaction with a user also need a way tooaccess secured resources, such as Web APIs.</span></span> <span data-ttu-id="33ddd-167">De här apparna kan autentisera och hämta token genom att använda hello appens identitet i stället en användares delegerade identitet med hello OAuth 2.0-klientautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="33ddd-167">These apps can authenticate and get tokens by using hello app's identity, rather than a user's delegated identity, with hello OAuth 2.0 client credentials flow.</span></span>

<span data-ttu-id="33ddd-168">I det här flödet hello app interagerar direkt med hello `/token` endpoint tooobtain slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="33ddd-168">In this flow, hello app interacts directly with hello `/token` endpoint tooobtain endpoints:</span></span>

![Daemon för app-flöde för autentisering](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

<span data-ttu-id="33ddd-170">toobuild en daemon app dokumentationen hello klienten autentiseringsuppgifter i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnittet eller försök med en [.NET sample-appen](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span><span class="sxs-lookup"><span data-stu-id="33ddd-170">toobuild a daemon app, see hello client credentials documentation in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section, or try a [.NET sample app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span></span>

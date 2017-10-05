---
title: "Apptyper för Azure Active Directory v2.0-slutpunkten | Microsoft Docs"
description: "Typer av appar och scenarier som stöds av Azure Active Directory v2.0-slutpunkten."
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
ms.openlocfilehash: 9d59e7f0e8f326c40be86e199d7712f6c565cc13
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="app-types-for-the-azure-active-directory-v20-endpoint"></a><span data-ttu-id="da698-103">Apptyper för Azure Active Directory v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="da698-103">App types for the Azure Active Directory v2.0 endpoint</span></span>
<span data-ttu-id="da698-104">Azure Active Directory (AD Azure) v2.0-slutpunkten stöder autentisering för en rad olika moderna apparkitekturer alla baserat på branschstandardprotokollen [OAuth 2.0- eller OpenID Connect](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="da698-104">The Azure Active Directory (Azure AD) v2.0 endpoint supports authentication for a variety of modern app architectures, all of them based on industry-standard protocols [OAuth 2.0 or OpenID Connect](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="da698-105">Den här artikeln beskriver typerna av appar som du kan skapa med hjälp av Azure AD v2.0, oavsett din önskat språk eller en plattform.</span><span class="sxs-lookup"><span data-stu-id="da698-105">This article describes the types of apps that you can build by using Azure AD v2.0, regardless of your preferred language or platform.</span></span> <span data-ttu-id="da698-106">Informationen i den här artikeln är utformat för att hjälpa dig att förstå övergripande scenarierna innan du [börjar arbeta med kod](active-directory-appmodel-v2-overview.md#getting-started).</span><span class="sxs-lookup"><span data-stu-id="da698-106">The information in this article is designed to help you understand high-level scenarios before you [start working with the code](active-directory-appmodel-v2-overview.md#getting-started).</span></span>

> [!NOTE]
> <span data-ttu-id="da698-107">V2.0-slutpunkten stöder inte alla Azure Active Directory-scenarier och funktioner.</span><span class="sxs-lookup"><span data-stu-id="da698-107">The v2.0 endpoint doesn't support all Azure Active Directory scenarios and features.</span></span> <span data-ttu-id="da698-108">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="da698-108">To determine whether you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="the-basics"></a><span data-ttu-id="da698-109">Grunderna</span><span class="sxs-lookup"><span data-stu-id="da698-109">The basics</span></span>
<span data-ttu-id="da698-110">Du måste registrera varje app som använder v2.0-slutpunkten i den [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="da698-110">You must register each app that uses the v2.0 endpoint in the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="da698-111">Registreringsprocessen samlar in och tilldelar dessa värden för din app:</span><span class="sxs-lookup"><span data-stu-id="da698-111">The app registration process collects and assigns these values for your app:</span></span>

* <span data-ttu-id="da698-112">En **program-ID** som unikt identifierar din app</span><span class="sxs-lookup"><span data-stu-id="da698-112">An **Application ID** that uniquely identifies your app</span></span>
* <span data-ttu-id="da698-113">En **omdirigerings-URI** som du kan använda för att dirigera svar tillbaka till din app</span><span class="sxs-lookup"><span data-stu-id="da698-113">A **Redirect URI** that you can use to direct responses back to your app</span></span>
* <span data-ttu-id="da698-114">Några andra scenariespecifika värden</span><span class="sxs-lookup"><span data-stu-id="da698-114">A few other scenario-specific values</span></span>

<span data-ttu-id="da698-115">Mer information lär du dig hur du [registrera en app](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="da698-115">For details, learn how to [register an app](active-directory-v2-app-registration.md).</span></span>

<span data-ttu-id="da698-116">När appen har registrerats kommunicerar appen med Azure AD genom att skicka förfrågningar till Azure AD v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="da698-116">After the app is registered, the app communicates with Azure AD by sending requests to the Azure AD v2.0 endpoint.</span></span> <span data-ttu-id="da698-117">Vi ger öppen källkod ramverk och bibliotek som ska hantera information om dessa begäranden.</span><span class="sxs-lookup"><span data-stu-id="da698-117">We provide open-source frameworks and libraries that handle the details of these requests.</span></span> <span data-ttu-id="da698-118">Du har också möjlighet att implementera autentiseringslogiken själv genom att skapa begäranden till dessa slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="da698-118">You also have the option to implement the authentication logic yourself by creating requests to these endpoints:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a><span data-ttu-id="da698-119">Webbappar</span><span class="sxs-lookup"><span data-stu-id="da698-119">Web apps</span></span>
<span data-ttu-id="da698-120">Du kan använda för webbprogram (.NET, PHP, Java, Ruby, Python, nod) som användaren kommer åt via en webbläsare, [OpenID Connect](active-directory-v2-protocols.md) för användarinloggning.</span><span class="sxs-lookup"><span data-stu-id="da698-120">For web apps (.NET, PHP, Java, Ruby, Python, Node) that the user accesses through a browser, you can use [OpenID Connect](active-directory-v2-protocols.md) for user sign-in.</span></span> <span data-ttu-id="da698-121">Tar emot en ID-token i OpenID Connect webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="da698-121">In OpenID Connect, the web app receives an ID token.</span></span> <span data-ttu-id="da698-122">Ett ID-token är en säkerhetstoken som verifierar användarens identitet och innehåller information om användaren i form av anspråk:</span><span class="sxs-lookup"><span data-stu-id="da698-122">An ID token is a security token that verifies the user's identity and provides information about the user in the form of claims:</span></span>

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

<span data-ttu-id="da698-123">Du kan lära dig om alla typer av token och anspråk som är tillgängliga för en app i den [v2.0 tokens referens](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="da698-123">You can learn about all the types of tokens and claims that are available to an app in the [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="da698-124">I web server apps utförs inloggning autentiseringsflödet följande övergripande steg:</span><span class="sxs-lookup"><span data-stu-id="da698-124">In web server apps, the sign-in authentication flow takes these high-level steps:</span></span>

![Autentiseringsflödet för Web app](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

<span data-ttu-id="da698-126">Du kan kontrollera användarens identitet genom att verifiera ID-token med en offentlig signeringsnyckel som tas emot från v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="da698-126">You can ensure the user's identity by validating the ID token with a public signing key that is received from the v2.0 endpoint.</span></span> <span data-ttu-id="da698-127">En sessionscookie har angetts som kan användas för att identifiera användaren vid efterföljande sidförfrågningar.</span><span class="sxs-lookup"><span data-stu-id="da698-127">A session cookie is set, which can be used to identify the user on subsequent page requests.</span></span>

<span data-ttu-id="da698-128">Om du vill se det här scenariot fungerar i praktiken provar du något av web app inloggning kodexemplen i vår v2.0 [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="da698-128">To see this scenario in action, try one of the web app sign-in code samples in our v2.0 [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="da698-129">Förutom enkel inloggning behöver ett webbprogram server komma åt en annan webbtjänst, till exempel en REST-API.</span><span class="sxs-lookup"><span data-stu-id="da698-129">In addition to simple sign-in, a web server app might need to access another web service, such as a REST API.</span></span> <span data-ttu-id="da698-130">I det här fallet server webbprogrammet bedriver ett kombinerade OpenID Connect och OAuth 2.0-flöde med hjälp av den [OAuth 2.0-auktoriseringskodflödet](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="da698-130">In this case, the web server app engages in a combined OpenID Connect and OAuth 2.0 flow, by using the [OAuth 2.0 authorization code flow](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="da698-131">Mer information om det här scenariot finns i avsnittet om [komma igång med webbappar och webb-API: er](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="da698-131">For more information about this scenario, read about [getting started with web apps and Web APIs](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span></span>

## <a name="web-apis"></a><span data-ttu-id="da698-132">Webb-API:er</span><span class="sxs-lookup"><span data-stu-id="da698-132">Web APIs</span></span>
<span data-ttu-id="da698-133">Du kan använda v2.0-slutpunkten för att skydda webbtjänster, till exempel appens RESTful webb-API.</span><span class="sxs-lookup"><span data-stu-id="da698-133">You can use the v2.0 endpoint to secure web services, such as your app's RESTful Web API.</span></span> <span data-ttu-id="da698-134">I stället för ID-token och sessionscookies använder webb-API en OAuth 2.0-åtkomsttoken att skydda data och autentisera inkommande begäranden.</span><span class="sxs-lookup"><span data-stu-id="da698-134">Instead of ID tokens and session cookies, a Web API uses an OAuth 2.0 access token to secure its data and to authenticate incoming requests.</span></span> <span data-ttu-id="da698-135">Anroparen av en webb-API lägger till en åtkomst-token i auktoriseringshuvudet för en HTTP-begäran, så här:</span><span class="sxs-lookup"><span data-stu-id="da698-135">The caller of a Web API appends an access token in the authorization header of an HTTP request, like this:</span></span>

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

<span data-ttu-id="da698-136">Webb-API använder åtkomsttoken att verifiera API-Anroparens identitet och att extrahera information om anroparen från anspråk som kodas i åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="da698-136">The Web API uses the access token to verify the API caller's identity and to extract information about the caller from claims that are encoded in the access token.</span></span> <span data-ttu-id="da698-137">Läs om vilka typer av token och anspråk som är tillgängliga för en app i den [v2.0 tokens referens](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="da698-137">To learn about all the types of tokens and claims that are available to an app, see the [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="da698-138">En webb-API kan ge användare möjlighet att välja eller välja bort funktioner eller data genom att exponera behörigheter, även kallat [scope](active-directory-v2-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="da698-138">A Web API can give users the power to opt in or opt out of specific functionality or data by exposing permissions, also known as [scopes](active-directory-v2-scopes.md).</span></span> <span data-ttu-id="da698-139">För en anropande app att hämta behörighet till ett omfång måste användaren samtyckta till i omfånget under ett flöde.</span><span class="sxs-lookup"><span data-stu-id="da698-139">For a calling app to acquire permission to a scope, the user must consent to the scope during a flow.</span></span> <span data-ttu-id="da698-140">V2.0-slutpunkten frågar användaren om behörighet och sedan registrerar behörigheter i alla åtkomsttoken som tar emot webb-API.</span><span class="sxs-lookup"><span data-stu-id="da698-140">The v2.0 endpoint asks the user for permission, and then records permissions in all access tokens that the Web API receives.</span></span> <span data-ttu-id="da698-141">Webb-API verifierar åtkomsttoken tas emot på varje anrop och kontrollerar auktorisering.</span><span class="sxs-lookup"><span data-stu-id="da698-141">The Web API validates the access tokens it receives on each call and performs authorization checks.</span></span>

<span data-ttu-id="da698-142">En webb-API kan få åtkomst-token från alla typer av appar, inklusive server webbprogram, skrivbord och mobila appar, sida appar, server-deamon och även andra webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="da698-142">A Web API can receive access tokens from all types of apps, including web server apps, desktop and mobile apps, single-page apps, server-side daemons, and even other Web APIs.</span></span> <span data-ttu-id="da698-143">Det övergripande flödet för en webb-API som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="da698-143">The high-level flow for a Web API looks like this:</span></span>

![Autentiseringsflödet för webb-API](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

<span data-ttu-id="da698-145">Om du vill veta hur du skyddar ett webb-API med hjälp av OAuth2 åtkomsttoken kolla Web API-kodexempel i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="da698-145">To learn how to secure a Web API by using OAuth2 access tokens, check out the Web API code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="da698-146">I många fall web API: er måste också gör utgående förfrågningar till andra underordnade webb-API: er som skyddas av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="da698-146">In many cases, web APIs also need to make outbound requests to other downstream web APIs secured by Azure Active Directory.</span></span>  <span data-ttu-id="da698-147">Om du vill göra det, web API: er kan dra nytta av Azure AD **på uppdrag av** flöde, där webb-API att utbyta ett inkommande åtkomst-token för en annan åtkomsttoken som ska användas i utgående förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="da698-147">To do so, web APIs can take advantage of Azure AD's **On Behalf Of** flow, which allows the web API to exchange an incoming access token for another access token to be used in outbound requests.</span></span>  <span data-ttu-id="da698-148">V2.0 slutpunktens uppdrag flödet beskrivs i [detaljerat här](active-directory-v2-protocols-oauth-on-behalf-of.md).</span><span class="sxs-lookup"><span data-stu-id="da698-148">The v2.0 endpoint's On Behalf Of flow is described in [detail here](active-directory-v2-protocols-oauth-on-behalf-of.md).</span></span>

## <a name="mobile-and-native-apps"></a><span data-ttu-id="da698-149">Mobila och interna appar</span><span class="sxs-lookup"><span data-stu-id="da698-149">Mobile and native apps</span></span>
<span data-ttu-id="da698-150">Enheten installerade appar, till exempel appar och program behöver ofta åtkomst till backend-tjänster eller webb-API: er som lagrar data och utföra funktioner för en användares räkning.</span><span class="sxs-lookup"><span data-stu-id="da698-150">Device-installed apps, such as mobile and desktop apps, often need to access back-end services or Web APIs that store data and perform functions on behalf of a user.</span></span> <span data-ttu-id="da698-151">De här apparna kan lägga till inloggning och auktorisering backend-tjänster med hjälp av den [OAuth 2.0-auktoriseringskodflödet](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="da698-151">These apps can add sign-in and authorization to back-end services by using the [OAuth 2.0 authorization code flow](active-directory-v2-protocols-oauth-code.md).</span></span>

<span data-ttu-id="da698-152">I det här flödet appen tar emot en Auktoriseringskoden från v2.0-slutpunkten när användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="da698-152">In this flow, the app receives an authorization code from the v2.0 endpoint when the user signs in.</span></span> <span data-ttu-id="da698-153">Auktoriseringskoden representerar appens behörighet att anropa backend-tjänster för användarens räkning som är inloggad.</span><span class="sxs-lookup"><span data-stu-id="da698-153">The authorization code represents the app's permission to call back-end services on behalf of the user who is signed in.</span></span> <span data-ttu-id="da698-154">Appen kan utbyta auktoriseringskod i bakgrunden för en OAuth 2.0-åtkomsttoken och en uppdateringstoken.</span><span class="sxs-lookup"><span data-stu-id="da698-154">The app can exchange the authorization code in the background for an OAuth 2.0 access token and a refresh token.</span></span> <span data-ttu-id="da698-155">Appen kan använda åtkomst-token för att autentisera till webb-API: er i HTTP-begäranden och använder uppdateringstoken för att hämta ny åtkomsttoken när äldre åtkomst-token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="da698-155">The app can use the access token to authenticate to Web APIs in HTTP requests, and use the refresh token to get new access tokens when older access tokens expire.</span></span>

![Autentiseringsflödet för inbyggda appen](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a><span data-ttu-id="da698-157">Single-page-appar (JavaScript)</span><span class="sxs-lookup"><span data-stu-id="da698-157">Single-page apps (JavaScript)</span></span>
<span data-ttu-id="da698-158">Många moderna appar innehåller en enda sida app-klientdel som främst är skriven i JavaScript.</span><span class="sxs-lookup"><span data-stu-id="da698-158">Many modern apps have a single-page app front end that primarily is written in JavaScript.</span></span> <span data-ttu-id="da698-159">Ofta skrivs med hjälp av ett ramverk som AngularJS, Ember.js eller Durandal.js.</span><span class="sxs-lookup"><span data-stu-id="da698-159">Often, it's written by using a framework like AngularJS, Ember.js, or Durandal.js.</span></span> <span data-ttu-id="da698-160">Azure AD v2.0-slutpunkten har stöd för dessa appar med hjälp av den [implicita flödet för OAuth 2.0](active-directory-v2-protocols-implicit.md).</span><span class="sxs-lookup"><span data-stu-id="da698-160">The Azure AD v2.0 endpoint supports these apps by using the [OAuth 2.0 implicit flow](active-directory-v2-protocols-implicit.md).</span></span>

<span data-ttu-id="da698-161">I det här flödet appen tar emot token direkt från v2.0 auktorisera slutpunkt, utan några server-till-server-utbyte.</span><span class="sxs-lookup"><span data-stu-id="da698-161">In this flow, the app receives tokens directly from the v2.0 authorize endpoint, without any server-to-server exchanges.</span></span> <span data-ttu-id="da698-162">Alla autentiseringslogiken och session hantering tar placera helt i JavaScript-klienten utan extra sidomdirigeringar.</span><span class="sxs-lookup"><span data-stu-id="da698-162">All authentication logic and session handling takes place entirely in the JavaScript client, without extra page redirects.</span></span>

![Implicit autentiseringsflödet](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

<span data-ttu-id="da698-164">Om du vill se det här scenariot fungerar i praktiken provar du något av kodexemplen sida app i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="da698-164">To see this scenario in action, try one of the single-page app code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

## <a name="daemons-and-server-side-apps"></a><span data-ttu-id="da698-165">Daemon och serversidan appar</span><span class="sxs-lookup"><span data-stu-id="da698-165">Daemons and server-side apps</span></span>
<span data-ttu-id="da698-166">Appar som har tidskrävande processer eller som fungerar utan interaktion med användaren måste också ett sätt att komma åt skyddade resurser, till exempel webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="da698-166">Apps that have long-running processes or that operate without interaction with a user also need a way to access secured resources, such as Web APIs.</span></span> <span data-ttu-id="da698-167">De här apparna kan autentisera och hämta token genom att använda appens identitet i stället en användares delegerade identitet med hjälp av OAuth 2.0-klientautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="da698-167">These apps can authenticate and get tokens by using the app's identity, rather than a user's delegated identity, with the OAuth 2.0 client credentials flow.</span></span>

<span data-ttu-id="da698-168">I det här flödet appen kommunicerar direkt med den `/token` slutpunkten för att hämta slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="da698-168">In this flow, the app interacts directly with the `/token` endpoint to obtain endpoints:</span></span>

![Daemon för app-flöde för autentisering](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

<span data-ttu-id="da698-170">Om du vill skapa en daemon app finns i dokumentationen för autentiseringsuppgifter av klienter i vår [komma igång](active-directory-appmodel-v2-overview.md#getting-started) avsnittet eller försök med en [.NET sample-appen](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span><span class="sxs-lookup"><span data-stu-id="da698-170">To build a daemon app, see the client credentials documentation in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section, or try a [.NET sample app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span></span>

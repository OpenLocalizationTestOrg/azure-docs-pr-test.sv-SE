---
title: "aaaAzure AD v2.0 .NET webb-app anropa API komma igång | Microsoft Docs"
description: "Hur toobuild en .NET MVC-Webbapp som anropar webbtjänster med hjälp av personliga Microsoft-konton och arbets- eller skolkonton för inloggning."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 56be906e-71de-469d-9a5c-9fc08aae4223
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1a70791418bc2a7d1fdfbafb9b5126a033a32292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="calling-a-web-api-from-a-net-web-app"></a><span data-ttu-id="0444a-103">Anropa ett webb-API från en .NET-webbapp</span><span class="sxs-lookup"><span data-stu-id="0444a-103">Calling a web API from a .NET web app</span></span>
<span data-ttu-id="0444a-104">Du kan snabbt lägga till autentisering tooyour webbappar och webb-API: er med stöd för både personliga Microsoft-konton och arbets-eller skolkonton med hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="0444a-104">With hello v2.0 endpoint, you can quickly add authentication tooyour web apps and web APIs with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="0444a-105">Här ska vi skapa en MVC-webbapp som loggar användarna in med OpenID Connect med hjälp från Microsofts OWIN mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="0444a-105">Here, we'll build an MVC web app that signs users in using OpenID Connect, with some help from Microsoft's OWIN middleware.</span></span>  <span data-ttu-id="0444a-106">hello webbprogrammet hämta OAuth 2.0-åtkomsttoken för en webb-api som skyddas av OAuth 2.0 som gör att skapa, läsa och ta bort på en viss användares ”uppgiftslistan”.</span><span class="sxs-lookup"><span data-stu-id="0444a-106">hello web app will get OAuth 2.0 access tokens for a web api secured by OAuth 2.0 that allows create, read, and delete on a given user's "To-Do List".</span></span>

<span data-ttu-id="0444a-107">Den här självstudiekursen kommer fokuserar huvudsakligen på med MSAL tooacquire och använda åtkomst-token i en webbapp som beskrivs i fullständig [här](active-directory-v2-flows.md#web-apps).</span><span class="sxs-lookup"><span data-stu-id="0444a-107">This tutorial will focus primarily on using MSAL tooacquire and use access tokens in a web app, described in full [here](active-directory-v2-flows.md#web-apps).</span></span>  <span data-ttu-id="0444a-108">Som förutsättningar, vill du kanske toofirst Lär dig hur för[lägga till webbapp för enkel inloggning tooa](active-directory-v2-devquickstarts-dotnet-web.md) eller hur för[att säkra ett webb-API](active-directory-v2-devquickstarts-dotnet-api.md).</span><span class="sxs-lookup"><span data-stu-id="0444a-108">As prerequisites, you may want toofirst learn how too[add basic sign-in tooa web app](active-directory-v2-devquickstarts-dotnet-web.md) or how too[properly secure a web API](active-directory-v2-devquickstarts-dotnet-api.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0444a-109">Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="0444a-109">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="0444a-110">toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="0444a-110">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-sample-code"></a><span data-ttu-id="0444a-111">Hämta exempelkoden</span><span class="sxs-lookup"><span data-stu-id="0444a-111">Download sample code</span></span>
<span data-ttu-id="0444a-112">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="0444a-112">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="0444a-113">toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) eller klona hello stommen:</span><span class="sxs-lookup"><span data-stu-id="0444a-113">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

<span data-ttu-id="0444a-114">Du kan också [hämta hello slutförts appen som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) eller klona hello slutförts app:</span><span class="sxs-lookup"><span data-stu-id="0444a-114">Alternatively, you can [download hello completed app as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) or clone hello completed app:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a><span data-ttu-id="0444a-115">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="0444a-115">Register an app</span></span>
<span data-ttu-id="0444a-116">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="0444a-116">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="0444a-117">Se till att:</span><span class="sxs-lookup"><span data-stu-id="0444a-117">Make sure to:</span></span>

* <span data-ttu-id="0444a-118">Kopiera hello **program-Id** tilldelade tooyour app måste den snart.</span><span class="sxs-lookup"><span data-stu-id="0444a-118">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>
* <span data-ttu-id="0444a-119">Skapa en **App hemlighet** av hello **lösenord** typ och kopiera värdet för senare</span><span class="sxs-lookup"><span data-stu-id="0444a-119">Create an **App Secret** of hello **Password** type, and copy down its value for later</span></span>
* <span data-ttu-id="0444a-120">Lägg till hello **Web** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="0444a-120">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="0444a-121">Ange rätt hello **omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="0444a-121">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="0444a-122">hello omdirigerings-uri anger tooAzure AD där autentisering svar som ska dirigeras - hello standard för den här kursen är `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="0444a-122">hello redirect uri indicates tooAzure AD where authentication responses should be directed - hello default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install-owin"></a><span data-ttu-id="0444a-123">Installera OWIN</span><span class="sxs-lookup"><span data-stu-id="0444a-123">Install OWIN</span></span>
<span data-ttu-id="0444a-124">Lägg till hello OWIN mellanprogram NuGet-paket toohello `TodoList-WebApp` projekt med hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="0444a-124">Add hello OWIN middleware NuGet packages toohello `TodoList-WebApp` project using hello Package Manager Console.</span></span>  <span data-ttu-id="0444a-125">hello OWIN mellanprogram kommer att använda tooissue inloggning och utloggningsförfrågningar, hantera hello användarens session och få information om hello användare, bland annat.</span><span class="sxs-lookup"><span data-stu-id="0444a-125">hello OWIN middleware will be used tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, amongst other things.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-hello-user-in"></a><span data-ttu-id="0444a-126">Logga hello användare i</span><span class="sxs-lookup"><span data-stu-id="0444a-126">Sign hello user in</span></span>
<span data-ttu-id="0444a-127">Nu konfigurera hello OWIN mellanprogram toouse hello [autentiseringsprotokollet OpenID Connect](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="0444a-127">Now configure hello OWIN middleware toouse hello [OpenID Connect authentication protocol](active-directory-v2-protocols.md).</span></span>  

* <span data-ttu-id="0444a-128">Öppna hello `web.config` filen i hello rot hello `TodoList-WebApp` projektet och ange din Apps konfigurationsvärden i hello `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0444a-128">Open hello `web.config` file in hello root of hello `TodoList-WebApp` project, and enter your app's configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="0444a-129">Hej `ida:ClientId` är hello **program-Id** tilldelade tooyour app i portalen för registrering av hello.</span><span class="sxs-lookup"><span data-stu-id="0444a-129">hello `ida:ClientId` is hello **Application Id** assigned tooyour app in hello registration portal.</span></span>
  * <span data-ttu-id="0444a-130">Hej `ida:ClientSecret` är hello **App hemlighet** du skapade i hello-portalen för registrering.</span><span class="sxs-lookup"><span data-stu-id="0444a-130">hello `ida:ClientSecret` is hello **App Secret** you created in hello registration portal.</span></span>
  * <span data-ttu-id="0444a-131">Hej `ida:RedirectUri` är hello **omdirigerings-Uri** du angav i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="0444a-131">hello `ida:RedirectUri` is hello **Redirect Uri** you entered in hello portal.</span></span>
* <span data-ttu-id="0444a-132">Öppna hello `web.config` filen i hello rot hello `TodoList-Service` projektet och Ersätt hello `ida:Audience` hello med samma **program-Id** som ovan.</span><span class="sxs-lookup"><span data-stu-id="0444a-132">Open hello `web.config` file in hello root of hello `TodoList-Service` project, and replace hello `ida:Audience` with hello same **Application Id** as above.</span></span>
* <span data-ttu-id="0444a-133">Öppna hello filen `App_Start\Startup.Auth.cs` och Lägg till `using` instruktioner för hello bibliotek från ovan.</span><span class="sxs-lookup"><span data-stu-id="0444a-133">Open hello file `App_Start\Startup.Auth.cs` and add `using` statements for hello libraries from above.</span></span>
* <span data-ttu-id="0444a-134">I Hej samma fil, implementera hello `ConfigureAuth(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="0444a-134">In hello same file, implement hello `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="0444a-135">Hej parametrar som du anger i `OpenIDConnectAuthenticationOptions` fungerar som koordinater för din app toocommunicate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0444a-135">hello parameters you provide in `OpenIDConnectAuthenticationOptions` will serve as coordinates for your app toocommunicate with Azure AD.</span></span>

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // hello `Authority` represents hello v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                    // hello `Scope` describes hello permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure hello user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // hello `AuthorizationCodeReceived` notification is used toocapture and redeem hello authorization_code that hello v2.0 endpoint returns tooyour app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-tooget-access-tokens"></a><span data-ttu-id="0444a-136">Använd MSAL tooget åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="0444a-136">Use MSAL tooget access tokens</span></span>
<span data-ttu-id="0444a-137">I hello `AuthorizationCodeReceived` meddelande vi vill toouse [OAuth 2.0 tillsammans med OpenID Connect](active-directory-v2-protocols.md) tooredeem hello authorization_code för en åtkomst-token toohello att göra tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0444a-137">In hello `AuthorizationCodeReceived` notification, we want toouse [OAuth 2.0 in tandem with OpenID Connect](active-directory-v2-protocols.md) tooredeem hello authorization_code for an access token toohello To-Do List Service.</span></span>  <span data-ttu-id="0444a-138">MSAL kan göra den här processen enkel du:</span><span class="sxs-lookup"><span data-stu-id="0444a-138">MSAL can make this process easy for you:</span></span>

* <span data-ttu-id="0444a-139">Installera först hello förhandsversionen av MSAL:</span><span class="sxs-lookup"><span data-stu-id="0444a-139">First, install hello preview version of MSAL:</span></span>

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* <span data-ttu-id="0444a-140">Och lägga till en annan `using` instruktionen toohello `App_Start\Startup.Auth.cs` -filen för MSAL.</span><span class="sxs-lookup"><span data-stu-id="0444a-140">And add another `using` statement toohello `App_Start\Startup.Auth.cs` file for MSAL.</span></span>
* <span data-ttu-id="0444a-141">Lägg nu till en ny metod hello `OnAuthorizationCodeReceived` händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="0444a-141">Now add a new method, hello `OnAuthorizationCodeReceived` event handler.</span></span>  <span data-ttu-id="0444a-142">Den här hanteraren använder MSAL tooacquire en åtkomst-token toohello API för att göra-lista och lagrar hello token i MSAL'S token-cache för senare:</span><span class="sxs-lookup"><span data-stu-id="0444a-142">This handler will use MSAL tooacquire an access token toohello To-Do List API, and will store hello token in MSAL's token cache for later:</span></span>

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using hello web app's clientId as hello scope, since hello web app and service share hello same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* <span data-ttu-id="0444a-143">I web apps har MSAL en extensible token-cache som kan använda toostore token.</span><span class="sxs-lookup"><span data-stu-id="0444a-143">In web apps, MSAL has an extensible token cache that can be used toostore tokens.</span></span>  <span data-ttu-id="0444a-144">Det här exemplet implementerar hello `NaiveSessionCache` som använder HTTP-session lagring toocache token.</span><span class="sxs-lookup"><span data-stu-id="0444a-144">This sample implements hello `NaiveSessionCache` which uses http session storage toocache tokens.</span></span>

<!-- TODO: Token Cache article -->


## <a name="call-hello-web-api"></a><span data-ttu-id="0444a-145">Anropa hello webb-API</span><span class="sxs-lookup"><span data-stu-id="0444a-145">Call hello Web API</span></span>
<span data-ttu-id="0444a-146">Nu är det dags tooactually använda hello access_token som du har införskaffade i steg 3.</span><span class="sxs-lookup"><span data-stu-id="0444a-146">Now it's time tooactually use hello access_token you acquired in step 3.</span></span>  <span data-ttu-id="0444a-147">Öppna hello webbapp `Controllers\TodoListController.cs` fil, vilket gör att alla hello CRUD begär toohello API för att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="0444a-147">Open hello web app's `Controllers\TodoListController.cs` file, which makes all hello CRUD requests toohello To-Do List API.</span></span>

* <span data-ttu-id="0444a-148">Du kan använda MSAL igen här toofetch access_tokens från hello MSAL cache.</span><span class="sxs-lookup"><span data-stu-id="0444a-148">You can use MSAL again here toofetch access_tokens from hello MSAL cache.</span></span>  <span data-ttu-id="0444a-149">Lägg först till en `using` -instruktion för MSAL toothis fil.</span><span class="sxs-lookup"><span data-stu-id="0444a-149">First, add a `using` statement for MSAL toothis file.</span></span>
  
    `using Microsoft.Identity.Client;`
* <span data-ttu-id="0444a-150">I hello `Index` åtgärdens, Använd MSAL `AcquireTokenSilentAsync` metoden tooget en access_token som kan använda tooread data från hello tjänsten för att göra-lista:</span><span class="sxs-lookup"><span data-stu-id="0444a-150">In hello `Index` action, use MSAL's `AcquireTokenSilentAsync` method tooget an access_token that can be used tooread data from hello To-Do List service:</span></span>

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using hello web app's clientId as hello scope, since hello web app and service share hello same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* <span data-ttu-id="0444a-151">hello exempel läggs sedan till hello resulterande token toohello HTTP GET-begäran som hello `Authorization` sidhuvud, vilka hello uppgiftslista tjänsten använder tooauthenticate hello begäran.</span><span class="sxs-lookup"><span data-stu-id="0444a-151">hello sample then adds hello resulting token toohello HTTP GET request as hello `Authorization` header, which hello To-Do List service uses tooauthenticate hello request.</span></span>
* <span data-ttu-id="0444a-152">Om hello uppgiftslista tjänsten returnerar en `401 Unauthorized` svar, hello access_tokens i MSAL har blivit ogiltigt av någon anledning.</span><span class="sxs-lookup"><span data-stu-id="0444a-152">If hello To-Do List service returns a `401 Unauthorized` response, hello access_tokens in MSAL have become invalid for some reason.</span></span>  <span data-ttu-id="0444a-153">I det här fallet bör du ta bort alla access_tokens från hello MSAL cache och visa hello användaren ett meddelande om att de kanske behöver toosign i igen, vilket startar om hello token förvärv flödet.</span><span class="sxs-lookup"><span data-stu-id="0444a-153">In this case, you should drop any access_tokens from hello MSAL cache and show hello user a message that they may need toosign in again, which will restart hello token acquisition flow.</span></span>

```C#
// ...
// If hello call failed with access denied, then drop hello current access token from hello cache,
// and show hello user an error indicating they might need toosign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need toosign in again.");
}
// ...
```

* <span data-ttu-id="0444a-154">På liknande sätt, om MSAL är tooreturn en access_token av någon anledning, bör du instruera hello användaren toosign i igen.</span><span class="sxs-lookup"><span data-stu-id="0444a-154">Similarly, if MSAL is unable tooreturn an access_token for any reason, you should instruct hello user toosign in again.</span></span>  <span data-ttu-id="0444a-155">Detta är lika enkelt som fångar in alla `MSALException`:</span><span class="sxs-lookup"><span data-stu-id="0444a-155">This is as simple as catching any `MSALException`:</span></span>

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show hello user an error indicating they might need toosign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading tooDo List: " + ee.Message + " You might need toolog out and log back in.");
}
// ...
```

* <span data-ttu-id="0444a-156">hello exakt samma `AcquireTokenSilentAsync` anropet är implementd i hello `Create` och `Delete` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0444a-156">hello exact same `AcquireTokenSilentAsync` call is implementd in hello `Create` and `Delete` actions.</span></span>  <span data-ttu-id="0444a-157">Du kan använda den här MSAL metoden tooget access_tokens när du behöver dem i din app i web apps.</span><span class="sxs-lookup"><span data-stu-id="0444a-157">In web apps, you can use this MSAL method tooget access_tokens whenever you need them in your app.</span></span>  <span data-ttu-id="0444a-158">MSAL hand tar om införskaffa cachelagring och uppdatera token för dig.</span><span class="sxs-lookup"><span data-stu-id="0444a-158">MSAL will take care of acquiring, caching, and refreshing tokens for you.</span></span>

<span data-ttu-id="0444a-159">Slutligen skapar och kör appen!</span><span class="sxs-lookup"><span data-stu-id="0444a-159">Finally, build and run your app!</span></span>  <span data-ttu-id="0444a-160">Logga in med ett Account eller en Azure AD-kontot och Observera hur hello användaridentitet avspeglas i hello övre navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="0444a-160">Sign in with either a Microsoft Account or an Azure AD Account, and notice how hello user's identity is reflected in hello top navigation bar.</span></span>  <span data-ttu-id="0444a-161">Lägg till och ta bort några objekt från hello användarens att göra-lista toosee hello OAuth 2.0 skyddade API-anrop i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0444a-161">Add and delete some items from hello user's To-Do List toosee hello OAuth 2.0 secured API calls in action.</span></span>  <span data-ttu-id="0444a-162">Nu har du en webbprogrammet & webb-API, både skyddas med standardprotokollen, som kan autentisera användare med både sina personliga och arbete/skola konton.</span><span class="sxs-lookup"><span data-stu-id="0444a-162">You now have a web app & web API, both secured using industry standard protocols, that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="0444a-163">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [här](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="0444a-163">For reference, hello completed sample (without your configuration values) [is provided here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="0444a-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0444a-164">Next Steps</span></span>
<span data-ttu-id="0444a-165">För ytterligare resurser, kolla:</span><span class="sxs-lookup"><span data-stu-id="0444a-165">For additional resources, check out:</span></span>

* [<span data-ttu-id="0444a-166">Utvecklarhandbok för hello v2.0 >></span><span class="sxs-lookup"><span data-stu-id="0444a-166">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="0444a-167">StackOverflow ”azure-active-directory” taggen >></span><span class="sxs-lookup"><span data-stu-id="0444a-167">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="0444a-168">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="0444a-168">Get security updates for our products</span></span>
<span data-ttu-id="0444a-169">Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="0444a-169">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>


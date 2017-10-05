---
title: "Azure AD v2.0 .NET webb-app anropa API komma igång | Microsoft Docs"
description: "Hur du skapar en .NET MVC-Webbapp som anropar web services personliga Microsoft-konton och arbete eller skola konton för inloggning."
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
ms.openlocfilehash: dc3162ae8e6ce622139125c2e78fa45d2e90d534
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="calling-a-web-api-from-a-net-web-app"></a><span data-ttu-id="c3119-103">Anropa ett webb-API från en .NET-webbapp</span><span class="sxs-lookup"><span data-stu-id="c3119-103">Calling a web API from a .NET web app</span></span>
<span data-ttu-id="c3119-104">Du kan snabbt lägga till autentisering i dina webbprogram och webb-API: er med stöd för både personliga Microsoft-konton och arbets-eller skolkonton med v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="c3119-104">With the v2.0 endpoint, you can quickly add authentication to your web apps and web APIs with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="c3119-105">Här ska vi skapa en MVC-webbapp som loggar användarna in med OpenID Connect med hjälp från Microsofts OWIN mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="c3119-105">Here, we'll build an MVC web app that signs users in using OpenID Connect, with some help from Microsoft's OWIN middleware.</span></span>  <span data-ttu-id="c3119-106">Webbprogrammet hämta OAuth 2.0-åtkomsttoken för en webb-api som skyddas av OAuth 2.0 som gör att skapa, läsa och ta bort på en viss användares ”uppgiftslistan”.</span><span class="sxs-lookup"><span data-stu-id="c3119-106">The web app will get OAuth 2.0 access tokens for a web api secured by OAuth 2.0 that allows create, read, and delete on a given user's "To-Do List".</span></span>

<span data-ttu-id="c3119-107">Den här självstudiekursen fokuserar huvudsakligen på använder MSAL för att hämta och använda åtkomst-token i en webbapp som beskrivs i fullständig [här](active-directory-v2-flows.md#web-apps).</span><span class="sxs-lookup"><span data-stu-id="c3119-107">This tutorial will focus primarily on using MSAL to acquire and use access tokens in a web app, described in full [here](active-directory-v2-flows.md#web-apps).</span></span>  <span data-ttu-id="c3119-108">Som krav kan du kanske vill först Lär dig hur du [lägga till enkel inloggning till en webbapp](active-directory-v2-devquickstarts-dotnet-web.md) eller hur du [att säkra ett webb-API](active-directory-v2-devquickstarts-dotnet-api.md).</span><span class="sxs-lookup"><span data-stu-id="c3119-108">As prerequisites, you may want to first learn how to [add basic sign-in to a web app](active-directory-v2-devquickstarts-dotnet-web.md) or how to [properly secure a web API](active-directory-v2-devquickstarts-dotnet-api.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c3119-109">Inte alla Azure Active Directory-scenarier och funktioner som stöds av v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="c3119-109">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="c3119-110">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="c3119-110">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-sample-code"></a><span data-ttu-id="c3119-111">Hämta exempelkoden</span><span class="sxs-lookup"><span data-stu-id="c3119-111">Download sample code</span></span>
<span data-ttu-id="c3119-112">Koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="c3119-112">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="c3119-113">Om du vill följa med kan du [ladda ned appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) eller klona stommen:</span><span class="sxs-lookup"><span data-stu-id="c3119-113">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

<span data-ttu-id="c3119-114">Du kan också [ladda ned den färdiga appen som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) eller klona den färdiga appen:</span><span class="sxs-lookup"><span data-stu-id="c3119-114">Alternatively, you can [download the completed app as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) or clone the completed app:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a><span data-ttu-id="c3119-115">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="c3119-115">Register an app</span></span>
<span data-ttu-id="c3119-116">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="c3119-116">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="c3119-117">Se till att:</span><span class="sxs-lookup"><span data-stu-id="c3119-117">Make sure to:</span></span>

* <span data-ttu-id="c3119-118">Kopiera den **program-Id** tilldelats din app måste den snart.</span><span class="sxs-lookup"><span data-stu-id="c3119-118">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>
* <span data-ttu-id="c3119-119">Skapa en **App hemlighet** av den **lösenord** typ och kopiera värdet för senare</span><span class="sxs-lookup"><span data-stu-id="c3119-119">Create an **App Secret** of the **Password** type, and copy down its value for later</span></span>
* <span data-ttu-id="c3119-120">Lägg till den **Web** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="c3119-120">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="c3119-121">Ange rätt **omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="c3119-121">Enter the correct **Redirect URI**.</span></span> <span data-ttu-id="c3119-122">Omdirigerings-uri anger till Azure AD där autentisering svar som ska dirigeras - standarden för den här kursen är `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="c3119-122">The redirect uri indicates to Azure AD where authentication responses should be directed - the default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install-owin"></a><span data-ttu-id="c3119-123">Installera OWIN</span><span class="sxs-lookup"><span data-stu-id="c3119-123">Install OWIN</span></span>
<span data-ttu-id="c3119-124">Lägg till NuGet-paket OWIN mellanprogram för att den `TodoList-WebApp` projekt med Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="c3119-124">Add the OWIN middleware NuGet packages to the `TodoList-WebApp` project using the Package Manager Console.</span></span>  <span data-ttu-id="c3119-125">OWIN-mellanprogram används för att utfärda inloggnings- och utloggningsförfrågningar, hantera användarens session och få information om användare, bland annat.</span><span class="sxs-lookup"><span data-stu-id="c3119-125">The OWIN middleware will be used to issue sign-in and sign-out requests, manage the user's session, and get information about the user, amongst other things.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a><span data-ttu-id="c3119-126">Logga in användaren</span><span class="sxs-lookup"><span data-stu-id="c3119-126">Sign the user in</span></span>
<span data-ttu-id="c3119-127">Nu konfigurera OWIN-mellanprogram för att använda den [autentiseringsprotokollet OpenID Connect](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="c3119-127">Now configure the OWIN middleware to use the [OpenID Connect authentication protocol](active-directory-v2-protocols.md).</span></span>  

* <span data-ttu-id="c3119-128">Öppna den `web.config` filen i roten av den `TodoList-WebApp` projektet och ange din Apps konfigurationsvärden i den `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c3119-128">Open the `web.config` file in the root of the `TodoList-WebApp` project, and enter your app's configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="c3119-129">Den `ida:ClientId` är den **program-Id** tilldelats din app i portalen för registrering.</span><span class="sxs-lookup"><span data-stu-id="c3119-129">The `ida:ClientId` is the **Application Id** assigned to your app in the registration portal.</span></span>
  * <span data-ttu-id="c3119-130">Den `ida:ClientSecret` är den **App hemlighet** du skapade i portalen för registrering.</span><span class="sxs-lookup"><span data-stu-id="c3119-130">The `ida:ClientSecret` is the **App Secret** you created in the registration portal.</span></span>
  * <span data-ttu-id="c3119-131">Den `ida:RedirectUri` är den **omdirigerings-Uri** du angav på portalen.</span><span class="sxs-lookup"><span data-stu-id="c3119-131">The `ida:RedirectUri` is the **Redirect Uri** you entered in the portal.</span></span>
* <span data-ttu-id="c3119-132">Öppna den `web.config` filen i roten av den `TodoList-Service` projektet och Ersätt den `ida:Audience` med samma **program-Id** som ovan.</span><span class="sxs-lookup"><span data-stu-id="c3119-132">Open the `web.config` file in the root of the `TodoList-Service` project, and replace the `ida:Audience` with the same **Application Id** as above.</span></span>
* <span data-ttu-id="c3119-133">Öppna filen `App_Start\Startup.Auth.cs` och Lägg till `using` instruktioner för bibliotek från ovan.</span><span class="sxs-lookup"><span data-stu-id="c3119-133">Open the file `App_Start\Startup.Auth.cs` and add `using` statements for the libraries from above.</span></span>
* <span data-ttu-id="c3119-134">I samma fil implementera den `ConfigureAuth(...)` metoden.</span><span class="sxs-lookup"><span data-stu-id="c3119-134">In the same file, implement the `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="c3119-135">De parametrar som du anger i `OpenIDConnectAuthenticationOptions` fungerar som koordinater för din app för att kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c3119-135">The parameters you provide in `OpenIDConnectAuthenticationOptions` will serve as coordinates for your app to communicate with Azure AD.</span></span>

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a><span data-ttu-id="c3119-136">Använd MSAL för att få åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="c3119-136">Use MSAL to get access tokens</span></span>
<span data-ttu-id="c3119-137">I den `AuthorizationCodeReceived` meddelande ska vi använda [OAuth 2.0 tillsammans med OpenID Connect](active-directory-v2-protocols.md) att lösa in authorization_code för tjänsten att göra en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="c3119-137">In the `AuthorizationCodeReceived` notification, we want to use [OAuth 2.0 in tandem with OpenID Connect](active-directory-v2-protocols.md) to redeem the authorization_code for an access token to the To-Do List Service.</span></span>  <span data-ttu-id="c3119-138">MSAL kan göra den här processen enkel du:</span><span class="sxs-lookup"><span data-stu-id="c3119-138">MSAL can make this process easy for you:</span></span>

* <span data-ttu-id="c3119-139">Installera först förhandsversionen av MSAL:</span><span class="sxs-lookup"><span data-stu-id="c3119-139">First, install the preview version of MSAL:</span></span>

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* <span data-ttu-id="c3119-140">Och lägga till en annan `using` -instruktionen för att den `App_Start\Startup.Auth.cs` filen för MSAL.</span><span class="sxs-lookup"><span data-stu-id="c3119-140">And add another `using` statement to the `App_Start\Startup.Auth.cs` file for MSAL.</span></span>
* <span data-ttu-id="c3119-141">Lägg nu till en ny metod i `OnAuthorizationCodeReceived` händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="c3119-141">Now add a new method, the `OnAuthorizationCodeReceived` event handler.</span></span>  <span data-ttu-id="c3119-142">Den här hanteraren MSAL ska använda för att få en åtkomsttoken att API för att göra-lista och lagrar token i MSAL'S token-cache för senare:</span><span class="sxs-lookup"><span data-stu-id="c3119-142">This handler will use MSAL to acquire an access token to the To-Do List API, and will store the token in MSAL's token cache for later:</span></span>

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* <span data-ttu-id="c3119-143">I web apps har MSAL en extensible token-cache som kan användas för att lagra token.</span><span class="sxs-lookup"><span data-stu-id="c3119-143">In web apps, MSAL has an extensible token cache that can be used to store tokens.</span></span>  <span data-ttu-id="c3119-144">Det här exemplet implementerar den `NaiveSessionCache` som använder HTTP-session lagring till cache-token.</span><span class="sxs-lookup"><span data-stu-id="c3119-144">This sample implements the `NaiveSessionCache` which uses http session storage to cache tokens.</span></span>

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a><span data-ttu-id="c3119-145">Anropa webb-API</span><span class="sxs-lookup"><span data-stu-id="c3119-145">Call the Web API</span></span>
<span data-ttu-id="c3119-146">Nu är det dags att faktiskt använder access_token som du har införskaffade i steg 3.</span><span class="sxs-lookup"><span data-stu-id="c3119-146">Now it's time to actually use the access_token you acquired in step 3.</span></span>  <span data-ttu-id="c3119-147">Öppna webbapp `Controllers\TodoListController.cs` blir alla CRUD-begäranden till API för att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="c3119-147">Open the web app's `Controllers\TodoListController.cs` file, which makes all the CRUD requests to the To-Do List API.</span></span>

* <span data-ttu-id="c3119-148">Du kan använda MSAL igen här att hämta access_tokens från MSAL-cachen.</span><span class="sxs-lookup"><span data-stu-id="c3119-148">You can use MSAL again here to fetch access_tokens from the MSAL cache.</span></span>  <span data-ttu-id="c3119-149">Lägg först till en `using` -instruktion för MSAL till den här filen.</span><span class="sxs-lookup"><span data-stu-id="c3119-149">First, add a `using` statement for MSAL to this file.</span></span>
  
    `using Microsoft.Identity.Client;`
* <span data-ttu-id="c3119-150">I den `Index` åtgärdens, Använd MSAL `AcquireTokenSilentAsync` metod för att hämta en access_token som kan användas för att läsa data från tjänsten för att göra-lista:</span><span class="sxs-lookup"><span data-stu-id="c3119-150">In the `Index` action, use MSAL's `AcquireTokenSilentAsync` method to get an access_token that can be used to read data from the To-Do List service:</span></span>

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* <span data-ttu-id="c3119-151">Exemplet lägger sedan till den resulterande token HTTP GET-begäran som den `Authorization` rubriken, som att göra-lista tjänsten använder för att autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="c3119-151">The sample then adds the resulting token to the HTTP GET request as the `Authorization` header, which the To-Do List service uses to authenticate the request.</span></span>
* <span data-ttu-id="c3119-152">Om tjänsten för att göra-lista returnerar en `401 Unauthorized` svar, access_tokens i MSAL har blivit ogiltigt av någon anledning.</span><span class="sxs-lookup"><span data-stu-id="c3119-152">If the To-Do List service returns a `401 Unauthorized` response, the access_tokens in MSAL have become invalid for some reason.</span></span>  <span data-ttu-id="c3119-153">I det här fallet bör du släppa alla access_tokens från MSAL cache och visar användaren ett meddelande om att de kan behöva logga in igen, vilket startar om token förvärv flödet.</span><span class="sxs-lookup"><span data-stu-id="c3119-153">In this case, you should drop any access_tokens from the MSAL cache and show the user a message that they may need to sign in again, which will restart the token acquisition flow.</span></span>

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

* <span data-ttu-id="c3119-154">På liknande sätt bör MSAL kan inte returnera en access_token av någon anledning, du instruera användaren att logga in igen.</span><span class="sxs-lookup"><span data-stu-id="c3119-154">Similarly, if MSAL is unable to return an access_token for any reason, you should instruct the user to sign in again.</span></span>  <span data-ttu-id="c3119-155">Detta är lika enkelt som fångar in alla `MSALException`:</span><span class="sxs-lookup"><span data-stu-id="c3119-155">This is as simple as catching any `MSALException`:</span></span>

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

* <span data-ttu-id="c3119-156">På exakt samma `AcquireTokenSilentAsync` anropet är implementd i den `Create` och `Delete` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c3119-156">The exact same `AcquireTokenSilentAsync` call is implementd in the `Create` and `Delete` actions.</span></span>  <span data-ttu-id="c3119-157">Du kan använda den här metoden MSAL för att hämta access_tokens när du behöver dem i din app i web apps.</span><span class="sxs-lookup"><span data-stu-id="c3119-157">In web apps, you can use this MSAL method to get access_tokens whenever you need them in your app.</span></span>  <span data-ttu-id="c3119-158">MSAL hand tar om införskaffa cachelagring och uppdatera token för dig.</span><span class="sxs-lookup"><span data-stu-id="c3119-158">MSAL will take care of acquiring, caching, and refreshing tokens for you.</span></span>

<span data-ttu-id="c3119-159">Slutligen skapar och kör appen!</span><span class="sxs-lookup"><span data-stu-id="c3119-159">Finally, build and run your app!</span></span>  <span data-ttu-id="c3119-160">Logga in med ett Account eller en Azure AD-kontot och hur användarens identitet visas i det övre navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="c3119-160">Sign in with either a Microsoft Account or an Azure AD Account, and notice how the user's identity is reflected in the top navigation bar.</span></span>  <span data-ttu-id="c3119-161">Lägg till och ta bort några objekt från användarens att göra-lista för att se OAuth 2.0 skyddade API-anrop i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="c3119-161">Add and delete some items from the user's To-Do List to see the OAuth 2.0 secured API calls in action.</span></span>  <span data-ttu-id="c3119-162">Nu har du en webbprogrammet & webb-API, både skyddas med standardprotokollen, som kan autentisera användare med både sina personliga och arbete/skola konton.</span><span class="sxs-lookup"><span data-stu-id="c3119-162">You now have a web app & web API, both secured using industry standard protocols, that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="c3119-163">För referens anger det slutförda exemplet (utan dina konfigurationsvärden) [här](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="c3119-163">For reference, the completed sample (without your configuration values) [is provided here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="c3119-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3119-164">Next Steps</span></span>
<span data-ttu-id="c3119-165">För ytterligare resurser, kolla:</span><span class="sxs-lookup"><span data-stu-id="c3119-165">For additional resources, check out:</span></span>

* [<span data-ttu-id="c3119-166">Utvecklarhandbok v2.0 >></span><span class="sxs-lookup"><span data-stu-id="c3119-166">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="c3119-167">StackOverflow ”azure-active-directory” taggen >></span><span class="sxs-lookup"><span data-stu-id="c3119-167">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="c3119-168">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="c3119-168">Get security updates for our products</span></span>
<span data-ttu-id="c3119-169">Vi rekommenderar att du aktiverar aviseringar om säkerhetsincidenter genom att gå till [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera på Microsoft Security Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="c3119-169">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>


---
title: "Azure AD v2.0 .NET web app inloggning komma igång | Microsoft Docs"
description: "Hur du skapar en .NET MVC-Webbapp som loggar användarna in med både personliga Account och arbets-eller skolkonton."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c8b97ac6-0a06-4367-81b6-7d1d98152b14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ba5bdf7daba6086b70aec54ebe25d4445fa708c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-net-mvc-web-app"></a><span data-ttu-id="b1f65-103">Lägga till inloggning till en .NET MVC-webbapp</span><span class="sxs-lookup"><span data-stu-id="b1f65-103">Add sign-in to an .NET MVC web app</span></span>
<span data-ttu-id="b1f65-104">Du kan snabbt lägga till autentisering till dina webbappar med stöd för både personliga Microsoft-konton och arbets-eller skolkonton med v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="b1f65-104">With the v2.0 endpoint, you can quickly add authentication to your web apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="b1f65-105">I ASP.NET-webbprogram, kan du göra detta med hjälp av Microsofts OWIN mellanprogram som ingår i .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="b1f65-105">In ASP.NET web apps, you can accomplish this using Microsoft's OWIN middleware included in .NET Framework 4.5.</span></span>

> [!NOTE]
> <span data-ttu-id="b1f65-106">Inte alla Azure Active Directory-scenarier och funktioner som stöds av v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="b1f65-106">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="b1f65-107">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="b1f65-107">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

 <span data-ttu-id="b1f65-108">Här ska vi skapa en webbapp som använder OWIN för att logga in användaren, visa viss information om användaren och logga ut från appen användaren.</span><span class="sxs-lookup"><span data-stu-id="b1f65-108">Here we'll build an web app that uses OWIN to sign the user in, display some information about the user, and sign the user out of the app.</span></span>

## <a name="download"></a><span data-ttu-id="b1f65-109">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="b1f65-109">Download</span></span>
<span data-ttu-id="b1f65-110">Koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="b1f65-110">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="b1f65-111">Om du vill följa med kan du [ladda ned appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) eller klona stommen:</span><span class="sxs-lookup"><span data-stu-id="b1f65-111">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

<span data-ttu-id="b1f65-112">Den färdiga appen finns i slutet av den här kursen samt.</span><span class="sxs-lookup"><span data-stu-id="b1f65-112">The completed app is provided at the end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="b1f65-113">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="b1f65-113">Register an app</span></span>
<span data-ttu-id="b1f65-114">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="b1f65-114">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="b1f65-115">Se till att:</span><span class="sxs-lookup"><span data-stu-id="b1f65-115">Make sure to:</span></span>

* <span data-ttu-id="b1f65-116">Kopiera den **program-Id** tilldelats din app måste den snart.</span><span class="sxs-lookup"><span data-stu-id="b1f65-116">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>
* <span data-ttu-id="b1f65-117">Lägg till den **Web** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="b1f65-117">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="b1f65-118">Ange rätt **omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="b1f65-118">Enter the correct **Redirect URI**.</span></span> <span data-ttu-id="b1f65-119">Omdirigerings-uri anger till Azure AD där autentisering svar som ska dirigeras - standarden för den här kursen är `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="b1f65-119">The redirect uri indicates to Azure AD where authentication responses should be directed - the default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install--configure-owin-authentication"></a><span data-ttu-id="b1f65-120">Installera och konfigurera OWIN-autentisering</span><span class="sxs-lookup"><span data-stu-id="b1f65-120">Install & configure OWIN authentication</span></span>
<span data-ttu-id="b1f65-121">Här kan konfigurerar vi OWIN-mellanprogrammet att använda autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="b1f65-121">Here, we'll configure the OWIN middleware to use the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="b1f65-122">OWIN används för att utfärda inloggnings- och utloggningsförfrågningar, hantera användarens session och få information om användare, bland annat.</span><span class="sxs-lookup"><span data-stu-id="b1f65-122">OWIN will be used to issue sign-in and sign-out requests, manage the user's session, and get information about the user, amongst other things.</span></span>

1. <span data-ttu-id="b1f65-123">Öppna först den `web.config` filen i roten av projektet och ange din Apps konfigurationsvärden i den `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b1f65-123">To begin, open the `web.config` file in the root of the project, and enter your app's configuration values in the `<appSettings>` section.</span></span>

  * <span data-ttu-id="b1f65-124">Den `ida:ClientId` är den **program-Id** tilldelats din app i portalen för registrering.</span><span class="sxs-lookup"><span data-stu-id="b1f65-124">The `ida:ClientId` is the **Application Id** assigned to your app in the registration portal.</span></span>
  * <span data-ttu-id="b1f65-125">Den `ida:RedirectUri` är den **omdirigerings-Uri** du angav på portalen.</span><span class="sxs-lookup"><span data-stu-id="b1f65-125">The `ida:RedirectUri` is the **Redirect Uri** you entered in the portal.</span></span>

2. <span data-ttu-id="b1f65-126">Lägg sedan till OWIN mellanprogram NuGet-paket i projektet med hjälp av Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="b1f65-126">Next, add the OWIN middleware NuGet packages to the project using the Package Manager Console.</span></span>

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. <span data-ttu-id="b1f65-127">Lägg till en ”OWIN-startklass” projektet kallas `Startup.cs` höger klickar du på projektet--> **Lägg till** --> **nytt objekt** --> Sök efter ”OWIN”.</span><span class="sxs-lookup"><span data-stu-id="b1f65-127">Add an "OWIN Startup Class" to the project called `Startup.cs`  Right click on the project --> **Add** --> **New Item** --> Search for "OWIN".</span></span>  <span data-ttu-id="b1f65-128">OWIN-mellanprogrammet anropar `Configuration(...)`-metoden när appen startas.</span><span class="sxs-lookup"><span data-stu-id="b1f65-128">The OWIN middleware will invoke the `Configuration(...)` method when your app starts.</span></span>
4. <span data-ttu-id="b1f65-129">Ändra klassdeklarationen till `public partial class Startup` -vi har implementerat en del av den här klassen som du redan i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="b1f65-129">Change the class declaration to `public partial class Startup` - we've already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="b1f65-130">I den `Configuration(...)` metod, gör ett anrop till ConfigureAuth(...) du konfigurerar autentisering för ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="b1f65-130">In the `Configuration(...)` method, make a call to ConfigureAuth(...) to set up authentication for your web app</span></span>  

        ```C#
        [assembly: OwinStartup(typeof(Startup))]
        
        namespace TodoList_WebApp
        {
            public partial class Startup
            {
                public void Configuration(IAppBuilder app)
                {
                    ConfigureAuth(app);
                }
            }
        }
        ```

5. <span data-ttu-id="b1f65-131">Öppna filen `App_Start\Startup.Auth.cs` och genomföra den `ConfigureAuth(...)` metoden.</span><span class="sxs-lookup"><span data-stu-id="b1f65-131">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="b1f65-132">De parametrar som du anger i `OpenIdConnectAuthenticationOptions` fungerar som koordinater för din app för att kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1f65-132">The parameters you provide in `OpenIdConnectAuthenticationOptions` will serve as coordinates for your app to communicate with Azure AD.</span></span>  <span data-ttu-id="b1f65-133">Du måste också konfigurera Cookie-autentisering - mellanprogram OpenID Connect använder cookies djupare.</span><span class="sxs-lookup"><span data-stu-id="b1f65-133">You'll also need to set up Cookie Authentication - the OpenID Connect middleware uses cookies underneath the covers.</span></span>

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
                                             Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
                                             RedirectUri = redirectUri,
                                             Scope = "openid email profile",
                                             ResponseType = "id_token",
                                             PostLogoutRedirectUri = redirectUri,
                                             TokenValidationParameters = new TokenValidationParameters
                                             {
                                                     ValidateIssuer = false,
                                             },
                                             Notifications = new OpenIdConnectAuthenticationNotifications
                                             {
                                                     AuthenticationFailed = OnAuthenticationFailed,
                                             }
                                     });
                     }
        ```

## <a name="send-authentication-requests"></a><span data-ttu-id="b1f65-134">Skicka autentiseringsbegäranden</span><span class="sxs-lookup"><span data-stu-id="b1f65-134">Send authentication requests</span></span>
<span data-ttu-id="b1f65-135">Appen har nu konfigurerats korrekt för att kommunicera med v2.0-slutpunkten med hjälp av autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="b1f65-135">Your app is now properly configured to communicate with the v2.0 endpoint using the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="b1f65-136">OWIN har tagit hand om alla ugly detaljer utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="b1f65-136">OWIN has taken care of all of the ugly details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span>  <span data-ttu-id="b1f65-137">Allt som återstår är att ge användarna ett sätt att logga in och logga ut.</span><span class="sxs-lookup"><span data-stu-id="b1f65-137">All that remains is to give your users a way to sign in and sign out.</span></span>

- <span data-ttu-id="b1f65-138">Du kan använda auktorisera taggar i dina domänkontrollanter kräver att användaren loggar in innan du använder en viss sida.</span><span class="sxs-lookup"><span data-stu-id="b1f65-138">You can use authorize tags in your controllers to require that user signs in before accessing a certain page.</span></span>  <span data-ttu-id="b1f65-139">Öppna `Controllers\HomeController.cs`, och Lägg till den `[Authorize]` tagg om domänkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="b1f65-139">Open `Controllers\HomeController.cs`, and add the `[Authorize]` tag to the About controller.</span></span>
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- <span data-ttu-id="b1f65-140">Du kan också använda OWIN för att skicka autentiseringsbegäranden från direkt i din kod.</span><span class="sxs-lookup"><span data-stu-id="b1f65-140">You can also use OWIN to directly issue authentication requests from within your code.</span></span>  <span data-ttu-id="b1f65-141">Öppna `Controllers\AccountController.cs`.</span><span class="sxs-lookup"><span data-stu-id="b1f65-141">Open `Controllers\AccountController.cs`.</span></span>  <span data-ttu-id="b1f65-142">I SignIn() och SignOut() åtgärder, utfärda du OpenID Connect challenge respektive utloggning begäranden.</span><span class="sxs-lookup"><span data-stu-id="b1f65-142">In the SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests, respectively.</span></span>

        ```C#
        public void SignIn()
        {
            // Send an OpenID Connect sign-in request.
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        
        // BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
        public void SignOut()
        {
            // Send an OpenID Connect sign-out request.
            HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
            Response.Redirect("/");
        }
        ```

- <span data-ttu-id="b1f65-143">Öppna nu `Views\Shared\_LoginPartial.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="b1f65-143">Now, open `Views\Shared\_LoginPartial.cshtml`.</span></span>  <span data-ttu-id="b1f65-144">Detta är där du visar användaren appens inloggning och utloggning länkar och skriva ut användarens namn i en vy.</span><span class="sxs-lookup"><span data-stu-id="b1f65-144">This is where you'll show the user your app's sign-in and sign-out links, and print out the user's name in a view.</span></span>

        ```HTML
        @if (Request.IsAuthenticated)
        {
            <text>
                <ul class="nav navbar-nav navbar-right">
                    <li class="navbar-text">
        
                        @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@
        
                        Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
                    </li>
                    <li>
                        @Html.ActionLink("Sign out", "SignOut", "Account")
                    </li>
                </ul>
            </text>
        }
        else
        {
            <ul class="nav navbar-nav navbar-right">
                <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
            </ul>
        }
        ```

## <a name="display-user-information"></a><span data-ttu-id="b1f65-145">Visa användarinformation</span><span class="sxs-lookup"><span data-stu-id="b1f65-145">Display user information</span></span>
<span data-ttu-id="b1f65-146">När du autentiserar användare med OpenID Connect returnerar ett id_token till appen som innehåller anspråk eller intyg om användaren v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="b1f65-146">When authenticating users with OpenID Connect, the v2.0 endpoint returns an id_token to the app that contains claims, or assertions about the user.</span></span>  <span data-ttu-id="b1f65-147">Du kan använda dessa anspråk för att anpassa din app:</span><span class="sxs-lookup"><span data-stu-id="b1f65-147">You can use these claims to personalize your app:</span></span>

- <span data-ttu-id="b1f65-148">Öppna filen `Controllers\HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="b1f65-148">Open the `Controllers\HomeController.cs` file.</span></span>  <span data-ttu-id="b1f65-149">Du kan komma åt användarens anspråk i din domänkontrollanter via den `ClaimsPrincipal.Current` säkerhetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="b1f65-149">You can access the user's claims in your controllers via the `ClaimsPrincipal.Current` security principal object.</span></span>

        ```C#
        [Authorize]
        public ActionResult About()
        {
            ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;
        
            // The object ID claim will only be emitted for work or school accounts at this time.
            Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
            ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;
        
            // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
            ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;
        
            // The subject or nameidentifier claim can be used to uniquely identify the user
            ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        
            return View();
        }
        ```

## <a name="run"></a><span data-ttu-id="b1f65-150">Kör</span><span class="sxs-lookup"><span data-stu-id="b1f65-150">Run</span></span>
<span data-ttu-id="b1f65-151">Slutligen skapar och kör appen!</span><span class="sxs-lookup"><span data-stu-id="b1f65-151">Finally, build and run your app!</span></span>   <span data-ttu-id="b1f65-152">Logga in med ett personligt Microsoft-Account eller ett arbets- eller skolkonto konto och hur användarens identitet visas i det övre navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="b1f65-152">Sign in with either a personal Microsoft Account or a work or school account, and notice how the user's identity is reflected in the top navigation bar.</span></span>  <span data-ttu-id="b1f65-153">Nu har du en webbapp som kan skyddas med standardprotokollen som kan autentisera användare med både sina personliga och arbete/skola konton.</span><span class="sxs-lookup"><span data-stu-id="b1f65-153">You now have a web app secured using industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="b1f65-154">För referens anger det slutförda exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), eller kan du klona den från GitHub:</span><span class="sxs-lookup"><span data-stu-id="b1f65-154">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="b1f65-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b1f65-155">Next steps</span></span>
<span data-ttu-id="b1f65-156">Du kan nu gå vidare till mer avancerade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b1f65-156">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="b1f65-157">Du kanske vill prova:</span><span class="sxs-lookup"><span data-stu-id="b1f65-157">You may want to try:</span></span>

[<span data-ttu-id="b1f65-158">Skydda ett webb-API med den v2.0-slutpunkten >></span><span class="sxs-lookup"><span data-stu-id="b1f65-158">Secure a Web API with the the v2.0 endpoint >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

<span data-ttu-id="b1f65-159">För ytterligare resurser, kolla:</span><span class="sxs-lookup"><span data-stu-id="b1f65-159">For additional resources, check out:</span></span>

* [<span data-ttu-id="b1f65-160">Utvecklarhandbok v2.0 >></span><span class="sxs-lookup"><span data-stu-id="b1f65-160">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="b1f65-161">StackOverflow ”azure-active-directory” taggen >></span><span class="sxs-lookup"><span data-stu-id="b1f65-161">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="b1f65-162">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="b1f65-162">Get security updates for our products</span></span>
<span data-ttu-id="b1f65-163">Vi rekommenderar att du aktiverar aviseringar om säkerhetsincidenter genom att gå till [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera på Microsoft Security Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="b1f65-163">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

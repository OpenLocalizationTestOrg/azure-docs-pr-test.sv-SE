---
title: "aaaAzure AD v2.0 .NET web app inloggning komma igång | Microsoft Docs"
description: "Hur toobuild en .NET MVC-Webbapp som loggar användarna in med både personliga Account och arbets-eller skolkonton."
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
ms.openlocfilehash: 241e9c90bd752fbecc3696ce4f1bed3f9772189d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-net-mvc-web-app"></a><span data-ttu-id="085bf-103">Lägga till inloggning tooan .NET MVC-webbapp</span><span class="sxs-lookup"><span data-stu-id="085bf-103">Add sign-in tooan .NET MVC web app</span></span>
<span data-ttu-id="085bf-104">Hello v2.0-slutpunkten kan du snabbt lägga till autentisering tooyour web apps med stöd för både personliga Microsoft-konton och arbets-eller skolkonton.</span><span class="sxs-lookup"><span data-stu-id="085bf-104">With hello v2.0 endpoint, you can quickly add authentication tooyour web apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="085bf-105">I ASP.NET-webbprogram, kan du göra detta med hjälp av Microsofts OWIN mellanprogram som ingår i .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="085bf-105">In ASP.NET web apps, you can accomplish this using Microsoft's OWIN middleware included in .NET Framework 4.5.</span></span>

> [!NOTE]
> <span data-ttu-id="085bf-106">Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="085bf-106">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="085bf-107">toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="085bf-107">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

 <span data-ttu-id="085bf-108">Här vi ska skapa en webbapp som använder OWIN toosign hello användaren i Visa viss information om hello användaren och logga hello användare utanför hello appen.</span><span class="sxs-lookup"><span data-stu-id="085bf-108">Here we'll build an web app that uses OWIN toosign hello user in, display some information about hello user, and sign hello user out of hello app.</span></span>

## <a name="download"></a><span data-ttu-id="085bf-109">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="085bf-109">Download</span></span>
<span data-ttu-id="085bf-110">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="085bf-110">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="085bf-111">toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) eller klona hello stommen:</span><span class="sxs-lookup"><span data-stu-id="085bf-111">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

<span data-ttu-id="085bf-112">hello slutförts app tillhandahålls hello slutet av den här kursen samt.</span><span class="sxs-lookup"><span data-stu-id="085bf-112">hello completed app is provided at hello end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="085bf-113">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="085bf-113">Register an app</span></span>
<span data-ttu-id="085bf-114">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="085bf-114">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="085bf-115">Se till att:</span><span class="sxs-lookup"><span data-stu-id="085bf-115">Make sure to:</span></span>

* <span data-ttu-id="085bf-116">Kopiera hello **program-Id** tilldelade tooyour app måste den snart.</span><span class="sxs-lookup"><span data-stu-id="085bf-116">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>
* <span data-ttu-id="085bf-117">Lägg till hello **Web** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="085bf-117">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="085bf-118">Ange rätt hello **omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="085bf-118">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="085bf-119">hello omdirigerings-uri anger tooAzure AD där autentisering svar som ska dirigeras - hello standard för den här kursen är `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="085bf-119">hello redirect uri indicates tooAzure AD where authentication responses should be directed - hello default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install--configure-owin-authentication"></a><span data-ttu-id="085bf-120">Installera och konfigurera OWIN-autentisering</span><span class="sxs-lookup"><span data-stu-id="085bf-120">Install & configure OWIN authentication</span></span>
<span data-ttu-id="085bf-121">Här kan konfigurerar vi hello OWIN mellanprogram toouse hello autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="085bf-121">Here, we'll configure hello OWIN middleware toouse hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="085bf-122">OWIN kommer att använda tooissue inloggning och utloggningsförfrågningar, hantera hello användarens session och få information om hello användare, bland annat.</span><span class="sxs-lookup"><span data-stu-id="085bf-122">OWIN will be used tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, amongst other things.</span></span>

1. <span data-ttu-id="085bf-123">toobegin, öppna hello `web.config` filen i hello roten av projektet hello och ange din Apps konfigurationsvärden i hello `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="085bf-123">toobegin, open hello `web.config` file in hello root of hello project, and enter your app's configuration values in hello `<appSettings>` section.</span></span>

  * <span data-ttu-id="085bf-124">Hej `ida:ClientId` är hello **program-Id** tilldelade tooyour app i portalen för registrering av hello.</span><span class="sxs-lookup"><span data-stu-id="085bf-124">hello `ida:ClientId` is hello **Application Id** assigned tooyour app in hello registration portal.</span></span>
  * <span data-ttu-id="085bf-125">Hej `ida:RedirectUri` är hello **omdirigerings-Uri** du angav i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="085bf-125">hello `ida:RedirectUri` is hello **Redirect Uri** you entered in hello portal.</span></span>

2. <span data-ttu-id="085bf-126">Lägg till hello OWIN mellanprogram NuGet paket toohello projektet med hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="085bf-126">Next, add hello OWIN middleware NuGet packages toohello project using hello Package Manager Console.</span></span>

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. <span data-ttu-id="085bf-127">Lägg till ett ”OWIN-startklass” toohello projekt kallas `Startup.cs` höger Klicka på hello projektet--> **Lägg till** --> **nytt objekt** --> Sök efter ”OWIN”.</span><span class="sxs-lookup"><span data-stu-id="085bf-127">Add an "OWIN Startup Class" toohello project called `Startup.cs`  Right click on hello project --> **Add** --> **New Item** --> Search for "OWIN".</span></span>  <span data-ttu-id="085bf-128">Hej OWIN mellanprogram ska anropa hello `Configuration(...)` metod när appen startar.</span><span class="sxs-lookup"><span data-stu-id="085bf-128">hello OWIN middleware will invoke hello `Configuration(...)` method when your app starts.</span></span>
4. <span data-ttu-id="085bf-129">Ändra hello klassdeklarationen för`public partial class Startup` -vi har implementerat en del av den här klassen som du redan i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="085bf-129">Change hello class declaration too`public partial class Startup` - we've already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="085bf-130">I hello `Configuration(...)` metod, gör ett anrop tooConfigureAuth(...) tooset in verifiering för ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="085bf-130">In hello `Configuration(...)` method, make a call tooConfigureAuth(...) tooset up authentication for your web app</span></span>  

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

5. <span data-ttu-id="085bf-131">Öppna hello filen `App_Start\Startup.Auth.cs` och implementera hello `ConfigureAuth(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="085bf-131">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="085bf-132">Hej parametrar som du anger i `OpenIdConnectAuthenticationOptions` fungerar som koordinater för din app toocommunicate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="085bf-132">hello parameters you provide in `OpenIdConnectAuthenticationOptions` will serve as coordinates for your app toocommunicate with Azure AD.</span></span>  <span data-ttu-id="085bf-133">Du måste också tooset in Cookie-autentisering - hello OpenID Connect mellanprogram använder cookies under hello omfattar.</span><span class="sxs-lookup"><span data-stu-id="085bf-133">You'll also need tooset up Cookie Authentication - hello OpenID Connect middleware uses cookies underneath hello covers.</span></span>

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

## <a name="send-authentication-requests"></a><span data-ttu-id="085bf-134">Skicka autentiseringsbegäranden</span><span class="sxs-lookup"><span data-stu-id="085bf-134">Send authentication requests</span></span>
<span data-ttu-id="085bf-135">Appen är nu korrekt konfigurerade toocommunicate med hello v2.0-slutpunkten med hjälp av autentiseringsprotokollet för hello OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="085bf-135">Your app is now properly configured toocommunicate with hello v2.0 endpoint using hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="085bf-136">OWIN har tagit hand om alla hello ugly information om utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="085bf-136">OWIN has taken care of all of hello ugly details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span>  <span data-ttu-id="085bf-137">Allt som fortfarande är toogive användarna ett sätt toosign i och logga ut.</span><span class="sxs-lookup"><span data-stu-id="085bf-137">All that remains is toogive your users a way toosign in and sign out.</span></span>

- <span data-ttu-id="085bf-138">Du kan använda auktorisera taggar i dina domänkontrollanter toorequire som användaren loggar in innan du använder en viss sida.</span><span class="sxs-lookup"><span data-stu-id="085bf-138">You can use authorize tags in your controllers toorequire that user signs in before accessing a certain page.</span></span>  <span data-ttu-id="085bf-139">Öppna `Controllers\HomeController.cs`, och Lägg till hello `[Authorize]` tagga toohello om styrenhet.</span><span class="sxs-lookup"><span data-stu-id="085bf-139">Open `Controllers\HomeController.cs`, and add hello `[Authorize]` tag toohello About controller.</span></span>
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- <span data-ttu-id="085bf-140">Du kan också använda OWIN toodirectly problemet autentiseringsbegäranden från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="085bf-140">You can also use OWIN toodirectly issue authentication requests from within your code.</span></span>  <span data-ttu-id="085bf-141">Öppna `Controllers\AccountController.cs`.</span><span class="sxs-lookup"><span data-stu-id="085bf-141">Open `Controllers\AccountController.cs`.</span></span>  <span data-ttu-id="085bf-142">I hello SignIn() och SignOut() åtgärder kan du utfärda OpenID Connect utmaning och utloggning begäranden respektive.</span><span class="sxs-lookup"><span data-stu-id="085bf-142">In hello SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests, respectively.</span></span>

        ```C#
        public void SignIn()
        {
            // Send an OpenID Connect sign-in request.
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        
        // BUGBUG: Ending a session with hello v2.0 endpoint is not yet supported.  Here, we just end hello session with hello web app.  
        public void SignOut()
        {
            // Send an OpenID Connect sign-out request.
            HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
            Response.Redirect("/");
        }
        ```

- <span data-ttu-id="085bf-143">Öppna nu `Views\Shared\_LoginPartial.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="085bf-143">Now, open `Views\Shared\_LoginPartial.cshtml`.</span></span>  <span data-ttu-id="085bf-144">Detta är där du visar hello användaren appens inloggning och utloggning länkar och skriva ut hello användarens namn i en vy.</span><span class="sxs-lookup"><span data-stu-id="085bf-144">This is where you'll show hello user your app's sign-in and sign-out links, and print out hello user's name in a view.</span></span>

        ```HTML
        @if (Request.IsAuthenticated)
        {
            <text>
                <ul class="nav navbar-nav navbar-right">
                    <li class="navbar-text">
        
                        @*hello 'preferred_username' claim can be used for showing hello user's primary way of identifying themselves.*@
        
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

## <a name="display-user-information"></a><span data-ttu-id="085bf-145">Visa användarinformation</span><span class="sxs-lookup"><span data-stu-id="085bf-145">Display user information</span></span>
<span data-ttu-id="085bf-146">När du autentiserar användare med OpenID Connect returnerar en id_token toohello app som innehåller anspråk eller intyg om hello användaren hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="085bf-146">When authenticating users with OpenID Connect, hello v2.0 endpoint returns an id_token toohello app that contains claims, or assertions about hello user.</span></span>  <span data-ttu-id="085bf-147">Du kan använda dessa anspråk toopersonalize din app:</span><span class="sxs-lookup"><span data-stu-id="085bf-147">You can use these claims toopersonalize your app:</span></span>

- <span data-ttu-id="085bf-148">Öppna hello `Controllers\HomeController.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="085bf-148">Open hello `Controllers\HomeController.cs` file.</span></span>  <span data-ttu-id="085bf-149">Du kan komma åt hello användaranspråk i dina domänkontrollanter via hello `ClaimsPrincipal.Current` säkerhetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="085bf-149">You can access hello user's claims in your controllers via hello `ClaimsPrincipal.Current` security principal object.</span></span>

        ```C#
        [Authorize]
        public ActionResult About()
        {
            ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;
        
            // hello object ID claim will only be emitted for work or school accounts at this time.
            Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
            ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;
        
            // hello 'preferred_username' claim can be used for showing hello user's primary way of identifying themselves
            ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;
        
            // hello subject or nameidentifier claim can be used toouniquely identify hello user
            ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        
            return View();
        }
        ```

## <a name="run"></a><span data-ttu-id="085bf-150">Kör</span><span class="sxs-lookup"><span data-stu-id="085bf-150">Run</span></span>
<span data-ttu-id="085bf-151">Slutligen skapar och kör appen!</span><span class="sxs-lookup"><span data-stu-id="085bf-151">Finally, build and run your app!</span></span>   <span data-ttu-id="085bf-152">Logga in med ett personligt Microsoft-Account eller ett arbets- eller skolkonto konto och Observera hur hello användaridentitet avspeglas i hello övre navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="085bf-152">Sign in with either a personal Microsoft Account or a work or school account, and notice how hello user's identity is reflected in hello top navigation bar.</span></span>  <span data-ttu-id="085bf-153">Nu har du en webbapp som kan skyddas med standardprotokollen som kan autentisera användare med både sina personliga och arbete/skola konton.</span><span class="sxs-lookup"><span data-stu-id="085bf-153">You now have a web app secured using industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="085bf-154">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), eller kan du klona den från GitHub:</span><span class="sxs-lookup"><span data-stu-id="085bf-154">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="085bf-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="085bf-155">Next steps</span></span>
<span data-ttu-id="085bf-156">Du kan nu gå vidare till mer avancerade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="085bf-156">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="085bf-157">Du kanske vill tootry:</span><span class="sxs-lookup"><span data-stu-id="085bf-157">You may want tootry:</span></span>

[<span data-ttu-id="085bf-158">Skydda ett webb-API med hello hello v2.0-slutpunkten >></span><span class="sxs-lookup"><span data-stu-id="085bf-158">Secure a Web API with hello hello v2.0 endpoint >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

<span data-ttu-id="085bf-159">För ytterligare resurser, kolla:</span><span class="sxs-lookup"><span data-stu-id="085bf-159">For additional resources, check out:</span></span>

* [<span data-ttu-id="085bf-160">Utvecklarhandbok för hello v2.0 >></span><span class="sxs-lookup"><span data-stu-id="085bf-160">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="085bf-161">StackOverflow ”azure-active-directory” taggen >></span><span class="sxs-lookup"><span data-stu-id="085bf-161">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="085bf-162">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="085bf-162">Get security updates for our products</span></span>
<span data-ttu-id="085bf-163">Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="085bf-163">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

---
title: "Komma igång aaaAzure AD-.NET-webbapp | Microsoft Docs"
description: "Skapa en .NET MVC-webbapp som kan integreras med Azure AD för inloggning."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e15a41a4-dc5d-4c90-b3fe-5dc33b9a1e96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 6d3098c9e3d7e1916ccb110c703f501ae52e788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="efebb-103">ASP.NET-webbprogram inloggning och utloggning med Azure AD</span><span class="sxs-lookup"><span data-stu-id="efebb-103">ASP.NET web app sign-in and sign-out with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="efebb-104">Genom att tillhandahålla en enkel inloggning och utloggning med bara några få rader med kod, enkelt Azure Active Directory (AD Azure) den för du toooutsource webbappen Identitetshantering.</span><span class="sxs-lookup"><span data-stu-id="efebb-104">By providing a single sign-in and sign-out with only a few lines of code, Azure Active Directory (Azure AD) makes it simple for you toooutsource web-app identity management.</span></span> <span data-ttu-id="efebb-105">Du kan signera användare aktivera och inaktivera ASP.NET-webbprogram med hjälp av hello Microsofts implementering av Open Web Interface för .NET (OWIN) mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="efebb-105">You can sign users in and out of ASP.NET web apps by using hello Microsoft implementation of Open Web Interface for .NET (OWIN) middleware.</span></span> <span data-ttu-id="efebb-106">Community-driven OWIN mellanprogram ingår i .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="efebb-106">Community-driven OWIN middleware is included in .NET Framework 4.5.</span></span> <span data-ttu-id="efebb-107">Den här artikeln visar hur toouse OWIN till:</span><span class="sxs-lookup"><span data-stu-id="efebb-107">This article shows how toouse OWIN to:</span></span>

* <span data-ttu-id="efebb-108">Logga in användare i tooweb appar med hjälp av Azure AD som hello identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="efebb-108">Sign users in tooweb apps by using Azure AD as hello identity provider.</span></span>
* <span data-ttu-id="efebb-109">Visa viss användarinformation.</span><span class="sxs-lookup"><span data-stu-id="efebb-109">Display some user information.</span></span>
* <span data-ttu-id="efebb-110">Logga in användare utanför hello appar.</span><span class="sxs-lookup"><span data-stu-id="efebb-110">Sign users out of hello apps.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="efebb-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="efebb-111">Before you get started</span></span>
* <span data-ttu-id="efebb-112">Hämta hello [app stommen](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) eller hämta hello [färdiga exemplet](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="efebb-112">Download hello [app skeleton](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or download hello [completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span></span>
* <span data-ttu-id="efebb-113">Du måste också en Azure AD-klient i vilken tooregister hello-app.</span><span class="sxs-lookup"><span data-stu-id="efebb-113">You also need an Azure AD tenant in which tooregister hello app.</span></span> <span data-ttu-id="efebb-114">Om du inte redan har en Azure AD-klient [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="efebb-114">If you don't already have an Azure AD tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="efebb-115">När du är klar hello Följ hello procedurerna i följande fyra avsnitt.</span><span class="sxs-lookup"><span data-stu-id="efebb-115">When you are ready, follow hello procedures in hello next four sections.</span></span>

## <a name="step-1-register-hello-new-app-with-azure-ad"></a><span data-ttu-id="efebb-116">Steg 1: Registrera hello ny app med Azure AD</span><span class="sxs-lookup"><span data-stu-id="efebb-116">Step 1: Register hello new app with Azure AD</span></span>
<span data-ttu-id="efebb-117">tooset hello app tooauthenticate användare, först registrera det i din klient hello följande:</span><span class="sxs-lookup"><span data-stu-id="efebb-117">tooset up hello app tooauthenticate users, first register it in your tenant by doing hello following:</span></span>

1. <span data-ttu-id="efebb-118">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="efebb-118">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="efebb-119">Klicka på namnet på ditt konto hello översta fältet.</span><span class="sxs-lookup"><span data-stu-id="efebb-119">On hello top bar, click your account name.</span></span> <span data-ttu-id="efebb-120">Under hello **Directory** listan, Välj hello Active Directory-klient där du vill att tooregister hello app.</span><span class="sxs-lookup"><span data-stu-id="efebb-120">Under hello **Directory** list, select hello Active Directory tenant where you want tooregister hello app.</span></span>
3. <span data-ttu-id="efebb-121">Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="efebb-121">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="efebb-122">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="efebb-122">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="efebb-123">Följ hello uppmanar toocreate en ny **webbprogram och/eller WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="efebb-123">Follow hello prompts toocreate a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="efebb-124">**Namnet** beskriver hello app toousers.</span><span class="sxs-lookup"><span data-stu-id="efebb-124">**Name** describes hello app toousers.</span></span>
  * <span data-ttu-id="efebb-125">**Inloggnings-URL** är hello bas-URL för hello app.</span><span class="sxs-lookup"><span data-stu-id="efebb-125">**Sign-On URL** is hello base URL of hello app.</span></span> <span data-ttu-id="efebb-126">hello stommen standard-URL är https://localhost:44320 /.</span><span class="sxs-lookup"><span data-stu-id="efebb-126">hello skeleton's default URL is https://localhost:44320/.</span></span>
6. <span data-ttu-id="efebb-127">När du har slutfört registreringen hello tilldelar Azure AD hello app ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="efebb-127">After you've completed hello registration, Azure AD assigns hello app a unique application ID.</span></span> <span data-ttu-id="efebb-128">Kopiera hello värdet från hello app sidan toouse i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="efebb-128">Copy hello value from hello app page toouse in hello next sections.</span></span>
7. <span data-ttu-id="efebb-129">Från hello **inställningar** -> **egenskaper** för programmet, uppdatera hello App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="efebb-129">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="efebb-130">Hej **App-ID URI** är en unik identifierare för hello app.</span><span class="sxs-lookup"><span data-stu-id="efebb-130">hello **App ID URI** is a unique identifier for hello app.</span></span> <span data-ttu-id="efebb-131">hello namnkonventionen är `https://<tenant-domain>/<app-name>` (till exempel `https://contoso.onmicrosoft.com/my-first-aad-app`).</span><span class="sxs-lookup"><span data-stu-id="efebb-131">hello naming convention is `https://<tenant-domain>/<app-name>` (for example, `https://contoso.onmicrosoft.com/my-first-aad-app`).</span></span>

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a><span data-ttu-id="efebb-132">Steg 2: Konfigurera hello app toouse hello OWIN autentisering pipeline</span><span class="sxs-lookup"><span data-stu-id="efebb-132">Step 2: Set up hello app toouse hello OWIN authentication pipeline</span></span>
<span data-ttu-id="efebb-133">I det här steget konfigurerar du hello OWIN mellanprogram toouse hello autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="efebb-133">In this step, you configure hello OWIN middleware toouse hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="efebb-134">Du använder OWIN tooissue inloggning och utloggning begäranden, hantera användarsessioner, hämta användarinformation och så vidare.</span><span class="sxs-lookup"><span data-stu-id="efebb-134">You use OWIN tooissue sign-in and sign-out requests, manage user sessions, get user information, and so forth.</span></span>

1. <span data-ttu-id="efebb-135">toobegin, lägga till hello OWIN mellanprogram NuGet paket toohello projekt med hjälp av hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="efebb-135">toobegin, add hello OWIN middleware NuGet packages toohello project by using hello Package Manager Console.</span></span>

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. <span data-ttu-id="efebb-136">tooadd en OWIN klassen toohello Startprojekt kallas `Startup.cs`, högerklicka på hello-projektet, Välj **Lägg till**väljer **nytt objekt**, och sök sedan efter **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="efebb-136">tooadd an OWIN Startup class toohello project called `Startup.cs`, right-click hello project, select **Add**, select **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="efebb-137">Hej OWIN mellanprogram anropar hello **Configuration(...)**  metod när hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="efebb-137">hello OWIN middleware invokes hello **Configuration(...)** method when hello app starts.</span></span>
3. <span data-ttu-id="efebb-138">Ändra hello klassdeklarationen för`public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="efebb-138">Change hello class declaration too`public partial class Startup`.</span></span> <span data-ttu-id="efebb-139">Vi har redan implementerats en del av den här klassen för dig i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="efebb-139">We've already implemented part of this class for you in another file.</span></span> <span data-ttu-id="efebb-140">I hello **Configuration(...)**  metod, gör ett anrop för**ConfgureAuth(...)**  tooset in verifiering för hello app.</span><span class="sxs-lookup"><span data-stu-id="efebb-140">In hello **Configuration(...)** method, make a call too**ConfgureAuth(...)** tooset up authentication for hello app.</span></span>  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. <span data-ttu-id="efebb-141">Öppna hello App_Start\Startup.Auth.cs-filen och sedan implementera hello **ConfigureAuth(...)**  metod.</span><span class="sxs-lookup"><span data-stu-id="efebb-141">Open hello App_Start\Startup.Auth.cs file, and then implement hello **ConfigureAuth(...)** method.</span></span> <span data-ttu-id="efebb-142">Hej parametrar som du anger i *OpenIDConnectAuthenticationOptions* fungerar som koordinater för hello app toocommunicate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="efebb-142">hello parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for hello app toocommunicate with Azure AD.</span></span> <span data-ttu-id="efebb-143">Du måste också tooset in cookie-autentisering, eftersom hello OpenID Connect mellanprogram använder cookies i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="efebb-143">You also need tooset up cookie authentication, because hello OpenID Connect middleware uses cookies in hello background.</span></span>

     ```C#
     public void ConfigureAuth(IAppBuilder app)
     {
         app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

         app.UseCookieAuthentication(new CookieAuthenticationOptions());

         app.UseOpenIdConnectAuthentication(
             new OpenIdConnectAuthenticationOptions
             {
                 ClientId = clientId,
                 Authority = authority,
                 PostLogoutRedirectUri = postLogoutRedirectUri,
                 Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = context =>
                        {
                            context.HandleResponse();
                            context.Response.Redirect("/Error?message=" + context.Exception.Message);
                            return Task.FromResult(0);
                        }
                    }
             });
     }
     ```

5. <span data-ttu-id="efebb-144">Öppna hello web.config-filen i projektet hello hello rot och ange hello konfigurationsvärden i hello `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="efebb-144">Open hello web.config file in hello root of hello project, and then enter hello configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="efebb-145">`ida:ClientId`: hello GUID som du kopierade från hello Azure-portalen i ”steg 1: registrera hello nya app med Azure AD”.</span><span class="sxs-lookup"><span data-stu-id="efebb-145">`ida:ClientId`: hello GUID you copied from hello Azure portal in "Step 1: Register hello new app with Azure AD."</span></span>
  * <span data-ttu-id="efebb-146">`ida:Tenant`: hello namnet på din Azure AD-klient (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="efebb-146">`ida:Tenant`: hello name of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="efebb-147">`ida:PostLogoutRedirectUri`: hello-indikator som talar om Azure AD där en användare ska omdirigeras när en begäran om utloggning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="efebb-147">`ida:PostLogoutRedirectUri`: hello indicator that tells Azure AD where a user should be redirected after a sign-out request is successfully completed.</span></span>

## <a name="step-3-use-owin-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="efebb-148">Steg 3: Använd OWIN tooissue inloggning och utloggning begär tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="efebb-148">Step 3: Use OWIN tooissue sign-in and sign-out requests tooAzure AD</span></span>
<span data-ttu-id="efebb-149">hello appen är nu korrekt konfigurerade toocommunicate med Azure AD med hjälp av autentiseringsprotokollet för hello OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="efebb-149">hello app is now properly configured toocommunicate with Azure AD by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="efebb-150">OWIN hanterades alla hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="efebb-150">OWIN has handled all of hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="efebb-151">Allt som fortfarande är toogive användarna ett sätt toosign i och logga ut.</span><span class="sxs-lookup"><span data-stu-id="efebb-151">All that remains is toogive your users a way toosign in and sign out.</span></span>

1. <span data-ttu-id="efebb-152">Du kan använda auktorisera taggar i domänkontrollanter toorequire användare toosign i innan de får tillgång till vissa sidor.</span><span class="sxs-lookup"><span data-stu-id="efebb-152">You can use authorize tags in your controllers toorequire users toosign in before they access certain pages.</span></span> <span data-ttu-id="efebb-153">toodo så öppna Controllers\HomeController.cs och Lägg sedan till hello `[Authorize]` tagga toohello om styrenhet.</span><span class="sxs-lookup"><span data-stu-id="efebb-153">toodo so, open Controllers\HomeController.cs, and then add hello `[Authorize]` tag toohello About controller.</span></span>

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. <span data-ttu-id="efebb-154">Du kan också använda OWIN toodirectly problemet autentiseringsbegäranden från inom din kod.</span><span class="sxs-lookup"><span data-stu-id="efebb-154">You can also use OWIN toodirectly issue authentication requests from within your code.</span></span> <span data-ttu-id="efebb-155">toodo det, öppnar du Controllers\AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="efebb-155">toodo so, open Controllers\AccountController.cs.</span></span> <span data-ttu-id="efebb-156">Sedan utfärda OpenID Connect utmaning och utloggning begäranden i hello SignIn() och SignOut() åtgärder.</span><span class="sxs-lookup"><span data-stu-id="efebb-156">Then, in hello SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests.</span></span>

     ```C#
     public void SignIn()
     {
         // Send an OpenID Connect sign-in request.
         if (!Request.IsAuthenticated)
         {
             HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
         }
     }
     public void SignOut()
     {
         // Send an OpenID Connect sign-out request.
         HttpContext.GetOwinContext().Authentication.SignOut(
              OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
     }
     ```

3. <span data-ttu-id="efebb-157">Öppna Views\Shared\_LoginPartial.cshtml tooshow hello användaren hello app inloggning och utloggning länkar och tooprint ut hello användarens namn i en vy.</span><span class="sxs-lookup"><span data-stu-id="efebb-157">Open Views\Shared\_LoginPartial.cshtml tooshow hello user hello app sign-in and sign-out links, and tooprint out hello user's name in a view.</span></span>

    ```HTML
    @if (Request.IsAuthenticated)
    {
     <text>
         <ul class="nav navbar-nav navbar-right">
             <li class="navbar-text">
                 Hello, @User.Identity.Name!
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

## <a name="step-4-display-user-information"></a><span data-ttu-id="efebb-158">Steg 4: Visa användarinformation</span><span class="sxs-lookup"><span data-stu-id="efebb-158">Step 4: Display user information</span></span>
<span data-ttu-id="efebb-159">När den autentiserar användare med OpenID Connect returnerar en id_token toohello app som innehåller ”anspråk” eller intyg om hello användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="efebb-159">When it authenticates users with OpenID Connect, Azure AD returns an id_token toohello app that contains "claims," or assertions about hello user.</span></span> <span data-ttu-id="efebb-160">Du kan använda dessa anspråk toopersonalize hello hello följande:</span><span class="sxs-lookup"><span data-stu-id="efebb-160">You can use these claims toopersonalize hello app by doing hello following:</span></span>

1. <span data-ttu-id="efebb-161">Öppna hello Controllers\HomeController.cs filen.</span><span class="sxs-lookup"><span data-stu-id="efebb-161">Open hello Controllers\HomeController.cs file.</span></span> <span data-ttu-id="efebb-162">Du kan komma åt hello användaranspråk i dina domänkontrollanter via hello `ClaimsPrincipal.Current` säkerhetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="efebb-162">You can access hello user's claims in your controllers via hello `ClaimsPrincipal.Current` security principal object.</span></span>

 ```C#
 public ActionResult About()
 {
     ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
     ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
     ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
     ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
     ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

     return View();
 }
 ```

2. <span data-ttu-id="efebb-163">Skapa och köra hello app.</span><span class="sxs-lookup"><span data-stu-id="efebb-163">Build and run hello app.</span></span> <span data-ttu-id="efebb-164">Om du inte redan skapat en ny användare i din klient med en onmicrosoft.com-domän, är nu hello tid toodo så.</span><span class="sxs-lookup"><span data-stu-id="efebb-164">If you haven't already created a new user in your tenant with an onmicrosoft.com domain, now is hello time toodo so.</span></span> <span data-ttu-id="efebb-165">Så här gör du:</span><span class="sxs-lookup"><span data-stu-id="efebb-165">Here's how:</span></span>

  <span data-ttu-id="efebb-166">a.</span><span class="sxs-lookup"><span data-stu-id="efebb-166">a.</span></span> <span data-ttu-id="efebb-167">Logga in med den aktuella användaren och Observera hur hello användaridentitet återspeglas på hello översta raden.</span><span class="sxs-lookup"><span data-stu-id="efebb-167">Sign in with that user, and note how hello user's identity is reflected on hello top bar.</span></span>

  <span data-ttu-id="efebb-168">b.</span><span class="sxs-lookup"><span data-stu-id="efebb-168">b.</span></span> <span data-ttu-id="efebb-169">Logga ut och logga sedan in igen som en annan användare i din klient.</span><span class="sxs-lookup"><span data-stu-id="efebb-169">Sign out, and then sign back in as another user in your tenant.</span></span>

  <span data-ttu-id="efebb-170">c.</span><span class="sxs-lookup"><span data-stu-id="efebb-170">c.</span></span> <span data-ttu-id="efebb-171">Om du känner dig särskilt ambitiösa, registrera och köra en annan instans av den här appen (med sin egen clientId) och titta på enkel inloggning i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="efebb-171">If you're feeling particularly ambitious, register and run another instance of this app (with its own clientId), and watch single sign-in in action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="efebb-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="efebb-172">Next steps</span></span>
<span data-ttu-id="efebb-173">Referenser finns [hello slutförts exempel](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (utan dina konfigurationsvärden).</span><span class="sxs-lookup"><span data-stu-id="efebb-173">For reference, see [hello completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="efebb-174">Du kan nu gå vidare toomore avancerade alternativ.</span><span class="sxs-lookup"><span data-stu-id="efebb-174">You can now move on toomore advanced topics.</span></span> <span data-ttu-id="efebb-175">Till exempel försöka [säkra ett webb-API med Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="efebb-175">For example, try [Secure a Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

---
title: "Azure AD-.NET-webbapp komma igång | Microsoft Docs"
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
ms.openlocfilehash: 7ac5d3e5cc28ead993e159d003244e6451acb0cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="d57f0-103">ASP.NET-webbprogram inloggning och utloggning med Azure AD</span><span class="sxs-lookup"><span data-stu-id="d57f0-103">ASP.NET web app sign-in and sign-out with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="d57f0-104">Genom att tillhandahålla en enkel inloggning och utloggning med bara några få rader med kod gör Azure Active Directory (AD Azure) det enkelt för dig att flytta ut webbappen Identitetshantering.</span><span class="sxs-lookup"><span data-stu-id="d57f0-104">By providing a single sign-in and sign-out with only a few lines of code, Azure Active Directory (Azure AD) makes it simple for you to outsource web-app identity management.</span></span> <span data-ttu-id="d57f0-105">Du kan signera användare aktivera och inaktivera ASP.NET-webbprogram med hjälp av Microsofts implementering av Open Web Interface för .NET (OWIN) mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="d57f0-105">You can sign users in and out of ASP.NET web apps by using the Microsoft implementation of Open Web Interface for .NET (OWIN) middleware.</span></span> <span data-ttu-id="d57f0-106">Community-driven OWIN mellanprogram ingår i .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="d57f0-106">Community-driven OWIN middleware is included in .NET Framework 4.5.</span></span> <span data-ttu-id="d57f0-107">Den här artikeln visar hur du använder OWIN till:</span><span class="sxs-lookup"><span data-stu-id="d57f0-107">This article shows how to use OWIN to:</span></span>

* <span data-ttu-id="d57f0-108">Logga in användare till webbprogram med hjälp av Azure AD som identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="d57f0-108">Sign users in to web apps by using Azure AD as the identity provider.</span></span>
* <span data-ttu-id="d57f0-109">Visa viss användarinformation.</span><span class="sxs-lookup"><span data-stu-id="d57f0-109">Display some user information.</span></span>
* <span data-ttu-id="d57f0-110">Logga in användare utanför apparna.</span><span class="sxs-lookup"><span data-stu-id="d57f0-110">Sign users out of the apps.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="d57f0-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="d57f0-111">Before you get started</span></span>
* <span data-ttu-id="d57f0-112">Hämta den [app stommen](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) eller hämta den [färdiga exemplet](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="d57f0-112">Download the [app skeleton](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or download the [completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span></span>
* <span data-ttu-id="d57f0-113">Du måste också en Azure AD-klient som ska registrera appen.</span><span class="sxs-lookup"><span data-stu-id="d57f0-113">You also need an Azure AD tenant in which to register the app.</span></span> <span data-ttu-id="d57f0-114">Om du inte redan har en Azure AD-klient [Lär dig hur du skaffa en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="d57f0-114">If you don't already have an Azure AD tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="d57f0-115">När du är klar följer du procedurerna i följande fyra avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d57f0-115">When you are ready, follow the procedures in the next four sections.</span></span>

## <a name="step-1-register-the-new-app-with-azure-ad"></a><span data-ttu-id="d57f0-116">Steg 1: Registrera den nya appen med Azure AD</span><span class="sxs-lookup"><span data-stu-id="d57f0-116">Step 1: Register the new app with Azure AD</span></span>
<span data-ttu-id="d57f0-117">Om du vill konfigurera appen för att autentisera användare måste först registrera det i din klient genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="d57f0-117">To set up the app to authenticate users, first register it in your tenant by doing the following:</span></span>

1. <span data-ttu-id="d57f0-118">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d57f0-118">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d57f0-119">Klicka på namnet på ditt konto på den översta raden.</span><span class="sxs-lookup"><span data-stu-id="d57f0-119">On the top bar, click your account name.</span></span> <span data-ttu-id="d57f0-120">Under den **Directory** väljer du Active Directory-klient som du vill registrera appen.</span><span class="sxs-lookup"><span data-stu-id="d57f0-120">Under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="d57f0-121">Klicka på **fler tjänster** i det vänstra fönstret och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d57f0-121">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="d57f0-122">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d57f0-122">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="d57f0-123">Följ anvisningarna för att skapa en ny **webbprogram och/eller WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="d57f0-123">Follow the prompts to create a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="d57f0-124">**Namnet** beskriver app till användare.</span><span class="sxs-lookup"><span data-stu-id="d57f0-124">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="d57f0-125">**Inloggnings-URL** är den grundläggande Webbadressen till appen.</span><span class="sxs-lookup"><span data-stu-id="d57f0-125">**Sign-On URL** is the base URL of the app.</span></span> <span data-ttu-id="d57f0-126">Standard-URL för den stommen är https://localhost:44320 /.</span><span class="sxs-lookup"><span data-stu-id="d57f0-126">The skeleton's default URL is https://localhost:44320/.</span></span>
6. <span data-ttu-id="d57f0-127">När du har slutfört registreringen, tilldelar Azure AD appen ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="d57f0-127">After you've completed the registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="d57f0-128">Kopiera värdet från appsidan att använda i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d57f0-128">Copy the value from the app page to use in the next sections.</span></span>
7. <span data-ttu-id="d57f0-129">Från den **inställningar** -> **egenskaper** för programmet, uppdatera App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="d57f0-129">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="d57f0-130">Den **App-ID URI** är en unik identifierare för appen.</span><span class="sxs-lookup"><span data-stu-id="d57f0-130">The **App ID URI** is a unique identifier for the app.</span></span> <span data-ttu-id="d57f0-131">Namngivningskonventionen är `https://<tenant-domain>/<app-name>` (till exempel `https://contoso.onmicrosoft.com/my-first-aad-app`).</span><span class="sxs-lookup"><span data-stu-id="d57f0-131">The naming convention is `https://<tenant-domain>/<app-name>` (for example, `https://contoso.onmicrosoft.com/my-first-aad-app`).</span></span>

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a><span data-ttu-id="d57f0-132">Steg 2: Konfigurera appen för att använda OWIN autentisering pipeline</span><span class="sxs-lookup"><span data-stu-id="d57f0-132">Step 2: Set up the app to use the OWIN authentication pipeline</span></span>
<span data-ttu-id="d57f0-133">I det här steget konfigurerar du OWIN-mellanprogrammet att använda autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="d57f0-133">In this step, you configure the OWIN middleware to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="d57f0-134">Du kan använda OWIN för att utfärda inloggnings- och utloggningsförfrågningar, hantera användarsessioner, hämta användarinformation och så vidare.</span><span class="sxs-lookup"><span data-stu-id="d57f0-134">You use OWIN to issue sign-in and sign-out requests, manage user sessions, get user information, and so forth.</span></span>

1. <span data-ttu-id="d57f0-135">Om du vill börja lägga till OWIN mellanprogram NuGet-paket i projektet med hjälp av Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="d57f0-135">To begin, add the OWIN middleware NuGet packages to the project by using the Package Manager Console.</span></span>

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. <span data-ttu-id="d57f0-136">Att lägga till en OWIN-startklass i projektet med namnet `Startup.cs`, högerklicka på projektet, Välj **Lägg till**väljer **nytt objekt**, och sök sedan efter **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="d57f0-136">To add an OWIN Startup class to the project called `Startup.cs`, right-click the project, select **Add**, select **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="d57f0-137">OWIN-mellanprogram anropar den **Configuration(...)**  metod när appen startar.</span><span class="sxs-lookup"><span data-stu-id="d57f0-137">The OWIN middleware invokes the **Configuration(...)** method when the app starts.</span></span>
3. <span data-ttu-id="d57f0-138">Ändra klassdeklarationen till `public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="d57f0-138">Change the class declaration to `public partial class Startup`.</span></span> <span data-ttu-id="d57f0-139">Vi har redan implementerats en del av den här klassen för dig i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="d57f0-139">We've already implemented part of this class for you in another file.</span></span> <span data-ttu-id="d57f0-140">I den **Configuration(...)**  gör ett anrop till metoden **ConfgureAuth(...)**  du konfigurerar autentisering för appen.</span><span class="sxs-lookup"><span data-stu-id="d57f0-140">In the **Configuration(...)** method, make a call to **ConfgureAuth(...)** to set up authentication for the app.</span></span>  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. <span data-ttu-id="d57f0-141">Öppna filen App_Start\Startup.Auth.cs och sedan implementera den **ConfigureAuth(...)**  metod.</span><span class="sxs-lookup"><span data-stu-id="d57f0-141">Open the App_Start\Startup.Auth.cs file, and then implement the **ConfigureAuth(...)** method.</span></span> <span data-ttu-id="d57f0-142">De parametrar som du anger i *OpenIDConnectAuthenticationOptions* fungerar som koordinater för appen för att kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d57f0-142">The parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for the app to communicate with Azure AD.</span></span> <span data-ttu-id="d57f0-143">Du måste också konfigurera cookie-autentisering, eftersom OpenID Connect mellanprogram använder cookies i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="d57f0-143">You also need to set up cookie authentication, because the OpenID Connect middleware uses cookies in the background.</span></span>

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

5. <span data-ttu-id="d57f0-144">Öppna web.config-filen i roten av projektet och sedan ange konfigurationsvärden i den `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d57f0-144">Open the web.config file in the root of the project, and then enter the configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="d57f0-145">`ida:ClientId`: Det GUID som du kopierade från Azure-portalen i ”steg 1: registrera den nya appen med Azure AD”.</span><span class="sxs-lookup"><span data-stu-id="d57f0-145">`ida:ClientId`: The GUID you copied from the Azure portal in "Step 1: Register the new app with Azure AD."</span></span>
  * <span data-ttu-id="d57f0-146">`ida:Tenant`: Namnet på din Azure AD-klient (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="d57f0-146">`ida:Tenant`: The name of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="d57f0-147">`ida:PostLogoutRedirectUri`: Indikatorn som talar om Azure AD där en användare ska omdirigeras när en begäran om utloggning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d57f0-147">`ida:PostLogoutRedirectUri`: The indicator that tells Azure AD where a user should be redirected after a sign-out request is successfully completed.</span></span>

## <a name="step-3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="d57f0-148">Steg 3: Använd OWIN att utfärda inloggnings- och utloggningsförfrågningar till Azure AD</span><span class="sxs-lookup"><span data-stu-id="d57f0-148">Step 3: Use OWIN to issue sign-in and sign-out requests to Azure AD</span></span>
<span data-ttu-id="d57f0-149">Appen har nu konfigurerats korrekt för att kommunicera med Azure AD med hjälp av autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="d57f0-149">The app is now properly configured to communicate with Azure AD by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="d57f0-150">OWIN hanterades alla detaljer utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="d57f0-150">OWIN has handled all of the details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="d57f0-151">Allt som återstår är att ge användarna ett sätt att logga in och logga ut.</span><span class="sxs-lookup"><span data-stu-id="d57f0-151">All that remains is to give your users a way to sign in and sign out.</span></span>

1. <span data-ttu-id="d57f0-152">Du kan använda auktorisera taggar i dina domänkontrollanter användare måste logga in innan de får tillgång till vissa sidor.</span><span class="sxs-lookup"><span data-stu-id="d57f0-152">You can use authorize tags in your controllers to require users to sign in before they access certain pages.</span></span> <span data-ttu-id="d57f0-153">Om du vill göra det, öppna Controllers\HomeController.cs och Lägg sedan till den `[Authorize]` tagg om domänkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="d57f0-153">To do so, open Controllers\HomeController.cs, and then add the `[Authorize]` tag to the About controller.</span></span>

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. <span data-ttu-id="d57f0-154">Du kan också använda OWIN för att skicka autentiseringsbegäranden från direkt i din kod.</span><span class="sxs-lookup"><span data-stu-id="d57f0-154">You can also use OWIN to directly issue authentication requests from within your code.</span></span> <span data-ttu-id="d57f0-155">Det gör du genom att öppna Controllers\AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="d57f0-155">To do so, open Controllers\AccountController.cs.</span></span> <span data-ttu-id="d57f0-156">Sedan kan utfärda OpenID Connect utmaning och utloggning begäranden i SignIn() och SignOut() åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d57f0-156">Then, in the SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests.</span></span>

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

3. <span data-ttu-id="d57f0-157">Öppna Views\Shared\_LoginPartial.cshtml att visa användaren app inloggning och utloggning länkar, och för att skriva ut användarens namn i en vy.</span><span class="sxs-lookup"><span data-stu-id="d57f0-157">Open Views\Shared\_LoginPartial.cshtml to show the user the app sign-in and sign-out links, and to print out the user's name in a view.</span></span>

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

## <a name="step-4-display-user-information"></a><span data-ttu-id="d57f0-158">Steg 4: Visa användarinformation</span><span class="sxs-lookup"><span data-stu-id="d57f0-158">Step 4: Display user information</span></span>
<span data-ttu-id="d57f0-159">När den autentiserar användare med OpenID Connect returnerar ett id_token till appen som innehåller ”anspråk” eller intyg om användaren i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d57f0-159">When it authenticates users with OpenID Connect, Azure AD returns an id_token to the app that contains "claims," or assertions about the user.</span></span> <span data-ttu-id="d57f0-160">Du kan använda dessa anspråk för att anpassa appen genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="d57f0-160">You can use these claims to personalize the app by doing the following:</span></span>

1. <span data-ttu-id="d57f0-161">Öppna filen Controllers\HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="d57f0-161">Open the Controllers\HomeController.cs file.</span></span> <span data-ttu-id="d57f0-162">Du kan komma åt användarens anspråk i din domänkontrollanter via den `ClaimsPrincipal.Current` säkerhetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="d57f0-162">You can access the user's claims in your controllers via the `ClaimsPrincipal.Current` security principal object.</span></span>

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

2. <span data-ttu-id="d57f0-163">Skapa och köra appen.</span><span class="sxs-lookup"><span data-stu-id="d57f0-163">Build and run the app.</span></span> <span data-ttu-id="d57f0-164">Om du inte redan skapat en ny användare i din klient med en onmicrosoft.com-domän, nu är det dags att göra det.</span><span class="sxs-lookup"><span data-stu-id="d57f0-164">If you haven't already created a new user in your tenant with an onmicrosoft.com domain, now is the time to do so.</span></span> <span data-ttu-id="d57f0-165">Så här gör du:</span><span class="sxs-lookup"><span data-stu-id="d57f0-165">Here's how:</span></span>

  <span data-ttu-id="d57f0-166">a.</span><span class="sxs-lookup"><span data-stu-id="d57f0-166">a.</span></span> <span data-ttu-id="d57f0-167">Logga in med den aktuella användaren och Observera hur användarens identitet avspeglas i det översta fältet.</span><span class="sxs-lookup"><span data-stu-id="d57f0-167">Sign in with that user, and note how the user's identity is reflected on the top bar.</span></span>

  <span data-ttu-id="d57f0-168">b.</span><span class="sxs-lookup"><span data-stu-id="d57f0-168">b.</span></span> <span data-ttu-id="d57f0-169">Logga ut och logga sedan in igen som en annan användare i din klient.</span><span class="sxs-lookup"><span data-stu-id="d57f0-169">Sign out, and then sign back in as another user in your tenant.</span></span>

  <span data-ttu-id="d57f0-170">c.</span><span class="sxs-lookup"><span data-stu-id="d57f0-170">c.</span></span> <span data-ttu-id="d57f0-171">Om du känner dig särskilt ambitiösa, registrera och köra en annan instans av den här appen (med sin egen clientId) och titta på enkel inloggning i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d57f0-171">If you're feeling particularly ambitious, register and run another instance of this app (with its own clientId), and watch single sign-in in action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d57f0-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d57f0-172">Next steps</span></span>
<span data-ttu-id="d57f0-173">Referenser finns [det slutförda exemplet](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (utan dina konfigurationsvärden).</span><span class="sxs-lookup"><span data-stu-id="d57f0-173">For reference, see [the completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="d57f0-174">Nu kan du gå vidare till mer avancerade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d57f0-174">You can now move on to more advanced topics.</span></span> <span data-ttu-id="d57f0-175">Till exempel försöka [säkra ett webb-API med Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="d57f0-175">For example, try [Secure a Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

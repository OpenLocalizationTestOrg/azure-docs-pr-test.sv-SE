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
# <a name="add-sign-in-tooan-net-mvc-web-app"></a>Lägga till inloggning tooan .NET MVC-webbapp
Hello v2.0-slutpunkten kan du snabbt lägga till autentisering tooyour web apps med stöd för både personliga Microsoft-konton och arbets-eller skolkonton.  I ASP.NET-webbprogram, kan du göra detta med hjälp av Microsofts OWIN mellanprogram som ingår i .NET Framework 4.5.

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
>
>

 Här vi ska skapa en webbapp som använder OWIN toosign hello användaren i Visa viss information om hello användaren och logga hello användare utanför hello appen.

## <a name="download"></a>Ladda ned
hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) eller klona hello stommen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

hello slutförts app tillhandahålls hello slutet av den här kursen samt.

## <a name="register-an-app"></a>Registrera en app
Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).  Se till att:

* Kopiera hello **program-Id** tilldelade tooyour app måste den snart.
* Lägg till hello **Web** plattform för din app.
* Ange rätt hello **omdirigerings-URI**. hello omdirigerings-uri anger tooAzure AD där autentisering svar som ska dirigeras - hello standard för den här kursen är `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Installera och konfigurera OWIN-autentisering
Här kan konfigurerar vi hello OWIN mellanprogram toouse hello autentiseringsprotokollet OpenID Connect.  OWIN kommer att använda tooissue inloggning och utloggningsförfrågningar, hantera hello användarens session och få information om hello användare, bland annat.

1. toobegin, öppna hello `web.config` filen i hello roten av projektet hello och ange din Apps konfigurationsvärden i hello `<appSettings>` avsnitt.

  * Hej `ida:ClientId` är hello **program-Id** tilldelade tooyour app i portalen för registrering av hello.
  * Hej `ida:RedirectUri` är hello **omdirigerings-Uri** du angav i hello-portalen.

2. Lägg till hello OWIN mellanprogram NuGet paket toohello projektet med hello Package Manager-konsolen.

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. Lägg till ett ”OWIN-startklass” toohello projekt kallas `Startup.cs` höger Klicka på hello projektet--> **Lägg till** --> **nytt objekt** --> Sök efter ”OWIN”.  Hej OWIN mellanprogram ska anropa hello `Configuration(...)` metod när appen startar.
4. Ändra hello klassdeklarationen för`public partial class Startup` -vi har implementerat en del av den här klassen som du redan i en annan fil.  I hello `Configuration(...)` metod, gör ett anrop tooConfigureAuth(...) tooset in verifiering för ditt webbprogram  

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

5. Öppna hello filen `App_Start\Startup.Auth.cs` och implementera hello `ConfigureAuth(...)` metod.  Hej parametrar som du anger i `OpenIdConnectAuthenticationOptions` fungerar som koordinater för din app toocommunicate med Azure AD.  Du måste också tooset in Cookie-autentisering - hello OpenID Connect mellanprogram använder cookies under hello omfattar.

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

## <a name="send-authentication-requests"></a>Skicka autentiseringsbegäranden
Appen är nu korrekt konfigurerade toocommunicate med hello v2.0-slutpunkten med hjälp av autentiseringsprotokollet för hello OpenID Connect.  OWIN har tagit hand om alla hello ugly information om utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner.  Allt som fortfarande är toogive användarna ett sätt toosign i och logga ut.

- Du kan använda auktorisera taggar i dina domänkontrollanter toorequire som användaren loggar in innan du använder en viss sida.  Öppna `Controllers\HomeController.cs`, och Lägg till hello `[Authorize]` tagga toohello om styrenhet.
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- Du kan också använda OWIN toodirectly problemet autentiseringsbegäranden från inom din kod.  Öppna `Controllers\AccountController.cs`.  I hello SignIn() och SignOut() åtgärder kan du utfärda OpenID Connect utmaning och utloggning begäranden respektive.

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

- Öppna nu `Views\Shared\_LoginPartial.cshtml`.  Detta är där du visar hello användaren appens inloggning och utloggning länkar och skriva ut hello användarens namn i en vy.

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

## <a name="display-user-information"></a>Visa användarinformation
När du autentiserar användare med OpenID Connect returnerar en id_token toohello app som innehåller anspråk eller intyg om hello användaren hello v2.0-slutpunkten.  Du kan använda dessa anspråk toopersonalize din app:

- Öppna hello `Controllers\HomeController.cs` fil.  Du kan komma åt hello användaranspråk i dina domänkontrollanter via hello `ClaimsPrincipal.Current` säkerhetsobjekt.

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

## <a name="run"></a>Kör
Slutligen skapar och kör appen!   Logga in med ett personligt Microsoft-Account eller ett arbets- eller skolkonto konto och Observera hur hello användaridentitet avspeglas i hello övre navigeringsfältet.  Nu har du en webbapp som kan skyddas med standardprotokollen som kan autentisera användare med både sina personliga och arbete/skola konton.

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), eller kan du klona den från GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Nästa steg
Du kan nu gå vidare till mer avancerade avsnitt.  Du kanske vill tootry:

[Skydda ett webb-API med hello hello v2.0-slutpunkten >>](active-directory-devquickstarts-webapi-dotnet.md)

För ytterligare resurser, kolla:

* [Utvecklarhandbok för hello v2.0 >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow ”azure-active-directory” taggen >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Hämta säkerhetsuppdateringar för våra produkter
Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.

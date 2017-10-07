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
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a>ASP.NET-webbprogram inloggning och utloggning med Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Genom att tillhandahålla en enkel inloggning och utloggning med bara några få rader med kod, enkelt Azure Active Directory (AD Azure) den för du toooutsource webbappen Identitetshantering. Du kan signera användare aktivera och inaktivera ASP.NET-webbprogram med hjälp av hello Microsofts implementering av Open Web Interface för .NET (OWIN) mellanprogram. Community-driven OWIN mellanprogram ingår i .NET Framework 4.5. Den här artikeln visar hur toouse OWIN till:

* Logga in användare i tooweb appar med hjälp av Azure AD som hello identitetsleverantör.
* Visa viss användarinformation.
* Logga in användare utanför hello appar.

## <a name="before-you-get-started"></a>Innan du börjar
* Hämta hello [app stommen](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) eller hämta hello [färdiga exemplet](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).
* Du måste också en Azure AD-klient i vilken tooregister hello-app. Om du inte redan har en Azure AD-klient [Lär dig hur tooget en](active-directory-howto-tenant.md).

När du är klar hello Följ hello procedurerna i följande fyra avsnitt.

## <a name="step-1-register-hello-new-app-with-azure-ad"></a>Steg 1: Registrera hello ny app med Azure AD
tooset hello app tooauthenticate användare, först registrera det i din klient hello följande:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på namnet på ditt konto hello översta fältet. Under hello **Directory** listan, Välj hello Active Directory-klient där du vill att tooregister hello app.
3. Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.
4. Klicka på **App registreringar**, och välj sedan **Lägg till**.
5. Följ hello uppmanar toocreate en ny **webbprogram och/eller WebAPI**.
  * **Namnet** beskriver hello app toousers.
  * **Inloggnings-URL** är hello bas-URL för hello app. hello stommen standard-URL är https://localhost:44320 /.
6. När du har slutfört registreringen hello tilldelar Azure AD hello app ett unikt-ID. Kopiera hello värdet från hello app sidan toouse i hello nästa avsnitt.
7. Från hello **inställningar** -> **egenskaper** för programmet, uppdatera hello App-ID-URI. Hej **App-ID URI** är en unik identifierare för hello app. hello namnkonventionen är `https://<tenant-domain>/<app-name>` (till exempel `https://contoso.onmicrosoft.com/my-first-aad-app`).

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a>Steg 2: Konfigurera hello app toouse hello OWIN autentisering pipeline
I det här steget konfigurerar du hello OWIN mellanprogram toouse hello autentiseringsprotokollet OpenID Connect. Du använder OWIN tooissue inloggning och utloggning begäranden, hantera användarsessioner, hämta användarinformation och så vidare.

1. toobegin, lägga till hello OWIN mellanprogram NuGet paket toohello projekt med hjälp av hello Package Manager-konsolen.

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. tooadd en OWIN klassen toohello Startprojekt kallas `Startup.cs`, högerklicka på hello-projektet, Välj **Lägg till**väljer **nytt objekt**, och sök sedan efter **OWIN**. Hej OWIN mellanprogram anropar hello **Configuration(...)**  metod när hello appen startar.
3. Ändra hello klassdeklarationen för`public partial class Startup`. Vi har redan implementerats en del av den här klassen för dig i en annan fil. I hello **Configuration(...)**  metod, gör ett anrop för**ConfgureAuth(...)**  tooset in verifiering för hello app.  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. Öppna hello App_Start\Startup.Auth.cs-filen och sedan implementera hello **ConfigureAuth(...)**  metod. Hej parametrar som du anger i *OpenIDConnectAuthenticationOptions* fungerar som koordinater för hello app toocommunicate med Azure AD. Du måste också tooset in cookie-autentisering, eftersom hello OpenID Connect mellanprogram använder cookies i hello bakgrund.

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

5. Öppna hello web.config-filen i projektet hello hello rot och ange hello konfigurationsvärden i hello `<appSettings>` avsnitt.
  * `ida:ClientId`: hello GUID som du kopierade från hello Azure-portalen i ”steg 1: registrera hello nya app med Azure AD”.
  * `ida:Tenant`: hello namnet på din Azure AD-klient (till exempel contoso.onmicrosoft.com).
  * `ida:PostLogoutRedirectUri`: hello-indikator som talar om Azure AD där en användare ska omdirigeras när en begäran om utloggning har slutförts.

## <a name="step-3-use-owin-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>Steg 3: Använd OWIN tooissue inloggning och utloggning begär tooAzure AD
hello appen är nu korrekt konfigurerade toocommunicate med Azure AD med hjälp av autentiseringsprotokollet för hello OpenID Connect. OWIN hanterades alla hello information utforma autentiseringsmeddelanden, verifiera token från Azure AD och upprätthålla användarsessioner. Allt som fortfarande är toogive användarna ett sätt toosign i och logga ut.

1. Du kan använda auktorisera taggar i domänkontrollanter toorequire användare toosign i innan de får tillgång till vissa sidor. toodo så öppna Controllers\HomeController.cs och Lägg sedan till hello `[Authorize]` tagga toohello om styrenhet.

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. Du kan också använda OWIN toodirectly problemet autentiseringsbegäranden från inom din kod. toodo det, öppnar du Controllers\AccountController.cs. Sedan utfärda OpenID Connect utmaning och utloggning begäranden i hello SignIn() och SignOut() åtgärder.

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

3. Öppna Views\Shared\_LoginPartial.cshtml tooshow hello användaren hello app inloggning och utloggning länkar och tooprint ut hello användarens namn i en vy.

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

## <a name="step-4-display-user-information"></a>Steg 4: Visa användarinformation
När den autentiserar användare med OpenID Connect returnerar en id_token toohello app som innehåller ”anspråk” eller intyg om hello användare i Azure AD. Du kan använda dessa anspråk toopersonalize hello hello följande:

1. Öppna hello Controllers\HomeController.cs filen. Du kan komma åt hello användaranspråk i dina domänkontrollanter via hello `ClaimsPrincipal.Current` säkerhetsobjekt.

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

2. Skapa och köra hello app. Om du inte redan skapat en ny användare i din klient med en onmicrosoft.com-domän, är nu hello tid toodo så. Så här gör du:

  a. Logga in med den aktuella användaren och Observera hur hello användaridentitet återspeglas på hello översta raden.

  b. Logga ut och logga sedan in igen som en annan användare i din klient.

  c. Om du känner dig särskilt ambitiösa, registrera och köra en annan instans av den här appen (med sin egen clientId) och titta på enkel inloggning i åtgärden.

## <a name="next-steps"></a>Nästa steg
Referenser finns [hello slutförts exempel](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (utan dina konfigurationsvärden).

Du kan nu gå vidare toomore avancerade alternativ. Till exempel försöka [säkra ett webb-API med Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

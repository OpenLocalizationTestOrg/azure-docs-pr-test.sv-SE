---
title: "aaaAzure AD v2 ASP.NET Web Server komma igång - inställningar | Microsoft Docs"
description: "Implementera Microsoft logga In på en ASP.NET-lösning med ett traditionellt webbläsarbaserade program med OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: eadc59666557e9cd294e6e99391001120579144c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="set-up-your-project"></a>Konfigurera ditt projekt

Det här avsnittet visar hello steg tooinstall och konfigurera hello autentisering pipeline via OWIN mellanprogram på en ASP.NET-projekt med OpenID Connect. 

> Föredrar toodownload det här exemplet Visual Studio-projekt i stället? [Hämta ett projekt](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) och hoppa över toohello [konfigurationssteget](#create-an-application-express) tooconfigure hello kodexemplet innan du kör.

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a>Skapa din ASP.NET-projekt

> 1. I Visual Studio:`File` > `New` > `Project`<br/>
> 2. Under *Visual C# \Web*väljer `ASP.NET Web Application (.NET Framework)`.
> 3. Namnge ditt program och klicka på *OK*
> 4. Välj `Empty` och välj hello kryssrutan tooadd `MVC` referenser
<!--end-collapse-->

## <a name="add-authentication-components"></a>Lägga till autentiseringskomponenter

1. I Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Lägg till *OWIN mellanprogram NuGet-paket* genom att skriva följande hello i hello fönstret Package Manager-konsolen:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Om dessa bibliotek

>hello bibliotek ovan aktivera enkel inloggning (SSO) med OpenID Connect via cookie-baserad autentisering. När autentiseringen är slutförd och hello token som representerar hello användaren skickas tooyour program, skapar OWIN mellanprogram sessions-cookie. hello webbläsaren använder sedan cookien för efterföljande förfrågningar så hello användaren inte behöver tooretype sitt lösenord och ingen ytterligare verifiering krävs.
<!--end-collapse-->

## <a name="configure-hello-authentication-pipeline"></a>Konfigurera hello autentisering pipeline
hello stegen nedan är används toocreate en OWIN mellanprogram startklass tooconfigure OpenID Connect-autentisering. Den här klassen kommer att köras automatiskt när IIS-processen startar.

> Om ditt projekt inte har en `Startup.cs` filen i rotmappen för hello:<br/>
> 1. Högerklicka på hello projektets rotmapp: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Ge den namnet`Startup.cs`

> Kontrollera hello klass du väljer en OWIN-startklass och inte en standard C#-klass. Kontrollera detta genom att kontrollera om du ser `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` ovanför hello namnområde.


1. Lägg till *OWIN* och *Microsoft.IdentityModel* refererar till för`Startup.cs`:

```csharp
using Microsoft.Owin;
using Owin;
using Microsoft.IdentityModel.Protocols;
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
using Microsoft.Owin.Security.Notifications;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Ersätt startklass med hello koden nedan:
</li>
</ol>

```csharp
public class Startup
{        
    // hello Client ID is used by hello application toouniquely identify itself tooAzure AD.
    string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

    // RedirectUri is hello URL where hello user will be redirected tooafter they sign in.
    string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

    // Tenant is hello tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
    static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

    // Authority is hello URL for authority, composed by Azure Active Directory v2 endpoint and hello tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
    string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

    /// <summary>
    /// Configure OWIN toouse OpenIdConnect 
    /// </summary>
    /// <param name="app"></param>
    public void Configuration(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Sets hello ClientId, authority, RedirectUri as obtained from web.config
                ClientId = clientId,
                Authority = authority,
                RedirectUri = redirectUri,
                // PostLogoutRedirectUri is hello page that users will be redirected tooafter sign-out. In this case, it is using hello home page
                PostLogoutRedirectUri = redirectUri,
                Scope = OpenIdConnectScopes.OpenIdProfile,
                // ResponseType is set toorequest hello id_token - which contains basic information about hello signed-in user
                ResponseType = OpenIdConnectResponseTypes.IdToken,
                // ValidateIssuer set toofalse tooallow personal and work accounts from any organization toosign in tooyour application
                // tooonly allow users from a single organizations, set ValidateIssuer tootrue and 'tenant' setting in web.config toohello tenant name
                // tooallow users from only a list of specific organizations, set ValidateIssuer tootrue and use ValidIssuers parameter 
                TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters() { ValidateIssuer = false },
                // OpenIdConnectAuthenticationNotifications configures OWIN toosend notification of failed authentications tooOnAuthenticationFailed method
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    AuthenticationFailed = OnAuthenticationFailed
                }
            }
        );
    }

    /// <summary>
    /// Handle failed authentication requests by redirecting hello user toohello home page with an error in hello query string
    /// </summary>
    /// <param name="context"></param>
    /// <returns></returns>
    private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
    {
        context.HandleResponse();
        context.Response.Redirect("/?errormessage=" + context.Exception.Message);
        return Task.FromResult(0);
    }
}

```
<!--start-collapse-->
> ### <a name="more-information"></a>Mer information

> Hej parametrar som du anger i *OpenIDConnectAuthenticationOptions* fungerar som koordinater för hello programmet toocommunicate med Azure AD. Eftersom hello OpenID Connect mellanprogram använder cookies i hello bakgrund, behöver du också tooset in cookie-autentisering som hello koden ovan visas. Hej *ValidateIssuer* värde anger OpenIdConnect toonot begränsa åtkomst tooone specifika organisation.
<!--end-collapse-->


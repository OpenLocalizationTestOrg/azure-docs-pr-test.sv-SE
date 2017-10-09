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
## <a name="set-up-your-project"></a><span data-ttu-id="f53df-103">Konfigurera ditt projekt</span><span class="sxs-lookup"><span data-stu-id="f53df-103">Set up your project</span></span>

<span data-ttu-id="f53df-104">Det här avsnittet visar hello steg tooinstall och konfigurera hello autentisering pipeline via OWIN mellanprogram på en ASP.NET-projekt med OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="f53df-104">This section shows hello steps tooinstall and configure hello authentication pipeline via OWIN middleware on an ASP.NET project using OpenID Connect.</span></span> 

> <span data-ttu-id="f53df-105">Föredrar toodownload det här exemplet Visual Studio-projekt i stället?</span><span class="sxs-lookup"><span data-stu-id="f53df-105">Prefer toodownload this sample's Visual Studio project instead?</span></span> <span data-ttu-id="f53df-106">[Hämta ett projekt](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) och hoppa över toohello [konfigurationssteget](#create-an-application-express) tooconfigure hello kodexemplet innan du kör.</span><span class="sxs-lookup"><span data-stu-id="f53df-106">[Download a project](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing.</span></span>

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a><span data-ttu-id="f53df-107">Skapa din ASP.NET-projekt</span><span class="sxs-lookup"><span data-stu-id="f53df-107">Create your ASP.NET project</span></span>

> 1. <span data-ttu-id="f53df-108">I Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="f53df-108">In Visual Studio: `File` > `New` > `Project`</span></span><br/>
> 2. <span data-ttu-id="f53df-109">Under *Visual C# \Web*väljer `ASP.NET Web Application (.NET Framework)`.</span><span class="sxs-lookup"><span data-stu-id="f53df-109">Under *Visual C#\Web*, select `ASP.NET Web Application (.NET Framework)`.</span></span>
> 3. <span data-ttu-id="f53df-110">Namnge ditt program och klicka på *OK*</span><span class="sxs-lookup"><span data-stu-id="f53df-110">Name your application and click *OK*</span></span>
> 4. <span data-ttu-id="f53df-111">Välj `Empty` och välj hello kryssrutan tooadd `MVC` referenser</span><span class="sxs-lookup"><span data-stu-id="f53df-111">Select `Empty` and select hello checkbox tooadd `MVC` references</span></span>
<!--end-collapse-->

## <a name="add-authentication-components"></a><span data-ttu-id="f53df-112">Lägga till autentiseringskomponenter</span><span class="sxs-lookup"><span data-stu-id="f53df-112">Add authentication components</span></span>

1. <span data-ttu-id="f53df-113">I Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`</span><span class="sxs-lookup"><span data-stu-id="f53df-113">In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`</span></span>
2. <span data-ttu-id="f53df-114">Lägg till *OWIN mellanprogram NuGet-paket* genom att skriva följande hello i hello fönstret Package Manager-konsolen:</span><span class="sxs-lookup"><span data-stu-id="f53df-114">Add *OWIN middleware NuGet packages* by typing hello following in hello Package Manager Console window:</span></span>

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a><span data-ttu-id="f53df-115">Om dessa bibliotek</span><span class="sxs-lookup"><span data-stu-id="f53df-115">About these libraries</span></span>

><span data-ttu-id="f53df-116">hello bibliotek ovan aktivera enkel inloggning (SSO) med OpenID Connect via cookie-baserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="f53df-116">hello libraries above enable single sign-on (SSO) using OpenID Connect via cookie-based authentication.</span></span> <span data-ttu-id="f53df-117">När autentiseringen är slutförd och hello token som representerar hello användaren skickas tooyour program, skapar OWIN mellanprogram sessions-cookie.</span><span class="sxs-lookup"><span data-stu-id="f53df-117">After authentication is completed and hello token representing hello user is sent tooyour application, OWIN middleware creates a session cookie.</span></span> <span data-ttu-id="f53df-118">hello webbläsaren använder sedan cookien för efterföljande förfrågningar så hello användaren inte behöver tooretype sitt lösenord och ingen ytterligare verifiering krävs.</span><span class="sxs-lookup"><span data-stu-id="f53df-118">hello browser then uses this cookie on subsequent requests so hello user doesn't need tooretype their password, and no additional verification is needed.</span></span>
<!--end-collapse-->

## <a name="configure-hello-authentication-pipeline"></a><span data-ttu-id="f53df-119">Konfigurera hello autentisering pipeline</span><span class="sxs-lookup"><span data-stu-id="f53df-119">Configure hello authentication pipeline</span></span>
<span data-ttu-id="f53df-120">hello stegen nedan är används toocreate en OWIN mellanprogram startklass tooconfigure OpenID Connect-autentisering.</span><span class="sxs-lookup"><span data-stu-id="f53df-120">hello steps below are used toocreate an OWIN middleware Startup Class tooconfigure OpenID Connect authentication.</span></span> <span data-ttu-id="f53df-121">Den här klassen kommer att köras automatiskt när IIS-processen startar.</span><span class="sxs-lookup"><span data-stu-id="f53df-121">This class will be executed automatically when your IIS process starts.</span></span>

> <span data-ttu-id="f53df-122">Om ditt projekt inte har en `Startup.cs` filen i rotmappen för hello:</span><span class="sxs-lookup"><span data-stu-id="f53df-122">If your project doesn't have a `Startup.cs` file in hello root folder:</span></span><br/>
> 1. <span data-ttu-id="f53df-123">Högerklicka på hello projektets rotmapp: >`Add` > `New Item...` > `OWIN Startup class`</span><span class="sxs-lookup"><span data-stu-id="f53df-123">Right click on hello project's root folder: >  `Add` > `New Item...` > `OWIN Startup class`</span></span><br/>
> 2. <span data-ttu-id="f53df-124">Ge den namnet`Startup.cs`</span><span class="sxs-lookup"><span data-stu-id="f53df-124">Name it `Startup.cs`</span></span>

> <span data-ttu-id="f53df-125">Kontrollera hello klass du väljer en OWIN-startklass och inte en standard C#-klass.</span><span class="sxs-lookup"><span data-stu-id="f53df-125">Make sure hello class selected is an OWIN Startup Class and not a standard C# class.</span></span> <span data-ttu-id="f53df-126">Kontrollera detta genom att kontrollera om du ser `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` ovanför hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="f53df-126">Confirm this by checking if you see `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` above hello namespace.</span></span>


1. <span data-ttu-id="f53df-127">Lägg till *OWIN* och *Microsoft.IdentityModel* refererar till för`Startup.cs`:</span><span class="sxs-lookup"><span data-stu-id="f53df-127">Add *OWIN* and *Microsoft.IdentityModel* references too`Startup.cs`:</span></span>

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
<span data-ttu-id="f53df-128">Ersätt startklass med hello koden nedan:</span><span class="sxs-lookup"><span data-stu-id="f53df-128">Replace Startup class with hello code below:</span></span>
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
> ### <a name="more-information"></a><span data-ttu-id="f53df-129">Mer information</span><span class="sxs-lookup"><span data-stu-id="f53df-129">More Information</span></span>

> <span data-ttu-id="f53df-130">Hej parametrar som du anger i *OpenIDConnectAuthenticationOptions* fungerar som koordinater för hello programmet toocommunicate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f53df-130">hello parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for hello application toocommunicate with Azure AD.</span></span> <span data-ttu-id="f53df-131">Eftersom hello OpenID Connect mellanprogram använder cookies i hello bakgrund, behöver du också tooset in cookie-autentisering som hello koden ovan visas.</span><span class="sxs-lookup"><span data-stu-id="f53df-131">Because hello OpenID Connect middleware uses cookies in hello background, you also need tooset up cookie authentication as hello code above shows.</span></span> <span data-ttu-id="f53df-132">Hej *ValidateIssuer* värde anger OpenIdConnect toonot begränsa åtkomst tooone specifika organisation.</span><span class="sxs-lookup"><span data-stu-id="f53df-132">hello *ValidateIssuer* value tells OpenIdConnect toonot restrict access tooone specific organization.</span></span>
<!--end-collapse-->


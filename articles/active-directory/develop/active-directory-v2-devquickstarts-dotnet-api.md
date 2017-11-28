---
title: "aaaAdd inloggning tooa .NET MVC webb-API med hjälp av hello Azure AD v2.0-slutpunkten | Microsoft Docs"
description: "Hur toobuild en .NET MVC webb-Api som accepterar token från både personliga Account och arbets-eller skolkonton."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e77bc4e0-d0c9-4075-a3f6-769e2c810206
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4e517145422bb6e9368e82a7eef4a5c57cce530a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-an-mvc-web-api"></a><span data-ttu-id="ee6bd-103">Skydda en MVC-webb-API</span><span class="sxs-lookup"><span data-stu-id="ee6bd-103">Secure an MVC web API</span></span>
<span data-ttu-id="ee6bd-104">Med Azure Active Directory hello v2.0-slutpunkten kan du skydda ett webb-API med hjälp av [OAuth 2.0](active-directory-v2-protocols.md) åtkomsttoken, aktivera användare med både personliga Microsoft-konto och arbets- eller skolkonto konton toosecurely åtkomst till webb-API.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-104">With Azure Active Directory hello v2.0 endpoint, you can protect a Web API using [OAuth 2.0](active-directory-v2-protocols.md) access tokens, enabling users with both personal Microsoft account and work or school accounts toosecurely access your Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="ee6bd-105">Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-105">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="ee6bd-106">toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="ee6bd-106">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

<span data-ttu-id="ee6bd-107">I ASP.NET webb-API: er, kan du göra detta med hjälp av Microsofts OWIN mellanprogram som ingår i .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-107">In ASP.NET web APIs, you can accomplish this using Microsoft’s OWIN middleware included in .NET Framework 4.5.</span></span>  <span data-ttu-id="ee6bd-108">Vi använder här OWIN toobuild en ”tooDo listan” MVC webb-API som gör att klienter toocreate och läsa uppgifter från en användares att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-108">Here we’ll use OWIN toobuild a "tooDo List" MVC Web API that allows clients toocreate and read tasks from a user's To-Do list.</span></span>  <span data-ttu-id="ee6bd-109">hello webb-API verifierar att inkommande begäranden innehåller en giltig åtkomst-token och avvisa alla begäranden som inte klarar valideringen på en skyddad väg.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-109">hello web API will validate that incoming requests contain a valid access token and reject any requests that do not pass validation on a protected route.</span></span>  <span data-ttu-id="ee6bd-110">Det här exemplet har skapats med Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-110">This sample was built using Visual Studio 2015.</span></span>

## <a name="download"></a><span data-ttu-id="ee6bd-111">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="ee6bd-111">Download</span></span>
<span data-ttu-id="ee6bd-112">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span><span class="sxs-lookup"><span data-stu-id="ee6bd-112">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span></span>  <span data-ttu-id="ee6bd-113">toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) eller klona hello stommen:</span><span class="sxs-lookup"><span data-stu-id="ee6bd-113">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

<span data-ttu-id="ee6bd-114">hello stommen app innehåller alla hello formaterad exempelkod för ett enkelt API, men saknar de över hello identity-relaterade objekt.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-114">hello skeleton app includes all hello boilerplate code for a simple API, but is missing all of hello identity-related pieces.</span></span> <span data-ttu-id="ee6bd-115">Om du inte vill toofollow längs, kan du i stället klona eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="ee6bd-115">If you don't want toofollow along, you can instead clone or [download hello completed sample](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span></span>

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="ee6bd-116">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="ee6bd-116">Register an app</span></span>
<span data-ttu-id="ee6bd-117">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="ee6bd-117">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="ee6bd-118">Se till att:</span><span class="sxs-lookup"><span data-stu-id="ee6bd-118">Make sure to:</span></span>

* <span data-ttu-id="ee6bd-119">Kopiera hello **program-Id** tilldelade tooyour app måste den snart.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-119">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>

<span data-ttu-id="ee6bd-120">Visual studio-lösning innehåller också en ”TodoListClient”, vilket är en enkel WPF-program.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-120">This visual studio solution also contains a "TodoListClient", which is a simple WPF app.</span></span>  <span data-ttu-id="ee6bd-121">Hej TodoListClient är används toodemonstrate hur en användare loggar in och hur en klient kan skicka begäranden tooyour Web API.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-121">hello TodoListClient is used toodemonstrate how a user signs-in and how a client can issue requests tooyour Web API.</span></span>  <span data-ttu-id="ee6bd-122">I det här fallet både hello TodoListClient och hello TodoListService representeras av hello samma app.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-122">In this case, both hello TodoListClient and hello TodoListService are represented by hello same app.</span></span>  <span data-ttu-id="ee6bd-123">tooconfigure Hej TodoListClient, bör du även:</span><span class="sxs-lookup"><span data-stu-id="ee6bd-123">tooconfigure hello TodoListClient, you should also:</span></span>

* <span data-ttu-id="ee6bd-124">Lägg till hello **Mobile** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-124">Add hello **Mobile** platform for your app.</span></span>

## <a name="install-owin"></a><span data-ttu-id="ee6bd-125">Installera OWIN</span><span class="sxs-lookup"><span data-stu-id="ee6bd-125">Install OWIN</span></span>
<span data-ttu-id="ee6bd-126">Nu när du har registrerat en app, behöver tooset upp din app toocommunicate med hello v2.0-slutpunkten i ordning toovalidate inkommande begäranden och token.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-126">Now that you’ve registered an app, you need tooset up your app toocommunicate with hello v2.0 endpoint in order toovalidate incoming requests & tokens.</span></span>

* <span data-ttu-id="ee6bd-127">toobegin, öppna hello lösningen och Lägg till hello OWIN mellanprogram NuGet-paket toohello TodoListService projektet med hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-127">toobegin, open hello solution and add hello OWIN middleware NuGet packages toohello TodoListService project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a><span data-ttu-id="ee6bd-128">Konfigurera OAuth-autentisering</span><span class="sxs-lookup"><span data-stu-id="ee6bd-128">Configure OAuth authentication</span></span>
* <span data-ttu-id="ee6bd-129">Lägg till en OWIN klassen toohello TodoListService Startprojekt kallas `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-129">Add an OWIN Startup class toohello TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="ee6bd-130">Högerklicka på projektet hello--> **Lägg till** --> **nytt objekt** --> Sök efter ”OWIN”.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-130">Right click on hello project --> **Add** --> **New Item** --> Search for “OWIN”.</span></span>  <span data-ttu-id="ee6bd-131">Hej OWIN mellanprogram ska anropa hello `Configuration(…)` metod när appen startar.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-131">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>
* <span data-ttu-id="ee6bd-132">Ändra hello klassdeklarationen för`public partial class Startup` -vi har implementerat en del av den här klassen som du redan i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-132">Change hello class declaration too`public partial class Startup` - we’ve already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="ee6bd-133">I hello `Configuration(…)` metod, gör ett anrop tooConfgureAuth(...) tooset in verifiering för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-133">In hello `Configuration(…)` method, make a call tooConfgureAuth(…) tooset up authentication for your web app.</span></span>

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* <span data-ttu-id="ee6bd-134">Öppna hello filen `App_Start\Startup.Auth.cs` och implementera hello `ConfigureAuth(…)` metod som ställer in hello Web API tooaccept token från hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-134">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(…)` method, which will set up hello Web API tooaccept tokens from hello v2.0 endpoint.</span></span>

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, hello TodoListClient and TodoListService
                // are represented using hello same Application Id - we use
                // hello Application Id toorepresent hello audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that hello user's organization (if applicable) has
                // signed up for hello app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up hello OWIN pipeline toouse OAuth 2.0 Bearer authentication.
        // hello options provided here tell hello middleware about hello type of tokens
        // that will be recieved, which are JWTs for hello v2.0 endpoint.

        // NOTE: hello usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by hello v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used toofetch & use hello OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* <span data-ttu-id="ee6bd-135">Nu kan du använda `[Authorize]` attribut tooprotect dina domänkontrollanter och åtgärder med OAuth 2.0-ägar-autentisering.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-135">Now you can use `[Authorize]` attributes tooprotect your controllers and actions with OAuth 2.0 bearer authentication.</span></span>  <span data-ttu-id="ee6bd-136">Skapa snygga hello `Controllers\TodoListController.cs` klass med en auktorisera-tagg.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-136">Decorate hello `Controllers\TodoListController.cs` class with an authorize tag.</span></span>  <span data-ttu-id="ee6bd-137">Detta tvingar hello användaren toosign i innan sidan.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-137">This will force hello user toosign in before accessing that page.</span></span>

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* <span data-ttu-id="ee6bd-138">När en auktoriserad anroparen har anropar en hello `TodoListController` API: er, hello åtgärden kanske behöver komma åt tooinformation om hello anroparen.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-138">When an authorized caller successfully invokes one of hello `TodoListController` APIs, hello action might need access tooinformation about hello caller.</span></span>  <span data-ttu-id="ee6bd-139">OWIN tillhandahåller åtkomst toohello anspråk i hello ägar-token via hello `ClaimsPrincpal` objekt.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-139">OWIN provides access toohello claims inside hello bearer token via hello `ClaimsPrincpal` object.</span></span>  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use hello ClaimsPrincipal tooaccess information about the
    // user making hello call.  In this case, we use hello 'sub' or
    // NameIdentifier claim tooserve as a key for hello tasks in hello data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* <span data-ttu-id="ee6bd-140">Slutligen öppna hello `web.config` filen i hello roten för hello TodoListService projekt och ange dina konfigurationsvärden i hello `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-140">Finally, open hello `web.config` file in hello root of hello TodoListService project, and enter your configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="ee6bd-141">Din `ida:Audience` är hello **program-Id** hello-appen som du angav i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-141">Your `ida:Audience` is hello **Application Id** of hello app that you entered in hello portal.</span></span>

## <a name="configure-hello-client-app"></a><span data-ttu-id="ee6bd-142">Konfigurera hello-klientappen</span><span class="sxs-lookup"><span data-stu-id="ee6bd-142">Configure hello client app</span></span>
<span data-ttu-id="ee6bd-143">Innan du kan se hello Todo tjänsten i åtgärden måste tooconfigure hello Todo listan klienten så att den kan hämta token från hello v2.0-slutpunkten och att anrop toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-143">Before you can see hello Todo List Service in action, you need tooconfigure hello Todo List Client so it can get tokens from hello v2.0 endpoint and make calls toohello service.</span></span>

* <span data-ttu-id="ee6bd-144">Öppna i hello TodoListClient project `App.config` och ange dina konfigurationsvärden i hello `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-144">In hello TodoListClient project, open `App.config` and enter your configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="ee6bd-145">Din `ida:ClientId` program-Id som du kopierade från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-145">Your `ida:ClientId` Application Id you copied from hello portal.</span></span>

<span data-ttu-id="ee6bd-146">Slutligen, rensa, skapa och köra varje projekt!</span><span class="sxs-lookup"><span data-stu-id="ee6bd-146">Finally, clean, build and run each project!</span></span>  <span data-ttu-id="ee6bd-147">Nu har du en .NET MVC webb-API som accepterar token från både personliga Microsoft-konton och arbets-eller skolkonton.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-147">You now have a .NET MVC Web API that accepts tokens from both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="ee6bd-148">Logga in på hello TodoListClient och anropa web api tooadd uppgifter toohello användarens att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-148">Sign into hello TodoListClient, and call your web api tooadd tasks toohello user's To-Do list.</span></span>

<span data-ttu-id="ee6bd-149">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), eller kan du klona den från GitHub:</span><span class="sxs-lookup"><span data-stu-id="ee6bd-149">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="ee6bd-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee6bd-150">Next steps</span></span>
<span data-ttu-id="ee6bd-151">Du kan nu gå vidare till ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-151">You can now move onto additional topics.</span></span>  <span data-ttu-id="ee6bd-152">Du kanske vill tootry:</span><span class="sxs-lookup"><span data-stu-id="ee6bd-152">You may want tootry:</span></span>

[<span data-ttu-id="ee6bd-153">Anropa ett webb-API från ett webbprogram >></span><span class="sxs-lookup"><span data-stu-id="ee6bd-153">Calling a Web API from a Web App >></span></span>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

<span data-ttu-id="ee6bd-154">För ytterligare resurser, kolla:</span><span class="sxs-lookup"><span data-stu-id="ee6bd-154">For additional resources, check out:</span></span>

* [<span data-ttu-id="ee6bd-155">Utvecklarhandbok för hello v2.0 >></span><span class="sxs-lookup"><span data-stu-id="ee6bd-155">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="ee6bd-156">StackOverflow ”azure-active-directory” taggen >></span><span class="sxs-lookup"><span data-stu-id="ee6bd-156">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="ee6bd-157">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="ee6bd-157">Get security updates for our products</span></span>
<span data-ttu-id="ee6bd-158">Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ee6bd-158">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

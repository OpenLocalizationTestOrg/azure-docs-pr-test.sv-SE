---
title: "Lägga till inloggning i en .NET MVC-webb-API med hjälp av Azure AD v2.0-slutpunkten | Microsoft Docs"
description: "Hur du skapar en .NET MVC webb-Api som accepterar token från både personliga Account och arbets-eller skolkonton."
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
ms.openlocfilehash: b2d7bbfcd9218698f71e9dfdb1ad5d9ff8740f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="secure-an-mvc-web-api"></a><span data-ttu-id="71442-103">Skydda en MVC-webb-API</span><span class="sxs-lookup"><span data-stu-id="71442-103">Secure an MVC web API</span></span>
<span data-ttu-id="71442-104">Med Azure Active Directory v2.0-slutpunkten kan du skydda ett webb-API med hjälp av [OAuth 2.0](active-directory-v2-protocols.md) åtkomst-token så att användarna med både personliga Microsoft-konto och arbets- eller skolkonto konton för säker åtkomst webb-API.</span><span class="sxs-lookup"><span data-stu-id="71442-104">With Azure Active Directory the v2.0 endpoint, you can protect a Web API using [OAuth 2.0](active-directory-v2-protocols.md) access tokens, enabling users with both personal Microsoft account and work or school accounts to securely access your Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="71442-105">Inte alla Azure Active Directory-scenarier och funktioner som stöds av v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="71442-105">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="71442-106">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="71442-106">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

<span data-ttu-id="71442-107">I ASP.NET webb-API: er, kan du göra detta med hjälp av Microsofts OWIN mellanprogram som ingår i .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="71442-107">In ASP.NET web APIs, you can accomplish this using Microsoft’s OWIN middleware included in .NET Framework 4.5.</span></span>  <span data-ttu-id="71442-108">Vi använder här OWIN för att skapa en ”att göra-lista” MVC webb-API som tillåter klienter att skapa och läsa uppgifter från en användares att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="71442-108">Here we’ll use OWIN to build a "To Do List" MVC Web API that allows clients to create and read tasks from a user's To-Do list.</span></span>  <span data-ttu-id="71442-109">Webb-API verifierar att inkommande begäranden innehåller en giltig åtkomst-token och avvisa alla begäranden som inte klarar valideringen på en skyddad väg.</span><span class="sxs-lookup"><span data-stu-id="71442-109">The web API will validate that incoming requests contain a valid access token and reject any requests that do not pass validation on a protected route.</span></span>  <span data-ttu-id="71442-110">Det här exemplet har skapats med Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="71442-110">This sample was built using Visual Studio 2015.</span></span>

## <a name="download"></a><span data-ttu-id="71442-111">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="71442-111">Download</span></span>
<span data-ttu-id="71442-112">Koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span><span class="sxs-lookup"><span data-stu-id="71442-112">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span></span>  <span data-ttu-id="71442-113">Om du vill följa med kan du [ladda ned appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) eller klona stommen:</span><span class="sxs-lookup"><span data-stu-id="71442-113">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

<span data-ttu-id="71442-114">Appen stommen innehåller formaterad exempelkod för ett enkelt API, men saknar alla identitetsrelaterade bitar.</span><span class="sxs-lookup"><span data-stu-id="71442-114">The skeleton app includes all the boilerplate code for a simple API, but is missing all of the identity-related pieces.</span></span> <span data-ttu-id="71442-115">Om du inte vill att följa instruktionerna du i stället klona eller [hämta det slutförda exemplet](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="71442-115">If you don't want to follow along, you can instead clone or [download the completed sample](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span></span>

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="71442-116">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="71442-116">Register an app</span></span>
<span data-ttu-id="71442-117">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="71442-117">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="71442-118">Se till att:</span><span class="sxs-lookup"><span data-stu-id="71442-118">Make sure to:</span></span>

* <span data-ttu-id="71442-119">Kopiera den **program-Id** tilldelats din app måste den snart.</span><span class="sxs-lookup"><span data-stu-id="71442-119">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>

<span data-ttu-id="71442-120">Visual studio-lösning innehåller också en ”TodoListClient”, vilket är en enkel WPF-program.</span><span class="sxs-lookup"><span data-stu-id="71442-120">This visual studio solution also contains a "TodoListClient", which is a simple WPF app.</span></span>  <span data-ttu-id="71442-121">TodoListClient används för att demonstrera hur en användare loggar in och hur en klient kan skicka begäranden till ditt webb-API.</span><span class="sxs-lookup"><span data-stu-id="71442-121">The TodoListClient is used to demonstrate how a user signs-in and how a client can issue requests to your Web API.</span></span>  <span data-ttu-id="71442-122">I det här fallet representeras både TodoListClient och TodoListService av samma app.</span><span class="sxs-lookup"><span data-stu-id="71442-122">In this case, both the TodoListClient and the TodoListService are represented by the same app.</span></span>  <span data-ttu-id="71442-123">Om du vill konfigurera TodoListClient, bör du också:</span><span class="sxs-lookup"><span data-stu-id="71442-123">To configure the TodoListClient, you should also:</span></span>

* <span data-ttu-id="71442-124">Lägg till den **Mobile** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="71442-124">Add the **Mobile** platform for your app.</span></span>

## <a name="install-owin"></a><span data-ttu-id="71442-125">Installera OWIN</span><span class="sxs-lookup"><span data-stu-id="71442-125">Install OWIN</span></span>
<span data-ttu-id="71442-126">Nu när du har registrerat en app som du behöver konfigurera din app att kommunicera med v2.0-slutpunkten för att kunna verifiera inkommande förfrågningar och token.</span><span class="sxs-lookup"><span data-stu-id="71442-126">Now that you’ve registered an app, you need to set up your app to communicate with the v2.0 endpoint in order to validate incoming requests & tokens.</span></span>

* <span data-ttu-id="71442-127">För att börja, öppna lösningen och Lägg till NuGet-paket OWIN mellanprogram TodoListService projektet med Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="71442-127">To begin, open the solution and add the OWIN middleware NuGet packages to the TodoListService project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a><span data-ttu-id="71442-128">Konfigurera OAuth-autentisering</span><span class="sxs-lookup"><span data-stu-id="71442-128">Configure OAuth authentication</span></span>
* <span data-ttu-id="71442-129">Lägg till en OWIN-startklass i TodoListService-projektet som kallas `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="71442-129">Add an OWIN Startup class to the TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="71442-130">Högerklicka på projektet--> **Lägg till** --> **nytt objekt** --> Sök efter ”OWIN”.</span><span class="sxs-lookup"><span data-stu-id="71442-130">Right click on the project --> **Add** --> **New Item** --> Search for “OWIN”.</span></span>  <span data-ttu-id="71442-131">OWIN-mellanprogrammet anropar `Configuration(…)`-metoden när appen startas.</span><span class="sxs-lookup"><span data-stu-id="71442-131">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>
* <span data-ttu-id="71442-132">Ändra klassdeklarationen till `public partial class Startup` -vi har implementerat en del av den här klassen som du redan i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="71442-132">Change the class declaration to `public partial class Startup` - we’ve already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="71442-133">I den `Configuration(…)` metod, gör ett anrop till ConfgureAuth(...) du konfigurerar autentisering för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="71442-133">In the `Configuration(…)` method, make a call to ConfgureAuth(…) to set up authentication for your web app.</span></span>

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* <span data-ttu-id="71442-134">Öppna filen `App_Start\Startup.Auth.cs` och genomföra den `ConfigureAuth(…)` metod som ställer in webb-API för att acceptera token från v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="71442-134">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(…)` method, which will set up the Web API to accept tokens from the v2.0 endpoint.</span></span>

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* <span data-ttu-id="71442-135">Nu kan du använda `[Authorize]` attribut för att skydda dina domänkontrollanter och åtgärder med OAuth 2.0-ägar-autentisering.</span><span class="sxs-lookup"><span data-stu-id="71442-135">Now you can use `[Authorize]` attributes to protect your controllers and actions with OAuth 2.0 bearer authentication.</span></span>  <span data-ttu-id="71442-136">Skapa snygga den `Controllers\TodoListController.cs` klass med en auktorisera-tagg.</span><span class="sxs-lookup"><span data-stu-id="71442-136">Decorate the `Controllers\TodoListController.cs` class with an authorize tag.</span></span>  <span data-ttu-id="71442-137">Detta tvingar användaren att logga in innan sidan.</span><span class="sxs-lookup"><span data-stu-id="71442-137">This will force the user to sign in before accessing that page.</span></span>

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* <span data-ttu-id="71442-138">När en auktoriserad anroparen har anropar någon av de `TodoListController` API: er, åtgärden kanske behöver åtkomst till information om anroparen.</span><span class="sxs-lookup"><span data-stu-id="71442-138">When an authorized caller successfully invokes one of the `TodoListController` APIs, the action might need access to information about the caller.</span></span>  <span data-ttu-id="71442-139">OWIN ger tillgång till anspråk i ägartoken via den `ClaimsPrincpal` objekt.</span><span class="sxs-lookup"><span data-stu-id="71442-139">OWIN provides access to the claims inside the bearer token via the `ClaimsPrincpal` object.</span></span>  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* <span data-ttu-id="71442-140">Slutligen, öppna den `web.config` filen i roten av projektet TodoListService och ange dina konfigurationsvärden i den `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="71442-140">Finally, open the `web.config` file in the root of the TodoListService project, and enter your configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="71442-141">Din `ida:Audience` är den **program-Id** för den app som du angav på portalen.</span><span class="sxs-lookup"><span data-stu-id="71442-141">Your `ida:Audience` is the **Application Id** of the app that you entered in the portal.</span></span>

## <a name="configure-the-client-app"></a><span data-ttu-id="71442-142">Konfigurera klientappen</span><span class="sxs-lookup"><span data-stu-id="71442-142">Configure the client app</span></span>
<span data-ttu-id="71442-143">Innan du kan se tjänsten Todo i åtgärden måste du konfigurera klienten för Todo-listan så att den kan hämta token från v2.0-slutpunkten och anrop till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="71442-143">Before you can see the Todo List Service in action, you need to configure the Todo List Client so it can get tokens from the v2.0 endpoint and make calls to the service.</span></span>

* <span data-ttu-id="71442-144">Öppna i projektet TodoListClient `App.config` och ange dina konfigurationsvärden i den `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="71442-144">In the TodoListClient project, open `App.config` and enter your configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="71442-145">Din `ida:ClientId` program-Id som du kopierade från portalen.</span><span class="sxs-lookup"><span data-stu-id="71442-145">Your `ida:ClientId` Application Id you copied from the portal.</span></span>

<span data-ttu-id="71442-146">Slutligen, rensa, skapa och köra varje projekt!</span><span class="sxs-lookup"><span data-stu-id="71442-146">Finally, clean, build and run each project!</span></span>  <span data-ttu-id="71442-147">Nu har du en .NET MVC webb-API som accepterar token från både personliga Microsoft-konton och arbets-eller skolkonton.</span><span class="sxs-lookup"><span data-stu-id="71442-147">You now have a .NET MVC Web API that accepts tokens from both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="71442-148">Logga in på TodoListClient och anropa dina webb-api för att lägga till aktiviteter i användarens att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="71442-148">Sign into the TodoListClient, and call your web api to add tasks to the user's To-Do list.</span></span>

<span data-ttu-id="71442-149">För referens anger det slutförda exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), eller kan du klona den från GitHub:</span><span class="sxs-lookup"><span data-stu-id="71442-149">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="71442-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71442-150">Next steps</span></span>
<span data-ttu-id="71442-151">Du kan nu gå vidare till ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="71442-151">You can now move onto additional topics.</span></span>  <span data-ttu-id="71442-152">Du kanske vill prova:</span><span class="sxs-lookup"><span data-stu-id="71442-152">You may want to try:</span></span>

[<span data-ttu-id="71442-153">Anropa ett webb-API från ett webbprogram >></span><span class="sxs-lookup"><span data-stu-id="71442-153">Calling a Web API from a Web App >></span></span>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

<span data-ttu-id="71442-154">För ytterligare resurser, kolla:</span><span class="sxs-lookup"><span data-stu-id="71442-154">For additional resources, check out:</span></span>

* [<span data-ttu-id="71442-155">Utvecklarhandbok v2.0 >></span><span class="sxs-lookup"><span data-stu-id="71442-155">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="71442-156">StackOverflow ”azure-active-directory” taggen >></span><span class="sxs-lookup"><span data-stu-id="71442-156">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="71442-157">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="71442-157">Get security updates for our products</span></span>
<span data-ttu-id="71442-158">Vi rekommenderar att du aktiverar aviseringar om säkerhetsincidenter genom att gå till [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera på Microsoft Security Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="71442-158">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

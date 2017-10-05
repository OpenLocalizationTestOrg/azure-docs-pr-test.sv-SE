---
title: Azure AD B2C | Microsoft Docs
description: "Skapa ett .NET-webb-API med Azure Active Directory B2C som skyddas med hjälp av OAuth 2.0-åtkomsttoken för autentisering."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 48749bfa2ab54a0e766a4aad4f39073cc4e90818
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a><span data-ttu-id="e64d1-103">Azure Active Directory B2C: Skapa ett .NET-webb-API</span><span class="sxs-lookup"><span data-stu-id="e64d1-103">Azure Active Directory B2C: Build a .NET web API</span></span>

<span data-ttu-id="e64d1-104">Med Azure Active Directory (Active AD) B2C kan du skydda ett webb-API med hjälp av OAuth 2.0-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="e64d1-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="e64d1-105">Dessa token gör att dina klientappar kan autentisera mot API:et.</span><span class="sxs-lookup"><span data-stu-id="e64d1-105">These tokens allow your client apps to authenticate to the API.</span></span> <span data-ttu-id="e64d1-106">Den här artikeln beskriver hur du skapar ett .NET MVC-baserat API för en att göra-lista som låter användare av ditt klientprogram köra CRUD-uppgifter.</span><span class="sxs-lookup"><span data-stu-id="e64d1-106">This article shows you how to create a .NET MVC "to-do list" API that allows users of your client application to CRUD tasks.</span></span> <span data-ttu-id="e64d1-107">Webb-API:t skyddas med hjälp av Azure AD B2C och tillåter endast autentiserade användare att hantera sina att göra-listor.</span><span class="sxs-lookup"><span data-stu-id="e64d1-107">The web API is secured using Azure AD B2C and only allows authenticated users to manage their to-do list.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="e64d1-108">Skapa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="e64d1-108">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="e64d1-109">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="e64d1-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="e64d1-110">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="e64d1-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="e64d1-111">Om du inte redan har en B2C-katalog [skapar du en](active-directory-b2c-get-started.md) innan du fortsätter den här guiden.</span><span class="sxs-lookup"><span data-stu-id="e64d1-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

> [!NOTE]
> <span data-ttu-id="e64d1-112">Klientprogrammet och webb-API:et måste använda samma Azure AD B2C-katalog.</span><span class="sxs-lookup"><span data-stu-id="e64d1-112">The client application and web API must use the same Azure AD B2C directory.</span></span>
>

## <a name="create-a-web-api"></a><span data-ttu-id="e64d1-113">Skapa ett webb-API</span><span class="sxs-lookup"><span data-stu-id="e64d1-113">Create a web API</span></span>

<span data-ttu-id="e64d1-114">Därefter måste du skapa en webb-API-app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="e64d1-114">Next, you need to create a web API app in your B2C directory.</span></span> <span data-ttu-id="e64d1-115">Det ger Azure AD den information som tjänsten behöver för att kommunicera säkert med din app.</span><span class="sxs-lookup"><span data-stu-id="e64d1-115">This gives Azure AD information that it needs to securely communicate with your app.</span></span> <span data-ttu-id="e64d1-116">Du skapar en app genom att följa [dessa anvisningar](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="e64d1-116">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="e64d1-117">Se till att:</span><span class="sxs-lookup"><span data-stu-id="e64d1-117">Be sure to:</span></span>

* <span data-ttu-id="e64d1-118">Lägga till en **webbapp** eller ett **webb-API** i programmet.</span><span class="sxs-lookup"><span data-stu-id="e64d1-118">Include a **web app** or **web API** in the application.</span></span>
* <span data-ttu-id="e64d1-119">Använd **omdirigerings-URI:n** `https://localhost:44332/` för webbappen.</span><span class="sxs-lookup"><span data-stu-id="e64d1-119">Use the **Redirect URI** `https://localhost:44332/` for the web app.</span></span> <span data-ttu-id="e64d1-120">Det här är standardplatsen för klienten för webbappen i det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="e64d1-120">This is the default location of the web app client for this code sample.</span></span>
* <span data-ttu-id="e64d1-121">Kopiera **program-ID:t** som har tilldelats din app.</span><span class="sxs-lookup"><span data-stu-id="e64d1-121">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="e64d1-122">Du behöver det senare.</span><span class="sxs-lookup"><span data-stu-id="e64d1-122">You'll need it later.</span></span>
* <span data-ttu-id="e64d1-123">Ange en appidentifierare i **App-ID-URI**.</span><span class="sxs-lookup"><span data-stu-id="e64d1-123">Enter an app identifier into **App ID URI**.</span></span>
* <span data-ttu-id="e64d1-124">Lägg till behörigheter via menyn med **publicerade omfång**.</span><span class="sxs-lookup"><span data-stu-id="e64d1-124">Add permissions through the **Published scopes** menu.</span></span>

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="e64d1-125">Skapa dina principer</span><span class="sxs-lookup"><span data-stu-id="e64d1-125">Create your policies</span></span>

<span data-ttu-id="e64d1-126">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="e64d1-126">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="e64d1-127">Du måste skapa en princip för att kommunicera med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="e64d1-127">You will need to create a policy to communicate with Azure AD B2C.</span></span> <span data-ttu-id="e64d1-128">Vi rekommenderar att du använder den kombinerade registrerings- och inloggningsprincipen som beskrivs i [referensartikeln för principer](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="e64d1-128">We recommend using the combined sign-up/sign-in policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="e64d1-129">Se till att göra följande när du skapar en princip:</span><span class="sxs-lookup"><span data-stu-id="e64d1-129">When you create your policy, be sure to:</span></span>

* <span data-ttu-id="e64d1-130">Välj **Visningsnamn** och andra registreringsattribut i principen.</span><span class="sxs-lookup"><span data-stu-id="e64d1-130">Choose **Display name** and other sign-up attributes in your policy.</span></span>
* <span data-ttu-id="e64d1-131">Välj **Visningsnamn** och **Objekt-ID** som programanspråk för varje princip.</span><span class="sxs-lookup"><span data-stu-id="e64d1-131">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="e64d1-132">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="e64d1-132">You can choose other claims as well.</span></span>
* <span data-ttu-id="e64d1-133">Kopiera **namnet** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="e64d1-133">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="e64d1-134">Du behöver principnamnet senare.</span><span class="sxs-lookup"><span data-stu-id="e64d1-134">You'll need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="e64d1-135">När du har skapat principen är det dags att skapa appen.</span><span class="sxs-lookup"><span data-stu-id="e64d1-135">After you have successfully created the policy, you're ready to build your app.</span></span>

## <a name="download-the-code"></a><span data-ttu-id="e64d1-136">Ladda ned koden</span><span class="sxs-lookup"><span data-stu-id="e64d1-136">Download the code</span></span>

<span data-ttu-id="e64d1-137">Koden för den här självstudiekursen finns på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="e64d1-137">The code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="e64d1-138">Du kan klona exemplet genom att köra:</span><span class="sxs-lookup"><span data-stu-id="e64d1-138">You can clone the sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="e64d1-139">När du har laddat ned exempelkoden öppnar du SLN-filen i Visual Studio för att sätta igång.</span><span class="sxs-lookup"><span data-stu-id="e64d1-139">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="e64d1-140">Lösningsfilen innehåller två projekt: `TaskWebApp` och `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="e64d1-140">The solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="e64d1-141">`TaskWebApp`är en MVC-webbapp som användaren interagerar med.</span><span class="sxs-lookup"><span data-stu-id="e64d1-141">`TaskWebApp` is an MVC web application that the user interacts with.</span></span> <span data-ttu-id="e64d1-142">`TaskService` är appens backend-webb-API som lagrar varje användares att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="e64d1-142">`TaskService` is the app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="e64d1-143">I den här artikeln beskrivs bara `TaskService`-programmet.</span><span class="sxs-lookup"><span data-stu-id="e64d1-143">This article will only discuss the `TaskService` application.</span></span> <span data-ttu-id="e64d1-144">Information om hur du skapar `TaskWebApp` med hjälp av Azure AD B2C finns i [självstudiekursen för .NET-webbappar](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="e64d1-144">To learn how to build `TaskWebApp` using Azure AD B2C, see [our .NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>

### <a name="update-the-azure-ad-b2c-configuration"></a><span data-ttu-id="e64d1-145">Uppdatera Azure AD B2C-konfigurationen</span><span class="sxs-lookup"><span data-stu-id="e64d1-145">Update the Azure AD B2C configuration</span></span>

<span data-ttu-id="e64d1-146">Det här exemplet är konfigurerat att använda principerna och klient-ID:t för vår demoklientorganisation.</span><span class="sxs-lookup"><span data-stu-id="e64d1-146">Our sample is configured to use the policies and client ID of our demo tenant.</span></span> <span data-ttu-id="e64d1-147">Om du vill använda din egen klientorganisation måste du göra följande:</span><span class="sxs-lookup"><span data-stu-id="e64d1-147">If you would like to use your own tenant, you will need to do the following:</span></span>

1. <span data-ttu-id="e64d1-148">Öppna `web.config` i `TaskService`-projektet och ersätt värdena för</span><span class="sxs-lookup"><span data-stu-id="e64d1-148">Open `web.config` in the `TaskService` project and replace the values for</span></span>
    * <span data-ttu-id="e64d1-149">`ida:Tenant` med namnet på din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="e64d1-149">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="e64d1-150">`ida:ClientId` med program-ID:t för ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="e64d1-150">`ida:ClientId` with your web API application ID</span></span>
    * <span data-ttu-id="e64d1-151">`ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip</span><span class="sxs-lookup"><span data-stu-id="e64d1-151">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="e64d1-152">Öppna `web.config` i `TaskWebApp`-projektet och ersätt värdena för</span><span class="sxs-lookup"><span data-stu-id="e64d1-152">Open `web.config` in the `TaskWebApp` project and replace the values for</span></span>
    * <span data-ttu-id="e64d1-153">`ida:Tenant` med namnet på din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="e64d1-153">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="e64d1-154">`ida:ClientId` med program-ID:t för din webbapp</span><span class="sxs-lookup"><span data-stu-id="e64d1-154">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="e64d1-155">`ida:ClientSecret` med den hemliga nyckeln för din webbapp</span><span class="sxs-lookup"><span data-stu-id="e64d1-155">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="e64d1-156">`ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip</span><span class="sxs-lookup"><span data-stu-id="e64d1-156">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="e64d1-157">`ida:EditProfilePolicyId` med namnet på din profilredigeringsprincip</span><span class="sxs-lookup"><span data-stu-id="e64d1-157">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="e64d1-158">`ida:ResetPasswordPolicyId` med namnet på din lösenordsåterställningsprincip</span><span class="sxs-lookup"><span data-stu-id="e64d1-158">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>


## <a name="secure-the-api"></a><span data-ttu-id="e64d1-159">Skydda API:et</span><span class="sxs-lookup"><span data-stu-id="e64d1-159">Secure the API</span></span>

<span data-ttu-id="e64d1-160">Om du har en klient som anropar ditt API kan du skydda API:et (t.ex `TaskService`) med hjälp av OAuth 2.0-ägartoken.</span><span class="sxs-lookup"><span data-stu-id="e64d1-160">When you have a client that calls your API, you can secure your API (e.g `TaskService`) by using OAuth 2.0 bearer tokens.</span></span> <span data-ttu-id="e64d1-161">Detta säkerställer att varje begäran till API:et endast är giltig om begäran har en ägartoken.</span><span class="sxs-lookup"><span data-stu-id="e64d1-161">This ensures that each request to your API will only be valid if the request has a bearer token.</span></span> <span data-ttu-id="e64d1-162">API:et kan acceptera och validera ägartoken med hjälp av Microsofts OWIN-bibliotek (Open Web Interface för .NET).</span><span class="sxs-lookup"><span data-stu-id="e64d1-162">Your API can accept and validate bearer tokens by using Microsoft's Open Web Interface for .NET (OWIN) library.</span></span>

### <a name="install-owin"></a><span data-ttu-id="e64d1-163">Installera OWIN</span><span class="sxs-lookup"><span data-stu-id="e64d1-163">Install OWIN</span></span>

<span data-ttu-id="e64d1-164">Börja med att installera OWIN OAuth-autentiseringspipelinen med hjälp av Visual Studio Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="e64d1-164">Begin by installing the OWIN OAuth authentication pipeline by using the Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

<span data-ttu-id="e64d1-165">När du gör det installeras OWIN-mellanprogrammet som accepterar och validerar ägartoken.</span><span class="sxs-lookup"><span data-stu-id="e64d1-165">This will install the OWIN middleware that will accept and validate bearer tokens.</span></span>

### <a name="add-an-owin-startup-class"></a><span data-ttu-id="e64d1-166">Lägga till en OWIN-startklass</span><span class="sxs-lookup"><span data-stu-id="e64d1-166">Add an OWIN startup class</span></span>

<span data-ttu-id="e64d1-167">Lägg till en OWIN-startklass i API:et med namnet `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="e64d1-167">Add an OWIN startup class to the API called `Startup.cs`.</span></span>  <span data-ttu-id="e64d1-168">Högerklicka på projektet, välj **Lägg till** och **Nytt objekt** och sök sedan efter OWIN.</span><span class="sxs-lookup"><span data-stu-id="e64d1-168">Right-click on the project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="e64d1-169">OWIN-mellanprogrammet anropar `Configuration(…)`-metoden när appen startas.</span><span class="sxs-lookup"><span data-stu-id="e64d1-169">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="e64d1-170">I vårt exempel ändrade vi klassdeklarationen till `public partial class Startup` och implementerade den andra delen av klassen i `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="e64d1-170">In our sample, we changed the class declaration to `public partial class Startup` and implemented the other part of the class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="e64d1-171">I `Configuration`-metoden lade vi till ett anrop till `ConfigureAuth`, som definieras i `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="e64d1-171">Inside the `Configuration` method, we added a call to `ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="e64d1-172">Efter ändringarna ser `Startup.cs` ut så här:</span><span class="sxs-lookup"><span data-stu-id="e64d1-172">After the modifications, `Startup.cs` looks like the following:</span></span>

```CSharp
// Startup.cs

public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of the class
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a><span data-ttu-id="e64d1-173">Konfigurera OAuth 2.0-autentisering</span><span class="sxs-lookup"><span data-stu-id="e64d1-173">Configure OAuth 2.0 authentication</span></span>

<span data-ttu-id="e64d1-174">Öppna filen `App_Start\Startup.Auth.cs` och implementera `ConfigureAuth(...)`-metoden.</span><span class="sxs-lookup"><span data-stu-id="e64d1-174">Open the file `App_Start\Startup.Auth.cs`, and implement the `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="e64d1-175">Det kan till exempel se ut så här:</span><span class="sxs-lookup"><span data-stu-id="e64d1-175">For example, it could look like the following:</span></span>

```CSharp
// App_Start\Startup.Auth.cs

 public partial class Startup
    {
        // These values are pulled from web.config
        public static string AadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
        public static string Tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        public static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
        public static string SignUpSignInPolicy = ConfigurationManager.AppSettings["ida:SignUpSignInPolicyId"];
        public static string DefaultPolicy = SignUpSignInPolicy;

        /*
         * Configure the authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where the audience of the token is equal to the client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-the-task-controller"></a><span data-ttu-id="e64d1-176">Skydda uppgiftskontrollanten</span><span class="sxs-lookup"><span data-stu-id="e64d1-176">Secure the task controller</span></span>

<span data-ttu-id="e64d1-177">När appen har konfigurerats att använda OAuth 2.0-autentisering kan du skydda webb-API:et genom att lägga till en `[Authorize]`-tagg i uppgiftskontrollanten.</span><span class="sxs-lookup"><span data-stu-id="e64d1-177">After the app is configured to use OAuth 2.0 authentication, you can secure your web API by adding an `[Authorize]` tag to the task controller.</span></span> <span data-ttu-id="e64d1-178">Det här är domänkontrollanten där all manipulering av att göra-listan äger rum. Därför bör du skydda hela kontrollanten på klassnivå.</span><span class="sxs-lookup"><span data-stu-id="e64d1-178">This is the controller where all to-do list manipulation takes place, so you should secure the entire controller at the class level.</span></span> <span data-ttu-id="e64d1-179">Du kan också lägga till `[Authorize]`-taggen för enskilda åtgärder för en mer detaljerad kontroll.</span><span class="sxs-lookup"><span data-stu-id="e64d1-179">You can also add the `[Authorize]` tag to individual actions for more fine-grained control.</span></span>

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a><span data-ttu-id="e64d1-180">Hämta användarinformation från token</span><span class="sxs-lookup"><span data-stu-id="e64d1-180">Get user information from the token</span></span>

<span data-ttu-id="e64d1-181">`TasksController` lagrar uppgifter i en databas där varje uppgift har en associerad användare som ”äger” uppgiften.</span><span class="sxs-lookup"><span data-stu-id="e64d1-181">`TasksController` stores tasks in a database where each task has an associated user who "owns" the task.</span></span> <span data-ttu-id="e64d1-182">Ägaren identifieras med hjälp av användarens **objekt-ID**.</span><span class="sxs-lookup"><span data-stu-id="e64d1-182">The owner is identified by the user's **object ID**.</span></span> <span data-ttu-id="e64d1-183">(Det är därför du måste lägga till objekt-ID:t som ett programanspråk i alla dina principer.)</span><span class="sxs-lookup"><span data-stu-id="e64d1-183">(This is why you needed to add the object ID as an application claim in all of your policies.)</span></span>

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-the-permissions-in-the-token"></a><span data-ttu-id="e64d1-184">Validera behörigheterna i token</span><span class="sxs-lookup"><span data-stu-id="e64d1-184">Validate the permissions in the token</span></span>

<span data-ttu-id="e64d1-185">Ett vanligt krav för webb-API:er är att validera ”omfång” i token.</span><span class="sxs-lookup"><span data-stu-id="e64d1-185">A common requirement for web APIs is to validate the "scopes" present in the token.</span></span> <span data-ttu-id="e64d1-186">Detta säkerställer att användaren har samtyckt till behörigheterna som krävs för att komma åt tjänsten för att göra-listan.</span><span class="sxs-lookup"><span data-stu-id="e64d1-186">This ensures that the user has consented to the permissions required to access the to-do list service.</span></span>

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "The Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-the-sample-app"></a><span data-ttu-id="e64d1-187">Kör exempelappen</span><span class="sxs-lookup"><span data-stu-id="e64d1-187">Run the sample app</span></span>

<span data-ttu-id="e64d1-188">Slutligen bygger du och kör både `TaskWebApp` och `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="e64d1-188">Finally, build and run both `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="e64d1-189">Skapa några uppgifter i användarens att göra-lista och notera hur de finns kvar i API:et även om du stoppar och startar om klienten.</span><span class="sxs-lookup"><span data-stu-id="e64d1-189">Create some tasks on the user's to-do list and notice how they are persisted in the API even after you stop and restart the client.</span></span>

## <a name="edit-your-policies"></a><span data-ttu-id="e64d1-190">Redigera dina principer</span><span class="sxs-lookup"><span data-stu-id="e64d1-190">Edit your policies</span></span>

<span data-ttu-id="e64d1-191">När du har skyddat ett API med hjälp av Azure AD B2C kan du experimentera med registrerings- och inloggningsprincipen och se effekterna (eller avsaknaden av dem) i API:et.</span><span class="sxs-lookup"><span data-stu-id="e64d1-191">After you have secured an API by using Azure AD B2C, you can experiment with your Sign-in/Sign-up policy and view the effects (or lack thereof) on the API.</span></span> <span data-ttu-id="e64d1-192">Du kan manipulera programanspråken i principerna och ändra användarinformationen som är tillgänglig i webb-API:et.</span><span class="sxs-lookup"><span data-stu-id="e64d1-192">You can manipulate the application claims in the policies and change the user information that is available in the web API.</span></span> <span data-ttu-id="e64d1-193">Eventuella anspråk som du lägger till är tillgängliga för det .NET MVC-baserade webb-API:et i `ClaimsPrincipal`-objektet på det sätt som beskrivits tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e64d1-193">Any claims that you add will be available to your .NET MVC web API in the `ClaimsPrincipal` object, as described earlier in this article.</span></span>

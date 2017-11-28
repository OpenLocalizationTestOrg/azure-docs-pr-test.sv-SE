---
title: aaaAzure AD B2C | Microsoft Docs
description: "Hur toobuild en .NET-webb-API med hjälp av Azure Active Directory B2C kan skyddas med OAuth 2.0-åtkomsttoken för autentisering."
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
ms.openlocfilehash: d45364216deda38ef44b60dd11e86d9a089ad509
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a><span data-ttu-id="3b181-103">Azure Active Directory B2C: Skapa ett .NET-webb-API</span><span class="sxs-lookup"><span data-stu-id="3b181-103">Azure Active Directory B2C: Build a .NET web API</span></span>

<span data-ttu-id="3b181-104">Med Azure Active Directory (Active AD) B2C kan du skydda ett webb-API med hjälp av OAuth 2.0-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="3b181-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="3b181-105">Dessa token kan klienten appar tooauthenticate toohello API.</span><span class="sxs-lookup"><span data-stu-id="3b181-105">These tokens allow your client apps tooauthenticate toohello API.</span></span> <span data-ttu-id="3b181-106">Den här artikeln visar hur toocreate en .NET MVC ”uppgiftslistan” API som kan användare med ditt program tooCRUD uppgifter.</span><span class="sxs-lookup"><span data-stu-id="3b181-106">This article shows you how toocreate a .NET MVC "to-do list" API that allows users of your client application tooCRUD tasks.</span></span> <span data-ttu-id="3b181-107">hello web API skyddas med Azure AD B2C och tillåter endast autentiserade användare toomanage sina att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="3b181-107">hello web API is secured using Azure AD B2C and only allows authenticated users toomanage their to-do list.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="3b181-108">Skapa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="3b181-108">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="3b181-109">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="3b181-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="3b181-110">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="3b181-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="3b181-111">Om du inte redan har en B2C-katalog [skapar du en](active-directory-b2c-get-started.md) innan du fortsätter den här guiden.</span><span class="sxs-lookup"><span data-stu-id="3b181-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

> [!NOTE]
> <span data-ttu-id="3b181-112">hello klientprogrammet och webb-API måste använda hello samma Azure AD B2C-katalog.</span><span class="sxs-lookup"><span data-stu-id="3b181-112">hello client application and web API must use hello same Azure AD B2C directory.</span></span>
>

## <a name="create-a-web-api"></a><span data-ttu-id="3b181-113">Skapa ett webb-API</span><span class="sxs-lookup"><span data-stu-id="3b181-113">Create a web API</span></span>

<span data-ttu-id="3b181-114">Sedan måste toocreate en web API-app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="3b181-114">Next, you need toocreate a web API app in your B2C directory.</span></span> <span data-ttu-id="3b181-115">Det ger Azure AD den information som den behöver toosecurely kommunicera med din app.</span><span class="sxs-lookup"><span data-stu-id="3b181-115">This gives Azure AD information that it needs toosecurely communicate with your app.</span></span> <span data-ttu-id="3b181-116">toocreate en app, Följ [instruktionerna](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="3b181-116">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="3b181-117">Se till att:</span><span class="sxs-lookup"><span data-stu-id="3b181-117">Be sure to:</span></span>

* <span data-ttu-id="3b181-118">Inkludera en **webbapp** eller **webb-API** i hello program.</span><span class="sxs-lookup"><span data-stu-id="3b181-118">Include a **web app** or **web API** in hello application.</span></span>
* <span data-ttu-id="3b181-119">Använd hello **omdirigerings-URI** `https://localhost:44332/` för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3b181-119">Use hello **Redirect URI** `https://localhost:44332/` for hello web app.</span></span> <span data-ttu-id="3b181-120">Detta är hello standardplatsen för hello klienten för webbappen i det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="3b181-120">This is hello default location of hello web app client for this code sample.</span></span>
* <span data-ttu-id="3b181-121">Kopiera hello **program-ID** som är tilldelade tooyour app.</span><span class="sxs-lookup"><span data-stu-id="3b181-121">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="3b181-122">Du behöver det senare.</span><span class="sxs-lookup"><span data-stu-id="3b181-122">You'll need it later.</span></span>
* <span data-ttu-id="3b181-123">Ange en appidentifierare i **App-ID-URI**.</span><span class="sxs-lookup"><span data-stu-id="3b181-123">Enter an app identifier into **App ID URI**.</span></span>
* <span data-ttu-id="3b181-124">Lägg till behörigheter via hello **publicerade scope** menyn.</span><span class="sxs-lookup"><span data-stu-id="3b181-124">Add permissions through hello **Published scopes** menu.</span></span>

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="3b181-125">Skapa dina principer</span><span class="sxs-lookup"><span data-stu-id="3b181-125">Create your policies</span></span>

<span data-ttu-id="3b181-126">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="3b181-126">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="3b181-127">Du behöver toocreate en princip toocommunicate med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="3b181-127">You will need toocreate a policy toocommunicate with Azure AD B2C.</span></span> <span data-ttu-id="3b181-128">Vi rekommenderar med hello kombineras sign-upp/inloggningsprincip, enligt beskrivningen i hello [referensartikeln om principer](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="3b181-128">We recommend using hello combined sign-up/sign-in policy, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="3b181-129">Se till att göra följande när du skapar en princip:</span><span class="sxs-lookup"><span data-stu-id="3b181-129">When you create your policy, be sure to:</span></span>

* <span data-ttu-id="3b181-130">Välj **Visningsnamn** och andra registreringsattribut i principen.</span><span class="sxs-lookup"><span data-stu-id="3b181-130">Choose **Display name** and other sign-up attributes in your policy.</span></span>
* <span data-ttu-id="3b181-131">Välj **Visningsnamn** och **Objekt-ID** som programanspråk för varje princip.</span><span class="sxs-lookup"><span data-stu-id="3b181-131">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="3b181-132">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="3b181-132">You can choose other claims as well.</span></span>
* <span data-ttu-id="3b181-133">Kopiera hello **namn** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="3b181-133">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="3b181-134">Du behöver hello principnamn senare.</span><span class="sxs-lookup"><span data-stu-id="3b181-134">You'll need hello policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="3b181-135">När du har skapat hello princip du är klar toobuild din app.</span><span class="sxs-lookup"><span data-stu-id="3b181-135">After you have successfully created hello policy, you're ready toobuild your app.</span></span>

## <a name="download-hello-code"></a><span data-ttu-id="3b181-136">Hämta hello koden</span><span class="sxs-lookup"><span data-stu-id="3b181-136">Download hello code</span></span>

<span data-ttu-id="3b181-137">hello-koden för den här självstudiekursen underhålls på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="3b181-137">hello code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="3b181-138">Du kan klona hello exemplet genom att köra:</span><span class="sxs-lookup"><span data-stu-id="3b181-138">You can clone hello sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="3b181-139">När du har hämtat hello exempelkod igång öppna hello Visual Studio SLN-filen tooget.</span><span class="sxs-lookup"><span data-stu-id="3b181-139">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="3b181-140">hello lösningsfilen innehåller två projekt: `TaskWebApp` och `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="3b181-140">hello solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="3b181-141">`TaskWebApp`är en MVC-webbapp som hello användaren interagerar med.</span><span class="sxs-lookup"><span data-stu-id="3b181-141">`TaskWebApp` is an MVC web application that hello user interacts with.</span></span> <span data-ttu-id="3b181-142">`TaskService`är hello appens backend-webb-API som lagrar varje användares att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="3b181-142">`TaskService` is hello app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="3b181-143">Den här artikeln handlar enbart hello `TaskService` program.</span><span class="sxs-lookup"><span data-stu-id="3b181-143">This article will only discuss hello `TaskService` application.</span></span> <span data-ttu-id="3b181-144">toolearn hur toobuild `TaskWebApp` med Azure AD B2C finns i [våra självstudier för .NET web app](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="3b181-144">toolearn how toobuild `TaskWebApp` using Azure AD B2C, see [our .NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>

### <a name="update-hello-azure-ad-b2c-configuration"></a><span data-ttu-id="3b181-145">Uppdatera hello Azure AD B2C-konfiguration</span><span class="sxs-lookup"><span data-stu-id="3b181-145">Update hello Azure AD B2C configuration</span></span>

<span data-ttu-id="3b181-146">Exemplet är konfigurerade toouse hello principer och klient-ID för vår demo-klient.</span><span class="sxs-lookup"><span data-stu-id="3b181-146">Our sample is configured toouse hello policies and client ID of our demo tenant.</span></span> <span data-ttu-id="3b181-147">Om du vill att toouse en egen klient behöver du toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="3b181-147">If you would like toouse your own tenant, you will need toodo hello following:</span></span>

1. <span data-ttu-id="3b181-148">Öppna `web.config` i hello `TaskService` projektet och Ersätt hello värden för</span><span class="sxs-lookup"><span data-stu-id="3b181-148">Open `web.config` in hello `TaskService` project and replace hello values for</span></span>
    * <span data-ttu-id="3b181-149">`ida:Tenant` med namnet på din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="3b181-149">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="3b181-150">`ida:ClientId` med program-ID:t för ditt webb-API</span><span class="sxs-lookup"><span data-stu-id="3b181-150">`ida:ClientId` with your web API application ID</span></span>
    * <span data-ttu-id="3b181-151">`ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip</span><span class="sxs-lookup"><span data-stu-id="3b181-151">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="3b181-152">Öppna `web.config` i hello `TaskWebApp` projektet och Ersätt hello värden för</span><span class="sxs-lookup"><span data-stu-id="3b181-152">Open `web.config` in hello `TaskWebApp` project and replace hello values for</span></span>
    * <span data-ttu-id="3b181-153">`ida:Tenant` med namnet på din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="3b181-153">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="3b181-154">`ida:ClientId` med program-ID:t för din webbapp</span><span class="sxs-lookup"><span data-stu-id="3b181-154">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="3b181-155">`ida:ClientSecret` med den hemliga nyckeln för din webbapp</span><span class="sxs-lookup"><span data-stu-id="3b181-155">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="3b181-156">`ida:SignUpSignInPolicyId` med namnet på din registrerings- eller inloggningsprincip</span><span class="sxs-lookup"><span data-stu-id="3b181-156">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="3b181-157">`ida:EditProfilePolicyId` med namnet på din profilredigeringsprincip</span><span class="sxs-lookup"><span data-stu-id="3b181-157">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="3b181-158">`ida:ResetPasswordPolicyId` med namnet på din lösenordsåterställningsprincip</span><span class="sxs-lookup"><span data-stu-id="3b181-158">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>


## <a name="secure-hello-api"></a><span data-ttu-id="3b181-159">Skydda hello-API</span><span class="sxs-lookup"><span data-stu-id="3b181-159">Secure hello API</span></span>

<span data-ttu-id="3b181-160">Om du har en klient som anropar ditt API kan du skydda API:et (t.ex `TaskService`) med hjälp av OAuth 2.0-ägartoken.</span><span class="sxs-lookup"><span data-stu-id="3b181-160">When you have a client that calls your API, you can secure your API (e.g `TaskService`) by using OAuth 2.0 bearer tokens.</span></span> <span data-ttu-id="3b181-161">Detta säkerställer att varje begäran tooyour API bara är giltiga om hello-begäran har ett ägartoken.</span><span class="sxs-lookup"><span data-stu-id="3b181-161">This ensures that each request tooyour API will only be valid if hello request has a bearer token.</span></span> <span data-ttu-id="3b181-162">API:et kan acceptera och validera ägartoken med hjälp av Microsofts OWIN-bibliotek (Open Web Interface för .NET).</span><span class="sxs-lookup"><span data-stu-id="3b181-162">Your API can accept and validate bearer tokens by using Microsoft's Open Web Interface for .NET (OWIN) library.</span></span>

### <a name="install-owin"></a><span data-ttu-id="3b181-163">Installera OWIN</span><span class="sxs-lookup"><span data-stu-id="3b181-163">Install OWIN</span></span>

<span data-ttu-id="3b181-164">Börja med att installera hello OWIN OAuth-autentisering pipeline med hjälp av hello Visual Studio Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="3b181-164">Begin by installing hello OWIN OAuth authentication pipeline by using hello Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

<span data-ttu-id="3b181-165">Detta installerar hello OWIN mellanprogram som kommer att acceptera och validera ägar-token.</span><span class="sxs-lookup"><span data-stu-id="3b181-165">This will install hello OWIN middleware that will accept and validate bearer tokens.</span></span>

### <a name="add-an-owin-startup-class"></a><span data-ttu-id="3b181-166">Lägga till en OWIN-startklass</span><span class="sxs-lookup"><span data-stu-id="3b181-166">Add an OWIN startup class</span></span>

<span data-ttu-id="3b181-167">Lägg till en OWIN Start klassen toohello API kallas `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="3b181-167">Add an OWIN startup class toohello API called `Startup.cs`.</span></span>  <span data-ttu-id="3b181-168">Högerklicka på hello-projektet, Välj **Lägg till** och **nytt objekt**, och sök sedan efter OWIN.</span><span class="sxs-lookup"><span data-stu-id="3b181-168">Right-click on hello project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="3b181-169">Hej OWIN mellanprogram ska anropa hello `Configuration(…)` metod när appen startar.</span><span class="sxs-lookup"><span data-stu-id="3b181-169">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="3b181-170">I vårt exempel vi ändrat hello klassdeklarationen för`public partial class Startup` och implementeras hello andra delar av hello klassen i `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="3b181-170">In our sample, we changed hello class declaration too`public partial class Startup` and implemented hello other part of hello class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="3b181-171">I hello `Configuration` metod, vi har lagt till ett anrop för`ConfigureAuth`, som har definierats i `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="3b181-171">Inside hello `Configuration` method, we added a call too`ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="3b181-172">Efter hello ändringar `Startup.cs` ser ut som följande hello:</span><span class="sxs-lookup"><span data-stu-id="3b181-172">After hello modifications, `Startup.cs` looks like hello following:</span></span>

```CSharp
// Startup.cs

public partial class Startup
{
    // hello OWIN middleware will invoke this method when hello app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of hello class
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a><span data-ttu-id="3b181-173">Konfigurera OAuth 2.0-autentisering</span><span class="sxs-lookup"><span data-stu-id="3b181-173">Configure OAuth 2.0 authentication</span></span>

<span data-ttu-id="3b181-174">Öppna hello filen `App_Start\Startup.Auth.cs`, och implementera hello `ConfigureAuth(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="3b181-174">Open hello file `App_Start\Startup.Auth.cs`, and implement hello `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="3b181-175">Det kan till exempel se ut så hello följande:</span><span class="sxs-lookup"><span data-stu-id="3b181-175">For example, it could look like hello following:</span></span>

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
         * Configure hello authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where hello audience of hello token is equal toohello client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches hello Azure AD B2C metadata & signing keys from hello OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-hello-task-controller"></a><span data-ttu-id="3b181-176">Skydda uppgiftskontrollanten hello</span><span class="sxs-lookup"><span data-stu-id="3b181-176">Secure hello task controller</span></span>

<span data-ttu-id="3b181-177">När hello appen är konfigurerad toouse OAuth 2.0-autentisering, du kan skydda ditt webb-API genom att lägga till en `[Authorize]` tagg toohello uppgiftskontrollanten.</span><span class="sxs-lookup"><span data-stu-id="3b181-177">After hello app is configured toouse OAuth 2.0 authentication, you can secure your web API by adding an `[Authorize]` tag toohello task controller.</span></span> <span data-ttu-id="3b181-178">Detta är hello domänkontrollant där alla manipulering för att göra-listan äger rum, därför bör du skydda hello hela kontrollanten på klassnivå hello.</span><span class="sxs-lookup"><span data-stu-id="3b181-178">This is hello controller where all to-do list manipulation takes place, so you should secure hello entire controller at hello class level.</span></span> <span data-ttu-id="3b181-179">Du kan också lägga till hello `[Authorize]` tagga tooindividual åtgärder för mer detaljerad kontroll.</span><span class="sxs-lookup"><span data-stu-id="3b181-179">You can also add hello `[Authorize]` tag tooindividual actions for more fine-grained control.</span></span>

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-hello-token"></a><span data-ttu-id="3b181-180">Hämta användarinformation från hello-token</span><span class="sxs-lookup"><span data-stu-id="3b181-180">Get user information from hello token</span></span>

<span data-ttu-id="3b181-181">`TasksController`lagrar uppgifter i en databas där varje uppgift har en associerad användare som ”äger” hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b181-181">`TasksController` stores tasks in a database where each task has an associated user who "owns" hello task.</span></span> <span data-ttu-id="3b181-182">hello ägaren identifieras med hello användarens **objekt-ID**.</span><span class="sxs-lookup"><span data-stu-id="3b181-182">hello owner is identified by hello user's **object ID**.</span></span> <span data-ttu-id="3b181-183">(Det är därför du behövs tooadd hello objekt-ID som ett program i alla dina principer.)</span><span class="sxs-lookup"><span data-stu-id="3b181-183">(This is why you needed tooadd hello object ID as an application claim in all of your policies.)</span></span>

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-hello-permissions-in-hello-token"></a><span data-ttu-id="3b181-184">Validera hello behörigheter i hello-token</span><span class="sxs-lookup"><span data-stu-id="3b181-184">Validate hello permissions in hello token</span></span>

<span data-ttu-id="3b181-185">Ett vanligt krav för webb-API: er är toovalidate hello ”scope” finns i hello-token.</span><span class="sxs-lookup"><span data-stu-id="3b181-185">A common requirement for web APIs is toovalidate hello "scopes" present in hello token.</span></span> <span data-ttu-id="3b181-186">Detta säkerställer att hello-användaren har godkänt toohello behörigheter krävs tooaccess hello att göra tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b181-186">This ensures that hello user has consented toohello permissions required tooaccess hello to-do list service.</span></span>

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "hello Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-hello-sample-app"></a><span data-ttu-id="3b181-187">Kör hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="3b181-187">Run hello sample app</span></span>

<span data-ttu-id="3b181-188">Slutligen bygger du och kör både `TaskWebApp` och `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="3b181-188">Finally, build and run both `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="3b181-189">Skapa några uppgifter i hello användarens att göra-lista och Observera hur de finns kvar i hello API när du stoppa och starta om hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="3b181-189">Create some tasks on hello user's to-do list and notice how they are persisted in hello API even after you stop and restart hello client.</span></span>

## <a name="edit-your-policies"></a><span data-ttu-id="3b181-190">Redigera dina principer</span><span class="sxs-lookup"><span data-stu-id="3b181-190">Edit your policies</span></span>

<span data-ttu-id="3b181-191">När du har skyddat ett API med hjälp av Azure AD B2C, kan du experimentera med din inloggning-i/Sign-upp principen och visa hello effekterna (eller avsaknad av) på hello API.</span><span class="sxs-lookup"><span data-stu-id="3b181-191">After you have secured an API by using Azure AD B2C, you can experiment with your Sign-in/Sign-up policy and view hello effects (or lack thereof) on hello API.</span></span> <span data-ttu-id="3b181-192">Du kan ändra hello programanspråken i hello principer och ändra hello användarinformation som finns tillgängliga i hello webb-API.</span><span class="sxs-lookup"><span data-stu-id="3b181-192">You can manipulate hello application claims in hello policies and change hello user information that is available in hello web API.</span></span> <span data-ttu-id="3b181-193">Anspråk som du lägger till kommer att vara tillgängliga tooyour .NET MVC webb-API i hello `ClaimsPrincipal` objekt, enligt beskrivningen tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="3b181-193">Any claims that you add will be available tooyour .NET MVC web API in hello `ClaimsPrincipal` object, as described earlier in this article.</span></span>

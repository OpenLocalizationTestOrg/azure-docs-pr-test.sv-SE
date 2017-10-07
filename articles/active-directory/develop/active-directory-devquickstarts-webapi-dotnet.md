---
title: "aaaAzure AD .NET web API komma igång | Microsoft Docs"
description: "Hur toobuild en .NET MVC webb-API som kan integreras med Azure AD för autentisering och auktorisering."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 67e74774-1748-43ea-8130-55275a18320f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 91c93e1fe18855f5648076e59e2ccf081eec34bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a><span data-ttu-id="20a6d-103">Skydda ett webb-API med ägar-token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="20a6d-103">Help protect a web API by using bearer tokens from Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="20a6d-104">Om du utvecklar ett program som ger åtkomst till tooprotected resurser måste tooknow hur tooprevent obefogat åt toothose resurser.</span><span class="sxs-lookup"><span data-stu-id="20a6d-104">If you’re building an application that provides access tooprotected resources, you need tooknow how tooprevent unwarranted access toothose resources.</span></span>
<span data-ttu-id="20a6d-105">Azure Active Directory (AD Azure) gör det enkelt och enkelt toohelp skydda ett webb-API med OAuth 2.0-ägar åtkomst-token med bara några få rader med kod.</span><span class="sxs-lookup"><span data-stu-id="20a6d-105">Azure Active Directory (Azure AD) makes it simple and straightforward toohelp protect a web API by using OAuth 2.0 bearer access tokens with only a few lines of code.</span></span>

<span data-ttu-id="20a6d-106">Du kan göra detta skydd genom att använda hello Microsofts implementering av hello community-driven OWIN mellanprogram ingår i .NET Framework 4.5 i ASP.NET web apps.</span><span class="sxs-lookup"><span data-stu-id="20a6d-106">In ASP.NET web apps, you can accomplish this protection by using hello Microsoft implementation of hello community-driven OWIN middleware included in .NET Framework 4.5.</span></span> <span data-ttu-id="20a6d-107">Vi använder här OWIN toobuild en ”tooDo listan” webb-API som:</span><span class="sxs-lookup"><span data-stu-id="20a6d-107">Here we’ll use OWIN toobuild a "tooDo List" web API that:</span></span>

* <span data-ttu-id="20a6d-108">Anger vilket API: er är skyddad.</span><span class="sxs-lookup"><span data-stu-id="20a6d-108">Designates which APIs are protected.</span></span>
* <span data-ttu-id="20a6d-109">Verifierar att hello webb-API-anrop kan innehålla en giltig åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="20a6d-109">Validates that hello web API calls contain a valid access token.</span></span>

<span data-ttu-id="20a6d-110">toobuild hello tooDo lista API, måste du först:</span><span class="sxs-lookup"><span data-stu-id="20a6d-110">toobuild hello tooDo List API, you first need to:</span></span>

1. <span data-ttu-id="20a6d-111">Registrera ett program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20a6d-111">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="20a6d-112">Ställ in hello app toouse hello OWIN autentisering pipeline.</span><span class="sxs-lookup"><span data-stu-id="20a6d-112">Set up hello app toouse hello OWIN authentication pipeline.</span></span>
3. <span data-ttu-id="20a6d-113">Konfigurera en klient programmet toocall hello webb-API.</span><span class="sxs-lookup"><span data-stu-id="20a6d-113">Configure a client application toocall hello web API.</span></span>

<span data-ttu-id="20a6d-114">tooget har startats [hämta hello app stommen](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="20a6d-114">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="20a6d-115">Varje är en lösning för Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="20a6d-115">Each is a Visual Studio 2013 solution.</span></span> <span data-ttu-id="20a6d-116">Du måste också en Azure AD-klient i vilka tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="20a6d-116">You also need an Azure AD tenant in which tooregister your application.</span></span> <span data-ttu-id="20a6d-117">Om du inte redan har en [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="20a6d-117">If you don't have one already, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="20a6d-118">Steg 1: Registrera ett program med Azure AD</span><span class="sxs-lookup"><span data-stu-id="20a6d-118">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="20a6d-119">toohelp skydda ditt program måste du först behöver toocreate ett program i din klient ger Azure AD med några uppgifter.</span><span class="sxs-lookup"><span data-stu-id="20a6d-119">toohelp secure your application, you first need toocreate an application in your tenant and provide Azure AD with a few key pieces of information.</span></span>

1. <span data-ttu-id="20a6d-120">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="20a6d-120">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="20a6d-121">Klicka på ditt konto hello översta fältet.</span><span class="sxs-lookup"><span data-stu-id="20a6d-121">On hello top bar, click your account.</span></span> <span data-ttu-id="20a6d-122">I hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="20a6d-122">In hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>

3. <span data-ttu-id="20a6d-123">Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="20a6d-123">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="20a6d-124">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="20a6d-124">Click **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="20a6d-125">Följ anvisningarna för hello och skapa en ny **webbprogram och/eller webb-API**.</span><span class="sxs-lookup"><span data-stu-id="20a6d-125">Follow hello prompts and create a new **Web Application and/or Web API**.</span></span>
  * <span data-ttu-id="20a6d-126">**Namnet** beskriver toousers ditt program.</span><span class="sxs-lookup"><span data-stu-id="20a6d-126">**Name** describes your application toousers.</span></span> <span data-ttu-id="20a6d-127">Ange **tooDo tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="20a6d-127">Enter **tooDo List Service**.</span></span>
  * <span data-ttu-id="20a6d-128">**Omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD använder tooreturn eventuella tokens som din app har begärt.</span><span class="sxs-lookup"><span data-stu-id="20a6d-128">**Redirect Uri** is a scheme and string combination that Azure AD uses tooreturn any tokens that your app has requested.</span></span> <span data-ttu-id="20a6d-129">Ange `https://localhost:44321/` för det här värdet.</span><span class="sxs-lookup"><span data-stu-id="20a6d-129">Enter `https://localhost:44321/` for this value.</span></span>

6. <span data-ttu-id="20a6d-130">Från hello **inställningar** -> **egenskaper** för programmet, uppdatera hello App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="20a6d-130">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="20a6d-131">Ange en klient-specifika identifierare.</span><span class="sxs-lookup"><span data-stu-id="20a6d-131">Enter a tenant-specific identifier.</span></span> <span data-ttu-id="20a6d-132">Ange till exempel `https://contoso.onmicrosoft.com/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="20a6d-132">For example, enter `https://contoso.onmicrosoft.com/TodoListService`.</span></span>

7. <span data-ttu-id="20a6d-133">Spara hello-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="20a6d-133">Save hello configuration.</span></span> <span data-ttu-id="20a6d-134">Lämna hello portal öppen, eftersom du måste också tooregister klientprogrammet inom kort.</span><span class="sxs-lookup"><span data-stu-id="20a6d-134">Leave hello portal open, because you'll also need tooregister your client application shortly.</span></span>

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a><span data-ttu-id="20a6d-135">Steg 2: Konfigurera hello app toouse hello OWIN autentisering pipeline</span><span class="sxs-lookup"><span data-stu-id="20a6d-135">Step 2: Set up hello app toouse hello OWIN authentication pipeline</span></span>
<span data-ttu-id="20a6d-136">toovalidate inkommande begäranden och token måste tooset upp ditt program toocommunicate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20a6d-136">toovalidate incoming requests and tokens, you need tooset up your application toocommunicate with Azure AD.</span></span>

1. <span data-ttu-id="20a6d-137">toobegin, öppna hello lösningen och lägga till hello OWIN mellanprogram NuGet-paket toohello TodoListService projekt med hjälp av hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="20a6d-137">toobegin, open hello solution and add hello OWIN middleware NuGet packages toohello TodoListService project by using hello Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. <span data-ttu-id="20a6d-138">Lägg till en OWIN klassen toohello TodoListService Startprojekt kallas `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="20a6d-138">Add an OWIN Startup class toohello TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="20a6d-139">Högerklicka på hello projektet, Välj **Lägg till** > **nytt objekt**, och sök sedan efter **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="20a6d-139">Right-click hello project, select **Add** > **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="20a6d-140">Hej OWIN mellanprogram ska anropa hello `Configuration(…)` metod när appen startar.</span><span class="sxs-lookup"><span data-stu-id="20a6d-140">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>

3. <span data-ttu-id="20a6d-141">Ändra hello klassdeklarationen för`public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="20a6d-141">Change hello class declaration too`public partial class Startup`.</span></span> <span data-ttu-id="20a6d-142">Vi har redan implementerats en del av den här klassen för dig i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="20a6d-142">We’ve already implemented part of this class for you in another file.</span></span> <span data-ttu-id="20a6d-143">I hello `Configuration(…)` metod, gör ett anrop för`ConfgureAuth(…)` tooset in verifiering för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="20a6d-143">In hello `Configuration(…)` method, make a call too`ConfgureAuth(…)` tooset up authentication for your web app.</span></span>

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. <span data-ttu-id="20a6d-144">Öppna hello filen `App_Start\Startup.Auth.cs` och implementera hello `ConfigureAuth(…)` metod.</span><span class="sxs-lookup"><span data-stu-id="20a6d-144">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(…)` method.</span></span> <span data-ttu-id="20a6d-145">Hej parametrar som du anger i `WindowsAzureActiveDirectoryBearerAuthenticationOptions` fungerar som koordinater för din app toocommunicate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20a6d-145">hello parameters that you provide in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` will serve as coordinates for your app toocommunicate with Azure AD.</span></span>

    ```C#
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
            });
    }
    ```

5. <span data-ttu-id="20a6d-146">Nu kan du använda `[Authorize]` attribut toohelp skydda dina domänkontrollanter och åtgärder med JSON-Webbtoken (JWT) ägar-autentisering.</span><span class="sxs-lookup"><span data-stu-id="20a6d-146">Now you can use `[Authorize]` attributes toohelp protect your controllers and actions with JSON Web Token (JWT) bearer authentication.</span></span> <span data-ttu-id="20a6d-147">Skapa snygga hello `Controllers\TodoListController.cs` klass med en auktorisera-tagg.</span><span class="sxs-lookup"><span data-stu-id="20a6d-147">Decorate hello `Controllers\TodoListController.cs` class with an authorize tag.</span></span> <span data-ttu-id="20a6d-148">Detta tvingar hello användaren toosign i innan sidan.</span><span class="sxs-lookup"><span data-stu-id="20a6d-148">This will force hello user toosign in before accessing that page.</span></span>

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    <span data-ttu-id="20a6d-149">När en auktoriserad anroparen har anropar en hello `TodoListController` API: er, hello åtgärden kanske behöver komma åt tooinformation om hello anroparen.</span><span class="sxs-lookup"><span data-stu-id="20a6d-149">When an authorized caller successfully invokes one of hello `TodoListController` APIs, hello action might need access tooinformation about hello caller.</span></span> <span data-ttu-id="20a6d-150">OWIN tillhandahåller åtkomst toohello anspråk i hello ägar-token via hello `ClaimsPrincpal` objekt.</span><span class="sxs-lookup"><span data-stu-id="20a6d-150">OWIN provides access toohello claims inside hello bearer token via hello `ClaimsPrincpal` object.</span></span>  

6. <span data-ttu-id="20a6d-151">Ett vanligt krav för webb-API: er är toovalidate hello ”scope” finns i hello-token.</span><span class="sxs-lookup"><span data-stu-id="20a6d-151">A common requirement for web APIs is toovalidate hello "scopes" present in hello token.</span></span> <span data-ttu-id="20a6d-152">Detta säkerställer att hello-användaren har godkänt toohello behörigheter krävs tooaccess hello tooDo tjänsten.</span><span class="sxs-lookup"><span data-stu-id="20a6d-152">This ensures that hello user has consented toohello permissions required tooaccess hello tooDo List Service.</span></span>

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is hello default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "hello Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. <span data-ttu-id="20a6d-153">Öppna hello `web.config` filen i hello roten för hello TodoListService projekt och ange dina konfigurationsvärden i hello `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="20a6d-153">Open hello `web.config` file in hello root of hello TodoListService project, and enter your configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="20a6d-154">`ida:Tenant`är din Azure AD-klient – till exempel contoso.onmicrosoft.com hello namn.</span><span class="sxs-lookup"><span data-stu-id="20a6d-154">`ida:Tenant` is hello name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="20a6d-155">`ida:Audience`är hello App-ID-URI för hello-program som du angav i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="20a6d-155">`ida:Audience` is hello App ID URI of hello application that you entered in hello Azure portal.</span></span>

## <a name="step-3-configure-a-client-application-and-run-hello-service"></a><span data-ttu-id="20a6d-156">Steg 3: Konfigurera ett klientprogram och köra hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="20a6d-156">Step 3: Configure a client application and run hello service</span></span>
<span data-ttu-id="20a6d-157">Innan du kan se hello tooDo tjänsten i åtgärden måste tooconfigure hello tooDo lista klienten så att den kan hämta token från Azure AD att anrop toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="20a6d-157">Before you can see hello tooDo List Service in action, you need tooconfigure hello tooDo List client so it can get tokens from Azure AD and make calls toohello service.</span></span>

1. <span data-ttu-id="20a6d-158">Gå tillbaka toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="20a6d-158">Go back toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="20a6d-159">Skapa ett nytt program i Azure AD-klienten och välj **internt klientprogram** hello resulterande fråga.</span><span class="sxs-lookup"><span data-stu-id="20a6d-159">Create a new application in your Azure AD tenant, and select **Native Client Application** in hello resulting prompt.</span></span>
  * <span data-ttu-id="20a6d-160">**Namnet** beskriver toousers ditt program.</span><span class="sxs-lookup"><span data-stu-id="20a6d-160">**Name** describes your application toousers.</span></span>
  * <span data-ttu-id="20a6d-161">Ange `http://TodoListClient/` för hello **omdirigerings-Uri** värde.</span><span class="sxs-lookup"><span data-stu-id="20a6d-161">Enter `http://TodoListClient/` for hello **Redirect Uri** value.</span></span>

3. <span data-ttu-id="20a6d-162">När du har slutfört registreringen tilldelar en unik program-ID tooyour app i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="20a6d-162">After you finish registration, Azure AD assigns a unique application ID tooyour app.</span></span> <span data-ttu-id="20a6d-163">Du behöver det här värdet i hello nästa steg, så kopiera den från hello appen på sidan.</span><span class="sxs-lookup"><span data-stu-id="20a6d-163">You’ll need this value in hello next steps, so copy it from hello application page.</span></span>

4. <span data-ttu-id="20a6d-164">Från hello **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="20a6d-164">From hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span> <span data-ttu-id="20a6d-165">Leta upp och välj hello tooDo tjänsten, Lägg till hello **åtkomst TodoListService** behörighet under **delegerade behörigheter**, och klicka sedan på **klar**.</span><span class="sxs-lookup"><span data-stu-id="20a6d-165">Locate and select hello tooDo List Service, add hello **Access TodoListService** permission under **Delegated Permissions**, and then click **Done**.</span></span>

5. <span data-ttu-id="20a6d-166">Öppna i Visual Studio `App.config` i hello TodoListClient projektet och sedan ange dina konfigurationsvärden i hello `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="20a6d-166">In Visual Studio, open `App.config` in hello TodoListClient project, and then enter your configuration values in hello `<appSettings>` section.</span></span>

  * <span data-ttu-id="20a6d-167">`ida:Tenant`är din Azure AD-klient – till exempel contoso.onmicrosoft.com hello namn.</span><span class="sxs-lookup"><span data-stu-id="20a6d-167">`ida:Tenant` is hello name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="20a6d-168">`ida:ClientId`är hello app-ID som du kopierade från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="20a6d-168">`ida:ClientId` is hello app ID that you copied from hello Azure portal.</span></span>
  * <span data-ttu-id="20a6d-169">`todo:TodoListResourceId`är hello App-ID-URI för hello tooDo lista tjänstprogram som du angav i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="20a6d-169">`todo:TodoListResourceId` is hello App ID URI of hello tooDo List Service application that you entered in hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="20a6d-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="20a6d-170">Next steps</span></span>
<span data-ttu-id="20a6d-171">Slutligen, rensa, skapa och kör varje projekt.</span><span class="sxs-lookup"><span data-stu-id="20a6d-171">Finally, clean, build, and run each project.</span></span> <span data-ttu-id="20a6d-172">Om du inte redan gjort nu är hello tid toocreate en ny användare i din klient med en *. onmicrosoft.com-domän.</span><span class="sxs-lookup"><span data-stu-id="20a6d-172">If you haven’t already, now is hello time toocreate a new user in your tenant with a *.onmicrosoft.com domain.</span></span> <span data-ttu-id="20a6d-173">Logga in toohello tooDo lista klienten med den aktuella användaren och lägga till vissa uppgifter toohello användarens att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="20a6d-173">Sign in toohello tooDo List client with that user, and add some tasks toohello user's to-do list.</span></span>

<span data-ttu-id="20a6d-174">Referens hello slutförts exemplet (utan dina konfigurationsvärden) är tillgängliga i [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="20a6d-174">For reference, hello completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="20a6d-175">Du kan nu gå vidare toomore identitet scenarier.</span><span class="sxs-lookup"><span data-stu-id="20a6d-175">You can now move on toomore identity scenarios.</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

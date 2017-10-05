---
title: "Azure AD-.NET-webb-API komma igång | Microsoft Docs"
description: "Hur du skapar ett .NET MVC-webb-API som kan integreras med Azure AD för autentisering och auktorisering."
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
ms.openlocfilehash: f44d75f45073a5d9aa9b1863ed227aba4efcf785
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="help-protect-a-web-api-by-using-bearer-tokens-from-azure-ad"></a><span data-ttu-id="86de3-103">Skydda ett webb-API med ägar-token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="86de3-103">Help protect a web API by using bearer tokens from Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="86de3-104">Om du utvecklar ett program som ger åtkomst till skyddade resurser, måste du känna till hur förhindra obefogat åtkomst till resurserna.</span><span class="sxs-lookup"><span data-stu-id="86de3-104">If you’re building an application that provides access to protected resources, you need to know how to prevent unwarranted access to those resources.</span></span>
<span data-ttu-id="86de3-105">Azure Active Directory (AD Azure) gör det lätt att skydda ett webb-API med OAuth 2.0-ägar access token med bara några få rader med kod.</span><span class="sxs-lookup"><span data-stu-id="86de3-105">Azure Active Directory (Azure AD) makes it simple and straightforward to help protect a web API by using OAuth 2.0 bearer access tokens with only a few lines of code.</span></span>

<span data-ttu-id="86de3-106">Du kan göra det här skyddet med hjälp av Microsofts implementering av den community-driven OWIN mellanprogram ingår i .NET Framework 4.5 i ASP.NET web apps.</span><span class="sxs-lookup"><span data-stu-id="86de3-106">In ASP.NET web apps, you can accomplish this protection by using the Microsoft implementation of the community-driven OWIN middleware included in .NET Framework 4.5.</span></span> <span data-ttu-id="86de3-107">Här använder vi OWIN att skapa en ”att göra-lista” webb-API som:</span><span class="sxs-lookup"><span data-stu-id="86de3-107">Here we’ll use OWIN to build a "To Do List" web API that:</span></span>

* <span data-ttu-id="86de3-108">Anger vilket API: er är skyddad.</span><span class="sxs-lookup"><span data-stu-id="86de3-108">Designates which APIs are protected.</span></span>
* <span data-ttu-id="86de3-109">Verifierar att webb-API-anrop kan innehålla en giltig åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="86de3-109">Validates that the web API calls contain a valid access token.</span></span>

<span data-ttu-id="86de3-110">Om du vill skapa att göra listan API: et, måste du först:</span><span class="sxs-lookup"><span data-stu-id="86de3-110">To build the To Do List API, you first need to:</span></span>

1. <span data-ttu-id="86de3-111">Registrera ett program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86de3-111">Register an application with Azure AD.</span></span>
2. <span data-ttu-id="86de3-112">Konfigurera appen för att använda OWIN autentisering pipeline.</span><span class="sxs-lookup"><span data-stu-id="86de3-112">Set up the app to use the OWIN authentication pipeline.</span></span>
3. <span data-ttu-id="86de3-113">Konfigurera ett klientprogram att anropa webb-API.</span><span class="sxs-lookup"><span data-stu-id="86de3-113">Configure a client application to call the web API.</span></span>

<span data-ttu-id="86de3-114">Du kommer igång [ladda ned stommen app](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) eller [hämta det slutförda exemplet](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="86de3-114">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="86de3-115">Varje är en lösning för Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="86de3-115">Each is a Visual Studio 2013 solution.</span></span> <span data-ttu-id="86de3-116">Du måste också en Azure AD-klient att registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="86de3-116">You also need an Azure AD tenant in which to register your application.</span></span> <span data-ttu-id="86de3-117">Om du inte redan har en [Lär dig hur du skaffa en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="86de3-117">If you don't have one already, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-an-application-with-azure-ad"></a><span data-ttu-id="86de3-118">Steg 1: Registrera ett program med Azure AD</span><span class="sxs-lookup"><span data-stu-id="86de3-118">Step 1: Register an application with Azure AD</span></span>
<span data-ttu-id="86de3-119">För att säkra programmet, du måste först skapa ett program i din klient och ger Azure AD med några uppgifter.</span><span class="sxs-lookup"><span data-stu-id="86de3-119">To help secure your application, you first need to create an application in your tenant and provide Azure AD with a few key pieces of information.</span></span>

1. <span data-ttu-id="86de3-120">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="86de3-120">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="86de3-121">Klicka på ditt konto på den översta raden.</span><span class="sxs-lookup"><span data-stu-id="86de3-121">On the top bar, click your account.</span></span> <span data-ttu-id="86de3-122">I den **Directory** Välj Azure AD-klient som du vill registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="86de3-122">In the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>

3. <span data-ttu-id="86de3-123">Klicka på **fler tjänster** i det vänstra fönstret och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="86de3-123">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="86de3-124">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86de3-124">Click **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="86de3-125">Följ anvisningarna och skapa en ny **webbprogram och/eller webb-API**.</span><span class="sxs-lookup"><span data-stu-id="86de3-125">Follow the prompts and create a new **Web Application and/or Web API**.</span></span>
  * <span data-ttu-id="86de3-126">**Namnet** beskriver ditt program till användare.</span><span class="sxs-lookup"><span data-stu-id="86de3-126">**Name** describes your application to users.</span></span> <span data-ttu-id="86de3-127">Ange **att göra-lista tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="86de3-127">Enter **To Do List Service**.</span></span>
  * <span data-ttu-id="86de3-128">**Omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD som används för att returnera de token som har begärt att din app.</span><span class="sxs-lookup"><span data-stu-id="86de3-128">**Redirect Uri** is a scheme and string combination that Azure AD uses to return any tokens that your app has requested.</span></span> <span data-ttu-id="86de3-129">Ange `https://localhost:44321/` för det här värdet.</span><span class="sxs-lookup"><span data-stu-id="86de3-129">Enter `https://localhost:44321/` for this value.</span></span>

6. <span data-ttu-id="86de3-130">Från den **inställningar** -> **egenskaper** för programmet, uppdatera App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="86de3-130">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="86de3-131">Ange en klient-specifika identifierare.</span><span class="sxs-lookup"><span data-stu-id="86de3-131">Enter a tenant-specific identifier.</span></span> <span data-ttu-id="86de3-132">Ange till exempel `https://contoso.onmicrosoft.com/TodoListService`.</span><span class="sxs-lookup"><span data-stu-id="86de3-132">For example, enter `https://contoso.onmicrosoft.com/TodoListService`.</span></span>

7. <span data-ttu-id="86de3-133">Spara konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="86de3-133">Save the configuration.</span></span> <span data-ttu-id="86de3-134">Lämna portalen öppen, eftersom du måste också registrera ditt klientprogram inom kort.</span><span class="sxs-lookup"><span data-stu-id="86de3-134">Leave the portal open, because you'll also need to register your client application shortly.</span></span>

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a><span data-ttu-id="86de3-135">Steg 2: Konfigurera appen för att använda OWIN autentisering pipeline</span><span class="sxs-lookup"><span data-stu-id="86de3-135">Step 2: Set up the app to use the OWIN authentication pipeline</span></span>
<span data-ttu-id="86de3-136">Om du vill validera inkommande begäranden och token, måste du konfigurera ditt program för att kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86de3-136">To validate incoming requests and tokens, you need to set up your application to communicate with Azure AD.</span></span>

1. <span data-ttu-id="86de3-137">Börja, öppna lösningen och Lägg till NuGet-paket OWIN mellanprogram TodoListService projektet med hjälp av Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="86de3-137">To begin, open the solution and add the OWIN middleware NuGet packages to the TodoListService project by using the Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
    PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
    ```

2. <span data-ttu-id="86de3-138">Lägg till en OWIN-startklass i TodoListService-projektet som kallas `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="86de3-138">Add an OWIN Startup class to the TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="86de3-139">Högerklicka på projektet, Välj **Lägg till** > **nytt objekt**, och sök sedan efter **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="86de3-139">Right-click the project, select **Add** > **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="86de3-140">OWIN-mellanprogrammet anropar `Configuration(…)`-metoden när appen startas.</span><span class="sxs-lookup"><span data-stu-id="86de3-140">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>

3. <span data-ttu-id="86de3-141">Ändra klassdeklarationen till `public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="86de3-141">Change the class declaration to `public partial class Startup`.</span></span> <span data-ttu-id="86de3-142">Vi har redan implementerats en del av den här klassen för dig i en annan fil.</span><span class="sxs-lookup"><span data-stu-id="86de3-142">We’ve already implemented part of this class for you in another file.</span></span> <span data-ttu-id="86de3-143">I den `Configuration(…)` gör ett anrop till metoden `ConfgureAuth(…)` du konfigurerar autentisering för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="86de3-143">In the `Configuration(…)` method, make a call to `ConfgureAuth(…)` to set up authentication for your web app.</span></span>

    ```C#
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
    ```

4. <span data-ttu-id="86de3-144">Öppna filen `App_Start\Startup.Auth.cs` och genomföra den `ConfigureAuth(…)` metoden.</span><span class="sxs-lookup"><span data-stu-id="86de3-144">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(…)` method.</span></span> <span data-ttu-id="86de3-145">De parametrar som du anger i `WindowsAzureActiveDirectoryBearerAuthenticationOptions` fungerar som koordinater för din app för att kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86de3-145">The parameters that you provide in `WindowsAzureActiveDirectoryBearerAuthenticationOptions` will serve as coordinates for your app to communicate with Azure AD.</span></span>

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

5. <span data-ttu-id="86de3-146">Nu kan du använda `[Authorize]` attribut för att skydda dina domänkontrollanter och åtgärder med JSON-Webbtoken (JWT) ägar-autentisering.</span><span class="sxs-lookup"><span data-stu-id="86de3-146">Now you can use `[Authorize]` attributes to help protect your controllers and actions with JSON Web Token (JWT) bearer authentication.</span></span> <span data-ttu-id="86de3-147">Skapa snygga den `Controllers\TodoListController.cs` klass med en auktorisera-tagg.</span><span class="sxs-lookup"><span data-stu-id="86de3-147">Decorate the `Controllers\TodoListController.cs` class with an authorize tag.</span></span> <span data-ttu-id="86de3-148">Detta tvingar användaren att logga in innan sidan.</span><span class="sxs-lookup"><span data-stu-id="86de3-148">This will force the user to sign in before accessing that page.</span></span>

    ```C#
    [Authorize]
    public class TodoListController : ApiController
    {
    ```

    <span data-ttu-id="86de3-149">När en auktoriserad anroparen har anropar någon av de `TodoListController` API: er, åtgärden kanske behöver åtkomst till information om anroparen.</span><span class="sxs-lookup"><span data-stu-id="86de3-149">When an authorized caller successfully invokes one of the `TodoListController` APIs, the action might need access to information about the caller.</span></span> <span data-ttu-id="86de3-150">OWIN ger tillgång till anspråk i ägartoken via den `ClaimsPrincpal` objekt.</span><span class="sxs-lookup"><span data-stu-id="86de3-150">OWIN provides access to the claims inside the bearer token via the `ClaimsPrincpal` object.</span></span>  

6. <span data-ttu-id="86de3-151">Ett vanligt krav för webb-API:er är att validera ”omfång” i token.</span><span class="sxs-lookup"><span data-stu-id="86de3-151">A common requirement for web APIs is to validate the "scopes" present in the token.</span></span> <span data-ttu-id="86de3-152">Detta säkerställer att användaren har godkänt för de behörigheter som krävs för att få åtkomst till att göra tjänsten.</span><span class="sxs-lookup"><span data-stu-id="86de3-152">This ensures that the user has consented to the permissions required to access the To Do List Service.</span></span>

    ```C#
    public IEnumerable<TodoItem> Get()
    {
        // user_impersonation is the default permission exposed by applications in Azure AD
        if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
        {
            throw new HttpResponseException(new HttpResponseMessage {
              StatusCode = HttpStatusCode.Unauthorized,
              ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
            });
        }
        ...
    }
    ```

7. <span data-ttu-id="86de3-153">Öppna den `web.config` filen i roten av projektet TodoListService och ange dina konfigurationsvärden i den `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="86de3-153">Open the `web.config` file in the root of the TodoListService project, and enter your configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="86de3-154">`ida:Tenant`är namnet på din Azure AD-klient – till exempel contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="86de3-154">`ida:Tenant` is the name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="86de3-155">`ida:Audience`är URI: N App-ID för det program som du angav i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="86de3-155">`ida:Audience` is the App ID URI of the application that you entered in the Azure portal.</span></span>

## <a name="step-3-configure-a-client-application-and-run-the-service"></a><span data-ttu-id="86de3-156">Steg 3: Konfigurera ett klientprogram och köra tjänsten</span><span class="sxs-lookup"><span data-stu-id="86de3-156">Step 3: Configure a client application and run the service</span></span>
<span data-ttu-id="86de3-157">Innan du kan se att göra tjänsten i åtgärden måste du konfigurera klienten för att göra-listan så att den kan hämta token från Azure AD-anrop till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="86de3-157">Before you can see the To Do List Service in action, you need to configure the To Do List client so it can get tokens from Azure AD and make calls to the service.</span></span>

1. <span data-ttu-id="86de3-158">Gå tillbaka till den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="86de3-158">Go back to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="86de3-159">Skapa ett nytt program i Azure AD-klienten och välj **internt klientprogram** i den resulterande frågan.</span><span class="sxs-lookup"><span data-stu-id="86de3-159">Create a new application in your Azure AD tenant, and select **Native Client Application** in the resulting prompt.</span></span>
  * <span data-ttu-id="86de3-160">**Namnet** beskriver ditt program till användare.</span><span class="sxs-lookup"><span data-stu-id="86de3-160">**Name** describes your application to users.</span></span>
  * <span data-ttu-id="86de3-161">Ange `http://TodoListClient/` för den **omdirigerings-Uri** värde.</span><span class="sxs-lookup"><span data-stu-id="86de3-161">Enter `http://TodoListClient/` for the **Redirect Uri** value.</span></span>

3. <span data-ttu-id="86de3-162">När du har slutfört registreringen tilldelar ett unikt ID i Azure AD till din app.</span><span class="sxs-lookup"><span data-stu-id="86de3-162">After you finish registration, Azure AD assigns a unique application ID to your app.</span></span> <span data-ttu-id="86de3-163">Du behöver det här värdet i nästa steg, så kopiera den från appen på sidan.</span><span class="sxs-lookup"><span data-stu-id="86de3-163">You’ll need this value in the next steps, so copy it from the application page.</span></span>

4. <span data-ttu-id="86de3-164">Från den **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="86de3-164">From the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span> <span data-ttu-id="86de3-165">Leta upp och välj att göra tjänsten, Lägg till den **åtkomst TodoListService** behörighet under **delegerade behörigheter**, och klicka sedan på **klar**.</span><span class="sxs-lookup"><span data-stu-id="86de3-165">Locate and select the To Do List Service, add the **Access TodoListService** permission under **Delegated Permissions**, and then click **Done**.</span></span>

5. <span data-ttu-id="86de3-166">Öppna i Visual Studio `App.config` i TodoListClient projektet och sedan ange dina konfigurationsvärden i den `<appSettings>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="86de3-166">In Visual Studio, open `App.config` in the TodoListClient project, and then enter your configuration values in the `<appSettings>` section.</span></span>

  * <span data-ttu-id="86de3-167">`ida:Tenant`är namnet på din Azure AD-klient – till exempel contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="86de3-167">`ida:Tenant` is the name of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="86de3-168">`ida:ClientId`är det app-ID som du kopierade från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="86de3-168">`ida:ClientId` is the app ID that you copied from the Azure portal.</span></span>
  * <span data-ttu-id="86de3-169">`todo:TodoListResourceId`är URI: N App-ID för att göra listan tjänstprogrammet som du angav i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="86de3-169">`todo:TodoListResourceId` is the App ID URI of the To Do List Service application that you entered in the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86de3-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="86de3-170">Next steps</span></span>
<span data-ttu-id="86de3-171">Slutligen, rensa, skapa och kör varje projekt.</span><span class="sxs-lookup"><span data-stu-id="86de3-171">Finally, clean, build, and run each project.</span></span> <span data-ttu-id="86de3-172">Om du inte redan gjort nu är det dags att skapa en ny användare i din klient med en *. onmicrosoft.com-domän.</span><span class="sxs-lookup"><span data-stu-id="86de3-172">If you haven’t already, now is the time to create a new user in your tenant with a *.onmicrosoft.com domain.</span></span> <span data-ttu-id="86de3-173">Logga in på klienten för att göra-lista med användaren och lägga till vissa uppgifter i användarens att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="86de3-173">Sign in to the To Do List client with that user, and add some tasks to the user's to-do list.</span></span>

<span data-ttu-id="86de3-174">För referens anger det slutförda exemplet (utan dina konfigurationsvärden) är tillgängligt i [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="86de3-174">For reference, the completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="86de3-175">Du kan nu gå vidare till mer identity-scenarier.</span><span class="sxs-lookup"><span data-stu-id="86de3-175">You can now move on to more identity scenarios.</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

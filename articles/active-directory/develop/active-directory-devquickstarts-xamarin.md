---
title: "Azure AD Xamarin komma igång | Microsoft Docs"
description: "Skapa Xamarin-program som integreras med Azure AD för inloggning och anropa Azure AD-skyddade API: er med hjälp av OAuth."
services: active-directory
documentationcenter: xamarin
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 54ee403f283bc5dc79911e2e813dd513ff595828
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a><span data-ttu-id="53eb1-103">Integrera Azure AD med Xamarin-appar</span><span class="sxs-lookup"><span data-stu-id="53eb1-103">Integrate Azure AD with Xamarin apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="53eb1-104">Du kan skriva mobila appar med Xamarin, i C# som kan köras på iOS, Android och Windows (mobila enheter och datorer).</span><span class="sxs-lookup"><span data-stu-id="53eb1-104">With Xamarin, you can write mobile apps in C# that can run on iOS, Android, and Windows (mobile devices and PCs).</span></span> <span data-ttu-id="53eb1-105">Om du utvecklar en app med Xamarin, gör du enkelt att autentisera användare med sina Azure AD-konton med hjälp av Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="53eb1-105">If you're building an app using Xamarin, Azure Active Directory (Azure AD) makes it simple to authenticate users with their Azure AD accounts.</span></span> <span data-ttu-id="53eb1-106">Appen kan också säkert att använda alla webb-API som skyddas av Azure AD, till exempel API: er för Office 365 eller Azure API.</span><span class="sxs-lookup"><span data-stu-id="53eb1-106">The app can also securely consume any web API that's protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="53eb1-107">För Xamarin-appar som behöver åtkomst till skyddade resurser, innehåller Azure AD Active Directory Authentication Library (ADAL).</span><span class="sxs-lookup"><span data-stu-id="53eb1-107">For Xamarin apps that need to access protected resources, Azure AD provides the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="53eb1-108">Det enda syftet med ADAL är att göra det enkelt för appar för att få åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="53eb1-108">The sole purpose of ADAL is to make it easy for apps to get access tokens.</span></span> <span data-ttu-id="53eb1-109">För att demonstrera hur lätt det är den här artikeln visar hur man skapar DirectorySearcher appar som:</span><span class="sxs-lookup"><span data-stu-id="53eb1-109">To demonstrate how easy it is, this article shows how to build DirectorySearcher apps that:</span></span>

* <span data-ttu-id="53eb1-110">Kör på iOS, Android, Windows-skrivbordet, Windows Phone och Windows Store.</span><span class="sxs-lookup"><span data-stu-id="53eb1-110">Run on iOS, Android, Windows Desktop, Windows Phone, and Windows Store.</span></span>
* <span data-ttu-id="53eb1-111">Använda en enda bärbar klassbiblioteket (PCL) för att autentisera användare och hämta token för Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="53eb1-111">Use a single portable class library (PCL) to authenticate users and get tokens for the Azure AD Graph API.</span></span>
* <span data-ttu-id="53eb1-112">Söka i en katalog för användare med ett angivet UPN.</span><span class="sxs-lookup"><span data-stu-id="53eb1-112">Search a directory for users with a given UPN.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="53eb1-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="53eb1-113">Before you get started</span></span>
* <span data-ttu-id="53eb1-114">Hämta den [stommen av ett projekt](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), eller hämta den [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="53eb1-114">Download the [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), or download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="53eb1-115">Varje version är en lösning i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="53eb1-115">Each download is a Visual Studio 2013 solution.</span></span>
* <span data-ttu-id="53eb1-116">Du måste också en Azure AD-klient att skapa användare och registrera appen.</span><span class="sxs-lookup"><span data-stu-id="53eb1-116">You also need an Azure AD tenant in which to create users and register the app.</span></span> <span data-ttu-id="53eb1-117">Om du inte redan har en klient [Lär dig hur du skaffa en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="53eb1-117">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="53eb1-118">När du är klar följer du procedurerna i följande fyra avsnitt.</span><span class="sxs-lookup"><span data-stu-id="53eb1-118">When you are ready, follow the procedures in the next four sections.</span></span>

## <a name="step-1-set-up-your-xamarin-development-environment"></a><span data-ttu-id="53eb1-119">Steg 1: Ställ in din utvecklingsmiljö för Xamarin</span><span class="sxs-lookup"><span data-stu-id="53eb1-119">Step 1: Set up your Xamarin development environment</span></span>
<span data-ttu-id="53eb1-120">Eftersom den här självstudiekursen innehåller projekt för iOS, Android och Windows, måste både Visual Studio och Xamarin.</span><span class="sxs-lookup"><span data-stu-id="53eb1-120">Because this tutorial includes projects for iOS, Android, and Windows, you need both Visual Studio and Xamarin.</span></span> <span data-ttu-id="53eb1-121">Om du vill skapa den nödvändiga miljön slutföras i [ställa upp och installera Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="53eb1-121">To create the necessary environment, complete the process in [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) on MSDN.</span></span> <span data-ttu-id="53eb1-122">Anvisningarna omfattar material som du kan granska Läs mer om Xamarin medan du väntar för installationer ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="53eb1-122">The instructions include material that you can review to learn more about Xamarin while you're waiting for the installations to be completed.</span></span>

<span data-ttu-id="53eb1-123">När du har slutfört installationen, öppnar du lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="53eb1-123">After you've completed the setup, open the solution in Visual Studio.</span></span> <span data-ttu-id="53eb1-124">Där hittar du sex projekt: fem plattformsspecifika projekt och en PCL, DirectorySearcher.cs som delas mellan alla plattformar.</span><span class="sxs-lookup"><span data-stu-id="53eb1-124">There, you will find six projects: five platform-specific projects and one PCL, DirectorySearcher.cs, which will be shared across all platforms.</span></span>

## <a name="step-2-register-the-directorysearcher-app"></a><span data-ttu-id="53eb1-125">Steg 2: Registrera DirectorySearcher-app</span><span class="sxs-lookup"><span data-stu-id="53eb1-125">Step 2: Register the DirectorySearcher app</span></span>
<span data-ttu-id="53eb1-126">Om du vill aktivera app att hämta token, måste du först registrera det i Azure AD-klienten och bevilja behörighet att komma åt Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="53eb1-126">To enable the app to get tokens, you first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API.</span></span> <span data-ttu-id="53eb1-127">Så här gör du:</span><span class="sxs-lookup"><span data-stu-id="53eb1-127">Here's how:</span></span>

1. <span data-ttu-id="53eb1-128">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="53eb1-128">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="53eb1-129">Klicka på ditt konto på den översta raden.</span><span class="sxs-lookup"><span data-stu-id="53eb1-129">On the top bar, click your account.</span></span> <span data-ttu-id="53eb1-130">Sedan, under den **Directory** väljer du Active Directory-klient som du vill registrera appen.</span><span class="sxs-lookup"><span data-stu-id="53eb1-130">Then, under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="53eb1-131">Klicka på **fler tjänster** i det vänstra fönstret och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="53eb1-131">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="53eb1-132">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="53eb1-132">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="53eb1-133">Att skapa en ny **internt klientprogram**, följ instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="53eb1-133">To create a new **Native Client Application**, follow the prompts.</span></span>
  * <span data-ttu-id="53eb1-134">**Namnet** beskriver app till användare.</span><span class="sxs-lookup"><span data-stu-id="53eb1-134">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="53eb1-135">**Omdirigerings-URI** är en kombination av schemat och strängen som Azure AD som används för att returnera token svar.</span><span class="sxs-lookup"><span data-stu-id="53eb1-135">**Redirect URI** is a scheme and string combination that Azure AD uses to return token responses.</span></span> <span data-ttu-id="53eb1-136">Ange ett värde (till exempel http://DirectorySearcher).</span><span class="sxs-lookup"><span data-stu-id="53eb1-136">Enter a value (for example, http://DirectorySearcher).</span></span>
6. <span data-ttu-id="53eb1-137">När du har slutfört registreringen, tilldelar Azure AD appen ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="53eb1-137">After you’ve completed registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="53eb1-138">Kopiera värdet från den **programmet** fliken, eftersom du behöver senare.</span><span class="sxs-lookup"><span data-stu-id="53eb1-138">Copy the value from the **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="53eb1-139">På den **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="53eb1-139">On the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="53eb1-140">Välj **Microsoft Graph** som API.</span><span class="sxs-lookup"><span data-stu-id="53eb1-140">Select **Microsoft Graph** as the API.</span></span> <span data-ttu-id="53eb1-141">Under **delegerade behörigheter**, lägga till den **läsa katalogdata** behörighet.</span><span class="sxs-lookup"><span data-stu-id="53eb1-141">Under **Delegated Permissions**, add the **Read Directory Data** permission.</span></span>  
<span data-ttu-id="53eb1-142">Den här åtgärden aktiverar appen att fråga Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="53eb1-142">This action enables the app to query the Graph API for users.</span></span>

## <a name="step-3-install-and-configure-adal"></a><span data-ttu-id="53eb1-143">Steg 3: Installera och konfigurera ADAL</span><span class="sxs-lookup"><span data-stu-id="53eb1-143">Step 3: Install and configure ADAL</span></span>
<span data-ttu-id="53eb1-144">Nu när du har en app i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="53eb1-144">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="53eb1-145">Om du vill aktivera ADAL att kommunicera med Azure AD, ger det viss information om appregistrering.</span><span class="sxs-lookup"><span data-stu-id="53eb1-145">To enable ADAL to communicate with Azure AD, give it some information about the app registration.</span></span>

1. <span data-ttu-id="53eb1-146">Lägg till ADAL DirectorySearcher projektet med hjälp av Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="53eb1-146">Add ADAL to the DirectorySearcher project by using the Package Manager Console.</span></span>

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    `

    <span data-ttu-id="53eb1-147">Observera att två biblioteket referenser läggs till varje projekt: PCL-delen av ADAL och en plattformsspecifik del.</span><span class="sxs-lookup"><span data-stu-id="53eb1-147">Note that two library references are added to each project: the PCL portion of ADAL and a platform-specific portion.</span></span>
2. <span data-ttu-id="53eb1-148">Öppna DirectorySearcher.cs i DirectorySearcherLib-projektet.</span><span class="sxs-lookup"><span data-stu-id="53eb1-148">In the DirectorySearcherLib project, open DirectorySearcher.cs.</span></span>
3. <span data-ttu-id="53eb1-149">Ersätt klassen medlemsvärden med värden som du angav i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="53eb1-149">Replace the class member values with the values that you entered in the Azure portal.</span></span> <span data-ttu-id="53eb1-150">Din kod refererar till dessa värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="53eb1-150">Your code refers to these values whenever it uses ADAL.</span></span>

  * <span data-ttu-id="53eb1-151">Den *klient* är domänen för din Azure AD-klient (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="53eb1-151">The *tenant* is the domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="53eb1-152">Den *clientId* är klient-ID för appen, som du kopierade från portalen.</span><span class="sxs-lookup"><span data-stu-id="53eb1-152">The *clientId* is the client ID of the app, which you copied from the portal.</span></span>
  * <span data-ttu-id="53eb1-153">Den *returnUri* är omdirigerings-URI som du angav på portalen (till exempel http://DirectorySearcher).</span><span class="sxs-lookup"><span data-stu-id="53eb1-153">The *returnUri* is the redirect URI that you entered in the portal (for example, http://DirectorySearcher).</span></span>

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a><span data-ttu-id="53eb1-154">Steg 4: Använda ADAL att hämta token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="53eb1-154">Step 4: Use ADAL to get tokens from Azure AD</span></span>
<span data-ttu-id="53eb1-155">Nästan alla appens autentiseringslogiken ligger i `DirectorySearcher.SearchByAlias(...)`.</span><span class="sxs-lookup"><span data-stu-id="53eb1-155">Almost all of the app's authentication logic lies in `DirectorySearcher.SearchByAlias(...)`.</span></span> <span data-ttu-id="53eb1-156">Allt som behövs i plattformsspecifika-projekt är att skicka en kontextuella parameter till den `DirectorySearcher` PCL.</span><span class="sxs-lookup"><span data-stu-id="53eb1-156">All that's necessary in the platform-specific projects is to pass a contextual parameter to the `DirectorySearcher` PCL.</span></span>

1. <span data-ttu-id="53eb1-157">Öppna DirectorySearcher.cs och Lägg sedan till en ny parameter till den `SearchByAlias(...)` metoden.</span><span class="sxs-lookup"><span data-stu-id="53eb1-157">Open DirectorySearcher.cs, and then add a new parameter to the `SearchByAlias(...)` method.</span></span> <span data-ttu-id="53eb1-158">`IPlatformParameters`är parametern kontextuella som kapslar in plattformsspecifika objekten som ADAL behöver för att utföra autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="53eb1-158">`IPlatformParameters` is the contextual parameter that encapsulates the platform-specific objects that ADAL needs to perform the authentication.</span></span>

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. <span data-ttu-id="53eb1-159">Initiera `AuthenticationContext`, vilket är den primära klassen av ADAL.</span><span class="sxs-lookup"><span data-stu-id="53eb1-159">Initialize `AuthenticationContext`, which is the primary class of ADAL.</span></span>  
<span data-ttu-id="53eb1-160">Den här åtgärden klarar ADAL koordinaterna kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="53eb1-160">This action passes ADAL the coordinates it needs to communicate with Azure AD.</span></span>
3. <span data-ttu-id="53eb1-161">Anropa `AcquireTokenAsync(...)`, som tar emot den `IPlatformParameters` objekt och anropar autentiseringsflödet som krävs för att returnera en token till appen.</span><span class="sxs-lookup"><span data-stu-id="53eb1-161">Call `AcquireTokenAsync(...)`, which accepts the `IPlatformParameters` object and invokes the authentication flow that's necessary to return a token to the app.</span></span>

    ```C#
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    <span data-ttu-id="53eb1-162">`AcquireTokenAsync(...)`försöker först att returnera en token för den begärda resursen (Graph API i det här fallet) utan att användarna anger sina autentiseringsuppgifter (via cachelagring eller uppdatera gamla token).</span><span class="sxs-lookup"><span data-stu-id="53eb1-162">`AcquireTokenAsync(...)` first attempts to return a token for the requested resource (the Graph API in this case) without prompting users to enter their credentials (via caching or refreshing old tokens).</span></span> <span data-ttu-id="53eb1-163">Vid behov visar den användare inloggningssidan för Azure AD före hämtning av den begärda token.</span><span class="sxs-lookup"><span data-stu-id="53eb1-163">As necessary, it shows users the Azure AD sign-in page before acquiring the requested token.</span></span>
4. <span data-ttu-id="53eb1-164">Koppla den åtkomst-token till Graph API-begäran i den **auktorisering** huvud:</span><span class="sxs-lookup"><span data-stu-id="53eb1-164">Attach the access token to the Graph API request in the **Authorization** header:</span></span>

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

<span data-ttu-id="53eb1-165">Det är allt för den `DirectorySearcher` PCL och appen datorns identitetsrelaterade kod.</span><span class="sxs-lookup"><span data-stu-id="53eb1-165">That's all for the `DirectorySearcher` PCL and the app's identity-related code.</span></span> <span data-ttu-id="53eb1-166">Allt som återstår är att anropa den `SearchByAlias(...)` metod i vyer för varje plattform och vid behov för att lägga till kod för att korrekt hantera UI-livscykeln.</span><span class="sxs-lookup"><span data-stu-id="53eb1-166">All that remains is to call the `SearchByAlias(...)` method in each platform's views and, where necessary, to add code for correctly handling the UI lifecycle.</span></span>

### <a name="android"></a><span data-ttu-id="53eb1-167">Android</span><span class="sxs-lookup"><span data-stu-id="53eb1-167">Android</span></span>
1. <span data-ttu-id="53eb1-168">Lägga till ett anrop till MainActivity.cs, `SearchByAlias(...)` Klicka på knappen hanterare:</span><span class="sxs-lookup"><span data-stu-id="53eb1-168">In MainActivity.cs, add a call to `SearchByAlias(...)` in the button click handler:</span></span>

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. <span data-ttu-id="53eb1-169">Åsidosätta den `OnActivityResult` livscykel metod för att vidarebefordra någon autentisering omdirigeras till en lämplig metod.</span><span class="sxs-lookup"><span data-stu-id="53eb1-169">Override the `OnActivityResult` lifecycle method to forward any authentication redirects back to the appropriate method.</span></span> <span data-ttu-id="53eb1-170">ADAL innehåller en hjälpmetod för detta i Android:</span><span class="sxs-lookup"><span data-stu-id="53eb1-170">ADAL provides a helper method for this in Android:</span></span>

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a><span data-ttu-id="53eb1-171">Windows-skrivbordet</span><span class="sxs-lookup"><span data-stu-id="53eb1-171">Windows Desktop</span></span>
<span data-ttu-id="53eb1-172">Gör ett anrop till i MainWindow.xaml.cs, `SearchByAlias(...)` genom att skicka en `WindowInteropHelper` på skrivbordet `PlatformParameters` objekt:</span><span class="sxs-lookup"><span data-stu-id="53eb1-172">In MainWindow.xaml.cs, make a call to `SearchByAlias(...)` by passing a `WindowInteropHelper` in the desktop's `PlatformParameters` object:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a><span data-ttu-id="53eb1-173">iOS</span><span class="sxs-lookup"><span data-stu-id="53eb1-173">iOS</span></span>
<span data-ttu-id="53eb1-174">I DirSearchClient_iOSViewController.cs iOS `PlatformParameters` objektet tar en referens till View-Controller:</span><span class="sxs-lookup"><span data-stu-id="53eb1-174">In DirSearchClient_iOSViewController.cs, the iOS `PlatformParameters` object takes a reference to the View Controller:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a><span data-ttu-id="53eb1-175">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="53eb1-175">Windows Universal</span></span>
<span data-ttu-id="53eb1-176">Öppna MainPage.xaml.cs i den universella Windows- och sedan implementera den `Search` metoden.</span><span class="sxs-lookup"><span data-stu-id="53eb1-176">In Windows Universal, open MainPage.xaml.cs, and then implement the `Search` method.</span></span> <span data-ttu-id="53eb1-177">Den här metoden använder en hjälpmetod i ett projekt för att uppdatera Användargränssnittet vid behov.</span><span class="sxs-lookup"><span data-stu-id="53eb1-177">This method uses a helper method in a shared project to update UI as necessary.</span></span>

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a><span data-ttu-id="53eb1-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53eb1-178">What's next</span></span>
<span data-ttu-id="53eb1-179">Nu har du en fungerande Xamarin-app som kan autentisera användare och på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 över fem olika plattformar.</span><span class="sxs-lookup"><span data-stu-id="53eb1-179">You now have a working Xamarin app that can authenticate users and securely call web APIs by using OAuth 2.0 across five different platforms.</span></span>

<span data-ttu-id="53eb1-180">Om du inte redan har fyllts i din klient med användare, nu är det dags att göra det.</span><span class="sxs-lookup"><span data-stu-id="53eb1-180">If you haven’t already populated your tenant with users, now is the time to do so.</span></span>

1. <span data-ttu-id="53eb1-181">Kör appen DirectorySearcher och logga sedan in med en användare.</span><span class="sxs-lookup"><span data-stu-id="53eb1-181">Run your DirectorySearcher app, and then sign in with one of the users.</span></span>
2. <span data-ttu-id="53eb1-182">Sök efter andra användare baserat på deras UPN.</span><span class="sxs-lookup"><span data-stu-id="53eb1-182">Search for other users based on their UPN.</span></span>

<span data-ttu-id="53eb1-183">ADAL gör det enkelt att lägga till vanliga identity-funktioner i appen.</span><span class="sxs-lookup"><span data-stu-id="53eb1-183">ADAL makes it easy to incorporate common identity features into the app.</span></span> <span data-ttu-id="53eb1-184">Det tar hand om ändrad arbetet, till exempel hantering av cache OAuth protokollstöd presenterar en inloggning Användargränssnittet för användaren och uppdatera gått token.</span><span class="sxs-lookup"><span data-stu-id="53eb1-184">It takes care of all the dirty work for you, such as cache management, OAuth protocol support, presenting the user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="53eb1-185">Du behöver bara ett enda API-anrop, `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="53eb1-185">You need to know only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="53eb1-186">För referens, ladda ned den [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (utan dina konfigurationsvärden).</span><span class="sxs-lookup"><span data-stu-id="53eb1-186">For reference, download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="53eb1-187">Du kan nu gå vidare till ytterligare identitet scenarier.</span><span class="sxs-lookup"><span data-stu-id="53eb1-187">You can now move on to additional identity scenarios.</span></span> <span data-ttu-id="53eb1-188">Till exempel försöka [skydda ett .NET-webb-API med Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="53eb1-188">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

---
title: "aaaAzure AD Xamarin komma igång | Microsoft Docs"
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
ms.openlocfilehash: 6a0d189648b7071558ac1cf2b908808668960a4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a><span data-ttu-id="b7da6-103">Integrera Azure AD med Xamarin-appar</span><span class="sxs-lookup"><span data-stu-id="b7da6-103">Integrate Azure AD with Xamarin apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="b7da6-104">Du kan skriva mobila appar med Xamarin, i C# som kan köras på iOS, Android och Windows (mobila enheter och datorer).</span><span class="sxs-lookup"><span data-stu-id="b7da6-104">With Xamarin, you can write mobile apps in C# that can run on iOS, Android, and Windows (mobile devices and PCs).</span></span> <span data-ttu-id="b7da6-105">Om du utvecklar en app med Xamarin, Azure Active Directory (AD Azure) som gör att det enkla tooauthenticate användare med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="b7da6-105">If you're building an app using Xamarin, Azure Active Directory (Azure AD) makes it simple tooauthenticate users with their Azure AD accounts.</span></span> <span data-ttu-id="b7da6-106">hello app kan också säkert att använda alla webb-API som skyddas av Azure AD, till exempel hello Office 365-API: er eller hello Azure API.</span><span class="sxs-lookup"><span data-stu-id="b7da6-106">hello app can also securely consume any web API that's protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="b7da6-107">Azure AD innehåller hello Active Directory Authentication Library (ADAL) för Xamarin-appar som behöver tooaccess skyddade resurser.</span><span class="sxs-lookup"><span data-stu-id="b7da6-107">For Xamarin apps that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="b7da6-108">hello uteslutande av ADAL är toomake det enkelt för appar tooget åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="b7da6-108">hello sole purpose of ADAL is toomake it easy for apps tooget access tokens.</span></span> <span data-ttu-id="b7da6-109">toodemonstrate hur lätt det är den här artikeln visar hur toobuild DirectorySearcher appar som:</span><span class="sxs-lookup"><span data-stu-id="b7da6-109">toodemonstrate how easy it is, this article shows how toobuild DirectorySearcher apps that:</span></span>

* <span data-ttu-id="b7da6-110">Kör på iOS, Android, Windows-skrivbordet, Windows Phone och Windows Store.</span><span class="sxs-lookup"><span data-stu-id="b7da6-110">Run on iOS, Android, Windows Desktop, Windows Phone, and Windows Store.</span></span>
* <span data-ttu-id="b7da6-111">Använda en enda bärbar klass bibliotek (PCL) tooauthenticate användare och hämta token för hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="b7da6-111">Use a single portable class library (PCL) tooauthenticate users and get tokens for hello Azure AD Graph API.</span></span>
* <span data-ttu-id="b7da6-112">Söka i en katalog för användare med ett angivet UPN.</span><span class="sxs-lookup"><span data-stu-id="b7da6-112">Search a directory for users with a given UPN.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="b7da6-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b7da6-113">Before you get started</span></span>
* <span data-ttu-id="b7da6-114">Hämta hello [stommen av ett projekt](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), eller hämta hello [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="b7da6-114">Download hello [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), or download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="b7da6-115">Varje version är en lösning i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b7da6-115">Each download is a Visual Studio 2013 solution.</span></span>
* <span data-ttu-id="b7da6-116">Du måste också en Azure AD-klient i vilka toocreate användare och registrera hello app.</span><span class="sxs-lookup"><span data-stu-id="b7da6-116">You also need an Azure AD tenant in which toocreate users and register hello app.</span></span> <span data-ttu-id="b7da6-117">Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="b7da6-117">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="b7da6-118">När du är klar hello Följ hello procedurerna i följande fyra avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b7da6-118">When you are ready, follow hello procedures in hello next four sections.</span></span>

## <a name="step-1-set-up-your-xamarin-development-environment"></a><span data-ttu-id="b7da6-119">Steg 1: Ställ in din utvecklingsmiljö för Xamarin</span><span class="sxs-lookup"><span data-stu-id="b7da6-119">Step 1: Set up your Xamarin development environment</span></span>
<span data-ttu-id="b7da6-120">Eftersom den här självstudiekursen innehåller projekt för iOS, Android och Windows, måste både Visual Studio och Xamarin.</span><span class="sxs-lookup"><span data-stu-id="b7da6-120">Because this tutorial includes projects for iOS, Android, and Windows, you need both Visual Studio and Xamarin.</span></span> <span data-ttu-id="b7da6-121">toocreate hello nödvändiga miljö, fullständig hello processen i [ställa upp och installera Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="b7da6-121">toocreate hello necessary environment, complete hello process in [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) on MSDN.</span></span> <span data-ttu-id="b7da6-122">hello anvisningarna inkluderar material som du kan granska toolearn mer om Xamarin medan du väntar hello installationer toobe slutförts.</span><span class="sxs-lookup"><span data-stu-id="b7da6-122">hello instructions include material that you can review toolearn more about Xamarin while you're waiting for hello installations toobe completed.</span></span>

<span data-ttu-id="b7da6-123">När du har slutfört installationen hello, öppna hello lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b7da6-123">After you've completed hello setup, open hello solution in Visual Studio.</span></span> <span data-ttu-id="b7da6-124">Där hittar du sex projekt: fem plattformsspecifika projekt och en PCL, DirectorySearcher.cs som delas mellan alla plattformar.</span><span class="sxs-lookup"><span data-stu-id="b7da6-124">There, you will find six projects: five platform-specific projects and one PCL, DirectorySearcher.cs, which will be shared across all platforms.</span></span>

## <a name="step-2-register-hello-directorysearcher-app"></a><span data-ttu-id="b7da6-125">Steg 2: Registrera hello DirectorySearcher app</span><span class="sxs-lookup"><span data-stu-id="b7da6-125">Step 2: Register hello DirectorySearcher app</span></span>
<span data-ttu-id="b7da6-126">tooenable hello tooget apptoken, måste du först tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="b7da6-126">tooenable hello app tooget tokens, you first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API.</span></span> <span data-ttu-id="b7da6-127">Så här gör du:</span><span class="sxs-lookup"><span data-stu-id="b7da6-127">Here's how:</span></span>

1. <span data-ttu-id="b7da6-128">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b7da6-128">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b7da6-129">Klicka på ditt konto hello översta fältet.</span><span class="sxs-lookup"><span data-stu-id="b7da6-129">On hello top bar, click your account.</span></span> <span data-ttu-id="b7da6-130">Sedan, under hello **Directory** listan, Välj hello Active Directory-klient där du vill att tooregister hello app.</span><span class="sxs-lookup"><span data-stu-id="b7da6-130">Then, under hello **Directory** list, select hello Active Directory tenant where you want tooregister hello app.</span></span>
3. <span data-ttu-id="b7da6-131">Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b7da6-131">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="b7da6-132">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b7da6-132">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="b7da6-133">toocreate en ny **internt klientprogram**, följer du anvisningarna för hello.</span><span class="sxs-lookup"><span data-stu-id="b7da6-133">toocreate a new **Native Client Application**, follow hello prompts.</span></span>
  * <span data-ttu-id="b7da6-134">**Namnet** beskriver hello app toousers.</span><span class="sxs-lookup"><span data-stu-id="b7da6-134">**Name** describes hello app toousers.</span></span>
  * <span data-ttu-id="b7da6-135">**Omdirigerings-URI** är en kombination av schemat och strängen som använder Azure AD tooreturn token svar.</span><span class="sxs-lookup"><span data-stu-id="b7da6-135">**Redirect URI** is a scheme and string combination that Azure AD uses tooreturn token responses.</span></span> <span data-ttu-id="b7da6-136">Ange ett värde (till exempel http://DirectorySearcher).</span><span class="sxs-lookup"><span data-stu-id="b7da6-136">Enter a value (for example, http://DirectorySearcher).</span></span>
6. <span data-ttu-id="b7da6-137">När du har slutfört registreringen, tilldelar Azure AD hello app ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="b7da6-137">After you’ve completed registration, Azure AD assigns hello app a unique application ID.</span></span> <span data-ttu-id="b7da6-138">Kopiera hello värdet från hello **programmet** fliken, eftersom du behöver senare.</span><span class="sxs-lookup"><span data-stu-id="b7da6-138">Copy hello value from hello **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="b7da6-139">På hello **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b7da6-139">On hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="b7da6-140">Välj **Microsoft Graph** som hello API.</span><span class="sxs-lookup"><span data-stu-id="b7da6-140">Select **Microsoft Graph** as hello API.</span></span> <span data-ttu-id="b7da6-141">Under **delegerade behörigheter**, lägga till hello **läsa katalogdata** behörighet.</span><span class="sxs-lookup"><span data-stu-id="b7da6-141">Under **Delegated Permissions**, add hello **Read Directory Data** permission.</span></span>  
<span data-ttu-id="b7da6-142">Den här åtgärden aktiverar hello app tooquery hello Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="b7da6-142">This action enables hello app tooquery hello Graph API for users.</span></span>

## <a name="step-3-install-and-configure-adal"></a><span data-ttu-id="b7da6-143">Steg 3: Installera och konfigurera ADAL</span><span class="sxs-lookup"><span data-stu-id="b7da6-143">Step 3: Install and configure ADAL</span></span>
<span data-ttu-id="b7da6-144">Nu när du har en app i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="b7da6-144">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="b7da6-145">tooenable ADAL toocommunicate med Azure AD, ger det viss information om hello appregistrering.</span><span class="sxs-lookup"><span data-stu-id="b7da6-145">tooenable ADAL toocommunicate with Azure AD, give it some information about hello app registration.</span></span>

1. <span data-ttu-id="b7da6-146">Lägg till ADAL toohello DirectorySearcher projektet genom att använda hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="b7da6-146">Add ADAL toohello DirectorySearcher project by using hello Package Manager Console.</span></span>

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

    <span data-ttu-id="b7da6-147">Observera att två biblioteket referenser läggs tooeach projekt: hello PCL delen av ADAL och en plattformsspecifik del.</span><span class="sxs-lookup"><span data-stu-id="b7da6-147">Note that two library references are added tooeach project: hello PCL portion of ADAL and a platform-specific portion.</span></span>
2. <span data-ttu-id="b7da6-148">Öppna DirectorySearcher.cs i hello DirectorySearcherLib projekt.</span><span class="sxs-lookup"><span data-stu-id="b7da6-148">In hello DirectorySearcherLib project, open DirectorySearcher.cs.</span></span>
3. <span data-ttu-id="b7da6-149">Ersätt hello klassen medlemsvärden med hello-värden som du angav i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7da6-149">Replace hello class member values with hello values that you entered in hello Azure portal.</span></span> <span data-ttu-id="b7da6-150">Din kod refererar toothese värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="b7da6-150">Your code refers toothese values whenever it uses ADAL.</span></span>

  * <span data-ttu-id="b7da6-151">Hej *klient* är hello domän för din Azure AD-klient (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="b7da6-151">hello *tenant* is hello domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="b7da6-152">Hej *clientId* är hello klient-ID för hello-app som du kopierade från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7da6-152">hello *clientId* is hello client ID of hello app, which you copied from hello portal.</span></span>
  * <span data-ttu-id="b7da6-153">Hej *returnUri* är hello omdirigerings-URI som du angav i hello-portal (till exempel http://DirectorySearcher).</span><span class="sxs-lookup"><span data-stu-id="b7da6-153">hello *returnUri* is hello redirect URI that you entered in hello portal (for example, http://DirectorySearcher).</span></span>

## <a name="step-4-use-adal-tooget-tokens-from-azure-ad"></a><span data-ttu-id="b7da6-154">Steg 4: Använda ADAL tooget token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7da6-154">Step 4: Use ADAL tooget tokens from Azure AD</span></span>
<span data-ttu-id="b7da6-155">Nästan alla hello app autentiseringslogiken ligger i `DirectorySearcher.SearchByAlias(...)`.</span><span class="sxs-lookup"><span data-stu-id="b7da6-155">Almost all of hello app's authentication logic lies in `DirectorySearcher.SearchByAlias(...)`.</span></span> <span data-ttu-id="b7da6-156">Allt som behövs i hello plattformsspecifika projekt är toopass en kontextuella parametern toohello `DirectorySearcher` PCL.</span><span class="sxs-lookup"><span data-stu-id="b7da6-156">All that's necessary in hello platform-specific projects is toopass a contextual parameter toohello `DirectorySearcher` PCL.</span></span>

1. <span data-ttu-id="b7da6-157">Öppna DirectorySearcher.cs och Lägg sedan till en ny parameter toohello `SearchByAlias(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="b7da6-157">Open DirectorySearcher.cs, and then add a new parameter toohello `SearchByAlias(...)` method.</span></span> <span data-ttu-id="b7da6-158">`IPlatformParameters`är sammanhangsberoende hello-parameter som kapslar in hello plattformsspecifika objekt att ADAL måste tooperform hello-autentisering.</span><span class="sxs-lookup"><span data-stu-id="b7da6-158">`IPlatformParameters` is hello contextual parameter that encapsulates hello platform-specific objects that ADAL needs tooperform hello authentication.</span></span>

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. <span data-ttu-id="b7da6-159">Initiera `AuthenticationContext`, vilket är hello primära klassen av ADAL.</span><span class="sxs-lookup"><span data-stu-id="b7da6-159">Initialize `AuthenticationContext`, which is hello primary class of ADAL.</span></span>  
<span data-ttu-id="b7da6-160">Den här åtgärden överför ADAL hello samordnar den behov toocommunicate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7da6-160">This action passes ADAL hello coordinates it needs toocommunicate with Azure AD.</span></span>
3. <span data-ttu-id="b7da6-161">Anropa `AcquireTokenAsync(...)`, som accepterar hello `IPlatformParameters` objekt och anropar hello autentiseringsflödet som är nödvändiga tooreturn en token toohello app.</span><span class="sxs-lookup"><span data-stu-id="b7da6-161">Call `AcquireTokenAsync(...)`, which accepts hello `IPlatformParameters` object and invokes hello authentication flow that's necessary tooreturn a token toohello app.</span></span>

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

    <span data-ttu-id="b7da6-162">`AcquireTokenAsync(...)`första försök tooreturn en token för hello begärd resurs (hello Graph API i det här fallet) utan att fråga användare tooenter sina autentiseringsuppgifter (via cachelagring eller uppdatera gamla token).</span><span class="sxs-lookup"><span data-stu-id="b7da6-162">`AcquireTokenAsync(...)` first attempts tooreturn a token for hello requested resource (hello Graph API in this case) without prompting users tooenter their credentials (via caching or refreshing old tokens).</span></span> <span data-ttu-id="b7da6-163">Vid behov visar den användare hello Azure AD-inloggningssida före hämtning hello begärda token.</span><span class="sxs-lookup"><span data-stu-id="b7da6-163">As necessary, it shows users hello Azure AD sign-in page before acquiring hello requested token.</span></span>
4. <span data-ttu-id="b7da6-164">Kopplingsbegäran hello åtkomst-token toohello Graph API i hello **auktorisering** huvud:</span><span class="sxs-lookup"><span data-stu-id="b7da6-164">Attach hello access token toohello Graph API request in hello **Authorization** header:</span></span>

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

<span data-ttu-id="b7da6-165">Det är allt för hello `DirectorySearcher` PCL och hello appens identitetsrelaterade kod.</span><span class="sxs-lookup"><span data-stu-id="b7da6-165">That's all for hello `DirectorySearcher` PCL and hello app's identity-related code.</span></span> <span data-ttu-id="b7da6-166">Allt som återstår är toocall hello `SearchByAlias(...)` metod i vyer för varje plattform och, vid behov tooadd kod för att korrekt hantera hello UI livscykel.</span><span class="sxs-lookup"><span data-stu-id="b7da6-166">All that remains is toocall hello `SearchByAlias(...)` method in each platform's views and, where necessary, tooadd code for correctly handling hello UI lifecycle.</span></span>

### <a name="android"></a><span data-ttu-id="b7da6-167">Android</span><span class="sxs-lookup"><span data-stu-id="b7da6-167">Android</span></span>
1. <span data-ttu-id="b7da6-168">MainActivity.cs, lägga till ett anrop för`SearchByAlias(...)` i hello knappen klickar du på hanterare:</span><span class="sxs-lookup"><span data-stu-id="b7da6-168">In MainActivity.cs, add a call too`SearchByAlias(...)` in hello button click handler:</span></span>

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. <span data-ttu-id="b7da6-169">Åsidosätt hello `OnActivityResult` livscykel metoden tooforward någon autentisering omdirigerar tillbaka toohello lämplig metod.</span><span class="sxs-lookup"><span data-stu-id="b7da6-169">Override hello `OnActivityResult` lifecycle method tooforward any authentication redirects back toohello appropriate method.</span></span> <span data-ttu-id="b7da6-170">ADAL innehåller en hjälpmetod för detta i Android:</span><span class="sxs-lookup"><span data-stu-id="b7da6-170">ADAL provides a helper method for this in Android:</span></span>

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a><span data-ttu-id="b7da6-171">Windows-skrivbordet</span><span class="sxs-lookup"><span data-stu-id="b7da6-171">Windows Desktop</span></span>
<span data-ttu-id="b7da6-172">I MainWindow.xaml.cs, gör ett anrop för`SearchByAlias(...)` genom att skicka en `WindowInteropHelper` i hello desktop `PlatformParameters` objekt:</span><span class="sxs-lookup"><span data-stu-id="b7da6-172">In MainWindow.xaml.cs, make a call too`SearchByAlias(...)` by passing a `WindowInteropHelper` in hello desktop's `PlatformParameters` object:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a><span data-ttu-id="b7da6-173">iOS</span><span class="sxs-lookup"><span data-stu-id="b7da6-173">iOS</span></span>
<span data-ttu-id="b7da6-174">I DirSearchClient_iOSViewController.cs, hello iOS `PlatformParameters` objektet tar en referens toohello View Controller:</span><span class="sxs-lookup"><span data-stu-id="b7da6-174">In DirSearchClient_iOSViewController.cs, hello iOS `PlatformParameters` object takes a reference toohello View Controller:</span></span>

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a><span data-ttu-id="b7da6-175">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="b7da6-175">Windows Universal</span></span>
<span data-ttu-id="b7da6-176">Öppna MainPage.xaml.cs i den universella Windows- och sedan implementera hello `Search` metod.</span><span class="sxs-lookup"><span data-stu-id="b7da6-176">In Windows Universal, open MainPage.xaml.cs, and then implement hello `Search` method.</span></span> <span data-ttu-id="b7da6-177">Den här metoden använder en hjälpmetod i ett delat projekt tooupdate Användargränssnittet vid behov.</span><span class="sxs-lookup"><span data-stu-id="b7da6-177">This method uses a helper method in a shared project tooupdate UI as necessary.</span></span>

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a><span data-ttu-id="b7da6-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7da6-178">What's next</span></span>
<span data-ttu-id="b7da6-179">Nu har du en fungerande Xamarin-app som kan autentisera användare och på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 över fem olika plattformar.</span><span class="sxs-lookup"><span data-stu-id="b7da6-179">You now have a working Xamarin app that can authenticate users and securely call web APIs by using OAuth 2.0 across five different platforms.</span></span>

<span data-ttu-id="b7da6-180">Om du inte redan har fyllts i din klient med användare, är nu hello tid toodo så.</span><span class="sxs-lookup"><span data-stu-id="b7da6-180">If you haven’t already populated your tenant with users, now is hello time toodo so.</span></span>

1. <span data-ttu-id="b7da6-181">Kör appen DirectorySearcher och logga sedan in med någon av hello användare.</span><span class="sxs-lookup"><span data-stu-id="b7da6-181">Run your DirectorySearcher app, and then sign in with one of hello users.</span></span>
2. <span data-ttu-id="b7da6-182">Sök efter andra användare baserat på deras UPN.</span><span class="sxs-lookup"><span data-stu-id="b7da6-182">Search for other users based on their UPN.</span></span>

<span data-ttu-id="b7da6-183">ADAL gör det enkelt tooincorporate vanliga identity-funktioner i hello app.</span><span class="sxs-lookup"><span data-stu-id="b7da6-183">ADAL makes it easy tooincorporate common identity features into hello app.</span></span> <span data-ttu-id="b7da6-184">Det tar hand om alla hello ändrad arbete för dig, t.ex hantering av cache OAuth protokollstöd presentera hello användare med en inloggning-Gränssnittet och uppdatera gått token.</span><span class="sxs-lookup"><span data-stu-id="b7da6-184">It takes care of all hello dirty work for you, such as cache management, OAuth protocol support, presenting hello user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="b7da6-185">Du behöver tooknow bara ett enda API-anrop `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="b7da6-185">You need tooknow only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="b7da6-186">Hämta hello referens [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (utan dina konfigurationsvärden).</span><span class="sxs-lookup"><span data-stu-id="b7da6-186">For reference, download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="b7da6-187">Du kan nu gå vidare tooadditional identitet scenarier.</span><span class="sxs-lookup"><span data-stu-id="b7da6-187">You can now move on tooadditional identity scenarios.</span></span> <span data-ttu-id="b7da6-188">Till exempel försöka [skydda ett .NET-webb-API med Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="b7da6-188">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

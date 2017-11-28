---
title: "aaaAzure AD Windows Store komma igång | Microsoft Docs"
description: "Skapa Windows Store-appar som integreras med Azure AD för inloggning och anropar Azure AD skyddade API: er som använder sig av OAuth."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 09/16/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 1d12c7b928bc0e94fb823f8db4a09ff416205e2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-windows-store-apps"></a><span data-ttu-id="c9738-103">Integrera Azure AD med Windows Store-appar</span><span class="sxs-lookup"><span data-stu-id="c9738-103">Integrate Azure AD with Windows Store apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="c9738-104">Windows Store 8.1 och tidigare version projekt stöds inte i Visual Studio-2017.</span><span class="sxs-lookup"><span data-stu-id="c9738-104">Windows Store 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="c9738-105">Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).</span><span class="sxs-lookup"><span data-stu-id="c9738-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="c9738-106">Om du utvecklar appar för Windows Store för hello Azure Active Directory (AD Azure) gör det lätt tooauthenticate dina användare med deras Active Directory-konton.</span><span class="sxs-lookup"><span data-stu-id="c9738-106">If you're developing apps for hello Windows Store, Azure Active Directory (Azure AD) makes it simple and straightforward tooauthenticate your users with their Active Directory accounts.</span></span> <span data-ttu-id="c9738-107">En app kan på ett säkert sätt använda alla webb-API som skyddas av Azure AD, till exempel hello Office 365-API: er eller hello Azure API genom att integrera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9738-107">By integrating with Azure AD, an app can securely consume any web API that's protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="c9738-108">För Windows Store-program som behöver tooaccess skyddade resurser, innehåller Azure AD hello Active Directory Authentication Library (ADAL).</span><span class="sxs-lookup"><span data-stu-id="c9738-108">For Windows Store desktop apps that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="c9738-109">hello uteslutande av ADAL är toomake det enkelt för hello app tooget åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="c9738-109">hello sole purpose of ADAL is toomake it easy for hello app tooget access tokens.</span></span> <span data-ttu-id="c9738-110">toodemonstrate hur lätt det är den här artikeln visar hur toobuild en DirectorySearcher Windows Store-app som:</span><span class="sxs-lookup"><span data-stu-id="c9738-110">toodemonstrate how easy it is, this article shows how toobuild a DirectorySearcher Windows Store app that:</span></span>

* <span data-ttu-id="c9738-111">Hämtar åtkomst till token för att anropa hello Azure AD Graph API med hjälp av hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="c9738-111">Gets access tokens for calling hello Azure AD Graph API by using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="c9738-112">Söker en katalog för användare med en viss användarens huvudnamn (UPN).</span><span class="sxs-lookup"><span data-stu-id="c9738-112">Searches a directory for users with a given user principal name (UPN).</span></span>
* <span data-ttu-id="c9738-113">Användare loggar ut.</span><span class="sxs-lookup"><span data-stu-id="c9738-113">Signs users out.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="c9738-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c9738-114">Before you get started</span></span>
* <span data-ttu-id="c9738-115">Hämta hello [stommen av ett projekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), eller hämta hello [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="c9738-115">Download hello [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), or download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span></span> <span data-ttu-id="c9738-116">Varje version är en lösning i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="c9738-116">Each download is a Visual Studio 2015 solution.</span></span>
* <span data-ttu-id="c9738-117">Du måste också en Azure AD-klient i vilka toocreate användare och registrera hello app.</span><span class="sxs-lookup"><span data-stu-id="c9738-117">You also need an Azure AD tenant in which toocreate users and register hello app.</span></span> <span data-ttu-id="c9738-118">Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="c9738-118">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="c9738-119">När du är klar hello Följ hello procedurerna i följande tre avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c9738-119">When you are ready, follow hello procedures in hello next three sections.</span></span>

## <a name="step-1-register-hello-directorysearcher-app"></a><span data-ttu-id="c9738-120">Steg 1: Registrera hello DirectorySearcher app</span><span class="sxs-lookup"><span data-stu-id="c9738-120">Step 1: Register hello DirectorySearcher app</span></span>
<span data-ttu-id="c9738-121">tooenable hello tooget apptoken, måste du först tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="c9738-121">tooenable hello app tooget tokens, you first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API.</span></span> <span data-ttu-id="c9738-122">Så här gör du:</span><span class="sxs-lookup"><span data-stu-id="c9738-122">Here's how:</span></span>

1. <span data-ttu-id="c9738-123">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c9738-123">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c9738-124">Klicka på ditt konto hello översta fältet.</span><span class="sxs-lookup"><span data-stu-id="c9738-124">On hello top bar, click your account.</span></span> <span data-ttu-id="c9738-125">Sedan, under hello **Directory** listan, Välj hello Active Directory-klient där du vill att tooregister hello app.</span><span class="sxs-lookup"><span data-stu-id="c9738-125">Then, under hello **Directory** list, select hello Active Directory tenant where you want tooregister hello app.</span></span>
3. <span data-ttu-id="c9738-126">Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="c9738-126">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="c9738-127">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c9738-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="c9738-128">Följ hello prompter toocreate en **internt klientprogram**.</span><span class="sxs-lookup"><span data-stu-id="c9738-128">Follow hello prompts toocreate a **Native Client Application**.</span></span>
  * <span data-ttu-id="c9738-129">**Namnet** beskriver hello app toousers.</span><span class="sxs-lookup"><span data-stu-id="c9738-129">**Name** describes hello app toousers.</span></span>
  * <span data-ttu-id="c9738-130">**Omdirigerings-URI** är en kombination av schemat och strängen som använder Azure AD tooreturn token svar.</span><span class="sxs-lookup"><span data-stu-id="c9738-130">**Redirect URI** is a scheme and string combination that Azure AD uses tooreturn token responses.</span></span> <span data-ttu-id="c9738-131">Ange ett platshållarvärde för nu (till exempel **http://DirectorySearcher**).</span><span class="sxs-lookup"><span data-stu-id="c9738-131">Enter a placeholder value for now (for example, **http://DirectorySearcher**).</span></span> <span data-ttu-id="c9738-132">Du måste ersätta hello värdet senare.</span><span class="sxs-lookup"><span data-stu-id="c9738-132">You'll replace hello value later.</span></span>
6. <span data-ttu-id="c9738-133">När du har slutfört registreringen hello tilldelar Azure AD hello app ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="c9738-133">After you’ve completed hello registration, Azure AD assigns hello app a unique application ID.</span></span> <span data-ttu-id="c9738-134">Kopiera hello värde på hello **programmet** fliken, eftersom du behöver senare.</span><span class="sxs-lookup"><span data-stu-id="c9738-134">Copy hello value on hello **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="c9738-135">På hello **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c9738-135">On hello **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="c9738-136">För hello **Azure Active Directory** app markerar **Microsoft Graph** som hello API.</span><span class="sxs-lookup"><span data-stu-id="c9738-136">For hello **Azure Active Directory** app, select **Microsoft Graph** as hello API.</span></span>
9. <span data-ttu-id="c9738-137">Under **delegerade behörigheter**, lägga till hello **Access hello-katalogen som hello inloggade användare** behörighet.</span><span class="sxs-lookup"><span data-stu-id="c9738-137">Under **Delegated Permissions**, add hello **Access hello directory as hello signed-in user** permission.</span></span> <span data-ttu-id="c9738-138">På så sätt kan hello app tooquery hello Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="c9738-138">Doing so enables hello app tooquery hello Graph API for users.</span></span>

## <a name="step-2-install-and-configure-adal"></a><span data-ttu-id="c9738-139">Steg 2: Installera och konfigurera ADAL</span><span class="sxs-lookup"><span data-stu-id="c9738-139">Step 2: Install and configure ADAL</span></span>
<span data-ttu-id="c9738-140">Nu när du har en app i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="c9738-140">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="c9738-141">tooenable ADAL toocommunicate med Azure AD, ger det viss information om hello appregistrering.</span><span class="sxs-lookup"><span data-stu-id="c9738-141">tooenable ADAL toocommunicate with Azure AD, give it some information about hello app registration.</span></span>

1. <span data-ttu-id="c9738-142">Lägg till ADAL toohello DirectorySearcher projektet genom att använda hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="c9738-142">Add ADAL toohello DirectorySearcher project by using hello Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. <span data-ttu-id="c9738-143">Öppna MainPage.xaml.cs i hello DirectorySearcher projekt.</span><span class="sxs-lookup"><span data-stu-id="c9738-143">In hello DirectorySearcher project, open MainPage.xaml.cs.</span></span>
3. <span data-ttu-id="c9738-144">Ersätt hello värden i hello **konfigurationsvärden** region med hello-värden som du angav i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c9738-144">Replace hello values in hello **Config Values** region with hello values that you entered in hello Azure portal.</span></span> <span data-ttu-id="c9738-145">Din kod refererar toothese värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="c9738-145">Your code refers toothese values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="c9738-146">Hej *klient* är hello domän för din Azure AD-klient (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="c9738-146">hello *tenant* is hello domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="c9738-147">Hej *clientId* är hello klient-ID för hello-app som du kopierade från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="c9738-147">hello *clientId* is hello client ID of hello app, which you copied from hello portal.</span></span>
4. <span data-ttu-id="c9738-148">Du måste nu toodiscover hello återanrop URI för Windows Store-app.</span><span class="sxs-lookup"><span data-stu-id="c9738-148">You now need toodiscover hello callback URI for your Windows Store app.</span></span> <span data-ttu-id="c9738-149">Ange en brytpunkt på den här raden i hello `MainPage` metoden:</span><span class="sxs-lookup"><span data-stu-id="c9738-149">Set a breakpoint on this line in hello `MainPage` method:</span></span>
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. <span data-ttu-id="c9738-150">Skapa hello-lösning, se till att alla paketet refererar till har återställts.</span><span class="sxs-lookup"><span data-stu-id="c9738-150">Build hello solution, making sure that all package references are restored.</span></span> <span data-ttu-id="c9738-151">Om några paket saknas, öppna hello NuGet Package Manager och återställa dem.</span><span class="sxs-lookup"><span data-stu-id="c9738-151">If any packages are missing, open hello NuGet Package Manager and restore them.</span></span>
6. <span data-ttu-id="c9738-152">Kör hello appen och kopiera hello värdet för `redirectUri` när hello brytpunkt träffar.</span><span class="sxs-lookup"><span data-stu-id="c9738-152">Run hello app, and copy hello value of `redirectUri` when hello breakpoint is hit.</span></span> <span data-ttu-id="c9738-153">hello värde ska se ut ungefär hello följande:</span><span class="sxs-lookup"><span data-stu-id="c9738-153">hello value should look something like hello following:</span></span>

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. <span data-ttu-id="c9738-154">Tillbaka på hello **inställningar** för hello app i hello Azure-portalen, lägga till en **RedirectUri** med hello före värdet.</span><span class="sxs-lookup"><span data-stu-id="c9738-154">Back on hello **Settings** tab of hello app in hello Azure portal, add a **RedirectUri** with hello preceding value.</span></span>  

## <a name="step-3-use-adal-tooget-tokens-from-azure-ad"></a><span data-ttu-id="c9738-155">Steg 3: Använda ADAL tooget token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9738-155">Step 3: Use ADAL tooget tokens from Azure AD</span></span>
<span data-ttu-id="c9738-156">hello grundläggande principen bakom ADAL är att när hello app måste en åtkomst-token, bara anropas `authContext.AcquireToken(…)`, och ADAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="c9738-156">hello basic principle behind ADAL is that whenever hello app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does hello rest.</span></span>  

1. <span data-ttu-id="c9738-157">Initiera hello app `AuthenticationContext`, vilket är hello primära klassen av ADAL.</span><span class="sxs-lookup"><span data-stu-id="c9738-157">Initialize hello app’s `AuthenticationContext`, which is hello primary class of ADAL.</span></span> <span data-ttu-id="c9738-158">Den här åtgärden klarar ADAL hello koordinater den behöver toocommunicate med Azure AD och anger hur toocache token.</span><span class="sxs-lookup"><span data-stu-id="c9738-158">This action passes ADAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. <span data-ttu-id="c9738-159">Leta upp hello `Search(...)` metod som anropas när användaren klickar på hello **Sök** knappen hello appens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c9738-159">Locate hello `Search(...)` method, which is invoked when users click hello **Search** button on hello app's UI.</span></span> <span data-ttu-id="c9738-160">Den här metoden gör en get-begäran toohello Azure AD Graph API tooquery för användare vars UPN-namnet börjar med hello angivna sökord.</span><span class="sxs-lookup"><span data-stu-id="c9738-160">This method makes a get request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span> <span data-ttu-id="c9738-161">tooquery hello Graph API är en åtkomst-token i hello begäran **auktorisering** huvud.</span><span class="sxs-lookup"><span data-stu-id="c9738-161">tooquery hello Graph API, include an access token in hello request's **Authorization** header.</span></span> <span data-ttu-id="c9738-162">Det är där ADAL kommer in.</span><span class="sxs-lookup"><span data-stu-id="c9738-162">This is where ADAL comes in.</span></span>

    ```C#
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    <span data-ttu-id="c9738-163">När hello app begär en token genom att anropa `AcquireTokenAsync(...)`, ADAL försöker tooreturn en token utan att uppge autentiseringsuppgifter hello användare.</span><span class="sxs-lookup"><span data-stu-id="c9738-163">When hello app requests a token by calling `AcquireTokenAsync(...)`, ADAL attempts tooreturn a token without asking hello user for credentials.</span></span> <span data-ttu-id="c9738-164">Om ADAL avgör hello användaren måste toosign i tooget en token, den visar inloggning dialogrutan samlar in hello användarens autentiseringsuppgifter och returnerar en token efter att autentiseringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="c9738-164">If ADAL determines that hello user needs toosign in tooget a token, it displays a sign-in dialog box, collects hello user's credentials, and returns a token after authentication succeeds.</span></span> <span data-ttu-id="c9738-165">Om ADAL är tooreturn en token av någon anledning, hello *AuthenticationResult* status är ett fel.</span><span class="sxs-lookup"><span data-stu-id="c9738-165">If ADAL is unable tooreturn a token for any reason, hello *AuthenticationResult* status is an error.</span></span>
3. <span data-ttu-id="c9738-166">Nu är det tid toouse hello åtkomsttoken du just har skaffat.</span><span class="sxs-lookup"><span data-stu-id="c9738-166">Now it's time toouse hello access token you just acquired.</span></span> <span data-ttu-id="c9738-167">Även i hello `Search(...)` metod, bifoga hello token toohello Graph API hämta begäran i hello **auktorisering** huvud:</span><span class="sxs-lookup"><span data-stu-id="c9738-167">Also in hello `Search(...)` method, attach hello token toohello Graph API get request in hello **Authorization** header:</span></span>

    ```C#
    // Add hello access token toohello Authorization header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. <span data-ttu-id="c9738-168">Du kan använda hello `AuthenticationResult` objekt toodisplay information om hello användare i hello appen, till exempel hello användar-ID:</span><span class="sxs-lookup"><span data-stu-id="c9738-168">You can use hello `AuthenticationResult` object toodisplay information about hello user in hello app, such as hello user's ID:</span></span>

    ```C#
    // Update hello page UI toorepresent hello signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. <span data-ttu-id="c9738-169">Du kan också använda ADAL toosign användare utanför hello appen.</span><span class="sxs-lookup"><span data-stu-id="c9738-169">You can also use ADAL toosign users out of hello app.</span></span> <span data-ttu-id="c9738-170">När hello användaren klickar på hello **logga ut** knappen, se till att hello nästa anrop för`AcquireTokenAsync(...)` visas en vy för inloggning.</span><span class="sxs-lookup"><span data-stu-id="c9738-170">When hello user clicks hello **Sign Out** button, ensure that hello next call too`AcquireTokenAsync(...)` shows a sign-in view.</span></span> <span data-ttu-id="c9738-171">Den här åtgärden är lika enkelt som att rensa hello token-cache med ADAL:</span><span class="sxs-lookup"><span data-stu-id="c9738-171">With ADAL, this action is as easy as clearing hello token cache:</span></span>

    ```C#
    private void SignOut()
    {
        // Clear session state from hello token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a><span data-ttu-id="c9738-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9738-172">What's next</span></span>
<span data-ttu-id="c9738-173">Nu har du en fungerande Windows Store-app som kan autentisera användare, på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och få grundläggande information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="c9738-173">You now have a working Windows Store app that can authenticate users, securely call web APIs using OAuth 2.0, and get basic information about hello user.</span></span>

<span data-ttu-id="c9738-174">Om du inte redan har fyllts i din klient med användare, är nu hello tid toodo så.</span><span class="sxs-lookup"><span data-stu-id="c9738-174">If you haven’t already populated your tenant with users, now is hello time toodo so.</span></span>
1. <span data-ttu-id="c9738-175">Kör appen DirectorySearcher och logga sedan in med någon av hello användare.</span><span class="sxs-lookup"><span data-stu-id="c9738-175">Run your DirectorySearcher app, and then sign in with one of hello users.</span></span>
2. <span data-ttu-id="c9738-176">Sök efter andra användare baserat på deras UPN.</span><span class="sxs-lookup"><span data-stu-id="c9738-176">Search for other users based on their UPN.</span></span>
3. <span data-ttu-id="c9738-177">Stäng hello app och köra den igen.</span><span class="sxs-lookup"><span data-stu-id="c9738-177">Close hello app, and rerun it.</span></span> <span data-ttu-id="c9738-178">Observera hur hello användarens session förblir intakt.</span><span class="sxs-lookup"><span data-stu-id="c9738-178">Notice how hello user’s session remains intact.</span></span>
4. <span data-ttu-id="c9738-179">Logga ut genom att högerklicka på toodisplay hello nedre fältet och sedan logga in igen som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="c9738-179">Sign out by right-clicking toodisplay hello bottom bar, and then sign back in as another user.</span></span>

<span data-ttu-id="c9738-180">ADAL gör det enkelt tooincorporate alla vanliga identitet funktionerna i hello app.</span><span class="sxs-lookup"><span data-stu-id="c9738-180">ADAL makes it easy tooincorporate all these common identity features into hello app.</span></span> <span data-ttu-id="c9738-181">Det tar hand om alla hello ändrad arbete för dig, t.ex hantering av cache OAuth protokollstöd presentera hello användare med en inloggning-Gränssnittet och uppdatera gått token.</span><span class="sxs-lookup"><span data-stu-id="c9738-181">It takes care of all hello dirty work for you, such as cache management, OAuth protocol support, presenting hello user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="c9738-182">Du behöver tooknow bara ett enda API-anrop `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="c9738-182">You need tooknow only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="c9738-183">Hämta hello referens [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (utan dina konfigurationsvärden).</span><span class="sxs-lookup"><span data-stu-id="c9738-183">For reference, download hello [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="c9738-184">Du kan nu gå vidare tooadditional identitet scenarier.</span><span class="sxs-lookup"><span data-stu-id="c9738-184">You can now move on tooadditional identity scenarios.</span></span> <span data-ttu-id="c9738-185">Till exempel försöka [skydda ett .NET-webb-API med Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="c9738-185">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

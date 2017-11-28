---
title: "aaaAzure AD .NET komma igång | Microsoft Docs"
description: "Hur skyddade toobuild ett program i .NET Windows-skrivbordet som kan integreras med Azure AD för inloggning och Azure AD-anrop API: er som använder sig av OAuth."
services: active-directory
documentationcenter: .net
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c09b358f24c7bfb371b34cf72ca48c0a45042f5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a><span data-ttu-id="e78ce-103">Integrera Azure AD i en Windows-skrivbord WPF-program</span><span class="sxs-lookup"><span data-stu-id="e78ce-103">Integrate Azure AD into a Windows Desktop WPF App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="e78ce-104">Om du utvecklar ett skrivbordsprogram gör Azure AD det lätt för tooauthenticate du dina användare med deras Active Directory-konton.</span><span class="sxs-lookup"><span data-stu-id="e78ce-104">If you're developing a desktop application, Azure AD makes it simple and straightforward for you tooauthenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="e78ce-105">Program-toosecurely kan också använda en webb-API som skyddas av Azure AD, som hello Office 365-API: er eller hello Azure API.</span><span class="sxs-lookup"><span data-stu-id="e78ce-105">It also enables your application toosecurely consume any web API protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="e78ce-106">Azure AD innehåller hello Active Directory Authentication Library eller ADAL för .NET interna klienter som behöver tooaccess skyddade resurser.</span><span class="sxs-lookup"><span data-stu-id="e78ce-106">For .NET native clients that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="e78ce-107">ADALS enda syfte i livslängd är toomake det enkelt för din app tooget åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="e78ce-107">ADAL's sole purpose in life is toomake it easy for your app tooget access tokens.</span></span>  <span data-ttu-id="e78ce-108">toodemonstrate hur lätt det är här vi ska skapa ett .NET uppgiftslista för WPF-program som:</span><span class="sxs-lookup"><span data-stu-id="e78ce-108">toodemonstrate just how easy it is, here we'll build a .NET WPF To-Do List application that:</span></span>

* <span data-ttu-id="e78ce-109">Hämtar åtkomst till token för att anropa hello Azure AD Graph API: et med hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="e78ce-109">Gets access tokens for calling hello Azure AD Graph API using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="e78ce-110">Söker en katalog för användare med ett visst alias.</span><span class="sxs-lookup"><span data-stu-id="e78ce-110">Searches a directory for users with a given alias.</span></span>
* <span data-ttu-id="e78ce-111">Användare loggar ut.</span><span class="sxs-lookup"><span data-stu-id="e78ce-111">Signs users out.</span></span>

<span data-ttu-id="e78ce-112">toobuild hello fullständig fungerande program, måste du:</span><span class="sxs-lookup"><span data-stu-id="e78ce-112">toobuild hello complete working application, you'll need to:</span></span>

1. <span data-ttu-id="e78ce-113">Registrera ditt program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e78ce-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="e78ce-114">Installera och konfigurera ADAL.</span><span class="sxs-lookup"><span data-stu-id="e78ce-114">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="e78ce-115">Använd ADAL tooget token från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e78ce-115">Use ADAL tooget tokens from Azure AD.</span></span>

<span data-ttu-id="e78ce-116">tooget har startats [hämta hello app stommen](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="e78ce-116">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="e78ce-117">Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="e78ce-117">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="e78ce-118">Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="e78ce-118">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-hello-directorysearcher-application"></a><span data-ttu-id="e78ce-119">1. Registrera hello DirectorySearcher program</span><span class="sxs-lookup"><span data-stu-id="e78ce-119">1. Register hello DirectorySearcher Application</span></span>
<span data-ttu-id="e78ce-120">tooenable tooget token för din app behöver du tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="e78ce-120">tooenable your app tooget tokens, you'll first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="e78ce-121">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e78ce-121">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e78ce-122">Hello översta, klicka på fältet för ditt konto och på hello **Directory** Välj plats tooregister programmet hello Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="e78ce-122">On hello top bar, click on your account and under hello **Directory** list, choose hello Active Directory tenant where you wish tooregister your application.</span></span>
3. <span data-ttu-id="e78ce-123">Klicka på **fler tjänster** i hello vänstra nav och välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e78ce-123">Click on **More Services** in hello left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="e78ce-124">Klicka på **App registreringar** och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e78ce-124">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="e78ce-125">Följ anvisningarna för hello och skapa en ny **internt klientprogram**.</span><span class="sxs-lookup"><span data-stu-id="e78ce-125">Follow hello prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="e78ce-126">Hej **namn** av hello programmet beskriver dina program tooend-användare</span><span class="sxs-lookup"><span data-stu-id="e78ce-126">hello **Name** of hello application will describe your application tooend-users</span></span>
  * <span data-ttu-id="e78ce-127">Hej **omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD ska använda tooreturn token svar.</span><span class="sxs-lookup"><span data-stu-id="e78ce-127">hello **Redirect Uri** is a scheme and string combination that Azure AD will use tooreturn token responses.</span></span>  <span data-ttu-id="e78ce-128">Ange ett värde specifika tooyour program, t.ex. `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="e78ce-128">Enter a value specific tooyour application, e.g. `http://DirectorySearcher`.</span></span>
6. <span data-ttu-id="e78ce-129">När du har slutfört registreringen tilldelas AAD appen ett unikt program-ID.</span><span class="sxs-lookup"><span data-stu-id="e78ce-129">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="e78ce-130">Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello appen på sidan.</span><span class="sxs-lookup"><span data-stu-id="e78ce-130">You'll need this value in hello next sections, so copy it from hello application page.</span></span>
7. <span data-ttu-id="e78ce-131">Från hello **inställningar** väljer **nödvändiga behörigheter** och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e78ce-131">From hello **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="e78ce-132">Välj hello **Microsoft Graph** som hello API och Lägg till hello **läsa katalogdata** behörighet under **delegerade behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="e78ce-132">Select hello **Microsoft Graph** as hello API and add hello **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="e78ce-133">Detta gör att dina program tooquery hello Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="e78ce-133">This will enable your application tooquery hello Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="e78ce-134">2. Installera och konfigurera ADAL</span><span class="sxs-lookup"><span data-stu-id="e78ce-134">2. Install & Configure ADAL</span></span>
<span data-ttu-id="e78ce-135">Nu när du har ett program i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="e78ce-135">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="e78ce-136">ADAL toobe kan toocommunicate med Azure AD, du måste för tooprovide med viss information om din appregistrering.</span><span class="sxs-lookup"><span data-stu-id="e78ce-136">In order for ADAL toobe able toocommunicate with Azure AD, you need tooprovide it with some information about your app registration.</span></span>

* <span data-ttu-id="e78ce-137">Börja genom att lägga till ADAL toohello DirectorySearcher projektet med hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="e78ce-137">Begin by adding ADAL toohello DirectorySearcher project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="e78ce-138">Öppna i hello DirectorySearcher project `app.config`.</span><span class="sxs-lookup"><span data-stu-id="e78ce-138">In hello DirectorySearcher project, open `app.config`.</span></span>  <span data-ttu-id="e78ce-139">Ersätt hello värdena för hello element i hello `<appSettings>` avsnittet tooreflect hello värden indata i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e78ce-139">Replace hello values of hello elements in hello `<appSettings>` section tooreflect hello values you input into hello Azure Portal.</span></span>  <span data-ttu-id="e78ce-140">Koden ska referera till dessa värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="e78ce-140">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="e78ce-141">Hej `ida:Tenant` är hello domän för din Azure AD-klient, t.ex. contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="e78ce-141">hello `ida:Tenant` is hello domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="e78ce-142">Hej `ida:ClientId` är hello clientId på ditt program som du kopierade från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e78ce-142">hello `ida:ClientId` is hello clientId of your application you copied from hello portal.</span></span>
  * <span data-ttu-id="e78ce-143">Hej `ida:RedirectUri` är hello omdirigerings-url som du har registrerat i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e78ce-143">hello `ida:RedirectUri` is hello redirect url you registered in hello portal.</span></span>

## <a name="3----use-adal-tooget-tokens-from-aad"></a><span data-ttu-id="e78ce-144">3.    Använd ADAL tooGet token från AAD</span><span class="sxs-lookup"><span data-stu-id="e78ce-144">3.    Use ADAL tooGet Tokens from AAD</span></span>
<span data-ttu-id="e78ce-145">hello grundläggande principen bakom ADAL är att när din app behöver en åtkomst-token kan bara anropas `authContext.AcquireTokenAsync(...)`, och ADAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="e78ce-145">hello basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireTokenAsync(...)`, and ADAL does hello rest.</span></span>  

* <span data-ttu-id="e78ce-146">I hello `DirectorySearcher` projektet öppnar `MainWindow.xaml.cs` och leta upp hello `MainWindow()` metod.</span><span class="sxs-lookup"><span data-stu-id="e78ce-146">In hello `DirectorySearcher` project, open `MainWindow.xaml.cs` and locate hello `MainWindow()` method.</span></span>  <span data-ttu-id="e78ce-147">hello första steget är tooinitialize appens `AuthenticationContext` -ADAL vars primära klassen.</span><span class="sxs-lookup"><span data-stu-id="e78ce-147">hello first step is tooinitialize your app's `AuthenticationContext` - ADAL's primary class.</span></span>  <span data-ttu-id="e78ce-148">Det är där du skickar ADAL hello koordinater måste toocommunicate med Azure AD och anger hur toocache token.</span><span class="sxs-lookup"><span data-stu-id="e78ce-148">This is where you pass ADAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* <span data-ttu-id="e78ce-149">Hitta nu hello `Search(...)` metod som kommer att anropas när hello användaren cliks hello ”Sök”-knappen i hello appens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="e78ce-149">Now locate hello `Search(...)` method, which will be invoked when hello user cliks hello "Search" button in hello app's UI.</span></span>  <span data-ttu-id="e78ce-150">Den här metoden gör en GET-begäran toohello Azure AD Graph API tooquery för användare vars UPN-namnet börjar med hello angivna sökord.</span><span class="sxs-lookup"><span data-stu-id="e78ce-150">This method makes a GET request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span>  <span data-ttu-id="e78ce-151">Men i ordning tooquery hello Graph API, behöver du tooinclude en access_token i hello `Authorization` rubriken för hello begäran - detta är där ADAL kommer in.</span><span class="sxs-lookup"><span data-stu-id="e78ce-151">But in order tooquery hello Graph API, you need tooinclude an access_token in hello `Authorization` header of hello request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate hello Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for hello tooDo item name");
        return;
    }

    // Get an Access Token for hello Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled hello sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* <span data-ttu-id="e78ce-152">När din app begär en token genom att anropa `AcquireTokenAsync(...)`, ADAL försöker tooreturn en token utan att uppge autentiseringsuppgifter hello användare.</span><span class="sxs-lookup"><span data-stu-id="e78ce-152">When your app requests a token by calling `AcquireTokenAsync(...)`, ADAL will attempt tooreturn a token without asking hello user for credentials.</span></span>  <span data-ttu-id="e78ce-153">Om ADAL avgör hello användaren måste toosign i tooget en token, visas en dialogruta med en inloggning, samla in hello användarens autentiseringsuppgifter och returnera en token vid autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="e78ce-153">If ADAL determines that hello user needs toosign in tooget a token, it will display a login dialog, collect hello user's credentials, and return a token upon successful authentication.</span></span>  <span data-ttu-id="e78ce-154">Om ADAL är tooreturn en token av någon anledning, genereras ett `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="e78ce-154">If ADAL is unable tooreturn a token for any reason, it will throw an `AdalException`.</span></span>
* <span data-ttu-id="e78ce-155">Observera att hello `AuthenticationResult` objektet innehåller en `UserInfo` objekt som kan vara används toocollect information som din app kan behöva.</span><span class="sxs-lookup"><span data-stu-id="e78ce-155">Notice that hello `AuthenticationResult` object contains a `UserInfo` object that can be used toocollect information your app may need.</span></span>  <span data-ttu-id="e78ce-156">I hello DirectorySearcher, `UserInfo` är används toocustomize hello appens användargränssnitt med hello användar-id.</span><span class="sxs-lookup"><span data-stu-id="e78ce-156">In hello DirectorySearcher, `UserInfo` is used toocustomize hello app's UI with hello user's id.</span></span>
* <span data-ttu-id="e78ce-157">När hello användaren klickar hello ”logga ut”, vi vill tooensure som hello nästa anrop för`AcquireTokenAsync(...)` ber hello användaren toosign i.</span><span class="sxs-lookup"><span data-stu-id="e78ce-157">When hello user clicks hello "Sign Out" button, we want tooensure that hello next call too`AcquireTokenAsync(...)` will ask hello user toosign in.</span></span>  <span data-ttu-id="e78ce-158">Det är lika enkelt som att rensa hello token-cache med ADAL:</span><span class="sxs-lookup"><span data-stu-id="e78ce-158">With ADAL, this is as easy as clearing hello token cache:</span></span>

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear hello token cache
    authContext.TokenCache.Clear();

    ...
}
```

* <span data-ttu-id="e78ce-159">Hello användaren inte klickar på hello ”logga ut” knappen ska toomaintain hello användarsession för hello nästa gång de kör hello DirectorySearcher.</span><span class="sxs-lookup"><span data-stu-id="e78ce-159">However, if hello user does not click hello "Sign Out" button, you will want toomaintain hello user's session for hello next time they run hello DirectorySearcher.</span></span>  <span data-ttu-id="e78ce-160">Du kan kontrollera ADAL'S token-cache för en befintlig token och uppdateras hello Användargränssnittet när hello app startar.</span><span class="sxs-lookup"><span data-stu-id="e78ce-160">When hello app launches, you can check ADAL's token cache for an existing token and update hello UI accordingly.</span></span>  <span data-ttu-id="e78ce-161">I hello `CheckForCachedToken()` metod, gör ett annat anrop för`AcquireTokenAsync(...)`, nu skicka i hello `PromptBehavior.Never` parameter.</span><span class="sxs-lookup"><span data-stu-id="e78ce-161">In hello `CheckForCachedToken()` method, make another call too`AcquireTokenAsync(...)`, this time passing in hello `PromptBehavior.Never` parameter.</span></span>  <span data-ttu-id="e78ce-162">`PromptBehavior.Never`talar om ADAL att hello användaren inte ska uppmanas att logga in och ADAL i stället utlösa ett undantag om det är tooreturn en token.</span><span class="sxs-lookup"><span data-stu-id="e78ce-162">`PromptBehavior.Never` will tell ADAL that hello user should not be prompted for sign in, and ADAL should instead throw an exception if it is unable tooreturn a token.</span></span>

```C#
public async void CheckForCachedToken() 
{
    // As hello application starts, try tooget an access token without prompting hello user.  If one exists, show hello user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed toomain page without singing hello user in.
        return;
    }

    // A valid token is in hello cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

<span data-ttu-id="e78ce-163">Grattis!</span><span class="sxs-lookup"><span data-stu-id="e78ce-163">Congratulations!</span></span> <span data-ttu-id="e78ce-164">Du nu ha en fungerande .NET WPF-program som har hello möjlighet tooauthenticate användare på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och hämta grundläggande information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="e78ce-164">You now have a working .NET WPF application that has hello ability tooauthenticate users, securely call Web APIs using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="e78ce-165">Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare.</span><span class="sxs-lookup"><span data-stu-id="e78ce-165">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span>  <span data-ttu-id="e78ce-166">Kör appen DirectorySearcher och logga in med något av dessa användare.</span><span class="sxs-lookup"><span data-stu-id="e78ce-166">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="e78ce-167">Sök efter andra användare baserat på deras UPN.</span><span class="sxs-lookup"><span data-stu-id="e78ce-167">Search for other users based on their UPN.</span></span>  <span data-ttu-id="e78ce-168">Stäng hello app och kör den igen.</span><span class="sxs-lookup"><span data-stu-id="e78ce-168">Close hello app, and re-run it.</span></span>  <span data-ttu-id="e78ce-169">Observera hur hello användarens session förblir intakt.</span><span class="sxs-lookup"><span data-stu-id="e78ce-169">Notice how hello user's session remains intact.</span></span>  <span data-ttu-id="e78ce-170">Logga ut och logga in igen som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="e78ce-170">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="e78ce-171">ADAL gör det enkelt tooincorporate alla dessa vanliga identity-funktioner i programmet.</span><span class="sxs-lookup"><span data-stu-id="e78ce-171">ADAL makes it easy tooincorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="e78ce-172">Det hand tar om alla hello ändrad arbete du - hantering av cache, OAuth protokollstöd presentera hello användare med en inloggning UI, uppdatera token har upphört att gälla och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="e78ce-172">It takes care of all hello dirty work for you - cache management, OAuth protocol support, presenting hello user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="e78ce-173">Allt du verkligen behöver tooknow är ett enda API-anrop `authContext.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="e78ce-173">All you really need tooknow is a single API call, `authContext.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="e78ce-174">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) tillhandahålls [här](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="e78ce-174">For reference, hello completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="e78ce-175">Du kan nu gå vidare tooadditional scenarier.</span><span class="sxs-lookup"><span data-stu-id="e78ce-175">You can now move on tooadditional scenarios.</span></span>  <span data-ttu-id="e78ce-176">Du kanske vill tootry:</span><span class="sxs-lookup"><span data-stu-id="e78ce-176">You may want tootry:</span></span>

[<span data-ttu-id="e78ce-177">Skydda ett .NET-webb-API med Azure AD >></span><span class="sxs-lookup"><span data-stu-id="e78ce-177">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


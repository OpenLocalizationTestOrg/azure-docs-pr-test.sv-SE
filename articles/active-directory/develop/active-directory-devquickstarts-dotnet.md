---
title: "Azure AD .NET komma igång | Microsoft Docs"
description: "Hur du skapar ett program i .NET Windows-skrivbordet som kan integreras med Azure AD för inloggning och Azure AD-anrop skyddade API: er som använder sig av OAuth."
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
ms.openlocfilehash: 7a252e0e5243c7b7489373845531cb913ca1f6aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a><span data-ttu-id="eb475-103">Integrera Azure AD i en Windows-skrivbord WPF-program</span><span class="sxs-lookup"><span data-stu-id="eb475-103">Integrate Azure AD into a Windows Desktop WPF App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="eb475-104">Om du utvecklar ett skrivbordsprogram Azure AD att gör det lätt att autentisera användarna med deras Active Directory-konton.</span><span class="sxs-lookup"><span data-stu-id="eb475-104">If you're developing a desktop application, Azure AD makes it simple and straightforward for you to authenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="eb475-105">Det gör också ditt program att använda alla webb-API som skyddas av Azure AD, till exempel API: er för Office 365 eller Azure API på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="eb475-105">It also enables your application to securely consume any web API protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="eb475-106">Azure AD innehåller Active Directory Authentication Library eller ADAL för .NET interna klienter som behöver åtkomst till skyddade resurser.</span><span class="sxs-lookup"><span data-stu-id="eb475-106">For .NET native clients that need to access protected resources, Azure AD provides the Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="eb475-107">ADALS uteslutande i livslängd är att göra det lättare för din app för att få åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="eb475-107">ADAL's sole purpose in life is to make it easy for your app to get access tokens.</span></span>  <span data-ttu-id="eb475-108">För att demonstrera hur lätt det är ska här vi skapa ett .NET uppgiftslista för WPF-program som:</span><span class="sxs-lookup"><span data-stu-id="eb475-108">To demonstrate just how easy it is, here we'll build a .NET WPF To-Do List application that:</span></span>

* <span data-ttu-id="eb475-109">Hämtar åtkomst till token för att anropa en Azure AD Graph API med hjälp av den [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="eb475-109">Gets access tokens for calling the Azure AD Graph API using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="eb475-110">Söker en katalog för användare med ett visst alias.</span><span class="sxs-lookup"><span data-stu-id="eb475-110">Searches a directory for users with a given alias.</span></span>
* <span data-ttu-id="eb475-111">Användare loggar ut.</span><span class="sxs-lookup"><span data-stu-id="eb475-111">Signs users out.</span></span>

<span data-ttu-id="eb475-112">För att skapa fullständiga fungerande program måste du:</span><span class="sxs-lookup"><span data-stu-id="eb475-112">To build the complete working application, you'll need to:</span></span>

1. <span data-ttu-id="eb475-113">Registrera ditt program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eb475-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="eb475-114">Installera och konfigurera ADAL.</span><span class="sxs-lookup"><span data-stu-id="eb475-114">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="eb475-115">Använd ADAL för att hämta token från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eb475-115">Use ADAL to get tokens from Azure AD.</span></span>

<span data-ttu-id="eb475-116">Du kommer igång [ladda ned stommen app](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) eller [hämta det slutförda exemplet](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="eb475-116">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="eb475-117">Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="eb475-117">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="eb475-118">Om du inte redan har en klient [Lär dig hur du skaffa en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="eb475-118">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-the-directorysearcher-application"></a><span data-ttu-id="eb475-119">1. Registrera DirectorySearcher-program</span><span class="sxs-lookup"><span data-stu-id="eb475-119">1. Register the DirectorySearcher Application</span></span>
<span data-ttu-id="eb475-120">Om du vill aktivera din app att hämta token måste du måste först registrera det i Azure AD-klienten och bevilja behörighet att komma åt Azure AD Graph-API:</span><span class="sxs-lookup"><span data-stu-id="eb475-120">To enable your app to get tokens, you'll first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API:</span></span>

1. <span data-ttu-id="eb475-121">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="eb475-121">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="eb475-122">Klicka på den översta raden på ditt konto och under den **Directory** Välj Active Directory-klient som du vill registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="eb475-122">On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.</span></span>
3. <span data-ttu-id="eb475-123">Klicka på **fler tjänster** i den vänstra nav och välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="eb475-123">Click on **More Services** in the left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="eb475-124">Klicka på **App registreringar** och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="eb475-124">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="eb475-125">Följ anvisningarna och skapa en ny **internt klientprogram**.</span><span class="sxs-lookup"><span data-stu-id="eb475-125">Follow the prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="eb475-126">Den **namn** av programmet beskriva programmet till slutanvändare</span><span class="sxs-lookup"><span data-stu-id="eb475-126">The **Name** of the application will describe your application to end-users</span></span>
  * <span data-ttu-id="eb475-127">Den **omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD för att returnera token svar.</span><span class="sxs-lookup"><span data-stu-id="eb475-127">The **Redirect Uri** is a scheme and string combination that Azure AD will use to return token responses.</span></span>  <span data-ttu-id="eb475-128">Ange ett värde som är specifik för ditt program, t.ex. `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="eb475-128">Enter a value specific to your application, e.g. `http://DirectorySearcher`.</span></span>
6. <span data-ttu-id="eb475-129">När du har slutfört registreringen tilldelas AAD appen ett unikt program-ID.</span><span class="sxs-lookup"><span data-stu-id="eb475-129">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="eb475-130">Du behöver det här värdet i nästa avsnitt så kopiera den från appen på sidan.</span><span class="sxs-lookup"><span data-stu-id="eb475-130">You'll need this value in the next sections, so copy it from the application page.</span></span>
7. <span data-ttu-id="eb475-131">Från den **inställningar** väljer **nödvändiga behörigheter** och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="eb475-131">From the **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="eb475-132">Välj den **Microsoft Graph** som API och Lägg till den **läsa katalogdata** behörighet under **delegerade behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="eb475-132">Select the **Microsoft Graph** as the API and add the **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="eb475-133">Detta gör att programmet att fråga Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="eb475-133">This will enable your application to query the Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="eb475-134">2. Installera och konfigurera ADAL</span><span class="sxs-lookup"><span data-stu-id="eb475-134">2. Install & Configure ADAL</span></span>
<span data-ttu-id="eb475-135">Nu när du har ett program i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="eb475-135">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="eb475-136">Du måste tillhandahålla information om din appregistrering för ADAL för att kunna kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eb475-136">In order for ADAL to be able to communicate with Azure AD, you need to provide it with some information about your app registration.</span></span>

* <span data-ttu-id="eb475-137">Börja genom att lägga till ADAL DirectorySearcher projektet med Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="eb475-137">Begin by adding ADAL to the DirectorySearcher project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="eb475-138">Öppna i projektet DirectorySearcher `app.config`.</span><span class="sxs-lookup"><span data-stu-id="eb475-138">In the DirectorySearcher project, open `app.config`.</span></span>  <span data-ttu-id="eb475-139">Ersätt värdena för elementen i den `<appSettings>` avsnittet för att återspegla de värden du matar in i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="eb475-139">Replace the values of the elements in the `<appSettings>` section to reflect the values you input into the Azure Portal.</span></span>  <span data-ttu-id="eb475-140">Koden ska referera till dessa värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="eb475-140">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="eb475-141">Den `ida:Tenant` är domänen för din Azure AD-klient, t.ex. contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="eb475-141">The `ida:Tenant` is the domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="eb475-142">Den `ida:ClientId` är clientId för ditt program som du kopierade från portalen.</span><span class="sxs-lookup"><span data-stu-id="eb475-142">The `ida:ClientId` is the clientId of your application you copied from the portal.</span></span>
  * <span data-ttu-id="eb475-143">Den `ida:RedirectUri` är omdirigeringen url som du har registrerat i portalen.</span><span class="sxs-lookup"><span data-stu-id="eb475-143">The `ida:RedirectUri` is the redirect url you registered in the portal.</span></span>

## <a name="3----use-adal-to-get-tokens-from-aad"></a><span data-ttu-id="eb475-144">3.    Använd ADAL för att hämta token från AAD</span><span class="sxs-lookup"><span data-stu-id="eb475-144">3.    Use ADAL to Get Tokens from AAD</span></span>
<span data-ttu-id="eb475-145">Den grundläggande principen bakom ADAL är att när din app behöver en åtkomst-token kan bara anropas `authContext.AcquireTokenAsync(...)`, och ADAL gör resten.</span><span class="sxs-lookup"><span data-stu-id="eb475-145">The basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireTokenAsync(...)`, and ADAL does the rest.</span></span>  

* <span data-ttu-id="eb475-146">I den `DirectorySearcher` projektet öppnar `MainWindow.xaml.cs` och leta upp den `MainWindow()` metoden.</span><span class="sxs-lookup"><span data-stu-id="eb475-146">In the `DirectorySearcher` project, open `MainWindow.xaml.cs` and locate the `MainWindow()` method.</span></span>  <span data-ttu-id="eb475-147">Det första steget är att initiera appens `AuthenticationContext` -ADAL vars primära klassen.</span><span class="sxs-lookup"><span data-stu-id="eb475-147">The first step is to initialize your app's `AuthenticationContext` - ADAL's primary class.</span></span>  <span data-ttu-id="eb475-148">Det är där du skickar ADAL koordinaterna behöver kommunicera med Azure AD och hur det kan cachelagra token.</span><span class="sxs-lookup"><span data-stu-id="eb475-148">This is where you pass ADAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* <span data-ttu-id="eb475-149">Leta reda på `Search(...)` metod som ska anropas när användaren cliks ”Sök” knappen i appens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="eb475-149">Now locate the `Search(...)` method, which will be invoked when the user cliks the "Search" button in the app's UI.</span></span>  <span data-ttu-id="eb475-150">Den här metoden gör en GET-begäran till Azure AD Graph API frågan för användare vars UPN-namnet börjar med den angivna söktermen.</span><span class="sxs-lookup"><span data-stu-id="eb475-150">This method makes a GET request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span>  <span data-ttu-id="eb475-151">Men om du vill fråga Graph API, måste du inkludera ett access_token i den `Authorization` huvud för begäran - detta är där ADAL kommer in.</span><span class="sxs-lookup"><span data-stu-id="eb475-151">But in order to query the Graph API, you need to include an access_token in the `Authorization` header of the request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
* <span data-ttu-id="eb475-152">När din app begär en token genom att anropa `AcquireTokenAsync(...)`, ADAL görs ett försök att returnera en token utan att fråga användaren om autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="eb475-152">When your app requests a token by calling `AcquireTokenAsync(...)`, ADAL will attempt to return a token without asking the user for credentials.</span></span>  <span data-ttu-id="eb475-153">Om ADAL avgör att användaren måste logga in att hämta en token, visas en dialogruta med en inloggning, samla in användarens autentiseringsuppgifter och returnera en token vid autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="eb475-153">If ADAL determines that the user needs to sign in to get a token, it will display a login dialog, collect the user's credentials, and return a token upon successful authentication.</span></span>  <span data-ttu-id="eb475-154">Om ADAL inte kan returnera en token av någon anledning, genereras ett `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="eb475-154">If ADAL is unable to return a token for any reason, it will throw an `AdalException`.</span></span>
* <span data-ttu-id="eb475-155">Observera att den `AuthenticationResult` objektet innehåller en `UserInfo` objekt som kan användas för att samla in information som din app kan behöva.</span><span class="sxs-lookup"><span data-stu-id="eb475-155">Notice that the `AuthenticationResult` object contains a `UserInfo` object that can be used to collect information your app may need.</span></span>  <span data-ttu-id="eb475-156">I DirectorySearcher `UserInfo` används för att anpassa appens användargränssnitt med det användar-id.</span><span class="sxs-lookup"><span data-stu-id="eb475-156">In the DirectorySearcher, `UserInfo` is used to customize the app's UI with the user's id.</span></span>
* <span data-ttu-id="eb475-157">När användaren klickar på knappen ”Logga ut” vi vill se till att nästa anrop till `AcquireTokenAsync(...)` uppmanas användaren att logga in.</span><span class="sxs-lookup"><span data-stu-id="eb475-157">When the user clicks the "Sign Out" button, we want to ensure that the next call to `AcquireTokenAsync(...)` will ask the user to sign in.</span></span>  <span data-ttu-id="eb475-158">Det är lika enkelt som att rensa cacheminnet token med ADAL:</span><span class="sxs-lookup"><span data-stu-id="eb475-158">With ADAL, this is as easy as clearing the token cache:</span></span>

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* <span data-ttu-id="eb475-159">Om användaren inte klicka på knappen ”Logga ut”, kommer du vill behålla användarens session för nästa gång de kör DirectorySearcher.</span><span class="sxs-lookup"><span data-stu-id="eb475-159">However, if the user does not click the "Sign Out" button, you will want to maintain the user's session for the next time they run the DirectorySearcher.</span></span>  <span data-ttu-id="eb475-160">Du kan kontrollera ADAL'S token-cache för en befintlig token och uppdateras Användargränssnittet när appen startar.</span><span class="sxs-lookup"><span data-stu-id="eb475-160">When the app launches, you can check ADAL's token cache for an existing token and update the UI accordingly.</span></span>  <span data-ttu-id="eb475-161">I den `CheckForCachedToken()` metod, gör ett annat anrop till `AcquireTokenAsync(...)`, nu skicka i den `PromptBehavior.Never` parameter.</span><span class="sxs-lookup"><span data-stu-id="eb475-161">In the `CheckForCachedToken()` method, make another call to `AcquireTokenAsync(...)`, this time passing in the `PromptBehavior.Never` parameter.</span></span>  <span data-ttu-id="eb475-162">`PromptBehavior.Never`ADAL anger att användaren inte ska uppmanas att logga in och ADAL i stället utlösa ett undantag om det inte går att returnera en token.</span><span class="sxs-lookup"><span data-stu-id="eb475-162">`PromptBehavior.Never` will tell ADAL that the user should not be prompted for sign in, and ADAL should instead throw an exception if it is unable to return a token.</span></span>

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
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

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

<span data-ttu-id="eb475-163">Grattis!</span><span class="sxs-lookup"><span data-stu-id="eb475-163">Congratulations!</span></span> <span data-ttu-id="eb475-164">Nu har en aktiv .NET WPF-program som har möjlighet att autentisera användare, på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och få grundläggande information om användaren.</span><span class="sxs-lookup"><span data-stu-id="eb475-164">You now have a working .NET WPF application that has the ability to authenticate users, securely call Web APIs using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="eb475-165">Om du inte redan gjort nu är det dags att fylla i din klient med vissa användare.</span><span class="sxs-lookup"><span data-stu-id="eb475-165">If you haven't already, now is the time to populate your tenant with some users.</span></span>  <span data-ttu-id="eb475-166">Kör appen DirectorySearcher och logga in med något av dessa användare.</span><span class="sxs-lookup"><span data-stu-id="eb475-166">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="eb475-167">Sök efter andra användare baserat på deras UPN.</span><span class="sxs-lookup"><span data-stu-id="eb475-167">Search for other users based on their UPN.</span></span>  <span data-ttu-id="eb475-168">Stäng appen och kör den igen.</span><span class="sxs-lookup"><span data-stu-id="eb475-168">Close the app, and re-run it.</span></span>  <span data-ttu-id="eb475-169">Observera hur användarens session förblir intakt.</span><span class="sxs-lookup"><span data-stu-id="eb475-169">Notice how the user's session remains intact.</span></span>  <span data-ttu-id="eb475-170">Logga ut och logga in igen som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="eb475-170">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="eb475-171">ADAL gör det enkelt att införliva alla dessa vanliga identity-funktioner i programmet.</span><span class="sxs-lookup"><span data-stu-id="eb475-171">ADAL makes it easy to incorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="eb475-172">Det hand tar om ändrad arbetet åt dig - hantering av cache, OAuth protokollstöd presentera användaren med en inloggning UI, uppdatera token har upphört att gälla och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="eb475-172">It takes care of all the dirty work for you - cache management, OAuth protocol support, presenting the user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="eb475-173">Allt du behöver veta är ett enda API-anrop `authContext.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="eb475-173">All you really need to know is a single API call, `authContext.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="eb475-174">Referens tillhandahålls det slutförda exemplet (utan dina konfigurationsvärden) [här](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="eb475-174">For reference, the completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).</span></span>  <span data-ttu-id="eb475-175">Du kan nu gå vidare till fler scenarier.</span><span class="sxs-lookup"><span data-stu-id="eb475-175">You can now move on to additional scenarios.</span></span>  <span data-ttu-id="eb475-176">Du kanske vill prova:</span><span class="sxs-lookup"><span data-stu-id="eb475-176">You may want to try:</span></span>

[<span data-ttu-id="eb475-177">Skydda ett .NET-webb-API med Azure AD >></span><span class="sxs-lookup"><span data-stu-id="eb475-177">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


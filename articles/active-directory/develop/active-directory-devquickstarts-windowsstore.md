---
title: "Azure AD Windows Store komma igång | Microsoft Docs"
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
ms.openlocfilehash: 6b5189dc06d7f8b0ed4426944948b904feba847e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="integrate-azure-ad-with-windows-store-apps"></a><span data-ttu-id="e2ace-103">Integrera Azure AD med Windows Store-appar</span><span class="sxs-lookup"><span data-stu-id="e2ace-103">Integrate Azure AD with Windows Store apps</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="e2ace-104">Windows Store 8.1 och tidigare version projekt stöds inte i Visual Studio-2017.</span><span class="sxs-lookup"><span data-stu-id="e2ace-104">Windows Store 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="e2ace-105">Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).</span><span class="sxs-lookup"><span data-stu-id="e2ace-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="e2ace-106">Om du utvecklar appar för Windows Store, Azure Active Directory (Azure AD) att gör det lätt att autentisera användarna med deras Active Directory-konton.</span><span class="sxs-lookup"><span data-stu-id="e2ace-106">If you're developing apps for the Windows Store, Azure Active Directory (Azure AD) makes it simple and straightforward to authenticate your users with their Active Directory accounts.</span></span> <span data-ttu-id="e2ace-107">En app kan på ett säkert sätt använda alla webb-API som skyddas av Azure AD, till exempel API: er för Office 365 eller Azure API genom att integrera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e2ace-107">By integrating with Azure AD, an app can securely consume any web API that's protected by Azure AD, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="e2ace-108">För Windows Store-program som behöver åtkomst till skyddade resurser, innehåller Azure AD Active Directory Authentication Library (ADAL).</span><span class="sxs-lookup"><span data-stu-id="e2ace-108">For Windows Store desktop apps that need to access protected resources, Azure AD provides the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="e2ace-109">Det enda syftet med ADAL är att göra det enkelt för programmet för att få åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="e2ace-109">The sole purpose of ADAL is to make it easy for the app to get access tokens.</span></span> <span data-ttu-id="e2ace-110">För att demonstrera hur lätt det är den här artikeln visar hur du skapar en DirectorySearcher Windows Store-app som:</span><span class="sxs-lookup"><span data-stu-id="e2ace-110">To demonstrate how easy it is, this article shows how to build a DirectorySearcher Windows Store app that:</span></span>

* <span data-ttu-id="e2ace-111">Hämtar åtkomst till token för att anropa Azure AD Graph-API med hjälp av den [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="e2ace-111">Gets access tokens for calling the Azure AD Graph API by using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="e2ace-112">Söker en katalog för användare med en viss användarens huvudnamn (UPN).</span><span class="sxs-lookup"><span data-stu-id="e2ace-112">Searches a directory for users with a given user principal name (UPN).</span></span>
* <span data-ttu-id="e2ace-113">Användare loggar ut.</span><span class="sxs-lookup"><span data-stu-id="e2ace-113">Signs users out.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="e2ace-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="e2ace-114">Before you get started</span></span>
* <span data-ttu-id="e2ace-115">Hämta den [stommen av ett projekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), eller hämta den [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="e2ace-115">Download the [skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), or download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).</span></span> <span data-ttu-id="e2ace-116">Varje version är en lösning i Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="e2ace-116">Each download is a Visual Studio 2015 solution.</span></span>
* <span data-ttu-id="e2ace-117">Du måste också en Azure AD-klient att skapa användare och registrera appen.</span><span class="sxs-lookup"><span data-stu-id="e2ace-117">You also need an Azure AD tenant in which to create users and register the app.</span></span> <span data-ttu-id="e2ace-118">Om du inte redan har en klient [Lär dig hur du skaffa en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="e2ace-118">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="e2ace-119">Följ procedurerna i följande tre avsnitt när du är klar.</span><span class="sxs-lookup"><span data-stu-id="e2ace-119">When you are ready, follow the procedures in the next three sections.</span></span>

## <a name="step-1-register-the-directorysearcher-app"></a><span data-ttu-id="e2ace-120">Steg 1: Registrera DirectorySearcher-app</span><span class="sxs-lookup"><span data-stu-id="e2ace-120">Step 1: Register the DirectorySearcher app</span></span>
<span data-ttu-id="e2ace-121">Om du vill aktivera app att hämta token, måste du först registrera det i Azure AD-klienten och bevilja behörighet att komma åt Azure AD Graph API.</span><span class="sxs-lookup"><span data-stu-id="e2ace-121">To enable the app to get tokens, you first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API.</span></span> <span data-ttu-id="e2ace-122">Så här gör du:</span><span class="sxs-lookup"><span data-stu-id="e2ace-122">Here's how:</span></span>

1. <span data-ttu-id="e2ace-123">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e2ace-123">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e2ace-124">Klicka på ditt konto på den översta raden.</span><span class="sxs-lookup"><span data-stu-id="e2ace-124">On the top bar, click your account.</span></span> <span data-ttu-id="e2ace-125">Sedan, under den **Directory** väljer du Active Directory-klient som du vill registrera appen.</span><span class="sxs-lookup"><span data-stu-id="e2ace-125">Then, under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="e2ace-126">Klicka på **fler tjänster** i det vänstra fönstret och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-126">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="e2ace-127">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="e2ace-128">Följ anvisningarna för att skapa en **internt klientprogram**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-128">Follow the prompts to create a **Native Client Application**.</span></span>
  * <span data-ttu-id="e2ace-129">**Namnet** beskriver app till användare.</span><span class="sxs-lookup"><span data-stu-id="e2ace-129">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="e2ace-130">**Omdirigerings-URI** är en kombination av schemat och strängen som Azure AD som används för att returnera token svar.</span><span class="sxs-lookup"><span data-stu-id="e2ace-130">**Redirect URI** is a scheme and string combination that Azure AD uses to return token responses.</span></span> <span data-ttu-id="e2ace-131">Ange ett platshållarvärde för nu (till exempel **http://DirectorySearcher**).</span><span class="sxs-lookup"><span data-stu-id="e2ace-131">Enter a placeholder value for now (for example, **http://DirectorySearcher**).</span></span> <span data-ttu-id="e2ace-132">Du måste ersätta värdet senare.</span><span class="sxs-lookup"><span data-stu-id="e2ace-132">You'll replace the value later.</span></span>
6. <span data-ttu-id="e2ace-133">När du har slutfört registreringen, tilldelar Azure AD appen ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="e2ace-133">After you’ve completed the registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="e2ace-134">Kopiera värdet på den **programmet** fliken, eftersom du behöver senare.</span><span class="sxs-lookup"><span data-stu-id="e2ace-134">Copy the value on the **Application** tab, because you'll need it later.</span></span>
7. <span data-ttu-id="e2ace-135">På den **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e2ace-135">On the **Settings** page, select **Required Permissions**, and then select **Add**.</span></span>
8. <span data-ttu-id="e2ace-136">För den **Azure Active Directory** app markerar **Microsoft Graph** som API.</span><span class="sxs-lookup"><span data-stu-id="e2ace-136">For the **Azure Active Directory** app, select **Microsoft Graph** as the API.</span></span>
9. <span data-ttu-id="e2ace-137">Under **delegerade behörigheter**, lägga till den **åtkomst till katalogen som den inloggade användaren** behörighet.</span><span class="sxs-lookup"><span data-stu-id="e2ace-137">Under **Delegated Permissions**, add the **Access the directory as the signed-in user** permission.</span></span> <span data-ttu-id="e2ace-138">På så sätt kan programmet för att fråga Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="e2ace-138">Doing so enables the app to query the Graph API for users.</span></span>

## <a name="step-2-install-and-configure-adal"></a><span data-ttu-id="e2ace-139">Steg 2: Installera och konfigurera ADAL</span><span class="sxs-lookup"><span data-stu-id="e2ace-139">Step 2: Install and configure ADAL</span></span>
<span data-ttu-id="e2ace-140">Nu när du har en app i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="e2ace-140">Now that you have an app in Azure AD, you can install ADAL and write your identity-related code.</span></span> <span data-ttu-id="e2ace-141">Om du vill aktivera ADAL att kommunicera med Azure AD, ger det viss information om appregistrering.</span><span class="sxs-lookup"><span data-stu-id="e2ace-141">To enable ADAL to communicate with Azure AD, give it some information about the app registration.</span></span>

1. <span data-ttu-id="e2ace-142">Lägg till ADAL DirectorySearcher projektet med hjälp av Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="e2ace-142">Add ADAL to the DirectorySearcher project by using the Package Manager Console.</span></span>

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. <span data-ttu-id="e2ace-143">Öppna MainPage.xaml.cs i DirectorySearcher-projektet.</span><span class="sxs-lookup"><span data-stu-id="e2ace-143">In the DirectorySearcher project, open MainPage.xaml.cs.</span></span>
3. <span data-ttu-id="e2ace-144">Ersätter värdena i den **konfigurationsvärden** region med värden som du angav i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e2ace-144">Replace the values in the **Config Values** region with the values that you entered in the Azure portal.</span></span> <span data-ttu-id="e2ace-145">Din kod refererar till dessa värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="e2ace-145">Your code refers to these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="e2ace-146">Den *klient* är domänen för din Azure AD-klient (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="e2ace-146">The *tenant* is the domain of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="e2ace-147">Den *clientId* är klient-ID för appen, som du kopierade från portalen.</span><span class="sxs-lookup"><span data-stu-id="e2ace-147">The *clientId* is the client ID of the app, which you copied from the portal.</span></span>
4. <span data-ttu-id="e2ace-148">Nu måste du identifiera motringning URI för Windows Store-app.</span><span class="sxs-lookup"><span data-stu-id="e2ace-148">You now need to discover the callback URI for your Windows Store app.</span></span> <span data-ttu-id="e2ace-149">Ange en brytpunkt på den här raden i den `MainPage` metoden:</span><span class="sxs-lookup"><span data-stu-id="e2ace-149">Set a breakpoint on this line in the `MainPage` method:</span></span>
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. <span data-ttu-id="e2ace-150">Skapa lösningen, se till att alla paketet refererar till har återställts.</span><span class="sxs-lookup"><span data-stu-id="e2ace-150">Build the solution, making sure that all package references are restored.</span></span> <span data-ttu-id="e2ace-151">Om några paket saknas, öppna NuGet Package Manager och återställa dem.</span><span class="sxs-lookup"><span data-stu-id="e2ace-151">If any packages are missing, open the NuGet Package Manager and restore them.</span></span>
6. <span data-ttu-id="e2ace-152">Kör appen och kopiera värdet för `redirectUri` när brytpunkten träffar.</span><span class="sxs-lookup"><span data-stu-id="e2ace-152">Run the app, and copy the value of `redirectUri` when the breakpoint is hit.</span></span> <span data-ttu-id="e2ace-153">Värdet ska se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="e2ace-153">The value should look something like the following:</span></span>

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. <span data-ttu-id="e2ace-154">Tillbaka på den **inställningar** fliken app i Azure-portalen, lägga till en **RedirectUri** med föregående värde.</span><span class="sxs-lookup"><span data-stu-id="e2ace-154">Back on the **Settings** tab of the app in the Azure portal, add a **RedirectUri** with the preceding value.</span></span>  

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a><span data-ttu-id="e2ace-155">Steg 3: Använda ADAL att hämta token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2ace-155">Step 3: Use ADAL to get tokens from Azure AD</span></span>
<span data-ttu-id="e2ace-156">Den grundläggande principen bakom ADAL är att när appen behöver en åtkomst-token kan bara anropas `authContext.AcquireToken(…)`, och ADAL gör resten.</span><span class="sxs-lookup"><span data-stu-id="e2ace-156">The basic principle behind ADAL is that whenever the app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does the rest.</span></span>  

1. <span data-ttu-id="e2ace-157">Initiera appens `AuthenticationContext`, vilket är den primära klassen av ADAL.</span><span class="sxs-lookup"><span data-stu-id="e2ace-157">Initialize the app’s `AuthenticationContext`, which is the primary class of ADAL.</span></span> <span data-ttu-id="e2ace-158">Den här åtgärden klarar ADAL koordinaterna behöver kommunicera med Azure AD och hur det kan cachelagra token.</span><span class="sxs-lookup"><span data-stu-id="e2ace-158">This action passes ADAL the coordinates it needs to communicate with Azure AD and tell it how to cache tokens.</span></span>

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. <span data-ttu-id="e2ace-159">Leta upp den `Search(...)` metod som anropas när användaren klickar på den **Sök** knapp i appens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="e2ace-159">Locate the `Search(...)` method, which is invoked when users click the **Search** button on the app's UI.</span></span> <span data-ttu-id="e2ace-160">Den här metoden gör en get-begäran till Azure AD Graph API frågan för användare vars UPN-namnet börjar med den angivna söktermen.</span><span class="sxs-lookup"><span data-stu-id="e2ace-160">This method makes a get request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span> <span data-ttu-id="e2ace-161">Inkludera en åtkomst-token i en begäran om du vill fråga Graph API **auktorisering** huvud.</span><span class="sxs-lookup"><span data-stu-id="e2ace-161">To query the Graph API, include an access token in the request's **Authorization** header.</span></span> <span data-ttu-id="e2ace-162">Det är där ADAL kommer in.</span><span class="sxs-lookup"><span data-stu-id="e2ace-162">This is where ADAL comes in.</span></span>

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
                ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    <span data-ttu-id="e2ace-163">När appen begär en token genom att anropa `AcquireTokenAsync(...)`, ADAL försöker denna att returnera en token utan att fråga användaren om autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e2ace-163">When the app requests a token by calling `AcquireTokenAsync(...)`, ADAL attempts to return a token without asking the user for credentials.</span></span> <span data-ttu-id="e2ace-164">Om ADAL avgör att användaren måste logga in att hämta en token, den visar inloggning dialogrutan samlar in användarens autentiseringsuppgifter och returnerar en token efter att autentiseringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="e2ace-164">If ADAL determines that the user needs to sign in to get a token, it displays a sign-in dialog box, collects the user's credentials, and returns a token after authentication succeeds.</span></span> <span data-ttu-id="e2ace-165">Om ADAL inte kan returnera en token av någon anledning den *AuthenticationResult* status är ett fel.</span><span class="sxs-lookup"><span data-stu-id="e2ace-165">If ADAL is unable to return a token for any reason, the *AuthenticationResult* status is an error.</span></span>
3. <span data-ttu-id="e2ace-166">Nu är det dags att använda den åtkomst-token som du precis har köpt.</span><span class="sxs-lookup"><span data-stu-id="e2ace-166">Now it's time to use the access token you just acquired.</span></span> <span data-ttu-id="e2ace-167">Även i den `Search(...)` metod, bifoga token till get-begäran för Graph API i den **auktorisering** huvud:</span><span class="sxs-lookup"><span data-stu-id="e2ace-167">Also in the `Search(...)` method, attach the token to the Graph API get request in the **Authorization** header:</span></span>

    ```C#
    // Add the access token to the Authorization header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. <span data-ttu-id="e2ace-168">Du kan använda den `AuthenticationResult` objekt att visa information om användaren i appen, till exempel det användar-ID:</span><span class="sxs-lookup"><span data-stu-id="e2ace-168">You can use the `AuthenticationResult` object to display information about the user in the app, such as the user's ID:</span></span>

    ```C#
    // Update the page UI to represent the signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. <span data-ttu-id="e2ace-169">Du kan också använda ADAL för att logga in användare utanför appen.</span><span class="sxs-lookup"><span data-stu-id="e2ace-169">You can also use ADAL to sign users out of the app.</span></span> <span data-ttu-id="e2ace-170">När användaren klickar på **logga ut** knappen, se till att nästa anrop till `AcquireTokenAsync(...)` visas en vy för inloggning.</span><span class="sxs-lookup"><span data-stu-id="e2ace-170">When the user clicks the **Sign Out** button, ensure that the next call to `AcquireTokenAsync(...)` shows a sign-in view.</span></span> <span data-ttu-id="e2ace-171">Den här åtgärden är lika enkelt som att rensa cacheminnet token med ADAL:</span><span class="sxs-lookup"><span data-stu-id="e2ace-171">With ADAL, this action is as easy as clearing the token cache:</span></span>

    ```C#
    private void SignOut()
    {
        // Clear session state from the token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a><span data-ttu-id="e2ace-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e2ace-172">What's next</span></span>
<span data-ttu-id="e2ace-173">Nu har du en fungerande Windows Store-app som kan autentisera användare, på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och få grundläggande information om användaren.</span><span class="sxs-lookup"><span data-stu-id="e2ace-173">You now have a working Windows Store app that can authenticate users, securely call web APIs using OAuth 2.0, and get basic information about the user.</span></span>

<span data-ttu-id="e2ace-174">Om du inte redan har fyllts i din klient med användare, nu är det dags att göra det.</span><span class="sxs-lookup"><span data-stu-id="e2ace-174">If you haven’t already populated your tenant with users, now is the time to do so.</span></span>
1. <span data-ttu-id="e2ace-175">Kör appen DirectorySearcher och logga sedan in med en användare.</span><span class="sxs-lookup"><span data-stu-id="e2ace-175">Run your DirectorySearcher app, and then sign in with one of the users.</span></span>
2. <span data-ttu-id="e2ace-176">Sök efter andra användare baserat på deras UPN.</span><span class="sxs-lookup"><span data-stu-id="e2ace-176">Search for other users based on their UPN.</span></span>
3. <span data-ttu-id="e2ace-177">Stäng appen och köra den igen.</span><span class="sxs-lookup"><span data-stu-id="e2ace-177">Close the app, and rerun it.</span></span> <span data-ttu-id="e2ace-178">Observera hur användarens session förblir intakt.</span><span class="sxs-lookup"><span data-stu-id="e2ace-178">Notice how the user’s session remains intact.</span></span>
4. <span data-ttu-id="e2ace-179">Logga ut genom att högerklicka på att visa det nedre fältet och sedan logga in igen som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="e2ace-179">Sign out by right-clicking to display the bottom bar, and then sign back in as another user.</span></span>

<span data-ttu-id="e2ace-180">ADAL gör det enkelt att lägga till alla vanliga identitet funktionerna i appen.</span><span class="sxs-lookup"><span data-stu-id="e2ace-180">ADAL makes it easy to incorporate all these common identity features into the app.</span></span> <span data-ttu-id="e2ace-181">Det tar hand om ändrad arbetet, till exempel hantering av cache OAuth protokollstöd presenterar en inloggning Användargränssnittet för användaren och uppdatera gått token.</span><span class="sxs-lookup"><span data-stu-id="e2ace-181">It takes care of all the dirty work for you, such as cache management, OAuth protocol support, presenting the user with a login UI, and refreshing expired tokens.</span></span> <span data-ttu-id="e2ace-182">Du behöver bara ett enda API-anrop, `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="e2ace-182">You need to know only a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="e2ace-183">För referens, ladda ned den [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (utan dina konfigurationsvärden).</span><span class="sxs-lookup"><span data-stu-id="e2ace-183">For reference, download the [completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="e2ace-184">Du kan nu gå vidare till ytterligare identitet scenarier.</span><span class="sxs-lookup"><span data-stu-id="e2ace-184">You can now move on to additional identity scenarios.</span></span> <span data-ttu-id="e2ace-185">Till exempel försöka [skydda ett .NET-webb-API med Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e2ace-185">For example, try [Secure a .NET Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

---
title: "aaaAzure AD Windows Phone komma igång | Microsoft Docs"
description: "Hur skyddade toobuild ett Windows Phone-program som kan integreras med Azure AD för inloggning och Azure AD-anrop API: er som använder sig av OAuth."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e766bfcdfae10483772154f4b5facdec05fc846f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-a-windows-phone-app"></a><span data-ttu-id="8f5e1-103">Integrera Azure AD med en Windows Phone-App</span><span class="sxs-lookup"><span data-stu-id="8f5e1-103">Integrate Azure AD with a Windows Phone App</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> <span data-ttu-id="8f5e1-104">Windows Phone 8.1 och projekt i tidigare versioner stöds inte i Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-104">Windows Phone 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="8f5e1-105">Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).</span><span class="sxs-lookup"><span data-stu-id="8f5e1-105">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

<span data-ttu-id="8f5e1-106">Om du utvecklar en app för Windows Phone 8.1, gör Azure AD det lätt för tooauthenticate du dina användare med deras Active Directory-konton.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-106">If you're developing a Windows Phone 8.1 app, Azure AD makes it simple and straightforward for you tooauthenticate your users with their Active Directory accounts.</span></span>  <span data-ttu-id="8f5e1-107">Program-toosecurely kan också använda en webb-API som skyddas av Azure AD, som hello Office 365-API: er eller hello Azure API.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-107">It also enables your application toosecurely consume any web API protected by Azure AD, such as hello Office 365 APIs or hello Azure API.</span></span>

> [!NOTE]
> <span data-ttu-id="8f5e1-108">Det här kodexemplet använder ADAL version 2.0.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-108">This code sample uses ADAL v2.0.</span></span>  <span data-ttu-id="8f5e1-109">Senaste hello-teknik kan du vill tooinstead försök vår [Windows Universal självstudier med ADAL v3.0](active-directory-devquickstarts-windowsstore.md).</span><span class="sxs-lookup"><span data-stu-id="8f5e1-109">For hello latest technology, you may want tooinstead try our [Windows Universal Tutorial using ADAL v3.0](active-directory-devquickstarts-windowsstore.md).</span></span>  <span data-ttu-id="8f5e1-110">Om du verkligen skapar en app för Windows Phone 8.1 är hello rätt plats.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-110">If you are indeed building an app for Windows Phone 8.1, this is hello right place.</span></span>  <span data-ttu-id="8f5e1-111">ADAL v2.0 stöds fortfarande helt och hello rekommenderat sätt att utveckla appar agianst Windows Phone 8.1 använder Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-111">ADAL v2.0 is still fully supported, and is hello recommended way of developing apps agianst Windows Phone 8.1 using Azure AD.</span></span>
> 
> 

<span data-ttu-id="8f5e1-112">Azure AD innehåller hello Active Directory Authentication Library eller ADAL för .NET interna klienter som behöver tooaccess skyddade resurser.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-112">For .NET native clients that need tooaccess protected resources, Azure AD provides hello Active Directory Authentication Library, or ADAL.</span></span>  <span data-ttu-id="8f5e1-113">ADALS enda syfte i livslängd är toomake det enkelt för din app tooget åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-113">ADAL’s sole purpose in life is toomake it easy for your app tooget access tokens.</span></span>  <span data-ttu-id="8f5e1-114">toodemonstrate hur lätt det är här vi ska skapa en ”Directory användaren” Windows Phone 8.1-app som:</span><span class="sxs-lookup"><span data-stu-id="8f5e1-114">toodemonstrate just how easy it is, here we’ll build a "Directory Searcher" Windows Phone 8.1 app that:</span></span>

* <span data-ttu-id="8f5e1-115">Hämtar åtkomst till token för att anropa hello Azure AD Graph API: et med hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="8f5e1-115">Gets access tokens for calling hello Azure AD Graph API using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="8f5e1-116">Söker en katalog för användare med ett angivet UPN.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-116">Searches a directory for users with a given UPN.</span></span>
* <span data-ttu-id="8f5e1-117">Användare loggar ut.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-117">Signs users out.</span></span>

<span data-ttu-id="8f5e1-118">toobuild hello fullständig fungerande program, måste du:</span><span class="sxs-lookup"><span data-stu-id="8f5e1-118">toobuild hello complete working application, you’ll need to:</span></span>

1. <span data-ttu-id="8f5e1-119">Registrera ditt program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-119">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="8f5e1-120">Installera och konfigurera ADAL.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-120">Install & Configure ADAL.</span></span>
3. <span data-ttu-id="8f5e1-121">Använd ADAL tooget token från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-121">Use ADAL tooget tokens from Azure AD.</span></span>

<span data-ttu-id="8f5e1-122">tooget har startats [ladda ned stommen av ett projekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="8f5e1-122">tooget started, [download a skeleton project](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="8f5e1-123">Varje är en lösning för Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-123">Each is a Visual Studio 2013 solution.</span></span>  <span data-ttu-id="8f5e1-124">Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-124">You'll also need an Azure AD tenant in which you can create users and register an application.</span></span>  <span data-ttu-id="8f5e1-125">Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="8f5e1-125">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="1-register-hello-directory-searcher-application"></a><span data-ttu-id="8f5e1-126">1. Registrera hello Directory användaren-program</span><span class="sxs-lookup"><span data-stu-id="8f5e1-126">1. Register hello Directory Searcher Application</span></span>
<span data-ttu-id="8f5e1-127">tooenable tooget token för din app behöver du tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="8f5e1-127">tooenable your app tooget tokens, you’ll first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="8f5e1-128">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8f5e1-128">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="8f5e1-129">Hello översta, klicka på fältet för ditt konto och på hello **Directory** Välj plats tooregister programmet hello Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-129">On hello top bar, click on your account and under hello **Directory** list, choose hello Active Directory tenant where you wish tooregister your application.</span></span>
3. <span data-ttu-id="8f5e1-130">Klicka på **fler tjänster** i hello vänstra nav och välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-130">Click on **More Services** in hello left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="8f5e1-131">Klicka på **App registreringar** och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-131">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="8f5e1-132">Följ anvisningarna för hello och skapa en ny **internt klientprogram**.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-132">Follow hello prompts and create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="8f5e1-133">Hej **namn** av hello programmet beskriver dina program tooend-användare</span><span class="sxs-lookup"><span data-stu-id="8f5e1-133">hello **Name** of hello application will describe your application tooend-users</span></span>
  * <span data-ttu-id="8f5e1-134">Hej **omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD ska använda tooreturn token svar.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-134">hello **Redirect Uri** is a scheme and string combination that Azure AD will use tooreturn token responses.</span></span>  <span data-ttu-id="8f5e1-135">Ange en platshållarvärde för nu, t.ex. `http://DirectorySearcher`.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-135">Enter a placeholder value for now, e.g. `http://DirectorySearcher`.</span></span>  <span data-ttu-id="8f5e1-136">Vi kommer att ersätta det här värdet senare.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-136">We'll replace this value later.</span></span>
6. <span data-ttu-id="8f5e1-137">När du har slutfört registreringen tilldelas AAD appen ett unikt program-ID.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-137">Once you’ve completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="8f5e1-138">Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello programfliken.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-138">You’ll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="8f5e1-139">Från hello **inställningar** väljer **nödvändiga behörigheter** och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-139">From hello **Settings** page, choose **Required Permissions** and choose **Add**.</span></span> <span data-ttu-id="8f5e1-140">Välj hello **Microsoft Graph** som hello API och Lägg till hello **läsa katalogdata** behörighet under **delegerade behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-140">Select hello **Microsoft Graph** as hello API and add hello **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="8f5e1-141">Detta gör att dina program tooquery hello Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-141">This will enable your application tooquery hello Graph API for users.</span></span>

## <a name="2-install--configure-adal"></a><span data-ttu-id="8f5e1-142">2. Installera och konfigurera ADAL</span><span class="sxs-lookup"><span data-stu-id="8f5e1-142">2. Install & Configure ADAL</span></span>
<span data-ttu-id="8f5e1-143">Nu när du har ett program i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-143">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="8f5e1-144">ADAL toobe kan toocommunicate med Azure AD, du måste för tooprovide med viss information om din appregistrering.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-144">In order for ADAL toobe able toocommunicate with Azure AD, you need tooprovide it with some information about your app registration.</span></span>

* <span data-ttu-id="8f5e1-145">Börja genom att lägga till ADAL toohello DirectorySearcher projektet med hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-145">Begin by adding ADAL toohello DirectorySearcher project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* <span data-ttu-id="8f5e1-146">Öppna i hello DirectorySearcher project `MainPage.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-146">In hello DirectorySearcher project, open `MainPage.xaml.cs`.</span></span>  <span data-ttu-id="8f5e1-147">Ersätt hello värden i hello `Config Values` region tooreflect hello värden indata i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-147">Replace hello values in hello `Config Values` region tooreflect hello values you input into hello Azure Portal.</span></span>  <span data-ttu-id="8f5e1-148">Koden ska referera till dessa värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-148">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="8f5e1-149">Hej `tenant` är hello domän för din Azure AD-klient, t.ex. contoso.onmicrosoft.com</span><span class="sxs-lookup"><span data-stu-id="8f5e1-149">hello `tenant` is hello domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com</span></span>
  * <span data-ttu-id="8f5e1-150">Hej `clientId` är hello clientId på ditt program som du kopierade från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-150">hello `clientId` is hello clientId of your application you copied from hello portal.</span></span>
* <span data-ttu-id="8f5e1-151">Du måste nu toodiscover hello återanrop uri för Windows Phone-app.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-151">You now need toodiscover hello callback uri for your Windows Phone app.</span></span>  <span data-ttu-id="8f5e1-152">Ange en brytpunkt på den här raden i hello `MainPage` metoden:</span><span class="sxs-lookup"><span data-stu-id="8f5e1-152">Set a breakpoint on this line in hello `MainPage` method:</span></span>

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* <span data-ttu-id="8f5e1-153">Kör hello appen och kopiera reserverats hello värdet för `redirectUri` när hello brytpunkt träffar.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-153">Run hello app, and copy aside hello value of `redirectUri` when hello breakpoint is hit.</span></span>  <span data-ttu-id="8f5e1-154">Det bör se ut ungefär så</span><span class="sxs-lookup"><span data-stu-id="8f5e1-154">It should look something like</span></span>

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* <span data-ttu-id="8f5e1-155">Tillbaka på hello **konfigurera** fliken i ditt program i hello Azure Management Portal ersätta hello värde i hello **RedirectUri** med det här värdet.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-155">Back on hello **Configure** tab of your application in hello Azure Management Portal, replace hello value of hello **RedirectUri** with this value.</span></span>  

## <a name="3-use-adal-tooget-tokens-from-aad"></a><span data-ttu-id="8f5e1-156">3. Använd ADAL tooGet token från AAD</span><span class="sxs-lookup"><span data-stu-id="8f5e1-156">3. Use ADAL tooGet Tokens from AAD</span></span>
<span data-ttu-id="8f5e1-157">hello grundläggande principen bakom ADAL är att när din app behöver en åtkomst-token kan bara anropas `authContext.AcquireToken(…)`, och ADAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-157">hello basic principle behind ADAL is that whenever your app needs an access token, it simply calls `authContext.AcquireToken(…)`, and ADAL does hello rest.</span></span>  

* <span data-ttu-id="8f5e1-158">hello första steget är tooinitialize appens `AuthenticationContext` -ADAL vars primära klassen.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-158">hello first step is tooinitialize your app’s `AuthenticationContext` - ADAL’s primary class.</span></span>  <span data-ttu-id="8f5e1-159">Det är där du skickar ADAL hello koordinater måste toocommunicate med Azure AD och anger hur toocache token.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-159">This is where you pass ADAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* <span data-ttu-id="8f5e1-160">Hitta nu hello `Search(...)` metod som kommer att anropas när hello användaren cliks hello ”Sök”-knappen i hello appens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-160">Now locate hello `Search(...)` method, which will be invoked when hello user cliks hello "Search" button in hello app's UI.</span></span>  <span data-ttu-id="8f5e1-161">Den här metoden gör en GET-begäran toohello Azure AD Graph API tooquery för användare vars UPN-namnet börjar med hello angivna sökord.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-161">This method makes a GET request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span>  <span data-ttu-id="8f5e1-162">Men i ordning tooquery hello Graph API, behöver du tooinclude en access_token i hello `Authorization` rubriken för hello begäran - detta är där ADAL kommer in.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-162">But in order tooquery hello Graph API, you need tooinclude an access_token in hello `Authorization` header of hello request - this is where ADAL comes in.</span></span>

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try tooget a token without triggering any user prompt.
    // ADAL will check whether hello requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained hello QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* <span data-ttu-id="8f5e1-163">Om interaktiv autentisering krävs ADAL använder Windows Phone Web Authentication Service Broker (Windows Adressbok) och [fortsättning modellen](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) toodisplay hello Azure AD inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-163">If interactive authentication is necessary, ADAL will use Windows Phone's Web Authentication Broker (WAB) and [continuation model](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) toodisplay hello Azure AD sign in page.</span></span>  <span data-ttu-id="8f5e1-164">När hello användaren loggar in, måste appen toopass ADAL hello resultaten av hello Windows Adressbok interaktion.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-164">When hello user signs in, your app needs toopass ADAL hello results of hello WAB interaction.</span></span>  <span data-ttu-id="8f5e1-165">Det är enkelt implementera hello `ContinueWebAuthentication` gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="8f5e1-165">This is as simple as implementing hello `ContinueWebAuthentication` interface:</span></span>

```C#
// This method is automatically invoked when hello application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass hello authentication interaction results tooADAL, which will
    // conclude hello token acquisition operation and invoke hello callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* <span data-ttu-id="8f5e1-166">Nu är det tid toouse hello `AuthenticationResult` som ADAL returnerade tooyour app.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-166">Now it's time toouse hello `AuthenticationResult` that ADAL returned tooyour app.</span></span>  <span data-ttu-id="8f5e1-167">I hello `QueryGraph(...)` motringning, bifoga hello access_token som du har köpt toohello GET-begäran i hello Authorization-huvud:</span><span class="sxs-lookup"><span data-stu-id="8f5e1-167">In hello `QueryGraph(...)` callback, attach hello access_token you acquired toohello GET request in hello Authorization header:</span></span>

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add hello access token toohello Authorization Header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* <span data-ttu-id="8f5e1-168">Du kan också använda hello `AuthenticationResult` objekt toodisplay information om hello användare i din app.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-168">You can also use hello `AuthenticationResult` object toodisplay information about hello user in your app.</span></span> <span data-ttu-id="8f5e1-169">I hello `QueryGraph(...)` metod, Använd hello resultatet tooshow hello användarens id på hello sida:</span><span class="sxs-lookup"><span data-stu-id="8f5e1-169">In hello `QueryGraph(...)` method, use hello result tooshow hello user's id on hello page:</span></span>

```C#
// Update hello Page UI toorepresent hello signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* <span data-ttu-id="8f5e1-170">Slutligen kan du använda ADAL toosign hello användare utanför programmet samt.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-170">Finally, you can use ADAL toosign hello user out of hte application as well.</span></span>  <span data-ttu-id="8f5e1-171">När hello användaren klickar hello ”logga ut”, vi vill tooensure som hello nästa anrop för`AcquireTokenSilentAsync(...)` misslyckas.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-171">When hello user clicks hello "Sign Out" button, we want tooensure that hello next call too`AcquireTokenSilentAsync(...)` will fail.</span></span>  <span data-ttu-id="8f5e1-172">Det är lika enkelt som att rensa hello token-cache med ADAL:</span><span class="sxs-lookup"><span data-stu-id="8f5e1-172">With ADAL, this is as easy as clearing hello token cache:</span></span>

```C#
private void SignOut()
{
    // Clear session state from hello token cache.
    authContext.TokenCache.Clear();

    ...
}
```

<span data-ttu-id="8f5e1-173">Grattis!</span><span class="sxs-lookup"><span data-stu-id="8f5e1-173">Congratulations!</span></span> <span data-ttu-id="8f5e1-174">Du nu har en aktiv Windows Phone-app som har hello möjlighet tooauthenticate användare på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och hämta grundläggande information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-174">You now have a working Windows Phone app that has hello ability tooauthenticate users, securely call Web APIs using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="8f5e1-175">Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-175">If you haven’t already, now is hello time toopopulate your tenant with some users.</span></span>  <span data-ttu-id="8f5e1-176">Kör appen DirectorySearcher och logga in med något av dessa användare.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-176">Run your DirectorySearcher app, and sign in with one of those users.</span></span>  <span data-ttu-id="8f5e1-177">Sök efter andra användare baserat på deras UPN.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-177">Search for other users based on their UPN.</span></span>  <span data-ttu-id="8f5e1-178">Stäng hello app och kör den igen.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-178">Close hello app, and re-run it.</span></span>  <span data-ttu-id="8f5e1-179">Observera hur hello användarens session förblir intakt.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-179">Notice how hello user’s session remains intact.</span></span>  <span data-ttu-id="8f5e1-180">Logga ut och logga in igen som en annan användare.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-180">Sign out, and sign back in as another user.</span></span>

<span data-ttu-id="8f5e1-181">ADAL gör det enkelt tooincorporate alla dessa vanliga identity-funktioner i programmet.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-181">ADAL makes it easy tooincorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="8f5e1-182">Det hand tar om alla hello ändrad arbete du - hantering av cache, OAuth protokollstöd presentera hello användare med en inloggning UI, uppdatera token har upphört att gälla och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-182">It takes care of all hello dirty work for you - cache management, OAuth protocol support, presenting hello user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="8f5e1-183">Allt du verkligen behöver tooknow är ett enda API-anrop `authContext.AcquireToken*(…)`.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-183">All you really need tooknow is a single API call, `authContext.AcquireToken*(…)`.</span></span>

<span data-ttu-id="8f5e1-184">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) tillhandahålls [här](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="8f5e1-184">For reference, hello completed sample (without your configuration values) is provided [here](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).</span></span>  <span data-ttu-id="8f5e1-185">Du kan nu gå vidare tooadditional identitet scenarier.</span><span class="sxs-lookup"><span data-stu-id="8f5e1-185">You can now move on tooadditional identity scenarios.</span></span>  <span data-ttu-id="8f5e1-186">Du kanske vill tootry:</span><span class="sxs-lookup"><span data-stu-id="8f5e1-186">You may want tootry:</span></span>

[<span data-ttu-id="8f5e1-187">Skydda ett .NET-webb-API med Azure AD >></span><span class="sxs-lookup"><span data-stu-id="8f5e1-187">Secure a .NET Web API with Azure AD >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


---
title: aaaAzure Active Directory v2.0 .NET inbyggda appen | Microsoft Docs
description: "Hur toobuild en intern app för .NET som loggar användarna in med både personliga Account och arbets-eller skolkonton."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 46d81e09-bad0-44ce-9026-881805976e72
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/30/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 9418eeba02b800feee5cb00219574eb16506f0a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooa-windows-desktop-app"></a><span data-ttu-id="e6ccf-103">Lägga till inloggning tooa Windows Desktop-appen</span><span class="sxs-lookup"><span data-stu-id="e6ccf-103">Add sign-in tooa Windows Desktop app</span></span>
<span data-ttu-id="e6ccf-104">Hello hello v2.0-slutpunkten kan du snabbt lägga till autentisering tooyour skrivbordsappar med stöd för både personliga Microsoft-konton och arbets-eller skolkonton.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-104">With hello hello v2.0 endpoint, you can quickly add authentication tooyour desktop apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="e6ccf-105">Den även kan din app toosecurely kommunicera med en serverdel webb-api, samt [hello Microsoft Graph](https://graph.microsoft.io) och några av hello [Office 365 Unified-API: er](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span><span class="sxs-lookup"><span data-stu-id="e6ccf-105">It also enables your app toosecurely communicate with a backend web api, as well as [hello Microsoft Graph](https://graph.microsoft.io) and a few of hello [Office 365 Unified APIs](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).</span></span>

> [!NOTE]
> <span data-ttu-id="e6ccf-106">Inte alla Azure Active Directory (AD)-scenarier och funktioner som stöds av hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-106">Not all Azure Active Directory (AD) scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="e6ccf-107">toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="e6ccf-107">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

<span data-ttu-id="e6ccf-108">För [.NET interna appar som körs på en enhet](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD innehåller hello Autentiseringsbibliotek för Microsoft Identity eller MSAL.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-108">For [.NET native apps that run on a device](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD provides hello Microsoft Identity Authentication Library, or MSAL.</span></span>  <span data-ttu-id="e6ccf-109">MSALS uteslutande i livslängd är det enkelt för din app tooget token för att anropa webbtjänster toomake.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-109">MSAL's sole purpose in life is toomake it easy for your app tooget tokens for calling web services.</span></span>  <span data-ttu-id="e6ccf-110">toodemonstrate hur lätt det är här vi ska skapa en uppgiftslista för .NET WPF-app som:</span><span class="sxs-lookup"><span data-stu-id="e6ccf-110">toodemonstrate just how easy it is, here we'll build a .NET WPF To-Do List app that:</span></span>

* <span data-ttu-id="e6ccf-111">Loggar hello användare i & hämtar åtkomst-token som använder hello [OAuth 2.0-autentiseringsprotokollet](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="e6ccf-111">Signs hello user in & gets access tokens using hello [OAuth 2.0 authentication protocol](active-directory-v2-protocols.md).</span></span>
* <span data-ttu-id="e6ccf-112">På ett säkert sätt anropar en serverdel uppgiftslista webbtjänsten, som också är skyddad av OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-112">Securely calls a backend To-Do List web service, which is also secured by OAuth 2.0.</span></span>
* <span data-ttu-id="e6ccf-113">Loggar hello användare ut.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-113">Signs hello user out.</span></span>

## <a name="download-sample-code"></a><span data-ttu-id="e6ccf-114">Hämta exempelkoden</span><span class="sxs-lookup"><span data-stu-id="e6ccf-114">Download sample code</span></span>
<span data-ttu-id="e6ccf-115">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="e6ccf-115">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).</span></span>  <span data-ttu-id="e6ccf-116">toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) eller klona hello stommen:</span><span class="sxs-lookup"><span data-stu-id="e6ccf-116">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

<span data-ttu-id="e6ccf-117">hello slutförts app tillhandahålls hello slutet av den här kursen samt.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-117">hello completed app is provided at hello end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="e6ccf-118">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="e6ccf-118">Register an app</span></span>
<span data-ttu-id="e6ccf-119">Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="e6ccf-119">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="e6ccf-120">Se till att:</span><span class="sxs-lookup"><span data-stu-id="e6ccf-120">Make sure to:</span></span>

* <span data-ttu-id="e6ccf-121">Kopiera hello **program-Id** tilldelade tooyour app måste den snart.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-121">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>
* <span data-ttu-id="e6ccf-122">Lägg till hello **Mobile** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-122">Add hello **Mobile** platform for your app.</span></span>

## <a name="install--configure-msal"></a><span data-ttu-id="e6ccf-123">Installera och konfigurera MSAL</span><span class="sxs-lookup"><span data-stu-id="e6ccf-123">Install & Configure MSAL</span></span>
<span data-ttu-id="e6ccf-124">Du kan installera MSAL och Skriv koden identitetsrelaterade nu när du har en app som registrerats med Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-124">Now that you have an app registered with Microsoft, you can install MSAL and write your identity-related code.</span></span>  <span data-ttu-id="e6ccf-125">För att MSAL toobe kan toocommunicate hello v2.0-slutpunkten måste tooprovide med viss information om din appregistrering.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-125">In order for MSAL toobe able toocommunicate hello v2.0 endpoint, you need tooprovide it with some information about your app registration.</span></span>

* <span data-ttu-id="e6ccf-126">Börja genom att lägga till MSAL toohello TodoListClient projektet med hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-126">Begin by adding MSAL toohello TodoListClient project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* <span data-ttu-id="e6ccf-127">Öppna i hello TodoListClient project `app.config`.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-127">In hello TodoListClient project, open `app.config`.</span></span>  <span data-ttu-id="e6ccf-128">Ersätt hello värdena för hello element i hello `<appSettings>` avsnittet tooreflect hello värden indata i portalen för registrering av hello-app.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-128">Replace hello values of hello elements in hello `<appSettings>` section tooreflect hello values you input into hello app registration portal.</span></span>  <span data-ttu-id="e6ccf-129">Koden ska referera till dessa värden när den används i MSAL.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-129">Your code will reference these values whenever it uses MSAL.</span></span>
  
  * <span data-ttu-id="e6ccf-130">Hej `ida:ClientId` är hello **program-Id** för din app som du kopierade från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-130">hello `ida:ClientId` is hello **Application Id** of your app you copied from hello portal.</span></span>
* <span data-ttu-id="e6ccf-131">I hello TodoList-Service-projekt, öppna `web.config` i hello rot hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-131">In hello TodoList-Service project, open `web.config` in hello root of hello project.</span></span>  
  
  * <span data-ttu-id="e6ccf-132">Ersätt hello `ida:Audience` värdet med hello samma **program-Id** från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-132">Replace hello `ida:Audience` value with hello same **Application Id** from hello portal.</span></span>

## <a name="use-msal-tooget-tokens"></a><span data-ttu-id="e6ccf-133">Använd MSAL tooget token</span><span class="sxs-lookup"><span data-stu-id="e6ccf-133">Use MSAL tooget tokens</span></span>
<span data-ttu-id="e6ccf-134">hello grundläggande principen bakom MSAL är att när en app måste en åtkomst-token, ringer du bara `app.AcquireToken(...)`, och MSAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-134">hello basic principle behind MSAL is that whenever your app needs an access token, you simply call `app.AcquireToken(...)`, and MSAL does hello rest.</span></span>  

* <span data-ttu-id="e6ccf-135">I hello `TodoListClient` projektet öppnar `MainWindow.xaml.cs` och leta upp hello `OnInitialized(...)` metod.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-135">In hello `TodoListClient` project, open `MainWindow.xaml.cs` and locate hello `OnInitialized(...)` method.</span></span>  <span data-ttu-id="e6ccf-136">hello första steget är tooinitialize appens `PublicClientApplication` -MSALS primära klass som representerar interna program.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-136">hello first step is tooinitialize your app's `PublicClientApplication` - MSAL's primary class representing native applications.</span></span>  <span data-ttu-id="e6ccf-137">Det är där du skickar MSAL hello koordinater måste toocommunicate med Azure AD och anger hur toocache token.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-137">This is where you pass MSAL hello coordinates it needs toocommunicate with Azure AD and tell it how toocache tokens.</span></span>

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* <span data-ttu-id="e6ccf-138">När hello app startar vi vill toocheck och se om hello användaren redan har loggat in hello app.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-138">When hello app starts up, we want toocheck and see if hello user is already signed into hello app.</span></span>  <span data-ttu-id="e6ccf-139">Men vi ännu vill inte tooinvoke en inloggning UI - vi kommer att hello användaren klicka på ”Logga In” toodo så.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-139">However, we don't want tooinvoke a sign-in UI just yet - we'll make hello user click "Sign In" toodo so.</span></span>  <span data-ttu-id="e6ccf-140">Även i hello `OnInitialized(...)` metoden:</span><span class="sxs-lookup"><span data-stu-id="e6ccf-140">Also in hello `OnInitialized(...)` method:</span></span>

```C#
// As hello app starts, we want toocheck toosee if hello user is already signed in.
// You can do so by trying tooget a token from MSAL, using hello method
// AcquireTokenSilent.  This forces MSAL toothrow an exception if it cannot
// get a token for hello user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in hello cache - or MSAL was able tooget a new oen via refresh token.
    // Proceed toofetch hello user's tasks from hello TodoListService via hello GetTodoList() method.

    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, hello app should take no action,
        // and simply show hello user hello sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

* <span data-ttu-id="e6ccf-141">Om hello användaren inte är inloggad och de klickar på knappen ”Logga In” för hello, vi vill tooinvoke inloggningen UI och hello användare anger sina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-141">If hello user is not signed in and they click hello "Sign In" button, we want tooinvoke a login UI and have hello user enter their credentials.</span></span>  <span data-ttu-id="e6ccf-142">Implementera hello inloggning knappen hanterare:</span><span class="sxs-lookup"><span data-stu-id="e6ccf-142">Implement hello Sign-In button handler:</span></span>

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign hello user out if they clicked hello "Clear Cache" button

// If hello user clicked hello 'Sign-In' button, force
// MSAL tooprompt hello user for credentials by using
// AcquireTokenAsync, a method that is guaranteed tooshow a prompt toohello user.
// MSAL will get a token for hello TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If hello user canceled hello login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by hello user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

* <span data-ttu-id="e6ccf-143">Om hello användaren har loggar in MSAL ska ta emot och cachelagra en token för dig och du kan fortsätta toocall hello `GetTodoList()` metod med förtroende.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-143">If hello user successfully signs-in, MSAL will receive and cache a token for you, and you can proceed toocall hello `GetTodoList()` method with confidence.</span></span>  <span data-ttu-id="e6ccf-144">Alla som har lämnat tooget en användares uppgifter är tooimplement hello `GetTodoList()` metod.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-144">All that's left tooget a user's tasks is tooimplement hello `GetTodoList()` method.</span></span>

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try tooget an access token toocall hello TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL toothrow an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show hello user a message
    // and let them click hello Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once hello token has been returned by MSAL,
// add it toohello http authorization header,
// before making hello call tooaccess hello tooDo list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When hello user is done managing their To-Do List, they may finally sign out of hello app by clicking hello "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If hello user clicked hello 'clear cache' button,
        // clear hello MSAL token cache and show hello user as signed out.
        // It's also necessary tooclear hello cookies from hello browser
        // control so hello next user has a chance toosign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a><span data-ttu-id="e6ccf-145">Kör</span><span class="sxs-lookup"><span data-stu-id="e6ccf-145">Run</span></span>
<span data-ttu-id="e6ccf-146">Grattis!</span><span class="sxs-lookup"><span data-stu-id="e6ccf-146">Congratulations!</span></span> <span data-ttu-id="e6ccf-147">Du har nu en fungerande .NET WPF-program som har hello möjlighet tooauthenticate användare & på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-147">You now have a working .NET WPF app that has hello ability tooauthenticate users & securely call Web APIs using OAuth 2.0.</span></span>  <span data-ttu-id="e6ccf-148">Köra båda projekten och logga in med ett personligt microsoftkonto eller ett arbets- eller skolkonto konto.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-148">Run your both projects, and sign in with either a personal Microsoft account or a work or school account.</span></span>  <span data-ttu-id="e6ccf-149">Lägga till uppgifter toothat användarens att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-149">Add tasks toothat user's To-Do list.</span></span>  <span data-ttu-id="e6ccf-150">Logga ut och logga in igen som en annan användare tooview sina att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-150">Sign out, and sign back in as another user tooview their To-Do list.</span></span>  <span data-ttu-id="e6ccf-151">Stäng hello app och kör den igen.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-151">Close hello app, and re-run it.</span></span>  <span data-ttu-id="e6ccf-152">Observera hur hello användarens session förblir intakta, som beror på att hello app cachelagrar token i en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-152">Notice how hello user's session remains intact - that is because hello app caches tokens in a local file.</span></span>

<span data-ttu-id="e6ccf-153">MSAL gör det enkelt tooincorporate vanliga identity-funktioner i din app använder både personliga och arbetsrelaterade konton.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-153">MSAL makes it easy tooincorporate common identity features into your app, using both personal and work accounts.</span></span>  <span data-ttu-id="e6ccf-154">Det hand tar om alla hello ändrad arbete du - hantering av cache, OAuth protokollstöd presentera hello användare med en inloggning UI, uppdatera token har upphört att gälla och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-154">It takes care of all hello dirty work for you - cache management, OAuth protocol support, presenting hello user with a login UI, refreshing expired tokens, and more.</span></span>  <span data-ttu-id="e6ccf-155">Allt du verkligen behöver tooknow är ett enda API-anrop `app.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-155">All you really need tooknow is a single API call, `app.AcquireTokenAsync(...)`.</span></span>

<span data-ttu-id="e6ccf-156">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), eller kan du klona den från GitHub:</span><span class="sxs-lookup"><span data-stu-id="e6ccf-156">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="e6ccf-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6ccf-157">Next steps</span></span>
<span data-ttu-id="e6ccf-158">Du kan nu gå vidare till mer avancerade avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-158">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="e6ccf-159">Du kanske vill tootry:</span><span class="sxs-lookup"><span data-stu-id="e6ccf-159">You may want tootry:</span></span>

* [<span data-ttu-id="e6ccf-160">Att säkra hello TodoListService Web API med hello v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="e6ccf-160">Securing hello TodoListService Web API with hello v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-dotnet-api.md)

<span data-ttu-id="e6ccf-161">För ytterligare resurser, kolla:</span><span class="sxs-lookup"><span data-stu-id="e6ccf-161">For additional resources, check out:</span></span>  

* [<span data-ttu-id="e6ccf-162">Utvecklarhandbok för hello v2.0 >></span><span class="sxs-lookup"><span data-stu-id="e6ccf-162">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="e6ccf-163">StackOverflow ”msal” taggen >></span><span class="sxs-lookup"><span data-stu-id="e6ccf-163">StackOverflow "msal" tag >></span></span>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="e6ccf-164">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="e6ccf-164">Get security updates for our products</span></span>
<span data-ttu-id="e6ccf-165">Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="e6ccf-165">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>


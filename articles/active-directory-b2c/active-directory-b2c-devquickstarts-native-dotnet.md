---
title: Azure Active Directory B2C | Microsoft Docs
description: "Hur du skapar ett Windows-skrivbordsprogram som innehåller inloggning, registrering och profilhantering med hjälp av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 9da14362-8216-4485-960e-af17cd5ba3bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.openlocfilehash: 8e2b5c704230ee2ba1395dc76a1551aaa8e7af7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a><span data-ttu-id="83ce4-103">Azure AD B2C: Skapa en Windows-skrivbordsapp</span><span class="sxs-lookup"><span data-stu-id="83ce4-103">Azure AD B2C: Build a Windows desktop app</span></span>
<span data-ttu-id="83ce4-104">Med hjälp av Azure Active Directory (AD Azure) B2C kan du lägga till kraftfulla självbetjäning Identitetshantering i appen skrivbord i några korta steg.</span><span class="sxs-lookup"><span data-stu-id="83ce4-104">By using Azure Active Directory (Azure AD) B2C, you can add powerful self-service identity management features to your desktop app in a few short steps.</span></span> <span data-ttu-id="83ce4-105">Den här artikeln visar hur du skapar ett .NET Windows Presentation Foundation (WPF) ”uppgiftslistan” som innehåller användarregistrering, inloggning och profilhantering.</span><span class="sxs-lookup"><span data-stu-id="83ce4-105">This article will show you how to create a .NET Windows Presentation Foundation (WPF) "to-do list" app that includes user sign-up, sign-in, and profile management.</span></span> <span data-ttu-id="83ce4-106">Appen tas stöd för registrering och inloggning med ett användarnamn eller e-post.</span><span class="sxs-lookup"><span data-stu-id="83ce4-106">The app will include support for sign-up and sign-in by using a user name or email.</span></span> <span data-ttu-id="83ce4-107">Den omfattar också stöd för registrering och inloggning med hjälp av sociala konton, till exempel Facebook och Google.</span><span class="sxs-lookup"><span data-stu-id="83ce4-107">It will also include support for sign-up and sign-in by using social accounts such as Facebook and Google.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="83ce4-108">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="83ce4-108">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="83ce4-109">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="83ce4-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="83ce4-110">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="83ce4-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="83ce4-111">Om du inte redan har en B2C-katalog [skapar du en](active-directory-b2c-get-started.md) innan du fortsätter den här guiden.</span><span class="sxs-lookup"><span data-stu-id="83ce4-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="83ce4-112">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="83ce4-112">Create an application</span></span>
<span data-ttu-id="83ce4-113">Nu måste du skapa en app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="83ce4-113">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="83ce4-114">Det ger Azure AD den information som tjänsten behöver för att kommunicera säkert med din app.</span><span class="sxs-lookup"><span data-stu-id="83ce4-114">This gives Azure AD information that it needs to securely communicate with your app.</span></span> <span data-ttu-id="83ce4-115">Du skapar en app genom att följa [dessa anvisningar](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="83ce4-115">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span>  <span data-ttu-id="83ce4-116">Se till att:</span><span class="sxs-lookup"><span data-stu-id="83ce4-116">Be sure to:</span></span>

* <span data-ttu-id="83ce4-117">Inkludera en **native client** i programmet.</span><span class="sxs-lookup"><span data-stu-id="83ce4-117">Include a **native client** in the application.</span></span>
* <span data-ttu-id="83ce4-118">Kopiera den **omdirigerings-URI** `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="83ce4-118">Copy the **Redirect URI** `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="83ce4-119">Den är standard-URL:en för det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="83ce4-119">It is the default URL for this code sample.</span></span>
* <span data-ttu-id="83ce4-120">Kopiera **program-ID:t** som har tilldelats din app.</span><span class="sxs-lookup"><span data-stu-id="83ce4-120">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="83ce4-121">Du behöver den senare.</span><span class="sxs-lookup"><span data-stu-id="83ce4-121">You will need it later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="83ce4-122">Skapa dina principer</span><span class="sxs-lookup"><span data-stu-id="83ce4-122">Create your policies</span></span>
<span data-ttu-id="83ce4-123">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="83ce4-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="83ce4-124">Det här kodexemplet innehåller tre identitetsupplevelser: registrering, inloggning och profilredigering.</span><span class="sxs-lookup"><span data-stu-id="83ce4-124">This code sample contains three identity experiences: sign up, sign in, and edit profile.</span></span> <span data-ttu-id="83ce4-125">Du måste skapa en princip för varje typ, enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="83ce4-125">You need to create a policy for each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="83ce4-126">Tänk på följande när du skapar de tre principerna:</span><span class="sxs-lookup"><span data-stu-id="83ce4-126">When you create the three policies, be sure to:</span></span>

* <span data-ttu-id="83ce4-127">Välj antingen **Registrering med användar-ID** eller **Registrering med e-postadress** i bladet för identitetsproviders.</span><span class="sxs-lookup"><span data-stu-id="83ce4-127">Choose either **User ID sign-up** or **Email sign-up** in the identity providers blade.</span></span>
* <span data-ttu-id="83ce4-128">Välj **Visningsnamn** och andra registreringsattribut i registreringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="83ce4-128">Choose **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="83ce4-129">Välj **Visningsnamn** och **Objekt-ID** som programanspråk för varje princip.</span><span class="sxs-lookup"><span data-stu-id="83ce4-129">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="83ce4-130">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="83ce4-130">You can choose other claims as well.</span></span>
* <span data-ttu-id="83ce4-131">Kopiera **namnet** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="83ce4-131">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="83ce4-132">Det bör ha prefixet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="83ce4-132">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="83ce4-133">Du behöver principnamnen senare.</span><span class="sxs-lookup"><span data-stu-id="83ce4-133">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="83ce4-134">När du har skapat de tre principerna är du redo att bygga din app.</span><span class="sxs-lookup"><span data-stu-id="83ce4-134">After you have successfully created the three policies, you're ready to build your app.</span></span>

## <a name="download-the-code"></a><span data-ttu-id="83ce4-135">Ladda ned koden</span><span class="sxs-lookup"><span data-stu-id="83ce4-135">Download the code</span></span>
<span data-ttu-id="83ce4-136">Koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="83ce4-136">The code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span></span> <span data-ttu-id="83ce4-137">Om du vill bygga exemplet allteftersom kan du [ladda ned stommen av ett projekt som en ZIP-fil](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="83ce4-137">To build the sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span></span> <span data-ttu-id="83ce4-138">Du kan också klona stommen:</span><span class="sxs-lookup"><span data-stu-id="83ce4-138">You can also clone the skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

<span data-ttu-id="83ce4-139">Den färdiga appen finns också [som en ZIP-fil](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) eller i `complete`-grenen för samma centrallager.</span><span class="sxs-lookup"><span data-stu-id="83ce4-139">The completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) or on the `complete` branch of the same repository.</span></span>

<span data-ttu-id="83ce4-140">När du har laddat ned exempelkoden öppnar du SLN-filen i Visual Studio för att sätta igång.</span><span class="sxs-lookup"><span data-stu-id="83ce4-140">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="83ce4-141">Den `TaskClient` projektet är WPF-skrivbordsprogram som användaren interagerar med.</span><span class="sxs-lookup"><span data-stu-id="83ce4-141">The `TaskClient` project is the WPF desktop application that the user interacts with.</span></span> <span data-ttu-id="83ce4-142">Vid tillämpningen av den här självstudiekursen anropar en aktivitet för backend-webb-API, finns i Azure som lagrar varje användares att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="83ce4-142">For the purposes of this tutorial, it calls a back-end task web API, hosted in Azure, that stores each user's to-do list.</span></span>  <span data-ttu-id="83ce4-143">Du behöver inte skapa webb-API, vi har redan det kör du.</span><span class="sxs-lookup"><span data-stu-id="83ce4-143">You do not need to build the web API, we already have it running for you.</span></span>

<span data-ttu-id="83ce4-144">Om du vill veta hur ett webb-API på ett säkert sätt autentiserar begäranden med hjälp av Azure AD B2C kan ta en titt på [webb-API komma igång artikel](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="83ce4-144">To learn how a web API securely authenticates requests by using Azure AD B2C, check out the [web API getting started article](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="execute-policies"></a><span data-ttu-id="83ce4-145">Köra principer</span><span class="sxs-lookup"><span data-stu-id="83ce4-145">Execute policies</span></span>
<span data-ttu-id="83ce4-146">Appen kommunicerar med Azure AD B2C genom att skicka meddelandena som anger den princip som ska köras som en del av HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="83ce4-146">Your app communicates with Azure AD B2C by sending authentication messages that specify the policy they want to execute as part of the HTTP request.</span></span> <span data-ttu-id="83ce4-147">För .NET program kan du använda Förhandsgranska Microsoft Authentication Library (MSAL) för att skicka meddelanden för OAuth 2.0-autentisering, köra principer och hämta token som anropa webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="83ce4-147">For .NET desktop applications, you can use the preview Microsoft Authentication Library (MSAL) to send OAuth 2.0 authentication messages, execute policies, and get tokens that call web APIs.</span></span>

### <a name="install-msal"></a><span data-ttu-id="83ce4-148">Installera MSAL</span><span class="sxs-lookup"><span data-stu-id="83ce4-148">Install MSAL</span></span>
<span data-ttu-id="83ce4-149">Lägg till MSAL till den `TaskClient` projekt med hjälp av Visual Studio Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="83ce4-149">Add MSAL to the `TaskClient` project by using the Visual Studio Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a><span data-ttu-id="83ce4-150">Ange din B2C-information</span><span class="sxs-lookup"><span data-stu-id="83ce4-150">Enter your B2C details</span></span>
<span data-ttu-id="83ce4-151">Öppna filen `Globals.cs` och ersätter varje egenskapsvärden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="83ce4-151">Open the file `Globals.cs` and replace each of the property values with your own.</span></span> <span data-ttu-id="83ce4-152">Den här klassen används i hela `TaskClient` till referens som ofta används värden.</span><span class="sxs-lookup"><span data-stu-id="83ce4-152">This class is used throughout `TaskClient` to reference commonly used values.</span></span>

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="create-the-publicclientapplication"></a><span data-ttu-id="83ce4-153">Skapa PublicClientApplication</span><span class="sxs-lookup"><span data-stu-id="83ce4-153">Create the PublicClientApplication</span></span>
<span data-ttu-id="83ce4-154">Den primära klassen för MSAL är `PublicClientApplication`.</span><span class="sxs-lookup"><span data-stu-id="83ce4-154">The primary class of MSAL is `PublicClientApplication`.</span></span> <span data-ttu-id="83ce4-155">Den här klassen representerar ditt program i Azure AD B2C-system.</span><span class="sxs-lookup"><span data-stu-id="83ce4-155">This class represents your application in the Azure AD B2C system.</span></span> <span data-ttu-id="83ce4-156">När app-initalizes skapar en instans av `PublicClientApplication` i `MainWindow.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="83ce4-156">When the app initalizes, create an instance of `PublicClientApplication` in `MainWindow.xaml.cs`.</span></span> <span data-ttu-id="83ce4-157">Detta kan användas i hela fönstret.</span><span class="sxs-lookup"><span data-stu-id="83ce4-157">This can be used throughout the window.</span></span>

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app,
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### <a name="initiate-a-sign-up-flow"></a><span data-ttu-id="83ce4-158">Initiera registrering flöde</span><span class="sxs-lookup"><span data-stu-id="83ce4-158">Initiate a sign-up flow</span></span>
<span data-ttu-id="83ce4-159">När en användare väljer att loggar in, som du vill initiera en anmälan flödet som använder registreringsprincipen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="83ce4-159">When a user opts to signs up, you want to initiate a sign-up flow that uses the sign-up policy you created.</span></span> <span data-ttu-id="83ce4-160">Genom att använda MSAL kan du bara anropa `pca.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="83ce4-160">By using MSAL, you just call `pca.AcquireTokenAsync(...)`.</span></span> <span data-ttu-id="83ce4-161">De parametrar som du skickar till `AcquireTokenAsync(...)` avgör vilka token felmeddelandet, den princip som används i begäran om autentisering och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="83ce4-161">The parameters you pass to `AcquireTokenAsync(...)` determine which token you receive, the policy used in the authentication request, and more.</span></span>

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a><span data-ttu-id="83ce4-162">Initiera ett flöde för inloggning</span><span class="sxs-lookup"><span data-stu-id="83ce4-162">Initiate a sign-in flow</span></span>
<span data-ttu-id="83ce4-163">Du kan starta ett flöde logga in på samma sätt som du startar en anmälan flöde.</span><span class="sxs-lookup"><span data-stu-id="83ce4-163">You can initiate a sign-in flow in the same way that you initiate a sign-up flow.</span></span> <span data-ttu-id="83ce4-164">När en användare loggar in, göra samma anrop till MSAL, nu med hjälp av din princip för inloggning:</span><span class="sxs-lookup"><span data-stu-id="83ce4-164">When a user signs in, make the same call to MSAL, this time by using your sign-in policy:</span></span>

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a><span data-ttu-id="83ce4-165">Initiera ett flöde för Redigera-profil</span><span class="sxs-lookup"><span data-stu-id="83ce4-165">Initiate an edit-profile flow</span></span>
<span data-ttu-id="83ce4-166">Igen, kan du köra en princip för Redigera-profil på samma sätt:</span><span class="sxs-lookup"><span data-stu-id="83ce4-166">Again, you can execute an edit-profile policy in the same fashion:</span></span>

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

<span data-ttu-id="83ce4-167">I alla dessa fall returnerar MSAL antingen en token i `AuthenticationResult` eller genererar ett undantag.</span><span class="sxs-lookup"><span data-stu-id="83ce4-167">In all of these cases, MSAL either returns a token in `AuthenticationResult` or throws an exception.</span></span> <span data-ttu-id="83ce4-168">Varje gång du får en token från MSAL, som du kan använda den `AuthenticationResult.User` objektet uppdateras informationen i appen, till exempel Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="83ce4-168">Each time you get a token from MSAL, you can use the `AuthenticationResult.User` object to update the user data in the app, such as the UI.</span></span> <span data-ttu-id="83ce4-169">ADAL cachelagrar också token för användning i andra delar av programmet.</span><span class="sxs-lookup"><span data-stu-id="83ce4-169">ADAL also caches the token for use in other parts of the application.</span></span>

### <a name="check-for-tokens-on-app-start"></a><span data-ttu-id="83ce4-170">Sök efter token på programstart</span><span class="sxs-lookup"><span data-stu-id="83ce4-170">Check for tokens on app start</span></span>
<span data-ttu-id="83ce4-171">Du kan också använda MSAL för att hålla reda på användarens inloggning tillstånd.</span><span class="sxs-lookup"><span data-stu-id="83ce4-171">You can also use MSAL to keep track of the user's sign-in state.</span></span>  <span data-ttu-id="83ce4-172">I den här appen som vi vill användaren förblir inloggad även när de stänga appen och öppna den igen.</span><span class="sxs-lookup"><span data-stu-id="83ce4-172">In this app, we want the user to remain signed in even after they close the app & re-open it.</span></span>  <span data-ttu-id="83ce4-173">Tillbaka i den `OnInitialized` åsidosätta använder MSAL'S `AcquireTokenSilent` metod för att söka efter cachelagrade token:</span><span class="sxs-lookup"><span data-stu-id="83ce4-173">Back inside the `OnInitialized` override, use MSAL's `AcquireTokenSilent` method to check for cached tokens:</span></span>

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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
```

## <a name="call-the-task-api"></a><span data-ttu-id="83ce4-174">Anropa aktivitetens API</span><span class="sxs-lookup"><span data-stu-id="83ce4-174">Call the task API</span></span>
<span data-ttu-id="83ce4-175">Du har nu för MSAL köra principer och hämta token.</span><span class="sxs-lookup"><span data-stu-id="83ce4-175">You have now used MSAL to execute policies and get tokens.</span></span>  <span data-ttu-id="83ce4-176">När du vill använda en dessa token för att anropa aktivitetens API du igen kan använda MSAL'S `AcquireTokenSilent` metod för att söka efter cachelagrade token:</span><span class="sxs-lookup"><span data-stu-id="83ce4-176">When you want to use one these tokens to call the task API, you can again use MSAL's `AcquireTokenSilent` method to check for cached tokens:</span></span>

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

<span data-ttu-id="83ce4-177">När anropet till `AcquireTokenSilentAsync(...)` lyckas och en token har hittats i cacheminnet, kan du lägga till token för att den `Authorization` rubriken för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="83ce4-177">When the call to `AcquireTokenSilentAsync(...)` succeeds and a token is found in the cache, you can add the token to the `Authorization` header of the HTTP request.</span></span> <span data-ttu-id="83ce4-178">Uppgiften webb-API använder det här sidhuvudet för att autentisera begäran att läsa användarens att göra-lista:</span><span class="sxs-lookup"><span data-stu-id="83ce4-178">The task web API will use this header to authenticate the request to read the user's to-do list:</span></span>

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a><span data-ttu-id="83ce4-179">Logga ut användaren</span><span class="sxs-lookup"><span data-stu-id="83ce4-179">Sign the user out</span></span>
<span data-ttu-id="83ce4-180">Slutligen kan du använda MSAL för att avsluta en användarsession med appen när användaren väljer **logga ut**.</span><span class="sxs-lookup"><span data-stu-id="83ce4-180">Finally, you can use MSAL to end a user's session with the app when the user selects **Sign out**.</span></span>  <span data-ttu-id="83ce4-181">När du använder MSAL, kan detta åstadkommas genom att avmarkera alla token från token-cache:</span><span class="sxs-lookup"><span data-stu-id="83ce4-181">When using MSAL, this is accomplished by clearing all of the tokens from the token cache:</span></span>

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a><span data-ttu-id="83ce4-182">Kör exempelappen</span><span class="sxs-lookup"><span data-stu-id="83ce4-182">Run the sample app</span></span>
<span data-ttu-id="83ce4-183">Slutligen skapar och köra exemplet.</span><span class="sxs-lookup"><span data-stu-id="83ce4-183">Finally, build and run the sample.</span></span>  <span data-ttu-id="83ce4-184">Registrera dig för appen med hjälp av ett e-postadress eller användaren namn.</span><span class="sxs-lookup"><span data-stu-id="83ce4-184">Sign up for the app by using an email address or user name.</span></span> <span data-ttu-id="83ce4-185">Logga ut och logga in igen som samma användare.</span><span class="sxs-lookup"><span data-stu-id="83ce4-185">Sign out and sign back in as the same user.</span></span> <span data-ttu-id="83ce4-186">Redigera användarens profil.</span><span class="sxs-lookup"><span data-stu-id="83ce4-186">Edit that user's profile.</span></span> <span data-ttu-id="83ce4-187">Logga ut och logga med hjälp av en annan användare.</span><span class="sxs-lookup"><span data-stu-id="83ce4-187">Sign out and sign up by using a different user.</span></span>

## <a name="add-social-idps"></a><span data-ttu-id="83ce4-188">Lägg till sociala IDPs</span><span class="sxs-lookup"><span data-stu-id="83ce4-188">Add social IDPs</span></span>
<span data-ttu-id="83ce4-189">För närvarande appen stöder endast användare registrering och inloggning med **lokala konton**.</span><span class="sxs-lookup"><span data-stu-id="83ce4-189">Currently, the app supports only user sign-up and sign-in that use **local accounts**.</span></span> <span data-ttu-id="83ce4-190">Dessa är konton som lagras i din B2C-katalog som använder ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="83ce4-190">These are accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="83ce4-191">Med hjälp av Azure AD B2C kan du lägga till stöd för andra identitetsleverantörer (IDPs) utan att ändra någon av din kod.</span><span class="sxs-lookup"><span data-stu-id="83ce4-191">By using Azure AD B2C, you can add support for other identity providers (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="83ce4-192">Börja genom att följa de detaljerade anvisningarna i de här artiklarna om du vill lägga till sociala IDPs i appen.</span><span class="sxs-lookup"><span data-stu-id="83ce4-192">To add social IDPs to your app, begin by following the detailed instructions in these articles.</span></span> <span data-ttu-id="83ce4-193">För varje IDP som du vill använda, måste du registrera ett program i systemet och skaffa ett klient-ID.</span><span class="sxs-lookup"><span data-stu-id="83ce4-193">For each IDP you want to support, you need to register an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="83ce4-194">Konfigurera Facebook som en IDP</span><span class="sxs-lookup"><span data-stu-id="83ce4-194">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="83ce4-195">Konfigurera Google som en IDP</span><span class="sxs-lookup"><span data-stu-id="83ce4-195">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="83ce4-196">Ställa in Amazon som en IDP</span><span class="sxs-lookup"><span data-stu-id="83ce4-196">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="83ce4-197">Ställa in LinkedIn som en IDP</span><span class="sxs-lookup"><span data-stu-id="83ce4-197">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="83ce4-198">När du lägger till identitetsleverantörer din B2C-katalog, måste du redigera var och en av dina tre principer att inkludera de nya IDPs som beskrivs i den [referensartikeln om principer](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="83ce4-198">After you add the identity providers to your B2C directory, you need to edit each of your three policies to include the new IDPs, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="83ce4-199">Kör appen igen när du har sparat dina principer.</span><span class="sxs-lookup"><span data-stu-id="83ce4-199">After you save your policies, run the app again.</span></span> <span data-ttu-id="83ce4-200">Du bör se nya IDPs läggas till som inloggning och registreringsalternativ i varje din identitet inträffar.</span><span class="sxs-lookup"><span data-stu-id="83ce4-200">You should see the new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="83ce4-201">Du kan experimentera med dina principer och studera effekten på din exempelapp.</span><span class="sxs-lookup"><span data-stu-id="83ce4-201">You can experiment with your policies and observe the effects on your sample app.</span></span> <span data-ttu-id="83ce4-202">Lägg till eller ta bort IDPs, manipulera programanspråken eller ändra registreringsattribut.</span><span class="sxs-lookup"><span data-stu-id="83ce4-202">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="83ce4-203">Experimentera tills du kan se hur principer och autentiseringsbegäranden MSAL knyta ihop.</span><span class="sxs-lookup"><span data-stu-id="83ce4-203">Experiment until you can see how policies, authentication requests, and MSAL tie together.</span></span>

<span data-ttu-id="83ce4-204">För referens anger det slutförda exemplet [har angetts som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="83ce4-204">For reference, the completed sample [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="83ce4-205">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="83ce4-205">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```

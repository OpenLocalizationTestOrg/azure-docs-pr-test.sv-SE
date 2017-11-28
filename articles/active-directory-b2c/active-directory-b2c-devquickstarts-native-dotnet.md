---
title: aaaAzure Active Directory B2C | Microsoft Docs
description: "Hur toobuild ett Windows-skrivbordsprogram som inkluderar inloggning, registrering och profilen hantering med hjälp av Azure Active Directory B2C."
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
ms.openlocfilehash: f22b0299ff74bfba2f3fea88f006da609859dda5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a><span data-ttu-id="d22e8-103">Azure AD B2C: Skapa en Windows-skrivbordsapp</span><span class="sxs-lookup"><span data-stu-id="d22e8-103">Azure AD B2C: Build a Windows desktop app</span></span>
<span data-ttu-id="d22e8-104">Med hjälp av Azure Active Directory (AD Azure) B2C kan du lägga till kraftfulla självbetjäning identity management tooyour-skrivbordsapp med funktioner i några korta steg.</span><span class="sxs-lookup"><span data-stu-id="d22e8-104">By using Azure Active Directory (Azure AD) B2C, you can add powerful self-service identity management features tooyour desktop app in a few short steps.</span></span> <span data-ttu-id="d22e8-105">Den här artikeln visar hur toocreate en .NET Windows Presentation Foundation (WPF) ”uppgiftslistan”-app med användarregistrering, inloggning och profilhantering.</span><span class="sxs-lookup"><span data-stu-id="d22e8-105">This article will show you how toocreate a .NET Windows Presentation Foundation (WPF) "to-do list" app that includes user sign-up, sign-in, and profile management.</span></span> <span data-ttu-id="d22e8-106">hello app innehåller stöd för registrering och inloggning med ett användarnamn eller e-post.</span><span class="sxs-lookup"><span data-stu-id="d22e8-106">hello app will include support for sign-up and sign-in by using a user name or email.</span></span> <span data-ttu-id="d22e8-107">Den omfattar också stöd för registrering och inloggning med hjälp av sociala konton, till exempel Facebook och Google.</span><span class="sxs-lookup"><span data-stu-id="d22e8-107">It will also include support for sign-up and sign-in by using social accounts such as Facebook and Google.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="d22e8-108">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="d22e8-108">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="d22e8-109">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="d22e8-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="d22e8-110">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="d22e8-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="d22e8-111">Om du inte redan har en B2C-katalog [skapar du en](active-directory-b2c-get-started.md) innan du fortsätter den här guiden.</span><span class="sxs-lookup"><span data-stu-id="d22e8-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="d22e8-112">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="d22e8-112">Create an application</span></span>
<span data-ttu-id="d22e8-113">Sedan måste toocreate en app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="d22e8-113">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="d22e8-114">Det ger Azure AD den information som den behöver toosecurely kommunicera med din app.</span><span class="sxs-lookup"><span data-stu-id="d22e8-114">This gives Azure AD information that it needs toosecurely communicate with your app.</span></span> <span data-ttu-id="d22e8-115">toocreate en app, Följ [instruktionerna](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="d22e8-115">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span>  <span data-ttu-id="d22e8-116">Se till att:</span><span class="sxs-lookup"><span data-stu-id="d22e8-116">Be sure to:</span></span>

* <span data-ttu-id="d22e8-117">Inkludera en **native client** i hello program.</span><span class="sxs-lookup"><span data-stu-id="d22e8-117">Include a **native client** in hello application.</span></span>
* <span data-ttu-id="d22e8-118">Kopiera hello **omdirigerings-URI** `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="d22e8-118">Copy hello **Redirect URI** `urn:ietf:wg:oauth:2.0:oob`.</span></span> <span data-ttu-id="d22e8-119">Det är hello standard-URL för det här kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="d22e8-119">It is hello default URL for this code sample.</span></span>
* <span data-ttu-id="d22e8-120">Kopiera hello **program-ID** som är tilldelade tooyour app.</span><span class="sxs-lookup"><span data-stu-id="d22e8-120">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="d22e8-121">Du behöver den senare.</span><span class="sxs-lookup"><span data-stu-id="d22e8-121">You will need it later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="d22e8-122">Skapa dina principer</span><span class="sxs-lookup"><span data-stu-id="d22e8-122">Create your policies</span></span>
<span data-ttu-id="d22e8-123">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="d22e8-123">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="d22e8-124">Det här kodexemplet innehåller tre identitetsupplevelser: registrering, inloggning och profilredigering.</span><span class="sxs-lookup"><span data-stu-id="d22e8-124">This code sample contains three identity experiences: sign up, sign in, and edit profile.</span></span> <span data-ttu-id="d22e8-125">Du behöver toocreate en princip för varje typ, enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="d22e8-125">You need toocreate a policy for each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="d22e8-126">Tänk på att när du skapar hello tre principer:</span><span class="sxs-lookup"><span data-stu-id="d22e8-126">When you create hello three policies, be sure to:</span></span>

* <span data-ttu-id="d22e8-127">Välj antingen **användar-ID anmälan** eller **e-postadress** i hello Identitetsproviders.</span><span class="sxs-lookup"><span data-stu-id="d22e8-127">Choose either **User ID sign-up** or **Email sign-up** in hello identity providers blade.</span></span>
* <span data-ttu-id="d22e8-128">Välj **Visningsnamn** och andra registreringsattribut i registreringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="d22e8-128">Choose **Display name** and other sign-up attributes in your sign-up policy.</span></span>
* <span data-ttu-id="d22e8-129">Välj **Visningsnamn** och **Objekt-ID** som programanspråk för varje princip.</span><span class="sxs-lookup"><span data-stu-id="d22e8-129">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="d22e8-130">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="d22e8-130">You can choose other claims as well.</span></span>
* <span data-ttu-id="d22e8-131">Kopiera hello **namn** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="d22e8-131">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="d22e8-132">Det bör ha hello prefixet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="d22e8-132">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="d22e8-133">Du behöver principnamnen senare.</span><span class="sxs-lookup"><span data-stu-id="d22e8-133">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="d22e8-134">När du har skapat hello tre principer, du är klar toobuild din app.</span><span class="sxs-lookup"><span data-stu-id="d22e8-134">After you have successfully created hello three policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-code"></a><span data-ttu-id="d22e8-135">Hämta hello koden</span><span class="sxs-lookup"><span data-stu-id="d22e8-135">Download hello code</span></span>
<span data-ttu-id="d22e8-136">Hej koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span><span class="sxs-lookup"><span data-stu-id="d22e8-136">hello code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet).</span></span> <span data-ttu-id="d22e8-137">toobuild hello exempel som du går du [ladda ned stommen av ett projekt som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="d22e8-137">toobuild hello sample as you go, you can [download a skeleton project as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip).</span></span> <span data-ttu-id="d22e8-138">Du kan också klona stommen hello:</span><span class="sxs-lookup"><span data-stu-id="d22e8-138">You can also clone hello skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

<span data-ttu-id="d22e8-139">hello slutförts appen finns också [som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) eller på hello `complete` grenen av hello samma databas.</span><span class="sxs-lookup"><span data-stu-id="d22e8-139">hello completed app is also [available as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) or on hello `complete` branch of hello same repository.</span></span>

<span data-ttu-id="d22e8-140">När du har hämtat hello exempelkod igång öppna hello Visual Studio SLN-filen tooget.</span><span class="sxs-lookup"><span data-stu-id="d22e8-140">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="d22e8-141">Hej `TaskClient` projektet är hello WPF skrivbordsprogram som hello användaren interagerar med.</span><span class="sxs-lookup"><span data-stu-id="d22e8-141">hello `TaskClient` project is hello WPF desktop application that hello user interacts with.</span></span> <span data-ttu-id="d22e8-142">Hello enligt den här självstudiekursen anropar en aktivitet för backend-webb-API, finns i Azure som lagrar varje användares att göra-lista.</span><span class="sxs-lookup"><span data-stu-id="d22e8-142">For hello purposes of this tutorial, it calls a back-end task web API, hosted in Azure, that stores each user's to-do list.</span></span>  <span data-ttu-id="d22e8-143">Du behöver inte toobuild hello webb-API, vi har redan det kör du.</span><span class="sxs-lookup"><span data-stu-id="d22e8-143">You do not need toobuild hello web API, we already have it running for you.</span></span>

<span data-ttu-id="d22e8-144">toolearn hur ett webb-API autentiserar på ett säkert sätt begäranden med hjälp av Azure AD B2C checka ut den [webb-API komma igång artikel](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="d22e8-144">toolearn how a web API securely authenticates requests by using Azure AD B2C, check out the [web API getting started article](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

## <a name="execute-policies"></a><span data-ttu-id="d22e8-145">Köra principer</span><span class="sxs-lookup"><span data-stu-id="d22e8-145">Execute policies</span></span>
<span data-ttu-id="d22e8-146">Appen kommunicerar med Azure AD B2C genom att skicka meddelandena som anger hello-princip som de vill tooexecute som en del av hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="d22e8-146">Your app communicates with Azure AD B2C by sending authentication messages that specify hello policy they want tooexecute as part of hello HTTP request.</span></span> <span data-ttu-id="d22e8-147">För .NET-program, kan du använda hello Förhandsgranska Microsoft Authentication Library (MSAL) toosend OAuth 2.0-autentiseringsmeddelanden, köra principer och hämta token att anropet web API: er.</span><span class="sxs-lookup"><span data-stu-id="d22e8-147">For .NET desktop applications, you can use hello preview Microsoft Authentication Library (MSAL) toosend OAuth 2.0 authentication messages, execute policies, and get tokens that call web APIs.</span></span>

### <a name="install-msal"></a><span data-ttu-id="d22e8-148">Installera MSAL</span><span class="sxs-lookup"><span data-stu-id="d22e8-148">Install MSAL</span></span>
<span data-ttu-id="d22e8-149">Lägg till MSAL toohello `TaskClient` projekt genom att använda hello Visual Studio Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="d22e8-149">Add MSAL toohello `TaskClient` project by using hello Visual Studio Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a><span data-ttu-id="d22e8-150">Ange din B2C-information</span><span class="sxs-lookup"><span data-stu-id="d22e8-150">Enter your B2C details</span></span>
<span data-ttu-id="d22e8-151">Öppna hello filen `Globals.cs` och Ersätt hello egenskapen värden med din egen.</span><span class="sxs-lookup"><span data-stu-id="d22e8-151">Open hello file `Globals.cs` and replace each of hello property values with your own.</span></span> <span data-ttu-id="d22e8-152">Den här klassen används i hela `TaskClient` tooreference används vanligtvis för värden.</span><span class="sxs-lookup"><span data-stu-id="d22e8-152">This class is used throughout `TaskClient` tooreference commonly used values.</span></span>

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

### <a name="create-hello-publicclientapplication"></a><span data-ttu-id="d22e8-153">Skapa hello PublicClientApplication</span><span class="sxs-lookup"><span data-stu-id="d22e8-153">Create hello PublicClientApplication</span></span>
<span data-ttu-id="d22e8-154">hello primära klassen för MSAL är `PublicClientApplication`.</span><span class="sxs-lookup"><span data-stu-id="d22e8-154">hello primary class of MSAL is `PublicClientApplication`.</span></span> <span data-ttu-id="d22e8-155">Den här klassen representerar ditt program i hello Azure AD B2C-system.</span><span class="sxs-lookup"><span data-stu-id="d22e8-155">This class represents your application in hello Azure AD B2C system.</span></span> <span data-ttu-id="d22e8-156">När hello app initalizes skapar en instans av `PublicClientApplication` i `MainWindow.xaml.cs`.</span><span class="sxs-lookup"><span data-stu-id="d22e8-156">When hello app initalizes, create an instance of `PublicClientApplication` in `MainWindow.xaml.cs`.</span></span> <span data-ttu-id="d22e8-157">Detta kan användas i hela hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="d22e8-157">This can be used throughout hello window.</span></span>

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens toopersist when hello user closes hello app,
        // we've extended hello MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### <a name="initiate-a-sign-up-flow"></a><span data-ttu-id="d22e8-158">Initiera registrering flöde</span><span class="sxs-lookup"><span data-stu-id="d22e8-158">Initiate a sign-up flow</span></span>
<span data-ttu-id="d22e8-159">När en användare väljer toosigns in, vill du tooinitiate ett anmälan flöde som använder hello registreringsprincipen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="d22e8-159">When a user opts toosigns up, you want tooinitiate a sign-up flow that uses hello sign-up policy you created.</span></span> <span data-ttu-id="d22e8-160">Genom att använda MSAL kan du bara anropa `pca.AcquireTokenAsync(...)`.</span><span class="sxs-lookup"><span data-stu-id="d22e8-160">By using MSAL, you just call `pca.AcquireTokenAsync(...)`.</span></span> <span data-ttu-id="d22e8-161">Hej parametrar som du skickar för`AcquireTokenAsync(...)` avgör vilka token felmeddelandet, hello princip som ska användas i hello autentiseringsbegäran och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="d22e8-161">hello parameters you pass too`AcquireTokenAsync(...)` determine which token you receive, hello policy used in hello authentication request, and more.</span></span>

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use hello app's clientId here as hello scope parameter, indicating that
        // you want a token toohello your app's backend web API (represented by
        // hello cloud hosted task API).  Use hello UiOptions.ForceLogin flag to
        // indicate tooMSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in hello app that hello user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When hello request completes successfully, you can get user
        // information from hello AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After hello sign up successfully completes, display hello user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of hello policy.
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

### <a name="initiate-a-sign-in-flow"></a><span data-ttu-id="d22e8-162">Initiera ett flöde för inloggning</span><span class="sxs-lookup"><span data-stu-id="d22e8-162">Initiate a sign-in flow</span></span>
<span data-ttu-id="d22e8-163">Du kan starta ett flöde i hello samma sätt som du startar en anmälan flöde.</span><span class="sxs-lookup"><span data-stu-id="d22e8-163">You can initiate a sign-in flow in hello same way that you initiate a sign-up flow.</span></span> <span data-ttu-id="d22e8-164">När en användare loggar in, se hello samma anropa tooMSAL, nu med hjälp av din princip för inloggning:</span><span class="sxs-lookup"><span data-stu-id="d22e8-164">When a user signs in, make hello same call tooMSAL, this time by using your sign-in policy:</span></span>

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

### <a name="initiate-an-edit-profile-flow"></a><span data-ttu-id="d22e8-165">Initiera ett flöde för Redigera-profil</span><span class="sxs-lookup"><span data-stu-id="d22e8-165">Initiate an edit-profile flow</span></span>
<span data-ttu-id="d22e8-166">Igen, kan du köra en princip för Redigera-profil i hello samma sätt:</span><span class="sxs-lookup"><span data-stu-id="d22e8-166">Again, you can execute an edit-profile policy in hello same fashion:</span></span>

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

<span data-ttu-id="d22e8-167">I alla dessa fall returnerar MSAL antingen en token i `AuthenticationResult` eller genererar ett undantag.</span><span class="sxs-lookup"><span data-stu-id="d22e8-167">In all of these cases, MSAL either returns a token in `AuthenticationResult` or throws an exception.</span></span> <span data-ttu-id="d22e8-168">Varje gång du får en token från MSAL, kan du använda hello `AuthenticationResult.User` objekt tooupdate hello användardata i hello appen, till exempel hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d22e8-168">Each time you get a token from MSAL, you can use hello `AuthenticationResult.User` object tooupdate hello user data in hello app, such as hello UI.</span></span> <span data-ttu-id="d22e8-169">ADAL även cacheminnen hello token till andra delar av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="d22e8-169">ADAL also caches hello token for use in other parts of hello application.</span></span>

### <a name="check-for-tokens-on-app-start"></a><span data-ttu-id="d22e8-170">Sök efter token på programstart</span><span class="sxs-lookup"><span data-stu-id="d22e8-170">Check for tokens on app start</span></span>
<span data-ttu-id="d22e8-171">Du kan också använda MSAL tookeep reda på hello användarens inloggning tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d22e8-171">You can also use MSAL tookeep track of hello user's sign-in state.</span></span>  <span data-ttu-id="d22e8-172">I den här appen som vi vill hello användaren tooremain är inloggad när de Stäng hello app och öppna den igen.</span><span class="sxs-lookup"><span data-stu-id="d22e8-172">In this app, we want hello user tooremain signed in even after they close hello app & re-open it.</span></span>  <span data-ttu-id="d22e8-173">Tillbaka i hello `OnInitialized` åsidosätta använder MSAL'S `AcquireTokenSilent` metoden toocheck för cachelagrade token:</span><span class="sxs-lookup"><span data-stu-id="d22e8-173">Back inside hello `OnInitialized` override, use MSAL's `AcquireTokenSilent` method toocheck for cached tokens:</span></span>

```C#
AuthenticationResult result = null;
try
{
    // If hello user has has a token cached with any policy, we'll display them as signed-in.
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
        // There are no tokens in hello cache.  Proceed without calling hello tooDo list service.
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

## <a name="call-hello-task-api"></a><span data-ttu-id="d22e8-174">Anropa API för hello-aktivitet</span><span class="sxs-lookup"><span data-stu-id="d22e8-174">Call hello task API</span></span>
<span data-ttu-id="d22e8-175">Du har nu använt MSAL tooexecute principer och hämta token.</span><span class="sxs-lookup"><span data-stu-id="d22e8-175">You have now used MSAL tooexecute policies and get tokens.</span></span>  <span data-ttu-id="d22e8-176">Om du vill toouse en dessa token toocall hello uppgiften API kan du igen kan använda MSAL'S `AcquireTokenSilent` metoden toocheck för cachelagrade token:</span><span class="sxs-lookup"><span data-stu-id="d22e8-176">When you want toouse one these tokens toocall hello task API, you can again use MSAL's `AcquireTokenSilent` method toocheck for cached tokens:</span></span>

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want toocheck for a cached token, independent of whatever policy was used tooacquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent tooindicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch hello exception and show hello user a message.
    catch (MsalException ex)
    {
        // There is no access token in hello cache, so prompt hello user toosign-in.
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

<span data-ttu-id="d22e8-177">När hello anrop för`AcquireTokenSilentAsync(...)` lyckas och en token har hittats i hello cache, kan du lägga till hello token toohello `Authorization` huvudet i hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="d22e8-177">When hello call too`AcquireTokenSilentAsync(...)` succeeds and a token is found in hello cache, you can add hello token toohello `Authorization` header of hello HTTP request.</span></span> <span data-ttu-id="d22e8-178">hello uppgiften webb-API använder den här rubriken tooauthenticate hello begäran tooread hello användarens att göra-lista:</span><span class="sxs-lookup"><span data-stu-id="d22e8-178">hello task web API will use this header tooauthenticate hello request tooread hello user's to-do list:</span></span>

```C#
    ...
    // Once hello token has been returned by MSAL, add it toohello http authorization header, before making hello call tooaccess hello tooDo list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call hello tooDo list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-hello-user-out"></a><span data-ttu-id="d22e8-179">Logga hello användaren ut</span><span class="sxs-lookup"><span data-stu-id="d22e8-179">Sign hello user out</span></span>
<span data-ttu-id="d22e8-180">Slutligen kan du använda MSAL tooend en användares session med hello app när hello användare väljer **logga ut**.  När du använder MSAL, kan detta åstadkommas genom att avmarkera alla hello-token från hello token-cache:</span><span class="sxs-lookup"><span data-stu-id="d22e8-180">Finally, you can use MSAL tooend a user's session with hello app when hello user selects **Sign out**.  When using MSAL, this is accomplished by clearing all of hello tokens from hello token cache:</span></span>

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of hello user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in hello browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update hello UI tooshow hello user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-hello-sample-app"></a><span data-ttu-id="d22e8-181">Kör hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="d22e8-181">Run hello sample app</span></span>
<span data-ttu-id="d22e8-182">Slutligen skapar och kör hello exempel.</span><span class="sxs-lookup"><span data-stu-id="d22e8-182">Finally, build and run hello sample.</span></span>  <span data-ttu-id="d22e8-183">Registrera dig för hello appen med hjälp av ett e-postadress eller användaren namn.</span><span class="sxs-lookup"><span data-stu-id="d22e8-183">Sign up for hello app by using an email address or user name.</span></span> <span data-ttu-id="d22e8-184">Logga ut och logga in som hello samma användare.</span><span class="sxs-lookup"><span data-stu-id="d22e8-184">Sign out and sign back in as hello same user.</span></span> <span data-ttu-id="d22e8-185">Redigera användarens profil.</span><span class="sxs-lookup"><span data-stu-id="d22e8-185">Edit that user's profile.</span></span> <span data-ttu-id="d22e8-186">Logga ut och logga med hjälp av en annan användare.</span><span class="sxs-lookup"><span data-stu-id="d22e8-186">Sign out and sign up by using a different user.</span></span>

## <a name="add-social-idps"></a><span data-ttu-id="d22e8-187">Lägg till sociala IDPs</span><span class="sxs-lookup"><span data-stu-id="d22e8-187">Add social IDPs</span></span>
<span data-ttu-id="d22e8-188">För närvarande hello app stöder endast användare registrering och inloggning som använder **lokala konton**.</span><span class="sxs-lookup"><span data-stu-id="d22e8-188">Currently, hello app supports only user sign-up and sign-in that use **local accounts**.</span></span> <span data-ttu-id="d22e8-189">Dessa är konton som lagras i din B2C-katalog som använder ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d22e8-189">These are accounts stored in your B2C directory that use a user name and password.</span></span> <span data-ttu-id="d22e8-190">Med hjälp av Azure AD B2C kan du lägga till stöd för andra identitetsleverantörer (IDPs) utan att ändra någon av din kod.</span><span class="sxs-lookup"><span data-stu-id="d22e8-190">By using Azure AD B2C, you can add support for other identity providers (IDPs) without changing any of your code.</span></span>

<span data-ttu-id="d22e8-191">tooadd sociala IDPs tooyour appen, börja med att följa hello detaljerade instruktioner i dessa artiklar.</span><span class="sxs-lookup"><span data-stu-id="d22e8-191">tooadd social IDPs tooyour app, begin by following hello detailed instructions in these articles.</span></span> <span data-ttu-id="d22e8-192">För varje IDP du vill toosupport, du behöver tooregister ett program i systemet och hämta ett klient-ID.</span><span class="sxs-lookup"><span data-stu-id="d22e8-192">For each IDP you want toosupport, you need tooregister an application in that system and obtain a client ID.</span></span>

* [<span data-ttu-id="d22e8-193">Konfigurera Facebook som en IDP</span><span class="sxs-lookup"><span data-stu-id="d22e8-193">Set up Facebook as an IDP</span></span>](active-directory-b2c-setup-fb-app.md)
* [<span data-ttu-id="d22e8-194">Konfigurera Google som en IDP</span><span class="sxs-lookup"><span data-stu-id="d22e8-194">Set up Google as an IDP</span></span>](active-directory-b2c-setup-goog-app.md)
* [<span data-ttu-id="d22e8-195">Ställa in Amazon som en IDP</span><span class="sxs-lookup"><span data-stu-id="d22e8-195">Set up Amazon as an IDP</span></span>](active-directory-b2c-setup-amzn-app.md)
* [<span data-ttu-id="d22e8-196">Ställa in LinkedIn som en IDP</span><span class="sxs-lookup"><span data-stu-id="d22e8-196">Set up LinkedIn as an IDP</span></span>](active-directory-b2c-setup-li-app.md)

<span data-ttu-id="d22e8-197">När du lägger till hello identitet providers tooyour B2C-katalog måste tooedit var och en av dina tre principer tooinclude hello nya IDPs som beskrivs i hello [referensartikeln om principer](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="d22e8-197">After you add hello identity providers tooyour B2C directory, you need tooedit each of your three policies tooinclude hello new IDPs, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="d22e8-198">När du har sparat dina principer, kör du hello appen igen.</span><span class="sxs-lookup"><span data-stu-id="d22e8-198">After you save your policies, run hello app again.</span></span> <span data-ttu-id="d22e8-199">Du bör se hello nya IDPs läggas till som alternativ för inloggning och registreringen i var och en av dina identitetsupplevelser.</span><span class="sxs-lookup"><span data-stu-id="d22e8-199">You should see hello new IDPs added as sign-in and sign-up options in each of your identity experiences.</span></span>

<span data-ttu-id="d22e8-200">Du kan experimentera med dina principer och studera hello inverkan på din exempelapp.</span><span class="sxs-lookup"><span data-stu-id="d22e8-200">You can experiment with your policies and observe hello effects on your sample app.</span></span> <span data-ttu-id="d22e8-201">Lägg till eller ta bort IDPs, manipulera programanspråken eller ändra registreringsattribut.</span><span class="sxs-lookup"><span data-stu-id="d22e8-201">Add or remove IDPs, manipulate application claims, or change sign-up attributes.</span></span> <span data-ttu-id="d22e8-202">Experimentera tills du kan se hur principer och autentiseringsbegäranden MSAL knyta ihop.</span><span class="sxs-lookup"><span data-stu-id="d22e8-202">Experiment until you can see how policies, authentication requests, and MSAL tie together.</span></span>

<span data-ttu-id="d22e8-203">För referens anger hello slutförts exempel [har angetts som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="d22e8-203">For reference, hello completed sample [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="d22e8-204">Du kan också klona det från GitHub:</span><span class="sxs-lookup"><span data-stu-id="d22e8-204">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```

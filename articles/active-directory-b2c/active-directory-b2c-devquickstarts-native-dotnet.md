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
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Skapa en Windows-skrivbordsapp
Med hjälp av Azure Active Directory (AD Azure) B2C kan du lägga till kraftfulla självbetjäning identity management tooyour-skrivbordsapp med funktioner i några korta steg. Den här artikeln visar hur toocreate en .NET Windows Presentation Foundation (WPF) ”uppgiftslistan”-app med användarregistrering, inloggning och profilhantering. hello app innehåller stöd för registrering och inloggning med ett användarnamn eller e-post. Den omfattar också stöd för registrering och inloggning med hjälp av sociala konton, till exempel Facebook och Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Skaffa en Azure AD B2C-katalog
Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.  En katalog är en behållare för alla användare, appar, grupper och mer. Om du inte redan har en B2C-katalog [skapar du en](active-directory-b2c-get-started.md) innan du fortsätter den här guiden.

## <a name="create-an-application"></a>Skapa ett program
Sedan måste toocreate en app i B2C-katalogen. Det ger Azure AD den information som den behöver toosecurely kommunicera med din app. toocreate en app, Följ [instruktionerna](active-directory-b2c-app-registration.md).  Se till att:

* Inkludera en **native client** i hello program.
* Kopiera hello **omdirigerings-URI** `urn:ietf:wg:oauth:2.0:oob`. Det är hello standard-URL för det här kodexemplet.
* Kopiera hello **program-ID** som är tilldelade tooyour app. Du behöver den senare.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Skapa dina principer
I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md). Det här kodexemplet innehåller tre identitetsupplevelser: registrering, inloggning och profilredigering. Du behöver toocreate en princip för varje typ, enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Tänk på att när du skapar hello tre principer:

* Välj antingen **användar-ID anmälan** eller **e-postadress** i hello Identitetsproviders.
* Välj **Visningsnamn** och andra registreringsattribut i registreringsprincipen.
* Välj **Visningsnamn** och **Objekt-ID** som programanspråk för varje princip. Du kan också välja andra anspråk.
* Kopiera hello **namn** på varje princip när du har skapat den. Det bör ha hello prefixet `b2c_1_`.  Du behöver principnamnen senare.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

När du har skapat hello tre principer, du är klar toobuild din app.

## <a name="download-hello-code"></a>Hämta hello koden
Hej koden för den här självstudiekursen [finns på GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). toobuild hello exempel som du går du [ladda ned stommen av ett projekt som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Du kan också klona stommen hello:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

hello slutförts appen finns också [som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) eller på hello `complete` grenen av hello samma databas.

När du har hämtat hello exempelkod igång öppna hello Visual Studio SLN-filen tooget. Hej `TaskClient` projektet är hello WPF skrivbordsprogram som hello användaren interagerar med. Hello enligt den här självstudiekursen anropar en aktivitet för backend-webb-API, finns i Azure som lagrar varje användares att göra-lista.  Du behöver inte toobuild hello webb-API, vi har redan det kör du.

toolearn hur ett webb-API autentiserar på ett säkert sätt begäranden med hjälp av Azure AD B2C checka ut den [webb-API komma igång artikel](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Köra principer
Appen kommunicerar med Azure AD B2C genom att skicka meddelandena som anger hello-princip som de vill tooexecute som en del av hello HTTP-begäran. För .NET-program, kan du använda hello Förhandsgranska Microsoft Authentication Library (MSAL) toosend OAuth 2.0-autentiseringsmeddelanden, köra principer och hämta token att anropet web API: er.

### <a name="install-msal"></a>Installera MSAL
Lägg till MSAL toohello `TaskClient` projekt genom att använda hello Visual Studio Package Manager-konsolen.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Ange din B2C-information
Öppna hello filen `Globals.cs` och Ersätt hello egenskapen värden med din egen. Den här klassen används i hela `TaskClient` tooreference används vanligtvis för värden.

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

### <a name="create-hello-publicclientapplication"></a>Skapa hello PublicClientApplication
hello primära klassen för MSAL är `PublicClientApplication`. Den här klassen representerar ditt program i hello Azure AD B2C-system. När hello app initalizes skapar en instans av `PublicClientApplication` i `MainWindow.xaml.cs`. Detta kan användas i hela hello-fönstret.

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

### <a name="initiate-a-sign-up-flow"></a>Initiera registrering flöde
När en användare väljer toosigns in, vill du tooinitiate ett anmälan flöde som använder hello registreringsprincipen som du skapade. Genom att använda MSAL kan du bara anropa `pca.AcquireTokenAsync(...)`. Hej parametrar som du skickar för`AcquireTokenAsync(...)` avgör vilka token felmeddelandet, hello princip som ska användas i hello autentiseringsbegäran och mycket mer.

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

### <a name="initiate-a-sign-in-flow"></a>Initiera ett flöde för inloggning
Du kan starta ett flöde i hello samma sätt som du startar en anmälan flöde. När en användare loggar in, se hello samma anropa tooMSAL, nu med hjälp av din princip för inloggning:

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

### <a name="initiate-an-edit-profile-flow"></a>Initiera ett flöde för Redigera-profil
Igen, kan du köra en princip för Redigera-profil i hello samma sätt:

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

I alla dessa fall returnerar MSAL antingen en token i `AuthenticationResult` eller genererar ett undantag. Varje gång du får en token från MSAL, kan du använda hello `AuthenticationResult.User` objekt tooupdate hello användardata i hello appen, till exempel hello Användargränssnittet. ADAL även cacheminnen hello token till andra delar av programmet hello.

### <a name="check-for-tokens-on-app-start"></a>Sök efter token på programstart
Du kan också använda MSAL tookeep reda på hello användarens inloggning tillstånd.  I den här appen som vi vill hello användaren tooremain är inloggad när de Stäng hello app och öppna den igen.  Tillbaka i hello `OnInitialized` åsidosätta använder MSAL'S `AcquireTokenSilent` metoden toocheck för cachelagrade token:

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

## <a name="call-hello-task-api"></a>Anropa API för hello-aktivitet
Du har nu använt MSAL tooexecute principer och hämta token.  Om du vill toouse en dessa token toocall hello uppgiften API kan du igen kan använda MSAL'S `AcquireTokenSilent` metoden toocheck för cachelagrade token:

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

När hello anrop för`AcquireTokenSilentAsync(...)` lyckas och en token har hittats i hello cache, kan du lägga till hello token toohello `Authorization` huvudet i hello HTTP-begäran. hello uppgiften webb-API använder den här rubriken tooauthenticate hello begäran tooread hello användarens att göra-lista:

```C#
    ...
    // Once hello token has been returned by MSAL, add it toohello http authorization header, before making hello call tooaccess hello tooDo list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call hello tooDo list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-hello-user-out"></a>Logga hello användaren ut
Slutligen kan du använda MSAL tooend en användares session med hello app när hello användare väljer **logga ut**.  När du använder MSAL, kan detta åstadkommas genom att avmarkera alla hello-token från hello token-cache:

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

## <a name="run-hello-sample-app"></a>Kör hello sample-appen
Slutligen skapar och kör hello exempel.  Registrera dig för hello appen med hjälp av ett e-postadress eller användaren namn. Logga ut och logga in som hello samma användare. Redigera användarens profil. Logga ut och logga med hjälp av en annan användare.

## <a name="add-social-idps"></a>Lägg till sociala IDPs
För närvarande hello app stöder endast användare registrering och inloggning som använder **lokala konton**. Dessa är konton som lagras i din B2C-katalog som använder ett användarnamn och lösenord. Med hjälp av Azure AD B2C kan du lägga till stöd för andra identitetsleverantörer (IDPs) utan att ändra någon av din kod.

tooadd sociala IDPs tooyour appen, börja med att följa hello detaljerade instruktioner i dessa artiklar. För varje IDP du vill toosupport, du behöver tooregister ett program i systemet och hämta ett klient-ID.

* [Konfigurera Facebook som en IDP](active-directory-b2c-setup-fb-app.md)
* [Konfigurera Google som en IDP](active-directory-b2c-setup-goog-app.md)
* [Ställa in Amazon som en IDP](active-directory-b2c-setup-amzn-app.md)
* [Ställa in LinkedIn som en IDP](active-directory-b2c-setup-li-app.md)

När du lägger till hello identitet providers tooyour B2C-katalog måste tooedit var och en av dina tre principer tooinclude hello nya IDPs som beskrivs i hello [referensartikeln om principer](active-directory-b2c-reference-policies.md). När du har sparat dina principer, kör du hello appen igen. Du bör se hello nya IDPs läggas till som alternativ för inloggning och registreringen i var och en av dina identitetsupplevelser.

Du kan experimentera med dina principer och studera hello inverkan på din exempelapp. Lägg till eller ta bort IDPs, manipulera programanspråken eller ändra registreringsattribut. Experimentera tills du kan se hur principer och autentiseringsbegäranden MSAL knyta ihop.

För referens anger hello slutförts exempel [har angetts som en .zip-fil](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Du kan också klona det från GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```

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
# <a name="add-sign-in-tooa-windows-desktop-app"></a>Lägga till inloggning tooa Windows Desktop-appen
Hello hello v2.0-slutpunkten kan du snabbt lägga till autentisering tooyour skrivbordsappar med stöd för både personliga Microsoft-konton och arbets-eller skolkonton.  Den även kan din app toosecurely kommunicera med en serverdel webb-api, samt [hello Microsoft Graph](https://graph.microsoft.io) och några av hello [Office 365 Unified-API: er](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [!NOTE]
> Inte alla Azure Active Directory (AD)-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

För [.NET interna appar som körs på en enhet](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD innehåller hello Autentiseringsbibliotek för Microsoft Identity eller MSAL.  MSALS uteslutande i livslängd är det enkelt för din app tooget token för att anropa webbtjänster toomake.  toodemonstrate hur lätt det är här vi ska skapa en uppgiftslista för .NET WPF-app som:

* Loggar hello användare i & hämtar åtkomst-token som använder hello [OAuth 2.0-autentiseringsprotokollet](active-directory-v2-protocols.md).
* På ett säkert sätt anropar en serverdel uppgiftslista webbtjänsten, som också är skyddad av OAuth 2.0.
* Loggar hello användare ut.

## <a name="download-sample-code"></a>Hämta exempelkoden
hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) eller klona hello stommen:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

hello slutförts app tillhandahålls hello slutet av den här kursen samt.

## <a name="register-an-app"></a>Registrera en app
Skapa en ny app på [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).  Se till att:

* Kopiera hello **program-Id** tilldelade tooyour app måste den snart.
* Lägg till hello **Mobile** plattform för din app.

## <a name="install--configure-msal"></a>Installera och konfigurera MSAL
Du kan installera MSAL och Skriv koden identitetsrelaterade nu när du har en app som registrerats med Microsoft.  För att MSAL toobe kan toocommunicate hello v2.0-slutpunkten måste tooprovide med viss information om din appregistrering.

* Börja genom att lägga till MSAL toohello TodoListClient projektet med hello Package Manager-konsolen.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* Öppna i hello TodoListClient project `app.config`.  Ersätt hello värdena för hello element i hello `<appSettings>` avsnittet tooreflect hello värden indata i portalen för registrering av hello-app.  Koden ska referera till dessa värden när den används i MSAL.
  
  * Hej `ida:ClientId` är hello **program-Id** för din app som du kopierade från hello-portalen.
* I hello TodoList-Service-projekt, öppna `web.config` i hello rot hello-projekt.  
  
  * Ersätt hello `ida:Audience` värdet med hello samma **program-Id** från hello-portalen.

## <a name="use-msal-tooget-tokens"></a>Använd MSAL tooget token
hello grundläggande principen bakom MSAL är att när en app måste en åtkomst-token, ringer du bara `app.AcquireToken(...)`, och MSAL hello rest.  

* I hello `TodoListClient` projektet öppnar `MainWindow.xaml.cs` och leta upp hello `OnInitialized(...)` metod.  hello första steget är tooinitialize appens `PublicClientApplication` -MSALS primära klass som representerar interna program.  Det är där du skickar MSAL hello koordinater måste toocommunicate med Azure AD och anger hur toocache token.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* När hello app startar vi vill toocheck och se om hello användaren redan har loggat in hello app.  Men vi ännu vill inte tooinvoke en inloggning UI - vi kommer att hello användaren klicka på ”Logga In” toodo så.  Även i hello `OnInitialized(...)` metoden:

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

* Om hello användaren inte är inloggad och de klickar på knappen ”Logga In” för hello, vi vill tooinvoke inloggningen UI och hello användare anger sina autentiseringsuppgifter.  Implementera hello inloggning knappen hanterare:

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

* Om hello användaren har loggar in MSAL ska ta emot och cachelagra en token för dig och du kan fortsätta toocall hello `GetTodoList()` metod med förtroende.  Alla som har lämnat tooget en användares uppgifter är tooimplement hello `GetTodoList()` metod.

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

## <a name="run"></a>Kör
Grattis! Du har nu en fungerande .NET WPF-program som har hello möjlighet tooauthenticate användare & på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0.  Köra båda projekten och logga in med ett personligt microsoftkonto eller ett arbets- eller skolkonto konto.  Lägga till uppgifter toothat användarens att göra-lista.  Logga ut och logga in igen som en annan användare tooview sina att göra-lista.  Stäng hello app och kör den igen.  Observera hur hello användarens session förblir intakta, som beror på att hello app cachelagrar token i en lokal fil.

MSAL gör det enkelt tooincorporate vanliga identity-funktioner i din app använder både personliga och arbetsrelaterade konton.  Det hand tar om alla hello ändrad arbete du - hantering av cache, OAuth protokollstöd presentera hello användare med en inloggning UI, uppdatera token har upphört att gälla och mycket mer.  Allt du verkligen behöver tooknow är ett enda API-anrop `app.AcquireTokenAsync(...)`.

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), eller kan du klona den från GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Nästa steg
Du kan nu gå vidare till mer avancerade avsnitt.  Du kanske vill tootry:

* [Att säkra hello TodoListService Web API med hello v2.0-slutpunkten](active-directory-v2-devquickstarts-dotnet-api.md)

För ytterligare resurser, kolla:  

* [Utvecklarhandbok för hello v2.0 >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow ”msal” taggen >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Hämta säkerhetsuppdateringar för våra produkter
Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.


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
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Integrera Azure AD i en Windows-skrivbord WPF-program
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Om du utvecklar ett skrivbordsprogram gör Azure AD det lätt för tooauthenticate du dina användare med deras Active Directory-konton.  Program-toosecurely kan också använda en webb-API som skyddas av Azure AD, som hello Office 365-API: er eller hello Azure API.

Azure AD innehåller hello Active Directory Authentication Library eller ADAL för .NET interna klienter som behöver tooaccess skyddade resurser.  ADALS enda syfte i livslängd är toomake det enkelt för din app tooget åtkomst-token.  toodemonstrate hur lätt det är här vi ska skapa ett .NET uppgiftslista för WPF-program som:

* Hämtar åtkomst till token för att anropa hello Azure AD Graph API: et med hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Söker en katalog för användare med ett visst alias.
* Användare loggar ut.

toobuild hello fullständig fungerande program, måste du:

1. Registrera ditt program med Azure AD.
2. Installera och konfigurera ADAL.
3. Använd ADAL tooget token från Azure AD.

tooget har startats [hämta hello app stommen](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program.  Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).

## <a name="1-register-hello-directorysearcher-application"></a>1. Registrera hello DirectorySearcher program
tooenable tooget token för din app behöver du tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello översta, klicka på fältet för ditt konto och på hello **Directory** Välj plats tooregister programmet hello Active Directory-klient.
3. Klicka på **fler tjänster** i hello vänstra nav och välj **Azure Active Directory**.
4. Klicka på **App registreringar** och välj **Lägg till**.
5. Följ anvisningarna för hello och skapa en ny **internt klientprogram**.
  * Hej **namn** av hello programmet beskriver dina program tooend-användare
  * Hej **omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD ska använda tooreturn token svar.  Ange ett värde specifika tooyour program, t.ex. `http://DirectorySearcher`.
6. När du har slutfört registreringen tilldelas AAD appen ett unikt program-ID.  Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello appen på sidan.
7. Från hello **inställningar** väljer **nödvändiga behörigheter** och välj **Lägg till**. Välj hello **Microsoft Graph** som hello API och Lägg till hello **läsa katalogdata** behörighet under **delegerade behörigheter**.  Detta gör att dina program tooquery hello Graph API för användare.

## <a name="2-install--configure-adal"></a>2. Installera och konfigurera ADAL
Nu när du har ett program i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.  ADAL toobe kan toocommunicate med Azure AD, du måste för tooprovide med viss information om din appregistrering.

* Börja genom att lägga till ADAL toohello DirectorySearcher projektet med hello Package Manager-konsolen.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* Öppna i hello DirectorySearcher project `app.config`.  Ersätt hello värdena för hello element i hello `<appSettings>` avsnittet tooreflect hello värden indata i hello Azure-portalen.  Koden ska referera till dessa värden när den använder ADAL.
  * Hej `ida:Tenant` är hello domän för din Azure AD-klient, t.ex. contoso.onmicrosoft.com
  * Hej `ida:ClientId` är hello clientId på ditt program som du kopierade från hello-portalen.
  * Hej `ida:RedirectUri` är hello omdirigerings-url som du har registrerat i hello-portalen.

## <a name="3----use-adal-tooget-tokens-from-aad"></a>3.    Använd ADAL tooGet token från AAD
hello grundläggande principen bakom ADAL är att när din app behöver en åtkomst-token kan bara anropas `authContext.AcquireTokenAsync(...)`, och ADAL hello rest.  

* I hello `DirectorySearcher` projektet öppnar `MainWindow.xaml.cs` och leta upp hello `MainWindow()` metod.  hello första steget är tooinitialize appens `AuthenticationContext` -ADAL vars primära klassen.  Det är där du skickar ADAL hello koordinater måste toocommunicate med Azure AD och anger hur toocache token.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* Hitta nu hello `Search(...)` metod som kommer att anropas när hello användaren cliks hello ”Sök”-knappen i hello appens användargränssnitt.  Den här metoden gör en GET-begäran toohello Azure AD Graph API tooquery för användare vars UPN-namnet börjar med hello angivna sökord.  Men i ordning tooquery hello Graph API, behöver du tooinclude en access_token i hello `Authorization` rubriken för hello begäran - detta är där ADAL kommer in.

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
* När din app begär en token genom att anropa `AcquireTokenAsync(...)`, ADAL försöker tooreturn en token utan att uppge autentiseringsuppgifter hello användare.  Om ADAL avgör hello användaren måste toosign i tooget en token, visas en dialogruta med en inloggning, samla in hello användarens autentiseringsuppgifter och returnera en token vid autentiseringen.  Om ADAL är tooreturn en token av någon anledning, genereras ett `AdalException`.
* Observera att hello `AuthenticationResult` objektet innehåller en `UserInfo` objekt som kan vara används toocollect information som din app kan behöva.  I hello DirectorySearcher, `UserInfo` är används toocustomize hello appens användargränssnitt med hello användar-id.
* När hello användaren klickar hello ”logga ut”, vi vill tooensure som hello nästa anrop för`AcquireTokenAsync(...)` ber hello användaren toosign i.  Det är lika enkelt som att rensa hello token-cache med ADAL:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear hello token cache
    authContext.TokenCache.Clear();

    ...
}
```

* Hello användaren inte klickar på hello ”logga ut” knappen ska toomaintain hello användarsession för hello nästa gång de kör hello DirectorySearcher.  Du kan kontrollera ADAL'S token-cache för en befintlig token och uppdateras hello Användargränssnittet när hello app startar.  I hello `CheckForCachedToken()` metod, gör ett annat anrop för`AcquireTokenAsync(...)`, nu skicka i hello `PromptBehavior.Never` parameter.  `PromptBehavior.Never`talar om ADAL att hello användaren inte ska uppmanas att logga in och ADAL i stället utlösa ett undantag om det är tooreturn en token.

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

Grattis! Du nu ha en fungerande .NET WPF-program som har hello möjlighet tooauthenticate användare på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och hämta grundläggande information om hello användare.  Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare.  Kör appen DirectorySearcher och logga in med något av dessa användare.  Sök efter andra användare baserat på deras UPN.  Stäng hello app och kör den igen.  Observera hur hello användarens session förblir intakt.  Logga ut och logga in igen som en annan användare.

ADAL gör det enkelt tooincorporate alla dessa vanliga identity-funktioner i programmet.  Det hand tar om alla hello ändrad arbete du - hantering av cache, OAuth protokollstöd presentera hello användare med en inloggning UI, uppdatera token har upphört att gälla och mycket mer.  Allt du verkligen behöver tooknow är ett enda API-anrop `authContext.AcquireTokenAsync(...)`.

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) tillhandahålls [här](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Du kan nu gå vidare tooadditional scenarier.  Du kanske vill tootry:

[Skydda ett .NET-webb-API med Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


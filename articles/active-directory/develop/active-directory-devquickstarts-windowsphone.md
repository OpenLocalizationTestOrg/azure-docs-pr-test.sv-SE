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
# <a name="integrate-azure-ad-with-a-windows-phone-app"></a>Integrera Azure AD med en Windows Phone-App
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Windows Phone 8.1 och projekt i tidigare versioner stöds inte i Visual Studio 2017.  Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).

Om du utvecklar en app för Windows Phone 8.1, gör Azure AD det lätt för tooauthenticate du dina användare med deras Active Directory-konton.  Program-toosecurely kan också använda en webb-API som skyddas av Azure AD, som hello Office 365-API: er eller hello Azure API.

> [!NOTE]
> Det här kodexemplet använder ADAL version 2.0.  Senaste hello-teknik kan du vill tooinstead försök vår [Windows Universal självstudier med ADAL v3.0](active-directory-devquickstarts-windowsstore.md).  Om du verkligen skapar en app för Windows Phone 8.1 är hello rätt plats.  ADAL v2.0 stöds fortfarande helt och hello rekommenderat sätt att utveckla appar agianst Windows Phone 8.1 använder Azure AD.
> 
> 

Azure AD innehåller hello Active Directory Authentication Library eller ADAL för .NET interna klienter som behöver tooaccess skyddade resurser.  ADALS enda syfte i livslängd är toomake det enkelt för din app tooget åtkomst-token.  toodemonstrate hur lätt det är här vi ska skapa en ”Directory användaren” Windows Phone 8.1-app som:

* Hämtar åtkomst till token för att anropa hello Azure AD Graph API: et med hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Söker en katalog för användare med ett angivet UPN.
* Användare loggar ut.

toobuild hello fullständig fungerande program, måste du:

1. Registrera ditt program med Azure AD.
2. Installera och konfigurera ADAL.
3. Använd ADAL tooget token från Azure AD.

tooget har startats [ladda ned stommen av ett projekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Varje är en lösning för Visual Studio 2013.  Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program.  Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).

## <a name="1-register-hello-directory-searcher-application"></a>1. Registrera hello Directory användaren-program
tooenable tooget token för din app behöver du tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello översta, klicka på fältet för ditt konto och på hello **Directory** Välj plats tooregister programmet hello Active Directory-klient.
3. Klicka på **fler tjänster** i hello vänstra nav och välj **Azure Active Directory**.
4. Klicka på **App registreringar** och välj **Lägg till**.
5. Följ anvisningarna för hello och skapa en ny **internt klientprogram**.
  * Hej **namn** av hello programmet beskriver dina program tooend-användare
  * Hej **omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD ska använda tooreturn token svar.  Ange en platshållarvärde för nu, t.ex. `http://DirectorySearcher`.  Vi kommer att ersätta det här värdet senare.
6. När du har slutfört registreringen tilldelas AAD appen ett unikt program-ID.  Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello programfliken.
7. Från hello **inställningar** väljer **nödvändiga behörigheter** och välj **Lägg till**. Välj hello **Microsoft Graph** som hello API och Lägg till hello **läsa katalogdata** behörighet under **delegerade behörigheter**.  Detta gör att dina program tooquery hello Graph API för användare.

## <a name="2-install--configure-adal"></a>2. Installera och konfigurera ADAL
Nu när du har ett program i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.  ADAL toobe kan toocommunicate med Azure AD, du måste för tooprovide med viss information om din appregistrering.

* Börja genom att lägga till ADAL toohello DirectorySearcher projektet med hello Package Manager-konsolen.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* Öppna i hello DirectorySearcher project `MainPage.xaml.cs`.  Ersätt hello värden i hello `Config Values` region tooreflect hello värden indata i hello Azure-portalen.  Koden ska referera till dessa värden när den använder ADAL.
  * Hej `tenant` är hello domän för din Azure AD-klient, t.ex. contoso.onmicrosoft.com
  * Hej `clientId` är hello clientId på ditt program som du kopierade från hello-portalen.
* Du måste nu toodiscover hello återanrop uri för Windows Phone-app.  Ange en brytpunkt på den här raden i hello `MainPage` metoden:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* Kör hello appen och kopiera reserverats hello värdet för `redirectUri` när hello brytpunkt träffar.  Det bör se ut ungefär så

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* Tillbaka på hello **konfigurera** fliken i ditt program i hello Azure Management Portal ersätta hello värde i hello **RedirectUri** med det här värdet.  

## <a name="3-use-adal-tooget-tokens-from-aad"></a>3. Använd ADAL tooGet token från AAD
hello grundläggande principen bakom ADAL är att när din app behöver en åtkomst-token kan bara anropas `authContext.AcquireToken(…)`, och ADAL hello rest.  

* hello första steget är tooinitialize appens `AuthenticationContext` -ADAL vars primära klassen.  Det är där du skickar ADAL hello koordinater måste toocommunicate med Azure AD och anger hur toocache token.

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* Hitta nu hello `Search(...)` metod som kommer att anropas när hello användaren cliks hello ”Sök”-knappen i hello appens användargränssnitt.  Den här metoden gör en GET-begäran toohello Azure AD Graph API tooquery för användare vars UPN-namnet börjar med hello angivna sökord.  Men i ordning tooquery hello Graph API, behöver du tooinclude en access_token i hello `Authorization` rubriken för hello begäran - detta är där ADAL kommer in.

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
* Om interaktiv autentisering krävs ADAL använder Windows Phone Web Authentication Service Broker (Windows Adressbok) och [fortsättning modellen](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) toodisplay hello Azure AD inloggningssida.  När hello användaren loggar in, måste appen toopass ADAL hello resultaten av hello Windows Adressbok interaktion.  Det är enkelt implementera hello `ContinueWebAuthentication` gränssnitt:

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

* Nu är det tid toouse hello `AuthenticationResult` som ADAL returnerade tooyour app.  I hello `QueryGraph(...)` motringning, bifoga hello access_token som du har köpt toohello GET-begäran i hello Authorization-huvud:

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
* Du kan också använda hello `AuthenticationResult` objekt toodisplay information om hello användare i din app. I hello `QueryGraph(...)` metod, Använd hello resultatet tooshow hello användarens id på hello sida:

```C#
// Update hello Page UI toorepresent hello signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* Slutligen kan du använda ADAL toosign hello användare utanför programmet samt.  När hello användaren klickar hello ”logga ut”, vi vill tooensure som hello nästa anrop för`AcquireTokenSilentAsync(...)` misslyckas.  Det är lika enkelt som att rensa hello token-cache med ADAL:

```C#
private void SignOut()
{
    // Clear session state from hello token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Grattis! Du nu har en aktiv Windows Phone-app som har hello möjlighet tooauthenticate användare på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och hämta grundläggande information om hello användare.  Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare.  Kör appen DirectorySearcher och logga in med något av dessa användare.  Sök efter andra användare baserat på deras UPN.  Stäng hello app och kör den igen.  Observera hur hello användarens session förblir intakt.  Logga ut och logga in igen som en annan användare.

ADAL gör det enkelt tooincorporate alla dessa vanliga identity-funktioner i programmet.  Det hand tar om alla hello ändrad arbete du - hantering av cache, OAuth protokollstöd presentera hello användare med en inloggning UI, uppdatera token har upphört att gälla och mycket mer.  Allt du verkligen behöver tooknow är ett enda API-anrop `authContext.AcquireToken*(…)`.

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) tillhandahålls [här](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Du kan nu gå vidare tooadditional identitet scenarier.  Du kanske vill tootry:

[Skydda ett .NET-webb-API med Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


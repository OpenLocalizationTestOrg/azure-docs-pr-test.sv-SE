---
title: "aaaAzure AD Windows Store komma igång | Microsoft Docs"
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
ms.openlocfilehash: 1d12c7b928bc0e94fb823f8db4a09ff416205e2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-windows-store-apps"></a>Integrera Azure AD med Windows Store-appar
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Windows Store 8.1 och tidigare version projekt stöds inte i Visual Studio-2017.  Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).

Om du utvecklar appar för Windows Store för hello Azure Active Directory (AD Azure) gör det lätt tooauthenticate dina användare med deras Active Directory-konton. En app kan på ett säkert sätt använda alla webb-API som skyddas av Azure AD, till exempel hello Office 365-API: er eller hello Azure API genom att integrera med Azure AD.

För Windows Store-program som behöver tooaccess skyddade resurser, innehåller Azure AD hello Active Directory Authentication Library (ADAL). hello uteslutande av ADAL är toomake det enkelt för hello app tooget åtkomst-token. toodemonstrate hur lätt det är den här artikeln visar hur toobuild en DirectorySearcher Windows Store-app som:

* Hämtar åtkomst till token för att anropa hello Azure AD Graph API med hjälp av hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Söker en katalog för användare med en viss användarens huvudnamn (UPN).
* Användare loggar ut.

## <a name="before-you-get-started"></a>Innan du börjar
* Hämta hello [stommen av ett projekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), eller hämta hello [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip). Varje version är en lösning i Visual Studio 2015.
* Du måste också en Azure AD-klient i vilka toocreate användare och registrera hello app. Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).

När du är klar hello Följ hello procedurerna i följande tre avsnitt.

## <a name="step-1-register-hello-directorysearcher-app"></a>Steg 1: Registrera hello DirectorySearcher app
tooenable hello tooget apptoken, måste du först tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API. Så här gör du:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på ditt konto hello översta fältet. Sedan, under hello **Directory** listan, Välj hello Active Directory-klient där du vill att tooregister hello app.
3. Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.
4. Klicka på **App registreringar**, och välj sedan **Lägg till**.
5. Följ hello prompter toocreate en **internt klientprogram**.
  * **Namnet** beskriver hello app toousers.
  * **Omdirigerings-URI** är en kombination av schemat och strängen som använder Azure AD tooreturn token svar. Ange ett platshållarvärde för nu (till exempel **http://DirectorySearcher**). Du måste ersätta hello värdet senare.
6. När du har slutfört registreringen hello tilldelar Azure AD hello app ett unikt-ID. Kopiera hello värde på hello **programmet** fliken, eftersom du behöver senare.
7. På hello **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.
8. För hello **Azure Active Directory** app markerar **Microsoft Graph** som hello API.
9. Under **delegerade behörigheter**, lägga till hello **Access hello-katalogen som hello inloggade användare** behörighet. På så sätt kan hello app tooquery hello Graph API för användare.

## <a name="step-2-install-and-configure-adal"></a>Steg 2: Installera och konfigurera ADAL
Nu när du har en app i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade. tooenable ADAL toocommunicate med Azure AD, ger det viss information om hello appregistrering.

1. Lägg till ADAL toohello DirectorySearcher projektet genom att använda hello Package Manager-konsolen.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. Öppna MainPage.xaml.cs i hello DirectorySearcher projekt.
3. Ersätt hello värden i hello **konfigurationsvärden** region med hello-värden som du angav i hello Azure-portalen. Din kod refererar toothese värden när den använder ADAL.
  * Hej *klient* är hello domän för din Azure AD-klient (till exempel contoso.onmicrosoft.com).
  * Hej *clientId* är hello klient-ID för hello-app som du kopierade från hello-portalen.
4. Du måste nu toodiscover hello återanrop URI för Windows Store-app. Ange en brytpunkt på den här raden i hello `MainPage` metoden:
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. Skapa hello-lösning, se till att alla paketet refererar till har återställts. Om några paket saknas, öppna hello NuGet Package Manager och återställa dem.
6. Kör hello appen och kopiera hello värdet för `redirectUri` när hello brytpunkt träffar. hello värde ska se ut ungefär hello följande:

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. Tillbaka på hello **inställningar** för hello app i hello Azure-portalen, lägga till en **RedirectUri** med hello före värdet.  

## <a name="step-3-use-adal-tooget-tokens-from-azure-ad"></a>Steg 3: Använda ADAL tooget token från Azure AD
hello grundläggande principen bakom ADAL är att när hello app måste en åtkomst-token, bara anropas `authContext.AcquireToken(…)`, och ADAL hello rest.  

1. Initiera hello app `AuthenticationContext`, vilket är hello primära klassen av ADAL. Den här åtgärden klarar ADAL hello koordinater den behöver toocommunicate med Azure AD och anger hur toocache token.

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. Leta upp hello `Search(...)` metod som anropas när användaren klickar på hello **Sök** knappen hello appens användargränssnitt. Den här metoden gör en get-begäran toohello Azure AD Graph API tooquery för användare vars UPN-namnet börjar med hello angivna sökord. tooquery hello Graph API är en åtkomst-token i hello begäran **auktorisering** huvud. Det är där ADAL kommer in.

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
                ShowAuthError(string.Format("If hello error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    När hello app begär en token genom att anropa `AcquireTokenAsync(...)`, ADAL försöker tooreturn en token utan att uppge autentiseringsuppgifter hello användare. Om ADAL avgör hello användaren måste toosign i tooget en token, den visar inloggning dialogrutan samlar in hello användarens autentiseringsuppgifter och returnerar en token efter att autentiseringen lyckas. Om ADAL är tooreturn en token av någon anledning, hello *AuthenticationResult* status är ett fel.
3. Nu är det tid toouse hello åtkomsttoken du just har skaffat. Även i hello `Search(...)` metod, bifoga hello token toohello Graph API hämta begäran i hello **auktorisering** huvud:

    ```C#
    // Add hello access token toohello Authorization header of hello call toohello Graph API, and call hello Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. Du kan använda hello `AuthenticationResult` objekt toodisplay information om hello användare i hello appen, till exempel hello användar-ID:

    ```C#
    // Update hello page UI toorepresent hello signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. Du kan också använda ADAL toosign användare utanför hello appen. När hello användaren klickar på hello **logga ut** knappen, se till att hello nästa anrop för`AcquireTokenAsync(...)` visas en vy för inloggning. Den här åtgärden är lika enkelt som att rensa hello token-cache med ADAL:

    ```C#
    private void SignOut()
    {
        // Clear session state from hello token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a>Nästa steg
Nu har du en fungerande Windows Store-app som kan autentisera användare, på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och få grundläggande information om hello användare.

Om du inte redan har fyllts i din klient med användare, är nu hello tid toodo så.
1. Kör appen DirectorySearcher och logga sedan in med någon av hello användare.
2. Sök efter andra användare baserat på deras UPN.
3. Stäng hello app och köra den igen. Observera hur hello användarens session förblir intakt.
4. Logga ut genom att högerklicka på toodisplay hello nedre fältet och sedan logga in igen som en annan användare.

ADAL gör det enkelt tooincorporate alla vanliga identitet funktionerna i hello app. Det tar hand om alla hello ändrad arbete för dig, t.ex hantering av cache OAuth protokollstöd presentera hello användare med en inloggning-Gränssnittet och uppdatera gått token. Du behöver tooknow bara ett enda API-anrop `authContext.AcquireToken*(…)`.

Hämta hello referens [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (utan dina konfigurationsvärden).

Du kan nu gå vidare tooadditional identitet scenarier. Till exempel försöka [skydda ett .NET-webb-API med Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

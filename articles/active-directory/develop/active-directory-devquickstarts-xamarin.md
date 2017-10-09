---
title: "aaaAzure AD Xamarin komma igång | Microsoft Docs"
description: "Skapa Xamarin-program som integreras med Azure AD för inloggning och anropa Azure AD-skyddade API: er med hjälp av OAuth."
services: active-directory
documentationcenter: xamarin
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6a0d189648b7071558ac1cf2b908808668960a4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-xamarin-apps"></a>Integrera Azure AD med Xamarin-appar
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Du kan skriva mobila appar med Xamarin, i C# som kan köras på iOS, Android och Windows (mobila enheter och datorer). Om du utvecklar en app med Xamarin, Azure Active Directory (AD Azure) som gör att det enkla tooauthenticate användare med sina Azure AD-konton. hello app kan också säkert att använda alla webb-API som skyddas av Azure AD, till exempel hello Office 365-API: er eller hello Azure API.

Azure AD innehåller hello Active Directory Authentication Library (ADAL) för Xamarin-appar som behöver tooaccess skyddade resurser. hello uteslutande av ADAL är toomake det enkelt för appar tooget åtkomst-token. toodemonstrate hur lätt det är den här artikeln visar hur toobuild DirectorySearcher appar som:

* Kör på iOS, Android, Windows-skrivbordet, Windows Phone och Windows Store.
* Använda en enda bärbar klass bibliotek (PCL) tooauthenticate användare och hämta token för hello Azure AD Graph API.
* Söka i en katalog för användare med ett angivet UPN.

## <a name="before-you-get-started"></a>Innan du börjar
* Hämta hello [stommen av ett projekt](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), eller hämta hello [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Varje version är en lösning i Visual Studio 2013.
* Du måste också en Azure AD-klient i vilka toocreate användare och registrera hello app. Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).

När du är klar hello Följ hello procedurerna i följande fyra avsnitt.

## <a name="step-1-set-up-your-xamarin-development-environment"></a>Steg 1: Ställ in din utvecklingsmiljö för Xamarin
Eftersom den här självstudiekursen innehåller projekt för iOS, Android och Windows, måste både Visual Studio och Xamarin. toocreate hello nödvändiga miljö, fullständig hello processen i [ställa upp och installera Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) på MSDN. hello anvisningarna inkluderar material som du kan granska toolearn mer om Xamarin medan du väntar hello installationer toobe slutförts.

När du har slutfört installationen hello, öppna hello lösningen i Visual Studio. Där hittar du sex projekt: fem plattformsspecifika projekt och en PCL, DirectorySearcher.cs som delas mellan alla plattformar.

## <a name="step-2-register-hello-directorysearcher-app"></a>Steg 2: Registrera hello DirectorySearcher app
tooenable hello tooget apptoken, måste du först tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API. Så här gör du:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på ditt konto hello översta fältet. Sedan, under hello **Directory** listan, Välj hello Active Directory-klient där du vill att tooregister hello app.
3. Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.
4. Klicka på **App registreringar**, och välj sedan **Lägg till**.
5. toocreate en ny **internt klientprogram**, följer du anvisningarna för hello.
  * **Namnet** beskriver hello app toousers.
  * **Omdirigerings-URI** är en kombination av schemat och strängen som använder Azure AD tooreturn token svar. Ange ett värde (till exempel http://DirectorySearcher).
6. När du har slutfört registreringen, tilldelar Azure AD hello app ett unikt-ID. Kopiera hello värdet från hello **programmet** fliken, eftersom du behöver senare.
7. På hello **inställningar** väljer **nödvändiga behörigheter**, och välj sedan **Lägg till**.
8. Välj **Microsoft Graph** som hello API. Under **delegerade behörigheter**, lägga till hello **läsa katalogdata** behörighet.  
Den här åtgärden aktiverar hello app tooquery hello Graph API för användare.

## <a name="step-3-install-and-configure-adal"></a>Steg 3: Installera och konfigurera ADAL
Nu när du har en app i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade. tooenable ADAL toocommunicate med Azure AD, ger det viss information om hello appregistrering.

1. Lägg till ADAL toohello DirectorySearcher projektet genom att använda hello Package Manager-konsolen.

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    `

    Observera att två biblioteket referenser läggs tooeach projekt: hello PCL delen av ADAL och en plattformsspecifik del.
2. Öppna DirectorySearcher.cs i hello DirectorySearcherLib projekt.
3. Ersätt hello klassen medlemsvärden med hello-värden som du angav i hello Azure-portalen. Din kod refererar toothese värden när den använder ADAL.

  * Hej *klient* är hello domän för din Azure AD-klient (till exempel contoso.onmicrosoft.com).
  * Hej *clientId* är hello klient-ID för hello-app som du kopierade från hello-portalen.
  * Hej *returnUri* är hello omdirigerings-URI som du angav i hello-portal (till exempel http://DirectorySearcher).

## <a name="step-4-use-adal-tooget-tokens-from-azure-ad"></a>Steg 4: Använda ADAL tooget token från Azure AD
Nästan alla hello app autentiseringslogiken ligger i `DirectorySearcher.SearchByAlias(...)`. Allt som behövs i hello plattformsspecifika projekt är toopass en kontextuella parametern toohello `DirectorySearcher` PCL.

1. Öppna DirectorySearcher.cs och Lägg sedan till en ny parameter toohello `SearchByAlias(...)` metod. `IPlatformParameters`är sammanhangsberoende hello-parameter som kapslar in hello plattformsspecifika objekt att ADAL måste tooperform hello-autentisering.

    ```C#
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. Initiera `AuthenticationContext`, vilket är hello primära klassen av ADAL.  
Den här åtgärden överför ADAL hello samordnar den behov toocommunicate med Azure AD.
3. Anropa `AcquireTokenAsync(...)`, som accepterar hello `IPlatformParameters` objekt och anropar hello autentiseringsflödet som är nödvändiga tooreturn en token toohello app.

    ```C#
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    `AcquireTokenAsync(...)`första försök tooreturn en token för hello begärd resurs (hello Graph API i det här fallet) utan att fråga användare tooenter sina autentiseringsuppgifter (via cachelagring eller uppdatera gamla token). Vid behov visar den användare hello Azure AD-inloggningssida före hämtning hello begärda token.
4. Kopplingsbegäran hello åtkomst-token toohello Graph API i hello **auktorisering** huvud:

    ```C#
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

Det är allt för hello `DirectorySearcher` PCL och hello appens identitetsrelaterade kod. Allt som återstår är toocall hello `SearchByAlias(...)` metod i vyer för varje plattform och, vid behov tooadd kod för att korrekt hantera hello UI livscykel.

### <a name="android"></a>Android
1. MainActivity.cs, lägga till ett anrop för`SearchByAlias(...)` i hello knappen klickar du på hanterare:

    ```C#
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. Åsidosätt hello `OnActivityResult` livscykel metoden tooforward någon autentisering omdirigerar tillbaka toohello lämplig metod. ADAL innehåller en hjälpmetod för detta i Android:

    ```C#
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a>Windows-skrivbordet
I MainWindow.xaml.cs, gör ett anrop för`SearchByAlias(...)` genom att skicka en `WindowInteropHelper` i hello desktop `PlatformParameters` objekt:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
I DirSearchClient_iOSViewController.cs, hello iOS `PlatformParameters` objektet tar en referens toohello View Controller:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Windows Universal
Öppna MainPage.xaml.cs i den universella Windows- och sedan implementera hello `Search` metod. Den här metoden använder en hjälpmetod i ett delat projekt tooupdate Användargränssnittet vid behov.

```C#
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a>Nästa steg
Nu har du en fungerande Xamarin-app som kan autentisera användare och på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 över fem olika plattformar.

Om du inte redan har fyllts i din klient med användare, är nu hello tid toodo så.

1. Kör appen DirectorySearcher och logga sedan in med någon av hello användare.
2. Sök efter andra användare baserat på deras UPN.

ADAL gör det enkelt tooincorporate vanliga identity-funktioner i hello app. Det tar hand om alla hello ändrad arbete för dig, t.ex hantering av cache OAuth protokollstöd presentera hello användare med en inloggning-Gränssnittet och uppdatera gått token. Du behöver tooknow bara ett enda API-anrop `authContext.AcquireToken*(…)`.

Hämta hello referens [färdiga exemplet](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (utan dina konfigurationsvärden).

Du kan nu gå vidare tooadditional identitet scenarier. Till exempel försöka [skydda ett .NET-webb-API med Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

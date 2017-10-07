---
title: "aaaAzure AD AngularJS komma igång | Microsoft Docs"
description: "Hur toobuild ett enda sida AngularJS-program som kan integreras med Azure AD för inloggning och anropar Azure AD-skyddade API: er med hjälp av OAuth."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: eca5e1c9662186dfae4f96ca3041f9350583cf79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="help-secure-angularjs-single-page-apps-by-using-azure-ad"></a>Skydda AngularJS sida appar med hjälp av Azure AD

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory (AD Azure) gör det lätt för du tooadd inloggning, utloggning, och säker OAuth-API-anrop tooyour sida appar.  Det gör att dina appar tooauthenticate användare med sina Windows Server Active Directory-konton och använda en webb-API som Azure AD hjälper till att skydda, till exempel hello Office 365-API: er eller hello Azure API.

För JavaScript-program som körs i en webbläsare, Azure AD innehåller hello Active Directory Authentication Library (ADAL) eller adal.js. hello uteslutande för adal.js är toomake det enkelt för din app tooget åtkomst-token. toodemonstrate hur lätt det är här vi ska skapa en AngularJS tooDo lista över program som:

* Loggar hello användare i toohello app med Azure AD som identitetsprovider hello.

* Visar information om hello användare.
* På ett säkert sätt hello anrop appens tooDo lista API med hjälp av ägar-token från Azure AD.
* Loggar hello användare utanför hello appen.

toobuild hello klar fungerande program, måste du:

1. Registrera din app med Azure AD.
2. Installera ADAL och konfigurera hello sida app.
3. Använda ADAL toohelp säkra sidor i hello sida app.

tooget har startats [hämta hello app stommen](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip). Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program. Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).

## <a name="step-1-register-hello-directorysearcher-application"></a>Steg 1: Registrera hello DirectorySearcher program
tooenable app tooauthenticate användare och hämta token, måste du först tooregister den i din Azure AD-klient:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Om du är inloggad toomultiple kataloger eventuellt tooensure du visar hello rätt katalog. toodo på hello översta fältet, klicka på ditt konto. Under hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.
3. Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.
4. Klicka på **App registreringar**, och välj sedan **Lägg till**.
5. Följ anvisningarna för hello och skapa ett nytt webbprogram och/eller webb-API:
  * **Namnet** beskriver toousers ditt program.
  * **Omdirigerings-Uri** är hello plats toowhich Azure AD som returneras av token. hello standardplatsen för det här exemplet är `https://localhost:44326/`.
6. När du har slutfört registreringen tilldelar en unik program-ID tooyour app i Azure AD.  Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello programfliken.
7. Adal.js använder hello OAuth implicita flödet toocommunicate med Azure AD. Du måste aktivera hello implicita flödet för programmet:
  1. Klicka på hello programmet och välj **Manifest** tooopen hello infogade manifestet editor.
  2. Leta upp hello `oauth2AllowImplicitFlow` egenskapen. Ange värdet för`true`.
  3. Klicka på **spara** toosave hello manifest.
8. Ge din klient för ditt program. Gå för**inställningar** > **egenskaper** > **nödvändiga behörigheter**, och klicka på hello **bevilja behörigheter**knappen hello översta raden. Klicka på **Ja** tooconfirm.

## <a name="step-2-install-adal-and-configure-hello-single-page-app"></a>Steg 2: Installera ADAL och konfigurera hello sida app
Nu när du har ett program i Azure AD kan du installera adal.js och Skriv koden identitetsrelaterade.

### <a name="configure-hello-javascript-client"></a>Konfigurera hello JavaScript-klienten
Börja genom att lägga till adal.js toohello TodoSPA projekt med hjälp av hello Package Manager-konsolen:
  1. Hämta [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) och lägga till den toohello `App/Scripts/` projektkatalogen.
  2. Hämta [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) och lägga till den toohello `App/Scripts/` projektkatalogen.
  3. Läsa in varje skript före hello slut hello `</body>` i `index.html`:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-hello-back-end-server"></a>Konfigurera hello backend-servern
För hello sida appens serverdel tooDo lista API tooaccept token från hello webbläsare måste hello serverdel konfigurationsinformation om hello appregistrering. Öppna i hello TodoSPA project `web.config`. Ersätt hello värdena för hello element i hello `<appSettings>` avsnittet tooreflect hello värden som du använde i hello Azure-portalen. Koden ska referera till dessa värden när den använder ADAL.
  * `ida:Tenant`är hello domän för din Azure AD-klient – till exempel contoso.onmicrosoft.com.
  * `ida:Audience`är hello klient-ID för programmet som du kopierade från hello-portalen.

## <a name="step-3-use-adal-toohelp-secure-pages-in-hello-single-page-app"></a>Steg 3: Använda ADAL toohelp säkra sidor i hello sida app
Adal.js integreras med AngularJS flödes- och HTTP-providers, så kan du säkra enskilda vyer i din app på en sida.

1. I `App/Scripts/app.js`, hämtas hello adal.js modulen:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. Initiera `adalProvider` med hello konfigurationsvärden för programmet registreringen, även i `App/Scripts/app.js`:

    ```js
    adalProvider.init(
      {
          instance: 'https://login.microsoftonline.com/',
          tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
          clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
          extraQueryParameter: 'nux=1',
          //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
      },
      $httpProvider
    );
    ```
3. Att säkra hello `TodoList` vyn i hello app med hjälp av en enda rad med kod: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Sammanfattning
Nu har du en säker sida-app som kan logga in användare och utfärda ägar-token-skyddade begäranden tooits backend-API. När en användare klickar på hello **TodoList** länk adal.js omdirigeras automatiskt tooAzure AD för inloggning om det behövs. Dessutom kan koppla adal.js automatiskt en åtkomst-token tooany Ajax-begäranden som skickas toohello appens serverdel.  

hello är föregående steg hello utan minsta nödvändiga toobuild en enda sida-app med hjälp av adal.js. Men några andra funktioner som är användbara i en sida app:

* tooexplicitly utfärda inloggnings- och utloggningsförfrågningar, kan du ange funktionerna i din domänkontrollanter som anropar adal.js.  I `App/Scripts/homeCtrl.js`:

    ```js
    ...
    $scope.login = function () {
        adalService.login();
    };
    $scope.logout = function () {
        adalService.logOut();
    };
    ...
    ```
* Du kanske vill toopresent användarinformation i hello appens användargränssnitt. Hej ADAL-tjänsten har redan lagts toohello `userDataCtrl` styrenhet, så du kan komma åt hello `userInfo` associerad vy, för objekt i hello `App/Views/UserData.html`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Det finns många scenarier där ska du tooknow om hello användaren är inloggad eller inte. Du kan också använda hello `userInfo` objekt toogather informationen.  Till exempel på `index.html`, du kan visa antingen hello **inloggning** eller **logga ut** knappen baserat på autentiseringsstatus:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

Din Azure AD-integrerade single-page-app kan autentisera användare, på ett säkert sätt anropa sina serverdelens med hjälp av OAuth 2.0 och få grundläggande information om hello användare. Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare. Kör appen tooDo lista sida och logga in med något av dessa användare. Lägga till uppgifter toohello användarens att göra-lista, logga ut och logga in igen.

Adal.js gör det enkelt tooincorporate vanliga identity-funktioner i programmet. Den tar hand om alla hello ändrad arbetet åt dig: hantering av cache, OAuth protokollstöd presentera hello användare med en inloggning användargränssnitt, uppdatera token har upphört att gälla och mycket mer.

Referens hello slutförts exemplet (utan dina konfigurationsvärden) är tillgängliga i [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Nästa steg
Du kan nu gå vidare tooadditional scenarier. Du kanske vill tootry: [anropa CORS-webb-API från en enda sida app](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

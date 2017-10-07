---
title: "aaaAzure AD v2.0 NodeJS AngularJS sida app komma igång | Microsoft Docs"
description: "Hur toobuild en vinkel JS sida-app som loggar in användare med både personliga Microsoft-konton och arbetar- eller skolkonton."
services: active-directory
documentationcenter: 
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: d286aa33-8a94-452f-beb7-ddc6c6daa5c8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 1ab450caf08ab05fba140b94b1b8de652e99cbc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-angularjs-single-page-app---nodejs"></a>Lägg till inloggning tooan AngularJS sida app - NodeJS
I den här artikeln ska vi lägga till logga in med Microsoft påslagen konton tooan AngularJS-app med hello Azure Active Directory v2.0-slutpunkten. hello v2.0-slutpunkten aktivera tooperform en enkel integrering i din app och autentiserar användare med både personliga och arbete/skola konton.

Det här exemplet är en enkel sida app för att göra-lista som lagrar uppgifter i en serverdel REST API, skrivna i NodeJS och skyddas med OAuth ägar-token från Azure AD.  Hej AngularJS appen kommer att använda våra öppen källkod JavaScript-autentiseringsbibliotek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello hela inloggning processen och hämta token för uppringande hello REST API.  hello samma mönster kan vara tillämpade tooauthenticate tooother REST API: er som hello [Microsoft Graph](https://graph.microsoft.com) eller hello Azure Resource Manager API: er.

> [!NOTE]
> Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.  toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

## <a name="download"></a>Ladda ned
tooget igång, behöver du toodownload & installera [node.js](https://nodejs.org).  Sedan kan du klona eller [hämta](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) stommen app:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

hello stommen app inkluderar alla hello formaterad exempelkod för en enkel AngularJS-app, men saknas i alla hello identitetsrelaterade delar.  Om du inte vill toofollow längs, kan du i stället klona eller [hämta](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) hello slutförts exempel.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a>Registrera en app
Först skapar en app i hello [App Registreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).  Se till att:

* Lägg till hello **Web** plattform för din app.
* Ange rätt hello **omdirigerings-URI**. hello standardvärdet för det här exemplet är `http://localhost:8080`.
* Lämna hello **Tillåt Implicit flöda** kryssrutan aktiverad. 

Kopiera hello **program-ID** som är tilldelade tooyour app behöver du den inom kort. 

## <a name="install-adaljs"></a>Installera adal.js
toostart, navigera tooproject som du laddade ned och installera adal.js.  Om du har [bower](http://bower.io/) installerat, du kan bara köra det här kommandot.  För alla beroende version avvikelser, Välj hello senare version.

```
bower install adal-angular#experimental
```

Alternativt kan du manuellt hämta [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) och [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Lägga till både filer toohello `app/lib/adal-angular-experimental/dist` directory.

Öppna hello-projekt i valfri textredigerare och läsa in adal.js hello slutet av hello sidan brödtext:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-hello-rest-api"></a>Ställ in hello REST API
Medan vi konfigurerar saker, kan get hello backend REST API fungerar.  Installera alla nödvändiga hello-paket i en kommandotolk genom att köra (Kontrollera att du är i hello översta katalogen i hello projekt):

```
npm install
```

Öppna `config.js` och Ersätt hello `audience` värde:

```js
exports.creds = {

     // TODO: Replace this value with hello Application ID from hello registration portal
     audience: '<Your-application-id>',

     ...
}
```

hello REST API använder det här värdet toovalidate token som tas emot från hello vinkel appen på AJAX-begäranden.  Observera att detta enkla REST-API som lagrar data i minnet - så varje gång toostop hello server, förlorar du alla tidigare skapade aktiviteter.

Det är allt hello tid vi toospend diskutera hur hello REST API fungerar.  Känna sig fria toopoke i hello kod, men om du vill toolearn mer om hur du skyddar web API: er med Azure AD, kolla [i den här artikeln](active-directory-v2-devquickstarts-node-api.md). 

## <a name="sign-users-in"></a>Logga in användare
Tid toowrite identitet kod.  Du kanske redan har upptäckt att adal.js innehåller en AngularJS-provider som spelas snyggt vinkel routning mekanismer.  Starta genom att lägga till hello adal modulen toohello app:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Du kan nu initiera hello `adalProvider` med program-ID:

```js
// app/scripts/app.js

...

adalProvider.init({

        // Use this value for hello public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 

        // hello 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',

        // Your application id from hello registration portal
        clientId: '<Your-application-id>',

        // If you're using IE, uncommment this line - hello default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',

    }, $httpProvider);
```

Bra, nu adal.js har all information som hello måste toosecure appen och loggar användarna i.  tooforce logga in för ett visst flöde i hello app allt som krävs är en rad med kod:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that hello user must be logged in tooaccess hello route
})

...
```

Nu när användaren klickar på hello `TodoList` länk adal.js omdirigeras automatiskt tooAzure AD för inloggning om det behövs.  Du kan också explicit skicka inloggning och utloggning begäranden genom att anropa adal.js i dina domänkontrollanter:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js hello same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {

        // Redirect hello user toosign in
        adalService.login();

    };
    $scope.logout = function () {

        // Redirect hello user toolog out    
        adalService.logOut();

    };
...
```

## <a name="display-user-info"></a>Visa användarinformation
Nu när hello är användaren inloggad, måste du tooaccess hello inloggade användarens autentiseringsdata i ditt program.  Adal.js visar den här informationen för dig i hello `userInfo` objekt.  tooaccess adal.js toohello rot omfattning hello motsvarande domänkontrollant först lägga till det här objektet i en vy:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Sedan kan du direkt adressera hello `userInfo` objekt i vyn: 

```html
<!--app/views/UserData.html-->

...

    <!--Get hello user's profile information from hello ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Du kan också använda hello `userInfo` objekt toodetermine om hello användaren är inloggad eller inte.

```html
<!--index.html-->

...

    <!--Use hello ADAL userInfo object tooshow hello right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-hello-rest-api"></a>Anropa hello REST API
Slutligen är det tid tooget vissa token och anropa hello REST API toocreate, läsa, uppdatera och ta bort uppgifter.  Väl vet du vad?  Du har inte toodo *något*.  Adal.js hand automatiskt tar om komma cachelagring och uppdatera token.  Den också hand tar om koppla de tokens toooutgoing AJAX-begäranden som du skickar toohello REST API.  

Hur fungerar det? Det är alla tack toohello magic av [AngularJS interceptorerna](https://docs.angularjs.org/api/ng/service/$http), vilket gör att adal.js tootransform utgående och inkommande HTTP-meddelanden.  Dessutom adal.js förutsätter att alla begäranden om att skicka toohello samma domän som hello fönstret ska använda token är avsedda för hello samma program-ID som hello AngularJS app.  Det är därför som vi använde hello samma program-ID i båda vinkel hello-app och hello NodeJS REST API.  Du kan åsidosätta detta beteende och berätta adal.js tooget token för andra REST API: er om det behövs - men för den här enkla scenariot hello standardvärden gör.

Här är ett kodfragment som visar hur lätt det är toosend begäranden med ägar-token från Azure AD:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Grattis!  Appen Azure AD-integrerade sida är slutförd.  Gå vidare, bugar.  Den kan autentisera användare, på ett säkert sätt anropa sina serverdelens REST-API med OpenID Connect och få grundläggande information om hello användare.  Out of box hello, den har stöd för alla användare med ett personligt Microsoft-Account eller arbete/skolkonto från Azure AD.  Pröva hello appen genom att köra:

```
node server.js
```

I en webbläsare navigerar för`http://localhost:8080`.  Logga in med ett personligt microsoftkonto eller arbete/skolkonto.  Lägg till aktiviteter toohello användarens att göra-lista och logga ut.  Logga in med hello annan typ av konto. Om du behöver en Azure AD-klient toocreate arbete/skola användare, [Lär dig hur tooget en här](active-directory-howto-tenant.md) (det är ledigt).

Lär dig mer om hello toocontinue Hej v2.0-slutpunkten, gå tillbaka tooour [v2.0 Utvecklarhandbok](active-directory-appmodel-v2-overview.md).  För ytterligare resurser, kolla:

* [Azure-exemplen på GitHub >>](https://github.com/Azure-Samples)
* [Azure AD på stackspill >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* Azure AD-dokumentationen på [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Hämta säkerhetsuppdateringar för våra produkter
Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.


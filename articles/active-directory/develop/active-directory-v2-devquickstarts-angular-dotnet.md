---
title: "aaaAzure AD v2.0 .NET AngularJS sida app komma igång | Microsoft Docs"
description: "Hur toobuild en vinkel JS sida-app som loggar in användare med både personliga Microsoft-konton och arbetar- eller skolkonton."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 6a341781-278f-461b-92ca-7572a06e6852
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: bd3fc8dce91eb0bedcbfed47a9b3ef52c5568c6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-angularjs-single-page-app---net"></a><span data-ttu-id="5e838-103">Lägg till inloggning tooan AngularJS sida app - .NET</span><span class="sxs-lookup"><span data-stu-id="5e838-103">Add sign-in tooan AngularJS single page app - .NET</span></span>
<span data-ttu-id="5e838-104">I den här artikeln ska vi lägga till logga in med Microsoft påslagen konton tooan AngularJS-app med hello Azure Active Directory v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="5e838-104">In this article we'll add sign in with Microsoft powered accounts tooan AngularJS app using hello Azure Active Directory v2.0 endpoint.</span></span>  <span data-ttu-id="5e838-105">hello v2.0-slutpunkten kan du tooperform en enkel integrering i din app och autentiserar användare med både personliga och arbete/skola konton.</span><span class="sxs-lookup"><span data-stu-id="5e838-105">hello v2.0 endpoint enables you tooperform a single integration in your app and authenticate users with both personal and work/school accounts.</span></span>

<span data-ttu-id="5e838-106">Det här exemplet är en enkel sida app för att göra-lista som lagrar uppgifter i en serverdel REST API skrivits hello .NET 4.5 MVC framework och skyddas med OAuth ägar-token från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e838-106">This sample is a simple To-Do List single page app that stores tasks in a backend REST API, written using hello .NET 4.5 MVC framework and secured using OAuth bearer tokens from Azure AD.</span></span>  <span data-ttu-id="5e838-107">Hej AngularJS appen kommer att använda våra öppen källkod JavaScript-autentiseringsbibliotek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello hela inloggning processen och hämta token för uppringande hello REST API.</span><span class="sxs-lookup"><span data-stu-id="5e838-107">hello AngularJS app will use our open source JavaScript authentication library [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello entire sign in process and acquire tokens for calling hello REST API.</span></span>  <span data-ttu-id="5e838-108">hello samma mönster kan vara tillämpade tooauthenticate tooother REST API: er som hello [Microsoft Graph](https://graph.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="5e838-108">hello same pattern can be applied tooauthenticate tooother REST APIs, like hello [Microsoft Graph](https://graph.microsoft.com).</span></span>

> [!NOTE]
> <span data-ttu-id="5e838-109">Inte alla Azure Active Directory-scenarier och funktioner som stöds av hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="5e838-109">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="5e838-110">toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="5e838-110">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download"></a><span data-ttu-id="5e838-111">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="5e838-111">Download</span></span>
<span data-ttu-id="5e838-112">tooget igång, ska du behöver toodownload & installera Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e838-112">tooget started, you'll need toodownload & install Visual Studio.</span></span>  <span data-ttu-id="5e838-113">Sedan kan du klona eller [hämta](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) stommen app:</span><span class="sxs-lookup"><span data-stu-id="5e838-113">Then you can clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) a skeleton app:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

<span data-ttu-id="5e838-114">hello stommen app inkluderar alla hello formaterad exempelkod för en enkel AngularJS-app, men saknas i alla hello identitetsrelaterade delar.</span><span class="sxs-lookup"><span data-stu-id="5e838-114">hello skeleton app includes all hello boilerplate code for a simple AngularJS app, but is missing all of hello identity-related pieces.</span></span>  <span data-ttu-id="5e838-115">Om du inte vill toofollow längs, kan du i stället klona eller [hämta](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) hello slutförts exempel.</span><span class="sxs-lookup"><span data-stu-id="5e838-115">If you don't want toofollow along, you can instead clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) hello completed sample.</span></span>

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="5e838-116">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="5e838-116">Register an app</span></span>
<span data-ttu-id="5e838-117">Först skapar en app i hello [App Registreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="5e838-117">First, create an app in hello [App Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="5e838-118">Se till att:</span><span class="sxs-lookup"><span data-stu-id="5e838-118">Make sure to:</span></span>

* <span data-ttu-id="5e838-119">Lägg till hello **Web** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="5e838-119">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="5e838-120">Ange rätt hello **omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="5e838-120">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="5e838-121">hello standardvärdet för det här exemplet är `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="5e838-121">hello default for this sample is `https://localhost:44326/`.</span></span>
* <span data-ttu-id="5e838-122">Lämna hello **Tillåt Implicit flöda** kryssrutan aktiverad.</span><span class="sxs-lookup"><span data-stu-id="5e838-122">Leave hello **Allow Implicit Flow** checkbox enabled.</span></span> 

<span data-ttu-id="5e838-123">Kopiera hello **program-ID** som är tilldelade tooyour app behöver du den inom kort.</span><span class="sxs-lookup"><span data-stu-id="5e838-123">Copy down hello **Application ID** that is assigned tooyour app, you'll need it shortly.</span></span> 

## <a name="install-adaljs"></a><span data-ttu-id="5e838-124">Installera adal.js</span><span class="sxs-lookup"><span data-stu-id="5e838-124">Install adal.js</span></span>
<span data-ttu-id="5e838-125">toostart, navigera tooproject som du laddade ned och installera adal.js.</span><span class="sxs-lookup"><span data-stu-id="5e838-125">toostart, navigate tooproject you downloaded and install adal.js.</span></span>  <span data-ttu-id="5e838-126">Om du har [bower](http://bower.io/) installerat, du kan bara köra det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="5e838-126">If you have [bower](http://bower.io/) installed, you can just run this command.</span></span>  <span data-ttu-id="5e838-127">För alla beroende version avvikelser, Välj hello senare version.</span><span class="sxs-lookup"><span data-stu-id="5e838-127">For any dependency version mismatches, just choose hello higher version.</span></span>

```
bower install adal-angular#experimental
```

<span data-ttu-id="5e838-128">Alternativt kan du manuellt hämta [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) och [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span><span class="sxs-lookup"><span data-stu-id="5e838-128">Alternatively, you can manually download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) and [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span></span>  <span data-ttu-id="5e838-129">Lägga till både filer toohello `app/lib/adal-angular-experimental/dist` för hello `TodoSPA` projekt.</span><span class="sxs-lookup"><span data-stu-id="5e838-129">Add both files toohello `app/lib/adal-angular-experimental/dist` directory of hello `TodoSPA` project.</span></span>

<span data-ttu-id="5e838-130">Nu öppna hello-projekt i Visual Studio och läsa in adal.js hello slutet av hello huvudsidan brödtext:</span><span class="sxs-lookup"><span data-stu-id="5e838-130">Now open hello project in Visual Studio, and load adal.js at hello end of hello main page's body:</span></span>

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-hello-rest-api"></a><span data-ttu-id="5e838-131">Ställ in hello REST API</span><span class="sxs-lookup"><span data-stu-id="5e838-131">Set up hello REST API</span></span>
<span data-ttu-id="5e838-132">Medan vi konfigurerar saker, det är dags hello backend REST API fungerar.</span><span class="sxs-lookup"><span data-stu-id="5e838-132">While we're setting things up, let's get hello backend REST API working.</span></span>  <span data-ttu-id="5e838-133">I hello rot hello projekt, öppna `web.config` och Ersätt hello `audience` värde.</span><span class="sxs-lookup"><span data-stu-id="5e838-133">In hello root of hello project, open `web.config` and replace hello `audience` value.</span></span>  <span data-ttu-id="5e838-134">hello REST API använder det här värdet toovalidate token som tas emot från hello vinkel appen på AJAX-begäranden.</span><span class="sxs-lookup"><span data-stu-id="5e838-134">hello REST API will use this value toovalidate tokens it receives from hello Angular app on AJAX requests.</span></span>

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>

...
```

<span data-ttu-id="5e838-135">Det är allt hello tid vi toospend diskutera hur hello REST API fungerar.</span><span class="sxs-lookup"><span data-stu-id="5e838-135">That's all hello time we're going toospend discussing how hello REST API works.</span></span>  <span data-ttu-id="5e838-136">Känna sig fria toopoke i hello kod, men om du vill toolearn mer om hur du skyddar web API: er med Azure AD, kolla [i den här artikeln](active-directory-v2-devquickstarts-dotnet-api.md).</span><span class="sxs-lookup"><span data-stu-id="5e838-136">Feel free toopoke around in hello code, but if you want toolearn more about securing web APIs with Azure AD, check out [this article](active-directory-v2-devquickstarts-dotnet-api.md).</span></span> 

## <a name="sign-users-in"></a><span data-ttu-id="5e838-137">Logga in användare</span><span class="sxs-lookup"><span data-stu-id="5e838-137">Sign users in</span></span>
<span data-ttu-id="5e838-138">Tid toowrite identitet kod.</span><span class="sxs-lookup"><span data-stu-id="5e838-138">Time toowrite some identity code.</span></span>  <span data-ttu-id="5e838-139">Du kanske redan har upptäckt att adal.js innehåller en AngularJS-provider som spelas snyggt vinkel routning mekanismer.</span><span class="sxs-lookup"><span data-stu-id="5e838-139">You might have already noticed that adal.js contains an AngularJS provider, which plays nicely with Angular routing mechanisms.</span></span>  <span data-ttu-id="5e838-140">Starta genom att lägga till hello adal modulen toohello app:</span><span class="sxs-lookup"><span data-stu-id="5e838-140">Start by adding hello adal module toohello app:</span></span>

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

<span data-ttu-id="5e838-141">Du kan nu initiera hello `adalProvider` med program-ID:</span><span class="sxs-lookup"><span data-stu-id="5e838-141">You can now initialize hello `adalProvider` with your Application ID:</span></span>

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

<span data-ttu-id="5e838-142">Bra, nu adal.js har all information som hello måste toosecure appen och loggar användarna i.</span><span class="sxs-lookup"><span data-stu-id="5e838-142">Great, now adal.js has all hello information it needs toosecure your app and sign users in.</span></span>  <span data-ttu-id="5e838-143">tooforce logga in för ett visst flöde i hello app allt som krävs är en rad med kod:</span><span class="sxs-lookup"><span data-stu-id="5e838-143">tooforce sign in for a particular route in hello app, all it takes is one line of code:</span></span>

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

<span data-ttu-id="5e838-144">Nu när användaren klickar på hello `TodoList` länk adal.js omdirigeras automatiskt tooAzure AD för inloggning om det behövs.</span><span class="sxs-lookup"><span data-stu-id="5e838-144">Now when a user clicks hello `TodoList` link, adal.js will automatically redirect tooAzure AD for sign-in if necessary.</span></span>  <span data-ttu-id="5e838-145">Du kan också explicit skicka inloggning och utloggning begäranden genom att anropa adal.js i dina domänkontrollanter:</span><span class="sxs-lookup"><span data-stu-id="5e838-145">You can also explicitly send sign-in and sign-out requests by invoking adal.js in your controllers:</span></span>

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

## <a name="display-user-info"></a><span data-ttu-id="5e838-146">Visa användarinformation</span><span class="sxs-lookup"><span data-stu-id="5e838-146">Display user info</span></span>
<span data-ttu-id="5e838-147">Nu när hello är användaren inloggad, måste du tooaccess hello inloggade användarens autentiseringsdata i ditt program.</span><span class="sxs-lookup"><span data-stu-id="5e838-147">Now that hello user is signed in, you'll probably need tooaccess hello signed-in user's authentication data in your application.</span></span>  <span data-ttu-id="5e838-148">Adal.js visar den här informationen för dig i hello `userInfo` objekt.</span><span class="sxs-lookup"><span data-stu-id="5e838-148">Adal.js exposes this information for you in hello `userInfo` object.</span></span>  <span data-ttu-id="5e838-149">tooaccess adal.js toohello rot omfattning hello motsvarande domänkontrollant först lägga till det här objektet i en vy:</span><span class="sxs-lookup"><span data-stu-id="5e838-149">tooaccess this object in a view, first add adal.js toohello root scope of hello corresponding controller:</span></span>

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

<span data-ttu-id="5e838-150">Sedan kan du direkt adressera hello `userInfo` objekt i vyn:</span><span class="sxs-lookup"><span data-stu-id="5e838-150">Then you can directly address hello `userInfo` object in your view:</span></span> 

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

<span data-ttu-id="5e838-151">Du kan också använda hello `userInfo` objekt toodetermine om hello användaren är inloggad eller inte.</span><span class="sxs-lookup"><span data-stu-id="5e838-151">You can also use hello `userInfo` object toodetermine if hello user is signed in or not.</span></span>

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

## <a name="call-hello-rest-api"></a><span data-ttu-id="5e838-152">Anropa hello REST API</span><span class="sxs-lookup"><span data-stu-id="5e838-152">Call hello REST API</span></span>
<span data-ttu-id="5e838-153">Slutligen är det tid tooget vissa token och anropa hello REST API toocreate, läsa, uppdatera och ta bort uppgifter.</span><span class="sxs-lookup"><span data-stu-id="5e838-153">Finally, it's time tooget some tokens and call hello REST API toocreate, read, update, and delete tasks.</span></span>  <span data-ttu-id="5e838-154">Väl vet du vad?</span><span class="sxs-lookup"><span data-stu-id="5e838-154">Well guess what?</span></span>  <span data-ttu-id="5e838-155">Du har inte toodo *något*.</span><span class="sxs-lookup"><span data-stu-id="5e838-155">You don't have toodo *a thing*.</span></span>  <span data-ttu-id="5e838-156">Adal.js hand automatiskt tar om komma cachelagring och uppdatera token.</span><span class="sxs-lookup"><span data-stu-id="5e838-156">Adal.js will automatically take care of getting, caching, and refreshing tokens.</span></span>  <span data-ttu-id="5e838-157">Den också hand tar om koppla de tokens toooutgoing AJAX-begäranden som du skickar toohello REST API.</span><span class="sxs-lookup"><span data-stu-id="5e838-157">It will also take care of attaching those tokens toooutgoing AJAX requests that you send toohello REST API.</span></span>  

<span data-ttu-id="5e838-158">Hur fungerar det?</span><span class="sxs-lookup"><span data-stu-id="5e838-158">How exactly does this work?</span></span> <span data-ttu-id="5e838-159">Det är alla tack toohello magic av [AngularJS interceptorerna](https://docs.angularjs.org/api/ng/service/$http), vilket gör att adal.js tootransform utgående och inkommande HTTP-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5e838-159">It's all thanks toohello magic of [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), which allows adal.js tootransform outgoing and incoming http messages.</span></span>  <span data-ttu-id="5e838-160">Dessutom adal.js förutsätter att alla begäranden om att skicka toohello samma domän som hello fönstret ska använda token är avsedda för hello samma program-ID som hello AngularJS app.</span><span class="sxs-lookup"><span data-stu-id="5e838-160">Furthermore, adal.js assumes that any requests send toohello same domain as hello window should use tokens intended for hello same Application ID as hello AngularJS app.</span></span>  <span data-ttu-id="5e838-161">Det är därför som vi använde hello samma program-ID i båda vinkel hello-app och hello NodeJS REST API.</span><span class="sxs-lookup"><span data-stu-id="5e838-161">This is why we used hello same Application ID in both hello Angular app and in hello NodeJS REST API.</span></span>  <span data-ttu-id="5e838-162">Du kan åsidosätta detta beteende och berätta adal.js tooget token för andra REST API: er om det behövs - men för den här enkla scenariot hello standardvärden gör.</span><span class="sxs-lookup"><span data-stu-id="5e838-162">Of course, you can override this behavior and tell adal.js tooget tokens for other REST APIs if necessary - but for this simple scenario hello defaults will do.</span></span>

<span data-ttu-id="5e838-163">Här är ett kodfragment som visar hur lätt det är toosend begäranden med ägar-token från Azure AD:</span><span class="sxs-lookup"><span data-stu-id="5e838-163">Here's a snippet that shows how easy it is toosend requests with bearer tokens from Azure AD:</span></span>

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

<span data-ttu-id="5e838-164">Grattis!</span><span class="sxs-lookup"><span data-stu-id="5e838-164">Congratulations!</span></span>  <span data-ttu-id="5e838-165">Appen Azure AD-integrerade sida är slutförd.</span><span class="sxs-lookup"><span data-stu-id="5e838-165">Your Azure AD integrated single page app is now complete.</span></span>  <span data-ttu-id="5e838-166">Gå vidare, bugar.</span><span class="sxs-lookup"><span data-stu-id="5e838-166">Go ahead, take a bow.</span></span>  <span data-ttu-id="5e838-167">Den kan autentisera användare, på ett säkert sätt anropa sina serverdelens REST-API med OpenID Connect och få grundläggande information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="5e838-167">It can authenticate users, securely call its backend REST API using OpenID Connect, and get basic information about hello user.</span></span>  <span data-ttu-id="5e838-168">Out of box hello, den har stöd för alla användare med ett personligt Microsoft-Account eller arbete/skolkonto från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e838-168">Out of hello box, it supports any user with a personal Microsoft Account or a work/school account from Azure AD.</span></span>  <span data-ttu-id="5e838-169">Kör hello app och i en webbläsare navigerar för`https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="5e838-169">Run hello app, and in a browser navigate too`https://localhost:44326/`.</span></span>  <span data-ttu-id="5e838-170">Logga in med ett personligt microsoftkonto eller arbete/skolkonto.</span><span class="sxs-lookup"><span data-stu-id="5e838-170">Sign in using either a personal Microsoft account or a work/school account.</span></span>  <span data-ttu-id="5e838-171">Lägg till aktiviteter toohello användarens att göra-lista och logga ut.  Logga in med hello annan typ av konto.</span><span class="sxs-lookup"><span data-stu-id="5e838-171">Add tasks toohello user's to-do list, and sign out.  Try signing in with hello other type of account.</span></span> <span data-ttu-id="5e838-172">Om du behöver en Azure AD-klient toocreate arbete/skola användare, [Lär dig hur tooget en här](active-directory-howto-tenant.md) (det är ledigt).</span><span class="sxs-lookup"><span data-stu-id="5e838-172">If you need an Azure AD tenant toocreate work/school users, [learn how tooget one here](active-directory-howto-tenant.md) (it's free).</span></span>

<span data-ttu-id="5e838-173">Lär dig mer om hello v2.0-slutpunkten, head tillbaka tooour toocontinue [v2.0 Utvecklarhandbok](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5e838-173">toocontinue learning about hello v2.0 endpoint, head back tooour [v2.0 developer guide](active-directory-appmodel-v2-overview.md).</span></span>  <span data-ttu-id="5e838-174">För ytterligare resurser, kolla:</span><span class="sxs-lookup"><span data-stu-id="5e838-174">For additional resources, check out:</span></span>

* [<span data-ttu-id="5e838-175">Azure-exemplen på GitHub >></span><span class="sxs-lookup"><span data-stu-id="5e838-175">Azure-Samples on GitHub >></span></span>](https://github.com/Azure-Samples)
* [<span data-ttu-id="5e838-176">Azure AD på stackspill >></span><span class="sxs-lookup"><span data-stu-id="5e838-176">Azure AD on Stack Overflow >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* <span data-ttu-id="5e838-177">Azure AD-dokumentationen på [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span><span class="sxs-lookup"><span data-stu-id="5e838-177">Azure AD documentation on [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span></span>

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="5e838-178">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="5e838-178">Get security updates for our products</span></span>
<span data-ttu-id="5e838-179">Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="5e838-179">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>


---
title: "Azure AD v2.0 NodeJS AngularJS sida app komma igång | Microsoft Docs"
description: "Hur du skapar en vinkel JS sida-app som loggar in användare med både personliga Microsoft-konton och arbets-eller skolkonton."
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
ms.openlocfilehash: 0e90171afd9c4c782fbb18375ab2d147497ef442
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a><span data-ttu-id="a8152-103">Lägga till inloggning till en AngularJS sida app - NodeJS</span><span class="sxs-lookup"><span data-stu-id="a8152-103">Add sign-in to an AngularJS single page app - NodeJS</span></span>
<span data-ttu-id="a8152-104">I den här artikeln ska vi lägga till logga in med Microsoft påslagen konton för en AngularJS-app med Azure Active Directory v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="a8152-104">In this article we'll add sign in with Microsoft powered accounts to an AngularJS app using the Azure Active Directory v2.0 endpoint.</span></span> <span data-ttu-id="a8152-105">v2.0-slutpunkten kan du utföra en enkel integrering i din app och autentiserar användare med både personliga och arbete/skola konton.</span><span class="sxs-lookup"><span data-stu-id="a8152-105">the v2.0 endpoint enable you to perform a single integration in your app and authenticate users with both personal and work/school accounts.</span></span>

<span data-ttu-id="a8152-106">Det här exemplet är en enkel sida app för att göra-lista som lagrar uppgifter i en serverdel REST API, skrivna i NodeJS och skyddas med OAuth ägar-token från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8152-106">This sample is a simple To-Do List single page app that stores tasks in a backend REST API, written in NodeJS and secured using OAuth bearer tokens from Azure AD.</span></span>  <span data-ttu-id="a8152-107">AngularJS-appen kommer att använda våra öppen källkod JavaScript-autentiseringsbibliotek [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) kan hantera hela tecknet i processen och hämta token för att anropa REST-API.</span><span class="sxs-lookup"><span data-stu-id="a8152-107">The AngularJS app will use our open source JavaScript authentication library [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) to handle the entire sign in process and acquire tokens for calling the REST API.</span></span>  <span data-ttu-id="a8152-108">Samma mönster som kan användas för att autentisera till andra REST-API: er, som den [Microsoft Graph](https://graph.microsoft.com) eller Azure Resource Manager API: er.</span><span class="sxs-lookup"><span data-stu-id="a8152-108">The same pattern can be applied to authenticate to other REST APIs, like the [Microsoft Graph](https://graph.microsoft.com) or the Azure Resource Manager APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="a8152-109">Inte alla Azure Active Directory-scenarier och funktioner som stöds av v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="a8152-109">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="a8152-110">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="a8152-110">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download"></a><span data-ttu-id="a8152-111">Ladda ned</span><span class="sxs-lookup"><span data-stu-id="a8152-111">Download</span></span>
<span data-ttu-id="a8152-112">Om du vill komma igång, måste du hämta och installera [node.js](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="a8152-112">To get started, you'll need to download & install [node.js](https://nodejs.org).</span></span>  <span data-ttu-id="a8152-113">Sedan kan du klona eller [hämta](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) stommen app:</span><span class="sxs-lookup"><span data-stu-id="a8152-113">Then you can clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) a skeleton app:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

<span data-ttu-id="a8152-114">Appen stommen innehåller formaterad exempelkod för en enkel AngularJS-app, men saknar alla identitetsrelaterade bitar.</span><span class="sxs-lookup"><span data-stu-id="a8152-114">The skeleton app includes all the boilerplate code for a simple AngularJS app, but is missing all of the identity-related pieces.</span></span>  <span data-ttu-id="a8152-115">Om du inte vill att följa instruktionerna du i stället klona eller [hämta](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) det slutförda exemplet.</span><span class="sxs-lookup"><span data-stu-id="a8152-115">If you don't want to follow along, you can instead clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) the completed sample.</span></span>

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a><span data-ttu-id="a8152-116">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="a8152-116">Register an app</span></span>
<span data-ttu-id="a8152-117">Först skapar du en app i den [App Registreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följa dessa [detaljerade steg](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="a8152-117">First, create an app in the [App Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="a8152-118">Se till att:</span><span class="sxs-lookup"><span data-stu-id="a8152-118">Make sure to:</span></span>

* <span data-ttu-id="a8152-119">Lägg till den **Web** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="a8152-119">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="a8152-120">Ange rätt **omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="a8152-120">Enter the correct **Redirect URI**.</span></span> <span data-ttu-id="a8152-121">Standardvärdet för det här exemplet är `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a8152-121">The default for this sample is `http://localhost:8080`.</span></span>
* <span data-ttu-id="a8152-122">Lämna den **Tillåt Implicit flöda** kryssrutan aktiverad.</span><span class="sxs-lookup"><span data-stu-id="a8152-122">Leave the **Allow Implicit Flow** checkbox enabled.</span></span> 

<span data-ttu-id="a8152-123">Kopiera den **program-ID** som har tilldelats din app behöver du den inom kort.</span><span class="sxs-lookup"><span data-stu-id="a8152-123">Copy down the **Application ID** that is assigned to your app, you'll need it shortly.</span></span> 

## <a name="install-adaljs"></a><span data-ttu-id="a8152-124">Installera adal.js</span><span class="sxs-lookup"><span data-stu-id="a8152-124">Install adal.js</span></span>
<span data-ttu-id="a8152-125">Starta genom att gå till projektet du hämtade och installera adal.js.</span><span class="sxs-lookup"><span data-stu-id="a8152-125">To start, navigate to project you downloaded and install adal.js.</span></span>  <span data-ttu-id="a8152-126">Om du har [bower](http://bower.io/) installerat, du kan bara köra det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="a8152-126">If you have [bower](http://bower.io/) installed, you can just run this command.</span></span>  <span data-ttu-id="a8152-127">Välj den högre versionen för alla beroende version avvikelser.</span><span class="sxs-lookup"><span data-stu-id="a8152-127">For any dependency version mismatches, just choose the higher version.</span></span>

```
bower install adal-angular#experimental
```

<span data-ttu-id="a8152-128">Alternativt kan du manuellt hämta [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) och [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span><span class="sxs-lookup"><span data-stu-id="a8152-128">Alternatively, you can manually download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) and [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span></span>  <span data-ttu-id="a8152-129">Lägg till båda filer i den `app/lib/adal-angular-experimental/dist` directory.</span><span class="sxs-lookup"><span data-stu-id="a8152-129">Add both files to the `app/lib/adal-angular-experimental/dist` directory.</span></span>

<span data-ttu-id="a8152-130">Öppna projektet i valfri textredigerare och läsa in adal.js i slutet av sidan brödtext:</span><span class="sxs-lookup"><span data-stu-id="a8152-130">Now open the project in your favorite text editor, and load adal.js at the end of the page body:</span></span>

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a><span data-ttu-id="a8152-131">Konfigurera REST API</span><span class="sxs-lookup"><span data-stu-id="a8152-131">Set up the REST API</span></span>
<span data-ttu-id="a8152-132">Medan vi konfigurerar saker, kan du få fungerande för backend-REST API.</span><span class="sxs-lookup"><span data-stu-id="a8152-132">While we're setting things up, lets get the backend REST API working.</span></span>  <span data-ttu-id="a8152-133">Installera de nödvändiga paketen i en kommandotolk genom att köra (Kontrollera att du befinner dig i den översta katalogen i projektet):</span><span class="sxs-lookup"><span data-stu-id="a8152-133">In a command prompt, install all the necessary packages by running (make sure you're in the top-level directory of the project):</span></span>

```
npm install
```

<span data-ttu-id="a8152-134">Öppna `config.js` och Ersätt den `audience` värde:</span><span class="sxs-lookup"><span data-stu-id="a8152-134">Now open `config.js` and replace the `audience` value:</span></span>

```js
exports.creds = {

     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',

     ...
}
```

<span data-ttu-id="a8152-135">REST API använder det här värdet för att validera token som tas emot från appen vinkel på AJAX-begäranden.</span><span class="sxs-lookup"><span data-stu-id="a8152-135">The REST API will use this value to validate tokens it receives from the Angular app on AJAX requests.</span></span>  <span data-ttu-id="a8152-136">Observera att detta enkla REST-API lagrar data i minnet - så varje tid för att stoppa servern, förlorar du alla tidigare skapade aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a8152-136">Note that this simple REST API stores data in-memory - so each time to stop the server, you will lose all previously created tasks.</span></span>

<span data-ttu-id="a8152-137">Det är den tid som ska vi använda diskutera hur REST API fungerar.</span><span class="sxs-lookup"><span data-stu-id="a8152-137">That's all the time we're going to spend discussing how the REST API works.</span></span>  <span data-ttu-id="a8152-138">Passa på att undersöka i koden, men om du vill veta mer om att skydda webb-API: er med Azure AD, kolla [i den här artikeln](active-directory-v2-devquickstarts-node-api.md).</span><span class="sxs-lookup"><span data-stu-id="a8152-138">Feel free to poke around in the code, but if you want to learn more about securing web APIs with Azure AD, check out [this article](active-directory-v2-devquickstarts-node-api.md).</span></span> 

## <a name="sign-users-in"></a><span data-ttu-id="a8152-139">Logga in användare</span><span class="sxs-lookup"><span data-stu-id="a8152-139">Sign users in</span></span>
<span data-ttu-id="a8152-140">Tid för att skriva kod identitet.</span><span class="sxs-lookup"><span data-stu-id="a8152-140">Time to write some identity code.</span></span>  <span data-ttu-id="a8152-141">Du kanske redan har upptäckt att adal.js innehåller en AngularJS-provider som spelas snyggt vinkel routning mekanismer.</span><span class="sxs-lookup"><span data-stu-id="a8152-141">You might have already noticed that adal.js contains an AngularJS provider, which plays nicely with Angular routing mechanisms.</span></span>  <span data-ttu-id="a8152-142">Starta genom att lägga till modulen adal appen:</span><span class="sxs-lookup"><span data-stu-id="a8152-142">Start by adding the adal module to the app:</span></span>

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

<span data-ttu-id="a8152-143">Du kan nu initiera den `adalProvider` med program-ID:</span><span class="sxs-lookup"><span data-stu-id="a8152-143">You can now initialize the `adalProvider` with your Application ID:</span></span>

```js
// app/scripts/app.js

...

adalProvider.init({

        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 

        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',

        // Your application id from the registration portal
        clientId: '<Your-application-id>',

        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',

    }, $httpProvider);
```

<span data-ttu-id="a8152-144">Bra, nu adal.js har all information som behövs för att skydda din app och inloggning av användare.</span><span class="sxs-lookup"><span data-stu-id="a8152-144">Great, now adal.js has all the information it needs to secure your app and sign users in.</span></span>  <span data-ttu-id="a8152-145">Om du vill tvinga inloggningen för ett visst flöde i appen, behöver du bara en rad med kod:</span><span class="sxs-lookup"><span data-stu-id="a8152-145">To force sign in for a particular route in the app, all it takes is one line of code:</span></span>

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

<span data-ttu-id="a8152-146">Nu när användaren klickar på `TodoList` länk adal.js omdirigeras automatiskt till Azure AD för inloggning om det behövs.</span><span class="sxs-lookup"><span data-stu-id="a8152-146">Now when a user clicks the `TodoList` link, adal.js will automatically redirect to Azure AD for sign-in if necessary.</span></span>  <span data-ttu-id="a8152-147">Du kan också explicit skicka inloggning och utloggning begäranden genom att anropa adal.js i dina domänkontrollanter:</span><span class="sxs-lookup"><span data-stu-id="a8152-147">You can also explicitly send sign-in and sign-out requests by invoking adal.js in your controllers:</span></span>

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {

        // Redirect the user to sign in
        adalService.login();

    };
    $scope.logout = function () {

        // Redirect the user to log out    
        adalService.logOut();

    };
...
```

## <a name="display-user-info"></a><span data-ttu-id="a8152-148">Visa användarinformation</span><span class="sxs-lookup"><span data-stu-id="a8152-148">Display user info</span></span>
<span data-ttu-id="a8152-149">Nu när användaren är inloggad, måste du förmodligen att komma åt den inloggade användarens autentiseringsdata i ditt program.</span><span class="sxs-lookup"><span data-stu-id="a8152-149">Now that the user is signed in, you'll probably need to access the signed-in user's authentication data in your application.</span></span>  <span data-ttu-id="a8152-150">Adal.js visar den här informationen för dig i den `userInfo` objekt.</span><span class="sxs-lookup"><span data-stu-id="a8152-150">Adal.js exposes this information for you in the `userInfo` object.</span></span>  <span data-ttu-id="a8152-151">För att komma åt det här objektet i en vy, först lägga till adal.js rot omfånget för motsvarande domänkontrollant:</span><span class="sxs-lookup"><span data-stu-id="a8152-151">To access this object in a view, first add adal.js to the root scope of the corresponding controller:</span></span>

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

<span data-ttu-id="a8152-152">Sedan direkt kan du lösa det `userInfo` objekt i vyn:</span><span class="sxs-lookup"><span data-stu-id="a8152-152">Then you can directly address the `userInfo` object in your view:</span></span> 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

<span data-ttu-id="a8152-153">Du kan också använda den `userInfo` objektet för att avgöra om användaren är inloggad eller inte.</span><span class="sxs-lookup"><span data-stu-id="a8152-153">You can also use the `userInfo` object to determine if the user is signed in or not.</span></span>

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a><span data-ttu-id="a8152-154">Anropa REST-API</span><span class="sxs-lookup"><span data-stu-id="a8152-154">Call the REST API</span></span>
<span data-ttu-id="a8152-155">Slutligen är det dags att hämta en token och anropa REST-API för att skapa, läsa, uppdatera och ta bort uppgifter.</span><span class="sxs-lookup"><span data-stu-id="a8152-155">Finally, it's time to get some tokens and call the REST API to create, read, update, and delete tasks.</span></span>  <span data-ttu-id="a8152-156">Väl vet du vad?</span><span class="sxs-lookup"><span data-stu-id="a8152-156">Well guess what?</span></span>  <span data-ttu-id="a8152-157">Du behöver inte *något*.</span><span class="sxs-lookup"><span data-stu-id="a8152-157">You don't have to do *a thing*.</span></span>  <span data-ttu-id="a8152-158">Adal.js hand automatiskt tar om komma cachelagring och uppdatera token.</span><span class="sxs-lookup"><span data-stu-id="a8152-158">Adal.js will automatically take care of getting, caching, and refreshing tokens.</span></span>  <span data-ttu-id="a8152-159">Den också hand tar om att de tokens till utgående AJAX-begäranden som du skickar till REST API.</span><span class="sxs-lookup"><span data-stu-id="a8152-159">It will also take care of attaching those tokens to outgoing AJAX requests that you send to the REST API.</span></span>  

<span data-ttu-id="a8152-160">Hur fungerar det?</span><span class="sxs-lookup"><span data-stu-id="a8152-160">How exactly does this work?</span></span> <span data-ttu-id="a8152-161">Det är alla tack vare Magiskt tal för [AngularJS interceptorerna](https://docs.angularjs.org/api/ng/service/$http), vilket gör att adal.js att omvandla inkommande och utgående HTTP-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a8152-161">It's all thanks to the magic of [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), which allows adal.js to transform outgoing and incoming http messages.</span></span>  <span data-ttu-id="a8152-162">Adal.js förutsätter dessutom att skicka förfrågningar till samma domän som fönstret ska använda token är avsedda för samma program-ID som AngularJS-app.</span><span class="sxs-lookup"><span data-stu-id="a8152-162">Furthermore, adal.js assumes that any requests send to the same domain as the window should use tokens intended for the same Application ID as the AngularJS app.</span></span>  <span data-ttu-id="a8152-163">Det är därför vi används samma program-ID i vinkel appen och i NodeJS REST API.</span><span class="sxs-lookup"><span data-stu-id="a8152-163">This is why we used the same Application ID in both the Angular app and in the NodeJS REST API.</span></span>  <span data-ttu-id="a8152-164">Du kan åsidosätta detta beteende och berätta adal.js att hämta token för andra REST API: er om det behövs - men med det här enkla scenariot standardvärdena gör.</span><span class="sxs-lookup"><span data-stu-id="a8152-164">Of course, you can override this behavior and tell adal.js to get tokens for other REST APIs if necessary - but for this simple scenario the defaults will do.</span></span>

<span data-ttu-id="a8152-165">Här är ett kodfragment som visar hur lätt det är att skicka begäranden med ägar-token från Azure AD:</span><span class="sxs-lookup"><span data-stu-id="a8152-165">Here's a snippet that shows how easy it is to send requests with bearer tokens from Azure AD:</span></span>

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

<span data-ttu-id="a8152-166">Grattis!</span><span class="sxs-lookup"><span data-stu-id="a8152-166">Congratulations!</span></span>  <span data-ttu-id="a8152-167">Appen Azure AD-integrerade sida är slutförd.</span><span class="sxs-lookup"><span data-stu-id="a8152-167">Your Azure AD integrated single page app is now complete.</span></span>  <span data-ttu-id="a8152-168">Gå vidare, bugar.</span><span class="sxs-lookup"><span data-stu-id="a8152-168">Go ahead, take a bow.</span></span>  <span data-ttu-id="a8152-169">Den kan autentisera användare, på ett säkert sätt anropa sina serverdelens REST-API med OpenID Connect och få grundläggande information om användaren.</span><span class="sxs-lookup"><span data-stu-id="a8152-169">It can authenticate users, securely call its backend REST API using OpenID Connect, and get basic information about the user.</span></span>  <span data-ttu-id="a8152-170">Out of box, den har stöd för alla användare med ett personligt Microsoft-Account eller arbete/skolkonto från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8152-170">Out of the box, it supports any user with a personal Microsoft Account or a work/school account from Azure AD.</span></span>  <span data-ttu-id="a8152-171">Pröva appen genom att köra:</span><span class="sxs-lookup"><span data-stu-id="a8152-171">Give the app a try by running:</span></span>

```
node server.js
```

<span data-ttu-id="a8152-172">I en webbläsare navigerar du till `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a8152-172">In a browser navigate to `http://localhost:8080`.</span></span>  <span data-ttu-id="a8152-173">Logga in med ett personligt microsoftkonto eller arbete/skolkonto.</span><span class="sxs-lookup"><span data-stu-id="a8152-173">Sign in using either a personal Microsoft account or a work/school account.</span></span>  <span data-ttu-id="a8152-174">Lägg till aktiviteter i användarens att göra-lista och logga ut.</span><span class="sxs-lookup"><span data-stu-id="a8152-174">Add tasks to the user's to-do list, and sign out.</span></span>  <span data-ttu-id="a8152-175">Försök att logga in med typ av konto.</span><span class="sxs-lookup"><span data-stu-id="a8152-175">Try signing in with the other type of account.</span></span> <span data-ttu-id="a8152-176">Om du behöver en Azure AD-klient för att skapa arbete/skola användare [Lär dig hur du skaffa en här](active-directory-howto-tenant.md) (det är ledigt).</span><span class="sxs-lookup"><span data-stu-id="a8152-176">If you need an Azure AD tenant to create work/school users, [learn how to get one here](active-directory-howto-tenant.md) (it's free).</span></span>

<span data-ttu-id="a8152-177">Fortsätta lära dig mer om den v2.0-slutpunkten head tillbaka till våra [v2.0 Utvecklarhandbok](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a8152-177">To continue learning about the the v2.0 endpoint, head back to our [v2.0 developer guide](active-directory-appmodel-v2-overview.md).</span></span>  <span data-ttu-id="a8152-178">För ytterligare resurser, kolla:</span><span class="sxs-lookup"><span data-stu-id="a8152-178">For additional resources, check out:</span></span>

* [<span data-ttu-id="a8152-179">Azure-exemplen på GitHub >></span><span class="sxs-lookup"><span data-stu-id="a8152-179">Azure-Samples on GitHub >></span></span>](https://github.com/Azure-Samples)
* [<span data-ttu-id="a8152-180">Azure AD på stackspill >></span><span class="sxs-lookup"><span data-stu-id="a8152-180">Azure AD on Stack Overflow >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* <span data-ttu-id="a8152-181">Azure AD-dokumentationen på [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span><span class="sxs-lookup"><span data-stu-id="a8152-181">Azure AD documentation on [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span></span>

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="a8152-182">Hämta säkerhetsuppdateringar för våra produkter</span><span class="sxs-lookup"><span data-stu-id="a8152-182">Get security updates for our products</span></span>
<span data-ttu-id="a8152-183">Vi rekommenderar att du aktiverar aviseringar om säkerhetsincidenter genom att gå till [den här sidan](https://technet.microsoft.com/security/dd252948) och prenumerera på Microsoft Security Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="a8152-183">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>


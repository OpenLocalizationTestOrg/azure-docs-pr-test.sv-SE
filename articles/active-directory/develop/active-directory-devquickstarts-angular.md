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
# <a name="help-secure-angularjs-single-page-apps-by-using-azure-ad"></a><span data-ttu-id="ae21b-103">Skydda AngularJS sida appar med hjälp av Azure AD</span><span class="sxs-lookup"><span data-stu-id="ae21b-103">Help secure AngularJS single-page apps by using Azure AD</span></span>

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="ae21b-104">Azure Active Directory (AD Azure) gör det lätt för du tooadd inloggning, utloggning, och säker OAuth-API-anrop tooyour sida appar.</span><span class="sxs-lookup"><span data-stu-id="ae21b-104">Azure Active Directory (Azure AD) makes it simple and straightforward for you tooadd sign-in, sign-out, and secure OAuth API calls tooyour single-page apps.</span></span>  <span data-ttu-id="ae21b-105">Det gör att dina appar tooauthenticate användare med sina Windows Server Active Directory-konton och använda en webb-API som Azure AD hjälper till att skydda, till exempel hello Office 365-API: er eller hello Azure API.</span><span class="sxs-lookup"><span data-stu-id="ae21b-105">It enables your apps tooauthenticate users with their Windows Server Active Directory accounts and consume any web API that Azure AD helps protect, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="ae21b-106">För JavaScript-program som körs i en webbläsare, Azure AD innehåller hello Active Directory Authentication Library (ADAL) eller adal.js.</span><span class="sxs-lookup"><span data-stu-id="ae21b-106">For JavaScript applications running in a browser, Azure AD provides hello Active Directory Authentication Library (ADAL), or adal.js.</span></span> <span data-ttu-id="ae21b-107">hello uteslutande för adal.js är toomake det enkelt för din app tooget åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="ae21b-107">hello sole purpose of adal.js is toomake it easy for your app tooget access tokens.</span></span> <span data-ttu-id="ae21b-108">toodemonstrate hur lätt det är här vi ska skapa en AngularJS tooDo lista över program som:</span><span class="sxs-lookup"><span data-stu-id="ae21b-108">toodemonstrate just how easy it is, here we'll build an AngularJS tooDo List application that:</span></span>

* <span data-ttu-id="ae21b-109">Loggar hello användare i toohello app med Azure AD som identitetsprovider hello.</span><span class="sxs-lookup"><span data-stu-id="ae21b-109">Signs hello user in toohello app by using Azure AD as hello identity provider.</span></span>

* <span data-ttu-id="ae21b-110">Visar information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="ae21b-110">Displays some information about hello user.</span></span>
* <span data-ttu-id="ae21b-111">På ett säkert sätt hello anrop appens tooDo lista API med hjälp av ägar-token från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae21b-111">Securely calls hello app's tooDo List API by using bearer tokens from Azure AD.</span></span>
* <span data-ttu-id="ae21b-112">Loggar hello användare utanför hello appen.</span><span class="sxs-lookup"><span data-stu-id="ae21b-112">Signs hello user out of hello app.</span></span>

<span data-ttu-id="ae21b-113">toobuild hello klar fungerande program, måste du:</span><span class="sxs-lookup"><span data-stu-id="ae21b-113">toobuild hello complete, working application, you need to:</span></span>

1. <span data-ttu-id="ae21b-114">Registrera din app med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae21b-114">Register your app with Azure AD.</span></span>
2. <span data-ttu-id="ae21b-115">Installera ADAL och konfigurera hello sida app.</span><span class="sxs-lookup"><span data-stu-id="ae21b-115">Install ADAL and configure hello single-page app.</span></span>
3. <span data-ttu-id="ae21b-116">Använda ADAL toohelp säkra sidor i hello sida app.</span><span class="sxs-lookup"><span data-stu-id="ae21b-116">Use ADAL toohelp secure pages in hello single-page app.</span></span>

<span data-ttu-id="ae21b-117">tooget har startats [hämta hello app stommen](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="ae21b-117">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="ae21b-118">Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="ae21b-118">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="ae21b-119">Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="ae21b-119">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-hello-directorysearcher-application"></a><span data-ttu-id="ae21b-120">Steg 1: Registrera hello DirectorySearcher program</span><span class="sxs-lookup"><span data-stu-id="ae21b-120">Step 1: Register hello DirectorySearcher application</span></span>
<span data-ttu-id="ae21b-121">tooenable app tooauthenticate användare och hämta token, måste du först tooregister den i din Azure AD-klient:</span><span class="sxs-lookup"><span data-stu-id="ae21b-121">tooenable your app tooauthenticate users and get tokens, you first need tooregister it in your Azure AD tenant:</span></span>

1. <span data-ttu-id="ae21b-122">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ae21b-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ae21b-123">Om du är inloggad toomultiple kataloger eventuellt tooensure du visar hello rätt katalog.</span><span class="sxs-lookup"><span data-stu-id="ae21b-123">If you are signed in toomultiple directories, you may need tooensure you are viewing hello correct directory.</span></span> <span data-ttu-id="ae21b-124">toodo på hello översta fältet, klicka på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="ae21b-124">toodo so, on hello top bar, click your account.</span></span> <span data-ttu-id="ae21b-125">Under hello **Directory** Välj hello Azure AD-klient där du vill att tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="ae21b-125">Under hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="ae21b-126">Klicka på **fler tjänster** i hello till vänster och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ae21b-126">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="ae21b-127">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ae21b-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="ae21b-128">Följ anvisningarna för hello och skapa ett nytt webbprogram och/eller webb-API:</span><span class="sxs-lookup"><span data-stu-id="ae21b-128">Follow hello prompts and create a new web application and/or web API:</span></span>
  * <span data-ttu-id="ae21b-129">**Namnet** beskriver toousers ditt program.</span><span class="sxs-lookup"><span data-stu-id="ae21b-129">**Name** describes your application toousers.</span></span>
  * <span data-ttu-id="ae21b-130">**Omdirigerings-Uri** är hello plats toowhich Azure AD som returneras av token.</span><span class="sxs-lookup"><span data-stu-id="ae21b-130">**Redirect Uri** is hello location toowhich Azure AD will return tokens.</span></span> <span data-ttu-id="ae21b-131">hello standardplatsen för det här exemplet är `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="ae21b-131">hello default location for this sample is `https://localhost:44326/`.</span></span>
6. <span data-ttu-id="ae21b-132">När du har slutfört registreringen tilldelar en unik program-ID tooyour app i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae21b-132">After you finish registration, Azure AD assigns a unique application ID tooyour app.</span></span>  <span data-ttu-id="ae21b-133">Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello programfliken.</span><span class="sxs-lookup"><span data-stu-id="ae21b-133">You'll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="ae21b-134">Adal.js använder hello OAuth implicita flödet toocommunicate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ae21b-134">Adal.js uses hello OAuth implicit flow toocommunicate with Azure AD.</span></span> <span data-ttu-id="ae21b-135">Du måste aktivera hello implicita flödet för programmet:</span><span class="sxs-lookup"><span data-stu-id="ae21b-135">You must enable hello implicit flow for your application:</span></span>
  1. <span data-ttu-id="ae21b-136">Klicka på hello programmet och välj **Manifest** tooopen hello infogade manifestet editor.</span><span class="sxs-lookup"><span data-stu-id="ae21b-136">Click hello application and select **Manifest** tooopen hello inline manifest editor.</span></span>
  2. <span data-ttu-id="ae21b-137">Leta upp hello `oauth2AllowImplicitFlow` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ae21b-137">Locate hello `oauth2AllowImplicitFlow` property.</span></span> <span data-ttu-id="ae21b-138">Ange värdet för`true`.</span><span class="sxs-lookup"><span data-stu-id="ae21b-138">Set its value too`true`.</span></span>
  3. <span data-ttu-id="ae21b-139">Klicka på **spara** toosave hello manifest.</span><span class="sxs-lookup"><span data-stu-id="ae21b-139">Click **Save** toosave hello manifest.</span></span>
8. <span data-ttu-id="ae21b-140">Ge din klient för ditt program.</span><span class="sxs-lookup"><span data-stu-id="ae21b-140">Grant permissions across your tenant for your application.</span></span> <span data-ttu-id="ae21b-141">Gå för**inställningar** > **egenskaper** > **nödvändiga behörigheter**, och klicka på hello **bevilja behörigheter**knappen hello översta raden.</span><span class="sxs-lookup"><span data-stu-id="ae21b-141">Go too**Settings** > **Properties** > **Required Permissions**, and click hello **Grant Permissions** button on hello top bar.</span></span> <span data-ttu-id="ae21b-142">Klicka på **Ja** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="ae21b-142">Click **Yes** tooconfirm.</span></span>

## <a name="step-2-install-adal-and-configure-hello-single-page-app"></a><span data-ttu-id="ae21b-143">Steg 2: Installera ADAL och konfigurera hello sida app</span><span class="sxs-lookup"><span data-stu-id="ae21b-143">Step 2: Install ADAL and configure hello single-page app</span></span>
<span data-ttu-id="ae21b-144">Nu när du har ett program i Azure AD kan du installera adal.js och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="ae21b-144">Now that you have an application in Azure AD, you can install adal.js and write your identity-related code.</span></span>

### <a name="configure-hello-javascript-client"></a><span data-ttu-id="ae21b-145">Konfigurera hello JavaScript-klienten</span><span class="sxs-lookup"><span data-stu-id="ae21b-145">Configure hello JavaScript client</span></span>
<span data-ttu-id="ae21b-146">Börja genom att lägga till adal.js toohello TodoSPA projekt med hjälp av hello Package Manager-konsolen:</span><span class="sxs-lookup"><span data-stu-id="ae21b-146">Begin by adding adal.js toohello TodoSPA project by using hello Package Manager Console:</span></span>
  1. <span data-ttu-id="ae21b-147">Hämta [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) och lägga till den toohello `App/Scripts/` projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="ae21b-147">Download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) and add it toohello `App/Scripts/` project directory.</span></span>
  2. <span data-ttu-id="ae21b-148">Hämta [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) och lägga till den toohello `App/Scripts/` projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="ae21b-148">Download [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) and add it toohello `App/Scripts/` project directory.</span></span>
  3. <span data-ttu-id="ae21b-149">Läsa in varje skript före hello slut hello `</body>` i `index.html`:</span><span class="sxs-lookup"><span data-stu-id="ae21b-149">Load each script before hello end of hello `</body>` in `index.html`:</span></span>

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-hello-back-end-server"></a><span data-ttu-id="ae21b-150">Konfigurera hello backend-servern</span><span class="sxs-lookup"><span data-stu-id="ae21b-150">Configure hello back end server</span></span>
<span data-ttu-id="ae21b-151">För hello sida appens serverdel tooDo lista API tooaccept token från hello webbläsare måste hello serverdel konfigurationsinformation om hello appregistrering.</span><span class="sxs-lookup"><span data-stu-id="ae21b-151">For hello single-page app's back-end tooDo List API tooaccept tokens from hello browser, hello back end needs configuration information about hello app registration.</span></span> <span data-ttu-id="ae21b-152">Öppna i hello TodoSPA project `web.config`.</span><span class="sxs-lookup"><span data-stu-id="ae21b-152">In hello TodoSPA project, open `web.config`.</span></span> <span data-ttu-id="ae21b-153">Ersätt hello värdena för hello element i hello `<appSettings>` avsnittet tooreflect hello värden som du använde i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ae21b-153">Replace hello values of hello elements in hello `<appSettings>` section tooreflect hello values that you used in hello Azure portal.</span></span> <span data-ttu-id="ae21b-154">Koden ska referera till dessa värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="ae21b-154">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="ae21b-155">`ida:Tenant`är hello domän för din Azure AD-klient – till exempel contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="ae21b-155">`ida:Tenant` is hello domain of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="ae21b-156">`ida:Audience`är hello klient-ID för programmet som du kopierade från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="ae21b-156">`ida:Audience` is hello client ID of your application that you copied from hello portal.</span></span>

## <a name="step-3-use-adal-toohelp-secure-pages-in-hello-single-page-app"></a><span data-ttu-id="ae21b-157">Steg 3: Använda ADAL toohelp säkra sidor i hello sida app</span><span class="sxs-lookup"><span data-stu-id="ae21b-157">Step 3: Use ADAL toohelp secure pages in hello single-page app</span></span>
<span data-ttu-id="ae21b-158">Adal.js integreras med AngularJS flödes- och HTTP-providers, så kan du säkra enskilda vyer i din app på en sida.</span><span class="sxs-lookup"><span data-stu-id="ae21b-158">Adal.js integrates with AngularJS route and HTTP providers, so you can help secure individual views in your single-page app.</span></span>

1. <span data-ttu-id="ae21b-159">I `App/Scripts/app.js`, hämtas hello adal.js modulen:</span><span class="sxs-lookup"><span data-stu-id="ae21b-159">In `App/Scripts/app.js`, bring in hello adal.js module:</span></span>

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. <span data-ttu-id="ae21b-160">Initiera `adalProvider` med hello konfigurationsvärden för programmet registreringen, även i `App/Scripts/app.js`:</span><span class="sxs-lookup"><span data-stu-id="ae21b-160">Initialize `adalProvider` by using hello configuration values of your application registration, also in `App/Scripts/app.js`:</span></span>

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
3. <span data-ttu-id="ae21b-161">Att säkra hello `TodoList` vyn i hello app med hjälp av en enda rad med kod: `requireADLogin`.</span><span class="sxs-lookup"><span data-stu-id="ae21b-161">Help secure hello `TodoList` view in hello app by using only one line of code: `requireADLogin`.</span></span>

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a><span data-ttu-id="ae21b-162">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ae21b-162">Summary</span></span>
<span data-ttu-id="ae21b-163">Nu har du en säker sida-app som kan logga in användare och utfärda ägar-token-skyddade begäranden tooits backend-API.</span><span class="sxs-lookup"><span data-stu-id="ae21b-163">You now have a secure single-page app that can sign in users and issue bearer-token-protected requests tooits back-end API.</span></span> <span data-ttu-id="ae21b-164">När en användare klickar på hello **TodoList** länk adal.js omdirigeras automatiskt tooAzure AD för inloggning om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ae21b-164">When a user clicks hello **TodoList** link, adal.js will automatically redirect tooAzure AD for sign-in if necessary.</span></span> <span data-ttu-id="ae21b-165">Dessutom kan koppla adal.js automatiskt en åtkomst-token tooany Ajax-begäranden som skickas toohello appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="ae21b-165">In addition, adal.js will automatically attach an access token tooany Ajax requests that are sent toohello app's back end.</span></span>  

<span data-ttu-id="ae21b-166">hello är föregående steg hello utan minsta nödvändiga toobuild en enda sida-app med hjälp av adal.js.</span><span class="sxs-lookup"><span data-stu-id="ae21b-166">hello preceding steps are hello bare minimum necessary toobuild a single-page app by using adal.js.</span></span> <span data-ttu-id="ae21b-167">Men några andra funktioner som är användbara i en sida app:</span><span class="sxs-lookup"><span data-stu-id="ae21b-167">But a few other features are useful in single-page app:</span></span>

* <span data-ttu-id="ae21b-168">tooexplicitly utfärda inloggnings- och utloggningsförfrågningar, kan du ange funktionerna i din domänkontrollanter som anropar adal.js.</span><span class="sxs-lookup"><span data-stu-id="ae21b-168">tooexplicitly issue sign-in and sign-out requests, you can define functions in your controllers that invoke adal.js.</span></span>  <span data-ttu-id="ae21b-169">I `App/Scripts/homeCtrl.js`:</span><span class="sxs-lookup"><span data-stu-id="ae21b-169">In `App/Scripts/homeCtrl.js`:</span></span>

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
* <span data-ttu-id="ae21b-170">Du kanske vill toopresent användarinformation i hello appens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ae21b-170">You might want toopresent user information in hello app's UI.</span></span> <span data-ttu-id="ae21b-171">Hej ADAL-tjänsten har redan lagts toohello `userDataCtrl` styrenhet, så du kan komma åt hello `userInfo` associerad vy, för objekt i hello `App/Views/UserData.html`:</span><span class="sxs-lookup"><span data-stu-id="ae21b-171">hello ADAL service has already been added toohello `userDataCtrl` controller, so you can access hello `userInfo` object in hello associated view, `App/Views/UserData.html`:</span></span>

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* <span data-ttu-id="ae21b-172">Det finns många scenarier där ska du tooknow om hello användaren är inloggad eller inte.</span><span class="sxs-lookup"><span data-stu-id="ae21b-172">There are many scenarios in which you'll want tooknow if hello user is signed in or not.</span></span> <span data-ttu-id="ae21b-173">Du kan också använda hello `userInfo` objekt toogather informationen.</span><span class="sxs-lookup"><span data-stu-id="ae21b-173">You can also use hello `userInfo` object toogather this information.</span></span>  <span data-ttu-id="ae21b-174">Till exempel på `index.html`, du kan visa antingen hello **inloggning** eller **logga ut** knappen baserat på autentiseringsstatus:</span><span class="sxs-lookup"><span data-stu-id="ae21b-174">For instance, in `index.html`, you can show either hello **Login** or **Logout** button based on authentication status:</span></span>

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

<span data-ttu-id="ae21b-175">Din Azure AD-integrerade single-page-app kan autentisera användare, på ett säkert sätt anropa sina serverdelens med hjälp av OAuth 2.0 och få grundläggande information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="ae21b-175">Your Azure AD-integrated single-page app can authenticate users, securely call its back end by using OAuth 2.0, and get basic information about hello user.</span></span> <span data-ttu-id="ae21b-176">Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare.</span><span class="sxs-lookup"><span data-stu-id="ae21b-176">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span> <span data-ttu-id="ae21b-177">Kör appen tooDo lista sida och logga in med något av dessa användare.</span><span class="sxs-lookup"><span data-stu-id="ae21b-177">Run your tooDo List single-page app, and sign in with one of those users.</span></span> <span data-ttu-id="ae21b-178">Lägga till uppgifter toohello användarens att göra-lista, logga ut och logga in igen.</span><span class="sxs-lookup"><span data-stu-id="ae21b-178">Add tasks toohello user's to-do list, sign out, and sign back in.</span></span>

<span data-ttu-id="ae21b-179">Adal.js gör det enkelt tooincorporate vanliga identity-funktioner i programmet.</span><span class="sxs-lookup"><span data-stu-id="ae21b-179">Adal.js makes it easy tooincorporate common identity features into your application.</span></span> <span data-ttu-id="ae21b-180">Den tar hand om alla hello ändrad arbetet åt dig: hantering av cache, OAuth protokollstöd presentera hello användare med en inloggning användargränssnitt, uppdatera token har upphört att gälla och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="ae21b-180">It takes care of all hello dirty work for you: cache management, OAuth protocol support, presenting hello user with a sign-in UI, refreshing expired tokens, and more.</span></span>

<span data-ttu-id="ae21b-181">Referens hello slutförts exemplet (utan dina konfigurationsvärden) är tillgängliga i [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="ae21b-181">For reference, hello completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae21b-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae21b-182">Next steps</span></span>
<span data-ttu-id="ae21b-183">Du kan nu gå vidare tooadditional scenarier.</span><span class="sxs-lookup"><span data-stu-id="ae21b-183">You can now move on tooadditional scenarios.</span></span> <span data-ttu-id="ae21b-184">Du kanske vill tootry: [anropa CORS-webb-API från en enda sida app](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span><span class="sxs-lookup"><span data-stu-id="ae21b-184">You might want tootry: [Call a CORS web API from a single-page app](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

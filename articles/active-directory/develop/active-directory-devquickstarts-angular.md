---
title: "Azure AD AngularJS komma igång | Microsoft Docs"
description: "Hur du skapar en enkel sida AngularJS-program som kan integreras med Azure AD för inloggning och anropar Azure AD-skyddade API: er med hjälp av OAuth."
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
ms.openlocfilehash: 4153910bc03f112f84c26cda6f9c78f11028b934
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="help-secure-angularjs-single-page-apps-by-using-azure-ad"></a><span data-ttu-id="df65b-103">Skydda AngularJS sida appar med hjälp av Azure AD</span><span class="sxs-lookup"><span data-stu-id="df65b-103">Help secure AngularJS single-page apps by using Azure AD</span></span>

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="df65b-104">Azure Active Directory (AD Azure) gör det lätt att lägga till inloggning, utloggning och säker OAuth-API anrop till en sida-appar.</span><span class="sxs-lookup"><span data-stu-id="df65b-104">Azure Active Directory (Azure AD) makes it simple and straightforward for you to add sign-in, sign-out, and secure OAuth API calls to your single-page apps.</span></span>  <span data-ttu-id="df65b-105">Det gör att dina appar att autentisera användare med sina Windows Server Active Directory-konton och använda en webb-API som Azure AD hjälper till att skydda, till exempel API: er för Office 365 eller Azure API.</span><span class="sxs-lookup"><span data-stu-id="df65b-105">It enables your apps to authenticate users with their Windows Server Active Directory accounts and consume any web API that Azure AD helps protect, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="df65b-106">För JavaScript-program som körs i en webbläsare, innehåller Azure AD Active Directory Authentication Library (ADAL) eller adal.js.</span><span class="sxs-lookup"><span data-stu-id="df65b-106">For JavaScript applications running in a browser, Azure AD provides the Active Directory Authentication Library (ADAL), or adal.js.</span></span> <span data-ttu-id="df65b-107">Det enda syftet med adal.js är att göra det lättare för din app för att få åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="df65b-107">The sole purpose of adal.js is to make it easy for your app to get access tokens.</span></span> <span data-ttu-id="df65b-108">För att demonstrera hur lätt det är ska här vi skapa ett program med AngularJS att göra-lista som:</span><span class="sxs-lookup"><span data-stu-id="df65b-108">To demonstrate just how easy it is, here we'll build an AngularJS To Do List application that:</span></span>

* <span data-ttu-id="df65b-109">Loggar användaren in i appen med Azure AD som identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="df65b-109">Signs the user in to the app by using Azure AD as the identity provider.</span></span>

* <span data-ttu-id="df65b-110">Visar information om användaren.</span><span class="sxs-lookup"><span data-stu-id="df65b-110">Displays some information about the user.</span></span>
* <span data-ttu-id="df65b-111">Anropar appens att göra listan API: N på ett säkert sätt med hjälp av ägar-token från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="df65b-111">Securely calls the app's To Do List API by using bearer tokens from Azure AD.</span></span>
* <span data-ttu-id="df65b-112">Loggar du ut från appen.</span><span class="sxs-lookup"><span data-stu-id="df65b-112">Signs the user out of the app.</span></span>

<span data-ttu-id="df65b-113">Om du vill skapa klar fungerande program måste du:</span><span class="sxs-lookup"><span data-stu-id="df65b-113">To build the complete, working application, you need to:</span></span>

1. <span data-ttu-id="df65b-114">Registrera din app med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="df65b-114">Register your app with Azure AD.</span></span>
2. <span data-ttu-id="df65b-115">Installera ADAL och konfigurera en sida-app.</span><span class="sxs-lookup"><span data-stu-id="df65b-115">Install ADAL and configure the single-page app.</span></span>
3. <span data-ttu-id="df65b-116">Använd ADAL för att säkra sidor i en sida-app.</span><span class="sxs-lookup"><span data-stu-id="df65b-116">Use ADAL to help secure pages in the single-page app.</span></span>

<span data-ttu-id="df65b-117">Du kommer igång [ladda ned stommen app](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) eller [hämta det slutförda exemplet](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="df65b-117">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="df65b-118">Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="df65b-118">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="df65b-119">Om du inte redan har en klient [Lär dig hur du skaffa en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="df65b-119">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-the-directorysearcher-application"></a><span data-ttu-id="df65b-120">Steg 1: Registrera DirectorySearcher-program</span><span class="sxs-lookup"><span data-stu-id="df65b-120">Step 1: Register the DirectorySearcher application</span></span>
<span data-ttu-id="df65b-121">Om du vill aktivera din app för att autentisera användare och hämta token måste du först registrera det i Azure AD-klient:</span><span class="sxs-lookup"><span data-stu-id="df65b-121">To enable your app to authenticate users and get tokens, you first need to register it in your Azure AD tenant:</span></span>

1. <span data-ttu-id="df65b-122">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="df65b-122">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="df65b-123">Om du är inloggad på flera kataloger kan du behöva se till att du visar rätt katalog.</span><span class="sxs-lookup"><span data-stu-id="df65b-123">If you are signed in to multiple directories, you may need to ensure you are viewing the correct directory.</span></span> <span data-ttu-id="df65b-124">Om du vill göra det, på den översta raden, klickar du på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="df65b-124">To do so, on the top bar, click your account.</span></span> <span data-ttu-id="df65b-125">Under den **Directory** Välj Azure AD-klient som du vill registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="df65b-125">Under the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>
3. <span data-ttu-id="df65b-126">Klicka på **fler tjänster** i det vänstra fönstret och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="df65b-126">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="df65b-127">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="df65b-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="df65b-128">Följ anvisningarna och skapa ett nytt webbprogram och/eller webb-API:</span><span class="sxs-lookup"><span data-stu-id="df65b-128">Follow the prompts and create a new web application and/or web API:</span></span>
  * <span data-ttu-id="df65b-129">**Namnet** beskriver ditt program till användare.</span><span class="sxs-lookup"><span data-stu-id="df65b-129">**Name** describes your application to users.</span></span>
  * <span data-ttu-id="df65b-130">**Omdirigerings-Uri** är den plats som Azure AD som returneras av token.</span><span class="sxs-lookup"><span data-stu-id="df65b-130">**Redirect Uri** is the location to which Azure AD will return tokens.</span></span> <span data-ttu-id="df65b-131">Standardplatsen för det här exemplet är `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="df65b-131">The default location for this sample is `https://localhost:44326/`.</span></span>
6. <span data-ttu-id="df65b-132">När du har slutfört registreringen tilldelar ett unikt ID i Azure AD till din app.</span><span class="sxs-lookup"><span data-stu-id="df65b-132">After you finish registration, Azure AD assigns a unique application ID to your app.</span></span>  <span data-ttu-id="df65b-133">Du behöver det här värdet i nästa avsnitt så kopiera den från programfliken.</span><span class="sxs-lookup"><span data-stu-id="df65b-133">You'll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="df65b-134">Adal.js använder implicita flödet för OAuth för att kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="df65b-134">Adal.js uses the OAuth implicit flow to communicate with Azure AD.</span></span> <span data-ttu-id="df65b-135">Du måste aktivera det implicita flödet för programmet:</span><span class="sxs-lookup"><span data-stu-id="df65b-135">You must enable the implicit flow for your application:</span></span>
  1. <span data-ttu-id="df65b-136">Klicka på programmet och välj **Manifest** att öppna redigeraren infogade manifestet.</span><span class="sxs-lookup"><span data-stu-id="df65b-136">Click the application and select **Manifest** to open the inline manifest editor.</span></span>
  2. <span data-ttu-id="df65b-137">Leta upp den `oauth2AllowImplicitFlow` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="df65b-137">Locate the `oauth2AllowImplicitFlow` property.</span></span> <span data-ttu-id="df65b-138">Ange värdet till `true`.</span><span class="sxs-lookup"><span data-stu-id="df65b-138">Set its value to `true`.</span></span>
  3. <span data-ttu-id="df65b-139">Klicka på **spara** spara manifestet.</span><span class="sxs-lookup"><span data-stu-id="df65b-139">Click **Save** to save the manifest.</span></span>
8. <span data-ttu-id="df65b-140">Ge din klient för ditt program.</span><span class="sxs-lookup"><span data-stu-id="df65b-140">Grant permissions across your tenant for your application.</span></span> <span data-ttu-id="df65b-141">Gå till **inställningar** > **egenskaper** > **nödvändiga behörigheter**, och klicka på den **bevilja med** knappen i det översta fältet.</span><span class="sxs-lookup"><span data-stu-id="df65b-141">Go to **Settings** > **Properties** > **Required Permissions**, and click the **Grant Permissions** button on the top bar.</span></span> <span data-ttu-id="df65b-142">Bekräfta genom att klicka på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="df65b-142">Click **Yes** to confirm.</span></span>

## <a name="step-2-install-adal-and-configure-the-single-page-app"></a><span data-ttu-id="df65b-143">Steg 2: Installera ADAL och konfigurera en sida-app</span><span class="sxs-lookup"><span data-stu-id="df65b-143">Step 2: Install ADAL and configure the single-page app</span></span>
<span data-ttu-id="df65b-144">Nu när du har ett program i Azure AD kan du installera adal.js och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="df65b-144">Now that you have an application in Azure AD, you can install adal.js and write your identity-related code.</span></span>

### <a name="configure-the-javascript-client"></a><span data-ttu-id="df65b-145">Konfigurera JavaScript-klienten</span><span class="sxs-lookup"><span data-stu-id="df65b-145">Configure the JavaScript client</span></span>
<span data-ttu-id="df65b-146">Börja genom att lägga till adal.js TodoSPA-projekt med hjälp av Package Manager-konsolen:</span><span class="sxs-lookup"><span data-stu-id="df65b-146">Begin by adding adal.js to the TodoSPA project by using the Package Manager Console:</span></span>
  1. <span data-ttu-id="df65b-147">Hämta [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) och lägger till den i den `App/Scripts/` projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="df65b-147">Download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) and add it to the `App/Scripts/` project directory.</span></span>
  2. <span data-ttu-id="df65b-148">Hämta [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) och lägger till den i den `App/Scripts/` projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="df65b-148">Download [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) and add it to the `App/Scripts/` project directory.</span></span>
  3. <span data-ttu-id="df65b-149">Läsa in varje skript före utgången av den `</body>` i `index.html`:</span><span class="sxs-lookup"><span data-stu-id="df65b-149">Load each script before the end of the `</body>` in `index.html`:</span></span>

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-the-back-end-server"></a><span data-ttu-id="df65b-150">Konfigurera backend-servern</span><span class="sxs-lookup"><span data-stu-id="df65b-150">Configure the back end server</span></span>
<span data-ttu-id="df65b-151">För sida appens backend-att göra listan API att acceptera token från webbläsaren, måste serverdelen konfigurationsinformation om appregistrering.</span><span class="sxs-lookup"><span data-stu-id="df65b-151">For the single-page app's back-end To Do List API to accept tokens from the browser, the back end needs configuration information about the app registration.</span></span> <span data-ttu-id="df65b-152">Öppna i projektet TodoSPA `web.config`.</span><span class="sxs-lookup"><span data-stu-id="df65b-152">In the TodoSPA project, open `web.config`.</span></span> <span data-ttu-id="df65b-153">Ersätt värdena för elementen i den `<appSettings>` avsnittet för att återspegla de värden som du använde i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="df65b-153">Replace the values of the elements in the `<appSettings>` section to reflect the values that you used in the Azure portal.</span></span> <span data-ttu-id="df65b-154">Koden ska referera till dessa värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="df65b-154">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="df65b-155">`ida:Tenant`är domänen för din Azure AD-klient – till exempel contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="df65b-155">`ida:Tenant` is the domain of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="df65b-156">`ida:Audience`är klient-ID för programmet som du kopierade från portalen.</span><span class="sxs-lookup"><span data-stu-id="df65b-156">`ida:Audience` is the client ID of your application that you copied from the portal.</span></span>

## <a name="step-3-use-adal-to-help-secure-pages-in-the-single-page-app"></a><span data-ttu-id="df65b-157">Steg 3: Använda ADAL för att säkra sidor i appen en sida</span><span class="sxs-lookup"><span data-stu-id="df65b-157">Step 3: Use ADAL to help secure pages in the single-page app</span></span>
<span data-ttu-id="df65b-158">Adal.js integreras med AngularJS flödes- och HTTP-providers, så kan du säkra enskilda vyer i din app på en sida.</span><span class="sxs-lookup"><span data-stu-id="df65b-158">Adal.js integrates with AngularJS route and HTTP providers, so you can help secure individual views in your single-page app.</span></span>

1. <span data-ttu-id="df65b-159">I `App/Scripts/app.js`, sätta i modulen adal.js:</span><span class="sxs-lookup"><span data-stu-id="df65b-159">In `App/Scripts/app.js`, bring in the adal.js module:</span></span>

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. <span data-ttu-id="df65b-160">Initiera `adalProvider` med hjälp av konfigurationsvärden för programmet registreringen, även i `App/Scripts/app.js`:</span><span class="sxs-lookup"><span data-stu-id="df65b-160">Initialize `adalProvider` by using the configuration values of your application registration, also in `App/Scripts/app.js`:</span></span>

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
3. <span data-ttu-id="df65b-161">Skydda den `TodoList` visa i appen med hjälp av en enda rad med kod: `requireADLogin`.</span><span class="sxs-lookup"><span data-stu-id="df65b-161">Help secure the `TodoList` view in the app by using only one line of code: `requireADLogin`.</span></span>

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a><span data-ttu-id="df65b-162">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="df65b-162">Summary</span></span>
<span data-ttu-id="df65b-163">Nu har du en säker sida-app som kan logga in användare och utfärda ägar-token-skyddade begäranden till dess backend-API.</span><span class="sxs-lookup"><span data-stu-id="df65b-163">You now have a secure single-page app that can sign in users and issue bearer-token-protected requests to its back-end API.</span></span> <span data-ttu-id="df65b-164">När användaren klickar på **TodoList** länk adal.js omdirigeras automatiskt till Azure AD för inloggning om det behövs.</span><span class="sxs-lookup"><span data-stu-id="df65b-164">When a user clicks the **TodoList** link, adal.js will automatically redirect to Azure AD for sign-in if necessary.</span></span> <span data-ttu-id="df65b-165">Dessutom kommer adal.js automatiskt koppla en åtkomst-token till Ajax-begäranden som skickas till appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="df65b-165">In addition, adal.js will automatically attach an access token to any Ajax requests that are sent to the app's back end.</span></span>  

<span data-ttu-id="df65b-166">Föregående steg är att skapa en enkel sida-app med hjälp av adal.js utan minsta nödvändiga.</span><span class="sxs-lookup"><span data-stu-id="df65b-166">The preceding steps are the bare minimum necessary to build a single-page app by using adal.js.</span></span> <span data-ttu-id="df65b-167">Men några andra funktioner som är användbara i en sida app:</span><span class="sxs-lookup"><span data-stu-id="df65b-167">But a few other features are useful in single-page app:</span></span>

* <span data-ttu-id="df65b-168">Om du vill utfärda explicit logga in och utloggningsförfrågningar, kan du definiera funktioner i din domänkontrollanter som anropar adal.js.</span><span class="sxs-lookup"><span data-stu-id="df65b-168">To explicitly issue sign-in and sign-out requests, you can define functions in your controllers that invoke adal.js.</span></span>  <span data-ttu-id="df65b-169">I `App/Scripts/homeCtrl.js`:</span><span class="sxs-lookup"><span data-stu-id="df65b-169">In `App/Scripts/homeCtrl.js`:</span></span>

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
* <span data-ttu-id="df65b-170">Du kanske vill presentera användarinformation i appens användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="df65b-170">You might want to present user information in the app's UI.</span></span> <span data-ttu-id="df65b-171">ADAL-tjänsten har redan lagts till i `userDataCtrl` styrenhet, så du kan komma åt den `userInfo` objekt i den associerade vyn `App/Views/UserData.html`:</span><span class="sxs-lookup"><span data-stu-id="df65b-171">The ADAL service has already been added to the `userDataCtrl` controller, so you can access the `userInfo` object in the associated view, `App/Views/UserData.html`:</span></span>

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* <span data-ttu-id="df65b-172">Det finns många scenarier där du vill veta om användaren är inloggad eller inte.</span><span class="sxs-lookup"><span data-stu-id="df65b-172">There are many scenarios in which you'll want to know if the user is signed in or not.</span></span> <span data-ttu-id="df65b-173">Du kan också använda den `userInfo` objekt att samla in informationen.</span><span class="sxs-lookup"><span data-stu-id="df65b-173">You can also use the `userInfo` object to gather this information.</span></span>  <span data-ttu-id="df65b-174">Till exempel på `index.html`, du kan visa den **inloggning** eller **logga ut** knappen baserat på autentiseringsstatus:</span><span class="sxs-lookup"><span data-stu-id="df65b-174">For instance, in `index.html`, you can show either the **Login** or **Logout** button based on authentication status:</span></span>

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

<span data-ttu-id="df65b-175">Din Azure AD-integrerade single-page-app kan autentisera användare, på ett säkert sätt anropa sina serverdelens med hjälp av OAuth 2.0 och få grundläggande information om användaren.</span><span class="sxs-lookup"><span data-stu-id="df65b-175">Your Azure AD-integrated single-page app can authenticate users, securely call its back end by using OAuth 2.0, and get basic information about the user.</span></span> <span data-ttu-id="df65b-176">Om du inte redan gjort nu är det dags att fylla i din klient med vissa användare.</span><span class="sxs-lookup"><span data-stu-id="df65b-176">If you haven't already, now is the time to populate your tenant with some users.</span></span> <span data-ttu-id="df65b-177">Kör appen att göra-lista sida och logga in med något av dessa användare.</span><span class="sxs-lookup"><span data-stu-id="df65b-177">Run your To Do List single-page app, and sign in with one of those users.</span></span> <span data-ttu-id="df65b-178">Lägg till aktiviteter i användarens att göra-lista, logga ut och logga in igen.</span><span class="sxs-lookup"><span data-stu-id="df65b-178">Add tasks to the user's to-do list, sign out, and sign back in.</span></span>

<span data-ttu-id="df65b-179">Adal.js gör det enkelt att införliva vanliga identity-funktioner i programmet.</span><span class="sxs-lookup"><span data-stu-id="df65b-179">Adal.js makes it easy to incorporate common identity features into your application.</span></span> <span data-ttu-id="df65b-180">Det tar hand om ändrad arbetet åt dig: hantering av cache, OAuth protokollstöd presentera användaren med en inloggning användargränssnitt, uppdatera token har upphört att gälla och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="df65b-180">It takes care of all the dirty work for you: cache management, OAuth protocol support, presenting the user with a sign-in UI, refreshing expired tokens, and more.</span></span>

<span data-ttu-id="df65b-181">För referens anger det slutförda exemplet (utan dina konfigurationsvärden) är tillgängligt i [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="df65b-181">For reference, the completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span>

## <a name="next-steps"></a><span data-ttu-id="df65b-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df65b-182">Next steps</span></span>
<span data-ttu-id="df65b-183">Du kan nu gå vidare till fler scenarier.</span><span class="sxs-lookup"><span data-stu-id="df65b-183">You can now move on to additional scenarios.</span></span> <span data-ttu-id="df65b-184">Du kanske vill prova: [anropa CORS-webb-API från en enda sida app](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span><span class="sxs-lookup"><span data-stu-id="df65b-184">You might want to try: [Call a CORS web API from a single-page app](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

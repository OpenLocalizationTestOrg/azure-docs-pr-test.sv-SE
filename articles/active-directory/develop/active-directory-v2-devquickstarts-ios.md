---
title: "Lägga till inloggning till en iOS-App med Azure AD v2.0-slutpunkten | Microsoft Docs"
description: "Hur du skapar en iOS-app som loggar in användare med både personliga Microsoft-konto och arbets-eller skolkonton med hjälp av tredjeparts-bibliotek."
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: fd3603c0-42f7-438c-87b5-a52d20d6344b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: cf1455dc3d55ea3581195f7a315556d134c23a26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a><span data-ttu-id="55b66-103">Lägga till inloggning till en iOS-app med hjälp av en tredjeparts-bibliotek med Graph API: et med v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="55b66-103">Add sign-in to an iOS app using a third-party library with Graph API using the v2.0 endpoint</span></span>
<span data-ttu-id="55b66-104">Microsofts identitetsplattform använder öppna standarder som OAuth2 och OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="55b66-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="55b66-105">Utvecklare kan använda alla bibliotek som de vill integrera med våra tjänster.</span><span class="sxs-lookup"><span data-stu-id="55b66-105">Developers can use any library they want to integrate with our services.</span></span> <span data-ttu-id="55b66-106">Vi har skrivit några genomgång som detta att demonstrera hur du konfigurerar tredjeparts-bibliotek för att ansluta till Microsoft identity-plattformen för att hjälpa utvecklare att använda vår plattform med andra bibliotek.</span><span class="sxs-lookup"><span data-stu-id="55b66-106">To help developers use our platform with other libraries, we've written a few walkthroughs like this one to demonstrate how to configure third-party libraries to connect to the Microsoft identity platform.</span></span> <span data-ttu-id="55b66-107">De flesta bibliotek som implementerar [RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kan ansluta till Microsoft identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="55b66-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect to the Microsoft identity platform.</span></span>

<span data-ttu-id="55b66-108">Med det program som skapar den här genomgången, användare logga in i organisationen och söka efter andra i organisationen med hjälp av Graph API.</span><span class="sxs-lookup"><span data-stu-id="55b66-108">With the application that this walkthrough creates, users can sign in to their organization and then search for others in their organization by using the Graph API.</span></span>

<span data-ttu-id="55b66-109">Om du har använt OAuth2 eller OpenID Connect eventuellt mycket av det här exempelkonfiguration ingen vits till dig.</span><span class="sxs-lookup"><span data-stu-id="55b66-109">If you're new to OAuth2 or OpenID Connect, much of this sample configuration may not make sense to you.</span></span> <span data-ttu-id="55b66-110">Vi rekommenderar att du läser [v2.0-protokoll - OAuth 2.0 auktorisering kod flöda](active-directory-v2-protocols-oauth-code.md) för bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="55b66-110">We recommend that you read  [v2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="55b66-111">Vissa funktioner i vår plattform som har ett uttryck i OAuth2 eller OpenID Connect standarder, till exempel villkorlig åtkomst och hantering av Intune måste du använda våra Microsoft Azure identitet bibliotek med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="55b66-111">Some features of our platform that do have an expression in the OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you to use our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="55b66-112">V2.0-slutpunkten har inte stöd för alla Azure Active Directory-scenarier och funktioner.</span><span class="sxs-lookup"><span data-stu-id="55b66-112">The v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="55b66-113">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="55b66-113">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-code-from-github"></a><span data-ttu-id="55b66-114">Hämta koden från GitHub</span><span class="sxs-lookup"><span data-stu-id="55b66-114">Download code from GitHub</span></span>
<span data-ttu-id="55b66-115">Koden för den här självstudiekursen [finns på GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span><span class="sxs-lookup"><span data-stu-id="55b66-115">The code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span></span>  <span data-ttu-id="55b66-116">Om du vill följa med kan du [ladda ned appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) eller klona stommen:</span><span class="sxs-lookup"><span data-stu-id="55b66-116">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

<span data-ttu-id="55b66-117">Du kan också hämta exempelfilerna och komma igång nu direkt:</span><span class="sxs-lookup"><span data-stu-id="55b66-117">You can also just download the sample and get started right away:</span></span>

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="55b66-118">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="55b66-118">Register an app</span></span>
<span data-ttu-id="55b66-119">Skapa en ny app på den [programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följer detaljerade anvisningar på [hur du registrerar en app med v2.0-slutpunkten](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="55b66-119">Create a new app at the [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow the detailed steps at  [How to register an app with the v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="55b66-120">Se till att:</span><span class="sxs-lookup"><span data-stu-id="55b66-120">Make sure to:</span></span>

* <span data-ttu-id="55b66-121">Kopiera den **program-Id** som har tilldelats din app eftersom du behöver den snart.</span><span class="sxs-lookup"><span data-stu-id="55b66-121">Copy the **Application Id** that's assigned to your app because you'll need it soon.</span></span>
* <span data-ttu-id="55b66-122">Lägg till den **Mobile** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="55b66-122">Add the **Mobile** platform for your app.</span></span>
* <span data-ttu-id="55b66-123">Kopiera den **omdirigerings-URI** från portalen.</span><span class="sxs-lookup"><span data-stu-id="55b66-123">Copy the **Redirect URI** from the portal.</span></span> <span data-ttu-id="55b66-124">Du måste använda standardvärdet för `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="55b66-124">You must use the default value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a><span data-ttu-id="55b66-125">Hämta från tredje part NXOAuth2 bibliotek och skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="55b66-125">Download the third-party NXOAuth2 library and create a workspace</span></span>
<span data-ttu-id="55b66-126">Den här genomgången använder OAuth2Client från GitHub, vilket är en OAuth2-biblioteket för Mac OS X och iOS (Cocoa och Cocoa touch).</span><span class="sxs-lookup"><span data-stu-id="55b66-126">For this walkthrough, you will use the OAuth2Client from GitHub, which is an OAuth2 library for Mac OS X and iOS (Cocoa and Cocoa touch).</span></span> <span data-ttu-id="55b66-127">Det här biblioteket baseras på utkast 10 av OAuth2-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="55b66-127">This library is based on draft 10 of the OAuth2 spec.</span></span> <span data-ttu-id="55b66-128">Den implementerar interna programprofilen och stöder autentiseringsslutpunkt för användaren.</span><span class="sxs-lookup"><span data-stu-id="55b66-128">It implements the native application profile and supports the authorization endpoint of the user.</span></span> <span data-ttu-id="55b66-129">Detta är allt du behöver integrera med Microsoft identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="55b66-129">These are all the things you'll need to integrate with the Microsoft identity platform.</span></span>

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a><span data-ttu-id="55b66-130">Lägg till biblioteket i projektet med CocoaPods</span><span class="sxs-lookup"><span data-stu-id="55b66-130">Add the library to your project by using CocoaPods</span></span>
<span data-ttu-id="55b66-131">CocoaPods är en beroendehanterare för Xcode-projekt.</span><span class="sxs-lookup"><span data-stu-id="55b66-131">CocoaPods is a dependency manager for Xcode projects.</span></span> <span data-ttu-id="55b66-132">Den hanterar tidigare installationsstegen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="55b66-132">It manages the previous installation steps automatically.</span></span>

```
$ vi Podfile
```
1. <span data-ttu-id="55b66-133">Lägg till följande i Podfile:</span><span class="sxs-lookup"><span data-stu-id="55b66-133">Add the following to this podfile:</span></span>
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. <span data-ttu-id="55b66-134">Läsa in podfile med CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="55b66-134">Load the podfile by using CocoaPods.</span></span> <span data-ttu-id="55b66-135">Då skapas en ny Xcode-arbetsyta som ska läsas in.</span><span class="sxs-lookup"><span data-stu-id="55b66-135">This will create a new Xcode workspace that you will load.</span></span>
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a><span data-ttu-id="55b66-136">Utforska strukturen för projektet</span><span class="sxs-lookup"><span data-stu-id="55b66-136">Explore the structure of the project</span></span>
<span data-ttu-id="55b66-137">Följande struktur har ställts in för projektet i stommen:</span><span class="sxs-lookup"><span data-stu-id="55b66-137">The following structure is set up for our project in the skeleton:</span></span>

* <span data-ttu-id="55b66-138">En bakgrundsläge med en UPN-sökning</span><span class="sxs-lookup"><span data-stu-id="55b66-138">A Master View with a UPN Search</span></span>
* <span data-ttu-id="55b66-139">En detaljerad vy för data om den markerade användaren</span><span class="sxs-lookup"><span data-stu-id="55b66-139">A Detail View for the data about the selected user</span></span>
* <span data-ttu-id="55b66-140">Vyn inloggning där en användare kan logga in på appen för att fråga diagrammet</span><span class="sxs-lookup"><span data-stu-id="55b66-140">A Login View where a user can sign in to the app to query the graph</span></span>

<span data-ttu-id="55b66-141">Vi flyttas till olika filer i stommen för att lägga till autentisering.</span><span class="sxs-lookup"><span data-stu-id="55b66-141">We will move to various files in the skeleton to add authentication.</span></span> <span data-ttu-id="55b66-142">Andra delar av kod, exempelvis visual koden rör inte identitet, men du.</span><span class="sxs-lookup"><span data-stu-id="55b66-142">Other parts of the code, such as the visual code, do not pertain to identity but are provided for you.</span></span>

## <a name="set-up-the-settingsplst-file-in-the-library"></a><span data-ttu-id="55b66-143">Konfigurera settings.plst filen i biblioteket</span><span class="sxs-lookup"><span data-stu-id="55b66-143">Set up the settings.plst file in the library</span></span>
* <span data-ttu-id="55b66-144">I snabbstartsprojektet öppnar den `settings.plist` filen.</span><span class="sxs-lookup"><span data-stu-id="55b66-144">In the QuickStart project, open the `settings.plist` file.</span></span> <span data-ttu-id="55b66-145">Ersätt värdena för elementen i avsnittet för att återspegla de värden som du använde i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="55b66-145">Replace the values of the elements in the section to reflect the values that you used in the Azure portal.</span></span> <span data-ttu-id="55b66-146">Koden ska referera till dessa värden när den används av Active Directory Authentication Library.</span><span class="sxs-lookup"><span data-stu-id="55b66-146">Your code will reference these values whenever it uses the Active Directory Authentication Library.</span></span>
  * <span data-ttu-id="55b66-147">Den `clientId` är klient-ID för programmet som du kopierade från portalen.</span><span class="sxs-lookup"><span data-stu-id="55b66-147">The `clientId` is the client ID of your application that you copied from the portal.</span></span>
  * <span data-ttu-id="55b66-148">Den `redirectUri` är omdirigerings-URL som angetts i portalen.</span><span class="sxs-lookup"><span data-stu-id="55b66-148">The `redirectUri` is the redirect URL that the portal provided.</span></span>

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a><span data-ttu-id="55b66-149">Ställ in NXOAuth2Client biblioteket i din LoginViewController</span><span class="sxs-lookup"><span data-stu-id="55b66-149">Set up the NXOAuth2Client library in your LoginViewController</span></span>
<span data-ttu-id="55b66-150">Biblioteket NXOAuth2Client kräver vissa värden för att konfigurera.</span><span class="sxs-lookup"><span data-stu-id="55b66-150">The NXOAuth2Client library requires some values to get set up.</span></span> <span data-ttu-id="55b66-151">Du kan använda anskaffats token för att anropa Graph API när du har slutfört uppgiften.</span><span class="sxs-lookup"><span data-stu-id="55b66-151">After you complete that task, you can use the acquired token to call the Graph API.</span></span> <span data-ttu-id="55b66-152">Eftersom `LoginView` kommer att anropas när vi behöver verifiera det praktiskt att placera konfigurationsvärden i filen.</span><span class="sxs-lookup"><span data-stu-id="55b66-152">Because `LoginView` will be called any time we need to authenticate, it makes sense to put configuration values in to that file.</span></span>

* <span data-ttu-id="55b66-153">Ska vi lägga till vissa värden till den `LoginViewController.m` fil att ange kontext för autentisering och auktorisering.</span><span class="sxs-lookup"><span data-stu-id="55b66-153">Let's add some values to the  `LoginViewController.m` file to set the context for authentication and authorization.</span></span> <span data-ttu-id="55b66-154">Information om värden följer koden.</span><span class="sxs-lookup"><span data-stu-id="55b66-154">Details about the values follow the code.</span></span>
  
    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

<span data-ttu-id="55b66-155">Nu ska vi titta på information om koden.</span><span class="sxs-lookup"><span data-stu-id="55b66-155">Let's look at details about the code.</span></span>

<span data-ttu-id="55b66-156">Den första strängen är för `scopes`.</span><span class="sxs-lookup"><span data-stu-id="55b66-156">The first string is for `scopes`.</span></span>  <span data-ttu-id="55b66-157">Den `User.Read` värde kan du läsa grundläggande profilen för den inloggade användaren.</span><span class="sxs-lookup"><span data-stu-id="55b66-157">The `User.Read` value allows you to read the basic profile of the signed in user.</span></span>

<span data-ttu-id="55b66-158">Du kan lära dig mer om alla tillgängliga scope på [behörighetsomfattningen för Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="55b66-158">You can learn more about all the available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="55b66-159">För `authURL`, `loginURL`, `bhh`, och `tokenURL`, bör du använda de värden som anges tidigare.</span><span class="sxs-lookup"><span data-stu-id="55b66-159">For `authURL`, `loginURL`, `bhh`, and `tokenURL`, you should use the values provided previously.</span></span> <span data-ttu-id="55b66-160">Om du använder öppen källkod bibliotek för Microsoft Azure identitet, hämtar vi informationen du med hjälp av vår metadataslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="55b66-160">If you use the open source Microsoft Azure Identity Libraries, we pull this data down for you by using our metadata endpoint.</span></span> <span data-ttu-id="55b66-161">Du slipper extrahera värdena själv. Vi gör det åt dig.</span><span class="sxs-lookup"><span data-stu-id="55b66-161">We've done the hard work of extracting these values for you.</span></span>

<span data-ttu-id="55b66-162">`keychain`-värdet är den behållare som NXOAuth2Client-biblioteket använder för att skapa en nyckelring som lagrar dina token.</span><span class="sxs-lookup"><span data-stu-id="55b66-162">The `keychain` value is the container that the NXOAuth2Client library will use to create a keychain to store your tokens.</span></span> <span data-ttu-id="55b66-163">Om du vill hämta enkel inloggning (SSO) över flera appar kan du ange samma nyckelringen i var och en av dina program och begäran användningen av den nyckelringen i Xcode-rättigheter.</span><span class="sxs-lookup"><span data-stu-id="55b66-163">If you'd like to get single sign-on (SSO) across numerous apps, you can specify the same keychain in each of your applications and request the use of that keychain in your Xcode entitlements.</span></span> <span data-ttu-id="55b66-164">Detta förklaras i Apples dokumentation.</span><span class="sxs-lookup"><span data-stu-id="55b66-164">This is explained in the Apple documentation.</span></span>

<span data-ttu-id="55b66-165">Resten av värdena krävs för att använda biblioteket och skapa platser för dig att utföra värden i kontexten.</span><span class="sxs-lookup"><span data-stu-id="55b66-165">The rest of these values are required to use the library and create places for you to carry values to the context.</span></span>

### <a name="create-a-url-cache"></a><span data-ttu-id="55b66-166">Skapa en URL-cache</span><span class="sxs-lookup"><span data-stu-id="55b66-166">Create a URL cache</span></span>
<span data-ttu-id="55b66-167">I `(void)viewDidLoad()`, vilket kallas alltid efter vyn har lästs in, följande kod primes ett cacheminne för våra användning.</span><span class="sxs-lookup"><span data-stu-id="55b66-167">Inside `(void)viewDidLoad()`, which is always called after the view is loaded, the following code primes a cache for our use.</span></span>

<span data-ttu-id="55b66-168">Lägg till följande kod:</span><span class="sxs-lookup"><span data-stu-id="55b66-168">Add the following code:</span></span>

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a><span data-ttu-id="55b66-169">Skapa en webbvy för inloggning</span><span class="sxs-lookup"><span data-stu-id="55b66-169">Create a WebView for sign-in</span></span>
<span data-ttu-id="55b66-170">En webbvy kan fråga användaren om faktorer som SMS textmeddelande (om konfigurerad) eller returnera felmeddelanden för användaren.</span><span class="sxs-lookup"><span data-stu-id="55b66-170">A WebView can prompt the user for additional factors like SMS text message (if configured) or return error messages to the user.</span></span> <span data-ttu-id="55b66-171">Här definierar du upp webbvyn och sedan skriva kod för att hantera återanrop som sker i webbvy från tjänsterna identitet.</span><span class="sxs-lookup"><span data-stu-id="55b66-171">Here you'll set up the WebView and then later write the code to handle the callbacks that will happen in the WebView from the identity services.</span></span>

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a><span data-ttu-id="55b66-172">Åsidosätt WebView-metoderna för att hantera autentiseringen</span><span class="sxs-lookup"><span data-stu-id="55b66-172">Override the WebView methods to handle authentication</span></span>
<span data-ttu-id="55b66-173">Om du vill ge webbvyn vad som händer när en användare måste logga in som beskrivits tidigare, kan du klistra in följande kod.</span><span class="sxs-lookup"><span data-stu-id="55b66-173">To tell the WebView what happens when a user needs to sign in as discussed previously, you can paste the following code.</span></span>

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a><span data-ttu-id="55b66-174">Skriv koden för att hantera resultatet från OAuth2-begäran</span><span class="sxs-lookup"><span data-stu-id="55b66-174">Write code to handle the result of the OAuth2 request</span></span>
<span data-ttu-id="55b66-175">Följande kod kommer att hantera den RedirectUrl anges som returneras från webbvyn.</span><span class="sxs-lookup"><span data-stu-id="55b66-175">The following code will handle the redirectURL that returns from the WebView.</span></span> <span data-ttu-id="55b66-176">Om autentisering inte lyckas försöker koden igen.</span><span class="sxs-lookup"><span data-stu-id="55b66-176">If authentication wasn't successful, the code will try again.</span></span> <span data-ttu-id="55b66-177">Under tiden ger biblioteket felet som du kan visa i konsolen eller hantera asynkront.</span><span class="sxs-lookup"><span data-stu-id="55b66-177">Meanwhile, the library will provide the error that you can see in the console or handle asynchronously.</span></span>

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a><span data-ttu-id="55b66-178">Konfigurera OAuth-kontexten (kallas kontoarkiv)</span><span class="sxs-lookup"><span data-stu-id="55b66-178">Set up the OAuth Context (called account store)</span></span>
<span data-ttu-id="55b66-179">Här kan du anropa `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` på delade kontoarkiv för varje tjänst som du vill att programmet ska kunna få åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="55b66-179">Here you can call `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` on the shared account store for each service that you want the application to be able to access.</span></span> <span data-ttu-id="55b66-180">Typen är en sträng som används som en identifierare för en viss tjänst.</span><span class="sxs-lookup"><span data-stu-id="55b66-180">The account type is a string that is used as an identifier for a certain service.</span></span> <span data-ttu-id="55b66-181">Eftersom du kommer åt Graph API koden refererar till den som `"myGraphService"`.</span><span class="sxs-lookup"><span data-stu-id="55b66-181">Because you are accessing the Graph API, the code refers to it as `"myGraphService"`.</span></span> <span data-ttu-id="55b66-182">Du ställa in en person som anger när något ändras med token.</span><span class="sxs-lookup"><span data-stu-id="55b66-182">You then set up an observer that will tell you when anything changes with the token.</span></span> <span data-ttu-id="55b66-183">När du har fått token som du kommer tillbaka användaren tillbaka till den `masterView`.</span><span class="sxs-lookup"><span data-stu-id="55b66-183">After you get the token, you return the user back to the `masterView`.</span></span>

```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a><span data-ttu-id="55b66-184">Ställ in bakgrundsläge att söka efter och visa användare från Graph-API</span><span class="sxs-lookup"><span data-stu-id="55b66-184">Set up the Master View to search and display the users from the Graph API</span></span>
<span data-ttu-id="55b66-185">Självstudier för många online förklarar hur du skapar en en app i Master-View-Controller (MVC) som visar returnerade data i rutnätet är utanför omfattningen för den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="55b66-185">A Master-View-Controller (MVC) app that displays the returned data in the grid is beyond the scope of this walkthrough, and many online tutorials explain how to build one.</span></span> <span data-ttu-id="55b66-186">Den här koden är i stommen-filen.</span><span class="sxs-lookup"><span data-stu-id="55b66-186">All this code is in the skeleton file.</span></span> <span data-ttu-id="55b66-187">Men behöver du hantera några saker i den här MVC-program:</span><span class="sxs-lookup"><span data-stu-id="55b66-187">However, you do need to deal with a few things in this MVC application:</span></span>

* <span data-ttu-id="55b66-188">Fånga upp när användaren skriver något i sökfältet</span><span class="sxs-lookup"><span data-stu-id="55b66-188">Intercept when a user types something in the search field</span></span>
* <span data-ttu-id="55b66-189">Ange ett objekt av data tillbaka till MasterView så att den kan visa resultaten i rutnätet</span><span class="sxs-lookup"><span data-stu-id="55b66-189">Provide an object of data back to the MasterView so it can display the results in the grid</span></span>

<span data-ttu-id="55b66-190">Vi ska göra de nedan.</span><span class="sxs-lookup"><span data-stu-id="55b66-190">We'll do those below.</span></span>

### <a name="add-a-check-to-see-if-youre-logged-in"></a><span data-ttu-id="55b66-191">Lägg till en kontroll för att se om du är inloggad</span><span class="sxs-lookup"><span data-stu-id="55b66-191">Add a check to see if you're logged in</span></span>
<span data-ttu-id="55b66-192">Programmet har liten eller om användaren inte är inloggad, så det är att kontrollera om det finns redan en token i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="55b66-192">The application does little if the user is not signed in, so it's smart to check if there is already a token in the cache.</span></span> <span data-ttu-id="55b66-193">Om inte du omdirigerar till kontollen för användaren att logga in.</span><span class="sxs-lookup"><span data-stu-id="55b66-193">If not, you redirect to the LoginView for the user to sign in.</span></span> <span data-ttu-id="55b66-194">Om du kommer ihåg det bästa sättet att utföra åtgärder när vyn läses in är att använda den `viewDidLoad()` metod som Apple ger oss.</span><span class="sxs-lookup"><span data-stu-id="55b66-194">If you recall, the best way to do actions when a view loads is to use the `viewDidLoad()` method that Apple provides us.</span></span>

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a><span data-ttu-id="55b66-195">Uppdatera tabellvyn när data tas emot</span><span class="sxs-lookup"><span data-stu-id="55b66-195">Update the Table View when data is received</span></span>
<span data-ttu-id="55b66-196">När Graph API returnerar data, måste du visa data.</span><span class="sxs-lookup"><span data-stu-id="55b66-196">When the Graph API returns data, you need to display the data.</span></span> <span data-ttu-id="55b66-197">För enkelhetens skull är här all kod att uppdatera tabellen.</span><span class="sxs-lookup"><span data-stu-id="55b66-197">For simplicity, here is all the code to update the table.</span></span> <span data-ttu-id="55b66-198">Du kan bara klistra in rätt värden i MVC formaterad koden.</span><span class="sxs-lookup"><span data-stu-id="55b66-198">You can just paste the right values in your MVC boilerplate code.</span></span>

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a><span data-ttu-id="55b66-199">Ger dig ett sätt att anropa Graph API när någon skriver i sökfältet</span><span class="sxs-lookup"><span data-stu-id="55b66-199">Provide a way to call the Graph API when someone types in the search field</span></span>
<span data-ttu-id="55b66-200">När en användare skriver en sökning i rutan, behöver du shove som för Graph API.</span><span class="sxs-lookup"><span data-stu-id="55b66-200">When a user types a search in the box, you need to shove that over to the Graph API.</span></span> <span data-ttu-id="55b66-201">Den `GraphAPICaller` -klassen, som du skapar i följande kod, separerar lookup-funktioner från presentationen.</span><span class="sxs-lookup"><span data-stu-id="55b66-201">The `GraphAPICaller` class, which you will build in the following code, separates the lookup functionality from the presentation.</span></span> <span data-ttu-id="55b66-202">Nu är det dags att skriva koden som flöden Sök tecken för Graph API.</span><span class="sxs-lookup"><span data-stu-id="55b66-202">For now, let's write the code that feeds any search characters to the Graph API.</span></span> <span data-ttu-id="55b66-203">Vi kan göra detta genom att tillhandahålla en metod som kallas `lookupInGraph`, som tar den sträng som vi vill söka efter.</span><span class="sxs-lookup"><span data-stu-id="55b66-203">We do this by providing a method called `lookupInGraph`, which takes the string that we want to search for.</span></span>

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a><span data-ttu-id="55b66-204">Skriv en hjälparklass för att komma åt Graph-API</span><span class="sxs-lookup"><span data-stu-id="55b66-204">Write a Helper class to access the Graph API</span></span>
<span data-ttu-id="55b66-205">Detta är kärnan i vårt program.</span><span class="sxs-lookup"><span data-stu-id="55b66-205">This is the core of our application.</span></span> <span data-ttu-id="55b66-206">Medan resten Infoga kod i MVC Standardmönster från Apple, skriva här du kod för att fråga diagrammet som användartyper och returnera dessa data.</span><span class="sxs-lookup"><span data-stu-id="55b66-206">Whereas the rest was inserting code in the default MVC pattern from Apple, here you write code to query the graph as the user types and then return that data.</span></span> <span data-ttu-id="55b66-207">En detaljerad förklaring följer det här är koden.</span><span class="sxs-lookup"><span data-stu-id="55b66-207">Here's the code, and a detailed explanation follows it.</span></span>

### <a name="create-a-new-objective-c-header-file"></a><span data-ttu-id="55b66-208">Skapa en ny rubrikfil i Objective C</span><span class="sxs-lookup"><span data-stu-id="55b66-208">Create a new Objective C header file</span></span>
<span data-ttu-id="55b66-209">Namn på filen `GraphAPICaller.h`, och Lägg till följande kod.</span><span class="sxs-lookup"><span data-stu-id="55b66-209">Name the file `GraphAPICaller.h`, and add the following code.</span></span>

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

<span data-ttu-id="55b66-210">Här ser du att en angivna metoden tar en sträng och returnerar ett completionBlock.</span><span class="sxs-lookup"><span data-stu-id="55b66-210">Here you see that a specified method takes a string and returns a completionBlock.</span></span> <span data-ttu-id="55b66-211">Den här completionBlock uppdateras som du kan ha gissa tabellen genom att tillhandahålla ett objekt med ifyllda data i realtid som användaren sökningar.</span><span class="sxs-lookup"><span data-stu-id="55b66-211">This completionBlock, as you may have guessed, will update the table by providing an object with populated data in real time as the user searches.</span></span>

### <a name="create-a-new-objective-c-file"></a><span data-ttu-id="55b66-212">Skapa en ny fil i Objective C</span><span class="sxs-lookup"><span data-stu-id="55b66-212">Create a new Objective C file</span></span>
<span data-ttu-id="55b66-213">Namn på filen `GraphAPICaller.m`, och Lägg till följande metod.</span><span class="sxs-lookup"><span data-stu-id="55b66-213">Name the file `GraphAPICaller.m`, and add the following method.</span></span>

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

<span data-ttu-id="55b66-214">Vi går igenom den här metoden i detalj.</span><span class="sxs-lookup"><span data-stu-id="55b66-214">Let's go through this method in detail.</span></span>

<span data-ttu-id="55b66-215">Kärnan i den här koden finns i den `NXOAuth2Request`, metod som använder parametrar som du redan har definierat i filen settings.plist.</span><span class="sxs-lookup"><span data-stu-id="55b66-215">The core of this code is in the `NXOAuth2Request`, method which takes the parameters that you've already defined in the settings.plist file.</span></span>

<span data-ttu-id="55b66-216">Det första steget är att skapa rätt Graph API-anropet.</span><span class="sxs-lookup"><span data-stu-id="55b66-216">The first step is to construct the right Graph API call.</span></span> <span data-ttu-id="55b66-217">Eftersom du anropar `/users`, du anger att genom att lägga till den resursen Graph API tillsammans med versionen.</span><span class="sxs-lookup"><span data-stu-id="55b66-217">Because you are calling `/users`, you specify that by appending it to the Graph API resource along with the version.</span></span> <span data-ttu-id="55b66-218">Det är praktiskt att placera dem i en extern inställningsfil eftersom de kan ändra som utvecklas för API: et.</span><span class="sxs-lookup"><span data-stu-id="55b66-218">It makes sense to put these in an external settings file because these can change as the API evolves.</span></span>

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

<span data-ttu-id="55b66-219">Du måste sedan ange parametrar som du ger även Graph API-anropet.</span><span class="sxs-lookup"><span data-stu-id="55b66-219">Next, you need to specify parameters that you will also provide to the Graph API call.</span></span> <span data-ttu-id="55b66-220">Det är *viktigt* att du inte anger parametrarna i resurs-slutpunkten eftersom som befordras för alla icke-URI förväntad tecken vid körning.</span><span class="sxs-lookup"><span data-stu-id="55b66-220">It is *very important* that you do not put the parameters in the resource endpoint because that is scrubbed for all non-URI conforming characters at runtime.</span></span> <span data-ttu-id="55b66-221">All kod som frågan måste anges i parametrarna.</span><span class="sxs-lookup"><span data-stu-id="55b66-221">All query code must be provided in the parameters.</span></span>

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

<span data-ttu-id="55b66-222">Du kan se detta anropar en `convertParamsToDictionary` metod som du ännu inte har skrivits.</span><span class="sxs-lookup"><span data-stu-id="55b66-222">You might notice this calls a `convertParamsToDictionary` method that you haven't written yet.</span></span> <span data-ttu-id="55b66-223">Låt oss göra nu i slutet av filen:</span><span class="sxs-lookup"><span data-stu-id="55b66-223">Let's do so now at the end of the file:</span></span>

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
<span data-ttu-id="55b66-224">Nu ska vi använda den `NXOAuth2Request` metod för att hämta data tillbaka från API: et i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="55b66-224">Next, let's use the `NXOAuth2Request` method to get data back from the API in JSON format.</span></span>

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

<span data-ttu-id="55b66-225">Slutligen kan du nu ska vi titta på hur du returnerar data till MasterViewController.</span><span class="sxs-lookup"><span data-stu-id="55b66-225">Finally, let's look at how you return the data to the MasterViewController.</span></span> <span data-ttu-id="55b66-226">Data returnerar som serialiseras och behöver avserialiseras och läsas in i ett objekt som MainViewController kan använda.</span><span class="sxs-lookup"><span data-stu-id="55b66-226">The data returns as serialized and needs to be deserialized and loaded in an object that the MainViewController can consume.</span></span> <span data-ttu-id="55b66-227">För detta ändamål stommen har en `User.m/h` -fil som skapar ett användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="55b66-227">For this purpose, the skeleton has a `User.m/h` file that creates a User object.</span></span> <span data-ttu-id="55b66-228">Du kan fylla det användarobjektet med information från diagrammet.</span><span class="sxs-lookup"><span data-stu-id="55b66-228">You populate that User object with information from the graph.</span></span>

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a><span data-ttu-id="55b66-229">Köra exemplet</span><span class="sxs-lookup"><span data-stu-id="55b66-229">Run the sample</span></span>
<span data-ttu-id="55b66-230">Om du har använt stommen eller följt tillsammans med den här genomgången ska nu ditt program att köras.</span><span class="sxs-lookup"><span data-stu-id="55b66-230">If you've used the skeleton or followed along with the walkthrough your application should now run.</span></span> <span data-ttu-id="55b66-231">Starta simulatorn och på **inloggning** att använda programmet.</span><span class="sxs-lookup"><span data-stu-id="55b66-231">Start the simulator and click **Sign in** to use the application.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="55b66-232">Hämta säkerhetsuppdateringar för vår produkt</span><span class="sxs-lookup"><span data-stu-id="55b66-232">Get security updates for our product</span></span>
<span data-ttu-id="55b66-233">Vi rekommenderar att du aktiverar aviseringar om säkerhetsincidenter genom att besöka den [säkerhet TechCenter](https://technet.microsoft.com/security/dd252948) och prenumerera på Security Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="55b66-233">We encourage you to get notifications of when security incidents occur by visiting the [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>


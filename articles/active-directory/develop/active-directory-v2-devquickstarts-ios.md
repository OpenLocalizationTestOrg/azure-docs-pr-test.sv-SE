---
title: "aaaAdd inloggning tooan iOS använder hello Azure AD v2.0-slutpunkten | Microsoft Docs"
description: "Hur toobuild en iOS-app som loggar in användare med både personliga Microsoft-konto och arbets-eller skolkonton med hjälp av tredjeparts-bibliotek."
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
ms.openlocfilehash: a384062e6e4bd398a2b12318800728e627e05c32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-ios-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a><span data-ttu-id="7a06a-103">Lägga till inloggning tooan iOS-app med en tredjeparts-bibliotek med Graph API: et med hello v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="7a06a-103">Add sign-in tooan iOS app using a third-party library with Graph API using hello v2.0 endpoint</span></span>
<span data-ttu-id="7a06a-104">hello Microsoft identity-plattformen använder öppna standarder, till exempel OAuth2 och OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="7a06a-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="7a06a-105">Utvecklare kan använda alla bibliotek som de vill toointegrate med våra tjänster.</span><span class="sxs-lookup"><span data-stu-id="7a06a-105">Developers can use any library they want toointegrate with our services.</span></span> <span data-ttu-id="7a06a-106">toohelp utvecklare använda vår plattform med andra bibliotek, vi har skrivit några genomgång som den här en toodemonstrate hur tooconfigure från tredje part bibliotek tooconnect toohello Microsoft identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-106">toohelp developers use our platform with other libraries, we've written a few walkthroughs like this one toodemonstrate how tooconfigure third-party libraries tooconnect toohello Microsoft identity platform.</span></span> <span data-ttu-id="7a06a-107">De flesta bibliotek som implementerar [hello RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kan ansluta toohello Microsoft identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect toohello Microsoft identity platform.</span></span>

<span data-ttu-id="7a06a-108">Med hello program som skapar den här genomgången, kan användare logga in tootheir organisation och söka efter andra i organisationen med hjälp av hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="7a06a-108">With hello application that this walkthrough creates, users can sign in tootheir organization and then search for others in their organization by using hello Graph API.</span></span>

<span data-ttu-id="7a06a-109">Om du är ny tooOAuth2 eller OpenID Connect är mycket av det här exemplet konfigurationen inte meningsfullt tooyou.</span><span class="sxs-lookup"><span data-stu-id="7a06a-109">If you're new tooOAuth2 or OpenID Connect, much of this sample configuration may not make sense tooyou.</span></span> <span data-ttu-id="7a06a-110">Vi rekommenderar att du läser [v2.0-protokoll - OAuth 2.0 auktorisering kod flöda](active-directory-v2-protocols-oauth-code.md) för bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="7a06a-110">We recommend that you read  [v2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="7a06a-111">Vissa funktioner i vår plattform som har ett uttryck i hello OAuth2 eller OpenID Connect standarder, till exempel villkorlig åtkomst och hantering av Intune behöver du toouse våra Microsoft Azure identitet bibliotek med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="7a06a-111">Some features of our platform that do have an expression in hello OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you toouse our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="7a06a-112">hello v2.0-slutpunkten har inte stöd för alla Azure Active Directory-scenarier och funktioner.</span><span class="sxs-lookup"><span data-stu-id="7a06a-112">hello v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="7a06a-113">toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="7a06a-113">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-code-from-github"></a><span data-ttu-id="7a06a-114">Hämta koden från GitHub</span><span class="sxs-lookup"><span data-stu-id="7a06a-114">Download code from GitHub</span></span>
<span data-ttu-id="7a06a-115">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span><span class="sxs-lookup"><span data-stu-id="7a06a-115">hello code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).</span></span>  <span data-ttu-id="7a06a-116">toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) eller klona hello stommen:</span><span class="sxs-lookup"><span data-stu-id="7a06a-116">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

<span data-ttu-id="7a06a-117">Du kan också hämta hello exempel och komma igång nu direkt:</span><span class="sxs-lookup"><span data-stu-id="7a06a-117">You can also just download hello sample and get started right away:</span></span>

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="7a06a-118">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="7a06a-118">Register an app</span></span>
<span data-ttu-id="7a06a-119">Skapa en ny app på hello [programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följ hello detaljerade anvisningar på [hur tooregister en app med hello v2.0-slutpunkten](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="7a06a-119">Create a new app at hello [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow hello detailed steps at  [How tooregister an app with hello v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="7a06a-120">Se till att:</span><span class="sxs-lookup"><span data-stu-id="7a06a-120">Make sure to:</span></span>

* <span data-ttu-id="7a06a-121">Kopiera hello **program-Id** som är tilldelade tooyour app eftersom du behöver den snart.</span><span class="sxs-lookup"><span data-stu-id="7a06a-121">Copy hello **Application Id** that's assigned tooyour app because you'll need it soon.</span></span>
* <span data-ttu-id="7a06a-122">Lägg till hello **Mobile** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="7a06a-122">Add hello **Mobile** platform for your app.</span></span>
* <span data-ttu-id="7a06a-123">Kopiera hello **omdirigerings-URI** från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-123">Copy hello **Redirect URI** from hello portal.</span></span> <span data-ttu-id="7a06a-124">Du måste använda hello standardvärdet `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="7a06a-124">You must use hello default value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="download-hello-third-party-nxoauth2-library-and-create-a-workspace"></a><span data-ttu-id="7a06a-125">Hämta hello från tredje part NXOAuth2 bibliotek och skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="7a06a-125">Download hello third-party NXOAuth2 library and create a workspace</span></span>
<span data-ttu-id="7a06a-126">Den här genomgången använder hello OAuth2Client från GitHub, vilket är en OAuth2-biblioteket för Mac OS X och iOS (Cocoa och Cocoa touch).</span><span class="sxs-lookup"><span data-stu-id="7a06a-126">For this walkthrough, you will use hello OAuth2Client from GitHub, which is an OAuth2 library for Mac OS X and iOS (Cocoa and Cocoa touch).</span></span> <span data-ttu-id="7a06a-127">Det här biblioteket är baserad på förslag 10 hello OAuth2-specifikationen. Den implementerar hello programspecifika profil och stöder hello autentiseringsslutpunkt för hello användare.</span><span class="sxs-lookup"><span data-stu-id="7a06a-127">This library is based on draft 10 of hello OAuth2 spec. It implements hello native application profile and supports hello authorization endpoint of hello user.</span></span> <span data-ttu-id="7a06a-128">Detta är allt du behöver toointegrate med identitetsplattformen för hello Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="7a06a-128">These are all hello things you'll need toointegrate with hello Microsoft identity platform.</span></span>

### <a name="add-hello-library-tooyour-project-by-using-cocoapods"></a><span data-ttu-id="7a06a-129">Lägga till hello biblioteket tooyour projekt med CocoaPods</span><span class="sxs-lookup"><span data-stu-id="7a06a-129">Add hello library tooyour project by using CocoaPods</span></span>
<span data-ttu-id="7a06a-130">CocoaPods är en beroendehanterare för Xcode-projekt.</span><span class="sxs-lookup"><span data-stu-id="7a06a-130">CocoaPods is a dependency manager for Xcode projects.</span></span> <span data-ttu-id="7a06a-131">Den hanterar hello föregående installationssteg automatiskt.</span><span class="sxs-lookup"><span data-stu-id="7a06a-131">It manages hello previous installation steps automatically.</span></span>

```
$ vi Podfile
```
1. <span data-ttu-id="7a06a-132">Lägg till följande toothis podfile hello:</span><span class="sxs-lookup"><span data-stu-id="7a06a-132">Add hello following toothis podfile:</span></span>
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. <span data-ttu-id="7a06a-133">Läsa in hello podfile med CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="7a06a-133">Load hello podfile by using CocoaPods.</span></span> <span data-ttu-id="7a06a-134">Då skapas en ny Xcode-arbetsyta som ska läsas in.</span><span class="sxs-lookup"><span data-stu-id="7a06a-134">This will create a new Xcode workspace that you will load.</span></span>
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-hello-structure-of-hello-project"></a><span data-ttu-id="7a06a-135">Utforska hello strukturen för hello-projekt</span><span class="sxs-lookup"><span data-stu-id="7a06a-135">Explore hello structure of hello project</span></span>
<span data-ttu-id="7a06a-136">hello efter strukturen har ställts in för projektet i hello stommen:</span><span class="sxs-lookup"><span data-stu-id="7a06a-136">hello following structure is set up for our project in hello skeleton:</span></span>

* <span data-ttu-id="7a06a-137">En bakgrundsläge med en UPN-sökning</span><span class="sxs-lookup"><span data-stu-id="7a06a-137">A Master View with a UPN Search</span></span>
* <span data-ttu-id="7a06a-138">En detaljerad vy för hello data om hello valda användare</span><span class="sxs-lookup"><span data-stu-id="7a06a-138">A Detail View for hello data about hello selected user</span></span>
* <span data-ttu-id="7a06a-139">Vyn inloggning där en användare kan logga in toohello app tooquery hello diagram</span><span class="sxs-lookup"><span data-stu-id="7a06a-139">A Login View where a user can sign in toohello app tooquery hello graph</span></span>

<span data-ttu-id="7a06a-140">Vi kommer att flyttas toovarious filer i hello stommen tooadd autentisering.</span><span class="sxs-lookup"><span data-stu-id="7a06a-140">We will move toovarious files in hello skeleton tooadd authentication.</span></span> <span data-ttu-id="7a06a-141">Andra delar av hello kod, exempelvis hello visual kod, gäller inte tooidentity men du.</span><span class="sxs-lookup"><span data-stu-id="7a06a-141">Other parts of hello code, such as hello visual code, do not pertain tooidentity but are provided for you.</span></span>

## <a name="set-up-hello-settingsplst-file-in-hello-library"></a><span data-ttu-id="7a06a-142">Ställ in hello settings.plst fil i hello-bibliotek</span><span class="sxs-lookup"><span data-stu-id="7a06a-142">Set up hello settings.plst file in hello library</span></span>
* <span data-ttu-id="7a06a-143">Öppna hello i hello snabbstartsprojekt `settings.plist` fil.</span><span class="sxs-lookup"><span data-stu-id="7a06a-143">In hello QuickStart project, open hello `settings.plist` file.</span></span> <span data-ttu-id="7a06a-144">Ersätt hello värdena för hello element i hello avsnittet tooreflect hello värden som du använde i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-144">Replace hello values of hello elements in hello section tooreflect hello values that you used in hello Azure portal.</span></span> <span data-ttu-id="7a06a-145">Koden ska referera till dessa värden när den använder hello Active Directory Authentication Library.</span><span class="sxs-lookup"><span data-stu-id="7a06a-145">Your code will reference these values whenever it uses hello Active Directory Authentication Library.</span></span>
  * <span data-ttu-id="7a06a-146">Hej `clientId` är hello klient-ID för programmet som du kopierade från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-146">hello `clientId` is hello client ID of your application that you copied from hello portal.</span></span>
  * <span data-ttu-id="7a06a-147">Hej `redirectUri` är hello omdirigerings-URL som hello-portalanvändare som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="7a06a-147">hello `redirectUri` is hello redirect URL that hello portal provided.</span></span>

## <a name="set-up-hello-nxoauth2client-library-in-your-loginviewcontroller"></a><span data-ttu-id="7a06a-148">Ställ in hello NXOAuth2Client bibliotek i din LoginViewController</span><span class="sxs-lookup"><span data-stu-id="7a06a-148">Set up hello NXOAuth2Client library in your LoginViewController</span></span>
<span data-ttu-id="7a06a-149">Hej NXOAuth2Client innehållsbiblioteket måste vissa värden tooget ställa in.</span><span class="sxs-lookup"><span data-stu-id="7a06a-149">hello NXOAuth2Client library requires some values tooget set up.</span></span> <span data-ttu-id="7a06a-150">När du har gjort det, kan du använda hello anskaffats token toocall hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="7a06a-150">After you complete that task, you can use hello acquired token toocall hello Graph API.</span></span> <span data-ttu-id="7a06a-151">Eftersom `LoginView` anropas när vi behöver tooauthenticate, är det klokt tooput konfigurationsvärden i toothat-filen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-151">Because `LoginView` will be called any time we need tooauthenticate, it makes sense tooput configuration values in toothat file.</span></span>

* <span data-ttu-id="7a06a-152">Lägg till vissa värden toohello `LoginViewController.m` filen tooset hello kontext för autentisering och auktorisering.</span><span class="sxs-lookup"><span data-stu-id="7a06a-152">Let's add some values toohello  `LoginViewController.m` file tooset hello context for authentication and authorization.</span></span> <span data-ttu-id="7a06a-153">Information om hello värden Följ hello kod.</span><span class="sxs-lookup"><span data-stu-id="7a06a-153">Details about hello values follow hello code.</span></span>
  
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

<span data-ttu-id="7a06a-154">Nu ska vi titta på detaljer om hello kod.</span><span class="sxs-lookup"><span data-stu-id="7a06a-154">Let's look at details about hello code.</span></span>

<span data-ttu-id="7a06a-155">hello första strängen är för `scopes`.</span><span class="sxs-lookup"><span data-stu-id="7a06a-155">hello first string is for `scopes`.</span></span>  <span data-ttu-id="7a06a-156">Hej `User.Read` värde kan du tooread hello grundprofil hello inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="7a06a-156">hello `User.Read` value allows you tooread hello basic profile of hello signed in user.</span></span>

<span data-ttu-id="7a06a-157">Du kan lära dig mer om alla tillgängliga hello-scope på [behörighetsomfattningen för Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="7a06a-157">You can learn more about all hello available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="7a06a-158">För `authURL`, `loginURL`, `bhh`, och `tokenURL`, bör du använda hello värden som tidigare.</span><span class="sxs-lookup"><span data-stu-id="7a06a-158">For `authURL`, `loginURL`, `bhh`, and `tokenURL`, you should use hello values provided previously.</span></span> <span data-ttu-id="7a06a-159">Om du använder hello Microsoft Azure identitet bibliotek med öppen källkod, hämtar vi informationen du med hjälp av vår metadataslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7a06a-159">If you use hello open source Microsoft Azure Identity Libraries, we pull this data down for you by using our metadata endpoint.</span></span> <span data-ttu-id="7a06a-160">Vi har gjort hello tunga arbetet med att extrahera värdena för dig.</span><span class="sxs-lookup"><span data-stu-id="7a06a-160">We've done hello hard work of extracting these values for you.</span></span>

<span data-ttu-id="7a06a-161">Hej `keychain` värde är hello-behållare som hello NXOAuth2Client biblioteket använder toocreate en nyckelringar toostore dina token.</span><span class="sxs-lookup"><span data-stu-id="7a06a-161">hello `keychain` value is hello container that hello NXOAuth2Client library will use toocreate a keychain toostore your tokens.</span></span> <span data-ttu-id="7a06a-162">Om du vill tooget enkel inloggning (SSO) över flera appar kan du ange hello samma nyckelringar i var och en av dina program och begära hello användning av den nyckelringen i Xcode-rättigheter.</span><span class="sxs-lookup"><span data-stu-id="7a06a-162">If you'd like tooget single sign-on (SSO) across numerous apps, you can specify hello same keychain in each of your applications and request hello use of that keychain in your Xcode entitlements.</span></span> <span data-ttu-id="7a06a-163">Detta förklaras i hello Apples dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7a06a-163">This is explained in hello Apple documentation.</span></span>

<span data-ttu-id="7a06a-164">hello resten av värdena är obligatoriska toouse hello bibliotek och skapa platser du toocarry värden toohello kontext.</span><span class="sxs-lookup"><span data-stu-id="7a06a-164">hello rest of these values are required toouse hello library and create places for you toocarry values toohello context.</span></span>

### <a name="create-a-url-cache"></a><span data-ttu-id="7a06a-165">Skapa en URL-cache</span><span class="sxs-lookup"><span data-stu-id="7a06a-165">Create a URL cache</span></span>
<span data-ttu-id="7a06a-166">I `(void)viewDidLoad()`, vilket kallas alltid efter hello vyn har lästs in, hello följande kod primes ett cacheminne för våra användning.</span><span class="sxs-lookup"><span data-stu-id="7a06a-166">Inside `(void)viewDidLoad()`, which is always called after hello view is loaded, hello following code primes a cache for our use.</span></span>

<span data-ttu-id="7a06a-167">Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="7a06a-167">Add hello following code:</span></span>

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

### <a name="create-a-webview-for-sign-in"></a><span data-ttu-id="7a06a-168">Skapa en webbvy för inloggning</span><span class="sxs-lookup"><span data-stu-id="7a06a-168">Create a WebView for sign-in</span></span>
<span data-ttu-id="7a06a-169">En webbvy kan fråga hello användaren om faktorer som SMS-textmeddelande (om konfigurerad) eller returnera fel meddelanden toohello användare.</span><span class="sxs-lookup"><span data-stu-id="7a06a-169">A WebView can prompt hello user for additional factors like SMS text message (if configured) or return error messages toohello user.</span></span> <span data-ttu-id="7a06a-170">Här ska du ange hello webbvy och sedan skriva hello kod toohandle hello återanrop sker i hello webbvy från hello identity services.</span><span class="sxs-lookup"><span data-stu-id="7a06a-170">Here you'll set up hello WebView and then later write hello code toohandle hello callbacks that will happen in hello WebView from hello identity services.</span></span>

```objc
-(void)requestOAuth2Access {
    //toosign in tooMicrosoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate toohello URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-hello-webview-methods-toohandle-authentication"></a><span data-ttu-id="7a06a-171">Åsidosätt hello webbvy metoder toohandle autentisering</span><span class="sxs-lookup"><span data-stu-id="7a06a-171">Override hello WebView methods toohandle authentication</span></span>
<span data-ttu-id="7a06a-172">tootell hello webbvy vad som händer när en användare behöver toosign i vilket beskrivs ovan, kan du klistra in hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="7a06a-172">tootell hello WebView what happens when a user needs toosign in as discussed previously, you can paste hello following code.</span></span>

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get hello auth token from a redirect so we need toohandle that in hello webview.

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

    // hello webview is where all hello communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if hello UIWebView is showing our authorization URL or consent URL, show hello UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide hello UIWebView, we've left hello authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide hello UIWebView, we've left hello authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read hello Location from hello UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by hello redirect URL we chose toouse from Microsoft APIs
        //continue hello OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-toohandle-hello-result-of-hello-oauth2-request"></a><span data-ttu-id="7a06a-173">Skriva koden toohandle hello resultatet av hello OAuth2 begäran</span><span class="sxs-lookup"><span data-stu-id="7a06a-173">Write code toohandle hello result of hello OAuth2 request</span></span>
<span data-ttu-id="7a06a-174">hello hanterar följande kod hello RedirectUrl anges som returnerar från hello webbvy.</span><span class="sxs-lookup"><span data-stu-id="7a06a-174">hello following code will handle hello redirectURL that returns from hello WebView.</span></span> <span data-ttu-id="7a06a-175">Om autentisering inte var lyckade försök hello kod igen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-175">If authentication wasn't successful, hello code will try again.</span></span> <span data-ttu-id="7a06a-176">Under tiden ger hello biblioteket hello-fel som du kan se i hello-konsolen eller hantera asynkront.</span><span class="sxs-lookup"><span data-stu-id="7a06a-176">Meanwhile, hello library will provide hello error that you can see in hello console or handle asynchronously.</span></span>

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse hello response for success or failure
     if (accessResult)
    //if success, complete hello OAuth2 flow by handling hello redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-hello-oauth-context-called-account-store"></a><span data-ttu-id="7a06a-177">Ställ in hello OAuth-kontexten (kallas kontoarkiv)</span><span class="sxs-lookup"><span data-stu-id="7a06a-177">Set up hello OAuth Context (called account store)</span></span>
<span data-ttu-id="7a06a-178">Här kan du anropa `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` på hello delade kontoarkiv för varje tjänst som du vill hello programmet toobe kan tooaccess.</span><span class="sxs-lookup"><span data-stu-id="7a06a-178">Here you can call `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` on hello shared account store for each service that you want hello application toobe able tooaccess.</span></span> <span data-ttu-id="7a06a-179">hello kontotypen är en sträng som används som en identifierare för en viss tjänst.</span><span class="sxs-lookup"><span data-stu-id="7a06a-179">hello account type is a string that is used as an identifier for a certain service.</span></span> <span data-ttu-id="7a06a-180">Eftersom du kommer åt hello Graph API hello koden refererar tooit som `"myGraphService"`.</span><span class="sxs-lookup"><span data-stu-id="7a06a-180">Because you are accessing hello Graph API, hello code refers tooit as `"myGraphService"`.</span></span> <span data-ttu-id="7a06a-181">Du ställa in en person som anger när något ändras med hello-token.</span><span class="sxs-lookup"><span data-stu-id="7a06a-181">You then set up an observer that will tell you when anything changes with hello token.</span></span> <span data-ttu-id="7a06a-182">När du får hello token kan du gå tillbaka hello användaren tillbaka toohello `masterView`.</span><span class="sxs-lookup"><span data-stu-id="7a06a-182">After you get hello token, you return hello user back toohello `masterView`.</span></span>

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

## <a name="set-up-hello-master-view-toosearch-and-display-hello-users-from-hello-graph-api"></a><span data-ttu-id="7a06a-183">Ställa in hello bakgrundsläge toosearch och visa hello användare från hello Graph API</span><span class="sxs-lookup"><span data-stu-id="7a06a-183">Set up hello Master View toosearch and display hello users from hello Graph API</span></span>
<span data-ttu-id="7a06a-184">En app i Master-View-Controller (MVC) som visar hello returnerade data i hello rutnät ligger utanför hello i den här genomgången och många online självstudier förklarar hur toobuild en.</span><span class="sxs-lookup"><span data-stu-id="7a06a-184">A Master-View-Controller (MVC) app that displays hello returned data in hello grid is beyond hello scope of this walkthrough, and many online tutorials explain how toobuild one.</span></span> <span data-ttu-id="7a06a-185">Den här koden är i stommen hello-filen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-185">All this code is in hello skeleton file.</span></span> <span data-ttu-id="7a06a-186">Du behöver dock toodeal med några saker i den här MVC-program:</span><span class="sxs-lookup"><span data-stu-id="7a06a-186">However, you do need toodeal with a few things in this MVC application:</span></span>

* <span data-ttu-id="7a06a-187">Fånga upp när användaren skriver något i sökfältet hello</span><span class="sxs-lookup"><span data-stu-id="7a06a-187">Intercept when a user types something in hello search field</span></span>
* <span data-ttu-id="7a06a-188">Ange ett objekt av data tillbaka toohello MasterView så hello resultat kan visas i rutnätet hello</span><span class="sxs-lookup"><span data-stu-id="7a06a-188">Provide an object of data back toohello MasterView so it can display hello results in hello grid</span></span>

<span data-ttu-id="7a06a-189">Vi ska göra de nedan.</span><span class="sxs-lookup"><span data-stu-id="7a06a-189">We'll do those below.</span></span>

### <a name="add-a-check-toosee-if-youre-logged-in"></a><span data-ttu-id="7a06a-190">Lägg till en kontroll toosee om du är inloggad</span><span class="sxs-lookup"><span data-stu-id="7a06a-190">Add a check toosee if you're logged in</span></span>
<span data-ttu-id="7a06a-191">hello program har lite om hello användaren inte är inloggad, så att den är smart toocheck om det finns redan en token i hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-191">hello application does little if hello user is not signed in, so it's smart toocheck if there is already a token in hello cache.</span></span> <span data-ttu-id="7a06a-192">Om inte du dirigerar toohello kontollen för hello användaren toosign i.</span><span class="sxs-lookup"><span data-stu-id="7a06a-192">If not, you redirect toohello LoginView for hello user toosign in.</span></span> <span data-ttu-id="7a06a-193">Om du kommer ihåg hello bästa sätt toodo åtgärder när vyn läses in är toouse hello `viewDidLoad()` metod som Apple ger oss.</span><span class="sxs-lookup"><span data-stu-id="7a06a-193">If you recall, hello best way toodo actions when a view loads is toouse hello `viewDidLoad()` method that Apple provides us.</span></span>

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

### <a name="update-hello-table-view-when-data-is-received"></a><span data-ttu-id="7a06a-194">Uppdatera hello tabellvy när data tas emot</span><span class="sxs-lookup"><span data-stu-id="7a06a-194">Update hello Table View when data is received</span></span>
<span data-ttu-id="7a06a-195">När hello Graph API returnerar data, måste toodisplay hello data.</span><span class="sxs-lookup"><span data-stu-id="7a06a-195">When hello Graph API returns data, you need toodisplay hello data.</span></span> <span data-ttu-id="7a06a-196">Här är alla hello kod tooupdate hello tabellen för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="7a06a-196">For simplicity, here is all hello code tooupdate hello table.</span></span> <span data-ttu-id="7a06a-197">Du kan bara klistra in hello rätt värden i MVC formaterad koden.</span><span class="sxs-lookup"><span data-stu-id="7a06a-197">You can just paste hello right values in your MVC boilerplate code.</span></span>

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


    // Configure hello cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-toocall-hello-graph-api-when-someone-types-in-hello-search-field"></a><span data-ttu-id="7a06a-198">Ange en sätt toocall hello Graph API när någon skriver i sökfältet hello</span><span class="sxs-lookup"><span data-stu-id="7a06a-198">Provide a way toocall hello Graph API when someone types in hello search field</span></span>
<span data-ttu-id="7a06a-199">När en användare skriver en sökning i hello ruta, måste tooshove som över toohello Graph API.</span><span class="sxs-lookup"><span data-stu-id="7a06a-199">When a user types a search in hello box, you need tooshove that over toohello Graph API.</span></span> <span data-ttu-id="7a06a-200">Hej `GraphAPICaller` -klassen, som du skapar i hello följande kod, separerar hello lookup-funktioner från hello presentation.</span><span class="sxs-lookup"><span data-stu-id="7a06a-200">hello `GraphAPICaller` class, which you will build in hello following code, separates hello lookup functionality from hello presentation.</span></span> <span data-ttu-id="7a06a-201">Nu är dags att skriva hello-kod som alla Sök tecken toohello Graph API-flöden.</span><span class="sxs-lookup"><span data-stu-id="7a06a-201">For now, let's write hello code that feeds any search characters toohello Graph API.</span></span> <span data-ttu-id="7a06a-202">Vi kan göra detta genom att tillhandahålla en metod som kallas `lookupInGraph`, som tar hello sträng som vi vill toosearch för.</span><span class="sxs-lookup"><span data-stu-id="7a06a-202">We do this by providing a method called `lookupInGraph`, which takes hello string that we want toosearch for.</span></span>

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

## <a name="write-a-helper-class-tooaccess-hello-graph-api"></a><span data-ttu-id="7a06a-203">Skriva ett Helper klassen tooaccess hello Graph API</span><span class="sxs-lookup"><span data-stu-id="7a06a-203">Write a Helper class tooaccess hello Graph API</span></span>
<span data-ttu-id="7a06a-204">Detta är hello kärnan i vårt program.</span><span class="sxs-lookup"><span data-stu-id="7a06a-204">This is hello core of our application.</span></span> <span data-ttu-id="7a06a-205">Medan hello rest Infoga kod i hello MVC Standardmönster från Apple, skriva här du kod tooquery hello diagram som hello användaren skriver och returnera dessa data.</span><span class="sxs-lookup"><span data-stu-id="7a06a-205">Whereas hello rest was inserting code in hello default MVC pattern from Apple, here you write code tooquery hello graph as hello user types and then return that data.</span></span> <span data-ttu-id="7a06a-206">En detaljerad förklaring följer det här är hello kod.</span><span class="sxs-lookup"><span data-stu-id="7a06a-206">Here's hello code, and a detailed explanation follows it.</span></span>

### <a name="create-a-new-objective-c-header-file"></a><span data-ttu-id="7a06a-207">Skapa en ny rubrikfil i Objective C</span><span class="sxs-lookup"><span data-stu-id="7a06a-207">Create a new Objective C header file</span></span>
<span data-ttu-id="7a06a-208">Namnet hello filen `GraphAPICaller.h`, och Lägg till följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="7a06a-208">Name hello file `GraphAPICaller.h`, and add hello following code.</span></span>

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

<span data-ttu-id="7a06a-209">Här ser du att en angivna metoden tar en sträng och returnerar ett completionBlock.</span><span class="sxs-lookup"><span data-stu-id="7a06a-209">Here you see that a specified method takes a string and returns a completionBlock.</span></span> <span data-ttu-id="7a06a-210">Den här completionBlock uppdateras som du kan ha gissa hello tabell genom att ange ett objekt med ifyllda data i realtid som hello användare söka.</span><span class="sxs-lookup"><span data-stu-id="7a06a-210">This completionBlock, as you may have guessed, will update hello table by providing an object with populated data in real time as hello user searches.</span></span>

### <a name="create-a-new-objective-c-file"></a><span data-ttu-id="7a06a-211">Skapa en ny fil i Objective C</span><span class="sxs-lookup"><span data-stu-id="7a06a-211">Create a new Objective C file</span></span>
<span data-ttu-id="7a06a-212">Namnet hello filen `GraphAPICaller.m`, och Lägg till följande metod hello.</span><span class="sxs-lookup"><span data-stu-id="7a06a-212">Name hello file `GraphAPICaller.m`, and add hello following method.</span></span>

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
                       // Process hello response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by hello key name being "value". It really is hello name of the
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

<span data-ttu-id="7a06a-213">Vi går igenom den här metoden i detalj.</span><span class="sxs-lookup"><span data-stu-id="7a06a-213">Let's go through this method in detail.</span></span>

<span data-ttu-id="7a06a-214">hello kärnan i den här koden är i hello `NXOAuth2Request`, metod som använder hello parametrar som du redan har definierat i hello settings.plist-filen.</span><span class="sxs-lookup"><span data-stu-id="7a06a-214">hello core of this code is in hello `NXOAuth2Request`, method which takes hello parameters that you've already defined in hello settings.plist file.</span></span>

<span data-ttu-id="7a06a-215">hello första steget är tooconstruct hello rätt Graph API-anrop.</span><span class="sxs-lookup"><span data-stu-id="7a06a-215">hello first step is tooconstruct hello right Graph API call.</span></span> <span data-ttu-id="7a06a-216">Eftersom du anropar `/users`, du anger att genom att lägga till den toohello Graph API resurs tillsammans med hello version.</span><span class="sxs-lookup"><span data-stu-id="7a06a-216">Because you are calling `/users`, you specify that by appending it toohello Graph API resource along with hello version.</span></span> <span data-ttu-id="7a06a-217">Gör det klokt tooput dessa i en extern inställningsfil eftersom de kan ändra hello API utvecklas.</span><span class="sxs-lookup"><span data-stu-id="7a06a-217">It makes sense tooput these in an external settings file because these can change as hello API evolves.</span></span>

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

<span data-ttu-id="7a06a-218">Sedan måste toospecify parametrar som du kan även ange toohello Graph API-anrop.</span><span class="sxs-lookup"><span data-stu-id="7a06a-218">Next, you need toospecify parameters that you will also provide toohello Graph API call.</span></span> <span data-ttu-id="7a06a-219">Det är *viktigt* att du inte anger hello parametrar i hello resurs slutpunkt eftersom som befordras för alla icke-URI förväntad tecken vid körning.</span><span class="sxs-lookup"><span data-stu-id="7a06a-219">It is *very important* that you do not put hello parameters in hello resource endpoint because that is scrubbed for all non-URI conforming characters at runtime.</span></span> <span data-ttu-id="7a06a-220">All kod som frågan måste anges i hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="7a06a-220">All query code must be provided in hello parameters.</span></span>

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

<span data-ttu-id="7a06a-221">Du kan se detta anropar en `convertParamsToDictionary` metod som du ännu inte har skrivits.</span><span class="sxs-lookup"><span data-stu-id="7a06a-221">You might notice this calls a `convertParamsToDictionary` method that you haven't written yet.</span></span> <span data-ttu-id="7a06a-222">Låt oss göra nu hello slutet av filen hello:</span><span class="sxs-lookup"><span data-stu-id="7a06a-222">Let's do so now at hello end of hello file:</span></span>

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
<span data-ttu-id="7a06a-223">Nu ska vi använda hello `NXOAuth2Request` metoden tooget data tillbaka från hello API i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="7a06a-223">Next, let's use hello `NXOAuth2Request` method tooget data back from hello API in JSON format.</span></span>

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
                       // Process hello response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

<span data-ttu-id="7a06a-224">Slutligen kan du nu ska vi titta på hur du returnerar hello data toohello MasterViewController.</span><span class="sxs-lookup"><span data-stu-id="7a06a-224">Finally, let's look at how you return hello data toohello MasterViewController.</span></span> <span data-ttu-id="7a06a-225">hello data returnerar som serialiseras och måste toobe avserialiseras och lästs in i ett objekt som hello MainViewController kan förbruka.</span><span class="sxs-lookup"><span data-stu-id="7a06a-225">hello data returns as serialized and needs toobe deserialized and loaded in an object that hello MainViewController can consume.</span></span> <span data-ttu-id="7a06a-226">För detta ändamål hello stommen har en `User.m/h` -fil som skapar ett användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="7a06a-226">For this purpose, hello skeleton has a `User.m/h` file that creates a User object.</span></span> <span data-ttu-id="7a06a-227">Du kan fylla det användarobjektet med information från hello diagram.</span><span class="sxs-lookup"><span data-stu-id="7a06a-227">You populate that User object with information from hello graph.</span></span>

```objc
                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by hello key name being "value". It really is hello name of the
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


## <a name="run-hello-sample"></a><span data-ttu-id="7a06a-228">Kör hello-exempel</span><span class="sxs-lookup"><span data-stu-id="7a06a-228">Run hello sample</span></span>
<span data-ttu-id="7a06a-229">Om du har används hello stommen eller följt tillsammans med hello genomgången ditt program nu ska köras.</span><span class="sxs-lookup"><span data-stu-id="7a06a-229">If you've used hello skeleton or followed along with hello walkthrough your application should now run.</span></span> <span data-ttu-id="7a06a-230">Starta hello simulator och på **logga in** toouse hello program.</span><span class="sxs-lookup"><span data-stu-id="7a06a-230">Start hello simulator and click **Sign in** toouse hello application.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="7a06a-231">Hämta säkerhetsuppdateringar för vår produkt</span><span class="sxs-lookup"><span data-stu-id="7a06a-231">Get security updates for our product</span></span>
<span data-ttu-id="7a06a-232">Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka hello [säkerhet TechCenter](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="7a06a-232">We encourage you tooget notifications of when security incidents occur by visiting hello [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>


---
title: "aaaAdd autentisering på iOS med Azure Mobile Apps"
description: "Lär dig hur toouse Azure Mobile Apps tooauthenticate användare av iOS-appen via en mängd olika identitetsleverantörer, inklusive AAD, Google, Facebook, Twitter och Microsoft."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: ef3d3cbe-e7ca-45f9-987f-80c44209dc06
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: glenga
ms.openlocfilehash: df129e1c7517582db0e4705e0a6e98345ac8a48c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-ios-app"></a><span data-ttu-id="e99b5-103">Lägg till autentisering tooyour iOS-app</span><span class="sxs-lookup"><span data-stu-id="e99b5-103">Add authentication tooyour iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="e99b5-104">I kursen får du lägger till autentisering toohello [iOS snabb start] projekt med en identitetsprovider som stöds.</span><span class="sxs-lookup"><span data-stu-id="e99b5-104">In this tutorial, you add authentication toohello [iOS quick start] project using a supported identity provider.</span></span> <span data-ttu-id="e99b5-105">Den här kursen är baserad på hello [iOS snabb start] självstudien måste du slutföra först.</span><span class="sxs-lookup"><span data-stu-id="e99b5-105">This tutorial is based on hello [iOS quick start] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="e99b5-106"><a name="register"></a>Registrera din app för autentisering och konfigurera hello Apptjänst</span><span class="sxs-lookup"><span data-stu-id="e99b5-106"><a name="register"></a>Register your app for authentication and configure hello App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="e99b5-107"><a name="redirecturl"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="e99b5-107"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="e99b5-108">Säker autentisering måste du definiera en ny URL-schema för din app.</span><span class="sxs-lookup"><span data-stu-id="e99b5-108">Secure authentication requires that you define a new URL scheme for your app.</span></span>  <span data-ttu-id="e99b5-109">Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar.</span><span class="sxs-lookup"><span data-stu-id="e99b5-109">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span>  <span data-ttu-id="e99b5-110">I den här självstudiekursen kommer vi använda URL-schemat _appname_ i hela.</span><span class="sxs-lookup"><span data-stu-id="e99b5-110">In this tutorial, we use the URL scheme _appname_ throughout.</span></span>  <span data-ttu-id="e99b5-111">Du kan dock använda alla URL-schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="e99b5-111">However, you can use any URL scheme you choose.</span></span>  <span data-ttu-id="e99b5-112">Det bör vara unikt tooyour mobila program.</span><span class="sxs-lookup"><span data-stu-id="e99b5-112">It should be unique tooyour mobile application.</span></span>  <span data-ttu-id="e99b5-113">tooenable hello omdirigering på serversidan th:</span><span class="sxs-lookup"><span data-stu-id="e99b5-113">tooenable hello redirection on th server side:</span></span>

1. <span data-ttu-id="e99b5-114">I hello [Azure-portalen], Välj din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="e99b5-114">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="e99b5-115">Klicka på hello **autentisering / auktorisering** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="e99b5-115">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="e99b5-116">Klicka på **Azure Active Directory** under hello **autentiseringsproviders** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e99b5-116">Click **Azure Active Directory** under hello **Authentication Providers** section.</span></span>

4. <span data-ttu-id="e99b5-117">Ange hello **hanteringsläge** för**Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="e99b5-117">Set hello **Management mode** too**Advanced**.</span></span>

5. <span data-ttu-id="e99b5-118">I hello **tillåtna externa omdirigerings-URL: er**, ange `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="e99b5-118">In hello **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="e99b5-119">Hej _appname_ i den här strängen är hello URL-schema för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="e99b5-119">hello _appname_ in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="e99b5-120">Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).</span><span class="sxs-lookup"><span data-stu-id="e99b5-120">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="e99b5-121">Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.</span><span class="sxs-lookup"><span data-stu-id="e99b5-121">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

6. <span data-ttu-id="e99b5-122">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e99b5-122">Click **OK**.</span></span>

7. <span data-ttu-id="e99b5-123">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e99b5-123">Click **Save**.</span></span>

## <span data-ttu-id="e99b5-124"><a name="permissions"></a>Begränsa behörigheter tooauthenticated användare</span><span class="sxs-lookup"><span data-stu-id="e99b5-124"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="e99b5-125">Tryck på i Xcode **kör** toostart hello app.</span><span class="sxs-lookup"><span data-stu-id="e99b5-125">In Xcode, press **Run** toostart hello app.</span></span> <span data-ttu-id="e99b5-126">Ett undantagsfel genereras eftersom hello app försöker tooaccess backend som en oautentiserad användare, men hello *TodoItem* tabellen nu kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="e99b5-126">An exception is raised because hello app attempts tooaccess the backend as an unauthenticated user, but hello *TodoItem* table now requires authentication.</span></span>

## <span data-ttu-id="e99b5-127"><a name="add-authentication"></a>Lägg till autentisering tooapp</span><span class="sxs-lookup"><span data-stu-id="e99b5-127"><a name="add-authentication"></a>Add authentication tooapp</span></span>
<span data-ttu-id="e99b5-128">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="e99b5-128">**Objective-C**:</span></span>

1. <span data-ttu-id="e99b5-129">Öppna på en Mac *QSTodoListViewController.m* i Xcode och Lägg till följande metod hello:</span><span class="sxs-lookup"><span data-stu-id="e99b5-129">On your Mac, open *QSTodoListViewController.m* in Xcode and add hello following method:</span></span>

    ```Objective-C
    - (void)loginAndGetData
    {
        QSAppDelegate *appDelegate = (QSAppDelegate *)[UIApplication sharedApplication].delegate;
        appDelegate.qsTodoService = self.todoService;

        [self.todoService.client loginWithProvider:@"google" urlScheme:@"appname" controller:self animated:YES completion:^(MSUser * _Nullable user, NSError * _Nullable error) {
            if (error) {
                NSLog(@"Login failed with error: %@, %@", error, [error userInfo]);
            }
            else {
                self.todoService.client.currentUser = user;
                NSLog(@"User logged in: %@", user.userId);

                [self refresh];
            }
        }];
    }
    ```

    <span data-ttu-id="e99b5-130">Ändra *google* för*microsoftaccount*, *twitter*, *facebook*, eller *windowsazureactivedirectory* om du inte använder Google som identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="e99b5-130">Change *google* too*microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="e99b5-131">Om du använder Facebook, måste du [godkända Facebook domäner] [ 1] i din app.</span><span class="sxs-lookup"><span data-stu-id="e99b5-131">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="e99b5-132">Ersätt hello **urlScheme** med ett unikt namn för ditt program.</span><span class="sxs-lookup"><span data-stu-id="e99b5-132">Replace hello **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="e99b5-133">Hej urlScheme bör vara hello samma som hello URL-schema-protokoll som du angav i hello **tillåtna externa omdirigerings-URL: er** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e99b5-133">hello urlScheme should be hello same as hello URL Scheme protocol that you specified in hello **Allowed External Redirect URLs** field in hello Azure portal.</span></span> <span data-ttu-id="e99b5-134">Hej urlScheme används av hello autentisering återanrop tooswitch tillbaka tooyour programmet när autentiseringsbegäran är klar.</span><span class="sxs-lookup"><span data-stu-id="e99b5-134">hello urlScheme is used by hello authentication callback tooswitch back tooyour application after the authentication request is complete.</span></span>

2. <span data-ttu-id="e99b5-135">Ersätt `[self refresh]` i `viewDidLoad` i *QSTodoListViewController.m* med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="e99b5-135">Replace `[self refresh]` in `viewDidLoad` in *QSTodoListViewController.m* with hello following code:</span></span>

    ```Objective-C
    [self loginAndGetData];
    ```

3. <span data-ttu-id="e99b5-136">Öppna hello `QSAppDelegate.h` och Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="e99b5-136">Open hello `QSAppDelegate.h` file and add hello following code:</span></span>

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. <span data-ttu-id="e99b5-137">Öppna hello `QSAppDelegate.m` och Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="e99b5-137">Open hello `QSAppDelegate.m` file and add hello following code:</span></span>

    ```Objective-C
    - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
    {
        if ([[url.scheme lowercaseString] isEqualToString:@"appname"]) {
            // Resume login flow
            return [self.qsTodoService.client resumeWithURL:url];
        }
        else {
            return NO;
        }
    }
    ```

   <span data-ttu-id="e99b5-138">Lägg till den här koden direkt innan hello rad läsning `#pragma mark - Core Data stack`.</span><span class="sxs-lookup"><span data-stu-id="e99b5-138">Add this code directly before hello line reading `#pragma mark - Core Data stack`.</span></span>  <span data-ttu-id="e99b5-139">Ersätt den _appname_ med hello urlScheme värde som du använde i steg 1.</span><span class="sxs-lookup"><span data-stu-id="e99b5-139">Replace the _appname_ wih hello urlScheme value that you used in step 1.</span></span>

5. <span data-ttu-id="e99b5-140">Öppna hello `AppName-Info.plist` (ersätter AppName med hello namnet på din app), och Lägg till hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="e99b5-140">Open hello `AppName-Info.plist` file (replacing AppName with hello name of your app), and add hello following code:</span></span>

    ```XML
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    <span data-ttu-id="e99b5-141">Den här koden ska placeras inuti hello `<dict>` element.</span><span class="sxs-lookup"><span data-stu-id="e99b5-141">This code should be placed inside hello `<dict>` element.</span></span>  <span data-ttu-id="e99b5-142">Ersätt hello _appname_ sträng (inom en matris för **CFBundleURLSchemes**) med hello appnamn du valde i steg 1.</span><span class="sxs-lookup"><span data-stu-id="e99b5-142">Replace hello _appname_ string (within the array for **CFBundleURLSchemes**) with hello app name you chose in step 1.</span></span>  <span data-ttu-id="e99b5-143">Du kan också göra dessa ändringar på hello plist editor - Klicka på hello `AppName-Info.plist` filen i XCode tooopen hello plist-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="e99b5-143">You can also make these changes in hello plist editor - click on hello `AppName-Info.plist` file in XCode tooopen hello plist editor.</span></span>

    <span data-ttu-id="e99b5-144">Ersätt hello `com.microsoft.azure.zumo` sträng för **CFBundleURLName** med ditt Apple-paketidentifierare.</span><span class="sxs-lookup"><span data-stu-id="e99b5-144">Replace hello `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

6. <span data-ttu-id="e99b5-145">Tryck på *kör* toostart hello appen och logga sedan in.</span><span class="sxs-lookup"><span data-stu-id="e99b5-145">Press *Run* toostart hello app, and then log in.</span></span> <span data-ttu-id="e99b5-146">När du har loggat in, bör du vara kan tooview hello göra-lista och göra uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="e99b5-146">When you are logged in, you should be able tooview hello Todo list and make updates.</span></span>

<span data-ttu-id="e99b5-147">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="e99b5-147">**Swift**:</span></span>

1. <span data-ttu-id="e99b5-148">Öppna på en Mac *ToDoTableViewController.swift* i Xcode och Lägg till följande metod hello:</span><span class="sxs-lookup"><span data-stu-id="e99b5-148">On your Mac, open *ToDoTableViewController.swift* in Xcode and add hello following method:</span></span>

    ```swift
    func loginAndGetData() {

        guard let client = self.table?.client, client.currentUser == nil else {
            return
        }

        let appDelegate = UIApplication.shared.delegate as! AppDelegate
        appDelegate.todoTableViewController = self

        let loginBlock: MSClientLoginBlock = {(user, error) -> Void in
            if (error != nil) {
                print("Error: \(error?.localizedDescription)")
            }
            else {
                client.currentUser = user
                print("User logged in: \(user?.userId)")
            }
        }

        client.login(withProvider:"google", urlScheme: "appname", controller: self, animated: true, completion: loginBlock)

    }
    ```

    <span data-ttu-id="e99b5-149">Ändra *google* för*microsoftaccount*, *twitter*, *facebook*, eller *windowsazureactivedirectory* om du inte använder Google som identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="e99b5-149">Change *google* too*microsoftaccount*, *twitter*, *facebook*, or *windowsazureactivedirectory* if you are not using Google as your identity provider.</span></span> <span data-ttu-id="e99b5-150">Om du använder Facebook, måste du [godkända Facebook domäner] [ 1] i din app.</span><span class="sxs-lookup"><span data-stu-id="e99b5-150">If you use Facebook, you must [whitelist Facebook domains][1] in your app.</span></span>

    <span data-ttu-id="e99b5-151">Ersätt hello **urlScheme** med ett unikt namn för ditt program.</span><span class="sxs-lookup"><span data-stu-id="e99b5-151">Replace hello **urlScheme** with a unique name for your application.</span></span>  <span data-ttu-id="e99b5-152">Hej urlScheme bör vara hello samma som hello URL-schema-protokoll som du angav i hello **tillåtna externa omdirigerings-URL: er** i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e99b5-152">hello urlScheme should be hello same as hello URL Scheme protocol that you specified in hello **Allowed External Redirect URLs** field in hello Azure portal.</span></span> <span data-ttu-id="e99b5-153">Hej urlScheme används av hello autentisering återanrop tooswitch tillbaka tooyour programmet när autentiseringsbegäran är klar.</span><span class="sxs-lookup"><span data-stu-id="e99b5-153">hello urlScheme is used by hello authentication callback tooswitch back tooyour application after the authentication request is complete.</span></span>

2. <span data-ttu-id="e99b5-154">Ta bort hello rader `self.refreshControl?.beginRefreshing()` och `self.onRefresh(self.refreshControl)` i slutet av `viewDidLoad()` i *ToDoTableViewController.swift*.</span><span class="sxs-lookup"><span data-stu-id="e99b5-154">Remove hello lines `self.refreshControl?.beginRefreshing()` and `self.onRefresh(self.refreshControl)` at the end of `viewDidLoad()` in *ToDoTableViewController.swift*.</span></span> <span data-ttu-id="e99b5-155">Lägg till ett anrop för`loginAndGetData()` i stället:</span><span class="sxs-lookup"><span data-stu-id="e99b5-155">Add a call too`loginAndGetData()` in their place:</span></span>

    ```swift
    loginAndGetData()
    ```

3. <span data-ttu-id="e99b5-156">Öppna hello `AppDelegate.swift` och Lägg till följande rad toohello hello `AppDelegate` klass:</span><span class="sxs-lookup"><span data-stu-id="e99b5-156">Open hello `AppDelegate.swift` file and add hello following line toohello `AppDelegate` class:</span></span>

    ```swift
    var todoTableViewController: ToDoTableViewController?

    func application(_ application: UIApplication, openURL url: NSURL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        if url.scheme?.lowercased() == "appname" {
            return (todoTableViewController!.table?.client.resume(with: url as URL))!
        }
        else {
            return false
        }
    }
    ```

    <span data-ttu-id="e99b5-157">Ersätt hello _appname_ med hello urlScheme värde som du använde i steg 1.</span><span class="sxs-lookup"><span data-stu-id="e99b5-157">Replace hello _appname_ wih hello urlScheme value that you used in step 1.</span></span>

4. <span data-ttu-id="e99b5-158">Öppna hello `AppName-Info.plist` (ersätter AppName med hello namnet på din app), och Lägg till hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="e99b5-158">Open hello `AppName-Info.plist` file (replacing AppName with hello name of your app), and add hello following code:</span></span>

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    <span data-ttu-id="e99b5-159">Den här koden ska placeras inuti hello `<dict>` element.</span><span class="sxs-lookup"><span data-stu-id="e99b5-159">This code should be placed inside hello `<dict>` element.</span></span>  <span data-ttu-id="e99b5-160">Ersätt hello _appname_ sträng (inom en matris för **CFBundleURLSchemes**) med hello appnamn du valde i steg 1.</span><span class="sxs-lookup"><span data-stu-id="e99b5-160">Replace hello _appname_ string (within the array for **CFBundleURLSchemes**) with hello app name you chose in step 1.</span></span>  <span data-ttu-id="e99b5-161">Du kan också göra dessa ändringar på hello plist editor - Klicka på hello `AppName-Info.plist` filen i XCode tooopen hello plist-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="e99b5-161">You can also make these changes in hello plist editor - click on hello `AppName-Info.plist` file in XCode tooopen hello plist editor.</span></span>

    <span data-ttu-id="e99b5-162">Ersätt hello `com.microsoft.azure.zumo` sträng för **CFBundleURLName** med ditt Apple-paketidentifierare.</span><span class="sxs-lookup"><span data-stu-id="e99b5-162">Replace hello `com.microsoft.azure.zumo` string for **CFBundleURLName** with your Apple bundle identifier.</span></span>

5. <span data-ttu-id="e99b5-163">Tryck på *kör* toostart hello appen och logga sedan in.</span><span class="sxs-lookup"><span data-stu-id="e99b5-163">Press *Run* toostart hello app, and then log in.</span></span> <span data-ttu-id="e99b5-164">När du har loggat in, bör du vara kan tooview hello göra-lista och göra uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="e99b5-164">When you are logged in, you should be able tooview hello Todo list and make updates.</span></span>

<span data-ttu-id="e99b5-165">Användning-autentisering används äpplen Inter App kommunikation.</span><span class="sxs-lookup"><span data-stu-id="e99b5-165">App Service Authentication uses Apples Inter-App Communication.</span></span>  <span data-ttu-id="e99b5-166">Mer information om detta finns i toohello [Apples dokumentation][2]</span><span class="sxs-lookup"><span data-stu-id="e99b5-166">For more details on this subject, refer toohello [Apple Documentation][2]</span></span>
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[Azure-portalen]: https://portal.azure.com

[iOS snabb start]: app-service-mobile-ios-get-started.md


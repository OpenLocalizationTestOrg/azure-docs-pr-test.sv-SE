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
# <a name="add-authentication-tooyour-ios-app"></a>Lägg till autentisering tooyour iOS-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

I kursen får du lägger till autentisering toohello [iOS snabb start] projekt med en identitetsprovider som stöds. Den här kursen är baserad på hello [iOS snabb start] självstudien måste du slutföra först.

## <a name="register"></a>Registrera din app för autentisering och konfigurera hello Apptjänst
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL

Säker autentisering måste du definiera en ny URL-schema för din app.  Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar.  I den här självstudiekursen kommer vi använda URL-schemat _appname_ i hela.  Du kan dock använda alla URL-schema som du väljer.  Det bör vara unikt tooyour mobila program.  tooenable hello omdirigering på serversidan th:

1. I hello [Azure-portalen], Välj din Apptjänst.

2. Klicka på hello **autentisering / auktorisering** menyalternativet.

3. Klicka på **Azure Active Directory** under hello **autentiseringsproviders** avsnitt.

4. Ange hello **hanteringsläge** för**Avancerat**.

5. I hello **tillåtna externa omdirigerings-URL: er**, ange `appname://easyauth.callback`.  Hej _appname_ i den här strängen är hello URL-schema för din mobila program.  Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).  Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.

6. Klicka på **OK**.

7. Klicka på **Spara**.

## <a name="permissions"></a>Begränsa behörigheter tooauthenticated användare
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Tryck på i Xcode **kör** toostart hello app. Ett undantagsfel genereras eftersom hello app försöker tooaccess backend som en oautentiserad användare, men hello *TodoItem* tabellen nu kräver autentisering.

## <a name="add-authentication"></a>Lägg till autentisering tooapp
**Objective-C**:

1. Öppna på en Mac *QSTodoListViewController.m* i Xcode och Lägg till följande metod hello:

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

    Ändra *google* för*microsoftaccount*, *twitter*, *facebook*, eller *windowsazureactivedirectory* om du inte använder Google som identitetsprovider. Om du använder Facebook, måste du [godkända Facebook domäner] [ 1] i din app.

    Ersätt hello **urlScheme** med ett unikt namn för ditt program.  Hej urlScheme bör vara hello samma som hello URL-schema-protokoll som du angav i hello **tillåtna externa omdirigerings-URL: er** i hello Azure-portalen. Hej urlScheme används av hello autentisering återanrop tooswitch tillbaka tooyour programmet när autentiseringsbegäran är klar.

2. Ersätt `[self refresh]` i `viewDidLoad` i *QSTodoListViewController.m* med hello följande kod:

    ```Objective-C
    [self loginAndGetData];
    ```

3. Öppna hello `QSAppDelegate.h` och Lägg till följande kod hello:

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. Öppna hello `QSAppDelegate.m` och Lägg till följande kod hello:

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

   Lägg till den här koden direkt innan hello rad läsning `#pragma mark - Core Data stack`.  Ersätt den _appname_ med hello urlScheme värde som du använde i steg 1.

5. Öppna hello `AppName-Info.plist` (ersätter AppName med hello namnet på din app), och Lägg till hello följande kod:

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

    Den här koden ska placeras inuti hello `<dict>` element.  Ersätt hello _appname_ sträng (inom en matris för **CFBundleURLSchemes**) med hello appnamn du valde i steg 1.  Du kan också göra dessa ändringar på hello plist editor - Klicka på hello `AppName-Info.plist` filen i XCode tooopen hello plist-redigeraren.

    Ersätt hello `com.microsoft.azure.zumo` sträng för **CFBundleURLName** med ditt Apple-paketidentifierare.

6. Tryck på *kör* toostart hello appen och logga sedan in. När du har loggat in, bör du vara kan tooview hello göra-lista och göra uppdateringar.

**SWIFT**:

1. Öppna på en Mac *ToDoTableViewController.swift* i Xcode och Lägg till följande metod hello:

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

    Ändra *google* för*microsoftaccount*, *twitter*, *facebook*, eller *windowsazureactivedirectory* om du inte använder Google som identitetsprovider. Om du använder Facebook, måste du [godkända Facebook domäner] [ 1] i din app.

    Ersätt hello **urlScheme** med ett unikt namn för ditt program.  Hej urlScheme bör vara hello samma som hello URL-schema-protokoll som du angav i hello **tillåtna externa omdirigerings-URL: er** i hello Azure-portalen. Hej urlScheme används av hello autentisering återanrop tooswitch tillbaka tooyour programmet när autentiseringsbegäran är klar.

2. Ta bort hello rader `self.refreshControl?.beginRefreshing()` och `self.onRefresh(self.refreshControl)` i slutet av `viewDidLoad()` i *ToDoTableViewController.swift*. Lägg till ett anrop för`loginAndGetData()` i stället:

    ```swift
    loginAndGetData()
    ```

3. Öppna hello `AppDelegate.swift` och Lägg till följande rad toohello hello `AppDelegate` klass:

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

    Ersätt hello _appname_ med hello urlScheme värde som du använde i steg 1.

4. Öppna hello `AppName-Info.plist` (ersätter AppName med hello namnet på din app), och Lägg till hello följande kod:

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

    Den här koden ska placeras inuti hello `<dict>` element.  Ersätt hello _appname_ sträng (inom en matris för **CFBundleURLSchemes**) med hello appnamn du valde i steg 1.  Du kan också göra dessa ändringar på hello plist editor - Klicka på hello `AppName-Info.plist` filen i XCode tooopen hello plist-redigeraren.

    Ersätt hello `com.microsoft.azure.zumo` sträng för **CFBundleURLName** med ditt Apple-paketidentifierare.

5. Tryck på *kör* toostart hello appen och logga sedan in. När du har loggat in, bör du vara kan tooview hello göra-lista och göra uppdateringar.

Användning-autentisering används äpplen Inter App kommunikation.  Mer information om detta finns i toohello [Apples dokumentation][2]
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[Azure-portalen]: https://portal.azure.com

[iOS snabb start]: app-service-mobile-ios-get-started.md


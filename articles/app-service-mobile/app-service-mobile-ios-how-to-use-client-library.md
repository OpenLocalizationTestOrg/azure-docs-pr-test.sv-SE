---
title: "aaaHow tooUse iOS SDK för Azure Mobile Apps"
description: "Hur tooUse iOS SDK för Azure Mobile Apps"
services: app-service\mobile
documentationcenter: ios
author: ysxu
manager: yochayk
editor: 
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
ms.openlocfilehash: fa299ab3f152bad12d821832fa9fb5495d1fa296
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ios-client-library-for-azure-mobile-apps"></a>Hur tooUse iOS-klientbiblioteket för Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Den här guiden lär du dig tooperform vanliga scenarier med hjälp av hello senaste [Azure Mobile Apps iOS SDK][1]. Om du är ny tooAzure Mobile Apps först slutföra [Azure Mobile Apps Snabbstart] toocreate en serverdel skapa en tabell och hämta en förskapad iOS Xcode-projekt. I den här guiden fokuserar på hello klientsidan iOS SDK. toolearn mer om hello serversidan SDK för hello backend, se hello Server SDK HOWTOs.

## <a name="reference-documentation"></a>Referensdokumentation
hello referensdokumentationen för klient-hello iOS SDK finns här: [Azure Mobile Apps iOS klienten referens][2].

## <a name="supported-platforms"></a>Plattformar som stöds
hello iOS SDK stöder Objective-C-projekt, Swift 2.2 projekt och Swift 2.3 projekt för iOS version 8.0 eller senare.

Hej ”server flöde” autentisering använder en webbvy för hello som visas i Användargränssnittet.  Om hello enheten inte kan toopresent en WebView UI, och sedan en annan metod för autentisering krävs är som utanför hello omfattning hello produkten.  
Detta SDK lämpar sig därför inte för titta på typen eller begränsade enheter på samma sätt.

## <a name="Setup"></a>Installationen och förutsättningar
Den här handboken förutsätts att du har skapat en serverdel med en tabell. Den här guiden förutsätter att hello-tabellen har samma schema som hello tabeller i dessa självstudier. Den här guiden förutsätter också att i din kod måste du referera till `MicrosoftAzureMobile.framework` och importera `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

## <a name="create-client"></a>Så här: skapa klienten
tooaccess en Azure Mobile Apps-serverdel i projektet, skapa en `MSClient`. Ersätt `AppUrl` med hello app-URL. Du kan lämna `gatewayURLString` och `applicationKey` tom. Om du ställer in en gateway för autentisering fylla `gatewayURLString` med hello gateway-URL.

**Objective-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**SWIFT**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <a name="table-reference"></a>Så här: skapa tabellreferens
tooaccess eller uppdatera data, skapa en referenstabell toohello backend. Ersätt `TodoItem` med hello namnet på din tabell

**Objective-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**SWIFT**:

```
let table = client.tableWithName("TodoItem")
```


## <a name="querying"></a>Så här: fråga Data
toocreate en databasfråga frågan hello `MSTable` objekt. hello följande fråga hämtar alla hello-objekt i `TodoItem` och loggar hello text för varje objekt.

**Objective-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="filtering"></a>Så här: filtret returnerade Data
toofilter resultat, det finns många tillgängliga alternativ.

toofilter med hjälp av ett predikat, Använd en `NSPredicate` och `readWithPredicate`. hello följande filtrerar returnerade data toofind endast ofullständiga Todo-objekt.

**Objective-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query hello TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query hello TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="query-object"></a>Så här: använda MSQuery
tooperform en komplex fråga, inklusive sortering och sidindelning, skapa ett `MSQuery` objekt, direkt eller genom att använda ett predikat:

**Objective-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**SWIFT**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`kan du styra flera olika beteenden för frågan.

* Ange ordning av resultat
* Begränsa vilka fält tooreturn
* Begränsa hur många poster tooreturn
* Ange totalt antal i svaret
* Ange anpassad fråga string-parametrar i begäran
* Tillämpa ytterligare funktioner

Köra en `MSQuery` frågan genom att anropa `readWithCompletion` på hello-objekt.

## <a name="sorting"></a>Så här: sortera Data med MSQuery
toosort resultat ska vi titta på ett exempel. toosort efter fältet 'text' stigande sedan efter ”klar” fallande anropa `MSQuery` t.ex:

**Objective-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Så här: begränsa fält och expandera frågeparametrar sträng med MSQuery
toolimit fält toobe returneras i en fråga, ange hello namnen på hello fält i hello **selectFields** egenskapen. Det här exemplet returnerar endast hello text och slutförda fält:

**Objective-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**SWIFT**:

```
query.selectFields = ["text", "complete"]
```

tooinclude ytterligare sträng frågeparametrar Hej server begär (till exempel eftersom ett anpassat skript för serversidan använder dem) kan fylla `query.parameters` t.ex:

**Objective-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**SWIFT**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Så här: Konfigurera sidstorlek
Med Azure Mobile Apps hello hello storlek kontroller antalet poster som hämtas i taget från hello backend-tabeller. Ett anrop för`pull` data skulle sedan batch in data, baserat på den här sidstorleken tills det inte finns några fler poster toopull.

Det är möjligt tooconfigure en storlek med **MSPullSettings** enligt nedan. hello sidan standardstorleken är 50 och hello exemplet nedan ändrar too3.

Du kan konfigurera en annan sidstorlek av prestandaskäl. Om du har ett stort antal små dataposter minskar en hög sidstorlek hello antalet turer för servern.

Den här inställningen styr endast hello sidstorleken på hello på klientsidan. Om hello klienten begär en större storlek än hello Mobile Apps-serverdel stöder, hello storlek är begränsad till hello maximala hello backend är konfigurerade toosupport.

Den här inställningen är också hello *nummer* dataposter, inte hello *bytestorlek*.

Om du ökar hello sidstorleken för klienten, bör du också öka hello sidstorleken på hello-servern. Se [”så här: justera hello tabell växlingsfilens storlek”](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för hello steg toodo.

**Objective-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


**SWIFT**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="inserting"></a>Så här: Infoga Data
tooinsert en ny tabellrad, skapa en `NSDictionary` och anropa `table insert`. Om [dynamiska schemat] är aktiverad, hello Azure App Service mobilserverdel genererar automatiskt nya kolumner som är baserat på hello `NSDictionary`.

Om `id` har inte angetts hello backend genererar automatiskt ett nytt unikt ID. Ange en egen `id` toouse e-postadresser, användarnamn eller dina egna anpassade värden som ID. Tillhandahåller egna ID kan underlätta kopplingar och affärsorienterade databasen logik.

Hej `result` innehåller hello nya objekt som har infogats. Det kan ha ytterligare beroende på din server-logik eller ändrade data jämfört med toowhat skickades toohello server.

**Objective-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <a name="modifying"></a>Så här: ändra Data
tooupdate en befintlig rad ändra ett objekt och anropet `update`:

**Objective-C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Du kan också ange hello rad-ID och hello uppdateras fältet:

**Objective-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Minimum hello `id` attributet måste anges när du gör uppdateringar.

## <a name="deleting"></a>Så här: ta bort Data
toodelete ett objekt, anropa `delete` hello objektet:

**Objective-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Du kan också ta bort genom att tillhandahålla en rad-ID:

**Objective-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Minimum hello `id` attributet måste anges när att tas bort.

## <a name="customapi"></a>Så här: anropa anpassade API
Med en anpassad API kan du exponera backend funktioner. Den har inte toomap tooa tabellen igen. Inte bara gör du få bättre kontroll över meddelanden, du kan även läsa/set sidhuvuden och ändra hello svar body-formatet. hur toocreate en anpassad API på hello serverdel läsa toolearn [anpassade API: er](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

toocall en anpassad API anropa `MSClient.invokeAPI`. hello förfrågan och svar innehåll behandlas som JSON. toouse andra typer av media, [Använd hello andra överlagring för `invokeAPI` ] [ 5].  toomake en `GET` begäran i stället för en `POST` begära parameter för `HTTPMethod` för`"GET"` och parametern `body` för`nil` (eftersom GET-begäranden saknar meddelandetext.) Om din anpassade API: et stöder andra HTTP-verb, ändra `HTTPMethod` på lämpligt sätt.

**Objective-C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**SWIFT**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <a name="templates"></a>Så här: registrera push-mallar toosend plattformsoberoende meddelanden
tooregister mallar och skicka mallar med din **client.push registerDeviceToken** metod i klientappen.

**Objective-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**SWIFT**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Mallarna är av typen NSDictionary och kan innehålla flera mallar i hello följande format:

**Objective-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**SWIFT**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Alla taggar tas bort från hello begäran för säkerhet.  tooadd taggar tooinstallations eller mallar inom installationer, se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps][4].  toosend meddelanden med mallarna registrerade arbeta med [Notification Hubs API: er][3].

## <a name="errors"></a>Så här: hantera fel
När du anropar en mobila serverdel i Azure App Service hello slutförande block innehåller en `NSError` parameter. När ett fel uppstår i den här parametern är inte är nil. I koden, bör du kontrollera den här parametern och hantera hello fel vid behov, som visas i hello föregående kodfragment.

hello filen [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] definierar hello konstanter `MSErrorResponseKey`, `MSErrorRequestKey`, och `MSErrorServerItemKey`. tooget mer data relaterade toohello fel:

**Objective-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**SWIFT**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Dessutom definierar hello filen konstanter för respektive felkod:

**Objective-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**SWIFT**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Så här: autentiserar användare med hello Active Directory Authentication Library
Du kan använda hello Active Directory Authentication Library (ADAL) toosign användare i ditt program med Azure Active Directory. Flödet för klientautentisering med hjälp av en identitetsleverantör SDK är bättre toousing hello `loginWithProvider:completion:` metod.  Flöde för klientautentisering ger en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.

1. Konfigurera mobilappsserverdelen för AAD-inloggning med följande hello [hur tooconfigure App Service för Active Directory-inloggningen] [ 7] kursen. Se till att toocomplete hello valfritt steg för att registrera en native client-program. För iOS, rekommenderar vi att hello omdirigering URI: N har hello formuläret `<app-scheme>://<bundle-id>`. Mer information finns i hello [ADAL iOS quickstart][8].
2. Installera ADAL med Cocoapods. Redigera din Podfile tooinclude hello definitionen, ersätter **din PROJECT** med hello namnet på ditt Xcode-projekt:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   och hello baljor:

        pod 'ADALiOS'
3. Med hjälp av hello terminalen kör `pod install` från hello katalog som innehåller ditt projekt och öppna sedan hello genereras Xcode-arbetsyta (inte hello-projekt).
4. Lägg till hello följande kod tooyour program, enligt toohello språk som du använder. I varje, se dessa ersättningar:

   * Ersätt **INSERT-UTFÄRDARE-här** med hello namnet hello-klient som du har etablerat ditt program. Formatet som ska vara https://login.microsoftonline.com/contoso.onmicrosoft.com. Det här värdet kan kopieras från hello domän-fliken i Azure Active Directory i hello [klassiska Azure-portalen].
   * Ersätt **INSERT-resurs-ID-här** med hello klient-ID för din mobilappsserverdel. Du kan hämta klient-ID från hello **Avancerat** fliken **inställningarna för Azure Active Directory** hello-portalen.
   * Ersätt **INSERT-klient-ID-här** med hello klient-ID som du kopierade från hello native client-program.
   * Ersätt **INSERT-OMDIRIGERINGS-URI-här** med webbplatsens */.auth/login/done* slutpunkten, med hjälp av hello HTTPS-schema. Det här värdet bör vara densamma för*https://contoso.azurewebsites.net/.auth/login/done*.

**Objective-C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**SWIFT**:

    // add hello following imports tooyour bridging header:
    //        #import <ADALiOS/ADAuthenticationContext.h>
    //        #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Så här: autentiserar användare med hello Facebook SDK för iOS
Du kan använda hello Facebook SDK för iOS toosign användare till programmet med hjälp av Facebook.  Med hjälp av en flödet för klientautentisering är bättre toousing hello `loginWithProvider:completion:` metod.  hello flödet klientautentisering ger en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.

1. Konfigurera mobilappsserverdelen för Facebook-inloggning genom att följa den [hur tooconfigure App Service för Facebook-inloggningen] [ 9] kursen.
2. Installera hello Facebook SDK för iOS med följande hello [Facebook SDK för iOS - komma igång] [ 10] dokumentation. Du kan lägga till hello iOS-plattformen tooyour registrering av befintlig istället för att skapa en app.
3. Facebooks dokumentationen innehåller vissa Objective-C-koden i hello App ombud. Om du använder **Swift**, kan du använda följande översättningar för AppDelegate.swift hello:

        // Add hello following import tooyour bridging header:
        //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }
4. I tillägg tooadding `FBSDKCoreKit.framework` tooyour projekt, även lägga till en referens för`FBSDKLoginKit.framework` i hello samma sätt.
5. Lägg till hello följande kod tooyour program, enligt toohello språk som du använder.

**Objective-C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {        
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**SWIFT**:

    // Add hello following imports tooyour bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Så här: autentiserar användare med Twitter-infrastrukturen för iOS
Du kan använda infrastrukturen för iOS toosign användare till programmet med Twitter. Flöde för klientautentisering är bättre toousing hello `loginWithProvider:completion:` metoden eftersom den innehåller en mer ursprungligt UX känslan och gör det möjligt för ytterligare anpassning.

1. Konfigurera mobilappsserverdelen för Twitter-inloggning med följande hello [hur tooconfigure App Service för Twitter inloggningen](app-service-mobile-how-to-configure-twitter-authentication.md) kursen.
2. Lägg till Fabric tooyour projektet genom följande hello [infrastruktur för iOS - komma igång] dokumentationen och konfigurera TwitterKit.

   > [!NOTE]
   > Som standard skapas Fabric ett Twitter-program. Du kan undvika att skapa ett program genom att registrera hello konsumentnyckel och konsumenthemlighet som du skapat tidigare med hello följande kodavsnitt.    Alternativt kan du ersätta hello konsumentnyckel och konsumenthemlighet värden du anger tooApp Service med hello värden du finns i hello [instrumentpanelen]. Om du väljer det här alternativet kan vara säker på att tooset hello återanrop URL tooa platshållarvärde, som `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.
   >
   >

    Om du väljer toouse hello hemligheter som du skapade tidigare, lägger du till hello följande kod tooyour App ombud:

    **Objective-C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }

    **SWIFT**:

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. Lägg till hello följande kod tooyour program, enligt toohello språk som du använder.

**Objective-C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**SWIFT**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Så här: autentiserar användare med hello Google inloggning SDK för iOS
Du kan använda hello Google inloggning SDK för iOS toosign användare till programmet använder ett Google-konto.  Google presenterade nyligen ändringar tootheir OAuth säkerhetsprinciper.  Dessa principändringar kräver hello användning av Google SDK i hello framtida.

1. Konfigurera mobilappsserverdelen för Google-inloggning med följande hello [hur tooconfigure App Service för Google inloggningen](app-service-mobile-how-to-configure-google-authentication.md) kursen.
2. Installera hello Google SDK för iOS med följande hello [starta Google Sign-In för iOS - integreringen](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumentation. Du kan hoppa över hello ”autentisera med en Backend-servern” avsnittet.
3. Lägg till hello följande tooyour ombud `signIn:didSignInForUser:withError:` metod, enligt toohello språk som du använder.

**Objective-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**SWIFT**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. Kontrollera att du också lägga till hello följande för`application:didFinishLaunchingWithOptions:` i din app delegera, Ersätt ”SERVER_CLIENT_ID” med hello samma ID som du använde tooconfigure Apptjänst i steg 1.

**Objective-C**:

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 **SWIFT**:

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. Lägg till följande kod tooyour program i en UIViewController som implementerar hello hello `GIDSignInUIDelegate` protokoll, enligt toohello språk som du använder.  Du loggas innan som signeras igen och även om du inte behöver tooenter dina autentiseringsuppgifter igen kan du se en dialogruta för medgivande.  Endast anropa denna metod när hello sessionstoken har gått ut.

   **Objective-C**:

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   **SWIFT**:

       // ...
       func authenticate() {
           GIDSignIn.sharedInstance().uiDelegate = self
           GIDSignIn.sharedInstance().signOut()
           GIDSignIn.sharedInstance().signIn()
       }

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create hello Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data toohello user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize hello client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile Apps Quick Start]: app-service-mobile-ios-get-started.md

[Add Mobile Services tooExisting App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts tooauthorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynamisk Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI toomanage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Instrumentpanelen]: https://www.fabric.io/home
[Infrastruktur för iOS - komma igång]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started

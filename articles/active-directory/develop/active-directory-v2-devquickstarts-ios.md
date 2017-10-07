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
# <a name="add-sign-in-tooan-ios-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a>Lägga till inloggning tooan iOS-app med en tredjeparts-bibliotek med Graph API: et med hello v2.0-slutpunkten
hello Microsoft identity-plattformen använder öppna standarder, till exempel OAuth2 och OpenID Connect. Utvecklare kan använda alla bibliotek som de vill toointegrate med våra tjänster. toohelp utvecklare använda vår plattform med andra bibliotek, vi har skrivit några genomgång som den här en toodemonstrate hur tooconfigure från tredje part bibliotek tooconnect toohello Microsoft identity-plattformen. De flesta bibliotek som implementerar [hello RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kan ansluta toohello Microsoft identity-plattformen.

Med hello program som skapar den här genomgången, kan användare logga in tootheir organisation och söka efter andra i organisationen med hjälp av hello Graph API.

Om du är ny tooOAuth2 eller OpenID Connect är mycket av det här exemplet konfigurationen inte meningsfullt tooyou. Vi rekommenderar att du läser [v2.0-protokoll - OAuth 2.0 auktorisering kod flöda](active-directory-v2-protocols-oauth-code.md) för bakgrunden.

> [!NOTE]
> Vissa funktioner i vår plattform som har ett uttryck i hello OAuth2 eller OpenID Connect standarder, till exempel villkorlig åtkomst och hantering av Intune behöver du toouse våra Microsoft Azure identitet bibliotek med öppen källkod.
> 
> 

hello v2.0-slutpunkten har inte stöd för alla Azure Active Directory-scenarier och funktioner.

> [!NOTE]
> toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

## <a name="download-code-from-github"></a>Hämta koden från GitHub
hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) eller klona hello stommen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Du kan också hämta hello exempel och komma igång nu direkt:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Registrera en app
Skapa en ny app på hello [programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följ hello detaljerade anvisningar på [hur tooregister en app med hello v2.0-slutpunkten](active-directory-v2-app-registration.md).  Se till att:

* Kopiera hello **program-Id** som är tilldelade tooyour app eftersom du behöver den snart.
* Lägg till hello **Mobile** plattform för din app.
* Kopiera hello **omdirigerings-URI** från hello-portalen. Du måste använda hello standardvärdet `urn:ietf:wg:oauth:2.0:oob`.

## <a name="download-hello-third-party-nxoauth2-library-and-create-a-workspace"></a>Hämta hello från tredje part NXOAuth2 bibliotek och skapa en arbetsyta
Den här genomgången använder hello OAuth2Client från GitHub, vilket är en OAuth2-biblioteket för Mac OS X och iOS (Cocoa och Cocoa touch). Det här biblioteket är baserad på förslag 10 hello OAuth2-specifikationen. Den implementerar hello programspecifika profil och stöder hello autentiseringsslutpunkt för hello användare. Detta är allt du behöver toointegrate med identitetsplattformen för hello Microsoft hello.

### <a name="add-hello-library-tooyour-project-by-using-cocoapods"></a>Lägga till hello biblioteket tooyour projekt med CocoaPods
CocoaPods är en beroendehanterare för Xcode-projekt. Den hanterar hello föregående installationssteg automatiskt.

```
$ vi Podfile
```
1. Lägg till följande toothis podfile hello:
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. Läsa in hello podfile med CocoaPods. Då skapas en ny Xcode-arbetsyta som ska läsas in.
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-hello-structure-of-hello-project"></a>Utforska hello strukturen för hello-projekt
hello efter strukturen har ställts in för projektet i hello stommen:

* En bakgrundsläge med en UPN-sökning
* En detaljerad vy för hello data om hello valda användare
* Vyn inloggning där en användare kan logga in toohello app tooquery hello diagram

Vi kommer att flyttas toovarious filer i hello stommen tooadd autentisering. Andra delar av hello kod, exempelvis hello visual kod, gäller inte tooidentity men du.

## <a name="set-up-hello-settingsplst-file-in-hello-library"></a>Ställ in hello settings.plst fil i hello-bibliotek
* Öppna hello i hello snabbstartsprojekt `settings.plist` fil. Ersätt hello värdena för hello element i hello avsnittet tooreflect hello värden som du använde i hello Azure-portalen. Koden ska referera till dessa värden när den använder hello Active Directory Authentication Library.
  * Hej `clientId` är hello klient-ID för programmet som du kopierade från hello-portalen.
  * Hej `redirectUri` är hello omdirigerings-URL som hello-portalanvändare som tillhandahålls.

## <a name="set-up-hello-nxoauth2client-library-in-your-loginviewcontroller"></a>Ställ in hello NXOAuth2Client bibliotek i din LoginViewController
Hej NXOAuth2Client innehållsbiblioteket måste vissa värden tooget ställa in. När du har gjort det, kan du använda hello anskaffats token toocall hello Graph API. Eftersom `LoginView` anropas när vi behöver tooauthenticate, är det klokt tooput konfigurationsvärden i toothat-filen.

* Lägg till vissa värden toohello `LoginViewController.m` filen tooset hello kontext för autentisering och auktorisering. Information om hello värden Följ hello kod.
  
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

Nu ska vi titta på detaljer om hello kod.

hello första strängen är för `scopes`.  Hej `User.Read` värde kan du tooread hello grundprofil hello inloggad användare.

Du kan lära dig mer om alla tillgängliga hello-scope på [behörighetsomfattningen för Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).

För `authURL`, `loginURL`, `bhh`, och `tokenURL`, bör du använda hello värden som tidigare. Om du använder hello Microsoft Azure identitet bibliotek med öppen källkod, hämtar vi informationen du med hjälp av vår metadataslutpunkten. Vi har gjort hello tunga arbetet med att extrahera värdena för dig.

Hej `keychain` värde är hello-behållare som hello NXOAuth2Client biblioteket använder toocreate en nyckelringar toostore dina token. Om du vill tooget enkel inloggning (SSO) över flera appar kan du ange hello samma nyckelringar i var och en av dina program och begära hello användning av den nyckelringen i Xcode-rättigheter. Detta förklaras i hello Apples dokumentation.

hello resten av värdena är obligatoriska toouse hello bibliotek och skapa platser du toocarry värden toohello kontext.

### <a name="create-a-url-cache"></a>Skapa en URL-cache
I `(void)viewDidLoad()`, vilket kallas alltid efter hello vyn har lästs in, hello följande kod primes ett cacheminne för våra användning.

Lägg till följande kod hello:

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

### <a name="create-a-webview-for-sign-in"></a>Skapa en webbvy för inloggning
En webbvy kan fråga hello användaren om faktorer som SMS-textmeddelande (om konfigurerad) eller returnera fel meddelanden toohello användare. Här ska du ange hello webbvy och sedan skriva hello kod toohandle hello återanrop sker i hello webbvy från hello identity services.

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

### <a name="override-hello-webview-methods-toohandle-authentication"></a>Åsidosätt hello webbvy metoder toohandle autentisering
tootell hello webbvy vad som händer när en användare behöver toosign i vilket beskrivs ovan, kan du klistra in hello följande kod.

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

### <a name="write-code-toohandle-hello-result-of-hello-oauth2-request"></a>Skriva koden toohandle hello resultatet av hello OAuth2 begäran
hello hanterar följande kod hello RedirectUrl anges som returnerar från hello webbvy. Om autentisering inte var lyckade försök hello kod igen. Under tiden ger hello biblioteket hello-fel som du kan se i hello-konsolen eller hantera asynkront.

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

### <a name="set-up-hello-oauth-context-called-account-store"></a>Ställ in hello OAuth-kontexten (kallas kontoarkiv)
Här kan du anropa `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` på hello delade kontoarkiv för varje tjänst som du vill hello programmet toobe kan tooaccess. hello kontotypen är en sträng som används som en identifierare för en viss tjänst. Eftersom du kommer åt hello Graph API hello koden refererar tooit som `"myGraphService"`. Du ställa in en person som anger när något ändras med hello-token. När du får hello token kan du gå tillbaka hello användaren tillbaka toohello `masterView`.

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

## <a name="set-up-hello-master-view-toosearch-and-display-hello-users-from-hello-graph-api"></a>Ställa in hello bakgrundsläge toosearch och visa hello användare från hello Graph API
En app i Master-View-Controller (MVC) som visar hello returnerade data i hello rutnät ligger utanför hello i den här genomgången och många online självstudier förklarar hur toobuild en. Den här koden är i stommen hello-filen. Du behöver dock toodeal med några saker i den här MVC-program:

* Fånga upp när användaren skriver något i sökfältet hello
* Ange ett objekt av data tillbaka toohello MasterView så hello resultat kan visas i rutnätet hello

Vi ska göra de nedan.

### <a name="add-a-check-toosee-if-youre-logged-in"></a>Lägg till en kontroll toosee om du är inloggad
hello program har lite om hello användaren inte är inloggad, så att den är smart toocheck om det finns redan en token i hello-cachen. Om inte du dirigerar toohello kontollen för hello användaren toosign i. Om du kommer ihåg hello bästa sätt toodo åtgärder när vyn läses in är toouse hello `viewDidLoad()` metod som Apple ger oss.

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

### <a name="update-hello-table-view-when-data-is-received"></a>Uppdatera hello tabellvy när data tas emot
När hello Graph API returnerar data, måste toodisplay hello data. Här är alla hello kod tooupdate hello tabellen för enkelhetens skull. Du kan bara klistra in hello rätt värden i MVC formaterad koden.

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

### <a name="provide-a-way-toocall-hello-graph-api-when-someone-types-in-hello-search-field"></a>Ange en sätt toocall hello Graph API när någon skriver i sökfältet hello
När en användare skriver en sökning i hello ruta, måste tooshove som över toohello Graph API. Hej `GraphAPICaller` -klassen, som du skapar i hello följande kod, separerar hello lookup-funktioner från hello presentation. Nu är dags att skriva hello-kod som alla Sök tecken toohello Graph API-flöden. Vi kan göra detta genom att tillhandahålla en metod som kallas `lookupInGraph`, som tar hello sträng som vi vill toosearch för.

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

## <a name="write-a-helper-class-tooaccess-hello-graph-api"></a>Skriva ett Helper klassen tooaccess hello Graph API
Detta är hello kärnan i vårt program. Medan hello rest Infoga kod i hello MVC Standardmönster från Apple, skriva här du kod tooquery hello diagram som hello användaren skriver och returnera dessa data. En detaljerad förklaring följer det här är hello kod.

### <a name="create-a-new-objective-c-header-file"></a>Skapa en ny rubrikfil i Objective C
Namnet hello filen `GraphAPICaller.h`, och Lägg till följande kod hello.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Här ser du att en angivna metoden tar en sträng och returnerar ett completionBlock. Den här completionBlock uppdateras som du kan ha gissa hello tabell genom att ange ett objekt med ifyllda data i realtid som hello användare söka.

### <a name="create-a-new-objective-c-file"></a>Skapa en ny fil i Objective C
Namnet hello filen `GraphAPICaller.m`, och Lägg till följande metod hello.

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

Vi går igenom den här metoden i detalj.

hello kärnan i den här koden är i hello `NXOAuth2Request`, metod som använder hello parametrar som du redan har definierat i hello settings.plist-filen.

hello första steget är tooconstruct hello rätt Graph API-anrop. Eftersom du anropar `/users`, du anger att genom att lägga till den toohello Graph API resurs tillsammans med hello version. Gör det klokt tooput dessa i en extern inställningsfil eftersom de kan ändra hello API utvecklas.

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Sedan måste toospecify parametrar som du kan även ange toohello Graph API-anrop. Det är *viktigt* att du inte anger hello parametrar i hello resurs slutpunkt eftersom som befordras för alla icke-URI förväntad tecken vid körning. All kod som frågan måste anges i hello parametrar.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Du kan se detta anropar en `convertParamsToDictionary` metod som du ännu inte har skrivits. Låt oss göra nu hello slutet av filen hello:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Nu ska vi använda hello `NXOAuth2Request` metoden tooget data tillbaka från hello API i JSON-format.

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

Slutligen kan du nu ska vi titta på hur du returnerar hello data toohello MasterViewController. hello data returnerar som serialiseras och måste toobe avserialiseras och lästs in i ett objekt som hello MainViewController kan förbruka. För detta ändamål hello stommen har en `User.m/h` -fil som skapar ett användarobjekt. Du kan fylla det användarobjektet med information från hello diagram.

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


## <a name="run-hello-sample"></a>Kör hello-exempel
Om du har används hello stommen eller följt tillsammans med hello genomgången ditt program nu ska köras. Starta hello simulator och på **logga in** toouse hello program.

## <a name="get-security-updates-for-our-product"></a>Hämta säkerhetsuppdateringar för vår produkt
Vi rekommenderar att du tooget meddelanden om när säkerhetsincidenter genom att besöka hello [säkerhet TechCenter](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.


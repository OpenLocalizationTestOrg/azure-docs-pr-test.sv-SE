---
title: Integrera Azure AD i en iOS-app | Microsoft Docs
description: "Hur du skapar en iOS-program som kan integreras med Azure AD för inloggnings- och Azure AD-anrop skyddade API: er med hjälp av OAuth."
services: active-directory
documentationcenter: ios
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: 57f465df99ac234466459b8031f61805d8334b59
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-into-an-ios-app"></a>Integrera Azure AD i en iOS-app
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> Testa förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/iOS) som hjälper dig att komma igång med Azure Active Directory på bara några minuter!  Developer-portalen får du hjälp med att registrera en app och integrera Azure AD i din kod.  När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdel som kan acceptera token och utföra valideringen. 
> 
> 

Azure Active Directory (AD Azure) tillhandahåller Active Directory Authentication Library eller ADAL för iOS-klienter som behöver åtkomst till skyddade resurser. ADAL förenklar som din app använder för att erhålla åtkomsttoken. Att demonstrera hur lätt det är i den här artikeln vi skapa ett uppgiftslista för Objective C-program som:

* Hämtar åtkomst till token för att anropa Azure AD Graph-API med hjälp av den [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Söker en katalog för användare med ett visst alias.

För att skapa fullständiga fungerande program måste du:

1. Registrera ditt program med Azure AD.
2. Installera och konfigurera ADAL.
3. Använd ADAL för att hämta token från Azure AD.

Du kommer igång [ladda ned stommen app](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) eller [hämta det slutförda exemplet](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program. Om du inte redan har en klient [Lär dig hur du skaffa en](active-directory-howto-tenant.md).


> [!TIP]
> Testa förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/iOS) som hjälper dig att komma igång med Azure AD i bara några minuter. Developer-portalen får du hjälp med att registrera en app och integrera Azure AD i din kod. När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdelsnätverk som kan ta emot token och utföra verifiering. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. Bestämma vilka din omdirigering URI är för iOS
Om du vill starta dina program på ett säkert sätt i vissa scenarier för enkel inloggning, måste du skapa en *omdirigerings-URI* i ett visst format. En omdirigerings-URI används för att se till att token tillbaka till rätt program som en förfrågan om dem.


IOS-format för en omdirigerings-URI: N är:

```
<app-scheme>://<bundle-id>
```

* **App-schemat** -detta är registrerad i ditt XCode-projekt. Det är hur andra program kan ringa dig. Du hittar detta under Info.plist -> URL typer -> URL-identifierare. Du bör skapa en om du inte redan har en eller flera konfigurerats.
* **paket-id** -detta är paket-ID hittades under ”identitet” ta bort dina Projektinställningar i XCode.

Ett exempel på den här koden för Snabbstart: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>2. Registrera DirectorySearcher-program
Om du vill konfigurera din app att hämta token måste du först registrera det i Azure AD-klienten och bevilja behörighet att komma åt Azure AD Graph-API:

1. Logga in på [Azure Portal](https://portal.azure.com).
2. Klicka på ditt konto på den översta raden. Under den **Directory** Välj Active Directory-klient som du vill registrera ditt program.
3. Klicka på **fler tjänster** i det vänstra navigeringsfönstret och välj sedan **Azure Active Directory**.
4. Klicka på **App registreringar**, och välj sedan **Lägg till**.
5. Följ anvisningarna för att skapa en ny **internt klientprogram**.
  * Den **namn** beskriver ditt program för slutanvändare av programmet.
  * Den **omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD som används för att returnera token svar.  Ange ett värde som är specifik för ditt program och är baserad på informationen om föregående omdirigerings-URI.
6. När du har slutfört registreringen, tilldelar Azure AD appen ett unikt-ID.  Du behöver det här värdet i nästa avsnitt så kopiera den från programfliken.
7. Från den **inställningar** väljer **nödvändiga behörigheter** och välj sedan **Lägg till**. Välj **Microsoft Graph** som API, och Lägg sedan till den **läsa katalogdata** behörighet under **delegerade behörigheter**.  Detta konfigurerar programmet att frågor till Azure AD Graph API för användare.

## <a name="3-install-and-configure-adal"></a>3. Installera och konfigurera ADAL
Nu när du har ett program i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.  Du måste tillhandahålla information om din appregistrering för ADAL att kommunicera med Azure AD.

1. Börja genom att lägga till ADAL DirectorySearcher projektet med CocoaPods.

    ```
    $ vi Podfile
    ```
2. Lägg till följande i Podfile:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. Läsa in podfile med CocoaPods. Det här steget skapar en ny XCode-arbetsyta som du läser in.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. Öppna filen plist i snabbstartsprojektet `settings.plist`.  Ersätt värdena för elementen i avsnittet för att återspegla de värden som du angav i Azure-portalen. Din kod refererar till dessa värden när den använder ADAL.
  * Den `tenant` är domänen för din Azure AD-klient, till exempel contoso.onmicrosoft.com.
  * Den `clientId` är klient-ID för programmet som du kopierade från portalen.
  * Den `redirectUri` är omdirigerings-URL som du har registrerat i portalen.

## <a name="4----use-adal-to-get-tokens-from-azure-ad"></a>4.    Använd ADAL för att hämta token från Azure AD
Den grundläggande principen bakom ADAL är att när din app behöver en åtkomst-token kan bara anropas en completionBlock `+(void) getToken : `, och ADAL gör resten.  

1. I den `QuickStart` projektet öppnar `GraphAPICaller.m` och leta upp den `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` kommentar längst upp.  Det är där du skickar ADAL koordinaterna via en CompletionBlock att kommunicera med Azure AD och hur det kan cachelagra token.

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. Nu ska vi använda denna token för att söka efter användare i diagrammet. Hitta de `// TODO: implement SearchUsersList` kommentar. Den här metoden gör en GET-begäran till Azure AD Graph API frågan för användare vars UPN-namnet börjar med den angivna söktermen.  Om du vill fråga Azure AD Graph API, måste du inkludera ett access_token i den `Authorization` huvudet i begäran. Det är där ADAL kommer in.

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab the JSON node at the top to get our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by the key name being "value". It really is the name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

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
         }];

    }

    ```


3. När din app begär en token genom att anropa `getToken(...)`, ADAL försöker denna att returnera en token utan att fråga användaren om autentiseringsuppgifter.  Om ADAL avgör att användaren måste logga in att hämta en token, visa en dialogruta för inloggning, samla in användarens autentiseringsuppgifter och sedan returnera en token efter en lyckad autentisering.  Om ADAL inte kan returnera en token av någon anledning, genererar en `AdalException`.

> [!Note] 
> Den `AuthenticationResult` objektet innehåller en `tokenCacheStoreItem` objekt som kan användas för att samla in information som kan behövas för din app. I Snabbstart, `tokenCacheStoreItem` används för att avgöra om autentisering redan har gjort.
>
>

## <a name="5-build-and-run-the-application"></a>5. Skapa och kör appen
Grattis! Nu har du en fungerande iOS-program som kan autentisera användare, på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och få grundläggande information om användaren.  Om du inte redan gjort nu är det dags att fylla i din klient med vissa användare.  Starta appen QuickStart och logga sedan in med något av dessa användare.  Sök efter andra användare baserat på deras UPN.  Stäng appen och sedan starta den igen.  Observera att användarens session förblir intakt.

ADAL gör det enkelt att införliva alla dessa vanliga identity-funktioner i programmet.  Det tar hand om ändrad arbetet, t.ex. hantering av cache OAuth protokollstöd presentera användaren med ett användargränssnitt för att logga in, och uppdatera gått token.  Allt du behöver veta är ett enda API-anrop `getToken`.

Referens tillhandahålls det slutförda exemplet (utan dina konfigurationsvärden) på [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="next-steps"></a>Nästa steg
Du kan nu gå vidare till fler scenarier.  Du kanske vill prova:

* [Skydda en Node.JS-webb-API med Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
* Läs [så att aktivera enkel inloggning mellan appar på iOS använder ADAL](active-directory-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


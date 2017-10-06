---
title: aaaIntegrate Azure AD i en iOS-app | Microsoft Docs
description: "Hur skyddade toobuild ett iOS-program som kan integreras med Azure AD för inloggnings- och Azure AD-anrop API: er med hjälp av OAuth."
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
ms.openlocfilehash: 6e05745b2b2b122995dcba896ab0f2ed32509e3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-ios-app"></a>Integrera Azure AD i en iOS-app
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> Försök hello förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/iOS) som hjälper dig att komma igång med Azure Active Directory på bara några minuter!  hello developer-portalen hjälper dig att hello process för att registrera en app och integrera Azure AD i din kod.  När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdel som kan acceptera token och utföra valideringen. 
> 
> 

Azure Active Directory (AD Azure) tillhandahåller hello Active Directory Authentication Library eller ADAL för iOS-klienter som behöver tooaccess skyddade resurser. ADAL förenklar hello processen att din app använder tooobtain åtkomsttoken. hur lätt det är i den här artikeln toodemonstrate vi skapa ett uppgiftslista för Objective C-program som:

* Hämtar åtkomst till token för att anropa hello Azure AD Graph API med hjälp av hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Söker en katalog för användare med ett visst alias.

toobuild hello fullständig fungerande program, måste du:

1. Registrera ditt program med Azure AD.
2. Installera och konfigurera ADAL.
3. Använd ADAL tooget token från Azure AD.

tooget har startats [hämta hello app stommen](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program. Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).


> [!TIP]
> Försök hello förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/iOS) som hjälper dig att komma igång med Azure AD i bara några minuter. hello developer-portalen hjälper dig att hello process för att registrera en app och integrera Azure AD i din kod. När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdelsnätverk som kan ta emot token och utföra verifiering. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. Bestämma vilka din omdirigering URI är för iOS
toosecurely starta dina program i vissa scenarier för enkel inloggning måste du skapa en *omdirigerings-URI* i ett visst format. En omdirigerings-URI är används tooensure som hello token returnerade toohello rätt program som en förfrågan om dem.


hello iOS format för en omdirigerings-URI: N är:

```
<app-scheme>://<bundle-id>
```

* **App-schemat** -detta är registrerad i ditt XCode-projekt. Det är hur andra program kan ringa dig. Du hittar detta under Info.plist -> URL typer -> URL-identifierare. Du bör skapa en om du inte redan har en eller flera konfigurerats.
* **paket-id** -detta är hello paket-ID hittades under ”identitet” ta bort dina Projektinställningar i XCode.

Ett exempel på den här koden för Snabbstart: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-hello-directorysearcher-application"></a>2. Registrera hello DirectorySearcher program
tooset upp din tooget apptoken, måste du först tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på ditt konto hello översta fältet. Under hello **Directory** Välj hello Active Directory-klient där du vill att tooregister ditt program.
3. Klicka på **fler tjänster** i hello vänstra navigeringsfönstret och välj sedan **Azure Active Directory**.
4. Klicka på **App registreringar**, och välj sedan **Lägg till**.
5. Följ hello uppmanar toocreate en ny **internt klientprogram**.
  * Hej **namn** av hello programmet beskriver programmet tooend användarna.
  * Hej **omdirigerings-Uri** är en kombination av schemat och strängen som använder Azure AD tooreturn token svar.  Ange ett värde som är specifika tooyour program och baserat på informationen för hello tidigare omdirigerings-URI.
6. När du har slutfört registreringen hello tilldelar Azure AD appen ett unikt-ID.  Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello programfliken.
7. Från hello **inställningar** väljer **nödvändiga behörigheter** och välj sedan **Lägg till**. Välj **Microsoft Graph** som hello API, och Lägg sedan till hello **läsa katalogdata** behörighet under **delegerade behörigheter**.  Detta konfigurerar ditt program tooquery hello Azure AD Graph API för användare.

## <a name="3-install-and-configure-adal"></a>3. Installera och konfigurera ADAL
Nu när du har ett program i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.  Du måste tooprovide för ADAL toocommunicate med Azure AD med viss information om din appregistrering.

1. Börja genom att lägga till ADAL toohello DirectorySearcher projekt med CocoaPods.

    ```
    $ vi Podfile
    ```
2. Lägg till följande toothis podfile hello:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. Läsa in hello podfile med CocoaPods. Det här steget skapar en ny XCode-arbetsyta som du läser in.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. Öppna hello plist-fil i hello snabbstartsprojekt `settings.plist`.  Ersätt hello värdena för hello element i hello avsnittet tooreflect hello värden som du angav i hello Azure-portalen. Din kod refererar till dessa värden när den använder ADAL.
  * Hej `tenant` är hello domän för din Azure AD-klient, till exempel contoso.onmicrosoft.com.
  * Hej `clientId` är hello klient-ID för programmet som du kopierade från hello-portalen.
  * Hej `redirectUri` är hello omdirigerings-URL som du har registrerat i hello-portalen.

## <a name="4----use-adal-tooget-tokens-from-azure-ad"></a>4.    Använd ADAL tooget token från Azure AD
hello grundläggande principen bakom ADAL är att när din app behöver en åtkomst-token kan bara anropas en completionBlock `+(void) getToken : `, och ADAL hello rest.  

1. I hello `QuickStart` projektet öppnar `GraphAPICaller.m` och leta upp hello `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` kommentar hello övre delen.  Detta är som du skickar ADAL hello koordinater via en CompletionBlock toocommunicate med Azure AD, och anger hur toocache token.

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
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy toodisplay hello correct mobile UX. You most likely won't need it in your code.
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

2. Nu måste vi toouse denna token toosearch för användare i hello diagram. Hitta hello `// TODO: implement SearchUsersList` kommentar. Den här metoden gör en GET-begäran toohello Azure AD Graph API tooquery för användare vars UPN-namnet börjar med hello angivna sökord.  tooquery hello Azure AD Graph API du behöver tooinclude en access_token i hello `Authorization` huvudet i begäran hello. Det är där ADAL kommer in.

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

                         // We can grab hello JSON node at hello top tooget our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by hello key name being "value". It really is hello name of the
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


3. När din app begär en token genom att anropa `getToken(...)`, ADAL försöker tooreturn en token utan att uppge autentiseringsuppgifter hello användare.  Om ADAL avgör hello användaren måste toosign i tooget en token, visa en dialogruta för inloggning, samla in hello användarens autentiseringsuppgifter och sedan returnera en token efter en lyckad autentisering.  Om ADAL inte kan tooreturn en token av någon anledning, genererar en `AdalException`.

> [!Note] 
> Hej `AuthenticationResult` objektet innehåller en `tokenCacheStoreItem` objekt som kan vara används toocollect hello information som kan behövas för din app. I hello Snabbstart, `tokenCacheStoreItem` är används toodetermine om autentisering redan har gjort.
>
>

## <a name="5-build-and-run-hello-application"></a>5. Skapa och köra programmet hello
Grattis! Nu har du en fungerande iOS-program som kan autentisera användare, på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och få grundläggande information om hello användare.  Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare.  Starta appen QuickStart och logga sedan in med något av dessa användare.  Sök efter andra användare baserat på deras UPN.  Stäng hello appen och sedan starta den igen.  Observera att hello användarens session förblir intakt.

ADAL gör det enkelt tooincorporate alla dessa vanliga identity-funktioner i programmet.  Det hand tar om alla hello ändrad arbete för dig, t.ex. hantering av cache OAuth protokollstöd presenterar en UI toosign i hello användare och uppdatera token har upphört att gälla.  Allt du verkligen behöver tooknow är ett enda API-anrop `getToken`.

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) finns på [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="next-steps"></a>Nästa steg
Du kan nu gå vidare tooadditional scenarier.  Du kanske vill tootry:

* [Skydda en Node.JS-webb-API med Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
* Läs [hur tooenable enkel inloggning mellan appar på iOS använder ADAL](active-directory-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


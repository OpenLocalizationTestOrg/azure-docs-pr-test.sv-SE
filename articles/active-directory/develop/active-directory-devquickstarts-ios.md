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
# <a name="integrate-azure-ad-into-an-ios-app"></a><span data-ttu-id="368c2-103">Integrera Azure AD i en iOS-app</span><span class="sxs-lookup"><span data-stu-id="368c2-103">Integrate Azure AD into an iOS app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="368c2-104">Försök hello förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/iOS) som hjälper dig att komma igång med Azure Active Directory på bara några minuter!</span><span class="sxs-lookup"><span data-stu-id="368c2-104">Try hello preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure Active Directory in just a few minutes!</span></span>  <span data-ttu-id="368c2-105">hello developer-portalen hjälper dig att hello process för att registrera en app och integrera Azure AD i din kod.</span><span class="sxs-lookup"><span data-stu-id="368c2-105">hello developer portal guides you through hello process of registering an app and integrating Azure AD into your code.</span></span>  <span data-ttu-id="368c2-106">När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdel som kan acceptera token och utföra valideringen.</span><span class="sxs-lookup"><span data-stu-id="368c2-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a backend that can accept tokens and perform validation.</span></span> 
> 
> 

<span data-ttu-id="368c2-107">Azure Active Directory (AD Azure) tillhandahåller hello Active Directory Authentication Library eller ADAL för iOS-klienter som behöver tooaccess skyddade resurser.</span><span class="sxs-lookup"><span data-stu-id="368c2-107">Azure Active Directory (Azure AD) provides hello Active Directory Authentication Library, or ADAL, for iOS clients that need tooaccess protected resources.</span></span> <span data-ttu-id="368c2-108">ADAL förenklar hello processen att din app använder tooobtain åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="368c2-108">ADAL simplifies hello process that your app uses tooobtain access tokens.</span></span> <span data-ttu-id="368c2-109">hur lätt det är i den här artikeln toodemonstrate vi skapa ett uppgiftslista för Objective C-program som:</span><span class="sxs-lookup"><span data-stu-id="368c2-109">toodemonstrate how easy it is, in this article we build an Objective C To-Do List application that:</span></span>

* <span data-ttu-id="368c2-110">Hämtar åtkomst till token för att anropa hello Azure AD Graph API med hjälp av hello [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="368c2-110">Gets access tokens for calling hello Azure AD Graph API by using hello [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="368c2-111">Söker en katalog för användare med ett visst alias.</span><span class="sxs-lookup"><span data-stu-id="368c2-111">Searches a directory for users with a given alias.</span></span>

<span data-ttu-id="368c2-112">toobuild hello fullständig fungerande program, måste du:</span><span class="sxs-lookup"><span data-stu-id="368c2-112">toobuild hello complete working application, you need to:</span></span>

1. <span data-ttu-id="368c2-113">Registrera ditt program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="368c2-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="368c2-114">Installera och konfigurera ADAL.</span><span class="sxs-lookup"><span data-stu-id="368c2-114">Install and configure ADAL.</span></span>
3. <span data-ttu-id="368c2-115">Använd ADAL tooget token från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="368c2-115">Use ADAL tooget tokens from Azure AD.</span></span>

<span data-ttu-id="368c2-116">tooget har startats [hämta hello app stommen](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="368c2-116">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span> <span data-ttu-id="368c2-117">Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="368c2-117">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="368c2-118">Om du inte redan har en klient [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="368c2-118">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>


> [!TIP]
> <span data-ttu-id="368c2-119">Försök hello förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/iOS) som hjälper dig att komma igång med Azure AD i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="368c2-119">Try hello preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="368c2-120">hello developer-portalen hjälper dig att hello process för att registrera en app och integrera Azure AD i din kod.</span><span class="sxs-lookup"><span data-stu-id="368c2-120">hello developer portal guides you through hello process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="368c2-121">När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdelsnätverk som kan ta emot token och utföra verifiering.</span><span class="sxs-lookup"><span data-stu-id="368c2-121">When you’re finished, you'll have a simple application that can authenticate users in your tenant, and a back end that can accept tokens and perform validation.</span></span> 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a><span data-ttu-id="368c2-122">1. Bestämma vilka din omdirigering URI är för iOS</span><span class="sxs-lookup"><span data-stu-id="368c2-122">1. Determine what your redirect URI is for iOS</span></span>
<span data-ttu-id="368c2-123">toosecurely starta dina program i vissa scenarier för enkel inloggning måste du skapa en *omdirigerings-URI* i ett visst format.</span><span class="sxs-lookup"><span data-stu-id="368c2-123">toosecurely start your applications in certain SSO scenarios, you must create a *redirect URI* in a particular format.</span></span> <span data-ttu-id="368c2-124">En omdirigerings-URI är används tooensure som hello token returnerade toohello rätt program som en förfrågan om dem.</span><span class="sxs-lookup"><span data-stu-id="368c2-124">A redirect URI is used tooensure that hello tokens return toohello correct application that asked for them.</span></span>


<span data-ttu-id="368c2-125">hello iOS format för en omdirigerings-URI: N är:</span><span class="sxs-lookup"><span data-stu-id="368c2-125">hello iOS format for a redirect URI is:</span></span>

```
<app-scheme>://<bundle-id>
```

* <span data-ttu-id="368c2-126">**App-schemat** -detta är registrerad i ditt XCode-projekt.</span><span class="sxs-lookup"><span data-stu-id="368c2-126">**app-scheme** - This is registered in your XCode project.</span></span> <span data-ttu-id="368c2-127">Det är hur andra program kan ringa dig.</span><span class="sxs-lookup"><span data-stu-id="368c2-127">It is how other applications can call you.</span></span> <span data-ttu-id="368c2-128">Du hittar detta under Info.plist -> URL typer -> URL-identifierare.</span><span class="sxs-lookup"><span data-stu-id="368c2-128">You can find this under Info.plist -> URL types -> URL Identifier.</span></span> <span data-ttu-id="368c2-129">Du bör skapa en om du inte redan har en eller flera konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="368c2-129">You should create one if you don't already have one or more configured.</span></span>
* <span data-ttu-id="368c2-130">**paket-id** -detta är hello paket-ID hittades under ”identitet” ta bort dina Projektinställningar i XCode.</span><span class="sxs-lookup"><span data-stu-id="368c2-130">**bundle-id** - This is hello Bundle Identifier found under "identity" un your project settings in XCode.</span></span>

<span data-ttu-id="368c2-131">Ett exempel på den här koden för Snabbstart: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span><span class="sxs-lookup"><span data-stu-id="368c2-131">An example for this QuickStart code: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span></span>

## <a name="2-register-hello-directorysearcher-application"></a><span data-ttu-id="368c2-132">2. Registrera hello DirectorySearcher program</span><span class="sxs-lookup"><span data-stu-id="368c2-132">2. Register hello DirectorySearcher application</span></span>
<span data-ttu-id="368c2-133">tooset upp din tooget apptoken, måste du först tooregister den i din Azure AD-klient och bevilja behörighet tooaccess hello Azure AD Graph API:</span><span class="sxs-lookup"><span data-stu-id="368c2-133">tooset up your app tooget tokens, you first need tooregister it in your Azure AD tenant and grant it permission tooaccess hello Azure AD Graph API:</span></span>

1. <span data-ttu-id="368c2-134">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="368c2-134">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="368c2-135">Klicka på ditt konto hello översta fältet.</span><span class="sxs-lookup"><span data-stu-id="368c2-135">On hello top bar, click your account.</span></span> <span data-ttu-id="368c2-136">Under hello **Directory** Välj hello Active Directory-klient där du vill att tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="368c2-136">Under hello **Directory** list, choose hello Active Directory tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="368c2-137">Klicka på **fler tjänster** i hello vänstra navigeringsfönstret och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="368c2-137">Click **More Services** in hello leftmost navigation pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="368c2-138">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="368c2-138">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="368c2-139">Följ hello uppmanar toocreate en ny **internt klientprogram**.</span><span class="sxs-lookup"><span data-stu-id="368c2-139">Follow hello prompts toocreate a new **Native Client Application**.</span></span>
  * <span data-ttu-id="368c2-140">Hej **namn** av hello programmet beskriver programmet tooend användarna.</span><span class="sxs-lookup"><span data-stu-id="368c2-140">hello **Name** of hello application describes your application tooend users.</span></span>
  * <span data-ttu-id="368c2-141">Hej **omdirigerings-Uri** är en kombination av schemat och strängen som använder Azure AD tooreturn token svar.</span><span class="sxs-lookup"><span data-stu-id="368c2-141">hello **Redirect Uri** is a scheme and string combination that Azure AD uses tooreturn token responses.</span></span>  <span data-ttu-id="368c2-142">Ange ett värde som är specifika tooyour program och baserat på informationen för hello tidigare omdirigerings-URI.</span><span class="sxs-lookup"><span data-stu-id="368c2-142">Enter a value that is specific tooyour application and is based on hello previous redirect URI information.</span></span>
6. <span data-ttu-id="368c2-143">När du har slutfört registreringen hello tilldelar Azure AD appen ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="368c2-143">After you've completed hello registration, Azure AD assigns your app a unique application ID.</span></span>  <span data-ttu-id="368c2-144">Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello programfliken.</span><span class="sxs-lookup"><span data-stu-id="368c2-144">You'll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="368c2-145">Från hello **inställningar** väljer **nödvändiga behörigheter** och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="368c2-145">From hello **Settings** page, select **Required Permissions** and then select **Add**.</span></span> <span data-ttu-id="368c2-146">Välj **Microsoft Graph** som hello API, och Lägg sedan till hello **läsa katalogdata** behörighet under **delegerade behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="368c2-146">Select **Microsoft Graph** as hello API, and then add hello **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="368c2-147">Detta konfigurerar ditt program tooquery hello Azure AD Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="368c2-147">This sets up your application tooquery hello Azure AD Graph API for users.</span></span>

## <a name="3-install-and-configure-adal"></a><span data-ttu-id="368c2-148">3. Installera och konfigurera ADAL</span><span class="sxs-lookup"><span data-stu-id="368c2-148">3. Install and configure ADAL</span></span>
<span data-ttu-id="368c2-149">Nu när du har ett program i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="368c2-149">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="368c2-150">Du måste tooprovide för ADAL toocommunicate med Azure AD med viss information om din appregistrering.</span><span class="sxs-lookup"><span data-stu-id="368c2-150">For ADAL toocommunicate with Azure AD, you need tooprovide it with some information about your app registration.</span></span>

1. <span data-ttu-id="368c2-151">Börja genom att lägga till ADAL toohello DirectorySearcher projekt med CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="368c2-151">Begin by adding ADAL toohello DirectorySearcher project by using CocoaPods.</span></span>

    ```
    $ vi Podfile
    ```
2. <span data-ttu-id="368c2-152">Lägg till följande toothis podfile hello:</span><span class="sxs-lookup"><span data-stu-id="368c2-152">Add hello following toothis podfile:</span></span>

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. <span data-ttu-id="368c2-153">Läsa in hello podfile med CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="368c2-153">Now load hello podfile by using CocoaPods.</span></span> <span data-ttu-id="368c2-154">Det här steget skapar en ny XCode-arbetsyta som du läser in.</span><span class="sxs-lookup"><span data-stu-id="368c2-154">This step creates a new XCode workspace that you load.</span></span>

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. <span data-ttu-id="368c2-155">Öppna hello plist-fil i hello snabbstartsprojekt `settings.plist`.</span><span class="sxs-lookup"><span data-stu-id="368c2-155">In hello QuickStart project, open hello plist file `settings.plist`.</span></span>  <span data-ttu-id="368c2-156">Ersätt hello värdena för hello element i hello avsnittet tooreflect hello värden som du angav i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="368c2-156">Replace hello values of hello elements in hello section tooreflect hello values that you entered in hello Azure portal.</span></span> <span data-ttu-id="368c2-157">Din kod refererar till dessa värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="368c2-157">Your code references these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="368c2-158">Hej `tenant` är hello domän för din Azure AD-klient, till exempel contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="368c2-158">hello `tenant` is hello domain of your Azure AD tenant, for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="368c2-159">Hej `clientId` är hello klient-ID för programmet som du kopierade från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="368c2-159">hello `clientId` is hello client ID of your application that you copied from hello portal.</span></span>
  * <span data-ttu-id="368c2-160">Hej `redirectUri` är hello omdirigerings-URL som du har registrerat i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="368c2-160">hello `redirectUri` is hello redirect URL that you registered in hello portal.</span></span>

## <a name="4----use-adal-tooget-tokens-from-azure-ad"></a><span data-ttu-id="368c2-161">4.    Använd ADAL tooget token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="368c2-161">4.    Use ADAL tooget tokens from Azure AD</span></span>
<span data-ttu-id="368c2-162">hello grundläggande principen bakom ADAL är att när din app behöver en åtkomst-token kan bara anropas en completionBlock `+(void) getToken : `, och ADAL hello rest.</span><span class="sxs-lookup"><span data-stu-id="368c2-162">hello basic principle behind ADAL is that whenever your app needs an access token, it simply calls a completionBlock `+(void) getToken : `, and ADAL does hello rest.</span></span>  

1. <span data-ttu-id="368c2-163">I hello `QuickStart` projektet öppnar `GraphAPICaller.m` och leta upp hello `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` kommentar hello övre delen.</span><span class="sxs-lookup"><span data-stu-id="368c2-163">In hello `QuickStart` project, open `GraphAPICaller.m` and locate hello `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` comment near hello top.</span></span>  <span data-ttu-id="368c2-164">Detta är som du skickar ADAL hello koordinater via en CompletionBlock toocommunicate med Azure AD, och anger hur toocache token.</span><span class="sxs-lookup"><span data-stu-id="368c2-164">This is where you pass ADAL hello coordinates through a CompletionBlock, toocommunicate with Azure AD, and tell it how toocache tokens.</span></span>

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

2. <span data-ttu-id="368c2-165">Nu måste vi toouse denna token toosearch för användare i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="368c2-165">Now we need toouse this token toosearch for users in hello graph.</span></span> <span data-ttu-id="368c2-166">Hitta hello `// TODO: implement SearchUsersList` kommentar.</span><span class="sxs-lookup"><span data-stu-id="368c2-166">Find hello `// TODO: implement SearchUsersList` comment.</span></span> <span data-ttu-id="368c2-167">Den här metoden gör en GET-begäran toohello Azure AD Graph API tooquery för användare vars UPN-namnet börjar med hello angivna sökord.</span><span class="sxs-lookup"><span data-stu-id="368c2-167">This method makes a GET request toohello Azure AD Graph API tooquery for users whose UPN begins with hello given search term.</span></span>  <span data-ttu-id="368c2-168">tooquery hello Azure AD Graph API du behöver tooinclude en access_token i hello `Authorization` huvudet i begäran hello.</span><span class="sxs-lookup"><span data-stu-id="368c2-168">tooquery hello Azure AD Graph API, you need tooinclude an access_token in hello `Authorization` header of hello request.</span></span> <span data-ttu-id="368c2-169">Det är där ADAL kommer in.</span><span class="sxs-lookup"><span data-stu-id="368c2-169">This is where ADAL comes in.</span></span>

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


3. <span data-ttu-id="368c2-170">När din app begär en token genom att anropa `getToken(...)`, ADAL försöker tooreturn en token utan att uppge autentiseringsuppgifter hello användare.</span><span class="sxs-lookup"><span data-stu-id="368c2-170">When your app requests a token by calling `getToken(...)`, ADAL attempts tooreturn a token without asking hello user for credentials.</span></span>  <span data-ttu-id="368c2-171">Om ADAL avgör hello användaren måste toosign i tooget en token, visa en dialogruta för inloggning, samla in hello användarens autentiseringsuppgifter och sedan returnera en token efter en lyckad autentisering.</span><span class="sxs-lookup"><span data-stu-id="368c2-171">If ADAL determines that hello user needs toosign in tooget a token, it will display a dialog box for sign-in, collect hello user's credentials, and then return a token after successful authentication.</span></span>  <span data-ttu-id="368c2-172">Om ADAL inte kan tooreturn en token av någon anledning, genererar en `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="368c2-172">If ADAL is not able tooreturn a token for any reason, it throws an `AdalException`.</span></span>

> [!Note] 
> <span data-ttu-id="368c2-173">Hej `AuthenticationResult` objektet innehåller en `tokenCacheStoreItem` objekt som kan vara används toocollect hello information som kan behövas för din app.</span><span class="sxs-lookup"><span data-stu-id="368c2-173">hello `AuthenticationResult` object contains a `tokenCacheStoreItem` object that can be used toocollect hello information that your app may need.</span></span> <span data-ttu-id="368c2-174">I hello Snabbstart, `tokenCacheStoreItem` är används toodetermine om autentisering redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="368c2-174">In hello QuickStart, `tokenCacheStoreItem` is used toodetermine if authentication is already done.</span></span>
>
>

## <a name="5-build-and-run-hello-application"></a><span data-ttu-id="368c2-175">5. Skapa och köra programmet hello</span><span class="sxs-lookup"><span data-stu-id="368c2-175">5. Build and run hello application</span></span>
<span data-ttu-id="368c2-176">Grattis!</span><span class="sxs-lookup"><span data-stu-id="368c2-176">Congratulations!</span></span> <span data-ttu-id="368c2-177">Nu har du en fungerande iOS-program som kan autentisera användare, på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och få grundläggande information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="368c2-177">You now have a working iOS application that can authenticate users, securely call Web APIs by using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="368c2-178">Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare.</span><span class="sxs-lookup"><span data-stu-id="368c2-178">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span>  <span data-ttu-id="368c2-179">Starta appen QuickStart och logga sedan in med något av dessa användare.</span><span class="sxs-lookup"><span data-stu-id="368c2-179">Start your QuickStart app, and then sign in with one of those users.</span></span>  <span data-ttu-id="368c2-180">Sök efter andra användare baserat på deras UPN.</span><span class="sxs-lookup"><span data-stu-id="368c2-180">Search for other users based on their UPN.</span></span>  <span data-ttu-id="368c2-181">Stäng hello appen och sedan starta den igen.</span><span class="sxs-lookup"><span data-stu-id="368c2-181">Close hello app, and then start it again.</span></span>  <span data-ttu-id="368c2-182">Observera att hello användarens session förblir intakt.</span><span class="sxs-lookup"><span data-stu-id="368c2-182">Notice that hello user's session remains intact.</span></span>

<span data-ttu-id="368c2-183">ADAL gör det enkelt tooincorporate alla dessa vanliga identity-funktioner i programmet.</span><span class="sxs-lookup"><span data-stu-id="368c2-183">ADAL makes it easy tooincorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="368c2-184">Det hand tar om alla hello ändrad arbete för dig, t.ex. hantering av cache OAuth protokollstöd presenterar en UI toosign i hello användare och uppdatera token har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="368c2-184">It takes care of all hello dirty work for you, like cache management, OAuth protocol support, presenting hello user with a UI toosign in, and refreshing expired tokens.</span></span>  <span data-ttu-id="368c2-185">Allt du verkligen behöver tooknow är ett enda API-anrop `getToken`.</span><span class="sxs-lookup"><span data-stu-id="368c2-185">All you really need tooknow is a single API call, `getToken`.</span></span>

<span data-ttu-id="368c2-186">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) finns på [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="368c2-186">For reference, hello completed sample (without your configuration values) is provided on [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="368c2-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="368c2-187">Next steps</span></span>
<span data-ttu-id="368c2-188">Du kan nu gå vidare tooadditional scenarier.</span><span class="sxs-lookup"><span data-stu-id="368c2-188">You can now move on tooadditional scenarios.</span></span>  <span data-ttu-id="368c2-189">Du kanske vill tootry:</span><span class="sxs-lookup"><span data-stu-id="368c2-189">You may want tootry:</span></span>

* [<span data-ttu-id="368c2-190">Skydda en Node.JS-webb-API med Azure AD</span><span class="sxs-lookup"><span data-stu-id="368c2-190">Secure a Node.JS Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)
* <span data-ttu-id="368c2-191">Läs [hur tooenable enkel inloggning mellan appar på iOS använder ADAL](active-directory-sso-ios.md)</span><span class="sxs-lookup"><span data-stu-id="368c2-191">Learn [how tooenable cross-app SSO on iOS using ADAL](active-directory-sso-ios.md)</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


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
# <a name="integrate-azure-ad-into-an-ios-app"></a><span data-ttu-id="29980-103">Integrera Azure AD i en iOS-app</span><span class="sxs-lookup"><span data-stu-id="29980-103">Integrate Azure AD into an iOS app</span></span>
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> <span data-ttu-id="29980-104">Testa förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/iOS) som hjälper dig att komma igång med Azure Active Directory på bara några minuter!</span><span class="sxs-lookup"><span data-stu-id="29980-104">Try the preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure Active Directory in just a few minutes!</span></span>  <span data-ttu-id="29980-105">Developer-portalen får du hjälp med att registrera en app och integrera Azure AD i din kod.</span><span class="sxs-lookup"><span data-stu-id="29980-105">The developer portal guides you through the process of registering an app and integrating Azure AD into your code.</span></span>  <span data-ttu-id="29980-106">När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdel som kan acceptera token och utföra valideringen.</span><span class="sxs-lookup"><span data-stu-id="29980-106">When you’re finished, you'll have a simple application that can authenticate users in your tenant and a backend that can accept tokens and perform validation.</span></span> 
> 
> 

<span data-ttu-id="29980-107">Azure Active Directory (AD Azure) tillhandahåller Active Directory Authentication Library eller ADAL för iOS-klienter som behöver åtkomst till skyddade resurser.</span><span class="sxs-lookup"><span data-stu-id="29980-107">Azure Active Directory (Azure AD) provides the Active Directory Authentication Library, or ADAL, for iOS clients that need to access protected resources.</span></span> <span data-ttu-id="29980-108">ADAL förenklar som din app använder för att erhålla åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="29980-108">ADAL simplifies the process that your app uses to obtain access tokens.</span></span> <span data-ttu-id="29980-109">Att demonstrera hur lätt det är i den här artikeln vi skapa ett uppgiftslista för Objective C-program som:</span><span class="sxs-lookup"><span data-stu-id="29980-109">To demonstrate how easy it is, in this article we build an Objective C To-Do List application that:</span></span>

* <span data-ttu-id="29980-110">Hämtar åtkomst till token för att anropa Azure AD Graph-API med hjälp av den [OAuth 2.0-autentiseringsprotokollet](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span><span class="sxs-lookup"><span data-stu-id="29980-110">Gets access tokens for calling the Azure AD Graph API by using the [OAuth 2.0 authentication protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).</span></span>
* <span data-ttu-id="29980-111">Söker en katalog för användare med ett visst alias.</span><span class="sxs-lookup"><span data-stu-id="29980-111">Searches a directory for users with a given alias.</span></span>

<span data-ttu-id="29980-112">För att skapa fullständiga fungerande program måste du:</span><span class="sxs-lookup"><span data-stu-id="29980-112">To build the complete working application, you need to:</span></span>

1. <span data-ttu-id="29980-113">Registrera ditt program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29980-113">Register your application with Azure AD.</span></span>
2. <span data-ttu-id="29980-114">Installera och konfigurera ADAL.</span><span class="sxs-lookup"><span data-stu-id="29980-114">Install and configure ADAL.</span></span>
3. <span data-ttu-id="29980-115">Använd ADAL för att hämta token från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29980-115">Use ADAL to get tokens from Azure AD.</span></span>

<span data-ttu-id="29980-116">Du kommer igång [ladda ned stommen app](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) eller [hämta det slutförda exemplet](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="29980-116">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span> <span data-ttu-id="29980-117">Du måste också en Azure AD-klient som du kan skapa användare och registrera ett program.</span><span class="sxs-lookup"><span data-stu-id="29980-117">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="29980-118">Om du inte redan har en klient [Lär dig hur du skaffa en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="29980-118">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>


> [!TIP]
> <span data-ttu-id="29980-119">Testa förhandsversionen av vår nya [utvecklarportalen](https://identity.microsoft.com/Docs/iOS) som hjälper dig att komma igång med Azure AD i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="29980-119">Try the preview of our new [developer portal](https://identity.microsoft.com/Docs/iOS) that helps you get up and running with Azure AD in just a few minutes.</span></span> <span data-ttu-id="29980-120">Developer-portalen får du hjälp med att registrera en app och integrera Azure AD i din kod.</span><span class="sxs-lookup"><span data-stu-id="29980-120">The developer portal guides you through the process of registering an app and integrating Azure AD into your code.</span></span> <span data-ttu-id="29980-121">När du är klar har du ett enkelt program som kan autentisera användare i din klient och en serverdelsnätverk som kan ta emot token och utföra verifiering.</span><span class="sxs-lookup"><span data-stu-id="29980-121">When you’re finished, you'll have a simple application that can authenticate users in your tenant, and a back end that can accept tokens and perform validation.</span></span> 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a><span data-ttu-id="29980-122">1. Bestämma vilka din omdirigering URI är för iOS</span><span class="sxs-lookup"><span data-stu-id="29980-122">1. Determine what your redirect URI is for iOS</span></span>
<span data-ttu-id="29980-123">Om du vill starta dina program på ett säkert sätt i vissa scenarier för enkel inloggning, måste du skapa en *omdirigerings-URI* i ett visst format.</span><span class="sxs-lookup"><span data-stu-id="29980-123">To securely start your applications in certain SSO scenarios, you must create a *redirect URI* in a particular format.</span></span> <span data-ttu-id="29980-124">En omdirigerings-URI används för att se till att token tillbaka till rätt program som en förfrågan om dem.</span><span class="sxs-lookup"><span data-stu-id="29980-124">A redirect URI is used to ensure that the tokens return to the correct application that asked for them.</span></span>


<span data-ttu-id="29980-125">IOS-format för en omdirigerings-URI: N är:</span><span class="sxs-lookup"><span data-stu-id="29980-125">The iOS format for a redirect URI is:</span></span>

```
<app-scheme>://<bundle-id>
```

* <span data-ttu-id="29980-126">**App-schemat** -detta är registrerad i ditt XCode-projekt.</span><span class="sxs-lookup"><span data-stu-id="29980-126">**app-scheme** - This is registered in your XCode project.</span></span> <span data-ttu-id="29980-127">Det är hur andra program kan ringa dig.</span><span class="sxs-lookup"><span data-stu-id="29980-127">It is how other applications can call you.</span></span> <span data-ttu-id="29980-128">Du hittar detta under Info.plist -> URL typer -> URL-identifierare.</span><span class="sxs-lookup"><span data-stu-id="29980-128">You can find this under Info.plist -> URL types -> URL Identifier.</span></span> <span data-ttu-id="29980-129">Du bör skapa en om du inte redan har en eller flera konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="29980-129">You should create one if you don't already have one or more configured.</span></span>
* <span data-ttu-id="29980-130">**paket-id** -detta är paket-ID hittades under ”identitet” ta bort dina Projektinställningar i XCode.</span><span class="sxs-lookup"><span data-stu-id="29980-130">**bundle-id** - This is the Bundle Identifier found under "identity" un your project settings in XCode.</span></span>

<span data-ttu-id="29980-131">Ett exempel på den här koden för Snabbstart: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span><span class="sxs-lookup"><span data-stu-id="29980-131">An example for this QuickStart code: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***</span></span>

## <a name="2-register-the-directorysearcher-application"></a><span data-ttu-id="29980-132">2. Registrera DirectorySearcher-program</span><span class="sxs-lookup"><span data-stu-id="29980-132">2. Register the DirectorySearcher application</span></span>
<span data-ttu-id="29980-133">Om du vill konfigurera din app att hämta token måste du först registrera det i Azure AD-klienten och bevilja behörighet att komma åt Azure AD Graph-API:</span><span class="sxs-lookup"><span data-stu-id="29980-133">To set up your app to get tokens, you first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API:</span></span>

1. <span data-ttu-id="29980-134">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="29980-134">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="29980-135">Klicka på ditt konto på den översta raden.</span><span class="sxs-lookup"><span data-stu-id="29980-135">On the top bar, click your account.</span></span> <span data-ttu-id="29980-136">Under den **Directory** Välj Active Directory-klient som du vill registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="29980-136">Under the **Directory** list, choose the Active Directory tenant where you want to register your application.</span></span>
3. <span data-ttu-id="29980-137">Klicka på **fler tjänster** i det vänstra navigeringsfönstret och välj sedan **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="29980-137">Click **More Services** in the leftmost navigation pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="29980-138">Klicka på **App registreringar**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="29980-138">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="29980-139">Följ anvisningarna för att skapa en ny **internt klientprogram**.</span><span class="sxs-lookup"><span data-stu-id="29980-139">Follow the prompts to create a new **Native Client Application**.</span></span>
  * <span data-ttu-id="29980-140">Den **namn** beskriver ditt program för slutanvändare av programmet.</span><span class="sxs-lookup"><span data-stu-id="29980-140">The **Name** of the application describes your application to end users.</span></span>
  * <span data-ttu-id="29980-141">Den **omdirigerings-Uri** är en kombination av schemat och strängen som Azure AD som används för att returnera token svar.</span><span class="sxs-lookup"><span data-stu-id="29980-141">The **Redirect Uri** is a scheme and string combination that Azure AD uses to return token responses.</span></span>  <span data-ttu-id="29980-142">Ange ett värde som är specifik för ditt program och är baserad på informationen om föregående omdirigerings-URI.</span><span class="sxs-lookup"><span data-stu-id="29980-142">Enter a value that is specific to your application and is based on the previous redirect URI information.</span></span>
6. <span data-ttu-id="29980-143">När du har slutfört registreringen, tilldelar Azure AD appen ett unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="29980-143">After you've completed the registration, Azure AD assigns your app a unique application ID.</span></span>  <span data-ttu-id="29980-144">Du behöver det här värdet i nästa avsnitt så kopiera den från programfliken.</span><span class="sxs-lookup"><span data-stu-id="29980-144">You'll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="29980-145">Från den **inställningar** väljer **nödvändiga behörigheter** och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="29980-145">From the **Settings** page, select **Required Permissions** and then select **Add**.</span></span> <span data-ttu-id="29980-146">Välj **Microsoft Graph** som API, och Lägg sedan till den **läsa katalogdata** behörighet under **delegerade behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="29980-146">Select **Microsoft Graph** as the API, and then add the **Read Directory Data** permission under **Delegated Permissions**.</span></span>  <span data-ttu-id="29980-147">Detta konfigurerar programmet att frågor till Azure AD Graph API för användare.</span><span class="sxs-lookup"><span data-stu-id="29980-147">This sets up your application to query the Azure AD Graph API for users.</span></span>

## <a name="3-install-and-configure-adal"></a><span data-ttu-id="29980-148">3. Installera och konfigurera ADAL</span><span class="sxs-lookup"><span data-stu-id="29980-148">3. Install and configure ADAL</span></span>
<span data-ttu-id="29980-149">Nu när du har ett program i Azure AD kan du installera ADAL och Skriv koden identitetsrelaterade.</span><span class="sxs-lookup"><span data-stu-id="29980-149">Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.</span></span>  <span data-ttu-id="29980-150">Du måste tillhandahålla information om din appregistrering för ADAL att kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="29980-150">For ADAL to communicate with Azure AD, you need to provide it with some information about your app registration.</span></span>

1. <span data-ttu-id="29980-151">Börja genom att lägga till ADAL DirectorySearcher projektet med CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="29980-151">Begin by adding ADAL to the DirectorySearcher project by using CocoaPods.</span></span>

    ```
    $ vi Podfile
    ```
2. <span data-ttu-id="29980-152">Lägg till följande i Podfile:</span><span class="sxs-lookup"><span data-stu-id="29980-152">Add the following to this podfile:</span></span>

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. <span data-ttu-id="29980-153">Läsa in podfile med CocoaPods.</span><span class="sxs-lookup"><span data-stu-id="29980-153">Now load the podfile by using CocoaPods.</span></span> <span data-ttu-id="29980-154">Det här steget skapar en ny XCode-arbetsyta som du läser in.</span><span class="sxs-lookup"><span data-stu-id="29980-154">This step creates a new XCode workspace that you load.</span></span>

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. <span data-ttu-id="29980-155">Öppna filen plist i snabbstartsprojektet `settings.plist`.</span><span class="sxs-lookup"><span data-stu-id="29980-155">In the QuickStart project, open the plist file `settings.plist`.</span></span>  <span data-ttu-id="29980-156">Ersätt värdena för elementen i avsnittet för att återspegla de värden som du angav i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="29980-156">Replace the values of the elements in the section to reflect the values that you entered in the Azure portal.</span></span> <span data-ttu-id="29980-157">Din kod refererar till dessa värden när den använder ADAL.</span><span class="sxs-lookup"><span data-stu-id="29980-157">Your code references these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="29980-158">Den `tenant` är domänen för din Azure AD-klient, till exempel contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="29980-158">The `tenant` is the domain of your Azure AD tenant, for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="29980-159">Den `clientId` är klient-ID för programmet som du kopierade från portalen.</span><span class="sxs-lookup"><span data-stu-id="29980-159">The `clientId` is the client ID of your application that you copied from the portal.</span></span>
  * <span data-ttu-id="29980-160">Den `redirectUri` är omdirigerings-URL som du har registrerat i portalen.</span><span class="sxs-lookup"><span data-stu-id="29980-160">The `redirectUri` is the redirect URL that you registered in the portal.</span></span>

## <a name="4----use-adal-to-get-tokens-from-azure-ad"></a><span data-ttu-id="29980-161">4.    Använd ADAL för att hämta token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="29980-161">4.    Use ADAL to get tokens from Azure AD</span></span>
<span data-ttu-id="29980-162">Den grundläggande principen bakom ADAL är att när din app behöver en åtkomst-token kan bara anropas en completionBlock `+(void) getToken : `, och ADAL gör resten.</span><span class="sxs-lookup"><span data-stu-id="29980-162">The basic principle behind ADAL is that whenever your app needs an access token, it simply calls a completionBlock `+(void) getToken : `, and ADAL does the rest.</span></span>  

1. <span data-ttu-id="29980-163">I den `QuickStart` projektet öppnar `GraphAPICaller.m` och leta upp den `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` kommentar längst upp.</span><span class="sxs-lookup"><span data-stu-id="29980-163">In the `QuickStart` project, open `GraphAPICaller.m` and locate the `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` comment near the top.</span></span>  <span data-ttu-id="29980-164">Det är där du skickar ADAL koordinaterna via en CompletionBlock att kommunicera med Azure AD och hur det kan cachelagra token.</span><span class="sxs-lookup"><span data-stu-id="29980-164">This is where you pass ADAL the coordinates through a CompletionBlock, to communicate with Azure AD, and tell it how to cache tokens.</span></span>

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

2. <span data-ttu-id="29980-165">Nu ska vi använda denna token för att söka efter användare i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="29980-165">Now we need to use this token to search for users in the graph.</span></span> <span data-ttu-id="29980-166">Hitta de `// TODO: implement SearchUsersList` kommentar.</span><span class="sxs-lookup"><span data-stu-id="29980-166">Find the `// TODO: implement SearchUsersList` comment.</span></span> <span data-ttu-id="29980-167">Den här metoden gör en GET-begäran till Azure AD Graph API frågan för användare vars UPN-namnet börjar med den angivna söktermen.</span><span class="sxs-lookup"><span data-stu-id="29980-167">This method makes a GET request to the Azure AD Graph API to query for users whose UPN begins with the given search term.</span></span>  <span data-ttu-id="29980-168">Om du vill fråga Azure AD Graph API, måste du inkludera ett access_token i den `Authorization` huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="29980-168">To query the Azure AD Graph API, you need to include an access_token in the `Authorization` header of the request.</span></span> <span data-ttu-id="29980-169">Det är där ADAL kommer in.</span><span class="sxs-lookup"><span data-stu-id="29980-169">This is where ADAL comes in.</span></span>

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


3. <span data-ttu-id="29980-170">När din app begär en token genom att anropa `getToken(...)`, ADAL försöker denna att returnera en token utan att fråga användaren om autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="29980-170">When your app requests a token by calling `getToken(...)`, ADAL attempts to return a token without asking the user for credentials.</span></span>  <span data-ttu-id="29980-171">Om ADAL avgör att användaren måste logga in att hämta en token, visa en dialogruta för inloggning, samla in användarens autentiseringsuppgifter och sedan returnera en token efter en lyckad autentisering.</span><span class="sxs-lookup"><span data-stu-id="29980-171">If ADAL determines that the user needs to sign in to get a token, it will display a dialog box for sign-in, collect the user's credentials, and then return a token after successful authentication.</span></span>  <span data-ttu-id="29980-172">Om ADAL inte kan returnera en token av någon anledning, genererar en `AdalException`.</span><span class="sxs-lookup"><span data-stu-id="29980-172">If ADAL is not able to return a token for any reason, it throws an `AdalException`.</span></span>

> [!Note] 
> <span data-ttu-id="29980-173">Den `AuthenticationResult` objektet innehåller en `tokenCacheStoreItem` objekt som kan användas för att samla in information som kan behövas för din app.</span><span class="sxs-lookup"><span data-stu-id="29980-173">The `AuthenticationResult` object contains a `tokenCacheStoreItem` object that can be used to collect the information that your app may need.</span></span> <span data-ttu-id="29980-174">I Snabbstart, `tokenCacheStoreItem` används för att avgöra om autentisering redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="29980-174">In the QuickStart, `tokenCacheStoreItem` is used to determine if authentication is already done.</span></span>
>
>

## <a name="5-build-and-run-the-application"></a><span data-ttu-id="29980-175">5. Skapa och kör appen</span><span class="sxs-lookup"><span data-stu-id="29980-175">5. Build and run the application</span></span>
<span data-ttu-id="29980-176">Grattis!</span><span class="sxs-lookup"><span data-stu-id="29980-176">Congratulations!</span></span> <span data-ttu-id="29980-177">Nu har du en fungerande iOS-program som kan autentisera användare, på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och få grundläggande information om användaren.</span><span class="sxs-lookup"><span data-stu-id="29980-177">You now have a working iOS application that can authenticate users, securely call Web APIs by using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="29980-178">Om du inte redan gjort nu är det dags att fylla i din klient med vissa användare.</span><span class="sxs-lookup"><span data-stu-id="29980-178">If you haven't already, now is the time to populate your tenant with some users.</span></span>  <span data-ttu-id="29980-179">Starta appen QuickStart och logga sedan in med något av dessa användare.</span><span class="sxs-lookup"><span data-stu-id="29980-179">Start your QuickStart app, and then sign in with one of those users.</span></span>  <span data-ttu-id="29980-180">Sök efter andra användare baserat på deras UPN.</span><span class="sxs-lookup"><span data-stu-id="29980-180">Search for other users based on their UPN.</span></span>  <span data-ttu-id="29980-181">Stäng appen och sedan starta den igen.</span><span class="sxs-lookup"><span data-stu-id="29980-181">Close the app, and then start it again.</span></span>  <span data-ttu-id="29980-182">Observera att användarens session förblir intakt.</span><span class="sxs-lookup"><span data-stu-id="29980-182">Notice that the user's session remains intact.</span></span>

<span data-ttu-id="29980-183">ADAL gör det enkelt att införliva alla dessa vanliga identity-funktioner i programmet.</span><span class="sxs-lookup"><span data-stu-id="29980-183">ADAL makes it easy to incorporate all of these common identity features into your application.</span></span>  <span data-ttu-id="29980-184">Det tar hand om ändrad arbetet, t.ex. hantering av cache OAuth protokollstöd presentera användaren med ett användargränssnitt för att logga in, och uppdatera gått token.</span><span class="sxs-lookup"><span data-stu-id="29980-184">It takes care of all the dirty work for you, like cache management, OAuth protocol support, presenting the user with a UI to sign in, and refreshing expired tokens.</span></span>  <span data-ttu-id="29980-185">Allt du behöver veta är ett enda API-anrop `getToken`.</span><span class="sxs-lookup"><span data-stu-id="29980-185">All you really need to know is a single API call, `getToken`.</span></span>

<span data-ttu-id="29980-186">Referens tillhandahålls det slutförda exemplet (utan dina konfigurationsvärden) på [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="29980-186">For reference, the completed sample (without your configuration values) is provided on [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="29980-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29980-187">Next steps</span></span>
<span data-ttu-id="29980-188">Du kan nu gå vidare till fler scenarier.</span><span class="sxs-lookup"><span data-stu-id="29980-188">You can now move on to additional scenarios.</span></span>  <span data-ttu-id="29980-189">Du kanske vill prova:</span><span class="sxs-lookup"><span data-stu-id="29980-189">You may want to try:</span></span>

* [<span data-ttu-id="29980-190">Skydda en Node.JS-webb-API med Azure AD</span><span class="sxs-lookup"><span data-stu-id="29980-190">Secure a Node.JS Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)
* <span data-ttu-id="29980-191">Läs [så att aktivera enkel inloggning mellan appar på iOS använder ADAL](active-directory-sso-ios.md)</span><span class="sxs-lookup"><span data-stu-id="29980-191">Learn [how to enable cross-app SSO on iOS using ADAL](active-directory-sso-ios.md)</span></span>  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]


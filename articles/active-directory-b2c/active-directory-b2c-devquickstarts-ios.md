---
title: "Hämta en token med en iOS-App - Azure AD B2C | Microsoft Docs"
description: "Den här artikeln visar hur du skapar en iOS-app som använder AppAuth med Azure Active Directory B2C hanterar användaridentiteter och autentiserar användare."
services: active-directory-b2c
documentationcenter: ios
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: d818a634-42c2-4cbd-bf73-32fa0c8c69d3
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objectivec
ms.topic: article
ms.date: 03/07/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: ebec5d910b8987dcc8155cd4ead00f87d219941c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a><span data-ttu-id="1592c-103">Azure AD B2C: Logga in med ett iOS-program</span><span class="sxs-lookup"><span data-stu-id="1592c-103">Azure AD B2C: Sign-in using an iOS application</span></span>

<span data-ttu-id="1592c-104">Microsofts identitetsplattform använder öppna standarder som OAuth2 och OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="1592c-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="1592c-105">Med en öppen standard protokollet erbjuder flera developer valet när du väljer ett bibliotek att integrera med våra tjänster.</span><span class="sxs-lookup"><span data-stu-id="1592c-105">Using an open standard protocol offers more developer choice when selecting a library to integrate with our services.</span></span> <span data-ttu-id="1592c-106">Vi har lagt till den här genomgången och andra som den hjälpa utvecklare skriva program som ansluter till Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="1592c-106">We've provided this walkthrough and others like it to aid developers with writing applications that connect to the Microsoft Identity platform.</span></span> <span data-ttu-id="1592c-107">De flesta bibliotek som implementerar [RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kan ansluta till Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="1592c-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) are able to connect to the Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="1592c-108">Microsoft inte tillhandahåller åtgärdar för tredjeparts-bibliotek och inte har gjort en genomgång av dessa bibliotek.</span><span class="sxs-lookup"><span data-stu-id="1592c-108">Microsoft does not provide fixes for third-party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="1592c-109">Det här exemplet använder en tredje parts bibliotek kallas AppAuth har testats för kompatibilitet i grundläggande scenarier med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="1592c-109">This sample is using a third-party library called AppAuth that has been tested for compatibility in basic scenarios with the Azure AD B2C.</span></span> <span data-ttu-id="1592c-110">Problem och funktionsförfrågningar ska dirigeras till biblioteksprojekt öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="1592c-110">Issues and feature requests should be directed to the library's open-source project.</span></span> <span data-ttu-id="1592c-111">Mer information finns i [den här artikeln](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span><span class="sxs-lookup"><span data-stu-id="1592c-111">For more information, see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span></span>
>
>

<span data-ttu-id="1592c-112">Om du har använt OAuth2 eller OpenID Connect eventuellt mycket av det här exempelkonfiguration ingen vits mycket för dig.</span><span class="sxs-lookup"><span data-stu-id="1592c-112">If you're new to OAuth2 or OpenID Connect, much of this sample configuration may not make much sense to you.</span></span> <span data-ttu-id="1592c-113">Vi rekommenderar att du läser en kort [översikt över protokollet som vi har dokumenterat här](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="1592c-113">We recommend you look at a brief [overview of the protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="1592c-114">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="1592c-114">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="1592c-115">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="1592c-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="1592c-116">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="1592c-116">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="1592c-117">Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="1592c-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="1592c-118">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="1592c-118">Create an application</span></span>
<span data-ttu-id="1592c-119">Därefter måste du skapa en app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="1592c-119">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="1592c-120">App-registreringen ger Azure AD den information som krävs för att kommunicera säkert med din app.</span><span class="sxs-lookup"><span data-stu-id="1592c-120">The app registration gives Azure AD information that it needs to communicate securely with your app.</span></span> <span data-ttu-id="1592c-121">Så här skapar du en mobil app [instruktionerna](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="1592c-121">To create a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="1592c-122">Se till att:</span><span class="sxs-lookup"><span data-stu-id="1592c-122">Be sure to:</span></span>

* <span data-ttu-id="1592c-123">Inkludera en **Native client** i programmet.</span><span class="sxs-lookup"><span data-stu-id="1592c-123">Include a **Native client** in the application.</span></span>
* <span data-ttu-id="1592c-124">Kopiera **program-ID:t** som har tilldelats din app.</span><span class="sxs-lookup"><span data-stu-id="1592c-124">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="1592c-125">Du behöver detta GUID senare.</span><span class="sxs-lookup"><span data-stu-id="1592c-125">You need this GUID later.</span></span>
* <span data-ttu-id="1592c-126">Konfigurera en **omdirigerings-URI** med ett anpassat schema (till exempel com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="1592c-126">Set up a **Redirect URI** with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="1592c-127">Du behöver den här URI senare.</span><span class="sxs-lookup"><span data-stu-id="1592c-127">You need this URI later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="1592c-128">Skapa dina principer</span><span class="sxs-lookup"><span data-stu-id="1592c-128">Create your policies</span></span>
<span data-ttu-id="1592c-129">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="1592c-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="1592c-130">Den här appen innehåller en identity-upplevelse: en kombinerad inloggning och registrering.</span><span class="sxs-lookup"><span data-stu-id="1592c-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="1592c-131">Skapa den här principen enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="1592c-131">Create this policy as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="1592c-132">Tänk på följande när du skapar principen:</span><span class="sxs-lookup"><span data-stu-id="1592c-132">When you create the policy, be sure to:</span></span>

* <span data-ttu-id="1592c-133">Under **registreringsattribut**, väljer du attributet **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="1592c-133">Under **Sign-up attributes**, select the attribute **Display name**.</span></span>  <span data-ttu-id="1592c-134">Du kan välja samt andra attribut.</span><span class="sxs-lookup"><span data-stu-id="1592c-134">You can select other attributes as well.</span></span>
* <span data-ttu-id="1592c-135">Under **Programanspråk**, Välj anspråk **visningsnamn** och **användarens objekt-ID**.</span><span class="sxs-lookup"><span data-stu-id="1592c-135">Under **Application claims**, select the claims **Display name** and **User's Object ID**.</span></span> <span data-ttu-id="1592c-136">Du kan välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="1592c-136">You can select other claims as well.</span></span>
* <span data-ttu-id="1592c-137">Kopiera **namnet** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="1592c-137">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="1592c-138">Principens namn med prefixet `b2c_1_` när du sparar principen.</span><span class="sxs-lookup"><span data-stu-id="1592c-138">Your policy name is prefixed with `b2c_1_` when you save the policy.</span></span>  <span data-ttu-id="1592c-139">Principnamnet måste senare.</span><span class="sxs-lookup"><span data-stu-id="1592c-139">You need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="1592c-140">När du har skapat dina principer är det dags att bygga appen.</span><span class="sxs-lookup"><span data-stu-id="1592c-140">After you have created your policies, you're ready to build your app.</span></span>

## <a name="download-the-sample-code"></a><span data-ttu-id="1592c-141">Hämta exempelkoden</span><span class="sxs-lookup"><span data-stu-id="1592c-141">Download the sample code</span></span>
<span data-ttu-id="1592c-142">Vi har angett ett fungerande exempel som använder AppAuth med Azure AD B2C [på GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="1592c-142">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="1592c-143">Du kan hämta koden och kör den.</span><span class="sxs-lookup"><span data-stu-id="1592c-143">You can download the code and run it.</span></span> <span data-ttu-id="1592c-144">Om du vill använda din egen Azure AD B2C-klient, följer du anvisningarna i den [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="1592c-144">To use your own Azure AD B2C tenant, follow the instructions in the [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="1592c-145">Det här exemplet har skapats genom att följa anvisningarna viktigt av den [iOS AppAuth projektet på GitHub](https://github.com/openid/AppAuth-iOS).</span><span class="sxs-lookup"><span data-stu-id="1592c-145">This sample was created by following the README instructions by the [iOS AppAuth project on GitHub](https://github.com/openid/AppAuth-iOS).</span></span> <span data-ttu-id="1592c-146">Mer information om hur exemplet och biblioteket fungerar referera till AppAuth README på GitHub.</span><span class="sxs-lookup"><span data-stu-id="1592c-146">For more details on how the sample and the library work, reference the AppAuth README on GitHub.</span></span>

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a><span data-ttu-id="1592c-147">Ändra din app att använda Azure AD B2C med AppAuth</span><span class="sxs-lookup"><span data-stu-id="1592c-147">Modifying your app to use Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="1592c-148">AppAuth stöder iOS 7 och senare.</span><span class="sxs-lookup"><span data-stu-id="1592c-148">AppAuth supports iOS 7 and above.</span></span>  <span data-ttu-id="1592c-149">Men för att stödja sociala inloggningar på Google SFSafariViewController behövs som kräver iOS 9 eller högre.</span><span class="sxs-lookup"><span data-stu-id="1592c-149">However, to support social logins on Google, SFSafariViewController is needed which requires iOS 9 or higher.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="1592c-150">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="1592c-150">Configuration</span></span>

<span data-ttu-id="1592c-151">Du kan konfigurera kommunikation med Azure AD B2C genom att ange både autentiseringsslutpunkt och tokenslutpunkten URI: er.</span><span class="sxs-lookup"><span data-stu-id="1592c-151">You can configure communication with Azure AD B2C by specifying both the authorization endpoint and token endpoint URIs.</span></span>  <span data-ttu-id="1592c-152">För att generera dessa URI: er, behöver du följande information:</span><span class="sxs-lookup"><span data-stu-id="1592c-152">To generate these URIs, you need the following information:</span></span>
* <span data-ttu-id="1592c-153">Klient-ID (till exempel contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="1592c-153">Tenant ID (for example, contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="1592c-154">Principens namn (till exempel B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="1592c-154">Policy name (for example, B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="1592c-155">Tokenslutpunkten URI kan genereras genom att ersätta innehavaren\_-ID och principen\_namn i följande URL:</span><span class="sxs-lookup"><span data-stu-id="1592c-155">The token endpoint URI can be generated by replacing the Tenant\_ID and the Policy\_Name in the following URL:</span></span>

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

<span data-ttu-id="1592c-156">Autentiseringsslutpunkt URI kan genereras genom att ersätta innehavaren\_-ID och principen\_namn i följande URL:</span><span class="sxs-lookup"><span data-stu-id="1592c-156">The authorization endpoint URI can be generated by replacing the Tenant\_ID and the Policy\_Name in the following URL:</span></span>

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

<span data-ttu-id="1592c-157">Kör följande kod för att skapa AuthorizationServiceConfiguration-objekt:</span><span class="sxs-lookup"><span data-stu-id="1592c-157">Run the following code to create your AuthorizationServiceConfiguration object:</span></span>

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a><span data-ttu-id="1592c-158">Auktorisera</span><span class="sxs-lookup"><span data-stu-id="1592c-158">Authorizing</span></span>

<span data-ttu-id="1592c-159">När du konfigurerar eller hämtar en auktorisering tjänstkonfiguration, kan en begäran om godkännande konstrueras.</span><span class="sxs-lookup"><span data-stu-id="1592c-159">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="1592c-160">För att skapa begäran, behöver du följande information:</span><span class="sxs-lookup"><span data-stu-id="1592c-160">To create the request, you need the following information:</span></span>  
* <span data-ttu-id="1592c-161">Klient-ID (till exempel 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="1592c-161">Client ID (for example, 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="1592c-162">Omdirigerings-URI med ett anpassat schema (till exempel com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span><span class="sxs-lookup"><span data-stu-id="1592c-162">Redirect URI with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span></span>

<span data-ttu-id="1592c-163">Båda objekten ska har sparats när du var [registrera din app](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="1592c-163">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

```objc
OIDAuthorizationRequest *request = 
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow = 
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

<span data-ttu-id="1592c-164">Om du vill konfigurera ditt program för att hantera omdirigering till URI: N med det anpassade schemat måste du uppdatera listan över URL: en scheman i din Info.pList:</span><span class="sxs-lookup"><span data-stu-id="1592c-164">To set up your application to handle the redirect to the URI with the custom scheme, you need to update the list of 'URL Schemes' in your Info.pList:</span></span>
* <span data-ttu-id="1592c-165">Öppna filen Info.pList.</span><span class="sxs-lookup"><span data-stu-id="1592c-165">Open Info.pList.</span></span>
* <span data-ttu-id="1592c-166">Hovra över en rad som 'Paket OS typen Code' och klicka på den \+ symbolen.</span><span class="sxs-lookup"><span data-stu-id="1592c-166">Hover over a row like 'Bundle OS Type Code' and click the \+ symbol.</span></span>
* <span data-ttu-id="1592c-167">Byt namn på den nya raden URL-typer.</span><span class="sxs-lookup"><span data-stu-id="1592c-167">Rename the new row 'URL types'.</span></span>
* <span data-ttu-id="1592c-168">Klicka på pilen till vänster om URL-typer att öppna trädet.</span><span class="sxs-lookup"><span data-stu-id="1592c-168">Click the arrow to the left of 'URL types' to open the tree.</span></span>
* <span data-ttu-id="1592c-169">Klicka på pilen till vänster om ' objektet 0' om du vill öppna i trädet.</span><span class="sxs-lookup"><span data-stu-id="1592c-169">Click the arrow to the left of 'Item 0' to open the tree.</span></span>
* <span data-ttu-id="1592c-170">Byt namn på första elementet under objektet 0 till URL-scheman.</span><span class="sxs-lookup"><span data-stu-id="1592c-170">Rename first item underneath Item 0 to 'URL Schemes'.</span></span>
* <span data-ttu-id="1592c-171">Klicka på pilen till vänster om URL: en scheman öppna trädet.</span><span class="sxs-lookup"><span data-stu-id="1592c-171">Click the arrow to the left of 'URL Schemes' to open the tree.</span></span>
* <span data-ttu-id="1592c-172">I kolumnen 'Value' är ett tomt fält till vänster om 'Objektet 0' under URL-scheman.</span><span class="sxs-lookup"><span data-stu-id="1592c-172">In the 'Value' column, there is a blank field to the left of 'Item 0' underneath 'URL Schemes'.</span></span>  <span data-ttu-id="1592c-173">Ange värdet till programmets unika systemet.</span><span class="sxs-lookup"><span data-stu-id="1592c-173">Set the value to your application's unique scheme.</span></span>  <span data-ttu-id="1592c-174">Värdet måste matcha det schema som används i RedirectUrl anges när du skapar OIDAuthorizationRequest-objekt.</span><span class="sxs-lookup"><span data-stu-id="1592c-174">The value must match the scheme used in redirectURL when creating the OIDAuthorizationRequest object.</span></span>  <span data-ttu-id="1592c-175">I vårt exempel använde vi schemat 'com.onmicrosoft.fabrikamb2c.exampleapp'.</span><span class="sxs-lookup"><span data-stu-id="1592c-175">In our sample, we used the scheme 'com.onmicrosoft.fabrikamb2c.exampleapp'.</span></span>

<span data-ttu-id="1592c-176">Referera till den [AppAuth guide](https://openid.github.io/AppAuth-iOS/) om hur du Slutför resten av processen.</span><span class="sxs-lookup"><span data-stu-id="1592c-176">Refer to the [AppAuth guide](https://openid.github.io/AppAuth-iOS/) on how to complete the rest of the process.</span></span> <span data-ttu-id="1592c-177">Om du behöver att snabbt komma igång med en fungerande app kan ta en titt [exemplet](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="1592c-177">If you need to quickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="1592c-178">Följ stegen i den [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) att ange din egen Azure AD B2C-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1592c-178">Follow the steps in the [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) to enter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="1592c-179">Vi är alltid öppna för feedback och förslag!</span><span class="sxs-lookup"><span data-stu-id="1592c-179">We are always open to feedback and suggestions!</span></span> <span data-ttu-id="1592c-180">Om du har problem med det här avsnittet eller rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="1592c-180">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at the bottom of the page.</span></span> <span data-ttu-id="1592c-181">För funktionsbegäranden, lägga till dem i [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="1592c-181">For feature requests, add them to [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>

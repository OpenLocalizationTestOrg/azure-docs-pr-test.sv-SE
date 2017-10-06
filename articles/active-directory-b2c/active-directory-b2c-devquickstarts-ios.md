---
title: "Hämta en token med en iOS-App - Azure AD B2C | Microsoft Docs"
description: "Den här artikeln visar hur toocreate en iOS-app som använder AppAuth med Azure Active Directory B2C toomanage användaridentiteter och autentiserar användare."
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
ms.openlocfilehash: e7cbe2de6e9ae3d45448cdd36292c457a0ef4887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a><span data-ttu-id="4db27-103">Azure AD B2C: Logga in med ett iOS-program</span><span class="sxs-lookup"><span data-stu-id="4db27-103">Azure AD B2C: Sign-in using an iOS application</span></span>

<span data-ttu-id="4db27-104">hello Microsoft identity-plattformen använder öppna standarder, till exempel OAuth2 och OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="4db27-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="4db27-105">Med en öppen standard protokollet erbjuder flera developer valet när du väljer ett bibliotek toointegrate med våra tjänster.</span><span class="sxs-lookup"><span data-stu-id="4db27-105">Using an open standard protocol offers more developer choice when selecting a library toointegrate with our services.</span></span> <span data-ttu-id="4db27-106">Vi har samlat i den här genomgången och andra som den tooaid utvecklare skriva program som ansluter toohello Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="4db27-106">We've provided this walkthrough and others like it tooaid developers with writing applications that connect toohello Microsoft Identity platform.</span></span> <span data-ttu-id="4db27-107">De flesta bibliotek som implementerar [hello RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) är kan tooconnect toohello Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="4db27-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) are able tooconnect toohello Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="4db27-108">Microsoft inte tillhandahåller åtgärdar för tredjeparts-bibliotek och inte har gjort en genomgång av dessa bibliotek.</span><span class="sxs-lookup"><span data-stu-id="4db27-108">Microsoft does not provide fixes for third-party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="4db27-109">Det här exemplet använder en tredje parts bibliotek kallas AppAuth har testats för kompatibilitet i grundläggande scenarier med hello Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="4db27-109">This sample is using a third-party library called AppAuth that has been tested for compatibility in basic scenarios with hello Azure AD B2C.</span></span> <span data-ttu-id="4db27-110">Problem och funktionsförfrågningar ska vara riktad toohello biblioteket öppen källkod projektet.</span><span class="sxs-lookup"><span data-stu-id="4db27-110">Issues and feature requests should be directed toohello library's open-source project.</span></span> <span data-ttu-id="4db27-111">Mer information finns i [den här artikeln](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span><span class="sxs-lookup"><span data-stu-id="4db27-111">For more information, see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).</span></span>
>
>

<span data-ttu-id="4db27-112">Om du är ny tooOAuth2 eller OpenID Connect är mycket av det här exemplet konfigurationen inte mycket bra tooyou.</span><span class="sxs-lookup"><span data-stu-id="4db27-112">If you're new tooOAuth2 or OpenID Connect, much of this sample configuration may not make much sense tooyou.</span></span> <span data-ttu-id="4db27-113">Vi rekommenderar att du tittar på en kort [översikt över hello-protokollet som vi dokumenteras här](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="4db27-113">We recommend you look at a brief [overview of hello protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="4db27-114">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="4db27-114">Get an Azure AD B2C directory</span></span>
<span data-ttu-id="4db27-115">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="4db27-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="4db27-116">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="4db27-116">A directory is a container for all your users, apps, groups, and more.</span></span> <span data-ttu-id="4db27-117">Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="4db27-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="4db27-118">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="4db27-118">Create an application</span></span>
<span data-ttu-id="4db27-119">Sedan måste toocreate en app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="4db27-119">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="4db27-120">Hej appregistrering ger Azure AD den information som behövs toocommunicate säkert med din app.</span><span class="sxs-lookup"><span data-stu-id="4db27-120">hello app registration gives Azure AD information that it needs toocommunicate securely with your app.</span></span> <span data-ttu-id="4db27-121">Följ toocreate en mobil app [instruktionerna](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="4db27-121">toocreate a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="4db27-122">Se till att:</span><span class="sxs-lookup"><span data-stu-id="4db27-122">Be sure to:</span></span>

* <span data-ttu-id="4db27-123">Inkludera en **Native client** i hello program.</span><span class="sxs-lookup"><span data-stu-id="4db27-123">Include a **Native client** in hello application.</span></span>
* <span data-ttu-id="4db27-124">Kopiera hello **program-ID** som är tilldelade tooyour app.</span><span class="sxs-lookup"><span data-stu-id="4db27-124">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="4db27-125">Du behöver detta GUID senare.</span><span class="sxs-lookup"><span data-stu-id="4db27-125">You need this GUID later.</span></span>
* <span data-ttu-id="4db27-126">Konfigurera en **omdirigerings-URI** med ett anpassat schema (till exempel com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="4db27-126">Set up a **Redirect URI** with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="4db27-127">Du behöver den här URI senare.</span><span class="sxs-lookup"><span data-stu-id="4db27-127">You need this URI later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="4db27-128">Skapa dina principer</span><span class="sxs-lookup"><span data-stu-id="4db27-128">Create your policies</span></span>
<span data-ttu-id="4db27-129">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="4db27-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="4db27-130">Den här appen innehåller en identity-upplevelse: en kombinerad inloggning och registrering.</span><span class="sxs-lookup"><span data-stu-id="4db27-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="4db27-131">Skapa den här principen enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="4db27-131">Create this policy as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="4db27-132">Tänk på att när du skapar hello princip:</span><span class="sxs-lookup"><span data-stu-id="4db27-132">When you create hello policy, be sure to:</span></span>

* <span data-ttu-id="4db27-133">Under **registreringsattribut**väljer hello attributet **visningsnamn**.</span><span class="sxs-lookup"><span data-stu-id="4db27-133">Under **Sign-up attributes**, select hello attribute **Display name**.</span></span>  <span data-ttu-id="4db27-134">Du kan välja samt andra attribut.</span><span class="sxs-lookup"><span data-stu-id="4db27-134">You can select other attributes as well.</span></span>
* <span data-ttu-id="4db27-135">Under **Programanspråk**, välja hello anspråk **visningsnamn** och **användarens objekt-ID**.</span><span class="sxs-lookup"><span data-stu-id="4db27-135">Under **Application claims**, select hello claims **Display name** and **User's Object ID**.</span></span> <span data-ttu-id="4db27-136">Du kan välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="4db27-136">You can select other claims as well.</span></span>
* <span data-ttu-id="4db27-137">Kopiera hello **namn** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="4db27-137">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="4db27-138">Principens namn med prefixet `b2c_1_` när du sparar hello princip.</span><span class="sxs-lookup"><span data-stu-id="4db27-138">Your policy name is prefixed with `b2c_1_` when you save hello policy.</span></span>  <span data-ttu-id="4db27-139">Du måste hello principnamn senare.</span><span class="sxs-lookup"><span data-stu-id="4db27-139">You need hello policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="4db27-140">När du har skapat dina principer kan du är klar toobuild din app.</span><span class="sxs-lookup"><span data-stu-id="4db27-140">After you have created your policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-sample-code"></a><span data-ttu-id="4db27-141">Hämta hello exempelkod</span><span class="sxs-lookup"><span data-stu-id="4db27-141">Download hello sample code</span></span>
<span data-ttu-id="4db27-142">Vi har angett ett fungerande exempel som använder AppAuth med Azure AD B2C [på GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="4db27-142">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="4db27-143">Du kan hämta hello koden och kör den.</span><span class="sxs-lookup"><span data-stu-id="4db27-143">You can download hello code and run it.</span></span> <span data-ttu-id="4db27-144">toouse din egen Azure AD B2C-klient, följer du anvisningarna hello i hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="4db27-144">toouse your own Azure AD B2C tenant, follow hello instructions in hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="4db27-145">Det här exemplet har skapats genom att följa anvisningarna för hello viktigt av hello [iOS AppAuth projektet på GitHub](https://github.com/openid/AppAuth-iOS).</span><span class="sxs-lookup"><span data-stu-id="4db27-145">This sample was created by following hello README instructions by hello [iOS AppAuth project on GitHub](https://github.com/openid/AppAuth-iOS).</span></span> <span data-ttu-id="4db27-146">Mer information om hur hello exempel och hello biblioteket fungerar hello referens AppAuth README på GitHub.</span><span class="sxs-lookup"><span data-stu-id="4db27-146">For more details on how hello sample and hello library work, reference hello AppAuth README on GitHub.</span></span>

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a><span data-ttu-id="4db27-147">Ändra din app toouse Azure AD B2C med AppAuth</span><span class="sxs-lookup"><span data-stu-id="4db27-147">Modifying your app toouse Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="4db27-148">AppAuth stöder iOS 7 och senare.</span><span class="sxs-lookup"><span data-stu-id="4db27-148">AppAuth supports iOS 7 and above.</span></span>  <span data-ttu-id="4db27-149">Dock toosupport sociala inloggningar på Google, SFSafariViewController behövs som kräver iOS 9 eller högre.</span><span class="sxs-lookup"><span data-stu-id="4db27-149">However, toosupport social logins on Google, SFSafariViewController is needed which requires iOS 9 or higher.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="4db27-150">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="4db27-150">Configuration</span></span>

<span data-ttu-id="4db27-151">Du kan konfigurera kommunikation med Azure AD B2C genom att ange både hello autentiseringsslutpunkt och tokenslutpunkten URI: er.</span><span class="sxs-lookup"><span data-stu-id="4db27-151">You can configure communication with Azure AD B2C by specifying both hello authorization endpoint and token endpoint URIs.</span></span>  <span data-ttu-id="4db27-152">toogenerate dessa URI: er måste hello följande information:</span><span class="sxs-lookup"><span data-stu-id="4db27-152">toogenerate these URIs, you need hello following information:</span></span>
* <span data-ttu-id="4db27-153">Klient-ID (till exempel contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="4db27-153">Tenant ID (for example, contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="4db27-154">Principens namn (till exempel B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="4db27-154">Policy name (for example, B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="4db27-155">hello-token för slutpunkt URI kan genereras genom att ersätta hello klient\_-ID och hello princip\_namn i hello följande URL:</span><span class="sxs-lookup"><span data-stu-id="4db27-155">hello token endpoint URI can be generated by replacing hello Tenant\_ID and hello Policy\_Name in hello following URL:</span></span>

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

<span data-ttu-id="4db27-156">Hej autentiseringsslutpunkt URI kan genereras genom att ersätta hello klient\_-ID och hello princip\_namn i hello följande URL:</span><span class="sxs-lookup"><span data-stu-id="4db27-156">hello authorization endpoint URI can be generated by replacing hello Tenant\_ID and hello Policy\_Name in hello following URL:</span></span>

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

<span data-ttu-id="4db27-157">Kör hello följande kod toocreate AuthorizationServiceConfiguration-objekt:</span><span class="sxs-lookup"><span data-stu-id="4db27-157">Run hello following code toocreate your AuthorizationServiceConfiguration object:</span></span>

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready tooperform hello auth request...
```

### <a name="authorizing"></a><span data-ttu-id="4db27-158">Auktorisera</span><span class="sxs-lookup"><span data-stu-id="4db27-158">Authorizing</span></span>

<span data-ttu-id="4db27-159">När du konfigurerar eller hämtar en auktorisering tjänstkonfiguration, kan en begäran om godkännande konstrueras.</span><span class="sxs-lookup"><span data-stu-id="4db27-159">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="4db27-160">toocreate hello begär du behöver hello följande information:</span><span class="sxs-lookup"><span data-stu-id="4db27-160">toocreate hello request, you need hello following information:</span></span>  
* <span data-ttu-id="4db27-161">Klient-ID (till exempel 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="4db27-161">Client ID (for example, 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="4db27-162">Omdirigerings-URI med ett anpassat schema (till exempel com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span><span class="sxs-lookup"><span data-stu-id="4db27-162">Redirect URI with a custom scheme (for example, com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)</span></span>

<span data-ttu-id="4db27-163">Båda objekten ska har sparats när du var [registrera din app](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="4db27-163">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

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

<span data-ttu-id="4db27-164">tooset upp ditt program toohandle hello omdirigering toohello URI med hello anpassat schema, behöver du tooupdate hello lista över URL-scheman i din Info.pList:</span><span class="sxs-lookup"><span data-stu-id="4db27-164">tooset up your application toohandle hello redirect toohello URI with hello custom scheme, you need tooupdate hello list of 'URL Schemes' in your Info.pList:</span></span>
* <span data-ttu-id="4db27-165">Öppna filen Info.pList.</span><span class="sxs-lookup"><span data-stu-id="4db27-165">Open Info.pList.</span></span>
* <span data-ttu-id="4db27-166">Hovra över en rad som 'Paket OS typen Code' och klicka på hello \+ symbolen.</span><span class="sxs-lookup"><span data-stu-id="4db27-166">Hover over a row like 'Bundle OS Type Code' and click hello \+ symbol.</span></span>
* <span data-ttu-id="4db27-167">Byt namn på hello nya 'URL radtyper'.</span><span class="sxs-lookup"><span data-stu-id="4db27-167">Rename hello new row 'URL types'.</span></span>
* <span data-ttu-id="4db27-168">Klicka på hello pilen toohello vänster i URL-typer tooopen hello träd.</span><span class="sxs-lookup"><span data-stu-id="4db27-168">Click hello arrow toohello left of 'URL types' tooopen hello tree.</span></span>
* <span data-ttu-id="4db27-169">Klicka på hello pilen toohello vänster 'Objektet 0' tooopen hello träd.</span><span class="sxs-lookup"><span data-stu-id="4db27-169">Click hello arrow toohello left of 'Item 0' tooopen hello tree.</span></span>
* <span data-ttu-id="4db27-170">Byt namn på första elementet under objektet 0 too'URL scheman.</span><span class="sxs-lookup"><span data-stu-id="4db27-170">Rename first item underneath Item 0 too'URL Schemes'.</span></span>
* <span data-ttu-id="4db27-171">Klicka på hello pilen toohello vänster i URL-scheman tooopen hello träd.</span><span class="sxs-lookup"><span data-stu-id="4db27-171">Click hello arrow toohello left of 'URL Schemes' tooopen hello tree.</span></span>
* <span data-ttu-id="4db27-172">I kolumnen 'Value' hello är ett tomt fält toohello till vänster i-objektet 0' under URL-scheman.</span><span class="sxs-lookup"><span data-stu-id="4db27-172">In hello 'Value' column, there is a blank field toohello left of 'Item 0' underneath 'URL Schemes'.</span></span>  <span data-ttu-id="4db27-173">Ange hello värdet tooyour programmets unika schemat.</span><span class="sxs-lookup"><span data-stu-id="4db27-173">Set hello value tooyour application's unique scheme.</span></span>  <span data-ttu-id="4db27-174">hello-värdet måste matcha hello-schema som används i RedirectUrl anges när du skapar hello OIDAuthorizationRequest objekt.</span><span class="sxs-lookup"><span data-stu-id="4db27-174">hello value must match hello scheme used in redirectURL when creating hello OIDAuthorizationRequest object.</span></span>  <span data-ttu-id="4db27-175">I vårt exempel använde vi hello schemat 'com.onmicrosoft.fabrikamb2c.exampleapp'.</span><span class="sxs-lookup"><span data-stu-id="4db27-175">In our sample, we used hello scheme 'com.onmicrosoft.fabrikamb2c.exampleapp'.</span></span>

<span data-ttu-id="4db27-176">Se toohello [AppAuth guide](https://openid.github.io/AppAuth-iOS/) på hur toocomplete hello resten av hello-processen.</span><span class="sxs-lookup"><span data-stu-id="4db27-176">Refer toohello [AppAuth guide](https://openid.github.io/AppAuth-iOS/) on how toocomplete hello rest of hello process.</span></span> <span data-ttu-id="4db27-177">Om du behöver tooquickly komma igång med en fungerande app, kolla [exemplet](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="4db27-177">If you need tooquickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c).</span></span> <span data-ttu-id="4db27-178">Åtgärderna i hello hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) tooenter Azure AD B2C konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="4db27-178">Follow hello steps in hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) tooenter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="4db27-179">Vi är alltid öppna toofeedback och förslag!</span><span class="sxs-lookup"><span data-stu-id="4db27-179">We are always open toofeedback and suggestions!</span></span> <span data-ttu-id="4db27-180">Om du har problem med det här avsnittet eller rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="4db27-180">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span> <span data-ttu-id="4db27-181">För funktionsbegäranden, lägga till dem för[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="4db27-181">For feature requests, add them too[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>

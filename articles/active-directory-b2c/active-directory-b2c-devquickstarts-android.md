---
title: "Azure Active Directory B2C: Hämta en token som använder en Android-App | Microsoft Docs"
description: "Den här artikeln visar hur toocreate en Android-app som använder AppAuth med Azure Active Directory B2C toomanage användaridentiteter och autentiserar användare."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: d00947c3-dcaa-4cb3-8c2e-d94e0746d8b2
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 03/06/2017
ms.author: parakhj
ms.openlocfilehash: 0236398673115a34951f035cb1e73e89417abf86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a><span data-ttu-id="1fe6e-103">Azure AD B2C: Logga in med ett Android-program</span><span class="sxs-lookup"><span data-stu-id="1fe6e-103">Azure AD B2C: Sign-in using an Android application</span></span>

<span data-ttu-id="1fe6e-104">hello Microsoft identity-plattformen använder öppna standarder, till exempel OAuth2 och OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="1fe6e-105">Detta gör att utvecklare tooleverage något bibliotek de önskar toointegrate med våra tjänster.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-105">This allows developers tooleverage any library they wish toointegrate with our services.</span></span> <span data-ttu-id="1fe6e-106">tooaid utvecklare i vår plattform med andra bibliotek, vi har skrivit några genomgång som den här en toodemonstrate hur tooconfigure 3 part bibliotek tooconnect toohello Microsoft identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-106">tooaid developers in using our platform with other libraries, we've written a few walkthroughs like this one toodemonstrate how tooconfigure 3rd party libraries tooconnect toohello Microsoft identity platform.</span></span> <span data-ttu-id="1fe6e-107">De flesta bibliotek som implementerar [hello RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kommer att kunna tooconnect toohello Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) will be able tooconnect toohello Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="1fe6e-108">Microsoft tillhandahåller inte korrigeringar för 3 part bibliotek och inte har gjort en genomgång av dessa bibliotek.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-108">Microsoft does not provide fixes for 3rd party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="1fe6e-109">Det här exemplet använder ett 3 part bibliotek kallas AppAuth har testats för kompatibilitet i grundläggande scenarier med hello Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-109">This sample is using a 3rd party library called AppAuth that has been tested for compatibility in basic scenarios with hello Azure AD B2C.</span></span> <span data-ttu-id="1fe6e-110">Problem och funktionsförfrågningar ska vara riktad toohello biblioteket öppen källkod projektet.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-110">Issues and feature requests should be directed toohello library's open-source project.</span></span> <span data-ttu-id="1fe6e-111">Se [i den här artikeln](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) för mer information.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-111">Please see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) for more information.</span></span>  
>
>

<span data-ttu-id="1fe6e-112">Om du är ny tooOAuth2 eller OpenID Connect är mycket av det här exemplet konfigurationen inte mycket bra tooyou.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-112">If you're new tooOAuth2 or OpenID Connect much of this sample configuration may not make much sense tooyou.</span></span> <span data-ttu-id="1fe6e-113">Vi rekommenderar att du tittar på en kort [översikt över hello-protokollet som vi dokumenteras här](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-113">We recommend you look at a brief [overview of hello protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="1fe6e-114">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="1fe6e-114">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="1fe6e-115">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="1fe6e-116">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-116">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="1fe6e-117">Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="1fe6e-118">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="1fe6e-118">Create an application</span></span>

<span data-ttu-id="1fe6e-119">Sedan måste toocreate en app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-119">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="1fe6e-120">Det ger Azure AD den information som behövs toocommunicate säkert med din app.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-120">This gives Azure AD information that it needs toocommunicate securely with your app.</span></span> <span data-ttu-id="1fe6e-121">Följ toocreate en mobil app [instruktionerna](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-121">toocreate a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="1fe6e-122">Se till att:</span><span class="sxs-lookup"><span data-stu-id="1fe6e-122">Be sure to:</span></span>

* <span data-ttu-id="1fe6e-123">Inkludera en **Native Client** i hello program.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-123">Include a **Native Client** in hello application.</span></span>
* <span data-ttu-id="1fe6e-124">Kopiera hello **program-ID** som är tilldelade tooyour app.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-124">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="1fe6e-125">Du behöver det senare.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-125">You will need this later.</span></span>
* <span data-ttu-id="1fe6e-126">Konfigurera en intern klient **omdirigerings-URI** (t.ex. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-126">Set up a native client **Redirect URI** (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="1fe6e-127">Du behöver även detta senare.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-127">You will also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="1fe6e-128">Skapa principer</span><span class="sxs-lookup"><span data-stu-id="1fe6e-128">Create your policies</span></span>

<span data-ttu-id="1fe6e-129">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="1fe6e-130">Den här appen innehåller en identity-upplevelse: en kombinerad inloggning och registrering.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="1fe6e-131">Du behöver toocreate principen, som beskrivs i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-131">You need toocreate this policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="1fe6e-132">Tänk på att när du skapar hello princip:</span><span class="sxs-lookup"><span data-stu-id="1fe6e-132">When you create hello policy, be sure to:</span></span>

* <span data-ttu-id="1fe6e-133">Välj hello **visningsnamn** som ett anmälan attribut i principen.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-133">Choose hello **Display name** as a sign-up attribute in your policy.</span></span>
* <span data-ttu-id="1fe6e-134">Välj hello **visningsnamn** och **objekt-ID** programanspråken i varje princip.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-134">Choose hello **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="1fe6e-135">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-135">You can choose other claims as well.</span></span>
* <span data-ttu-id="1fe6e-136">Kopiera hello **namn** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-136">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="1fe6e-137">Det bör ha hello prefixet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-137">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="1fe6e-138">Du behöver hello principnamn senare.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-138">You'll need hello policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="1fe6e-139">När du har skapat dina principer kan du är klar toobuild din app.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-139">After you have created your policies, you're ready toobuild your app.</span></span>

## <a name="download-hello-sample-code"></a><span data-ttu-id="1fe6e-140">Hämta hello exempelkod</span><span class="sxs-lookup"><span data-stu-id="1fe6e-140">Download hello sample code</span></span>

<span data-ttu-id="1fe6e-141">Vi har angett ett fungerande exempel som använder AppAuth med Azure AD B2C [på GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-141">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="1fe6e-142">Du kan hämta hello koden och kör den.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-142">You can download hello code and run it.</span></span> <span data-ttu-id="1fe6e-143">Du kan snabbt komma igång med din egen app med Azure AD B2C konfigurationen genom att följa instruktionerna hello i hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-143">You can quickly get started with your own app using your own Azure AD B2C configuration by following hello instructions in hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="1fe6e-144">hello prov är en ändring av hello-exemplet som anges av [AppAuth](https://openid.github.io/AppAuth-Android/).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-144">hello sample is a modification of hello sample provided by [AppAuth](https://openid.github.io/AppAuth-Android/).</span></span> <span data-ttu-id="1fe6e-145">Besök deras sidan toolearn mer om AppAuth och dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-145">Please visit their page toolearn more about AppAuth and its features.</span></span>

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a><span data-ttu-id="1fe6e-146">Ändra din app toouse Azure AD B2C med AppAuth</span><span class="sxs-lookup"><span data-stu-id="1fe6e-146">Modifying your app toouse Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="1fe6e-147">AppAuth stöder Android API 16 (Jellybean) och senare.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-147">AppAuth supports Android API 16 (Jellybean) and above.</span></span> <span data-ttu-id="1fe6e-148">Vi rekommenderar att du använder API-23 och högre.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-148">We recommend using API 23 and above.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="1fe6e-149">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="1fe6e-149">Configuration</span></span>

<span data-ttu-id="1fe6e-150">Du kan konfigurera kommunikation med Azure AD B2C genom att antingen ange hello identifiera URI eller genom att ange både hello autentiseringsslutpunkt och tokenslutpunkten URI: er.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-150">You can configure communication with Azure AD B2C by either specifying hello discovery URI or by specifying both hello authorization endpoint and token endpoint URIs.</span></span> <span data-ttu-id="1fe6e-151">I båda fallen måste hello följande information:</span><span class="sxs-lookup"><span data-stu-id="1fe6e-151">In either case, you will need hello following information:</span></span>

* <span data-ttu-id="1fe6e-152">Klient-ID (t.ex. contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="1fe6e-152">Tenant ID (e.g. contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="1fe6e-153">Principens namn (t.ex. B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="1fe6e-153">Policy name (e.g. B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="1fe6e-154">Om du väljer tooautomatically identifiera hello auktoriserings- och token-slutpunkt för URI: er, måste toofetch information från hello identifiering URI.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-154">If you choose tooautomatically discover hello authorization and token endpoint URIs, you will need toofetch information from hello discovery URI.</span></span> <span data-ttu-id="1fe6e-155">hello identifiering URI kan genereras genom att ersätta hello klient\_-ID och hello princip\_namn i hello följande URL:</span><span class="sxs-lookup"><span data-stu-id="1fe6e-155">hello discovery URI can be generated by replacing hello Tenant\_ID and hello Policy\_Name in hello following URL:</span></span>

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

<span data-ttu-id="1fe6e-156">Du kan hämta hello auktoriserings- och tokenslutpunkten URI: er och skapa ett AuthorizationServiceConfiguration objekt genom att köra hello följande:</span><span class="sxs-lookup"><span data-stu-id="1fe6e-156">You can then acquire hello authorization and token endpoint URIs and create an AuthorizationServiceConfiguration object by running hello following:</span></span>

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed tooretrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed tooauthorization...
        }
      }
  });
```

<span data-ttu-id="1fe6e-157">Istället för att använda identifiering tooobtain hello auktoriserings- och tokenslutpunkten URI: er, du kan också ange dem explicit genom att ersätta hello klient\_-ID och hello princip\_namn i hello URL nedan:</span><span class="sxs-lookup"><span data-stu-id="1fe6e-157">Instead of using discovery tooobtain hello authorization and token endpoint URIs, you can also specify them explicitly by replacing hello Tenant\_ID and hello Policy\_Name in hello URL's below:</span></span>

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

<span data-ttu-id="1fe6e-158">Kör hello följande kod toocreate AuthorizationServiceConfiguration-objekt:</span><span class="sxs-lookup"><span data-stu-id="1fe6e-158">Run hello following code toocreate your AuthorizationServiceConfiguration object:</span></span>

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform hello auth request...
```

### <a name="authorizing"></a><span data-ttu-id="1fe6e-159">Auktorisera</span><span class="sxs-lookup"><span data-stu-id="1fe6e-159">Authorizing</span></span>

<span data-ttu-id="1fe6e-160">När du konfigurerar eller hämtar en auktorisering tjänstkonfiguration, kan en begäran om godkännande konstrueras.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-160">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="1fe6e-161">toocreate hello begäran måste hello följande information:</span><span class="sxs-lookup"><span data-stu-id="1fe6e-161">toocreate hello request, you will need hello following information:</span></span>

* <span data-ttu-id="1fe6e-162">Klient-ID (t.ex. 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="1fe6e-162">Client ID (e.g. 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="1fe6e-163">Omdirigerings-URI med ett anpassat schema (t.ex. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span><span class="sxs-lookup"><span data-stu-id="1fe6e-163">Redirect URI with a custom scheme (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span></span>

<span data-ttu-id="1fe6e-164">Båda objekten ska har sparats när du var [registrera din app](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-164">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

<span data-ttu-id="1fe6e-165">Se toohello [AppAuth guide](https://openid.github.io/AppAuth-Android/) på hur toocomplete hello resten av hello-processen.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-165">Please refer toohello [AppAuth guide](https://openid.github.io/AppAuth-Android/) on how toocomplete hello rest of hello process.</span></span> <span data-ttu-id="1fe6e-166">Om du behöver tooquickly komma igång med en fungerande app, kolla [exemplet](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-166">If you need tooquickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="1fe6e-167">Åtgärderna i hello hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) tooenter Azure AD B2C konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-167">Follow hello steps in hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) tooenter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="1fe6e-168">Vi är alltid öppna toofeedback och förslag!</span><span class="sxs-lookup"><span data-stu-id="1fe6e-168">We are always open toofeedback and suggestions!</span></span> <span data-ttu-id="1fe6e-169">Om du har problem med det här avsnittet eller rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="1fe6e-169">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span> <span data-ttu-id="1fe6e-170">För funktionsbegäranden, lägga till dem för[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="1fe6e-170">For feature requests, add them too[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>


---
title: "Azure Active Directory B2C: Hämta en token som använder en Android-App | Microsoft Docs"
description: "Den här artikeln visar hur du skapar en Android-app som använder AppAuth med Azure Active Directory B2C hanterar användaridentiteter och autentiserar användare."
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
ms.openlocfilehash: cd4b8048245be49ea79bcb1b364f2f99c56f8291
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a><span data-ttu-id="0d3e0-103">Azure AD B2C: Logga in med ett Android-program</span><span class="sxs-lookup"><span data-stu-id="0d3e0-103">Azure AD B2C: Sign-in using an Android application</span></span>

<span data-ttu-id="0d3e0-104">Microsofts identitetsplattform använder öppna standarder som OAuth2 och OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="0d3e0-105">Det innebär att utvecklare kan utnyttja alla bibliotek som de vill integrera med våra tjänster.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-105">This allows developers to leverage any library they wish to integrate with our services.</span></span> <span data-ttu-id="0d3e0-106">Vi har skrivit några genomgång som detta att demonstrera hur du konfigurerar 3 part bibliotek för att ansluta till Microsoft identity-plattformen för att hjälpa utvecklare i vår plattform med andra bibliotek.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-106">To aid developers in using our platform with other libraries, we've written a few walkthroughs like this one to demonstrate how to configure 3rd party libraries to connect to the Microsoft identity platform.</span></span> <span data-ttu-id="0d3e0-107">De flesta bibliotek som implementerar [RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kan ansluta till Microsoft Identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) will be able to connect to the Microsoft Identity platform.</span></span>

> [!WARNING]
> <span data-ttu-id="0d3e0-108">Microsoft tillhandahåller inte korrigeringar för 3 part bibliotek och inte har gjort en genomgång av dessa bibliotek.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-108">Microsoft does not provide fixes for 3rd party libraries and has not done a review of those libraries.</span></span> <span data-ttu-id="0d3e0-109">Det här exemplet använder ett 3 part bibliotek kallas AppAuth har testats för kompatibilitet i grundläggande scenarier med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-109">This sample is using a 3rd party library called AppAuth that has been tested for compatibility in basic scenarios with the Azure AD B2C.</span></span> <span data-ttu-id="0d3e0-110">Problem och funktionsförfrågningar ska dirigeras till biblioteksprojekt öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-110">Issues and feature requests should be directed to the library's open-source project.</span></span> <span data-ttu-id="0d3e0-111">Se [i den här artikeln](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) för mer information.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-111">Please see [this article](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) for more information.</span></span>  
>
>

<span data-ttu-id="0d3e0-112">Om du inte har erfarenhet av OAuth2 eller OpenID Connect kanske du inte får ut så mycket av den här exempelkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-112">If you're new to OAuth2 or OpenID Connect much of this sample configuration may not make much sense to you.</span></span> <span data-ttu-id="0d3e0-113">Vi rekommenderar att du läser en kort [översikt över protokollet som vi har dokumenterat här](active-directory-b2c-reference-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-113">We recommend you look at a brief [overview of the protocol we've documented here](active-directory-b2c-reference-protocols.md).</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="0d3e0-114">Skaffa en Azure AD B2C-katalog</span><span class="sxs-lookup"><span data-stu-id="0d3e0-114">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="0d3e0-115">Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-115">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="0d3e0-116">En katalog är en behållare för alla användare, appar, grupper och mer.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-116">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="0d3e0-117">Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-117">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="0d3e0-118">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="0d3e0-118">Create an application</span></span>

<span data-ttu-id="0d3e0-119">Därefter måste du skapa en app i B2C-katalogen.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-119">Next, you need to create an app in your B2C directory.</span></span> <span data-ttu-id="0d3e0-120">Det ger Azure AD den information som tjänsten behöver för att kommunicera säkert med din app.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-120">This gives Azure AD information that it needs to communicate securely with your app.</span></span> <span data-ttu-id="0d3e0-121">Så här skapar du en mobil app [instruktionerna](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-121">To create a mobile app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="0d3e0-122">Se till att:</span><span class="sxs-lookup"><span data-stu-id="0d3e0-122">Be sure to:</span></span>

* <span data-ttu-id="0d3e0-123">Inkludera en **Native Client** i programmet.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-123">Include a **Native Client** in the application.</span></span>
* <span data-ttu-id="0d3e0-124">Kopiera **program-ID:t** som har tilldelats din app.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-124">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="0d3e0-125">Du behöver det senare.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-125">You will need this later.</span></span>
* <span data-ttu-id="0d3e0-126">Konfigurera en intern klient **omdirigerings-URI** (t.ex. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-126">Set up a native client **Redirect URI** (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect).</span></span> <span data-ttu-id="0d3e0-127">Du behöver även detta senare.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-127">You will also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="0d3e0-128">Skapa principer</span><span class="sxs-lookup"><span data-stu-id="0d3e0-128">Create your policies</span></span>

<span data-ttu-id="0d3e0-129">I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-129">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="0d3e0-130">Den här appen innehåller en identity-upplevelse: en kombinerad inloggning och registrering.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-130">This app contains one identity experience: a combined sign-in and sign-up.</span></span> <span data-ttu-id="0d3e0-131">Du måste skapa den här principen, som beskrivs i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-131">You need to create this policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="0d3e0-132">Tänk på följande när du skapar principen:</span><span class="sxs-lookup"><span data-stu-id="0d3e0-132">When you create the policy, be sure to:</span></span>

* <span data-ttu-id="0d3e0-133">Välj den **visningsnamn** som ett anmälan attribut i principen.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-133">Choose the **Display name** as a sign-up attribute in your policy.</span></span>
* <span data-ttu-id="0d3e0-134">Välj det **visningsnamn** och **objekt-ID** som programmet gör anspråk på i varje princip.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-134">Choose the **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="0d3e0-135">Du kan också välja andra anspråk.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-135">You can choose other claims as well.</span></span>
* <span data-ttu-id="0d3e0-136">Kopiera **namnet** på varje princip när du har skapat den.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-136">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="0d3e0-137">Det bör ha prefixet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-137">It should have the prefix `b2c_1_`.</span></span>  <span data-ttu-id="0d3e0-138">Du behöver principnamnet senare.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-138">You'll need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="0d3e0-139">När du har skapat dina principer är det dags att bygga appen.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-139">After you have created your policies, you're ready to build your app.</span></span>

## <a name="download-the-sample-code"></a><span data-ttu-id="0d3e0-140">Hämta exempelkoden</span><span class="sxs-lookup"><span data-stu-id="0d3e0-140">Download the sample code</span></span>

<span data-ttu-id="0d3e0-141">Vi har angett ett fungerande exempel som använder AppAuth med Azure AD B2C [på GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-141">We have provided a working sample that uses AppAuth with Azure AD B2C [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="0d3e0-142">Du kan hämta koden och kör den.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-142">You can download the code and run it.</span></span> <span data-ttu-id="0d3e0-143">Du kan snabbt komma igång med din egen app med Azure AD B2C konfigurationen genom att följa anvisningarna i den [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-143">You can quickly get started with your own app using your own Azure AD B2C configuration by following the instructions in the [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).</span></span>

<span data-ttu-id="0d3e0-144">Exemplet är en ändring av exemplet som anges av [AppAuth](https://openid.github.io/AppAuth-Android/).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-144">The sample is a modification of the sample provided by [AppAuth](https://openid.github.io/AppAuth-Android/).</span></span> <span data-ttu-id="0d3e0-145">Besök deras sidan om du vill veta mer om AppAuth och dess funktioner.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-145">Please visit their page to learn more about AppAuth and its features.</span></span>

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a><span data-ttu-id="0d3e0-146">Ändra din app att använda Azure AD B2C med AppAuth</span><span class="sxs-lookup"><span data-stu-id="0d3e0-146">Modifying your app to use Azure AD B2C with AppAuth</span></span>

> [!NOTE]
> <span data-ttu-id="0d3e0-147">AppAuth stöder Android API 16 (Jellybean) och senare.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-147">AppAuth supports Android API 16 (Jellybean) and above.</span></span> <span data-ttu-id="0d3e0-148">Vi rekommenderar att du använder API-23 och högre.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-148">We recommend using API 23 and above.</span></span>
>

### <a name="configuration"></a><span data-ttu-id="0d3e0-149">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="0d3e0-149">Configuration</span></span>

<span data-ttu-id="0d3e0-150">Du kan konfigurera kommunikation med Azure AD B2C genom att ange anger identifieringen URI eller genom att ange både autentiseringsslutpunkt och tokenslutpunkten URI: er.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-150">You can configure communication with Azure AD B2C by either specifying the discovery URI or by specifying both the authorization endpoint and token endpoint URIs.</span></span> <span data-ttu-id="0d3e0-151">I båda fallen måste följande information:</span><span class="sxs-lookup"><span data-stu-id="0d3e0-151">In either case, you will need the following information:</span></span>

* <span data-ttu-id="0d3e0-152">Klient-ID (t.ex. contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="0d3e0-152">Tenant ID (e.g. contoso.onmicrosoft.com)</span></span>
* <span data-ttu-id="0d3e0-153">Principens namn (t.ex. B2C\_1\_SignUpIn)</span><span class="sxs-lookup"><span data-stu-id="0d3e0-153">Policy name (e.g. B2C\_1\_SignUpIn)</span></span>

<span data-ttu-id="0d3e0-154">Om du väljer att automatiskt identifiera auktoriserings- och tokenslutpunkten URI: er måste att hämta information från identifiering URI.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-154">If you choose to automatically discover the authorization and token endpoint URIs, you will need to fetch information from the discovery URI.</span></span> <span data-ttu-id="0d3e0-155">Identifieringen URI kan genereras genom att ersätta innehavaren\_-ID och principen\_namn i följande URL:</span><span class="sxs-lookup"><span data-stu-id="0d3e0-155">The discovery URI can be generated by replacing the Tenant\_ID and the Policy\_Name in the following URL:</span></span>

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

<span data-ttu-id="0d3e0-156">Du kan hämta auktoriserings- och tokenslutpunkten URI: er och skapa ett AuthorizationServiceConfiguration objekt genom att köra följande:</span><span class="sxs-lookup"><span data-stu-id="0d3e0-156">You can then acquire the authorization and token endpoint URIs and create an AuthorizationServiceConfiguration object by running the following:</span></span>

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
            Log.w(TAG, "Failed to retrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed to authorization...
        }
      }
  });
```

<span data-ttu-id="0d3e0-157">I stället för att få fram auktorisering och tokenslutpunkten URI: er med hjälp av identifiering du kan också ange dem explicit genom att ersätta innehavaren\_-ID och principen\_namn i URL: er nedan:</span><span class="sxs-lookup"><span data-stu-id="0d3e0-157">Instead of using discovery to obtain the authorization and token endpoint URIs, you can also specify them explicitly by replacing the Tenant\_ID and the Policy\_Name in the URL's below:</span></span>

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

<span data-ttu-id="0d3e0-158">Kör följande kod för att skapa AuthorizationServiceConfiguration-objekt:</span><span class="sxs-lookup"><span data-stu-id="0d3e0-158">Run the following code to create your AuthorizationServiceConfiguration object:</span></span>

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform the auth request...
```

### <a name="authorizing"></a><span data-ttu-id="0d3e0-159">Auktorisera</span><span class="sxs-lookup"><span data-stu-id="0d3e0-159">Authorizing</span></span>

<span data-ttu-id="0d3e0-160">När du konfigurerar eller hämtar en auktorisering tjänstkonfiguration, kan en begäran om godkännande konstrueras.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-160">After configuring or retrieving an authorization service configuration, an authorization request can be constructed.</span></span> <span data-ttu-id="0d3e0-161">För att skapa begäran, behöver du följande information:</span><span class="sxs-lookup"><span data-stu-id="0d3e0-161">To create the request, you will need the following information:</span></span>

* <span data-ttu-id="0d3e0-162">Klient-ID (t.ex. 00000000-0000-0000-0000-000000000000)</span><span class="sxs-lookup"><span data-stu-id="0d3e0-162">Client ID (e.g. 00000000-0000-0000-0000-000000000000)</span></span>
* <span data-ttu-id="0d3e0-163">Omdirigerings-URI med ett anpassat schema (t.ex. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span><span class="sxs-lookup"><span data-stu-id="0d3e0-163">Redirect URI with a custom scheme (e.g. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)</span></span>

<span data-ttu-id="0d3e0-164">Båda objekten ska har sparats när du var [registrera din app](#create-an-application).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-164">Both items should have been saved when you were [registering your app](#create-an-application).</span></span>

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

<span data-ttu-id="0d3e0-165">Mer information finns i [AppAuth guide](https://openid.github.io/AppAuth-Android/) om hur du Slutför resten av processen.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-165">Please refer to the [AppAuth guide](https://openid.github.io/AppAuth-Android/) on how to complete the rest of the process.</span></span> <span data-ttu-id="0d3e0-166">Om du behöver att snabbt komma igång med en fungerande app kan ta en titt [exemplet](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-166">If you need to quickly get started with a working app, check out [our sample](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c).</span></span> <span data-ttu-id="0d3e0-167">Följ stegen i den [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) att ange din egen Azure AD B2C-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-167">Follow the steps in the [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) to enter your own Azure AD B2C configuration.</span></span>

<span data-ttu-id="0d3e0-168">Vi är alltid öppna för feedback och förslag!</span><span class="sxs-lookup"><span data-stu-id="0d3e0-168">We are always open to feedback and suggestions!</span></span> <span data-ttu-id="0d3e0-169">Om du har problem med det här avsnittet eller rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="0d3e0-169">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at the bottom of the page.</span></span> <span data-ttu-id="0d3e0-170">För funktionsbegäranden, lägga till dem i [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="0d3e0-170">For feature requests, add them to [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>


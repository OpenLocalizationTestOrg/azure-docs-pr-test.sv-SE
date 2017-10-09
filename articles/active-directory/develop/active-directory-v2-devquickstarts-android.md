---
title: aaaAzure Active Directory v2.0 Android-app | Microsoft Docs
description: "Hur hello toobuild en Android-app som loggar in användare med både personliga Microsoft-konto och arbete eller skola konton och anrop Graph API med hjälp av tredjeparts-bibliotek."
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 16294c07-f27d-45c9-833f-7dbb12083794
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 1dd40bd3bcea28c629abce09abaed66b38774162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-android-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a><span data-ttu-id="79220-103">Lägga till inloggning tooan Android-app som använder ett tredjeparts-bibliotek med Graph API: et med hello v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="79220-103">Add sign-in tooan Android app using a third-party library with Graph API using hello v2.0 endpoint</span></span>
<span data-ttu-id="79220-104">hello Microsoft identity-plattformen använder öppna standarder, till exempel OAuth2 och OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="79220-104">hello Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="79220-105">Utvecklare kan använda alla bibliotek som de vill toointegrate med våra tjänster.</span><span class="sxs-lookup"><span data-stu-id="79220-105">Developers can use any library they want toointegrate with our services.</span></span> <span data-ttu-id="79220-106">toohelp utvecklare använda vår plattform med andra bibliotek, vi har skrivit några genomgång som den här en toodemonstrate hur tooconfigure från tredje part bibliotek tooconnect toohello Microsoft identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="79220-106">toohelp developers use our platform with other libraries, we've written a few walkthroughs like this one toodemonstrate how tooconfigure third-party libraries tooconnect toohello Microsoft identity platform.</span></span> <span data-ttu-id="79220-107">De flesta bibliotek som implementerar [hello RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kan ansluta toohello Microsoft identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="79220-107">Most libraries that implement [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect toohello Microsoft identity platform.</span></span>

<span data-ttu-id="79220-108">Hello om programmet som skapar den här genomgången kan användare logga in tootheir organisation och sök sedan efter sig själva i organisationen med hjälp av hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="79220-108">With hello application that this walkthrough creates, users can sign in tootheir organization and then search for themselves in their organization by using hello Graph API.</span></span>

<span data-ttu-id="79220-109">Om du är ny tooOAuth2 eller OpenID Connect är mycket av det här exemplet konfigurationen inte meningsfullt tooyou.</span><span class="sxs-lookup"><span data-stu-id="79220-109">If you're new tooOAuth2 or OpenID Connect, much of this sample configuration may not make sense tooyou.</span></span> <span data-ttu-id="79220-110">Vi rekommenderar att du läser [2.0 protokoll - OAuth 2.0 auktorisering kod flöda](active-directory-v2-protocols-oauth-code.md) för bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="79220-110">We recommend that you read [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="79220-111">Vissa funktioner i vår plattform som har ett uttryck i hello OAuth2 eller OpenID Connect standarder, till exempel villkorlig åtkomst och hantering av Intune behöver du toouse våra Microsoft Azure identitet bibliotek med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="79220-111">Some features of our platform that do have an expression in hello OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you toouse our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="79220-112">hello v2.0-slutpunkten har inte stöd för alla Azure Active Directory-scenarier och funktioner.</span><span class="sxs-lookup"><span data-stu-id="79220-112">hello v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="79220-113">toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="79220-113">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-hello-code-from-github"></a><span data-ttu-id="79220-114">Hämta hello koden från GitHub</span><span class="sxs-lookup"><span data-stu-id="79220-114">Download hello code from GitHub</span></span>
<span data-ttu-id="79220-115">hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span><span class="sxs-lookup"><span data-stu-id="79220-115">hello code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span></span>  <span data-ttu-id="79220-116">toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) eller klona hello stommen:</span><span class="sxs-lookup"><span data-stu-id="79220-116">toofollow along, you can  [download hello app's skeleton as a .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) or clone hello skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

<span data-ttu-id="79220-117">Du kan också hämta hello exempel och komma igång nu direkt:</span><span class="sxs-lookup"><span data-stu-id="79220-117">You can also just download hello sample and get started right away:</span></span>

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="79220-118">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="79220-118">Register an app</span></span>
<span data-ttu-id="79220-119">Skapa en ny app på hello [programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följ hello detaljerade anvisningar på [hur tooregister en app med hello v2.0-slutpunkten](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="79220-119">Create a new app at hello [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow hello detailed steps at [How tooregister an app with hello v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="79220-120">Se till att:</span><span class="sxs-lookup"><span data-stu-id="79220-120">Make sure to:</span></span>

* <span data-ttu-id="79220-121">Kopiera hello **program-Id** som är tilldelade tooyour app eftersom du behöver den snart.</span><span class="sxs-lookup"><span data-stu-id="79220-121">Copy hello **Application Id** that's assigned tooyour app because you'll need it soon.</span></span>
* <span data-ttu-id="79220-122">Lägg till hello **Mobile** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="79220-122">Add hello **Mobile** platform for your app.</span></span>

> <span data-ttu-id="79220-123">Obs: hello programregistreringsportalen ger en **omdirigerings-URI** värde.</span><span class="sxs-lookup"><span data-stu-id="79220-123">Note: hello Application registration portal provides a **Redirect URI** value.</span></span> <span data-ttu-id="79220-124">I det här exemplet måste du dock använda hello standardvärdet `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span><span class="sxs-lookup"><span data-stu-id="79220-124">However, in this example you must use hello default value of `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>
> 
> 

## <a name="download-hello-nxoauth2-third-party-library-and-create-a-workspace"></a><span data-ttu-id="79220-125">Hämta hello NXOAuth2 tredjeparts-bibliotek och skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="79220-125">Download hello NXOAuth2 third-party library and create a workspace</span></span>
<span data-ttu-id="79220-126">Den här genomgången använder hello OIDCAndroidLib från GitHub, vilket är en OAuth2-biblioteket baserat på hello OpenID Connect koden för Google.</span><span class="sxs-lookup"><span data-stu-id="79220-126">For this walkthrough, you will use hello OIDCAndroidLib from GitHub, which is an OAuth2 library based on hello OpenID Connect code of Google.</span></span> <span data-ttu-id="79220-127">Den implementerar hello programspecifika profil och stöder hello autentiseringsslutpunkt för hello användare.</span><span class="sxs-lookup"><span data-stu-id="79220-127">It implements hello native application profile and supports hello authorization endpoint of hello user.</span></span> <span data-ttu-id="79220-128">Detta är allt du behöver toointegrate med identitetsplattformen för hello Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="79220-128">These are all hello things that you'll need toointegrate with hello Microsoft identity platform.</span></span>

<span data-ttu-id="79220-129">Klona hello OIDCAndroidLib lagringsplatsen tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="79220-129">Clone hello OIDCAndroidLib repo tooyour computer.</span></span>

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a><span data-ttu-id="79220-131">Konfigurera din miljö för Android Studio</span><span class="sxs-lookup"><span data-stu-id="79220-131">Set up your Android Studio environment</span></span>
1. <span data-ttu-id="79220-132">Skapa ett nytt Android Studio-projekt och acceptera hello standardinställningarna i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="79220-132">Create a new Android Studio project and accept hello defaults in hello wizard.</span></span>
   
    ![Skapa nytt projekt i Android Studio](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Mål Android-enheter](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Lägg till en aktivitet toomobile](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. <span data-ttu-id="79220-136">tooset Flytta upp dina projektmoduler hello klona lagringsplatsen toohello projektets plats.</span><span class="sxs-lookup"><span data-stu-id="79220-136">tooset up your project modules, move hello cloned repo toohello project location.</span></span> <span data-ttu-id="79220-137">Du kan också skapa hello-projektet och sedan klona den direkt toohello projektets plats.</span><span class="sxs-lookup"><span data-stu-id="79220-137">You can also create hello project and then clone it directly toohello project location.</span></span>
   
    ![Projektmoduler](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. <span data-ttu-id="79220-139">Öppna hello Projektinställningar moduler med hjälp av hello snabbmenyn eller med hjälp av hello Ctrl + Alt + Maj + S genväg.</span><span class="sxs-lookup"><span data-stu-id="79220-139">Open hello project modules settings by using hello context menu or by using hello Ctrl+Alt+Maj+S shortcut.</span></span>
   
    ![Moduler Projektinställningar](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. <span data-ttu-id="79220-141">Ta bort hello standard app modulen eftersom du bara vill hello projektinställningar för behållaren.</span><span class="sxs-lookup"><span data-stu-id="79220-141">Remove hello default app module because you only want hello project container settings.</span></span>
   
    ![hello standard app modul](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. <span data-ttu-id="79220-143">Importera moduler från hello klonade lagringsplatsen toohello aktuella projektet.</span><span class="sxs-lookup"><span data-stu-id="79220-143">Import modules from hello cloned repo toohello current project.</span></span>
   
    <span data-ttu-id="79220-144">![Importera gradle projekt](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Skapa ny modulsida](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span><span class="sxs-lookup"><span data-stu-id="79220-144">![Import gradle project](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Create new module page](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span></span>
6. <span data-ttu-id="79220-145">Upprepa dessa steg för hello `oidlib-sample` modul.</span><span class="sxs-lookup"><span data-stu-id="79220-145">Repeat these steps for hello `oidlib-sample` module.</span></span>
7. <span data-ttu-id="79220-146">Kontrollera hello oidclib beroenden på hello `oidlib-sample` modul.</span><span class="sxs-lookup"><span data-stu-id="79220-146">Check hello oidclib dependencies on hello `oidlib-sample` module.</span></span>
   
    ![oidclib beroenden för hello oidlib sampel modul](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. <span data-ttu-id="79220-148">Klicka på **OK** och vänta tills gradle-synkroniseringen.</span><span class="sxs-lookup"><span data-stu-id="79220-148">Click **OK** and wait for gradle sync.</span></span>
   
    <span data-ttu-id="79220-149">Din settings.gradle bör se ut som:</span><span class="sxs-lookup"><span data-stu-id="79220-149">Your settings.gradle should look like:</span></span>
   
    ![Skärmbild av settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. <span data-ttu-id="79220-151">Skapa hello exempel app toomake att hello urvalet körs korrekt.</span><span class="sxs-lookup"><span data-stu-id="79220-151">Build hello sample app toomake sure that hello sample running correctly.</span></span>
   
    <span data-ttu-id="79220-152">Du kommer inte att kunna toouse detta med Azure Active Directory ännu.</span><span class="sxs-lookup"><span data-stu-id="79220-152">You won't be able toouse this with Azure Active Directory yet.</span></span> <span data-ttu-id="79220-153">Vi behöver tooconfigure vissa slutpunkter först.</span><span class="sxs-lookup"><span data-stu-id="79220-153">We'll need tooconfigure some endpoints first.</span></span> <span data-ttu-id="79220-154">Detta är tooensure som du inte har en Android Studio problem innan vi börjar anpassa hello sample-appen.</span><span class="sxs-lookup"><span data-stu-id="79220-154">This is tooensure you don't have an Android Studio issues before we start customizing hello sample app.</span></span>
10. <span data-ttu-id="79220-155">Skapa och köra `oidlib-sample` som hello mål i Android Studio.</span><span class="sxs-lookup"><span data-stu-id="79220-155">Build and run `oidlib-sample` as hello target in Android Studio.</span></span>
    
    ![Förlopp för oidlib sampel build](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. <span data-ttu-id="79220-157">Ta bort hello `app ` katalogen som har lämnat när hello modulen bort från hello projektet eftersom Android Studio inte bort för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="79220-157">Delete hello `app ` directory that was left when you removed hello module from hello project because Android Studio doesn't delete it for safety.</span></span>
    
    ![Filstruktur som innehåller hello programkatalogen](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. <span data-ttu-id="79220-159">Öppna hello **redigera konfigurationer** menyn tooremove hello kör konfiguration som har också kvar när du har tagit bort hello modul från hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="79220-159">Open hello **Edit Configurations** menu tooremove hello run configuration that was also left when you removed hello module from hello project.</span></span>
    
    <span data-ttu-id="79220-160">![Redigera konfigurationer menyn](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![kör konfigurationen av appen](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span><span class="sxs-lookup"><span data-stu-id="79220-160">![Edit configurations menu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
![Run configuration of app](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span></span>

## <a name="configure-hello-endpoints-of-hello-sample"></a><span data-ttu-id="79220-161">Konfigurera hello slutpunkter av hello-exempel</span><span class="sxs-lookup"><span data-stu-id="79220-161">Configure hello endpoints of hello sample</span></span>
<span data-ttu-id="79220-162">Nu när du har hello `oidlib-sample` kör har vi redigera vissa slutpunkter tooget denna arbeta med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="79220-162">Now that you have hello `oidlib-sample` running successfully, let's edit some endpoints tooget this working with Azure Active Directory.</span></span>

### <a name="configure-your-client-by-editing-hello-oidcclientconfxml-file"></a><span data-ttu-id="79220-163">Konfigurera din klient genom att redigera hello oidc_clientconf.xml fil</span><span class="sxs-lookup"><span data-stu-id="79220-163">Configure your client by editing hello oidc_clientconf.xml file</span></span>
1. <span data-ttu-id="79220-164">Ange hello klienten toodo OAuth2 endast eftersom du använder OAuth2 flöden endast tooget en token och anropa hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="79220-164">Because you are using OAuth2 flows only tooget a token and call hello Graph API, set hello client toodo OAuth2 only.</span></span> <span data-ttu-id="79220-165">OIDC kommer i ett senare exempel.</span><span class="sxs-lookup"><span data-stu-id="79220-165">OIDC will come in a later example.</span></span>
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. <span data-ttu-id="79220-166">Konfigurera din klient-ID som du har fått från portalen för registrering av hello.</span><span class="sxs-lookup"><span data-stu-id="79220-166">Configure your client ID that you received from hello registration portal.</span></span>
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. <span data-ttu-id="79220-167">Konfigurera din omdirigerings-URI med hello en nedan.</span><span class="sxs-lookup"><span data-stu-id="79220-167">Configure your redirect URI with hello one below.</span></span>
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. <span data-ttu-id="79220-168">Konfigurera scope som du behöver i ordning tooaccess hello Graph API.</span><span class="sxs-lookup"><span data-stu-id="79220-168">Configure your scopes that you need in order tooaccess hello Graph API.</span></span>
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

<span data-ttu-id="79220-169">Hej `User.Read` värde i `oidc_scopes` tillåter du tooread hello grundläggande profilinformation hello inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="79220-169">hello `User.Read` value in `oidc_scopes` allows you tooread hello basic profile hello signed in user.</span></span>
<span data-ttu-id="79220-170">Du kan lära dig mer om alla tillgängliga hello-scope på [behörighetsomfattningen för Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="79220-170">You can learn more about all hello available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="79220-171">Om du vill ha information om `openid` eller `offline_access` som scope i OpenID Connect finns [2.0 protokoll - OAuth 2.0 auktorisering kod flöda](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="79220-171">If you'd like explanations about `openid` or `offline_access` as scopes in OpenID Connect, see [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md).</span></span>

### <a name="configure-your-client-endpoints-by-editing-hello-oidcendpointsxml-file"></a><span data-ttu-id="79220-172">Konfigurera dina klientslutpunkter för din genom att redigera hello oidc_endpoints.xml fil</span><span class="sxs-lookup"><span data-stu-id="79220-172">Configure your client endpoints by editing hello oidc_endpoints.xml file</span></span>
* <span data-ttu-id="79220-173">Öppna hello `oidc_endpoints.xml` filen och att Hej följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="79220-173">Open hello `oidc_endpoints.xml` file and make hello following changes:</span></span>
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

<span data-ttu-id="79220-174">Dessa slutpunkter bör aldrig ändras om du använder OAuth2 som din protokoll.</span><span class="sxs-lookup"><span data-stu-id="79220-174">These endpoints should never change if you are using OAuth2 as your protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="79220-175">Hej slutpunkter för `userInfoEndpoint` och `revocationEndpoint` stöds inte för närvarande av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="79220-175">hello endpoints for `userInfoEndpoint` and `revocationEndpoint` are currently not supported by Azure Active Directory.</span></span> <span data-ttu-id="79220-176">Om du lämnar dessa med hello example.com standardvärdet för att påminna dig att de inte är tillgängliga i exemplet hello :-)</span><span class="sxs-lookup"><span data-stu-id="79220-176">If you leave these with hello default example.com value, you will be reminded that they are not available in hello sample :-)</span></span>
> 
> 

## <a name="configure-a-graph-api-call"></a><span data-ttu-id="79220-177">Konfigurera ett Graph API-anrop</span><span class="sxs-lookup"><span data-stu-id="79220-177">Configure a Graph API call</span></span>
* <span data-ttu-id="79220-178">Öppna hello `HomeActivity.java` filen och att Hej följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="79220-178">Open hello `HomeActivity.java` file and make hello following changes:</span></span>
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

<span data-ttu-id="79220-179">Ett enkelt Graph API-anrop returnerar här våra information.</span><span class="sxs-lookup"><span data-stu-id="79220-179">Here a simple Graph API call returns our information.</span></span>

<span data-ttu-id="79220-180">De är alla hello ändringar som du behöver toodo.</span><span class="sxs-lookup"><span data-stu-id="79220-180">Those are all hello changes that you need toodo.</span></span> <span data-ttu-id="79220-181">Kör hello `oidlib-sample` program och klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="79220-181">Run hello `oidlib-sample` application, and click **Sign in**.</span></span>

<span data-ttu-id="79220-182">När du har autentiserats Välj hello **begära skyddade resursen** knappen tootest din anropet toohello Graph API.</span><span class="sxs-lookup"><span data-stu-id="79220-182">After you've successfully authenticated, select hello **Request Protected Resource** button tootest your call toohello Graph API.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="79220-183">Hämta säkerhetsuppdateringar för vår produkt</span><span class="sxs-lookup"><span data-stu-id="79220-183">Get security updates for our product</span></span>
<span data-ttu-id="79220-184">Vi rekommenderar att du tooget aviseringar om säkerhetsincidenter genom att besöka hello [säkerhet TechCenter](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="79220-184">We encourage you tooget notifications about security incidents by visiting hello [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>


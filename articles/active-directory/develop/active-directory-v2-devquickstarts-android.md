---
title: Azure Active Directory v2.0 Android-app | Microsoft Docs
description: "Hur du skapar en Android-app som loggar in användare med både personliga Microsoft-konto och arbete eller skola konton och Graph API-anrop med hjälp av tredjeparts-bibliotek."
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
ms.openlocfilehash: c0a5a818c61f7af7ff04bf890b54e8364f3b21b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a><span data-ttu-id="d3dd1-103">Lägga till inloggning till en Android-app med hjälp av en tredjeparts-bibliotek med Graph API: et med v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="d3dd1-103">Add sign-in to an Android app using a third-party library with Graph API using the v2.0 endpoint</span></span>
<span data-ttu-id="d3dd1-104">Microsofts identitetsplattform använder öppna standarder som OAuth2 och OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-104">The Microsoft identity platform uses open standards such as OAuth2 and OpenID Connect.</span></span> <span data-ttu-id="d3dd1-105">Utvecklare kan använda alla bibliotek som de vill integrera med våra tjänster.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-105">Developers can use any library they want to integrate with our services.</span></span> <span data-ttu-id="d3dd1-106">Vi har skrivit några genomgång som detta att demonstrera hur du konfigurerar tredjeparts-bibliotek för att ansluta till Microsoft identity-plattformen för att hjälpa utvecklare att använda vår plattform med andra bibliotek.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-106">To help developers use our platform with other libraries, we've written a few walkthroughs like this one to demonstrate how to configure third-party libraries to connect to the Microsoft identity platform.</span></span> <span data-ttu-id="d3dd1-107">De flesta bibliotek som implementerar [RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kan ansluta till Microsoft identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-107">Most libraries that implement [the RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) can connect to the Microsoft identity platform.</span></span>

<span data-ttu-id="d3dd1-108">Med det program som skapar den här genomgången, användare logga in i organisationen och sök sedan efter sig själva i organisationen med hjälp av Graph API.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-108">With the application that this walkthrough creates, users can sign in to their organization and then search for themselves in their organization by using the Graph API.</span></span>

<span data-ttu-id="d3dd1-109">Om du har använt OAuth2 eller OpenID Connect eventuellt mycket av det här exempelkonfiguration ingen vits till dig.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-109">If you're new to OAuth2 or OpenID Connect, much of this sample configuration may not make sense to you.</span></span> <span data-ttu-id="d3dd1-110">Vi rekommenderar att du läser [2.0 protokoll - OAuth 2.0 auktorisering kod flöda](active-directory-v2-protocols-oauth-code.md) för bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-110">We recommend that you read [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) for background.</span></span>

> [!NOTE]
> <span data-ttu-id="d3dd1-111">Vissa funktioner i vår plattform som har ett uttryck i OAuth2 eller OpenID Connect standarder, till exempel villkorlig åtkomst och hantering av Intune måste du använda våra Microsoft Azure identitet bibliotek med öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-111">Some features of our platform that do have an expression in the OAuth2 or OpenID Connect standards, such as Conditional Access and Intune policy management, require you to use our open source Microsoft Azure Identity Libraries.</span></span>
> 
> 

<span data-ttu-id="d3dd1-112">V2.0-slutpunkten har inte stöd för alla Azure Active Directory-scenarier och funktioner.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-112">The v2.0 endpoint does not support all Azure Active Directory scenarios and features.</span></span>

> [!NOTE]
> <span data-ttu-id="d3dd1-113">Läs mer om för att avgöra om du ska använda v2.0-slutpunkten [v2.0 begränsningar](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="d3dd1-113">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-the-code-from-github"></a><span data-ttu-id="d3dd1-114">Hämta koden från GitHub</span><span class="sxs-lookup"><span data-stu-id="d3dd1-114">Download the code from GitHub</span></span>
<span data-ttu-id="d3dd1-115">Koden för den här självstudiekursen [finns på GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span><span class="sxs-lookup"><span data-stu-id="d3dd1-115">The code for this tutorial is maintained [on GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).</span></span>  <span data-ttu-id="d3dd1-116">Om du vill följa med kan du [ladda ned appens stomme som en .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) eller klona stommen:</span><span class="sxs-lookup"><span data-stu-id="d3dd1-116">To follow along, you can  [download the app's skeleton as a .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) or clone the skeleton:</span></span>

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

<span data-ttu-id="d3dd1-117">Du kan också hämta exempelfilerna och komma igång nu direkt:</span><span class="sxs-lookup"><span data-stu-id="d3dd1-117">You can also just download the sample and get started right away:</span></span>

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a><span data-ttu-id="d3dd1-118">Registrera en app</span><span class="sxs-lookup"><span data-stu-id="d3dd1-118">Register an app</span></span>
<span data-ttu-id="d3dd1-119">Skapa en ny app på den [programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följer detaljerade anvisningar på [hur du registrerar en app med v2.0-slutpunkten](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="d3dd1-119">Create a new app at the [Application registration portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow the detailed steps at [How to register an app with the v2.0 endpoint](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="d3dd1-120">Se till att:</span><span class="sxs-lookup"><span data-stu-id="d3dd1-120">Make sure to:</span></span>

* <span data-ttu-id="d3dd1-121">Kopiera den **program-Id** som har tilldelats din app eftersom du behöver den snart.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-121">Copy the **Application Id** that's assigned to your app because you'll need it soon.</span></span>
* <span data-ttu-id="d3dd1-122">Lägg till den **Mobile** plattform för din app.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-122">Add the **Mobile** platform for your app.</span></span>

> <span data-ttu-id="d3dd1-123">Obs: Programregistreringsportalen ger en **omdirigerings-URI** värde.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-123">Note: The Application registration portal provides a **Redirect URI** value.</span></span> <span data-ttu-id="d3dd1-124">I det här exemplet måste du dock använda standardvärdet för `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-124">However, in this example you must use the default value of `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>
> 
> 

## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a><span data-ttu-id="d3dd1-125">Hämta NXOAuth2 från tredje part biblioteket och skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="d3dd1-125">Download the NXOAuth2 third-party library and create a workspace</span></span>
<span data-ttu-id="d3dd1-126">Den här genomgången använder OIDCAndroidLib från GitHub, vilket är en OAuth2-biblioteket baserat på OpenID Connect-koden för Google.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-126">For this walkthrough, you will use the OIDCAndroidLib from GitHub, which is an OAuth2 library based on the OpenID Connect code of Google.</span></span> <span data-ttu-id="d3dd1-127">Den implementerar interna programprofilen och stöder autentiseringsslutpunkt för användaren.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-127">It implements the native application profile and supports the authorization endpoint of the user.</span></span> <span data-ttu-id="d3dd1-128">Detta är allt som du behöver integrera med Microsoft identity-plattformen.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-128">These are all the things that you'll need to integrate with the Microsoft identity platform.</span></span>

<span data-ttu-id="d3dd1-129">Klona lagringsplatsen OIDCAndroidLib till datorn.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-129">Clone the OIDCAndroidLib repo to your computer.</span></span>

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a><span data-ttu-id="d3dd1-131">Konfigurera din miljö för Android Studio</span><span class="sxs-lookup"><span data-stu-id="d3dd1-131">Set up your Android Studio environment</span></span>
1. <span data-ttu-id="d3dd1-132">Skapa ett nytt Android Studio-projekt och acceptera standardinställningarna i guiden.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-132">Create a new Android Studio project and accept the defaults in the wizard.</span></span>
   
    ![Skapa nytt projekt i Android Studio](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Mål Android-enheter](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Lägga till en aktivitet till mobile](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. <span data-ttu-id="d3dd1-136">Flytta klonade lagringsplatsen till projektets plats om du vill konfigurera ditt projektmoduler.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-136">To set up your project modules, move the cloned repo to the project location.</span></span> <span data-ttu-id="d3dd1-137">Du kan också skapa projektet och klona den direkt till projektets plats.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-137">You can also create the project and then clone it directly to the project location.</span></span>
   
    ![Projektmoduler](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. <span data-ttu-id="d3dd1-139">Öppna projektet moduler inställningar med hjälp av snabbmenyn eller genom att använda kortkommandot Ctrl + Alt + Maj + S.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-139">Open the project modules settings by using the context menu or by using the Ctrl+Alt+Maj+S shortcut.</span></span>
   
    ![Moduler Projektinställningar](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. <span data-ttu-id="d3dd1-141">Ta bort modulen standard appen eftersom du bara vill projektinställningar för behållaren.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-141">Remove the default app module because you only want the project container settings.</span></span>
   
    ![Modulen standard app](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. <span data-ttu-id="d3dd1-143">Importera moduler från klonade lagringsplatsen till det aktuella projektet.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-143">Import modules from the cloned repo to the current project.</span></span>
   
    <span data-ttu-id="d3dd1-144">![Importera gradle projekt](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Skapa ny modulsida](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span><span class="sxs-lookup"><span data-stu-id="d3dd1-144">![Import gradle project](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Create new module page](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)</span></span>
6. <span data-ttu-id="d3dd1-145">Upprepa dessa steg för den `oidlib-sample` modul.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-145">Repeat these steps for the `oidlib-sample` module.</span></span>
7. <span data-ttu-id="d3dd1-146">Kontrollera oidclib-beroenden på den `oidlib-sample` modul.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-146">Check the oidclib dependencies on the `oidlib-sample` module.</span></span>
   
    ![oidclib beroenden på modulen oidlib-exempel](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. <span data-ttu-id="d3dd1-148">Klicka på **OK** och vänta tills gradle-synkroniseringen.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-148">Click **OK** and wait for gradle sync.</span></span>
   
    <span data-ttu-id="d3dd1-149">Din settings.gradle bör se ut som:</span><span class="sxs-lookup"><span data-stu-id="d3dd1-149">Your settings.gradle should look like:</span></span>
   
    ![Skärmbild av settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. <span data-ttu-id="d3dd1-151">Skapa sample-appen för att se till att provet körs korrekt.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-151">Build the sample app to make sure that the sample running correctly.</span></span>
   
    <span data-ttu-id="d3dd1-152">Du kan inte användas med Azure Active Directory ännu.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-152">You won't be able to use this with Azure Active Directory yet.</span></span> <span data-ttu-id="d3dd1-153">Vi behöver att konfigurera vissa slutpunkter först.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-153">We'll need to configure some endpoints first.</span></span> <span data-ttu-id="d3dd1-154">Detta är att se till att du inte har en Android Studio problem innan vi börjar anpassa sample-appen.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-154">This is to ensure you don't have an Android Studio issues before we start customizing the sample app.</span></span>
10. <span data-ttu-id="d3dd1-155">Skapa och köra `oidlib-sample` som mål i Android Studio.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-155">Build and run `oidlib-sample` as the target in Android Studio.</span></span>
    
    ![Förlopp för oidlib sampel build](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. <span data-ttu-id="d3dd1-157">Ta bort den `app ` katalogen som har lämnat när modulen bort från projektet eftersom Android Studio inte bort för säkerhet.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-157">Delete the `app ` directory that was left when you removed the module from the project because Android Studio doesn't delete it for safety.</span></span>
    
    ![Filstruktur som innehåller programkatalogen](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. <span data-ttu-id="d3dd1-159">Öppna den **redigera konfigurationer** du vill ta bort kör konfigurationen lämnades även när du har tagit bort modulen från projektet.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-159">Open the **Edit Configurations** menu to remove the run configuration that was also left when you removed the module from the project.</span></span>
    
    <span data-ttu-id="d3dd1-160">![Redigera konfigurationer menyn](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![kör konfigurationen av appen](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span><span class="sxs-lookup"><span data-stu-id="d3dd1-160">![Edit configurations menu](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
![Run configuration of app](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)</span></span>

## <a name="configure-the-endpoints-of-the-sample"></a><span data-ttu-id="d3dd1-161">Konfigurera slutpunkter i exempel</span><span class="sxs-lookup"><span data-stu-id="d3dd1-161">Configure the endpoints of the sample</span></span>
<span data-ttu-id="d3dd1-162">Nu när du har den `oidlib-sample` kör har vi att redigera vissa slutpunkter för att få den här arbeta med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-162">Now that you have the `oidlib-sample` running successfully, let's edit some endpoints to get this working with Azure Active Directory.</span></span>

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a><span data-ttu-id="d3dd1-163">Konfigurera din klient genom att redigera filen oidc_clientconf.xml</span><span class="sxs-lookup"><span data-stu-id="d3dd1-163">Configure your client by editing the oidc_clientconf.xml file</span></span>
1. <span data-ttu-id="d3dd1-164">Eftersom du använder OAuth2 flöden bara för att hämta en token och anropa Graph API angett att klienten ska göra OAuth2 enbart.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-164">Because you are using OAuth2 flows only to get a token and call the Graph API, set the client to do OAuth2 only.</span></span> <span data-ttu-id="d3dd1-165">OIDC kommer i ett senare exempel.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-165">OIDC will come in a later example.</span></span>
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. <span data-ttu-id="d3dd1-166">Konfigurera din klient-ID som du har fått från portalen för registrering.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-166">Configure your client ID that you received from the registration portal.</span></span>
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. <span data-ttu-id="d3dd1-167">Konfigurera omdirigerings-URI med nedan.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-167">Configure your redirect URI with the one below.</span></span>
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. <span data-ttu-id="d3dd1-168">Konfigurera scope som du behöver för att komma åt Graph API.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-168">Configure your scopes that you need in order to access the Graph API.</span></span>
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

<span data-ttu-id="d3dd1-169">Den `User.Read` värde i `oidc_scopes` kan du läsa de grundläggande profilinformation den signerade i användare.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-169">The `User.Read` value in `oidc_scopes` allows you to read the basic profile the signed in user.</span></span>
<span data-ttu-id="d3dd1-170">Du kan lära dig mer om alla tillgängliga scope på [behörighetsomfattningen för Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).</span><span class="sxs-lookup"><span data-stu-id="d3dd1-170">You can learn more about all the available scopes at [Microsoft Graph permission scopes](https://graph.microsoft.io/docs/authorization/permission_scopes).</span></span>

<span data-ttu-id="d3dd1-171">Om du vill ha information om `openid` eller `offline_access` som scope i OpenID Connect finns [2.0 protokoll - OAuth 2.0 auktorisering kod flöda](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="d3dd1-171">If you'd like explanations about `openid` or `offline_access` as scopes in OpenID Connect, see [2.0 Protocols - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md).</span></span>

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a><span data-ttu-id="d3dd1-172">Konfigurera dina klientslutpunkter för din genom att redigera filen oidc_endpoints.xml</span><span class="sxs-lookup"><span data-stu-id="d3dd1-172">Configure your client endpoints by editing the oidc_endpoints.xml file</span></span>
* <span data-ttu-id="d3dd1-173">Öppna den `oidc_endpoints.xml` filen och gör följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="d3dd1-173">Open the `oidc_endpoints.xml` file and make the following changes:</span></span>
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

<span data-ttu-id="d3dd1-174">Dessa slutpunkter bör aldrig ändras om du använder OAuth2 som din protokoll.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-174">These endpoints should never change if you are using OAuth2 as your protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="d3dd1-175">Slutpunkterna för `userInfoEndpoint` och `revocationEndpoint` stöds inte för närvarande av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-175">The endpoints for `userInfoEndpoint` and `revocationEndpoint` are currently not supported by Azure Active Directory.</span></span> <span data-ttu-id="d3dd1-176">Om du lämnar dessa med example.com standardvärdet för att påminna dig att de inte är tillgängliga i exemplet :-)</span><span class="sxs-lookup"><span data-stu-id="d3dd1-176">If you leave these with the default example.com value, you will be reminded that they are not available in the sample :-)</span></span>
> 
> 

## <a name="configure-a-graph-api-call"></a><span data-ttu-id="d3dd1-177">Konfigurera ett Graph API-anrop</span><span class="sxs-lookup"><span data-stu-id="d3dd1-177">Configure a Graph API call</span></span>
* <span data-ttu-id="d3dd1-178">Öppna den `HomeActivity.java` filen och gör följande ändringar:</span><span class="sxs-lookup"><span data-stu-id="d3dd1-178">Open the `HomeActivity.java` file and make the following changes:</span></span>
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

<span data-ttu-id="d3dd1-179">Ett enkelt Graph API-anrop returnerar här våra information.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-179">Here a simple Graph API call returns our information.</span></span>

<span data-ttu-id="d3dd1-180">Det här är de ändringar som du behöver göra.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-180">Those are all the changes that you need to do.</span></span> <span data-ttu-id="d3dd1-181">Kör den `oidlib-sample` program och klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-181">Run the `oidlib-sample` application, and click **Sign in**.</span></span>

<span data-ttu-id="d3dd1-182">När du har autentiserats, Välj den **begära skyddade resursen** knappen för att testa samtalet för Graph API.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-182">After you've successfully authenticated, select the **Request Protected Resource** button to test your call to the Graph API.</span></span>

## <a name="get-security-updates-for-our-product"></a><span data-ttu-id="d3dd1-183">Hämta säkerhetsuppdateringar för vår produkt</span><span class="sxs-lookup"><span data-stu-id="d3dd1-183">Get security updates for our product</span></span>
<span data-ttu-id="d3dd1-184">Vi rekommenderar att du få meddelanden om säkerhetsincidenter genom att besöka den [säkerhet TechCenter](https://technet.microsoft.com/security/dd252948) och prenumerera på Security Advisory-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="d3dd1-184">We encourage you to get notifications about security incidents by visiting the [Security TechCenter](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>


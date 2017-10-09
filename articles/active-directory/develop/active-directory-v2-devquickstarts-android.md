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
# <a name="add-sign-in-tooan-android-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a>Lägga till inloggning tooan Android-app som använder ett tredjeparts-bibliotek med Graph API: et med hello v2.0-slutpunkten
hello Microsoft identity-plattformen använder öppna standarder, till exempel OAuth2 och OpenID Connect. Utvecklare kan använda alla bibliotek som de vill toointegrate med våra tjänster. toohelp utvecklare använda vår plattform med andra bibliotek, vi har skrivit några genomgång som den här en toodemonstrate hur tooconfigure från tredje part bibliotek tooconnect toohello Microsoft identity-plattformen. De flesta bibliotek som implementerar [hello RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kan ansluta toohello Microsoft identity-plattformen.

Hello om programmet som skapar den här genomgången kan användare logga in tootheir organisation och sök sedan efter sig själva i organisationen med hjälp av hello Graph API.

Om du är ny tooOAuth2 eller OpenID Connect är mycket av det här exemplet konfigurationen inte meningsfullt tooyou. Vi rekommenderar att du läser [2.0 protokoll - OAuth 2.0 auktorisering kod flöda](active-directory-v2-protocols-oauth-code.md) för bakgrunden.

> [!NOTE]
> Vissa funktioner i vår plattform som har ett uttryck i hello OAuth2 eller OpenID Connect standarder, till exempel villkorlig åtkomst och hantering av Intune behöver du toouse våra Microsoft Azure identitet bibliotek med öppen källkod.
> 
> 

hello v2.0-slutpunkten har inte stöd för alla Azure Active Directory-scenarier och funktioner.

> [!NOTE]
> toodetermine om du ska använda hello v2.0-slutpunkten Läs om [v2.0 begränsningar](active-directory-v2-limitations.md).
> 
> 

## <a name="download-hello-code-from-github"></a>Hämta hello koden från GitHub
hello-koden för den här självstudiekursen upprätthålls [på GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  toofollow längs kan du [hämta hello appens stomme som en .zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) eller klona hello stommen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Du kan också hämta hello exempel och komma igång nu direkt:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Registrera en app
Skapa en ny app på hello [programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), eller följ hello detaljerade anvisningar på [hur tooregister en app med hello v2.0-slutpunkten](active-directory-v2-app-registration.md).  Se till att:

* Kopiera hello **program-Id** som är tilldelade tooyour app eftersom du behöver den snart.
* Lägg till hello **Mobile** plattform för din app.

> Obs: hello programregistreringsportalen ger en **omdirigerings-URI** värde. I det här exemplet måste du dock använda hello standardvärdet `https://login.microsoftonline.com/common/oauth2/nativeclient`.
> 
> 

## <a name="download-hello-nxoauth2-third-party-library-and-create-a-workspace"></a>Hämta hello NXOAuth2 tredjeparts-bibliotek och skapa en arbetsyta
Den här genomgången använder hello OIDCAndroidLib från GitHub, vilket är en OAuth2-biblioteket baserat på hello OpenID Connect koden för Google. Den implementerar hello programspecifika profil och stöder hello autentiseringsslutpunkt för hello användare. Detta är allt du behöver toointegrate med identitetsplattformen för hello Microsoft hello.

Klona hello OIDCAndroidLib lagringsplatsen tooyour dator.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Konfigurera din miljö för Android Studio
1. Skapa ett nytt Android Studio-projekt och acceptera hello standardinställningarna i guiden hello.
   
    ![Skapa nytt projekt i Android Studio](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Mål Android-enheter](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Lägg till en aktivitet toomobile](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. tooset Flytta upp dina projektmoduler hello klona lagringsplatsen toohello projektets plats. Du kan också skapa hello-projektet och sedan klona den direkt toohello projektets plats.
   
    ![Projektmoduler](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. Öppna hello Projektinställningar moduler med hjälp av hello snabbmenyn eller med hjälp av hello Ctrl + Alt + Maj + S genväg.
   
    ![Moduler Projektinställningar](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. Ta bort hello standard app modulen eftersom du bara vill hello projektinställningar för behållaren.
   
    ![hello standard app modul](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. Importera moduler från hello klonade lagringsplatsen toohello aktuella projektet.
   
    ![Importera gradle projekt](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Skapa ny modulsida](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)
6. Upprepa dessa steg för hello `oidlib-sample` modul.
7. Kontrollera hello oidclib beroenden på hello `oidlib-sample` modul.
   
    ![oidclib beroenden för hello oidlib sampel modul](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. Klicka på **OK** och vänta tills gradle-synkroniseringen.
   
    Din settings.gradle bör se ut som:
   
    ![Skärmbild av settings.gradle](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. Skapa hello exempel app toomake att hello urvalet körs korrekt.
   
    Du kommer inte att kunna toouse detta med Azure Active Directory ännu. Vi behöver tooconfigure vissa slutpunkter först. Detta är tooensure som du inte har en Android Studio problem innan vi börjar anpassa hello sample-appen.
10. Skapa och köra `oidlib-sample` som hello mål i Android Studio.
    
    ![Förlopp för oidlib sampel build](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. Ta bort hello `app ` katalogen som har lämnat när hello modulen bort från hello projektet eftersom Android Studio inte bort för säkerhet.
    
    ![Filstruktur som innehåller hello programkatalogen](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. Öppna hello **redigera konfigurationer** menyn tooremove hello kör konfiguration som har också kvar när du har tagit bort hello modul från hello-projekt.
    
    ![Redigera konfigurationer menyn](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![kör konfigurationen av appen](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-hello-endpoints-of-hello-sample"></a>Konfigurera hello slutpunkter av hello-exempel
Nu när du har hello `oidlib-sample` kör har vi redigera vissa slutpunkter tooget denna arbeta med Azure Active Directory.

### <a name="configure-your-client-by-editing-hello-oidcclientconfxml-file"></a>Konfigurera din klient genom att redigera hello oidc_clientconf.xml fil
1. Ange hello klienten toodo OAuth2 endast eftersom du använder OAuth2 flöden endast tooget en token och anropa hello Graph API. OIDC kommer i ett senare exempel.
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. Konfigurera din klient-ID som du har fått från portalen för registrering av hello.
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. Konfigurera din omdirigerings-URI med hello en nedan.
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. Konfigurera scope som du behöver i ordning tooaccess hello Graph API.
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

Hej `User.Read` värde i `oidc_scopes` tillåter du tooread hello grundläggande profilinformation hello inloggad användare.
Du kan lära dig mer om alla tillgängliga hello-scope på [behörighetsomfattningen för Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes).

Om du vill ha information om `openid` eller `offline_access` som scope i OpenID Connect finns [2.0 protokoll - OAuth 2.0 auktorisering kod flöda](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-hello-oidcendpointsxml-file"></a>Konfigurera dina klientslutpunkter för din genom att redigera hello oidc_endpoints.xml fil
* Öppna hello `oidc_endpoints.xml` filen och att Hej följande ändringar:
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Dessa slutpunkter bör aldrig ändras om du använder OAuth2 som din protokoll.

> [!NOTE]
> Hej slutpunkter för `userInfoEndpoint` och `revocationEndpoint` stöds inte för närvarande av Azure Active Directory. Om du lämnar dessa med hello example.com standardvärdet för att påminna dig att de inte är tillgängliga i exemplet hello :-)
> 
> 

## <a name="configure-a-graph-api-call"></a>Konfigurera ett Graph API-anrop
* Öppna hello `HomeActivity.java` filen och att Hej följande ändringar:
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Ett enkelt Graph API-anrop returnerar här våra information.

De är alla hello ändringar som du behöver toodo. Kör hello `oidlib-sample` program och klicka på **logga in**.

När du har autentiserats Välj hello **begära skyddade resursen** knappen tootest din anropet toohello Graph API.

## <a name="get-security-updates-for-our-product"></a>Hämta säkerhetsuppdateringar för vår produkt
Vi rekommenderar att du tooget aviseringar om säkerhetsincidenter genom att besöka hello [säkerhet TechCenter](https://technet.microsoft.com/security/dd252948) och prenumerera tooSecurity Advisory-aviseringar.


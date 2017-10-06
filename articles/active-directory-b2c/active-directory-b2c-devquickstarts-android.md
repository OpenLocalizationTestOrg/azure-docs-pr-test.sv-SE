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
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a>Azure AD B2C: Logga in med ett Android-program

hello Microsoft identity-plattformen använder öppna standarder, till exempel OAuth2 och OpenID Connect. Detta gör att utvecklare tooleverage något bibliotek de önskar toointegrate med våra tjänster. tooaid utvecklare i vår plattform med andra bibliotek, vi har skrivit några genomgång som den här en toodemonstrate hur tooconfigure 3 part bibliotek tooconnect toohello Microsoft identity-plattformen. De flesta bibliotek som implementerar [hello RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) kommer att kunna tooconnect toohello Microsoft Identity-plattformen.

> [!WARNING]
> Microsoft tillhandahåller inte korrigeringar för 3 part bibliotek och inte har gjort en genomgång av dessa bibliotek. Det här exemplet använder ett 3 part bibliotek kallas AppAuth har testats för kompatibilitet i grundläggande scenarier med hello Azure AD B2C. Problem och funktionsförfrågningar ska vara riktad toohello biblioteket öppen källkod projektet. Se [i den här artikeln](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) för mer information.  
>
>

Om du är ny tooOAuth2 eller OpenID Connect är mycket av det här exemplet konfigurationen inte mycket bra tooyou. Vi rekommenderar att du tittar på en kort [översikt över hello-protokollet som vi dokumenteras här](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Skaffa en Azure AD B2C-katalog

Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient. En katalog är en behållare för alla användare, appar, grupper och mer. Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.

## <a name="create-an-application"></a>Skapa ett program

Sedan måste toocreate en app i B2C-katalogen. Det ger Azure AD den information som behövs toocommunicate säkert med din app. Följ toocreate en mobil app [instruktionerna](active-directory-b2c-app-registration.md). Se till att:

* Inkludera en **Native Client** i hello program.
* Kopiera hello **program-ID** som är tilldelade tooyour app. Du behöver det senare.
* Konfigurera en intern klient **omdirigerings-URI** (t.ex. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Du behöver även detta senare.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Skapa principer

I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md). Den här appen innehåller en identity-upplevelse: en kombinerad inloggning och registrering. Du behöver toocreate principen, som beskrivs i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Tänk på att när du skapar hello princip:

* Välj hello **visningsnamn** som ett anmälan attribut i principen.
* Välj hello **visningsnamn** och **objekt-ID** programanspråken i varje princip. Du kan också välja andra anspråk.
* Kopiera hello **namn** på varje princip när du har skapat den. Det bör ha hello prefixet `b2c_1_`.  Du behöver hello principnamn senare.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

När du har skapat dina principer kan du är klar toobuild din app.

## <a name="download-hello-sample-code"></a>Hämta hello exempelkod

Vi har angett ett fungerande exempel som använder AppAuth med Azure AD B2C [på GitHub](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Du kan hämta hello koden och kör den. Du kan snabbt komma igång med din egen app med Azure AD B2C konfigurationen genom att följa instruktionerna hello i hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).

hello prov är en ändring av hello-exemplet som anges av [AppAuth](https://openid.github.io/AppAuth-Android/). Besök deras sidan toolearn mer om AppAuth och dess funktioner.

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a>Ändra din app toouse Azure AD B2C med AppAuth

> [!NOTE]
> AppAuth stöder Android API 16 (Jellybean) och senare. Vi rekommenderar att du använder API-23 och högre.
>

### <a name="configuration"></a>Konfiguration

Du kan konfigurera kommunikation med Azure AD B2C genom att antingen ange hello identifiera URI eller genom att ange både hello autentiseringsslutpunkt och tokenslutpunkten URI: er. I båda fallen måste hello följande information:

* Klient-ID (t.ex. contoso.onmicrosoft.com)
* Principens namn (t.ex. B2C\_1\_SignUpIn)

Om du väljer tooautomatically identifiera hello auktoriserings- och token-slutpunkt för URI: er, måste toofetch information från hello identifiering URI. hello identifiering URI kan genereras genom att ersätta hello klient\_-ID och hello princip\_namn i hello följande URL:

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

Du kan hämta hello auktoriserings- och tokenslutpunkten URI: er och skapa ett AuthorizationServiceConfiguration objekt genom att köra hello följande:

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

Istället för att använda identifiering tooobtain hello auktoriserings- och tokenslutpunkten URI: er, du kan också ange dem explicit genom att ersätta hello klient\_-ID och hello princip\_namn i hello URL nedan:

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

Kör hello följande kod toocreate AuthorizationServiceConfiguration-objekt:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform hello auth request...
```

### <a name="authorizing"></a>Auktorisera

När du konfigurerar eller hämtar en auktorisering tjänstkonfiguration, kan en begäran om godkännande konstrueras. toocreate hello begäran måste hello följande information:

* Klient-ID (t.ex. 00000000-0000-0000-0000-000000000000)
* Omdirigerings-URI med ett anpassat schema (t.ex. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)

Båda objekten ska har sparats när du var [registrera din app](#create-an-application).

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Se toohello [AppAuth guide](https://openid.github.io/AppAuth-Android/) på hur toocomplete hello resten av hello-processen. Om du behöver tooquickly komma igång med en fungerande app, kolla [exemplet](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Åtgärderna i hello hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) tooenter Azure AD B2C konfigurationen.

Vi är alltid öppna toofeedback och förslag! Om du har problem med det här avsnittet eller rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback på hello hello sidans nederkant. För funktionsbegäranden, lägga till dem för[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).


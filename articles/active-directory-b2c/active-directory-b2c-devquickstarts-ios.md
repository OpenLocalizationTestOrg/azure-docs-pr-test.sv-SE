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
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Azure AD B2C: Logga in med ett iOS-program

hello Microsoft identity-plattformen använder öppna standarder, till exempel OAuth2 och OpenID Connect. Med en öppen standard protokollet erbjuder flera developer valet när du väljer ett bibliotek toointegrate med våra tjänster. Vi har samlat i den här genomgången och andra som den tooaid utvecklare skriva program som ansluter toohello Microsoft Identity-plattformen. De flesta bibliotek som implementerar [hello RFC6749 OAuth2-specifikationen](https://tools.ietf.org/html/rfc6749) är kan tooconnect toohello Microsoft Identity-plattformen.

> [!WARNING]
> Microsoft inte tillhandahåller åtgärdar för tredjeparts-bibliotek och inte har gjort en genomgång av dessa bibliotek. Det här exemplet använder en tredje parts bibliotek kallas AppAuth har testats för kompatibilitet i grundläggande scenarier med hello Azure AD B2C. Problem och funktionsförfrågningar ska vara riktad toohello biblioteket öppen källkod projektet. Mer information finns i [den här artikeln](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

Om du är ny tooOAuth2 eller OpenID Connect är mycket av det här exemplet konfigurationen inte mycket bra tooyou. Vi rekommenderar att du tittar på en kort [översikt över hello-protokollet som vi dokumenteras här](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Skaffa en Azure AD B2C-katalog
Innan du kan använda Azure AD B2C måste du skapa en katalog eller klient. En katalog är en behållare för alla användare, appar, grupper och mer. Om du inte redan har en [skapar du en B2C-katalog](active-directory-b2c-get-started.md) innan du fortsätter.

## <a name="create-an-application"></a>Skapa ett program
Sedan måste toocreate en app i B2C-katalogen. Hej appregistrering ger Azure AD den information som behövs toocommunicate säkert med din app. Följ toocreate en mobil app [instruktionerna](active-directory-b2c-app-registration.md). Se till att:

* Inkludera en **Native client** i hello program.
* Kopiera hello **program-ID** som är tilldelade tooyour app. Du behöver detta GUID senare.
* Konfigurera en **omdirigerings-URI** med ett anpassat schema (till exempel com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Du behöver den här URI senare.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Skapa dina principer
I Azure AD B2C definieras varje användarupplevelse av en [princip](active-directory-b2c-reference-policies.md). Den här appen innehåller en identity-upplevelse: en kombinerad inloggning och registrering. Skapa den här principen enligt beskrivningen i den [referensartikeln om principer](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Tänk på att när du skapar hello princip:

* Under **registreringsattribut**väljer hello attributet **visningsnamn**.  Du kan välja samt andra attribut.
* Under **Programanspråk**, välja hello anspråk **visningsnamn** och **användarens objekt-ID**. Du kan välja andra anspråk.
* Kopiera hello **namn** på varje princip när du har skapat den. Principens namn med prefixet `b2c_1_` när du sparar hello princip.  Du måste hello principnamn senare.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

När du har skapat dina principer kan du är klar toobuild din app.

## <a name="download-hello-sample-code"></a>Hämta hello exempelkod
Vi har angett ett fungerande exempel som använder AppAuth med Azure AD B2C [på GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Du kan hämta hello koden och kör den. toouse din egen Azure AD B2C-klient, följer du anvisningarna hello i hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

Det här exemplet har skapats genom att följa anvisningarna för hello viktigt av hello [iOS AppAuth projektet på GitHub](https://github.com/openid/AppAuth-iOS). Mer information om hur hello exempel och hello biblioteket fungerar hello referens AppAuth README på GitHub.

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a>Ändra din app toouse Azure AD B2C med AppAuth

> [!NOTE]
> AppAuth stöder iOS 7 och senare.  Dock toosupport sociala inloggningar på Google, SFSafariViewController behövs som kräver iOS 9 eller högre.
>

### <a name="configuration"></a>Konfiguration

Du kan konfigurera kommunikation med Azure AD B2C genom att ange både hello autentiseringsslutpunkt och tokenslutpunkten URI: er.  toogenerate dessa URI: er måste hello följande information:
* Klient-ID (till exempel contoso.onmicrosoft.com)
* Principens namn (till exempel B2C\_1\_SignUpIn)

hello-token för slutpunkt URI kan genereras genom att ersätta hello klient\_-ID och hello princip\_namn i hello följande URL:

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

Hej autentiseringsslutpunkt URI kan genereras genom att ersätta hello klient\_-ID och hello princip\_namn i hello följande URL:

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

Kör hello följande kod toocreate AuthorizationServiceConfiguration-objekt:

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready tooperform hello auth request...
```

### <a name="authorizing"></a>Auktorisera

När du konfigurerar eller hämtar en auktorisering tjänstkonfiguration, kan en begäran om godkännande konstrueras. toocreate hello begär du behöver hello följande information:  
* Klient-ID (till exempel 00000000-0000-0000-0000-000000000000)
* Omdirigerings-URI med ett anpassat schema (till exempel com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)

Båda objekten ska har sparats när du var [registrera din app](#create-an-application).

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

tooset upp ditt program toohandle hello omdirigering toohello URI med hello anpassat schema, behöver du tooupdate hello lista över URL-scheman i din Info.pList:
* Öppna filen Info.pList.
* Hovra över en rad som 'Paket OS typen Code' och klicka på hello \+ symbolen.
* Byt namn på hello nya 'URL radtyper'.
* Klicka på hello pilen toohello vänster i URL-typer tooopen hello träd.
* Klicka på hello pilen toohello vänster 'Objektet 0' tooopen hello träd.
* Byt namn på första elementet under objektet 0 too'URL scheman.
* Klicka på hello pilen toohello vänster i URL-scheman tooopen hello träd.
* I kolumnen 'Value' hello är ett tomt fält toohello till vänster i-objektet 0' under URL-scheman.  Ange hello värdet tooyour programmets unika schemat.  hello-värdet måste matcha hello-schema som används i RedirectUrl anges när du skapar hello OIDAuthorizationRequest objekt.  I vårt exempel använde vi hello schemat 'com.onmicrosoft.fabrikamb2c.exampleapp'.

Se toohello [AppAuth guide](https://openid.github.io/AppAuth-iOS/) på hur toocomplete hello resten av hello-processen. Om du behöver tooquickly komma igång med en fungerande app, kolla [exemplet](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Åtgärderna i hello hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) tooenter Azure AD B2C konfigurationen.

Vi är alltid öppna toofeedback och förslag! Om du har problem med det här avsnittet eller rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback på hello hello sidans nederkant. För funktionsbegäranden, lägga till dem för[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

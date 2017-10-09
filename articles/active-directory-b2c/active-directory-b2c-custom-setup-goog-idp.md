---
title: "Azure Active Directory B2C: Lägga till Google + som en OAuth2-identitetsleverantör anpassade principer"
description: "Exempel med hjälp av Google + som identitetsleverantör med OAuth2-protokollet"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 4feff21979c9c3b3b12c7a1cae4db0121d1bd79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Lägga till Google + som en OAuth2-identitetsleverantör anpassade principer

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Den här guiden visar hur tooenable inloggning för användare från Google + konto via hello [anpassade principer](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Krav

Fullständig hello stegen i hello [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) artikel.

De här stegen innefattar:

1.  Skapa ett Google + konto program.
2.  Lägga till hello Google + konto programmet viktiga tooAzure AD B2C
3.  Lägga till anspråk tooa principen
4.  Registrera hello Google + konto anspråk providern tooa användaren resa
5.  Överför hello princip tooan Azure AD B2C-klient och testa den

## <a name="create-a-google-account-application"></a>Skapa ett Google + konto program
toouse Google + som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate Google +-programmet och lämna hello rätt parametrar. Du kan registrera en Google +-programmet här: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

1.  Gå toohello [Google utvecklare konsolen](https://console.developers.google.com/) och logga in med Google + autentiseringsuppgifterna för ditt konto.
2.  Klicka på **skapa projekt**, ange en **projektnamn**, och klicka sedan på **skapa**.

3.  Klicka på hello **menyn projekt**.

    ![Google +-konto – Välj projekt](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Klicka på hello  **+**  knappen.

    ![Google + konto – Skapa nytt projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Ange en **projektnamn**, och klicka sedan på **skapa**.

    ![Google +-konto – nytt projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Vänta tills hello projektet är klar och klicka på hello **menyn projekt**.

    ![Google + konto - vänta tills det nya projektet är klar toouse](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Klicka på ditt projektnamn.

    ![Google +-konto – Välj hello nytt projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Klicka på **API-hanterare** och klicka sedan på **autentiseringsuppgifter** i hello vänstra navigeringsrutan.
9.  Klicka på hello **OAuth-medgivande skärmen** fliken hello överst.

    ![Google +-konto – ange OAuth-medgivande skärmen](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Välj eller ange en giltig **e-postadress**, ange en **produktnamn**, och klicka på **spara**.

    ![Google + - programmet autentiseringsuppgifter](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Klicka på **nya autentiseringsuppgifter** och välj sedan **OAuth-klient-ID**.

    ![Google + - skapa nya autentiseringsuppgifter för programmet](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  Under **programtyp**väljer **webbprogrammet**.

    ![Google + – Välj apptyp](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Ange en **namn** för ditt program ange `https://login.microsoftonline.com` i hello **behörighet JavaScript ursprung** fältet och `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **auktoriserad omdirigerings-URI: er**fält. Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com). Hej **{klient}** värdet är skiftlägeskänsligt. Klicka på **Skapa**.

    ![Google + - ge behörighet JavaScript ursprung och omdirigerings-URI: er](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Kopiera hello värdena för **klient-Id** och **klienthemlighet**. Du måste båda tooconfigure Google + som en identitetsleverantör i din klient. **Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.

    ![Google + - kopia hello värden för klient-Id och klienten hemligt](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-hello-google-account-application-key-tooazure-ad-b2c"></a>Lägg till hello Google + konto programmet viktiga tooAzure AD B2C
Federation med Google + konton kräver en klienthemlighet för Google + konto tootrust Azure AD B2C uppdrag hello program. Du behöver toostore din Google + programhemlighet i Azure AD B2C-klient:  

1.  Gå tooyour Azure AD B2C-klient och använda **B2C inställningar** > **identitet upplevelse Framework**
2.  Välj **princip nycklar** tooview hello-nycklar som är tillgängliga i din klient.
3.  Klicka på **+ Lägg till**.
4.  För **alternativ**, använda **manuell**.
5.  För **namn**, Använd `GoogleSecret`.  
    hello prefixet `B2C_1A_` kan läggas till automatiskt.
6.  I hello **hemlighet** ange ditt hemliga för Microsoft-program från https://apps.dev.microsoft.com
7.  För **nyckelanvändning**, använda **signatur**.
8.  Klicka på **Skapa**
9.  Bekräfta att du har skapat hello nyckeln `B2C_1A_GoogleSecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Lägg till en anspråksprovider i din princip för tillägg

Om du vill användare toosign i med hjälp av Google + konto behöver du toodefine Google + konto som en anspråksprovider. Du behöver med andra ord toospecify en slutpunkt som Azure AD B2C kommunicerar med. hello endpoint innehåller en uppsättning anspråk som används av Azure AD B2C-tooverify som en specifik användare har autentiserats.

Definiera Google + kontot som en anspråksprovider genom att lägga till `<ClaimsProvider>` nod i tillägget-principfil:

1.  Öppna hello princip tilläggsfilen (TrustFrameworkExtensions.xml) från arbetskatalogen. Om du behöver en XML-redigerare [försök Visual Studio Code](https://code.visualstudio.com/download), en enkel plattformsoberoende redigerare.
2.  Hitta hello `<ClaimsProviders>` avsnitt
3.  Lägg till följande XML-kodstycke under hello hello `ClaimsProviders` element och Ersätt `client_id` värdet med Google + konto programmet klient-ID innan du sparar hello-filen.  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want hello user tooselect his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-hello-google-account-claims-provider-toosign-up-or-sign-in-user-journey"></a>Registrera hello Google + konto anspråk providern tooSign upp eller logga in användaren resa

hello identitetsleverantören har ställts in.  Men finns det inte i någon av hello sign-upp/inloggning skärmar. Lägg till hello Google + identitet providern tooyour användarkonto `SignUpOrSignIn` användaren resa. toomake det tillgängliga, skapar vi en dubblett av en befintlig mall användaren resa.  Vi Lägg sedan till hello Google + konto identitetsleverantör:

>[!NOTE]
>
>Om du har kopierat hello `<UserJourneys>` element från bas-fil för din princip toohello (TrustFrameworkExtensions.xml), kan du hoppa över toothis avsnittet.

1.  Öppna grundläggande hello-filen för principen (till exempel TrustFrameworkBase.xml).
2.  Hitta hello `<UserJourneys>` element och kopiera hela hello-innehållet i `<UserJourneys>` nod.
3.  Öppna filen hello-tillägg (till exempel TrustFrameworkExtensions.xml) och hitta hello `<UserJourneys>` element. Lägg till ett om hello elementet inte finns.
4.  Klistra in hello hela innehållet i `<UserJournesy>` nod som du kopierade som underordnad till hello `<UserJourneys>` element.

### <a name="display-hello-button"></a>Visa hello-knappen
Hej `<ClaimsProviderSelections>` elementet definierar hello lista med alternativ för val av anspråk providern och deras inbördes ordning.  `<ClaimsProviderSelection>`elementet är detsamma tooan identitet provider-knappen en sign-upp/inloggningssidan. Om du lägger till en `<ClaimsProviderSelection>` element för Google + konto, en ny knapp visas när en användare de hamnar på hello-sidan. tooadd det här elementet:

1.  Hitta hello `<UserJourney>` nod som innehåller `Id="SignUpOrSignIn"` i hello användaren resa som du kopierade.
2.  Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`
3.  Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Länken hello tooan åtgärd
Nu när du har en knapp på plats måste toolink den tooan åtgärd. hello åtgärd i det här fallet är för Azure AD B2C toocommunicate med Google + konto tooreceive en token.

1.  Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.
2.  Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Se till att hello `Id` har samma värde som hello `TargetClaimsExchangeId` i föregående avsnitt hello
> * Se till att `TechnicalProfileReferenceId` -ID har angetts toohello tekniska profilen du skapade tidigare (Google-OAUTH).

## <a name="upload-hello-policy-tooyour-tenant"></a>Överför hello princip tooyour klient
1.  I hello [Azure-portalen](https://portal.azure.com), växla till hello [kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och öppna hello **Azure AD B2C** bladet.
2.  Välj **identitet upplevelse Framework**.
3.  Öppna hello **alla principer** bladet.
4.  Välj **överföra princip**.
5.  Kontrollera **skriva över hello principen om den finns** rutan.
6.  **Överför** TrustFrameworkExtensions.xml och se till att inte misslyckas verifieringen hello

## <a name="test-hello-custom-policy-by-using-run-now"></a>Testa hello anpassad princip med Kör nu
1.  Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.

    >[!NOTE]
    >
    >    **Kör nu** kräver minst ett program toobe preregistered hello innehavaren. 
    >    toolearn hur tooregister program finns hello Azure AD B2C [Kom igång](active-directory-b2c-get-started.md) artikel eller hello [appregistrering](active-directory-b2c-app-registration.md) artikel.


2.  Öppna **B2C_1A_signup_signin**, hello förlitande part (RP) anpassad princip som du överfört. Välj **kör nu**.
3.  Du ska kunna toosign in med Google +.

## <a name="optional-register-hello-google-account-claims-provider-tooprofile-edit-user-journey"></a>[Valfritt] Registrera hello Google + konto anspråk providern tooProfile Redigera användare resa
Vill du kanske tooadd hello Google + konto identitetsleverantör också tooyour användaren `ProfileEdit` användaren resa. toomake den är tillgänglig, vi Upprepa hello senaste två steg:

### <a name="display-hello-button"></a>Visa hello-knappen
1.  Öppna filen för hello tillägg av principen (till exempel TrustFrameworkExtensions.xml).
2.  Hitta hello `<UserJourney>` nod som innehåller `Id="ProfileEdit"` i hello användaren resa som du kopierade.
3.  Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`
4.  Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Länken hello tooan åtgärd
1.  Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.
2.  Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a>Testa hello anpassad profil-Redigera princip genom att använda Kör nu

1.  Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.
2.  Öppna **B2C_1A_ProfileEdit**, hello förlitande part (RP) anpassad princip som du överfört. Välj **kör nu**.
3.  Du ska kunna toosign in med Google +.

## <a name="download-hello-complete-policy-files"></a>Hämta hello fullständig principfiler
Valfritt: Vi rekommenderar att du skapar ditt scenario med din egen anpassade principfiler när du har slutfört hello komma igång med anpassade principer igenom istället för att använda dessa exempelfiler.  [Exempelfiler för principen för referens](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)

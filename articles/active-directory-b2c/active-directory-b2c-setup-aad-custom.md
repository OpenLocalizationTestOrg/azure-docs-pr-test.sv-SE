---
title: "Azure Active Directory B2C: Lägg till en Azure AD-provider med hjälp av anpassade principer | Microsoft Docs"
description: "Lär dig mer om Azure Active Directory B2C anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 31f0dfe5-1ad0-4a25-a53b-8acc71bcea72
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: 9b0c32086cebc171d91da2e7bfb48136723ccd4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a>Azure Active Directory B2C: Logga in med hjälp av Azure AD-konton

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Den här artikeln lär du dig hur tooenable inloggning för användare från en viss Azure Active Directory (AD Azure) organisation via hello [anpassade principer](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Krav

Fullständig hello stegen i hello [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) artikel.

De här stegen innefattar:

1. Skapa ett Azure Active Directory B2C-klient (Azure AD B2C).
2. Skapar ett program för Azure AD B2C.
3. Registrering av två principmodulen program.
4. Inställning av nycklar.
5. Konfigurera hello startpaket.

## <a name="create-an-azure-ad-app"></a>Skapa en Azure AD-app

tooenable-inloggning för användare från en specifik Azure AD-organisation, behöver du tooregister ett program i hello organisationens Azure AD-klient.

>[!NOTE]
> Vi använder ”contoso.com” för hello organisationens Azure AD-klientorganisation och ”fabrikamb2c.onmicrosoft.com” som hello Azure AD B2C-klient i hello följa anvisningar.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
1. Välj ditt konto hello översta fältet. Från hello **Directory** Välj hello organisationens Azure AD-klient där du vill att tooregister programmet (contoso.com).
1. Välj **fler tjänster** i hello till vänster och Sök efter ”App registreringar”.
1. Välj **nya appregistrering**.
1. Ange ett namn för ditt program (till exempel `Azure AD B2C App`).
1. Välj **webbapp / API** för hello programtyp.
1. För **inloggnings-URL**, ange hello följande URL, där `yourtenant` ersätts av hello namn för din Azure AD B2C-klient (`fabrikamb2c.onmicrosoft.com`):

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Spara hello program-ID.
1. Välj hello nyligen skapade programmet.
1. Under hello **inställningar** bladet väljer **nycklar**.
1. Skapa en ny nyckel och spara den. Du använder den i hello stegen i hello nästa avsnitt.

## <a name="add-hello-azure-ad-key-tooazure-ad-b2c"></a>Lägg till nyckel hello Azure AD-tooAzure AD B2C

Du behöver toostore hello contoso.com programmet nyckeln i din Azure AD B2C-klient. toodo detta:
1. Gå tooyour Azure AD B2C-klient och använda **B2C inställningar** > **identitet upplevelse Framework** > **princip nycklar**.
1. Välj **+ Lägg till**.
1. Välj eller ange dessa alternativ:
   * Välj **manuell**.
   * För **namn**, Välj ett namn som matchar din Azure AD-klientnamn (till exempel `ContosoAppSecret`).  hello prefixet `B2C_1A_` läggs till automatiskt toohello namnet på din nyckel.
   * Klistra in programnyckeln i hello **hemlighet** rutan.
   * Välj **signatur**.
1. Välj **Skapa**.
1. Bekräfta att du har skapat hello nyckeln `B2C_1A_ContosoAppSecret`.


## <a name="add-a-claims-provider-in-your-base-policy"></a>Lägg till en anspråksprovider i en grundläggande princip

Om du vill användare toosign i med hjälp av Azure AD, måste toodefine Azure AD som en anspråksprovider. Du behöver med andra ord toospecify en slutpunkt som Azure AD B2C kommer att kommunicera med. hello endpoint ger en uppsättning anspråk som används av Azure AD B2C-tooverify som en specifik användare har autentiserats. 

Du kan definiera Azure AD som en anspråksprovider genom att lägga till Azure AD toohello `<ClaimsProvider>` nod i hello tilläggsfilen i principen:

1. Öppna filen hello-tillägg (TrustFrameworkExtensions.xml) från arbetskatalogen.
1. Hitta hello `<ClaimsProviders>` avsnitt. Om det inte finns lägger du till den under hello rotnoden.
1. Lägg till en ny `<ClaimsProvider>` nod på följande sätt:

    ```XML
    <ClaimsProvider>
        <Domain>Contoso</Domain>
        <DisplayName>Login using Contoso</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="ContosoProfile">
                <DisplayName>Contoso Employee</DisplayName>
                <Description>Login with your Contoso account</Description>
                <Protocol Name="OpenIdConnect"/>
                <OutputTokenFormat>JWT</OutputTokenFormat>
                <Metadata>
                    <Item Key="METADATA">https://login.windows.net/contoso.com/.well-known/openid-configuration</Item>
                    <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
                    <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="response_types">id_token</Item>
                    <Item Key="UsePolicyInRedirectUri">false</Item>
                </Metadata>
                <CryptographicKeys>
                    <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
                </CryptographicKeys>
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
                    <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
                    <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
                    <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
                    <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
                    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="contosoAuthentication" />
                    <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
                </OutputClaims>
                <OutputClaimsTransformations>
                    <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
                    <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
                    <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
                    <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
                </OutputClaimsTransformations>
                <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
            </TechnicalProfile>
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Under hello `<ClaimsProvider>` noden uppdatering hello värde för `<Domain>` tooa unikt värde som kan använda toodistinguish den från andra identitetsleverantörer.
1. Under hello `<ClaimsProvider>` noden uppdatering hello värde för `<DisplayName>` tooa eget namn för hello anspråk providern. Det här värdet används inte för närvarande.

### <a name="update-hello-technical-profile"></a>Uppdatera hello tekniska profil

tooget en token från hello Azure AD-slutpunkten måste toodefine hello protokoll att Azure AD B2C ska använda toocommunicate med Azure AD. Detta görs i hello `<TechnicalProfile>` element av `<ClaimsProvider>`.
1. Uppdatera hello-ID för hello `<TechnicalProfile>` nod. Detta ID är används toorefer toothis tekniska profil från andra delar av hello princip.
1. Uppdatera hello värde för `<DisplayName>`. Det här värdet kommer att visas på hello-knappen Logga in på skärmen inloggning.
1. Uppdatera hello värde för `<Description>`.
1. Azure AD används hello OpenID Connect-protokollet, så se till att hello-värdet för `<Protocol>` är `"OpenIdConnect"`.

Du behöver tooupdate hello `<Metadata>` i hello XML-filen som anges tooearlier tooreflect hello konfigurationsinställningar för ditt specifika Azure AD-klient. Uppdatera hello metadatavärden på följande sätt i hello XML-fil:

1. Ange `<Item Key="METADATA">` för`https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, där `yourAzureADtenant` är Azure AD-klientnamn (contoso.com).
1. Öppna din webbläsare och gå toohello `METADATA` URL som du precis har uppdaterats.
1. Leta efter hello 'issuer-objekt i hello webbläsare och kopiering av värdet. Det bör se ut som följande hello: `https://sts.windows.net/{tenantId}/`.
1. Klistra in hello värde för `<Item Key="ProviderName">` i hello XML-fil.
1. Ange `<Item Key="client_id">` toohello program-ID från hello appregistrering.
1. Ange `<Item Key="IdTokenAudience">` toohello program-ID från hello appregistrering.
1. Se till att `<Item Key="response_types">` har angetts för`id_token`.
1. Se till att `<Item Key="UsePolicyInRedirectUri">` har angetts för`false`.

Du måste också toolink hello Azure AD hemlighet som du har registrerat i din Azure AD med Azure AD B2C-klient toohello `<ClaimsProvider>` information:

* I hello `<CryptographicKeys>` under hello före XML-fil, uppdatera hello värde för `StorageReferenceId` toohello referens-ID för hello hemlighet som du definierade (till exempel `ContosoAppSecret`).

### <a name="upload-hello-extension-file-for-verification"></a>Överför hello tilläggsfilen för verifiering

Nu bör du har konfigurerat principen så att Azure AD B2C vet hur toocommunicate med Azure AD-katalogen. Försök att ladda upp hello tilläggsfilen av din princip bara tooconfirm som inte har några problem hittills. toodo så:

1. Öppna hello **alla principer** bladet i Azure AD B2C-klient.
1. Kontrollera hello **skriva över hello principen om den finns** rutan.
1. Ladda upp filen hello-tillägg (TrustFrameworkExtensions.xml) och se till att inte misslyckas verifieringen hello.

## <a name="register-hello-azure-ad-claims-provider-tooa-user-journey"></a>Registrera hello Azure AD anspråk providern tooa användaren resa

Du måste nu tooadd hello Azure AD identity providern tooone resor dina användare. Nu hello identitetsleverantören har ställts in, men den är inte tillgänglig i någon av hello sign-upp/inloggning skärmar. toomake det tillgängliga, vi skapar en dubblett av en befintlig mall användaren resa, och sedan ändra den så att den har även identitetsleverantör hello Azure AD:

1. Öppna grundläggande hello-filen för principen (till exempel TrustFrameworkBase.xml).
1. Hitta hello `<UserJourneys>` element och kopiera hello hela `<UserJourney>` nod som innehåller `Id="SignUpOrSignIn"`.
1. Öppna filen hello-tillägg (till exempel TrustFrameworkExtensions.xml) och hitta hello `<UserJourneys>` element. Lägg till ett om hello elementet inte finns.
1. Klistra in hello hela `<UserJourney>` nod som du kopierade som underordnad till hello `<UserJourneys>` element.
1. Byt namn på hello-ID för hello nya användare resa (t.ex, Byt namn på det `SignUpOrSignUsingContoso`).

### <a name="display-hello-button"></a>Visa hello ”knappen”

Hej `<ClaimsProviderSelection>` elementet är detsamma tooan identitet provider-knappen en sign-upp/inloggningssida. Om du lägger till en `<ClaimsProviderSelection>` element för Azure AD, en ny knapp visas när en användare de hamnar på hello-sidan. tooadd det här elementet:

1. Hitta hello `<OrchestrationStep>` nod som innehåller `Order="1"` i hello användaren resa som du nyss skapade.
1. Lägg till hello följande:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Ange `TargetClaimsExchangeId` tooan lämpligt värde. Vi rekommenderar följande hello samma konvention som andra:  *\[ClaimProviderName\]Exchange*.

### <a name="link-hello-button-tooan-action"></a>Länken hello tooan åtgärd

Nu när du har en knapp på plats måste toolink den tooan åtgärd. hello åtgärd i det här fallet är för Azure AD B2C toocommunicate med Azure AD tooreceive en token. Länka hello tooan åtgärd genom att länka tekniska hello-profil för din Azure AD-anspråksleverantör:

1. Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.
1. Lägg till hello följande:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Uppdatera `Id` toohello samma värde som `TargetClaimsExchangeId` i hello föregående avsnitt.
1. Uppdatera `TechnicalProfileReferenceId` toohello ID för hello tekniska profilen du skapade tidigare (ContosoProfile).

### <a name="upload-hello-updated-extension-file"></a>Överför hello uppdaterade tilläggsfilen

Du är klar ändra hello-tilläggsfilen. Spara den. Överför hello-fil och se till att alla verifieringar lyckas.

### <a name="update-hello-rp-file"></a>Uppdatera hello RP-filen

Nu behöver du tooupdate hello förlitande part (RP)-fil som initierar hello användaren resa som du just skapat:

1. Gör en kopia av SignUpOrSignIn.xml i arbetskatalogen och byta namn på den (till exempel byta namn på den tooSignUpOrSignInWithAAD.xml).
1. Öppna hello nya fil- och update hello `PolicyId` attribut för `<TrustFrameworkPolicy>` med ett unikt värde (till exempel SignUpOrSignInWithAAD). <br> Detta blir hello namnet på principen (till exempel B2C\_1A\_SignUpOrSignInWithAAD).
1. Ändra hello `ReferenceId` attribut i `<DefaultUserJourney>` toomatch hello-ID för hello nya användare resa som du skapade (SignUpOrSignUsingContoso).
1. Spara dina ändringar och överför hello-fil.

## <a name="troubleshooting"></a>Felsökning

Testa hello anpassad princip som du just har överfört öppna dess bladet och klicka på **kör nu**. toodiagnose problem, Läs mer om [felsökning](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Nästa steg

Ge feedback för[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).

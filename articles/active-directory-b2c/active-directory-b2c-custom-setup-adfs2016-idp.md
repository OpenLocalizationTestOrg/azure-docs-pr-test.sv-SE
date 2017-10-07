---
title: "Azure Active Directory B2C: Lägga till AD FS som en SAML-identitetsprovider anpassade principer"
description: Hur-tooarticle om hur du konfigurerar AD FS 2016 med SAML-protokoll och anpassade principer
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
ms.openlocfilehash: 30fb7700e7834e3d91fab1fc1b169b761584b204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Lägga till AD FS som en SAML-identitetsprovider anpassade principer

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Den här artikeln lär du dig hur tooenable inloggning för användare från AD FS-konto via hello [anpassade principer](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Krav

Fullständig hello stegen i hello [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) artikel.

De här stegen innefattar:

1.  Skapa en AD FS förlitande part.
2.  Lägger till hello ADFS förlitande part certifikat tooAzure AD B2C.
3.  Lägga till anspråk tooa principen.
4.  Registrera hello ADFS konto anspråk providern tooa användaren resa.
5.  Överför hello princip tooan Azure AD B2C-klient och testa den.

## <a name="toocreate-a-claims-aware-relying-party-trust"></a>toocreate ett anspråksmedvetet förtroende för förlitande part

toouse ADFS som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate en AD FS förtroende för förlitande part och lämna hello rätt parametrar.

tooadd en ny förlitande part förtroende med hjälp av hello AD FS snapin-modulen Hantering och manuellt konfigurera hello inställningar, utföra hello följa proceduren på en federationsserver.

Medlemskap i **administratörer**, eller motsvarande, på dator hello hello minsta nödvändiga toocomplete den här proceduren. Läs mer om hur du använder hello lämpliga konton och gruppmedlemskap i [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477)

1.  I Serverhanteraren klickar du på **verktyg**, och välj sedan **AD FS Management**.

2.  Klicka på **Lägg till förlitande part**.
    ![Lägg till förlitande part](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)

3.  På hello **Välkommen** väljer **anspråk medveten** och på **starta**.
    ![Välj anspråk medveten på hello välkomstsidan](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)
4.  På hello **Välj datakälla** klickar du på **ange information om hello förlitande part manuellt**, och klicka sedan på **nästa**.
    ![Ange information om hello förlitande part](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)

5.  På hello **ange visningsnamn** anger du ett namn i **visningsnamn**under **anteckningar** Skriv en beskrivning för den här förlitande parten och klickar sedan på **nästa** .
    ![Ange namn och anteckningar](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)
6.  Valfri. Om du har ett certifikat med valfritt tokenkryptering, sedan på hello **konfigurera certifikat** klickar du på **Bläddra** toolocate din certifikatsfil och klicka sedan på **nästa** .
    ![Konfigurera certifikat](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)
7.  På hello **konfigurera URL** sidan, Välj hello **aktivera stöd för hello protokollet SAML 2.0 WebSSO** kryssrutan. Under **URL för förlitande part SAML 2.0 SSO**skriver hello Security Assertion Markup Language (SAML) slutpunkt-URL för den här förlitande parten och klickar sedan på **nästa**.  För hello **URL för förlitande part SAML 2.0 SSO**, klistra in hello `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`. Ersätt {klient} med namnet för din klient (till exempel contosob2c.onmicrosoft.com) och Ersätt hello {princip} med tillägg-principnamn (till exempel B2C_1A_TrustFrameworkExtensions).
    > [!IMPORTANT]
    >hello principnamn är hello en signup_or_signin princip ärver från, i det här fallet är det: `B2C_1A_TrustFrameworkExtensions`.
    >Hello-URL kan till exempel vara: https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.

    ![Förlitande part SAML 2.0 SSO-tjänstens URL](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. På hello **konfigurera identifierare** anger hello samma Webbadress som hello föregående steg, klicka på **Lägg till** tooadd dem toohello listan och klicka sedan på **nästa**.
    ![Förlitande parts förtroende identifierare](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)
9.  På hello **Välj princip för åtkomstkontroll** Välj en princip och klicka på **nästa**.
    ![Välj princip för åtkomstkontroll](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)
10.  På hello **klar tooAdd förtroende** sidan Granska hello inställningarna och klicka sedan på **nästa** toosave din förlitande part information.
    ![Spara informationen om förlitande part förtroende](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)
11.  På hello **Slutför** klickar du på **Stäng**, den här åtgärden visas automatiskt hello **redigera Anspråksregler** dialogrutan.
    ![Redigera Anspråksregler](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)
12. Klicka på **Lägg till regel**.  
      ![Lägg till ny regel](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)
13.  I **anspråksregelmall**väljer **skicka LDAP-attribut som anspråk**.
    ![Välj Skicka LDAP-attribut som anspråk mall-regel](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)
14.  Ange **Regelnamn för anspråk**. För hello **attributarkiv** Välj **Välj Active Directory** Lägg till följande anspråk hello och klicka sedan på **Slutför** och **OK**.
    ![Ange egenskaper för regeln](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)
15.  I Serverhanteraren väljer **förtroende för förlitande part** Välj hello förlitande partsförtroende som du skapade och klicka sedan **egenskaper**.
    ![Redigera egenskaper för förlitande part](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)
16.  En hello förlitande part förtroende (B2C Demo) egenskaper klickar du på **signatur** och klicka på **Lägg till**.  
    ![Ange signatur](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)
17.  Lägg till din Signaturcertifikat (.cert-fil, utan privat nyckel).  
    ![Lägg till din Signaturcertifikat](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  Klicka på hello förlitande part förtroende (B2C Demo) i fönstret **Avancerat** fliken och ändra hello **Secure hash algorithm** för**SHA-1**, klickar du på **Ok**.  
    ![Ställ in säkra hash-algoritmen tooSHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)

## <a name="add-hello-adfs-account-application-key-tooazure-ad-b2c"></a>Lägg till hello ADFS konto programmet viktiga tooAzure AD B2C
Federation med AD FS-konton kräver en klienthemlighet för ADFS konto tootrust Azure AD B2C uppdrag hello program. Du måste toostore AD FS-certifikat i din Azure AD B2C-klient. 

1.  Gå tooyour Azure AD B2C-klient och använda **B2C inställningar** > **identitet upplevelse Framework**
2.  Välj **princip nycklar** tooview hello-nycklar som är tillgängliga i din klient.
3.  Klicka på **+ Lägg till**.
4.  För **alternativ**, använda **överför**.
5.  För **namn**, Använd `ADFSSamlCert`.  
    hello prefixet `B2C_1A_` kan läggas till automatiskt.
6.  I hello filöverföringen ** väljer din PFX-fil för certifikat med privat nyckel. Obs: det här certifikatet (med hello privata nyckeln) ska vara hello samma ett som utfärdats och används för hello ADFS förlitande part.
![Ladda upp nyckeln för principen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)
7.  Klicka på **Skapa**
8.  Bekräfta att du har skapat hello nyckeln `B2C_1A_ADFSSamlCert`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Lägg till en anspråksprovider i din princip för tillägg
Om du vill användare toosign i med hjälp av AD FS-konto behöver du toodefine ADFS-konto som en anspråksprovider. Du behöver med andra ord toospecify en slutpunkt som Azure AD B2C kommunicerar med. hello endpoint innehåller en uppsättning anspråk som används av Azure AD B2C-tooverify som en specifik användare har autentiserats.

Definiera ADFS som en anspråksprovider genom att lägga till `<ClaimsProvider>` nod i tillägget-principfil:

1. Öppna hello princip tilläggsfilen (TrustFrameworkExtensions.xml) från arbetskatalogen. Om du behöver en XML-redigerare [försök Visual Studio Code](https://code.visualstudio.com/download), en enkel plattformsoberoende redigerare.
2. Hitta hello `<ClaimsProviders>` avsnitt
3. Lägg till följande XML-kodstycke under hello hello `ClaimsProviders` element och Ersätt `identityProvider` med din DNS (godtyckligt värde som anger din domän) och sparar hello-filen. 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
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

## <a name="register-hello-adfs-account-claims-provider-toosign-up-or-sign-in-user-journey"></a>Registrera hello ADFS konto anspråk providern tooSign upp eller logga in användaren resa
Nu har hello identitetsleverantören ställts in.  Men finns det inte i någon av hello sign-upp/inloggning skärmar. Nu måste tooadd hello ADFS identitet providern tooyour användarkonto `SignUpOrSignIn` användaren resa. toomake det tillgängliga, skapar vi en dubblett av en befintlig mall användaren resa.  Sedan ändra vi den så att den omfattar hello ADFS identitetsleverantör:
    >[!NOTE]
    >If you previously copied hello `<UserJourneys>` element from base file of your policy toohello extension file (TrustFrameworkExtensions.xml) you can skip this section.
1.  Öppna grundläggande hello-filen för principen (till exempel TrustFrameworkBase.xml).
2.  Hitta hello `<UserJourneys>` element och kopiera hela hello-innehållet i `<UserJourneys>` nod.
3.  Öppna filen hello-tillägg (till exempel TrustFrameworkExtensions.xml) och hitta hello `<UserJourneys>` element. Lägg till ett om hello elementet inte finns.
4.  Klistra in hello hela innehållet i `<UserJournesy>` nod som du kopierade som underordnad till hello `<UserJourneys>` element.

### <a name="display-hello-button"></a>Visa hello-knappen
Hej `<ClaimsProviderSelections>` elementet definierar hello lista med alternativ för val av anspråk providern och deras inbördes ordning.  `<ClaimsProviderSelection>`elementet är detsamma tooan identitet provider-knappen en sign-upp/inloggningssidan. Om du lägger till en `<ClaimsProviderSelection>` element för AD FS-konto, en ny knapp visas när en användare de hamnar på hello-sidan. tooadd det här elementet:

1.  Hitta hello `<UserJourney>` nod som innehåller `Id="SignUpOrSignIn"` i hello användaren resa som du kopierade.
2.  Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`
3.  Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-hello-button-tooan-action"></a>Länken hello tooan åtgärd

Nu när du har en knapp på plats måste toolink den tooan åtgärd. hello åtgärd i det här fallet är för Azure AD B2C toocommunicate med AD FS-konto tooreceive en token. Länka hello tooan åtgärd genom att länka tekniska hello-profil för din AD FS-konto anspråksleverantör:

1.  Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.
2.  Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * Se till att hello `Id` har samma värde som hello `TargetClaimsExchangeId` i hello föregående avsnitt.
> * Se till att `TechnicalProfileReferenceId` anges toohello tekniska profilen du skapade tidigare (Contoso-SAML2).

## <a name="upload-hello-policy-tooyour-tenant"></a>Överför hello princip tooyour klient
1.  I hello [Azure-portalen](https://portal.azure.com), växla till hello [kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och öppna hello **Azure AD B2C** bladet.
2.  Välj **identitet upplevelse Framework**.
3.  Öppna hello **alla principer** bladet.
4.  Välj **överföra princip**.
5.  Kontrollera **skriva över hello principen om den finns** rutan.
6.  **Överför** TrustFrameworkExtensions.xml och se till att inte misslyckas verifieringen hello

## <a name="test-hello-custom-policy-by-using-run-now"></a>Testa hello anpassad princip med Kör nu
1.  Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.
2.  Öppna **B2C_1A_signup_signin**, hello förlitande part (RP) anpassad princip som du överfört. Välj **kör nu**.
3.  Du ska kunna toosign i med hjälp av AD FS-konto.

## <a name="optional-register-hello-adfs-account-claims-provider-tooprofile-edit-user-journey"></a>[Valfritt] Registrera hello ADFS konto anspråk providern tooProfile Redigera användare resa
Vill du kanske tooadd hello ADFS konto identitetsleverantör också tooyour användaren `ProfileEdit` användaren resa. toomake den är tillgänglig, vi Upprepa hello senaste två steg:

### <a name="display-hello-button"></a>Visa hello-knappen
1.  Öppna filen för hello tillägg av principen (till exempel TrustFrameworkExtensions.xml).
2.  Hitta hello `<UserJourney>` nod som innehåller `Id="ProfileEdit"` i hello användaren resa som du kopierade.
3.  Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`
4.  Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Länken hello tooan åtgärd
1.  Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.
2.  Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a>Testa hello anpassad profil-Redigera princip genom att använda Kör nu
1.  Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.
2.  Öppna **B2C_1A_ProfileEdit**, hello förlitande part (RP) anpassad princip som du överfört. Välj **kör nu**.
3.  Du ska kunna toosign i med hjälp av AD FS-konto.

## <a name="download-hello-complete-policy-files"></a>Hämta hello fullständig principfiler
Valfritt: Vi rekommenderar att du skapar ditt scenario med din egen anpassade principfiler när du har slutfört hello komma igång med anpassade principer igenom. [Principen exempelfiler endast för referens](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)

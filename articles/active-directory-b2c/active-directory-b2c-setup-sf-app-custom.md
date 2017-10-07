---
title: "Azure Active Directory B2C: Lägga till en Salesforce SAML-provider med hjälp av anpassade principer | Microsoft Docs"
description: "Mer information om hur toocreate och hantera anpassade principer för Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d7f4143f-cd7c-4939-91a8-231a4104dc2c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/11/2017
ms.author: parakhj
ms.openlocfilehash: f14c9d96980ff124110db7cfb58bf7cd81750b7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a>Azure Active Directory B2C: Logga in med hjälp av Salesforce konton via SAML

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Den här artikeln beskrivs hur du toouse [anpassade principer](active-directory-b2c-overview-custom.md) tooset in inloggning för användare från en viss Salesforce-organisation.

## <a name="prerequisites"></a>Krav

### <a name="azure-ad-b2c-setup"></a>Installationsprogram för Azure AD B2C

Kontrollera att du har slutfört alla steg i hello som visar hur för[komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) i Azure Active Directory B2C (Azure AD B2C).

Exempel på dessa är:

* Skapa en Azure AD B2C-klient.
* Skapa ett program med Azure AD B2C.
* Registrera två motorn för program.
* Ställ in nycklar.
* Ställ in hello startpaket.

### <a name="salesforce-setup"></a>Salesforce-installationen

I den här artikeln förutsätter vi att du redan har gjort hello följande:

* Registrerat dig för en Salesforce-konto. Du kan registrera dig för en [kostnadsfritt konto Developer Edition](https://developer.salesforce.com/signup).
* [Konfigurera min domän](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) för organisationen Salesforce.

## <a name="set-up-salesforce-so-users-can-federate"></a>Konfigurera Salesforce så att användare kan federera

toohelp Azure AD B2C kommunicera med Salesforce måste du tooget hello Salesforce metadata-URL.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Ställ in Salesforce som en identitetsleverantör

> [!NOTE]
> I den här artikeln förutsätter vi att du använder [Salesforce blixtsnabb upplevelse](https://developer.salesforce.com/page/Lightning_Experience_FAQ).

1. [Logga in tooSalesforce](https://login.salesforce.com/). 
2. På hello vänster-menyn under **inställningar**, expandera **identitet**, och klicka sedan på **identitetsleverantör**.
3. Klicka på **aktivera identitetsleverantör**.
4. Under **väljer hello certifikat**väljer hello-certifikat som du vill Salesforce toouse toocommunicate med Azure AD B2C. (Du kan använda hello standardcertifikatet.) Klicka på **Spara**. 

### <a name="create-a-connected-app-in-salesforce"></a>Skapa en ansluten app i Salesforce

1. På hello **identitetsleverantör** sidan finns för**leverantörer**.
2. Klicka på **leverantörer skapas nu via anslutna appar. Klicka här.**
3. Under **grundläggande Information**, ange hello krävs värden för anslutna appen.
4. Under **Webbprograminställningarna**väljer hello **aktivera SAML** kryssrutan.
5. I hello **enhets-ID** anger hello följande URL: en. Kontrollera att du ersätter hello värde för `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. I hello **ACS URL** anger hello följande URL: en. Kontrollera att du ersätter hello värde för `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Lämna hello standardvärden för alla andra inställningar.
8. Rulla toohello längst ned på hello listan och klicka sedan på **spara**.

### <a name="get-hello-metadata-url"></a>Hämta hello metadata-URL

1. Klicka på översiktssidan för anslutna appen hello **hantera**.
2. Kopiera hello värde för **Metadata slutpunktsidentifiering**, och spara den. Du ska använda senare i den här artikeln.

### <a name="set-up-salesforce-users-toofederate"></a>Ställ in Salesforce användare toofederate

1. På hello **hantera** sidan appens anslutna gå för**profiler**.
2. Klicka på **hantera profiler**.
3. Välj hello profiler (eller grupper av användare) som du vill toofederate med Azure AD B2C. Som systemadministratör bör du välja hello **systemadministratören** kryssrutan så att du kan federera med hjälp av ditt Salesforce-konto.

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a>Generera ett signeringscertifikat för Azure AD B2C

Begäranden skickade tooSalesforce måste toobe som signerats av Azure AD B2C. toogenerate signeringscertifikatet, öppna Azure PowerShell och kör sedan följande kommandon hello.

> [!NOTE]
> Kontrollera att du uppdaterar hello innehavarens namn och lösenord i hello översta två rader.

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-hello-saml-signing-certificate-tooazure-ad-b2c"></a>Lägg till hello SAML signering certifikat tooAzure AD B2C

Överför hello signering certifikat tooyour Azure AD B2C-klient: 

1. Gå tooyour Azure AD B2C-klient. Klicka på **inställningar** > **identitet upplevelse Framework** > **princip nycklar**.
2. Klicka på **+ Lägg till**, och sedan:
    1. Klicka på **alternativ** > **överför**.
    2. Ange en **namn** (till exempel SAMLSigningCert). hello prefixet *B2C_1A_* läggs till automatiskt toohello namnet på din nyckel.
    3. tooselect ditt certifikat väljer **överför filkontroll**. 
    4. Ange hello certifikatets lösenord som du anger i hello PowerShell-skript.
3. Klicka på **Skapa**.
4. Kontrollera att du har skapat en nyckel (till exempel B2C_1A_SAMLSigningCert). Anteckna hello fullständigt namn (inklusive *B2C_1A_*). Du kommer att referera toothis nyckeln senare i hello princip.

## <a name="create-hello-salesforce-saml-claims-provider-in-your-base-policy"></a>Skapa hello Salesforce SAML anspråksprovider i din grundläggande princip

Du måste toodefine Salesforce som en anspråksprovider så att användarna kan logga in med hjälp av Salesforce. Du behöver med andra ord toospecify hello slutpunkt som Azure AD B2C kommer att kommunicera med. hello endpoint kommer *ange* en uppsättning *anspråk* att Azure AD B2C använder tooverify som en specifik användare har autentiserats. toodo detta, Lägg till en `<ClaimsProvider>` för Salesforce i hello tilläggsfilen i principen:

1. Öppna filen hello-tillägg (TrustFrameworkExtensions.xml) i arbetskatalogen.
2. Hitta hello `<ClaimsProviders>` avsnitt. Om det inte finns kan du skapa det under hello rotnoden.
3. Lägg till en ny `<ClaimsProvider>`:

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="RequestsSigned">false</Item>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="externalIdp"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
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

Under hello `<ClaimsProvider>` nod:

1. Ändra hello för `<Domain>` tooa unikt värde som särskiljer `<ClaimsProvider>` från andra identitetsleverantörer.
2. Uppdatera hello värdet för `<DisplayName>` tooa visningsnamn för hello anspråk providern. Det här värdet används för närvarande inte.

### <a name="update-hello-technical-profile"></a>Uppdatera hello tekniska profil

tooget en SAML-token från Salesforce, definiera hello protokoll att Azure AD B2C kommer att använda toocommunicate med Azure Active Directory (AD Azure). Gör detta i hello `<TechnicalProfile>` element av `<ClaimsProvider>`:

1. Uppdatera hello-ID för hello `<TechnicalProfile>` nod. Detta ID är används toorefer toothis tekniska profil från andra delar av hello princip.
2. Uppdatera hello värde för `<DisplayName>`. Det här värdet visas på hello-knappen Logga in på sidan logga in.
3. Uppdatera hello värde för `<Description>`.
4. Salesforce använder hello SAML 2.0-protokollet. Se till att hello-värdet för `<Protocol>` är **SAML2**.

Uppdatera hello `<Metadata>` avsnitt i hello före XML-tooreflect hello inställningar för ditt specifika Salesforce-konto. Uppdatera hello metadatavärden i hello XML:

1. Uppdatera hello värdet för `<Item Key="PartnerEntity">` med hello Salesforce metadata-URL som du kopierade tidigare. Det har hello följande format: 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. I hello `<CryptographicKeys>` avsnittet hello värde för båda instanser av `StorageReferenceId` toohello namnet på hello nyckel för signeringscertifikatet (till exempel B2C_1A_SAMLSigningCert).

### <a name="upload-hello-extension-file-for-verification"></a>Överför hello tilläggsfilen för verifiering

Principen har nu konfigurerats så att Azure AD B2C vet hur toocommunicate med Salesforce. Försök att ladda upp hello tilläggsfilen i principen, tooverify att det inte finns några problem hittills. tooupload hello tilläggsfilen i principen:

1. I din Azure AD B2C-klient går toohello **alla principer** bladet.
2. Välj hello **skriva över hello principen om den finns** kryssrutan.
3. Ladda upp filen hello-tillägg (TrustFrameworkExtensions.xml). Se till att den inte inte kan valideras.

## <a name="register-hello-salesforce-saml-claims-provider-tooa-user-journey"></a>Registrera hello Salesforce SAML anspråk providern tooa användaren resa

Lägg till hello Salesforce SAML identitet providern tooone resor dina användare. Nu hello identitetsleverantören har ställts in, men den är inte tillgänglig på någon av hello användaren registrering eller inloggning sidor. tooadd hello identitet providern tooa inloggningssidan, först skapar en dubblett av en befintlig mall användaren resa. Ändra hello mallen så att den har även hello Azure AD-identitetsleverantör.

1. Öppna grundläggande hello-filen för principen (till exempel TrustFrameworkBase.xml).
2. Hitta hello `<UserJourneys>` element, och sedan kopiera hello hela `<UserJourney>` värde, inklusive Id = ”SignUpOrSignIn”.
3. Öppna filen hello-tillägg (till exempel TrustFrameworkExtensions.xml). Hitta hello `<UserJourneys>` element. Skapa en om hello elementet inte finns.
4. Klistra in hello hela kopieras `<UserJourney>` som underordnad till hello `<UserJourneys>` element.
5. Byt namn på hello-ID för nya hello `<UserJourney>` (till exempel SignUpOrSignUsingContoso).

### <a name="display-hello-identity-provider-button"></a>Visa hello identitet providern knappen

Hej `<ClaimsProviderSelection>` elementet är detsamma tooan identitet provider-knappen i ett registrering eller inloggning. Genom att lägga till en `<ClaimsProviderSelection>` element för Salesforce nya knappen visas när en användare toothis sidan. knapp för toodisplay hello identitet providern:

1. I hello `<UserJourney>` som du har skapat, hitta hello `<OrchestrationStep>` med `Order="1"`.
2. Lägg till följande XML hello:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. Ange `TargetClaimsExchangeId` tooa logiskt värde. Vi rekommenderar följande hello samma konvention som andra (exempelvis  *\[ClaimProviderName\]Exchange*).

### <a name="link-hello-identity-provider-button-tooan-action"></a>Länka hello identitet providern tooan åtgärd

Nu när du har en identity-providern knapp för länka tooan åtgärd. I det här fallet är hello åtgärd för Azure AD B2C toocommunicate med Salesforce tooreceive en SAML-token. Du kan göra detta genom att länka tekniska hello-profil för dina Salesforce SAML anspråksleverantör:

1. I hello `<UserJourney>` nod, hitta hello `<OrchestrationStep>` med `Order="2"`.
2. Lägg till följande XML hello:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. Uppdatera `Id` toohello samma värde som du använde tidigare i `TargetClaimsExchangeId`.
4. Uppdatera `TechnicalProfileReferenceId` toohello `Id` av hello tekniska profilen du skapade tidigare (till exempel ContosoProfile).

### <a name="upload-hello-updated-extension-file"></a>Överför hello uppdaterade tilläggsfilen

Du är klar ändra hello-tilläggsfilen. Spara och ladda upp den här filen. Se till att alla verifieringar lyckas.

### <a name="update-hello-relying-party-file"></a>Uppdatera hello förlitande part-filen

Därefter uppdaterar hello förlitande part (RP)-fil som initierar hello användaren resa som du skapade:

1. Gör en kopia av SignUpOrSignIn.xml i arbetskatalogen. Sedan, byta namn på den (till exempel SignUpOrSignInWithAAD.xml).
2. Öppna hello nya fil- och update hello `PolicyId` attribut för `<TrustFrameworkPolicy>` med ett unikt värde. Detta är hello namn för principen (till exempel SignUpOrSignInWithAAD).
3. Ändra hello `ReferenceId` attribut i `<DefaultUserJourney>` toomatch hello `Id` av hello nya användare resa som du skapade (till exempel SignUpOrSignUsingContoso).
4. Spara ändringarna och sedan överföra hello-fil.

## <a name="test-and-troubleshoot"></a>Testa och felsöka

tootest hello anpassad princip som du just har överfört, i hello Azure-portalen går toohello principbladet och klicka sedan på **kör nu**. Om den inte finns [felsöka principer för anpassade](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Nästa steg

Ge feedback för[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).

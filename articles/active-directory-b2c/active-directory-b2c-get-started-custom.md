---
title: "Azure Active Directory B2C: Komma igång med anpassade principer | Microsoft Docs"
description: "Hur tooget igång med Azure Active Directory B2C anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja;parahk;gsacavdm
ms.openlocfilehash: 5ee133806395cddf18682769a6cad149889d82d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-get-started-with-custom-policies"></a>Azure Active Directory B2C: Komma igång med anpassade principer

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

När du slutför hello stegen i den här artikeln är en anpassad princip stöder ”lokalt konto” registrering eller inloggning via en e-postadress och lösenord. Du kommer också att förbereda din miljö för att lägga till identitetsleverantörer (till exempel Facebook eller Azure Active Directory). Vi rekommenderar att du toocomplete som stegen innan du läsa mer om andra användningsområden för hello Azure Active Directory (AD Azure) B2C identitet upplevelse Framework.

## <a name="prerequisites"></a>Krav

Innan du fortsätter bör du kontrollera att du har en Azure AD B2C-klient som är en behållare för alla användare, appar, principer och mer. Om du inte har någon redan måste för[skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md). Vi rekommenderar att alla utvecklare toocomplete hello Azure AD B2C inbyggd princip genomgång starkt och konfigurera sina program med inbyggt principer innan du fortsätter. Dina program fungerar med båda typer av principer när du har en mindre ändring toohello princip namn tooinvoke hello anpassad princip.

>[!NOTE]
>tooaccess anpassad princip för redigering, behöver du en giltig Azure-prenumeration länkade tooyour klient. Om du inte gjort [länkade Azure-prenumeration för tooan din Azure AD B2C-klient](active-directory-b2c-how-to-enable-billing.md) eller Azure-prenumerationen är inaktiverad, hello identitet upplevelse Framework knappen inte tillgänglig.

## <a name="add-signing-and-encryption-keys-tooyour-b2c-tenant-for-use-by-custom-policies"></a>Lägg till signering och kryptering nycklar tooyour B2C-klient för användning av anpassade principer

1. Öppna hello **identitet upplevelse Framework** bladet i inställningarna för Azure AD B2C-klient.
2. Välj **princip nycklar** tooview hello-nycklar som är tillgängliga i din klient.
3. Skapa B2C_1A_TokenSigningKeyContainer om det inte finns:<br>
    a. Välj **Lägg till**. <br>
    b. Välj **generera**.<br>
    c. För **namn**, Använd `TokenSigningKeyContainer`. <br> 
    hello prefixet `B2C_1A_` kan läggas till automatiskt.<br>
    d. För **nyckeltyp**, använda **RSA**.<br>
    e. För **datum**, Använd hello standardvärden. <br>
    f. För **nyckelanvändning**, använda **signatur**.<br>
    g. Välj **Skapa**.<br>
4. Skapa B2C_1A_TokenEncryptionKeyContainer om det inte finns:<br>
 a. Välj **Lägg till**.<br>
 b. Välj **generera**.<br>
 c. För **namn**, Använd `TokenEncryptionKeyContainer`. <br>
   hello prefixet `B2C_1A`_ kan läggas till automatiskt.<br>
 d. För **nyckeltyp**, använda **RSA**.<br>
 e. För **datum**, Använd hello standardvärden.<br>
 f. För **nyckelanvändning**, använda **kryptering**.<br>
 g. Välj **Skapa**.<br>
5. Skapa B2C_1A_FacebookSecret. <br>
Om du redan har en hemlighet för Facebook-programmet kan du lägga till den som en princip för viktiga tooyour klient. I annat fall måste du skapa hello nyckel med en platshållare värdet så att dina principer valideras.<br>
 a. Välj **Lägg till**.<br>
 b. För **alternativ**, använda **manuell**.<br>
 c. För **namn**, Använd `FacebookSecret`. <br>
 hello prefixet `B2C_1A_` kan läggas till automatiskt.<br>
 d. I hello **hemlighet** ange din FacebookSecret från developers.facebook.com eller `0` som platshållare. *Detta är inte din Facebook app-ID.* <br>
 e. För **nyckelanvändning**, använda **signatur**. <br>
 f. Välj **skapa** och bekräfta skapas.

## <a name="register-identity-experience-framework-applications"></a>Registrera identiteten upplevelse Framework-program

Azure AD B2C kräver tooregister två extra program som används av hello motorn toosign in och logga in användare.

>[!NOTE]
>Du måste skapa två program för att aktivera inloggning med lokala konton: IdentityExperienceFramework (webbprogram) och ProxyIdentityExperienceFramework (en intern app) med delegerad behörighet från hello IdentityExperienceFramework app. Lokala konton finns bara i din klient. Användarna logga med en unik e-postadress och lösenord kombination tooaccess dina klient-registrerade program.

### <a name="create-hello-identityexperienceframework-application"></a>Skapa hello IdentityExperienceFramework program

1. I hello [Azure-portalen](https://portal.azure.com), växla till hello [kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md).
2. Öppna hello **Azure Active Directory** bladet (inte hello **Azure AD B2C** bladet). Du kan behöva tooselect **fler tjänster** toofind den.
3. Välj **App registreringar**.
4. Välj **nya appregistrering**.
   * För **namn**, Använd `IdentityExperienceFramework`.
   * För **programtyp**, använda **webb-app/API**.
   * För **inloggnings-URL**, använda `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, där `yourtenant` är ditt domännamn i Azure AD B2C-klient.
5. Välj **Skapa**.
6. När den har skapats, Välj hello nyskapad program **IdentityExperienceFramework**.<br>
   * Välj **egenskaper**.<br>
   * Kopiera hello program-ID och spara den för senare.

### <a name="create-hello-proxyidentityexperienceframework-application"></a>Skapa hello ProxyIdentityExperienceFramework program

1. Välj **App registreringar**.
1. Välj **nya appregistrering**.
   * För **namn**, Använd `ProxyIdentityExperienceFramework`.
   * För **programtyp**, använda **interna**.
   * För **omdirigerings-URI**, använda `https://login.microsoftonline.com/yourtenant.onmicrosoft.com`, där `yourtenant` är Azure AD B2C-klient.
1. Välj **Skapa**.
1. När den har skapats, Välj hello program **ProxyIdentityExperienceFramework**.<br>
   * Välj **egenskaper**. <br>
   * Kopiera hello program-ID och spara den för senare.
1. Välj **nödvändiga behörigheter**.
1. Välj **Lägg till**.
1. Välj **väljer en API**.
1. Sök efter hello namn IdentityExperienceFramework. Välj **IdentityExperienceFramework** i hello resultaten och klickar sedan på **Välj**.
1. Markera kryssrutan om hello bredvid för**åtkomst IdentityExperienceFramework**, och klicka sedan på **Välj**.
1. Välj **klar**.
1. Välj **bevilja med**, och sedan bekräfta genom att välja **Ja**.

## <a name="download-starter-pack-and-modify-policies"></a>Hämta startpaket och ändra principer

Anpassade principer är en uppsättning XML-filer som måste överföras toobe tooyour Azure AD B2C-klient. Vi tillhandahåller starter packs tooget du igång snabbare. Varje startpaket i hello följande lista innehåller hello minsta antal tekniska profiler och användaren resor behövs tooachieve hello scenarier beskrivs:
 * LocalAccounts. Använda hello lokala konton.
 * SocialAccounts. Använda hello (eller externa) sociala konton.
 * **SocialAndLocalAccounts**. Vi använder den här filen hello genomgång.
 * SocialAndLocalAccountsWithMFA. Alternativ för social, lokal och Multifaktorautentisering ingår.

Varje startpaket innehåller:

* Hej [grundläggande filen](active-directory-b2c-overview-custom.md#policy-files) av hello princip. Några ändringar är nödvändiga toohello bas.
* Hej [tilläggsfilen](active-directory-b2c-overview-custom.md#policy-files) av hello princip.  Den här filen är där de flesta konfigurationsändringar görs.
* [Förlitande part filer](active-directory-b2c-overview-custom.md#policy-files) uppgiftsspecifika filer som anropas av ditt program.

>[!NOTE]
>Om valideringen har stöd för XML-redigerare, validera hello filer mot hello TrustFrameworkPolicy_0.3.0.0.xsd XML-schema som finns i hello startpaket hello rotkatalog. XML-schemavalideringen identifierar fel innan du laddar upp.

 Nu sätter vi igång:

1. Hämta active-directory-b2c-custom-policy-starterpack från GitHub. [Hämta hello ZIP-filen](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) eller köra

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```
2. Öppna hello SocialAndLocalAccounts mapp.  Det grundläggande hello-fil (TrustFrameworkBase.xml) i den här mappen innehåller innehåll som behövs för både lokala och social företagets konton. hello sociala innehåll stör inte hello steg för att komma lokala konton och körs.
3. Öppna TrustFrameworkBase.xml. Om du behöver en XML-redigerare [försök Visual Studio Code](https://code.visualstudio.com/download), en enkel plattformsoberoende redigerare.
4. I hello rot `TrustFrameworkPolicy` element-, update hello `TenantId` och `PublicPolicyUri` attribut, ersätter `yourtenant.onmicrosoft.com` med hello domännamn för din Azure AD B2C-klient:
   ```xml
    <TrustFrameworkPolicy
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
    PolicySchemaVersion="0.3.0.0"
    TenantId="yourtenant.onmicrosoft.com"
    PolicyId="B2C_1A_TrustFrameworkBase"
    PublicPolicyUri="http://yourtenant.onmicrosoft.com">
    ```
   >[!NOTE]
   >`PolicyId`är hello princip som du ser i hello portal och hello namn som den här principfilen refererar till andra principfiler.

5. Spara hello-filen.
6. Öppna TrustFrameworkExtensions.xml. Ändra hello samma två genom att ersätta `yourtenant.onmicrosoft.com` med din Azure AD B2C-klient. Se hello samma ersättning i hello `<TenantId>` element för totalt tre ändringar. Spara hello-filen.
7. Öppna SignUpOrSignIn.xml. Ändra hello samma genom att ersätta `yourtenant.onmicrosoft.com` med din Azure AD B2C-klient på tre platser. Spara hello-filen.
8. Öppna hello lösenordsåterställning och profilen redigera filer. Ändra hello samma genom att ersätta `yourtenant.onmicrosoft.com` med din Azure AD B2C-klient på tre platser i varje fil. Spara hello-filer.

### <a name="add-hello-application-ids-tooyour-custom-policy"></a>Lägg till anpassad princip för tooyour hello program-ID: N
Lägg till filen med tillägg hello program-ID: N toohello (`TrustFrameworkExtensions.xml`):

1. Hitta hello element i hello-tillägg-filen (TrustFrameworkExtensions.xml) `<TechnicalProfile Id="login-NonInteractive">`.
2. Ersätt båda förekomster av `IdentityExperienceFrameworkAppId` med hello program-ID för hello identitet upplevelse Framework-program som du skapade tidigare. Här är ett exempel:

   ```xml
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```
3. Ersätt båda förekomster av `ProxyIdentityExperienceFrameworkAppId` med hello program-ID för hello Proxy identitet upplevelse Framework-program som du skapade tidigare.
4. Spara tilläggsfilen.

## <a name="upload-hello-policies-tooyour-tenant"></a>Överför hello principer tooyour klient

1. I hello [Azure-portalen](https://portal.azure.com), växla till hello [kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och öppna hello **Azure AD B2C** bladet.
2. Välj **identitet upplevelse Framework**.
3. Välj **överföra princip**.

    >[!WARNING]
    >hello anpassad principfiler måste överföras i hello följande ordning:

1. Överför TrustFrameworkBase.xml.
2. Överför TrustFrameworkExtensions.xml.
3. Överför SignUpOrSignin.xml.
4. Ladda upp dina övriga principfiler.

När en fil har överförts hello namnet på hello principfil inledd med `B2C_1A_`.

## <a name="test-hello-custom-policy-by-using-run-now"></a>Testa hello anpassad princip med Kör nu

1. Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.

   >[!NOTE]
   >**Kör nu** kräver minst ett program toobe preregistered hello innehavaren. Program som måste registreras i hello B2C-klient genom att använda hello **program** Menyvalet i Azure AD B2C eller genom att använda hello identitet upplevelse Framework tooinvoke både inbyggda och anpassade principer. Bara en registrering per program krävs.<br><br>
   toolearn hur tooregister program finns hello Azure AD B2C [Kom igång](active-directory-b2c-get-started.md) artikel eller hello [appregistrering](active-directory-b2c-app-registration.md) artikel.  

2. Öppna B2C_1A_signup_signin, hello förlitande part (RP) anpassad princip som du överfört. Välj **kör nu**.

3. Du ska kunna toosign med en e-postadress.

4. Logga in med hello samma konto tooconfirm som du har hello korrekt konfiguration.

>[!NOTE]
>En vanlig orsak till inloggningsfel är en felaktigt konfigurerad IdentityExperienceFramework app.


## <a name="next-steps"></a>Nästa steg

### <a name="add-facebook-as-an-identity-provider"></a>Lägg till Facebook som en identitetsleverantör
tooset in Facebook:
1. [Konfigurera en Facebook-programmet för developers.facebook.com](active-directory-b2c-setup-fb-app.md).
2. [Lägg till hello Facebook programmet hemliga tooyour Azure AD B2C-klient](#add-signing-and-encryption-keys-to-your-b2c-tenant-for-use-by-custom-policies).
3. Ersätt hello värdet för i principfilen för hello TrustFrameworkExtensions `client_id` med hello Facebook program-ID:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace hello value of client_id in this technical profile with hello Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```
4. Överför hello TrustFrameworkExtensions.xml princip filen tooyour klient.
5. Testa med **kör nu** eller genom att anropa hello principen direkt från din registrerade programmet.

### <a name="add-azure-active-directory-as-an-identity-provider"></a>Lägg till Azure Active Directory som en identitetsleverantör
grundläggande hello-filen används redan i den här Kom igång-guide innehåller hello innehållet som du behöver för att lägga till andra identitetsleverantörer. Information om hur du konfigurerar inloggningar finns hello [Azure Active Directory B2C: Logga in med hjälp av Azure AD-konton](active-directory-b2c-setup-aad-custom.md) artikel.

En översikt över anpassade principer som använder hello identitet upplevelse Framework i Azure AD B2C finns hello [Azure Active Directory B2C: anpassade policys](active-directory-b2c-overview-custom.md) artikel. 

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
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a><span data-ttu-id="d500f-103">Azure Active Directory B2C: Logga in med hjälp av Salesforce konton via SAML</span><span class="sxs-lookup"><span data-stu-id="d500f-103">Azure Active Directory B2C: Sign in by using Salesforce accounts via SAML</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="d500f-104">Den här artikeln beskrivs hur du toouse [anpassade principer](active-directory-b2c-overview-custom.md) tooset in inloggning för användare från en viss Salesforce-organisation.</span><span class="sxs-lookup"><span data-stu-id="d500f-104">This article shows you how toouse [custom policies](active-directory-b2c-overview-custom.md) tooset up sign-in for users from a specific Salesforce organization.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d500f-105">Krav</span><span class="sxs-lookup"><span data-stu-id="d500f-105">Prerequisites</span></span>

### <a name="azure-ad-b2c-setup"></a><span data-ttu-id="d500f-106">Installationsprogram för Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="d500f-106">Azure AD B2C setup</span></span>

<span data-ttu-id="d500f-107">Kontrollera att du har slutfört alla steg i hello som visar hur för[komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) i Azure Active Directory B2C (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="d500f-107">Ensure that you have completed all of hello steps that show you how too[get started with custom policies](active-directory-b2c-get-started-custom.md) in Azure Active Directory B2C (Azure AD B2C).</span></span>

<span data-ttu-id="d500f-108">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="d500f-108">These include:</span></span>

* <span data-ttu-id="d500f-109">Skapa en Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="d500f-109">Create an Azure AD B2C tenant.</span></span>
* <span data-ttu-id="d500f-110">Skapa ett program med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d500f-110">Create an Azure AD B2C application.</span></span>
* <span data-ttu-id="d500f-111">Registrera två motorn för program.</span><span class="sxs-lookup"><span data-stu-id="d500f-111">Register two policy engine applications.</span></span>
* <span data-ttu-id="d500f-112">Ställ in nycklar.</span><span class="sxs-lookup"><span data-stu-id="d500f-112">Set up keys.</span></span>
* <span data-ttu-id="d500f-113">Ställ in hello startpaket.</span><span class="sxs-lookup"><span data-stu-id="d500f-113">Set up hello starter pack.</span></span>

### <a name="salesforce-setup"></a><span data-ttu-id="d500f-114">Salesforce-installationen</span><span class="sxs-lookup"><span data-stu-id="d500f-114">Salesforce setup</span></span>

<span data-ttu-id="d500f-115">I den här artikeln förutsätter vi att du redan har gjort hello följande:</span><span class="sxs-lookup"><span data-stu-id="d500f-115">In this article, we assume that you have already done hello following:</span></span>

* <span data-ttu-id="d500f-116">Registrerat dig för en Salesforce-konto.</span><span class="sxs-lookup"><span data-stu-id="d500f-116">Signed up for a Salesforce account.</span></span> <span data-ttu-id="d500f-117">Du kan registrera dig för en [kostnadsfritt konto Developer Edition](https://developer.salesforce.com/signup).</span><span class="sxs-lookup"><span data-stu-id="d500f-117">You can sign up for a [free Developer Edition account](https://developer.salesforce.com/signup).</span></span>
* <span data-ttu-id="d500f-118">[Konfigurera min domän](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) för organisationen Salesforce.</span><span class="sxs-lookup"><span data-stu-id="d500f-118">[Set up a My Domain](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) for your Salesforce organization.</span></span>

## <a name="set-up-salesforce-so-users-can-federate"></a><span data-ttu-id="d500f-119">Konfigurera Salesforce så att användare kan federera</span><span class="sxs-lookup"><span data-stu-id="d500f-119">Set up Salesforce so users can federate</span></span>

<span data-ttu-id="d500f-120">toohelp Azure AD B2C kommunicera med Salesforce måste du tooget hello Salesforce metadata-URL.</span><span class="sxs-lookup"><span data-stu-id="d500f-120">toohelp Azure AD B2C communicate with Salesforce, you need tooget hello Salesforce metadata URL.</span></span>

### <a name="set-up-salesforce-as-an-identity-provider"></a><span data-ttu-id="d500f-121">Ställ in Salesforce som en identitetsleverantör</span><span class="sxs-lookup"><span data-stu-id="d500f-121">Set up Salesforce as an identity provider</span></span>

> [!NOTE]
> <span data-ttu-id="d500f-122">I den här artikeln förutsätter vi att du använder [Salesforce blixtsnabb upplevelse](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span><span class="sxs-lookup"><span data-stu-id="d500f-122">In this article, we assume you are using [Salesforce Lightning Experience](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span></span>

1. <span data-ttu-id="d500f-123">[Logga in tooSalesforce](https://login.salesforce.com/).</span><span class="sxs-lookup"><span data-stu-id="d500f-123">[Sign in tooSalesforce](https://login.salesforce.com/).</span></span> 
2. <span data-ttu-id="d500f-124">På hello vänster-menyn under **inställningar**, expandera **identitet**, och klicka sedan på **identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="d500f-124">On hello left menu, under **Settings**, expand **Identity**, and then click **Identity Provider**.</span></span>
3. <span data-ttu-id="d500f-125">Klicka på **aktivera identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="d500f-125">Click **Enable Identity Provider**.</span></span>
4. <span data-ttu-id="d500f-126">Under **väljer hello certifikat**väljer hello-certifikat som du vill Salesforce toouse toocommunicate med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d500f-126">Under **Select hello certificate**, select hello certificate you want Salesforce toouse toocommunicate with Azure AD B2C.</span></span> <span data-ttu-id="d500f-127">(Du kan använda hello standardcertifikatet.) Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d500f-127">(You can use hello default certificate.) Click **Save**.</span></span> 

### <a name="create-a-connected-app-in-salesforce"></a><span data-ttu-id="d500f-128">Skapa en ansluten app i Salesforce</span><span class="sxs-lookup"><span data-stu-id="d500f-128">Create a connected app in Salesforce</span></span>

1. <span data-ttu-id="d500f-129">På hello **identitetsleverantör** sidan finns för**leverantörer**.</span><span class="sxs-lookup"><span data-stu-id="d500f-129">On hello **Identity Provider** page, go too**Service Providers**.</span></span>
2. <span data-ttu-id="d500f-130">Klicka på **leverantörer skapas nu via anslutna appar. Klicka här.**</span><span class="sxs-lookup"><span data-stu-id="d500f-130">Click **Service Providers are now created via Connected Apps. Click here.**</span></span>
3. <span data-ttu-id="d500f-131">Under **grundläggande Information**, ange hello krävs värden för anslutna appen.</span><span class="sxs-lookup"><span data-stu-id="d500f-131">Under **Basic Information**,  enter hello required values for your connected app.</span></span>
4. <span data-ttu-id="d500f-132">Under **Webbprograminställningarna**väljer hello **aktivera SAML** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="d500f-132">Under **Web App Settings**, select hello **Enable SAML** check box.</span></span>
5. <span data-ttu-id="d500f-133">I hello **enhets-ID** anger hello följande URL: en.</span><span class="sxs-lookup"><span data-stu-id="d500f-133">In hello **Entity ID** field, enter hello following URL.</span></span> <span data-ttu-id="d500f-134">Kontrollera att du ersätter hello värde för `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="d500f-134">Ensure that you replace hello value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. <span data-ttu-id="d500f-135">I hello **ACS URL** anger hello följande URL: en.</span><span class="sxs-lookup"><span data-stu-id="d500f-135">In hello **ACS URL** field, enter hello following URL.</span></span> <span data-ttu-id="d500f-136">Kontrollera att du ersätter hello värde för `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="d500f-136">Ensure that you replace hello value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. <span data-ttu-id="d500f-137">Lämna hello standardvärden för alla andra inställningar.</span><span class="sxs-lookup"><span data-stu-id="d500f-137">Leave hello default values for all other settings.</span></span>
8. <span data-ttu-id="d500f-138">Rulla toohello längst ned på hello listan och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="d500f-138">Scroll toohello bottom of hello list, and then click **Save**.</span></span>

### <a name="get-hello-metadata-url"></a><span data-ttu-id="d500f-139">Hämta hello metadata-URL</span><span class="sxs-lookup"><span data-stu-id="d500f-139">Get hello metadata URL</span></span>

1. <span data-ttu-id="d500f-140">Klicka på översiktssidan för anslutna appen hello **hantera**.</span><span class="sxs-lookup"><span data-stu-id="d500f-140">On hello overview page of your connected app, click **Manage**.</span></span>
2. <span data-ttu-id="d500f-141">Kopiera hello värde för **Metadata slutpunktsidentifiering**, och spara den.</span><span class="sxs-lookup"><span data-stu-id="d500f-141">Copy hello value for **Metadata Discovery Endpoint**, and then save it.</span></span> <span data-ttu-id="d500f-142">Du ska använda senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="d500f-142">You'll use it later in this article.</span></span>

### <a name="set-up-salesforce-users-toofederate"></a><span data-ttu-id="d500f-143">Ställ in Salesforce användare toofederate</span><span class="sxs-lookup"><span data-stu-id="d500f-143">Set up Salesforce users toofederate</span></span>

1. <span data-ttu-id="d500f-144">På hello **hantera** sidan appens anslutna gå för**profiler**.</span><span class="sxs-lookup"><span data-stu-id="d500f-144">On hello **Manage** page of your connected app, go too**Profiles**.</span></span>
2. <span data-ttu-id="d500f-145">Klicka på **hantera profiler**.</span><span class="sxs-lookup"><span data-stu-id="d500f-145">Click **Manage Profiles**.</span></span>
3. <span data-ttu-id="d500f-146">Välj hello profiler (eller grupper av användare) som du vill toofederate med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d500f-146">Select hello profiles (or groups of users) that you want toofederate with Azure AD B2C.</span></span> <span data-ttu-id="d500f-147">Som systemadministratör bör du välja hello **systemadministratören** kryssrutan så att du kan federera med hjälp av ditt Salesforce-konto.</span><span class="sxs-lookup"><span data-stu-id="d500f-147">As a system administrator, select hello **System Administrator** check box, so that you can federate by using your Salesforce account.</span></span>

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a><span data-ttu-id="d500f-148">Generera ett signeringscertifikat för Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="d500f-148">Generate a signing certificate for Azure AD B2C</span></span>

<span data-ttu-id="d500f-149">Begäranden skickade tooSalesforce måste toobe som signerats av Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d500f-149">Requests sent tooSalesforce need toobe signed by Azure AD B2C.</span></span> <span data-ttu-id="d500f-150">toogenerate signeringscertifikatet, öppna Azure PowerShell och kör sedan följande kommandon hello.</span><span class="sxs-lookup"><span data-stu-id="d500f-150">toogenerate a signing certificate, open Azure PowerShell, and then run hello following commands.</span></span>

> [!NOTE]
> <span data-ttu-id="d500f-151">Kontrollera att du uppdaterar hello innehavarens namn och lösenord i hello översta två rader.</span><span class="sxs-lookup"><span data-stu-id="d500f-151">Make sure that you update hello tenant name and password in hello top two lines.</span></span>

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-hello-saml-signing-certificate-tooazure-ad-b2c"></a><span data-ttu-id="d500f-152">Lägg till hello SAML signering certifikat tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="d500f-152">Add hello SAML signing certificate tooAzure AD B2C</span></span>

<span data-ttu-id="d500f-153">Överför hello signering certifikat tooyour Azure AD B2C-klient:</span><span class="sxs-lookup"><span data-stu-id="d500f-153">Upload hello signing certificate tooyour Azure AD B2C tenant:</span></span> 

1. <span data-ttu-id="d500f-154">Gå tooyour Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="d500f-154">Go tooyour Azure AD B2C tenant.</span></span> <span data-ttu-id="d500f-155">Klicka på **inställningar** > **identitet upplevelse Framework** > **princip nycklar**.</span><span class="sxs-lookup"><span data-stu-id="d500f-155">Click **Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
2. <span data-ttu-id="d500f-156">Klicka på **+ Lägg till**, och sedan:</span><span class="sxs-lookup"><span data-stu-id="d500f-156">Click **+Add**, and then:</span></span>
    1. <span data-ttu-id="d500f-157">Klicka på **alternativ** > **överför**.</span><span class="sxs-lookup"><span data-stu-id="d500f-157">Click **Options** > **Upload**.</span></span>
    2. <span data-ttu-id="d500f-158">Ange en **namn** (till exempel SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="d500f-158">Enter a **Name** (for example, SAMLSigningCert).</span></span> <span data-ttu-id="d500f-159">hello prefixet *B2C_1A_* läggs till automatiskt toohello namnet på din nyckel.</span><span class="sxs-lookup"><span data-stu-id="d500f-159">hello prefix *B2C_1A_* is automatically added toohello name of your key.</span></span>
    3. <span data-ttu-id="d500f-160">tooselect ditt certifikat väljer **överför filkontroll**.</span><span class="sxs-lookup"><span data-stu-id="d500f-160">tooselect your certificate, select **upload file control**.</span></span> 
    4. <span data-ttu-id="d500f-161">Ange hello certifikatets lösenord som du anger i hello PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="d500f-161">Enter hello certificate's password that you set in hello PowerShell script.</span></span>
3. <span data-ttu-id="d500f-162">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d500f-162">Click **Create**.</span></span>
4. <span data-ttu-id="d500f-163">Kontrollera att du har skapat en nyckel (till exempel B2C_1A_SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="d500f-163">Verify that you've created a key (for example, B2C_1A_SAMLSigningCert).</span></span> <span data-ttu-id="d500f-164">Anteckna hello fullständigt namn (inklusive *B2C_1A_*).</span><span class="sxs-lookup"><span data-stu-id="d500f-164">Take note of hello full name (including *B2C_1A_*).</span></span> <span data-ttu-id="d500f-165">Du kommer att referera toothis nyckeln senare i hello princip.</span><span class="sxs-lookup"><span data-stu-id="d500f-165">You will refer toothis key later in hello policy.</span></span>

## <a name="create-hello-salesforce-saml-claims-provider-in-your-base-policy"></a><span data-ttu-id="d500f-166">Skapa hello Salesforce SAML anspråksprovider i din grundläggande princip</span><span class="sxs-lookup"><span data-stu-id="d500f-166">Create hello Salesforce SAML claims provider in your base policy</span></span>

<span data-ttu-id="d500f-167">Du måste toodefine Salesforce som en anspråksprovider så att användarna kan logga in med hjälp av Salesforce.</span><span class="sxs-lookup"><span data-stu-id="d500f-167">You need toodefine Salesforce as a claims provider so users can sign in by using Salesforce.</span></span> <span data-ttu-id="d500f-168">Du behöver med andra ord toospecify hello slutpunkt som Azure AD B2C kommer att kommunicera med.</span><span class="sxs-lookup"><span data-stu-id="d500f-168">In other words, you need toospecify hello endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="d500f-169">hello endpoint kommer *ange* en uppsättning *anspråk* att Azure AD B2C använder tooverify som en specifik användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="d500f-169">hello endpoint will *provide* a set of *claims* that Azure AD B2C uses tooverify that a specific user has authenticated.</span></span> <span data-ttu-id="d500f-170">toodo detta, Lägg till en `<ClaimsProvider>` för Salesforce i hello tilläggsfilen i principen:</span><span class="sxs-lookup"><span data-stu-id="d500f-170">toodo this, add a `<ClaimsProvider>` for Salesforce in hello extension file of your policy:</span></span>

1. <span data-ttu-id="d500f-171">Öppna filen hello-tillägg (TrustFrameworkExtensions.xml) i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="d500f-171">In your working directory, open hello extension file (TrustFrameworkExtensions.xml).</span></span>
2. <span data-ttu-id="d500f-172">Hitta hello `<ClaimsProviders>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d500f-172">Find hello `<ClaimsProviders>` section.</span></span> <span data-ttu-id="d500f-173">Om det inte finns kan du skapa det under hello rotnoden.</span><span class="sxs-lookup"><span data-stu-id="d500f-173">If it does not exist, create it under hello root node.</span></span>
3. <span data-ttu-id="d500f-174">Lägg till en ny `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="d500f-174">Add a new `<ClaimsProvider>`:</span></span>

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

<span data-ttu-id="d500f-175">Under hello `<ClaimsProvider>` nod:</span><span class="sxs-lookup"><span data-stu-id="d500f-175">Under hello `<ClaimsProvider>` node:</span></span>

1. <span data-ttu-id="d500f-176">Ändra hello för `<Domain>` tooa unikt värde som särskiljer `<ClaimsProvider>` från andra identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="d500f-176">Change hello value for `<Domain>` tooa unique value that distinguishes `<ClaimsProvider>` from other identity providers.</span></span>
2. <span data-ttu-id="d500f-177">Uppdatera hello värdet för `<DisplayName>` tooa visningsnamn för hello anspråk providern.</span><span class="sxs-lookup"><span data-stu-id="d500f-177">Update hello value for `<DisplayName>` tooa display name for hello claims provider.</span></span> <span data-ttu-id="d500f-178">Det här värdet används för närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="d500f-178">Currently, this value is not used.</span></span>

### <a name="update-hello-technical-profile"></a><span data-ttu-id="d500f-179">Uppdatera hello tekniska profil</span><span class="sxs-lookup"><span data-stu-id="d500f-179">Update hello technical profile</span></span>

<span data-ttu-id="d500f-180">tooget en SAML-token från Salesforce, definiera hello protokoll att Azure AD B2C kommer att använda toocommunicate med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d500f-180">tooget a SAML token from Salesforce, define hello protocols that Azure AD B2C will use toocommunicate with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="d500f-181">Gör detta i hello `<TechnicalProfile>` element av `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="d500f-181">Do this in hello `<TechnicalProfile>` element of `<ClaimsProvider>`:</span></span>

1. <span data-ttu-id="d500f-182">Uppdatera hello-ID för hello `<TechnicalProfile>` nod.</span><span class="sxs-lookup"><span data-stu-id="d500f-182">Update hello ID of hello `<TechnicalProfile>` node.</span></span> <span data-ttu-id="d500f-183">Detta ID är används toorefer toothis tekniska profil från andra delar av hello princip.</span><span class="sxs-lookup"><span data-stu-id="d500f-183">This ID is used toorefer toothis technical profile from other parts of hello policy.</span></span>
2. <span data-ttu-id="d500f-184">Uppdatera hello värde för `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="d500f-184">Update hello value for `<DisplayName>`.</span></span> <span data-ttu-id="d500f-185">Det här värdet visas på hello-knappen Logga in på sidan logga in.</span><span class="sxs-lookup"><span data-stu-id="d500f-185">This value is displayed on hello sign-in button on your sign-in page.</span></span>
3. <span data-ttu-id="d500f-186">Uppdatera hello värde för `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="d500f-186">Update hello value for `<Description>`.</span></span>
4. <span data-ttu-id="d500f-187">Salesforce använder hello SAML 2.0-protokollet.</span><span class="sxs-lookup"><span data-stu-id="d500f-187">Salesforce uses hello SAML 2.0 protocol.</span></span> <span data-ttu-id="d500f-188">Se till att hello-värdet för `<Protocol>` är **SAML2**.</span><span class="sxs-lookup"><span data-stu-id="d500f-188">Ensure that hello value for `<Protocol>` is **SAML2**.</span></span>

<span data-ttu-id="d500f-189">Uppdatera hello `<Metadata>` avsnitt i hello före XML-tooreflect hello inställningar för ditt specifika Salesforce-konto.</span><span class="sxs-lookup"><span data-stu-id="d500f-189">Update hello `<Metadata>` section in hello preceding XML tooreflect hello settings for your specific Salesforce account.</span></span> <span data-ttu-id="d500f-190">Uppdatera hello metadatavärden i hello XML:</span><span class="sxs-lookup"><span data-stu-id="d500f-190">In hello XML, update hello metadata values:</span></span>

1. <span data-ttu-id="d500f-191">Uppdatera hello värdet för `<Item Key="PartnerEntity">` med hello Salesforce metadata-URL som du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="d500f-191">Update hello value of `<Item Key="PartnerEntity">` with hello Salesforce metadata URL you copied earlier.</span></span> <span data-ttu-id="d500f-192">Det har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="d500f-192">It has hello following format:</span></span> 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. <span data-ttu-id="d500f-193">I hello `<CryptographicKeys>` avsnittet hello värde för båda instanser av `StorageReferenceId` toohello namnet på hello nyckel för signeringscertifikatet (till exempel B2C_1A_SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="d500f-193">In hello `<CryptographicKeys>` section, update hello value for both instances of `StorageReferenceId` toohello name of hello key of your signing certificate (for example, B2C_1A_SAMLSigningCert).</span></span>

### <a name="upload-hello-extension-file-for-verification"></a><span data-ttu-id="d500f-194">Överför hello tilläggsfilen för verifiering</span><span class="sxs-lookup"><span data-stu-id="d500f-194">Upload hello extension file for verification</span></span>

<span data-ttu-id="d500f-195">Principen har nu konfigurerats så att Azure AD B2C vet hur toocommunicate med Salesforce.</span><span class="sxs-lookup"><span data-stu-id="d500f-195">Your policy is now configured so that Azure AD B2C knows how toocommunicate with Salesforce.</span></span> <span data-ttu-id="d500f-196">Försök att ladda upp hello tilläggsfilen i principen, tooverify att det inte finns några problem hittills.</span><span class="sxs-lookup"><span data-stu-id="d500f-196">Try uploading hello extension file of your policy, tooverify that there aren't any issues so far.</span></span> <span data-ttu-id="d500f-197">tooupload hello tilläggsfilen i principen:</span><span class="sxs-lookup"><span data-stu-id="d500f-197">tooupload hello extension file of your policy:</span></span>

1. <span data-ttu-id="d500f-198">I din Azure AD B2C-klient går toohello **alla principer** bladet.</span><span class="sxs-lookup"><span data-stu-id="d500f-198">In your Azure AD B2C tenant, go toohello **All Policies** blade.</span></span>
2. <span data-ttu-id="d500f-199">Välj hello **skriva över hello principen om den finns** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="d500f-199">Select hello **Overwrite hello policy if it exists** check box.</span></span>
3. <span data-ttu-id="d500f-200">Ladda upp filen hello-tillägg (TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="d500f-200">Upload hello extension file (TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="d500f-201">Se till att den inte inte kan valideras.</span><span class="sxs-lookup"><span data-stu-id="d500f-201">Ensure that it doesn't fail validation.</span></span>

## <a name="register-hello-salesforce-saml-claims-provider-tooa-user-journey"></a><span data-ttu-id="d500f-202">Registrera hello Salesforce SAML anspråk providern tooa användaren resa</span><span class="sxs-lookup"><span data-stu-id="d500f-202">Register hello Salesforce SAML claims provider tooa user journey</span></span>

<span data-ttu-id="d500f-203">Lägg till hello Salesforce SAML identitet providern tooone resor dina användare.</span><span class="sxs-lookup"><span data-stu-id="d500f-203">Next, add hello Salesforce SAML identity provider tooone of your user journeys.</span></span> <span data-ttu-id="d500f-204">Nu hello identitetsleverantören har ställts in, men den är inte tillgänglig på någon av hello användaren registrering eller inloggning sidor.</span><span class="sxs-lookup"><span data-stu-id="d500f-204">At this point, hello identity provider has been set up, but it’s not available on any of hello user sign-up or sign-in pages.</span></span> <span data-ttu-id="d500f-205">tooadd hello identitet providern tooa inloggningssidan, först skapar en dubblett av en befintlig mall användaren resa.</span><span class="sxs-lookup"><span data-stu-id="d500f-205">tooadd hello identity provider tooa sign-in page, first, create a duplicate of an existing template user journey.</span></span> <span data-ttu-id="d500f-206">Ändra hello mallen så att den har även hello Azure AD-identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="d500f-206">Then, modify hello template so that it also has hello Azure AD identity provider.</span></span>

1. <span data-ttu-id="d500f-207">Öppna grundläggande hello-filen för principen (till exempel TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="d500f-207">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2. <span data-ttu-id="d500f-208">Hitta hello `<UserJourneys>` element, och sedan kopiera hello hela `<UserJourney>` värde, inklusive Id = ”SignUpOrSignIn”.</span><span class="sxs-lookup"><span data-stu-id="d500f-208">Find hello `<UserJourneys>` element, and then copy hello entire `<UserJourney>` value, including Id=”SignUpOrSignIn”.</span></span>
3. <span data-ttu-id="d500f-209">Öppna filen hello-tillägg (till exempel TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="d500f-209">Open hello extension file (for example, TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="d500f-210">Hitta hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="d500f-210">Find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="d500f-211">Skapa en om hello elementet inte finns.</span><span class="sxs-lookup"><span data-stu-id="d500f-211">If hello element doesn't exist, create one.</span></span>
4. <span data-ttu-id="d500f-212">Klistra in hello hela kopieras `<UserJourney>` som underordnad till hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="d500f-212">Paste hello entire copied `<UserJourney>` as a child of hello `<UserJourneys>` element.</span></span>
5. <span data-ttu-id="d500f-213">Byt namn på hello-ID för nya hello `<UserJourney>` (till exempel SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="d500f-213">Rename hello ID of hello new `<UserJourney>` (for example, SignUpOrSignUsingContoso).</span></span>

### <a name="display-hello-identity-provider-button"></a><span data-ttu-id="d500f-214">Visa hello identitet providern knappen</span><span class="sxs-lookup"><span data-stu-id="d500f-214">Display hello identity provider button</span></span>

<span data-ttu-id="d500f-215">Hej `<ClaimsProviderSelection>` elementet är detsamma tooan identitet provider-knappen i ett registrering eller inloggning.</span><span class="sxs-lookup"><span data-stu-id="d500f-215">hello `<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up or sign-in page.</span></span> <span data-ttu-id="d500f-216">Genom att lägga till en `<ClaimsProviderSelection>` element för Salesforce nya knappen visas när en användare toothis sidan.</span><span class="sxs-lookup"><span data-stu-id="d500f-216">By adding an `<ClaimsProviderSelection>` element for Salesforce, a new button appears when a user goes toothis page.</span></span> <span data-ttu-id="d500f-217">knapp för toodisplay hello identitet providern:</span><span class="sxs-lookup"><span data-stu-id="d500f-217">toodisplay hello identity provider button:</span></span>

1. <span data-ttu-id="d500f-218">I hello `<UserJourney>` som du har skapat, hitta hello `<OrchestrationStep>` med `Order="1"`.</span><span class="sxs-lookup"><span data-stu-id="d500f-218">In hello `<UserJourney>` that you created, find hello `<OrchestrationStep>` with `Order="1"`.</span></span>
2. <span data-ttu-id="d500f-219">Lägg till följande XML hello:</span><span class="sxs-lookup"><span data-stu-id="d500f-219">Add hello following XML:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. <span data-ttu-id="d500f-220">Ange `TargetClaimsExchangeId` tooa logiskt värde.</span><span class="sxs-lookup"><span data-stu-id="d500f-220">Set `TargetClaimsExchangeId` tooa logical value.</span></span> <span data-ttu-id="d500f-221">Vi rekommenderar följande hello samma konvention som andra (exempelvis  *\[ClaimProviderName\]Exchange*).</span><span class="sxs-lookup"><span data-stu-id="d500f-221">We recommend following hello same convention as others (for example, *\[ClaimProviderName\]Exchange*).</span></span>

### <a name="link-hello-identity-provider-button-tooan-action"></a><span data-ttu-id="d500f-222">Länka hello identitet providern tooan åtgärd</span><span class="sxs-lookup"><span data-stu-id="d500f-222">Link hello identity provider button tooan action</span></span>

<span data-ttu-id="d500f-223">Nu när du har en identity-providern knapp för länka tooan åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d500f-223">Now that you have an identity provider button in place, link it tooan action.</span></span> <span data-ttu-id="d500f-224">I det här fallet är hello åtgärd för Azure AD B2C toocommunicate med Salesforce tooreceive en SAML-token.</span><span class="sxs-lookup"><span data-stu-id="d500f-224">In this case, hello action is for Azure AD B2C toocommunicate with Salesforce tooreceive a SAML token.</span></span> <span data-ttu-id="d500f-225">Du kan göra detta genom att länka tekniska hello-profil för dina Salesforce SAML anspråksleverantör:</span><span class="sxs-lookup"><span data-stu-id="d500f-225">You can do this by linking hello technical profile for your Salesforce SAML claims provider:</span></span>

1. <span data-ttu-id="d500f-226">I hello `<UserJourney>` nod, hitta hello `<OrchestrationStep>` med `Order="2"`.</span><span class="sxs-lookup"><span data-stu-id="d500f-226">In hello `<UserJourney>` node, find hello `<OrchestrationStep>` with `Order="2"`.</span></span>
2. <span data-ttu-id="d500f-227">Lägg till följande XML hello:</span><span class="sxs-lookup"><span data-stu-id="d500f-227">Add hello following XML:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. <span data-ttu-id="d500f-228">Uppdatera `Id` toohello samma värde som du använde tidigare i `TargetClaimsExchangeId`.</span><span class="sxs-lookup"><span data-stu-id="d500f-228">Update `Id` toohello same value that you used earlier for `TargetClaimsExchangeId`.</span></span>
4. <span data-ttu-id="d500f-229">Uppdatera `TechnicalProfileReferenceId` toohello `Id` av hello tekniska profilen du skapade tidigare (till exempel ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="d500f-229">Update `TechnicalProfileReferenceId` toohello `Id` of hello technical profile you created earlier (for example, ContosoProfile).</span></span>

### <a name="upload-hello-updated-extension-file"></a><span data-ttu-id="d500f-230">Överför hello uppdaterade tilläggsfilen</span><span class="sxs-lookup"><span data-stu-id="d500f-230">Upload hello updated extension file</span></span>

<span data-ttu-id="d500f-231">Du är klar ändra hello-tilläggsfilen.</span><span class="sxs-lookup"><span data-stu-id="d500f-231">You are done modifying hello extension file.</span></span> <span data-ttu-id="d500f-232">Spara och ladda upp den här filen.</span><span class="sxs-lookup"><span data-stu-id="d500f-232">Save and upload this file.</span></span> <span data-ttu-id="d500f-233">Se till att alla verifieringar lyckas.</span><span class="sxs-lookup"><span data-stu-id="d500f-233">Ensure that all validations succeed.</span></span>

### <a name="update-hello-relying-party-file"></a><span data-ttu-id="d500f-234">Uppdatera hello förlitande part-filen</span><span class="sxs-lookup"><span data-stu-id="d500f-234">Update hello relying party file</span></span>

<span data-ttu-id="d500f-235">Därefter uppdaterar hello förlitande part (RP)-fil som initierar hello användaren resa som du skapade:</span><span class="sxs-lookup"><span data-stu-id="d500f-235">Next, update hello relying party (RP) file that initiates hello user journey that you created:</span></span>

1. <span data-ttu-id="d500f-236">Gör en kopia av SignUpOrSignIn.xml i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="d500f-236">Make a copy of SignUpOrSignIn.xml in your working directory.</span></span> <span data-ttu-id="d500f-237">Sedan, byta namn på den (till exempel SignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="d500f-237">Then, rename it (for example, SignUpOrSignInWithAAD.xml).</span></span>
2. <span data-ttu-id="d500f-238">Öppna hello nya fil- och update hello `PolicyId` attribut för `<TrustFrameworkPolicy>` med ett unikt värde.</span><span class="sxs-lookup"><span data-stu-id="d500f-238">Open hello new file and update hello `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value.</span></span> <span data-ttu-id="d500f-239">Detta är hello namn för principen (till exempel SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="d500f-239">This is hello name of your policy (for example, SignUpOrSignInWithAAD).</span></span>
3. <span data-ttu-id="d500f-240">Ändra hello `ReferenceId` attribut i `<DefaultUserJourney>` toomatch hello `Id` av hello nya användare resa som du skapade (till exempel SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="d500f-240">Modify hello `ReferenceId` attribute in `<DefaultUserJourney>` toomatch hello `Id` of hello new user journey that you created (for example, SignUpOrSignUsingContoso).</span></span>
4. <span data-ttu-id="d500f-241">Spara ändringarna och sedan överföra hello-fil.</span><span class="sxs-lookup"><span data-stu-id="d500f-241">Save your changes, and then upload hello file.</span></span>

## <a name="test-and-troubleshoot"></a><span data-ttu-id="d500f-242">Testa och felsöka</span><span class="sxs-lookup"><span data-stu-id="d500f-242">Test and troubleshoot</span></span>

<span data-ttu-id="d500f-243">tootest hello anpassad princip som du just har överfört, i hello Azure-portalen går toohello principbladet och klicka sedan på **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="d500f-243">tootest hello custom policy that you just uploaded, in hello Azure portal, go toohello policy blade, and then click **Run now**.</span></span> <span data-ttu-id="d500f-244">Om den inte finns [felsöka principer för anpassade](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d500f-244">If it fails, see [Troubleshoot custom policies](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d500f-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d500f-245">Next steps</span></span>

<span data-ttu-id="d500f-246">Ge feedback för[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="d500f-246">Provide feedback too[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>

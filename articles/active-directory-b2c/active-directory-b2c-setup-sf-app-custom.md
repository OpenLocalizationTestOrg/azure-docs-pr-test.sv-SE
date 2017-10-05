---
title: "Azure Active Directory B2C: Lägga till en Salesforce SAML-provider med hjälp av anpassade principer | Microsoft Docs"
description: "Lär dig mer om hur du skapar och hanterar Azure Active Directory B2C anpassade principer."
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
ms.openlocfilehash: 269cbd80fb6e861fa8588025eec70b6c6e2890d7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a><span data-ttu-id="9c2d8-103">Azure Active Directory B2C: Logga in med hjälp av Salesforce konton via SAML</span><span class="sxs-lookup"><span data-stu-id="9c2d8-103">Azure Active Directory B2C: Sign in by using Salesforce accounts via SAML</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="9c2d8-104">Den här artikeln visar hur du använder [anpassade principer](active-directory-b2c-overview-custom.md) att ställa in inloggning för användare från en viss Salesforce-organisation.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-104">This article shows you how to use [custom policies](active-directory-b2c-overview-custom.md) to set up sign-in for users from a specific Salesforce organization.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c2d8-105">Krav</span><span class="sxs-lookup"><span data-stu-id="9c2d8-105">Prerequisites</span></span>

### <a name="azure-ad-b2c-setup"></a><span data-ttu-id="9c2d8-106">Installationsprogram för Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9c2d8-106">Azure AD B2C setup</span></span>

<span data-ttu-id="9c2d8-107">Kontrollera att du har slutfört alla steg som visar hur till [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) i Azure Active Directory B2C (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-107">Ensure that you have completed all of the steps that show you how to [get started with custom policies](active-directory-b2c-get-started-custom.md) in Azure Active Directory B2C (Azure AD B2C).</span></span>

<span data-ttu-id="9c2d8-108">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-108">These include:</span></span>

* <span data-ttu-id="9c2d8-109">Skapa en Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-109">Create an Azure AD B2C tenant.</span></span>
* <span data-ttu-id="9c2d8-110">Skapa ett program med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-110">Create an Azure AD B2C application.</span></span>
* <span data-ttu-id="9c2d8-111">Registrera två motorn för program.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-111">Register two policy engine applications.</span></span>
* <span data-ttu-id="9c2d8-112">Ställ in nycklar.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-112">Set up keys.</span></span>
* <span data-ttu-id="9c2d8-113">Ställ in startpaket.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-113">Set up the starter pack.</span></span>

### <a name="salesforce-setup"></a><span data-ttu-id="9c2d8-114">Salesforce-installationen</span><span class="sxs-lookup"><span data-stu-id="9c2d8-114">Salesforce setup</span></span>

<span data-ttu-id="9c2d8-115">I den här artikeln förutsätter vi att du redan har gjort följande:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-115">In this article, we assume that you have already done the following:</span></span>

* <span data-ttu-id="9c2d8-116">Registrerat dig för en Salesforce-konto.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-116">Signed up for a Salesforce account.</span></span> <span data-ttu-id="9c2d8-117">Du kan registrera dig för en [kostnadsfritt konto Developer Edition](https://developer.salesforce.com/signup).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-117">You can sign up for a [free Developer Edition account](https://developer.salesforce.com/signup).</span></span>
* <span data-ttu-id="9c2d8-118">[Konfigurera min domän](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) för organisationen Salesforce.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-118">[Set up a My Domain](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) for your Salesforce organization.</span></span>

## <a name="set-up-salesforce-so-users-can-federate"></a><span data-ttu-id="9c2d8-119">Konfigurera Salesforce så att användare kan federera</span><span class="sxs-lookup"><span data-stu-id="9c2d8-119">Set up Salesforce so users can federate</span></span>

<span data-ttu-id="9c2d8-120">Du måste hämta metadata Salesforce-URL för att Azure AD B2C kommunicera med Salesforce.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-120">To help Azure AD B2C communicate with Salesforce, you need to get the Salesforce metadata URL.</span></span>

### <a name="set-up-salesforce-as-an-identity-provider"></a><span data-ttu-id="9c2d8-121">Ställ in Salesforce som en identitetsleverantör</span><span class="sxs-lookup"><span data-stu-id="9c2d8-121">Set up Salesforce as an identity provider</span></span>

> [!NOTE]
> <span data-ttu-id="9c2d8-122">I den här artikeln förutsätter vi att du använder [Salesforce blixtsnabb upplevelse](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-122">In this article, we assume you are using [Salesforce Lightning Experience](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span></span>

1. <span data-ttu-id="9c2d8-123">[Logga in till Salesforce](https://login.salesforce.com/).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-123">[Sign in to Salesforce](https://login.salesforce.com/).</span></span> 
2. <span data-ttu-id="9c2d8-124">På den vänstra menyn under **inställningar**, expandera **identitet**, och klicka sedan på **identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-124">On the left menu, under **Settings**, expand **Identity**, and then click **Identity Provider**.</span></span>
3. <span data-ttu-id="9c2d8-125">Klicka på **aktivera identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-125">Click **Enable Identity Provider**.</span></span>
4. <span data-ttu-id="9c2d8-126">Under **Välj certifikatet**, Välj det certifikat du vill Salesforce som du använder för att kommunicera med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-126">Under **Select the certificate**, select the certificate you want Salesforce to use to communicate with Azure AD B2C.</span></span> <span data-ttu-id="9c2d8-127">(Du kan använda standardcertifikatet.) Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-127">(You can use the default certificate.) Click **Save**.</span></span> 

### <a name="create-a-connected-app-in-salesforce"></a><span data-ttu-id="9c2d8-128">Skapa en ansluten app i Salesforce</span><span class="sxs-lookup"><span data-stu-id="9c2d8-128">Create a connected app in Salesforce</span></span>

1. <span data-ttu-id="9c2d8-129">På den **identitetsleverantör** går du till **leverantörer**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-129">On the **Identity Provider** page, go to **Service Providers**.</span></span>
2. <span data-ttu-id="9c2d8-130">Klicka på **leverantörer skapas nu via anslutna appar. Klicka här.**</span><span class="sxs-lookup"><span data-stu-id="9c2d8-130">Click **Service Providers are now created via Connected Apps. Click here.**</span></span>
3. <span data-ttu-id="9c2d8-131">Under **grundläggande Information**, ange obligatoriska värden för anslutna appen.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-131">Under **Basic Information**,  enter the required values for your connected app.</span></span>
4. <span data-ttu-id="9c2d8-132">Under **Webbprograminställningarna**, Välj den **aktivera SAML** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-132">Under **Web App Settings**, select the **Enable SAML** check box.</span></span>
5. <span data-ttu-id="9c2d8-133">I den **enhets-ID** , ange följande URL-adress.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-133">In the **Entity ID** field, enter the following URL.</span></span> <span data-ttu-id="9c2d8-134">Se till att ersätta värdet för `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-134">Ensure that you replace the value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. <span data-ttu-id="9c2d8-135">I den **ACS URL** , ange följande URL-adress.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-135">In the **ACS URL** field, enter the following URL.</span></span> <span data-ttu-id="9c2d8-136">Se till att ersätta värdet för `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-136">Ensure that you replace the value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. <span data-ttu-id="9c2d8-137">Lämna standardvärdena för alla andra inställningar.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-137">Leave the default values for all other settings.</span></span>
8. <span data-ttu-id="9c2d8-138">Bläddra längst ned i listan och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-138">Scroll to the bottom of the list, and then click **Save**.</span></span>

### <a name="get-the-metadata-url"></a><span data-ttu-id="9c2d8-139">Hämta metadata-URL</span><span class="sxs-lookup"><span data-stu-id="9c2d8-139">Get the metadata URL</span></span>

1. <span data-ttu-id="9c2d8-140">På översiktssidan för anslutna appen **hantera**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-140">On the overview page of your connected app, click **Manage**.</span></span>
2. <span data-ttu-id="9c2d8-141">Kopiera värdet för **Metadata slutpunktsidentifiering**, och spara den.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-141">Copy the value for **Metadata Discovery Endpoint**, and then save it.</span></span> <span data-ttu-id="9c2d8-142">Du ska använda senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-142">You'll use it later in this article.</span></span>

### <a name="set-up-salesforce-users-to-federate"></a><span data-ttu-id="9c2d8-143">Konfigurera Salesforce användare att federera</span><span class="sxs-lookup"><span data-stu-id="9c2d8-143">Set up Salesforce users to federate</span></span>

1. <span data-ttu-id="9c2d8-144">På den **hantera** sidan av dina anslutna appen, gå till **profiler**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-144">On the **Manage** page of your connected app, go to **Profiles**.</span></span>
2. <span data-ttu-id="9c2d8-145">Klicka på **hantera profiler**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-145">Click **Manage Profiles**.</span></span>
3. <span data-ttu-id="9c2d8-146">Välj profiler (eller grupper av användare) som du vill federera med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-146">Select the profiles (or groups of users) that you want to federate with Azure AD B2C.</span></span> <span data-ttu-id="9c2d8-147">Som systemadministratör bör du välja den **systemadministratören** kryssrutan så att du kan federera med hjälp av ditt Salesforce-konto.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-147">As a system administrator, select the **System Administrator** check box, so that you can federate by using your Salesforce account.</span></span>

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a><span data-ttu-id="9c2d8-148">Generera ett signeringscertifikat för Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9c2d8-148">Generate a signing certificate for Azure AD B2C</span></span>

<span data-ttu-id="9c2d8-149">Förfrågningar som skickas till Salesforce måste signeras av Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-149">Requests sent to Salesforce need to be signed by Azure AD B2C.</span></span> <span data-ttu-id="9c2d8-150">Öppna Azure PowerShell och kör följande kommandon för att generera ett signeringscertifikat.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-150">To generate a signing certificate, open Azure PowerShell, and then run the following commands.</span></span>

> [!NOTE]
> <span data-ttu-id="9c2d8-151">Kontrollera att du uppdaterar innehavarens namn och lösenord i de två översta raderna.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-151">Make sure that you update the tenant name and password in the top two lines.</span></span>

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-the-saml-signing-certificate-to-azure-ad-b2c"></a><span data-ttu-id="9c2d8-152">Lägg till SAML-signeringscertifikat i Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="9c2d8-152">Add the SAML signing certificate to Azure AD B2C</span></span>

<span data-ttu-id="9c2d8-153">Överför signeringscertifikatet till din Azure AD B2C-klient:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-153">Upload the signing certificate to your Azure AD B2C tenant:</span></span> 

1. <span data-ttu-id="9c2d8-154">Gå till din Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-154">Go to your Azure AD B2C tenant.</span></span> <span data-ttu-id="9c2d8-155">Klicka på **inställningar** > **identitet upplevelse Framework** > **princip nycklar**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-155">Click **Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
2. <span data-ttu-id="9c2d8-156">Klicka på **+ Lägg till**, och sedan:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-156">Click **+Add**, and then:</span></span>
    1. <span data-ttu-id="9c2d8-157">Klicka på **alternativ** > **överför**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-157">Click **Options** > **Upload**.</span></span>
    2. <span data-ttu-id="9c2d8-158">Ange en **namn** (till exempel SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-158">Enter a **Name** (for example, SAMLSigningCert).</span></span> <span data-ttu-id="9c2d8-159">Prefixet *B2C_1A_* läggs automatiskt till namnet på din nyckel.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-159">The prefix *B2C_1A_* is automatically added to the name of your key.</span></span>
    3. <span data-ttu-id="9c2d8-160">Om du vill välja certifikatet **överför filkontroll**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-160">To select your certificate, select **upload file control**.</span></span> 
    4. <span data-ttu-id="9c2d8-161">Ange lösenord för certifikatet som du anger i PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-161">Enter the certificate's password that you set in the PowerShell script.</span></span>
3. <span data-ttu-id="9c2d8-162">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-162">Click **Create**.</span></span>
4. <span data-ttu-id="9c2d8-163">Kontrollera att du har skapat en nyckel (till exempel B2C_1A_SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-163">Verify that you've created a key (for example, B2C_1A_SAMLSigningCert).</span></span> <span data-ttu-id="9c2d8-164">Anteckna det fullständiga namnet (inklusive *B2C_1A_*).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-164">Take note of the full name (including *B2C_1A_*).</span></span> <span data-ttu-id="9c2d8-165">Du kommer att referera till den här nyckeln senare i principen.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-165">You will refer to this key later in the policy.</span></span>

## <a name="create-the-salesforce-saml-claims-provider-in-your-base-policy"></a><span data-ttu-id="9c2d8-166">Skapa anspråksprovider Salesforce SAML i din grundläggande princip</span><span class="sxs-lookup"><span data-stu-id="9c2d8-166">Create the Salesforce SAML claims provider in your base policy</span></span>

<span data-ttu-id="9c2d8-167">Du måste definiera Salesforce som en anspråksprovider så att användarna kan logga in med hjälp av Salesforce.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-167">You need to define Salesforce as a claims provider so users can sign in by using Salesforce.</span></span> <span data-ttu-id="9c2d8-168">Med andra ord, måste du ange den slutpunkt som Azure AD B2C kommer att kommunicera med.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-168">In other words, you need to specify the endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="9c2d8-169">Slutpunkten ska *ange* en uppsättning *anspråk* som Azure AD B2C använder för att verifiera att en specifik användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-169">The endpoint will *provide* a set of *claims* that Azure AD B2C uses to verify that a specific user has authenticated.</span></span> <span data-ttu-id="9c2d8-170">Det gör du genom att lägga till en `<ClaimsProvider>` för Salesforce i tilläggsfilen i principen:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-170">To do this, add a `<ClaimsProvider>` for Salesforce in the extension file of your policy:</span></span>

1. <span data-ttu-id="9c2d8-171">Öppna tilläggsfilen (TrustFrameworkExtensions.xml) i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-171">In your working directory, open the extension file (TrustFrameworkExtensions.xml).</span></span>
2. <span data-ttu-id="9c2d8-172">Hitta de `<ClaimsProviders>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-172">Find the `<ClaimsProviders>` section.</span></span> <span data-ttu-id="9c2d8-173">Om det inte finns kan du skapa det under rotnoden.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-173">If it does not exist, create it under the root node.</span></span>
3. <span data-ttu-id="9c2d8-174">Lägg till en ny `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-174">Add a new `<ClaimsProvider>`:</span></span>

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

<span data-ttu-id="9c2d8-175">Under den `<ClaimsProvider>` nod:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-175">Under the `<ClaimsProvider>` node:</span></span>

1. <span data-ttu-id="9c2d8-176">Ändra värdet för `<Domain>` till ett unikt värde som särskiljer `<ClaimsProvider>` från andra identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-176">Change the value for `<Domain>` to a unique value that distinguishes `<ClaimsProvider>` from other identity providers.</span></span>
2. <span data-ttu-id="9c2d8-177">Uppdatera värdet för `<DisplayName>` till ett visningsnamn för anspråksprovidern.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-177">Update the value for `<DisplayName>` to a display name for the claims provider.</span></span> <span data-ttu-id="9c2d8-178">Det här värdet används för närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-178">Currently, this value is not used.</span></span>

### <a name="update-the-technical-profile"></a><span data-ttu-id="9c2d8-179">Uppdatera tekniska profilen</span><span class="sxs-lookup"><span data-stu-id="9c2d8-179">Update the technical profile</span></span>

<span data-ttu-id="9c2d8-180">Om du vill hämta en SAML-token från Salesforce, definiera de protokoll som använder Azure AD B2C för att kommunicera med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-180">To get a SAML token from Salesforce, define the protocols that Azure AD B2C will use to communicate with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="9c2d8-181">Gör detta i den `<TechnicalProfile>` element av `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-181">Do this in the `<TechnicalProfile>` element of `<ClaimsProvider>`:</span></span>

1. <span data-ttu-id="9c2d8-182">Uppdatera ID för den `<TechnicalProfile>` nod.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-182">Update the ID of the `<TechnicalProfile>` node.</span></span> <span data-ttu-id="9c2d8-183">Detta ID används för att referera till den här tekniska profilen från andra delar av principen.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-183">This ID is used to refer to this technical profile from other parts of the policy.</span></span>
2. <span data-ttu-id="9c2d8-184">Uppdatera värdet för `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-184">Update the value for `<DisplayName>`.</span></span> <span data-ttu-id="9c2d8-185">Det här värdet visas på knappen Logga in på sidan logga in.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-185">This value is displayed on the sign-in button on your sign-in page.</span></span>
3. <span data-ttu-id="9c2d8-186">Uppdatera värdet för `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-186">Update the value for `<Description>`.</span></span>
4. <span data-ttu-id="9c2d8-187">Salesforce använder SAML 2.0-protokollet.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-187">Salesforce uses the SAML 2.0 protocol.</span></span> <span data-ttu-id="9c2d8-188">Kontrollera att värdet för `<Protocol>` är **SAML2**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-188">Ensure that the value for `<Protocol>` is **SAML2**.</span></span>

<span data-ttu-id="9c2d8-189">Uppdatering av `<Metadata>` avsnitt i föregående XML att visa inställningarna för ditt specifika Salesforce-konto.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-189">Update the `<Metadata>` section in the preceding XML to reflect the settings for your specific Salesforce account.</span></span> <span data-ttu-id="9c2d8-190">Uppdatera värdena metadata i XML:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-190">In the XML, update the metadata values:</span></span>

1. <span data-ttu-id="9c2d8-191">Uppdatera värdet i `<Item Key="PartnerEntity">` med Salesforce-metadata-URL som du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-191">Update the value of `<Item Key="PartnerEntity">` with the Salesforce metadata URL you copied earlier.</span></span> <span data-ttu-id="9c2d8-192">Det har följande format:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-192">It has the following format:</span></span> 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. <span data-ttu-id="9c2d8-193">I den `<CryptographicKeys>` och uppdatera värdet för båda instanser av `StorageReferenceId` till namnet på nyckeln i ditt signeringscertifikat (till exempel B2C_1A_SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-193">In the `<CryptographicKeys>` section, update the value for both instances of `StorageReferenceId` to the name of the key of your signing certificate (for example, B2C_1A_SAMLSigningCert).</span></span>

### <a name="upload-the-extension-file-for-verification"></a><span data-ttu-id="9c2d8-194">Ladda upp tilläggsfilen för verifiering</span><span class="sxs-lookup"><span data-stu-id="9c2d8-194">Upload the extension file for verification</span></span>

<span data-ttu-id="9c2d8-195">Principen har nu konfigurerats så att Azure AD B2C vet hur att kommunicera med Salesforce.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-195">Your policy is now configured so that Azure AD B2C knows how to communicate with Salesforce.</span></span> <span data-ttu-id="9c2d8-196">Försök att ladda upp tilläggsfilen av din princip för att kontrollera att det inte finns några problem hittills.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-196">Try uploading the extension file of your policy, to verify that there aren't any issues so far.</span></span> <span data-ttu-id="9c2d8-197">Att överföra tilläggsfilen i principen:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-197">To upload the extension file of your policy:</span></span>

1. <span data-ttu-id="9c2d8-198">I din Azure AD B2C-klient går du till den **alla principer** bladet.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-198">In your Azure AD B2C tenant, go to the **All Policies** blade.</span></span>
2. <span data-ttu-id="9c2d8-199">Välj den **skriva över principen om den finns** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-199">Select the **Overwrite the policy if it exists** check box.</span></span>
3. <span data-ttu-id="9c2d8-200">Ladda upp fil (TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-200">Upload the extension file (TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="9c2d8-201">Se till att den inte inte kan valideras.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-201">Ensure that it doesn't fail validation.</span></span>

## <a name="register-the-salesforce-saml-claims-provider-to-a-user-journey"></a><span data-ttu-id="9c2d8-202">Registrera Salesforce SAML anspråksprovidern för en användare resa</span><span class="sxs-lookup"><span data-stu-id="9c2d8-202">Register the Salesforce SAML claims provider to a user journey</span></span>

<span data-ttu-id="9c2d8-203">Lägg sedan till Salesforce SAML-identitetsprovider till någon av dina användare resor.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-203">Next, add the Salesforce SAML identity provider to one of your user journeys.</span></span> <span data-ttu-id="9c2d8-204">Nu identitetsleverantören har ställts in, men den är inte tillgänglig på någon av sidorna användaren registrering eller inloggning.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-204">At this point, the identity provider has been set up, but it’s not available on any of the user sign-up or sign-in pages.</span></span> <span data-ttu-id="9c2d8-205">Om du vill lägga till identitetsleverantören till en inloggningssida, först skapa en dubblett av en befintlig mall användaren resa.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-205">To add the identity provider to a sign-in page, first, create a duplicate of an existing template user journey.</span></span> <span data-ttu-id="9c2d8-206">Ändra sedan mallen så att den har även Azure AD-identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-206">Then, modify the template so that it also has the Azure AD identity provider.</span></span>

1. <span data-ttu-id="9c2d8-207">Öppna filen grundläggande av principen (till exempel TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-207">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2. <span data-ttu-id="9c2d8-208">Hitta de `<UserJourneys>` element, och sedan kopiera hela `<UserJourney>` värde, inklusive Id = ”SignUpOrSignIn”.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-208">Find the `<UserJourneys>` element, and then copy the entire `<UserJourney>` value, including Id=”SignUpOrSignIn”.</span></span>
3. <span data-ttu-id="9c2d8-209">Öppna tilläggsfilen (till exempel TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-209">Open the extension file (for example, TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="9c2d8-210">Hitta de `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-210">Find the `<UserJourneys>` element.</span></span> <span data-ttu-id="9c2d8-211">Om det inte finns elementet, skapa en.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-211">If the element doesn't exist, create one.</span></span>
4. <span data-ttu-id="9c2d8-212">Klistra in hela kopieras `<UserJourney>` som underordnad till den `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-212">Paste the entire copied `<UserJourney>` as a child of the `<UserJourneys>` element.</span></span>
5. <span data-ttu-id="9c2d8-213">Byt namn på ID för den nya `<UserJourney>` (till exempel SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-213">Rename the ID of the new `<UserJourney>` (for example, SignUpOrSignUsingContoso).</span></span>

### <a name="display-the-identity-provider-button"></a><span data-ttu-id="9c2d8-214">Visa knappen för providern identitet</span><span class="sxs-lookup"><span data-stu-id="9c2d8-214">Display the identity provider button</span></span>

<span data-ttu-id="9c2d8-215">Den `<ClaimsProviderSelection>` element är detsamma som knappen identity-providern på en registrering eller inloggning.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-215">The `<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up or sign-in page.</span></span> <span data-ttu-id="9c2d8-216">Genom att lägga till en `<ClaimsProviderSelection>` element för Salesforce nya knappen visas när en användare till den här sidan.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-216">By adding an `<ClaimsProviderSelection>` element for Salesforce, a new button appears when a user goes to this page.</span></span> <span data-ttu-id="9c2d8-217">Knappen ska visas identitet providern:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-217">To display the identity provider button:</span></span>

1. <span data-ttu-id="9c2d8-218">I den `<UserJourney>` som du har skapat, söka efter den `<OrchestrationStep>` med `Order="1"`.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-218">In the `<UserJourney>` that you created, find the `<OrchestrationStep>` with `Order="1"`.</span></span>
2. <span data-ttu-id="9c2d8-219">Lägg till följande XML-filen:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-219">Add the following XML:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. <span data-ttu-id="9c2d8-220">Ange `TargetClaimsExchangeId` till ett logiskt värde.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-220">Set `TargetClaimsExchangeId` to a logical value.</span></span> <span data-ttu-id="9c2d8-221">Vi rekommenderar följande samma regler som andra (exempelvis  *\[ClaimProviderName\]Exchange*).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-221">We recommend following the same convention as others (for example, *\[ClaimProviderName\]Exchange*).</span></span>

### <a name="link-the-identity-provider-button-to-an-action"></a><span data-ttu-id="9c2d8-222">Länka knappen leverantör identitet till en åtgärd</span><span class="sxs-lookup"><span data-stu-id="9c2d8-222">Link the identity provider button to an action</span></span>

<span data-ttu-id="9c2d8-223">Nu när du har en identity-providern knapp på plats kan du länka den till en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-223">Now that you have an identity provider button in place, link it to an action.</span></span> <span data-ttu-id="9c2d8-224">I det här fallet är åtgärden för Azure AD B2C att kommunicera med Salesforce ta emot en SAML-token.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-224">In this case, the action is for Azure AD B2C to communicate with Salesforce to receive a SAML token.</span></span> <span data-ttu-id="9c2d8-225">Du kan göra detta genom att länka tekniska profilen för dina Salesforce SAML anspråksleverantör:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-225">You can do this by linking the technical profile for your Salesforce SAML claims provider:</span></span>

1. <span data-ttu-id="9c2d8-226">I den `<UserJourney>` kan hitta den `<OrchestrationStep>` med `Order="2"`.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-226">In the `<UserJourney>` node, find the `<OrchestrationStep>` with `Order="2"`.</span></span>
2. <span data-ttu-id="9c2d8-227">Lägg till följande XML-filen:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-227">Add the following XML:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. <span data-ttu-id="9c2d8-228">Uppdatera `Id` till samma värde som du använde tidigare för `TargetClaimsExchangeId`.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-228">Update `Id` to the same value that you used earlier for `TargetClaimsExchangeId`.</span></span>
4. <span data-ttu-id="9c2d8-229">Uppdatera `TechnicalProfileReferenceId` till den `Id` av tekniska profilen du skapade tidigare (till exempel ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-229">Update `TechnicalProfileReferenceId` to the `Id` of the technical profile you created earlier (for example, ContosoProfile).</span></span>

### <a name="upload-the-updated-extension-file"></a><span data-ttu-id="9c2d8-230">Ladda upp filen uppdaterade tillägg</span><span class="sxs-lookup"><span data-stu-id="9c2d8-230">Upload the updated extension file</span></span>

<span data-ttu-id="9c2d8-231">Du är klar ändra tilläggsfilen.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-231">You are done modifying the extension file.</span></span> <span data-ttu-id="9c2d8-232">Spara och ladda upp den här filen.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-232">Save and upload this file.</span></span> <span data-ttu-id="9c2d8-233">Se till att alla verifieringar lyckas.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-233">Ensure that all validations succeed.</span></span>

### <a name="update-the-relying-party-file"></a><span data-ttu-id="9c2d8-234">Uppdatera filen förlitande part</span><span class="sxs-lookup"><span data-stu-id="9c2d8-234">Update the relying party file</span></span>

<span data-ttu-id="9c2d8-235">Därefter uppdaterar du filen förlitande part (RP) som initierar transporten användare som du skapade:</span><span class="sxs-lookup"><span data-stu-id="9c2d8-235">Next, update the relying party (RP) file that initiates the user journey that you created:</span></span>

1. <span data-ttu-id="9c2d8-236">Gör en kopia av SignUpOrSignIn.xml i arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-236">Make a copy of SignUpOrSignIn.xml in your working directory.</span></span> <span data-ttu-id="9c2d8-237">Sedan, byta namn på den (till exempel SignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-237">Then, rename it (for example, SignUpOrSignInWithAAD.xml).</span></span>
2. <span data-ttu-id="9c2d8-238">Öppna ny fil och uppdatera den `PolicyId` attribut för `<TrustFrameworkPolicy>` med ett unikt värde.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-238">Open the new file and update the `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value.</span></span> <span data-ttu-id="9c2d8-239">Detta är namnet på principen (till exempel SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-239">This is the name of your policy (for example, SignUpOrSignInWithAAD).</span></span>
3. <span data-ttu-id="9c2d8-240">Ändra den `ReferenceId` attribut i `<DefaultUserJourney>` så att den matchar den `Id` transporten för nya användare som du skapade (till exempel SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-240">Modify the `ReferenceId` attribute in `<DefaultUserJourney>` to match the `Id` of the new user journey that you created (for example, SignUpOrSignUsingContoso).</span></span>
4. <span data-ttu-id="9c2d8-241">Spara ändringarna och sedan ladda upp filen.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-241">Save your changes, and then upload the file.</span></span>

## <a name="test-and-troubleshoot"></a><span data-ttu-id="9c2d8-242">Testa och felsöka</span><span class="sxs-lookup"><span data-stu-id="9c2d8-242">Test and troubleshoot</span></span>

<span data-ttu-id="9c2d8-243">Om du vill testa den anpassade principen som du just har överfört i Azure portal, gå till principbladet och klicka sedan på **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="9c2d8-243">To test the custom policy that you just uploaded, in the Azure portal, go to the policy blade, and then click **Run now**.</span></span> <span data-ttu-id="9c2d8-244">Om den inte finns [felsöka principer för anpassade](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-244">If it fails, see [Troubleshoot custom policies](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c2d8-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c2d8-245">Next steps</span></span>

<span data-ttu-id="9c2d8-246">Ge feedback till [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="9c2d8-246">Provide feedback to [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>

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
ms.openlocfilehash: 6c073d70debfdc3560405955d65fa9ccaa7d8b1f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a><span data-ttu-id="1d2be-103">Azure Active Directory B2C: Logga in med hjälp av Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1d2be-103">Azure Active Directory B2C: Sign in by using Azure AD accounts</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="1d2be-104">Den här artikeln visar hur du aktiverar inloggning för användare från en viss Azure Active Directory (AD Azure) organisation med [anpassade principer](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="1d2be-104">This article shows you how to enable sign-in for users from a specific Azure Active Directory (Azure AD) organization through the use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d2be-105">Krav</span><span class="sxs-lookup"><span data-stu-id="1d2be-105">Prerequisites</span></span>

<span data-ttu-id="1d2be-106">Utför stegen i den [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="1d2be-106">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="1d2be-107">De här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="1d2be-107">These steps include:</span></span>

1. <span data-ttu-id="1d2be-108">Skapa ett Azure Active Directory B2C-klient (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="1d2be-108">Creating an Azure Active Directory B2C (Azure AD B2C) tenant.</span></span>
2. <span data-ttu-id="1d2be-109">Skapar ett program för Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="1d2be-109">Creating an Azure AD B2C application.</span></span>
3. <span data-ttu-id="1d2be-110">Registrering av två principmodulen program.</span><span class="sxs-lookup"><span data-stu-id="1d2be-110">Registering two policy-engine applications.</span></span>
4. <span data-ttu-id="1d2be-111">Inställning av nycklar.</span><span class="sxs-lookup"><span data-stu-id="1d2be-111">Setting up keys.</span></span>
5. <span data-ttu-id="1d2be-112">Konfigurera startpaket.</span><span class="sxs-lookup"><span data-stu-id="1d2be-112">Setting up the starter pack.</span></span>

## <a name="create-an-azure-ad-app"></a><span data-ttu-id="1d2be-113">Skapa en Azure AD-app</span><span class="sxs-lookup"><span data-stu-id="1d2be-113">Create an Azure AD app</span></span>

<span data-ttu-id="1d2be-114">Aktivera inloggning för användare från en specifik Azure AD-organisation, måste du registrera ett program i organisations Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="1d2be-114">To enable sign-in for users from a specific Azure AD organization, you need to register an application within the organizational Azure AD tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="1d2be-115">Vi använder ”contoso.com” för organisations Azure AD-klient och ”fabrikamb2c.onmicrosoft.com” som Azure AD B2C-klient i följande instruktioner.</span><span class="sxs-lookup"><span data-stu-id="1d2be-115">We use "contoso.com" for the organizational Azure AD tenant and "fabrikamb2c.onmicrosoft.com" as the Azure AD B2C tenant in the following instructions.</span></span>

1. <span data-ttu-id="1d2be-116">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1d2be-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="1d2be-117">Välj ditt konto på den översta raden.</span><span class="sxs-lookup"><span data-stu-id="1d2be-117">On the top bar, select your account.</span></span> <span data-ttu-id="1d2be-118">Från den **Directory** Välj organisations Azure AD-klient som du vill registrera ditt program (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="1d2be-118">From the **Directory** list, choose the organizational Azure AD tenant where you want to register your application (contoso.com).</span></span>
1. <span data-ttu-id="1d2be-119">Välj **fler tjänster** i den vänstra rutan och söka efter ”App registreringar”.</span><span class="sxs-lookup"><span data-stu-id="1d2be-119">Select **More services** in the left pane, and search for "App registrations."</span></span>
1. <span data-ttu-id="1d2be-120">Välj **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-120">Select **New application registration**.</span></span>
1. <span data-ttu-id="1d2be-121">Ange ett namn för ditt program (till exempel `Azure AD B2C App`).</span><span class="sxs-lookup"><span data-stu-id="1d2be-121">Enter a name for your application (for example, `Azure AD B2C App`).</span></span>
1. <span data-ttu-id="1d2be-122">Välj **webbapp / API** för programmet.</span><span class="sxs-lookup"><span data-stu-id="1d2be-122">Select **Web app / API** for the application type.</span></span>
1. <span data-ttu-id="1d2be-123">För **inloggnings-URL**, ange följande URL, där `yourtenant` ersättas med namnet på din Azure AD B2C-klient (`fabrikamb2c.onmicrosoft.com`):</span><span class="sxs-lookup"><span data-stu-id="1d2be-123">For **Sign-on URL**, enter the following URL, where `yourtenant` is replaced by the name of your Azure AD B2C tenant (`fabrikamb2c.onmicrosoft.com`):</span></span>

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. <span data-ttu-id="1d2be-124">Spara programmet-ID.</span><span class="sxs-lookup"><span data-stu-id="1d2be-124">Save the application ID.</span></span>
1. <span data-ttu-id="1d2be-125">Välj nyligen skapade programmet.</span><span class="sxs-lookup"><span data-stu-id="1d2be-125">Select the newly created application.</span></span>
1. <span data-ttu-id="1d2be-126">Under den **inställningar** bladet väljer **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-126">Under the **Settings** blade, select **Keys**.</span></span>
1. <span data-ttu-id="1d2be-127">Skapa en ny nyckel och spara den.</span><span class="sxs-lookup"><span data-stu-id="1d2be-127">Create a new key, and save it.</span></span> <span data-ttu-id="1d2be-128">Du använder den i stegen i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1d2be-128">You will use it in the steps in the next section.</span></span>

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a><span data-ttu-id="1d2be-129">Lägg till Azure AD-nyckel till Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="1d2be-129">Add the Azure AD key to Azure AD B2C</span></span>

<span data-ttu-id="1d2be-130">Du måste lagra nyckeln contoso.com program i din Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="1d2be-130">You need to store the contoso.com application key in your Azure AD B2C tenant.</span></span> <span data-ttu-id="1d2be-131">Gör så här:</span><span class="sxs-lookup"><span data-stu-id="1d2be-131">To do this:</span></span>
1. <span data-ttu-id="1d2be-132">Gå till din Azure AD B2C-klient och välj **B2C inställningar** > **identitet upplevelse Framework** > **princip nycklar**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-132">Go to your Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
1. <span data-ttu-id="1d2be-133">Välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-133">Select **+Add**.</span></span>
1. <span data-ttu-id="1d2be-134">Välj eller ange dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="1d2be-134">Select or enter these options:</span></span>
   * <span data-ttu-id="1d2be-135">Välj **manuell**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-135">Select **Manual**.</span></span>
   * <span data-ttu-id="1d2be-136">För **namn**, Välj ett namn som matchar din Azure AD-klientnamn (till exempel `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="1d2be-136">For **Name**, choose a name that matches your Azure AD tenant name (for example, `ContosoAppSecret`).</span></span>  <span data-ttu-id="1d2be-137">Prefixet `B2C_1A_` läggs automatiskt till namnet på din nyckel.</span><span class="sxs-lookup"><span data-stu-id="1d2be-137">The prefix `B2C_1A_` is added automatically to the name of your key.</span></span>
   * <span data-ttu-id="1d2be-138">Klistra in nyckeln program i den **hemlighet** rutan.</span><span class="sxs-lookup"><span data-stu-id="1d2be-138">Paste your application key in the **Secret** box.</span></span>
   * <span data-ttu-id="1d2be-139">Välj **signatur**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-139">Select **Signature**.</span></span>
1. <span data-ttu-id="1d2be-140">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-140">Select **Create**.</span></span>
1. <span data-ttu-id="1d2be-141">Bekräfta att du har skapat nyckeln `B2C_1A_ContosoAppSecret`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-141">Confirm that you've created the key `B2C_1A_ContosoAppSecret`.</span></span>


## <a name="add-a-claims-provider-in-your-base-policy"></a><span data-ttu-id="1d2be-142">Lägg till en anspråksprovider i en grundläggande princip</span><span class="sxs-lookup"><span data-stu-id="1d2be-142">Add a claims provider in your base policy</span></span>

<span data-ttu-id="1d2be-143">Om du vill att användarna att logga in med hjälp av Azure AD, måste du definiera Azure AD som en anspråksprovider.</span><span class="sxs-lookup"><span data-stu-id="1d2be-143">If you want users to sign in by using Azure AD, you need to define Azure AD as a claims provider.</span></span> <span data-ttu-id="1d2be-144">Med andra ord, måste du ange en slutpunkt som Azure AD B2C kommer att kommunicera med.</span><span class="sxs-lookup"><span data-stu-id="1d2be-144">In other words, you need to specify an endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="1d2be-145">Slutpunkten ger en uppsättning anspråk som används av Azure AD B2C för att verifiera att en specifik användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="1d2be-145">The endpoint will provide a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span> 

<span data-ttu-id="1d2be-146">Du kan definiera Azure AD som en anspråksprovider genom att lägga till Azure AD för att den `<ClaimsProvider>` nod i tilläggsfilen i principen:</span><span class="sxs-lookup"><span data-stu-id="1d2be-146">You can define Azure AD as a claims provider by adding Azure AD to the `<ClaimsProvider>` node in the extension file of your policy:</span></span>

1. <span data-ttu-id="1d2be-147">Öppna tilläggsfilen (TrustFrameworkExtensions.xml) från arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="1d2be-147">Open the extension file (TrustFrameworkExtensions.xml) from your working directory.</span></span>
1. <span data-ttu-id="1d2be-148">Hitta de `<ClaimsProviders>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1d2be-148">Find the `<ClaimsProviders>` section.</span></span> <span data-ttu-id="1d2be-149">Om det inte finns lägger du till den under rotnoden.</span><span class="sxs-lookup"><span data-stu-id="1d2be-149">If it does not exist, add it under the root node.</span></span>
1. <span data-ttu-id="1d2be-150">Lägg till en ny `<ClaimsProvider>` nod på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1d2be-150">Add a new `<ClaimsProvider>` node as follows:</span></span>

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

1. <span data-ttu-id="1d2be-151">Under den `<ClaimsProvider>` nod, uppdatera värdet för `<Domain>` till ett unikt värde som kan användas för att skilja den från andra identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="1d2be-151">Under the `<ClaimsProvider>` node, update the value for `<Domain>` to a unique value that can be used to distinguish it from other identity providers.</span></span>
1. <span data-ttu-id="1d2be-152">Under den `<ClaimsProvider>` nod, uppdatera värdet för `<DisplayName>` till ett eget namn för anspråksprovidern.</span><span class="sxs-lookup"><span data-stu-id="1d2be-152">Under the `<ClaimsProvider>` node, update the value for `<DisplayName>` to a friendly name for the claims provider.</span></span> <span data-ttu-id="1d2be-153">Det här värdet används inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="1d2be-153">This value is not currently used.</span></span>

### <a name="update-the-technical-profile"></a><span data-ttu-id="1d2be-154">Uppdatera tekniska profilen</span><span class="sxs-lookup"><span data-stu-id="1d2be-154">Update the technical profile</span></span>

<span data-ttu-id="1d2be-155">Om du vill hämta en token från Azure AD-slutpunkten måste du definiera de protokoll som Azure AD B2C ska använda för att kommunicera med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1d2be-155">To get a token from the Azure AD endpoint, you need to define the protocols that Azure AD B2C should use to communicate with Azure AD.</span></span> <span data-ttu-id="1d2be-156">Detta görs i den `<TechnicalProfile>` element av `<ClaimsProvider>`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-156">This is done inside the `<TechnicalProfile>` element of  `<ClaimsProvider>`.</span></span>
1. <span data-ttu-id="1d2be-157">Uppdatera ID för den `<TechnicalProfile>` nod.</span><span class="sxs-lookup"><span data-stu-id="1d2be-157">Update the ID of the `<TechnicalProfile>` node.</span></span> <span data-ttu-id="1d2be-158">Detta ID används för att referera till den här tekniska profilen från andra delar av principen.</span><span class="sxs-lookup"><span data-stu-id="1d2be-158">This ID is used to refer to this technical profile from other parts of the policy.</span></span>
1. <span data-ttu-id="1d2be-159">Uppdatera värdet för `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-159">Update the value for `<DisplayName>`.</span></span> <span data-ttu-id="1d2be-160">Det här värdet kommer att visas på knappen Logga in på skärmen inloggning.</span><span class="sxs-lookup"><span data-stu-id="1d2be-160">This value will be displayed on the sign-in button on your sign-in screen.</span></span>
1. <span data-ttu-id="1d2be-161">Uppdatera värdet för `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-161">Update the value for `<Description>`.</span></span>
1. <span data-ttu-id="1d2be-162">Azure AD används OpenID Connect-protokollet, så se till att värdet för `<Protocol>` är `"OpenIdConnect"`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-162">Azure AD uses the OpenID Connect protocol, so ensure that the value for `<Protocol>` is `"OpenIdConnect"`.</span></span>

<span data-ttu-id="1d2be-163">Du måste uppdatera de `<Metadata>` i XML-filen som anges ovan motsvarar konfigurationsinställningarna för ditt specifika Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="1d2be-163">You need to update the `<Metadata>` section in the XML file referred to earlier to reflect the configuration settings for your specific Azure AD tenant.</span></span> <span data-ttu-id="1d2be-164">I XML-filen att uppdatera metadatavärden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="1d2be-164">In the XML file, update the metadata values as follows:</span></span>

1. <span data-ttu-id="1d2be-165">Ange `<Item Key="METADATA">` till `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, där `yourAzureADtenant` är Azure AD-klientnamn (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="1d2be-165">Set `<Item Key="METADATA">` to `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, where `yourAzureADtenant` is your Azure AD tenant name (contoso.com).</span></span>
1. <span data-ttu-id="1d2be-166">Öppna webbläsaren och gå till den `METADATA` URL som du precis har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="1d2be-166">Open your browser and go to the `METADATA` URL that you just updated.</span></span>
1. <span data-ttu-id="1d2be-167">Leta efter 'issuer-objektet i webbläsaren och kopiera värdet.</span><span class="sxs-lookup"><span data-stu-id="1d2be-167">In the browser, look for the 'issuer' object and copy its value.</span></span> <span data-ttu-id="1d2be-168">Det bör se ut ungefär så här: `https://sts.windows.net/{tenantId}/`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-168">It should look like the following: `https://sts.windows.net/{tenantId}/`.</span></span>
1. <span data-ttu-id="1d2be-169">Klistra in värdet för `<Item Key="ProviderName">` i XML-filen.</span><span class="sxs-lookup"><span data-stu-id="1d2be-169">Paste the value for `<Item Key="ProviderName">` in the XML file.</span></span>
1. <span data-ttu-id="1d2be-170">Ange `<Item Key="client_id">` till det program-ID från appregistrering.</span><span class="sxs-lookup"><span data-stu-id="1d2be-170">Set `<Item Key="client_id">` to the application ID from the app registration.</span></span>
1. <span data-ttu-id="1d2be-171">Ange `<Item Key="IdTokenAudience">` till det program-ID från appregistrering.</span><span class="sxs-lookup"><span data-stu-id="1d2be-171">Set `<Item Key="IdTokenAudience">` to the application ID from the app registration.</span></span>
1. <span data-ttu-id="1d2be-172">Se till att `<Item Key="response_types">` är inställd på `id_token`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-172">Ensure that `<Item Key="response_types">` is set to `id_token`.</span></span>
1. <span data-ttu-id="1d2be-173">Se till att `<Item Key="UsePolicyInRedirectUri">` är inställd på `false`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-173">Ensure that `<Item Key="UsePolicyInRedirectUri">` is set to `false`.</span></span>

<span data-ttu-id="1d2be-174">Du måste också länka Azure AD-hemlighet som du har registrerat i din Azure AD B2C-klient med Azure AD `<ClaimsProvider>` information:</span><span class="sxs-lookup"><span data-stu-id="1d2be-174">You also need to link the Azure AD secret that you registered in your Azure AD B2C tenant to the Azure AD `<ClaimsProvider>` information:</span></span>

* <span data-ttu-id="1d2be-175">I den `<CryptographicKeys>` i föregående XML-filen och uppdatera värdet för `StorageReferenceId` referens-ID: t för den hemlighet som du har definierat (till exempel `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="1d2be-175">In the `<CryptographicKeys>` section in the preceding XML file, update the value for `StorageReferenceId` to the reference ID of the secret that you defined (for example, `ContosoAppSecret`).</span></span>

### <a name="upload-the-extension-file-for-verification"></a><span data-ttu-id="1d2be-176">Ladda upp tilläggsfilen för verifiering</span><span class="sxs-lookup"><span data-stu-id="1d2be-176">Upload the extension file for verification</span></span>

<span data-ttu-id="1d2be-177">Nu bör har du konfigurerat principen så att Azure AD B2C vet hur att kommunicera med Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="1d2be-177">By now, you have configured your policy so that Azure AD B2C knows how to communicate with your Azure AD directory.</span></span> <span data-ttu-id="1d2be-178">Försök att ladda upp tilläggsfilen av din princip bara för att bekräfta att den inte har några problem hittills.</span><span class="sxs-lookup"><span data-stu-id="1d2be-178">Try uploading the extension file of your policy just to confirm that it doesn't have any issues so far.</span></span> <span data-ttu-id="1d2be-179">Gör så här:</span><span class="sxs-lookup"><span data-stu-id="1d2be-179">To do so:</span></span>

1. <span data-ttu-id="1d2be-180">Öppna den **alla principer** bladet i Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="1d2be-180">Open the **All Policies** blade in your Azure AD B2C tenant.</span></span>
1. <span data-ttu-id="1d2be-181">Kontrollera den **skriva över principen om den finns** rutan.</span><span class="sxs-lookup"><span data-stu-id="1d2be-181">Check the **Overwrite the policy if it exists** box.</span></span>
1. <span data-ttu-id="1d2be-182">Ladda upp fil (TrustFrameworkExtensions.xml) och se till att inte misslyckas verifieringen.</span><span class="sxs-lookup"><span data-stu-id="1d2be-182">Upload the extension file (TrustFrameworkExtensions.xml), and ensure that it does not fail the validation.</span></span>

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a><span data-ttu-id="1d2be-183">Registrera Azure AD-anspråksprovidern för en användare resa</span><span class="sxs-lookup"><span data-stu-id="1d2be-183">Register the Azure AD claims provider to a user journey</span></span>

<span data-ttu-id="1d2be-184">Du måste nu lägga till Azure AD-identitetsleverantör till någon av dina användare resor.</span><span class="sxs-lookup"><span data-stu-id="1d2be-184">You now need to add the Azure AD identity provider to one of your user journeys.</span></span> <span data-ttu-id="1d2be-185">Nu identitetsleverantören har ställts in, men den är inte tillgänglig i alla skärmar sign-upp/inloggning.</span><span class="sxs-lookup"><span data-stu-id="1d2be-185">At this point, the identity provider has been set up, but it’s not available in any of the sign-up/sign-in screens.</span></span> <span data-ttu-id="1d2be-186">Om du vill göra den tillgänglig, kommer vi skapar en dubblett av en befintlig mall användaren resa och sedan ändra den så att den också har identitetsleverantören som Azure AD:</span><span class="sxs-lookup"><span data-stu-id="1d2be-186">To make it available, we will create a duplicate of an existing template user journey, and then modify it so that it also has the Azure AD identity provider:</span></span>

1. <span data-ttu-id="1d2be-187">Öppna filen grundläggande av principen (till exempel TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="1d2be-187">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
1. <span data-ttu-id="1d2be-188">Hitta de `<UserJourneys>` element och kopiera hela `<UserJourney>` nod som innehåller `Id="SignUpOrSignIn"`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-188">Find the `<UserJourneys>` element and copy the entire `<UserJourney>` node that includes `Id="SignUpOrSignIn"`.</span></span>
1. <span data-ttu-id="1d2be-189">Öppna tilläggsfilen (till exempel TrustFrameworkExtensions.xml) och Sök efter den `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="1d2be-189">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="1d2be-190">Om elementet inte finns, kan du lägga till en.</span><span class="sxs-lookup"><span data-stu-id="1d2be-190">If the element doesn't exist, add one.</span></span>
1. <span data-ttu-id="1d2be-191">Klistra in hela `<UserJourney>` nod som du kopierade som underordnad till den `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="1d2be-191">Paste the entire `<UserJourney>` node that you copied as a child of the `<UserJourneys>` element.</span></span>
1. <span data-ttu-id="1d2be-192">Byt namn på ID för den nya användare resan (t.ex, Byt namn på det `SignUpOrSignUsingContoso`).</span><span class="sxs-lookup"><span data-stu-id="1d2be-192">Rename the ID of the new user journey (for example, rename as `SignUpOrSignUsingContoso`).</span></span>

### <a name="display-the-button"></a><span data-ttu-id="1d2be-193">Visa knappen ””</span><span class="sxs-lookup"><span data-stu-id="1d2be-193">Display the "button"</span></span>

<span data-ttu-id="1d2be-194">Den `<ClaimsProviderSelection>` element är detsamma som knappen identity-providern på en sign-upp/inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="1d2be-194">The `<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up/sign-in screen.</span></span> <span data-ttu-id="1d2be-195">Om du lägger till en `<ClaimsProviderSelection>` element för Azure AD, en ny knapp visas när en användare de hamnar på sidan.</span><span class="sxs-lookup"><span data-stu-id="1d2be-195">If you add a `<ClaimsProviderSelection>` element for Azure AD, a new button shows up when a user lands on the page.</span></span> <span data-ttu-id="1d2be-196">Lägg till det här elementet:</span><span class="sxs-lookup"><span data-stu-id="1d2be-196">To add this element:</span></span>

1. <span data-ttu-id="1d2be-197">Hitta de `<OrchestrationStep>` nod som innehåller `Order="1"` i transporten användare som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="1d2be-197">Find the `<OrchestrationStep>` node that includes `Order="1"` in the user journey that you just created.</span></span>
1. <span data-ttu-id="1d2be-198">Lägg till följande:</span><span class="sxs-lookup"><span data-stu-id="1d2be-198">Add the following:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. <span data-ttu-id="1d2be-199">Ange `TargetClaimsExchangeId` till ett lämpligt värde.</span><span class="sxs-lookup"><span data-stu-id="1d2be-199">Set `TargetClaimsExchangeId` to an appropriate value.</span></span> <span data-ttu-id="1d2be-200">Vi rekommenderar följande samma regler som andra:  *\[ClaimProviderName\]Exchange*.</span><span class="sxs-lookup"><span data-stu-id="1d2be-200">We recommend following the same convention as others: *\[ClaimProviderName\]Exchange*.</span></span>

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="1d2be-201">Länka knappen till en åtgärd</span><span class="sxs-lookup"><span data-stu-id="1d2be-201">Link the button to an action</span></span>

<span data-ttu-id="1d2be-202">Nu när du har en knapp på plats måste länka till en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="1d2be-202">Now that you have a button in place, you need to link it to an action.</span></span> <span data-ttu-id="1d2be-203">Åtgärden är i det här fallet för Azure AD B2C att kommunicera med Azure AD ta emot en token.</span><span class="sxs-lookup"><span data-stu-id="1d2be-203">The action, in this case, is for Azure AD B2C to communicate with Azure AD to receive a token.</span></span> <span data-ttu-id="1d2be-204">Länka knappen till en åtgärd genom att länka tekniska profilen för din Azure AD-anspråksleverantör:</span><span class="sxs-lookup"><span data-stu-id="1d2be-204">Link the button to an action by linking the technical profile for your Azure AD claims provider:</span></span>

1. <span data-ttu-id="1d2be-205">Hitta de `<OrchestrationStep>` som innehåller `Order="2"` i den `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="1d2be-205">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
1. <span data-ttu-id="1d2be-206">Lägg till följande:</span><span class="sxs-lookup"><span data-stu-id="1d2be-206">Add the following:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. <span data-ttu-id="1d2be-207">Uppdatera `Id` till samma värde som `TargetClaimsExchangeId` i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1d2be-207">Update `Id` to the same value as that of `TargetClaimsExchangeId` in the preceding section.</span></span>
1. <span data-ttu-id="1d2be-208">Uppdatera `TechnicalProfileReferenceId` -ID: t för den tekniska profilen du skapade tidigare (ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="1d2be-208">Update `TechnicalProfileReferenceId` to the ID of the technical profile you created earlier (ContosoProfile).</span></span>

### <a name="upload-the-updated-extension-file"></a><span data-ttu-id="1d2be-209">Ladda upp filen uppdaterade tillägg</span><span class="sxs-lookup"><span data-stu-id="1d2be-209">Upload the updated extension file</span></span>

<span data-ttu-id="1d2be-210">Du är klar ändra tilläggsfilen.</span><span class="sxs-lookup"><span data-stu-id="1d2be-210">You are done modifying the extension file.</span></span> <span data-ttu-id="1d2be-211">Spara den.</span><span class="sxs-lookup"><span data-stu-id="1d2be-211">Save it.</span></span> <span data-ttu-id="1d2be-212">Sedan överföra filen och se till att alla verifieringar lyckas.</span><span class="sxs-lookup"><span data-stu-id="1d2be-212">Then upload the file, and ensure that all validations succeed.</span></span>

### <a name="update-the-rp-file"></a><span data-ttu-id="1d2be-213">Uppdatera RP-filen</span><span class="sxs-lookup"><span data-stu-id="1d2be-213">Update the RP file</span></span>

<span data-ttu-id="1d2be-214">Nu måste du uppdatera filen förlitande part (RP) som initierar användaren resan som du just skapat:</span><span class="sxs-lookup"><span data-stu-id="1d2be-214">You now need to update the relying party (RP) file that will initiate the user journey that you just created:</span></span>

1. <span data-ttu-id="1d2be-215">Gör en kopia av SignUpOrSignIn.xml i arbetskatalogen och byta namn på den (till exempel ändra dess namn till SignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="1d2be-215">Make a copy of SignUpOrSignIn.xml in your working directory, and rename it (for example, rename it to SignUpOrSignInWithAAD.xml).</span></span>
1. <span data-ttu-id="1d2be-216">Öppna ny fil och uppdatera den `PolicyId` attribut för `<TrustFrameworkPolicy>` med ett unikt värde (till exempel SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="1d2be-216">Open the new file and update the `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value (for example, SignUpOrSignInWithAAD).</span></span> <br> <span data-ttu-id="1d2be-217">Det här är namnet på principen (till exempel B2C\_1A\_SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="1d2be-217">This will be the name of your policy (for example, B2C\_1A\_SignUpOrSignInWithAAD).</span></span>
1. <span data-ttu-id="1d2be-218">Ändra den `ReferenceId` attribut i `<DefaultUserJourney>` att matcha ID för den nya användare resa som du skapade (SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="1d2be-218">Modify the `ReferenceId` attribute in `<DefaultUserJourney>` to match the ID of the new user journey that you created (SignUpOrSignUsingContoso).</span></span>
1. <span data-ttu-id="1d2be-219">Spara ändringarna och ladda upp filen.</span><span class="sxs-lookup"><span data-stu-id="1d2be-219">Save your changes, and upload the file.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1d2be-220">Felsökning</span><span class="sxs-lookup"><span data-stu-id="1d2be-220">Troubleshooting</span></span>

<span data-ttu-id="1d2be-221">Testa den anpassade principen som du just har överfört öppna dess bladet och klicka på **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-221">Test the custom policy that you just uploaded by opening its blade and clicking **Run now**.</span></span> <span data-ttu-id="1d2be-222">Läs mer om för att diagnostisera problem [felsökning](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="1d2be-222">To diagnose problems, read about [troubleshooting](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d2be-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d2be-223">Next steps</span></span>

<span data-ttu-id="1d2be-224">Ge feedback till [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="1d2be-224">Provide feedback to [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>

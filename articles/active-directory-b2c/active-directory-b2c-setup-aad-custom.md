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
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a><span data-ttu-id="be4a4-103">Azure Active Directory B2C: Logga in med hjälp av Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="be4a4-103">Azure Active Directory B2C: Sign in by using Azure AD accounts</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="be4a4-104">Den här artikeln lär du dig hur tooenable inloggning för användare från en viss Azure Active Directory (AD Azure) organisation via hello [anpassade principer](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="be4a4-104">This article shows you how tooenable sign-in for users from a specific Azure Active Directory (Azure AD) organization through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be4a4-105">Krav</span><span class="sxs-lookup"><span data-stu-id="be4a4-105">Prerequisites</span></span>

<span data-ttu-id="be4a4-106">Fullständig hello stegen i hello [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="be4a4-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="be4a4-107">De här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="be4a4-107">These steps include:</span></span>

1. <span data-ttu-id="be4a4-108">Skapa ett Azure Active Directory B2C-klient (Azure AD B2C).</span><span class="sxs-lookup"><span data-stu-id="be4a4-108">Creating an Azure Active Directory B2C (Azure AD B2C) tenant.</span></span>
2. <span data-ttu-id="be4a4-109">Skapar ett program för Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="be4a4-109">Creating an Azure AD B2C application.</span></span>
3. <span data-ttu-id="be4a4-110">Registrering av två principmodulen program.</span><span class="sxs-lookup"><span data-stu-id="be4a4-110">Registering two policy-engine applications.</span></span>
4. <span data-ttu-id="be4a4-111">Inställning av nycklar.</span><span class="sxs-lookup"><span data-stu-id="be4a4-111">Setting up keys.</span></span>
5. <span data-ttu-id="be4a4-112">Konfigurera hello startpaket.</span><span class="sxs-lookup"><span data-stu-id="be4a4-112">Setting up hello starter pack.</span></span>

## <a name="create-an-azure-ad-app"></a><span data-ttu-id="be4a4-113">Skapa en Azure AD-app</span><span class="sxs-lookup"><span data-stu-id="be4a4-113">Create an Azure AD app</span></span>

<span data-ttu-id="be4a4-114">tooenable-inloggning för användare från en specifik Azure AD-organisation, behöver du tooregister ett program i hello organisationens Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="be4a4-114">tooenable sign-in for users from a specific Azure AD organization, you need tooregister an application within hello organizational Azure AD tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="be4a4-115">Vi använder ”contoso.com” för hello organisationens Azure AD-klientorganisation och ”fabrikamb2c.onmicrosoft.com” som hello Azure AD B2C-klient i hello följa anvisningar.</span><span class="sxs-lookup"><span data-stu-id="be4a4-115">We use "contoso.com" for hello organizational Azure AD tenant and "fabrikamb2c.onmicrosoft.com" as hello Azure AD B2C tenant in hello following instructions.</span></span>

1. <span data-ttu-id="be4a4-116">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="be4a4-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="be4a4-117">Välj ditt konto hello översta fältet.</span><span class="sxs-lookup"><span data-stu-id="be4a4-117">On hello top bar, select your account.</span></span> <span data-ttu-id="be4a4-118">Från hello **Directory** Välj hello organisationens Azure AD-klient där du vill att tooregister programmet (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="be4a4-118">From hello **Directory** list, choose hello organizational Azure AD tenant where you want tooregister your application (contoso.com).</span></span>
1. <span data-ttu-id="be4a4-119">Välj **fler tjänster** i hello till vänster och Sök efter ”App registreringar”.</span><span class="sxs-lookup"><span data-stu-id="be4a4-119">Select **More services** in hello left pane, and search for "App registrations."</span></span>
1. <span data-ttu-id="be4a4-120">Välj **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="be4a4-120">Select **New application registration**.</span></span>
1. <span data-ttu-id="be4a4-121">Ange ett namn för ditt program (till exempel `Azure AD B2C App`).</span><span class="sxs-lookup"><span data-stu-id="be4a4-121">Enter a name for your application (for example, `Azure AD B2C App`).</span></span>
1. <span data-ttu-id="be4a4-122">Välj **webbapp / API** för hello programtyp.</span><span class="sxs-lookup"><span data-stu-id="be4a4-122">Select **Web app / API** for hello application type.</span></span>
1. <span data-ttu-id="be4a4-123">För **inloggnings-URL**, ange hello följande URL, där `yourtenant` ersätts av hello namn för din Azure AD B2C-klient (`fabrikamb2c.onmicrosoft.com`):</span><span class="sxs-lookup"><span data-stu-id="be4a4-123">For **Sign-on URL**, enter hello following URL, where `yourtenant` is replaced by hello name of your Azure AD B2C tenant (`fabrikamb2c.onmicrosoft.com`):</span></span>

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. <span data-ttu-id="be4a4-124">Spara hello program-ID.</span><span class="sxs-lookup"><span data-stu-id="be4a4-124">Save hello application ID.</span></span>
1. <span data-ttu-id="be4a4-125">Välj hello nyligen skapade programmet.</span><span class="sxs-lookup"><span data-stu-id="be4a4-125">Select hello newly created application.</span></span>
1. <span data-ttu-id="be4a4-126">Under hello **inställningar** bladet väljer **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="be4a4-126">Under hello **Settings** blade, select **Keys**.</span></span>
1. <span data-ttu-id="be4a4-127">Skapa en ny nyckel och spara den.</span><span class="sxs-lookup"><span data-stu-id="be4a4-127">Create a new key, and save it.</span></span> <span data-ttu-id="be4a4-128">Du använder den i hello stegen i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="be4a4-128">You will use it in hello steps in hello next section.</span></span>

## <a name="add-hello-azure-ad-key-tooazure-ad-b2c"></a><span data-ttu-id="be4a4-129">Lägg till nyckel hello Azure AD-tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="be4a4-129">Add hello Azure AD key tooAzure AD B2C</span></span>

<span data-ttu-id="be4a4-130">Du behöver toostore hello contoso.com programmet nyckeln i din Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="be4a4-130">You need toostore hello contoso.com application key in your Azure AD B2C tenant.</span></span> <span data-ttu-id="be4a4-131">toodo detta:</span><span class="sxs-lookup"><span data-stu-id="be4a4-131">toodo this:</span></span>
1. <span data-ttu-id="be4a4-132">Gå tooyour Azure AD B2C-klient och använda **B2C inställningar** > **identitet upplevelse Framework** > **princip nycklar**.</span><span class="sxs-lookup"><span data-stu-id="be4a4-132">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
1. <span data-ttu-id="be4a4-133">Välj **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="be4a4-133">Select **+Add**.</span></span>
1. <span data-ttu-id="be4a4-134">Välj eller ange dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="be4a4-134">Select or enter these options:</span></span>
   * <span data-ttu-id="be4a4-135">Välj **manuell**.</span><span class="sxs-lookup"><span data-stu-id="be4a4-135">Select **Manual**.</span></span>
   * <span data-ttu-id="be4a4-136">För **namn**, Välj ett namn som matchar din Azure AD-klientnamn (till exempel `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="be4a4-136">For **Name**, choose a name that matches your Azure AD tenant name (for example, `ContosoAppSecret`).</span></span>  <span data-ttu-id="be4a4-137">hello prefixet `B2C_1A_` läggs till automatiskt toohello namnet på din nyckel.</span><span class="sxs-lookup"><span data-stu-id="be4a4-137">hello prefix `B2C_1A_` is added automatically toohello name of your key.</span></span>
   * <span data-ttu-id="be4a4-138">Klistra in programnyckeln i hello **hemlighet** rutan.</span><span class="sxs-lookup"><span data-stu-id="be4a4-138">Paste your application key in hello **Secret** box.</span></span>
   * <span data-ttu-id="be4a4-139">Välj **signatur**.</span><span class="sxs-lookup"><span data-stu-id="be4a4-139">Select **Signature**.</span></span>
1. <span data-ttu-id="be4a4-140">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="be4a4-140">Select **Create**.</span></span>
1. <span data-ttu-id="be4a4-141">Bekräfta att du har skapat hello nyckeln `B2C_1A_ContosoAppSecret`.</span><span class="sxs-lookup"><span data-stu-id="be4a4-141">Confirm that you've created hello key `B2C_1A_ContosoAppSecret`.</span></span>


## <a name="add-a-claims-provider-in-your-base-policy"></a><span data-ttu-id="be4a4-142">Lägg till en anspråksprovider i en grundläggande princip</span><span class="sxs-lookup"><span data-stu-id="be4a4-142">Add a claims provider in your base policy</span></span>

<span data-ttu-id="be4a4-143">Om du vill användare toosign i med hjälp av Azure AD, måste toodefine Azure AD som en anspråksprovider.</span><span class="sxs-lookup"><span data-stu-id="be4a4-143">If you want users toosign in by using Azure AD, you need toodefine Azure AD as a claims provider.</span></span> <span data-ttu-id="be4a4-144">Du behöver med andra ord toospecify en slutpunkt som Azure AD B2C kommer att kommunicera med.</span><span class="sxs-lookup"><span data-stu-id="be4a4-144">In other words, you need toospecify an endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="be4a4-145">hello endpoint ger en uppsättning anspråk som används av Azure AD B2C-tooverify som en specifik användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="be4a4-145">hello endpoint will provide a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span> 

<span data-ttu-id="be4a4-146">Du kan definiera Azure AD som en anspråksprovider genom att lägga till Azure AD toohello `<ClaimsProvider>` nod i hello tilläggsfilen i principen:</span><span class="sxs-lookup"><span data-stu-id="be4a4-146">You can define Azure AD as a claims provider by adding Azure AD toohello `<ClaimsProvider>` node in hello extension file of your policy:</span></span>

1. <span data-ttu-id="be4a4-147">Öppna filen hello-tillägg (TrustFrameworkExtensions.xml) från arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="be4a4-147">Open hello extension file (TrustFrameworkExtensions.xml) from your working directory.</span></span>
1. <span data-ttu-id="be4a4-148">Hitta hello `<ClaimsProviders>` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="be4a4-148">Find hello `<ClaimsProviders>` section.</span></span> <span data-ttu-id="be4a4-149">Om det inte finns lägger du till den under hello rotnoden.</span><span class="sxs-lookup"><span data-stu-id="be4a4-149">If it does not exist, add it under hello root node.</span></span>
1. <span data-ttu-id="be4a4-150">Lägg till en ny `<ClaimsProvider>` nod på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="be4a4-150">Add a new `<ClaimsProvider>` node as follows:</span></span>

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

1. <span data-ttu-id="be4a4-151">Under hello `<ClaimsProvider>` noden uppdatering hello värde för `<Domain>` tooa unikt värde som kan använda toodistinguish den från andra identitetsleverantörer.</span><span class="sxs-lookup"><span data-stu-id="be4a4-151">Under hello `<ClaimsProvider>` node, update hello value for `<Domain>` tooa unique value that can be used toodistinguish it from other identity providers.</span></span>
1. <span data-ttu-id="be4a4-152">Under hello `<ClaimsProvider>` noden uppdatering hello värde för `<DisplayName>` tooa eget namn för hello anspråk providern.</span><span class="sxs-lookup"><span data-stu-id="be4a4-152">Under hello `<ClaimsProvider>` node, update hello value for `<DisplayName>` tooa friendly name for hello claims provider.</span></span> <span data-ttu-id="be4a4-153">Det här värdet används inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="be4a4-153">This value is not currently used.</span></span>

### <a name="update-hello-technical-profile"></a><span data-ttu-id="be4a4-154">Uppdatera hello tekniska profil</span><span class="sxs-lookup"><span data-stu-id="be4a4-154">Update hello technical profile</span></span>

<span data-ttu-id="be4a4-155">tooget en token från hello Azure AD-slutpunkten måste toodefine hello protokoll att Azure AD B2C ska använda toocommunicate med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="be4a4-155">tooget a token from hello Azure AD endpoint, you need toodefine hello protocols that Azure AD B2C should use toocommunicate with Azure AD.</span></span> <span data-ttu-id="be4a4-156">Detta görs i hello `<TechnicalProfile>` element av `<ClaimsProvider>`.</span><span class="sxs-lookup"><span data-stu-id="be4a4-156">This is done inside hello `<TechnicalProfile>` element of  `<ClaimsProvider>`.</span></span>
1. <span data-ttu-id="be4a4-157">Uppdatera hello-ID för hello `<TechnicalProfile>` nod.</span><span class="sxs-lookup"><span data-stu-id="be4a4-157">Update hello ID of hello `<TechnicalProfile>` node.</span></span> <span data-ttu-id="be4a4-158">Detta ID är används toorefer toothis tekniska profil från andra delar av hello princip.</span><span class="sxs-lookup"><span data-stu-id="be4a4-158">This ID is used toorefer toothis technical profile from other parts of hello policy.</span></span>
1. <span data-ttu-id="be4a4-159">Uppdatera hello värde för `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="be4a4-159">Update hello value for `<DisplayName>`.</span></span> <span data-ttu-id="be4a4-160">Det här värdet kommer att visas på hello-knappen Logga in på skärmen inloggning.</span><span class="sxs-lookup"><span data-stu-id="be4a4-160">This value will be displayed on hello sign-in button on your sign-in screen.</span></span>
1. <span data-ttu-id="be4a4-161">Uppdatera hello värde för `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="be4a4-161">Update hello value for `<Description>`.</span></span>
1. <span data-ttu-id="be4a4-162">Azure AD används hello OpenID Connect-protokollet, så se till att hello-värdet för `<Protocol>` är `"OpenIdConnect"`.</span><span class="sxs-lookup"><span data-stu-id="be4a4-162">Azure AD uses hello OpenID Connect protocol, so ensure that hello value for `<Protocol>` is `"OpenIdConnect"`.</span></span>

<span data-ttu-id="be4a4-163">Du behöver tooupdate hello `<Metadata>` i hello XML-filen som anges tooearlier tooreflect hello konfigurationsinställningar för ditt specifika Azure AD-klient.</span><span class="sxs-lookup"><span data-stu-id="be4a4-163">You need tooupdate hello `<Metadata>` section in hello XML file referred tooearlier tooreflect hello configuration settings for your specific Azure AD tenant.</span></span> <span data-ttu-id="be4a4-164">Uppdatera hello metadatavärden på följande sätt i hello XML-fil:</span><span class="sxs-lookup"><span data-stu-id="be4a4-164">In hello XML file, update hello metadata values as follows:</span></span>

1. <span data-ttu-id="be4a4-165">Ange `<Item Key="METADATA">` för`https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, där `yourAzureADtenant` är Azure AD-klientnamn (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="be4a4-165">Set `<Item Key="METADATA">` too`https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, where `yourAzureADtenant` is your Azure AD tenant name (contoso.com).</span></span>
1. <span data-ttu-id="be4a4-166">Öppna din webbläsare och gå toohello `METADATA` URL som du precis har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="be4a4-166">Open your browser and go toohello `METADATA` URL that you just updated.</span></span>
1. <span data-ttu-id="be4a4-167">Leta efter hello 'issuer-objekt i hello webbläsare och kopiering av värdet.</span><span class="sxs-lookup"><span data-stu-id="be4a4-167">In hello browser, look for hello 'issuer' object and copy its value.</span></span> <span data-ttu-id="be4a4-168">Det bör se ut som följande hello: `https://sts.windows.net/{tenantId}/`.</span><span class="sxs-lookup"><span data-stu-id="be4a4-168">It should look like hello following: `https://sts.windows.net/{tenantId}/`.</span></span>
1. <span data-ttu-id="be4a4-169">Klistra in hello värde för `<Item Key="ProviderName">` i hello XML-fil.</span><span class="sxs-lookup"><span data-stu-id="be4a4-169">Paste hello value for `<Item Key="ProviderName">` in hello XML file.</span></span>
1. <span data-ttu-id="be4a4-170">Ange `<Item Key="client_id">` toohello program-ID från hello appregistrering.</span><span class="sxs-lookup"><span data-stu-id="be4a4-170">Set `<Item Key="client_id">` toohello application ID from hello app registration.</span></span>
1. <span data-ttu-id="be4a4-171">Ange `<Item Key="IdTokenAudience">` toohello program-ID från hello appregistrering.</span><span class="sxs-lookup"><span data-stu-id="be4a4-171">Set `<Item Key="IdTokenAudience">` toohello application ID from hello app registration.</span></span>
1. <span data-ttu-id="be4a4-172">Se till att `<Item Key="response_types">` har angetts för`id_token`.</span><span class="sxs-lookup"><span data-stu-id="be4a4-172">Ensure that `<Item Key="response_types">` is set too`id_token`.</span></span>
1. <span data-ttu-id="be4a4-173">Se till att `<Item Key="UsePolicyInRedirectUri">` har angetts för`false`.</span><span class="sxs-lookup"><span data-stu-id="be4a4-173">Ensure that `<Item Key="UsePolicyInRedirectUri">` is set too`false`.</span></span>

<span data-ttu-id="be4a4-174">Du måste också toolink hello Azure AD hemlighet som du har registrerat i din Azure AD med Azure AD B2C-klient toohello `<ClaimsProvider>` information:</span><span class="sxs-lookup"><span data-stu-id="be4a4-174">You also need toolink hello Azure AD secret that you registered in your Azure AD B2C tenant toohello Azure AD `<ClaimsProvider>` information:</span></span>

* <span data-ttu-id="be4a4-175">I hello `<CryptographicKeys>` under hello före XML-fil, uppdatera hello värde för `StorageReferenceId` toohello referens-ID för hello hemlighet som du definierade (till exempel `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="be4a4-175">In hello `<CryptographicKeys>` section in hello preceding XML file, update hello value for `StorageReferenceId` toohello reference ID of hello secret that you defined (for example, `ContosoAppSecret`).</span></span>

### <a name="upload-hello-extension-file-for-verification"></a><span data-ttu-id="be4a4-176">Överför hello tilläggsfilen för verifiering</span><span class="sxs-lookup"><span data-stu-id="be4a4-176">Upload hello extension file for verification</span></span>

<span data-ttu-id="be4a4-177">Nu bör du har konfigurerat principen så att Azure AD B2C vet hur toocommunicate med Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="be4a4-177">By now, you have configured your policy so that Azure AD B2C knows how toocommunicate with your Azure AD directory.</span></span> <span data-ttu-id="be4a4-178">Försök att ladda upp hello tilläggsfilen av din princip bara tooconfirm som inte har några problem hittills.</span><span class="sxs-lookup"><span data-stu-id="be4a4-178">Try uploading hello extension file of your policy just tooconfirm that it doesn't have any issues so far.</span></span> <span data-ttu-id="be4a4-179">toodo så:</span><span class="sxs-lookup"><span data-stu-id="be4a4-179">toodo so:</span></span>

1. <span data-ttu-id="be4a4-180">Öppna hello **alla principer** bladet i Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="be4a4-180">Open hello **All Policies** blade in your Azure AD B2C tenant.</span></span>
1. <span data-ttu-id="be4a4-181">Kontrollera hello **skriva över hello principen om den finns** rutan.</span><span class="sxs-lookup"><span data-stu-id="be4a4-181">Check hello **Overwrite hello policy if it exists** box.</span></span>
1. <span data-ttu-id="be4a4-182">Ladda upp filen hello-tillägg (TrustFrameworkExtensions.xml) och se till att inte misslyckas verifieringen hello.</span><span class="sxs-lookup"><span data-stu-id="be4a4-182">Upload hello extension file (TrustFrameworkExtensions.xml), and ensure that it does not fail hello validation.</span></span>

## <a name="register-hello-azure-ad-claims-provider-tooa-user-journey"></a><span data-ttu-id="be4a4-183">Registrera hello Azure AD anspråk providern tooa användaren resa</span><span class="sxs-lookup"><span data-stu-id="be4a4-183">Register hello Azure AD claims provider tooa user journey</span></span>

<span data-ttu-id="be4a4-184">Du måste nu tooadd hello Azure AD identity providern tooone resor dina användare.</span><span class="sxs-lookup"><span data-stu-id="be4a4-184">You now need tooadd hello Azure AD identity provider tooone of your user journeys.</span></span> <span data-ttu-id="be4a4-185">Nu hello identitetsleverantören har ställts in, men den är inte tillgänglig i någon av hello sign-upp/inloggning skärmar.</span><span class="sxs-lookup"><span data-stu-id="be4a4-185">At this point, hello identity provider has been set up, but it’s not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="be4a4-186">toomake det tillgängliga, vi skapar en dubblett av en befintlig mall användaren resa, och sedan ändra den så att den har även identitetsleverantör hello Azure AD:</span><span class="sxs-lookup"><span data-stu-id="be4a4-186">toomake it available, we will create a duplicate of an existing template user journey, and then modify it so that it also has hello Azure AD identity provider:</span></span>

1. <span data-ttu-id="be4a4-187">Öppna grundläggande hello-filen för principen (till exempel TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="be4a4-187">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
1. <span data-ttu-id="be4a4-188">Hitta hello `<UserJourneys>` element och kopiera hello hela `<UserJourney>` nod som innehåller `Id="SignUpOrSignIn"`.</span><span class="sxs-lookup"><span data-stu-id="be4a4-188">Find hello `<UserJourneys>` element and copy hello entire `<UserJourney>` node that includes `Id="SignUpOrSignIn"`.</span></span>
1. <span data-ttu-id="be4a4-189">Öppna filen hello-tillägg (till exempel TrustFrameworkExtensions.xml) och hitta hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="be4a4-189">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="be4a4-190">Lägg till ett om hello elementet inte finns.</span><span class="sxs-lookup"><span data-stu-id="be4a4-190">If hello element doesn't exist, add one.</span></span>
1. <span data-ttu-id="be4a4-191">Klistra in hello hela `<UserJourney>` nod som du kopierade som underordnad till hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="be4a4-191">Paste hello entire `<UserJourney>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>
1. <span data-ttu-id="be4a4-192">Byt namn på hello-ID för hello nya användare resa (t.ex, Byt namn på det `SignUpOrSignUsingContoso`).</span><span class="sxs-lookup"><span data-stu-id="be4a4-192">Rename hello ID of hello new user journey (for example, rename as `SignUpOrSignUsingContoso`).</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="be4a4-193">Visa hello ”knappen”</span><span class="sxs-lookup"><span data-stu-id="be4a4-193">Display hello "button"</span></span>

<span data-ttu-id="be4a4-194">Hej `<ClaimsProviderSelection>` elementet är detsamma tooan identitet provider-knappen en sign-upp/inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="be4a4-194">hello `<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in screen.</span></span> <span data-ttu-id="be4a4-195">Om du lägger till en `<ClaimsProviderSelection>` element för Azure AD, en ny knapp visas när en användare de hamnar på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="be4a4-195">If you add a `<ClaimsProviderSelection>` element for Azure AD, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="be4a4-196">tooadd det här elementet:</span><span class="sxs-lookup"><span data-stu-id="be4a4-196">tooadd this element:</span></span>

1. <span data-ttu-id="be4a4-197">Hitta hello `<OrchestrationStep>` nod som innehåller `Order="1"` i hello användaren resa som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="be4a4-197">Find hello `<OrchestrationStep>` node that includes `Order="1"` in hello user journey that you just created.</span></span>
1. <span data-ttu-id="be4a4-198">Lägg till hello följande:</span><span class="sxs-lookup"><span data-stu-id="be4a4-198">Add hello following:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. <span data-ttu-id="be4a4-199">Ange `TargetClaimsExchangeId` tooan lämpligt värde.</span><span class="sxs-lookup"><span data-stu-id="be4a4-199">Set `TargetClaimsExchangeId` tooan appropriate value.</span></span> <span data-ttu-id="be4a4-200">Vi rekommenderar följande hello samma konvention som andra:  *\[ClaimProviderName\]Exchange*.</span><span class="sxs-lookup"><span data-stu-id="be4a4-200">We recommend following hello same convention as others: *\[ClaimProviderName\]Exchange*.</span></span>

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="be4a4-201">Länken hello tooan åtgärd</span><span class="sxs-lookup"><span data-stu-id="be4a4-201">Link hello button tooan action</span></span>

<span data-ttu-id="be4a4-202">Nu när du har en knapp på plats måste toolink den tooan åtgärd.</span><span class="sxs-lookup"><span data-stu-id="be4a4-202">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="be4a4-203">hello åtgärd i det här fallet är för Azure AD B2C toocommunicate med Azure AD tooreceive en token.</span><span class="sxs-lookup"><span data-stu-id="be4a4-203">hello action, in this case, is for Azure AD B2C toocommunicate with Azure AD tooreceive a token.</span></span> <span data-ttu-id="be4a4-204">Länka hello tooan åtgärd genom att länka tekniska hello-profil för din Azure AD-anspråksleverantör:</span><span class="sxs-lookup"><span data-stu-id="be4a4-204">Link hello button tooan action by linking hello technical profile for your Azure AD claims provider:</span></span>

1. <span data-ttu-id="be4a4-205">Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="be4a4-205">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
1. <span data-ttu-id="be4a4-206">Lägg till hello följande:</span><span class="sxs-lookup"><span data-stu-id="be4a4-206">Add hello following:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. <span data-ttu-id="be4a4-207">Uppdatera `Id` toohello samma värde som `TargetClaimsExchangeId` i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="be4a4-207">Update `Id` toohello same value as that of `TargetClaimsExchangeId` in hello preceding section.</span></span>
1. <span data-ttu-id="be4a4-208">Uppdatera `TechnicalProfileReferenceId` toohello ID för hello tekniska profilen du skapade tidigare (ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="be4a4-208">Update `TechnicalProfileReferenceId` toohello ID of hello technical profile you created earlier (ContosoProfile).</span></span>

### <a name="upload-hello-updated-extension-file"></a><span data-ttu-id="be4a4-209">Överför hello uppdaterade tilläggsfilen</span><span class="sxs-lookup"><span data-stu-id="be4a4-209">Upload hello updated extension file</span></span>

<span data-ttu-id="be4a4-210">Du är klar ändra hello-tilläggsfilen.</span><span class="sxs-lookup"><span data-stu-id="be4a4-210">You are done modifying hello extension file.</span></span> <span data-ttu-id="be4a4-211">Spara den.</span><span class="sxs-lookup"><span data-stu-id="be4a4-211">Save it.</span></span> <span data-ttu-id="be4a4-212">Överför hello-fil och se till att alla verifieringar lyckas.</span><span class="sxs-lookup"><span data-stu-id="be4a4-212">Then upload hello file, and ensure that all validations succeed.</span></span>

### <a name="update-hello-rp-file"></a><span data-ttu-id="be4a4-213">Uppdatera hello RP-filen</span><span class="sxs-lookup"><span data-stu-id="be4a4-213">Update hello RP file</span></span>

<span data-ttu-id="be4a4-214">Nu behöver du tooupdate hello förlitande part (RP)-fil som initierar hello användaren resa som du just skapat:</span><span class="sxs-lookup"><span data-stu-id="be4a4-214">You now need tooupdate hello relying party (RP) file that will initiate hello user journey that you just created:</span></span>

1. <span data-ttu-id="be4a4-215">Gör en kopia av SignUpOrSignIn.xml i arbetskatalogen och byta namn på den (till exempel byta namn på den tooSignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="be4a4-215">Make a copy of SignUpOrSignIn.xml in your working directory, and rename it (for example, rename it tooSignUpOrSignInWithAAD.xml).</span></span>
1. <span data-ttu-id="be4a4-216">Öppna hello nya fil- och update hello `PolicyId` attribut för `<TrustFrameworkPolicy>` med ett unikt värde (till exempel SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="be4a4-216">Open hello new file and update hello `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value (for example, SignUpOrSignInWithAAD).</span></span> <br> <span data-ttu-id="be4a4-217">Detta blir hello namnet på principen (till exempel B2C\_1A\_SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="be4a4-217">This will be hello name of your policy (for example, B2C\_1A\_SignUpOrSignInWithAAD).</span></span>
1. <span data-ttu-id="be4a4-218">Ändra hello `ReferenceId` attribut i `<DefaultUserJourney>` toomatch hello-ID för hello nya användare resa som du skapade (SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="be4a4-218">Modify hello `ReferenceId` attribute in `<DefaultUserJourney>` toomatch hello ID of hello new user journey that you created (SignUpOrSignUsingContoso).</span></span>
1. <span data-ttu-id="be4a4-219">Spara dina ändringar och överför hello-fil.</span><span class="sxs-lookup"><span data-stu-id="be4a4-219">Save your changes, and upload hello file.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="be4a4-220">Felsökning</span><span class="sxs-lookup"><span data-stu-id="be4a4-220">Troubleshooting</span></span>

<span data-ttu-id="be4a4-221">Testa hello anpassad princip som du just har överfört öppna dess bladet och klicka på **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="be4a4-221">Test hello custom policy that you just uploaded by opening its blade and clicking **Run now**.</span></span> <span data-ttu-id="be4a4-222">toodiagnose problem, Läs mer om [felsökning](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="be4a4-222">toodiagnose problems, read about [troubleshooting](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="be4a4-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be4a4-223">Next steps</span></span>

<span data-ttu-id="be4a4-224">Ge feedback för[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="be4a4-224">Provide feedback too[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>

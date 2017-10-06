---
title: "Azure Active Directory B2C: Lägga till Microsoft-konto (Hanterade) som en identitetsleverantör anpassade principer"
description: "Exempel med hjälp av Microsoft som identitetsleverantör med OpenID Connect (OIDC)-protokollet"
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
ms.openlocfilehash: 577ac612f69015e6790f2fa9f2cfb42c2b524b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a><span data-ttu-id="d7c8c-103">Azure Active Directory B2C: Lägga till Microsoft-konto (Hanterade) som en identitetsleverantör anpassade principer</span><span class="sxs-lookup"><span data-stu-id="d7c8c-103">Azure Active Directory B2C: Add Microsoft Account (MSA) as an identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="d7c8c-104">Den här artikeln lär du dig hur tooenable inloggning för användare från Microsoft-konto (MSA) via hello [anpassade principer](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d7c8c-104">This article shows you how tooenable sign-in for users from Microsoft account (MSA) through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7c8c-105">Krav</span><span class="sxs-lookup"><span data-stu-id="d7c8c-105">Prerequisites</span></span>
<span data-ttu-id="d7c8c-106">Fullständig hello stegen i hello [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="d7c8c-107">De här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-107">These steps include:</span></span>

1.  <span data-ttu-id="d7c8c-108">Skapa ett program för Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-108">Creating a Microsoft account application.</span></span>
2.  <span data-ttu-id="d7c8c-109">Lägga till hello Microsoft-konto programmet viktiga tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="d7c8c-109">Adding hello Microsoft account application key tooAzure AD B2C</span></span>
3.  <span data-ttu-id="d7c8c-110">Lägga till anspråk tooa principen</span><span class="sxs-lookup"><span data-stu-id="d7c8c-110">Adding claims provider tooa policy</span></span>
4.  <span data-ttu-id="d7c8c-111">Registrera hello Account anspråk providern tooa användaren resa</span><span class="sxs-lookup"><span data-stu-id="d7c8c-111">Registering hello Microsoft Account claims provider tooa user journey</span></span>
5.  <span data-ttu-id="d7c8c-112">Överför hello princip tooan Azure AD B2C-klient och testa den</span><span class="sxs-lookup"><span data-stu-id="d7c8c-112">Uploading hello policy tooan Azure AD B2C tenant and test it</span></span>

## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="d7c8c-113">Skapa ett program för Microsoft-konto</span><span class="sxs-lookup"><span data-stu-id="d7c8c-113">Create a Microsoft account application</span></span>
<span data-ttu-id="d7c8c-114">toouse Microsoft-kontot som en identitetsleverantör i Azure Active Directory (AD Azure) B2C kan du behöver toocreate ett program för Microsoft-konto och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-114">toouse Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Microsoft account application and supply it with hello right parameters.</span></span> <span data-ttu-id="d7c8c-115">Du behöver ett microsoftkonto.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-115">You need a Microsoft account.</span></span> <span data-ttu-id="d7c8c-116">Om du inte har någon besöker [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="d7c8c-116">If you don’t have one, visit [https://www.live.com/](https://www.live.com/).</span></span>

1.  <span data-ttu-id="d7c8c-117">Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) och logga in med ditt Microsoft-kontouppgifter.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-117">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2.  <span data-ttu-id="d7c8c-118">Klicka på **Lägg till en app**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-118">Click **Add an app**.</span></span>

    ![Microsoft-konto – Lägg till en app](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  <span data-ttu-id="d7c8c-120">Ange en **namn** för programmet, **kontakta e-post**, avmarkera **vi kan hjälpa dig att komma igång** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-120">Provide a **Name** for your application, **Contact email**, uncheck **Let us help you get started** and click **Create**.</span></span>

    ![Microsoft-konto - registrera ditt program](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  <span data-ttu-id="d7c8c-122">Kopiera hello värdet för **program-Id**. Du behöver den tooconfigure Microsoft-konto som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-122">Copy hello value of **Application Id**. You need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span>

    ![Microsoft-konto – kopiera hello värdet för program-Id](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  <span data-ttu-id="d7c8c-124">Klicka på **Lägg till plattformen**</span><span class="sxs-lookup"><span data-stu-id="d7c8c-124">Click on **Add platform**</span></span>

    ![Microsoft-konto – Lägg till plattformen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  <span data-ttu-id="d7c8c-126">Hello plattformslistan och väljer **Web**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-126">From hello platform list, choose **Web**.</span></span>

    ![Microsoft-konto - hello plattformslistan väljer Web](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  <span data-ttu-id="d7c8c-128">Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **omdirigerings-URI: er** fältet.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-128">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Redirect URIs** field.</span></span> <span data-ttu-id="d7c8c-129">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="d7c8c-129">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>

    ![Microsoft-konto - uppsättningen omdirigerings-URL: er](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  <span data-ttu-id="d7c8c-131">Klicka på **generera nya lösenord** under hello **programmet hemligheter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-131">Click on **Generate New Password** under hello **Application Secrets** section.</span></span> <span data-ttu-id="d7c8c-132">Kopiera hello nytt lösenord visas på skärmen.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-132">Copy hello new password displayed on screen.</span></span> <span data-ttu-id="d7c8c-133">Du behöver den tooconfigure Microsoft-konto som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-133">You need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="d7c8c-134">Lösenordet är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-134">This password is an important security credential.</span></span>

    ![Microsoft-konto – Skapa nytt lösenord](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft-konto – kopiera hello nya lösenord](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  <span data-ttu-id="d7c8c-137">Hello kryssrutan som anger att kontrollsumman **Live SDK stöd** under hello **avancerade alternativ** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-137">Check hello box that says **Live SDK support** under hello **Advanced Options** section.</span></span> <span data-ttu-id="d7c8c-138">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-138">Click **Save**.</span></span>

    ![Microsoft-konto - stöd för Live SDK](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-hello-microsoft-account-application-key-tooazure-ad-b2c"></a><span data-ttu-id="d7c8c-140">Lägg till hello Microsoft-konto programmet viktiga tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="d7c8c-140">Add hello Microsoft account application key tooAzure AD B2C</span></span>
<span data-ttu-id="d7c8c-141">Federation med Microsoft-konton kräver en klienthemlighet för Microsoft-konto tootrust Azure AD B2C uppdrag hello program.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-141">Federation with Microsoft accounts requires a client secret for Microsoft account tootrust Azure AD B2C on behalf of hello application.</span></span> <span data-ttu-id="d7c8c-142">Du behöver toostore Microsoft-konto programmet secert i Azure AD B2C-klient:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-142">You need toostore your Microsoft account application secert in Azure AD B2C tenant:</span></span>   

1.  <span data-ttu-id="d7c8c-143">Gå tooyour Azure AD B2C-klient och använda **B2C inställningar** > **identitet upplevelse Framework**</span><span class="sxs-lookup"><span data-stu-id="d7c8c-143">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="d7c8c-144">Välj **princip nycklar** tooview hello-nycklar som är tillgängliga i din klient.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-144">Select **Policy Keys** tooview hello keys available in your tenant.</span></span>
3.  <span data-ttu-id="d7c8c-145">Klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-145">Click **+Add**.</span></span>
4.  <span data-ttu-id="d7c8c-146">För **alternativ**, använda **manuell**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-146">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="d7c8c-147">För **namn**, Använd `MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-147">For **Name**, use `MSASecret`.</span></span>  
    <span data-ttu-id="d7c8c-148">hello prefixet `B2C_1A_` kan läggas till automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-148">hello prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="d7c8c-149">I hello **hemlighet** ange ditt hemliga för Microsoft-program från https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="d7c8c-149">In hello **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="d7c8c-150">För **nyckelanvändning**, använda **signatur**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-150">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="d7c8c-151">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="d7c8c-151">Click **Create**</span></span>
9.  <span data-ttu-id="d7c8c-152">Bekräfta att du har skapat hello nyckeln `B2C_1A_MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-152">Confirm that you've created hello key `B2C_1A_MSASecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="d7c8c-153">Lägg till en anspråksprovider i din princip för tillägg</span><span class="sxs-lookup"><span data-stu-id="d7c8c-153">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="d7c8c-154">Om du vill användare toosign i med hjälp av Account måste toodefine Account som en anspråksprovider.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-154">If you want users toosign in by using Microsoft Account, you need toodefine Microsoft Account as a claims provider.</span></span> <span data-ttu-id="d7c8c-155">Du behöver med andra ord toospecify en slutpunkt som Azure AD B2C kommunicerar med.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-155">In other words, you need toospecify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="d7c8c-156">hello endpoint innehåller en uppsättning anspråk som används av Azure AD B2C-tooverify som en specifik användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-156">hello endpoint provides a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span>

<span data-ttu-id="d7c8c-157">Definiera Account som en anspråksprovider genom att lägga till `<ClaimsProvider>` nod i tillägget-principfil:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-157">Define Microsoft Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="d7c8c-158">Öppna hello princip tilläggsfilen (TrustFrameworkExtensions.xml) från arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-158">Open hello extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="d7c8c-159">Om du behöver en XML-redigerare [försök Visual Studio Code](https://code.visualstudio.com/download), en enkel plattformsoberoende redigerare.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-159">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="d7c8c-160">Hitta hello `<ClaimsProviders>` avsnitt</span><span class="sxs-lookup"><span data-stu-id="d7c8c-160">Find hello `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="d7c8c-161">Lägg till följande XML-kodstycke under hello `ClaimsProviders` element:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-161">Add following XML snippet under hello `ClaimsProviders` element:</span></span>

    ```xml
<ClaimsProvider>
    <Domain>live.com</Domain>
    <DisplayName>Microsoft Account</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="MSA-OIDC">
        <DisplayName>Microsoft Account</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <Metadata>
        <Item Key="ProviderName">https://login.live.com</Item>
        <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="scope">openid profile email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Microsoft application client id</Item>
        </Metadata>
    <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
    </CryptographicKeys>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="email" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

4.  <span data-ttu-id="d7c8c-162">Ersätt `client_id` värdet med Account programklienten Id</span><span class="sxs-lookup"><span data-stu-id="d7c8c-162">Replace `client_id` value with your Microsoft Account application client Id</span></span>

5.  <span data-ttu-id="d7c8c-163">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-163">Save hello file.</span></span>

## <a name="register-hello-microsoft-account-claims-provider-toosign-up-or-sign-in-user-journey"></a><span data-ttu-id="d7c8c-164">Registrera hello Account anspråk providern tooSign upp eller logga in användaren resa</span><span class="sxs-lookup"><span data-stu-id="d7c8c-164">Register hello Microsoft Account claims provider tooSign up or Sign-in user journey</span></span>

<span data-ttu-id="d7c8c-165">Nu hello identitetsleverantören har ställts in, men den är inte tillgänglig i någon av hello sign-upp/inloggning skärmar.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-165">At this point, hello identity provider has been set up, but it’s not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="d7c8c-166">Nu måste tooadd hello Microsoft Account identitet providern tooyour user `SignUpOrSignIn` användaren resa.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-166">Now you need tooadd hello Microsoft Account identity provider tooyour user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="d7c8c-167">toomake det tillgängliga, skapar vi en dubblett av en befintlig mall användaren resa.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-167">toomake it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="d7c8c-168">Vi Lägg sedan till hello Account identitetsleverantör:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-168">Then we add hello Microsoft Account identity provider:</span></span>

> [!NOTE]
>
><span data-ttu-id="d7c8c-169">Om du tidigare kopierade hello `<UserJourneys>` element från bas-filen för din princip toohello tilläggsfilen `TrustFrameworkExtensions.xml`, kan du hoppa över toothis avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-169">If you previously copied hello `<UserJourneys>` element from base file of your policy toohello extension file `TrustFrameworkExtensions.xml`, you can skip toothis section.</span></span>

1.  <span data-ttu-id="d7c8c-170">Öppna grundläggande hello-filen för principen (till exempel TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="d7c8c-170">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="d7c8c-171">Hitta hello `<UserJourneys>` element och kopiera hela hello-innehållet i `<UserJourneys>` nod.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-171">Find hello `<UserJourneys>` element and copy hello entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="d7c8c-172">Öppna filen hello-tillägg (till exempel TrustFrameworkExtensions.xml) och hitta hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-172">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="d7c8c-173">Lägg till ett om hello elementet inte finns.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-173">If hello element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="d7c8c-174">Klistra in hello hela innehållet i `<UserJournesy>` nod som du kopierade som underordnad till hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-174">Paste hello entire content of `<UserJournesy>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="d7c8c-175">Visa hello-knappen</span><span class="sxs-lookup"><span data-stu-id="d7c8c-175">Display hello button</span></span>
<span data-ttu-id="d7c8c-176">Hej `<ClaimsProviderSelections>` elementet definierar hello lista med alternativ för val av anspråk providern och deras inbördes ordning.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-176">hello `<ClaimsProviderSelections>` element defines hello list of claims provider selection options and their order.</span></span>  <span data-ttu-id="d7c8c-177">`<ClaimsProviderSelection>`elementet är detsamma tooan identitet provider-knappen en sign-upp/inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-177">`<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="d7c8c-178">Om du lägger till en `<ClaimsProviderSelection>` element för Microsoft-konto, en ny knapp visas när en användare de hamnar på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-178">If you add a `<ClaimsProviderSelection>` element for Microsoft account, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="d7c8c-179">tooadd det här elementet:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-179">tooadd this element:</span></span>

1.  <span data-ttu-id="d7c8c-180">Hitta hello `<UserJourney>` nod som innehåller `Id="SignUpOrSignIn"` i hello användaren resa som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-180">Find hello `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in hello user journey that you copied.</span></span>
2.  <span data-ttu-id="d7c8c-181">Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="d7c8c-181">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="d7c8c-182">Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-182">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="d7c8c-183">Länken hello tooan åtgärd</span><span class="sxs-lookup"><span data-stu-id="d7c8c-183">Link hello button tooan action</span></span>
<span data-ttu-id="d7c8c-184">Nu när du har en knapp på plats måste toolink den tooan åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-184">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="d7c8c-185">hello åtgärd i det här fallet är för Azure AD B2C toocommunicate med Account tooreceive en token.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-185">hello action, in this case, is for Azure AD B2C toocommunicate with Microsoft Account tooreceive a token.</span></span> <span data-ttu-id="d7c8c-186">Länka hello tooan åtgärd genom att länka tekniska hello-profil för din Account anspråksleverantör:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-186">Link hello button tooan action by linking hello technical profile for your Microsoft Account claims provider:</span></span>

1.  <span data-ttu-id="d7c8c-187">Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-187">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="d7c8c-188">Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-188">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * <span data-ttu-id="d7c8c-189">Se till att hello `Id` har samma värde som hello `TargetClaimsExchangeId` i föregående avsnitt hello</span><span class="sxs-lookup"><span data-stu-id="d7c8c-189">Ensure hello `Id` has hello same value as that of `TargetClaimsExchangeId` in hello preceding section</span></span>
>   * <span data-ttu-id="d7c8c-190">Se till att `TechnicalProfileReferenceId` -ID har angetts toohello tekniska profilen du skapade tidigare (MSA-OIDC).</span><span class="sxs-lookup"><span data-stu-id="d7c8c-190">Ensure `TechnicalProfileReferenceId` ID is set toohello technical profile you created earlier (MSA-OIDC).</span></span>

## <a name="upload-hello-policy-tooyour-tenant"></a><span data-ttu-id="d7c8c-191">Överför hello princip tooyour klient</span><span class="sxs-lookup"><span data-stu-id="d7c8c-191">Upload hello policy tooyour tenant</span></span>
1.  <span data-ttu-id="d7c8c-192">I hello [Azure-portalen](https://portal.azure.com), växla till hello [kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och öppna hello **Azure AD B2C** bladet.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-192">In hello [Azure portal](https://portal.azure.com), switch into hello [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open hello **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="d7c8c-193">Välj **identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-193">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="d7c8c-194">Öppna hello **alla principer** bladet.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-194">Open hello **All Policies** blade.</span></span>
4.  <span data-ttu-id="d7c8c-195">Välj **överföra princip**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-195">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="d7c8c-196">Kontrollera **skriva över hello principen om den finns** rutan.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-196">Check **Overwrite hello policy if it exists** box.</span></span>
6.  <span data-ttu-id="d7c8c-197">**Överför** TrustFrameworkExtensions.xml och se till att inte misslyckas verifieringen hello</span><span class="sxs-lookup"><span data-stu-id="d7c8c-197">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail hello validation</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="d7c8c-198">Testa hello anpassad princip med Kör nu</span><span class="sxs-lookup"><span data-stu-id="d7c8c-198">Test hello custom policy by using Run Now</span></span>

1.  <span data-ttu-id="d7c8c-199">Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-199">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
> [!NOTE]
>
><span data-ttu-id="d7c8c-200">**Kör nu** kräver minst ett program toobe preregistered hello innehavaren.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-200">**Run now** requires at least one application toobe preregistered on hello tenant.</span></span> <span data-ttu-id="d7c8c-201">toolearn hur tooregister program finns hello Azure AD B2C [Kom igång](active-directory-b2c-get-started.md) artikel eller hello [appregistrering](active-directory-b2c-app-registration.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-201">toolearn how tooregister applications, see hello Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or hello [Application registration](active-directory-b2c-app-registration.md) article.</span></span>
2.  <span data-ttu-id="d7c8c-202">Öppna **B2C_1A_signup_signin**, hello förlitande part (RP) anpassad princip som du överfört.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-202">Open **B2C_1A_signup_signin**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="d7c8c-203">Välj **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-203">Select **Run now**.</span></span>
3.  <span data-ttu-id="d7c8c-204">Du ska kunna toosign in med Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-204">You should be able toosign in using Microsoft account.</span></span>

## <a name="optional-register-hello-microsoft-account-claims-provider-tooprofile-edit-user-journey"></a><span data-ttu-id="d7c8c-205">[Valfritt] Registrera hello Account anspråk providern tooProfile Redigera användare resa</span><span class="sxs-lookup"><span data-stu-id="d7c8c-205">[Optional] Register hello Microsoft Account claims provider tooProfile-Edit user journey</span></span>
<span data-ttu-id="d7c8c-206">Vill du kanske tooadd hello Account identitetsleverantör också tooyour användaren `ProfileEdit` användaren resa.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-206">You may want tooadd hello Microsoft Account identity provider also tooyour user `ProfileEdit` user journey.</span></span> <span data-ttu-id="d7c8c-207">toomake den är tillgänglig, vi Upprepa hello senaste två steg:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-207">toomake it available, we repeat hello last two steps:</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="d7c8c-208">Visa hello-knappen</span><span class="sxs-lookup"><span data-stu-id="d7c8c-208">Display hello button</span></span>
1.  <span data-ttu-id="d7c8c-209">Öppna filen för hello tillägg av principen (till exempel TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="d7c8c-209">Open hello extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="d7c8c-210">Hitta hello `<UserJourney>` nod som innehåller `Id="ProfileEdit"` i hello användaren resa som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-210">Find hello `<UserJourney>` node that includes `Id="ProfileEdit"` in hello user journey that you copied.</span></span>
3.  <span data-ttu-id="d7c8c-211">Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="d7c8c-211">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="d7c8c-212">Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-212">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="d7c8c-213">Länken hello tooan åtgärd</span><span class="sxs-lookup"><span data-stu-id="d7c8c-213">Link hello button tooan action</span></span>
1.  <span data-ttu-id="d7c8c-214">Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-214">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="d7c8c-215">Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:</span><span class="sxs-lookup"><span data-stu-id="d7c8c-215">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="d7c8c-216">Testa hello anpassad profil-Redigera princip genom att använda Kör nu</span><span class="sxs-lookup"><span data-stu-id="d7c8c-216">Test hello custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="d7c8c-217">Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-217">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="d7c8c-218">Öppna **B2C_1A_ProfileEdit**, hello förlitande part (RP) anpassad princip som du överfört.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-218">Open **B2C_1A_ProfileEdit**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="d7c8c-219">Välj **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-219">Select **Run now**.</span></span>
3.  <span data-ttu-id="d7c8c-220">Du ska kunna toosign in med Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-220">You should be able toosign in using Microsoft account.</span></span>

## <a name="download-hello-complete-policy-files"></a><span data-ttu-id="d7c8c-221">Hämta hello fullständig principfiler</span><span class="sxs-lookup"><span data-stu-id="d7c8c-221">Download hello complete policy files</span></span>
<span data-ttu-id="d7c8c-222">Valfritt: Vi rekommenderar att du skapar ditt scenario med din egen anpassade principfiler när du har slutfört hello komma igång med anpassade principer igenom istället för att använda dessa exempelfiler.</span><span class="sxs-lookup"><span data-stu-id="d7c8c-222">Optional: We recommend you build your scenario using your own Custom policy files after completing hello Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="d7c8c-223">Exempelfiler för principen för referens</span><span class="sxs-lookup"><span data-stu-id="d7c8c-223">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)

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
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a><span data-ttu-id="de67a-103">Azure Active Directory B2C: Lägga till Google + som en OAuth2-identitetsleverantör anpassade principer</span><span class="sxs-lookup"><span data-stu-id="de67a-103">Azure Active Directory B2C: Add Google+ as an OAuth2 identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="de67a-104">Den här guiden visar hur tooenable inloggning för användare från Google + konto via hello [anpassade principer](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="de67a-104">This guide shows you how tooenable sign-in for users from Google+ account through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de67a-105">Krav</span><span class="sxs-lookup"><span data-stu-id="de67a-105">Prerequisites</span></span>

<span data-ttu-id="de67a-106">Fullständig hello stegen i hello [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="de67a-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="de67a-107">De här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="de67a-107">These steps include:</span></span>

1.  <span data-ttu-id="de67a-108">Skapa ett Google + konto program.</span><span class="sxs-lookup"><span data-stu-id="de67a-108">Creating a Google+ account application.</span></span>
2.  <span data-ttu-id="de67a-109">Lägga till hello Google + konto programmet viktiga tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="de67a-109">Adding hello Google+ account application key tooAzure AD B2C</span></span>
3.  <span data-ttu-id="de67a-110">Lägga till anspråk tooa principen</span><span class="sxs-lookup"><span data-stu-id="de67a-110">Adding claims provider tooa policy</span></span>
4.  <span data-ttu-id="de67a-111">Registrera hello Google + konto anspråk providern tooa användaren resa</span><span class="sxs-lookup"><span data-stu-id="de67a-111">Registering hello Google+ account claims provider tooa user journey</span></span>
5.  <span data-ttu-id="de67a-112">Överför hello princip tooan Azure AD B2C-klient och testa den</span><span class="sxs-lookup"><span data-stu-id="de67a-112">Uploading hello policy tooan Azure AD B2C tenant and test it</span></span>

## <a name="create-a-google-account-application"></a><span data-ttu-id="de67a-113">Skapa ett Google + konto program</span><span class="sxs-lookup"><span data-stu-id="de67a-113">Create a Google+ account application</span></span>
<span data-ttu-id="de67a-114">toouse Google + som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate Google +-programmet och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="de67a-114">toouse Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Google+ application and supply it with hello right parameters.</span></span> <span data-ttu-id="de67a-115">Du kan registrera en Google +-programmet här: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span><span class="sxs-lookup"><span data-stu-id="de67a-115">You can register a Google+ application here: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span></span>

1.  <span data-ttu-id="de67a-116">Gå toohello [Google utvecklare konsolen](https://console.developers.google.com/) och logga in med Google + autentiseringsuppgifterna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="de67a-116">Go toohello [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2.  <span data-ttu-id="de67a-117">Klicka på **skapa projekt**, ange en **projektnamn**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="de67a-117">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>

3.  <span data-ttu-id="de67a-118">Klicka på hello **menyn projekt**.</span><span class="sxs-lookup"><span data-stu-id="de67a-118">Click on hello **Projects menu**.</span></span>

    ![Google +-konto – Välj projekt](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  <span data-ttu-id="de67a-120">Klicka på hello  **+**  knappen.</span><span class="sxs-lookup"><span data-stu-id="de67a-120">Click on hello **+** button.</span></span>

    ![Google + konto – Skapa nytt projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  <span data-ttu-id="de67a-122">Ange en **projektnamn**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="de67a-122">Enter a **Project name**, and then click **Create**.</span></span>

    ![Google +-konto – nytt projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  <span data-ttu-id="de67a-124">Vänta tills hello projektet är klar och klicka på hello **menyn projekt**.</span><span class="sxs-lookup"><span data-stu-id="de67a-124">Wait until hello project is ready and click on hello **Projects menu**.</span></span>

    ![Google + konto - vänta tills det nya projektet är klar toouse](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  <span data-ttu-id="de67a-126">Klicka på ditt projektnamn.</span><span class="sxs-lookup"><span data-stu-id="de67a-126">Click on your project name.</span></span>

    ![Google +-konto – Välj hello nytt projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  <span data-ttu-id="de67a-128">Klicka på **API-hanterare** och klicka sedan på **autentiseringsuppgifter** i hello vänstra navigeringsrutan.</span><span class="sxs-lookup"><span data-stu-id="de67a-128">Click **API Manager** and then click **Credentials** in hello left navigation.</span></span>
9.  <span data-ttu-id="de67a-129">Klicka på hello **OAuth-medgivande skärmen** fliken hello överst.</span><span class="sxs-lookup"><span data-stu-id="de67a-129">Click hello **OAuth consent screen** tab at hello top.</span></span>

    ![Google +-konto – ange OAuth-medgivande skärmen](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  <span data-ttu-id="de67a-131">Välj eller ange en giltig **e-postadress**, ange en **produktnamn**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="de67a-131">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>

    ![Google + - programmet autentiseringsuppgifter](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  <span data-ttu-id="de67a-133">Klicka på **nya autentiseringsuppgifter** och välj sedan **OAuth-klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="de67a-133">Click **New credentials** and then choose **OAuth client ID**.</span></span>

    ![Google + - skapa nya autentiseringsuppgifter för programmet](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  <span data-ttu-id="de67a-135">Under **programtyp**väljer **webbprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="de67a-135">Under **Application type**, select **Web application**.</span></span>

    ![Google + – Välj apptyp](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  <span data-ttu-id="de67a-137">Ange en **namn** för ditt program ange `https://login.microsoftonline.com` i hello **behörighet JavaScript ursprung** fältet och `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **auktoriserad omdirigerings-URI: er**fält.</span><span class="sxs-lookup"><span data-stu-id="de67a-137">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in hello **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Authorized redirect URIs** field.</span></span> <span data-ttu-id="de67a-138">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="de67a-138">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="de67a-139">Hej **{klient}** värdet är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="de67a-139">hello **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="de67a-140">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="de67a-140">Click **Create**.</span></span>

    ![Google + - ge behörighet JavaScript ursprung och omdirigerings-URI: er](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  <span data-ttu-id="de67a-142">Kopiera hello värdena för **klient-Id** och **klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="de67a-142">Copy hello values of **Client Id** and **Client secret**.</span></span> <span data-ttu-id="de67a-143">Du måste båda tooconfigure Google + som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="de67a-143">You need both tooconfigure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="de67a-144">**Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="de67a-144">**Client secret** is an important security credential.</span></span>

    ![Google + - kopia hello värden för klient-Id och klienten hemligt](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-hello-google-account-application-key-tooazure-ad-b2c"></a><span data-ttu-id="de67a-146">Lägg till hello Google + konto programmet viktiga tooAzure AD B2C</span><span class="sxs-lookup"><span data-stu-id="de67a-146">Add hello Google+ account application key tooAzure AD B2C</span></span>
<span data-ttu-id="de67a-147">Federation med Google + konton kräver en klienthemlighet för Google + konto tootrust Azure AD B2C uppdrag hello program.</span><span class="sxs-lookup"><span data-stu-id="de67a-147">Federation with Google+ accounts requires a client secret for Google+ account tootrust Azure AD B2C on behalf of hello application.</span></span> <span data-ttu-id="de67a-148">Du behöver toostore din Google + programhemlighet i Azure AD B2C-klient:</span><span class="sxs-lookup"><span data-stu-id="de67a-148">You need toostore your Google+ application secret in Azure AD B2C tenant:</span></span>  

1.  <span data-ttu-id="de67a-149">Gå tooyour Azure AD B2C-klient och använda **B2C inställningar** > **identitet upplevelse Framework**</span><span class="sxs-lookup"><span data-stu-id="de67a-149">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="de67a-150">Välj **princip nycklar** tooview hello-nycklar som är tillgängliga i din klient.</span><span class="sxs-lookup"><span data-stu-id="de67a-150">Select **Policy Keys** tooview hello keys available in your tenant.</span></span>
3.  <span data-ttu-id="de67a-151">Klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="de67a-151">Click **+Add**.</span></span>
4.  <span data-ttu-id="de67a-152">För **alternativ**, använda **manuell**.</span><span class="sxs-lookup"><span data-stu-id="de67a-152">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="de67a-153">För **namn**, Använd `GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="de67a-153">For **Name**, use `GoogleSecret`.</span></span>  
    <span data-ttu-id="de67a-154">hello prefixet `B2C_1A_` kan läggas till automatiskt.</span><span class="sxs-lookup"><span data-stu-id="de67a-154">hello prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="de67a-155">I hello **hemlighet** ange ditt hemliga för Microsoft-program från https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="de67a-155">In hello **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="de67a-156">För **nyckelanvändning**, använda **signatur**.</span><span class="sxs-lookup"><span data-stu-id="de67a-156">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="de67a-157">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="de67a-157">Click **Create**</span></span>
9.  <span data-ttu-id="de67a-158">Bekräfta att du har skapat hello nyckeln `B2C_1A_GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="de67a-158">Confirm that you've created hello key `B2C_1A_GoogleSecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="de67a-159">Lägg till en anspråksprovider i din princip för tillägg</span><span class="sxs-lookup"><span data-stu-id="de67a-159">Add a claims provider in your extension policy</span></span>

<span data-ttu-id="de67a-160">Om du vill användare toosign i med hjälp av Google + konto behöver du toodefine Google + konto som en anspråksprovider.</span><span class="sxs-lookup"><span data-stu-id="de67a-160">If you want users toosign in by using Google+ account, you need toodefine Google+ account as a claims provider.</span></span> <span data-ttu-id="de67a-161">Du behöver med andra ord toospecify en slutpunkt som Azure AD B2C kommunicerar med.</span><span class="sxs-lookup"><span data-stu-id="de67a-161">In other words, you need toospecify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="de67a-162">hello endpoint innehåller en uppsättning anspråk som används av Azure AD B2C-tooverify som en specifik användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="de67a-162">hello endpoint provides a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span>

<span data-ttu-id="de67a-163">Definiera Google + kontot som en anspråksprovider genom att lägga till `<ClaimsProvider>` nod i tillägget-principfil:</span><span class="sxs-lookup"><span data-stu-id="de67a-163">Define Google+ Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="de67a-164">Öppna hello princip tilläggsfilen (TrustFrameworkExtensions.xml) från arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="de67a-164">Open hello extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="de67a-165">Om du behöver en XML-redigerare [försök Visual Studio Code](https://code.visualstudio.com/download), en enkel plattformsoberoende redigerare.</span><span class="sxs-lookup"><span data-stu-id="de67a-165">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="de67a-166">Hitta hello `<ClaimsProviders>` avsnitt</span><span class="sxs-lookup"><span data-stu-id="de67a-166">Find hello `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="de67a-167">Lägg till följande XML-kodstycke under hello hello `ClaimsProviders` element och Ersätt `client_id` värdet med Google + konto programmet klient-ID innan du sparar hello-filen.</span><span class="sxs-lookup"><span data-stu-id="de67a-167">Add hello following XML snippet under hello `ClaimsProviders` element and replace `client_id` value with your Google+ account application client ID before saving hello file.</span></span>  

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

## <a name="register-hello-google-account-claims-provider-toosign-up-or-sign-in-user-journey"></a><span data-ttu-id="de67a-168">Registrera hello Google + konto anspråk providern tooSign upp eller logga in användaren resa</span><span class="sxs-lookup"><span data-stu-id="de67a-168">Register hello Google+ account claims provider tooSign up or Sign in user journey</span></span>

<span data-ttu-id="de67a-169">hello identitetsleverantören har ställts in.</span><span class="sxs-lookup"><span data-stu-id="de67a-169">hello identity provider has been set up.</span></span>  <span data-ttu-id="de67a-170">Men finns det inte i någon av hello sign-upp/inloggning skärmar.</span><span class="sxs-lookup"><span data-stu-id="de67a-170">However, it is not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="de67a-171">Lägg till hello Google + identitet providern tooyour användarkonto `SignUpOrSignIn` användaren resa.</span><span class="sxs-lookup"><span data-stu-id="de67a-171">Add hello Google+ account identity provider tooyour user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="de67a-172">toomake det tillgängliga, skapar vi en dubblett av en befintlig mall användaren resa.</span><span class="sxs-lookup"><span data-stu-id="de67a-172">toomake it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="de67a-173">Vi Lägg sedan till hello Google + konto identitetsleverantör:</span><span class="sxs-lookup"><span data-stu-id="de67a-173">Then we add hello Google+ account identity provider:</span></span>

>[!NOTE]
>
><span data-ttu-id="de67a-174">Om du har kopierat hello `<UserJourneys>` element från bas-fil för din princip toohello (TrustFrameworkExtensions.xml), kan du hoppa över toothis avsnittet.</span><span class="sxs-lookup"><span data-stu-id="de67a-174">If you copied hello `<UserJourneys>` element from base file of your policy toohello extension file (TrustFrameworkExtensions.xml), you can skip toothis section.</span></span>

1.  <span data-ttu-id="de67a-175">Öppna grundläggande hello-filen för principen (till exempel TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="de67a-175">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="de67a-176">Hitta hello `<UserJourneys>` element och kopiera hela hello-innehållet i `<UserJourneys>` nod.</span><span class="sxs-lookup"><span data-stu-id="de67a-176">Find hello `<UserJourneys>` element and copy hello entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="de67a-177">Öppna filen hello-tillägg (till exempel TrustFrameworkExtensions.xml) och hitta hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="de67a-177">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="de67a-178">Lägg till ett om hello elementet inte finns.</span><span class="sxs-lookup"><span data-stu-id="de67a-178">If hello element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="de67a-179">Klistra in hello hela innehållet i `<UserJournesy>` nod som du kopierade som underordnad till hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="de67a-179">Paste hello entire content of `<UserJournesy>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="de67a-180">Visa hello-knappen</span><span class="sxs-lookup"><span data-stu-id="de67a-180">Display hello button</span></span>
<span data-ttu-id="de67a-181">Hej `<ClaimsProviderSelections>` elementet definierar hello lista med alternativ för val av anspråk providern och deras inbördes ordning.</span><span class="sxs-lookup"><span data-stu-id="de67a-181">hello `<ClaimsProviderSelections>` element defines hello list of claims provider selection options and their order.</span></span>  <span data-ttu-id="de67a-182">`<ClaimsProviderSelection>`elementet är detsamma tooan identitet provider-knappen en sign-upp/inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="de67a-182">`<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="de67a-183">Om du lägger till en `<ClaimsProviderSelection>` element för Google + konto, en ny knapp visas när en användare de hamnar på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="de67a-183">If you add a `<ClaimsProviderSelection>` element for Google+ account, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="de67a-184">tooadd det här elementet:</span><span class="sxs-lookup"><span data-stu-id="de67a-184">tooadd this element:</span></span>

1.  <span data-ttu-id="de67a-185">Hitta hello `<UserJourney>` nod som innehåller `Id="SignUpOrSignIn"` i hello användaren resa som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="de67a-185">Find hello `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in hello user journey that you copied.</span></span>
2.  <span data-ttu-id="de67a-186">Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="de67a-186">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="de67a-187">Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:</span><span class="sxs-lookup"><span data-stu-id="de67a-187">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="de67a-188">Länken hello tooan åtgärd</span><span class="sxs-lookup"><span data-stu-id="de67a-188">Link hello button tooan action</span></span>
<span data-ttu-id="de67a-189">Nu när du har en knapp på plats måste toolink den tooan åtgärd.</span><span class="sxs-lookup"><span data-stu-id="de67a-189">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="de67a-190">hello åtgärd i det här fallet är för Azure AD B2C toocommunicate med Google + konto tooreceive en token.</span><span class="sxs-lookup"><span data-stu-id="de67a-190">hello action, in this case, is for Azure AD B2C toocommunicate with Google+ account tooreceive a token.</span></span>

1.  <span data-ttu-id="de67a-191">Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="de67a-191">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="de67a-192">Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:</span><span class="sxs-lookup"><span data-stu-id="de67a-192">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * <span data-ttu-id="de67a-193">Se till att hello `Id` har samma värde som hello `TargetClaimsExchangeId` i föregående avsnitt hello</span><span class="sxs-lookup"><span data-stu-id="de67a-193">Ensure hello `Id` has hello same value as that of `TargetClaimsExchangeId` in hello preceding section</span></span>
> * <span data-ttu-id="de67a-194">Se till att `TechnicalProfileReferenceId` -ID har angetts toohello tekniska profilen du skapade tidigare (Google-OAUTH).</span><span class="sxs-lookup"><span data-stu-id="de67a-194">Ensure `TechnicalProfileReferenceId` ID is set toohello technical profile you created earlier (Google-OAUTH).</span></span>

## <a name="upload-hello-policy-tooyour-tenant"></a><span data-ttu-id="de67a-195">Överför hello princip tooyour klient</span><span class="sxs-lookup"><span data-stu-id="de67a-195">Upload hello policy tooyour tenant</span></span>
1.  <span data-ttu-id="de67a-196">I hello [Azure-portalen](https://portal.azure.com), växla till hello [kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och öppna hello **Azure AD B2C** bladet.</span><span class="sxs-lookup"><span data-stu-id="de67a-196">In hello [Azure portal](https://portal.azure.com), switch into hello [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open hello **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="de67a-197">Välj **identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="de67a-197">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="de67a-198">Öppna hello **alla principer** bladet.</span><span class="sxs-lookup"><span data-stu-id="de67a-198">Open hello **All Policies** blade.</span></span>
4.  <span data-ttu-id="de67a-199">Välj **överföra princip**.</span><span class="sxs-lookup"><span data-stu-id="de67a-199">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="de67a-200">Kontrollera **skriva över hello principen om den finns** rutan.</span><span class="sxs-lookup"><span data-stu-id="de67a-200">Check **Overwrite hello policy if it exists** box.</span></span>
6.  <span data-ttu-id="de67a-201">**Överför** TrustFrameworkExtensions.xml och se till att inte misslyckas verifieringen hello</span><span class="sxs-lookup"><span data-stu-id="de67a-201">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail hello validation</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="de67a-202">Testa hello anpassad princip med Kör nu</span><span class="sxs-lookup"><span data-stu-id="de67a-202">Test hello custom policy by using Run Now</span></span>
1.  <span data-ttu-id="de67a-203">Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="de67a-203">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>

    >[!NOTE]
    >
    >    <span data-ttu-id="de67a-204">**Kör nu** kräver minst ett program toobe preregistered hello innehavaren.</span><span class="sxs-lookup"><span data-stu-id="de67a-204">**Run now** requires at least one application toobe preregistered on hello tenant.</span></span> 
    >    <span data-ttu-id="de67a-205">toolearn hur tooregister program finns hello Azure AD B2C [Kom igång](active-directory-b2c-get-started.md) artikel eller hello [appregistrering](active-directory-b2c-app-registration.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="de67a-205">toolearn how tooregister applications, see hello Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or hello [Application registration](active-directory-b2c-app-registration.md) article.</span></span>


2.  <span data-ttu-id="de67a-206">Öppna **B2C_1A_signup_signin**, hello förlitande part (RP) anpassad princip som du överfört.</span><span class="sxs-lookup"><span data-stu-id="de67a-206">Open **B2C_1A_signup_signin**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="de67a-207">Välj **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="de67a-207">Select **Run now**.</span></span>
3.  <span data-ttu-id="de67a-208">Du ska kunna toosign in med Google +.</span><span class="sxs-lookup"><span data-stu-id="de67a-208">You should be able toosign in using Google+ account.</span></span>

## <a name="optional-register-hello-google-account-claims-provider-tooprofile-edit-user-journey"></a><span data-ttu-id="de67a-209">[Valfritt] Registrera hello Google + konto anspråk providern tooProfile Redigera användare resa</span><span class="sxs-lookup"><span data-stu-id="de67a-209">[Optional] Register hello Google+ account claims provider tooProfile-Edit user journey</span></span>
<span data-ttu-id="de67a-210">Vill du kanske tooadd hello Google + konto identitetsleverantör också tooyour användaren `ProfileEdit` användaren resa.</span><span class="sxs-lookup"><span data-stu-id="de67a-210">You may want tooadd hello Google+ account identity provider also tooyour user `ProfileEdit` user journey.</span></span> <span data-ttu-id="de67a-211">toomake den är tillgänglig, vi Upprepa hello senaste två steg:</span><span class="sxs-lookup"><span data-stu-id="de67a-211">toomake it available, we repeat hello last two steps:</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="de67a-212">Visa hello-knappen</span><span class="sxs-lookup"><span data-stu-id="de67a-212">Display hello button</span></span>
1.  <span data-ttu-id="de67a-213">Öppna filen för hello tillägg av principen (till exempel TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="de67a-213">Open hello extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="de67a-214">Hitta hello `<UserJourney>` nod som innehåller `Id="ProfileEdit"` i hello användaren resa som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="de67a-214">Find hello `<UserJourney>` node that includes `Id="ProfileEdit"` in hello user journey that you copied.</span></span>
3.  <span data-ttu-id="de67a-215">Leta upp hello `<OrchestrationStep>` nod som innehåller`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="de67a-215">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="de67a-216">Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:</span><span class="sxs-lookup"><span data-stu-id="de67a-216">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="de67a-217">Länken hello tooan åtgärd</span><span class="sxs-lookup"><span data-stu-id="de67a-217">Link hello button tooan action</span></span>
1.  <span data-ttu-id="de67a-218">Hitta hello `<OrchestrationStep>` som innehåller `Order="2"` i hello `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="de67a-218">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="de67a-219">Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:</span><span class="sxs-lookup"><span data-stu-id="de67a-219">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="de67a-220">Testa hello anpassad profil-Redigera princip genom att använda Kör nu</span><span class="sxs-lookup"><span data-stu-id="de67a-220">Test hello custom Profile-Edit policy by using Run Now</span></span>

1.  <span data-ttu-id="de67a-221">Öppna **Azure AD B2C inställningar** och gå för**identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="de67a-221">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="de67a-222">Öppna **B2C_1A_ProfileEdit**, hello förlitande part (RP) anpassad princip som du överfört.</span><span class="sxs-lookup"><span data-stu-id="de67a-222">Open **B2C_1A_ProfileEdit**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="de67a-223">Välj **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="de67a-223">Select **Run now**.</span></span>
3.  <span data-ttu-id="de67a-224">Du ska kunna toosign in med Google +.</span><span class="sxs-lookup"><span data-stu-id="de67a-224">You should be able toosign in using Google+ account.</span></span>

## <a name="download-hello-complete-policy-files"></a><span data-ttu-id="de67a-225">Hämta hello fullständig principfiler</span><span class="sxs-lookup"><span data-stu-id="de67a-225">Download hello complete policy files</span></span>
<span data-ttu-id="de67a-226">Valfritt: Vi rekommenderar att du skapar ditt scenario med din egen anpassade principfiler när du har slutfört hello komma igång med anpassade principer igenom istället för att använda dessa exempelfiler.</span><span class="sxs-lookup"><span data-stu-id="de67a-226">Optional: We recommend you build your scenario using your own Custom policy files after completing hello Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="de67a-227">Exempelfiler för principen för referens</span><span class="sxs-lookup"><span data-stu-id="de67a-227">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)

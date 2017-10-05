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
ms.openlocfilehash: 8c981046ff41d3927ff60d6dc4f40366ae25ba74
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a><span data-ttu-id="d196e-103">Azure Active Directory B2C: Lägga till Microsoft-konto (Hanterade) som en identitetsleverantör anpassade principer</span><span class="sxs-lookup"><span data-stu-id="d196e-103">Azure Active Directory B2C: Add Microsoft Account (MSA) as an identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="d196e-104">Den här artikeln visar hur du aktiverar inloggning för användare från Microsoft-konto (MSA) med [anpassade principer](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d196e-104">This article shows you how to enable sign-in for users from Microsoft account (MSA) through the use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d196e-105">Krav</span><span class="sxs-lookup"><span data-stu-id="d196e-105">Prerequisites</span></span>
<span data-ttu-id="d196e-106">Utför stegen i den [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="d196e-106">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="d196e-107">De här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="d196e-107">These steps include:</span></span>

1.  <span data-ttu-id="d196e-108">Skapa ett program för Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="d196e-108">Creating a Microsoft account application.</span></span>
2.  <span data-ttu-id="d196e-109">Lägga till nyckeln programmet Microsoft-konto i Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="d196e-109">Adding the Microsoft account application key to Azure AD B2C</span></span>
3.  <span data-ttu-id="d196e-110">Att lägga till anspråksprovidern för en princip</span><span class="sxs-lookup"><span data-stu-id="d196e-110">Adding claims provider to a policy</span></span>
4.  <span data-ttu-id="d196e-111">Registrera Account anspråksprovidern för en användare resa</span><span class="sxs-lookup"><span data-stu-id="d196e-111">Registering the Microsoft Account claims provider to a user journey</span></span>
5.  <span data-ttu-id="d196e-112">Ladda upp principen till en Azure AD B2C-klient och testa den</span><span class="sxs-lookup"><span data-stu-id="d196e-112">Uploading the policy to an Azure AD B2C tenant and test it</span></span>

## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="d196e-113">Skapa ett program för Microsoft-konto</span><span class="sxs-lookup"><span data-stu-id="d196e-113">Create a Microsoft account application</span></span>
<span data-ttu-id="d196e-114">Om du vill använda Microsoft-konto som en identitetsleverantör i Azure Active Directory (AD Azure) B2C måste du skapa ett program för Microsoft-konto och ange rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="d196e-114">To use Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Microsoft account application and supply it with the right parameters.</span></span> <span data-ttu-id="d196e-115">Du behöver ett microsoftkonto.</span><span class="sxs-lookup"><span data-stu-id="d196e-115">You need a Microsoft account.</span></span> <span data-ttu-id="d196e-116">Om du inte har någon besöker [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="d196e-116">If you don’t have one, visit [https://www.live.com/](https://www.live.com/).</span></span>

1.  <span data-ttu-id="d196e-117">Gå till den [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) och logga in med ditt Microsoft-kontouppgifter.</span><span class="sxs-lookup"><span data-stu-id="d196e-117">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2.  <span data-ttu-id="d196e-118">Klicka på **Lägg till en app**.</span><span class="sxs-lookup"><span data-stu-id="d196e-118">Click **Add an app**.</span></span>

    ![Microsoft-konto – Lägg till en app](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  <span data-ttu-id="d196e-120">Ange en **namn** för programmet, **kontakta e-post**, avmarkera **vi kan hjälpa dig att komma igång** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="d196e-120">Provide a **Name** for your application, **Contact email**, uncheck **Let us help you get started** and click **Create**.</span></span>

    ![Microsoft-konto - registrera ditt program](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  <span data-ttu-id="d196e-122">Kopiera värdet för **program-Id**. Du behöver det för att konfigurera Microsoft-konto som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="d196e-122">Copy the value of **Application Id**. You need it to configure Microsoft account as an identity provider in your tenant.</span></span>

    ![Microsoft-konto – Kopiera värdet för program-Id](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  <span data-ttu-id="d196e-124">Klicka på **Lägg till plattformen**</span><span class="sxs-lookup"><span data-stu-id="d196e-124">Click on **Add platform**</span></span>

    ![Microsoft-konto – Lägg till plattformen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  <span data-ttu-id="d196e-126">Plattformslistan och väljer **Web**.</span><span class="sxs-lookup"><span data-stu-id="d196e-126">From the platform list, choose **Web**.</span></span>

    ![Microsoft-konto - plattformslistan väljer Web](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  <span data-ttu-id="d196e-128">Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i den **omdirigerings-URI: er** fältet.</span><span class="sxs-lookup"><span data-stu-id="d196e-128">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Redirect URIs** field.</span></span> <span data-ttu-id="d196e-129">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="d196e-129">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>

    ![Microsoft-konto - uppsättningen omdirigerings-URL: er](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  <span data-ttu-id="d196e-131">Klicka på **generera nya lösenord** under den **programmet hemligheter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d196e-131">Click on **Generate New Password** under the **Application Secrets** section.</span></span> <span data-ttu-id="d196e-132">Kopiera det nya lösenordet som visas på skärmen.</span><span class="sxs-lookup"><span data-stu-id="d196e-132">Copy the new password displayed on screen.</span></span> <span data-ttu-id="d196e-133">Du behöver det för att konfigurera Microsoft-konto som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="d196e-133">You need it to configure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="d196e-134">Lösenordet är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="d196e-134">This password is an important security credential.</span></span>

    ![Microsoft-konto – Skapa nytt lösenord](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft-konto – kopiera det nya lösenordet](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  <span data-ttu-id="d196e-137">Markera kryssrutan som säger **Live SDK stöd** under den **avancerade alternativ** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d196e-137">Check the box that says **Live SDK support** under the **Advanced Options** section.</span></span> <span data-ttu-id="d196e-138">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d196e-138">Click **Save**.</span></span>

    ![Microsoft-konto - stöd för Live SDK](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a><span data-ttu-id="d196e-140">Lägg till program-tangenten Microsoft-konto i Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="d196e-140">Add the Microsoft account application key to Azure AD B2C</span></span>
<span data-ttu-id="d196e-141">Federation med Microsoft-konton kräver en klienthemlighet för Microsoft-konto för Azure AD B2C-förtroende för programmet.</span><span class="sxs-lookup"><span data-stu-id="d196e-141">Federation with Microsoft accounts requires a client secret for Microsoft account to trust Azure AD B2C on behalf of the application.</span></span> <span data-ttu-id="d196e-142">Du måste lagra secert för programmet ditt Microsoft-konto i Azure AD B2C-klient:</span><span class="sxs-lookup"><span data-stu-id="d196e-142">You need to store your Microsoft account application secert in Azure AD B2C tenant:</span></span>   

1.  <span data-ttu-id="d196e-143">Gå till din Azure AD B2C-klient och välj **B2C inställningar** > **identitet upplevelse Framework**</span><span class="sxs-lookup"><span data-stu-id="d196e-143">Go to your Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="d196e-144">Välj **princip nycklar** att visa nycklarna som är tillgängliga i din klient.</span><span class="sxs-lookup"><span data-stu-id="d196e-144">Select **Policy Keys** to view the keys available in your tenant.</span></span>
3.  <span data-ttu-id="d196e-145">Klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d196e-145">Click **+Add**.</span></span>
4.  <span data-ttu-id="d196e-146">För **alternativ**, använda **manuell**.</span><span class="sxs-lookup"><span data-stu-id="d196e-146">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="d196e-147">För **namn**, Använd `MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="d196e-147">For **Name**, use `MSASecret`.</span></span>  
    <span data-ttu-id="d196e-148">Prefixet `B2C_1A_` kan läggas till automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d196e-148">The prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="d196e-149">I den **hemlighet** ange ditt hemliga för Microsoft-program från https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="d196e-149">In the **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="d196e-150">För **nyckelanvändning**, använda **signatur**.</span><span class="sxs-lookup"><span data-stu-id="d196e-150">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="d196e-151">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="d196e-151">Click **Create**</span></span>
9.  <span data-ttu-id="d196e-152">Bekräfta att du har skapat nyckeln `B2C_1A_MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="d196e-152">Confirm that you've created the key `B2C_1A_MSASecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="d196e-153">Lägg till en anspråksprovider i din princip för tillägg</span><span class="sxs-lookup"><span data-stu-id="d196e-153">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="d196e-154">Om du vill att användarna att logga in med hjälp av Account måste du definiera Account som en anspråksprovider.</span><span class="sxs-lookup"><span data-stu-id="d196e-154">If you want users to sign in by using Microsoft Account, you need to define Microsoft Account as a claims provider.</span></span> <span data-ttu-id="d196e-155">Med andra ord, måste du ange en slutpunkt som Azure AD B2C kommunicerar med.</span><span class="sxs-lookup"><span data-stu-id="d196e-155">In other words, you need to specify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="d196e-156">Slutpunkten innehåller en uppsättning anspråk som används av Azure AD B2C för att verifiera att en specifik användare har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="d196e-156">The endpoint provides a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span>

<span data-ttu-id="d196e-157">Definiera Account som en anspråksprovider genom att lägga till `<ClaimsProvider>` nod i tillägget-principfil:</span><span class="sxs-lookup"><span data-stu-id="d196e-157">Define Microsoft Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="d196e-158">Öppna filen för princip för tillägg (TrustFrameworkExtensions.xml) från arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="d196e-158">Open the extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="d196e-159">Om du behöver en XML-redigerare [försök Visual Studio Code](https://code.visualstudio.com/download), en enkel plattformsoberoende redigerare.</span><span class="sxs-lookup"><span data-stu-id="d196e-159">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="d196e-160">Hitta de `<ClaimsProviders>` avsnitt</span><span class="sxs-lookup"><span data-stu-id="d196e-160">Find the `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="d196e-161">Lägg till följande XML-kodstycke under den `ClaimsProviders` element:</span><span class="sxs-lookup"><span data-stu-id="d196e-161">Add following XML snippet under the `ClaimsProviders` element:</span></span>

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

4.  <span data-ttu-id="d196e-162">Ersätt `client_id` värdet med Account programklienten Id</span><span class="sxs-lookup"><span data-stu-id="d196e-162">Replace `client_id` value with your Microsoft Account application client Id</span></span>

5.  <span data-ttu-id="d196e-163">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="d196e-163">Save the file.</span></span>

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a><span data-ttu-id="d196e-164">Registrera Account anspråk leverantör för att logga in eller logga in användaren resa</span><span class="sxs-lookup"><span data-stu-id="d196e-164">Register the Microsoft Account claims provider to Sign up or Sign-in user journey</span></span>

<span data-ttu-id="d196e-165">Nu identitetsleverantören har ställts in, men den är inte tillgänglig i alla skärmar sign-upp/inloggning.</span><span class="sxs-lookup"><span data-stu-id="d196e-165">At this point, the identity provider has been set up, but it’s not available in any of the sign-up/sign-in screens.</span></span> <span data-ttu-id="d196e-166">Nu måste du lägga till identitetsleverantören Account användaren `SignUpOrSignIn` användaren resa.</span><span class="sxs-lookup"><span data-stu-id="d196e-166">Now you need to add the Microsoft Account identity provider to your user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="d196e-167">Om du vill göra den tillgänglig, kan vi skapa en dubblett av en befintlig mall användaren resa.</span><span class="sxs-lookup"><span data-stu-id="d196e-167">To make it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="d196e-168">Vi Lägg sedan till identitetsleverantören Account:</span><span class="sxs-lookup"><span data-stu-id="d196e-168">Then we add the Microsoft Account identity provider:</span></span>

> [!NOTE]
>
><span data-ttu-id="d196e-169">Om du tidigare har kopierat den `<UserJourneys>` element från bas-filen för din princip för tilläggsfilen `TrustFrameworkExtensions.xml`, du kan hoppa till det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d196e-169">If you previously copied the `<UserJourneys>` element from base file of your policy to the extension file `TrustFrameworkExtensions.xml`, you can skip to this section.</span></span>

1.  <span data-ttu-id="d196e-170">Öppna filen grundläggande av principen (till exempel TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="d196e-170">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="d196e-171">Hitta de `<UserJourneys>` element och kopiera hela innehållet i `<UserJourneys>` nod.</span><span class="sxs-lookup"><span data-stu-id="d196e-171">Find the `<UserJourneys>` element and copy the entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="d196e-172">Öppna tilläggsfilen (till exempel TrustFrameworkExtensions.xml) och Sök efter den `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="d196e-172">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="d196e-173">Om elementet inte finns, kan du lägga till en.</span><span class="sxs-lookup"><span data-stu-id="d196e-173">If the element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="d196e-174">Klistra in hela innehållet i `<UserJournesy>` nod som du kopierade som underordnad till den `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="d196e-174">Paste the entire content of `<UserJournesy>` node that you copied as a child of the `<UserJourneys>` element.</span></span>

### <a name="display-the-button"></a><span data-ttu-id="d196e-175">Visa knappen</span><span class="sxs-lookup"><span data-stu-id="d196e-175">Display the button</span></span>
<span data-ttu-id="d196e-176">Den `<ClaimsProviderSelections>` elementet definierar en lista över alternativ för val av anspråk providern och deras inbördes ordning.</span><span class="sxs-lookup"><span data-stu-id="d196e-176">The `<ClaimsProviderSelections>` element defines the list of claims provider selection options and their order.</span></span>  <span data-ttu-id="d196e-177">`<ClaimsProviderSelection>`elementet är detsamma som knappen identity-providern på en sign-upp/inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="d196e-177">`<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="d196e-178">Om du lägger till en `<ClaimsProviderSelection>` element för Microsoft-konto, en ny knapp visas när en användare de hamnar på sidan.</span><span class="sxs-lookup"><span data-stu-id="d196e-178">If you add a `<ClaimsProviderSelection>` element for Microsoft account, a new button shows up when a user lands on the page.</span></span> <span data-ttu-id="d196e-179">Lägg till det här elementet:</span><span class="sxs-lookup"><span data-stu-id="d196e-179">To add this element:</span></span>

1.  <span data-ttu-id="d196e-180">Hitta de `<UserJourney>` nod som innehåller `Id="SignUpOrSignIn"` i transporten användare som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="d196e-180">Find the `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in the user journey that you copied.</span></span>
2.  <span data-ttu-id="d196e-181">Leta upp den `<OrchestrationStep>` nod som innehåller`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="d196e-181">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="d196e-182">Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:</span><span class="sxs-lookup"><span data-stu-id="d196e-182">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="d196e-183">Länka knappen till en åtgärd</span><span class="sxs-lookup"><span data-stu-id="d196e-183">Link the button to an action</span></span>
<span data-ttu-id="d196e-184">Nu när du har en knapp på plats måste länka till en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d196e-184">Now that you have a button in place, you need to link it to an action.</span></span> <span data-ttu-id="d196e-185">Åtgärden är i det här fallet för Azure AD B2C att kommunicera med Account ta emot en token.</span><span class="sxs-lookup"><span data-stu-id="d196e-185">The action, in this case, is for Azure AD B2C to communicate with Microsoft Account to receive a token.</span></span> <span data-ttu-id="d196e-186">Länka knappen till en åtgärd genom att länka tekniska profilen för din Account anspråksleverantör:</span><span class="sxs-lookup"><span data-stu-id="d196e-186">Link the button to an action by linking the technical profile for your Microsoft Account claims provider:</span></span>

1.  <span data-ttu-id="d196e-187">Hitta de `<OrchestrationStep>` som innehåller `Order="2"` i den `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="d196e-187">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="d196e-188">Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:</span><span class="sxs-lookup"><span data-stu-id="d196e-188">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * <span data-ttu-id="d196e-189">Se till att den `Id` har samma värde som `TargetClaimsExchangeId` i föregående avsnitt</span><span class="sxs-lookup"><span data-stu-id="d196e-189">Ensure the `Id` has the same value as that of `TargetClaimsExchangeId` in the preceding section</span></span>
>   * <span data-ttu-id="d196e-190">Se till att `TechnicalProfileReferenceId` -ID har angetts för teknisk profilen du skapade tidigare (MSA-OIDC).</span><span class="sxs-lookup"><span data-stu-id="d196e-190">Ensure `TechnicalProfileReferenceId` ID is set to the technical profile you created earlier (MSA-OIDC).</span></span>

## <a name="upload-the-policy-to-your-tenant"></a><span data-ttu-id="d196e-191">Ladda upp principen till din klient</span><span class="sxs-lookup"><span data-stu-id="d196e-191">Upload the policy to your tenant</span></span>
1.  <span data-ttu-id="d196e-192">I den [Azure-portalen](https://portal.azure.com), växla till den [kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och öppna den **Azure AD B2C** bladet.</span><span class="sxs-lookup"><span data-stu-id="d196e-192">In the [Azure portal](https://portal.azure.com), switch into the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open the **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="d196e-193">Välj **identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="d196e-193">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="d196e-194">Öppna den **alla principer** bladet.</span><span class="sxs-lookup"><span data-stu-id="d196e-194">Open the **All Policies** blade.</span></span>
4.  <span data-ttu-id="d196e-195">Välj **överföra princip**.</span><span class="sxs-lookup"><span data-stu-id="d196e-195">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="d196e-196">Kontrollera **skriva över principen om den finns** rutan.</span><span class="sxs-lookup"><span data-stu-id="d196e-196">Check **Overwrite the policy if it exists** box.</span></span>
6.  <span data-ttu-id="d196e-197">**Överför** TrustFrameworkExtensions.xml och se till att inte misslyckas verifieringen</span><span class="sxs-lookup"><span data-stu-id="d196e-197">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail the validation</span></span>

## <a name="test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="d196e-198">Testa den anpassade principen genom att använda Kör nu</span><span class="sxs-lookup"><span data-stu-id="d196e-198">Test the custom policy by using Run Now</span></span>

1.  <span data-ttu-id="d196e-199">Öppna **Azure AD B2C inställningar** och gå till **identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="d196e-199">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
> [!NOTE]
>
><span data-ttu-id="d196e-200">**Kör nu** kräver minst ett program till preregistered för innehavaren.</span><span class="sxs-lookup"><span data-stu-id="d196e-200">**Run now** requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="d196e-201">Om du vill lära dig mer om att registrera program, finns i Azure AD B2C [Kom igång](active-directory-b2c-get-started.md) artikel eller [appregistrering](active-directory-b2c-app-registration.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="d196e-201">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>
2.  <span data-ttu-id="d196e-202">Öppna **B2C_1A_signup_signin**, förlitande part (RP) anpassade principer som du har överfört.</span><span class="sxs-lookup"><span data-stu-id="d196e-202">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="d196e-203">Välj **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="d196e-203">Select **Run now**.</span></span>
3.  <span data-ttu-id="d196e-204">Du ska kunna logga in med Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="d196e-204">You should be able to sign in using Microsoft account.</span></span>

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a><span data-ttu-id="d196e-205">[Valfritt] Registrera Account anspråksprovidern för profil-Redigera användare resa</span><span class="sxs-lookup"><span data-stu-id="d196e-205">[Optional] Register the Microsoft Account claims provider to Profile-Edit user journey</span></span>
<span data-ttu-id="d196e-206">Du kanske vill lägga till identitetsleverantören Account också till dina användare `ProfileEdit` användare resa.</span><span class="sxs-lookup"><span data-stu-id="d196e-206">You may want to add the Microsoft Account identity provider also to your user `ProfileEdit` user journey.</span></span> <span data-ttu-id="d196e-207">Vi Upprepa de två sista stegen för att göra den tillgänglig:</span><span class="sxs-lookup"><span data-stu-id="d196e-207">To make it available, we repeat the last two steps:</span></span>

### <a name="display-the-button"></a><span data-ttu-id="d196e-208">Visa knappen</span><span class="sxs-lookup"><span data-stu-id="d196e-208">Display the button</span></span>
1.  <span data-ttu-id="d196e-209">Öppna filen för tillägg av principen (till exempel TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="d196e-209">Open the extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="d196e-210">Hitta de `<UserJourney>` nod som innehåller `Id="ProfileEdit"` i transporten användare som du kopierade.</span><span class="sxs-lookup"><span data-stu-id="d196e-210">Find the `<UserJourney>` node that includes `Id="ProfileEdit"` in the user journey that you copied.</span></span>
3.  <span data-ttu-id="d196e-211">Leta upp den `<OrchestrationStep>` nod som innehåller`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="d196e-211">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="d196e-212">Lägg till följande XML-kodstycke under `<ClaimsProviderSelections>` nod:</span><span class="sxs-lookup"><span data-stu-id="d196e-212">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="d196e-213">Länka knappen till en åtgärd</span><span class="sxs-lookup"><span data-stu-id="d196e-213">Link the button to an action</span></span>
1.  <span data-ttu-id="d196e-214">Hitta de `<OrchestrationStep>` som innehåller `Order="2"` i den `<UserJourney>` nod.</span><span class="sxs-lookup"><span data-stu-id="d196e-214">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="d196e-215">Lägg till följande XML-kodstycke under `<ClaimsExchanges>` nod:</span><span class="sxs-lookup"><span data-stu-id="d196e-215">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="d196e-216">Testa en anpassad profil-Redigera princip genom att använda Kör nu</span><span class="sxs-lookup"><span data-stu-id="d196e-216">Test the custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="d196e-217">Öppna **Azure AD B2C inställningar** och gå till **identitet upplevelse Framework**.</span><span class="sxs-lookup"><span data-stu-id="d196e-217">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="d196e-218">Öppna **B2C_1A_ProfileEdit**, förlitande part (RP) anpassade principer som du har överfört.</span><span class="sxs-lookup"><span data-stu-id="d196e-218">Open **B2C_1A_ProfileEdit**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="d196e-219">Välj **kör nu**.</span><span class="sxs-lookup"><span data-stu-id="d196e-219">Select **Run now**.</span></span>
3.  <span data-ttu-id="d196e-220">Du ska kunna logga in med Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="d196e-220">You should be able to sign in using Microsoft account.</span></span>

## <a name="download-the-complete-policy-files"></a><span data-ttu-id="d196e-221">Hämta de fullständiga principfilerna</span><span class="sxs-lookup"><span data-stu-id="d196e-221">Download the complete policy files</span></span>
<span data-ttu-id="d196e-222">Valfritt: Vi rekommenderar att du skapar ditt scenario med en egen anpassad princip för filer när du har slutfört komma igång med principer för anpassade igenom istället för att använda dessa exempelfiler.</span><span class="sxs-lookup"><span data-stu-id="d196e-222">Optional: We recommend you build your scenario using your own Custom policy files after completing the Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="d196e-223">Exempelfiler för principen för referens</span><span class="sxs-lookup"><span data-stu-id="d196e-223">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)

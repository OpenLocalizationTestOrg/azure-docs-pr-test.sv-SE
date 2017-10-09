---
title: "Azure Active Directory B2C: Ändra logga in i anpassade principer och konfigurera självbetjäningsportalen vars provider"
description: "En genomgång för att lägga till anspråk toosign in och konfigurera hello användarindata"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: tbd
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/29/2017
ms.author: joroja
ms.openlocfilehash: c31d737263fef3e771bdf451b809b0ca522c8fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-modify-sign-up-tooadd-new-claims-and-configure-user-input"></a><span data-ttu-id="0affe-103">Azure Active Directory B2C: Ändra tooadd nya anspråk-registrering och konfigurera indata från användaren.</span><span class="sxs-lookup"><span data-stu-id="0affe-103">Azure Active Directory B2C: Modify sign up tooadd new claims and configure user input.</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="0affe-104">I den här artikeln får du lägga till en ny anges av användaren post (ett anspråk) tooyour signup användaren-resa.</span><span class="sxs-lookup"><span data-stu-id="0affe-104">In this article, you will add a new user provided entry (a claim) tooyour signup user journey.</span></span>  <span data-ttu-id="0affe-105">Du kan konfigurera hello-posten som en listrutan och definiera om det behövs.</span><span class="sxs-lookup"><span data-stu-id="0affe-105">You will configure hello entry as a dropdown, and define if it is required.</span></span>

<span data-ttu-id="0affe-106">Redigera Sipi tootrigger test handoff.</span><span class="sxs-lookup"><span data-stu-id="0affe-106">Edited by Sipi tootrigger test handoff.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0affe-107">Krav</span><span class="sxs-lookup"><span data-stu-id="0affe-107">Prerequisites</span></span>

* <span data-ttu-id="0affe-108">Fullständig hello stegen i artikeln hello [komma igång med principer för anpassade](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="0affe-108">Complete hello steps in hello article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>  <span data-ttu-id="0affe-109">Testa hello signup/inloggning användaren resa toosignup ett nytt lokalt konto innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="0affe-109">Test hello signup/signin user journey toosignup a new local account before proceeding.</span></span>


<span data-ttu-id="0affe-110">Första datainsamlingen från användarna uppnås via signup/inloggning.</span><span class="sxs-lookup"><span data-stu-id="0affe-110">Gathering initial data from your users is achieved via signup/signin.</span></span>  <span data-ttu-id="0affe-111">Ytterligare anspråk kan samlas in senare via profil Redigera användare resor.</span><span class="sxs-lookup"><span data-stu-id="0affe-111">Additional claims can be gathered later via profile edit user journeys.</span></span> <span data-ttu-id="0affe-112">När Azure AD B2C samlar in information direkt från hello användaren interaktivt, hello identitet upplevelse Framework använder dess `selfasserted provider`.</span><span class="sxs-lookup"><span data-stu-id="0affe-112">Anytime Azure AD B2C gathers information directly from hello user interactively, hello Identity Experience Framework uses its `selfasserted provider`.</span></span> <span data-ttu-id="0affe-113">hello stegen nedan tillämpas när den här providern används.</span><span class="sxs-lookup"><span data-stu-id="0affe-113">hello steps below apply anytime this provider is used.</span></span>


## <a name="define-hello-claim-its-display-name-and-hello-user-input-type"></a><span data-ttu-id="0affe-114">Definiera hello anspråk, dess namn och hello användarindata typ</span><span class="sxs-lookup"><span data-stu-id="0affe-114">Define hello claim, its display name and hello user input type</span></span>
<span data-ttu-id="0affe-115">Kan be hello användare om deras stad.</span><span class="sxs-lookup"><span data-stu-id="0affe-115">Lets ask hello user for their city.</span></span>  <span data-ttu-id="0affe-116">Lägg till följande element toohello hello `<ClaimsSchema>` element i hello TrustFrameWorkExtensions principfil:</span><span class="sxs-lookup"><span data-stu-id="0affe-116">Add hello following element toohello `<ClaimsSchema>` element in hello TrustFrameWorkExtensions policy file:</span></span>

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
<span data-ttu-id="0affe-117">Det finns ytterligare alternativ kan du här toocustomize hello anspråk.</span><span class="sxs-lookup"><span data-stu-id="0affe-117">There are additional choices you can make here toocustomize hello claim.</span></span>  <span data-ttu-id="0affe-118">En fullständig schemat finns toohello **identitet upplevelse Framework teknisk referenshandbok**.</span><span class="sxs-lookup"><span data-stu-id="0affe-118">For a full schema, refer toohello **Identity Experience Framework Technical Reference Guide**.</span></span>  <span data-ttu-id="0affe-119">Den här guiden kommer att publiceras snart i hello referensavsnitt.</span><span class="sxs-lookup"><span data-stu-id="0affe-119">This guide will be published soon in hello reference section.</span></span>

* <span data-ttu-id="0affe-120">`<DisplayName>`är en sträng som definierar hello användarinriktad *etikett*</span><span class="sxs-lookup"><span data-stu-id="0affe-120">`<DisplayName>` is a string that defines hello user-facing *label*</span></span>

* <span data-ttu-id="0affe-121">`<UserHelpText>`hjälper hello användaren förstå vad som krävs</span><span class="sxs-lookup"><span data-stu-id="0affe-121">`<UserHelpText>` helps hello user understand what is required</span></span>

* <span data-ttu-id="0affe-122">`<UserInputType>`innehåller hello följande fyra alternativ markerade nedan:</span><span class="sxs-lookup"><span data-stu-id="0affe-122">`<UserInputType>` has hello following four options highlighted below:</span></span>
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * <span data-ttu-id="0affe-123">`RadioSingleSelectduration`-Tillämpar ett val.</span><span class="sxs-lookup"><span data-stu-id="0affe-123">`RadioSingleSelectduration` - Enforces a single selection.</span></span>
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>RadioSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

    * <span data-ttu-id="0affe-124">`DropdownSingleSelect`-Kan hello val av endast giltigt värde.</span><span class="sxs-lookup"><span data-stu-id="0affe-124">`DropdownSingleSelect` - Allows hello selection of only valid value.</span></span>

![Skärmbild som visar dropdown alternativet](./media/active-directory-b2c-configure-signup-self-asserted-custom/dropdown-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```


* <span data-ttu-id="0affe-126">`CheckboxMultiSelect`Tillåter hello val av ett eller flera värden.</span><span class="sxs-lookup"><span data-stu-id="0affe-126">`CheckboxMultiSelect` Allows for hello selection of one or more values.</span></span>

![Skärmbild av multiselect alternativet](./media/active-directory-b2c-configure-signup-self-asserted-custom/multiselect-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>Receive updates from which cities?</DisplayName>
  <DataType>string</DataType>
  <UserInputType>CheckboxMultiSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

## <a name="add-hello-claim-toohello-sign-upsign-in-user-journey"></a><span data-ttu-id="0affe-128">Lägg till hello anspråk toohello logga upp/logga in användaren resa</span><span class="sxs-lookup"><span data-stu-id="0affe-128">Add hello claim toohello sign up/sign in user journey</span></span>

1. <span data-ttu-id="0affe-129">Lägg till hello anspråk som en `<OutputClaim ClaimTypeReferenceId="city"/>` toohello TechnicalProfile `LocalAccountSignUpWithLogonEmail` (hittas i hello TrustFrameworkBase princip-fil).</span><span class="sxs-lookup"><span data-stu-id="0affe-129">Add hello claim as an `<OutputClaim ClaimTypeReferenceId="city"/>` toohello TechnicalProfile `LocalAccountSignUpWithLogonEmail` (found in hello TrustFrameworkBase policy file).</span></span>  <span data-ttu-id="0affe-130">Observera att den här TechnicalProfile använder hello SelfAssertedAttributeProvider.</span><span class="sxs-lookup"><span data-stu-id="0affe-130">Note this TechnicalProfile uses hello SelfAssertedAttributeProvider.</span></span>

  ```xml
  <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
    <DisplayName>Email signup</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
      <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
      <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
      <Item Key="language.button_continue">Create</Item>
    </Metadata>
    <CryptographicKeys>
      <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
    </CryptographicKeys>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
      <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" />
      <OutputClaim ClaimTypeReferenceId="newUser" />
      <!-- Optional claims, toobe collected from hello user -->
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surName" />
      <OutputClaim ClaimTypeReferenceId="city"/>
    </OutputClaims>
    <ValidationTechnicalProfiles>
      <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
    </ValidationTechnicalProfiles>
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

2. <span data-ttu-id="0affe-131">Lägg till hello anspråk toohello AAD-UserWriteUsingLogonEmail som en `<PersistedClaim ClaimTypeReferenceId="city" />` toowrite hello anspråk toohello AAD-katalogen efter insamling från hello användare.</span><span class="sxs-lookup"><span data-stu-id="0affe-131">Add hello claim toohello AAD-UserWriteUsingLogonEmail as a `<PersistedClaim ClaimTypeReferenceId="city" />` toowrite hello claim toohello AAD directory after collecting it from hello user.</span></span> <span data-ttu-id="0affe-132">Du kan hoppa över det här steget om du föredrar att inte toopersist hello anspråk i hello directory för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="0affe-132">You may skip this step if you prefer not toopersist hello claim in hello directory for future use.</span></span>

  ```xml
  <!-- Technical profiles for local accounts -->
  <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
    <Metadata>
      <Item Key="Operation">Write</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" Required="true" />
    </InputClaims>
    <PersistedClaims>
      <!-- Required claims -->
      <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
      <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password" />
      <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
      <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />
      <!-- Optional claims. -->
      <PersistedClaim ClaimTypeReferenceId="givenName" />
      <PersistedClaim ClaimTypeReferenceId="surname" />
      <PersistedClaim ClaimTypeReferenceId="city" />
    </PersistedClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

3. <span data-ttu-id="0affe-133">Lägg till hello anspråk toohello TechnicalProfile som läser från hello katalog när en användare loggar in som en`<OutputClaim ClaimTypeReferenceId="city" />`</span><span class="sxs-lookup"><span data-stu-id="0affe-133">Add hello claim toohello TechnicalProfile that reads from hello directory when a user logs in as an `<OutputClaim ClaimTypeReferenceId="city" />`</span></span>

  ```xml
  <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
    <Metadata>
      <Item Key="Operation">Read</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">An account could not be found for hello provided user ID.</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames" Required="true" />
    </InputClaims>
    <OutputClaims>
      <!-- Required claims -->
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <!-- Optional claims -->
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="otherMails" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
  </TechnicalProfile>
  ```

4. <span data-ttu-id="0affe-134">Lägg till hello `<OutputClaim ClaimTypeReferenceId="city" />` toohello RP principfil SignUporSignIn.xml så att denna begäran skickas toohello program i hello token efter en lyckad användaren resa.</span><span class="sxs-lookup"><span data-stu-id="0affe-134">Add hello `<OutputClaim ClaimTypeReferenceId="city" />` toohello RP policy file SignUporSignIn.xml so this claim is sent toohello application in hello token after a successful user journey.</span></span>

  ```xml
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
  ```

## <a name="test-hello-custom-policy-using-run-now"></a><span data-ttu-id="0affe-135">Testa hello anpassad princip med ”Kör nu”</span><span class="sxs-lookup"><span data-stu-id="0affe-135">Test hello custom policy using "Run Now"</span></span>

1. <span data-ttu-id="0affe-136">Öppna hello **Azure AD B2C bladet** och navigera för**identitet upplevelse Framework > anpassade principer**.</span><span class="sxs-lookup"><span data-stu-id="0affe-136">Open hello **Azure AD B2C Blade** and navigate too**Identity Experience Framework > Custom policies**.</span></span>
2. <span data-ttu-id="0affe-137">Välj hello anpassad princip som du överfört och klicka på hello **kör nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="0affe-137">Select hello custom policy that you uploaded, and click hello **Run now** button.</span></span>
3. <span data-ttu-id="0affe-138">Du ska kunna toosign med en e-postadress.</span><span class="sxs-lookup"><span data-stu-id="0affe-138">You should be able toosign up using an email address.</span></span>

<span data-ttu-id="0affe-139">hello signup skärm i testläge bör se ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="0affe-139">hello signup screen in test mode should look similar toothis:</span></span>

![Skärmbild av alternativ för ändrade anmälan](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  <span data-ttu-id="0affe-141">hello token tillbaka tooyour programmet nu omfattar hello `city` anspråk som visas nedan</span><span class="sxs-lookup"><span data-stu-id="0affe-141">hello token back tooyour application will now include hello `city` claim as shown below</span></span>
```json
{
  "exp": 1493596822,
  "nbf": 1493593222,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "9c2a3a9e-ac65-4e46-a12d-9557b63033a9",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustf_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1493593222,
  "auth_time": 1493593222,
  "email": "joe@outlook.com",
  "given_name": "Joe",
  "family_name": "Ras",
  "city": "Bellevue",
  "name": "unknown"
}
```

## <a name="optional-remove-email-verification-from-signup-journey"></a><span data-ttu-id="0affe-142">Valfritt: Ta bort e-Postverifiering från registreringen resa</span><span class="sxs-lookup"><span data-stu-id="0affe-142">Optional: Remove email verification from signup journey</span></span>

<span data-ttu-id="0affe-143">tooskip e-Postverifiering hello princip författaren kan välja tooremove `PartnerClaimType="Verified.Email"`.</span><span class="sxs-lookup"><span data-stu-id="0affe-143">tooskip email verification, hello policy author can choose tooremove `PartnerClaimType="Verified.Email"`.</span></span> <span data-ttu-id="0affe-144">hello e-postadress kommer att krävs men inte kontrolleras, såvida inte ”krävs” = true tas bort.</span><span class="sxs-lookup"><span data-stu-id="0affe-144">hello email address will be required but not verified, unless “Required” = true is removed.</span></span>  <span data-ttu-id="0affe-145">Du noga överväga om det här alternativet är rätt för din användningsfall!</span><span class="sxs-lookup"><span data-stu-id="0affe-145">Carefully consider if this option is right for your use cases!</span></span>

<span data-ttu-id="0affe-146">Verifiera e-post är aktiverat som standard i hello `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` i hello TrustFrameworkBase principfil hello startpaket:</span><span class="sxs-lookup"><span data-stu-id="0affe-146">Verified email is enabled by default in hello `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` in hello TrustFrameworkBase policy file in hello starter pack:</span></span>
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a><span data-ttu-id="0affe-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0affe-147">Next steps</span></span>

<span data-ttu-id="0affe-148">Lägg till hello nya anspråk toohello flöden sociala inloggningsnamn genom att ändra hello TechnicalProfiles nedan.</span><span class="sxs-lookup"><span data-stu-id="0affe-148">Add hello new claim toohello flows for social account logins by changing hello TechnicalProfiles listed below.</span></span> <span data-ttu-id="0affe-149">Dessa används av sociala/federerat konto inloggningar toowrite och läsa hello användardata med hello alternativeSecurityId som hello lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="0affe-149">These are used by social/federated account logins toowrite and read hello user data using hello alternativeSecurityId as hello locator.</span></span>
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

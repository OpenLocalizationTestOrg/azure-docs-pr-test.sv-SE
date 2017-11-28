---
title: "Azure Active Directory B2C: Ändra logga in i anpassade principer och konfigurera självbetjäningsportalen vars provider"
description: "En genomgång för att lägga till anspråk som ska logga och konfigurera användarindata"
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
ms.openlocfilehash: 64b9d904d7d070052e125b479f4719d208c9ff85
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/28/2017
---
# <a name="azure-active-directory-b2c-modify-sign-up-to-add-new-claims-and-configure-user-input"></a><span data-ttu-id="d02e0-103">Azure Active Directory B2C: Ändra logga in för att lägga till nya anspråk och konfigurera indata från användaren.</span><span class="sxs-lookup"><span data-stu-id="d02e0-103">Azure Active Directory B2C: Modify sign up to add new claims and configure user input.</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="d02e0-104">I den här artikeln, ska du lägga till en ny post för användare som har angetts (ett anspråk) din anmälan användaren resa.</span><span class="sxs-lookup"><span data-stu-id="d02e0-104">In this article, you will add a new user provided entry (a claim) to your signup user journey.</span></span>  <span data-ttu-id="d02e0-105">Du kan konfigurera posten som en listrutan och definiera om det behövs.</span><span class="sxs-lookup"><span data-stu-id="d02e0-105">You will configure the entry as a dropdown, and define if it is required.</span></span>

<span data-ttu-id="d02e0-106">Redigera Sipi att utlösa handoff för testet.</span><span class="sxs-lookup"><span data-stu-id="d02e0-106">Edited by Sipi to trigger test handoff.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d02e0-107">Krav</span><span class="sxs-lookup"><span data-stu-id="d02e0-107">Prerequisites</span></span>

* <span data-ttu-id="d02e0-108">Utför stegen i artikeln [komma igång med principer för anpassade](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d02e0-108">Complete the steps in the article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>  <span data-ttu-id="d02e0-109">Testa signup/inloggning användaren resa till registreringen ett nytt lokalt konto innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="d02e0-109">Test the signup/signin user journey to signup a new local account before proceeding.</span></span>


<span data-ttu-id="d02e0-110">Första datainsamlingen från användarna uppnås via signup/inloggning.</span><span class="sxs-lookup"><span data-stu-id="d02e0-110">Gathering initial data from your users is achieved via signup/signin.</span></span>  <span data-ttu-id="d02e0-111">Ytterligare anspråk kan samlas in senare via profil Redigera användare resor.</span><span class="sxs-lookup"><span data-stu-id="d02e0-111">Additional claims can be gathered later via profile edit user journeys.</span></span> <span data-ttu-id="d02e0-112">När Azure AD B2C samlar in information direkt från användaren interaktivt, identitet upplevelse Framework använder dess `selfasserted provider`.</span><span class="sxs-lookup"><span data-stu-id="d02e0-112">Anytime Azure AD B2C gathers information directly from the user interactively, the Identity Experience Framework uses its `selfasserted provider`.</span></span> <span data-ttu-id="d02e0-113">Stegen nedan tillämpas när den här providern används.</span><span class="sxs-lookup"><span data-stu-id="d02e0-113">The steps below apply anytime this provider is used.</span></span>


## <a name="define-the-claim-its-display-name-and-the-user-input-type"></a><span data-ttu-id="d02e0-114">Definiera anspråket, dess namn och användaren Indatatyp</span><span class="sxs-lookup"><span data-stu-id="d02e0-114">Define the claim, its display name and the user input type</span></span>
<span data-ttu-id="d02e0-115">Kan be användaren för sina ort.</span><span class="sxs-lookup"><span data-stu-id="d02e0-115">Lets ask the user for their city.</span></span>  <span data-ttu-id="d02e0-116">Lägg till följande elementet så att den `<ClaimsSchema>` element i filen TrustFrameWorkExtensions princip:</span><span class="sxs-lookup"><span data-stu-id="d02e0-116">Add the following element to the `<ClaimsSchema>` element in the TrustFrameWorkExtensions policy file:</span></span>

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
<span data-ttu-id="d02e0-117">Det finns ytterligare alternativ som du gör här anpassa anspråket.</span><span class="sxs-lookup"><span data-stu-id="d02e0-117">There are additional choices you can make here to customize the claim.</span></span>  <span data-ttu-id="d02e0-118">En fullständig schemat finns i den **identitet upplevelse Framework teknisk referenshandbok**.</span><span class="sxs-lookup"><span data-stu-id="d02e0-118">For a full schema, refer to the **Identity Experience Framework Technical Reference Guide**.</span></span>  <span data-ttu-id="d02e0-119">Den här guiden kommer att publiceras snart i referensavsnittet.</span><span class="sxs-lookup"><span data-stu-id="d02e0-119">This guide will be published soon in the reference section.</span></span>

* <span data-ttu-id="d02e0-120">`<DisplayName>`är en sträng som definierar användarinriktade *etikett*</span><span class="sxs-lookup"><span data-stu-id="d02e0-120">`<DisplayName>` is a string that defines the user-facing *label*</span></span>

* <span data-ttu-id="d02e0-121">`<UserHelpText>`hjälper användaren att förstå vad som krävs</span><span class="sxs-lookup"><span data-stu-id="d02e0-121">`<UserHelpText>` helps the user understand what is required</span></span>

* <span data-ttu-id="d02e0-122">`<UserInputType>`innehåller följande fyra alternativ markerade nedan:</span><span class="sxs-lookup"><span data-stu-id="d02e0-122">`<UserInputType>` has the following four options highlighted below:</span></span>
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * <span data-ttu-id="d02e0-123">`RadioSingleSelectduration`-Tillämpar ett val.</span><span class="sxs-lookup"><span data-stu-id="d02e0-123">`RadioSingleSelectduration` - Enforces a single selection.</span></span>
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

    * <span data-ttu-id="d02e0-124">`DropdownSingleSelect`-Du kan välja endast giltigt värde.</span><span class="sxs-lookup"><span data-stu-id="d02e0-124">`DropdownSingleSelect` - Allows the selection of only valid value.</span></span>

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


* <span data-ttu-id="d02e0-126">`CheckboxMultiSelect`Gör det möjligt för att välja ett eller flera värden.</span><span class="sxs-lookup"><span data-stu-id="d02e0-126">`CheckboxMultiSelect` Allows for the selection of one or more values.</span></span>

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

## <a name="add-the-claim-to-the-sign-upsign-in-user-journey"></a><span data-ttu-id="d02e0-128">Lägga till anspråk till inloggningssidan upp/logga in användaren resa</span><span class="sxs-lookup"><span data-stu-id="d02e0-128">Add the claim to the sign up/sign in user journey</span></span>

1. <span data-ttu-id="d02e0-129">Lägga till anspråk som en `<OutputClaim ClaimTypeReferenceId="city"/>` till TechnicalProfile `LocalAccountSignUpWithLogonEmail` (hittas i principfilen TrustFrameworkBase).</span><span class="sxs-lookup"><span data-stu-id="d02e0-129">Add the claim as an `<OutputClaim ClaimTypeReferenceId="city"/>` to the TechnicalProfile `LocalAccountSignUpWithLogonEmail` (found in the TrustFrameworkBase policy file).</span></span>  <span data-ttu-id="d02e0-130">Observera att den här TechnicalProfile använder SelfAssertedAttributeProvider.</span><span class="sxs-lookup"><span data-stu-id="d02e0-130">Note this TechnicalProfile uses the SelfAssertedAttributeProvider.</span></span>

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
      <!-- Optional claims, to be collected from the user -->
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

2. <span data-ttu-id="d02e0-131">Lägga till anspråk till AAD-UserWriteUsingLogonEmail som en `<PersistedClaim ClaimTypeReferenceId="city" />` att skriva anspråk till AAD-katalogen efter insamling från användaren.</span><span class="sxs-lookup"><span data-stu-id="d02e0-131">Add the claim to the AAD-UserWriteUsingLogonEmail as a `<PersistedClaim ClaimTypeReferenceId="city" />` to write the claim to the AAD directory after collecting it from the user.</span></span> <span data-ttu-id="d02e0-132">Du kan hoppa över det här steget om du inte vill bevara anspråk i katalogen för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="d02e0-132">You may skip this step if you prefer not to persist the claim in the directory for future use.</span></span>

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

3. <span data-ttu-id="d02e0-133">Lägga till anspråk till TechnicalProfile som läser från katalogen när en användare loggar in som en`<OutputClaim ClaimTypeReferenceId="city" />`</span><span class="sxs-lookup"><span data-stu-id="d02e0-133">Add the claim to the TechnicalProfile that reads from the directory when a user logs in as an `<OutputClaim ClaimTypeReferenceId="city" />`</span></span>

  ```xml
  <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
    <Metadata>
      <Item Key="Operation">Read</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">An account could not be found for the provided user ID.</Item>
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

4. <span data-ttu-id="d02e0-134">Lägg till den `<OutputClaim ClaimTypeReferenceId="city" />` filen SignUporSignIn.xml till RP principen så att denna begäran skickas till programmet i token efter en lyckad användaren resa.</span><span class="sxs-lookup"><span data-stu-id="d02e0-134">Add the `<OutputClaim ClaimTypeReferenceId="city" />` to the RP policy file SignUporSignIn.xml so this claim is sent to the application in the token after a successful user journey.</span></span>

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

## <a name="test-the-custom-policy-using-run-now"></a><span data-ttu-id="d02e0-135">Testa den anpassade principen med hjälp av ”kör nu”</span><span class="sxs-lookup"><span data-stu-id="d02e0-135">Test the custom policy using "Run Now"</span></span>

1. <span data-ttu-id="d02e0-136">Öppna den **Azure AD B2C bladet** och gå till **identitet upplevelse Framework > anpassade principer**.</span><span class="sxs-lookup"><span data-stu-id="d02e0-136">Open the **Azure AD B2C Blade** and navigate to **Identity Experience Framework > Custom policies**.</span></span>
2. <span data-ttu-id="d02e0-137">Välj den anpassade principen som du överfört och klicka på den **kör nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="d02e0-137">Select the custom policy that you uploaded, and click the **Run now** button.</span></span>
3. <span data-ttu-id="d02e0-138">Du ska kunna logga med en e-postadress.</span><span class="sxs-lookup"><span data-stu-id="d02e0-138">You should be able to sign up using an email address.</span></span>

<span data-ttu-id="d02e0-139">Skärmen registreringen i testläge bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="d02e0-139">The signup screen in test mode should look similar to this:</span></span>

![Skärmbild av alternativ för ändrade anmälan](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  <span data-ttu-id="d02e0-141">Token tillbaka till ditt program innehåller nu den `city` anspråk som visas nedan</span><span class="sxs-lookup"><span data-stu-id="d02e0-141">The token back to your application will now include the `city` claim as shown below</span></span>
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

## <a name="optional-remove-email-verification-from-signup-journey"></a><span data-ttu-id="d02e0-142">Valfritt: Ta bort e-Postverifiering från registreringen resa</span><span class="sxs-lookup"><span data-stu-id="d02e0-142">Optional: Remove email verification from signup journey</span></span>

<span data-ttu-id="d02e0-143">Om du vill hoppa över e-Postverifiering princip författaren kan du ta bort `PartnerClaimType="Verified.Email"`.</span><span class="sxs-lookup"><span data-stu-id="d02e0-143">To skip email verification, the policy author can choose to remove `PartnerClaimType="Verified.Email"`.</span></span> <span data-ttu-id="d02e0-144">E-postadressen kommer krävs men inte kontrolleras, såvida inte ”krävs” = true tas bort.</span><span class="sxs-lookup"><span data-stu-id="d02e0-144">The email address will be required but not verified, unless “Required” = true is removed.</span></span>  <span data-ttu-id="d02e0-145">Du noga överväga om det här alternativet är rätt för din användningsfall!</span><span class="sxs-lookup"><span data-stu-id="d02e0-145">Carefully consider if this option is right for your use cases!</span></span>

<span data-ttu-id="d02e0-146">Verifiera e-post är aktiverat som standard i den `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` TrustFrameworkBase principfilen i starter pack:</span><span class="sxs-lookup"><span data-stu-id="d02e0-146">Verified email is enabled by default in the `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` in the TrustFrameworkBase policy file in the starter pack:</span></span>
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a><span data-ttu-id="d02e0-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d02e0-147">Next steps</span></span>

<span data-ttu-id="d02e0-148">Lägga till nya anspråk till flödena för sociala inloggningsnamn genom att ändra TechnicalProfiles nedan.</span><span class="sxs-lookup"><span data-stu-id="d02e0-148">Add the new claim to the flows for social account logins by changing the TechnicalProfiles listed below.</span></span> <span data-ttu-id="d02e0-149">Dessa används av sociala/federerad inloggningsnamn för att skriva och läsa informationen med hjälp av alternativeSecurityId som positioneraren.</span><span class="sxs-lookup"><span data-stu-id="d02e0-149">These are used by social/federated account logins to write and read the user data using the alternativeSecurityId as the locator.</span></span>
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

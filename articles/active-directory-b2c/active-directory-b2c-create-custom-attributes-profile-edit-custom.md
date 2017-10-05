---
title: "Azure Active Directory B2C: Lägga till egna attribut i anpassade principer och använda i profilen redigera | Microsoft Docs"
description: "En genomgång av använder tilläggsegenskaper, anpassade attribut och inkludera dem i användargränssnittet"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 67c9f6eca18e2dd77e00b8bc8c7bcc546ea3936e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a><span data-ttu-id="846e8-103">Azure Active Directory B2C: Skapa och använda anpassade attribut i en anpassad profil redigera principen</span><span class="sxs-lookup"><span data-stu-id="846e8-103">Azure Active Directory B2C: Creating and using custom attributes in a custom profile edit policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="846e8-104">I den här artikeln, skapa ett anpassat attribut i Azure AD B2C-katalogen och använda den här nya attributet som ett anpassat anspråk i profilen Redigera användare transporten.</span><span class="sxs-lookup"><span data-stu-id="846e8-104">In this article, you create a custom attribute in your Azure AD B2C directory and use this new attribute as a custom claim in the profile edit user journey.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="846e8-105">Krav</span><span class="sxs-lookup"><span data-stu-id="846e8-105">Prerequisites</span></span>

<span data-ttu-id="846e8-106">Utför stegen i artikeln [komma igång med principer för anpassade](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="846e8-106">Complete the steps in the article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a><span data-ttu-id="846e8-107">Använd anpassade attribut för att samla in information om dina kunder i Azure Active Directory B2C anpassade principer</span><span class="sxs-lookup"><span data-stu-id="846e8-107">Use custom attributes to collect information about your customers in Azure Active Directory B2C using custom policies</span></span>
<span data-ttu-id="846e8-108">Din Azure Active Directory (AD Azure) B2C-katalog som levereras med en inbyggd uppsättning attribut: namn, efternamn, ort, postnummer, userPrincipalName får osv.  Du behöver ofta skapa egna attribut.</span><span class="sxs-lookup"><span data-stu-id="846e8-108">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of attributes: Given Name, Surname, City, Postal Code, userPrincipalName, etc.  You often need to create your own attributes.</span></span>  <span data-ttu-id="846e8-109">Exempel:</span><span class="sxs-lookup"><span data-stu-id="846e8-109">For example:</span></span>
* <span data-ttu-id="846e8-110">Ett kund-riktade program måste bevara ett attribut, till exempel ”LoyaltyNumber”.</span><span class="sxs-lookup"><span data-stu-id="846e8-110">A customer-facing application needs to persist an attribute such as "LoyaltyNumber."</span></span>
* <span data-ttu-id="846e8-111">En identitetsleverantör har unika användar-ID som måste till exempel ”uniqueUserGUID” ”.</span><span class="sxs-lookup"><span data-stu-id="846e8-111">An identity provider has a unique user identifier that must be saved such as "uniqueUserGUID.""</span></span>
* <span data-ttu-id="846e8-112">En anpassad användare resa måste bevara tillståndet för användaren, till exempel ”migrationStatus”.</span><span class="sxs-lookup"><span data-stu-id="846e8-112">A custom user journey needs to persist the state of user such as "migrationStatus."</span></span>

<span data-ttu-id="846e8-113">Med Azure AD B2C kan du utöka uppsättningen attribut som lagras för varje användarkonto.</span><span class="sxs-lookup"><span data-stu-id="846e8-113">With Azure AD B2C, you can extend the set of attributes stored on each user account.</span></span> <span data-ttu-id="846e8-114">Du kan också läsa och skriva dessa attribut med hjälp av den [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="846e8-114">You can also read and write these attributes by using the [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

<span data-ttu-id="846e8-115">Egenskaper för webbtjänsttillägg utöka schemat för användarobjekt i katalogen.</span><span class="sxs-lookup"><span data-stu-id="846e8-115">Extension properties extend the schema of the user objects in the directory.</span></span>  <span data-ttu-id="846e8-116">Villkoren utökade egenskapen, det anpassade attributet och anpassade anspråk refererar till samma sak i kontexten för den här artikeln och namnet varierar beroende på kontext (program, objekt, princip).</span><span class="sxs-lookup"><span data-stu-id="846e8-116">The terms extension property, custom attribute and custom claim refer to the same thing in the context of this article and the name varies depending on the context (application, object, policy).</span></span>

<span data-ttu-id="846e8-117">Egenskaper för webbtjänsttillägg kan endast registreras på ett programobjekt trots att de kan innehålla data för en användare.</span><span class="sxs-lookup"><span data-stu-id="846e8-117">Extension properties can only be registered on an Application object even though they may contain data for a User.</span></span> <span data-ttu-id="846e8-118">Egenskapen är kopplad till programmet.</span><span class="sxs-lookup"><span data-stu-id="846e8-118">The property is attached to the application.</span></span> <span data-ttu-id="846e8-119">Programobjektet måste beviljas skrivbehörighet för att registrera en egenskap för tillägget.</span><span class="sxs-lookup"><span data-stu-id="846e8-119">The Application object must be granted write access to register an extension property.</span></span> <span data-ttu-id="846e8-120">100 tilläggsegenskaper (över alla typer och alla program) kan skrivas till något enskilt objekt.</span><span class="sxs-lookup"><span data-stu-id="846e8-120">100 Extension properties (across ALL types and ALL applications) can be written to any single object.</span></span> <span data-ttu-id="846e8-121">Egenskaper för webbtjänsttillägg läggs till i katalogen måltypen och blir omedelbart tillgängligt i Azure AD B2C directory-klient.</span><span class="sxs-lookup"><span data-stu-id="846e8-121">Extension properties are added to the target directory type and becomes immediately accessible in the Azure AD B2C directory tenant.</span></span>
<span data-ttu-id="846e8-122">Om programmet raderas tas de tilläggsegenskaper tillsammans med alla data i dem för alla användare också bort.</span><span class="sxs-lookup"><span data-stu-id="846e8-122">If the application is deleted, those Extension properties along with any data contained in them for all users are also removed.</span></span> <span data-ttu-id="846e8-123">Om en egenskap för tillägget tas bort av programmet tas bort den på katalogobjekt målet och de värden som tas bort.</span><span class="sxs-lookup"><span data-stu-id="846e8-123">If an extension property is deleted by the Application, it is removed on the target directory objects, and the values deleted.</span></span>

<span data-ttu-id="846e8-124">Egenskaper för webbtjänsttillägg finns bara i kontexten för ett program som är registrerade i klienten.</span><span class="sxs-lookup"><span data-stu-id="846e8-124">Extension properties exist only in the context of a registered  Application in the tenant.</span></span> <span data-ttu-id="846e8-125">Objekt-id för programmet måste inkluderas i TechnicalProfile som använder den.</span><span class="sxs-lookup"><span data-stu-id="846e8-125">The object id of that Application must be included in the TechnicalProfile that use it.</span></span>

>[!NOTE]
><span data-ttu-id="846e8-126">Azure AD B2C-katalogen innehåller vanligtvis ett webbprogram med namnet `b2c-extensions-app`.</span><span class="sxs-lookup"><span data-stu-id="846e8-126">The Azure AD B2C directory typically includes a Web App named `b2c-extensions-app`.</span></span>  <span data-ttu-id="846e8-127">Det här programmet används främst av b2c inbyggda principer för anpassade anspråk som skapats via Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="846e8-127">This application is primarily used by the b2c built-in  policies for the custom claims created via the Azure portal.</span></span>  <span data-ttu-id="846e8-128">Det här programmet rekommenderas för att registrera tillägg för b2c anpassade principer endast för avancerade användare.</span><span class="sxs-lookup"><span data-stu-id="846e8-128">Using this application to register extensions for b2c custom policies is recommended only for advanced users.</span></span>  <span data-ttu-id="846e8-129">Instruktioner för detta finns i avsnittet nästa steg i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="846e8-129">Instructions for this are included in the Next Steps section in this article.</span></span>


## <a name="creating-a-new-application-to-store-the-extension-properties"></a><span data-ttu-id="846e8-130">Skapa ett nytt program för att lagra egenskaper för webbtjänsttillägg</span><span class="sxs-lookup"><span data-stu-id="846e8-130">Creating a new application to store the extension properties</span></span>

1. <span data-ttu-id="846e8-131">Öppna en webbläsarsession och navigera till den [Azure får](https://portal.azure.com) och logga in med administratörsbehörighet för B2C-katalog som du vill konfigurera.</span><span class="sxs-lookup"><span data-stu-id="846e8-131">Open a browsing session and navigate to the [Azure porta](https://portal.azure.com) and sign in with administrative credentials of the B2C Directory you wish to configure.</span></span>
1. <span data-ttu-id="846e8-132">Klicka på **Azure Active Directory** på den vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="846e8-132">Click **Azure Active Directory** on the left navigation menu.</span></span> <span data-ttu-id="846e8-133">Du kan behöva hitta genom att välja fler tjänster >.</span><span class="sxs-lookup"><span data-stu-id="846e8-133">You may need to find it by selecting More services>.</span></span>
1. <span data-ttu-id="846e8-134">Välj **App registreringar** och på **nya appregistrering**</span><span class="sxs-lookup"><span data-stu-id="846e8-134">Select **App registrations** and click **New application registration**</span></span>
1. <span data-ttu-id="846e8-135">Ange följande rekommenderade poster:</span><span class="sxs-lookup"><span data-stu-id="846e8-135">Provide the following recommended entries:</span></span>
  * <span data-ttu-id="846e8-136">Ange ett namn för webbprogrammet: **WebApp-GraphAPI-DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="846e8-136">Specify a name for the web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
  * <span data-ttu-id="846e8-137">Programtyp: webb-app/API</span><span class="sxs-lookup"><span data-stu-id="846e8-137">Application type: Web app/API</span></span>
  * <span data-ttu-id="846e8-138">URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions inloggning</span><span class="sxs-lookup"><span data-stu-id="846e8-138">Sign-on URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions</span></span>
1. <span data-ttu-id="846e8-139">Välj ** Skapa.</span><span class="sxs-lookup"><span data-stu-id="846e8-139">Select **Create.</span></span> <span data-ttu-id="846e8-140">Åtgärden lyckades visas i den **meddelanden**</span><span class="sxs-lookup"><span data-stu-id="846e8-140">Successful completion appears in the **notifications**</span></span>
1. <span data-ttu-id="846e8-141">Välj det nyligen skapade webbprogrammet: **WebApp-GraphAPI-DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="846e8-141">Select the newly created web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
1. <span data-ttu-id="846e8-142">Välj inställningar: **nödvändiga behörigheter**</span><span class="sxs-lookup"><span data-stu-id="846e8-142">Select Settings: **Required permissions**</span></span>
1. <span data-ttu-id="846e8-143">Välj API **Windows Active Directory**</span><span class="sxs-lookup"><span data-stu-id="846e8-143">Select API **Windows Active Directory**</span></span>
1. <span data-ttu-id="846e8-144">Markera programbehörigheter: **läsning och skrivning katalogdata**, och **spara**</span><span class="sxs-lookup"><span data-stu-id="846e8-144">Place a checkmark in Application Permissions: **Read and write directory data**, and **Save**</span></span>
1. <span data-ttu-id="846e8-145">Välj **bevilja behörigheter** och bekräfta **Ja**.</span><span class="sxs-lookup"><span data-stu-id="846e8-145">Choose **Grant permissions** and confirm **Yes**.</span></span>
1. <span data-ttu-id="846e8-146">Kopiera till Urklipp och spara följande identifierare från WebApp-GraphAPI-DirectoryExtensions > Inställningar > Egenskaper ></span><span class="sxs-lookup"><span data-stu-id="846e8-146">Copy to your clipboard and save the following identifiers from WebApp-GraphAPI-DirectoryExtensions>Settings>Properties></span></span>
*  <span data-ttu-id="846e8-147">**Program-ID** .</span><span class="sxs-lookup"><span data-stu-id="846e8-147">**Application ID** .</span></span> <span data-ttu-id="846e8-148">Exempel:`103ee0e6-f92d-4183-b576-8c3739027780`</span><span class="sxs-lookup"><span data-stu-id="846e8-148">Example: `103ee0e6-f92d-4183-b576-8c3739027780`</span></span>
* <span data-ttu-id="846e8-149">**Objekt-ID**.</span><span class="sxs-lookup"><span data-stu-id="846e8-149">**Object ID**.</span></span> <span data-ttu-id="846e8-150">Exempel:`80d8296a-da0a-49ee-b6ab-fd232aa45201`</span><span class="sxs-lookup"><span data-stu-id="846e8-150">Example: `80d8296a-da0a-49ee-b6ab-fd232aa45201`</span></span>



## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a><span data-ttu-id="846e8-151">Ändra en anpassad princip för att lägga till ApplicationObjectId</span><span class="sxs-lookup"><span data-stu-id="846e8-151">Modifying your custom policy to add the ApplicationObjectId</span></span>

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item>
                <Item Key="ClientId">insert appId here</Item>
              </Metadata>
            <!-- End of changes -->
              <CryptographicKeys>
                <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
              </CryptographicKeys>
              <IncludeInSso>false</IncludeInSso>
              <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
            </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
```

>[!NOTE]
><span data-ttu-id="846e8-152">Den <TechnicalProfile Id="AAD-Common"> kallas för ”gemensamma” eftersom dess element ingår i och återanvändas i alla de Azure Active Directory TechnicalProfiles med hjälp av element:`<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span><span class="sxs-lookup"><span data-stu-id="846e8-152">The <TechnicalProfile Id="AAD-Common"> is referred to as "common" because its elements are included in and reused in all the Azure Active Directory TechnicalProfiles by using the element: `<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span></span>

>[!NOTE]
><span data-ttu-id="846e8-153">När TechnicalProfile skriver för första gången till den nyligen skapade utökade egenskapen, kan det uppstå ett enstaka fel.</span><span class="sxs-lookup"><span data-stu-id="846e8-153">When the TechnicalProfile writes for the first time to the newly created extension property, you may experience a one-time error.</span></span>  <span data-ttu-id="846e8-154">Den utökade egenskapen skapas första gången den används.</span><span class="sxs-lookup"><span data-stu-id="846e8-154">The extension property is created the first time it is used.</span></span>  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a><span data-ttu-id="846e8-155">Med hjälp av egenskapen extension / anpassade attribut i en användare resa</span><span class="sxs-lookup"><span data-stu-id="846e8-155">Using the new extension property / custom attribute in a user journey</span></span>


1. <span data-ttu-id="846e8-156">Öppna filen förlitande Party(RP) som beskriver principen Redigera användare resa.</span><span class="sxs-lookup"><span data-stu-id="846e8-156">Open the Relying Party(RP) file that describes your policy edit user journey.</span></span>  <span data-ttu-id="846e8-157">Om du startar kan det vara tillrådligt att hämta din redan konfigurerade version av filen RP PolicyEdit direkt från avsnittet anpassad princip för Azure B2C i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="846e8-157">If you are starting out, it may be advisable to download your already configured version of the RP-PolicyEdit file directly from the Azure B2C Custom Policy section in the Azure portal.</span></span>  <span data-ttu-id="846e8-158">Du kan också öppna XML-filen från lagringsmappen.</span><span class="sxs-lookup"><span data-stu-id="846e8-158">Alternatively, open your XML file from your storage folder.</span></span>
2. <span data-ttu-id="846e8-159">Lägg till ett anpassat anspråk `loyaltyId`.</span><span class="sxs-lookup"><span data-stu-id="846e8-159">Add a custom claim `loyaltyId`.</span></span>  <span data-ttu-id="846e8-160">Genom att lägga till ett anpassat anspråk i den `<RelyingParty>` element den skickas som en parameter för UserJourney TechnicalProfiles och ingår i token för programmet.</span><span class="sxs-lookup"><span data-stu-id="846e8-160">By including the custom claim in the `<RelyingParty>` element, it is passed as a parameter to the UserJourney TechnicalProfiles and included in the token for the application.</span></span>
```xml
<RelyingParty>
   <DefaultUserJourney ReferenceId="ProfileEdit" />
   <TechnicalProfile Id="PolicyProfile">
     <DisplayName>PolicyProfile</DisplayName>
     <Protocol Name="OpenIdConnect" />
     <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
       <OutputClaim ClaimTypeReferenceId="city" />

       <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />

     </OutputClaims>
     <SubjectNamingInfo ClaimType="sub" />
   </TechnicalProfile>
 </RelyingParty>
 ```
3. <span data-ttu-id="846e8-161">Lägg till en definition av anspråk i princip tilläggsfilen `TrustFrameworkExtensions.xml` inuti den `<ClaimsSchema>` element som visas.</span><span class="sxs-lookup"><span data-stu-id="846e8-161">Add a claim definition to the Extension policy file  `TrustFrameworkExtensions.xml` inside the `<ClaimsSchema>` element as shown.</span></span>
```xml
<ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
            <DisplayName>Loyalty Identification Tag</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your loyalty number from your membership card</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
</ClaimsSchema>
```
4. <span data-ttu-id="846e8-162">Lägg till samma anspråk definition till den grundläggande principfilen `TrustFrameworkBase.xml`.</span><span class="sxs-lookup"><span data-stu-id="846e8-162">Add the same claim definition to the Base policy file `TrustFrameworkBase.xml`.</span></span>  
><span data-ttu-id="846e8-163">Lägga till en `ClaimType` definition i både bas- och tilläggsfilen är normalt inte behövs, men eftersom nästa steg lägger till extension_loyaltyId TechnicalProfiles i filen bas, princip verifieraren avvisar överföringen av grundläggande filen utan att den.</span><span class="sxs-lookup"><span data-stu-id="846e8-163">Adding a `ClaimType` definition in both the base and the extensions file is normally not necessary, however since the next steps will add the extension_loyaltyId to TechnicalProfiles in the Base file, the policy validator will reject the upload of the base file without it.</span></span>
><span data-ttu-id="846e8-164">Det kan vara praktiskt att spåra körningen av användaren transporten med namnet ”ProfileEdit” i filen TrustFrameworkBase.xml.</span><span class="sxs-lookup"><span data-stu-id="846e8-164">It may be useful to trace the execution of the user journey named "ProfileEdit" in the TrustFrameworkBase.xml file.</span></span>  <span data-ttu-id="846e8-165">Sök efter användare transporten med samma namn i redigeringsprogrammet och Observera att Orchestration steg 5 anropar TechnicalProfileReferenceID = ”SelfAsserted ProfileUpdate”.</span><span class="sxs-lookup"><span data-stu-id="846e8-165">Search for the user journey of the same name in your editor and observe that Orchestration Step 5 invokes the TechnicalProfileReferenceID="SelfAsserted-ProfileUpdate".</span></span>  <span data-ttu-id="846e8-166">Söka och inspektera den här TechnicalProfile för att bekanta dig med den.</span><span class="sxs-lookup"><span data-stu-id="846e8-166">Search and inspect this TechnicalProfile to familiarize yourself with the flow.</span></span>
5. <span data-ttu-id="846e8-167">Lägg till loyaltyId som inkommande och utgående anspråk i TechnicalProfile ”SelfAsserted ProfileUpdate”</span><span class="sxs-lookup"><span data-stu-id="846e8-167">Add loyaltyId as input and output claim in the TechnicalProfile "SelfAsserted-ProfileUpdate"</span></span>
```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
          <DisplayName>User ID signup</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>

            <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
            <InputClaim ClaimTypeReferenceId="userPrincipalName" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. <span data-ttu-id="846e8-168">Lägga till anspråk i TechnicalProfile ”AAD-UserWriteProfileUsingObjectId” att bevara värdet för anspråket i tilläggsegenskapen för den aktuella användaren i katalogen.</span><span class="sxs-lookup"><span data-stu-id="846e8-168">Add claim in TechnicalProfile "AAD-UserWriteProfileUsingObjectId" to persist the value of the claim in the extension property, for the current user in the directory.</span></span>
```xml
<TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
          <Metadata>
            <Item Key="Operation">Write</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <PersistedClaims>
            <!-- Required claims -->
            <PersistedClaim ClaimTypeReferenceId="objectId" />

            <!-- Optional claims -->
            <PersistedClaim ClaimTypeReferenceId="givenName" />
            <PersistedClaim ClaimTypeReferenceId="surname" />
            <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />

          </PersistedClaims>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
```
7. <span data-ttu-id="846e8-169">Lägga till anspråk i TechnicalProfile ”AAD-UserReadUsingObjectId” att läsa värdet för attributet för anknytning varje gång en användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="846e8-169">Add claim in TechnicalProfile "AAD-UserReadUsingObjectId" to read the value of the extension attribute every time a user logs in.</span></span> <span data-ttu-id="846e8-170">Hittills har TechnicalProfiles ändrats i flödet för lokala konton.</span><span class="sxs-lookup"><span data-stu-id="846e8-170">Thus far the TechnicalProfiles have been changed in the flow of local accounts only.</span></span>  <span data-ttu-id="846e8-171">Om det nya attributet önskade i flödet för social/federerat konto, måste en annan uppsättning TechnicalProfiles ändras.</span><span class="sxs-lookup"><span data-stu-id="846e8-171">If the new attribute is desired in the flow of a social/federated account, a different set of TechnicalProfiles needs to be changed.</span></span> <span data-ttu-id="846e8-172">Se nästa steg.</span><span class="sxs-lookup"><span data-stu-id="846e8-172">See Next Steps.</span></span>

```xml
<!-- The following technical profile is used to read data after user authenticates. -->
     <TechnicalProfile Id="AAD-UserReadUsingObjectId">
       <Metadata>
         <Item Key="Operation">Read</Item>
         <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
       </Metadata>
       <IncludeInSso>false</IncludeInSso>
       <InputClaims>
         <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
       </InputClaims>
       <OutputClaims>
         <!-- Optional claims -->
         <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
         <OutputClaim ClaimTypeReferenceId="displayName" />
         <OutputClaim ClaimTypeReferenceId="otherMails" />
         <OutputClaim ClaimTypeReferenceId="givenName" />
         <OutputClaim ClaimTypeReferenceId="surname" />
         <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
       </OutputClaims>
       <IncludeTechnicalProfile ReferenceId="AAD-Common" />
     </TechnicalProfile>
```


>[!IMPORTANT]
><span data-ttu-id="846e8-173">Elementet IncludeTechnicalProfile lägger till alla element i AAD-gemensamma denna TechnicalProfile.</span><span class="sxs-lookup"><span data-stu-id="846e8-173">The IncludeTechnicalProfile element adds all the elements of AAD-Common to this TechnicalProfile.</span></span>

## <a name="test-the-custom-policy-using-run-now"></a><span data-ttu-id="846e8-174">Testa den anpassade principen med hjälp av ”kör nu”</span><span class="sxs-lookup"><span data-stu-id="846e8-174">Test the custom policy using "Run Now"</span></span>
1. <span data-ttu-id="846e8-175">Öppna den **Azure AD B2C bladet** och gå till **identitet upplevelse Framework > anpassade principer**.</span><span class="sxs-lookup"><span data-stu-id="846e8-175">Open the **Azure AD B2C Blade** and navigate to **Identity Experience Framework > Custom policies**.</span></span>
1. <span data-ttu-id="846e8-176">Välj den anpassade principen som du överfört och klicka på den **kör nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="846e8-176">Select the custom policy that you uploaded, and click the **Run now** button.</span></span>
1. <span data-ttu-id="846e8-177">Du ska kunna logga med en e-postadress.</span><span class="sxs-lookup"><span data-stu-id="846e8-177">You should be able to sign up using an email address.</span></span>

<span data-ttu-id="846e8-178">Token id skickas tillbaka till ditt program innehåller egenskapen extension som ett anpassat anspråk som föregås av extension_loyaltyId.</span><span class="sxs-lookup"><span data-stu-id="846e8-178">The  id token sent back to your application includes the new extension property as a custom claim preceded by extension_loyaltyId.</span></span> <span data-ttu-id="846e8-179">Visa exempel.</span><span class="sxs-lookup"><span data-stu-id="846e8-179">See example.</span></span>

```
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a><span data-ttu-id="846e8-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="846e8-180">Next steps</span></span>

<span data-ttu-id="846e8-181">Lägga till nya anspråk till flödena för sociala inloggningsnamn genom att ändra TechnicalProfiles i listan.</span><span class="sxs-lookup"><span data-stu-id="846e8-181">Add the new claim to the flows for social account logins by changing the TechnicalProfiles listed.</span></span> <span data-ttu-id="846e8-182">Dessa två TechnicalProfiles som används av sociala/federerad inloggningsnamn att skriva och läsa informationen med hjälp av alternativeSecurityId som lokaliserare för användarobjektet.</span><span class="sxs-lookup"><span data-stu-id="846e8-182">These two TechnicalProfiles are used by social/federated account logins to write and read the user data using the alternativeSecurityId as the locator of the user object.</span></span>
```
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

<span data-ttu-id="846e8-183">Med hjälp av samma tilläggsattribut mellan inbyggda och anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="846e8-183">Using the same extension attributes between built-in and custom policies.</span></span>
<span data-ttu-id="846e8-184">När du lägger till tilläggsattribut (aka anpassade attribut) via portalen upplevelsen attributen registreras med hjälp av den ** b2c-tillägg-app som finns i varje b2c-klient.</span><span class="sxs-lookup"><span data-stu-id="846e8-184">When you add extension attributes (aka custom attributes) via the portal experience, those attributes are registered using the **b2c-extensions-app that exists in every b2c tenant.</span></span>  <span data-ttu-id="846e8-185">Du använder dessa tilläggsattribut i en egen princip:</span><span class="sxs-lookup"><span data-stu-id="846e8-185">To use these extension attributes in your custom policy:</span></span>
1. <span data-ttu-id="846e8-186">Gå till under din b2c-klient i portal.azure.com **Azure Active Directory** och välj **App registreringar**</span><span class="sxs-lookup"><span data-stu-id="846e8-186">Within your b2c tenant in portal.azure.com, navigate to **Azure Active Directory** and select **App registrations**</span></span>
2. <span data-ttu-id="846e8-187">Hitta din **b2c-tillägg-app** och markera den</span><span class="sxs-lookup"><span data-stu-id="846e8-187">Find your **b2c-extensions-app** and select it</span></span>
3. <span data-ttu-id="846e8-188">Under 'Essentials-post i **program-ID** och **objekt-ID**</span><span class="sxs-lookup"><span data-stu-id="846e8-188">Under 'Essentials' record the **Application ID** and the **Object ID**</span></span>
4. <span data-ttu-id="846e8-189">Inkludera dem i din AAD-gemensamma tekniska Profilmetadata som på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="846e8-189">Include them in your AAD-Common Technical profile metadata like as follows:</span></span>

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is the "Object ID" from the "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is the "Application ID" from the "b2c-extensions-app"-->
              </Metadata>
```

<span data-ttu-id="846e8-190">Om du vill behålla konsekvens med portalen upplevelsen, skapa dessa attribut med hjälp av portalen UI *innan* du använder dem i din anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="846e8-190">To keep consistency with the portal experience, create these attributes using the portal UI *before* you use them in your custom policies.</span></span>  <span data-ttu-id="846e8-191">När du skapar ett attribut ”ActivationStatus” i portalen måste du referera till den på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="846e8-191">When you create an attribute "ActivationStatus" in the portal, you must refer to it as follows:</span></span>

```
extension_ActivationStatus in the custom policy
extension_<app-guid>_ActivationStatus via the Graph API.
```


## <a name="reference"></a><span data-ttu-id="846e8-192">Referens</span><span class="sxs-lookup"><span data-stu-id="846e8-192">Reference</span></span>

* <span data-ttu-id="846e8-193">En **tekniska profil (TP)** är en elementtyp som kan betraktas som en *funktionen* som definierar en slutpunkt namn, dess metadata, dess protokoll och information utbyten av anspråk som identitet upplevelse Framework ska utföra.</span><span class="sxs-lookup"><span data-stu-id="846e8-193">A **Technical Profile (TP)** is an element type that can be thought of as a *function* that defines an endpoint’s name, its metadata, its protocol, and details the exchange of claims that the Identity Experience Framework should perform.</span></span>  <span data-ttu-id="846e8-194">När detta *funktionen* anropas i en orchestration-steg eller från en annan TechnicalProfile, InputClaims och OutputClaims tillhandahålls som parametrar av anroparen.</span><span class="sxs-lookup"><span data-stu-id="846e8-194">When this *function* is called in an orchestration step or from another TechnicalProfile, the InputClaims and OutputClaims are provided as parameters by the caller.</span></span>


* <span data-ttu-id="846e8-195">Fullständig behandling på tilläggsegenskaper finns i artikeln [DIRECTORY-SCHEMAUTÖKNINGAR | GRAPH API-BEGREPP](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span><span class="sxs-lookup"><span data-stu-id="846e8-195">For full treatment on extension properties, see the article [DIRECTORY SCHEMA EXTENSIONS | GRAPH API CONCEPTS](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span></span>

>[!NOTE]
><span data-ttu-id="846e8-196">Tilläggsattribut i Graph API är namngivna med konventionen `extension_ApplicationObjectID_attributename`.</span><span class="sxs-lookup"><span data-stu-id="846e8-196">Extension attributes in Graph API are named using the convention `extension_ApplicationObjectID_attributename`.</span></span> <span data-ttu-id="846e8-197">Anpassade principer som refererar till tillägg attribut som extension_attributename, vilket utesluter ApplicationObjectId i XML</span><span class="sxs-lookup"><span data-stu-id="846e8-197">Custom policies refer to extensions attributes as extension_attributename, thus omitting the ApplicationObjectId in the XML</span></span>

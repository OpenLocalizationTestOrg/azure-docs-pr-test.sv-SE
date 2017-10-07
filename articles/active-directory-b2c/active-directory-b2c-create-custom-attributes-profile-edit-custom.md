---
title: "Azure Active Directory B2C: Lägga till egna attribut toocustom principer och använda i profilen redigera | Microsoft Docs"
description: "En genomgång av använder tilläggsegenskaper, anpassade attribut och inkludera dem i hello användargränssnittet"
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
ms.openlocfilehash: 8cc9c6a38d7652797ba54a3e02078ac2bf4a693b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a><span data-ttu-id="8b9ce-103">Azure Active Directory B2C: Skapa och använda anpassade attribut i en anpassad profil redigera principen</span><span class="sxs-lookup"><span data-stu-id="8b9ce-103">Azure Active Directory B2C: Creating and using custom attributes in a custom profile edit policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="8b9ce-104">I den här artikeln, skapa ett anpassat attribut i Azure AD B2C-katalogen och använda den här nya attributet som ett anpassat anspråk i hello profil Redigera användare resa.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-104">In this article, you create a custom attribute in your Azure AD B2C directory and use this new attribute as a custom claim in hello profile edit user journey.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b9ce-105">Krav</span><span class="sxs-lookup"><span data-stu-id="8b9ce-105">Prerequisites</span></span>

<span data-ttu-id="8b9ce-106">Fullständig hello stegen i artikeln hello [komma igång med principer för anpassade](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="8b9ce-106">Complete hello steps in hello article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>

## <a name="use-custom-attributes-toocollect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a><span data-ttu-id="8b9ce-107">Använd anpassade attribut toocollect information om dina kunder i Azure Active Directory B2C anpassade principer</span><span class="sxs-lookup"><span data-stu-id="8b9ce-107">Use custom attributes toocollect information about your customers in Azure Active Directory B2C using custom policies</span></span>
<span data-ttu-id="8b9ce-108">Din Azure Active Directory (AD Azure) B2C-katalog som levereras med en inbyggd uppsättning attribut: namn, efternamn, ort, postnummer, userPrincipalName får osv.  Du behöver ofta toocreate egna attribut.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-108">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of attributes: Given Name, Surname, City, Postal Code, userPrincipalName, etc.  You often need toocreate your own attributes.</span></span>  <span data-ttu-id="8b9ce-109">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8b9ce-109">For example:</span></span>
* <span data-ttu-id="8b9ce-110">En kund-riktade programmet behöver toopersist ett attribut, till exempel ”LoyaltyNumber”.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-110">A customer-facing application needs toopersist an attribute such as "LoyaltyNumber."</span></span>
* <span data-ttu-id="8b9ce-111">En identitetsleverantör har unika användar-ID som måste till exempel ”uniqueUserGUID” ”.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-111">An identity provider has a unique user identifier that must be saved such as "uniqueUserGUID.""</span></span>
* <span data-ttu-id="8b9ce-112">En anpassad användare resa måste toopersist hello tillståndet för användaren, till exempel ”migrationStatus”.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-112">A custom user journey needs toopersist hello state of user such as "migrationStatus."</span></span>

<span data-ttu-id="8b9ce-113">Med Azure AD B2C kan du utöka hello uppsättning attribut som lagras för varje användarkonto.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-113">With Azure AD B2C, you can extend hello set of attributes stored on each user account.</span></span> <span data-ttu-id="8b9ce-114">Du kan också läsa och skriva dessa attribut med hjälp av hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="8b9ce-114">You can also read and write these attributes by using hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

<span data-ttu-id="8b9ce-115">Egenskaper för webbtjänsttillägg utöka hello hello användarobjekt i hello directory-schema.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-115">Extension properties extend hello schema of hello user objects in hello directory.</span></span>  <span data-ttu-id="8b9ce-116">hello villkoren utökade egenskapen, anpassat attribut och anpassade anspråk refererar toohello samma sak i hello kontexten för den här artikeln och hello varierar beroende på hello kontext (program, objekt, princip).</span><span class="sxs-lookup"><span data-stu-id="8b9ce-116">hello terms extension property, custom attribute and custom claim refer toohello same thing in hello context of this article and hello name varies depending on hello context (application, object, policy).</span></span>

<span data-ttu-id="8b9ce-117">Egenskaper för webbtjänsttillägg kan endast registreras på ett programobjekt trots att de kan innehålla data för en användare.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-117">Extension properties can only be registered on an Application object even though they may contain data for a User.</span></span> <span data-ttu-id="8b9ce-118">hello-egenskapen är anslutna toohello program.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-118">hello property is attached toohello application.</span></span> <span data-ttu-id="8b9ce-119">hello programobjektet måste vara beviljade skrivbehörighet tooregister en egenskap för tillägget.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-119">hello Application object must be granted write access tooregister an extension property.</span></span> <span data-ttu-id="8b9ce-120">100 tilläggsegenskaper (över alla typer och alla program) kan skrivas tooany enskilt objekt.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-120">100 Extension properties (across ALL types and ALL applications) can be written tooany single object.</span></span> <span data-ttu-id="8b9ce-121">Egenskaper för webbtjänsttillägg läggs toohello directory måltypen och blir omedelbart tillgängligt i directory hello Azure AD B2C-klient.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-121">Extension properties are added toohello target directory type and becomes immediately accessible in hello Azure AD B2C directory tenant.</span></span>
<span data-ttu-id="8b9ce-122">Om programmet hello raderas tas de tilläggsegenskaper tillsammans med alla data i dem för alla användare också bort.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-122">If hello application is deleted, those Extension properties along with any data contained in them for all users are also removed.</span></span> <span data-ttu-id="8b9ce-123">Om en egenskap för tillägget tas bort av hello program tas det bort på hello målet katalogobjekt och hello värden tas bort.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-123">If an extension property is deleted by hello Application, it is removed on hello target directory objects, and hello values deleted.</span></span>

<span data-ttu-id="8b9ce-124">Egenskaper för webbtjänsttillägg finns bara i hello registrerade programmet i hello-klient.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-124">Extension properties exist only in hello context of a registered  Application in hello tenant.</span></span> <span data-ttu-id="8b9ce-125">hello objekt-id för programmet måste inkluderas i hello TechnicalProfile som använder den.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-125">hello object id of that Application must be included in hello TechnicalProfile that use it.</span></span>

>[!NOTE]
><span data-ttu-id="8b9ce-126">hello Azure AD B2C-katalogen innehåller vanligtvis ett webbprogram med namnet `b2c-extensions-app`.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-126">hello Azure AD B2C directory typically includes a Web App named `b2c-extensions-app`.</span></span>  <span data-ttu-id="8b9ce-127">Det här programmet används främst av hello b2c inbyggda principer för anpassade hello anspråk som skapats via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-127">This application is primarily used by hello b2c built-in  policies for hello custom claims created via hello Azure portal.</span></span>  <span data-ttu-id="8b9ce-128">Med det här programmet tooregister tillägg för b2c anpassade principer rekommenderas endast för avancerade användare.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-128">Using this application tooregister extensions for b2c custom policies is recommended only for advanced users.</span></span>  <span data-ttu-id="8b9ce-129">Instruktioner för detta ingår i hello nästa avsnitt i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-129">Instructions for this are included in hello Next Steps section in this article.</span></span>


## <a name="creating-a-new-application-toostore-hello-extension-properties"></a><span data-ttu-id="8b9ce-130">Skapa en ny toostore programmet hello tilläggsegenskaper</span><span class="sxs-lookup"><span data-stu-id="8b9ce-130">Creating a new application toostore hello extension properties</span></span>

1. <span data-ttu-id="8b9ce-131">Öppna en webbläsarsession och navigera toohello [Azure får](https://portal.azure.com) och logga in med administratörsbehörighet på hello B2C-katalog som du vill tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-131">Open a browsing session and navigate toohello [Azure porta](https://portal.azure.com) and sign in with administrative credentials of hello B2C Directory you wish tooconfigure.</span></span>
1. <span data-ttu-id="8b9ce-132">Klicka på **Azure Active Directory** på hello vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-132">Click **Azure Active Directory** on hello left navigation menu.</span></span> <span data-ttu-id="8b9ce-133">Du kan behöva toofind den genom att välja fler tjänster >.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-133">You may need toofind it by selecting More services>.</span></span>
1. <span data-ttu-id="8b9ce-134">Välj **App registreringar** och på **nya appregistrering**</span><span class="sxs-lookup"><span data-stu-id="8b9ce-134">Select **App registrations** and click **New application registration**</span></span>
1. <span data-ttu-id="8b9ce-135">Ange hello följande rekommenderade poster:</span><span class="sxs-lookup"><span data-stu-id="8b9ce-135">Provide hello following recommended entries:</span></span>
  * <span data-ttu-id="8b9ce-136">Ange ett namn för hello-webbprogram: **WebApp-GraphAPI-DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="8b9ce-136">Specify a name for hello web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
  * <span data-ttu-id="8b9ce-137">Programtyp: webb-app/API</span><span class="sxs-lookup"><span data-stu-id="8b9ce-137">Application type: Web app/API</span></span>
  * <span data-ttu-id="8b9ce-138">URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions inloggning</span><span class="sxs-lookup"><span data-stu-id="8b9ce-138">Sign-on URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions</span></span>
1. <span data-ttu-id="8b9ce-139">Välj ** Skapa.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-139">Select **Create.</span></span> <span data-ttu-id="8b9ce-140">Åtgärden lyckades visas i hello **meddelanden**</span><span class="sxs-lookup"><span data-stu-id="8b9ce-140">Successful completion appears in hello **notifications**</span></span>
1. <span data-ttu-id="8b9ce-141">Välj hello nyskapad webbprogram: **WebApp-GraphAPI-DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="8b9ce-141">Select hello newly created web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
1. <span data-ttu-id="8b9ce-142">Välj inställningar: **nödvändiga behörigheter**</span><span class="sxs-lookup"><span data-stu-id="8b9ce-142">Select Settings: **Required permissions**</span></span>
1. <span data-ttu-id="8b9ce-143">Välj API **Windows Active Directory**</span><span class="sxs-lookup"><span data-stu-id="8b9ce-143">Select API **Windows Active Directory**</span></span>
1. <span data-ttu-id="8b9ce-144">Markera programbehörigheter: **läsning och skrivning katalogdata**, och **spara**</span><span class="sxs-lookup"><span data-stu-id="8b9ce-144">Place a checkmark in Application Permissions: **Read and write directory data**, and **Save**</span></span>
1. <span data-ttu-id="8b9ce-145">Välj **bevilja behörigheter** och bekräfta **Ja**.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-145">Choose **Grant permissions** and confirm **Yes**.</span></span>
1. <span data-ttu-id="8b9ce-146">Kopiera tooyour Urklipp och spara hello följande identifierare från WebApp-GraphAPI-DirectoryExtensions > Inställningar > Egenskaper ></span><span class="sxs-lookup"><span data-stu-id="8b9ce-146">Copy tooyour clipboard and save hello following identifiers from WebApp-GraphAPI-DirectoryExtensions>Settings>Properties></span></span>
*  <span data-ttu-id="8b9ce-147">**Program-ID** .</span><span class="sxs-lookup"><span data-stu-id="8b9ce-147">**Application ID** .</span></span> <span data-ttu-id="8b9ce-148">Exempel:`103ee0e6-f92d-4183-b576-8c3739027780`</span><span class="sxs-lookup"><span data-stu-id="8b9ce-148">Example: `103ee0e6-f92d-4183-b576-8c3739027780`</span></span>
* <span data-ttu-id="8b9ce-149">**Objekt-ID**.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-149">**Object ID**.</span></span> <span data-ttu-id="8b9ce-150">Exempel:`80d8296a-da0a-49ee-b6ab-fd232aa45201`</span><span class="sxs-lookup"><span data-stu-id="8b9ce-150">Example: `80d8296a-da0a-49ee-b6ab-fd232aa45201`</span></span>



## <a name="modifying-your-custom-policy-tooadd-hello-applicationobjectid"></a><span data-ttu-id="8b9ce-151">Ändra en anpassad princip tooadd hello ApplicationObjectId</span><span class="sxs-lookup"><span data-stu-id="8b9ce-151">Modifying your custom policy tooadd hello ApplicationObjectId</span></span>

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
><span data-ttu-id="8b9ce-152">Hej <TechnicalProfile Id="AAD-Common"> är refererad tooas ”gemensamma” eftersom dess element ingår i och återanvändas i alla hello Azure Active Directory TechnicalProfiles med hjälp av hello element:`<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span><span class="sxs-lookup"><span data-stu-id="8b9ce-152">hello <TechnicalProfile Id="AAD-Common"> is referred tooas "common" because its elements are included in and reused in all hello Azure Active Directory TechnicalProfiles by using hello element: `<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span></span>

>[!NOTE]
><span data-ttu-id="8b9ce-153">När hello TechnicalProfile skriver för hello första gången toohello nyskapad tilläggsegenskapen, kan det uppstå ett enstaka fel.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-153">When hello TechnicalProfile writes for hello first time toohello newly created extension property, you may experience a one-time error.</span></span>  <span data-ttu-id="8b9ce-154">Hej tilläggsegenskapen skapas hello första gången den används.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-154">hello extension property is created hello first time it is used.</span></span>  

## <a name="using-hello-new-extension-property--custom-attribute-in-a-user-journey"></a><span data-ttu-id="8b9ce-155">Med hjälp av hello nya tilläggsegenskapen / anpassade attribut i en användare resa</span><span class="sxs-lookup"><span data-stu-id="8b9ce-155">Using hello new extension property / custom attribute in a user journey</span></span>


1. <span data-ttu-id="8b9ce-156">Öppna hello förlitande Party(RP)-fil som beskriver principen Redigera användare resa.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-156">Open hello Relying Party(RP) file that describes your policy edit user journey.</span></span>  <span data-ttu-id="8b9ce-157">Om du startar kan det vara lämpligt toodownload din redan konfigurerade version av hello RP PolicyEdit filen direkt från hello anpassad princip för Azure B2C-avsnittet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-157">If you are starting out, it may be advisable toodownload your already configured version of hello RP-PolicyEdit file directly from hello Azure B2C Custom Policy section in hello Azure portal.</span></span>  <span data-ttu-id="8b9ce-158">Du kan också öppna XML-filen från lagringsmappen.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-158">Alternatively, open your XML file from your storage folder.</span></span>
2. <span data-ttu-id="8b9ce-159">Lägg till ett anpassat anspråk `loyaltyId`.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-159">Add a custom claim `loyaltyId`.</span></span>  <span data-ttu-id="8b9ce-160">Genom att inkludera hello anpassade anspråk i hello `<RelyingParty>` element den skickas som en parameter toohello UserJourney TechnicalProfiles och ingår i hello token för hello program.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-160">By including hello custom claim in hello `<RelyingParty>` element, it is passed as a parameter toohello UserJourney TechnicalProfiles and included in hello token for hello application.</span></span>
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
3. <span data-ttu-id="8b9ce-161">Lägg till en principfil för anspråk definition toohello tillägget `TrustFrameworkExtensions.xml` inuti hello `<ClaimsSchema>` element som visas.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-161">Add a claim definition toohello Extension policy file  `TrustFrameworkExtensions.xml` inside hello `<ClaimsSchema>` element as shown.</span></span>
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
4. <span data-ttu-id="8b9ce-162">Lägg till hello samma anspråk definitionsfilen toohello grundläggande princip `TrustFrameworkBase.xml`.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-162">Add hello same claim definition toohello Base policy file `TrustFrameworkBase.xml`.</span></span>  
><span data-ttu-id="8b9ce-163">Lägga till en `ClaimType` definition i både hello bas- och hello tillägg är normalt inte nödvändigt, men eftersom hello nästa steg läggs hello extension_loyaltyId tooTechnicalProfiles i grundläggande hello-filen, hello princip verifieraren avvisar hello överför hello grundläggande filen utan att den.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-163">Adding a `ClaimType` definition in both hello base and hello extensions file is normally not necessary, however since hello next steps will add hello extension_loyaltyId tooTechnicalProfiles in hello Base file, hello policy validator will reject hello upload of hello base file without it.</span></span>
><span data-ttu-id="8b9ce-164">Det kan vara användbart tootrace hello körningen av hello användaren resa med namnet ”ProfileEdit” i hello TrustFrameworkBase.xml-filen.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-164">It may be useful tootrace hello execution of hello user journey named "ProfileEdit" in hello TrustFrameworkBase.xml file.</span></span>  <span data-ttu-id="8b9ce-165">Sök efter hello användaren resa av hello samma namn i redigeringsprogrammet och Observera att Orchestration steg 5 anropar hello TechnicalProfileReferenceID = ”SelfAsserted ProfileUpdate”.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-165">Search for hello user journey of hello same name in your editor and observe that Orchestration Step 5 invokes hello TechnicalProfileReferenceID="SelfAsserted-ProfileUpdate".</span></span>  <span data-ttu-id="8b9ce-166">Söka och inspektera den här TechnicalProfile toofamiliarize själv med hello flödet.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-166">Search and inspect this TechnicalProfile toofamiliarize yourself with hello flow.</span></span>
5. <span data-ttu-id="8b9ce-167">Lägg till loyaltyId som inkommande och utgående anspråk i hello TechnicalProfile ”SelfAsserted ProfileUpdate”</span><span class="sxs-lookup"><span data-stu-id="8b9ce-167">Add loyaltyId as input and output claim in hello TechnicalProfile "SelfAsserted-ProfileUpdate"</span></span>
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

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. <span data-ttu-id="8b9ce-168">Lägga till anspråk i TechnicalProfile ”AAD-UserWriteProfileUsingObjectId” toopersist hello värdet för hello anspråk i hello tilläggsegenskapen för hello aktuella användare i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-168">Add claim in TechnicalProfile "AAD-UserWriteProfileUsingObjectId" toopersist hello value of hello claim in hello extension property, for hello current user in hello directory.</span></span>
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
7. <span data-ttu-id="8b9ce-169">Lägga till anspråk i TechnicalProfile ”AAD-UserReadUsingObjectId” tooread hello värdet för hello tillägget attributet varje gång en användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-169">Add claim in TechnicalProfile "AAD-UserReadUsingObjectId" tooread hello value of hello extension attribute every time a user logs in.</span></span> <span data-ttu-id="8b9ce-170">Hittills hello TechnicalProfiles har ändrats i hello flödet för lokala konton.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-170">Thus far hello TechnicalProfiles have been changed in hello flow of local accounts only.</span></span>  <span data-ttu-id="8b9ce-171">Om hello nytt attribut önskas i hello flödet för social/federerat konto måste en annan uppsättning TechnicalProfiles toobe ändras.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-171">If hello new attribute is desired in hello flow of a social/federated account, a different set of TechnicalProfiles needs toobe changed.</span></span> <span data-ttu-id="8b9ce-172">Se nästa steg.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-172">See Next Steps.</span></span>

```xml
<!-- hello following technical profile is used tooread data after user authenticates. -->
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
><span data-ttu-id="8b9ce-173">Hej IncludeTechnicalProfile element lägger till alla hello-element i AAD-gemensamma toothis TechnicalProfile.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-173">hello IncludeTechnicalProfile element adds all hello elements of AAD-Common toothis TechnicalProfile.</span></span>

## <a name="test-hello-custom-policy-using-run-now"></a><span data-ttu-id="8b9ce-174">Testa hello anpassad princip med ”Kör nu”</span><span class="sxs-lookup"><span data-stu-id="8b9ce-174">Test hello custom policy using "Run Now"</span></span>
1. <span data-ttu-id="8b9ce-175">Öppna hello **Azure AD B2C bladet** och navigera för**identitet upplevelse Framework > anpassade principer**.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-175">Open hello **Azure AD B2C Blade** and navigate too**Identity Experience Framework > Custom policies**.</span></span>
1. <span data-ttu-id="8b9ce-176">Välj hello anpassad princip som du överfört och klicka på hello **kör nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-176">Select hello custom policy that you uploaded, and click hello **Run now** button.</span></span>
1. <span data-ttu-id="8b9ce-177">Du ska kunna toosign med en e-postadress.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-177">You should be able toosign up using an email address.</span></span>

<span data-ttu-id="8b9ce-178">hello-ID-token som skickas tillbaka tooyour program innehåller hello nya tilläggsegenskapen som ett anpassat anspråk som föregås av extension_loyaltyId.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-178">hello  id token sent back tooyour application includes hello new extension property as a custom claim preceded by extension_loyaltyId.</span></span> <span data-ttu-id="8b9ce-179">Visa exempel.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-179">See example.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8b9ce-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b9ce-180">Next steps</span></span>

<span data-ttu-id="8b9ce-181">Lägg till hello nya anspråk toohello flöden sociala inloggningsnamn genom att ändra hello TechnicalProfiles visas.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-181">Add hello new claim toohello flows for social account logins by changing hello TechnicalProfiles listed.</span></span> <span data-ttu-id="8b9ce-182">Dessa två TechnicalProfiles används av sociala/federerat konto inloggningar toowrite och läsa hello användardata med hello alternativeSecurityId som hello lokaliserare för hello användarobjekt.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-182">These two TechnicalProfiles are used by social/federated account logins toowrite and read hello user data using hello alternativeSecurityId as hello locator of hello user object.</span></span>
```
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

<span data-ttu-id="8b9ce-183">Med hjälp av hello samma tilläggsattribut mellan inbyggda och anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-183">Using hello same extension attributes between built-in and custom policies.</span></span>
<span data-ttu-id="8b9ce-184">När du lägger till tilläggsattribut (aka anpassade attribut) via hello-portaler attributen registreras med hjälp av hello ** b2c-tillägg-app som finns i varje b2c-klient.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-184">When you add extension attributes (aka custom attributes) via hello portal experience, those attributes are registered using hello **b2c-extensions-app that exists in every b2c tenant.</span></span>  <span data-ttu-id="8b9ce-185">toouse dessa tilläggsattribut i en egen princip:</span><span class="sxs-lookup"><span data-stu-id="8b9ce-185">toouse these extension attributes in your custom policy:</span></span>
1. <span data-ttu-id="8b9ce-186">Navigera för i din b2c-klient i portal.azure.com**Azure Active Directory** och välj **App registreringar**</span><span class="sxs-lookup"><span data-stu-id="8b9ce-186">Within your b2c tenant in portal.azure.com, navigate too**Azure Active Directory** and select **App registrations**</span></span>
2. <span data-ttu-id="8b9ce-187">Hitta din **b2c-tillägg-app** och markera den</span><span class="sxs-lookup"><span data-stu-id="8b9ce-187">Find your **b2c-extensions-app** and select it</span></span>
3. <span data-ttu-id="8b9ce-188">Under 'Essentials' post hello **program-ID** och hello **objekt-ID**</span><span class="sxs-lookup"><span data-stu-id="8b9ce-188">Under 'Essentials' record hello **Application ID** and hello **Object ID**</span></span>
4. <span data-ttu-id="8b9ce-189">Inkludera dem i din AAD-gemensamma tekniska Profilmetadata som på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8b9ce-189">Include them in your AAD-Common Technical profile metadata like as follows:</span></span>

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is hello "Object ID" from hello "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is hello "Application ID" from hello "b2c-extensions-app"-->
              </Metadata>
```

<span data-ttu-id="8b9ce-190">tookeep konsekvens med hello-portaler skapa dessa attribut med hello portalens användargränssnitt *innan* du använder dem i din anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-190">tookeep consistency with hello portal experience, create these attributes using hello portal UI *before* you use them in your custom policies.</span></span>  <span data-ttu-id="8b9ce-191">När du skapar ett attribut ”ActivationStatus” hello-portalen måste du referera tooit på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8b9ce-191">When you create an attribute "ActivationStatus" in hello portal, you must refer tooit as follows:</span></span>

```
extension_ActivationStatus in hello custom policy
extension_<app-guid>_ActivationStatus via hello Graph API.
```


## <a name="reference"></a><span data-ttu-id="8b9ce-192">Referens</span><span class="sxs-lookup"><span data-stu-id="8b9ce-192">Reference</span></span>

* <span data-ttu-id="8b9ce-193">En **tekniska profil (TP)** är en elementtyp som kan betraktas som en *funktionen* som definierar en slutpunkt namn, dess metadata, dess protokoll och information hello utbyten av anspråk som hello identitet Upplevelse Framework ska utföra.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-193">A **Technical Profile (TP)** is an element type that can be thought of as a *function* that defines an endpoint’s name, its metadata, its protocol, and details hello exchange of claims that hello Identity Experience Framework should perform.</span></span>  <span data-ttu-id="8b9ce-194">När detta *funktionen* anropas i en orchestration-steg eller från en annan TechnicalProfile, hello InputClaims och OutputClaims tillhandahålls som parametrar av hello anroparen.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-194">When this *function* is called in an orchestration step or from another TechnicalProfile, hello InputClaims and OutputClaims are provided as parameters by hello caller.</span></span>


* <span data-ttu-id="8b9ce-195">Fullständig behandling på tilläggsegenskaper finns hello artikel [DIRECTORY-SCHEMAUTÖKNINGAR | GRAPH API-BEGREPP](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span><span class="sxs-lookup"><span data-stu-id="8b9ce-195">For full treatment on extension properties, see hello article [DIRECTORY SCHEMA EXTENSIONS | GRAPH API CONCEPTS](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span></span>

>[!NOTE]
><span data-ttu-id="8b9ce-196">Tilläggsattribut i Graph API är namngivna med hello konventionen `extension_ApplicationObjectID_attributename`.</span><span class="sxs-lookup"><span data-stu-id="8b9ce-196">Extension attributes in Graph API are named using hello convention `extension_ApplicationObjectID_attributename`.</span></span> <span data-ttu-id="8b9ce-197">Anpassade principer refererar tooextensions attribut som extension_attributename, vilket utesluter hello ApplicationObjectId i hello XML</span><span class="sxs-lookup"><span data-stu-id="8b9ce-197">Custom policies refer tooextensions attributes as extension_attributename, thus omitting hello ApplicationObjectId in hello XML</span></span>

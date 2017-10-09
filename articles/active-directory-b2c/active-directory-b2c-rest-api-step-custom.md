---
title: "Azure Active Directory B2C: REST API-anspråk utbyte som ett orchestration-steg | Microsoft Docs"
description: "Ett ämne på Azure Active Directory B2C anpassade principer som integreras med en API"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: 90a495029f48d70232ef3f99de4ea4d351395aa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a><span data-ttu-id="25946-103">Genomgång: Integrera utbyte av REST API-anspråk i din Azure AD B2C användaren resa som ett orchestration-steg</span><span class="sxs-lookup"><span data-stu-id="25946-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>

<span data-ttu-id="25946-104">hello identitet upplevelse Framework (IEF) för Azure Active Directory B2C det (Azure AD B2C) gör det möjligt för hello identitet developer toointegrate interaktion med en RESTful-API i en resa för användaren.</span><span class="sxs-lookup"><span data-stu-id="25946-104">hello Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables hello identity developer toointegrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="25946-105">Hello slutet av den här genomgången, kommer du att kunna toocreate en Azure AD B2C användaren resa som interagerar med RESTful-tjänster.</span><span class="sxs-lookup"><span data-stu-id="25946-105">At hello end of this walkthrough, you will be able toocreate an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="25946-106">hello IEF skickar data i anspråk och tar emot data tillbaka i anspråk.</span><span class="sxs-lookup"><span data-stu-id="25946-106">hello IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="25946-107">hello REST API anspråk exchange:</span><span class="sxs-lookup"><span data-stu-id="25946-107">hello REST API claims exchange:</span></span>

- <span data-ttu-id="25946-108">Kan utformas som en orchestration-steg.</span><span class="sxs-lookup"><span data-stu-id="25946-108">Can be designed as an orchestration step.</span></span>
- <span data-ttu-id="25946-109">Kan utlösa en extern åtgärd.</span><span class="sxs-lookup"><span data-stu-id="25946-109">Can trigger an external action.</span></span> <span data-ttu-id="25946-110">Det kan till exempel logga en händelse i en extern databas.</span><span class="sxs-lookup"><span data-stu-id="25946-110">For instance, it can log an event in an external database.</span></span>
- <span data-ttu-id="25946-111">Kan använda toofetch ett värde och sedan lagra den i hello användardatabas.</span><span class="sxs-lookup"><span data-stu-id="25946-111">Can be used toofetch a value and then store it in hello user database.</span></span>

<span data-ttu-id="25946-112">Du kan använda hello emot anspråk senare toochange hello flödet av körningen.</span><span class="sxs-lookup"><span data-stu-id="25946-112">You can use hello received claims later toochange hello flow of execution.</span></span>

<span data-ttu-id="25946-113">Du kan också utforma hello interaktion som en verifieringsprofil.</span><span class="sxs-lookup"><span data-stu-id="25946-113">You can also design hello interaction as a validation profile.</span></span> <span data-ttu-id="25946-114">Mer information finns i [genomgång: integrera REST API-anspråk utbyte i din Azure AD B2C användaren resa som autentiserades på indata från användaren](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="25946-114">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input](active-directory-b2c-rest-api-validation-custom.md).</span></span>

<span data-ttu-id="25946-115">hello scenario är att när användaren utför en profil-redigering, vi vill:</span><span class="sxs-lookup"><span data-stu-id="25946-115">hello scenario is that when a user performs a profile edit, we want to:</span></span>

1. <span data-ttu-id="25946-116">Leta upp hello användare i ett externt system.</span><span class="sxs-lookup"><span data-stu-id="25946-116">Look up hello user in an external system.</span></span>
2. <span data-ttu-id="25946-117">Hämta hello stad där användaren har registrerats.</span><span class="sxs-lookup"><span data-stu-id="25946-117">Get hello city where that user is registered.</span></span>
3. <span data-ttu-id="25946-118">Returnera attributet toohello programmet som ett anspråk.</span><span class="sxs-lookup"><span data-stu-id="25946-118">Return that attribute toohello application as a claim.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25946-119">Krav</span><span class="sxs-lookup"><span data-stu-id="25946-119">Prerequisites</span></span>

- <span data-ttu-id="25946-120">En Azure AD B2C-klient som konfigurerats toocomplete ett lokalt konto sign-upp/inloggning, enligt beskrivningen i [komma igång](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="25946-120">An Azure AD B2C tenant configured toocomplete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="25946-121">En REST API-slutpunkten toointeract med.</span><span class="sxs-lookup"><span data-stu-id="25946-121">A REST API endpoint toointeract with.</span></span> <span data-ttu-id="25946-122">Den här genomgången använder en enkel Azure funktionen app webhook som ett exempel.</span><span class="sxs-lookup"><span data-stu-id="25946-122">This walkthrough uses a simple Azure function app webhook as an example.</span></span>
- <span data-ttu-id="25946-123">*Rekommenderade*: fullständig hello [REST API-anspråk exchange genomgång som en verifiering steg](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="25946-123">*Recommended*: Complete hello [REST API claims exchange walkthrough as a validation step](active-directory-b2c-rest-api-validation-custom.md).</span></span>

## <a name="step-1-prepare-hello-rest-api-function"></a><span data-ttu-id="25946-124">Steg 1: Förbered hello REST API-funktion</span><span class="sxs-lookup"><span data-stu-id="25946-124">Step 1: Prepare hello REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="25946-125">Installationen av REST API-funktioner är utanför hello omfånget för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="25946-125">Setup of REST API functions is outside hello scope of this article.</span></span> <span data-ttu-id="25946-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) ger en utmärkt toolkit toocreate RESTful-tjänster i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="25946-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit toocreate RESTful services in hello cloud.</span></span>

<span data-ttu-id="25946-127">Vi har konfigurerat en Azure-funktion som tar emot ett anspråk som kallas `email`, och sedan returnerar hello anspråk `city` med hello tilldelas värdet `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="25946-127">We have set up an Azure function that receives a claim called `email`, and then returns hello claim `city` with hello assigned value of `Redmond`.</span></span> <span data-ttu-id="25946-128">hello exempel Azure funktionen finns på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="25946-128">hello sample Azure function is on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

<span data-ttu-id="25946-129">Hej `userMessage` anspråk som hello Azure returnerar är valfri i den här kontexten och hello IEF ignoreras den.</span><span class="sxs-lookup"><span data-stu-id="25946-129">hello `userMessage` claim that hello Azure function returns is optional in this context, and hello IEF will ignore it.</span></span> <span data-ttu-id="25946-130">Du kan eventuellt använda den som ett meddelande skickades toohello program och presenteras toohello användaren senare.</span><span class="sxs-lookup"><span data-stu-id="25946-130">You can potentially use it as a message passed toohello application and presented toohello user later.</span></span>

```csharp
if (requestContentAsJObject.email == null)
{
    return request.CreateResponse(HttpStatusCode.BadRequest);
}

var email = ((string) requestContentAsJObject.email).ToLower();

return request.CreateResponse<ResponseContent>(
    HttpStatusCode.OK,
    new ResponseContent
    {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        userMessage = "User Found",
        city = "Redmond"
    },
    new JsonMediaTypeFormatter(),
    "application/json");
```

<span data-ttu-id="25946-131">En funktionsapp i Azure-gör det enkelt tooget hello funktions-URL, vilket innefattar hello identifierare för hello specifik funktion.</span><span class="sxs-lookup"><span data-stu-id="25946-131">An Azure function app makes it easy tooget hello function URL, which includes hello identifier of hello specific function.</span></span> <span data-ttu-id="25946-132">I det här fallet hello URL: en är: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span><span class="sxs-lookup"><span data-stu-id="25946-132">In this case, hello URL is: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span></span> <span data-ttu-id="25946-133">Du kan använda den för att testa.</span><span class="sxs-lookup"><span data-stu-id="25946-133">You can use it for testing.</span></span>

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a><span data-ttu-id="25946-134">Steg 2: Konfigurera exchange för hello RESTful-API-anspråk som en teknisk profil i filen TrustFrameworExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="25946-134">Step 2: Configure hello RESTful API claims exchange as a technical profile in your TrustFrameworExtensions.xml file</span></span>

<span data-ttu-id="25946-135">En teknisk profil är hello fullständig konfiguration av hello exchange önskad med hello RESTful-tjänst.</span><span class="sxs-lookup"><span data-stu-id="25946-135">A technical profile is hello full configuration of hello exchange desired with hello RESTful service.</span></span> <span data-ttu-id="25946-136">Öppna hello TrustFrameworkExtensions.xml filen och Lägg till följande XML-kodstycke i hello hello `<ClaimsProvider>` element.</span><span class="sxs-lookup"><span data-stu-id="25946-136">Open hello TrustFrameworkExtensions.xml file and add hello following XML snippet inside hello `<ClaimsProvider>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="25946-137">I hello följande XML, RESTful-providern `Version=1.0.0.0` beskrivs som hello-protokollet.</span><span class="sxs-lookup"><span data-stu-id="25946-137">In hello following XML, RESTful provider `Version=1.0.0.0` is described as hello protocol.</span></span> <span data-ttu-id="25946-138">Överväg att som hello-funktion som kommer att interagera med externa hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="25946-138">Consider it as hello function that will interact with hello external service.</span></span> <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

```XML
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-LookUpLoyaltyWebHook">
            <DisplayName>Check LookUpLoyalty Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
            </InputClaims>
            <OutputClaims>
                <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
            </OutputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

<span data-ttu-id="25946-139">Hej `<InputClaims>` elementet definierar hello anspråk som ska skickas från hello IEF toohello REST-tjänst.</span><span class="sxs-lookup"><span data-stu-id="25946-139">hello `<InputClaims>` element defines hello claims that will be sent from hello IEF toohello REST service.</span></span> <span data-ttu-id="25946-140">I det här exemplet hello innehållet i hello anspråk `givenName` skickas toohello REST-tjänst som hello anspråk `email`.</span><span class="sxs-lookup"><span data-stu-id="25946-140">In this example, hello contents of hello claim `givenName` will be sent toohello REST service as hello claim `email`.</span></span>  

<span data-ttu-id="25946-141">Hej `<OutputClaims>` elementet definierar hello anspråk som hello IEF förväntar sig från hello REST-tjänst.</span><span class="sxs-lookup"><span data-stu-id="25946-141">hello `<OutputClaims>` element defines hello claims that hello IEF will expect from hello REST service.</span></span> <span data-ttu-id="25946-142">Oavsett hello antal anspråk som tas emot hello IEF kommer att använda dem som anges här.</span><span class="sxs-lookup"><span data-stu-id="25946-142">Regardless of hello number of claims that are received, hello IEF will use only those identified here.</span></span> <span data-ttu-id="25946-143">I det här exemplet ett anspråk som tas emot som `city` mappade tooan IEF anspråk anropas `city`.</span><span class="sxs-lookup"><span data-stu-id="25946-143">In this example, a claim received as `city` will be mapped tooan IEF claim called `city`.</span></span>

## <a name="step-3-add-hello-new-claim-city-toohello-schema-of-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="25946-144">Steg 3: Lägg till nytt anspråk för hello `city` toohello schemat för TrustFrameworkExtensions.xml-fil</span><span class="sxs-lookup"><span data-stu-id="25946-144">Step 3: Add hello new claim `city` toohello schema of your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="25946-145">hello anspråk `city` är ännu inte definierats var som helst i vår schemat.</span><span class="sxs-lookup"><span data-stu-id="25946-145">hello claim `city` is not yet defined anywhere in our schema.</span></span> <span data-ttu-id="25946-146">Lägg därför till en definition i hello element `<BuildingBlocks>`.</span><span class="sxs-lookup"><span data-stu-id="25946-146">So, add a definition inside hello element `<BuildingBlocks>`.</span></span> <span data-ttu-id="25946-147">Du hittar det här elementet hello början av hello TrustFrameworkExtensions.xml fil.</span><span class="sxs-lookup"><span data-stu-id="25946-147">You can find this element at hello beginning of hello TrustFrameworkExtensions.xml file.</span></span>

```XML
<BuildingBlocks>
    <!--hello claimtype city must be added toohello TrustFrameworkPolicy-->
    <!-- You can add new claims in hello BASE file Section III, or in hello extensions file-->
    <ClaimsSchema>
        <ClaimType Id="city">
            <DisplayName>City</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your city</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-4-include-hello-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a><span data-ttu-id="25946-148">Steg 4: Ta hello REST service anspråk exchange som ett orchestration-steg i din profil Redigera användare resa i TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="25946-148">Step 4: Include hello REST service claims exchange as an orchestration step in your profile edit user journey in TrustFrameworkExtensions.xml</span></span>

<span data-ttu-id="25946-149">Lägga till ett steg toohello profil Redigera användare resa efter hello användaren har autentiserats (orchestration-steg 1 – 4 i hello följande XML) och hello användaren har angett hello uppdateras profilinformation (steg 5).</span><span class="sxs-lookup"><span data-stu-id="25946-149">Add a step toohello profile edit user journey, after hello user has been authenticated (orchestration steps 1-4 in hello following XML) and hello user has provided hello updated profile information (step 5).</span></span>

> [!NOTE]
> <span data-ttu-id="25946-150">Det finns många användningsområden där hello REST API-anrop kan användas som ett orchestration-steg.</span><span class="sxs-lookup"><span data-stu-id="25946-150">There are many use cases where hello REST API call can be used as an orchestration step.</span></span> <span data-ttu-id="25946-151">Som ett orchestration-steg den kan användas som ett externt system update tooan när en användare har slutfört en uppgift som första gången registrering eller som en profil uppdatering tookeep information som synkroniseras.</span><span class="sxs-lookup"><span data-stu-id="25946-151">As an orchestration step, it can be used as an update tooan external system after a user has successfully completed a task like first-time registration, or as a profile update tookeep information synchronized.</span></span> <span data-ttu-id="25946-152">I det här fallet är det använda tooaugment hello informationen toohello programmet när hello profil redigera.</span><span class="sxs-lookup"><span data-stu-id="25946-152">In this case, it's used tooaugment hello information provided toohello application after hello profile edit.</span></span>

<span data-ttu-id="25946-153">Kopiera hello profil Redigera användare resa XML-koden från hello TrustFrameworkBase.xml tooyour TrustFrameworkExtensions.xml fil i hello `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="25946-153">Copy hello profile edit user journey XML code from hello TrustFrameworkBase.xml file tooyour TrustFrameworkExtensions.xml file inside hello `<UserJourneys>` element.</span></span> <span data-ttu-id="25946-154">Gör hello ändras i steg 6.</span><span class="sxs-lookup"><span data-stu-id="25946-154">Then make hello modification under step 6.</span></span>

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> <span data-ttu-id="25946-155">Om din version inte matchar hello ordning se till att du infogar hello kod som hello steg innan hello `ClaimsExchange` typen `SendClaims`.</span><span class="sxs-lookup"><span data-stu-id="25946-155">If hello order does not match your version, make sure that you insert hello code as hello step before hello `ClaimsExchange` type `SendClaims`.</span></span>

<span data-ttu-id="25946-156">hello sista XML för hello användaren resa bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="25946-156">hello final XML for hello user journey should look like this:</span></span>

```XML
<UserJourney Id="ProfileEdit">
    <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
            <ClaimsProviderSelections>
                <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
                <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
                <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>localAccountAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>socialIdpAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="5" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <!-- Add a step 6 toohello user journey before hello JWT token is created-->
        <OrchestrationStep Order="6" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    </OrchestrationSteps>
    <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="step-5-add-hello-claim-city-tooyour-relying-party-policy-file-so-hello-claim-is-sent-tooyour-application"></a><span data-ttu-id="25946-157">Steg 5: Lägg till hello anspråk `city` tooyour förlitande part princip filen så hello anspråk skickas tooyour program</span><span class="sxs-lookup"><span data-stu-id="25946-157">Step 5: Add hello claim `city` tooyour relying party policy file so hello claim is sent tooyour application</span></span>

<span data-ttu-id="25946-158">Redigera filen ProfileEdit.xml förlitande part (RP) och ändra hello `<TechnicalProfile Id="PolicyProfile">` elementet tooadd hello följande: `<OutputClaim ClaimTypeReferenceId="city" />`.</span><span class="sxs-lookup"><span data-stu-id="25946-158">Edit your ProfileEdit.xml relying party (RP) file and modify hello `<TechnicalProfile Id="PolicyProfile">` element tooadd hello following: `<OutputClaim ClaimTypeReferenceId="city" />`.</span></span>

<span data-ttu-id="25946-159">När du har lagt till hello nytt anspråk hello tekniska profil ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="25946-159">After you add hello new claim, hello technical profile looks like this:</span></span>

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="step-6-upload-your-changes-and-test"></a><span data-ttu-id="25946-160">Steg 6: Överföra dina ändringar och testa</span><span class="sxs-lookup"><span data-stu-id="25946-160">Step 6: Upload your changes and test</span></span>

<span data-ttu-id="25946-161">Skriv över hello befintliga versioner av hello princip.</span><span class="sxs-lookup"><span data-stu-id="25946-161">Overwrite hello existing versions of hello policy.</span></span>

1.  <span data-ttu-id="25946-162">(Valfritt:) Spara hello befintlig version (genom att ladda ned) av tilläggsfilen innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="25946-162">(Optional:) Save hello existing version (by downloading) of your extensions file before you proceed.</span></span> <span data-ttu-id="25946-163">tookeep hello inledande komplexitet låg, rekommenderar vi att du inte överföra flera versioner av hello-tillägg-fil.</span><span class="sxs-lookup"><span data-stu-id="25946-163">tookeep hello initial complexity low, we recommend that you do not upload multiple versions of hello extensions file.</span></span>
2.  <span data-ttu-id="25946-164">(Valfritt:) Byt namn på hello ny version av hello princip-ID för hello princip redigera filen genom att ändra `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span><span class="sxs-lookup"><span data-stu-id="25946-164">(Optional:) Rename hello new version of hello policy ID for hello policy edit file by changing   `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span></span>
3.  <span data-ttu-id="25946-165">Överför hello-tillägg-fil.</span><span class="sxs-lookup"><span data-stu-id="25946-165">Upload hello extensions file.</span></span>
4.  <span data-ttu-id="25946-166">Överför hello principfil redigera RP.</span><span class="sxs-lookup"><span data-stu-id="25946-166">Upload hello policy edit RP file.</span></span>
5.  <span data-ttu-id="25946-167">Använd **kör nu** tootest hello princip.</span><span class="sxs-lookup"><span data-stu-id="25946-167">Use **Run Now** tootest hello policy.</span></span> <span data-ttu-id="25946-168">Granska hello token som hello IEF returnerar toohello program.</span><span class="sxs-lookup"><span data-stu-id="25946-168">Review hello token that hello IEF returns toohello application.</span></span>

<span data-ttu-id="25946-169">Om allt är korrekt konfigurerat, hello token tas hello nytt anspråk `city`, med hello värdet `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="25946-169">If everything is set up correctly, hello token will include hello new claim `city`, with hello value `Redmond`.</span></span>

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a><span data-ttu-id="25946-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25946-170">Next steps</span></span>

[<span data-ttu-id="25946-171">Använd en REST-API som en verifiering steg</span><span class="sxs-lookup"><span data-stu-id="25946-171">Use a REST API as a validation step</span></span>](active-directory-b2c-rest-api-validation-custom.md)

[<span data-ttu-id="25946-172">Ändra hello profil redigera toogather ytterligare information från användarna</span><span class="sxs-lookup"><span data-stu-id="25946-172">Modify hello profile edit toogather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

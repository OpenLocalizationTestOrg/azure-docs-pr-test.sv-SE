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
ms.openlocfilehash: dc319c97e64e55861b84cc3943667418077a05d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a><span data-ttu-id="46efb-103">Genomgång: Integrera utbyte av REST API-anspråk i din Azure AD B2C användaren resa som ett orchestration-steg</span><span class="sxs-lookup"><span data-stu-id="46efb-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>

<span data-ttu-id="46efb-104">Den identitet upplevelse Framework (IEF) för Azure Active Directory B2C det (Azure AD B2C) gör det möjligt för identitet utvecklare att integrera en interaktion med en RESTful-API i en resa för användaren.</span><span class="sxs-lookup"><span data-stu-id="46efb-104">The Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables the identity developer to integrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="46efb-105">I slutet av den här genomgången kommer du att kunna skapa en Azure AD B2C användaren resa som interagerar med RESTful-tjänster.</span><span class="sxs-lookup"><span data-stu-id="46efb-105">At the end of this walkthrough, you will be able to create an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="46efb-106">IEF skickar data i anspråk och tar emot data tillbaka i anspråk.</span><span class="sxs-lookup"><span data-stu-id="46efb-106">The IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="46efb-107">REST API anspråk exchange:</span><span class="sxs-lookup"><span data-stu-id="46efb-107">The REST API claims exchange:</span></span>

- <span data-ttu-id="46efb-108">Kan utformas som en orchestration-steg.</span><span class="sxs-lookup"><span data-stu-id="46efb-108">Can be designed as an orchestration step.</span></span>
- <span data-ttu-id="46efb-109">Kan utlösa en extern åtgärd.</span><span class="sxs-lookup"><span data-stu-id="46efb-109">Can trigger an external action.</span></span> <span data-ttu-id="46efb-110">Det kan till exempel logga en händelse i en extern databas.</span><span class="sxs-lookup"><span data-stu-id="46efb-110">For instance, it can log an event in an external database.</span></span>
- <span data-ttu-id="46efb-111">Kan användas för att hämta ett värde och sedan lagra den i databasen.</span><span class="sxs-lookup"><span data-stu-id="46efb-111">Can be used to fetch a value and then store it in the user database.</span></span>

<span data-ttu-id="46efb-112">Du kan använda mottagna anspråk senare ändra flödet av körningen.</span><span class="sxs-lookup"><span data-stu-id="46efb-112">You can use the received claims later to change the flow of execution.</span></span>

<span data-ttu-id="46efb-113">Du kan också utforma interaktionen som en verifieringsprofil.</span><span class="sxs-lookup"><span data-stu-id="46efb-113">You can also design the interaction as a validation profile.</span></span> <span data-ttu-id="46efb-114">Mer information finns i [genomgång: integrera REST API-anspråk utbyte i din Azure AD B2C användaren resa som autentiserades på indata från användaren](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="46efb-114">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input](active-directory-b2c-rest-api-validation-custom.md).</span></span>

<span data-ttu-id="46efb-115">Scenariot är att när användaren utför en profil-redigering, vi vill:</span><span class="sxs-lookup"><span data-stu-id="46efb-115">The scenario is that when a user performs a profile edit, we want to:</span></span>

1. <span data-ttu-id="46efb-116">Leta upp användaren i ett externt system.</span><span class="sxs-lookup"><span data-stu-id="46efb-116">Look up the user in an external system.</span></span>
2. <span data-ttu-id="46efb-117">Hämta den stad där användaren har registrerats.</span><span class="sxs-lookup"><span data-stu-id="46efb-117">Get the city where that user is registered.</span></span>
3. <span data-ttu-id="46efb-118">Returnera attributet till programmet som ett anspråk.</span><span class="sxs-lookup"><span data-stu-id="46efb-118">Return that attribute to the application as a claim.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46efb-119">Krav</span><span class="sxs-lookup"><span data-stu-id="46efb-119">Prerequisites</span></span>

- <span data-ttu-id="46efb-120">En Azure AD B2C-klient som konfigurerats för att slutföra ett lokalt konto sign-upp/inloggning, enligt beskrivningen i [komma igång](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="46efb-120">An Azure AD B2C tenant configured to complete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="46efb-121">En REST API-slutpunkt kan interagera med.</span><span class="sxs-lookup"><span data-stu-id="46efb-121">A REST API endpoint to interact with.</span></span> <span data-ttu-id="46efb-122">Den här genomgången använder en enkel Azure funktionen app webhook som ett exempel.</span><span class="sxs-lookup"><span data-stu-id="46efb-122">This walkthrough uses a simple Azure function app webhook as an example.</span></span>
- <span data-ttu-id="46efb-123">*Rekommenderade*: Slutför den [REST API-anspråk exchange genomgång som en verifiering steg](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="46efb-123">*Recommended*: Complete the [REST API claims exchange walkthrough as a validation step](active-directory-b2c-rest-api-validation-custom.md).</span></span>

## <a name="step-1-prepare-the-rest-api-function"></a><span data-ttu-id="46efb-124">Steg 1: Förbered REST API-funktion</span><span class="sxs-lookup"><span data-stu-id="46efb-124">Step 1: Prepare the REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="46efb-125">Installationen av REST API-funktioner som ligger utanför omfånget för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="46efb-125">Setup of REST API functions is outside the scope of this article.</span></span> <span data-ttu-id="46efb-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) ger en utmärkt toolkit för att skapa RESTful-tjänster i molnet.</span><span class="sxs-lookup"><span data-stu-id="46efb-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit to create RESTful services in the cloud.</span></span>

<span data-ttu-id="46efb-127">Vi har konfigurerat en Azure-funktion som tar emot ett anspråk som kallas `email`, och returnerar sedan anspråket `city` med tilldelade värdet för `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="46efb-127">We have set up an Azure function that receives a claim called `email`, and then returns the claim `city` with the assigned value of `Redmond`.</span></span> <span data-ttu-id="46efb-128">Exemplet Azure funktionen finns på [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="46efb-128">The sample Azure function is on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

<span data-ttu-id="46efb-129">Den `userMessage` anspråk som returnerar funktionen Azure är valfri i den här kontexten och IEF ignoreras den.</span><span class="sxs-lookup"><span data-stu-id="46efb-129">The `userMessage` claim that the Azure function returns is optional in this context, and the IEF will ignore it.</span></span> <span data-ttu-id="46efb-130">Du kan eventuellt använda den som ett meddelande skickas till programmet och visas för användaren senare.</span><span class="sxs-lookup"><span data-stu-id="46efb-130">You can potentially use it as a message passed to the application and presented to the user later.</span></span>

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

<span data-ttu-id="46efb-131">En funktionsapp i Azure-gör det enkelt att hämta funktions-URL som innehåller identifierare för specifik funktion.</span><span class="sxs-lookup"><span data-stu-id="46efb-131">An Azure function app makes it easy to get the function URL, which includes the identifier of the specific function.</span></span> <span data-ttu-id="46efb-132">I det här fallet URL-Adressen är: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span><span class="sxs-lookup"><span data-stu-id="46efb-132">In this case, the URL is: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span></span> <span data-ttu-id="46efb-133">Du kan använda den för att testa.</span><span class="sxs-lookup"><span data-stu-id="46efb-133">You can use it for testing.</span></span>

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a><span data-ttu-id="46efb-134">Steg 2: Konfigurera exchange för RESTful-API-anspråk som en teknisk profil i filen TrustFrameworExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="46efb-134">Step 2: Configure the RESTful API claims exchange as a technical profile in your TrustFrameworExtensions.xml file</span></span>

<span data-ttu-id="46efb-135">En teknisk profil är den fullständiga konfigurationen av exchange önskad med RESTful-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="46efb-135">A technical profile is the full configuration of the exchange desired with the RESTful service.</span></span> <span data-ttu-id="46efb-136">Öppna filen TrustFrameworkExtensions.xml och Lägg till följande XML-kodstycke i den `<ClaimsProvider>` element.</span><span class="sxs-lookup"><span data-stu-id="46efb-136">Open the TrustFrameworkExtensions.xml file and add the following XML snippet inside the `<ClaimsProvider>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="46efb-137">I följande XML RESTful-providern `Version=1.0.0.0` beskrivs som protokoll.</span><span class="sxs-lookup"><span data-stu-id="46efb-137">In the following XML, RESTful provider `Version=1.0.0.0` is described as the protocol.</span></span> <span data-ttu-id="46efb-138">Överväg att som den funktion som ska interagera med externa-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="46efb-138">Consider it as the function that will interact with the external service.</span></span> <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

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

<span data-ttu-id="46efb-139">Den `<InputClaims>` elementet definierar vilka anspråk som skickas från IEF REST-tjänst.</span><span class="sxs-lookup"><span data-stu-id="46efb-139">The `<InputClaims>` element defines the claims that will be sent from the IEF to the REST service.</span></span> <span data-ttu-id="46efb-140">I det här exemplet innehållet i anspråket `givenName` kommer att skickas till REST-tjänst som anspråket `email`.</span><span class="sxs-lookup"><span data-stu-id="46efb-140">In this example, the contents of the claim `givenName` will be sent to the REST service as the claim `email`.</span></span>  

<span data-ttu-id="46efb-141">Den `<OutputClaims>` elementet definierar vilka anspråk som IEF ska förvänta sig från REST-tjänst.</span><span class="sxs-lookup"><span data-stu-id="46efb-141">The `<OutputClaims>` element defines the claims that the IEF will expect from the REST service.</span></span> <span data-ttu-id="46efb-142">Oavsett vilket antal anspråk som tas emot av IEF kommer att använda dem som anges här.</span><span class="sxs-lookup"><span data-stu-id="46efb-142">Regardless of the number of claims that are received, the IEF will use only those identified here.</span></span> <span data-ttu-id="46efb-143">I det här exemplet ett anspråk som tas emot som `city` kommer att mappas till en IEF anspråk som kallas `city`.</span><span class="sxs-lookup"><span data-stu-id="46efb-143">In this example, a claim received as `city` will be mapped to an IEF claim called `city`.</span></span>

## <a name="step-3-add-the-new-claim-city-to-the-schema-of-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="46efb-144">Steg 3: Lägg till nytt anspråk `city` i schemat för TrustFrameworkExtensions.xml-fil</span><span class="sxs-lookup"><span data-stu-id="46efb-144">Step 3: Add the new claim `city` to the schema of your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="46efb-145">Anspråket `city` är ännu inte definierats var som helst i vår schemat.</span><span class="sxs-lookup"><span data-stu-id="46efb-145">The claim `city` is not yet defined anywhere in our schema.</span></span> <span data-ttu-id="46efb-146">Lägg därför till en definition i elementet `<BuildingBlocks>`.</span><span class="sxs-lookup"><span data-stu-id="46efb-146">So, add a definition inside the element `<BuildingBlocks>`.</span></span> <span data-ttu-id="46efb-147">Du hittar det här elementet i början av filen TrustFrameworkExtensions.xml.</span><span class="sxs-lookup"><span data-stu-id="46efb-147">You can find this element at the beginning of the TrustFrameworkExtensions.xml file.</span></span>

```XML
<BuildingBlocks>
    <!--The claimtype city must be added to the TrustFrameworkPolicy-->
    <!-- You can add new claims in the BASE file Section III, or in the extensions file-->
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

## <a name="step-4-include-the-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a><span data-ttu-id="46efb-148">Steg 4: Ta REST service anspråk exchange som en orchestration-steg i din profil Redigera användare resa i TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="46efb-148">Step 4: Include the REST service claims exchange as an orchestration step in your profile edit user journey in TrustFrameworkExtensions.xml</span></span>

<span data-ttu-id="46efb-149">Lägga till ett steg till profilen Redigera användare resan, efter att användaren har autentiserats (orchestration-steg 1 – 4 i följande XML) och användaren har angett informationen uppdaterade profilen (steg 5).</span><span class="sxs-lookup"><span data-stu-id="46efb-149">Add a step to the profile edit user journey, after the user has been authenticated (orchestration steps 1-4 in the following XML) and the user has provided the updated profile information (step 5).</span></span>

> [!NOTE]
> <span data-ttu-id="46efb-150">Det finns många användningsområden där REST API-anrop kan användas som ett orchestration-steg.</span><span class="sxs-lookup"><span data-stu-id="46efb-150">There are many use cases where the REST API call can be used as an orchestration step.</span></span> <span data-ttu-id="46efb-151">Som ett orchestration-steg kan den användas som en uppdatering för ett externt system när en användare har slutfört en uppgift som första gången registrering eller som en uppdatering av spårningsprofil för att synkronisera informationen.</span><span class="sxs-lookup"><span data-stu-id="46efb-151">As an orchestration step, it can be used as an update to an external system after a user has successfully completed a task like first-time registration, or as a profile update to keep information synchronized.</span></span> <span data-ttu-id="46efb-152">I det här fallet används det att utöka informationen till programmet när profilen har redigerat.</span><span class="sxs-lookup"><span data-stu-id="46efb-152">In this case, it's used to augment the information provided to the application after the profile edit.</span></span>

<span data-ttu-id="46efb-153">Kopiera profilen Redigera användare resa XML-koden från filen TrustFrameworkBase.xml i filen TrustFrameworkExtensions.xml inuti den `<UserJourneys>` element.</span><span class="sxs-lookup"><span data-stu-id="46efb-153">Copy the profile edit user journey XML code from the TrustFrameworkBase.xml file to your TrustFrameworkExtensions.xml file inside the `<UserJourneys>` element.</span></span> <span data-ttu-id="46efb-154">Gör ändringar i steg 6.</span><span class="sxs-lookup"><span data-stu-id="46efb-154">Then make the modification under step 6.</span></span>

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> <span data-ttu-id="46efb-155">Om ordern inte överensstämmer med din version, kontrollerar du att du infogar koden som steg innan den `ClaimsExchange` typen `SendClaims`.</span><span class="sxs-lookup"><span data-stu-id="46efb-155">If the order does not match your version, make sure that you insert the code as the step before the `ClaimsExchange` type `SendClaims`.</span></span>

<span data-ttu-id="46efb-156">Sista XML för transporten användare bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="46efb-156">The final XML for the user journey should look like this:</span></span>

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
        <!-- Add a step 6 to the user journey before the JWT token is created-->
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

## <a name="step-5-add-the-claim-city-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a><span data-ttu-id="46efb-157">Steg 5: Lägg till anspråket `city` till den förlitande parten princip filen så att begäran skickas till ditt program</span><span class="sxs-lookup"><span data-stu-id="46efb-157">Step 5: Add the claim `city` to your relying party policy file so the claim is sent to your application</span></span>

<span data-ttu-id="46efb-158">Redigera filen ProfileEdit.xml förlitande part (RP) och ändra den `<TechnicalProfile Id="PolicyProfile">` element att lägga till följande: `<OutputClaim ClaimTypeReferenceId="city" />`.</span><span class="sxs-lookup"><span data-stu-id="46efb-158">Edit your ProfileEdit.xml relying party (RP) file and modify the `<TechnicalProfile Id="PolicyProfile">` element to add the following: `<OutputClaim ClaimTypeReferenceId="city" />`.</span></span>

<span data-ttu-id="46efb-159">När du lägger till nya anspråk, tekniska profilen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="46efb-159">After you add the new claim, the technical profile looks like this:</span></span>

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

## <a name="step-6-upload-your-changes-and-test"></a><span data-ttu-id="46efb-160">Steg 6: Överföra dina ändringar och testa</span><span class="sxs-lookup"><span data-stu-id="46efb-160">Step 6: Upload your changes and test</span></span>

<span data-ttu-id="46efb-161">Skriv över de befintliga versionerna av principen.</span><span class="sxs-lookup"><span data-stu-id="46efb-161">Overwrite the existing versions of the policy.</span></span>

1.  <span data-ttu-id="46efb-162">(Valfritt:) Spara den befintliga versionen (genom att ladda ned) av tilläggsfilen innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="46efb-162">(Optional:) Save the existing version (by downloading) of your extensions file before you proceed.</span></span> <span data-ttu-id="46efb-163">Om du vill behålla inledande komplexitet låg, rekommenderar vi att du inte överföra flera versioner av tilläggsfilen.</span><span class="sxs-lookup"><span data-stu-id="46efb-163">To keep the initial complexity low, we recommend that you do not upload multiple versions of the extensions file.</span></span>
2.  <span data-ttu-id="46efb-164">(Valfritt:) Byt namn på den nya versionen av princip-ID för att redigera filen genom att ändra `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span><span class="sxs-lookup"><span data-stu-id="46efb-164">(Optional:) Rename the new version of the policy ID for the policy edit file by changing   `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span></span>
3.  <span data-ttu-id="46efb-165">Ladda upp tilläggsfilen.</span><span class="sxs-lookup"><span data-stu-id="46efb-165">Upload the extensions file.</span></span>
4.  <span data-ttu-id="46efb-166">Överför principfilen redigera RP.</span><span class="sxs-lookup"><span data-stu-id="46efb-166">Upload the policy edit RP file.</span></span>
5.  <span data-ttu-id="46efb-167">Använd **kör nu** att testa principen.</span><span class="sxs-lookup"><span data-stu-id="46efb-167">Use **Run Now** to test the policy.</span></span> <span data-ttu-id="46efb-168">Granska den token som IEF returnerar till programmet.</span><span class="sxs-lookup"><span data-stu-id="46efb-168">Review the token that the IEF returns to the application.</span></span>

<span data-ttu-id="46efb-169">Om allt är korrekt konfigurerat, token tas nytt anspråk `city`, med värdet `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="46efb-169">If everything is set up correctly, the token will include the new claim `city`, with the value `Redmond`.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="46efb-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46efb-170">Next steps</span></span>

[<span data-ttu-id="46efb-171">Använd en REST-API som en verifiering steg</span><span class="sxs-lookup"><span data-stu-id="46efb-171">Use a REST API as a validation step</span></span>](active-directory-b2c-rest-api-validation-custom.md)

[<span data-ttu-id="46efb-172">Ändra profil Redigera för att samla in ytterligare information från användarna</span><span class="sxs-lookup"><span data-stu-id="46efb-172">Modify the profile edit to gather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

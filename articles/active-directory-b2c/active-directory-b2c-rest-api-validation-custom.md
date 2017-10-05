---
title: "Azure Active Directory B2C: REST API-anspråk utbyte som verifiering | Microsoft Docs"
description: "Ett ämne på Azure Active Directory B2C anpassade principer"
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
ms.openlocfilehash: eb44a0d2234c9ee3801d8b3a1655d877aa2f4fef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a><span data-ttu-id="75203-103">Genomgång: Integrera utbyte av REST API-anspråk i din Azure AD B2C användaren resa som autentiserades på indata från användaren</span><span class="sxs-lookup"><span data-stu-id="75203-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input</span></span>

<span data-ttu-id="75203-104">Den identitet upplevelse Framework (IEF) för Azure Active Directory B2C det (Azure AD B2C) gör det möjligt för identitet utvecklare att integrera en interaktion med en RESTful-API i en resa för användaren.</span><span class="sxs-lookup"><span data-stu-id="75203-104">The Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables the identity developer to integrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="75203-105">I slutet av den här genomgången kommer du att kunna skapa en Azure AD B2C användaren resa som interagerar med RESTful-tjänster.</span><span class="sxs-lookup"><span data-stu-id="75203-105">At the end of this walkthrough, you will be able to create an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="75203-106">IEF skickar data i anspråk och tar emot data tillbaka i anspråk.</span><span class="sxs-lookup"><span data-stu-id="75203-106">The IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="75203-107">Interaktion med API:</span><span class="sxs-lookup"><span data-stu-id="75203-107">The interaction with the API:</span></span>

- <span data-ttu-id="75203-108">Kan utformas som en REST API anspråk exchange eller verifieringsprofil som görs i ett orchestration-steg.</span><span class="sxs-lookup"><span data-stu-id="75203-108">Can be designed as a REST API claims exchange or as a validation profile, which happens inside an orchestration step.</span></span>
- <span data-ttu-id="75203-109">Vanligtvis verifierar indata från användaren.</span><span class="sxs-lookup"><span data-stu-id="75203-109">Typically validates input from the user.</span></span> <span data-ttu-id="75203-110">Om värdet från användaren avvisas kan användaren försöka igen att ange ett giltigt värde med möjlighet att returnera ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="75203-110">If the value from the user is rejected, the user can try again to enter a valid value with the opportunity to return an error message.</span></span>

<span data-ttu-id="75203-111">Du kan också utforma interaktionen som ett orchestration-steg.</span><span class="sxs-lookup"><span data-stu-id="75203-111">You can also design the interaction as an orchestration step.</span></span> <span data-ttu-id="75203-112">Mer information finns i [genomgång: integrera REST API-anspråk utbyte i din Azure AD B2C användaren resa som ett orchestration-steg](active-directory-b2c-rest-api-step-custom.md).</span><span class="sxs-lookup"><span data-stu-id="75203-112">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step](active-directory-b2c-rest-api-step-custom.md).</span></span>

<span data-ttu-id="75203-113">Exempelvis den verifiering profil använder vi profil Redigera användare resa i pack startfil ProfileEdit.xml.</span><span class="sxs-lookup"><span data-stu-id="75203-113">For the validation profile example, we will use the profile edit user journey in the starter pack file ProfileEdit.xml.</span></span>

<span data-ttu-id="75203-114">Vi kan kontrollera att namnet som användaren i profilen Redigera inte är en del av en undantagslista.</span><span class="sxs-lookup"><span data-stu-id="75203-114">We can verify that the name provided by the user in the profile edit is not part of an exclusion list.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75203-115">Krav</span><span class="sxs-lookup"><span data-stu-id="75203-115">Prerequisites</span></span>

- <span data-ttu-id="75203-116">En Azure AD B2C-klient som konfigurerats för att slutföra ett lokalt konto sign-upp/inloggning, enligt beskrivningen i [komma igång](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="75203-116">An Azure AD B2C tenant configured to complete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="75203-117">En REST API-slutpunkt kan interagera med.</span><span class="sxs-lookup"><span data-stu-id="75203-117">A REST API endpoint to interact with.</span></span> <span data-ttu-id="75203-118">Den här genomgången vi har konfigurerat en demo-webbplatsen som heter [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) med en REST API-tjänst.</span><span class="sxs-lookup"><span data-stu-id="75203-118">For this walkthrough, we've set up a demo site called [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) with a REST API service.</span></span>

## <a name="step-1-prepare-the-rest-api-function"></a><span data-ttu-id="75203-119">Steg 1: Förbered REST API-funktion</span><span class="sxs-lookup"><span data-stu-id="75203-119">Step 1: Prepare the REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="75203-120">Installationen av REST API-funktioner som ligger utanför omfånget för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="75203-120">Setup of REST API functions is outside the scope of this article.</span></span> <span data-ttu-id="75203-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) ger en utmärkt toolkit för att skapa RESTful-tjänster i molnet.</span><span class="sxs-lookup"><span data-stu-id="75203-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit to create RESTful services in the cloud.</span></span>

<span data-ttu-id="75203-122">Vi har skapat en Azure-funktion som tar emot ett anspråk som den förväntas som `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="75203-122">We have created an Azure function that receives a claim that it expects as `playerTag`.</span></span> <span data-ttu-id="75203-123">Funktionen verifierar om detta anspråk finns.</span><span class="sxs-lookup"><span data-stu-id="75203-123">The function validates whether this claim exists.</span></span> <span data-ttu-id="75203-124">Du har åtkomst till fullständig Azure Funktionskoden i [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="75203-124">You can access the complete Azure function code in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"The player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

<span data-ttu-id="75203-125">IEF förväntar sig den `userMessage` anspråk som returnerar funktionen Azure.</span><span class="sxs-lookup"><span data-stu-id="75203-125">The IEF expects the `userMessage` claim that the Azure function returns.</span></span> <span data-ttu-id="75203-126">Det här anspråket visas som en sträng till användaren om valideringen misslyckas, till exempel när status 409 konflikt returneras i föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="75203-126">This claim will be presented as a string to the user if the validation fails, such as when a 409 conflict status is returned in the preceding example.</span></span>

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="75203-127">Steg 2: Konfigurera exchange för RESTful-API-anspråk som en teknisk profil i filen TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="75203-127">Step 2: Configure the RESTful API claims exchange as a technical profile in your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="75203-128">En teknisk profil är den fullständiga konfigurationen av exchange önskad med RESTful-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="75203-128">A technical profile is the full configuration of the exchange desired with the RESTful service.</span></span> <span data-ttu-id="75203-129">Öppna filen TrustFrameworkExtensions.xml och Lägg till följande XML-kodstycke i den `<ClaimsProviders>` element.</span><span class="sxs-lookup"><span data-stu-id="75203-129">Open the TrustFrameworkExtensions.xml file and add the following XML snippet inside the `<ClaimsProviders>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="75203-130">I följande XML RESTful-providern `Version=1.0.0.0` beskrivs som protokoll.</span><span class="sxs-lookup"><span data-stu-id="75203-130">In the following XML, RESTful provider `Version=1.0.0.0` is described as the protocol.</span></span> <span data-ttu-id="75203-131">Överväg att som den funktion som ska interagera med externa-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="75203-131">Consider it as the function that will interact with the external service.</span></span> <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

<span data-ttu-id="75203-132">Den `InputClaims` elementet definierar vilka anspråk som skickas från IEF REST-tjänst.</span><span class="sxs-lookup"><span data-stu-id="75203-132">The `InputClaims` element defines the claims that will be sent from the IEF to the REST service.</span></span> <span data-ttu-id="75203-133">I det här exemplet innehållet i anspråket `givenName` kommer att skickas till REST-tjänst som `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="75203-133">In this example, the contents of the claim `givenName` will be sent to the REST service as `playerTag`.</span></span> <span data-ttu-id="75203-134">I detta exempel förväntar på IEF sig inte anspråk tillbaka.</span><span class="sxs-lookup"><span data-stu-id="75203-134">In this example, the IEF does not expect claims back.</span></span> <span data-ttu-id="75203-135">I stället väntar på svar från REST-tjänst och åtgärder baserat på statuskoder som tas emot.</span><span class="sxs-lookup"><span data-stu-id="75203-135">Instead, it waits for a response from the REST service and acts based on the status codes that it receives.</span></span>

## <a name="step-3-include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-to-validate-the-user-input"></a><span data-ttu-id="75203-136">Steg 3: Inkludera RESTful-tjänst anspråk exchange i själva uppgavs tekniska profil där du vill verifiera användardata</span><span class="sxs-lookup"><span data-stu-id="75203-136">Step 3: Include the RESTful service claims exchange in self-asserted technical profile where you want to validate the user input</span></span>

<span data-ttu-id="75203-137">De vanligaste används steget verifiering i interaktion med användaren.</span><span class="sxs-lookup"><span data-stu-id="75203-137">The most common use of the validation step is in the interaction with a user.</span></span> <span data-ttu-id="75203-138">All interaktion där användaren förväntas ge indata är *själva vars tekniska profiler*.</span><span class="sxs-lookup"><span data-stu-id="75203-138">All interactions where the user is expected to provide input are *self-asserted technical profiles*.</span></span> <span data-ttu-id="75203-139">I det här exemplet ska vi lägga till valideringen oberoende Asserted ProfileUpdate tekniska profilen.</span><span class="sxs-lookup"><span data-stu-id="75203-139">For this example, we will add the validation to the Self-Asserted-ProfileUpdate technical profile.</span></span> <span data-ttu-id="75203-140">Det här är tekniska profil som förlitande part (RP) principfilen `Profile Edit` använder.</span><span class="sxs-lookup"><span data-stu-id="75203-140">This is the technical profile that the relying party (RP) policy file `Profile Edit` uses.</span></span>

<span data-ttu-id="75203-141">Lägga till anspråk exchange till själva uppgavs tekniska profilen:</span><span class="sxs-lookup"><span data-stu-id="75203-141">To add the claims exchange to the self-asserted technical profile:</span></span>

1. <span data-ttu-id="75203-142">Öppna filen TrustFrameworkBase.xml och Sök efter `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span><span class="sxs-lookup"><span data-stu-id="75203-142">Open the TrustFrameworkBase.xml file and search for `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span></span>
2. <span data-ttu-id="75203-143">Granska konfigurationen av den här tekniska profilen.</span><span class="sxs-lookup"><span data-stu-id="75203-143">Review the configuration of this technical profile.</span></span> <span data-ttu-id="75203-144">Se hur exchange med användaren har definierats som anspråk som blir ombedd av användaren (inkommande anspråk) och anspråk som förväntas från själva uppgavs providern (utgående anspråk).</span><span class="sxs-lookup"><span data-stu-id="75203-144">Observe how the exchange with the user is defined as claims that will be asked of the user (input claims) and claims that will be expected back from the self-asserted provider (output claims).</span></span>
3. <span data-ttu-id="75203-145">Sök efter `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, och Lägg märke till att den här profilen anropas som orchestration-steg 6 av `<UserJourney Id="ProfileEdit">`.</span><span class="sxs-lookup"><span data-stu-id="75203-145">Search for `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, and notice that this profile is invoked as orchestration step 6 of `<UserJourney Id="ProfileEdit">`.</span></span>

## <a name="step-4-upload-and-test-the-profile-edit-rp-policy-file"></a><span data-ttu-id="75203-146">Steg 4: Överför och testa redigera RP princip profilfilen</span><span class="sxs-lookup"><span data-stu-id="75203-146">Step 4: Upload and test the profile edit RP policy file</span></span>

1. <span data-ttu-id="75203-147">Ladda upp den nya versionen av filen TrustFrameworkExtensions.xml.</span><span class="sxs-lookup"><span data-stu-id="75203-147">Upload the new version of the TrustFrameworkExtensions.xml file.</span></span>
2. <span data-ttu-id="75203-148">Använd **kör nu** att testa profilen redigera RP principfilen.</span><span class="sxs-lookup"><span data-stu-id="75203-148">Use **Run now** to test the profile edit RP policy file.</span></span>
3. <span data-ttu-id="75203-149">Testa verifieringen genom att tillhandahålla en av de befintliga (till exempel mcvinny) i den **Förnamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="75203-149">Test the validation by providing one of the existing names (for example, mcvinny) in the **Given Name** field.</span></span> <span data-ttu-id="75203-150">Om allt är korrekt konfigurerat, kan du bör få ett meddelande som meddelar användaren att taggen player används redan.</span><span class="sxs-lookup"><span data-stu-id="75203-150">If everything is set up correctly, you should receive a message that notifies the user that the player tag is already used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75203-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="75203-151">Next steps</span></span>

[<span data-ttu-id="75203-152">Ändra profil redigera och användaren registrering för att samla in ytterligare information från användarna</span><span class="sxs-lookup"><span data-stu-id="75203-152">Modify the profile edit and user registration to gather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[<span data-ttu-id="75203-153">Genomgång: Integrera utbyte av REST API-anspråk i din Azure AD B2C användaren resa som ett orchestration-steg</span><span class="sxs-lookup"><span data-stu-id="75203-153">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>](active-directory-b2c-rest-api-step-custom.md)

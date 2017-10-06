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
ms.openlocfilehash: cec6c6e110514a8bbe0e0780f36738ff21ae2f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a><span data-ttu-id="18a67-103">Genomgång: Integrera utbyte av REST API-anspråk i din Azure AD B2C användaren resa som autentiserades på indata från användaren</span><span class="sxs-lookup"><span data-stu-id="18a67-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input</span></span>

<span data-ttu-id="18a67-104">hello identitet upplevelse Framework (IEF) för Azure Active Directory B2C det (Azure AD B2C) gör det möjligt för hello identitet developer toointegrate interaktion med en RESTful-API i en resa för användaren.</span><span class="sxs-lookup"><span data-stu-id="18a67-104">hello Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables hello identity developer toointegrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="18a67-105">Hello slutet av den här genomgången, kommer du att kunna toocreate en Azure AD B2C användaren resa som interagerar med RESTful-tjänster.</span><span class="sxs-lookup"><span data-stu-id="18a67-105">At hello end of this walkthrough, you will be able toocreate an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="18a67-106">hello IEF skickar data i anspråk och tar emot data tillbaka i anspråk.</span><span class="sxs-lookup"><span data-stu-id="18a67-106">hello IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="18a67-107">hello interaktion med hello API:</span><span class="sxs-lookup"><span data-stu-id="18a67-107">hello interaction with hello API:</span></span>

- <span data-ttu-id="18a67-108">Kan utformas som en REST API anspråk exchange eller verifieringsprofil som görs i ett orchestration-steg.</span><span class="sxs-lookup"><span data-stu-id="18a67-108">Can be designed as a REST API claims exchange or as a validation profile, which happens inside an orchestration step.</span></span>
- <span data-ttu-id="18a67-109">Vanligtvis verifierar indata från användaren hello.</span><span class="sxs-lookup"><span data-stu-id="18a67-109">Typically validates input from hello user.</span></span> <span data-ttu-id="18a67-110">Om hello värdet från hello användaren avvisas hello användaren kan försöka igen tooenter ett giltigt värde med hello möjlighet tooreturn ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="18a67-110">If hello value from hello user is rejected, hello user can try again tooenter a valid value with hello opportunity tooreturn an error message.</span></span>

<span data-ttu-id="18a67-111">Du kan också utforma hello interaktion som ett orchestration-steg.</span><span class="sxs-lookup"><span data-stu-id="18a67-111">You can also design hello interaction as an orchestration step.</span></span> <span data-ttu-id="18a67-112">Mer information finns i [genomgång: integrera REST API-anspråk utbyte i din Azure AD B2C användaren resa som ett orchestration-steg](active-directory-b2c-rest-api-step-custom.md).</span><span class="sxs-lookup"><span data-stu-id="18a67-112">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step](active-directory-b2c-rest-api-step-custom.md).</span></span>

<span data-ttu-id="18a67-113">Vi ska använda hello profil Redigera användare resa i hello pack startfil ProfileEdit.xml exempelvis hello validering profil.</span><span class="sxs-lookup"><span data-stu-id="18a67-113">For hello validation profile example, we will use hello profile edit user journey in hello starter pack file ProfileEdit.xml.</span></span>

<span data-ttu-id="18a67-114">Vi kan verifiera att hello namn anges av hello användaren i hello profilen Redigera inte är en del av en undantagslista.</span><span class="sxs-lookup"><span data-stu-id="18a67-114">We can verify that hello name provided by hello user in hello profile edit is not part of an exclusion list.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18a67-115">Krav</span><span class="sxs-lookup"><span data-stu-id="18a67-115">Prerequisites</span></span>

- <span data-ttu-id="18a67-116">En Azure AD B2C-klient som konfigurerats toocomplete ett lokalt konto sign-upp/inloggning, enligt beskrivningen i [komma igång](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="18a67-116">An Azure AD B2C tenant configured toocomplete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="18a67-117">En REST API-slutpunkten toointeract med.</span><span class="sxs-lookup"><span data-stu-id="18a67-117">A REST API endpoint toointeract with.</span></span> <span data-ttu-id="18a67-118">Den här genomgången vi har konfigurerat en demo-webbplatsen som heter [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) med en REST API-tjänst.</span><span class="sxs-lookup"><span data-stu-id="18a67-118">For this walkthrough, we've set up a demo site called [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) with a REST API service.</span></span>

## <a name="step-1-prepare-hello-rest-api-function"></a><span data-ttu-id="18a67-119">Steg 1: Förbered hello REST API-funktion</span><span class="sxs-lookup"><span data-stu-id="18a67-119">Step 1: Prepare hello REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="18a67-120">Installationen av REST API-funktioner är utanför hello omfånget för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="18a67-120">Setup of REST API functions is outside hello scope of this article.</span></span> <span data-ttu-id="18a67-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) ger en utmärkt toolkit toocreate RESTful-tjänster i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="18a67-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit toocreate RESTful services in hello cloud.</span></span>

<span data-ttu-id="18a67-122">Vi har skapat en Azure-funktion som tar emot ett anspråk som den förväntas som `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="18a67-122">We have created an Azure function that receives a claim that it expects as `playerTag`.</span></span> <span data-ttu-id="18a67-123">hello funktionen verifierar om detta anspråk finns.</span><span class="sxs-lookup"><span data-stu-id="18a67-123">hello function validates whether this claim exists.</span></span> <span data-ttu-id="18a67-124">Du kan komma åt hello fullständig Azure Funktionskoden i [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="18a67-124">You can access hello complete Azure function code in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

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
      userMessage = $"hello player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

<span data-ttu-id="18a67-125">hello IEF förväntar hello `userMessage` anspråk som hello Azure funktionen returnerar.</span><span class="sxs-lookup"><span data-stu-id="18a67-125">hello IEF expects hello `userMessage` claim that hello Azure function returns.</span></span> <span data-ttu-id="18a67-126">Det här anspråket visas som en sträng toohello användare om hello valideringen misslyckas, till exempel när status 409 konflikt returneras i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="18a67-126">This claim will be presented as a string toohello user if hello validation fails, such as when a 409 conflict status is returned in hello preceding example.</span></span>

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="18a67-127">Steg 2: Konfigurera exchange för hello RESTful-API-anspråk som en teknisk profil i filen TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="18a67-127">Step 2: Configure hello RESTful API claims exchange as a technical profile in your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="18a67-128">En teknisk profil är hello fullständig konfiguration av hello exchange önskad med hello RESTful-tjänst.</span><span class="sxs-lookup"><span data-stu-id="18a67-128">A technical profile is hello full configuration of hello exchange desired with hello RESTful service.</span></span> <span data-ttu-id="18a67-129">Öppna hello TrustFrameworkExtensions.xml filen och Lägg till följande XML-kodstycke i hello hello `<ClaimsProviders>` element.</span><span class="sxs-lookup"><span data-stu-id="18a67-129">Open hello TrustFrameworkExtensions.xml file and add hello following XML snippet inside hello `<ClaimsProviders>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="18a67-130">I hello följande XML, RESTful-providern `Version=1.0.0.0` beskrivs som hello-protokollet.</span><span class="sxs-lookup"><span data-stu-id="18a67-130">In hello following XML, RESTful provider `Version=1.0.0.0` is described as hello protocol.</span></span> <span data-ttu-id="18a67-131">Överväg att som hello-funktion som kommer att interagera med externa hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="18a67-131">Consider it as hello function that will interact with hello external service.</span></span> <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

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

<span data-ttu-id="18a67-132">Hej `InputClaims` elementet definierar hello anspråk som ska skickas från hello IEF toohello REST-tjänst.</span><span class="sxs-lookup"><span data-stu-id="18a67-132">hello `InputClaims` element defines hello claims that will be sent from hello IEF toohello REST service.</span></span> <span data-ttu-id="18a67-133">I det här exemplet hello innehållet i hello anspråk `givenName` skickas toohello REST-tjänst som `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="18a67-133">In this example, hello contents of hello claim `givenName` will be sent toohello REST service as `playerTag`.</span></span> <span data-ttu-id="18a67-134">I det här exemplet anspråk hello IEF inte förväntar sig tillbaka.</span><span class="sxs-lookup"><span data-stu-id="18a67-134">In this example, hello IEF does not expect claims back.</span></span> <span data-ttu-id="18a67-135">I stället väntar på svar från hello REST-tjänst och åtgärder baserat på hello statuskoder som tas emot.</span><span class="sxs-lookup"><span data-stu-id="18a67-135">Instead, it waits for a response from hello REST service and acts based on hello status codes that it receives.</span></span>

## <a name="step-3-include-hello-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-toovalidate-hello-user-input"></a><span data-ttu-id="18a67-136">Steg 3: Inkludera hello RESTful-tjänst anspråk exchange i själva uppgavs tekniska profil där du vill att toovalidate hello användarindata</span><span class="sxs-lookup"><span data-stu-id="18a67-136">Step 3: Include hello RESTful service claims exchange in self-asserted technical profile where you want toovalidate hello user input</span></span>

<span data-ttu-id="18a67-137">hello används vanligaste hello validering steg i hello interaktion med användaren.</span><span class="sxs-lookup"><span data-stu-id="18a67-137">hello most common use of hello validation step is in hello interaction with a user.</span></span> <span data-ttu-id="18a67-138">All interaktion där hello användaren är förväntade tooprovide indata är *själva vars tekniska profiler*.</span><span class="sxs-lookup"><span data-stu-id="18a67-138">All interactions where hello user is expected tooprovide input are *self-asserted technical profiles*.</span></span> <span data-ttu-id="18a67-139">I det här exemplet ska vi lägga till hello toohello egen Asserted ProfileUpdate tekniska verifieringsprofil.</span><span class="sxs-lookup"><span data-stu-id="18a67-139">For this example, we will add hello validation toohello Self-Asserted-ProfileUpdate technical profile.</span></span> <span data-ttu-id="18a67-140">Detta är hello tekniska profil som hello förlitande part (RP) principfil `Profile Edit` använder.</span><span class="sxs-lookup"><span data-stu-id="18a67-140">This is hello technical profile that hello relying party (RP) policy file `Profile Edit` uses.</span></span>

<span data-ttu-id="18a67-141">tooadd hello anspråk exchange toohello själva vars tekniska profil:</span><span class="sxs-lookup"><span data-stu-id="18a67-141">tooadd hello claims exchange toohello self-asserted technical profile:</span></span>

1. <span data-ttu-id="18a67-142">Öppna hello TrustFrameworkBase.xml filen och Sök efter `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span><span class="sxs-lookup"><span data-stu-id="18a67-142">Open hello TrustFrameworkBase.xml file and search for `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span></span>
2. <span data-ttu-id="18a67-143">Granska hello konfigurationen av den här tekniska profilen.</span><span class="sxs-lookup"><span data-stu-id="18a67-143">Review hello configuration of this technical profile.</span></span> <span data-ttu-id="18a67-144">Se hur hello exchange med hello användare har definierats som anspråk som blir ombedd av hello användare (inkommande anspråk) och anspråk som förväntas tillbaka från själva uppgavs hello-providern (utgående anspråk).</span><span class="sxs-lookup"><span data-stu-id="18a67-144">Observe how hello exchange with hello user is defined as claims that will be asked of hello user (input claims) and claims that will be expected back from hello self-asserted provider (output claims).</span></span>
3. <span data-ttu-id="18a67-145">Sök efter `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, och Lägg märke till att den här profilen anropas som orchestration-steg 6 av `<UserJourney Id="ProfileEdit">`.</span><span class="sxs-lookup"><span data-stu-id="18a67-145">Search for `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, and notice that this profile is invoked as orchestration step 6 of `<UserJourney Id="ProfileEdit">`.</span></span>

## <a name="step-4-upload-and-test-hello-profile-edit-rp-policy-file"></a><span data-ttu-id="18a67-146">Steg 4: Överför och testa hello redigera RP princip profilfil</span><span class="sxs-lookup"><span data-stu-id="18a67-146">Step 4: Upload and test hello profile edit RP policy file</span></span>

1. <span data-ttu-id="18a67-147">Överför hello ny version av hello TrustFrameworkExtensions.xml filen.</span><span class="sxs-lookup"><span data-stu-id="18a67-147">Upload hello new version of hello TrustFrameworkExtensions.xml file.</span></span>
2. <span data-ttu-id="18a67-148">Använd **kör nu** tootest hello profil redigera RP principfil.</span><span class="sxs-lookup"><span data-stu-id="18a67-148">Use **Run now** tootest hello profile edit RP policy file.</span></span>
3. <span data-ttu-id="18a67-149">Testa hello-validering genom att tillhandahålla en av hello befintliga (till exempel mcvinny) i hello **Förnamn** fältet.</span><span class="sxs-lookup"><span data-stu-id="18a67-149">Test hello validation by providing one of hello existing names (for example, mcvinny) in hello **Given Name** field.</span></span> <span data-ttu-id="18a67-150">Om allt är korrekt konfigurerat, får ett meddelande som meddelar användaren hello hello player-tagg som redan används.</span><span class="sxs-lookup"><span data-stu-id="18a67-150">If everything is set up correctly, you should receive a message that notifies hello user that hello player tag is already used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18a67-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18a67-151">Next steps</span></span>

[<span data-ttu-id="18a67-152">Ändra hello profil redigera och användaren registrering toogather ytterligare information från användarna</span><span class="sxs-lookup"><span data-stu-id="18a67-152">Modify hello profile edit and user registration toogather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[<span data-ttu-id="18a67-153">Genomgång: Integrera utbyte av REST API-anspråk i din Azure AD B2C användaren resa som ett orchestration-steg</span><span class="sxs-lookup"><span data-stu-id="18a67-153">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>](active-directory-b2c-rest-api-step-custom.md)

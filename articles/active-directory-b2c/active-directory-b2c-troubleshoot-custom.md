---
title: Application Insights tootroubleshoot anpassade principer - Azure AD B2C | Microsoft Docs
description: "hur toosetup Application Insights tootrace hello körning av anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: saeda
ms.openlocfilehash: c02d7178512c7f9e022385371c3effd4f8cb7726
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-collecting-logs"></a><span data-ttu-id="41e83-103">Azure Active Directory B2C: Insamling av loggar</span><span class="sxs-lookup"><span data-stu-id="41e83-103">Azure Active Directory B2C: Collecting Logs</span></span>

<span data-ttu-id="41e83-104">Den här artikeln innehåller steg för att samla in loggar från Azure AD B2C så att du kan felsöka problem med din anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="41e83-104">This article provides steps for collecting logs from Azure AD B2C so that you can diagnose problems with your custom policies.</span></span>

>[!NOTE]
><span data-ttu-id="41e83-105">För närvarande hello detaljerad aktivitetsloggar som beskrivs här är utformade **bara** tooaid i utvecklingen av anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="41e83-105">Currently, hello detailed activity logs described here are designed **ONLY** tooaid in development of custom policies.</span></span> <span data-ttu-id="41e83-106">Använd inte utvecklingsläge i produktion.</span><span class="sxs-lookup"><span data-stu-id="41e83-106">Do not use development mode  in production.</span></span>  <span data-ttu-id="41e83-107">Loggar samla in alla anspråk skickas tooand från hello identitetsleverantörer under utveckling.</span><span class="sxs-lookup"><span data-stu-id="41e83-107">Logs collect all claims sent tooand from hello identity providers during development.</span></span>  <span data-ttu-id="41e83-108">Om används i produktionen ansvarar hello developer för personligt identifierbar information (privat identifierbar Information) som samlas in i hello App Insights logg som de äger.</span><span class="sxs-lookup"><span data-stu-id="41e83-108">If used in production, hello developer assumes responsibility for PII (Privately Identifiable Information) collected in hello App Insights log that they own.</span></span>  <span data-ttu-id="41e83-109">Dessa detaljerade loggar samlas endast in när hello princip placeras på **UTVECKLINGSLÄGE**.</span><span class="sxs-lookup"><span data-stu-id="41e83-109">These detailed logs are only collected when hello policy is placed on **DEVELOPMENT MODE**.</span></span>


## <a name="use-application-insights"></a><span data-ttu-id="41e83-110">Använda Application Insights</span><span class="sxs-lookup"><span data-stu-id="41e83-110">Use Application Insights</span></span>

<span data-ttu-id="41e83-111">Azure AD B2C stöder en funktion för att skicka data tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="41e83-111">Azure AD B2C supports a feature for sending data tooApplication Insights.</span></span>  <span data-ttu-id="41e83-112">Application Insights ger ett sätt toodiagnose undantag och visualisera prestandaproblem i programmet.</span><span class="sxs-lookup"><span data-stu-id="41e83-112">Application Insights provides a way toodiagnose exceptions and visualize application performance issues.</span></span>

### <a name="setup-application-insights"></a><span data-ttu-id="41e83-113">Installera Application Insights</span><span class="sxs-lookup"><span data-stu-id="41e83-113">Setup Application Insights</span></span>

1. <span data-ttu-id="41e83-114">Gå toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="41e83-114">Go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="41e83-115">Se till att du är i hello-klient med din Azure-prenumeration (inte din Azure AD B2C-klient).</span><span class="sxs-lookup"><span data-stu-id="41e83-115">Ensure you are in hello tenant with your Azure subscription (not your Azure AD B2C tenant).</span></span>
1. <span data-ttu-id="41e83-116">Klicka på **+ ny** i hello vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="41e83-116">Click **+ New** in hello left-hand navigation menu.</span></span>
1. <span data-ttu-id="41e83-117">Söka efter och välja **Programinsikter**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="41e83-117">Search for and select **Application Insights**, then click **Create**.</span></span>
1. <span data-ttu-id="41e83-118">Slutför hello formuläret och klickar på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="41e83-118">Complete hello form and click **Create**.</span></span> <span data-ttu-id="41e83-119">Välj **allmänna** för hello **programtyp**.</span><span class="sxs-lookup"><span data-stu-id="41e83-119">Select **General** for hello **Application Type**.</span></span>
1. <span data-ttu-id="41e83-120">När hello resursen har skapats, öppnar du hello Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="41e83-120">Once hello resource has been created, open hello Application Insights resource.</span></span>
1. <span data-ttu-id="41e83-121">Hitta **egenskaper** hello i vänster-menyn och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="41e83-121">Find **Properties** in hello left-menu, and click on it.</span></span>
1. <span data-ttu-id="41e83-122">Kopiera hello **Instrumentation nyckeln** och spara den för hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="41e83-122">Copy hello **Instrumentation Key** and save it for hello next section.</span></span>

### <a name="set-up-hello-custom-policy"></a><span data-ttu-id="41e83-123">Ställ in hello anpassad princip</span><span class="sxs-lookup"><span data-stu-id="41e83-123">Set up hello custom policy</span></span>

1. <span data-ttu-id="41e83-124">Öppna hello RP-filen (till exempel SignUpOrSignin.xml).</span><span class="sxs-lookup"><span data-stu-id="41e83-124">Open hello RP file (for example, SignUpOrSignin.xml).</span></span>
1. <span data-ttu-id="41e83-125">Lägg till följande attribut toohello hello `<TrustFrameworkPolicy>` element:</span><span class="sxs-lookup"><span data-stu-id="41e83-125">Add hello following attributes toohello `<TrustFrameworkPolicy>` element:</span></span>

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. <span data-ttu-id="41e83-126">Om den inte redan finns, lägger du till en underordnad nod `<UserJourneyBehaviors>` toohello `<RelyingParty>` nod.</span><span class="sxs-lookup"><span data-stu-id="41e83-126">If it doesn't exist already, add a child node `<UserJourneyBehaviors>` toohello `<RelyingParty>` node.</span></span> <span data-ttu-id="41e83-127">Det måste finnas omedelbart efter hello`<DefaultUserJourney ReferenceId="YourPolicyName" />`</span><span class="sxs-lookup"><span data-stu-id="41e83-127">It must be located immediately after hello `<DefaultUserJourney ReferenceId="YourPolicyName" />`</span></span>
2. <span data-ttu-id="41e83-128">Lägg till följande nod som underordnad till hello hello `<UserJourneyBehaviors>` element.</span><span class="sxs-lookup"><span data-stu-id="41e83-128">Add hello following node as a child of hello `<UserJourneyBehaviors>` element.</span></span> <span data-ttu-id="41e83-129">Se till att tooreplace `{Your Application Insights Key}` med hello **Instrumentation nyckeln** som du fick från Application Insights i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="41e83-129">Make sure tooreplace `{Your Application Insights Key}` with hello **Instrumentation Key** that you obtained from Application Insights in hello previous section.</span></span>

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * <span data-ttu-id="41e83-130">`DeveloperMode="true"`Anger ApplicationInsights tooexpedite hello telemetri via hello process-pipelinen bra för utveckling, men begränsad vid stora volymer.</span><span class="sxs-lookup"><span data-stu-id="41e83-130">`DeveloperMode="true"` tells ApplicationInsights tooexpedite hello telemetry through hello processing pipeline, good for development, but constrained at high volumes.</span></span>
  * <span data-ttu-id="41e83-131">`ClientEnabled="true"`skickar hello ApplicationInsights klientskript för spårning av klientsidan och visa sidfel (behövs inte).</span><span class="sxs-lookup"><span data-stu-id="41e83-131">`ClientEnabled="true"` sends hello ApplicationInsights client-side script for tracking page view and client-side errors (not needed).</span></span>
  * <span data-ttu-id="41e83-132">`ServerEnabled="true"`skickar hello befintliga UserJourneyRecorder JSON som en anpassad händelse tooApplication insikter.</span><span class="sxs-lookup"><span data-stu-id="41e83-132">`ServerEnabled="true"` sends hello existing UserJourneyRecorder JSON as a custom event tooApplication Insights.</span></span>
<span data-ttu-id="41e83-133">Exempel:</span><span class="sxs-lookup"><span data-stu-id="41e83-133">Sample:</span></span>

  ```XML
  <TrustFrameworkPolicy
    ...
    TenantId="fabrikamb2c.onmicrosoft.com"
    PolicyId="SignUpOrSignInWithAAD"
    DeploymentMode="Development"
    UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="YourPolicyName" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
  </TrustFrameworkPolicy>
  ```

3. <span data-ttu-id="41e83-134">Överföra hello princip.</span><span class="sxs-lookup"><span data-stu-id="41e83-134">Upload hello policy.</span></span>

### <a name="see-hello-logs-in-application-insights"></a><span data-ttu-id="41e83-135">Se hello loggar i Application Insights</span><span class="sxs-lookup"><span data-stu-id="41e83-135">See hello logs in Application Insights</span></span>

>[!NOTE]
> <span data-ttu-id="41e83-136">Det finns en kort fördröjning (mindre än fem minuter) innan du kan se nya loggar i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="41e83-136">There is a short delay (less than five minutes) before you can see new logs in Application Insights.</span></span>

1. <span data-ttu-id="41e83-137">Öppna hello Application Insights-resurs som du skapade i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="41e83-137">Open hello Application Insights resource that you created in hello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="41e83-138">I hello **översikt** -menyn klickar du på **Analytics**.</span><span class="sxs-lookup"><span data-stu-id="41e83-138">In hello **Overview** menu, click on **Analytics**.</span></span>
1. <span data-ttu-id="41e83-139">Öppna en ny flik i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="41e83-139">Open a new tab in Application Insights.</span></span>
1. <span data-ttu-id="41e83-140">Här är en lista över frågor som du kan använda toosee hello loggar</span><span class="sxs-lookup"><span data-stu-id="41e83-140">Here is a list of queries you can use toosee hello logs</span></span>

| <span data-ttu-id="41e83-141">Fråga</span><span class="sxs-lookup"><span data-stu-id="41e83-141">Query</span></span> | <span data-ttu-id="41e83-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="41e83-142">Description</span></span> |
|---------------------|--------------------|
<span data-ttu-id="41e83-143">spår</span><span class="sxs-lookup"><span data-stu-id="41e83-143">traces</span></span> | <span data-ttu-id="41e83-144">Visa alla hello-loggar som genereras av Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="41e83-144">See all of hello logs generated by Azure AD B2C</span></span> |
<span data-ttu-id="41e83-145">spår \\</span><span class="sxs-lookup"><span data-stu-id="41e83-145">traces \\</span></span>| <span data-ttu-id="41e83-146">där tidsstämpel > ago(1d)</span><span class="sxs-lookup"><span data-stu-id="41e83-146">where timestamp > ago(1d)</span></span> | <span data-ttu-id="41e83-147">Visa alla hello-loggar som genereras av Azure AD B2C för hello sista dagen</span><span class="sxs-lookup"><span data-stu-id="41e83-147">See all of hello logs generated by Azure AD B2C for hello last day</span></span>

<span data-ttu-id="41e83-148">hello poster kan vara långt.</span><span class="sxs-lookup"><span data-stu-id="41e83-148">hello entries may be long.</span></span>  <span data-ttu-id="41e83-149">Exportera tooCSV för en närmare titt.</span><span class="sxs-lookup"><span data-stu-id="41e83-149">Export tooCSV for a closer look.</span></span>

<span data-ttu-id="41e83-150">Du kan lära dig mer om hello Analytics verktyget [här](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span><span class="sxs-lookup"><span data-stu-id="41e83-150">You can learn more about hello Analytics tool [here](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span></span>

>[!NOTE]
><span data-ttu-id="41e83-151">hello community har utvecklat en användare resa viewer toohelp identitet utvecklare.</span><span class="sxs-lookup"><span data-stu-id="41e83-151">hello community has developed a user journey viewer toohelp identity developers.</span></span>  <span data-ttu-id="41e83-152">Den är inte stöds av Microsoft och göras tillgänglig strikt som-är.</span><span class="sxs-lookup"><span data-stu-id="41e83-152">It is not supported by Microsoft and made available strictly as-is.</span></span>  <span data-ttu-id="41e83-153">Det läser från Application Insights-instans och innehåller en korrekt struktur för hello användaren resa händelser.</span><span class="sxs-lookup"><span data-stu-id="41e83-153">It reads from your Application Insights instance and provides a well-structure view of hello user journey events.</span></span>  <span data-ttu-id="41e83-154">Du skaffar hello källkoden och distribuerar den i din egen lösning.</span><span class="sxs-lookup"><span data-stu-id="41e83-154">You obtain hello source code and deploy it in your own solution.</span></span>

>[!NOTE]
><span data-ttu-id="41e83-155">För närvarande hello detaljerad aktivitetsloggar som beskrivs här är utformade **bara** tooaid i utvecklingen av anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="41e83-155">Currently, hello detailed activity logs described here are designed **ONLY** tooaid in development of custom policies.</span></span> <span data-ttu-id="41e83-156">Använd inte utvecklingsläge i produktion.</span><span class="sxs-lookup"><span data-stu-id="41e83-156">Do not use development mode in production.</span></span>  <span data-ttu-id="41e83-157">Loggar samla in alla anspråk skickas tooand från hello identitetsleverantörer under utveckling.</span><span class="sxs-lookup"><span data-stu-id="41e83-157">Logs collect all claims sent tooand from hello identity providers during development.</span></span>  <span data-ttu-id="41e83-158">Om används i produktionen ansvarar hello developer för personligt identifierbar information (privat identifierbar Information) som samlas in i hello App Insights logg som de äger.</span><span class="sxs-lookup"><span data-stu-id="41e83-158">If used in production, hello developer assumes responsibility for PII (Privately Identifiable Information) collected in hello App Insights log that they own.</span></span>  <span data-ttu-id="41e83-159">Dessa detaljerade loggar samlas endast in när hello princip placeras på **UTVECKLINGSLÄGE**.</span><span class="sxs-lookup"><span data-stu-id="41e83-159">These detailed logs are only collected when hello policy is placed on **DEVELOPMENT MODE**.</span></span>

[<span data-ttu-id="41e83-160">Github-lagringsplatsen för exempel på stöds inte anpassade princip och relaterade verktyg</span><span class="sxs-lookup"><span data-stu-id="41e83-160">Github Repository for Unsupported Custom Policy Samples and Related tools</span></span>](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a><span data-ttu-id="41e83-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="41e83-161">Next Steps</span></span>

<span data-ttu-id="41e83-162">Utforska hello data i Application Insights toohelp du förstår hur hello identitet upplevelse Framework underliggande B2C fungerar toodeliver inträffar i din egen identitet.</span><span class="sxs-lookup"><span data-stu-id="41e83-162">Explore hello data in Application Insights toohelp you understand how hello Identity Experience Framework underlying B2C works toodeliver your own identity experiences.</span></span>

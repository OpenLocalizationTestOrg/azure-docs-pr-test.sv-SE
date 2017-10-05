---
title: "Application Insights för att felsöka principer för anpassad - Azure AD B2C | Microsoft Docs"
description: "hur du konfigurerar Application Insights för att spåra körningen av anpassade principer"
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
ms.openlocfilehash: 8c79df33cd5f04f490e2cc6372f7e8ac1c4d9bbe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-collecting-logs"></a><span data-ttu-id="8a346-103">Azure Active Directory B2C: Insamling av loggar</span><span class="sxs-lookup"><span data-stu-id="8a346-103">Azure Active Directory B2C: Collecting Logs</span></span>

<span data-ttu-id="8a346-104">Den här artikeln innehåller steg för att samla in loggar från Azure AD B2C så att du kan felsöka problem med din anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="8a346-104">This article provides steps for collecting logs from Azure AD B2C so that you can diagnose problems with your custom policies.</span></span>

>[!NOTE]
><span data-ttu-id="8a346-105">För närvarande är utformade för detaljerad aktivitetsloggar som beskrivs här **bara** vid utveckling av anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="8a346-105">Currently, the detailed activity logs described here are designed **ONLY** to aid in development of custom policies.</span></span> <span data-ttu-id="8a346-106">Använd inte utvecklingsläge i produktion.</span><span class="sxs-lookup"><span data-stu-id="8a346-106">Do not use development mode  in production.</span></span>  <span data-ttu-id="8a346-107">Loggar samla in alla anspråk som skickas till och från identitetsleverantörer under utvecklingen.</span><span class="sxs-lookup"><span data-stu-id="8a346-107">Logs collect all claims sent to and from the identity providers during development.</span></span>  <span data-ttu-id="8a346-108">Om används i produktionen ansvarar utvecklaren för personligt identifierbar information (privat identifierbar Information) som samlas in i loggen för App Insights som de äger.</span><span class="sxs-lookup"><span data-stu-id="8a346-108">If used in production, the developer assumes responsibility for PII (Privately Identifiable Information) collected in the App Insights log that they own.</span></span>  <span data-ttu-id="8a346-109">Dessa detaljerade loggar samlas endast in när principen är placerad **UTVECKLINGSLÄGE**.</span><span class="sxs-lookup"><span data-stu-id="8a346-109">These detailed logs are only collected when the policy is placed on **DEVELOPMENT MODE**.</span></span>


## <a name="use-application-insights"></a><span data-ttu-id="8a346-110">Använda Application Insights</span><span class="sxs-lookup"><span data-stu-id="8a346-110">Use Application Insights</span></span>

<span data-ttu-id="8a346-111">Azure AD B2C stöder en funktion för att skicka data till Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8a346-111">Azure AD B2C supports a feature for sending data to Application Insights.</span></span>  <span data-ttu-id="8a346-112">Application Insights är ett sätt att diagnostisera undantag och visualisera prestandaproblem i programmet.</span><span class="sxs-lookup"><span data-stu-id="8a346-112">Application Insights provides a way to diagnose exceptions and visualize application performance issues.</span></span>

### <a name="setup-application-insights"></a><span data-ttu-id="8a346-113">Installera Application Insights</span><span class="sxs-lookup"><span data-stu-id="8a346-113">Setup Application Insights</span></span>

1. <span data-ttu-id="8a346-114">Gå till [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8a346-114">Go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8a346-115">Se till att du är i klienten med din Azure-prenumeration (inte din Azure AD B2C-klient).</span><span class="sxs-lookup"><span data-stu-id="8a346-115">Ensure you are in the tenant with your Azure subscription (not your Azure AD B2C tenant).</span></span>
1. <span data-ttu-id="8a346-116">Klicka på **+ ny** i den vänstra navigeringsmenyn.</span><span class="sxs-lookup"><span data-stu-id="8a346-116">Click **+ New** in the left-hand navigation menu.</span></span>
1. <span data-ttu-id="8a346-117">Söka efter och välja **Programinsikter**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="8a346-117">Search for and select **Application Insights**, then click **Create**.</span></span>
1. <span data-ttu-id="8a346-118">Fyll i formuläret och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="8a346-118">Complete the form and click **Create**.</span></span> <span data-ttu-id="8a346-119">Välj **allmänna** för den **programtyp**.</span><span class="sxs-lookup"><span data-stu-id="8a346-119">Select **General** for the **Application Type**.</span></span>
1. <span data-ttu-id="8a346-120">När resursen har skapats kan du öppna Application Insights-resursen.</span><span class="sxs-lookup"><span data-stu-id="8a346-120">Once the resource has been created, open the Application Insights resource.</span></span>
1. <span data-ttu-id="8a346-121">Hitta **egenskaper** i vänster-menyn och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="8a346-121">Find **Properties** in the left-menu, and click on it.</span></span>
1. <span data-ttu-id="8a346-122">Kopiera den **Instrumentation nyckeln** och spara den för nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8a346-122">Copy the **Instrumentation Key** and save it for the next section.</span></span>

### <a name="set-up-the-custom-policy"></a><span data-ttu-id="8a346-123">Konfigurera en anpassad princip</span><span class="sxs-lookup"><span data-stu-id="8a346-123">Set up the custom policy</span></span>

1. <span data-ttu-id="8a346-124">Öppna filen RP (till exempel SignUpOrSignin.xml).</span><span class="sxs-lookup"><span data-stu-id="8a346-124">Open the RP file (for example, SignUpOrSignin.xml).</span></span>
1. <span data-ttu-id="8a346-125">Lägg till följande attribut till den `<TrustFrameworkPolicy>` element:</span><span class="sxs-lookup"><span data-stu-id="8a346-125">Add the following attributes to the `<TrustFrameworkPolicy>` element:</span></span>

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. <span data-ttu-id="8a346-126">Om den inte redan finns, lägger du till en underordnad nod `<UserJourneyBehaviors>` till den `<RelyingParty>` nod.</span><span class="sxs-lookup"><span data-stu-id="8a346-126">If it doesn't exist already, add a child node `<UserJourneyBehaviors>` to the `<RelyingParty>` node.</span></span> <span data-ttu-id="8a346-127">Det måste finnas omedelbart efter den`<DefaultUserJourney ReferenceId="YourPolicyName" />`</span><span class="sxs-lookup"><span data-stu-id="8a346-127">It must be located immediately after the `<DefaultUserJourney ReferenceId="YourPolicyName" />`</span></span>
2. <span data-ttu-id="8a346-128">Lägg till följande nod som är underordnad den `<UserJourneyBehaviors>` element.</span><span class="sxs-lookup"><span data-stu-id="8a346-128">Add the following node as a child of the `<UserJourneyBehaviors>` element.</span></span> <span data-ttu-id="8a346-129">Ersätt `{Your Application Insights Key}` med den **Instrumentation nyckeln** som du fick från Application Insights i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8a346-129">Make sure to replace `{Your Application Insights Key}` with the **Instrumentation Key** that you obtained from Application Insights in the previous section.</span></span>

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * <span data-ttu-id="8a346-130">`DeveloperMode="true"`Anger ApplicationInsights att påskynda telemetri via pipeline bearbetning bra för utveckling, men begränsad vid stora volymer.</span><span class="sxs-lookup"><span data-stu-id="8a346-130">`DeveloperMode="true"` tells ApplicationInsights to expedite the telemetry through the processing pipeline, good for development, but constrained at high volumes.</span></span>
  * <span data-ttu-id="8a346-131">`ClientEnabled="true"`skickar klientskript ApplicationInsights för spårning av klientsidan och visa sidfel (behövs inte).</span><span class="sxs-lookup"><span data-stu-id="8a346-131">`ClientEnabled="true"` sends the ApplicationInsights client-side script for tracking page view and client-side errors (not needed).</span></span>
  * <span data-ttu-id="8a346-132">`ServerEnabled="true"`skickar befintliga UserJourneyRecorder JSON som en anpassad händelse till Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8a346-132">`ServerEnabled="true"` sends the existing UserJourneyRecorder JSON as a custom event to Application Insights.</span></span>
<span data-ttu-id="8a346-133">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8a346-133">Sample:</span></span>

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

3. <span data-ttu-id="8a346-134">Överför principen.</span><span class="sxs-lookup"><span data-stu-id="8a346-134">Upload the policy.</span></span>

### <a name="see-the-logs-in-application-insights"></a><span data-ttu-id="8a346-135">Se loggarna i Application Insights</span><span class="sxs-lookup"><span data-stu-id="8a346-135">See the logs in Application Insights</span></span>

>[!NOTE]
> <span data-ttu-id="8a346-136">Det finns en kort fördröjning (mindre än fem minuter) innan du kan se nya loggar i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8a346-136">There is a short delay (less than five minutes) before you can see new logs in Application Insights.</span></span>

1. <span data-ttu-id="8a346-137">Öppna Application Insights-resursen som du skapade i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8a346-137">Open the Application Insights resource that you created in the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="8a346-138">I den **översikt** -menyn klickar du på **Analytics**.</span><span class="sxs-lookup"><span data-stu-id="8a346-138">In the **Overview** menu, click on **Analytics**.</span></span>
1. <span data-ttu-id="8a346-139">Öppna en ny flik i Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8a346-139">Open a new tab in Application Insights.</span></span>
1. <span data-ttu-id="8a346-140">Här är en lista över frågor som du kan använda för att visa loggar</span><span class="sxs-lookup"><span data-stu-id="8a346-140">Here is a list of queries you can use to see the logs</span></span>

| <span data-ttu-id="8a346-141">Fråga</span><span class="sxs-lookup"><span data-stu-id="8a346-141">Query</span></span> | <span data-ttu-id="8a346-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8a346-142">Description</span></span> |
|---------------------|--------------------|
<span data-ttu-id="8a346-143">spår</span><span class="sxs-lookup"><span data-stu-id="8a346-143">traces</span></span> | <span data-ttu-id="8a346-144">Visa alla loggar som genereras av Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="8a346-144">See all of the logs generated by Azure AD B2C</span></span> |
<span data-ttu-id="8a346-145">spår \\</span><span class="sxs-lookup"><span data-stu-id="8a346-145">traces \\</span></span>| <span data-ttu-id="8a346-146">där tidsstämpel > ago(1d)</span><span class="sxs-lookup"><span data-stu-id="8a346-146">where timestamp > ago(1d)</span></span> | <span data-ttu-id="8a346-147">Visa alla loggar som genereras av Azure AD B2C för den sista dagen</span><span class="sxs-lookup"><span data-stu-id="8a346-147">See all of the logs generated by Azure AD B2C for the last day</span></span>

<span data-ttu-id="8a346-148">Posterna kan vara långt.</span><span class="sxs-lookup"><span data-stu-id="8a346-148">The entries may be long.</span></span>  <span data-ttu-id="8a346-149">Exportera till CSV för en närmare titt.</span><span class="sxs-lookup"><span data-stu-id="8a346-149">Export to CSV for a closer look.</span></span>

<span data-ttu-id="8a346-150">Du kan lära dig mer om verktyget Analytics [här](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span><span class="sxs-lookup"><span data-stu-id="8a346-150">You can learn more about the Analytics tool [here](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span></span>

>[!NOTE]
><span data-ttu-id="8a346-151">Gemenskapen har utvecklat ett användaren resa visningsprogram att hjälpa utvecklare att identitet.</span><span class="sxs-lookup"><span data-stu-id="8a346-151">The community has developed a user journey viewer to help identity developers.</span></span>  <span data-ttu-id="8a346-152">Den är inte stöds av Microsoft och göras tillgänglig strikt som-är.</span><span class="sxs-lookup"><span data-stu-id="8a346-152">It is not supported by Microsoft and made available strictly as-is.</span></span>  <span data-ttu-id="8a346-153">Det läser från Application Insights-instans och innehåller en korrekt struktur för användaren resa händelser.</span><span class="sxs-lookup"><span data-stu-id="8a346-153">It reads from your Application Insights instance and provides a well-structure view of the user journey events.</span></span>  <span data-ttu-id="8a346-154">Du hämta källkoden och distribuerar den i din egen lösning.</span><span class="sxs-lookup"><span data-stu-id="8a346-154">You obtain the source code and deploy it in your own solution.</span></span>

>[!NOTE]
><span data-ttu-id="8a346-155">För närvarande är utformade för detaljerad aktivitetsloggar som beskrivs här **bara** vid utveckling av anpassade principer.</span><span class="sxs-lookup"><span data-stu-id="8a346-155">Currently, the detailed activity logs described here are designed **ONLY** to aid in development of custom policies.</span></span> <span data-ttu-id="8a346-156">Använd inte utvecklingsläge i produktion.</span><span class="sxs-lookup"><span data-stu-id="8a346-156">Do not use development mode in production.</span></span>  <span data-ttu-id="8a346-157">Loggar samla in alla anspråk som skickas till och från identitetsleverantörer under utvecklingen.</span><span class="sxs-lookup"><span data-stu-id="8a346-157">Logs collect all claims sent to and from the identity providers during development.</span></span>  <span data-ttu-id="8a346-158">Om används i produktionen ansvarar utvecklaren för personligt identifierbar information (privat identifierbar Information) som samlas in i loggen för App Insights som de äger.</span><span class="sxs-lookup"><span data-stu-id="8a346-158">If used in production, the developer assumes responsibility for PII (Privately Identifiable Information) collected in the App Insights log that they own.</span></span>  <span data-ttu-id="8a346-159">Dessa detaljerade loggar samlas endast in när principen är placerad **UTVECKLINGSLÄGE**.</span><span class="sxs-lookup"><span data-stu-id="8a346-159">These detailed logs are only collected when the policy is placed on **DEVELOPMENT MODE**.</span></span>

[<span data-ttu-id="8a346-160">Github-lagringsplatsen för exempel på stöds inte anpassade princip och relaterade verktyg</span><span class="sxs-lookup"><span data-stu-id="8a346-160">Github Repository for Unsupported Custom Policy Samples and Related tools</span></span>](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a><span data-ttu-id="8a346-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a346-161">Next Steps</span></span>

<span data-ttu-id="8a346-162">Utforska data i Application Insights som hjälper dig att förstå hur identitet upplevelse ramen underliggande B2C fungerar för att leverera din egen identitet upplevelsen.</span><span class="sxs-lookup"><span data-stu-id="8a346-162">Explore the data in Application Insights to help you understand how the Identity Experience Framework underlying B2C works to deliver your own identity experiences.</span></span>

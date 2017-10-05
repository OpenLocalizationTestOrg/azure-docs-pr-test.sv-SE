---
title: "Datamodell för Azure-program insikter telemetri - telemetri kontexten | Microsoft Docs"
description: Application Insights telemetri kontexten datamodellen
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/15/2017
ms.author: sergkanz
ms.openlocfilehash: d6a0cad8bda6ca68aa691867e84f540c5ac9f6f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="telemetry-context-application-insights-data-model"></a><span data-ttu-id="37892-103">Telemetri kontext: Application Insights-datamodell</span><span class="sxs-lookup"><span data-stu-id="37892-103">Telemetry context: Application Insights data model</span></span>

<span data-ttu-id="37892-104">Varje telemetri-objekt kan ha ett starkt typifierad kontexten fält.</span><span class="sxs-lookup"><span data-stu-id="37892-104">Every telemetry item may have a strongly typed context fields.</span></span> <span data-ttu-id="37892-105">Samtliga fält gör det möjligt för ett specifikt scenario för övervakning.</span><span class="sxs-lookup"><span data-stu-id="37892-105">Every field enables a specific monitoring scenario.</span></span> <span data-ttu-id="37892-106">Använda samlingen med anpassade egenskaper för att lagra anpassade eller programspecifika relevant information.</span><span class="sxs-lookup"><span data-stu-id="37892-106">Use the custom properties collection to store custom or application-specific contextual information.</span></span>


##<a name="application-version"></a><span data-ttu-id="37892-107">Programversion</span><span class="sxs-lookup"><span data-stu-id="37892-107">Application version</span></span>

<span data-ttu-id="37892-108">Information i fälten programmet kontext är alltid om programmet som skickar telemetrin.</span><span class="sxs-lookup"><span data-stu-id="37892-108">Information in the application context fields is always about the application that is sending the telemetry.</span></span> <span data-ttu-id="37892-109">Programversion används för att analysera trend ändringar i programmets beteende och dess korrelation till distributioner.</span><span class="sxs-lookup"><span data-stu-id="37892-109">Application version is used to analyze trend changes in the application behavior and its correlation to the deployments.</span></span>

<span data-ttu-id="37892-110">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="37892-110">Max length: 1024</span></span>


##<a name="client-ip-address"></a><span data-ttu-id="37892-111">Klientens IP-adress</span><span class="sxs-lookup"><span data-stu-id="37892-111">Client IP address</span></span>

<span data-ttu-id="37892-112">IP-adressen för klientenheten.</span><span class="sxs-lookup"><span data-stu-id="37892-112">The IP address of the client device.</span></span> <span data-ttu-id="37892-113">IPv4 och IPv6 stöds.</span><span class="sxs-lookup"><span data-stu-id="37892-113">IPv4 and IPv6 are supported.</span></span> <span data-ttu-id="37892-114">När telemetri som skickas från en tjänst, är plats-kontexten om användaren som initierade igen i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="37892-114">When telemetry is sent from a service, the location context is about the user that initiated the operation in the service.</span></span> <span data-ttu-id="37892-115">Application Insights extrahera informationen geografiska plats från klientens IP-Adressen och sedan trunkera den.</span><span class="sxs-lookup"><span data-stu-id="37892-115">Application Insights extract the geo-location information from the client IP and then truncate it.</span></span> <span data-ttu-id="37892-116">Så att klientens IP-Adressen ensamt kan inte användas som slutanvändaren identifierbar information.</span><span class="sxs-lookup"><span data-stu-id="37892-116">So client IP by itself cannot be used as end-user identifiable information.</span></span> 

<span data-ttu-id="37892-117">Maxlängd: 46</span><span class="sxs-lookup"><span data-stu-id="37892-117">Max length: 46</span></span>


##<a name="device-type"></a><span data-ttu-id="37892-118">Typ av enhet</span><span class="sxs-lookup"><span data-stu-id="37892-118">Device type</span></span>

<span data-ttu-id="37892-119">Det här fältet har ursprungligen används för att ange vilken typ av enhet som används av slutanvändaren av programmet.</span><span class="sxs-lookup"><span data-stu-id="37892-119">Originally this field was used to indicate the type of the device the end user of the application is using.</span></span> <span data-ttu-id="37892-120">Idag används huvudsakligen för att skilja JavaScript telemetri med typ av enhet ange webbläsare om du från serversidan telemetri med enheten ”dator”.</span><span class="sxs-lookup"><span data-stu-id="37892-120">Today used primarily to distinguish JavaScript telemetry with the device type 'Browser' from server-side telemetry with the device type 'PC'.</span></span>

<span data-ttu-id="37892-121">Maxlängd: 64</span><span class="sxs-lookup"><span data-stu-id="37892-121">Max length: 64</span></span>


##<a name="operation-id"></a><span data-ttu-id="37892-122">Åtgärds-id</span><span class="sxs-lookup"><span data-stu-id="37892-122">Operation id</span></span>

<span data-ttu-id="37892-123">En unik identifierare för rot-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="37892-123">A unique identifier of the root operation.</span></span> <span data-ttu-id="37892-124">Den här identifieraren kan gruppen telemetri över flera komponenter.</span><span class="sxs-lookup"><span data-stu-id="37892-124">This identifier allows to group telemetry across multiple components.</span></span> <span data-ttu-id="37892-125">Se [telemetri korrelation](application-insights-correlation.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="37892-125">See [telemetry correlation](application-insights-correlation.md) for details.</span></span> <span data-ttu-id="37892-126">Åtgärds-id skapas av en begäran eller vyn sida.</span><span class="sxs-lookup"><span data-stu-id="37892-126">The operation id is created by either a request or a page view.</span></span> <span data-ttu-id="37892-127">All telemetri anger det här fältet till värdet för vyn som innehåller begäran eller sidan.</span><span class="sxs-lookup"><span data-stu-id="37892-127">All other telemetry sets this field to the value for the containing request or page view.</span></span> 

<span data-ttu-id="37892-128">Maxlängd: 128</span><span class="sxs-lookup"><span data-stu-id="37892-128">Max length: 128</span></span>


##<a name="parent-operation-id"></a><span data-ttu-id="37892-129">Överordnad åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="37892-129">Parent operation ID</span></span>

<span data-ttu-id="37892-130">Unik identifierare för telemetri objektets omedelbart överordnade objekt.</span><span class="sxs-lookup"><span data-stu-id="37892-130">The unique identifier of the telemetry item's immediate parent.</span></span> <span data-ttu-id="37892-131">Se [telemetri korrelation](application-insights-correlation.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="37892-131">See [telemetry correlation](application-insights-correlation.md) for details.</span></span>

<span data-ttu-id="37892-132">Maxlängd: 128</span><span class="sxs-lookup"><span data-stu-id="37892-132">Max length: 128</span></span>


##<a name="operation-name"></a><span data-ttu-id="37892-133">Åtgärdsnamn</span><span class="sxs-lookup"><span data-stu-id="37892-133">Operation name</span></span>

<span data-ttu-id="37892-134">Namnet (gruppering) av åtgärden.</span><span class="sxs-lookup"><span data-stu-id="37892-134">The name (group) of the operation.</span></span> <span data-ttu-id="37892-135">Åtgärdsnamnet har skapats med en begäran eller vyn sida.</span><span class="sxs-lookup"><span data-stu-id="37892-135">The operation name is created by either a request or a page view.</span></span> <span data-ttu-id="37892-136">Alla andra objekt i telemetri ange fältet till värdet för vyn som innehåller begäran eller sidan.</span><span class="sxs-lookup"><span data-stu-id="37892-136">All other telemetry items set this field to the value for the containing request or page view.</span></span> <span data-ttu-id="37892-137">Åtgärdsnamnet används för att söka efter alla objekt som telemetri för en grupp av åtgärder (till exempel ' GET-Home/Index').</span><span class="sxs-lookup"><span data-stu-id="37892-137">Operation name is used for finding all the telemetry items for a group of operations (for example 'GET Home/Index').</span></span> <span data-ttu-id="37892-138">Den här kontexten egenskapen används för att besvara frågor som ”vad är vanliga undantag på den här sidan”.</span><span class="sxs-lookup"><span data-stu-id="37892-138">This context property is used to answer questions like "what are the typical exceptions thrown on this page."</span></span>

<span data-ttu-id="37892-139">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="37892-139">Max length: 1024</span></span>


##<a name="synthetic-source-of-the-operation"></a><span data-ttu-id="37892-140">Syntetiska källan för åtgärden</span><span class="sxs-lookup"><span data-stu-id="37892-140">Synthetic source of the operation</span></span>

<span data-ttu-id="37892-141">Namnet på syntetiska källa.</span><span class="sxs-lookup"><span data-stu-id="37892-141">Name of synthetic source.</span></span> <span data-ttu-id="37892-142">Vissa telemetri från programmet kan representera syntetisk trafik.</span><span class="sxs-lookup"><span data-stu-id="37892-142">Some telemetry from the application may represent synthetic traffic.</span></span> <span data-ttu-id="37892-143">Det kan vara web crawler indexering webbplats, tillgänglighetstester för webbplatsen eller spår från diagnostik bibliotek som Application Insights SDK sig själv.</span><span class="sxs-lookup"><span data-stu-id="37892-143">It may be web crawler indexing the web site, site availability tests, or traces from diagnostic libraries like Application Insights SDK itself.</span></span>

<span data-ttu-id="37892-144">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="37892-144">Max length: 1024</span></span>


##<a name="session-id"></a><span data-ttu-id="37892-145">Sessions-id</span><span class="sxs-lookup"><span data-stu-id="37892-145">Session id</span></span>

<span data-ttu-id="37892-146">Sessions-ID - instans av användarinteraktion med appen.</span><span class="sxs-lookup"><span data-stu-id="37892-146">Session ID - the instance of the user's interaction with the app.</span></span> <span data-ttu-id="37892-147">Information i fälten session kontext är alltid om slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="37892-147">Information in the session context fields is always about the end user.</span></span> <span data-ttu-id="37892-148">När telemetri som skickas från en tjänst, är sessionskontexten om användaren som initierade igen i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="37892-148">When telemetry is sent from a service, the session context is about the user that initiated the operation in the service.</span></span>

<span data-ttu-id="37892-149">Maxlängd: 64</span><span class="sxs-lookup"><span data-stu-id="37892-149">Max length: 64</span></span>


##<a name="anonymous-user-id"></a><span data-ttu-id="37892-150">Anonym användar-id</span><span class="sxs-lookup"><span data-stu-id="37892-150">Anonymous user id</span></span>

<span data-ttu-id="37892-151">Id för anonyma användare.</span><span class="sxs-lookup"><span data-stu-id="37892-151">Anonymous user id.</span></span> <span data-ttu-id="37892-152">Representerar slutanvändaren av programmet.</span><span class="sxs-lookup"><span data-stu-id="37892-152">Represents the end user of the application.</span></span> <span data-ttu-id="37892-153">När telemetri som skickas från en tjänst, är användarkontexten om användaren som initierade igen i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="37892-153">When telemetry is sent from a service, the user context is about the user that initiated the operation in the service.</span></span>

<span data-ttu-id="37892-154">[Provtagning](app-insights-sampling.md) är en av metoderna för att minska mängden insamlade telemetri.</span><span class="sxs-lookup"><span data-stu-id="37892-154">[Sampling](app-insights-sampling.md) is one of the techniques to minimize the amount of collected telemetry.</span></span> <span data-ttu-id="37892-155">Sampling algoritmen försöker antingen exempel in eller ut all korrelerad telemetri.</span><span class="sxs-lookup"><span data-stu-id="37892-155">Sampling algorithm attempts to either sample in or out all the correlated telemetry.</span></span> <span data-ttu-id="37892-156">Anonym användar-id används för provtagning poäng generation.</span><span class="sxs-lookup"><span data-stu-id="37892-156">Anonymous user id is used for sampling score generation.</span></span> <span data-ttu-id="37892-157">Så att anonyma användar-id måste vara ett tillräckligt slumpmässiga värde.</span><span class="sxs-lookup"><span data-stu-id="37892-157">So anonymous user id should be a random enough value.</span></span> 

<span data-ttu-id="37892-158">Använder anonym användar-id för att lagra användarnamnet är ett missbruk av fältet.</span><span class="sxs-lookup"><span data-stu-id="37892-158">Using anonymous user id to store user name is a misuse of the field.</span></span> <span data-ttu-id="37892-159">Använd autentiserad användar-id.</span><span class="sxs-lookup"><span data-stu-id="37892-159">Use Authenticated user id.</span></span>

<span data-ttu-id="37892-160">Maxlängd: 128</span><span class="sxs-lookup"><span data-stu-id="37892-160">Max length: 128</span></span>


##<a name="authenticated-user-id"></a><span data-ttu-id="37892-161">Autentiserat användar-id</span><span class="sxs-lookup"><span data-stu-id="37892-161">Authenticated user id</span></span>

<span data-ttu-id="37892-162">Autentiserat användar-id.</span><span class="sxs-lookup"><span data-stu-id="37892-162">Authenticated user id.</span></span> <span data-ttu-id="37892-163">Motsatsen till anonyma användar-id i det här fältet motsvarar användaren med ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="37892-163">The opposite of anonymous user id, this field represents the user with a friendly name.</span></span> <span data-ttu-id="37892-164">Eftersom dess PII-information den har inte samlats in som standard av de flesta SDK.</span><span class="sxs-lookup"><span data-stu-id="37892-164">Since its PII information it is not collected by default by most SDK.</span></span>

<span data-ttu-id="37892-165">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="37892-165">Max length: 1024</span></span>


##<a name="account-id"></a><span data-ttu-id="37892-166">Konto-id</span><span class="sxs-lookup"><span data-stu-id="37892-166">Account id</span></span>

<span data-ttu-id="37892-167">I program med flera klienter är konto-ID eller namnet som användaren fungerar med.</span><span class="sxs-lookup"><span data-stu-id="37892-167">In multi-tenant applications this is the account ID or name, which the user is acting with.</span></span> <span data-ttu-id="37892-168">Exempel kanske prenumerations-ID för Azure-portalen eller blogg bloggar plattform.</span><span class="sxs-lookup"><span data-stu-id="37892-168">Examples may be subscription ID for Azure portal or blog name blogging platform.</span></span>

<span data-ttu-id="37892-169">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="37892-169">Max length: 1024</span></span>


##<a name="cloud-role"></a><span data-ttu-id="37892-170">Molnet roll</span><span class="sxs-lookup"><span data-stu-id="37892-170">Cloud role</span></span>

<span data-ttu-id="37892-171">Namnet på rollen som programmet tillhör.</span><span class="sxs-lookup"><span data-stu-id="37892-171">Name of the role the application is a part of.</span></span> <span data-ttu-id="37892-172">Mappar direkt till rollnamn i azure.</span><span class="sxs-lookup"><span data-stu-id="37892-172">Maps directly to the role name in azure.</span></span> <span data-ttu-id="37892-173">Kan också användas för att skilja micro tjänster som ingår i ett enda program.</span><span class="sxs-lookup"><span data-stu-id="37892-173">Can also be used to distinguish micro services, which are part of a single application.</span></span>

<span data-ttu-id="37892-174">Maxlängd: 256</span><span class="sxs-lookup"><span data-stu-id="37892-174">Max length: 256</span></span>


##<a name="cloud-role-instance"></a><span data-ttu-id="37892-175">Molnet rollinstans</span><span class="sxs-lookup"><span data-stu-id="37892-175">Cloud role instance</span></span>

<span data-ttu-id="37892-176">Namnet på den instans där programmet körs.</span><span class="sxs-lookup"><span data-stu-id="37892-176">Name of the instance where the application is running.</span></span> <span data-ttu-id="37892-177">Datornamnet för lokal instansnamn för Azure.</span><span class="sxs-lookup"><span data-stu-id="37892-177">Computer name for on-premises, instance name for Azure.</span></span>

<span data-ttu-id="37892-178">Maxlängd: 256</span><span class="sxs-lookup"><span data-stu-id="37892-178">Max length: 256</span></span>


##<a name="internal-sdk-version"></a><span data-ttu-id="37892-179">Internt: SDK-version</span><span class="sxs-lookup"><span data-stu-id="37892-179">Internal: SDK version</span></span>

<span data-ttu-id="37892-180">SDK-version.</span><span class="sxs-lookup"><span data-stu-id="37892-180">SDK version.</span></span> <span data-ttu-id="37892-181">Se https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification information.</span><span class="sxs-lookup"><span data-stu-id="37892-181">See https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification for information.</span></span>

<span data-ttu-id="37892-182">Maxlängd: 64</span><span class="sxs-lookup"><span data-stu-id="37892-182">Max length: 64</span></span>


##<a name="internal-node-name"></a><span data-ttu-id="37892-183">Internt: Nodnamnet</span><span class="sxs-lookup"><span data-stu-id="37892-183">Internal: Node name</span></span>

<span data-ttu-id="37892-184">Det här fältet motsvarar den nodnamn som används för fakturering.</span><span class="sxs-lookup"><span data-stu-id="37892-184">This field represents the node name used for billing purposes.</span></span> <span data-ttu-id="37892-185">Används för att åsidosätta standard identifieringen av noder.</span><span class="sxs-lookup"><span data-stu-id="37892-185">Use it to override the standard detection of nodes.</span></span>

<span data-ttu-id="37892-186">Maxlängd: 256</span><span class="sxs-lookup"><span data-stu-id="37892-186">Max length: 256</span></span>


## <a name="next-steps"></a><span data-ttu-id="37892-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37892-187">Next steps</span></span>

- <span data-ttu-id="37892-188">Lär dig hur du [utöka och filtrera telemetri](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="37892-188">Learn how to [extend and filter telemetry](app-insights-api-filtering-sampling.md).</span></span>
- <span data-ttu-id="37892-189">Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.</span><span class="sxs-lookup"><span data-stu-id="37892-189">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="37892-190">Kolla standard kontexten egenskapssamlingen [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span><span class="sxs-lookup"><span data-stu-id="37892-190">Check out standard context properties collection [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span></span>

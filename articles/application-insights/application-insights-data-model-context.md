---
title: aaaAzure Application Insights telemetri datamodellen - telemetri kontexten | Microsoft Docs
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
ms.openlocfilehash: 6cdd6240d1c448f883d104a871ee9d5f6b5af2ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="telemetry-context-application-insights-data-model"></a><span data-ttu-id="64fd3-103">Telemetri kontext: Application Insights-datamodell</span><span class="sxs-lookup"><span data-stu-id="64fd3-103">Telemetry context: Application Insights data model</span></span>

<span data-ttu-id="64fd3-104">Varje telemetri-objekt kan ha ett starkt typifierad kontexten fält.</span><span class="sxs-lookup"><span data-stu-id="64fd3-104">Every telemetry item may have a strongly typed context fields.</span></span> <span data-ttu-id="64fd3-105">Samtliga fält gör det möjligt för ett specifikt scenario för övervakning.</span><span class="sxs-lookup"><span data-stu-id="64fd3-105">Every field enables a specific monitoring scenario.</span></span> <span data-ttu-id="64fd3-106">Använd hello anpassade egenskaper samling toostore anpassade eller programspecifika kontextuella information.</span><span class="sxs-lookup"><span data-stu-id="64fd3-106">Use hello custom properties collection toostore custom or application-specific contextual information.</span></span>


##<a name="application-version"></a><span data-ttu-id="64fd3-107">Programversion</span><span class="sxs-lookup"><span data-stu-id="64fd3-107">Application version</span></span>

<span data-ttu-id="64fd3-108">Hello programmet kontexten fälten är alltid om hello-program som skickar hello telemetri.</span><span class="sxs-lookup"><span data-stu-id="64fd3-108">Information in hello application context fields is always about hello application that is sending hello telemetry.</span></span> <span data-ttu-id="64fd3-109">Programversion är används tooanalyze trend ändringar i hello programmets beteende och dess korrelation toohello distributioner.</span><span class="sxs-lookup"><span data-stu-id="64fd3-109">Application version is used tooanalyze trend changes in hello application behavior and its correlation toohello deployments.</span></span>

<span data-ttu-id="64fd3-110">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="64fd3-110">Max length: 1024</span></span>


##<a name="client-ip-address"></a><span data-ttu-id="64fd3-111">Klientens IP-adress</span><span class="sxs-lookup"><span data-stu-id="64fd3-111">Client IP address</span></span>

<span data-ttu-id="64fd3-112">hello IP-adressen för hello klientenhet.</span><span class="sxs-lookup"><span data-stu-id="64fd3-112">hello IP address of hello client device.</span></span> <span data-ttu-id="64fd3-113">IPv4 och IPv6 stöds.</span><span class="sxs-lookup"><span data-stu-id="64fd3-113">IPv4 and IPv6 are supported.</span></span> <span data-ttu-id="64fd3-114">När telemetri som skickas från en tjänst, är hello plats kontext om hello-användaren som initierade hello-åtgärden i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="64fd3-114">When telemetry is sent from a service, hello location context is about hello user that initiated hello operation in hello service.</span></span> <span data-ttu-id="64fd3-115">Application Insights extrahera hello geografiska plats information från hello klientens IP-Adressen och sedan trunkera den.</span><span class="sxs-lookup"><span data-stu-id="64fd3-115">Application Insights extract hello geo-location information from hello client IP and then truncate it.</span></span> <span data-ttu-id="64fd3-116">Så att klientens IP-Adressen ensamt kan inte användas som slutanvändaren identifierbar information.</span><span class="sxs-lookup"><span data-stu-id="64fd3-116">So client IP by itself cannot be used as end-user identifiable information.</span></span> 

<span data-ttu-id="64fd3-117">Maxlängd: 46</span><span class="sxs-lookup"><span data-stu-id="64fd3-117">Max length: 46</span></span>


##<a name="device-type"></a><span data-ttu-id="64fd3-118">Typ av enhet</span><span class="sxs-lookup"><span data-stu-id="64fd3-118">Device type</span></span>

<span data-ttu-id="64fd3-119">Ursprungligen användes i det här fältet tooindicate hello typ av enhet hello hello slutanvändaren av programmet hello används.</span><span class="sxs-lookup"><span data-stu-id="64fd3-119">Originally this field was used tooindicate hello type of hello device hello end user of hello application is using.</span></span> <span data-ttu-id="64fd3-120">Idag används främst toodistinguish JavaScript telemetri med hello enhetstyp 'Webbläsare' från serversidan telemetri med hello enhetstyp ”dator”.</span><span class="sxs-lookup"><span data-stu-id="64fd3-120">Today used primarily toodistinguish JavaScript telemetry with hello device type 'Browser' from server-side telemetry with hello device type 'PC'.</span></span>

<span data-ttu-id="64fd3-121">Maxlängd: 64</span><span class="sxs-lookup"><span data-stu-id="64fd3-121">Max length: 64</span></span>


##<a name="operation-id"></a><span data-ttu-id="64fd3-122">Åtgärds-id</span><span class="sxs-lookup"><span data-stu-id="64fd3-122">Operation id</span></span>

<span data-ttu-id="64fd3-123">En unik identifierare för hello rot igen.</span><span class="sxs-lookup"><span data-stu-id="64fd3-123">A unique identifier of hello root operation.</span></span> <span data-ttu-id="64fd3-124">Den här identifieraren kan toogroup telemetri över flera komponenter.</span><span class="sxs-lookup"><span data-stu-id="64fd3-124">This identifier allows toogroup telemetry across multiple components.</span></span> <span data-ttu-id="64fd3-125">Se [telemetri korrelation](application-insights-correlation.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="64fd3-125">See [telemetry correlation](application-insights-correlation.md) for details.</span></span> <span data-ttu-id="64fd3-126">hello åtgärds-id skapas av en begäran eller vyn sida.</span><span class="sxs-lookup"><span data-stu-id="64fd3-126">hello operation id is created by either a request or a page view.</span></span> <span data-ttu-id="64fd3-127">All telemetri anger det här fältet toohello värde för hello som innehåller begäran eller sidan.</span><span class="sxs-lookup"><span data-stu-id="64fd3-127">All other telemetry sets this field toohello value for hello containing request or page view.</span></span> 

<span data-ttu-id="64fd3-128">Maxlängd: 128</span><span class="sxs-lookup"><span data-stu-id="64fd3-128">Max length: 128</span></span>


##<a name="parent-operation-id"></a><span data-ttu-id="64fd3-129">Överordnad åtgärds-ID</span><span class="sxs-lookup"><span data-stu-id="64fd3-129">Parent operation ID</span></span>

<span data-ttu-id="64fd3-130">Hej Unik identifierare för hello telemetri objektets omedelbart överordnade objekt.</span><span class="sxs-lookup"><span data-stu-id="64fd3-130">hello unique identifier of hello telemetry item's immediate parent.</span></span> <span data-ttu-id="64fd3-131">Se [telemetri korrelation](application-insights-correlation.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="64fd3-131">See [telemetry correlation](application-insights-correlation.md) for details.</span></span>

<span data-ttu-id="64fd3-132">Maxlängd: 128</span><span class="sxs-lookup"><span data-stu-id="64fd3-132">Max length: 128</span></span>


##<a name="operation-name"></a><span data-ttu-id="64fd3-133">Åtgärdsnamn</span><span class="sxs-lookup"><span data-stu-id="64fd3-133">Operation name</span></span>

<span data-ttu-id="64fd3-134">hello namn (gruppering) av hello-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="64fd3-134">hello name (group) of hello operation.</span></span> <span data-ttu-id="64fd3-135">hello åtgärdsnamn skapas av en begäran eller vyn sida.</span><span class="sxs-lookup"><span data-stu-id="64fd3-135">hello operation name is created by either a request or a page view.</span></span> <span data-ttu-id="64fd3-136">Alla andra objekt i telemetri ange fältet toohello värdet för hello som innehåller begäran eller sidan.</span><span class="sxs-lookup"><span data-stu-id="64fd3-136">All other telemetry items set this field toohello value for hello containing request or page view.</span></span> <span data-ttu-id="64fd3-137">Åtgärdsnamnet används för att söka efter alla hello telemetri objekt för en grupp av åtgärder (till exempel ' GET-Home/Index').</span><span class="sxs-lookup"><span data-stu-id="64fd3-137">Operation name is used for finding all hello telemetry items for a group of operations (for example 'GET Home/Index').</span></span> <span data-ttu-id="64fd3-138">Den här kontexten-egenskapen är används tooanswer frågor som ”vad är hello vanliga undantag på den här sidan”.</span><span class="sxs-lookup"><span data-stu-id="64fd3-138">This context property is used tooanswer questions like "what are hello typical exceptions thrown on this page."</span></span>

<span data-ttu-id="64fd3-139">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="64fd3-139">Max length: 1024</span></span>


##<a name="synthetic-source-of-hello-operation"></a><span data-ttu-id="64fd3-140">Syntetiska källan för hello åtgärden</span><span class="sxs-lookup"><span data-stu-id="64fd3-140">Synthetic source of hello operation</span></span>

<span data-ttu-id="64fd3-141">Namnet på syntetiska källa.</span><span class="sxs-lookup"><span data-stu-id="64fd3-141">Name of synthetic source.</span></span> <span data-ttu-id="64fd3-142">Vissa telemetri från hello program kan representera syntetisk trafik.</span><span class="sxs-lookup"><span data-stu-id="64fd3-142">Some telemetry from hello application may represent synthetic traffic.</span></span> <span data-ttu-id="64fd3-143">Det kan vara web crawler indexering hello-webbplats, tillgänglighetstester för webbplatsen eller spår från diagnostik bibliotek som Application Insights SDK sig själv.</span><span class="sxs-lookup"><span data-stu-id="64fd3-143">It may be web crawler indexing hello web site, site availability tests, or traces from diagnostic libraries like Application Insights SDK itself.</span></span>

<span data-ttu-id="64fd3-144">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="64fd3-144">Max length: 1024</span></span>


##<a name="session-id"></a><span data-ttu-id="64fd3-145">Sessions-id</span><span class="sxs-lookup"><span data-stu-id="64fd3-145">Session id</span></span>

<span data-ttu-id="64fd3-146">Sessions-ID - hello hello användarens interaktion med hello app-instansen.</span><span class="sxs-lookup"><span data-stu-id="64fd3-146">Session ID - hello instance of hello user's interaction with hello app.</span></span> <span data-ttu-id="64fd3-147">Hello session kontexten fälten är alltid om hello slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="64fd3-147">Information in hello session context fields is always about hello end user.</span></span> <span data-ttu-id="64fd3-148">När telemetri som skickas från en tjänst, är hello sessionskontexten om hello-användaren som initierade hello-åtgärden i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="64fd3-148">When telemetry is sent from a service, hello session context is about hello user that initiated hello operation in hello service.</span></span>

<span data-ttu-id="64fd3-149">Maxlängd: 64</span><span class="sxs-lookup"><span data-stu-id="64fd3-149">Max length: 64</span></span>


##<a name="anonymous-user-id"></a><span data-ttu-id="64fd3-150">Anonym användar-id</span><span class="sxs-lookup"><span data-stu-id="64fd3-150">Anonymous user id</span></span>

<span data-ttu-id="64fd3-151">Id för anonyma användare. Representerar hello slutanvändaren av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="64fd3-151">Anonymous user id. Represents hello end user of hello application.</span></span> <span data-ttu-id="64fd3-152">När telemetri som skickas från en tjänst, är hello användarkontext om hello-användaren som initierade hello-åtgärden i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="64fd3-152">When telemetry is sent from a service, hello user context is about hello user that initiated hello operation in hello service.</span></span>

<span data-ttu-id="64fd3-153">[Provtagning](app-insights-sampling.md) är en av hello tekniker toominimize hello mängden insamlade telemetri.</span><span class="sxs-lookup"><span data-stu-id="64fd3-153">[Sampling](app-insights-sampling.md) is one of hello techniques toominimize hello amount of collected telemetry.</span></span> <span data-ttu-id="64fd3-154">Exempel in eller ut alla hello korrelerad provtagning algoritmen försök tooeither telemetri.</span><span class="sxs-lookup"><span data-stu-id="64fd3-154">Sampling algorithm attempts tooeither sample in or out all hello correlated telemetry.</span></span> <span data-ttu-id="64fd3-155">Anonym användar-id används för provtagning poäng generation.</span><span class="sxs-lookup"><span data-stu-id="64fd3-155">Anonymous user id is used for sampling score generation.</span></span> <span data-ttu-id="64fd3-156">Så att anonyma användar-id måste vara ett tillräckligt slumpmässiga värde.</span><span class="sxs-lookup"><span data-stu-id="64fd3-156">So anonymous user id should be a random enough value.</span></span> 

<span data-ttu-id="64fd3-157">Med hjälp av användarnamn för anonym användare id toostore är ett missbruk av hello-fältet.</span><span class="sxs-lookup"><span data-stu-id="64fd3-157">Using anonymous user id toostore user name is a misuse of hello field.</span></span> <span data-ttu-id="64fd3-158">Använd autentiserad användar-id.</span><span class="sxs-lookup"><span data-stu-id="64fd3-158">Use Authenticated user id.</span></span>

<span data-ttu-id="64fd3-159">Maxlängd: 128</span><span class="sxs-lookup"><span data-stu-id="64fd3-159">Max length: 128</span></span>


##<a name="authenticated-user-id"></a><span data-ttu-id="64fd3-160">Autentiserat användar-id</span><span class="sxs-lookup"><span data-stu-id="64fd3-160">Authenticated user id</span></span>

<span data-ttu-id="64fd3-161">Autentiserat användar-id. hello motsatt av anonym användar-id, i det här fältet visar hello användare med ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="64fd3-161">Authenticated user id. hello opposite of anonymous user id, this field represents hello user with a friendly name.</span></span> <span data-ttu-id="64fd3-162">Eftersom dess PII-information den har inte samlats in som standard av de flesta SDK.</span><span class="sxs-lookup"><span data-stu-id="64fd3-162">Since its PII information it is not collected by default by most SDK.</span></span>

<span data-ttu-id="64fd3-163">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="64fd3-163">Max length: 1024</span></span>


##<a name="account-id"></a><span data-ttu-id="64fd3-164">Konto-id</span><span class="sxs-lookup"><span data-stu-id="64fd3-164">Account id</span></span>

<span data-ttu-id="64fd3-165">I program med flera klienter är hello konto-ID eller namn, vilka hello användare fungerar med.</span><span class="sxs-lookup"><span data-stu-id="64fd3-165">In multi-tenant applications this is hello account ID or name, which hello user is acting with.</span></span> <span data-ttu-id="64fd3-166">Exempel kanske prenumerations-ID för Azure-portalen eller blogg bloggar plattform.</span><span class="sxs-lookup"><span data-stu-id="64fd3-166">Examples may be subscription ID for Azure portal or blog name blogging platform.</span></span>

<span data-ttu-id="64fd3-167">Maxlängd: 1024</span><span class="sxs-lookup"><span data-stu-id="64fd3-167">Max length: 1024</span></span>


##<a name="cloud-role"></a><span data-ttu-id="64fd3-168">Molnet roll</span><span class="sxs-lookup"><span data-stu-id="64fd3-168">Cloud role</span></span>

<span data-ttu-id="64fd3-169">Namnet på hello rollen hello programmet är en del av.</span><span class="sxs-lookup"><span data-stu-id="64fd3-169">Name of hello role hello application is a part of.</span></span> <span data-ttu-id="64fd3-170">Mappar direkt toohello rollnamnet i azure.</span><span class="sxs-lookup"><span data-stu-id="64fd3-170">Maps directly toohello role name in azure.</span></span> <span data-ttu-id="64fd3-171">Måste använda toodistinguish micro tjänster som ingår i ett enda program.</span><span class="sxs-lookup"><span data-stu-id="64fd3-171">Can also be used toodistinguish micro services, which are part of a single application.</span></span>

<span data-ttu-id="64fd3-172">Maxlängd: 256</span><span class="sxs-lookup"><span data-stu-id="64fd3-172">Max length: 256</span></span>


##<a name="cloud-role-instance"></a><span data-ttu-id="64fd3-173">Molnet rollinstans</span><span class="sxs-lookup"><span data-stu-id="64fd3-173">Cloud role instance</span></span>

<span data-ttu-id="64fd3-174">Namn på hello-instans där hello programmet körs.</span><span class="sxs-lookup"><span data-stu-id="64fd3-174">Name of hello instance where hello application is running.</span></span> <span data-ttu-id="64fd3-175">Datornamnet för lokal instansnamn för Azure.</span><span class="sxs-lookup"><span data-stu-id="64fd3-175">Computer name for on-premises, instance name for Azure.</span></span>

<span data-ttu-id="64fd3-176">Maxlängd: 256</span><span class="sxs-lookup"><span data-stu-id="64fd3-176">Max length: 256</span></span>


##<a name="internal-sdk-version"></a><span data-ttu-id="64fd3-177">Internt: SDK-version</span><span class="sxs-lookup"><span data-stu-id="64fd3-177">Internal: SDK version</span></span>

<span data-ttu-id="64fd3-178">SDK-version.</span><span class="sxs-lookup"><span data-stu-id="64fd3-178">SDK version.</span></span> <span data-ttu-id="64fd3-179">Se https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification information.</span><span class="sxs-lookup"><span data-stu-id="64fd3-179">See https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification for information.</span></span>

<span data-ttu-id="64fd3-180">Maxlängd: 64</span><span class="sxs-lookup"><span data-stu-id="64fd3-180">Max length: 64</span></span>


##<a name="internal-node-name"></a><span data-ttu-id="64fd3-181">Internt: Nodnamnet</span><span class="sxs-lookup"><span data-stu-id="64fd3-181">Internal: Node name</span></span>

<span data-ttu-id="64fd3-182">Det här fältet motsvarar hello nodnamnet används för fakturering.</span><span class="sxs-lookup"><span data-stu-id="64fd3-182">This field represents hello node name used for billing purposes.</span></span> <span data-ttu-id="64fd3-183">Använd den toooverride hello standard identifiering av noder.</span><span class="sxs-lookup"><span data-stu-id="64fd3-183">Use it toooverride hello standard detection of nodes.</span></span>

<span data-ttu-id="64fd3-184">Maxlängd: 256</span><span class="sxs-lookup"><span data-stu-id="64fd3-184">Max length: 256</span></span>


## <a name="next-steps"></a><span data-ttu-id="64fd3-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="64fd3-185">Next steps</span></span>

- <span data-ttu-id="64fd3-186">Lär dig hur för[utöka och filtrera telemetri](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="64fd3-186">Learn how too[extend and filter telemetry](app-insights-api-filtering-sampling.md).</span></span>
- <span data-ttu-id="64fd3-187">Se [datamodellen](application-insights-data-model.md) för Application Insights typer och modell.</span><span class="sxs-lookup"><span data-stu-id="64fd3-187">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="64fd3-188">Kolla standard kontexten egenskapssamlingen [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span><span class="sxs-lookup"><span data-stu-id="64fd3-188">Check out standard context properties collection [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span></span>

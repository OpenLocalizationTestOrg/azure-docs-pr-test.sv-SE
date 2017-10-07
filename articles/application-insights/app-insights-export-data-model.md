---
title: aaaAzure Application Insights-datamodell | Microsoft Docs
description: "Beskriver egenskaper som exporteras från löpande export i JSON och används som filter."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: cabad41c-0518-4669-887f-3087aef865ea
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: bwren
ms.openlocfilehash: 5ff3ce7953b91cc69b5d96c0ea9b6d58a6016e61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-export-data-model"></a><span data-ttu-id="dba47-103">Application Insights Export-datamodell</span><span class="sxs-lookup"><span data-stu-id="dba47-103">Application Insights Export Data Model</span></span>
<span data-ttu-id="dba47-104">Den här tabellen visar hello egenskaper för telemetri som skickas från hello [Programinsikter](app-insights-overview.md) SDK toohello portal.</span><span class="sxs-lookup"><span data-stu-id="dba47-104">This table lists hello properties of telemetry sent from hello [Application Insights](app-insights-overview.md) SDKs toohello portal.</span></span>
<span data-ttu-id="dba47-105">Du ser dessa egenskaper i data från [löpande Export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="dba47-105">You'll see these properties in data output from [Continuous Export](app-insights-export-telemetry.md).</span></span>
<span data-ttu-id="dba47-106">De visas också egenskapsfilter i [mått Explorer](app-insights-metrics-explorer.md) och [diagnostiska Sök](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="dba47-106">They also appear in property filters in [Metric Explorer](app-insights-metrics-explorer.md) and [Diagnostic Search](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="dba47-107">Punkter toonote:</span><span class="sxs-lookup"><span data-stu-id="dba47-107">Points toonote:</span></span>

* <span data-ttu-id="dba47-108">`[0]`i dessa tabeller anger du en punkt i hello sökväg där du har tooinsert ett index. men det är inte alltid 0.</span><span class="sxs-lookup"><span data-stu-id="dba47-108">`[0]` in these tables denotes a point in hello path where you have tooinsert an index; but it isn't always 0.</span></span>
* <span data-ttu-id="dba47-109">Tidsvaraktigheter finns i en mikrosekundnivå så 10000000 tiondels == 1 sekund.</span><span class="sxs-lookup"><span data-stu-id="dba47-109">Time durations are in tenths of a microsecond, so 10000000 == 1 second.</span></span>
* <span data-ttu-id="dba47-110">Datum och tider är UTC och anges i hello ISO-format`yyyy-MM-DDThh:mm:ss.sssZ`</span><span class="sxs-lookup"><span data-stu-id="dba47-110">Dates and times are UTC, and are given in hello ISO format `yyyy-MM-DDThh:mm:ss.sssZ`</span></span>


## <a name="example"></a><span data-ttu-id="dba47-111">Exempel</span><span class="sxs-lookup"><span data-stu-id="dba47-111">Example</span></span>
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent tooclient
        "success": true, // Default == responseCode<400
        // Request id becomes hello operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently hello following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized too0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent tooportal:
        "samplingRate": 100.0,
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  <span data-ttu-id="dba47-112">}</span><span class="sxs-lookup"><span data-stu-id="dba47-112">}</span></span>

## <a name="context"></a><span data-ttu-id="dba47-113">Kontext</span><span class="sxs-lookup"><span data-stu-id="dba47-113">Context</span></span>
<span data-ttu-id="dba47-114">Alla typer av telemetri åtföljs av en kontext-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="dba47-114">All types of telemetry are accompanied by a context section.</span></span> <span data-ttu-id="dba47-115">Alla dessa fält skickas med varje datapunkt.</span><span class="sxs-lookup"><span data-stu-id="dba47-115">Not all of these fields are transmitted with every data point.</span></span>

| <span data-ttu-id="dba47-116">Sökväg</span><span class="sxs-lookup"><span data-stu-id="dba47-116">Path</span></span> | <span data-ttu-id="dba47-117">Typ</span><span class="sxs-lookup"><span data-stu-id="dba47-117">Type</span></span> | <span data-ttu-id="dba47-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dba47-118">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dba47-119">Context.Custom.dimensions [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-119">context.custom.dimensions [0]</span></span> |<span data-ttu-id="dba47-120">objektet]</span><span class="sxs-lookup"><span data-stu-id="dba47-120">object [ ]</span></span> |<span data-ttu-id="dba47-121">Nyckel / värde-strängpar som anges av parametern anpassade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="dba47-121">Key-value string pairs set by custom properties parameter.</span></span> <span data-ttu-id="dba47-122">Nyckeln får innehålla högst 100, värden får innehålla högst 1024.</span><span class="sxs-lookup"><span data-stu-id="dba47-122">Key max length 100, values max length 1024.</span></span> <span data-ttu-id="dba47-123">Fler än 100 unika värden hello-egenskapen kan sökas men kan inte användas för segmentering.</span><span class="sxs-lookup"><span data-stu-id="dba47-123">More than 100 unique values, hello property can be searched but cannot be used for segmentation.</span></span> <span data-ttu-id="dba47-124">Max 200 nycklar per ikey.</span><span class="sxs-lookup"><span data-stu-id="dba47-124">Max 200 keys per ikey.</span></span> |
| <span data-ttu-id="dba47-125">Context.Custom.Metrics [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-125">context.custom.metrics [0]</span></span> |<span data-ttu-id="dba47-126">objektet]</span><span class="sxs-lookup"><span data-stu-id="dba47-126">object [ ]</span></span> |<span data-ttu-id="dba47-127">Nyckel-värdepar anges av parametern anpassade mått och av TrackMetrics.</span><span class="sxs-lookup"><span data-stu-id="dba47-127">Key-value pairs set by custom measurements parameter and by TrackMetrics.</span></span> <span data-ttu-id="dba47-128">Maximal nyckellängd 100, värdena kan vara numeriskt.</span><span class="sxs-lookup"><span data-stu-id="dba47-128">Key max length 100, values may be numeric.</span></span> |
| <span data-ttu-id="dba47-129">context.data.eventTime</span><span class="sxs-lookup"><span data-stu-id="dba47-129">context.data.eventTime</span></span> |<span data-ttu-id="dba47-130">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-130">string</span></span> |<span data-ttu-id="dba47-131">UTC</span><span class="sxs-lookup"><span data-stu-id="dba47-131">UTC</span></span> |
| <span data-ttu-id="dba47-132">context.data.isSynthetic</span><span class="sxs-lookup"><span data-stu-id="dba47-132">context.data.isSynthetic</span></span> |<span data-ttu-id="dba47-133">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="dba47-133">boolean</span></span> |<span data-ttu-id="dba47-134">Begäran visas toocome från en bot eller web test.</span><span class="sxs-lookup"><span data-stu-id="dba47-134">Request appears toocome from a bot or web test.</span></span> |
| <span data-ttu-id="dba47-135">context.data.samplingRate</span><span class="sxs-lookup"><span data-stu-id="dba47-135">context.data.samplingRate</span></span> |<span data-ttu-id="dba47-136">Antal</span><span class="sxs-lookup"><span data-stu-id="dba47-136">number</span></span> |<span data-ttu-id="dba47-137">Procentandelen telemetri som genererats av hello SDK som skickas tooportal.</span><span class="sxs-lookup"><span data-stu-id="dba47-137">Percentage of telemetry generated by hello SDK that is sent tooportal.</span></span> <span data-ttu-id="dba47-138">Intervallet 0,0 100,0.</span><span class="sxs-lookup"><span data-stu-id="dba47-138">Range 0.0-100.0.</span></span> |
| <span data-ttu-id="dba47-139">Context.Device</span><span class="sxs-lookup"><span data-stu-id="dba47-139">context.device</span></span> |<span data-ttu-id="dba47-140">Objektet</span><span class="sxs-lookup"><span data-stu-id="dba47-140">object</span></span> |<span data-ttu-id="dba47-141">Klientenheten</span><span class="sxs-lookup"><span data-stu-id="dba47-141">Client device</span></span> |
| <span data-ttu-id="dba47-142">Context.Device.Browser</span><span class="sxs-lookup"><span data-stu-id="dba47-142">context.device.browser</span></span> |<span data-ttu-id="dba47-143">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-143">string</span></span> |<span data-ttu-id="dba47-144">Internet Explorer, Chrome...</span><span class="sxs-lookup"><span data-stu-id="dba47-144">IE, Chrome, ...</span></span> |
| <span data-ttu-id="dba47-145">context.device.browserVersion</span><span class="sxs-lookup"><span data-stu-id="dba47-145">context.device.browserVersion</span></span> |<span data-ttu-id="dba47-146">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-146">string</span></span> |<span data-ttu-id="dba47-147">Chrome 48,0...</span><span class="sxs-lookup"><span data-stu-id="dba47-147">Chrome 48.0, ...</span></span> |
| <span data-ttu-id="dba47-148">context.device.deviceModel</span><span class="sxs-lookup"><span data-stu-id="dba47-148">context.device.deviceModel</span></span> |<span data-ttu-id="dba47-149">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-149">string</span></span> | |
| <span data-ttu-id="dba47-150">context.device.deviceName</span><span class="sxs-lookup"><span data-stu-id="dba47-150">context.device.deviceName</span></span> |<span data-ttu-id="dba47-151">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-151">string</span></span> | |
| <span data-ttu-id="dba47-152">Context.Device.ID</span><span class="sxs-lookup"><span data-stu-id="dba47-152">context.device.id</span></span> |<span data-ttu-id="dba47-153">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-153">string</span></span> | |
| <span data-ttu-id="dba47-154">Context.Device.Locale</span><span class="sxs-lookup"><span data-stu-id="dba47-154">context.device.locale</span></span> |<span data-ttu-id="dba47-155">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-155">string</span></span> |<span data-ttu-id="dba47-156">en GB, de-DE...</span><span class="sxs-lookup"><span data-stu-id="dba47-156">en-GB, de-DE, ...</span></span> |
| <span data-ttu-id="dba47-157">Context.Device.Network</span><span class="sxs-lookup"><span data-stu-id="dba47-157">context.device.network</span></span> |<span data-ttu-id="dba47-158">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-158">string</span></span> | |
| <span data-ttu-id="dba47-159">context.device.oemName</span><span class="sxs-lookup"><span data-stu-id="dba47-159">context.device.oemName</span></span> |<span data-ttu-id="dba47-160">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-160">string</span></span> | |
| <span data-ttu-id="dba47-161">context.device.osVersion</span><span class="sxs-lookup"><span data-stu-id="dba47-161">context.device.osVersion</span></span> |<span data-ttu-id="dba47-162">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-162">string</span></span> |<span data-ttu-id="dba47-163">Värdens operativsystem</span><span class="sxs-lookup"><span data-stu-id="dba47-163">Host OS</span></span> |
| <span data-ttu-id="dba47-164">context.device.roleInstance</span><span class="sxs-lookup"><span data-stu-id="dba47-164">context.device.roleInstance</span></span> |<span data-ttu-id="dba47-165">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-165">string</span></span> |<span data-ttu-id="dba47-166">ID för server-värd</span><span class="sxs-lookup"><span data-stu-id="dba47-166">ID of server host</span></span> |
| <span data-ttu-id="dba47-167">context.device.roleName</span><span class="sxs-lookup"><span data-stu-id="dba47-167">context.device.roleName</span></span> |<span data-ttu-id="dba47-168">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-168">string</span></span> | |
| <span data-ttu-id="dba47-169">Context.Device.Type</span><span class="sxs-lookup"><span data-stu-id="dba47-169">context.device.type</span></span> |<span data-ttu-id="dba47-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-170">string</span></span> |<span data-ttu-id="dba47-171">PC webbläsare...</span><span class="sxs-lookup"><span data-stu-id="dba47-171">PC, Browser, ...</span></span> |
| <span data-ttu-id="dba47-172">Context.Location</span><span class="sxs-lookup"><span data-stu-id="dba47-172">context.location</span></span> |<span data-ttu-id="dba47-173">Objektet</span><span class="sxs-lookup"><span data-stu-id="dba47-173">object</span></span> |<span data-ttu-id="dba47-174">Härleds från clientip.</span><span class="sxs-lookup"><span data-stu-id="dba47-174">Derived from clientip.</span></span> |
| <span data-ttu-id="dba47-175">Context.location.City</span><span class="sxs-lookup"><span data-stu-id="dba47-175">context.location.city</span></span> |<span data-ttu-id="dba47-176">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-176">string</span></span> |<span data-ttu-id="dba47-177">Om den är känd som härrör från clientip,</span><span class="sxs-lookup"><span data-stu-id="dba47-177">Derived from clientip, if known</span></span> |
| <span data-ttu-id="dba47-178">Context.location.clientip</span><span class="sxs-lookup"><span data-stu-id="dba47-178">context.location.clientip</span></span> |<span data-ttu-id="dba47-179">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-179">string</span></span> |<span data-ttu-id="dba47-180">Senaste Åttahörning är anonymiserade too0.</span><span class="sxs-lookup"><span data-stu-id="dba47-180">Last octagon is anonymized too0.</span></span> |
| <span data-ttu-id="dba47-181">Context.location.continent</span><span class="sxs-lookup"><span data-stu-id="dba47-181">context.location.continent</span></span> |<span data-ttu-id="dba47-182">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-182">string</span></span> | |
| <span data-ttu-id="dba47-183">Context.location.Country</span><span class="sxs-lookup"><span data-stu-id="dba47-183">context.location.country</span></span> |<span data-ttu-id="dba47-184">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-184">string</span></span> | |
| <span data-ttu-id="dba47-185">Context.location.province</span><span class="sxs-lookup"><span data-stu-id="dba47-185">context.location.province</span></span> |<span data-ttu-id="dba47-186">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-186">string</span></span> |<span data-ttu-id="dba47-187">Region</span><span class="sxs-lookup"><span data-stu-id="dba47-187">State or province</span></span> |
| <span data-ttu-id="dba47-188">Context.operation.ID</span><span class="sxs-lookup"><span data-stu-id="dba47-188">context.operation.id</span></span> |<span data-ttu-id="dba47-189">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-189">string</span></span> |<span data-ttu-id="dba47-190">Objekt som har samma åtgärds-id visas som relaterade objekt i hello portal hello.</span><span class="sxs-lookup"><span data-stu-id="dba47-190">Items that have hello same operation id are shown as Related Items in hello portal.</span></span> <span data-ttu-id="dba47-191">Vanligtvis hello id för förfrågan.</span><span class="sxs-lookup"><span data-stu-id="dba47-191">Usually hello request id.</span></span> |
| <span data-ttu-id="dba47-192">Context.operation.Name</span><span class="sxs-lookup"><span data-stu-id="dba47-192">context.operation.name</span></span> |<span data-ttu-id="dba47-193">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-193">string</span></span> |<span data-ttu-id="dba47-194">URL eller begäran</span><span class="sxs-lookup"><span data-stu-id="dba47-194">url or request name</span></span> |
| <span data-ttu-id="dba47-195">context.operation.parentId</span><span class="sxs-lookup"><span data-stu-id="dba47-195">context.operation.parentId</span></span> |<span data-ttu-id="dba47-196">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-196">string</span></span> |<span data-ttu-id="dba47-197">Tillåter kapslade relaterade objekt.</span><span class="sxs-lookup"><span data-stu-id="dba47-197">Allows nested related items.</span></span> |
| <span data-ttu-id="dba47-198">Context.session.ID</span><span class="sxs-lookup"><span data-stu-id="dba47-198">context.session.id</span></span> |<span data-ttu-id="dba47-199">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-199">string</span></span> |<span data-ttu-id="dba47-200">ID för en grupp av åtgärder från hello samma källa.</span><span class="sxs-lookup"><span data-stu-id="dba47-200">Id of a group of operations from hello same source.</span></span> <span data-ttu-id="dba47-201">30 minuter utan en åtgärd signalerar hello slutet av en session.</span><span class="sxs-lookup"><span data-stu-id="dba47-201">A period of 30 minutes without an operation signals hello end of a session.</span></span> |
| <span data-ttu-id="dba47-202">context.session.isFirst</span><span class="sxs-lookup"><span data-stu-id="dba47-202">context.session.isFirst</span></span> |<span data-ttu-id="dba47-203">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="dba47-203">boolean</span></span> | |
| <span data-ttu-id="dba47-204">context.user.accountAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="dba47-204">context.user.accountAcquisitionDate</span></span> |<span data-ttu-id="dba47-205">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-205">string</span></span> | |
| <span data-ttu-id="dba47-206">context.user.anonAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="dba47-206">context.user.anonAcquisitionDate</span></span> |<span data-ttu-id="dba47-207">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-207">string</span></span> | |
| <span data-ttu-id="dba47-208">context.user.anonId</span><span class="sxs-lookup"><span data-stu-id="dba47-208">context.user.anonId</span></span> |<span data-ttu-id="dba47-209">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-209">string</span></span> | |
| <span data-ttu-id="dba47-210">context.user.authAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="dba47-210">context.user.authAcquisitionDate</span></span> |<span data-ttu-id="dba47-211">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-211">string</span></span> |[<span data-ttu-id="dba47-212">Autentiserad användare</span><span class="sxs-lookup"><span data-stu-id="dba47-212">Authenticated User</span></span>](app-insights-api-custom-events-metrics.md#authenticated-users) |
| <span data-ttu-id="dba47-213">context.user.isAuthenticated</span><span class="sxs-lookup"><span data-stu-id="dba47-213">context.user.isAuthenticated</span></span> |<span data-ttu-id="dba47-214">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="dba47-214">boolean</span></span> | |
| <span data-ttu-id="dba47-215">internal.data.documentVersion</span><span class="sxs-lookup"><span data-stu-id="dba47-215">internal.data.documentVersion</span></span> |<span data-ttu-id="dba47-216">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-216">string</span></span> | |
| <span data-ttu-id="dba47-217">internal.data.ID</span><span class="sxs-lookup"><span data-stu-id="dba47-217">internal.data.id</span></span> |<span data-ttu-id="dba47-218">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-218">string</span></span> | |

## <a name="events"></a><span data-ttu-id="dba47-219">Händelser</span><span class="sxs-lookup"><span data-stu-id="dba47-219">Events</span></span>
<span data-ttu-id="dba47-220">Anpassade händelser som genererats av [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="dba47-220">Custom events generated by [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).</span></span>

| <span data-ttu-id="dba47-221">Sökväg</span><span class="sxs-lookup"><span data-stu-id="dba47-221">Path</span></span> | <span data-ttu-id="dba47-222">Typ</span><span class="sxs-lookup"><span data-stu-id="dba47-222">Type</span></span> | <span data-ttu-id="dba47-223">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dba47-223">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dba47-224">händelseantal [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-224">event [0] count</span></span> |<span data-ttu-id="dba47-225">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-225">integer</span></span> |<span data-ttu-id="dba47-226">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="dba47-226">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="dba47-227">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="dba47-227">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="dba47-228">händelsenamn [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-228">event [0] name</span></span> |<span data-ttu-id="dba47-229">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-229">string</span></span> |<span data-ttu-id="dba47-230">Händelsenamn.</span><span class="sxs-lookup"><span data-stu-id="dba47-230">Event name.</span></span>  <span data-ttu-id="dba47-231">Maxlängd 250.</span><span class="sxs-lookup"><span data-stu-id="dba47-231">Max length 250.</span></span> |
| <span data-ttu-id="dba47-232">händelsen [0] url</span><span class="sxs-lookup"><span data-stu-id="dba47-232">event [0] url</span></span> |<span data-ttu-id="dba47-233">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-233">string</span></span> | |
| <span data-ttu-id="dba47-234">händelsen [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="dba47-234">event [0] urlData.base</span></span> |<span data-ttu-id="dba47-235">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-235">string</span></span> | |
| <span data-ttu-id="dba47-236">händelsen [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="dba47-236">event [0] urlData.host</span></span> |<span data-ttu-id="dba47-237">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-237">string</span></span> | |

## <a name="exceptions"></a><span data-ttu-id="dba47-238">Undantag</span><span class="sxs-lookup"><span data-stu-id="dba47-238">Exceptions</span></span>
<span data-ttu-id="dba47-239">Rapporter [undantag](app-insights-asp-net-exceptions.md) i hello server och hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="dba47-239">Reports [exceptions](app-insights-asp-net-exceptions.md) in hello server and in hello browser.</span></span>

| <span data-ttu-id="dba47-240">Sökväg</span><span class="sxs-lookup"><span data-stu-id="dba47-240">Path</span></span> | <span data-ttu-id="dba47-241">Typ</span><span class="sxs-lookup"><span data-stu-id="dba47-241">Type</span></span> | <span data-ttu-id="dba47-242">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dba47-242">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dba47-243">sammansättningen basicException [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-243">basicException [0] assembly</span></span> |<span data-ttu-id="dba47-244">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-244">string</span></span> | |
| <span data-ttu-id="dba47-245">Antal basicException [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-245">basicException [0] count</span></span> |<span data-ttu-id="dba47-246">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-246">integer</span></span> |<span data-ttu-id="dba47-247">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="dba47-247">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="dba47-248">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="dba47-248">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="dba47-249">basicException [0] exceptionGroup</span><span class="sxs-lookup"><span data-stu-id="dba47-249">basicException [0] exceptionGroup</span></span> |<span data-ttu-id="dba47-250">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-250">string</span></span> | |
| <span data-ttu-id="dba47-251">basicException [0] exceptionType</span><span class="sxs-lookup"><span data-stu-id="dba47-251">basicException [0] exceptionType</span></span> |<span data-ttu-id="dba47-252">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-252">string</span></span> | |
| <span data-ttu-id="dba47-253">basicException [0] failedUserCodeMethod</span><span class="sxs-lookup"><span data-stu-id="dba47-253">basicException [0] failedUserCodeMethod</span></span> |<span data-ttu-id="dba47-254">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-254">string</span></span> | |
| <span data-ttu-id="dba47-255">basicException [0] failedUserCodeAssembly</span><span class="sxs-lookup"><span data-stu-id="dba47-255">basicException [0] failedUserCodeAssembly</span></span> |<span data-ttu-id="dba47-256">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-256">string</span></span> | |
| <span data-ttu-id="dba47-257">basicException [0] handledAt</span><span class="sxs-lookup"><span data-stu-id="dba47-257">basicException [0] handledAt</span></span> |<span data-ttu-id="dba47-258">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-258">string</span></span> | |
| <span data-ttu-id="dba47-259">basicException [0] hasFullStack</span><span class="sxs-lookup"><span data-stu-id="dba47-259">basicException [0] hasFullStack</span></span> |<span data-ttu-id="dba47-260">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="dba47-260">boolean</span></span> | |
| <span data-ttu-id="dba47-261">basicException [0]-id</span><span class="sxs-lookup"><span data-stu-id="dba47-261">basicException [0] id</span></span> |<span data-ttu-id="dba47-262">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-262">string</span></span> | |
| <span data-ttu-id="dba47-263">metoden basicException [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-263">basicException [0] method</span></span> |<span data-ttu-id="dba47-264">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-264">string</span></span> | |
| <span data-ttu-id="dba47-265">basicException [0] meddelande</span><span class="sxs-lookup"><span data-stu-id="dba47-265">basicException [0] message</span></span> |<span data-ttu-id="dba47-266">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-266">string</span></span> |<span data-ttu-id="dba47-267">Undantagsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="dba47-267">Exception message.</span></span> <span data-ttu-id="dba47-268">Maxlängd 10k.</span><span class="sxs-lookup"><span data-stu-id="dba47-268">Max length 10k.</span></span> |
| <span data-ttu-id="dba47-269">basicException [0] outerExceptionMessage</span><span class="sxs-lookup"><span data-stu-id="dba47-269">basicException [0] outerExceptionMessage</span></span> |<span data-ttu-id="dba47-270">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-270">string</span></span> | |
| <span data-ttu-id="dba47-271">basicException [0] outerExceptionThrownAtAssembly</span><span class="sxs-lookup"><span data-stu-id="dba47-271">basicException [0] outerExceptionThrownAtAssembly</span></span> |<span data-ttu-id="dba47-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-272">string</span></span> | |
| <span data-ttu-id="dba47-273">basicException [0] outerExceptionThrownAtMethod</span><span class="sxs-lookup"><span data-stu-id="dba47-273">basicException [0] outerExceptionThrownAtMethod</span></span> |<span data-ttu-id="dba47-274">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-274">string</span></span> | |
| <span data-ttu-id="dba47-275">basicException [0] outerExceptionType</span><span class="sxs-lookup"><span data-stu-id="dba47-275">basicException [0] outerExceptionType</span></span> |<span data-ttu-id="dba47-276">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-276">string</span></span> | |
| <span data-ttu-id="dba47-277">basicException [0] outerId</span><span class="sxs-lookup"><span data-stu-id="dba47-277">basicException [0] outerId</span></span> |<span data-ttu-id="dba47-278">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-278">string</span></span> | |
| <span data-ttu-id="dba47-279">basicException [0] parsedStack [0] sammansättning</span><span class="sxs-lookup"><span data-stu-id="dba47-279">basicException [0] parsedStack [0] assembly</span></span> |<span data-ttu-id="dba47-280">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-280">string</span></span> | |
| <span data-ttu-id="dba47-281">basicException [0] parsedStack [0] filnamn</span><span class="sxs-lookup"><span data-stu-id="dba47-281">basicException [0] parsedStack [0] fileName</span></span> |<span data-ttu-id="dba47-282">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-282">string</span></span> | |
| <span data-ttu-id="dba47-283">basicException [0] parsedStack [0] nivå</span><span class="sxs-lookup"><span data-stu-id="dba47-283">basicException [0] parsedStack [0] level</span></span> |<span data-ttu-id="dba47-284">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-284">integer</span></span> | |
| <span data-ttu-id="dba47-285">basicException [0] parsedStack [0] rad</span><span class="sxs-lookup"><span data-stu-id="dba47-285">basicException [0] parsedStack [0] line</span></span> |<span data-ttu-id="dba47-286">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-286">integer</span></span> | |
| <span data-ttu-id="dba47-287">basicException [0] parsedStack [0] metod</span><span class="sxs-lookup"><span data-stu-id="dba47-287">basicException [0] parsedStack [0] method</span></span> |<span data-ttu-id="dba47-288">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-288">string</span></span> | |
| <span data-ttu-id="dba47-289">basicException [0] stack</span><span class="sxs-lookup"><span data-stu-id="dba47-289">basicException [0] stack</span></span> |<span data-ttu-id="dba47-290">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-290">string</span></span> |<span data-ttu-id="dba47-291">Maxlängd 10k</span><span class="sxs-lookup"><span data-stu-id="dba47-291">Max length 10k</span></span> |
| <span data-ttu-id="dba47-292">typeName basicException [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-292">basicException [0] typeName</span></span> |<span data-ttu-id="dba47-293">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-293">string</span></span> | |

## <a name="trace-messages"></a><span data-ttu-id="dba47-294">Spåra meddelanden</span><span class="sxs-lookup"><span data-stu-id="dba47-294">Trace Messages</span></span>
<span data-ttu-id="dba47-295">Skickas av [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace), och av hello [loggning kort](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="dba47-295">Sent by [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace), and by hello [logging adapters](app-insights-asp-net-trace-logs.md).</span></span>

| <span data-ttu-id="dba47-296">Sökväg</span><span class="sxs-lookup"><span data-stu-id="dba47-296">Path</span></span> | <span data-ttu-id="dba47-297">Typ</span><span class="sxs-lookup"><span data-stu-id="dba47-297">Type</span></span> | <span data-ttu-id="dba47-298">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dba47-298">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dba47-299">meddelandet [0] loggningsnamn</span><span class="sxs-lookup"><span data-stu-id="dba47-299">message [0] loggerName</span></span> |<span data-ttu-id="dba47-300">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-300">string</span></span> | |
| <span data-ttu-id="dba47-301">[0] meddelandeparametrar</span><span class="sxs-lookup"><span data-stu-id="dba47-301">message [0] parameters</span></span> |<span data-ttu-id="dba47-302">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-302">string</span></span> | |
| <span data-ttu-id="dba47-303">meddelandet [0] rådata</span><span class="sxs-lookup"><span data-stu-id="dba47-303">message [0] raw</span></span> |<span data-ttu-id="dba47-304">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-304">string</span></span> |<span data-ttu-id="dba47-305">hello loggmeddelande, får innehålla högst 10k.</span><span class="sxs-lookup"><span data-stu-id="dba47-305">hello log message, max length 10k.</span></span> |
| <span data-ttu-id="dba47-306">meddelandet [0] severityLevel</span><span class="sxs-lookup"><span data-stu-id="dba47-306">message [0] severityLevel</span></span> |<span data-ttu-id="dba47-307">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-307">string</span></span> | |

## <a name="remote-dependency"></a><span data-ttu-id="dba47-308">Fjärråtkomst beroende</span><span class="sxs-lookup"><span data-stu-id="dba47-308">Remote dependency</span></span>
<span data-ttu-id="dba47-309">Skickas av TrackDependency.</span><span class="sxs-lookup"><span data-stu-id="dba47-309">Sent by TrackDependency.</span></span> <span data-ttu-id="dba47-310">Används tooreport prestanda och användning av [anropar toodependencies](app-insights-asp-net-dependencies.md) i hello server och AJAX-anrop i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="dba47-310">Used tooreport performance and usage of [calls toodependencies](app-insights-asp-net-dependencies.md) in hello server, and AJAX calls in hello browser.</span></span>

| <span data-ttu-id="dba47-311">Sökväg</span><span class="sxs-lookup"><span data-stu-id="dba47-311">Path</span></span> | <span data-ttu-id="dba47-312">Typ</span><span class="sxs-lookup"><span data-stu-id="dba47-312">Type</span></span> | <span data-ttu-id="dba47-313">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dba47-313">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dba47-314">asynkrona remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-314">remoteDependency [0] async</span></span> |<span data-ttu-id="dba47-315">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="dba47-315">boolean</span></span> | |
| <span data-ttu-id="dba47-316">remoteDependency [0] baseName</span><span class="sxs-lookup"><span data-stu-id="dba47-316">remoteDependency [0] baseName</span></span> |<span data-ttu-id="dba47-317">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-317">string</span></span> | |
| <span data-ttu-id="dba47-318">remoteDependency [0] commandName</span><span class="sxs-lookup"><span data-stu-id="dba47-318">remoteDependency [0] commandName</span></span> |<span data-ttu-id="dba47-319">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-319">string</span></span> |<span data-ttu-id="dba47-320">Till exempel ”home/index”</span><span class="sxs-lookup"><span data-stu-id="dba47-320">For example "home/index"</span></span> |
| <span data-ttu-id="dba47-321">Antal remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-321">remoteDependency [0] count</span></span> |<span data-ttu-id="dba47-322">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-322">integer</span></span> |<span data-ttu-id="dba47-323">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="dba47-323">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="dba47-324">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="dba47-324">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="dba47-325">remoteDependency [0] dependencyTypeName</span><span class="sxs-lookup"><span data-stu-id="dba47-325">remoteDependency [0] dependencyTypeName</span></span> |<span data-ttu-id="dba47-326">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-326">string</span></span> |<span data-ttu-id="dba47-327">HTTP, SQL...</span><span class="sxs-lookup"><span data-stu-id="dba47-327">HTTP, SQL, ...</span></span> |
| <span data-ttu-id="dba47-328">remoteDependency [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="dba47-328">remoteDependency [0] durationMetric.value</span></span> |<span data-ttu-id="dba47-329">Antal</span><span class="sxs-lookup"><span data-stu-id="dba47-329">number</span></span> |<span data-ttu-id="dba47-330">Tiden från anropet toocompletion svar av beroende</span><span class="sxs-lookup"><span data-stu-id="dba47-330">Time from call toocompletion of response by dependency</span></span> |
| <span data-ttu-id="dba47-331">remoteDependency [0]-id</span><span class="sxs-lookup"><span data-stu-id="dba47-331">remoteDependency [0] id</span></span> |<span data-ttu-id="dba47-332">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-332">string</span></span> | |
| <span data-ttu-id="dba47-333">remoteDependency [0] namn</span><span class="sxs-lookup"><span data-stu-id="dba47-333">remoteDependency [0] name</span></span> |<span data-ttu-id="dba47-334">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-334">string</span></span> |<span data-ttu-id="dba47-335">URL-adressen.</span><span class="sxs-lookup"><span data-stu-id="dba47-335">Url.</span></span> <span data-ttu-id="dba47-336">Maxlängd 250.</span><span class="sxs-lookup"><span data-stu-id="dba47-336">Max length 250.</span></span> |
| <span data-ttu-id="dba47-337">remoteDependency [0] resultCode</span><span class="sxs-lookup"><span data-stu-id="dba47-337">remoteDependency [0] resultCode</span></span> |<span data-ttu-id="dba47-338">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-338">string</span></span> |<span data-ttu-id="dba47-339">från HTTP-beroendet</span><span class="sxs-lookup"><span data-stu-id="dba47-339">from HTTP dependency</span></span> |
| <span data-ttu-id="dba47-340">remoteDependency [0] lyckades</span><span class="sxs-lookup"><span data-stu-id="dba47-340">remoteDependency [0] success</span></span> |<span data-ttu-id="dba47-341">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="dba47-341">boolean</span></span> | |
| <span data-ttu-id="dba47-342">typ av remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-342">remoteDependency [0] type</span></span> |<span data-ttu-id="dba47-343">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-343">string</span></span> |<span data-ttu-id="dba47-344">HTTP, Sql...</span><span class="sxs-lookup"><span data-stu-id="dba47-344">Http, Sql,...</span></span> |
| <span data-ttu-id="dba47-345">remoteDependency [0] url</span><span class="sxs-lookup"><span data-stu-id="dba47-345">remoteDependency [0] url</span></span> |<span data-ttu-id="dba47-346">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-346">string</span></span> |<span data-ttu-id="dba47-347">Maxlängd 2000</span><span class="sxs-lookup"><span data-stu-id="dba47-347">Max length 2000</span></span> |
| <span data-ttu-id="dba47-348">remoteDependency [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="dba47-348">remoteDependency [0] urlData.base</span></span> |<span data-ttu-id="dba47-349">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-349">string</span></span> |<span data-ttu-id="dba47-350">Maxlängd 2000</span><span class="sxs-lookup"><span data-stu-id="dba47-350">Max length 2000</span></span> |
| <span data-ttu-id="dba47-351">remoteDependency [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="dba47-351">remoteDependency [0] urlData.hashTag</span></span> |<span data-ttu-id="dba47-352">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-352">string</span></span> | |
| <span data-ttu-id="dba47-353">remoteDependency [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="dba47-353">remoteDependency [0] urlData.host</span></span> |<span data-ttu-id="dba47-354">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-354">string</span></span> |<span data-ttu-id="dba47-355">Maxlängd 200</span><span class="sxs-lookup"><span data-stu-id="dba47-355">Max length 200</span></span> |

## <a name="requests"></a><span data-ttu-id="dba47-356">Begäranden</span><span class="sxs-lookup"><span data-stu-id="dba47-356">Requests</span></span>
<span data-ttu-id="dba47-357">Skickas av [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest).</span><span class="sxs-lookup"><span data-stu-id="dba47-357">Sent by [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest).</span></span> <span data-ttu-id="dba47-358">hello standard moduler använder den här tooreports serversvarstid, mätt hello-servern.</span><span class="sxs-lookup"><span data-stu-id="dba47-358">hello standard modules use this tooreports server response time, measured at hello server.</span></span>

| <span data-ttu-id="dba47-359">Sökväg</span><span class="sxs-lookup"><span data-stu-id="dba47-359">Path</span></span> | <span data-ttu-id="dba47-360">Typ</span><span class="sxs-lookup"><span data-stu-id="dba47-360">Type</span></span> | <span data-ttu-id="dba47-361">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dba47-361">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dba47-362">antalet begäranden [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-362">request [0] count</span></span> |<span data-ttu-id="dba47-363">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-363">integer</span></span> |<span data-ttu-id="dba47-364">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="dba47-364">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="dba47-365">Till exempel: 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="dba47-365">For example: 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="dba47-366">begäran [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="dba47-366">request [0] durationMetric.value</span></span> |<span data-ttu-id="dba47-367">Antal</span><span class="sxs-lookup"><span data-stu-id="dba47-367">number</span></span> |<span data-ttu-id="dba47-368">Tiden från begäran ankommande tooresponse.</span><span class="sxs-lookup"><span data-stu-id="dba47-368">Time from request arriving tooresponse.</span></span> <span data-ttu-id="dba47-369">1e7 == 1s</span><span class="sxs-lookup"><span data-stu-id="dba47-369">1e7 == 1s</span></span> |
| <span data-ttu-id="dba47-370">id för förfrågan [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-370">request [0] id</span></span> |<span data-ttu-id="dba47-371">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-371">string</span></span> |<span data-ttu-id="dba47-372">Åtgärds-id</span><span class="sxs-lookup"><span data-stu-id="dba47-372">Operation id</span></span> |
| <span data-ttu-id="dba47-373">namn på förfrågan [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-373">request [0] name</span></span> |<span data-ttu-id="dba47-374">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-374">string</span></span> |<span data-ttu-id="dba47-375">GET/POST + bas-url.</span><span class="sxs-lookup"><span data-stu-id="dba47-375">GET/POST + url base.</span></span>  <span data-ttu-id="dba47-376">Maxlängd 250</span><span class="sxs-lookup"><span data-stu-id="dba47-376">Max length 250</span></span> |
| <span data-ttu-id="dba47-377">begäran [0] responseCode</span><span class="sxs-lookup"><span data-stu-id="dba47-377">request [0] responseCode</span></span> |<span data-ttu-id="dba47-378">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-378">integer</span></span> |<span data-ttu-id="dba47-379">HTTP-svaret tooclient</span><span class="sxs-lookup"><span data-stu-id="dba47-379">HTTP response sent tooclient</span></span> |
| <span data-ttu-id="dba47-380">begäran [0] lyckades</span><span class="sxs-lookup"><span data-stu-id="dba47-380">request [0] success</span></span> |<span data-ttu-id="dba47-381">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="dba47-381">boolean</span></span> |<span data-ttu-id="dba47-382">Standard == (responseCode &lt; 400)</span><span class="sxs-lookup"><span data-stu-id="dba47-382">Default == (responseCode &lt; 400)</span></span> |
| <span data-ttu-id="dba47-383">url-begäran [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-383">request [0] url</span></span> |<span data-ttu-id="dba47-384">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-384">string</span></span> |<span data-ttu-id="dba47-385">Inte inklusive värden</span><span class="sxs-lookup"><span data-stu-id="dba47-385">Not including host</span></span> |
| <span data-ttu-id="dba47-386">begäran [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="dba47-386">request [0] urlData.base</span></span> |<span data-ttu-id="dba47-387">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-387">string</span></span> | |
| <span data-ttu-id="dba47-388">begäran [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="dba47-388">request [0] urlData.hashTag</span></span> |<span data-ttu-id="dba47-389">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-389">string</span></span> | |
| <span data-ttu-id="dba47-390">begäran [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="dba47-390">request [0] urlData.host</span></span> |<span data-ttu-id="dba47-391">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-391">string</span></span> | |

## <a name="page-view-performance"></a><span data-ttu-id="dba47-392">Vyn sida prestanda</span><span class="sxs-lookup"><span data-stu-id="dba47-392">Page View Performance</span></span>
<span data-ttu-id="dba47-393">Skickas av hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="dba47-393">Sent by hello browser.</span></span> <span data-ttu-id="dba47-394">Mått hello tid tooprocess en sida från användaren initierande hello begäran toodisplay fullständig (exklusive asynkrona AJAX-anrop).</span><span class="sxs-lookup"><span data-stu-id="dba47-394">Measures hello time tooprocess a page, from user initiating hello request toodisplay complete (excluding async AJAX calls).</span></span>

<span data-ttu-id="dba47-395">Kontexten värden visa klientens operativsystem och version på webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="dba47-395">Context values show client OS and browser version.</span></span>

| <span data-ttu-id="dba47-396">Sökväg</span><span class="sxs-lookup"><span data-stu-id="dba47-396">Path</span></span> | <span data-ttu-id="dba47-397">Typ</span><span class="sxs-lookup"><span data-stu-id="dba47-397">Type</span></span> | <span data-ttu-id="dba47-398">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dba47-398">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dba47-399">clientPerformance [0] clientProcess.value</span><span class="sxs-lookup"><span data-stu-id="dba47-399">clientPerformance [0] clientProcess.value</span></span> |<span data-ttu-id="dba47-400">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-400">integer</span></span> |<span data-ttu-id="dba47-401">Tiden från slutet av tar emot hello HTML toodisplaying hello sida.</span><span class="sxs-lookup"><span data-stu-id="dba47-401">Time from end of receiving hello HTML toodisplaying hello page.</span></span> |
| <span data-ttu-id="dba47-402">clientPerformance [0] namn</span><span class="sxs-lookup"><span data-stu-id="dba47-402">clientPerformance [0] name</span></span> |<span data-ttu-id="dba47-403">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-403">string</span></span> | |
| <span data-ttu-id="dba47-404">clientPerformance [0] networkConnection.value</span><span class="sxs-lookup"><span data-stu-id="dba47-404">clientPerformance [0] networkConnection.value</span></span> |<span data-ttu-id="dba47-405">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-405">integer</span></span> |<span data-ttu-id="dba47-406">Tidsåtgång tooestablish en nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="dba47-406">Time taken tooestablish a network connection.</span></span> |
| <span data-ttu-id="dba47-407">clientPerformance [0] receiveRequest.value</span><span class="sxs-lookup"><span data-stu-id="dba47-407">clientPerformance [0] receiveRequest.value</span></span> |<span data-ttu-id="dba47-408">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-408">integer</span></span> |<span data-ttu-id="dba47-409">Tiden från slutet av skickar hello begäran tooreceiving hello HTML i svar.</span><span class="sxs-lookup"><span data-stu-id="dba47-409">Time from end of sending hello request tooreceiving hello HTML in reply.</span></span> |
| <span data-ttu-id="dba47-410">clientPerformance [0] sendRequest.value</span><span class="sxs-lookup"><span data-stu-id="dba47-410">clientPerformance [0] sendRequest.value</span></span> |<span data-ttu-id="dba47-411">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-411">integer</span></span> |<span data-ttu-id="dba47-412">Tiden från vidtas toosend hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="dba47-412">Time from taken toosend hello HTTP request.</span></span> |
| <span data-ttu-id="dba47-413">clientPerformance [0] total.value</span><span class="sxs-lookup"><span data-stu-id="dba47-413">clientPerformance [0] total.value</span></span> |<span data-ttu-id="dba47-414">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-414">integer</span></span> |<span data-ttu-id="dba47-415">Tiden från att starta toosend hello begäran toodisplaying hello sidan.</span><span class="sxs-lookup"><span data-stu-id="dba47-415">Time from starting toosend hello request toodisplaying hello page.</span></span> |
| <span data-ttu-id="dba47-416">clientPerformance [0] url</span><span class="sxs-lookup"><span data-stu-id="dba47-416">clientPerformance [0] url</span></span> |<span data-ttu-id="dba47-417">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-417">string</span></span> |<span data-ttu-id="dba47-418">URL för den här begäran</span><span class="sxs-lookup"><span data-stu-id="dba47-418">URL of this request</span></span> |
| <span data-ttu-id="dba47-419">clientPerformance [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="dba47-419">clientPerformance [0] urlData.base</span></span> |<span data-ttu-id="dba47-420">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-420">string</span></span> | |
| <span data-ttu-id="dba47-421">clientPerformance [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="dba47-421">clientPerformance [0] urlData.hashTag</span></span> |<span data-ttu-id="dba47-422">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-422">string</span></span> | |
| <span data-ttu-id="dba47-423">clientPerformance [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="dba47-423">clientPerformance [0] urlData.host</span></span> |<span data-ttu-id="dba47-424">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-424">string</span></span> | |
| <span data-ttu-id="dba47-425">clientPerformance [0] urlData.protocol</span><span class="sxs-lookup"><span data-stu-id="dba47-425">clientPerformance [0] urlData.protocol</span></span> |<span data-ttu-id="dba47-426">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-426">string</span></span> | |

## <a name="page-views"></a><span data-ttu-id="dba47-427">Sidvisningar</span><span class="sxs-lookup"><span data-stu-id="dba47-427">Page Views</span></span>
<span data-ttu-id="dba47-428">Skickas av trackPageView() eller [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)</span><span class="sxs-lookup"><span data-stu-id="dba47-428">Sent by trackPageView() or [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)</span></span>

| <span data-ttu-id="dba47-429">Sökväg</span><span class="sxs-lookup"><span data-stu-id="dba47-429">Path</span></span> | <span data-ttu-id="dba47-430">Typ</span><span class="sxs-lookup"><span data-stu-id="dba47-430">Type</span></span> | <span data-ttu-id="dba47-431">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dba47-431">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dba47-432">Visa [0] antal</span><span class="sxs-lookup"><span data-stu-id="dba47-432">view [0] count</span></span> |<span data-ttu-id="dba47-433">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-433">integer</span></span> |<span data-ttu-id="dba47-434">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="dba47-434">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="dba47-435">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="dba47-435">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="dba47-436">Visa [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="dba47-436">view [0] durationMetric.value</span></span> |<span data-ttu-id="dba47-437">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-437">integer</span></span> |<span data-ttu-id="dba47-438">Värdet som du kan också ställa in i trackPageView() eller startTrackPage() - stopTrackPage().</span><span class="sxs-lookup"><span data-stu-id="dba47-438">Value optionally set in trackPageView() or by startTrackPage() - stopTrackPage().</span></span> <span data-ttu-id="dba47-439">Hej inte samma som clientPerformance värden.</span><span class="sxs-lookup"><span data-stu-id="dba47-439">Not hello same as clientPerformance values.</span></span> |
| <span data-ttu-id="dba47-440">vynamn [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-440">view [0] name</span></span> |<span data-ttu-id="dba47-441">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-441">string</span></span> |<span data-ttu-id="dba47-442">Rubrik.</span><span class="sxs-lookup"><span data-stu-id="dba47-442">Page title.</span></span>  <span data-ttu-id="dba47-443">Maxlängd 250</span><span class="sxs-lookup"><span data-stu-id="dba47-443">Max length 250</span></span> |
| <span data-ttu-id="dba47-444">Visa [0] url</span><span class="sxs-lookup"><span data-stu-id="dba47-444">view [0] url</span></span> |<span data-ttu-id="dba47-445">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-445">string</span></span> | |
| <span data-ttu-id="dba47-446">Visa [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="dba47-446">view [0] urlData.base</span></span> |<span data-ttu-id="dba47-447">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-447">string</span></span> | |
| <span data-ttu-id="dba47-448">Visa [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="dba47-448">view [0] urlData.hashTag</span></span> |<span data-ttu-id="dba47-449">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-449">string</span></span> | |
| <span data-ttu-id="dba47-450">Visa [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="dba47-450">view [0] urlData.host</span></span> |<span data-ttu-id="dba47-451">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-451">string</span></span> | |

## <a name="availability"></a><span data-ttu-id="dba47-452">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="dba47-452">Availability</span></span>
<span data-ttu-id="dba47-453">Rapporter [tillgänglighet webbtester](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="dba47-453">Reports [availability web tests](app-insights-monitor-web-app-availability.md).</span></span>

| <span data-ttu-id="dba47-454">Sökväg</span><span class="sxs-lookup"><span data-stu-id="dba47-454">Path</span></span> | <span data-ttu-id="dba47-455">Typ</span><span class="sxs-lookup"><span data-stu-id="dba47-455">Type</span></span> | <span data-ttu-id="dba47-456">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="dba47-456">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dba47-457">tillgänglighet [0] availabilityMetric.name</span><span class="sxs-lookup"><span data-stu-id="dba47-457">availability [0] availabilityMetric.name</span></span> |<span data-ttu-id="dba47-458">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-458">string</span></span> |<span data-ttu-id="dba47-459">availability</span><span class="sxs-lookup"><span data-stu-id="dba47-459">availability</span></span> |
| <span data-ttu-id="dba47-460">tillgänglighet [0] availabilityMetric.value</span><span class="sxs-lookup"><span data-stu-id="dba47-460">availability [0] availabilityMetric.value</span></span> |<span data-ttu-id="dba47-461">Antal</span><span class="sxs-lookup"><span data-stu-id="dba47-461">number</span></span> |<span data-ttu-id="dba47-462">1.0 eller 0,0</span><span class="sxs-lookup"><span data-stu-id="dba47-462">1.0 or 0.0</span></span> |
| <span data-ttu-id="dba47-463">Antal tillgänglighet [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-463">availability [0] count</span></span> |<span data-ttu-id="dba47-464">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-464">integer</span></span> |<span data-ttu-id="dba47-465">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="dba47-465">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="dba47-466">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="dba47-466">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="dba47-467">tillgänglighet [0] dataSizeMetric.name</span><span class="sxs-lookup"><span data-stu-id="dba47-467">availability [0] dataSizeMetric.name</span></span> |<span data-ttu-id="dba47-468">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-468">string</span></span> | |
| <span data-ttu-id="dba47-469">tillgänglighet [0] dataSizeMetric.value</span><span class="sxs-lookup"><span data-stu-id="dba47-469">availability [0] dataSizeMetric.value</span></span> |<span data-ttu-id="dba47-470">heltal</span><span class="sxs-lookup"><span data-stu-id="dba47-470">integer</span></span> | |
| <span data-ttu-id="dba47-471">tillgänglighet [0] durationMetric.name</span><span class="sxs-lookup"><span data-stu-id="dba47-471">availability [0] durationMetric.name</span></span> |<span data-ttu-id="dba47-472">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-472">string</span></span> | |
| <span data-ttu-id="dba47-473">tillgänglighet [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="dba47-473">availability [0] durationMetric.value</span></span> |<span data-ttu-id="dba47-474">Antal</span><span class="sxs-lookup"><span data-stu-id="dba47-474">number</span></span> |<span data-ttu-id="dba47-475">Varaktighet för testet.</span><span class="sxs-lookup"><span data-stu-id="dba47-475">Duration of test.</span></span> <span data-ttu-id="dba47-476">1e7 == 1s</span><span class="sxs-lookup"><span data-stu-id="dba47-476">1e7==1s</span></span> |
| <span data-ttu-id="dba47-477">meddelande om tillgänglighet [0]</span><span class="sxs-lookup"><span data-stu-id="dba47-477">availability [0] message</span></span> |<span data-ttu-id="dba47-478">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-478">string</span></span> |<span data-ttu-id="dba47-479">Fel diagnostik</span><span class="sxs-lookup"><span data-stu-id="dba47-479">Failure diagnostic</span></span> |
| <span data-ttu-id="dba47-480">tillgänglighet [0] resultat</span><span class="sxs-lookup"><span data-stu-id="dba47-480">availability [0] result</span></span> |<span data-ttu-id="dba47-481">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-481">string</span></span> |<span data-ttu-id="dba47-482">Eller inte</span><span class="sxs-lookup"><span data-stu-id="dba47-482">Pass/Fail</span></span> |
| <span data-ttu-id="dba47-483">tillgänglighet [0] runLocation</span><span class="sxs-lookup"><span data-stu-id="dba47-483">availability [0] runLocation</span></span> |<span data-ttu-id="dba47-484">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-484">string</span></span> |<span data-ttu-id="dba47-485">GEO-källan för HTTP-begäranden</span><span class="sxs-lookup"><span data-stu-id="dba47-485">Geo source of http req</span></span> |
| <span data-ttu-id="dba47-486">tillgänglighet [0] testName</span><span class="sxs-lookup"><span data-stu-id="dba47-486">availability [0] testName</span></span> |<span data-ttu-id="dba47-487">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-487">string</span></span> | |
| <span data-ttu-id="dba47-488">tillgänglighet [0] testRunId</span><span class="sxs-lookup"><span data-stu-id="dba47-488">availability [0] testRunId</span></span> |<span data-ttu-id="dba47-489">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-489">string</span></span> | |
| <span data-ttu-id="dba47-490">tillgänglighet [0] testTimestamp</span><span class="sxs-lookup"><span data-stu-id="dba47-490">availability [0] testTimestamp</span></span> |<span data-ttu-id="dba47-491">Sträng</span><span class="sxs-lookup"><span data-stu-id="dba47-491">string</span></span> | |

## <a name="metrics"></a><span data-ttu-id="dba47-492">Mått</span><span class="sxs-lookup"><span data-stu-id="dba47-492">Metrics</span></span>
<span data-ttu-id="dba47-493">Genereras av TrackMetric().</span><span class="sxs-lookup"><span data-stu-id="dba47-493">Generated by TrackMetric().</span></span>

<span data-ttu-id="dba47-494">hello värde hittades i context.custom.metrics[0]</span><span class="sxs-lookup"><span data-stu-id="dba47-494">hello metric value is found in context.custom.metrics[0]</span></span>

<span data-ttu-id="dba47-495">Exempel:</span><span class="sxs-lookup"><span data-stu-id="dba47-495">For example:</span></span>

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a><span data-ttu-id="dba47-496">Om måttvärden</span><span class="sxs-lookup"><span data-stu-id="dba47-496">About metric values</span></span>
<span data-ttu-id="dba47-497">Måttvärden, både i mått rapporter och på andra platser, rapporteras med en standard objektstruktur.</span><span class="sxs-lookup"><span data-stu-id="dba47-497">Metric values, both in metric reports and elsewhere, are reported with a standard object structure.</span></span> <span data-ttu-id="dba47-498">Exempel:</span><span class="sxs-lookup"><span data-stu-id="dba47-498">For example:</span></span>

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

<span data-ttu-id="dba47-499">För närvarande - men detta kan ändras i framtida - alla värden som rapporterats från hello standard SDK-moduler, hello `count==1` och endast hello `name` och `value` fälten är användbara.</span><span class="sxs-lookup"><span data-stu-id="dba47-499">Currently - though this might change in hello future - in all values reported from hello standard SDK modules, `count==1` and only hello `name` and `value` fields are useful.</span></span> <span data-ttu-id="dba47-500">hello enda fallet där de skulle vara olika skulle vara om du skriver TrackMetric anrop i som du anger hello andra parametrar.</span><span class="sxs-lookup"><span data-stu-id="dba47-500">hello only case where they would be different would be if you write your own TrackMetric calls in which you set hello other parameters.</span></span>

<span data-ttu-id="dba47-501">Hej syfte hello andra fält är tooallow mått toobe samman i hello SDK, tooreduce trafik toohello portal.</span><span class="sxs-lookup"><span data-stu-id="dba47-501">hello purpose of hello other fields is tooallow metrics toobe aggregated in hello SDK, tooreduce traffic toohello portal.</span></span> <span data-ttu-id="dba47-502">Du kan till exempel genomsnittlig flera efterföljande avläsningar innan du skickar varje mått rapport.</span><span class="sxs-lookup"><span data-stu-id="dba47-502">For example, you could average several successive readings before sending each metric report.</span></span> <span data-ttu-id="dba47-503">Du skulle sedan beräkna hello min, max, standardavvikelse och samlat värde (sum eller medelvärde) och ange talet för antal toohello av avläsningar som representeras av hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="dba47-503">Then you would calculate hello min, max, standard deviation and aggregate value (sum or average) and set count toohello number of readings represented by hello report.</span></span>

<span data-ttu-id="dba47-504">Vi har utelämnats hello används sällan fält count, min, max, stdDev och sampledValue i hello tabellerna ovan.</span><span class="sxs-lookup"><span data-stu-id="dba47-504">In hello tables above, we have omitted hello rarely-used fields count, min, max, stdDev and sampledValue.</span></span>

<span data-ttu-id="dba47-505">I stället före sammanställa statistik och, du kan använda [provtagning](app-insights-sampling.md) om du behöver tooreduce hello telemetrivolym.</span><span class="sxs-lookup"><span data-stu-id="dba47-505">Instead of pre-aggregating metrics, you can use [sampling](app-insights-sampling.md) if you need tooreduce hello volume of telemetry.</span></span>

### <a name="durations"></a><span data-ttu-id="dba47-506">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="dba47-506">Durations</span></span>
<span data-ttu-id="dba47-507">Förutom där annat anges, representeras varaktighet i tiondels mikrosekundnivå, så att 10000000.0 innebär 1 sekund.</span><span class="sxs-lookup"><span data-stu-id="dba47-507">Except where otherwise noted, durations are represented in tenths of a microsecond, so that 10000000.0 means 1 second.</span></span>

## <a name="see-also"></a><span data-ttu-id="dba47-508">Se även</span><span class="sxs-lookup"><span data-stu-id="dba47-508">See also</span></span>
* [<span data-ttu-id="dba47-509">Application Insights</span><span class="sxs-lookup"><span data-stu-id="dba47-509">Application Insights</span></span>](app-insights-overview.md)
* [<span data-ttu-id="dba47-510">Löpande Export</span><span class="sxs-lookup"><span data-stu-id="dba47-510">Continuous Export</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="dba47-511">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="dba47-511">Code samples</span></span>](app-insights-export-telemetry.md#code-samples)

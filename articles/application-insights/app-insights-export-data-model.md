---
title: Azure Application Insights datamodellen | Microsoft Docs
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
ms.openlocfilehash: a485ddd555f65473d81896effc4a3562bda71410
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-export-data-model"></a><span data-ttu-id="ebefe-103">Application Insights Export-datamodell</span><span class="sxs-lookup"><span data-stu-id="ebefe-103">Application Insights Export Data Model</span></span>
<span data-ttu-id="ebefe-104">Den här tabellen innehåller egenskaper för telemetri som skickats från den [Programinsikter](app-insights-overview.md) SDK: er till portalen.</span><span class="sxs-lookup"><span data-stu-id="ebefe-104">This table lists the properties of telemetry sent from the [Application Insights](app-insights-overview.md) SDKs to the portal.</span></span>
<span data-ttu-id="ebefe-105">Du ser dessa egenskaper i data från [löpande Export](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="ebefe-105">You'll see these properties in data output from [Continuous Export](app-insights-export-telemetry.md).</span></span>
<span data-ttu-id="ebefe-106">De visas också egenskapsfilter i [mått Explorer](app-insights-metrics-explorer.md) och [diagnostiska Sök](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="ebefe-106">They also appear in property filters in [Metric Explorer](app-insights-metrics-explorer.md) and [Diagnostic Search](app-insights-diagnostic-search.md).</span></span>

<span data-ttu-id="ebefe-107">Pekar på Observera:</span><span class="sxs-lookup"><span data-stu-id="ebefe-107">Points to note:</span></span>

* <span data-ttu-id="ebefe-108">`[0]`i dessa tabeller anger du en punkt i sökvägen där du måste infoga ett index. men det är inte alltid 0.</span><span class="sxs-lookup"><span data-stu-id="ebefe-108">`[0]` in these tables denotes a point in the path where you have to insert an index; but it isn't always 0.</span></span>
* <span data-ttu-id="ebefe-109">Tidsvaraktigheter finns i en mikrosekundnivå så 10000000 tiondels == 1 sekund.</span><span class="sxs-lookup"><span data-stu-id="ebefe-109">Time durations are in tenths of a microsecond, so 10000000 == 1 second.</span></span>
* <span data-ttu-id="ebefe-110">Datum och tider är UTC och anges i formatet ISO`yyyy-MM-DDThh:mm:ss.sssZ`</span><span class="sxs-lookup"><span data-stu-id="ebefe-110">Dates and times are UTC, and are given in the ISO format `yyyy-MM-DDThh:mm:ss.sssZ`</span></span>


## <a name="example"></a><span data-ttu-id="ebefe-111">Exempel</span><span class="sxs-lookup"><span data-stu-id="ebefe-111">Example</span></span>
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
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
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
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
  <span data-ttu-id="ebefe-112">}</span><span class="sxs-lookup"><span data-stu-id="ebefe-112">}</span></span>

## <a name="context"></a><span data-ttu-id="ebefe-113">Kontext</span><span class="sxs-lookup"><span data-stu-id="ebefe-113">Context</span></span>
<span data-ttu-id="ebefe-114">Alla typer av telemetri åtföljs av en kontext-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ebefe-114">All types of telemetry are accompanied by a context section.</span></span> <span data-ttu-id="ebefe-115">Alla dessa fält skickas med varje datapunkt.</span><span class="sxs-lookup"><span data-stu-id="ebefe-115">Not all of these fields are transmitted with every data point.</span></span>

| <span data-ttu-id="ebefe-116">Sökväg</span><span class="sxs-lookup"><span data-stu-id="ebefe-116">Path</span></span> | <span data-ttu-id="ebefe-117">Typ</span><span class="sxs-lookup"><span data-stu-id="ebefe-117">Type</span></span> | <span data-ttu-id="ebefe-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-118">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebefe-119">Context.Custom.dimensions [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-119">context.custom.dimensions [0]</span></span> |<span data-ttu-id="ebefe-120">objektet]</span><span class="sxs-lookup"><span data-stu-id="ebefe-120">object [ ]</span></span> |<span data-ttu-id="ebefe-121">Nyckel / värde-strängpar som anges av parametern anpassade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ebefe-121">Key-value string pairs set by custom properties parameter.</span></span> <span data-ttu-id="ebefe-122">Nyckeln får innehålla högst 100, värden får innehålla högst 1024.</span><span class="sxs-lookup"><span data-stu-id="ebefe-122">Key max length 100, values max length 1024.</span></span> <span data-ttu-id="ebefe-123">Fler än 100 unika värden egenskapen kan sökas men kan inte användas för segmentering.</span><span class="sxs-lookup"><span data-stu-id="ebefe-123">More than 100 unique values, the property can be searched but cannot be used for segmentation.</span></span> <span data-ttu-id="ebefe-124">Max 200 nycklar per ikey.</span><span class="sxs-lookup"><span data-stu-id="ebefe-124">Max 200 keys per ikey.</span></span> |
| <span data-ttu-id="ebefe-125">Context.Custom.Metrics [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-125">context.custom.metrics [0]</span></span> |<span data-ttu-id="ebefe-126">objektet]</span><span class="sxs-lookup"><span data-stu-id="ebefe-126">object [ ]</span></span> |<span data-ttu-id="ebefe-127">Nyckel-värdepar anges av parametern anpassade mått och av TrackMetrics.</span><span class="sxs-lookup"><span data-stu-id="ebefe-127">Key-value pairs set by custom measurements parameter and by TrackMetrics.</span></span> <span data-ttu-id="ebefe-128">Maximal nyckellängd 100, värdena kan vara numeriskt.</span><span class="sxs-lookup"><span data-stu-id="ebefe-128">Key max length 100, values may be numeric.</span></span> |
| <span data-ttu-id="ebefe-129">context.data.eventTime</span><span class="sxs-lookup"><span data-stu-id="ebefe-129">context.data.eventTime</span></span> |<span data-ttu-id="ebefe-130">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-130">string</span></span> |<span data-ttu-id="ebefe-131">UTC</span><span class="sxs-lookup"><span data-stu-id="ebefe-131">UTC</span></span> |
| <span data-ttu-id="ebefe-132">context.data.isSynthetic</span><span class="sxs-lookup"><span data-stu-id="ebefe-132">context.data.isSynthetic</span></span> |<span data-ttu-id="ebefe-133">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="ebefe-133">boolean</span></span> |<span data-ttu-id="ebefe-134">Begäran verkar komma från en bot eller web test.</span><span class="sxs-lookup"><span data-stu-id="ebefe-134">Request appears to come from a bot or web test.</span></span> |
| <span data-ttu-id="ebefe-135">context.data.samplingRate</span><span class="sxs-lookup"><span data-stu-id="ebefe-135">context.data.samplingRate</span></span> |<span data-ttu-id="ebefe-136">Antal</span><span class="sxs-lookup"><span data-stu-id="ebefe-136">number</span></span> |<span data-ttu-id="ebefe-137">Procentandelen telemetri som genererats av SDK som skickas till portalen.</span><span class="sxs-lookup"><span data-stu-id="ebefe-137">Percentage of telemetry generated by the SDK that is sent to portal.</span></span> <span data-ttu-id="ebefe-138">Intervallet 0,0 100,0.</span><span class="sxs-lookup"><span data-stu-id="ebefe-138">Range 0.0-100.0.</span></span> |
| <span data-ttu-id="ebefe-139">Context.Device</span><span class="sxs-lookup"><span data-stu-id="ebefe-139">context.device</span></span> |<span data-ttu-id="ebefe-140">Objektet</span><span class="sxs-lookup"><span data-stu-id="ebefe-140">object</span></span> |<span data-ttu-id="ebefe-141">Klientenheten</span><span class="sxs-lookup"><span data-stu-id="ebefe-141">Client device</span></span> |
| <span data-ttu-id="ebefe-142">Context.Device.Browser</span><span class="sxs-lookup"><span data-stu-id="ebefe-142">context.device.browser</span></span> |<span data-ttu-id="ebefe-143">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-143">string</span></span> |<span data-ttu-id="ebefe-144">Internet Explorer, Chrome...</span><span class="sxs-lookup"><span data-stu-id="ebefe-144">IE, Chrome, ...</span></span> |
| <span data-ttu-id="ebefe-145">context.device.browserVersion</span><span class="sxs-lookup"><span data-stu-id="ebefe-145">context.device.browserVersion</span></span> |<span data-ttu-id="ebefe-146">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-146">string</span></span> |<span data-ttu-id="ebefe-147">Chrome 48,0...</span><span class="sxs-lookup"><span data-stu-id="ebefe-147">Chrome 48.0, ...</span></span> |
| <span data-ttu-id="ebefe-148">context.device.deviceModel</span><span class="sxs-lookup"><span data-stu-id="ebefe-148">context.device.deviceModel</span></span> |<span data-ttu-id="ebefe-149">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-149">string</span></span> | |
| <span data-ttu-id="ebefe-150">context.device.deviceName</span><span class="sxs-lookup"><span data-stu-id="ebefe-150">context.device.deviceName</span></span> |<span data-ttu-id="ebefe-151">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-151">string</span></span> | |
| <span data-ttu-id="ebefe-152">Context.Device.ID</span><span class="sxs-lookup"><span data-stu-id="ebefe-152">context.device.id</span></span> |<span data-ttu-id="ebefe-153">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-153">string</span></span> | |
| <span data-ttu-id="ebefe-154">Context.Device.Locale</span><span class="sxs-lookup"><span data-stu-id="ebefe-154">context.device.locale</span></span> |<span data-ttu-id="ebefe-155">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-155">string</span></span> |<span data-ttu-id="ebefe-156">en GB, de-DE...</span><span class="sxs-lookup"><span data-stu-id="ebefe-156">en-GB, de-DE, ...</span></span> |
| <span data-ttu-id="ebefe-157">Context.Device.Network</span><span class="sxs-lookup"><span data-stu-id="ebefe-157">context.device.network</span></span> |<span data-ttu-id="ebefe-158">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-158">string</span></span> | |
| <span data-ttu-id="ebefe-159">context.device.oemName</span><span class="sxs-lookup"><span data-stu-id="ebefe-159">context.device.oemName</span></span> |<span data-ttu-id="ebefe-160">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-160">string</span></span> | |
| <span data-ttu-id="ebefe-161">context.device.osVersion</span><span class="sxs-lookup"><span data-stu-id="ebefe-161">context.device.osVersion</span></span> |<span data-ttu-id="ebefe-162">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-162">string</span></span> |<span data-ttu-id="ebefe-163">Värdens operativsystem</span><span class="sxs-lookup"><span data-stu-id="ebefe-163">Host OS</span></span> |
| <span data-ttu-id="ebefe-164">context.device.roleInstance</span><span class="sxs-lookup"><span data-stu-id="ebefe-164">context.device.roleInstance</span></span> |<span data-ttu-id="ebefe-165">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-165">string</span></span> |<span data-ttu-id="ebefe-166">ID för server-värd</span><span class="sxs-lookup"><span data-stu-id="ebefe-166">ID of server host</span></span> |
| <span data-ttu-id="ebefe-167">context.device.roleName</span><span class="sxs-lookup"><span data-stu-id="ebefe-167">context.device.roleName</span></span> |<span data-ttu-id="ebefe-168">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-168">string</span></span> | |
| <span data-ttu-id="ebefe-169">Context.Device.Type</span><span class="sxs-lookup"><span data-stu-id="ebefe-169">context.device.type</span></span> |<span data-ttu-id="ebefe-170">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-170">string</span></span> |<span data-ttu-id="ebefe-171">PC webbläsare...</span><span class="sxs-lookup"><span data-stu-id="ebefe-171">PC, Browser, ...</span></span> |
| <span data-ttu-id="ebefe-172">Context.Location</span><span class="sxs-lookup"><span data-stu-id="ebefe-172">context.location</span></span> |<span data-ttu-id="ebefe-173">Objektet</span><span class="sxs-lookup"><span data-stu-id="ebefe-173">object</span></span> |<span data-ttu-id="ebefe-174">Härleds från clientip.</span><span class="sxs-lookup"><span data-stu-id="ebefe-174">Derived from clientip.</span></span> |
| <span data-ttu-id="ebefe-175">Context.location.City</span><span class="sxs-lookup"><span data-stu-id="ebefe-175">context.location.city</span></span> |<span data-ttu-id="ebefe-176">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-176">string</span></span> |<span data-ttu-id="ebefe-177">Om den är känd som härrör från clientip,</span><span class="sxs-lookup"><span data-stu-id="ebefe-177">Derived from clientip, if known</span></span> |
| <span data-ttu-id="ebefe-178">Context.location.clientip</span><span class="sxs-lookup"><span data-stu-id="ebefe-178">context.location.clientip</span></span> |<span data-ttu-id="ebefe-179">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-179">string</span></span> |<span data-ttu-id="ebefe-180">Senaste Åttahörning anonym till 0.</span><span class="sxs-lookup"><span data-stu-id="ebefe-180">Last octagon is anonymized to 0.</span></span> |
| <span data-ttu-id="ebefe-181">Context.location.continent</span><span class="sxs-lookup"><span data-stu-id="ebefe-181">context.location.continent</span></span> |<span data-ttu-id="ebefe-182">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-182">string</span></span> | |
| <span data-ttu-id="ebefe-183">Context.location.Country</span><span class="sxs-lookup"><span data-stu-id="ebefe-183">context.location.country</span></span> |<span data-ttu-id="ebefe-184">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-184">string</span></span> | |
| <span data-ttu-id="ebefe-185">Context.location.province</span><span class="sxs-lookup"><span data-stu-id="ebefe-185">context.location.province</span></span> |<span data-ttu-id="ebefe-186">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-186">string</span></span> |<span data-ttu-id="ebefe-187">Region</span><span class="sxs-lookup"><span data-stu-id="ebefe-187">State or province</span></span> |
| <span data-ttu-id="ebefe-188">Context.operation.ID</span><span class="sxs-lookup"><span data-stu-id="ebefe-188">context.operation.id</span></span> |<span data-ttu-id="ebefe-189">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-189">string</span></span> |<span data-ttu-id="ebefe-190">Objekt som har samma åtgärds-id visas som relaterade objekt i portalen.</span><span class="sxs-lookup"><span data-stu-id="ebefe-190">Items that have the same operation id are shown as Related Items in the portal.</span></span> <span data-ttu-id="ebefe-191">Vanligtvis begäran-id.</span><span class="sxs-lookup"><span data-stu-id="ebefe-191">Usually the request id.</span></span> |
| <span data-ttu-id="ebefe-192">Context.operation.Name</span><span class="sxs-lookup"><span data-stu-id="ebefe-192">context.operation.name</span></span> |<span data-ttu-id="ebefe-193">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-193">string</span></span> |<span data-ttu-id="ebefe-194">URL eller begäran</span><span class="sxs-lookup"><span data-stu-id="ebefe-194">url or request name</span></span> |
| <span data-ttu-id="ebefe-195">context.operation.parentId</span><span class="sxs-lookup"><span data-stu-id="ebefe-195">context.operation.parentId</span></span> |<span data-ttu-id="ebefe-196">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-196">string</span></span> |<span data-ttu-id="ebefe-197">Tillåter kapslade relaterade objekt.</span><span class="sxs-lookup"><span data-stu-id="ebefe-197">Allows nested related items.</span></span> |
| <span data-ttu-id="ebefe-198">Context.session.ID</span><span class="sxs-lookup"><span data-stu-id="ebefe-198">context.session.id</span></span> |<span data-ttu-id="ebefe-199">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-199">string</span></span> |<span data-ttu-id="ebefe-200">ID för en grupp av åtgärder från samma källa.</span><span class="sxs-lookup"><span data-stu-id="ebefe-200">Id of a group of operations from the same source.</span></span> <span data-ttu-id="ebefe-201">30 minuter utan en åtgärd signalerar till slutet av en session.</span><span class="sxs-lookup"><span data-stu-id="ebefe-201">A period of 30 minutes without an operation signals the end of a session.</span></span> |
| <span data-ttu-id="ebefe-202">context.session.isFirst</span><span class="sxs-lookup"><span data-stu-id="ebefe-202">context.session.isFirst</span></span> |<span data-ttu-id="ebefe-203">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="ebefe-203">boolean</span></span> | |
| <span data-ttu-id="ebefe-204">context.user.accountAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="ebefe-204">context.user.accountAcquisitionDate</span></span> |<span data-ttu-id="ebefe-205">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-205">string</span></span> | |
| <span data-ttu-id="ebefe-206">context.user.anonAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="ebefe-206">context.user.anonAcquisitionDate</span></span> |<span data-ttu-id="ebefe-207">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-207">string</span></span> | |
| <span data-ttu-id="ebefe-208">context.user.anonId</span><span class="sxs-lookup"><span data-stu-id="ebefe-208">context.user.anonId</span></span> |<span data-ttu-id="ebefe-209">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-209">string</span></span> | |
| <span data-ttu-id="ebefe-210">context.user.authAcquisitionDate</span><span class="sxs-lookup"><span data-stu-id="ebefe-210">context.user.authAcquisitionDate</span></span> |<span data-ttu-id="ebefe-211">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-211">string</span></span> |[<span data-ttu-id="ebefe-212">Autentiserad användare</span><span class="sxs-lookup"><span data-stu-id="ebefe-212">Authenticated User</span></span>](app-insights-api-custom-events-metrics.md#authenticated-users) |
| <span data-ttu-id="ebefe-213">context.user.isAuthenticated</span><span class="sxs-lookup"><span data-stu-id="ebefe-213">context.user.isAuthenticated</span></span> |<span data-ttu-id="ebefe-214">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="ebefe-214">boolean</span></span> | |
| <span data-ttu-id="ebefe-215">internal.data.documentVersion</span><span class="sxs-lookup"><span data-stu-id="ebefe-215">internal.data.documentVersion</span></span> |<span data-ttu-id="ebefe-216">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-216">string</span></span> | |
| <span data-ttu-id="ebefe-217">internal.data.ID</span><span class="sxs-lookup"><span data-stu-id="ebefe-217">internal.data.id</span></span> |<span data-ttu-id="ebefe-218">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-218">string</span></span> | |

## <a name="events"></a><span data-ttu-id="ebefe-219">Händelser</span><span class="sxs-lookup"><span data-stu-id="ebefe-219">Events</span></span>
<span data-ttu-id="ebefe-220">Anpassade händelser som genererats av [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="ebefe-220">Custom events generated by [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent).</span></span>

| <span data-ttu-id="ebefe-221">Sökväg</span><span class="sxs-lookup"><span data-stu-id="ebefe-221">Path</span></span> | <span data-ttu-id="ebefe-222">Typ</span><span class="sxs-lookup"><span data-stu-id="ebefe-222">Type</span></span> | <span data-ttu-id="ebefe-223">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-223">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebefe-224">händelseantal [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-224">event [0] count</span></span> |<span data-ttu-id="ebefe-225">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-225">integer</span></span> |<span data-ttu-id="ebefe-226">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="ebefe-226">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="ebefe-227">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="ebefe-227">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="ebefe-228">händelsenamn [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-228">event [0] name</span></span> |<span data-ttu-id="ebefe-229">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-229">string</span></span> |<span data-ttu-id="ebefe-230">Händelsenamn.</span><span class="sxs-lookup"><span data-stu-id="ebefe-230">Event name.</span></span>  <span data-ttu-id="ebefe-231">Maxlängd 250.</span><span class="sxs-lookup"><span data-stu-id="ebefe-231">Max length 250.</span></span> |
| <span data-ttu-id="ebefe-232">händelsen [0] url</span><span class="sxs-lookup"><span data-stu-id="ebefe-232">event [0] url</span></span> |<span data-ttu-id="ebefe-233">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-233">string</span></span> | |
| <span data-ttu-id="ebefe-234">händelsen [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="ebefe-234">event [0] urlData.base</span></span> |<span data-ttu-id="ebefe-235">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-235">string</span></span> | |
| <span data-ttu-id="ebefe-236">händelsen [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="ebefe-236">event [0] urlData.host</span></span> |<span data-ttu-id="ebefe-237">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-237">string</span></span> | |

## <a name="exceptions"></a><span data-ttu-id="ebefe-238">Undantag</span><span class="sxs-lookup"><span data-stu-id="ebefe-238">Exceptions</span></span>
<span data-ttu-id="ebefe-239">Rapporter [undantag](app-insights-asp-net-exceptions.md) på servern och i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ebefe-239">Reports [exceptions](app-insights-asp-net-exceptions.md) in the server and in the browser.</span></span>

| <span data-ttu-id="ebefe-240">Sökväg</span><span class="sxs-lookup"><span data-stu-id="ebefe-240">Path</span></span> | <span data-ttu-id="ebefe-241">Typ</span><span class="sxs-lookup"><span data-stu-id="ebefe-241">Type</span></span> | <span data-ttu-id="ebefe-242">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-242">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebefe-243">sammansättningen basicException [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-243">basicException [0] assembly</span></span> |<span data-ttu-id="ebefe-244">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-244">string</span></span> | |
| <span data-ttu-id="ebefe-245">Antal basicException [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-245">basicException [0] count</span></span> |<span data-ttu-id="ebefe-246">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-246">integer</span></span> |<span data-ttu-id="ebefe-247">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="ebefe-247">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="ebefe-248">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="ebefe-248">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="ebefe-249">basicException [0] exceptionGroup</span><span class="sxs-lookup"><span data-stu-id="ebefe-249">basicException [0] exceptionGroup</span></span> |<span data-ttu-id="ebefe-250">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-250">string</span></span> | |
| <span data-ttu-id="ebefe-251">basicException [0] exceptionType</span><span class="sxs-lookup"><span data-stu-id="ebefe-251">basicException [0] exceptionType</span></span> |<span data-ttu-id="ebefe-252">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-252">string</span></span> | |
| <span data-ttu-id="ebefe-253">basicException [0] failedUserCodeMethod</span><span class="sxs-lookup"><span data-stu-id="ebefe-253">basicException [0] failedUserCodeMethod</span></span> |<span data-ttu-id="ebefe-254">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-254">string</span></span> | |
| <span data-ttu-id="ebefe-255">basicException [0] failedUserCodeAssembly</span><span class="sxs-lookup"><span data-stu-id="ebefe-255">basicException [0] failedUserCodeAssembly</span></span> |<span data-ttu-id="ebefe-256">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-256">string</span></span> | |
| <span data-ttu-id="ebefe-257">basicException [0] handledAt</span><span class="sxs-lookup"><span data-stu-id="ebefe-257">basicException [0] handledAt</span></span> |<span data-ttu-id="ebefe-258">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-258">string</span></span> | |
| <span data-ttu-id="ebefe-259">basicException [0] hasFullStack</span><span class="sxs-lookup"><span data-stu-id="ebefe-259">basicException [0] hasFullStack</span></span> |<span data-ttu-id="ebefe-260">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="ebefe-260">boolean</span></span> | |
| <span data-ttu-id="ebefe-261">basicException [0]-id</span><span class="sxs-lookup"><span data-stu-id="ebefe-261">basicException [0] id</span></span> |<span data-ttu-id="ebefe-262">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-262">string</span></span> | |
| <span data-ttu-id="ebefe-263">metoden basicException [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-263">basicException [0] method</span></span> |<span data-ttu-id="ebefe-264">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-264">string</span></span> | |
| <span data-ttu-id="ebefe-265">basicException [0] meddelande</span><span class="sxs-lookup"><span data-stu-id="ebefe-265">basicException [0] message</span></span> |<span data-ttu-id="ebefe-266">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-266">string</span></span> |<span data-ttu-id="ebefe-267">Undantagsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="ebefe-267">Exception message.</span></span> <span data-ttu-id="ebefe-268">Maxlängd 10k.</span><span class="sxs-lookup"><span data-stu-id="ebefe-268">Max length 10k.</span></span> |
| <span data-ttu-id="ebefe-269">basicException [0] outerExceptionMessage</span><span class="sxs-lookup"><span data-stu-id="ebefe-269">basicException [0] outerExceptionMessage</span></span> |<span data-ttu-id="ebefe-270">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-270">string</span></span> | |
| <span data-ttu-id="ebefe-271">basicException [0] outerExceptionThrownAtAssembly</span><span class="sxs-lookup"><span data-stu-id="ebefe-271">basicException [0] outerExceptionThrownAtAssembly</span></span> |<span data-ttu-id="ebefe-272">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-272">string</span></span> | |
| <span data-ttu-id="ebefe-273">basicException [0] outerExceptionThrownAtMethod</span><span class="sxs-lookup"><span data-stu-id="ebefe-273">basicException [0] outerExceptionThrownAtMethod</span></span> |<span data-ttu-id="ebefe-274">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-274">string</span></span> | |
| <span data-ttu-id="ebefe-275">basicException [0] outerExceptionType</span><span class="sxs-lookup"><span data-stu-id="ebefe-275">basicException [0] outerExceptionType</span></span> |<span data-ttu-id="ebefe-276">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-276">string</span></span> | |
| <span data-ttu-id="ebefe-277">basicException [0] outerId</span><span class="sxs-lookup"><span data-stu-id="ebefe-277">basicException [0] outerId</span></span> |<span data-ttu-id="ebefe-278">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-278">string</span></span> | |
| <span data-ttu-id="ebefe-279">basicException [0] parsedStack [0] sammansättning</span><span class="sxs-lookup"><span data-stu-id="ebefe-279">basicException [0] parsedStack [0] assembly</span></span> |<span data-ttu-id="ebefe-280">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-280">string</span></span> | |
| <span data-ttu-id="ebefe-281">basicException [0] parsedStack [0] filnamn</span><span class="sxs-lookup"><span data-stu-id="ebefe-281">basicException [0] parsedStack [0] fileName</span></span> |<span data-ttu-id="ebefe-282">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-282">string</span></span> | |
| <span data-ttu-id="ebefe-283">basicException [0] parsedStack [0] nivå</span><span class="sxs-lookup"><span data-stu-id="ebefe-283">basicException [0] parsedStack [0] level</span></span> |<span data-ttu-id="ebefe-284">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-284">integer</span></span> | |
| <span data-ttu-id="ebefe-285">basicException [0] parsedStack [0] rad</span><span class="sxs-lookup"><span data-stu-id="ebefe-285">basicException [0] parsedStack [0] line</span></span> |<span data-ttu-id="ebefe-286">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-286">integer</span></span> | |
| <span data-ttu-id="ebefe-287">basicException [0] parsedStack [0] metod</span><span class="sxs-lookup"><span data-stu-id="ebefe-287">basicException [0] parsedStack [0] method</span></span> |<span data-ttu-id="ebefe-288">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-288">string</span></span> | |
| <span data-ttu-id="ebefe-289">basicException [0] stack</span><span class="sxs-lookup"><span data-stu-id="ebefe-289">basicException [0] stack</span></span> |<span data-ttu-id="ebefe-290">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-290">string</span></span> |<span data-ttu-id="ebefe-291">Maxlängd 10k</span><span class="sxs-lookup"><span data-stu-id="ebefe-291">Max length 10k</span></span> |
| <span data-ttu-id="ebefe-292">typeName basicException [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-292">basicException [0] typeName</span></span> |<span data-ttu-id="ebefe-293">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-293">string</span></span> | |

## <a name="trace-messages"></a><span data-ttu-id="ebefe-294">Spåra meddelanden</span><span class="sxs-lookup"><span data-stu-id="ebefe-294">Trace Messages</span></span>
<span data-ttu-id="ebefe-295">Skickas av [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace), och av de [loggning kort](app-insights-asp-net-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="ebefe-295">Sent by [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace), and by the [logging adapters](app-insights-asp-net-trace-logs.md).</span></span>

| <span data-ttu-id="ebefe-296">Sökväg</span><span class="sxs-lookup"><span data-stu-id="ebefe-296">Path</span></span> | <span data-ttu-id="ebefe-297">Typ</span><span class="sxs-lookup"><span data-stu-id="ebefe-297">Type</span></span> | <span data-ttu-id="ebefe-298">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-298">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebefe-299">meddelandet [0] loggningsnamn</span><span class="sxs-lookup"><span data-stu-id="ebefe-299">message [0] loggerName</span></span> |<span data-ttu-id="ebefe-300">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-300">string</span></span> | |
| <span data-ttu-id="ebefe-301">[0] meddelandeparametrar</span><span class="sxs-lookup"><span data-stu-id="ebefe-301">message [0] parameters</span></span> |<span data-ttu-id="ebefe-302">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-302">string</span></span> | |
| <span data-ttu-id="ebefe-303">meddelandet [0] rådata</span><span class="sxs-lookup"><span data-stu-id="ebefe-303">message [0] raw</span></span> |<span data-ttu-id="ebefe-304">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-304">string</span></span> |<span data-ttu-id="ebefe-305">Loggmeddelande får innehålla högst 10k.</span><span class="sxs-lookup"><span data-stu-id="ebefe-305">The log message, max length 10k.</span></span> |
| <span data-ttu-id="ebefe-306">meddelandet [0] severityLevel</span><span class="sxs-lookup"><span data-stu-id="ebefe-306">message [0] severityLevel</span></span> |<span data-ttu-id="ebefe-307">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-307">string</span></span> | |

## <a name="remote-dependency"></a><span data-ttu-id="ebefe-308">Fjärråtkomst beroende</span><span class="sxs-lookup"><span data-stu-id="ebefe-308">Remote dependency</span></span>
<span data-ttu-id="ebefe-309">Skickas av TrackDependency.</span><span class="sxs-lookup"><span data-stu-id="ebefe-309">Sent by TrackDependency.</span></span> <span data-ttu-id="ebefe-310">Används för att rapportprestanda och användning av [anrop till beroenden](app-insights-asp-net-dependencies.md) i server och AJAX-anrop i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ebefe-310">Used to report performance and usage of [calls to dependencies](app-insights-asp-net-dependencies.md) in the server, and AJAX calls in the browser.</span></span>

| <span data-ttu-id="ebefe-311">Sökväg</span><span class="sxs-lookup"><span data-stu-id="ebefe-311">Path</span></span> | <span data-ttu-id="ebefe-312">Typ</span><span class="sxs-lookup"><span data-stu-id="ebefe-312">Type</span></span> | <span data-ttu-id="ebefe-313">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-313">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebefe-314">asynkrona remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-314">remoteDependency [0] async</span></span> |<span data-ttu-id="ebefe-315">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="ebefe-315">boolean</span></span> | |
| <span data-ttu-id="ebefe-316">remoteDependency [0] baseName</span><span class="sxs-lookup"><span data-stu-id="ebefe-316">remoteDependency [0] baseName</span></span> |<span data-ttu-id="ebefe-317">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-317">string</span></span> | |
| <span data-ttu-id="ebefe-318">remoteDependency [0] commandName</span><span class="sxs-lookup"><span data-stu-id="ebefe-318">remoteDependency [0] commandName</span></span> |<span data-ttu-id="ebefe-319">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-319">string</span></span> |<span data-ttu-id="ebefe-320">Till exempel ”home/index”</span><span class="sxs-lookup"><span data-stu-id="ebefe-320">For example "home/index"</span></span> |
| <span data-ttu-id="ebefe-321">Antal remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-321">remoteDependency [0] count</span></span> |<span data-ttu-id="ebefe-322">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-322">integer</span></span> |<span data-ttu-id="ebefe-323">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="ebefe-323">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="ebefe-324">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="ebefe-324">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="ebefe-325">remoteDependency [0] dependencyTypeName</span><span class="sxs-lookup"><span data-stu-id="ebefe-325">remoteDependency [0] dependencyTypeName</span></span> |<span data-ttu-id="ebefe-326">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-326">string</span></span> |<span data-ttu-id="ebefe-327">HTTP, SQL...</span><span class="sxs-lookup"><span data-stu-id="ebefe-327">HTTP, SQL, ...</span></span> |
| <span data-ttu-id="ebefe-328">remoteDependency [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-328">remoteDependency [0] durationMetric.value</span></span> |<span data-ttu-id="ebefe-329">Antal</span><span class="sxs-lookup"><span data-stu-id="ebefe-329">number</span></span> |<span data-ttu-id="ebefe-330">Tid vid anrop till svaret av beroende har slutförts</span><span class="sxs-lookup"><span data-stu-id="ebefe-330">Time from call to completion of response by dependency</span></span> |
| <span data-ttu-id="ebefe-331">remoteDependency [0]-id</span><span class="sxs-lookup"><span data-stu-id="ebefe-331">remoteDependency [0] id</span></span> |<span data-ttu-id="ebefe-332">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-332">string</span></span> | |
| <span data-ttu-id="ebefe-333">remoteDependency [0] namn</span><span class="sxs-lookup"><span data-stu-id="ebefe-333">remoteDependency [0] name</span></span> |<span data-ttu-id="ebefe-334">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-334">string</span></span> |<span data-ttu-id="ebefe-335">URL-adressen.</span><span class="sxs-lookup"><span data-stu-id="ebefe-335">Url.</span></span> <span data-ttu-id="ebefe-336">Maxlängd 250.</span><span class="sxs-lookup"><span data-stu-id="ebefe-336">Max length 250.</span></span> |
| <span data-ttu-id="ebefe-337">remoteDependency [0] resultCode</span><span class="sxs-lookup"><span data-stu-id="ebefe-337">remoteDependency [0] resultCode</span></span> |<span data-ttu-id="ebefe-338">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-338">string</span></span> |<span data-ttu-id="ebefe-339">från HTTP-beroendet</span><span class="sxs-lookup"><span data-stu-id="ebefe-339">from HTTP dependency</span></span> |
| <span data-ttu-id="ebefe-340">remoteDependency [0] lyckades</span><span class="sxs-lookup"><span data-stu-id="ebefe-340">remoteDependency [0] success</span></span> |<span data-ttu-id="ebefe-341">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="ebefe-341">boolean</span></span> | |
| <span data-ttu-id="ebefe-342">typ av remoteDependency [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-342">remoteDependency [0] type</span></span> |<span data-ttu-id="ebefe-343">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-343">string</span></span> |<span data-ttu-id="ebefe-344">HTTP, Sql...</span><span class="sxs-lookup"><span data-stu-id="ebefe-344">Http, Sql,...</span></span> |
| <span data-ttu-id="ebefe-345">remoteDependency [0] url</span><span class="sxs-lookup"><span data-stu-id="ebefe-345">remoteDependency [0] url</span></span> |<span data-ttu-id="ebefe-346">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-346">string</span></span> |<span data-ttu-id="ebefe-347">Maxlängd 2000</span><span class="sxs-lookup"><span data-stu-id="ebefe-347">Max length 2000</span></span> |
| <span data-ttu-id="ebefe-348">remoteDependency [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="ebefe-348">remoteDependency [0] urlData.base</span></span> |<span data-ttu-id="ebefe-349">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-349">string</span></span> |<span data-ttu-id="ebefe-350">Maxlängd 2000</span><span class="sxs-lookup"><span data-stu-id="ebefe-350">Max length 2000</span></span> |
| <span data-ttu-id="ebefe-351">remoteDependency [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="ebefe-351">remoteDependency [0] urlData.hashTag</span></span> |<span data-ttu-id="ebefe-352">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-352">string</span></span> | |
| <span data-ttu-id="ebefe-353">remoteDependency [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="ebefe-353">remoteDependency [0] urlData.host</span></span> |<span data-ttu-id="ebefe-354">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-354">string</span></span> |<span data-ttu-id="ebefe-355">Maxlängd 200</span><span class="sxs-lookup"><span data-stu-id="ebefe-355">Max length 200</span></span> |

## <a name="requests"></a><span data-ttu-id="ebefe-356">Begäranden</span><span class="sxs-lookup"><span data-stu-id="ebefe-356">Requests</span></span>
<span data-ttu-id="ebefe-357">Skickas av [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest).</span><span class="sxs-lookup"><span data-stu-id="ebefe-357">Sent by [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest).</span></span> <span data-ttu-id="ebefe-358">Modulerna som standard används för att rapporter serversvarstid, mätt på servern.</span><span class="sxs-lookup"><span data-stu-id="ebefe-358">The standard modules use this to reports server response time, measured at the server.</span></span>

| <span data-ttu-id="ebefe-359">Sökväg</span><span class="sxs-lookup"><span data-stu-id="ebefe-359">Path</span></span> | <span data-ttu-id="ebefe-360">Typ</span><span class="sxs-lookup"><span data-stu-id="ebefe-360">Type</span></span> | <span data-ttu-id="ebefe-361">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-361">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebefe-362">antalet begäranden [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-362">request [0] count</span></span> |<span data-ttu-id="ebefe-363">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-363">integer</span></span> |<span data-ttu-id="ebefe-364">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="ebefe-364">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="ebefe-365">Till exempel: 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="ebefe-365">For example: 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="ebefe-366">begäran [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-366">request [0] durationMetric.value</span></span> |<span data-ttu-id="ebefe-367">Antal</span><span class="sxs-lookup"><span data-stu-id="ebefe-367">number</span></span> |<span data-ttu-id="ebefe-368">Tid för begäran som inkommer till svar.</span><span class="sxs-lookup"><span data-stu-id="ebefe-368">Time from request arriving to response.</span></span> <span data-ttu-id="ebefe-369">1e7 == 1s</span><span class="sxs-lookup"><span data-stu-id="ebefe-369">1e7 == 1s</span></span> |
| <span data-ttu-id="ebefe-370">id för förfrågan [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-370">request [0] id</span></span> |<span data-ttu-id="ebefe-371">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-371">string</span></span> |<span data-ttu-id="ebefe-372">Åtgärds-id</span><span class="sxs-lookup"><span data-stu-id="ebefe-372">Operation id</span></span> |
| <span data-ttu-id="ebefe-373">namn på förfrågan [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-373">request [0] name</span></span> |<span data-ttu-id="ebefe-374">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-374">string</span></span> |<span data-ttu-id="ebefe-375">GET/POST + bas-url.</span><span class="sxs-lookup"><span data-stu-id="ebefe-375">GET/POST + url base.</span></span>  <span data-ttu-id="ebefe-376">Maxlängd 250</span><span class="sxs-lookup"><span data-stu-id="ebefe-376">Max length 250</span></span> |
| <span data-ttu-id="ebefe-377">begäran [0] responseCode</span><span class="sxs-lookup"><span data-stu-id="ebefe-377">request [0] responseCode</span></span> |<span data-ttu-id="ebefe-378">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-378">integer</span></span> |<span data-ttu-id="ebefe-379">HTTP-svar som skickats till klienten</span><span class="sxs-lookup"><span data-stu-id="ebefe-379">HTTP response sent to client</span></span> |
| <span data-ttu-id="ebefe-380">begäran [0] lyckades</span><span class="sxs-lookup"><span data-stu-id="ebefe-380">request [0] success</span></span> |<span data-ttu-id="ebefe-381">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="ebefe-381">boolean</span></span> |<span data-ttu-id="ebefe-382">Standard == (responseCode &lt; 400)</span><span class="sxs-lookup"><span data-stu-id="ebefe-382">Default == (responseCode &lt; 400)</span></span> |
| <span data-ttu-id="ebefe-383">url-begäran [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-383">request [0] url</span></span> |<span data-ttu-id="ebefe-384">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-384">string</span></span> |<span data-ttu-id="ebefe-385">Inte inklusive värden</span><span class="sxs-lookup"><span data-stu-id="ebefe-385">Not including host</span></span> |
| <span data-ttu-id="ebefe-386">begäran [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="ebefe-386">request [0] urlData.base</span></span> |<span data-ttu-id="ebefe-387">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-387">string</span></span> | |
| <span data-ttu-id="ebefe-388">begäran [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="ebefe-388">request [0] urlData.hashTag</span></span> |<span data-ttu-id="ebefe-389">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-389">string</span></span> | |
| <span data-ttu-id="ebefe-390">begäran [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="ebefe-390">request [0] urlData.host</span></span> |<span data-ttu-id="ebefe-391">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-391">string</span></span> | |

## <a name="page-view-performance"></a><span data-ttu-id="ebefe-392">Vyn sida prestanda</span><span class="sxs-lookup"><span data-stu-id="ebefe-392">Page View Performance</span></span>
<span data-ttu-id="ebefe-393">Skickas av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ebefe-393">Sent by the browser.</span></span> <span data-ttu-id="ebefe-394">Mäter tid att bearbeta en sida från användare initierar en begäran om att visa fullständiga (exklusive asynkrona AJAX-anrop).</span><span class="sxs-lookup"><span data-stu-id="ebefe-394">Measures the time to process a page, from user initiating the request to display complete (excluding async AJAX calls).</span></span>

<span data-ttu-id="ebefe-395">Kontexten värden visa klientens operativsystem och version på webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="ebefe-395">Context values show client OS and browser version.</span></span>

| <span data-ttu-id="ebefe-396">Sökväg</span><span class="sxs-lookup"><span data-stu-id="ebefe-396">Path</span></span> | <span data-ttu-id="ebefe-397">Typ</span><span class="sxs-lookup"><span data-stu-id="ebefe-397">Type</span></span> | <span data-ttu-id="ebefe-398">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-398">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebefe-399">clientPerformance [0] clientProcess.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-399">clientPerformance [0] clientProcess.value</span></span> |<span data-ttu-id="ebefe-400">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-400">integer</span></span> |<span data-ttu-id="ebefe-401">Tiden från slutet av tar emot HTML för att visa sidan.</span><span class="sxs-lookup"><span data-stu-id="ebefe-401">Time from end of receiving the HTML to displaying the page.</span></span> |
| <span data-ttu-id="ebefe-402">clientPerformance [0] namn</span><span class="sxs-lookup"><span data-stu-id="ebefe-402">clientPerformance [0] name</span></span> |<span data-ttu-id="ebefe-403">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-403">string</span></span> | |
| <span data-ttu-id="ebefe-404">clientPerformance [0] networkConnection.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-404">clientPerformance [0] networkConnection.value</span></span> |<span data-ttu-id="ebefe-405">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-405">integer</span></span> |<span data-ttu-id="ebefe-406">Åtgången tid för att upprätta en nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="ebefe-406">Time taken to establish a network connection.</span></span> |
| <span data-ttu-id="ebefe-407">clientPerformance [0] receiveRequest.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-407">clientPerformance [0] receiveRequest.value</span></span> |<span data-ttu-id="ebefe-408">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-408">integer</span></span> |<span data-ttu-id="ebefe-409">Tiden från slutet av begäran skickades till mottagning HTML i svar.</span><span class="sxs-lookup"><span data-stu-id="ebefe-409">Time from end of sending the request to receiving the HTML in reply.</span></span> |
| <span data-ttu-id="ebefe-410">clientPerformance [0] sendRequest.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-410">clientPerformance [0] sendRequest.value</span></span> |<span data-ttu-id="ebefe-411">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-411">integer</span></span> |<span data-ttu-id="ebefe-412">Tiden från att skicka HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="ebefe-412">Time from taken to send the HTTP request.</span></span> |
| <span data-ttu-id="ebefe-413">clientPerformance [0] total.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-413">clientPerformance [0] total.value</span></span> |<span data-ttu-id="ebefe-414">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-414">integer</span></span> |<span data-ttu-id="ebefe-415">Tiden mellan start-och skicka begäran till visar sidan.</span><span class="sxs-lookup"><span data-stu-id="ebefe-415">Time from starting to send the request to displaying the page.</span></span> |
| <span data-ttu-id="ebefe-416">clientPerformance [0] url</span><span class="sxs-lookup"><span data-stu-id="ebefe-416">clientPerformance [0] url</span></span> |<span data-ttu-id="ebefe-417">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-417">string</span></span> |<span data-ttu-id="ebefe-418">URL för den här begäran</span><span class="sxs-lookup"><span data-stu-id="ebefe-418">URL of this request</span></span> |
| <span data-ttu-id="ebefe-419">clientPerformance [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="ebefe-419">clientPerformance [0] urlData.base</span></span> |<span data-ttu-id="ebefe-420">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-420">string</span></span> | |
| <span data-ttu-id="ebefe-421">clientPerformance [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="ebefe-421">clientPerformance [0] urlData.hashTag</span></span> |<span data-ttu-id="ebefe-422">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-422">string</span></span> | |
| <span data-ttu-id="ebefe-423">clientPerformance [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="ebefe-423">clientPerformance [0] urlData.host</span></span> |<span data-ttu-id="ebefe-424">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-424">string</span></span> | |
| <span data-ttu-id="ebefe-425">clientPerformance [0] urlData.protocol</span><span class="sxs-lookup"><span data-stu-id="ebefe-425">clientPerformance [0] urlData.protocol</span></span> |<span data-ttu-id="ebefe-426">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-426">string</span></span> | |

## <a name="page-views"></a><span data-ttu-id="ebefe-427">Sidvisningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-427">Page Views</span></span>
<span data-ttu-id="ebefe-428">Skickas av trackPageView() eller [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)</span><span class="sxs-lookup"><span data-stu-id="ebefe-428">Sent by trackPageView() or [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)</span></span>

| <span data-ttu-id="ebefe-429">Sökväg</span><span class="sxs-lookup"><span data-stu-id="ebefe-429">Path</span></span> | <span data-ttu-id="ebefe-430">Typ</span><span class="sxs-lookup"><span data-stu-id="ebefe-430">Type</span></span> | <span data-ttu-id="ebefe-431">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-431">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebefe-432">Visa [0] antal</span><span class="sxs-lookup"><span data-stu-id="ebefe-432">view [0] count</span></span> |<span data-ttu-id="ebefe-433">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-433">integer</span></span> |<span data-ttu-id="ebefe-434">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="ebefe-434">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="ebefe-435">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="ebefe-435">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="ebefe-436">Visa [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-436">view [0] durationMetric.value</span></span> |<span data-ttu-id="ebefe-437">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-437">integer</span></span> |<span data-ttu-id="ebefe-438">Värdet som du kan också ställa in i trackPageView() eller startTrackPage() - stopTrackPage().</span><span class="sxs-lookup"><span data-stu-id="ebefe-438">Value optionally set in trackPageView() or by startTrackPage() - stopTrackPage().</span></span> <span data-ttu-id="ebefe-439">Inte samma som clientPerformance värden.</span><span class="sxs-lookup"><span data-stu-id="ebefe-439">Not the same as clientPerformance values.</span></span> |
| <span data-ttu-id="ebefe-440">vynamn [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-440">view [0] name</span></span> |<span data-ttu-id="ebefe-441">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-441">string</span></span> |<span data-ttu-id="ebefe-442">Rubrik.</span><span class="sxs-lookup"><span data-stu-id="ebefe-442">Page title.</span></span>  <span data-ttu-id="ebefe-443">Maxlängd 250</span><span class="sxs-lookup"><span data-stu-id="ebefe-443">Max length 250</span></span> |
| <span data-ttu-id="ebefe-444">Visa [0] url</span><span class="sxs-lookup"><span data-stu-id="ebefe-444">view [0] url</span></span> |<span data-ttu-id="ebefe-445">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-445">string</span></span> | |
| <span data-ttu-id="ebefe-446">Visa [0] urlData.base</span><span class="sxs-lookup"><span data-stu-id="ebefe-446">view [0] urlData.base</span></span> |<span data-ttu-id="ebefe-447">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-447">string</span></span> | |
| <span data-ttu-id="ebefe-448">Visa [0] urlData.hashTag</span><span class="sxs-lookup"><span data-stu-id="ebefe-448">view [0] urlData.hashTag</span></span> |<span data-ttu-id="ebefe-449">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-449">string</span></span> | |
| <span data-ttu-id="ebefe-450">Visa [0] urlData.host</span><span class="sxs-lookup"><span data-stu-id="ebefe-450">view [0] urlData.host</span></span> |<span data-ttu-id="ebefe-451">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-451">string</span></span> | |

## <a name="availability"></a><span data-ttu-id="ebefe-452">Tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="ebefe-452">Availability</span></span>
<span data-ttu-id="ebefe-453">Rapporter [tillgänglighet webbtester](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="ebefe-453">Reports [availability web tests](app-insights-monitor-web-app-availability.md).</span></span>

| <span data-ttu-id="ebefe-454">Sökväg</span><span class="sxs-lookup"><span data-stu-id="ebefe-454">Path</span></span> | <span data-ttu-id="ebefe-455">Typ</span><span class="sxs-lookup"><span data-stu-id="ebefe-455">Type</span></span> | <span data-ttu-id="ebefe-456">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ebefe-456">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebefe-457">tillgänglighet [0] availabilityMetric.name</span><span class="sxs-lookup"><span data-stu-id="ebefe-457">availability [0] availabilityMetric.name</span></span> |<span data-ttu-id="ebefe-458">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-458">string</span></span> |<span data-ttu-id="ebefe-459">availability</span><span class="sxs-lookup"><span data-stu-id="ebefe-459">availability</span></span> |
| <span data-ttu-id="ebefe-460">tillgänglighet [0] availabilityMetric.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-460">availability [0] availabilityMetric.value</span></span> |<span data-ttu-id="ebefe-461">Antal</span><span class="sxs-lookup"><span data-stu-id="ebefe-461">number</span></span> |<span data-ttu-id="ebefe-462">1.0 eller 0,0</span><span class="sxs-lookup"><span data-stu-id="ebefe-462">1.0 or 0.0</span></span> |
| <span data-ttu-id="ebefe-463">Antal tillgänglighet [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-463">availability [0] count</span></span> |<span data-ttu-id="ebefe-464">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-464">integer</span></span> |<span data-ttu-id="ebefe-465">100 / ([provtagning](app-insights-sampling.md) hastighet).</span><span class="sxs-lookup"><span data-stu-id="ebefe-465">100/([sampling](app-insights-sampling.md) rate).</span></span> <span data-ttu-id="ebefe-466">Till exempel 4 =&gt; 25%.</span><span class="sxs-lookup"><span data-stu-id="ebefe-466">For example 4 =&gt; 25%.</span></span> |
| <span data-ttu-id="ebefe-467">tillgänglighet [0] dataSizeMetric.name</span><span class="sxs-lookup"><span data-stu-id="ebefe-467">availability [0] dataSizeMetric.name</span></span> |<span data-ttu-id="ebefe-468">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-468">string</span></span> | |
| <span data-ttu-id="ebefe-469">tillgänglighet [0] dataSizeMetric.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-469">availability [0] dataSizeMetric.value</span></span> |<span data-ttu-id="ebefe-470">heltal</span><span class="sxs-lookup"><span data-stu-id="ebefe-470">integer</span></span> | |
| <span data-ttu-id="ebefe-471">tillgänglighet [0] durationMetric.name</span><span class="sxs-lookup"><span data-stu-id="ebefe-471">availability [0] durationMetric.name</span></span> |<span data-ttu-id="ebefe-472">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-472">string</span></span> | |
| <span data-ttu-id="ebefe-473">tillgänglighet [0] durationMetric.value</span><span class="sxs-lookup"><span data-stu-id="ebefe-473">availability [0] durationMetric.value</span></span> |<span data-ttu-id="ebefe-474">Antal</span><span class="sxs-lookup"><span data-stu-id="ebefe-474">number</span></span> |<span data-ttu-id="ebefe-475">Varaktighet för testet.</span><span class="sxs-lookup"><span data-stu-id="ebefe-475">Duration of test.</span></span> <span data-ttu-id="ebefe-476">1e7 == 1s</span><span class="sxs-lookup"><span data-stu-id="ebefe-476">1e7==1s</span></span> |
| <span data-ttu-id="ebefe-477">meddelande om tillgänglighet [0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-477">availability [0] message</span></span> |<span data-ttu-id="ebefe-478">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-478">string</span></span> |<span data-ttu-id="ebefe-479">Fel diagnostik</span><span class="sxs-lookup"><span data-stu-id="ebefe-479">Failure diagnostic</span></span> |
| <span data-ttu-id="ebefe-480">tillgänglighet [0] resultat</span><span class="sxs-lookup"><span data-stu-id="ebefe-480">availability [0] result</span></span> |<span data-ttu-id="ebefe-481">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-481">string</span></span> |<span data-ttu-id="ebefe-482">Eller inte</span><span class="sxs-lookup"><span data-stu-id="ebefe-482">Pass/Fail</span></span> |
| <span data-ttu-id="ebefe-483">tillgänglighet [0] runLocation</span><span class="sxs-lookup"><span data-stu-id="ebefe-483">availability [0] runLocation</span></span> |<span data-ttu-id="ebefe-484">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-484">string</span></span> |<span data-ttu-id="ebefe-485">GEO-källan för HTTP-begäranden</span><span class="sxs-lookup"><span data-stu-id="ebefe-485">Geo source of http req</span></span> |
| <span data-ttu-id="ebefe-486">tillgänglighet [0] testName</span><span class="sxs-lookup"><span data-stu-id="ebefe-486">availability [0] testName</span></span> |<span data-ttu-id="ebefe-487">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-487">string</span></span> | |
| <span data-ttu-id="ebefe-488">tillgänglighet [0] testRunId</span><span class="sxs-lookup"><span data-stu-id="ebefe-488">availability [0] testRunId</span></span> |<span data-ttu-id="ebefe-489">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-489">string</span></span> | |
| <span data-ttu-id="ebefe-490">tillgänglighet [0] testTimestamp</span><span class="sxs-lookup"><span data-stu-id="ebefe-490">availability [0] testTimestamp</span></span> |<span data-ttu-id="ebefe-491">Sträng</span><span class="sxs-lookup"><span data-stu-id="ebefe-491">string</span></span> | |

## <a name="metrics"></a><span data-ttu-id="ebefe-492">Mått</span><span class="sxs-lookup"><span data-stu-id="ebefe-492">Metrics</span></span>
<span data-ttu-id="ebefe-493">Genereras av TrackMetric().</span><span class="sxs-lookup"><span data-stu-id="ebefe-493">Generated by TrackMetric().</span></span>

<span data-ttu-id="ebefe-494">Måttet finns i context.custom.metrics[0]</span><span class="sxs-lookup"><span data-stu-id="ebefe-494">The metric value is found in context.custom.metrics[0]</span></span>

<span data-ttu-id="ebefe-495">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ebefe-495">For example:</span></span>

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

## <a name="about-metric-values"></a><span data-ttu-id="ebefe-496">Om måttvärden</span><span class="sxs-lookup"><span data-stu-id="ebefe-496">About metric values</span></span>
<span data-ttu-id="ebefe-497">Måttvärden, både i mått rapporter och på andra platser, rapporteras med en standard objektstruktur.</span><span class="sxs-lookup"><span data-stu-id="ebefe-497">Metric values, both in metric reports and elsewhere, are reported with a standard object structure.</span></span> <span data-ttu-id="ebefe-498">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ebefe-498">For example:</span></span>

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

<span data-ttu-id="ebefe-499">För närvarande - men detta kan ändras i framtiden – alla värden som rapporterats från standard SDK-moduler, `count==1` och endast de `name` och `value` fälten är användbara.</span><span class="sxs-lookup"><span data-stu-id="ebefe-499">Currently - though this might change in the future - in all values reported from the standard SDK modules, `count==1` and only the `name` and `value` fields are useful.</span></span> <span data-ttu-id="ebefe-500">Det enda fallet där de skulle vara olika skulle vara om du skriver TrackMetric anrop i som du ange andra parametrar.</span><span class="sxs-lookup"><span data-stu-id="ebefe-500">The only case where they would be different would be if you write your own TrackMetric calls in which you set the other parameters.</span></span>

<span data-ttu-id="ebefe-501">Syftet med de andra fälten är att mått ska aggregeras i SDK, för att minska trafiken till portalen.</span><span class="sxs-lookup"><span data-stu-id="ebefe-501">The purpose of the other fields is to allow metrics to be aggregated in the SDK, to reduce traffic to the portal.</span></span> <span data-ttu-id="ebefe-502">Du kan till exempel genomsnittlig flera efterföljande avläsningar innan du skickar varje mått rapport.</span><span class="sxs-lookup"><span data-stu-id="ebefe-502">For example, you could average several successive readings before sending each metric report.</span></span> <span data-ttu-id="ebefe-503">Du skulle sedan beräkna min, max, standardavvikelse och samlat värde (sum eller medelvärde) och ange antal för antal värden som representeras av rapporten.</span><span class="sxs-lookup"><span data-stu-id="ebefe-503">Then you would calculate the min, max, standard deviation and aggregate value (sum or average) and set count to the number of readings represented by the report.</span></span>

<span data-ttu-id="ebefe-504">Vi har utelämnats används sällan fält count, min, max, stdDev och sampledValue i tabellerna ovan.</span><span class="sxs-lookup"><span data-stu-id="ebefe-504">In the tables above, we have omitted the rarely-used fields count, min, max, stdDev and sampledValue.</span></span>

<span data-ttu-id="ebefe-505">I stället före sammanställa statistik och, du kan använda [provtagning](app-insights-sampling.md) om du vill minska mängden telemetri.</span><span class="sxs-lookup"><span data-stu-id="ebefe-505">Instead of pre-aggregating metrics, you can use [sampling](app-insights-sampling.md) if you need to reduce the volume of telemetry.</span></span>

### <a name="durations"></a><span data-ttu-id="ebefe-506">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="ebefe-506">Durations</span></span>
<span data-ttu-id="ebefe-507">Förutom där annat anges, representeras varaktighet i tiondels mikrosekundnivå, så att 10000000.0 innebär 1 sekund.</span><span class="sxs-lookup"><span data-stu-id="ebefe-507">Except where otherwise noted, durations are represented in tenths of a microsecond, so that 10000000.0 means 1 second.</span></span>

## <a name="see-also"></a><span data-ttu-id="ebefe-508">Se även</span><span class="sxs-lookup"><span data-stu-id="ebefe-508">See also</span></span>
* [<span data-ttu-id="ebefe-509">Application Insights</span><span class="sxs-lookup"><span data-stu-id="ebefe-509">Application Insights</span></span>](app-insights-overview.md)
* [<span data-ttu-id="ebefe-510">Löpande Export</span><span class="sxs-lookup"><span data-stu-id="ebefe-510">Continuous Export</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="ebefe-511">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="ebefe-511">Code samples</span></span>](app-insights-export-telemetry.md#code-samples)

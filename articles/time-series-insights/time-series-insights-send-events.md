---
title: "aaaSend händelser tooAzure tid serien insikter miljö | Microsoft Docs"
description: "Den här kursen ingår hello steg toopush händelser tooyour tid serien insikter miljö"
keywords: 
services: tsi
documentationcenter: 
author: venkatgct
manager: jhubbard
editor: 
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: venkatja
ms.openlocfilehash: dbccc23f61351a0033cd48c1a02fb3841b45d560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooa-time-series-insights-environment-using-event-hub"></a><span data-ttu-id="dec07-103">Skicka händelser tooa tid serien insikter miljö med händelsehubb</span><span class="sxs-lookup"><span data-stu-id="dec07-103">Send events tooa Time Series Insights environment using event hub</span></span>

<span data-ttu-id="dec07-104">Den här självstudiekursen beskrivs hur toocreate och konfigurera händelsehubb och kör ett exempel programmet toopush händelser.</span><span class="sxs-lookup"><span data-stu-id="dec07-104">This tutorial explains how toocreate and configure event hub and run a sample application toopush events.</span></span> <span data-ttu-id="dec07-105">Om du har en befintlig händelsehubb som innehåller händelser i JSON-format kan du hoppa över den här självstudien och visa din miljö i [time series insights](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dec07-105">If you have an existing event hub with events in JSON format, skip this tutorial and view your environment in [time series insights](https://insights.timeseries.azure.com).</span></span>

## <a name="configure-an-event-hub"></a><span data-ttu-id="dec07-106">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="dec07-106">Configure an event hub</span></span>
1. <span data-ttu-id="dec07-107">toocreate en händelsehubb, följ instruktionerna från hello Event Hub [dokumentationen](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).</span><span class="sxs-lookup"><span data-stu-id="dec07-107">toocreate an event hub, follow instructions from hello Event Hub [documentation](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).</span></span>

2. <span data-ttu-id="dec07-108">Se till att skapa en konsumentgrupp som enbart används av din Time Series Insights-händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="dec07-108">Make sure you create a consumer group that is used exclusively by your Time Series Insights event source.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="dec07-109">Kontrollera att den här konsumentgruppen inte används av andra tjänster (t.ex Stream Analytics-jobb eller en annan Time Series Insights-miljö).</span><span class="sxs-lookup"><span data-stu-id="dec07-109">Make sure this consumer group is not used by any other service (such as Stream Analytics job or another Time Series Insights environment).</span></span> <span data-ttu-id="dec07-110">Om konsumentgrupp används av andra tjänster, Läs åtgärden påverkas negativt för den här miljön och hello andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="dec07-110">If consumer group is used by other services, read operation is negatively affected for this environment and hello other services.</span></span> <span data-ttu-id="dec07-111">Om du använder ”$Default” som hello konsumentgrupp leda toopotential återanvändning av andra läsare.</span><span class="sxs-lookup"><span data-stu-id="dec07-111">If you are using “$Default” as hello consumer group, it could lead toopotential reuse by other readers.</span></span>

  ![Välja konsumentgrupp för Event Hub](media/send-events/consumer-group.png)

3. <span data-ttu-id="dec07-113">Skapa ”MySendPolicy” på hello händelsehubb, som används toosend händelser i hello csharp exemplet.</span><span class="sxs-lookup"><span data-stu-id="dec07-113">On hello event hub, create “MySendPolicy” that is used toosend events in hello csharp sample.</span></span>

  ![Välj Policyer för delad åtkomst och klicka på knappen Lägg till](media/send-events/shared-access-policy.png)  

  ![Lägg till ny policy för delad åtkomst](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a><span data-ttu-id="dec07-116">Skapa händelsekälla för Time Series Insights</span><span class="sxs-lookup"><span data-stu-id="dec07-116">Create Time Series Insights event source</span></span>
1. <span data-ttu-id="dec07-117">Om du inte har skapat en händelsekälla följer [instruktionerna](time-series-insights-add-event-source.md) toocreate en händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="dec07-117">If you haven't created an event source, follow [these instructions](time-series-insights-add-event-source.md) toocreate an event source.</span></span>

2. <span data-ttu-id="dec07-118">Ange ”deviceTimestamp” som hello tidsstämpel egenskapsnamn – den här egenskapen används som hello faktiska tidsstämpel i hello csharp exemplet.</span><span class="sxs-lookup"><span data-stu-id="dec07-118">Specify “deviceTimestamp” as hello timestamp property name – this property is used as hello actual timestamp in hello csharp sample.</span></span> <span data-ttu-id="dec07-119">egenskapsnamn för hello tidsstämpel är skiftlägeskänsligt och värden måste följa hello format __åååå-MM-ddTHH. FFFFFFFK__ när de skickas som JSON tooevent hubb.</span><span class="sxs-lookup"><span data-stu-id="dec07-119">hello timestamp property name is case-sensitive and values must follow hello format __yyyy-MM-ddTHH:mm:ss.FFFFFFFK__ when sent as JSON tooevent hub.</span></span> <span data-ttu-id="dec07-120">Om hello egenskapen inte finns i hello händelse, används hello händelsetid hubb köas.</span><span class="sxs-lookup"><span data-stu-id="dec07-120">If hello property does not exist in hello event, then hello event hub enqueued time is used.</span></span>

  ![Skapa händelsekälla](media/send-events/event-source-1.png)

## <a name="sample-code-toopush-events"></a><span data-ttu-id="dec07-122">Exempel kod toopush händelser</span><span class="sxs-lookup"><span data-stu-id="dec07-122">Sample code toopush events</span></span>
1. <span data-ttu-id="dec07-123">Gå toohello event hub principen ”MySendPolicy” och kopiera anslutningssträngen hello med hello principnyckel.</span><span class="sxs-lookup"><span data-stu-id="dec07-123">Go toohello event hub policy “MySendPolicy” and copy hello connection string with hello policy key.</span></span>

  ![Kopiera anslutningssträngen MySendPolicy](media/send-events/sample-code-connection-string.png)

2. <span data-ttu-id="dec07-125">Kör följande kod hello som toosend 600 händelser per var och en av hello tre enheter.</span><span class="sxs-lookup"><span data-stu-id="dec07-125">Run hello following code that toosend 600 events per each of hello three devices.</span></span> <span data-ttu-id="dec07-126">Uppdatera `eventHubConnectionString` med anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="dec07-126">Update `eventHubConnectionString` with your connection string.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON tooevent hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a><span data-ttu-id="dec07-127">JSON-former som stöds</span><span class="sxs-lookup"><span data-stu-id="dec07-127">Supported JSON shapes</span></span>
### <a name="sample-1"></a><span data-ttu-id="dec07-128">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="dec07-128">Sample 1</span></span>

#### <a name="input"></a><span data-ttu-id="dec07-129">Indata</span><span class="sxs-lookup"><span data-stu-id="dec07-129">Input</span></span>

<span data-ttu-id="dec07-130">Ett enda JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="dec07-130">A simple JSON object.</span></span>

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a><span data-ttu-id="dec07-131">Resultat – 1 händelse</span><span class="sxs-lookup"><span data-stu-id="dec07-131">Output - 1 event</span></span>

|<span data-ttu-id="dec07-132">id</span><span class="sxs-lookup"><span data-stu-id="dec07-132">id</span></span>|<span data-ttu-id="dec07-133">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="dec07-133">timestamp</span></span>|
|--------|---------------|
|<span data-ttu-id="dec07-134">device1</span><span class="sxs-lookup"><span data-stu-id="dec07-134">device1</span></span>|<span data-ttu-id="dec07-135">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="dec07-135">2016-01-08T01:08:00Z</span></span>|

### <a name="sample-2"></a><span data-ttu-id="dec07-136">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="dec07-136">Sample 2</span></span>

#### <a name="input"></a><span data-ttu-id="dec07-137">Indata</span><span class="sxs-lookup"><span data-stu-id="dec07-137">Input</span></span>
<span data-ttu-id="dec07-138">En JSON-matris med två JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="dec07-138">A JSON array with two JSON objects.</span></span> <span data-ttu-id="dec07-139">Varje JSON-objekt ska vara konverterade tooan händelse.</span><span class="sxs-lookup"><span data-stu-id="dec07-139">Each JSON object will be converted tooan event.</span></span>
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---2-events"></a><span data-ttu-id="dec07-140">Resultat – 2 händelser</span><span class="sxs-lookup"><span data-stu-id="dec07-140">Output - 2 Events</span></span>

|<span data-ttu-id="dec07-141">id</span><span class="sxs-lookup"><span data-stu-id="dec07-141">id</span></span>|<span data-ttu-id="dec07-142">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="dec07-142">timestamp</span></span>|
|--------|---------------|
|<span data-ttu-id="dec07-143">device1</span><span class="sxs-lookup"><span data-stu-id="dec07-143">device1</span></span>|<span data-ttu-id="dec07-144">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="dec07-144">2016-01-08T01:08:00Z</span></span>|
|<span data-ttu-id="dec07-145">device2</span><span class="sxs-lookup"><span data-stu-id="dec07-145">device2</span></span>|<span data-ttu-id="dec07-146">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="dec07-146">2016-01-08T01:17:00Z</span></span>|
### <a name="sample-3"></a><span data-ttu-id="dec07-147">Exempel 3</span><span class="sxs-lookup"><span data-stu-id="dec07-147">Sample 3</span></span>

#### <a name="input"></a><span data-ttu-id="dec07-148">Indata</span><span class="sxs-lookup"><span data-stu-id="dec07-148">Input</span></span>

<span data-ttu-id="dec07-149">Ett JSON-objekt med en kapslad JSON-matris som innehåller två JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="dec07-149">A JSON object with a nested JSON array containing two JSON objects.</span></span>
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---2-events"></a><span data-ttu-id="dec07-150">Resultat – 2 händelser</span><span class="sxs-lookup"><span data-stu-id="dec07-150">Output - 2 Events</span></span>
<span data-ttu-id="dec07-151">Observera att hello egenskapen ”plats” har kopierats över tooeach hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="dec07-151">Note that hello property "location" is copied over tooeach of hello event.</span></span>

|<span data-ttu-id="dec07-152">location</span><span class="sxs-lookup"><span data-stu-id="dec07-152">location</span></span>|<span data-ttu-id="dec07-153">events.id</span><span class="sxs-lookup"><span data-stu-id="dec07-153">events.id</span></span>|<span data-ttu-id="dec07-154">events.timestamp</span><span class="sxs-lookup"><span data-stu-id="dec07-154">events.timestamp</span></span>|
|--------|---------------|----------------------|
|<span data-ttu-id="dec07-155">WestUs</span><span class="sxs-lookup"><span data-stu-id="dec07-155">WestUs</span></span>|<span data-ttu-id="dec07-156">device1</span><span class="sxs-lookup"><span data-stu-id="dec07-156">device1</span></span>|<span data-ttu-id="dec07-157">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="dec07-157">2016-01-08T01:08:00Z</span></span>|
|<span data-ttu-id="dec07-158">WestUs</span><span class="sxs-lookup"><span data-stu-id="dec07-158">WestUs</span></span>|<span data-ttu-id="dec07-159">device2</span><span class="sxs-lookup"><span data-stu-id="dec07-159">device2</span></span>|<span data-ttu-id="dec07-160">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="dec07-160">2016-01-08T01:17:00Z</span></span>|

### <a name="sample-4"></a><span data-ttu-id="dec07-161">Exempel 4</span><span class="sxs-lookup"><span data-stu-id="dec07-161">Sample 4</span></span>

#### <a name="input"></a><span data-ttu-id="dec07-162">Indata</span><span class="sxs-lookup"><span data-stu-id="dec07-162">Input</span></span>

<span data-ttu-id="dec07-163">Ett JSON-objekt med en kapslad JSON-matris som innehåller två JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="dec07-163">A JSON object with a nested JSON array containing two JSON objects.</span></span> <span data-ttu-id="dec07-164">Den här indata visar att hello globala egenskaper kan representeras av hello komplexa JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="dec07-164">This input demonstrates that hello global properties may be represented by hello complex JSON object.</span></span>

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---2-events"></a><span data-ttu-id="dec07-165">Resultat – 2 händelser</span><span class="sxs-lookup"><span data-stu-id="dec07-165">Output - 2 Events</span></span>

|<span data-ttu-id="dec07-166">location</span><span class="sxs-lookup"><span data-stu-id="dec07-166">location</span></span>|<span data-ttu-id="dec07-167">manufacturer.name</span><span class="sxs-lookup"><span data-stu-id="dec07-167">manufacturer.name</span></span>|<span data-ttu-id="dec07-168">manufacturer.location</span><span class="sxs-lookup"><span data-stu-id="dec07-168">manufacturer.location</span></span>|<span data-ttu-id="dec07-169">events.id</span><span class="sxs-lookup"><span data-stu-id="dec07-169">events.id</span></span>|<span data-ttu-id="dec07-170">events.timestamp</span><span class="sxs-lookup"><span data-stu-id="dec07-170">events.timestamp</span></span>|<span data-ttu-id="dec07-171">events.data.type</span><span class="sxs-lookup"><span data-stu-id="dec07-171">events.data.type</span></span>|<span data-ttu-id="dec07-172">events.data.units</span><span class="sxs-lookup"><span data-stu-id="dec07-172">events.data.units</span></span>|<span data-ttu-id="dec07-173">events.data.value</span><span class="sxs-lookup"><span data-stu-id="dec07-173">events.data.value</span></span>|
|---|---|---|---|---|---|---|---|
|<span data-ttu-id="dec07-174">WestUs</span><span class="sxs-lookup"><span data-stu-id="dec07-174">WestUs</span></span>|<span data-ttu-id="dec07-175">manufacturer1</span><span class="sxs-lookup"><span data-stu-id="dec07-175">manufacturer1</span></span>|<span data-ttu-id="dec07-176">EastUs</span><span class="sxs-lookup"><span data-stu-id="dec07-176">EastUs</span></span>|<span data-ttu-id="dec07-177">device1</span><span class="sxs-lookup"><span data-stu-id="dec07-177">device1</span></span>|<span data-ttu-id="dec07-178">2016-01-08T01:08:00Z</span><span class="sxs-lookup"><span data-stu-id="dec07-178">2016-01-08T01:08:00Z</span></span>|<span data-ttu-id="dec07-179">tryck</span><span class="sxs-lookup"><span data-stu-id="dec07-179">pressure</span></span>|<span data-ttu-id="dec07-180">psi</span><span class="sxs-lookup"><span data-stu-id="dec07-180">psi</span></span>|<span data-ttu-id="dec07-181">108.09</span><span class="sxs-lookup"><span data-stu-id="dec07-181">108.09</span></span>|
|<span data-ttu-id="dec07-182">WestUs</span><span class="sxs-lookup"><span data-stu-id="dec07-182">WestUs</span></span>|<span data-ttu-id="dec07-183">manufacturer1</span><span class="sxs-lookup"><span data-stu-id="dec07-183">manufacturer1</span></span>|<span data-ttu-id="dec07-184">EastUs</span><span class="sxs-lookup"><span data-stu-id="dec07-184">EastUs</span></span>|<span data-ttu-id="dec07-185">device2</span><span class="sxs-lookup"><span data-stu-id="dec07-185">device2</span></span>|<span data-ttu-id="dec07-186">2016-01-08T01:17:00Z</span><span class="sxs-lookup"><span data-stu-id="dec07-186">2016-01-08T01:17:00Z</span></span>|<span data-ttu-id="dec07-187">vibration</span><span class="sxs-lookup"><span data-stu-id="dec07-187">vibration</span></span>|<span data-ttu-id="dec07-188">abs G</span><span class="sxs-lookup"><span data-stu-id="dec07-188">abs G</span></span>|<span data-ttu-id="dec07-189">217.09</span><span class="sxs-lookup"><span data-stu-id="dec07-189">217.09</span></span>|

## <a name="next-steps"></a><span data-ttu-id="dec07-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dec07-190">Next steps</span></span>

* <span data-ttu-id="dec07-191">Visa din miljö i [Time Series Insights-portalen](https://insights.timeseries.azure.com)</span><span class="sxs-lookup"><span data-stu-id="dec07-191">View your environment in [Time Series Insights Portal](https://insights.timeseries.azure.com)</span></span>

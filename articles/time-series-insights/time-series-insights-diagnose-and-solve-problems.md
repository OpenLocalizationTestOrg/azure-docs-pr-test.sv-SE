---
title: "Diagnostisera och lösa problem | Microsoft Docs"
description: "Den här kursen visar hur du diagnostisera och lösa problem i miljön tid serien insikter"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/24/2017
ms.author: venkatja
ms.openlocfilehash: 4b8d5fdab1744b2db658917f91d6dac05db30d2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a><span data-ttu-id="d3cfe-103">Diagnostisera och lösa problem i miljön tid serien insikter</span><span class="sxs-lookup"><span data-stu-id="d3cfe-103">Diagnose and solve problems in your Time Series Insights environment</span></span>

## <a name="i-dont-see-my-data"></a><span data-ttu-id="d3cfe-104">Data visas inte</span><span class="sxs-lookup"><span data-stu-id="d3cfe-104">I don't see my data</span></span>
<span data-ttu-id="d3cfe-105">Här följer några orsaker varför du inte kan se dina data i din miljö i det [Azure tid serien Insights-portalen](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d3cfe-105">Here are some reasons why you might not see your data in your environment in the [Azure Time Series Insights portal](https://insights.timeseries.azure.com).</span></span>

### <a name="your-event-source-doesnt-have-data-in-json-format"></a><span data-ttu-id="d3cfe-106">Din händelsekälla saknar data i JSON-format</span><span class="sxs-lookup"><span data-stu-id="d3cfe-106">Your event source doesn't have data in JSON format</span></span>
<span data-ttu-id="d3cfe-107">Azure tid serien Insights har stöd för JSON-data i dag.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-107">Azure Time Series Insights supports only JSON data today.</span></span> <span data-ttu-id="d3cfe-108">JSON-exempel finns [stöds JSON former](time-series-insights-send-events.md#supported-json-shapes).</span><span class="sxs-lookup"><span data-stu-id="d3cfe-108">For JSON samples, see [Supported JSON shapes](time-series-insights-send-events.md#supported-json-shapes).</span></span>

### <a name="when-you-registered-your-event-source-you-didnt-provide-the-key-that-has-the-required-permission"></a><span data-ttu-id="d3cfe-109">När du har registrerat din händelsekälla uppgav du nyckeln som har behörigheten som krävs</span><span class="sxs-lookup"><span data-stu-id="d3cfe-109">When you registered your event source, you didn't provide the key that has the required permission</span></span>
* <span data-ttu-id="d3cfe-110">För en IoT-hubb måste du ange den nyckel som har **tjänsten ansluta** behörighet.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-110">For an IoT hub, you need to provide the key that has **service connect** permission.</span></span>

   ![IoT-hubb tjänsten ansluta behörighet](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   <span data-ttu-id="d3cfe-112">Som visas i föregående bild, antingen principer **iothubowner** och **service** skulle fungera eftersom båda **tjänsten ansluta** behörighet.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-112">As shown in the preceding image, either of the policies **iothubowner** and **service** would work, because both have **service connect** permission.</span></span>
* <span data-ttu-id="d3cfe-113">För en händelsehubb, måste du ange den nyckel som har **lyssna** behörighet.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-113">For an event hub, you need to provide the key that has **Listen** permission.</span></span>

   ![Event hub lyssna behörighet](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   <span data-ttu-id="d3cfe-115">Som visas i föregående bild, antingen principer **läsa** och **hantera** skulle fungera eftersom båda **lyssna** behörighet.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-115">As shown in the preceding image, either of the policies **read** and **manage** would work, because both have **Listen** permission.</span></span>

### <a name="the-provided-consumer-group-is-not-exclusive-to-time-series-insights"></a><span data-ttu-id="d3cfe-116">Den angivna konsumentgruppen är inte bara tiden serien insikter</span><span class="sxs-lookup"><span data-stu-id="d3cfe-116">The provided consumer group is not exclusive to Time Series Insights</span></span>
<span data-ttu-id="d3cfe-117">För en IoT-hubb eller en händelsehubb, under registreringen måste vi du ange konsumentgrupp som ska användas för att läsa in dina data.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-117">For an IoT hub or an event hub, during registration we require you to specify the consumer group that should be used for reading your data.</span></span> <span data-ttu-id="d3cfe-118">Den här konsumentgrupp får inte delas.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-118">This consumer group must not be shared.</span></span> <span data-ttu-id="d3cfe-119">Om den delas underliggande händelsehubben automatiskt kopplas från en läsare slumpmässigt.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-119">If it's shared, the underlying event hub automatically disconnects one of the readers randomly.</span></span>

## <a name="i-see-my-data-but-theres-a-lag"></a><span data-ttu-id="d3cfe-120">Mina data visas, men det finns en fördröjning</span><span class="sxs-lookup"><span data-stu-id="d3cfe-120">I see my data, but there's a lag</span></span>
<span data-ttu-id="d3cfe-121">Här finns skäl visas delar av informationen i din miljö i det [tid serien Insights-portalen](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d3cfe-121">Here are reasons why you might see partial data in your environment in the [Time Series Insights portal](https://insights.timeseries.azure.com).</span></span>

### <a name="your-environment-is-getting-throttled"></a><span data-ttu-id="d3cfe-122">Hämta begränsas din miljö</span><span class="sxs-lookup"><span data-stu-id="d3cfe-122">Your environment is getting throttled</span></span>
<span data-ttu-id="d3cfe-123">Gränsen tillämpas baserat på SKU miljötyp och kapacitet.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-123">The throttling limit is enforced based on the environment's SKU type and capacity.</span></span> <span data-ttu-id="d3cfe-124">Alla händelsekällor i miljön dela den här kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-124">All event sources in the environment share this capacity.</span></span> <span data-ttu-id="d3cfe-125">Om händelsekällan för din IoT-hubb eller händelsehubb är push-överföring av data utöver de tvingande gränserna, ser du begränsning och en fördröjning.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-125">If the event source for your IoT hub or event hub is pushing data beyond the enforced limits, you see throttling and a lag.</span></span>

<span data-ttu-id="d3cfe-126">I följande diagram visas en gång serien insikter miljö som har en SKU S1 och en kapacitet på 3.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-126">The following diagram shows a Time Series Insights environment that has a SKU of S1 and a capacity of 3.</span></span> <span data-ttu-id="d3cfe-127">Det kan 3 miljoner ingångshändelser per dag.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-127">It can ingress 3 million events per day.</span></span>

![Aktuell miljö artikelnummerkapaciteten](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

<span data-ttu-id="d3cfe-129">Anta att den här miljön är vill föra in meddelanden från en händelsehubb med meddelande om ingångs-sats som visas i följande diagram:</span><span class="sxs-lookup"><span data-stu-id="d3cfe-129">Assume that this environment is ingesting messages from an event hub with the ingress rate shown in the following diagram:</span></span>

![Exempel meddelanden om ingångs-sats för en händelsehubb](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

<span data-ttu-id="d3cfe-131">I diagrammet visas de dagliga ingång frekvensen är ~ 67,000 meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-131">As shown in the diagram, the daily ingress rate is ~67,000 messages.</span></span> <span data-ttu-id="d3cfe-132">Denna kurs översätter ungefär 46 meddelanden varje minut.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-132">This rate translates roughly to 46 messages every minute.</span></span> <span data-ttu-id="d3cfe-133">Om hub händelsemeddelande förenklas till en enda gång serien insikter händelse, ser den här miljön ingen begränsning.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-133">If each event hub message is flattened to a single Time Series Insights event, this environment sees no throttling.</span></span> <span data-ttu-id="d3cfe-134">Om hub händelsemeddelande förenklas till 100 tid serien insikter händelser, ska sedan 4,600 händelser vara inhämtas varje minut.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-134">If each event hub message is flattened to 100 Time Series Insights events, then 4,600 events should be ingested every minute.</span></span> <span data-ttu-id="d3cfe-135">En S1 SKU-miljö som har en kapacitet på 3 kan endast 2 100 ingångshändelser varje minut (1 miljon händelser per dag = 700 händelser per minut på 3 enheter = 2 100 händelser per minut).</span><span class="sxs-lookup"><span data-stu-id="d3cfe-135">An S1 SKU environment that has a capacity of 3 can ingress only 2,100 events every minute (1 million events per day = 700 events per minute at 3 units = 2,100 events per minute).</span></span> <span data-ttu-id="d3cfe-136">Därför kan du se en fördröjning på grund av begränsning.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-136">Therefore you see a lag due to throttling.</span></span> 

<span data-ttu-id="d3cfe-137">En översikt över hur förenkla logik fungerar, se [stöds JSON former](time-series-insights-send-events.md#supported-json-shapes).</span><span class="sxs-lookup"><span data-stu-id="d3cfe-137">For a high-level understanding of how flattening logic works, see [Supported JSON shapes](time-series-insights-send-events.md#supported-json-shapes).</span></span>

#### <a name="recommended-steps"></a><span data-ttu-id="d3cfe-138">Rekommenderade åtgärder</span><span class="sxs-lookup"><span data-stu-id="d3cfe-138">Recommended steps</span></span>
<span data-ttu-id="d3cfe-139">Om du vill åtgärda fördröjningen ökar du kapaciteten SKU för din miljö.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-139">To fix the lag, increase the SKU capacity of your environment.</span></span> <span data-ttu-id="d3cfe-140">Mer information finns i [så här skalar du tid serien insikter miljön](time-series-insights-how-to-scale-your-environment.md).</span><span class="sxs-lookup"><span data-stu-id="d3cfe-140">For more information, see [How to scale your Time Series Insights environment](time-series-insights-how-to-scale-your-environment.md).</span></span>

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a><span data-ttu-id="d3cfe-141">Du sänder historiska data och orsakar långsam ingång</span><span class="sxs-lookup"><span data-stu-id="d3cfe-141">You're pushing historical data and causing slow ingress</span></span>
<span data-ttu-id="d3cfe-142">Om du ansluter en befintlig datakälla för händelsen, är det troligt att din IoT-hubb eller händelsehubb redan har data i den.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-142">If you are connecting an existing event source, it's likely that your IoT hub or event hub already has data in it.</span></span> <span data-ttu-id="d3cfe-143">Miljön startar så att data från början av den händelsekälla kvarhållningsperiod för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-143">So the environment starts pulling data from the beginning of the event source's message retention period.</span></span> 

<span data-ttu-id="d3cfe-144">Detta är standardinställningen och inte kan åsidosättas.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-144">This behavior is the default behavior and cannot be overridden.</span></span> <span data-ttu-id="d3cfe-145">Du kan engagera begränsning och det kan ta en stund att titta på mata in historiska data.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-145">You can engage throttling, and it may take a while to catch up on ingesting historical data.</span></span>

#### <a name="recommended-steps"></a><span data-ttu-id="d3cfe-146">Rekommenderade åtgärder</span><span class="sxs-lookup"><span data-stu-id="d3cfe-146">Recommended steps</span></span>
<span data-ttu-id="d3cfe-147">Om du vill åtgärda fördröjningen gör du följande:</span><span class="sxs-lookup"><span data-stu-id="d3cfe-147">To fix the lag, take the following steps:</span></span>
1. <span data-ttu-id="d3cfe-148">Öka artikelnummerkapaciteten till det högsta tillåtna värdet (10 i det här fallet).</span><span class="sxs-lookup"><span data-stu-id="d3cfe-148">Increase the SKU capacity to the maximum allowed value (10 in this case).</span></span> <span data-ttu-id="d3cfe-149">När kapaciteten ökas startar processen ingång fånga upp mycket snabbare.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-149">After the capacity is increased, the ingress process starts catching up much faster.</span></span> <span data-ttu-id="d3cfe-150">Kan du visualisera hur snabbt du Ja via Tillgänglighetsdiagrammet i den [tid serien Insights-portalen](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d3cfe-150">You can visualize how quickly you're catching up through the availability chart in the [Time Series Insights portal](https://insights.timeseries.azure.com).</span></span> <span data-ttu-id="d3cfe-151">Du debiteras för bättre kapacitet.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-151">You are charged for the increased capacity.</span></span>
2. <span data-ttu-id="d3cfe-152">När fördröjningen fångats, minska artikelnummerkapaciteten tillbaka till normal ingång-hastighet.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-152">After the lag is caught up, decrease the SKU capacity back to your normal ingress rate.</span></span>

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a><span data-ttu-id="d3cfe-153">Min händelsekälla *tidsstämpel egenskapsnamn* inställningen fungerar inte</span><span class="sxs-lookup"><span data-stu-id="d3cfe-153">My event source's *timestamp property name* setting doesn't work</span></span>
<span data-ttu-id="d3cfe-154">Kontrollera att namnet och värdet överensstämmer med följande regler:</span><span class="sxs-lookup"><span data-stu-id="d3cfe-154">Ensure that the name and value conform to the following rules:</span></span>
* <span data-ttu-id="d3cfe-155">Egenskapsnamnet tidsstämpel är _skiftlägeskänsliga_.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-155">The timestamp property name is _case-sensitive_.</span></span>
* <span data-ttu-id="d3cfe-156">Egenskapsvärdet tidsstämpel som kommer från din händelsekälla som JSON-strängen ska ha formatet _åååå-MM-ddTHH. FFFFFFFK_.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-156">The timestamp property value that's coming from your event source, as a JSON string, should have the format _yyyy-MM-ddTHH:mm:ss.FFFFFFFK_.</span></span> <span data-ttu-id="d3cfe-157">Ett exempel på sådana sträng är ”2008-04-12T12:53Z”.</span><span class="sxs-lookup"><span data-stu-id="d3cfe-157">An example of such a string is “2008-04-12T12:53Z”.</span></span>

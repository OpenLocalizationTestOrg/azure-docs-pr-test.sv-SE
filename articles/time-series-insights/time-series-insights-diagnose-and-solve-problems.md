---
title: "aaaDiagnose och lösa problem | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur toodiagnose och lösa problem i miljön tid serien insikter"
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
ms.openlocfilehash: 00893d4bec497f5f8bf7093be5b96f1844446d13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a><span data-ttu-id="81ce8-103">Diagnostisera och lösa problem i miljön tid serien insikter</span><span class="sxs-lookup"><span data-stu-id="81ce8-103">Diagnose and solve problems in your Time Series Insights environment</span></span>

## <a name="i-dont-see-my-data"></a><span data-ttu-id="81ce8-104">Data visas inte</span><span class="sxs-lookup"><span data-stu-id="81ce8-104">I don't see my data</span></span>
<span data-ttu-id="81ce8-105">Här följer några orsaker varför du inte kan se dina data i din miljö i hello [Azure tid serien Insights-portalen](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="81ce8-105">Here are some reasons why you might not see your data in your environment in hello [Azure Time Series Insights portal](https://insights.timeseries.azure.com).</span></span>

### <a name="your-event-source-doesnt-have-data-in-json-format"></a><span data-ttu-id="81ce8-106">Din händelsekälla saknar data i JSON-format</span><span class="sxs-lookup"><span data-stu-id="81ce8-106">Your event source doesn't have data in JSON format</span></span>
<span data-ttu-id="81ce8-107">Azure tid serien Insights har stöd för JSON-data i dag.</span><span class="sxs-lookup"><span data-stu-id="81ce8-107">Azure Time Series Insights supports only JSON data today.</span></span> <span data-ttu-id="81ce8-108">JSON-exempel finns [stöds JSON former](time-series-insights-send-events.md#supported-json-shapes).</span><span class="sxs-lookup"><span data-stu-id="81ce8-108">For JSON samples, see [Supported JSON shapes](time-series-insights-send-events.md#supported-json-shapes).</span></span>

### <a name="when-you-registered-your-event-source-you-didnt-provide-hello-key-that-has-hello-required-permission"></a><span data-ttu-id="81ce8-109">När du har registrerat din händelsekälla uppgav du hello-nyckeln som har behörighet för hello krävs</span><span class="sxs-lookup"><span data-stu-id="81ce8-109">When you registered your event source, you didn't provide hello key that has hello required permission</span></span>
* <span data-ttu-id="81ce8-110">För en IoT-hubb måste tooprovide hello nyckel som har **tjänsten ansluta** behörighet.</span><span class="sxs-lookup"><span data-stu-id="81ce8-110">For an IoT hub, you need tooprovide hello key that has **service connect** permission.</span></span>

   ![IoT-hubb tjänsten ansluta behörighet](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   <span data-ttu-id="81ce8-112">Som visas i föregående bild, hello antingen hello principer **iothubowner** och **service** skulle fungera eftersom båda **tjänsten ansluta** behörighet.</span><span class="sxs-lookup"><span data-stu-id="81ce8-112">As shown in hello preceding image, either of hello policies **iothubowner** and **service** would work, because both have **service connect** permission.</span></span>
* <span data-ttu-id="81ce8-113">För en händelsehubb, behöver du tooprovide hello nyckel som har **lyssna** behörighet.</span><span class="sxs-lookup"><span data-stu-id="81ce8-113">For an event hub, you need tooprovide hello key that has **Listen** permission.</span></span>

   ![Event hub lyssna behörighet](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   <span data-ttu-id="81ce8-115">Som visas i föregående bild, hello antingen hello principer **läsa** och **hantera** skulle fungera eftersom båda **lyssna** behörighet.</span><span class="sxs-lookup"><span data-stu-id="81ce8-115">As shown in hello preceding image, either of hello policies **read** and **manage** would work, because both have **Listen** permission.</span></span>

### <a name="hello-provided-consumer-group-is-not-exclusive-tootime-series-insights"></a><span data-ttu-id="81ce8-116">hello angetts konsumentgrupp inte samtidigt tooTime serie insikter</span><span class="sxs-lookup"><span data-stu-id="81ce8-116">hello provided consumer group is not exclusive tooTime Series Insights</span></span>
<span data-ttu-id="81ce8-117">För en IoT-hubb eller en händelsehubb, under registreringen kräver vi att du toospecify hello konsumentgrupp som ska användas för att läsa in dina data.</span><span class="sxs-lookup"><span data-stu-id="81ce8-117">For an IoT hub or an event hub, during registration we require you toospecify hello consumer group that should be used for reading your data.</span></span> <span data-ttu-id="81ce8-118">Den här konsumentgrupp får inte delas.</span><span class="sxs-lookup"><span data-stu-id="81ce8-118">This consumer group must not be shared.</span></span> <span data-ttu-id="81ce8-119">Om den delas hello underliggande händelsehubb automatiskt kopplas från en av hello läsare slumpmässigt.</span><span class="sxs-lookup"><span data-stu-id="81ce8-119">If it's shared, hello underlying event hub automatically disconnects one of hello readers randomly.</span></span>

## <a name="i-see-my-data-but-theres-a-lag"></a><span data-ttu-id="81ce8-120">Mina data visas, men det finns en fördröjning</span><span class="sxs-lookup"><span data-stu-id="81ce8-120">I see my data, but there's a lag</span></span>
<span data-ttu-id="81ce8-121">Här finns skäl visas delar av informationen i din miljö i hello [tid serien Insights-portalen](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="81ce8-121">Here are reasons why you might see partial data in your environment in hello [Time Series Insights portal](https://insights.timeseries.azure.com).</span></span>

### <a name="your-environment-is-getting-throttled"></a><span data-ttu-id="81ce8-122">Hämta begränsas din miljö</span><span class="sxs-lookup"><span data-stu-id="81ce8-122">Your environment is getting throttled</span></span>
<span data-ttu-id="81ce8-123">hello begränsning gränsen tillämpas utifrån hello miljö SKU typ och kapacitet.</span><span class="sxs-lookup"><span data-stu-id="81ce8-123">hello throttling limit is enforced based on hello environment's SKU type and capacity.</span></span> <span data-ttu-id="81ce8-124">Alla händelsekällor i hello miljö dela den här kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="81ce8-124">All event sources in hello environment share this capacity.</span></span> <span data-ttu-id="81ce8-125">Om hello händelsekällan för din IoT-hubb eller händelsehubb push-överföring av data utöver hello tillämpas gränser, ser du begränsning och en fördröjning.</span><span class="sxs-lookup"><span data-stu-id="81ce8-125">If hello event source for your IoT hub or event hub is pushing data beyond hello enforced limits, you see throttling and a lag.</span></span>

<span data-ttu-id="81ce8-126">hello följande diagram visar en tid serien insikter miljö som har en SKU S1 och en kapacitet på 3.</span><span class="sxs-lookup"><span data-stu-id="81ce8-126">hello following diagram shows a Time Series Insights environment that has a SKU of S1 and a capacity of 3.</span></span> <span data-ttu-id="81ce8-127">Det kan 3 miljoner ingångshändelser per dag.</span><span class="sxs-lookup"><span data-stu-id="81ce8-127">It can ingress 3 million events per day.</span></span>

![Aktuell miljö artikelnummerkapaciteten](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

<span data-ttu-id="81ce8-129">Anta att den här miljön är vill föra in meddelanden från en händelsehubb med hello ingång hastighet visas i följande diagram hello:</span><span class="sxs-lookup"><span data-stu-id="81ce8-129">Assume that this environment is ingesting messages from an event hub with hello ingress rate shown in hello following diagram:</span></span>

![Exempel meddelanden om ingångs-sats för en händelsehubb](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

<span data-ttu-id="81ce8-131">I hello diagram visas hello dagliga ingång frekvensen är ~ 67,000 meddelanden.</span><span class="sxs-lookup"><span data-stu-id="81ce8-131">As shown in hello diagram, hello daily ingress rate is ~67,000 messages.</span></span> <span data-ttu-id="81ce8-132">Denna kurs översätter ungefär too46 meddelanden varje minut.</span><span class="sxs-lookup"><span data-stu-id="81ce8-132">This rate translates roughly too46 messages every minute.</span></span> <span data-ttu-id="81ce8-133">Om hub händelsemeddelande är sammanslagna tooa tid serien insikter händelse, ser den här miljön ingen begränsning.</span><span class="sxs-lookup"><span data-stu-id="81ce8-133">If each event hub message is flattened tooa single Time Series Insights event, this environment sees no throttling.</span></span> <span data-ttu-id="81ce8-134">Om hub händelsemeddelande Flat too100 tid serien insikter händelser ska sedan 4,600 händelser vara inhämtas varje minut.</span><span class="sxs-lookup"><span data-stu-id="81ce8-134">If each event hub message is flattened too100 Time Series Insights events, then 4,600 events should be ingested every minute.</span></span> <span data-ttu-id="81ce8-135">En S1 SKU-miljö som har en kapacitet på 3 kan endast 2 100 ingångshändelser varje minut (1 miljon händelser per dag = 700 händelser per minut på 3 enheter = 2 100 händelser per minut).</span><span class="sxs-lookup"><span data-stu-id="81ce8-135">An S1 SKU environment that has a capacity of 3 can ingress only 2,100 events every minute (1 million events per day = 700 events per minute at 3 units = 2,100 events per minute).</span></span> <span data-ttu-id="81ce8-136">Därför visas en fördröjning på grund av toothrottling.</span><span class="sxs-lookup"><span data-stu-id="81ce8-136">Therefore you see a lag due toothrottling.</span></span> 

<span data-ttu-id="81ce8-137">En översikt över hur förenkla logik fungerar, se [stöds JSON former](time-series-insights-send-events.md#supported-json-shapes).</span><span class="sxs-lookup"><span data-stu-id="81ce8-137">For a high-level understanding of how flattening logic works, see [Supported JSON shapes](time-series-insights-send-events.md#supported-json-shapes).</span></span>

#### <a name="recommended-steps"></a><span data-ttu-id="81ce8-138">Rekommenderade åtgärder</span><span class="sxs-lookup"><span data-stu-id="81ce8-138">Recommended steps</span></span>
<span data-ttu-id="81ce8-139">toofix hello lag, öka hello artikelnummerkapaciteten i din miljö.</span><span class="sxs-lookup"><span data-stu-id="81ce8-139">toofix hello lag, increase hello SKU capacity of your environment.</span></span> <span data-ttu-id="81ce8-140">Mer information finns i [hur tooscale tid serien insikter miljön](time-series-insights-how-to-scale-your-environment.md).</span><span class="sxs-lookup"><span data-stu-id="81ce8-140">For more information, see [How tooscale your Time Series Insights environment](time-series-insights-how-to-scale-your-environment.md).</span></span>

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a><span data-ttu-id="81ce8-141">Du sänder historiska data och orsakar långsam ingång</span><span class="sxs-lookup"><span data-stu-id="81ce8-141">You're pushing historical data and causing slow ingress</span></span>
<span data-ttu-id="81ce8-142">Om du ansluter en befintlig datakälla för händelsen, är det troligt att din IoT-hubb eller händelsehubb redan har data i den.</span><span class="sxs-lookup"><span data-stu-id="81ce8-142">If you are connecting an existing event source, it's likely that your IoT hub or event hub already has data in it.</span></span> <span data-ttu-id="81ce8-143">Hello miljö startar så att data från hello början av hello händelsekällans kvarhållningsperiod för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="81ce8-143">So hello environment starts pulling data from hello beginning of hello event source's message retention period.</span></span> 

<span data-ttu-id="81ce8-144">Detta är standardfunktionen för hello och inte kan åsidosättas.</span><span class="sxs-lookup"><span data-stu-id="81ce8-144">This behavior is hello default behavior and cannot be overridden.</span></span> <span data-ttu-id="81ce8-145">Du kan engagera begränsning och det kan ta en stund toocatch in på att mata in historiska data.</span><span class="sxs-lookup"><span data-stu-id="81ce8-145">You can engage throttling, and it may take a while toocatch up on ingesting historical data.</span></span>

#### <a name="recommended-steps"></a><span data-ttu-id="81ce8-146">Rekommenderade åtgärder</span><span class="sxs-lookup"><span data-stu-id="81ce8-146">Recommended steps</span></span>
<span data-ttu-id="81ce8-147">toofix hello lag, vidta hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="81ce8-147">toofix hello lag, take hello following steps:</span></span>
1. <span data-ttu-id="81ce8-148">Öka hello SKU kapacitet toohello högsta tillåtna värdet (10 i det här fallet).</span><span class="sxs-lookup"><span data-stu-id="81ce8-148">Increase hello SKU capacity toohello maximum allowed value (10 in this case).</span></span> <span data-ttu-id="81ce8-149">När hello kapacitet ökas startar hello ingång processen fånga upp mycket snabbare.</span><span class="sxs-lookup"><span data-stu-id="81ce8-149">After hello capacity is increased, hello ingress process starts catching up much faster.</span></span> <span data-ttu-id="81ce8-150">Du kan visualisera hur snabbt du Ja via hello tillgänglighet diagram i hello [tid serien Insights-portalen](https://insights.timeseries.azure.com).</span><span class="sxs-lookup"><span data-stu-id="81ce8-150">You can visualize how quickly you're catching up through hello availability chart in hello [Time Series Insights portal](https://insights.timeseries.azure.com).</span></span> <span data-ttu-id="81ce8-151">Du debiteras för hello ökade kapacitet.</span><span class="sxs-lookup"><span data-stu-id="81ce8-151">You are charged for hello increased capacity.</span></span>
2. <span data-ttu-id="81ce8-152">När hello fördröjning fångats upp, minska hello SKU kapacitet tillbaka tooyour normal ingång hastighet.</span><span class="sxs-lookup"><span data-stu-id="81ce8-152">After hello lag is caught up, decrease hello SKU capacity back tooyour normal ingress rate.</span></span>

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a><span data-ttu-id="81ce8-153">Min händelsekälla *tidsstämpel egenskapsnamn* inställningen fungerar inte</span><span class="sxs-lookup"><span data-stu-id="81ce8-153">My event source's *timestamp property name* setting doesn't work</span></span>
<span data-ttu-id="81ce8-154">Säkerställa att hello namn och värde överensstämmer toohello följande regler:</span><span class="sxs-lookup"><span data-stu-id="81ce8-154">Ensure that hello name and value conform toohello following rules:</span></span>
* <span data-ttu-id="81ce8-155">hello tidsstämpel egenskapsnamnet är _skiftlägeskänsliga_.</span><span class="sxs-lookup"><span data-stu-id="81ce8-155">hello timestamp property name is _case-sensitive_.</span></span>
* <span data-ttu-id="81ce8-156">hello egenskapen tidsstämpelvärde som kommer från din händelsekälla som JSON-strängen ska ha hello format _åååå-MM-ddTHH. FFFFFFFK_.</span><span class="sxs-lookup"><span data-stu-id="81ce8-156">hello timestamp property value that's coming from your event source, as a JSON string, should have hello format _yyyy-MM-ddTHH:mm:ss.FFFFFFFK_.</span></span> <span data-ttu-id="81ce8-157">Ett exempel på sådana sträng är ”2008-04-12T12:53Z”.</span><span class="sxs-lookup"><span data-stu-id="81ce8-157">An example of such a string is “2008-04-12T12:53Z”.</span></span>

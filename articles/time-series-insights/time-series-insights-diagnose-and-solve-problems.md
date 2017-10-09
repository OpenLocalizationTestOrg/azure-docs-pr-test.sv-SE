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
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnostisera och lösa problem i miljön tid serien insikter

## <a name="i-dont-see-my-data"></a>Data visas inte
Här följer några orsaker varför du inte kan se dina data i din miljö i hello [Azure tid serien Insights-portalen](https://insights.timeseries.azure.com).

### <a name="your-event-source-doesnt-have-data-in-json-format"></a>Din händelsekälla saknar data i JSON-format
Azure tid serien Insights har stöd för JSON-data i dag. JSON-exempel finns [stöds JSON former](time-series-insights-send-events.md#supported-json-shapes).

### <a name="when-you-registered-your-event-source-you-didnt-provide-hello-key-that-has-hello-required-permission"></a>När du har registrerat din händelsekälla uppgav du hello-nyckeln som har behörighet för hello krävs
* För en IoT-hubb måste tooprovide hello nyckel som har **tjänsten ansluta** behörighet.

   ![IoT-hubb tjänsten ansluta behörighet](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Som visas i föregående bild, hello antingen hello principer **iothubowner** och **service** skulle fungera eftersom båda **tjänsten ansluta** behörighet.
* För en händelsehubb, behöver du tooprovide hello nyckel som har **lyssna** behörighet.

   ![Event hub lyssna behörighet](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Som visas i föregående bild, hello antingen hello principer **läsa** och **hantera** skulle fungera eftersom båda **lyssna** behörighet.

### <a name="hello-provided-consumer-group-is-not-exclusive-tootime-series-insights"></a>hello angetts konsumentgrupp inte samtidigt tooTime serie insikter
För en IoT-hubb eller en händelsehubb, under registreringen kräver vi att du toospecify hello konsumentgrupp som ska användas för att läsa in dina data. Den här konsumentgrupp får inte delas. Om den delas hello underliggande händelsehubb automatiskt kopplas från en av hello läsare slumpmässigt.

## <a name="i-see-my-data-but-theres-a-lag"></a>Mina data visas, men det finns en fördröjning
Här finns skäl visas delar av informationen i din miljö i hello [tid serien Insights-portalen](https://insights.timeseries.azure.com).

### <a name="your-environment-is-getting-throttled"></a>Hämta begränsas din miljö
hello begränsning gränsen tillämpas utifrån hello miljö SKU typ och kapacitet. Alla händelsekällor i hello miljö dela den här kapaciteten. Om hello händelsekällan för din IoT-hubb eller händelsehubb push-överföring av data utöver hello tillämpas gränser, ser du begränsning och en fördröjning.

hello följande diagram visar en tid serien insikter miljö som har en SKU S1 och en kapacitet på 3. Det kan 3 miljoner ingångshändelser per dag.

![Aktuell miljö artikelnummerkapaciteten](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Anta att den här miljön är vill föra in meddelanden från en händelsehubb med hello ingång hastighet visas i följande diagram hello:

![Exempel meddelanden om ingångs-sats för en händelsehubb](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

I hello diagram visas hello dagliga ingång frekvensen är ~ 67,000 meddelanden. Denna kurs översätter ungefär too46 meddelanden varje minut. Om hub händelsemeddelande är sammanslagna tooa tid serien insikter händelse, ser den här miljön ingen begränsning. Om hub händelsemeddelande Flat too100 tid serien insikter händelser ska sedan 4,600 händelser vara inhämtas varje minut. En S1 SKU-miljö som har en kapacitet på 3 kan endast 2 100 ingångshändelser varje minut (1 miljon händelser per dag = 700 händelser per minut på 3 enheter = 2 100 händelser per minut). Därför visas en fördröjning på grund av toothrottling. 

En översikt över hur förenkla logik fungerar, se [stöds JSON former](time-series-insights-send-events.md#supported-json-shapes).

#### <a name="recommended-steps"></a>Rekommenderade åtgärder
toofix hello lag, öka hello artikelnummerkapaciteten i din miljö. Mer information finns i [hur tooscale tid serien insikter miljön](time-series-insights-how-to-scale-your-environment.md).

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a>Du sänder historiska data och orsakar långsam ingång
Om du ansluter en befintlig datakälla för händelsen, är det troligt att din IoT-hubb eller händelsehubb redan har data i den. Hello miljö startar så att data från hello början av hello händelsekällans kvarhållningsperiod för meddelandet. 

Detta är standardfunktionen för hello och inte kan åsidosättas. Du kan engagera begränsning och det kan ta en stund toocatch in på att mata in historiska data.

#### <a name="recommended-steps"></a>Rekommenderade åtgärder
toofix hello lag, vidta hello följande steg:
1. Öka hello SKU kapacitet toohello högsta tillåtna värdet (10 i det här fallet). När hello kapacitet ökas startar hello ingång processen fånga upp mycket snabbare. Du kan visualisera hur snabbt du Ja via hello tillgänglighet diagram i hello [tid serien Insights-portalen](https://insights.timeseries.azure.com). Du debiteras för hello ökade kapacitet.
2. När hello fördröjning fångats upp, minska hello SKU kapacitet tillbaka tooyour normal ingång hastighet.

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Min händelsekälla *tidsstämpel egenskapsnamn* inställningen fungerar inte
Säkerställa att hello namn och värde överensstämmer toohello följande regler:
* hello tidsstämpel egenskapsnamnet är _skiftlägeskänsliga_.
* hello egenskapen tidsstämpelvärde som kommer från din händelsekälla som JSON-strängen ska ha hello format _åååå-MM-ddTHH. FFFFFFFK_. Ett exempel på sådana sträng är ”2008-04-12T12:53Z”.

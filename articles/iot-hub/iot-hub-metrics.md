---
title: "aaaUse mått toomonitor Azure IoT Hub | Microsoft Docs"
description: "Hur toouse Azure IoT Hub mått tooassess och övervaka hello övergripande hälsa för din IoT-hubbar."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a47108fd-f994-4105-b21d-5b8f697b699c
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7d045013fb0229f488e72c93a6f668048b9d5c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-iot-hub-metrics"></a>Förstå IoT-hubb mått
IoT-hubb mått får du bättre information om hello tillstånd hello Azure IoT resurser i din Azure-prenumeration. IoT-hubb mått aktivera du tooassess hello övergripande hälsa för hello IoT-hubb-tjänsten och hello enheter anslutna tooit. Användarinriktad statistik är viktiga eftersom de hjälper dig att se vad som händer din IoT-hubb och hjälper dig att grundorsaken problem och utan att behöva toocontact Azure-supporten.

Mått är aktiverade som standard. Du kan visa mått för IoT-hubb hello Azure-portalen.

## <a name="how-tooview-iot-hub-metrics"></a>Hur tooview IoT-hubb mått
1. Skapa en IoT-hubb. Du hittar anvisningar om hur toocreate en IoT-hubb i hello [Kom igång] [ lnk-get-started] guide.
2. Öppna hello bladet för din IoT-hubb. Därifrån klickar du på **mått**.
   
    ![][1]
3. Du kan visa hello mått för din IoT-hubb och skapa anpassade vyer av dina från hello mått bladet. Du kan välja toosend mått data tooan Händelsehubbar slutpunkten eller ett Azure Storage-konto genom att klicka på **diagnostikinställningarna**.
   
    ![][2]

## <a name="iot-hub-metrics-and-how-toouse-them"></a>IoT-hubb mått och hur toouse dem.
IoT-hubb innehåller flera mått toogive du en översikt över hello hälsotillståndet för din hubb och hello Totalt antal anslutna enheter. Du kan kombinera information från flera mått toopaint en större bild av hello tillståndet för hello IoT-hubb. hello i den följande tabellen beskrivs hello mätvärden varje IoT-hubb spårar och hur varje mått relaterad toohello övergripande status för hello IoT-hubb.

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetri meddelande Skicka försök|Antal|Totalt|Antal försök toobe för enhet till moln telemetri meddelanden skickas tooyour IoT-hubb|
|d2c.telemetry.Ingress.Success|Telemetri meddelanden som skickas|Antal|Totalt|Antal meddelanden enhet till moln telemetri skickats tooyour IoT-hubb|
|c2d.commands.egress.Complete.Success|Kommandon som slutförd|Antal|Totalt|Antal moln till enhet kommandon har slutförts av hello-enhet|
|c2d.commands.egress.Abandon.Success|Avbrutna kommandon|Antal|Totalt|Antal moln till enhet kommandon avbrutna av hello-enhet|
|c2d.commands.egress.Reject.Success|Kommandon som avvisat|Antal|Totalt|Antal moln till enhet kommandon som avvisats av hello-enhet|
|devices.totalDevices|Totalt antal enheter|Antal|Totalt|Antal enheter registrerade tooyour IoT-hubb|
|devices.connectedDevices.allProtocol|Anslutna enheter|Antal|Totalt|Antal enheter anslutna tooyour IoT-hubb|
|d2c.telemetry.egress.Success|Telemetri meddelanden|Antal|Totalt|Antal gånger som meddelanden har skrivits tooendpoints (totalt)|
|d2c.telemetry.egress.dropped|Ignorerade meddelanden|Antal|Totalt|Antal meddelanden som utelämnats eftersom de matchade inte några vägar och hello återställningsplats vägen har inaktiverats|
|d2c.telemetry.egress.orphaned|Överblivna meddelanden|Antal|Totalt|hello antal meddelanden som inte matchar några vägar inklusive hello återställningsplats väg|
|d2c.telemetry.egress.Invalid|Ogiltig meddelanden|Antal|Totalt|hello antalet meddelanden inte levereras på grund av tooincompatibility med hello slutpunkt|
|d2c.telemetry.egress.fallback|Meddelanden som matchar villkoret återställningsplats|Antal|Totalt|Antal meddelanden som skrivs toohello återställningsplats slutpunkt|
|d2c.endpoints.egress.eventHubs|Meddelanden levereras tooEvent hubbslutpunkter|Antal|Totalt|Antal gånger som meddelanden har har skrivits tooEvent hubbslutpunkter|
|d2c.endpoints.latency.eventHubs|Meddelandet svarstid för Event Hub-slutpunkter|millisekunder|Genomsnittlig|hello genomsnittlig svarstid mellan meddelandet ingång toohello IoT-hubb och meddelandet ingång i Event Hub slutpunkt, i millisekunder|
|d2c.endpoints.egress.serviceBusQueues|Meddelanden levereras tooService Bus-kö slutpunkter|Antal|Totalt|Antal gånger som meddelanden har har skrivits tooService Bus-kö slutpunkter|
|d2c.endpoints.latency.serviceBusQueues|Meddelandet svarstid för Service Bus-kö-slutpunkter|millisekunder|Genomsnittlig|hello genomsnittlig svarstid mellan meddelandet ingång toohello IoT-hubb och meddelandet ingång till en slutpunkt för Service Bus-kö, i millisekunder|
|d2c.endpoints.egress.serviceBusTopics|Meddelanden levereras tooService Bus-ämne slutpunkter|Antal|Totalt|Antal gånger som meddelanden har har skrivits tooService Bus-ämne slutpunkter|
|d2c.endpoints.latency.serviceBusTopics|Meddelandet svarstiden för Service Bus-ämne slutpunkter|millisekunder|Genomsnittlig|hello genomsnittlig svarstid mellan meddelandet ingång toohello IoT-hubb och meddelandet ingång till en slutpunkt för Service Bus-ämne, i millisekunder|
|d2c.endpoints.egress.builtIn.events|Meddelanden som har levererats toohello inbyggd slutpunkt (meddelanden/händelser)|Antal|Totalt|Antal gånger som meddelanden har har skrivits toohello inbyggd slutpunkt (meddelanden/händelser)|
|d2c.endpoints.latency.builtIn.events|Meddelandet svarstid för hello inbyggd slutpunkt (meddelanden/händelser)|millisekunder|Genomsnittlig|hello genomsnittlig svarstid mellan meddelandet ingång toohello IoT-hubb och meddelandet ingång i hello inbyggd slutpunkt (meddelanden/händelser), i millisekunder |
|d2c.Twin.Read.Success|Lyckad dubbla läser från enheter|Antal|Totalt|hello antal alla lyckade enhet initieras dubbla läser.|
|d2c.Twin.Read.failure|Det gick inte dubbla läsningar av enheter|Antal|Totalt|Det gick inte att enhet initieras dubbla läsningar hello antalet.|
|d2c.Twin.Read.size|Svarsstorlek dubbla läsningar av enheter|Byte|Genomsnittlig|hello medelvärde, min och max för alla lyckade enhet initieras dubbla läsningar.|
|d2c.Twin.Update.Success|Lyckad dubbla uppdateringar från enheter|Antal|Totalt|hello antalet alla lyckade enhet initieras dubbla uppdateringar.|
|d2c.Twin.Update.failure|Det gick inte dubbla uppdateringar från enheter|Antal|Totalt|Det gick inte att enheten-initierad dubbla uppdateringar hello antalet.|
|d2c.Twin.Update.size|Storleken på dubbla uppdateringar från enheter|Byte|Genomsnittlig|hello medel, min och max storlek för alla lyckade enhet initieras dubbla uppdateringar.|
|c2d.methods.Success|Lyckad direkta metoden anrop|Antal|Totalt|hello antal alla lyckade direkt metodanrop.|
|c2d.methods.failure|Det gick inte direkt metod anrop|Antal|Totalt|Det gick inte att direkt metodanrop hello antalet.|
|c2d.methods.requestSize|Storleken på direkta metoden anrop för begäran|Byte|Genomsnittlig|hello medel, min och max för alla lyckade direkt metodbegäranden.|
|c2d.methods.responseSize|Storlek för direkta metoden anrop|Byte|Genomsnittlig|hello medelvärde, min och max för alla lyckade direkta metoden svar.|
|c2d.Twin.Read.Success|Lyckad dubbla läser från serverdelen|Antal|Totalt|hello antal alla lyckade tillbaka-end-initierad dubbla läser.|
|c2d.Twin.Read.failure|Misslyckade dubbla läser från serverdelen|Antal|Totalt|Det gick inte att tillbaka-end-initierad dubbla läsningar hello antalet.|
|c2d.Twin.Read.size|Storlek för dubbla läser från serverdelen|Byte|Genomsnittlig|hello medelvärde, min och max av alla lyckade tillbaka-end-initierad dubbla läsningar.|
|c2d.Twin.Update.Success|Lyckad dubbla uppdateringar från serverdelen|Antal|Totalt|hello antalet alla lyckade tillbaka-end-initierad dubbla uppdateringar.|
|c2d.Twin.Update.failure|Misslyckade dubbla uppdateringar från serverdelen|Antal|Totalt|Det gick inte att tillbaka-end-initierad dubbla uppdateringar hello antalet.|
|c2d.Twin.Update.size|Storleken på dubbla uppdateringar från serverdelen|Byte|Genomsnittlig|hello medel, min och max storlek för alla lyckade tillbaka-end-initierad dubbla uppdateringar.|
|twinQueries.success|Lyckad dubbla frågor|Antal|Totalt|hello antal alla lyckade dubbla frågor.|
|twinQueries.failure|Misslyckade dubbla frågor|Antal|Totalt|hello antal alla misslyckade dubbla frågor.|
|twinQueries.resultSize|Dubbla frågor storlek|Byte|Genomsnittlig|hello medelvärde, min och max hello resultatet av alla lyckade dubbla frågor storlek.|
|jobs.createTwinUpdateJob.success|Lyckad skapande av dubbla uppdatera jobb|Antal|Totalt|hello antal alla lyckade skapandet av dubbla uppdateringsjobb.|
|jobs.createTwinUpdateJob.failure|Misslyckade skapande av dubbla uppdatera jobb|Antal|Totalt|hello antal alla misslyckad generering av dubbla uppdateringsjobb.|
|jobs.createDirectMethodJob.success|Lyckad skapande av metoden anrops-jobb|Antal|Totalt|hello antal alla har skapats på direkta metoden anrop jobb.|
|jobs.createDirectMethodJob.failure|Misslyckade skapande av metoden anrops-jobb|Antal|Totalt|hello antal alla misslyckad generering av direkta metoden anrop jobb.|
|jobs.listJobs.success|Antal samtal toolist jobb|Antal|Totalt|hello antal alla lyckade anrop toolist jobb.|
|jobs.listJobs.failure|Misslyckade anrop toolist jobb|Antal|Totalt|hello antal alla misslyckade anrop toolist jobb.|
|jobs.cancelJob.success|Lyckad jobbavbrott|Antal|Totalt|hello antal alla lyckade anropar toocancel ett jobb.|
|jobs.cancelJob.failure|Avbryta misslyckade jobb|Antal|Totalt|hello antal alla misslyckade anrop toocancel ett jobb.|
|jobs.queryJobs.success|Jobbfrågor|Antal|Totalt|hello antal alla lyckade anrop tooquery jobb.|
|jobs.queryJobs.failure|Misslyckade jobbfrågor|Antal|Totalt|hello antal alla misslyckade anrop tooquery jobb.|
|jobs.Completed|Slutförda jobb|Antal|Totalt|hello antal alla slutförda jobb.|
|jobs.Failed|Misslyckade jobb|Antal|Totalt|hello antal alla misslyckade jobb.|

## <a name="next-steps"></a>Nästa steg
Nu när du har läst en översikt över IoT-hubb mått, så den här länken toolearn mer om hur du hanterar Azure IoT-hubb:

* [Åtgärder som övervakning][lnk-monitor]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Simulera en enhet med Azure IoT kant][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

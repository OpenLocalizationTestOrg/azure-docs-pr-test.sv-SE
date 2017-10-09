---
title: aaaUnderstand Azure IoT Hub jobb | Microsoft Docs
description: "Utvecklarhandbok - schemaläggning av jobb toorun på flera enheter anslutna tooyour IoT-hubb. Jobb kan uppdatera taggar och önskade egenskaper och anropa direkt metoder på flera enheter."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: fe78458f-4f14-4358-ac83-4f7bd14ee8da
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: 8be134e6c379feae5087df8f562a74505c57afee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-jobs-on-multiple-devices"></a>Schemalägga jobb på flera enheter
## <a name="overview"></a>Översikt
Enligt beskrivningen i föregående artiklar Azure IoT Hub möjliggör ett antal byggblock ([dubbla enhetsegenskaper och taggar] [ lnk-twin-devguide] och [direkt metoder][lnk-dev-methods]).  Normalt backend-appar Aktivera enheten administratörer och ansvariga för tooupdate och interagera med IoT-enheter gruppvis och vid ett schemalagt klockslag.  Jobb kapslar in hello körning av dubbla uppdateringar och direkt metoder mot en uppsättning enheter i taget schema.  En operator skulle till exempel använda en backend-app som skulle startar och spårar en jobbet tooreboot en uppsättning enheter i att skapa 43 och våning 3 vid en tidpunkt som inte skulle vara störande toohello operations hello byggnaden.

### <a name="when-toouse"></a>När toouse
Överväg att använda jobb när: en lösning för serverdelen måste tooschedule och spåra vidare någon hello följande aktiviteter på en uppsättning enheter:

* Uppdatera önskade egenskaper
* Uppdatera taggar
* Anropa direkt metoder

## <a name="job-lifecycle"></a>Jobbet livscykel
Jobb initieras av hello lösningens serverdel och underhålls av IoT-hubb.  Du kan starta ett jobb via en tjänst-riktade URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) och status för ett jobb som körs via en tjänst-riktade URI-fråga (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).  När ett jobb startas kan frågar efter jobb hello backend-app toorefresh hello status för jobb som körs.

> [!NOTE]
> När du startar en egenskapsnamn och värden kan endast innehålla US-ASCII utskrivbara alfanumeriskt, förutom eventuella i hello följande uppsättning: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.
> 
> 

## <a name="reference-topics"></a>Referensinformation:
hello efter referensavsnitt ge mer information om hur du använder jobb.

## <a name="jobs-tooexecute-direct-methods"></a>Jobb tooexecute direkt metoder
hello följer hello HTTP 1.1-begäran-information för att köra en [direkt metod] [ lnk-dev-methods] på en uppsättning enheter med hjälp av ett jobb:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
hello frågevillkoret kan också vara på en enda enhets-Id eller en lista över enhets-ID som visas nedan

**Exempel**
```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
[IoT-hubb Query Language] [ lnk-query] täcker IoT-hubb frågespråket i mer detalj.

## <a name="jobs-tooupdate-device-twin-properties"></a>Jobb tooupdate dubbla enhetsegenskaper
hello följer hello HTTP 1.1 Frågedetaljer för att uppdatera enheten två egenskaper med hjälp av ett jobb:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Frågar efter status för jobb
hello följer hello HTTP 1.1 Frågedetaljer för [frågar efter jobb][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

Hej continuationToken tillhandahålls av hello svar.  

## <a name="jobs-properties"></a>Egenskaper för jobb
hello följer en lista över egenskaper och motsvarande beskrivningar som kan användas när du frågar för jobb eller jobb resultat.

| Egenskap | Beskrivning |
| --- | --- |
| **jobId** |Programmet ange ID för hello jobb. |
| **startTime** |Program som starttid (ISO 8601) för hello jobbet. |
| **Sluttid** |IoT-hubb som avses datum (ISO 8601) när hello jobbet har slutförts. Gäller endast när hello jobbet når hello ”klar” tillstånd. |
| **typ** |Typer av jobb: |
| **scheduledUpdateTwin**: jobben-tooupdate en uppsättning egenskaper eller taggar. | |
| **scheduledDeviceMethod**: jobben-tooinvoke en enhet metod i en uppsättning twins för enheten. | |
| **status** |Hello jobbet aktuella tillstånd. Möjliga värden för status: |
| **väntande** : schemaläggs och väntar toobe tas upp av hello jobbet-tjänsten. | |
| **schemalagda** : schemalagd vid en tidpunkt i hello framtida. | |
| **kör** : aktiva jobb. | |
| **avbröt** : jobbet har avbrutits. | |
| **Det gick inte** : jobbet misslyckades. | |
| **slutföra** : jobbet har slutförts. | |
| **deviceJobStatistics** |Statistik om hello jobbkörningen. |

**deviceJobStatistics** egenskaper.

| Egenskap | Beskrivning |
| --- | --- |
| **deviceJobStatistics.deviceCount** |Antal enheter i hello-jobbet. |
| **deviceJobStatistics.failedCount** |Antal enheter där hello-jobbet misslyckades. |
| **deviceJobStatistics.succeededCount** |Antal enheter där hello jobbet lyckades. |
| **deviceJobStatistics.runningCount** |Antalet enheter som för närvarande körs hello jobb. |
| **deviceJobStatistics.pendingCount** |Antalet enheter som väntar på toorun hello jobb. |

### <a name="additional-reference-material"></a>Ytterligare referensmaterialet
Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:

* [IoT-hubbslutpunkter] [ lnk-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.
* [Begränsning och kvoter] [ lnk-quotas] beskriver hello kvoter som gäller toohello IoT-hubb-tjänsten och hello bandbreddsbegränsning beteende tooexpect när du använder hello-tjänsten.
* [Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] visar hello SDK: er för olika språk du en användning när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.
* [IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning] [ lnk-query] beskriver hello IoT-hubb frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb.
* [Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för hello MQTT protokollet.

## <a name="next-steps"></a>Nästa steg
Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb kursen:

* [Schemat och sändning jobb][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md

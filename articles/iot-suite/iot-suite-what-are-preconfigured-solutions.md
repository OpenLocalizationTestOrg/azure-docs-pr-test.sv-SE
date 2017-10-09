---
title: "aaaAzure IoT förkonfigurerade lösningar | Microsoft Docs"
description: "En beskrivning av hello Azure IoT förkonfigurerade lösningar och deras arkitektur med länkar tooadditional resurser."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: bd059d08ab458fdb0b6f49b3ac469db930dab09e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-hello-azure-iot-suite-preconfigured-solutions"></a>Vad är hello Azure IoT Suite förkonfigurerade lösningar?

hello Azure IoT Suite förkonfigurerade lösningar är implementeringar av vanliga mönster för IoT-lösningen som du distribuerar tooAzure med din prenumeration. Du kan använda hello förkonfigurerade lösningar:

* Som en startpunkt för dina egna IoT-lösningar.
* toolearn om vanliga mönster i IoT-lösningen och utveckling.

Varje förkonfigurerade lösningen är en fullständig, slutpunkt-till-slutpunkt-implementering som använder simulerade enheter toogenerate telemetri.

Du kan hämta hello fullständig källa kod toocustomize och utöka hello lösning toomeet dina specifika IoT-krav.

> [!NOTE]
> toodeploy något av hello förkonfigurerade lösningar, besök [Microsoft Azure IoT Suite][lnk-azureiotsuite]. hello artikel [Kom igång med hello IoT förkonfigurerade lösningar] [ lnk-getstarted-preconfigured] innehåller mer information om hur toodeploy och kör något av hello lösningar.

hello följande tabell visar hur hello lösningar mappa toospecific IoT-funktioner:

| Lösning | Datainhämtning | Enhetsidentitet | Enhetshantering | Kommando och kontroll | Regler och åtgärder | Förutsägelseanalys |
| --- | --- | --- | --- | --- | --- | --- |
| [Fjärrövervakning][lnk-getstarted-preconfigured] |Ja |Ja |Ja |Ja |Ja |- |
| [Förebyggande underhåll][lnk-predictive-maintenance] |Ja |Ja |- |Ja |Ja |Ja |
| [Ansluten fabrik][lnk-getstarted-factory] |Ja |Ja |Ja |Ja |Ja |- |

* *Datapåfyllning*: ingång av data vid skala toohello moln.
* *Enhetsidentitet*: hantera unikt enhets identiteter och styra enheten toohello lösning.
* *Enhetshantering*: Hantera enhetsmetadata och utför åtgärder som omstarter av enheten och uppgraderingar av den inbyggda programvaran.
* *Kommando- och*: toocause hello enheten tootake en åtgärd, skicka meddelanden tooa enheten från hello molnet.
* *Regler och åtgärder*: tooact på specifika enhet till moln data, hello lösningens serverdel använder regler.
* *Förutsägelseanalyser*: hello lösningens serverdel analyserar data för enhet till moln toopredict när specifika åtgärder ska utföras. Till exempel analysera flygplan motorn telemetri toodetermine när motorn Underhåll förfaller.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Översikt över den förkonfigurerade lösningen för fjärrövervakning

Vi har valt toodiscuss hello fjärråtkomst övervakning förkonfigurerade lösning i den här artikeln eftersom det visar många vanliga designelement som hello andra lösningar för resursen.

hello illustrerar följande diagram hello viktiga delar i hello remote övervakningslösning. hello innehåller följande avsnitt mer information om de här elementen.

![Arkitekturen i den förkonfigurerade lösningen för fjärrövervakning][img-remote-monitoring-arch]

## <a name="devices"></a>Enheter

När du distribuerar hello remote förkonfigurerade övervakningslösning är fyra simulerade enheter företablerad i hello-lösning som simulerar en kylningsenhet. Dessa simulerade enheter har en inbyggd temperatur- och fuktighetsmodell som genererar telemetri. Dessa simulerade enheter ingår:

- Visa hello slutpunkt till slutpunkt flödet av data via hello-lösning.
- Ange en lämplig källa för telemetri.
- Ange ett mål för metoder eller kommandon om du är en backend-utvecklare med hello lösningen som en startpunkt för en anpassad implementering.

hello simulerade enheter i hello-lösning kan svara toohello följande kommunikation moln till enhet:

- *Metoder ([direkt metoder][lnk-direct-methods])*: en dubbelriktad kommunikationsmetod där en ansluten enhet är förväntade toorespond omedelbart.
- *Kommandon (moln till enhet meddelanden)*: en enkelriktad kommunikationsmetod där en enhet hämtar hello-kommando från en beständig kö.

En jämförelse av de olika metoderna finns i [Cloud-to-device communications guidance][lnk-c2d-guidance] (Vägledning för kommunikation från moln till enhet).

När en enhet ansluter först tooIoT hubb i hello förkonfigurerade lösningen, skickar en enhet information meddelandet toohello hubb. Det här meddelandet räknar hello metoder hello enheten kan svara. I hello remote förkonfigurerade övervakningslösning, stöder simulerade enheter dessa metoder:

* *Initiera Firmware uppdatera*: den här metoden initierar en asynkron åtgärd på hello enheten tooperform en firmware-uppdatering. hello asynkrona aktiviteten använder rapporterade egenskaper toodeliver status uppdateringar toohello lösning instrumentpanelen.
* *Starta om*: den här metoden gör hello simulerade enheten tooreboot.
* *FactoryReset*: den här metoden utlöser en fabriksåterställning på hello simulerade enheten.

När en enhet ansluter först tooIoT hubb i hello förkonfigurerade lösningen, skickar en enhet information meddelandet toohello hubb. Det här meddelandet räknar hello kommandon hello enheten kan svara. I hello remote förkonfigurerade övervakningslösning, stöder simulerade enheter dessa kommandon:

* *Ping-enheten*: hello enheten svarar toothis kommandot med en bekräftelse. Det här kommandot är användbart för att kontrollera hello enheten är fortfarande aktivt och lyssnar.
* *Starta telemetri*: instruerar hello enheten toostart skicka telemetri.
* *Stoppa telemetri*: instruerar hello enheten toostop skicka telemetri.
* *Ändra Ställ utgångspunkt temperatur*: kontroller hello simulerade temperatur telemetri värden hello enheten skickar. Det här kommandot är användbart för att testa backend-logiken.
* *Diagnostiska telemetri*: styr om hello enheten ska skicka hello externa temperatur som telemetri.
* *Ändra enhetens tillstånd*: Anger hello tillstånd metadata enhetsegenskap som hello rapporter enheter. Det här kommandot är användbart för att testa backend-logiken.

Du kan lägga till flera simulerade enheterna toohello lösning som genererar hello samma telemetri och svara toohello samma metoder och kommandon.

Dessutom tooresponding toocommands och metoder hello lösningen använder [enhet twins][lnk-device-twin]. Enheterna använder enheten twins tooreport egenskapen värden toohello lösningens serverdel. hello lösning instrumentpanelen använder enheten twins tooset toonew önskad egenskapsvärden på enheter. Under hello firmware uppdatera processen hello simulerade enheten rapporter uppdatera hello status för hello med hjälp av rapporterade egenskaper.

## <a name="iot-hub"></a>IoT Hub

I den här förkonfigurerade lösningen hello IoT-hubb instans motsvarar toohello *Molngateway* i en typisk [IoT-lösningsarkitektur][lnk-what-is-azure-iot].

En IoT-hubb tar emot telemetri från hello enheter på en enda slutpunkt. En IoT-hubb har också enhetsspecifika slutpunkter där varje enhet kan hämta hello-kommandon som skickas tooit.

Hej IoT-hubb tillgängliggör hello emot telemetri via hello tjänsten på klientsidan telemetri läsa slutpunkt.

hello enheten hanteringsmöjligheter för IoT-hubb kan du toomanage egenskaper för din enhet från hello lösning portal och schema jobb som utför åtgärder som:

- Starta om enheter
- Ändra enhetstillstånd
- Uppdateringar av inbyggd programvara

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

hello förkonfigurerade lösningen använder tre [Azure Stream Analytics] [ lnk-asa] (ASA) jobb toofilter hello telemetri dataström från hello enheter:

* *DeviceInfo jobbet* -utdata data tooan Event hub som dirigerar enhetsregistret för enheten registreringen-specifika meddelanden toohello lösning. Det här enhetsregistret är en Azure Cosmos DB-databas. Dessa meddelanden skickas när en enhet ansluter först eller i svaret tooa **ändra enhetsstatus** kommando.
* *Telemetri jobbet* – skickar alla rådata telemetri tooAzure blob-lagring för kyla och beräknar telemetri aggregeringar som visas i instrumentpanelen för hello-lösning.
* *Regler jobbet* - filter hello telemetri strömmen för värden som överskrider tröskelvärden för varje regel och utdata hello data tooan Event hub. När en regel utlöses visas hello lösning portalens instrumentpanel den här händelsen som en ny rad i hello larm historiktabellen. Dessa regler kan även utlösa en åtgärd som baseras på hello inställningarna på hello **regler** och **åtgärder** vyer i hello lösning portal.

I den här förkonfigurerade lösningen hello ASA jobb utgör en del av toohello **IoT-lösningens serverdel** i en typisk [IoT-lösningsarkitektur][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Händelseprocessor

I den här förkonfigurerade lösningen hello Händelseprocessorn utgör en del av hello **IoT-lösningens serverdel** i en typisk [IoT-lösningsarkitektur][lnk-what-is-azure-iot].

Hej **DeviceInfo** och **regler** ASA jobb skickar sina utdata tooEvent NAV för leverans av tooother backend-tjänster. hello lösningen använder en [EventProcessorHost] [ lnk-event-processor] instans som körs en [Webbjobb][lnk-web-job], tooread hälsningsmeddelande från dessa händelsehubbar. Hej **EventProcessorHost** använder:
- Hej **DeviceInfo** data tooupdate hello enhetsdata hello Cosmos-DB-databas.
- Hej **regler** data tooinvoke hello logik app och uppdatera hello aviseringar visas i hello lösning portal.

## <a name="device-identity-registry-device-twin-and-cosmos-db"></a>Enhetsidentitetsregistret, enhetstvillingar och Cosmos DB

Varje IoT-hubb innehåller ett [enhetsidentitetsregister][lnk-identity-registry] som lagrar enhetsnycklar. Använder denna information för IoT-hubb autentisera enheter – en enhet måste registreras och har en giltig nyckel innan den kan ansluta toohello hubb.

En [enheten dubbla] [ lnk-device-twin] är ett JSON-dokument som hanteras av hello IoT-hubb. En enhetstvilling för en enhet innehåller:

- Rapporterat egenskaper som skickas av hello enheten toohello hubb. Du kan visa egenskaperna i hello lösning portal.
- Egenskaper som du vill toosend toohello enhet. Du kan ange dessa egenskaper i hello lösning portal.
- Taggar som finns bara i hello enheten dubbla och inte på hello enhet. Du kan använda dessa taggar toofilter listan med enheter i hello lösning portal.

Den här lösningen använder enheten twins toomanage enhetens metadata. hello lösningen använder också en Cosmos-DB databasen toostore ytterligare Lösningsspecifika enhetsdata, till exempel hello-kommandon som stöds av varje enhet och hello kommandohistoriken.

hello lösning måste också hålla hello information i hello enhetsidentitetsregistret synkroniseras med hello innehållet i hello Cosmos-DB-databas. Hej **EventProcessorHost** använder hello data från **DeviceInfo** stream analytics-jobbet toomanage hello synkronisering.

## <a name="solution-portal"></a>Lösningsportal

![lösningsportal][img-dashboard]

hello lösning portal är ett webbaserat gränssnitt som är distribuerade toohello moln som en del av hello förkonfigurerade lösningen. Här kan du:

* Visa telemetri och larmhistorik på en instrumentpanel.
* Etablera nya enheter.
* Hantera och övervaka enheter.
* Skicka kommandon toospecific enheter.
* Anropa metoder på specifika enheter.
* Hantera regler och åtgärder.
* Schemalägga jobb toorun på en eller flera enheter.

I den här förkonfigurerade lösningen hello lösning portal utgör en del av hello **IoT-lösningens serverdel** och en del av hello **bearbetnings-och** i hello typiska [IoT-lösningen arkitektur för][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Nästa steg

Mer information om IoT-lösningsarkitekturer finns i [Microsoft Azure IoT-tjänster: referensarkitektur][lnk-refarch].

Nu du vet vilka förkonfigurerade lösningen är att komma igång genom att distribuera hello *fjärrövervaknings* förkonfigurerade lösningen: [Kom igång med hello förkonfigurerade lösningar] [ lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-getstarted-factory]: iot-suite-connected-factory-overview.md

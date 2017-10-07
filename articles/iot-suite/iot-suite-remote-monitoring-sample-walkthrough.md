---
title: "aaaRemote övervakning förkonfigurerade lösningen genomgången | Microsoft Docs"
description: "En beskrivning av hello Azure IoT förkonfigurerade lösningen fjärråtkomst övervakning och dess arkitektur."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 57a336bd94938c2b9ee5d3456ea8e45446cf3d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Genomgång av den förkonfigurerade lösningen för fjärrövervakning

Hej IoT Suite fjärrövervaknings [förkonfigurerade lösningen] [ lnk-preconfigured-solutions] är en implementering av en slutpunkt till slutpunkt övervakningslösning för flera datorer som körs på fjärrplatser. hello lösningen kombinerar Azure nyckeltjänster tooprovide en allmänna implementering av hello affärsscenario. Du kan använda hello lösning som en startpunkt för din egen implementering och [anpassa] [ lnk-customize] den toomeet egna specifika krav i företaget.

Den här artikeln guidar dig igenom några av hello viktiga delar i hello fjärråtkomst övervakning lösning tooenable toounderstand hur det fungerar. Med den här kunskapen kan du sedan:

* Felsöka problem i hello-lösning.
* Planera hur toocustomize toohello lösning toomeet egna specifika krav. 
* Utforma en egen IoT-lösning som använder Azure-tjänster.

## <a name="logical-architecture"></a>Logisk arkitektur

följande diagram hello beskrivs hello logiska komponenter av hello förkonfigurerade lösningen:

![Logisk arkitektur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a>Simulerade enheter

I hello förkonfigurerade lösningen representerar hello simulerade enhet en kylningsenhet (till exempel en byggnad luftkonditionering eller anläggning luften hanteringsenhet). När du distribuerar hello förkonfigurerade lösningen kan du också automatiskt etablera fyra simulerade enheter som kör i en [Azure Webjobs][lnk-webjobs]. hello simulerade enheter gör det lättare för du tooexplore hello beteendet för hello lösning utan hello måste toodeploy alla fysiska enheter. toodeploy en riktigt fysisk enhet finns hello [ansluta din enhet toohello remote förkonfigurerade övervakningslösning] [ lnk-connect-rm] kursen.

### <a name="device-to-cloud-messages"></a>Meddelanden från enheten till molnet

Varje simulerade enhet kan skicka hello följande meddelande typer tooIoT hubb:

| Meddelande | Beskrivning |
| --- | --- |
| Start |När hello enheten startar skickar den ett **enhetsinformation** meddelande som innehåller information om själva toohello serverdel. Dessa data innehåller hello enhets-id och en lista över hello kommandon och metoder hello enhet stöder. |
| Närvaro |En enhet skickar regelbundet en **förekomst** meddelande tooreport om hello förekomsten av en sensor känner av hello enhet. |
| Telemetri |En enhet skickar regelbundet en **telemetri** meddelande som rapporterar simulerade värden för hello temperatur- och fuktighetskonsekvens samlas in från hello enhet har simulerade sensorer. |

> [!NOTE]
> hello lösningen lagrar hello lista med kommandon som stöds av hello-enhet i en Cosmos-DB-databas och inte i hello enheten dubbla.

### <a name="properties-and-device-twins"></a>Egenskaper och enhetstvillingar

hello simulerade enheterna skickar hello följande enhet egenskaper toohello [dubbla] [ lnk-device-twins] i hello IoT-hubb som *rapporterade egenskaper*. hello enheten skickar rapporterade egenskaper vid start och svar tooa **ändra enhetens tillstånd** kommando eller metod.

| Egenskap | Syfte |
| --- | --- |
| Config.TelemetryInterval | Frekvens (sekunder) hello enheten skickar telemetri |
| Config.TemperatureMeanValue | Anger hello medelvärdet för hello simulerade temperatur telemetri |
| Device.DeviceID |ID som är angivna eller tilldelas när en enhet skapas i hello-lösning |
| Device.DeviceState | Tillstånd som rapporteras av hello-enhet |
| Device.CreatedTime |Tid hello enheten har skapats i hello-lösning |
| Device.StartupTime |Tid hello enheten startades |
| Device.LastDesiredPropertyChange |hello versionsnumret för senaste önskade hello-egenskapen ändras |
| Device.Location.Latitude |Latitud platsen för hello-enhet |
| Device.Location.Longitude |Longitud platsen för hello-enhet |
| System.Manufacturer |Enhetstillverkare |
| System.ModelNumber |Modellnumret för hello-enhet |
| System.SerialNumber |Serienumret för hello-enhet |
| System.FirmwareVersion |Aktuell version av den inbyggda programvaran på hello-enhet |
| System.Platform |Plattformsarkitektur av hello-enhet |
| System.Processor |Processorn körs hello-enhet |
| System.InstalledRAM |Mängden RAM-minne på hello-enhet |

hello simulator lägger dessa egenskaper i simulerade enheter med exempelvärden. Varje gång hello simulator initierar en simulerad enhet, hello enheten rapporterar hello fördefinierade metadata tooIoT hubb som rapporterades egenskaper. Rapporterat egenskaper kan bara uppdateras genom hello enhet. toochange rapporterade egenskapen du egenskapen en önskad i lösningen portal. Det är hello ansvar hello enheten:

1. Med jämna mellanrum att hämta egenskaper från hello IoT-hubb.
2. Uppdatera konfigurationen med hello önskad egenskapsvärde.
3. Skicka hello nya värdet tillbaka toohello navet som rapporterades egenskap.

Hello lösning instrumentpanel kan du använda *önskade egenskaper* tooset egenskaper på en enhet med hjälp av hello [enheten dubbla][lnk-device-twins]. En enhet läser vanligtvis ett önskade egenskapsvärde från hello hubb tooupdate dess interna tillståndet och rapporten hello ändra tillbaka som en egenskap för rapporterade.

> [!NOTE]
> hello simulerade enheten koden endast använder hello **Desired.Config.TemperatureMeanValue** och **Desired.Config.TelemetryInterval** egenskaper tooupdate hello rapporterade egenskaper som skickas tillbaka tooIoT hubb. Alla andra ändringsbegäranden för önskad egenskapen ignoreras i hello simulerade enheten.

### <a name="methods"></a>Metoder

hello simulerade enheter kan hantera hello följande metoder ([direkt metoder][lnk-direct-methods]) anropas från portalen för hello lösning hello IoT-hubb:

| Metod | Beskrivning |
| --- | --- |
| InitiateFirmwareUpdate |Instruerar hello enheten tooperform en firmware-uppdatering |
| Starta om |Instruerar hello enheten tooreboot |
| FactoryReset |Instruerar hello enheten tooperform en fabriksåterställa |

Vissa metoder använda rapporterade egenskaper tooreport på pågår. Till exempel hello **InitiateFirmwareUpdate** metoden simulerar körs hello update asynkront på hello enhet. hello-metoden returnerar omedelbart på hello enhet medan hello asynkrona aktiviteten fortsätter toosend statusuppdateringar tillbaka toohello lösningen en instrumentpanel med hjälp av rapporterade egenskaper.

### <a name="commands"></a>Kommandon

hello simulerade enheter kan hantera hello följande kommandon (moln till enhet meddelanden) som skickas från portalen för hello lösning hello IoT-hubb:

| Kommando | Beskrivning |
| --- | --- |
| PingDevice |Skickar en *ping* toohello enhet toocheck den är aktiv |
| StartTelemetry |Startar hello enhet som skickar telemetri |
| StopTelemetry |Stoppar hello enheten från att skicka telemetri |
| ChangeSetPointTemp |Ändringar hello set punktvärdet runt vilken hello slumpmässiga data som genereras |
| DiagnosticTelemetry |Utlösare hello enheten simulatorn toosend ett ytterligare telemetri värde (externalTemp) |
| ChangeDeviceState |Ändrar en egenskap för utökat läge för hello enhet och skickar hälsningsmeddelande enheten information från hello-enhet |

> [!NOTE]
> En jämförelse av dessa kommandon (meddelanden från molnet till enheten) och metoder (direkta metoder) finns i artikeln om [kommunikation mellan moln och enheter][lnk-c2d-guidance].

## <a name="iot-hub"></a>IoT Hub

Hej [IoT-hubb] [ lnk-iothub] en data som skickas från hello enheter i hello moln och gör den tillgänglig toohello Azure Stream Analytics (ASA) jobb. Varje stream ASA jobbet används en separat IoT-hubb konsumenten grupp tooread hello ström av meddelanden från dina enheter.

Hej IoT-hubb i hello-lösning också:

- Upprätthåller en identitetsregistret som lagrar hello-ID: n och autentiseringsnycklar för alla hello enheter tillåts tooconnect toohello portal. Du kan aktivera och inaktivera enheter via hello identitetsregistret.
- Skickar kommandon tooyour enheter åt hello lösning portal.
- Anropar metoder på dina enheter åt hello lösning portal.
- Att underhålla enhetstvillingar för alla registrerade enheter. En enhet dubbla lagrar hello egenskapsvärden som rapporterats av en enhet. En enhet dubbla lagrar också önskade egenskaper, ange hello lösning för i portalen, hello enheten tooretrieve vid nästa anslutning.
- Scheman jobb tooset egenskaper för flera enheter eller anropa metoder i flera enheter.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

I hello remote övervakningslösning, [Azure Stream Analytics] [ lnk-asa] (ASA) skickar enheten har tagits emot av hello IoT-hubb tooother backend-komponenter för bearbetning eller lagring. Olika ASA jobb utför funktioner baserat på hälsningsmeddelande hello innehåll.

**Jobbet 1: Enhetsinformationen** filtrerar informationsmeddelanden från hello inkommande meddelandeströmmen och skickar dem tooan Event Hub slutpunkt. En enhet skickar meddelanden med enheten vid start och svar tooa **SendDeviceInfo** kommando. Det här jobbet använder hello följande fråga definition tooidentify **enhetsinformation** meddelanden:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Det här jobbet skickar dess utdata tooan Event Hub för vidare bearbetning.

**Jobb 2: Regler** utvärderar inkommande telemetrivärden för temperatur och fuktighet mot tröskelvärdena för varje enhet. Tröskelvärdena är inställda i hello regler redigeraren i hello lösning användarportalen. Varje enhet/värde-par lagras med en tidsstämpel i en blobb som Stream Analytics läser in som **referensdata**. hello jobbet jämför ett tomt värde mot hello angiven tröskel för hello enhet. Om den överskrider hello ' >' villkor hello jobbet matar ut en **larm** händelse som anger att hello tröskelvärde har överskridits och ger hello enhet, värde och tidsstämpelvärden. Det här jobbet använder följande definition tooidentify telemetri frågemeddelanden som ska utlösa ett larm hello:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

hello jobbet skickar dess utdata tooan Event Hub för vidare bearbetning och sparar informationen om varje avisering tooblob lagring från där hello lösning portal kan läsa hello aviseringsinformation.

**Jobbet 3: Telemetri** fungerar på hello inkommande enheten telemetri ström på två sätt. hello skickar först alla telemetri meddelanden från hello enheter toopersistent blob storage för långsiktig lagring. hello beräknar andra fuktighet genomsnittlig, lägsta och högsta värden över ett skjutfönster fem minuter långa och skickar den här tooblob datalagring. hello lösning portal läser hello telemetridata från blob storage toopopulate hello diagram. Det här jobbet använder följande frågedefinitionen hello:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Händelsehubbar

Hej **enhetsinformation** och **regler** ASA jobb utdata sina data tooEvent Hubs tooreliably framåt på toohello **Händelseprocessorn** körs i hello Webbjobb.

## <a name="azure-storage"></a>Azure Storage

hello lösningen använder Azure blob storage toopersist alla hello rådata och sammanfattade telemetridata från hello enheter i hello-lösning. hello portal läser hello telemetridata från blob storage toopopulate hello diagram. toodisplay aviseringar hello lösning portal läser hello data från blob storage som registrerar konfigurerades telemetri värden som överskrider hello tröskelvärden. hello lösningen använder också blob storage toorecord hello tröskelvärden du angett i hello lösning portal.

## <a name="webjobs"></a>Webbjobb

Dessutom toohosting hello enheten simulatorer hello WebJobs i hello-lösning också värden hello **Händelseprocessorn** körs i en Azure-WebJob som hanterar svar för kommandot. Den använder kommandot svar meddelanden tooupdate hello kommandot enhetshistorik (som lagras i hello Cosmos-DB-databas).

## <a name="cosmos-db"></a>Cosmos DB

hello lösningen använder en Cosmos-DB toostore databasinformation om hello enheter anslutna toohello lösningen. Informationen omfattar hello historik av kommandon som skickas toodevices från hello lösning portal och metoder som anropas från hello lösning portal.

## <a name="solution-portal"></a>Lösningsportal

hello lösning portal är en webbapp som distribueras som en del av hello förkonfigurerade lösning. hello viktiga sidor i hello lösning portal är hello instrumentpanelen och hello enhetslistan.

### <a name="dashboard"></a>Instrumentpanel

Den här sidan i hello webbapp använder PowerBI javascript kontroller (se [PowerBI-visuell information lagringsplatsen](https://www.github.com/Microsoft/PowerBI-visuals)) toovisualize hello telemetridata från hello-enheter. hello lösningen använder hello ASA telemetri jobbet toowrite hello telemetri tooblob datalagring.

### <a name="device-list"></a>Enhetslista

Den här sidan i hello lösning portal kan du:

* Etablera en ny enhet. Den här åtgärden anger hello unikt enhets-id och genererar hello autentiseringsnyckel. Skriver information om hello enheten tooboth hello IoT-hubb identitetsregistret och hello Lösningsspecifika Cosmos-DB-databas.
* Hantera enhetsegenskaper. Den här åtgärden används för att visa befintliga egenskaper och uppdatera dem med nya egenskaper.
* Skicka kommandon tooa enhet.
* Visa historiken för hello-kommandot för en enhet.
* Aktivera och inaktivera enheter.

## <a name="next-steps"></a>Nästa steg

hello innehåller följande TechNet blogginlägg mer information om hello fjärråtkomst övervakning förkonfigurerade lösningen:

* [IoT Suite - Under hello huven - Fjärrövervaknings](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [IoT Suite - Remote Monitoring - Adding Live and Simulated Devices](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Du kan fortsätta komma igång med IoT Suite genom att läsa hello följande artiklar:

* [Ansluta din enhet toohello remote förkonfigurerade övervakningslösning][lnk-connect-rm]
* [Behörigheter för hello azureiotsuite.com plats][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twins]:  ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md

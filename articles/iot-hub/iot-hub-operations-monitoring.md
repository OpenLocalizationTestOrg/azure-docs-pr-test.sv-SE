---
title: "aaaAzure IoT-hubb operations övervakning | Microsoft Docs"
description: "Hur toouse Azure IoT Hub operations övervakning toomonitor hello status för åtgärder för din IoT-hubb i realtid."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a299f3a5-b14d-4586-9c3b-44aea14ed013
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.openlocfilehash: a0b233ef2d9bd0827e19fa30fdbdd49b2b61b813
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="iot-hub-operations-monitoring"></a>IoT-hubb operations övervakning

IoT-hubb operations övervakning kan du toomonitor hello status för åtgärder för din IoT-hubb i realtid. IoT-hubb spårar händelser över flera kategorier av åtgärder. Du kan välja att skicka händelser från en eller flera kategorier tooan slutpunkten för din IoT-hubb för bearbetning. Du kan övervaka hello data för fel eller ställa in mer komplexa bearbetning baserat på datamönster.

IoT-hubb övervakar sex kategorier av händelser:

* Enhetens identitet åtgärder
* Enhetstelemetrin
* Meddelanden moln till enhet
* Anslutningar
* Filöverföringar
* Meddelanderoutning

## <a name="how-tooenable-operations-monitoring"></a>Hur tooenable operations övervakning

1. Skapa en IoT-hubb. Du hittar anvisningar om hur toocreate en IoT-hubb i hello [Kom igång] [ lnk-get-started] guide.

1. Öppna hello bladet för din IoT-hubb. Därifrån klickar du på **Operations övervakning**.

    ![Åtkomståtgärder övervakning konfigurationen hello-portalen][1]

1. Välj hello övervakning av du vill toomonitor och klicka sedan på **spara**. hello händelser som är tillgängliga för att läsa från hello Event Hub-kompatibel slutpunkt som anges i **övervakningsinställningarna**. Hej IoT-hubb kallas `messages/operationsmonitoringevents`.

    ![Konfigurera operations övervakning på din IoT-hubb][2]

> [!NOTE]
> Att välja **utförlig** övervakning för hello **anslutningar** kategori orsakar IoT-hubb toogenerate ytterligare diagnostiska meddelanden. Alla kategorier, hello **utförlig** inställningsändringar hello mängd information IoT-hubb ingår i varje felmeddelande.

## <a name="event-categories-and-how-toouse-them"></a>Kategorier och hur toouse dem.

Varje operations övervakning kategorin spårar en annan typ av interaktion med IoT-hubb och varje övervakning kategori har ett schema som definierar hur händelser i den kategorin är strukturerad.

### <a name="device-identity-operations"></a>Enhetens identitet åtgärder

hello enhetskategori identitet operations spårar fel som uppstår när du försöker toocreate, uppdatera eller ta bort en post i registret för din IoT-hubb identitet. Spårning av den här kategorin är användbart för att etablera scenarier.

```json
{
    "time": "UTC timestamp",
        "operationName": "create",
        "category": "DeviceIdentityOperations",
        "level": "Error",
        "statusCode": 4XX,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "durationMs": 1234,
        "userAgent": "userAgent",
        "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a>Enhetstelemetrin

hello telemetri enhetskategori spårar fel som uppstår vid hello IoT-hubb och är relaterade toohello telemetri pipeline. Den här kategorin innehåller fel som uppstår när du skickar telemetriska händelser (till exempel begränsning) och ta emot telemetriska händelser (till exempel obehöriga läsare). Den här kategorin kan inte fånga fel som orsakats av kod som körs på själva hello-enheten.

```json
{
        "messageSizeInBytes": 1234,
        "batching": 0,
        "protocol": "Amqp",
        "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
        "time": "UTC timestamp",
        "operationName": "ingress",
        "category": "DeviceTelemetry",
        "level": "Error",
        "statusCode": 4XX,
        "statusType": 4XX001,
        "statusDescription": "MessageDescription",
        "deviceId": "device-ID",
        "EventProcessedUtcTime": "UTC timestamp",
        "PartitionId": 1,
        "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a>Moln till enhet kommandon

hello moln till enhet kommandon kategorin spårar fel som uppstår vid hello IoT-hubb och är relaterade toohello moln-till-enhetsmeddelande pipeline. Den här kategorin innehåller fel som uppstår när meddelanden moln till enhet (till exempel obehöriga avsändaren), ta emot meddelanden moln till enhet (till exempel leverans för många) och ta emot meddelandet moln till enhet feedback (t ex feedback har upphört att gälla). Den här kategorin fånga inte fel från en enhet som hanterar ett moln till enhet meddelande felaktigt om moln till enhet hälsningsmeddelande levereras.

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": “UTC timestamp"
}
```

### <a name="connections"></a>Anslutningar

hello anslutningar kategorin spårar fel som uppstår när enheter ansluta eller koppla från en IoT-hubb. Spårning av den här kategorin är användbar för att identifiera obehöriga anslutningsförsök och för att spåra när anslutningen bryts för enheter i områden i dålig anslutning.

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a>Filöverföringar

hello filen överför kategorin spårar fel som uppstår vid hello IoT-hubb och är relaterade toofile överför funktioner. Den här kategorin omfattar:

* Fel som inträffar med hello SAS URI, till exempel när det upphör att gälla innan en enhet meddelar hello hubb för en överförda.
* Det gick inte överföringar som rapporterats av hello enhet.
* Fel som uppstår när en fil inte hittas i lagringen under skapande av IoT-hubb notification meddelandet.

Den här kategorin kan inte fånga fel som uppstår direkt medan hello enheten laddar upp en fil toostorage.

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a>Meddelanderoutning

hello-meddelande routning kategorin spårar fel som inträffar när meddelandet väg utvärdering och slutpunkten hälsa som uppfattas av IoT-hubb. Den här kategorin innefattar händelser, t.ex. när en regel utvärderar för ”Odefinierad” när IoT-hubb markerar en slutpunkt som förlorade och andra fel togs emot från en slutpunkt. Den här kategorin omfattar inte felen om hälsningsmeddelande för sig (t.ex enheten begränsning fel) som har rapporterats under hello ”enhetstelemetrin” kategori.

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="view-events"></a>Visa händelser

Du kan använda hello *iothub explorer* verktyget tooquickly testa att din IoT-hubb genererar övervakning händelser. tooinstall hello verktyget, se hello instruktionerna i hello [iothub explorer] [ lnk-iothub-explorer] GitHub-lagringsplatsen.

1. Se till att hello **anslutningar** övervakning kategori har angetts för**utförlig** hello-portalen.

1. Kör hello efter kommandot tooread från hello övervakning slutpunkt i en kommandotolk:

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. Kör följande kommando toosimulate hello en enhet som skickar meddelanden från enhet till moln i en annan kommandotolk:

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. hello första kommandotolk visar hello övervakning händelser som hello simulerade enheten ansluter tooyour IoT-hubb.

## <a name="connect-toohello-monitoring-endpoint"></a>Ansluta toohello övervakning slutpunkt

hello övervakning slutpunkt på din IoT-hubb är Event Hub-kompatibel slutpunkt. Du kan använda någon mekanism som fungerar med Händelsehubbar tooread övervakning meddelanden från den här slutpunkten. hello skapar följande exempel en grundläggande läsare som inte lämpar sig för en distribution med hög genomströmning. Mer information om hur tooprocess meddelanden från Händelsehubbar finns hello [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] kursen.

tooconnect toohello övervakning slutpunkt, behöver du en sträng och hello endpoint anslutningsnamn. hello följande steg visar hur toofind hello nödvändiga värden i hello portal:

1. Navigera tooyour resursbladet för IoT-hubb i hello-portalen.

1. Välj **Operations övervakning**, och anteckna hello **Event Hub-kompatibelt namn** och **Event Hub-kompatibel endpoint** värden:

    ![Event Hub-kompatibel endpoint värden][img-endpoints]

1. Välj **principer för delad åtkomst**, Välj **tjänsten**. Anteckna hello **primärnyckel** värde:

    ![Primär nyckel för tjänsten delad åtkomst-princip][img-service-key]

hello följande kodexempel för C# hämtas från Visual Studio **klassiska Windows-skrivbordet** C#-konsolapp. hello-projekt har hello **WindowsAzure.ServiceBus** NuGet-paketet installeras.

* Ersätt hello anslutning sträng platshållaren med en anslutningssträng som använder hello **Event Hub-kompatibel endpoint** och tjänsten **primärnyckel** värden som du antecknade tidigare enligt följande hello Exempel:

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* Ersätt hello övervakning endpoint platshållare med hello **Event Hub-kompatibelt namn** värdet som du antecknade tidigare.

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key tooexit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();

        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }

        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }

            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));

            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
    }
}
```

## <a name="next-steps"></a>Nästa steg
toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Simulera en enhet med Azure IoT kant][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png
[img-endpoints]: media/iot-hub-operations-monitoring/monitoring-endpoint.png
[img-service-key]: media/iot-hub-operations-monitoring/service-key.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
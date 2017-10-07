---
title: "aaaGet igång med Azure IoT Hub (.NET) | Microsoft Docs"
description: "Lär dig hur toosend enhet till moln meddelanden tooAzure IoT-hubb med IoT-SDK för .NET. Skapa simulerade enheten och tjänsten appar tooregister enheten, skicka meddelanden och läsa meddelanden från IoT-hubb."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56cf14687411898ea0fa4ebb1782e18b3930809c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-tooyour-iot-hub-using-net"></a>Ansluta din enhet tooyour IoT-hubb med hjälp av .NET

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Hello slutet av den här självstudiekursen har du tre .NET konsolappar:

* **CreateDeviceIdentity**, vilket skapar en enhetsidentitet och tillhörande nyckeln tooconnect din enhetsapp.
* **ReadDeviceToCloudMessages**, som visar hello telemetri som skickats av din enhetsapp.
* **SimulatedDevice**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och skickar meddelandet telemetri varje sekund med hello MQTT-protokollet.

Du kan hämta eller klona hello Visual Studio-lösning som innehåller hello tre appar från Github.

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> Information om hello Azure IoT-SDK: er som du kan använda toobuild både program toorun på enheter och din lösningens serverdel finns [Azure IoT SDK][lnk-hub-sdks].

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nu har du skapat din IoT-hubb och du har hello värdnamn och IoT-hubb anslutningssträngen som du behöver toocomplete hello resten av den här kursen.

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a>Ta emot meddelanden från enheten till molnet
I det här avsnittet ska du skapa en .NET-konsolapp som läser ”enhet till molnet”-meddelanden från IoT Hub. En IoT-hubb Exponerar en [Azure Event Hubs][lnk-event-hubs-overview]-kompatibel endpoint tooenable du tooread meddelanden från enhet till moln. enkel tookeep saker, den här guiden skapar en grundläggande läsare som inte lämpar sig för en distribution med hög genomströmning. toolearn hur tooprocess enhet till moln meddelanden i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen. Mer information om hur tooprocess meddelanden från Händelsehubbar finns hello [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] kursen. (Den här kursen är tillämpliga toohello IoT-hubb Event Hub-kompatibel slutpunkter.)

> [!NOTE]
> hello använder Event Hub-kompatibel slutpunkt för att läsa meddelanden från enhet till moln alltid hello AMQP-protokollet.

1. I Visual Studio att lägga till en Visual C# klassiska skrivbordet projektet toohello aktuella-lösning med hello **Konsolapp (.NET Framework)** projektmall. Se till att hello av .NET Framework 4.5.1 eller senare. Namnet hello projektet **ReadDeviceToCloudMessages**.

    ![Nytt Visual C# Windows Classic Desktop-projekt][10a]

2. I Solution Explorer högerklickar du på hello **ReadDeviceToCloudMessages** projektet och klicka sedan på **hantera NuGet-paket**.

3. I hello **NuGet Package Manager** fönster, söka efter **WindowsAzure.ServiceBus**väljer **installera**, och Godkänn hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens för[Azure Service Bus][lnk-servicebus-nuget], med dess beroenden. Det här paketet kan hello programmet tooconnect toohello Event Hub-kompatibel slutpunkt på din IoT-hubb.

4. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello hub du skapat i hello avsnittet ”Skapa en IoT-hubb”.

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. Lägg till följande metod toohello hello **programmet** klass:

    ```csharp
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested) break;
            EventData eventData = await eventHubReceiver.ReceiveAsync();
            if (eventData == null) continue;

            string data = Encoding.UTF8.GetString(eventData.GetBytes());
            Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
        }
    }
    ```

    Den här metoden använder en **EventHubReceiver** instans tooreceive meddelanden från alla hello IoT hub-enhet till moln får partitioner. Observera hur du skickar en `DateTime.Now` parameter när du skapar hello **EventHubReceiver** objektet, så att den bara tar emot meddelanden som skickas när den har startat. Det här filtret är användbart i en testmiljö så att du kan se hello aktuella uppsättning av meddelanden. Koden bör se till att den bearbetar alla hälsningsmeddelande i en produktionsmiljö. Mer information finns i självstudiekursen hello [hur tooprocess IoT-hubb meddelanden från enhet till moln][lnk-process-d2c-tutorial].

7. Slutligen lägger du till följande rader toohello hello **Main** metoden:

    ```csharp
    Console.WriteLine("Receive messages. Ctrl-C tooexit.\n");
    eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

    var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

    CancellationTokenSource cts = new CancellationTokenSource();

    System.Console.CancelKeyPress += (s, e) =>
    {
        e.Cancel = true;
        cts.Cancel();
        Console.WriteLine("Exiting...");
    };

    var tasks = new List<Task>();
    foreach (string partition in d2cPartitions)
    {
        tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
    }  
    Task.WaitAll(tasks.ToArray());
    ```

## <a name="create-a-device-app"></a>Skapa en enhetsapp

I det här avsnittet skapar du en .NET-konsolapp som simulerar en enhet som skickar meddelanden från enhet till moln tooan IoT-hubb.

1. I Visual Studio att lägga till en Visual C# klassiska skrivbordet projektet toohello aktuella-lösning med hello **Konsolapp (.NET Framework)** projektmall. Se till att hello av .NET Framework 4.5.1 eller senare. Namnet hello projektet **SimulatedDevice**.

    ![Nytt Visual C# Windows Classic Desktop-projekt][10b]

2. I Solution Explorer högerklickar du på hello **SimulatedDevice** projektet och klicka sedan på **hantera NuGet-paket**.

3. I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **Microsoft.Azure.Devices.Client**väljer **installera** tooinstall hello **Microsoft.Azure.Devices.Client** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [SDK NuGet-paketet för Azure IoT-enhet] [ lnk-device-nuget] och dess beroenden.

4. Lägg till följande hello `using` instruktionen hello överst i hello **Program.cs** fil:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. Lägg till följande fält toohello hello **programmet** klass. Ersätt `{iot hub hostname}` med hello IoT hub-värdnamnet du hämtade i hello ”skapa en IoT-hubb”. Ersätt `{device key}` med hello enhetsnyckel som du hämtade i hello ”skapa en enhetsidentitet”.

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. Lägg till följande metod toohello hello **programmet** klass:

    ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        double minTemperature = 20;
        double minHumidity = 60;
        int messageId = 1;
        Random rand = new Random();

        while (true)
        {
            double currentTemperature = minTemperature + rand.NextDouble() * 15;
            double currentHumidity = minHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
                messageId = messageId++,
                deviceId = "myFirstDevice",
                temperature = currentTemperature,
                humidity = currentHumidity
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
    ```

    Den här metoden skickar ett nytt ”enhet till molnet”-meddelande varje sekund. hello-meddelande innehåller en JSON-serialiserade objekt med hello enhets-ID och slumpmässigt genererade siffror toosimulate en temperatursensor och en fuktighet sensor.

7. Slutligen lägger du till följande rader toohello hello **Main** metoden:

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);

    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    Som standard hello **skapa** metod i en app för .NET Framework skapar en **DeviceClient** -instans som använder hello AMQP protokollet toocommunicate med IoT-hubben. toouse hello MQTT eller HTTP-protokollet använder hello åsidosättning av hello **skapa** metod som du kan använda toospecify hello-protokollet. UWP och PCL klienter använder hello HTTP-protokollet som standard. Om du använder hello HTTP-protokollet bör du också lägga till hello **Microsoft.AspNet.WebApi.Client** NuGet-paketet tooyour projekt tooinclude hello **System.Net.Http.Formatting** namnområde.

Den här kursen tar dig igenom hello steg toocreate en IoT-hubb enhetsapp. Du kan också använda hello [ansluten tjänst för Azure IoT Hub] [ lnk-connected-service] Visual Studio-tillägget tooadd hello nödvändig kod tooyour enhetsapp.

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].

## <a name="run-hello-apps"></a>Köra hello appar

Du är nu redo toorun hello appar.

1. Högerklicka på din lösning i Solution Explorer i Visual Studio och klicka sedan på **Ange startprojekt**. Välj **flera Startprojekt**, och välj sedan **starta** som hello åtgärd för båda hello **ReadDeviceToCloudMessages** och **SimulatedDevice** projekt.

    ![Egenskaper för startprojekt][41]

2. Tryck på **F5** toostart både appar som körs. Hej konsolens utdata från hello **SimulatedDevice** visar hello meddelanden appen enheten skickar tooyour IoT-hubb. Hej konsolens utdata från hello **ReadDeviceToCloudMessages** app visar hälsningsmeddelande som tar emot din IoT-hubb.

    ![Konsolens utdata från appar][42]

3. Hej **användning** panelen i hello [Azure-portalen] [ lnk-portal] visar hello antalet meddelanden som skickas toohello IoT-hubb:

    ![Panelen Användning på Azure Portal][43]

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen konfigurerade en IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du har använt den här enhetens identitet tooenable hello enheten app toosend meddelanden från enhet till moln toohello IoT-hubb. Du kan också skapat en app som visar hello som tagits emot av hello IoT-hubb.

toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:

* [Connecting your device][lnk-connect-device] (Ansluta din enhet)
* [Connecting your device][lnk-device-management] (Komma igång med enhetshantering)
* [Komma igång med IoT Edge][lnk-iot-edge]

toolearn hur tooextend IoT-lösningen och processen enhet till moln meddelandena i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10a]: ./media/iot-hub-csharp-csharp-getstarted/create-receive-csharp1.png
[10b]: ./media/iot-hub-csharp-csharp-getstarted/create-device-csharp1.png


<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

---
title: "aaaProcess Azure IoT Hub-enhet till moln meddelanden med hjälp av vägar (.Net) | Microsoft Docs"
description: "Hur meddelanden tooprocess IoT-hubb meddelanden från enhet till moln med hjälp av regler för Routning och anpassade slutpunkter toodispatch tooother backend-tjänster."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: c1dd5be04ca30c65af2be466ba6c8c1858333154
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a>IoT-hubb enhet till moln-meddelanden med hjälp av vägar (.NET)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Den här kursen bygger på hello [Kom igång med IoT-hubb] kursen. hello Självstudier:

* Visar hur toouse routning regler toodispatch meddelanden från enhet till moln i ett enkelt och konfigurationsbaserade sätt.
* Illustrerar hur tooisolate interaktiva meddelanden som kräver omedelbar åtgärd från hello lösning serverdel för vidare bearbetning. En enhet kan till exempel skicka larmmeddelandet som utlöser lägga till ett ärende i ett CRM-system. Däremot-datapunkt meddelanden, till exempel temperatur telemetri, mata in en analytics-motorn.

Hello slutet av den här självstudiekursen, kan du köra tre .NET konsolappar:

* **SimulatedDevice**, en modifierad version av hello-app som skapats i hello [Kom igång med IoT-hubb] självstudiekursen skickar meddelanden från enhet till moln datapunkt varje sekund och interaktiva enhet till moln meddelanden var 10 sekunder.
* **ReadDeviceToCloudMessages** som visar hello icke-kritiska telemetri som skickats av din enhetsapp.
* **ReadCriticalQueue** tar bort hello viktiga meddelanden som skickas av din enhetsapp från en Service Bus-kö. Den här kön är anslutna toohello IoT-hubb.

> [!NOTE]
> IoT-hubben har SDK stöd för många vilka plattformar och språk, inklusive C, Java och JavaScript. toolearn hur tooreplace hello simulerade enheten med en fysisk enhet i den här kursen finns hello [Azure IoT Developer Center].

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Ett aktivt Azure-konto. <br/>Om du inte har ett konto kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter.

Du bör ha vissa grundläggande kunskaper om [Azure Storage] och [Azure Service Bus].

## <a name="send-interactive-messages"></a>Skicka interaktiva meddelanden

Ändra hello enhetsapp som du skapade i hello [Kom igång med IoT-hubb] självstudiekursen toooccasionally skicka interaktiva meddelanden.

I Visual Studio i hello **SimulatedDevice** projekt, Ersätt hello `SendDeviceToCloudMessagesAsync` metod med hello följande kod:

```csharp
private static async void SendDeviceToCloudMessagesAsync()
{
    double minTemperature = 20;
    double minHumidity = 60;
    Random rand = new Random();

    while (true)
    {
        double currentTemperature = minTemperature + rand.NextDouble() * 15;
        double currentHumidity = minHumidity + rand.NextDouble() * 20;

        var telemetryDataPoint = new
        {
            deviceId = "myFirstDevice",
            temperature = currentTemperature,
            humidity = currentHumidity
        };
        var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
        string levelValue;

        if (rand.NextDouble() > 0.7)
        {
            messageString = "This is a critical message";
            levelValue = "critical";
        }
        else
        {
            levelValue = "normal";
        }
        
        var message = new Message(Encoding.ASCII.GetBytes(messageString));
        message.Properties.Add("level", levelValue);
        
        await deviceClient.SendEventAsync(message);
        Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

        await Task.Delay(1000);
    }
}
```

Den här metoden lägger slumpmässigt hello egenskapen `"level": "critical"` toomessages som skickas av hello enhet som simulerar ett meddelande som kräver omedelbara åtgärder med hello lösning serverdel. Hej enhetsapp Överför denna information i Egenskaper för hello-meddelande, i stället för i hello meddelandetexten, så att IoT-hubb kan vidarebefordra hello meddelandets toohello rätt meddelande mål.

> [!NOTE]
> Du kan använda egenskaper tooroute meddelanden för olika scenarier, inklusive kall sökväg, bearbetning, dessutom toohello hot sökväg exemplet som visas här.

> [!NOTE]
> Den här självstudiekursen implementerar inte några återförsöksprincip på hello ut för enkelhetens skull. I produktionskod, bör du implementera en återförsöksprincip som exponentiell backoff enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].

## <a name="route-messages-tooa-queue-in-your-iot-hub"></a>Väg tooa kön i din IoT-hubb

I det här avsnittet får du:

* Skapa en Service Bus-kö.
* Anslut den tooyour IoT-hubb.
* Konfigurera din IoT-hubb toosend meddelanden toohello kö baserat på hello förekomsten av en egenskap på hello-meddelande.

Mer information om hur tooprocess meddelanden från Service Bus-köer finns [Kom igång med köer][Service Bus queue].

1. Skapa en Service Bus-kö som beskrivs i [Kom igång med köer][Service Bus queue]. hello kön måste vara i hello samma prenumeration och region som din IoT-hubb. Anteckna hello namnområde och kön namn.

    > [!NOTE]
    > Service Bus-köer och ämnen som används som IoT-hubbslutpunkter inte får ha **sessioner** eller **dubblettidentifiering** aktiverat. Om något av dessa alternativ är aktiverade hello ändpunkt som **inte åtkomlig** i hello Azure-portalen.

2. I hello Azure-portalen, öppna din IoT-hubb och klickar på **slutpunkter**.
    
    ![Slutpunkter i IoT-hubb][30]

3. I hello **slutpunkter** bladet, klickar du på **Lägg till** på hello översta tooadd kön tooyour IoT-hubb. Namnet hello endpoint **CriticalQueue** och använda hello-listrutor tooselect **Service Bus-kö**hello Service Bus-namnrymd som kön finns och hello namnet på din kö. När du är klar klickar du på **spara** längst ned hello.
    
    ![Lägga till en slutpunkt][31]
    
4. Klicka på **vägar** i din IoT-hubb. Klicka på **Lägg till** hello överst i hello bladet toocreate en routningsregel som skickar meddelanden toohello kö du just lagt till. Välj **DeviceTelemetry** som hello datakälla. Ange `level="critical"` som hello villkor och välj hello kö som du just har lagt till som en anpassad slutpunkt som hello routning regeln slutpunkt. När du är klar klickar du på **spara** längst ned hello.
    
    ![Lägga till en väg][32]
    
    Kontrollera hello återställningsplats vägen anges för**på**. Det här värdet är hello standardkonfigurationen för en IoT-hubb.
    
    ![Fallback-väg][33]

## <a name="read-from-hello-queue-endpoint"></a>Läsa från hello kön slutpunkt

I det här avsnittet läsa hälsningsmeddelande från hello kön slutpunkt.

1. I Visual Studio att lägga till en Visual C# klassiska skrivbordet projektet toohello aktuella-lösning med hello **Konsolapp (.NET Framework)** projektmall. Namnet hello projektet **ReadCriticalQueue**.

2. I Solution Explorer högerklickar du på hello **ReadCriticalQueue** projektet och klicka sedan på **hantera NuGet-paket**. Den här åtgärden visar hello **NuGet Package Manager** fönster.

3. Sök efter **WindowsAzure.ServiceBus**, klickar du på **installera**, och Godkänn hello villkor för användning. Den här åtgärden hämtar, installerar och lägger till en referens toohello Azure Service Bus med dess beroenden.

4. Lägg till följande hello **med** instruktioner överst hello i hello **Program.cs** fil:
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Slutligen lägger du till följande rader toohello hello **Main** metod. Ersätt hello-anslutningssträngen med **lyssna** behörigheter för hello kö:
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C tooexit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-hello-applications"></a>Köra hello program
Nu är du redo toorun hello program.

1. Högerklicka på lösningen i Visual Studio i Solution Explorer och markera **ange Startprojekt**. Välj **flera Startprojekt**och välj **starta** som hello åtgärd för hello **ReadDeviceToCloudMessages**, **SimulatedDevice**, och **ReadCriticalQueue** projekt.
2. Tryck på **F5** toostart hello tre konsolappar. Hej **ReadDeviceToCloudMessages** appen har endast icke-kritiska meddelanden skickas från hello **SimulatedDevice** program och hello **ReadCriticalQueue** app har bara kritiska meddelanden.
   
   ![Tre konsolappar][50]

## <a name="next-steps"></a>Nästa steg
I kursen får du lärt dig hur tooreliably skicka meddelanden från enhet till moln med hjälp av hello meddelandet routningsfunktionen IoT-hubb.

Hej [hur toosend moln till enhet meddelanden med IoT-hubb] [ lnk-c2d] visar hur toosend meddelanden tooyour enheter från din lösningens serverdel.

toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite][lnk-suite].

toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].

toolearn mer om meddelanderoutning i IoT Hub, se [skicka och ta emot meddelanden med IoT-hubben][lnk-devguide-messaging].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md
[Kom igång med IoT-hubb]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

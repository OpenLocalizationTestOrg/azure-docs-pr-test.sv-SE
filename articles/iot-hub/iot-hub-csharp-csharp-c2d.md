---
title: aaaCloud till enhet meddelanden med Azure IoT Hub (.NET) | Microsoft Docs
description: "Hur meddelanden toosend moln till enhet tooa enhet från en Azure IoT-hubb med hello Azure IoT SDK för .NET. Du ändrar en enhet tooreceive moln till enhet meddelanden och ändra en backend-app toosend moln till enhet hälsningsmeddelande."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: a31c05ed-6ec0-40f3-99ab-8fdd28b1a89a
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: f6a7618b164d95c8ddaf28943f244aeeb568217f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-messages-from-hello-cloud-tooyour-device-with-iot-hub-net"></a>Skicka meddelanden från hello moln tooyour enhet med IoT Hub (.NET)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Introduktion
Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas. Hej [Kom igång med IoT-hubb] kursen visar hur toocreate en IoT-hubb etablera en enhetsidentitet i den och code en enhetsapp som skickar meddelanden från enhet till moln.

Den här kursen bygger på [Kom igång med IoT-hubb]. Den visar hur till:

* Skicka meddelanden moln till enhet tooa enhet via IoT-hubb från din lösningens serverdel.
* Ta emot meddelanden moln till enhet på en enhet.
* Begära leverans bekräftelse från din lösningens serverdel (*feedback*) för meddelanden som skickas tooa enheten från IoT-hubb.

Du hittar mer information om moln till enhet meddelanden i hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].

Hello slutet av den här självstudiekursen kör du två .NET konsolappar:

* **SimulatedDevice**, en modifierad version av hello-app som skapats i [Kom igång med IoT-hubb], som ansluter tooyour IoT-hubb och tar emot meddelanden moln till enhet.
* **SendCloudToDevice**, som skickar ett moln-till-enhetsmeddelande toohello enheten appen via IoT-hubb och tar emot dess leverans bekräftelse.

> [!NOTE]
> IoT-hubben har SDK stöd för många vilka plattformar och språk (inklusive C, Java och Javascript) via [Azure IoT-enhet SDK]. Stegvisa instruktioner om hur tooconnect enhet toothis kursens kod och vanligtvis tooAzure IoT Hub, se hello [IoT-hubb Utvecklarhandbok].
> 
> 

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

## <a name="receive-messages-in-hello-device-app"></a>Ta emot meddelanden i hello enhetsapp
I det här avsnittet ska du ändra hello enhetsapp som du skapade i [Kom igång med IoT-hubb] tooreceive moln till enhet meddelanden från hello IoT-hubb.

1. I Visual Studio i hello **SimulatedDevice** projekt, Lägg till följande metod toohello hello **programmet** klass.
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud toodevice messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    Hej `ReceiveAsync` returnerar metoden asynkront emot hälsningsmeddelande hello samtidigt som den tas emot av hello enhet. Den returnerar *null* efter en specifiable tidsgräns (i det här fallet används hello standardvärdet på en minut). När hello appen tar emot en *null*, den ska fortsätta toowait för nya meddelanden. Det här kravet är hello orsak hello `if (receivedMessage == null) continue` rad.
   
    Hej anrop för`CompleteAsync()` meddelar IoT-hubb hello meddelandet har bearbetats. hello-meddelande kan tas bort på ett säkert sätt från hello enhetskö. Om något hände förhindrade hello enheten appen från att slutföras hello bearbetningen av hello-meddelande ger IoT-hubb den igen. Sedan är det viktigt att meddelandebehandling logiken i hello enhetsapp är *idempotent*, så att ta emot hello samma meddelande flera gånger producerar hello samma resultat. Ett program kan också tillfälligt Avbryt ett meddelande som resulterar i IoT-hubb behålla hello-meddelande i hello kö för framtida användning. Eller hello programmet kan avvisa ett meddelande som tar permanent bort hello-meddelande från kön hello. Mer information om hello moln-till-enhetsmeddelande livscykel finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].
   
   > [!NOTE]
   > När du använder HTTP i stället för MQTT eller AMQP som transport, hello `ReceiveAsync` metoden returnerar omedelbart. hello stöds mönstret för moln till enhet meddelanden med HTTP är periodvis anslutna enheter som kontrollerar för meddelanden sällan (mindre än var 25: e minut). Utfärda mer HTTP får resultat i IoT-hubb bandbreddsbegränsning hello-begäranden. Mer information om hello skillnader mellan MQTT, AMQP och HTTP-stöd och IoT-hubb begränsning finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].
   > 
   > 
2. Lägg till följande metod i hello hello **Main** metoden precis före hello `Console.ReadLine()` rad:
   
        ReceiveC2dAsync();

> [!NOTE]
> Den här självstudiekursen implementerar inte några återförsöksprincip sätt. I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].
> 
> 

## <a name="send-a-cloud-to-device-message"></a>Skicka ett meddelande moln till enhet
I det här avsnittet kan du skriva en .NET-konsolapp som skickar meddelanden moln till enhet toohello enhetsapp.

1. Skapa ett Visual C#-Skrivbordsapprojekt-projekt i hello aktuella Visual Studio-lösning med hjälp av hello **konsolprogram** projektmall. Namnet hello projektet **SendCloudToDevice**.
   
    ![Nytt projekt i Visual Studio][20]
2. Högerklicka på hello lösningen i Solution Explorer och klicka sedan på **hantera NuGet-paket för lösningen...** . 
   
    Den här åtgärden öppnar hello **hantera NuGet-paket** fönster.
3. Sök efter **Microsoft.Azure.Devices**, klickar du på **installera**, och Godkänn hello villkor för användning. 
   
    Detta hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK NuGet-paketet].

4. Lägg till följande hello `using` instruktionen hello överst i hello **Program.cs** fil:
   
        using Microsoft.Azure.Devices;
5. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållare värdet med hello IoT hub-anslutningssträng från [Kom igång med IoT-hubb]:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Lägg till följande metod toohello hello **programmet** klass:
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud toodevice message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    Den här metoden skickar en ny moln-till-enhetsmeddelande toohello enhet med hello ID `myFirstDevice`. Ändra den här parametern endast om den ändrats från hello som används i [Kom igång med IoT-hubb].
7. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key toosend a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. I Visual Studio högerklickar du på din lösning och välj **ange Startprojekt...** . Välj **flera Startprojekt**och välj hello **starta** åtgärd för **ReadDeviceToCloudMessages**, **SimulatedDevice**, och **SendCloudToDevice**.
9. Tryck på **F5**. Alla tre program ska starta. Välj hello **SendCloudToDevice** windows och tryck på **RETUR**. Du bör se hello-meddelande tas emot av hello enhetsapp.
   
   ![Appen tar emot meddelandet][21]

## <a name="receive-delivery-feedback"></a>Ta emot leverans feedback
Det är möjligt toorequest leverans (eller sista) bekräftelser från IoT-hubb för varje moln till enhet-meddelande. Det här alternativet aktiverar hello lösning serverdel tooeasily informera logik för återförsök eller ersättning. Mer information om moln till enhet feedback finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].

I det här avsnittet kan du ändra hello **SendCloudToDevice** app toorequest feedback och tar emot det från IoT-hubb.

1. I Visual Studio i hello **SendCloudToDevice** projekt, Lägg till följande metod toohello hello **programmet** klass.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    Obs den här ta emot mönstret är hello samma en används tooreceive moln till enhet meddelanden från hello enhetsapp.
2. Lägg till följande metod i hello hello **Main** metod, direkt efter hello `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` rad:
   
        ReceiveFeedbackAsync();
3. toorequest feedback för hello leverans av meddelandet moln till enhet du har toospecify en egenskap i hello **SendCloudToDeviceMessageAsync** metod. Lägg till följande rad direkt efter hello hello `var commandMessage = new Message(...);` rad:
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. Köra hello appar genom att trycka på **F5**. Du bör se alla tre program startar. Välj hello **SendCloudToDevice** windows och tryck på **RETUR**. Du bör se hello meddelandet tas emot av hello enhetsapp och efter några sekunder, hello feedback meddelandet tas emot av din **SendCloudToDevice** program.
   
   ![Appen tar emot meddelandet][22]

> [!NOTE]
> Den här självstudiekursen implementerar inte några återförsöksprincip sätt. I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].
> 
> 

## <a name="next-steps"></a>Nästa steg
I kursen får du lära sig hur toosend och ta emot meddelanden moln till enhet. 

toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite].

toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT service SDK NuGet-paketet]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md

[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md
[Kom igång med IoT-hubb]: iot-hub-csharp-csharp-getstarted.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://docs.microsoft.com/en-us/azure/iot-suite/
[Azure IoT-enhet SDK]: iot-hub-devguide-sdks.md
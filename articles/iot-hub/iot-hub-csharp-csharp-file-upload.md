---
title: "aaaUpload filer från enheter tooAzure IoT-hubb med .NET | Microsoft Docs"
description: "Hur tooupload filer från en enhet toohello moln med Azure IoT-enhet SDK för .NET. Överförda filer lagras i ett Azure storage blob-behållaren."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: elioda
ms.openlocfilehash: 07d555f6ba8b067bbd3233bc8eebaa220ad2388b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub-using-net"></a>Ladda upp filer från din enhet toohello moln med IoT-hubben med hjälp av .NET

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Den här kursen bygger på hello koden i hello [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) självstudiekursen tooshow du hur toouse hello filen överför funktionerna i IoT-hubb. Den visar hur till:

- Ange en enhet på ett säkert sätt med en Azure blob-URI för att överföra en fil.
- Använd hello IoT-hubb överför meddelanden tootrigger filbearbetning hello-filen i din app-serverdelen.

Hej [Kom igång med IoT-hubb](iot-hub-csharp-csharp-getstarted.md) och [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) självstudiekurser visar hello grundläggande enhet till moln och moln till enhet meddelandefunktioner för IoT-hubb. Hej [meddelanden processen enhet till moln](iot-hub-csharp-csharp-process-d2c.md) självstudiekursen beskriver ett sätt tooreliably spara enhet till moln meddelanden i Azure blob storage. Men i vissa fall kan du enkelt kartdata hello enheterna skickar till relativt liten enhet till moln hälsningsmeddelande som accepterar IoT-hubb. Exempel:

* Stora filer som innehåller bilder
* Videoklipp
* Vibration data samplas vid hög frekvens
* Någon form av förbearbetade data

Dessa filer finns vanligtvis i hello molnet med hjälp av verktyg som bearbetas i batch [Azure Data Factory](../data-factory/index.md) eller hello [Hadoop](../hdinsight/index.md) stacken. När du behöver tooupload filer från en enhet, kan du fortfarande använda hello säkerheten och pålitligheten för IoT-hubb.

Hello slutet av den här kursen kan du köra två .NET konsolappar:

* **SimulatedDevice**, en modifierad version av hello-app som skapats i hello [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) kursen. Den här appen överför en fil toostorage med en SAS-URI som tillhandahålls av din IoT-hubb.
* **ReadFileUploadNotification**, som tar emot filen överföra meddelanden från din IoT-hubb.

> [!NOTE]
> IoT-hubb stöder många vilka plattformar och språk (inklusive C, Java och Javascript) via Azure IoT-enhet SDK: er. Se toohello [Azure IoT Developer Center] stegvisa anvisningar för hur tooconnect din enhet tooAzure IoT-hubb.

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Överför en fil från en enhetsapp

I det här avsnittet kan du ändra hello enhetsapp som du skapade i [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) tooreceive moln till enhet meddelanden från hello IoT-hubb.

1. I Visual Studio högerklickar du på hello **SimulatedDevice** projektet, klicka på **Lägg till**, och klicka sedan på **befintlig artikel**. Navigera tooan image-filen och inkludera den i projektet. Den här självstudiekursen förutsätts hello avbildningen heter `image.jpg`.

1. Högerklicka på hello avbildningen och klicka på **egenskaper**. Se till att **kopiera tooOutput Directory** har angetts för**Kopiera alltid**.

    ![][1]

1. I hello **Program.cs** lägger du till följande instruktioner hello överst i filen hello hello:

    ```csharp
    using System.IO;
    ```

1. Lägg till följande metod toohello hello **programmet** klass:

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time tooupload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    Hej `UploadToBlobAsync` metoden tar i hello filnamnet och dataström källan för hello filen toobe överförs och hanterar hello överför toostorage. hello konsolprogram visar hello tid det tar tooupload hello-filen.

1. Lägg till följande metod i hello hello **Main** metoden precis före hello `Console.ReadLine()` rad:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Den här självstudiekursen implementerar inte några återförsöksprincip sätt. I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].

## <a name="receive-a-file-upload-notification"></a>Ta emot ett meddelande om överföringen

I det här avsnittet kan du skriva en .NET-konsolapp som tar emot filen överföra meddelanden från IoT-hubb.

1. Skapa ett Visual C# Windows-projekt i hello aktuella Visual Studio-lösning med hjälp av hello **konsolprogram** projektmall. Namnet hello projektet **ReadFileUploadNotification**.

    ![Nytt projekt i Visual Studio][2]

1. I Solution Explorer högerklickar du på hello **ReadFileUploadNotification** projektet och klicka sedan på **hantera NuGet-paket...** .

1. I hello **NuGet Package Manager** fönster, söka efter **Microsoft.Azure.Devices**, klickar du på **installera**, och Godkänn hello villkor för användning.

    Den här åtgärden hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK NuGet-paketet] i hello **ReadFileUploadNotification** projekt.

1. I hello **Program.cs** lägger du till följande instruktioner hello överst i filen hello hello:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållare värdet med hello IoT hub-anslutningssträng från [Kom igång med IoT-hubb]:

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. Lägg till följande metod toohello hello **programmet** klass:

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    Obs den här ta emot mönstret är hello samma en används tooreceive moln till enhet meddelanden från hello enhetsapp.

1. Slutligen lägger du till följande rader toohello hello **Main** metoden:

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter tooexit\n");
    Console.ReadLine();
    ```

## <a name="run-hello-applications"></a>Köra hello program

Nu är du redo toorun hello program.

1. Högerklicka på lösningen i Visual Studio och välj **ange Startprojekt**. Välj **flera Startprojekt**och välj hello **starta** åtgärd för **ReadFileUploadNotification** och **SimulatedDevice**.

1. Tryck på **F5**. Båda programmen ska starta. Du bör se hello överföringen har slutförts i ett konsolprogram och hello överför meddelandet tas emot av hello andra konsolprogram. Du kan använda hello [Azure-portalen] eller Visual Studio Server Explorer toocheck hello förekomst av hello överförs filen i ditt Azure Storage-konto.

    ![][50]

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig hur toouse hello filöverföring funktionerna i IoT-hubb toosimplify filöverföringar från enheter. Du kan fortsätta tooexplore IoT-hubb funktioner och scenarier med hello följande artiklar:

* [Skapa en IoT-hubb programmässigt][lnk-create-hub]
* [Introduktion tooC SDK][lnk-c-sdk]
* [Azure IoT-SDK][lnk-sdks]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Simulera en enhet med IoT kant][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png

<!-- Links -->

[Azure-portalen]: https://portal.azure.com/

[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure IoT service SDK NuGet-paketet]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md

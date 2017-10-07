---
title: aaaUse Azure IoT Hub direkt metoder (.NET/.NET) | Microsoft Docs
description: "Hur toouse Azure IoT Hub direkt metoder. Du kan använda hello Azure IoT-enhet SDK för .NET tooimplement en simulerad enhetsapp som innehåller en direkt metod och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som anropar hello direkta metoden."
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: dkshir
ms.openlocfilehash: d4fa093a99558ec6faf294c2583a14a722b9ac03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnet"></a>Använda direkt metoder (.NET/.NET)
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

I den här kursen är vi kommer toodevelop två .NET konsolappar:

* **CallMethodOnDevice**, en backend-app, som anropar en metod i hello simulerade enhetsapp och visar hello svar.
* **SimulateDeviceMethods**, en konsolapp som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och svarar toohello-metoden anropas av hello molnet.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.
> 
> 

toocomplete den här kursen behöver du:

* Visual Studio 2015 eller Visual Studio 2017.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Om du vill toocreate hello enhetsidentitet programmässigt i stället kan du läsa hello motsvarande avsnitt i hello [ansluta din simulerade enhet tooyour IoT-hubb med hjälp av .NET] [ lnk-device-identity-csharp] artikel.


## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
I det här avsnittet skapar du en .NET-konsolapp som svarar tooa metoden anropas av hello-lösningen slutet.

1. I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall. Namnet hello projektet **SimulateDeviceMethods**.
   
    ![Ny Visual C# Windows Classic enhetsapp][img-createdeviceapp]
    
1. I Solution Explorer högerklickar du på hello **SimulateDeviceMethods** projektet och klicka sedan på **hantera NuGet-paket...** .
1. I hello **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices.client**. Välj **installera** tooinstall hello **Microsoft.Azure.Devices.Client** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT-enhet SDK] [ lnk-nuget-client-sdk] NuGet-paketet och dess beroenden.
   
    ![NuGet-Pakethanteraren fönstret-klientappen][img-clientnuget]
1. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;

1. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållarvärde med anslutningssträngen för hello enheten som du antecknade i hello föregående avsnitt.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. Lägg till hello följande tooimplement hello direkt metod på hello enhet:

        static Task<MethodResponse> WriteLineToConsole(MethodRequest methodRequest, object userContext)
        {
            Console.WriteLine();
            Console.WriteLine("\t{0}", methodRequest.DataAsJson);
            Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);

            string result = "'Input was written toolog.'";
            return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
        }

1. Slutligen lägger du till följande kod toohello hello **Main** metoden tooopen hello anslutning tooyour IoT hub och initiera hello metoden lyssnare:
   
        try
        {
            Console.WriteLine("Connecting toohub");
            Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

            // setup callback for "writeLine" method
            Client.SetMethodHandlerAsync("writeLine", WriteLineToConsole, null).Wait();
            Console.WriteLine("Waiting for direct method call\n Press enter tooexit.");
            Console.ReadLine();

            Console.WriteLine("Exiting...");

            // as a good practice, remove hello "writeLine" handler
            Client.SetMethodHandlerAsync("writeLine", null, null).Wait();
            Client.CloseAsync().Wait();
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }
        
1. I hello Visual Studio Solution Explorer högerklickar du på din lösning och klicka sedan på **ange Startprojekt...** . Välj **enda Startprojekt**, och välj sedan hello **SimulateDeviceMethods** projekt i hello nedrullningsbara menyn.        

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel anslutning försök), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Anropa en metod som direkt på en enhet
I det här avsnittet skapar du en .NET-konsolapp som anropar en metod i hello simulerade enheten appen och sedan visar hello svar.

1. I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall. Se till att hello av .NET Framework 4.5.1 eller senare. Namnet hello projektet **CallMethodOnDevice**.
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createserviceapp]
2. I Solution Explorer högerklickar du på hello **CallMethodOnDevice** projektet och klicka sedan på **hantera NuGet-paket...** .
3. I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]

4. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices;
5. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Lägg till följande metod toohello hello **programmet** klass:
   
        private static async Task InvokeMethod()
        {
            var methodInvocation = new CloudToDeviceMethod("writeLine") { ResponseTimeout = TimeSpan.FromSeconds(30) };
            methodInvocation.SetPayloadJson("'a line toobe written'");

            var response = await serviceClient.InvokeDeviceMethodAsync("myDeviceId", methodInvocation);

            Console.WriteLine("Response status: {0}, payload:", response.Status);
            Console.WriteLine(response.GetPayloadAsJson());
        }
   
    Den här metoden startar direkt metod med namnet `writeLine` på hello `myDeviceId` enhet. Sedan skriver hello-svar som tillhandahålls av hello enheten på hello-konsolen. Observera hur det är möjligt toospecify ett timeout-värde för hello enheten toorespond.
7. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        InvokeMethod().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. I hello Visual Studio Solution Explorer högerklickar du på din lösning och klicka sedan på **ange Startprojekt...** . Välj **enda Startprojekt**, och välj sedan hello **CallMethodOnDevice** projekt i hello nedrullningsbara menyn.

## <a name="run-hello-applications"></a>Köra hello program
Du är nu redo toorun hello program.

1. Kör hello .NET enhetsapp **SimulateDeviceMethods**. Den ska börja lyssna efter metodanrop från din IoT-hubb: 

    ![Enhetsapp kör][img-deviceapprun]
1. Nu hello enheten är ansluten och väntar på att metoden anrop, kör hello .NET **CallMethodOnDevice** tooinvoke hello mobilappmetoden i hello simulerade enhetsapp. Du bör se hello enhetens svar skrivs i hello-konsolen.
   
    ![Kör Service-appen][img-serviceapprun]
1. hello enheten reagerar sedan toohello metoden genom att skriva ut det här meddelandet:
   
    ![Direkta metoden anropas på hello-enhet][img-directmethodinvoked]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du har använt den här enheten identitet tooenable hello simulerade enheten app tooreact toomethods anropas av hello molnet. Skapade du även en app som anropar metoder på hello enhet och visar hello svar från hello enhet. 

toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:

* [Kom igång med IoT-hubb]
* [Schema-jobb på flera enheter][lnk-devguide-jobs]

toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.

<!-- Images. -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-direct-methods/device-app-nuget.png
[img-createserviceapp]: ./media/iot-hub-csharp-csharp-direct-methods/create-service-app.png
[img-servicenuget]: ./media/iot-hub-csharp-csharp-direct-methods/service-app-nuget.png
[img-deviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-device-app.png
[img-serviceapprun]: ./media/iot-hub-csharp-csharp-direct-methods/run-service-app.png
[img-directmethodinvoked]: ./media/iot-hub-csharp-csharp-direct-methods/direct-method-invoked.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[Kom igång med IoT-hubb]: iot-hub-node-node-getstarted.md

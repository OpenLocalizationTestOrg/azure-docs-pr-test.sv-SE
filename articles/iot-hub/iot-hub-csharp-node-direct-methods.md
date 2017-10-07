---
title: aaaUse Azure IoT Hub direkt metoder (.NET/nod) | Microsoft Docs
description: "Hur toouse Azure IoT Hub direkt metoder. Du kan använda hello Azure IoT-enhet SDK för Node.js tooimplement en simulerad enhetsapp som innehåller en direkt metod och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som anropar hello direkta metoden."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: nberdy
ms.openlocfilehash: f566f939be840eb308b00ffa4e05c4e5b3fefb39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-netnode"></a>Använda direkt metoder (.NET/nod)
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

I den här självstudiekursen kommer vi toodevelop en .NET och Node.js-konsolapp:

* **CallMethodOnDevice.sln**, en .NET backend-app, som anropar en metod i hello simulerade enhetsapp och visar hello svar.
* **SimulatedDevice.js**, en Node.js-app som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och svarar toohello-metoden anropas av hello molnet.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.
> 
> 

toocomplete den här kursen behöver du:

* Visual Studio 2015 eller Visual Studio 2017.
* Node.js version 0.10.x eller senare.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
I det här avsnittet skapar du en Node.js-konsolprogram som svarar tooa metoden anropas av hello-lösningen slutet.

1. Skapa en ny tom mapp med namnet **simulateddevice**. I hello **simulateddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** och **azure-iot-enhet – mqtt** paket:
   
    ```
        npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Med hjälp av en textredigerare, skapa en fil i hello **simulateddevice** mapp och ger den namnet **SimulatedDevice.js**.
4. Lägg till följande hello `require` uttryck högst hello början av hello **SimulatedDevice.js** fil:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Lägg till en **connectionString** variabel och använda den toocreate en **DeviceClient** instans. Ersätt **{enheten anslutningssträngen}** med hello enheten anslutningssträng som du genererade i hello *skapa en enhetsidentitet* avsnitt:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Lägg till hello följande funktion tooimplement hello direkt metod på hello enhet:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Öppna hello anslutning tooyour IoT-hubb och initiera lyssnaren för hello-metoden:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Spara och Stäng hello **SimulatedDevice.js** fil.

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel anslutning försök), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].
> 
> 

## <a name="call-a-direct-method-on-a-device"></a>Anropa en metod som direkt på en enhet
I det här avsnittet skapar du en .NET-konsolapp som anropar en metod i hello simulerade enheten appen och sedan visar hello svar.

1. I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall. Se till att hello av .NET Framework 4.5.1 eller senare. Namnet hello projektet **CallMethodOnDevice**.
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][10]
2. I Solution Explorer högerklickar du på hello **CallMethodOnDevice** projektet och klicka sedan på **hantera NuGet-paket...** .
3. I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.
   
    ![Fönstret för NuGet-pakethanteraren][11]

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

## <a name="run-hello-applications"></a>Köra hello program
Du är nu redo toorun hello program.

1. I hello Visual Studio Solution Explorer högerklickar du på din lösning och klicka sedan på **ange Startprojekt...** . Välj **enda Startprojekt**, och välj sedan hello **CallMethodOnDevice** projekt i hello nedrullningsbara menyn.

2. Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando toostart lyssnar efter metodanrop från din IoT-hubb hello:
   
    ```
    node SimulatedDevice.js
    ```
   Vänta tills hello simulerade enheten tooopen:![][7]
3. Nu hello enheten är ansluten och väntar på att metoden anrop, kör hello .NET **CallMethodOnDevice** tooinvoke hello mobilappmetoden i hello simulerade enhetsapp. Du bör se hello enhetens svar skrivs i hello-konsolen.
   
    ![][8]
4. hello enheten reagerar sedan toohello metoden genom att skriva ut det här meddelandet:
   
    ![][9]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du har använt den här enheten identitet tooenable hello simulerade enheten app tooreact toomethods anropas av hello molnet. Skapade du även en app som anropar metoder på hello enhet och visar hello svar från hello enhet. 

toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:

* [Kom igång med IoT-hubb]
* [Schema-jobb på flera enheter][lnk-devguide-jobs]

toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.

<!-- Images. -->
[7]: ./media/iot-hub-csharp-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-csharp-node-direct-methods/netserviceapp.png
[9]: ./media/iot-hub-csharp-node-direct-methods/methods-output.png

[10]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp1.png
[11]: ./media/iot-hub-csharp-node-direct-methods/direct-methods-csharp2.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Kom igång med IoT-hubb]: iot-hub-node-node-getstarted.md

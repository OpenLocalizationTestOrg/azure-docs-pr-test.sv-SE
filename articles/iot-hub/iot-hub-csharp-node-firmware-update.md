---
title: aaaDevice firmware-uppdatering med Azure IoT Hub (.NET/nod) | Microsoft Docs
description: "Hur uppdaterar toouse enhetshantering i Azure IoT Hub tooinitiate en enhetens inbyggda programvara. Du använder hello Azure IoT-enhet SDK för Node.js tooimplement en simulerad enhetsapp och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som utlöser hello firmware-uppdatering."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/17/2017
ms.author: juanpere
ms.openlocfilehash: d48899651bcd5d67e577c971be0f9453c8f21592
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-device-management-tooinitiate-a-device-firmware-update-netnode"></a>Använda device management tooinitiate en enhet på en firmware-uppdatering (.NET/nod)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Introduktion
I hello [komma igång med hantering av enheter] [ lnk-dm-getstarted] självstudiekursen kommer du sett hur toouse hello [enheten dubbla] [ lnk-devtwin] och [direkt metoder] [ lnk-c2dmethod] primitiver tooremotely starta om en enhet. Den här självstudiekursen använder hello samma IoT-hubb primitiver och visar hur toodo en slutpunkt till slutpunkt simulerade firmware-uppdatering.  Det här mönstret används i hello uppdatering av inbyggd hanteringsprogramvara för hello [hallon Pi enheten implementering exempel][lnk-rpi-implementation].

I den här självstudiekursen lär du dig att:

* Skapa en .NET-konsolapp som anropar hello firmwareUpdate direkt metod i hello simulerade enheten appen via din IoT-hubb.
* Skapa en simulerad enhetsapp som implementerar en **firmwareUpdate** direkt metod. Den här metoden initierar en process i flera steg som väntar toodownload hello firmware bild, laddar hello firmware avbildningen och slutligen gäller hello firmware avbildningen. Under varje steg i hello update rapporterade hello enheten använder hello egenskaper tooreport på pågår.

Hello slutet av den här självstudiekursen har du en enhet för Node.js konsolapp och en serverdel för .NET (C#) konsolapp:

**dmpatterns_fwupdate_service.js**, som anropar en direkt metod i hello simulerade enhetsapp visar hello svar och regelbundet (varje 500ms) visar hello uppdateras rapporterade egenskaper.

**TriggerFWUpdate**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare, tar emot en firmwareUpdate direkt metod, körs via en flera tillstånd processen toosimulate inklusive en inbyggd uppdatering: väntar på hello avbildningen hämtas hämtar hello ny avbildning och slutligen använder hello-bild.

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Node.js-version 0.12.x eller senare <br/>  [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

Följ hello [Kom igång med enhetshantering](iot-hub-csharp-node-device-management-get-started.md) artikel toocreate din IoT-hubb och hämta anslutningssträngen för IoT-hubb.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a>Utlös en fjärransluten firmware-uppdatering på hello-enhet med en direkt metod
I det här avsnittet skapar du en .NET-konsolapp (med C#) som initierar en fjärransluten firmware-uppdatering på en enhet. hello appen använder en direkta metoden tooinitiate hello uppdatering och använder enheten dubbla frågor tooperiodically få hello status för hello active firmware-uppdatering.

1. I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall. Namnet hello projektet **TriggerFWUpdate**.

    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]

1. I Solution Explorer högerklickar du på hello **TriggerFWUpdate** projektet och klicka sedan på **hantera NuGet-paket...** .
1. I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.

    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
1. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello flera platshållarvärdena med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello och hello-Id för din enhet.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
1. Lägg till följande metod toohello hello **programmet** klass:
   
        public static async Task QueryTwinFWUpdateReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
1. Lägg till följande metod toohello hello **programmet** klass:

        public static async Task StartFirmwareUpdate()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);
            method.SetPayloadJson(
                @"{
                    fwPackageUri : 'https://someurl'
                }");

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

1. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartFirmwareUpdate().Wait();
        QueryTwinFWUpdateReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
1. Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **TriggerFWUpdate** projektet är **starta**.

1. Skapa hello lösning.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a>Köra hello appar
Du är nu redo toorun hello appar.

1. Kommandotolken hello i hello **manageddevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. I Visual Studio högerklickar du på hello **TriggerFWUpdate** projectRun toohello C#-konsolapp, Välj **felsöka** och **Starta ny instans**.

3. Du ser hello enheten svar toohello direkt metod i hello-konsolen.

    ![Inbyggd programvara har uppdaterats][img-fwupdate]

## <a name="next-steps"></a>Nästa steg
I kursen får du använde en direkta metoden tootrigger en fjärransluten firmware-uppdatering på en enhet och använda hello rapporterade egenskaper toofollow hello fortskrider hello firmware-uppdatering.

toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-firmware-update/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-firmware-update/createnetapp.png
[img-fwupdate]: media/iot-hub-csharp-node-firmware-update/fwupdated.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
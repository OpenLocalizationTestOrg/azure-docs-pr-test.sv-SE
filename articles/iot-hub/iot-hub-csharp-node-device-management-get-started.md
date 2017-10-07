---
title: "aaaGet igång med Azure IoT Hub enhetshantering (.NET/nod) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet management tooinitiate fjärranslutna enheten startas om. Du kan använda hello Azure IoT-enhet SDK för Node.js tooimplement en simulerad enhetsapp som innehåller en direkt metod och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som anropar hello direkta metoden."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2016
ms.author: juanpere
ms.openlocfilehash: ea6d50dfac1c222d7836e3bf5503c6c9b8c89dfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-netnode"></a>Komma igång med hantering av enheter (.NET/nod)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

I den här självstudiekursen lär du dig att:

* Använd hello Azure portal toocreate en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.
* Skapa en simulerad enhetsapp som innehåller en direkt metod som startar om enheten. Direkta metoder anropas från hello molnet.
* Skapa en .NET-konsolapp som anropar hello omstart direkt metod i hello simulerade enheten appen via din IoT-hubb.

Hello slutet av den här självstudiekursen har du en enhet för Node.js konsolapp och en serverdel för .NET (C#) konsolapp:

**dmpatterns_getstarted_device.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare, tar emot en direkt metod för omstart simulerar en fysisk omstart och rapporterar hello tid för hello senaste omstarten.

**TriggerReboot**, som anropar en direkt metod i hello simulerade enhetsapp visar hello svar och visar hello uppdateras rapporterade egenskaper.

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Node.js-version 0.12.x eller senare <br/>  [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a>Utlös en fjärransluten omstart på hello-enhet med en direkt metod
I det här avsnittet skapar du en .NET-konsolapp (med C#) som initierar en fjärransluten omstart på en enhet med en direkt metod. hello appen använder enheten dubbla frågor toodiscover hello omstart senast för enheten.

1. Lägga till en ny lösning för Visual C# klassiska skrivbordet projektet tooa i Visual Studio med hello **Konsolapp (.NET Framework)** projektmall. Se till att hello av .NET Framework 4.5.1 eller senare. Namnet hello projektet **TriggerReboot**.

    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]

2. I Solution Explorer högerklickar du på hello **TriggerReboot** projektet och klicka sedan på **hantera NuGet-paket**.
3. I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.

    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
4. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello och hello målenhet.
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. Lägg till följande metod toohello hello **programmet** klass.  Den här koden hämtar hello enheten dubbla för hello startas om enheten och utdata hello rapporterade egenskaper.
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. Lägg till följande metod toohello hello **programmet** klass.  Den här koden initierar hello omstart på hello-enhet med en direkt metod.

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
8. Skapa hello lösning.

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
I det här avsnittet kommer du att

* Skapa en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln
* Utlös en simulerad enhet-omstart
* Använd hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast startas om

1. Skapa en ny tom mapp som kallas **manageddevice**.  I hello **manageddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.  Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **manageddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Med hjälp av en textredigerare, skapa en ny **dmpatterns_getstarted_device.js** filen i hello **manageddevice** mapp.
4. Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_getstarted_device.js** fil:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans.  Ersätt hello anslutningssträngen med anslutningssträngen enhet.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Lägg till hello följande funktion tooimplement hello direkt metod på hello-enhet
   
    ```
    var onReboot = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report hello reboot before hello physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. Lägg till följande hello code tooopen hello anslutning tooyour IoT-hubb och starta hello direkta metoden lyssnare:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. Spara och Stäng hello **dmpatterns_getstarted_device.js** fil.
   
> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].


## <a name="run-hello-apps"></a>Köra hello appar
Du är nu redo toorun hello appar.

1. Kommandotolken hello i hello **manageddevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Kör hello C#-konsolapp **TriggerReboot**. Högerklicka på hello **TriggerReboot** projektet, Välj **felsöka**, och välj sedan **Starta ny instans**.

3. Du ser hello enheten svar toohello direkt metod i hello-konsolen.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
---
title: "aaaGet igång med Azure IoT Hub-enhet twins (.NET/nod) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooadd taggar och sedan använda en IoT-hubb-fråga. Du kan använda hello Azure IoT-enhet SDK för Node.js tooimplement hello simulerade enheten appen och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som lägger till hello taggar och kör hello IoT-hubb frågan."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: elioda
ms.openlocfilehash: 1cec082ebddc19c9b87998a5fd0159d32b07acd8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-netnode"></a>Kom igång med enheten twins (.NET/nod)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Hello slutet av den här självstudiekursen har du en .NET och Node.js-konsolapp:

* **AddTagsAndQuery.sln**, en .NET backend-app som lägger till taggar och frågar enheten twins.
* **TwinSimulatedDevice.js**, en Node.js-app som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och rapporterar tillståndet anslutningen.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.
> 
> 

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Node.js version 0.10.x eller senare.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a>Skapa hello service-appen
I det här avsnittet skapar du en .NET-konsolapp (med C#) som lägger till platsen metadata toohello enheten dubbla som är associerade med **myDeviceId**. Därefter frågor hello enheten twins lagras i hello IoT-hubb hello enheter finns i hello oss och hello som rapporterats av en mobil anslutning.

1. I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall. Namnet hello projektet **AddTagsAndQuery**.
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]
1. I Solution Explorer högerklickar du på hello **AddTagsAndQuery** projektet och klicka sedan på **hantera NuGet-paket...** .
1. I hello **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices**. Välj **installera** tooinstall hello **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
        using Microsoft.Azure.Devices;
1. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Lägg till följande metod toohello hello **programmet** klass:
   
        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }
   
    Hej **RegistryManager** klassen visar alla hello metoder krävs toointeract med enheten twins från hello-tjänsten. hello föregående kod först initierar hello **registryManager** objekt, och sedan hämtar hello enheten dubbla för **myDeviceId**, och slutligen uppdateras taggarna hello önskad platsinformation.
   
    När du har uppdaterat, körs två frågor: hello väljer först endast hello enheten twins av enheter som finns i hello **Redmond43** anläggningar och hello andra förfinar hello frågan tooselect bara hello enheter som även är anslutna via mobilnät.
   
    Observera att hello föregående kod när den skapar hello **frågan** objekt, anger ett maximalt antal returnerade dokument. Hej **frågan** objektet innehåller en **HasMoreResults** boolesk egenskap som du kan använda tooinvoke hello **GetNextAsTwinAsync** metoder flera gånger tooretrieve alla resultat. En metod som kallas **GetNextAsJson** är tillgänglig för resultat som är inte enheten twins, till exempel resultatet av aggregering frågor.
1. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **AddTagsAndQuery** projektet är **starta**. Skapa hello lösning.
1. Kör det här programmet genom att högerklicka på hello **AddTagsAndQuery** projekt och välja **felsöka**, följt av **Starta ny instans**. Du bör se en enhet i hello resultat för hello frågan ställa för alla enheter i **Redmond43** och ingen för hello-fråga som begränsar hello resultatet toodevices som använder ett mobilnät.
   
    ![Resultatet av frågan i fönstret][img-addtagapp]

I nästa avsnitt om hello, skapar du en enhetsapp som rapporterar hello anslutningsinformation och ändringar hello resultatet av frågan hello hello föregående avsnitt.

## <a name="create-hello-device-app"></a>Skapa hello enhetsapp
I det här avsnittet skapar du en Node.js-konsolprogram som ansluter tooyour hubb som **myDeviceId**, och sedan uppdaterar rapporterade egenskaper toocontain hello informationen att servern är ansluten med hjälp av ett mobilnät.

1. Skapa en ny tom mapp som kallas **reportconnectivity**. I hello **reportconnectivity** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello.
   
    ```
    npm init
    ```
1. Vid en kommandotolk i hello **reportconnectivity** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet**, och **azure-iot-enhet – mqtt** paketet :
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Med hjälp av en textredigerare, skapa en ny **ReportConnectivity.js** filen i hello **reportconnectivity** mapp.
1. Lägg till följande kod toohello hello **ReportConnectivity.js** filen och ersätta hello platshållare för enheten anslutningssträngen med hello något du kopierade när du skapade hello **myDeviceId** enhet identitet:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    Hej **klienten** objektet innehåller alla hello-metoder som du behöver toointeract med enheten twins från hello enhet. Hej föregående kod när den initierar hello **klienten** objekt, hämtar hello enheten dubbla för **myDeviceId** och uppdaterar egenskapen rapporterade med hello anslutningsinformation.
1. Kör hello enhetsapp
   
        node ReportConnectivity.js
   
    Du bör se hello-meddelande `twin state reported`.
1. Nu när hello enheten rapporterade dess anslutningsinformation, bör den visas i båda frågor. Kör hello .NET **AddTagsAndQuery** app toorun hello frågar igen. Den här gången **myDeviceId** ska visas i båda frågeresultaten.
   
    ![][img-addtagapp2]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du lagt till enhetsmetadata som taggar från en backend-app och skrev en simulerad enhet app tooreport anslutning enhetsinformation i hello enheten dubbla. Du också lära sig hur tooquery informationen med hjälp av frågespråket för hello SQL-liknande IoT-hubb.

Använd hello följande resurser toolearn hur till:

* Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen
* Konfigurera enheter med hjälp av enheten två egenskaper med hello [Använd önskad egenskaper tooconfigure enheter] [ lnk-twin-how-to-configure] självstudiekursen
* Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app) med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md


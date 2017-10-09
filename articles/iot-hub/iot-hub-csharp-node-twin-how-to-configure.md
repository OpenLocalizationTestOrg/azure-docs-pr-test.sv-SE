---
title: "Egenskaper för aaaUse Azure IoT Hub enhet dubbla (.NET/nod) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooconfigure enheter. Du använder hello Azure IoT-enhet SDK för Node.js tooimplement en simulerad enhetsapp och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som ändrar en konfiguration för enheter med hjälp av en enhet dubbla."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: elioda
ms.openlocfilehash: 840a1b2e45f4763131299577583aa89015dcdd1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices"></a>Använda önskade egenskaper tooconfigure enheter
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Hello slutet av den här självstudiekursen har du två konsolappar:

* **SimulateDeviceConfiguration.js**, en simulerad enhetsapp som väntar på en uppdatering för önskad konfiguration och rapporterar hello status för en simulerad konfigurationsprocessen för uppdateringen.
* **SetDesiredConfigurationAndQuery**, en .NET backend-app, vilket anger hello önskad konfiguration på en enhet och frågor hello konfigurationsprocessen för uppdateringen.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.
> 
> 

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Node.js version 0.10.x eller senare.
* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.

Om du har följt hello [Kom igång med enheten twins] [ lnk-twin-tutorial] kursen har du redan en IoT-hubb och en enhetsidentitet kallas **myDeviceId**. I så fall kan du hoppa över toohello [skapa hello simulerade enhetsapp] [ lnk-how-to-configure-createapp] avsnitt.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a>Skapa hello simulerade enhetsapp
I det här avsnittet skapar du en Node.js-konsolprogram som ansluter tooyour hubb som **myDeviceId**, väntar på en uppdatering för önskad konfiguration och rapporterar uppdateringar på hello simulerade konfigurationsprocessen för uppdateringen.

1. Skapa en ny tom mapp som kallas **simulatedeviceconfiguration**. I hello **simulatedeviceconfiguration** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello.
   
    ```
    npm init
    ```
1. Vid en kommandotolk i hello **simulatedeviceconfiguration** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** och **azure-iot-enhet – mqtt**paket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. Med hjälp av en textredigerare, skapa en ny **SimulateDeviceConfiguration.js** filen i hello **simulatedeviceconfiguration** mapp.
1. Lägg till följande kod toohello hello **SimulateDeviceConfiguration.js** filen och ersätta hello **{enheten anslutningssträngen}** med hello enheten anslutningssträngen som du kopierade när du Skapa hello **myDeviceId** enhets-ID:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    Hej **klienten** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello enhet. Den här koden initierar hello **klienten** objekt, hämtar hello enheten dubbla för **myDeviceId**, och sedan kopplar en hanterare för hello uppdatering på *önskade egenskaper*. hello hanterare verifierar att det är en verklig configuration ändringsbegäran genom att jämföra hello configIds sedan anropar en metod som startar hello konfigurationsändring.
   
    Observera att för hello dig ut av enkelhet den här koden använder en hårdkodad standard för hello inledande konfiguration. En verklig app skulle förmodligen att läsa in konfigurationen från en lokal lagring.
   
   > [!IMPORTANT]
   > Önskad egenskapen Ändringshändelser släpps alltid en gång vid enhetsanslutning. Kontrollera att det finns en verklig ändring i hello toocheck önskade egenskaper innan du utför några åtgärder.
   > 
   > 
1. Lägg till följande metoder innan hello hello `client.open()` anrop:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    Hej **initConfigChange** metoden uppdateringar hello rapporterade egenskaper för hello lokala dubbla enhetsobjekt med hello konfiguration uppdateringsstatus begäran och anger hello för**väntande**, och sedan uppdateringar hello enheten dubbla på hello-tjänsten. När en uppdatering hello enheten dubbla är den simulerar en tidskrävande process som avslutar hello körning av **completeConfigChange**. Den här metoden uppdaterar hello lokala rapporterade egenskaper sätta hello status för**lyckade** och ta bort hello **pendingConfig** objekt. Sedan uppdateras hello enheten dubbla på hello-tjänsten.
   
    Observera toosave bandbredd som rapporteras egenskaper uppdateras genom att ange endast hello egenskaper toobe ändrade (med namnet **korrigering** i hello ovan kod), i stället för att ersätta hello hela dokumentet.
   
   > [!NOTE]
   > Den här självstudiekursen simulerar inte någon funktion för samtidiga konfigurationsuppdateringar. Vissa processer för uppdatering av konfiguration kan vara kan tooaccommodate ändringar av konfigurationen för målet medan hello uppdateringen körs, vissa kanske har tooqueue dem och vissa kan avvisa dem med ett feltillstånd. Se till att tooconsider hello önskat beteende för din specifika konfigurationsprocessen och lägga till lämpliga hello-logik innan du påbörjar hello konfigurationsändring.
   > 
   > 
1. Kör hello enhetsapp:
   
        node SimulateDeviceConfiguration.js
   
    Du bör se hello-meddelande `retrieved device twin`. Behåll hello appen körs.

## <a name="create-hello-service-app"></a>Skapa hello service-appen
I det här avsnittet skapar du en .NET-konsolapp som uppdateringar hello *önskade egenskaper* på hello enheten dubbla som är associerade med **myDeviceId** med ett nytt telemetri configuration-objekt. Sedan frågar hello enheten twins lagras i hello IoT-hubb och visar hello skillnaden mellan hello önskad och rapporterade konfigurationer av hello enhet.

1. I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall. Namnet hello projektet **SetDesiredConfigurationAndQuery**.
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]
1. I Solution Explorer högerklickar du på hello **SetDesiredConfigurationAndQuery** projektet och klicka sedan på **hantera NuGet-paket...** .
1. I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. Lägg till följande metod toohello hello **programmet** klass:
   
        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
   
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired configuration");
   
            try
            {
                while (true)
                {
                    var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                    var results = await query.GetNextAsTwinAsync();
                    foreach (var result in results)
                    {
                        Console.WriteLine("Config report for: {0}", result.DeviceId);
                        Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                        Console.WriteLine();
                    }
                    Thread.Sleep(10000);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Exception: {ex.Message}");
            }
        }
   
    Hej **registret** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello-tjänsten. Den här koden initierar hello **registret** objekt, hämtar hello enheten dubbla för **myDeviceId**, och uppdaterar sedan dess egenskaper med ett nytt telemetri configuration-objekt.
    Efter det frågar hello enheten twins lagras i hello IoT-hubb var 10: e sekund och utskrifter hello önskad och rapporterade telemetri konfigurationer. Se toohello [IoT-hubb frågespråket] [ lnk-query] toolearn hur toogenerate omfattande rapporter på alla enheter.
   
   > [!IMPORTANT]
   > Det här programmet frågar IoT-hubb var 10: e sekund som illustration. Använd frågar toogenerate användarinriktad rapporter över flera enheter, och inte toodetect ändringar. Om din lösning kräver meddelanden i realtid av enhetshändelser, använder [dubbla meddelanden][lnk-twin-notifications].
   > 
   > 
1. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **SetDesiredConfigurationAndQuery** projektet är **starta**. Skapa hello lösning.
1. Med **SimulateDeviceConfiguration.js** körs, kör hello .NET-program från Visual Studio med hjälp av **F5** och du bör se hello rapporterade konfigurationsändring från **lyckade** för**väntande** för**lyckade** igen med hello ny aktiv frekvens som skickar fem minuter i stället för 24 timmar.

 ![Enheten har konfigurerats][img-deviceconfigured]
   
   > [!IMPORTANT]
   > Det finns en fördröjning på upp tooa minut mellan hello enheten rapporten igen och hello frågeresultatet. Detta är tooenable hello frågan infrastruktur toowork i hög skala. tooretrieve konsekvent vyer för en enskild enhet dubbla använda hello **getDeviceTwin** metod i hello **registret** klass.
   > 
   > 

## <a name="next-steps"></a>Nästa steg
I kursen får du ange en önskad konfiguration som *önskade egenskaper* från hello lösning serverdel och skrev en enhet app toodetect som ändras och simulera en flera steg uppdateringsprocess reporting statusen via hello rapporterade Egenskaper.

Använd hello följande resurser toolearn hur till:

* Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen
* schemalägga eller utföra åtgärder på stora mängder enheter finns hello [schema och broadcast jobb] [ lnk-schedule-jobs] kursen.
* Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app), med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-node-twin-how-to-configure/deviceconfigured.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-csharp-node-twin-how-to-configure.md#create-the-simulated-device-app

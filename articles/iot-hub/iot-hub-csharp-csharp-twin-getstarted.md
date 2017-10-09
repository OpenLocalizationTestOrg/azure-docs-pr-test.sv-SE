---
title: "aaaGet igång med Azure IoT Hub-enhet twins (.NET/.NET) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooadd taggar och sedan använda en IoT-hubb-fråga. Du kan använda hello Azure IoT-enhet SDK för .NET tooimplement hello simulerade enheten appen och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som lägger till hello taggar och kör hello IoT-hubb frågan."
services: iot-hub
documentationcenter: node
author: dsk-2015
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: dkshir
ms.openlocfilehash: 7fa73ac896c44e79c6522d252cd1515bd6e7bb2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-netnet"></a>Kom igång med enheten twins (.NET/.NET)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Hello slutet av den här självstudiekursen måste de här apparna för .NET-konsolen:

* **CreateDeviceIdentity**, en .NET-app som skapar en enhets-ID och tillhörande nyckeln tooconnect appen simulerade enheten.
* **AddTagsAndQuery**, en .NET backend-app som lägger till taggar och frågar enheten twins.
* **ReportConnectivity**, en enhet .NET-app som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och rapporterar tillståndet anslutningen.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.
> 
> 

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Om du vill toocreate hello enhetsidentitet programmässigt i stället kan du läsa hello motsvarande avsnitt i hello [ansluta din simulerade enhet tooyour IoT-hubb med hjälp av .NET] [ lnk-device-identity-csharp] artikel.

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
I det här avsnittet skapar du en .NET-konsolapp som ansluter tooyour hubb som **myDeviceId**, och sedan uppdaterar rapporterade egenskaper toocontain hello informationen att servern är ansluten med hjälp av ett mobilnät.

1. I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall. Namnet hello projektet **ReportConnectivity**.
   
    ![Ny Visual C# Windows Classic enhetsapp][img-createdeviceapp]
    
1. I Solution Explorer högerklickar du på hello **ReportConnectivity** projektet och klicka sedan på **hantera NuGet-paket...** .
1. I hello **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices.client**. Välj **installera** tooinstall hello **Microsoft.Azure.Devices.Client** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT-enhet SDK] [ lnk-nuget-client-sdk] NuGet-paketet och dess beroenden.
   
    ![NuGet-Pakethanteraren fönstret-klientappen][img-clientnuget]
1. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållarvärde med anslutningssträngen för hello enheten som du antecknade i hello föregående avsnitt.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. Lägg till följande metod toohello hello **programmet** klass:

       public static async void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting toohub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
                Console.WriteLine("Retrieving twin");
                await Client.GetTwinAsync();
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    Hej **klienten** objektet innehåller alla hello-metoder som du behöver toointeract med enheten twins från hello enhet. Hej koden som visas ovan, initierar hello **klienten** objekt och hämtar hello enheten dubbla för **myDeviceId**.

1. Lägg till följande metod toohello hello **programmet** klass:
   
        public static async void ReportConnectivity()
        {
            try
            {
                Console.WriteLine("Sending connectivity data as reported property");
                
                TwinCollection reportedProperties, connectivity;
                reportedProperties = new TwinCollection();
                connectivity = new TwinCollection();
                connectivity["type"] = "cellular";
                reportedProperties["connectivity"] = connectivity;
                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

   Hej koden ovan uppdateringar **myDeviceId**har rapporterats egenskap med hello anslutningsinformation.

1. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
       try
       {
            InitClient();
            ReportConnectivity();
       }
       catch (Exception ex)
       {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
       }
       Console.WriteLine("Press Enter tooexit.");
       Console.ReadLine();

1. Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **ReportConnectivity** projektet är **starta**. Skapa hello lösning.
1. Kör det här programmet genom att högerklicka på hello **ReportConnectivity** projekt och välja **felsöka**, följt av **Starta ny instans**. Du bör se information om hello dubbla och sedan skicka anslutning som en *rapporterade egenskapen*.
   
    ![Kör enhetsanslutning app tooreport][img-rundeviceapp]
    
    
1. Nu när hello enheten rapporterade dess anslutningsinformation, bör den visas i båda frågor. Kör hello .NET **AddTagsAndQuery** app toorun hello frågar igen. Den här gången **myDeviceId** ska visas i båda frågeresultaten.
   
    ![Enhetsanslutning som har rapporterats][img-tagappsuccess]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du lagt till enhetsmetadata som taggar från en backend-app och skrev en simulerad enhet app tooreport anslutning enhetsinformation i hello enheten dubbla. Du också lära sig hur tooquery informationen med hjälp av frågespråket för hello SQL-liknande IoT-hubb.

Använd hello följande resurser toolearn hur till:

* Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen
* Konfigurera enheter med hjälp av enheten två egenskaper med hello [Använd önskad egenskaper tooconfigure enheter] [ lnk-twin-how-to-configure] självstudiekursen
* Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app) med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png
[img-rundeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png
[img-tagappsuccess]: media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md


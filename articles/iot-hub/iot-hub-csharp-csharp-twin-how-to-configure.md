---
title: "Egenskaper för aaaUse Azure IoT Hub enhet dubbla (.NET/.NET) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooconfigure enheter. Du använder hello Azure IoT-enhet SDK för .NET tooimplement en simulerad enhetsapp och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som ändrar en konfiguration för enheter med hjälp av en enhet dubbla."
services: iot-hub
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.assetid: 3c627476-f982-43c9-bd17-e0698c5d236d
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dkshir
ms.openlocfilehash: 486436d29abfd5158c253adc5abf5935e0e1fdba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices"></a>Använda önskade egenskaper tooconfigure enheter
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Hello slutet av den här självstudiekursen, kommer du har två .NET konsolappar:

* **SimulateDeviceConfiguration**, en simulerad enhetsapp som väntar på en uppdatering för önskad konfiguration och rapporterar hello status för en simulerad konfigurationsprocessen för uppdateringen.
* **SetDesiredConfigurationAndQuery**, en backend-app, vilket anger hello önskad konfiguration på en enhet och frågor hello konfigurationsprocessen för uppdateringen.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.
> 
> 

toocomplete den här kursen behöver du hello följande:

* Visual Studio 2015 eller Visual Studio 2017.
* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.

Om du har följt hello [Kom igång med enheten twins] [ lnk-twin-tutorial] kursen har du redan en IoT-hubb och en enhetsidentitet kallas **myDeviceId**. I så fall kan du hoppa över toohello [skapa hello simulerade enhetsapp] [ lnk-how-to-configure-createapp] avsnitt.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a>Skapa hello simulerade enhetsapp
I det här avsnittet skapar du en .NET-konsolapp som ansluter tooyour hubb som **myDeviceId**, väntar på en uppdatering för önskad konfiguration och rapporterar uppdateringar på hello simulerade konfigurationsprocessen för uppdateringen.

1. I Visual Studio kan du skapa ett nytt projekt i Visual C# klassiska skrivbordet med hello **konsolprogram** projektmall. Namnet hello projektet **SimulateDeviceConfiguration**.
   
    ![Ny Visual C# Windows Classic enhetsapp][img-createdeviceapp]

1. I Solution Explorer högerklickar du på hello **SimulateDeviceConfiguration** projektet och klicka sedan på **hantera NuGet-paket...** .
1. I hello **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices.client**. Välj **installera** tooinstall hello **Microsoft.Azure.Devices.Client** paketet och acceptera hello villkor för användning. Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT-enhet SDK] [ lnk-nuget-client-sdk] NuGet-paketet och dess beroenden.
   
    ![NuGet-Pakethanteraren fönstret-klientappen][img-clientnuget]
1. Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. Lägg till följande fält toohello hello **programmet** klass. Ersätt hello platshållarvärde med anslutningssträngen för hello enheten som du antecknade i hello föregående avsnitt.
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. Lägg till följande metod toohello hello **programmet** klass:
 
        public static void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting toohub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }
    Hej **klienten** objektet innehåller alla hello-metoder som du behöver toointeract med enheten twins från hello enhet. Hej koden som visas ovan, initierar hello **klienten** objekt och hämtar hello enheten dubbla för **myDeviceId**.

1. Lägg till följande metod toohello hello **programmet** klass. Den här metoden anger hello ursprungliga värden för telemetri på hello lokala enhet och sedan uppdateringar hello dubbla för enheten.

        public static async void InitTelemetry()
        {
            try
            {
                Console.WriteLine("Report initial telemetry config:");
                TwinCollection telemetryConfig = new TwinCollection();
                
                telemetryConfig["configId"] = "0";
                telemetryConfig["sendFrequency"] = "24h";
                reportedProperties["telemetryConfig"] = telemetryConfig;
                Console.WriteLine(JsonConvert.SerializeObject(reportedProperties));

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Lägg till följande metod toohello hello **programmet** klass. Det här är en motringning som identifierar en ändring i *önskade egenskaper* i hello enheten dubbla.

        private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
        {
            try
            {
                Console.WriteLine("Desired property change:");
                Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                var currentTelemetryConfig = reportedProperties["telemetryConfig"];
                var desiredTelemetryConfig = desiredProperties["telemetryConfig"];

                if ((desiredTelemetryConfig != null) && (desiredTelemetryConfig["configId"] != currentTelemetryConfig["configId"]))
                {
                    Console.WriteLine("\nInitiating config change");
                    currentTelemetryConfig["status"] = "Pending";
                    currentTelemetryConfig["pendingConfig"] = desiredTelemetryConfig;

                    await Client.UpdateReportedPropertiesAsync(reportedProperties);

                    CompleteConfigChange();
                }
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    Den här metoden uppdateringar hello rapporterade egenskaper för hello lokala dubbla enhetsobjekt med hello konfiguration uppdateringsstatus begäran och anger hello för**väntande**, och sedan uppdateringar hello enheten dubbla på hello-tjänsten. När en uppdatering av hello enheten dubbla är klar hello config ändringen genom att anropa metoden hello `CompleteConfigChange` i hello nästa punkt.

1. Lägg till följande metod toohello hello **programmet** klass. Den här metoden simulerar en återställning av enheten och sedan uppdateringar hello egenskaper för lokal rapporterade sätta hello status för**lyckade** och tar bort hello **pendingConfig** element. Sedan uppdateras hello enheten dubbla på hello-tjänsten. 

        public static async void CompleteConfigChange()
        {
            try
            {
                var currentTelemetryConfig = reportedProperties["telemetryConfig"];

                Console.WriteLine("\nSimulating device reset");
                await Task.Delay(30000); 

                Console.WriteLine("\nCompleting config change");
                currentTelemetryConfig["configId"] = currentTelemetryConfig["pendingConfig"]["configId"];
                currentTelemetryConfig["sendFrequency"] = currentTelemetryConfig["pendingConfig"]["sendFrequency"];
                currentTelemetryConfig["status"] = "Success";
                currentTelemetryConfig["pendingConfig"] = null;

                await Client.UpdateReportedPropertiesAsync(reportedProperties);
                Console.WriteLine("Config change complete \nPress any key tooexit.");
            }
            catch (AggregateException ex)
            {
                foreach (Exception exception in ex.InnerExceptions)
                {
                    Console.WriteLine();
                    Console.WriteLine("Error in sample: {0}", exception);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

1. Slutligen lägger du till följande rader toohello hello **Main** metoden:

        try
        {
            InitClient();
            InitTelemetry();

            Console.WriteLine("Wait for desired telemetry...");
            Client.SetDesiredPropertyUpdateCallback(OnDesiredPropertyChanged, null).Wait();
            Console.ReadKey();
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
        }

   > [!NOTE]
   > Den här självstudiekursen simulerar inte någon funktion för samtidiga konfigurationsuppdateringar. Vissa processer för uppdatering av konfiguration kan vara kan tooaccommodate ändringar av konfigurationen för målet medan hello uppdateringen körs, vissa kanske har tooqueue dem och vissa kan avvisa dem med ett feltillstånd. Se till att tooconsider hello önskat beteende för din specifika konfigurationsprocessen och lägga till lämpliga hello-logik innan du påbörjar hello konfigurationsändring.
   > 
   > 
1. Skapa hello lösning och kör sedan hello enhetsapp från Visual Studio genom att klicka på **F5**. Du bör se hälsningsmeddelande som anger att den simulerade enheten hämtar hello enheten dubbla, konfigurera hello telemetri och väntar på att ändra önskade egenskap på hello utdata-konsolen. Behåll hello appen körs.

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
   
    Hej **registret** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello-tjänsten. Den här koden initierar hello **registret** objekt, hämtar hello enheten dubbla för **myDeviceId**, och uppdaterar sedan dess egenskaper med ett nytt telemetri configuration-objekt.
    Efter det frågar hello enheten twins lagras i hello IoT-hubb var 10: e sekund och utskrifter hello önskad och rapporterade telemetri konfigurationer. Se toohello [IoT-hubb frågespråket] [ lnk-query] toolearn hur toogenerate omfattande rapporter på alla enheter.
   
   > [!IMPORTANT]
   > Det här programmet frågar IoT-hubb var 10: e sekund som illustration. Använd frågar toogenerate användarinriktad rapporter över flera enheter, och inte toodetect ändringar. Om din lösning kräver meddelanden i realtid av enhetshändelser, använder [dubbla meddelanden][lnk-twin-notifications].
   > 
   > 
1. Slutligen lägger du till följande rader toohello hello **Main** metoden:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **SetDesiredConfigurationAndQuery** projektet är **starta**. Skapa hello lösning.
1. Med **SimulateDeviceConfiguration** enheten app körs, kör hello service-appen från Visual Studio med hjälp av **F5**. Du bör se hello rapporterade konfigurationsändring från **väntande** för**lyckade** med hello ny aktiv skicka frekvensen av fem minuter i stället för 24 timmar.

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
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createnetapp.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-how-to-configure/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-how-to-configure/devicesdknuget.png
[img-deviceconfigured]: media/iot-hub-csharp-csharp-twin-how-to-configure/deviceconfigured.png


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0/

[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-twin-tutorial]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-how-to-configure-createapp]: iot-hub-csharp-csharp-twin-how-to-configure.md#create-the-simulated-device-app

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
# <a name="use-desired-properties-tooconfigure-devices"></a><span data-ttu-id="9070b-104">Använda önskade egenskaper tooconfigure enheter</span><span class="sxs-lookup"><span data-stu-id="9070b-104">Use desired properties tooconfigure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="9070b-105">Hello slutet av den här självstudiekursen, kommer du har två .NET konsolappar:</span><span class="sxs-lookup"><span data-stu-id="9070b-105">At hello end of this tutorial, you will have two .NET console apps:</span></span>

* <span data-ttu-id="9070b-106">**SimulateDeviceConfiguration**, en simulerad enhetsapp som väntar på en uppdatering för önskad konfiguration och rapporterar hello status för en simulerad konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="9070b-106">**SimulateDeviceConfiguration**, a simulated device app that waits for a desired configuration update and reports hello status of a simulated configuration update process.</span></span>
* <span data-ttu-id="9070b-107">**SetDesiredConfigurationAndQuery**, en backend-app, vilket anger hello önskad konfiguration på en enhet och frågor hello konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="9070b-107">**SetDesiredConfigurationAndQuery**, a back-end app, which sets hello desired configuration on a device and queries hello configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="9070b-108">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="9070b-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="9070b-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="9070b-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="9070b-110">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="9070b-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="9070b-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="9070b-111">An active Azure account.</span></span> <span data-ttu-id="9070b-112">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="9070b-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="9070b-113">Om du har följt hello [Kom igång med enheten twins] [ lnk-twin-tutorial] kursen har du redan en IoT-hubb och en enhetsidentitet kallas **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="9070b-113">If you followed hello [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="9070b-114">I så fall kan du hoppa över toohello [skapa hello simulerade enhetsapp] [ lnk-how-to-configure-createapp] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9070b-114">In that case, you can skip toohello [Create hello simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a><span data-ttu-id="9070b-115">Skapa hello simulerade enhetsapp</span><span class="sxs-lookup"><span data-stu-id="9070b-115">Create hello simulated device app</span></span>
<span data-ttu-id="9070b-116">I det här avsnittet skapar du en .NET-konsolapp som ansluter tooyour hubb som **myDeviceId**, väntar på en uppdatering för önskad konfiguration och rapporterar uppdateringar på hello simulerade konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="9070b-116">In this section, you create a .NET console app that connects tooyour hub as **myDeviceId**, waits for a desired configuration update and then reports updates on hello simulated configuration update process.</span></span>

1. <span data-ttu-id="9070b-117">I Visual Studio kan du skapa ett nytt projekt i Visual C# klassiska skrivbordet med hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="9070b-117">In Visual Studio, create a new Visual C# Windows Classic Desktop project by using hello **Console Application** project template.</span></span> <span data-ttu-id="9070b-118">Namnet hello projektet **SimulateDeviceConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="9070b-118">Name hello project **SimulateDeviceConfiguration**.</span></span>
   
    ![Ny Visual C# Windows Classic enhetsapp][img-createdeviceapp]

1. <span data-ttu-id="9070b-120">I Solution Explorer högerklickar du på hello **SimulateDeviceConfiguration** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="9070b-120">In Solution Explorer, right-click hello **SimulateDeviceConfiguration** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="9070b-121">I hello **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="9070b-121">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="9070b-122">Välj **installera** tooinstall hello **Microsoft.Azure.Devices.Client** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="9070b-122">Select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="9070b-123">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT-enhet SDK] [ lnk-nuget-client-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="9070b-123">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Pakethanteraren fönstret-klientappen][img-clientnuget]
1. <span data-ttu-id="9070b-125">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="9070b-125">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="9070b-126">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="9070b-126">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="9070b-127">Ersätt hello platshållarvärde med anslutningssträngen för hello enheten som du antecknade i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9070b-127">Replace hello placeholder value with hello device connection string that you noted in hello previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;
        static TwinCollection reportedProperties = new TwinCollection();

1. <span data-ttu-id="9070b-128">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="9070b-128">Add hello following method toohello **Program** class:</span></span>
 
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
    <span data-ttu-id="9070b-129">Hej **klienten** objektet innehåller alla hello-metoder som du behöver toointeract med enheten twins från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="9070b-129">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="9070b-130">Hej koden som visas ovan, initierar hello **klienten** objekt och hämtar hello enheten dubbla för **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="9070b-130">hello code shown above, initializes hello **Client** object, and then retrieves hello device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="9070b-131">Lägg till följande metod toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="9070b-131">Add hello following method toohello **Program** class.</span></span> <span data-ttu-id="9070b-132">Den här metoden anger hello ursprungliga värden för telemetri på hello lokala enhet och sedan uppdateringar hello dubbla för enheten.</span><span class="sxs-lookup"><span data-stu-id="9070b-132">This method sets hello initial values of telemetry on hello local device and then updates hello device twin.</span></span>

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

1. <span data-ttu-id="9070b-133">Lägg till följande metod toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="9070b-133">Add hello following method toohello **Program** class.</span></span> <span data-ttu-id="9070b-134">Det här är en motringning som identifierar en ändring i *önskade egenskaper* i hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="9070b-134">This is a callback which will detect a change in *desired properties* in hello device twin.</span></span>

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

    <span data-ttu-id="9070b-135">Den här metoden uppdateringar hello rapporterade egenskaper för hello lokala dubbla enhetsobjekt med hello konfiguration uppdateringsstatus begäran och anger hello för**väntande**, och sedan uppdateringar hello enheten dubbla på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9070b-135">This method updates hello reported properties on hello local device twin object with hello configuration update request and sets hello status too**Pending**, then updates hello device twin on hello service.</span></span> <span data-ttu-id="9070b-136">När en uppdatering av hello enheten dubbla är klar hello config ändringen genom att anropa metoden hello `CompleteConfigChange` i hello nästa punkt.</span><span class="sxs-lookup"><span data-stu-id="9070b-136">After successfully updating hello device twin, it completes hello config change by calling hello method `CompleteConfigChange` described in hello next point.</span></span>

1. <span data-ttu-id="9070b-137">Lägg till följande metod toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="9070b-137">Add hello following method toohello **Program** class.</span></span> <span data-ttu-id="9070b-138">Den här metoden simulerar en återställning av enheten och sedan uppdateringar hello egenskaper för lokal rapporterade sätta hello status för**lyckade** och tar bort hello **pendingConfig** element.</span><span class="sxs-lookup"><span data-stu-id="9070b-138">This method simulates a device reset, then updates hello local reported properties setting hello status too**Success** and removes hello **pendingConfig** element.</span></span> <span data-ttu-id="9070b-139">Sedan uppdateras hello enheten dubbla på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9070b-139">It then updates hello device twin on hello service.</span></span> 

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

1. <span data-ttu-id="9070b-140">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="9070b-140">Finally add hello following lines toohello **Main** method:</span></span>

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
   > <span data-ttu-id="9070b-141">Den här självstudiekursen simulerar inte någon funktion för samtidiga konfigurationsuppdateringar.</span><span class="sxs-lookup"><span data-stu-id="9070b-141">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="9070b-142">Vissa processer för uppdatering av konfiguration kan vara kan tooaccommodate ändringar av konfigurationen för målet medan hello uppdateringen körs, vissa kanske har tooqueue dem och vissa kan avvisa dem med ett feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="9070b-142">Some configuration update processes might be able tooaccommodate changes of target configuration while hello update is running, some might have tooqueue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="9070b-143">Se till att tooconsider hello önskat beteende för din specifika konfigurationsprocessen och lägga till lämpliga hello-logik innan du påbörjar hello konfigurationsändring.</span><span class="sxs-lookup"><span data-stu-id="9070b-143">Make sure tooconsider hello desired behavior for your specific configuration process, and add hello appropriate logic before initiating hello configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="9070b-144">Skapa hello lösning och kör sedan hello enhetsapp från Visual Studio genom att klicka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="9070b-144">Build hello solution and then run hello device app from Visual Studio by clicking **F5**.</span></span> <span data-ttu-id="9070b-145">Du bör se hälsningsmeddelande som anger att den simulerade enheten hämtar hello enheten dubbla, konfigurera hello telemetri och väntar på att ändra önskade egenskap på hello utdata-konsolen.</span><span class="sxs-lookup"><span data-stu-id="9070b-145">On hello output console, you should see hello messages indicating that your simulated device is retrieving hello device twin, setting up hello telemetry, and waiting for desired property change.</span></span> <span data-ttu-id="9070b-146">Behåll hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="9070b-146">Keep hello app running.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="9070b-147">Skapa hello service-appen</span><span class="sxs-lookup"><span data-stu-id="9070b-147">Create hello service app</span></span>
<span data-ttu-id="9070b-148">I det här avsnittet skapar du en .NET-konsolapp som uppdateringar hello *önskade egenskaper* på hello enheten dubbla som är associerade med **myDeviceId** med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="9070b-148">In this section, you will create a .NET console app that updates hello *desired properties* on hello device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="9070b-149">Sedan frågar hello enheten twins lagras i hello IoT-hubb och visar hello skillnaden mellan hello önskad och rapporterade konfigurationer av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="9070b-149">It then queries hello device twins stored in hello IoT hub and shows hello difference between hello desired and reported configurations of hello device.</span></span>

1. <span data-ttu-id="9070b-150">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="9070b-150">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="9070b-151">Namnet hello projektet **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="9070b-151">Name hello project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]
1. <span data-ttu-id="9070b-153">I Solution Explorer högerklickar du på hello **SetDesiredConfigurationAndQuery** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="9070b-153">In Solution Explorer, right-click hello **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="9070b-154">I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="9070b-154">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="9070b-155">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="9070b-155">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="9070b-157">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="9070b-157">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="9070b-158">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="9070b-158">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="9070b-159">Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="9070b-159">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="9070b-160">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="9070b-160">Add hello following method toohello **Program** class:</span></span>
   
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
   
    <span data-ttu-id="9070b-161">Hej **registret** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9070b-161">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="9070b-162">Den här koden initierar hello **registret** objekt, hämtar hello enheten dubbla för **myDeviceId**, och uppdaterar sedan dess egenskaper med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="9070b-162">This code initializes hello **Registry** object, retrieves hello device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="9070b-163">Efter det frågar hello enheten twins lagras i hello IoT-hubb var 10: e sekund och utskrifter hello önskad och rapporterade telemetri konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="9070b-163">After that, it queries hello device twins stored in hello IoT hub every 10 seconds, and prints hello desired and reported telemetry configurations.</span></span> <span data-ttu-id="9070b-164">Se toohello [IoT-hubb frågespråket] [ lnk-query] toolearn hur toogenerate omfattande rapporter på alla enheter.</span><span class="sxs-lookup"><span data-stu-id="9070b-164">Refer toohello [IoT Hub query language][lnk-query] toolearn how toogenerate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="9070b-165">Det här programmet frågar IoT-hubb var 10: e sekund som illustration.</span><span class="sxs-lookup"><span data-stu-id="9070b-165">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="9070b-166">Använd frågar toogenerate användarinriktad rapporter över flera enheter, och inte toodetect ändringar.</span><span class="sxs-lookup"><span data-stu-id="9070b-166">Use queries toogenerate user-facing reports across many devices, and not toodetect changes.</span></span> <span data-ttu-id="9070b-167">Om din lösning kräver meddelanden i realtid av enhetshändelser, använder [dubbla meddelanden][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="9070b-167">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="9070b-168">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="9070b-168">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. <span data-ttu-id="9070b-169">Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **SetDesiredConfigurationAndQuery** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="9070b-169">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="9070b-170">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="9070b-170">Build hello solution.</span></span>
1. <span data-ttu-id="9070b-171">Med **SimulateDeviceConfiguration** enheten app körs, kör hello service-appen från Visual Studio med hjälp av **F5**.</span><span class="sxs-lookup"><span data-stu-id="9070b-171">With **SimulateDeviceConfiguration** device app running, run hello service app from Visual Studio using **F5**.</span></span> <span data-ttu-id="9070b-172">Du bör se hello rapporterade konfigurationsändring från **väntande** för**lyckade** med hello ny aktiv skicka frekvensen av fem minuter i stället för 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="9070b-172">You should see hello reported configuration change from **Pending** too**Success** with hello new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Enheten har konfigurerats][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="9070b-174">Det finns en fördröjning på upp tooa minut mellan hello enheten rapporten igen och hello frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="9070b-174">There is a delay of up tooa minute between hello device report operation and hello query result.</span></span> <span data-ttu-id="9070b-175">Detta är tooenable hello frågan infrastruktur toowork i hög skala.</span><span class="sxs-lookup"><span data-stu-id="9070b-175">This is tooenable hello query infrastructure toowork at very high scale.</span></span> <span data-ttu-id="9070b-176">tooretrieve konsekvent vyer för en enskild enhet dubbla använda hello **getDeviceTwin** metod i hello **registret** klass.</span><span class="sxs-lookup"><span data-stu-id="9070b-176">tooretrieve consistent views of a single device twin use hello **getDeviceTwin** method in hello **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="9070b-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9070b-177">Next steps</span></span>
<span data-ttu-id="9070b-178">I kursen får du ange en önskad konfiguration som *önskade egenskaper* från hello lösning serverdel och skrev en enhet app toodetect som ändras och simulera en flera steg uppdateringsprocess reporting statusen via hello rapporterade Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="9070b-178">In this tutorial, you set a desired configuration as *desired properties* from hello solution back end, and wrote a device app toodetect that change and simulate a multi-step update process reporting its status through hello reported properties.</span></span>

<span data-ttu-id="9070b-179">Använd hello följande resurser toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="9070b-179">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="9070b-180">Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="9070b-180">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="9070b-181">schemalägga eller utföra åtgärder på stora mängder enheter finns hello [schema och broadcast jobb] [ lnk-schedule-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="9070b-181">schedule or perform operations on large sets of devices see hello [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="9070b-182">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app), med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="9070b-182">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

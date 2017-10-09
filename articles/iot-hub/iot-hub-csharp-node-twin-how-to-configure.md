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
# <a name="use-desired-properties-tooconfigure-devices"></a><span data-ttu-id="a9088-104">Använda önskade egenskaper tooconfigure enheter</span><span class="sxs-lookup"><span data-stu-id="a9088-104">Use desired properties tooconfigure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="a9088-105">Hello slutet av den här självstudiekursen har du två konsolappar:</span><span class="sxs-lookup"><span data-stu-id="a9088-105">At hello end of this tutorial, you will have two console apps:</span></span>

* <span data-ttu-id="a9088-106">**SimulateDeviceConfiguration.js**, en simulerad enhetsapp som väntar på en uppdatering för önskad konfiguration och rapporterar hello status för en simulerad konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="a9088-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports hello status of a simulated configuration update process.</span></span>
* <span data-ttu-id="a9088-107">**SetDesiredConfigurationAndQuery**, en .NET backend-app, vilket anger hello önskad konfiguration på en enhet och frågor hello konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="a9088-107">**SetDesiredConfigurationAndQuery**, a .NET back-end app, which sets hello desired configuration on a device and queries hello configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="a9088-108">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="a9088-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="a9088-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="a9088-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="a9088-110">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a9088-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="a9088-111">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="a9088-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="a9088-112">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a9088-112">An active Azure account.</span></span> <span data-ttu-id="a9088-113">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="a9088-113">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="a9088-114">Om du har följt hello [Kom igång med enheten twins] [ lnk-twin-tutorial] kursen har du redan en IoT-hubb och en enhetsidentitet kallas **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="a9088-114">If you followed hello [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="a9088-115">I så fall kan du hoppa över toohello [skapa hello simulerade enhetsapp] [ lnk-how-to-configure-createapp] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a9088-115">In that case, you can skip toohello [Create hello simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-hello-simulated-device-app"></a><span data-ttu-id="a9088-116">Skapa hello simulerade enhetsapp</span><span class="sxs-lookup"><span data-stu-id="a9088-116">Create hello simulated device app</span></span>
<span data-ttu-id="a9088-117">I det här avsnittet skapar du en Node.js-konsolprogram som ansluter tooyour hubb som **myDeviceId**, väntar på en uppdatering för önskad konfiguration och rapporterar uppdateringar på hello simulerade konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="a9088-117">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, waits for a desired configuration update and then reports updates on hello simulated configuration update process.</span></span>

1. <span data-ttu-id="a9088-118">Skapa en ny tom mapp som kallas **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="a9088-118">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="a9088-119">I hello **simulatedeviceconfiguration** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="a9088-119">In hello **simulatedeviceconfiguration** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="a9088-120">Acceptera alla standardvärden för hello.</span><span class="sxs-lookup"><span data-stu-id="a9088-120">Accept all hello defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="a9088-121">Vid en kommandotolk i hello **simulatedeviceconfiguration** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** och **azure-iot-enhet – mqtt**paket:</span><span class="sxs-lookup"><span data-stu-id="a9088-121">At your command prompt in hello **simulatedeviceconfiguration** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="a9088-122">Med hjälp av en textredigerare, skapa en ny **SimulateDeviceConfiguration.js** filen i hello **simulatedeviceconfiguration** mapp.</span><span class="sxs-lookup"><span data-stu-id="a9088-122">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in hello **simulatedeviceconfiguration** folder.</span></span>
1. <span data-ttu-id="a9088-123">Lägg till följande kod toohello hello **SimulateDeviceConfiguration.js** filen och ersätta hello **{enheten anslutningssträngen}** med hello enheten anslutningssträngen som du kopierade när du Skapa hello **myDeviceId** enhets-ID:</span><span class="sxs-lookup"><span data-stu-id="a9088-123">Add hello following code toohello **SimulateDeviceConfiguration.js** file, and substitute hello **{device connection string}** placeholder with hello device connection string you copied when you created hello **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="a9088-124">Hej **klienten** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="a9088-124">hello **Client** object exposes all hello methods required toointeract with device twins from hello device.</span></span> <span data-ttu-id="a9088-125">Den här koden initierar hello **klienten** objekt, hämtar hello enheten dubbla för **myDeviceId**, och sedan kopplar en hanterare för hello uppdatering på *önskade egenskaper*.</span><span class="sxs-lookup"><span data-stu-id="a9088-125">This code initializes hello **Client** object, retrieves hello device twin for **myDeviceId**, and then attaches a handler for hello update on *desired properties*.</span></span> <span data-ttu-id="a9088-126">hello hanterare verifierar att det är en verklig configuration ändringsbegäran genom att jämföra hello configIds sedan anropar en metod som startar hello konfigurationsändring.</span><span class="sxs-lookup"><span data-stu-id="a9088-126">hello handler verifies that there is an actual configuration change request by comparing hello configIds, then invokes a method that starts hello configuration change.</span></span>
   
    <span data-ttu-id="a9088-127">Observera att för hello dig ut av enkelhet den här koden använder en hårdkodad standard för hello inledande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a9088-127">Note that for hello sake of simplicity, this code uses a hard-coded default for hello initial configuration.</span></span> <span data-ttu-id="a9088-128">En verklig app skulle förmodligen att läsa in konfigurationen från en lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="a9088-128">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="a9088-129">Önskad egenskapen Ändringshändelser släpps alltid en gång vid enhetsanslutning.</span><span class="sxs-lookup"><span data-stu-id="a9088-129">Desired property change events are always emitted once at device connection.</span></span> <span data-ttu-id="a9088-130">Kontrollera att det finns en verklig ändring i hello toocheck önskade egenskaper innan du utför några åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a9088-130">Make sure toocheck that there is an actual change in hello desired properties before performing any action.</span></span>
   > 
   > 
1. <span data-ttu-id="a9088-131">Lägg till följande metoder innan hello hello `client.open()` anrop:</span><span class="sxs-lookup"><span data-stu-id="a9088-131">Add hello following methods before hello `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="a9088-132">Hej **initConfigChange** metoden uppdateringar hello rapporterade egenskaper för hello lokala dubbla enhetsobjekt med hello konfiguration uppdateringsstatus begäran och anger hello för**väntande**, och sedan uppdateringar hello enheten dubbla på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a9088-132">hello **initConfigChange** method updates hello reported properties on hello local device twin object with hello configuration update request and sets hello status too**Pending**, then updates hello device twin on hello service.</span></span> <span data-ttu-id="a9088-133">När en uppdatering hello enheten dubbla är den simulerar en tidskrävande process som avslutar hello körning av **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="a9088-133">After successfully updating hello device twin, it simulates a long running process that terminates in hello execution of **completeConfigChange**.</span></span> <span data-ttu-id="a9088-134">Den här metoden uppdaterar hello lokala rapporterade egenskaper sätta hello status för**lyckade** och ta bort hello **pendingConfig** objekt.</span><span class="sxs-lookup"><span data-stu-id="a9088-134">This method updates hello local reported properties setting hello status too**Success** and removing hello **pendingConfig** object.</span></span> <span data-ttu-id="a9088-135">Sedan uppdateras hello enheten dubbla på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a9088-135">It then updates hello device twin on hello service.</span></span>
   
    <span data-ttu-id="a9088-136">Observera toosave bandbredd som rapporteras egenskaper uppdateras genom att ange endast hello egenskaper toobe ändrade (med namnet **korrigering** i hello ovan kod), i stället för att ersätta hello hela dokumentet.</span><span class="sxs-lookup"><span data-stu-id="a9088-136">Note that, toosave bandwidth, reported properties are updated by specifying only hello properties toobe modified (named **patch** in hello above code), instead of replacing hello whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a9088-137">Den här självstudiekursen simulerar inte någon funktion för samtidiga konfigurationsuppdateringar.</span><span class="sxs-lookup"><span data-stu-id="a9088-137">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="a9088-138">Vissa processer för uppdatering av konfiguration kan vara kan tooaccommodate ändringar av konfigurationen för målet medan hello uppdateringen körs, vissa kanske har tooqueue dem och vissa kan avvisa dem med ett feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="a9088-138">Some configuration update processes might be able tooaccommodate changes of target configuration while hello update is running, some might have tooqueue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="a9088-139">Se till att tooconsider hello önskat beteende för din specifika konfigurationsprocessen och lägga till lämpliga hello-logik innan du påbörjar hello konfigurationsändring.</span><span class="sxs-lookup"><span data-stu-id="a9088-139">Make sure tooconsider hello desired behavior for your specific configuration process, and add hello appropriate logic before initiating hello configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="a9088-140">Kör hello enhetsapp:</span><span class="sxs-lookup"><span data-stu-id="a9088-140">Run hello device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="a9088-141">Du bör se hello-meddelande `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="a9088-141">You should see hello message `retrieved device twin`.</span></span> <span data-ttu-id="a9088-142">Behåll hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="a9088-142">Keep hello app running.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="a9088-143">Skapa hello service-appen</span><span class="sxs-lookup"><span data-stu-id="a9088-143">Create hello service app</span></span>
<span data-ttu-id="a9088-144">I det här avsnittet skapar du en .NET-konsolapp som uppdateringar hello *önskade egenskaper* på hello enheten dubbla som är associerade med **myDeviceId** med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="a9088-144">In this section, you will create a .NET console app that updates hello *desired properties* on hello device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="a9088-145">Sedan frågar hello enheten twins lagras i hello IoT-hubb och visar hello skillnaden mellan hello önskad och rapporterade konfigurationer av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="a9088-145">It then queries hello device twins stored in hello IoT hub and shows hello difference between hello desired and reported configurations of hello device.</span></span>

1. <span data-ttu-id="a9088-146">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="a9088-146">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="a9088-147">Namnet hello projektet **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="a9088-147">Name hello project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]
1. <span data-ttu-id="a9088-149">I Solution Explorer högerklickar du på hello **SetDesiredConfigurationAndQuery** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="a9088-149">In Solution Explorer, right-click hello **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="a9088-150">I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="a9088-150">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="a9088-151">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="a9088-151">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="a9088-153">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="a9088-153">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="a9088-154">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="a9088-154">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="a9088-155">Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="a9088-155">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="a9088-156">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="a9088-156">Add hello following method toohello **Program** class:</span></span>
   
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
   
    <span data-ttu-id="a9088-157">Hej **registret** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a9088-157">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="a9088-158">Den här koden initierar hello **registret** objekt, hämtar hello enheten dubbla för **myDeviceId**, och uppdaterar sedan dess egenskaper med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="a9088-158">This code initializes hello **Registry** object, retrieves hello device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="a9088-159">Efter det frågar hello enheten twins lagras i hello IoT-hubb var 10: e sekund och utskrifter hello önskad och rapporterade telemetri konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="a9088-159">After that, it queries hello device twins stored in hello IoT hub every 10 seconds, and prints hello desired and reported telemetry configurations.</span></span> <span data-ttu-id="a9088-160">Se toohello [IoT-hubb frågespråket] [ lnk-query] toolearn hur toogenerate omfattande rapporter på alla enheter.</span><span class="sxs-lookup"><span data-stu-id="a9088-160">Refer toohello [IoT Hub query language][lnk-query] toolearn how toogenerate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="a9088-161">Det här programmet frågar IoT-hubb var 10: e sekund som illustration.</span><span class="sxs-lookup"><span data-stu-id="a9088-161">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="a9088-162">Använd frågar toogenerate användarinriktad rapporter över flera enheter, och inte toodetect ändringar.</span><span class="sxs-lookup"><span data-stu-id="a9088-162">Use queries toogenerate user-facing reports across many devices, and not toodetect changes.</span></span> <span data-ttu-id="a9088-163">Om din lösning kräver meddelanden i realtid av enhetshändelser, använder [dubbla meddelanden][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="a9088-163">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="a9088-164">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="a9088-164">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key tooquit.");
        Console.ReadLine();
1. <span data-ttu-id="a9088-165">Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **SetDesiredConfigurationAndQuery** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="a9088-165">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="a9088-166">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="a9088-166">Build hello solution.</span></span>
1. <span data-ttu-id="a9088-167">Med **SimulateDeviceConfiguration.js** körs, kör hello .NET-program från Visual Studio med hjälp av **F5** och du bör se hello rapporterade konfigurationsändring från **lyckade** för**väntande** för**lyckade** igen med hello ny aktiv frekvens som skickar fem minuter i stället för 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="a9088-167">With **SimulateDeviceConfiguration.js** running, run hello .NET application from Visual Studio using **F5** and you should see hello reported configuration change from **Success** too**Pending** too**Success** again with hello new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Enheten har konfigurerats][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="a9088-169">Det finns en fördröjning på upp tooa minut mellan hello enheten rapporten igen och hello frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="a9088-169">There is a delay of up tooa minute between hello device report operation and hello query result.</span></span> <span data-ttu-id="a9088-170">Detta är tooenable hello frågan infrastruktur toowork i hög skala.</span><span class="sxs-lookup"><span data-stu-id="a9088-170">This is tooenable hello query infrastructure toowork at very high scale.</span></span> <span data-ttu-id="a9088-171">tooretrieve konsekvent vyer för en enskild enhet dubbla använda hello **getDeviceTwin** metod i hello **registret** klass.</span><span class="sxs-lookup"><span data-stu-id="a9088-171">tooretrieve consistent views of a single device twin use hello **getDeviceTwin** method in hello **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="a9088-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9088-172">Next steps</span></span>
<span data-ttu-id="a9088-173">I kursen får du ange en önskad konfiguration som *önskade egenskaper* från hello lösning serverdel och skrev en enhet app toodetect som ändras och simulera en flera steg uppdateringsprocess reporting statusen via hello rapporterade Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a9088-173">In this tutorial, you set a desired configuration as *desired properties* from hello solution back end, and wrote a device app toodetect that change and simulate a multi-step update process reporting its status through hello reported properties.</span></span>

<span data-ttu-id="a9088-174">Använd hello följande resurser toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="a9088-174">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="a9088-175">Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="a9088-175">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="a9088-176">schemalägga eller utföra åtgärder på stora mängder enheter finns hello [schema och broadcast jobb] [ lnk-schedule-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="a9088-176">schedule or perform operations on large sets of devices see hello [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="a9088-177">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app), med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="a9088-177">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

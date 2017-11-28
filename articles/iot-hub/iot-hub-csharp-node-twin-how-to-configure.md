---
title: "Använd Azure IoT Hub dubbla enhetsegenskaper (.NET/nod) | Microsoft Docs"
description: "Hur du använder Azure IoT Hub-enhet twins för att konfigurera enheter. Du kan använda Azure IoT-enhet SDK för Node.js för att implementera en simulerad enhetsapp och tjänsten Azure IoT SDK för .NET att implementera ett service-appen som ändrar en konfiguration för enheter med hjälp av en enhet dubbla."
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
ms.openlocfilehash: 78b4523fa7d0c056f84214429730a5df1bcdcef7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-desired-properties-to-configure-devices"></a><span data-ttu-id="11197-104">Använd önskade egenskaper för att konfigurera enheter</span><span class="sxs-lookup"><span data-stu-id="11197-104">Use desired properties to configure devices</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="11197-105">I slutet av den här kursen har du två konsolappar:</span><span class="sxs-lookup"><span data-stu-id="11197-105">At the end of this tutorial, you will have two console apps:</span></span>

* <span data-ttu-id="11197-106">**SimulateDeviceConfiguration.js**, en simulerad enhetsapp som väntar på en uppdatering för önskad konfiguration och rapporterar status för en simulerad konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="11197-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports the status of a simulated configuration update process.</span></span>
* <span data-ttu-id="11197-107">**SetDesiredConfigurationAndQuery**, en .NET backend-app, vilket anger du önskad konfiguration på en enhet och frågar konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="11197-107">**SetDesiredConfigurationAndQuery**, a .NET back-end app, which sets the desired configuration on a device and queries the configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="11197-108">Artikeln [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om Azure IoT-SDK: er som du kan använda för att skapa både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="11197-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="11197-109">Den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="11197-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="11197-110">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="11197-110">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="11197-111">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="11197-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="11197-112">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="11197-112">An active Azure account.</span></span> <span data-ttu-id="11197-113">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="11197-113">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

<span data-ttu-id="11197-114">Om du har följt den [Kom igång med enheten twins] [ lnk-twin-tutorial] kursen har du redan en IoT-hubb och en enhetsidentitet kallas **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="11197-114">If you followed the [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**.</span></span> <span data-ttu-id="11197-115">I så fall kan du hoppa till den [skapa den simulerade enhetsapp] [ lnk-how-to-configure-createapp] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="11197-115">In that case, you can skip to the [Create the simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

<a id="#create-the-simulated-device-app"></a>
## <a name="create-the-simulated-device-app"></a><span data-ttu-id="11197-116">Skapa den simulerade enhetsapp</span><span class="sxs-lookup"><span data-stu-id="11197-116">Create the simulated device app</span></span>
<span data-ttu-id="11197-117">I det här avsnittet skapar du en Node.js-konsolprogram som ansluter till din hubb som **myDeviceId**, väntar på en uppdatering för önskad konfiguration och rapporterar uppdateringar på uppdateringsprocessen simulerade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="11197-117">In this section, you create a Node.js console app that connects to your hub as **myDeviceId**, waits for a desired configuration update and then reports updates on the simulated configuration update process.</span></span>

1. <span data-ttu-id="11197-118">Skapa en ny tom mapp som kallas **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="11197-118">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="11197-119">I den **simulatedeviceconfiguration** mapp, skapa en ny package.json-fil med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="11197-119">In the **simulatedeviceconfiguration** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="11197-120">Acceptera alla standardvärden.</span><span class="sxs-lookup"><span data-stu-id="11197-120">Accept all the defaults.</span></span>
   
    ```
    npm init
    ```
1. <span data-ttu-id="11197-121">Vid en kommandotolk i den **simulatedeviceconfiguration** mapp, kör följande kommando för att installera den **azure iot-enhet** och **azure-iot-enhet – mqtt** paket:</span><span class="sxs-lookup"><span data-stu-id="11197-121">At your command prompt in the **simulatedeviceconfiguration** folder, run the following command to install the **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
1. <span data-ttu-id="11197-122">Med hjälp av en textredigerare, skapa en ny **SimulateDeviceConfiguration.js** filen i den **simulatedeviceconfiguration** mapp.</span><span class="sxs-lookup"><span data-stu-id="11197-122">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in the **simulatedeviceconfiguration** folder.</span></span>
1. <span data-ttu-id="11197-123">Lägg till följande kod i den **SimulateDeviceConfiguration.js** filen och Ersätt den **{enheten anslutningssträngen}** platshållaren med den anslutningssträng för enheten som du kopierade när du skapade den **myDeviceId** enhets-ID:</span><span class="sxs-lookup"><span data-stu-id="11197-123">Add the following code to the **SimulateDeviceConfiguration.js** file, and substitute the **{device connection string}** placeholder with the device connection string you copied when you created the **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="11197-124">Den **klienten** objektet innehåller de metoder som krävs för att interagera med enheten twins från enheten.</span><span class="sxs-lookup"><span data-stu-id="11197-124">The **Client** object exposes all the methods required to interact with device twins from the device.</span></span> <span data-ttu-id="11197-125">Den här koden initierar den **klienten** objekt, hämtar enheten dubbla för **myDeviceId**, och sedan kopplar en hanterare för uppdateringen på *önskade egenskaper*.</span><span class="sxs-lookup"><span data-stu-id="11197-125">This code initializes the **Client** object, retrieves the device twin for **myDeviceId**, and then attaches a handler for the update on *desired properties*.</span></span> <span data-ttu-id="11197-126">Hanteraren verifierar att det är en verklig configuration ändringsbegäran genom att jämföra configIds sedan anropar en metod som startar konfigurationsändringen.</span><span class="sxs-lookup"><span data-stu-id="11197-126">The handler verifies that there is an actual configuration change request by comparing the configIds, then invokes a method that starts the configuration change.</span></span>
   
    <span data-ttu-id="11197-127">Observera att för enkelhetens skull, den här koden använder en hårdkodad standard för den inledande konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="11197-127">Note that for the sake of simplicity, this code uses a hard-coded default for the initial configuration.</span></span> <span data-ttu-id="11197-128">En verklig app skulle förmodligen att läsa in konfigurationen från en lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="11197-128">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="11197-129">Önskad egenskapen Ändringshändelser släpps alltid en gång vid enhetsanslutning.</span><span class="sxs-lookup"><span data-stu-id="11197-129">Desired property change events are always emitted once at device connection.</span></span> <span data-ttu-id="11197-130">Se till att kontrollera att det finns en verklig ändring i egenskaperna innan du utför några åtgärder.</span><span class="sxs-lookup"><span data-stu-id="11197-130">Make sure to check that there is an actual change in the desired properties before performing any action.</span></span>
   > 
   > 
1. <span data-ttu-id="11197-131">Lägg till följande metoder innan den `client.open()` anrop:</span><span class="sxs-lookup"><span data-stu-id="11197-131">Add the following methods before the `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="11197-132">Den **initConfigChange** metoden uppdaterar rapporterade egenskaper på lokal dubbla enhetsobjektet med begäran om uppdatering konfiguration och anger statusen till **väntande**, uppdaterar enheten dubbla på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="11197-132">The **initConfigChange** method updates the reported properties on the local device twin object with the configuration update request and sets the status to **Pending**, then updates the device twin on the service.</span></span> <span data-ttu-id="11197-133">När en uppdatering av enheten dubbla, den simulerar en tidskrävande process som slutar i körningen av **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="11197-133">After successfully updating the device twin, it simulates a long running process that terminates in the execution of **completeConfigChange**.</span></span> <span data-ttu-id="11197-134">Den här metoden uppdaterar egenskaperna lokala rapporterade statusen angetts som **lyckade** och ta bort den **pendingConfig** objekt.</span><span class="sxs-lookup"><span data-stu-id="11197-134">This method updates the local reported properties setting the status to **Success** and removing the **pendingConfig** object.</span></span> <span data-ttu-id="11197-135">Sedan uppdaterar den enheten dubbla på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="11197-135">It then updates the device twin on the service.</span></span>
   
    <span data-ttu-id="11197-136">Observera att, för att spara bandbredd, rapporteras egenskaper uppdateras genom att ange egenskaperna som ska ändras (med namnet **korrigering** i koden ovan), i stället för att ersätta hela dokumentet.</span><span class="sxs-lookup"><span data-stu-id="11197-136">Note that, to save bandwidth, reported properties are updated by specifying only the properties to be modified (named **patch** in the above code), instead of replacing the whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="11197-137">Den här självstudiekursen simulerar inte någon funktion för samtidiga konfigurationsuppdateringar.</span><span class="sxs-lookup"><span data-stu-id="11197-137">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="11197-138">Vissa processer för uppdatering av konfiguration kanske kan hantera förändringar av mål-konfiguration när uppdateringen körs, kanske vissa måste placeras i kö och vissa kan avvisa dem med ett feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="11197-138">Some configuration update processes might be able to accommodate changes of target configuration while the update is running, some might have to queue them, and some could reject them with an error condition.</span></span> <span data-ttu-id="11197-139">Se till att tänka på önskat beteende för din specifika konfigurationsprocessen och Lägg till lämpliga logiken innan du påbörjar konfigurationsändringen.</span><span class="sxs-lookup"><span data-stu-id="11197-139">Make sure to consider the desired behavior for your specific configuration process, and add the appropriate logic before initiating the configuration change.</span></span>
   > 
   > 
1. <span data-ttu-id="11197-140">Kör appen enhet:</span><span class="sxs-lookup"><span data-stu-id="11197-140">Run the device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="11197-141">Du bör se meddelandet `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="11197-141">You should see the message `retrieved device twin`.</span></span> <span data-ttu-id="11197-142">Fortsätt att köra appen.</span><span class="sxs-lookup"><span data-stu-id="11197-142">Keep the app running.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="11197-143">Skapa service-appen</span><span class="sxs-lookup"><span data-stu-id="11197-143">Create the service app</span></span>
<span data-ttu-id="11197-144">I det här avsnittet skapar du en .NET-konsolapp som uppdaterar det *önskade egenskaper* på enhet-dubbla som är associerade med **myDeviceId** med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="11197-144">In this section, you will create a .NET console app that updates the *desired properties* on the device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="11197-145">Sedan frågar enheten-twins lagras i IoT-hubb och visar skillnaden mellan önskade och rapporterade konfigurationer av enheten.</span><span class="sxs-lookup"><span data-stu-id="11197-145">It then queries the device twins stored in the IoT hub and shows the difference between the desired and reported configurations of the device.</span></span>

1. <span data-ttu-id="11197-146">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den aktuella lösningen med hjälp av projektmallen **Konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="11197-146">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="11197-147">Namnge projektet **SetDesiredConfigurationAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="11197-147">Name the project **SetDesiredConfigurationAndQuery**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]
1. <span data-ttu-id="11197-149">I Solution Explorer högerklickar du på den **SetDesiredConfigurationAndQuery** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="11197-149">In Solution Explorer, right-click the **SetDesiredConfigurationAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="11197-150">Välj **Bläddra** i fönstret för **NuGet-pakethanteraren**, leta upp **microsoft.azure.devices**, välj **Installera** för att installera **Microsoft.Azure.Devices**-paketet och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="11197-150">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="11197-151">Denna procedur hämtar, installerar och lägger till en referens för [NuGet-paketetet SDK för Azure IoT-tjänster][lnk-nuget-service-sdk] och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="11197-151">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="11197-153">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="11197-153">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;
1. <span data-ttu-id="11197-154">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="11197-154">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="11197-155">Ersätt platshållarvärdet med IoT Hub-anslutningssträngen som du skapade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="11197-155">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="11197-156">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="11197-156">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="11197-157">Den **registret** objektet innehåller de metoder som krävs för att interagera med enheten twins från tjänsten.</span><span class="sxs-lookup"><span data-stu-id="11197-157">The **Registry** object exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="11197-158">Den här koden initierar den **registret** objekt, hämtar enheten dubbla för **myDeviceId**, och uppdaterar sedan dess egenskaper med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="11197-158">This code initializes the **Registry** object, retrieves the device twin for **myDeviceId**, and then updates its desired properties with a new telemetry configuration object.</span></span>
    <span data-ttu-id="11197-159">Efter att den frågar enheten-twins lagras i IoT-hubben var 10: e sekund och skriver ut önskad och rapporterade telemetri konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="11197-159">After that, it queries the device twins stored in the IoT hub every 10 seconds, and prints the desired and reported telemetry configurations.</span></span> <span data-ttu-id="11197-160">Referera till den [IoT-hubb frågespråket] [ lnk-query] att lära dig hur du skapar omfattande rapporter på alla enheter.</span><span class="sxs-lookup"><span data-stu-id="11197-160">Refer to the [IoT Hub query language][lnk-query] to learn how to generate rich reports across all your devices.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="11197-161">Det här programmet frågar IoT-hubb var 10: e sekund som illustration.</span><span class="sxs-lookup"><span data-stu-id="11197-161">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="11197-162">Använda frågor för att generera användarinriktad rapporter över flera enheter och inte för att identifiera ändringar.</span><span class="sxs-lookup"><span data-stu-id="11197-162">Use queries to generate user-facing reports across many devices, and not to detect changes.</span></span> <span data-ttu-id="11197-163">Om din lösning kräver meddelanden i realtid av enhetshändelser, använder [dubbla meddelanden][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="11197-163">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
   > 
   > 
1. <span data-ttu-id="11197-164">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="11197-164">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery().Wait();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();
1. <span data-ttu-id="11197-165">I Solution Explorer, öppna den **ange Startprojekt...**  och kontrollera att den **åtgärd** för **SetDesiredConfigurationAndQuery** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="11197-165">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **SetDesiredConfigurationAndQuery** project is **Start**.</span></span> <span data-ttu-id="11197-166">Skapa lösningen.</span><span class="sxs-lookup"><span data-stu-id="11197-166">Build the solution.</span></span>
1. <span data-ttu-id="11197-167">Med **SimulateDeviceConfiguration.js** körs, kör .NET-program från Visual Studio med hjälp av **F5** och du bör se rapporterade konfigurationsändringen från **lyckade** till **väntande** till **lyckade** igen med den nya aktivt frekvens som skickar fem minuter i stället för 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="11197-167">With **SimulateDeviceConfiguration.js** running, run the .NET application from Visual Studio using **F5** and you should see the reported configuration change from **Success** to **Pending** to **Success** again with the new active send frequency of five minutes instead of 24 hours.</span></span>

 ![Enheten har konfigurerats][img-deviceconfigured]
   
   > [!IMPORTANT]
   > <span data-ttu-id="11197-169">Det finns en fördröjning på upp till en minut mellan enheten rapporten igen och frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="11197-169">There is a delay of up to a minute between the device report operation and the query result.</span></span> <span data-ttu-id="11197-170">Detta är att aktivera query-infrastruktur för att arbeta med mycket hög skala.</span><span class="sxs-lookup"><span data-stu-id="11197-170">This is to enable the query infrastructure to work at very high scale.</span></span> <span data-ttu-id="11197-171">Att hämta konsekvent vyer för en enskild enhet dubbel användning av **getDeviceTwin** metod i den **registret** klass.</span><span class="sxs-lookup"><span data-stu-id="11197-171">To retrieve consistent views of a single device twin use the **getDeviceTwin** method in the **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="11197-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="11197-172">Next steps</span></span>
<span data-ttu-id="11197-173">I kursen får du ange en önskad konfiguration som *önskade egenskaper* från lösningen serverdel och skrev en enhetsapp för att identifiera den ändringen och simulera en flera steg uppdateringsprocess reporting dess status i rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="11197-173">In this tutorial, you set a desired configuration as *desired properties* from the solution back end, and wrote a device app to detect that change and simulate a multi-step update process reporting its status through the reported properties.</span></span>

<span data-ttu-id="11197-174">Använd följande resurser för att lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="11197-174">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="11197-175">Skicka telemetri från enheter med den [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="11197-175">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="11197-176">schemalägga eller utföra åtgärder på stora mängder enheter finns i [schema och broadcast jobb] [ lnk-schedule-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="11197-176">schedule or perform operations on large sets of devices see the [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="11197-177">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app), med den [metoder från direkt] [ lnk-methods-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="11197-177">control devices interactively (such as turning on a fan from a user-controlled app), with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

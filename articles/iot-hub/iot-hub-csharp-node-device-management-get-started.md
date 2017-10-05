---
title: "Kom igång med Azure IoT Hub enhetshantering (.NET/nod) | Microsoft Docs"
description: "Hur du använder hantering av Azure IoT Hub-enheter för att initiera en omstart av fjärranslutna enheter. Du kan använda Azure IoT-enhet SDK för Node.js för att implementera en simulerad enhetsapp som innehåller en direkt metod och Azure IoT SDK för .NET att implementera ett service-appen som anropar metoden direkt."
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
ms.openlocfilehash: d97fc5493570985f94c23032c870628d6a089dcd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-device-management-netnode"></a><span data-ttu-id="08475-104">Komma igång med hantering av enheter (.NET/nod)</span><span class="sxs-lookup"><span data-stu-id="08475-104">Get started with device management (.NET/Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="08475-105">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="08475-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="08475-106">Använda Azure portal för att skapa en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="08475-106">Use the Azure portal to create an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="08475-107">Skapa en simulerad enhetsapp som innehåller en direkt metod som startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="08475-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="08475-108">Direkta metoder anropas från molnet.</span><span class="sxs-lookup"><span data-stu-id="08475-108">Direct methods are invoked from the cloud.</span></span>
* <span data-ttu-id="08475-109">Skapa en .NET-konsolapp som anropar metoden omstart direkt i appen simulerade enheten via din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="08475-109">Create a .NET console app that calls the reboot direct method in the simulated device app through your IoT hub.</span></span>

<span data-ttu-id="08475-110">I slutet av den här kursen har du en enhet för Node.js konsolapp och en serverdel för .NET (C#) konsolapp:</span><span class="sxs-lookup"><span data-stu-id="08475-110">At the end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="08475-111">**dmpatterns_getstarted_device.js**, som ansluter till din IoT-hubb med enhetens identitet skapade tidigare, tar emot en direkt metod för omstart, simulerar en fysisk omstart och rapporterar tid för den senaste omstarten.</span><span class="sxs-lookup"><span data-stu-id="08475-111">**dmpatterns_getstarted_device.js**, which connects to your IoT hub with the device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports the time for the last reboot.</span></span>

<span data-ttu-id="08475-112">**TriggerReboot**, som anropar en direkt metod i appen simulerade enheten visar svaret och visar den uppdaterade rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="08475-112">**TriggerReboot**, which calls a direct method in the simulated device app, displays the response, and displays the updated reported properties.</span></span>

<span data-ttu-id="08475-113">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="08475-113">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="08475-114">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="08475-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="08475-115">Node.js-version 0.12.x eller senare</span><span class="sxs-lookup"><span data-stu-id="08475-115">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="08475-116">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur du installerar Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="08475-116">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="08475-117">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="08475-117">An active Azure account.</span></span> <span data-ttu-id="08475-118">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="08475-118">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a><span data-ttu-id="08475-119">Utlös en fjärransluten omstart på enheten med en direkt metod</span><span class="sxs-lookup"><span data-stu-id="08475-119">Trigger a remote reboot on the device using a direct method</span></span>
<span data-ttu-id="08475-120">I det här avsnittet skapar du en .NET-konsolapp (med C#) som initierar en fjärransluten omstart på en enhet med en direkt metod.</span><span class="sxs-lookup"><span data-stu-id="08475-120">In this section, you create a .NET console app (using C#) that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="08475-121">Appen använder enheten dubbla frågor för att identifiera senast omstart för enheten.</span><span class="sxs-lookup"><span data-stu-id="08475-121">The app uses device twin queries to discover the last reboot time for that device.</span></span>

1. <span data-ttu-id="08475-122">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den nya lösningen med hjälp av projektmallen **Konsolapp (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="08475-122">In Visual Studio, add a Visual C# Windows Classic Desktop project to a new solution by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="08475-123">Kontrollera att .NET Framework-versionen är 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="08475-123">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="08475-124">Namnge projektet **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="08475-124">Name the project **TriggerReboot**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]

2. <span data-ttu-id="08475-126">I Solution Explorer högerklickar du på den **TriggerReboot** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="08475-126">In Solution Explorer, right-click the **TriggerReboot** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="08475-127">Välj **Bläddra** i fönstret för **NuGet-pakethanteraren**, leta upp **microsoft.azure.devices**, välj **Installera** för att installera **Microsoft.Azure.Devices**-paketet och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="08475-127">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="08475-128">Denna procedur hämtar, installerar och lägger till en referens för [NuGet-paketetet SDK för Azure IoT-tjänster][lnk-nuget-service-sdk] och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="08475-128">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
4. <span data-ttu-id="08475-130">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="08475-130">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. <span data-ttu-id="08475-131">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="08475-131">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="08475-132">Ersätt platshållaren värdet med IoT-hubb anslutningssträngen för hubben som du skapade i föregående avsnitt och målenheten.</span><span class="sxs-lookup"><span data-stu-id="08475-132">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section and the target device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. <span data-ttu-id="08475-133">Lägg till följande metod för att den **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="08475-133">Add the following method to the **Program** class.</span></span>  <span data-ttu-id="08475-134">Den här koden hämtar enheten dubbla för om enheten och matar ut rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="08475-134">This code gets the device twin for the rebooting device and outputs the reported properties.</span></span>
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. <span data-ttu-id="08475-135">Lägg till följande metod för att den **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="08475-135">Add the following method to the **Program** class.</span></span>  <span data-ttu-id="08475-136">Den här koden initierar omstart på enheten med en direkt metod.</span><span class="sxs-lookup"><span data-stu-id="08475-136">This code initiates the reboot on the device using a direct method.</span></span>

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. <span data-ttu-id="08475-137">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="08475-137">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
        
8. <span data-ttu-id="08475-138">Skapa lösningen.</span><span class="sxs-lookup"><span data-stu-id="08475-138">Build the solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="08475-139">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="08475-139">Create a simulated device app</span></span>
<span data-ttu-id="08475-140">I det här avsnittet kommer du att</span><span class="sxs-lookup"><span data-stu-id="08475-140">In this section, you will</span></span>

* <span data-ttu-id="08475-141">Skapa en Node.js-konsolapp som svarar på en direkt metod som anropas via molnet</span><span class="sxs-lookup"><span data-stu-id="08475-141">Create a Node.js console app that responds to a direct method called by the cloud</span></span>
* <span data-ttu-id="08475-142">Utlös en simulerad enhet-omstart</span><span class="sxs-lookup"><span data-stu-id="08475-142">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="08475-143">Använda egenskaperna rapporterade för att aktivera enheten dubbla frågor för att identifiera enheter och när de senaste startas om</span><span class="sxs-lookup"><span data-stu-id="08475-143">Use the reported properties to enable device twin queries to identify devices and when they last rebooted</span></span>

1. <span data-ttu-id="08475-144">Skapa en ny tom mapp som kallas **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="08475-144">Create a new empty folder called **manageddevice**.</span></span>  <span data-ttu-id="08475-145">I mappen **manageddevice** skapar du en package.json-fil med hjälp av följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="08475-145">In the **manageddevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="08475-146">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="08475-146">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="08475-147">Vid en kommandotolk i den **manageddevice** mapp, kör följande kommando för att installera den **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt** paketet:</span><span class="sxs-lookup"><span data-stu-id="08475-147">At your command prompt in the **manageddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="08475-148">Med hjälp av en textredigerare, skapa en ny **dmpatterns_getstarted_device.js** filen i den **manageddevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="08475-148">Using a text editor, create a new **dmpatterns_getstarted_device.js** file in the **manageddevice** folder.</span></span>
4. <span data-ttu-id="08475-149">Lägg till följande 'krävs, instruktioner i början av den **dmpatterns_getstarted_device.js** fil:</span><span class="sxs-lookup"><span data-stu-id="08475-149">Add the following 'require' statements at the start of the **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="08475-150">Lägg till en **connectionString**-variabel och använd den för att skapa en **klientinstans**.</span><span class="sxs-lookup"><span data-stu-id="08475-150">Add a **connectionString** variable and use it to create a **Client** instance.</span></span>  <span data-ttu-id="08475-151">Ersätt anslutningssträngen med anslutningssträngen enhet.</span><span class="sxs-lookup"><span data-stu-id="08475-151">Replace the connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="08475-152">Lägg till följande funktion för att implementera metoden direkt på enheten</span><span class="sxs-lookup"><span data-stu-id="08475-152">Add the following function to implement the direct method on the device</span></span>
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
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
7. <span data-ttu-id="08475-153">Lägg till följande kod för att öppna anslutningen till din IoT-hubb och starta lyssnaren direkta metoden:</span><span class="sxs-lookup"><span data-stu-id="08475-153">Add the following code to open the connection to your IoT hub and start the direct method listener:</span></span>
   
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
8. <span data-ttu-id="08475-154">Spara och Stäng den **dmpatterns_getstarted_device.js** fil.</span><span class="sxs-lookup"><span data-stu-id="08475-154">Save and close the **dmpatterns_getstarted_device.js** file.</span></span>
   
> [!NOTE]
> <span data-ttu-id="08475-155">För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="08475-155">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="08475-156">I produktionskoden bör du implementera principer för omförsök (till exempel en exponentiell backoff), vilket rekommenderas i MSDN-artikeln om [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="08475-156">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>


## <a name="run-the-apps"></a><span data-ttu-id="08475-157">Kör apparna</span><span class="sxs-lookup"><span data-stu-id="08475-157">Run the apps</span></span>
<span data-ttu-id="08475-158">Nu är det dags att köra apparna.</span><span class="sxs-lookup"><span data-stu-id="08475-158">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="08475-159">I Kommandotolken i den **manageddevice** mapp, kör följande kommando för att börja lyssna efter omstart direkta metoden.</span><span class="sxs-lookup"><span data-stu-id="08475-159">At the command prompt in the **manageddevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="08475-160">Kör C#-konsolapp **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="08475-160">Run the C# console app **TriggerReboot**.</span></span> <span data-ttu-id="08475-161">Högerklicka på den **TriggerReboot** projektet, Välj **felsöka**, och välj sedan **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="08475-161">Right-click the **TriggerReboot** project, select **Debug**, and then select **Start new instance**.</span></span>

3. <span data-ttu-id="08475-162">Svaret från enheten till den direkta metoden i konsolen visas.</span><span class="sxs-lookup"><span data-stu-id="08475-162">You see the device response to the direct method in the console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png
[img-servicenuget]: media/iot-hub-csharp-node-device-management-get-started/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-device-management-get-started/createnetapp.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
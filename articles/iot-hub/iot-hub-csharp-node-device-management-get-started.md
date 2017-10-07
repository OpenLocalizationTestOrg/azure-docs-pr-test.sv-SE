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
# <a name="get-started-with-device-management-netnode"></a><span data-ttu-id="d9383-104">Komma igång med hantering av enheter (.NET/nod)</span><span class="sxs-lookup"><span data-stu-id="d9383-104">Get started with device management (.NET/Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="d9383-105">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="d9383-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="d9383-106">Använd hello Azure portal toocreate en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d9383-106">Use hello Azure portal toocreate an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="d9383-107">Skapa en simulerad enhetsapp som innehåller en direkt metod som startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="d9383-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="d9383-108">Direkta metoder anropas från hello molnet.</span><span class="sxs-lookup"><span data-stu-id="d9383-108">Direct methods are invoked from hello cloud.</span></span>
* <span data-ttu-id="d9383-109">Skapa en .NET-konsolapp som anropar hello omstart direkt metod i hello simulerade enheten appen via din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d9383-109">Create a .NET console app that calls hello reboot direct method in hello simulated device app through your IoT hub.</span></span>

<span data-ttu-id="d9383-110">Hello slutet av den här självstudiekursen har du en enhet för Node.js konsolapp och en serverdel för .NET (C#) konsolapp:</span><span class="sxs-lookup"><span data-stu-id="d9383-110">At hello end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="d9383-111">**dmpatterns_getstarted_device.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare, tar emot en direkt metod för omstart simulerar en fysisk omstart och rapporterar hello tid för hello senaste omstarten.</span><span class="sxs-lookup"><span data-stu-id="d9383-111">**dmpatterns_getstarted_device.js**, which connects tooyour IoT hub with hello device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports hello time for hello last reboot.</span></span>

<span data-ttu-id="d9383-112">**TriggerReboot**, som anropar en direkt metod i hello simulerade enhetsapp visar hello svar och visar hello uppdateras rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="d9383-112">**TriggerReboot**, which calls a direct method in hello simulated device app, displays hello response, and displays hello updated reported properties.</span></span>

<span data-ttu-id="d9383-113">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="d9383-113">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="d9383-114">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d9383-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="d9383-115">Node.js-version 0.12.x eller senare</span><span class="sxs-lookup"><span data-stu-id="d9383-115">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="d9383-116">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="d9383-116">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="d9383-117">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d9383-117">An active Azure account.</span></span> <span data-ttu-id="d9383-118">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="d9383-118">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a><span data-ttu-id="d9383-119">Utlös en fjärransluten omstart på hello-enhet med en direkt metod</span><span class="sxs-lookup"><span data-stu-id="d9383-119">Trigger a remote reboot on hello device using a direct method</span></span>
<span data-ttu-id="d9383-120">I det här avsnittet skapar du en .NET-konsolapp (med C#) som initierar en fjärransluten omstart på en enhet med en direkt metod.</span><span class="sxs-lookup"><span data-stu-id="d9383-120">In this section, you create a .NET console app (using C#) that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="d9383-121">hello appen använder enheten dubbla frågor toodiscover hello omstart senast för enheten.</span><span class="sxs-lookup"><span data-stu-id="d9383-121">hello app uses device twin queries toodiscover hello last reboot time for that device.</span></span>

1. <span data-ttu-id="d9383-122">Lägga till en ny lösning för Visual C# klassiska skrivbordet projektet tooa i Visual Studio med hello **Konsolapp (.NET Framework)** projektmall.</span><span class="sxs-lookup"><span data-stu-id="d9383-122">In Visual Studio, add a Visual C# Windows Classic Desktop project tooa new solution by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="d9383-123">Se till att hello av .NET Framework 4.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d9383-123">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="d9383-124">Namnet hello projektet **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="d9383-124">Name hello project **TriggerReboot**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]

2. <span data-ttu-id="d9383-126">I Solution Explorer högerklickar du på hello **TriggerReboot** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="d9383-126">In Solution Explorer, right-click hello **TriggerReboot** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="d9383-127">I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="d9383-127">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="d9383-128">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="d9383-128">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
4. <span data-ttu-id="d9383-130">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="d9383-130">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Shared;
        
5. <span data-ttu-id="d9383-131">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="d9383-131">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="d9383-132">Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello och hello målenhet.</span><span class="sxs-lookup"><span data-stu-id="d9383-132">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section and hello target device.</span></span>
   
        static RegistryManager registryManager;
        static string connString = "{iot hub connection string}";
        static ServiceClient client;
        static JobClient jobClient;
        static string targetDevice = "{deviceIdForTargetDevice}";
        
6. <span data-ttu-id="d9383-133">Lägg till följande metod toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="d9383-133">Add hello following method toohello **Program** class.</span></span>  <span data-ttu-id="d9383-134">Den här koden hämtar hello enheten dubbla för hello startas om enheten och utdata hello rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="d9383-134">This code gets hello device twin for hello rebooting device and outputs hello reported properties.</span></span>
   
        public static async Task QueryTwinRebootReported()
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);
            Console.WriteLine(twin.Properties.Reported.ToJson());
        }
        
7. <span data-ttu-id="d9383-135">Lägg till följande metod toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="d9383-135">Add hello following method toohello **Program** class.</span></span>  <span data-ttu-id="d9383-136">Den här koden initierar hello omstart på hello-enhet med en direkt metod.</span><span class="sxs-lookup"><span data-stu-id="d9383-136">This code initiates hello reboot on hello device using a direct method.</span></span>

        public static async Task StartReboot()
        {
            client = ServiceClient.CreateFromConnectionString(connString);
            CloudToDeviceMethod method = new CloudToDeviceMethod("reboot");
            method.ResponseTimeout = TimeSpan.FromSeconds(30);

            CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

            Console.WriteLine("Invoked firmware update on device.");
        }

7. <span data-ttu-id="d9383-137">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="d9383-137">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connString);
        StartReboot().Wait();
        QueryTwinRebootReported().Wait();
        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
        
8. <span data-ttu-id="d9383-138">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="d9383-138">Build hello solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="d9383-139">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="d9383-139">Create a simulated device app</span></span>
<span data-ttu-id="d9383-140">I det här avsnittet kommer du att</span><span class="sxs-lookup"><span data-stu-id="d9383-140">In this section, you will</span></span>

* <span data-ttu-id="d9383-141">Skapa en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln</span><span class="sxs-lookup"><span data-stu-id="d9383-141">Create a Node.js console app that responds tooa direct method called by hello cloud</span></span>
* <span data-ttu-id="d9383-142">Utlös en simulerad enhet-omstart</span><span class="sxs-lookup"><span data-stu-id="d9383-142">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="d9383-143">Använd hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast startas om</span><span class="sxs-lookup"><span data-stu-id="d9383-143">Use hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted</span></span>

1. <span data-ttu-id="d9383-144">Skapa en ny tom mapp som kallas **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="d9383-144">Create a new empty folder called **manageddevice**.</span></span>  <span data-ttu-id="d9383-145">I hello **manageddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="d9383-145">In hello **manageddevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="d9383-146">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="d9383-146">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="d9383-147">Vid en kommandotolk i hello **manageddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:</span><span class="sxs-lookup"><span data-stu-id="d9383-147">At your command prompt in hello **manageddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="d9383-148">Med hjälp av en textredigerare, skapa en ny **dmpatterns_getstarted_device.js** filen i hello **manageddevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="d9383-148">Using a text editor, create a new **dmpatterns_getstarted_device.js** file in hello **manageddevice** folder.</span></span>
4. <span data-ttu-id="d9383-149">Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_getstarted_device.js** fil:</span><span class="sxs-lookup"><span data-stu-id="d9383-149">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="d9383-150">Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans.</span><span class="sxs-lookup"><span data-stu-id="d9383-150">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span>  <span data-ttu-id="d9383-151">Ersätt hello anslutningssträngen med anslutningssträngen enhet.</span><span class="sxs-lookup"><span data-stu-id="d9383-151">Replace hello connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="d9383-152">Lägg till hello följande funktion tooimplement hello direkt metod på hello-enhet</span><span class="sxs-lookup"><span data-stu-id="d9383-152">Add hello following function tooimplement hello direct method on hello device</span></span>
   
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
7. <span data-ttu-id="d9383-153">Lägg till följande hello code tooopen hello anslutning tooyour IoT-hubb och starta hello direkta metoden lyssnare:</span><span class="sxs-lookup"><span data-stu-id="d9383-153">Add hello following code tooopen hello connection tooyour IoT hub and start hello direct method listener:</span></span>
   
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
8. <span data-ttu-id="d9383-154">Spara och Stäng hello **dmpatterns_getstarted_device.js** fil.</span><span class="sxs-lookup"><span data-stu-id="d9383-154">Save and close hello **dmpatterns_getstarted_device.js** file.</span></span>
   
> [!NOTE]
> <span data-ttu-id="d9383-155">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="d9383-155">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="d9383-156">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="d9383-156">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>


## <a name="run-hello-apps"></a><span data-ttu-id="d9383-157">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="d9383-157">Run hello apps</span></span>
<span data-ttu-id="d9383-158">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="d9383-158">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="d9383-159">Kommandotolken hello i hello **manageddevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.</span><span class="sxs-lookup"><span data-stu-id="d9383-159">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="d9383-160">Kör hello C#-konsolapp **TriggerReboot**.</span><span class="sxs-lookup"><span data-stu-id="d9383-160">Run hello C# console app **TriggerReboot**.</span></span> <span data-ttu-id="d9383-161">Högerklicka på hello **TriggerReboot** projektet, Välj **felsöka**, och välj sedan **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="d9383-161">Right-click hello **TriggerReboot** project, select **Debug**, and then select **Start new instance**.</span></span>

3. <span data-ttu-id="d9383-162">Du ser hello enheten svar toohello direkt metod i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="d9383-162">You see hello device response toohello direct method in hello console.</span></span>

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
---
title: "Kom igång med Azure IoT Hub enhetshantering (nod) | Microsoft Docs"
description: "Hur du använder IoT-hubb enhetshantering för att initiera en omstart av fjärranslutna enheter. Du kan använda Azure IoT-SDK för Node.js för att implementera en simulerad enhetsapp som innehåller en direkt metod och en service-appen som anropar metoden direkt."
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
ms.date: 08/25/2017
ms.author: juanpere
ms.openlocfilehash: 332a3e62cb1ef75e2c6dd5562ee799465c401128
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-device-management-node"></a><span data-ttu-id="5a8e8-104">Komma igång med hantering av enheter (nod)</span><span class="sxs-lookup"><span data-stu-id="5a8e8-104">Get started with device management (Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="5a8e8-105">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="5a8e8-106">Använda Azure portal för att skapa en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-106">Use the Azure portal to create an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="5a8e8-107">Skapa en simulerad enhetsapp som innehåller en direkt metod som startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="5a8e8-108">Direkta metoder anropas från molnet.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-108">Direct methods are invoked from the cloud.</span></span>
* <span data-ttu-id="5a8e8-109">Skapa en Node.js-konsolprogram som anropar metoden omstart direkt i appen simulerade enheten via din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-109">Create a Node.js console app that calls the reboot direct method in the simulated device app through your IoT hub.</span></span>

<span data-ttu-id="5a8e8-110">I slutet av den här kursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-110">At the end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="5a8e8-111">**dmpatterns_getstarted_device.js**, som ansluter till din IoT-hubb med enhetens identitet skapade tidigare, tar emot en direkt metod för omstart, simulerar en fysisk omstart och rapporterar tid för den senaste omstarten.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-111">**dmpatterns_getstarted_device.js**, which connects to your IoT hub with the device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports the time for the last reboot.</span></span>

<span data-ttu-id="5a8e8-112">**dmpatterns_getstarted_service.js**, som anropar en direkt metod i appen simulerade enheten visar svaret och visar den uppdaterade rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-112">**dmpatterns_getstarted_service.js**, which calls a direct method in the simulated device app, displays the response, and displays the updated reported properties.</span></span>

<span data-ttu-id="5a8e8-113">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-113">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="5a8e8-114">Node.js-version 0.12.x eller senare</span><span class="sxs-lookup"><span data-stu-id="5a8e8-114">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="5a8e8-115">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur du installerar Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-115">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="5a8e8-116">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-116">An active Azure account.</span></span> <span data-ttu-id="5a8e8-117">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="5a8e8-117">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="5a8e8-118">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="5a8e8-118">Create a simulated device app</span></span>
<span data-ttu-id="5a8e8-119">I det här avsnittet kommer du att</span><span class="sxs-lookup"><span data-stu-id="5a8e8-119">In this section, you will</span></span>

* <span data-ttu-id="5a8e8-120">Skapa en Node.js-konsolapp som svarar på en direkt metod som anropas via molnet</span><span class="sxs-lookup"><span data-stu-id="5a8e8-120">Create a Node.js console app that responds to a direct method called by the cloud</span></span>
* <span data-ttu-id="5a8e8-121">Utlös en simulerad enhet-omstart</span><span class="sxs-lookup"><span data-stu-id="5a8e8-121">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="5a8e8-122">Använda egenskaperna rapporterade för att aktivera enheten dubbla frågor för att identifiera enheter och när de senaste startas om</span><span class="sxs-lookup"><span data-stu-id="5a8e8-122">Use the reported properties to enable device twin queries to identify devices and when they last rebooted</span></span>

1. <span data-ttu-id="5a8e8-123">Skapa en tom mapp med namnet **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-123">Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="5a8e8-124">I mappen **manageddevice** skapar du en package.json-fil med hjälp av följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-124">In the **manageddevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="5a8e8-125">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-125">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="5a8e8-126">Vid en kommandotolk i den **manageddevice** mapp, kör följande kommando för att installera den **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt** paketet:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-126">At your command prompt in the **manageddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="5a8e8-127">Med hjälp av en textredigerare, skapa en **dmpatterns_getstarted_device.js** filen i den **manageddevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-127">Using a text editor, create a **dmpatterns_getstarted_device.js** file in the **manageddevice** folder.</span></span>
4. <span data-ttu-id="5a8e8-128">Lägg till följande 'krävs, instruktioner i början av den **dmpatterns_getstarted_device.js** fil:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-128">Add the following 'require' statements at the start of the **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="5a8e8-129">Lägg till en **connectionString**-variabel och använd den för att skapa en **klientinstans**.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-129">Add a **connectionString** variable and use it to create a **Client** instance.</span></span>  <span data-ttu-id="5a8e8-130">Ersätt anslutningssträngen med anslutningssträngen enhet.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-130">Replace the connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="5a8e8-131">Lägg till följande funktion för att implementera metoden direkt på enheten</span><span class="sxs-lookup"><span data-stu-id="5a8e8-131">Add the following function to implement the direct method on the device</span></span>
   
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
7. <span data-ttu-id="5a8e8-132">Öppna anslutning till din IoT-hubb och starta lyssnaren direkta metoden:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-132">Open the connection to your IoT hub and start the direct method listener:</span></span>
   
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
8. <span data-ttu-id="5a8e8-133">Spara och Stäng den **dmpatterns_getstarted_device.js** fil.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-133">Save and close the **dmpatterns_getstarted_device.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="5a8e8-134">För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-134">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="5a8e8-135">I produktionskoden bör du implementera principer för omförsök (till exempel en exponentiell backoff), vilket rekommenderas i MSDN-artikeln om [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="5a8e8-135">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a><span data-ttu-id="5a8e8-136">Utlös en fjärransluten omstart på enheten med en direkt metod</span><span class="sxs-lookup"><span data-stu-id="5a8e8-136">Trigger a remote reboot on the device using a direct method</span></span>
<span data-ttu-id="5a8e8-137">I det här avsnittet skapar du en Node.js-konsolprogram som initierar en fjärransluten omstart på en enhet med en direkt metod.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-137">In this section, you create a Node.js console app that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="5a8e8-138">Appen använder enheten dubbla frågor för att identifiera senast omstart för enheten.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-138">The app uses device twin queries to discover the last reboot time for that device.</span></span>

1. <span data-ttu-id="5a8e8-139">Skapa en tom mapp som kallas **triggerrebootondevice**.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-139">Create an empty folder called **triggerrebootondevice**.</span></span>  <span data-ttu-id="5a8e8-140">I den **triggerrebootondevice** mapp, skapa en package.json-fil med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-140">In the **triggerrebootondevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="5a8e8-141">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-141">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="5a8e8-142">Vid en kommandotolk i den **triggerrebootondevice** mapp, kör följande kommando för att installera den **azure iothub** enheten SDK-paketet och **azure-iot-enhet – mqtt** paketet:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-142">At your command prompt in the **triggerrebootondevice** folder, run the following command to install the **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="5a8e8-143">Med hjälp av en textredigerare, skapa en **dmpatterns_getstarted_service.js** filen i den **triggerrebootondevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-143">Using a text editor, create a **dmpatterns_getstarted_service.js** file in the **triggerrebootondevice** folder.</span></span>
4. <span data-ttu-id="5a8e8-144">Lägg till följande 'krävs, instruktioner i början av den **dmpatterns_getstarted_service.js** fil:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-144">Add the following 'require' statements at the start of the **dmpatterns_getstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="5a8e8-145">Lägg till följande variabeldeklarationer och ersätta platshållarvärdena:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-145">Add the following variable declarations and replace the placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. <span data-ttu-id="5a8e8-146">Lägg till följande funktion för att anropa metoden enheten för att starta om målenheten:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-146">Add the following function to invoke the device method to reboot the target device:</span></span>
   
    ```
    var startRebootDevice = function(twin) {
   
        var methodName = "reboot";
   
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
   
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```
7. <span data-ttu-id="5a8e8-147">Lägg till följande funktion för att fråga efter enheten och få omstart senast:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-147">Add the following function to query for the device and get the last reboot time:</span></span>
   
    ```
    var queryTwinLastReboot = function() {
   
        registry.getTwin(deviceToReboot, function(err, twin){
   
            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
8. <span data-ttu-id="5a8e8-148">Lägg till följande kod för att anropa funktionerna som utlöser direkt metod för omstart och fråga för omstart senast:</span><span class="sxs-lookup"><span data-stu-id="5a8e8-148">Add the following code to call the functions that trigger the reboot direct method and query for the last reboot time:</span></span>
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. <span data-ttu-id="5a8e8-149">Spara och Stäng den **dmpatterns_getstarted_service.js** fil.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-149">Save and close the **dmpatterns_getstarted_service.js** file.</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="5a8e8-150">Kör apparna</span><span class="sxs-lookup"><span data-stu-id="5a8e8-150">Run the apps</span></span>
<span data-ttu-id="5a8e8-151">Nu är det dags att köra apparna.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-151">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="5a8e8-152">I Kommandotolken i den **manageddevice** mapp, kör följande kommando för att börja lyssna efter omstart direkta metoden.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-152">At the command prompt in the **manageddevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="5a8e8-153">I Kommandotolken i den **triggerrebootondevice** mapp, kör följande kommando för att utlösa remote omstart och fråga för enheten dubbla att söka efter senaste omstart tid.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-153">At the command prompt in the **triggerrebootondevice** folder, run the following command to trigger the remote reboot and query for the device twin to find the last reboot time.</span></span>
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. <span data-ttu-id="5a8e8-154">Svaret från enheten till den direkta metoden i konsolen visas.</span><span class="sxs-lookup"><span data-stu-id="5a8e8-154">You see the device response to the direct method in the console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

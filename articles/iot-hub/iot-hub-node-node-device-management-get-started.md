---
title: "aaaGet igång med Azure IoT Hub enhetshantering (nod) | Microsoft Docs"
description: "Hur toouse IoT-hubb device management tooinitiate fjärranslutna enheten startas om. Du kan använda hello Azure IoT-SDK för Node.js tooimplement en simulerad enhetsapp som innehåller en direkt metod och en service-appen som anropar hello direkta metoden."
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
ms.openlocfilehash: 5dd1878e71231850fb95f4170b823f1e86c3ee83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-node"></a><span data-ttu-id="04500-104">Komma igång med hantering av enheter (nod)</span><span class="sxs-lookup"><span data-stu-id="04500-104">Get started with device management (Node)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="04500-105">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="04500-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="04500-106">Använd hello Azure portal toocreate en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="04500-106">Use hello Azure portal toocreate an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="04500-107">Skapa en simulerad enhetsapp som innehåller en direkt metod som startar om enheten.</span><span class="sxs-lookup"><span data-stu-id="04500-107">Create a simulated device app that contains a direct method that reboots that device.</span></span> <span data-ttu-id="04500-108">Direkta metoder anropas från hello molnet.</span><span class="sxs-lookup"><span data-stu-id="04500-108">Direct methods are invoked from hello cloud.</span></span>
* <span data-ttu-id="04500-109">Skapa en Node.js-konsolprogram som anropar hello omstart direkt metod i hello simulerade enheten appen via din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="04500-109">Create a Node.js console app that calls hello reboot direct method in hello simulated device app through your IoT hub.</span></span>

<span data-ttu-id="04500-110">Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="04500-110">At hello end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="04500-111">**dmpatterns_getstarted_device.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare, tar emot en direkt metod för omstart simulerar en fysisk omstart och rapporterar hello tid för hello senaste omstarten.</span><span class="sxs-lookup"><span data-stu-id="04500-111">**dmpatterns_getstarted_device.js**, which connects tooyour IoT hub with hello device identity created earlier, receives a reboot direct method, simulates a physical reboot, and reports hello time for hello last reboot.</span></span>

<span data-ttu-id="04500-112">**dmpatterns_getstarted_service.js**, som anropar en direkt metod i hello simulerade enhetsapp visar hello svar och visar hello uppdateras rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="04500-112">**dmpatterns_getstarted_service.js**, which calls a direct method in hello simulated device app, displays hello response, and displays hello updated reported properties.</span></span>

<span data-ttu-id="04500-113">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="04500-113">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="04500-114">Node.js-version 0.12.x eller senare</span><span class="sxs-lookup"><span data-stu-id="04500-114">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="04500-115">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="04500-115">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="04500-116">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="04500-116">An active Azure account.</span></span> <span data-ttu-id="04500-117">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="04500-117">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="04500-118">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="04500-118">Create a simulated device app</span></span>
<span data-ttu-id="04500-119">I det här avsnittet kommer du att</span><span class="sxs-lookup"><span data-stu-id="04500-119">In this section, you will</span></span>

* <span data-ttu-id="04500-120">Skapa en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln</span><span class="sxs-lookup"><span data-stu-id="04500-120">Create a Node.js console app that responds tooa direct method called by hello cloud</span></span>
* <span data-ttu-id="04500-121">Utlös en simulerad enhet-omstart</span><span class="sxs-lookup"><span data-stu-id="04500-121">Trigger a simulated device reboot</span></span>
* <span data-ttu-id="04500-122">Använd hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast startas om</span><span class="sxs-lookup"><span data-stu-id="04500-122">Use hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted</span></span>

1. <span data-ttu-id="04500-123">Skapa en tom mapp med namnet **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="04500-123">Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="04500-124">I hello **manageddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="04500-124">In hello **manageddevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="04500-125">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="04500-125">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="04500-126">Vid en kommandotolk i hello **manageddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:</span><span class="sxs-lookup"><span data-stu-id="04500-126">At your command prompt in hello **manageddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="04500-127">Med hjälp av en textredigerare, skapa en **dmpatterns_getstarted_device.js** filen i hello **manageddevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="04500-127">Using a text editor, create a **dmpatterns_getstarted_device.js** file in hello **manageddevice** folder.</span></span>
4. <span data-ttu-id="04500-128">Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_getstarted_device.js** fil:</span><span class="sxs-lookup"><span data-stu-id="04500-128">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="04500-129">Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans.</span><span class="sxs-lookup"><span data-stu-id="04500-129">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span>  <span data-ttu-id="04500-130">Ersätt hello anslutningssträngen med anslutningssträngen enhet.</span><span class="sxs-lookup"><span data-stu-id="04500-130">Replace hello connection string with your device connection string.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="04500-131">Lägg till hello följande funktion tooimplement hello direkt metod på hello-enhet</span><span class="sxs-lookup"><span data-stu-id="04500-131">Add hello following function tooimplement hello direct method on hello device</span></span>
   
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
7. <span data-ttu-id="04500-132">Öppna hello anslutning tooyour IoT-hubb och starta hello direkta metoden lyssnare:</span><span class="sxs-lookup"><span data-stu-id="04500-132">Open hello connection tooyour IoT hub and start hello direct method listener:</span></span>
   
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
8. <span data-ttu-id="04500-133">Spara och Stäng hello **dmpatterns_getstarted_device.js** fil.</span><span class="sxs-lookup"><span data-stu-id="04500-133">Save and close hello **dmpatterns_getstarted_device.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="04500-134">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="04500-134">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="04500-135">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="04500-135">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a><span data-ttu-id="04500-136">Utlös en fjärransluten omstart på hello-enhet med en direkt metod</span><span class="sxs-lookup"><span data-stu-id="04500-136">Trigger a remote reboot on hello device using a direct method</span></span>
<span data-ttu-id="04500-137">I det här avsnittet skapar du en Node.js-konsolprogram som initierar en fjärransluten omstart på en enhet med en direkt metod.</span><span class="sxs-lookup"><span data-stu-id="04500-137">In this section, you create a Node.js console app that initiates a remote reboot on a device using a direct method.</span></span> <span data-ttu-id="04500-138">hello appen använder enheten dubbla frågor toodiscover hello omstart senast för enheten.</span><span class="sxs-lookup"><span data-stu-id="04500-138">hello app uses device twin queries toodiscover hello last reboot time for that device.</span></span>

1. <span data-ttu-id="04500-139">Skapa en tom mapp som kallas **triggerrebootondevice**.</span><span class="sxs-lookup"><span data-stu-id="04500-139">Create an empty folder called **triggerrebootondevice**.</span></span>  <span data-ttu-id="04500-140">I hello **triggerrebootondevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="04500-140">In hello **triggerrebootondevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="04500-141">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="04500-141">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="04500-142">Vid en kommandotolk i hello **triggerrebootondevice** mapp, kör följande kommando tooinstall hello hello **azure iothub** enheten SDK-paketet och **azure-iot-enhet-mqtt** paketet:</span><span class="sxs-lookup"><span data-stu-id="04500-142">At your command prompt in hello **triggerrebootondevice** folder, run hello following command tooinstall hello **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="04500-143">Med hjälp av en textredigerare, skapa en **dmpatterns_getstarted_service.js** filen i hello **triggerrebootondevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="04500-143">Using a text editor, create a **dmpatterns_getstarted_service.js** file in hello **triggerrebootondevice** folder.</span></span>
4. <span data-ttu-id="04500-144">Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_getstarted_service.js** fil:</span><span class="sxs-lookup"><span data-stu-id="04500-144">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="04500-145">Lägg till följande variabeldeklarationer hello och ersätta platshållarvärdena hello:</span><span class="sxs-lookup"><span data-stu-id="04500-145">Add hello following variable declarations and replace hello placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. <span data-ttu-id="04500-146">Lägg till följande funktion tooinvoke hello enheten metoden tooreboot hello målenhet hello:</span><span class="sxs-lookup"><span data-stu-id="04500-146">Add hello following function tooinvoke hello device method tooreboot hello target device:</span></span>
   
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
                console.log("Successfully invoked hello device tooreboot.");  
            }
        });
    };
    ```
7. <span data-ttu-id="04500-147">Lägg till hello följande fungera tooquery för hello enhet och få hello omstart senast:</span><span class="sxs-lookup"><span data-stu-id="04500-147">Add hello following function tooquery for hello device and get hello last reboot time:</span></span>
   
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
                console.log('Waiting for device tooreport last reboot time.');
        });
    };
    ```
8. <span data-ttu-id="04500-148">Lägg till följande kod toocall hello funktioner som utlöser hello hello omstart direkta metoden och fråga efter Hej senast omstart tid:</span><span class="sxs-lookup"><span data-stu-id="04500-148">Add hello following code toocall hello functions that trigger hello reboot direct method and query for hello last reboot time:</span></span>
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. <span data-ttu-id="04500-149">Spara och Stäng hello **dmpatterns_getstarted_service.js** fil.</span><span class="sxs-lookup"><span data-stu-id="04500-149">Save and close hello **dmpatterns_getstarted_service.js** file.</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="04500-150">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="04500-150">Run hello apps</span></span>
<span data-ttu-id="04500-151">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="04500-151">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="04500-152">Kommandotolken hello i hello **manageddevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.</span><span class="sxs-lookup"><span data-stu-id="04500-152">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. <span data-ttu-id="04500-153">Kommandotolken hello i hello **triggerrebootondevice** , kör följande kommando tootrigger hello remote hello omstart och fråga för hello enheten dubbla toofind hello senast starta om tid.</span><span class="sxs-lookup"><span data-stu-id="04500-153">At hello command prompt in hello **triggerrebootondevice** folder, run hello following command tootrigger hello remote reboot and query for hello device twin toofind hello last reboot time.</span></span>
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. <span data-ttu-id="04500-154">Du ser hello enheten svar toohello direkt metod i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="04500-154">You see hello device response toohello direct method in hello console.</span></span>

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups toomanage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

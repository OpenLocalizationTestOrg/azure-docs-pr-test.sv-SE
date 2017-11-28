---
title: aaaAzure IoT-hubb direkt metoder (nod) | Microsoft Docs
description: "Hur toouse Azure IoT Hub direkt metoder. Du kan använda hello Azure IoT SDK för Node.js tooimplement en simulerad enhetsapp som innehåller en direkt metod och en service-appen som anropar hello direkta metoden."
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ea9c73ca-7778-4e38-a8f1-0bee9d142f04
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 12300ba451816fec1f80163b633f6b6e411d9e5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a><span data-ttu-id="2e1de-104">Använd direkt metoderna på din IoT-enhet med Node.js</span><span class="sxs-lookup"><span data-stu-id="2e1de-104">Use direct methods on your IoT device with Node.js</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="2e1de-105">Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="2e1de-105">At hello end of this tutorial, you have two Node.js console apps:</span></span>

* <span data-ttu-id="2e1de-106">**CallMethodOnDevice.js**, som anropar en metod i hello simulerade enhetsapp och visar hello svar.</span><span class="sxs-lookup"><span data-stu-id="2e1de-106">**CallMethodOnDevice.js**, which calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="2e1de-107">**SimulatedDevice.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och svarar toohello-metoden anropas av hello molnet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-107">**SimulatedDevice.js**, which connects tooyour IoT hub with hello device identity created earlier, and responds toohello method called by hello cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="2e1de-108">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="2e1de-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="2e1de-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="2e1de-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="2e1de-110">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="2e1de-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="2e1de-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="2e1de-111">An active Azure account.</span></span> <span data-ttu-id="2e1de-112">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="2e1de-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="2e1de-113">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="2e1de-113">Create a simulated device app</span></span>
<span data-ttu-id="2e1de-114">I det här avsnittet skapar du en Node.js-konsolprogram som svarar tooa-metoden anropas av hello molnet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-114">In this section, you create a Node.js console app that responds tooa method called by hello cloud.</span></span>

1. <span data-ttu-id="2e1de-115">Skapa en ny tom mapp med namnet **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="2e1de-115">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="2e1de-116">I hello **simulateddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="2e1de-116">In hello **simulateddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="2e1de-117">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="2e1de-117">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="2e1de-118">Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:</span><span class="sxs-lookup"><span data-stu-id="2e1de-118">At your command prompt in hello **simulateddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="2e1de-119">Med hjälp av en textredigerare, skapa en ny **SimulatedDevice.js** filen i hello **simulateddevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="2e1de-119">Using a text editor, create a new **SimulatedDevice.js** file in hello **simulateddevice** folder.</span></span>
4. <span data-ttu-id="2e1de-120">Lägg till följande hello `require` uttryck högst hello början av hello **SimulatedDevice.js** fil:</span><span class="sxs-lookup"><span data-stu-id="2e1de-120">Add hello following `require` statements at hello start of hello **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="2e1de-121">Lägg till en **connectionString** variabel och använda den toocreate en **DeviceClient** instans.</span><span class="sxs-lookup"><span data-stu-id="2e1de-121">Add a **connectionString** variable and use it toocreate a **DeviceClient** instance.</span></span> <span data-ttu-id="2e1de-122">Ersätt **{enheten anslutningssträngen}** med hello enheten anslutningssträng som du genererade i hello *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="2e1de-122">Replace **{device connection string}** with hello device connection string you generated in hello *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="2e1de-123">Lägg till hello följande funktion tooimplement hello metod på hello enhet:</span><span class="sxs-lookup"><span data-stu-id="2e1de-123">Add hello following function tooimplement hello method on hello device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written toolog.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="2e1de-124">Öppna hello anslutning tooyour IoT-hubb och börja initiera hello metoden lyssnare:</span><span class="sxs-lookup"><span data-stu-id="2e1de-124">Open hello connection tooyour IoT hub and start initialize hello method listener:</span></span>
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. <span data-ttu-id="2e1de-125">Spara och Stäng hello **SimulatedDevice.js** fil.</span><span class="sxs-lookup"><span data-stu-id="2e1de-125">Save and close hello **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="2e1de-126">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="2e1de-126">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="2e1de-127">I produktionskod, bör du implementera försök principer (till exempel anslutning försök), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="2e1de-127">In production code, you should implement retry policies (such as connection retry), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-method-on-a-device"></a><span data-ttu-id="2e1de-128">Anropa en metod på en enhet</span><span class="sxs-lookup"><span data-stu-id="2e1de-128">Call a method on a device</span></span>
<span data-ttu-id="2e1de-129">I det här avsnittet skapar du en Node.js-konsolprogram som anropar en metod i hello simulerade enheten appen och sedan visar hello svar.</span><span class="sxs-lookup"><span data-stu-id="2e1de-129">In this section, you create a Node.js console app that calls a method in hello simulated device app and then displays hello response.</span></span>

1. <span data-ttu-id="2e1de-130">Skapa en ny tom mapp som kallas **callmethodondevice**.</span><span class="sxs-lookup"><span data-stu-id="2e1de-130">Create a new empty folder called **callmethodondevice**.</span></span> <span data-ttu-id="2e1de-131">I hello **callmethodondevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="2e1de-131">In hello **callmethodondevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="2e1de-132">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="2e1de-132">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="2e1de-133">Vid en kommandotolk i hello **callmethodondevice** mapp, kör följande kommando tooinstall hello hello **azure iothub** paketet:</span><span class="sxs-lookup"><span data-stu-id="2e1de-133">At your command prompt in hello **callmethodondevice** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="2e1de-134">Med hjälp av en textredigerare, skapa en **CallMethodOnDevice.js** filen i hello **callmethodondevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="2e1de-134">Using a text editor, create a **CallMethodOnDevice.js** file in hello **callmethodondevice** folder.</span></span>
4. <span data-ttu-id="2e1de-135">Lägg till följande hello `require` uttryck högst hello början av hello **CallMethodOnDevice.js** fil:</span><span class="sxs-lookup"><span data-stu-id="2e1de-135">Add hello following `require` statements at hello start of hello **CallMethodOnDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="2e1de-136">Lägg till följande variabeldeklarationen hello och Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för din hubb:</span><span class="sxs-lookup"><span data-stu-id="2e1de-136">Add hello following variable declaration and replace hello placeholder value with hello IoT Hub connection string for your hub:</span></span>
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. <span data-ttu-id="2e1de-137">Skapa hello klienten tooopen hello anslutning tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2e1de-137">Create hello client tooopen hello connection tooyour IoT hub.</span></span>
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. <span data-ttu-id="2e1de-138">Lägg till följande funktion tooinvoke hello metoden och Skriv ut hello enheten svar toohello klientenhetskonsolen hello:</span><span class="sxs-lookup"><span data-stu-id="2e1de-138">Add hello following function tooinvoke hello device method and print hello device response toohello console:</span></span>
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed tooinvoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. <span data-ttu-id="2e1de-139">Spara och Stäng hello **CallMethodOnDevice.js** fil.</span><span class="sxs-lookup"><span data-stu-id="2e1de-139">Save and close hello **CallMethodOnDevice.js** file.</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="2e1de-140">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="2e1de-140">Run hello apps</span></span>
<span data-ttu-id="2e1de-141">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="2e1de-141">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="2e1de-142">Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando toostart lyssnar efter metodanrop från din IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="2e1de-142">At a command prompt in hello **simulateddevice** folder, run hello following command toostart listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. <span data-ttu-id="2e1de-143">Vid en kommandotolk i hello **callmethodondevice** mapp, kör följande kommando toobegin övervaka din IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="2e1de-143">At a command prompt in hello **callmethodondevice** folder, run hello following command toobegin monitoring your IoT hub:</span></span>
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. <span data-ttu-id="2e1de-144">Hello enheten reagera toohello metoden genom att skriva ut hello-meddelande och hello-program som kallas hello metoden visa hello svar från hello enhet visas:</span><span class="sxs-lookup"><span data-stu-id="2e1de-144">You will see hello device react toohello method by printing out hello message and hello application which called hello method display hello response from hello device:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="2e1de-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2e1de-145">Next steps</span></span>
<span data-ttu-id="2e1de-146">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="2e1de-146">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="2e1de-147">Du har använt den här enheten identitet tooenable hello simulerade enheten app tooreact toomethods anropas av hello molnet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-147">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="2e1de-148">Skapade du även en app som anropar metoder på hello enhet och visar hello svar från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="2e1de-148">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span> 

<span data-ttu-id="2e1de-149">toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:</span><span class="sxs-lookup"><span data-stu-id="2e1de-149">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="2e1de-150">[Kom igång med IoT-hubb]</span><span class="sxs-lookup"><span data-stu-id="2e1de-150">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="2e1de-151">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="2e1de-151">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="2e1de-152">toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="2e1de-152">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

<!-- Images. -->
[7]: ./media/iot-hub-node-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-node-node-direct-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-node-node-direct-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Kom igång med IoT-hubb]: iot-hub-node-node-getstarted.md

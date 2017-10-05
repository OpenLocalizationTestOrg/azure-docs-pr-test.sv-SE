---
title: Azure IoT-hubb direkt metoder (nod) | Microsoft Docs
description: "Hur du använder Azure IoT Hub direkt metoder. Du kan använda Azure IoT-SDK för Node.js för att implementera en simulerad enhetsapp som innehåller en direkt metod och en service-appen som anropar metoden direkt."
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
ms.openlocfilehash: 83725c3ae3fd3807f2469be888e270ba078a8972
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a><span data-ttu-id="0ac4d-104">Använd direkt metoderna på din IoT-enhet med Node.js</span><span class="sxs-lookup"><span data-stu-id="0ac4d-104">Use direct methods on your IoT device with Node.js</span></span>
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="0ac4d-105">I slutet av den här kursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-105">At the end of this tutorial, you have two Node.js console apps:</span></span>

* <span data-ttu-id="0ac4d-106">**CallMethodOnDevice.js**, som anropar en metod i appen simulerade enheten och visar svaret.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-106">**CallMethodOnDevice.js**, which calls a method in the simulated device app and displays the response.</span></span>
* <span data-ttu-id="0ac4d-107">**SimulatedDevice.js**, som ansluter till din IoT-hubb med enhetens identitet skapade tidigare och svarar på metoden som anropades av molnet.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-107">**SimulatedDevice.js**, which connects to your IoT hub with the device identity created earlier, and responds to the method called by the cloud.</span></span>

> [!NOTE]
> <span data-ttu-id="0ac4d-108">Artikeln om [Azure IoT SDK:er][lnk-hub-sdks] innehåller information om Azure IoT SDK:er som du kan använda för att skapa båda apparna så att de kan köras på enheter och på lösningens backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both applications to run on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="0ac4d-109">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-109">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="0ac4d-110">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="0ac4d-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-111">An active Azure account.</span></span> <span data-ttu-id="0ac4d-112">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="0ac4d-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="0ac4d-113">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="0ac4d-113">Create a simulated device app</span></span>
<span data-ttu-id="0ac4d-114">I det här avsnittet skapar du en Node.js-konsolprogram som svarar på en metod som anropas av molnet.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-114">In this section, you create a Node.js console app that responds to a method called by the cloud.</span></span>

1. <span data-ttu-id="0ac4d-115">Skapa en ny tom mapp med namnet **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-115">Create a new empty folder called **simulateddevice**.</span></span> <span data-ttu-id="0ac4d-116">I mappen **simulateddevice** skapar du en package.json-fil med hjälp av följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-116">In the **simulateddevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="0ac4d-117">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-117">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="0ac4d-118">I Kommandotolken i mappen **simulateddevice** kör du följande kommando för att installera SDK-paketet **azure-iot-device** och paketet **azure-iot-device-mqtt**:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-118">At your command prompt in the **simulateddevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="0ac4d-119">Med hjälp av en textredigerare skapar du en ny **SimulatedDevice.js**-fil i mappen **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-119">Using a text editor, create a new **SimulatedDevice.js** file in the **simulateddevice** folder.</span></span>
4. <span data-ttu-id="0ac4d-120">Lägg till följande `require`-instruktioner i början av **SimulatedDevice.js**-filen:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-120">Add the following `require` statements at the start of the **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. <span data-ttu-id="0ac4d-121">Lägg till en **connectionString** variabel och använda den för att skapa en **DeviceClient** instans.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-121">Add a **connectionString** variable and use it to create a **DeviceClient** instance.</span></span> <span data-ttu-id="0ac4d-122">Ersätt **{enheten anslutningssträngen}** med enhetsanslutningen sträng som du genererade i den *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-122">Replace **{device connection string}** with the device connection string you generated in the *Create a device identity* section:</span></span>
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. <span data-ttu-id="0ac4d-123">Lägg till följande funktion för att implementera metoden på enheten:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-123">Add the following function to implement the method on the device:</span></span>
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. <span data-ttu-id="0ac4d-124">Öppna anslutning till din IoT-hubb och starta initiera lyssnaren för metoden:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-124">Open the connection to your IoT hub and start initialize the method listener:</span></span>
   
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
8. <span data-ttu-id="0ac4d-125">Spara och stäng filen **SimulatedDevice.js**.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-125">Save and close the **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="0ac4d-126">För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-126">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="0ac4d-127">I produktionskod, bör du implementera försök principer (till exempel anslutning försök), enligt förslaget i MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="0ac4d-127">In production code, you should implement retry policies (such as connection retry), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="call-a-method-on-a-device"></a><span data-ttu-id="0ac4d-128">Anropa en metod på en enhet</span><span class="sxs-lookup"><span data-stu-id="0ac4d-128">Call a method on a device</span></span>
<span data-ttu-id="0ac4d-129">I det här avsnittet skapar du en Node.js-konsolprogram som anropar en metod i appen simulerade enheten och sedan visar svaret.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-129">In this section, you create a Node.js console app that calls a method in the simulated device app and then displays the response.</span></span>

1. <span data-ttu-id="0ac4d-130">Skapa en ny tom mapp som kallas **callmethodondevice**.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-130">Create a new empty folder called **callmethodondevice**.</span></span> <span data-ttu-id="0ac4d-131">I den **callmethodondevice** mapp, skapa en package.json-fil med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-131">In the **callmethodondevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="0ac4d-132">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-132">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="0ac4d-133">Vid en kommandotolk i den **callmethodondevice** mapp, kör följande kommando för att installera den **azure iothub** paketet:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-133">At your command prompt in the **callmethodondevice** folder, run the following command to install the **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="0ac4d-134">Med hjälp av en textredigerare, skapa en **CallMethodOnDevice.js** filen i den **callmethodondevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-134">Using a text editor, create a **CallMethodOnDevice.js** file in the **callmethodondevice** folder.</span></span>
4. <span data-ttu-id="0ac4d-135">Lägg till följande `require` instruktioner i början av den **CallMethodOnDevice.js** fil:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-135">Add the following `require` statements at the start of the **CallMethodOnDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="0ac4d-136">Lägg till följande variabeldeklaration och ersätt platshållarvärdet med IoT Hub-anslutningssträngen för din hubb:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-136">Add the following variable declaration and replace the placeholder value with the IoT Hub connection string for your hub:</span></span>
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. <span data-ttu-id="0ac4d-137">Skapa klient för att öppna anslutningen till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-137">Create the client to open the connection to your IoT hub.</span></span>
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. <span data-ttu-id="0ac4d-138">Lägg till följande funktion att anropa metoden enhet och skriva ut svaret från enheten till konsolen:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-138">Add the following function to invoke the device method and print the device response to the console:</span></span>
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. <span data-ttu-id="0ac4d-139">Spara och Stäng den **CallMethodOnDevice.js** fil.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-139">Save and close the **CallMethodOnDevice.js** file.</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="0ac4d-140">Kör apparna</span><span class="sxs-lookup"><span data-stu-id="0ac4d-140">Run the apps</span></span>
<span data-ttu-id="0ac4d-141">Nu är det dags att köra apparna.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-141">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="0ac4d-142">Vid en kommandotolk i den **simulateddevice** mapp, kör följande kommando för att påbörja avlyssning för metodanrop från din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-142">At a command prompt in the **simulateddevice** folder, run the following command to start listening for method calls from your IoT Hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. <span data-ttu-id="0ac4d-143">Vid en kommandotolk i den **callmethodondevice** mapp, kör följande kommando för att börja övervaka din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-143">At a command prompt in the **callmethodondevice** folder, run the following command to begin monitoring your IoT hub:</span></span>
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. <span data-ttu-id="0ac4d-144">Enheten ta hänsyn till metoden genom att skriva ut meddelandet och programmet anropa metoden visas svaret från enheten visas:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-144">You will see the device react to the method by printing out the message and the application which called the method display the response from the device:</span></span>
   
    ![][9]

## <a name="next-steps"></a><span data-ttu-id="0ac4d-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ac4d-145">Next steps</span></span>
<span data-ttu-id="0ac4d-146">I den här självstudiekursen konfigurerade du en ny IoT Hub på Azure Portal och skapade sedan en enhetsidentitet i IoT-hubbens identitetsregister.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-146">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="0ac4d-147">Denna enhetsidentitet använde för att aktivera appen simulerade enheten att ta hänsyn till metoder som anropas av molnet.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-147">You used this device identity to enable the simulated device app to react to methods invoked by the cloud.</span></span> <span data-ttu-id="0ac4d-148">Du kan också skapat en app som anropar metoder på enheten och visar svaret från enheten.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-148">You also created an app that invokes methods on the device and displays the response from the device.</span></span> 

<span data-ttu-id="0ac4d-149">Mer information om hur du kan komma igång med IoT Hub och utforska andra IoT-scenarier finns här:</span><span class="sxs-lookup"><span data-stu-id="0ac4d-149">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="0ac4d-150">[Kom igång med IoT-hubb]</span><span class="sxs-lookup"><span data-stu-id="0ac4d-150">[Get started with IoT Hub]</span></span>
* <span data-ttu-id="0ac4d-151">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="0ac4d-151">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="0ac4d-152">Information om hur du utökar din IoT-lösningen och schema metodanrop på flera enheter finns i [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="0ac4d-152">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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

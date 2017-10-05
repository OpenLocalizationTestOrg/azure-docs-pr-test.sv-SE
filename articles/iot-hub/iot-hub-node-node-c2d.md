---
title: Moln till enhet meddelanden med Azure IoT Hub (nod) | Microsoft Docs
description: "Hur du skickar meddelanden moln till enhet till en enhet från en Azure IoT-hubb med hjälp av Azure IoT-SDK för Node.js. Du kan ändra en simulerad enhetsapp för att ta emot meddelanden moln till enhet och ändra en backend-app för att skicka meddelanden moln till enhet."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3ca8a78f-ade2-46e8-8a49-d5d599cdf1f1
ms.service: iot-hub
ms.devlang: javascript
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 4580bda5633f84a7c7af0dc85f3cea4951024836
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a><span data-ttu-id="d9352-104">Skicka meddelanden moln till enhet med IoT-hubb (nod)</span><span class="sxs-lookup"><span data-stu-id="d9352-104">Send cloud-to-device messages with IoT Hub (Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="d9352-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="d9352-105">Introduction</span></span>
<span data-ttu-id="d9352-106">Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="d9352-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="d9352-107">Den [Kom igång med IoT-hubb] kursen visar hur du skapar en IoT-hubb, etablera en enhetsidentitet i den och code en simulerad enhetsapp som skickar meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="d9352-107">The [Get started with IoT Hub] tutorial shows how to create an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="d9352-108">Den här kursen bygger på [Kom igång med IoT-hubb].</span><span class="sxs-lookup"><span data-stu-id="d9352-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="d9352-109">Den visar hur till:</span><span class="sxs-lookup"><span data-stu-id="d9352-109">It shows you how to:</span></span>

* <span data-ttu-id="d9352-110">Skicka meddelanden moln till enhet på en enhet till IoT-hubb från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="d9352-110">From your solution back end, send cloud-to-device messages to a single device through IoT Hub.</span></span>
* <span data-ttu-id="d9352-111">Ta emot meddelanden moln till enhet på en enhet.</span><span class="sxs-lookup"><span data-stu-id="d9352-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="d9352-112">Begära leverans bekräftelse från din lösningens serverdel (*feedback*) för meddelanden som skickas till en enhet från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d9352-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent to a device from IoT Hub.</span></span>

<span data-ttu-id="d9352-113">Du hittar mer information om moln till enhet meddelanden i den [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="d9352-113">You can find more information on cloud-to-device messages in the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="d9352-114">I slutet av den här kursen kan köra du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="d9352-114">At the end of this tutorial, you run two Node.js console apps:</span></span>

* <span data-ttu-id="d9352-115">**SimulatedDevice**, en modifierad version av app som skapats i [Kom igång med IoT-hubb], som ansluter till din IoT-hubb och tar emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="d9352-115">**SimulatedDevice**, a modified version of the app created in [Get started with IoT Hub], which connects to your IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="d9352-116">**SendCloudToDeviceMessage**, som skickar ett moln till enhet-meddelande till appen simulerade enheten via IoT-hubb och tar emot dess leverans bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="d9352-116">**SendCloudToDeviceMessage**, which sends a cloud-to-device message to the simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="d9352-117">IoT-hubben har SDK stöd för många enhetsplattformar och språk (inklusive C, Java och Javascript) via SDK för Azure IoT-enhet.</span><span class="sxs-lookup"><span data-stu-id="d9352-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="d9352-118">Stegvisa instruktioner om hur du ansluter enheten till den här självstudiekursen kod och vanligtvis Azure IoT Hub finns i [Azure IoT Developer Center].</span><span class="sxs-lookup"><span data-stu-id="d9352-118">For step-by-step instructions on how to connect your device to this tutorial's code, and generally to Azure IoT Hub, see the [Azure IoT Developer Center].</span></span>
> 
> 

<span data-ttu-id="d9352-119">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d9352-119">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="d9352-120">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="d9352-120">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="d9352-121">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d9352-121">An active Azure account.</span></span> <span data-ttu-id="d9352-122">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="d9352-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-the-simulated-device-app"></a><span data-ttu-id="d9352-123">Ta emot meddelanden i appen simulerade enheten</span><span class="sxs-lookup"><span data-stu-id="d9352-123">Receive messages in the simulated device app</span></span>
<span data-ttu-id="d9352-124">I det här avsnittet kan du ändra den simulerade enhetsapp som du skapade i [Kom igång med IoT-hubb] ta emot meddelanden moln till enhet från IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="d9352-124">In this section, you modify the simulated device app you created in [Get started with IoT Hub] to receive cloud-to-device messages from the IoT hub.</span></span>

1. <span data-ttu-id="d9352-125">Använd en textredigerare och öppna filen SimulatedDevice.js.</span><span class="sxs-lookup"><span data-stu-id="d9352-125">Using a text editor, open the SimulatedDevice.js file.</span></span>
2. <span data-ttu-id="d9352-126">Ändra den **connectCallback** funktion för att hantera meddelanden som skickats från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="d9352-126">Modify the **connectCallback** function to handle messages sent from IoT Hub.</span></span> <span data-ttu-id="d9352-127">I det här exemplet enheten alltid anropar den **fullständig** funktion för att meddela IoT-hubb att meddelandet har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="d9352-127">In this example, the device always invokes the **complete** function to notify IoT Hub that it has processed the message.</span></span> <span data-ttu-id="d9352-128">Den nya versionen av den **connectCallback** funktionen ser ut som följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="d9352-128">Your new version of the **connectCallback** function looks like the following snippet:</span></span>
   
    ```javascript
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var temperature = 20 + (Math.random() * 15);
            var humidity = 60 + (Math.random() * 20);            
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
            var message = new Message(data);
            message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
   > [!NOTE]
   > <span data-ttu-id="d9352-129">Om du använder HTTP i stället för MQTT eller AMQP som transport, den **DeviceClient** instans söker efter meddelanden från IoT-hubb sällan (mindre än var 25: e minut).</span><span class="sxs-lookup"><span data-stu-id="d9352-129">If you use HTTP instead of MQTT or AMQP as the transport, the **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="d9352-130">Mer information om skillnaderna mellan MQTT, AMQP och HTTP-stöd och IoT-hubb begränsning finns på [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="d9352-130">For more information about the differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="d9352-131">Skicka ett meddelande moln till enhet</span><span class="sxs-lookup"><span data-stu-id="d9352-131">Send a cloud-to-device message</span></span>
<span data-ttu-id="d9352-132">I det här avsnittet skapar du en Node.js-konsolapp som skickar moln till enhet meddelanden i appen simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="d9352-132">In this section, you create a Node.js console app that sends cloud-to-device messages to the simulated device app.</span></span> <span data-ttu-id="d9352-133">Du behöver enhets-ID på den enhet som du lade till i den [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="d9352-133">You need the device ID of the device you added in the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="d9352-134">Du måste också anslutningssträngen IoT-hubb för din hubb hittar du i den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="d9352-134">You also need the IoT Hub connection string for your hub that you can find in the [Azure portal].</span></span>

1. <span data-ttu-id="d9352-135">Skapa en tom mapp som kallas **sendcloudtodevicemessage**.</span><span class="sxs-lookup"><span data-stu-id="d9352-135">Create an empty folder called **sendcloudtodevicemessage**.</span></span> <span data-ttu-id="d9352-136">I den **sendcloudtodevicemessage** mapp, skapa en package.json-fil med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="d9352-136">In the **sendcloudtodevicemessage** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="d9352-137">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="d9352-137">Accept all the defaults:</span></span>
   
    ```shell
    npm init
    ```
2. <span data-ttu-id="d9352-138">Vid en kommandotolk i den **sendcloudtodevicemessage** mapp, kör följande kommando för att installera den **azure iothub** paketet:</span><span class="sxs-lookup"><span data-stu-id="d9352-138">At your command prompt in the **sendcloudtodevicemessage** folder, run the following command to install the **azure-iothub** package:</span></span>
   
    ```shell
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="d9352-139">Med hjälp av en textredigerare, skapa en **SendCloudToDeviceMessage.js** filen i den **sendcloudtodevicemessage** mapp.</span><span class="sxs-lookup"><span data-stu-id="d9352-139">Using a text editor, create a **SendCloudToDeviceMessage.js** file in the **sendcloudtodevicemessage** folder.</span></span>
4. <span data-ttu-id="d9352-140">Lägg till följande `require` instruktioner i början av den **SendCloudToDeviceMessage.js** fil:</span><span class="sxs-lookup"><span data-stu-id="d9352-140">Add the following `require` statements at the start of the **SendCloudToDeviceMessage.js** file:</span></span>
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. <span data-ttu-id="d9352-141">Lägg till följande kod i **SendCloudToDeviceMessage.js** fil.</span><span class="sxs-lookup"><span data-stu-id="d9352-141">Add the following code to **SendCloudToDeviceMessage.js** file.</span></span> <span data-ttu-id="d9352-142">Ersätt värdet för ”{iot-hubb anslutningssträngen}” platshållaren med IoT-hubb anslutningssträngen för hubben som du skapade i den [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="d9352-142">Replace the "{iot hub connection string}" placeholder value with the IoT Hub connection string for the hub you created in the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="d9352-143">Ersätt platshållaren ”{enhets-id}” med enhets-ID på den enhet som du lade till i den [Kom igång med IoT-hubb] kursen:</span><span class="sxs-lookup"><span data-stu-id="d9352-143">Replace the "{device id}" placeholder with the device ID of the device you added in the [Get started with IoT Hub] tutorial:</span></span>
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="d9352-144">Lägg till följande funktion om du vill skriva ut åtgärden resulterar i konsolen:</span><span class="sxs-lookup"><span data-stu-id="d9352-144">Add the following function to print operation results to the console:</span></span>
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. <span data-ttu-id="d9352-145">Lägg till följande funktion om du vill skriva ut leverans feedback meddelanden till konsolen:</span><span class="sxs-lookup"><span data-stu-id="d9352-145">Add the following function to print delivery feedback messages to the console:</span></span>
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. <span data-ttu-id="d9352-146">Lägg till följande kod för att skicka ett meddelande till din enhet och hantera meddelandet feedback när enheten har bekräftat meddelandet moln till enhet:</span><span class="sxs-lookup"><span data-stu-id="d9352-146">Add the following code to send a message to your device and handle the feedback message when the device acknowledges the cloud-to-device message:</span></span>
   
    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```
9. <span data-ttu-id="d9352-147">Spara och Stäng **SendCloudToDeviceMessage.js** fil.</span><span class="sxs-lookup"><span data-stu-id="d9352-147">Save and close **SendCloudToDeviceMessage.js** file.</span></span>

## <a name="run-the-applications"></a><span data-ttu-id="d9352-148">Köra programmen</span><span class="sxs-lookup"><span data-stu-id="d9352-148">Run the applications</span></span>
<span data-ttu-id="d9352-149">Nu är det dags att köra programmen.</span><span class="sxs-lookup"><span data-stu-id="d9352-149">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="d9352-150">I Kommandotolken i den **simulateddevice** mapp, kör följande kommando för att skicka telemetri till IoT-hubb och lyssnar efter meddelanden moln till enhet:</span><span class="sxs-lookup"><span data-stu-id="d9352-150">At the command prompt in the **simulateddevice** folder, run the following command to send telemetry to IoT Hub and to listen for cloud-to-device messages:</span></span>
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![Kör appen simulerade enheten][img-simulated-device]
2. <span data-ttu-id="d9352-152">Vid en kommandotolk i den **sendcloudtodevicemessage** mapp, kör följande kommando för att skicka ett meddelande moln till enhet och vänta tills bekräftelse feedback:</span><span class="sxs-lookup"><span data-stu-id="d9352-152">At a command prompt in the **sendcloudtodevicemessage** folder, run the following command to send a cloud-to-device message and wait for the acknowledgment feedback:</span></span>
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Kör appen för att skicka kommandot moln till enhet][img-send-command]
   
   > [!NOTE]
   > <span data-ttu-id="d9352-154">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="d9352-154">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="d9352-155">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="d9352-155">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="d9352-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d9352-156">Next steps</span></span>
<span data-ttu-id="d9352-157">I den här självstudiekursen beskrivs hur du skickar och tar emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="d9352-157">In this tutorial, you learned how to send and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="d9352-158">Exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="d9352-158">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="d9352-159">Mer information om hur du utvecklar lösningar med IoT-hubb finns i [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="d9352-159">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

<span data-ttu-id="d9352-160">[Kom igång med IoT-hubb]: iot-hub-node-node-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="d9352-160">[Get started with IoT Hub]: iot-hub-node-node-getstarted.md</span></span>
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
<span data-ttu-id="d9352-161">[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="d9352-161">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
<span data-ttu-id="d9352-162">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="d9352-162">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span></span>
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
<span data-ttu-id="d9352-163">[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="d9352-163">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
<span data-ttu-id="d9352-164">[Azure-portalen]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="d9352-164">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="d9352-165">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span><span class="sxs-lookup"><span data-stu-id="d9352-165">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span></span>

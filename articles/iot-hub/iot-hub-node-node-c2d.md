---
title: aaaCloud till enhet meddelanden med Azure IoT Hub (nod) | Microsoft Docs
description: "Hur meddelanden toosend moln till enhet tooa enhet från en Azure IoT-hubb med hello Azure IoT SDK för Node.js. Du ändrar en simulerad enhet tooreceive moln till enhet meddelanden och ändra en backend-app toosend moln till enhet hälsningsmeddelande."
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
ms.openlocfilehash: 1ccae0cada52193c2abb91504c086cac226e93da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a><span data-ttu-id="b1c5f-104">Skicka meddelanden moln till enhet med IoT-hubb (nod)</span><span class="sxs-lookup"><span data-stu-id="b1c5f-104">Send cloud-to-device messages with IoT Hub (Node)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="b1c5f-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="b1c5f-105">Introduction</span></span>
<span data-ttu-id="b1c5f-106">Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="b1c5f-107">Hej [Kom igång med IoT-hubb] kursen visar hur toocreate en IoT-hubb etablera en enhetsidentitet i den och code en simulerad enhetsapp som skickar meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-107">hello [Get started with IoT Hub] tutorial shows how toocreate an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="b1c5f-108">Den här kursen bygger på [Kom igång med IoT-hubb].</span><span class="sxs-lookup"><span data-stu-id="b1c5f-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="b1c5f-109">Den visar hur till:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-109">It shows you how to:</span></span>

* <span data-ttu-id="b1c5f-110">Skicka meddelanden moln till enhet tooa enhet via IoT-hubb från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-110">From your solution back end, send cloud-to-device messages tooa single device through IoT Hub.</span></span>
* <span data-ttu-id="b1c5f-111">Ta emot meddelanden moln till enhet på en enhet.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="b1c5f-112">Begära leverans bekräftelse från din lösningens serverdel (*feedback*) för meddelanden som skickas tooa enheten från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent tooa device from IoT Hub.</span></span>

<span data-ttu-id="b1c5f-113">Du hittar mer information om moln till enhet meddelanden i hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="b1c5f-113">You can find more information on cloud-to-device messages in hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="b1c5f-114">Hello slutet av den här självstudiekursen kör du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-114">At hello end of this tutorial, you run two Node.js console apps:</span></span>

* <span data-ttu-id="b1c5f-115">**SimulatedDevice**, en modifierad version av hello-app som skapats i [Kom igång med IoT-hubb], som ansluter tooyour IoT-hubb och tar emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-115">**SimulatedDevice**, a modified version of hello app created in [Get started with IoT Hub], which connects tooyour IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="b1c5f-116">**SendCloudToDeviceMessage**, som skickar ett moln-till-enhetsmeddelande toohello simulerade enheten appen via IoT-hubb och tar emot dess leverans bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-116">**SendCloudToDeviceMessage**, which sends a cloud-to-device message toohello simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="b1c5f-117">IoT-hubben har SDK stöd för många enhetsplattformar och språk (inklusive C, Java och Javascript) via SDK för Azure IoT-enhet.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="b1c5f-118">Stegvisa instruktioner om hur tooconnect enhet toothis kursens kod och vanligtvis tooAzure IoT Hub, se hello [Azure IoT Developer Center].</span><span class="sxs-lookup"><span data-stu-id="b1c5f-118">For step-by-step instructions on how tooconnect your device toothis tutorial's code, and generally tooAzure IoT Hub, see hello [Azure IoT Developer Center].</span></span>
> 
> 

<span data-ttu-id="b1c5f-119">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-119">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="b1c5f-120">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-120">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="b1c5f-121">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-121">An active Azure account.</span></span> <span data-ttu-id="b1c5f-122">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="b1c5f-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-hello-simulated-device-app"></a><span data-ttu-id="b1c5f-123">Ta emot meddelanden i hello simulerade enhetsapp</span><span class="sxs-lookup"><span data-stu-id="b1c5f-123">Receive messages in hello simulated device app</span></span>
<span data-ttu-id="b1c5f-124">I det här avsnittet kan du ändra hello simulerade enhetsapp som du skapade i [Kom igång med IoT-hubb] tooreceive moln till enhet meddelanden från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-124">In this section, you modify hello simulated device app you created in [Get started with IoT Hub] tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="b1c5f-125">Använd en textredigerare och öppna hello SimulatedDevice.js filen.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-125">Using a text editor, open hello SimulatedDevice.js file.</span></span>
2. <span data-ttu-id="b1c5f-126">Ändra hello **connectCallback** fungerar toohandle meddelanden som skickats från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-126">Modify hello **connectCallback** function toohandle messages sent from IoT Hub.</span></span> <span data-ttu-id="b1c5f-127">I det här exemplet anropar hello enhet alltid hello **fullständig** fungerar toonotify IoT-hubb som har behandlats hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-127">In this example, hello device always invokes hello **complete** function toonotify IoT Hub that it has processed hello message.</span></span> <span data-ttu-id="b1c5f-128">Den nya versionen av hello **connectCallback** funktionen ser ut som följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-128">Your new version of hello **connectCallback** function looks like hello following snippet:</span></span>
   
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
        // Create a message and send it toohello IoT Hub every second
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
   > <span data-ttu-id="b1c5f-129">Om du använder HTTP i stället för MQTT eller AMQP som hello transport hello **DeviceClient** instans söker efter meddelanden från IoT-hubb sällan (mindre än var 25: e minut).</span><span class="sxs-lookup"><span data-stu-id="b1c5f-129">If you use HTTP instead of MQTT or AMQP as hello transport, hello **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="b1c5f-130">Mer information om hello skillnader mellan MQTT, AMQP och HTTP-stöd och IoT-hubb begränsning finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="b1c5f-130">For more information about hello differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="b1c5f-131">Skicka ett meddelande moln till enhet</span><span class="sxs-lookup"><span data-stu-id="b1c5f-131">Send a cloud-to-device message</span></span>
<span data-ttu-id="b1c5f-132">I det här avsnittet skapar du en Node.js-konsolapp som skickar meddelanden moln till enhet toohello simulerade enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-132">In this section, you create a Node.js console app that sends cloud-to-device messages toohello simulated device app.</span></span> <span data-ttu-id="b1c5f-133">Du behöver hello enhets-ID för hello-enhet som du lade till i hello [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-133">You need hello device ID of hello device you added in hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="b1c5f-134">Du måste också hello anslutningssträngen för IoT-hubb för din hubb hittar du i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="b1c5f-134">You also need hello IoT Hub connection string for your hub that you can find in hello [Azure portal].</span></span>

1. <span data-ttu-id="b1c5f-135">Skapa en tom mapp som kallas **sendcloudtodevicemessage**.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-135">Create an empty folder called **sendcloudtodevicemessage**.</span></span> <span data-ttu-id="b1c5f-136">I hello **sendcloudtodevicemessage** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-136">In hello **sendcloudtodevicemessage** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="b1c5f-137">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-137">Accept all hello defaults:</span></span>
   
    ```shell
    npm init
    ```
2. <span data-ttu-id="b1c5f-138">Vid en kommandotolk i hello **sendcloudtodevicemessage** mapp, kör följande kommando tooinstall hello hello **azure iothub** paketet:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-138">At your command prompt in hello **sendcloudtodevicemessage** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```shell
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="b1c5f-139">Med hjälp av en textredigerare, skapa en **SendCloudToDeviceMessage.js** filen i hello **sendcloudtodevicemessage** mapp.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-139">Using a text editor, create a **SendCloudToDeviceMessage.js** file in hello **sendcloudtodevicemessage** folder.</span></span>
4. <span data-ttu-id="b1c5f-140">Lägg till följande hello `require` uttryck högst hello början av hello **SendCloudToDeviceMessage.js** fil:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-140">Add hello following `require` statements at hello start of hello **SendCloudToDeviceMessage.js** file:</span></span>
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. <span data-ttu-id="b1c5f-141">Lägg till följande kod för hello**SendCloudToDeviceMessage.js** fil.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-141">Add hello following code too**SendCloudToDeviceMessage.js** file.</span></span> <span data-ttu-id="b1c5f-142">Ersätt hello ”{iot-hubb anslutningssträngen}” platshållarvärde med hello IoT-hubb anslutningssträngen för hello hub du skapat i hello [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-142">Replace hello "{iot hub connection string}" placeholder value with hello IoT Hub connection string for hello hub you created in hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="b1c5f-143">Ersätt platshållaren hello ”{enhets-id}” med hello enhets-ID för hello-enhet som du lade till i hello [Kom igång med IoT-hubb] kursen:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-143">Replace hello "{device id}" placeholder with hello device ID of hello device you added in hello [Get started with IoT Hub] tutorial:</span></span>
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="b1c5f-144">Lägg till hello följande funktion tooprint åtgärden resultat toohello konsolen:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-144">Add hello following function tooprint operation results toohello console:</span></span>
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. <span data-ttu-id="b1c5f-145">Lägg till följande funktion tooprint leverans feedback meddelanden toohello konsolen hello:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-145">Add hello following function tooprint delivery feedback messages toohello console:</span></span>
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. <span data-ttu-id="b1c5f-146">Lägg till följande hello code toosend en meddelandet tooyour enhet och hantera feedback hälsningsmeddelande när hello-enhet bekräftar hälsningsmeddelande moln till enhet:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-146">Add hello following code toosend a message tooyour device and handle hello feedback message when hello device acknowledges hello cloud-to-device message:</span></span>
   
    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud toodevice message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```
9. <span data-ttu-id="b1c5f-147">Spara och Stäng **SendCloudToDeviceMessage.js** fil.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-147">Save and close **SendCloudToDeviceMessage.js** file.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="b1c5f-148">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="b1c5f-148">Run hello applications</span></span>
<span data-ttu-id="b1c5f-149">Du är nu redo toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-149">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="b1c5f-150">Kommandotolken hello i hello **simulateddevice** mapp, kör följande hello kommandot toosend telemetri tooIoT hubb och toolisten för meddelanden moln till enhet:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-150">At hello command prompt in hello **simulateddevice** folder, run hello following command toosend telemetry tooIoT Hub and toolisten for cloud-to-device messages:</span></span>
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![Kör hello simulerade enhetsapp][img-simulated-device]
2. <span data-ttu-id="b1c5f-152">Vid en kommandotolk i hello **sendcloudtodevicemessage** -mappen och kör följande kommando toosend hello meddelandet moln till enhet och vänta tills hello bekräftelse feedback:</span><span class="sxs-lookup"><span data-stu-id="b1c5f-152">At a command prompt in hello **sendcloudtodevicemessage** folder, run hello following command toosend a cloud-to-device message and wait for hello acknowledgment feedback:</span></span>
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Kör hello app toosend hello moln till enhet kommando][img-send-command]
   
   > [!NOTE]
   > <span data-ttu-id="b1c5f-154">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-154">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="b1c5f-155">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="b1c5f-155">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="b1c5f-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b1c5f-156">Next steps</span></span>
<span data-ttu-id="b1c5f-157">I kursen får du lära sig hur toosend och ta emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="b1c5f-157">In this tutorial, you learned how toosend and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="b1c5f-158">toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="b1c5f-158">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="b1c5f-159">toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="b1c5f-159">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Kom igång med IoT-hubb]: iot-hub-node-node-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portalen]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

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
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a>Skicka meddelanden moln till enhet med IoT-hubb (nod)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Introduktion
Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas. Hej [Kom igång med IoT-hubb] kursen visar hur toocreate en IoT-hubb etablera en enhetsidentitet i den och code en simulerad enhetsapp som skickar meddelanden från enhet till moln.

Den här kursen bygger på [Kom igång med IoT-hubb]. Den visar hur till:

* Skicka meddelanden moln till enhet tooa enhet via IoT-hubb från din lösningens serverdel.
* Ta emot meddelanden moln till enhet på en enhet.
* Begära leverans bekräftelse från din lösningens serverdel (*feedback*) för meddelanden som skickas tooa enheten från IoT-hubb.

Du hittar mer information om moln till enhet meddelanden i hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].

Hello slutet av den här självstudiekursen kör du två Node.js-konsolappar:

* **SimulatedDevice**, en modifierad version av hello-app som skapats i [Kom igång med IoT-hubb], som ansluter tooyour IoT-hubb och tar emot meddelanden moln till enhet.
* **SendCloudToDeviceMessage**, som skickar ett moln-till-enhetsmeddelande toohello simulerade enheten appen via IoT-hubb och tar emot dess leverans bekräftelse.

> [!NOTE]
> IoT-hubben har SDK stöd för många enhetsplattformar och språk (inklusive C, Java och Javascript) via SDK för Azure IoT-enhet. Stegvisa instruktioner om hur tooconnect enhet toothis kursens kod och vanligtvis tooAzure IoT Hub, se hello [Azure IoT Developer Center].
> 
> 

toocomplete den här kursen behöver du hello följande:

* Node.js version 0.10.x eller senare.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

## <a name="receive-messages-in-hello-simulated-device-app"></a>Ta emot meddelanden i hello simulerade enhetsapp
I det här avsnittet kan du ändra hello simulerade enhetsapp som du skapade i [Kom igång med IoT-hubb] tooreceive moln till enhet meddelanden från hello IoT-hubb.

1. Använd en textredigerare och öppna hello SimulatedDevice.js filen.
2. Ändra hello **connectCallback** fungerar toohandle meddelanden som skickats från IoT-hubb. I det här exemplet anropar hello enhet alltid hello **fullständig** fungerar toonotify IoT-hubb som har behandlats hello-meddelande. Den nya versionen av hello **connectCallback** funktionen ser ut som följande fragment hello:
   
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
   > Om du använder HTTP i stället för MQTT eller AMQP som hello transport hello **DeviceClient** instans söker efter meddelanden från IoT-hubb sällan (mindre än var 25: e minut). Mer information om hello skillnader mellan MQTT, AMQP och HTTP-stöd och IoT-hubb begränsning finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a>Skicka ett meddelande moln till enhet
I det här avsnittet skapar du en Node.js-konsolapp som skickar meddelanden moln till enhet toohello simulerade enhetsapp. Du behöver hello enhets-ID för hello-enhet som du lade till i hello [Kom igång med IoT-hubb] kursen. Du måste också hello anslutningssträngen för IoT-hubb för din hubb hittar du i hello [Azure-portalen].

1. Skapa en tom mapp som kallas **sendcloudtodevicemessage**. I hello **sendcloudtodevicemessage** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```shell
    npm init
    ```
2. Vid en kommandotolk i hello **sendcloudtodevicemessage** mapp, kör följande kommando tooinstall hello hello **azure iothub** paketet:
   
    ```shell
    npm install azure-iothub --save
    ```
3. Med hjälp av en textredigerare, skapa en **SendCloudToDeviceMessage.js** filen i hello **sendcloudtodevicemessage** mapp.
4. Lägg till följande hello `require` uttryck högst hello början av hello **SendCloudToDeviceMessage.js** fil:
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. Lägg till följande kod för hello**SendCloudToDeviceMessage.js** fil. Ersätt hello ”{iot-hubb anslutningssträngen}” platshållarvärde med hello IoT-hubb anslutningssträngen för hello hub du skapat i hello [Kom igång med IoT-hubb] kursen. Ersätt platshållaren hello ”{enhets-id}” med hello enhets-ID för hello-enhet som du lade till i hello [Kom igång med IoT-hubb] kursen:
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. Lägg till hello följande funktion tooprint åtgärden resultat toohello konsolen:
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Lägg till följande funktion tooprint leverans feedback meddelanden toohello konsolen hello:
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. Lägg till följande hello code toosend en meddelandet tooyour enhet och hantera feedback hälsningsmeddelande när hello-enhet bekräftar hälsningsmeddelande moln till enhet:
   
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
9. Spara och Stäng **SendCloudToDeviceMessage.js** fil.

## <a name="run-hello-applications"></a>Köra hello program
Du är nu redo toorun hello program.

1. Kommandotolken hello i hello **simulateddevice** mapp, kör följande hello kommandot toosend telemetri tooIoT hubb och toolisten för meddelanden moln till enhet:
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![Kör hello simulerade enhetsapp][img-simulated-device]
2. Vid en kommandotolk i hello **sendcloudtodevicemessage** -mappen och kör följande kommando toosend hello meddelandet moln till enhet och vänta tills hello bekräftelse feedback:
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Kör hello app toosend hello moln till enhet kommando][img-send-command]
   
   > [!NOTE]
   > Den här självstudiekursen implementerar inte några återförsöksprincip sätt. I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].
   > 
   > 

## <a name="next-steps"></a>Nästa steg
I kursen får du lära sig hur toosend och ta emot meddelanden moln till enhet. 

toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite].

toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].

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

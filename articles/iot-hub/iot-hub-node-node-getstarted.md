---
title: "aaaGet igång med Azure IoT Hub (nod) | Microsoft Docs"
description: "Lär dig hur toosend enhet till moln meddelanden tooAzure IoT-hubb med IoT-SDK för Node.js. Skapa simulerade enheten och tjänsten appar tooregister enheten, skicka meddelanden och läsa meddelanden från IoT-hubb."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 56618522-9a31-42c6-94bf-55e2233b39ac
ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0747895365f2359a9c38ea1e85a5881d6efec0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-node"></a>Ansluta din simulerade enhet tooyour IoT-hubb med hjälp av noden
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Hello slutet av den här självstudiekursen har du tre Node.js konsolappar:

* **CreateDeviceIdentity.js**, vilket skapar en enhetsidentitet och tillhörande nyckeln tooconnect appen simulerade enheten.
* **ReadDeviceToCloudMessages.js**, som visar hello telemetri som skickats av din simulerade enhetsapp.
* **SimulatedDevice.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och skickar meddelandet telemetri som alla andra med hello MQTT-protokollet.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.
> 
> 

toocomplete den här kursen behöver du hello följande:

* Node.js version 0.10.x eller senare.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nu har du skapat din IoT Hub. Du har hello IoT Hub-värdnamnet och hello IoT-hubb anslutningssträngen som du behöver toocomplete hello resten av den här kursen.

## <a name="create-a-device-identity"></a>Skapa en enhetsidentitet
I det här avsnittet skapar du en Node.js-konsolprogram som skapar en enhetsidentitet i hello identitetsregistret i din IoT-hubb. En enhet kan bara ansluta tooIoT hubb om det finns en post i registret för hello identitet. Mer information finns i hello **Identitetsregistret** avsnitt i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity]. När du kör den här konsolen appen genereras ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den skickar enhet till moln meddelanden tooIoT hubb.

1. Skapa en ny tom mapp med namnet **createdeviceidentity**. I hello **createdeviceidentity** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **createdeviceidentity** mapp, kör följande kommando tooinstall hello hello **azure iothub** Service SDK-paketet:
   
    ```
    npm install azure-iothub --save
    ```
3. Med hjälp av en textredigerare, skapa en **CreateDeviceIdentity.js** filen i hello **createdeviceidentity** mapp.
4. Lägg till följande hello `require` instruktionen hello början av hello **CreateDeviceIdentity.js** fil:
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. Lägg till följande kod toohello hello **CreateDeviceIdentity.js** filen och Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello: 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. Lägg till följande kod toocreate en enhet definition i hello identitetsregistret i din IoT-hubb hello. Den här koden skapar en enhet om hello enhets-ID inte finns i hello identitetsregistret, annars returneras hello nyckeln för hello befintlig enhet:
   
    ```
    var device = {
      deviceId: 'myFirstNodeDevice'
    }
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });
   
    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device ID: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.symmetricKey.primaryKey);
      }
    }
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

7. Spara och stäng filen **CreateDeviceIdentity.js**.
8. toorun hello **createdeviceidentity** program, köra följande kommando i Kommandotolken hello i hello createdeviceidentity mapp hello:
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. Anteckna hello **enhets-ID** och **enhetsnyckel**. Du måste dessa värden senare när du skapar ett program som ansluter tooIoT hubb som en enhet.

> [!NOTE]
> Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb. Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet. Om ditt program måste toostore andra enhetsspecifika metadata, använder den en programspecifika butik. Mer information finns i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].
> 
> 

<a id="D2C_node"></a>
## <a name="receive-device-to-cloud-messages"></a>Ta emot meddelanden från enheten till molnet
I det här avsnittet skapar du en Node.js-konsolapp som läser ”enhet till molnet”-meddelanden från IoT Hub. En IoT-hubb Exponerar en [Händelsehubbar][lnk-event-hubs-overview]-kompatibel endpoint tooenable du tooread meddelanden från enhet till moln. enkel tookeep saker, den här guiden skapar en grundläggande läsare som inte lämpar sig för en distribution med hög genomströmning. Hej [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen visar hur tooprocess enhet till moln meddelanden i större skala. Hej [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] självstudier innehåller ytterligare information om hur tooprocess meddelanden från Event Hubs och är tillämplig toohello IoT-hubb Event Hub-kompatibel slutpunkter.

> [!NOTE]
> hello använder Event Hub-kompatibel slutpunkt för att läsa meddelanden från enhet till moln alltid hello AMQP-protokollet.
> 
> 

1. Skapa en tom mapp med namnet **readdevicetocloudmessages**. I hello **readdevicetocloudmessages** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **readdevicetocloudmessages** mapp, kör följande kommando tooinstall hello hello **azure händelsehubbar** paketet:
   
    ```
    npm install azure-event-hubs --save
    ```
3. Med hjälp av en textredigerare, skapa en **ReadDeviceToCloudMessages.js** filen i hello **readdevicetocloudmessages** mapp.
4. Lägg till följande hello `require` uttryck högst hello början av hello **ReadDeviceToCloudMessages.js** fil:
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. Lägg till följande variabeldeklarationen hello och Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för din hubb:
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. Lägg till följande två funktioner som skriver ut utdata toohello konsolen hello:
   
    ```
    var printError = function (err) {
      console.log(err.message);
    };
   
    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```
7. Lägg till följande kod toocreate hello hello **EventHubClient**, öppna hello anslutning tooyour IoT-hubb och skapa en mottagare för varje partition. Det här programmet använder ett filter när den skapar en mottagare så att hello mottagaren läser endast meddelanden som skickas tooIoT hubb när hello mottagaren börjar köras. Det här filtret är användbart i en testmiljö så att du ser bara hello aktuella uppsättning av meddelanden. Koden bör se till att den bearbetar alla hälsningsmeddelande i en produktionsmiljö. Mer information finns i hello [hur tooprocess IoT-hubb meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen:
   
    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```
8. Spara och Stäng hello **ReadDeviceToCloudMessages.js** fil.

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
I det här avsnittet skapar du en Node.js-konsolprogram som simulerar en enhet som skickar meddelanden från enhet till moln tooan IoT-hubb.

1. Skapa en tom mapp med namnet **simulateddevice**. I hello **simulateddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Med hjälp av en textredigerare, skapa en **SimulatedDevice.js** filen i hello **simulateddevice** mapp.
4. Lägg till följande hello `require` uttryck högst hello början av hello **SimulatedDevice.js** fil:
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans. Ersätt **{youriothostname}** med hello namnet hello IoT-hubb som du skapade hello *skapar en IoT-hubb* avsnitt. Ersätt **{yourdevicekey}** hello enheten nyckelvärdet du genererade i hello *skapa en enhetsidentitet* avsnitt:
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. Lägg till följande funktion toodisplay utdata från programmet hello hello:
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Skapa ett återanrop och använda hello **setInterval** fungerar toosend meddelandet tooyour IoT-hubb varje sekund:
   
    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
   
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
8. Öppna hello anslutning tooyour IoT-hubb och börja skicka meddelanden:
   
    ```
    client.open(connectCallback);
    ```
9. Spara och Stäng hello **SimulatedDevice.js** fil.

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].
> 
> 

## <a name="run-hello-apps"></a>Köra hello appar
Du är nu redo toorun hello appar.

1. Vid en kommandotolk i hello **readdevicetocloudmessages** mapp, kör följande kommando toobegin övervaka din IoT-hubb hello:
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![Node.js IoT-hubb app toomonitor enhet till moln meddelanden][7]
2. Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando toobegin skicka telemetri data tooyour IoT-hubb hello:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![Node.js IoT-hubb enheten toosend enhet till moln meddelanden][8]
3. Hej **användning** panelen i hello [Azure-portalen] [ lnk-portal] visar hello antalet meddelanden som skickas toohello IoT-hubb:
   
    ![Azure portal användning panelen visar antalet meddelanden som skickas tooIoT Hub][43]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du har använt den här enhetens identitet tooenable hello simulerade enheten app toosend meddelanden från enhet till moln toohello IoT-hubb. Du kan också skapat en app som visar hello som tagits emot av hello IoT-hubb. 

toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:

* [Connecting your device][lnk-connect-device] (Ansluta din enhet)
* [Connecting your device][lnk-device-management] (Komma igång med enhetshantering)
* [Komma igång med Azure IoT Edge][lnk-iot-edge]

toolearn hur tooextend IoT-lösningen och processen enhet till moln meddelandena i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]


<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

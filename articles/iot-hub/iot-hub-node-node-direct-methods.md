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
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a>Använd direkt metoderna på din IoT-enhet med Node.js
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:

* **CallMethodOnDevice.js**, som anropar en metod i hello simulerade enhetsapp och visar hello svar.
* **SimulatedDevice.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och svarar toohello-metoden anropas av hello molnet.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.
> 
> 

toocomplete den här kursen behöver du hello följande:

* Node.js version 0.10.x eller senare.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
I det här avsnittet skapar du en Node.js-konsolprogram som svarar tooa-metoden anropas av hello molnet.

1. Skapa en ny tom mapp med namnet **simulateddevice**. I hello **simulateddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Med hjälp av en textredigerare, skapa en ny **SimulatedDevice.js** filen i hello **simulateddevice** mapp.
4. Lägg till följande hello `require` uttryck högst hello början av hello **SimulatedDevice.js** fil:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Lägg till en **connectionString** variabel och använda den toocreate en **DeviceClient** instans. Ersätt **{enheten anslutningssträngen}** med hello enheten anslutningssträng som du genererade i hello *skapa en enhetsidentitet* avsnitt:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Lägg till hello följande funktion tooimplement hello metod på hello enhet:
   
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
7. Öppna hello anslutning tooyour IoT-hubb och börja initiera hello metoden lyssnare:
   
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
8. Spara och Stäng hello **SimulatedDevice.js** fil.

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel anslutning försök), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].
> 
> 

## <a name="call-a-method-on-a-device"></a>Anropa en metod på en enhet
I det här avsnittet skapar du en Node.js-konsolprogram som anropar en metod i hello simulerade enheten appen och sedan visar hello svar.

1. Skapa en ny tom mapp som kallas **callmethodondevice**. I hello **callmethodondevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **callmethodondevice** mapp, kör följande kommando tooinstall hello hello **azure iothub** paketet:
   
    ```
    npm install azure-iothub --save
    ```
3. Med hjälp av en textredigerare, skapa en **CallMethodOnDevice.js** filen i hello **callmethodondevice** mapp.
4. Lägg till följande hello `require` uttryck högst hello början av hello **CallMethodOnDevice.js** fil:
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. Lägg till följande variabeldeklarationen hello och Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för din hubb:
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. Skapa hello klienten tooopen hello anslutning tooyour IoT-hubb.
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. Lägg till följande funktion tooinvoke hello metoden och Skriv ut hello enheten svar toohello klientenhetskonsolen hello:
   
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
8. Spara och Stäng hello **CallMethodOnDevice.js** fil.

## <a name="run-hello-apps"></a>Köra hello appar
Du är nu redo toorun hello appar.

1. Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando toostart lyssnar efter metodanrop från din IoT-hubb hello:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. Vid en kommandotolk i hello **callmethodondevice** mapp, kör följande kommando toobegin övervaka din IoT-hubb hello:
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. Hello enheten reagera toohello metoden genom att skriva ut hello-meddelande och hello-program som kallas hello metoden visa hello svar från hello enhet visas:
   
    ![][9]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du har använt den här enheten identitet tooenable hello simulerade enheten app tooreact toomethods anropas av hello molnet. Skapade du även en app som anropar metoder på hello enhet och visar hello svar från hello enhet. 

toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:

* [Kom igång med IoT-hubb]
* [Schema-jobb på flera enheter][lnk-devguide-jobs]

toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.

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

---
title: aaaSchedule jobb med Azure IoT Hub (nod) | Microsoft Docs
description: "Hur jobbet tooschedule en Azure IoT-hubb tooinvoke direkt metod på flera enheter. Du kan använda hello Azure IoT SDK för Node.js tooimplement hello simulerade enhetsappar och ett app toorun hello jobb."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: be293362447fbcddaa3433b66f208f22545fe0c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a>Schemat och sändning jobb (nod)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub är en helt hanterad tjänst som gör en backend-app toocreate och spåra jobb som schemalägger och uppdatera miljoner enheter.  Jobb kan användas för hello följande åtgärder:

* Uppdatera önskade egenskaper
* Uppdatera taggar
* Anropa direkt metoder

Begreppsmässigt ett jobb radbryts någon av följande åtgärder och spårar hello förloppet för körning mot en uppsättning enheter, som definieras av en enhet dubbla fråga.  Till exempel kan en backend-app använda en jobbet tooinvoke en metod för omstart på 10 000 enheter som angetts av en enhet dubbla fråga och schemalagda vid ett senare tillfälle.  Programmet kan sedan följa förloppet som var och en av dessa enheter tar emot och kör hello omstart metod.

Mer information om var och en av dessa funktioner i de här artiklarna:

* Enheten dubbla och egenskaper: [Kom igång med enheten twins] [ lnk-get-started-twin] och [Självstudier: hur toouse enheten dubbla egenskaper][lnk-twin-props]
* dirigera metoder: [IoT-hubb Utvecklarhandbok - direkt metoder] [ lnk-dev-methods] och [Självstudier: Dirigera metoder][lnk-c2d-methods]

I den här självstudiekursen lär du dig att:

* Skapa en simulerad enhetsapp som har en direkt metod som gör att **lockDoor** som kan anropas av hello lösningens serverdel.
* Skapa en Node.js-konsolprogram som anrop hello **lockDoor** direkt metod i hello simulerade enhetsapp med hjälp av ett jobb och uppdateringar hello önskade egenskaper med hjälp av ett jobb för enheten.

Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:

**simDevice.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet och tar emot en **lockDoor** direkt metod.

**scheduleJobService.js**, som anropar en direkt metod i hello simulerade enheten app och uppdatera hello enheten dubblas önskade egenskaper med hjälp av ett jobb.

toocomplete den här kursen behöver du hello följande:

* Node.js-version 0.12.x eller senare <br/>  [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
I det här avsnittet skapar du en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln, vilket utlöser simulerade enheten startas om och använder hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast startas om.

1. Skapa en ny tom mapp som kallas **simDevice**.  I hello **simDevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.  Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **simDevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet-mqtt** paket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Med hjälp av en textredigerare, skapa en ny **simDevice.js** filen i hello **simDevice** mapp.
4. Lägg till följande hello 'krävs, instruktioner hello början av hello **simDevice.js** fil:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Lägg till följande funktion toohandle hello hello **lockDoor** metod.
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```
7. Lägg till följande kod tooregister hello hanterare för hello hello **lockDoor** metod.
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. Spara och Stäng hello **simDevice.js** fil.

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Schemalägga jobb för direkta metoden och uppdatera egenskaperna för en enhet dubbla
I det här avsnittet skapar du en Node.js-konsolprogram som initierar en fjärransluten **lockDoor** på en enhet med hjälp av en direkt metod och uppdatera hello enheten dubblas egenskaper.

1. Skapa en ny tom mapp som kallas **scheduleJobService**.  I hello **scheduleJobService** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.  Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **scheduleJobService** mapp, kör följande kommando tooinstall hello hello **azure iothub** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:
   
    ```
    npm install azure-iothub uuid --save
    ```
3. Med hjälp av en textredigerare, skapa en ny **scheduleJobService.js** filen i hello **scheduleJobService** mapp.
4. Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_gscheduleJobServiceetstarted_service.js** fil:
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. Lägg till följande variabeldeklarationer hello och ersätta platshållarvärdena hello:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. Lägg till följande funktion som kommer att använda toomonitor hello körning av jobbet hello hello:
   
    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```
7. Lägg till hello följande tooschedule hello jobbet kod som anropar hello enheten metoden:
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable tooprocess method
    };
   
    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                queryCondition,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
8. Lägg till följande kod tooschedule hello jobbet tooupdate hello enheten dubbla hello:
   
    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };
   
    var twinJobId = uuid.v4();
   
    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                queryCondition,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
9. Spara och Stäng hello **scheduleJobService.js** fil.

## <a name="run-hello-applications"></a>Köra hello program
Du är nu redo toorun hello program.

1. Kommandotolken hello i hello **simDevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.
   
    ```
    node simDevice.js
    ```
2. Kommandotolken hello i hello **scheduleJobService** mapp, kör följande kommando tootrigger hello jobb toolock hello dörren och uppdatera hello dubbla hello
   
    ```
    node scheduleJobService.js
    ```
3. Du ser hello enheten svar toohello direkt metod i hello-konsolen.

## <a name="next-steps"></a>Nästa steg
I den här kursen används ett jobb tooschedule en direkta metoden tooa enhet och hello uppdatering av hello enheten dubbla egenskaper.

komma igång med IoT-hubb och device management mönster, till exempel remote över hello luften firmware-uppdatering toocontinue finns:

[Självstudier: Hur toodo en inbyggd programvara uppdatera][lnk-fwupdate]

komma igång med IoT-hubb toocontinue finns [komma igång med Azure IoT kant][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

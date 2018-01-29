---
title: "Schemalägga med Azure IoT Hub (nod) | Microsoft Docs"
description: "Så här schemalägger du ett Azure IoT Hub-jobb att anropa en direkt metod på flera enheter. Azure IoT-SDK för Node.js används för att implementera de simulerade enheten apparna och service-appen för att köra jobbet."
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
ms.date: 10/06/2017
ms.author: juanpere
ms.openlocfilehash: e607f5db8b4f2a974cb172d4581dadefe7851275
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/18/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a>Schemat och sändning jobb (nod)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT-hubb är en helt hanterad tjänst som gör en backend-app kan skapa och spåra jobb som schemalägger och uppdatera miljoner enheter.  Jobb kan användas för följande åtgärder:

* Uppdatera önskade egenskaper
* Uppdatera taggar
* Anropa direkt metoder

Begreppsmässigt ett jobb radbryts i någon av följande åtgärder och spårar förloppet för körning mot en uppsättning enheter, som definieras av en enhet dubbla fråga.  En backend-app kan till exempel använda ett jobb för att anropa en metod för omstart i 10 000 enheter som angetts av en enhet dubbla fråga och schemalagda vid ett senare tillfälle.  Programmet kan sedan följa förloppet som var och en av dessa enheter tar emot och kör metod för omstart.

Mer information om var och en av dessa funktioner i de här artiklarna:

* Enheten dubbla och egenskaper: [Kom igång med enheten twins] [ lnk-get-started-twin] och [Självstudier: hur du använder identiska enhetsegenskaper][lnk-twin-props]
* dirigera metoder: [IoT-hubb Utvecklarhandbok - direkt metoder] [ lnk-dev-methods] och [Självstudier: Dirigera metoder][lnk-c2d-methods]

I den här självstudiekursen lär du dig att:

* Skapa en simulerad enhet Node.js-app som har en direkt metod, vilket gör att **lockDoor**, som kan anropas av lösningens serverdel.
* Skapa en Node.js-konsolprogram som anropar den **lockDoor** direkt metod i appen simulerade enheten med ett jobb och uppdateringar de egenskaper med hjälp av en enhet jobbet.

I slutet av den här kursen har du två Node.js-appar:

**simDevice.js**, som ansluter till din IoT-hubb med enhetens identitet och tar emot en **lockDoor** direkt metod.

**scheduleJobService.js**, som anropar en direkt metod i appen simulerade enheten och uppdaterar den enheten dubbla önskade egenskaper med hjälp av ett jobb.

För att kunna genomföra den här kursen behöver du följande:

* Node.js-version 4.0.x eller senare <br/>  [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur du installerar Node.js för den här självstudiekursen på Windows- eller Linux.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
I det här avsnittet skapar du en Node.js-konsolprogram som svarar på en direkt metod som anropas av moln, vilket utlöser en simulerad **lockDoor** metod.

1. Skapa en ny tom mapp som kallas **simDevice**.  I den **simDevice** mapp, skapa en package.json-fil med följande kommando vid en kommandotolk.  Acceptera alla standardvärden:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i den **simDevice** mapp, kör följande kommando för att installera den **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt** paketet:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Med hjälp av en textredigerare, skapa en ny **simDevice.js** filen i den **simDevice** mapp.
4. Lägg till följande 'krävs, instruktioner i början av den **simDevice.js** fil:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Lägg till en **connectionString**-variabel och använd den för att skapa en **klientinstans**.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Lägg till följande funktion att hantera den **lockDoor** metod.
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```
7. Lägg till följande kod för att registrera hanterare för den **lockDoor** metod.
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. Spara och Stäng den **simDevice.js** fil.

> [!NOTE]
> För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen. I produktionskoden bör du implementera principer för omförsök (till exempel en exponentiell backoff), vilket rekommenderas i MSDN-artikeln om [hantering av tillfälliga fel][lnk-transient-faults].
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Schemalägga jobb för direkta metoden och uppdatera egenskaperna för en enhet dubbla
I det här avsnittet skapar du en Node.js-konsolprogram som initierar en fjärransluten **lockDoor** på en enhet med en direkt metod och uppdatera enheten-dubbla egenskaper.

1. Skapa en ny tom mapp som kallas **scheduleJobService**.  I den **scheduleJobService** mapp, skapa en package.json-fil med följande kommando vid en kommandotolk.  Acceptera alla standardvärden:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i den **scheduleJobService** mapp, kör följande kommando för att installera den **azure iothub** enheten SDK-paketet och **azure-iot-enhet – mqtt** paketet:
   
    ```
    npm install azure-iothub uuid --save
    ```
3. Med hjälp av en textredigerare, skapa en ny **scheduleJobService.js** filen i den **scheduleJobService** mapp.
4. Lägg till följande 'krävs, instruktioner i början av den **dmpatterns_gscheduleJobServiceetstarted_service.js** fil:
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. Lägg till följande variabeldeklarationer och ersätta platshållarvärdena:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  300;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. Lägg till följande funktion som används för att övervaka körning av jobbet:
   
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
7. Lägg till följande kod för att schemalägga jobb som anropar metoden enhet:
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable to process method
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
8. Lägg till följande kod för att schemalägga jobbet för att uppdatera enheten dubbla:
   
    ```
    var twinPatch = {
       etag: '*', 
       properties: {
           desired: {
               building: '43', 
               floor: 3
           }
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
9. Spara och Stäng den **scheduleJobService.js** fil.

## <a name="run-the-applications"></a>Köra programmen
Nu är det dags att köra programmen.

1. I Kommandotolken i den **simDevice** mapp, kör följande kommando för att börja lyssna efter omstart direkta metoden.
   
    ```
    node simDevice.js
    ```
2. I Kommandotolken i den **scheduleJobService** mapp, kör följande kommando för att utlösa jobb för att låsa dörren och uppdatera dubbla
   
    ```
    node scheduleJobService.js
    ```
3. Svaret från enheten till den direkta metoden i konsolen visas.

## <a name="next-steps"></a>Nästa steg
I den här kursen används ett jobb för att schemalägga en direkt metod för att en enhet och enheten-dubbla egenskaper att uppdatera.

Om du vill fortsätta komma igång med IoT-hubb och device management mönster, till exempel remote via luften firmware-uppdatering, se:

[Självstudier: Hur du gör en firmware-uppdatering][lnk-fwupdate]

Om du vill fortsätta komma igång med IoT-hubb finns [komma igång med Azure IoT kant][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

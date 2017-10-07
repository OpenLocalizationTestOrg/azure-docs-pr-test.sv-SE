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
# <a name="get-started-with-device-management-node"></a>Komma igång med hantering av enheter (nod)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

I den här självstudiekursen lär du dig att:

* Använd hello Azure portal toocreate en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.
* Skapa en simulerad enhetsapp som innehåller en direkt metod som startar om enheten. Direkta metoder anropas från hello molnet.
* Skapa en Node.js-konsolprogram som anropar hello omstart direkt metod i hello simulerade enheten appen via din IoT-hubb.

Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:

**dmpatterns_getstarted_device.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare, tar emot en direkt metod för omstart simulerar en fysisk omstart och rapporterar hello tid för hello senaste omstarten.

**dmpatterns_getstarted_service.js**, som anropar en direkt metod i hello simulerade enhetsapp visar hello svar och visar hello uppdateras rapporterade egenskaper.

toocomplete den här kursen behöver du hello följande:

* Node.js-version 0.12.x eller senare <br/>  [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
I det här avsnittet kommer du att

* Skapa en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln
* Utlös en simulerad enhet-omstart
* Använd hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast startas om

1. Skapa en tom mapp med namnet **manageddevice**.  I hello **manageddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.  Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **manageddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Med hjälp av en textredigerare, skapa en **dmpatterns_getstarted_device.js** filen i hello **manageddevice** mapp.
4. Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_getstarted_device.js** fil:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans.  Ersätt hello anslutningssträngen med anslutningssträngen enhet.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Lägg till hello följande funktion tooimplement hello direkt metod på hello-enhet
   
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
7. Öppna hello anslutning tooyour IoT-hubb och starta hello direkta metoden lyssnare:
   
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
8. Spara och Stäng hello **dmpatterns_getstarted_device.js** fil.

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a>Utlös en fjärransluten omstart på hello-enhet med en direkt metod
I det här avsnittet skapar du en Node.js-konsolprogram som initierar en fjärransluten omstart på en enhet med en direkt metod. hello appen använder enheten dubbla frågor toodiscover hello omstart senast för enheten.

1. Skapa en tom mapp som kallas **triggerrebootondevice**.  I hello **triggerrebootondevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.  Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **triggerrebootondevice** mapp, kör följande kommando tooinstall hello hello **azure iothub** enheten SDK-paketet och **azure-iot-enhet-mqtt** paketet:
   
    ```
    npm install azure-iothub --save
    ```
3. Med hjälp av en textredigerare, skapa en **dmpatterns_getstarted_service.js** filen i hello **triggerrebootondevice** mapp.
4. Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_getstarted_service.js** fil:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Lägg till följande variabeldeklarationer hello och ersätta platshållarvärdena hello:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. Lägg till följande funktion tooinvoke hello enheten metoden tooreboot hello målenhet hello:
   
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
7. Lägg till hello följande fungera tooquery för hello enhet och få hello omstart senast:
   
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
8. Lägg till följande kod toocall hello funktioner som utlöser hello hello omstart direkta metoden och fråga efter Hej senast omstart tid:
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. Spara och Stäng hello **dmpatterns_getstarted_service.js** fil.

## <a name="run-hello-apps"></a>Köra hello appar
Du är nu redo toorun hello appar.

1. Kommandotolken hello i hello **manageddevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Kommandotolken hello i hello **triggerrebootondevice** , kör följande kommando tootrigger hello remote hello omstart och fråga för hello enheten dubbla toofind hello senast starta om tid.
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. Du ser hello enheten svar toohello direkt metod i hello-konsolen.

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

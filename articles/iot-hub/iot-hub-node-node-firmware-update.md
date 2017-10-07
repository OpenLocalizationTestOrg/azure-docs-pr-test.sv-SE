---
title: aaaDevice firmware-uppdatering med Azure IoT Hub (nod) | Microsoft Docs
description: "Hur uppdaterar toouse enhetshantering i Azure IoT Hub tooinitiate en enhetens inbyggda programvara. Du kan använda hello Azure IoT SDK för Node.js tooimplement en simulerad enhetsapp och en service-appen som utlöser hello firmware-uppdatering."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2017
ms.author: juanpere
ms.openlocfilehash: 99d4b369e7aba334bf713e0c657e6e5d227fb691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-device-management-tooinitiate-a-device-firmware-update-nodenode"></a>Använda device management tooinitiate en enhet på en firmware-uppdatering (nod/nod)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Introduktion
I hello [komma igång med hantering av enheter] [ lnk-dm-getstarted] självstudiekursen kommer du sett hur toouse hello [enheten dubbla] [ lnk-devtwin] och [direkt metoder] [ lnk-c2dmethod] primitiver tooremotely starta om en enhet. Den här självstudiekursen använder hello samma IoT-hubb primitiver och vägledning och visar hur toodo en slutpunkt till slutpunkt simulerade firmware-uppdatering.  Det här mönstret används i hello uppdatering av inbyggd hanteringsprogramvara för hello Intel modern enhet exempel.

I den här självstudiekursen lär du dig att:

* Skapa en Node.js-konsolprogram som anropar hello firmwareUpdate direkt metod i hello simulerade enheten appen via din IoT-hubb.
* Skapa en simulerad enhetsapp som implementerar en **firmwareUpdate** direkt metod. Den här metoden initierar en process i flera steg som väntar toodownload hello firmware bild, laddar hello firmware avbildningen och slutligen gäller hello firmware avbildningen. Under varje steg i hello update rapporterade hello enheten använder hello egenskaper tooreport på pågår.

Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:

**dmpatterns_fwupdate_service.js**, som anropar en direkt metod i hello simulerade enhetsapp visar hello svar och regelbundet (varje 500ms) visar hello uppdateras rapporterade egenskaper.

**dmpatterns_fwupdate_device.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare, tar emot en firmwareUpdate direkt metod, körs via en flera tillstånd processen toosimulate inklusive en inbyggd uppdatering: väntar på hello bild hämta nedladdningen hello nya avbildningen och slutligen tillämpar hello bild.

toocomplete den här kursen behöver du hello följande:

* Node.js-version 0.12.x eller senare <br/>  [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

Följ hello [Kom igång med enhetshantering](iot-hub-node-node-device-management-get-started.md) artikel toocreate din IoT-hubb och hämta anslutningssträngen för IoT-hubb.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a>Utlös en fjärransluten firmware-uppdatering på hello-enhet med en direkt metod
I det här avsnittet skapar du en Node.js-konsolprogram som initierar en fjärransluten firmware-uppdatering på en enhet. hello appen använder en direkta metoden tooinitiate hello uppdatering och använder enheten dubbla frågor tooperiodically få hello status för hello active firmware-uppdatering.

1. Skapa en tom mapp som kallas **triggerfwupdateondevice**.  I hello **triggerfwupdateondevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.  Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **triggerfwupdateondevice** mapp, kör följande kommando tooinstall hello hello **azure iot hub** och **azure-iot-enhet – mqtt** enhet SDK-paket:
   
    ```
    npm install azure-iothub --save
    ```
3. Med hjälp av en textredigerare, skapa en **dmpatterns_getstarted_service.js** filen i hello **triggerfwupdateondevice** mapp.
4. Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_getstarted_service.js** fil:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Lägg till följande variabeldeklarationer hello och ersätta platshållarvärdena hello:
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. Lägg till följande hello fungera toofind och visa hello värde hello firmwareUpdate rapporterade egenskapen.
   
    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```
7. Lägg till följande funktion tooinvoke hello firmwareUpdate metoden tooreboot hello målenhet hello:
   
    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
   
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
   
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
   
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start hello firmware update on hello device: ' + err.message)
        } 
      });
    };
    ```
8. Slutligen, Lägg till hello följande fungera toocode toostart hello firmware update sequence och starta regelbundet visar hello rapporterade egenskaper:
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. Spara och Stäng hello **dmpatterns_fwupdate_service.js** fil.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a>Köra hello appar
Du är nu redo toorun hello appar.

1. Kommandotolken hello i hello **manageddevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Kommandotolken hello i hello **triggerfwupdateondevice** , kör följande kommando tootrigger hello remote hello omstart och fråga för hello enheten dubbla toofind hello senast starta om tid.
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. Du ser hello enheten svar toohello direkt metod i hello-konsolen.

## <a name="next-steps"></a>Nästa steg
I kursen får du använde en direkta metoden tootrigger en fjärransluten firmware-uppdatering på en enhet och använda hello rapporterade egenskaper toofollow hello fortskrider hello firmware-uppdatering.

toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

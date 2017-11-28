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
# <a name="use-device-management-tooinitiate-a-device-firmware-update-nodenode"></a><span data-ttu-id="6b660-104">Använda device management tooinitiate en enhet på en firmware-uppdatering (nod/nod)</span><span class="sxs-lookup"><span data-stu-id="6b660-104">Use device management tooinitiate a device firmware update (Node/Node)</span></span>
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a><span data-ttu-id="6b660-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="6b660-105">Introduction</span></span>
<span data-ttu-id="6b660-106">I hello [komma igång med hantering av enheter] [ lnk-dm-getstarted] självstudiekursen kommer du sett hur toouse hello [enheten dubbla] [ lnk-devtwin] och [direkt metoder] [ lnk-c2dmethod] primitiver tooremotely starta om en enhet.</span><span class="sxs-lookup"><span data-stu-id="6b660-106">In hello [Get started with device management][lnk-dm-getstarted] tutorial, you saw how toouse hello [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod] primitives tooremotely reboot a device.</span></span> <span data-ttu-id="6b660-107">Den här självstudiekursen använder hello samma IoT-hubb primitiver och vägledning och visar hur toodo en slutpunkt till slutpunkt simulerade firmware-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="6b660-107">This tutorial uses hello same IoT Hub primitives and provides guidance and shows you how toodo an end-to-end simulated firmware update.</span></span>  <span data-ttu-id="6b660-108">Det här mönstret används i hello uppdatering av inbyggd hanteringsprogramvara för hello Intel modern enhet exempel.</span><span class="sxs-lookup"><span data-stu-id="6b660-108">This pattern is used in hello firmware update implementation for hello Intel Edison device sample.</span></span>

<span data-ttu-id="6b660-109">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="6b660-109">This tutorial shows you how to:</span></span>

* <span data-ttu-id="6b660-110">Skapa en Node.js-konsolprogram som anropar hello firmwareUpdate direkt metod i hello simulerade enheten appen via din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6b660-110">Create a Node.js console app that calls hello firmwareUpdate direct method in hello simulated device app through your IoT hub.</span></span>
* <span data-ttu-id="6b660-111">Skapa en simulerad enhetsapp som implementerar en **firmwareUpdate** direkt metod.</span><span class="sxs-lookup"><span data-stu-id="6b660-111">Create a simulated device app that implements a **firmwareUpdate** direct method.</span></span> <span data-ttu-id="6b660-112">Den här metoden initierar en process i flera steg som väntar toodownload hello firmware bild, laddar hello firmware avbildningen och slutligen gäller hello firmware avbildningen.</span><span class="sxs-lookup"><span data-stu-id="6b660-112">This method initiates a multi-stage process that waits toodownload hello firmware image, downloads hello firmware image, and finally applies hello firmware image.</span></span> <span data-ttu-id="6b660-113">Under varje steg i hello update rapporterade hello enheten använder hello egenskaper tooreport på pågår.</span><span class="sxs-lookup"><span data-stu-id="6b660-113">During each stage of hello update, hello device uses hello reported properties tooreport on progress.</span></span>

<span data-ttu-id="6b660-114">Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="6b660-114">At hello end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="6b660-115">**dmpatterns_fwupdate_service.js**, som anropar en direkt metod i hello simulerade enhetsapp visar hello svar och regelbundet (varje 500ms) visar hello uppdateras rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6b660-115">**dmpatterns_fwupdate_service.js**, which calls a direct method in hello simulated device app, displays hello response, and periodically (every 500ms) displays hello updated reported properties.</span></span>

<span data-ttu-id="6b660-116">**dmpatterns_fwupdate_device.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare, tar emot en firmwareUpdate direkt metod, körs via en flera tillstånd processen toosimulate inklusive en inbyggd uppdatering: väntar på hello bild hämta nedladdningen hello nya avbildningen och slutligen tillämpar hello bild.</span><span class="sxs-lookup"><span data-stu-id="6b660-116">**dmpatterns_fwupdate_device.js**, which connects tooyour IoT hub with hello device identity created earlier, receives a firmwareUpdate direct method, runs through a multi-state process toosimulate a firmware update including: waiting for hello image download, downloading hello new image, and finally applying hello image.</span></span>

<span data-ttu-id="6b660-117">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="6b660-117">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="6b660-118">Node.js-version 0.12.x eller senare</span><span class="sxs-lookup"><span data-stu-id="6b660-118">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="6b660-119">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="6b660-119">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="6b660-120">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="6b660-120">An active Azure account.</span></span> <span data-ttu-id="6b660-121">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="6b660-121">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="6b660-122">Följ hello [Kom igång med enhetshantering](iot-hub-node-node-device-management-get-started.md) artikel toocreate din IoT-hubb och hämta anslutningssträngen för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="6b660-122">Follow hello [Get started with device management](iot-hub-node-node-device-management-get-started.md) article toocreate your IoT hub and get your IoT Hub connection string.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-hello-device-using-a-direct-method"></a><span data-ttu-id="6b660-123">Utlös en fjärransluten firmware-uppdatering på hello-enhet med en direkt metod</span><span class="sxs-lookup"><span data-stu-id="6b660-123">Trigger a remote firmware update on hello device using a direct method</span></span>
<span data-ttu-id="6b660-124">I det här avsnittet skapar du en Node.js-konsolprogram som initierar en fjärransluten firmware-uppdatering på en enhet.</span><span class="sxs-lookup"><span data-stu-id="6b660-124">In this section, you create a Node.js console app that initiates a remote firmware update on a device.</span></span> <span data-ttu-id="6b660-125">hello appen använder en direkta metoden tooinitiate hello uppdatering och använder enheten dubbla frågor tooperiodically få hello status för hello active firmware-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="6b660-125">hello app uses a direct method tooinitiate hello update and uses device twin queries tooperiodically get hello status of hello active firmware update.</span></span>

1. <span data-ttu-id="6b660-126">Skapa en tom mapp som kallas **triggerfwupdateondevice**.</span><span class="sxs-lookup"><span data-stu-id="6b660-126">Create an empty folder called **triggerfwupdateondevice**.</span></span>  <span data-ttu-id="6b660-127">I hello **triggerfwupdateondevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="6b660-127">In hello **triggerfwupdateondevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="6b660-128">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="6b660-128">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="6b660-129">Vid en kommandotolk i hello **triggerfwupdateondevice** mapp, kör följande kommando tooinstall hello hello **azure iot hub** och **azure-iot-enhet – mqtt** enhet SDK-paket:</span><span class="sxs-lookup"><span data-stu-id="6b660-129">At your command prompt in hello **triggerfwupdateondevice** folder, run hello following command tooinstall hello **azure-iot-hub** and **azure-iot-device-mqtt** Device SDK packages:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="6b660-130">Med hjälp av en textredigerare, skapa en **dmpatterns_getstarted_service.js** filen i hello **triggerfwupdateondevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="6b660-130">Using a text editor, create a **dmpatterns_getstarted_service.js** file in hello **triggerfwupdateondevice** folder.</span></span>
4. <span data-ttu-id="6b660-131">Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_getstarted_service.js** fil:</span><span class="sxs-lookup"><span data-stu-id="6b660-131">Add hello following 'require' statements at hello start of hello **dmpatterns_getstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. <span data-ttu-id="6b660-132">Lägg till följande variabeldeklarationer hello och ersätta platshållarvärdena hello:</span><span class="sxs-lookup"><span data-stu-id="6b660-132">Add hello following variable declarations and replace hello placeholder values:</span></span>
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. <span data-ttu-id="6b660-133">Lägg till följande hello fungera toofind och visa hello värde hello firmwareUpdate rapporterade egenskapen.</span><span class="sxs-lookup"><span data-stu-id="6b660-133">Add hello following function toofind and display hello value of hello firmwareUpdate reported property.</span></span>
   
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
7. <span data-ttu-id="6b660-134">Lägg till följande funktion tooinvoke hello firmwareUpdate metoden tooreboot hello målenhet hello:</span><span class="sxs-lookup"><span data-stu-id="6b660-134">Add hello following function tooinvoke hello firmwareUpdate method tooreboot hello target device:</span></span>
   
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
8. <span data-ttu-id="6b660-135">Slutligen, Lägg till hello följande fungera toocode toostart hello firmware update sequence och starta regelbundet visar hello rapporterade egenskaper:</span><span class="sxs-lookup"><span data-stu-id="6b660-135">Finally, Add hello following function toocode toostart hello firmware update sequence and start periodically showing hello reported properties:</span></span>
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. <span data-ttu-id="6b660-136">Spara och Stäng hello **dmpatterns_fwupdate_service.js** fil.</span><span class="sxs-lookup"><span data-stu-id="6b660-136">Save and close hello **dmpatterns_fwupdate_service.js** file.</span></span>

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-hello-apps"></a><span data-ttu-id="6b660-137">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="6b660-137">Run hello apps</span></span>
<span data-ttu-id="6b660-138">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="6b660-138">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="6b660-139">Kommandotolken hello i hello **manageddevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.</span><span class="sxs-lookup"><span data-stu-id="6b660-139">At hello command prompt in hello **manageddevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. <span data-ttu-id="6b660-140">Kommandotolken hello i hello **triggerfwupdateondevice** , kör följande kommando tootrigger hello remote hello omstart och fråga för hello enheten dubbla toofind hello senast starta om tid.</span><span class="sxs-lookup"><span data-stu-id="6b660-140">At hello command prompt in hello **triggerfwupdateondevice** folder, run hello following command tootrigger hello remote reboot and query for hello device twin toofind hello last reboot time.</span></span>
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. <span data-ttu-id="6b660-141">Du ser hello enheten svar toohello direkt metod i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="6b660-141">You see hello device response toohello direct method in hello console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b660-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6b660-142">Next steps</span></span>
<span data-ttu-id="6b660-143">I kursen får du använde en direkta metoden tootrigger en fjärransluten firmware-uppdatering på en enhet och använda hello rapporterade egenskaper toofollow hello fortskrider hello firmware-uppdatering.</span><span class="sxs-lookup"><span data-stu-id="6b660-143">In this tutorial, you used a direct method tootrigger a remote firmware update on a device and used hello reported properties toofollow hello progress of hello firmware update.</span></span>

<span data-ttu-id="6b660-144">toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="6b660-144">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

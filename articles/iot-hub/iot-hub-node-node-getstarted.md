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
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-node"></a><span data-ttu-id="a1de8-104">Ansluta din simulerade enhet tooyour IoT-hubb med hjälp av noden</span><span class="sxs-lookup"><span data-stu-id="a1de8-104">Connect your simulated device tooyour IoT hub using Node</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="a1de8-105">Hello slutet av den här självstudiekursen har du tre Node.js konsolappar:</span><span class="sxs-lookup"><span data-stu-id="a1de8-105">At hello end of this tutorial, you have three Node.js console apps:</span></span>

* <span data-ttu-id="a1de8-106">**CreateDeviceIdentity.js**, vilket skapar en enhetsidentitet och tillhörande nyckeln tooconnect appen simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="a1de8-106">**CreateDeviceIdentity.js**, which creates a device identity and associated security key tooconnect your simulated device app.</span></span>
* <span data-ttu-id="a1de8-107">**ReadDeviceToCloudMessages.js**, som visar hello telemetri som skickats av din simulerade enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="a1de8-107">**ReadDeviceToCloudMessages.js**, which displays hello telemetry sent by your simulated device app.</span></span>
* <span data-ttu-id="a1de8-108">**SimulatedDevice.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och skickar meddelandet telemetri som alla andra med hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="a1de8-108">**SimulatedDevice.js**, which connects tooyour IoT hub with hello device identity created earlier, and sends a telemetry message every second using hello MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="a1de8-109">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="a1de8-109">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="a1de8-110">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="a1de8-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="a1de8-111">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="a1de8-111">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="a1de8-112">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a1de8-112">An active Azure account.</span></span> <span data-ttu-id="a1de8-113">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="a1de8-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="a1de8-114">Nu har du skapat din IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a1de8-114">You have now created your IoT hub.</span></span> <span data-ttu-id="a1de8-115">Du har hello IoT Hub-värdnamnet och hello IoT-hubb anslutningssträngen som du behöver toocomplete hello resten av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a1de8-115">You have hello IoT Hub host name and hello IoT Hub connection string that you need toocomplete hello rest of this tutorial.</span></span>

## <a name="create-a-device-identity"></a><span data-ttu-id="a1de8-116">Skapa en enhetsidentitet</span><span class="sxs-lookup"><span data-stu-id="a1de8-116">Create a device identity</span></span>
<span data-ttu-id="a1de8-117">I det här avsnittet skapar du en Node.js-konsolprogram som skapar en enhetsidentitet i hello identitetsregistret i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a1de8-117">In this section, you create a Node.js console app that creates a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="a1de8-118">En enhet kan bara ansluta tooIoT hubb om det finns en post i registret för hello identitet.</span><span class="sxs-lookup"><span data-stu-id="a1de8-118">A device can only connect tooIoT hub if it has an entry in hello identity registry.</span></span> <span data-ttu-id="a1de8-119">Mer information finns i hello **Identitetsregistret** avsnitt i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="a1de8-119">For more information, see hello **Identity Registry** section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="a1de8-120">När du kör den här konsolen appen genereras ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den skickar enhet till moln meddelanden tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="a1de8-120">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span>

1. <span data-ttu-id="a1de8-121">Skapa en ny tom mapp med namnet **createdeviceidentity**.</span><span class="sxs-lookup"><span data-stu-id="a1de8-121">Create a new empty folder called **createdeviceidentity**.</span></span> <span data-ttu-id="a1de8-122">I hello **createdeviceidentity** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="a1de8-122">In hello **createdeviceidentity** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="a1de8-123">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="a1de8-123">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="a1de8-124">Vid en kommandotolk i hello **createdeviceidentity** mapp, kör följande kommando tooinstall hello hello **azure iothub** Service SDK-paketet:</span><span class="sxs-lookup"><span data-stu-id="a1de8-124">At your command prompt in hello **createdeviceidentity** folder, run hello following command tooinstall hello **azure-iothub** Service SDK package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="a1de8-125">Med hjälp av en textredigerare, skapa en **CreateDeviceIdentity.js** filen i hello **createdeviceidentity** mapp.</span><span class="sxs-lookup"><span data-stu-id="a1de8-125">Using a text editor, create a **CreateDeviceIdentity.js** file in hello **createdeviceidentity** folder.</span></span>
4. <span data-ttu-id="a1de8-126">Lägg till följande hello `require` instruktionen hello början av hello **CreateDeviceIdentity.js** fil:</span><span class="sxs-lookup"><span data-stu-id="a1de8-126">Add hello following `require` statement at hello start of hello **CreateDeviceIdentity.js** file:</span></span>
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. <span data-ttu-id="a1de8-127">Lägg till följande kod toohello hello **CreateDeviceIdentity.js** filen och Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello:</span><span class="sxs-lookup"><span data-stu-id="a1de8-127">Add hello following code toohello **CreateDeviceIdentity.js** file and replace hello placeholder value with hello IoT Hub connection string for hello hub you created in hello previous section:</span></span> 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="a1de8-128">Lägg till följande kod toocreate en enhet definition i hello identitetsregistret i din IoT-hubb hello.</span><span class="sxs-lookup"><span data-stu-id="a1de8-128">Add hello following code toocreate a device definition in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="a1de8-129">Den här koden skapar en enhet om hello enhets-ID inte finns i hello identitetsregistret, annars returneras hello nyckeln för hello befintlig enhet:</span><span class="sxs-lookup"><span data-stu-id="a1de8-129">This code creates a device if hello device ID does not exist in hello identity registry, otherwise it returns hello key of hello existing device:</span></span>
   
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

7. <span data-ttu-id="a1de8-130">Spara och stäng filen **CreateDeviceIdentity.js**.</span><span class="sxs-lookup"><span data-stu-id="a1de8-130">Save and close **CreateDeviceIdentity.js** file.</span></span>
8. <span data-ttu-id="a1de8-131">toorun hello **createdeviceidentity** program, köra följande kommando i Kommandotolken hello i hello createdeviceidentity mapp hello:</span><span class="sxs-lookup"><span data-stu-id="a1de8-131">toorun hello **createdeviceidentity** application, execute hello following command at hello command prompt in hello createdeviceidentity folder:</span></span>
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. <span data-ttu-id="a1de8-132">Anteckna hello **enhets-ID** och **enhetsnyckel**.</span><span class="sxs-lookup"><span data-stu-id="a1de8-132">Make a note of hello **Device ID** and **Device key**.</span></span> <span data-ttu-id="a1de8-133">Du måste dessa värden senare när du skapar ett program som ansluter tooIoT hubb som en enhet.</span><span class="sxs-lookup"><span data-stu-id="a1de8-133">You need these values later when you create an application that connects tooIoT Hub as a device.</span></span>

> [!NOTE]
> <span data-ttu-id="a1de8-134">Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a1de8-134">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="a1de8-135">Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet.</span><span class="sxs-lookup"><span data-stu-id="a1de8-135">It stores device IDs and keys toouse as security credentials and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="a1de8-136">Om ditt program måste toostore andra enhetsspecifika metadata, använder den en programspecifika butik.</span><span class="sxs-lookup"><span data-stu-id="a1de8-136">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="a1de8-137">Mer information finns i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="a1de8-137">For more information, see hello [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 

<a id="D2C_node"></a>
## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="a1de8-138">Ta emot meddelanden från enheten till molnet</span><span class="sxs-lookup"><span data-stu-id="a1de8-138">Receive device-to-cloud messages</span></span>
<span data-ttu-id="a1de8-139">I det här avsnittet skapar du en Node.js-konsolapp som läser ”enhet till molnet”-meddelanden från IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a1de8-139">In this section, you create a Node.js console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="a1de8-140">En IoT-hubb Exponerar en [Händelsehubbar][lnk-event-hubs-overview]-kompatibel endpoint tooenable du tooread meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="a1de8-140">An IoT hub exposes an [Event Hubs][lnk-event-hubs-overview]-compatible endpoint tooenable you tooread device-to-cloud messages.</span></span> <span data-ttu-id="a1de8-141">enkel tookeep saker, den här guiden skapar en grundläggande läsare som inte lämpar sig för en distribution med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="a1de8-141">tookeep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="a1de8-142">Hej [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen visar hur tooprocess enhet till moln meddelanden i större skala.</span><span class="sxs-lookup"><span data-stu-id="a1de8-142">hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial shows you how tooprocess device-to-cloud messages at scale.</span></span> <span data-ttu-id="a1de8-143">Hej [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] självstudier innehåller ytterligare information om hur tooprocess meddelanden från Event Hubs och är tillämplig toohello IoT-hubb Event Hub-kompatibel slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="a1de8-143">hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial provides further information on how tooprocess messages from Event Hubs and is applicable toohello IoT Hub Event Hub-compatible endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="a1de8-144">hello använder Event Hub-kompatibel slutpunkt för att läsa meddelanden från enhet till moln alltid hello AMQP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="a1de8-144">hello Event Hub-compatible endpoint for reading device-to-cloud messages always uses hello AMQP protocol.</span></span>
> 
> 

1. <span data-ttu-id="a1de8-145">Skapa en tom mapp med namnet **readdevicetocloudmessages**.</span><span class="sxs-lookup"><span data-stu-id="a1de8-145">Create an empty folder called **readdevicetocloudmessages**.</span></span> <span data-ttu-id="a1de8-146">I hello **readdevicetocloudmessages** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="a1de8-146">In hello **readdevicetocloudmessages** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="a1de8-147">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="a1de8-147">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="a1de8-148">Vid en kommandotolk i hello **readdevicetocloudmessages** mapp, kör följande kommando tooinstall hello hello **azure händelsehubbar** paketet:</span><span class="sxs-lookup"><span data-stu-id="a1de8-148">At your command prompt in hello **readdevicetocloudmessages** folder, run hello following command tooinstall hello **azure-event-hubs** package:</span></span>
   
    ```
    npm install azure-event-hubs --save
    ```
3. <span data-ttu-id="a1de8-149">Med hjälp av en textredigerare, skapa en **ReadDeviceToCloudMessages.js** filen i hello **readdevicetocloudmessages** mapp.</span><span class="sxs-lookup"><span data-stu-id="a1de8-149">Using a text editor, create a **ReadDeviceToCloudMessages.js** file in hello **readdevicetocloudmessages** folder.</span></span>
4. <span data-ttu-id="a1de8-150">Lägg till följande hello `require` uttryck högst hello början av hello **ReadDeviceToCloudMessages.js** fil:</span><span class="sxs-lookup"><span data-stu-id="a1de8-150">Add hello following `require` statements at hello start of hello **ReadDeviceToCloudMessages.js** file:</span></span>
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. <span data-ttu-id="a1de8-151">Lägg till följande variabeldeklarationen hello och Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för din hubb:</span><span class="sxs-lookup"><span data-stu-id="a1de8-151">Add hello following variable declaration and replace hello placeholder value with hello IoT Hub connection string for your hub:</span></span>
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. <span data-ttu-id="a1de8-152">Lägg till följande två funktioner som skriver ut utdata toohello konsolen hello:</span><span class="sxs-lookup"><span data-stu-id="a1de8-152">Add hello following two functions that print output toohello console:</span></span>
   
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
7. <span data-ttu-id="a1de8-153">Lägg till följande kod toocreate hello hello **EventHubClient**, öppna hello anslutning tooyour IoT-hubb och skapa en mottagare för varje partition.</span><span class="sxs-lookup"><span data-stu-id="a1de8-153">Add hello following code toocreate hello **EventHubClient**, open hello connection tooyour IoT Hub, and create a receiver for each partition.</span></span> <span data-ttu-id="a1de8-154">Det här programmet använder ett filter när den skapar en mottagare så att hello mottagaren läser endast meddelanden som skickas tooIoT hubb när hello mottagaren börjar köras.</span><span class="sxs-lookup"><span data-stu-id="a1de8-154">This application uses a filter when it creates a receiver so that hello receiver only reads messages sent tooIoT Hub after hello receiver starts running.</span></span> <span data-ttu-id="a1de8-155">Det här filtret är användbart i en testmiljö så att du ser bara hello aktuella uppsättning av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a1de8-155">This filter is useful in a test environment so you see just hello current set of messages.</span></span> <span data-ttu-id="a1de8-156">Koden bör se till att den bearbetar alla hälsningsmeddelande i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a1de8-156">In a production environment, your code should make sure that it processes all hello messages.</span></span> <span data-ttu-id="a1de8-157">Mer information finns i hello [hur tooprocess IoT-hubb meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen:</span><span class="sxs-lookup"><span data-stu-id="a1de8-157">For more information, see hello [How tooprocess IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] tutorial:</span></span>
   
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
8. <span data-ttu-id="a1de8-158">Spara och Stäng hello **ReadDeviceToCloudMessages.js** fil.</span><span class="sxs-lookup"><span data-stu-id="a1de8-158">Save and close hello **ReadDeviceToCloudMessages.js** file.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="a1de8-159">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="a1de8-159">Create a simulated device app</span></span>
<span data-ttu-id="a1de8-160">I det här avsnittet skapar du en Node.js-konsolprogram som simulerar en enhet som skickar meddelanden från enhet till moln tooan IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a1de8-160">In this section, you create a Node.js console app that simulates a device that sends device-to-cloud messages tooan IoT hub.</span></span>

1. <span data-ttu-id="a1de8-161">Skapa en tom mapp med namnet **simulateddevice**.</span><span class="sxs-lookup"><span data-stu-id="a1de8-161">Create an empty folder called **simulateddevice**.</span></span> <span data-ttu-id="a1de8-162">I hello **simulateddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="a1de8-162">In hello **simulateddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="a1de8-163">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="a1de8-163">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="a1de8-164">Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:</span><span class="sxs-lookup"><span data-stu-id="a1de8-164">At your command prompt in hello **simulateddevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="a1de8-165">Med hjälp av en textredigerare, skapa en **SimulatedDevice.js** filen i hello **simulateddevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="a1de8-165">Using a text editor, create a **SimulatedDevice.js** file in hello **simulateddevice** folder.</span></span>
4. <span data-ttu-id="a1de8-166">Lägg till följande hello `require` uttryck högst hello början av hello **SimulatedDevice.js** fil:</span><span class="sxs-lookup"><span data-stu-id="a1de8-166">Add hello following `require` statements at hello start of hello **SimulatedDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. <span data-ttu-id="a1de8-167">Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans.</span><span class="sxs-lookup"><span data-stu-id="a1de8-167">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span> <span data-ttu-id="a1de8-168">Ersätt **{youriothostname}** med hello namnet hello IoT-hubb som du skapade hello *skapar en IoT-hubb* avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a1de8-168">Replace **{youriothostname}** with hello name of hello IoT hub you created hello *Create an IoT Hub* section.</span></span> <span data-ttu-id="a1de8-169">Ersätt **{yourdevicekey}** hello enheten nyckelvärdet du genererade i hello *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="a1de8-169">Replace **{yourdevicekey}** with hello device key value you generated in hello *Create a device identity* section:</span></span>
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. <span data-ttu-id="a1de8-170">Lägg till följande funktion toodisplay utdata från programmet hello hello:</span><span class="sxs-lookup"><span data-stu-id="a1de8-170">Add hello following function toodisplay output from hello application:</span></span>
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. <span data-ttu-id="a1de8-171">Skapa ett återanrop och använda hello **setInterval** fungerar toosend meddelandet tooyour IoT-hubb varje sekund:</span><span class="sxs-lookup"><span data-stu-id="a1de8-171">Create a callback and use hello **setInterval** function toosend a message tooyour IoT hub every second:</span></span>
   
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
8. <span data-ttu-id="a1de8-172">Öppna hello anslutning tooyour IoT-hubb och börja skicka meddelanden:</span><span class="sxs-lookup"><span data-stu-id="a1de8-172">Open hello connection tooyour IoT Hub and start sending messages:</span></span>
   
    ```
    client.open(connectCallback);
    ```
9. <span data-ttu-id="a1de8-173">Spara och Stäng hello **SimulatedDevice.js** fil.</span><span class="sxs-lookup"><span data-stu-id="a1de8-173">Save and close hello **SimulatedDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="a1de8-174">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="a1de8-174">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="a1de8-175">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="a1de8-175">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="run-hello-apps"></a><span data-ttu-id="a1de8-176">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="a1de8-176">Run hello apps</span></span>
<span data-ttu-id="a1de8-177">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="a1de8-177">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="a1de8-178">Vid en kommandotolk i hello **readdevicetocloudmessages** mapp, kör följande kommando toobegin övervaka din IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="a1de8-178">At a command prompt in hello **readdevicetocloudmessages** folder, run hello following command toobegin monitoring your IoT hub:</span></span>
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![Node.js IoT-hubb app toomonitor enhet till moln meddelanden][7]
2. <span data-ttu-id="a1de8-180">Vid en kommandotolk i hello **simulateddevice** mapp, kör följande kommando toobegin skicka telemetri data tooyour IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="a1de8-180">At a command prompt in hello **simulateddevice** folder, run hello following command toobegin sending telemetry data tooyour IoT hub:</span></span>
   
    ```
    node SimulatedDevice.js
    ```
   
    ![Node.js IoT-hubb enheten toosend enhet till moln meddelanden][8]
3. <span data-ttu-id="a1de8-182">Hej **användning** panelen i hello [Azure-portalen] [ lnk-portal] visar hello antalet meddelanden som skickas toohello IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="a1de8-182">hello **Usage** tile in hello [Azure portal][lnk-portal] shows hello number of messages sent toohello IoT hub:</span></span>
   
    ![Azure portal användning panelen visar antalet meddelanden som skickas tooIoT Hub][43]

## <a name="next-steps"></a><span data-ttu-id="a1de8-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1de8-184">Next steps</span></span>
<span data-ttu-id="a1de8-185">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="a1de8-185">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="a1de8-186">Du har använt den här enhetens identitet tooenable hello simulerade enheten app toosend meddelanden från enhet till moln toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a1de8-186">You used this device identity tooenable hello simulated device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="a1de8-187">Du kan också skapat en app som visar hello som tagits emot av hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="a1de8-187">You also created an app that displays hello messages received by hello IoT hub.</span></span> 

<span data-ttu-id="a1de8-188">toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:</span><span class="sxs-lookup"><span data-stu-id="a1de8-188">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="a1de8-189">[Connecting your device][lnk-connect-device] (Ansluta din enhet)</span><span class="sxs-lookup"><span data-stu-id="a1de8-189">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="a1de8-190">[Connecting your device][lnk-device-management] (Komma igång med enhetshantering)</span><span class="sxs-lookup"><span data-stu-id="a1de8-190">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="a1de8-191">[Komma igång med Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="a1de8-191">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="a1de8-192">toolearn hur tooextend IoT-lösningen och processen enhet till moln meddelandena i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="a1de8-192">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
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

---
title: "aaaGet igång med Azure IoT Hub-enhet twins (nod) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooadd taggar och sedan använda en IoT-hubb-fråga. Du använder hello Azure IoT-SDK för Node.js tooimplement hello simulerade enhetsapp och en service-app som lägger till hello taggar och kör hello IoT-hubb frågan."
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: 
ms.assetid: 314c88e4-cce1-441c-b75a-d2e08e39ae7d
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.openlocfilehash: d60b8c3de85e9285e496b86e27d4ee31a0554a1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-node"></a><span data-ttu-id="aa8c1-104">Kom igång med enheten twins (nod)</span><span class="sxs-lookup"><span data-stu-id="aa8c1-104">Get started with device twins (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="aa8c1-105">Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-105">At hello end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="aa8c1-106">**AddTagsAndQuery.js**, en Node.js backend-app som lägger till taggar och frågar enheten twins.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-106">**AddTagsAndQuery.js**, a Node.js back-end app, which adds tags and queries device twins.</span></span>
* <span data-ttu-id="aa8c1-107">**TwinSimulatedDevice.js**, en Node.js-app som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och rapporterar tillståndet anslutningen.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-107">**TwinSimulatedDevice.js**, a Node.js app which simulates a device that connects tooyour IoT hub with hello device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="aa8c1-108">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="aa8c1-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="aa8c1-110">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="aa8c1-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-111">An active Azure account.</span></span> <span data-ttu-id="aa8c1-112">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="aa8c1-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-service-app"></a><span data-ttu-id="aa8c1-113">Skapa hello service-appen</span><span class="sxs-lookup"><span data-stu-id="aa8c1-113">Create hello service app</span></span>
<span data-ttu-id="aa8c1-114">I det här avsnittet skapar du en Node.js-konsolprogram som lägger till platsen metadata toohello enheten dubbla som är associerade med **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-114">In this section, you create a Node.js console app that adds location metadata toohello device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="aa8c1-115">Därefter frågor hello enheten twins lagras i hello IoT-hubb hello enheter finns i hello oss och hello som rapporterar en mobil anslutning.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-115">It then queries hello device twins stored in hello IoT hub selecting hello devices located in hello US, and then hello ones that are reporting a cellular connection.</span></span>

1. <span data-ttu-id="aa8c1-116">Skapa en ny tom mapp som kallas **addtagsandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-116">Create a new empty folder called **addtagsandqueryapp**.</span></span> <span data-ttu-id="aa8c1-117">I hello **addtagsandqueryapp** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-117">In hello **addtagsandqueryapp** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="aa8c1-118">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-118">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="aa8c1-119">Vid en kommandotolk i hello **addtagsandqueryapp** mapp, kör följande kommando tooinstall hello hello **azure iothub** paketet:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-119">At your command prompt in hello **addtagsandqueryapp** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub --save
    ```
3. <span data-ttu-id="aa8c1-120">Med hjälp av en textredigerare, skapa en ny **AddTagsAndQuery.js** filen i hello **addtagsandqueryapp** mapp.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-120">Using a text editor, create a new **AddTagsAndQuery.js** file in hello **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="aa8c1-121">Lägg till följande kod toohello hello **AddTagsAndQuery.js** filen och ersätta hello **{anslutningssträngen för iot-hubb}** med hello IoT-hubb anslutningssträngen som du kopierade när du skapade din hubb:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-121">Add hello following code toohello **AddTagsAndQuery.js** file, and substitute hello **{iot hub connection string}** placeholder with hello IoT Hub connection string you copied when you created your hub:</span></span>
   
        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
   
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });
   
    <span data-ttu-id="aa8c1-122">Hej **registret** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-122">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="aa8c1-123">hello föregående kod först initierar hello **registret** objekt, och sedan hämtar hello enheten dubbla för **myDeviceId**, och slutligen uppdateras taggarna hello önskad platsinformation.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-123">hello previous code first initializes hello **Registry** object, then retrieves hello device twin for **myDeviceId**, and finally updates its tags with hello desired location information.</span></span>
   
    <span data-ttu-id="aa8c1-124">Efter hello uppdaterar hello taggar som den anrop hello **queryTwins** funktion.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-124">After hello updating hello tags it calls hello **queryTwins** function.</span></span>
5. <span data-ttu-id="aa8c1-125">Lägg till följande kod hello slutet av hello **AddTagsAndQuery.js** tooimplement hello **queryTwins** funktionen:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-125">Add hello following code at hello end of  **AddTagsAndQuery.js** tooimplement hello **queryTwins** function:</span></span>
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
   
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   
    <span data-ttu-id="aa8c1-126">hello föregående kod körs två frågor: hello väljer först endast hello enheten twins av enheter som finns i hello **Redmond43** anläggningar och hello andra förfinar hello frågan tooselect bara hello enheter som även är anslutna via mobilnät.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-126">hello previous code executes two queries: hello first selects only hello device twins of devices located in hello **Redmond43** plant, and hello second refines hello query tooselect only hello devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="aa8c1-127">Observera att hello föregående kod när den skapar hello **frågan** objekt, anger ett maximalt antal returnerade dokument.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-127">Note that hello previous code, when it creates hello **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="aa8c1-128">Hej **frågan** objektet innehåller en **hasMoreResults** boolesk egenskap som du kan använda tooinvoke hello **nextAsTwin** metoder flera gånger tooretrieve alla resultat.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-128">hello **query** object contains a **hasMoreResults** boolean property that you can use tooinvoke hello **nextAsTwin** methods multiple times tooretrieve all results.</span></span> <span data-ttu-id="aa8c1-129">En metod som kallas **nästa** är tillgänglig för resultat som inte är enheten twins exempelvis resultatet av aggregering frågor.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-129">A method called **next** is available for results that are not device twins for example, results of aggregation queries.</span></span>
6. <span data-ttu-id="aa8c1-130">Kör programmet hello med:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-130">Run hello application with:</span></span>
   
        node AddTagsAndQuery.js
   
    <span data-ttu-id="aa8c1-131">Du bör se en enhet i hello resultat för hello frågan ställa för alla enheter i **Redmond43** och ingen för hello-fråga som begränsar hello resultatet toodevices som använder ett mobilnät.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-131">You should see one device in hello results for hello query asking for all devices located in **Redmond43** and none for hello query that restricts hello results toodevices that use a cellular network.</span></span>
   
    ![][1]

<span data-ttu-id="aa8c1-132">I nästa avsnitt om hello skapar du en enhetsapp som rapporterar hello anslutningsinformation och ändringar hello resultatet av frågan hello hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-132">In hello next section you create a device app that reports hello connectivity information and changes hello result of hello query in hello previous section.</span></span>

## <a name="create-hello-device-app"></a><span data-ttu-id="aa8c1-133">Skapa hello enhetsapp</span><span class="sxs-lookup"><span data-stu-id="aa8c1-133">Create hello device app</span></span>
<span data-ttu-id="aa8c1-134">I det här avsnittet skapar du en Node.js-konsolprogram som ansluter tooyour hubb som **myDeviceId**, och sedan uppdaterar sin enhet dubbla har rapporterat egenskaper toocontain hello information att den är ansluten med hjälp av ett mobilnät.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-134">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, and then updates its device twin's reported properties toocontain hello information that it is connected using a cellular network.</span></span>

> [!NOTE]
> <span data-ttu-id="aa8c1-135">Just nu är enheten twins kan endast nås från enheter som ansluter tooIoT hubb med hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-135">At this time, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span> <span data-ttu-id="aa8c1-136">Se toohello [MQTT stöd] [ lnk-devguide-mqtt] artikel anvisningar för hur tooconvert befintliga enheten app toouse MQTT.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-136">Please refer toohello [MQTT support][lnk-devguide-mqtt] article for instructions on how tooconvert existing device app toouse MQTT.</span></span>
> 
> 

1. <span data-ttu-id="aa8c1-137">Skapa en ny tom mapp som kallas **reportconnectivity**.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-137">Create a new empty folder called **reportconnectivity**.</span></span> <span data-ttu-id="aa8c1-138">I hello **reportconnectivity** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-138">In hello **reportconnectivity** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="aa8c1-139">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-139">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="aa8c1-140">Vid en kommandotolk i hello **reportconnectivity** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet**, och **azure-iot-enhet – mqtt** paketet :</span><span class="sxs-lookup"><span data-stu-id="aa8c1-140">At your command prompt in hello **reportconnectivity** folder, run hello following command tooinstall hello **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="aa8c1-141">Med hjälp av en textredigerare, skapa en ny **ReportConnectivity.js** filen i hello **reportconnectivity** mapp.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-141">Using a text editor, create a new **ReportConnectivity.js** file in hello **reportconnectivity** folder.</span></span>
4. <span data-ttu-id="aa8c1-142">Lägg till följande kod toohello hello **ReportConnectivity.js** filen och ersätta hello **{enheten anslutningssträngen}** med hello enheten anslutningssträngen som du kopierade när du skapade hello **myDeviceId** enhets-ID:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-142">Add hello following code toohello **ReportConnectivity.js** file, and substitute hello **{device connection string}** placeholder with hello device connection string you copied when you created hello **myDeviceId** device identity:</span></span>
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    <span data-ttu-id="aa8c1-143">Hej **klienten** objektet innehåller alla hello-metoder som du behöver toointeract med enheten twins från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-143">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="aa8c1-144">Hej föregående kod när den initierar hello **klienten** objekt, hämtar hello enheten dubbla för **myDeviceId** och uppdaterar egenskapen rapporterade med hello anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-144">hello previous code, after it initializes hello **Client** object, retrieves hello device twin for **myDeviceId** and updates its reported property with hello connectivity information.</span></span>
5. <span data-ttu-id="aa8c1-145">Kör hello enhetsapp</span><span class="sxs-lookup"><span data-stu-id="aa8c1-145">Run hello device app</span></span>
   
        node ReportConnectivity.js
   
    <span data-ttu-id="aa8c1-146">Du bör se hello-meddelande `twin state reported`.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-146">You should see hello message `twin state reported`.</span></span>
6. <span data-ttu-id="aa8c1-147">Nu när hello enheten rapporterade dess anslutningsinformation, bör den visas i båda frågor.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-147">Now that hello device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="aa8c1-148">Gå tillbaka i hello **addtagsandqueryapp** mappen och kör hello frågar igen:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-148">Go back in hello **addtagsandqueryapp** folder and run hello queries again:</span></span>
   
        node AddTagsAndQuery.js
   
    <span data-ttu-id="aa8c1-149">Den här gången **myDeviceId** ska visas i båda frågeresultaten.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-149">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![][3]

## <a name="next-steps"></a><span data-ttu-id="aa8c1-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa8c1-150">Next steps</span></span>
<span data-ttu-id="aa8c1-151">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-151">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="aa8c1-152">Du lagt till enhetsmetadata som taggar från en backend-app och skrev en simulerad enhet app tooreport anslutning enhetsinformation i hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-152">You added device metadata as tags from a back-end app, and wrote a simulated device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="aa8c1-153">Du också lära sig hur tooquery informationen med hjälp av frågespråket för hello SQL-liknande IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-153">You also learned how tooquery this information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="aa8c1-154">Använd hello följande resurser toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="aa8c1-154">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="aa8c1-155">Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="aa8c1-155">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="aa8c1-156">Konfigurera enheter med hjälp av enheten två egenskaper med hello [Använd önskad egenskaper tooconfigure enheter] [ lnk-twin-how-to-configure] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="aa8c1-156">configure devices using device twin's desired properties with hello [Use desired properties tooconfigure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="aa8c1-157">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app), med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="aa8c1-157">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

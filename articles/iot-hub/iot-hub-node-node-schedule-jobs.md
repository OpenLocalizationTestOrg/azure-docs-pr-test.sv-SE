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
# <a name="schedule-and-broadcast-jobs-node"></a><span data-ttu-id="04179-104">Schemat och sändning jobb (nod)</span><span class="sxs-lookup"><span data-stu-id="04179-104">Schedule and broadcast jobs (Node)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="04179-105">Azure IoT Hub är en helt hanterad tjänst som gör en backend-app toocreate och spåra jobb som schemalägger och uppdatera miljoner enheter.</span><span class="sxs-lookup"><span data-stu-id="04179-105">Azure IoT Hub is a fully managed service that enables a back-end app toocreate and track jobs that schedule and update millions of devices.</span></span>  <span data-ttu-id="04179-106">Jobb kan användas för hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="04179-106">Jobs can be used for hello following actions:</span></span>

* <span data-ttu-id="04179-107">Uppdatera önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="04179-107">Update desired properties</span></span>
* <span data-ttu-id="04179-108">Uppdatera taggar</span><span class="sxs-lookup"><span data-stu-id="04179-108">Update tags</span></span>
* <span data-ttu-id="04179-109">Anropa direkt metoder</span><span class="sxs-lookup"><span data-stu-id="04179-109">Invoke direct methods</span></span>

<span data-ttu-id="04179-110">Begreppsmässigt ett jobb radbryts någon av följande åtgärder och spårar hello förloppet för körning mot en uppsättning enheter, som definieras av en enhet dubbla fråga.</span><span class="sxs-lookup"><span data-stu-id="04179-110">Conceptually, a job wraps one of these actions and tracks hello progress of execution against a set of devices, which is defined by a device twin query.</span></span>  <span data-ttu-id="04179-111">Till exempel kan en backend-app använda en jobbet tooinvoke en metod för omstart på 10 000 enheter som angetts av en enhet dubbla fråga och schemalagda vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="04179-111">For example, a back-end app can use a job tooinvoke a reboot method on 10,000 devices, specified by a device twin query and scheduled at a future time.</span></span>  <span data-ttu-id="04179-112">Programmet kan sedan följa förloppet som var och en av dessa enheter tar emot och kör hello omstart metod.</span><span class="sxs-lookup"><span data-stu-id="04179-112">That application can then track progress as each of those devices receive and execute hello reboot method.</span></span>

<span data-ttu-id="04179-113">Mer information om var och en av dessa funktioner i de här artiklarna:</span><span class="sxs-lookup"><span data-stu-id="04179-113">Learn more about each of these capabilities in these articles:</span></span>

* <span data-ttu-id="04179-114">Enheten dubbla och egenskaper: [Kom igång med enheten twins] [ lnk-get-started-twin] och [Självstudier: hur toouse enheten dubbla egenskaper][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="04179-114">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How toouse device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="04179-115">dirigera metoder: [IoT-hubb Utvecklarhandbok - direkt metoder] [ lnk-dev-methods] och [Självstudier: Dirigera metoder][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="04179-115">direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="04179-116">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="04179-116">This tutorial shows you how to:</span></span>

* <span data-ttu-id="04179-117">Skapa en simulerad enhetsapp som har en direkt metod som gör att **lockDoor** som kan anropas av hello lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="04179-117">Create a simulated device app that has a direct method which enables **lockDoor** which can be called by hello solution back end.</span></span>
* <span data-ttu-id="04179-118">Skapa en Node.js-konsolprogram som anrop hello **lockDoor** direkt metod i hello simulerade enhetsapp med hjälp av ett jobb och uppdateringar hello önskade egenskaper med hjälp av ett jobb för enheten.</span><span class="sxs-lookup"><span data-stu-id="04179-118">Create a Node.js console app that calls hello **lockDoor** direct method in hello simulated device app using a job and updates hello desired properties using a device job.</span></span>

<span data-ttu-id="04179-119">Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="04179-119">At hello end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="04179-120">**simDevice.js**, som ansluter tooyour IoT-hubb med hello enhetsidentitet och tar emot en **lockDoor** direkt metod.</span><span class="sxs-lookup"><span data-stu-id="04179-120">**simDevice.js**, which connects tooyour IoT hub with hello device identity and receives a **lockDoor** direct method.</span></span>

<span data-ttu-id="04179-121">**scheduleJobService.js**, som anropar en direkt metod i hello simulerade enheten app och uppdatera hello enheten dubblas önskade egenskaper med hjälp av ett jobb.</span><span class="sxs-lookup"><span data-stu-id="04179-121">**scheduleJobService.js**, which calls a direct method in hello simulated device app and update hello device twin's desired properties using a job.</span></span>

<span data-ttu-id="04179-122">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="04179-122">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="04179-123">Node.js-version 0.12.x eller senare</span><span class="sxs-lookup"><span data-stu-id="04179-123">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="04179-124">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="04179-124">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="04179-125">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="04179-125">An active Azure account.</span></span> <span data-ttu-id="04179-126">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="04179-126">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="04179-127">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="04179-127">Create a simulated device app</span></span>
<span data-ttu-id="04179-128">I det här avsnittet skapar du en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln, vilket utlöser simulerade enheten startas om och använder hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast startas om.</span><span class="sxs-lookup"><span data-stu-id="04179-128">In this section, you create a Node.js console app that responds tooa direct method called by hello cloud, which triggers a simulated device reboot and uses hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="04179-129">Skapa en ny tom mapp som kallas **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="04179-129">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="04179-130">I hello **simDevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="04179-130">In hello **simDevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="04179-131">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="04179-131">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="04179-132">Vid en kommandotolk i hello **simDevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet-mqtt** paket:</span><span class="sxs-lookup"><span data-stu-id="04179-132">At your command prompt in hello **simDevice** folder, run hello following command tooinstall hello **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="04179-133">Med hjälp av en textredigerare, skapa en ny **simDevice.js** filen i hello **simDevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="04179-133">Using a text editor, create a new **simDevice.js** file in hello **simDevice** folder.</span></span>
4. <span data-ttu-id="04179-134">Lägg till följande hello 'krävs, instruktioner hello början av hello **simDevice.js** fil:</span><span class="sxs-lookup"><span data-stu-id="04179-134">Add hello following 'require' statements at hello start of hello **simDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="04179-135">Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans.</span><span class="sxs-lookup"><span data-stu-id="04179-135">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="04179-136">Lägg till följande funktion toohandle hello hello **lockDoor** metod.</span><span class="sxs-lookup"><span data-stu-id="04179-136">Add hello following function toohandle hello **lockDoor** method.</span></span>
   
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
7. <span data-ttu-id="04179-137">Lägg till följande kod tooregister hello hanterare för hello hello **lockDoor** metod.</span><span class="sxs-lookup"><span data-stu-id="04179-137">Add hello following code tooregister hello handler for hello **lockDoor** method.</span></span>
   
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
8. <span data-ttu-id="04179-138">Spara och Stäng hello **simDevice.js** fil.</span><span class="sxs-lookup"><span data-stu-id="04179-138">Save and close hello **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="04179-139">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="04179-139">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="04179-140">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="04179-140">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a><span data-ttu-id="04179-141">Schemalägga jobb för direkta metoden och uppdatera egenskaperna för en enhet dubbla</span><span class="sxs-lookup"><span data-stu-id="04179-141">Schedule jobs for calling a direct method and updating a device twin's properties</span></span>
<span data-ttu-id="04179-142">I det här avsnittet skapar du en Node.js-konsolprogram som initierar en fjärransluten **lockDoor** på en enhet med hjälp av en direkt metod och uppdatera hello enheten dubblas egenskaper.</span><span class="sxs-lookup"><span data-stu-id="04179-142">In this section, you create a Node.js console app that initiates a remote **lockDoor** on a device using a direct method and update hello device twin's properties.</span></span>

1. <span data-ttu-id="04179-143">Skapa en ny tom mapp som kallas **scheduleJobService**.</span><span class="sxs-lookup"><span data-stu-id="04179-143">Create a new empty folder called **scheduleJobService**.</span></span>  <span data-ttu-id="04179-144">I hello **scheduleJobService** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="04179-144">In hello **scheduleJobService** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="04179-145">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="04179-145">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="04179-146">Vid en kommandotolk i hello **scheduleJobService** mapp, kör följande kommando tooinstall hello hello **azure iothub** enheten SDK-paketet och **azure-iot-enhet – mqtt**paketet:</span><span class="sxs-lookup"><span data-stu-id="04179-146">At your command prompt in hello **scheduleJobService** folder, run hello following command tooinstall hello **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub uuid --save
    ```
3. <span data-ttu-id="04179-147">Med hjälp av en textredigerare, skapa en ny **scheduleJobService.js** filen i hello **scheduleJobService** mapp.</span><span class="sxs-lookup"><span data-stu-id="04179-147">Using a text editor, create a new **scheduleJobService.js** file in hello **scheduleJobService** folder.</span></span>
4. <span data-ttu-id="04179-148">Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_gscheduleJobServiceetstarted_service.js** fil:</span><span class="sxs-lookup"><span data-stu-id="04179-148">Add hello following 'require' statements at hello start of hello **dmpatterns_gscheduleJobServiceetstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. <span data-ttu-id="04179-149">Lägg till följande variabeldeklarationer hello och ersätta platshållarvärdena hello:</span><span class="sxs-lookup"><span data-stu-id="04179-149">Add hello following variable declarations and replace hello placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="04179-150">Lägg till följande funktion som kommer att använda toomonitor hello körning av jobbet hello hello:</span><span class="sxs-lookup"><span data-stu-id="04179-150">Add hello following function that will be used toomonitor hello execution of hello job:</span></span>
   
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
7. <span data-ttu-id="04179-151">Lägg till hello följande tooschedule hello jobbet kod som anropar hello enheten metoden:</span><span class="sxs-lookup"><span data-stu-id="04179-151">Add hello following code tooschedule hello job that calls hello device method:</span></span>
   
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
8. <span data-ttu-id="04179-152">Lägg till följande kod tooschedule hello jobbet tooupdate hello enheten dubbla hello:</span><span class="sxs-lookup"><span data-stu-id="04179-152">Add hello following code tooschedule hello job tooupdate hello device twin:</span></span>
   
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
9. <span data-ttu-id="04179-153">Spara och Stäng hello **scheduleJobService.js** fil.</span><span class="sxs-lookup"><span data-stu-id="04179-153">Save and close hello **scheduleJobService.js** file.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="04179-154">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="04179-154">Run hello applications</span></span>
<span data-ttu-id="04179-155">Du är nu redo toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="04179-155">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="04179-156">Kommandotolken hello i hello **simDevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.</span><span class="sxs-lookup"><span data-stu-id="04179-156">At hello command prompt in hello **simDevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>
   
    ```
    node simDevice.js
    ```
2. <span data-ttu-id="04179-157">Kommandotolken hello i hello **scheduleJobService** mapp, kör följande kommando tootrigger hello jobb toolock hello dörren och uppdatera hello dubbla hello</span><span class="sxs-lookup"><span data-stu-id="04179-157">At hello command prompt in hello **scheduleJobService** folder, run hello following command tootrigger hello jobs toolock hello door and update hello twin</span></span>
   
    ```
    node scheduleJobService.js
    ```
3. <span data-ttu-id="04179-158">Du ser hello enheten svar toohello direkt metod i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="04179-158">You see hello device response toohello direct method in hello console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04179-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="04179-159">Next steps</span></span>
<span data-ttu-id="04179-160">I den här kursen används ett jobb tooschedule en direkta metoden tooa enhet och hello uppdatering av hello enheten dubbla egenskaper.</span><span class="sxs-lookup"><span data-stu-id="04179-160">In this tutorial, you used a job tooschedule a direct method tooa device and hello update of hello device twin's properties.</span></span>

<span data-ttu-id="04179-161">komma igång med IoT-hubb och device management mönster, till exempel remote över hello luften firmware-uppdatering toocontinue finns:</span><span class="sxs-lookup"><span data-stu-id="04179-161">toocontinue getting started with IoT Hub and device management patterns such as remote over hello air firmware update, see:</span></span>

<span data-ttu-id="04179-162">[Självstudier: Hur toodo en inbyggd programvara uppdatera][lnk-fwupdate]</span><span class="sxs-lookup"><span data-stu-id="04179-162">[Tutorial: How toodo a firmware update][lnk-fwupdate]</span></span>

<span data-ttu-id="04179-163">komma igång med IoT-hubb toocontinue finns [komma igång med Azure IoT kant][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="04179-163">toocontinue getting started with IoT Hub, see [Getting started with Azure IoT Edge][lnk-iot-edge].</span></span>

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

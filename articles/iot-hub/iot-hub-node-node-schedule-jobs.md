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
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: 42e594dc6a8a8be619b5652bf8e44cf883650489
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a><span data-ttu-id="2682c-104">Schemat och sändning jobb (nod)</span><span class="sxs-lookup"><span data-stu-id="2682c-104">Schedule and broadcast jobs (Node)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="2682c-105">Azure IoT-hubb är en helt hanterad tjänst som gör en backend-app kan skapa och spåra jobb som schemalägger och uppdatera miljoner enheter.</span><span class="sxs-lookup"><span data-stu-id="2682c-105">Azure IoT Hub is a fully managed service that enables a back-end app to create and track jobs that schedule and update millions of devices.</span></span>  <span data-ttu-id="2682c-106">Jobb kan användas för följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="2682c-106">Jobs can be used for the following actions:</span></span>

* <span data-ttu-id="2682c-107">Uppdatera önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="2682c-107">Update desired properties</span></span>
* <span data-ttu-id="2682c-108">Uppdatera taggar</span><span class="sxs-lookup"><span data-stu-id="2682c-108">Update tags</span></span>
* <span data-ttu-id="2682c-109">Anropa direkt metoder</span><span class="sxs-lookup"><span data-stu-id="2682c-109">Invoke direct methods</span></span>

<span data-ttu-id="2682c-110">Begreppsmässigt ett jobb radbryts i någon av följande åtgärder och spårar förloppet för körning mot en uppsättning enheter, som definieras av en enhet dubbla fråga.</span><span class="sxs-lookup"><span data-stu-id="2682c-110">Conceptually, a job wraps one of these actions and tracks the progress of execution against a set of devices, which is defined by a device twin query.</span></span>  <span data-ttu-id="2682c-111">En backend-app kan till exempel använda ett jobb för att anropa en metod för omstart i 10 000 enheter som angetts av en enhet dubbla fråga och schemalagda vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="2682c-111">For example, a back-end app can use a job to invoke a reboot method on 10,000 devices, specified by a device twin query and scheduled at a future time.</span></span>  <span data-ttu-id="2682c-112">Programmet kan sedan följa förloppet som var och en av dessa enheter tar emot och kör metod för omstart.</span><span class="sxs-lookup"><span data-stu-id="2682c-112">That application can then track progress as each of those devices receive and execute the reboot method.</span></span>

<span data-ttu-id="2682c-113">Mer information om var och en av dessa funktioner i de här artiklarna:</span><span class="sxs-lookup"><span data-stu-id="2682c-113">Learn more about each of these capabilities in these articles:</span></span>

* <span data-ttu-id="2682c-114">Enheten dubbla och egenskaper: [Kom igång med enheten twins] [ lnk-get-started-twin] och [Självstudier: hur du använder identiska enhetsegenskaper][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="2682c-114">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How to use device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="2682c-115">dirigera metoder: [IoT-hubb Utvecklarhandbok - direkt metoder] [ lnk-dev-methods] och [Självstudier: Dirigera metoder][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="2682c-115">direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="2682c-116">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="2682c-116">This tutorial shows you how to:</span></span>

* <span data-ttu-id="2682c-117">Skapa en simulerad enhetsapp som har en direkt metod som gör att **lockDoor** som kan anropas av lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="2682c-117">Create a simulated device app that has a direct method which enables **lockDoor** which can be called by the solution back end.</span></span>
* <span data-ttu-id="2682c-118">Skapa en Node.js-konsolprogram som anropar den **lockDoor** direkt metod i appen simulerade enheten med ett jobb och uppdateringar de egenskaper med hjälp av en enhet jobbet.</span><span class="sxs-lookup"><span data-stu-id="2682c-118">Create a Node.js console app that calls the **lockDoor** direct method in the simulated device app using a job and updates the desired properties using a device job.</span></span>

<span data-ttu-id="2682c-119">I slutet av den här kursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="2682c-119">At the end of this tutorial, you have two Node.js console apps:</span></span>

<span data-ttu-id="2682c-120">**simDevice.js**, som ansluter till din IoT-hubb med enhetens identitet och tar emot en **lockDoor** direkt metod.</span><span class="sxs-lookup"><span data-stu-id="2682c-120">**simDevice.js**, which connects to your IoT hub with the device identity and receives a **lockDoor** direct method.</span></span>

<span data-ttu-id="2682c-121">**scheduleJobService.js**, som anropar en direkt metod i appen simulerade enheten och uppdatera enheten dubbla önskade egenskaper med hjälp av ett jobb.</span><span class="sxs-lookup"><span data-stu-id="2682c-121">**scheduleJobService.js**, which calls a direct method in the simulated device app and update the device twin's desired properties using a job.</span></span>

<span data-ttu-id="2682c-122">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2682c-122">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="2682c-123">Node.js-version 0.12.x eller senare</span><span class="sxs-lookup"><span data-stu-id="2682c-123">Node.js version 0.12.x or later,</span></span> <br/>  <span data-ttu-id="2682c-124">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur du installerar Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="2682c-124">[Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="2682c-125">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="2682c-125">An active Azure account.</span></span> <span data-ttu-id="2682c-126">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="2682c-126">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="2682c-127">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="2682c-127">Create a simulated device app</span></span>
<span data-ttu-id="2682c-128">I det här avsnittet skapar du en Node.js-konsolprogram som svarar på en direkt metod som anropas av molnet, vilket utlöser simulerade enheten startas om och använder rapporterade egenskaperna för att aktivera enheten dubbla frågor för att identifiera enheter och när de senast startas om.</span><span class="sxs-lookup"><span data-stu-id="2682c-128">In this section, you create a Node.js console app that responds to a direct method called by the cloud, which triggers a simulated device reboot and uses the reported properties to enable device twin queries to identify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="2682c-129">Skapa en ny tom mapp som kallas **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="2682c-129">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="2682c-130">I den **simDevice** mapp, skapa en package.json-fil med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="2682c-130">In the **simDevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="2682c-131">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="2682c-131">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="2682c-132">Vid en kommandotolk i den **simDevice** mapp, kör följande kommando för att installera den **azure iot-enhet** enheten SDK-paketet och **azure-iot-enhet – mqtt** paketet:</span><span class="sxs-lookup"><span data-stu-id="2682c-132">At your command prompt in the **simDevice** folder, run the following command to install the **azure-iot-device** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="2682c-133">Med hjälp av en textredigerare, skapa en ny **simDevice.js** filen i den **simDevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="2682c-133">Using a text editor, create a new **simDevice.js** file in the **simDevice** folder.</span></span>
4. <span data-ttu-id="2682c-134">Lägg till följande 'krävs, instruktioner i början av den **simDevice.js** fil:</span><span class="sxs-lookup"><span data-stu-id="2682c-134">Add the following 'require' statements at the start of the **simDevice.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. <span data-ttu-id="2682c-135">Lägg till en **connectionString**-variabel och använd den för att skapa en **klientinstans**.</span><span class="sxs-lookup"><span data-stu-id="2682c-135">Add a **connectionString** variable and use it to create a **Client** instance.</span></span>  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. <span data-ttu-id="2682c-136">Lägg till följande funktion att hantera den **lockDoor** metod.</span><span class="sxs-lookup"><span data-stu-id="2682c-136">Add the following function to handle the **lockDoor** method.</span></span>
   
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
7. <span data-ttu-id="2682c-137">Lägg till följande kod för att registrera hanterare för den **lockDoor** metod.</span><span class="sxs-lookup"><span data-stu-id="2682c-137">Add the following code to register the handler for the **lockDoor** method.</span></span>
   
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
8. <span data-ttu-id="2682c-138">Spara och Stäng den **simDevice.js** fil.</span><span class="sxs-lookup"><span data-stu-id="2682c-138">Save and close the **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="2682c-139">För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="2682c-139">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="2682c-140">I produktionskoden bör du implementera principer för omförsök (till exempel en exponentiell backoff), vilket rekommenderas i MSDN-artikeln om [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="2682c-140">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a><span data-ttu-id="2682c-141">Schemalägga jobb för direkta metoden och uppdatera egenskaperna för en enhet dubbla</span><span class="sxs-lookup"><span data-stu-id="2682c-141">Schedule jobs for calling a direct method and updating a device twin's properties</span></span>
<span data-ttu-id="2682c-142">I det här avsnittet skapar du en Node.js-konsolprogram som initierar en fjärransluten **lockDoor** på en enhet med en direkt metod och uppdatera enheten-dubbla egenskaper.</span><span class="sxs-lookup"><span data-stu-id="2682c-142">In this section, you create a Node.js console app that initiates a remote **lockDoor** on a device using a direct method and update the device twin's properties.</span></span>

1. <span data-ttu-id="2682c-143">Skapa en ny tom mapp som kallas **scheduleJobService**.</span><span class="sxs-lookup"><span data-stu-id="2682c-143">Create a new empty folder called **scheduleJobService**.</span></span>  <span data-ttu-id="2682c-144">I den **scheduleJobService** mapp, skapa en package.json-fil med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="2682c-144">In the **scheduleJobService** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="2682c-145">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="2682c-145">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="2682c-146">Vid en kommandotolk i den **scheduleJobService** mapp, kör följande kommando för att installera den **azure iothub** enheten SDK-paketet och **azure-iot-enhet – mqtt** paketet:</span><span class="sxs-lookup"><span data-stu-id="2682c-146">At your command prompt in the **scheduleJobService** folder, run the following command to install the **azure-iothub** Device SDK package and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iothub uuid --save
    ```
3. <span data-ttu-id="2682c-147">Med hjälp av en textredigerare, skapa en ny **scheduleJobService.js** filen i den **scheduleJobService** mapp.</span><span class="sxs-lookup"><span data-stu-id="2682c-147">Using a text editor, create a new **scheduleJobService.js** file in the **scheduleJobService** folder.</span></span>
4. <span data-ttu-id="2682c-148">Lägg till följande 'krävs, instruktioner i början av den **dmpatterns_gscheduleJobServiceetstarted_service.js** fil:</span><span class="sxs-lookup"><span data-stu-id="2682c-148">Add the following 'require' statements at the start of the **dmpatterns_gscheduleJobServiceetstarted_service.js** file:</span></span>
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. <span data-ttu-id="2682c-149">Lägg till följande variabeldeklarationer och ersätta platshållarvärdena:</span><span class="sxs-lookup"><span data-stu-id="2682c-149">Add the following variable declarations and replace the placeholder values:</span></span>
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. <span data-ttu-id="2682c-150">Lägg till följande funktion som ska användas för att övervaka körning av jobbet:</span><span class="sxs-lookup"><span data-stu-id="2682c-150">Add the following function that will be used to monitor the execution of the job:</span></span>
   
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
7. <span data-ttu-id="2682c-151">Lägg till följande kod för att schemalägga jobb som anropar metoden enhet:</span><span class="sxs-lookup"><span data-stu-id="2682c-151">Add the following code to schedule the job that calls the device method:</span></span>
   
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
8. <span data-ttu-id="2682c-152">Lägg till följande kod för att schemalägga jobbet för att uppdatera enheten dubbla:</span><span class="sxs-lookup"><span data-stu-id="2682c-152">Add the following code to schedule the job to update the device twin:</span></span>
   
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
9. <span data-ttu-id="2682c-153">Spara och Stäng den **scheduleJobService.js** fil.</span><span class="sxs-lookup"><span data-stu-id="2682c-153">Save and close the **scheduleJobService.js** file.</span></span>

## <a name="run-the-applications"></a><span data-ttu-id="2682c-154">Köra programmen</span><span class="sxs-lookup"><span data-stu-id="2682c-154">Run the applications</span></span>
<span data-ttu-id="2682c-155">Nu är det dags att köra programmen.</span><span class="sxs-lookup"><span data-stu-id="2682c-155">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="2682c-156">I Kommandotolken i den **simDevice** mapp, kör följande kommando för att börja lyssna efter omstart direkta metoden.</span><span class="sxs-lookup"><span data-stu-id="2682c-156">At the command prompt in the **simDevice** folder, run the following command to begin listening for the reboot direct method.</span></span>
   
    ```
    node simDevice.js
    ```
2. <span data-ttu-id="2682c-157">I Kommandotolken i den **scheduleJobService** mapp, kör följande kommando för att utlösa jobb för att låsa dörren och uppdatera dubbla</span><span class="sxs-lookup"><span data-stu-id="2682c-157">At the command prompt in the **scheduleJobService** folder, run the following command to trigger the jobs to lock the door and update the twin</span></span>
   
    ```
    node scheduleJobService.js
    ```
3. <span data-ttu-id="2682c-158">Svaret från enheten till den direkta metoden i konsolen visas.</span><span class="sxs-lookup"><span data-stu-id="2682c-158">You see the device response to the direct method in the console.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2682c-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2682c-159">Next steps</span></span>
<span data-ttu-id="2682c-160">I den här kursen används ett jobb för att schemalägga en direkt metod för att en enhet och enheten-dubbla egenskaper att uppdatera.</span><span class="sxs-lookup"><span data-stu-id="2682c-160">In this tutorial, you used a job to schedule a direct method to a device and the update of the device twin's properties.</span></span>

<span data-ttu-id="2682c-161">Om du vill fortsätta komma igång med IoT-hubb och device management mönster, till exempel remote via luften firmware-uppdatering, se:</span><span class="sxs-lookup"><span data-stu-id="2682c-161">To continue getting started with IoT Hub and device management patterns such as remote over the air firmware update, see:</span></span>

<span data-ttu-id="2682c-162">[Självstudier: Hur du gör en firmware-uppdatering][lnk-fwupdate]</span><span class="sxs-lookup"><span data-stu-id="2682c-162">[Tutorial: How to do a firmware update][lnk-fwupdate]</span></span>

<span data-ttu-id="2682c-163">Om du vill fortsätta komma igång med IoT-hubb finns [komma igång med Azure IoT kant][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="2682c-163">To continue getting started with IoT Hub, see [Getting started with Azure IoT Edge][lnk-iot-edge].</span></span>

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

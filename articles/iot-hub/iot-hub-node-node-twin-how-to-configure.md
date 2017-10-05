---
title: "Använd Azure IoT Hub dubbla enhetsegenskaper (nod) | Microsoft Docs"
description: "Hur du använder Azure IoT Hub-enhet twins för att konfigurera enheter. Du kan använda Azure IoT-SDK för Node.js för att implementera en simulerad enhetsapp och en service-app som ändrar en konfiguration för enheter med hjälp av en enhet dubbla."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: elioda
ms.openlocfilehash: 771106ce7b00a5231d9929e4b5ea34aefe693597
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-desired-properties-to-configure-devices-node"></a><span data-ttu-id="5c86f-104">Använd önskade egenskaper för att konfigurera enheter (nod)</span><span class="sxs-lookup"><span data-stu-id="5c86f-104">Use desired properties to configure devices (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="5c86f-105">I slutet av den här kursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="5c86f-105">At the end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="5c86f-106">**SimulateDeviceConfiguration.js**, en simulerad enhetsapp som väntar på en uppdatering för önskad konfiguration och rapporterar status för en simulerad konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="5c86f-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports the status of a simulated configuration update process.</span></span>
* <span data-ttu-id="5c86f-107">**SetDesiredConfigurationAndQuery.js**, en Node.js backend-app, vilket anger du önskad konfiguration på en enhet och frågar konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="5c86f-107">**SetDesiredConfigurationAndQuery.js**, a Node.js back-end app, which sets the desired configuration on a device and queries the configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="5c86f-108">Artikeln [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om Azure IoT-SDK: er som du kan använda för att skapa både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="5c86f-108">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="5c86f-109">Den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="5c86f-109">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="5c86f-110">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="5c86f-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="5c86f-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="5c86f-111">An active Azure account.</span></span> <span data-ttu-id="5c86f-112">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="5c86f-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="5c86f-113">Om du har följt den [Kom igång med enheten twins] [ lnk-twin-tutorial] kursen har du redan en IoT-hubb och en enhetsidentitet kallas **myDeviceId**, och du kan hoppa till [Skapa den simulerade enhetsapp] [ lnk-how-to-configure-createapp] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5c86f-113">If you followed the [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**; and you can skip to the [Create the simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a><span data-ttu-id="5c86f-114">Skapa den simulerade enhetsapp</span><span class="sxs-lookup"><span data-stu-id="5c86f-114">Create the simulated device app</span></span>
<span data-ttu-id="5c86f-115">I det här avsnittet skapar du en Node.js-konsolprogram som ansluter till din hubb som **myDeviceId**, väntar på en uppdatering för önskad konfiguration och rapporterar uppdateringar på uppdateringsprocessen simulerade konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5c86f-115">In this section, you create a Node.js console app that connects to your hub as **myDeviceId**, waits for a desired configuration update and then reports updates on the simulated configuration update process.</span></span>

1. <span data-ttu-id="5c86f-116">Skapa en ny tom mapp som kallas **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="5c86f-116">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="5c86f-117">I den **simulatedeviceconfiguration** mapp, skapa en ny package.json-fil med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="5c86f-117">In the **simulatedeviceconfiguration** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="5c86f-118">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="5c86f-118">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="5c86f-119">Vid en kommandotolk i den **simulatedeviceconfiguration** mapp, kör följande kommando för att installera den **azure iot-enhet**, och **azure-iot-enhet-mqtt** paket:</span><span class="sxs-lookup"><span data-stu-id="5c86f-119">At your command prompt in the **simulatedeviceconfiguration** folder, run the following command to install the **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="5c86f-120">Med hjälp av en textredigerare, skapa en ny **SimulateDeviceConfiguration.js** filen i den **simulatedeviceconfiguration** mapp.</span><span class="sxs-lookup"><span data-stu-id="5c86f-120">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in the **simulatedeviceconfiguration** folder.</span></span>
4. <span data-ttu-id="5c86f-121">Lägg till följande kod i den **SimulateDeviceConfiguration.js** filen och Ersätt den **{enheten anslutningssträngen}** platshållaren med den anslutningssträng för enheten som du kopierade när du skapade den **myDeviceId** enhets-ID:</span><span class="sxs-lookup"><span data-stu-id="5c86f-121">Add the following code to the **SimulateDeviceConfiguration.js** file, and substitute the **{device connection string}** placeholder with the device connection string you copied when you created the **myDeviceId** device identity:</span></span>
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    <span data-ttu-id="5c86f-122">Den **klienten** objektet innehåller de metoder som krävs för att interagera med enheten twins från enheten.</span><span class="sxs-lookup"><span data-stu-id="5c86f-122">The **Client** object exposes all the methods required to interact with device twins from the device.</span></span> <span data-ttu-id="5c86f-123">Föregående kod när den initieras den **klienten** objekt, hämtar enheten dubbla för **myDeviceId**, och bifogar en hanterare för uppdatering på Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5c86f-123">The previous code, after it initializes the **Client** object, retrieves the device twin for **myDeviceId**, and attaches a handler for the update on desired properties.</span></span> <span data-ttu-id="5c86f-124">Hanteraren verifierar att det är en verklig configuration ändringsbegäran genom att jämföra configIds sedan anropar en metod som startar konfigurationsändringen.</span><span class="sxs-lookup"><span data-stu-id="5c86f-124">The handler verifies that there is an actual configuration change request by comparing the configIds, then invokes a method that starts the configuration change.</span></span>
   
    <span data-ttu-id="5c86f-125">Observera att för enkelhetens skull, föregående kod använder en hårdkodad standard för den inledande konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="5c86f-125">Note that for the sake of simplicity, the previous code uses a hard-coded default for the inital configuration.</span></span> <span data-ttu-id="5c86f-126">En verklig app skulle förmodligen att läsa in konfigurationen från en lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="5c86f-126">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="5c86f-127">Önskad egenskapen Ändringshändelser släpps alltid en gång vid enhetsanslutning, se till att kontrollera att det finns en verklig ändring i egenskaperna innan du utför några åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5c86f-127">Desired property change events are always emitted once at device connection, make sure to check that there is an actual change in the desired properties before performing any action.</span></span>
   > 
   > 
5. <span data-ttu-id="5c86f-128">Lägg till följande metoder innan den `client.open()` anrop:</span><span class="sxs-lookup"><span data-stu-id="5c86f-128">Add the following methods before the `client.open()` invocation:</span></span>
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    <span data-ttu-id="5c86f-129">Den **initConfigChange** metoden uppdaterar rapporterade egenskaper för lokal enhet dubbla objektet med begäran om uppdatering konfiguration och anger statusen till **väntande**, uppdaterar sedan enheten dubbla på den tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5c86f-129">The **initConfigChange** method updates reported properties on the local device twin object with the configuration update request and sets the status to **Pending**, then updates the device twin on the service.</span></span> <span data-ttu-id="5c86f-130">När en uppdatering av enheten dubbla, den simulerar en tidskrävande process som slutar i körningen av **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="5c86f-130">After successfully updating the device twin, it simulates a long running process that terminates in the execution of **completeConfigChange**.</span></span> <span data-ttu-id="5c86f-131">Den här metoden uppdaterar den lokala enheten dubbla rapporterade egenskaper som anger status till **lyckade** och ta bort den **pendingConfig** objekt.</span><span class="sxs-lookup"><span data-stu-id="5c86f-131">This method updates the local device twin's reported properties setting the status to **Success** and removing the **pendingConfig** object.</span></span> <span data-ttu-id="5c86f-132">Sedan uppdaterar den enheten dubbla på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5c86f-132">It then updates the device twin on the service.</span></span>
   
    <span data-ttu-id="5c86f-133">Observera att, för att spara bandbredd, rapporteras egenskaper uppdateras genom att ange egenskaperna som ska ändras (med namnet **korrigering** i koden ovan), i stället för att ersätta hela dokumentet.</span><span class="sxs-lookup"><span data-stu-id="5c86f-133">Note that, to save bandwidth, reported properties are updated by specifying only the properties to be modified (named **patch** in the above code), instead of replacing the whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5c86f-134">Den här självstudiekursen simulerar inte någon funktion för samtidiga konfigurationsuppdateringar.</span><span class="sxs-lookup"><span data-stu-id="5c86f-134">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="5c86f-135">Vissa processer för uppdatering av konfiguration kanske kan hantera förändringar av mål-konfiguration när uppdateringen körs, andra behöva placeras i kö och andra kan avvisa dem med ett feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="5c86f-135">Some configuration update processes might be able to accommodate changes of target configuration while the update is running, others might have to queue them, and others could reject them with an error condition.</span></span> <span data-ttu-id="5c86f-136">Se till att tänka på önskat beteende för din specifika konfigurationsprocessen och Lägg till lämpliga logiken innan du påbörjar konfigurationsändringen.</span><span class="sxs-lookup"><span data-stu-id="5c86f-136">Make sure to consider the desired behavior for your specific configuration process, and add the appropriate logic before initiating the configuration change.</span></span>
   > 
   > 
6. <span data-ttu-id="5c86f-137">Kör appen enhet:</span><span class="sxs-lookup"><span data-stu-id="5c86f-137">Run the device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="5c86f-138">Du bör se meddelandet `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="5c86f-138">You should see the message `retrieved device twin`.</span></span> <span data-ttu-id="5c86f-139">Fortsätt att köra appen.</span><span class="sxs-lookup"><span data-stu-id="5c86f-139">Keep the app running.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="5c86f-140">Skapa service-appen</span><span class="sxs-lookup"><span data-stu-id="5c86f-140">Create the service app</span></span>
<span data-ttu-id="5c86f-141">I det här avsnittet skapar du en Node.js-konsolprogram som uppdaterar det *önskade egenskaper* på enhet-dubbla som är associerade med **myDeviceId** med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="5c86f-141">In this section, you will create a Node.js console app that updates the *desired properties* on the device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="5c86f-142">Sedan frågar enheten-twins lagras i IoT-hubb och visar skillnaden mellan önskade och rapporterade konfigurationer av enheten.</span><span class="sxs-lookup"><span data-stu-id="5c86f-142">It then queries the device twins stored in the IoT hub and shows the difference between the desired and reported configurations of the device.</span></span>

1. <span data-ttu-id="5c86f-143">Skapa en ny tom mapp som kallas **setdesiredandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="5c86f-143">Create a new empty folder called **setdesiredandqueryapp**.</span></span> <span data-ttu-id="5c86f-144">I den **setdesiredandqueryapp** mapp, skapa en ny package.json-fil med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="5c86f-144">In the **setdesiredandqueryapp** folder, create a new package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="5c86f-145">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="5c86f-145">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="5c86f-146">Vid en kommandotolk i den **setdesiredandqueryapp** mapp, kör följande kommando för att installera den **azure iothub** paketet:</span><span class="sxs-lookup"><span data-stu-id="5c86f-146">At your command prompt in the **setdesiredandqueryapp** folder, run the following command to install the **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. <span data-ttu-id="5c86f-147">Med hjälp av en textredigerare, skapa en ny **SetDesiredAndQuery.js** filen i den **addtagsandqueryapp** mapp.</span><span class="sxs-lookup"><span data-stu-id="5c86f-147">Using a text editor, create a new **SetDesiredAndQuery.js** file in the **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="5c86f-148">Lägg till följande kod i den **SetDesiredAndQuery.js** filen och Ersätt den **{anslutningssträngen för iot-hubb}** med IoT-hubb anslutningssträngen som du kopierade när du skapade din hubb:</span><span class="sxs-lookup"><span data-stu-id="5c86f-148">Add the following code to the **SetDesiredAndQuery.js** file, and substitute the **{iot hub connection string}** placeholder with the IoT Hub connection string you copied when you created your hub:</span></span>
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    <span data-ttu-id="5c86f-149">Den **registret** objektet innehåller de metoder som krävs för att interagera med enheten twins från tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5c86f-149">The **Registry** object exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="5c86f-150">Föregående kod när den initieras den **registret** objekt, hämtar enheten dubbla för **myDeviceId**, och uppdaterar dess egenskaper med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="5c86f-150">The previous code, after it initializes the **Registry** object, retrieves the device twin for **myDeviceId**, and updates its desired properties with a new telemetry configuration object.</span></span> <span data-ttu-id="5c86f-151">Därefter anropar den **queryTwins** fungerar händelse 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="5c86f-151">After that, it calls the **queryTwins** function event 10 seconds.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="5c86f-152">Det här programmet frågar IoT-hubb var 10: e sekund som illustration.</span><span class="sxs-lookup"><span data-stu-id="5c86f-152">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="5c86f-153">Använda frågor för att generera användarinriktad rapporter över flera enheter och inte för att identifiera ändringar.</span><span class="sxs-lookup"><span data-stu-id="5c86f-153">Use queries to generate user-facing reports across many devices, and not to detect changes.</span></span> <span data-ttu-id="5c86f-154">Om din lösning kräver meddelanden i realtid av enhetshändelser, använder [dubbla meddelanden][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="5c86f-154">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
    > 
    ><span data-ttu-id="5c86f-155">.</span><span class="sxs-lookup"><span data-stu-id="5c86f-155">.</span></span>

1. <span data-ttu-id="5c86f-156">Lägg till följande kod behörigheten innan den `registry.getDeviceTwin()` anrop för att implementera den **queryTwins** funktionen:</span><span class="sxs-lookup"><span data-stu-id="5c86f-156">Add the following code right before the `registry.getDeviceTwin()` invocation to implement the **queryTwins** function:</span></span>
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    <span data-ttu-id="5c86f-157">Föregående kod frågorna enheten twins lagras i IoT-hubb och skriver ut önskad och rapporterade telemetri konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="5c86f-157">The previous code queries the device twins stored in the IoT hub and prints the desired and reported telemetry configurations.</span></span> <span data-ttu-id="5c86f-158">Referera till den [IoT-hubb frågespråket] [ lnk-query] att lära dig hur du skapar omfattande rapporter på alla enheter.</span><span class="sxs-lookup"><span data-stu-id="5c86f-158">Refer to the [IoT Hub query language][lnk-query] to learn how to generate rich reports across all your devices.</span></span>
2. <span data-ttu-id="5c86f-159">Med **SimulateDeviceConfiguration.js** körs, köra program med:</span><span class="sxs-lookup"><span data-stu-id="5c86f-159">With **SimulateDeviceConfiguration.js** running, run the application with:</span></span>
   
        node SetDesiredAndQuery.js 5m
   
    <span data-ttu-id="5c86f-160">Du bör se rapporterade konfigurationsändringen från **lyckade** till **väntande** till **lyckade** igen med den nya aktivt frekvens som skickar fem minuter i stället för 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="5c86f-160">You should see the reported configuration change from **Success** to **Pending** to **Success** again with the new active send frequency of five minutes instead of 24 hours.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="5c86f-161">Det finns en fördröjning på upp till en minut mellan enheten rapporten igen och frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="5c86f-161">There is a delay of up to a minute between the device report operation and the query result.</span></span> <span data-ttu-id="5c86f-162">Detta är att aktivera query-infrastruktur för att arbeta med mycket hög skala.</span><span class="sxs-lookup"><span data-stu-id="5c86f-162">This is to enable the query infrastructure to work at very high scale.</span></span> <span data-ttu-id="5c86f-163">Att hämta konsekvent vyer för en enskild enhet dubbel användning av **getDeviceTwin** metod i den **registret** klass.</span><span class="sxs-lookup"><span data-stu-id="5c86f-163">To retrieve consistent views of a single device twin use the **getDeviceTwin** method in the **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="5c86f-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c86f-164">Next steps</span></span>
<span data-ttu-id="5c86f-165">I kursen får du ange en önskad konfiguration som *önskade egenskaper* från en backend-app och skrev en simulerad enhetsapp för att identifiera den ändringen och simulera en flera steg uppdateringsprocess reporting statusen  *rapporterade egenskaper* till dubbla för enheten.</span><span class="sxs-lookup"><span data-stu-id="5c86f-165">In this tutorial, you set a desired configuration as *desired properties* from a back-end app, and wrote a simulated device app to detect that change and simulate a multi-step update process reporting its status as *reported properties* to the device twin.</span></span>

<span data-ttu-id="5c86f-166">Använd följande resurser för att lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="5c86f-166">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="5c86f-167">Skicka telemetri från enheter med den [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="5c86f-167">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="5c86f-168">schemalägga eller utföra åtgärder på stora mängder enheter finns i [schema och broadcast jobb] [ lnk-schedule-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="5c86f-168">schedule or perform operations on large sets of devices see the [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="5c86f-169">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app), med den [metoder från direkt] [ lnk-methods-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="5c86f-169">control devices interactively (such as turning on a fan from a user-controlled app), with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app

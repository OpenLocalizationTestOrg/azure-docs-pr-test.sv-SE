---
title: "Egenskaper för aaaUse Azure IoT Hub enhet dubbla (nod) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooconfigure enheter. Du kan använda hello Azure IoT SDK för Node.js tooimplement en simulerad enhetsapp och en service-app som ändrar en konfiguration för enheter med hjälp av en enhet dubbla."
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
ms.openlocfilehash: 7ebfe2dfa0876bf04fdbaceae55db76456523e8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices-node"></a><span data-ttu-id="a233e-104">Använd önskade egenskaper tooconfigure enheter (nod)</span><span class="sxs-lookup"><span data-stu-id="a233e-104">Use desired properties tooconfigure devices (Node)</span></span>
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

<span data-ttu-id="a233e-105">Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="a233e-105">At hello end of this tutorial, you will have two Node.js console apps:</span></span>

* <span data-ttu-id="a233e-106">**SimulateDeviceConfiguration.js**, en simulerad enhetsapp som väntar på en uppdatering för önskad konfiguration och rapporterar hello status för en simulerad konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="a233e-106">**SimulateDeviceConfiguration.js**, a simulated device app that waits for a desired configuration update and reports hello status of a simulated configuration update process.</span></span>
* <span data-ttu-id="a233e-107">**SetDesiredConfigurationAndQuery.js**, en Node.js backend-app, vilket anger hello önskad konfiguration på en enhet och frågor hello konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="a233e-107">**SetDesiredConfigurationAndQuery.js**, a Node.js back-end app, which sets hello desired configuration on a device and queries hello configuration update process.</span></span>

> [!NOTE]
> <span data-ttu-id="a233e-108">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="a233e-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="a233e-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="a233e-109">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="a233e-110">Node.js version 0.10.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="a233e-110">Node.js version 0.10.x or later.</span></span>
* <span data-ttu-id="a233e-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a233e-111">An active Azure account.</span></span> <span data-ttu-id="a233e-112">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="a233e-112">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="a233e-113">Om du har följt hello [Kom igång med enheten twins] [ lnk-twin-tutorial] kursen har du redan en IoT-hubb och en enhetsidentitet kallas **myDeviceId**, och du kan hoppa över toohello [ Skapa hello simulerade enhetsapp] [ lnk-how-to-configure-createapp] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a233e-113">If you followed hello [Get started with device twins][lnk-twin-tutorial] tutorial, you already have an IoT hub and a device identity called **myDeviceId**; and you can skip toohello [Create hello simulated device app][lnk-how-to-configure-createapp] section.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-simulated-device-app"></a><span data-ttu-id="a233e-114">Skapa hello simulerade enhetsapp</span><span class="sxs-lookup"><span data-stu-id="a233e-114">Create hello simulated device app</span></span>
<span data-ttu-id="a233e-115">I det här avsnittet skapar du en Node.js-konsolprogram som ansluter tooyour hubb som **myDeviceId**, väntar på en uppdatering för önskad konfiguration och rapporterar uppdateringar på hello simulerade konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="a233e-115">In this section, you create a Node.js console app that connects tooyour hub as **myDeviceId**, waits for a desired configuration update and then reports updates on hello simulated configuration update process.</span></span>

1. <span data-ttu-id="a233e-116">Skapa en ny tom mapp som kallas **simulatedeviceconfiguration**.</span><span class="sxs-lookup"><span data-stu-id="a233e-116">Create a new empty folder called **simulatedeviceconfiguration**.</span></span> <span data-ttu-id="a233e-117">I hello **simulatedeviceconfiguration** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="a233e-117">In hello **simulatedeviceconfiguration** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="a233e-118">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="a233e-118">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="a233e-119">Vid en kommandotolk i hello **simulatedeviceconfiguration** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet**, och **azure-iot-enhet – mqtt**paketet:</span><span class="sxs-lookup"><span data-stu-id="a233e-119">At your command prompt in hello **simulatedeviceconfiguration** folder, run hello following command tooinstall hello **azure-iot-device**, and **azure-iot-device-mqtt** package:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. <span data-ttu-id="a233e-120">Med hjälp av en textredigerare, skapa en ny **SimulateDeviceConfiguration.js** filen i hello **simulatedeviceconfiguration** mapp.</span><span class="sxs-lookup"><span data-stu-id="a233e-120">Using a text editor, create a new **SimulateDeviceConfiguration.js** file in hello **simulatedeviceconfiguration** folder.</span></span>
4. <span data-ttu-id="a233e-121">Lägg till följande kod toohello hello **SimulateDeviceConfiguration.js** filen och ersätta hello **{enheten anslutningssträngen}** med hello enheten anslutningssträngen som du kopierade när du Skapa hello **myDeviceId** enhets-ID:</span><span class="sxs-lookup"><span data-stu-id="a233e-121">Add hello following code toohello **SimulateDeviceConfiguration.js** file, and substitute hello **{device connection string}** placeholder with hello device connection string you copied when you created hello **myDeviceId** device identity:</span></span>
   
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
   
    <span data-ttu-id="a233e-122">Hej **klienten** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="a233e-122">hello **Client** object exposes all hello methods required toointeract with device twins from hello device.</span></span> <span data-ttu-id="a233e-123">Hej föregående kod när den initierar hello **klienten** objekt, hämtar hello enheten dubbla för **myDeviceId**, och bifogar en hanterare för hello uppdatering på Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a233e-123">hello previous code, after it initializes hello **Client** object, retrieves hello device twin for **myDeviceId**, and attaches a handler for hello update on desired properties.</span></span> <span data-ttu-id="a233e-124">hello hanterare verifierar att det är en verklig configuration ändringsbegäran genom att jämföra hello configIds sedan anropar en metod som startar hello konfigurationsändring.</span><span class="sxs-lookup"><span data-stu-id="a233e-124">hello handler verifies that there is an actual configuration change request by comparing hello configIds, then invokes a method that starts hello configuration change.</span></span>
   
    <span data-ttu-id="a233e-125">Observera att för hello dig ut av enkelhet hello föregående kod använder en hårdkodad standard för hello inledande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a233e-125">Note that for hello sake of simplicity, hello previous code uses a hard-coded default for hello inital configuration.</span></span> <span data-ttu-id="a233e-126">En verklig app skulle förmodligen att läsa in konfigurationen från en lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="a233e-126">A real app would probably load that configuration from a local storage.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="a233e-127">Önskade egenskapen Ändringshändelser släpps alltid en gång vid enhetsanslutning, se till att det finns en verklig ändring i hello toocheck önskade egenskaper innan du utför några åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a233e-127">Desired property change events are always emitted once at device connection, make sure toocheck that there is an actual change in hello desired properties before performing any action.</span></span>
   > 
   > 
5. <span data-ttu-id="a233e-128">Lägg till följande metoder innan hello hello `client.open()` anrop:</span><span class="sxs-lookup"><span data-stu-id="a233e-128">Add hello following methods before hello `client.open()` invocation:</span></span>
   
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
   
    <span data-ttu-id="a233e-129">Hej **initConfigChange** metoden uppdateringar rapporteras egenskaper på hello lokala dubbla enhetsobjekt med hello konfigurationsbegäran för uppdatering och anger hello status för**väntande**, och sedan uppdateringar hello enhet dubbla på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a233e-129">hello **initConfigChange** method updates reported properties on hello local device twin object with hello configuration update request and sets hello status too**Pending**, then updates hello device twin on hello service.</span></span> <span data-ttu-id="a233e-130">När en uppdatering hello enheten dubbla är den simulerar en tidskrävande process som avslutar hello körning av **completeConfigChange**.</span><span class="sxs-lookup"><span data-stu-id="a233e-130">After successfully updating hello device twin, it simulates a long running process that terminates in hello execution of **completeConfigChange**.</span></span> <span data-ttu-id="a233e-131">Den här metoden uppdateringar hello lokala enhet dubbla har rapporterats egenskaper som anger hello status för**lyckade** och ta bort hello **pendingConfig** objekt.</span><span class="sxs-lookup"><span data-stu-id="a233e-131">This method updates hello local device twin's reported properties setting hello status too**Success** and removing hello **pendingConfig** object.</span></span> <span data-ttu-id="a233e-132">Sedan uppdateras hello enheten dubbla på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a233e-132">It then updates hello device twin on hello service.</span></span>
   
    <span data-ttu-id="a233e-133">Observera toosave bandbredd som rapporteras egenskaper uppdateras genom att ange endast hello egenskaper toobe ändrade (med namnet **korrigering** i hello ovan kod), i stället för att ersätta hello hela dokumentet.</span><span class="sxs-lookup"><span data-stu-id="a233e-133">Note that, toosave bandwidth, reported properties are updated by specifying only hello properties toobe modified (named **patch** in hello above code), instead of replacing hello whole document.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a233e-134">Den här självstudiekursen simulerar inte någon funktion för samtidiga konfigurationsuppdateringar.</span><span class="sxs-lookup"><span data-stu-id="a233e-134">This tutorial does not simulate any behavior for concurrent configuration updates.</span></span> <span data-ttu-id="a233e-135">Vissa processer för uppdatering av konfiguration kan vara kan tooaccommodate ändringar av konfigurationen för målet medan hello uppdateringen körs, andra kan ha tooqueue dem och andra kan avvisa dem med ett feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="a233e-135">Some configuration update processes might be able tooaccommodate changes of target configuration while hello update is running, others might have tooqueue them, and others could reject them with an error condition.</span></span> <span data-ttu-id="a233e-136">Se till att tooconsider hello önskat beteende för din specifika konfigurationsprocessen och lägga till lämpliga hello-logik innan du påbörjar hello konfigurationsändring.</span><span class="sxs-lookup"><span data-stu-id="a233e-136">Make sure tooconsider hello desired behavior for your specific configuration process, and add hello appropriate logic before initiating hello configuration change.</span></span>
   > 
   > 
6. <span data-ttu-id="a233e-137">Kör hello enhetsapp:</span><span class="sxs-lookup"><span data-stu-id="a233e-137">Run hello device app:</span></span>
   
        node SimulateDeviceConfiguration.js
   
    <span data-ttu-id="a233e-138">Du bör se hello-meddelande `retrieved device twin`.</span><span class="sxs-lookup"><span data-stu-id="a233e-138">You should see hello message `retrieved device twin`.</span></span> <span data-ttu-id="a233e-139">Behåll hello appen körs.</span><span class="sxs-lookup"><span data-stu-id="a233e-139">Keep hello app running.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="a233e-140">Skapa hello service-appen</span><span class="sxs-lookup"><span data-stu-id="a233e-140">Create hello service app</span></span>
<span data-ttu-id="a233e-141">I det här avsnittet skapar du en Node.js-konsolprogram som uppdateringar hello *önskade egenskaper* på hello enheten dubbla som är associerade med **myDeviceId** med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="a233e-141">In this section, you will create a Node.js console app that updates hello *desired properties* on hello device twin associated with **myDeviceId** with a new telemetry configuration object.</span></span> <span data-ttu-id="a233e-142">Sedan frågar hello enheten twins lagras i hello IoT-hubb och visar hello skillnaden mellan hello önskad och rapporterade konfigurationer av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="a233e-142">It then queries hello device twins stored in hello IoT hub and shows hello difference between hello desired and reported configurations of hello device.</span></span>

1. <span data-ttu-id="a233e-143">Skapa en ny tom mapp som kallas **setdesiredandqueryapp**.</span><span class="sxs-lookup"><span data-stu-id="a233e-143">Create a new empty folder called **setdesiredandqueryapp**.</span></span> <span data-ttu-id="a233e-144">I hello **setdesiredandqueryapp** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="a233e-144">In hello **setdesiredandqueryapp** folder, create a new package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="a233e-145">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="a233e-145">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```
2. <span data-ttu-id="a233e-146">Vid en kommandotolk i hello **setdesiredandqueryapp** mapp, kör följande kommando tooinstall hello hello **azure iothub** paketet:</span><span class="sxs-lookup"><span data-stu-id="a233e-146">At your command prompt in hello **setdesiredandqueryapp** folder, run hello following command tooinstall hello **azure-iothub** package:</span></span>
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. <span data-ttu-id="a233e-147">Med hjälp av en textredigerare, skapa en ny **SetDesiredAndQuery.js** filen i hello **addtagsandqueryapp** mapp.</span><span class="sxs-lookup"><span data-stu-id="a233e-147">Using a text editor, create a new **SetDesiredAndQuery.js** file in hello **addtagsandqueryapp** folder.</span></span>
4. <span data-ttu-id="a233e-148">Lägg till följande kod toohello hello **SetDesiredAndQuery.js** filen och ersätta hello **{anslutningssträngen för iot-hubb}** med hello IoT-hubb anslutningssträngen som du kopierade när du skapade din hubb :</span><span class="sxs-lookup"><span data-stu-id="a233e-148">Add hello following code toohello **SetDesiredAndQuery.js** file, and substitute hello **{iot hub connection string}** placeholder with hello IoT Hub connection string you copied when you created your hub:</span></span>
   
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

    <span data-ttu-id="a233e-149">Hej **registret** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a233e-149">hello **Registry** object exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="a233e-150">Hej föregående kod när den initierar hello **registret** objekt, hämtar hello enheten dubbla för **myDeviceId**, och uppdaterar dess egenskaper med ett nytt telemetri configuration-objekt.</span><span class="sxs-lookup"><span data-stu-id="a233e-150">hello previous code, after it initializes hello **Registry** object, retrieves hello device twin for **myDeviceId**, and updates its desired properties with a new telemetry configuration object.</span></span> <span data-ttu-id="a233e-151">Därefter anropar hello **queryTwins** fungerar händelse 10 sekunder.</span><span class="sxs-lookup"><span data-stu-id="a233e-151">After that, it calls hello **queryTwins** function event 10 seconds.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a233e-152">Det här programmet frågar IoT-hubb var 10: e sekund som illustration.</span><span class="sxs-lookup"><span data-stu-id="a233e-152">This application queries IoT Hub every 10 seconds for illustrative purposes.</span></span> <span data-ttu-id="a233e-153">Använd frågar toogenerate användarinriktad rapporter över flera enheter, och inte toodetect ändringar.</span><span class="sxs-lookup"><span data-stu-id="a233e-153">Use queries toogenerate user-facing reports across many devices, and not toodetect changes.</span></span> <span data-ttu-id="a233e-154">Om din lösning kräver meddelanden i realtid av enhetshändelser, använder [dubbla meddelanden][lnk-twin-notifications].</span><span class="sxs-lookup"><span data-stu-id="a233e-154">If your solution requires real-time notifications of device events, use [twin notifications][lnk-twin-notifications].</span></span>
    > 
    ><span data-ttu-id="a233e-155">.</span><span class="sxs-lookup"><span data-stu-id="a233e-155">.</span></span>

1. <span data-ttu-id="a233e-156">Lägg till följande kod precis innan hello hello `registry.getDeviceTwin()` anrop tooimplement hello **queryTwins** funktionen:</span><span class="sxs-lookup"><span data-stu-id="a233e-156">Add hello following code right before hello `registry.getDeviceTwin()` invocation tooimplement hello **queryTwins** function:</span></span>
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
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
   
    <span data-ttu-id="a233e-157">hello föregående kod frågor hello enheten twins lagras i hello IoT-hubb och utskrifter hello önskad och rapporterade telemetri konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="a233e-157">hello previous code queries hello device twins stored in hello IoT hub and prints hello desired and reported telemetry configurations.</span></span> <span data-ttu-id="a233e-158">Se toohello [IoT-hubb frågespråket] [ lnk-query] toolearn hur toogenerate omfattande rapporter på alla enheter.</span><span class="sxs-lookup"><span data-stu-id="a233e-158">Refer toohello [IoT Hub query language][lnk-query] toolearn how toogenerate rich reports across all your devices.</span></span>
2. <span data-ttu-id="a233e-159">Med **SimulateDeviceConfiguration.js** körs, kör hello program med:</span><span class="sxs-lookup"><span data-stu-id="a233e-159">With **SimulateDeviceConfiguration.js** running, run hello application with:</span></span>
   
        node SetDesiredAndQuery.js 5m
   
    <span data-ttu-id="a233e-160">Du bör se hello rapporterade konfigurationsändring från **lyckade** för**väntande** för**lyckade** igen med hello ny aktiv frekvens som skickar fem minuter i stället för 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="a233e-160">You should see hello reported configuration change from **Success** too**Pending** too**Success** again with hello new active send frequency of five minutes instead of 24 hours.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="a233e-161">Det finns en fördröjning på upp tooa minut mellan hello enheten rapporten igen och hello frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="a233e-161">There is a delay of up tooa minute between hello device report operation and hello query result.</span></span> <span data-ttu-id="a233e-162">Detta är tooenable hello frågan infrastruktur toowork i hög skala.</span><span class="sxs-lookup"><span data-stu-id="a233e-162">This is tooenable hello query infrastructure toowork at very high scale.</span></span> <span data-ttu-id="a233e-163">tooretrieve konsekvent vyer för en enskild enhet dubbla använda hello **getDeviceTwin** metod i hello **registret** klass.</span><span class="sxs-lookup"><span data-stu-id="a233e-163">tooretrieve consistent views of a single device twin use hello **getDeviceTwin** method in hello **Registry** class.</span></span>
   > 
   > 

## <a name="next-steps"></a><span data-ttu-id="a233e-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a233e-164">Next steps</span></span>
<span data-ttu-id="a233e-165">I kursen får du ange en önskad konfiguration som *önskade egenskaper* från en backend-app och skrev en simulerad enhet app toodetect som ändras och simulera en flera steg uppdateringsprocess reporting statusen  *rapporterade egenskaper* toohello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="a233e-165">In this tutorial, you set a desired configuration as *desired properties* from a back-end app, and wrote a simulated device app toodetect that change and simulate a multi-step update process reporting its status as *reported properties* toohello device twin.</span></span>

<span data-ttu-id="a233e-166">Använd hello följande resurser toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="a233e-166">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="a233e-167">Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="a233e-167">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="a233e-168">schemalägga eller utföra åtgärder på stora mängder enheter finns hello [schema och broadcast jobb] [ lnk-schedule-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="a233e-168">schedule or perform operations on large sets of devices see hello [Schedule and broadcast jobs][lnk-schedule-jobs] tutorial.</span></span>
* <span data-ttu-id="a233e-169">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app), med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="a233e-169">control devices interactively (such as turning on a fan from a user-controlled app), with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

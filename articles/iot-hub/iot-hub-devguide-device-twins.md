---
title: aaaUnderstand Azure IoT Hub-enhet twins | Microsoft Docs
description: "Utvecklarhandbok - Använd enheten twins toosynchronize tillstånd och konfiguration av data mellan IoT-hubb och dina enheter"
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7dade18665108ed352ff3d18e864dc34f451bbf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a><span data-ttu-id="ae23d-103">Förstå och använda enheten twins i IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="ae23d-103">Understand and use device twins in IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="ae23d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="ae23d-104">Overview</span></span>
<span data-ttu-id="ae23d-105">*Enheten twins* är JSON-dokument som lagrar tillstånd enhetsinformation (metadata, konfigurationer och villkor).</span><span class="sxs-lookup"><span data-stu-id="ae23d-105">*Device twins* are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="ae23d-106">IoT-hubb kvarstår en enhet dubbla för varje enhet som du ansluter tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="ae23d-106">IoT Hub persists a device twin for each device that you connect tooIoT Hub.</span></span> <span data-ttu-id="ae23d-107">Den här artikeln beskrivs:</span><span class="sxs-lookup"><span data-stu-id="ae23d-107">This article describes:</span></span>

* <span data-ttu-id="ae23d-108">Hej strukturen för hello enheten dubbla: *taggar*, *önskade* och *rapporterade egenskaper*, och</span><span class="sxs-lookup"><span data-stu-id="ae23d-108">hello structure of hello device twin: *tags*, *desired* and *reported properties*, and</span></span>
* <span data-ttu-id="ae23d-109">hello-åtgärder som appar för enheter och -servrar kan utföra på enheten twins.</span><span class="sxs-lookup"><span data-stu-id="ae23d-109">hello operations that device apps and back ends can perform on device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="ae23d-110">Enheten twins är för närvarande enbart tillgänglig från enheter som ansluter tooIoT hubb med hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="ae23d-110">Currently, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span> <span data-ttu-id="ae23d-111">Se toohello [MQTT stöd] [ lnk-devguide-mqtt] artikel anvisningar för hur tooconvert befintliga enheten app toouse MQTT.</span><span class="sxs-lookup"><span data-stu-id="ae23d-111">Refer toohello [MQTT support][lnk-devguide-mqtt] article for instructions on how tooconvert existing device app toouse MQTT.</span></span>
> 
> 

### <a name="when-toouse"></a><span data-ttu-id="ae23d-112">När toouse</span><span class="sxs-lookup"><span data-stu-id="ae23d-112">When toouse</span></span>
<span data-ttu-id="ae23d-113">Använd enhet twins till:</span><span class="sxs-lookup"><span data-stu-id="ae23d-113">Use device twins to:</span></span>

* <span data-ttu-id="ae23d-114">Lagra enhetsspecifika metadata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="ae23d-114">Store device-specific metadata in hello cloud.</span></span> <span data-ttu-id="ae23d-115">Till exempel hello distribution platsen för en Varuautomat.</span><span class="sxs-lookup"><span data-stu-id="ae23d-115">For example, hello deployment location of a vending machine.</span></span>
* <span data-ttu-id="ae23d-116">Rapportera aktuell statusinformation, till exempel tillgängliga funktioner och villkor från din enhet.</span><span class="sxs-lookup"><span data-stu-id="ae23d-116">Report current state information such as available capabilities and conditions from your device app.</span></span> <span data-ttu-id="ae23d-117">Till exempel en enhet är ansluten tooyour IoT-hubb över mobil- eller WiFi.</span><span class="sxs-lookup"><span data-stu-id="ae23d-117">For example, a device is connected tooyour IoT hub over cellular or WiFi.</span></span>
* <span data-ttu-id="ae23d-118">Synkronisera hello tillståndet för tidskrävande arbetsflöden mellan enhetsapp och backend-app.</span><span class="sxs-lookup"><span data-stu-id="ae23d-118">Synchronize hello state of long-running workflows between device app and back-end app.</span></span> <span data-ttu-id="ae23d-119">När hello lösning tillbaka anger slutet exempelvis hello ny inbyggd programvara version tooinstall och hello enheten apprapporter hello olika stegen i hello uppdateringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="ae23d-119">For example, when hello solution back end specifies hello new firmware version tooinstall, and hello device app reports hello various stages of hello update process.</span></span>
* <span data-ttu-id="ae23d-120">Fråga din enhetsmetadata, konfiguration eller tillstånd.</span><span class="sxs-lookup"><span data-stu-id="ae23d-120">Query your device metadata, configuration, or state.</span></span>

<span data-ttu-id="ae23d-121">Se för[enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] anvisningar om hur du använder rapporterade egenskaper, meddelanden från enhet till moln eller ladda upp filen.</span><span class="sxs-lookup"><span data-stu-id="ae23d-121">Refer too[Device-to-cloud communication guidance][lnk-d2c-guidance] for guidance on using reported properties, device-to-cloud messages, or file upload.</span></span>
<span data-ttu-id="ae23d-122">Se för[moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] anvisningar om hur du använder egenskaper, direkt metoder eller moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ae23d-122">Refer too[Cloud-to-device communication guidance][lnk-c2d-guidance] for guidance on using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="device-twins"></a><span data-ttu-id="ae23d-123">Enheten twins</span><span class="sxs-lookup"><span data-stu-id="ae23d-123">Device twins</span></span>
<span data-ttu-id="ae23d-124">Enheten twins lagra enhetsrelaterade information som:</span><span class="sxs-lookup"><span data-stu-id="ae23d-124">Device twins store device-related information that:</span></span>

* <span data-ttu-id="ae23d-125">Enhet och tillbaka slutar använda toosynchronize enheten villkor och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ae23d-125">Device and back ends can use toosynchronize device conditions and configuration.</span></span>
* <span data-ttu-id="ae23d-126">hello lösningens serverdel kan använda tooquery och rikta långvariga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="ae23d-126">hello solution back end can use tooquery and target long-running operations.</span></span>

<span data-ttu-id="ae23d-127">hello livscykeln för en enhet dubbla länkas toohello motsvarande [enhetsidentitet][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="ae23d-127">hello lifecycle of a device twin is linked toohello corresponding [device identity][lnk-identity].</span></span> <span data-ttu-id="ae23d-128">Enheten twins implicit skapas och tas bort när en ny enhetsidentitet skapas eller tas bort i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ae23d-128">Device twins are implicitly created and deleted when a new device identity is created or deleted in IoT Hub.</span></span>

<span data-ttu-id="ae23d-129">En enhet dubbla är en JSON-dokument som innehåller:</span><span class="sxs-lookup"><span data-stu-id="ae23d-129">A device twin is a JSON document that includes:</span></span>

* <span data-ttu-id="ae23d-130">**Taggar**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-130">**Tags**.</span></span> <span data-ttu-id="ae23d-131">En del av hello JSON-dokument som hello lösningens serverdel kan läsa från och skriva till.</span><span class="sxs-lookup"><span data-stu-id="ae23d-131">A section of hello JSON document that hello solution back end can read from and write to.</span></span> <span data-ttu-id="ae23d-132">Taggar är inte synliga toodevice appar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-132">Tags are not visible toodevice apps.</span></span>
* <span data-ttu-id="ae23d-133">**Egenskaper för Desired**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-133">**Desired properties**.</span></span> <span data-ttu-id="ae23d-134">Används tillsammans med rapporterade egenskaper toosynchronize enhetskonfigurationen eller villkor.</span><span class="sxs-lookup"><span data-stu-id="ae23d-134">Used along with reported properties toosynchronize device configuration or conditions.</span></span> <span data-ttu-id="ae23d-135">Egenskaper kan endast anges av hello lösning tillbaka slutet och kan läsas av hello enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="ae23d-135">Desired properties can only be set by hello solution back end and can be read by hello device app.</span></span> <span data-ttu-id="ae23d-136">Hej enhetsapp kan också få meddelanden i realtid om ändringar i hello önskade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ae23d-136">hello device app can also be notified in real time of changes in hello desired properties.</span></span>
* <span data-ttu-id="ae23d-137">**Rapporterade egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-137">**Reported properties**.</span></span> <span data-ttu-id="ae23d-138">Används tillsammans med egenskaper toosynchronize enhetskonfigurationen eller villkor.</span><span class="sxs-lookup"><span data-stu-id="ae23d-138">Used along with desired properties toosynchronize device configuration or conditions.</span></span> <span data-ttu-id="ae23d-139">Rapporterat egenskaper kan kan endast anges av hello enhetsapp och läsa och efterfrågas av hello lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="ae23d-139">Reported properties can only be set by hello device app and can be read and queried by hello solution back end.</span></span>

<span data-ttu-id="ae23d-140">Dessutom hello roten för hello enheten dubbla JSON-dokumentet innehåller hello skrivskyddade egenskaper från hello motsvarande enhetsidentitet lagras i hello [identitetsregistret][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="ae23d-140">Additionally, hello root of hello device twin JSON document contains hello read-only properties from hello corresponding device identity stored in hello [identity registry][lnk-identity].</span></span>

![][img-twin]

<span data-ttu-id="ae23d-141">hello som följande exempel visar en enhet dubbla JSON-dokumentet:</span><span class="sxs-lookup"><span data-stu-id="ae23d-141">hello following example shows a device twin JSON document:</span></span>

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

<span data-ttu-id="ae23d-142">I hello rotobjektet hello Systemegenskaper och behållarobjekt för `tags` och båda `reported` och `desired` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ae23d-142">In hello root object, are hello system properties, and container objects for `tags` and both `reported` and `desired` properties.</span></span> <span data-ttu-id="ae23d-143">Hej `properties` behållaren innehåller vissa skrivskyddad element (`$metadata`, `$etag`, och `$version`) beskrivs i hello [enhetens dubbla metadata] [ lnk-twin-metadata] och [ Optimistisk samtidighet] [ lnk-concurrency] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ae23d-143">hello `properties` container contains some read-only elements (`$metadata`, `$etag`, and `$version`) described in hello [Device twin metadata][lnk-twin-metadata] and [Optimistic concurrency][lnk-concurrency] sections.</span></span>

### <a name="reported-property-example"></a><span data-ttu-id="ae23d-144">Rapporterat egenskapen exempel</span><span class="sxs-lookup"><span data-stu-id="ae23d-144">Reported property example</span></span>
<span data-ttu-id="ae23d-145">I föregående exempel hello hello enheten dubbla innehåller en `batteryLevel` egenskap som rapporteras av hello enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="ae23d-145">In hello previous example, hello device twin contains a `batteryLevel` property that is reported by hello device app.</span></span> <span data-ttu-id="ae23d-146">Den här egenskapen gör det möjligt tooquery och fungerar på enheter utifrån hello senaste rapporterade nivå.</span><span class="sxs-lookup"><span data-stu-id="ae23d-146">This property makes it possible tooquery and operate on devices based on hello last reported battery level.</span></span> <span data-ttu-id="ae23d-147">Andra exempel är hello app reporting enheten enhetsfunktioner eller alternativ för nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="ae23d-147">Other examples include hello device app reporting device capabilities or connectivity options.</span></span>

> [!NOTE]
> <span data-ttu-id="ae23d-148">Rapporterat egenskaper förenkla scenarier där hello lösningens serverdel är intresserad av hello senast kända värdet för en egenskap.</span><span class="sxs-lookup"><span data-stu-id="ae23d-148">Reported properties simplify scenarios where hello solution back end is interested in hello last known value of a property.</span></span> <span data-ttu-id="ae23d-149">Använd [meddelanden från enhet till moln] [ lnk-d2c] om hello lösningens serverdel måste tooprocess enhetstelemetrin i hello form av tidsstämplad händelser, till exempel tidsserier.</span><span class="sxs-lookup"><span data-stu-id="ae23d-149">Use [device-to-cloud messages][lnk-d2c] if hello solution back end needs tooprocess device telemetry in hello form of sequences of timestamped events, such as time series.</span></span>

### <a name="desired-property-example"></a><span data-ttu-id="ae23d-150">Exempel på önskade egenskapen</span><span class="sxs-lookup"><span data-stu-id="ae23d-150">Desired property example</span></span>
<span data-ttu-id="ae23d-151">I föregående exempel hello hello `telemetryConfig` enheten dubbla önskad och rapporterade egenskaper används av hello lösningens serverdel och hello app toosynchronize hello telemetri enhetskonfiguration för den här enheten.</span><span class="sxs-lookup"><span data-stu-id="ae23d-151">In hello previous example, hello `telemetryConfig` device twin desired and reported properties are used by hello solution back end and hello device app toosynchronize hello telemetry configuration for this device.</span></span> <span data-ttu-id="ae23d-152">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ae23d-152">For example:</span></span>

1. <span data-ttu-id="ae23d-153">hello lösningens serverdel egenskapen hello önskad med hello desired configuration värde.</span><span class="sxs-lookup"><span data-stu-id="ae23d-153">hello solution back end sets hello desired property with hello desired configuration value.</span></span> <span data-ttu-id="ae23d-154">Här är hello-delen av hello dokument med hello önskad egenskapsuppsättning:</span><span class="sxs-lookup"><span data-stu-id="ae23d-154">Here is hello portion of hello document with hello desired property set:</span></span>
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. <span data-ttu-id="ae23d-155">Hej enhetsapp meddelas hello ändra omedelbart om ansluten eller vid hello först återansluta.</span><span class="sxs-lookup"><span data-stu-id="ae23d-155">hello device app is notified of hello change immediately if connected, or at hello first reconnect.</span></span> <span data-ttu-id="ae23d-156">Hej enhetsapp rapporterar hello uppdateras konfigurationen (eller ett feltillstånd med hello `status` egenskap).</span><span class="sxs-lookup"><span data-stu-id="ae23d-156">hello device app then reports hello updated configuration (or an error condition using hello `status` property).</span></span> <span data-ttu-id="ae23d-157">Här är hello-delen av hello rapporterade egenskaper:</span><span class="sxs-lookup"><span data-stu-id="ae23d-157">Here is hello portion of hello reported properties:</span></span>
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. <span data-ttu-id="ae23d-158">hello lösningens serverdel kan spåra hello resultaten av hello konfigurationsbegäran mellan många olika enheter, av [frågar] [ lnk-query] twins för enheten.</span><span class="sxs-lookup"><span data-stu-id="ae23d-158">hello solution back end can track hello results of hello configuration operation across many devices, by [querying][lnk-query] device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="ae23d-159">hello föregående kodfragment är exempel, optimerade för läsbarhet enkelriktade tooencode en enhetskonfiguration och dess status.</span><span class="sxs-lookup"><span data-stu-id="ae23d-159">hello preceding snippets are examples, optimized for readability, of one way tooencode a device configuration and its status.</span></span> <span data-ttu-id="ae23d-160">IoT-hubb medför inte ett visst schema för hello enheten dubbla önskad och rapporterade egenskaper i hello enheten twins.</span><span class="sxs-lookup"><span data-stu-id="ae23d-160">IoT Hub does not impose a specific schema for hello device twin desired and reported properties in hello device twins.</span></span>
> 
> 

<span data-ttu-id="ae23d-161">Du kan använda twins toosynchronize långvariga åtgärder, t.ex uppdateringar av inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="ae23d-161">You can use twins toosynchronize long-running operations such as firmware updates.</span></span> <span data-ttu-id="ae23d-162">Mer information om hur toouse egenskaper toosynchronize och spåra långvarig åtgärd på enheter, se [Använd önskad egenskaper tooconfigure enheter][lnk-twin-properties].</span><span class="sxs-lookup"><span data-stu-id="ae23d-162">For more information on how toouse properties toosynchronize and track a long running operation across devices, see [Use desired properties tooconfigure devices][lnk-twin-properties].</span></span>

## <a name="back-end-operations"></a><span data-ttu-id="ae23d-163">Backend-åtgärder</span><span class="sxs-lookup"><span data-stu-id="ae23d-163">Back-end operations</span></span>
<span data-ttu-id="ae23d-164">hello lösningens serverdel fungerar på hello enheten dubbla med hello följande atomiska åtgärder som exponeras via HTTP:</span><span class="sxs-lookup"><span data-stu-id="ae23d-164">hello solution back end operates on hello device twin using hello following atomic operations, exposed through HTTP:</span></span>

1. <span data-ttu-id="ae23d-165">**Hämta enheten dubbla med id**. Den här åtgärden returnerar hello enheten dubbla dokument, inklusive taggar och önskas, rapporteras och Systemegenskaper.</span><span class="sxs-lookup"><span data-stu-id="ae23d-165">**Retrieve device twin by id**. This operation returns hello device twin document, including tags and desired, reported, and system properties.</span></span>
2. <span data-ttu-id="ae23d-166">**Delvis uppdatera enheten dubbla**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-166">**Partially update device twin**.</span></span> <span data-ttu-id="ae23d-167">Den här åtgärden kan hello taggar för hello lösning serverdel toopartially eller önskade egenskaper i en delad enhet.</span><span class="sxs-lookup"><span data-stu-id="ae23d-167">This operation enables hello solution back end toopartially update hello tags or desired properties in a device twin.</span></span> <span data-ttu-id="ae23d-168">hello deluppdatering uttrycks som ett JSON-dokument som läggs till eller uppdaterar en egenskap.</span><span class="sxs-lookup"><span data-stu-id="ae23d-168">hello partial update is expressed as a JSON document that adds or updates any property.</span></span> <span data-ttu-id="ae23d-169">Egenskaper som angetts för`null` tas bort.</span><span class="sxs-lookup"><span data-stu-id="ae23d-169">Properties set too`null` are removed.</span></span> <span data-ttu-id="ae23d-170">hello följande exempel skapas en ny önskad egenskap med värdet `{"newProperty": "newValue"}`, skriver över befintliga hello-värdet för `existingProperty` med `"otherNewValue"`, och tar bort `otherOldProperty`.</span><span class="sxs-lookup"><span data-stu-id="ae23d-170">hello following example creates a new desired property with value `{"newProperty": "newValue"}`, overwrites hello existing value of `existingProperty` with `"otherNewValue"`, and removes `otherOldProperty`.</span></span> <span data-ttu-id="ae23d-171">Några andra ändringar görs tooexisting önskad egenskaperna eller taggarna:</span><span class="sxs-lookup"><span data-stu-id="ae23d-171">No other changes are made tooexisting desired properties or tags:</span></span>
   
        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }
3. <span data-ttu-id="ae23d-172">**Ersätt egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-172">**Replace desired properties**.</span></span> <span data-ttu-id="ae23d-173">Den här åtgärden aktiverar hello lösning serverdel toocompletely skriva över alla befintliga önskade egenskaper och ersätta ett nytt JSON-dokument för `properties/desired`.</span><span class="sxs-lookup"><span data-stu-id="ae23d-173">This operation enables hello solution back end toocompletely overwrite all existing desired properties and substitute a new JSON document for `properties/desired`.</span></span>
4. <span data-ttu-id="ae23d-174">**Ersätt taggar**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-174">**Replace tags**.</span></span> <span data-ttu-id="ae23d-175">Den här åtgärden aktiverar hello lösning serverdel toocompletely skriva över alla befintliga taggar och ersätta ett nytt JSON-dokument för `tags`.</span><span class="sxs-lookup"><span data-stu-id="ae23d-175">This operation enables hello solution back end toocompletely overwrite all existing tags and substitute a new JSON document for `tags`.</span></span>
5. <span data-ttu-id="ae23d-176">**Ta emot meddelanden med dubbla**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-176">**Receive twin notifications**.</span></span> <span data-ttu-id="ae23d-177">Den här åtgärden tillåter hello lösning serverdel toobe meddelad när hello dubbla ändras.</span><span class="sxs-lookup"><span data-stu-id="ae23d-177">This operation allows hello solution back end toobe notified when hello twin is modified.</span></span> <span data-ttu-id="ae23d-178">toodo så IoT-lösningen behöver toocreate en väg och tooset hello datakällan lika för*twinChangeEvents*.</span><span class="sxs-lookup"><span data-stu-id="ae23d-178">toodo so, your IoT solution needs toocreate a route and tooset hello Data Source equal too*twinChangeEvents*.</span></span> <span data-ttu-id="ae23d-179">Inga dubbla meddelanden skickas som standard, som är det inför finns ingen sådan vägar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-179">By default, no twin notifications are sent, that is, no such routes pre-exist.</span></span> <span data-ttu-id="ae23d-180">Om ändringshastigheten hello är för hög eller av andra orsaker, till exempel internt fel hello IoT-hubb kan skicka endast ett meddelande som innehåller alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-180">If hello rate of change is too high, or for other reasons, such as internal failures, hello IoT Hub might send only one notification that contains all changes.</span></span> <span data-ttu-id="ae23d-181">Så om ditt program måste tillförlitliga granskning och loggning av alla mellanliggande tillstånd, sedan fortfarande rekommenderas att du använder D2C meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ae23d-181">So, if your application needs reliable auditing and logging of all intermediate states, then it is still recommended that you use D2C messages.</span></span> <span data-ttu-id="ae23d-182">hello dubbla meddelandet innehåller egenskaperna och innehållet.</span><span class="sxs-lookup"><span data-stu-id="ae23d-182">hello twin notification message includes properties, and body.</span></span>

    - <span data-ttu-id="ae23d-183">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="ae23d-183">Properties</span></span>

    | <span data-ttu-id="ae23d-184">Namn</span><span class="sxs-lookup"><span data-stu-id="ae23d-184">Name</span></span> | <span data-ttu-id="ae23d-185">Värde</span><span class="sxs-lookup"><span data-stu-id="ae23d-185">Value</span></span> |
    | --- | --- |
    <span data-ttu-id="ae23d-186">$content-typ</span><span class="sxs-lookup"><span data-stu-id="ae23d-186">$content-type</span></span> | <span data-ttu-id="ae23d-187">application/json</span><span class="sxs-lookup"><span data-stu-id="ae23d-187">application/json</span></span> |
    <span data-ttu-id="ae23d-188">$iothub-enqueuedtime</span><span class="sxs-lookup"><span data-stu-id="ae23d-188">$iothub-enqueuedtime</span></span> |  <span data-ttu-id="ae23d-189">Tid när hello-meddelande skickades</span><span class="sxs-lookup"><span data-stu-id="ae23d-189">Time when hello notification was sent</span></span> |
    <span data-ttu-id="ae23d-190">$iothub-meddelande-källa</span><span class="sxs-lookup"><span data-stu-id="ae23d-190">$iothub-message-source</span></span> | <span data-ttu-id="ae23d-191">twinChangeEvents</span><span class="sxs-lookup"><span data-stu-id="ae23d-191">twinChangeEvents</span></span> |
    <span data-ttu-id="ae23d-192">$content-kodning</span><span class="sxs-lookup"><span data-stu-id="ae23d-192">$content-encoding</span></span> | <span data-ttu-id="ae23d-193">UTF-8</span><span class="sxs-lookup"><span data-stu-id="ae23d-193">utf-8</span></span> |
    <span data-ttu-id="ae23d-194">deviceId</span><span class="sxs-lookup"><span data-stu-id="ae23d-194">deviceId</span></span> | <span data-ttu-id="ae23d-195">ID för hello-enhet</span><span class="sxs-lookup"><span data-stu-id="ae23d-195">Id of hello device</span></span> |
    <span data-ttu-id="ae23d-196">hubName</span><span class="sxs-lookup"><span data-stu-id="ae23d-196">hubName</span></span> | <span data-ttu-id="ae23d-197">Namnet på IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="ae23d-197">Name of IoT Hub</span></span> |
    <span data-ttu-id="ae23d-198">operationTimestamp</span><span class="sxs-lookup"><span data-stu-id="ae23d-198">operationTimestamp</span></span> | <span data-ttu-id="ae23d-199">[ISO8601] tidsstämpeln för åtgärden</span><span class="sxs-lookup"><span data-stu-id="ae23d-199">[ISO8601] timestamp of operation</span></span> |
    <span data-ttu-id="ae23d-200">iothub-meddelande-schema</span><span class="sxs-lookup"><span data-stu-id="ae23d-200">iothub-message-schema</span></span> | <span data-ttu-id="ae23d-201">deviceLifecycleNotification</span><span class="sxs-lookup"><span data-stu-id="ae23d-201">deviceLifecycleNotification</span></span> |
    <span data-ttu-id="ae23d-202">opType</span><span class="sxs-lookup"><span data-stu-id="ae23d-202">opType</span></span> | <span data-ttu-id="ae23d-203">”replaceTwin” eller ”updateTwin”</span><span class="sxs-lookup"><span data-stu-id="ae23d-203">"replaceTwin" or "updateTwin"</span></span> |

    <span data-ttu-id="ae23d-204">Meddelandet Systemegenskaper föregås hello `'$'` symbolen.</span><span class="sxs-lookup"><span data-stu-id="ae23d-204">Message system properties are prefixed with hello `'$'` symbol.</span></span>

    - <span data-ttu-id="ae23d-205">Innehåll</span><span class="sxs-lookup"><span data-stu-id="ae23d-205">Body</span></span>
        
    <span data-ttu-id="ae23d-206">Det här avsnittet innehåller alla hello dubbla ändringar i en JSON-format.</span><span class="sxs-lookup"><span data-stu-id="ae23d-206">This section includes all hello twin changes in a JSON format.</span></span> <span data-ttu-id="ae23d-207">Hello samma format används som en korrigering, med skillnaden hello som den kan innehålla alla dubbla avsnitt: taggar, properties.reported, properties.desired och att den innehåller hello ”$metadata” element.</span><span class="sxs-lookup"><span data-stu-id="ae23d-207">It uses hello same format as a patch, with hello difference that it can contain all twin sections: tags, properties.reported, properties.desired, and that it contains hello “$metadata” elements.</span></span> <span data-ttu-id="ae23d-208">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ae23d-208">For example,</span></span>
    ```
    {
        "properties": {
            "desired": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            },
            "reported": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            }
        }
    }
    ``` 

<span data-ttu-id="ae23d-209">Alla hello stöd för föregående [Optimistisk samtidighet] [ lnk-concurrency] och kräver hello **ServiceConnect** behörighet, som definierats i hello [säkerhet ] [ lnk-security] artikel.</span><span class="sxs-lookup"><span data-stu-id="ae23d-209">All hello preceding operations support [Optimistic concurrency][lnk-concurrency] and require hello **ServiceConnect** permission, as defined in hello [Security][lnk-security] article.</span></span>

<span data-ttu-id="ae23d-210">Dessutom tillbaka toothese åtgärder, hello lösning slutet kan:</span><span class="sxs-lookup"><span data-stu-id="ae23d-210">In addition toothese operations, hello solution back end can:</span></span>

* <span data-ttu-id="ae23d-211">Fråga hello enheten twins med hello SQL-liknande [IoT-hubb frågespråket][lnk-query].</span><span class="sxs-lookup"><span data-stu-id="ae23d-211">Query hello device twins using hello SQL-like [IoT Hub query language][lnk-query].</span></span>
* <span data-ttu-id="ae23d-212">Utföra åtgärder på stora mängder enheten twins med [jobb][lnk-jobs].</span><span class="sxs-lookup"><span data-stu-id="ae23d-212">Perform operations on large sets of device twins using [jobs][lnk-jobs].</span></span>

## <a name="device-operations"></a><span data-ttu-id="ae23d-213">Åtgärder för enhet</span><span class="sxs-lookup"><span data-stu-id="ae23d-213">Device operations</span></span>
<span data-ttu-id="ae23d-214">hello enhetsapp fungerar på hello enheten dubbla med hello följande atomiska åtgärder:</span><span class="sxs-lookup"><span data-stu-id="ae23d-214">hello device app operates on hello device twin using hello following atomic operations:</span></span>

1. <span data-ttu-id="ae23d-215">**Hämta enheten dubbla**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-215">**Retrieve device twin**.</span></span> <span data-ttu-id="ae23d-216">Den här åtgärden returnerar hello enheten dubbla dokumentet (inklusive taggar och önskas, rapporteras och Systemegenskaper) för hello ansluten enhet.</span><span class="sxs-lookup"><span data-stu-id="ae23d-216">This operation returns hello device twin document (including tags and desired, reported and system properties) for hello currently connected device.</span></span>
2. <span data-ttu-id="ae23d-217">**Uppdatera delvis rapporterade egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-217">**Partially update reported properties**.</span></span> <span data-ttu-id="ae23d-218">Den här åtgärden aktiverar hello deluppdatering av hello rapporterade hello anslutna enhetens egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ae23d-218">This operation enables hello partial update of hello reported properties of hello currently connected device.</span></span> <span data-ttu-id="ae23d-219">Den här åtgärden använder hello uppdatera samma JSON-format som hello lösning tillbaka användningsområden för en deluppdatering av egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ae23d-219">This operation uses hello same JSON update format that hello solution back end uses for a partial update of desired properties.</span></span>
3. <span data-ttu-id="ae23d-220">**Se egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="ae23d-220">**Observe desired properties**.</span></span> <span data-ttu-id="ae23d-221">hello anslutna enheter kan välja toobe meddelanden om uppdateringar toohello önskade egenskaper innan de inträffar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-221">hello currently connected device can choose toobe notified of updates toohello desired properties when they happen.</span></span> <span data-ttu-id="ae23d-222">hello enheten tar emot hello samma form av uppdatering (eller delvis ersättning) som körs av hello lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="ae23d-222">hello device receives hello same form of update (partial or full replacement) executed by hello solution back end.</span></span>

<span data-ttu-id="ae23d-223">Alla hello föregående operationer kräver hello **DeviceConnect** behörighet, som definierats i hello [säkerhet] [ lnk-security] artikel.</span><span class="sxs-lookup"><span data-stu-id="ae23d-223">All hello preceding operations require hello **DeviceConnect** permission, as defined in hello [Security][lnk-security] article.</span></span>

<span data-ttu-id="ae23d-224">Hej [Azure IoT-enhet SDK] [ lnk-sdks] gör det enkelt toouse hello föregående åtgärder från många språk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-224">hello [Azure IoT device SDKs][lnk-sdks] make it easy toouse hello preceding operations from many languages and platforms.</span></span> <span data-ttu-id="ae23d-225">Mer information om hello detaljer för IoT-hubb primitiver för synkronisering av egenskaper finns i [enheten återanslutning flödet][lnk-reconnection].</span><span class="sxs-lookup"><span data-stu-id="ae23d-225">More information on hello details of IoT Hub primitives for desired properties synchronization can be found in [Device reconnection flow][lnk-reconnection].</span></span>

> [!NOTE]
> <span data-ttu-id="ae23d-226">Enheten twins är för närvarande enbart tillgänglig från enheter som ansluter tooIoT hubb med hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="ae23d-226">Currently, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="ae23d-227">Referensinformation:</span><span class="sxs-lookup"><span data-stu-id="ae23d-227">Reference topics:</span></span>
<span data-ttu-id="ae23d-228">hello ger följande Referensinformation dig mer om styra åtkomst tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ae23d-228">hello following reference topics provide you with more information about controlling access tooyour IoT hub.</span></span>

## <a name="tags-and-properties-format"></a><span data-ttu-id="ae23d-229">Taggar och egenskaper format</span><span class="sxs-lookup"><span data-stu-id="ae23d-229">Tags and properties format</span></span>
<span data-ttu-id="ae23d-230">Taggar, önskade och rapporterade egenskaper är JSON-objekt med hello följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="ae23d-230">Tags, desired, and reported properties are JSON objects with hello following restrictions:</span></span>

* <span data-ttu-id="ae23d-231">Alla nycklar i JSON-objekt är skiftlägeskänsliga 64 byte UTF-8, UNICODE-strängar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-231">All keys in JSON objects are case-sensitive 64 bytes UTF-8 UNICODE strings.</span></span> <span data-ttu-id="ae23d-232">Tillåtna tecken undanta Unicode-kontrolltecken (segment C0 och C1) och `'.'`, `' '`, och `'$'`.</span><span class="sxs-lookup"><span data-stu-id="ae23d-232">Allowed characters exclude UNICODE control characters (segments C0 and C1), and `'.'`, `' '`, and `'$'`.</span></span>
* <span data-ttu-id="ae23d-233">Alla värden i JSON-objekt kan vara av följande typer av JSON hello: boolean, nummer, sträng, objekt.</span><span class="sxs-lookup"><span data-stu-id="ae23d-233">All values in JSON objects can be of hello following JSON types: boolean, number, string, object.</span></span> <span data-ttu-id="ae23d-234">Matriser är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="ae23d-234">Arrays are not allowed.</span></span>
* <span data-ttu-id="ae23d-235">JSON-objekt i taggar, önskade och rapporterade egenskaper kan ha högst 5.</span><span class="sxs-lookup"><span data-stu-id="ae23d-235">All JSON objects in tags, desired, and reported properties can have a maximum depth of 5.</span></span> <span data-ttu-id="ae23d-236">Exempelvis är hello följande objekt giltiga:</span><span class="sxs-lookup"><span data-stu-id="ae23d-236">For instance, hello following object is valid:</span></span>

        {
            ...
            "tags": {
                "one": {
                    "two": {
                        "three": {
                            "four": {
                                "five": {
                                    "property": "value"
                                }
                            }
                        }
                    }
                }
            },
            ...
        }

* <span data-ttu-id="ae23d-237">Alla strängvärden kan innehålla högst 512 byte i längd.</span><span class="sxs-lookup"><span data-stu-id="ae23d-237">All string values can be at most 512 bytes in length.</span></span>

## <a name="device-twin-size"></a><span data-ttu-id="ae23d-238">Enheten dubbla storlek</span><span class="sxs-lookup"><span data-stu-id="ae23d-238">Device twin size</span></span>
<span data-ttu-id="ae23d-239">IoT-hubb tillämpar en begränsning på 8KB storleken på hello värden för `tags`, `properties/desired`, och `properties/reported`, exklusive skrivskyddade element.</span><span class="sxs-lookup"><span data-stu-id="ae23d-239">IoT Hub enforces an 8KB size limitation on hello values of `tags`, `properties/desired`, and `properties/reported`, excluding read-only elements.</span></span>
<span data-ttu-id="ae23d-240">hello storlek beräknas genom att räkna alla tecken utom UNICODE styra tecken (segment C0 och C1) och blanksteg `' '` när den visas utanför en strängkonstant.</span><span class="sxs-lookup"><span data-stu-id="ae23d-240">hello size is computed by counting all characters excluding UNICODE control characters (segments C0 and C1) and space `' '` when it appears outside of a string constant.</span></span>
<span data-ttu-id="ae23d-241">IoT-hubb avvisar alla åtgärder som skulle öka hello storleken på dessa dokument över hello gräns med ett fel.</span><span class="sxs-lookup"><span data-stu-id="ae23d-241">IoT Hub rejects with an error all operations that would increase hello size of those documents above hello limit.</span></span>

## <a name="device-twin-metadata"></a><span data-ttu-id="ae23d-242">Enhetens dubbla metadata</span><span class="sxs-lookup"><span data-stu-id="ae23d-242">Device twin metadata</span></span>
<span data-ttu-id="ae23d-243">IoT-hubb underhåller hello tidsstämpel hello senaste uppdateringen för varje JSON-objekt i enheten dubbla önskad och rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ae23d-243">IoT Hub maintains hello timestamp of hello last update for each JSON object in device twin desired and reported properties.</span></span> <span data-ttu-id="ae23d-244">hello tidsstämplar i UTC och kodats i hello [ISO8601] format `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span><span class="sxs-lookup"><span data-stu-id="ae23d-244">hello timestamps are in UTC and encoded in hello [ISO8601] format `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span></span>
<span data-ttu-id="ae23d-245">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ae23d-245">For example:</span></span>

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

<span data-ttu-id="ae23d-246">Den här informationen sparas på varje nivå (inte bara hello löv av hello JSON-strukturen) toopreserve uppdateringar som tar bort objektnycklar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-246">This information is kept at every level (not just hello leaves of hello JSON structure) toopreserve updates that remove object keys.</span></span>

## <a name="optimistic-concurrency"></a><span data-ttu-id="ae23d-247">Optimistisk samtidighet</span><span class="sxs-lookup"><span data-stu-id="ae23d-247">Optimistic concurrency</span></span>
<span data-ttu-id="ae23d-248">Taggar, önskad och rapporterade egenskaper alla stöd för Optimistisk samtidighet.</span><span class="sxs-lookup"><span data-stu-id="ae23d-248">Tags, desired, and reported properties all support optimistic concurrency.</span></span>
<span data-ttu-id="ae23d-249">Taggar har en ETag enligt [RFC7232], som representerar hello taggen JSON-representation.</span><span class="sxs-lookup"><span data-stu-id="ae23d-249">Tags have an ETag, as per [RFC7232], that represents hello tag's JSON representation.</span></span> <span data-ttu-id="ae23d-250">Du kan använda ETags i villkorlig uppdateringsåtgärder från hello lösning serverdel tooensure konsekvenskontroll.</span><span class="sxs-lookup"><span data-stu-id="ae23d-250">You can use ETags in conditional update operations from hello solution back end tooensure consistency.</span></span>

<span data-ttu-id="ae23d-251">Enheten dubbla önskad och rapporterade egenskaper har inte ETags, men har en `$version` värdet som garanterat toobe inkrementell.</span><span class="sxs-lookup"><span data-stu-id="ae23d-251">Device twin desired and reported properties do not have ETags, but have a `$version` value that is guaranteed toobe incremental.</span></span> <span data-ttu-id="ae23d-252">På liknande sätt kan tooan ETag hello version användas av hello uppdaterar part tooenforce konsekvenskontroll av uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-252">Similarly tooan ETag, hello version can be used by hello updating party tooenforce consistency of updates.</span></span> <span data-ttu-id="ae23d-253">Till exempel en enhetsapp för en rapporterade egenskap eller hello lösningens serverdel för en önskad egenskap.</span><span class="sxs-lookup"><span data-stu-id="ae23d-253">For example, a device app for a reported property or hello solution back end for a desired property.</span></span>

<span data-ttu-id="ae23d-254">Versioner är också användbart när en observing agent (till exempel hello enhetsapp sett hello önskade egenskaper) måste stämma lopp mellan hello resultatet av en hämtningen och ett uppdateringsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="ae23d-254">Versions are also useful when an observing agent (such as hello device app observing hello desired properties) must reconcile races between hello result of a retrieve operation and an update notification.</span></span> <span data-ttu-id="ae23d-255">Hej avsnittet [enheten återanslutning flödet] [ lnk-reconnection] innehåller mer information.</span><span class="sxs-lookup"><span data-stu-id="ae23d-255">hello section [Device reconnection flow][lnk-reconnection] provides more information.</span></span>

## <a name="device-reconnection-flow"></a><span data-ttu-id="ae23d-256">Enheten återanslutning flöde</span><span class="sxs-lookup"><span data-stu-id="ae23d-256">Device reconnection flow</span></span>
<span data-ttu-id="ae23d-257">IoT-hubb bevaras inte egenskaper uppdateringsmeddelanden för frånkopplade enheter.</span><span class="sxs-lookup"><span data-stu-id="ae23d-257">IoT Hub does not preserve desired properties update notifications for disconnected devices.</span></span> <span data-ttu-id="ae23d-258">Följer att en enhet som ansluter måste hämta hello fullständig egenskaper dokument i tillägget toosubscribing för meddelanden om uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-258">It follows that a device that is connecting must retrieve hello full desired properties document, in addition toosubscribing for update notifications.</span></span> <span data-ttu-id="ae23d-259">Angivna hello möjligheten att lopp mellan uppdateringsmeddelanden och fullständig hämtning säkerställas hello följande flödet:</span><span class="sxs-lookup"><span data-stu-id="ae23d-259">Given hello possibility of races between update notifications and full retrieval, hello following flow must be ensured:</span></span>

1. <span data-ttu-id="ae23d-260">Enhetsapp ansluter tooan IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ae23d-260">Device app connects tooan IoT hub.</span></span>
2. <span data-ttu-id="ae23d-261">Enhetsapp prenumererar för önskade egenskaper meddelanden om uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="ae23d-261">Device app subscribes for desired properties update notifications.</span></span>
3. <span data-ttu-id="ae23d-262">Enheten appen hämtar hello hela dokumentet för egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ae23d-262">Device app retrieves hello full document for desired properties.</span></span>

<span data-ttu-id="ae23d-263">Hej enhetsapp kan ignorera alla meddelanden med `$version` mindre än eller lika med hello version av hello fullständig hämtade dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ae23d-263">hello device app can ignore all notifications with `$version` less or equal than hello version of hello full retrieved document.</span></span> <span data-ttu-id="ae23d-264">Den här metoden är möjligt eftersom IoT-hubb garanterar att det alltid öka versioner.</span><span class="sxs-lookup"><span data-stu-id="ae23d-264">This approach is possible because IoT Hub guarantees that versions always increment.</span></span>

> [!NOTE]
> <span data-ttu-id="ae23d-265">Den här logiken har redan implementerats i hello [Azure IoT-enhet SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="ae23d-265">This logic is already implemented in hello [Azure IoT device SDKs][lnk-sdks].</span></span> <span data-ttu-id="ae23d-266">Den här beskrivningen är användbart om hello enheten appen kan inte använda någon av Azure IoT-enhet SDK: er och måste programmet hello MQTT gränssnittet direkt.</span><span class="sxs-lookup"><span data-stu-id="ae23d-266">This description is useful only if hello device app cannot use any of Azure IoT device SDKs and must program hello MQTT interface directly.</span></span>
> 
> 

## <a name="additional-reference-material"></a><span data-ttu-id="ae23d-267">Ytterligare referensmaterialet</span><span class="sxs-lookup"><span data-stu-id="ae23d-267">Additional reference material</span></span>
<span data-ttu-id="ae23d-268">Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:</span><span class="sxs-lookup"><span data-stu-id="ae23d-268">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="ae23d-269">Hej [IoT-hubbslutpunkter] [ lnk-endpoints] artikeln hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="ae23d-269">hello [IoT Hub endpoints][lnk-endpoints] article describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="ae23d-270">Hej [begränsning och kvoter] [ lnk-quotas] artikeln hello kvoter som gäller toohello IoT-hubb-tjänsten och hello bandbreddsbegränsning beteende tooexpect när du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ae23d-270">hello [Throttling and quotas][lnk-quotas] article describes hello quotas that apply toohello IoT Hub service and hello throttling behavior tooexpect when you use hello service.</span></span>
* <span data-ttu-id="ae23d-271">Hej [Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] artikeln visar hello olika språk SDK: du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="ae23d-271">hello [Azure IoT device and service SDKs][lnk-sdks] article lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="ae23d-272">Hej [IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning] [ lnk-query] artikeln hello IoT-hubb frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb .</span><span class="sxs-lookup"><span data-stu-id="ae23d-272">hello [IoT Hub query language for device twins, jobs, and message routing][lnk-query] article describes hello IoT Hub query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="ae23d-273">Hej [IoT-hubb MQTT stöd] [ lnk-devguide-mqtt] artikeln innehåller mer information om stöd för IoT-hubb för hello MQTT protokoll.</span><span class="sxs-lookup"><span data-stu-id="ae23d-273">hello [IoT Hub MQTT support][lnk-devguide-mqtt] article provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae23d-274">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae23d-274">Next steps</span></span>
<span data-ttu-id="ae23d-275">Nu du har lärt dig om enheten twins, kanske du är intresserad av i följande avsnitt i IoT-hubb developer hello:</span><span class="sxs-lookup"><span data-stu-id="ae23d-275">Now you have learned about device twins, you may be interested in hello following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="ae23d-276">[Anropa en metod som är direkt på en enhet][lnk-methods]</span><span class="sxs-lookup"><span data-stu-id="ae23d-276">[Invoke a direct method on a device][lnk-methods]</span></span>
* <span data-ttu-id="ae23d-277">[Schema-jobb på flera enheter][lnk-jobs]</span><span class="sxs-lookup"><span data-stu-id="ae23d-277">[Schedule jobs on multiple devices][lnk-jobs]</span></span>

<span data-ttu-id="ae23d-278">Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb Självstudier:</span><span class="sxs-lookup"><span data-stu-id="ae23d-278">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorials:</span></span>

* <span data-ttu-id="ae23d-279">[Hur toouse hello enheten dubbla][lnk-twin-tutorial]</span><span class="sxs-lookup"><span data-stu-id="ae23d-279">[How toouse hello device twin][lnk-twin-tutorial]</span></span>
* <span data-ttu-id="ae23d-280">[Hur toouse enheten dubbla egenskaper][lnk-twin-properties]</span><span class="sxs-lookup"><span data-stu-id="ae23d-280">[How toouse device twin properties][lnk-twin-properties]</span></span>

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#device-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png

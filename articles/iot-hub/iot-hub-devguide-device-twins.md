---
title: "Förstå Azure IoT Hub-enhet twins | Microsoft Docs"
description: "Utvecklarhandbok - Använd enhet twins att synkronisera och konfigurationsändringar data mellan IoT-hubb och dina enheter"
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
ms.openlocfilehash: b316aa419d558547f90a914a22fb29935076de21
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a><span data-ttu-id="3f8f4-103">Förstå och använda enheten twins i IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="3f8f4-103">Understand and use device twins in IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="3f8f4-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="3f8f4-104">Overview</span></span>
<span data-ttu-id="3f8f4-105">*Enheten twins* är JSON-dokument som lagrar tillstånd enhetsinformation (metadata, konfigurationer och villkor).</span><span class="sxs-lookup"><span data-stu-id="3f8f4-105">*Device twins* are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="3f8f4-106">IoT Hub lagrar en enhetstvilling för varje enhet som du ansluter till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-106">IoT Hub persists a device twin for each device that you connect to IoT Hub.</span></span> <span data-ttu-id="3f8f4-107">Den här artikeln beskrivs:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-107">This article describes:</span></span>

* <span data-ttu-id="3f8f4-108">Strukturen för enheten dubbla: *taggar*, *önskade* och *rapporterade egenskaper*, och</span><span class="sxs-lookup"><span data-stu-id="3f8f4-108">The structure of the device twin: *tags*, *desired* and *reported properties*, and</span></span>
* <span data-ttu-id="3f8f4-109">De åtgärder som appar för enheter och -servrar kan utföra på enheten twins.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-109">The operations that device apps and back ends can perform on device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="3f8f4-110">Enheten twins för närvarande enbart tillgänglig från enheter som ansluter till IoT-hubb med MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-110">Currently, device twins are accessible only from devices that connect to IoT Hub using the MQTT protocol.</span></span> <span data-ttu-id="3f8f4-111">Referera till den [MQTT stöd] [ lnk-devguide-mqtt] artikel för instruktioner om hur du konverterar en befintlig enhetsapp att använda MQTT.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-111">Refer to the [MQTT support][lnk-devguide-mqtt] article for instructions on how to convert existing device app to use MQTT.</span></span>
> 
> 

### <a name="when-to-use"></a><span data-ttu-id="3f8f4-112">När du ska använda detta</span><span class="sxs-lookup"><span data-stu-id="3f8f4-112">When to use</span></span>
<span data-ttu-id="3f8f4-113">Använd enhet twins till:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-113">Use device twins to:</span></span>

* <span data-ttu-id="3f8f4-114">Lagra enhetsspecifika metadata i molnet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-114">Store device-specific metadata in the cloud.</span></span> <span data-ttu-id="3f8f4-115">Till exempel distributionsplatsen för en Varuautomat.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-115">For example, the deployment location of a vending machine.</span></span>
* <span data-ttu-id="3f8f4-116">Rapportera aktuell statusinformation, till exempel tillgängliga funktioner och villkor från din enhet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-116">Report current state information such as available capabilities and conditions from your device app.</span></span> <span data-ttu-id="3f8f4-117">Till exempel en enhet är ansluten till din IoT-hubb över mobil- eller WiFi.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-117">For example, a device is connected to your IoT hub over cellular or WiFi.</span></span>
* <span data-ttu-id="3f8f4-118">Synkronisera tillståndet för tidskrävande arbetsflöden mellan enhetsapp och backend-app.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-118">Synchronize the state of long-running workflows between device app and back-end app.</span></span> <span data-ttu-id="3f8f4-119">Till exempel när lösningen tillbaka slutet anger den nya versionen på inbyggd programvara ska installeras och appen enheter rapporterar de olika stegen för uppdateringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-119">For example, when the solution back end specifies the new firmware version to install, and the device app reports the various stages of the update process.</span></span>
* <span data-ttu-id="3f8f4-120">Fråga din enhetsmetadata, konfiguration eller tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-120">Query your device metadata, configuration, or state.</span></span>

<span data-ttu-id="3f8f4-121">Referera till [enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] anvisningar om hur du använder rapporterade egenskaper, meddelanden från enhet till moln eller ladda upp filen.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-121">Refer to [Device-to-cloud communication guidance][lnk-d2c-guidance] for guidance on using reported properties, device-to-cloud messages, or file upload.</span></span>
<span data-ttu-id="3f8f4-122">Referera till [moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] anvisningar om hur du använder egenskaper, direkt metoder eller moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-122">Refer to [Cloud-to-device communication guidance][lnk-c2d-guidance] for guidance on using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="device-twins"></a><span data-ttu-id="3f8f4-123">Enheten twins</span><span class="sxs-lookup"><span data-stu-id="3f8f4-123">Device twins</span></span>
<span data-ttu-id="3f8f4-124">Enheten twins lagra enhetsrelaterade information som:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-124">Device twins store device-related information that:</span></span>

* <span data-ttu-id="3f8f4-125">Enhet och tillbaka slutar använda för att synkronisera enheten villkor och konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-125">Device and back ends can use to synchronize device conditions and configuration.</span></span>
* <span data-ttu-id="3f8f4-126">Lösningens serverdel kan använda för att fråga och mål långvariga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-126">The solution back end can use to query and target long-running operations.</span></span>

<span data-ttu-id="3f8f4-127">Livscykeln för en enhet dubbla är kopplad till motsvarande [enhetsidentitet][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="3f8f4-127">The lifecycle of a device twin is linked to the corresponding [device identity][lnk-identity].</span></span> <span data-ttu-id="3f8f4-128">Enheten twins implicit skapas och tas bort när en ny enhetsidentitet skapas eller tas bort i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-128">Device twins are implicitly created and deleted when a new device identity is created or deleted in IoT Hub.</span></span>

<span data-ttu-id="3f8f4-129">En enhet dubbla är en JSON-dokument som innehåller:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-129">A device twin is a JSON document that includes:</span></span>

* <span data-ttu-id="3f8f4-130">**Taggar**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-130">**Tags**.</span></span> <span data-ttu-id="3f8f4-131">En del av JSON-dokumentet som lösningens serverdel kan läsa från och skriva till.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-131">A section of the JSON document that the solution back end can read from and write to.</span></span> <span data-ttu-id="3f8f4-132">Taggar visas inte för appar för enheter.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-132">Tags are not visible to device apps.</span></span>
* <span data-ttu-id="3f8f4-133">**Egenskaper för Desired**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-133">**Desired properties**.</span></span> <span data-ttu-id="3f8f4-134">Används tillsammans med rapporterade egenskaper för att synkronisera enhetskonfigurationen eller villkor.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-134">Used along with reported properties to synchronize device configuration or conditions.</span></span> <span data-ttu-id="3f8f4-135">Egenskaper kan endast anges av lösningen tillbaka slutet och kan läsas av appen enhet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-135">Desired properties can only be set by the solution back end and can be read by the device app.</span></span> <span data-ttu-id="3f8f4-136">Appen enheten kan också få meddelanden i realtid om ändringar i egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-136">The device app can also be notified in real time of changes in the desired properties.</span></span>
* <span data-ttu-id="3f8f4-137">**Rapporterade egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-137">**Reported properties**.</span></span> <span data-ttu-id="3f8f4-138">Används tillsammans med egenskaper för att synkronisera enhetskonfigurationen eller villkor.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-138">Used along with desired properties to synchronize device configuration or conditions.</span></span> <span data-ttu-id="3f8f4-139">Rapporterat egenskaper kan kan endast ställas in via appen för enheter och läsa och efterfrågas av lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-139">Reported properties can only be set by the device app and can be read and queried by the solution back end.</span></span>

<span data-ttu-id="3f8f4-140">Dessutom roten på enhet dubbla JSON-dokumentet innehåller skrivskyddade egenskaper från motsvarande enhetens identitet lagras i den [identitetsregistret][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="3f8f4-140">Additionally, the root of the device twin JSON document contains the read-only properties from the corresponding device identity stored in the [identity registry][lnk-identity].</span></span>

![][img-twin]

<span data-ttu-id="3f8f4-141">I följande exempel visas en enhet dubbla JSON-dokumentet:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-141">The following example shows a device twin JSON document:</span></span>

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

<span data-ttu-id="3f8f4-142">I rotobjektet Systemegenskaper, och behållarobjekt för `tags` och båda `reported` och `desired` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-142">In the root object, are the system properties, and container objects for `tags` and both `reported` and `desired` properties.</span></span> <span data-ttu-id="3f8f4-143">Den `properties` behållaren innehåller vissa skrivskyddad element (`$metadata`, `$etag`, och `$version`) beskrivs i den [enhetens dubbla metadata] [ lnk-twin-metadata] och [Optimistisk samtidighet] [ lnk-concurrency] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-143">The `properties` container contains some read-only elements (`$metadata`, `$etag`, and `$version`) described in the [Device twin metadata][lnk-twin-metadata] and [Optimistic concurrency][lnk-concurrency] sections.</span></span>

### <a name="reported-property-example"></a><span data-ttu-id="3f8f4-144">Rapporterat egenskapen exempel</span><span class="sxs-lookup"><span data-stu-id="3f8f4-144">Reported property example</span></span>
<span data-ttu-id="3f8f4-145">I det förra exemplet, enhet dubbla innehåller en `batteryLevel` egenskap som rapporteras av appen enhet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-145">In the previous example, the device twin contains a `batteryLevel` property that is reported by the device app.</span></span> <span data-ttu-id="3f8f4-146">Den här egenskapen gör det möjligt att fråga efter och fungerar på enheter utifrån den senaste rapporterade batterinivån.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-146">This property makes it possible to query and operate on devices based on the last reported battery level.</span></span> <span data-ttu-id="3f8f4-147">Andra exempel är enheten app reporting enhetsfunktioner eller alternativ för nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-147">Other examples include the device app reporting device capabilities or connectivity options.</span></span>

> [!NOTE]
> <span data-ttu-id="3f8f4-148">Rapporterat egenskaper förenkla scenarier där lösningens serverdel är intresserad av det senaste kända värdet för en egenskap.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-148">Reported properties simplify scenarios where the solution back end is interested in the last known value of a property.</span></span> <span data-ttu-id="3f8f4-149">Använd [meddelanden från enhet till moln] [ lnk-d2c] om lösningens serverdel behöver för att bearbeta enhetstelemetrin i form av tidsstämplad händelser, till exempel tidsserier.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-149">Use [device-to-cloud messages][lnk-d2c] if the solution back end needs to process device telemetry in the form of sequences of timestamped events, such as time series.</span></span>

### <a name="desired-property-example"></a><span data-ttu-id="3f8f4-150">Exempel på önskade egenskapen</span><span class="sxs-lookup"><span data-stu-id="3f8f4-150">Desired property example</span></span>
<span data-ttu-id="3f8f4-151">I föregående exempel är den `telemetryConfig` enheten dubbla önskad och rapporterade egenskaper används av lösningens serverdel och enheter appen synkronisera telemetri konfigurationen för den här enheten.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-151">In the previous example, the `telemetryConfig` device twin desired and reported properties are used by the solution back end and the device app to synchronize the telemetry configuration for this device.</span></span> <span data-ttu-id="3f8f4-152">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-152">For example:</span></span>

1. <span data-ttu-id="3f8f4-153">Lösningens serverdel egenskapen önskade med önskad konfiguration-värde.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-153">The solution back end sets the desired property with the desired configuration value.</span></span> <span data-ttu-id="3f8f4-154">Här är del av dokumentet med de önskade egenskapen:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-154">Here is the portion of the document with the desired property set:</span></span>
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. <span data-ttu-id="3f8f4-155">Enheten appen meddelas ändra omedelbart om ansluten, eller vid första återanslutning.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-155">The device app is notified of the change immediately if connected, or at the first reconnect.</span></span> <span data-ttu-id="3f8f4-156">Appen enheten rapporterar den uppdaterade konfigurationen (eller ett fel villkor med hjälp av den `status` egenskap).</span><span class="sxs-lookup"><span data-stu-id="3f8f4-156">The device app then reports the updated configuration (or an error condition using the `status` property).</span></span> <span data-ttu-id="3f8f4-157">Här är del av rapporterade egenskaper:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-157">Here is the portion of the reported properties:</span></span>
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. <span data-ttu-id="3f8f4-158">Lösningens serverdel kan spåra resultatet av åtgärden configuration mellan många olika enheter, av [frågar] [ lnk-query] twins för enheten.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-158">The solution back end can track the results of the configuration operation across many devices, by [querying][lnk-query] device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="3f8f4-159">Föregående kodfragment är exempel, optimerade för läsbarhet på ett sätt att koda en enhetskonfiguration och dess status.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-159">The preceding snippets are examples, optimized for readability, of one way to encode a device configuration and its status.</span></span> <span data-ttu-id="3f8f4-160">IoT-hubb medför inte ett visst schema för enheten dubbla önskad och rapporterade egenskaper i twins för enheten.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-160">IoT Hub does not impose a specific schema for the device twin desired and reported properties in the device twins.</span></span>
> 
> 

<span data-ttu-id="3f8f4-161">Du kan använda twins för att synkronisera långvariga åtgärder, t.ex uppdateringar av inbyggd programvara.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-161">You can use twins to synchronize long-running operations such as firmware updates.</span></span> <span data-ttu-id="3f8f4-162">Mer information om hur du använder egenskaper för att synkronisera och spåra långvarig åtgärd mellan enheter finns [Använd önskad egenskaper att konfigurera enheter][lnk-twin-properties].</span><span class="sxs-lookup"><span data-stu-id="3f8f4-162">For more information on how to use properties to synchronize and track a long running operation across devices, see [Use desired properties to configure devices][lnk-twin-properties].</span></span>

## <a name="back-end-operations"></a><span data-ttu-id="3f8f4-163">Backend-åtgärder</span><span class="sxs-lookup"><span data-stu-id="3f8f4-163">Back-end operations</span></span>
<span data-ttu-id="3f8f4-164">Lösningens serverdel körs på den enheten dubbla med hjälp av följande atomiska åtgärder exponeras via HTTP:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-164">The solution back end operates on the device twin using the following atomic operations, exposed through HTTP:</span></span>

1. <span data-ttu-id="3f8f4-165">**Hämta enheten dubbla med id**. Den här åtgärden Returnerar enheten dubbla dokument, inklusive taggar och du kan rapporteras och Systemegenskaper.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-165">**Retrieve device twin by id**. This operation returns the device twin document, including tags and desired, reported, and system properties.</span></span>
2. <span data-ttu-id="3f8f4-166">**Delvis uppdatera enheten dubbla**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-166">**Partially update device twin**.</span></span> <span data-ttu-id="3f8f4-167">Den här åtgärden kan lösningens serverdel delvis uppdaterar taggar eller önskade egenskaper i en delad enhet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-167">This operation enables the solution back end to partially update the tags or desired properties in a device twin.</span></span> <span data-ttu-id="3f8f4-168">Delvis uppdatering uttrycks som ett JSON-dokument som läggs till eller uppdaterar en egenskap.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-168">The partial update is expressed as a JSON document that adds or updates any property.</span></span> <span data-ttu-id="3f8f4-169">Ange egenskaper `null` tas bort.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-169">Properties set to `null` are removed.</span></span> <span data-ttu-id="3f8f4-170">I följande exempel skapas en ny önskad egenskap med värdet `{"newProperty": "newValue"}`, skriver över det befintliga värdet av `existingProperty` med `"otherNewValue"`, och tar bort `otherOldProperty`.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-170">The following example creates a new desired property with value `{"newProperty": "newValue"}`, overwrites the existing value of `existingProperty` with `"otherNewValue"`, and removes `otherOldProperty`.</span></span> <span data-ttu-id="3f8f4-171">Några andra ändringar har gjorts i befintliga egenskaper eller taggar:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-171">No other changes are made to existing desired properties or tags:</span></span>
   
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
3. <span data-ttu-id="3f8f4-172">**Ersätt egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-172">**Replace desired properties**.</span></span> <span data-ttu-id="3f8f4-173">Den här åtgärden aktiverar lösningens serverdel att skriva över alla befintliga egenskaper och ersätta ett nytt JSON-dokument för `properties/desired`.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-173">This operation enables the solution back end to completely overwrite all existing desired properties and substitute a new JSON document for `properties/desired`.</span></span>
4. <span data-ttu-id="3f8f4-174">**Ersätt taggar**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-174">**Replace tags**.</span></span> <span data-ttu-id="3f8f4-175">Den här åtgärden aktiverar lösningens serverdel att skriva över alla befintliga taggar och ersätta ett nytt JSON-dokument för `tags`.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-175">This operation enables the solution back end to completely overwrite all existing tags and substitute a new JSON document for `tags`.</span></span>
5. <span data-ttu-id="3f8f4-176">**Ta emot meddelanden med dubbla**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-176">**Receive twin notifications**.</span></span> <span data-ttu-id="3f8f4-177">Den här åtgärden kan lösningens serverdel ska meddelas när dubbla ändras.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-177">This operation allows the solution back end to be notified when the twin is modified.</span></span> <span data-ttu-id="3f8f4-178">Om du vill göra det, IoT-lösningen behöver skapa en väg och datakällan ska vara lika med *twinChangeEvents*.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-178">To do so, your IoT solution needs to create a route and to set the Data Source equal to *twinChangeEvents*.</span></span> <span data-ttu-id="3f8f4-179">Inga dubbla meddelanden skickas som standard, som är det inför finns ingen sådan vägar.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-179">By default, no twin notifications are sent, that is, no such routes pre-exist.</span></span> <span data-ttu-id="3f8f4-180">Om ändringshastigheten är för hög eller av andra orsaker, till exempel internt fel IoT-hubben kan skicka endast ett meddelande som innehåller alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-180">If the rate of change is too high, or for other reasons, such as internal failures, the IoT Hub might send only one notification that contains all changes.</span></span> <span data-ttu-id="3f8f4-181">Så om ditt program måste tillförlitliga granskning och loggning av alla mellanliggande tillstånd, sedan fortfarande rekommenderas att du använder D2C meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-181">So, if your application needs reliable auditing and logging of all intermediate states, then it is still recommended that you use D2C messages.</span></span> <span data-ttu-id="3f8f4-182">Dubbla meddelandet innehåller egenskaperna och innehållet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-182">The twin notification message includes properties, and body.</span></span>

    - <span data-ttu-id="3f8f4-183">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="3f8f4-183">Properties</span></span>

    | <span data-ttu-id="3f8f4-184">Namn</span><span class="sxs-lookup"><span data-stu-id="3f8f4-184">Name</span></span> | <span data-ttu-id="3f8f4-185">Värde</span><span class="sxs-lookup"><span data-stu-id="3f8f4-185">Value</span></span> |
    | --- | --- |
    <span data-ttu-id="3f8f4-186">$content-typ</span><span class="sxs-lookup"><span data-stu-id="3f8f4-186">$content-type</span></span> | <span data-ttu-id="3f8f4-187">application/json</span><span class="sxs-lookup"><span data-stu-id="3f8f4-187">application/json</span></span> |
    <span data-ttu-id="3f8f4-188">$iothub-enqueuedtime</span><span class="sxs-lookup"><span data-stu-id="3f8f4-188">$iothub-enqueuedtime</span></span> |  <span data-ttu-id="3f8f4-189">Tidpunkt som meddelandet skickades</span><span class="sxs-lookup"><span data-stu-id="3f8f4-189">Time when the notification was sent</span></span> |
    <span data-ttu-id="3f8f4-190">$iothub-meddelande-källa</span><span class="sxs-lookup"><span data-stu-id="3f8f4-190">$iothub-message-source</span></span> | <span data-ttu-id="3f8f4-191">twinChangeEvents</span><span class="sxs-lookup"><span data-stu-id="3f8f4-191">twinChangeEvents</span></span> |
    <span data-ttu-id="3f8f4-192">$content-kodning</span><span class="sxs-lookup"><span data-stu-id="3f8f4-192">$content-encoding</span></span> | <span data-ttu-id="3f8f4-193">UTF-8</span><span class="sxs-lookup"><span data-stu-id="3f8f4-193">utf-8</span></span> |
    <span data-ttu-id="3f8f4-194">deviceId</span><span class="sxs-lookup"><span data-stu-id="3f8f4-194">deviceId</span></span> | <span data-ttu-id="3f8f4-195">ID för enheten</span><span class="sxs-lookup"><span data-stu-id="3f8f4-195">Id of the device</span></span> |
    <span data-ttu-id="3f8f4-196">hubName</span><span class="sxs-lookup"><span data-stu-id="3f8f4-196">hubName</span></span> | <span data-ttu-id="3f8f4-197">Namnet på IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="3f8f4-197">Name of IoT Hub</span></span> |
    <span data-ttu-id="3f8f4-198">operationTimestamp</span><span class="sxs-lookup"><span data-stu-id="3f8f4-198">operationTimestamp</span></span> | <span data-ttu-id="3f8f4-199">[ISO8601] tidsstämpeln för åtgärden</span><span class="sxs-lookup"><span data-stu-id="3f8f4-199">[ISO8601] timestamp of operation</span></span> |
    <span data-ttu-id="3f8f4-200">iothub-meddelande-schema</span><span class="sxs-lookup"><span data-stu-id="3f8f4-200">iothub-message-schema</span></span> | <span data-ttu-id="3f8f4-201">deviceLifecycleNotification</span><span class="sxs-lookup"><span data-stu-id="3f8f4-201">deviceLifecycleNotification</span></span> |
    <span data-ttu-id="3f8f4-202">opType</span><span class="sxs-lookup"><span data-stu-id="3f8f4-202">opType</span></span> | <span data-ttu-id="3f8f4-203">”replaceTwin” eller ”updateTwin”</span><span class="sxs-lookup"><span data-stu-id="3f8f4-203">"replaceTwin" or "updateTwin"</span></span> |

    <span data-ttu-id="3f8f4-204">Meddelandet Systemegenskaper föregås av `'$'` symbolen.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-204">Message system properties are prefixed with the `'$'` symbol.</span></span>

    - <span data-ttu-id="3f8f4-205">Innehåll</span><span class="sxs-lookup"><span data-stu-id="3f8f4-205">Body</span></span>
        
    <span data-ttu-id="3f8f4-206">Det här avsnittet innehåller dubbla ändringarna i en JSON-format.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-206">This section includes all the twin changes in a JSON format.</span></span> <span data-ttu-id="3f8f4-207">Samma format används som en korrigering, med skillnaden att den kan innehålla alla två avsnitt: taggar, properties.reported, properties.desired och att den innehåller ”$metadata”-element.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-207">It uses the same format as a patch, with the difference that it can contain all twin sections: tags, properties.reported, properties.desired, and that it contains the “$metadata” elements.</span></span> <span data-ttu-id="3f8f4-208">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-208">For example,</span></span>
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

<span data-ttu-id="3f8f4-209">Stöd för alla föregående operationer [Optimistisk samtidighet] [ lnk-concurrency] och kräver den **ServiceConnect** behörighet, som definieras i den [säkerhet] [ lnk-security] artikel.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-209">All the preceding operations support [Optimistic concurrency][lnk-concurrency] and require the **ServiceConnect** permission, as defined in the [Security][lnk-security] article.</span></span>

<span data-ttu-id="3f8f4-210">Förutom dessa åtgärder kan lösningens serverdel</span><span class="sxs-lookup"><span data-stu-id="3f8f4-210">In addition to these operations, the solution back end can:</span></span>

* <span data-ttu-id="3f8f4-211">Fråga enhet-twins med hjälp av SQL-liknande [IoT-hubb frågespråket][lnk-query].</span><span class="sxs-lookup"><span data-stu-id="3f8f4-211">Query the device twins using the SQL-like [IoT Hub query language][lnk-query].</span></span>
* <span data-ttu-id="3f8f4-212">Utföra åtgärder på stora mängder enheten twins med [jobb][lnk-jobs].</span><span class="sxs-lookup"><span data-stu-id="3f8f4-212">Perform operations on large sets of device twins using [jobs][lnk-jobs].</span></span>

## <a name="device-operations"></a><span data-ttu-id="3f8f4-213">Åtgärder för enhet</span><span class="sxs-lookup"><span data-stu-id="3f8f4-213">Device operations</span></span>
<span data-ttu-id="3f8f4-214">Enheten appen körs på den enheten dubbla med hjälp av följande atomiska åtgärder:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-214">The device app operates on the device twin using the following atomic operations:</span></span>

1. <span data-ttu-id="3f8f4-215">**Hämta enheten dubbla**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-215">**Retrieve device twin**.</span></span> <span data-ttu-id="3f8f4-216">Den här åtgärden Returnerar enheten dubbla dokumentet (inklusive taggar och önskas, rapporteras och Systemegenskaper) för den anslutna enheten.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-216">This operation returns the device twin document (including tags and desired, reported and system properties) for the currently connected device.</span></span>
2. <span data-ttu-id="3f8f4-217">**Uppdatera delvis rapporterade egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-217">**Partially update reported properties**.</span></span> <span data-ttu-id="3f8f4-218">Den här åtgärden aktiverar deluppdatering rapporterade egenskaper för den anslutna enheten.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-218">This operation enables the partial update of the reported properties of the currently connected device.</span></span> <span data-ttu-id="3f8f4-219">Den här åtgärden används samma JSON update format-att lösningen tillbaka för en deluppdatering av egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-219">This operation uses the same JSON update format that the solution back end uses for a partial update of desired properties.</span></span>
3. <span data-ttu-id="3f8f4-220">**Se egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-220">**Observe desired properties**.</span></span> <span data-ttu-id="3f8f4-221">Den anslutna enheten kan välja att aviseras om uppdateringar av egenskaperna när de inträffar.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-221">The currently connected device can choose to be notified of updates to the desired properties when they happen.</span></span> <span data-ttu-id="3f8f4-222">Enheten tar emot uppdateringen (eller delvis ersättning) som körs av lösningens serverdel samma formulär.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-222">The device receives the same form of update (partial or full replacement) executed by the solution back end.</span></span>

<span data-ttu-id="3f8f4-223">Alla föregående operationer kräver den **DeviceConnect** behörighet, som definieras i den [säkerhet] [ lnk-security] artikel.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-223">All the preceding operations require the **DeviceConnect** permission, as defined in the [Security][lnk-security] article.</span></span>

<span data-ttu-id="3f8f4-224">Den [Azure IoT-enhet SDK] [ lnk-sdks] gör det lättare att använda föregående åtgärder från många språk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-224">The [Azure IoT device SDKs][lnk-sdks] make it easy to use the preceding operations from many languages and platforms.</span></span> <span data-ttu-id="3f8f4-225">Mer information om information om IoT-hubb primitiver för synkronisering av egenskaper finns i [enheten återanslutning flödet][lnk-reconnection].</span><span class="sxs-lookup"><span data-stu-id="3f8f4-225">More information on the details of IoT Hub primitives for desired properties synchronization can be found in [Device reconnection flow][lnk-reconnection].</span></span>

> [!NOTE]
> <span data-ttu-id="3f8f4-226">Enheten twins för närvarande enbart tillgänglig från enheter som ansluter till IoT-hubb med MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-226">Currently, device twins are accessible only from devices that connect to IoT Hub using the MQTT protocol.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="3f8f4-227">Referensinformation:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-227">Reference topics:</span></span>
<span data-ttu-id="3f8f4-228">Följande referensavsnitt ge mer information om hur du styr åtkomst till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-228">The following reference topics provide you with more information about controlling access to your IoT hub.</span></span>

## <a name="tags-and-properties-format"></a><span data-ttu-id="3f8f4-229">Taggar och egenskaper format</span><span class="sxs-lookup"><span data-stu-id="3f8f4-229">Tags and properties format</span></span>
<span data-ttu-id="3f8f4-230">Taggar, önskade och rapporterade egenskaper är JSON-objekt med följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-230">Tags, desired, and reported properties are JSON objects with the following restrictions:</span></span>

* <span data-ttu-id="3f8f4-231">Alla nycklar i JSON-objekt är skiftlägeskänsliga 64 byte UTF-8, UNICODE-strängar.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-231">All keys in JSON objects are case-sensitive 64 bytes UTF-8 UNICODE strings.</span></span> <span data-ttu-id="3f8f4-232">Tillåtna tecken undanta Unicode-kontrolltecken (segment C0 och C1) och `'.'`, `' '`, och `'$'`.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-232">Allowed characters exclude UNICODE control characters (segments C0 and C1), and `'.'`, `' '`, and `'$'`.</span></span>
* <span data-ttu-id="3f8f4-233">Alla värden i JSON-objekt kan vara följande typer av JSON: boolean, nummer, sträng, objekt.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-233">All values in JSON objects can be of the following JSON types: boolean, number, string, object.</span></span> <span data-ttu-id="3f8f4-234">Matriser är inte tillåtna.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-234">Arrays are not allowed.</span></span>
* <span data-ttu-id="3f8f4-235">JSON-objekt i taggar, önskade och rapporterade egenskaper kan ha högst 5.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-235">All JSON objects in tags, desired, and reported properties can have a maximum depth of 5.</span></span> <span data-ttu-id="3f8f4-236">Följande objekt är till exempel giltig:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-236">For instance, the following object is valid:</span></span>

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

* <span data-ttu-id="3f8f4-237">Alla strängvärden kan innehålla högst 512 byte i längd.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-237">All string values can be at most 512 bytes in length.</span></span>

## <a name="device-twin-size"></a><span data-ttu-id="3f8f4-238">Enheten dubbla storlek</span><span class="sxs-lookup"><span data-stu-id="3f8f4-238">Device twin size</span></span>
<span data-ttu-id="3f8f4-239">IoT-hubb tillämpar en begränsning på 8KB storleken på värdena i `tags`, `properties/desired`, och `properties/reported`, exklusive skrivskyddade element.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-239">IoT Hub enforces an 8KB size limitation on the values of `tags`, `properties/desired`, and `properties/reported`, excluding read-only elements.</span></span>
<span data-ttu-id="3f8f4-240">Storleken beräknas genom att räkna alla tecken utom UNICODE styra tecken (segment C0 och C1) och blanksteg `' '` när den visas utanför en strängkonstant.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-240">The size is computed by counting all characters excluding UNICODE control characters (segments C0 and C1) and space `' '` when it appears outside of a string constant.</span></span>
<span data-ttu-id="3f8f4-241">IoT-hubb avvisar alla åtgärder som kan öka storleken på dessa dokument än gränsen med ett fel.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-241">IoT Hub rejects with an error all operations that would increase the size of those documents above the limit.</span></span>

## <a name="device-twin-metadata"></a><span data-ttu-id="3f8f4-242">Enhetens dubbla metadata</span><span class="sxs-lookup"><span data-stu-id="3f8f4-242">Device twin metadata</span></span>
<span data-ttu-id="3f8f4-243">IoT-hubb underhåller tidsstämpel för den senaste uppdateringen för varje JSON-objekt i enheten dubbla önskad och rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-243">IoT Hub maintains the timestamp of the last update for each JSON object in device twin desired and reported properties.</span></span> <span data-ttu-id="3f8f4-244">Tidsstämplar i UTC och kodats i den [ISO8601] format `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-244">The timestamps are in UTC and encoded in the [ISO8601] format `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span></span>
<span data-ttu-id="3f8f4-245">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-245">For example:</span></span>

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

<span data-ttu-id="3f8f4-246">Denna information lagras på varje nivå (inte bara löv i JSON-strukturen) för att spara uppdateringar som tar bort objektnycklar.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-246">This information is kept at every level (not just the leaves of the JSON structure) to preserve updates that remove object keys.</span></span>

## <a name="optimistic-concurrency"></a><span data-ttu-id="3f8f4-247">Optimistisk samtidighet</span><span class="sxs-lookup"><span data-stu-id="3f8f4-247">Optimistic concurrency</span></span>
<span data-ttu-id="3f8f4-248">Taggar, önskad och rapporterade egenskaper alla stöd för Optimistisk samtidighet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-248">Tags, desired, and reported properties all support optimistic concurrency.</span></span>
<span data-ttu-id="3f8f4-249">Taggar har en ETag enligt [RFC7232], som representerar den tagg JSON-representation.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-249">Tags have an ETag, as per [RFC7232], that represents the tag's JSON representation.</span></span> <span data-ttu-id="3f8f4-250">Du kan använda ETags i villkorlig uppdateringsåtgärder från lösningens serverdel för att säkerställa konsekvens.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-250">You can use ETags in conditional update operations from the solution back end to ensure consistency.</span></span>

<span data-ttu-id="3f8f4-251">Enheten dubbla önskad och rapporterade egenskaper har inte ETags, men har en `$version` värde som garanterat inkrementell.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-251">Device twin desired and reported properties do not have ETags, but have a `$version` value that is guaranteed to be incremental.</span></span> <span data-ttu-id="3f8f4-252">På liknande sätt till en ETag kan versionen användas av uppdatering part vill använda konsekvent av uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-252">Similarly to an ETag, the version can be used by the updating party to enforce consistency of updates.</span></span> <span data-ttu-id="3f8f4-253">Till exempel en enhetsapp för en rapporterade egenskap eller lösningens serverdel för en önskad egenskap.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-253">For example, a device app for a reported property or the solution back end for a desired property.</span></span>

<span data-ttu-id="3f8f4-254">Versioner är också användbart när en observing agent (till exempel appen enhet observerar du egenskaperna) måste stämma lopp mellan resultatet av en hämta och ett uppdateringsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-254">Versions are also useful when an observing agent (such as the device app observing the desired properties) must reconcile races between the result of a retrieve operation and an update notification.</span></span> <span data-ttu-id="3f8f4-255">Avsnittet [enheten återanslutning flödet] [ lnk-reconnection] innehåller mer information.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-255">The section [Device reconnection flow][lnk-reconnection] provides more information.</span></span>

## <a name="device-reconnection-flow"></a><span data-ttu-id="3f8f4-256">Enheten återanslutning flöde</span><span class="sxs-lookup"><span data-stu-id="3f8f4-256">Device reconnection flow</span></span>
<span data-ttu-id="3f8f4-257">IoT-hubb bevaras inte egenskaper uppdateringsmeddelanden för frånkopplade enheter.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-257">IoT Hub does not preserve desired properties update notifications for disconnected devices.</span></span> <span data-ttu-id="3f8f4-258">Följer att en enhet som ansluter måste hämta dokumentet med fullständig önskade egenskaper, utöver att prenumerera på meddelanden om uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-258">It follows that a device that is connecting must retrieve the full desired properties document, in addition to subscribing for update notifications.</span></span> <span data-ttu-id="3f8f4-259">Möjlighet lopp mellan uppdateringsmeddelanden och fullständig hämtning, måste följande flöde säkerställas:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-259">Given the possibility of races between update notifications and full retrieval, the following flow must be ensured:</span></span>

1. <span data-ttu-id="3f8f4-260">Enheten appen ansluter till en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-260">Device app connects to an IoT hub.</span></span>
2. <span data-ttu-id="3f8f4-261">Enhetsapp prenumererar för önskade egenskaper meddelanden om uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-261">Device app subscribes for desired properties update notifications.</span></span>
3. <span data-ttu-id="3f8f4-262">Enhetsapp hämtar det fullständiga dokumentet för egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-262">Device app retrieves the full document for desired properties.</span></span>

<span data-ttu-id="3f8f4-263">Appen enheten kan ignorera alla meddelanden med `$version` mindre än eller lika med versionen av det fullständiga hämtade dokumentet.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-263">The device app can ignore all notifications with `$version` less or equal than the version of the full retrieved document.</span></span> <span data-ttu-id="3f8f4-264">Den här metoden är möjligt eftersom IoT-hubb garanterar att det alltid öka versioner.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-264">This approach is possible because IoT Hub guarantees that versions always increment.</span></span>

> [!NOTE]
> <span data-ttu-id="3f8f4-265">Den här logiken har redan implementerats i den [Azure IoT-enhet SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="3f8f4-265">This logic is already implemented in the [Azure IoT device SDKs][lnk-sdks].</span></span> <span data-ttu-id="3f8f4-266">Den här beskrivningen är användbart om enheten appen kan inte använda någon av Azure IoT-enhet SDK: er och programmet måste gränssnittet MQTT direkt.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-266">This description is useful only if the device app cannot use any of Azure IoT device SDKs and must program the MQTT interface directly.</span></span>
> 
> 

## <a name="additional-reference-material"></a><span data-ttu-id="3f8f4-267">Ytterligare referensmaterialet</span><span class="sxs-lookup"><span data-stu-id="3f8f4-267">Additional reference material</span></span>
<span data-ttu-id="3f8f4-268">Andra referensavsnitten i utvecklarhandboken för IoT-hubben är:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-268">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="3f8f4-269">Den [IoT-hubbslutpunkter] [ lnk-endpoints] artikeln beskriver de olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-269">The [IoT Hub endpoints][lnk-endpoints] article describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="3f8f4-270">Den [begränsning och kvoter] [ lnk-quotas] artikeln kvoterna som gäller för IoT-hubb-tjänsten och bandbreddsbegränsning beteende som händer när du använder tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-270">The [Throttling and quotas][lnk-quotas] article describes the quotas that apply to the IoT Hub service and the throttling behavior to expect when you use the service.</span></span>
* <span data-ttu-id="3f8f4-271">Den [Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] artikeln innehåller olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-271">The [Azure IoT device and service SDKs][lnk-sdks] article lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="3f8f4-272">Den [IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning] [ lnk-query] IoT-hubb frågespråk som du kan använda för att hämta information från IoT-hubb om enheten twins och jobb för artikeln.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-272">The [IoT Hub query language for device twins, jobs, and message routing][lnk-query] article describes the IoT Hub query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="3f8f4-273">Den [IoT-hubb MQTT stöd] [ lnk-devguide-mqtt] artikeln innehåller mer information om IoT-hubb stöd för protokollet MQTT.</span><span class="sxs-lookup"><span data-stu-id="3f8f4-273">The [IoT Hub MQTT support][lnk-devguide-mqtt] article provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f8f4-274">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f8f4-274">Next steps</span></span>
<span data-ttu-id="3f8f4-275">Nu har du fått veta om enheten twins du vill ha i följande IoT-hubb developer guide avsnitt:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-275">Now you have learned about device twins, you may be interested in the following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="3f8f4-276">[Anropa en metod som är direkt på en enhet][lnk-methods]</span><span class="sxs-lookup"><span data-stu-id="3f8f4-276">[Invoke a direct method on a device][lnk-methods]</span></span>
* <span data-ttu-id="3f8f4-277">[Schema-jobb på flera enheter][lnk-jobs]</span><span class="sxs-lookup"><span data-stu-id="3f8f4-277">[Schedule jobs on multiple devices][lnk-jobs]</span></span>

<span data-ttu-id="3f8f4-278">Om du vill testa vissa av de begrepp som beskrivs i den här artikeln får du är intresserad av IoT-hubb följande kurser:</span><span class="sxs-lookup"><span data-stu-id="3f8f4-278">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorials:</span></span>

* <span data-ttu-id="3f8f4-279">[Hur du använder enheten dubbla][lnk-twin-tutorial]</span><span class="sxs-lookup"><span data-stu-id="3f8f4-279">[How to use the device twin][lnk-twin-tutorial]</span></span>
* <span data-ttu-id="3f8f4-280">[Hur du använder identiska enhetsegenskaper][lnk-twin-properties]</span><span class="sxs-lookup"><span data-stu-id="3f8f4-280">[How to use device twin properties][lnk-twin-properties]</span></span>

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

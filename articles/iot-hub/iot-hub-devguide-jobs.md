---
title: aaaUnderstand Azure IoT Hub jobb | Microsoft Docs
description: "Utvecklarhandbok - schemaläggning av jobb toorun på flera enheter anslutna tooyour IoT-hubb. Jobb kan uppdatera taggar och önskade egenskaper och anropa direkt metoder på flera enheter."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: fe78458f-4f14-4358-ac83-4f7bd14ee8da
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
ms.openlocfilehash: 8be134e6c379feae5087df8f562a74505c57afee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-jobs-on-multiple-devices"></a><span data-ttu-id="7851e-104">Schemalägga jobb på flera enheter</span><span class="sxs-lookup"><span data-stu-id="7851e-104">Schedule jobs on multiple devices</span></span>
## <a name="overview"></a><span data-ttu-id="7851e-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="7851e-105">Overview</span></span>
<span data-ttu-id="7851e-106">Enligt beskrivningen i föregående artiklar Azure IoT Hub möjliggör ett antal byggblock ([dubbla enhetsegenskaper och taggar] [ lnk-twin-devguide] och [direkt metoder][lnk-dev-methods]).</span><span class="sxs-lookup"><span data-stu-id="7851e-106">As described by previous articles, Azure IoT Hub enables a number of building blocks ([device twin properties and tags][lnk-twin-devguide] and [direct methods][lnk-dev-methods]).</span></span>  <span data-ttu-id="7851e-107">Normalt backend-appar Aktivera enheten administratörer och ansvariga för tooupdate och interagera med IoT-enheter gruppvis och vid ett schemalagt klockslag.</span><span class="sxs-lookup"><span data-stu-id="7851e-107">Typically, back-end apps enable device administrators and operators tooupdate and interact with IoT devices in bulk and at a scheduled time.</span></span>  <span data-ttu-id="7851e-108">Jobb kapslar in hello körning av dubbla uppdateringar och direkt metoder mot en uppsättning enheter i taget schema.</span><span class="sxs-lookup"><span data-stu-id="7851e-108">Jobs encapsulate hello execution of device twin updates and direct methods against a set of devices at a schedule time.</span></span>  <span data-ttu-id="7851e-109">En operator skulle till exempel använda en backend-app som skulle startar och spårar en jobbet tooreboot en uppsättning enheter i att skapa 43 och våning 3 vid en tidpunkt som inte skulle vara störande toohello operations hello byggnaden.</span><span class="sxs-lookup"><span data-stu-id="7851e-109">For example, an operator would use a back-end app that would initiate and track a job tooreboot a set of devices in building 43 and floor 3 at a time that would not be disruptive toohello operations of hello building.</span></span>

### <a name="when-toouse"></a><span data-ttu-id="7851e-110">När toouse</span><span class="sxs-lookup"><span data-stu-id="7851e-110">When toouse</span></span>
<span data-ttu-id="7851e-111">Överväg att använda jobb när: en lösning för serverdelen måste tooschedule och spåra vidare någon hello följande aktiviteter på en uppsättning enheter:</span><span class="sxs-lookup"><span data-stu-id="7851e-111">Consider using jobs when: a solution back end needs tooschedule and track progress any of hello following activities on a set of device:</span></span>

* <span data-ttu-id="7851e-112">Uppdatera önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="7851e-112">Update desired properties</span></span>
* <span data-ttu-id="7851e-113">Uppdatera taggar</span><span class="sxs-lookup"><span data-stu-id="7851e-113">Update tags</span></span>
* <span data-ttu-id="7851e-114">Anropa direkt metoder</span><span class="sxs-lookup"><span data-stu-id="7851e-114">Invoke direct methods</span></span>

## <a name="job-lifecycle"></a><span data-ttu-id="7851e-115">Jobbet livscykel</span><span class="sxs-lookup"><span data-stu-id="7851e-115">Job lifecycle</span></span>
<span data-ttu-id="7851e-116">Jobb initieras av hello lösningens serverdel och underhålls av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7851e-116">Jobs are initiated by hello solution back end and maintained by IoT Hub.</span></span>  <span data-ttu-id="7851e-117">Du kan starta ett jobb via en tjänst-riktade URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) och status för ett jobb som körs via en tjänst-riktade URI-fråga (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span><span class="sxs-lookup"><span data-stu-id="7851e-117">You can initiate a job through a service-facing URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) and query for progress on an executing job through a service-facing URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`).</span></span>  <span data-ttu-id="7851e-118">När ett jobb startas kan frågar efter jobb hello backend-app toorefresh hello status för jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="7851e-118">Once a job is initiated, querying for jobs enables hello back-end app toorefresh hello status of running jobs.</span></span>

> [!NOTE]
> <span data-ttu-id="7851e-119">När du startar en egenskapsnamn och värden kan endast innehålla US-ASCII utskrivbara alfanumeriskt, förutom eventuella i hello följande uppsättning: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="7851e-119">When you initiate a job, property names and values can only contain US-ASCII printable alphanumeric, except any in hello following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="7851e-120">Referensinformation:</span><span class="sxs-lookup"><span data-stu-id="7851e-120">Reference topics:</span></span>
<span data-ttu-id="7851e-121">hello efter referensavsnitt ge mer information om hur du använder jobb.</span><span class="sxs-lookup"><span data-stu-id="7851e-121">hello following reference topics provide you with more information about using jobs.</span></span>

## <a name="jobs-tooexecute-direct-methods"></a><span data-ttu-id="7851e-122">Jobb tooexecute direkt metoder</span><span class="sxs-lookup"><span data-stu-id="7851e-122">Jobs tooexecute direct methods</span></span>
<span data-ttu-id="7851e-123">hello följer hello HTTP 1.1-begäran-information för att köra en [direkt metod] [ lnk-dev-methods] på en uppsättning enheter med hjälp av ett jobb:</span><span class="sxs-lookup"><span data-stu-id="7851e-123">hello following is hello HTTP 1.1 request details for executing a [direct method][lnk-dev-methods] on a set of devices using a job:</span></span>

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
<span data-ttu-id="7851e-124">hello frågevillkoret kan också vara på en enda enhets-Id eller en lista över enhets-ID som visas nedan</span><span class="sxs-lookup"><span data-stu-id="7851e-124">hello query condition can also be on a single device Id or on a list of device Ids as shown below</span></span>

<span data-ttu-id="7851e-125">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="7851e-125">**Examples**</span></span>
```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
<span data-ttu-id="7851e-126">[IoT-hubb Query Language] [ lnk-query] täcker IoT-hubb frågespråket i mer detalj.</span><span class="sxs-lookup"><span data-stu-id="7851e-126">[IoT Hub Query Language][lnk-query] covers IoT Hub query language in additional detail.</span></span>

## <a name="jobs-tooupdate-device-twin-properties"></a><span data-ttu-id="7851e-127">Jobb tooupdate dubbla enhetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="7851e-127">Jobs tooupdate device twin properties</span></span>
<span data-ttu-id="7851e-128">hello följer hello HTTP 1.1 Frågedetaljer för att uppdatera enheten två egenskaper med hjälp av ett jobb:</span><span class="sxs-lookup"><span data-stu-id="7851e-128">hello following is hello HTTP 1.1 request details for updating device twin properties using a job:</span></span>

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a><span data-ttu-id="7851e-129">Frågar efter status för jobb</span><span class="sxs-lookup"><span data-stu-id="7851e-129">Querying for progress on jobs</span></span>
<span data-ttu-id="7851e-130">hello följer hello HTTP 1.1 Frågedetaljer för [frågar efter jobb][lnk-query]:</span><span class="sxs-lookup"><span data-stu-id="7851e-130">hello following is hello HTTP 1.1 request details for [querying for jobs][lnk-query]:</span></span>

    ```
    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```

<span data-ttu-id="7851e-131">Hej continuationToken tillhandahålls av hello svar.</span><span class="sxs-lookup"><span data-stu-id="7851e-131">hello continuationToken is provided from hello response.</span></span>  

## <a name="jobs-properties"></a><span data-ttu-id="7851e-132">Egenskaper för jobb</span><span class="sxs-lookup"><span data-stu-id="7851e-132">Jobs Properties</span></span>
<span data-ttu-id="7851e-133">hello följer en lista över egenskaper och motsvarande beskrivningar som kan användas när du frågar för jobb eller jobb resultat.</span><span class="sxs-lookup"><span data-stu-id="7851e-133">hello following is a list of properties and corresponding descriptions, which can be used when querying for jobs or job results.</span></span>

| <span data-ttu-id="7851e-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7851e-134">Property</span></span> | <span data-ttu-id="7851e-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7851e-135">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7851e-136">**jobId**</span><span class="sxs-lookup"><span data-stu-id="7851e-136">**jobId**</span></span> |<span data-ttu-id="7851e-137">Programmet ange ID för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="7851e-137">Application provided ID for hello job.</span></span> |
| <span data-ttu-id="7851e-138">**startTime**</span><span class="sxs-lookup"><span data-stu-id="7851e-138">**startTime**</span></span> |<span data-ttu-id="7851e-139">Program som starttid (ISO 8601) för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="7851e-139">Application provided start time (ISO-8601) for hello job.</span></span> |
| <span data-ttu-id="7851e-140">**Sluttid**</span><span class="sxs-lookup"><span data-stu-id="7851e-140">**endTime**</span></span> |<span data-ttu-id="7851e-141">IoT-hubb som avses datum (ISO 8601) när hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7851e-141">IoT Hub provided date (ISO-8601) for when hello job completed.</span></span> <span data-ttu-id="7851e-142">Gäller endast när hello jobbet når hello ”klar” tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7851e-142">Valid only after hello job reaches hello 'completed' state.</span></span> |
| <span data-ttu-id="7851e-143">**typ**</span><span class="sxs-lookup"><span data-stu-id="7851e-143">**type**</span></span> |<span data-ttu-id="7851e-144">Typer av jobb:</span><span class="sxs-lookup"><span data-stu-id="7851e-144">Types of jobs:</span></span> |
| <span data-ttu-id="7851e-145">**scheduledUpdateTwin**: jobben-tooupdate en uppsättning egenskaper eller taggar.</span><span class="sxs-lookup"><span data-stu-id="7851e-145">**scheduledUpdateTwin**: A job used tooupdate a set of desired properties or tags.</span></span> | |
| <span data-ttu-id="7851e-146">**scheduledDeviceMethod**: jobben-tooinvoke en enhet metod i en uppsättning twins för enheten.</span><span class="sxs-lookup"><span data-stu-id="7851e-146">**scheduledDeviceMethod**: A job used tooinvoke a device method on a set of device twins.</span></span> | |
| <span data-ttu-id="7851e-147">**status**</span><span class="sxs-lookup"><span data-stu-id="7851e-147">**status**</span></span> |<span data-ttu-id="7851e-148">Hello jobbet aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="7851e-148">Current state of hello job.</span></span> <span data-ttu-id="7851e-149">Möjliga värden för status:</span><span class="sxs-lookup"><span data-stu-id="7851e-149">Possible values for status:</span></span> |
| <span data-ttu-id="7851e-150">**väntande** : schemaläggs och väntar toobe tas upp av hello jobbet-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7851e-150">**pending** : Scheduled and waiting toobe picked up by hello job service.</span></span> | |
| <span data-ttu-id="7851e-151">**schemalagda** : schemalagd vid en tidpunkt i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="7851e-151">**scheduled** : Scheduled for a time in hello future.</span></span> | |
| <span data-ttu-id="7851e-152">**kör** : aktiva jobb.</span><span class="sxs-lookup"><span data-stu-id="7851e-152">**running** : Currently active job.</span></span> | |
| <span data-ttu-id="7851e-153">**avbröt** : jobbet har avbrutits.</span><span class="sxs-lookup"><span data-stu-id="7851e-153">**cancelled** : Job has been cancelled.</span></span> | |
| <span data-ttu-id="7851e-154">**Det gick inte** : jobbet misslyckades.</span><span class="sxs-lookup"><span data-stu-id="7851e-154">**failed** : Job failed.</span></span> | |
| <span data-ttu-id="7851e-155">**slutföra** : jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7851e-155">**completed** : Job has completed.</span></span> | |
| <span data-ttu-id="7851e-156">**deviceJobStatistics**</span><span class="sxs-lookup"><span data-stu-id="7851e-156">**deviceJobStatistics**</span></span> |<span data-ttu-id="7851e-157">Statistik om hello jobbkörningen.</span><span class="sxs-lookup"><span data-stu-id="7851e-157">Statistics about hello job's execution.</span></span> |

<span data-ttu-id="7851e-158">**deviceJobStatistics** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7851e-158">**deviceJobStatistics** properties.</span></span>

| <span data-ttu-id="7851e-159">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7851e-159">Property</span></span> | <span data-ttu-id="7851e-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7851e-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7851e-161">**deviceJobStatistics.deviceCount**</span><span class="sxs-lookup"><span data-stu-id="7851e-161">**deviceJobStatistics.deviceCount**</span></span> |<span data-ttu-id="7851e-162">Antal enheter i hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="7851e-162">Number of devices in hello job.</span></span> |
| <span data-ttu-id="7851e-163">**deviceJobStatistics.failedCount**</span><span class="sxs-lookup"><span data-stu-id="7851e-163">**deviceJobStatistics.failedCount**</span></span> |<span data-ttu-id="7851e-164">Antal enheter där hello-jobbet misslyckades.</span><span class="sxs-lookup"><span data-stu-id="7851e-164">Number of devices where hello job failed.</span></span> |
| <span data-ttu-id="7851e-165">**deviceJobStatistics.succeededCount**</span><span class="sxs-lookup"><span data-stu-id="7851e-165">**deviceJobStatistics.succeededCount**</span></span> |<span data-ttu-id="7851e-166">Antal enheter där hello jobbet lyckades.</span><span class="sxs-lookup"><span data-stu-id="7851e-166">Number of devices where hello job succeeded.</span></span> |
| <span data-ttu-id="7851e-167">**deviceJobStatistics.runningCount**</span><span class="sxs-lookup"><span data-stu-id="7851e-167">**deviceJobStatistics.runningCount**</span></span> |<span data-ttu-id="7851e-168">Antalet enheter som för närvarande körs hello jobb.</span><span class="sxs-lookup"><span data-stu-id="7851e-168">Number of devices that are currently running hello job.</span></span> |
| <span data-ttu-id="7851e-169">**deviceJobStatistics.pendingCount**</span><span class="sxs-lookup"><span data-stu-id="7851e-169">**deviceJobStatistics.pendingCount**</span></span> |<span data-ttu-id="7851e-170">Antalet enheter som väntar på toorun hello jobb.</span><span class="sxs-lookup"><span data-stu-id="7851e-170">Number of devices that are pending toorun hello job.</span></span> |

### <a name="additional-reference-material"></a><span data-ttu-id="7851e-171">Ytterligare referensmaterialet</span><span class="sxs-lookup"><span data-stu-id="7851e-171">Additional reference material</span></span>
<span data-ttu-id="7851e-172">Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:</span><span class="sxs-lookup"><span data-stu-id="7851e-172">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="7851e-173">[IoT-hubbslutpunkter] [ lnk-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="7851e-173">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="7851e-174">[Begränsning och kvoter] [ lnk-quotas] beskriver hello kvoter som gäller toohello IoT-hubb-tjänsten och hello bandbreddsbegränsning beteende tooexpect när du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7851e-174">[Throttling and quotas][lnk-quotas] describes hello quotas that apply toohello IoT Hub service and hello throttling behavior tooexpect when you use hello service.</span></span>
* <span data-ttu-id="7851e-175">[Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] visar hello SDK: er för olika språk du en användning när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="7851e-175">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you an use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="7851e-176">[IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning] [ lnk-query] beskriver hello IoT-hubb frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb.</span><span class="sxs-lookup"><span data-stu-id="7851e-176">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes hello IoT Hub query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="7851e-177">[Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för hello MQTT protokollet.</span><span class="sxs-lookup"><span data-stu-id="7851e-177">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7851e-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7851e-178">Next steps</span></span>
<span data-ttu-id="7851e-179">Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb kursen:</span><span class="sxs-lookup"><span data-stu-id="7851e-179">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorial:</span></span>

* <span data-ttu-id="7851e-180">[Schemat och sändning jobb][lnk-jobs-tutorial]</span><span class="sxs-lookup"><span data-stu-id="7851e-180">[Schedule and broadcast jobs][lnk-jobs-tutorial]</span></span>

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md

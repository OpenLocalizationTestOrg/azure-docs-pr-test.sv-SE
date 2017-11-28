---
title: "aaaUnderstand hello Azure IoT Hub-frågespråket | Microsoft Docs"
description: "Utvecklarhandbok - beskrivning av hello SQL-liknande IoT-hubb-frågespråket tooretrieve information om enheten twins och jobb från din IoT-hubb som används."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/17
ms.author: elioda
ms.openlocfilehash: 01a7c8ffdf44c6c27b834739d02c8fef1dd3d3fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-query-language-for-device-twins-jobs-and-message-routing"></a><span data-ttu-id="eb6f4-103">Referens - frågespråket för IoT-hubb för enheten twins, jobb och meddelanderoutning</span><span class="sxs-lookup"><span data-stu-id="eb6f4-103">Reference - IoT Hub query language for device twins, jobs, and message routing</span></span>

<span data-ttu-id="eb6f4-104">IoT-hubb innehåller en kraftfull SQL-liknande språk tooretrieve information angående [enhet twins] [ lnk-twins] och [jobb][lnk-jobs], och [meddelanderoutning][lnk-devguide-messaging-routes].</span><span class="sxs-lookup"><span data-stu-id="eb6f4-104">IoT Hub provides a powerful SQL-like language tooretrieve information regarding [device twins][lnk-twins] and [jobs][lnk-jobs], and [message routing][lnk-devguide-messaging-routes].</span></span> <span data-ttu-id="eb6f4-105">Den här artikeln beskriver:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-105">This article presents:</span></span>

* <span data-ttu-id="eb6f4-106">En introduktion toohello viktigaste funktionerna i hello IoT-hubb frågespråket och</span><span class="sxs-lookup"><span data-stu-id="eb6f4-106">An introduction toohello major features of hello IoT Hub query language, and</span></span>
* <span data-ttu-id="eb6f4-107">hello detaljerad beskrivning av hello-språket.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-107">hello detailed description of hello language.</span></span>

## <a name="get-started-with-device-twin-queries"></a><span data-ttu-id="eb6f4-108">Kom igång med enheten dubbla frågor</span><span class="sxs-lookup"><span data-stu-id="eb6f4-108">Get started with device twin queries</span></span>
<span data-ttu-id="eb6f4-109">[Enheten twins] [ lnk-twins] godtyckliga JSON-objekt kan innehålla både taggar och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-109">[Device twins][lnk-twins] can contain arbitrary JSON objects as both tags and properties.</span></span> <span data-ttu-id="eb6f4-110">IoT-hubb kan du tooquery enhet twins som enda JSON-dokument som innehåller alla dubbla enhetsinformation.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-110">IoT Hub enables you tooquery device twins as a single JSON document containing all device twin information.</span></span>
<span data-ttu-id="eb6f4-111">Anta exempelvis att din IoT-hubb enheten twins har hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-111">Assume, for instance, that your IoT hub device twins have hello following structure:</span></span>

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        "location": {
            "region": "US",
            "plant": "Redmond43"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300
            },
            "$metadata": {
            ...
            },
            "$version": 4
        },
        "reported": {
            "connectivity": {
                "type": "cellular"
            },
            "telemetryConfig": {
                "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",
                "sendFrequencyInSecs": 300,
                "status": "Success"
            },
            "$metadata": {
            ...
            },
            "$version": 7
        }
    }
}
```

<span data-ttu-id="eb6f4-112">IoT-hubb visar hello enheten twins som en dokumentsamling som heter **enheter**.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-112">IoT Hub exposes hello device twins as a document collection called **devices**.</span></span>
<span data-ttu-id="eb6f4-113">Hello följande fråga hämtar så hello hela uppsättningen med enheten twins:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-113">So hello following query retrieves hello whole set of device twins:</span></span>

```sql
SELECT * FROM devices
```

> [!NOTE]
> <span data-ttu-id="eb6f4-114">[Azure IoT-SDK] [ lnk-hub-sdks] stöd för sidindelning av stora resultat.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-114">[Azure IoT SDKs][lnk-hub-sdks] support paging of large results.</span></span>

<span data-ttu-id="eb6f4-115">IoT-hubb kan tooretrieve enhet twins med valfri villkor för filtrering.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-115">IoT Hub allows you tooretrieve device twins filtering with arbitrary conditions.</span></span> <span data-ttu-id="eb6f4-116">Till exempel</span><span class="sxs-lookup"><span data-stu-id="eb6f4-116">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
```

<span data-ttu-id="eb6f4-117">hämtar hello enheten twins med hello **location.region** grupp för**USA**.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-117">retrieves hello device twins with hello **location.region** tag set too**US**.</span></span>
<span data-ttu-id="eb6f4-118">Booleska operatorer och aritmetiskt jämförelser stöds, till exempel</span><span class="sxs-lookup"><span data-stu-id="eb6f4-118">Boolean operators and arithmetic comparisons are supported as well, for example</span></span>

```sql
SELECT * FROM devices
WHERE tags.location.region = 'US'
    AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60
```

<span data-ttu-id="eb6f4-119">hämtar alla enhet twins finns i hello oss konfigurerade toosend telemetri mindre ofta än varje minut.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-119">retrieves all device twins located in hello US configured toosend telemetry less often than every minute.</span></span> <span data-ttu-id="eb6f4-120">Bekvämlighets skull, är det också möjligt toouse matriskonstanter med hello **IN** och **ni** (inte i) operatörer.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-120">As a convenience, it is also possible toouse array constants with hello **IN** and **NIN** (not in) operators.</span></span> <span data-ttu-id="eb6f4-121">Till exempel</span><span class="sxs-lookup"><span data-stu-id="eb6f4-121">For instance,</span></span>

```sql
SELECT * FROM devices
WHERE properties.reported.connectivity IN ['wired', 'wifi']
```

<span data-ttu-id="eb6f4-122">hämtar alla enheten twins som rapporteras Wi-Fi eller trådbunden anslutning.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-122">retrieves all device twins that reported WiFi or wired connectivity.</span></span> <span data-ttu-id="eb6f4-123">Det är ofta nödvändiga tooidentify alla twins för enheten som innehåller en specifik egenskap.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-123">It is often necessary tooidentify all device twins that contain a specific property.</span></span> <span data-ttu-id="eb6f4-124">IoT-hubb stöder hello funktionen `is_defined()` för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-124">IoT Hub supports hello function `is_defined()` for this purpose.</span></span> <span data-ttu-id="eb6f4-125">Till exempel</span><span class="sxs-lookup"><span data-stu-id="eb6f4-125">For instance,</span></span>

```SQL
SELECT * FROM devices
WHERE is_defined(properties.reported.connectivity)
```

<span data-ttu-id="eb6f4-126">Hämta alla enheten twins som definierar hello `connectivity` rapporterade egenskapen.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-126">retrieved all device twins that define hello `connectivity` reported property.</span></span> <span data-ttu-id="eb6f4-127">Se toohello [WHERE-satsen] [ lnk-query-where] avsnitt hello fullständig referens i hello filtreringsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-127">Refer toohello [WHERE clause][lnk-query-where] section for hello full reference of hello filtering capabilities.</span></span>

<span data-ttu-id="eb6f4-128">Gruppering och aggregeringar stöds också.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-128">Grouping and aggregations are also supported.</span></span> <span data-ttu-id="eb6f4-129">Till exempel</span><span class="sxs-lookup"><span data-stu-id="eb6f4-129">For instance,</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="eb6f4-130">Returnerar hello antal hello enheter i varje status för konfiguration av telemetri.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-130">returns hello count of hello devices in each telemetry configuration status.</span></span>

```json
[
    {
        "numberOfDevices": 3,
        "status": "Success"
    },
    {
        "numberOfDevices": 2,
        "status": "Pending"
    },
    {
        "numberOfDevices": 1,
        "status": "Error"
    }
]
```

<span data-ttu-id="eb6f4-131">hello illustrerar föregående exempel en situation där tre enheter rapporteras lyckad konfiguration, två fortfarande använda hello konfiguration och en rapporterade ett fel.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-131">hello preceding example illustrates a situation where three devices reported successful configuration, two are still applying hello configuration, and one reported an error.</span></span>

### <a name="c-example"></a><span data-ttu-id="eb6f4-132">C#-exempel</span><span class="sxs-lookup"><span data-stu-id="eb6f4-132">C# example</span></span>
<span data-ttu-id="eb6f4-133">hello frågefunktioner som exponeras av hello [C# service SDK] [ lnk-hub-sdks] i hello **RegistryManager** klass.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-133">hello query functionality is exposed by hello [C# service SDK][lnk-hub-sdks] in hello **RegistryManager** class.</span></span>
<span data-ttu-id="eb6f4-134">Här är ett exempel på en enkel fråga:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-134">Here is an example of a simple query:</span></span>

```csharp
var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
while (query.HasMoreResults)
{
    var page = await query.GetNextAsTwinAsync();
    foreach (var twin in page)
    {
        // do work on twin object
    }
}
```

<span data-ttu-id="eb6f4-135">Observera hur hello **frågan** instans-objekt med en storlek (upp too1000) och sedan flera sidor kan hämtas genom att anropa hello **GetNextAsTwinAsync** metoder flera gånger.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-135">Note how hello **query** object is instantiated with a page size (up too1000), and then multiple pages can be retrieved by calling hello **GetNextAsTwinAsync** methods multiple times.</span></span>
<span data-ttu-id="eb6f4-136">Observera att hello frågeobjekt visar flera **nästa\***, beroende på hello deserialisering alternativet krävs av hello fråga, till exempel dubbla eller jobb enhetsobjekt, eller vanligt JSON toobe används när du använder projektioner.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-136">Note that hello query object exposes multiple **Next\***, depending on hello deserialization option required by hello query, such as device twin or job objects, or plain JSON toobe used when using projections.</span></span>

### <a name="nodejs-example"></a><span data-ttu-id="eb6f4-137">Node.js-exempel</span><span class="sxs-lookup"><span data-stu-id="eb6f4-137">Node.js example</span></span>
<span data-ttu-id="eb6f4-138">hello frågefunktioner som exponeras av hello [Azure IoT service SDK för Node.js] [ lnk-hub-sdks] i hello **registret** objekt.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-138">hello query functionality is exposed by hello [Azure IoT service SDK for Node.js][lnk-hub-sdks] in hello **Registry** object.</span></span>
<span data-ttu-id="eb6f4-139">Här är ett exempel på en enkel fråga:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-139">Here is an example of a simple query:</span></span>

```nodejs
var query = registry.createQuery('SELECT * FROM devices', 100);
var onResults = function(err, results) {
    if (err) {
        console.error('Failed toofetch hello results: ' + err.message);
    } else {
        // Do something with hello results
        results.forEach(function(twin) {
            console.log(twin.deviceId);
        });

        if (query.hasMoreResults) {
            query.nextAsTwin(onResults);
        }
    }
};
query.nextAsTwin(onResults);
```

<span data-ttu-id="eb6f4-140">Observera hur hello **frågan** instans-objekt med en storlek (upp too1000) och sedan flera sidor kan hämtas genom att anropa hello **nextAsTwin** metoder flera gånger.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-140">Note how hello **query** object is instantiated with a page size (up too1000), and then multiple pages can be retrieved by calling hello **nextAsTwin** methods multiple times.</span></span>
<span data-ttu-id="eb6f4-141">Observera att hello frågeobjekt visar flera **nästa\***, beroende på hello deserialisering alternativet krävs av hello fråga, till exempel dubbla eller jobb enhetsobjekt, eller vanligt JSON toobe används när du använder projektioner.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-141">Note that hello query object exposes multiple **next\***, depending on hello deserialization option required by hello query, such as device twin or job objects, or plain JSON toobe used when using projections.</span></span>

### <a name="limitations"></a><span data-ttu-id="eb6f4-142">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="eb6f4-142">Limitations</span></span>
> [!IMPORTANT]
> <span data-ttu-id="eb6f4-143">Frågeresultatet kan ha några minuter för fördröjning med avseende toohello senaste värden i enheten twins.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-143">Query results can have a few minutes of delay with respect toohello latest values in device twins.</span></span> <span data-ttu-id="eb6f4-144">Om frågar enskild enhet twins-ID: t är det alltid bättre toouse hello hämta enheten dubbla API, som alltid innehåller hello senaste värden och som har högre throttling-begränsning.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-144">If querying individual device twins by id, it is always preferable toouse hello retrieve device twin API, which always contains hello latest values and has higher throttling limits.</span></span>

<span data-ttu-id="eb6f4-145">För närvarande jämförelser stöds endast mellan primitiva typer (inga objekt), till exempel `... WHERE properties.desired.config = properties.reported.config` stöds endast om dessa egenskaper har primitiva värden.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-145">Currently, comparisons are supported only between primitive types (no objects), for instance `... WHERE properties.desired.config = properties.reported.config` is supported only if those properties have primitive values.</span></span>

## <a name="get-started-with-jobs-queries"></a><span data-ttu-id="eb6f4-146">Kom igång med jobb-frågor</span><span class="sxs-lookup"><span data-stu-id="eb6f4-146">Get started with jobs queries</span></span>
<span data-ttu-id="eb6f4-147">[Jobb] [ lnk-jobs] ger ett sätt tooexecute åtgärder på uppsättningar av enheter.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-147">[Jobs][lnk-jobs] provide a way tooexecute operations on sets of devices.</span></span> <span data-ttu-id="eb6f4-148">Varje enhet dubbla innehåller information om hello hello jobb som den är en del i en samling som heter **jobb**.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-148">Each device twin contains hello information of hello jobs of which it is part in a collection called **jobs**.</span></span>
<span data-ttu-id="eb6f4-149">Logiskt</span><span class="sxs-lookup"><span data-stu-id="eb6f4-149">Logically,</span></span>

```json
{
    "deviceId": "myDeviceId",
    "etag": "AAAAAAAAAAc=",
    "tags": {
        ...
    },
    "properties": {
        ...
    },
    "jobs": [
        {
            "deviceId": "myDeviceId",
            "jobId": "myJobId",
            "jobType": "scheduleTwinUpdate",
            "status": "completed",
            "startTimeUtc": "2016-09-29T18:18:52.7418462",
            "endTimeUtc": "2016-09-29T18:20:52.7418462",
            "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
            "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
            "outcome": {
                "deviceMethodResponse": null
            }
        },
        ...
    ]
}
```

<span data-ttu-id="eb6f4-150">Den här samlingen är för närvarande frågbar som **devices.jobs** i hello frågespråk för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-150">Currently, this collection is queryable as **devices.jobs** in hello IoT Hub query language.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eb6f4-151">För närvarande returneras hello jobb egenskap aldrig när du frågar enheten twins (frågor som innehåller 'från enheter-).</span><span class="sxs-lookup"><span data-stu-id="eb6f4-151">Currently, hello jobs property is never returned when querying device twins (that is, queries that contains 'FROM devices').</span></span> <span data-ttu-id="eb6f4-152">Endast kan nås direkt med frågor med `FROM devices.jobs`.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-152">It can only be accessed directly with queries using `FROM devices.jobs`.</span></span>
>
>

<span data-ttu-id="eb6f4-153">Till exempel tooget alla jobb (tidigare och schemalagda) som påverkar en enstaka enhet, kan du använda hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-153">For instance, tooget all jobs (past and scheduled) that affect a single device, you can use hello following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
```

<span data-ttu-id="eb6f4-154">Observera hur den här frågan ger hello enhetsspecifika status (och hello direkta metoden svar) för varje jobb som returneras.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-154">Note how this query provides hello device-specific status (and possibly hello direct method response) of each job returned.</span></span>
<span data-ttu-id="eb6f4-155">Det är också möjligt toofilter med valfri boolesk villkor för alla objektegenskaper i hello **devices.jobs** samling.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-155">It is also possible toofilter with arbitrary Boolean conditions on all object properties in hello **devices.jobs** collection.</span></span>
<span data-ttu-id="eb6f4-156">Hej exempelvis följande fråga:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-156">For instance, hello following query:</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.deviceId = 'myDeviceId'
    AND devices.jobs.jobType = 'scheduleTwinUpdate'
    AND devices.jobs.status = 'completed'
    AND devices.jobs.createdTimeUtc > '2016-09-01'
```

<span data-ttu-id="eb6f4-157">hämtar alla slutförts enheten dubbla Uppdateringsjobb för enheten **myDeviceId** som skapats efter September 2016.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-157">retrieves all completed device twin update jobs for device **myDeviceId** that were created after September 2016.</span></span>

<span data-ttu-id="eb6f4-158">Det är också möjligt tooretrieve hello per enhet resultat för ett enda jobb.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-158">It is also possible tooretrieve hello per-device outcomes of a single job.</span></span>

```sql
SELECT * FROM devices.jobs
WHERE devices.jobs.jobId = 'myJobId'
```

### <a name="limitations"></a><span data-ttu-id="eb6f4-159">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="eb6f4-159">Limitations</span></span>
<span data-ttu-id="eb6f4-160">För närvarande en sökning på **devices.jobs** stöder inte:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-160">Currently, queries on **devices.jobs** do not support:</span></span>

* <span data-ttu-id="eb6f4-161">Projektioner därför bara `SELECT *` är möjligt.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-161">Projections, therefore only `SELECT *` is possible.</span></span>
* <span data-ttu-id="eb6f4-162">Villkor som refererar toohello enheten dubbla i tillägg toojob egenskaper (se hello föregående avsnitt).</span><span class="sxs-lookup"><span data-stu-id="eb6f4-162">Conditions that refer toohello device twin in addition toojob properties (see hello preceding section).</span></span>
* <span data-ttu-id="eb6f4-163">Utföra aggregeringar, till exempel antal, avg, gruppera efter.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-163">Performing aggregations, such as count, avg, group by.</span></span>

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a><span data-ttu-id="eb6f4-164">Kom igång med enhet till moln meddelandet vägar frågeuttryck</span><span class="sxs-lookup"><span data-stu-id="eb6f4-164">Get started with device-to-cloud message routes query expressions</span></span>

<span data-ttu-id="eb6f4-165">Med hjälp av [enhet till moln vägar][lnk-devguide-messaging-routes], kan du konfigurera IoT-hubb toodispatch enhet till moln meddelanden toodifferent slutpunkter som är baserade på uttryck som utvärderas mot enskilda meddelanden.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-165">Using [device-to-cloud routes][lnk-devguide-messaging-routes], you can configure IoT Hub toodispatch device-to-cloud messages toodifferent endpoints based on expressions evaluated against individual messages.</span></span>

<span data-ttu-id="eb6f4-166">hello flödet [villkoret] [ lnk-query-expressions] använder hello samma IoT-hubb-frågespråket i villkoren för dubbla och jobb.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-166">hello route [condition][lnk-query-expressions] uses hello same IoT Hub query language as conditions in twin and job queries.</span></span> <span data-ttu-id="eb6f4-167">Väg villkor utvärderas på hello meddelandehuvuden och brödtext.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-167">Route conditions are evaluated on hello message headers and body.</span></span> <span data-ttu-id="eb6f4-168">Din routning frågeuttryck kan innebära endast meddelanderubriker, endast hello meddelandetexten, eller båda meddelande sidhuvuden och meddelande.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-168">Your routing query expression may involve only message headers, only hello message body, or both message headers and message body.</span></span> <span data-ttu-id="eb6f4-169">IoT-hubb förutsätter ett visst schema för hello sidhuvuden och meddelandetexten i ordning tooroute meddelanden och hello följande avsnitt beskrivs vad som krävs för IoT-hubb tooproperly flöde:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-169">IoT Hub assumes a specific schema for hello headers and message body in order tooroute messages, and hello following sections describe what is required for IoT Hub tooproperly route:</span></span>

### <a name="routing-on-message-headers"></a><span data-ttu-id="eb6f4-170">Routning på meddelanderubriker</span><span class="sxs-lookup"><span data-stu-id="eb6f4-170">Routing on message headers</span></span>

<span data-ttu-id="eb6f4-171">IoT-hubb förutsätter hello följande JSON-representation av meddelandehuvuden för meddelanderoutning:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-171">IoT Hub assumes hello following JSON representation of message headers for message routing:</span></span>

```json
{
    "$messageId": "",
    "$enqueuedTime": "",
    "$to": "",
    "$expiryTimeUtc": "",
    "$correlationId": "",
    "$userId": "",
    "$ack": "",
    "$connectionDeviceId": "",
    "$connectionDeviceGenerationId": "",
    "$connectionAuthMethod": "",
    "$content-type": "",
    "$content-encoding": "",

    "userProperty1": "",
    "userProperty2": ""
}
```

<span data-ttu-id="eb6f4-172">Meddelandet Systemegenskaper föregås hello `'$'` symbolen.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-172">Message system properties are prefixed with hello `'$'` symbol.</span></span>
<span data-ttu-id="eb6f4-173">Egenskaper för användare nås alltid med deras namn.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-173">User properties are always accessed with their name.</span></span> <span data-ttu-id="eb6f4-174">Om ett användarnamn för egenskapen händer toocoincide med en systemegenskap (exempelvis `$to`), hello användaregenskap hämtas med hello `$to` uttryck.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-174">If a user property name happens toocoincide with a system property (such as `$to`), hello user property will be retrieved with hello `$to` expression.</span></span>
<span data-ttu-id="eb6f4-175">Du kan alltid komma åt hello Systemegenskapen med hakparenteser `{}`: du kan exempelvis använda hello uttryck `{$to}` tooaccess hello Systemegenskapen `to`.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-175">You can always access hello system property using brackets `{}`: for instance, you can use hello expression `{$to}` tooaccess hello system property `to`.</span></span> <span data-ttu-id="eb6f4-176">Inom hakparenteser egenskapsnamn hämta alltid hello motsvarande Systemegenskapen.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-176">Bracketed property names always retrieve hello corresponding system property.</span></span>

<span data-ttu-id="eb6f4-177">Kom ihåg att egenskapsnamn är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-177">Remember that property names are case insensitive.</span></span>

> [!NOTE]
> <span data-ttu-id="eb6f4-178">Alla egenskaper är strängar.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-178">All message properties are strings.</span></span> <span data-ttu-id="eb6f4-179">Systemegenskaper, enligt beskrivningen i hello [Utvecklarhandbok][lnk-devguide-messaging-format], är inte tillgängliga toouse i frågor.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-179">System properties, as described in hello [developer guide][lnk-devguide-messaging-format], are currently not available toouse in queries.</span></span>
>

<span data-ttu-id="eb6f4-180">Om du använder till exempel en `messageType` egenskap, kanske du vill tooroute alla telemetri tooone slutpunkt och alla aviseringar tooanother slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-180">For example, if you use a `messageType` property, you might want tooroute all telemetry tooone endpoint, and all alerts tooanother endpoint.</span></span> <span data-ttu-id="eb6f4-181">Du kan skriva hello efter uttrycket tooroute hello telemetri:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-181">You can write hello following expression tooroute hello telemetry:</span></span>

```sql
messageType = 'telemetry'
```

<span data-ttu-id="eb6f4-182">Och hello efter uttrycket tooroute hello varningsmeddelanden:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-182">And hello following expression tooroute hello alert messages:</span></span>

```sql
messageType = 'alert'
```

<span data-ttu-id="eb6f4-183">Booleskt uttryck och funktioner stöds också.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-183">Boolean expressions and functions are also supported.</span></span> <span data-ttu-id="eb6f4-184">Den här funktionen kan du toodistinguish mellan allvarlighetsgrad, till exempel:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-184">This feature enables you toodistinguish between severity level, for example:</span></span>

```sql
messageType = 'alerts' AND as_number(severity) <= 2
```

<span data-ttu-id="eb6f4-185">Se toohello [uttryck och villkor] [ lnk-query-expressions] avsnittet hello fullständig lista över stöds operatorer och funktioner.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-185">Refer toohello [Expression and conditions][lnk-query-expressions] section for hello full list of supported operators and functions.</span></span>

### <a name="routing-on-message-bodies"></a><span data-ttu-id="eb6f4-186">Routning på meddelandetext</span><span class="sxs-lookup"><span data-stu-id="eb6f4-186">Routing on message bodies</span></span>

<span data-ttu-id="eb6f4-187">IoT-hubb kan endast vidarebefordra baserat på meddelandetexten innehållet om hello-meddelandetexten är korrekt formaterad JSON kodning i UTF-8, UTF-16 eller UTF-32.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-187">IoT Hub can only route based on message body contents if hello message body is properly formed JSON encoded in either UTF-8, UTF-16, or UTF-32.</span></span> <span data-ttu-id="eb6f4-188">Du måste ange hello innehållstypen för hello-meddelande för`application/json` och hello innehåll kodning tooone hello stöds UTF-kodningar i hello meddelandet huvuden tooallow IoT-hubb tooroute hello-meddelande utifrån hello brödtext innehållet.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-188">You must set hello content type of hello message too`application/json` and hello content encoding tooone of hello supported UTF encodings in hello message headers tooallow IoT Hub tooroute hello message based on hello body contents.</span></span> <span data-ttu-id="eb6f4-189">Om någon av hello huvuden anges försöker IoT-hubben inte tooevaluate alla frågeuttryck som involverar hello brödtext mot hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-189">If either of hello headers is not specified, IoT Hub will not attempt tooevaluate any query expression involving hello body against hello message.</span></span> <span data-ttu-id="eb6f4-190">Om meddelandet inte är ett JSON-meddelande eller om hello-meddelande inte anger hello content-type och Innehållskodning, kan du fortfarande använda meddelanderoutning baserat på hello meddelandehuvuden tooroute hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-190">If your message is not a JSON message, or if hello message does not specify hello content type and content encoding, you may still use message routing tooroute hello message based on hello message headers.</span></span>

<span data-ttu-id="eb6f4-191">Du kan använda `$body` i hello fråga uttryck tooroute hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-191">You can use `$body` in hello query expression tooroute hello message.</span></span> <span data-ttu-id="eb6f4-192">Du kan använda en enkel body-referens, brödtext matrisreferens eller flera body-referenser i hello frågeuttryck.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-192">You can use a simple body reference, body array reference, or multiple body references in hello query expression.</span></span> <span data-ttu-id="eb6f4-193">Din frågeuttryck kan också kombinera en brödtext referens med en referens för sidhuvudet.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-193">Your query expression can also combine a body reference with a message header reference.</span></span> <span data-ttu-id="eb6f4-194">Hello följande är till exempel alla giltig fråga uttryck:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-194">For example, hello following are all valid query expressions:</span></span>

```sql
$body.message.Weather.Location.State = 'WA'
$body.Weather.HistoricalData[0].Month = 'Feb'
$body.Weather.Temperature = 50 AND $body.message.Weather.IsEnabled
length($body.Weather.Location.State) = 2
$body.Weather.Temperature = 50 AND Status = 'Active'
```

## <a name="basics-of-an-iot-hub-query"></a><span data-ttu-id="eb6f4-195">Grunderna i en fråga för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="eb6f4-195">Basics of an IoT Hub query</span></span>
<span data-ttu-id="eb6f4-196">Alla IoT-hubb fråga består av en väljer och från-satser och av valfri var och en GROUP BY satser.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-196">Every IoT Hub query consists of a SELECT and FROM clauses and by optional WHERE and GROUP BY clauses.</span></span> <span data-ttu-id="eb6f4-197">Varje fråga körs på en mängd JSON-dokument, till exempel enhet twins.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-197">Every query is run on a collection of JSON documents, for example device twins.</span></span> <span data-ttu-id="eb6f4-198">hello FROM-satsen anger hello dokumentet samling toobe hävdade på (**enheter** eller **devices.jobs**).</span><span class="sxs-lookup"><span data-stu-id="eb6f4-198">hello FROM clause indicates hello document collection toobe iterated on (**devices** or **devices.jobs**).</span></span> <span data-ttu-id="eb6f4-199">Sedan hello filter i hello där tillämpas.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-199">Then, hello filter in hello WHERE clause is applied.</span></span> <span data-ttu-id="eb6f4-200">Med aggregeringar, grupperas hello resultat i det här steget som anges i hello GROUP BY-sats och en rad genereras för varje grupp som anges i hello SELECT-satsen.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-200">With aggregations, hello results of this step are grouped as specified in hello GROUP BY clause and, for each group, a row is generated as specified in hello SELECT clause.</span></span>

```sql
SELECT <select_list>
FROM <from_specification>
[WHERE <filter_condition>]
[GROUP BY <group_specification>]
```

## <a name="from-clause"></a><span data-ttu-id="eb6f4-201">FROM-satsen</span><span class="sxs-lookup"><span data-stu-id="eb6f4-201">FROM clause</span></span>
<span data-ttu-id="eb6f4-202">Hej **från < from_specification >** satsen anta bara två värden: **från enheter**, tooquery enhet twins eller **från devices.jobs**, tooquery jobb per enhet information.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-202">hello **FROM <from_specification>** clause can assume only two values: **FROM devices**, tooquery device twins, or **FROM devices.jobs**, tooquery job per-device details.</span></span>

## <a name="where-clause"></a><span data-ttu-id="eb6f4-203">WHERE-satsen</span><span class="sxs-lookup"><span data-stu-id="eb6f4-203">WHERE clause</span></span>
<span data-ttu-id="eb6f4-204">Hej **där < filter_condition >** -satsen är valfria.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-204">hello **WHERE <filter_condition>** clause is optional.</span></span> <span data-ttu-id="eb6f4-205">Anger ett eller flera villkor att hello JSON-dokument i hello från samlingen måste uppfylla toobe som ingår i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-205">It specifies one or more conditions that hello JSON documents in hello FROM collection must satisfy toobe included as part of hello result.</span></span> <span data-ttu-id="eb6f4-206">JSON-dokument måste utvärderas hello anges villkor för ”true” toobe som ingår i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-206">Any JSON document must evaluate hello specified conditions too"true" toobe included in hello result.</span></span>

<span data-ttu-id="eb6f4-207">hello tillåtna villkor som beskrivs i avsnittet [uttryck och villkor][lnk-query-expressions].</span><span class="sxs-lookup"><span data-stu-id="eb6f4-207">hello allowed conditions are described in section [Expressions and conditions][lnk-query-expressions].</span></span>

## <a name="select-clause"></a><span data-ttu-id="eb6f4-208">SELECT-satsen</span><span class="sxs-lookup"><span data-stu-id="eb6f4-208">SELECT clause</span></span>
<span data-ttu-id="eb6f4-209">hello SELECT-satsen (**väljer < select_list >**) är obligatoriskt och anger vilka värden hämtas från hello fråga.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-209">hello SELECT clause (**SELECT <select_list>**) is mandatory and specifies what values are retrieved from hello query.</span></span> <span data-ttu-id="eb6f4-210">Den anger hello JSON värden toobe toogenerate nya JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-210">It specifies hello JSON values toobe used toogenerate new JSON objects.</span></span>
<span data-ttu-id="eb6f4-211">För varje element i hello filtrerade (och alternativt grupperade) delmängd av hello från samlingen, hello Projektionsfasen genererar ett nytt JSON-objekt med hello värdena som anges i hello SELECT-satsen.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-211">For each element of hello filtered (and optionally grouped) subset of hello FROM collection, hello projection phase generates a new JSON object, constructed with hello values specified in hello SELECT clause.</span></span>

<span data-ttu-id="eb6f4-212">Nedan följer hello grammatik hello SELECT-satsen:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-212">Following is hello grammar of hello SELECT clause:</span></span>

```
SELECT [TOP <max number>] <projection list>

<projection_list> ::=
    '*'
    | <projection_element> AS alias [, <projection_element> AS alias]+

<projection_element> :==
    attribute_name
    | <projection_element> '.' attribute_name
    | <aggregate>

<aggregate> :==
    count()
    | avg(<projection_element>)
    | sum(<projection_element>)
    | min(<projection_element>)
    | max(<projection_element>)
```

<span data-ttu-id="eb6f4-213">där **attribute_name** refererar tooany-egenskapen för hello JSON-dokumentet i hello från samlingen.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-213">where **attribute_name** refers tooany property of hello JSON document in hello FROM collection.</span></span> <span data-ttu-id="eb6f4-214">Några exempel på SELECT-satser kan hittas i hello [komma igång med enheten dubbla frågor] [ lnk-query-getstarted] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-214">Some examples of SELECT clauses can be found in hello [Getting started with device twin queries][lnk-query-getstarted] section.</span></span>

<span data-ttu-id="eb6f4-215">För närvarande markeringen satser skiljer sig **Välj \***  stöds endast i sammanställd frågor för enheten twins.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-215">Currently, selection clauses different than **SELECT \*** are only supported in aggregate queries on device twins.</span></span>

## <a name="group-by-clause"></a><span data-ttu-id="eb6f4-216">GROUP BY-sats</span><span class="sxs-lookup"><span data-stu-id="eb6f4-216">GROUP BY clause</span></span>
<span data-ttu-id="eb6f4-217">Hej **GROUP BY < group_specification >** -satsen är ett valfritt steg som kan utföras när hello filter som angetts i hello WHERE-satsen och välj innan hello projektionen som anges i hello.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-217">hello **GROUP BY <group_specification>** clause is an optional step that can be executed after hello filter specified in hello WHERE clause, and before hello projection specified in hello SELECT.</span></span> <span data-ttu-id="eb6f4-218">Grupperas dokument baserat på hello-värdet för ett attribut.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-218">It groups documents based on hello value of an attribute.</span></span> <span data-ttu-id="eb6f4-219">Dessa grupper är används toogenerate samman värden som anges i hello SELECT-satsen.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-219">These groups are used toogenerate aggregated values as specified in hello SELECT clause.</span></span>

<span data-ttu-id="eb6f4-220">Ett exempel på en fråga med GROUP BY är:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-220">An example of a query using GROUP BY is:</span></span>

```sql
SELECT properties.reported.telemetryConfig.status AS status,
    COUNT() AS numberOfDevices
FROM devices
GROUP BY properties.reported.telemetryConfig.status
```

<span data-ttu-id="eb6f4-221">hello formella syntaxen för GROUP BY är:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-221">hello formal syntax for GROUP BY is:</span></span>

```
GROUP BY <group_by_element>
<group_by_element> :==
    attribute_name
    | < group_by_element > '.' attribute_name
```

<span data-ttu-id="eb6f4-222">där **attribute_name** refererar tooany-egenskapen för hello JSON-dokumentet i hello från samlingen.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-222">where **attribute_name** refers tooany property of hello JSON document in hello FROM collection.</span></span>

<span data-ttu-id="eb6f4-223">Hello-GROUP BY-sats stöds för närvarande bara när du frågar enheten twins.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-223">Currently, hello GROUP BY clause is only supported when querying device twins.</span></span>

## <a name="expressions-and-conditions"></a><span data-ttu-id="eb6f4-224">Uttryck och villkor</span><span class="sxs-lookup"><span data-stu-id="eb6f4-224">Expressions and conditions</span></span>
<span data-ttu-id="eb6f4-225">På en hög nivå, en *uttryck*:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-225">At a high level, an *expression*:</span></span>

* <span data-ttu-id="eb6f4-226">Utvärderar tooan instans av en JSON-typ (till exempel boolesk, tal, sträng, matris eller objekt) och</span><span class="sxs-lookup"><span data-stu-id="eb6f4-226">Evaluates tooan instance of a JSON type (such as Boolean, number, string, array, or object), and</span></span>
* <span data-ttu-id="eb6f4-227">Definieras av manipulera data från hello enheten JSON-dokumentet och konstanter med hjälp av inbyggda operatorer och funktioner.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-227">Is defined by manipulating data coming from hello device JSON document and constants using built-in operators and functions.</span></span>

<span data-ttu-id="eb6f4-228">*Villkor* är uttryck som utvärderas tooa booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-228">*Conditions* are expressions that evaluate tooa Boolean.</span></span> <span data-ttu-id="eb6f4-229">En konstant som skiljer sig boolesk **true** betraktas som **FALSKT** (inklusive **null**, **Odefinierad**, valfri objektet eller matrisen instans alla sträng och tydligt hello boolesk **FALSKT**).</span><span class="sxs-lookup"><span data-stu-id="eb6f4-229">Any constant different than Boolean **true** is considered as **false** (including **null**, **undefined**, any object or array instance, any string, and clearly hello Boolean **false**).</span></span>

<span data-ttu-id="eb6f4-230">hello syntaxen för uttryck är:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-230">hello syntax for expressions is:</span></span>

```
<expression> ::=
    <constant> |
    attribute_name |
    <function_call> |
    <expression> binary_operator <expression> |
    <create_array_expression> |
    '(' <expression> ')'

<function_call> ::=
    <function_name> '(' expression ')'

<constant> ::=
    <undefined_constant>
    | <null_constant>
    | <number_constant>
    | <string_constant>
    | <array_constant>

<undefined_constant> ::= undefined
<null_constant> ::= null
<number_constant> ::= decimal_literal | hexadecimal_literal
<string_constant> ::= string_literal
<array_constant> ::= '[' <constant> [, <constant>]+ ']'
```

<span data-ttu-id="eb6f4-231">Där:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-231">where:</span></span>

| <span data-ttu-id="eb6f4-232">Symbol</span><span class="sxs-lookup"><span data-stu-id="eb6f4-232">Symbol</span></span> | <span data-ttu-id="eb6f4-233">Definition</span><span class="sxs-lookup"><span data-stu-id="eb6f4-233">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="eb6f4-234">attribute_name</span><span class="sxs-lookup"><span data-stu-id="eb6f4-234">attribute_name</span></span> | <span data-ttu-id="eb6f4-235">En egenskap av hello JSON-dokumentet i hello **FROM** samling.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-235">Any property of hello JSON document in hello **FROM** collection.</span></span> |
| <span data-ttu-id="eb6f4-236">binary_operator</span><span class="sxs-lookup"><span data-stu-id="eb6f4-236">binary_operator</span></span> | <span data-ttu-id="eb6f4-237">En binär operator som anges i hello [operatörer](#operators) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-237">Any binary operator listed in hello [Operators](#operators) section.</span></span> |
| <span data-ttu-id="eb6f4-238">function_name</span><span class="sxs-lookup"><span data-stu-id="eb6f4-238">function_name</span></span>| <span data-ttu-id="eb6f4-239">En funktion som anges i hello [funktioner](#functions) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-239">Any function listed in hello [Functions](#functions) section.</span></span> |
| <span data-ttu-id="eb6f4-240">decimal_literal</span><span class="sxs-lookup"><span data-stu-id="eb6f4-240">decimal_literal</span></span> |<span data-ttu-id="eb6f4-241">Ett flyttal uttryckt i decimalformat.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-241">A float expressed in decimal notation.</span></span> |
| <span data-ttu-id="eb6f4-242">hexadecimal_literal</span><span class="sxs-lookup"><span data-stu-id="eb6f4-242">hexadecimal_literal</span></span> |<span data-ttu-id="eb6f4-243">Ett tal anges av hello sträng: 0 x' följt av en sträng med hexadecimala siffror.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-243">A number expressed by hello string ‘0x’ followed by a string of hexadecimal digits.</span></span> |
| <span data-ttu-id="eb6f4-244">string_literal</span><span class="sxs-lookup"><span data-stu-id="eb6f4-244">string_literal</span></span> |<span data-ttu-id="eb6f4-245">Stränglitteraler är Unicode-strängar som representeras av en följd av noll eller flera Unicode-tecken eller escape-sekvenser.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-245">String literals are Unicode strings represented by a sequence of zero or more Unicode characters or escape sequences.</span></span> <span data-ttu-id="eb6f4-246">Stränglitteraler omges av enkla citattecken (apostrof: ') eller dubbla citattecken (citattecken ”:).</span><span class="sxs-lookup"><span data-stu-id="eb6f4-246">String literals are enclosed in single quotes (apostrophe: ' ) or double quotes (quotation mark: ").</span></span> <span data-ttu-id="eb6f4-247">Tillåtna visar: `\'`, `\"`, `\\`, `\uXXXX` för Unicode-tecken som definieras av 4 hexadecimala siffror.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-247">Allowed escapes: `\'`, `\"`, `\\`, `\uXXXX` for Unicode characters defined by 4 hexadecimal digits.</span></span> |

### <a name="operators"></a><span data-ttu-id="eb6f4-248">Operatorer</span><span class="sxs-lookup"><span data-stu-id="eb6f4-248">Operators</span></span>
<span data-ttu-id="eb6f4-249">följande operatorer hello stöds:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-249">hello following operators are supported:</span></span>

| <span data-ttu-id="eb6f4-250">Familj</span><span class="sxs-lookup"><span data-stu-id="eb6f4-250">Family</span></span> | <span data-ttu-id="eb6f4-251">Operatorer</span><span class="sxs-lookup"><span data-stu-id="eb6f4-251">Operators</span></span> |
| --- | --- |
| <span data-ttu-id="eb6f4-252">Aritmetiska</span><span class="sxs-lookup"><span data-stu-id="eb6f4-252">Arithmetic</span></span> |<span data-ttu-id="eb6f4-253">+,-,*,/,%</span><span class="sxs-lookup"><span data-stu-id="eb6f4-253">+,-,*,/,%</span></span> |
| <span data-ttu-id="eb6f4-254">Logiska</span><span class="sxs-lookup"><span data-stu-id="eb6f4-254">Logical</span></span> |<span data-ttu-id="eb6f4-255">OCH, ELLER INTE</span><span class="sxs-lookup"><span data-stu-id="eb6f4-255">AND, OR, NOT</span></span> |
| <span data-ttu-id="eb6f4-256">Jämförelse</span><span class="sxs-lookup"><span data-stu-id="eb6f4-256">Comparison</span></span> |<span data-ttu-id="eb6f4-257">=, !=, <, >, <=, >=, <></span><span class="sxs-lookup"><span data-stu-id="eb6f4-257">=, !=, <, >, <=, >=, <></span></span> |

### <a name="functions"></a><span data-ttu-id="eb6f4-258">Funktioner</span><span class="sxs-lookup"><span data-stu-id="eb6f4-258">Functions</span></span>
<span data-ttu-id="eb6f4-259">När du frågar jobb och twins hello stöds endast är funktionen:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-259">When querying twins and jobs hello only supported function is:</span></span>

| <span data-ttu-id="eb6f4-260">Funktionen</span><span class="sxs-lookup"><span data-stu-id="eb6f4-260">Function</span></span> | <span data-ttu-id="eb6f4-261">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="eb6f4-261">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="eb6f4-262">IS_DEFINED(Property)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-262">IS_DEFINED(property)</span></span> | <span data-ttu-id="eb6f4-263">Returnerar ett booleskt värde som anger om egenskapen hello har tilldelats ett värde (inklusive `null`).</span><span class="sxs-lookup"><span data-stu-id="eb6f4-263">Returns a Boolean indicating if hello property has been assigned a value (including `null`).</span></span> |

<span data-ttu-id="eb6f4-264">Hello följande matematiska funktioner stöds i vägar villkor:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-264">In routes conditions, hello following math functions are supported:</span></span>

| <span data-ttu-id="eb6f4-265">Funktionen</span><span class="sxs-lookup"><span data-stu-id="eb6f4-265">Function</span></span> | <span data-ttu-id="eb6f4-266">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="eb6f4-266">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="eb6f4-267">ABS(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-267">ABS(x)</span></span> | <span data-ttu-id="eb6f4-268">Returnerar hello absolut (positivt) värdet för hello angetts numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-268">Returns hello absolute (positive) value of hello specified numeric expression.</span></span> |
| <span data-ttu-id="eb6f4-269">EXP(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-269">EXP(x)</span></span> | <span data-ttu-id="eb6f4-270">Returnerar hello exponentiell värdet för hello angivna numeriska uttrycket (e ^ x).</span><span class="sxs-lookup"><span data-stu-id="eb6f4-270">Returns hello exponential value of hello specified numeric expression (e^x).</span></span> |
| <span data-ttu-id="eb6f4-271">Power(x,y)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-271">POWER(x,y)</span></span> | <span data-ttu-id="eb6f4-272">Returnerar hello hello anges värdet uttryck toohello angetts power (x ^ y).</span><span class="sxs-lookup"><span data-stu-id="eb6f4-272">Returns hello value of hello specified expression toohello specified power (x^y).</span></span>|
| <span data-ttu-id="eb6f4-273">Square(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-273">SQUARE(x)</span></span> | <span data-ttu-id="eb6f4-274">Returnerar hello rektangulär hello angetts numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-274">Returns hello square of hello specified numeric value.</span></span> |
| <span data-ttu-id="eb6f4-275">CEILING(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-275">CEILING(x)</span></span> | <span data-ttu-id="eb6f4-276">Returnerar hello minsta heltal som är större än eller lika med för att hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-276">Returns hello smallest integer value greater than, or equal to, hello specified numeric expression.</span></span> |
| <span data-ttu-id="eb6f4-277">FLOOR(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-277">FLOOR(x)</span></span> | <span data-ttu-id="eb6f4-278">Returnerar hello största heltal som är mindre än eller lika toohello angivna numeriska uttrycket.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-278">Returns hello largest integer less than or equal toohello specified numeric expression.</span></span> |
| <span data-ttu-id="eb6f4-279">Sign(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-279">SIGN(x)</span></span> | <span data-ttu-id="eb6f4-280">Returnerar hello positiv (+ 1), noll (0) eller minustecken (-1) för hello angetts numeriskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-280">Returns hello positive (+1), zero (0), or negative (-1) sign of hello specified numeric expression.</span></span>|
| <span data-ttu-id="eb6f4-281">Rot(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-281">SQRT(x)</span></span> | <span data-ttu-id="eb6f4-282">Returnerar hello rektangulär hello angetts numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-282">Returns hello square of hello specified numeric value.</span></span> |

<span data-ttu-id="eb6f4-283">Hello efter typ kontrollerar och kastar funktioner stöds i vägar villkor:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-283">In routes conditions, hello following type checking and casting functions are supported:</span></span>

| <span data-ttu-id="eb6f4-284">Funktionen</span><span class="sxs-lookup"><span data-stu-id="eb6f4-284">Function</span></span> | <span data-ttu-id="eb6f4-285">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="eb6f4-285">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="eb6f4-286">AS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="eb6f4-286">AS_NUMBER</span></span> | <span data-ttu-id="eb6f4-287">Konverterar hello Indatasträngen tooa tal.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-287">Converts hello input string tooa number.</span></span> <span data-ttu-id="eb6f4-288">`noop`Om indata är ett tal. `Undefined` om strängen inte representerar ett tal.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-288">`noop` if input is a number; `Undefined` if string does not represent a number.</span></span>|
| <span data-ttu-id="eb6f4-289">IS_ARRAY</span><span class="sxs-lookup"><span data-stu-id="eb6f4-289">IS_ARRAY</span></span> | <span data-ttu-id="eb6f4-290">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är en matris.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-290">Returns a Boolean value indicating if hello type of hello specified expression is an array.</span></span> |
| <span data-ttu-id="eb6f4-291">IS_BOOL</span><span class="sxs-lookup"><span data-stu-id="eb6f4-291">IS_BOOL</span></span> | <span data-ttu-id="eb6f4-292">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-292">Returns a Boolean value indicating if hello type of hello specified expression is a Boolean.</span></span> |
| <span data-ttu-id="eb6f4-293">IS_DEFINED</span><span class="sxs-lookup"><span data-stu-id="eb6f4-293">IS_DEFINED</span></span> | <span data-ttu-id="eb6f4-294">Returnerar ett booleskt värde som anger om egenskapen hello har tilldelats ett värde.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-294">Returns a Boolean indicating if hello property has been assigned a value.</span></span> |
| <span data-ttu-id="eb6f4-295">IS_NULL</span><span class="sxs-lookup"><span data-stu-id="eb6f4-295">IS_NULL</span></span> | <span data-ttu-id="eb6f4-296">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är null.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-296">Returns a Boolean value indicating if hello type of hello specified expression is null.</span></span> |
| <span data-ttu-id="eb6f4-297">IS_NUMBER</span><span class="sxs-lookup"><span data-stu-id="eb6f4-297">IS_NUMBER</span></span> | <span data-ttu-id="eb6f4-298">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett tal.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-298">Returns a Boolean value indicating if hello type of hello specified expression is a number.</span></span> |
| <span data-ttu-id="eb6f4-299">IS_OBJECT</span><span class="sxs-lookup"><span data-stu-id="eb6f4-299">IS_OBJECT</span></span> | <span data-ttu-id="eb6f4-300">Returnerar ett booleskt värde som anger om hello typ av hello angivna uttrycket är ett JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-300">Returns a Boolean value indicating if hello type of hello specified expression is a JSON object.</span></span> |
| <span data-ttu-id="eb6f4-301">IS_PRIMITIVE</span><span class="sxs-lookup"><span data-stu-id="eb6f4-301">IS_PRIMITIVE</span></span> | <span data-ttu-id="eb6f4-302">Returnerar ett booleskt värde som anger om hello hello angetts uttrycket är en primitiv (string, Boolean, numeriska eller `null`).</span><span class="sxs-lookup"><span data-stu-id="eb6f4-302">Returns a Boolean value indicating if hello type of hello specified expression is a primitive (string, Boolean, numeric or `null`).</span></span> |
| <span data-ttu-id="eb6f4-303">IS_STRING</span><span class="sxs-lookup"><span data-stu-id="eb6f4-303">IS_STRING</span></span> | <span data-ttu-id="eb6f4-304">Returnerar ett booleskt värde som anger om hello hello angetts uttryck är en sträng.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-304">Returns a Boolean value indicating if hello type of hello specified expression is a string.</span></span> |

<span data-ttu-id="eb6f4-305">Hello följande strängfunktioner stöds i vägar villkor:</span><span class="sxs-lookup"><span data-stu-id="eb6f4-305">In routes conditions, hello following string functions are supported:</span></span>

| <span data-ttu-id="eb6f4-306">Funktionen</span><span class="sxs-lookup"><span data-stu-id="eb6f4-306">Function</span></span> | <span data-ttu-id="eb6f4-307">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="eb6f4-307">Description</span></span> |
| -------- | ----------- |
| <span data-ttu-id="eb6f4-308">CONCAT(x,...)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-308">CONCAT(x, …)</span></span> | <span data-ttu-id="eb6f4-309">Returnerar en sträng som är hello resultat sammanfoga två eller flera strängvärden.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-309">Returns a string that is hello result of concatenating two or more string values.</span></span> |
| <span data-ttu-id="eb6f4-310">LENGTH(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-310">LENGTH(x)</span></span> | <span data-ttu-id="eb6f4-311">Returnerar hello antalet tecken i hello angetts stränguttryck.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-311">Returns hello number of characters of hello specified string expression.</span></span>|
| <span data-ttu-id="eb6f4-312">LOWER(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-312">LOWER(x)</span></span> | <span data-ttu-id="eb6f4-313">Returnerar ett stränguttryck efter konvertering versal data toolowercase.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-313">Returns a string expression after converting uppercase character data toolowercase.</span></span> |
| <span data-ttu-id="eb6f4-314">UPPER(x)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-314">UPPER(x)</span></span> | <span data-ttu-id="eb6f4-315">Returnerar ett stränguttryck efter konvertering gemen data toouppercase.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-315">Returns a string expression after converting lowercase character data toouppercase.</span></span> |
| <span data-ttu-id="eb6f4-316">SUBSTRING (sträng, start [, längd])</span><span class="sxs-lookup"><span data-stu-id="eb6f4-316">SUBSTRING(string, start [, length])</span></span> | <span data-ttu-id="eb6f4-317">Returnerar en del av ett stränguttryck som börjar vid hello angetts nollbaserade teckenposition och fortsätter toohello angiven längd eller toohello slutet av hello strängen.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-317">Returns part of a string expression starting at hello specified character zero-based position and continues toohello specified length, or toohello end of hello string.</span></span> |
| <span data-ttu-id="eb6f4-318">INDEX_OF (string, fragment)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-318">INDEX_OF(string, fragment)</span></span> | <span data-ttu-id="eb6f4-319">Returnerar hello startposition för hello första förekomsten av hello andra stränguttryck inom hello första angivet stränguttryck eller -1 om hello strängen inte hittas.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-319">Returns hello starting position of hello first occurrence of hello second string expression within hello first specified string expression, or -1 if hello string is not found.</span></span>|
| <span data-ttu-id="eb6f4-320">STARTS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-320">STARTS_WITH(x, y)</span></span> | <span data-ttu-id="eb6f4-321">Returnerar ett booleskt värde som anger om hello första stränguttryck börjar med hello andra.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-321">Returns a Boolean indicating whether hello first string expression starts with hello second.</span></span> |
| <span data-ttu-id="eb6f4-322">ENDS_WITH (x, y)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-322">ENDS_WITH(x, y)</span></span> | <span data-ttu-id="eb6f4-323">Returnerar ett booleskt värde som anger om hello första stränguttryck slutar med hello andra.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-323">Returns a Boolean indicating whether hello first string expression ends with hello second.</span></span> |
| <span data-ttu-id="eb6f4-324">CONTAINS(x,y)</span><span class="sxs-lookup"><span data-stu-id="eb6f4-324">CONTAINS(x,y)</span></span> | <span data-ttu-id="eb6f4-325">Returnerar ett booleskt värde som anger om hello första stränguttryck innehåller hello andra.</span><span class="sxs-lookup"><span data-stu-id="eb6f4-325">Returns a Boolean indicating whether hello first string expression contains hello second.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="eb6f4-326">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eb6f4-326">Next steps</span></span>
<span data-ttu-id="eb6f4-327">Lär dig hur tooexecute frågor i dina appar med hjälp av [Azure IoT SDK][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="eb6f4-327">Learn how tooexecute queries in your apps using [Azure IoT SDKs][lnk-hub-sdks].</span></span>

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messages-read-custom.md
[lnk-devguide-messaging-format]: iot-hub-devguide-messages-construct.md
[lnk-devguide-messaging-routes]: ./iot-hub-devguide-messages-read-custom.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md

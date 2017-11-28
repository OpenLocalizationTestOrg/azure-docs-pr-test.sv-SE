---
title: aaaUnderstand Azure IoT Hub direkt metoder | Microsoft Docs
description: "Utvecklarhandbok - Använd direkt metoder tooinvoke kod på dina enheter från en app service."
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d15d44a0c3e1d1cda1669c1ed011c2f932e3d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a><span data-ttu-id="02251-103">Förstå och anropa direkt metoder från IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="02251-103">Understand and invoke direct methods from IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="02251-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="02251-104">Overview</span></span>
<span data-ttu-id="02251-105">IoT-hubb ger dig möjlighet tooinvoke direkt metoder på enheter från hello molnet.</span><span class="sxs-lookup"><span data-stu-id="02251-105">IoT Hub gives you ability tooinvoke direct methods on devices from hello cloud.</span></span> <span data-ttu-id="02251-106">Direkta metoder representerar en request-reply-interaktion med en enhet liknande tooan HTTP anropa i att de lyckas eller misslyckas omedelbart (efter en användardefinierade timeout).</span><span class="sxs-lookup"><span data-stu-id="02251-106">Direct methods represent a request-reply interaction with a device similar tooan HTTP call in that they succeed or fail immediately (after a user-specified timeout).</span></span> <span data-ttu-id="02251-107">Detta är användbart för scenarier där hello loppet av omedelbara åtgärder är olika beroende på om hello enheten var kan toorespond, till exempel skicka ett SMS-wake-up tooa enheten om en enhet är offline (SMS är dyrare än ett metodanrop).</span><span class="sxs-lookup"><span data-stu-id="02251-107">This is useful for scenarios where hello course of immediate action is different depending on whether hello device was able toorespond, such as sending an SMS wake-up tooa device if a device is offline (SMS being more expensive than a method call).</span></span>

<span data-ttu-id="02251-108">Varje enhet metod riktar sig till en enda enhet.</span><span class="sxs-lookup"><span data-stu-id="02251-108">Each device method targets a single device.</span></span> <span data-ttu-id="02251-109">[Jobb] [ lnk-devguide-jobs] ger ett sätt tooinvoke direkt metoder på flera enheter och schemalägga metodanropet för frånkopplade enheter.</span><span class="sxs-lookup"><span data-stu-id="02251-109">[Jobs][lnk-devguide-jobs] provide a way tooinvoke direct methods on multiple devices, and schedule method invocation for disconnected devices.</span></span>

<span data-ttu-id="02251-110">Alla med **tjänsten ansluta** behörigheter för IoT-hubb kan anropa en metod på en enhet.</span><span class="sxs-lookup"><span data-stu-id="02251-110">Anyone with **service connect** permissions on IoT Hub may invoke a method on a device.</span></span>

### <a name="when-toouse"></a><span data-ttu-id="02251-111">När toouse</span><span class="sxs-lookup"><span data-stu-id="02251-111">When toouse</span></span>
<span data-ttu-id="02251-112">Direkta metoder följer ett mönster i begäran och svar och är avsedda för kommunikation som kräver omedelbar bekräftelse av deras resultat, vanligtvis interaktiv kontroll av hello enhet, till exempel tooturn på fläktar.</span><span class="sxs-lookup"><span data-stu-id="02251-112">Direct methods follow a request-response pattern and are meant for communications that require immediate confirmation of their result, usually interactive control of hello device, for example tooturn on a fan.</span></span>

<span data-ttu-id="02251-113">Se för[moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] dirigera om osäkra mellan med hjälp av egenskaper, metoder eller moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="02251-113">Refer too[Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="method-lifecycle"></a><span data-ttu-id="02251-114">Metoden livscykel</span><span class="sxs-lookup"><span data-stu-id="02251-114">Method lifecycle</span></span>
<span data-ttu-id="02251-115">Direkta metoder implementeras på hello enhet och kan kräva noll eller fler indata i hello metoden nyttolast toocorrectly instansiera.</span><span class="sxs-lookup"><span data-stu-id="02251-115">Direct methods are implemented on hello device and may require zero or more inputs in hello method payload toocorrectly instantiate.</span></span> <span data-ttu-id="02251-116">Du anropa en metod som har direkt via en tjänst-riktade URI (`{iot hub}/twins/{device id}/methods/`).</span><span class="sxs-lookup"><span data-stu-id="02251-116">You invoke a direct method through a service-facing URI (`{iot hub}/twins/{device id}/methods/`).</span></span> <span data-ttu-id="02251-117">En enhet tar emot direkt metoder igenom avsnittet enhetsspecifika MQTT (`$iothub/methods/POST/{method name}/`).</span><span class="sxs-lookup"><span data-stu-id="02251-117">A device receives direct methods through a device-specific MQTT topic (`$iothub/methods/POST/{method name}/`).</span></span> <span data-ttu-id="02251-118">Vi stöder direkt metoder på ytterligare enheter på klientsidan nätverksprotokoll i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="02251-118">We may support direct methods on additional device-side networking protocols in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="02251-119">När du anropar en direkt metod på en enhet, egenskapsnamn och värden kan endast innehålla US-ASCII utskrivbara alfanumeriskt, förutom eventuella i hello följande uppsättning: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="02251-119">When you invoke a direct method on a device, property names and values can only contain US-ASCII printable alphanumeric, except any in hello following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

<span data-ttu-id="02251-120">Direkta metoder är synkrona och antingen lyckas eller misslyckas efter hello tidsgränsen (standard: 30 sekunder, går in too3600 sekunder).</span><span class="sxs-lookup"><span data-stu-id="02251-120">Direct methods are synchronous and either succeed or fail after hello timeout period (default: 30 seconds, settable up too3600 seconds).</span></span> <span data-ttu-id="02251-121">Direkta metoder är användbara i interaktiva scenarier där du vill att en enhet tooact om hello enheten är online och ta emot kommandon, som att slå på en enstaka via telefonen.</span><span class="sxs-lookup"><span data-stu-id="02251-121">Direct methods are useful in interactive scenarios where you want a device tooact if and only if hello device is online and receiving commands, such as turning on a light from a phone.</span></span> <span data-ttu-id="02251-122">I så fall kan du toosee en omedelbar lyckad eller misslyckad så hello Molntjänsten kan fungera på hello resultatet så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="02251-122">In these scenarios, you want toosee an immediate success or failure so hello cloud service can act on hello result as soon as possible.</span></span> <span data-ttu-id="02251-123">hello-enhet kan returnera vissa meddelandetexten på grund av hello-metoden, men det inte behövs för hello metoden toodo så.</span><span class="sxs-lookup"><span data-stu-id="02251-123">hello device may return some message body as a result of hello method, but it isn't required for hello method toodo so.</span></span> <span data-ttu-id="02251-124">Det finns ingen garanti på sortering eller alla samtidighet semantik på metodanrop.</span><span class="sxs-lookup"><span data-stu-id="02251-124">There is no guarantee on ordering or any concurrency semantics on method calls.</span></span>

<span data-ttu-id="02251-125">Direkta metoden är HTTP-only från hello molnet sida och MQTT endast från hello enhetens sida.</span><span class="sxs-lookup"><span data-stu-id="02251-125">Direct method are HTTP-only from hello cloud side, and MQTT-only from hello device side.</span></span>

<span data-ttu-id="02251-126">hello nyttolasten för metodbegäranden och svar är en JSON-dokumentet upp too8KB.</span><span class="sxs-lookup"><span data-stu-id="02251-126">hello payload for method requests and responses is a JSON document up too8KB.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="02251-127">Referensinformation:</span><span class="sxs-lookup"><span data-stu-id="02251-127">Reference topics:</span></span>
<span data-ttu-id="02251-128">hello ger följande Referensinformation dig mer information om hur du använder direkta metoder.</span><span class="sxs-lookup"><span data-stu-id="02251-128">hello following reference topics provide you with more information about using direct methods.</span></span>

## <a name="invoke-a-direct-method-from-a-back-end-app"></a><span data-ttu-id="02251-129">Anropa en metod som är direkt från en backend-app</span><span class="sxs-lookup"><span data-stu-id="02251-129">Invoke a direct method from a back-end app</span></span>
### <a name="method-invocation"></a><span data-ttu-id="02251-130">Metodanropet</span><span class="sxs-lookup"><span data-stu-id="02251-130">Method invocation</span></span>
<span data-ttu-id="02251-131">Direkt metod anrop på en enhet är HTTP-anrop som omfattar:</span><span class="sxs-lookup"><span data-stu-id="02251-131">Direct method invocations on a device are HTTP calls which comprise:</span></span>

* <span data-ttu-id="02251-132">Hej *URI* specifika toohello enhet (`{iot hub}/twins/{device id}/methods/`)</span><span class="sxs-lookup"><span data-stu-id="02251-132">hello *URI* specific toohello device (`{iot hub}/twins/{device id}/methods/`)</span></span>
* <span data-ttu-id="02251-133">hello POST *metod*</span><span class="sxs-lookup"><span data-stu-id="02251-133">hello POST *method*</span></span>
* <span data-ttu-id="02251-134">*Huvuden* som innehåller hello auktorisering, begär ID, content-type och Innehållskodning</span><span class="sxs-lookup"><span data-stu-id="02251-134">*Headers* which contain hello authorization, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="02251-135">En transparent JSON *brödtext* i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="02251-135">A transparent JSON *body* in hello following format:</span></span>

```
{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

<span data-ttu-id="02251-136">Tidsgränsen är i sekunder.</span><span class="sxs-lookup"><span data-stu-id="02251-136">Timeout is in seconds.</span></span> <span data-ttu-id="02251-137">Om tidsgränsen inte har angetts används som standard too30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="02251-137">If timeout is not set, it defaults too30 seconds.</span></span>

### <a name="response"></a><span data-ttu-id="02251-138">Svar</span><span class="sxs-lookup"><span data-stu-id="02251-138">Response</span></span>
<span data-ttu-id="02251-139">hello backend-app tar emot ett svar som omfattar:</span><span class="sxs-lookup"><span data-stu-id="02251-139">hello back-end app receives a response which comprises:</span></span>

* <span data-ttu-id="02251-140">*HTTP-statuskod*, som används för fel som kommer från hello IoT-hubb, inklusive ett 404-fel för enheter som för närvarande inte ansluten</span><span class="sxs-lookup"><span data-stu-id="02251-140">*HTTP status code*, which is used for errors coming from hello IoT Hub, including a 404 error for devices not currently connected</span></span>
* <span data-ttu-id="02251-141">*Huvuden* som innehåller hello ETag, begär ID, content-type och Innehållskodning</span><span class="sxs-lookup"><span data-stu-id="02251-141">*Headers* which contain hello ETag, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="02251-142">En JSON *brödtext* i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="02251-142">A JSON *body* in hello following format:</span></span>

```
{
    "status" : 201,
    "payload" : {...}
}
```

   <span data-ttu-id="02251-143">Båda `status` och `body` tillhandahålls av hello enheten och använda toorespond med hello enhetens egna statuskod och/eller beskrivning.</span><span class="sxs-lookup"><span data-stu-id="02251-143">Both `status` and `body` are provided by hello device and used toorespond with hello device's own status code and/or description.</span></span>

## <a name="handle-a-direct-method-on-a-device"></a><span data-ttu-id="02251-144">Hantera en direkt metod på en enhet</span><span class="sxs-lookup"><span data-stu-id="02251-144">Handle a direct method on a device</span></span>
### <a name="method-invocation"></a><span data-ttu-id="02251-145">Metodanropet</span><span class="sxs-lookup"><span data-stu-id="02251-145">Method invocation</span></span>
<span data-ttu-id="02251-146">Enheter får direkta metodbegäranden om hello MQTT avsnittet:`$iothub/methods/POST/{method name}/?$rid={request id}`</span><span class="sxs-lookup"><span data-stu-id="02251-146">Devices receive direct method requests on hello MQTT topic: `$iothub/methods/POST/{method name}/?$rid={request id}`</span></span>

<span data-ttu-id="02251-147">hello brödtext vilka hello enheten tar emot har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="02251-147">hello body which hello device receives is in hello following format:</span></span>

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

<span data-ttu-id="02251-148">Metodbegäranden är QoS 0.</span><span class="sxs-lookup"><span data-stu-id="02251-148">Method requests are QoS 0.</span></span>

### <a name="response"></a><span data-ttu-id="02251-149">Svar</span><span class="sxs-lookup"><span data-stu-id="02251-149">Response</span></span>
<span data-ttu-id="02251-150">hello enheten skickar svar för`$iothub/methods/res/{status}/?$rid={request id}`, där:</span><span class="sxs-lookup"><span data-stu-id="02251-150">hello device sends responses too`$iothub/methods/res/{status}/?$rid={request id}`, where:</span></span>

* <span data-ttu-id="02251-151">Hej `status` egenskapen är hello enheten angivna status för körning av metoden.</span><span class="sxs-lookup"><span data-stu-id="02251-151">hello `status` property is hello device-supplied status of method execution.</span></span>
* <span data-ttu-id="02251-152">Hej `$rid` egenskapen är hello begäran-ID från hello metodanropet togs emot från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="02251-152">hello `$rid` property is hello request ID from hello method invocation received from IoT Hub.</span></span>

<span data-ttu-id="02251-153">hello brödtext har angetts av hello enheten och kan vara status.</span><span class="sxs-lookup"><span data-stu-id="02251-153">hello body is set by hello device and can be any status.</span></span>

## <a name="additional-reference-material"></a><span data-ttu-id="02251-154">Ytterligare referensmaterialet</span><span class="sxs-lookup"><span data-stu-id="02251-154">Additional reference material</span></span>
<span data-ttu-id="02251-155">Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:</span><span class="sxs-lookup"><span data-stu-id="02251-155">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="02251-156">[IoT-hubbslutpunkter] [ lnk-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="02251-156">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="02251-157">[Begränsning och kvoter] [ lnk-quotas] beskriver hello kvoter som gäller toohello IoT-hubb-tjänsten och hello bandbreddsbegränsning beteende tooexpect när du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="02251-157">[Throttling and quotas][lnk-quotas] describes hello quotas that apply toohello IoT Hub service and hello throttling behavior tooexpect when you use hello service.</span></span>
* <span data-ttu-id="02251-158">[Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] visar hello olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="02251-158">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="02251-159">[IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning] [ lnk-query] beskriver hello IoT-hubb frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb.</span><span class="sxs-lookup"><span data-stu-id="02251-159">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes hello IoT Hub query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="02251-160">[Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för hello MQTT protokollet.</span><span class="sxs-lookup"><span data-stu-id="02251-160">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02251-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02251-161">Next steps</span></span>
<span data-ttu-id="02251-162">Nu har du lärt dig hur toouse direkt metoder, du kan vara intresserad hello följande IoT-hubb developer guide ämne:</span><span class="sxs-lookup"><span data-stu-id="02251-162">Now you have learned how toouse direct methods, you may be interested in hello following IoT Hub developer guide topic:</span></span>

* <span data-ttu-id="02251-163">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="02251-163">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="02251-164">Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb kursen:</span><span class="sxs-lookup"><span data-stu-id="02251-164">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorial:</span></span>

* <span data-ttu-id="02251-165">[Använda direct-metoder][lnk-methods-tutorial]</span><span class="sxs-lookup"><span data-stu-id="02251-165">[Use direct methods][lnk-methods-tutorial]</span></span>

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md

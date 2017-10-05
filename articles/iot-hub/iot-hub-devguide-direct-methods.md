---
title: "Förstå Azure IoT Hub direkt metoder | Microsoft Docs"
description: "Utvecklarhandbok - Använd direkt metoder för att anropa kod på dina enheter från en app service."
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
ms.openlocfilehash: 77e788a32097edbcb1cd4faaa45f35812eabd94a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a><span data-ttu-id="1f44c-103">Förstå och anropa direkt metoder från IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="1f44c-103">Understand and invoke direct methods from IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="1f44c-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1f44c-104">Overview</span></span>
<span data-ttu-id="1f44c-105">IoT-hubb kan du anropa direkt metoder på enheter från molnet.</span><span class="sxs-lookup"><span data-stu-id="1f44c-105">IoT Hub gives you ability to invoke direct methods on devices from the cloud.</span></span> <span data-ttu-id="1f44c-106">Direkta metoder representerar en request-reply-interaktion med en enhet som liknar ett HTTP-anrop i att de lyckas eller misslyckas omedelbart (efter en användardefinierade timeout).</span><span class="sxs-lookup"><span data-stu-id="1f44c-106">Direct methods represent a request-reply interaction with a device similar to an HTTP call in that they succeed or fail immediately (after a user-specified timeout).</span></span> <span data-ttu-id="1f44c-107">Detta är användbart för scenarier där loppet av omedelbara åtgärder är olika beroende på om enheten har kunna svara, till exempel skicka ett SMS-wake-up till en enhet om en enhet är offline (SMS är dyrare än ett metodanrop).</span><span class="sxs-lookup"><span data-stu-id="1f44c-107">This is useful for scenarios where the course of immediate action is different depending on whether the device was able to respond, such as sending an SMS wake-up to a device if a device is offline (SMS being more expensive than a method call).</span></span>

<span data-ttu-id="1f44c-108">Varje enhet metod riktar sig till en enda enhet.</span><span class="sxs-lookup"><span data-stu-id="1f44c-108">Each device method targets a single device.</span></span> <span data-ttu-id="1f44c-109">[Jobb] [ lnk-devguide-jobs] är ett sätt att anropa direkt metoder på flera enheter och schemalägga metodanropet för frånkopplade enheter.</span><span class="sxs-lookup"><span data-stu-id="1f44c-109">[Jobs][lnk-devguide-jobs] provide a way to invoke direct methods on multiple devices, and schedule method invocation for disconnected devices.</span></span>

<span data-ttu-id="1f44c-110">Alla med **tjänsten ansluta** behörigheter för IoT-hubb kan anropa en metod på en enhet.</span><span class="sxs-lookup"><span data-stu-id="1f44c-110">Anyone with **service connect** permissions on IoT Hub may invoke a method on a device.</span></span>

### <a name="when-to-use"></a><span data-ttu-id="1f44c-111">När du ska använda detta</span><span class="sxs-lookup"><span data-stu-id="1f44c-111">When to use</span></span>
<span data-ttu-id="1f44c-112">Direkta metoder följer ett mönster i begäran och svar och är avsedda för kommunikation som kräver omedelbar bekräftelse av deras resultat, vanligtvis interaktiv kontroll på enheten, till exempel för att aktivera en fläkt.</span><span class="sxs-lookup"><span data-stu-id="1f44c-112">Direct methods follow a request-response pattern and are meant for communications that require immediate confirmation of their result, usually interactive control of the device, for example to turn on a fan.</span></span>

<span data-ttu-id="1f44c-113">Referera till [moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] dirigera om osäkra mellan med hjälp av egenskaper, metoder eller moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1f44c-113">Refer to [Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="method-lifecycle"></a><span data-ttu-id="1f44c-114">Metoden livscykel</span><span class="sxs-lookup"><span data-stu-id="1f44c-114">Method lifecycle</span></span>
<span data-ttu-id="1f44c-115">Direkta metoder implementeras på enheten och kan kräva noll eller fler indata i metod nyttolasten att instansiera korrekt.</span><span class="sxs-lookup"><span data-stu-id="1f44c-115">Direct methods are implemented on the device and may require zero or more inputs in the method payload to correctly instantiate.</span></span> <span data-ttu-id="1f44c-116">Du anropa en metod som har direkt via en tjänst-riktade URI (`{iot hub}/twins/{device id}/methods/`).</span><span class="sxs-lookup"><span data-stu-id="1f44c-116">You invoke a direct method through a service-facing URI (`{iot hub}/twins/{device id}/methods/`).</span></span> <span data-ttu-id="1f44c-117">En enhet tar emot direkt metoder igenom avsnittet enhetsspecifika MQTT (`$iothub/methods/POST/{method name}/`).</span><span class="sxs-lookup"><span data-stu-id="1f44c-117">A device receives direct methods through a device-specific MQTT topic (`$iothub/methods/POST/{method name}/`).</span></span> <span data-ttu-id="1f44c-118">Vi stöder direkt metoder på ytterligare enheter på klientsidan nätverksprotokoll i framtiden.</span><span class="sxs-lookup"><span data-stu-id="1f44c-118">We may support direct methods on additional device-side networking protocols in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="1f44c-119">När du anropar en direkt metod på en enhet, egenskapsnamn och värden kan endast innehålla US-ASCII utskrivbara alfanumeriskt, förutom eventuella i följande: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="1f44c-119">When you invoke a direct method on a device, property names and values can only contain US-ASCII printable alphanumeric, except any in the following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

<span data-ttu-id="1f44c-120">Direkta metoder är synkrona och antingen lyckas eller misslyckas när tidsgränsen (standard: 30 sekunder, ange upp till 3 600 sekunder).</span><span class="sxs-lookup"><span data-stu-id="1f44c-120">Direct methods are synchronous and either succeed or fail after the timeout period (default: 30 seconds, settable up to 3600 seconds).</span></span> <span data-ttu-id="1f44c-121">Direkta metoder är användbara i interaktiva scenarier där du vill att en enhet så att den fungerar endast om enheten är online och ta emot kommandon, som att slå på en enstaka via telefonen.</span><span class="sxs-lookup"><span data-stu-id="1f44c-121">Direct methods are useful in interactive scenarios where you want a device to act if and only if the device is online and receiving commands, such as turning on a light from a phone.</span></span> <span data-ttu-id="1f44c-122">I dessa scenarier som du vill se en omedelbar lyckad eller misslyckad så Molntjänsten kan fungera på resultatet så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="1f44c-122">In these scenarios, you want to see an immediate success or failure so the cloud service can act on the result as soon as possible.</span></span> <span data-ttu-id="1f44c-123">Enheten kan returnera vissa meddelandetexten på grund av metoden, men det inte krävs att göra det-metoden.</span><span class="sxs-lookup"><span data-stu-id="1f44c-123">The device may return some message body as a result of the method, but it isn't required for the method to do so.</span></span> <span data-ttu-id="1f44c-124">Det finns ingen garanti på sortering eller alla samtidighet semantik på metodanrop.</span><span class="sxs-lookup"><span data-stu-id="1f44c-124">There is no guarantee on ordering or any concurrency semantics on method calls.</span></span>

<span data-ttu-id="1f44c-125">Direkta metoden är HTTP-only från molnet sida och MQTT endast från enheter-sidan.</span><span class="sxs-lookup"><span data-stu-id="1f44c-125">Direct method are HTTP-only from the cloud side, and MQTT-only from the device side.</span></span>

<span data-ttu-id="1f44c-126">Nyttolasten för metodbegäranden och svar är en JSON-dokumentet upp till 8KB.</span><span class="sxs-lookup"><span data-stu-id="1f44c-126">The payload for method requests and responses is a JSON document up to 8KB.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="1f44c-127">Referensinformation:</span><span class="sxs-lookup"><span data-stu-id="1f44c-127">Reference topics:</span></span>
<span data-ttu-id="1f44c-128">Följande referensavsnitt ge mer information om hur du använder direkta metoder.</span><span class="sxs-lookup"><span data-stu-id="1f44c-128">The following reference topics provide you with more information about using direct methods.</span></span>

## <a name="invoke-a-direct-method-from-a-back-end-app"></a><span data-ttu-id="1f44c-129">Anropa en metod som är direkt från en backend-app</span><span class="sxs-lookup"><span data-stu-id="1f44c-129">Invoke a direct method from a back-end app</span></span>
### <a name="method-invocation"></a><span data-ttu-id="1f44c-130">Metodanropet</span><span class="sxs-lookup"><span data-stu-id="1f44c-130">Method invocation</span></span>
<span data-ttu-id="1f44c-131">Direkt metod anrop på en enhet är HTTP-anrop som omfattar:</span><span class="sxs-lookup"><span data-stu-id="1f44c-131">Direct method invocations on a device are HTTP calls which comprise:</span></span>

* <span data-ttu-id="1f44c-132">Den *URI* specifik för enheten (`{iot hub}/twins/{device id}/methods/`)</span><span class="sxs-lookup"><span data-stu-id="1f44c-132">The *URI* specific to the device (`{iot hub}/twins/{device id}/methods/`)</span></span>
* <span data-ttu-id="1f44c-133">EFTER *metod*</span><span class="sxs-lookup"><span data-stu-id="1f44c-133">The POST *method*</span></span>
* <span data-ttu-id="1f44c-134">*Huvuden* som innehåller tillstånd, begär ID, content-type och Innehållskodning</span><span class="sxs-lookup"><span data-stu-id="1f44c-134">*Headers* which contain the authorization, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="1f44c-135">En transparent JSON *brödtext* i följande format:</span><span class="sxs-lookup"><span data-stu-id="1f44c-135">A transparent JSON *body* in the following format:</span></span>

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

<span data-ttu-id="1f44c-136">Tidsgränsen är i sekunder.</span><span class="sxs-lookup"><span data-stu-id="1f44c-136">Timeout is in seconds.</span></span> <span data-ttu-id="1f44c-137">Om inte tidsgränsen anges standardvärdet 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="1f44c-137">If timeout is not set, it defaults to 30 seconds.</span></span>

### <a name="response"></a><span data-ttu-id="1f44c-138">Svar</span><span class="sxs-lookup"><span data-stu-id="1f44c-138">Response</span></span>
<span data-ttu-id="1f44c-139">Backend-app tar emot ett svar som omfattar:</span><span class="sxs-lookup"><span data-stu-id="1f44c-139">The back-end app receives a response which comprises:</span></span>

* <span data-ttu-id="1f44c-140">*HTTP-statuskod*, som används för fel som kommer från IoT-hubben, inklusive ett 404-fel för enheter som för närvarande inte ansluten</span><span class="sxs-lookup"><span data-stu-id="1f44c-140">*HTTP status code*, which is used for errors coming from the IoT Hub, including a 404 error for devices not currently connected</span></span>
* <span data-ttu-id="1f44c-141">*Huvuden* som innehåller en ETag, begär ID, content-type och Innehållskodning</span><span class="sxs-lookup"><span data-stu-id="1f44c-141">*Headers* which contain the ETag, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="1f44c-142">En JSON *brödtext* i följande format:</span><span class="sxs-lookup"><span data-stu-id="1f44c-142">A JSON *body* in the following format:</span></span>

```
{
    "status" : 201,
    "payload" : {...}
}
```

   <span data-ttu-id="1f44c-143">Båda `status` och `body` som tillhandahålls av enheten och används för att svara med enhetens egna statuskod och/eller beskrivning.</span><span class="sxs-lookup"><span data-stu-id="1f44c-143">Both `status` and `body` are provided by the device and used to respond with the device's own status code and/or description.</span></span>

## <a name="handle-a-direct-method-on-a-device"></a><span data-ttu-id="1f44c-144">Hantera en direkt metod på en enhet</span><span class="sxs-lookup"><span data-stu-id="1f44c-144">Handle a direct method on a device</span></span>
### <a name="method-invocation"></a><span data-ttu-id="1f44c-145">Metodanropet</span><span class="sxs-lookup"><span data-stu-id="1f44c-145">Method invocation</span></span>
<span data-ttu-id="1f44c-146">Enheter får direkta metodbegäranden om MQTT avsnittet:`$iothub/methods/POST/{method name}/?$rid={request id}`</span><span class="sxs-lookup"><span data-stu-id="1f44c-146">Devices receive direct method requests on the MQTT topic: `$iothub/methods/POST/{method name}/?$rid={request id}`</span></span>

<span data-ttu-id="1f44c-147">Meddelandetexten som enheten tar emot är i följande format:</span><span class="sxs-lookup"><span data-stu-id="1f44c-147">The body which the device receives is in the following format:</span></span>

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

<span data-ttu-id="1f44c-148">Metodbegäranden är QoS 0.</span><span class="sxs-lookup"><span data-stu-id="1f44c-148">Method requests are QoS 0.</span></span>

### <a name="response"></a><span data-ttu-id="1f44c-149">Svar</span><span class="sxs-lookup"><span data-stu-id="1f44c-149">Response</span></span>
<span data-ttu-id="1f44c-150">Enheten skickar svar till `$iothub/methods/res/{status}/?$rid={request id}`, där:</span><span class="sxs-lookup"><span data-stu-id="1f44c-150">The device sends responses to `$iothub/methods/res/{status}/?$rid={request id}`, where:</span></span>

* <span data-ttu-id="1f44c-151">Den `status` egenskapen innebär att enheten har angett metod utförs.</span><span class="sxs-lookup"><span data-stu-id="1f44c-151">The `status` property is the device-supplied status of method execution.</span></span>
* <span data-ttu-id="1f44c-152">Den `$rid` egenskapen är begäran-ID från metodanropet togs emot från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="1f44c-152">The `$rid` property is the request ID from the method invocation received from IoT Hub.</span></span>

<span data-ttu-id="1f44c-153">Innehållet har angetts av enheten och kan vara status.</span><span class="sxs-lookup"><span data-stu-id="1f44c-153">The body is set by the device and can be any status.</span></span>

## <a name="additional-reference-material"></a><span data-ttu-id="1f44c-154">Ytterligare referensmaterialet</span><span class="sxs-lookup"><span data-stu-id="1f44c-154">Additional reference material</span></span>
<span data-ttu-id="1f44c-155">Andra referensavsnitten i utvecklarhandboken för IoT-hubben är:</span><span class="sxs-lookup"><span data-stu-id="1f44c-155">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="1f44c-156">[IoT-hubbslutpunkter] [ lnk-endpoints] beskriver de olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="1f44c-156">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="1f44c-157">[Begränsning och kvoter] [ lnk-quotas] beskriver kvoterna som gäller för IoT-hubb-tjänsten och bandbreddsbegränsning beteende som händer när du använder tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1f44c-157">[Throttling and quotas][lnk-quotas] describes the quotas that apply to the IoT Hub service and the throttling behavior to expect when you use the service.</span></span>
* <span data-ttu-id="1f44c-158">[Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] Listar olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="1f44c-158">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="1f44c-159">[IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning] [ lnk-query] beskriver IoT-hubb frågespråk som du kan använda för att hämta information från IoT-hubb om enheten twins och jobb.</span><span class="sxs-lookup"><span data-stu-id="1f44c-159">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes the IoT Hub query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="1f44c-160">[Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="1f44c-160">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f44c-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f44c-161">Next steps</span></span>
<span data-ttu-id="1f44c-162">Nu du har lärt dig hur du använder direkta metoder, kan du är intresserad av i följande avsnitt för IoT-hubb developer-guide:</span><span class="sxs-lookup"><span data-stu-id="1f44c-162">Now you have learned how to use direct methods, you may be interested in the following IoT Hub developer guide topic:</span></span>

* <span data-ttu-id="1f44c-163">[Schema-jobb på flera enheter][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="1f44c-163">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="1f44c-164">Om du vill testa vissa av de begrepp som beskrivs i den här artikeln får du är intresserad av följande IoT-hubb kursen:</span><span class="sxs-lookup"><span data-stu-id="1f44c-164">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorial:</span></span>

* <span data-ttu-id="1f44c-165">[Använda direct-metoder][lnk-methods-tutorial]</span><span class="sxs-lookup"><span data-stu-id="1f44c-165">[Use direct methods][lnk-methods-tutorial]</span></span>

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

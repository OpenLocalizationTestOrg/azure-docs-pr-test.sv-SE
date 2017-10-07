---
title: aaaUnderstand Azure IoT Hub-enhet till moln messaging | Microsoft Docs
description: "Utvecklarhandbok - hur toouse enhet till moln meddelanden med IoT-hubben. Innehåller information om hur du skickar data både telemetri och icke-telemtry vidarebefordra toodeliver meddelanden."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 07dc8a6be747365c7efbc528ab2762b0d9790758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-device-to-cloud-messages-tooiot-hub"></a><span data-ttu-id="27bd9-104">Skicka meddelanden från enhet till moln tooIoT Hub</span><span class="sxs-lookup"><span data-stu-id="27bd9-104">Send device-to-cloud messages tooIoT Hub</span></span>

<span data-ttu-id="27bd9-105">toosend tidsserier telemetri och varningar från dina enheter tooyour lösningens serverdel, skicka meddelanden från enhet till moln från din enhet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="27bd9-105">toosend time-series telemetry and alerts from your devices tooyour solution back end, send device-to-cloud messages from your device tooyour IoT hub.</span></span> <span data-ttu-id="27bd9-106">En beskrivning av andra enhet till moln-alternativ som stöds av IoT-hubb finns [enhet till moln kommunikation vägledning][lnk-d2c-guidance].</span><span class="sxs-lookup"><span data-stu-id="27bd9-106">For a discussion of other device-to-cloud options supported by IoT Hub, see [Device-to-cloud communications guidance][lnk-d2c-guidance].</span></span>

<span data-ttu-id="27bd9-107">Du skickar meddelanden från enhet till moln via en enhet riktade slutpunkt (**/devices/ {deviceId} / meddelanden/händelser**).</span><span class="sxs-lookup"><span data-stu-id="27bd9-107">You send device-to-cloud messages through a device-facing endpoint (**/devices/{deviceId}/messages/events**).</span></span> <span data-ttu-id="27bd9-108">Routningsregler och sedan dirigera dina meddelanden tooone hello service-riktade slutpunkter på din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="27bd9-108">Routing rules then route your messages tooone of hello service-facing endpoints on your IoT hub.</span></span> <span data-ttu-id="27bd9-109">Regler för Routning använder hello sidhuvuden och brödtext hälsningsmeddelande enhet till moln som passerar genom din hubb toodetermine där tooroute dem.</span><span class="sxs-lookup"><span data-stu-id="27bd9-109">Routing rules use hello headers and body of hello device-to-cloud messages flowing through your hub toodetermine where tooroute them.</span></span> <span data-ttu-id="27bd9-110">Som standard är meddelanden routade toohello inbyggd service-riktade slutpunkt (**meddelanden/händelser**), som är kompatibel med [Händelsehubbar][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="27bd9-110">By default, messages are routed toohello built-in service-facing endpoint (**messages/events**), that is compatible with [Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="27bd9-111">Därför kan du använda standard [Händelsehubbar integrering och SDK] [ lnk-compatible-endpoint] tooreceive meddelanden från enhet till moln i din lösning serverdel.</span><span class="sxs-lookup"><span data-stu-id="27bd9-111">Therefore, you can use standard [Event Hubs integration and SDKs][lnk-compatible-endpoint] tooreceive device-to-cloud messages in your solution back end.</span></span>

<span data-ttu-id="27bd9-112">IoT-hubb implementerar enhet till moln-meddelanden med hjälp av en strömmande meddelandemönster används.</span><span class="sxs-lookup"><span data-stu-id="27bd9-112">IoT Hub implements device-to-cloud messaging using a streaming messaging pattern.</span></span> <span data-ttu-id="27bd9-113">IoT-hubb meddelanden från enhet till moln är mer som [Händelsehubbar] [ lnk-event-hubs] *händelser* än [Service Bus] [ lnk-servicebus] *meddelanden* att en stor volym med händelser som passerar genom hello-tjänst som kan läsas av flera läsare.</span><span class="sxs-lookup"><span data-stu-id="27bd9-113">IoT Hub's device-to-cloud messages are more like [Event Hubs][lnk-event-hubs] *events* than [Service Bus][lnk-servicebus] *messages* in that there is a high volume of events passing through hello service that can be read by multiple readers.</span></span>

<span data-ttu-id="27bd9-114">Enhet till moln meddelanden med IoT-hubben har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="27bd9-114">Device-to-cloud messaging with IoT Hub has hello following characteristics:</span></span>

* <span data-ttu-id="27bd9-115">Meddelanden från enhet till moln är beständiga och behållas i en IoT-hubb standard **meddelanden/händelser** slutpunkt för in tooseven dagar.</span><span class="sxs-lookup"><span data-stu-id="27bd9-115">Device-to-cloud messages are durable and retained in an IoT hub's default **messages/events** endpoint for up tooseven days.</span></span>
* <span data-ttu-id="27bd9-116">Meddelanden från enhet till moln kan innehålla högst 256 KB och kan grupperas i batchar toooptimize skickar.</span><span class="sxs-lookup"><span data-stu-id="27bd9-116">Device-to-cloud messages can be at most 256 KB, and can be grouped in batches toooptimize sends.</span></span> <span data-ttu-id="27bd9-117">Batchar kan innehålla högst 256 KB.</span><span class="sxs-lookup"><span data-stu-id="27bd9-117">Batches can be at most 256 KB.</span></span>
* <span data-ttu-id="27bd9-118">Enligt beskrivningen i hello [kontroll åtkomst tooIoT hubb] [ lnk-devguide-security] avsnittet, IoT-hubb kan per enhet autentisering och åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="27bd9-118">As explained in hello [Control access tooIoT Hub][lnk-devguide-security] section, IoT Hub enables per-device authentication and access control.</span></span>
* <span data-ttu-id="27bd9-119">IoT-hubb kan toocreate av too10 anpassade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="27bd9-119">IoT Hub allows you toocreate up too10 custom endpoints.</span></span> <span data-ttu-id="27bd9-120">Meddelanden levereras toohello slutpunkter baserat på vägar som konfigurerats på din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="27bd9-120">Messages are delivered toohello endpoints based on routes configured on your IoT hub.</span></span> <span data-ttu-id="27bd9-121">Mer information finns i [regler för routning](#routing-rules).</span><span class="sxs-lookup"><span data-stu-id="27bd9-121">For more information, see [Routing rules](#routing-rules).</span></span>
* <span data-ttu-id="27bd9-122">IoT-hubb kan miljontals samtidigt anslutna enheter (se [kvoter och begränsning][lnk-quotas]).</span><span class="sxs-lookup"><span data-stu-id="27bd9-122">IoT Hub enables millions of simultaneously connected devices (see [Quotas and throttling][lnk-quotas]).</span></span>
* <span data-ttu-id="27bd9-123">IoT-hubb kan inte godtycklig partitionering.</span><span class="sxs-lookup"><span data-stu-id="27bd9-123">IoT Hub does not allow arbitrary partitioning.</span></span> <span data-ttu-id="27bd9-124">Meddelanden från enhet till moln partitioneras baserat på deras ursprung **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="27bd9-124">Device-to-cloud messages are partitioned based on their originating **deviceId**.</span></span>

<span data-ttu-id="27bd9-125">Mer information om hello skillnader mellan hello IoT-hubb och Händelsehubbar tjänster finns [jämförelse av Azure IoT Hub och Azure Event Hubs][lnk-comparison].</span><span class="sxs-lookup"><span data-stu-id="27bd9-125">For more information about hello differences between hello IoT Hub and Event Hubs services, see [Comparison of Azure IoT Hub and Azure Event Hubs][lnk-comparison].</span></span>

## <a name="send-non-telemetry-traffic"></a><span data-ttu-id="27bd9-126">Skicka ej telemetri trafik</span><span class="sxs-lookup"><span data-stu-id="27bd9-126">Send non-telemetry traffic</span></span>

<span data-ttu-id="27bd9-127">Ofta dessutom tootelemetry datapunkter, enheterna skickar meddelanden och förfrågningar som kräver separat utförande och hantering i hello lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="27bd9-127">Often, in addition tootelemetry data points, devices send messages and requests that require separate execution and handling in hello solution back end.</span></span> <span data-ttu-id="27bd9-128">Till exempel serverdel kritiska aviseringar som skall utlösa en specifik åtgärd i hello.</span><span class="sxs-lookup"><span data-stu-id="27bd9-128">For example, critical alerts that must trigger a specific action in hello back end.</span></span> <span data-ttu-id="27bd9-129">Du kan enkelt skriva en [routningsregel] [ lnk-devguide-custom] toosend dessa typer av meddelanden tooan slutpunkten dedicated tootheir bearbetning baserat på antingen ett sidhuvud på hello-meddelande eller ett värde i hello meddelandetexten.</span><span class="sxs-lookup"><span data-stu-id="27bd9-129">You can easily write a [routing rule][lnk-devguide-custom] toosend these types of messages tooan endpoint dedicated tootheir processing based on either a header on hello message or a value in hello message body.</span></span>

<span data-ttu-id="27bd9-130">Mer information om hello bästa sätt tooprocess kind det här meddelandet finns hello [Självstudier: hur tooprocess IoT-hubb meddelanden från enhet till moln] [ lnk-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="27bd9-130">For more information about hello best way tooprocess this kind of message, see hello [Tutorial: How tooprocess IoT Hub device-to-cloud messages][lnk-d2c-tutorial] tutorial.</span></span>

## <a name="route-device-to-cloud-messages"></a><span data-ttu-id="27bd9-131">Vidarebefordra meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="27bd9-131">Route device-to-cloud messages</span></span>

<span data-ttu-id="27bd9-132">Har du två alternativ för routning meddelanden från enhet till moln tooyour backend-appar:</span><span class="sxs-lookup"><span data-stu-id="27bd9-132">You have two options for routing device-to-cloud messages tooyour back-end apps:</span></span>

* <span data-ttu-id="27bd9-133">Använd hello inbyggda [Event Hub-kompatibel endpoint] [ lnk-compatible-endpoint] tooenable backend-appar tooread hello enhet till moln har tagits emot av hello hubb.</span><span class="sxs-lookup"><span data-stu-id="27bd9-133">Use hello built-in [Event Hub-compatible endpoint][lnk-compatible-endpoint] tooenable back-end apps tooread hello device-to-cloud messages received by hello hub.</span></span> <span data-ttu-id="27bd9-134">toolearn om hello inbyggd Event Hub-kompatibel slutpunkt finns [läsa meddelanden från enhet till moln från hello inbyggd slutpunkt][lnk-devguide-builtin].</span><span class="sxs-lookup"><span data-stu-id="27bd9-134">toolearn about hello built-in Event Hub-compatible endpoint, see [Read device-to-cloud messages from hello built-in endpoint][lnk-devguide-builtin].</span></span>
* <span data-ttu-id="27bd9-135">Använd Routning regler toosend meddelanden toocustom slutpunkter i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="27bd9-135">Use routing rules toosend messages toocustom endpoints in your IoT hub.</span></span> <span data-ttu-id="27bd9-136">Anpassade slutpunkter aktivera din serverdel appar tooread enhet till moln-meddelanden med Händelsehubbar, Service Bus-köer eller Service Bus-ämnen.</span><span class="sxs-lookup"><span data-stu-id="27bd9-136">Custom endpoints enable your back-end apps tooread device-to-cloud messages using Event Hubs, Service Bus queues, or Service Bus topics.</span></span> <span data-ttu-id="27bd9-137">toolearn om Routning och anpassade slutpunkter, se [använda anpassade slutpunkter och routningsregler för meddelanden från enhet till moln][lnk-devguide-custom].</span><span class="sxs-lookup"><span data-stu-id="27bd9-137">toolearn about routing and custom endpoints, see [Use custom endpoints and routing rules for device-to-cloud messages][lnk-devguide-custom].</span></span>

## <a name="anti-spoofing-properties"></a><span data-ttu-id="27bd9-138">Skydd mot förfalskning egenskaper</span><span class="sxs-lookup"><span data-stu-id="27bd9-138">Anti-spoofing properties</span></span>

<span data-ttu-id="27bd9-139">tooavoid enhet förfalskning i meddelanden från enhet till moln, IoT-hubb stämplar alla meddelanden med hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="27bd9-139">tooavoid device spoofing in device-to-cloud messages, IoT Hub stamps all messages with hello following properties:</span></span>

* <span data-ttu-id="27bd9-140">**ConnectionDeviceId**</span><span class="sxs-lookup"><span data-stu-id="27bd9-140">**ConnectionDeviceId**</span></span>
* <span data-ttu-id="27bd9-141">**ConnectionDeviceGenerationId**</span><span class="sxs-lookup"><span data-stu-id="27bd9-141">**ConnectionDeviceGenerationId**</span></span>
* <span data-ttu-id="27bd9-142">**ConnectionAuthMethod**</span><span class="sxs-lookup"><span data-stu-id="27bd9-142">**ConnectionAuthMethod**</span></span>

<span data-ttu-id="27bd9-143">hello först två innehåller hello **deviceId** och **generationId** av hello kommer enheten enligt [identitet enhetsegenskaper][lnk-device-properties].</span><span class="sxs-lookup"><span data-stu-id="27bd9-143">hello first two contain hello **deviceId** and **generationId** of hello originating device, as per [Device identity properties][lnk-device-properties].</span></span>

<span data-ttu-id="27bd9-144">Hej **ConnectionAuthMethod** -egenskapen innehåller ett serialiserat JSON-objekt med hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="27bd9-144">hello **ConnectionAuthMethod** property contains a JSON serialized object, with hello following properties:</span></span>

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a><span data-ttu-id="27bd9-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27bd9-145">Next steps</span></span>

<span data-ttu-id="27bd9-146">Information om hello SDK kan du använda toosend meddelanden från enhet till moln, finns [Azure IoT SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="27bd9-146">For information about hello SDKs you can use toosend device-to-cloud messages, see [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="27bd9-147">Hej [Kom igång] [ lnk-get-started] självstudiekurser visar hur toosend enhet till moln meddelanden från både simulerade och fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="27bd9-147">hello [Get Started][lnk-get-started] tutorials show you how toosend device-to-cloud messages from both simulated and physical devices.</span></span> <span data-ttu-id="27bd9-148">Mer information finns i hello [processen IoT Hub-enhet till moln meddelanden med hjälp av vägar] [ lnk-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="27bd9-148">For more detail, see hello [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial.</span></span>

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: iot-hub-get-started.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

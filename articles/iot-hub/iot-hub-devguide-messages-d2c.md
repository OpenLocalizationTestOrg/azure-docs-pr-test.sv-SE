---
title: "Förstå Azure IoT Hub-enhet till moln messaging | Microsoft Docs"
description: "Utvecklare guide - hur du använder enhet till moln-meddelanden med IoT-hubben. Innehåller information om hur du skickar data både telemetri och icke-telemtry routning för att leverera meddelanden."
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
ms.openlocfilehash: d856e26084ee79386a2e8e0e527804bda86b477b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="send-device-to-cloud-messages-to-iot-hub"></a><span data-ttu-id="82afe-104">Skicka meddelanden från enhet till moln till IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="82afe-104">Send device-to-cloud messages to IoT Hub</span></span>

<span data-ttu-id="82afe-105">Skicka meddelanden från enhet till moln för att skicka tidsserier telemetri och aviseringar från dina enheter till din lösningens serverdel, från en enhet till IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="82afe-105">To send time-series telemetry and alerts from your devices to your solution back end, send device-to-cloud messages from your device to your IoT hub.</span></span> <span data-ttu-id="82afe-106">En beskrivning av andra enhet till moln-alternativ som stöds av IoT-hubb finns [enhet till moln kommunikation vägledning][lnk-d2c-guidance].</span><span class="sxs-lookup"><span data-stu-id="82afe-106">For a discussion of other device-to-cloud options supported by IoT Hub, see [Device-to-cloud communications guidance][lnk-d2c-guidance].</span></span>

<span data-ttu-id="82afe-107">Du skickar meddelanden från enhet till moln via en enhet riktade slutpunkt (**/devices/ {deviceId} / meddelanden/händelser**).</span><span class="sxs-lookup"><span data-stu-id="82afe-107">You send device-to-cloud messages through a device-facing endpoint (**/devices/{deviceId}/messages/events**).</span></span> <span data-ttu-id="82afe-108">Routningsregler och vidarebefordra meddelanden till en av slutpunkterna service-riktade på din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="82afe-108">Routing rules then route your messages to one of the service-facing endpoints on your IoT hub.</span></span> <span data-ttu-id="82afe-109">Regler för routning använda sidhuvuden och innehållet i de meddelanden från enhet till moln passerar genom din hubb för att avgöra var du vill dirigera dem.</span><span class="sxs-lookup"><span data-stu-id="82afe-109">Routing rules use the headers and body of the device-to-cloud messages flowing through your hub to determine where to route them.</span></span> <span data-ttu-id="82afe-110">Som standard meddelanden dirigeras till den inbyggda tjänst-riktade slutpunkten (**meddelanden/händelser**), som är kompatibel med [Händelsehubbar][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="82afe-110">By default, messages are routed to the built-in service-facing endpoint (**messages/events**), that is compatible with [Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="82afe-111">Därför kan du använda standard [Händelsehubbar integrering och SDK] [ lnk-compatible-endpoint] att ta emot meddelanden från enhet till moln i din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="82afe-111">Therefore, you can use standard [Event Hubs integration and SDKs][lnk-compatible-endpoint] to receive device-to-cloud messages in your solution back end.</span></span>

<span data-ttu-id="82afe-112">IoT-hubb implementerar enhet till moln-meddelanden med hjälp av en strömmande meddelandemönster används.</span><span class="sxs-lookup"><span data-stu-id="82afe-112">IoT Hub implements device-to-cloud messaging using a streaming messaging pattern.</span></span> <span data-ttu-id="82afe-113">IoT-hubb meddelanden från enhet till moln är mer som [Händelsehubbar] [ lnk-event-hubs] *händelser* än [Service Bus] [ lnk-servicebus] *meddelanden* att en stor volym med händelser som passerar den tjänst som kan läsas av flera läsare.</span><span class="sxs-lookup"><span data-stu-id="82afe-113">IoT Hub's device-to-cloud messages are more like [Event Hubs][lnk-event-hubs] *events* than [Service Bus][lnk-servicebus] *messages* in that there is a high volume of events passing through the service that can be read by multiple readers.</span></span>

<span data-ttu-id="82afe-114">Enhet till moln meddelanden med IoT-hubben har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="82afe-114">Device-to-cloud messaging with IoT Hub has the following characteristics:</span></span>

* <span data-ttu-id="82afe-115">Meddelanden från enhet till moln är beständiga och behållas i en IoT-hubb standard **meddelanden/händelser** slutpunkt för upp till sju dagar.</span><span class="sxs-lookup"><span data-stu-id="82afe-115">Device-to-cloud messages are durable and retained in an IoT hub's default **messages/events** endpoint for up to seven days.</span></span>
* <span data-ttu-id="82afe-116">Meddelanden från enhet till moln kan innehålla högst 256 KB och kan grupperas i batchar att optimera skickar.</span><span class="sxs-lookup"><span data-stu-id="82afe-116">Device-to-cloud messages can be at most 256 KB, and can be grouped in batches to optimize sends.</span></span> <span data-ttu-id="82afe-117">Batchar kan innehålla högst 256 KB.</span><span class="sxs-lookup"><span data-stu-id="82afe-117">Batches can be at most 256 KB.</span></span>
* <span data-ttu-id="82afe-118">Enligt beskrivningen i den [styra åtkomsten till IoT-hubb] [ lnk-devguide-security] avsnittet, IoT-hubb kan per enhet autentisering och åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="82afe-118">As explained in the [Control access to IoT Hub][lnk-devguide-security] section, IoT Hub enables per-device authentication and access control.</span></span>
* <span data-ttu-id="82afe-119">IoT-hubb kan du skapa upp till 10 anpassade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="82afe-119">IoT Hub allows you to create up to 10 custom endpoints.</span></span> <span data-ttu-id="82afe-120">Meddelanden levereras till slutpunkterna baserat på vägar som konfigurerats på din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="82afe-120">Messages are delivered to the endpoints based on routes configured on your IoT hub.</span></span> <span data-ttu-id="82afe-121">Mer information finns i [regler för routning](#routing-rules).</span><span class="sxs-lookup"><span data-stu-id="82afe-121">For more information, see [Routing rules](#routing-rules).</span></span>
* <span data-ttu-id="82afe-122">IoT-hubb kan miljontals samtidigt anslutna enheter (se [kvoter och begränsning][lnk-quotas]).</span><span class="sxs-lookup"><span data-stu-id="82afe-122">IoT Hub enables millions of simultaneously connected devices (see [Quotas and throttling][lnk-quotas]).</span></span>
* <span data-ttu-id="82afe-123">IoT-hubb kan inte godtycklig partitionering.</span><span class="sxs-lookup"><span data-stu-id="82afe-123">IoT Hub does not allow arbitrary partitioning.</span></span> <span data-ttu-id="82afe-124">Meddelanden från enhet till moln partitioneras baserat på deras ursprung **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="82afe-124">Device-to-cloud messages are partitioned based on their originating **deviceId**.</span></span>

<span data-ttu-id="82afe-125">Mer information om skillnaderna mellan de IoT-hubb och Händelsehubbar tjänster finns [jämförelse av Azure IoT Hub och Azure Event Hubs][lnk-comparison].</span><span class="sxs-lookup"><span data-stu-id="82afe-125">For more information about the differences between the IoT Hub and Event Hubs services, see [Comparison of Azure IoT Hub and Azure Event Hubs][lnk-comparison].</span></span>

## <a name="send-non-telemetry-traffic"></a><span data-ttu-id="82afe-126">Skicka ej telemetri trafik</span><span class="sxs-lookup"><span data-stu-id="82afe-126">Send non-telemetry traffic</span></span>

<span data-ttu-id="82afe-127">Ofta skicka enheter förutom telemetridatapunkter, meddelanden och förfrågningar som kräver separat utförande och hantering i lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="82afe-127">Often, in addition to telemetry data points, devices send messages and requests that require separate execution and handling in the solution back end.</span></span> <span data-ttu-id="82afe-128">Till exempel kritiska aviseringar som skall utlösa en specifik åtgärd i serverdelen.</span><span class="sxs-lookup"><span data-stu-id="82afe-128">For example, critical alerts that must trigger a specific action in the back end.</span></span> <span data-ttu-id="82afe-129">Du kan enkelt skriva en [routningsregel] [ lnk-devguide-custom] att skicka dessa typer av meddelanden till en slutpunkt som är dedikerad till bearbetningen baserat på antingen ett sidhuvud på meddelandet eller ett värde i meddelandetexten.</span><span class="sxs-lookup"><span data-stu-id="82afe-129">You can easily write a [routing rule][lnk-devguide-custom] to send these types of messages to an endpoint dedicated to their processing based on either a header on the message or a value in the message body.</span></span>

<span data-ttu-id="82afe-130">Mer information om det bästa sättet att bearbeta den här typen av meddelande finns i [Självstudier: bearbeta meddelanden från enhet till moln IoT-hubb] [ lnk-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="82afe-130">For more information about the best way to process this kind of message, see the [Tutorial: How to process IoT Hub device-to-cloud messages][lnk-d2c-tutorial] tutorial.</span></span>

## <a name="route-device-to-cloud-messages"></a><span data-ttu-id="82afe-131">Vidarebefordra meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="82afe-131">Route device-to-cloud messages</span></span>

<span data-ttu-id="82afe-132">Har du två alternativ för att dirigera enhet till moln-meddelanden till backend-appar:</span><span class="sxs-lookup"><span data-stu-id="82afe-132">You have two options for routing device-to-cloud messages to your back-end apps:</span></span>

* <span data-ttu-id="82afe-133">Använd inbyggt [Event Hub-kompatibel endpoint] [ lnk-compatible-endpoint] aktivera backend-appar att läsa de enhet till moln har tagits emot av hubben.</span><span class="sxs-lookup"><span data-stu-id="82afe-133">Use the built-in [Event Hub-compatible endpoint][lnk-compatible-endpoint] to enable back-end apps to read the device-to-cloud messages received by the hub.</span></span> <span data-ttu-id="82afe-134">Mer information om inbyggda Event Hub-kompatibel slutpunkten, se [läsa meddelanden från enhet till moln från inbyggd slutpunkt][lnk-devguide-builtin].</span><span class="sxs-lookup"><span data-stu-id="82afe-134">To learn about the built-in Event Hub-compatible endpoint, see [Read device-to-cloud messages from the built-in endpoint][lnk-devguide-builtin].</span></span>
* <span data-ttu-id="82afe-135">Använda regler för routning för att skicka meddelanden till anpassade slutpunkter i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="82afe-135">Use routing rules to send messages to custom endpoints in your IoT hub.</span></span> <span data-ttu-id="82afe-136">Anpassade slutpunkter aktivera dina backend-appar att läsa meddelanden från enhet till moln med Händelsehubbar, Service Bus-köer eller Service Bus-ämnen.</span><span class="sxs-lookup"><span data-stu-id="82afe-136">Custom endpoints enable your back-end apps to read device-to-cloud messages using Event Hubs, Service Bus queues, or Service Bus topics.</span></span> <span data-ttu-id="82afe-137">Mer information om Routning och anpassade slutpunkter, se [använda anpassade slutpunkter och routningsregler för meddelanden från enhet till moln][lnk-devguide-custom].</span><span class="sxs-lookup"><span data-stu-id="82afe-137">To learn about routing and custom endpoints, see [Use custom endpoints and routing rules for device-to-cloud messages][lnk-devguide-custom].</span></span>

## <a name="anti-spoofing-properties"></a><span data-ttu-id="82afe-138">Skydd mot förfalskning egenskaper</span><span class="sxs-lookup"><span data-stu-id="82afe-138">Anti-spoofing properties</span></span>

<span data-ttu-id="82afe-139">För att undvika enheten förfalskning i meddelanden från enhet till moln, IoT-hubb stämplar alla meddelanden med följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="82afe-139">To avoid device spoofing in device-to-cloud messages, IoT Hub stamps all messages with the following properties:</span></span>

* <span data-ttu-id="82afe-140">**ConnectionDeviceId**</span><span class="sxs-lookup"><span data-stu-id="82afe-140">**ConnectionDeviceId**</span></span>
* <span data-ttu-id="82afe-141">**ConnectionDeviceGenerationId**</span><span class="sxs-lookup"><span data-stu-id="82afe-141">**ConnectionDeviceGenerationId**</span></span>
* <span data-ttu-id="82afe-142">**ConnectionAuthMethod**</span><span class="sxs-lookup"><span data-stu-id="82afe-142">**ConnectionAuthMethod**</span></span>

<span data-ttu-id="82afe-143">De första två innehåller den **deviceId** och **generationId** på den ursprungliga enheten enligt [identitet enhetsegenskaper][lnk-device-properties].</span><span class="sxs-lookup"><span data-stu-id="82afe-143">The first two contain the **deviceId** and **generationId** of the originating device, as per [Device identity properties][lnk-device-properties].</span></span>

<span data-ttu-id="82afe-144">Den **ConnectionAuthMethod** -egenskapen innehåller ett serialiserat JSON-objekt med följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="82afe-144">The **ConnectionAuthMethod** property contains a JSON serialized object, with the following properties:</span></span>

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a><span data-ttu-id="82afe-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82afe-145">Next steps</span></span>

<span data-ttu-id="82afe-146">Information om SDK: erna som du kan använda för att skicka meddelanden från enhet till moln finns [Azure IoT SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="82afe-146">For information about the SDKs you can use to send device-to-cloud messages, see [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="82afe-147">Den [Kom igång] [ lnk-get-started] självstudiekurser visar hur du skickar meddelanden från enhet till moln från både simulerade och fysiska enheter.</span><span class="sxs-lookup"><span data-stu-id="82afe-147">The [Get Started][lnk-get-started] tutorials show you how to send device-to-cloud messages from both simulated and physical devices.</span></span> <span data-ttu-id="82afe-148">Mer information finns i [processen IoT Hub-enhet till moln meddelanden med hjälp av vägar] [ lnk-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="82afe-148">For more detail, see the [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial.</span></span>

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

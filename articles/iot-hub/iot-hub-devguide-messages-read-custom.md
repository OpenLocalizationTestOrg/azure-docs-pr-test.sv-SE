---
title: "anpassade slutpunkter för aaaUnderstand Azure IoT Hub | Microsoft Docs"
description: "Utvecklarhandbok - med hjälp av Routning regler tooroute meddelanden från enhet till moln toocustom slutpunkter."
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
ms.openlocfilehash: daa9cfb35d0853e316bbf469b034d4dadbd4e85d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a><span data-ttu-id="33d22-103">Använd meddelandevägar och anpassade slutpunkter för meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="33d22-103">Use message routes and custom endpoints for device-to-cloud messages</span></span>

<span data-ttu-id="33d22-104">IoT-hubb kan du tooroute [meddelanden från enhet till moln] [ lnk-device-to-cloud] tooIoT hubb service-riktade slutpunkter baserat på Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="33d22-104">IoT Hub enables you tooroute [device-to-cloud messages][lnk-device-to-cloud] tooIoT Hub service-facing endpoints based on message properties.</span></span> <span data-ttu-id="33d22-105">Routning regler ger du hello flexibilitet toosend måste där de behöver toogo utan hello-meddelanden för ytterligare tjänster tooprocess meddelanden eller toowrite ytterligare kod.</span><span class="sxs-lookup"><span data-stu-id="33d22-105">Routing rules give you hello flexibility toosend messages where they need toogo without hello need for additional services tooprocess messages or toowrite additional code.</span></span> <span data-ttu-id="33d22-106">Varje regel för vidarebefordran av du konfigurerar har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="33d22-106">Each routing rule you configure has hello following properties:</span></span>

| <span data-ttu-id="33d22-107">Egenskap</span><span class="sxs-lookup"><span data-stu-id="33d22-107">Property</span></span>      | <span data-ttu-id="33d22-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="33d22-108">Description</span></span> |
| ------------- | ----------- |
| <span data-ttu-id="33d22-109">**Namn**</span><span class="sxs-lookup"><span data-stu-id="33d22-109">**Name**</span></span>      | <span data-ttu-id="33d22-110">hello unika namn som identifierar hello regeln.</span><span class="sxs-lookup"><span data-stu-id="33d22-110">hello unique name that identifies hello rule.</span></span> |
| <span data-ttu-id="33d22-111">**Källa**</span><span class="sxs-lookup"><span data-stu-id="33d22-111">**Source**</span></span>    | <span data-ttu-id="33d22-112">hello ursprung hello strömma toobe efterlevts.</span><span class="sxs-lookup"><span data-stu-id="33d22-112">hello origin of hello data stream toobe acted upon.</span></span> <span data-ttu-id="33d22-113">Till exempel enhetstelemetrin.</span><span class="sxs-lookup"><span data-stu-id="33d22-113">For example, device telemetry.</span></span> |
| <span data-ttu-id="33d22-114">**Villkor**</span><span class="sxs-lookup"><span data-stu-id="33d22-114">**Condition**</span></span> | <span data-ttu-id="33d22-115">hello frågeuttrycket för hello routningsregel som körs mot hello-meddelande sidhuvuden och brödtext och används toodetermine om det finns en matchning för hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="33d22-115">hello query expression for hello routing rule that is run against hello message's headers and body and used toodetermine whether it is a match for hello endpoint.</span></span> <span data-ttu-id="33d22-116">Mer information om hur du skapar en väg villkoret finns hello [referens - frågespråket för jobb och enheten twins][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="33d22-116">For more information about constructing a route condition, see hello [Reference - query language for device twins and jobs][lnk-devguide-query-language].</span></span> |
| <span data-ttu-id="33d22-117">**Slutpunkt**</span><span class="sxs-lookup"><span data-stu-id="33d22-117">**Endpoint**</span></span>  | <span data-ttu-id="33d22-118">hello namnet på hello slutpunkt där IoT-hubb skickar meddelanden som matchar hello villkor.</span><span class="sxs-lookup"><span data-stu-id="33d22-118">hello name of hello endpoint where IoT Hub sends messages that match hello condition.</span></span> <span data-ttu-id="33d22-119">Slutpunkter måste vara i hello samma region som hello IoT-hubb annars du kanske att debiteras för cross-region skriver.</span><span class="sxs-lookup"><span data-stu-id="33d22-119">Endpoints should be in hello same region as hello IoT hub, otherwise you may be charged for cross-region writes.</span></span> |

<span data-ttu-id="33d22-120">Ett enda meddelande kan matcha hello tillståndet på flera regler för routning, som ger fallet IoT-hubb hello meddelandet toohello slutpunkt som är associerade med varje matchade regel.</span><span class="sxs-lookup"><span data-stu-id="33d22-120">A single message may match hello condition on multiple routing rules, in which case IoT Hub delivers hello message toohello endpoint associated with each matched rule.</span></span> <span data-ttu-id="33d22-121">IoT-hubb deduplicates också automatiskt meddelandeleverans, så om ett meddelande matchar flera regler som har hello samma mål den bara skrivs toothat mål en gång.</span><span class="sxs-lookup"><span data-stu-id="33d22-121">IoT Hub also automatically deduplicates message delivery, so if a message matches multiple rules that all have hello same destination, it is only written toothat destination once.</span></span>

<span data-ttu-id="33d22-122">En IoT-hubb har standard [inbyggd slutpunkt][lnk-built-in].</span><span class="sxs-lookup"><span data-stu-id="33d22-122">An IoT hub has a default [built-in endpoint][lnk-built-in].</span></span> <span data-ttu-id="33d22-123">Du kan skapa anpassade slutpunkter tooroute meddelanden tooby länkning av andra tjänster i din prenumeration toohello hubb.</span><span class="sxs-lookup"><span data-stu-id="33d22-123">You can create custom endpoints tooroute messages tooby linking other services in your subscription toohello hub.</span></span> <span data-ttu-id="33d22-124">IoT-hubb stöder för närvarande Händelsehubbar, Service Bus-köer och Service Bus-ämnen som anpassade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="33d22-124">IoT Hub currently supports Event Hubs, Service Bus queues, and Service Bus topics as custom endpoints.</span></span>

> [!WARNING]
> <span data-ttu-id="33d22-125">Service Bus-köer och ämnen med **sessioner** eller **dubblettidentifiering** aktiverat som anpassade slutpunkter stöds inte.</span><span class="sxs-lookup"><span data-stu-id="33d22-125">Service Bus queues and topics with **Sessions** or **Duplicate Detection** enabled are not supported as custom endpoints.</span></span>

<span data-ttu-id="33d22-126">Mer information om hur du skapar anpassade slutpunkter i IoT-hubb finns [IoT-hubbslutpunkter][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="33d22-126">For more information about creating custom endpoints in IoT Hub, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="33d22-127">För mer information om läsning från anpassade slutpunkter, se:</span><span class="sxs-lookup"><span data-stu-id="33d22-127">For more information about reading from custom endpoints, see:</span></span>

* <span data-ttu-id="33d22-128">Läsning från [Händelsehubbar][lnk-getstarted-eh].</span><span class="sxs-lookup"><span data-stu-id="33d22-128">Reading from [Event Hubs][lnk-getstarted-eh].</span></span>
* <span data-ttu-id="33d22-129">Läsning från [Service Bus-köer][lnk-getstarted-queue].</span><span class="sxs-lookup"><span data-stu-id="33d22-129">Reading from [Service Bus queues][lnk-getstarted-queue].</span></span>
* <span data-ttu-id="33d22-130">Läsning från [Service Bus-ämnen][lnk-getstarted-topic].</span><span class="sxs-lookup"><span data-stu-id="33d22-130">Reading from [Service Bus topics][lnk-getstarted-topic].</span></span>

### <a name="next-steps"></a><span data-ttu-id="33d22-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33d22-131">Next steps</span></span>

<span data-ttu-id="33d22-132">Läs mer om IoT-hubbslutpunkter [IoT-hubbslutpunkter][lnk-devguide-endpoints].</span><span class="sxs-lookup"><span data-stu-id="33d22-132">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-devguide-endpoints].</span></span>

<span data-ttu-id="33d22-133">Mer information om hello frågespråket du använder regler för routning av toodefine, se [IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning][lnk-devguide-query-language].</span><span class="sxs-lookup"><span data-stu-id="33d22-133">For more information about hello query language you use toodefine routing rules, see [IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query-language].</span></span>

<span data-ttu-id="33d22-134">Hej [processen IoT Hub-enhet till moln meddelanden med hjälp av vägar] [ lnk-d2c-tutorial] kursen visar hur toouse routning regler och anpassade slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="33d22-134">hello [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial shows you how toouse routing rules and custom endpoints.</span></span>

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md

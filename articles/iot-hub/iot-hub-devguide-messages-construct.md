---
title: "Förstå Azure IoT Hub-meddelandeformat | Microsoft Docs"
description: "Utvecklarhandbok - beskrivs formatet och förväntade innehållet i IoT-hubb meddelanden."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: e6eafb1a0030b022da2b5d0b787e092f3067c99f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-and-read-iot-hub-messages"></a><span data-ttu-id="ebe9b-103">Skapa och läsa IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="ebe9b-103">Create and read IoT Hub messages</span></span>

<span data-ttu-id="ebe9b-104">För att stödja smidig samverkan över protokoll, definierar IoT-hubb ett gemensamt meddelandeformat för alla enheter riktade protokoll.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-104">To support seamless interoperability across protocols, IoT Hub defines a common message format for all device-facing protocols.</span></span> <span data-ttu-id="ebe9b-105">Formatet som används för både [enhet till moln] [ lnk-d2c] och [moln till enhet] [ lnk-c2d] meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-105">This message format is used for both [device-to-cloud][lnk-d2c] and [cloud-to-device][lnk-c2d] messages.</span></span> <span data-ttu-id="ebe9b-106">En [IoT-hubb meddelandet] [ lnk-messaging] består av:</span><span class="sxs-lookup"><span data-stu-id="ebe9b-106">An [IoT Hub message][lnk-messaging] consists of:</span></span>

* <span data-ttu-id="ebe9b-107">En uppsättning *Systemegenskaper*.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-107">A set of *system properties*.</span></span> <span data-ttu-id="ebe9b-108">Egenskaper som IoT-hubb tolkar eller anger.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-108">Properties that IoT Hub interprets or sets.</span></span> <span data-ttu-id="ebe9b-109">Den här uppsättningen är förinställda.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-109">This set is predetermined.</span></span>
* <span data-ttu-id="ebe9b-110">En uppsättning *programegenskaper*.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-110">A set of *application properties*.</span></span> <span data-ttu-id="ebe9b-111">En ordlista med egenskaper för anslutningssträngar som programmet kan definiera och åtkomst utan att avbryta serialiseringen för brödtext.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-111">A dictionary of string properties that the application can define and access, without needing to deserialize the message body.</span></span> <span data-ttu-id="ebe9b-112">IoT-hubb ändrar aldrig dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-112">IoT Hub never modifies these properties.</span></span>
* <span data-ttu-id="ebe9b-113">En täckande binära brödtext.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-113">An opaque binary body.</span></span>

<span data-ttu-id="ebe9b-114">Egenskapsnamn och värden kan endast innehålla alfanumeriska ASCII-tecken, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`\` när du:</span><span class="sxs-lookup"><span data-stu-id="ebe9b-114">Property names and values can only contain ASCII alphanumeric characters, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`\` when you:</span></span>

* <span data-ttu-id="ebe9b-115">Skicka meddelanden från enhet till moln med hjälp av HTTP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-115">Send device-to-cloud messages using the HTTP protocol.</span></span>
* <span data-ttu-id="ebe9b-116">Skicka meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-116">Send cloud-to-device messages.</span></span>

<span data-ttu-id="ebe9b-117">Mer information om hur du kodar och avkodar meddelanden som skickas med olika protokoll finns [Azure IoT SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="ebe9b-117">For more information about how to encode and decode messages sent using different protocols, see [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="ebe9b-118">I följande tabell visas en uppsättning egenskaper i IoT-hubb-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-118">The following table lists the set of system properties in IoT Hub messages.</span></span>

| <span data-ttu-id="ebe9b-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ebe9b-119">Property</span></span> | <span data-ttu-id="ebe9b-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ebe9b-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="ebe9b-121">messageId</span><span class="sxs-lookup"><span data-stu-id="ebe9b-121">MessageId</span></span> |<span data-ttu-id="ebe9b-122">En användare går identifierare för meddelandet som används för request-reply-mönster.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-122">A user-settable identifier for the message used for request-reply patterns.</span></span> <span data-ttu-id="ebe9b-123">Format: En skiftlägeskänslig sträng (upp till 128 tecken) av alfanumeriska tecken, ASCII-7-bitars + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-123">Format: A case-sensitive string (up to 128 characters long) of ASCII 7-bit alphanumeric characters + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`.</span></span> |
| <span data-ttu-id="ebe9b-124">Sekvensnummer</span><span class="sxs-lookup"><span data-stu-id="ebe9b-124">Sequence number</span></span> |<span data-ttu-id="ebe9b-125">En siffra (unika per enhet kö) IoT-hubben har tilldelats varje moln till enhet-meddelande.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-125">A number (unique per device-queue) assigned by IoT Hub to each cloud-to-device message.</span></span> |
| <span data-ttu-id="ebe9b-126">Till</span><span class="sxs-lookup"><span data-stu-id="ebe9b-126">To</span></span> |<span data-ttu-id="ebe9b-127">Ett mål som anges i [moln till enhet] [ lnk-c2d] meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-127">A destination specified in [Cloud-to-Device][lnk-c2d] messages.</span></span> |
| <span data-ttu-id="ebe9b-128">ExpiryTimeUtc</span><span class="sxs-lookup"><span data-stu-id="ebe9b-128">ExpiryTimeUtc</span></span> |<span data-ttu-id="ebe9b-129">Datum och tid för meddelandet upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-129">Date and time of message expiration.</span></span> |
| <span data-ttu-id="ebe9b-130">EnqueuedTime</span><span class="sxs-lookup"><span data-stu-id="ebe9b-130">EnqueuedTime</span></span> |<span data-ttu-id="ebe9b-131">Datum och tid i [moln till enhet] [ lnk-c2d] meddelande togs emot av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-131">Date and time the [Cloud-to-Device][lnk-c2d] message was received by IoT Hub.</span></span> |
| <span data-ttu-id="ebe9b-132">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="ebe9b-132">CorrelationId</span></span> |<span data-ttu-id="ebe9b-133">En strängegenskap i ett svarsmeddelande som vanligtvis innehåller MessageId för begäran i request-reply-mönster.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-133">A string property in a response message that typically contains the MessageId of the request, in request-reply patterns.</span></span> |
| <span data-ttu-id="ebe9b-134">Användar-ID</span><span class="sxs-lookup"><span data-stu-id="ebe9b-134">UserId</span></span> |<span data-ttu-id="ebe9b-135">Ett ID som används för att ange ursprung av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-135">An ID used to specify the origin of messages.</span></span> <span data-ttu-id="ebe9b-136">När meddelanden som genereras av IoT-hubb, den är inställd på `{iot hub name}`.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-136">When messages are generated by IoT Hub, it is set to `{iot hub name}`.</span></span> |
| <span data-ttu-id="ebe9b-137">Ack</span><span class="sxs-lookup"><span data-stu-id="ebe9b-137">Ack</span></span> |<span data-ttu-id="ebe9b-138">En feedback generatorn för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-138">A feedback message generator.</span></span> <span data-ttu-id="ebe9b-139">Den här egenskapen används i meddelanden moln till enhet för att begära IoT-hubb för att generera feedback meddelanden till följd av användningen av meddelandet av enheten.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-139">This property is used in cloud-to-device messages to request IoT Hub to generate feedback messages as a result of the consumption of the message by the device.</span></span> <span data-ttu-id="ebe9b-140">Möjliga värden: **ingen** (standard): ingen feedback-meddelandet genereras **positivt**: ett meddelande feedback om meddelandet slutfördes **negativa**: ta emot en feedback meddelande om meddelandet har upphört att gälla (eller leverans av maximalt antal nåddes) utan slutförs av enheten, eller **fullständig**: både positiva och negativa.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-140">Possible values: **none** (default): no feedback message is generated, **positive**: receive a feedback message if the message was completed, **negative**: receive a feedback message if the message expired (or maximum delivery count was reached) without being completed by the device, or **full**: both positive and negative.</span></span> <span data-ttu-id="ebe9b-141">Mer information finns i [meddelande feedback][lnk-feedback].</span><span class="sxs-lookup"><span data-stu-id="ebe9b-141">For more information, see [Message feedback][lnk-feedback].</span></span> |
| <span data-ttu-id="ebe9b-142">ConnectionDeviceId</span><span class="sxs-lookup"><span data-stu-id="ebe9b-142">ConnectionDeviceId</span></span> |<span data-ttu-id="ebe9b-143">Ett ID som angetts av IoT-hubb på meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-143">An ID set by IoT Hub on device-to-cloud messages.</span></span> <span data-ttu-id="ebe9b-144">Den innehåller den **deviceId** på den enhet som skickade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-144">It contains the **deviceId** of the device that sent the message.</span></span> |
| <span data-ttu-id="ebe9b-145">ConnectionDeviceGenerationId</span><span class="sxs-lookup"><span data-stu-id="ebe9b-145">ConnectionDeviceGenerationId</span></span> |<span data-ttu-id="ebe9b-146">Ett ID som angetts av IoT-hubb på meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-146">An ID set by IoT Hub on device-to-cloud messages.</span></span> <span data-ttu-id="ebe9b-147">Den innehåller den **generationId** (enligt [identitet enhetsegenskaper][lnk-device-properties]) på den enhet som skickade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-147">It contains the **generationId** (as per [Device identity properties][lnk-device-properties]) of the device that sent the message.</span></span> |
| <span data-ttu-id="ebe9b-148">ConnectionAuthMethod</span><span class="sxs-lookup"><span data-stu-id="ebe9b-148">ConnectionAuthMethod</span></span> |<span data-ttu-id="ebe9b-149">En autentiseringsmetod som angetts av IoT-hubb på meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-149">An authentication method set by IoT Hub on device-to-cloud messages.</span></span> <span data-ttu-id="ebe9b-150">Den här egenskapen innehåller information om autentiseringsmetoden som används för att autentisera enheten skickar meddelandet.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-150">This property contains information about the authentication method used to authenticate the device sending the message.</span></span> <span data-ttu-id="ebe9b-151">Mer information finns i [enheten till molnet skydd mot förfalskning][lnk-antispoofing].</span><span class="sxs-lookup"><span data-stu-id="ebe9b-151">For more information, see [Device to cloud anti-spoofing][lnk-antispoofing].</span></span> |

## <a name="message-size"></a><span data-ttu-id="ebe9b-152">Meddelandestorlek</span><span class="sxs-lookup"><span data-stu-id="ebe9b-152">Message size</span></span>

<span data-ttu-id="ebe9b-153">IoT-hubb mäter meddelandestorlek på ett sätt som protokoll-oberoende överväger faktiska nyttolasten.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-153">IoT Hub measures message size in a protocol-agnostic way, considering only the actual payload.</span></span> <span data-ttu-id="ebe9b-154">Storlek i byte beräknas som summan av följande:</span><span class="sxs-lookup"><span data-stu-id="ebe9b-154">The size in bytes is calculated as the sum of the following:</span></span>

* <span data-ttu-id="ebe9b-155">Brödtext storlek i byte.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-155">The body size in bytes.</span></span>
* <span data-ttu-id="ebe9b-156">Storlek i byte för alla värden i meddelandet Systemegenskaper.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-156">The size in bytes of all the values of the message system properties.</span></span>
* <span data-ttu-id="ebe9b-157">Storlek i byte för alla användare egenskapsnamn och värden.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-157">The size in bytes of all user property names and values.</span></span>

<span data-ttu-id="ebe9b-158">Egenskapsnamn och värden är begränsade till ASCII-tecken, så att längden på strängarna som är lika med storleken i byte.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-158">Property names and values are limited to ASCII characters, so the length of the strings equals the size in bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebe9b-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ebe9b-159">Next steps</span></span>

<span data-ttu-id="ebe9b-160">Information om storleksgränser för meddelanden i IoT-hubb finns [IoT-hubb kvoter och begränsning][lnk-quotas].</span><span class="sxs-lookup"><span data-stu-id="ebe9b-160">For information about message size limits in IoT Hub, see [IoT Hub quotas and throttling][lnk-quotas].</span></span>

<span data-ttu-id="ebe9b-161">Information om hur du skapar och läsa meddelanden i olika programmeringsspråk för IoT-hubb finns i [Kom igång] [ lnk-get-started] självstudier.</span><span class="sxs-lookup"><span data-stu-id="ebe9b-161">To learn how to create and read IoT Hub messages in various programming languages, see the [Get started][lnk-get-started] tutorials.</span></span>

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties

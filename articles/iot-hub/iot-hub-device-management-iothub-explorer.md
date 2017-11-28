---
title: aaaAzure IoT-enhetshantering med iothub explorer | Microsoft Docs
description: "Verktyget hello iothub explorer CLI för Azure IoT Hub enhetshantering med hello direkt metoder och hanteringsalternativ för hello dubbla egenskaper."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Azure iot enhetshantering, azure iot-hubb enhetshantering, device management iot, enhetshantering för iot-hubb"
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: xshi
ms.openlocfilehash: e0a5e6120db5c4fb12f7f8b605a56e0e4aad9217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a><span data-ttu-id="8a03f-104">Använda iothub explorer för hantering av Azure IoT Hub-enheter</span><span class="sxs-lookup"><span data-stu-id="8a03f-104">Use iothub-explorer for Azure IoT Hub device management</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="8a03f-106">[iothub explorer](https://github.com/azure/iothub-explorer) är ett CLI-verktyg som du kör på en värd datorn toomanage enheten identiteter i din IoT-hubb registret.</span><span class="sxs-lookup"><span data-stu-id="8a03f-106">[iothub-explorer](https://github.com/azure/iothub-explorer) is a CLI tool that you run on a host computer toomanage device identities in your IoT hub registry.</span></span> <span data-ttu-id="8a03f-107">Den innehåller alternativ som du kan använda tooperform olika uppgifter.</span><span class="sxs-lookup"><span data-stu-id="8a03f-107">It comes with management options that you can use tooperform various tasks.</span></span>

| <span data-ttu-id="8a03f-108">Hanteringsalternativ</span><span class="sxs-lookup"><span data-stu-id="8a03f-108">Management option</span></span>          | <span data-ttu-id="8a03f-109">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="8a03f-109">Task</span></span>                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a03f-110">Direkta metoder</span><span class="sxs-lookup"><span data-stu-id="8a03f-110">Direct methods</span></span>             | <span data-ttu-id="8a03f-111">Gör en enhet som fungerar som startar eller stoppar skickar meddelanden eller starta om hello enhet.</span><span class="sxs-lookup"><span data-stu-id="8a03f-111">Make a device act such as starting or stopping sending messages or rebooting hello device.</span></span>                                        |
| <span data-ttu-id="8a03f-112">Dubbla önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="8a03f-112">Twin desired properties</span></span>    | <span data-ttu-id="8a03f-113">Placera en enhet i vissa lägen, till exempel ange en Indikator toogreen eller ange hello telemetri skicka too30 minuter.</span><span class="sxs-lookup"><span data-stu-id="8a03f-113">Put a device into certain states, such as setting an LED toogreen or setting hello telemetry send interval too30 minutes.</span></span>         |
| <span data-ttu-id="8a03f-114">Dubbla rapporterade egenskaper</span><span class="sxs-lookup"><span data-stu-id="8a03f-114">Twin reported properties</span></span>   | <span data-ttu-id="8a03f-115">Hämta hello rapporterade tillståndet för en enhet.</span><span class="sxs-lookup"><span data-stu-id="8a03f-115">Get hello reported state of a device.</span></span> <span data-ttu-id="8a03f-116">Till exempel rapporterar hello enheten hello Indikator blinkar nu.</span><span class="sxs-lookup"><span data-stu-id="8a03f-116">For example, hello device reports hello LED is blinking now.</span></span>                                    |
| <span data-ttu-id="8a03f-117">Dubbla taggar</span><span class="sxs-lookup"><span data-stu-id="8a03f-117">Twin tags</span></span>                  | <span data-ttu-id="8a03f-118">Lagra enhetsspecifika metadata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="8a03f-118">Store device-specific metadata in hello cloud.</span></span> <span data-ttu-id="8a03f-119">Till exempel hello distribution platsen för en Varuautomat.</span><span class="sxs-lookup"><span data-stu-id="8a03f-119">For example, hello deployment location of a vending machine.</span></span>                         |
| <span data-ttu-id="8a03f-120">Meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="8a03f-120">Cloud-to-device messages</span></span>   | <span data-ttu-id="8a03f-121">Skicka meddelanden tooa enhet.</span><span class="sxs-lookup"><span data-stu-id="8a03f-121">Send notifications tooa device.</span></span> <span data-ttu-id="8a03f-122">Till exempel ”är det mycket troligt toorain idag.</span><span class="sxs-lookup"><span data-stu-id="8a03f-122">For example, "It is very likely toorain today.</span></span> <span data-ttu-id="8a03f-123">Glöm inte toobring ett samlingsnamn ”.</span><span class="sxs-lookup"><span data-stu-id="8a03f-123">Don't forget toobring an umbrella."</span></span>              |
| <span data-ttu-id="8a03f-124">Enheten dubbla frågor</span><span class="sxs-lookup"><span data-stu-id="8a03f-124">Device twin queries</span></span>        | <span data-ttu-id="8a03f-125">Fråga alla enheten twins tooretrieve dem med valfri villkor, till exempel identifiera hello-enheter som är tillgängliga för användning.</span><span class="sxs-lookup"><span data-stu-id="8a03f-125">Query all device twins tooretrieve those with arbitrary conditions, such as identifying hello devices that are available for use.</span></span> |

<span data-ttu-id="8a03f-126">Mer detaljerad förklaring på hello skillnader och vägledning om hur du använder dessa alternativ finns [enhet till moln kommunikation vägledning](iot-hub-devguide-d2c-guidance.md) och [moln till enhet kommunikation vägledning](iot-hub-devguide-c2d-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="8a03f-126">For more detailed explanation on hello differences and guidance on using these options, see [Device-to-cloud communication guidance](iot-hub-devguide-d2c-guidance.md) and [Cloud-to-device communication guidance](iot-hub-devguide-c2d-guidance.md).</span></span>

> [!NOTE]
> <span data-ttu-id="8a03f-127">Enhetstvillingar är JSON-dokument som lagrar information om enhetstillstånd (metadata, konfigurationer och villkor).</span><span class="sxs-lookup"><span data-stu-id="8a03f-127">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="8a03f-128">IoT-hubb kvarstår en enhet dubbla för varje enhet som ansluter tooit.</span><span class="sxs-lookup"><span data-stu-id="8a03f-128">IoT Hub persists a device twin for each device that connects tooit.</span></span> <span data-ttu-id="8a03f-129">Läs mer om enheten twins [Kom igång med enheten twins](iot-hub-node-node-twin-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="8a03f-129">For more information about device twins, see [Get started with device twins](iot-hub-node-node-twin-getstarted.md).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="8a03f-130">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="8a03f-130">What you learn</span></span>

<span data-ttu-id="8a03f-131">Du lär dig med olika alternativ på utvecklingsdatorn iothub explorer.</span><span class="sxs-lookup"><span data-stu-id="8a03f-131">You learn using iothub-explorer with various management options on your development machine.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="8a03f-132">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="8a03f-132">What you do</span></span>

<span data-ttu-id="8a03f-133">Kör iothub explorer med olika hanteringsalternativ för.</span><span class="sxs-lookup"><span data-stu-id="8a03f-133">Run iothub-explorer with various management options.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8a03f-134">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="8a03f-134">What you need</span></span>

- <span data-ttu-id="8a03f-135">Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="8a03f-135">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="8a03f-136">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8a03f-136">An active Azure subscription.</span></span>
  - <span data-ttu-id="8a03f-137">En Azure IoT-hubb i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8a03f-137">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="8a03f-138">Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8a03f-138">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="8a03f-139">Kontrollera att enheten kör med hello klientprogrammet under den här kursen.</span><span class="sxs-lookup"><span data-stu-id="8a03f-139">Make sure your device is running with hello client application during this tutorial.</span></span>
- <span data-ttu-id="8a03f-140">iothub-explorer [installera iothub explorer](https://github.com/azure/iothub-explorer) på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="8a03f-140">iothub-explorer, [Install iothub-explorer](https://github.com/azure/iothub-explorer) on your development machine.</span></span>

## <a name="connect-tooyour-iot-hub"></a><span data-ttu-id="8a03f-141">Ansluta tooyour IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="8a03f-141">Connect tooyour IoT hub</span></span>

<span data-ttu-id="8a03f-142">Anslut tooyour IoT-hubb genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8a03f-142">Connect tooyour IoT hub by running hello following command:</span></span>

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a><span data-ttu-id="8a03f-143">Använd iothub explorer med direkta metoder</span><span class="sxs-lookup"><span data-stu-id="8a03f-143">Use iothub-explorer with direct methods</span></span>

<span data-ttu-id="8a03f-144">Anropa hello `start` metod i hello enheten app toosend meddelanden tooyour IoT-hubb genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8a03f-144">Invoke hello `start` method in hello device app toosend messages tooyour IoT hub by running hello following command:</span></span>

```bash
iothub-explorer device-method <your device Id> start
```

<span data-ttu-id="8a03f-145">Anropa hello `stop` metod i hello enheten app toostop skickar meddelanden tooyour IoT-hubb genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8a03f-145">Invoke hello `stop` method in hello device app toostop sending messages tooyour IoT hub by running hello following command:</span></span>

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a><span data-ttu-id="8a03f-146">Använd iothub explorer med dubblas egenskaper</span><span class="sxs-lookup"><span data-stu-id="8a03f-146">Use iothub-explorer with twin’s desired properties</span></span>

<span data-ttu-id="8a03f-147">Ange ett intervall önskade egenskapen = 3000 genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8a03f-147">Set a desired property interval = 3000 by running hello following command:</span></span>

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

<span data-ttu-id="8a03f-148">Den här egenskapen kan läsas av enheten.</span><span class="sxs-lookup"><span data-stu-id="8a03f-148">This property can be read by your device.</span></span>

## <a name="use-iothub-explorer-with-twins-reported-properties"></a><span data-ttu-id="8a03f-149">Använd iothub explorer med dubblas rapporterade egenskaper</span><span class="sxs-lookup"><span data-stu-id="8a03f-149">Use iothub-explorer with twin’s reported properties</span></span>

<span data-ttu-id="8a03f-150">Hämta hello rapporterade egenskaperna för hello enhet genom att köra hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="8a03f-150">Get hello reported properties of hello device by running hello following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="8a03f-151">En av hello egenskaper är $metadata. $lastUpdated som visar hello senast enheten skickar och tar emot ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="8a03f-151">One of hello properties is $metadata.$lastUpdated which shows hello last time this device sends or receives a message.</span></span>

## <a name="use-iothub-explorer-with-twins-tags"></a><span data-ttu-id="8a03f-152">Använd iothub explorer med dubblas taggar</span><span class="sxs-lookup"><span data-stu-id="8a03f-152">Use iothub-explorer with twin’s tags</span></span>

<span data-ttu-id="8a03f-153">Visa hello taggar och egenskaperna för hello enhet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8a03f-153">Display hello tags and properties of hello device by running hello following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="8a03f-154">Lägga till en roll för fältet = temperatur & fuktighet toohello enheten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8a03f-154">Add a field role = temperature&humidity toohello device by running hello following command:</span></span>

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a><span data-ttu-id="8a03f-155">Använda iothub explorer med meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="8a03f-155">Use iothub-explorer with Cloud-to-device messages</span></span>

<span data-ttu-id="8a03f-156">Skicka en ”Hello World” meddelande toohello enhet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8a03f-156">Send a "Hello World" message toohello device by running hello following command:</span></span>

```bash
iothub-explorer send <device-id> "Hello World"
```

<span data-ttu-id="8a03f-157">Se [använder iothub explorer toosend och ta emot meddelanden mellan din enhet och IoT-hubb](iot-hub-explorer-cloud-device-messaging.md) verkliga scenarier för att använda det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="8a03f-157">See [Use iothub-explorer toosend and receive messages between your device and IoT Hub](iot-hub-explorer-cloud-device-messaging.md) for a real scenario of using this command.</span></span>

## <a name="use-iothub-explorer-with-device-twins-queries"></a><span data-ttu-id="8a03f-158">Använda iothub explorer med enheten twins frågor</span><span class="sxs-lookup"><span data-stu-id="8a03f-158">Use iothub-explorer with device twins queries</span></span>

<span data-ttu-id="8a03f-159">Fråga enheter med en tagg av rollen = 'temperatur- och fuktighetskonsekvens' genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8a03f-159">Query devices with a tag of role = 'temperature&humidity' by running hello following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

<span data-ttu-id="8a03f-160">Fråga alla enheter utom de som har en tagg av rollen = 'temperatur- och fuktighetskonsekvens' genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="8a03f-160">Query all devices except those with a tag of role = 'temperature&humidity' by running hello following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a><span data-ttu-id="8a03f-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a03f-161">Next steps</span></span>

<span data-ttu-id="8a03f-162">Du har lärt dig hur toouse iothub-explorer med olika hanteringsalternativ för.</span><span class="sxs-lookup"><span data-stu-id="8a03f-162">You've learned how toouse iothub-explorer with various management options.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

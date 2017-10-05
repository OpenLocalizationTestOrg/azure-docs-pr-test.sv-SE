---
title: Azure IoT-enhetshantering med iothub explorer | Microsoft Docs
description: "Verktyget iothub explorer CLI för Azure IoT Hub-enhetshantering med direkta metoder och de två alternativ för egenskaper."
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
ms.openlocfilehash: 5b7a5057bdfb5920fbb5759bed1f5561cfa1d7e0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a><span data-ttu-id="3785c-104">Använda iothub explorer för hantering av Azure IoT Hub-enheter</span><span class="sxs-lookup"><span data-stu-id="3785c-104">Use iothub-explorer for Azure IoT Hub device management</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="3785c-106">[iothub explorer](https://github.com/azure/iothub-explorer) är ett CLI-verktyg som du kör på en värd datorn för att hantera enheten identiteter i din IoT-hubb registret.</span><span class="sxs-lookup"><span data-stu-id="3785c-106">[iothub-explorer](https://github.com/azure/iothub-explorer) is a CLI tool that you run on a host computer to manage device identities in your IoT hub registry.</span></span> <span data-ttu-id="3785c-107">Den innehåller alternativ som du kan använda för att utföra olika uppgifter.</span><span class="sxs-lookup"><span data-stu-id="3785c-107">It comes with management options that you can use to perform various tasks.</span></span>

| <span data-ttu-id="3785c-108">Hanteringsalternativ</span><span class="sxs-lookup"><span data-stu-id="3785c-108">Management option</span></span>          | <span data-ttu-id="3785c-109">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="3785c-109">Task</span></span>                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3785c-110">Direkta metoder</span><span class="sxs-lookup"><span data-stu-id="3785c-110">Direct methods</span></span>             | <span data-ttu-id="3785c-111">Gör en enhet som agerar som startar eller stoppar skickar meddelanden eller starta om enheten.</span><span class="sxs-lookup"><span data-stu-id="3785c-111">Make a device act such as starting or stopping sending messages or rebooting the device.</span></span>                                        |
| <span data-ttu-id="3785c-112">Dubbla önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="3785c-112">Twin desired properties</span></span>    | <span data-ttu-id="3785c-113">Lägga till en enhet i vissa tillstånd, till exempel ange en Indikator till grönt eller inställningen telemetri skicka intervall till 30 minuter.</span><span class="sxs-lookup"><span data-stu-id="3785c-113">Put a device into certain states, such as setting an LED to green or setting the telemetry send interval to 30 minutes.</span></span>         |
| <span data-ttu-id="3785c-114">Dubbla rapporterade egenskaper</span><span class="sxs-lookup"><span data-stu-id="3785c-114">Twin reported properties</span></span>   | <span data-ttu-id="3785c-115">Hämta rapporterat status för en enhet.</span><span class="sxs-lookup"><span data-stu-id="3785c-115">Get the reported state of a device.</span></span> <span data-ttu-id="3785c-116">Till exempel rapporterar enheten Indikatorn blinkar nu.</span><span class="sxs-lookup"><span data-stu-id="3785c-116">For example, the device reports the LED is blinking now.</span></span>                                    |
| <span data-ttu-id="3785c-117">Dubbla taggar</span><span class="sxs-lookup"><span data-stu-id="3785c-117">Twin tags</span></span>                  | <span data-ttu-id="3785c-118">Lagra enhetsspecifika metadata i molnet.</span><span class="sxs-lookup"><span data-stu-id="3785c-118">Store device-specific metadata in the cloud.</span></span> <span data-ttu-id="3785c-119">Till exempel distributionsplatsen för en Varuautomat.</span><span class="sxs-lookup"><span data-stu-id="3785c-119">For example, the deployment location of a vending machine.</span></span>                         |
| <span data-ttu-id="3785c-120">Meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="3785c-120">Cloud-to-device messages</span></span>   | <span data-ttu-id="3785c-121">Skicka meddelanden till en enhet.</span><span class="sxs-lookup"><span data-stu-id="3785c-121">Send notifications to a device.</span></span> <span data-ttu-id="3785c-122">Till exempel ”är det mycket sannolikt att regn idag.</span><span class="sxs-lookup"><span data-stu-id="3785c-122">For example, "It is very likely to rain today.</span></span> <span data-ttu-id="3785c-123">Glöm inte att få en övergripande ”.</span><span class="sxs-lookup"><span data-stu-id="3785c-123">Don't forget to bring an umbrella."</span></span>              |
| <span data-ttu-id="3785c-124">Enheten dubbla frågor</span><span class="sxs-lookup"><span data-stu-id="3785c-124">Device twin queries</span></span>        | <span data-ttu-id="3785c-125">Fråga alla enheten twins för att hämta dem med valfri villkor, till exempel identifiera de enheter som är tillgängliga för användning.</span><span class="sxs-lookup"><span data-stu-id="3785c-125">Query all device twins to retrieve those with arbitrary conditions, such as identifying the devices that are available for use.</span></span> |

<span data-ttu-id="3785c-126">Mer detaljerad förklaring om skillnaderna och vägledning om hur du använder dessa alternativ finns [enhet till moln kommunikation vägledning](iot-hub-devguide-d2c-guidance.md) och [moln till enhet kommunikation vägledning](iot-hub-devguide-c2d-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="3785c-126">For more detailed explanation on the differences and guidance on using these options, see [Device-to-cloud communication guidance](iot-hub-devguide-d2c-guidance.md) and [Cloud-to-device communication guidance](iot-hub-devguide-c2d-guidance.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3785c-127">Enhetstvillingar är JSON-dokument som lagrar information om enhetstillstånd (metadata, konfigurationer och villkor).</span><span class="sxs-lookup"><span data-stu-id="3785c-127">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="3785c-128">IoT-hubb kvarstår en enhet dubbla för varje enhet som ansluter till den.</span><span class="sxs-lookup"><span data-stu-id="3785c-128">IoT Hub persists a device twin for each device that connects to it.</span></span> <span data-ttu-id="3785c-129">Läs mer om enheten twins [Kom igång med enheten twins](iot-hub-node-node-twin-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="3785c-129">For more information about device twins, see [Get started with device twins](iot-hub-node-node-twin-getstarted.md).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="3785c-130">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="3785c-130">What you learn</span></span>

<span data-ttu-id="3785c-131">Du lär dig med olika alternativ på utvecklingsdatorn iothub explorer.</span><span class="sxs-lookup"><span data-stu-id="3785c-131">You learn using iothub-explorer with various management options on your development machine.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="3785c-132">Vad du gör</span><span class="sxs-lookup"><span data-stu-id="3785c-132">What you do</span></span>

<span data-ttu-id="3785c-133">Kör iothub explorer med olika hanteringsalternativ för.</span><span class="sxs-lookup"><span data-stu-id="3785c-133">Run iothub-explorer with various management options.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="3785c-134">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="3785c-134">What you need</span></span>

- <span data-ttu-id="3785c-135">Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar följande krav:</span><span class="sxs-lookup"><span data-stu-id="3785c-135">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="3785c-136">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3785c-136">An active Azure subscription.</span></span>
  - <span data-ttu-id="3785c-137">En Azure IoT-hubb i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3785c-137">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="3785c-138">Ett klientprogram som skickar meddelanden till din Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3785c-138">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="3785c-139">Kontrollera att enheten kör med klientprogrammet under den här kursen.</span><span class="sxs-lookup"><span data-stu-id="3785c-139">Make sure your device is running with the client application during this tutorial.</span></span>
- <span data-ttu-id="3785c-140">iothub-explorer [installera iothub explorer](https://github.com/azure/iothub-explorer) på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="3785c-140">iothub-explorer, [Install iothub-explorer](https://github.com/azure/iothub-explorer) on your development machine.</span></span>

## <a name="connect-to-your-iot-hub"></a><span data-ttu-id="3785c-141">Ansluta till din IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="3785c-141">Connect to your IoT hub</span></span>

<span data-ttu-id="3785c-142">Anslut till din IoT-hubb genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-142">Connect to your IoT hub by running the following command:</span></span>

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a><span data-ttu-id="3785c-143">Använd iothub explorer med direkta metoder</span><span class="sxs-lookup"><span data-stu-id="3785c-143">Use iothub-explorer with direct methods</span></span>

<span data-ttu-id="3785c-144">Anropa den `start` metod i appen enhet för att skicka meddelanden till din IoT-hubb genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-144">Invoke the `start` method in the device app to send messages to your IoT hub by running the following command:</span></span>

```bash
iothub-explorer device-method <your device Id> start
```

<span data-ttu-id="3785c-145">Anropa den `stop` metod i appen enheten slutar skicka meddelanden till din IoT-hubb genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-145">Invoke the `stop` method in the device app to stop sending messages to your IoT hub by running the following command:</span></span>

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a><span data-ttu-id="3785c-146">Använd iothub explorer med dubblas egenskaper</span><span class="sxs-lookup"><span data-stu-id="3785c-146">Use iothub-explorer with twin’s desired properties</span></span>

<span data-ttu-id="3785c-147">Ange ett intervall önskade egenskapen = 3000 genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-147">Set a desired property interval = 3000 by running the following command:</span></span>

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

<span data-ttu-id="3785c-148">Den här egenskapen kan läsas av enheten.</span><span class="sxs-lookup"><span data-stu-id="3785c-148">This property can be read by your device.</span></span>

## <a name="use-iothub-explorer-with-twins-reported-properties"></a><span data-ttu-id="3785c-149">Använd iothub explorer med dubblas rapporterade egenskaper</span><span class="sxs-lookup"><span data-stu-id="3785c-149">Use iothub-explorer with twin’s reported properties</span></span>

<span data-ttu-id="3785c-150">Hämta rapporterade egenskaperna för enheten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-150">Get the reported properties of the device by running the following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="3785c-151">En av egenskaperna är $metadata. $lastUpdated som visar senast enheten skickar och tar emot ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="3785c-151">One of the properties is $metadata.$lastUpdated which shows the last time this device sends or receives a message.</span></span>

## <a name="use-iothub-explorer-with-twins-tags"></a><span data-ttu-id="3785c-152">Använd iothub explorer med dubblas taggar</span><span class="sxs-lookup"><span data-stu-id="3785c-152">Use iothub-explorer with twin’s tags</span></span>

<span data-ttu-id="3785c-153">Visa taggar och egenskaper för enheten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-153">Display the tags and properties of the device by running the following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="3785c-154">Lägga till en roll för fältet = temperatur & fuktighet till enheten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-154">Add a field role = temperature&humidity to the device by running the following command:</span></span>

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a><span data-ttu-id="3785c-155">Använda iothub explorer med meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="3785c-155">Use iothub-explorer with Cloud-to-device messages</span></span>

<span data-ttu-id="3785c-156">Skicka ett meddelande om ”Hello World” till enheten genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-156">Send a "Hello World" message to the device by running the following command:</span></span>

```bash
iothub-explorer send <device-id> "Hello World"
```

<span data-ttu-id="3785c-157">Se [använda iothub explorer för att skicka och ta emot meddelanden mellan enheten och IoT-hubb](iot-hub-explorer-cloud-device-messaging.md) verkliga scenarier för att använda det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="3785c-157">See [Use iothub-explorer to send and receive messages between your device and IoT Hub](iot-hub-explorer-cloud-device-messaging.md) for a real scenario of using this command.</span></span>

## <a name="use-iothub-explorer-with-device-twins-queries"></a><span data-ttu-id="3785c-158">Använda iothub explorer med enheten twins frågor</span><span class="sxs-lookup"><span data-stu-id="3785c-158">Use iothub-explorer with device twins queries</span></span>

<span data-ttu-id="3785c-159">Fråga enheter med en tagg av rollen = 'temperatur- och fuktighetskonsekvens' genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-159">Query devices with a tag of role = 'temperature&humidity' by running the following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

<span data-ttu-id="3785c-160">Fråga alla enheter utom de som har en tagg av rollen = 'temperatur- och fuktighetskonsekvens' genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3785c-160">Query all devices except those with a tag of role = 'temperature&humidity' by running the following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a><span data-ttu-id="3785c-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3785c-161">Next steps</span></span>

<span data-ttu-id="3785c-162">Du har lärt dig hur du använder iothub explorer med olika hanteringsalternativ för.</span><span class="sxs-lookup"><span data-stu-id="3785c-162">You've learned how to use iothub-explorer with various management options.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

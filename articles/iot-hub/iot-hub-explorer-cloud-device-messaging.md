---
title: meddelanden med iothub explorer aaaManage Azure IoT Hub moln enhet | Microsoft Docs
description: "Lär dig hur toouse hello iothub explorer CLI verktyget toomonitor toocloud (D2C) meddelanden och meddelanden moln toodevice (C2D) i Azure IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: iothub explorer moln enhet messaging iot-hubb molnet toodevice molnet toodevice meddelanden
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 741383b5631714cc2fef3309541fd2117aa7824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-toosend-and-receive-messages-between-your-device-and-iot-hub"></a><span data-ttu-id="54fc5-104">Använd iothub explorer toosend och ta emot meddelanden mellan din enhet och IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="54fc5-104">Use iothub-explorer toosend and receive messages between your device and IoT Hub</span></span>

![Diagram för slutpunkt till slutpunkt](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="54fc5-106">[iothub explorer](https://github.com/azure/iothub-explorer) har en handfull kommandon som gör det enklare hantering av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="54fc5-106">[iothub-explorer](https://github.com/azure/iothub-explorer) has a handful of commands that makes IoT Hub management easier.</span></span> <span data-ttu-id="54fc5-107">Den här självstudiekursen fokuserar på hur toouse iothub explorer toosend och ta emot meddelanden mellan enheten och din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="54fc5-107">This tutorial focuses on how toouse iothub-explorer toosend and receive messages between your device and your IoT hub.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="54fc5-108">Vad får du lära dig</span><span class="sxs-lookup"><span data-stu-id="54fc5-108">What you will learn</span></span>

<span data-ttu-id="54fc5-109">Du lär dig hur toouse iothub explorer toomonitor enhet till moln meddelanden och meddelanden som toosend moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="54fc5-109">You learn how toouse iothub-explorer toomonitor device-to-cloud messages and toosend cloud-to-device messages.</span></span> <span data-ttu-id="54fc5-110">Meddelanden från enhet till moln kan vara sensordata att enheten samlar in och skickar sedan tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="54fc5-110">Device-to-cloud messages could be sensor data that your device collects and then sends tooyour IoT hub.</span></span> <span data-ttu-id="54fc5-111">Moln till enhet meddelanden kan vara att din IoT-hubb skickar tooyour enhet tooblink en Indikator är anslutna tooyour enhet-kommandon.</span><span class="sxs-lookup"><span data-stu-id="54fc5-111">Cloud-to-device messages could be commands that your IoT hub sends tooyour device tooblink an LED that is connected tooyour device.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="54fc5-112">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="54fc5-112">What you will do</span></span>

- <span data-ttu-id="54fc5-113">Använd iothub explorer toomonitor meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="54fc5-113">Use iothub-explorer toomonitor device-to-cloud messages.</span></span>
- <span data-ttu-id="54fc5-114">Använd iothub explorer toosend moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="54fc5-114">Use iothub-explorer toosend cloud-to-device messages.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="54fc5-115">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="54fc5-115">What you need</span></span>

- <span data-ttu-id="54fc5-116">Kursen [konfigurera enheten](iot-hub-raspberry-pi-kit-node-get-started.md) slutförts som omfattar hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="54fc5-116">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="54fc5-117">En aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54fc5-117">An active Azure subscription.</span></span>
  - <span data-ttu-id="54fc5-118">En Azure IoT-hubb i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54fc5-118">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="54fc5-119">Ett klientprogram som skickar meddelanden tooyour Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="54fc5-119">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="54fc5-120">iothub-explorer.</span><span class="sxs-lookup"><span data-stu-id="54fc5-120">iothub-explorer.</span></span> <span data-ttu-id="54fc5-121">([Installera iothub explorer](https://github.com/azure/iothub-explorer))</span><span class="sxs-lookup"><span data-stu-id="54fc5-121">([Install iothub-explorer](https://github.com/azure/iothub-explorer))</span></span>

## <a name="monitor-device-to-cloud-messages"></a><span data-ttu-id="54fc5-122">Övervaka meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="54fc5-122">Monitor device-to-cloud messages</span></span>

<span data-ttu-id="54fc5-123">toomonitor meddelanden som skickas från din enhet tooyour IoT-hubb, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="54fc5-123">toomonitor messages that are sent from your device tooyour IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="54fc5-124">Öppna ett konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="54fc5-124">Open a console window.</span></span>
1. <span data-ttu-id="54fc5-125">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="54fc5-125">Run hello following command:</span></span>

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > <span data-ttu-id="54fc5-126">Hämta `<device-id>` och `<IoTHubConnectionString>` från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="54fc5-126">Get `<device-id>` and `<IoTHubConnectionString>` from your IoT hub.</span></span> <span data-ttu-id="54fc5-127">Kontrollera att du är klar hello tidigare kursen.</span><span class="sxs-lookup"><span data-stu-id="54fc5-127">Make sure you've finished hello previous tutorial.</span></span> <span data-ttu-id="54fc5-128">Eller så kan du försöka toouse `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` om du har `HostName`, `SharedAccessKeyName` och `SharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="54fc5-128">Or you can try toouse `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` if you have `HostName`, `SharedAccessKeyName` and `SharedAccessKey`.</span></span>

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="54fc5-129">Skicka meddelanden från moln till enhet</span><span class="sxs-lookup"><span data-stu-id="54fc5-129">Send cloud-to-device messages</span></span>

<span data-ttu-id="54fc5-130">toosend ett meddelande från IoT-hubb tooyour enheten, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="54fc5-130">toosend a message from your IoT hub tooyour device, follow these steps:</span></span>

1. <span data-ttu-id="54fc5-131">Öppna ett konsolfönster.</span><span class="sxs-lookup"><span data-stu-id="54fc5-131">Open a console window.</span></span>
1. <span data-ttu-id="54fc5-132">Starta en session på din IoT-hubb genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="54fc5-132">Start a session on your IoT hub by running hello following command:</span></span>

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. <span data-ttu-id="54fc5-133">Skicka ett meddelande tooyour enheten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="54fc5-133">Send a message tooyour device by running hello following command:</span></span>

   ```bash
   iothub-explorer send <device-id> <message>
   ```

<span data-ttu-id="54fc5-134">hello kommandot blinkar hello Indikator som är anslutna tooyour enhet och skickar hello meddelandet tooyour enhet.</span><span class="sxs-lookup"><span data-stu-id="54fc5-134">hello command blinks hello LED that is connected tooyour device and sends hello message tooyour device.</span></span>

> [!Note]
> <span data-ttu-id="54fc5-135">Det behövs ingen hälsningspaket enheten toosend en separat ack kommandot tillbaka tooyour IoT-hubb vid mottagning av hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="54fc5-135">There is no need for hello device toosend a separate ack command back tooyour IoT hub upon receiving hello message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54fc5-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54fc5-136">Next steps</span></span>

<span data-ttu-id="54fc5-137">Du har lärt dig hur toomonitor enhet till moln meddelanden och meddelanden moln till enhet mellan din IoT-enhet och Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54fc5-137">You’ve learned how toomonitor device-to-cloud messages and send cloud-to-device messages between your IoT device and Azure IoT Hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

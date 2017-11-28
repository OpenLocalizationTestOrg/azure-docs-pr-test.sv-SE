---
title: aaaAzure IoT-hubb skalning | Microsoft Docs
description: "Hur tooscale din IoT-hubb toosupport din förväntade genomströmningen. Innehåller en sammanfattning av hello stöds genomflödet för varje nivå och alternativ för delning."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3b8bf6c44631c65b34b69752d9043c21db24bb01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-your-iot-hub-solution"></a><span data-ttu-id="e4a4c-104">Skala din lösning för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="e4a4c-104">Scale your IoT hub solution</span></span>
<span data-ttu-id="e4a4c-105">Azure IoT-hubb kan stödja tooa miljoner samtidigt anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-105">Azure IoT Hub can support up tooa million simultaneously connected devices.</span></span> <span data-ttu-id="e4a4c-106">Mer information finns i [IoT-hubb priser][lnk-pricing].</span><span class="sxs-lookup"><span data-stu-id="e4a4c-106">For more information, see [IoT Hub pricing][lnk-pricing].</span></span> <span data-ttu-id="e4a4c-107">Varje IoT Hub-enhet kan ett visst antal dagliga meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-107">Each IoT Hub unit allows a certain number of daily messages.</span></span>

<span data-ttu-id="e4a4c-108">tooproperly skala din lösning bör du överväga att viss användningen av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-108">tooproperly scale your solution, consider your particular use of IoT Hub.</span></span> <span data-ttu-id="e4a4c-109">I synnerhet Tänk hello krävs belastning genomströmning för hello följande typer av åtgärder:</span><span class="sxs-lookup"><span data-stu-id="e4a4c-109">In particular, consider hello required peak throughput for hello following categories of operations:</span></span>

* <span data-ttu-id="e4a4c-110">Meddelanden från enheten till molnet</span><span class="sxs-lookup"><span data-stu-id="e4a4c-110">Device-to-cloud messages</span></span>
* <span data-ttu-id="e4a4c-111">Meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="e4a4c-111">Cloud-to-device messages</span></span>
* <span data-ttu-id="e4a4c-112">Identitetsregisteråtgärder</span><span class="sxs-lookup"><span data-stu-id="e4a4c-112">Identity registry operations</span></span>

<span data-ttu-id="e4a4c-113">I toothis genomströmning informationen, se [IoT-hubb kvoter och begränsningar] [ IoT Hub quotas and throttles] och därefter utforma din lösning.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-113">In addition toothis throughput information, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles] and design your solution accordingly.</span></span>

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a><span data-ttu-id="e4a4c-114">Enhet till moln och moln till enhet genomströmningen</span><span class="sxs-lookup"><span data-stu-id="e4a4c-114">Device-to-cloud and cloud-to-device message throughput</span></span>
<span data-ttu-id="e4a4c-115">hello bästa sätt toosize en IoT-hubb-lösning är tooevaluate hello-trafik på grundval av per enhet.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-115">hello best way toosize an IoT Hub solution is tooevaluate hello traffic on a per-unit basis.</span></span>

<span data-ttu-id="e4a4c-116">Meddelanden från enhet till moln följer dessa riktlinjer för varaktigt dataflöde.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-116">Device-to-cloud messages follow these sustained throughput guidelines.</span></span>

| <span data-ttu-id="e4a4c-117">Nivå</span><span class="sxs-lookup"><span data-stu-id="e4a4c-117">Tier</span></span> | <span data-ttu-id="e4a4c-118">Varaktigt dataflöde</span><span class="sxs-lookup"><span data-stu-id="e4a4c-118">Sustained throughput</span></span> | <span data-ttu-id="e4a4c-119">Varaktiga överföringshastighet</span><span class="sxs-lookup"><span data-stu-id="e4a4c-119">Sustained send rate</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e4a4c-120">S1</span><span class="sxs-lookup"><span data-stu-id="e4a4c-120">S1</span></span> |<span data-ttu-id="e4a4c-121">Konfigurera too1111 KB per minut per enhet</span><span class="sxs-lookup"><span data-stu-id="e4a4c-121">Up too1111 KB/minute per unit</span></span><br/><span data-ttu-id="e4a4c-122">(1,5 GB/dag/unit)</span><span class="sxs-lookup"><span data-stu-id="e4a4c-122">(1.5 GB/day/unit)</span></span> |<span data-ttu-id="e4a4c-123">Medelvärde för 278 meddelanden per minut per enhet</span><span class="sxs-lookup"><span data-stu-id="e4a4c-123">Average of 278 messages/minute per unit</span></span><br/><span data-ttu-id="e4a4c-124">(400 000 meddelanden/dag per enhet)</span><span class="sxs-lookup"><span data-stu-id="e4a4c-124">(400,000 messages/day per unit)</span></span> |
| <span data-ttu-id="e4a4c-125">S2</span><span class="sxs-lookup"><span data-stu-id="e4a4c-125">S2</span></span> |<span data-ttu-id="e4a4c-126">Konfigurera too16 MB per minut per enhet</span><span class="sxs-lookup"><span data-stu-id="e4a4c-126">Up too16 MB/minute per unit</span></span><br/><span data-ttu-id="e4a4c-127">(22,8 GB/dag/unit)</span><span class="sxs-lookup"><span data-stu-id="e4a4c-127">(22.8 GB/day/unit)</span></span> |<span data-ttu-id="e4a4c-128">Medelvärde för 4,167 meddelanden per minut per enhet</span><span class="sxs-lookup"><span data-stu-id="e4a4c-128">Average of 4,167 messages/minute per unit</span></span><br/><span data-ttu-id="e4a4c-129">(6 miljoner meddelanden/dag per enhet)</span><span class="sxs-lookup"><span data-stu-id="e4a4c-129">(6 million messages/day per unit)</span></span> |
| <span data-ttu-id="e4a4c-130">S3</span><span class="sxs-lookup"><span data-stu-id="e4a4c-130">S3</span></span> |<span data-ttu-id="e4a4c-131">Konfigurera too814 MB per minut per enhet</span><span class="sxs-lookup"><span data-stu-id="e4a4c-131">Up too814 MB/minute per unit</span></span><br/><span data-ttu-id="e4a4c-132">(1144.4 GB/dag/unit)</span><span class="sxs-lookup"><span data-stu-id="e4a4c-132">(1144.4 GB/day/unit)</span></span> |<span data-ttu-id="e4a4c-133">Medelvärde för 208,333 meddelanden per minut per enhet</span><span class="sxs-lookup"><span data-stu-id="e4a4c-133">Average of 208,333 messages/minute per unit</span></span><br/><span data-ttu-id="e4a4c-134">(300 miljoner meddelanden/dag per enhet)</span><span class="sxs-lookup"><span data-stu-id="e4a4c-134">(300 million messages/day per unit)</span></span> |

## <a name="identity-registry-operation-throughput"></a><span data-ttu-id="e4a4c-135">Identitet registret åtgärden genomflöde</span><span class="sxs-lookup"><span data-stu-id="e4a4c-135">Identity registry operation throughput</span></span>
<span data-ttu-id="e4a4c-136">IoT-hubb identitet registret åtgärder är inte avsedda toobe körning åtgärder, som de är oftast relaterade toodevice etablering.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-136">IoT Hub identity registry operations are not supposed toobe run-time operations, as they are mostly related toodevice provisioning.</span></span>

<span data-ttu-id="e4a4c-137">Specifika burst prestanda siffror finns [IoT-hubb kvoter och begränsningar][IoT Hub quotas and throttles].</span><span class="sxs-lookup"><span data-stu-id="e4a4c-137">For specific burst performance numbers, see [IoT Hub quotas and throttles][IoT Hub quotas and throttles].</span></span>

## <a name="sharding"></a><span data-ttu-id="e4a4c-138">Horisontell partitionering</span><span class="sxs-lookup"><span data-stu-id="e4a4c-138">Sharding</span></span>
<span data-ttu-id="e4a4c-139">När en enda IoT-hubb kan skala toomillions av enheter, kräver ibland din lösning specifika prestandaegenskaper som en enda IoT-hubb inte kan garantera.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-139">While a single IoT hub can scale toomillions of devices, sometimes your solution requires specific performance characteristics that a single IoT hub cannot guarantee.</span></span> <span data-ttu-id="e4a4c-140">I så fall bör du partitionera dina enheter till flera IoT-hubbar.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-140">In that case, it is recommended that you partition your devices into multiple IoT hubs.</span></span> <span data-ttu-id="e4a4c-141">Flera IoT-hubbar Utjämna trafiken belastning och hämta hello krävs genomströmning eller åtgärden priser som krävs.</span><span class="sxs-lookup"><span data-stu-id="e4a4c-141">Multiple IoT hubs smooth traffic bursts and obtain hello required throughput or operation rates that are required.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4a4c-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4a4c-142">Next steps</span></span>
<span data-ttu-id="e4a4c-143">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="e4a4c-143">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="e4a4c-144">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="e4a4c-144">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="e4a4c-145">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="e4a4c-145">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

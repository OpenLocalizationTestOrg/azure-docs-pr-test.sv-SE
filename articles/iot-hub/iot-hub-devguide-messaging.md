---
title: aaaUnderstand Azure IoT Hub-meddelanden | Microsoft Docs
description: "Utvecklarhandbok - enhet till moln och moln till enhet meddelanden med IoT-hubben. Innehåller information om meddelandeformat och stöds kommunikationsprotokoll."
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
ms.openlocfilehash: a610741e23e243f392f1c042f9ab4a00d42f734f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a><span data-ttu-id="e6d3a-104">Enhet till moln och moln till enhet meddelanden med IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="e6d3a-104">Device-to-cloud and cloud-to-device messaging with IoT Hub</span></span>

<span data-ttu-id="e6d3a-105">Använd IoT-hubb messaging toocommunicate med dina enheter genom att:</span><span class="sxs-lookup"><span data-stu-id="e6d3a-105">Use IoT Hub messaging toocommunicate with your devices by:</span></span>

* <span data-ttu-id="e6d3a-106">Skicka [enhet till moln] [ lnk-d2c] meddelanden från enheter tooyour lösningen serverdel.</span><span class="sxs-lookup"><span data-stu-id="e6d3a-106">Sending [device-to-cloud][lnk-d2c] messages from your devices tooyour solution back end.</span></span>
* <span data-ttu-id="e6d3a-107">Skicka [moln till enhet] [ lnk-c2d] meddelanden från hello-lösningen avslutar tooyour enheter.</span><span class="sxs-lookup"><span data-stu-id="e6d3a-107">Sending [cloud-to-device][lnk-c2d] messages from hello solution back end tooyour devices.</span></span>

<span data-ttu-id="e6d3a-108">Grundläggande egenskaper för IoT-hubb meddelandefunktioner är hello tillförlitlighet och hållbarhet av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e6d3a-108">Core properties of IoT Hub messaging functionality are hello reliability and durability of messages.</span></span> <span data-ttu-id="e6d3a-109">De här egenskaperna aktivera återhämtning toointermittent anslutning på hello enheten sida och tooload ger spikar i diagrammet i händelsebearbetning på hello molnet sida.</span><span class="sxs-lookup"><span data-stu-id="e6d3a-109">These properties enable resilience toointermittent connectivity on hello device side, and tooload spikes in event processing on hello cloud side.</span></span> <span data-ttu-id="e6d3a-110">IoT-hubb implementerar *minst en gång* leverans garanterar för både enhet till moln och moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e6d3a-110">IoT Hub implements *at least once* delivery guarantees for both device-to-cloud and cloud-to-device messaging.</span></span>

<span data-ttu-id="e6d3a-111">En introduktion toohello funktionerna i IoT-hubb finns hello artiklar [Azure och Sakernas Internet] [ lnk-azure-iot] och [översikt över hello Azure IoT Hub service] [lnk-iot-hub-overview].</span><span class="sxs-lookup"><span data-stu-id="e6d3a-111">For an introduction toohello capabilities of IoT Hub, see hello articles [Azure and Internet of Things][lnk-azure-iot] and [Overview of hello Azure IoT Hub service][lnk-iot-hub-overview].</span></span>

## <a name="when-toouse-iot-hub-messaging"></a><span data-ttu-id="e6d3a-112">När meddelanden om toouse IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="e6d3a-112">When toouse IoT Hub messaging</span></span>

<span data-ttu-id="e6d3a-113">Använda meddelanden från enhet till moln för att skicka tid serien telemetri och aviseringar från din enhet och moln till enhet meddelanden för enkelriktade meddelanden tooyour enheten appen.</span><span class="sxs-lookup"><span data-stu-id="e6d3a-113">Use device-to-cloud messages for sending time series telemetry and alerts from your device app, and cloud-to-device messages for one-way notifications tooyour device app.</span></span>

* <span data-ttu-id="e6d3a-114">Se för[enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] om osäkra mellan med hjälp av meddelanden från enhet till moln, rapporterade egenskaper eller ladda upp filen.</span><span class="sxs-lookup"><span data-stu-id="e6d3a-114">Refer too[Device-to-cloud communication guidance][lnk-d2c-guidance] if in doubt between using device-to-cloud messages, reported properties, or file upload.</span></span>
* <span data-ttu-id="e6d3a-115">Se för[moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] om osäkra mellan att använda moln till enhet meddelanden, önskade egenskaper eller direkt metoder.</span><span class="sxs-lookup"><span data-stu-id="e6d3a-115">Refer too[Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using cloud-to-device messages, desired properties, or direct methods.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6d3a-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6d3a-116">Next steps</span></span>

* <span data-ttu-id="e6d3a-117">Lär dig mer om IoT-hubb [enhet till moln messaging][lnk-d2c].</span><span class="sxs-lookup"><span data-stu-id="e6d3a-117">Learn about IoT Hub [device-to-cloud messaging][lnk-d2c].</span></span>
* <span data-ttu-id="e6d3a-118">Lär dig mer om IoT-hubb [moln till enhet messaging][lnk-c2d].</span><span class="sxs-lookup"><span data-stu-id="e6d3a-118">Learn about IoT Hub [cloud-to-device messaging][lnk-c2d].</span></span>

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
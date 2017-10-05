---
title: "Förstå Azure IoT Hub-meddelanden | Microsoft Docs"
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
ms.openlocfilehash: f54398d7ac46bf178d2bb603669b399d25370736
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a><span data-ttu-id="48e79-104">Enhet till moln och moln till enhet meddelanden med IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="48e79-104">Device-to-cloud and cloud-to-device messaging with IoT Hub</span></span>

<span data-ttu-id="48e79-105">Använd IoT-hubb-meddelanden för att kommunicera med dina enheter genom att:</span><span class="sxs-lookup"><span data-stu-id="48e79-105">Use IoT Hub messaging to communicate with your devices by:</span></span>

* <span data-ttu-id="48e79-106">Skicka [enhet till moln] [ lnk-d2c] meddelanden från dina enheter till din lösning serverdel.</span><span class="sxs-lookup"><span data-stu-id="48e79-106">Sending [device-to-cloud][lnk-d2c] messages from your devices to your solution back end.</span></span>
* <span data-ttu-id="48e79-107">Skicka [moln till enhet] [ lnk-c2d] meddelanden från lösningen serverdel till dina enheter.</span><span class="sxs-lookup"><span data-stu-id="48e79-107">Sending [cloud-to-device][lnk-c2d] messages from the solution back end to your devices.</span></span>

<span data-ttu-id="48e79-108">Grundläggande egenskaper för IoT-hubb meddelandefunktioner är tillförlitlighet och hållbarhet av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="48e79-108">Core properties of IoT Hub messaging functionality are the reliability and durability of messages.</span></span> <span data-ttu-id="48e79-109">De här egenskaperna aktiverar återhämtning för återkommande anslutning på enhetens sida och att läsa in toppar i händelsebearbetning på sidan moln.</span><span class="sxs-lookup"><span data-stu-id="48e79-109">These properties enable resilience to intermittent connectivity on the device side, and to load spikes in event processing on the cloud side.</span></span> <span data-ttu-id="48e79-110">IoT-hubb implementerar *minst en gång* leverans garanterar för både enhet till moln och moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="48e79-110">IoT Hub implements *at least once* delivery guarantees for both device-to-cloud and cloud-to-device messaging.</span></span>

<span data-ttu-id="48e79-111">En introduktion till funktionerna för IoT-hubb finns i artiklar [Azure och Sakernas Internet] [ lnk-azure-iot] och [översikt över tjänsten Azure IoT Hub] [ lnk-iot-hub-overview].</span><span class="sxs-lookup"><span data-stu-id="48e79-111">For an introduction to the capabilities of IoT Hub, see the articles [Azure and Internet of Things][lnk-azure-iot] and [Overview of the Azure IoT Hub service][lnk-iot-hub-overview].</span></span>

## <a name="when-to-use-iot-hub-messaging"></a><span data-ttu-id="48e79-112">När du ska använda IoT-hubb-meddelanden</span><span class="sxs-lookup"><span data-stu-id="48e79-112">When to use IoT Hub messaging</span></span>

<span data-ttu-id="48e79-113">Använda meddelanden från enhet till moln för att skicka tid serien telemetri och aviseringar från din enhet och moln till enhet meddelanden för envägs-meddelanden till din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="48e79-113">Use device-to-cloud messages for sending time series telemetry and alerts from your device app, and cloud-to-device messages for one-way notifications to your device app.</span></span>

* <span data-ttu-id="48e79-114">Referera till [enhet till moln kommunikation vägledning] [ lnk-d2c-guidance] om osäkra mellan med hjälp av meddelanden från enhet till moln, rapporterade egenskaper eller ladda upp filen.</span><span class="sxs-lookup"><span data-stu-id="48e79-114">Refer to [Device-to-cloud communication guidance][lnk-d2c-guidance] if in doubt between using device-to-cloud messages, reported properties, or file upload.</span></span>
* <span data-ttu-id="48e79-115">Referera till [moln till enhet kommunikation vägledning] [ lnk-c2d-guidance] om osäkra mellan att använda moln till enhet meddelanden, önskade egenskaper eller direkt metoder.</span><span class="sxs-lookup"><span data-stu-id="48e79-115">Refer to [Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using cloud-to-device messages, desired properties, or direct methods.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48e79-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48e79-116">Next steps</span></span>

* <span data-ttu-id="48e79-117">Lär dig mer om IoT-hubb [enhet till moln messaging][lnk-d2c].</span><span class="sxs-lookup"><span data-stu-id="48e79-117">Learn about IoT Hub [device-to-cloud messaging][lnk-d2c].</span></span>
* <span data-ttu-id="48e79-118">Lär dig mer om IoT-hubb [moln till enhet messaging][lnk-c2d].</span><span class="sxs-lookup"><span data-stu-id="48e79-118">Learn about IoT Hub [cloud-to-device messaging][lnk-c2d].</span></span>

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
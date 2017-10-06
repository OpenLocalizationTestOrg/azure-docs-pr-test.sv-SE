---
title: "Azure IoT-hubb - komma igång ansluter IoT-enheter toohello molntjänster | Microsoft Docs"
description: "Lär dig hur tooconnect din IoT-kort och starter kits tooAzure IoT-hubb. Enheterna kan skicka telemetri tooIoT NAV- och IoT-hubb kan övervaka och hantera dina enheter."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: "självstudiekurs för Azure iot-hubb"
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: 6dc956308009091532019ff84aec881f042f0104
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-tutorials"></a><span data-ttu-id="2b838-105">Azure IoT-hubb komma igång Självstudier</span><span class="sxs-lookup"><span data-stu-id="2b838-105">Azure IoT Hub get started tutorials</span></span>

<span data-ttu-id="2b838-106">Du kan använda Azure IoT Hub och hello Azure IoT-enhet SDK toobuild Sakernas Internet (IoT) lösningar:</span><span class="sxs-lookup"><span data-stu-id="2b838-106">You can use Azure IoT Hub and hello Azure IoT device SDKs toobuild Internet of Things (IoT) solutions:</span></span>

* <span data-ttu-id="2b838-107">Azure IoT Hub är en helt hanterad tjänst i hello moln som ansluter, övervakar och hanterar IoT-enheter på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="2b838-107">Azure IoT Hub is a fully managed service in hello cloud that securely connects, monitors, and manages your IoT devices.</span></span> <span data-ttu-id="2b838-108">Använd hello Azure IoT-enhet SDK tooimplement IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="2b838-108">Use hello Azure IoT Device SDKs tooimplement your IoT devices.</span></span>
* <span data-ttu-id="2b838-109">Använd en IoT-gateway i mer komplexa IoT-scenarier.</span><span class="sxs-lookup"><span data-stu-id="2b838-109">Use an IoT gateway in more complex IoT scenarios.</span></span> <span data-ttu-id="2b838-110">Till exempel där du behöver tooconsider faktorer, till exempel äldre enheter, kostnader för bandbredd, principer för säkerhet och sekretess eller edge databearbetning.</span><span class="sxs-lookup"><span data-stu-id="2b838-110">For example, where you need tooconsider factors such as legacy devices, bandwidth costs, security and privacy policies, or edge data processing.</span></span> <span data-ttu-id="2b838-111">I så fall kan använda du Azure IoT kant tooimplement en gateway som ansluter enheter tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2b838-111">In these scenarios, you use Azure IoT Edge tooimplement a gateway that connects devices tooyour IoT hub.</span></span>

## <a name="what-hello-tutorials-cover"></a><span data-ttu-id="2b838-112">Hello handledningar täcker</span><span class="sxs-lookup"><span data-stu-id="2b838-112">What hello tutorials cover</span></span>

<span data-ttu-id="2b838-113">Dessa självstudiekurser införa tooAzure IoT-hubb och hello enhet SDK: er.</span><span class="sxs-lookup"><span data-stu-id="2b838-113">These tutorials introduce you tooAzure IoT Hub and hello device SDKs.</span></span> <span data-ttu-id="2b838-114">hello självstudier beskriver vanliga IoT scenarier toodemonstrate hello funktionerna i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2b838-114">hello tutorials cover common IoT scenarios toodemonstrate hello capabilities of IoT Hub.</span></span> <span data-ttu-id="2b838-115">hello självstudier också illustrerar hur toocombine IoT-hubb med andra Azure-tjänster och verktyg toobuild mer kraftfulla IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="2b838-115">hello tutorials also illustrate how toocombine IoT Hub with other Azure services and tools toobuild more powerful IoT solutions.</span></span> <span data-ttu-id="2b838-116">I hello självstudier, kan du välja toouse simulerad eller verkliga IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="2b838-116">In hello tutorials, you can choose toouse either simulated or real IoT devices.</span></span> <span data-ttu-id="2b838-117">Dessutom kan du lära dig hur toouse en gateway tooenable enheter tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2b838-117">In addition, you can learn how toouse a gateway tooenable devices tooconnect tooyour IoT hub.</span></span>

## <a name="set-up-your-device"></a><span data-ttu-id="2b838-118">Konfigurera enheten</span><span class="sxs-lookup"><span data-stu-id="2b838-118">Set up your device</span></span>

<span data-ttu-id="2b838-119">Ansluta en IoT-enhet eller gateway tooAzure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2b838-119">Connect an IoT device or gateway tooAzure IoT Hub.</span></span> <span data-ttu-id="2b838-120">Du kan välja en fysisk eller simulerade enheten tooget igång:</span><span class="sxs-lookup"><span data-stu-id="2b838-120">You can choose a physical or simulated device tooget started:</span></span>

| <span data-ttu-id="2b838-121">IoT-enhet</span><span class="sxs-lookup"><span data-stu-id="2b838-121">IoT device</span></span>                       | <span data-ttu-id="2b838-122">Programmeringsspråk</span><span class="sxs-lookup"><span data-stu-id="2b838-122">Programming language</span></span> |
|----------------------------------|----------------------|
| <span data-ttu-id="2b838-123">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="2b838-123">Raspberry Pi</span></span>                     | <span data-ttu-id="2b838-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="2b838-124">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="2b838-125">IoT DevKit</span><span class="sxs-lookup"><span data-stu-id="2b838-125">IoT DevKit</span></span>                       | <span data-ttu-id="2b838-126">[Arduino i VSCode][DevKit]</span><span class="sxs-lookup"><span data-stu-id="2b838-126">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="2b838-127">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="2b838-127">Intel Edison</span></span>                     | <span data-ttu-id="2b838-128">[Node.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="2b838-128">[Node.js][Ed_Nd], [C][Ed_C]</span></span>    |
| <span data-ttu-id="2b838-129">Adafruit ludd HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="2b838-129">Adafruit Feather HUZZAH ESP8266</span></span>  | <span data-ttu-id="2b838-130">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="2b838-130">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="2b838-131">Sparkfun ESP8266 sak Dev</span><span class="sxs-lookup"><span data-stu-id="2b838-131">Sparkfun ESP8266 Thing Dev</span></span>       | <span data-ttu-id="2b838-132">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="2b838-132">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="2b838-133">Adafruit ludd M0</span><span class="sxs-lookup"><span data-stu-id="2b838-133">Adafruit Feather M0</span></span>              | <span data-ttu-id="2b838-134">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="2b838-134">[Arduino][M0_Ard]</span></span>              |
| <span data-ttu-id="2b838-135">Simulerade enhet på datorn</span><span class="sxs-lookup"><span data-stu-id="2b838-135">Simulated device on PC</span></span>           | <span data-ttu-id="2b838-136">[.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span><span class="sxs-lookup"><span data-stu-id="2b838-136">[.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth]</span></span> |
| <span data-ttu-id="2b838-137">Online enheten simulatorn</span><span class="sxs-lookup"><span data-stu-id="2b838-137">Online device simulator</span></span>         | <span data-ttu-id="2b838-138">[Raspberry Pi (Node.js)][Ol_Sim]</span><span class="sxs-lookup"><span data-stu-id="2b838-138">[Raspberry Pi (Node.js)][Ol_Sim]</span></span> |

<span data-ttu-id="2b838-139">Dessutom kan du använda en IoT-Edge gateway tooenable enheter tooconnect tooyour IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="2b838-139">In addition, you can use an IoT Edge gateway tooenable devices tooconnect tooyour IoT hub:</span></span>

| <span data-ttu-id="2b838-140">Gateway-enhet</span><span class="sxs-lookup"><span data-stu-id="2b838-140">Gateway device</span></span>               | <span data-ttu-id="2b838-141">Programmeringsspråk</span><span class="sxs-lookup"><span data-stu-id="2b838-141">Programming language</span></span> | <span data-ttu-id="2b838-142">Plattform</span><span class="sxs-lookup"><span data-stu-id="2b838-142">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="2b838-143">Intel NUC (model DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="2b838-143">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="2b838-144">C</span><span class="sxs-lookup"><span data-stu-id="2b838-144">C</span></span>                    | <span data-ttu-id="2b838-145">[Vind floden Linux][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="2b838-145">[Wind River Linux][NUC_Lnx]</span></span> |
| <span data-ttu-id="2b838-146">Simulerade gateway</span><span class="sxs-lookup"><span data-stu-id="2b838-146">Simulated gateway</span></span>            | <span data-ttu-id="2b838-147">C</span><span class="sxs-lookup"><span data-stu-id="2b838-147">C</span></span>                    | <span data-ttu-id="2b838-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span><span class="sxs-lookup"><span data-stu-id="2b838-148">[Linux][Sim_Lnx], [Windows][Sim_Win]</span></span> |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md

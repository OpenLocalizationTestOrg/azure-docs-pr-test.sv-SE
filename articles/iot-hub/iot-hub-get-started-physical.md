---
title: "Komma igång med att ansluta fysiska enheter tooAzure IoT-hubb | Microsoft Docs"
description: "Lär dig hur tooconnect fysiska enheter och anslagstavlor tooAzure IoT-hubb. Enheterna kan skicka telemetri tooIoT NAV- och IoT-hubb kan övervaka och hantera dina enheter."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: "självstudiekurs för Azure iot-hubb"
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: 47ce289c438b2f495d499d724c38ddc4b3307425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a><span data-ttu-id="98651-105">Azure IoT-hubb Kom igång med fysiska enheter självstudier</span><span class="sxs-lookup"><span data-stu-id="98651-105">Azure IoT Hub get started with physical devices tutorials</span></span>

<span data-ttu-id="98651-106">Dessa självstudiekurser införa tooAzure IoT-hubb och hello enhet SDK: er.</span><span class="sxs-lookup"><span data-stu-id="98651-106">These tutorials introduce you tooAzure IoT Hub and hello device SDKs.</span></span> <span data-ttu-id="98651-107">hello självstudier beskriver vanliga IoT scenarier toodemonstrate hello funktionerna i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="98651-107">hello tutorials cover common IoT scenarios toodemonstrate hello capabilities of IoT Hub.</span></span> <span data-ttu-id="98651-108">hello självstudier också illustrerar hur toocombine IoT-hubb med andra Azure-tjänster och verktyg toobuild mer kraftfulla IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="98651-108">hello tutorials also illustrate how toocombine IoT Hub with other Azure services and tools toobuild more powerful IoT solutions.</span></span> <span data-ttu-id="98651-109">hello självstudier som anges i hello följande tabell visar du hur toocreate fysiska IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="98651-109">hello tutorials listed in hello following table show you how toocreate physical IoT devices.</span></span>

| <span data-ttu-id="98651-110">IoT-enhet</span><span class="sxs-lookup"><span data-stu-id="98651-110">IoT device</span></span>                       | <span data-ttu-id="98651-111">Programmeringsspråk</span><span class="sxs-lookup"><span data-stu-id="98651-111">Programming language</span></span> |
|---------------------------------|----------------------|
| <span data-ttu-id="98651-112">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="98651-112">Raspberry Pi</span></span>                    | <span data-ttu-id="98651-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="98651-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="98651-114">IoT DevKit</span><span class="sxs-lookup"><span data-stu-id="98651-114">IoT DevKit</span></span>                      | <span data-ttu-id="98651-115">[Arduino i VSCode][DevKit]</span><span class="sxs-lookup"><span data-stu-id="98651-115">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="98651-116">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="98651-116">Intel Edison</span></span>                    | <span data-ttu-id="98651-117">[Node.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="98651-117">[Node.js][Ed_Nd], [C][Ed_C]</span></span>           |
| <span data-ttu-id="98651-118">Adafruit ludd HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="98651-118">Adafruit Feather HUZZAH ESP8266</span></span> | <span data-ttu-id="98651-119">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="98651-119">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="98651-120">Sparkfun ESP8266 sak Dev</span><span class="sxs-lookup"><span data-stu-id="98651-120">Sparkfun ESP8266 Thing Dev</span></span>      | <span data-ttu-id="98651-121">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="98651-121">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="98651-122">Adafruit ludd M0</span><span class="sxs-lookup"><span data-stu-id="98651-122">Adafruit Feather M0</span></span>             | <span data-ttu-id="98651-123">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="98651-123">[Arduino][M0_Ard]</span></span>              |

<span data-ttu-id="98651-124">Dessutom kan du använda en IoT-Edge gateway tooenable enheter tooconnect tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="98651-124">In addition, you can use an IoT Edge gateway tooenable devices tooconnect tooyour IoT hub.</span></span>

| <span data-ttu-id="98651-125">Gateway-enhet</span><span class="sxs-lookup"><span data-stu-id="98651-125">Gateway device</span></span>               | <span data-ttu-id="98651-126">Programmeringsspråk</span><span class="sxs-lookup"><span data-stu-id="98651-126">Programming language</span></span> | <span data-ttu-id="98651-127">Plattform</span><span class="sxs-lookup"><span data-stu-id="98651-127">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="98651-128">Intel NUC (model DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="98651-128">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="98651-129">C</span><span class="sxs-lookup"><span data-stu-id="98651-129">C</span></span>                    | <span data-ttu-id="98651-130">[Vind floden Linux][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="98651-130">[Wind River Linux][NUC_Lnx]</span></span> |

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
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md

---
title: "Komma igång med att ansluta fysiska enheter till Azure IoT Hub | Microsoft Docs"
description: "Lär dig mer om att ansluta till Azure IoT Hub fysiska enheter och -kort. Enheterna kan skicka telemetri IoT-hubb och IoT-hubb kan övervaka och hantera dina enheter."
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
ms.openlocfilehash: f4128b6b049aa876e170c56dcf2e40720644dc3d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a><span data-ttu-id="dc659-105">Azure IoT-hubb Kom igång med fysiska enheter självstudier</span><span class="sxs-lookup"><span data-stu-id="dc659-105">Azure IoT Hub get started with physical devices tutorials</span></span>

<span data-ttu-id="dc659-106">Dessa självstudiekurser introduktion till Azure IoT Hub och SDK-enheten.</span><span class="sxs-lookup"><span data-stu-id="dc659-106">These tutorials introduce you to Azure IoT Hub and the device SDKs.</span></span> <span data-ttu-id="dc659-107">Kursen beskriver vanliga IoT-scenarier för att demonstrera funktionerna i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="dc659-107">The tutorials cover common IoT scenarios to demonstrate the capabilities of IoT Hub.</span></span> <span data-ttu-id="dc659-108">Självstudierna visar även hur du kombinerar IoT-hubb med andra Azure-tjänster och verktyg för att skapa mer kraftfulla IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="dc659-108">The tutorials also illustrate how to combine IoT Hub with other Azure services and tools to build more powerful IoT solutions.</span></span> <span data-ttu-id="dc659-109">Självstudiekurser som anges i följande tabell visar hur du skapar fysiska IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="dc659-109">The tutorials listed in the following table show you how to create physical IoT devices.</span></span>

| <span data-ttu-id="dc659-110">IoT-enhet</span><span class="sxs-lookup"><span data-stu-id="dc659-110">IoT device</span></span>                       | <span data-ttu-id="dc659-111">Programmeringsspråk</span><span class="sxs-lookup"><span data-stu-id="dc659-111">Programming language</span></span> |
|---------------------------------|----------------------|
| <span data-ttu-id="dc659-112">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="dc659-112">Raspberry Pi</span></span>                    | <span data-ttu-id="dc659-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span><span class="sxs-lookup"><span data-stu-id="dc659-113">[Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]</span></span>  |
| <span data-ttu-id="dc659-114">IoT DevKit</span><span class="sxs-lookup"><span data-stu-id="dc659-114">IoT DevKit</span></span>                      | <span data-ttu-id="dc659-115">[Arduino i VSCode][DevKit]</span><span class="sxs-lookup"><span data-stu-id="dc659-115">[Arduino in VSCode][DevKit]</span></span>     |
| <span data-ttu-id="dc659-116">Intel Edison</span><span class="sxs-lookup"><span data-stu-id="dc659-116">Intel Edison</span></span>                    | <span data-ttu-id="dc659-117">[Node.js][Ed_Nd], [C][Ed_C]</span><span class="sxs-lookup"><span data-stu-id="dc659-117">[Node.js][Ed_Nd], [C][Ed_C]</span></span>           |
| <span data-ttu-id="dc659-118">Adafruit ludd HUZZAH ESP8266</span><span class="sxs-lookup"><span data-stu-id="dc659-118">Adafruit Feather HUZZAH ESP8266</span></span> | <span data-ttu-id="dc659-119">[Arduino][Hu_Ard]</span><span class="sxs-lookup"><span data-stu-id="dc659-119">[Arduino][Hu_Ard]</span></span>              |
| <span data-ttu-id="dc659-120">Sparkfun ESP8266 sak Dev</span><span class="sxs-lookup"><span data-stu-id="dc659-120">Sparkfun ESP8266 Thing Dev</span></span>      | <span data-ttu-id="dc659-121">[Arduino][Th_Ard]</span><span class="sxs-lookup"><span data-stu-id="dc659-121">[Arduino][Th_Ard]</span></span>              |
| <span data-ttu-id="dc659-122">Adafruit ludd M0</span><span class="sxs-lookup"><span data-stu-id="dc659-122">Adafruit Feather M0</span></span>             | <span data-ttu-id="dc659-123">[Arduino][M0_Ard]</span><span class="sxs-lookup"><span data-stu-id="dc659-123">[Arduino][M0_Ard]</span></span>              |

<span data-ttu-id="dc659-124">Dessutom kan du använda en IoT-gräns-gatewayen att enheterna ansluter till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="dc659-124">In addition, you can use an IoT Edge gateway to enable devices to connect to your IoT hub.</span></span>

| <span data-ttu-id="dc659-125">Gateway-enhet</span><span class="sxs-lookup"><span data-stu-id="dc659-125">Gateway device</span></span>               | <span data-ttu-id="dc659-126">Programmeringsspråk</span><span class="sxs-lookup"><span data-stu-id="dc659-126">Programming language</span></span> | <span data-ttu-id="dc659-127">Plattform</span><span class="sxs-lookup"><span data-stu-id="dc659-127">Platform</span></span>         |
|------------------------------|----------------------|------------------|
| <span data-ttu-id="dc659-128">Intel NUC (model DE3815TYKE)</span><span class="sxs-lookup"><span data-stu-id="dc659-128">Intel NUC (model DE3815TYKE)</span></span> | <span data-ttu-id="dc659-129">C</span><span class="sxs-lookup"><span data-stu-id="dc659-129">C</span></span>                    | <span data-ttu-id="dc659-130">[Vind floden Linux][NUC_Lnx]</span><span class="sxs-lookup"><span data-stu-id="dc659-130">[Wind River Linux][NUC_Lnx]</span></span> |

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

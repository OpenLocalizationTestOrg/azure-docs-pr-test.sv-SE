---
title: "Komma igång med att ansluta simulerade enheter till Azure IoT Hub | Microsoft Docs"
description: "Lär dig hur du skapar simulerade IoT-enheter och Anslut dem till Azure IoT Hub. Enheterna kan skicka telemetri IoT-hubb och Iot-hubb kan övervaka och hantera dina enheter."
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
ms.date: 06/02/2017
ms.author: dobett
ms.openlocfilehash: 436b3057509a831837159e814490959a2d7455a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-hub-get-started-with-simulated-devices-tutorials"></a><span data-ttu-id="94fcf-105">Azure IoT-hubb Kom igång med simulerade enheterna självstudier</span><span class="sxs-lookup"><span data-stu-id="94fcf-105">Azure IoT Hub get started with simulated devices tutorials</span></span>

<span data-ttu-id="94fcf-106">Dessa självstudiekurser introduktion till Azure IoT Hub och SDK-enheten.</span><span class="sxs-lookup"><span data-stu-id="94fcf-106">These tutorials introduce you to Azure IoT Hub and the device SDKs.</span></span> <span data-ttu-id="94fcf-107">Kursen beskriver vanliga IoT-scenarier för att demonstrera funktionerna i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="94fcf-107">The tutorials cover common IoT scenarios to demonstrate the capabilities of IoT Hub.</span></span> <span data-ttu-id="94fcf-108">Självstudierna visar även hur du kombinerar IoT-hubb med andra Azure-tjänster och verktyg för att skapa mer kraftfulla IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="94fcf-108">The tutorials also illustrate how to combine IoT Hub with other Azure services and tools to build more powerful IoT solutions.</span></span> <span data-ttu-id="94fcf-109">Självstudiekurser som anges i följande tabell visar hur du skapar simulerade IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="94fcf-109">The tutorials listed in the following table show you how to create simulated IoT devices.</span></span>

| <span data-ttu-id="94fcf-110">Programmeringsspråk</span><span class="sxs-lookup"><span data-stu-id="94fcf-110">Programming language</span></span> |
|----------------------|
| <span data-ttu-id="94fcf-111">[.NET][Sim_NET]</span><span class="sxs-lookup"><span data-stu-id="94fcf-111">[.NET][Sim_NET]</span></span>      |
| <span data-ttu-id="94fcf-112">[Java][Sim_Jav]</span><span class="sxs-lookup"><span data-stu-id="94fcf-112">[Java][Sim_Jav]</span></span>      |
| <span data-ttu-id="94fcf-113">[Node.js][Sim_Nd]</span><span class="sxs-lookup"><span data-stu-id="94fcf-113">[Node.js][Sim_Nd]</span></span>    |
| <span data-ttu-id="94fcf-114">[Python][Sim_Pyth]</span><span class="sxs-lookup"><span data-stu-id="94fcf-114">[Python][Sim_Pyth]</span></span>   |

<span data-ttu-id="94fcf-115">Dessutom kan du använda en IoT-gräns-gatewayen för att aktivera simulerade enheter ansluter till din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="94fcf-115">In addition, you can use an IoT Edge gateway to enable simulated devices to connect to your IoT hub.</span></span>

| <span data-ttu-id="94fcf-116">Programmeringsspråk</span><span class="sxs-lookup"><span data-stu-id="94fcf-116">Programming language</span></span> | <span data-ttu-id="94fcf-117">Plattform</span><span class="sxs-lookup"><span data-stu-id="94fcf-117">Platform</span></span>           |
|----------------------|------------------- |
| <span data-ttu-id="94fcf-118">C</span><span class="sxs-lookup"><span data-stu-id="94fcf-118">C</span></span>                    | <span data-ttu-id="94fcf-119">[Linux][Sim_Lnx]</span><span class="sxs-lookup"><span data-stu-id="94fcf-119">[Linux][Sim_Lnx]</span></span>   |
| <span data-ttu-id="94fcf-120">C</span><span class="sxs-lookup"><span data-stu-id="94fcf-120">C</span></span>                    | <span data-ttu-id="94fcf-121">[Windows][Sim_Win]</span><span class="sxs-lookup"><span data-stu-id="94fcf-121">[Windows][Sim_Win]</span></span> |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md

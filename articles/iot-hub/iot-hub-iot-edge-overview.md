---
title: "Översikt över Azure IoT-Edge | Microsoft Docs"
description: "Beskriver arkitekturen nyckelbegreppen i Azure IoT kant som gateways, moduler och mäklare."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dobett
ms.openlocfilehash: ecdd56c91a8fc2011b3d7abe93b9d27c1e1e0bef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a><span data-ttu-id="431d4-103">Azure IoT kant arkitektur begrepp</span><span class="sxs-lookup"><span data-stu-id="431d4-103">Azure IoT Edge architectural concepts</span></span>

<span data-ttu-id="431d4-104">Innan du granska alla exempelkod eller skapa egna fältet gateway med IoT kant, bör du förstå viktiga begrepp som ligger till grund för arkitekturen för IoT kant.</span><span class="sxs-lookup"><span data-stu-id="431d4-104">Before you examine any sample code or create your own field gateway using IoT Edge, you should understand the key concepts that underpin the architecture of IoT Edge.</span></span>

## <a name="iot-edge-modules"></a><span data-ttu-id="431d4-105">IoT-Edge-moduler</span><span class="sxs-lookup"><span data-stu-id="431d4-105">IoT Edge modules</span></span>

<span data-ttu-id="431d4-106">Du skapar en gateway med Azure IoT kant genom att skapa och montera *IoT kant moduler*.</span><span class="sxs-lookup"><span data-stu-id="431d4-106">You build a gateway with Azure IoT Edge by creating and assembling *IoT Edge modules*.</span></span> <span data-ttu-id="431d4-107">Moduler använder *meddelanden* för att utbyta data med varandra.</span><span class="sxs-lookup"><span data-stu-id="431d4-107">Modules use *messages* to exchange data with each other.</span></span> <span data-ttu-id="431d4-108">En modul får ett meddelande, utför en åtgärd på det, omvandlar det eventuellt till ett nytt meddelande och publicerar det sedan för andra moduler att bearbeta.</span><span class="sxs-lookup"><span data-stu-id="431d4-108">A module receives a message, performs some action on it, optionally transforms it into a new message, and then publishes it for other modules to process.</span></span> <span data-ttu-id="431d4-109">Vissa moduler kan bara generera nya meddelanden och bearbetar aldrig inkommande meddelanden.</span><span class="sxs-lookup"><span data-stu-id="431d4-109">Some modules might only produce new messages and never process incoming messages.</span></span> <span data-ttu-id="431d4-110">En kedja av moduler skapar en databearbetningspipeline med varje modul som utför en omvandling av data vid en punkt i denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="431d4-110">A chain of modules creates a data processing pipeline with each module performing a transformation on the data at one point in that pipeline.</span></span>

![En kedja med moduler i en gateway som skapats med Azure IoT Egde][1]

<span data-ttu-id="431d4-112">IoT-Edge innehåller följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="431d4-112">IoT Edge contains the following components:</span></span>

* <span data-ttu-id="431d4-113">Färdig moduler som utför vanliga gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="431d4-113">Pre-written modules that perform common gateway functions.</span></span>
* <span data-ttu-id="431d4-114">Gränssnitt som utvecklare kan använda för att skriva anpassade moduler.</span><span class="sxs-lookup"><span data-stu-id="431d4-114">The interfaces a developer can use to write custom modules.</span></span>
* <span data-ttu-id="431d4-115">Den infrastruktur som krävs för att distribuera och köra en uppsättning moduler.</span><span class="sxs-lookup"><span data-stu-id="431d4-115">The infrastructure necessary to deploy and run a set of modules.</span></span>

<span data-ttu-id="431d4-116">SDK innehåller ett Abstraktionslager som låter dig skapa gatewayer som ska köras på olika operativsystem och plattformar.</span><span class="sxs-lookup"><span data-stu-id="431d4-116">The SDK provides an abstraction layer that enables you to build gateways to run on various operating systems and platforms.</span></span>

![Azure IoT Edge-abstraktionslager][2]

## <a name="messages"></a><span data-ttu-id="431d4-118">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="431d4-118">Messages</span></span>

<span data-ttu-id="431d4-119">Tänk på att även om moduler som skickar meddelanden till varandra är ett bekvämt sätt att få en överblick över hur en gateway fungerar, så återspeglar det inte korrekt vad som händer.</span><span class="sxs-lookup"><span data-stu-id="431d4-119">Although thinking about modules passing messages to each other is a convenient way to conceptualize how a gateway functions, it does not accurately reflect what happens.</span></span> <span data-ttu-id="431d4-120">IoT kant moduler använda förhandlare för att kommunicera med varandra.</span><span class="sxs-lookup"><span data-stu-id="431d4-120">IoT Edge modules use a broker to communicate with each other.</span></span> <span data-ttu-id="431d4-121">Moduler publicera meddelanden i Service broker (med meddelandemönster, till exempel bus eller publicera/prenumerera) och därefter låta Service broker vidarebefordra meddelandet till de moduler som är anslutna till den.</span><span class="sxs-lookup"><span data-stu-id="431d4-121">Modules publish messages to the broker (using messaging patterns such as bus, or publish/subscribe) and then let the broker route the message to the modules connected to it.</span></span>

<span data-ttu-id="431d4-122">En modul använder funktionen **Broker_Publish** för att publicera ett meddelande till meddelandekön.</span><span class="sxs-lookup"><span data-stu-id="431d4-122">A module uses the **Broker_Publish** function to publish a message to the broker.</span></span> <span data-ttu-id="431d4-123">Den asynkrona meddelandekön levererar meddelanden till en modul genom att anropa en återanropsfunktion.</span><span class="sxs-lookup"><span data-stu-id="431d4-123">The broker delivers messages to a module by invoking a callback function.</span></span> <span data-ttu-id="431d4-124">Ett meddelande består av en uppsättning nyckel-/värdeegenskaper och innehåll som skickas som ett block med minne.</span><span class="sxs-lookup"><span data-stu-id="431d4-124">A message consists of a set of key/value properties and content passed as a block of memory.</span></span>

![Agentens roll i Azure IoT Edge][3]

## <a name="message-routing-and-filtering"></a><span data-ttu-id="431d4-126">Meddelandedirigering och filtrering</span><span class="sxs-lookup"><span data-stu-id="431d4-126">Message routing and filtering</span></span>

<span data-ttu-id="431d4-127">Det finns två sätt att dirigera meddelanden till rätt IoT kant-modulerna:</span><span class="sxs-lookup"><span data-stu-id="431d4-127">There are two ways to direct messages to the correct IoT Edge modules:</span></span>

* <span data-ttu-id="431d4-128">Du kan skicka en uppsättning länkar kan skickas till Service broker så att Service broker vet källa och mottagare för varje modul.</span><span class="sxs-lookup"><span data-stu-id="431d4-128">You can pass a set of links can be passed to the broker so the broker knows the source and sink for each module.</span></span>
* <span data-ttu-id="431d4-129">En modul kan filtrera i egenskaperna för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="431d4-129">A module can filter on the properties of the message.</span></span>

<span data-ttu-id="431d4-130">En modul ska bara agera på ett meddelande om meddelandet är avsett för modulen.</span><span class="sxs-lookup"><span data-stu-id="431d4-130">A module should only act upon a message if the message is intended for it.</span></span> <span data-ttu-id="431d4-131">Länkar och meddelandet filtrering effektivt skapar du en message-pipeline.</span><span class="sxs-lookup"><span data-stu-id="431d4-131">Links and message filtering effectively create a message pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="431d4-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="431d4-132">Next steps</span></span>

<span data-ttu-id="431d4-133">Dessa begrepp som används i ett exempel kan du köra finns [utforska Azure IoT kant arkitektur][lnk-hello-world].</span><span class="sxs-lookup"><span data-stu-id="431d4-133">To see these concepts applied in a sample you can run, see [Explore Azure IoT Edge architecture][lnk-hello-world].</span></span>

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md
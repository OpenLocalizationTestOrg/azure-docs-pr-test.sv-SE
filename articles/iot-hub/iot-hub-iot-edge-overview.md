---
title: aaaOverview Azure IoT kant | Microsoft Docs
description: "Beskriver hello arkitektur nyckelbegrepp i Azure IoT kant som gateways, moduler och mäklare."
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
ms.openlocfilehash: 32debc0d4f40cfd7f2cce7cf8c76b12ec18ee2dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a><span data-ttu-id="4217c-103">Azure IoT kant arkitektur begrepp</span><span class="sxs-lookup"><span data-stu-id="4217c-103">Azure IoT Edge architectural concepts</span></span>

<span data-ttu-id="4217c-104">Innan du granska alla exempelkod eller skapa egna fältet gateway med IoT kant, bör du förstå hello viktiga begrepp som ligger till grund för hello arkitekturen för IoT kant.</span><span class="sxs-lookup"><span data-stu-id="4217c-104">Before you examine any sample code or create your own field gateway using IoT Edge, you should understand hello key concepts that underpin hello architecture of IoT Edge.</span></span>

## <a name="iot-edge-modules"></a><span data-ttu-id="4217c-105">IoT-Edge-moduler</span><span class="sxs-lookup"><span data-stu-id="4217c-105">IoT Edge modules</span></span>

<span data-ttu-id="4217c-106">Du skapar en gateway med Azure IoT kant genom att skapa och montera *IoT kant moduler*.</span><span class="sxs-lookup"><span data-stu-id="4217c-106">You build a gateway with Azure IoT Edge by creating and assembling *IoT Edge modules*.</span></span> <span data-ttu-id="4217c-107">Använda moduler *meddelanden* tooexchange data med varandra.</span><span class="sxs-lookup"><span data-stu-id="4217c-107">Modules use *messages* tooexchange data with each other.</span></span> <span data-ttu-id="4217c-108">En modul tar emot ett meddelande, utför en åtgärd, omvandlar eventuellt det till ett nytt meddelande och publicerar den för andra moduler tooprocess.</span><span class="sxs-lookup"><span data-stu-id="4217c-108">A module receives a message, performs some action on it, optionally transforms it into a new message, and then publishes it for other modules tooprocess.</span></span> <span data-ttu-id="4217c-109">Vissa moduler kan bara generera nya meddelanden och bearbetar aldrig inkommande meddelanden.</span><span class="sxs-lookup"><span data-stu-id="4217c-109">Some modules might only produce new messages and never process incoming messages.</span></span> <span data-ttu-id="4217c-110">En kedja av moduler skapar en pipeline för databearbetning med varje modul som utför en omvandling på hello data en gång i denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="4217c-110">A chain of modules creates a data processing pipeline with each module performing a transformation on hello data at one point in that pipeline.</span></span>

![En kedja med moduler i en gateway som skapats med Azure IoT Egde][1]

<span data-ttu-id="4217c-112">IoT-Edge innehåller hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="4217c-112">IoT Edge contains hello following components:</span></span>

* <span data-ttu-id="4217c-113">Färdig moduler som utför vanliga gateway-funktioner.</span><span class="sxs-lookup"><span data-stu-id="4217c-113">Pre-written modules that perform common gateway functions.</span></span>
* <span data-ttu-id="4217c-114">hello gränssnitt en utvecklare kan använda toowrite anpassade moduler.</span><span class="sxs-lookup"><span data-stu-id="4217c-114">hello interfaces a developer can use toowrite custom modules.</span></span>
* <span data-ttu-id="4217c-115">Hej infrastruktur nödvändiga toodeploy och köra en uppsättning moduler.</span><span class="sxs-lookup"><span data-stu-id="4217c-115">hello infrastructure necessary toodeploy and run a set of modules.</span></span>

<span data-ttu-id="4217c-116">hello SDK tillhandahåller ett Abstraktionslager som gör att du toobuild gateways toorun på olika operativsystem och plattformar.</span><span class="sxs-lookup"><span data-stu-id="4217c-116">hello SDK provides an abstraction layer that enables you toobuild gateways toorun on various operating systems and platforms.</span></span>

![Azure IoT Edge-abstraktionslager][2]

## <a name="messages"></a><span data-ttu-id="4217c-118">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="4217c-118">Messages</span></span>

<span data-ttu-id="4217c-119">Även om du tänker moduler som skickar meddelanden tooeach andra är ett bekvämt sätt tooconceptualize hur en gateway-funktioner, den inte korrekt speglar vad som händer.</span><span class="sxs-lookup"><span data-stu-id="4217c-119">Although thinking about modules passing messages tooeach other is a convenient way tooconceptualize how a gateway functions, it does not accurately reflect what happens.</span></span> <span data-ttu-id="4217c-120">IoT-Edge moduler använder en broker toocommunicate med varandra.</span><span class="sxs-lookup"><span data-stu-id="4217c-120">IoT Edge modules use a broker toocommunicate with each other.</span></span> <span data-ttu-id="4217c-121">Moduler publicera meddelanden toohello broker (med meddelandemönster, till exempel bus eller publicera/prenumerera) och därefter låta hello broker väg hello meddelandet toohello moduler anslutna tooit.</span><span class="sxs-lookup"><span data-stu-id="4217c-121">Modules publish messages toohello broker (using messaging patterns such as bus, or publish/subscribe) and then let hello broker route hello message toohello modules connected tooit.</span></span>

<span data-ttu-id="4217c-122">En modul använder hello **Broker_Publish** toopublish förhandlare meddelandet toohello att fungera.</span><span class="sxs-lookup"><span data-stu-id="4217c-122">A module uses hello **Broker_Publish** function toopublish a message toohello broker.</span></span> <span data-ttu-id="4217c-123">hello broker levererar meddelanden tooa modulen genom att anropa en callback-funktion.</span><span class="sxs-lookup"><span data-stu-id="4217c-123">hello broker delivers messages tooa module by invoking a callback function.</span></span> <span data-ttu-id="4217c-124">Ett meddelande består av en uppsättning nyckel-/värdeegenskaper och innehåll som skickas som ett block med minne.</span><span class="sxs-lookup"><span data-stu-id="4217c-124">A message consists of a set of key/value properties and content passed as a block of memory.</span></span>

![hello roll hello Broker i Azure IoT kant][3]

## <a name="message-routing-and-filtering"></a><span data-ttu-id="4217c-126">Meddelandedirigering och filtrering</span><span class="sxs-lookup"><span data-stu-id="4217c-126">Message routing and filtering</span></span>

<span data-ttu-id="4217c-127">Det finns två sätt toodirect meddelanden toohello rätt IoT kant moduler:</span><span class="sxs-lookup"><span data-stu-id="4217c-127">There are two ways toodirect messages toohello correct IoT Edge modules:</span></span>

* <span data-ttu-id="4217c-128">Du kan skicka en uppsättning länkar kan skickas toohello broker så hello broker vet hello källa och mottagare för varje modul.</span><span class="sxs-lookup"><span data-stu-id="4217c-128">You can pass a set of links can be passed toohello broker so hello broker knows hello source and sink for each module.</span></span>
* <span data-ttu-id="4217c-129">En modul kan filtrera efter hello egenskaper för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="4217c-129">A module can filter on hello properties of hello message.</span></span>

<span data-ttu-id="4217c-130">En modul bör endast agera på ett meddelande om hello-meddelande är avsedd för den.</span><span class="sxs-lookup"><span data-stu-id="4217c-130">A module should only act upon a message if hello message is intended for it.</span></span> <span data-ttu-id="4217c-131">Länkar och meddelandet filtrering effektivt skapar du en message-pipeline.</span><span class="sxs-lookup"><span data-stu-id="4217c-131">Links and message filtering effectively create a message pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4217c-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4217c-132">Next steps</span></span>

<span data-ttu-id="4217c-133">toosee dessa begrepp som används i ett prov du kan köra finns [utforska Azure IoT kant arkitektur][lnk-hello-world].</span><span class="sxs-lookup"><span data-stu-id="4217c-133">toosee these concepts applied in a sample you can run, see [Explore Azure IoT Edge architecture][lnk-hello-world].</span></span>

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md
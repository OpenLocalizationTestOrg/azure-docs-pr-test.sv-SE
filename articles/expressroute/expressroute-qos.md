---
title: "aaaQoS krav för ExpressRoute | Microsoft Docs"
description: "Den här sidan innehåller detaljerade krav för att konfigurera och hantera QoS för ExpressRoute-kretsar."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: db1c1447-0283-4a09-907b-ae481adc40c7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: cherylmc
ms.openlocfilehash: 46cc81bd38ff50dd9e7a1bfdd0faa457ff7b2fa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-qos-requirements"></a><span data-ttu-id="fc91b-103">QoS-krav för ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="fc91b-103">ExpressRoute QoS requirements</span></span>
<span data-ttu-id="fc91b-104">Skype för företag har olika arbetsbelastningar som kräver särskild QoS-behandling.</span><span class="sxs-lookup"><span data-stu-id="fc91b-104">Skype for Business has various workloads that require differentiated QoS treatment.</span></span> <span data-ttu-id="fc91b-105">Om du planerar tooconsume röst tjänster via ExpressRoute, bör du följa toohello krav som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="fc91b-105">If you plan tooconsume voice services through ExpressRoute, you should adhere toohello requirements described below.</span></span>

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> <span data-ttu-id="fc91b-106">QoS-krav tillämpa toohello Microsoft endast peering.</span><span class="sxs-lookup"><span data-stu-id="fc91b-106">QoS requirements apply toohello Microsoft peering only.</span></span> <span data-ttu-id="fc91b-107">hello DSCP-värden i nätverkstrafiken tas emot på offentlig Azure-peering och privat Azure-peering kommer att återställa too0.</span><span class="sxs-lookup"><span data-stu-id="fc91b-107">hello DSCP values in your network traffic received on Azure public peering and Azure private peering will be reset too0.</span></span> 
> 
> 

<span data-ttu-id="fc91b-108">hello innehåller följande tabell en lista över DSCP-markeringar som används av Skype för företag.</span><span class="sxs-lookup"><span data-stu-id="fc91b-108">hello following table provides a list of DSCP markings used by Skype for Business.</span></span> <span data-ttu-id="fc91b-109">Se för[hantera QoS för Skype för företag](https://technet.microsoft.com/library/gg405409.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="fc91b-109">Refer too[Managing QoS for Skype for Business](https://technet.microsoft.com/library/gg405409.aspx) for more information.</span></span>

| <span data-ttu-id="fc91b-110">**Trafikklass**</span><span class="sxs-lookup"><span data-stu-id="fc91b-110">**Traffic Class**</span></span> | <span data-ttu-id="fc91b-111">**Behandling (DSCP-markering)**</span><span class="sxs-lookup"><span data-stu-id="fc91b-111">**Treatment (DSCP Marking)**</span></span> | <span data-ttu-id="fc91b-112">**Arbetsbelastningar i Skype för företag**</span><span class="sxs-lookup"><span data-stu-id="fc91b-112">**Skype for Business Workloads**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fc91b-113">**Röst**</span><span class="sxs-lookup"><span data-stu-id="fc91b-113">**Voice**</span></span> |<span data-ttu-id="fc91b-114">EF (46)</span><span class="sxs-lookup"><span data-stu-id="fc91b-114">EF (46)</span></span> |<span data-ttu-id="fc91b-115">Skype-/Lync-röst</span><span class="sxs-lookup"><span data-stu-id="fc91b-115">Skype / Lync voice</span></span> |
| <span data-ttu-id="fc91b-116">**Interaktiv**</span><span class="sxs-lookup"><span data-stu-id="fc91b-116">**Interactive**</span></span> |<span data-ttu-id="fc91b-117">AF41 (34)</span><span class="sxs-lookup"><span data-stu-id="fc91b-117">AF41 (34)</span></span> |<span data-ttu-id="fc91b-118">Video, VBSS</span><span class="sxs-lookup"><span data-stu-id="fc91b-118">Video, VBSS</span></span> |
| <span data-ttu-id="fc91b-119">AF21 (18)</span><span class="sxs-lookup"><span data-stu-id="fc91b-119">AF21 (18)</span></span> |<span data-ttu-id="fc91b-120">Appdelning</span><span class="sxs-lookup"><span data-stu-id="fc91b-120">App sharing</span></span> | |
| <span data-ttu-id="fc91b-121">**Standard**</span><span class="sxs-lookup"><span data-stu-id="fc91b-121">**Default**</span></span> |<span data-ttu-id="fc91b-122">AF11 (10)</span><span class="sxs-lookup"><span data-stu-id="fc91b-122">AF11 (10)</span></span> |<span data-ttu-id="fc91b-123">Filöverföring</span><span class="sxs-lookup"><span data-stu-id="fc91b-123">File transfer</span></span> |
| <span data-ttu-id="fc91b-124">CS0 (0)</span><span class="sxs-lookup"><span data-stu-id="fc91b-124">CS0 (0)</span></span> |<span data-ttu-id="fc91b-125">Annat</span><span class="sxs-lookup"><span data-stu-id="fc91b-125">Anything else</span></span> | |

* <span data-ttu-id="fc91b-126">Du bör klassificera hello arbetsbelastningar och markera hello rätt DSCP-värden.</span><span class="sxs-lookup"><span data-stu-id="fc91b-126">You should classify hello workloads and mark hello right DSCP values.</span></span> <span data-ttu-id="fc91b-127">Följ riktlinjerna för hello [här](https://technet.microsoft.com/library/gg405409.aspx) om hur tooset DSCP-markeringar i nätverket.</span><span class="sxs-lookup"><span data-stu-id="fc91b-127">Follow hello guidance provided [here](https://technet.microsoft.com/library/gg405409.aspx) on how tooset DSCP markings in your network.</span></span>
* <span data-ttu-id="fc91b-128">Du bör konfigurera och ge stöd för flera QoS-köer i nätverket.</span><span class="sxs-lookup"><span data-stu-id="fc91b-128">You should configure and support multiple QoS queues within your network.</span></span> <span data-ttu-id="fc91b-129">Röst måste vara en fristående klass och behandlas hello EF anges i RFC 3246.</span><span class="sxs-lookup"><span data-stu-id="fc91b-129">Voice must be a standalone class and receive hello EF treatment specified in RFC 3246.</span></span> 
* <span data-ttu-id="fc91b-130">Du kan bestämma hello queuing mekanism överbelastning princip och bandbreddsallokering per trafik klass.</span><span class="sxs-lookup"><span data-stu-id="fc91b-130">You can decide hello queuing mechanism, congestion detection policy, and bandwidth allocation per traffic class.</span></span> <span data-ttu-id="fc91b-131">Men hello DSCP-markering för Skype för företag arbetsbelastningar måste bevaras.</span><span class="sxs-lookup"><span data-stu-id="fc91b-131">But, hello DSCP marking for Skype for Business workloads must be preserved.</span></span> <span data-ttu-id="fc91b-132">Om du använder DSCP-markeringar som inte listas här ovan, t.ex. AF31 (26), du måste skriva om too0 denna DSCP-värde innan du skickar hello paket tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="fc91b-132">If you are using DSCP markings not listed above, e.g. AF31 (26), you must rewrite this DSCP value too0 before sending hello packet tooMicrosoft.</span></span> <span data-ttu-id="fc91b-133">Microsoft skickar bara paket som markerats med hello DSCP-värde som visas i hello ovanför tabell.</span><span class="sxs-lookup"><span data-stu-id="fc91b-133">Microsoft only sends packets marked with hello DSCP value shown in hello above table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="fc91b-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc91b-134">Next steps</span></span>
* <span data-ttu-id="fc91b-135">Se toohello krav för [routning](expressroute-routing.md) och [NAT](expressroute-nat.md).</span><span class="sxs-lookup"><span data-stu-id="fc91b-135">Refer toohello requirements for [Routing](expressroute-routing.md) and [NAT](expressroute-nat.md).</span></span>
* <span data-ttu-id="fc91b-136">Se hello följande länkar tooconfigure ExpressRoute-anslutning.</span><span class="sxs-lookup"><span data-stu-id="fc91b-136">See hello following links tooconfigure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="fc91b-137">Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="fc91b-137">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-classic.md)
  * [<span data-ttu-id="fc91b-138">Konfigurera routning</span><span class="sxs-lookup"><span data-stu-id="fc91b-138">Configure routing</span></span>](expressroute-howto-routing-classic.md)
  * [<span data-ttu-id="fc91b-139">Länka ett VNet tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="fc91b-139">Link a VNet tooan ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)


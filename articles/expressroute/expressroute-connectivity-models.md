---
title: "ExpressRoute-anslutningsmodeller: Anslut till Microsoft Azure via nätverkstjänstleverantörer, utbyten och Ethernet-leverantörer | Microsoft Docs"
description: "I den här artikeln beskrivs olika lägen för anslutningen mellan kundens nätverk och Microsoft Azure-, Office 365- och Dynamics 365-tjänster. Kunder kan använda MPLS-leverantörer, molnutbyten och Ethernet-leverantörer."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2017
ms.author: cherylmc
ms.openlocfilehash: 00f97da2189491103c461b49ac82feb92d8f4b9b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="expressroute-connectivity-models"></a><span data-ttu-id="7044f-104">ExpressRoute-anslutningsmodeller</span><span class="sxs-lookup"><span data-stu-id="7044f-104">ExpressRoute connectivity models</span></span>
<span data-ttu-id="7044f-105">Du kan skapa en anslutning mellan ditt lokala nätverk och Microsoft-molnet på tre olika sätt, [CloudExchange-samplacering](#CloudExchange), [Ethernet-anslutning mellan punkter](#Ethernet) och [anslutning alla-till-alla (IPVPN)](#IPVPN).</span><span class="sxs-lookup"><span data-stu-id="7044f-105">You can create a connection between your on-premises network and the Microsoft cloud in three different ways, [CloudExchange Co-location](#CloudExchange), [Point-to-point Ethernet Connection](#Ethernet), and [Any-to-any (IPVPN) Connection](#IPVPN).</span></span> <span data-ttu-id="7044f-106">Anslutningsleverantörer kan erbjuda en eller flera modeller för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="7044f-106">Connectivity providers can offer one or more connectivity models.</span></span> <span data-ttu-id="7044f-107">Du kan rådfråga din anslutningsleverantör om vilken modell som passar dig bäst.</span><span class="sxs-lookup"><span data-stu-id="7044f-107">You can work with your connectivity provider to pick the model that works best for you.</span></span>
<br><br>

![Modelldiagram för ExpressRoute-anslutning](./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png)

## <span data-ttu-id="7044f-109"><a name="CloudExchange"></a>Samordnad i ett molnutbyte</span><span class="sxs-lookup"><span data-stu-id="7044f-109"><a name="CloudExchange"></a>Co-located at a cloud exchange</span></span>
<span data-ttu-id="7044f-110">Om du är samordnad i en anläggning med ett molnutbyte, kan du beställa virtuella korsanslutningar till Microsoft-molnet via samordningsleverantörens Ethernet-utbyte.</span><span class="sxs-lookup"><span data-stu-id="7044f-110">If you are co-located in a facility with a cloud exchange, you can order virtual cross-connections to the Microsoft cloud through the co-location provider’s Ethernet exchange.</span></span> <span data-ttu-id="7044f-111">Samordningsleverantörer kan erbjuda Layer-2-anslutningar eller hanterade Layer 3-korsanslutningar mellan infrastrukturen i samordningsanläggningen och Microsoft-molnet.</span><span class="sxs-lookup"><span data-stu-id="7044f-111">Co-location providers can offer either Layer 2 cross-connections, or managed Layer 3 cross-connections between your infrastructure in the co-location facility and the Microsoft cloud.</span></span>

## <span data-ttu-id="7044f-112"><a name="Ethernet"></a>Anslutningar mellan punkter med Ethernet</span><span class="sxs-lookup"><span data-stu-id="7044f-112"><a name="Ethernet"></a>Point-to-point Ethernet connections</span></span>
<span data-ttu-id="7044f-113">Du kan ansluta dina lokala datacenter/kontor till Microsoft-molnet via ”point-to-point”-länkar med Ethernet.</span><span class="sxs-lookup"><span data-stu-id="7044f-113">You can connect your on-premises datacenters/offices to the Microsoft cloud through point-to-point Ethernet links.</span></span> <span data-ttu-id="7044f-114">”Point-to-Point”-leverantörer kan erbjuda Layer-2-anslutningar eller hanterade Layer 3-anslutningar mellan din webbplats och Microsoft-molnet.</span><span class="sxs-lookup"><span data-stu-id="7044f-114">Point-to-point Ethernet providers can offer Layer 2 connections, or managed Layer 3 connections between your site and the Microsoft cloud.</span></span>

## <span data-ttu-id="7044f-115"><a name="IPVPN"></a>”Any-to-any”-nätverk (IPVPN)</span><span class="sxs-lookup"><span data-stu-id="7044f-115"><a name="IPVPN"></a>Any-to-any (IPVPN) networks</span></span>
<span data-ttu-id="7044f-116">Du kan integrera ditt WAN med Microsoft-molnet.</span><span class="sxs-lookup"><span data-stu-id="7044f-116">You can integrate your WAN with the Microsoft cloud.</span></span> <span data-ttu-id="7044f-117">IPVPN-leverantörer (vanligtvis MPLS VPN) erbjuder ”any-to-any”-anslutningar mellan dina avdelningskontor och datacentra.</span><span class="sxs-lookup"><span data-stu-id="7044f-117">IPVPN providers (typically MPLS VPN) offer any-to-any connectivity between your branch offices and datacenters.</span></span> <span data-ttu-id="7044f-118">Microsoft-molnet kan kopplas samman med ditt WAN-nätverk så att det ser ut precis som andra avdelningskontor.</span><span class="sxs-lookup"><span data-stu-id="7044f-118">The Microsoft cloud can be interconnected to your WAN to make it look just like any other branch office.</span></span> <span data-ttu-id="7044f-119">WAN-leverantörer erbjuder vanligtvis hanterad Layer 3-anslutning.</span><span class="sxs-lookup"><span data-stu-id="7044f-119">WAN providers typically offer managed Layer 3 connectivity.</span></span> <span data-ttu-id="7044f-120">Alla ExpressRoute-funktioner och egenskaper är identiska i ovanstående anslutningsmodeller.</span><span class="sxs-lookup"><span data-stu-id="7044f-120">ExpressRoute capabilities and features are all identical across all of the above connectivity models.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7044f-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7044f-121">Next steps</span></span>
* <span data-ttu-id="7044f-122">Läs mer om ExpressRoute-anslutningar och routningsdomäner.</span><span class="sxs-lookup"><span data-stu-id="7044f-122">Learn about ExpressRoute connections and routing domains.</span></span> <span data-ttu-id="7044f-123">Se [ExpressRoute-anslutningar och routningsdomäner](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="7044f-123">See [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="7044f-124">Läs mer om ExpressRoute-funktioner.</span><span class="sxs-lookup"><span data-stu-id="7044f-124">Learn about ExpressRoute features.</span></span> <span data-ttu-id="7044f-125">Se [Teknisk översikt för ExpressRoute](expressroute-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="7044f-125">See the [ExpressRoute Technical Overview](expressroute-introduction.md)</span></span>
* <span data-ttu-id="7044f-126">Hitta en tjänstleverantör.</span><span class="sxs-lookup"><span data-stu-id="7044f-126">Find a service provider.</span></span> <span data-ttu-id="7044f-127">Se [ExpressRoute-partners och peeringplatser](expressroute-locations.md).</span><span class="sxs-lookup"><span data-stu-id="7044f-127">See [ExpressRoute partners and peering locations](expressroute-locations.md).</span></span>
* <span data-ttu-id="7044f-128">Kontrollera att alla krav är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="7044f-128">Ensure that all prerequisites are met.</span></span> <span data-ttu-id="7044f-129">Se [ExpressRoute-krav](expressroute-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="7044f-129">See [ExpressRoute prerequisites](expressroute-prerequisites.md).</span></span>
* <span data-ttu-id="7044f-130">Se kraven för [routning](expressroute-routing.md), [NAT](expressroute-nat.md) och [QoS](expressroute-qos.md).</span><span class="sxs-lookup"><span data-stu-id="7044f-130">Refer to the requirements for [Routing](expressroute-routing.md), [NAT](expressroute-nat.md), and [QoS](expressroute-qos.md).</span></span>
* <span data-ttu-id="7044f-131">Konfigurera ExpressRoute-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="7044f-131">Configure your ExpressRoute connection.</span></span>
  * [<span data-ttu-id="7044f-132">Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7044f-132">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
  * [<span data-ttu-id="7044f-133">Konfigurera routning</span><span class="sxs-lookup"><span data-stu-id="7044f-133">Configure routing</span></span>](expressroute-howto-routing-portal-resource-manager.md)
  * [<span data-ttu-id="7044f-134">Länka ett VNet till en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="7044f-134">Link a VNet to an ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)
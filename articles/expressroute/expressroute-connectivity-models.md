---
title: "Modeller för ExpressRoute-anslutningen: ansluta tooMicrosoft Azure via nätverksleverantörer, utbyte och Ethernet-providers | Microsoft Docs"
description: "Den här artikeln beskriver hello olika lägen för anslutningen mellan hello kundens nätverk och Microsoft Azure och Office 365, Dynamics 365-tjänster. Kunder kan använda MPLS-leverantörer, molnutbyten och Ethernet-leverantörer."
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
ms.openlocfilehash: 2682e6e45b2892869068f132bedb4bb08e3f89a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-connectivity-models"></a><span data-ttu-id="4cf9d-104">ExpressRoute-anslutningsmodeller</span><span class="sxs-lookup"><span data-stu-id="4cf9d-104">ExpressRoute connectivity models</span></span>
<span data-ttu-id="4cf9d-105">Du kan skapa en anslutning mellan ditt lokala nätverk och hello Microsoft cloud på tre olika sätt, [CloudExchange samplacering](#CloudExchange), [Point-to-point Ethernet-anslutning](#Ethernet), och [Alla-till-alla (IPVPN) anslutning](#IPVPN).</span><span class="sxs-lookup"><span data-stu-id="4cf9d-105">You can create a connection between your on-premises network and hello Microsoft cloud in three different ways, [CloudExchange Co-location](#CloudExchange), [Point-to-point Ethernet Connection](#Ethernet), and [Any-to-any (IPVPN) Connection](#IPVPN).</span></span> <span data-ttu-id="4cf9d-106">Anslutningsleverantörer kan erbjuda en eller flera modeller för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-106">Connectivity providers can offer one or more connectivity models.</span></span> <span data-ttu-id="4cf9d-107">Du kan arbeta med din anslutning toopick hello leverantörsmodell som passar dig bäst.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-107">You can work with your connectivity provider toopick hello model that works best for you.</span></span>
<br><br>

![Modelldiagram för ExpressRoute-anslutning](./media/expressroute-connectivity-models/expressroute-connectivity-models-diagram.png)

## <span data-ttu-id="4cf9d-109"><a name="CloudExchange"></a>Samordnad i ett molnutbyte</span><span class="sxs-lookup"><span data-stu-id="4cf9d-109"><a name="CloudExchange"></a>Co-located at a cloud exchange</span></span>
<span data-ttu-id="4cf9d-110">Om du är samordnad på en plats med en exchange i molnet, kan du sortera virtuella cross-anslutningar toohello Microsoft cloud via hello samplacering providern Ethernet exchange.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-110">If you are co-located in a facility with a cloud exchange, you can order virtual cross-connections toohello Microsoft cloud through hello co-location provider’s Ethernet exchange.</span></span> <span data-ttu-id="4cf9d-111">Samplacering leverantörer kan erbjuda Lager2 cross-anslutningar eller hanterade Layer 3 cross-anslutningar mellan din infrastruktur i hello samplacering anläggning och hello Microsoft cloud.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-111">Co-location providers can offer either Layer 2 cross-connections, or managed Layer 3 cross-connections between your infrastructure in hello co-location facility and hello Microsoft cloud.</span></span>

## <span data-ttu-id="4cf9d-112"><a name="Ethernet"></a>Anslutningar mellan punkter med Ethernet</span><span class="sxs-lookup"><span data-stu-id="4cf9d-112"><a name="Ethernet"></a>Point-to-point Ethernet connections</span></span>
<span data-ttu-id="4cf9d-113">Du kan ansluta din lokala Datacenter/kontor toohello Microsoft cloud via point-to-point Ethernet-länkar.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-113">You can connect your on-premises datacenters/offices toohello Microsoft cloud through point-to-point Ethernet links.</span></span> <span data-ttu-id="4cf9d-114">Point-to-Point Ethernet-leverantörer kan erbjuda nivå 2-anslutningar eller hanterade Layer 3-anslutningar mellan platsen och hello Microsoft cloud.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-114">Point-to-point Ethernet providers can offer Layer 2 connections, or managed Layer 3 connections between your site and hello Microsoft cloud.</span></span>

## <span data-ttu-id="4cf9d-115"><a name="IPVPN"></a>”Any-to-any”-nätverk (IPVPN)</span><span class="sxs-lookup"><span data-stu-id="4cf9d-115"><a name="IPVPN"></a>Any-to-any (IPVPN) networks</span></span>
<span data-ttu-id="4cf9d-116">Du kan integrera din WAN med hello Microsoft cloud.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-116">You can integrate your WAN with hello Microsoft cloud.</span></span> <span data-ttu-id="4cf9d-117">IPVPN-leverantörer (vanligtvis MPLS VPN) erbjuder ”any-to-any”-anslutningar mellan dina avdelningskontor och datacentra.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-117">IPVPN providers (typically MPLS VPN) offer any-to-any connectivity between your branch offices and datacenters.</span></span> <span data-ttu-id="4cf9d-118">hello Microsoft molnet kan vara sammankopplade tooyour WAN toomake den ser ut precis som andra lokalkontoret.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-118">hello Microsoft cloud can be interconnected tooyour WAN toomake it look just like any other branch office.</span></span> <span data-ttu-id="4cf9d-119">WAN-leverantörer erbjuder vanligtvis hanterad Layer 3-anslutning.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-119">WAN providers typically offer managed Layer 3 connectivity.</span></span> <span data-ttu-id="4cf9d-120">ExpressRoute-funktioner är alla identiska för alla hello ovan connectivity-modeller.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-120">ExpressRoute capabilities and features are all identical across all of hello above connectivity models.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4cf9d-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4cf9d-121">Next steps</span></span>
* <span data-ttu-id="4cf9d-122">Läs mer om ExpressRoute-anslutningar och routningsdomäner.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-122">Learn about ExpressRoute connections and routing domains.</span></span> <span data-ttu-id="4cf9d-123">Se [ExpressRoute-anslutningar och routningsdomäner](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="4cf9d-123">See [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="4cf9d-124">Läs mer om ExpressRoute-funktioner.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-124">Learn about ExpressRoute features.</span></span> <span data-ttu-id="4cf9d-125">Se hello [teknisk översikt för ExpressRoute](expressroute-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="4cf9d-125">See hello [ExpressRoute Technical Overview](expressroute-introduction.md)</span></span>
* <span data-ttu-id="4cf9d-126">Hitta en tjänstleverantör.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-126">Find a service provider.</span></span> <span data-ttu-id="4cf9d-127">Se [ExpressRoute-partners och peeringplatser](expressroute-locations.md).</span><span class="sxs-lookup"><span data-stu-id="4cf9d-127">See [ExpressRoute partners and peering locations](expressroute-locations.md).</span></span>
* <span data-ttu-id="4cf9d-128">Kontrollera att alla krav är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-128">Ensure that all prerequisites are met.</span></span> <span data-ttu-id="4cf9d-129">Se [ExpressRoute-krav](expressroute-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="4cf9d-129">See [ExpressRoute prerequisites](expressroute-prerequisites.md).</span></span>
* <span data-ttu-id="4cf9d-130">Se toohello krav för [routning](expressroute-routing.md), [NAT](expressroute-nat.md), och [QoS](expressroute-qos.md).</span><span class="sxs-lookup"><span data-stu-id="4cf9d-130">Refer toohello requirements for [Routing](expressroute-routing.md), [NAT](expressroute-nat.md), and [QoS](expressroute-qos.md).</span></span>
* <span data-ttu-id="4cf9d-131">Konfigurera ExpressRoute-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="4cf9d-131">Configure your ExpressRoute connection.</span></span>
  * [<span data-ttu-id="4cf9d-132">Skapa en ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="4cf9d-132">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-portal-resource-manager.md)
  * [<span data-ttu-id="4cf9d-133">Konfigurera routning</span><span class="sxs-lookup"><span data-stu-id="4cf9d-133">Configure routing</span></span>](expressroute-howto-routing-portal-resource-manager.md)
  * [<span data-ttu-id="4cf9d-134">Länka ett VNet tooan ExpressRoute-krets</span><span class="sxs-lookup"><span data-stu-id="4cf9d-134">Link a VNet tooan ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-portal-resource-manager.md)

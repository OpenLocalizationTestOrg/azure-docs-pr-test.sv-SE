---
title: "Översikt över BGP med Azure VPN-gatewayer | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över BGP med Azures VPN-gatewayer."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: f8c3985c-c128-4f34-835c-0e88742bf36e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: 60d8dd45ecbd4a075721c25acadb21d4e2fd5448
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a><span data-ttu-id="1430f-103">Översikt över BGP med Azures VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="1430f-103">Overview of BGP with Azure VPN Gateways</span></span>
<span data-ttu-id="1430f-104">Den här artikeln innehåller en översikt över BGP-stöd (Border Gateway Protocol) i Azures VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="1430f-104">This article provides an overview of BGP (Border Gateway Protocol) support in Azure VPN Gateways.</span></span>

<span data-ttu-id="1430f-105">BGP är ett standardroutningsprotokoll som vanligen används på Internet för att utbyta information om routning och åtkomst mellan två eller flera nätverk.</span><span class="sxs-lookup"><span data-stu-id="1430f-105">BGP is the standard routing protocol commonly used in the Internet to exchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="1430f-106">När det används i virtuella Azure-nätverk möjliggör BGP att Azures VPN-gatewayer och dina lokala VPN-enheter (kallas BGP-peer eller grannar) kan utbyta ”vägar” som informerar båda gatewayerna om åtkomsten för de prefix som ska passera genom de gateways eller routrar som berörs.</span><span class="sxs-lookup"><span data-stu-id="1430f-106">When used in the context of Azure Virtual Networks, BGP enables the Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, to exchange "routes" that will inform both gateways on the availability and reachability for those prefixes to go through the gateways or routers involved.</span></span> <span data-ttu-id="1430f-107">BGP kan också möjliggöra överföringsroutning mellan flera nätverk genom att sprida vägar som BGP-gatewayen får information om från en BGP-peer till alla andra BGP-peers.</span><span class="sxs-lookup"><span data-stu-id="1430f-107">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer to all other BGP peers.</span></span> 

## <a name="why-use-bgp"></a><span data-ttu-id="1430f-108">Varför ska man använda BGP?</span><span class="sxs-lookup"><span data-stu-id="1430f-108">Why use BGP?</span></span>
<span data-ttu-id="1430f-109">BGP är en valfri funktion som du kan använda med Azures routningsbaserade VPN-gateways.</span><span class="sxs-lookup"><span data-stu-id="1430f-109">BGP is an optional feature you can use with Azure Route-Based VPN gateways.</span></span> <span data-ttu-id="1430f-110">Du bör också kontrollera att dina lokala VPN-enheter stöder BGP innan du aktiverar funktionen.</span><span class="sxs-lookup"><span data-stu-id="1430f-110">You should also make sure your on-premises VPN devices support BGP before you enable the feature.</span></span> <span data-ttu-id="1430f-111">Du kan fortsätta att använda Azures VPN-gatewayer och lokala VPN-enheter utan BGP.</span><span class="sxs-lookup"><span data-stu-id="1430f-111">You can continue to use Azure VPN gateways and your on-premises VPN devices without BGP.</span></span> <span data-ttu-id="1430f-112">Det motsvarar att använda statiska vägar (utan BGP) *kontra* dynamisk routning med BGP mellan dina nätverk och Azure.</span><span class="sxs-lookup"><span data-stu-id="1430f-112">It is the equivalent of using static routes (without BGP) *vs.* using dynamic routing with BGP between your networks and Azure.</span></span>

<span data-ttu-id="1430f-113">Det finns flera fördelar och nya funktioner med BGP:</span><span class="sxs-lookup"><span data-stu-id="1430f-113">There are several advantages and new capabilities with BGP:</span></span>

### <a name="support-automatic-and-flexible-prefix-updates"></a><span data-ttu-id="1430f-114">Stöd för automatiska och flexibla prefixuppdateringar</span><span class="sxs-lookup"><span data-stu-id="1430f-114">Support automatic and flexible prefix updates</span></span>
<span data-ttu-id="1430f-115">Med BGP behöver du bara deklarera ett minsta prefix till en specifik BGP-peer via IPsec S2S VPN-tunneln.</span><span class="sxs-lookup"><span data-stu-id="1430f-115">With BGP, you only need to declare a minimum prefix to a specific BGP peer over the IPsec S2S VPN tunnel.</span></span> <span data-ttu-id="1430f-116">Det kan vara så litet som ett värdprefix (/32) för BGP-peerens IP-adressen till din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="1430f-116">It can be as small as a host prefix (/32) of the BGP peer IP address of your on-premises VPN device.</span></span> <span data-ttu-id="1430f-117">Du kan styra vilka lokala nätverksprefix som du vill annonsera till Azure för att tillåta åtkomst för Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="1430f-117">You can control which on-premises network prefixes you want to advertise to Azure to allow your Azure Virtual Network to access.</span></span>

<span data-ttu-id="1430f-118">Du kan också annonsera större prefix som kan innehålla några av dina VNet-adressprefixer, till exempel en stor privata IP-adressutrymme (till exempel 10.0.0.0/8).</span><span class="sxs-lookup"><span data-stu-id="1430f-118">You can also advertise larger prefixes that may include some of your VNet address prefixes, such as a large private IP address space (for example, 10.0.0.0/8).</span></span> <span data-ttu-id="1430f-119">Observera att prefix inte får vara identiskt med något av VNet-prefix.</span><span class="sxs-lookup"><span data-stu-id="1430f-119">Note though the prefixes cannot be identical with any one of your VNet prefixes.</span></span> <span data-ttu-id="1430f-120">De vägar som är identiska med dina VNet-prefix kommer att avvisas.</span><span class="sxs-lookup"><span data-stu-id="1430f-120">Those routes identical to your VNet prefixes will be rejected.</span></span>

### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a><span data-ttu-id="1430f-121">Stöd för flera tunnlar mellan ett VNet och en lokal plats med automatisk redundans baserat på BGP</span><span class="sxs-lookup"><span data-stu-id="1430f-121">Support multiple tunnels between a VNet and an on-premises site with automatic failover based on BGP</span></span>
<span data-ttu-id="1430f-122">Du kan upprätta flera anslutningar mellan ditt Azure-VNet och dina lokala VPN-enheter på samma plats.</span><span class="sxs-lookup"><span data-stu-id="1430f-122">You can establish multiple connections between your Azure VNet and your on-premises VPN devices in the same location.</span></span> <span data-ttu-id="1430f-123">Den här funktionen innehåller flera tunnlar (sökvägar) mellan två nätverk i en aktiv-aktiv konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1430f-123">This capability provides multiple tunnels (paths) between the two networks in an active-active configuration.</span></span> <span data-ttu-id="1430f-124">Om något av tunnlarna kopplas från, kommer att tas motsvarande vägar via BGP och trafiken flyttas automatiskt till återstående tunnlar.</span><span class="sxs-lookup"><span data-stu-id="1430f-124">If one of the tunnels is disconnected, the corresponding routes will be withdrawn via BGP and the traffic automatically shifts to the remaining tunnels.</span></span>

<span data-ttu-id="1430f-125">I följande diagram visas ett enkelt exempel på den här installationen med hög tillgänglighet:</span><span class="sxs-lookup"><span data-stu-id="1430f-125">The following diagram shows a simple example of this highly available setup:</span></span>

![Flera aktiva sökvägar](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a><span data-ttu-id="1430f-127">Stöd för överföringsroutning mellan dina lokala nätverk och flera Azure-VNets</span><span class="sxs-lookup"><span data-stu-id="1430f-127">Support transit routing between your on-premises networks and multiple Azure VNets</span></span>
<span data-ttu-id="1430f-128">Med BGP kan flera gateways lära sig och sprida prefix från olika nätverk, oavsett om de är direkt eller indirekt anslutna.</span><span class="sxs-lookup"><span data-stu-id="1430f-128">BGP enables multiple gateways to learn and propagate prefixes from different networks, whether they are directly or indirectly connected.</span></span> <span data-ttu-id="1430f-129">Detta kan möjliggöra överföringsroutning med Azures VPN- gatewayer mellan lokala platser eller över flera virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="1430f-129">This can enable transit routing with Azure VPN gateways between your on-premises sites or across multiple Azure Virtual Networks.</span></span>

<span data-ttu-id="1430f-130">I följande diagram visas ett exempel på en multihopptopologi med flera sökvägar som kan passera mellan de två lokala nätverken via Azures VPN-gatewayer inom Microsoft Networks:</span><span class="sxs-lookup"><span data-stu-id="1430f-130">The following diagram shows an example of a multi-hop topology with multiple paths that can transit traffic between the two on-premises networks through Azure VPN gateways within the Microsoft Networks:</span></span>

![Multihoppöverföring](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faq"></a><span data-ttu-id="1430f-132">BGP VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="1430f-132">BGP FAQ</span></span>
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="1430f-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1430f-133">Next steps</span></span>
<span data-ttu-id="1430f-134">Se i [Komma igång med BGP på Azures VPN-gatewayer](vpn-gateway-bgp-resource-manager-ps.md) hur du konfigurerar BGP för flera lokala platser och VNet-till-VNet-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="1430f-134">See [Getting started with BGP on Azure VPN gateways](vpn-gateway-bgp-resource-manager-ps.md) for steps to configure BGP for your cross-premises and VNet-to-VNet connections.</span></span>


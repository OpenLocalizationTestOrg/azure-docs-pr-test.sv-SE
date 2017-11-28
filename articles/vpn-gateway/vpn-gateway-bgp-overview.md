---
title: aaaOverview av BGP med Azure VPN-gatewayer | Microsoft Docs
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
ms.openlocfilehash: ced3f77ecd791c84fb72b96447e839be3bf94846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a><span data-ttu-id="53942-103">Översikt över BGP med Azures VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="53942-103">Overview of BGP with Azure VPN Gateways</span></span>
<span data-ttu-id="53942-104">Den här artikeln innehåller en översikt över BGP-stöd (Border Gateway Protocol) i Azures VPN-gatewayer.</span><span class="sxs-lookup"><span data-stu-id="53942-104">This article provides an overview of BGP (Border Gateway Protocol) support in Azure VPN Gateways.</span></span>

<span data-ttu-id="53942-105">BGP är hello standard routingprotokoll ofta används i hello Internet tooexchange Routning och reachability information mellan två eller flera nätverk.</span><span class="sxs-lookup"><span data-stu-id="53942-105">BGP is hello standard routing protocol commonly used in hello Internet tooexchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="53942-106">När användas i hello kontext med virtuella Azure-nätverk, kan BGP hello Azure VPN-gatewayer och din lokala VPN-enheter som kallas BGP-peers eller grannar, tooexchange ”dirigerar” som informerar båda gateways på hello tillgänglighet och tillgänglighet för dem prefix toogo via hello gateways eller routrar som ingår.</span><span class="sxs-lookup"><span data-stu-id="53942-106">When used in hello context of Azure Virtual Networks, BGP enables hello Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, tooexchange "routes" that will inform both gateways on hello availability and reachability for those prefixes toogo through hello gateways or routers involved.</span></span> <span data-ttu-id="53942-107">BGP kan också aktivera transitroutning mellan flera nätverk av sprider vägar som en gateway med BGP lär sig från en BGP peer tooall andra BGP-peers.</span><span class="sxs-lookup"><span data-stu-id="53942-107">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer tooall other BGP peers.</span></span> 

## <a name="why-use-bgp"></a><span data-ttu-id="53942-108">Varför ska man använda BGP?</span><span class="sxs-lookup"><span data-stu-id="53942-108">Why use BGP?</span></span>
<span data-ttu-id="53942-109">BGP är en valfri funktion som du kan använda med Azures routningsbaserade VPN-gateways.</span><span class="sxs-lookup"><span data-stu-id="53942-109">BGP is an optional feature you can use with Azure Route-Based VPN gateways.</span></span> <span data-ttu-id="53942-110">Du bör också kontrollera din lokala VPN-enheter stöder BGP innan du aktiverar hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="53942-110">You should also make sure your on-premises VPN devices support BGP before you enable hello feature.</span></span> <span data-ttu-id="53942-111">Du kan fortsätta toouse Azure VPN-gatewayer och lokala VPN-enheter utan BGP.</span><span class="sxs-lookup"><span data-stu-id="53942-111">You can continue toouse Azure VPN gateways and your on-premises VPN devices without BGP.</span></span> <span data-ttu-id="53942-112">Det är hello motsvarande för att använda statiska vägar (utan BGP) *kontra* med hjälp av dynamisk routning med BGP mellan ditt nätverk och Azure.</span><span class="sxs-lookup"><span data-stu-id="53942-112">It is hello equivalent of using static routes (without BGP) *vs.* using dynamic routing with BGP between your networks and Azure.</span></span>

<span data-ttu-id="53942-113">Det finns flera fördelar och nya funktioner med BGP:</span><span class="sxs-lookup"><span data-stu-id="53942-113">There are several advantages and new capabilities with BGP:</span></span>

### <a name="support-automatic-and-flexible-prefix-updates"></a><span data-ttu-id="53942-114">Stöd för automatiska och flexibla prefixuppdateringar</span><span class="sxs-lookup"><span data-stu-id="53942-114">Support automatic and flexible prefix updates</span></span>
<span data-ttu-id="53942-115">Med BGP behöver du bara toodeclare en minsta prefixet tooa specifika BGP-peer för hello IPsec S2S VPN-tunneln.</span><span class="sxs-lookup"><span data-stu-id="53942-115">With BGP, you only need toodeclare a minimum prefix tooa specific BGP peer over hello IPsec S2S VPN tunnel.</span></span> <span data-ttu-id="53942-116">Det kan vara så liten som en värdprefixet (/ 32) av hello BGP peer-IP-adressen för din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="53942-116">It can be as small as a host prefix (/32) of hello BGP peer IP address of your on-premises VPN device.</span></span> <span data-ttu-id="53942-117">Du kan styra vilka lokala nätverksprefix som du vill tooadvertise tooAzure tooallow Azure Virtual Network-tooaccess.</span><span class="sxs-lookup"><span data-stu-id="53942-117">You can control which on-premises network prefixes you want tooadvertise tooAzure tooallow your Azure Virtual Network tooaccess.</span></span>

<span data-ttu-id="53942-118">Du kan också annonsera större prefix som kan innehålla några av dina VNet-adressprefixer, till exempel en stor privata IP-adressutrymme (till exempel 10.0.0.0/8).</span><span class="sxs-lookup"><span data-stu-id="53942-118">You can also advertise larger prefixes that may include some of your VNet address prefixes, such as a large private IP address space (for example, 10.0.0.0/8).</span></span> <span data-ttu-id="53942-119">Observera att hello prefix inte får vara identiskt med något av VNet-prefix.</span><span class="sxs-lookup"><span data-stu-id="53942-119">Note though hello prefixes cannot be identical with any one of your VNet prefixes.</span></span> <span data-ttu-id="53942-120">Dessa vägar identiska tooyour VNet-prefix avvisas.</span><span class="sxs-lookup"><span data-stu-id="53942-120">Those routes identical tooyour VNet prefixes will be rejected.</span></span>

### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a><span data-ttu-id="53942-121">Stöd för flera tunnlar mellan ett VNet och en lokal plats med automatisk redundans baserat på BGP</span><span class="sxs-lookup"><span data-stu-id="53942-121">Support multiple tunnels between a VNet and an on-premises site with automatic failover based on BGP</span></span>
<span data-ttu-id="53942-122">Du kan skapa flera anslutningar mellan ditt Azure VNet och lokala VPN-enheter i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="53942-122">You can establish multiple connections between your Azure VNet and your on-premises VPN devices in hello same location.</span></span> <span data-ttu-id="53942-123">Den här funktionen innehåller flera tunnlar (sökvägar) mellan hello två nätverk i en aktiv-aktiv konfiguration.</span><span class="sxs-lookup"><span data-stu-id="53942-123">This capability provides multiple tunnels (paths) between hello two networks in an active-active configuration.</span></span> <span data-ttu-id="53942-124">Om en av hello tunnlar kopplas hello motsvarande vägar utgår via BGP och hello trafik flyttas automatiskt toohello återstående tunnlar.</span><span class="sxs-lookup"><span data-stu-id="53942-124">If one of hello tunnels is disconnected, hello corresponding routes will be withdrawn via BGP and hello traffic automatically shifts toohello remaining tunnels.</span></span>

<span data-ttu-id="53942-125">hello följande diagram visar ett enkelt exempel på hög tillgänglighet installationen:</span><span class="sxs-lookup"><span data-stu-id="53942-125">hello following diagram shows a simple example of this highly available setup:</span></span>

![Flera aktiva sökvägar](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a><span data-ttu-id="53942-127">Stöd för överföringsroutning mellan dina lokala nätverk och flera Azure-VNets</span><span class="sxs-lookup"><span data-stu-id="53942-127">Support transit routing between your on-premises networks and multiple Azure VNets</span></span>
<span data-ttu-id="53942-128">BGP kan flera gateways toolearn och sprida prefix från flera olika nätverk, om de är direkt eller indirekt anslutna.</span><span class="sxs-lookup"><span data-stu-id="53942-128">BGP enables multiple gateways toolearn and propagate prefixes from different networks, whether they are directly or indirectly connected.</span></span> <span data-ttu-id="53942-129">Detta kan möjliggöra överföringsroutning med Azures VPN- gatewayer mellan lokala platser eller över flera virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="53942-129">This can enable transit routing with Azure VPN gateways between your on-premises sites or across multiple Azure Virtual Networks.</span></span>

<span data-ttu-id="53942-130">hello visar följande diagram ett exempel på en Multi-hop topologi med flera sökvägar som kan transit trafik mellan hello två lokala nätverk via Azure VPN-gatewayer i hello Microsoft Networks:</span><span class="sxs-lookup"><span data-stu-id="53942-130">hello following diagram shows an example of a multi-hop topology with multiple paths that can transit traffic between hello two on-premises networks through Azure VPN gateways within hello Microsoft Networks:</span></span>

![Multihoppöverföring](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faq"></a><span data-ttu-id="53942-132">BGP VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="53942-132">BGP FAQ</span></span>
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="53942-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53942-133">Next steps</span></span>
<span data-ttu-id="53942-134">Se [komma igång med BGP på Azure VPN-gatewayer](vpn-gateway-bgp-resource-manager-ps.md) för steg tooconfigure BGP för anslutningar mellan platser och VNet-till-VNet-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="53942-134">See [Getting started with BGP on Azure VPN gateways](vpn-gateway-bgp-resource-manager-ps.md) for steps tooconfigure BGP for your cross-premises and VNet-to-VNet connections.</span></span>


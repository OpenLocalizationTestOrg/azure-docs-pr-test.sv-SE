---
title: "Planering och design för anslutningar mellan platser: Azure VPN-Gateway | Microsoft Docs"
description: "Lär dig mer om VPN-Gateway planering och design för anslutningar mellan platser, hybrid och VNet-till-VNet-anslutningar"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: d5aaab83-4e74-4484-8bf0-cc465811e757
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2017
ms.author: cherylmc
ms.openlocfilehash: 0ebc3ef4a64432e993dd6ed69766bb64544fe433
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a><span data-ttu-id="53b9a-103">Planering och design för VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="53b9a-103">Planning and design for VPN Gateway</span></span>

<span data-ttu-id="53b9a-104">Planera och utforma din mellan platser och VNet-till-VNet-konfigurationer kan vara antingen enkla eller komplexa, beroende på dina nätverk behov.</span><span class="sxs-lookup"><span data-stu-id="53b9a-104">Planning and designing your cross-premises and VNet-to-VNet configurations can be either simple, or complicated, depending on your networking needs.</span></span> <span data-ttu-id="53b9a-105">Den här artikeln vägleder dig genom grundläggande överväganden för planering och design.</span><span class="sxs-lookup"><span data-stu-id="53b9a-105">This article walks you through basic planning and design considerations.</span></span>

## <span data-ttu-id="53b9a-106"><a name="planning"></a>Planera</span><span class="sxs-lookup"><span data-stu-id="53b9a-106"><a name="planning"></a>Planning</span></span>

### <span data-ttu-id="53b9a-107"><a name="compare"></a>Alternativ för nätverksanslutning mellan platser</span><span class="sxs-lookup"><span data-stu-id="53b9a-107"><a name="compare"></a>Cross-premises connectivity options</span></span>

<span data-ttu-id="53b9a-108">Om du vill ansluta din lokala platser på ett säkert sätt till ett virtuellt nätverk du har tre olika sätt att göra det: plats-till-plats, punkt-till-plats och ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="53b9a-108">If you want to connect your on-premises sites securely to a virtual network, you have three different ways to do so: Site-to-Site, Point-to-Site, and ExpressRoute.</span></span> <span data-ttu-id="53b9a-109">Jämför olika mellan lokala anslutningar som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="53b9a-109">Compare the different cross-premises connections that are available.</span></span> <span data-ttu-id="53b9a-110">Det alternativ du väljer kan bero på olika överväganden som:</span><span class="sxs-lookup"><span data-stu-id="53b9a-110">The option you choose can depend on various considerations, such as:</span></span>

* <span data-ttu-id="53b9a-111">Vilken typ av dataflöde kräver din lösning?</span><span class="sxs-lookup"><span data-stu-id="53b9a-111">What kind of throughput does your solution require?</span></span>
* <span data-ttu-id="53b9a-112">Vill du kommunicera över offentligt Internet via en säker VPN eller via en privat anslutning?</span><span class="sxs-lookup"><span data-stu-id="53b9a-112">Do you want to communicate over the public Internet via secure VPN, or over a private connection?</span></span>
* <span data-ttu-id="53b9a-113">Har du en offentlig IP-adress som kan användas?</span><span class="sxs-lookup"><span data-stu-id="53b9a-113">Do you have a public IP address available to use?</span></span>
* <span data-ttu-id="53b9a-114">Planerar du att använda någon VPN-enhet?</span><span class="sxs-lookup"><span data-stu-id="53b9a-114">Are you planning to use a VPN device?</span></span> <span data-ttu-id="53b9a-115">I så fall, är den kompatibel?</span><span class="sxs-lookup"><span data-stu-id="53b9a-115">If so, is it compatible?</span></span>
* <span data-ttu-id="53b9a-116">Ansluter du bara några få datorer eller vill du ha en permanent anslutning för platsen?</span><span class="sxs-lookup"><span data-stu-id="53b9a-116">Are you connecting just a few computers, or do you want a persistent connection for your site?</span></span>
* <span data-ttu-id="53b9a-117">Vilken typ av VPN-gateway krävs för lösningen som du vill skapa?</span><span class="sxs-lookup"><span data-stu-id="53b9a-117">What type of VPN gateway is required for the solution you want to create?</span></span>
* <span data-ttu-id="53b9a-118">Vilken gateway SKU bör du använda?</span><span class="sxs-lookup"><span data-stu-id="53b9a-118">Which gateway SKU should you use?</span></span>

### <span data-ttu-id="53b9a-119"><a name="planningtable"></a>Planera tabell</span><span class="sxs-lookup"><span data-stu-id="53b9a-119"><a name="planningtable"></a>Planning table</span></span>

<span data-ttu-id="53b9a-120">I följande tabell kan hjälpa dig att bestämma det bästa alternativet för anslutning för din lösning.</span><span class="sxs-lookup"><span data-stu-id="53b9a-120">The following table can help you decide the best connectivity option for your solution.</span></span>

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <span data-ttu-id="53b9a-121"><a name="gwsku"></a>Gateway-SKU:er</span><span class="sxs-lookup"><span data-stu-id="53b9a-121"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <span data-ttu-id="53b9a-122"><a name="wf"></a>Arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="53b9a-122"><a name="wf"></a>Workflow</span></span>

<span data-ttu-id="53b9a-123">I följande lista beskrivs vanliga arbetsflödet för molnet anslutning:</span><span class="sxs-lookup"><span data-stu-id="53b9a-123">The following list outlines the common workflow for cloud connectivity:</span></span>

1. <span data-ttu-id="53b9a-124">Utforma och planera din topologi för anslutningen och ange-adressutrymmen för alla nätverk som du vill ansluta till.</span><span class="sxs-lookup"><span data-stu-id="53b9a-124">Design and plan your connectivity topology and list the address spaces for all networks you want to connect.</span></span>
2. <span data-ttu-id="53b9a-125">Skapa ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="53b9a-125">Create an Azure virtual network.</span></span> 
3. <span data-ttu-id="53b9a-126">Skapa en VPN-gateway för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="53b9a-126">Create a VPN gateway for the virtual network.</span></span>
4. <span data-ttu-id="53b9a-127">Skapa och konfigurera anslutningar till lokala nätverk eller andra virtuella nätverk (vid behov).</span><span class="sxs-lookup"><span data-stu-id="53b9a-127">Create and configure connections to on-premises networks or other virtual networks (as needed).</span></span>
5. <span data-ttu-id="53b9a-128">Skapa och konfigurera en punkt-till-plats-anslutning för din Azure VPN-gateway (vid behov).</span><span class="sxs-lookup"><span data-stu-id="53b9a-128">Create and configure a Point-to-Site connection for your Azure VPN gateway (as needed).</span></span>

## <span data-ttu-id="53b9a-129"><a name="design"></a>Design</span><span class="sxs-lookup"><span data-stu-id="53b9a-129"><a name="design"></a>Design</span></span>
### <span data-ttu-id="53b9a-130"><a name="topologies"></a>Topologier för anslutning</span><span class="sxs-lookup"><span data-stu-id="53b9a-130"><a name="topologies"></a>Connection topologies</span></span>

<span data-ttu-id="53b9a-131">Starta genom att studera diagrammen i den [om VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="53b9a-131">Start by looking at the diagrams in the [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span> <span data-ttu-id="53b9a-132">Artikeln innehåller vanliga diagram distributionsmodeller för varje topologi och tillgänglig distribution-verktyg som du kan använda för att distribuera din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="53b9a-132">The article contains basic diagrams, the deployment models for each topology, and the available deployment tools you can use to deploy your configuration.</span></span>

### <span data-ttu-id="53b9a-133"><a name="designbasics"></a>Design-grunderna</span><span class="sxs-lookup"><span data-stu-id="53b9a-133"><a name="designbasics"></a>Design basics</span></span>

<span data-ttu-id="53b9a-134">I följande avsnitt beskrivs grunderna för VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="53b9a-134">The following sections discuss the VPN gateway basics.</span></span> 

#### <span data-ttu-id="53b9a-135"><a name="servicelimits"></a>Nätverk tjänster gränser</span><span class="sxs-lookup"><span data-stu-id="53b9a-135"><a name="servicelimits"></a>Networking services limits</span></span>

<span data-ttu-id="53b9a-136">Rulla tabeller för att visa [nätverk services gränser](../azure-subscription-service-limits.md#networking-limits).</span><span class="sxs-lookup"><span data-stu-id="53b9a-136">Scroll through the tables to view [networking services limits](../azure-subscription-service-limits.md#networking-limits).</span></span> <span data-ttu-id="53b9a-137">De gränser som visas kan påverka din design.</span><span class="sxs-lookup"><span data-stu-id="53b9a-137">The limits listed may impact your design.</span></span>

#### <span data-ttu-id="53b9a-138"><a name="subnets"></a>Om undernät</span><span class="sxs-lookup"><span data-stu-id="53b9a-138"><a name="subnets"></a>About subnets</span></span>

<span data-ttu-id="53b9a-139">När du skapar anslutningar måste du undernät-intervall.</span><span class="sxs-lookup"><span data-stu-id="53b9a-139">When you are creating connections, you must consider your subnet ranges.</span></span> <span data-ttu-id="53b9a-140">Du kan inte ha överlappande undernät-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="53b9a-140">You cannot have overlapping subnet address ranges.</span></span> <span data-ttu-id="53b9a-141">Ett annat överlappande undernät är när ett virtuellt nätverk eller lokala plats innehåller samma adressutrymme som innehåller den andra platsen.</span><span class="sxs-lookup"><span data-stu-id="53b9a-141">An overlapping subnet is when one virtual network or on-premises location contains the same address space that the other location contains.</span></span> <span data-ttu-id="53b9a-142">Detta innebär att du måste din nätverket tekniker för dina lokala lokalt nätverk för att dela ut ett intervall som du kan använda för din Azure-IP-adressering utrymme-undernät.</span><span class="sxs-lookup"><span data-stu-id="53b9a-142">This means that you need your network engineers for your local on-premises networks to carve out a range for you to use for your Azure IP addressing space/subnets.</span></span> <span data-ttu-id="53b9a-143">Du måste adressutrymme som inte används på det lokala lokala nätverket.</span><span class="sxs-lookup"><span data-stu-id="53b9a-143">You need address space that is not being used on the local on-premises network.</span></span>

<span data-ttu-id="53b9a-144">Undvika överlappande undernät är också viktigt när du arbetar med VNet-till-VNet-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="53b9a-144">Avoiding overlapping subnets is also important when you are working with VNet-to-VNet connections.</span></span> <span data-ttu-id="53b9a-145">Om dina undernät överlappar varandra och en IP-adress finns i både skicka och Vnet, misslyckas VNet-till-VNet-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="53b9a-145">If your subnets overlap and an IP address exists in both the sending and destination VNets, VNet-to-VNet connections fail.</span></span> <span data-ttu-id="53b9a-146">Azure kan inte skicka data till andra VNet eftersom måladressen tillhör skicka VNet.</span><span class="sxs-lookup"><span data-stu-id="53b9a-146">Azure can't route the data to the other VNet because the destination address is part of the sending VNet.</span></span>

<span data-ttu-id="53b9a-147">VPN-gatewayer kräver ett specifikt undernät som kallas en gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="53b9a-147">VPN Gateways require a specific subnet called a gateway subnet.</span></span> <span data-ttu-id="53b9a-148">Alla gateway-undernät måste ha namnet GatewaySubnet för att fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="53b9a-148">All gateway subnets must be named GatewaySubnet to work properly.</span></span> <span data-ttu-id="53b9a-149">Se till att inte namnge din gateway-undernätet ett annat namn och inte distribuera virtuella datorer eller något annat till gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="53b9a-149">Be sure not to name your gateway subnet a different name, and don't deploy VMs or anything else to the gateway subnet.</span></span> <span data-ttu-id="53b9a-150">Se [Gateway-undernät](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span><span class="sxs-lookup"><span data-stu-id="53b9a-150">See [Gateway Subnets](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span></span>

#### <span data-ttu-id="53b9a-151"><a name="local"></a>Om lokala nätverksgatewayer</span><span class="sxs-lookup"><span data-stu-id="53b9a-151"><a name="local"></a>About local network gateways</span></span>

<span data-ttu-id="53b9a-152">Den lokala nätverksgatewayen avser vanligtvis din lokala plats.</span><span class="sxs-lookup"><span data-stu-id="53b9a-152">The local network gateway typically refers to your on-premises location.</span></span> <span data-ttu-id="53b9a-153">I den klassiska distributionsmodellen kallas den lokala nätverksgatewayen en lokal nätverksplats.</span><span class="sxs-lookup"><span data-stu-id="53b9a-153">In the classic deployment model, the local network gateway is referred to as a Local Network Site.</span></span> <span data-ttu-id="53b9a-154">När du konfigurerar en lokal nätverksgateway du ge det ett namn, ange den offentliga IP-adressen för lokala VPN-enhet och ange adressprefix i den lokala platsen.</span><span class="sxs-lookup"><span data-stu-id="53b9a-154">When you configure a local network gateway, you give it a name, specify the public IP address of the on-premises VPN device, and specify the address prefixes that are in the on-premises location.</span></span> <span data-ttu-id="53b9a-155">Azure tittar på mål-adressprefix för nätverkstrafik, tittar du den konfiguration som du har angett för den lokala nätverksgatewayen och därefter dirigerar paket.</span><span class="sxs-lookup"><span data-stu-id="53b9a-155">Azure looks at the destination address prefixes for network traffic, consults the configuration that you have specified for the local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="53b9a-156">Du kan ändra adressprefixen efter behov.</span><span class="sxs-lookup"><span data-stu-id="53b9a-156">You can modify the address prefixes as needed.</span></span> <span data-ttu-id="53b9a-157">Mer information finns i [lokala nätverksgatewayer](vpn-gateway-about-vpn-gateway-settings.md#lng).</span><span class="sxs-lookup"><span data-stu-id="53b9a-157">For more information, see [Local network gateways](vpn-gateway-about-vpn-gateway-settings.md#lng).</span></span>

#### <span data-ttu-id="53b9a-158"><a name="gwtype"></a>Om gateway-typer</span><span class="sxs-lookup"><span data-stu-id="53b9a-158"><a name="gwtype"></a>About gateway types</span></span>

<span data-ttu-id="53b9a-159">Det är viktigt att välja rätt gateway-typ för din topologi.</span><span class="sxs-lookup"><span data-stu-id="53b9a-159">Selecting the correct gateway type for your topology is critical.</span></span> <span data-ttu-id="53b9a-160">Om du väljer fel typ fungerar din gateway inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="53b9a-160">If you select the wrong type, your gateway won't work properly.</span></span> <span data-ttu-id="53b9a-161">Gateway-typen anger hur gatewayen ansluter och är en obligatorisk konfigurationsinställning i Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="53b9a-161">The gateway type specifies how the gateway itself connects and is a required configuration setting for the Resource Manager deployment model.</span></span>

<span data-ttu-id="53b9a-162">Gateway-typer är:</span><span class="sxs-lookup"><span data-stu-id="53b9a-162">The gateway types are:</span></span>

* <span data-ttu-id="53b9a-163">Vpn</span><span class="sxs-lookup"><span data-stu-id="53b9a-163">Vpn</span></span>
* <span data-ttu-id="53b9a-164">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="53b9a-164">ExpressRoute</span></span>

#### <span data-ttu-id="53b9a-165"><a name="connectiontype"></a>Om anslutningstyper</span><span class="sxs-lookup"><span data-stu-id="53b9a-165"><a name="connectiontype"></a>About connection types</span></span>

<span data-ttu-id="53b9a-166">Varje konfiguration kräver en viss anslutningstyp.</span><span class="sxs-lookup"><span data-stu-id="53b9a-166">Each configuration requires a specific connection type.</span></span> <span data-ttu-id="53b9a-167">Anslutningstyperna är:</span><span class="sxs-lookup"><span data-stu-id="53b9a-167">The connection types are:</span></span>

* <span data-ttu-id="53b9a-168">IPsec</span><span class="sxs-lookup"><span data-stu-id="53b9a-168">IPsec</span></span>
* <span data-ttu-id="53b9a-169">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="53b9a-169">Vnet2Vnet</span></span>
* <span data-ttu-id="53b9a-170">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="53b9a-170">ExpressRoute</span></span>
* <span data-ttu-id="53b9a-171">VPNClient</span><span class="sxs-lookup"><span data-stu-id="53b9a-171">VPNClient</span></span>

#### <span data-ttu-id="53b9a-172"><a name="vpntype"></a>Om VPN-typer</span><span class="sxs-lookup"><span data-stu-id="53b9a-172"><a name="vpntype"></a>About VPN types</span></span>

<span data-ttu-id="53b9a-173">Varje konfigurationen kräver en viss typ av VPN.</span><span class="sxs-lookup"><span data-stu-id="53b9a-173">Each configuration requires a specific VPN type.</span></span> <span data-ttu-id="53b9a-174">Om du kombinerar två konfigurationer, till exempel om du skapar en plats-till-plats-anslutning och en punkt-till-plats-anslutning till samma VNet, måste du använda en VPN-typ som uppfyller båda anslutningskraven.</span><span class="sxs-lookup"><span data-stu-id="53b9a-174">If you are combining two configurations, such as creating a Site-to-Site connection and a Point-to-Site connection to the same VNet, you must use a VPN type that satisfies both connection requirements.</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="53b9a-175">Följande tabeller visar VPN-typ som mappas till varje anslutningskonfiguration av.</span><span class="sxs-lookup"><span data-stu-id="53b9a-175">The following tables show the VPN type as it maps to each connection configuration.</span></span> <span data-ttu-id="53b9a-176">Kontrollera att VPN-typ för din gateway matchar den konfiguration som du vill skapa.</span><span class="sxs-lookup"><span data-stu-id="53b9a-176">Make sure the VPN type for your gateway matches the configuration that you want to create.</span></span> 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <span data-ttu-id="53b9a-177"><a name="devices"></a>Enheter för VPN för plats-till-plats-anslutningar</span><span class="sxs-lookup"><span data-stu-id="53b9a-177"><a name="devices"></a>VPN devices for Site-to-Site connections</span></span>

<span data-ttu-id="53b9a-178">Om du vill konfigurera en plats-till-plats-anslutning, oavsett distributionsmodell, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="53b9a-178">To configure a Site-to-Site connection, regardless of deployment model, you need the following items:</span></span>

* <span data-ttu-id="53b9a-179">En VPN-enhet som är kompatibel med Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="53b9a-179">A VPN device that is compatible with Azure VPN gateways</span></span>
* <span data-ttu-id="53b9a-180">En offentlig IPv4-adress som inte finns bakom en NAT</span><span class="sxs-lookup"><span data-stu-id="53b9a-180">A public-facing IPv4 IP address that is not behind a NAT</span></span>

<span data-ttu-id="53b9a-181">Du måste ha erfarenhet att konfigurera din VPN-enhet eller ha någon som kan konfigurera enheten för dig.</span><span class="sxs-lookup"><span data-stu-id="53b9a-181">You need to have experience configuring your VPN device, or have someone that can configure the device for you.</span></span>

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <span data-ttu-id="53b9a-182"><a name="forcedtunnel"></a>Överväg att framtvingad tunnel Routning</span><span class="sxs-lookup"><span data-stu-id="53b9a-182"><a name="forcedtunnel"></a>Consider forced tunnel routing</span></span>

<span data-ttu-id="53b9a-183">Du kan konfigurera Tvingad tunneling för de flesta konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="53b9a-183">For most configurations, you can configure forced tunneling.</span></span> <span data-ttu-id="53b9a-184">Tvingad tunneling kan du omdirigera eller ”force” alla Internet-bunden trafik tillbaka till din lokala plats via en plats-till-plats VPN-tunnel för kontroll och granskning.</span><span class="sxs-lookup"><span data-stu-id="53b9a-184">Forced tunneling lets you redirect or "force" all Internet-bound traffic back to your on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="53b9a-185">Detta är en kritisk säkerhetskraven för de flesta företags IT principer.</span><span class="sxs-lookup"><span data-stu-id="53b9a-185">This is a critical security requirement for most enterprise IT policies.</span></span> 

<span data-ttu-id="53b9a-186">Utan Tvingad tunneling ska Internet-bunden trafik från din virtuella datorer i Azure alltid passerar från Azure nätverksinfrastruktur direkt ut till Internet, utan att du vill granska eller granska trafiken.</span><span class="sxs-lookup"><span data-stu-id="53b9a-186">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out to the Internet, without the option to allow you to inspect or audit the traffic.</span></span> <span data-ttu-id="53b9a-187">Obehörig Internetåtkomst kan leda till avslöjande av information och andra typer av säkerhetsintrång.</span><span class="sxs-lookup"><span data-stu-id="53b9a-187">Unauthorized Internet access can potentially lead to information disclosure or other types of security breaches.</span></span>

<span data-ttu-id="53b9a-188">En anslutning för Tvingad tunneltrafik kan konfigureras i båda distributionsmodellerna och med hjälp av olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="53b9a-188">A forced tunneling connection can be configured in both deployment models and by using different tools.</span></span> <span data-ttu-id="53b9a-189">Mer information finns i [konfigurera Tvingad tunneltrafik](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="53b9a-189">For more information, see [Configure forced tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>

<span data-ttu-id="53b9a-190">**Tvingad tunneltrafik diagram**</span><span class="sxs-lookup"><span data-stu-id="53b9a-190">**Forced tunneling diagram**</span></span>

![Azure VPN-Gateway Tvingad tunneltrafik diagram](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a><span data-ttu-id="53b9a-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="53b9a-192">Next steps</span></span>

<span data-ttu-id="53b9a-193">Finns det [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md) och [om VPN-Gateway](vpn-gateway-about-vpngateways.md) artiklar för mer information som hjälper dig med din design.</span><span class="sxs-lookup"><span data-stu-id="53b9a-193">See the [VPN Gateway FAQ](vpn-gateway-vpn-faq.md) and [About VPN Gateway](vpn-gateway-about-vpngateways.md) articles for more information to help you with your design.</span></span>

<span data-ttu-id="53b9a-194">Mer information om inställningar för specifika gatewayen finns [om VPN-Gateway inställningar](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="53b9a-194">For more information about specific gateway settings, see [About VPN Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>
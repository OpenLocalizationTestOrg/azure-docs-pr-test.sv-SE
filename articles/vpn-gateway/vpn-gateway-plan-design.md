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
ms.openlocfilehash: 3d4587ba31d163384212eca88a7e2c0ba8f3b21f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="planning-and-design-for-vpn-gateway"></a><span data-ttu-id="c2687-103">Planering och design för VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="c2687-103">Planning and design for VPN Gateway</span></span>

<span data-ttu-id="c2687-104">Planera och utforma din mellan platser och VNet-till-VNet-konfigurationer kan vara antingen enkla eller komplexa, beroende på dina nätverk behov.</span><span class="sxs-lookup"><span data-stu-id="c2687-104">Planning and designing your cross-premises and VNet-to-VNet configurations can be either simple, or complicated, depending on your networking needs.</span></span> <span data-ttu-id="c2687-105">Den här artikeln vägleder dig genom grundläggande överväganden för planering och design.</span><span class="sxs-lookup"><span data-stu-id="c2687-105">This article walks you through basic planning and design considerations.</span></span>

## <span data-ttu-id="c2687-106"><a name="planning"></a>Planera</span><span class="sxs-lookup"><span data-stu-id="c2687-106"><a name="planning"></a>Planning</span></span>

### <span data-ttu-id="c2687-107"><a name="compare"></a>Alternativ för nätverksanslutning mellan platser</span><span class="sxs-lookup"><span data-stu-id="c2687-107"><a name="compare"></a>Cross-premises connectivity options</span></span>

<span data-ttu-id="c2687-108">Om du vill tooconnect dina lokala platser på ett säkert sätt tooa virtuellt nätverk, du har tre olika sätt toodo så: plats-till-plats, punkt-till-plats och ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="c2687-108">If you want tooconnect your on-premises sites securely tooa virtual network, you have three different ways toodo so: Site-to-Site, Point-to-Site, and ExpressRoute.</span></span> <span data-ttu-id="c2687-109">Jämför hello olika anslutningar mellan platser som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="c2687-109">Compare hello different cross-premises connections that are available.</span></span> <span data-ttu-id="c2687-110">hello alternativ du väljer kan bero på olika överväganden som:</span><span class="sxs-lookup"><span data-stu-id="c2687-110">hello option you choose can depend on various considerations, such as:</span></span>

* <span data-ttu-id="c2687-111">Vilken typ av dataflöde kräver din lösning?</span><span class="sxs-lookup"><span data-stu-id="c2687-111">What kind of throughput does your solution require?</span></span>
* <span data-ttu-id="c2687-112">Vill du toocommunicate över hello offentliga Internet via en säker VPN eller över en privat anslutning?</span><span class="sxs-lookup"><span data-stu-id="c2687-112">Do you want toocommunicate over hello public Internet via secure VPN, or over a private connection?</span></span>
* <span data-ttu-id="c2687-113">Har du en offentlig IP-adress tillgängliga toouse?</span><span class="sxs-lookup"><span data-stu-id="c2687-113">Do you have a public IP address available toouse?</span></span>
* <span data-ttu-id="c2687-114">Planerar du toouse en VPN-enhet?</span><span class="sxs-lookup"><span data-stu-id="c2687-114">Are you planning toouse a VPN device?</span></span> <span data-ttu-id="c2687-115">I så fall, är den kompatibel?</span><span class="sxs-lookup"><span data-stu-id="c2687-115">If so, is it compatible?</span></span>
* <span data-ttu-id="c2687-116">Ansluter du bara några få datorer eller vill du ha en permanent anslutning för platsen?</span><span class="sxs-lookup"><span data-stu-id="c2687-116">Are you connecting just a few computers, or do you want a persistent connection for your site?</span></span>
* <span data-ttu-id="c2687-117">Vilken typ av VPN-gateway krävs för hello lösningen som du vill toocreate?</span><span class="sxs-lookup"><span data-stu-id="c2687-117">What type of VPN gateway is required for hello solution you want toocreate?</span></span>
* <span data-ttu-id="c2687-118">Vilken gateway SKU bör du använda?</span><span class="sxs-lookup"><span data-stu-id="c2687-118">Which gateway SKU should you use?</span></span>

### <span data-ttu-id="c2687-119"><a name="planningtable"></a>Planera tabell</span><span class="sxs-lookup"><span data-stu-id="c2687-119"><a name="planningtable"></a>Planning table</span></span>

<span data-ttu-id="c2687-120">hello i den följande tabellen kan hjälpa dig avgöra hello bästa anslutningsmöjlighet för lösningen.</span><span class="sxs-lookup"><span data-stu-id="c2687-120">hello following table can help you decide hello best connectivity option for your solution.</span></span>

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <span data-ttu-id="c2687-121"><a name="gwsku"></a>Gateway-SKU:er</span><span class="sxs-lookup"><span data-stu-id="c2687-121"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <span data-ttu-id="c2687-122"><a name="wf"></a>Arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="c2687-122"><a name="wf"></a>Workflow</span></span>

<span data-ttu-id="c2687-123">hello hello följande lista beskrivs vanliga arbetsflöde för moln-anslutningen:</span><span class="sxs-lookup"><span data-stu-id="c2687-123">hello following list outlines hello common workflow for cloud connectivity:</span></span>

1. <span data-ttu-id="c2687-124">Utforma och planera din anslutning topologi och lista hello adressutrymmen för alla nätverk du vill tooconnect.</span><span class="sxs-lookup"><span data-stu-id="c2687-124">Design and plan your connectivity topology and list hello address spaces for all networks you want tooconnect.</span></span>
2. <span data-ttu-id="c2687-125">Skapa ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="c2687-125">Create an Azure virtual network.</span></span> 
3. <span data-ttu-id="c2687-126">Skapa en VPN-gateway för hello virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="c2687-126">Create a VPN gateway for hello virtual network.</span></span>
4. <span data-ttu-id="c2687-127">Skapa och konfigurera anslutningar tooon lokala nätverk eller andra virtuella nätverk (vid behov).</span><span class="sxs-lookup"><span data-stu-id="c2687-127">Create and configure connections tooon-premises networks or other virtual networks (as needed).</span></span>
5. <span data-ttu-id="c2687-128">Skapa och konfigurera en punkt-till-plats-anslutning för din Azure VPN-gateway (vid behov).</span><span class="sxs-lookup"><span data-stu-id="c2687-128">Create and configure a Point-to-Site connection for your Azure VPN gateway (as needed).</span></span>

## <span data-ttu-id="c2687-129"><a name="design"></a>Design</span><span class="sxs-lookup"><span data-stu-id="c2687-129"><a name="design"></a>Design</span></span>
### <span data-ttu-id="c2687-130"><a name="topologies"></a>Topologier för anslutning</span><span class="sxs-lookup"><span data-stu-id="c2687-130"><a name="topologies"></a>Connection topologies</span></span>

<span data-ttu-id="c2687-131">Starta genom att titta på hello diagrammen i hello [om VPN-Gateway](vpn-gateway-about-vpngateways.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c2687-131">Start by looking at hello diagrams in hello [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span> <span data-ttu-id="c2687-132">hello artikeln innehåller vanliga diagram, hello distributionsmodeller för varje topologi och hello tillgänglig distributionsverktyg som du kan använda toodeploy din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c2687-132">hello article contains basic diagrams, hello deployment models for each topology, and hello available deployment tools you can use toodeploy your configuration.</span></span>

### <span data-ttu-id="c2687-133"><a name="designbasics"></a>Design-grunderna</span><span class="sxs-lookup"><span data-stu-id="c2687-133"><a name="designbasics"></a>Design basics</span></span>

<span data-ttu-id="c2687-134">hello följande avsnitt beskrivs grunderna för hello VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="c2687-134">hello following sections discuss hello VPN gateway basics.</span></span> 

#### <span data-ttu-id="c2687-135"><a name="servicelimits"></a>Nätverk tjänster gränser</span><span class="sxs-lookup"><span data-stu-id="c2687-135"><a name="servicelimits"></a>Networking services limits</span></span>

<span data-ttu-id="c2687-136">Rulla hello tabeller tooview [nätverk services gränser](../azure-subscription-service-limits.md#networking-limits).</span><span class="sxs-lookup"><span data-stu-id="c2687-136">Scroll through hello tables tooview [networking services limits](../azure-subscription-service-limits.md#networking-limits).</span></span> <span data-ttu-id="c2687-137">hello begränsningarna kan påverka din design.</span><span class="sxs-lookup"><span data-stu-id="c2687-137">hello limits listed may impact your design.</span></span>

#### <span data-ttu-id="c2687-138"><a name="subnets"></a>Om undernät</span><span class="sxs-lookup"><span data-stu-id="c2687-138"><a name="subnets"></a>About subnets</span></span>

<span data-ttu-id="c2687-139">När du skapar anslutningar måste du undernät-intervall.</span><span class="sxs-lookup"><span data-stu-id="c2687-139">When you are creating connections, you must consider your subnet ranges.</span></span> <span data-ttu-id="c2687-140">Du kan inte ha överlappande undernät-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="c2687-140">You cannot have overlapping subnet address ranges.</span></span> <span data-ttu-id="c2687-141">Ett annat överlappande undernät är när en virtuell nätverks- eller lokal plats innehåller hello innehåller samma adressutrymme som hello annan plats.</span><span class="sxs-lookup"><span data-stu-id="c2687-141">An overlapping subnet is when one virtual network or on-premises location contains hello same address space that hello other location contains.</span></span> <span data-ttu-id="c2687-142">Det innebär att du måste nätverk-tekniker för din lokala lokalt nätverk toocarve ut ett intervall för du toouse för din Azure-IP-adressering utrymme-undernät.</span><span class="sxs-lookup"><span data-stu-id="c2687-142">This means that you need your network engineers for your local on-premises networks toocarve out a range for you toouse for your Azure IP addressing space/subnets.</span></span> <span data-ttu-id="c2687-143">Du måste adressutrymme som inte används på hello lokala lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="c2687-143">You need address space that is not being used on hello local on-premises network.</span></span>

<span data-ttu-id="c2687-144">Undvika överlappande undernät är också viktigt när du arbetar med VNet-till-VNet-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="c2687-144">Avoiding overlapping subnets is also important when you are working with VNet-to-VNet connections.</span></span> <span data-ttu-id="c2687-145">Om dina undernät överlappar varandra och en IP-adress finns i både skicka hello- och målservrar Vnet, misslyckas VNet-till-VNet-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="c2687-145">If your subnets overlap and an IP address exists in both hello sending and destination VNets, VNet-to-VNet connections fail.</span></span> <span data-ttu-id="c2687-146">Azure kan inte vidarebefordra hello data toohello andra VNet eftersom hello måladress är en del av hello skicka VNet.</span><span class="sxs-lookup"><span data-stu-id="c2687-146">Azure can't route hello data toohello other VNet because hello destination address is part of hello sending VNet.</span></span>

<span data-ttu-id="c2687-147">VPN-gatewayer kräver ett specifikt undernät som kallas en gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="c2687-147">VPN Gateways require a specific subnet called a gateway subnet.</span></span> <span data-ttu-id="c2687-148">Alla gateway-undernät måste ha namnet GatewaySubnet toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="c2687-148">All gateway subnets must be named GatewaySubnet toowork properly.</span></span> <span data-ttu-id="c2687-149">Glöm inte tooname din gateway-undernät med ett annat namn och inte distribuera virtuella datorer eller något annat toohello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="c2687-149">Be sure not tooname your gateway subnet a different name, and don't deploy VMs or anything else toohello gateway subnet.</span></span> <span data-ttu-id="c2687-150">Se [Gateway-undernät](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span><span class="sxs-lookup"><span data-stu-id="c2687-150">See [Gateway Subnets](vpn-gateway-about-vpn-gateway-settings.md#gwsub).</span></span>

#### <span data-ttu-id="c2687-151"><a name="local"></a>Om lokala nätverksgatewayer</span><span class="sxs-lookup"><span data-stu-id="c2687-151"><a name="local"></a>About local network gateways</span></span>

<span data-ttu-id="c2687-152">hello lokal nätverksgateway refererar vanligtvis tooyour lokal plats.</span><span class="sxs-lookup"><span data-stu-id="c2687-152">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="c2687-153">Hello lokal nätverksgateway är refererad tooas en lokal nätverksplats i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="c2687-153">In hello classic deployment model, hello local network gateway is referred tooas a Local Network Site.</span></span> <span data-ttu-id="c2687-154">När du konfigurerar en lokal nätverksgateway kan du ge det ett namn Ange hello offentliga IP-adressen för hello lokala VPN-enhet och ange hello adressprefix i hello lokal plats.</span><span class="sxs-lookup"><span data-stu-id="c2687-154">When you configure a local network gateway, you give it a name, specify hello public IP address of hello on-premises VPN device, and specify hello address prefixes that are in hello on-premises location.</span></span> <span data-ttu-id="c2687-155">Azure tittar på hello mål-adressprefix för nätverkstrafik, tittar hello-konfiguration som du har angett för hello lokal nätverksgateway och därefter dirigerar paket.</span><span class="sxs-lookup"><span data-stu-id="c2687-155">Azure looks at hello destination address prefixes for network traffic, consults hello configuration that you have specified for hello local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="c2687-156">Du kan ändra hello adressprefix efter behov.</span><span class="sxs-lookup"><span data-stu-id="c2687-156">You can modify hello address prefixes as needed.</span></span> <span data-ttu-id="c2687-157">Mer information finns i [lokala nätverksgatewayer](vpn-gateway-about-vpn-gateway-settings.md#lng).</span><span class="sxs-lookup"><span data-stu-id="c2687-157">For more information, see [Local network gateways](vpn-gateway-about-vpn-gateway-settings.md#lng).</span></span>

#### <span data-ttu-id="c2687-158"><a name="gwtype"></a>Om gateway-typer</span><span class="sxs-lookup"><span data-stu-id="c2687-158"><a name="gwtype"></a>About gateway types</span></span>

<span data-ttu-id="c2687-159">Det är viktigt att välja hello rätt gateway-typ för din topologi.</span><span class="sxs-lookup"><span data-stu-id="c2687-159">Selecting hello correct gateway type for your topology is critical.</span></span> <span data-ttu-id="c2687-160">Om du väljer hello fel typ fungerar din gateway inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="c2687-160">If you select hello wrong type, your gateway won't work properly.</span></span> <span data-ttu-id="c2687-161">hello gateway-typen anger hur själva hello gatewayen ansluter och är en konfigurationsinställning som krävs för hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="c2687-161">hello gateway type specifies how hello gateway itself connects and is a required configuration setting for hello Resource Manager deployment model.</span></span>

<span data-ttu-id="c2687-162">hello gateway typer är:</span><span class="sxs-lookup"><span data-stu-id="c2687-162">hello gateway types are:</span></span>

* <span data-ttu-id="c2687-163">Vpn</span><span class="sxs-lookup"><span data-stu-id="c2687-163">Vpn</span></span>
* <span data-ttu-id="c2687-164">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="c2687-164">ExpressRoute</span></span>

#### <span data-ttu-id="c2687-165"><a name="connectiontype"></a>Om anslutningstyper</span><span class="sxs-lookup"><span data-stu-id="c2687-165"><a name="connectiontype"></a>About connection types</span></span>

<span data-ttu-id="c2687-166">Varje konfiguration kräver en viss anslutningstyp.</span><span class="sxs-lookup"><span data-stu-id="c2687-166">Each configuration requires a specific connection type.</span></span> <span data-ttu-id="c2687-167">hello anslutningstyper är:</span><span class="sxs-lookup"><span data-stu-id="c2687-167">hello connection types are:</span></span>

* <span data-ttu-id="c2687-168">IPsec</span><span class="sxs-lookup"><span data-stu-id="c2687-168">IPsec</span></span>
* <span data-ttu-id="c2687-169">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="c2687-169">Vnet2Vnet</span></span>
* <span data-ttu-id="c2687-170">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="c2687-170">ExpressRoute</span></span>
* <span data-ttu-id="c2687-171">VPNClient</span><span class="sxs-lookup"><span data-stu-id="c2687-171">VPNClient</span></span>

#### <span data-ttu-id="c2687-172"><a name="vpntype"></a>Om VPN-typer</span><span class="sxs-lookup"><span data-stu-id="c2687-172"><a name="vpntype"></a>About VPN types</span></span>

<span data-ttu-id="c2687-173">Varje konfigurationen kräver en viss typ av VPN.</span><span class="sxs-lookup"><span data-stu-id="c2687-173">Each configuration requires a specific VPN type.</span></span> <span data-ttu-id="c2687-174">Om du kombinerar två konfigurationer, till exempel skapa en plats-till-plats-anslutning och en punkt-till-plats-anslutning toohello samma virtuella nätverk, måste du använda en VPN-typ som uppfyller båda anslutning krav.</span><span class="sxs-lookup"><span data-stu-id="c2687-174">If you are combining two configurations, such as creating a Site-to-Site connection and a Point-to-Site connection toohello same VNet, you must use a VPN type that satisfies both connection requirements.</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="c2687-175">hello följande tabeller visar hello VPN-typ som mappas tooeach anslutningskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="c2687-175">hello following tables show hello VPN type as it maps tooeach connection configuration.</span></span> <span data-ttu-id="c2687-176">Se till att hello VPN-typ för din gateway matchar hello-konfiguration som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="c2687-176">Make sure hello VPN type for your gateway matches hello configuration that you want toocreate.</span></span> 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <span data-ttu-id="c2687-177"><a name="devices"></a>Enheter för VPN för plats-till-plats-anslutningar</span><span class="sxs-lookup"><span data-stu-id="c2687-177"><a name="devices"></a>VPN devices for Site-to-Site connections</span></span>

<span data-ttu-id="c2687-178">tooconfigure plats-till-plats-anslutning, oavsett distributionsmodell, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c2687-178">tooconfigure a Site-to-Site connection, regardless of deployment model, you need hello following items:</span></span>

* <span data-ttu-id="c2687-179">En VPN-enhet som är kompatibel med Azure VPN-gatewayer</span><span class="sxs-lookup"><span data-stu-id="c2687-179">A VPN device that is compatible with Azure VPN gateways</span></span>
* <span data-ttu-id="c2687-180">En offentlig IPv4-adress som inte finns bakom en NAT</span><span class="sxs-lookup"><span data-stu-id="c2687-180">A public-facing IPv4 IP address that is not behind a NAT</span></span>

<span data-ttu-id="c2687-181">Du behöver toohave erfarenhet att konfigurera din VPN-enhet eller har någon som du kan konfigurera hello enhet för dig.</span><span class="sxs-lookup"><span data-stu-id="c2687-181">You need toohave experience configuring your VPN device, or have someone that can configure hello device for you.</span></span>

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <span data-ttu-id="c2687-182"><a name="forcedtunnel"></a>Överväg att framtvingad tunnel Routning</span><span class="sxs-lookup"><span data-stu-id="c2687-182"><a name="forcedtunnel"></a>Consider forced tunnel routing</span></span>

<span data-ttu-id="c2687-183">Du kan konfigurera Tvingad tunneling för de flesta konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="c2687-183">For most configurations, you can configure forced tunneling.</span></span> <span data-ttu-id="c2687-184">Tvingad tunneltrafik kan du omdirigera eller ”force” alla Internet-bunden trafik tillbaka tooyour lokal plats via en plats-till-plats VPN-tunnel för kontroll och granskning.</span><span class="sxs-lookup"><span data-stu-id="c2687-184">Forced tunneling lets you redirect or "force" all Internet-bound traffic back tooyour on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="c2687-185">Detta är en kritisk säkerhetskraven för de flesta företags IT principer.</span><span class="sxs-lookup"><span data-stu-id="c2687-185">This is a critical security requirement for most enterprise IT policies.</span></span> 

<span data-ttu-id="c2687-186">Utan Tvingad tunneling ska Internet-bunden trafik från din virtuella datorer i Azure alltid passerar från Azure nätverksinfrastruktur direkt ut toohello Internet, utan hello alternativet tooallow du tooinspect eller audit hello-trafik.</span><span class="sxs-lookup"><span data-stu-id="c2687-186">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out toohello Internet, without hello option tooallow you tooinspect or audit hello traffic.</span></span> <span data-ttu-id="c2687-187">Obehörig Internetåtkomst kan leda tooinformation avslöjas eller andra typer av säkerhetsintrång.</span><span class="sxs-lookup"><span data-stu-id="c2687-187">Unauthorized Internet access can potentially lead tooinformation disclosure or other types of security breaches.</span></span>

<span data-ttu-id="c2687-188">En anslutning för Tvingad tunneltrafik kan konfigureras i båda distributionsmodellerna och med hjälp av olika verktyg.</span><span class="sxs-lookup"><span data-stu-id="c2687-188">A forced tunneling connection can be configured in both deployment models and by using different tools.</span></span> <span data-ttu-id="c2687-189">Mer information finns i [konfigurera Tvingad tunneltrafik](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="c2687-189">For more information, see [Configure forced tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>

<span data-ttu-id="c2687-190">**Tvingad tunneltrafik diagram**</span><span class="sxs-lookup"><span data-stu-id="c2687-190">**Forced tunneling diagram**</span></span>

![Azure VPN-Gateway Tvingad tunneltrafik diagram](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a><span data-ttu-id="c2687-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c2687-192">Next steps</span></span>

<span data-ttu-id="c2687-193">Se hello [VPN-Gateway FAQ](vpn-gateway-vpn-faq.md) och [om VPN-Gateway](vpn-gateway-about-vpngateways.md) artiklar för mer information toohelp du med din design.</span><span class="sxs-lookup"><span data-stu-id="c2687-193">See hello [VPN Gateway FAQ](vpn-gateway-vpn-faq.md) and [About VPN Gateway](vpn-gateway-about-vpngateways.md) articles for more information toohelp you with your design.</span></span>

<span data-ttu-id="c2687-194">Mer information om inställningar för specifika gatewayen finns [om VPN-Gateway inställningar](vpn-gateway-about-vpn-gateway-settings.md).</span><span class="sxs-lookup"><span data-stu-id="c2687-194">For more information about specific gateway settings, see [About VPN Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).</span></span>
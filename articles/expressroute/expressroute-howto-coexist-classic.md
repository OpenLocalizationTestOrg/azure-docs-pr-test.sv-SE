---
title: "Konfigurera ExpressRoute-anslutningar och VPN-anslutningar för plats-till-plats som kan samexistera: Azure (klassisk) | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar ExpressRoute- och en plats-till-plats VPN-anslutning som kan finnas för hello klassiska distributionsmodellen."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: dcf1a5af-a289-466a-b812-0bfedbd2bda0
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: charwen
ms.openlocfilehash: abb30fff55e8ec243f2920c5b2f70c43717755fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-classic"></a><span data-ttu-id="729c4-103">Konfigurera ExpressRoute-anslutningar och VPN-anslutningar för plats-till-plats som kan samexistera (klassisk)</span><span class="sxs-lookup"><span data-stu-id="729c4-103">Configure ExpressRoute and Site-to-Site coexisting connections (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="729c4-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="729c4-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="729c4-105">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="729c4-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="729c4-106">Med hello möjlighet tooconfigure plats-till-plats VPN- och ExpressRoute har flera fördelar.</span><span class="sxs-lookup"><span data-stu-id="729c4-106">Having hello ability tooconfigure Site-to-Site VPN and ExpressRoute has several advantages.</span></span> <span data-ttu-id="729c4-107">Du kan konfigurera VPN för plats-till-plats som en säker redundans sökväg för ExressRoute eller använda plats-till-plats VPN tooconnect toosites som inte är anslutna via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="729c4-107">You can configure Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs tooconnect toosites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="729c4-108">Vi tar upp hello steg tooconfigure båda scenarierna i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="729c4-108">We will cover hello steps tooconfigure both scenarios in this article.</span></span> <span data-ttu-id="729c4-109">Den här artikeln gäller toohello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="729c4-109">This article applies toohello classic deployment model.</span></span> <span data-ttu-id="729c4-110">Den här konfigurationen är inte tillgänglig i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="729c4-110">This configuration is not available in hello portal.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="729c4-111">**Om distributionsmodeller för Azure**</span><span class="sxs-lookup"><span data-stu-id="729c4-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="729c4-112">ExpressRoute-kretsar måste konfigureras före innan du följer hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="729c4-112">ExpressRoute circuits must be pre-configured before you follow hello instructions below.</span></span> <span data-ttu-id="729c4-113">Kontrollera att du har följt hello guider för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-classic.md) och [konfigurera routning](expressroute-howto-routing-classic.md) innan du följer hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="729c4-113">Make sure that you have followed hello guides too[create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and [configure routing](expressroute-howto-routing-classic.md) before you follow hello steps below.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="729c4-114">Gränser och begränsningar</span><span class="sxs-lookup"><span data-stu-id="729c4-114">Limits and limitations</span></span>
* <span data-ttu-id="729c4-115">**Transit-routing stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="729c4-115">**Transit routing is not supported.**</span></span> <span data-ttu-id="729c4-116">Du kan inte routa (via Azure) mellan ditt lokala nätverk som ansluter via plats-till-plats-VPN och ditt lokala nätverk som ansluter via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="729c4-116">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="729c4-117">**Punkt-till-plats stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="729c4-117">**Point-to-site is not supported.**</span></span> <span data-ttu-id="729c4-118">Du kan inte aktivera punkt-till-plats VPN-anslutningar toohello samma virtuella nätverk som är anslutna tooExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="729c4-118">You can't enable point-to-site VPN connections toohello same VNet that is connected tooExpressRoute.</span></span> <span data-ttu-id="729c4-119">Punkt-till-plats VPN- och ExpressRoute kan inte samexistera för hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="729c4-119">Point-to-site VPN and ExpressRoute cannot coexist for hello same VNet.</span></span>
* <span data-ttu-id="729c4-120">**Tvingad tunneling kan inte aktiveras på hello plats-till-plats VPN-gateway.**</span><span class="sxs-lookup"><span data-stu-id="729c4-120">**Forced tunneling cannot be enabled on hello Site-to-Site VPN gateway.**</span></span> <span data-ttu-id="729c4-121">Du kan bara ”framtvinga” alla Internet-bunden trafik tillbaka tooyour lokalt nätverk via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="729c4-121">You can only "force" all Internet-bound traffic back tooyour on-premises network via ExpressRoute.</span></span>
* <span data-ttu-id="729c4-122">**Basic-SKU-gatewayen stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="729c4-122">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="729c4-123">Du måste använda en icke - grundläggande SKU-gateway för båda hello [ExpressRoute-gateway](expressroute-about-virtual-network-gateways.md) och hello [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="729c4-123">You must use a non-Basic SKU gateway for both hello [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and hello [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="729c4-124">**Enbart routebaserad VPN-gateway stöds.**</span><span class="sxs-lookup"><span data-stu-id="729c4-124">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="729c4-125">Du måste använda en routebaserad [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="729c4-125">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="729c4-126">**Statiska vägar ska konfigureras för din VPN-gateway.**</span><span class="sxs-lookup"><span data-stu-id="729c4-126">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="729c4-127">Om nätverket är anslutet tooboth ExpressRoute och en plats-till-plats VPN, måste du ha en statisk väg som konfigurerats i ditt lokala nätverk tooroute hello plats-till-plats VPN-anslutningen toohello offentliga Internet.</span><span class="sxs-lookup"><span data-stu-id="729c4-127">If your local network is connected tooboth ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network tooroute hello Site-to-Site VPN connection toohello public Internet.</span></span>
* <span data-ttu-id="729c4-128">**ExpressRoute-gatewayen måste konfigureras först.**</span><span class="sxs-lookup"><span data-stu-id="729c4-128">**ExpressRoute gateway must be configured first.**</span></span> <span data-ttu-id="729c4-129">Du måste först skapa hello ExpressRoute-gateway innan du lägger till hello plats-till-plats VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="729c4-129">You must create hello ExpressRoute gateway first before you add hello Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="729c4-130">Konfigurationsdesign</span><span class="sxs-lookup"><span data-stu-id="729c4-130">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="729c4-131">Konfigurera en VPN för plats till plats som en redundanssökväg för ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="729c4-131">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="729c4-132">Du kan konfigurera en VPN-anslutning för plats till plats som en säkerhetskopia av ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="729c4-132">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="729c4-133">Detta gäller endast toovirtual nätverk länkade toohello Azure privat peering sökväg.</span><span class="sxs-lookup"><span data-stu-id="729c4-133">This applies only toovirtual networks linked toohello Azure private peering path.</span></span> <span data-ttu-id="729c4-134">Det finns ingen VPN-baserad redundanslösning för tjänster som är tillgängliga via Azures offentliga och Microsofts peerings.</span><span class="sxs-lookup"><span data-stu-id="729c4-134">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="729c4-135">Hej ExpressRoute-kretsen är alltid hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="729c4-135">hello ExpressRoute circuit is always hello primary link.</span></span> <span data-ttu-id="729c4-136">Data flödar genom hello plats-till-plats VPN-väg endast om hello ExpressRoute-krets misslyckades.</span><span class="sxs-lookup"><span data-stu-id="729c4-136">Data will flow through hello Site-to-Site VPN path only if hello ExpressRoute circuit fails.</span></span> 

> [!NOTE]
> <span data-ttu-id="729c4-137">ExpressRoute-kretsen är prioriterade via plats-till-plats VPN när båda vägar är hello samma, använder Azure hello längsta prefix matchar toochoose hello vägen mot hello paketets mål.</span><span class="sxs-lookup"><span data-stu-id="729c4-137">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are hello same, Azure will use hello longest prefix match toochoose hello route towards hello packet's destination.</span></span>
> 
> 

![Samexistera](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a><span data-ttu-id="729c4-139">Konfigurera en plats-till-plats VPN-tooconnect toosites som inte är anslutna via ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="729c4-139">Configure a Site-to-Site VPN tooconnect toosites not connected through ExpressRoute</span></span>
<span data-ttu-id="729c4-140">Du kan konfigurera vissa platser ansluter direkt tooAzure via plats-till-plats VPN och vissa platser ansluter via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="729c4-140">You can configure your network where some sites connect directly tooAzure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Samexistera](media/expressroute-howto-coexist-classic/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="729c4-142">Det går inte att konfigurera ett virtuellt nätverk som en överföringsrouter.</span><span class="sxs-lookup"><span data-stu-id="729c4-142">You cannot a configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-hello-steps-toouse"></a><span data-ttu-id="729c4-143">Att välja hello steg toouse</span><span class="sxs-lookup"><span data-stu-id="729c4-143">Selecting hello steps toouse</span></span>
<span data-ttu-id="729c4-144">Det finns två olika uppsättningar av procedurer toochoose från i ordning tooconfigure anslutningar som kan finnas.</span><span class="sxs-lookup"><span data-stu-id="729c4-144">There are two different sets of procedures toochoose from in order tooconfigure connections that can coexist.</span></span> <span data-ttu-id="729c4-145">hello configuration procedur som du väljer beror på om du har ett befintligt virtuellt nätverk som du vill tooconnect till, eller önskade toocreate ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="729c4-145">hello configuration procedure that you select will depend on whether you have an existing virtual network that you want tooconnect to, or you want toocreate a new virtual network.</span></span>

* <span data-ttu-id="729c4-146">Jag inte har ett VNet och behöver toocreate en.</span><span class="sxs-lookup"><span data-stu-id="729c4-146">I don't have a VNet and need toocreate one.</span></span>
  
    <span data-ttu-id="729c4-147">Om du inte redan har ett virtuellt nätverk, får den här proceduren du att skapa ett nytt virtuellt nätverk med hjälp av hello klassiska distributionsmodellen och skapa nya ExpressRoute- och plats-till-plats VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="729c4-147">If you don’t already have a virtual network, this procedure will walk you through creating a new virtual network using hello classic deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="729c4-148">tooconfigure, följ hello instruktionerna i artikeln hello [toocreate ett nytt virtuellt nätverk och samtidiga anslutningar](#new).</span><span class="sxs-lookup"><span data-stu-id="729c4-148">tooconfigure, follow hello steps in hello article section [toocreate a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="729c4-149">Jag har redan en klassisk distributionsmodell för VNet.</span><span class="sxs-lookup"><span data-stu-id="729c4-149">I already have a classic deployment model VNet.</span></span>
  
    <span data-ttu-id="729c4-150">Du kanske redan har ett virtuellt nätverk på plats med en befintlig VPN-anslutning för plats till plats eller en ExpressRoute-anslutning.</span><span class="sxs-lookup"><span data-stu-id="729c4-150">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="729c4-151">Hej artikel avsnittet [tooconfigure coexsiting anslutningar för ett redan existerande VNet](#add) beskriver hur du tar bort hello gateway och sedan skapa nya ExpressRoute och plats-till-plats VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="729c4-151">hello article section [tooconfigure coexsiting connections for an already existing VNet](#add) will walk you through deleting hello gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="729c4-152">Observera att när du skapar hello nya anslutningar, hello steg måste slutföras i en särskild ordning.</span><span class="sxs-lookup"><span data-stu-id="729c4-152">Note that when creating hello new connections, hello steps must be completed in a very specific order.</span></span> <span data-ttu-id="729c4-153">Använd inte hello instruktionerna i andra artiklar toocreate din gateway och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="729c4-153">Don't use hello instructions in other articles toocreate your gateways and connections.</span></span>
  
    <span data-ttu-id="729c4-154">I den här proceduren skapar anslutningar som kan finnas kräver du toodelete din gateway, och sedan konfigurera ny gateway.</span><span class="sxs-lookup"><span data-stu-id="729c4-154">In this procedure, creating connections that can coexist will require you toodelete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="729c4-155">Detta innebär att du har driftstopp för anslutningar mellan platser när du tar bort och återskapa din gateway och anslutningar, men du behöver inte toomigrate någon av dina virtuella datorer eller tjänster tooa nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="729c4-155">This means you will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need toomigrate any of your VMs or services tooa new virtual network.</span></span> <span data-ttu-id="729c4-156">Dina virtuella datorer och tjänster kommer fortfarande att kunna toocommunicate ut via hello belastningsutjämnare när du konfigurerar din gateway om de är konfigurerade toodo så.</span><span class="sxs-lookup"><span data-stu-id="729c4-156">Your VMs and services will still be able toocommunicate out through hello load balancer while you configure your gateway if they are configured toodo so.</span></span>

## <span data-ttu-id="729c4-157"><a name="new"></a>toocreate ett nytt virtuellt nätverk och samtidiga anslutningar</span><span class="sxs-lookup"><span data-stu-id="729c4-157"><a name="new"></a>toocreate a new virtual network and coexisting connections</span></span>
<span data-ttu-id="729c4-158">Den här proceduren vägleder dig genom att skapa ett VNet samt plats-till-plats- och ExpressRoute-anslutningar som ska finnas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="729c4-158">This procedure will walk you through creating a VNet and create Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="729c4-159">Du behöver tooinstall hello senaste versionen av hello Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="729c4-159">You'll need tooinstall hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="729c4-160">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="729c4-160">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span> <span data-ttu-id="729c4-161">Observera att hello-cmdlets som du vill använda för den här konfigurationen kan vara något annorlunda än vad du kan känna till.</span><span class="sxs-lookup"><span data-stu-id="729c4-161">Note that hello cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="729c4-162">Anges till toouse hello-cmdlets i dessa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="729c4-162">Be sure toouse hello cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="729c4-163">Skapa ett schema för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="729c4-163">Create a schema for your virtual network.</span></span> <span data-ttu-id="729c4-164">Läs mer om hello Konfigurationsschemat [Azure Virtual Network Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="729c4-164">For more information about hello configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   
    <span data-ttu-id="729c4-165">När du skapar schemat måste du kontrollera att du använder hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="729c4-165">When you create your schema, make sure you use hello following values:</span></span>
   
   * <span data-ttu-id="729c4-166">hello gateway-undernät för virtuellt nätverk för hello måste vara minst/27 eller ett kortare prefix (till exempel /26 eller /25).</span><span class="sxs-lookup"><span data-stu-id="729c4-166">hello gateway subnet for hello virtual network must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   * <span data-ttu-id="729c4-167">anslutningstypen för hello gateway dedikerad ””.</span><span class="sxs-lookup"><span data-stu-id="729c4-167">hello gateway connection type is "Dedicated".</span></span>
     
             <VirtualNetworkSite name="MyAzureVNET" Location="Central US">
               <AddressSpace>
                 <AddressPrefix>10.17.159.192/26</AddressPrefix>
               </AddressSpace>
               <Subnets>
                 <Subnet name="Subnet-1">
                   <AddressPrefix>10.17.159.192/27</AddressPrefix>
                 </Subnet>
                 <Subnet name="GatewaySubnet">
                   <AddressPrefix>10.17.159.224/27</AddressPrefix>
                 </Subnet>
               </Subnets>
               <Gateway>
                 <ConnectionsToLocalNetwork>
                   <LocalNetworkSiteRef name="MyLocalNetwork">
                     <Connection type="Dedicated" />
                   </LocalNetworkSiteRef>
                 </ConnectionsToLocalNetwork>
               </Gateway>
             </VirtualNetworkSite>
3. <span data-ttu-id="729c4-168">När du skapar och konfigurerar xml-schemafil, överför du hello-fil.</span><span class="sxs-lookup"><span data-stu-id="729c4-168">After creating and configuring your xml schema file, upload hello file.</span></span> <span data-ttu-id="729c4-169">Detta skapar det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="729c4-169">This will create your virtual network.</span></span>
   
    <span data-ttu-id="729c4-170">Använd följande cmdlet tooupload hello din fil ersätter hello värdet med dina egna.</span><span class="sxs-lookup"><span data-stu-id="729c4-170">Use hello following cmdlet tooupload your file, replacing hello value with your own.</span></span>
   
        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'
4. <span data-ttu-id="729c4-171"><a name="gw"></a>Skapa en ExpressRoute-gateway.</span><span class="sxs-lookup"><span data-stu-id="729c4-171"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="729c4-172">Vara säker på att toospecify hello GatewaySKU som *Standard*, *HighPerformance*, eller *UltraPerformance* och hello GatewayType som *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="729c4-172">Be sure toospecify hello GatewaySKU as *Standard*, *HighPerformance*, or *UltraPerformance* and hello GatewayType as *DynamicRouting*.</span></span>
   
    <span data-ttu-id="729c4-173">Använd hello följande exempel, ersätter hello värden för din egen.</span><span class="sxs-lookup"><span data-stu-id="729c4-173">Use hello following sample, substituting hello values for your own.</span></span>
   
        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance
5. <span data-ttu-id="729c4-174">Länka hello ExpressRoute-gateway toohello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="729c4-174">Link hello ExpressRoute gateway toohello ExpressRoute circuit.</span></span> <span data-ttu-id="729c4-175">Det här steget har slutförts, upprättas hello anslutning mellan ditt lokala nätverk och Azure via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="729c4-175">After this step has been completed, hello connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span>
   
        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET
6. <span data-ttu-id="729c4-176"><a name="vpngw"></a>Därefter skapar du din plats-till-plats-VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="729c4-176"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="729c4-177">Hej GatewaySKU måste vara *Standard*, *HighPerformance*, eller *UltraPerformance* och hello GatewayType måste vara *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="729c4-177">hello GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance* and hello GatewayType must be *DynamicRouting*.</span></span>
   
        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance
   
    <span data-ttu-id="729c4-178">tooretrieve hello gatewayinställningarna för virtuella nätverk, inklusive hello gateway-ID och hello offentlig IP, använda hello `Get-AzureVirtualNetworkGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="729c4-178">tooretrieve hello virtual network gateway settings, including hello gateway ID and hello public IP, use hello `Get-AzureVirtualNetworkGateway` cmdlet.</span></span>
   
        Get-AzureVirtualNetworkGateway
   
        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for hello following virtual network: GNSDesMoines
        LastEventID          : 23002
        State                : Provisioned
        VIPAddress           : 104.43.x.y
        DefaultSite          :
        GatewaySKU           : HighPerformance
        Location             :
        VnetId               : 979aabcf-e47f-4136-ab9b-b4780c1e1bd5
        SubnetId             :
        EnableBgp            : False
        OperationDescription : Get-AzureVirtualNetworkGateway
        OperationId          : 42773656-85e1-a6b6-8705-35473f1e6f6a
        OperationStatus      : Succeeded
7. <span data-ttu-id="729c4-179">Skapa en lokal plats för VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="729c4-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="729c4-180">Det här kommandot konfigurerar inte din lokala VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="729c4-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="729c4-181">I stället kan du tooprovide hello lokala gateway-inställningar, till exempel hello offentlig IP-adress och hello lokala adressutrymmet, så att hello Azure VPN-gateway kan ansluta tooit.</span><span class="sxs-lookup"><span data-stu-id="729c4-181">Rather, it allows you tooprovide hello local gateway settings, such as hello public IP and hello on-premises address space, so that hello Azure VPN gateway can connect tooit.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="729c4-182">hello lokal plats för hello plats-till-plats VPN har inte definierats i hello netcfg.</span><span class="sxs-lookup"><span data-stu-id="729c4-182">hello local site for hello Site-to-Site VPN is not defined in hello netcfg.</span></span> <span data-ttu-id="729c4-183">I stället måste du använda den här cmdlet toospecify hello lokal plats parametrar.</span><span class="sxs-lookup"><span data-stu-id="729c4-183">Instead, you must use this cmdlet toospecify hello local site parameters.</span></span> <span data-ttu-id="729c4-184">Du kan inte definiera den med hjälp av portalen eller hello netcfg filen.</span><span class="sxs-lookup"><span data-stu-id="729c4-184">You cannot define it using either portal, or hello netcfg file.</span></span>
   > 
   > 
   
    <span data-ttu-id="729c4-185">Använd hello följande exempel, ersätter hello värden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="729c4-185">Use hello following sample, replacing hello values with your own.</span></span>
   
        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>
   
   > [!NOTE]
   > <span data-ttu-id="729c4-186">Om det lokala nätverket har flera routningar kan du ange dem i en matris.</span><span class="sxs-lookup"><span data-stu-id="729c4-186">If your local network has multiple routes, you can pass them all in as an array.</span></span>  <span data-ttu-id="729c4-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span><span class="sxs-lookup"><span data-stu-id="729c4-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span></span>  
   > 
   > 

    <span data-ttu-id="729c4-188">tooretrieve hello gatewayinställningarna för virtuella nätverk, inklusive hello gateway-ID och hello offentlig IP, använda hello `Get-AzureVirtualNetworkGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="729c4-188">tooretrieve hello virtual network gateway settings, including hello gateway ID and hello public IP, use hello `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="729c4-189">Se följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="729c4-189">See hello following example.</span></span>

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


1. <span data-ttu-id="729c4-190">Konfigurera din lokala VPN-enhet tooconnect toohello ny gateway.</span><span class="sxs-lookup"><span data-stu-id="729c4-190">Configure your local VPN device tooconnect toohello new gateway.</span></span> <span data-ttu-id="729c4-191">Använd hello information som du hämtade i steg 6 när du konfigurerar VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="729c4-191">Use hello information that you retrieved in step 6 when configuring your VPN device.</span></span> <span data-ttu-id="729c4-192">Mer information om VPN-enhetskonfiguration finns i [VPN-enhetskonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="729c4-192">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
2. <span data-ttu-id="729c4-193">Länken hello plats-till-plats VPN-gateway på Azure toohello lokala gateway.</span><span class="sxs-lookup"><span data-stu-id="729c4-193">Link hello Site-to-Site VPN gateway on Azure toohello local gateway.</span></span>
   
    <span data-ttu-id="729c4-194">I det här exemplet är connectedEntityId hello lokala gateway-ID som du hittar genom att köra `Get-AzureLocalNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="729c4-194">In this example, connectedEntityId is hello local gateway ID, which you can find by running `Get-AzureLocalNetworkGateway`.</span></span> <span data-ttu-id="729c4-195">Du kan hitta virtualNetworkGatewayId med hello `Get-AzureVirtualNetworkGateway` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="729c4-195">You can find virtualNetworkGatewayId by using hello `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="729c4-196">Efter det här steget upprättas hello anslutning mellan ditt lokala nätverk och Azure via hello plats-till-plats VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="729c4-196">After this step, hello connection between your local network and Azure via hello Site-to-Site VPN connection is established.</span></span>

        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <span data-ttu-id="729c4-197"><a name="add"></a>tooconfigure coexsiting anslutningar för ett befintligt virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="729c4-197"><a name="add"></a>tooconfigure coexsiting connections for an already existing VNet</span></span>
<span data-ttu-id="729c4-198">Om du har ett befintligt virtuellt nätverk kontrollerar du hello gateway undernätets storlek.</span><span class="sxs-lookup"><span data-stu-id="729c4-198">If you have an existing virtual network, check hello gateway subnet size.</span></span> <span data-ttu-id="729c4-199">Om hello gateway-undernät är /28 eller /29, måste du först ta bort hello virtuell nätverksgateway och öka storleken på hello gateway-undernät.</span><span class="sxs-lookup"><span data-stu-id="729c4-199">If hello gateway subnet is /28 or /29, you must first delete hello virtual network gateway and increase hello gateway subnet size.</span></span> <span data-ttu-id="729c4-200">hello stegen i det här avsnittet visar hur toodo som.</span><span class="sxs-lookup"><span data-stu-id="729c4-200">hello steps in this section will show you how toodo that.</span></span>

<span data-ttu-id="729c4-201">Om hello gateway-undernät är minst/27 eller större och hello virtuellt nätverk är anslutna via ExpressRoute kan du hoppa över hello stegen nedan och fortsätta för[”steg 6 – skapa en plats-till-plats VPN-gateway”](#vpngw) hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="729c4-201">If hello gateway subnet is /27 or larger and hello virtual network is connected via ExpressRoute, you can skip hello steps below and proceed too["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in hello previous section.</span></span>

> [!NOTE]
> <span data-ttu-id="729c4-202">När du tar bort hello befintlig gateway förlorar din lokala lokala hello anslutning tooyour virtuellt nätverk när du arbetar med den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="729c4-202">When you delete hello existing gateway, your local premises will lose hello connection tooyour virtual network while you are working on this configuration.</span></span>
> 
> 

1. <span data-ttu-id="729c4-203">Du behöver tooinstall hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="729c4-203">You'll need tooinstall hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="729c4-204">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="729c4-204">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span> <span data-ttu-id="729c4-205">Observera att hello-cmdlets som du vill använda för den här konfigurationen kan vara något annorlunda än vad du kan känna till.</span><span class="sxs-lookup"><span data-stu-id="729c4-205">Note that hello cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="729c4-206">Anges till toouse hello-cmdlets i dessa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="729c4-206">Be sure toouse hello cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="729c4-207">Ta bort hello befintlig ExpressRoute eller plats-till-plats VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="729c4-207">Delete hello existing ExpressRoute or Site-to-Site VPN gateway.</span></span> <span data-ttu-id="729c4-208">Använd hello följande cmdlet, ersätter hello värden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="729c4-208">Use hello following cmdlet, replacing hello values with your own.</span></span>
   
        Remove-AzureVNetGateway –VnetName MyAzureVNET
3. <span data-ttu-id="729c4-209">Exportera hello virtuellt nätverk schema.</span><span class="sxs-lookup"><span data-stu-id="729c4-209">Export hello virtual network schema.</span></span> <span data-ttu-id="729c4-210">Använd hello följande PowerShell-cmdleten, ersätter hello värden med dina egna.</span><span class="sxs-lookup"><span data-stu-id="729c4-210">Use hello following PowerShell cmdlet, replacing hello values with your own.</span></span>
   
        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”
4. <span data-ttu-id="729c4-211">Redigera schemat för hello nätverket konfigurationsfilen så att hello gateway-undernät är minst/27 eller ett kortare prefix (till exempel /26 eller /25).</span><span class="sxs-lookup"><span data-stu-id="729c4-211">Edit hello network configuration file schema so that hello gateway subnet is /27 or a shorter prefix (such as /26 or /25).</span></span> <span data-ttu-id="729c4-212">Se följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="729c4-212">See hello following example.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="729c4-213">Om du inte har tillräckligt med IP-adresser som är kvar i ditt virtuella nätverk tooincrease hello gateway undernätets storlek, måste tooadd flera IP-adressutrymme.</span><span class="sxs-lookup"><span data-stu-id="729c4-213">If you don't have enough IP addresses left in your virtual network tooincrease hello gateway subnet size, you need tooadd more IP address space.</span></span> <span data-ttu-id="729c4-214">Läs mer om hello Konfigurationsschemat [Azure Virtual Network Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="729c4-214">For more information about hello configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   > 
   > 
   
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
5. <span data-ttu-id="729c4-215">Om din gateway som tidigare var en plats-till-plats-VPN, måste du också ändra hello anslutningstypen för**dedikerad**.</span><span class="sxs-lookup"><span data-stu-id="729c4-215">If your previous gateway was a Site-to-Site VPN, you must also change hello connection type too**Dedicated**.</span></span>
   
                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
6. <span data-ttu-id="729c4-216">Just nu har du ett VNet utan gateways.</span><span class="sxs-lookup"><span data-stu-id="729c4-216">At this point, you'll have a VNet with no gateways.</span></span> <span data-ttu-id="729c4-217">toocreate nya gatewayer och slutföra dina anslutningar, du kan fortsätta med [steg 4: skapa en ExpressRoute-gateway](#gw)hittades i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="729c4-217">toocreate new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in hello preceding set of steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="729c4-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="729c4-218">Next steps</span></span>
<span data-ttu-id="729c4-219">Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md)</span><span class="sxs-lookup"><span data-stu-id="729c4-219">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md)</span></span>


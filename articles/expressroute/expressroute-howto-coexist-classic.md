---
title: "Konfigurera ExpressRoute-anslutningar och VPN-anslutningar för plats-till-plats som kan samexistera: Azure (klassisk) | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att konfigurera ExpressRoute och en VPN-anslutning för plats till plats som kan samexistera för den klassiska distributionsmodellen."
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
ms.openlocfilehash: 09d1649f0ca0cf4ca464d95b29461cad3fe51788
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections-classic"></a><span data-ttu-id="90013-103">Konfigurera ExpressRoute-anslutningar och VPN-anslutningar för plats-till-plats som kan samexistera (klassisk)</span><span class="sxs-lookup"><span data-stu-id="90013-103">Configure ExpressRoute and Site-to-Site coexisting connections (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="90013-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="90013-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="90013-105">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="90013-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="90013-106">Att kunna konfigurera VPN för plats till plats och ExpressRoute har flera fördelar.</span><span class="sxs-lookup"><span data-stu-id="90013-106">Having the ability to configure Site-to-Site VPN and ExpressRoute has several advantages.</span></span> <span data-ttu-id="90013-107">Du kan konfigurera VPN för plats till plats som en säker redundanssökväg för ExpressRoute, eller använda VPN för plats till plats för att ansluta till platser som inte är anslutna via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="90013-107">You can configure Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs to connect to sites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="90013-108">Vi beskriver stegen för att konfigurera båda scenarierna i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="90013-108">We will cover the steps to configure both scenarios in this article.</span></span> <span data-ttu-id="90013-109">Den här artikeln gäller den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="90013-109">This article applies to the classic deployment model.</span></span> <span data-ttu-id="90013-110">Den här konfigurationen är inte tillgänglig i portalen.</span><span class="sxs-lookup"><span data-stu-id="90013-110">This configuration is not available in the portal.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="90013-111">**Om distributionsmodeller för Azure**</span><span class="sxs-lookup"><span data-stu-id="90013-111">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="90013-112">ExpressRoute-kretsarna måste förkonfigureras innan du följer anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="90013-112">ExpressRoute circuits must be pre-configured before you follow the instructions below.</span></span> <span data-ttu-id="90013-113">Kontrollera att du har följt riktlinjerna för att [skapa en ExpressRoute-krets](expressroute-howto-circuit-classic.md) och [konfigurera routning](expressroute-howto-routing-classic.md) innan du följer stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="90013-113">Make sure that you have followed the guides to [create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and [configure routing](expressroute-howto-routing-classic.md) before you follow the steps below.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="90013-114">Gränser och begränsningar</span><span class="sxs-lookup"><span data-stu-id="90013-114">Limits and limitations</span></span>
* <span data-ttu-id="90013-115">**Transit-routing stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="90013-115">**Transit routing is not supported.**</span></span> <span data-ttu-id="90013-116">Du kan inte routa (via Azure) mellan ditt lokala nätverk som ansluter via plats-till-plats-VPN och ditt lokala nätverk som ansluter via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="90013-116">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="90013-117">**Punkt-till-plats stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="90013-117">**Point-to-site is not supported.**</span></span> <span data-ttu-id="90013-118">Du kan inte aktivera punkt-till-plats-VPN-anslutningar till samma VNet som är anslutet till ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="90013-118">You can't enable point-to-site VPN connections to the same VNet that is connected to ExpressRoute.</span></span> <span data-ttu-id="90013-119">Punkt-till-plats-VPN och ExpressRoute kan inte samexistera för samma VNet.</span><span class="sxs-lookup"><span data-stu-id="90013-119">Point-to-site VPN and ExpressRoute cannot coexist for the same VNet.</span></span>
* <span data-ttu-id="90013-120">**Framtvingad tunneling kan inte aktiveras för plats-till-plats-VPN-gatewayen.**</span><span class="sxs-lookup"><span data-stu-id="90013-120">**Forced tunneling cannot be enabled on the Site-to-Site VPN gateway.**</span></span> <span data-ttu-id="90013-121">Du kan bara "tvinga" all Internetriktad trafik tillbaka till ditt lokala nätverk via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="90013-121">You can only "force" all Internet-bound traffic back to your on-premises network via ExpressRoute.</span></span>
* <span data-ttu-id="90013-122">**Basic-SKU-gatewayen stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="90013-122">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="90013-123">Du måste använda en icke-Basic SKU-gateway för både [ExpressRoute-gatewayen](expressroute-about-virtual-network-gateways.md) och [VPN-gatewayen](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="90013-123">You must use a non-Basic SKU gateway for both the [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and the [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="90013-124">**Enbart routebaserad VPN-gateway stöds.**</span><span class="sxs-lookup"><span data-stu-id="90013-124">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="90013-125">Du måste använda en routebaserad [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="90013-125">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="90013-126">**Statiska vägar ska konfigureras för din VPN-gateway.**</span><span class="sxs-lookup"><span data-stu-id="90013-126">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="90013-127">Om ditt lokala nätverk är anslutet både till ExpressRoute och en plats-till-plats-VPN så måste du ha konfigurerat en statisk väg i ditt lokala nätverk för att routa plats-till-plats-VPN-anslutningen till det offentliga Internet.</span><span class="sxs-lookup"><span data-stu-id="90013-127">If your local network is connected to both ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network to route the Site-to-Site VPN connection to the public Internet.</span></span>
* <span data-ttu-id="90013-128">**ExpressRoute-gatewayen måste konfigureras först.**</span><span class="sxs-lookup"><span data-stu-id="90013-128">**ExpressRoute gateway must be configured first.**</span></span> <span data-ttu-id="90013-129">Du måste skapa ExpressRoute-gatewayen först innan du lägger till en plats-till-plats-VPN-gateway för plats till plats.</span><span class="sxs-lookup"><span data-stu-id="90013-129">You must create the ExpressRoute gateway first before you add the Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="90013-130">Konfigurationsdesign</span><span class="sxs-lookup"><span data-stu-id="90013-130">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="90013-131">Konfigurera en VPN för plats till plats som en redundanssökväg för ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="90013-131">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="90013-132">Du kan konfigurera en VPN-anslutning för plats till plats som en säkerhetskopia av ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="90013-132">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="90013-133">Detta gäller endast virtuella nätverk som är länkade till Azures privata peering-sökväg.</span><span class="sxs-lookup"><span data-stu-id="90013-133">This applies only to virtual networks linked to the Azure private peering path.</span></span> <span data-ttu-id="90013-134">Det finns ingen VPN-baserad redundanslösning för tjänster som är tillgängliga via Azures offentliga och Microsofts peerings.</span><span class="sxs-lookup"><span data-stu-id="90013-134">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="90013-135">ExpressRoute-kretsen är alltid den primära länken.</span><span class="sxs-lookup"><span data-stu-id="90013-135">The ExpressRoute circuit is always the primary link.</span></span> <span data-ttu-id="90013-136">Data flödar endast via VPN-sökvägen för plats till plats om ExpressRoute-kretsen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="90013-136">Data will flow through the Site-to-Site VPN path only if the ExpressRoute circuit fails.</span></span> 

> [!NOTE]
> <span data-ttu-id="90013-137">Medan ExpressRoute-kretsen är prioriterad över plats-till-plats-VPN när bägge vägarna är samma, kommer Azure att använda den längsta prefixmatchning för att välja väg till paketets mål.</span><span class="sxs-lookup"><span data-stu-id="90013-137">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are the same, Azure will use the longest prefix match to choose the route towards the packet's destination.</span></span>
> 
> 

![Samexistera](media/expressroute-howto-coexist-classic/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a><span data-ttu-id="90013-139">Konfigurera en VPN för plats till plats för att ansluta till platser som inte är anslutna via ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="90013-139">Configure a Site-to-Site VPN to connect to sites not connected through ExpressRoute</span></span>
<span data-ttu-id="90013-140">Du kan konfigurera nätverket där vissa platser ansluter direkt till Azure via VPN för plats till plats och vissa platser ansluter via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="90013-140">You can configure your network where some sites connect directly to Azure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Samexistera](media/expressroute-howto-coexist-classic/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="90013-142">Det går inte att konfigurera ett virtuellt nätverk som en överföringsrouter.</span><span class="sxs-lookup"><span data-stu-id="90013-142">You cannot a configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-the-steps-to-use"></a><span data-ttu-id="90013-143">Välj vilka steg du ska använda</span><span class="sxs-lookup"><span data-stu-id="90013-143">Selecting the steps to use</span></span>
<span data-ttu-id="90013-144">Det finns två uppsättningar procedurer att välja mellan för att konfigurera anslutningar som kan existera tillsammans.</span><span class="sxs-lookup"><span data-stu-id="90013-144">There are two different sets of procedures to choose from in order to configure connections that can coexist.</span></span> <span data-ttu-id="90013-145">Vilken konfigurationsprocedur du väljer beror på om du har ett befintligt virtuellt nätverk som du vill ansluta till, eller om du vill skapa ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="90013-145">The configuration procedure that you select will depend on whether you have an existing virtual network that you want to connect to, or you want to create a new virtual network.</span></span>

* <span data-ttu-id="90013-146">Jag har inte något VNet och behöver skapa ett.</span><span class="sxs-lookup"><span data-stu-id="90013-146">I don't have a VNet and need to create one.</span></span>
  
    <span data-ttu-id="90013-147">Om du inte redan har ett virtuellt nätverk kan den här proceduren hjälpa dig att skapa ett nytt virtuellt nätverk med hjälp av en klassisk distributionsmodell, samt skapa nya ExpressRoute- och VPN-anslutningar för plats till plats.</span><span class="sxs-lookup"><span data-stu-id="90013-147">If you don’t already have a virtual network, this procedure will walk you through creating a new virtual network using the classic deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="90013-148">Om du vill konfigurera följer du stegen i artikelavsnittet [Så här skapar du ett nytt virtuellt nätverk och samtidiga anslutningar](#new).</span><span class="sxs-lookup"><span data-stu-id="90013-148">To configure, follow the steps in the article section [To create a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="90013-149">Jag har redan en klassisk distributionsmodell för VNet.</span><span class="sxs-lookup"><span data-stu-id="90013-149">I already have a classic deployment model VNet.</span></span>
  
    <span data-ttu-id="90013-150">Du kanske redan har ett virtuellt nätverk på plats med en befintlig VPN-anslutning för plats till plats eller en ExpressRoute-anslutning.</span><span class="sxs-lookup"><span data-stu-id="90013-150">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="90013-151">Artikelavsnittet [Så här konfigurerar du samexisterande anslutningar för ett befintligt VNet](#add) visar hur du tar bort gatewayen och sedan skapar nya ExpressRoute- och VPN-anslutningar för plats till plats.</span><span class="sxs-lookup"><span data-stu-id="90013-151">The article section [To configure coexsiting connections for an already existing VNet](#add) will walk you through deleting the gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="90013-152">Observera att när du skapar nya anslutningar måste stegen utföras i en mycket specifik ordning.</span><span class="sxs-lookup"><span data-stu-id="90013-152">Note that when creating the new connections, the steps must be completed in a very specific order.</span></span> <span data-ttu-id="90013-153">Följ inte anvisningarna i andra artiklar när du skapar dina gateways och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="90013-153">Don't use the instructions in other articles to create your gateways and connections.</span></span>
  
    <span data-ttu-id="90013-154">I den här proceduren skapar du anslutningar som kan samexistera, vilket kräver att du tar bort din gateway och sedan konfigurerar nya gateways.</span><span class="sxs-lookup"><span data-stu-id="90013-154">In this procedure, creating connections that can coexist will require you to delete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="90013-155">Detta innebär att du har stilleståndstid för anslutningar på flera platser när du tar bort och återskapar din gateway och dina anslutningar, men du behöver inte migrera några virtuella datorer eller tjänster till ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="90013-155">This means you will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need to migrate any of your VMs or services to a new virtual network.</span></span> <span data-ttu-id="90013-156">Dina virtuella datorer och tjänster kommer fortfarande att kunna kommunicera ut via belastningsutjämnaren när du konfigurerar din gateway, om de är konfigurerade för att göra det.</span><span class="sxs-lookup"><span data-stu-id="90013-156">Your VMs and services will still be able to communicate out through the load balancer while you configure your gateway if they are configured to do so.</span></span>

## <span data-ttu-id="90013-157"><a name="new"></a>Så här skapar du ett nytt virtuellt nätverk och samexisterande anslutningar</span><span class="sxs-lookup"><span data-stu-id="90013-157"><a name="new"></a>To create a new virtual network and coexisting connections</span></span>
<span data-ttu-id="90013-158">Den här proceduren vägleder dig genom att skapa ett VNet samt plats-till-plats- och ExpressRoute-anslutningar som ska finnas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="90013-158">This procedure will walk you through creating a VNet and create Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="90013-159">Du måste installera den senaste versionen av Azure PowerShell-cmdletarna.</span><span class="sxs-lookup"><span data-stu-id="90013-159">You'll need to install the latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="90013-160">Mer information om hur man installerar PowerShell-cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="90013-160">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span> <span data-ttu-id="90013-161">Observera att de cmdletar som du använder för den här konfigurationen kan se annorlunda ut än de du tidigare använt.</span><span class="sxs-lookup"><span data-stu-id="90013-161">Note that the cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="90013-162">Var noga med att använda de cmdletar som anges i instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="90013-162">Be sure to use the cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="90013-163">Skapa ett schema för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="90013-163">Create a schema for your virtual network.</span></span> <span data-ttu-id="90013-164">Mer information om konfigurationsschemat finns i [Konfigurationsschema för Azure Virtual Network](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="90013-164">For more information about the configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   
    <span data-ttu-id="90013-165">När du skapar ditt schema bör du vara noga med att använda följande värden:</span><span class="sxs-lookup"><span data-stu-id="90013-165">When you create your schema, make sure you use the following values:</span></span>
   
   * <span data-ttu-id="90013-166">Gateway-undernätet för det virtuella nätverket måste vara /27 eller ett kortare prefix (till exempel /26 eller /25).</span><span class="sxs-lookup"><span data-stu-id="90013-166">The gateway subnet for the virtual network must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   * <span data-ttu-id="90013-167">Anslutningstypen för gatewayen är ”Dedikerad”.</span><span class="sxs-lookup"><span data-stu-id="90013-167">The gateway connection type is "Dedicated".</span></span>
     
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
3. <span data-ttu-id="90013-168">När du har skapat och konfigurerat schemats XML-fil, överför du filen.</span><span class="sxs-lookup"><span data-stu-id="90013-168">After creating and configuring your xml schema file, upload the file.</span></span> <span data-ttu-id="90013-169">Detta skapar det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="90013-169">This will create your virtual network.</span></span>
   
    <span data-ttu-id="90013-170">Använd följande cmdlet för att överföra filen och ersätta värdet med ditt egna.</span><span class="sxs-lookup"><span data-stu-id="90013-170">Use the following cmdlet to upload your file, replacing the value with your own.</span></span>
   
        Set-AzureVNetConfig -ConfigurationPath 'C:\NetworkConfig.xml'
4. <span data-ttu-id="90013-171"><a name="gw"></a>Skapa en ExpressRoute-gateway.</span><span class="sxs-lookup"><span data-stu-id="90013-171"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="90013-172">Se till att ange GatewaySKU som *standard*, *HighPerformance*, eller *UltraPerformance* och GatewayType som *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="90013-172">Be sure to specify the GatewaySKU as *Standard*, *HighPerformance*, or *UltraPerformance* and the GatewayType as *DynamicRouting*.</span></span>
   
    <span data-ttu-id="90013-173">Använd följande exempel och ersätt värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="90013-173">Use the following sample, substituting the values for your own.</span></span>
   
        New-AzureVNetGateway -VNetName MyAzureVNET -GatewayType DynamicRouting -GatewaySKU HighPerformance
5. <span data-ttu-id="90013-174">Länka ExpressRoute-gatewayen till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="90013-174">Link the ExpressRoute gateway to the ExpressRoute circuit.</span></span> <span data-ttu-id="90013-175">När det här steget har slutförts har anslutningen mellan ditt lokala nätverk och Azure, via ExpressRoute, upprättats.</span><span class="sxs-lookup"><span data-stu-id="90013-175">After this step has been completed, the connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span>
   
        New-AzureDedicatedCircuitLink -ServiceKey <service-key> -VNetName MyAzureVNET
6. <span data-ttu-id="90013-176"><a name="vpngw"></a>Därefter skapar du din plats-till-plats-VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="90013-176"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="90013-177">GatewaySKU måste vara *Standard*, *HighPerformance*, eller *UltraPerformance* och GatewayType måste vara *DynamicRouting*.</span><span class="sxs-lookup"><span data-stu-id="90013-177">The GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance* and the GatewayType must be *DynamicRouting*.</span></span>
   
        New-AzureVirtualNetworkGateway -VNetName MyAzureVNET -GatewayName S2SVPN -GatewayType DynamicRouting -GatewaySKU  HighPerformance
   
    <span data-ttu-id="90013-178">Om du ska hämta inställningarna för den virtuella nätverksgatewayen, inklusive gateway-ID och offentlig IP, använder du cmdleten `Get-AzureVirtualNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="90013-178">To retrieve the virtual network gateway settings, including the gateway ID and the public IP, use the `Get-AzureVirtualNetworkGateway` cmdlet.</span></span>
   
        Get-AzureVirtualNetworkGateway
   
        GatewayId            : 348ae011-ffa9-4add-b530-7cb30010565e
        GatewayName          : S2SVPN
        LastEventData        :
        GatewayType          : DynamicRouting
        LastEventTimeStamp   : 5/29/2015 4:41:41 PM
        LastEventMessage     : Successfully created a gateway for the following virtual network: GNSDesMoines
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
7. <span data-ttu-id="90013-179">Skapa en lokal plats för VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="90013-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="90013-180">Det här kommandot konfigurerar inte din lokala VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="90013-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="90013-181">I stället kan du ange lokala gateway-inställningar, som t.ex. offentlig IP-adress och lokalt adressutrymme, så att Azure VPN-gatewayen kan ansluta till den.</span><span class="sxs-lookup"><span data-stu-id="90013-181">Rather, it allows you to provide the local gateway settings, such as the public IP and the on-premises address space, so that the Azure VPN gateway can connect to it.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="90013-182">Den lokala platsen för VPN för plats till plats är inte definierad i netcfg.</span><span class="sxs-lookup"><span data-stu-id="90013-182">The local site for the Site-to-Site VPN is not defined in the netcfg.</span></span> <span data-ttu-id="90013-183">I stället måste du använda denna cmdlet för att ange de lokala platsparametrarna.</span><span class="sxs-lookup"><span data-stu-id="90013-183">Instead, you must use this cmdlet to specify the local site parameters.</span></span> <span data-ttu-id="90013-184">Du kan inte definiera den med hjälp av portalen eller netcfg-filen.</span><span class="sxs-lookup"><span data-stu-id="90013-184">You cannot define it using either portal, or the netcfg file.</span></span>
   > 
   > 
   
    <span data-ttu-id="90013-185">Använd följande exempel och ersätt värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="90013-185">Use the following sample, replacing the values with your own.</span></span>
   
        New-AzureLocalNetworkGateway -GatewayName MyLocalNetwork -IpAddress <MyLocalGatewayIp> -AddressSpace <MyLocalNetworkAddress>
   
   > [!NOTE]
   > <span data-ttu-id="90013-186">Om det lokala nätverket har flera routningar kan du ange dem i en matris.</span><span class="sxs-lookup"><span data-stu-id="90013-186">If your local network has multiple routes, you can pass them all in as an array.</span></span>  <span data-ttu-id="90013-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span><span class="sxs-lookup"><span data-stu-id="90013-187">$MyLocalNetworkAddress = @("10.1.2.0/24","10.1.3.0/24","10.2.1.0/24")</span></span>  
   > 
   > 

    <span data-ttu-id="90013-188">Om du ska hämta inställningarna för den virtuella nätverksgatewayen, inklusive gateway-ID och offentlig IP, använder du cmdleten `Get-AzureVirtualNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="90013-188">To retrieve the virtual network gateway settings, including the gateway ID and the public IP, use the `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="90013-189">Se följande exempel.</span><span class="sxs-lookup"><span data-stu-id="90013-189">See the following example.</span></span>

        Get-AzureLocalNetworkGateway

        GatewayId            : 532cb428-8c8c-4596-9a4f-7ae3a9fcd01b
        GatewayName          : MyLocalNetwork
        IpAddress            : 23.39.x.y
        AddressSpace         : {10.1.2.0/24}
        OperationDescription : Get-AzureLocalNetworkGateway
        OperationId          : ddc4bfae-502c-adc7-bd7d-1efbc00b3fe5
        OperationStatus      : Succeeded


1. <span data-ttu-id="90013-190">Konfigurera din lokala VPN-enhet så att den ansluter till den nya gatewayen.</span><span class="sxs-lookup"><span data-stu-id="90013-190">Configure your local VPN device to connect to the new gateway.</span></span> <span data-ttu-id="90013-191">Använd den information som du hämtade i steg 6 när du konfigurerar VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="90013-191">Use the information that you retrieved in step 6 when configuring your VPN device.</span></span> <span data-ttu-id="90013-192">Mer information om VPN-enhetskonfiguration finns i [VPN-enhetskonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="90013-192">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
2. <span data-ttu-id="90013-193">Länka VPN-gatewayen för plats till plats på Azure till den lokala gatewayen.</span><span class="sxs-lookup"><span data-stu-id="90013-193">Link the Site-to-Site VPN gateway on Azure to the local gateway.</span></span>
   
    <span data-ttu-id="90013-194">I det här exemplet är connectedEntityId det lokala gateway-ID som du hittar genom att köra `Get-AzureLocalNetworkGateway`.</span><span class="sxs-lookup"><span data-stu-id="90013-194">In this example, connectedEntityId is the local gateway ID, which you can find by running `Get-AzureLocalNetworkGateway`.</span></span> <span data-ttu-id="90013-195">Du kan hitta virtualNetworkGatewayId med hjälp av `Get-AzureVirtualNetworkGateway`-cmdleten.</span><span class="sxs-lookup"><span data-stu-id="90013-195">You can find virtualNetworkGatewayId by using the `Get-AzureVirtualNetworkGateway` cmdlet.</span></span> <span data-ttu-id="90013-196">Efter det här steget upprättas anslutningen mellan ditt lokala nätverk och Azure via VPN-anslutningen för plats till plats.</span><span class="sxs-lookup"><span data-stu-id="90013-196">After this step, the connection between your local network and Azure via the Site-to-Site VPN connection is established.</span></span>

        New-AzureVirtualNetworkGatewayConnection -connectedEntityId <local-network-gateway-id> -gatewayConnectionName Azure2Local -gatewayConnectionType IPsec -sharedKey abc123 -virtualNetworkGatewayId <azure-s2s-vpn-gateway-id>

## <span data-ttu-id="90013-197"><a name="add"></a>Så här konfigurerar du samexisterande anslutningar för ett befintligt VNet</span><span class="sxs-lookup"><span data-stu-id="90013-197"><a name="add"></a>To configure coexsiting connections for an already existing VNet</span></span>
<span data-ttu-id="90013-198">Om du har ett befintligt virtuellt nätverk, kontrollerar du storleken på gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="90013-198">If you have an existing virtual network, check the gateway subnet size.</span></span> <span data-ttu-id="90013-199">Om gateway-undernätet är /28 eller /29, måste du först ta bort den virtuella nätverksgatewayen och öka storleken för gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="90013-199">If the gateway subnet is /28 or /29, you must first delete the virtual network gateway and increase the gateway subnet size.</span></span> <span data-ttu-id="90013-200">Stegen i det här avsnittet visar hur du gör.</span><span class="sxs-lookup"><span data-stu-id="90013-200">The steps in this section will show you how to do that.</span></span>

<span data-ttu-id="90013-201">Om gateway-undernätet är /27 eller större och det virtuella nätverket är anslutet via ExpressRoute, kan du hoppa över stegen nedan och gå vidare till [”Steg 6 – Skapa en VPN-gateway för plats till plats”](#vpngw) i föregående avsnitt. </span><span class="sxs-lookup"><span data-stu-id="90013-201">If the gateway subnet is /27 or larger and the virtual network is connected via ExpressRoute, you can skip the steps below and proceed to ["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in the previous section.</span></span>

> [!NOTE]
> <span data-ttu-id="90013-202">När du tar bort den befintliga gatewayen förlorar dina lokala platser anslutningen till ditt virtuella nätverk medan du arbetar med konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="90013-202">When you delete the existing gateway, your local premises will lose the connection to your virtual network while you are working on this configuration.</span></span>
> 
> 

1. <span data-ttu-id="90013-203">Du måste installera den senaste versionen av Azure Resource Managers PowerShell-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="90013-203">You'll need to install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="90013-204">Mer information om hur man installerar PowerShell-cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="90013-204">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span> <span data-ttu-id="90013-205">Observera att de cmdletar som du använder för den här konfigurationen kan se annorlunda ut än de du tidigare använt.</span><span class="sxs-lookup"><span data-stu-id="90013-205">Note that the cmdlets that you'll use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="90013-206">Var noga med att använda de cmdletar som anges i instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="90013-206">Be sure to use the cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="90013-207">Ta bort den befintliga ExpressRoute- eller VPN-gatewayen för plats till plats.</span><span class="sxs-lookup"><span data-stu-id="90013-207">Delete the existing ExpressRoute or Site-to-Site VPN gateway.</span></span> <span data-ttu-id="90013-208">Använd följande cmdlet och ersätt värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="90013-208">Use the following cmdlet, replacing the values with your own.</span></span>
   
        Remove-AzureVNetGateway –VnetName MyAzureVNET
3. <span data-ttu-id="90013-209">Exportera det virtuella nätverksschemat.</span><span class="sxs-lookup"><span data-stu-id="90013-209">Export the virtual network schema.</span></span> <span data-ttu-id="90013-210">Använd följande PowerShell-cmdlet och ersätt värdena med dina egna.</span><span class="sxs-lookup"><span data-stu-id="90013-210">Use the following PowerShell cmdlet, replacing the values with your own.</span></span>
   
        Get-AzureVNetConfig –ExportToFile “C:\NetworkConfig.xml”
4. <span data-ttu-id="90013-211">Redigera nätverksschemat för konfigurationsfilen så att gateway-undernätet är /27 eller ett kortare prefix (till exempel /26 eller /25).</span><span class="sxs-lookup"><span data-stu-id="90013-211">Edit the network configuration file schema so that the gateway subnet is /27 or a shorter prefix (such as /26 or /25).</span></span> <span data-ttu-id="90013-212">Se följande exempel.</span><span class="sxs-lookup"><span data-stu-id="90013-212">See the following example.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="90013-213">Om du inte har tillräckligt med IP-adresser kvar i det virtuella nätverket för att kunna öka storleken på gateway-undernätet, måste du lägga till mer IP-adressutrymme.</span><span class="sxs-lookup"><span data-stu-id="90013-213">If you don't have enough IP addresses left in your virtual network to increase the gateway subnet size, you need to add more IP address space.</span></span> <span data-ttu-id="90013-214">Mer information om konfigurationsschemat finns i [Konfigurationsschema för Azure Virtual Network](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="90013-214">For more information about the configuration schema, see [Azure Virtual Network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>
   > 
   > 
   
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.17.159.224/27</AddressPrefix>
          </Subnet>
5. <span data-ttu-id="90013-215">Om din gateway tidigare var en VPN för plats till plats, måste du dessutom ändra anslutningstypen till **Dedikerad**.</span><span class="sxs-lookup"><span data-stu-id="90013-215">If your previous gateway was a Site-to-Site VPN, you must also change the connection type to **Dedicated**.</span></span>
   
                 <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="MyLocalNetwork">
                      <Connection type="Dedicated" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
6. <span data-ttu-id="90013-216">Just nu har du ett VNet utan gateways.</span><span class="sxs-lookup"><span data-stu-id="90013-216">At this point, you'll have a VNet with no gateways.</span></span> <span data-ttu-id="90013-217">Om du vill slutföra dina anslutningar och skapa nya gateways kan du fortsätta med [Steg 4 – Skapa en ExpressRoute-gateway](#gw), som finns i den föregående uppsättningen med steg.</span><span class="sxs-lookup"><span data-stu-id="90013-217">To create new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in the preceding set of steps.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90013-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="90013-218">Next steps</span></span>
<span data-ttu-id="90013-219">Mer information om ExpressRoute finns i [Vanliga frågor och svar om ExpressRoute](expressroute-faqs.md)</span><span class="sxs-lookup"><span data-stu-id="90013-219">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md)</span></span>


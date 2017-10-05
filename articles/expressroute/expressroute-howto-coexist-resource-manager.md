---
title: "Konfigurera ExpressRoute-anslutningar och anslutningar för plats-till-plats som kan samexistera: Azure (Resource Manager) | Microsoft Docs"
description: "Den här artikeln visar hur du konfigurerar ExpressRoute och en VPN-anslutning för plats till plats som kan samexistera för Resource Manager-modellen."
documentationcenter: na
services: expressroute
author: charwen
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7717b14-3da3-4a6d-b78e-a5020766bc2c
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: charwen,cherylmc
ms.openlocfilehash: b29147a37f9a90fc80e16b350ac9b91daac1d7f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a><span data-ttu-id="145b9-103">Konfigurera ExpressRoute-anslutningar och anslutningar för plats-till-plats som kan samexistera</span><span class="sxs-lookup"><span data-stu-id="145b9-103">Configure ExpressRoute and Site-to-Site coexisting connections</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="145b9-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="145b9-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="145b9-105">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="145b9-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="145b9-106">Att konfigurera VPN för plats till plats och samexisterande ExpressRoute-anslutningar har flera fördelar.</span><span class="sxs-lookup"><span data-stu-id="145b9-106">Configuring Site-to-Site VPN and ExpressRoute coexisting connections has several advantages.</span></span> <span data-ttu-id="145b9-107">Du kan konfigurera en VPN för plats till plats som en säker redundanssökväg för ExpressRoute, eller använda plats-till-plats-VPN för att ansluta till platser som inte är anslutna via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="145b9-107">You can configure a Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs to connect to sites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="145b9-108">Vi beskriver stegen för att konfigurera båda scenarierna i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="145b9-108">We cover the steps to configure both scenarios in this article.</span></span> <span data-ttu-id="145b9-109">Den här artikeln gäller distributionsmodellen i Resource Manager, och PowerShell används.</span><span class="sxs-lookup"><span data-stu-id="145b9-109">This article applies to the Resource Manager deployment model and uses PowerShell.</span></span> <span data-ttu-id="145b9-110">Den här konfigurationen är inte tillgänglig i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="145b9-110">This configuration is not available in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="145b9-111">ExpressRoute-kretsarna måste förkonfigureras innan du följer anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="145b9-111">ExpressRoute circuits must be pre-configured before you follow the instructions below.</span></span> <span data-ttu-id="145b9-112">Kontrollera att du har följt riktlinjerna för att [skapa en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och [konfigurera routning](expressroute-howto-routing-arm.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="145b9-112">Make sure that you have followed the guides to [create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [configure routing](expressroute-howto-routing-arm.md) before you proceed.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="145b9-113">Gränser och begränsningar</span><span class="sxs-lookup"><span data-stu-id="145b9-113">Limits and limitations</span></span>
* <span data-ttu-id="145b9-114">**Transit-routing stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="145b9-114">**Transit routing is not supported.**</span></span> <span data-ttu-id="145b9-115">Du kan inte routa (via Azure) mellan ditt lokala nätverk som ansluter via plats-till-plats-VPN och ditt lokala nätverk som ansluter via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="145b9-115">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="145b9-116">**Basic-SKU-gatewayen stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="145b9-116">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="145b9-117">Du måste använda en icke-Basic SKU-gateway för både [ExpressRoute-gatewayen](expressroute-about-virtual-network-gateways.md) och [VPN-gatewayen](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="145b9-117">You must use a non-Basic SKU gateway for both the [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and the [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="145b9-118">**Enbart routebaserad VPN-gateway stöds.**</span><span class="sxs-lookup"><span data-stu-id="145b9-118">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="145b9-119">Du måste använda en routebaserad [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="145b9-119">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="145b9-120">**Statiska vägar ska konfigureras för din VPN-gateway.**</span><span class="sxs-lookup"><span data-stu-id="145b9-120">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="145b9-121">Om ditt lokala nätverk är anslutet både till ExpressRoute och en plats-till-plats-VPN så måste du ha konfigurerat en statisk väg i ditt lokala nätverk för att routa plats-till-plats-VPN-anslutningen till det offentliga Internet.</span><span class="sxs-lookup"><span data-stu-id="145b9-121">If your local network is connected to both ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network to route the Site-to-Site VPN connection to the public Internet.</span></span>
* <span data-ttu-id="145b9-122">**ExpressRoute-gatewayen måste konfigureras först och länkas till en krets.**</span><span class="sxs-lookup"><span data-stu-id="145b9-122">**ExpressRoute gateway must be configured first and linked to a circuit.**</span></span> <span data-ttu-id="145b9-123">Du måste skapa ExpressRoute-gatewayen först och länka den till en krets innan du lägger till en plats-till-plats-VPN-gateway för plats till plats.</span><span class="sxs-lookup"><span data-stu-id="145b9-123">You must create the ExpressRoute gateway first and link it to a circuit before you add the Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="145b9-124">Konfigurationsdesign</span><span class="sxs-lookup"><span data-stu-id="145b9-124">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="145b9-125">Konfigurera en VPN för plats till plats som en redundanssökväg för ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="145b9-125">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="145b9-126">Du kan konfigurera en VPN-anslutning för plats till plats som en säkerhetskopia av ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="145b9-126">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="145b9-127">Detta gäller endast virtuella nätverk som är länkade till Azures privata peering-sökväg.</span><span class="sxs-lookup"><span data-stu-id="145b9-127">This applies only to virtual networks linked to the Azure private peering path.</span></span> <span data-ttu-id="145b9-128">Det finns ingen VPN-baserad redundanslösning för tjänster som är tillgängliga via Azures offentliga och Microsofts peerings.</span><span class="sxs-lookup"><span data-stu-id="145b9-128">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="145b9-129">ExpressRoute-kretsen är alltid den primära länken.</span><span class="sxs-lookup"><span data-stu-id="145b9-129">The ExpressRoute circuit is always the primary link.</span></span> <span data-ttu-id="145b9-130">Data flödar endast via plats-till-plats-VPN-sökvägen om ExpressRoute-kretsen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="145b9-130">Data flows through the Site-to-Site VPN path only if the ExpressRoute circuit fails.</span></span>

> [!NOTE]
> <span data-ttu-id="145b9-131">Medan ExpressRoute-kretsen är prioriterad över plats-till-plats-VPN när bägge vägarna är samma, kommer Azure att använda den längsta prefixmatchning för att välja väg till paketets mål.</span><span class="sxs-lookup"><span data-stu-id="145b9-131">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are the same, Azure will use the longest prefix match to choose the route towards the packet's destination.</span></span>
> 
> 

![Samexistera](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a><span data-ttu-id="145b9-133">Konfigurera en VPN för plats till plats för att ansluta till platser som inte är anslutna via ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="145b9-133">Configure a Site-to-Site VPN to connect to sites not connected through ExpressRoute</span></span>
<span data-ttu-id="145b9-134">Du kan konfigurera nätverket där vissa platser ansluter direkt till Azure via VPN för plats till plats och vissa platser ansluter via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="145b9-134">You can configure your network where some sites connect directly to Azure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Samexistera](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="145b9-136">Det går inte att konfigurera ett virtuellt nätverk som en överföringsrouter.</span><span class="sxs-lookup"><span data-stu-id="145b9-136">You cannot configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-the-steps-to-use"></a><span data-ttu-id="145b9-137">Välj vilka steg du ska använda</span><span class="sxs-lookup"><span data-stu-id="145b9-137">Selecting the steps to use</span></span>
<span data-ttu-id="145b9-138">Det finns två uppsättningar procedurer för att välja bland.</span><span class="sxs-lookup"><span data-stu-id="145b9-138">There are two different sets of procedures to choose from.</span></span> <span data-ttu-id="145b9-139">Vilken konfigurationsprocedur du väljer beror på om du har ett befintligt virtuellt nätverk som du vill ansluta till, eller om du vill skapa ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="145b9-139">The configuration procedure that you select depends on whether you have an existing virtual network that you want to connect to, or you want to create a new virtual network.</span></span>

* <span data-ttu-id="145b9-140">Jag har inte något VNet och behöver skapa ett.</span><span class="sxs-lookup"><span data-stu-id="145b9-140">I don't have a VNet and need to create one.</span></span>
  
    <span data-ttu-id="145b9-141">Om du inte redan har ett virtuellt nätverk kan den här proceduren hjälpa dig att skapa ett nytt virtuellt nätverk med hjälp av distributionsmodellen i Resource Manager samt skapa nya ExpressRoute- och plats-till-plats-VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="145b9-141">If you don’t already have a virtual network, this procedure walks you through creating a new virtual network using Resource Manager deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="145b9-142">Om du vill konfigurera ett virtuellt nätverk följer du stegen i [Så här skapar du ett nytt virtuellt nätverk och samexisterande anslutningar](#new).</span><span class="sxs-lookup"><span data-stu-id="145b9-142">To configure a virtual network, follow the steps in [To create a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="145b9-143">Jag har redan en distributionsmodell för Resource Manager i VNet.</span><span class="sxs-lookup"><span data-stu-id="145b9-143">I already have a Resource Manager deployment model VNet.</span></span>
  
    <span data-ttu-id="145b9-144">Du kanske redan har ett virtuellt nätverk på plats med en befintlig VPN-anslutning för plats till plats eller en ExpressRoute-anslutning.</span><span class="sxs-lookup"><span data-stu-id="145b9-144">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="145b9-145">Avsnittet [Så här konfigurerar du samexisterande anslutningar för ett befintligt VNet](#add) visar hur du tar bort gatewayen och sedan skapar nya ExpressRoute- och plats-till-plats-VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="145b9-145">The [To configure coexisting connections for an already existing VNet](#add) section walks you through deleting the gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="145b9-146">När du skapar nya anslutningar måste stegen utföras i en specifik ordning.</span><span class="sxs-lookup"><span data-stu-id="145b9-146">When creating the new connections, the steps must be completed in a specific order.</span></span> <span data-ttu-id="145b9-147">Följ inte anvisningarna i andra artiklar när du skapar dina gateways och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="145b9-147">Don't use the instructions in other articles to create your gateways and connections.</span></span>
  
    <span data-ttu-id="145b9-148">I den här proceduren skapar du anslutningar som kan samexistera, vilket kräver att du tar bort din gateway och sedan konfigurerar nya gateways.</span><span class="sxs-lookup"><span data-stu-id="145b9-148">In this procedure, creating connections that can coexist requires you to delete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="145b9-149">Du kommer att ha stilleståndstid för anslutningar på flera platser när du tar bort och återskapar din gateway och dina anslutningar, men du behöver inte migrera några virtuella datorer eller tjänster till ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="145b9-149">You will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need to migrate any of your VMs or services to a new virtual network.</span></span> <span data-ttu-id="145b9-150">Dina virtuella datorer och tjänster kommer fortfarande att kunna kommunicera ut via belastningsutjämnaren när du konfigurerar din gateway, om de är konfigurerade för att göra det.</span><span class="sxs-lookup"><span data-stu-id="145b9-150">Your VMs and services will still be able to communicate out through the load balancer while you configure your gateway if they are configured to do so.</span></span>

## <span data-ttu-id="145b9-151"><a name="new"></a>Så här skapar du ett nytt virtuellt nätverk och samexisterande anslutningar</span><span class="sxs-lookup"><span data-stu-id="145b9-151"><a name="new"></a>To create a new virtual network and coexisting connections</span></span>
<span data-ttu-id="145b9-152">Den här proceduren vägleder dig genom att skapa ett VNet samt plats-till-plats- och ExpressRoute-anslutningar som ska finnas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="145b9-152">This procedure walks you through creating a VNet and Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="145b9-153">Installera den senaste versionen av Azure PowerShell-cmdletarna.</span><span class="sxs-lookup"><span data-stu-id="145b9-153">Install the latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="145b9-154">Mer information om hur man installerar cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="145b9-154">For information about installing the cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="145b9-155">De cmdletar som du använder för den här konfigurationen kan se annorlunda ut än de du tidigare använt.</span><span class="sxs-lookup"><span data-stu-id="145b9-155">The cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="145b9-156">Var noga med att använda de cmdletar som anges i instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="145b9-156">Be sure to use the cmdlets specified in these instructions.</span></span>
2. <span data-ttu-id="145b9-157">Logga in på ditt konto och konfigurera miljön.</span><span class="sxs-lookup"><span data-stu-id="145b9-157">Log in to your account and set up the environment.</span></span>

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. <span data-ttu-id="145b9-158">Skapa ett virtuellt nätverk, inklusive gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="145b9-158">Create a virtual network including Gateway Subnet.</span></span> <span data-ttu-id="145b9-159">Mer information om den virtuella nätverkskonfigurationen finns i [Konfiguration av Azure Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="145b9-159">For more information about the virtual network configuration, see [Azure Virtual Network configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="145b9-160">Gateway-undernätet måste vara /27 eller ett kortare prefix (till exempel /26 eller /25).</span><span class="sxs-lookup"><span data-stu-id="145b9-160">The Gateway Subnet must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   > 
   > 
   
    <span data-ttu-id="145b9-161">Skapa ett nytt VNet.</span><span class="sxs-lookup"><span data-stu-id="145b9-161">Create a new VNet.</span></span>

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    <span data-ttu-id="145b9-162">Lägg till undernät.</span><span class="sxs-lookup"><span data-stu-id="145b9-162">Add subnets.</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="145b9-163">Spara VNet-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="145b9-163">Save the VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="145b9-164"><a name="gw"></a>Skapa en ExpressRoute-gateway.</span><span class="sxs-lookup"><span data-stu-id="145b9-164"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="145b9-165">Mer information om konfiguration av ExpressRoute-gateways finns i [Konfiguration av ExpressRoute-gateway](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="145b9-165">For more information about the ExpressRoute gateway configuration, see [ExpressRoute gateway configuration](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="145b9-166">GatewaySKU:n måste vara *Standard*, *HighPerformance*, eller *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="145b9-166">The GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. <span data-ttu-id="145b9-167">Länka ExpressRoute-gatewayen till ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="145b9-167">Link the ExpressRoute gateway to the ExpressRoute circuit.</span></span> <span data-ttu-id="145b9-168">När det här steget har slutförts har anslutningen mellan ditt lokala nätverk och Azure, via ExpressRoute, upprättats.</span><span class="sxs-lookup"><span data-stu-id="145b9-168">After this step has been completed, the connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span> <span data-ttu-id="145b9-169">Mer information om länken finns i [Länka VNets till ExpressRoute](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="145b9-169">For more information about the link operation, see [Link VNets to ExpressRoute](expressroute-howto-linkvnet-arm.md).</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <span data-ttu-id="145b9-170"><a name="vpngw"></a>Därefter skapar du din plats-till-plats-VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="145b9-170"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="145b9-171">Mer information om konfigurationen av VPN-gatewayen finns i [Konfigurera ett VNet med en plats-till-plats-anslutning](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="145b9-171">For more information about the VPN gateway configuration, see [Configure a VNet with a Site-to-Site connection](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span></span> <span data-ttu-id="145b9-172">GatewaySKU:n måste vara *Standard*, *HighPerformance*, eller *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="145b9-172">The GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span> <span data-ttu-id="145b9-173">VpnType måste vara *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="145b9-173">The VpnType must *RouteBased*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    <span data-ttu-id="145b9-174">Azure VPN-gateway har stöd för BGP-routningsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="145b9-174">Azure VPN gateway supports BGP routing protocol.</span></span> <span data-ttu-id="145b9-175">Du kan ange ASN (AS Number) för det virtuella nätverket genom att lägga till växeln - Asn i följande kommando.</span><span class="sxs-lookup"><span data-stu-id="145b9-175">You can specify ASN (AS Number) for that Virtual Network by adding the -Asn switch in the following command.</span></span> <span data-ttu-id="145b9-176">Om du inte anger parametern blir standardvärdet 65515.</span><span class="sxs-lookup"><span data-stu-id="145b9-176">Not specifying that parameter will default to AS number 65515.</span></span>

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    <span data-ttu-id="145b9-177">Du hittar BGP peering-IP och AS-numret som Azure använder för VPN-gatewayen i $azureVpn.BgpSettings.BgpPeeringAddress och $azureVpn.BgpSettings.Asn.</span><span class="sxs-lookup"><span data-stu-id="145b9-177">You can find the BGP peering IP and the AS number that Azure uses for the VPN gateway in $azureVpn.BgpSettings.BgpPeeringAddress and $azureVpn.BgpSettings.Asn.</span></span> <span data-ttu-id="145b9-178">Mer information finns i [Konfigurera BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) för Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="145b9-178">For more information, see [Configure BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) for Azure VPN gateway.</span></span>
7. <span data-ttu-id="145b9-179">Skapa en lokal plats för VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="145b9-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="145b9-180">Det här kommandot konfigurerar inte din lokala VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="145b9-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="145b9-181">I stället kan du ange lokala gateway-inställningar, som t.ex. offentlig IP-adress och lokalt adressutrymme, så att Azure VPN-gatewayen kan ansluta till den.</span><span class="sxs-lookup"><span data-stu-id="145b9-181">Rather, it allows you to provide the local gateway settings, such as the public IP and the on-premises address space, so that the Azure VPN gateway can connect to it.</span></span>
   
    <span data-ttu-id="145b9-182">Om din lokala VPN-enhet bara stöder statisk routning, kan du konfigurera de statiska vägarna på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="145b9-182">If your local VPN device only supports static routing, you can configure the static routes in the following way:</span></span>

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    <span data-ttu-id="145b9-183">Om din lokala VPN-enhet stöder BGP och du vill aktivera dynamisk routning, måste du veta BGP peering IP och AS-numret som din lokala VPN-enhet använder.</span><span class="sxs-lookup"><span data-stu-id="145b9-183">If your local VPN device supports the BGP and you want to enable dynamic routing, you need to know the BGP peering IP and the AS number that your local VPN device uses.</span></span>

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for the BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. <span data-ttu-id="145b9-184">Konfigurera din lokala VPN-enhet till att ansluta till en ny Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="145b9-184">Configure your local VPN device to connect to the new Azure VPN gateway.</span></span> <span data-ttu-id="145b9-185">Mer information om VPN-enhetskonfiguration finns i [VPN-enhetskonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="145b9-185">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
9. <span data-ttu-id="145b9-186">Länka VPN-gatewayen för plats till plats på Azure till den lokala gatewayen.</span><span class="sxs-lookup"><span data-stu-id="145b9-186">Link the Site-to-Site VPN gateway on Azure to the local gateway.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <span data-ttu-id="145b9-187"><a name="add"></a>Så här konfigurerar du samexisterande anslutningar för ett befintligt VNet</span><span class="sxs-lookup"><span data-stu-id="145b9-187"><a name="add"></a>To configure coexisting connections for an already existing VNet</span></span>
<span data-ttu-id="145b9-188">Om du har ett befintligt virtuellt nätverk, kontrollerar du storleken på gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="145b9-188">If you have an existing virtual network, check the gateway subnet size.</span></span> <span data-ttu-id="145b9-189">Om gateway-undernätet är /28 eller /29, måste du först ta bort den virtuella nätverksgatewayen och öka storleken för gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="145b9-189">If the gateway subnet is /28 or /29, you must first delete the virtual network gateway and increase the gateway subnet size.</span></span> <span data-ttu-id="145b9-190">Stegen i det här avsnittet visar hur du gör.</span><span class="sxs-lookup"><span data-stu-id="145b9-190">The steps in this section show you how to do that.</span></span>

<span data-ttu-id="145b9-191">Om gateway-undernätet är /27 eller större och det virtuella nätverket är anslutet via ExpressRoute, kan du hoppa över stegen nedan och gå vidare till [”Steg 6 – Skapa en VPN-gateway för plats till plats”](#vpngw) i föregående avsnitt. </span><span class="sxs-lookup"><span data-stu-id="145b9-191">If the gateway subnet is /27 or larger and the virtual network is connected via ExpressRoute, you can skip the steps below and proceed to ["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in the previous section.</span></span> 

> [!NOTE]
> <span data-ttu-id="145b9-192">När du tar bort den befintliga gatewayen förlorar dina lokala platser anslutningen till ditt virtuella nätverk medan du arbetar med konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="145b9-192">When you delete the existing gateway, your local premises will lose the connection to your virtual network while you are working on this configuration.</span></span> 
> 
> 

1. <span data-ttu-id="145b9-193">Du måste installera den senaste versionen av Azure PowerShell-cmdletarna.</span><span class="sxs-lookup"><span data-stu-id="145b9-193">You'll need to install the latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="145b9-194">Mer information om hur du installerar cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="145b9-194">For more information about installing cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="145b9-195">De cmdletar som du använder för den här konfigurationen kan se annorlunda ut än de du tidigare använt.</span><span class="sxs-lookup"><span data-stu-id="145b9-195">The cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="145b9-196">Var noga med att använda de cmdletar som anges i instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="145b9-196">Be sure to use the cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="145b9-197">Ta bort den befintliga ExpressRoute- eller VPN-gatewayen för plats till plats.</span><span class="sxs-lookup"><span data-stu-id="145b9-197">Delete the existing ExpressRoute or Site-to-Site VPN gateway.</span></span>

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. <span data-ttu-id="145b9-198">Ta bort gateway-undernätet. </span><span class="sxs-lookup"><span data-stu-id="145b9-198">Delete Gateway Subnet.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="145b9-199">Lägg till ett Gateway-undernät som är /27 eller större.</span><span class="sxs-lookup"><span data-stu-id="145b9-199">Add a Gateway Subnet that is /27 or larger.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="145b9-200">Om du inte har tillräckligt med IP-adresser kvar i det virtuella nätverket för att kunna öka storleken på gateway-undernätet, måste du lägga till mer IP-adressutrymme.</span><span class="sxs-lookup"><span data-stu-id="145b9-200">If you don't have enough IP addresses left in your virtual network to increase the gateway subnet size, you need to add more IP address space.</span></span>
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="145b9-201">Spara VNet-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="145b9-201">Save the VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. <span data-ttu-id="145b9-202">Just nu har du ett VNet utan gateways.</span><span class="sxs-lookup"><span data-stu-id="145b9-202">At this point, you have a VNet with no gateways.</span></span> <span data-ttu-id="145b9-203">Om du vill slutföra dina anslutningar och skapa nya gateways kan du fortsätta med [Steg 4 – Skapa en ExpressRoute-gateway](#gw), som finns i den föregående uppsättningen med steg.</span><span class="sxs-lookup"><span data-stu-id="145b9-203">To create new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in the preceding set of steps.</span></span>

## <a name="to-add-point-to-site-configuration-to-the-vpn-gateway"></a><span data-ttu-id="145b9-204">Så här lägger du till punkt-till-plats-konfiguration till VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="145b9-204">To add point-to-site configuration to the VPN gateway</span></span>
<span data-ttu-id="145b9-205">Du kan följa stegen nedan om du vill lägga till punkt-till-plats-konfiguration för VPN-gateway i en samexisterande inställning.</span><span class="sxs-lookup"><span data-stu-id="145b9-205">You can follow the steps below to add Point-to-Site configuration to your VPN gateway in a co-existence setup.</span></span>

1. <span data-ttu-id="145b9-206">Lägga till VPN-klientadresspoolen.</span><span class="sxs-lookup"><span data-stu-id="145b9-206">Add VPN Client address pool.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. <span data-ttu-id="145b9-207">Överför VPN-rotcertifikatet till Azure för din VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="145b9-207">Upload the VPN root certificate to Azure for your VPN gateway.</span></span> <span data-ttu-id="145b9-208">I det här exemplet förutsätts att rotcertifikatet lagras i den lokala datorn där följande PowerShell-cmdletar körs.</span><span class="sxs-lookup"><span data-stu-id="145b9-208">In this example, it's assumed that the root certificate is stored in the local machine where the following PowerShell cmdlets are run.</span></span>

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

<span data-ttu-id="145b9-209">Mer information om punkt-till-plats-VPN finns i [Konfigurera en punkt-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="145b9-209">For more information on Point-to-Site VPN, see [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="145b9-210">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="145b9-210">Next steps</span></span>
<span data-ttu-id="145b9-211">Mer information om ExpressRoute finns i [Vanliga frågor och svar om ExpressRoute](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="145b9-211">For more information about ExpressRoute, see the [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

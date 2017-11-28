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
ms.openlocfilehash: efda9f89d95617c8c4e75af91b20631dc468d4db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a><span data-ttu-id="b111c-103">Konfigurera ExpressRoute-anslutningar och anslutningar för plats-till-plats som kan samexistera</span><span class="sxs-lookup"><span data-stu-id="b111c-103">Configure ExpressRoute and Site-to-Site coexisting connections</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b111c-104">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b111c-104">PowerShell - Resource Manager</span></span>](expressroute-howto-coexist-resource-manager.md)
> * [<span data-ttu-id="b111c-105">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="b111c-105">PowerShell - Classic</span></span>](expressroute-howto-coexist-classic.md)
> 
> 

<span data-ttu-id="b111c-106">Att konfigurera VPN för plats till plats och samexisterande ExpressRoute-anslutningar har flera fördelar.</span><span class="sxs-lookup"><span data-stu-id="b111c-106">Configuring Site-to-Site VPN and ExpressRoute coexisting connections has several advantages.</span></span> <span data-ttu-id="b111c-107">Du kan konfigurera en plats-till-plats-VPN som en säker redundans sökväg för ExressRoute eller använda plats-till-plats VPN tooconnect toosites som inte är anslutna via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b111c-107">You can configure a Site-to-Site VPN as a secure failover path for ExressRoute, or use Site-to-Site VPNs tooconnect toosites that are not connected through ExpressRoute.</span></span> <span data-ttu-id="b111c-108">Vi upp hello steg tooconfigure båda scenarierna i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b111c-108">We cover hello steps tooconfigure both scenarios in this article.</span></span> <span data-ttu-id="b111c-109">Den här artikeln gäller toohello Resource Manager-modellen och använder PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b111c-109">This article applies toohello Resource Manager deployment model and uses PowerShell.</span></span> <span data-ttu-id="b111c-110">Den här konfigurationen är inte tillgänglig i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b111c-110">This configuration is not available in hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b111c-111">ExpressRoute-kretsar måste konfigureras före innan du följer hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="b111c-111">ExpressRoute circuits must be pre-configured before you follow hello instructions below.</span></span> <span data-ttu-id="b111c-112">Kontrollera att du har följt hello guider för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och [konfigurera routning](expressroute-howto-routing-arm.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="b111c-112">Make sure that you have followed hello guides too[create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and [configure routing](expressroute-howto-routing-arm.md) before you proceed.</span></span>
> 
> 

## <a name="limits-and-limitations"></a><span data-ttu-id="b111c-113">Gränser och begränsningar</span><span class="sxs-lookup"><span data-stu-id="b111c-113">Limits and limitations</span></span>
* <span data-ttu-id="b111c-114">**Transit-routing stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="b111c-114">**Transit routing is not supported.**</span></span> <span data-ttu-id="b111c-115">Du kan inte routa (via Azure) mellan ditt lokala nätverk som ansluter via plats-till-plats-VPN och ditt lokala nätverk som ansluter via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b111c-115">You cannot route (via Azure) between your local network connected via Site-to-Site VPN and your local network connected via ExpressRoute.</span></span>
* <span data-ttu-id="b111c-116">**Basic-SKU-gatewayen stöds inte.**</span><span class="sxs-lookup"><span data-stu-id="b111c-116">**Basic SKU gateway is not supported.**</span></span> <span data-ttu-id="b111c-117">Du måste använda en icke - grundläggande SKU-gateway för båda hello [ExpressRoute-gateway](expressroute-about-virtual-network-gateways.md) och hello [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="b111c-117">You must use a non-Basic SKU gateway for both hello [ExpressRoute gateway](expressroute-about-virtual-network-gateways.md) and hello [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="b111c-118">**Enbart routebaserad VPN-gateway stöds.**</span><span class="sxs-lookup"><span data-stu-id="b111c-118">**Only route-based VPN gateway is supported.**</span></span> <span data-ttu-id="b111c-119">Du måste använda en routebaserad [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="b111c-119">You must use a route-based [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>
* <span data-ttu-id="b111c-120">**Statiska vägar ska konfigureras för din VPN-gateway.**</span><span class="sxs-lookup"><span data-stu-id="b111c-120">**Static route should be configured for your VPN gateway.**</span></span> <span data-ttu-id="b111c-121">Om nätverket är anslutet tooboth ExpressRoute och en plats-till-plats VPN, måste du ha en statisk väg som konfigurerats i ditt lokala nätverk tooroute hello plats-till-plats VPN-anslutningen toohello offentliga Internet.</span><span class="sxs-lookup"><span data-stu-id="b111c-121">If your local network is connected tooboth ExpressRoute and a Site-to-Site VPN, you must have a static route configured in your local network tooroute hello Site-to-Site VPN connection toohello public Internet.</span></span>
* <span data-ttu-id="b111c-122">**ExpressRoute-gateway måste konfigureras först och länka tooa krets.**</span><span class="sxs-lookup"><span data-stu-id="b111c-122">**ExpressRoute gateway must be configured first and linked tooa circuit.**</span></span> <span data-ttu-id="b111c-123">Du måste först skapa hello ExpressRoute-gateway och länka det tooa krets innan du lägger till hello plats-till-plats VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="b111c-123">You must create hello ExpressRoute gateway first and link it tooa circuit before you add hello Site-to-Site VPN gateway.</span></span>

## <a name="configuration-designs"></a><span data-ttu-id="b111c-124">Konfigurationsdesign</span><span class="sxs-lookup"><span data-stu-id="b111c-124">Configuration designs</span></span>
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a><span data-ttu-id="b111c-125">Konfigurera en VPN för plats till plats som en redundanssökväg för ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="b111c-125">Configure a Site-to-Site VPN as a failover path for ExpressRoute</span></span>
<span data-ttu-id="b111c-126">Du kan konfigurera en VPN-anslutning för plats till plats som en säkerhetskopia av ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b111c-126">You can configure a Site-to-Site VPN connection as a backup for ExpressRoute.</span></span> <span data-ttu-id="b111c-127">Detta gäller endast toovirtual nätverk länkade toohello Azure privat peering sökväg.</span><span class="sxs-lookup"><span data-stu-id="b111c-127">This applies only toovirtual networks linked toohello Azure private peering path.</span></span> <span data-ttu-id="b111c-128">Det finns ingen VPN-baserad redundanslösning för tjänster som är tillgängliga via Azures offentliga och Microsofts peerings.</span><span class="sxs-lookup"><span data-stu-id="b111c-128">There is no VPN-based failover solution for services accessible through Azure public and Microsoft peerings.</span></span> <span data-ttu-id="b111c-129">Hej ExpressRoute-kretsen är alltid hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="b111c-129">hello ExpressRoute circuit is always hello primary link.</span></span> <span data-ttu-id="b111c-130">Data flödar genom hello plats-till-plats VPN-väg endast om hello ExpressRoute-krets misslyckades.</span><span class="sxs-lookup"><span data-stu-id="b111c-130">Data flows through hello Site-to-Site VPN path only if hello ExpressRoute circuit fails.</span></span>

> [!NOTE]
> <span data-ttu-id="b111c-131">ExpressRoute-kretsen är prioriterade via plats-till-plats VPN när båda vägar är hello samma, använder Azure hello längsta prefix matchar toochoose hello vägen mot hello paketets mål.</span><span class="sxs-lookup"><span data-stu-id="b111c-131">While ExpressRoute circuit is preferred over Site-to-Site VPN when both routes are hello same, Azure will use hello longest prefix match toochoose hello route towards hello packet's destination.</span></span>
> 
> 

![Samexistera](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a><span data-ttu-id="b111c-133">Konfigurera en plats-till-plats VPN-tooconnect toosites som inte är anslutna via ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="b111c-133">Configure a Site-to-Site VPN tooconnect toosites not connected through ExpressRoute</span></span>
<span data-ttu-id="b111c-134">Du kan konfigurera vissa platser ansluter direkt tooAzure via plats-till-plats VPN och vissa platser ansluter via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b111c-134">You can configure your network where some sites connect directly tooAzure over Site-to-Site VPN, and some sites connect through ExpressRoute.</span></span> 

![Samexistera](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> <span data-ttu-id="b111c-136">Det går inte att konfigurera ett virtuellt nätverk som en överföringsrouter.</span><span class="sxs-lookup"><span data-stu-id="b111c-136">You cannot configure a virtual network as a transit router.</span></span>
> 
> 

## <a name="selecting-hello-steps-toouse"></a><span data-ttu-id="b111c-137">Att välja hello steg toouse</span><span class="sxs-lookup"><span data-stu-id="b111c-137">Selecting hello steps toouse</span></span>
<span data-ttu-id="b111c-138">Det finns två olika uppsättningar av procedurer toochoose från.</span><span class="sxs-lookup"><span data-stu-id="b111c-138">There are two different sets of procedures toochoose from.</span></span> <span data-ttu-id="b111c-139">hello configuration procedur som du väljer beror på om du har ett befintligt virtuellt nätverk som du vill tooconnect till, eller önskade toocreate ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b111c-139">hello configuration procedure that you select depends on whether you have an existing virtual network that you want tooconnect to, or you want toocreate a new virtual network.</span></span>

* <span data-ttu-id="b111c-140">Jag inte har ett VNet och behöver toocreate en.</span><span class="sxs-lookup"><span data-stu-id="b111c-140">I don't have a VNet and need toocreate one.</span></span>
  
    <span data-ttu-id="b111c-141">Om du inte redan har ett virtuellt nätverk kan den här proceduren hjälpa dig att skapa ett nytt virtuellt nätverk med hjälp av distributionsmodellen i Resource Manager samt skapa nya ExpressRoute- och plats-till-plats-VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="b111c-141">If you don’t already have a virtual network, this procedure walks you through creating a new virtual network using Resource Manager deployment model and creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="b111c-142">tooconfigure ett virtuellt nätverk gör hello i [toocreate ett nytt virtuellt nätverk och samtidiga anslutningar](#new).</span><span class="sxs-lookup"><span data-stu-id="b111c-142">tooconfigure a virtual network, follow hello steps in [toocreate a new virtual network and coexisting connections](#new).</span></span>
* <span data-ttu-id="b111c-143">Jag har redan en distributionsmodell för Resource Manager i VNet.</span><span class="sxs-lookup"><span data-stu-id="b111c-143">I already have a Resource Manager deployment model VNet.</span></span>
  
    <span data-ttu-id="b111c-144">Du kanske redan har ett virtuellt nätverk på plats med en befintlig VPN-anslutning för plats till plats eller en ExpressRoute-anslutning.</span><span class="sxs-lookup"><span data-stu-id="b111c-144">You may already have a virtual network in place with an existing Site-to-Site VPN connection or ExpressRoute connection.</span></span> <span data-ttu-id="b111c-145">Hej [tooconfigure samtidiga anslutningar för ett redan existerande VNet](#add) avsnitt vägleder dig genom att ta bort hello gateway och sedan skapa nya ExpressRoute- och plats-till-plats VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="b111c-145">hello [tooconfigure coexisting connections for an already existing VNet](#add) section walks you through deleting hello gateway, and then creating new ExpressRoute and Site-to-Site VPN connections.</span></span> <span data-ttu-id="b111c-146">När du skapar hello nya anslutningar, måste hello steg slutföras i en viss ordning.</span><span class="sxs-lookup"><span data-stu-id="b111c-146">When creating hello new connections, hello steps must be completed in a specific order.</span></span> <span data-ttu-id="b111c-147">Använd inte hello instruktionerna i andra artiklar toocreate din gateway och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="b111c-147">Don't use hello instructions in other articles toocreate your gateways and connections.</span></span>
  
    <span data-ttu-id="b111c-148">I den här proceduren skapar anslutningar som kan finnas kräver du toodelete din gateway och konfigurera nya gatewayer.</span><span class="sxs-lookup"><span data-stu-id="b111c-148">In this procedure, creating connections that can coexist requires you toodelete your gateway, and then configure new gateways.</span></span> <span data-ttu-id="b111c-149">Du måste driftstopp för anslutningar mellan platser när du tar bort och återskapa din gateway och anslutningar, men du behöver inte toomigrate någon av dina virtuella datorer eller tjänster tooa nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b111c-149">You will have downtime for your cross-premises connections while you delete and recreate your gateway and connections, but you will not need toomigrate any of your VMs or services tooa new virtual network.</span></span> <span data-ttu-id="b111c-150">Dina virtuella datorer och tjänster kommer fortfarande att kunna toocommunicate ut via hello belastningsutjämnare när du konfigurerar din gateway om de är konfigurerade toodo så.</span><span class="sxs-lookup"><span data-stu-id="b111c-150">Your VMs and services will still be able toocommunicate out through hello load balancer while you configure your gateway if they are configured toodo so.</span></span>

## <span data-ttu-id="b111c-151"><a name="new"></a>toocreate ett nytt virtuellt nätverk och samtidiga anslutningar</span><span class="sxs-lookup"><span data-stu-id="b111c-151"><a name="new"></a>toocreate a new virtual network and coexisting connections</span></span>
<span data-ttu-id="b111c-152">Den här proceduren vägleder dig genom att skapa ett VNet samt plats-till-plats- och ExpressRoute-anslutningar som ska finnas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="b111c-152">This procedure walks you through creating a VNet and Site-to-Site and ExpressRoute connections that will coexist.</span></span>

1. <span data-ttu-id="b111c-153">Installera hello senaste versionen av hello Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="b111c-153">Install hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="b111c-154">Information om hur du installerar hello cmdlets finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b111c-154">For information about installing hello cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="b111c-155">hello-cmdletar som du använder för den här konfigurationen kan vara något annorlunda än vad du kan känna till.</span><span class="sxs-lookup"><span data-stu-id="b111c-155">hello cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="b111c-156">Anges till toouse hello-cmdlets i dessa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="b111c-156">Be sure toouse hello cmdlets specified in these instructions.</span></span>
2. <span data-ttu-id="b111c-157">Logga in tooyour konto och konfigurera hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="b111c-157">Log in tooyour account and set up hello environment.</span></span>

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. <span data-ttu-id="b111c-158">Skapa ett virtuellt nätverk, inklusive gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="b111c-158">Create a virtual network including Gateway Subnet.</span></span> <span data-ttu-id="b111c-159">Mer information om konfiguration av virtuellt nätverk hello finns [Azure Virtual Network configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b111c-159">For more information about hello virtual network configuration, see [Azure Virtual Network configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="b111c-160">hello Gateway-undernätet måste vara minst/27 eller ett kortare prefix (till exempel /26 eller /25).</span><span class="sxs-lookup"><span data-stu-id="b111c-160">hello Gateway Subnet must be /27 or a shorter prefix (such as /26 or /25).</span></span>
   > 
   > 
   
    <span data-ttu-id="b111c-161">Skapa ett nytt VNet.</span><span class="sxs-lookup"><span data-stu-id="b111c-161">Create a new VNet.</span></span>

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    <span data-ttu-id="b111c-162">Lägg till undernät.</span><span class="sxs-lookup"><span data-stu-id="b111c-162">Add subnets.</span></span>

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="b111c-163">Spara konfigurationen för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b111c-163">Save hello VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="b111c-164"><a name="gw"></a>Skapa en ExpressRoute-gateway.</span><span class="sxs-lookup"><span data-stu-id="b111c-164"><a name="gw"></a>Create an ExpressRoute gateway.</span></span> <span data-ttu-id="b111c-165">Mer information om gateway-konfiguration för hello ExpressRoute finns [ExpressRoute gatewaykonfigurationen](expressroute-howto-add-gateway-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b111c-165">For more information about hello ExpressRoute gateway configuration, see [ExpressRoute gateway configuration](expressroute-howto-add-gateway-resource-manager.md).</span></span> <span data-ttu-id="b111c-166">Hej GatewaySKU måste vara *Standard*, *HighPerformance*, eller *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="b111c-166">hello GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. <span data-ttu-id="b111c-167">Länka hello ExpressRoute-gateway toohello ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="b111c-167">Link hello ExpressRoute gateway toohello ExpressRoute circuit.</span></span> <span data-ttu-id="b111c-168">Det här steget har slutförts, upprättas hello anslutning mellan ditt lokala nätverk och Azure via ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="b111c-168">After this step has been completed, hello connection between your on-premises network and Azure, through ExpressRoute, is established.</span></span> <span data-ttu-id="b111c-169">Läs mer om hello länkåtgärder [länk Vnet tooExpressRoute](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="b111c-169">For more information about hello link operation, see [Link VNets tooExpressRoute](expressroute-howto-linkvnet-arm.md).</span></span>

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <span data-ttu-id="b111c-170"><a name="vpngw"></a>Därefter skapar du din plats-till-plats-VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="b111c-170"><a name="vpngw"></a>Next, create your Site-to-Site VPN gateway.</span></span> <span data-ttu-id="b111c-171">Mer information om VPN-gateway-konfiguration för hello finns [konfigurera ett virtuellt nätverk med en plats-till-plats-anslutning](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b111c-171">For more information about hello VPN gateway configuration, see [Configure a VNet with a Site-to-Site connection](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).</span></span> <span data-ttu-id="b111c-172">Hej GatewaySKU måste vara *Standard*, *HighPerformance*, eller *UltraPerformance*.</span><span class="sxs-lookup"><span data-stu-id="b111c-172">hello GatewaySKU must be *Standard*, *HighPerformance*, or *UltraPerformance*.</span></span> <span data-ttu-id="b111c-173">Hej VpnType måste *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="b111c-173">hello VpnType must *RouteBased*.</span></span>

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    <span data-ttu-id="b111c-174">Azure VPN-gateway har stöd för BGP-routningsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="b111c-174">Azure VPN gateway supports BGP routing protocol.</span></span> <span data-ttu-id="b111c-175">Du kan ange ASN (AS Number) för det virtuella nätverket genom att lägga till hello Asn - växel i hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="b111c-175">You can specify ASN (AS Number) for that Virtual Network by adding hello -Asn switch in hello following command.</span></span> <span data-ttu-id="b111c-176">Ange inte parametern kommer standard tooAS number 65515.</span><span class="sxs-lookup"><span data-stu-id="b111c-176">Not specifying that parameter will default tooAS number 65515.</span></span>

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    <span data-ttu-id="b111c-177">Du kan hitta hello BGP-peering IP och hello som nummer som Azure använder för hello VPN-gateway i $azureVpn.BgpSettings.BgpPeeringAddress och $azureVpn.BgpSettings.Asn.</span><span class="sxs-lookup"><span data-stu-id="b111c-177">You can find hello BGP peering IP and hello AS number that Azure uses for hello VPN gateway in $azureVpn.BgpSettings.BgpPeeringAddress and $azureVpn.BgpSettings.Asn.</span></span> <span data-ttu-id="b111c-178">Mer information finns i [Konfigurera BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) för Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="b111c-178">For more information, see [Configure BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) for Azure VPN gateway.</span></span>
7. <span data-ttu-id="b111c-179">Skapa en lokal plats för VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="b111c-179">Create a local site VPN gateway entity.</span></span> <span data-ttu-id="b111c-180">Det här kommandot konfigurerar inte din lokala VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="b111c-180">This command doesn’t configure your on-premises VPN gateway.</span></span> <span data-ttu-id="b111c-181">I stället kan du tooprovide hello lokala gateway-inställningar, till exempel hello offentlig IP-adress och hello lokala adressutrymmet, så att hello Azure VPN-gateway kan ansluta tooit.</span><span class="sxs-lookup"><span data-stu-id="b111c-181">Rather, it allows you tooprovide hello local gateway settings, such as hello public IP and hello on-premises address space, so that hello Azure VPN gateway can connect tooit.</span></span>
   
    <span data-ttu-id="b111c-182">Om din lokala VPN-enhet stöder endast statisk routning, kan du konfigurera hello statiska vägar i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b111c-182">If your local VPN device only supports static routing, you can configure hello static routes in hello following way:</span></span>

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    <span data-ttu-id="b111c-183">Om din lokala VPN-enhet stöder hello BGP och du vill tooenable dynamisk routning, måste tooknow hello BGP peering IP-adress och hello som number som din lokala VPN-enhet använder.</span><span class="sxs-lookup"><span data-stu-id="b111c-183">If your local VPN device supports hello BGP and you want tooenable dynamic routing, you need tooknow hello BGP peering IP and hello AS number that your local VPN device uses.</span></span>

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for hello BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. <span data-ttu-id="b111c-184">Konfigurera din lokala VPN-enhet tooconnect toohello nya Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="b111c-184">Configure your local VPN device tooconnect toohello new Azure VPN gateway.</span></span> <span data-ttu-id="b111c-185">Mer information om VPN-enhetskonfiguration finns i [VPN-enhetskonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span><span class="sxs-lookup"><span data-stu-id="b111c-185">For more information about VPN device configuration, see [VPN Device Configuration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).</span></span>
9. <span data-ttu-id="b111c-186">Länken hello plats-till-plats VPN-gateway på Azure toohello lokala gateway.</span><span class="sxs-lookup"><span data-stu-id="b111c-186">Link hello Site-to-Site VPN gateway on Azure toohello local gateway.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <span data-ttu-id="b111c-187"><a name="add"></a>tooconfigure samtidiga anslutningar för ett befintligt virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="b111c-187"><a name="add"></a>tooconfigure coexisting connections for an already existing VNet</span></span>
<span data-ttu-id="b111c-188">Om du har ett befintligt virtuellt nätverk kontrollerar du hello gateway undernätets storlek.</span><span class="sxs-lookup"><span data-stu-id="b111c-188">If you have an existing virtual network, check hello gateway subnet size.</span></span> <span data-ttu-id="b111c-189">Om hello gateway-undernät är /28 eller /29, måste du först ta bort hello virtuell nätverksgateway och öka storleken på hello gateway-undernät.</span><span class="sxs-lookup"><span data-stu-id="b111c-189">If hello gateway subnet is /28 or /29, you must first delete hello virtual network gateway and increase hello gateway subnet size.</span></span> <span data-ttu-id="b111c-190">hello steg i det här avsnittet visar hur toodo som.</span><span class="sxs-lookup"><span data-stu-id="b111c-190">hello steps in this section show you how toodo that.</span></span>

<span data-ttu-id="b111c-191">Om hello gateway-undernät är minst/27 eller större och hello virtuellt nätverk är anslutna via ExpressRoute kan du hoppa över hello stegen nedan och fortsätta för[”steg 6 – skapa en plats-till-plats VPN-gateway”](#vpngw) hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b111c-191">If hello gateway subnet is /27 or larger and hello virtual network is connected via ExpressRoute, you can skip hello steps below and proceed too["Step 6 - Create a Site-to-Site VPN gateway"](#vpngw) in hello previous section.</span></span> 

> [!NOTE]
> <span data-ttu-id="b111c-192">När du tar bort hello befintlig gateway förlorar din lokala lokala hello anslutning tooyour virtuellt nätverk när du arbetar med den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="b111c-192">When you delete hello existing gateway, your local premises will lose hello connection tooyour virtual network while you are working on this configuration.</span></span> 
> 
> 

1. <span data-ttu-id="b111c-193">Du behöver tooinstall hello senaste versionen av hello Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="b111c-193">You'll need tooinstall hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="b111c-194">Mer information om hur du installerar cmdlets finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b111c-194">For more information about installing cmdlets, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="b111c-195">hello-cmdletar som du använder för den här konfigurationen kan vara något annorlunda än vad du kan känna till.</span><span class="sxs-lookup"><span data-stu-id="b111c-195">hello cmdlets that you use for this configuration may be slightly different than what you might be familiar with.</span></span> <span data-ttu-id="b111c-196">Anges till toouse hello-cmdlets i dessa instruktioner.</span><span class="sxs-lookup"><span data-stu-id="b111c-196">Be sure toouse hello cmdlets specified in these instructions.</span></span> 
2. <span data-ttu-id="b111c-197">Ta bort hello befintlig ExpressRoute eller plats-till-plats VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="b111c-197">Delete hello existing ExpressRoute or Site-to-Site VPN gateway.</span></span>

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. <span data-ttu-id="b111c-198">Ta bort gateway-undernätet. </span><span class="sxs-lookup"><span data-stu-id="b111c-198">Delete Gateway Subnet.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. <span data-ttu-id="b111c-199">Lägg till ett Gateway-undernät som är /27 eller större.</span><span class="sxs-lookup"><span data-stu-id="b111c-199">Add a Gateway Subnet that is /27 or larger.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b111c-200">Om du inte har tillräckligt med IP-adresser som är kvar i ditt virtuella nätverk tooincrease hello gateway undernätets storlek, måste tooadd flera IP-adressutrymme.</span><span class="sxs-lookup"><span data-stu-id="b111c-200">If you don't have enough IP addresses left in your virtual network tooincrease hello gateway subnet size, you need tooadd more IP address space.</span></span>
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    <span data-ttu-id="b111c-201">Spara konfigurationen för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b111c-201">Save hello VNet configuration.</span></span>

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. <span data-ttu-id="b111c-202">Just nu har du ett VNet utan gateways.</span><span class="sxs-lookup"><span data-stu-id="b111c-202">At this point, you have a VNet with no gateways.</span></span> <span data-ttu-id="b111c-203">toocreate nya gatewayer och slutföra dina anslutningar, du kan fortsätta med [steg 4: skapa en ExpressRoute-gateway](#gw)hittades i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="b111c-203">toocreate new gateways and complete your connections, you can proceed with [Step 4 - Create an ExpressRoute gateway](#gw), found in hello preceding set of steps.</span></span>

## <a name="tooadd-point-to-site-configuration-toohello-vpn-gateway"></a><span data-ttu-id="b111c-204">tooadd punkt-till-plats configuration toohello VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="b111c-204">tooadd point-to-site configuration toohello VPN gateway</span></span>
<span data-ttu-id="b111c-205">Du kan följa hello stegen nedan tooadd punkt-till-plats configuration tooyour VPN-gateway i en inställning för existerar.</span><span class="sxs-lookup"><span data-stu-id="b111c-205">You can follow hello steps below tooadd Point-to-Site configuration tooyour VPN gateway in a co-existence setup.</span></span>

1. <span data-ttu-id="b111c-206">Lägga till VPN-klientadresspoolen.</span><span class="sxs-lookup"><span data-stu-id="b111c-206">Add VPN Client address pool.</span></span>

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. <span data-ttu-id="b111c-207">Överför hello VPN root certificate tooAzure för VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="b111c-207">Upload hello VPN root certificate tooAzure for your VPN gateway.</span></span> <span data-ttu-id="b111c-208">I det här exemplet förutsätts att hello rotcertifikat lagras i hello lokala dator där hello följande PowerShell-cmdlets körs.</span><span class="sxs-lookup"><span data-stu-id="b111c-208">In this example, it's assumed that hello root certificate is stored in hello local machine where hello following PowerShell cmdlets are run.</span></span>

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

<span data-ttu-id="b111c-209">Mer information om punkt-till-plats-VPN finns i [Konfigurera en punkt-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b111c-209">For more information on Point-to-Site VPN, see [Configure a Point-to-Site connection](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b111c-210">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b111c-210">Next steps</span></span>
<span data-ttu-id="b111c-211">Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="b111c-211">For more information about ExpressRoute, see hello [ExpressRoute FAQ](expressroute-faqs.md).</span></span>

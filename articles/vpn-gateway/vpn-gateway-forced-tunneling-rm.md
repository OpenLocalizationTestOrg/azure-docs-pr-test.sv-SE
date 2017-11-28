---
title: "Konfigurera Tvingad tunneltrafik för Azure plats-till-plats-anslutningar: Resource Manager | Microsoft Docs"
description: "Så här dirigerar eller 'tvinga' alla Internet-bunden trafik tillbaka till den lokala platsen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: 207c53924863eb51ee369fe46d5ad12fb1905c53
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a><span data-ttu-id="5b350-103">Konfigurera framtvingad tunneling med distributionsmodellen Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5b350-103">Configure forced tunneling using the Azure Resource Manager deployment model</span></span>

<span data-ttu-id="5b350-104">Tvingad tunneling kan du omdirigera eller ”force” alla Internet-bunden trafik tillbaka till din lokala plats via en plats-till-plats VPN-tunnel för kontroll och granskning.</span><span class="sxs-lookup"><span data-stu-id="5b350-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back to your on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="5b350-105">Detta är en kritisk säkerhetskraven för de flesta företags IT principer.</span><span class="sxs-lookup"><span data-stu-id="5b350-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="5b350-106">Utan Tvingad tunneltrafik, Internet-bunden trafik från din virtuella datorer i Azure alltid går från Azure nätverksinfrastruktur direkt ut till Internet, utan att du vill granska eller granska trafiken.</span><span class="sxs-lookup"><span data-stu-id="5b350-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure always traverses from Azure network infrastructure directly out to the Internet, without the option to allow you to inspect or audit the traffic.</span></span> <span data-ttu-id="5b350-107">Obehörig Internetåtkomst kan leda till avslöjande av information och andra typer av säkerhetsintrång.</span><span class="sxs-lookup"><span data-stu-id="5b350-107">Unauthorized Internet access can potentially lead to information disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

<span data-ttu-id="5b350-108">Den här artikeln beskriver hur du konfigurerar Tvingad tunneltrafik för virtuella nätverk som skapats med hjälp av Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="5b350-108">This article walks you through configuring forced tunneling for virtual networks created using the Resource Manager deployment model.</span></span> <span data-ttu-id="5b350-109">Tvingad tunneltrafik kan konfigureras med hjälp av PowerShell inte via portalen.</span><span class="sxs-lookup"><span data-stu-id="5b350-109">Forced tunneling can be configured by using PowerShell, not through the portal.</span></span> <span data-ttu-id="5b350-110">Om du vill konfigurera Tvingad tunneling för den klassiska distributionsmodellen Välj klassiska artikel från listan följande:</span><span class="sxs-lookup"><span data-stu-id="5b350-110">If you want to configure forced tunneling for the classic deployment model, select classic article from the following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5b350-111">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="5b350-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="5b350-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5b350-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a><span data-ttu-id="5b350-113">Om Tvingad tunneltrafik</span><span class="sxs-lookup"><span data-stu-id="5b350-113">About forced tunneling</span></span>

<span data-ttu-id="5b350-114">Följande diagram illustrerar hur Tvingad tunneltrafik fungerar.</span><span class="sxs-lookup"><span data-stu-id="5b350-114">The following diagram illustrates how forced tunneling works.</span></span> 

![Forcerade tunnlar](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

<span data-ttu-id="5b350-116">I exemplet ovan tunneldata klientdel undernät inte tvingas.</span><span class="sxs-lookup"><span data-stu-id="5b350-116">In the example above, the Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="5b350-117">Arbetsbelastningar i undernätet Frontend kan fortsätta att acceptera och svara på kundernas önskemål direkt från Internet.</span><span class="sxs-lookup"><span data-stu-id="5b350-117">The workloads in the Frontend subnet can continue to accept and respond to customer requests from the Internet directly.</span></span> <span data-ttu-id="5b350-118">Halva nivå och Backend-undernät tvingas tunnel.</span><span class="sxs-lookup"><span data-stu-id="5b350-118">The Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="5b350-119">Alla utgående anslutningar från dessa två undernät till Internet ska tvingas eller omdirigeras till en lokal plats via en S2S VPN-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="5b350-119">Any outbound connections from these two subnets to the Internet will be forced or redirected back to an on-premises site via one of the S2S VPN tunnels.</span></span>

<span data-ttu-id="5b350-120">På så sätt kan du begränsa och inspektera Internetåtkomst från dina virtuella datorer eller molntjänster i Azure, medan du aktivera din flernivåtjänst arkitektur som krävs.</span><span class="sxs-lookup"><span data-stu-id="5b350-120">This allows you to restrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing to enable your multi-tier service architecture required.</span></span> <span data-ttu-id="5b350-121">Om det finns ingen Internet-riktade arbetsbelastningar i dina virtuella nätverk, kan du också använda Tvingad tunneling till hela virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="5b350-121">If there are no Internet-facing workloads in your virtual networks, you also can apply forced tunneling to the entire virtual networks.</span></span>

## <a name="requirements-and-considerations"></a><span data-ttu-id="5b350-122">Krav och överväganden</span><span class="sxs-lookup"><span data-stu-id="5b350-122">Requirements and considerations</span></span>

<span data-ttu-id="5b350-123">Tvingad tunneling i Azure konfigureras via användardefinierade vägar för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="5b350-123">Forced tunneling in Azure is configured via virtual network user-defined routes.</span></span> <span data-ttu-id="5b350-124">Omdirigera trafik till en lokal plats uttrycks som en standardväg till Azure VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="5b350-124">Redirecting traffic to an on-premises site is expressed as a Default Route to the Azure VPN gateway.</span></span> <span data-ttu-id="5b350-125">Mer information om användardefinierade Routning och virtuella nätverk finns [användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5b350-125">For more information about user-defined routing and virtual networks, see [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>

* <span data-ttu-id="5b350-126">Varje undernät för virtuellt nätverk har en inbyggd, system-routningstabell.</span><span class="sxs-lookup"><span data-stu-id="5b350-126">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="5b350-127">Routningstabellen för systemet har följande tre grupper av vägar:</span><span class="sxs-lookup"><span data-stu-id="5b350-127">The system routing table has the following three groups of routes:</span></span>
  
  * <span data-ttu-id="5b350-128">**Lokal VNet vägar:** direkt till målet virtuella datorer i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="5b350-128">**Local VNet routes:** Directly to the destination VMs in the same virtual network.</span></span>
  * <span data-ttu-id="5b350-129">**Lokala vägar:** till Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="5b350-129">**On-premises routes:** To the Azure VPN gateway.</span></span>
  * <span data-ttu-id="5b350-130">**Standardvägen:** direkt till Internet.</span><span class="sxs-lookup"><span data-stu-id="5b350-130">**Default route:** Directly to the Internet.</span></span> <span data-ttu-id="5b350-131">Paket avsedda för de privata IP-adresser som inte omfattas av föregående två vägar tas bort.</span><span class="sxs-lookup"><span data-stu-id="5b350-131">Packets destined to the private IP addresses not covered by the previous two routes are dropped.</span></span>
* <span data-ttu-id="5b350-132">Den här proceduren använder användardefinierade vägar (UDR) för att skapa en routningstabell för att lägga till en standardväg och sedan associera routningstabellen för din VNet-adressundernät för att aktivera Tvingad tunneltrafik på dessa undernät.</span><span class="sxs-lookup"><span data-stu-id="5b350-132">This procedure uses user-defined routes (UDR) to create a routing table to add a default route, and then associate the routing table to your VNet subnet(s) to enable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="5b350-133">Tvingad tunneling måste vara kopplad till ett virtuellt nätverk som har en ruttbaserad VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="5b350-133">Forced tunneling must be associated with a VNet that has a route-based VPN gateway.</span></span> <span data-ttu-id="5b350-134">Du måste ange en ”standardwebbplats” bland de lokala mellan lokala platserna anslutna till det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="5b350-134">You need to set a "default site" among the cross-premises local sites connected to the virtual network.</span></span> <span data-ttu-id="5b350-135">Lokala VPN-enheten måste också konfigureras med 0.0.0.0/0 som trafik väljare.</span><span class="sxs-lookup"><span data-stu-id="5b350-135">Also, the on-premises VPN device must be configured using 0.0.0.0/0 as traffic selectors.</span></span> 
* <span data-ttu-id="5b350-136">ExpressRoute Tvingad tunneltrafik konfigureras inte via den här mekanismen, men har aktiverats genom att annonsera en standardväg via ExpressRoute BGP-peeringsessioner i stället.</span><span class="sxs-lookup"><span data-stu-id="5b350-136">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via the ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="5b350-137">Mer information finns i [ExpressRoute dokumentation](https://azure.microsoft.com/documentation/services/expressroute/).</span><span class="sxs-lookup"><span data-stu-id="5b350-137">For more information, see the [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/).</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="5b350-138">Konfigurationsöversikt</span><span class="sxs-lookup"><span data-stu-id="5b350-138">Configuration overview</span></span>

<span data-ttu-id="5b350-139">Följande procedur kan du skapa en resursgrupp och ett VNet.</span><span class="sxs-lookup"><span data-stu-id="5b350-139">The following procedure helps you create a resource group and a VNet.</span></span> <span data-ttu-id="5b350-140">Sedan skapar en VPN-gateway och konfigurera Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="5b350-140">You'll then create a VPN gateway and configure forced tunneling.</span></span> <span data-ttu-id="5b350-141">I den här proceduren har tre undernät i det virtuella nätverket MultiTier-VNet: 'Klientdel', 'Midtier' och 'Backend' med fyra mellan lokala anslutningar: 'DefaultSiteHQ' och tre filialer.</span><span class="sxs-lookup"><span data-stu-id="5b350-141">In this procedure, the virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend', with four cross-premises connections: 'DefaultSiteHQ', and three Branches.</span></span>

<span data-ttu-id="5b350-142">Anvisningarna för proceduren 'DefaultSiteHQ' som standard plats-anslutning för Tvingad tunneltrafik, och konfigurera Midtier och 'Backend-undernät att använda Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="5b350-142">The procedure steps set the 'DefaultSiteHQ' as the default site connection for forced tunneling, and configure the 'Midtier' and 'Backend' subnets to use forced tunneling.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5b350-143">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="5b350-143">Before you begin</span></span>

<span data-ttu-id="5b350-144">Installera den senaste versionen av Azure Resource Managers PowerShell-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="5b350-144">Install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="5b350-145">Mer information om hur man installerar PowerShell-cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5b350-145">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

### <a name="to-log-in"></a><span data-ttu-id="5b350-146">Logga in</span><span class="sxs-lookup"><span data-stu-id="5b350-146">To log in</span></span>

[!INCLUDE [To log in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a><span data-ttu-id="5b350-147">Konfigurera tvingad tunneltrafik</span><span class="sxs-lookup"><span data-stu-id="5b350-147">Configure forced tunneling</span></span>

1. <span data-ttu-id="5b350-148">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="5b350-148">Create a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. <span data-ttu-id="5b350-149">Skapa ett virtuellt nätverk och ange undernät.</span><span class="sxs-lookup"><span data-stu-id="5b350-149">Create a virtual network and specify subnets.</span></span>

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. <span data-ttu-id="5b350-150">Skapa de lokala nätverksgatewayerna.</span><span class="sxs-lookup"><span data-stu-id="5b350-150">Create the local network gateways.</span></span>

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. <span data-ttu-id="5b350-151">Skapa routningstabellen och väg regeln.</span><span class="sxs-lookup"><span data-stu-id="5b350-151">Create the route table and route rule.</span></span>

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. <span data-ttu-id="5b350-152">Associera routningstabellen till Midtier och Backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="5b350-152">Associate the route table to the Midtier and Backend subnets.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. <span data-ttu-id="5b350-153">Skapa en Gateway med en standardwebbplats.</span><span class="sxs-lookup"><span data-stu-id="5b350-153">Create the Gateway with a default site.</span></span> <span data-ttu-id="5b350-154">Det här steget tar en stund att slutföra ibland 45 minuter eller mer, eftersom du skapar och konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="5b350-154">This step takes some time to complete, sometimes 45 minutes or more, because you are creating and configuring the gateway.</span></span><br> <span data-ttu-id="5b350-155">Den **- GatewayDefaultSite** är cmdlet-parameter som tillåter framtvingad routningskonfiguration att fungera, så var noga med för att konfigurera den här inställningen korrekt.</span><span class="sxs-lookup"><span data-stu-id="5b350-155">The **-GatewayDefaultSite** is the cmdlet parameter that allows the forced routing configuration to work, so take care to configure this setting properly.</span></span> <span data-ttu-id="5b350-156">Den här parametern är tillgängliga i PowerShell 1.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5b350-156">This parameter is available in PowerShell 1.0 or later.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. <span data-ttu-id="5b350-157">Upprätta plats-till-plats VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="5b350-157">Establish the Site-to-Site VPN connections.</span></span>

  ```powershell
  $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
  $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
  $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
  $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
  $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
  Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
  ```
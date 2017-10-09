---
title: "Konfigurera Tvingad tunneltrafik för Azure plats-till-plats-anslutningar: Resource Manager | Microsoft Docs"
description: Hur tooredirect eller 'tvinga' alla Internet-bunden trafik tillbaka tooyour lokal plats.
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
ms.openlocfilehash: 6bc52c04ab0749a674c9863be5e4f9a9f7c98df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-azure-resource-manager-deployment-model"></a><span data-ttu-id="f51f4-103">Konfigurera Tvingad tunneling med hello Azure Resource Manager-distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="f51f4-103">Configure forced tunneling using hello Azure Resource Manager deployment model</span></span>

<span data-ttu-id="f51f4-104">Tvingad tunneltrafik kan du omdirigera eller ”force” alla Internet-bunden trafik tillbaka tooyour lokal plats via en plats-till-plats VPN-tunnel för kontroll och granskning.</span><span class="sxs-lookup"><span data-stu-id="f51f4-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back tooyour on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="f51f4-105">Detta är en kritisk säkerhetskraven för de flesta företags IT principer.</span><span class="sxs-lookup"><span data-stu-id="f51f4-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="f51f4-106">Utan Tvingad tunneling passerar Internet-bunden trafik från din virtuella datorer i Azure alltid från Azure nätverksinfrastruktur direkt ut toohello Internet, utan hello alternativet tooallow du tooinspect eller audit hello-trafik.</span><span class="sxs-lookup"><span data-stu-id="f51f4-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure always traverses from Azure network infrastructure directly out toohello Internet, without hello option tooallow you tooinspect or audit hello traffic.</span></span> <span data-ttu-id="f51f4-107">Obehörig Internetåtkomst kan leda tooinformation avslöjas eller andra typer av säkerhetsintrång.</span><span class="sxs-lookup"><span data-stu-id="f51f4-107">Unauthorized Internet access can potentially lead tooinformation disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

<span data-ttu-id="f51f4-108">Den här artikeln beskriver hur du konfigurerar Tvingad tunneltrafik för virtuella nätverk som skapats med hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="f51f4-108">This article walks you through configuring forced tunneling for virtual networks created using hello Resource Manager deployment model.</span></span> <span data-ttu-id="f51f4-109">Tvingad tunneltrafik kan konfigureras med hjälp av PowerShell inte via hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f51f4-109">Forced tunneling can be configured by using PowerShell, not through hello portal.</span></span> <span data-ttu-id="f51f4-110">Om du vill tooconfigure Tvingad tunneltrafik för hello klassiska distributionsmodellen, Välj klassiska artikel hello följande listrutan:</span><span class="sxs-lookup"><span data-stu-id="f51f4-110">If you want tooconfigure forced tunneling for hello classic deployment model, select classic article from hello following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f51f4-111">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="f51f4-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="f51f4-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f51f4-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a><span data-ttu-id="f51f4-113">Om Tvingad tunneltrafik</span><span class="sxs-lookup"><span data-stu-id="f51f4-113">About forced tunneling</span></span>

<span data-ttu-id="f51f4-114">hello följande diagram illustrerar hur Tvingad tunneltrafik fungerar.</span><span class="sxs-lookup"><span data-stu-id="f51f4-114">hello following diagram illustrates how forced tunneling works.</span></span> 

![Forcerade tunnlar](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

<span data-ttu-id="f51f4-116">Hej Klientdelens undernät inte tvingas tunneldata i hello exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="f51f4-116">In hello example above, hello Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="f51f4-117">hello arbetsbelastningar i hello klientdel undernät kan fortsätta tooaccept och svara toocustomer begäranden från hello Internet direkt.</span><span class="sxs-lookup"><span data-stu-id="f51f4-117">hello workloads in hello Frontend subnet can continue tooaccept and respond toocustomer requests from hello Internet directly.</span></span> <span data-ttu-id="f51f4-118">hello halva nivå och Backend-undernät tvingas tunnel.</span><span class="sxs-lookup"><span data-stu-id="f51f4-118">hello Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="f51f4-119">Alla utgående anslutningar från dessa två undernät toohello Internet blir framtvingad eller omdirigerade tillbaka tooan lokal plats via en hello S2S VPN-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="f51f4-119">Any outbound connections from these two subnets toohello Internet will be forced or redirected back tooan on-premises site via one of hello S2S VPN tunnels.</span></span>

<span data-ttu-id="f51f4-120">Detta ger dig toorestrict och inspektera Internetåtkomst från dina virtuella datorer eller molntjänster i Azure, medan tooenable flernivåtjänst arkitekturen krävs.</span><span class="sxs-lookup"><span data-stu-id="f51f4-120">This allows you toorestrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing tooenable your multi-tier service architecture required.</span></span> <span data-ttu-id="f51f4-121">Om det finns ingen Internet-riktade arbetsbelastningar i ditt virtuella nätverk kan använda du också Tvingad tunneltrafik toohello hela virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f51f4-121">If there are no Internet-facing workloads in your virtual networks, you also can apply forced tunneling toohello entire virtual networks.</span></span>

## <a name="requirements-and-considerations"></a><span data-ttu-id="f51f4-122">Krav och överväganden</span><span class="sxs-lookup"><span data-stu-id="f51f4-122">Requirements and considerations</span></span>

<span data-ttu-id="f51f4-123">Tvingad tunneling i Azure konfigureras via användardefinierade vägar för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f51f4-123">Forced tunneling in Azure is configured via virtual network user-defined routes.</span></span> <span data-ttu-id="f51f4-124">Omdirigera trafik tooan lokal plats uttrycks som en standardväg toohello Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="f51f4-124">Redirecting traffic tooan on-premises site is expressed as a Default Route toohello Azure VPN gateway.</span></span> <span data-ttu-id="f51f4-125">Mer information om användardefinierade Routning och virtuella nätverk finns [användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f51f4-125">For more information about user-defined routing and virtual networks, see [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>

* <span data-ttu-id="f51f4-126">Varje undernät för virtuellt nätverk har en inbyggd, system-routningstabell.</span><span class="sxs-lookup"><span data-stu-id="f51f4-126">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="f51f4-127">routningstabellen för hello system har hello följande tre grupper av vägar:</span><span class="sxs-lookup"><span data-stu-id="f51f4-127">hello system routing table has hello following three groups of routes:</span></span>
  
  * <span data-ttu-id="f51f4-128">**Lokal VNet vägar:** direkt toohello mål virtuella datorer i hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f51f4-128">**Local VNet routes:** Directly toohello destination VMs in hello same virtual network.</span></span>
  * <span data-ttu-id="f51f4-129">**Lokala vägar:** toohello Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="f51f4-129">**On-premises routes:** toohello Azure VPN gateway.</span></span>
  * <span data-ttu-id="f51f4-130">**Standardvägen:** direkt toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="f51f4-130">**Default route:** Directly toohello Internet.</span></span> <span data-ttu-id="f51f4-131">Paket avsedda toohello privata IP-adresser som inte omfattas av hello föregående två vägar tas bort.</span><span class="sxs-lookup"><span data-stu-id="f51f4-131">Packets destined toohello private IP addresses not covered by hello previous two routes are dropped.</span></span>
* <span data-ttu-id="f51f4-132">Den här proceduren använder användardefinierade vägar (UDR) toocreate en routning tabell tooadd en standardväg och sedan associera hello routning tabell tooyour VNet adressundernät tooenable Tvingad tunneltrafik på dessa undernät.</span><span class="sxs-lookup"><span data-stu-id="f51f4-132">This procedure uses user-defined routes (UDR) toocreate a routing table tooadd a default route, and then associate hello routing table tooyour VNet subnet(s) tooenable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="f51f4-133">Tvingad tunneling måste vara kopplad till ett virtuellt nätverk som har en ruttbaserad VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="f51f4-133">Forced tunneling must be associated with a VNet that has a route-based VPN gateway.</span></span> <span data-ttu-id="f51f4-134">Du måste tooset en ”standardwebbplats” bland hello lokala mellan lokala platser anslutna toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="f51f4-134">You need tooset a "default site" among hello cross-premises local sites connected toohello virtual network.</span></span> <span data-ttu-id="f51f4-135">Dessutom hello lokala VPN-enhet måste konfigureras med 0.0.0.0/0 som trafik väljare.</span><span class="sxs-lookup"><span data-stu-id="f51f4-135">Also, hello on-premises VPN device must be configured using 0.0.0.0/0 as traffic selectors.</span></span> 
* <span data-ttu-id="f51f4-136">ExpressRoute Tvingad tunneltrafik konfigureras inte via den här mekanismen men i stället har aktiverats genom att annonsera en standardväg via hello ExpressRoute BGP peeringsessioner.</span><span class="sxs-lookup"><span data-stu-id="f51f4-136">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via hello ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="f51f4-137">Mer information finns i hello [ExpressRoute dokumentation](https://azure.microsoft.com/documentation/services/expressroute/).</span><span class="sxs-lookup"><span data-stu-id="f51f4-137">For more information, see hello [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/).</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="f51f4-138">Konfigurationsöversikt</span><span class="sxs-lookup"><span data-stu-id="f51f4-138">Configuration overview</span></span>

<span data-ttu-id="f51f4-139">hello nedan hjälper dig att skapa en resursgrupp och ett VNet.</span><span class="sxs-lookup"><span data-stu-id="f51f4-139">hello following procedure helps you create a resource group and a VNet.</span></span> <span data-ttu-id="f51f4-140">Sedan skapar en VPN-gateway och konfigurera Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="f51f4-140">You'll then create a VPN gateway and configure forced tunneling.</span></span> <span data-ttu-id="f51f4-141">I den här proceduren hello virtuella nätverket MultiTier-VNet har tre undernät: 'Klientdel', 'Midtier' och 'Backend' med fyra mellan lokala anslutningar: 'DefaultSiteHQ' och tre filialer.</span><span class="sxs-lookup"><span data-stu-id="f51f4-141">In this procedure, hello virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend', with four cross-premises connections: 'DefaultSiteHQ', and three Branches.</span></span>

<span data-ttu-id="f51f4-142">hello inställningssteg ange hello 'DefaultSiteHQ' som hello standard plats-anslutning för Tvingad tunneltrafik och konfigurera hello 'Midtier' och 'Backend-undernät toouse Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="f51f4-142">hello procedure steps set hello 'DefaultSiteHQ' as hello default site connection for forced tunneling, and configure hello 'Midtier' and 'Backend' subnets toouse forced tunneling.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f51f4-143">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="f51f4-143">Before you begin</span></span>

<span data-ttu-id="f51f4-144">Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="f51f4-144">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="f51f4-145">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="f51f4-145">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

### <a name="toolog-in"></a><span data-ttu-id="f51f4-146">toolog i</span><span class="sxs-lookup"><span data-stu-id="f51f4-146">toolog in</span></span>

[!INCLUDE [toolog in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a><span data-ttu-id="f51f4-147">Konfigurera tvingad tunneltrafik</span><span class="sxs-lookup"><span data-stu-id="f51f4-147">Configure forced tunneling</span></span>

1. <span data-ttu-id="f51f4-148">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f51f4-148">Create a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. <span data-ttu-id="f51f4-149">Skapa ett virtuellt nätverk och ange undernät.</span><span class="sxs-lookup"><span data-stu-id="f51f4-149">Create a virtual network and specify subnets.</span></span>

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. <span data-ttu-id="f51f4-150">Skapa hello lokala nätverksgatewayer.</span><span class="sxs-lookup"><span data-stu-id="f51f4-150">Create hello local network gateways.</span></span>

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. <span data-ttu-id="f51f4-151">Skapa hello flödet tabell och väg regel.</span><span class="sxs-lookup"><span data-stu-id="f51f4-151">Create hello route table and route rule.</span></span>

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. <span data-ttu-id="f51f4-152">Associera hello flödet tabell toohello Midtier och Backend-undernät.</span><span class="sxs-lookup"><span data-stu-id="f51f4-152">Associate hello route table toohello Midtier and Backend subnets.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. <span data-ttu-id="f51f4-153">Skapa hello Gateway med en standardwebbplats.</span><span class="sxs-lookup"><span data-stu-id="f51f4-153">Create hello Gateway with a default site.</span></span> <span data-ttu-id="f51f4-154">Det här steget tar några tid toocomplete, ibland 45 minuter eller mer, eftersom du skapar och konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="f51f4-154">This step takes some time toocomplete, sometimes 45 minutes or more, because you are creating and configuring hello gateway.</span></span><br> <span data-ttu-id="f51f4-155">Hej **- GatewayDefaultSite** är Hej cmdlet-parameter som tillåter hello tvingas routning configuration toowork, så ta försiktighet tooconfigure inställningen korrekt.</span><span class="sxs-lookup"><span data-stu-id="f51f4-155">hello **-GatewayDefaultSite** is hello cmdlet parameter that allows hello forced routing configuration toowork, so take care tooconfigure this setting properly.</span></span> <span data-ttu-id="f51f4-156">Den här parametern är tillgängliga i PowerShell 1.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f51f4-156">This parameter is available in PowerShell 1.0 or later.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. <span data-ttu-id="f51f4-157">Upprätta hello plats-till-plats VPN-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="f51f4-157">Establish hello Site-to-Site VPN connections.</span></span>

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
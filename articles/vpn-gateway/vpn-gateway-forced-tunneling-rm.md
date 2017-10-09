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
# <a name="configure-forced-tunneling-using-hello-azure-resource-manager-deployment-model"></a>Konfigurera Tvingad tunneling med hello Azure Resource Manager-distributionsmodellen

Tvingad tunneltrafik kan du omdirigera eller ”force” alla Internet-bunden trafik tillbaka tooyour lokal plats via en plats-till-plats VPN-tunnel för kontroll och granskning. Detta är en kritisk säkerhetskraven för de flesta företags IT principer. Utan Tvingad tunneling passerar Internet-bunden trafik från din virtuella datorer i Azure alltid från Azure nätverksinfrastruktur direkt ut toohello Internet, utan hello alternativet tooallow du tooinspect eller audit hello-trafik. Obehörig Internetåtkomst kan leda tooinformation avslöjas eller andra typer av säkerhetsintrång.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Den här artikeln beskriver hur du konfigurerar Tvingad tunneltrafik för virtuella nätverk som skapats med hello Resource Manager-modellen. Tvingad tunneltrafik kan konfigureras med hjälp av PowerShell inte via hello-portalen. Om du vill tooconfigure Tvingad tunneltrafik för hello klassiska distributionsmodellen, Välj klassiska artikel hello följande listrutan:

> [!div class="op_single_selector"]
> * [PowerShell – Klassisk](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell – Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a>Om Tvingad tunneltrafik

hello följande diagram illustrerar hur Tvingad tunneltrafik fungerar. 

![Forcerade tunnlar](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Hej Klientdelens undernät inte tvingas tunneldata i hello exemplet ovan. hello arbetsbelastningar i hello klientdel undernät kan fortsätta tooaccept och svara toocustomer begäranden från hello Internet direkt. hello halva nivå och Backend-undernät tvingas tunnel. Alla utgående anslutningar från dessa två undernät toohello Internet blir framtvingad eller omdirigerade tillbaka tooan lokal plats via en hello S2S VPN-tunnlar.

Detta ger dig toorestrict och inspektera Internetåtkomst från dina virtuella datorer eller molntjänster i Azure, medan tooenable flernivåtjänst arkitekturen krävs. Om det finns ingen Internet-riktade arbetsbelastningar i ditt virtuella nätverk kan använda du också Tvingad tunneltrafik toohello hela virtuella nätverk.

## <a name="requirements-and-considerations"></a>Krav och överväganden

Tvingad tunneling i Azure konfigureras via användardefinierade vägar för virtuellt nätverk. Omdirigera trafik tooan lokal plats uttrycks som en standardväg toohello Azure VPN-gateway. Mer information om användardefinierade Routning och virtuella nätverk finns [användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md).

* Varje undernät för virtuellt nätverk har en inbyggd, system-routningstabell. routningstabellen för hello system har hello följande tre grupper av vägar:
  
  * **Lokal VNet vägar:** direkt toohello mål virtuella datorer i hello samma virtuella nätverk.
  * **Lokala vägar:** toohello Azure VPN-gateway.
  * **Standardvägen:** direkt toohello Internet. Paket avsedda toohello privata IP-adresser som inte omfattas av hello föregående två vägar tas bort.
* Den här proceduren använder användardefinierade vägar (UDR) toocreate en routning tabell tooadd en standardväg och sedan associera hello routning tabell tooyour VNet adressundernät tooenable Tvingad tunneltrafik på dessa undernät.
* Tvingad tunneling måste vara kopplad till ett virtuellt nätverk som har en ruttbaserad VPN-gateway. Du måste tooset en ”standardwebbplats” bland hello lokala mellan lokala platser anslutna toohello virtuellt nätverk. Dessutom hello lokala VPN-enhet måste konfigureras med 0.0.0.0/0 som trafik väljare. 
* ExpressRoute Tvingad tunneltrafik konfigureras inte via den här mekanismen men i stället har aktiverats genom att annonsera en standardväg via hello ExpressRoute BGP peeringsessioner. Mer information finns i hello [ExpressRoute dokumentation](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="configuration-overview"></a>Konfigurationsöversikt

hello nedan hjälper dig att skapa en resursgrupp och ett VNet. Sedan skapar en VPN-gateway och konfigurera Tvingad tunneltrafik. I den här proceduren hello virtuella nätverket MultiTier-VNet har tre undernät: 'Klientdel', 'Midtier' och 'Backend' med fyra mellan lokala anslutningar: 'DefaultSiteHQ' och tre filialer.

hello inställningssteg ange hello 'DefaultSiteHQ' som hello standard plats-anslutning för Tvingad tunneltrafik och konfigurera hello 'Midtier' och 'Backend-undernät toouse Tvingad tunneltrafik.

## <a name="before-you-begin"></a>Innan du börjar

Installera hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.

### <a name="toolog-in"></a>toolog i

[!INCLUDE [toolog in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a>Konfigurera tvingad tunneltrafik

1. Skapa en resursgrupp.

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. Skapa ett virtuellt nätverk och ange undernät.

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. Skapa hello lokala nätverksgatewayer.

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. Skapa hello flödet tabell och väg regel.

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. Associera hello flödet tabell toohello Midtier och Backend-undernät.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. Skapa hello Gateway med en standardwebbplats. Det här steget tar några tid toocomplete, ibland 45 minuter eller mer, eftersom du skapar och konfigurerar hello gateway.<br> Hej **- GatewayDefaultSite** är Hej cmdlet-parameter som tillåter hello tvingas routning configuration toowork, så ta försiktighet tooconfigure inställningen korrekt. Den här parametern är tillgängliga i PowerShell 1.0 eller senare.

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. Upprätta hello plats-till-plats VPN-anslutningar.

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
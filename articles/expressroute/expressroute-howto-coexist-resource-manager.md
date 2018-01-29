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
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/18/2017
---
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a>Konfigurera ExpressRoute-anslutningar och anslutningar för plats-till-plats som kan samexistera
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell – Klassisk](expressroute-howto-coexist-classic.md)
> 
> 

Att konfigurera VPN för plats till plats och samexisterande ExpressRoute-anslutningar har flera fördelar. Du kan konfigurera en VPN för plats till plats som en säker redundanssökväg för ExpressRoute, eller använda plats-till-plats-VPN för att ansluta till platser som inte är anslutna via ExpressRoute. Vi beskriver stegen för att konfigurera båda scenarierna i den här artikeln. Den här artikeln gäller distributionsmodellen i Resource Manager, och PowerShell används. Den här konfigurationen är inte tillgänglig i Azure-portalen.

> [!IMPORTANT]
> ExpressRoute-kretsarna måste förkonfigureras innan du följer anvisningarna nedan. Kontrollera att du har följt riktlinjerna för att [skapa en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och [konfigurera routning](expressroute-howto-routing-arm.md) innan du fortsätter.
> 
> 

## <a name="limits-and-limitations"></a>Gränser och begränsningar
* **Transit-routing stöds inte.** Du kan inte routa (via Azure) mellan ditt lokala nätverk som ansluter via plats-till-plats-VPN och ditt lokala nätverk som ansluter via ExpressRoute.
* **Basic-SKU-gatewayen stöds inte.** Du måste använda en icke-Basic SKU-gateway för både [ExpressRoute-gatewayen](expressroute-about-virtual-network-gateways.md) och [VPN-gatewayen](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Enbart routebaserad VPN-gateway stöds.** Du måste använda en routebaserad [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Statiska vägar ska konfigureras för din VPN-gateway.** Om ditt lokala nätverk är anslutet både till ExpressRoute och en plats-till-plats-VPN så måste du ha konfigurerat en statisk väg i ditt lokala nätverk för att routa plats-till-plats-VPN-anslutningen till det offentliga Internet.
* **ExpressRoute-gatewayen måste konfigureras först och länkas till en krets.** Du måste skapa ExpressRoute-gatewayen först och länka den till en krets innan du lägger till en plats-till-plats-VPN-gateway för plats till plats.

## <a name="configuration-designs"></a>Konfigurationsdesign
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Konfigurera en VPN för plats till plats som en redundanssökväg för ExpressRoute
Du kan konfigurera en VPN-anslutning för plats till plats som en säkerhetskopia av ExpressRoute. Detta gäller endast virtuella nätverk som är länkade till Azures privata peering-sökväg. Det finns ingen VPN-baserad redundanslösning för tjänster som är tillgängliga via Azures offentliga och Microsofts peerings. ExpressRoute-kretsen är alltid den primära länken. Data flödar endast via plats-till-plats-VPN-sökvägen om ExpressRoute-kretsen misslyckas.

> [!NOTE]
> Medan ExpressRoute-kretsen är prioriterad över plats-till-plats-VPN när bägge vägarna är samma, kommer Azure att använda den längsta prefixmatchning för att välja väg till paketets mål.
> 
> 

![Samexistera](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-to-connect-to-sites-not-connected-through-expressroute"></a>Konfigurera en VPN för plats till plats för att ansluta till platser som inte är anslutna via ExpressRoute
Du kan konfigurera nätverket där vissa platser ansluter direkt till Azure via VPN för plats till plats och vissa platser ansluter via ExpressRoute. 

![Samexistera](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> Det går inte att konfigurera ett virtuellt nätverk som en överföringsrouter.
> 
> 

## <a name="selecting-the-steps-to-use"></a>Välj vilka steg du ska använda
Det finns två uppsättningar procedurer för att välja bland. Vilken konfigurationsprocedur du väljer beror på om du har ett befintligt virtuellt nätverk som du vill ansluta till, eller om du vill skapa ett nytt virtuellt nätverk.

* Jag har inte något VNet och behöver skapa ett.
  
    Om du inte redan har ett virtuellt nätverk kan den här proceduren hjälpa dig att skapa ett nytt virtuellt nätverk med hjälp av distributionsmodellen i Resource Manager samt skapa nya ExpressRoute- och plats-till-plats-VPN-anslutningar. Om du vill konfigurera ett virtuellt nätverk följer du stegen i [Så här skapar du ett nytt virtuellt nätverk och samexisterande anslutningar](#new).
* Jag har redan en distributionsmodell för Resource Manager i VNet.
  
    Du kanske redan har ett virtuellt nätverk på plats med en befintlig VPN-anslutning för plats till plats eller en ExpressRoute-anslutning. Avsnittet [Så här konfigurerar du samexisterande anslutningar för ett befintligt VNet](#add) visar hur du tar bort gatewayen och sedan skapar nya ExpressRoute- och plats-till-plats-VPN-anslutningar. När du skapar nya anslutningar måste stegen utföras i en specifik ordning. Följ inte anvisningarna i andra artiklar när du skapar dina gateways och anslutningar.
  
    I den här proceduren skapar du anslutningar som kan samexistera, vilket kräver att du tar bort din gateway och sedan konfigurerar nya gateways. Du kommer att ha stilleståndstid för anslutningar på flera platser när du tar bort och återskapar din gateway och dina anslutningar, men du behöver inte migrera några virtuella datorer eller tjänster till ett nytt virtuellt nätverk. Dina virtuella datorer och tjänster kommer fortfarande att kunna kommunicera ut via belastningsutjämnaren när du konfigurerar din gateway, om de är konfigurerade för att göra det.

## <a name="new"></a>Så här skapar du ett nytt virtuellt nätverk och samexisterande anslutningar
Den här proceduren vägleder dig genom att skapa ett VNet samt plats-till-plats- och ExpressRoute-anslutningar som ska finnas samtidigt.

1. Installera den senaste versionen av Azure PowerShell-cmdletarna. Mer information om hur man installerar cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview). De cmdletar som du använder för den här konfigurationen kan se annorlunda ut än de du tidigare använt. Var noga med att använda de cmdletar som anges i instruktionerna.
2. Logga in på ditt konto och konfigurera miljön.

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. Skapa ett virtuellt nätverk, inklusive gateway-undernätet. Mer information om den virtuella nätverkskonfigurationen finns i [Konfiguration av Azure Virtual Network](../virtual-network/virtual-networks-create-vnet-arm-ps.md).
   
   > [!IMPORTANT]
   > Gateway-undernätet måste vara /27 eller ett kortare prefix (till exempel /26 eller /25).
   > 
   > 
   
    Skapa ett nytt VNet.

  ```powershell
  $vnet = New-AzureRmVirtualNetwork -Name "CoexVnet" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AddressPrefix "10.200.0.0/16"
  ```
   
    Lägg till undernät.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name "App" -VirtualNetwork $vnet -AddressPrefix "10.200.1.0/24"
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Spara VNet-konfigurationen.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <a name="gw"></a>Skapa en ExpressRoute-gateway. Mer information om konfiguration av ExpressRoute-gateways finns i [Konfiguration av ExpressRoute-gateway](expressroute-howto-add-gateway-resource-manager.md). GatewaySKU:n måste vara *Standard*, *HighPerformance*, eller *UltraPerformance*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. Länka ExpressRoute-gatewayen till ExpressRoute-kretsen. När det här steget har slutförts har anslutningen mellan ditt lokala nätverk och Azure, via ExpressRoute, upprättats. Mer information om länken finns i [Länka VNets till ExpressRoute](expressroute-howto-linkvnet-arm.md).

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <a name="vpngw"></a>Därefter skapar du din plats-till-plats-VPN-gateway. Mer information om konfigurationen av VPN-gatewayen finns i [Konfigurera ett VNet med en plats-till-plats-anslutning](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md). GatewaySKU:n måste vara *Standard*, *HighPerformance*, eller *UltraPerformance*. VpnType måste vara *RouteBased*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    Azure VPN-gateway har stöd för BGP-routningsprotokoll. Du kan ange ASN (AS Number) för det virtuella nätverket genom att lägga till växeln - Asn i följande kommando. Om du inte anger parametern blir standardvärdet 65515.

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    Du hittar BGP peering-IP och AS-numret som Azure använder för VPN-gatewayen i $azureVpn.BgpSettings.BgpPeeringAddress och $azureVpn.BgpSettings.Asn. Mer information finns i [Konfigurera BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) för Azure VPN-gateway.
7. Skapa en lokal plats för VPN-gatewayen. Det här kommandot konfigurerar inte din lokala VPN-gateway. I stället kan du ange lokala gateway-inställningar, som t.ex. offentlig IP-adress och lokalt adressutrymme, så att Azure VPN-gatewayen kan ansluta till den.
   
    Om din lokala VPN-enhet bara stöder statisk routning, kan du konfigurera de statiska vägarna på följande sätt:

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    Om din lokala VPN-enhet stöder BGP och du vill aktivera dynamisk routning, måste du veta BGP peering IP och AS-numret som din lokala VPN-enhet använder.

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for the BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. Konfigurera din lokala VPN-enhet till att ansluta till en ny Azure VPN-gateway. Mer information om VPN-enhetskonfiguration finns i [VPN-enhetskonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).
9. Länka VPN-gatewayen för plats till plats på Azure till den lokala gatewayen.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <a name="add"></a>Så här konfigurerar du samexisterande anslutningar för ett befintligt VNet
Om du har ett befintligt virtuellt nätverk, kontrollerar du storleken på gateway-undernätet. Om gateway-undernätet är /28 eller /29, måste du först ta bort den virtuella nätverksgatewayen och öka storleken för gateway-undernätet. Stegen i det här avsnittet visar hur du gör.

Om gateway-undernätet är /27 eller större och det virtuella nätverket är anslutet via ExpressRoute, kan du hoppa över stegen nedan och gå vidare till [”Steg 6 – Skapa en VPN-gateway för plats till plats”](#vpngw) i föregående avsnitt.  

> [!NOTE]
> När du tar bort den befintliga gatewayen förlorar dina lokala platser anslutningen till ditt virtuella nätverk medan du arbetar med konfigurationen. 
> 
> 

1. Du måste installera den senaste versionen av Azure PowerShell-cmdletarna. Mer information om hur du installerar cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview). De cmdletar som du använder för den här konfigurationen kan se annorlunda ut än de du tidigare använt. Var noga med att använda de cmdletar som anges i instruktionerna. 
2. Ta bort den befintliga ExpressRoute- eller VPN-gatewayen för plats till plats.

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. Ta bort gateway-undernätet. 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. Lägg till ett Gateway-undernät som är /27 eller större.
   
   > [!NOTE]
   > Om du inte har tillräckligt med IP-adresser kvar i det virtuella nätverket för att kunna öka storleken på gateway-undernätet, måste du lägga till mer IP-adressutrymme.
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Spara VNet-konfigurationen.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. Just nu har du ett VNet utan gateways. Om du vill slutföra dina anslutningar och skapa nya gateways kan du fortsätta med [Steg 4 – Skapa en ExpressRoute-gateway](#gw), som finns i den föregående uppsättningen med steg.

## <a name="to-add-point-to-site-configuration-to-the-vpn-gateway"></a>Så här lägger du till punkt-till-plats-konfiguration till VPN-gateway
Du kan följa stegen nedan om du vill lägga till punkt-till-plats-konfiguration för VPN-gateway i en samexisterande inställning.

1. Lägga till VPN-klientadresspoolen.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. Överför VPN-rotcertifikatet till Azure för din VPN-gateway. I det här exemplet förutsätts att rotcertifikatet lagras i den lokala datorn där följande PowerShell-cmdletar körs.

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

Mer information om punkt-till-plats-VPN finns i [Konfigurera en punkt-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## <a name="next-steps"></a>Nästa steg
Mer information om ExpressRoute finns i [Vanliga frågor och svar om ExpressRoute](expressroute-faqs.md).

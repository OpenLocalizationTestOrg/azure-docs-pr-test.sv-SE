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
# <a name="configure-expressroute-and-site-to-site-coexisting-connections"></a>Konfigurera ExpressRoute-anslutningar och anslutningar för plats-till-plats som kan samexistera
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-howto-coexist-resource-manager.md)
> * [PowerShell – Klassisk](expressroute-howto-coexist-classic.md)
> 
> 

Att konfigurera VPN för plats till plats och samexisterande ExpressRoute-anslutningar har flera fördelar. Du kan konfigurera en plats-till-plats-VPN som en säker redundans sökväg för ExressRoute eller använda plats-till-plats VPN tooconnect toosites som inte är anslutna via ExpressRoute. Vi upp hello steg tooconfigure båda scenarierna i den här artikeln. Den här artikeln gäller toohello Resource Manager-modellen och använder PowerShell. Den här konfigurationen är inte tillgänglig i hello Azure-portalen.

> [!IMPORTANT]
> ExpressRoute-kretsar måste konfigureras före innan du följer hello anvisningarna nedan. Kontrollera att du har följt hello guider för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och [konfigurera routning](expressroute-howto-routing-arm.md) innan du fortsätter.
> 
> 

## <a name="limits-and-limitations"></a>Gränser och begränsningar
* **Transit-routing stöds inte.** Du kan inte routa (via Azure) mellan ditt lokala nätverk som ansluter via plats-till-plats-VPN och ditt lokala nätverk som ansluter via ExpressRoute.
* **Basic-SKU-gatewayen stöds inte.** Du måste använda en icke - grundläggande SKU-gateway för båda hello [ExpressRoute-gateway](expressroute-about-virtual-network-gateways.md) och hello [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Enbart routebaserad VPN-gateway stöds.** Du måste använda en routebaserad [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).
* **Statiska vägar ska konfigureras för din VPN-gateway.** Om nätverket är anslutet tooboth ExpressRoute och en plats-till-plats VPN, måste du ha en statisk väg som konfigurerats i ditt lokala nätverk tooroute hello plats-till-plats VPN-anslutningen toohello offentliga Internet.
* **ExpressRoute-gateway måste konfigureras först och länka tooa krets.** Du måste först skapa hello ExpressRoute-gateway och länka det tooa krets innan du lägger till hello plats-till-plats VPN-gateway.

## <a name="configuration-designs"></a>Konfigurationsdesign
### <a name="configure-a-site-to-site-vpn-as-a-failover-path-for-expressroute"></a>Konfigurera en VPN för plats till plats som en redundanssökväg för ExpressRoute
Du kan konfigurera en VPN-anslutning för plats till plats som en säkerhetskopia av ExpressRoute. Detta gäller endast toovirtual nätverk länkade toohello Azure privat peering sökväg. Det finns ingen VPN-baserad redundanslösning för tjänster som är tillgängliga via Azures offentliga och Microsofts peerings. Hej ExpressRoute-kretsen är alltid hello primära länken. Data flödar genom hello plats-till-plats VPN-väg endast om hello ExpressRoute-krets misslyckades.

> [!NOTE]
> ExpressRoute-kretsen är prioriterade via plats-till-plats VPN när båda vägar är hello samma, använder Azure hello längsta prefix matchar toochoose hello vägen mot hello paketets mål.
> 
> 

![Samexistera](media/expressroute-howto-coexist-resource-manager/scenario1.jpg)

### <a name="configure-a-site-to-site-vpn-tooconnect-toosites-not-connected-through-expressroute"></a>Konfigurera en plats-till-plats VPN-tooconnect toosites som inte är anslutna via ExpressRoute
Du kan konfigurera vissa platser ansluter direkt tooAzure via plats-till-plats VPN och vissa platser ansluter via ExpressRoute. 

![Samexistera](media/expressroute-howto-coexist-resource-manager/scenario2.jpg)

> [!NOTE]
> Det går inte att konfigurera ett virtuellt nätverk som en överföringsrouter.
> 
> 

## <a name="selecting-hello-steps-toouse"></a>Att välja hello steg toouse
Det finns två olika uppsättningar av procedurer toochoose från. hello configuration procedur som du väljer beror på om du har ett befintligt virtuellt nätverk som du vill tooconnect till, eller önskade toocreate ett nytt virtuellt nätverk.

* Jag inte har ett VNet och behöver toocreate en.
  
    Om du inte redan har ett virtuellt nätverk kan den här proceduren hjälpa dig att skapa ett nytt virtuellt nätverk med hjälp av distributionsmodellen i Resource Manager samt skapa nya ExpressRoute- och plats-till-plats-VPN-anslutningar. tooconfigure ett virtuellt nätverk gör hello i [toocreate ett nytt virtuellt nätverk och samtidiga anslutningar](#new).
* Jag har redan en distributionsmodell för Resource Manager i VNet.
  
    Du kanske redan har ett virtuellt nätverk på plats med en befintlig VPN-anslutning för plats till plats eller en ExpressRoute-anslutning. Hej [tooconfigure samtidiga anslutningar för ett redan existerande VNet](#add) avsnitt vägleder dig genom att ta bort hello gateway och sedan skapa nya ExpressRoute- och plats-till-plats VPN-anslutningar. När du skapar hello nya anslutningar, måste hello steg slutföras i en viss ordning. Använd inte hello instruktionerna i andra artiklar toocreate din gateway och anslutningar.
  
    I den här proceduren skapar anslutningar som kan finnas kräver du toodelete din gateway och konfigurera nya gatewayer. Du måste driftstopp för anslutningar mellan platser när du tar bort och återskapa din gateway och anslutningar, men du behöver inte toomigrate någon av dina virtuella datorer eller tjänster tooa nytt virtuellt nätverk. Dina virtuella datorer och tjänster kommer fortfarande att kunna toocommunicate ut via hello belastningsutjämnare när du konfigurerar din gateway om de är konfigurerade toodo så.

## <a name="new"></a>toocreate ett nytt virtuellt nätverk och samtidiga anslutningar
Den här proceduren vägleder dig genom att skapa ett VNet samt plats-till-plats- och ExpressRoute-anslutningar som ska finnas samtidigt.

1. Installera hello senaste versionen av hello Azure PowerShell-cmdlets. Information om hur du installerar hello cmdlets finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). hello-cmdletar som du använder för den här konfigurationen kan vara något annorlunda än vad du kan känna till. Anges till toouse hello-cmdlets i dessa instruktioner.
2. Logga in tooyour konto och konfigurera hello-miljö.

  ```powershell
  login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName 'yoursubscription'
  $location = "Central US"
  $resgrp = New-AzureRmResourceGroup -Name "ErVpnCoex" -Location $location
  $VNetASN = 65010
  ```
3. Skapa ett virtuellt nätverk, inklusive gateway-undernätet. Mer information om konfiguration av virtuellt nätverk hello finns [Azure Virtual Network configuration](../virtual-network/virtual-networks-create-vnet-arm-ps.md).
   
   > [!IMPORTANT]
   > hello Gateway-undernätet måste vara minst/27 eller ett kortare prefix (till exempel /26 eller /25).
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
   
    Spara konfigurationen för hello virtuellt nätverk.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
4. <a name="gw"></a>Skapa en ExpressRoute-gateway. Mer information om gateway-konfiguration för hello ExpressRoute finns [ExpressRoute gatewaykonfigurationen](expressroute-howto-add-gateway-resource-manager.md). Hej GatewaySKU måste vara *Standard*, *HighPerformance*, eller *UltraPerformance*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "ERGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "ERGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  $gw = New-AzureRmVirtualNetworkGateway -Name "ERGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "ExpressRoute" -GatewaySku Standard
  ```
5. Länka hello ExpressRoute-gateway toohello ExpressRoute-kretsen. Det här steget har slutförts, upprättas hello anslutning mellan ditt lokala nätverk och Azure via ExpressRoute. Läs mer om hello länkåtgärder [länk Vnet tooExpressRoute](expressroute-howto-linkvnet-arm.md).

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "YourCircuit" -ResourceGroupName "YourCircuitResourceGroup"
  New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $gw -PeerId $ckt.Id -ConnectionType ExpressRoute
  ```
6. <a name="vpngw"></a>Därefter skapar du din plats-till-plats-VPN-gateway. Mer information om VPN-gateway-konfiguration för hello finns [konfigurera ett virtuellt nätverk med en plats-till-plats-anslutning](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md). Hej GatewaySKU måste vara *Standard*, *HighPerformance*, eller *UltraPerformance*. Hej VpnType måste *RouteBased*.

  ```powershell
  $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $gwIP = New-AzureRmPublicIpAddress -Name "VPNGatewayIP" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -AllocationMethod Dynamic
  $gwConfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "VPNGatewayIpConfig" -SubnetId $gwSubnet.Id -PublicIpAddressId $gwIP.Id
  New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard"
  ```
   
    Azure VPN-gateway har stöd för BGP-routningsprotokoll. Du kan ange ASN (AS Number) för det virtuella nätverket genom att lägga till hello Asn - växel i hello följande kommando. Ange inte parametern kommer standard tooAS number 65515.

  ```powershell
  $azureVpn = New-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -IpConfigurations $gwConfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "Standard" -Asn $VNetASN
  ```
   
    Du kan hitta hello BGP-peering IP och hello som nummer som Azure använder för hello VPN-gateway i $azureVpn.BgpSettings.BgpPeeringAddress och $azureVpn.BgpSettings.Asn. Mer information finns i [Konfigurera BGP](../vpn-gateway/vpn-gateway-bgp-resource-manager-ps.md) för Azure VPN-gateway.
7. Skapa en lokal plats för VPN-gatewayen. Det här kommandot konfigurerar inte din lokala VPN-gateway. I stället kan du tooprovide hello lokala gateway-inställningar, till exempel hello offentlig IP-adress och hello lokala adressutrymmet, så att hello Azure VPN-gateway kan ansluta tooit.
   
    Om din lokala VPN-enhet stöder endast statisk routning, kan du konfigurera hello statiska vägar i hello följande sätt:

  ```powershell
  $MyLocalNetworkAddress = @("10.100.0.0/16","10.101.0.0/16","10.102.0.0/16")
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress *<Public IP>* -AddressPrefix $MyLocalNetworkAddress
  ```
   
    Om din lokala VPN-enhet stöder hello BGP och du vill tooenable dynamisk routning, måste tooknow hello BGP peering IP-adress och hello som number som din lokala VPN-enhet använder.

  ```powershell
  $localVPNPublicIP = "<Public IP>"
  $localBGPPeeringIP = "<Private IP for hello BGP session>"
  $localBGPASN = "<ASN>"
  $localAddressPrefix = $localBGPPeeringIP + "/32"
  $localVpn = New-AzureRmLocalNetworkGateway -Name "LocalVPNGateway" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -GatewayIpAddress $localVPNPublicIP -AddressPrefix $localAddressPrefix -BgpPeeringAddress $localBGPPeeringIP -Asn $localBGPASN
  ```
8. Konfigurera din lokala VPN-enhet tooconnect toohello nya Azure VPN-gateway. Mer information om VPN-enhetskonfiguration finns i [VPN-enhetskonfiguration](../vpn-gateway/vpn-gateway-about-vpn-devices.md).
9. Länken hello plats-till-plats VPN-gateway på Azure toohello lokala gateway.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  New-AzureRmVirtualNetworkGatewayConnection -Name "VPNConnection" -ResourceGroupName $resgrp.ResourceGroupName -Location $location -VirtualNetworkGateway1 $azureVpn -LocalNetworkGateway2 $localVpn -ConnectionType IPsec -SharedKey <yourkey>
  ```

## <a name="add"></a>tooconfigure samtidiga anslutningar för ett befintligt virtuellt nätverk
Om du har ett befintligt virtuellt nätverk kontrollerar du hello gateway undernätets storlek. Om hello gateway-undernät är /28 eller /29, måste du först ta bort hello virtuell nätverksgateway och öka storleken på hello gateway-undernät. hello steg i det här avsnittet visar hur toodo som.

Om hello gateway-undernät är minst/27 eller större och hello virtuellt nätverk är anslutna via ExpressRoute kan du hoppa över hello stegen nedan och fortsätta för[”steg 6 – skapa en plats-till-plats VPN-gateway”](#vpngw) hello föregående avsnitt. 

> [!NOTE]
> När du tar bort hello befintlig gateway förlorar din lokala lokala hello anslutning tooyour virtuellt nätverk när du arbetar med den här konfigurationen. 
> 
> 

1. Du behöver tooinstall hello senaste versionen av hello Azure PowerShell-cmdlets. Mer information om hur du installerar cmdlets finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). hello-cmdletar som du använder för den här konfigurationen kan vara något annorlunda än vad du kan känna till. Anges till toouse hello-cmdlets i dessa instruktioner. 
2. Ta bort hello befintlig ExpressRoute eller plats-till-plats VPN-gateway.

  ```powershell 
  Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
  ```
3. Ta bort gateway-undernätet. 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup> Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
  ```
4. Lägg till ett Gateway-undernät som är /27 eller större.
   
   > [!NOTE]
   > Om du inte har tillräckligt med IP-adresser som är kvar i ditt virtuella nätverk tooincrease hello gateway undernätets storlek, måste tooadd flera IP-adressutrymme.
   > 
   > 

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
  Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.0/24"
  ```
   
    Spara konfigurationen för hello virtuellt nätverk.

  ```powershell
  $vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
5. Just nu har du ett VNet utan gateways. toocreate nya gatewayer och slutföra dina anslutningar, du kan fortsätta med [steg 4: skapa en ExpressRoute-gateway](#gw)hittades i hello föregående steg.

## <a name="tooadd-point-to-site-configuration-toohello-vpn-gateway"></a>tooadd punkt-till-plats configuration toohello VPN-gateway
Du kan följa hello stegen nedan tooadd punkt-till-plats configuration tooyour VPN-gateway i en inställning för existerar.

1. Lägga till VPN-klientadresspoolen.

  ```powershell
  $azureVpn = Get-AzureRmVirtualNetworkGateway -Name "VPNGateway" -ResourceGroupName $resgrp.ResourceGroupName
  Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $azureVpn -VpnClientAddressPool "10.251.251.0/24"
  ```
2. Överför hello VPN root certificate tooAzure för VPN-gateway. I det här exemplet förutsätts att hello rotcertifikat lagras i hello lokala dator där hello följande PowerShell-cmdlets körs.

  ```powershell
  $p2sCertFullName = "RootErVpnCoexP2S.cer" 
  $p2sCertMatchName = "RootErVpnCoexP2S" 
  $p2sCertToUpload=get-childitem Cert:\CurrentUser\My | Where-Object {$_.Subject -match $p2sCertMatchName} 
  if ($p2sCertToUpload.count -eq 1){write-host "cert found"} else {write-host "cert not found" exit} 
  $p2sCertData = [System.Convert]::ToBase64String($p2sCertToUpload.RawData) Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $p2sCertFullName -VirtualNetworkGatewayname $azureVpn.Name -ResourceGroupName $resgrp.ResourceGroupName -PublicCertData $p2sCertData
  ```

Mer information om punkt-till-plats-VPN finns i [Konfigurera en punkt-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md).

## <a name="next-steps"></a>Nästa steg
Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).

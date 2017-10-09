---
title: "Konfigurera aktiv-aktiv S2S VPN-anslutningar för VPN-gatewayer: Azure Resource Manager: PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du konfigurerar aktiv-aktiv anslutningar med Azure VPN-gatewayer med Azure Resource Manager och PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2017
ms.author: yushwang
ms.openlocfilehash: 964eedc7698e42bf0e082f0105845f2a339daf57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways"></a>Konfigurera aktiv-aktiv S2S VPN-anslutningar med Azure VPN-gatewayer

Den här artikeln vägleder dig genom hello steg toocreate aktiv-aktiv mellan platser och VNet-till-VNet-anslutningar med hello Resource Manager-distributionsmodellen och PowerShell.

## <a name="about-highly-available-cross-premises-connections"></a>Om hög tillgänglighet anslutningar mellan platser
tooachieve hög tillgänglighet för anslutningar mellan platser och VNet-till-VNet-anslutningen bör du distribuera flera VPN-gatewayer och upprätta flera parallella anslutningar mellan ditt nätverk och Azure. Se [hög tillgänglighet mellan platser och VNet-till-VNet-anslutningar](vpn-gateway-highlyavailable.md) en översikt över alternativ för nätverksanslutning och topologi.

Den här artikeln innehåller instruktioner för hello tooset upp en aktiv-aktiv mellan lokala VPN-anslutning och aktiv-aktiv anslutning mellan två virtuella nätverk:

* [Del 1 - Skapa och konfigurera Azure VPN-gateway i aktivt-aktivt läge](#aagateway)
* [Del 2 – upprätta aktiv-aktiv anslutningar mellan platser](#aacrossprem)
* [Del 3 – skapa aktiv-aktiv VNet-till-VNet-anslutningar](#aav2v)
* [En del 4 – uppdatera en befintlig gateway mellan aktiv-aktiv och aktivt vänteläge](#aaupdate)

Du kan kombinera dessa tillsammans toobuild en mer komplexa, hög tillgänglighet nätverkstopologi som uppfyller dina behov.

> [!IMPORTANT]
> Observera att hello aktiv-aktiv läge använder endast hello följande SKU: er: 
  * VpnGw1 VpnGw2, VpnGw3
  * HighPerformance (för gamla äldre SKU: er)
> 
> 

## <a name ="aagateway"></a>Del 1 - Skapa och konfigurera VPN-gatewayer för aktiv-aktiv
hello följande konfigurerar Azure VPN-gateway i lägena för aktiv-aktiv. hello viktiga skillnader mellan hello aktiv-aktiv och aktivt vänteläge gateway:

* Du behöver toocreate två Gateway IP-konfigurationer med två offentliga IP-adresser
* Hej EnableActiveActiveFeature flagga måste anges
* hello gateway SKU: N måste vara VpnGw1 VpnGw2, VpnGw3 eller HighPerformance (äldre SKU).

hello är andra egenskaper hello samma som hello aktiva aktiva gateways. 

### <a name="before-you-begin"></a>Innan du börjar
* Kontrollera att du har en Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).
* Du behöver tooinstall hello Azure Resource Manager PowerShell-cmdlets. Se [översikt av Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.

### <a name="step-1---create-and-configure-vnet1"></a>Steg 1 – Skapa och konfigurera VNet1
#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler
I den här övningen börjar vi med att deklarera våra variabler. hello exemplet nedan förklarar hello variabler med hello värden för den här övningen. Vara säker på att tooreplace hello värden med dina egna när du konfigurerar för produktion. Du kan använda dessa variabler om du kör via hello steg toobecome bekant med den här typen av konfiguration. Ändra hello variabler, kopiera och klistra in i PowerShell-konsolen.

```powershell
$Sub1 = "Ross"
$RG1 = "TestAARG1"
$Location1 = "West US"
$VNetName1 = "TestVNet1"
$FESubName1 = "FrontEnd"
$BESubName1 = "Backend"
$GWSubName1 = "GatewaySubnet"
$VNetPrefix11 = "10.11.0.0/16"
$VNetPrefix12 = "10.12.0.0/16"
$FESubPrefix1 = "10.11.0.0/24"
$BESubPrefix1 = "10.12.0.0/24"
$GWSubPrefix1 = "10.12.255.0/27"
$VNet1ASN = 65010
$DNS1 = "8.8.8.8"
$GWName1 = "VNet1GW"
$GW1IPName1 = "VNet1GWIP1"
$GW1IPName2 = "VNet1GWIP2"
$GW1IPconf1 = "gw1ipconf1"
$GW1IPconf2 = "gw1ipconf2"
$Connection12 = "VNet1toVNet2"
$Connection151 = "VNet1toSite5_1"
$Connection152 = "VNet1toSite5_2"
```

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a>2. Anslut tooyour prenumeration och skapa en ny resursgrupp
Kontrollera att du växla tooPowerShell läge toouse hello Resource Manager-cmdletar. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

Öppna PowerShell-konsolen och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. Skapa TestVNet1
hello exemplet nedan skapar ett virtuellt nätverk med namnet TestVNet1 och tre undernät, en kallas GatewaySubnet, en klientdel som anropats och ett kallas Backend. När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet. Om du ger det något annat namn går det inte att skapa gatewayen.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Steg 2 – Skapa hello VPN-gateway för TestVNet1 med aktiv-aktiv läge
#### <a name="1-create-hello-public-ip-addresses-and-gateway-ip-configurations"></a>1. Skapa hello offentliga IP-adresser och IP-konfigurationer för gateway
Begäran två offentliga IP-adresser toobe allokerade toohello gateway skapas för ditt VNet. Du måste också definiera hello undernät och IP-konfigurationer som krävs.

```powershell
$gw1pip1 = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$gw1pip2 = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
$gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2
```

#### <a name="2-create-hello-vpn-gateway-with-active-active-configuration"></a>2. Skapa hello VPN-gateway med aktiv-aktiv konfiguration
Skapa hello virtuell nätverksgateway för TestVNet1. Observera att det finns två GatewayIpConfig poster och hello EnableActiveActiveFeature flaggan har angetts. Skapa en gateway kan ta ett tag (45 minuter eller mer toocomplete).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN -EnableActiveActiveFeature -Debug
```

#### <a name="3-obtain-hello-gateway-public-ip-addresses-and-hello-bgp-peer-ip-address"></a>3. Hämta hello gateway offentliga IP-adresser och hello BGP-Peer-IP-adress
När du har skapat hello gateway måste tooobtain hello BGP-Peer-IP-adress på hello Azure VPN-Gateway. Den här adressen är nödvändiga tooconfigure hello Azure VPN-Gateway som en BGP-Peer för dina lokala VPN-enheter.

```powershell
$gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
$gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
```

Använd hello följande cmdlets tooshow hello två offentliga IP-adresser för VPN-gateway och deras motsvarande BGP-Peer-IP-adresser för varje gateway-instans:

```powershell

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }
```

hello ordning hello offentliga IP-adresser för hello gateway-instanser och hello motsvarande BGP-Peering-adresser är hello samma. I det här exemplet hello gateway VM med en offentlig IP för 40.112.190.5 använder 10.12.255.4 som sin BGP-Peering adress och hello gateway med 138.91.156.129 använder 10.12.255.5. Den här informationen behövs när du ställer in dina lokala VPN-enheter som ansluter toohello aktiv-aktiv gateway. hello gateway framgår hello diagrammet nedan med alla adresser:

![aktiv-aktiv gateway](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Du kan använda den här gatewayen tooestablish aktiv-aktiv mellan lokala eller VNet-till-VNet-anslutningen när hello gateway har skapats. hello följande avsnitt kommer att gå igenom hello steg toocomplete hello övningen.

## <a name ="aacrossprem"></a>Del 2 – upprätta en anslutning för aktiv-aktiv mellan platser
tooestablish en anslutning mellan platser, behöver du toocreate en lokal nätverksgateway toorepresent din lokala VPN-enhet och anslutningen tooconnect hello Azure VPN-gateway med hello lokal nätverksgateway. I det här exemplet är hello Azure VPN-gateway i aktivt-aktivt läge. Även om det finns endast en lokal VPN-enheter (lokal nätverksgateway) och en anslutning resurser, kommer båda Azure VPN gateway-instanser därför upprätta S2S VPN-tunnlar med hello lokala enhet.

Innan du fortsätter, kontrollera att du har slutfört [del 1](#aagateway) för den här övningen.

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a>Steg 1 – Skapa och konfigurera hello lokal nätverksgateway
#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler
Den här övningen fortsätter toobuild hello-konfiguration visas i hello diagram. Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.

```powershell
$RG5 = "TestAARG5"
$Location5 = "West US"
$LNGName51 = "Site5_1"
$LNGPrefix51 = "10.52.255.253/32"
$LNGIP51 = "131.107.72.22"
$LNGASN5 = 65050
$BGPPeerIP51 = "10.52.255.253"
```

Ett par saker toonote om hello lokala gateway nätverksparametrar:

* hello lokal nätverksgateway kan vara i hello samma eller annan plats och grupp som hello VPN-gateway. Det här exemplet visar dem i olika resursgrupper men i hello samma Azure-plats.
* Om det finns bara en lokal VPN-enhet som du ser ovan, kan hello aktiv-aktiv anslutning arbeta med eller utan BGP-protokollet. Det här exemplet använder BGP för hello mellan lokala anslutningen.
* Om BGP är aktiverat är hello prefix som du behöver toodeclare för hello lokal nätverksgateway hello värden adressen för BGP-Peer-IP-adress på VPN-enhet. I det här fallet är det en /32 prefixet ”10.52.255.253/32”.
* Du måste använda olika BGP, ASN: er, mellan ditt lokala nätverk och Azure VNet som en påminnelse. Om de är hello samma måste toochange ditt VNet ASN om din lokala VPN-enhet redan använder hello ASN toopeer med andra BGP-grannar.

#### <a name="2-create-hello-local-network-gateway-for-site5"></a>2. Skapa hello lokal nätverksgateway för Site5
Innan du fortsätter, kontrollera att du är fortfarande ansluten tooSubscription 1. Skapa hello resursgrupp om den inte har skapats ännu.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5
New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a>Steg 2 – ansluta hello VNet gateway och lokal nätverksgateway
#### <a name="1-get-hello-two-gateways"></a>1. Hämta hello två gateways

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw1 = Get-AzureRmLocalNetworkGateway  -Name $LNGName51 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a>2. Skapa hello TestVNet1 tooSite5 anslutning
I det här steget skapar hello anslutning från TestVNet1 tooSite5_1 med ”Enablegpg” ställa in också$ True.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN och BGP parametrar för din lokala VPN-enhet
hello exemplet nedan visar hello-parametrar som du ska ange i hello BGP konfigurationsavsnittet på din lokala VPN-enhet för den här övningen:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.253
    - Prefix tooannounce: (till exempel) 10.51.0.0/16 och 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet BGP IP 1: 10.12.255.4 för tunnel too40.112.190.5
    - Azure VNet BGP IP 2: 10.12.255.5 för tunnel too138.91.156.129
    - Statiska vägar: mål 10.12.255.4/32, nexthop hello VPN-tunnel gränssnittet too40.112.190.5 mål 10.12.255.5/32, nexthop hello VPN-tunnel gränssnittet too138.91.156.129
    - eBGP Multihopp: Kontrollera hello ”Multihopp” alternativet för eBGP är aktiverat på enheten om det behövs

hello-anslutningen ska upprättas efter några minuter och hello BGP-peeringsessionen startar när hello IPsec-anslutning har upprättats. Hittills har det här exemplet konfigureras endast en lokal VPN-enhet, vilket resulterar i hello diagrammet nedan:

![aktiv-aktiv-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-toohello-active-active-vpn-gateway"></a>Steg 3 – ansluta två lokala VPN-enheter toohello aktiv-aktiv VPN-gateway
Om du har två VPN-enheter på hello samma lokala nätverk, kan du uppnå dubbla redundans genom anslutande hello Azure VPN gateway toohello andra VPN-enhet.

#### <a name="1-create-hello-second-local-network-gateway-for-site5"></a>1. Skapa hello andra lokal nätverksgateway för Site5
Observera att hello IP-adressen för gateway-adressprefix och adress för BGP-peering för hello andra lokal nätverksgateway inte får överlappa med hello tidigare lokal nätverksgateway för hello samma lokala nätverk.

```powershell
$LNGName52 = "Site5_2"
$LNGPrefix52 = "10.52.255.254/32"
$LNGIP52 = "131.107.72.23"
$BGPPeerIP52 = "10.52.255.254"

New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
```

#### <a name="2-connect-hello-vnet-gateway-and-hello-second-local-network-gateway"></a>2. Ansluta hello VNet gateway och hello andra lokal nätverksgateway
Skapa hello anslutning från TestVNet1 tooSite5_2 med ”Enablegpg” ställa in också$ True

```powershell
$lng5gw2 = Get-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True
```

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. Parametrarna för VPN och BGP för andra lokala VPN-enheten
På liknande sätt nedan visar hello parametrar kommer in hello andra VPN-enhet:

```
- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP 1  : 10.12.255.4 for tunnel too40.112.190.5
- Azure VNet BGP IP 2  : 10.12.255.5 for tunnel too138.91.156.129
- Static routes        : Destination 10.12.255.4/32, nexthop hello VPN tunnel interface too40.112.190.5
                         Destination 10.12.255.5/32, nexthop hello VPN tunnel interface too138.91.156.129
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

När hello-anslutning (tunnlar) upprättas, har du dubbla redundant VPN-enheter och ansluter dina lokala nätverk och Azure-tunnlar:

![dubbla-redundans-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)

## <a name ="aav2v"></a>Del 3: upprätta en anslutning för aktiv-aktiv VNet-till-VNet
Det här avsnittet skapar du en aktiv-aktiv VNet-till-VNet-anslutning med BGP. 

hello anvisningarna nedan fortsätta från hello stegen ovan. Du måste slutföra [del 1](#aagateway) toocreate TestVNet1 och konfigurera hello VPN-Gateway med BGP. 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a>Steg 1 – Skapa TestVNet2 och hello VPN-gateway
Det är viktigt toomake till att hello IP-adressutrymme hello nytt virtuellt nätverk, TestVNet2, inte överlappar med någon av VNet-intervall.

I det här exemplet hello virtuella nätverk som tillhör toohello samma prenumeration. Du kan konfigurera VNet-till-VNet-anslutningar mellan olika prenumerationer; Se för[konfigurera VNet-till-VNet-anslutningen](vpn-gateway-vnet-vnet-rm-ps.md) toolearn mer information. Kontrollera att du lägger till hello ”-Enablegpg $True” när skapar hello anslutningar tooenable BGP.

#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler
Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.

```powershell
$RG2 = "TestAARG2"
$Location2 = "East US"
$VNetName2 = "TestVNet2"
$FESubName2 = "FrontEnd"
$BESubName2 = "Backend"
$GWSubName2 = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$VNet2ASN = 65020
$DNS2 = "8.8.8.8"
$GWName2 = "VNet2GW"
$GW2IPName1 = "VNet2GWIP1"
$GW2IPconf1 = "gw2ipconf1"
$GW2IPName2 = "VNet2GWIP2"
$GW2IPconf2 = "gw2ipconf2"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-testvnet2-in-hello-new-resource-group"></a>2. Skapa TestVNet2 i hello ny resursgrupp

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-hello-active-active-vpn-gateway-for-testvnet2"></a>3. Skapa hello aktiv-aktiv VPN-gateway för TestVNet2
Begäran två offentliga IP-adresser toobe allokerade toohello gateway skapas för ditt VNet. Du måste också definiera hello undernät och IP-konfigurationer som krävs.

```powershell
$gw2pip1 = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$gw2pip2 = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
$gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2
```

Skapa hello VPN-gateway med hello som antal och hello ”EnableActiveActiveFeature”-flagga. Observera att du måste åsidosätta hello standard ASN på Azure VPN-gatewayer. hello ASN: er för hello anslutna Vnet måste vara olika tooenable BGP och transitroutning.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet2ASN -EnableActiveActiveFeature
```

### <a name="step-2---connect-hello-testvnet1-and-testvnet2-gateways"></a>Steg 2 – ansluta hello TestVNet1 och TestVNet2 gateways
I det här exemplet är både gateways i hello samma prenumeration. Du kan slutföra det här steget i hello samma PowerShell-session.

#### <a name="1-get-both-gateways"></a>1. Hämta båda gateways
Kontrollera att du loggar in och ansluta tooSubscription 1.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Skapa både anslutningar
I det här steget ska du skapa hello anslutning från TestVNet1 tooTestVNet2 och hello anslutning från TestVNet2 tooTestVNet1.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Vara säker på att tooenable BGP för både anslutningar.
> 
> 

När du har slutfört de här stegen hello anslutning ska upprätta några minuter och hello BGP-peeringsessionen tas upp när hello VNet-till-VNet-anslutningen har slutförts med dubbla redundans:

![aktiv-aktiv-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>En del 4 – uppdatera en befintlig gateway mellan aktiv-aktiv och aktivt vänteläge
hello sista avsnittet beskriver hur du kan konfigurera en befintlig Azure VPN-gateway i aktivt vänteläge tooactive-aktivt läge eller vice versa.

> [!NOTE]
> Det här avsnittet innehåller hello steg tooresize en äldre SKU (gamla SKU) av en VPN-gateway som redan har skapat från Standard tooHighPerformance. Dessa steg ska du inte uppgradera en gamla äldre SKU tooone av hello nya SKU: er.
> 
> 

### <a name="configure-an-active-standby-gateway-tooactive-active-gateway"></a>Konfigurera en gateway i aktivt vänteläge tooactive active-gateway
#### <a name="1-gateway-parameters"></a>1. Gateway-parametrar
följande exempel hello konverterar en gateway i aktivt vänteläge till en aktiv-aktiv gateway. Du behöver toocreate en annan offentlig IP-adress och sedan lägga till en andra Gateway IP-konfiguration. Nedan visas hello parametrar används:

```powershell
$GWName = "TestVNetAA1GW"
$VNetName = "TestVNetAA1"
$RG = "TestVPNActiveActive01"
$GWIPName2 = "gwpip2"
$GWIPconf2 = "gw1ipconf2"

$vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$location = $gw.Location
```

#### <a name="2-create-hello-public-ip-address-then-add-hello-second-gateway-ip-configuration"></a>2. Skapa hello offentliga IP-adressen och lägger till hello andra gateway IP-konfiguration

```powershell
$gwpip2 = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2
```

#### <a name="3-enable-active-active-mode-and-update-hello-gateway"></a>3. Aktivera aktiv-aktiv läge och uppdatera hello-gateway
Du måste ange hello gateway-objekt i PowerShell tootrigger hello faktiska uppdateringen. hello SKU hello virtuell nätverksgateway måste också ändras (storleksändrade) tooHighPerformance eftersom den har skapats tidigare som Standard.

```powershell
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance
```

Den här uppdateringen kan ta 30 too45 minuter.

### <a name="configure-an-active-active-gateway-tooactive-standby-gateway"></a>Konfigurera en gateway för aktiv-aktiv tooactive standby-gateway
#### <a name="1-gateway-parameters"></a>1. Gateway-parametrar
Använd hello samma parametrar som ovan, hämta hello namnet på hello IP-konfigurationen som du vill tooremove.

```powershell
$GWName = "TestVNetAA1GW"
$RG = "TestVPNActiveActive01"

$gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
$ipconfname = $gw.IpConfigurations[1].Name
```

#### <a name="2-remove-hello-gateway-ip-configuration-and-disable-hello-active-active-mode"></a>2. Ta bort hello gateway IP-konfigurationen och inaktivera hello aktiv-aktiv läge
Dessutom måste du ange hello gateway-objekt i PowerShell tootrigger hello faktiska uppdateringen.

```powershell
Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature
```

Den här uppdateringen kan ta upp too30 för 45 minuter.

## <a name="next-steps"></a>Nästa steg
När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.

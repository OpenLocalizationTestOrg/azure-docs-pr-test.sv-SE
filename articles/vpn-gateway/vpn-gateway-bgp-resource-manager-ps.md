---
title: "Konfigurera BGP på Azure VPN-gatewayer: hanteraren för filserverresurser: PowerShell | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att konfigurera BGP med Azure VPN-gatewayer med Azure Resource Manager och PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 905b11a7-1333-482c-820b-0fd0f44238e5
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: yushwang
ms.openlocfilehash: a9d13ae6b319e2efa8965dc2955c9b89ac3fd12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-bgp-on-azure-vpn-gateways-using-powershell"></a>Hur tooconfigure BGP på Azure VPN-gatewayer med hjälp av PowerShell
Den här artikeln vägleder dig genom hello steg tooenable BGP på en mellan lokala plats-till-plats (S2S) VPN-anslutning och en VNet-till-VNet-anslutning med hello Resource Manager-distributionsmodellen och PowerShell.

## <a name="about-bgp"></a>Om BGP
BGP är hello standard routingprotokoll ofta används i hello Internet tooexchange Routning och reachability information mellan två eller flera nätverk. BGP möjliggör hello Azure VPN-gatewayer och dina lokala VPN-enheter som kallas BGP-peers eller grannar, tooexchange ”dirigerar” som informerar båda gateways på hello tillgänglighet och tillgängligheten för de prefix toogo via hello gateways eller routrar som ingår. BGP kan också aktivera transitroutning mellan flera nätverk av sprider vägar som en gateway med BGP lär sig från en BGP peer tooall andra BGP-peers.

Se [översikt av BGP med Azure VPN-gatewayer](vpn-gateway-bgp-overview.md) mer beskrivning på fördelarna BGP och toounderstand hello tekniska krav och överväganden för att använda BGP.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Komma igång med BGP på Azure VPN-gatewayer

Den här artikeln vägleder dig genom hello steg toodo hello följande uppgifter:

* [Del 1 - Aktivera BGP på din Azure VPN-gateway](#enablebgp)
* [Del 2 – upprätta en anslutning mellan lokala BGP](#crossprembgp)
* [Del 3: upprätta en anslutning för VNet-till-VNet med BGP](#v2vbgp)

Varje del av hello instruktioner utgör ett grundläggande byggblock för att aktivera BGP i nätverksanslutningen. Om du har slutfört alla tre delar kan skapa hello topologi som visas i följande diagram hello:

![BGP-topologi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Du kan kombinera delar tillsammans toobuild en mer komplexa, flera hopp övergångsnätverket som uppfyller dina behov.

## <a name ="enablebgp"></a>Del 1 – konfigurera BGP på hello Azure VPN-Gateway
hello konfigurationssteg ställa in hello BGP-parametrarna för hello Azure VPN-gateway som visas i följande diagram hello:

![BGP-Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Innan du börjar
* Kontrollera att du har en Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).
* Installera hello Azure Resource Manager PowerShell-cmdlets. Mer information om hur du installerar hello PowerShell-cmdlets finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview). 

### <a name="step-1---create-and-configure-vnet1"></a>Steg 1 – Skapa och konfigurera VNet1
#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler
Den här övningen starta genom att deklarera våra variabler. hello förklarar följande exempel hello variabler med hello värden för den här övningen. Vara säker på att tooreplace hello värden med dina egna när du konfigurerar för produktion. Du kan använda dessa variabler om du kör via hello steg toobecome bekant med den här typen av konfiguration. Ändra hello variabler, kopiera och klistra in i PowerShell-konsolen.

```powershell
$Sub1 = "Replace_With_Your_Subcription_Name"
$RG1 = "TestBGPRG1"
$Location1 = "East US"
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
$GWIPName1 = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection12 = "VNet1toVNet2"
$Connection15 = "VNet1toSite5"
```

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a>2. Anslut tooyour prenumeration och skapa en ny resursgrupp
toouse hello Resource Manager cmdlets, se till att du växla tooPowerShell läge. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

Öppna PowerShell-konsolen och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. Skapa TestVNet1
hello skapar följande exempel ett virtuellt nätverk med namnet TestVNet1 och tre undernät, en kallas GatewaySubnet, en klientdel som anropats och ett kallas Backend. När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet. Om du ger det något annat namn går det inte att skapa gatewayen.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-hello-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Steg 2 – Skapa hello VPN-Gateway för TestVNet1 med BGP-parametrar
#### <a name="1-create-hello-ip-and-subnet-configurations"></a>1. Skapa hello IP-adress och nätmask
Begär en offentlig IP-adress toobe allokerade toohello gateway skapas för ditt VNet. Du måste också definiera hello krävs undernät och IP-konfigurationer.

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-hello-vpn-gateway-with-hello-as-number"></a>2. Skapa hello VPN-gateway med hello som tal
Skapa hello virtuell nätverksgateway för TestVNet1. BGP kräver en Ruttbaserad VPN-gateway och hello tillägg parametern, - Asn tooset hello ASN (AS Number) för TestVNet1. Om du inte anger hello ASN parametern tilldelas ASN 65515. Skapa en gateway kan ta ett tag (30 minuter eller mer toocomplete).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-hello-azure-bgp-peer-ip-address"></a>3. Hämta hello Azure BGP peer-IP-adress
När hello gateway har skapats måste tooobtain hello BGP-Peer-IP-adress på hello Azure VPN-Gateway. Den här adressen är nödvändiga tooconfigure hello Azure VPN-Gateway som en BGP-Peer för dina lokala VPN-enheter.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

hello sista kommandot visar hello motsvarande BGP konfigurationer på hello Azure VPN-Gateway. Exempel:

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

När hello gateway har skapats kan använda du denna gateway tooestablish mellan platser eller VNet-till-VNet-anslutning med BGP. hello igenom avsnitten hello steg toocomplete hello övningen.

## <a name ="crossprembbgp"></a>Del 2 – upprätta en anslutning mellan lokala BGP

tooestablish en anslutning mellan platser, behöver du toocreate en lokal nätverksgateway toorepresent din lokala VPN-enhet och en anslutning tooconnect hello VPN-gateway med hello lokal nätverksgateway. Det finns artiklar som vägleder dig genom stegen, innehåller den här artikeln hello ytterligare egenskaper krävs toospecify hello BGP konfigurationsparametrar.

![BGP för mellan platser](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Innan du fortsätter bör du kontrollera att du har slutfört [del 1](#enablebgp) för den här övningen.

### <a name="step-1---create-and-configure-hello-local-network-gateway"></a>Steg 1 – Skapa och konfigurera hello lokal nätverksgateway

#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler

Den här övningen fortsätter toobuild hello-konfiguration visas i hello diagram. Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

Ett par saker toonote om hello lokala gateway nätverksparametrar:

* hello lokal nätverksgateway kan vara i hello samma eller annan plats och grupp som hello VPN-gateway. Det här exemplet visar dem i olika resursgrupper på olika platser.
* hello minsta prefix som du behöver toodeclare för hello lokal nätverksgateway är hello värden adressen för BGP-Peer-IP-adress på VPN-enhet. I det här fallet är det en /32 prefixet ”10.52.255.254/32”.
* Du måste använda olika BGP, ASN: er, mellan ditt lokala nätverk och Azure VNet som en påminnelse. Om de är hello samma måste toochange ditt VNet ASN om din lokala VPN-enhet redan använder hello ASN toopeer med andra BGP-grannar.

Innan du fortsätter kan du kontrollera att du är fortfarande ansluten tooSubscription 1.

#### <a name="2-create-hello-local-network-gateway-for-site5"></a>2. Skapa hello lokal nätverksgateway för Site5

Vara att toocreate hello resursgrupp om den inte har skapats innan du skapar hello lokal nätverksgateway. Lägg märke till hello två ytterligare parametrar för hello lokal nätverksgateway: Asn och BgpPeerAddress.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-hello-vnet-gateway-and-local-network-gateway"></a>Steg 2 – ansluta hello VNet gateway och lokal nätverksgateway

#### <a name="1-get-hello-two-gateways"></a>1. Hämta hello två gateways

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-hello-testvnet1-toosite5-connection"></a>2. Skapa hello TestVNet1 tooSite5 anslutning

I det här steget skapar du hello anslutning från TestVNet1 tooSite5. Du måste ange ”-Enablegpg $True” tooenable BGP för den här anslutningen. Som tidigare diskuterats, det är möjligt toohave både BGP och icke-BGP-anslutningarna för hello samma Azure VPN-Gateway. Om BGP är aktiverat i Anslutningsegenskapen hello kommer Azure inte aktivera BGP för den här anslutningen även om BGP parametrar har redan konfigurerats på båda gateways.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

hello visas följande exempel hello-parametrar som du anger i hello BGP konfigurationsavsnittet på din lokala VPN-enhet för den här övningen:

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes tooannounce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being hello VPN tunnel interface on your device
- eBGP Multihop        : Ensure hello "multihop" option for eBGP is enabled on your device if needed
```

hello-anslutningen har upprättats efter några minuter och hello BGP peering-sessionen startar när hello IPsec-anslutning har upprättats.

## <a name ="v2vbgp"></a>Del 3: upprätta en anslutning för VNet-till-VNet med BGP

Det här avsnittet lägger till en VNet-till-VNet-anslutning med BGP, som visas i följande diagram hello:

![BGP för VNet-till-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

hello följa anvisningar fortsätta från hello föregående steg. Du måste slutföra [del I](#enablebgp) toocreate TestVNet1 och konfigurera hello VPN-Gateway med BGP. 

### <a name="step-1---create-testvnet2-and-hello-vpn-gateway"></a>Steg 1 – Skapa TestVNet2 och hello VPN-gateway

Det är viktigt toomake till att hello IP-adressutrymme hello nytt virtuellt nätverk, TestVNet2, inte överlappar med någon av VNet-intervall.

I det här exemplet hello virtuella nätverk som tillhör toohello samma prenumeration. Du kan konfigurera VNet-till-VNet-anslutningar mellan olika prenumerationer. Mer information finns i [konfigurera VNet-till-VNet-anslutningen](vpn-gateway-vnet-vnet-rm-ps.md). Kontrollera att du lägger till hello ”-Enablegpg $True” när skapar hello anslutningar tooenable BGP.

#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler

Vara säker på att tooreplace hello värden med hello som du vill använda toouse för din konfiguration.

```powershell
$RG2 = "TestBGPRG2"
$Location2 = "West US"
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
$GWIPName2 = "VNet2GWIP"
$GWIPconfName2 = "gwipconf2"
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

#### <a name="3-create-hello-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. Skapa hello VPN-gateway för TestVNet2 med BGP-parametrar

Begär en offentlig IP-adress toobe allokerade toohello gateway du skapar för din VNet och definiera hello krävs undernät och IP-konfigurationer.

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

Skapa hello VPN-gateway med hello som tal. Du måste åsidosätta hello standard ASN på Azure VPN-gatewayer. hello ASN: er för hello anslutna Vnet måste vara olika tooenable BGP och transitroutning.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
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

I det här steget skapar du hello anslutning från TestVNet1 tooTestVNet2 och hello anslutning från TestVNet2 tooTestVNet1.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Vara säker på att tooenable BGP för både anslutningar.
> 
> 

När du har slutfört de här stegen ansluta hello efter några minuter. hello BGP-peeringsessionen är upp när hello VNet-till-VNet-anslutningen har slutförts.

Om du har slutfört alla tre delar av den här övningen har du etablerat hello efter nätverkets topologi:

![BGP för VNet-till-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Nästa steg

När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.

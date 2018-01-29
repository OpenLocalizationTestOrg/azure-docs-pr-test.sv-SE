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
ms.openlocfilehash: b00a3fe7ba4b12c2e9c486188c292cd6fafb60a3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-powershell"></a>Hur du konfigurerar BGP på Azure VPN-gatewayer med hjälp av PowerShell
Den här artikeln vägleder dig genom stegen för att aktivera BGP på en mellan lokala plats-till-plats (S2S) VPN-anslutning och en VNet-till-VNet-anslutning med Resource Manager-distributionsmodellen och PowerShell.

## <a name="about-bgp"></a>Om BGP
BGP är ett standardroutningsprotokoll som vanligen används på Internet för att utbyta information om routning och åtkomst mellan två eller flera nätverk. BGP kan Azure VPN-gatewayer och din lokala VPN-enheter som kallas BGP-peers eller grannar, för att utbyta ”dirigerar” som ger information om båda gateways på tillgängligheten och tillgängligheten för de prefix som ska gå igenom gateways eller routrar som ingår. BGP kan också möjliggöra överföringsroutning mellan flera nätverk genom att sprida vägar som BGP-gatewayen får information om från en BGP-peer till alla andra BGP-peers.

Se [översikt av BGP med Azure VPN-gatewayer](vpn-gateway-bgp-overview.md) för flera diskussion om fördelarna med BGP och att förstå de tekniska krav och överväganden för att använda BGP.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Komma igång med BGP på Azure VPN-gatewayer

Den här artikeln vägleder dig genom stegen för att göra följande:

* [Del 1 - Aktivera BGP på din Azure VPN-gateway](#enablebgp)
* [Del 2 – upprätta en anslutning mellan lokala BGP](#crossprembgp)
* [Del 3: upprätta en anslutning för VNet-till-VNet med BGP](#v2vbgp)

Varje del av instruktionerna utgör ett grundläggande byggblock för att aktivera BGP i nätverksanslutningen. Om du har slutfört alla tre delar kan skapa topologin som visas i följande diagram:

![BGP-topologi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Du kan kombinera delar tillsammans för att skapa en mer komplexa, flera hopp övergångsnätverket som uppfyller dina behov.

## <a name ="enablebgp"></a>Del 1 – konfigurera BGP på Azure VPN-Gateway
Konfigurationsstegen ställa in Azure VPN-gateway BGP-parametrar som visas i följande diagram:

![BGP-Gateway](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Innan du börjar
* Kontrollera att du har en Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).
* Installera Azure Resource Manager PowerShell-cmdlets. Mer information om hur du installerar PowerShell-cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview). 

### <a name="step-1---create-and-configure-vnet1"></a>Steg 1 – Skapa och konfigurera VNet1
#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler
Den här övningen starta genom att deklarera våra variabler. I följande exempel deklarerar variabler med hjälp av värdena för den här övningen. Se till att ersätta värdena med dina egna när du konfigurerar för produktion. Du kan använda dessa variabler om du använder anvisningarna för att bekanta dig med den här typen av konfiguration. Ändra variablerna samt kopiera och klistra in dem i PowerShell-konsolen.

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

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. Anslut till din prenumeration och skapa en ny resursgrupp
Kontrollera att du växlar till PowerShell-läge för att använda Resource Manager-cmdletar. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

Öppna PowerShell-konsolen och anslut till ditt konto. Använd följande exempel för att ansluta:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-testvnet1"></a>3. Skapa TestVNet1
I följande exempel skapar ett virtuellt nätverk med namnet TestVNet1 och tre undernät, en kallas GatewaySubnet, en klientdel som anropats och ett kallas Backend. När du ersätter värden är det viktigt att du alltid namnger gateway-undernätet specifikt till GatewaySubnet. Om du ger det något annat namn går det inte att skapa gatewayen.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1
```

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Steg 2 – skapa VPN-Gateway för TestVNet1 med BGP-parametrar
#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. Skapa IP-adress och nätmask konfigurationer
Begär en offentlig IP-adress som ska allokeras till den gateway som du ska skapa för det virtuella nätverket. Du måste också definiera krävs undernätet och IP-konfigurationer.

```powershell
$gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

$vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1
```

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. Skapa VPN-gateway med AS-nummer
Skapa den virtuella nätverksgatewayen för TestVNet1. BGP kräver en Ruttbaserad VPN-gateway och parametern tillägg, Asn - ange ASN (AS Number) för TestVNet1. Om du inte anger parametern ASN tilldelas ASN 65515. Att skapa en gateway kan ta ett tag (30 minuter eller mer att slutföra).

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN
```

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Hämta Azure BGP-Peer-IP-adress
När gatewayen är skapad, måste du skaffa BGP-Peer-IP-adressen på Azure VPN-Gateway. Den här adressen behövs för att konfigurera Azure VPN-Gateway som en BGP-Peer för dina lokala VPN-enheter.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet1gw.BgpSettingsText
```

Det sista kommandot visas motsvarande BGP konfigurationer på Azure VPN-Gateway. Exempel:

```powershell
$vnet1gw.BgpSettingsText
{
    "Asn": 65010,
    "BgpPeeringAddress": "10.12.255.30",
    "PeerWeight": 0
}
```

Du kan använda denna gateway för att upprätta anslutningar mellan lokala eller VNet-till-VNet-anslutning med BGP när gatewayen har skapats. I följande avsnitt gå igenom stegen för att slutföra den här övningen.

## <a name ="crossprembbgp"></a>Del 2 – upprätta en anslutning mellan lokala BGP

För att upprätta en anslutning mellan platser, måste du skapa en lokal nätverksgateway som representerar din lokala VPN-enhet och en anslutning till ansluta VPN-gateway med den lokala nätverksgatewayen. Det finns artiklar som vägleder dig genom stegen, innehåller den här artikeln ytterligare egenskaper som krävs för att ange parametrar för BGP-konfiguration.

![BGP för mellan platser](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Innan du fortsätter bör du kontrollera att du har slutfört [del 1](#enablebgp) för den här övningen.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Steg 1 – Skapa och konfigurera den lokala nätverksgatewayen

#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler

Den här övningen fortsätter att skapa den konfiguration som visas i diagrammet. Ersätt värdena med de som du vill använda för din konfiguration.

```powershell
$RG5 = "TestBGPRG5"
$Location5 = "East US 2"
$LNGName5 = "Site5"
$LNGPrefix50 = "10.52.255.254/32"
$LNGIP5 = "Your_VPN_Device_IP"
$LNGASN5 = 65050
$BGPPeerIP5 = "10.52.255.254"
```

Några saker att Observera för de lokala nätverket gateway-parametrarna:

* Den lokala nätverksgatewayen kan vara i samma eller en annan plats och resursgruppen som VPN-gateway. Det här exemplet visar dem i olika resursgrupper på olika platser.
* Minsta prefixet måste du deklarera för den lokala nätverksgatewayen är värdadressen för BGP-Peer-IP-adress på VPN-enhet. I det här fallet är det en /32 prefixet ”10.52.255.254/32”.
* Du måste använda olika BGP, ASN: er, mellan ditt lokala nätverk och Azure VNet som en påminnelse. Om de är samma som du behöver ändra ditt VNet ASN om din lokala VPN-enhet använder redan ASN till peer-datorn med andra BGP-grannar.

Innan du fortsätter kontrollerar du att du fortfarande är ansluten till Prenumeration 1.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Skapa lokal nätverksgateway för Site5

Se till att skapa resursgruppen om det inte har skapats innan du skapar den lokala nätverksgatewayen. Lägg märke till de två andra parametrarna för den lokala nätverksgatewayen: Asn och BgpPeerAddress.

```powershell
New-AzureRmResourceGroup -Name $RG5 -Location $Location5

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Steg 2 – ansluta VNet gateway och lokal nätverksgateway

#### <a name="1-get-the-two-gateways"></a>1. Hämta två gateways

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5
```

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Skapa TestVNet1 till Site5 anslutning

I det här steget skapar du anslutningen från TestVNet1 till Site5. Du måste ange ”-Enablegpg $True” att aktivera BGP för den här anslutningen. Som tidigare diskuterats, är det möjligt att ha både BGP och icke-BGP-anslutningar för samma Azure VPN-Gateway. Om BGP är aktiverat i Anslutningsegenskapen ska Azure inte aktivera BGP för den här anslutningen även om BGP parametrar har redan konfigurerats på båda gateways.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

I följande exempel visas de parametrar som du anger i konfigurationsavsnittet BGP på din lokala VPN-enhet för den här övningen:

```

- Site5 ASN            : 65050
- Site5 BGP IP         : 10.52.255.254
- Prefixes to announce : (for example) 10.51.0.0/16 and 10.52.0.0/16
- Azure VNet ASN       : 65010
- Azure VNet BGP IP    : 10.12.255.30
- Static route         : Add a route for 10.12.255.30/32, with nexthop being the VPN tunnel interface on your device
- eBGP Multihop        : Ensure the "multihop" option for eBGP is enabled on your device if needed
```

BGP-peeringsessionen startas en gång IPsec anslutningen upprättas anslutningen upprättas efter några minuter.

## <a name ="v2vbgp"></a>Del 3: upprätta en anslutning för VNet-till-VNet med BGP

Det här avsnittet lägger till en VNet-till-VNet-anslutning med BGP, som visas i följande diagram:

![BGP för VNet-till-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Följande instruktioner fortsätta från föregående steg. Du måste slutföra [del I](#enablebgp) att skapa och konfigurera TestVNet1 och VPN-Gateway med BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Steg 1 – Skapa TestVNet2 och VPN-gateway

Det är viktigt att se till att IP-adressutrymmet för det nya virtuella nätverket TestVNet2, inte överlappar med någon av VNet-intervall.

I det här exemplet tillhör de virtuella nätverken samma prenumeration. Du kan konfigurera VNet-till-VNet-anslutningar mellan olika prenumerationer. Mer information finns i [konfigurera VNet-till-VNet-anslutningen](vpn-gateway-vnet-vnet-rm-ps.md). Kontrollera att du lägger till den ”-Enablegpg $True” när du skapar anslutningar för att aktivera BGP.

#### <a name="1-declare-your-variables"></a>1. Deklarera dina variabler

Ersätt värdena med de som du vill använda för din konfiguration.

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

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Skapa TestVNet2 i den nya resursgruppen

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2
```

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. Skapa VPN-gateway för TestVNet2 med BGP-parametrar

Begär offentlig IP-adress som ska allokeras till den gateway som du skapar för din VNet och definiera krävs undernätet och IP-konfigurationer.

```powershell
$gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

$vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2
```

Skapa VPN-gateway med AS-nummer. Du måste åsidosätta standardvärdet ASN på Azure VPN-gatewayer. ASN: er för det anslutna Vnet måste vara olika för att aktivera BGP och transitroutning.

```powershell
New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN
```

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Steg 2 – ansluta TestVNet1 och TestVNet2 gateway

I det här exemplet är både gateways i samma prenumeration. Du kan slutföra det här steget i samma PowerShell-sessionen.

#### <a name="1-get-both-gateways"></a>1. Hämta båda gateways

Kontrollera att du har loggat in och anslutit till Prenumeration 1.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
```

#### <a name="2-create-both-connections"></a>2. Skapa både anslutningar

I det här steget skapar du anslutningen från TestVNet1 till TestVNet2 och anslutningen från TestVNet2 till TestVNet1.

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True
```

> [!IMPORTANT]
> Glöm inte att aktivera BGP för både anslutningar.
> 
> 

När du har slutfört de här stegen, upprättas anslutningen efter några minuter. BGP-peeringsessionen är upp när VNet-till-VNet-anslutningen har slutförts.

Om du har slutfört alla tre delar av den här övningen har du skapat följande nätverkets topologi:

![BGP för VNet-till-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Nästa steg

När anslutningen är klar kan du lägga till virtuella datorer till dina virtuella nätverk. Se [Skapa en virtuell dator](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) för anvisningar.
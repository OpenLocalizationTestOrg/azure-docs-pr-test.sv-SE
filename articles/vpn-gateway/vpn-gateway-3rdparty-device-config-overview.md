---
title: aaaAbout 3 part VPN-enhetens konfiguration tooconnect tooAzure VPN-gatewayer | Microsoft Docs
description: "Den här artikeln innehåller en översikt över 3 part VPN enhetskonfigurationer för att ansluta tooAzure VPN-gatewayer."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 3bb4fc94bc625386c2d0a02e1dcbdeb38ee0665e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-3rd-party-vpn-device-configurations"></a>Översikt över 3 part VPN enhetskonfigurationer
Den här artikeln innehåller en översikt över lokala VPN-enhetskonfigurationer för att ansluta tooAzure VPN-gatewayer. hello exemplet Azure-nätverk och VPN-gateway installationen kommer att använda tooconnect toodifferent lokala VPN-enheter med hello samma parametrar.

## <a name="device-requirements"></a>Enhetskrav på
Azure VPN-gatewayer Använd standard IPsec/IKE-protokoll-paket för S2S VPN-tunnlar. Se för[om VPN-enheter](vpn-gateway-about-vpn-devices.md) hello mer IPsec/IKE protokollparametrar och standard kryptografiska algoritmer för Azure VPN-gatewayer. Du kan också ange hello exakta kombinationen av kryptografiska algoritmer och viktiga fördelar för en viss anslutning enligt beskrivningen i [om krav för kryptografiska](vpn-gateway-about-compliance-crypto.md).

## <a name ="singletunnel"></a>En VPN-tunnel
hello första topologi består av en S2S VPN-tunnel mellan en Azure VPN-gateway och din lokala VPN-enhet. Du kan också konfigurera BGP över hello VPN-tunnel.

![en tunnel](./media/vpn-gateway-3rdparty-device-config-overview/singletunnel.png)

Se för[Konfigurera plats-till-plats-anslutning](vpn-gateway-howto-site-to-site-resource-manager-portal.md) detaljerad, stegvisa anvisningar. hello följande avsnitt listas hello parametrar och ge exempel PowerShell-skriptet toohelp dig att komma igång.

### <a name="network-and-vpn-gateway-information"></a>Information om nätverk och VPN-gateway
Det här avsnittet listas hello parametrar för hello exemplen ovan.

| **Parametern**                | **Värde**                    |
| ---                          | ---                          |
| VNet-adressprefixer        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN gateway-IP         | Azure VPN-Gateway IP         |
| Lokala adressprefix | 10.51.0.0/16<br>10.52.0.0/16 |
| Lokala VPN-enhetens IP    | Lokala VPN-enhetens IP    |
| * VNet BGP ASN                | 65010                        |
| * Azure BGP-peer-IP           | 10.12.255.30                 |
| * Lokala BGP ASN         | 65050                        |
| * Lokala BGP-peer-IP     | 10.52.255.254                |

* (*) Valfria parametrar för BGP endast

### <a name="sample-powershell-script"></a>PowerShell-exempelskript
[Skapa en S2S VPN-anslutning med hjälp av PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md) har hello detaljerade instruktioner. Det här avsnittet innehåller ett exempel skriptet tooget som du startade.

```powershell
# Declare your variables

$Sub1          = "Replace_With_Your_Subcription_Name"
$RG1           = "TestRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$VNet1ASN      = 65010
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GWIPName1     = "VNet1GWIP"
$GWIPconfName1 = "gwipconf1"
$Connection15  = "VNet1toSite5"
$LNGName5      = "Site5"
$LNGPrefix50   = "10.52.255.254/32"
$LNGPrefix51   = "10.51.0.0/16"
$LNGPrefix52   = "10.52.0.0/16"
$LNGIP5        = "Your_VPN_Device_IP"
$LNGASN5       = 65050
$BGPPeerIP5    = "10.52.255.254"

# Connect tooyour subscription and create a new resource group

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1

# Create virtual network

$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1 $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

# Create VPN gateway

$gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -Asn $VNet1ASN

# Create local network gateway

New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix51,$LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

# Create hello S2S VPN connection

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False
```

### <a name ="policybased"></a>[Valfritt] Använda anpassade IPsec/IKE-princip med ”UsePolicyBasedTrafficSelectors”
Om VPN-enheter inte stöder ”alla-till-alla” trafik väljare (route-baserade/VTI-baserad konfiguration), kommer du behöver toocreate en anpassad princip för IPsec/IKE och konfigurera alternativet ”UsePolicyBasedTrafficSelectors”, enligt beskrivningen i [i den här artikeln ](vpn-gateway-connect-multiple-policybased-rm-ps.md).

> [!IMPORTANT]
> Du måste toocreate en IPsec/IKE-princip i ordning tooenable ”UsePolicyBasedTrafficSelectors” alternativet på hello-anslutning.

hello exempelskript nedan skapar en IPsec/IKE-princip med hello följande algoritmer och parametrar:
* IKEv2: AES256, SHA384 DHGroup24
* IPsec: AES256, SHA1, PFS24 SA livstid 7200 sekunder & 20480000KB (20GB)

Det gäller hello princip och aktiverar ”UesPolicyBasedTrafficSelectors” på hello-anslutning.

```powershell
$ipsecpolicy5 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA1 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 20480000

$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $False -IpsecPolicies $ipsecpolicy5 -UsePolicyBasedTrafficSelectors $True
```

### <a name ="bgp"></a>[Valfritt] Använda BGP för S2S VPN-anslutning
Du kan eventuellt använda BGP på hello-anslutning. Se [BGP för VPN-gateway](vpn-gateway-bgp-resource-manager-ps.md). Det finns två skillnader:

hello lokala adressprefix kan vara en enskild värdadress, hello lokala BGP peer-IP-adress:

```powershell
New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5
```

Du måste ange ”-Enablegpg” för$ True när du skapar hello anslutning:

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True
```

## <a name="next-steps"></a>Nästa steg
Se [konfigurera aktiv-aktiv VPN-gatewayer för anslutningar mellan platser och VNet-till-VNet-anslutningar](vpn-gateway-activeactive-rm-powershell.md) åtgärder tooconfigure aktiv-aktiv mellan platser och VNet-till-VNet-anslutningar.

